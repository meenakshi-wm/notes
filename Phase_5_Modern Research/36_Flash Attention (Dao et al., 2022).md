📘 DAY 36 — Paper Reproduction: Flash Attention (Dao et al., 2022)

Goal:

Understand and reproduce the Flash Attention algorithm — the IO-aware attention implementation that reduces memory from O(N²) to O(N) and speeds up training by 2-4x.

---

# 🌟 Beginner's Big Picture

> **What is Attention?** In AI language models, every word needs to "look at" every other word to understand context. If your sentence has $N$ words, standard attention builds an $N \times N$ table of word-to-word scores. That table grows *quadratically* — double the words, quadruple the memory!

⭐ **One-sentence summary**: Flash Attention computes the *exact same result* as standard attention but **never writes the giant $N \times N$ table to slow memory** — it works on small tiles, one piece at a time.

### 🎯 Analogy: The Multiplication Table

| Approach | Analogy | Memory |
|----------|---------|--------|
| 🐢 Standard Attention | Write the **entire** 100×100 multiplication table on a huge sheet of paper, *then* look up entries | $O(N^2)$ — need the whole table |
| ⚡ Flash Attention | Compute each row of the table in your head, write down only the final answer per row — **never write the full table** | $O(N)$ — only need one row at a time |

### 🏗️ Why This Matters in Production

> Flash Attention is used in **GPT-4, Claude, LLaMA, Gemini**, and virtually every modern LLM. It is the reason models can process **64K–128K+ token context windows** on a single GPU. Available out-of-the-box via `torch.nn.functional.scaled_dot_product_attention` (PyTorch ≥ 2.0).

---

# 1️⃣ Paper Summary

| Aspect | Detail |
|--------|--------|
| Problem | Standard attention requires O(N²) memory |
| Key insight | Attention is memory-bandwidth-bound, not compute-bound |
| Solution | Tiled computation with online softmax |
| Result | 2-4x faster, enables much longer sequences |

---

# 2️⃣ Why Standard Attention Is Slow

Standard: S = QK^T → P = softmax(S) → O = PV

The N×N matrices S and P are materialized in GPU HBM.
For N=8192, d=128: S = 8192² × 4 bytes = 256 MB per head.

But HBM bandwidth is the bottleneck, not compute!
Computing QK^T is fast; reading/writing the N×N matrix is slow.

### 📐 Math: Standard Attention

Given queries $Q$, keys $K$, values $V \in \mathbb{R}^{N \times d}$:

$$\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^T}{\sqrt{d}}\right) V$$

- The matrix $S = QK^T$ is $N \times N$ → storing it costs $O(N^2)$ memory.
- **Total HBM accesses** (reads + writes) for standard attention: $\Theta(Nd + N^2)$ (Dao et al., 2022, Theorem 1).

### 🏛️ Analogy: HBM vs SRAM = Library Stacks vs Your Reading Desk

| GPU Memory | Analogy | Size | Speed |
|------------|---------|------|-------|
| 🗄️ **HBM** (High Bandwidth Memory) | Library stacks — huge but you must walk to get each book | 40–80 GB | ~2 TB/s |
| 🪑 **SRAM** (on-chip cache) | Your reading desk — tiny but instantly accessible | ~20 MB | ~19 TB/s |

> ⭐ Standard attention writes the entire $N \times N$ table to the *library stacks* (HBM). Flash Attention keeps intermediate results on the *reading desk* (SRAM) and only writes the final answer back.

---

# 3️⃣ Flash Attention Algorithm

Tile Q, K, V into blocks. Process block by block, never materializing the full N×N matrix.

### 📐 Core Ideas

**1. Tiling** 🧩 — Split $Q$ into blocks of $B_r$ rows and $K, V$ into blocks of $B_c$ rows. Each tile computes a small $B_r \times B_c$ piece of the attention matrix in fast SRAM.

> ⭐ **Analogy: Jigsaw Puzzle** — Instead of laying out the entire 10,000-piece puzzle on the floor, you solve it **section by section** on a small desk, gluing finished sections together as you go.

**2. Online Softmax Trick** 📊 — Softmax needs the *maximum* over all scores, but we only see one block at a time. The trick (Milakov & Gimelshein, 2018; Rabe & Staats, 2021): keep a **running maximum** $m$ and **running sum** $\ell$, and rescale previous results when a new block has a larger max.

> ⭐ **Analogy: Grading Papers** — Imagine grading exams one by one and maintaining a running class average. When you see a new score, you update the average on-the-fly — you never need all scores at once.

Mathematically, for block $j$:

$$m^{(j)} = \max(m^{(j-1)},\; \max(S_{ij}))$$
$$\ell^{(j)} = e^{m^{(j-1)} - m^{(j)}} \ell^{(j-1)} + \sum_k e^{S_{ijk} - m^{(j)}}$$
$$O^{(j)} = \frac{e^{m^{(j-1)} - m^{(j)}} \ell^{(j-1)} O^{(j-1)} + e^{S_{ij} - m^{(j)}} V_j}{\ell^{(j)}}$$

**3. IO Complexity** ⚡ — Flash Attention reduces HBM accesses to:

$$\Theta\!\left(\frac{N^2 d^2}{M}\right)$$

where $M$ = SRAM size. Since $M \gg d^2$ on modern GPUs, this is a **huge** reduction over the standard $\Theta(Nd + N^2)$ (Dao et al., 2022, Theorem 2).

```python
def flash_attention_reference(Q, K, V, block_size=256):
    """Reference implementation of Flash Attention (simplified)."""
    B, H, N, d = Q.shape
    O = torch.zeros_like(Q)
    l = torch.zeros(B, H, N, 1, device=Q.device)  # Normalizer
    m = torch.full((B, H, N, 1), -float('inf'), device=Q.device)  # Running max
    
    scale = 1.0 / math.sqrt(d)
    
    # Iterate over K, V blocks
    for j_start in range(0, N, block_size):
        j_end = min(j_start + block_size, N)
        K_j = K[:, :, j_start:j_end, :]  # (B, H, Bk, d)
        V_j = V[:, :, j_start:j_end, :]  # (B, H, Bk, d)
        
        # Iterate over Q blocks
        for i_start in range(0, N, block_size):
            i_end = min(i_start + block_size, N)
            Q_i = Q[:, :, i_start:i_end, :]  # (B, H, Bq, d)
            
            # Compute local attention scores
            S_ij = Q_i @ K_j.transpose(-2, -1) * scale  # (B, H, Bq, Bk)
            
            # Online softmax update
            m_ij = S_ij.max(dim=-1, keepdim=True).values  # Local max
            m_old = m[:, :, i_start:i_end, :]
            m_new = torch.maximum(m_old, m_ij)
            
            # Rescale old accumulator
            alpha = torch.exp(m_old - m_new)
            beta = torch.exp(m_ij - m_new)
            
            P_ij = torch.exp(S_ij - m_ij) * beta  # Numerically stable
            
            l_old = l[:, :, i_start:i_end, :]
            l_new = alpha * l_old + P_ij.sum(dim=-1, keepdim=True)
            
            O_old = O[:, :, i_start:i_end, :]
            O[:, :, i_start:i_end, :] = (alpha * O_old * l_old + P_ij @ V_j) / l_new
            
            m[:, :, i_start:i_end, :] = m_new
            l[:, :, i_start:i_end, :] = l_new
    
    return O
```

---

# 4️⃣ Verification: Compare with Standard Attention

```python
def standard_attention(Q, K, V):
    """Standard O(N²) attention for reference."""
    scale = 1.0 / math.sqrt(Q.shape[-1])
    S = Q @ K.transpose(-2, -1) * scale
    P = torch.softmax(S, dim=-1)
    return P @ V

# Test correctness
B, H, N, d = 2, 8, 1024, 64
Q = torch.randn(B, H, N, d, device='cuda')
K = torch.randn(B, H, N, d, device='cuda')
V = torch.randn(B, H, N, d, device='cuda')

out_standard = standard_attention(Q, K, V)
out_flash = flash_attention_reference(Q, K, V, block_size=128)

max_diff = (out_standard - out_flash).abs().max()
print(f"Max difference: {max_diff:.6e}")  # Should be < 1e-5
assert max_diff < 1e-4, "Flash Attention output doesn't match!"
```

---

# 5️⃣ Memory Analysis

```python
def memory_comparison(seq_lengths, d=128, n_heads=32, dtype_bytes=2):
    """Compare memory usage of standard vs flash attention."""
    print(f"{'Seq Len':>10} {'Standard':>12} {'Flash':>12} {'Savings':>10}")
    
    for N in seq_lengths:
        # Standard: store QKT (N×N) + P (N×N) + O (N×d)
        standard_mem = (N * N * 2 + N * d) * n_heads * dtype_bytes
        
        # Flash: store O (N×d) + l,m (N×1 each)
        flash_mem = (N * d + N * 2) * n_heads * dtype_bytes
        
        print(f"{N:>10} {standard_mem/1e9:>10.2f}GB {flash_mem/1e6:>10.1f}MB {standard_mem/flash_mem:>8.0f}x")

memory_comparison([2048, 4096, 8192, 16384, 32768, 65536, 131072])
```

---

# 6️⃣ Causal Masking in Flash Attention

For autoregressive models, apply causal mask within the tiled computation:

```python
def flash_attention_causal(Q, K, V, block_size=256):
    """Flash Attention with causal masking."""
    B, H, N, d = Q.shape
    O = torch.zeros_like(Q)
    l = torch.zeros(B, H, N, 1, device=Q.device)
    m = torch.full((B, H, N, 1), -float('inf'), device=Q.device)
    scale = 1.0 / math.sqrt(d)
    
    for j_start in range(0, N, block_size):
        j_end = min(j_start + block_size, N)
        K_j = K[:, :, j_start:j_end, :]
        V_j = V[:, :, j_start:j_end, :]
        
        for i_start in range(0, N, block_size):
            i_end = min(i_start + block_size, N)
            
            # Skip blocks entirely above diagonal
            if i_start + block_size <= j_start:
                # All positions in Q block are before K block → skip
                continue
            
            Q_i = Q[:, :, i_start:i_end, :]
            S_ij = Q_i @ K_j.transpose(-2, -1) * scale
            
            # Apply causal mask within this block
            i_indices = torch.arange(i_start, i_end, device=Q.device)
            j_indices = torch.arange(j_start, j_end, device=Q.device)
            causal_mask = i_indices[:, None] >= j_indices[None, :]
            S_ij = S_ij.masked_fill(~causal_mask, -float('inf'))
            
            # Online softmax update (same as before)
            # ...
    
    return O
```

---

# 7️⃣ Using PyTorch's Built-in Flash Attention

```python
# PyTorch 2.0+ automatically uses Flash Attention
import torch.nn.functional as F

output = F.scaled_dot_product_attention(
    Q, K, V,
    is_causal=True,
    # Automatically dispatches to Flash Attention v2 when available
)

# Benchmark
import time

for N in [1024, 4096, 16384]:
    Q = torch.randn(1, 32, N, 128, device='cuda', dtype=torch.float16)
    K = torch.randn(1, 32, N, 128, device='cuda', dtype=torch.float16)
    V = torch.randn(1, 32, N, 128, device='cuda', dtype=torch.float16)
    
    torch.cuda.synchronize()
    start = time.perf_counter()
    for _ in range(100):
        _ = F.scaled_dot_product_attention(Q, K, V, is_causal=True)
    torch.cuda.synchronize()
    elapsed = (time.perf_counter() - start) / 100
    print(f"N={N}: {elapsed*1000:.2f} ms")
```

---

# 📝 Summary

| Aspect | Standard | Flash Attention |
|--------|----------|----------------|
| Memory | O(N²) | O(N) |
| Speed | Baseline | 2-4x faster |
| Max sequence | ~4K (80GB) | 128K+ (80GB) |
| Key technique | Materialize full matrices | Tiled + online softmax |
| Backward | Standard autograd | Custom backward (recompute) |

---

# 🏭 Production Scenarios

| System | How Flash Attention Helps |
|--------|---------------------------|
| 🤖 **GPT-4 / Claude / Gemini** | Enables 128K+ context windows for long-document understanding |
| 🦙 **LLaMA / Mistral fine-tuning** | 2-4x faster training on consumer GPUs; fits longer sequences in VRAM |
| 🔍 **RAG pipelines** | Retrieve & attend over many chunks without OOM errors |
| 💬 **Chat applications** | Maintain long conversation history within a single forward pass |
| 📱 **On-device inference** | Reduced memory lets smaller GPUs / edge devices run longer contexts |

```python
# Using Flash Attention in production (PyTorch ≥ 2.0)
import torch, torch.nn.functional as F

with torch.backends.cuda.sdp_kernel(enable_flash=True, enable_math=False, enable_mem_efficient=False):
    out = F.scaled_dot_product_attention(Q, K, V, is_causal=True)
```

---

# 📋 Cheat Sheet

| Symbol | Meaning |
|--------|---------|
| $N$ | Sequence length (number of tokens) |
| $d$ | Head dimension (typically 64 or 128) |
| $M$ | SRAM size (on-chip fast memory, ~20 MB) |
| $B_r, B_c$ | Block sizes for Q-rows and K/V-rows in tiling |
| $m$ | Running maximum for online softmax |
| $\ell$ | Running sum of exponentials (softmax denominator) |
| HBM | High Bandwidth Memory — large but slow GPU RAM |
| SRAM | Static RAM — tiny but ultra-fast on-chip cache |

**Key Complexity Comparison**:

| | Standard Attention | Flash Attention |
|---|---|---|
| Memory | $O(N^2)$ | $O(N)$ |
| HBM accesses | $\Theta(Nd + N^2)$ | $\Theta(N^2 d^2 M^{-1})$ |
| Exact output? | ✅ Yes | ✅ Yes (mathematically identical) |

---

# 📚 Research Citations

| Paper | Key Contribution |
|-------|------------------|
| ⭐ **Dao et al. (2022)** — *FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness* | Original algorithm; IO-complexity analysis; tiling + online softmax |
| ⭐ **Dao (2023)** — *FlashAttention-2: Faster Attention with Better Parallelism and Work Partitioning* | 2x speedup over v1 via improved thread-block scheduling & reduced non-matmul FLOPs |
| **Rabe & Staats (2021)** — *Self-Attention Does Not Need O(N²) Memory* | Showed online softmax enables O(1) memory attention; theoretical foundation for Flash |
| **Milakov & Gimelshein (2018)** — *Online Normalizer Calculation for Softmax* | Numerically stable online softmax trick used inside Flash Attention |
| **Vaswani et al. (2017)** — *Attention Is All You Need* | Original Transformer and scaled dot-product attention |

---

# 📖 Resources

### Books
- 📕 *Efficient Transformers: A Survey* — Tay et al. (2020) — comprehensive overview of efficient attention methods
- 📕 *Deep Learning* — Goodfellow, Bengio, Courville — foundational background on neural networks & optimization

### Papers & Code
- 📄 [FlashAttention paper (arXiv:2205.14135)](https://arxiv.org/abs/2205.14135)
- 📄 [FlashAttention-2 paper (arXiv:2307.08691)](https://arxiv.org/abs/2307.08691)
- 📄 [Rabe & Staats (arXiv:2112.05682)](https://arxiv.org/abs/2112.05682)
- 💻 [Flash Attention GitHub — Dao-AILab/flash-attention](https://github.com/Dao-AILab/flash-attention)
- 📝 [Tri Dao's blog on Flash Attention](https://tridao.me/blog/)
- 📘 [PyTorch SDPA docs](https://pytorch.org/docs/stable/generated/torch.nn.functional.scaled_dot_product_attention.html)

---

**Tomorrow**: Reproducing DPO (Direct Preference Optimization).
