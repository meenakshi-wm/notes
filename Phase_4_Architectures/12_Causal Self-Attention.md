📘 DAY 12 — Causal Self-Attention Deep Dive & KV Cache Foundations 🧠🔮

> *"The secret of GPT isn't that it understands language — it's that it writes one word at a time, never peeking ahead."*

---

## 🌟 Why This Matters (The Big Picture for Beginners)

Imagine you're writing a story, but with one strict rule: **you can only ever look at what you've already written** — never peek ahead at what comes next. That's exactly how ChatGPT, GPT-4, Claude, and every autoregressive language model works!

This single constraint — called **causal self-attention** — is the beating heart of modern AI text generation. Today we'll learn:

| 🎯 What You'll Learn | 🍕 Pizza Analogy |
|---|---|
| Why models can't see the future | Like reading a book left-to-right, no skipping ahead |
| How the causal mask works | A "no cheating" shield on a test |
| KV-cache optimization | Remembering answers you already figured out |
| MQA / GQA attention | Sharing notes efficiently in a study group |
| Training vs Inference | Practice exam (see all answers) vs real exam (one question at a time) |

⭐ **Beginner-friendly rule of thumb**: If someone says "autoregressive," just think **"writes one word at a time, only looking backward."**

---

## 🎯 Goal:

Master the autoregressive property of GPT-style models — WHY each token can only see previous tokens, HOW the causal mask works during training vs inference, and the critical optimization of KV-caching that makes generation feasible. This bridges the gap between training and inference.

## ✅ If this is deeply understood:
You'll understand the fundamental difference between training and inference in LLMs, why generation is slow (and how KV-cache fixes it), and the autoregressive assumption that both enables and limits these models.

---

# 1️⃣ Training vs Inference: Two Different Worlds 🌍↔️🌎

> 🍕 **Beginner Analogy**: Think of training like a **practice test where you have the answer key** — you can check all answers at once. Inference (generation) is like **the real exam** — you answer one question at a time, and each answer might depend on your previous ones.

## Training: Teacher Forcing (Parallel)

During training, we have the COMPLETE sequence: [The, cat, sat, on, the, mat]

We process ALL positions simultaneously:
- Position 0: predicts "cat" given "The"
- Position 1: predicts "sat" given "The, cat"
- Position 2: predicts "on" given "The, cat, sat"
...

This is possible because of the CAUSAL MASK — we don't need to actually generate sequentially.

One forward pass gives us loss at ALL positions. Extremely efficient.

⭐ **Why this is genius**: Instead of training the model by making it generate one word at a time (slow!), we show it the full sentence but use a clever "mask" that prevents cheating. It's like giving a student the full test paper but covering future questions with a sheet of paper — they can only see the questions they've already answered.

## Inference: Autoregressive (Sequential)

During generation, we don't know future tokens.

Step 1: Input "The" → predict "cat"
Step 2: Input "The cat" → predict "sat"
Step 3: Input "The cat sat" → predict "on"
...

Each step requires a FULL forward pass. Generation is O(n) forward passes for n tokens.

> 🧠 **Real-World Feel**: When you type in ChatGPT and see words appearing one at a time — that's autoregressive generation happening live! Each word is a separate computation step.

### ⚡ Training vs Inference at a Glance

| Aspect | 🏋️ Training | 🚀 Inference (Generation) |
|--------|----------|----------------------|
| Sees full sequence? | ✅ Yes (with mask) | ❌ No — builds token by token |
| Parallelizable? | ✅ All positions at once | ❌ Sequential (one token at a time) |
| Uses causal mask? | ✅ Prevents peeking ahead | Not needed (future doesn't exist yet) |
| Speed | 🟢 Fast (GPU parallel) | 🔴 Slow (sequential bottleneck) |
| Analogy | Open-book practice test | Closed-book real exam |

---

# 2️⃣ The Causal Mask: Mathematics 🎭🔢

> 🍕 **Beginner Analogy — "No Peeking on a Test"**: Imagine you're taking a test where each answer depends on your previous answers. The teacher gives you ALL questions at once (efficient!), but covers future questions with a sheet of paper. You physically **cannot** see question 5 while answering question 3. The causal mask is that sheet of paper — it blocks the model from "seeing" future tokens.

## Mask Construction

For sequence length $T$:

$$
M[i,j] = \begin{cases}
0 & \text{if } j \leq i \quad \text{(allowed: current or past)} \\
-\infty & \text{if } j > i \quad \text{(blocked: future positions)}
\end{cases}
$$

Applied to attention scores BEFORE softmax:

$$
\text{scores\_masked} = \text{scores} + M
$$

$$
\text{attn\_weights} = \text{softmax}(\text{scores\_masked})
$$

When $M[i,j] = -\infty$: softmax produces 0 weight → token $i$ can't see token $j$.

⭐ **Why $-\infty$?** Because $e^{-\infty} = 0$. When softmax computes $\frac{e^{x_j}}{\sum e^{x_k}}$, any position with $-\infty$ contributes zero to the numerator. It's as if that position doesn't exist! It's mathematically elegant — no `if` statements needed, just add $-\infty$ and let the math handle it. 🎯

> 💡 **Think of it as**: The full masked attention equation is:
> 
> $$\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^\top + M}{\sqrt{d_k}}\right)V$$
> 
> where $M$ is the causal mask matrix, $d_k$ is the dimension of each key vector, and the $\sqrt{d_k}$ scaling prevents the dot products from getting too large (which would make softmax too "peaky").

### 🔍 Visual: What the Mask Looks Like

```
Token:      The   cat   sat   on
         ┌─────┬─────┬─────┬─────┐
  The    │  ✅  │  🚫  │  🚫  │  🚫  │  ← "The" can only see itself
  cat    │  ✅  │  ✅  │  🚫  │  🚫  │  ← "cat" sees "The" + itself
  sat    │  ✅  │  ✅  │  ✅  │  🚫  │  ← "sat" sees all before + itself
  on     │  ✅  │  ✅  │  ✅  │  ✅  │  ← "on" sees everything before + itself
         └─────┴─────┴─────┴─────┘
  
  ✅ = 0 (allowed)    🚫 = -∞ (blocked)
```

This is a **lower triangular** matrix — the shape of a staircase going down-right!

```python
import torch

def create_causal_mask(seq_len):
    """Create causal attention mask."""
    # Lower triangular matrix
    mask = torch.tril(torch.ones(seq_len, seq_len))
    # Convert 0s to -inf
    mask = mask.masked_fill(mask == 0, float('-inf'))
    mask = mask.masked_fill(mask == 1, 0.0)
    return mask

# For seq_len=4:
# [[  0, -inf, -inf, -inf],
#  [  0,    0, -inf, -inf],
#  [  0,    0,    0, -inf],
#  [  0,    0,    0,    0]]
```

## Why This Works with Teacher Forcing

Key insight: the causal mask means position $i$'s output ONLY depends on positions $0..i$.

So even though we compute ALL positions in parallel, each position's prediction is equivalent to what we'd get if we ran the model autoregressively up to that point.

```
Training:   All positions computed in ONE forward pass (causal mask ensures correctness)
Inference:  Positions computed ONE AT A TIME (no mask needed — future doesn't exist yet)
```

> 🎯 **Why this matters in production**: During training, this parallelism means you can train on entire documents at once instead of word-by-word. A training run that would take years sequentially takes weeks in parallel. This is literally what made GPT-scale models possible. (Vaswani et al., 2017 — "Attention Is All You Need" 📄)

---

# 3️⃣ KV-Cache: The Critical Inference Optimization 💾⚡

> 🍕 **Beginner Analogy — "Remembering What You Already Thought About"**: Imagine you're writing an essay by hand. Every time you write a new sentence, you have to **re-read the entire essay from the beginning** to remember the context. That's insanely wasteful! Instead, you could just **keep notes** about what you've already written, and only think about the new sentence. That's KV-cache — it saves the model's "thoughts" (Keys and Values) about previous tokens so it doesn't re-compute them.

## The Problem: Redundant Computation

At step $t$ of generation:
- Input: $[\text{token}_0, \text{token}_1, \ldots, \text{token}_{t-1}, \text{token}_t]$
- We need attention for position $t$
- This requires $K$ and $V$ for ALL previous positions

Naive approach: recompute $Q$, $K$, $V$ for the ENTIRE sequence at each step.
Step 1: compute KV for 1 token
Step 2: compute KV for 2 tokens (re-computing token 0's KV!)
Step N: compute KV for N tokens (re-computing tokens 0..N-1!)

Total: $O(n^2)$ KV computations. Wasteful!

> ⭐ **Why only cache K and V, not Q?** During generation, we only need the Query for the *current* token (the new one we're generating). But we need Keys and Values for *all* previous tokens to compute attention. Q is cheap (just one token), K and V are expensive (all previous tokens). 🧠

## The Solution: Cache Key and Value Tensors

After computing $K_i$ and $V_i$ for position $i$, SAVE them.
At step $t+1$, only compute $K$ and $V$ for the NEW token, then concatenate with cache.

### 📐 The Math Behind KV-Cache

Without cache (naive), at generation step $t$:

$$Q_t, K_{0:t}, V_{0:t} = \text{Linear}(x_{0:t}) \quad \text{— recompute everything!}$$

With cache:

$$Q_t = W_Q \cdot x_t \quad \text{(only new token)}$$
$$K_t = W_K \cdot x_t \quad \text{(only new token)}$$
$$V_t = W_V \cdot x_t \quad \text{(only new token)}$$
$$K_{0:t} = \text{concat}(\text{cache}_K, K_t)$$
$$V_{0:t} = \text{concat}(\text{cache}_V, V_t)$$
$$\text{Attention}_t = \text{softmax}\!\left(\frac{Q_t \cdot K_{0:t}^\top}{\sqrt{d_k}}\right) V_{0:t}$$

This reduces per-step compute from $O(t \cdot d)$ to $O(d)$ for the projection step! 🚀

```python
class CachedCausalAttention(torch.nn.Module):
    def __init__(self, d_model, num_heads):
        super().__init__()
        self.num_heads = num_heads
        self.head_dim = d_model // num_heads
        self.qkv_proj = torch.nn.Linear(d_model, 3 * d_model, bias=False)
        self.out_proj = torch.nn.Linear(d_model, d_model, bias=False)
    
    def forward(self, x, kv_cache=None):
        """
        x: (B, T, d_model) — during training T = full sequence, during inference T = 1
        kv_cache: tuple of (cached_k, cached_v) or None
        """
        B, T, C = x.shape
        
        # Project
        qkv = self.qkv_proj(x)
        q, k, v = qkv.chunk(3, dim=-1)
        
        q = q.view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        k = k.view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        v = v.view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        
        # KV Cache logic
        if kv_cache is not None:
            cached_k, cached_v = kv_cache
            k = torch.cat([cached_k, k], dim=2)  # Append new K to cache
            v = torch.cat([cached_v, v], dim=2)  # Append new V to cache
        
        # Store updated cache for next step
        new_kv_cache = (k, v)
        
        # Compute attention
        # q: (B, H, T_new, d) — T_new is 1 during generation
        # k: (B, H, T_total, d) — T_total grows each step
        scores = torch.matmul(q, k.transpose(-2, -1)) / (self.head_dim ** 0.5)
        
        # Causal mask (only needed during training; during generation T_new=1)
        if T > 1:
            mask = torch.tril(torch.ones(T, k.shape[2], device=x.device))
            scores = scores.masked_fill(mask == 0, float('-inf'))
        
        attn = torch.softmax(scores, dim=-1)
        out = torch.matmul(attn, v)
        
        out = out.transpose(1, 2).contiguous().view(B, T, C)
        return self.out_proj(out), new_kv_cache
```

## KV-Cache Memory Analysis 📊

Per layer, per token:
- K cache: $\text{num\_heads} \times \text{head\_dim} \times \text{sizeof(dtype)}$
- V cache: $\text{num\_heads} \times \text{head\_dim} \times \text{sizeof(dtype)}$
- Total per layer per token: $2 \times d_{\text{model}} \times \text{sizeof(dtype)}$

For LLaMA-2 70B (80 layers, $d_{\text{model}}=8192$, fp16):
- Per token: $80 \times 2 \times 8192 \times 2 \text{ bytes} = 2.6 \text{ MB}$
- For 4096 tokens: **10.5 GB** just for KV cache! 😱

This is why KV-cache memory is a critical bottleneck for long-context inference.

### ⭐ KV-Cache Memory Formula (Cheat Sheet)

$$\text{KV-cache memory} = 2 \times L \times d_{\text{model}} \times T \times \text{sizeof(dtype)}$$

where:
- $L$ = number of layers
- $d_{\text{model}}$ = model hidden dimension
- $T$ = sequence length (number of cached tokens)
- Factor of 2 = one for K, one for V

| Model | Layers | $d_{\text{model}}$ | KV per token (fp16) | KV for 4K context |
|-------|--------|------------|-------|----------|
| GPT-2 (117M) | 12 | 768 | 36 KB | 144 MB |
| LLaMA-7B | 32 | 4096 | 512 KB | 2 GB |
| LLaMA-70B | 80 | 8192 | 2.6 MB | **10.5 GB** |
| GPT-4 (rumored) | 120 | 12288 | 5.9 MB | **24 GB** |

> 🏭 **Production Impact**: For companies serving millions of users (like OpenAI, Anthropic), KV-cache memory is the #1 bottleneck determining how many users they can serve simultaneously. This is why optimizations like MQA, GQA, and quantized KV-cache (e.g., FP8) are critical in production deployments.

---

# 4️⃣ Generation with KV-Cache 🏭🔄

> 🍕 **Beginner Analogy**: Think of generation like building a LEGO tower. Without KV-cache, every time you add a block, you tear down the whole tower and rebuild it from scratch. With KV-cache, you just snap the new block on top. Same result, way less work!

```python
@torch.no_grad()
def generate_with_cache(model, input_ids, max_new_tokens, temperature=1.0):
    """Efficient generation using KV-cache."""
    B = input_ids.shape[0]
    
    # Phase 1: "Prefill" — process the entire prompt
    # This is the initial forward pass with the full prompt
    kv_caches = [None] * model.num_layers
    logits, kv_caches = model.forward_with_cache(input_ids, kv_caches)
    
    # Take last token's logits
    next_logits = logits[:, -1, :] / temperature
    next_token = torch.multinomial(F.softmax(next_logits, dim=-1), num_samples=1)
    generated = [next_token]
    
    # Phase 2: "Decode" — generate one token at a time with cache
    for _ in range(max_new_tokens - 1):
        # Only pass the NEW token (T=1), reuse cached KV
        logits, kv_caches = model.forward_with_cache(next_token, kv_caches)
        
        next_logits = logits[:, -1, :] / temperature
        next_token = torch.multinomial(F.softmax(next_logits, dim=-1), num_samples=1)
        generated.append(next_token)
    
    return torch.cat([input_ids] + generated, dim=-1)
```

### Two Phases of Generation

1. **Prefill**: Process entire prompt in parallel (like training). Compute-bound.
2. **Decode**: Generate tokens one at a time. Memory-bound (KV-cache reads).

Prefill is fast (parallel). Decode is slow (sequential, memory-bandwidth limited).

This is why "time to first token" is fast but "tokens per second" is slow.

### ⭐ Prefill vs Decode — Deep Dive

| Phase | What Happens | Bottleneck | Speed | Analogy |
|-------|-------------|-----------|-------|---------|
| 🟢 **Prefill** | Process entire prompt at once | GPU compute (FLOPs) | Fast ⚡ | Reading the whole question paper |
| 🔴 **Decode** | Generate 1 token, read entire KV-cache | Memory bandwidth | Slow 🐌 | Writing answers one at a time |

> 🏭 **Production Insight**: In production systems like vLLM and TensorRT-LLM, prefill and decode are often scheduled separately. Some systems batch many prefill requests together, and separately batch decode requests, because they have completely different computational profiles. This is called **iteration-level scheduling** and it's critical for maximizing GPU utilization.

### 🔬 Flash Attention (Dao et al., 2022) — The Speed Revolution

Standard attention computes the full $T \times T$ attention matrix, which requires $O(T^2)$ memory — disastrous for long sequences!

**Flash Attention** rewrites the attention computation to be **IO-aware**:
- Instead of materializing the full attention matrix in GPU HBM (slow memory), it computes attention in **tiles/blocks** that fit in SRAM (fast on-chip memory)
- Reduces memory from $O(T^2)$ to $O(T)$ 🎯
- **2-4× faster** than standard attention on A100 GPUs
- Enables training on much longer sequences (4K → 100K+)

> 📄 **Citation**: Dao, Fu, Ermon, Rudra, Ré. *"FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness"* (NeurIPS 2022). Updated in Flash Attention-2 (2023) with further 2× speedup.

---

# 5️⃣ Attention Patterns in Practice 🔍🧩

> 🍕 **Beginner Analogy**: When you read a sentence, different parts of your brain focus on different things — some track grammar, some track meaning, some track who said what. Similarly, different "attention heads" in a transformer learn to focus on different patterns. Let's see what they discover on their own!

## What Does the Model Actually Attend To?

Research has found several characteristic attention patterns:

### 🧲 Induction Heads
Learn to copy: if "AB" appeared before, when seeing "A" again, attend to "B" after the previous "A".
This is how in-context learning works!

> 💡 **Example**: If the text says "The capital of France is Paris. ... The capital of France is ___", an induction head will attend to "Paris" (the token that followed "France is" last time) and copy it. This is the mechanism behind few-shot prompting! (Olsson et al., 2022 — "In-context Learning and Induction Heads" 📄)

### 📍 Local Attention
Some heads attend primarily to nearby tokens (positional proximity).

> 💡 Like reading glasses that only focus on the nearby words — useful for grammar, punctuation, and local syntax patterns.

### 🕳️ Sink Tokens
Many heads attend heavily to the FIRST token, regardless of content.
The first token acts as a "garbage collection" for attention mass.

> 💡 **Why?** Softmax requires attention weights to sum to 1. When a head has "nothing relevant to attend to," it dumps the excess probability onto the first token (like a default/null target). This was discovered by Xiao et al. (2023) in "Efficient Streaming Language Models with Attention Sinks" 📄, and it's why keeping the first token in the KV-cache matters for streaming inference!

### 🌐 Global Attention
Some heads attend to semantically related tokens regardless of distance.

> 💡 These are the "meaning" heads — they might connect "he" to "John" even if they're 200 tokens apart. This is where the real "understanding" happens.

---

# 6️⃣ Multi-Query and Grouped-Query Attention 🤝📉

> 🍕 **Beginner Analogy — Study Group Notes**: Imagine a study group where everyone writes their own questions (Queries), but instead of everyone keeping their own separate answer key (Keys) and study notes (Values), the group shares ONE copy. That saves a TON of paper! This is Multi-Query Attention. Grouped-Query is a middle ground — small groups share notes.

## Standard Multi-Head Attention (MHA)
- Each head has its own $Q$, $K$, $V$ projections
- Parameters: $3 \times d_{\text{model}}^2$ (for $Q$, $K$, $V$)
- KV-cache per head: full size

## Multi-Query Attention (MQA) — Shazeer, 2019 📄
- Each head has its own $Q$, but ALL heads share ONE $K$ and ONE $V$
- Parameters: $d_{\text{model}}^2 + 2 \times d_{\text{head}} \times d_{\text{model}}$ (much less)
- KV-cache is $\text{num\_heads}\times$ smaller!

> 📄 **Citation**: Shazeer. *"Fast Transformer Decoding: One Write-Head is All You Need"* (2019). Originally proposed for faster inference in machine translation models. Later adopted by PaLM, Falcon, and others.

## Grouped-Query Attention (GQA) — Used in LLaMA-2/3 📄
- Heads are divided into groups
- Each GROUP shares $K$ and $V$
- Compromise between MHA quality and MQA efficiency

> 📄 **Citation**: Ainslie, Lee-Thorp, de Jong, Zemlyanskiy, Lebrón, Sanghai. *"GQA: Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints"* (EMNLP 2023).

### ⭐ MHA vs MQA vs GQA Comparison Table

| Variant | $Q$ heads | $K$/$V$ heads | KV-Cache Size | Quality | Used In |
|---------|-----------|---------------|-------------|---------|---------|
| **MHA** (standard) | $H$ | $H$ | $1\times$ (baseline) | 🟢 Best | GPT-2, BERT |
| **MQA** | $H$ | 1 | $\frac{1}{H}\times$ 🎯 | 🟡 Slightly worse | PaLM, Falcon |
| **GQA** | $H$ | $G$ (groups) | $\frac{G}{H}\times$ | 🟢 Near MHA | LLaMA-2, LLaMA-3, Mistral |

> For LLaMA-2 70B with 64 $Q$-heads and 8 $K$/$V$-heads (GQA with $G=8$): KV-cache is **8×** smaller than standard MHA! That's 10.5 GB → **~1.3 GB**. This is the difference between fitting on one GPU vs needing multiple GPUs just for the cache. 🏭

```python
class GroupedQueryAttention(torch.nn.Module):
    def __init__(self, d_model, num_heads, num_kv_heads):
        super().__init__()
        self.num_heads = num_heads
        self.num_kv_heads = num_kv_heads
        self.head_dim = d_model // num_heads
        self.heads_per_group = num_heads // num_kv_heads
        
        self.W_q = torch.nn.Linear(d_model, num_heads * self.head_dim, bias=False)
        self.W_k = torch.nn.Linear(d_model, num_kv_heads * self.head_dim, bias=False)
        self.W_v = torch.nn.Linear(d_model, num_kv_heads * self.head_dim, bias=False)
        self.W_o = torch.nn.Linear(d_model, d_model, bias=False)
    
    def forward(self, x):
        B, T, _ = x.shape
        
        q = self.W_q(x).view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        k = self.W_k(x).view(B, T, self.num_kv_heads, self.head_dim).transpose(1, 2)
        v = self.W_v(x).view(B, T, self.num_kv_heads, self.head_dim).transpose(1, 2)
        
        # Repeat K, V for each group
        # (B, num_kv_heads, T, d) → (B, num_heads, T, d)
        k = k.repeat_interleave(self.heads_per_group, dim=1)
        v = v.repeat_interleave(self.heads_per_group, dim=1)
        
        # Standard attention
        scores = torch.matmul(q, k.transpose(-2, -1)) / (self.head_dim ** 0.5)
        # Apply causal mask...
        attn = torch.softmax(scores, dim=-1)
        out = torch.matmul(attn, v)
        
        out = out.transpose(1, 2).contiguous().view(B, T, -1)
        return self.W_o(out)
```

### 🔬 Multi-Head Splitting — How It Works Under the Hood

The key insight of multi-head attention is that instead of one big attention operation, we split into $H$ parallel smaller ones:

$$d_{\text{head}} = \frac{d_{\text{model}}}{H}$$

Each head $h$ computes:

$$\text{head}_h = \text{softmax}\!\left(\frac{Q_h K_h^\top}{\sqrt{d_{\text{head}}}}\right) V_h$$

Then all heads are concatenated and projected:

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \ldots, \text{head}_H) \cdot W^O$$

> ⭐ **Why multiple heads?** Each head can learn to attend to different types of relationships — one for syntax, one for semantics, one for coreference, etc. It's like having multiple "experts" analyzing the text simultaneously.

---

# 7️⃣ Pre-LN vs Post-LN: The Normalization Debate ⚖️🔧

> 🍕 **Beginner Analogy**: Imagine you're stacking pancakes. After each pancake, you can either flatten the stack (normalize) **after** adding toppings (Post-LN), or flatten **before** adding toppings (Pre-LN). Pre-LN keeps the stack more stable because you normalize the base before building on it, preventing the whole thing from toppling as it gets tall.

## Post-LN (Original Transformer — Vaswani et al., 2017)
```
x → Attention → Add(x) → LayerNorm → FFN → Add → LayerNorm
```

## Pre-LN (GPT-2 and modern models)
```
x → LayerNorm → Attention → Add(x) → LayerNorm → FFN → Add(x)
```

### Why Pre-LN Won

Post-LN:
- Gradients must pass through LayerNorm at every residual
- Gradient scale becomes dependent on layer depth
- Requires learning rate warmup, careful initialization
- Can diverge during training

Pre-LN:
- Gradient path through residuals is clean (just addition)
- More stable training dynamics
- Less sensitive to learning rate
- But slightly worse final performance (solved by other techniques)

> ⭐ **The math reason**: In Pre-LN, the residual stream has a direct additive path: $x_{l+1} = x_l + f({\rm LN}(x_l))$. Gradients flow through the "$+ x_l$" path without any transformation — they're just 1! In Post-LN, gradients must pass through LayerNorm, which introduces multiplicative factors that can vanish or explode in deep networks. This is why Pre-LN transformers can train stably with 100+ layers while Post-LN often fails beyond ~12.

### 🏭 What Modern Models Use

| Model | Normalization | Notes |
|-------|-------------|-------|
| Original Transformer | Post-LN | Required warmup, careful tuning |
| GPT-2 | Pre-LN | Enabled stable training |
| LLaMA 1/2/3 | **RMSNorm** (Pre-LN) | Even simpler — no mean subtraction |
| PaLM | Pre-LN | Standard for large models |
| Gemma | RMSNorm (Pre-LN) | Google's approach |

---

# 8️⃣ Practical: Building Attention Visualization 📊🎨

> 🍕 **Beginner Analogy**: This is like getting X-ray vision into the model's brain — you can literally SEE what each "attention head" is looking at when it processes your text. It's like a heatmap showing "when generating this word, which previous words was the model thinking about?"

```python
import matplotlib.pyplot as plt

def visualize_attention(model, tokenizer, text):
    """Visualize attention patterns for input text."""
    tokens = tokenizer.encode(text)
    input_ids = torch.tensor([tokens])
    
    # Hook to capture attention weights
    attention_maps = []
    
    def hook_fn(module, input, output):
        # Capture attention weights before dropout
        attention_maps.append(output[1])  # Assuming model returns attn weights
    
    # Register hooks on attention layers
    hooks = []
    for block in model.blocks:
        h = block.attn.register_forward_hook(hook_fn)
        hooks.append(h)
    
    # Forward pass
    with torch.no_grad():
        model(input_ids)
    
    # Remove hooks
    for h in hooks:
        h.remove()
    
    # Plot attention for first layer, first head
    token_labels = tokenizer.convert_ids_to_tokens(tokens)
    attn = attention_maps[0][0, 0].numpy()  # Layer 0, Head 0
    
    fig, ax = plt.subplots(figsize=(8, 8))
    ax.imshow(attn, cmap='viridis')
    ax.set_xticks(range(len(token_labels)))
    ax.set_yticks(range(len(token_labels)))
    ax.set_xticklabels(token_labels, rotation=45, ha='right')
    ax.set_yticklabels(token_labels)
    ax.set_xlabel('Key (attending TO)')
    ax.set_ylabel('Query (attending FROM)')
    ax.set_title('Attention Pattern: Layer 0, Head 0')
    plt.tight_layout()
    plt.show()
```

> ⭐ **Try it yourself**: Load GPT-2 from HuggingFace (`model = GPT2Model.from_pretrained('gpt2', output_attentions=True)`) and visualize what it attends to. You'll see the causal mask pattern (upper triangle is always zero!) and discover induction heads, local attention, and sink tokens in action.

---

# 📝 Summary

| Concept | Key Insight |
|---------|-------------|
| Causal mask | Enables parallel training of autoregressive model |
| Teacher forcing | Use ground truth as input during training |
| KV-Cache | Store computed K, V; only compute new token's KV during generation |
| Prefill vs Decode | Prefill = parallel (compute-bound), Decode = sequential (memory-bound) |
| GQA | Share K, V across head groups; saves KV-cache memory |
| Pre-LN | Normalize before sublayer; more stable training |
| Attention patterns | Induction heads, local, global, sink tokens |

---

# 🧾 Cheat Sheet — Causal Self-Attention in 60 Seconds

```
🎯 CORE IDEA: Each token can ONLY attend to itself and previous tokens (never future)

📐 KEY FORMULA:
   Attention(Q,K,V) = softmax((QKᵀ + M) / √d_k) · V
   where M[i,j] = 0 if j≤i, -∞ if j>i

💾 KV-CACHE: Don't recompute K,V for old tokens — cache them!
   Memory = 2 × layers × d_model × seq_len × dtype_size

⚡ INFERENCE PHASES:
   Prefill → process prompt (fast, parallel, compute-bound)
   Decode  → generate tokens (slow, sequential, memory-bound)

🤝 ATTENTION VARIANTS:
   MHA: every head has own K,V        (full quality, full cost)
   MQA: all heads share 1 K,V         (fast inference, slight quality loss)
   GQA: groups share K,V              (best of both — used in LLaMA-2/3)

🔥 FLASH ATTENTION: tile-based, IO-aware → O(T) memory instead of O(T²)
```

---

# 📚 Research Citations & Deep Dive Reading

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|-----------------|
| ⭐ Attention Is All You Need | Vaswani, Shazeer, Parmar, et al. | 2017 | Introduced the Transformer architecture, self-attention, and the causal mask for decoder models |
| ⭐ Fast Transformer Decoding: One Write-Head is All You Need | Shazeer | 2019 | Multi-Query Attention (MQA) — share K,V across all heads for faster inference |
| ⭐ FlashAttention: Fast and Memory-Efficient Exact Attention | Dao, Fu, Ermon, Rudra, Ré | 2022 | IO-aware attention algorithm, $O(T)$ memory, 2-4× speedup |
| FlashAttention-2 | Dao | 2023 | Further 2× speedup via better parallelism and work partitioning |
| ⭐ GQA: Training Generalized Multi-Query Transformer Models | Ainslie, Lee-Thorp, de Jong, et al. | 2023 | Grouped-Query Attention — middle ground between MHA and MQA |
| In-context Learning and Induction Heads | Olsson, Elhage, Nanda, et al. (Anthropic) | 2022 | Discovered induction heads as the mechanism for in-context learning |
| Efficient Streaming Language Models with Attention Sinks | Xiao, Tian, Chen, et al. | 2023 | Discovered "attention sink" tokens; enables streaming infinite-length inference |
| Language Models are Few-Shot Learners (GPT-3) | Brown, Mann, Ryder, et al. | 2020 | Demonstrated scaling of autoregressive models with causal attention |

---

# 🧠 Beginner's Mental Model — The Complete Picture

```
You type: "Tell me about cats"
                    │
                    ▼
    ┌─────────────────────────────┐
    │  📥 PREFILL PHASE           │  Process entire prompt at once
    │  Tokens: [Tell, me, about,  │  (uses causal mask so each token
    │          cats]               │   only sees previous ones)
    │  ⏱️ Fast! (parallel)         │  KV-cache populated for all tokens
    └─────────┬───────────────────┘
              │
              ▼
    ┌─────────────────────────────┐
    │  🔄 DECODE PHASE            │  Generate ONE token at a time
    │                             │
    │  Step 1: → "Cats"           │  Q for new token, K/V from cache
    │  Step 2: → "are"            │  Cache grows by 1 entry
    │  Step 3: → "fascinating"    │  Repeat until done
    │  Step 4: → "creatures"      │
    │  ...                        │
    │  ⏱️ Slower (sequential)      │
    └─────────────────────────────┘
```

> ⭐ **The One Sentence Summary**: Causal self-attention is the constraint that makes language models *generate* text (can't see the future), while KV-caching is the optimization that makes it *fast enough* to be practical.

---

**Tomorrow**: The Feed-Forward Network (MLP) deep dive — SwiGLU activation, parameter count analysis, and the role of FFN as "memory" in transformers.
