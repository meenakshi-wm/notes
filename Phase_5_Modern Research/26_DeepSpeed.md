📘 DAY 26 — DeepSpeed & Large-Scale Training Frameworks

Goal:

Master DeepSpeed ZeRO — the framework that enables training models too large for any single GPU. Understand ZeRO stages, offloading, and when to use DeepSpeed vs FSDP vs Megatron.

> 🧠 **Beginner Analogy**: Imagine you're building a massive LEGO set — so big that no single table can hold all the pieces, instructions, and tools at once. DeepSpeed is like a system that lets multiple tables (GPUs) work together, each holding only a portion of the pieces, so together they can build something no single table could handle alone!

> ⭐ **Why This Matters**: DeepSpeed was used to train BLOOM (176B parameters), powers parts of ChatGPT's training infrastructure, and is the go-to framework for training models beyond 10 billion parameters. If you want to build or fine-tune large language models, you *need* to understand DeepSpeed.

---

# 1️⃣ The Memory Problem for Large Models

> 🎒 **Beginner Analogy**: Think of GPU memory like a backpack. A small model is a notebook — fits easily. A 13B parameter model is like packing an entire library into one backpack. It simply won't fit! We need a way to split the load across multiple backpacks (GPUs).

Training a 13B model with Adam in FP16:
- Weights (FP16): 26 GB
- Gradients (FP16): 26 GB
- Optimizer states (FP32 master + momentum + variance): 156 GB
- Activations: varies
- **Total: ~208 GB minimum** → doesn't fit on ANY single GPU

📐 **Memory Formula** — For a model with $\Psi$ parameters using mixed-precision Adam:

$$M_{\text{total}} = \underbrace{2\Psi}_{\text{FP16 weights}} + \underbrace{2\Psi}_{\text{FP16 gradients}} + \underbrace{4\Psi + 4\Psi + 4\Psi}_{\text{FP32 master + momentum + variance}} = 16\Psi \text{ bytes}$$

For 13B parameters: $16 \times 13 \times 10^9 = 208$ GB 🤯

---

# 2️⃣ ZeRO: Zero Redundancy Optimizer

Key insight: In DDP, every GPU has a full copy of everything. ZeRO eliminates this redundancy.

> 📚 **Research**: Rajbhandari et al. (2020) — *"ZeRO: Memory Optimizations Toward Training Trillion Parameter Models"* introduced this breakthrough approach (Microsoft Research).

### ZeRO Stage 1: Partition Optimizer States
Each GPU holds 1/N of optimizer states. All-gather before update.
Memory: 26 + 26 + 156/N GB per GPU (N GPUs)

> 🔧 **Analogy**: ZeRO-1 = Workers in a workshop who **share the tool storage room** (optimizer states) but each keeps their own materials (weights) and scrap bin (gradients) at their workstation.

📐 **Stage 1 Memory per GPU**:

$$M_1 = \frac{2\Psi + 2\Psi}{N_d} + 2\Psi$$

where $\Psi$ = number of parameters, $N_d$ = number of GPUs (data-parallel degree).

### ZeRO Stage 2: + Partition Gradients
Each GPU holds 1/N of gradients.
Memory: 26 + 26/N + 156/N GB per GPU

> 🗑️ **Analogy**: ZeRO-2 = Workers now **also share the scrap bin** (gradients). Each worker only keeps the scrap from the part they're responsible for cleaning up.

📐 **Stage 2 Memory per GPU**:

$$M_2 = \frac{2\Psi + 2\Psi + 2\Psi}{N_d} + 2\Psi$$

### ZeRO Stage 3: + Partition Parameters
Each GPU holds 1/N of weights.
Memory: 26/N + 26/N + 156/N = 208/N GB per GPU

> 🤝 **Analogy**: ZeRO-3 = Workers **share everything** — tools, materials, and scrap. Each worker only holds the **current piece** they're actively working on, fetching what they need from others on demand.

📐 **Stage 3 Memory per GPU** — everything is partitioned:

$$M_3 = \frac{12\Psi}{N_d}$$

With 8 GPUs: 208/8 = 26 GB per GPU. Fits on one A100!

⭐ **ZeRO Stages At a Glance**:

| Stage | What's Partitioned | Memory Savings | Communication Cost |
|-------|-------------------|---------------|-------------------|
| Stage 0 (DDP) | Nothing | 1× (baseline) | Low |
| Stage 1 | Optimizer states | ~4× | Low |
| Stage 2 | + Gradients | ~8× | Medium |
| Stage 3 | + Parameters | ~$N_d$× | Higher |

---

# 3️⃣ DeepSpeed Configuration

```json
{
    "bf16": {"enabled": true},
    "zero_optimization": {
        "stage": 2,
        "allgather_partitions": true,
        "reduce_scatter": true,
        "overlap_comm": true,
        "contiguous_gradients": true
    },
    "gradient_accumulation_steps": 4,
    "gradient_clipping": 1.0,
    "train_batch_size": 32,
    "train_micro_batch_size_per_gpu": 4,
    "optimizer": {
        "type": "AdamW",
        "params": {"lr": 2e-4, "betas": [0.9, 0.95], "weight_decay": 0.1}
    },
    "scheduler": {
        "type": "WarmupCosineDecay",
        "params": {"warmup_num_steps": 1000, "total_num_steps": 100000}
    }
}
```

---

# 4️⃣ DeepSpeed Training Script

```python
import deepspeed
import argparse

def train():
    parser = argparse.ArgumentParser()
    parser = deepspeed.add_config_arguments(parser)
    args = parser.parse_args()
    
    model = MyLargeModel()
    
    # DeepSpeed handles DDP, optimizer, scheduler
    model_engine, optimizer, _, scheduler = deepspeed.initialize(
        args=args,
        model=model,
        model_parameters=model.parameters(),
    )
    
    for epoch in range(num_epochs):
        for batch in train_loader:
            x, y = batch
            x = x.to(model_engine.device)
            y = y.to(model_engine.device)
            
            loss = model_engine(x, y)
            model_engine.backward(loss)
            model_engine.step()
    
    # Save checkpoint (handles sharded states)
    model_engine.save_checkpoint('./checkpoints', tag='final')

# Launch: deepspeed --num_gpus=8 train.py --deepspeed_config ds_config.json
```

---

# 5️⃣ ZeRO-Offload: Use CPU Memory Too

> 📚 **Research**: Ren et al. (2021) — *"ZeRO-Offload: Democratizing Billion-Scale Model Training"* showed how to leverage CPU memory and compute for training.

When GPU memory isn't enough even with ZeRO-3:

> 🅿️ **Analogy**: CPU offloading is like an **overflow parking lot**. When the main garage (GPU) is full, you park extra cars (tensors) in the lot across the street (CPU RAM). It takes longer to walk there and back, but at least you have somewhere to put them!

```json
{
    "zero_optimization": {
        "stage": 3,
        "offload_optimizer": {
            "device": "cpu",
            "pin_memory": true
        },
        "offload_param": {
            "device": "cpu",
            "pin_memory": true
        }
    }
}
```

This can train a 70B model on a SINGLE A100 (but very slowly).

### 🌌 ZeRO-Infinity: Offload to NVMe Disk Too

> 📚 **Research**: Rajbhandari et al. (2021) — *"ZeRO-Infinity: Breaking the GPU Memory Wall for Extreme Scale Deep Learning"*.

> 💾 **Analogy**: ZeRO-Infinity = Using **both a disk warehouse AND a CPU parking lot** as extra storage. It's like having a warehouse (NVMe SSD), a parking lot (CPU RAM), and the main garage (GPU) — data flows between all three levels as needed.

```json
{
    "zero_optimization": {
        "stage": 3,
        "offload_optimizer": { "device": "nvme", "nvme_path": "/local_nvme" },
        "offload_param": { "device": "nvme", "nvme_path": "/local_nvme" }
    }
}
```

⭐ Enables training **trillion-parameter** models on limited GPU clusters!

---

# 6️⃣ Framework Comparison

| Feature | DDP | FSDP | DeepSpeed | Megatron-LM |
|---------|-----|------|-----------|-------------|
| Optimizer sharding | No | Optional | ZeRO 1-3 | No |
| Parameter sharding | No | Yes | ZeRO 3 | Tensor parallel |
| CPU offloading | No | No | Yes | No |
| Pipeline parallel | No | No | Yes | Yes |
| Tensor parallel | No | No | Limited | Best |
| Ease of use | Easy | Medium | Medium | Hard |
| Best for | <10B | 7-70B | 7-175B | 175B+ |
| Ecosystem | PyTorch | PyTorch | Microsoft | NVIDIA |

---

# 7️⃣ HuggingFace Accelerate: Unified Interface

```python
from accelerate import Accelerator

accelerator = Accelerator(
    mixed_precision='bf16',
    gradient_accumulation_steps=4,
)

model = MyModel()
optimizer = torch.optim.AdamW(model.parameters(), lr=1e-4)
loader = DataLoader(dataset, batch_size=8)

# Accelerate wraps everything
model, optimizer, loader = accelerator.prepare(model, optimizer, loader)

for epoch in range(num_epochs):
    for batch in loader:
        with accelerator.accumulate(model):
            loss = model(**batch)
            accelerator.backward(loss)
            optimizer.step()
            optimizer.zero_grad()

# Works with DDP, FSDP, or DeepSpeed depending on config
# accelerate config → interactive setup
# accelerate launch train.py
```

---

# 8️⃣ Decision Tree for Training Frameworks

```
Model size?
├── <7B: DDP + gradient checkpointing + BF16
├── 7-13B: FSDP or DeepSpeed ZeRO-2/3
├── 13-70B: DeepSpeed ZeRO-3 or FSDP on 8+ GPUs
├── 70-175B: DeepSpeed ZeRO-3 + offloading on 32+ GPUs
└── 175B+: Megatron-LM with tensor + pipeline parallel on 256+ GPUs

Using HuggingFace? → Accelerate (wraps all of the above)
```

---

# 📝 Summary

| Stage | What's Sharded | Memory per GPU |
|-------|---------------|---------------|
| ZeRO-0 (DDP) | Nothing | Full copy |
| ZeRO-1 | Optimizer states | ~65% of DDP |
| ZeRO-2 | + Gradients | ~50% of DDP |
| ZeRO-3 | + Parameters | 1/N of total |
| + Offload | CPU memory used | Even less GPU |

---

# 🏭 Production Scenarios

| Scenario | Recommended Setup | Real-World Example |
|----------|------------------|--------------------|
| Fine-tune 7B model | ZeRO-2 on 4× A100 | LLaMA-2 7B fine-tuning |
| Train 13B from scratch | ZeRO-3 on 8× A100 | Common research setup |
| Train 70B model | ZeRO-3 + offload on 32+ GPUs | LLaMA-2 70B scale |
| Train 100B+ model | ZeRO-3 + Infinity on 64+ GPUs | BLOOM-176B (BigScience) |
| RLHF fine-tuning | DeepSpeed-Chat | ChatGPT-style alignment |

⭐ **Key Production Facts**:
- 🌸 **BLOOM (176B)**: Trained using DeepSpeed ZeRO-3 on 384 A100 GPUs
- 🤖 **ChatGPT**: Microsoft's infrastructure uses DeepSpeed for large-scale training
- 🤗 **HuggingFace Accelerate**: Seamlessly integrates DeepSpeed — just run `accelerate config` and select DeepSpeed as backend
- 💬 **DeepSpeed-Chat**: Purpose-built RLHF pipeline (SFT → Reward Model → PPO) for ChatGPT-like training

---

# 📋 DeepSpeed Cheat Sheet

| What You Want | What To Use | Command / Config |
|---------------|-------------|------------------|
| Basic distributed training | ZeRO Stage 1 | `"stage": 1` |
| Reduce memory ~50% | ZeRO Stage 2 | `"stage": 2` |
| Train 100B+ models | ZeRO Stage 3 | `"stage": 3` |
| GPU memory not enough | CPU Offload | `"offload_param": {"device": "cpu"}` |
| Even CPU RAM not enough | NVMe Offload | `"offload_param": {"device": "nvme"}` |
| Fast mixed precision | BF16 | `"bf16": {"enabled": true}` |
| HuggingFace integration | Accelerate | `accelerate launch --use_deepspeed` |
| RLHF training | DeepSpeed-Chat | `deepspeed chat/training/...` |

📐 **Quick Memory Estimation** — for $\Psi$ parameters on $N_d$ GPUs:

$$\text{ZeRO-3 memory per GPU} \approx \frac{12\Psi}{N_d} \text{ bytes}$$

*Example*: 70B params on 64 GPUs → $\frac{12 \times 70 \times 10^9}{64} \approx 13.1$ GB per GPU ✅

---

# 📚 Resources

### 📄 Papers
- ⭐ Rajbhandari, S. et al. (2020). *"ZeRO: Memory Optimizations Toward Training Trillion Parameter Models"*. SC'20. [arXiv:1910.02054]
- ⭐ Ren, J. et al. (2021). *"ZeRO-Offload: Democratizing Billion-Scale Model Training"*. USENIX ATC'21. [arXiv:2101.06840]
- ⭐ Rajbhandari, S. et al. (2021). *"ZeRO-Infinity: Breaking the GPU Memory Wall for Extreme Scale Deep Learning"*. SC'21. [arXiv:2104.07857]
- Aminabadi, R.Y. et al. (2022). *"DeepSpeed-Inference: Enabling Efficient Inference of Transformer Models at Unprecedented Scale"*. SC'22.
- Yao, Z. et al. (2023). *"DeepSpeed-Chat: Easy, Fast and Affordable RLHF Training of ChatGPT-like Models at All Scales"*. [arXiv:2308.01320]

### 📖 Books
- Tunstall, von Werra & Wolf — *Natural Language Processing with Transformers* (O'Reilly) — Ch. on scaling & distributed training
- Raschka — *Build a Large Language Model (From Scratch)* — practical coverage of DeepSpeed integration
- Lakshmanan, Robinson & Munn — *Machine Learning Design Patterns* (O'Reilly) — distributed training patterns

### 🔗 Documentation & Guides
- DeepSpeed Official Docs: https://www.deepspeed.ai/
- HuggingFace DeepSpeed Integration: https://huggingface.co/docs/accelerate/usage_guides/deepspeed
- DeepSpeed GitHub Examples: https://github.com/microsoft/DeepSpeed/tree/master/examples
- Microsoft DeepSpeed Blog: https://www.microsoft.com/en-us/research/blog/deepspeed/

**Tomorrow**: Pipeline & Tensor Parallelism.
