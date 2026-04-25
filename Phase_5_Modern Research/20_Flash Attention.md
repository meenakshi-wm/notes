📘 DAY 20 — Flash Attention & Memory-Efficient Training

Goal:

Master Flash Attention — the algorithm that made long-context LLMs practical. Understand why standard attention is memory-bottlenecked, how tiling solves it, and implement memory-efficient training techniques.

---

> 🌟 **Why This Matters (Beginner Start Here!):**
> Imagine you're a student taking notes in class. The teacher speaks for 2 hours straight. **Standard attention** is like trying to write down *every single word* on a *tiny sticky note* before you can start studying — you run out of space! **Flash Attention** is like taking notes *chapter by chapter* in a notebook, summarizing as you go. You never need to write the entire lecture at once. This single idea is what lets ChatGPT, GPT-4, Llama, and every modern AI read *entire books* at once instead of just a few paragraphs.
>
> — ⭐ **Flash Attention is arguably the most important systems optimization in modern AI** ⭐

---

## 🔑 Key Terminology for Absolute Beginners

| Term | Plain English | Analogy |
|------|--------------|---------|
| **Attention** | How an AI decides which words to focus on when reading a sentence | Like highlighting the important parts of a textbook 🖍️ |
| **Sequence Length (N)** | How many words/tokens the AI reads at once | The length of the passage you're reading 📖 |
| **HBM (High Bandwidth Memory)** | The GPU's main memory — big but slow | 🏭 A warehouse: holds a LOT, but it's across town |
| **SRAM (Static RAM)** | The GPU's on-chip cache — tiny but blazing fast | 🖥️ Your desk: small, but everything's at arm's reach |
| **Tiling** | Breaking a big computation into small chunks | 🍕 Eating pizza slice by slice instead of the whole pie at once |
| **Softmax** | A function that turns raw scores into probabilities (sums to 1) | Like grading on a curve: every student's score becomes a percentage |
| **FLOPs** | Floating-point operations — how much "math work" the GPU does | The number of arithmetic problems on a math test ✏️ |
| **Materialization** | Actually creating and storing a full matrix in memory | Printing the entire encyclopedia vs. looking up one page at a time |

---

# 1️⃣ The Attention Memory Problem

> 🎯 **Beginner Analogy — The Sticky Note Problem:**
> Imagine you need to compare every word in a book to *every other word* to find relationships. For a 1,000-word book, that's 1,000 × 1,000 = 1,000,000 comparisons. Now imagine a 128,000-word book... that's **16.4 BILLION** comparisons. Standard attention tries to write ALL those comparisons onto a single sticky note (GPU memory). It doesn't fit! 📝💥

Standard attention for sequence length N:

$Q, K, V \in \mathbb{R}^{N \times d}$

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V$$

The problem: $QK^T$ is $N \times N$.
For $N = 128K$, $d = 128$: $QK^T = 128K \times 128K \times 2 \text{ bytes} = 32\text{GB}$. Per head. Per layer!

This is the memory bottleneck that limits context length.

**⭐ Why this matters in practice:** Before Flash Attention, most models were limited to 512–2048 tokens. That's about 1–2 pages of text. With Flash Attention, models now handle 128K+ tokens (an entire novel!).

### 📊 The Quadratic Wall — Memory Grows Explosively

| Sequence Length ($N$) | $N \times N$ Entries | Memory for $QK^T$ (FP16) | Can Fit in 80GB GPU? |
|---|---|---|---|
| 512 | 262K | 0.5 MB | ✅ Easily |
| 2,048 | 4.2M | 8 MB | ✅ Yes |
| 8,192 | 67M | 128 MB | ✅ Yes |
| 32,768 | 1.07B | 2 GB | ⚠️ Tight |
| 131,072 | 17.2B | 32 GB | ❌ Per head! |

> 📚 **Research Context:** The quadratic memory bottleneck was the central motivation for Dao et al. (2022), "FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness" — the paper that changed how every LLM is trained today.

---

# 2️⃣ Flash Attention: The Key Insight

> 🎯 **Beginner Analogy — Chapter-by-Chapter Reading:**
> 📖 Standard attention = You photocopy the ENTIRE book, lay out all pages on the floor (your tiny apartment), and THEN start studying. If the book is too long, your apartment floods with paper.
> 📖 Flash attention = You read **one chapter at a time**, take notes, then put the chapter back on the shelf. You only ever have ONE chapter on your desk. Your notes at the end are *exactly the same* — you just never needed the whole book at once!

### 🏗️ GPU Memory Hierarchy (Critical Concept!)

```
┌─────────────────────────────────────────────┐
│                    HBM                       │
│         (High Bandwidth Memory)              │
│     🏭 The Warehouse: 40-80 GB              │
│     Bandwidth: ~2 TB/s                       │
│     Far from compute cores                   │
│                                              │
│   ┌──────────────────────────────────┐       │
│   │            SRAM                  │       │
│   │      (On-chip Cache)             │       │
│   │   🖥️ The Desk: ~20 MB          │       │
│   │   Bandwidth: ~19 TB/s           │       │
│   │   Right next to compute cores   │       │
│   │                                  │       │
│   │   ⚡ ~10× faster than HBM!      │       │
│   └──────────────────────────────────┘       │
└─────────────────────────────────────────────┘
```

> ⭐ **The key realization:** Modern GPUs are **memory-bound**, not compute-bound. The bottleneck isn't doing the math — it's **moving data** between HBM and SRAM. Flash Attention minimizes these data transfers!

### Standard Attention (Slow — Many HBM Trips) 🐌

Standard attention:
1. Compute full $S = QK^T$ ($N \times N$) — store in HBM
2. Compute $P = \text{softmax}(S)$ ($N \times N$) — store in HBM
3. Compute $O = P \cdot V$ — store in HBM
Total HBM access: $O(N^2 + Nd)$ reads/writes

> 🚚 **Analogy:** This is like a construction worker who walks to the warehouse *three separate times* to get materials, carrying the *entire supply* each time. Exhausting!

### Flash Attention (Fast — Minimal HBM Trips) ⚡

Flash Attention: NEVER materialize the $N \times N$ matrix!

Instead, use **tiling** (the pizza strategy 🍕):
1. Load small blocks of Q, K, V from HBM to SRAM
2. Compute partial attention in SRAM
3. Combine results using **online softmax**
4. Write only the output O back to HBM

Total HBM access: $O\!\left(\frac{N^2 d^2}{M}\right)$ where $M$ = SRAM size
For typical settings: **2-4× fewer memory reads/writes**.

### ⚖️ Standard vs. Flash Attention — Complete Comparison

| Property | Standard Attention | Flash Attention |
|---|---|---|
| **Memory complexity** | $O(N^2)$ | ⭐ $O(N)$ |
| **HBM accesses** | $O(N^2 + Nd)$ | $O\!\left(\frac{N^2 d^2}{M}\right)$ |
| **FLOPs** | $O(N^2 d)$ | $O(N^2 d)$ — *same!* |
| **Materializes $N \times N$ matrix?** | ✅ Yes (the problem!) | ❌ Never |
| **Where computation happens** | Mostly HBM ↔ SRAM transfers | Mostly inside SRAM |
| **Wall-clock speedup** | Baseline | **2-4× faster** ⚡ |
| **Exact or approximate?** | Exact | ⭐ **Also exact!** (not an approximation) |
| **Max practical seq length** | ~2K-8K | **128K+** 🚀 |

> 💡 **Crucial insight:** Flash Attention computes the **exact same result** as standard attention. It's not an approximation — it's a smarter way to organize the *same* computation. Same math, fewer memory trips.

> 📚 **Citation:** Dao, T., Fu, D.Y., Ermon, S., Rudra, A., & Ré, C. (2022). "FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness." *NeurIPS 2022*.

---

# 3️⃣ Online Softmax Trick

> 🎯 **Beginner Analogy — Grading Exams One by One:**
> Imagine you're a teacher who needs to grade 1,000 exams **on a curve** (that's what softmax does — turns raw scores into percentages). The "standard" way: collect ALL 1,000 exams, find the highest score, then calculate everyone's curved grade. But what if exams arrive one at a time? **Online softmax** = you grade each exam as it arrives, keeping a **running average** and **running maximum**. When you get a new highest score, you quickly adjust all previous grades. At the end, your curved grades are *exactly the same* as if you'd waited for all exams! ✅

The key challenge: softmax requires knowing the max and sum over the FULL row.

### 📐 The Math Behind Online Softmax

Standard softmax over a full row $\mathbf{x} = [x_1, x_2, ..., x_N]$:

$$\text{softmax}(x_i) = \frac{e^{x_i - m}}{\sum_{j=1}^{N} e^{x_j - m}}, \quad m = \max_j(x_j)$$

The problem: you need $m = \max$ over **all** $N$ elements before computing **any** output.

**Online softmax** splits this into blocks and maintains running statistics:
- $m^{(t)}$: running maximum after processing block $t$
- $\ell^{(t)}$: running sum of exponentials after block $t$
- $o^{(t)}$: running weighted output after block $t$

When a new block $t+1$ arrives with local max $m^{(\text{local})}$:

$$m^{(t+1)} = \max\!\left(m^{(t)},\, m^{(\text{local})}\right)$$

$$\ell^{(t+1)} = e^{m^{(t)} - m^{(t+1)}} \cdot \ell^{(t)} + \sum_j e^{s_j - m^{(t+1)}}$$

$$o^{(t+1)} = e^{m^{(t)} - m^{(t+1)}} \cdot o^{(t)} + \sum_j e^{s_j - m^{(t+1)}} \cdot v_j$$

Final output: $o^{(\text{final})} / \ell^{(\text{final})}$

> ⭐ **The magic:** The rescaling factor $e^{m^{(t)} - m^{(t+1)}}$ "corrects" all previous computations when a new maximum is discovered. This means we **never need the full** $N \times N$ **matrix in memory!**

> 📚 **Citation:** Milakov, M. & Gimelshein, N. (2018). "Online normalizer calculation for softmax." *arXiv:1805.02867*.

Online softmax (Milakov & Gimelshein, 2018):

```python
def online_softmax_attention(Q_block, K_blocks, V_blocks):
    """Process K,V in blocks, maintaining running softmax."""
    m_prev = -float('inf')  # running max
    l_prev = 0.0           # running sum of exp
    o_prev = 0.0           # running output
    
    for K_j, V_j in zip(K_blocks, V_blocks):
        # Attention scores for this block
        s_j = Q_block @ K_j.T / sqrt_d
        
        # Update running max
        m_new = max(m_prev, s_j.max(dim=-1))
        
        # Rescale previous accumulator
        alpha = exp(m_prev - m_new)
        
        # New exponentials
        p_j = exp(s_j - m_new)
        
        # Update running sum
        l_new = alpha * l_prev + p_j.sum(dim=-1)
        
        # Update output: rescale old output + add new contribution
        o_new = alpha * o_prev + p_j @ V_j
        
        m_prev, l_prev, o_prev = m_new, l_new, o_new
    
    return o_prev / l_prev  # Normalize
```

> 💡 **Step-by-step walkthrough for beginners:**
> 1. Start with "I know nothing" → max = $-\infty$, sum = 0
> 2. Read first chunk of K → compute scores, set initial max and sum
> 3. Read second chunk of K → maybe there's a bigger score! If so, **correct** the previous sum using the rescaling factor $\alpha = e^{m_{\text{old}} - m_{\text{new}}}$
> 4. Keep going through all chunks...
> 5. At the end, divide output by the total sum → **exact softmax result!** 🎉

### 🍕 Tiling — Breaking the Matrix into Bite-Sized Pieces

Flash Attention splits Q, K, V into blocks:
- **Q blocks:** $B_r \times d$ (rows of queries)
- **K, V blocks:** $B_c \times d$ (rows of keys/values)

where $B_r$ and $B_c$ are chosen so blocks **fit in SRAM**:

$$B_c = \left\lceil \frac{M}{4d} \right\rceil, \quad B_r = \min\!\left(\left\lceil \frac{M}{4d} \right\rceil, d\right)$$

> 🍕 **Analogy:** Instead of trying to stuff the whole pizza ($N \times N$ matrix) into your mouth, you cut it into slices ($B_r \times B_c$ blocks) and eat one at a time. Much more manageable!

```
Full N×N Matrix:              Tiled Processing:
┌─────────────────┐           ┌──┬──┬──┬──┐
│                 │           │B₁│B₂│B₃│B₄│  ← Process one
│   Entire QKᵀ   │    →      ├──┼──┼──┼──┤     block at a time
│   (HUGE! 💥)   │           │B₅│B₆│B₇│B₈│     in SRAM
│                 │           ├──┼──┼──┼──┤
│                 │           │B₉│..│..│..│
└─────────────────┘           └──┴──┴──┴──┘
```

---

# 4️⃣ Using Flash Attention in PyTorch

> 🎯 **Beginner Note:** You don't need to implement Flash Attention yourself! PyTorch 2.0+ has it **built right in**. When you call `scaled_dot_product_attention`, PyTorch automatically uses Flash Attention if your GPU supports it. It's like upgrading from a regular oven to a convection oven — same recipe, faster cooking, you just need the right appliance! 🍳

> 🏭 **Production Reality:** Flash Attention is the **default** attention implementation in virtually every modern LLM:
> - **GPT-4** — uses Flash Attention for training & inference
> - **Llama 2/3** — Meta's open models use it throughout
> - **Claude, Gemini, Mistral** — all major foundation models
> - **Hugging Face Transformers** — `attn_implementation="flash_attention_2"` flag
> - Enables **2-4× training speedup** and **128K+ context windows** 🚀

```python
import torch
import torch.nn.functional as F

# PyTorch 2.0+ has Flash Attention built in!
# F.scaled_dot_product_attention automatically uses Flash Attention
# when inputs meet requirements (contiguous, correct dtype, etc.)

B, H, N, D = 4, 32, 8192, 128
q = torch.randn(B, H, N, D, device='cuda', dtype=torch.float16)
k = torch.randn(B, H, N, D, device='cuda', dtype=torch.float16)
v = torch.randn(B, H, N, D, device='cuda', dtype=torch.float16)

# This automatically dispatches to Flash Attention!
output = F.scaled_dot_product_attention(q, k, v, is_causal=True)

# Check which backend was used
with torch.backends.cuda.sdp_kernel(
    enable_flash=True, enable_math=False, enable_mem_efficient=False
):
    output_flash = F.scaled_dot_product_attention(q, k, v, is_causal=True)
```

> 💡 **What each parameter means (for beginners):**
> - `B=4`: batch size — processing 4 sequences at once
> - `H=32`: number of attention heads — 32 "perspectives" looking at the text
> - `N=8192`: sequence length — 8,192 tokens (~6,000 words)
> - `D=128`: head dimension — size of each attention "feature"
> - `is_causal=True`: the model can only look at *past* tokens (like reading left to right)
> - `dtype=torch.float16`: using half-precision to save memory

### 🔧 Flash Attention 2 — Even Faster! (Dao, 2023)

> 📚 **Citation:** Dao, T. (2023). "FlashAttention-2: Faster Attention with Better Parallelism and Work Partitioning." *ICLR 2024*.

Flash Attention **2** improves on v1 with:

| Improvement | What Changed | Speedup |
|---|---|---|
| **Better work partitioning** | Parallelize over sequence length, not just batch & heads | ~2× over FA1 |
| **Reduced non-matmul FLOPs** | Fewer rescaling operations in the inner loop | ~10% faster |
| **Better GPU occupancy** | More thread blocks active simultaneously | Higher utilization |
| **Supports more features** | Head dimensions up to 256, GQA/MQA support | Broader applicability |

```python
# Using Flash Attention 2 via Hugging Face 🤗
from transformers import AutoModelForCausalLM

model = AutoModelForCausalLM.from_pretrained(
    "meta-llama/Llama-2-7b-hf",
    attn_implementation="flash_attention_2",  # ← Enable FA2!
    torch_dtype=torch.float16,
    device_map="auto"
)
```

### 🧩 Block-Sparse Flash Attention

> 🎯 **Analogy:** Regular Flash Attention reads every chapter of the book. Block-sparse Flash Attention has a **table of contents** that says "skip chapters 3, 7, and 12 — they're not relevant." Even faster! 📑

For many tasks, not all tokens need to attend to all other tokens. Block-sparse attention applies a **mask** at the block level:

$$\text{Attention}_{\text{sparse}}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}} + \text{BlockMask}\right) V$$

where BlockMask is $-\infty$ for blocks to skip and $0$ for blocks to compute.

**Speedup:** Proportional to sparsity — if only 50% of blocks are non-zero, you get ~2× additional speedup on top of Flash Attention!

> 📚 **Citation:** Rabe, M.N. & Staats, C. (2021). "Self-attention Does Not Need $O(n^2)$ Memory." *arXiv:2112.05682*. (Independently discovered memory-efficient attention with similar ideas.)

---

# 5️⃣ Memory Savings

> 🎯 **Beginner Analogy:** Think of it as **compression for computation**. Standard attention is like carrying the *entire filing cabinet* to your desk to find one document. Flash Attention is like having an assistant bring you *one folder at a time*. The filing cabinet (HBM) stays where it is — you only need desk space (SRAM) for one folder! 🗄️ → 📂

| Sequence Length | Standard Attention Memory | Flash Attention Memory | Savings |
|----------------|--------------------------|----------------------|---------|
| 2,048 | 16 MB | ~1 MB | 16× |
| 8,192 | 256 MB | ~4 MB | 64× |
| 32,768 | 4 GB | ~16 MB | 256× |
| 131,072 | 64 GB | ~64 MB | 1000× |

Flash Attention memory: $O(N)$ instead of $O(N^2)$. This is what enables 128K+ context.

### 📐 Why the Memory Savings?

| What's Stored | Standard Attention | Flash Attention |
|---|---|---|
| $QK^T$ matrix ($N \times N$) | ✅ Full matrix in HBM | ❌ Never created! |
| Softmax output ($N \times N$) | ✅ Full matrix in HBM | ❌ Never stored! |
| Intermediate blocks | N/A | ✅ Tiny blocks in SRAM |
| Final output $O$ ($N \times d$) | ✅ | ✅ |
| **Peak memory** | $O(N^2)$ 💥 | $O(N)$ ✅ |

> ⭐ **Production impact:** This is why GPT-4 can read your entire codebase at once (128K tokens), while GPT-3 could only handle ~4K tokens. Same architecture, but Flash Attention removes the memory wall!

---

# 6️⃣ Gradient Checkpointing

> 🎯 **Beginner Analogy — The Bookmark Strategy:**
> Imagine reading a choose-your-own-adventure book 📚. Normally, you'd put a **sticky note on every page** you visit (saving all activations) so you can trace back your path. That uses a LOT of sticky notes! **Gradient checkpointing** = you only put sticky notes on **every 5th page**. When you need to trace back, you re-read those few pages in between. Fewer sticky notes (less memory), but a bit more re-reading (more compute). Worth it!

Trade compute for memory during backward pass:

Standard: Store all intermediate activations → high memory
Checkpointing: Only store inputs at checkpoints → recompute during backward

```python
from torch.utils.checkpoint import checkpoint

class TransformerWithCheckpointing(nn.Module):
    def __init__(self, num_layers=32):
        super().__init__()
        self.layers = nn.ModuleList([TransformerBlock() for _ in range(num_layers)])
    
    def forward(self, x):
        for layer in self.layers:
            # Recompute this layer's forward pass during backward
            x = checkpoint(layer, x, use_reentrant=False)
        return x
```

Memory reduction: From $O(L \times N \times d)$ to $O(\sqrt{L} \times N \times d)$ with selective checkpointing.
Cost: ~33% more compute (recomputing forward passes).

### 📊 Gradient Checkpointing Trade-off

| Strategy | Memory | Compute Overhead | Use Case |
|---|---|---|---|
| No checkpointing | $O(L \cdot N \cdot d)$ 💥 | 0% | Small models, lots of GPU RAM |
| Every-layer checkpointing | $O(N \cdot d)$ ✅ | +33% | Large models, limited RAM |
| Selective (every $k$ layers) | $O(k \cdot N \cdot d)$ | +~20% | ⭐ Best trade-off for production |

---

# 7️⃣ Activation Memory Optimization

> 🎯 **Beginner Analogy:** During a road trip 🚗, you take photos (activations) at every stop. Three strategies to save phone storage:
> 1. **Gradient checkpointing** = Only keep photos at major landmarks; retake the rest later
> 2. **CPU offloading** = Upload photos to the cloud (CPU RAM) and free phone space (GPU); download when needed
> 3. **Lower precision** = Save photos as JPEGs instead of RAW files — smaller but still looks good!

```python
# 1. Gradient checkpointing (above)
# 2. Activation offloading to CPU
class CPUOffloadLayer(nn.Module):
    def forward(self, x):
        # Save activation on CPU during forward
        self.saved = x.cpu()
        return self.actual_forward(x)
    
    def backward_hook(self):
        # Load back to GPU for backward
        return self.saved.cuda()

# 3. Selective precision for activations
# Store activations in FP16 even when computing in FP32
```

---

# 8️⃣ Complete Memory-Efficient Training Recipe

> 🎯 **Beginner Analogy — The Ultimate Kitchen Strategy 👨‍🍳:**
> Imagine you're cooking a huge banquet in a tiny kitchen. You combine EVERY trick:
> 1. **Mixed precision** = Use smaller bowls where you can (saves counter space)
> 2. **Gradient checkpointing** = Don't keep every intermediate dish out; remake some when needed
> 3. **Gradient accumulation** = Cook 4 small batches pretending it's 1 big batch
> 4. **Gradient clipping** = Don't let any single ingredient overpower the dish
>
> This is exactly what production LLM training looks like! 🏭

```python
def train_memory_efficient(model, loader, epochs, device='cuda'):
    """Production-grade memory-efficient training."""
    # 1. Mixed precision
    model = model.to(device)
    
    # 2. Gradient checkpointing
    model.gradient_checkpointing_enable()
    
    # 3. Optimizer with memory savings
    optimizer = torch.optim.AdamW(model.parameters(), lr=1e-4)
    
    # 4. Gradient accumulation (simulate larger batches)
    accumulation_steps = 4
    
    for epoch in range(epochs):
        for i, (x, y) in enumerate(loader):
            x, y = x.to(device), y.to(device)
            
            with torch.cuda.amp.autocast(dtype=torch.bfloat16):
                loss = model(x, y) / accumulation_steps
            
            loss.backward()
            
            if (i + 1) % accumulation_steps == 0:
                torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
                optimizer.step()
                optimizer.zero_grad()
```

> 💡 **Line-by-line for beginners:**
> - `model.gradient_checkpointing_enable()` → Trades ~33% more compute for ~60% less memory
> - `torch.cuda.amp.autocast(dtype=torch.bfloat16)` → Uses 16-bit numbers instead of 32-bit (half the memory, nearly same accuracy)
> - `loss / accumulation_steps` → Averages gradients over 4 mini-batches, simulating 4× batch size
> - `clip_grad_norm_(_, 1.0)` → Prevents "exploding gradients" that can crash training

---

# 9️⃣ Memory Budget Estimation

> 🎯 **Beginner Analogy:** Before moving into an apartment, you estimate: "Bed takes X square feet, desk takes Y, couch takes Z — will it all fit?" This function does the same for GPU memory! 🏠

```python
def estimate_memory(num_params_B, seq_len, batch_size, num_layers, hidden_dim):
    """Estimate GPU memory needs in GB."""
    params_mem = num_params_B * 1e9 * 2 / 1e9       # FP16 weights
    master_weights = num_params_B * 1e9 * 4 / 1e9    # FP32 master
    gradients_mem = num_params_B * 1e9 * 2 / 1e9     # FP16 gradients
    optimizer_mem = num_params_B * 1e9 * 8 / 1e9     # Adam states (2 FP32)
    
    # Activation memory (per layer, without checkpointing)
    act_per_layer = batch_size * seq_len * hidden_dim * 2 / 1e9 * 12  # ~12 tensors
    activations = act_per_layer * num_layers
    
    total = params_mem + master_weights + gradients_mem + optimizer_mem + activations
    
    with_checkpoint = params_mem + master_weights + gradients_mem + optimizer_mem + act_per_layer * 2
    
    print(f"Without checkpointing: {total:.1f} GB")
    print(f"With checkpointing: {with_checkpoint:.1f} GB")
    return total, with_checkpoint

estimate_memory(7, seq_len=2048, batch_size=4, num_layers=32, hidden_dim=4096)
```

### 📊 Memory Breakdown for a 7B Model (Example)

| Component | Memory (GB) | % of Total | Notes |
|---|---|---|---|
| FP16 Weights | 14 GB | 12% | $7B \times 2$ bytes |
| FP32 Master Weights | 28 GB | 24% | For mixed-precision training |
| FP16 Gradients | 14 GB | 12% | Same size as weights |
| Optimizer States (Adam) | 56 GB | 48% | 2 FP32 states per param! |
| Activations | ~5 GB | 4% | With checkpointing + Flash Attn |
| **Total** | **~117 GB** | | Needs multi-GPU or ZeRO! |

> ⭐ **Key insight:** Optimizer states dominate memory, not activations! Flash Attention solves the activation problem, but you still need techniques like ZeRO (Day 21) for the rest.

---

# 📝 Summary

| Technique | Memory Savings | Compute Cost |
|-----------|---------------|-------------|
| Flash Attention | $O(N^2) \to O(N)$ for attention | Same or faster ⚡ |
| Gradient Checkpointing | ~60% activation reduction | +33% compute |
| Mixed Precision (BF16) | ~50% weights + activations | Faster! 🚀 |
| Gradient Accumulation | Simulate large batch | None |
| Activation Offloading | Move to CPU RAM | +transfer time |

---

# 🧠 Flash Attention Cheat Sheet (Tear-Out Reference)

```
┌──────────────────────────────────────────────────────────────┐
│               ⚡ FLASH ATTENTION CHEAT SHEET ⚡               │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  WHAT: IO-aware exact attention that avoids N×N matrix       │
│  WHY:  Standard attention is memory-bound, not compute-bound │
│  HOW:  Tiling + Online Softmax                               │
│                                                              │
│  FORMULA:                                                    │
│  Attention(Q,K,V) = softmax(QKᵀ/√d)·V                       │
│  (Same math, different memory access pattern)                │
│                                                              │
│  COMPLEXITY:                                                 │
│  ┌───────────────┬──────────────┬──────────────────┐         │
│  │               │  Standard    │  Flash            │         │
│  ├───────────────┼──────────────┼──────────────────┤         │
│  │  Memory       │  O(N²)      │  O(N)      ⭐     │         │
│  │  HBM Access   │  O(N²+Nd)   │  O(N²d²/M)       │         │
│  │  FLOPs        │  O(N²d)     │  O(N²d)   same!   │         │
│  └───────────────┴──────────────┴──────────────────┘         │
│                                                              │
│  KEY IDEAS:                                                  │
│  1. Never materialize N×N matrix ❌                          │
│  2. Tile Q,K,V into blocks that fit in SRAM 🍕              │
│  3. Online softmax rescales with e^(m_old - m_new) 📊       │
│  4. Write ONLY final output O to HBM 📝                     │
│                                                              │
│  IN PYTORCH:                                                 │
│  F.scaled_dot_product_attention(q, k, v, is_causal=True)     │
│                                                              │
│  IN HUGGING FACE:                                            │
│  model = AutoModel.from_pretrained(name,                     │
│      attn_implementation="flash_attention_2")                │
│                                                              │
│  SPEEDUP: 2-4× wall-clock, enables 128K+ context            │
└──────────────────────────────────────────────────────────────┘
```

---

# 🗺️ Flash Attention — The Big Picture (How Everything Connects)

```
  The Problem                    The Solution                  The Result
  ──────────                     ────────────                  ──────────
  
  Standard Attention             Flash Attention               Modern LLMs
  ┌─────────────┐               ┌─────────────┐              ┌───────────────┐
  │ QKᵀ = N×N   │──memory──→    │ Tiling into  │──enables──→ │ GPT-4 (128K)  │
  │ matrix 💥   │  wall!        │ small blocks │              │ Llama 3       │
  │ O(N²) mem   │               │ in SRAM 🍕   │              │ Claude        │
  └─────────────┘               │ + Online     │              │ Gemini        │
                                │ Softmax 📊   │              │ Mistral       │
                                │ O(N) memory  │              └───────────────┘
                                └─────────────┘
                                       │
                                       ├── Flash Attention 1 (Dao et al., 2022)
                                       ├── Flash Attention 2 (Dao, 2023)  
                                       ├── Block-Sparse variants
                                       └── Ring Attention (for multi-GPU)
```

---

# 📚 Resources — Go Deeper!

### 📄 Essential Papers
| Paper | Authors | Year | Key Contribution |
|---|---|---|---|
| **FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness** | Dao, Fu, Ermon, Rudra, Ré | 2022 | The original Flash Attention paper — introduced tiling + online softmax for exact attention |
| **FlashAttention-2: Faster Attention with Better Parallelism and Work Partitioning** | Dao | 2023 | 2× faster than FA1, better GPU utilization, supports more head dimensions |
| **Self-attention Does Not Need $O(n^2)$ Memory** | Rabe & Staats | 2021 | Independently showed memory-efficient exact attention is possible |
| **Online normalizer calculation for softmax** | Milakov & Gimelshein | 2018 | The online softmax trick that enables Flash Attention's tiling |
| **Efficient Transformers: A Survey** | Tay, Dehghani, Bahri, Metzler | 2022 | Comprehensive survey of all efficient attention methods |

### 📖 Books
| Book | Author(s) | Why Read It |
|---|---|---|
| **Efficient Deep Learning** | Menghani | Covers Flash Attention, quantization, pruning — all systems optimization |
| **Deep Learning Systems** | Xiao et al. | GPU memory hierarchy, kernel optimization, systems-level thinking |
| **Programming Massively Parallel Processors** | Kirk & Hwu | CUDA, GPU architecture, SRAM/HBM details — understand *why* Flash Attention works |

### 🔗 Online Resources
- 🌐 **Tri Dao's Blog** — The creator of Flash Attention explains the algorithm in plain English
- 🌐 **PyTorch SDPA Documentation** — Official docs for `scaled_dot_product_attention`
- 🌐 **Flash Attention GitHub** — `Dao-AILab/flash-attention` — source code, benchmarks, and installation
- 🌐 **Hugging Face Flash Attention 2 Integration Guide** — How to enable FA2 in any HF model
- 🌐 **Aleksa Gordic's "Flash Attention Explained"** — Excellent visual walkthrough on YouTube

### 🎓 Prerequisites (What to study first)
| Topic | Why You Need It | Where in This Course |
|---|---|---|
| Matrix Multiplication | Understanding $QK^T$ and $PV$ | Phase 1, Day 2 |
| Softmax Function | The core operation Flash Attention optimizes | Phase 3 |
| GPU Architecture (SRAM vs HBM) | Understanding *why* IO-awareness matters | Phase 5, Day 19 |
| Transformer Attention | The standard attention mechanism Flash Attention replaces | Phase 4 |

---

**Tomorrow**: Triton — Writing Custom GPU Kernels in Python.
