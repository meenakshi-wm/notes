📘 DAY 19 — Distributed Training: DDP & FSDP

Goal:

Master multi-GPU training — understand DataParallel vs DistributedDataParallel, implement DDP training, and learn when to use Fully Sharded Data Parallel (FSDP) for large models.

---

## 🌟 Beginner's Overview: What Is Distributed Training?

> **No AI background? Start here!**

Imagine you need to read a 10,000-page book and write a summary. Alone, it takes months. But if you split the pages among 8 friends, each reads their portion, and you all combine notes — you finish in weeks! 📚

**Distributed training** works the same way: instead of one computer (GPU) learning from all the data, you spread the work across *multiple GPUs* so training finishes much faster.

**Key vocabulary for absolute beginners:**

| Term | Plain English 🗣️ | Technical Meaning |
|------|------------------|-------------------|
| **GPU** | A super-fast processor designed for parallel math | Graphics Processing Unit — the hardware that runs training |
| **Model** | The "brain" being trained | A neural network with millions/billions of adjustable numbers (parameters) |
| **Gradient** | Feedback on how wrong the model was | A mathematical direction telling each parameter how to improve |
| **Batch** | A handful of examples the model studies at once | A subset of training data processed in one step |
| **Epoch** | One full pass through all the data | The model sees every example once |
| **Node** | One physical computer | A machine that may contain multiple GPUs |
| **World size** | Total number of workers | Total number of GPU processes participating |
| **Rank** | A worker's ID number | Unique integer $0, 1, \ldots, N-1$ assigned to each GPU process |

---

# 1️⃣ Why Distributed Training?

Single GPU can train a 7B model in ~months. 8 GPUs → ~weeks. 256 GPUs → ~days.

> 🍕 **Analogy**: Making 100 pizzas alone takes all night. With 8 pizza ovens running in parallel, you finish by dinner!

Two dimensions of parallelism:
- **Data Parallel**: Same model on each GPU, different data
- **Model Parallel**: Different parts of model on different GPUs

### ⭐ Effective Batch Size Formula

When you split data across $N$ GPUs, the *effective batch size* seen by the model per step is:

$$B_{\text{eff}} = B_{\text{per\_gpu}} \times N_{\text{GPUs}}$$

> 🎒 **Analogy**: If each of 4 students reads 8 pages per round, the class collectively covers $8 \times 4 = 32$ pages per round.

### ⭐ Linear Scaling Rule (Goyal et al., 2017)

When you increase the effective batch size by $N\times$, you should also scale the learning rate:

$$\text{lr}_{\text{new}} = \text{lr}_{\text{base}} \times N_{\text{GPUs}}$$

**Why?** Larger batches give more stable gradient estimates, so you can take bigger steps without overshooting. This was validated training ImageNet in 1 hour on 256 GPUs (Goyal et al., 2017).

> ⚠️ **Practical tip**: Linear scaling works well up to a point. For very large batch sizes, use *warmup* — start with a small learning rate and gradually ramp up over the first few hundred steps.

### 📊 Training Speed Reference Table

| GPUs | Approx. Time for 7B Model | Speedup vs 1 GPU |
|------|---------------------------|-------------------|
| 1 | ~6 months | 1× |
| 8 | ~3-4 weeks | ~6-7× |
| 64 | ~4-5 days | ~40-50× |
| 256 | ~1-2 days | ~150-200× |

> ⭐ Perfect linear speedup ($S = N$) is the theoretical ideal but communication overhead reduces it in practice.

---

# 2️⃣ DataParallel (DP) vs DistributedDataParallel (DDP)

### 🍳 The Kitchen Analogy

> **DataParallel (DP)** = One head chef 👨‍🍳 telling all the cooks what to do. Every dish comes back to the head chef for tasting before the next round. The head chef becomes the bottleneck — everyone waits on them!
>
> **DDP** = Many chefs 👨‍🍳👩‍🍳👨‍🍳👩‍🍳, each reading the *same recipe* independently. They each cook different dishes, then pass notes around the table so everyone learns. No single bottleneck!

| Feature | DP ❌ | DDP ✅ |
|---------|-----|-----|
| Process model | Single process, multi-thread | Multi-process |
| Communication | GPU 0 aggregates | AllReduce (no bottleneck) |
| GIL limitation | Yes (Python threads) | No (separate processes) |
| Speed | Slower (~60% scaling) | Near-linear scaling |
| Recommendation | Never use | Always use |

### 🔍 Why Is DP So Bad?

1. **Python GIL** 🔒: Python's Global Interpreter Lock means threads can't truly run in parallel. DP uses threads, so it hits this wall.
2. **GPU 0 bottleneck**: All gradients are gathered to GPU 0, averaged there, then broadcast back. GPU 0 does extra work while others sit idle.
3. **Memory imbalance**: GPU 0 uses more memory (it holds all the gathered gradients), so you may have to reduce batch size.

> ⭐ **Production rule**: Always use DDP. PyTorch's own documentation says *"DistributedDataParallel is recommended over DataParallel"* (Li et al., 2020).

---

# 3️⃣ DDP: How It Works

```
GPU 0: model copy + batch₀ → grad₀ ─┐
GPU 1: model copy + batch₁ → grad₁ ─┤  AllReduce
GPU 2: model copy + batch₂ → grad₂ ─┤  (average)
GPU 3: model copy + batch₃ → grad₃ ─┘
                                        ↓
                              avg_grad on ALL GPUs
                                        ↓
                    Each GPU updates its own copy with same avg_grad
                    → All models stay in sync
```

### ⭐ AllReduce: The Core Operation

AllReduce: O(N) data transferred regardless of GPU count (ring algorithm).

> 🎓 **Analogy**: Imagine a group of students who each solve a different part of an exam. Then they *share answers* so that **every student** ends up with the *complete, averaged* solution. That's AllReduce!

Formally, if GPU $i$ has gradient $g_i$, AllReduce computes and distributes the average to *every* GPU:

$$\bar{g} = \frac{1}{N}\sum_{i=1}^{N} g_i$$

After AllReduce, **every GPU has exactly the same $\bar{g}$**, so when each GPU updates its model independently, they all stay perfectly in sync.

### 🔄 Ring-AllReduce Algorithm

> 📝 **Analogy**: Passing notes in a circle! 🔄 Each student passes their partial answer to the next person, who adds their own piece to it. After going around the full circle, everyone has the complete answer.

**How it works step by step:**

1. Arrange $N$ GPUs in a logical ring: GPU 0 → GPU 1 → GPU 2 → ... → GPU $N-1$ → GPU 0
2. Split each gradient tensor into $N$ chunks
3. **Scatter-Reduce phase** ($N-1$ steps): Each GPU sends one chunk to its neighbor and receives one chunk, *adding* (reducing) as it goes. After $N-1$ steps, each GPU holds the fully-reduced version of one chunk.
4. **All-Gather phase** ($N-1$ steps): Each GPU sends its fully-reduced chunk around the ring so everyone gets all chunks.

```
Ring-AllReduce with 4 GPUs:

Step 1: GPU0──chunk→GPU1──chunk→GPU2──chunk→GPU3──chunk→GPU0
Step 2: GPU0──chunk→GPU1──chunk→GPU2──chunk→GPU3──chunk→GPU0
Step 3: GPU0──chunk→GPU1──chunk→GPU2──chunk→GPU3──chunk→GPU0
(After 2×(N-1) = 6 steps, every GPU has the full averaged gradient)
```

**Why Ring-AllReduce is brilliant:**
- Total data sent per GPU: $2 \times \frac{(N-1)}{N} \times D$ where $D$ = gradient size
- This is ≈ $2D$ regardless of $N$ — **communication doesn't blow up with more GPUs!** 🎉
- Popularized by Baidu Research (2017) and used in Horovod framework

### 📦 Gradient Bucketing (DDP Optimization)

> 📬 **Analogy**: Instead of sending one letter at a time to the post office, you *batch your mail* into a big envelope and send it all at once. Much more efficient!

DDP doesn't wait until all gradients are computed to start AllReduce. Instead:
1. Gradients are grouped into **buckets** (default: 25MB each)
2. As soon as a bucket is full, AllReduce starts for that bucket
3. **Computation and communication overlap** — while the GPU is computing gradients for the next layer, the previous layer's gradients are already being synchronized

This overlap is why DDP achieves near-linear scaling!

### 📡 Communication Backends

| Backend | Best For | Protocol |
|---------|----------|----------|
| **NCCL** ⭐ | GPU ↔ GPU (same/different nodes) | NVLink, PCIe, InfiniBand |
| **Gloo** | CPU training, fallback | TCP/IP |
| **MPI** | HPC clusters | Vendor MPI implementations |

> ⭐ **Production rule**: ALWAYS use **NCCL** (NVIDIA Collective Communications Library) for GPU training. It's optimized for NVIDIA hardware and is the default in PyTorch DDP.

---

# 4️⃣ DDP Implementation

```python
import torch
import torch.distributed as dist
from torch.nn.parallel import DistributedDataParallel as DDP
from torch.utils.data.distributed import DistributedSampler
import os

def setup(rank, world_size):
    os.environ['MASTER_ADDR'] = 'localhost'
    os.environ['MASTER_PORT'] = '12355'
    dist.init_process_group("nccl", rank=rank, world_size=world_size)
    torch.cuda.set_device(rank)

def cleanup():
    dist.destroy_process_group()

def train_ddp(rank, world_size, epochs=10):
    setup(rank, world_size)
    device = torch.device(f'cuda:{rank}')
    
    # Model
    model = MyModel().to(device)
    model = DDP(model, device_ids=[rank])
    
    # Data: each GPU gets different subset
    dataset = MyDataset()
    sampler = DistributedSampler(dataset, num_replicas=world_size, rank=rank)
    loader = DataLoader(dataset, batch_size=32, sampler=sampler, num_workers=4, pin_memory=True)
    
    optimizer = torch.optim.AdamW(model.parameters(), lr=1e-4)
    scaler = torch.cuda.amp.GradScaler()
    
    for epoch in range(epochs):
        sampler.set_epoch(epoch)  # Shuffle differently each epoch
        model.train()
        
        for x, y in loader:
            x, y = x.to(device), y.to(device)
            
            with torch.cuda.amp.autocast(dtype=torch.bfloat16):
                loss = model(x, y)
            
            scaler.scale(loss).backward()
            scaler.unscale_(optimizer)
            torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
            scaler.step(optimizer)
            scaler.update()
            optimizer.zero_grad()
        
        # Only rank 0 saves checkpoints
        if rank == 0:
            torch.save(model.module.state_dict(), f'checkpoint_epoch{epoch}.pt')
    
    cleanup()

# Launch
import torch.multiprocessing as mp
world_size = torch.cuda.device_count()
mp.spawn(train_ddp, args=(world_size,), nprocs=world_size)
```

### 🔑 Key Implementation Details for Beginners

| Code Element | What It Does (Plain English) |
|-------------|-----------------------------|
| `dist.init_process_group("nccl")` | Start a group chat between all GPUs using NVIDIA's fast messaging system |
| `DDP(model, device_ids=[rank])` | Wrap the model so gradients are automatically shared across GPUs |
| `DistributedSampler` | Makes sure each GPU gets *different* data (no duplicates!) |
| `sampler.set_epoch(epoch)` | Reshuffles data each epoch so GPUs see fresh combinations |
| `model.module.state_dict()` | Access the original model inside the DDP wrapper for saving |
| `rank == 0` guard | Only one GPU saves checkpoints (avoids 8 GPUs writing the same file!) |
| `pin_memory=True` | Pre-loads data into GPU-friendly memory for faster transfer |
| `GradScaler` + `autocast` | Mixed precision training — uses 16-bit math for speed, 32-bit for stability |

> ⭐ **Production tip**: Always use `model.module` to access the underlying model when saving. The DDP wrapper adds extra structure that you don't want in your saved checkpoint.

---

# 5️⃣ torchrun Launch (Preferred)

> 🚀 **`torchrun`** replaced the older `torch.distributed.launch` command. It handles fault tolerance, elastic scaling, and environment variable setup automatically.

```bash
# Single node, 4 GPUs
torchrun --nproc_per_node=4 train.py

# Multi-node (2 nodes × 8 GPUs each)
# Node 0:
torchrun --nproc_per_node=8 --nnodes=2 --node_rank=0 --master_addr=node0 --master_port=12355 train.py
# Node 1:
torchrun --nproc_per_node=8 --nnodes=2 --node_rank=1 --master_addr=node0 --master_port=12355 train.py
```

```python
# When using torchrun, setup simplifies:
def setup():
    dist.init_process_group("nccl")
    rank = int(os.environ["LOCAL_RANK"])
    torch.cuda.set_device(rank)
    return rank
```

### 📋 torchrun Environment Variables

| Variable | Meaning |
|----------|--------|
| `LOCAL_RANK` | GPU index on this machine (0, 1, 2, ...) |
| `RANK` | Global rank across all nodes |
| `WORLD_SIZE` | Total number of GPU processes |
| `MASTER_ADDR` | IP address of the coordinator node |
| `MASTER_PORT` | Port for coordination |

> 🏭 **Multi-node production setup**: In real clusters (SLURM, Kubernetes), the orchestrator sets `MASTER_ADDR` and `MASTER_PORT` for you. You just call `dist.init_process_group("nccl")` and PyTorch reads the environment variables automatically.

### ⚠️ Common Pitfalls

| Mistake | Symptom | Fix |
|---------|---------|-----|
| Forgetting `sampler.set_epoch(epoch)` | All GPUs see same data order every epoch | Add the call before each epoch's loop |
| Using `model.state_dict()` instead of `model.module.state_dict()` | Checkpoint contains DDP wrapper keys | Always use `.module` |
| Not using NCCL backend for GPUs | Very slow training | Use `"nccl"` (not `"gloo"`) |
| Different random seeds across GPUs | Models diverge | DDP handles this, but be careful with data augmentation |
| Port conflict on `MASTER_PORT` | Connection refused | Pick a free port (e.g., 29500) |

---

# 6️⃣ FSDP: Fully Sharded Data Parallel

For models too large to fit on one GPU even in FP16.

> 📖 **Analogy**: In DDP, every chef has a *complete copy* of the recipe book (wasteful!). In FSDP, each chef holds only *their chapter* of the book. When they need another chapter, they borrow it briefly, use it, then give it back. This way, a single kitchen can handle a recipe book too large for any one chef to carry! 📚

DDP: Each GPU has full model copy → 8 GPUs × 14GB = 112GB wasted on redundancy.
FSDP: Shard model parameters across GPUs → each GPU holds 1/N of the model.

```
FSDP before forward pass of layer L:
1. AllGather: Collect full layer L weights from all GPUs
2. Forward: Compute with full weights
3. Free: Discard non-local shards (keep only 1/N)

FSDP during backward:
1. AllGather: Get full weights again
2. Backward: Compute gradients
3. ReduceScatter: Average and distribute gradient shards
4. Free: Discard non-local shards
```

```python
from torch.distributed.fsdp import FullyShardedDataParallel as FSDP
from torch.distributed.fsdp import ShardingStrategy

model = MyLargeModel().to(device)
model = FSDP(
    model,
    sharding_strategy=ShardingStrategy.FULL_SHARD,
    mixed_precision=MixedPrecision(
        param_dtype=torch.bfloat16,
        reduce_dtype=torch.bfloat16,
        buffer_dtype=torch.bfloat16,
    ),
    auto_wrap_policy=size_based_auto_wrap_policy,
)
```

---

# 7️⃣ When to Use What

| Scenario | Solution |
|----------|----------|
| Model fits on 1 GPU | No parallelism needed |
| Model fits per GPU, want speed | DDP |
| Model barely fits per GPU | DDP + gradient checkpointing |
| Model doesn't fit on 1 GPU | FSDP or tensor parallelism |
| 100B+ parameter model | FSDP + tensor + pipeline parallelism |

---

# 8️⃣ Scaling Efficiency

```python
def compute_scaling_efficiency(single_gpu_throughput, multi_gpu_throughput, num_gpus):
    ideal = single_gpu_throughput * num_gpus
    efficiency = multi_gpu_throughput / ideal * 100
    return efficiency

# Typical scaling (same node):
# 2 GPUs: 95% efficiency
# 4 GPUs: 90% efficiency
# 8 GPUs: 85% efficiency
# 64 GPUs (8 nodes): 70-80% efficiency
# 256 GPUs: 60-75% efficiency
```

### ⭐ Speedup Formula

**Ideal speedup** with $N$ GPUs:

$$S_{\text{ideal}} = N$$

**Actual speedup** accounts for communication overhead:

$$S_{\text{actual}} = \frac{N}{1 + \frac{t_{\text{comm}}}{t_{\text{comp}}}}$$

Where:
- $t_{\text{comm}}$ = time spent synchronizing gradients (communication)
- $t_{\text{comp}}$ = time spent computing forward + backward pass

> 🚗 **Analogy**: If it takes 1 hour to drive ($t_{\text{comp}}$) and 10 minutes to stop for gas ($t_{\text{comm}}$), your effective speed is reduced. With DDP's gradient bucketing overlap, it's like refueling while driving!

The ratio $\frac{t_{\text{comm}}}{t_{\text{comp}}}$ is key:
- **Large models** → lots of computation, small ratio → great scaling ✅
- **Small models** → little computation, large ratio → poor scaling ❌

### 📊 Scaling Efficiency Reference

| Setup | GPUs | Typical Efficiency | Why |
|-------|------|--------------------|-----|
| Single node (NVLink) | 2 | ~95% | Ultra-fast GPU-to-GPU |
| Single node (NVLink) | 8 | ~85-90% | Still very fast |
| Multi-node (InfiniBand) | 64 | ~70-80% | Network adds latency |
| Multi-node (Ethernet) | 64 | ~50-65% | Ethernet is slow for gradients |
| Large cluster | 256 | ~60-75% | Depends on interconnect |

> ⭐ **Production insight**: Interconnect matters HUGELY at scale. Moving from 10Gbps Ethernet to 400Gbps InfiniBand can double your multi-node scaling efficiency.

---

# 📝 Summary

| Method | Memory per GPU | Communication | Best For |
|--------|---------------|---------------|----------|
| DDP | Full model | AllReduce (gradients) | Most training |
| FSDP-SHARD_GRAD | Full model | ReduceScatter (grads) | Gradient memory savings |
| FSDP-FULL_SHARD | 1/N model | AllGather + ReduceScatter | Large model training |

---

# 🧾 DDP Cheat Sheet

| What | How |
|------|-----|
| **Import** | `from torch.nn.parallel import DistributedDataParallel as DDP` |
| **Init** | `dist.init_process_group("nccl")` |
| **Wrap model** | `model = DDP(model, device_ids=[rank])` |
| **Data splitting** | `DistributedSampler(dataset, num_replicas=world_size, rank=rank)` |
| **Shuffle per epoch** | `sampler.set_epoch(epoch)` |
| **Save checkpoint** | `if rank == 0: torch.save(model.module.state_dict(), path)` |
| **Launch** | `torchrun --nproc_per_node=NUM_GPUS train.py` |
| **Backend** | `"nccl"` for GPUs, `"gloo"` for CPU |
| **Effective batch** | $B_{\text{eff}} = B_{\text{per\_gpu}} \times N_{\text{GPUs}}$ |
| **LR scaling** | $\text{lr}_{\text{new}} = \text{lr}_{\text{base}} \times N_{\text{GPUs}}$ |
| **AllReduce** | $\bar{g} = \frac{1}{N}\sum_{i=1}^{N} g_i$ |
| **Actual speedup** | $S = \frac{N}{1 + t_{\text{comm}} / t_{\text{comp}}}$ |

---

# 🔬 Key Research Papers

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|------------------|
| *PyTorch Distributed: Experiences on Accelerating Data Parallel Training* | Li et al. | 2020 | Design and optimization of PyTorch DDP — gradient bucketing, computation-communication overlap |
| *Accurate, Large Minibatch SGD: Training ImageNet in 1 Hour* | Goyal et al. | 2017 | Linear scaling rule for learning rate + warmup strategy for large-batch training |
| *Bringing HPC Techniques to Deep Learning* | Baidu Research | 2017 | Ring-AllReduce applied to deep learning — enables bandwidth-optimal gradient synchronization |
| *Horovod: fast and easy distributed deep learning in TensorFlow* | Sergeev & Del Balso | 2018 | Horovod framework using ring-allreduce for easy multi-GPU training across frameworks |
| *ZeRO: Memory Optimizations Toward Training Trillion Parameter Models* | Rajbhandari et al. | 2020 | Sharded optimizer states — the idea behind FSDP |

---

# 📚 Resources

### Books 📖
- **"Deep Learning" by Goodfellow, Bengio, Courville** — Chapter 12 covers computational considerations for large-scale training
- **"Programming PyTorch for Deep Learning" by Ian Pointer** — Practical guide including distributed training setup
- **"Distributed Machine Learning Patterns" by Yuan Tang** — Dedicated book on distributed training patterns and architectures

### Papers 📄
- [Accurate, Large Minibatch SGD: Training ImageNet in 1 Hour](https://arxiv.org/abs/1706.02677) — Goyal et al., 2017
- [PyTorch Distributed: Experiences on Accelerating Data Parallel Training](https://arxiv.org/abs/2006.15704) — Li et al., 2020
- [Horovod: fast and easy distributed deep learning in TensorFlow](https://arxiv.org/abs/1802.05799) — Sergeev & Del Balso, 2018
- [ZeRO: Memory Optimizations Toward Training Trillion Parameter Models](https://arxiv.org/abs/1910.02054) — Rajbhandari et al., 2020

### Tutorials & Docs 🔗
- [PyTorch DDP Tutorial](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html) — Official step-by-step guide
- [PyTorch FSDP Tutorial](https://pytorch.org/tutorials/intermediate/FSDP_tutorial.html) — Official FSDP guide
- [Horovod Documentation](https://horovod.readthedocs.io/) — Alternative distributed training framework
- [NCCL Documentation](https://docs.nvidia.com/deeplearning/nccl/) — NVIDIA's communication library
- [PyTorch Distributed Overview](https://pytorch.org/docs/stable/distributed.html) — Full API reference

---

**Tomorrow**: Flash Attention & Memory-Efficient Training.
