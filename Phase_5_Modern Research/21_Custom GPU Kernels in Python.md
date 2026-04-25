📘 DAY 21 — Triton: Custom GPU Kernels in Python

Goal:

Master OpenAI Triton — write high-performance GPU kernels in Python without CUDA C++. Understand the block-based programming model, implement fused operations, and benchmark against PyTorch.

---

## 🌟 Who Is This For?

> **No AI or GPU programming experience needed!** This guide explains custom GPU kernels from scratch. If you've never heard of a "kernel" (in computing, not popcorn 🍿), you're in the right place.

## 🧠 Prerequisites Check

| You Should Know | Why |
|---|---|
| Basic Python | All GPU code here is written in Python |
| What a GPU is | A chip in your computer designed for parallel math |
| Basic math (multiply, add) | We'll do math on thousands of numbers at once |

---

## 🎯 The Big Picture: Why Custom GPU Kernels?

> 🍳 **Analogy: Custom Kernel = Custom Recipe vs. Frozen Meals**
>
> Imagine you're cooking dinner:
> - **Using PyTorch built-in ops** = buying frozen meals 🥶 — convenient, works fine, but you can't customize the seasoning or combine dishes efficiently
> - **Writing a custom GPU kernel** = cooking from scratch with your own recipe 👨‍🍳 — more effort, but you control every ingredient, and you can cook multiple things in one pan!
>
> Custom kernels let you tell the GPU *exactly* what to do, often making things **2-10x faster** than generic operations.

### What Even Is a "Kernel"? 🤔

In GPU computing, a **kernel** is simply a function that runs on the GPU. That's it! When people say "custom GPU kernel," they mean: **a function YOU wrote that runs on the GPU** instead of using someone else's pre-built function.

```
CPU World:  def add(a, b): return a + b       ← runs on your CPU (one calculation at a time)
GPU World:  @gpu_kernel add(a, b): return a + b ← runs on your GPU (thousands of calculations simultaneously!)
```

### Why Not Just Use PyTorch? ⭐

| Scenario | PyTorch Built-in | Custom Kernel |
|----------|-----------------|---------------|
| Standard operations (matmul, relu) | ✅ Great | ❌ Overkill |
| Combining 3+ operations | ❌ Slow (separate memory trips) | ✅ **Fuse into one** |
| Novel activation functions | ❌ Not available | ✅ Write your own |
| Squeezing last 20% performance | ❌ Generic | ✅ Optimized for YOUR use case |
| Research prototyping | ✅ Fast to write | ⚠️ More effort |

---

# 1️⃣ What Is Triton?

Triton is a Python-based language for writing GPU kernels that:
- Compiles to native GPU code (PTX → SASS)
- Handles memory coalescing, shared memory, etc. automatically
- Achieves 80-90% of hand-tuned CUDA performance
- 10x simpler than writing CUDA C++

Used by PyTorch's torch.compile backend and many production systems.

> 🍳 **Analogy: Triton = Cooking With a Recipe Template**
>
> If writing raw CUDA C++ is like cooking from scratch with no recipe (you need to know every technique, every temperature, every timing), then **Triton is like cooking with a detailed recipe template** — it handles the tricky timing and temperature for you, and you just fill in what ingredients (operations) you want!

### ⭐ The Three Ways to Write GPU Kernels in Python

| Tool | Difficulty | Speed | Best For | Analogy |
|------|-----------|-------|----------|--------|
| **CuPy** 🟢 | Easy | Good | NumPy-like GPU ops | Microwave cooking — push a button |
| **Numba CUDA** 🟡 | Medium | Very Good | Thread-level control | Following a recipe step-by-step |
| **Triton** 🟠 | Medium | Excellent | Block-level, fused ops | Recipe template — fill in the blanks |
| **CUDA C++** 🔴 | Hard | Best (if expert) | Maximum control | Professional chef — no recipe needed |

> 🗣️ **Numba Analogy**: Numba is like **speaking broken GPU language with a translator** 🌐. You write Python, Numba translates it to GPU code. It's not as elegant as native CUDA, but it gets the job done without learning a new language!

### CuPy: The Easiest Path 🟢

```python
import cupy as cp

# CuPy: Just like NumPy, but on GPU!
x_gpu = cp.array([1, 2, 3, 4, 5])  # Data lives on GPU
y_gpu = x_gpu * 2 + 1               # Computed on GPU!
print(y_gpu)  # [3, 5, 7, 9, 11]
```

### Numba CUDA: Thread-Level Control 🟡

```python
from numba import cuda
import numpy as np

@cuda.jit  # This decorator says "run this on the GPU!"
def add_kernel_numba(a, b, result):
    # Each thread knows its own index
    idx = cuda.grid(1)  # Which element am I responsible for?
    if idx < a.size:
        result[idx] = a[idx] + b[idx]

# Launch: tell GPU how many threads to use
a = cuda.to_device(np.array([1, 2, 3, 4]))
b = cuda.to_device(np.array([10, 20, 30, 40]))
result = cuda.device_array(4)
add_kernel_numba[1, 4](a, b, result)  # 1 block, 4 threads
print(result.copy_to_host())  # [11, 22, 33, 44]
```

### How GPU Execution Works (The Key Idea) 💡

```
┌─────────────────────────────────────────────────────┐
│                    GPU                               │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │ Block 0 │ │ Block 1 │ │ Block 2 │ │ Block 3 │   │
│  │┌──┬──┬─┐│ │┌──┬──┬─┐│ │┌──┬──┬─┐│ │┌──┬──┬─┐│   │
│  ││T0│T1│T2││ ││T0│T1│T2││ ││T0│T1│T2││ ││T0│T1│T2││   │
│  │└──┴──┴─┘│ │└──┴──┴─┘│ │└──┴──┴─┘│ │└──┴──┴─┘│   │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘   │
│                                                      │
│  Each thread computes ONE element. Thousands run     │
│  simultaneously! 🚀                                  │
└─────────────────────────────────────────────────────┘
```

**Thread indexing formula** ⭐:

$$\text{global\_id} = \text{block\_id} \times \text{block\_size} + \text{thread\_id}$$

> Example: Block 2, Thread 1, Block size 3 → $\text{global\_id} = 2 \times 3 + 1 = 7$ → this thread handles element 7!

---

# 2️⃣ Triton Programming Model

> 🏗️ **Key Insight**: Instead of telling each individual worker (thread) what to do, Triton lets you give instructions to a **team of workers (block)** at once. It's like managing a construction crew — you don't tell each person which brick to lay; you assign sections of the wall to teams!

Instead of programming individual threads (CUDA), Triton programs operate on **blocks** of data:

### CUDA vs. Triton Mental Model

| Aspect | CUDA (Thread-level) | Triton (Block-level) |
|--------|---------------------|---------------------|
| You program | Individual threads | Blocks of data |
| Memory management | Manual (shared memory, coalescing) | Automatic ✨ |
| Complexity | Very high | Medium |
| Performance ceiling | 100% (if expert) | 80-90% |
| Analogy | Directing each actor | Directing scenes |

```python
import triton
import triton.language as tl

@triton.jit
def add_kernel(x_ptr, y_ptr, output_ptr, n_elements, BLOCK_SIZE: tl.constexpr):
    # Each program instance handles one block of BLOCK_SIZE elements
    pid = tl.program_id(0)
    block_start = pid * BLOCK_SIZE
    offsets = block_start + tl.arange(0, BLOCK_SIZE)
    mask = offsets < n_elements
    
    # Load blocks of data
    x = tl.load(x_ptr + offsets, mask=mask)
    y = tl.load(y_ptr + offsets, mask=mask)
    
    # Compute
    output = x + y
    
    # Store result
    tl.store(output_ptr + offsets, output, mask=mask)

# Launch
def add(x, y):
    output = torch.empty_like(x)
    n = x.numel()
    grid = lambda meta: (triton.cdiv(n, meta['BLOCK_SIZE']),)
    add_kernel[grid](x, y, output, n, BLOCK_SIZE=1024)
    return output
```

### 🔍 Line-by-Line Breakdown for Beginners

Let's decode this like we're reading a recipe 📋:

| Line | What It Does | Plain English |
|------|-------------|---------------|
| `@triton.jit` | Decorator | "Hey Triton, compile this for the GPU!" |
| `pid = tl.program_id(0)` | Get block ID | "Which team am I?" (Team 0, 1, 2...) |
| `block_start = pid * BLOCK_SIZE` | Calculate start | "My team starts at element #1024, #2048..." |
| `offsets = block_start + tl.arange(0, BLOCK_SIZE)` | Element indices | "I'm responsible for elements 1024 to 2047" |
| `mask = offsets < n_elements` | Boundary check | "Don't process beyond the end of the array!" |
| `x = tl.load(x_ptr + offsets, mask=mask)` | Load from memory | "Grab my chunk of data from GPU memory" |
| `output = x + y` | Compute | "Do the actual math!" |
| `tl.store(...)` | Save result | "Write my answer back to GPU memory" |
| `grid = lambda meta: (...)` | Grid size | "How many teams do we need total?" |
| `add_kernel[grid](...)` | Launch! | "Go, teams, go!" 🚀 |

> ⭐ **Key Formula**: Number of blocks needed = $\lceil \frac{N}{\text{BLOCK\_SIZE}} \rceil$ where $N$ is total elements

---

# 3️⃣ Fused Softmax (Real Example)

Fusion = combining multiple operations into one kernel → fewer memory round-trips.

> 🚗 **Analogy: Kernel Fusion = Combining Errands Into One Trip**
>
> Without fusion: You drive to the grocery store 🏪, come home, then drive to the pharmacy 🏥, come home, then drive to the bank 🏦, come home. Three round trips!
>
> With fusion: You plan your route — grocery → pharmacy → bank → home. **One trip, all errands done!** 🌟
>
> In GPU terms: each "trip" is reading/writing to slow GPU main memory (HBM). Fusion keeps data in fast registers instead.

### What Is Softmax? (Quick Refresher) 🧠

Softmax converts a row of numbers into probabilities that sum to 1:

$$\text{softmax}(x_i) = \frac{e^{x_i}}{\sum_{j} e^{x_j}}$$

> Example: `[2.0, 1.0, 0.1]` → `[0.659, 0.242, 0.099]` (they sum to 1.0!)
>
> This is used **everywhere** in AI — every time a language model picks the next word, softmax is involved!

```python
@triton.jit
def softmax_kernel(output_ptr, input_ptr, input_row_stride, output_row_stride, n_cols, BLOCK_SIZE: tl.constexpr):
    row_idx = tl.program_id(0)
    row_start_ptr = input_ptr + row_idx * input_row_stride
    
    col_offsets = tl.arange(0, BLOCK_SIZE)
    mask = col_offsets < n_cols
    
    # Load row
    row = tl.load(row_start_ptr + col_offsets, mask=mask, other=-float('inf'))
    
    # Softmax: numerically stable
    row_max = tl.max(row, axis=0)
    row = row - row_max
    numerator = tl.exp(row)
    denominator = tl.sum(numerator, axis=0)
    softmax_output = numerator / denominator
    
    # Store
    output_row_start = output_ptr + row_idx * output_row_stride
    tl.store(output_row_start + col_offsets, softmax_output, mask=mask)

def triton_softmax(x):
    n_rows, n_cols = x.shape
    BLOCK_SIZE = triton.next_power_of_2(n_cols)
    output = torch.empty_like(x)
    softmax_kernel[(n_rows,)](output, x, x.stride(0), output.stride(0), n_cols, BLOCK_SIZE=BLOCK_SIZE)
    return output
```

### Why Is This Faster? 🏁

The fused version does **everything in one kernel**: load row → find max → subtract max → exponentiate → sum → divide → store. The PyTorch version would launch **4-5 separate kernels**, each reading and writing the entire row to slow HBM memory!

| Operation | PyTorch (Unfused) | Triton (Fused) |
|-----------|-------------------|----------------|
| Memory reads from HBM | 4-5 times | 1 time ✅ |
| Memory writes to HBM | 4-5 times | 1 time ✅ |
| Kernel launches | 4-5 | 1 ✅ |
| Overhead | High | Low ✅ |

---

# 4️⃣ Fused Operations: Why They Matter

> 🎯 **This is the #1 reason people write custom GPU kernels!** Memory bandwidth — not compute — is usually the bottleneck.

### 🧠 Understanding the Memory Bottleneck

Modern GPUs can do math **incredibly fast** but reading/writing memory is **slow** in comparison:

```
🧠 GPU Compute:   312 TFLOPS (A100)  ← Can do 312 trillion operations/second!
💾 GPU Memory:    2 TB/s bandwidth   ← Can move "only" 2 trillion bytes/second
```

**Arithmetic Intensity** measures how much math you do per byte of memory accessed:

$$I = \frac{\text{FLOPs}}{\text{Bytes accessed}}$$

> 🍔 **Analogy**: Arithmetic intensity is like "how much cooking you do per grocery trip." If you buy one ingredient per trip (low intensity), you spend all your time driving. If you buy everything at once (high intensity), you spend your time actually cooking!

### ⭐ The Roofline Model

This fundamental model tells you the maximum performance your kernel can achieve:

$$\text{Performance} = \min(\text{Peak FLOPS},\; I \times \text{Bandwidth})$$

Where $I$ is arithmetic intensity. If $I$ is low (memory-bound), you're limited by bandwidth. If $I$ is high (compute-bound), you're limited by peak FLOPS.

```
Performance
    │       ╱──────────── Peak FLOPS (compute-bound)
    │      ╱
    │     ╱
    │    ╱   ← Your kernel is here if memory-bound
    │   ╱
    │  ╱
    │ ╱
    │╱
    └──────────────────>
        Arithmetic Intensity (FLOPs/Byte)
```

### Kernel Fusion: The Math

**Kernel fusion savings**: Reduces memory reads/writes from $2N$ to $N$ (or better)!

For $N$ elements with 3 unfused operations:
- **Unfused**: $3 \times 2N = 6N$ memory operations (each op reads and writes all $N$ elements)
- **Fused**: $N + N = 2N$ memory operations (one read at start, one write at end)
- **Savings**: $\frac{6N - 2N}{6N} = 67\%$ fewer memory operations! 🎉

Without fusion:
```python
# 3 separate kernel launches, 3 memory round-trips
y = x * weight          # kernel 1: load x, weight → compute → store y
y = y + bias            # kernel 2: load y, bias → compute → store y  
y = torch.relu(y)       # kernel 3: load y → compute → store y
# Total: 6 loads + 3 stores from/to HBM
```

With fusion:
```python
@triton.jit
def fused_linear_relu(x_ptr, w_ptr, b_ptr, out_ptr, ...):
    x = tl.load(x_ptr + offsets)
    w = tl.load(w_ptr + offsets)
    b = tl.load(b_ptr + offsets)
    y = tl.maximum(x * w + b, 0)  # All in registers!
    tl.store(out_ptr + offsets, y)
# Total: 3 loads + 1 store. 2.5x fewer memory ops!
```

### 🏢 Production Example: Flash Attention

**Flash Attention** (Dao et al., 2022) is the most famous example of kernel fusion in AI:

| Aspect | Standard Attention | Flash Attention (Custom Kernel) |
|--------|-------------------|--------------------------------|
| Memory complexity | $O(N^2)$ | $O(N)$ 🌟 |
| Speed | 1x | 2-4x faster |
| Sequence length | Limited by GPU memory | Much longer sequences |
| How? | Materializes full attention matrix | **Fused kernel** keeps tiles in fast SRAM |

> ⭐ Flash Attention literally couldn't exist without custom GPU kernels! It's used in virtually every modern large language model.

---

# 5️⃣ Matrix Multiplication in Triton

> 🧩 **Why MatMul?** Matrix multiplication is the **single most important operation** in all of AI. Neural networks are basically giant matrix multiplications! The faster your matmul, the faster your AI.

### The Tiled Approach (How It Works) 💡

Instead of computing the entire result at once, Triton breaks the matrices into **tiles** (small blocks):

```
Matrix A (M×K)          Matrix B (K×N)          Result C (M×N)
┌───┬───┬───┐     ┌───┬───┬───┐     ┌───┬───┬───┐
│ A0│ A1│ A2│     │ B0│ B1│ B2│     │ C0│ C1│ C2│
├───┼───┼───┤  ×  ├───┼───┼───┤  =  ├───┼───┼───┤
│ A3│ A4│ A5│     │ B3│ B4│ B5│     │ C3│ C4│ C5│
└───┴───┴───┘     └───┴───┴───┘     └───┴───┴───┘

Each GPU block computes ONE tile of C.
E.g., Block (0,1) computes C1 by loading tiles from row 0 of A and col 1 of B.
```

```python
@triton.jit
def matmul_kernel(
    a_ptr, b_ptr, c_ptr,
    M, N, K,
    stride_am, stride_ak, stride_bk, stride_bn, stride_cm, stride_cn,
    BLOCK_M: tl.constexpr, BLOCK_N: tl.constexpr, BLOCK_K: tl.constexpr,
):
    pid_m = tl.program_id(0)
    pid_n = tl.program_id(1)
    
    offs_m = pid_m * BLOCK_M + tl.arange(0, BLOCK_M)
    offs_n = pid_n * BLOCK_N + tl.arange(0, BLOCK_N)
    offs_k = tl.arange(0, BLOCK_K)
    
    a_ptrs = a_ptr + offs_m[:, None] * stride_am + offs_k[None, :] * stride_ak
    b_ptrs = b_ptr + offs_k[:, None] * stride_bk + offs_n[None, :] * stride_bn
    
    accumulator = tl.zeros((BLOCK_M, BLOCK_N), dtype=tl.float32)
    
    for k in range(0, K, BLOCK_K):
        a = tl.load(a_ptrs, mask=offs_k[None, :] + k < K)
        b = tl.load(b_ptrs, mask=offs_k[:, None] + k < K)
        accumulator += tl.dot(a, b)
        a_ptrs += BLOCK_K * stride_ak
        b_ptrs += BLOCK_K * stride_bk
    
    c_ptrs = c_ptr + offs_m[:, None] * stride_cm + offs_n[None, :] * stride_cn
    tl.store(c_ptrs, accumulator, mask=(offs_m[:, None] < M) & (offs_n[None, :] < N))
```

### 🔍 Understanding the Loop

The `for k in range(0, K, BLOCK_K)` loop is the key — it **accumulates partial results** tile by tile:

```
Iteration 1: Load tile A[0:64, 0:32] and B[0:32, 0:64] → accumulate
Iteration 2: Load tile A[0:64, 32:64] and B[32:64, 0:64] → accumulate
...until all K elements are processed!
```

> 🧠 This tiled approach is why GPUs can multiply enormous matrices — each block only needs a small piece of memory at a time!

### Memory Coalescing ⭐

> 🏪 **Analogy: Memory Coalescing = Sequential vs. Random Ordering**
>
> Imagine 32 people in a line at a deli counter:
> - **Coalesced** (good): Person 1 orders item 1, person 2 orders item 2, person 3 orders item 3... The clerk just grabs items in order! Fast! ⚡
> - **Uncoalesced** (bad): Person 1 orders item 47, person 2 orders item 3, person 3 orders item 91... The clerk runs all over the store! Slow! 🐢
>
> GPU memory works the same way. When adjacent threads access adjacent memory locations, the GPU fetches data in one efficient burst.

Triton handles memory coalescing **automatically** — this is one of its biggest advantages over raw CUDA!

---

# 6️⃣ Autotuning

> 🎶 **Analogy**: Autotuning is like a musician trying different tunings to find which sounds best. Triton tries different block sizes and picks the fastest one!

### Why Autotuning Matters

The "best" block size depends on your specific GPU, data size, and operation. Instead of guessing, Triton tries multiple configurations and keeps the winner:

Triton can automatically search for best block sizes:

```python
@triton.autotune(
    configs=[
        triton.Config({'BLOCK_M': 128, 'BLOCK_N': 128, 'BLOCK_K': 32}),
        triton.Config({'BLOCK_M': 64, 'BLOCK_N': 128, 'BLOCK_K': 32}),
        triton.Config({'BLOCK_M': 128, 'BLOCK_N': 64, 'BLOCK_K': 64}),
        triton.Config({'BLOCK_M': 64, 'BLOCK_N': 64, 'BLOCK_K': 64}),
    ],
    key=['M', 'N', 'K'],
)
@triton.jit
def matmul_kernel(a_ptr, b_ptr, c_ptr, M, N, K, ...):
    ...
```

### How Autotuning Works 🔧

```
🏃 Run 1: BLOCK_M=128, BLOCK_N=128, BLOCK_K=32 → 2.3ms
🏃 Run 2: BLOCK_M=64,  BLOCK_N=128, BLOCK_K=32 → 1.8ms ⭐ Winner!
🏃 Run 3: BLOCK_M=128, BLOCK_N=64,  BLOCK_K=64 → 2.1ms
🏃 Run 4: BLOCK_M=64,  BLOCK_N=64,  BLOCK_K=64 → 2.5ms

Triton remembers: for M=1024, N=1024, K=512 → use config #2!
```

> ⚠️ **Production note**: Autotuning adds startup time on first run. In production, cache the results so subsequent runs are instant.

---

# 7️⃣ Benchmarking

> 🏁 **Why Benchmark?** "If you can't measure it, you can't improve it." Benchmarking tells you if your custom kernel is actually faster than the built-in version.

### Key Metrics to Measure

| Metric | What It Tells You | Unit |
|--------|-------------------|------|
| **Latency** | How long one call takes | milliseconds (ms) |
| **Throughput** | How much data you can process per second | GB/s or TFLOPS |
| **Speedup** | How much faster vs. baseline | 2x, 3x, etc. |

### Calculating Throughput

For a memory-bound operation like softmax:

$$\text{Throughput (GB/s)} = \frac{2 \times N_{\text{elements}} \times \text{element\_size (bytes)}}{\text{time (seconds)} \times 10^9}$$

The factor of 2 accounts for one read + one write per element.

```python
@triton.testing.perf_report(
    triton.testing.Benchmark(
        x_names=['N'],
        x_vals=[2**i for i in range(8, 15)],
        line_arg='provider',
        line_vals=['triton', 'torch'],
        line_names=['Triton', 'PyTorch'],
        ylabel='GB/s',
        plot_name='softmax-performance',
    )
)
def benchmark(N, provider):
    x = torch.randn(128, N, device='cuda', dtype=torch.float32)
    if provider == 'torch':
        fn = lambda: torch.softmax(x, dim=-1)
    else:
        fn = lambda: triton_softmax(x)
    ms = triton.testing.do_bench(fn)
    gbps = 2 * x.numel() * x.element_size() / ms * 1e-6
    return gbps

benchmark.run(print_data=True, show_plots=True)
```

### 📊 Understanding Benchmark Results

```
     N    |   Triton (GB/s)  |  PyTorch (GB/s)  |  Speedup
  ---------|-----------------|------------------|----------
     256   |     45.2        |     42.1         |   1.07x
    1024   |    180.5        |    120.3         |   1.50x  ⭐
    4096   |    310.2        |    195.8         |   1.58x  ⭐
   16384   |    420.1        |    280.5         |   1.50x  ⭐
```

> 💡 Notice: Triton shines more with larger data! Small data has too much launch overhead for the custom kernel to win big.

### ⭐ Kernel Launch Overhead

Every time you call a GPU kernel, there's a fixed cost (~5-10 microseconds) just to *start* it:

```
|<--- Launch overhead --->|<--- Actual computation --->|
|     ~5-10 µs            |    Depends on data size    |

Small data:  [====OVERHEAD====][=COMPUTE=]        ← Overhead dominates! 👎
Large data:  [=OVERHEAD=][==========COMPUTE==========] ← Compute dominates! 👍
```

This is another reason kernel fusion helps — **one launch instead of three!**

---

# 8️⃣ Vectorized Operations & Why They Matter

> 🚀 **Vectorized** = processing multiple data elements with a single instruction

### Scalar vs. Vectorized

```python
# ❌ Scalar (slow): process one element at a time
for i in range(1000000):
    result[i] = a[i] + b[i]    # 1 million separate add instructions!

# ✅ Vectorized (fast): process chunks at once
result = a + b                  # GPU processes thousands simultaneously!
```

Triton is inherently vectorized because it operates on **blocks** of data. When you write `x + y` in a Triton kernel, it adds an entire block of elements at once!

### The Vector Width Matters

| Operation Type | Elements per Instruction | Relative Speed |
|---------------|------------------------|----------------|
| Scalar (CPU loop) | 1 | 1x |
| SIMD (CPU vectorized) | 4-16 | 4-16x |
| GPU thread | 1 (but thousands of threads) | ~100x |
| GPU block (Triton) | 256-1024 per block | ~1000x+ |

---

# 9️⃣ Production Scenarios: Custom Kernels in the Real World 🏢

### Where Custom Kernels Are Used Today

| Product/System | Custom Kernel | Why |
|---------------|--------------|-----|
| **PyTorch 2.0** (torch.compile) | Triton-generated fused kernels | Automatic fusion of user code |
| **Flash Attention** (Dao et al.) | Hand-written CUDA + Triton | $O(N)$ memory attention |
| **Fused Optimizers** (NVIDIA Apex) | Custom AdamW kernel | Fuse parameter update steps |
| **Custom Activations** (SwiGLU, GeGLU) | Triton kernels | Novel activations not in PyTorch |
| **Quantized Inference** (GPTQ, AWQ) | Custom dequant + matmul | Fuse dequantization with compute |
| **vLLM** (serving framework) | PagedAttention kernel | Efficient KV-cache management |

### ⭐ Production Best Practices

1. **Always benchmark** against PyTorch baseline — custom kernels aren't always faster!
2. **Test edge cases**: empty tensors, non-contiguous memory, unusual shapes
3. **Numerical accuracy**: compare outputs against reference implementation (allow small floating-point differences)
4. **Cache autotuning results** for reproducible performance
5. **Document block size choices** and why they were selected

### When NOT to Write Custom Kernels ❌

| Situation | Better Alternative |
|-----------|-------------------|
| Standard operations | Use PyTorch built-ins |
| Prototyping | Use torch.compile (auto-fusion) |
| Simple elementwise ops | torch.compile handles these well |
| You need it tomorrow | PyTorch is good enough |
| Team can't maintain GPU code | Stick with high-level APIs |

---

# 📝 Summary

| Concept | Key Takeaway |
|---------|--------------|
| Block programming | Operate on blocks, not individual threads |
| Kernel fusion | Combine ops → fewer HBM round-trips |
| Autotuning | Triton searches best block sizes |
| Performance | 80-90% of hand-tuned CUDA |
| Simplicity | 10x simpler than CUDA C++ |
| Arithmetic Intensity | $I = \frac{\text{FLOPs}}{\text{Bytes}}$ — higher is better |
| Roofline Model | $\text{Perf} = \min(\text{Peak FLOPS}, I \times \text{BW})$ |
| Thread Indexing | $\text{global\_id} = \text{block\_id} \times \text{block\_size} + \text{thread\_id}$ |
| Memory Coalescing | Adjacent threads should access adjacent memory |
| Launch Overhead | ~5-10µs per kernel → fuse to reduce launches |

---

# 📚 Cheat Sheet: Custom GPU Kernels in Python

## Quick Reference Table

| Task | CuPy | Numba CUDA | Triton |
|------|------|-----------|--------|
| Install | `pip install cupy` | `pip install numba` | `pip install triton` |
| Decorator | N/A | `@cuda.jit` | `@triton.jit` |
| Thread index | N/A | `cuda.grid(1)` | `tl.program_id(0)` |
| Load data | Automatic | Manual indexing | `tl.load(ptr + offsets)` |
| Store data | Automatic | Manual indexing | `tl.store(ptr + offsets, data)` |
| Memory mgmt | Automatic | Manual | Semi-automatic |
| Best for | Quick GPU ops | Fine-grained control | Fused, high-perf kernels |

## Key Formulas ⭐

| Formula | Expression | When to Use |
|---------|-----------|-------------|
| Arithmetic Intensity | $I = \frac{\text{FLOPs}}{\text{Bytes accessed}}$ | Determine if memory-bound or compute-bound |
| Roofline Model | $\text{Perf} = \min(\text{Peak FLOPS}, I \times \text{BW})$ | Predict max achievable performance |
| Fusion Savings | Memory ops: $2N$ (fused) vs $6N$ (unfused for 3 ops) | Estimate fusion benefit |
| Thread Index | $\text{gid} = \text{bid} \times \text{bsz} + \text{tid}$ | Map threads to data elements |
| Blocks Needed | $\lceil \frac{N}{\text{BLOCK\_SIZE}} \rceil$ | Determine grid size |
| Throughput | $\frac{2 \times N \times \text{elem\_size}}{\text{time} \times 10^9}$ GB/s | Measure memory bandwidth utilization |

## Common Triton Patterns

```python
# Pattern 1: Elementwise operation
@triton.jit
def elementwise_kernel(x_ptr, out_ptr, N, BLOCK: tl.constexpr):
    pid = tl.program_id(0)
    offs = pid * BLOCK + tl.arange(0, BLOCK)
    mask = offs < N
    x = tl.load(x_ptr + offs, mask=mask)
    tl.store(out_ptr + offs, YOUR_OP(x), mask=mask)

# Pattern 2: Row-wise reduction
@triton.jit
def row_reduce_kernel(x_ptr, out_ptr, N_COLS, BLOCK: tl.constexpr):
    row = tl.program_id(0)
    offs = tl.arange(0, BLOCK)
    mask = offs < N_COLS
    x = tl.load(x_ptr + row * N_COLS + offs, mask=mask, other=0)
    result = tl.sum(x, axis=0)  # or tl.max, tl.min
    tl.store(out_ptr + row, result)
```

---

# 🔬 Research Citations

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|------------------|
| *Triton: An Intermediate Language and Compiler for Tiled Neural Network Computations* | Tillet, Kung, Cox | 2019 | Introduced Triton — block-based GPU programming model |
| *FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness* | Dao, Fu, Ermon, Rudra, Ré | 2022 | Showed custom kernels enable $O(N)$ memory attention |
| *PyTorch 2.0: torch.compile and TorchInductor* | PyTorch Team | 2023 | Integrated Triton as backend compiler for automatic kernel generation |
| *Roofline: An Insightful Visual Performance Model* | Williams, Waterman, Patterson | 2009 | Foundational model for understanding compute vs. memory bottlenecks |
| *FlashAttention-2: Faster Attention with Better Parallelism and Work Partitioning* | Dao | 2023 | Improved kernel design achieving 2x over FlashAttention-1 |
| *Numba: A LLVM-based Python JIT Compiler* | Lam, Pitrou, Seibert | 2015 | Enabled GPU programming from Python via CUDA JIT |

---

# 📚 Resources

## 📖 Books

| Book | Author(s) | Best For |
|------|-----------|----------|
| *Programming Massively Parallel Processors* (4th ed.) | Hwu, Kirk, El Hajj | ⭐ **The** GPU programming textbook — covers CUDA from basics to advanced |
| *CUDA by Example* | Sanders, Kandrot | Beginner-friendly CUDA with practical examples |
| *The CUDA Handbook* | Wilt | Deep reference for CUDA performance optimization |
| *Parallel and High Performance Computing* | Robey, Zamora | Broader parallel computing context |

## 📄 Papers

- Tillet et al. (2019) — *Triton: An Intermediate Language and Compiler for Tiled Neural Network Computations* — [arXiv:1907.00587](https://arxiv.org/abs/1907.00587)
- Dao et al. (2022) — *FlashAttention* — [arXiv:2205.14135](https://arxiv.org/abs/2205.14135)
- Dao (2023) — *FlashAttention-2* — [arXiv:2307.08691](https://arxiv.org/abs/2307.08691)

## 🌐 Online Resources

| Resource | Link | Description |
|----------|------|-------------|
| Triton Official Docs & Tutorials | https://triton-lang.org | ⭐ Start here! Excellent tutorials with runnable examples |
| NVIDIA CUDA Programming Guide | https://docs.nvidia.com/cuda/ | Official CUDA reference |
| Numba CUDA Documentation | https://numba.readthedocs.io | Python GPU programming via Numba |
| CuPy Documentation | https://cupy.dev | NumPy-compatible GPU arrays |
| GPU MODE Lecture Series | YouTube: GPU MODE | Community lectures on GPU programming |
| Triton GitHub Repository | https://github.com/triton-lang/triton | Source code + examples |

---

> 🌟 **Key Takeaway for Beginners**: You don't need to write custom GPU kernels for most tasks — PyTorch and torch.compile handle 95% of cases. But understanding how they work gives you **superpowers** for the remaining 5% where performance really matters. Start with CuPy (easiest), graduate to Triton (best balance), and only reach for CUDA C++ if you absolutely need the last drop of performance.

**Tomorrow**: Quantization — Making Models Smaller and Faster.
