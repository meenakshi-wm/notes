📘 DAY 29 — Networking & Communication for Distributed AI

Goal:

Understand the networking layer that enables distributed training and inference — NVLink, InfiniBand, NCCL collectives, and how communication overhead affects scaling.

---

## 🌟 Beginner Analogy: Why Networking Matters

> Imagine training an AI model is like a **group project** at work. Each person (GPU) does their part, but they need to **share results** constantly. The speed of sharing depends on **how they're connected**:
>
> - 🏢 **NVLink** = A **direct hallway** between offices — walk right over, super fast!
> - 🚪 **PCIe** = Going through the **building lobby** — slower, everyone shares the path
> - 🛣️ **InfiniBand** = An **express highway** between buildings — fast long-distance travel
> - 🛤️ **Ethernet** = **Regular city streets** — gets you there, but with traffic lights
> - 📬 **RDMA** = Delivery **directly to your desk** (bypasses the reception desk entirely!)
> - 🔀 **NVSwitch** = A **central hub** connecting ALL offices — anyone can reach anyone instantly
>
> ⭐ The faster GPUs can share data, the faster training finishes!

### ⭐ Key Formulas at a Glance

| Formula | Meaning |
|---------|--------|
| $BW = \frac{Data}{Time}$ | **Bandwidth** — how much data flows per second |
| $Latency = t_{propagation} + t_{serialization} + t_{queuing}$ | **Latency** — total delay for one message |
| $T_{AR} = 2(N-1) \frac{M}{N \cdot BW}$ | **Ring AllReduce time** — $N$ = GPUs, $M$ = message size |
| $R_{overhead} = \frac{T_{comm}}{T_{comm} + T_{comp}}$ | **Communication overhead ratio** — lower is better |

> 💡 **Beginner tip**: Bandwidth is like the width of a pipe (how much water flows), latency is how long it takes the first drop to arrive!

---

> ⚠️ **Production Insight**: Network is the **#1 bottleneck** in distributed training. The standard approach: **NVLink for intra-node** (GPUs in same machine) + **InfiniBand for inter-node** (between machines). NVIDIA DGX systems use **NVSwitch** for full-bisection intra-node bandwidth. **Communication-computation overlap** is essential for efficiency.

---

# 1️⃣ Communication Landscape

```
Intra-GPU:        Registers/Shared Memory  ~20 TB/s
Intra-Node:       NVLink 4.0               900 GB/s (H100)
                  PCIe Gen5                128 GB/s (bidirectional)
Inter-Node:       InfiniBand NDR           400 Gbps = 50 GB/s
                  RoCE v2                  200-400 Gbps
                  Ethernet                 100 Gbps = 12.5 GB/s
```

Rule: 10-50x bandwidth drop between intra-node and inter-node.
This is why tensor parallelism stays within a node.

> 🏠 **Beginner Analogy**: Think of it like internet speed — transferring files between folders on your computer (intra-node) is WAY faster than uploading them to a cloud server (inter-node). That's why we keep the most talkative tasks (tensor parallelism) on the same machine!

| Connection Type | Analogy | Bandwidth | Relative Speed |
|----------------|---------|-----------|----------------|
| Registers/Shared Mem | 🧠 Thinking in your head | ~20 TB/s | ⭐⭐⭐⭐⭐ |
| NVLink 4.0 | 🏢 Direct hallway | 900 GB/s | ⭐⭐⭐⭐ |
| PCIe Gen5 | 🚪 Building lobby | 128 GB/s | ⭐⭐⭐ |
| InfiniBand NDR | 🛣️ Express highway | 50 GB/s | ⭐⭐ |
| Ethernet 100G | 🛤️ City streets | 12.5 GB/s | ⭐ |

---

# 2️⃣ NVLink & NVSwitch

NVLink connects GPUs within a node without going through PCIe.

H100 DGX: 8 GPUs connected via NVSwitch (full bisection bandwidth)
Any GPU can talk to any other GPU at 900 GB/s.

```
DGX H100 Topology:
GPU0 ←→ GPU1 ←→ GPU2 ←→ GPU3
  ↕        ↕        ↕        ↕    (NVSwitch provides all-to-all)
GPU4 ←→ GPU5 ←→ GPU6 ←→ GPU7
```

Why it matters: AllReduce across 8 GPUs at 900 GB/s vs 128 GB/s (PCIe) = 7x faster.

> 🔀 **NVSwitch Analogy**: Without NVSwitch, GPUs must pass messages in a chain (like a game of telephone 📞). With NVSwitch, every GPU has a **direct line** to every other GPU — like a **central switchboard operator** connecting any two offices instantly!

> 📚 **Research**: See *NVIDIA NVLink and NVSwitch Whitepapers* for architecture details. The DGX H100 uses 4th-gen NVSwitch providing 900 GB/s per GPU and 7.2 TB/s total bisection bandwidth.

---

# 3️⃣ Collective Communication Operations

> 🎓 **Beginner Analogy**: Imagine 4 students each solved different parts of a homework. **Collectives** are the different ways they can share answers:
> - **AllReduce** = Everyone shares, everyone gets the combined answer ✅
> - **AllGather** = Everyone shares, everyone gets ALL individual answers 📋
> - **ReduceScatter** = Answers are combined, but each student only keeps ONE part 🧩
> - **Broadcast** = One student reads their answer aloud to everyone 📢

### AllReduce
Every GPU contributes data → result (sum/avg) goes to ALL GPUs.
Used by DDP for gradient synchronization.

### AllGather  
Every GPU contributes data → ALL GPUs get ALL data concatenated.
Used by FSDP/ZeRO-3 to reconstruct full parameters.

### ReduceScatter
Every GPU contributes data → sum computed → result SHARDED across GPUs.
Used by FSDP/ZeRO-3 for gradient reduction + sharding.

### Broadcast
One GPU sends data → ALL GPUs receive.

```python
import torch.distributed as dist

# AllReduce example
tensor = torch.randn(1000, device=f'cuda:{rank}')
dist.all_reduce(tensor, op=dist.ReduceOp.SUM)
tensor /= world_size  # Average

# AllGather example
local_tensor = torch.randn(100, device=f'cuda:{rank}')
gathered = [torch.zeros_like(local_tensor) for _ in range(world_size)]
dist.all_gather(gathered, local_tensor)
full_tensor = torch.cat(gathered)  # 100 * world_size elements
```

---

# 4️⃣ NCCL: NVIDIA Collective Communication Library

NCCL implements optimized collectives using ring and tree algorithms:

### Ring AllReduce
```
GPU 0 → GPU 1 → GPU 2 → GPU 3 → GPU 0

Step 1: Each GPU sends 1/N of its data to the next GPU
Step 2: Each GPU receives + adds + forwards
After 2(N-1) steps: all GPUs have the sum

Data transferred per GPU: 2 × (N-1)/N × data_size ≈ 2 × data_size
Independent of N! Optimal for large data.
```

> 📐 **Math**: For ring AllReduce with $N$ GPUs and message size $M$:
>
> $$T_{AllReduce} = 2(N-1) \cdot \frac{M}{N \cdot BW}$$
>
> As $N \to \infty$, each GPU sends $\approx 2M$ regardless of $N$ — this is why ring AllReduce **scales well**! ⭐
>
> **Example**: 8 GPUs, 14 GB gradient, 900 GB/s NVLink:
> $$T_{AR} = 2(8-1) \cdot \frac{14}{8 \times 900} = 14 \times \frac{14}{7200} \approx 27 \text{ ms}$$

### Communication-Computation Overlap
```python
# DDP overlaps backward computation with gradient AllReduce
# As each layer's backward completes -> start AllReduce for that layer
# While GPU computes backward for next layer

# This is automatic in DDP:
model = DistributedDataParallel(model, device_ids=[rank])
# DDP registers backward hooks that trigger AllReduce per bucket
```

---

# 5️⃣ Communication Cost Analysis

```python
def communication_time(data_bytes, bandwidth_gbps, latency_us=5):
    """Estimate communication time."""
    bandwidth_bytes = bandwidth_gbps * 1e9 / 8
    transfer_time = data_bytes / bandwidth_bytes
    total = latency_us * 1e-6 + transfer_time
    return total

# AllReduce 7B gradients (FP16)
data = 7e9 * 2  # 14 GB
t_nvlink = communication_time(data, 900 * 8)   # NVLink (GB/s to Gbps)
t_ib = communication_time(data, 400)            # InfiniBand  
t_eth = communication_time(data, 100)           # Ethernet

print(f"NVLink: {t_nvlink*1000:.1f} ms")  # ~2 ms
print(f"InfiniBand: {t_ib*1000:.1f} ms")  # ~280 ms
print(f"Ethernet: {t_eth*1000:.1f} ms")    # ~1120 ms
```

> 📐 **Communication Overhead Ratio**: Measures how much time is "wasted" on communication:
>
> $$R_{overhead} = \frac{T_{comm}}{T_{comm} + T_{comp}}$$
>
> ⭐ **Goal**: Keep $R_{overhead} < 0.1$ (less than 10% overhead). If it's higher, your GPUs are waiting around instead of computing!
>
> 💡 **Beginner tip**: If your forward+backward takes 500 ms and AllReduce takes 50 ms → $R = \frac{50}{550} \approx 9\%$ ✅ Good!

---

# 6️⃣ Bandwidth vs Latency

| Operation | Dominant Factor |
|-----------|----------------|
| Large AllReduce (gradients) | Bandwidth |
| Small AllReduce (scalar metrics) | Latency |
| Pipeline parallel (activations) | Latency + bandwidth |
| Data loading from network storage | Bandwidth |

For large model training, bandwidth is king.
For serving (many small requests), latency matters more.

> 🚰 **Beginner Analogy**: **Bandwidth** = how wide the pipe is (gallons per minute). **Latency** = how long until the first drop arrives. A fire hose (NVLink) has massive bandwidth. A garden hose from next door (Ethernet) arrives quickly (low latency for small data) but can't fill a pool fast.

> 📬 **RDMA (Remote Direct Memory Access)**: Delivers data **directly to GPU memory**, bypassing the CPU entirely — like a courier who walks straight to your desk instead of going through reception! Used by InfiniBand and RoCE to minimize latency.

---

# 7️⃣ Network Topology for AI Clusters

### Fat-Tree (Most Common)
```
    [Core Switches]
   /    |    |    \
 [Agg] [Agg] [Agg] [Agg]
  / \    / \   / \    / \
[N] [N] [N] [N] [N] [N] [N] [N]
```

### Rail-Optimized (for GPU clusters)
Each GPU connects to own NIC → own switch "rail."
Reduces cross-switch traffic for AllReduce patterns.

> 🏗️ **Production**: Large clusters (Meta's 16K GPU cluster, Google's TPU pods) use **rail-optimized** topologies. Each GPU gets its own network card so AllReduce traffic doesn't bottleneck at shared switches. This is standard for 1000+ GPU clusters.

> 📚 **Research**: See *InfiniBand Trade Association (IBTA)* specifications for RDMA and InfiniBand protocol details. Meta's "[LLaMA infrastructure](https://arxiv.org/abs/2302.13971)" paper describes their networking setup.

---

# 8️⃣ Practical: Monitoring Communication

```python
# Check NCCL environment
import os
os.environ['NCCL_DEBUG'] = 'INFO'  # Verbose NCCL logging

# Benchmark AllReduce
def benchmark_allreduce(size_mb, world_size, rank):
    tensor = torch.randn(int(size_mb * 1e6 / 4), device=f'cuda:{rank}')
    
    # Warmup
    for _ in range(5):
        dist.all_reduce(tensor)
    torch.cuda.synchronize()
    
    import time
    start = time.perf_counter()
    for _ in range(100):
        dist.all_reduce(tensor)
    torch.cuda.synchronize()
    elapsed = time.perf_counter() - start
    
    bandwidth = 2 * size_mb * 100 / elapsed / 1000  # GB/s (ring algorithm)
    if rank == 0:
        print(f"AllReduce {size_mb}MB: {bandwidth:.1f} GB/s")
```

---

# 📝 Summary

| Layer | Technology | Bandwidth |
|-------|-----------|-----------|
| Intra-node GPU-GPU | NVLink | 900 GB/s |
| Intra-node GPU-CPU | PCIe Gen5 | 64 GB/s |
| Inter-node | InfiniBand NDR | 50 GB/s |
| Inter-DC | Ethernet | 12.5 GB/s |

| Collective | Used By | Data Flow |
|-----------|---------|-----------|
| AllReduce | DDP | All → All |
| AllGather | FSDP forward | Shards → Full |
| ReduceScatter | FSDP backward | Full → Shards |

---

# 🧾 Cheat Sheet

| Concept | Key Point | Formula / Rule |
|---------|-----------|----------------|
| Bandwidth | Data throughput per second | $BW = \frac{Data}{Time}$ |
| Latency | Time for first byte to arrive | $L = t_{prop} + t_{serial} + t_{queue}$ |
| Ring AllReduce | Scales well — each GPU sends ~$2M$ | $T_{AR} = 2(N-1)\frac{M}{N \cdot BW}$ |
| Overhead ratio | Keep below 10% | $R = \frac{T_{comm}}{T_{comm}+T_{comp}}$ |
| NVLink | Intra-node GPU↔GPU | 900 GB/s (H100) |
| InfiniBand NDR | Inter-node | 400 Gbps = 50 GB/s |
| NCCL | Optimized GPU collectives | Ring + Tree algorithms |
| RDMA | Bypasses CPU for network I/O | Used by IB & RoCE |
| Tensor Parallelism | Keep intra-node (NVLink) | Bandwidth-sensitive ⭐ |
| Pipeline Parallelism | Can go inter-node (IB) | Latency-sensitive |

---

# 📚 Research & Citations

| Source | Topic | Link |
|--------|-------|------|
| NVIDIA NVLink Whitepaper | NVLink/NVSwitch architecture | [NVIDIA NVLink](https://www.nvidia.com/en-us/data-center/nvlink/) |
| InfiniBand Trade Association | RDMA & IB specs | [IBTA](https://www.infinibandta.org/) |
| NCCL GitHub (NVIDIA) | Collective communication library | [NCCL](https://github.com/NVIDIA/nccl) |
| Touvron et al., 2023 | LLaMA training infrastructure | arXiv:2302.13971 |
| NVIDIA NCCL Developer Guide | Ring/tree algorithms & tuning | [NCCL Docs](https://docs.nvidia.com/deeplearning/nccl/) |

---

# 📖 Resources

### Books 📚
- **"High Performance Datacenter Networks"** — Dennis Abts & John Kim — Architectures, topologies, and routing for AI-scale clusters
- **"Interconnection Networks"** — Jose Duato, Sudhakar Yalamanchili, Lionel Ni — Comprehensive textbook on network design
- **"Programming Massively Parallel Processors"** — David Kirk & Wen-mei Hwu — Covers GPU communication patterns

### Papers 📄
- *Bringing HPC Techniques to Deep Learning* (Gibiansky, 2017) — Ring AllReduce explained
- *Efficient Large-Scale Language Model Training on GPU Clusters Using Megatron-LM* (Narayanan et al., 2021) — Parallelism + networking strategies
- *LLaMA: Open and Efficient Foundation Language Models* (Touvron et al., 2023) — Production training infrastructure

### Online Resources 🌐
- [NVIDIA Networking Docs](https://docs.nvidia.com/networking/) — ConnectX, InfiniBand, NVLink
- [NCCL Documentation](https://docs.nvidia.com/deeplearning/nccl/) — Collective operations, environment variables, tuning
- [PyTorch Distributed Docs](https://pytorch.org/docs/stable/distributed.html) — `torch.distributed` API
- [NVIDIA DGX Architecture](https://www.nvidia.com/en-us/data-center/dgx-platform/) — Full system topology

---

**Tomorrow**: Systems & Hardware Capstone — End-to-End Training Cluster Design.
