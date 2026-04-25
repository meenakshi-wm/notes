📘 DAY 30 — Systems & Hardware Capstone: Training Cluster Design

Goal:

Synthesize everything from Days 76-89. Design a complete training cluster for different model scales, make build-vs-rent decisions, and understand total cost of ownership.

---

## 🏭 The Big Picture Analogy

> ⭐ **Think of a training cluster like a factory floor.** You're building a product (a trained AI model), and you need to design the entire factory:
> - **GPU nodes** = individual workstations where the actual work happens 🔧
> - **Network fabric (NVLink, InfiniBand)** = the conveyor belt system connecting all workstations 🔗
> - **Storage (SSDs, NFS)** = the warehouse holding all raw materials (training data) and finished goods (checkpoints) 📦
> - **Cooling system** = the ventilation keeping the factory from overheating 🌬️
> - **Fault tolerance** = backup generators and spare parts so production never stops ⚡
>
> A badly designed factory wastes time and money — the same is true for a training cluster!

## 🔑 Core Formulas You Need to Know

| Formula | What It Means |
|---------|---------------|
| $C = 6ND$ | **Total compute** — $N$ = model parameters, $D$ = training tokens (Chinchilla scaling law, Hoffmann et al., 2022) |
| $T = \frac{C}{N_{GPU} \times \text{FLOPS}_{peak} \times \text{MFU}}$ | **Training time** — how long the job takes given your hardware |
| $\text{Cost} = T \times N_{GPU} \times \$/hr$ | **Total cost** — wall-clock time × GPU count × hourly rate |
| $\text{MFU} = \frac{\text{Actual FLOPS}}{\text{Peak FLOPS}}$ | **Model FLOPs Utilization** — what fraction of theoretical GPU power you actually use (typically 30-55%) |

> ⭐ **Analogy:** MFU is like a car's fuel efficiency. Your engine *can* produce 300 HP, but on city roads you use maybe 40%. Similarly, GPUs have peak FLOPS but real training only uses 30–55% due to memory transfers, communication, and idle time.

---

# 1️⃣ System Design Exercise: Train a 7B Model

**Goal**: Train Llama-2 7B equivalent from scratch on 2T tokens.

```python
# Compute requirement
params = 7e9
tokens = 2e12
flops = 6 * params * tokens  # 8.4e22 FLOPs

# Time with different setups
setups = {
    '8x A100': {'gpus': 8, 'peak': 312e12, 'mfu': 0.45, 'cost_hr': 32},
    '8x H100': {'gpus': 8, 'peak': 989e12, 'mfu': 0.50, 'cost_hr': 80},
    '32x H100': {'gpus': 32, 'peak': 989e12, 'mfu': 0.45, 'cost_hr': 320},
}

for name, s in setups.items():
    effective = s['gpus'] * s['peak'] * s['mfu']
    time_sec = flops / effective
    time_days = time_sec / 86400
    cost = time_sec / 3600 * s['cost_hr']
    print(f"{name}: {time_days:.0f} days, ${cost:,.0f}")
```

Results:
- 8× A100: ~75 days, ~$58K
- 8× H100: ~24 days, ~$46K
- 32× H100: ~7 days, ~$54K

**Decision**: 8× H100 — best cost efficiency, reasonable time.

> ⭐ **Beginner Note:** The formula behind every line above is:
> $$T_{\text{days}} = \frac{6 \times N \times D}{N_{GPU} \times \text{FLOPS}_{peak} \times \text{MFU} \times 86400}$$
> where 86400 converts seconds → days. Cost = $T_{\text{hours}} \times \$/hr$.

> 🏭 **Factory analogy:** Adding more workstations (GPUs) reduces wall-clock time, but coordination overhead grows — like adding workers to an assembly line where they start bumping into each other.

---

# 2️⃣ Configuration Choices

```yaml
# Training config for 7B on 8x H100
model:
  params: 7B
  layers: 32
  hidden: 4096
  heads: 32
  seq_len: 4096

training:
  parallelism:
    DP: 8        # All GPUs do data parallelism
    TP: 1        # Model fits on one GPU
    PP: 1        # No pipeline needed
  framework: FSDP  # or DeepSpeed ZeRO-2
  precision: BF16
  
  batch_size: 
    micro: 4     # Per GPU
    global: 256  # With gradient accumulation (4 micro × 8 GPU × 8 accum)
  
  optimizer:
    type: AdamW
    lr: 3e-4
    warmup: 2000 steps
    decay: cosine to 3e-5
    weight_decay: 0.1
  
  memory:
    gradient_checkpointing: true
    flash_attention: true
    mixed_precision: bf16

data:
  format: memmap
  num_workers: 8
  pin_memory: true
  
checkpointing:
  interval: 1000 steps
  path: /storage/checkpoints/
```

---

# 3️⃣ Design for Different Scales

| Model | GPUs | DP | TP | PP | Framework | Time (est) | Cost |
|-------|------|-----|-----|-----|-----------|-----------|------|
| 1.3B | 8×A100 | 8 | 1 | 1 | DDP | ~3 days | ~$3K |
| 7B | 8×H100 | 8 | 1 | 1 | FSDP/ZeRO-2 | ~24 days | ~$46K |
| 13B | 16×H100 | 8 | 2 | 1 | FSDP | ~30 days | ~$115K |
| 70B | 64×H100 | 8 | 8 | 1 | ZeRO-3 + TP | ~45 days | ~$690K |
| 405B | 2048×H100 | 128 | 8 | 2 | Megatron 3D | ~60 days | ~$50M+ |

> ⭐ **Why do parallelism strategies change with scale?**
> - **DP (Data Parallel):** Each GPU gets a different batch — like giving each worker a different task. Works when the model fits on one GPU.
> - **TP (Tensor Parallel):** Split a single layer across GPUs — like two workers each assembling half a component. Needed when layers are too big for one GPU.
> - **PP (Pipeline Parallel):** Different GPUs handle different layers — like an assembly line where station 1 does step 1, station 2 does step 2. Needed for 100B+ models.

### 🏭 Production Clusters: Real-World Examples

| Cluster | Organization | GPUs | Network | Notable Achievement |
|---------|-------------|------|---------|---------------------|
| **RSC (Research SuperCluster)** | Meta | 16,000× A100 (80GB) | 200 Gbps InfiniBand | Trained LLaMA models (Touvron et al., 2023) |
| **TPU v4 Pods** | Google | 4,096 TPU v4 chips | Custom ICI fabric | Trained PaLM 540B in ~60 days (Chowdhery et al., 2022) |
| **DGX SuperPOD** | NVIDIA | 140× DGX H100 (1,120 H100s) | 400 Gbps InfiniBand NDR | Reference architecture for enterprise AI |
| **Frontier cluster** | Microsoft/OpenAI | ~10,000+ A100s → H100s | InfiniBand HDR/NDR | Trained GPT-4 (estimated) |

> 📖 **Citation:** Chowdhery et al. (2022), *"PaLM: Scaling Language Modeling with Pathways"* — demonstrated efficient use of 6,144 TPU v4 chips with 46.2% MFU, showing that hardware utilization is critical at scale.

> 📖 **Citation:** Zhang et al. (2022), *"OPT-175B: An Open Pre-trained Transformer Language Model"* — documented the engineering challenges of training on 992 A100 GPUs, including ~35 manual restarts due to hardware failures.

---

# 4️⃣ Build vs Rent Decision

> 🏭 **Analogy:** Should you buy a factory or lease space? If you're running production 24/7, owning is cheaper. If you only need it for a few months a year, leasing makes sense.

```python
def build_vs_rent(num_h100s, training_days_per_year, cloud_cost_per_gpu_hr):
    # Build costs (approximate)
    cost_per_h100_server = 300_000  # 8x H100 DGX
    infra_cost = 50_000  # Per server: networking, cooling, rack
    num_servers = num_h100s // 8
    capex = num_servers * (cost_per_h100_server + infra_cost)
    annual_opex = capex * 0.15  # Power, cooling, staff
    
    build_3yr = capex + 3 * annual_opex
    build_monthly = build_3yr / 36
    
    # Rent costs 
    rent_hours = training_days_per_year * 24 * num_h100s
    rent_annual = rent_hours * cloud_cost_per_gpu_hr
    rent_3yr = rent_annual * 3
    rent_monthly = rent_3yr / 36
    
    print(f"BUILD (3yr): ${build_3yr:,.0f} (${build_monthly:,.0f}/mo)")
    print(f"RENT (3yr):  ${rent_3yr:,.0f} (${rent_monthly:,.0f}/mo)")
    print(f"Break-even utilization: {build_3yr / rent_3yr * 100:.0f}% of rent time")

# If you run GPUs >60% of the time → build
# If you run GPUs <40% of the time → rent
# In between → depends on specifics
```

---

# 5️⃣ Reliability & Fault Tolerance

> 🏭 **Analogy:** In a factory with 1,000 machines, something breaks every day. You don't shut down the whole factory — you have **backup generators** (redundant power), **spare parts** (hot-swap GPUs), and **insurance records** (checkpoints) so you can resume production instantly.

At scale, failures are EXPECTED:
- 1000 GPUs × 1 year: expect 50-100 GPU failures
- Memory errors, NVLink failures, switch failures

### ⏱️ Mean Time Between Failures (MTBF)

$$\text{MTBF}_{\text{cluster}} = \frac{\text{MTBF}_{\text{single GPU}}}{N_{GPU}}$$

| Cluster Size | Single GPU MTBF | Cluster MTBF | Failures/Month |
|-------------|----------------|-------------|----------------|
| 8 GPUs | ~50,000 hrs | ~6,250 hrs (~260 days) | ~0.1 |
| 64 GPUs | ~50,000 hrs | ~781 hrs (~32 days) | ~1 |
| 1,024 GPUs | ~50,000 hrs | ~49 hrs (~2 days) | ~15 |
| 16,000 GPUs | ~50,000 hrs | ~3 hrs | ~240 |

> ⭐ **Key insight:** At 16K GPUs, you get a failure every ~3 hours! This is why Meta's RSC and Google's TPU pods invest heavily in automatic failure detection and recovery. OPT-175B training required ~35 manual restarts (Zhang et al., 2022).

```python
class FaultTolerantTrainer:
    def __init__(self, model, checkpoint_dir, checkpoint_interval=500):
        self.checkpoint_dir = checkpoint_dir
        self.checkpoint_interval = checkpoint_interval
    
    def train(self, num_steps):
        start_step = self.load_latest_checkpoint()
        
        for step in range(start_step, num_steps):
            try:
                self.train_step()
                
                if step % self.checkpoint_interval == 0:
                    self.save_checkpoint(step)
                    
            except RuntimeError as e:
                if 'NCCL' in str(e) or 'CUDA' in str(e):
                    print(f"GPU failure at step {step}. Restarting from checkpoint.")
                    self.handle_failure()
                    self.load_latest_checkpoint()
                else:
                    raise
    
    def handle_failure(self):
        # Re-initialize distributed group (potentially with replacement GPU)
        dist.destroy_process_group()
        dist.init_process_group("nccl")
```

---

---

# 🔧 Cluster Design Decision Checklist

| Decision Area | Key Questions | Beginner Tip |
|--------------|---------------|-------------|
| **GPU Selection** 🖥️ | A100 vs H100 vs TPU? How much VRAM? | H100 = latest & fastest; A100 = cheaper & proven |
| **Network Topology** 🔗 | InfiniBand vs RoCE? Bandwidth? | InfiniBand = gold standard for multi-node training |
| **Storage Architecture** 📦 | NVMe vs NFS? Throughput needed? | Fast storage prevents GPUs from waiting for data |
| **Cooling** 🌬️ | Air vs liquid? Heat density? | Liquid cooling needed for dense H100 clusters (700W/GPU) |
| **Power** ⚡ | Total kW? Redundancy? | 8× H100 DGX = ~10 kW; plan for 2× headroom |
| **Fault Tolerance** 🛡️ | Checkpoint frequency? Hot-swap? | Checkpoint every 500-1000 steps; automate recovery |

---

# 6️⃣ Phase Summary: Days 76-90

| Day | Topic | Key Takeaway |
|-----|-------|-------------|
| 76 | GPU Architecture | Tensor Cores = 15x speedup for matmul |
| 77 | CUDA | Kernel launches, memory management |
| 78 | Mixed Precision | BF16 = 2x speed, no quality loss |
| 79 | DDP & FSDP | Multi-GPU data + model parallelism |
| 80 | Flash Attention | O(N) memory for attention |
| 81 | Triton | Custom GPU kernels in Python |
| 82 | Quantization | INT4 = 4x smaller, ~97% quality |
| 83 | Infrastructure | Cloud costs, cluster design |
| 84 | Profiling | Find and fix bottlenecks |
| 85 | Data Loading | Don't starve the GPU |
| 86 | DeepSpeed | ZeRO for training huge models |
| 87 | TP & PP | 3D parallelism for 100B+ |
| 88 | Inference | KV cache, batching, speculative |
| 89 | Networking | NVLink, InfiniBand, NCCL |
| 90 | **Capstone** | Complete cluster design |

---

# 📝 Founder Edge

| Decision | Recommendation |
|----------|---------------|
| First GPU | RTX 4090 for dev, A100 for serious work |
| Framework | HuggingFace + Accelerate (start), DeepSpeed (scale) |
| Serving | vLLM for production, Ollama for dev |
| Quantization | INT4 GPTQ/AWQ for inference, BF16 for training |
| Scale plan | 1 GPU → 8 GPU → multi-node (only when needed) |
| Build vs rent | Rent until >60% utilization sustained |

**Next Phase**: Days 91-105 — Paper Reproduction Sprint.

---

# 📋 Cheat Sheet: Training Cluster Design

| Concept | Formula / Rule | Quick Explanation |
|---------|---------------|-------------------|
| Total Compute | $C = 6ND$ | 6 × parameters × tokens (Chinchilla) |
| Training Time | $T = \frac{C}{N_{GPU} \times \text{FLOPS} \times \text{MFU}}$ | Compute ÷ effective throughput |
| Training Cost | $\text{Cost} = T \times N_{GPU} \times \$/hr$ | Time × GPUs × hourly price |
| MFU | $\frac{\text{Actual FLOPS}}{\text{Peak FLOPS}}$ | Typically 30–55% in practice |
| Cluster MTBF | $\frac{\text{MTBF}_{GPU}}{N_{GPU}}$ | More GPUs = more frequent failures |
| Build vs Rent | >60% utilization → build | Below 40% → rent cloud GPUs |
| Chinchilla Optimal | $D \approx 20N$ | Train on ~20× more tokens than parameters |
| Memory per GPU | $\approx 18\text{–}20 \times$ params (bytes, BF16+Adam) | 7B model ≈ 140 GB optimizer state |

---

# 📚 Resources

## 📖 Papers
| Paper | Authors | Year | Key Contribution |
|-------|---------|------|------------------|
| *PaLM: Scaling Language Modeling with Pathways* | Chowdhery et al. | 2022 | 540B model on 6,144 TPU v4 chips, 46.2% MFU |
| *OPT-175B: An Open Pre-trained Transformer Language Model* | Zhang et al. | 2022 | Open-sourced training logs showing real-world cluster failures |
| *Training Compute-Optimal Large Language Models* | Hoffmann et al. | 2022 | Chinchilla scaling laws: $C = 6ND$, optimal $D \approx 20N$ |
| *Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism* | Shoeybi et al. | 2019 | Tensor parallelism and 3D parallelism strategies |
| *DGX SuperPOD Reference Architecture* | NVIDIA | 2023 | Enterprise cluster design with InfiniBand NDR |

## 📕 Books & Guides
- **"Designing Distributed Systems"** — Brendan Burns (O'Reilly) — patterns for reliable, scalable systems
- **"Large Scale Machine Learning with Python"** — Bastiaan Sjardin et al. — practical distributed ML
- **NVIDIA DGX Documentation** — [docs.nvidia.com/dgx](https://docs.nvidia.com/dgx) — official hardware specs and cluster guides
- **Google Cloud TPU Documentation** — architecture and best practices for TPU pod training
- **Microsoft DeepSpeed Documentation** — [deepspeed.ai](https://www.deepspeed.ai/) — ZeRO optimizer and 3D parallelism tutorials
- **HuggingFace Accelerate Docs** — beginner-friendly multi-GPU training guide
