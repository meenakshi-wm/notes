📘 DAY 16 — GPU Architecture for Deep Learning

Goal:

Understand GPU hardware from the ML practitioner's perspective — why GPUs are essential for deep learning, how they execute computations, and what determines training throughput.

---

> 🧭 **Who is this for?** Anyone curious about *why* deep learning needs special hardware. No prior AI or hardware knowledge required. We'll build every concept from scratch with analogies, visuals, and real-world scenarios.

> ⭐ **The Big Picture**: Deep learning is just mountains of simple math (mostly multiplying huge tables of numbers). Regular processors (CPUs) are like 16 genius professors who each solve complex problems one at a time. GPUs are like an army of 10,000 simple factory workers who each do one tiny multiplication — but they all work *at the same time*. When you have billions of tiny multiplications, the army wins every time. 🏭

---

# 1️⃣ Why GPUs for Deep Learning?

## 🔑 The Core Idea (Beginner Analogy)

> 🧠 **Analogy — Restaurant Kitchen**:
> - **CPU** = A kitchen with **8 master chefs** 👨‍🍳. Each chef can make any dish perfectly — sushi, pasta, soufflé — but there are only 8 of them. Great for a fancy tasting menu.
> - **GPU** = A kitchen with **10,000 line cooks** 🧑‍🍳. Each cook can only do ONE thing — chop an onion, flip a burger — but there are *thousands* working simultaneously. Great for feeding a stadium.
>
> Deep learning is like feeding a stadium. You don't need 8 geniuses. You need 10,000 workers all chopping onions at the same time.

CPU: 8-64 cores, optimized for latency (single-task fast)
GPU: 1000s of cores, optimized for throughput (many tasks in parallel)

Deep learning = massive matrix multiplications = embarrassingly parallel.

### 📐 What is a Matrix Multiplication?

If you've never seen one — a **matrix** is just a table (grid) of numbers. Multiplying two matrices means doing lots of multiply-and-add operations. A 1000×1000 matrix multiply requires **2 billion** arithmetic operations. Neural networks do this thousands of times per training step.

$$C_{ij} = \sum_{k=1}^{n} A_{ik} \cdot B_{kj}$$

Each element $C_{ij}$ can be computed **independently** — that's why GPUs are perfect. Each of those 10,000 workers computes one element of $C$.

### 💡 Why "Embarrassingly Parallel"?

This is an actual computer science term! It means the problem is *so easy to split up* that it's almost embarrassing. Each multiplication is independent — no worker needs to wait for another worker's result.

A single training step of GPT-3: ~$3.6 \times 10^{17}$ FLOPS. At 100 TFLOPS per GPU, that's ~3600 GPU-seconds.

> ⭐ **FLOPS** = Floating Point Operations Per Second — how many decimal-number calculations the chip can do each second. **TFLOPS** = Trillion FLOPS ($10^{12}$).

| Property | CPU (Intel Xeon) | GPU (NVIDIA H100) |
|----------|-----------------|-------------------|
| 🧮 Cores | 8–64 complex cores | 16,896 simple CUDA cores |
| ⚡ Clock Speed | ~3.5 GHz | ~1.8 GHz |
| 🔧 Best at | Complex logic, branching | Simple parallel math |
| 🎯 Design Philosophy | Minimize latency per task | Maximize throughput overall |
| 💰 Cost | ~$5,000 | ~$30,000 |
| 🧠 Each Core Can... | Run complex programs | Do basic arithmetic |

> 📖 **Research Context**: The GPU revolution in AI started with Krizhevsky, Sutskever & Hinton (2012), "ImageNet Classification with Deep Convolutional Neural Networks" (AlexNet) — the first major demonstration that GPUs could dramatically accelerate deep learning, training ~6× faster than CPUs.

---

# 2️⃣ NVIDIA GPU Architecture

> 🧠 **Analogy — A City**: Think of a GPU as a **city** 🏙️:
> - The **GPU chip** = the entire city
> - **Streaming Multiprocessors (SMs)** = individual **factories** in the city (132 factories in H100)
> - **CUDA cores** inside each SM = **workers** in each factory (128 workers per factory)
> - **Tensor Cores** inside each SM = **specialized robots** that do matrix math 10× faster than regular workers
> - **Shared Memory** = a **shelf** inside each factory (fast to grab from)
> - **L2 Cache** = a **warehouse** shared by all factories
> - **HBM (Global Memory)** = the city's huge **shipping port** (lots of storage, but far away)

### Key Components

```
GPU Chip (e.g., H100)
├── Streaming Multiprocessors (SMs) × 132
│   ├── CUDA Cores (FP32) × 128 per SM = 16,896 total
│   ├── Tensor Cores × 4 per SM = 528 total
│   ├── Register File (256KB per SM)
│   └── Shared Memory / L1 Cache (256KB per SM)
├── L2 Cache (50MB)
├── HBM3 Memory (80GB, 3.35 TB/s bandwidth)
└── NVLink / PCIe interconnect
```

### 🔍 Breaking Down Each Component (Beginner-Friendly)

| Component | What It Is | Analogy | Why It Matters |
|-----------|-----------|---------|---------------|
| **CUDA Core** | A tiny processor that does one math operation | 🧑‍🍳 A line cook | More cores = more parallel math |
| **Tensor Core** | Hardware designed specifically for matrix multiply | 🤖 A robot arm on an assembly line | 15–30× faster than CUDA cores for matrix math |
| **SM** | A cluster of cores + local memory | 🏭 A factory with workers and a shelf | The basic building block of a GPU |
| **Register File** | Fastest, tiniest memory right next to each core | ✋ Your own hands (instant access) | Zero delay — data is RIGHT THERE |
| **Shared Memory** | Fast memory shared within one SM | 📦 A shelf next to the workbench | ~20 cycles to access |
| **L2 Cache** | Medium memory shared across ALL SMs | 🏬 A central warehouse | ~200 cycles to access |
| **HBM** | Huge main memory (80GB) | 🚢 The city's shipping port | Lots of space, but ~400 cycles to access |
| **NVLink** | High-speed link between GPUs | 🛤️ An express railroad between cities | Needed for multi-GPU training |

### Execution Hierarchy

Thread → Warp (32 threads) → Block → Grid

All 32 threads in a warp execute the SAME instruction (SIMT).
Divergent branches → both paths executed → wasted cycles.

### 🧵 What Does This Mean in Plain English?

> 🧠 **Analogy — Marching Band**:
> - A **Thread** = one musician 🎵
> - A **Warp** (32 threads) = a **row of 32 musicians** marching in lockstep. They ALL play the same note at the same time.
> - A **Block** = a **section** of the band (e.g., all trumpets)
> - A **Grid** = the **entire marching band** 🎺
>
> **SIMT** (Single Instruction, Multiple Threads) means all 32 musicians in a row play the SAME note. If you tell some to play a C and others a D — the band has to play C first, THEN D. That's **warp divergence**, and it wastes time.

> 📖 **Reference**: NVIDIA H100 Tensor Core GPU Architecture Whitepaper (2022) — details the Hopper architecture with 132 SMs, 4th-gen Tensor Cores, and the Transformer Engine.

> ⭐ **Production Tip**: When writing CUDA kernels or choosing batch sizes, always aim for multiples of 32 (the warp size). Odd batch sizes cause warp divergence and waste GPU cycles.

---

# 3️⃣ Memory Hierarchy

> 🧠 **Analogy — Getting Ingredients in a Kitchen** 🍳:
> Imagine you're a chef. Where you keep your ingredients determines how fast you can cook:
>
> | Where | Kitchen Analogy | Speed |
> |-------|----------------|-------|
> | **Registers** | 🤲 Ingredients **in your hands** | **Instant** — already holding it |
> | **Shared Memory (SRAM)** | 📦 Ingredients on the **counter next to you** | **Very fast** — just reach over |
> | **L2 Cache** | 🏬 Ingredients in the **pantry down the hall** | **Moderate** — quick walk |
> | **HBM (Global Memory)** | 🏭 Ingredients in a **warehouse across town** | **Slow** — need a truck |
> | **CPU RAM** | 🌍 Ingredients in **another city** | **Very slow** — need a delivery |
> | **NVMe SSD** | 🌎 Ingredients on **another continent** | **Extremely slow** |
>
> The art of GPU programming is keeping data as close to the "hands" as possible! 🎯

| Level | Size | Bandwidth | Latency |
|-------|------|-----------|---------|
| Registers | 256KB/SM | ~20 TB/s | 0 cycles |
| Shared Memory | 256KB/SM | ~19 TB/s | ~20 cycles |
| L2 Cache | 50MB | ~12 TB/s | ~200 cycles |
| HBM (Global) | 80GB | 3.35 TB/s | ~400 cycles |
| CPU RAM | 512GB+ | 50 GB/s | 1000s cycles |
| NVMe SSD | TBs | 7 GB/s | millions |

The GPU's main bottleneck is often memory bandwidth, not compute. This is the "memory wall."

### 🛣️ What is "Bandwidth"? (Beginner Explanation)

> 🧠 **Analogy — Highway** 🛣️:
> - **Bandwidth** = the **width of a highway** (how many lanes it has). A wider highway can move more cars per second.
> - **Latency** = the **distance** you have to drive. Even on a 10-lane highway, if the warehouse is 100 miles away, it still takes a while.
>
> **HBM3 bandwidth = 3.35 TB/s** means the highway between the GPU's main memory and its cores can move 3.35 *trillion* bytes every second. That sounds like a lot, but the GPU can *compute* even faster — so it's often sitting idle, waiting for data to arrive. 😤

**Bandwidth** is measured in **bytes per second** (B/s). Think of it as:

$$\text{Bandwidth} = \frac{\text{Amount of data moved}}{\text{Time to move it}}$$

### 🧱 The Memory Wall Problem

> ⭐ **Key Insight**: Modern GPUs can compute ~1000 TFLOPS but can only *feed data* at ~3.35 TB/s. That means for every byte of data loaded from HBM, the GPU needs to do at least ~300 arithmetic operations to stay busy. If your operation does fewer than ~300 operations per byte loaded, the GPU sits idle waiting for data. This is the **memory wall**.

### 🔑 HBM, SRAM, and Registers — What Are These?

| Memory Type | Full Name | 🏗️ What It Is | 💡 Key Fact |
|-------------|-----------|---------------|------------|
| **Registers** | — | Tiny storage cells built into each core | Fastest possible, but only ~256KB per SM |
| **SRAM** | Static Random Access Memory | Fast on-chip memory (shared memory + caches) | Expensive to make, uses 6 transistors per bit |
| **HBM** | High Bandwidth Memory | 3D-stacked DRAM chips on the GPU package | Huge capacity (80GB), connected by thousands of wires |
| **DRAM** | Dynamic Random Access Memory | Standard computer memory (CPU RAM) | Cheap and large, but far from GPU cores |

> 📖 **Research Context**: The memory wall is the central motivation behind Dao et al. (2022), "FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness." FlashAttention redesigns the attention algorithm to minimize HBM reads/writes by keeping data in SRAM, achieving 2–4× speedup on real workloads — a landmark GPU-aware algorithm design.

---

# 4️⃣ Tensor Cores

> 🧠 **Analogy — Specialized Assembly Line** 🏭:
> Imagine a car factory. **CUDA cores** are like general-purpose workers — they can weld, paint, or assemble any part, but they do it one step at a time. **Tensor Cores** are like a **specialized robot arm** that's been purpose-built to do ONE thing: attach wheels. It can attach 4 wheels simultaneously in one motion, while a regular worker would do them one by one.
>
> For matrix multiplication (the "wheel-attaching" of deep learning), Tensor Cores are **15–30× faster** than regular CUDA cores. 🚀

Specialized hardware for matrix multiply-accumulate:

$$D = A \times B + C$$

Where $A$, $B$ are FP16/BF16 and $D$ can be FP32.

### 🔢 What Do These Number Formats Mean?

| Format | Full Name | Bits | Precision | Use Case |
|--------|-----------|------|-----------|----------|
| **FP32** | 32-bit Floating Point | 32 | High (7 decimal digits) | Training (traditional) |
| **FP16** | 16-bit Floating Point | 16 | Medium (3.3 digits) | Training with mixed precision |
| **BF16** | Brain Float 16 | 16 | Medium (same range as FP32, less precision) | Training (preferred for stability) |
| **FP8** | 8-bit Floating Point | 8 | Low | Inference (H100+) |
| **INT8** | 8-bit Integer | 8 | Very low | Inference quantization |

> ⭐ **Why Use Lower Precision?** Using FP16 instead of FP32 means each number takes **half the space**. This means: (1) You can fit **2× more data** in memory, (2) You move data **2× faster** through the bandwidth highway, (3) Tensor Cores can process it **much faster**. The small loss in precision rarely hurts model quality!

One Tensor Core: $4 \times 4 \times 4$ matrix multiply per cycle.
H100 Tensor Cores: 989 TFLOPS (FP16), 1979 TFLOPS (FP8).

Without Tensor Cores (CUDA FP32): 67 TFLOPS.
**Tensor Cores = 15-30x speedup for matrix ops.**

### 📊 Tensor Core Speedup Visualization

```
CUDA Cores (FP32):  ████████ 67 TFLOPS
Tensor Cores (FP16): ████████████████████████████████████████████████████████████████████████████████████ 989 TFLOPS
Tensor Cores (FP8):  ██████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████ 1979 TFLOPS
```

> ⭐ **Production Tip**: Always use `torch.cuda.amp` (Automatic Mixed Precision) in PyTorch to automatically leverage Tensor Cores. This single change can speed up training by 2–3× with minimal code changes:
> ```python
> scaler = torch.cuda.amp.GradScaler()
> with torch.cuda.amp.autocast():
>     output = model(input)
>     loss = criterion(output, target)
> ```

> 📖 **Reference**: Micikevicius et al. (2018), "Mixed Precision Training" — the foundational paper showing that FP16 training with loss scaling matches FP32 quality while dramatically improving throughput.

---

# 5️⃣ GPU Comparison for ML

> 🧠 **Analogy — Choosing a Truck for Delivery** 🚚:
> - **RTX 3090/4090** = a pickup truck. Great for personal use, small loads (fine-tuning, small models, experimentation).
> - **A100** = a delivery van. Built for business. Can handle serious cargo (training medium-large models).
> - **H100** = a semi-truck. Maximum capacity and speed for the heaviest loads (training LLMs, massive inference).
> - **H200** = a *bigger* semi-truck. Same engine as H100, but with a much larger cargo bed (141GB of faster memory).

| GPU | VRAM | BW | FP16 TFLOPS | Price |
|-----|------|------|-------------|-------|
| RTX 3090 | 24GB | 936 GB/s | 71 | $1,500 |
| RTX 4090 | 24GB | 1,008 GB/s | 165 | $1,600 |
| A100 80GB | 80GB | 2,039 GB/s | 312 | $15,000 |
| H100 80GB | 80GB | 3,350 GB/s | 989 | $30,000 |
| H200 | 141GB | 4,800 GB/s | 989 | $35,000 |

For training LLMs: VRAM and memory bandwidth matter most.
For inference: VRAM (= max model size) and latency.

### 🏗️ GPU Generations — The Evolution (Beginner Timeline)

| Generation | Architecture | Year | Key Innovation | Flagship |
|-----------|-------------|------|----------------|----------|
| Pascal | P100 | 2016 | First GPU widely used for DL | Tesla P100 |
| Volta | V100 | 2017 | ⭐ **Invented Tensor Cores** | Tesla V100 |
| Ampere | A100 | 2020 | 3rd-gen Tensor Cores, TF32, MIG | A100 80GB |
| Hopper | H100 | 2022 | 4th-gen Tensor Cores, FP8, Transformer Engine | H100 80GB |
| Blackwell | B200 | 2024 | 5th-gen Tensor Cores, FP4, 2nd-gen Transformer Engine | B200 |

> ⭐ **The V100 was the turning point** — it introduced Tensor Cores in 2017, which made matrix multiplication 5–12× faster overnight. Every GPU generation since has improved Tensor Cores further.

### 🏭 Production Decision Guide: Which GPU for What?

| Task | Recommended GPU | Why | Monthly Cloud Cost (approx.) |
|------|----------------|-----|------------------------------|
| 🧪 Experimentation / Hobby | RTX 4090 | Cheap, 24GB enough for small models | $0.40/hr (Lambda) |
| 📚 Fine-tuning 7B models | A100 40GB | Enough VRAM, good throughput | $1.10/hr (AWS) |
| 🏋️ Training 13B–70B models | A100 80GB × 4-8 | Need multi-GPU for large models | $4.50/hr × 4-8 |
| 🚀 Training 100B+ models | H100 × 8+ | Maximum throughput, NVLink | $8.50/hr × 8+ |
| ⚡ Inference (low latency) | H100 / H200 | FP8 Tensor Cores, large VRAM | $3.50/hr |
| 💰 Budget inference | RTX 4090 / A10G | Good perf/dollar for serving | $0.40–$1.00/hr |

> 💡 **Cloud GPU Providers**: AWS (p4d/p5 instances), Google Cloud (A3/A2 VMs), Azure (ND series), Lambda Labs, CoreWeave, RunPod, vast.ai. Spot/preemptible instances can save 60–70% but may be interrupted.

### 💾 GPU Memory Management (Production Essential)

> ⭐ **The #1 beginner error**: `CUDA out of memory` 💥. This means your model + data + optimizer states don't fit in GPU VRAM. Here's where memory goes:

| Memory Consumer | Size (7B param model, FP16) | Why |
|----------------|----------------------------|-----|
| Model weights | ~14 GB | 7B × 2 bytes (FP16) |
| Optimizer states (Adam) | ~28 GB | 2 copies × FP32 (momentum + variance) |
| Gradients | ~14 GB | Same size as weights |
| Activations | ~10–30 GB | Depends on batch size & sequence length |
| **Total** | **~66–86 GB** | Barely fits on A100 80GB! |

> 🧠 **That's why fine-tuning a 7B model needs 80GB!** The model itself is only 14GB, but all the training overhead triples the memory. Techniques like **gradient checkpointing**, **LoRA**, and **DeepSpeed ZeRO** exist specifically to solve this.

> 📖 **Reference**: Rajbhandari et al. (2020), "ZeRO: Memory Optimizations Toward Training Trillion Parameter Models" — partitions optimizer states, gradients, and parameters across GPUs to dramatically reduce per-GPU memory.

---

# 6️⃣ Arithmetic Intensity & Roofline Model

> 🧠 **Analogy — Factory Workers and Delivery Trucks** 🏭🚚:
> Imagine your factory workers (GPU cores) can assemble 1000 widgets per hour. But the delivery truck (memory bandwidth) can only bring 100 boxes of parts per hour.
>
> - If each box of parts makes **1 widget** → workers build 100 widgets/hr, then sit idle waiting for more parts. **The truck is the bottleneck** = **Memory-Bound** 🚚❌
> - If each box of parts makes **100 widgets** → workers are busy non-stop building 1000 widgets/hr. **Workers are the bottleneck** = **Compute-Bound** 🧑‍🏭❌
>
> **Arithmetic Intensity** tells you how many widgets you can make per box of parts = how many FLOPs you can do per byte of data loaded.

### 📐 The Math (Don't Worry, It's Simple!)

$$\text{Arithmetic Intensity (AI)} = \frac{\text{FLOPs (computation)}}{\text{Bytes moved from memory}}$$

**Machine Balance Point** (where memory and compute are equally fast):

$$\text{Machine Balance} = \frac{\text{Peak FLOPS}}{\text{Peak Bandwidth}}$$

For H100: $\frac{989 \text{ TFLOPS}}{3.35 \text{ TB/s}} \approx 295 \text{ FLOPs/byte}$

**Decision rule:**
- If $\text{AI} < \text{Machine Balance}$ → **Memory-bound** 🚚 (GPU starving for data)
- If $\text{AI} > \text{Machine Balance}$ → **Compute-bound** 🧑‍🏭 (GPU cores maxed out)

Arithmetic Intensity (AI) = FLOPs / Bytes moved

If AI < machine's compute-to-bandwidth ratio → memory-bound
If AI > ratio → compute-bound

### 📊 Common Operations — Are They Memory-Bound or Compute-Bound?

| Operation | Arithmetic Intensity | Bound | Explanation |
|-----------|---------------------|-------|-------------|
| Element-wise ops (ReLU, add) | ~1 FLOP/byte | 🚚 Memory | Load number, do 1 operation, store back |
| LayerNorm / Softmax | ~5-10 FLOPs/byte | 🚚 Memory | Multiple passes over data, few FLOPs |
| Self-Attention | ~10-50 FLOPs/byte | 🚚 Memory (usually) | Depends on sequence length |
| Matrix Multiply (large) | ~300+ FLOPs/byte | 🧑‍🏭 Compute | Lots of FLOPs per byte loaded |
| Convolution (large) | ~200+ FLOPs/byte | 🧑‍🏭 Compute | Similar to matmul |

> ⭐ **Key Insight**: Most operations in a Transformer are **memory-bound**, NOT compute-bound! Only the big matrix multiplications (the linear layers) are compute-bound. This is why FlashAttention (which reduces memory movement) is so impactful.

### 📈 The Roofline Model (Visual Explanation)

The **Roofline Model** is a simple graph that shows your GPU's performance ceiling:

```
Performance   ^
(TFLOPS)      |         _______________  ← Peak Compute (989 TFLOPS)
              |        /
              |       /   ← Slope = Peak Bandwidth (3.35 TB/s)
              |      /
              |     /
              |    /
              |   /
              |  /
              | /
              |/
              +──────────────────────────>
              0     Arithmetic Intensity (FLOPs/byte)
                         ↑
                   Machine Balance Point (~295)
```

- **Left of the knee** (low AI): You're under the sloped roof. Memory-bound. Performance = AI × Bandwidth.
- **Right of the knee** (high AI): You're under the flat roof. Compute-bound. Performance = Peak FLOPS.
- **The goal**: Get your operations to the RIGHT of the knee! 🎯

```python
def roofline_analysis(flops, bytes_accessed, peak_flops, peak_bandwidth):
    ai = flops / bytes_accessed
    machine_balance = peak_flops / peak_bandwidth
    
    if ai < machine_balance:
        achieved = ai * peak_bandwidth  # memory-bound
        bottleneck = "memory bandwidth"
    else:
        achieved = peak_flops  # compute-bound
        bottleneck = "compute"
    
    utilization = achieved / peak_flops * 100
    return achieved, bottleneck, utilization

# Example: Matrix multiply 4096x4096x4096 in FP16
flops = 2 * 4096**3  # ~137B FLOPs
bytes = 3 * 4096**2 * 2  # A, B, C matrices in FP16
achieved, bottleneck, util = roofline_analysis(flops, bytes, 989e12, 3.35e12)
# AI = 2730. This is compute-bound. Good!
```

> 💡 **Production Example**: If your training is slow, profile it!
> - If GPU utilization is low (< 50%) → likely **memory-bound**. Fix: increase batch size, use FlashAttention, fuse operations.
> - If GPU utilization is high (> 90%) → likely **compute-bound**. Fix: use mixed precision (FP16/BF16), get a faster GPU.

> 📖 **Research Context**: Williams, Waterman & Patterson (2009), "Roofline: An Insightful Visual Performance Model for Multicore Architectures" — the original roofline paper. Still the go-to framework for understanding GPU performance bottlenecks.

---

# 7️⃣ Practical GPU Profiling

> 🧠 **What is Profiling?** It's like putting a **stopwatch** ⏱️ on every part of your code to find out where it's slow. Just like a doctor diagnoses before prescribing, you should **profile before optimizing**.

```python
import torch
import time

def benchmark_matmul(size=4096, dtype=torch.float16):
    device = 'cuda'
    a = torch.randn(size, size, dtype=dtype, device=device)
    b = torch.randn(size, size, dtype=dtype, device=device)
    
    # Warmup
    for _ in range(10):
        c = a @ b
    torch.cuda.synchronize()
    
    # Benchmark
    start = time.perf_counter()
    for _ in range(100):
        c = a @ b
    torch.cuda.synchronize()
    elapsed = time.perf_counter() - start
    
    flops = 2 * size**3 * 100
    tflops = flops / elapsed / 1e12
    print(f"Size {size}: {tflops:.1f} TFLOPS ({dtype})")
    return tflops

# Compare dtypes
benchmark_matmul(4096, torch.float32)   # Without Tensor Cores
benchmark_matmul(4096, torch.float16)   # With Tensor Cores
benchmark_matmul(4096, torch.bfloat16)  # With Tensor Cores
```

### 🔍 Understanding the Benchmark Code (Line by Line for Beginners)

| Line | What It Does | Why |
|------|-------------|-----|
| `torch.randn(size, size, ...)` | Creates a random matrix on the GPU | We need data to multiply |
| `device='cuda'` | Puts the data on the GPU (not CPU) | CPU matrices won't use GPU cores |
| Warmup loop (10 iterations) | "Warms up" the GPU | First runs are slow due to initialization |
| `torch.cuda.synchronize()` | Waits for GPU to finish | GPU runs **asynchronously** — without this, timing is wrong |
| `2 * size**3` | Formula for FLOPs in a matrix multiply | A matmul of $n \times n$ matrices = $2n^3$ FLOPs |
| `tflops = flops / elapsed / 1e12` | Converts to TFLOPS | Divides by $10^{12}$ to get trillions |

> ⭐ **What to Expect**: On an H100, you should see roughly:
> - FP32 (no Tensor Cores): ~60 TFLOPS
> - FP16 (Tensor Cores): ~800+ TFLOPS
> - BF16 (Tensor Cores): ~800+ TFLOPS
>
> If your numbers are much lower, something is wrong (wrong dtype, small matrices, CPU bottleneck).

### 🛠️ Production Profiling Tools

| Tool | What It Does | When to Use |
|------|-------------|-------------|
| `torch.profiler` | PyTorch's built-in GPU profiler | First stop for any performance issue |
| `nvidia-smi` | Shows GPU utilization, memory, temperature | Quick health check |
| `nsys` (Nsight Systems) | Timeline view of CPU/GPU activity | Finding idle gaps and bottlenecks |
| `ncu` (Nsight Compute) | Deep kernel-level analysis | Optimizing individual CUDA kernels |
| `torch.cuda.max_memory_allocated()` | Peak GPU memory usage in PyTorch | Debugging OOM errors |

---

# 📝 Summary

| Concept | Key Takeaway |
|---------|-------------|
| GPU vs CPU | 1000s of cores for parallel matrix ops |
| Tensor Cores | 15-30x speedup for matmul in FP16/BF16 |
| Memory hierarchy | HBM bandwidth is the common bottleneck |
| Arithmetic intensity | Determines if you're memory or compute bound |
| VRAM | Limits max model size that fits in one GPU |

---

# 🧾 Cheat Sheet — GPU Architecture at a Glance

```
┌─────────────────────────────────────────────────────────────────────┐
│                    GPU ARCHITECTURE CHEAT SHEET                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  🧮 GPU = 1000s of simple cores (CUDA) + specialized cores (Tensor) │
│  🧠 CPU = few complex cores. GPU = many simple cores.               │
│                                                                     │
│  📦 MEMORY HIERARCHY (fastest → slowest):                           │
│     Registers → Shared Memory (SRAM) → L2 Cache → HBM → CPU RAM    │
│     🤲 hands   📦 counter       🏬 pantry  🏭 warehouse  🌍 other city │
│                                                                     │
│  ⚡ TENSOR CORES: Specialized for matrix multiply.                  │
│     FP32 (no TC): ~67 TFLOPS │ FP16 (TC): ~989 TFLOPS = 15× faster │
│                                                                     │
│  🛣️ BANDWIDTH = highway width. LATENCY = highway length.            │
│     H100 HBM: 3.35 TB/s bandwidth, ~400 cycle latency              │
│                                                                     │
│  📊 ARITHMETIC INTENSITY = FLOPs / Bytes moved                      │
│     Low AI → memory-bound 🚚  │  High AI → compute-bound 🧑‍🏭        │
│     H100 balance point: ~295 FLOPs/byte                             │
│                                                                     │
│  🎯 RULES OF THUMB:                                                 │
│     • Batch size = multiple of 32 (warp size)                       │
│     • Always use mixed precision (FP16/BF16)                        │
│     • Profile before optimizing                                     │
│     • Memory-bound? → FlashAttention, kernel fusion                 │
│     • Compute-bound? → Better hardware, lower precision             │
│     • OOM? → Gradient checkpointing, LoRA, DeepSpeed ZeRO          │
│                                                                     │
│  💰 GPU SELECTION:                                                  │
│     Hobby: RTX 4090 │ Fine-tune: A100 │ Train LLM: H100 × 8       │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

# 📚 Resources

### 📖 Books
| Book | Author(s) | Why Read It |
|------|-----------|-------------|
| *Programming Massively Parallel Processors* (4th ed.) | David Kirk & Wen-mei Hwu | **The** textbook on GPU programming. Covers CUDA from basics to advanced. |
| *CUDA by Example* | Jason Sanders & Edward Kandrot | Hands-on introduction to CUDA with practical examples |
| *Computer Architecture: A Quantitative Approach* (6th ed.) | Hennessy & Patterson | Deep dive into memory hierarchies, bandwidth, and the roofline model |
| *Deep Learning* (Ch. 12: Applications) | Goodfellow, Bengio & Courville | Covers hardware considerations for deep learning workloads |

### 📄 Key Papers
| Paper | Year | Key Contribution |
|-------|------|-----------------|
| Krizhevsky, Sutskever & Hinton — "ImageNet Classification with Deep CNNs" | 2012 | Proved GPUs could revolutionize deep learning (AlexNet) |
| Micikevicius et al. — "Mixed Precision Training" | 2018 | Showed FP16 + loss scaling matches FP32 quality |
| Williams, Waterman & Patterson — "Roofline: An Insightful Visual Performance Model" | 2009 | The roofline model for understanding compute vs memory bounds |
| Dao et al. — "FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness" | 2022 | GPU-aware algorithm design, 2–4× attention speedup by minimizing HBM I/O |
| Rajbhandari et al. — "ZeRO: Memory Optimizations Toward Training Trillion Parameter Models" | 2020 | Partitioning optimizer/gradient/parameter memory across GPUs |
| NVIDIA — "H100 Tensor Core GPU Architecture" (Whitepaper) | 2022 | Official technical deep-dive into Hopper architecture |
| NVIDIA — "A100 Tensor Core GPU Architecture" (Whitepaper) | 2020 | Official Ampere architecture reference |

### 🔗 Online Resources
| Resource | Link | Description |
|----------|------|-------------|
| NVIDIA CUDA Programming Guide | developer.nvidia.com/cuda-toolkit | Official CUDA documentation and tutorials |
| NVIDIA GPU Benchmarks (MLPerf) | mlcommons.org/benchmarks | Standardized ML hardware benchmarks |
| Lambda Labs GPU Benchmark | lambdalabs.com/gpu-benchmarks | Practical deep learning GPU comparisons |
| Horace He — "Making Deep Learning Go Brrrr" (Blog) | horace.io | Excellent intuitive explanation of GPU performance |
| NVIDIA Developer Blog | developer.nvidia.com/blog | Latest GPU optimization techniques |
| PyTorch Performance Tuning Guide | pytorch.org/tutorials | Official guide to maximizing PyTorch GPU performance |

---

**Tomorrow**: CUDA Programming Fundamentals for ML Engineers.
