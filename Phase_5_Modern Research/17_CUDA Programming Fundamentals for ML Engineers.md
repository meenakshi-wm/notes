📘 DAY 17 — CUDA Programming Fundamentals for ML Engineers

> *"The GPU is not just a faster processor — it's a fundamentally different way of thinking about computation."* — NVIDIA CUDA Programming Guide (2007–present)

Goal:

Understand CUDA programming at the level needed for ML engineering — kernel launches, memory management, and how PyTorch uses CUDA under the hood. Write simple CUDA kernels.

---

## 🌍 Why Should You Care About CUDA? (The Big Picture)

Every time you train a neural network, generate an image with Stable Diffusion, or chat with an LLM — **thousands of tiny computations happen simultaneously on your GPU**. CUDA is the programming language that makes this possible.

> 🏭 **Factory Analogy**: Imagine a CPU as a single brilliant engineer who does tasks one at a time (very fast, very smart). A GPU is a **factory floor with thousands of workers** — each worker is simpler, but together they finish massive jobs in a fraction of the time. CUDA is the **management system** that organizes all those workers.

### 🤔 What Is CUDA, Exactly?

**CUDA** (Compute Unified Device Architecture) is a programming platform created by NVIDIA (2006) that lets you write code that runs on **GPU hardware** instead of the CPU. It's the backbone of modern deep learning.

| Term | Plain English | Analogy 🏭 |
|------|--------------|-------------|
| **Host** | Your CPU + regular RAM | The factory's front office |
| **Device** | Your GPU + GPU memory (VRAM) | The factory floor |
| **Kernel** | A function that runs on the GPU | The instruction sheet given to all workers 📋 |
| **Thread** | One unit of execution on the GPU | One worker 👷 |
| **Block** | A group of threads | A team of workers sharing a room 🏠 |
| **Grid** | All the blocks together | The entire factory floor 🏭 |
| **Warp** | 32 threads that execute in lockstep | 32 workers marching in perfect sync 🎵 |

⭐ **Key Insight**: A GPU doesn't run code "faster" — it runs **thousands of copies of simple code simultaneously**. This is called **parallelism**, and it's why GPUs are perfect for matrix math (which is what neural networks do!).

> 📖 **Research Reference**: NVIDIA introduced CUDA in 2006, fundamentally changing scientific computing. The CUDA Programming Guide (NVIDIA, continuously updated) remains the authoritative reference for GPU programming.

---

# 1️⃣ CUDA Execution Model

## 🧠 The Core Idea (No AI Knowledge Required)

When you have a task like "add 1 million numbers together," a CPU would do it one by one (or maybe 4-8 at a time). A GPU assigns **one thread to each number** and does them all at once!

> 🏭 **Analogy**: You have 1 million envelopes to stamp. The CPU approach: one person stamps them one at a time. The GPU approach: you hire 1 million people, hand each person one envelope, and yell "STAMP NOW!" — they all finish in the time it takes to stamp one envelope.

## 📐 The Hierarchy: Grid → Blocks → Threads

```
CPU (Host) → launches → GPU (Device) Kernel

Grid of Blocks → each Block has Threads
Grid: (gridDim.x, gridDim.y)
Block: (blockDim.x, blockDim.y)

Thread ID = blockIdx.x * blockDim.x + threadIdx.x
```

### 🔍 Breaking Down the Thread ID Formula

The **Global Thread ID** tells each thread which piece of data to work on:

$$\text{Global Thread ID} = \text{blockIdx.x} \times \text{blockDim.x} + \text{threadIdx.x}$$

> 🏭 **Analogy**: Imagine a factory with numbered rooms (blocks), and each room has numbered desks (threads). If each room has 256 desks:
> - Room 0, Desk 5 → Worker ID = $0 \times 256 + 5 = 5$
> - Room 1, Desk 0 → Worker ID = $1 \times 256 + 0 = 256$
> - Room 3, Desk 100 → Worker ID = $3 \times 256 + 100 = 868$

Example: Process 1M elements with 256 threads/block → 3,907 blocks.

$$\text{blocks} = \left\lceil \frac{n}{\text{threads per block}} \right\rceil = \left\lceil \frac{1{,}000{,}000}{256} \right\rceil = 3{,}907$$

### ⚡ Warps: The Secret Execution Unit

A **warp** is a group of exactly 32 threads that the GPU executes **in lockstep** (they all do the same instruction at the same time).

> 🏭 **Analogy**: Think of 32 workers marching in a parade. They all lift their left foot at the same time, then their right foot. If one worker needs to do something different (say, turn left while others go straight), **everyone has to wait** while that worker does their thing. This is called **warp divergence**, and it's why `if/else` statements in GPU code can be slow!

| Concept | Value | Why It Matters |
|---------|-------|----------------|
| Threads per Warp | 32 | Hardware-fixed; all execute same instruction |
| Max Threads per Block | 1024 | Hardware limit on most modern GPUs |
| Typical Threads per Block | 128 or 256 | Sweet spot for occupancy |
| Max Blocks per Grid | $2^{31} - 1$ | Essentially unlimited |

⭐ **Rule of Thumb**: Always make your block size a multiple of 32 (the warp size). Common choices: 128, 256, or 512.

> 📖 **Citation**: The warp-based execution model is detailed in Chapter 4 of the *NVIDIA CUDA C++ Programming Guide* (2024). Understanding warps is essential for writing efficient GPU code.

---

# 2️⃣ Writing a Simple CUDA Kernel

## 🧠 What's a Kernel? (Beginner Explanation)

A **kernel** is just a function that runs on the GPU instead of the CPU. The special part: it runs on **thousands of threads simultaneously**, with each thread knowing its own unique ID.

> 🏭 **Analogy**: A kernel is like an **instruction sheet** you hand out to every worker in the factory. The sheet says: "Look at your worker ID number. Go to shelf #[your ID]. Pick up the item. Do [this operation] on it. Put the result on output shelf #[your ID]." Every worker follows the same instructions but works on different data. This programming model is called **SIMT** (Single Instruction, Multiple Threads).

The `__global__` keyword tells the compiler: *"This function should run on the GPU, but can be called from the CPU."*

```cpp
// vector_add.cu
__global__ void vector_add(float* a, float* b, float* c, int n) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    if (idx < n) {
        c[idx] = a[idx] + b[idx];
    }
}

// Launch
int n = 1000000;
int threads = 256;
int blocks = (n + threads - 1) / threads;
vector_add<<<blocks, threads>>>(a_gpu, b_gpu, c_gpu, n);
```

### 🔍 Line-by-Line Walkthrough

| Line | What It Does | Plain English |
|------|-------------|---------------|
| `__global__` | CUDA keyword | "This function runs on the GPU" |
| `float* a, float* b, float* c` | Pointers to GPU memory | Addresses of data arrays on the GPU |
| `int idx = blockIdx.x * blockDim.x + threadIdx.x` | Calculate unique thread ID | "What's my worker number?" |
| `if (idx < n)` | Bounds check | "Am I within the valid data range?" (some threads have nothing to do) |
| `c[idx] = a[idx] + b[idx]` | The actual work | "Add my two numbers and store the result" |
| `<<<blocks, threads>>>` | Launch configuration | "Send this job to the factory with this many rooms and workers per room" |

### 🧮 Why the `if (idx < n)` Check?

When $n = 1{,}000{,}000$ and threads per block $= 256$:

$$\text{blocks} = \frac{1{,}000{,}000 + 256 - 1}{256} = 3{,}907$$

$$\text{total threads launched} = 3{,}907 \times 256 = 1{,}000{,}192$$

We launch **192 extra threads** that have nothing to do! The `if (idx < n)` check prevents them from writing to invalid memory.

⭐ **This is a fundamental CUDA pattern**: Always check bounds because you almost always launch more threads than you need.

### 🏗️ CUDA Function Qualifiers

| Qualifier | Called From | Runs On | Example Use |
|-----------|-----------|---------|-------------|
| `__global__` | Host (CPU) | Device (GPU) | Kernel functions — your main entry point |
| `__device__` | Device (GPU) | Device (GPU) | Helper functions called by kernels |
| `__host__` | Host (CPU) | Host (CPU) | Regular CPU functions (default) |
| `__host__ __device__` | Both | Both | Utility math functions usable everywhere |

### 🏭 Production Example: Custom Fused ReLU + Bias Kernel

In production ML, you often **fuse** multiple operations into a single kernel to avoid extra memory reads/writes:

```cpp
// Instead of two separate operations: bias_add then relu
// Fuse them into one kernel for 2x memory bandwidth savings
__global__ void fused_relu_bias(float* x, float* bias, float* out, int n, int features) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    if (idx < n) {
        float val = x[idx] + bias[idx % features];  // Add bias
        out[idx] = val > 0.0f ? val : 0.0f;          // ReLU
    }
}
```

> 💡 **Why Fuse?** Each kernel launch reads data from GPU memory (slow!) and writes results back. If you have two kernels, you read/write twice. Fusing means you read once, compute both operations, and write once. This is the core idea behind **Flash Attention** (Dao et al., 2022).

---

# 3️⃣ Using CUDA from Python (Numba/Triton)

## 🧠 Why Python? (Beginner Context)

Most ML engineers don't write raw C++ CUDA code every day. Instead, they use **Python wrappers** that let you write GPU code in a more familiar language. Think of it as ordering through a translator — you speak Python, and the translator converts it to CUDA for the GPU.

### Numba CUDA

> 🏭 **Analogy**: Numba is like a **phrase book** — you write Python, and Numba translates it directly to GPU machine code. It's great for simple kernels.

```python
from numba import cuda
import numpy as np

@cuda.jit
def vector_add_kernel(a, b, c):
    idx = cuda.grid(1)
    if idx < c.shape[0]:
        c[idx] = a[idx] + b[idx]

n = 1_000_000
a = cuda.to_device(np.random.randn(n).astype(np.float32))
b = cuda.to_device(np.random.randn(n).astype(np.float32))
c = cuda.device_array(n, dtype=np.float32)

threads_per_block = 256
blocks = (n + threads_per_block - 1) // threads_per_block
vector_add_kernel[blocks, threads_per_block](a, b, c)
result = c.copy_to_host()
```

| Step | Code | What Happens |
|------|------|-------------|
| 1. Decorate | `@cuda.jit` | Tells Numba "compile this for the GPU" |
| 2. Get ID | `cuda.grid(1)` | Same as the C++ thread ID formula, but Numba does the math for you |
| 3. Upload data | `cuda.to_device(...)` | Copy data from CPU RAM → GPU VRAM 📤 |
| 4. Launch | `kernel[blocks, threads](...)` | Square brackets instead of `<<<>>>`, same idea |
| 5. Download | `.copy_to_host()` | Copy result from GPU VRAM → CPU RAM 📥 |

### 🔺 Triton: The Modern Alternative to CUDA

**Triton** (Tillet et al., 2019) is a Python-based language for writing GPU kernels that is **much easier** than raw CUDA. OpenAI developed it to let ML researchers write custom kernels without needing deep GPU architecture knowledge.

> 🏭 **Analogy**: If CUDA is like building a car from individual parts, Triton is like assembling a car from **pre-built modules**. You still control what the car does, but you don't need to machine every bolt yourself.

```python
import triton
import triton.language as tl
import torch

@triton.jit
def vector_add_triton(a_ptr, b_ptr, c_ptr, n, BLOCK_SIZE: tl.constexpr):
    pid = tl.program_id(0)                         # Which block am I?
    offsets = pid * BLOCK_SIZE + tl.arange(0, BLOCK_SIZE)  # My data indices
    mask = offsets < n                              # Bounds check
    a = tl.load(a_ptr + offsets, mask=mask)         # Load from memory
    b = tl.load(b_ptr + offsets, mask=mask)
    tl.store(c_ptr + offsets, a + b, mask=mask)     # Store result

# Launch
n = 1_000_000
a = torch.randn(n, device='cuda')
b = torch.randn(n, device='cuda')
c = torch.empty(n, device='cuda')
vector_add_triton[(n + 1023) // 1024,](a, b, c, n, BLOCK_SIZE=1024)
```

| Feature | Raw CUDA (C++) | Numba | Triton |
|---------|---------------|-------|--------|
| Language | C++ | Python | Python |
| Difficulty | 🔴 Hard | 🟡 Medium | 🟢 Easy |
| Performance | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| Auto-tuning | ❌ Manual | ❌ Manual | ✅ Built-in |
| Shared memory | ✅ Manual | ✅ Manual | ✅ Automatic |
| Used by | NVIDIA, custom libs | Quick prototyping | OpenAI, PyTorch 2.0+ |

> 📖 **Citation**: Tillet, P., Kung, H.T., & Cox, D. (2019). *Triton: An Intermediate Language and Compiler for Tiled Neural Network Computations*. Proceedings of the 3rd ACM SIGPLAN International Workshop on Machine Learning and Programming Languages.

⭐ **Production Insight**: PyTorch 2.0's `torch.compile` uses Triton under the hood to generate custom GPU kernels automatically. When you call `torch.compile(model)`, PyTorch analyzes your model, generates Triton kernels, and compiles them — all without you writing a single line of CUDA!

### PyTorch Custom CUDA Extension

> 🏗️ **When would you use this?** When you need maximum performance for a custom operation that PyTorch doesn't have built-in (e.g., a novel attention mechanism for your research paper).

```python
# Using torch.utils.cpp_extension for inline CUDA
from torch.utils.cpp_extension import load_inline

cuda_src = """
__global__ void relu_kernel(float* x, float* out, int n) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    if (idx < n) {
        out[idx] = x[idx] > 0 ? x[idx] : 0;
    }
}

torch::Tensor custom_relu(torch::Tensor x) {
    auto out = torch::zeros_like(x);
    int n = x.numel();
    int threads = 256;
    int blocks = (n + threads - 1) / threads;
    relu_kernel<<<blocks, threads>>>(x.data_ptr<float>(), out.data_ptr<float>(), n);
    return out;
}
"""

custom_ops = load_inline(
    name='custom_ops',
    cpp_sources="torch::Tensor custom_relu(torch::Tensor x);",
    cuda_sources=cuda_src,
    functions=['custom_relu']
)
```

### 🏭 Production Scenario: Flash Attention Custom Kernel

The most famous example of custom CUDA kernels in production ML is **Flash Attention** (Dao et al., 2022). Standard attention computes:

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V$$

The naive implementation creates a massive $N \times N$ attention matrix (for sequence length $N$). Flash Attention uses a **custom tiled CUDA kernel** that:
1. Never materializes the full $N \times N$ matrix
2. Uses **shared memory** (the team whiteboard 📋) to keep tiles of Q, K, V close to the compute units
3. Achieves **2-4x speedup** and uses $O(N)$ instead of $O(N^2)$ memory

> 📖 **Citation**: Dao, T., Fu, D., Ermon, S., Rudra, A., & Ré, C. (2022). *FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness*. NeurIPS 2022.

---

# 4️⃣ Memory Management Patterns

## 🧠 GPU Memory Types (Beginner Explanation)

The GPU has several types of memory, each with different speeds and purposes. Understanding these is **critical** for writing fast GPU code.

> 🏭 **Factory Memory Analogy**:
> - **Registers** = Your own pockets 👖 (fastest, tiny, private to each worker)
> - **Shared Memory** = Team whiteboard 📋 (fast, shared by workers in the same room/block)
> - **Global Memory** = Warehouse 🏪 (huge but slow, accessible by everyone)
> - **Constant Memory** = Posted bulletin board 📌 (read-only, fast when everyone reads same value)

| Memory Type | Scope | Speed | Size | Analogy 🏭 |
|------------|-------|-------|------|------------|
| **Registers** | Per thread | ⚡⚡⚡⚡⚡ ~1 cycle | ~255 per thread | Worker's pockets |
| **Shared Memory** | Per block | ⚡⚡⚡⚡ ~5 cycles | 48-164 KB per block | Team whiteboard |
| **L1 Cache** | Per block | ⚡⚡⚡⚡ ~5 cycles | 128-256 KB | Room's reference shelf |
| **L2 Cache** | Per GPU | ⚡⚡⚡ ~50 cycles | 4-50 MB | Floor's library |
| **Global Memory (VRAM)** | Per GPU | ⚡ ~500 cycles | 8-80 GB | Warehouse |
| **Constant Memory** | Per GPU | ⚡⚡⚡⚡ (cached) | 64 KB | Bulletin board |

### 📐 The Memory Bandwidth Problem

The #1 bottleneck in GPU computing is **memory bandwidth** — how fast you can move data between global memory and the compute units:

$$\text{Arithmetic Intensity} = \frac{\text{FLOPs (compute)}}{\text{Bytes moved (memory)}}$$

- If arithmetic intensity is **low** → you're **memory-bound** (waiting for data)
- If arithmetic intensity is **high** → you're **compute-bound** (GPU cores are the bottleneck)

> 💡 Most ML operations (element-wise ops, normalization) are **memory-bound**. This is why kernel fusion matters so much — it reduces memory traffic!

### 🔧 Shared Memory Example

Shared memory is the "team whiteboard" — memory shared by all threads within a block. It's ~100x faster than global memory:

```cpp
__global__ void reduce_sum(float* input, float* output, int n) {
    __shared__ float sdata[256];  // Team whiteboard: 256 floats
    
    int tid = threadIdx.x;
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    
    // Each thread loads one element to shared memory
    sdata[tid] = (idx < n) ? input[idx] : 0.0f;
    __syncthreads();  // 🚦 Wait for ALL threads to finish loading
    
    // Reduction in shared memory (parallel sum)
    for (int s = blockDim.x / 2; s > 0; s >>= 1) {
        if (tid < s) {
            sdata[tid] += sdata[tid + s];
        }
        __syncthreads();  // 🚦 Synchronize after each step
    }
    
    // Thread 0 writes the block's result
    if (tid == 0) output[blockIdx.x] = sdata[0];
}
```

> 🏭 **Analogy**: Imagine 256 workers need to add up 256 numbers. Instead of sending all numbers to the warehouse and back, they write them on the **team whiteboard**. Then pairs of workers add their numbers together, halving the count each round: 256 → 128 → 64 → ... → 1. This is **parallel reduction**.

### 🚦 Synchronization: `__syncthreads()`

`__syncthreads()` is a **barrier** — it makes every thread in the block stop and wait until ALL threads reach that point.

> 🏭 **Analogy**: It's like a teacher saying "Everyone put your pencils down and wait. Nobody moves on to step 2 until everyone finishes step 1." Without this, some fast workers might try to read data that slower workers haven't written yet → **race condition** (bugs!).

### 🔧 PyTorch Memory Management in Practice

```python
import torch

# Explicit GPU memory management
x = torch.randn(1000, 1000, device='cuda')  # Allocate on GPU
y = x.cpu()  # Copy to CPU
z = y.cuda()  # Copy back to GPU

# Memory info
print(f"Allocated: {torch.cuda.memory_allocated() / 1e9:.2f} GB")
print(f"Reserved: {torch.cuda.memory_reserved() / 1e9:.2f} GB")
print(f"Max allocated: {torch.cuda.max_memory_allocated() / 1e9:.2f} GB")

# Free memory
del x
torch.cuda.empty_cache()  # Returns memory to CUDA allocator pool

# Pin memory for faster CPU→GPU transfer
loader = DataLoader(dataset, pin_memory=True, num_workers=4)
```

### 📌 Pinned (Page-Locked) Memory

> 🏭 **Analogy**: Normal CPU memory is like a library where books can be shuffled around (paged to disk). **Pinned memory** puts a "DO NOT MOVE" sticker on the books. The GPU delivery truck 🚛 knows exactly where to find them → **2x faster transfers!**

$$\text{Transfer speedup} \approx 2\times \text{ with pinned memory (PCIe 4.0: 32 GB/s → 25 GB/s usable)}$$

| Memory Type | Speed (CPU→GPU) | When to Use |
|------------|-----------------|-------------|
| Regular (pageable) | Baseline | Default, no setup needed |
| Pinned (`pin_memory=True`) | ~2x faster | DataLoaders, large batch transfers |
| Unified (managed) | Auto-migrates | Prototyping, oversubscribed memory |

⭐ **Production Tip**: Always use `pin_memory=True` in your DataLoader when training on GPU. It's free performance!

---

# 5️⃣ CUDA Streams & Async Execution

## 🧠 What Are Streams? (Beginner Explanation)

A **CUDA stream** is a sequence of GPU operations that execute in order. Think of it as a **conveyor belt** — items on the same belt go in order, but you can have **multiple belts running simultaneously**.

> 🏭 **Analogy**: Imagine a kitchen with one chef (the default stream). Orders come in, and the chef handles them one at a time. Now imagine you have **three chefs** (three streams) — they can all cook different dishes at the same time! As long as two chefs aren't trying to use the same pan, everything works great.

Operations on the same stream execute sequentially. Different streams can overlap.

```python
s1 = torch.cuda.Stream()
s2 = torch.cuda.Stream()

# Default stream
a = torch.randn(1000, 1000, device='cuda')
b = a @ a

# Concurrent operations on different streams
with torch.cuda.stream(s1):
    c = torch.randn(1000, 1000, device='cuda')
    d = c @ c

with torch.cuda.stream(s2):
    e = torch.randn(1000, 1000, device='cuda')
    f = e @ e

# Synchronize
torch.cuda.synchronize()
```

### 🔍 Stream Execution Diagram

```
Time ──────────────────────────────────────────────→

Default Stream: [Create a] → [a @ a]
Stream 1:                     [Create c] → [c @ c]        ← runs concurrently!
Stream 2:                     [Create e] → [e @ e]        ← runs concurrently!
                                                    ↓
                                              synchronize()
```

### 🏭 Production Use: Overlapping Compute and Data Transfer

The most powerful use of streams is **overlapping data transfer with computation**:

```python
# Pipeline: while GPU computes batch N, CPU prepares batch N+1
stream_compute = torch.cuda.Stream()
stream_transfer = torch.cuda.Stream()

for batch_idx in range(num_batches):
    with torch.cuda.stream(stream_transfer):
        next_batch = next_batch_data.cuda(non_blocking=True)  # Transfer
    
    with torch.cuda.stream(stream_compute):
        output = model(current_batch)  # Compute (overlaps with transfer!)
        loss = criterion(output, labels)
    
    current_batch = next_batch
```

⭐ **Key Concept**: `non_blocking=True` tells PyTorch "start this transfer and immediately return — don't wait for it to finish." This is how you overlap compute and data movement.

---

# 6️⃣ Common Performance Pitfalls

## 🧠 Why GPU Code Can Be Slow (Even on a Fast GPU)

The most common mistake beginners make: **the GPU isn't slow — your code is making the GPU wait!** Most performance problems come from unnecessary synchronization, data movement, or underutilizing the hardware.

> 🏭 **Analogy**: Imagine your factory has 10,000 workers, but you keep calling them to the front office (CPU) to ask "hey, what was the result of that last task?" Every time you do that, ALL 10,000 workers stop and wait. That's what `.item()` does!

| Pitfall | Problem | Solution |
|---------|---------|----------|
| CPU-GPU sync | .item(), print(tensor) forces sync | Batch operations, avoid per-step sync |
| Small kernels | Launch overhead dominates | Fuse operations, use larger batches |
| Data transfer | CPU↔GPU copy is slow | Keep data on GPU, use pin_memory |
| Non-contiguous | Triggers memory copy | .contiguous() before ops |
| Python overhead | GIL limits throughput | torch.compile, CUDA graphs |
| Warp divergence | `if/else` causes threads to wait | Minimize branching in hot loops |
| Low occupancy | Not enough threads active | Tune block size, reduce register usage |
| Uncoalesced access | Threads access scattered memory | Access consecutive addresses |

### 🔍 Deep Dive: Memory Coalescing

**Memory coalescing** means threads in a warp access **consecutive** memory addresses. The GPU loads memory in chunks (128 bytes = 32 floats). If all 32 threads in a warp want consecutive floats, the GPU loads **1 chunk**. If they want scattered floats, it might need **32 separate loads** — 32x slower!

> 🏭 **Analogy**: At the warehouse, it's faster to grab 32 items from the same shelf (coalesced) than to run to 32 different aisles (scattered).

```
✅ GOOD (Coalesced): Thread 0 → addr[0], Thread 1 → addr[1], Thread 2 → addr[2]...
❌ BAD  (Strided):   Thread 0 → addr[0], Thread 1 → addr[1000], Thread 2 → addr[2000]...
```

$$\text{Bandwidth utilization} = \frac{\text{Useful bytes}}{\text{Total bytes transferred}} \times 100\%$$

### ⚠️ The Sync Problem in Detail

```python
# BAD: syncs every iteration
for batch in loader:
    loss = model(batch)
    print(loss.item())  # GPU→CPU sync!

# GOOD: accumulate, sync periodically
running_loss = 0
for i, batch in enumerate(loader):
    loss = model(batch)
    running_loss += loss.detach()
    if i % 100 == 0:
        print(f"Loss: {running_loss.item() / 100:.4f}")
        running_loss = 0
```

### 📊 Cost of Common Operations

| Operation | Time | Impact |
|-----------|------|--------|
| Kernel launch overhead | ~5-10 μs | Adds up with many small kernels |
| `tensor.item()` (GPU→CPU sync) | ~50-100 μs | Stalls entire GPU pipeline |
| CPU→GPU memory copy (1 MB) | ~50 μs | PCIe bandwidth limited |
| GPU compute (1M multiply) | ~1 μs | Incredibly fast! |
| Python function call | ~1 μs | Negligible individually, adds up in loops |

⭐ **Golden Rule**: The GPU is a **throughput machine** — keep it busy! Never make it stop and wait for the CPU unless absolutely necessary. Batch your synchronization points.

---

# 7️⃣ torch.compile and CUDA Graphs

## 🧠 What Problem Do These Solve? (Beginner Context)

Every time PyTorch runs an operation, there's overhead: Python interprets the line of code, finds the right GPU kernel, launches it, and returns. For a model with thousands of operations, this overhead adds up. **torch.compile** and **CUDA Graphs** eliminate this overhead in different ways.

> 🏭 **Analogy — torch.compile**: Normally, a factory manager (Python) walks to each worker one at a time with instructions. `torch.compile` is like writing ALL the instructions on one big sheet in advance and handing it to the factory floor — no more trips back and forth. The JIT compiler figures out the optimal order and fuses operations.

> 🏭 **Analogy — CUDA Graphs**: Imagine you record a video 🎥 of the factory doing one job perfectly. Next time you need the same job done, you just press "play" — no need to re-read instructions, re-assign workers, etc. This is exactly what CUDA Graphs do: **record** a sequence of GPU operations once, then **replay** it cheaply.

```python
# torch.compile: JIT compilation for faster execution
model = torch.compile(model)  # PyTorch 2.0+

# CUDA Graphs: record and replay GPU operations
# Eliminates kernel launch overhead for static computation graphs
g = torch.cuda.CUDAGraph()
with torch.cuda.graph(g):
    output = model(static_input)

# Replay (much faster than re-launching kernels)
static_input.copy_(new_data)
g.replay()
```

### 📊 Performance Comparison

| Technique | Typical Speedup | Ease of Use | Limitations |
|-----------|----------------|-------------|-------------|
| Baseline PyTorch (eager) | 1x | ⭐⭐⭐⭐⭐ Easy | Python overhead on every op |
| `torch.compile` | 1.1x–1.8x | ⭐⭐⭐⭐ One-liner | Graph breaks on dynamic shapes |
| CUDA Graphs | 1.5x–3x | ⭐⭐⭐ Moderate | Static shapes only, no control flow |
| Custom CUDA kernels | 2x–10x | ⭐ Hard | Requires CUDA expertise |
| Triton kernels | 1.5x–5x | ⭐⭐⭐ Python-based | Learning curve, but much easier than CUDA |

### 🔍 How torch.compile Works Under the Hood

```
Your PyTorch code
      ↓
[TorchDynamo] — captures Python execution as a computation graph
      ↓
[TorchInductor] — optimizes the graph (fusion, memory planning)
      ↓
[Triton / CUDA] — generates optimized GPU kernels
      ↓
Compiled function (fast!)
```

> 📖 **Citation**: Ansel, J., Yang, E., He, H., et al. (2024). *PyTorch 2: Faster Machine Learning Through Dynamic Python Bytecode Transformation and Graph Compilation*. ASPLOS 2024.

⭐ **Production Tip**: Start with `torch.compile(model)` — it's a free 10-40% speedup for most models. Only go to custom CUDA/Triton kernels when you need more.

---

# 8️⃣ 🔬 Warp-Level Operations (Advanced)

## 🧠 What Are Warp-Level Operations?

Threads within a warp can **directly communicate** without going through shared memory. These are called **warp-level primitives** or **warp shuffle** operations — they're the fastest way for nearby threads to share data.

> 🏭 **Analogy**: Instead of writing messages on the team whiteboard (shared memory), 32 workers standing in a line can just **whisper directly to each other**. It's faster because there's no writing/reading from a board.

### Key Warp-Level Operations

| Operation | CUDA Function | What It Does | Analogy 🏭 |
|-----------|-------------|-------------|------------|
| Shuffle down | `__shfl_down_sync()` | Thread gets value from thread + offset | "Worker 5, what's your number?" asked by Worker 3 |
| Shuffle up | `__shfl_up_sync()` | Thread gets value from thread - offset | Opposite direction |
| Broadcast | `__shfl_sync()` | All threads get value from one thread | "Everyone, Worker 0's number is 42!" |
| Ballot | `__ballot_sync()` | Collect boolean from each thread → bitmask | "Raise your hand if your number > 10" |
| Vote all | `__all_sync()` | True if ALL threads meet condition | "Did everyone finish?" |

```cpp
// Warp-level reduction (sum 32 values without shared memory!)
__device__ float warp_reduce_sum(float val) {
    for (int offset = 16; offset > 0; offset >>= 1) {
        val += __shfl_down_sync(0xffffffff, val, offset);
    }
    return val;  // Thread 0 has the sum
}
```

> 📖 **Citation**: Warp-level primitives were introduced in CUDA 9.0 with the concepts of cooperative groups. See *NVIDIA CUDA C++ Programming Guide*, Chapter 7: Cooperative Groups.

---

# 📝 Summary

| Concept | Key Takeaway | Beginner Analogy 🏭 |
|---------|-------------|---------------------|
| CUDA kernels | Grid of blocks of threads, each thread processes element(s) | Factory → Rooms → Workers, each worker processes one item |
| Memory | Pin memory, keep tensors on GPU, avoid sync | Keep items on the factory floor, not in the warehouse |
| Shared memory | Fast memory shared within a block (~100x faster than global) | Team whiteboard — fast for team coordination |
| Streams | Overlap compute and data transfer | Multiple conveyor belts running in parallel |
| Warps | 32 threads executing in lockstep | 32 workers marching in sync |
| torch.compile | JIT optimization for 10-40% speedup | Pre-writing all instructions on one big sheet |
| CUDA Graphs | Eliminate launch overhead for static graphs | Recording a video and pressing "replay" |
| Triton | Python-friendly GPU kernel writing | Building a car from pre-built modules |

---

# 🧾 CUDA Programming Cheat Sheet

## 🔧 Essential Commands

```bash
# Check GPU info
nvidia-smi                          # GPU status, memory usage, temperature
nvcc --version                      # CUDA compiler version

# Compile CUDA code
nvcc -o vector_add vector_add.cu    # Compile a CUDA file
nvcc -arch=sm_80 -O3 main.cu       # Compile with optimization for A100

# Profile GPU code
nsys profile python train.py        # System-wide profiling
ncu --set full ./my_kernel          # Kernel-level profiling
```

## 📐 Key Formulas

| Formula | LaTeX | When to Use |
|---------|-------|-------------|
| Global Thread ID | $\text{idx} = \text{blockIdx.x} \times \text{blockDim.x} + \text{threadIdx.x}$ | Every kernel |
| Number of Blocks | $\text{blocks} = \lceil n / \text{threads} \rceil$ | Kernel launch config |
| Arithmetic Intensity | $\text{AI} = \text{FLOPs} / \text{Bytes}$ | Performance analysis |
| Memory Bandwidth | $\text{BW} = \text{Bytes} / \text{Time}$ | Benchmarking |
| Occupancy | $\frac{\text{Active warps}}{\text{Max warps per SM}}$ | Tuning block size |

## 🏗️ CUDA Keyword Quick Reference

| Keyword | Meaning |
|---------|---------|
| `__global__` | GPU kernel (called from CPU, runs on GPU) |
| `__device__` | GPU function (called from GPU, runs on GPU) |
| `__host__` | CPU function (default) |
| `__shared__` | Block-level shared memory |
| `__syncthreads()` | Barrier within a block |
| `<<<blocks, threads>>>` | Kernel launch syntax |
| `threadIdx.x` | Thread index within block |
| `blockIdx.x` | Block index within grid |
| `blockDim.x` | Number of threads per block |
| `gridDim.x` | Number of blocks in grid |

## ⚡ PyTorch GPU Quick Reference

```python
# Device management
torch.cuda.is_available()           # Check GPU
torch.cuda.device_count()           # Number of GPUs
torch.cuda.current_device()         # Current GPU index
torch.cuda.get_device_name()        # GPU name (e.g., "NVIDIA A100")

# Memory management  
torch.cuda.memory_allocated()       # Currently allocated bytes
torch.cuda.memory_reserved()        # Total reserved by allocator
torch.cuda.max_memory_allocated()   # Peak usage
torch.cuda.empty_cache()            # Release unused cached memory

# Performance tools
torch.compile(model)                # JIT compile (PyTorch 2.0+)
torch.cuda.synchronize()            # Wait for all GPU ops to complete
torch.cuda.Stream()                 # Create async stream
```

---

# 📚 Resources

## 📖 Books

| Book | Author(s) | Best For |
|------|----------|----------|
| **Programming Massively Parallel Processors** (4th ed.) | Hwu, Kirk, El Hajj (2022) | The definitive CUDA textbook — covers architecture through optimization |
| **CUDA by Example** | Sanders & Kandrot (2010) | Beginner-friendly introduction with practical examples |
| **Professional CUDA C Programming** | Cheng, Grossman, McKercher (2014) | Intermediate to advanced CUDA patterns |
| **GPU Gems 3** | NVIDIA (2007, free online) | Collection of advanced GPU programming techniques |

## 📄 Key Papers

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|-----------------|
| *Triton: An Intermediate Language and Compiler for Tiled Neural Network Computations* | Tillet, Kung, Cox | 2019 | Python-based GPU kernel language — used by PyTorch 2.0 |
| *FlashAttention: Fast and Memory-Efficient Exact Attention* | Dao, Fu, Ermon, Rudra, Ré | 2022 | Custom CUDA kernels that make attention 2-4x faster |
| *FlashAttention-2: Faster Attention with Better Parallelism and Work Partitioning* | Dao | 2023 | Improved Flash Attention leveraging warp-level optimization |
| *PyTorch 2: Faster ML Through Dynamic Python Bytecode Transformation* | Ansel, Yang, He et al. | 2024 | torch.compile and TorchInductor (Triton backend) |

## 🌐 Online Resources

| Resource | Link / Description |
|----------|--------------------|
| **NVIDIA CUDA C++ Programming Guide** | The official reference — start here (docs.nvidia.com/cuda) |
| **NVIDIA CUDA Toolkit Documentation** | API reference, best practices guide, tuning guide |
| **Triton Documentation** | triton-lang.org — tutorials and API reference |
| **PyTorch CUDA Semantics** | pytorch.org/docs — GPU memory management, streams, amp |
| **GPU Mode Discord/YouTube** | Community for GPU programming — lectures, code reviews |
| **CUDA MODE Lecture Series** | YouTube lecture series on practical CUDA programming |
| **Lei Mao's Blog** | High-quality CUDA programming tutorials |

---

**Tomorrow**: Mixed Precision Training — Using FP16/BF16 for 2x Speed.
