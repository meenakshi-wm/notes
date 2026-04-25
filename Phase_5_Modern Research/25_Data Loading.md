📘 DAY 25 — Data Loading & Preprocessing at Scale

Goal:

Master high-performance data pipelines for ML training — avoid the data loading bottleneck that wastes GPU cycles. Implement efficient pipelines for text, images, and multi-modal data.

---

## 🌟 Beginner Analogy: The Restaurant Kitchen

> Imagine a **restaurant** 🍽️. The **GPU is the chef** — fast, expensive, and should always be cooking. The **DataLoader is the conveyor belt** 🏭 that brings ingredients (data) to the chef. If the conveyor belt is slow, the chef stands around doing nothing — and you're paying for idle time!
>
> - **`num_workers`** = **prep cooks** 👨‍🍳👩‍🍳 chopping ingredients in parallel. More cooks = faster prep (up to a point).
> - **Prefetching** = **laying out tomorrow's clothes tonight** 👔 — preparing the next batch while the current one is being used.
> - **`pin_memory`** = an **express lane** 🚀 from the kitchen prep area (CPU) directly to the stove (GPU) — no waiting in line.
> - **`collate_fn`** = **sorting mail into organized bundles** 📬 — taking individual samples and packaging them neatly into a batch.

⭐ **Key Insight**: Data loading is often the **#1 bottleneck** in training. A $10,000 GPU sitting idle because of slow data loading is like hiring a world-class chef and making them wait for groceries.

---

# 1️⃣ The Data Loading Bottleneck

If data loading takes longer than a training step, the GPU sits idle.

### 📐 Throughput Math

DataLoader **throughput** measures how fast batches arrive:

$$T = \frac{B}{t_{\text{batch}}}$$

Where $T$ = throughput (samples/sec), $B$ = batch size, $t_{\text{batch}}$ = time to load one batch.

⭐ **Goal**: Ensure $t_{\text{load}} < t_{\text{train}}$ so the GPU never waits.

With **prefetching**, loading and training overlap:

$$t_{\text{effective}} = \max(t_{\text{load}}, t_{\text{train}})$$

Instead of the naive sequential time $t_{\text{load}} + t_{\text{train}}$ per step. This is why pipelining can nearly **double** your training speed! 🚀

> 🏭 **Analogy**: Think of a **conveyor belt in a factory**. Without pipelining, the belt stops every time a worker picks up an item. With pipelining, the belt keeps moving — new items arrive just as old ones are picked up.

```
Naive pipeline:
  [Load batch] → [Train step] → [Load batch] → [Train step]
  GPU idle!       GPU busy        GPU idle!       GPU busy

Pipelined:
  GPU: [Wait] [Train step 1] [Train step 2] [Train step 3] ...
  CPU: [Load 1] [Load 2]     [Load 3]       [Load 4]      ...
       ↑ Overlap loading and training
```

---

# 2️⃣ PyTorch DataLoader Optimization

```python
from torch.utils.data import DataLoader

# BAD: single-threaded, unpinned
loader_bad = DataLoader(dataset, batch_size=32)

# GOOD: multiprocessing, pinned, prefetched
loader_good = DataLoader(
    dataset,
    batch_size=32,
    num_workers=8,           # Parallel data loading processes
    pin_memory=True,         # Pinned memory for faster GPU transfer
    prefetch_factor=2,       # Prefetch 2 batches per worker
    persistent_workers=True, # Don't restart worker processes each epoch
    drop_last=True,          # Consistent batch sizes for performance
)
```

### How Many Workers?

> 👨‍🍳 **Analogy**: `num_workers` is like **prep cooks** in a kitchen. Too few and the chef waits; too many and they bump into each other (context-switching overhead + RAM usage).

⭐ **Rule of thumb**:

$$W_{\text{optimal}} \approx 4 \times N_{\text{GPUs}}$$

But always **benchmark** — the optimal value depends on your CPU cores, I/O speed, and dataset complexity.

| GPUs | Recommended `num_workers` | Notes |
|------|--------------------------|-------|
| 1 | 4–8 | Start with 4, test up |
| 4 | 12–16 | Scale with GPUs |
| 8 | 16–32 | Watch RAM usage |

```python
# Rule of thumb: num_workers = 4 × num_GPUs
# But benchmark to find optimal:
import time

for num_workers in [0, 2, 4, 8, 12, 16]:
    loader = DataLoader(dataset, batch_size=64, num_workers=num_workers, pin_memory=True)
    start = time.perf_counter()
    for i, batch in enumerate(loader):
        if i >= 50:
            break
        _ = batch[0].cuda(non_blocking=True)
    elapsed = time.perf_counter() - start
    print(f"workers={num_workers}: {elapsed:.2f}s for 50 batches")
```

---

# 3️⃣ Memory-Mapped Datasets for Large Text

> 📖 **Analogy**: Memory mapping is like using a **bookmark in a giant library book** 📚. Instead of photocopying the entire book into your notebook (RAM), you just point to the page you need and read it directly. The OS handles fetching pages from disk transparently.

For datasets too large for RAM, use memory mapping:

```python
import numpy as np
import os

class MemmapDataset:
    """Memory-mapped dataset for tokenized text. O(1) RAM regardless of size."""
    
    def __init__(self, data_path, seq_len=2048, dtype=np.uint16):
        self.data = np.memmap(data_path, dtype=dtype, mode='r')
        self.seq_len = seq_len
    
    def __len__(self):
        return len(self.data) // self.seq_len - 1
    
    def __getitem__(self, idx):
        start = idx * self.seq_len
        chunk = self.data[start:start + self.seq_len + 1].astype(np.int64)
        x = torch.from_numpy(chunk[:-1])
        y = torch.from_numpy(chunk[1:])
        return x, y

# Create memmap file from tokenized data
def create_memmap(token_ids, output_path):
    arr = np.array(token_ids, dtype=np.uint16)
    mm = np.memmap(output_path, dtype=np.uint16, mode='w+', shape=arr.shape)
    mm[:] = arr[:]
    mm.flush()
    print(f"Created {output_path}: {os.path.getsize(output_path)/1e9:.2f} GB")
```

---

# 4️⃣ Streaming Datasets (HuggingFace)

> 🌊 **Analogy**: Streaming is like watching **Netflix** instead of downloading the entire movie first. You start using data immediately while the rest arrives in the background.

⭐ **Production Tip**: For datasets >1TB, streaming is essential. Mosaic's **StreamingDataset** (Mosaicml, 2023) and HuggingFace streaming both support deterministic resumption — critical for long training runs that may crash.

For datasets too large to download fully:

```python
from datasets import load_dataset

# Stream from HuggingFace Hub — no full download
dataset = load_dataset("cerebras/SlimPajama-627B", split="train", streaming=True)

def tokenize_and_chunk(examples, tokenizer, seq_len=2048):
    tokens = tokenizer(examples['text'], truncation=False)
    concatenated = sum(tokens['input_ids'], [])
    chunks = [concatenated[i:i+seq_len] for i in range(0, len(concatenated)-seq_len, seq_len)]
    return {'input_ids': chunks}

# Process in streaming fashion
tokenized = dataset.map(tokenize_and_chunk, batched=True, remove_columns=dataset.column_names)
loader = DataLoader(tokenized, batch_size=32)
```

---

# 5️⃣ Image Data Pipeline

> 🖼️ **Production Note**: For large-scale image training, **FFCV** (Leclerc et al., 2022) demonstrated **3.5× faster ImageNet training** by eliminating Python overhead in the data pipeline. **NVIDIA DALI** offloads preprocessing to GPU, freeing CPU for data loading.

```python
from torchvision import transforms
from torch.utils.data import Dataset
from PIL import Image
import io

class EfficientImageDataset(Dataset):
    """Optimized image dataset with on-the-fly augmentation."""
    
    def __init__(self, image_paths, labels, size=256):
        self.paths = image_paths
        self.labels = labels
        self.transform = transforms.Compose([
            transforms.RandomResizedCrop(size, scale=(0.8, 1.0)),
            transforms.RandomHorizontalFlip(),
            transforms.ColorJitter(0.2, 0.2, 0.2, 0.1),
            transforms.ToTensor(),
            transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225]),
        ])
    
    def __getitem__(self, idx):
        img = Image.open(self.paths[idx]).convert('RGB')
        img = self.transform(img)
        return img, self.labels[idx]

# WebDataset for large-scale image datasets (tar-based)
import webdataset as wds

dataset = wds.WebDataset("dataset-{000..099}.tar")  \
    .shuffle(1000)                                    \
    .decode("pil")                                    \
    .to_tuple("jpg", "json")                          \
    .map_tuple(transform, lambda x: x['label'])

loader = DataLoader(dataset, batch_size=64, num_workers=8)
```

---

# 6️⃣ Data Mixing for LLM Training

> 🎨 **Analogy**: Data mixing is like a **DJ mixing tracks** 🎧. You want the right balance of genres (web, code, books) to create the perfect set (model). Too much of one source and the model becomes one-dimensional.

```python
class MixedDataset:
    """Mix multiple data sources with specified proportions."""
    
    def __init__(self, datasets, weights):
        self.datasets = datasets
        self.weights = np.array(weights) / sum(weights)
        self.cumulative = np.cumsum(self.weights)
    
    def __iter__(self):
        iterators = [iter(d) for d in self.datasets]
        while True:
            r = np.random.random()
            idx = np.searchsorted(self.cumulative, r)
            try:
                yield next(iterators[idx])
            except StopIteration:
                iterators[idx] = iter(self.datasets[idx])
                yield next(iterators[idx])

# Llama-style data mix
mixed = MixedDataset(
    datasets=[web_data, code_data, wiki_data, books_data],
    weights=[0.67, 0.17, 0.06, 0.10]  # Proportions from paper
)
```

---

# 7️⃣ Bottleneck Detection 🔍

Profile your data pipeline **separately** from training to find bottlenecks:

```python
import time

def profile_dataloader(loader, n_batches=100):
    """Measure pure data loading time (no training)."""
    times = []
    for i, batch in enumerate(loader):
        if i == 0:
            start = time.perf_counter()  # Skip first batch (warmup)
            continue
        if i >= n_batches:
            break
        times.append(time.perf_counter() - start)
        start = time.perf_counter()
    avg_ms = 1000 * sum(times) / len(times)
    print(f"Avg batch load time: {avg_ms:.1f} ms")
    return avg_ms
```

⭐ **Bottleneck Decision Rule**:

$$\text{If } t_{\text{load}} > t_{\text{train}} \Rightarrow \text{Data bottleneck — optimize loading}$$
$$\text{If } t_{\text{train}} > t_{\text{load}} \Rightarrow \text{Compute bottleneck — optimize model/hardware}$$

---

# 8️⃣ Performance Checklist

| Check | Target |
|-------|--------|
| GPU utilization during data loading | >95% 🟢 |
| num_workers | $4 \times N_{\text{GPUs}}$ (benchmark) |
| pin_memory | True |
| prefetch_factor | 2-4 |
| Data on SSD, not HDD | Required for >1M samples |
| Preprocessing offline, not per-batch | Tokenize once, train many times |
| Avoid Python objects in __getitem__ | Use numpy/tensors directly |

---

# 📝 Summary

| Technique | When to Use | Benefit |
|-----------|------------|---------|
| Multi-worker DataLoader | Always | Parallel data loading |
| Memory-mapped files | Text >10GB | O(1) RAM usage |
| Streaming datasets | Data >1TB | No full download |
| WebDataset | Image datasets | Efficient tar-based I/O |
| Data mixing | Multi-source training | Control data proportions |

---

# 🏁 Cheat Sheet

| Concept | Formula / Rule | Analogy |
|---------|---------------|--------|
| Throughput | $T = B / t_{\text{batch}}$ | Conveyor belt speed |
| Prefetch overlap | $t_{\text{eff}} = \max(t_{\text{load}}, t_{\text{train}})$ | Laying out clothes tonight |
| Optimal workers | $W \approx 4 \times N_{\text{GPUs}}$ | Prep cooks in kitchen |
| `pin_memory=True` | Enables async CPU→GPU transfer | Express lane to GPU 🚀 |
| `collate_fn` | Custom batch assembly | Sorting mail into bundles 📬 |
| Memory mapping | O(1) RAM for any dataset size | Bookmark in a library book |
| Streaming | Process data without full download | Netflix vs. downloading |

---

# 📚 Production-Scale Tools Comparison

| Tool | Best For | Key Feature | Reference |
|------|----------|------------|----------|
| **PyTorch DataLoader** | General purpose | Built-in, flexible | PyTorch Docs |
| **FFCV** | Image classification | 3.5× faster ImageNet | Leclerc et al., 2022 |
| **WebDataset** | Large image/tar datasets | Sequential I/O, sharding | Aizman et al., 2019 |
| **NVIDIA DALI** | GPU-accelerated preprocessing | Offload transforms to GPU | NVIDIA, 2018 |
| **Mosaic StreamingDataset** | Multi-node streaming | Deterministic resumption | MosaicML, 2023 |
| **HuggingFace Datasets** | NLP, streaming | Arrow-backed, hub integration | Lhoest et al., 2021 |

---

# 🔬 Research Citations

1. **Leclerc, G., et al.** (2022). *"FFCV: Accelerating Training by Removing Data Bottlenecks."* — Showed data loading is often the dominant bottleneck; FFCV achieves ImageNet training in minutes by eliminating Python overhead.
2. **Aizman, A., et al.** (2019). *"High Performance I/O For Large Scale Deep Learning."* — Introduced WebDataset, tar-based sequential I/O for scalable image pipelines.
3. **MosaicML** (2023). *"StreamingDataset: Fast, Deterministic, Elastic."* — Streaming with deterministic sample ordering for crash-resilient multi-node training.
4. **NVIDIA** (2018). *"DALI: A Library for Accelerating Data Processing in Deep Learning."* — GPU-accelerated data augmentation pipeline.
5. **Lhoest, Q., et al.** (2021). *"Datasets: A Community Library for Natural Language Processing."* — HuggingFace Datasets with Apache Arrow backend.

---

# 📖 Resources

### Books
- *Programming PyTorch for Deep Learning* — Ian Pointer (Ch. on DataLoaders)
- *Efficient Processing of Deep Neural Networks* — Sze et al. (data movement analysis)
- *Deep Learning Systems* — Vijay Janapa Reddi et al. (systems perspective on data pipelines)

### Papers
- [FFCV: Accelerating Training by Removing Data Bottlenecks](https://arxiv.org/abs/2306.12517) — Leclerc et al., 2022
- [High Performance I/O For Large Scale Deep Learning (WebDataset)](https://arxiv.org/abs/2105.03075) — Aizman et al., 2019
- [Datasets: A Community Library for NLP](https://arxiv.org/abs/2109.02846) — Lhoest et al., 2021

### Documentation & Tools
- [PyTorch DataLoader Official Docs](https://pytorch.org/docs/stable/data.html)
- [NVIDIA DALI Documentation](https://docs.nvidia.com/deeplearning/dali/user-guide/docs/)
- [Mosaic StreamingDataset](https://docs.mosaicml.com/streaming/stable/)
- [WebDataset GitHub](https://github.com/webdataset/webdataset)
- [FFCV Library](https://ffcv.io/)

---

**Tomorrow**: DeepSpeed & Training Frameworks.
