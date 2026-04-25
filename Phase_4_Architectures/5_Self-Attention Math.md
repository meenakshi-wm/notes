📘 DAY 5 — Self-Attention Math — The Core of Modern AI 🧠✨

> *"Attention is all you need."* — Vaswani et al. (2017), the paper that changed everything

---

## 🎯 Goal

Understand self-attention not as "a mechanism in transformers," but as the **fundamental operation that revolutionized AI** by allowing EVERY token to directly attend to EVERY other token — eliminating the sequential bottleneck of RNNs and enabling massive parallelism. Master the precise Query-Key-Value math, understand WHY we scale by $\sqrt{d_k}$, HOW softmax creates attention weights, and implement a complete attention layer from scratch.

## ✅ If This Is Deeply Understood

You understand the mathematical core of GPT, BERT, LLaMA, and every modern LLM. You can derive attention gradients, explain why attention is $O(n^2)$ in sequence length, and reason about what the model "pays attention to" — the single most important concept in modern AI.

---

> ### 🎉 The Party Analogy (Read This First!)
>
> Imagine you walk into a **huge party** 🎈 with 100 people. You need to figure out who to talk to.
>
> - **Without attention (old RNN way):** You MUST talk to people **one by one**, in order, from the door. By the time you reach person #50, you've forgotten what person #1 said. 😫
> - **With self-attention (new way):** You can **look at EVERYONE at once**, instantly decide who's most relevant to you, and focus your conversation accordingly. 🎯
>
> That's self-attention in a nutshell — every word in a sentence gets to "look at" every other word simultaneously and decide which ones matter most.

---

# 1️⃣ From RNNs to Attention — Why the Revolution? 🔄➡️⚡

## The RNN Bottleneck 🐌

> **Beginner Analogy:** Imagine a game of **telephone** 📞 — Person 1 whispers a message to Person 2, who whispers to Person 3, and so on. By the time it reaches Person 100, the original message is completely garbled!

RNNs process sequences LEFT to RIGHT, one token at a time.
Information must flow through a chain: $h_1 \rightarrow h_2 \rightarrow \ldots \rightarrow h_t$.

Problems:
1. **Sequential computation** 🐢: Can't parallelize across time steps. GPU utilization is terrible.
2. **Information bottleneck** 🧊: Token 1's info must survive $T-1$ transformations to reach token $T$.
3. **Vanishing gradients** 📉: Even LSTMs struggle beyond ~500 tokens.

| Problem | RNN Reality | Self-Attention Solution |
|---------|-----------|----------------------|
| Speed | One token at a time 🐌 | All tokens in parallel ⚡ |
| Memory | Information decays over distance | Direct access to any position |
| Range | ~500 tokens max (practical) | 128K+ tokens (GPT-4) |
| GPU Use | Low (sequential) | High (massively parallel) |

## Attention: Direct Connections ⚡

> **Beginner Analogy:** Instead of the telephone game, imagine everyone is in a **group chat** 💬 — Person 100 can directly read what Person 1 typed. No information loss!

What if EVERY token could directly "look at" every other token?

Token 50 can directly access Token 1. No chain of hidden states needed.
Path length for any pair of tokens: $O(1)$ instead of $O(n)$.

⭐ **This is the key insight of attention:** replace sequential processing with DIRECT, parallel connections.

## The Origin: Bahdanau Attention (2014) 📜

Originally introduced for machine translation:
Instead of compressing the entire source sentence into one vector,
let the decoder ATTEND to different source positions for each output word.

"attention" = a learned, dynamic weighted average of all source positions.

⭐ **Vaswani et al. (2017)** generalized this into **SELF-attention**:
a sequence attends to ITSELF, and this is ALL you need.

---

# 2️⃣ The Query-Key-Value Framework 🔑🔍📦

## Intuition: A Retrieval System

> ### 💡 The Dating App Analogy
>
> Think of self-attention like a **dating app** 💘:
>
> - **Query (Q)** 🔍 = Your **profile preferences** — "What am I looking for?" (tall, funny, likes dogs)
> - **Key (K)** 🔑 = Each person's **profile headline** — "What do I have to offer?" (6'2", comedian, has golden retriever)
> - **Value (V)** 📦 = Each person's **actual personality/info** — "What you ACTUALLY get when you match"
>
> The app **compares your preferences (Q) against everyone's headlines (K)**, scores the matches, then shows you the **actual profiles (V)** of the best matches, weighted by compatibility!

Think of attention as a **soft dictionary lookup**:
- **Query (Q)** 🔍: "What am I looking for?" — the current token's request
- **Key (K)** 🔑: "What do I contain?" — each token's advertised content
- **Value (V)** 📦: "What information do I provide?" — each token's actual content

Process:
1. Compare the query against every key (dot product) 📊
2. Convert similarity scores to weights (softmax) 📈
3. Take a weighted sum of values ➕

⭐ **The result:** each token gets a context-aware representation that incorporates information from all relevant tokens.

## From Inputs to Q, K, V 🧮

Given input $X \in \mathbb{R}^{n \times d}$ ($n$ tokens, $d$ dimensions):

$$Q = X \cdot W_Q \quad \text{where } W_Q \in \mathbb{R}^{d \times d_k}$$

$$K = X \cdot W_K \quad \text{where } W_K \in \mathbb{R}^{d \times d_k}$$

$$V = X \cdot W_V \quad \text{where } W_V \in \mathbb{R}^{d \times d_v}$$

> **Beginner Analogy:** Think of $W_Q$, $W_K$, $W_V$ as three different pairs of **tinted glasses** 🕶️. When you look at the same word through each pair, you see different aspects of it — one shows what it's *searching for*, another shows what it *advertises*, and the third shows what it *actually contains*.

These are LEARNED linear projections. The network learns:
- WHAT to ask for ($W_Q$) 🔍
- HOW to describe content ($W_K$) 🔑
- WHAT information to provide ($W_V$) 📦

## Self-Attention: The Formula ⭐⭐⭐

$$\boxed{\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^T}{\sqrt{d_k}}\right)V}$$

> 🏆 **This is THE most important equation in modern AI.** If you remember ONE formula from this entire study plan, make it this one.

Step by step:
1. **$QK^T$** 📊: dot product of every query with every key → similarity matrix ($n \times n$)
2. **$\div \sqrt{d_k}$** ⚖️: scale to control variance (prevents extreme values)
3. **softmax** 📈: normalize each row to get attention weights $\in [0, 1]$
4. **$\times V$** 📦: weighted sum of values using attention weights

| Step | Operation | Shape | What It Does |
|------|-----------|-------|-------------|
| 1 | $QK^T$ | $(n \times d_k) \cdot (d_k \times n) = n \times n$ | Compute compatibility scores |
| 2 | $\div \sqrt{d_k}$ | $n \times n$ | Normalize to prevent explosion |
| 3 | $\text{softmax}(\cdot)$ | $n \times n$ | Convert to probabilities (0-1) |
| 4 | $\times V$ | $(n \times n) \cdot (n \times d_v) = n \times d_v$ | Weighted mix of values |

⭐ **Output:** each token is now a weighted combination of ALL values.

---

# 3️⃣ Why Scale by $\sqrt{d_k}$? — Critical Mathematical Detail 📐🔥

## The Variance Problem 📊

> **Beginner Analogy:** Imagine you're grading **essays** 📝. If one grader scores on a 1-10 scale and another on a 1-1000 scale, the second grader's scores will dominate. Scaling by $\sqrt{d_k}$ is like converting everyone to the SAME grading scale so comparisons are fair!

If $q, k \in \mathbb{R}^{d_k}$ with elements drawn from $\mathcal{N}(0, 1)$:

$$E[q^T k] = \sum_{i} E[q_i \cdot k_i] = \sum_{i} 0 = 0 \quad \checkmark$$

$$\text{Var}[q^T k] = \sum_{i} \text{Var}[q_i \cdot k_i] = \sum_{i} 1 = d_k$$

⭐ **The variance of the dot product GROWS with $d_k$!**

For $d_k = 64$: $\text{std}(q^T k) = \sqrt{64} = 8$.
Some dot products will be very large (e.g., 20+) and some very small (e.g., -20).

## The Softmax Saturation Problem 💀

$$\text{softmax}([20, -20, 0]) \approx [1.0, 0.0, 0.0]$$

> **Beginner Analogy:** Imagine a **volume knob** 🔊 turned up to MAX. At maximum volume, you can't tell the difference between sounds anymore — everything is just LOUD. That's what happens to softmax with extreme values — it becomes "all or nothing" and loses its ability to make nuanced decisions.

When inputs have high magnitude, softmax outputs become ONE-HOT.
Gradients of softmax approach ZERO in this regime (saturation).

⭐ **Without scaling:** attention becomes hard and non-differentiable as $d_k$ grows.

## The Fix: Divide by $\sqrt{d_k}$ ✅

$$\text{Var}\!\left[\frac{q^T k}{\sqrt{d_k}}\right] = \frac{d_k}{d_k} = 1$$

The dot products now have **unit variance REGARDLESS of dimension**.
softmax receives well-behaved inputs → smooth gradients → effective training.

| Scenario | $d_k$ | Raw Std Dev | After $\div\sqrt{d_k}$ | Softmax Behavior |
|----------|--------|------------|----------------------|-----------------|
| Small | 8 | $\sqrt{8} \approx 2.8$ | $\approx 1.0$ | ✅ Smooth |
| Medium | 64 | $\sqrt{64} = 8$ | $\approx 1.0$ | ✅ Smooth |
| Large | 128 | $\sqrt{128} \approx 11.3$ | $\approx 1.0$ | ✅ Smooth |
| Without scaling | 128 | $\approx 11.3$ | N/A | 💀 Saturated/Dead |

## ⚠️ This Is Not Optional

Without scaling, large models ($d_k = 128$) literally CANNOT train.
The gradients die immediately. This is why the original paper says **"Scaled Dot-Product Attention."**

---

# 4️⃣ Softmax as Attention Weight Factory 🏭📈

## The Softmax Function

> **Beginner Analogy:** Imagine you rate 5 restaurants: scores of 10, 5, 3, 1, 1. Softmax converts these raw scores into **percentages that add up to 100%** — so you know *exactly* how much you prefer each one relative to the others. Restaurant 1 might get 85%, restaurant 2 gets 10%, and the rest share the remaining 5%. 🍕📊

For a vector $z$ of attention scores:

$$\text{softmax}(z_i) = \frac{e^{z_i}}{\sum_{j} e^{z_j}}$$

Properties:
1. ✅ Output $\in (0, 1)$ for each element
2. ✅ Sum to 1: $\sum_{i} \text{softmax}(z_i) = 1$
3. ✅ Monotone: larger input → larger output
4. ✅ Differentiable everywhere

## Interpretation as Attention Weights 🎯

Row $i$ of the attention matrix: attention weights for token $i$.
$\alpha_{ij}$ = how much token $i$ attends to token $j$.

$$\sum_{j} \alpha_{ij} = 1 \quad \text{(each token's attention sums to 1)}$$

The output for token $i$:

$$\text{output}_i = \sum_{j} \alpha_{ij} \cdot v_j$$

> **Beginner Analogy:** Think of attention weights as a **mixing board** 🎛️ in a recording studio. Each slider controls how much of each instrument (token) you hear in the final mix. The sliders must add up to 100% (you can't create sound from nothing!).

This is a WEIGHTED AVERAGE of all value vectors.
High $\alpha_{ij}$ → token $i$ incorporates a lot of information from token $j$.

## Temperature Scaling 🌡️

$$\text{softmax}(z / \tau) \quad \text{where } \tau \text{ is the temperature}$$

| Temperature | Effect | Attention Pattern | Real-World Analogy |
|------------|--------|-------------------|-------------------|
| $\tau \to 0$ | Hard attention (argmax — one-hot) | Focus on ONE token only | Spotlight 🔦 |
| $\tau = 1$ | Standard softmax | Balanced focus | Normal vision 👁️ |
| $\tau \to \infty$ | Uniform attention (all weights = $1/n$) | Equal attention to all | Floodlight 💡 |

⭐ The $\sqrt{d_k}$ scaling is effectively a temperature: $\tau = \sqrt{d_k}$.

## Attention Patterns in Practice 🔬

Different attention heads learn different patterns:
- 📍 **Local attention**: attend to nearby tokens (positional pattern)
- 🌐 **Global attention**: attend to special tokens like `[CLS]` or `[SEP]`
- 📝 **Syntactic attention**: subject attends to its verb
- 💭 **Semantic attention**: "it" attends to its referent

⭐ These patterns are LEARNED, not programmed. The model discovers useful attention strategies from data.

---

# 5️⃣ Causal (Masked) Attention — For Autoregressive Models 🎭🚫

## Why Masking? 🤔

> **Beginner Analogy:** Imagine taking a **test** 📝 where you can see ALL the answers before writing yours — that's cheating! Causal masking is like putting a **cover sheet** over future answers so you can only see what came BEFORE your current question.

In language models (GPT, LLaMA), we predict the NEXT token.
Token $t$ should only see tokens $1, 2, \ldots, t$ (not future tokens $t+1, \ldots, T$).

⭐ **Without masking:** the model can "cheat" by looking at the answer.

## The Mask 🎭

Create an upper-triangular mask matrix $M$:

$$M[i, j] = \begin{cases} 0 & \text{if } j \leq i \quad \text{(can attend ✅)} \\ -\infty & \text{if } j > i \quad \text{(cannot attend 🚫)} \end{cases}$$

Apply BEFORE softmax:

$$\text{Attention} = \text{softmax}\!\left(\frac{QK^T}{\sqrt{d_k}} + M\right) \cdot V$$

Since $\text{softmax}(-\infty) = 0$, masked positions get **zero attention weight**.

## Visualization 👁️

For sequence of length 4:
```
Attention mask (✅ = can see, 🚫 = blocked):
  t1  t2  t3  t4
t1 [0   -∞  -∞  -∞]
t2 [0    0  -∞  -∞]
t3 [0    0   0  -∞]
t4 [0    0   0   0]
```

| Token | Can See | Why |
|-------|---------|-----|
| Token 1 | Only itself | First word — nothing before it |
| Token 2 | Tokens 1-2 | Can look back at Token 1 |
| Token 3 | Tokens 1-3 | Growing context window |
| Token 4 | Tokens 1-4 | Full past context |

Token 1: can only see itself
Token 2: can see tokens 1-2
Token 3: can see tokens 1-3
Token 4: can see tokens 1-4

⭐ This is **CAUSAL masking** — each position only has causal (past) context.

> 🏭 **Production Note:** EVERY autoregressive LLM (GPT-4, Claude, Gemini, LLaMA) uses causal masking during training. BERT uses **bidirectional** attention (no mask) because it's not autoregressive — it sees the whole sentence at once.

---

# 6️⃣ Multi-Head Attention — Parallel Attention Subspaces 🧠🧠🧠

## Why Multiple Heads? 🤔

> ### 💡 The Committee Analogy
>
> Imagine a **hiring committee** 👥 reviewing a job candidate:
>
> - **Head 1** (the Grammar Expert 📝): Focuses on sentence structure — does the subject agree with the verb?
> - **Head 2** (the Meaning Expert 💭): Focuses on semantics — what does "it" refer to?
> - **Head 3** (the Position Expert 📍): Focuses on nearby words — what's the local context?
> - **Head 4** (the Topic Expert 📚): Focuses on the big picture — what's this document about?
>
> Each "head" looks at the SAME candidate from a DIFFERENT perspective, then they **combine their opinions** for a final decision. That's multi-head attention! 🎯

A single attention head can only learn ONE type of relationship.
But language has MANY simultaneous relationships:
- 📝 Syntactic: subject-verb agreement
- 💭 Semantic: coreference resolution  
- 📍 Positional: local context
- 📚 Topical: document-level similarity

## Architecture 🏗️

Instead of one attention with $d_{\text{model}}$ dimensions:
Split into $h$ heads, each with $d_k = d_{\text{model}} / h$ dimensions.

For each head $i$:

$$Q_i = X \cdot W_Q^i \quad (d_{\text{model}} \to d_k)$$
$$K_i = X \cdot W_K^i \quad (d_{\text{model}} \to d_k)$$
$$V_i = X \cdot W_V^i \quad (d_{\text{model}} \to d_v)$$

$$\text{head}_i = \text{Attention}(Q_i, K_i, V_i)$$

Concatenate and project:

$$\boxed{\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \ldots, \text{head}_h) \cdot W^O}$$

Where $W^O \in \mathbb{R}^{(h \cdot d_v) \times d_{\text{model}}}$

## Parameter Count 🔢

| Component | Parameters | Notes |
|-----------|-----------|-------|
| Per head (Q, K, V) | $3 \times d_{\text{model}} \times d_k$ | Where $d_k = d_{\text{model}}/h$ |
| All $h$ heads | $3 \times d_{\text{model}}^2$ | Same as single-head! |
| Output projection $W^O$ | $d_{\text{model}}^2$ | Mixes head outputs |
| **Total** | $4 \times d_{\text{model}}^2$ | **No extra cost!** |

⭐ **Multi-head attention has NO extra parameters compared to single-head.**
It just PARTITIONS the same computation into parallel subspaces.

## Why This Works 🎯

Each head operates in a DIFFERENT subspace:
- 📍 Head 1 in first $d_k$ dimensions might learn positional attention
- 📝 Head 2 in next $d_k$ dimensions might learn syntactic attention
- 💭 Head 3 might learn semantic attention

The output projection $W^O$ MIXES these different perspectives back together.

> 🏭 **Production Reality:** GPT-3 uses 96 heads, GPT-4 likely uses 128+, LLaMA-70B uses 64 heads. Research shows some heads can be pruned without quality loss — not all perspectives are equally useful for every task.

---

# 7️⃣ Computational Complexity — The $O(n^2)$ Problem 💻📈

## Self-Attention Complexity

> **Beginner Analogy:** Imagine a room with $n$ people, and EVERY person must shake hands with every other person 🤝. With 10 people, that's $10 \times 10 = 100$ handshakes. With 100 people, that's $100 \times 100 = 10{,}000$ handshakes. Double the people → 4x the handshakes! That's the $O(n^2)$ problem.

For sequence length $n$ and model dimension $d$:

$QK^T$: matrix multiplication $(n \times d) \times (d \times n) \to (n \times n)$

$$\text{Computation: } O(n^2 \cdot d)$$
$$\text{Memory: } O(n^2) \text{ for the attention matrix}$$

## The Scaling Problem 📊💀

| Sequence Length ($n$) | Attention Matrix Size | Status |
|----------------------|----------------------|--------|
| 512 | 262K entries | ✅ Easy |
| 2,048 | 4.2M entries | ✅ Fine |
| 8,192 | 67M entries | ⚠️ Getting heavy |
| 32,768 | 1.07B entries | ❌ Very expensive |
| 131,072 (128K) | 17.2B entries | 💀 Need optimizations! |

⭐ **GPT-4 uses ~128K context.** That's $128K \times 128K = 16.4$ BILLION attention entries.
This is why long-context models need FlashAttention, Ring Attention, and other optimizations.

## Flash Attention — The Practical Solution ⚡

> **Beginner Analogy:** Imagine you need to add up a HUGE spreadsheet 📊. The naive way is to load the ENTIRE spreadsheet into memory — but what if it's too big to fit? Flash Attention is like processing it **chunk by chunk**, keeping a running total, and never needing the whole thing in memory at once.

Standard attention: write full $n \times n$ matrix to GPU memory (slow HBM access).
Flash Attention (Dao et al., 2022): compute attention in BLOCKS, never materializing the full matrix.

⭐ **Key insight:** softmax can be computed incrementally using the **online softmax trick**.
Reduces memory from $O(n^2)$ to $O(n)$, speeds up by 2-4× via better GPU memory access patterns.

> 🏭 **Production:** Flash Attention is now standard in ALL modern LLM implementations — PyTorch 2.0+ includes it natively via `torch.nn.functional.scaled_dot_product_attention()`.

---

# 8️⃣ Attention Variants — Beyond Vanilla 🔀🧪

## Cross-Attention 🔄

> **Beginner Analogy:** In self-attention, you talk to people at YOUR party. Cross-attention is like **FaceTiming someone at a DIFFERENT party** 📱 — your questions (Q) come from your conversation, but the answers (K, V) come from somewhere else entirely!

Query from one sequence, Key/Value from another.
Used in: machine translation decoders, multimodal models (image→text).

$$Q = X_{\text{decoder}} \cdot W_Q$$
$$K = X_{\text{encoder}} \cdot W_K$$
$$V = X_{\text{encoder}} \cdot W_V$$

## Linear Attention 📉➡️📈

Replace $\text{softmax}(QK^T)V$ with $\phi(Q)(\phi(K)^T V)$ where $\phi$ is a feature map.

| | Standard Attention | Linear Attention |
|--|-------------------|-----------------|
| **Complexity** | $O(n^2 \cdot d)$ | $O(n \cdot d^2)$ |
| **Scaling** | Quadratic in $n$ | Linear in $n$ |
| **Quality** | Gold standard | May lose expressiveness |
| **Used in** | GPT-4, Claude | Research/niche |

Trade-off: approximation, may lose some expressiveness.

## Sliding Window Attention 🪟

> **Beginner Analogy:** Instead of looking at the ENTIRE party, you only look at the **5 people closest to you** 👀. Much faster, and for most conversations, nearby context is what matters most!

Each token only attends to the nearest $w$ tokens.
Complexity: $O(n \cdot w)$ instead of $O(n^2)$.
Used in Mistral, Longformer.

## Sparse Attention 🕸️

Attend to a subset of tokens based on learned or fixed patterns.
Used in BigBird, Longformer, GPT-3's early experiments.

> 🏭 **Production Landscape of Attention Variants:**
>
> | Model | Attention Type | Why |
> |-------|---------------|-----|
> | GPT-4 | Standard + Flash | Maximum quality |
> | Claude | Standard + optimizations | Quality-focused |
> | Mistral 7B | Sliding Window | Efficiency for size |
> | LLaMA 3 | Grouped Query Attention | KV-cache efficiency |
> | Vision Transformers (ViT) | Standard self-attention | Images as patches |
> | AlphaFold 2 | Cross + self-attention | Protein structure |
> | BERT | Bidirectional (no mask) | Full context understanding |

---

# 9️⃣ Implementation — Self-Attention from Scratch 💻🔧

> 🎯 **Why code it yourself?** Reading the formula is like reading a recipe. CODING it is like actually cooking the dish. You'll understand every ingredient and every step at a visceral level.

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
import numpy as np
import math

# ═══════════════════════════════════════════════════════════
# Scaled Dot-Product Attention — From Scratch
# ═══════════════════════════════════════════════════════════

def scaled_dot_product_attention(Q, K, V, mask=None):
    """
    Compute scaled dot-product attention.
    
    Args:
        Q: (batch, n_heads, seq_len, d_k)
        K: (batch, n_heads, seq_len, d_k)
        V: (batch, n_heads, seq_len, d_v)
        mask: (batch, 1, seq_len, seq_len) or None
    
    Returns:
        output: (batch, n_heads, seq_len, d_v)
        attention_weights: (batch, n_heads, seq_len, seq_len)
    """
    d_k = Q.size(-1)
    
    # Step 1: Compute attention scores
    # (batch, heads, seq, d_k) @ (batch, heads, d_k, seq) → (batch, heads, seq, seq)
    scores = torch.matmul(Q, K.transpose(-2, -1))
    
    # Step 2: Scale by sqrt(d_k)
    scores = scores / math.sqrt(d_k)
    
    # Step 3: Apply mask (if provided)
    if mask is not None:
        scores = scores.masked_fill(mask == 0, float('-inf'))
    
    # Step 4: Softmax to get attention weights
    attention_weights = F.softmax(scores, dim=-1)
    
    # Step 5: Apply attention weights to values
    output = torch.matmul(attention_weights, V)
    
    return output, attention_weights


# ═══════════════════════════════════════════════════════════
# Multi-Head Attention — From Scratch
# ═══════════════════════════════════════════════════════════

class MultiHeadAttention(nn.Module):
    """
    Multi-Head Attention mechanism.
    
    Implements the exact architecture from "Attention Is All You Need."
    """
    
    def __init__(self, d_model, n_heads):
        super().__init__()
        
        assert d_model % n_heads == 0, "d_model must be divisible by n_heads"
        
        self.d_model = d_model
        self.n_heads = n_heads
        self.d_k = d_model // n_heads
        
        # Linear projections for Q, K, V
        self.W_Q = nn.Linear(d_model, d_model, bias=False)
        self.W_K = nn.Linear(d_model, d_model, bias=False)
        self.W_V = nn.Linear(d_model, d_model, bias=False)
        
        # Output projection
        self.W_O = nn.Linear(d_model, d_model, bias=False)
    
    def forward(self, x, mask=None):
        """
        Args:
            x: (batch_size, seq_len, d_model)
            mask: (batch_size, 1, seq_len, seq_len) or None
        
        Returns:
            output: (batch_size, seq_len, d_model)
            attention_weights: (batch_size, n_heads, seq_len, seq_len)
        """
        batch_size, seq_len, _ = x.shape
        
        # Linear projections
        Q = self.W_Q(x)  # (batch, seq, d_model)
        K = self.W_K(x)
        V = self.W_V(x)
        
        # Reshape to (batch, n_heads, seq_len, d_k)
        Q = Q.view(batch_size, seq_len, self.n_heads, self.d_k).transpose(1, 2)
        K = K.view(batch_size, seq_len, self.n_heads, self.d_k).transpose(1, 2)
        V = V.view(batch_size, seq_len, self.n_heads, self.d_k).transpose(1, 2)
        
        # Apply attention
        attn_output, attn_weights = scaled_dot_product_attention(Q, K, V, mask)
        
        # Reshape back: (batch, n_heads, seq, d_k) → (batch, seq, d_model)
        attn_output = attn_output.transpose(1, 2).contiguous()
        attn_output = attn_output.view(batch_size, seq_len, self.d_model)
        
        # Output projection
        output = self.W_O(attn_output)
        
        return output, attn_weights


# ═══════════════════════════════════════════════════════════
# Causal Mask Generator
# ═══════════════════════════════════════════════════════════

def create_causal_mask(seq_len):
    """Create causal (autoregressive) attention mask."""
    mask = torch.tril(torch.ones(seq_len, seq_len))
    return mask.unsqueeze(0).unsqueeze(0)  # (1, 1, seq, seq)


# ═══════════════════════════════════════════════════════════
# Tests and Demonstrations
# ═══════════════════════════════════════════════════════════

print("=" * 70)
print("SELF-ATTENTION: Complete Implementation Tests")
print("=" * 70)

# Test 1: Basic attention
print("\n--- Test 1: Basic Scaled Dot-Product Attention ---")
batch_size = 2
seq_len = 4
d_model = 64
n_heads = 8
d_k = d_model // n_heads

Q = torch.randn(batch_size, n_heads, seq_len, d_k)
K = torch.randn(batch_size, n_heads, seq_len, d_k)
V = torch.randn(batch_size, n_heads, seq_len, d_k)

output, weights = scaled_dot_product_attention(Q, K, V)
print(f"Q shape: {Q.shape}")
print(f"Output shape: {output.shape}")
print(f"Attention weights shape: {weights.shape}")
print(f"Weights sum per row: {weights[0, 0].sum(dim=-1)}")  # Should be all 1.0

# Test 2: Causal attention
print("\n--- Test 2: Causal (Masked) Attention ---")
causal_mask = create_causal_mask(seq_len)
output_causal, weights_causal = scaled_dot_product_attention(Q, K, V, causal_mask)
print(f"Causal mask:\n{causal_mask[0, 0]}")
print(f"Attention weights (head 0, batch 0):\n{weights_causal[0, 0].detach().numpy().round(3)}")
print(f"Note: upper triangle should be ~0 (masked)")

# Test 3: Multi-Head Attention
print("\n--- Test 3: Multi-Head Attention ---")
mha = MultiHeadAttention(d_model=64, n_heads=8)
x = torch.randn(batch_size, seq_len, 64)
output_mha, attn_weights = mha(x, causal_mask)
print(f"Input shape: {x.shape}")
print(f"Output shape: {output_mha.shape}")
print(f"Attention weights shape: {attn_weights.shape}")
print(f"MHA parameters: {sum(p.numel() for p in mha.parameters()):,}")

# Test 4: Verify scaling effect
print("\n--- Test 4: Scaling Effect Demonstration ---")
d_k_test = 64
q = torch.randn(1, 1, 1, d_k_test)
k = torch.randn(1, 1, 10, d_k_test)

# Without scaling
raw_scores = torch.matmul(q, k.transpose(-2, -1))
# With scaling
scaled_scores = raw_scores / math.sqrt(d_k_test)

print(f"d_k = {d_k_test}")
print(f"Raw scores — mean: {raw_scores.mean():.3f}, std: {raw_scores.std():.3f}")
print(f"Scaled scores — mean: {scaled_scores.mean():.3f}, std: {scaled_scores.std():.3f}")
print(f"Raw softmax entropy: {-(F.softmax(raw_scores, -1) * F.log_softmax(raw_scores, -1)).sum():.4f}")
print(f"Scaled softmax entropy: {-(F.softmax(scaled_scores, -1) * F.log_softmax(scaled_scores, -1)).sum():.4f}")
print(f"Higher entropy = more uniform (less peaked) attention")

# Test 5: Attention as information routing
print("\n--- Test 5: Attention Learns to Copy ---")

# Create a sequence where position 2 should attend strongly to position 0
# (e.g., simulating coreference)
x_demo = torch.zeros(1, 5, 64)
x_demo[0, 0] = torch.randn(64)  # Target info
x_demo[0, 2] = x_demo[0, 0]     # Query similar to target
x_demo[0, 1] = torch.randn(64)  # Random
x_demo[0, 3] = torch.randn(64)  # Random
x_demo[0, 4] = torch.randn(64)  # Random

# With identity projection, similar tokens attend to each other
mha_demo = MultiHeadAttention(d_model=64, n_heads=1)
# Initialize to near-identity
with torch.no_grad():
    for name, param in mha_demo.named_parameters():
        if 'W_Q' in name or 'W_K' in name or 'W_V' in name or 'W_O' in name:
            nn.init.eye_(param)

out_demo, weights_demo = mha_demo(x_demo)
print(f"Attention from position 2 (similar to pos 0):")
print(f"  Weights: {weights_demo[0, 0, 2].detach().numpy().round(3)}")
print(f"  Position 0 gets highest weight (similar query-key)")


# ═══════════════════════════════════════════════════════════
# NumPy Reference Implementation (for deep understanding)
# ═══════════════════════════════════════════════════════════

print("\n" + "=" * 70)
print("NUMPY REFERENCE IMPLEMENTATION")
print("=" * 70)

def attention_numpy(Q, K, V, mask=None):
    """Pure NumPy attention for maximum transparency."""
    d_k = Q.shape[-1]
    
    # Dot product
    scores = Q @ K.T  # (seq, seq)
    
    # Scale
    scores = scores / np.sqrt(d_k)
    
    # Mask
    if mask is not None:
        scores = np.where(mask, scores, -1e9)
    
    # Softmax (numerically stable)
    scores_max = np.max(scores, axis=-1, keepdims=True)
    exp_scores = np.exp(scores - scores_max)
    weights = exp_scores / np.sum(exp_scores, axis=-1, keepdims=True)
    
    # Weighted sum
    output = weights @ V
    
    return output, weights

# Demo with small matrices
np.random.seed(42)
seq_len_np = 4
d_k_np = 8

Q_np = np.random.randn(seq_len_np, d_k_np)
K_np = np.random.randn(seq_len_np, d_k_np)
V_np = np.random.randn(seq_len_np, d_k_np)

# Causal mask
mask_np = np.tril(np.ones((seq_len_np, seq_len_np), dtype=bool))

out_np, weights_np = attention_numpy(Q_np, K_np, V_np, mask_np)

print(f"\nSequence length: {seq_len_np}, d_k: {d_k_np}")
print(f"\nAttention weights (causal):")
for i in range(seq_len_np):
    row = ' '.join(f'{w:.3f}' for w in weights_np[i])
    print(f"  Token {i}: [{row}]")
print(f"\nEach row sums to: {weights_np.sum(axis=-1).round(4)}")
```

---

# 🔟 Deep Reflection Questions (Research Mindset) 🧪🔬

> 💡 These questions go BEYOND the basics. They're designed to make you think like a researcher, not just a practitioner.

1. 🤔 Self-attention is $O(n^2)$ in sequence length. For a 100K token context:
   that's 10 BILLION attention scores per layer per head.
   Why is this still worth it compared to RNNs? What does attention buy
   that justifies the quadratic cost?

2. 📐 If you remove the scaling by $\sqrt{d_k}$, what happens to attention patterns as $d_k$ increases?
   Derive the variance of $QK^T$ entries and show exactly when softmax saturates.

3. 🧮 The attention matrix is a STOCHASTIC matrix (rows sum to 1).
   What does this imply about information capacity? Can attention "amplify" a signal,
   or can it only "route" existing information?

4. ⚖️ In multi-head attention, each head has $d_k = d_{\text{model}} / h$ dimensions.
   As $h$ increases, $d_k$ decreases. Is there a trade-off between
   number of perspectives vs. quality of each perspective?

5. ⚡ Causal masking makes attention $O(n^2/2)$ instead of $O(n^2)$.
   But for INFERENCE, we only need the LAST row of the attention matrix.
   How does KV caching exploit this? (Preview of Day 40)

6. 📉 Linear attention replaces $\text{softmax}(QK^T)V$ with $\phi(Q)(\phi(K)^T V)$.
   What mathematical property of softmax makes this replacement lossy?
   Why hasn't linear attention replaced standard attention?

7. 🔬 Attention weights are often interpreted as "what the model pays attention to."
   Is this interpretation always correct? Research: when do attention weights
   NOT correspond to feature importance?

---

# 🚀 Startup Founder Insight — Why Self-Attention = Business Advantage

Self-attention is literally THE core operation of modern AI. Every LLM — GPT-4, Claude, Gemini, LLaMA, Mistral — runs self-attention at its heart. Understanding this math at a deep level means:

- 🧠 You can reason about **MODEL CAPABILITIES**: "Can this model learn X?" depends on what attention can compute
- 💰 You can estimate **COMPUTE COSTS**: attention cost = $2 \times n^2 \times d \times L$ (per forward pass)
- 🏗️ You can make **ARCHITECTURE DECISIONS**: how many heads? what dimension? how much context?
- ⚠️ You can understand **LIMITATIONS**: the $n^2$ scaling is literally why we can't have infinite context

Companies building on LLMs that DON'T understand attention math:
- ❌ Can't predict when their product will fail (context too long, attention dilution)
- ❌ Can't optimize inference costs (KV cache sizing, batch strategies)
- ❌ Can't debug quality issues (attention patterns reveal model behavior)

⭐ **Self-attention understanding is the dividing line between "using AI tools" and "building AI products."**

---

# 📝 Homework (Required)

1. 🔧 Implement attention WITHOUT PyTorch — pure NumPy, including the backward pass (gradients of softmax, gradients through QKV projections).
2. 📊 Create a visualization: for a 10-token sequence, plot the attention weight matrix as a heatmap. Use real text and a trained model (use HuggingFace transformers library).
3. 📐 Empirically verify the $\sqrt{d_k}$ scaling: train two small attention models on the same data, one with scaling and one without. Compare convergence.
4. 🔄 Implement cross-attention (where Q comes from one sequence and K, V from another). Verify shapes and logic.
5. 🔢 Compute the exact FLOPs for one attention layer with $d_{\text{model}}=4096$, $n_{\text{heads}}=32$, $\text{seq\_len}=2048$. How does this compare to the FLOPs of one feedforward layer?
6. ⚡ Research FlashAttention: what is the "online softmax" trick and why does it enable memory-efficient attention? Write a pseudocode implementation.

---

# 🔬 Deep Research — Landmark Papers & Innovations

> These are the papers that built the foundation of modern AI attention mechanisms.

| Year | Paper | Authors | Key Contribution | Impact |
|------|-------|---------|-----------------|--------|
| 2014 | *Neural Machine Translation by Jointly Learning to Align and Translate* | Bahdanau, Cho, Bengio | 🏆 First attention mechanism for seq2seq | Started the attention revolution |
| 2015 | *Effective Approaches to Attention-based Neural Machine Translation* | Luong, Pham, Manning | Global vs. local attention variants | Simplified attention, practical improvements |
| 2017 | *Attention Is All You Need* | Vaswani et al. | ⭐ Self-attention, multi-head attention, Transformer | THE paper that changed everything |
| 2019 | *Fast Transformer Decoding: One Write-Head is All You Need* | Shazeer | Multi-Query Attention (MQA) | 10-100x faster inference |
| 2022 | *FlashAttention: Fast and Memory-Efficient Exact Attention* | Dao et al. | IO-aware attention algorithm | 2-4x speedup, $O(n)$ memory |
| 2023 | *GQA: Training Generalized Multi-Query Transformer Models* | Ainslie et al. | Grouped Query Attention | Best of MHA and MQA |

### ⭐ Key Research Insights

- **Multi-Query Attention (MQA):** All heads share the SAME K and V projections, only Q differs. Reduces KV-cache memory by $h\times$ during inference. Used in PaLM, Falcon.
- **Grouped Query Attention (GQA):** Compromise between MHA and MQA — groups of heads share K,V. Used in LLaMA 2/3.
- **Flash Attention 2 (2023):** Further optimizations achieving near-optimal GPU utilization. Now the default in PyTorch.
- **Ring Attention (2023):** Distributes attention computation across multiple GPUs for extremely long contexts (1M+ tokens).

---

# 🏭 Production Scenarios — Self-Attention in the Real World

| Application | How Self-Attention is Used | Scale |
|------------|---------------------------|-------|
| **ChatGPT / Claude** 💬 | Causal self-attention in every layer | 128K+ context, billions of params |
| **Google Search** 🔍 | BERT bidirectional attention for query understanding | Billions of queries/day |
| **GitHub Copilot** 👨‍💻 | Attention over code context | Millions of completions/day |
| **AlphaFold** 🧬 | Cross-attention between amino acid sequences | Solved protein folding |
| **DALL-E / Midjourney** 🎨 | Cross-attention between text and image patches | Billions of images generated |
| **Tesla Autopilot** 🚗 | Self-attention over camera inputs | Real-time, safety-critical |
| **Whisper (Speech)** 🎤 | Attention over audio features | Transcription at scale |

### ⚡ KV-Cache: The Inference Secret Weapon

During inference (generating one token at a time), you don't need to recompute K and V for all previous tokens — you **cache** them!

- Without KV-cache: generating token $t$ costs $O(t^2)$ → total for $n$ tokens: $O(n^3)$ 💀
- With KV-cache: generating token $t$ costs $O(t)$ → total for $n$ tokens: $O(n^2)$ ✅
- KV-cache memory per layer: $2 \times n \times d_{\text{model}} \times \text{precision\_bytes}$

⭐ For LLaMA-70B with 128K context at FP16: KV-cache alone = **~40 GB per request**. This is why inference is expensive!

---

# 📋 CHEAT SHEET — Self-Attention Math ⚡

> **Print this. Pin it to your wall. Reference it daily.** 📌

### Core Formula
$$\boxed{\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^T}{\sqrt{d_k}}\right)V}$$

### Projections
$$Q = XW_Q, \quad K = XW_K, \quad V = XW_V$$

### Multi-Head Attention
$$\text{head}_i = \text{Attention}(XW_Q^i,\; XW_K^i,\; XW_V^i)$$
$$\text{MultiHead}(Q,K,V) = \text{Concat}(\text{head}_1, \ldots, \text{head}_h)W^O$$

### Causal Mask
$$\text{Attention} = \text{softmax}\!\left(\frac{QK^T}{\sqrt{d_k}} + M\right)V, \quad M_{ij} = \begin{cases} 0 & j \leq i \\ -\infty & j > i \end{cases}$$

### Key Dimensions

| Symbol | Meaning | Typical Values |
|--------|---------|---------------|
| $n$ | Sequence length | 512 – 128K |
| $d_{\text{model}}$ | Model dimension | 768 – 8192 |
| $h$ | Number of heads | 12 – 128 |
| $d_k = d_{\text{model}}/h$ | Head dimension | 64 – 128 |

### Complexity

| Operation | Time | Memory |
|-----------|------|--------|
| Self-attention | $O(n^2 \cdot d)$ | $O(n^2)$ |
| Flash Attention | $O(n^2 \cdot d)$ (same) | $O(n)$ ← ⚡ |
| Linear Attention | $O(n \cdot d^2)$ | $O(n \cdot d)$ |
| Sliding Window | $O(n \cdot w \cdot d)$ | $O(n \cdot w)$ |

### The Scaling Rule (WHY $\sqrt{d_k}$)
$$\text{Var}[q^T k] = d_k \quad \xrightarrow{\div\sqrt{d_k}} \quad \text{Var}\!\left[\frac{q^T k}{\sqrt{d_k}}\right] = 1$$

### Quick Glossary 📖

| Term | One-Line Definition |
|------|-------------------|
| **Self-Attention** | Every token looks at every other token to decide relevance |
| **Query (Q)** 🔍 | "What am I looking for?" |
| **Key (K)** 🔑 | "What do I have to offer?" |
| **Value (V)** 📦 | "What information do I actually share?" |
| **Softmax** | Converts raw scores to probabilities (0-1, sum to 1) |
| **Causal Mask** | Prevents looking at future tokens (for text generation) |
| **Multi-Head** | Multiple parallel attention "perspectives" |
| **Flash Attention** | Memory-efficient attention (same math, smarter computation) |
| **KV-Cache** | Stores past K,V to avoid recomputation during generation |
| **MQA** | All heads share K,V — faster inference |
| **GQA** | Groups of heads share K,V — balance of quality/speed |
