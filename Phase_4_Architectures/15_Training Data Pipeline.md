📘 DAY 15 — Training Data Pipeline: DataLoaders for LLM Pre-training

> *"Your model is only as good as the data you feed it — and HOW you feed it."* 🍽️

Goal:

Build a complete data pipeline for training language models — from raw text files to GPU-ready batches. Master document packing, sequence construction, shuffling, distributed data loading, and efficiency optimizations that make training on billions of tokens feasible.

If this is deeply understood:
You can set up data pipelines for any scale of LLM training, understand the bottlenecks (CPU preprocessing vs GPU compute), and design data loading strategies that don't waste GPU cycles.

---

## 🧠 Why Should You Care? (The Restaurant Analogy) 🍕

Imagine you're running a **massive restaurant** (your AI model). Your kitchen (GPU) can cook incredibly fast. But if the **ingredients arrive slowly** from the warehouse (disk), or the **prep cooks** (CPU workers) can't chop vegetables fast enough, your kitchen sits **idle and wasting money** 💸.

A **Training Data Pipeline** is the entire supply chain from raw ingredients (text files) → prepped ingredients (tokens) → plated dishes (GPU-ready batches). Get this wrong, and your expensive GPU sits around doing nothing. Get it right, and you train models **2-10× faster** with the same hardware.

> ⭐ **KEY INSIGHT**: At companies like OpenAI, Google, and Meta, data pipeline engineering is a **dedicated role**. A badly designed pipeline can waste millions of dollars in GPU compute.

### 🎯 What You'll Learn Today

| Topic | Beginner Analogy | Why It Matters |
|-------|-----------------|----------------|
| **Tokenization** | Translating English → secret code numbers | Models only understand numbers, not words |
| **Document Packing** | Filling shipping boxes with zero wasted space | Wastes 0% compute on empty padding |
| **DataLoader** | Conveyor belt feeding the kitchen | Keeps the GPU constantly busy |
| **Batching** | Grouping students into classrooms | Process many examples at once = faster |
| **Shuffling** | Shuffling a deck of cards | Prevents model from memorizing order |
| **Memory Mapping** | Reading a book without photocopying it first | Handle terabytes without running out of RAM |
| **num_workers** | Hiring more prep cooks | Parallel data preparation |
| **pin_memory** | Express lane to GPU | Faster CPU→GPU data transfer |

---

# 1️⃣ The Data Pipeline Overview

### 🏭 Think of It Like a Factory Assembly Line

Every step transforms the data a little more, getting it closer to what the model can actually eat. Just like a car factory: raw steel → stamped parts → assembled frames → painted cars → shipped out.

```
Raw Text Files (.txt, .jsonl, .parquet)        🗄️ "Raw ingredients in the warehouse"
         ↓
[Tokenization] — Text → Token IDs              🔤→🔢 "Translate words to numbers"
         ↓
[Document Concatenation] — Merge all tokens     📦 "Pour everything into one big bin"
  into one stream
         ↓
[Sequence Construction] — Cut into fixed-length  ✂️ "Cut into equal-sized pieces"
  sequences
         ↓
[Batching] — Group sequences into batches       👥 "Group pieces into trays"
         ↓
[Shuffling] — Randomize order                   🔀 "Shuffle the deck"
         ↓
[GPU Transfer] — Move to device                 🚀 "Express delivery to the kitchen"
         ↓
Training Loop                                   🔥 "Start cooking!"
```

> ⭐ **BEGINNER TIP**: If ANY step in this pipeline is slow, the ENTIRE training slows down. It's like a traffic jam — the slowest lane determines everyone's speed. This is called being **"data-bound"** vs **"compute-bound"**.

---

# 2️⃣ Step 1: Tokenization

> 🔤→🔢 **Beginner Analogy**: Imagine you're sending a message to someone who only speaks "Number Language." You need a **dictionary** that converts every word into a unique number. "Hello" → 15339, "world" → 995. **Tokenization** is building and using that dictionary. The twist? Modern tokenizers don't split by words — they split by **subwords** (pieces of words), so "unhappiness" might become ["un", "happi", "ness"] → [348, 17730, 1108].

### 📊 Why Tokenize First?

| Approach | Speed | Storage | Used By |
|----------|-------|---------|---------|
| Tokenize during training | 🐌 Slow (re-tokenizes every epoch) | Uses raw text (large) | Nobody serious |
| **Pre-tokenize once, save** | 🚀 Fast (just load numbers) | Binary files (compact) | **Everyone** ⭐ |

> ⭐ **PRODUCTION REALITY**: GPT-4, LLaMA, Gemini — ALL pre-tokenize their entire training corpus before training begins. For a 15 trillion token dataset, tokenization alone can take **days** on a large CPU cluster.

## Pre-Tokenize the Entire Corpus

For efficiency, tokenize ONCE and save to disk:

```python
from transformers import AutoTokenizer
from pathlib import Path
import numpy as np
import json

def tokenize_corpus(input_dir, output_dir, tokenizer_name="gpt2"):
    """Tokenize all text files and save as binary token arrays."""
    tokenizer = AutoTokenizer.from_pretrained(tokenizer_name)
    output_dir = Path(output_dir)
    output_dir.mkdir(exist_ok=True)
    
    total_tokens = 0
    
    for txt_file in Path(input_dir).glob("**/*.txt"):
        with open(txt_file, 'r', encoding='utf-8') as f:
            text = f.read()
        
        # Tokenize
        token_ids = tokenizer.encode(text, add_special_tokens=False)
        
        # Save as numpy array (uint16 for vocab < 65536, uint32 otherwise)
        dtype = np.uint16 if tokenizer.vocab_size < 65536 else np.uint32
        arr = np.array(token_ids, dtype=dtype)
        
        out_path = output_dir / f"{txt_file.stem}.bin"
        arr.tofile(out_path)
        
        total_tokens += len(token_ids)
        print(f"  {txt_file.name}: {len(token_ids):,} tokens")
    
    print(f"\nTotal: {total_tokens:,} tokens")
    return total_tokens

# Usage:
# tokenize_corpus("data/raw/", "data/tokenized/")
```

### Why Save as Binary?

> 💡 **Beginner Analogy**: Imagine writing the number 42. In text, that's 2 characters = 2 bytes. But in binary (as a uint16), it's ALWAYS exactly 2 bytes whether the number is 1 or 65535. No commas, no brackets, no wasted space. It's like vacuum-sealing your food instead of putting it in bulky containers.

- Text file: ~4 bytes per character
- JSON with token IDs: ~4 bytes per digit + delimiters
- Binary uint16: exactly 2 bytes per token

For 1B tokens: text = ~4GB, np.uint16 binary = 2GB (2× smaller, instant loading)

### 📐 The Math of Storage Efficiency

For a corpus with $N$ tokens:

$$\text{Binary storage} = N \times \text{sizeof(dtype)} = N \times 2 \text{ bytes (uint16)}$$

$$\text{JSON storage} \approx N \times (d + 1) \text{ bytes}$$

where $d$ is the average number of digits per token ID. For a vocabulary of 50,257 (GPT-2), $d \approx 4.5$, so JSON uses $\approx 2.75\times$ more space.

> ⭐ **REAL-WORLD NUMBERS**: The Pile dataset (800GB of text) tokenizes to roughly 300B tokens. As binary uint16, that's $300 \times 10^9 \times 2 = 600\text{GB}$. As JSON, it would be ~1.65TB. That difference matters when you need to shuffle data across a 100-node cluster!

### 🏭 Production Tokenization Pipeline (HuggingFace)

```python
# Production-grade tokenization using HuggingFace datasets library
# This handles sharding, multiprocessing, and caching automatically

from datasets import load_dataset

# Stream a massive dataset without downloading it all
dataset = load_dataset("allenai/c4", "en", streaming=True)

# Tokenize with multiprocessing
def tokenize_function(examples):
    return tokenizer(examples["text"], truncation=False)

tokenized = dataset.map(tokenize_function, batched=True, num_proc=64)
```

---

# 3️⃣ Step 2: Document Packing

> 📦 **Beginner Analogy**: You're shipping books 📚 of different sizes in identical boxes. You have two choices:
> 1. **Wasteful way**: Put one book per box, fill remaining space with bubble wrap (padding) — 80% of your truck is carrying air! 😱
> 2. **Smart way**: Stack books **end-to-end** in a long row, then cut the row into box-sized chunks. Now every box is 100% full! ✂️📦
>
> Document packing is method #2. Zero wasted space = zero wasted GPU compute.

## The Naive Approach (Wasteful) ❌

Pad each document to max_length:
```
Doc 1: [The, cat, sat, <pad>, <pad>, <pad>, ...]  → 80% padding waste!
Doc 2: [Hello, world, <pad>, <pad>, <pad>, ...]
```

Short documents = massive padding = wasted compute.

### 📐 The Cost of Padding

If your average document is $L_{\text{avg}}$ tokens and you pad to $L_{\text{max}}$:

$$\text{Waste ratio} = 1 - \frac{L_{\text{avg}}}{L_{\text{max}}}$$

For web data: $L_{\text{avg}} \approx 200$, $L_{\text{max}} = 2048 \Rightarrow$ waste $= 1 - \frac{200}{2048} = 90.2\%$ 😱

> ⭐ **That means 90% of your GPU is crunching ZEROS.** At $2/hr per GPU and a 30-day training run, that's **\$1,296 wasted per GPU** on padding alone!

## The Smart Approach: Concatenation + EOS ✅

Concatenate all documents with separator tokens:

```
[Doc1_tokens] <eos> [Doc2_tokens] <eos> [Doc3_tokens] <eos> ...
```

Then cut into fixed-length sequences:

```
Seq 1: [Doc1_tokens... Doc1_end <eos> Doc2_start...]
Seq 2: [Doc2_continued... Doc2_end <eos> Doc3_start...]
```

Zero padding waste! Every token contributes to training.

> 💡 **Think of it this way**: Instead of giving each student their own classroom (even if they only need one desk), you fill every classroom to capacity. Every seat = every token position is doing useful work.

### Cross-Document Attention

Issue: tokens from Doc2 can attend to Doc1 tokens in the same sequence.
Solution options:
1. **Don't worry about it** (most common, works fine in practice) ← ⭐ Used by GPT-3, LLaMA
2. **Document attention mask**: prevent cross-document attention ← Used by T5, UL2
3. **Pack with reset**: insert special tokens that reset attention

> 🔬 **RESEARCH INSIGHT**: Raffel et al. (2020) "Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer" found that packing with attention masking to prevent cross-document attention gives a small but consistent quality improvement (~0.1-0.3% on benchmarks). However, most practitioners skip it because the engineering complexity isn't worth the tiny gain at scale.

```python
class PackedDataset:
    """Packed dataset that concatenates documents efficiently."""
    
    def __init__(self, data_dir, seq_length, eos_token_id):
        self.seq_length = seq_length
        self.eos_token_id = eos_token_id
        
        # Memory-map all token files for efficient access
        self.data = []
        for bin_file in sorted(Path(data_dir).glob("*.bin")):
            tokens = np.memmap(bin_file, dtype=np.uint16, mode='r')
            self.data.append(tokens)
        
        # Concatenate virtually (track offsets)
        self.offsets = [0]
        for d in self.data:
            self.offsets.append(self.offsets[-1] + len(d))
        self.total_tokens = self.offsets[-1]
        
        self.num_sequences = self.total_tokens // seq_length
        print(f"Dataset: {self.total_tokens:,} tokens, {self.num_sequences:,} sequences")
    
    def __len__(self):
        return self.num_sequences
    
    def __getitem__(self, idx):
        start = idx * self.seq_length
        end = start + self.seq_length + 1  # +1 for target
        
        # Get tokens across file boundaries
        tokens = self._get_tokens(start, end)
        
        x = torch.tensor(tokens[:-1], dtype=torch.long)
        y = torch.tensor(tokens[1:], dtype=torch.long)
        return x, y
    
    def _get_tokens(self, start, end):
        """Get tokens from virtual concatenation, handling file boundaries."""
        tokens = []
        remaining = end - start
        current_pos = start
        
        for i, d in enumerate(self.data):
            file_start = self.offsets[i]
            file_end = self.offsets[i + 1]
            
            if current_pos >= file_end:
                continue
            if current_pos < file_start:
                continue
            
            local_start = current_pos - file_start
            local_end = min(local_start + remaining, len(d))
            
            tokens.extend(d[local_start:local_end].tolist())
            remaining -= (local_end - local_start)
            current_pos = file_end
            
            if remaining <= 0:
                break
        
        return tokens
```

### 📊 Packing Strategies Comparison

| Strategy | Padding Waste | Cross-Doc Attention? | Complexity | Used By |
|----------|:------------:|:--------------------:|:----------:|---------|
| Pad each doc | 60-90% 😱 | No | Simple | Small-scale only |
| **Concatenate + EOS** | **0%** ⭐ | Yes (harmless) | Medium | **GPT-3, LLaMA, Mistral** |
| Concat + attention mask | 0% | Prevented | Complex | T5, UL2 |
| Best-fit bin packing | ~2-5% | Prevented | Very complex | InstructGPT, some SFT |

---

# 4️⃣ Step 3: Efficient DataLoader

> 🏭 **Beginner Analogy — The Conveyor Belt**: A `DataLoader` is a **conveyor belt** in your factory. It automatically:
> - Grabs raw materials from storage (reads data from disk) 📂
> - Groups them into batches (puts items on the same tray) 🍽️
> - Shuffles the order (so the model doesn't memorize patterns) 🔀
> - Delivers them to the machine (sends to GPU) 🚀
>
> Without a DataLoader, you'd have to manually grab one item at a time. Imagine a sushi restaurant without the conveyor belt — the chef would have to walk to each customer! 🍣

### ⭐ Key DataLoader Parameters Explained

| Parameter | What It Does | Beginner Analogy | Typical Value |
|-----------|-------------|-----------------|---------------|
| `batch_size` | How many samples per batch | Students per classroom | 32-512 |
| `num_workers` | Parallel data loading processes | **Hiring more prep cooks** 👨‍🍳 | 4-8 |
| `pin_memory` | Pre-allocate GPU-friendly memory | **Express lane to GPU** 🏎️ | `True` always |
| `prefetch_factor` | Batches loaded ahead of time | Pre-making next orders | 2 |
| `persistent_workers` | Keep workers alive between epochs | Don't fire & rehire cooks | `True` |
| `drop_last` | Drop incomplete final batch | Toss the half-empty tray | `True` for training |
| `shuffle` | Randomize sample order | **Shuffle the deck of cards** 🃏 | `True` for training |

### 📐 Batching Math

For a batch of $B$ sequences, each of length $L$, the input tensor shape is:

$$\mathbf{X} \in \mathbb{Z}^{B \times L}$$

Total tokens per batch: $B \times L$. With $B = 32$ and $L = 2048$, each batch contains $65{,}536$ tokens.

**Throughput** in tokens/second:

$$\text{Throughput} = \frac{B \times L}{\text{time per step (seconds)}}$$

> ⭐ **WHY num_workers MATTERS**: Your GPU processes a batch in ~50ms, but loading data from disk takes ~200ms. With `num_workers=1`, the GPU waits 150ms doing NOTHING. With `num_workers=4`, four cooks prep batches in parallel, and one is always ready when the GPU finishes. The GPU **never waits**. 🎯

```python
from torch.utils.data import DataLoader, IterableDataset
import random

class StreamingDataset(IterableDataset):
    """Memory-efficient streaming dataset for large corpora."""
    
    def __init__(self, data_files, seq_length, tokenizer, shuffle_files=True):
        self.data_files = data_files
        self.seq_length = seq_length
        self.tokenizer = tokenizer
        self.shuffle_files = shuffle_files
    
    def __iter__(self):
        worker_info = torch.utils.data.get_worker_info()
        
        # Partition files across workers
        if worker_info is not None:
            files = self.data_files[worker_info.id::worker_info.num_workers]
        else:
            files = self.data_files
        
        if self.shuffle_files:
            random.shuffle(files)
        
        buffer = []
        
        for file_path in files:
            tokens = np.memmap(file_path, dtype=np.uint16, mode='r')
            buffer.extend(tokens.tolist())
            
            # Yield complete sequences from buffer
            while len(buffer) >= self.seq_length + 1:
                seq = buffer[:self.seq_length + 1]
                buffer = buffer[self.seq_length:]
                
                x = torch.tensor(seq[:-1], dtype=torch.long)
                y = torch.tensor(seq[1:], dtype=torch.long)
                yield x, y

# Create DataLoader
def create_dataloader(data_dir, seq_length, batch_size, tokenizer, num_workers=4):
    """Create an efficient DataLoader for LLM training."""
    data_files = sorted(Path(data_dir).glob("*.bin"))
    
    dataset = StreamingDataset(data_files, seq_length, tokenizer)
    
    dataloader = DataLoader(
        dataset,
        batch_size=batch_size,
        num_workers=num_workers,
        pin_memory=True,        # Faster CPU→GPU transfer
        prefetch_factor=2,       # Prefetch 2 batches per worker
        persistent_workers=True, # Don't restart workers each epoch
    )
    
    return dataloader
```

### 🔀 Why Shuffling Matters

> 🃏 **Beginner Analogy**: Imagine studying for an exam by reading all history chapters first, then all science, then all math. You'd **forget history by the time you reach math**! Instead, mix all subjects randomly — your brain retains more because it's constantly switching context.
>
> The same applies to neural networks. If you feed all Wikipedia articles first, then all code, the model **catastrophically forgets** Wikipedia by the time it finishes code. Shuffling prevents this!

Mathematically, shuffling ensures the gradient estimate from each mini-batch is an **unbiased estimator** of the true gradient:

$$\mathbb{E}[\nabla_\theta \mathcal{L}_{\text{batch}}] = \nabla_\theta \mathcal{L}_{\text{full dataset}}$$

Without shuffling, correlated batches create **biased gradients** that can make training unstable or slow.

### 🏭 Production: HuggingFace Streaming Datasets

```python
# For TB-scale data, you NEVER download the whole thing
# HuggingFace streaming lets you iterate without full download

from datasets import load_dataset

# Stream C4 (305GB compressed, ~800GB uncompressed)
dataset = load_dataset("allenai/c4", "en", streaming=True, split="train")

# Iterate lazily — only downloads what you need
for example in dataset:
    text = example["text"]
    # tokenize and train...
    
# Shuffle with a buffer (pseudo-random, memory-efficient)
shuffled = dataset.shuffle(seed=42, buffer_size=10_000)
```

> ⭐ **PRODUCTION TIP**: At scale, companies use **WebDataset** (tar-based sharded format) or **Mosaic StreamingDataset** for multi-node streaming with deterministic resumption after crashes. These handle the "we trained for 3 days and it crashed — can we resume from the exact sample?" problem.

---

# 5️⃣ Step 4: Handling Multiple Data Sources

> 🍲 **Beginner Analogy**: Imagine making a smoothie 🥤. You don't use ONLY bananas — you blend bananas (67%), strawberries (15%), yogurt (8%), honey (5%), and ice (5%). Each ingredient contributes different nutrients (flavors of knowledge). Too much of one = bad taste. The **data mix** is your smoothie recipe!

## Data Mixing for Pre-training

Modern LLMs train on a MIX of data sources:

### 📊 Real-World Data Mixes

| Model | Web Data | Code | Books | Wikipedia | Scientific | Other |
|-------|:--------:|:----:|:-----:|:---------:|:----------:|:-----:|
| **LLaMA** | 67% | 4.5% | 4.5% | 4.5% | 2.5% | 17% (CC/C4) |
| **GPT-3** | 60% (CC) | — | 16% | 3% | — | 21% (WebText/Books) |
| **Chinchilla** | ~80% | ~5% | ~5% | ~5% | — | ~5% |
| **Phi-1** (Microsoft) | 0% | 0% | 0% | 0% | 0% | **100% textbook-quality** ⭐ |

> 🔬 **LANDMARK RESEARCH**: Gunasekar et al. (2023) **"Textbooks Are All You Need"** showed that a 1.3B parameter model trained on **only 7B tokens** of high-quality textbook data outperformed models trained on 100× more web data. This paper shook the field — it proved **data quality >> data quantity**. A curated 7B tokens beat a sloppy 1T tokens!

```python
class MixedDataset:
    """Mix multiple datasets with specified proportions."""
    
    def __init__(self, datasets, weights):
        """
        datasets: list of dataset objects
        weights: list of sampling weights (must sum to 1)
        """
        self.datasets = datasets
        self.weights = weights
        assert abs(sum(weights) - 1.0) < 1e-6
    
    def __iter__(self):
        iterators = [iter(d) for d in self.datasets]
        
        while True:
            # Sample which dataset to draw from
            dataset_idx = random.choices(range(len(self.datasets)), weights=self.weights)[0]
            
            try:
                yield next(iterators[dataset_idx])
            except StopIteration:
                # Restart exhausted dataset
                iterators[dataset_idx] = iter(self.datasets[dataset_idx])
                yield next(iterators[dataset_idx])

# Example: LLaMA-style data mix
# webpages: 67%, code: 15%, books: 8%, wikipedia: 5%, papers: 5%
```

> ⭐ **WHY THE MIX MATTERS**: The Doremi algorithm (Xie et al., 2023, "DoReMi: Optimizing Data Mixtures by Reweighting") showed that **automatically learning the optimal data mix** can improve model performance by the equivalent of training with 2× the compute. The mix is arguably more important than the model architecture!

## Curriculum Learning (Advanced)

> 📚 **Beginner Analogy**: Teaching a child. You start with picture books 🖼️, then chapter books 📖, then textbooks 📕, then research papers 📄. You don't hand a 5-year-old a PhD thesis! **Curriculum learning** is the same idea — start the model on "easy" data, gradually introduce "harder" data.

Start training with "easy" data, gradually add "harder" data:
```python
class CurriculumScheduler:
    def __init__(self, total_steps):
        self.total_steps = total_steps
    
    def get_weights(self, current_step):
        progress = current_step / self.total_steps
        if progress < 0.3:
            return {'wiki': 0.5, 'books': 0.3, 'web': 0.2}
        elif progress < 0.7:
            return {'wiki': 0.3, 'books': 0.2, 'web': 0.3, 'code': 0.2}
        else:
            return {'wiki': 0.2, 'books': 0.15, 'web': 0.35, 'code': 0.2, 'math': 0.1}
```

> 🔬 **RESEARCH**: Bengio et al. (2009) **"Curriculum Learning"** first proposed this idea. Recent work by Chen et al. (2023) on **"Skill-It!"** showed that ordering training data by skill complexity can achieve the same performance with **50% fewer training tokens**. This is especially powerful for math and code, where problems have natural difficulty levels.

---

# 6️⃣ Memory Mapping for Large Files

> 📖 **Beginner Analogy**: Imagine you have a **1,000-page encyclopedia** 📚 on your bookshelf (disk). The normal way to read it: photocopy ALL 1,000 pages onto your desk (RAM), then start reading. But your desk only fits 100 pages! 💥
>
> **Memory mapping** is like having a magical bookmark 🔖 that lets you read ANY page directly from the shelf without photocopying it first. The operating system handles the magic — it loads only the pages you actually look at, and quietly puts them back when you're done.
>
> This is how LLMs handle **terabytes** of training data on machines with only **64-256GB** of RAM!

### 📐 Why Memory Mapping Works

Without mmap: Must load entire file into RAM. For a 500GB token file: **need 500GB RAM** 💸

With mmap: OS loads only the **pages you access** (typically 4KB chunks). Reading token at position $i$ only loads the 4KB page containing that token:

$$\text{Page number} = \left\lfloor \frac{i \times \text{sizeof(token)}}{\text{page\_size}} \right\rfloor$$

For uint16 tokens with 4KB pages, each page holds $\frac{4096}{2} = 2048$ tokens.

> ⭐ **MAGIC PROPERTY**: `np.memmap` files behave EXACTLY like regular numpy arrays in code. You can slice them, index them, iterate over them — the OS transparently loads/unloads pages behind the scenes. Your code doesn't change at all!

```python
import numpy as np
import torch
from torch.utils.data import Dataset

class MemmapDataset(Dataset):
    """Dataset using memory-mapped files for zero-copy data access.
    
    Memory mapping lets us access terabyte-scale token files
    without loading them into RAM. The OS handles paging.
    """
    
    def __init__(self, file_path, seq_length, dtype=np.uint16):
        self.seq_length = seq_length
        self.data = np.memmap(file_path, dtype=dtype, mode='r')
        self.num_samples = len(self.data) // seq_length
        print(f"MemmapDataset: {len(self.data):,} tokens from {file_path}")
    
    def __len__(self):
        return self.num_samples - 1  # -1 because we need target
    
    def __getitem__(self, idx):
        start = idx * self.seq_length
        chunk = self.data[start:start + self.seq_length + 1]
        x = torch.from_numpy(chunk[:-1].astype(np.int64))
        y = torch.from_numpy(chunk[1:].astype(np.int64))
        return x, y
```

### 📊 Memory Mapping vs Regular Loading

| Metric | `np.load()` (regular) | `np.memmap()` (mapped) |
|--------|:---------------------:|:---------------------:|
| Load 500GB file | 500GB RAM needed 💀 | ~0MB RAM (on-demand) ⭐ |
| Time to "open" | Minutes (full copy) | Instant (no copy) |
| Random access | Fast (all in RAM) | Slightly slower (OS paging) |
| Sequential access | Fast | Equally fast (OS prefetches) |
| Multiple processes | Each needs full copy | **Share** same physical pages |

> 🏭 **PRODUCTION**: Meta's LLaMA training code uses memory-mapped files extensively. Their `metaseq` library memory-maps all shard files across 2,048 GPUs — the OS efficiently shares pages between processes on the same machine, dramatically reducing total RAM usage.

---

# 7️⃣ Data Quality Filters

> 🧹 **Beginner Analogy**: You're making a salad 🥗. You don't just dump everything in — you wash the lettuce, remove rotten tomatoes, pick out stones. **Data quality filtering** is the same: you remove garbage text, duplicates, toxic content, and boilerplate before feeding it to the model. A model trained on garbage WILL produce garbage — "garbage in, garbage out" is the #1 law of machine learning! 🗑️➡️🗑️

### ⭐ Why Data Quality is THE Most Important Factor

> 🔬 **LANDMARK RESEARCH**: 
> - **Gunasekar et al. (2023) "Textbooks Are All You Need"**: Microsoft's Phi-1.3B model, trained on only 7B high-quality tokens, outperformed models trained on 100× more data. **Quality beats quantity by 100×.**
> - **Lee et al. (2022) "Deduplicating Training Data Makes Language Models Better"**: Simply removing duplicate documents improved perplexity by 10% and reduced memorization of training data (privacy win!).
> - **Penedo et al. (2023) "The RefinedWeb Dataset"**: Showed that aggressively filtered web data can match curated datasets, leading to the Falcon models.
> - **Soldaini et al. (2024) "Dolma"**: The open dataset behind OLMo, with detailed documentation of every filtering step — a gold standard for reproducible data pipelines.

### 📊 Common Data Quality Filters

| Filter | What It Catches | Impact on Quality |
|--------|----------------|:-----------------:|
| **Length filter** | Empty/tiny documents | ⭐⭐⭐ |
| **Repetition filter** | "Buy now! Buy now! Buy now!" | ⭐⭐⭐⭐ |
| **Language detection** | Non-target language text | ⭐⭐⭐ |
| **Perplexity filter** | Gibberish / random text | ⭐⭐⭐⭐ |
| **Deduplication** | Exact/near duplicate documents | ⭐⭐⭐⭐⭐ |
| **Toxic content filter** | Harmful/offensive content | ⭐⭐⭐ (safety) |
| **PII removal** | Names, emails, phone numbers | ⭐⭐⭐ (privacy) |
| **Decontamination** | Test set leaks in training data | ⭐⭐⭐⭐⭐ (evaluation integrity) |

```python
def filter_document(text):
    """Quality filters for pre-training data."""
    # Length filter
    if len(text) < 100:
        return False
    
    # Repetition filter
    words = text.split()
    if len(set(words)) / max(len(words), 1) < 0.1:  # >90% repeated words
        return False
    
    # Language detection (basic)
    ascii_ratio = sum(1 for c in text if ord(c) < 128) / len(text)
    if ascii_ratio < 0.5:  # Likely not English (adjust per need)
        return False
    
    # Perplexity filter (using a small reference model)
    # High perplexity = likely garbage/random text
    # Too low perplexity = likely boilerplate/repetitive
    
    return True

def decontaminate(train_data, test_data, n_gram=13):
    """Remove test set contamination from training data."""
    # Extract n-grams from test set
    test_ngrams = set()
    for text in test_data:
        words = text.split()
        for i in range(len(words) - n_gram + 1):
            test_ngrams.add(tuple(words[i:i + n_gram]))
    
    # Filter training data
    clean_train = []
    for text in train_data:
        words = text.split()
        contaminated = False
        for i in range(len(words) - n_gram + 1):
            if tuple(words[i:i + n_gram]) in test_ngrams:
                contaminated = True
                break
        if not contaminated:
            clean_train.append(text)
    
    return clean_train
```

### 🔬 Deep Dive: Deduplication Methods

> 🎯 **Why Deduplication is Critical**: If a sentence appears 1000× in your training data, the model will **memorize** it rather than **learn** from it. Lee et al. (2022) showed that deduplication:
> - Reduces training loss by ~10%
> - Decreases verbatim memorization by 10×
> - Actually IMPROVES downstream task performance

| Method | Scale | How It Works | Used By |
|--------|:-----:|-------------|---------|
| **Exact hash** | TB+ | Hash each document, remove identical hashes | Basic dedup |
| **MinHash + LSH** | TB+ | Approximate near-duplicate detection (Jaccard similarity) | **C4, The Pile, RedPajama** ⭐ |
| **Suffix Array** | 100GB | Find all repeated substrings of length $n$ | Lee et al. 2022 |
| **SemDeDup** | 100B+ | Embedding-based semantic dedup | Abbas et al. 2023 |

$$\text{Jaccard similarity}(A, B) = \frac{|A \cap B|}{|A \cup B|}$$

where $A$ and $B$ are the sets of n-grams in two documents. If $J(A,B) > 0.8$, they're near-duplicates.

---

# 8️⃣ Distributed Data Loading

> 🌐 **Beginner Analogy**: You're running a **chain of restaurants** with 64 kitchens (GPUs) across 8 buildings (nodes). Each kitchen needs its OWN supply of ingredients — if two kitchens get the same potatoes, you're cooking the same dish twice (wasted work!). A `DistributedSampler` is the **logistics manager** who ensures every kitchen gets a **unique, non-overlapping** slice of the total ingredients. No duplicates, no gaps.

### 📐 How Data Gets Split

With $N$ total samples and $W$ GPUs (world_size), each GPU gets:

$$\text{Samples per GPU} = \left\lfloor \frac{N}{W} \right\rfloor$$

GPU with rank $r$ gets samples at indices: $\{r, r+W, r+2W, \ldots\}$ (interleaved assignment)

> ⭐ **CRITICAL BUG ALERT**: Forgetting `sampler.set_epoch(epoch)` is the **#1 distributed training bug**. Without it, every GPU gets the SAME order every epoch. The model sees data in the same order repeatedly, which severely hurts generalization. This single line of code has caused debugging sessions lasting days at multiple companies! 🐛

```python
from torch.utils.data.distributed import DistributedSampler

def create_distributed_dataloader(dataset, batch_size, rank, world_size):
    """Create DataLoader for distributed training."""
    sampler = DistributedSampler(
        dataset,
        num_replicas=world_size,
        rank=rank,
        shuffle=True,
        seed=42
    )
    
    dataloader = DataLoader(
        dataset,
        batch_size=batch_size,
        sampler=sampler,        # DistributedSampler handles shuffling
        num_workers=4,
        pin_memory=True,
        drop_last=True,         # Drop incomplete last batch (important for DDP)
    )
    
    return dataloader, sampler

# In training loop:
# for epoch in range(num_epochs):
#     sampler.set_epoch(epoch)  # CRITICAL: ensures different shuffling each epoch
#     for batch in dataloader:
#         train_step(batch)
```

### 📊 Distributed Data Pipeline Architecture

```
Node 0 (8 GPUs)                    Node 1 (8 GPUs)
┌─────────────────────┐           ┌─────────────────────┐
│ GPU 0: samples 0,16,32...│       │ GPU 8: samples 8,24,40...│
│ GPU 1: samples 1,17,33...│       │ GPU 9: samples 9,25,41...│
│ ...                       │       │ ...                       │
│ GPU 7: samples 7,23,39...│       │ GPU 15: samples 15,31,47..│
└─────────────────────┘           └─────────────────────┘
         ↕ AllReduce (gradient sync)          ↕
```

> 🏭 **PRODUCTION TIP**: For truly massive runs (1000+ GPUs), DistributedSampler isn't enough. Teams use **sharded data** — each node gets its own shard files on local NVMe SSDs. No network file system bottleneck. Meta's training infrastructure pre-distributes data shards to each node before training begins.
```

---

# 9️⃣ Performance Optimization

> ⚡ **Beginner Analogy**: You're driving a race car (GPU), but you keep stopping at red lights (data loading). Performance optimization is about **turning every red light green** — making sure data arrives BEFORE the GPU needs it.

## Profiling Bottlenecks

> 🔍 **First Rule of Optimization**: MEASURE before you optimize! Don't guess where the bottleneck is — **profile it**. The bottleneck is almost never where you think it is.

```python
import time

class DataLoadProfiler:
    def __init__(self, dataloader):
        self.dataloader = dataloader
    
    def profile(self, num_batches=100):
        times = []
        data_iter = iter(self.dataloader)
        
        for i in range(num_batches):
            start = time.time()
            batch = next(data_iter)
            # Simulate moving to GPU
            for key in batch:
                if isinstance(batch[key], torch.Tensor):
                    batch[key] = batch[key].cuda(non_blocking=True)
            torch.cuda.synchronize()
            times.append(time.time() - start)
        
        print(f"Average batch load time: {1000*sum(times)/len(times):.1f}ms")
        print(f"Throughput: {len(times)/sum(times):.0f} batches/sec")
        print(f"If batch_size=32, seq_len=512: {32*512*len(times)/sum(times):.0f} tokens/sec")
```

### 📐 The Pipeline Math

Your training throughput is limited by the **slower** of two stages:

$$\text{Throughput} = \min\left(\frac{B \times L}{t_{\text{data}}},\; \frac{B \times L}{t_{\text{compute}}}\right)$$

where $t_{\text{data}}$ is time to load a batch and $t_{\text{compute}}$ is time for the forward + backward pass.

- If $t_{\text{data}} > t_{\text{compute}}$: You are **data-bound** 🐌 → fix your pipeline!
- If $t_{\text{compute}} > t_{\text{data}}$: You are **compute-bound** ✅ → pipeline is healthy

> ⭐ **GOAL**: Always be compute-bound. If you're data-bound, you're paying for GPU time while the GPU does nothing.

## Tips for Fast Data Loading

1. **Pre-tokenize everything**: never tokenize during training
2. **Use binary files**: np.uint16/np.uint32, not JSON
3. **Memory map files**: `np.memmap` for zero-copy access
4. **Multiple workers**: `num_workers=4-8` for parallel loading
5. **Pin memory**: `pin_memory=True` for faster CPU→GPU transfer
6. **Prefetch**: `prefetch_factor=2` to overlap loading with training

### 🏎️ Performance Optimization Decision Tree

```
Is your GPU utilization < 90%?
├── YES → You're data-bound! Fix the pipeline:
│   ├── Are you tokenizing during training? → Pre-tokenize!
│   ├── Using JSON/text files? → Convert to binary
│   ├── num_workers=0? → Set to 4-8
│   ├── pin_memory=False? → Set to True
│   ├── Loading from network drive? → Copy to local SSD
│   └── Still slow? → Profile with DataLoadProfiler above
└── NO → You're compute-bound ✅ (pipeline is fine)
```

### 📊 Impact of Each Optimization

| Optimization | Typical Speedup | Effort | Priority |
|-------------|:--------------:|:------:|:--------:|
| Pre-tokenization | 10-100× | Medium | ⭐⭐⭐⭐⭐ |
| Binary files (uint16) | 2-5× | Low | ⭐⭐⭐⭐⭐ |
| Memory mapping | Enables TB-scale | Low | ⭐⭐⭐⭐ |
| `num_workers=4` | 2-4× | Trivial | ⭐⭐⭐⭐⭐ |
| `pin_memory=True` | 1.1-1.5× | Trivial | ⭐⭐⭐⭐ |
| `prefetch_factor=2` | 1.1-1.3× | Trivial | ⭐⭐⭐ |
| Local SSD vs NFS | 2-10× | Infrastructure | ⭐⭐⭐⭐ |
| Document packing | 2-10× (vs padding) | Medium | ⭐⭐⭐⭐⭐ |

---

# 📝 Summary

| Component | Key Decision |
|-----------|-------------|
| Pre-tokenization | Tokenize once, save as binary (uint16) |
| Document packing | Concatenate with <eos>, cut into fixed-length sequences |
| Memory mapping | np.memmap for terabyte-scale data |
| Data mixing | Weight different sources (web, code, books) |
| Quality filtering | Length, repetition, language, perplexity filters |
| Decontamination | Remove test set n-grams from training data |
| Distributed | DistributedSampler, set_epoch() per epoch |
| Performance | Binary files + memmap + num_workers + pin_memory |

---

# 🧾 CHEAT SHEET: Training Data Pipeline in 60 Seconds

```
┌─────────────────────────────────────────────────────────┐
│           🚀 TRAINING DATA PIPELINE CHEAT SHEET         │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  1. TOKENIZE ONCE → save as .bin (uint16)               │
│     tokenizer.encode(text) → np.array.tofile()          │
│                                                          │
│  2. PACK DOCUMENTS → concatenate with <eos>             │
│     [doc1] <eos> [doc2] <eos> → cut into chunks         │
│     Zero padding waste!                                  │
│                                                          │
│  3. MEMORY MAP → np.memmap(file, dtype=np.uint16, 'r')  │
│     TB-scale files, zero RAM usage                       │
│                                                          │
│  4. DATALOADER → the conveyor belt                       │
│     batch_size=32, num_workers=4, pin_memory=True        │
│     prefetch_factor=2, persistent_workers=True           │
│                                                          │
│  5. SHUFFLE → always! (sampler.set_epoch(epoch) for DDP) │
│                                                          │
│  6. QUALITY FILTER → dedup, length, repetition, language │
│     Data quality > data quantity (100×!)                  │
│                                                          │
│  7. MIX SOURCES → web 67% + code 15% + books 8% + ...   │
│                                                          │
│  GOAL: GPU utilization > 90% (compute-bound, not data-   │
│  bound). If GPU is idle, fix the pipeline!               │
└─────────────────────────────────────────────────────────┘
```

---

# 🔬 Deep Research & Citations

### Foundational Papers

| Paper | Year | Key Contribution |
|-------|:----:|-----------------|
| **"Textbooks Are All You Need"** — Gunasekar et al. | 2023 | Proved data quality >> quantity. 1.3B model on 7B curated tokens beat 100× larger datasets |
| **"Deduplicating Training Data Makes Language Models Better"** — Lee et al. | 2022 | Showed dedup improves perplexity 10%, reduces memorization 10× |
| **"The RefinedWeb Dataset for Falcon LLM"** — Penedo et al. | 2023 | Demonstrated filtered web data matches curated data; open-sourced filtering pipeline |
| **"Scaling Data-Constrained Language Models"** — Muennighoff et al. | 2023 | Characterized behavior when repeating data (up to 4 epochs helps, then degrades) |
| **"DoReMi: Optimizing Data Mixtures"** — Xie et al. | 2023 | Automatic data mix optimization = equivalent to 2× compute |
| **"Curriculum Learning"** — Bengio et al. | 2009 | Pioneered ordering training data by difficulty |
| **"Dolma: An Open Corpus"** — Soldaini et al. | 2024 | Gold standard for reproducible, documented data pipelines |
| **"D4: Improving LLM Pretraining via Document De-Duplication and Diversification"** — Tirumala et al. | 2023 | Combined dedup with diversity sampling for better pre-training |
| **"Data Selection for Language Models via Importance Resampling"** — Xie et al. | 2023 | DSIR algorithm for selecting high-quality data subsets |

### ⭐ Key Scaling Laws for Data

The **Chinchilla scaling law** (Hoffmann et al., 2022) states the optimal ratio:

$$N_{\text{opt}} \approx 20 \times D$$

where $N_{\text{opt}}$ is the optimal number of training tokens and $D$ is the number of model parameters. A 7B parameter model should train on ~140B tokens.

But Llama 3 (2024) trained a 8B model on **15T tokens** (107× the Chinchilla-optimal), showing that **over-training small models on more data** produces surprisingly strong results for inference-efficient deployment.

---

# 🎯 Self-Test Questions

1. **Why do we pre-tokenize instead of tokenizing during training?** (Hint: what happens to GPU utilization?)
2. **What's the padding waste ratio if avg doc = 500 tokens, max_length = 4096?** (Use the formula!)
3. **Why is `sampler.set_epoch(epoch)` critical in distributed training?**
4. **How does memory mapping let you use a 1TB file on a 64GB RAM machine?**
5. **If your GPU utilization is 40%, where's the bottleneck likely to be?**
6. **Why did Microsoft's Phi-1 outperform much larger models despite seeing 100× fewer tokens?**

---

# 🗺️ Connection to the Big Picture

```
Data Pipeline (TODAY) → feeds → Training Loop → uses → Optimizer
       ↑                              ↓                    ↓
  Data Quality              Loss Function          LR Schedule
  determines what         determines how           determines
  the model SEES          it LEARNS                learning SPEED
```

> ⭐ **FINAL INSIGHT**: The data pipeline is the **foundation** of everything. A perfect architecture with perfect hyperparameters will produce garbage if fed garbage data. As Andrej Karpathy famously said: *"Most of neural network training is data wrangling."* The companies winning the AI race aren't just building better models — they're building better data pipelines. 🏆

**Tomorrow**: Learning Rate Scheduling — Warmup, Cosine Decay, and the learning rate schedule that all modern LLMs use.
