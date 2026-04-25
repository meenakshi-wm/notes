📘 DAY 27 — Pipeline & Tensor Parallelism

Goal:

Master the two advanced forms of model parallelism used for training the largest models — tensor parallelism (split layers across GPUs) and pipeline parallelism (split layers between GPUs).

---

# 🌟 Beginner's Guide: What Is This About?

> ⭐ **No AI knowledge needed!** Imagine training a massive AI brain with **billions** of parameters — it simply won't fit on one computer chip (GPU). We need to **split the work** across many chips.

### 🏭 The Assembly Line Analogy

Think of building a car on an **assembly line**:

| Real-World Assembly Line | Pipeline Parallelism |
|---|---|
| 🚗 Each station does one part (doors, engine, paint) | Each GPU handles a **group of layers** |
| 🚗 Cars move from station to station | Data (microbatches) flows GPU → GPU |
| 🚗 Multiple cars on the line at once | Multiple microbatches in-flight simultaneously |
| 🚗 Idle time when line starts up / winds down | **Pipeline bubble** = wasted GPU time |
| 🚗 More cars on the line → less idle % | More microbatches → smaller bubble fraction |

**Tensor parallelism** is different — imagine a single station where **two workers split one task** (e.g., two painters each paint half the car at the same time). That's splitting a *single layer* across GPUs.

---

# 1️⃣ Three Types of Parallelism

| Type | What's Split | Communication | Best For |
|------|-------------|---------------|---------|
| Data Parallel | Data (batches) | AllReduce (gradients) | Speed scaling |
| Tensor Parallel | Individual layers | AllReduce (activations) | Intra-node (NVLink) |
| Pipeline Parallel | Groups of layers | Point-to-point (activations) | Inter-node |

Modern large-scale training uses ALL THREE simultaneously (3D parallelism).

---

# 2️⃣ Tensor Parallelism (TP)

Split each layer's computation across GPUs within a node.

### Example: Linear Layer Split
W ∈ R^{d × 4d} (e.g., FFN projection)

Split by columns: W = [W₁ | W₂] where W₁, W₂ ∈ R^{d × 2d}

```
GPU 0: Y₁ = X · W₁    (d × 2d matrix multiply)
GPU 1: Y₂ = X · W₂    (d × 2d matrix multiply)
               ↓
         Y = [Y₁ | Y₂]  (concatenate, or AllReduce depending on next op)
```

### Megatron-Style TP for Transformer

```
Self-Attention (TP=2):
  GPU 0: Q₁, K₁, V₁ → heads 0-15 → O₁
  GPU 1: Q₂, K₂, V₂ → heads 16-31 → O₂
  AllReduce: O = O₁ + O₂

FFN (TP=2):
  GPU 0: H₁ = GELU(X · W₁_up) · W₁_down
  GPU 1: H₂ = GELU(X · W₂_up) · W₂_down
  AllReduce: H = H₁ + H₂
```

Each layer requires 2 AllReduce operations (one per sub-layer). Must be on fast interconnect (NVLink).

---

# 3️⃣ Pipeline Parallelism (PP)

Split the model into stages, each on a different GPU:

```
Stage 0 (GPU 0): Layers 0-7
Stage 1 (GPU 1): Layers 8-15
Stage 2 (GPU 2): Layers 16-23
Stage 3 (GPU 3): Layers 24-31
```

### 📐 Key Math: Memory Savings

With $p$ pipeline stages, each stage holds only a fraction of the total model parameters $\Psi$:

$$\text{Memory per stage} = \frac{\Psi}{p}$$

> ⭐ **Analogy**: If a book has 320 pages and 4 friends each take 80 pages to summarize — that's pipeline parallelism for memory!

Problem: Naive pipeline has terrible utilization — "pipeline bubble":

```
Naive (1 batch):
GPU 0: [Fwd]                                [Bwd]
GPU 1:       [Fwd]                    [Bwd]
GPU 2:             [Fwd]        [Bwd]
GPU 3:                   [Fwd+Bwd]
       ~75% idle time!
```

---

# 4️⃣ Micro-Batching: Reducing the Bubble

Split each batch into micro-batches and pipeline them:

```
GPipe schedule (4 micro-batches):
GPU 0: [F1][F2][F3][F4]            [B4][B3][B2][B1]
GPU 1:     [F1][F2][F3][F4]    [B4][B3][B2][B1]
GPU 2:         [F1][F2][F3][F4][B4][B3][B2][B1]
GPU 3:             [F1][F2][F3][F4][B4][B3][B2][B1]

Bubble fraction = (p-1) / (m + p - 1)
With p=4 stages, m=4 micro-batches: 3/7 = 43% bubble
With p=4 stages, m=32 micro-batches: 3/35 = 8.6% bubble
```

### 📐 Pipeline Bubble Math

The **bubble ratio** — the fraction of time GPUs sit idle:

$$\beta = \frac{p - 1}{m}$$

where $p$ = number of pipeline stages, $m$ = number of microbatches.

The **ideal throughput** (fraction of useful work vs. total time):

$$\text{Throughput}_{\text{ideal}} = \frac{m}{m + p - 1}$$

| $p$ (stages) | $m$ (microbatches) | Bubble $\beta$ | Throughput | Efficiency |
|---|---|---|---|---|
| 4 | 4 | 75% | 57% | ⚠️ Poor |
| 4 | 16 | 19% | 84% | ✅ Good |
| 4 | 32 | 9% | 91% | ✅ Great |
| 8 | 64 | 11% | 90% | ✅ Great |
| 8 | 8 | 88% | 53% | ❌ Terrible |

> ⭐ **Rule of thumb**: Keep $m \gg p$ (many more microbatches than stages) to minimize the bubble!

> 🏭 **Analogy**: If your assembly line has 4 stations and you only send 4 cars through, 43% of the time *some station is idle*. Send 32 cars, and idle time drops to ~9%.

---

# 5️⃣ 1F1B Schedule (Interleaved)

> ⭐ **GPipe vs PipeDream (1F1B) Analogy**:
> - **GPipe** 🚗➡️➡️➡️➡️ then ⬅️⬅️⬅️⬅️ = All cars go forward first, then ALL go backward. Simple but uses lots of memory (must store all activations).
> - **PipeDream (1F1B)** 🚦 = Like a **traffic signal** — alternate one forward, one backward. Less memory, same bubble.

| Schedule | Memory Cost | Bubble | Complexity |
|---|---|---|---|
| GPipe | $O(m)$ activations | $\frac{p-1}{m+p-1}$ | Simple |
| 1F1B (PipeDream) | $O(p)$ activations | Same bubble | Moderate |
| Interleaved 1F1B | $O(p)$ activations | $\frac{p-1}{m \cdot v + p - 1}$ ($v$ = virtual stages) | Complex |

Better than GPipe — start backward before all forwards complete:

```
1F1B schedule:
GPU 0: [F1][F2][F3][F4][B1][F5][B2][F6][B3][B4]
GPU 1:     [F1][F2][F3][B1][F4][B2][F5][B3][B4]
GPU 2:         [F1][F2][B1][F3][B2][F4][B3][B4]
GPU 3:             [F1][B1][F2][B2][F3][B3][B4]

Less memory: only need to store p (not m) micro-batch activations.
```

> ⭐ **Why 1F1B wins on memory**: GPipe must remember activations for ALL $m$ microbatches until backward starts. 1F1B only keeps $p$ activations alive at once — a huge saving when $m \gg p$.

---

# 6️⃣ 3D Parallelism

Combine all three for massive models:

```
Example: 175B model on 128 GPUs (16 nodes × 8 GPUs)

Tensor Parallel: 8 (within each node over NVLink)
Pipeline Parallel: 4 (across 4 groups of 4 nodes)  
Data Parallel: 4 (4 replicas)

8 × 4 × 4 = 128 GPUs

Node 0-3:  PP Stage 0, DP Replica 0  (each node has TP=8)
Node 4-7:  PP Stage 1, DP Replica 0
Node 8-11: PP Stage 2, DP Replica 0
Node 12-15: PP Stage 3, DP Replica 0
... (repeat for other DP replicas, or use ZeRO)
```

---

# 7️⃣ Implementation with Megatron-LM

```python
# Megatron-style tensor parallel linear layer
class ColumnParallelLinear(nn.Module):
    def __init__(self, in_features, out_features, tp_size):
        super().__init__()
        self.tp_rank = dist.get_rank() % tp_size
        self.out_per_partition = out_features // tp_size
        self.weight = nn.Parameter(torch.randn(self.out_per_partition, in_features))
    
    def forward(self, x):
        # Each GPU computes its partition
        output = F.linear(x, self.weight)
        return output  # No communication needed here

class RowParallelLinear(nn.Module):
    def __init__(self, in_features, out_features, tp_size):
        super().__init__()
        self.tp_rank = dist.get_rank() % tp_size
        self.in_per_partition = in_features // tp_size
        self.weight = nn.Parameter(torch.randn(out_features, self.in_per_partition))
    
    def forward(self, x):
        output = F.linear(x, self.weight)
        # AllReduce across TP group
        dist.all_reduce(output, group=self.tp_group)
        return output
```

---

# 8️⃣ When to Use What

| Scenario | DP | TP | PP |
|----------|-----|-----|-----|
| 7B, 8 GPUs | 8 | 1 | 1 |
| 13B, 8 GPUs | 4 | 2 | 1 |
| 70B, 32 GPUs | 4 | 8 | 1 |
| 70B, 64 GPUs | 8 | 8 | 1 |
| 175B, 128 GPUs | 4 | 8 | 4 |
| 540B, 2048 GPUs | 16 | 8 | 16 |

Rules of thumb:
- TP within node (NVLink is fast)
- PP across nodes (only point-to-point)
- DP for remaining parallelism

---

# 📝 Summary

| Parallelism | Splits | Communication | Efficiency |
|------------|--------|---------------|------------|
| Data (DP) | Batches | AllReduce (1× model size) | ~90%+ |
| Tensor (TP) | Layers | AllReduce (2× per layer) | ~85%+ (needs NVLink) |
| Pipeline (PP) | Layer groups | Point-to-point | 85-95% (depends on bubbles) |
| 3D | All | All types | Used for 100B+ models |

**Tomorrow**: Model Serving & Inference Optimization.

---

# 🏭 Production Scenarios

| System | Model Size | 3D Config (TP×PP×DP) | Key Insight |
|---|---|---|---|
| **Megatron-LM** (NVIDIA) | 530B (MT-NLG) | 8×35×??? | Interleaved 1F1B schedule, NVLink for TP |
| **PaLM** (Google, 2022) | 540B | Pathways system | 6144 TPUs, novel 2D pipeline schedule |
| **GPT-3** (OpenAI) | 175B | 8×8×~4 | One of the first 3D parallelism deployments |
| **LLaMA-2 70B** (Meta) | 70B | 8×1×DP | TP only (fits in one node), no pipeline needed |
| **Bloom** (BigScience) | 176B | 4×12×DP | DeepSpeed ZeRO + PP for open-source training |

> ⭐ **Key production lessons**:
> - Pipeline parallelism is essential for models that **don't fit on a single node** even with tensor parallelism
> - Combined with TP + DP → **3D parallelism** is the standard for 100B+ models
> - Interleaved schedules (Megatron-LM) reduce bubble overhead by $v\times$ with $v$ virtual stages
> - Communication placement matters: TP on **NVLink** (fast), PP on **InfiniBand** (cross-node)

---

# 📚 Research Citations

| Paper | Authors | Year | Key Contribution |
|---|---|---|---|
| **GPipe: Efficient Training of Giant Neural Networks using Pipeline Parallelism** | Huang et al. | 2019 | Introduced micro-batch pipelining, gradient accumulation across microbatches |
| **PipeDream: Generalized Pipeline Parallelism for DNN Training** | Narayanan et al. | 2019 | 1F1B schedule, weight stashing for async updates |
| **Efficient Large-Scale Language Model Training on GPU Clusters** | Narayanan et al. | 2021 | Interleaved 1F1B, 3D parallelism analysis, bubble reduction via virtual stages |
| **Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism** | Shoeybi et al. | 2020 | Tensor parallelism for Transformers, column/row parallel patterns |
| **PaLM: Scaling Language Modeling with Pathways** | Chowdhery et al. | 2022 | 540B model, Pathways system for pipeline + data parallelism at scale |

---

# 🧾 Cheat Sheet

| Concept | Formula / Rule | Remember |
|---|---|---|
| Bubble ratio | $\beta = \frac{p-1}{m}$ | More microbatches → smaller bubble 📉 |
| Ideal throughput | $\frac{m}{m + p - 1}$ | Approaches 100% as $m \to \infty$ |
| Memory per stage | $\frac{\Psi}{p}$ | Linear memory savings with more stages |
| GPipe memory | $O(m)$ activations | Must store all micro-batch activations |
| 1F1B memory | $O(p)$ activations | Only $p$ activations in flight |
| TP placement | Within node (NVLink) | 2 AllReduce ops per layer |
| PP placement | Across nodes (InfiniBand) | Point-to-point only |
| 3D parallelism | $\text{TP} \times \text{PP} \times \text{DP} = \text{Total GPUs}$ | Standard for 100B+ models |
| Rule of thumb | $m \gg p$ | Keep microbatches >> stages |

---

# 📖 Resources

### Books
- 📕 **"Deep Learning"** — Goodfellow, Bengio, Courville (Ch. 12: Distributed Training)
- 📕 **"Efficient Processing of Deep Neural Networks"** — Sze et al. (parallelism strategies)
- 📕 **"Programming Massively Parallel Processors"** — Kirk & Hwu (GPU architecture fundamentals)

### Papers
- 📄 Huang et al. (2019) — [GPipe: Efficient Training of Giant Neural Networks using Pipeline Parallelism](https://arxiv.org/abs/1811.06965)
- 📄 Narayanan et al. (2019) — [PipeDream: Generalized Pipeline Parallelism for DNN Training](https://arxiv.org/abs/1806.03377)
- 📄 Narayanan et al. (2021) — [Efficient Large-Scale Language Model Training on GPU Clusters](https://arxiv.org/abs/2104.04473)
- 📄 Shoeybi et al. (2020) — [Megatron-LM: Training Multi-Billion Parameter Language Models](https://arxiv.org/abs/1909.08053)
- 📄 Chowdhery et al. (2022) — [PaLM: Scaling Language Modeling with Pathways](https://arxiv.org/abs/2204.02311)

### Tutorials & Docs
- 🔗 [NVIDIA Megatron-LM GitHub](https://github.com/NVIDIA/Megatron-LM) — Reference implementation of 3D parallelism
- 🔗 [HuggingFace — Model Parallelism Tutorial](https://huggingface.co/docs/transformers/parallelism) — Beginner-friendly guide to TP, PP, DP
- 🔗 [DeepSpeed Pipeline Parallelism](https://www.deepspeed.ai/tutorials/pipeline/) — Microsoft's pipeline parallelism tutorial
- 🔗 [PyTorch Distributed Overview](https://pytorch.org/tutorials/beginner/dist_overview.html) — Official distributed training docs
