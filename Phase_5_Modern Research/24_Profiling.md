📘 DAY 24 — Profiling & Debugging GPU Training

Goal:

Master profiling tools for GPU training — identify bottlenecks, measure GPU utilization, debug CUDA OOM errors, and optimize training throughput.

---

# 🌟 Beginner's Big Picture: What Is Profiling?

> 🏥 **Analogy: Profiling = A Doctor's Checkup for Your AI Model**
>
> When you feel sick, you don't just guess — you go to a doctor who runs tests (blood pressure, X-ray, etc.) to find out *exactly* what's wrong. **Profiling is the same thing for your code.** It runs "diagnostic tests" on your training to find where time and memory are being wasted.

**Why should you care?** Most untrained practitioners throw money at bigger GPUs when their code is only using 20% of the hardware they already have. Profiling shows you the truth.

| 🤔 Without Profiling | ✅ With Profiling |
|---|---|
| "Training is slow... maybe I need a bigger GPU?" | "Data loading takes 70% of my time — I'll fix that first" |
| Guessing and hoping | Measuring and knowing |
| Wasting $$$$ on cloud GPUs | Squeezing maximum value from hardware |

⭐ **Golden Rule**: *Always profile BEFORE optimizing.* Never guess where the bottleneck is.

---

# 📐 Key Mathematical Formulas

### GPU Utilization

$$U = \frac{t_{compute}}{t_{compute} + t_{idle} + t_{transfer}}$$

where $t_{compute}$ = time GPU spends computing, $t_{idle}$ = time GPU is waiting (e.g., for data), $t_{transfer}$ = time moving data between CPU ↔ GPU.

> 🎯 Goal: Get $U$ as close to $1.0$ (100%) as possible.

### Throughput

$$\text{Throughput} = \frac{\text{samples (or tokens) processed}}{\text{wall-clock time}}$$

Higher = better. This is your primary speed metric.

### Amdahl's Law ⭐

$$S = \frac{1}{(1 - p) + \frac{p}{N}}$$

where $S$ = speedup, $p$ = fraction of work that is parallelizable, $N$ = number of parallel processors.

> 🏭 **Analogy**: If 90% of a factory's work can use robots ($p=0.9$) and you add $N=10$ robots, your max speedup is $S = \frac{1}{0.1 + 0.09} \approx 5.3\times$ — **not** $10\times$! The non-parallel 10% limits you.

### Arithmetic Intensity

$$I = \frac{\text{FLOPs}}{\text{Bytes transferred to/from memory}}$$

This tells you whether your code is **compute-bound** ($I$ is high → good for GPUs) or **memory-bound** ($I$ is low → GPU sits idle waiting for data).

> 📊 The **Roofline Model** (Williams et al., 2009) uses arithmetic intensity to visualize whether you're limited by compute or memory bandwidth.

---

# 1️⃣ Why Profile?

Typical unoptimized training wastes 50-80% of GPU capacity:
- Data loading is slow (GPU waits for data)
- Small batch sizes (GPU underutilized)
- Python overhead (GIL, eager execution)
- Unnecessary CPU-GPU synchronization

Profiling tells you WHERE the time goes.

> 🏭 **Analogy: Bottleneck = The Slowest Worker on an Assembly Line**
>
> Imagine a car factory with 5 stations. If Station 3 takes 10 minutes but every other station takes 2 minutes, the whole line produces one car every 10 minutes — no matter how fast the others are. **That's a bottleneck.** Profiling finds your "Station 3."

⭐ **Production Tip**: The #1 most common bottleneck in real training jobs is **data loading**, not the model itself. Always check this first!

| 🔍 Common Bottleneck | 📍 Where It Hides | 🛠️ Quick Fix |
|---|---|---|
| Slow data loading | CPU → GPU pipeline | `num_workers=4+`, `pin_memory=True` |
| CPU-GPU transfer overhead | `.to(device)` calls | Batch transfers, `non_blocking=True` |
| Python GIL overhead | Eager execution | `torch.compile()` |
| Tiny CUDA kernels | Unoptimized ops | Operator fusion, `torch.compile()` |

---

# 2️⃣ Quick GPU Monitoring

> 📹 **Analogy: GPU Timeline = A Security Camera**
>
> `nvidia-smi` is like checking the security camera feed for your GPU. It shows you a snapshot of what's happening *right now*: Is the GPU busy? How much memory is used? Is it overheating? The profiler (Section 3) is like *recording* the full footage so you can play it back in slow motion.

```bash
# nvidia-smi: basic GPU status
nvidia-smi  # One-shot
watch -n 1 nvidia-smi  # Live monitoring

# Key metrics:
# GPU-Util: Should be >90% during training
# Memory-Usage: How much VRAM is used
# Temp: Keep below 80°C for sustained work
# Power: Near TDP = GPU is working hard
```

```python
# From Python
import subprocess
def gpu_stats():
    result = subprocess.run(['nvidia-smi', '--query-gpu=utilization.gpu,memory.used,memory.total,temperature.gpu',
                            '--format=csv,noheader,nounits'], capture_output=True, text=True)
    print(result.stdout)
```



```python
from torch.profiler import profile, record_function, ProfilerActivity

# Profile a training step
with profile(
    activities=[ProfilerActivity.CPU, ProfilerActivity.CUDA],
    record_shapes=True,
    profile_memory=True,
    with_stack=True,
) as prof:
    with record_function("training_step"):
        output = model(input_data)
        loss = criterion(output, target)
        loss.backward()
        optimizer.step()

# Print summary
print(prof.key_averages().table(sort_by="cuda_time_total", row_limit=20))

# Export for visualization
---

# 3️⃣ PyTorch Profiler

⭐ **This is your most important profiling tool.** `torch.profiler` records every CPU and GPU operation, how long each took, and how much memory was used.

> Think of it as attaching a stopwatch to every single step your model performs.

---

# 4️⃣ Profiler with TensorBoard

```python
from torch.profiler import profile, schedule, tensorboard_trace_handler

# Profile multiple steps with warmup
with profile(
    activities=[ProfilerActivity.CPU, ProfilerActivity.CUDA],
    schedule=schedule(wait=1, warmup=1, active=3, repeat=2),
    on_trace_ready=tensorboard_trace_handler('./logs/profile'),
    record_shapes=True,
    profile_memory=True,
    with_stack=True,
) as prof:
    for step, (x, y) in enumerate(train_loader):
        if step >= 10:
            break
        output = model(x.cuda())
        loss = criterion(output, y.cuda())
        loss.backward()
        optimizer.step()
        optimizer.zero_grad()
        prof.step()

---

# 5️⃣ Debugging CUDA Out of Memory (OOM)

> 💳 **Analogy: Memory Profiling = Your Bank Statement**
>
> Your GPU has a fixed VRAM "budget" (e.g., 24 GB on an RTX 4090). Every tensor you create is a "purchase." An OOM error means you've overspent. Memory profiling shows you your "statement" — exactly which tensors ate up your budget.



```python
# Step 1: Find peak memory usage
torch.cuda.reset_peak_memory_stats()
# ... run training step ...
peak = torch.cuda.max_memory_allocated() / 1e9
print(f"Peak memory: {peak:.2f} GB")

# Step 2: Memory snapshot for detailed analysis
torch.cuda.memory._record_memory_history(max_entries=100000)
# ... run training step ...
torch.cuda.memory._dump_snapshot("memory_snapshot.pickle")
# Visualize at: pytorch.org/memory_viz

# Step 3: Common OOM solutions
solutions = {
    "Reduce batch size": "Most direct fix",
    "Gradient checkpointing": "model.gradient_checkpointing_enable()",
    "Mixed precision": "torch.cuda.amp.autocast(dtype=torch.bfloat16)",
    "Gradient accumulation": "Accumulate grads over micro-batches",
    "Clear cache": "torch.cuda.empty_cache() between steps",
    "Reduce sequence length": "If variable-length, pad less",
    "Use FSDP": "Shard model across GPUs",
}
```

---

# 6️⃣ Measuring Training Throughput

```python
import time

class ThroughputTracker:
    def __init__(self):
        self.start_time = None
        self.total_tokens = 0
        self.total_samples = 0
    
    def start(self):
        torch.cuda.synchronize()
        self.start_time = time.perf_counter()
    
    def update(self, batch_size, seq_len):
        self.total_tokens += batch_size * seq_len
        self.total_samples += batch_size
    
    def report(self):
        torch.cuda.synchronize()
        elapsed = time.perf_counter() - self.start_time
        tokens_per_sec = self.total_tokens / elapsed
        samples_per_sec = self.total_samples / elapsed
        
        print(f"Throughput: {tokens_per_sec:,.0f} tokens/sec")
        print(f"           {samples_per_sec:.1f} samples/sec")
        print(f"           {tokens_per_sec * 6 * num_params / 1e12:.1f} TFLOPS (approx MFU)")
        return tokens_per_sec

# Usage
tracker = ThroughputTracker()
tracker.start()
for step, batch in enumerate(loader):
    # ... training step ...
    tracker.update(batch.shape[0], batch.shape[1])
    if step == 100:
        break
tracker.report()
```

---

# 7️⃣ Model FLOPS Utilization (MFU)

> 🎯 **What is MFU?** It answers: "What percentage of my GPU's *theoretical maximum* computing power am I actually using?" An MFU of 50% means half the GPU's potential is being wasted.
>
> $$\text{MFU} = \frac{\text{Achieved FLOP/s}}{\text{Peak FLOP/s}} = \frac{6 \cdot N_{params} \cdot \text{tokens/sec}}{\text{GPU Peak FLOP/s}}$$

```python
def compute_mfu(tokens_per_sec, num_params, gpu_flops_peak):
    """
    MFU = achieved FLOPs / peak FLOPs
    For transformer: ~6 * N * tokens_per_sec FLOPs (fwd + bwd)
    """
    achieved_flops = 6 * num_params * tokens_per_sec
    mfu = achieved_flops / gpu_flops_peak
    return mfu

# Example: 7B model, 50K tokens/sec, H100
mfu = compute_mfu(50000, 7e9, 989e12)
print(f"MFU: {mfu:.1%}")  # ~21% → room for improvement
# Good MFU: 40-55% for training
```

---

# 8️⃣ Common Bottleneck Patterns

| Symptom | Cause | Fix |
|---------|-------|-----|
| GPU-Util 0% periodically | Data loading | More workers, pin_memory, prefetch |
| GPU-Util <50% constant | Small batch/model | Bigger batch, fuse ops |
| High CPU time in profiler | Python overhead | torch.compile, CUDA graphs |
| Many small CUDA kernels | Op-by-op execution | torch.compile, fusion |
| Memory spikes | Large batch or activation | Gradient checkpointing |
| Slow AllReduce | Network bandwidth | Overlap comm/compute |

---

# 9️⃣ The Roofline Model 📊

The **Roofline Model** (Williams, Waterman & Patterson, 2009) is the gold-standard way to understand whether your kernel is **compute-bound** or **memory-bound**.

$$\text{Attainable Performance} = \min\left(\text{Peak FLOP/s},\ I \times \text{Peak Bandwidth}\right)$$

where $I$ = arithmetic intensity (FLOPs/byte).

| Regime | Arithmetic Intensity | Limited By | Action |
|---|---|---|---|
| Memory-bound | Low $I$ | Memory bandwidth | Improve data reuse, fuse ops |
| Compute-bound | High $I$ | Peak FLOP/s | Use Tensor Cores, mixed precision |

> 🏋️ **Beginner Analogy**: Imagine carrying bricks (data) to build a wall (compute). If you're slow at carrying bricks, it doesn't matter how fast you can lay them — you're **memory-bound**. If bricks arrive fast but you lay them slowly, you're **compute-bound**.

---

# 🔟 Production Profiling Checklist

⭐ Follow this order in real projects:

| Step | Action | Tool |
|---|---|---|
| 1 | Check GPU utilization | `nvidia-smi` |
| 2 | Profile 5-10 training steps | `torch.profiler` |
| 3 | Visualize timeline | TensorBoard flame graph |
| 4 | Identify top-3 time consumers | Profiler table (sort by `cuda_time_total`) |
| 5 | Check arithmetic intensity | Roofline analysis / NVIDIA Nsight Compute |
| 6 | Fix bottleneck, re-profile | Iterate until $U > 0.8$ |

> 🏥 **Remember**: Profile → Diagnose → Fix → Re-profile. Just like a doctor: Test → Diagnose → Treat → Follow-up.

---

# 📝 Summary

| Tool | Use Case |
|------|----------|
| nvidia-smi | Quick GPU utilization check |
| PyTorch Profiler | Detailed kernel-level analysis |
| TensorBoard Plugin | Visual flame graphs |
| Memory snapshot | Debug OOM errors |
| MFU calculation | Overall training efficiency |
| Throughput tracking | Tokens/sec, samples/sec |
| NVIDIA Nsight Compute | Low-level kernel profiling |
| Roofline Model | Compute vs. memory bound analysis |

---

# 📋 Cheat Sheet

| Concept | Formula / Key Fact |
|---|---|
| GPU Utilization | $U = \frac{t_{compute}}{t_{compute}+t_{idle}+t_{transfer}}$ — aim for $U > 0.8$ |
| Throughput | $\text{tokens/sec}$ or $\text{samples/sec}$ |
| Amdahl's Law | $S = \frac{1}{(1-p) + p/N}$ — serial portion limits speedup |
| Arithmetic Intensity | $I = \text{FLOPs} / \text{Bytes}$ — high = compute-bound, low = memory-bound |
| MFU | $\text{MFU} = \frac{6 \cdot N \cdot \text{tokens/sec}}{\text{Peak FLOP/s}}$ — good: 40-55% |
| Roofline Performance | $\min(\text{Peak FLOP/s},\ I \times \text{Peak BW})$ |
| #1 Rule | ⭐ **Profile BEFORE optimizing** — never guess! |

---

# 🔑 Analogy Recap

| Analogy | Concept |
|---|---|
| 🏥 Doctor's checkup | Profiling = running diagnostics on your training |
| 📹 Security camera | GPU timeline = recorded footage of CPU/GPU activity |
| 💳 Bank statement | Memory profiling = seeing where your VRAM "budget" went |
| 🏭 Slowest assembly worker | Bottleneck = the one stage that limits everything |
| 🏋️ Carrying bricks vs laying them | Memory-bound vs compute-bound |

---

# 📚 Research Citations

1. **Williams, S., Waterman, A., & Patterson, D. (2009)**. *Roofline: An Insightful Visual Performance Model for Multicore Architectures.* Communications of the ACM, 52(4), 65–76. — The foundational paper on the Roofline model.
2. **NVIDIA Nsight Systems & Nsight Compute Documentation** — Industry-standard GPU profiling tools for system-level and kernel-level analysis.
3. **PyTorch Profiler (torch.profiler)** — Built-in profiling API with TensorBoard integration. Introduced in PyTorch 1.8+.
4. **Narayanan, D. et al. (2021)**. *Efficient Large-Scale Language Model Training on GPU Clusters Using Megatron-LM.* SC '21. — Discusses MFU measurement and throughput optimization at scale.
5. **Chowdhery, A. et al. (2022)**. *PaLM: Scaling Language Modeling with Pathways.* — Reports MFU benchmarks (46.2% on 6144 TPU v4 chips).

---

# 📖 Resources

### 📕 Books
- **"Performance Analysis and Tuning on Modern CPUs"** — Denis Bakhvalov (2020). Comprehensive guide to performance profiling concepts (many principles transfer to GPU).
- **"Programming Massively Parallel Processors"** — Kirk & Hwu. Essential GPU computing textbook covering memory hierarchy and optimization.
- **"CUDA by Example"** — Sanders & Kandrot. Beginner-friendly introduction to GPU programming and profiling.

### 📄 Papers & Tutorials
- [PyTorch Profiler Tutorial](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html) — Official step-by-step guide to `torch.profiler`
- [PyTorch TensorBoard Profiler Tutorial](https://pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html) — Visualizing profiler output
- [NVIDIA Nsight Systems Docs](https://docs.nvidia.com/nsight-systems/) — System-wide GPU profiling
- [NVIDIA Nsight Compute Docs](https://docs.nvidia.com/nsight-compute/) — Kernel-level profiling and Roofline analysis
- [PyTorch Memory Profiling](https://pytorch.org/blog/understanding-gpu-memory-1/) — Official guide to debugging OOM
- Williams et al. (2009) — *Roofline Model* (see citation above)

### 🎥 Videos
- NVIDIA GTC talks on Nsight profiling (free on NVIDIA On-Demand)
- PyTorch Conference talks on profiling & optimization

---

**Tomorrow**: Data Loading & Preprocessing at Scale.
