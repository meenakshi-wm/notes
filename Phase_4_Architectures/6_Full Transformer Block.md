# 📘 DAY 6 — Full Transformer Block — Encoder Architecture, RoPE, GQA & KV Caching 🏗️🔥

> *"A transformer block is like one complete **thinking step** for an AI brain. Stack many blocks together? That's how you get deeper, richer understanding — like thinking harder about a problem, layer by layer."* 🧠

---

## 🎯 Goal

Understand the complete transformer block as the fundamental building unit of modern AI — not just attention, but the FULL architecture: multi-head attention with Rotary Positional Encoding (RoPE), residual connections, LayerNorm (pre-norm vs post-norm), feedforward networks with SwiGLU, Grouped Query Attention (GQA), and KV caching for efficient inference. Implement a Llama-3 style transformer from scratch, understanding every component at the mathematical and implementation level.

## ⭐ If This Is Deeply Understood

You can read the Llama-3, GPT, or Mistral architecture papers and understand EVERY design choice. You can implement, modify, and debug transformer models. You understand the engineering that makes modern LLMs possible — from RoPE's elegant rotation math to KV caching that makes inference 100x faster.

> 🎒 **Beginner Analogy — The Assembly Line**: Imagine building an **assembly line** for understanding language. Each station (transformer block) has workers who: (1) 🔍 look at all the words together (**attention**), (2) 🛤️ remember what came in originally (**residual connection**), (3) ⚖️ do a quality check (**layer norm**), and (4) 🧠 think deeply about what they found (**feed-forward network**). Stack 32 stations → GPT-level intelligence. Stack 80 → Llama-3 70B.

---

## 🌍 Where This Powers the Real World

| Product | Architecture | What It Does |
|---------|-------------|---------------|
| 🤖 **GPT-4** (OpenAI) | Decoder-only transformer | ChatGPT, coding, reasoning |
| 🧠 **Claude** (Anthropic) | Decoder-only transformer | Conversation, analysis, safety |
| 💎 **Gemini** (Google) | Decoder-only transformer | Multimodal AI |
| 🦙 **LLaMA 3** (Meta) | Decoder-only transformer | Open-source LLM |
| 📖 **BERT** (Google) | Encoder-only transformer | Search ranking, NLP classification |
| 🔄 **T5** (Google) | Encoder-decoder transformer | Translation, summarization |
| 👁️ **ViT** (Google) | Vision transformer | Image classification |
| 🎧 **Whisper** (OpenAI) | Encoder-decoder transformer | Speech recognition |

> 💡 **Encoder** = reading & understanding the input (like reading a book). **Decoder** = writing the answer (like writing a summary). **Cross-attention** = the decoder "peeking back" at what the encoder understood — like glancing at your notes while writing an essay!

---

# 1️⃣ The Transformer Block — Complete Architecture 🏗️

> 🎒 **Beginner Analogy**: A transformer block is like one **floor of a smart building**. Each floor has two rooms: 🔍 Room 1 (attention) lets all the words **talk to each other**. 🧠 Room 2 (FFN) lets each word **think privately**. The 🛤️ elevator (residual connection) makes sure information flows freely up and down. The building inspector (LayerNorm) checks everything between rooms.

## ⭐ Original Transformer (Vaswani et al., 2017) — Post-Norm

```
Input
  → Multi-Head Attention → Add (residual) → LayerNorm
  → Feed-Forward Network → Add (residual) → LayerNorm
Output
```

**Post-Norm equations:**

$$x = \text{LayerNorm}\bigl(x + \text{MultiHeadAttention}(x)\bigr)$$

$$x = \text{LayerNorm}\bigl(x + \text{FFN}(x)\bigr)$$

## ⭐ Modern Transformer (Llama, GPT-3+) — Pre-Norm

```
Input
  → LayerNorm → Multi-Head Attention → Add (residual)
  → LayerNorm → Feed-Forward Network → Add (residual)
Output
```

**Pre-Norm equations (used in ALL modern LLMs):**

$$x = x + \text{MultiHeadAttention}\bigl(\text{LayerNorm}(x)\bigr)$$

$$x = x + \text{FFN}\bigl(\text{LayerNorm}(x)\bigr)$$

> 🎒 **Beginner Analogy — Pre-Norm vs Post-Norm**:
> - **Post-Norm** = grading your test AFTER you finish (errors may have already snowballed) ❌
> - **Pre-Norm** = organizing your desk BEFORE starting homework (prevents mistakes from piling up) ✅

### ⭐ Why Pre-Norm is Now STANDARD

Pre-norm is now STANDARD because:
1. 📊 Gradients flow directly through residual connections (no norm in the path)
2. 🛡️ Training is more stable — norm before attention prevents activation explosion
3. 🏗️ Can train deeper models without warmup tricks

> 📚 **Research**: Xiong et al. (2020) — *"On Layer Normalization in the Transformer Architecture"* proved mathematically that pre-norm enables stable training without learning rate warmup. Now standard in GPT-3+, LLaMA, Claude, and all modern LLMs.

## ⭐ Why Each Component Exists

| Component | Purpose | Analogy 🎒 | What Happens Without It ❌ |
|-----------|---------|-----------|---------------------------|
| 🔍 Multi-Head Attention | Token-to-token interaction | **Group discussion** — words talk to each other | Tokens can't communicate |
| 🛤️ Residual Connection | Gradient highway, identity mapping | **Express elevator** — bypasses all floors directly | Can't train deep models |
| ⚖️ LayerNorm | Stabilize activations | **Quality control inspector** — keeps numbers in range | Training diverges |
| 🧠 Feed-Forward Network | Per-token nonlinear transformation | **Private think time** — each word reflects alone | No per-token processing capacity |
| 📍 Positional Encoding | Order information | **Seat numbers** — who sits where matters! | "dog bites man" = "man bites dog" |

### ⭐ The FFN Formula (Original Transformer)

$$\text{FFN}(x) = \text{GELU}(xW_1 + b_1)W_2 + b_2$$

Where $W_1 \in \mathbb{R}^{d \times d_{ff}}$, $W_2 \in \mathbb{R}^{d_{ff} \times d}$, typically $d_{ff} = 4d$.

### ⭐ Parameter Count Formula for a Full Transformer

$$\text{Total Parameters} \approx 12d^2 \cdot L$$

Where $d$ = model dimension, $L$ = number of layers.

> 💡 **Key Insight**: Doubling the model dimension $d$ **quadruples** the parameters per layer (because $d^2$). This is why scaling from 7B to 70B parameters requires much larger GPUs!

### 🏛️ Encoder vs Decoder vs Encoder-Decoder

| Architecture | What It Does | Analogy 🎒 | Examples |
|-------------|-------------|-----------|----------|
| **Encoder-only** | Reads & understands input | 📖 Reading a book and highlighting key ideas | BERT, RoBERTa |
| **Decoder-only** | Generates output token by token | ✍️ Writing a story one word at a time | GPT-4, Claude, LLaMA, Gemini |
| **Encoder-Decoder** | Reads input, then generates output | 📖→✍️ Reading a French book, then writing the English translation | T5, Whisper, BART |

> 🔑 **Cross-Attention**: In encoder-decoder models, the decoder uses **cross-attention** to "peek back" at the encoder's output — like glancing at your source notes while writing a translation!

---

# 2️⃣ Residual Connections — The Gradient Highway 🛣️

> 🎒 **Beginner Analogy**: Imagine you're passing a message down a long chain of people (like the game "telephone"). Normally, the message gets garbled after a few people. A **residual connection** is like giving everyone a **photocopy of the original message** — even if someone garbles it, the next person can compare with the original! This is why we can stack 100+ layers without losing information.

## ⭐ The Formula

$$\text{output} = x + \text{sublayer}(x)$$

> 🎒 **In plain English**: The output = **what came in** + **what this layer figured out**. You always keep the original!

For a transformer with $L$ layers:

$$x_L = x_0 + \sum_{i=1}^{L} \text{sublayer}_i(x_{i-1})$$

> 💡 This means the final output is the **original input PLUS all the transformations** from every layer. Nothing is ever lost!

## ⭐ Why This Works — Gradient Flow 📊

Gradient of output w.r.t. input:

$$\frac{\partial x_L}{\partial x_0} = I + \frac{\partial}{\partial x_0} \left[\sum_{i} \text{sublayer}_i(x_{i-1})\right]$$

The identity matrix $I$ ensures gradients ALWAYS flow. 🛤️
Even if all sublayer gradients vanish: $\frac{\partial x_L}{\partial x_0} \geq I$.

> 🎒 **Beginner Analogy**: Without residual connections, training a deep network is like whispering a message through 100 people — it disappears. **With** residual connections, it's like having a **dedicated express lane** on a highway — your message zooms straight through regardless of traffic! 🚀

| Scenario | Gradient Behavior | Analogy 🎒 |
|----------|------------------|-----------|
| ❌ Without residuals (12+ layers) | Gradient vanishes → can't train | Whisper through 100 people |
| ✅ With residuals (100+ layers) | Gradient has shortcut path | Express highway lane |
| 🏗️ Real LLMs (32–80 layers) | Stable training at massive scale | Elevator in a skyscraper |

Without residual connections, gradients must flow through EVERY layer sequentially.
12 layers of attention + FFN → gradient vanishes. Same problem as deep RNNs.

With residual connections, gradients have a SHORTCUT path directly from loss to any layer.
This is why we can train 100+ layer transformers.

## ⭐ The Ensemble Interpretation 🎰

A residual network of depth $L$ can be viewed as an ensemble of $2^L$ paths.
Each path has a different subset of layers. Most paths are short.
The model learns to combine features from paths of different depths.

> 📚 **Research**: He et al. (2016) — *"Deep Residual Learning"* introduced skip connections for 152-layer CNNs. Veit et al. (2016) showed residual networks behave like **ensembles** of shallow networks. Vaswani et al. (2017) adopted this for transformers, making deep stacking possible.

---

# 3️⃣ Layer Normalization — Stabilizing the Transformer ⚖️

> 🎒 **Beginner Analogy**: Imagine a classroom where some students shout (huge numbers) and others whisper (tiny numbers). LayerNorm is like a **volume equalizer** 🎚️ — it brings everyone to the same comfortable volume level so the teacher (next layer) can hear everyone clearly. Without it, the shouters drown out the whisperers and learning breaks down!

## ⭐ LayerNorm Formula

For a vector $x \in \mathbb{R}^d$:

$$\text{LayerNorm}(x) = \gamma \odot \frac{x - \mu}{\sqrt{\sigma^2 + \epsilon}} + \beta$$

Where:
- $\mu = \frac{1}{d} \sum_{i} x_i$ — mean across features
- $\sigma^2 = \frac{1}{d} \sum_{i} (x_i - \mu)^2$ — variance across features
- $\gamma, \beta \in \mathbb{R}^d$ — learned scale and shift parameters
- $\epsilon \approx 10^{-5}$ — numerical stability (prevents division by zero)

> 🎒 **Step by step**: (1) Calculate the average of all numbers. (2) Subtract the average (center around zero). (3) Divide by the spread (standard deviation). (4) Scale and shift with learned parameters $\gamma$ and $\beta$. Done! ✅

## ⭐ LayerNorm vs. BatchNorm — Why Transformers Use LayerNorm

| Feature | BatchNorm 📦 | LayerNorm 📝 |
|---------|-----------|----------|
| Normalize across | Batch dimension | Feature dimension |
| Statistics | Per-feature across batch | Per-sample across features |
| Depends on batch size | ❌ Yes (needs big batches) | ✅ No (works with any batch) |
| Works with batch=1 | ❌ No (needs batch statistics) | ✅ Yes |
| Sequence models | ❌ Problematic (variable length) | ✅ Works perfectly |
| Used in | CNNs (ResNet, etc.) | Transformers (GPT, BERT, LLaMA) |

> 💡 **Why LayerNorm wins for LLMs**: During inference, you often process **one sequence at a time** (batch=1). BatchNorm fails here. LayerNorm doesn't care about batch size — it only looks at features within a single sample.

## ⭐ RMSNorm — The Modern Standard (Llama, Mistral) 🚀

Modern LLMs use RMSNorm instead of LayerNorm:

$$\text{RMSNorm}(x) = \gamma \odot \frac{x}{\text{RMS}(x)}$$

$$\text{RMS}(x) = \sqrt{\frac{1}{d} \sum_{i} x_i^2}$$

Differences from LayerNorm:
- ❌ No mean subtraction (no recentering)
- ❌ No bias term $\beta$
- ✅ ~10-15% faster (fewer operations)
- ✅ Empirically equivalent or better performance

Why RMSNorm works: the mean subtraction in LayerNorm is often unnecessary.
What matters is **SCALE normalization**, not centering.

> 🎒 **Beginner Analogy**: LayerNorm = first center everyone at zero, THEN adjust volume. RMSNorm = just adjust the volume directly (skip the centering step). Turns out centering doesn't help much, so why waste time? ⏩

| Normalization | Formula | Speed | Used In |
|--------------|---------|-------|---------|
| LayerNorm | $\gamma \odot \frac{x - \mu}{\sqrt{\sigma^2 + \epsilon}} + \beta$ | Baseline | BERT, GPT-2 |
| RMSNorm | $\gamma \odot \frac{x}{\text{RMS}(x)}$ | ~10-15% faster 🚀 | LLaMA, Mistral, Qwen |

> 📚 **Research**: Zhang & Sennrich (2019) — *"Root Mean Square Layer Normalization"* showed RMSNorm achieves comparable performance with less computation. Now adopted by all major open-source LLMs.

---

# 4️⃣ Positional Encoding — Giving Transformers a Sense of Order 📍

> 🎒 **Beginner Analogy**: Imagine reading a sentence where all the words are written on separate cards and thrown into a pile. You can see every word, but you have NO idea what order they go in! "dog bites man" and "man bites dog" look the same to you. **Positional encoding** is like numbering each card — now the order is clear! 🔢

## ⭐ The Problem

Self-attention is **PERMUTATION EQUIVARIANT**:
If you shuffle the input tokens, the output shuffles identically.
The model has NO notion of position, order, or sequence.

"The cat sat" and "sat cat The" produce the same attention patterns (just permuted). 😱

> 💡 **Why this matters**: Without position info, the transformer literally cannot tell the difference between "dog bites man" and "man bites dog" — those are VERY different meanings!

## ⭐ Original: Sinusoidal Positional Encoding (Vaswani et al., 2017)

$$PE(pos, 2i) = \sin\!\left(\frac{pos}{10000^{2i/d}}\right)$$

$$PE(pos, 2i+1) = \cos\!\left(\frac{pos}{10000^{2i/d}}\right)$$

Properties:
- ✅ Deterministic (no learned parameters)
- ✅ $PE(pos+k)$ can be represented as a linear function of $PE(pos)$
- ✅ Different dimensions encode position at different frequencies

Limitation: added to embeddings → position and content information are mixed. ⚠️

> 🎒 **Analogy**: Like adding a GPS coordinate to each word — but the coordinate gets mixed in with the word's meaning, which can cause confusion.

## ⭐ Learned Positional Encoding (GPT-2)

$$\text{Embed} = \text{TokenEmbed}(x) + \text{PositionEmbed}(pos)$$

Where PositionEmbed is a learned embedding table of shape $(\text{max\_length}, d_{\text{model}})$.

Limitation: **FIXED maximum length**. Can't extrapolate beyond training length. ❌

## ⭐ Rotary Positional Encoding (RoPE) — The Modern Standard 🏆

RoPE encodes position by **ROTATING** the query and key vectors.
This is used in Llama, Mistral, Qwen, and most modern LLMs.

> 🎒 **Analogy**: Instead of stapling a position tag onto each word (addition), RoPE **rotates** the word's direction based on its position — like turning a compass needle. Two words close together are rotated by similar angles, so their "alignment" (dot product) naturally reflects their proximity!

| Method | How It Works | Max Length | Used In |
|--------|-------------|------------|---------|
| Sinusoidal PE | Add fixed sin/cos patterns | Fixed | Original Transformer |
| Learned PE | Add learned embeddings | Fixed ❌ | GPT-2 |
| **RoPE** 🏆 | **Rotate Q and K vectors** | **Flexible** ✅ | **LLaMA, Mistral, GPT-NeoX** |

---

# 5️⃣ RoPE — Rotary Positional Encoding (Deep Dive) 🔄

> 🎒 **Beginner Analogy**: Imagine a clock face. RoPE assigns each word position a specific angle on the clock. Words at position 1 get a small rotation, position 2 gets a bit more, etc. When comparing two words, what matters is the **angle between them** (relative position), not where each one points (absolute position). Nearby words have small angle differences → high similarity. Far-apart words have large angle differences → low similarity. Elegant! ⏰

## ⭐ Core Idea

Instead of ADDING position information, **ROTATE** Q and K vectors based on position.

The attention score between positions $m$ and $n$:

$$q_m^T \cdot k_n = (R_m \cdot q)^T \cdot (R_n \cdot k) = q^T \cdot R_{n-m} \cdot k$$

The score depends **ONLY on the relative position** $(n - m)$, not absolute positions!
This is **relative positional encoding** — naturally handles variable-length sequences. ✅

## ⭐ The Rotation Matrix

For a 2D vector (pair of dimensions):

$$R(\theta) = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}$$

Rotating vector $v$ by angle $\theta$:

$$R(\theta) \cdot v = \begin{bmatrix} v_1\cos\theta - v_2\sin\theta \\ v_1\sin\theta + v_2\cos\theta \end{bmatrix}$$

## ⭐ RoPE Formula

For position $m$ and dimension pair $i$:

$$\theta_i = \frac{m}{10000^{2i/d}}$$

Apply rotation to **PAIRS** of dimensions in Q and K:
For dimensions $(2i, 2i+1)$:

$$q'_{2i} = q_{2i}\cos(\theta_i) - q_{2i+1}\sin(\theta_i)$$

$$q'_{2i+1} = q_{2i}\sin(\theta_i) + q_{2i+1}\cos(\theta_i)$$

Same for K. **V is NOT rotated** (only Q and K need positional information for attention scores). ⭐

## ⭐ Why RoPE Works

1. 🔄 **Relative position encoding**: The dot product $q_m \cdot k_n$ depends on $(m-n)$, not $m$ or $n$ individually.
2. 🆓 **No extra parameters**: Pure mathematical rotation, nothing to learn.
3. 📏 **Natural length extrapolation**: The rotation formula works for ANY position $m$.
4. ⚡ **Efficient**: Implemented as element-wise operations (not matrix multiplications).

## ⭐ RoPE Implementation Detail

The rotation at position $m$ uses frequencies:

$$\theta_i = \frac{1}{10000^{2i/d}} \quad \text{for } i = 0, 1, \ldots, d/2 - 1$$

| Dimension | Frequency | Captures |
|-----------|-----------|----------|
| Low dimensions (small $i$) | LOW frequency 🐌 | LONG-range position (far-apart words) |
| High dimensions (large $i$) | HIGH frequency ⚡ | SHORT-range position (nearby words) |

This multi-frequency encoding allows the model to represent position at **multiple scales**.

## ⭐ Extended Context with RoPE 📏→📏📏📏

To extend beyond training context (e.g., train on 4K, infer on 32K):
- **NTK-aware scaling**: adjust the base frequency (10000 → higher)
- **YaRN**: combine NTK-aware scaling with attention temperature
- **Dynamic NTK**: scale based on actual sequence length

These work because RoPE's rotational structure degrades gracefully.

> 📚 **Research**: Su et al. (2021) — *"RoFormer: Enhanced Transformer with Rotary Position Embedding"*. Extended by Code Llama (Rozière et al., 2023) with NTK-aware scaling for 100K+ context lengths. Llama-3.1 uses $\theta = 500{,}000$ for 128K context.

---

# 6️⃣ Feed-Forward Network — SwiGLU (Modern Standard) 🧠

> 🎒 **Beginner Analogy**: If attention is the **group discussion** where words talk to each other, the FFN is **private think time** — each word goes into its own little room and thinks deeply about what it learned from the discussion. The FFN expands the word's understanding into a bigger space (like spreading out your notes on a large desk), processes it, then compresses it back down. 📝

## ⭐ Original FFN (Vaswani 2017)

$$\text{FFN}(x) = W_2 \cdot \text{ReLU}(W_1 \cdot x + b_1) + b_2$$

Where $W_1 \in \mathbb{R}^{d_{ff} \times d}$ and $W_2 \in \mathbb{R}^{d \times d_{ff}}$, typically $d_{ff} = 4d$.

> 🎒 **In plain English**: Take the word → expand it to 4× bigger → apply a "keep only positive values" filter (ReLU) → compress it back to original size. Like inflating a balloon 🎈, looking at it carefully, then deflating it.

## ⭐ SwiGLU FFN (Llama, Mistral) — The Modern Standard 🏆

$$\text{SwiGLU}(x) = \bigl(\text{Swish}(W_1 \cdot x) \odot W_3 \cdot x\bigr) \cdot W_2$$

Where:
- $\text{Swish}(x) = x \cdot \sigma(\beta x)$, with $\beta$ typically = 1 (also called SiLU)
- $W_1, W_3 \in \mathbb{R}^{d_{ff} \times d}$ — two "gate" projections
- $W_2 \in \mathbb{R}^{d \times d_{ff}}$ — down projection
- $\odot$ = element-wise multiplication (the "gating")

> 🎒 **Beginner Analogy — The Gating Mechanism**: Imagine two reviewers reading the same essay. Reviewer 1 ($W_1$) decides "which ideas are important" (the gate). Reviewer 2 ($W_3$) provides "all the ideas." The gate ⊙ multiply means: only the ideas Reviewer 1 approves get through! This selective filtering is what makes SwiGLU powerful.

## ⭐ Why SwiGLU?

The **GATING mechanism** (element-wise multiply of two projections) allows the network to:
- 🎯 Selectively activate different dimensions
- 🧮 Learn more complex nonlinear functions per token
- 📊 Empirically: ~1-2% better performance than ReLU FFN at same compute

## ⭐ Parameter Count Comparison

| FFN Type | Formula | Params (with $d_{ff} = 4d$) |
|----------|---------|----------------------------|
| Standard FFN | $2 \times d \times d_{ff}$ | $8d^2$ |
| SwiGLU FFN | $3 \times d \times d_{ff}$ (three weight matrices) | Would be $12d^2$ ❌ |
| SwiGLU (adjusted) | $3 \times d \times \frac{2}{3}(4d)$ = $3 \times d \times 2.67d$ | $\approx 8d^2$ ✅ |

To match parameter count, Llama uses $d_{ff} = \frac{2}{3} \times 4d \approx 2.67d$:

$$\text{SwiGLU params} = 3 \times d \times 2.67d = 8d^2 \quad \text{(same as standard FFN)}$$

Llama-3 8B: $d = 4096$, $d_{ff} = 14336$ (which is $3.5d$ — slightly larger for performance). 🦙

> 📚 **Research**: Shazeer (2020) — *"GLU Variants Improve Transformer"* showed SwiGLU outperforms all other FFN variants. Now standard in LLaMA, Mistral, PaLM, and Gemini.

---

# 7️⃣ Grouped Query Attention (GQA) — Memory-Efficient Multi-Head Attention 🧩

> 🎒 **Beginner Analogy**: Imagine 32 detectives (query heads) investigating a case. In standard MHA, each detective has their **own copy** of all the evidence files (K, V) — 32 copies total! 📁📁📁... That's expensive. With GQA, every 4 detectives **share one evidence room** — only 8 rooms needed instead of 32. The detectives still ask their own unique questions, they just look at shared evidence. Saves 4× the filing cabinets! 🗄️

## ⭐ The KV Cache Memory Problem 💾

Standard Multi-Head Attention (MHA):
- Each head has its own Q, K, V projections
- KV cache size per token: $2 \times n_{\text{heads}} \times d_k \times n_{\text{layers}} \times \text{sizeof}(\text{dtype})$

For Llama-3 8B with 32 heads, $d_k=128$, 32 layers, fp16:

$$\text{KV cache per token} = 2 \times 32 \times 128 \times 32 \times 2 \text{ bytes} = 524{,}288 \text{ bytes} = 512\text{ KB}$$

For 8192 tokens: $512\text{ KB} \times 8192 = \mathbf{4\text{ GB}}$ of KV cache! 😱

For a batch of 32 sequences: **128 GB** just for KV cache. This dominates GPU memory.

## ⭐ Multi-Query Attention (MQA) — The Extreme Solution

ALL heads share the SAME K and V projections.
Only Q is per-head. K and V have 1 head each.

- KV cache reduction: **32×** (from 32 heads to 1) 🚀
- Trade-off: ~1-2% quality loss ⚠️

## ⭐ Grouped Query Attention (GQA) — The Sweet Spot 🏆

Split heads into **GROUPS**. Each group shares K and V.

Example: 32 query heads, 8 KV groups → 4 query heads per KV group.

```
Q heads:  Q1  Q2  Q3  Q4 | Q5  Q6  Q7  Q8 | ... (32 total)
KV groups: K1,V1          | K2,V2          | ... (8 total)
```

Heads Q1-Q4 share K1,V1. Heads Q5-Q8 share K2,V2. Etc.

## ⭐ GQA Formula

For group $g$ containing query heads $\{h_1, \ldots, h_m\}$:

$$K_g = X \cdot W_K^g \quad \text{(shared key for group)}$$

$$V_g = X \cdot W_V^g \quad \text{(shared value for group)}$$

For head $h_i$ in group $g$:

$$Q_{h_i} = X \cdot W_Q^{h_i} \quad \text{(individual query)}$$

$$\text{Attention}_{h_i} = \text{softmax}\!\left(\frac{Q_{h_i} \cdot K_g^T}{\sqrt{d_k}}\right) \cdot V_g$$

## ⭐ Why GQA Works

Each query head computes its OWN query (what to look for).
But keys and values (what's available) are SHARED within a group.

> 🎒 **Intuition**: Different detectives ask different questions, but the **evidence database** they search is shared. This is remarkably effective because most of the "intelligence" is in the **questions** (queries), not the evidence (keys/values).

## ⭐ GQA in Modern Models

| Model | Q Heads | KV Heads | Type | KV Cache Reduction |
|-------|---------|----------|------|-------------------|
| 🦙 Llama-2 70B | 64 | 8 | GQA | **8×** |
| 🦙 Llama-3 8B | 32 | 8 | GQA | **4×** |
| 🦙 Llama-3 70B | 64 | 8 | GQA | **8×** |
| 🌪️ Mistral 7B | 32 | 8 | GQA | **4×** |
| 🧠 GPT-3 | 96 | 96 | MHA (no sharing) | 1× |
| 🌴 PaLM | 16 | 1 | MQA | **16×** |

> 📚 **Research**: Ainslie et al. (2023) — *"GQA: Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints"* showed GQA achieves near-MHA quality with dramatic memory savings. Now standard in all major LLMs.

---

# 8️⃣ KV Caching — Making Inference 100x Faster ⚡

> 🎒 **Beginner Analogy**: Imagine you're writing a story, and every time you add a new word, you have to **re-read the entire story from the beginning** to decide what comes next. For a 4000-word story, adding word #4001 means re-reading all 4000 words. That's insanely slow! 🐌 **KV caching** is like keeping sticky notes 📌 of what you already thought about — when you add word #4001, you just read your notes instead of re-reading everything. Instant speedup!

## ⭐ The Problem: Redundant Computation 🔄

During autoregressive generation, we generate one token at a time:

```
Step 1: Process [The]              → predict "cat"
Step 2: Process [The, cat]         → predict "sat"
Step 3: Process [The, cat, sat]    → predict "on"
```

❌ **WITHOUT caching**: at step 3, we recompute attention for ALL tokens including "The" and "cat."
This means: step $T$ requires $O(T^2)$ computation. Total for $T$ tokens: $O(T^3)$! 😱

## ⭐ The Solution: Cache K and V 📌

Observation: K and V for past tokens **DON'T CHANGE** (causal attention only looks backward).
Only the NEW token's K and V need to be computed.

At step $t$:
1. Compute $Q_t, K_t, V_t$ for ONLY the new token
2. Append $K_t$ to cached keys: $K_{\text{cache}} = [K_1, K_2, \ldots, K_t]$
3. Append $V_t$ to cached values: $V_{\text{cache}} = [V_1, V_2, \ldots, V_t]$
4. Compute attention: $\text{softmax}\!\left(\frac{Q_t \cdot K_{\text{cache}}^T}{\sqrt{d_k}}\right) \cdot V_{\text{cache}}$

| Metric | Without KV Cache ❌ | With KV Cache ✅ |
|--------|-------------------|-----------------|
| Step $t$ computation | $O(t^2 \times d)$ | $O(t \times d)$ |
| Total for $T$ tokens | $O(T^3 \times d)$ 🐌 | $O(T^2 \times d)$ 🚀 |
| Speedup for 4096 tokens | Baseline | **~4096× faster!** |

> 🎒 **In plain English**: Instead of re-reading the entire book every time you write a new word, you just check your notes (the cache) and write the next word. From $O(T^3)$ to $O(T^2)$ — this is what makes ChatGPT respond in seconds instead of minutes!

## ⭐ KV Cache Memory Analysis 💾

Per layer, per token:
- Key cache: $n_{\text{kv\_heads}} \times d_k$ values
- Value cache: $n_{\text{kv\_heads}} \times d_v$ values
- Total: $2 \times n_{\text{kv\_heads}} \times d_k$ values

For full model with $L$ layers, sequence length $T$:

$$\text{Cache size} = 2 \times L \times n_{\text{kv\_heads}} \times d_k \times T \times \text{sizeof}(\text{dtype})$$

## ⭐ Why GQA Reduces KV Cache 📉

With GQA (fewer KV heads), the cache is smaller:

| Configuration | KV Cache per Token | For 8192 Tokens |
|--------------|-------------------|-----------------|
| Full MHA (32 KV heads) | $2 \times 32 \times 128 \times 32 \times 2 = 524{,}288$ bytes | **4 GB** 😱 |
| GQA (8 KV heads) | $2 \times 32 \times 8 \times 128 \times 2 = 131{,}072$ bytes | **1 GB** ✅ |
| **Reduction** | | **4× smaller!** 🚀 |

4× reduction! This directly translates to 4× larger batch sizes or 4× longer context.

## ⭐ KV Cache: Prefill vs. Decode 🔀

| Phase | What Happens | Speed | Bottleneck |
|-------|-------------|-------|------------|
| **Prefill** 📥 | Process entire prompt at once (parallel) | Fast ⚡ | Compute-bound |
| **Decode** 📤 | Generate tokens one at a time (sequential) | Slower 🔄 | Memory-bound (reading KV cache) |

**Prefill phase**: Process the entire prompt at once (parallel).
- Compute K, V for all prompt tokens simultaneously
- This IS parallelizable (like training)

**Decode phase**: Generate tokens one at a time (sequential).
- Each step appends one K, V to the cache
- Computation is memory-bound (reading KV cache from memory)
- This is where KV caching matters most

> 📚 **Research**: Pope et al. (2022) — *"Efficiently Scaling Transformer Inference"* analyzed KV cache bottlenecks. PagedAttention (Kwon et al., 2023 — vLLM) solves memory fragmentation with paged KV cache, enabling 2-4× higher throughput in production.

---

# 9️⃣ Implementation — Complete Llama-3 Style Transformer 💻🦙

> 🎒 **What's happening below**: We're building a **mini version** of the exact same architecture that powers LLaMA-3, Mistral, and other billion-parameter models — just with smaller dimensions so it fits on your laptop! Every component (RMSNorm, RoPE, GQA, SwiGLU, KV caching) is implemented from scratch. Read the comments carefully — they map directly to the math above.

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
import math
from dataclasses import dataclass
from typing import Optional, Tuple

# ═══════════════════════════════════════════════════════════
# Configuration
# ═══════════════════════════════════════════════════════════

@dataclass
class TransformerConfig:
    """Llama-3 style transformer configuration."""
    vocab_size: int = 1000       # Small for demo
    d_model: int = 256           # Model dimension
    n_layers: int = 4            # Number of transformer blocks
    n_heads: int = 8             # Number of query heads
    n_kv_heads: int = 4          # Number of KV heads (GQA)
    d_ff: int = 688              # FFN intermediate dim (~2.67 * d_model)
    max_seq_len: int = 512       # Maximum sequence length
    rope_theta: float = 10000.0  # RoPE base frequency
    norm_eps: float = 1e-5       # RMSNorm epsilon
    dropout: float = 0.1         # Dropout rate


# ═══════════════════════════════════════════════════════════
# RMSNorm
# ═══════════════════════════════════════════════════════════

class RMSNorm(nn.Module):
    """Root Mean Square Layer Normalization (used in Llama)."""
    
    def __init__(self, dim: int, eps: float = 1e-5):
        super().__init__()
        self.eps = eps
        self.weight = nn.Parameter(torch.ones(dim))  # γ (scale)
    
    def forward(self, x):
        # RMS(x) = sqrt(mean(x^2))
        rms = torch.sqrt(torch.mean(x ** 2, dim=-1, keepdim=True) + self.eps)
        return self.weight * (x / rms)


# ═══════════════════════════════════════════════════════════
# Rotary Positional Encoding (RoPE)
# ═══════════════════════════════════════════════════════════

def precompute_rope_frequencies(dim: int, max_seq_len: int, theta: float = 10000.0):
    """
    Precompute the RoPE rotation frequencies.
    
    Args:
        dim: dimension of the head (d_k)
        max_seq_len: maximum sequence length
        theta: base frequency (10000 in original)
    
    Returns:
        cos, sin: both of shape (max_seq_len, dim//2)
    """
    # Frequency for each dimension pair
    # θᵢ = 1 / theta^(2i/dim) for i = 0, 1, ..., dim/2 - 1
    freqs = 1.0 / (theta ** (torch.arange(0, dim, 2).float() / dim))
    
    # Position indices
    positions = torch.arange(max_seq_len).float()
    
    # Outer product: angles[pos, i] = pos * freqs[i]
    angles = torch.outer(positions, freqs)  # (max_seq_len, dim//2)
    
    # Return cos and sin
    return torch.cos(angles), torch.sin(angles)


def apply_rope(x: torch.Tensor, cos: torch.Tensor, sin: torch.Tensor):
    """
    Apply Rotary Positional Encoding to Q or K tensors.
    
    Args:
        x: (batch, n_heads, seq_len, d_k)
        cos: (seq_len, d_k//2)
        sin: (seq_len, d_k//2)
    
    Returns:
        Rotated tensor of same shape
    """
    # Split into pairs: (x1, x2) for each dimension pair
    d_k = x.shape[-1]
    x1 = x[..., :d_k//2]   # First half of each pair
    x2 = x[..., d_k//2:]   # Second half of each pair
    
    # Reshape cos/sin for broadcasting
    cos = cos[:x.shape[2]].unsqueeze(0).unsqueeze(0)  # (1, 1, seq, d_k//2)
    sin = sin[:x.shape[2]].unsqueeze(0).unsqueeze(0)
    
    # Apply rotation:
    # x'₁ = x₁ cos θ - x₂ sin θ
    # x'₂ = x₁ sin θ + x₂ cos θ
    out1 = x1 * cos - x2 * sin
    out2 = x1 * sin + x2 * cos
    
    return torch.cat([out1, out2], dim=-1)


# ═══════════════════════════════════════════════════════════
# Grouped Query Attention (GQA) with KV Cache
# ═══════════════════════════════════════════════════════════

class GroupedQueryAttention(nn.Module):
    """
    Grouped Query Attention with RoPE and KV caching.
    This is the attention mechanism used in Llama-3.
    """
    
    def __init__(self, config: TransformerConfig):
        super().__init__()
        
        self.n_heads = config.n_heads
        self.n_kv_heads = config.n_kv_heads
        self.d_k = config.d_model // config.n_heads
        self.n_groups = config.n_heads // config.n_kv_heads  # Heads per KV group
        
        # Q projection: one per query head
        self.W_Q = nn.Linear(config.d_model, config.n_heads * self.d_k, bias=False)
        # K, V projections: one per KV head (shared within groups)
        self.W_K = nn.Linear(config.d_model, config.n_kv_heads * self.d_k, bias=False)
        self.W_V = nn.Linear(config.d_model, config.n_kv_heads * self.d_k, bias=False)
        # Output projection
        self.W_O = nn.Linear(config.n_heads * self.d_k, config.d_model, bias=False)
        
        self.dropout = nn.Dropout(config.dropout)
    
    def forward(
        self, 
        x: torch.Tensor, 
        cos: torch.Tensor, 
        sin: torch.Tensor,
        mask: Optional[torch.Tensor] = None,
        kv_cache: Optional[Tuple[torch.Tensor, torch.Tensor]] = None,
        use_cache: bool = False
    ):
        """
        Args:
            x: (batch, seq_len, d_model)
            cos, sin: RoPE frequencies
            mask: causal mask
            kv_cache: tuple of (cached_K, cached_V) from previous steps
            use_cache: whether to return updated KV cache
        """
        batch_size, seq_len, _ = x.shape
        
        # Project to Q, K, V
        Q = self.W_Q(x)  # (batch, seq, n_heads * d_k)
        K = self.W_K(x)  # (batch, seq, n_kv_heads * d_k)
        V = self.W_V(x)  # (batch, seq, n_kv_heads * d_k)
        
        # Reshape to multi-head format
        Q = Q.view(batch_size, seq_len, self.n_heads, self.d_k).transpose(1, 2)
        K = K.view(batch_size, seq_len, self.n_kv_heads, self.d_k).transpose(1, 2)
        V = V.view(batch_size, seq_len, self.n_kv_heads, self.d_k).transpose(1, 2)
        # Q: (batch, n_heads, seq, d_k)
        # K, V: (batch, n_kv_heads, seq, d_k)
        
        # Apply RoPE to Q and K (NOT V!)
        Q = apply_rope(Q, cos, sin)
        K = apply_rope(K, cos, sin)
        
        # KV Cache: append new K, V to cache
        if kv_cache is not None:
            cached_K, cached_V = kv_cache
            K = torch.cat([cached_K, K], dim=2)  # Append along seq dimension
            V = torch.cat([cached_V, V], dim=2)
        
        new_cache = (K, V) if use_cache else None
        
        # Expand KV heads to match Q heads (GQA)
        # Each KV head is shared by n_groups query heads
        if self.n_kv_heads != self.n_heads:
            K = K.repeat_interleave(self.n_groups, dim=1)  # (batch, n_heads, seq, d_k)
            V = V.repeat_interleave(self.n_groups, dim=1)
        
        # Scaled dot-product attention
        scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.d_k)
        
        if mask is not None:
            scores = scores.masked_fill(mask == 0, float('-inf'))
        
        attn_weights = F.softmax(scores, dim=-1)
        attn_weights = self.dropout(attn_weights)
        
        # Apply attention to values
        attn_output = torch.matmul(attn_weights, V)
        
        # Reshape back: (batch, n_heads, seq, d_k) → (batch, seq, d_model)
        attn_output = attn_output.transpose(1, 2).contiguous()
        attn_output = attn_output.view(batch_size, seq_len, -1)
        
        # Output projection
        output = self.W_O(attn_output)
        
        return output, new_cache


# ═══════════════════════════════════════════════════════════
# SwiGLU Feed-Forward Network
# ═══════════════════════════════════════════════════════════

class SwiGLUFFN(nn.Module):
    """
    SwiGLU Feed-Forward Network (used in Llama).
    
    SwiGLU(x) = (Swish(W₁x) ⊙ W₃x) · W₂
    """
    
    def __init__(self, d_model: int, d_ff: int, dropout: float = 0.1):
        super().__init__()
        self.w1 = nn.Linear(d_model, d_ff, bias=False)  # Gate projection
        self.w2 = nn.Linear(d_ff, d_model, bias=False)   # Down projection
        self.w3 = nn.Linear(d_model, d_ff, bias=False)   # Up projection
        self.dropout = nn.Dropout(dropout)
    
    def forward(self, x):
        # SwiGLU: (Swish(W1·x) ⊙ W3·x) · W2
        # Swish(x) = x * sigmoid(x) = SiLU(x)
        gate = F.silu(self.w1(x))       # Swish activation
        up = self.w3(x)                  # Linear projection
        hidden = gate * up               # Gated activation
        hidden = self.dropout(hidden)
        output = self.w2(hidden)         # Down projection
        return output


# ═══════════════════════════════════════════════════════════
# Transformer Block (Llama-3 Style)
# ═══════════════════════════════════════════════════════════

class TransformerBlock(nn.Module):
    """
    Single transformer block with:
    - Pre-norm (RMSNorm)
    - Grouped Query Attention with RoPE
    - SwiGLU FFN
    - Residual connections
    """
    
    def __init__(self, config: TransformerConfig):
        super().__init__()
        self.attention_norm = RMSNorm(config.d_model, config.norm_eps)
        self.attention = GroupedQueryAttention(config)
        self.ffn_norm = RMSNorm(config.d_model, config.norm_eps)
        self.ffn = SwiGLUFFN(config.d_model, config.d_ff, config.dropout)
    
    def forward(self, x, cos, sin, mask=None, kv_cache=None, use_cache=False):
        # Pre-norm → Attention → Residual
        normed = self.attention_norm(x)
        attn_output, new_cache = self.attention(
            normed, cos, sin, mask, kv_cache, use_cache
        )
        x = x + attn_output
        
        # Pre-norm → FFN → Residual
        normed = self.ffn_norm(x)
        ffn_output = self.ffn(normed)
        x = x + ffn_output
        
        return x, new_cache


# ═══════════════════════════════════════════════════════════
# Complete Transformer (Llama-3 Style)
# ═══════════════════════════════════════════════════════════

class LlamaStyleTransformer(nn.Module):
    """
    Complete Llama-3 style transformer language model.
    
    Components:
    - Token embedding (no positional embedding — RoPE handles position)
    - N transformer blocks with GQA + SwiGLU + RMSNorm
    - Final RMSNorm + linear projection to vocabulary
    """
    
    def __init__(self, config: TransformerConfig):
        super().__init__()
        self.config = config
        
        # Token embedding (NO positional embedding — RoPE is applied in attention)
        self.token_embedding = nn.Embedding(config.vocab_size, config.d_model)
        
        # Transformer blocks
        self.layers = nn.ModuleList([
            TransformerBlock(config) for _ in range(config.n_layers)
        ])
        
        # Final normalization
        self.norm = RMSNorm(config.d_model, config.norm_eps)
        
        # Output projection (language model head)
        self.lm_head = nn.Linear(config.d_model, config.vocab_size, bias=False)
        
        # Weight tying: embedding = output projection
        self.lm_head.weight = self.token_embedding.weight
        
        # Precompute RoPE frequencies
        d_k = config.d_model // config.n_heads
        self.register_buffer('rope_cos', torch.zeros(config.max_seq_len, d_k // 2))
        self.register_buffer('rope_sin', torch.zeros(config.max_seq_len, d_k // 2))
        cos, sin = precompute_rope_frequencies(d_k, config.max_seq_len, config.rope_theta)
        self.rope_cos.copy_(cos)
        self.rope_sin.copy_(sin)
        
        # Initialize weights
        self.apply(self._init_weights)
    
    def _init_weights(self, module):
        if isinstance(module, nn.Linear):
            nn.init.normal_(module.weight, mean=0.0, std=0.02)
            if module.bias is not None:
                nn.init.zeros_(module.bias)
        elif isinstance(module, nn.Embedding):
            nn.init.normal_(module.weight, mean=0.0, std=0.02)
    
    def forward(
        self, 
        input_ids: torch.Tensor,
        past_kv_caches: Optional[list] = None,
        use_cache: bool = False
    ):
        """
        Args:
            input_ids: (batch, seq_len) — token indices
            past_kv_caches: list of (K, V) tuples, one per layer
            use_cache: whether to return KV caches for next step
        
        Returns:
            logits: (batch, seq_len, vocab_size)
            new_kv_caches: list of (K, V) tuples if use_cache=True
        """
        batch_size, seq_len = input_ids.shape
        
        # Determine position offset (for KV cache continuation)
        start_pos = 0
        if past_kv_caches is not None and past_kv_caches[0] is not None:
            start_pos = past_kv_caches[0][0].shape[2]  # Length of cached sequence
        
        # Get RoPE for current positions
        cos = self.rope_cos[start_pos:start_pos + seq_len]
        sin = self.rope_sin[start_pos:start_pos + seq_len]
        
        # Create causal mask
        if seq_len > 1:
            mask = torch.tril(torch.ones(seq_len, seq_len, device=input_ids.device))
            # If we have cache, we need to attend to cached tokens too
            if start_pos > 0:
                # Can attend to all cached positions + causal within new tokens
                cache_mask = torch.ones(seq_len, start_pos, device=input_ids.device)
                mask = torch.cat([cache_mask, mask], dim=-1)
            mask = mask.unsqueeze(0).unsqueeze(0)  # (1, 1, seq, total_seq)
        else:
            mask = None  # Single token — no masking needed
        
        # Token embeddings
        x = self.token_embedding(input_ids)  # (batch, seq, d_model)
        
        # Apply transformer blocks
        new_kv_caches = []
        for i, layer in enumerate(self.layers):
            past_kv = past_kv_caches[i] if past_kv_caches is not None else None
            x, new_cache = layer(x, cos, sin, mask, past_kv, use_cache)
            new_kv_caches.append(new_cache)
        
        # Final norm + projection
        x = self.norm(x)
        logits = self.lm_head(x)  # (batch, seq, vocab_size)
        
        if use_cache:
            return logits, new_kv_caches
        return logits, None


# ═══════════════════════════════════════════════════════════
# Tests and Demonstrations
# ═══════════════════════════════════════════════════════════

print("=" * 70)
print("LLAMA-3 STYLE TRANSFORMER — Complete Implementation")
print("=" * 70)

config = TransformerConfig()
model = LlamaStyleTransformer(config)

# Count parameters
total_params = sum(p.numel() for p in model.parameters())
print(f"\nModel Configuration:")
print(f"  d_model: {config.d_model}")
print(f"  n_layers: {config.n_layers}")
print(f"  n_heads (Q): {config.n_heads}")
print(f"  n_kv_heads: {config.n_kv_heads}")
print(f"  d_ff: {config.d_ff}")
print(f"  GQA groups: {config.n_heads // config.n_kv_heads} Q heads per KV head")
print(f"  Total parameters: {total_params:,}")

# Test 1: Forward pass
print("\n--- Test 1: Forward Pass ---")
batch_size = 2
seq_len = 32
input_ids = torch.randint(0, config.vocab_size, (batch_size, seq_len))

logits, _ = model(input_ids)
print(f"Input shape: {input_ids.shape}")
print(f"Output logits shape: {logits.shape}")
print(f"Expected: ({batch_size}, {seq_len}, {config.vocab_size})")

# Test 2: KV caching — verify correctness
print("\n--- Test 2: KV Cache Correctness ---")
model.eval()
with torch.no_grad():
    # Full forward pass
    full_logits, _ = model(input_ids)
    
    # Incremental forward pass with KV caching
    # Step 1: Process first token
    first_logits, kv_caches = model(input_ids[:, :1], use_cache=True)
    
    # Steps 2+: Process one token at a time
    all_logits = [first_logits]
    for t in range(1, seq_len):
        next_logits, kv_caches = model(
            input_ids[:, t:t+1], 
            past_kv_caches=kv_caches,
            use_cache=True
        )
        all_logits.append(next_logits)
    
    # Concatenate incremental logits
    incremental_logits = torch.cat(all_logits, dim=1)
    
    # Compare: should be (nearly) identical
    max_diff = (full_logits - incremental_logits).abs().max().item()
    print(f"Max difference between full and cached: {max_diff:.6e}")
    print(f"Caching correct: {max_diff < 1e-4}")

# Test 3: KV cache memory analysis
print("\n--- Test 3: KV Cache Memory Analysis ---")
d_k = config.d_model // config.n_heads
kv_per_token = 2 * config.n_kv_heads * d_k * config.n_layers
print(f"KV cache per token per layer: {2 * config.n_kv_heads * d_k} values")
print(f"KV cache per token (all layers): {kv_per_token} values")
print(f"KV cache per token (fp16): {kv_per_token * 2 / 1024:.2f} KB")
print(f"KV cache for 2048 tokens (fp16): {kv_per_token * 2048 * 2 / 1024 / 1024:.2f} MB")

# Compare with full MHA
kv_full_mha = 2 * config.n_heads * d_k * config.n_layers
print(f"\nWith full MHA (no GQA): {kv_full_mha * 2048 * 2 / 1024 / 1024:.2f} MB")
print(f"GQA reduction factor: {config.n_heads / config.n_kv_heads}x")

# Test 4: RoPE verification
print("\n--- Test 4: RoPE Relative Position Property ---")
d_k_test = 16
cos_test, sin_test = precompute_rope_frequencies(d_k_test, 100)

# Create two queries at positions 5 and 10, and a key at position 20
q = torch.randn(1, 1, 1, d_k_test)

# Apply RoPE at different positions
cos_5 = cos_test[5:6].unsqueeze(0).unsqueeze(0)
sin_5 = sin_test[5:6].unsqueeze(0).unsqueeze(0)
cos_10 = cos_test[10:11].unsqueeze(0).unsqueeze(0)
sin_10 = sin_test[10:11].unsqueeze(0).unsqueeze(0)
cos_15 = cos_test[15:16].unsqueeze(0).unsqueeze(0)
sin_15 = sin_test[15:16].unsqueeze(0).unsqueeze(0)
cos_20 = cos_test[20:21].unsqueeze(0).unsqueeze(0)
sin_20 = sin_test[20:21].unsqueeze(0).unsqueeze(0)

q_at_5 = apply_rope(q, cos_test, sin_test)
# Verify the rotation was applied
print(f"Original q norm: {q.norm():.4f}")
print(f"Rotated q norm: {q_at_5.norm():.4f}")
print(f"Norms are equal (rotation preserves norm): {abs(q.norm() - q_at_5.norm()) < 1e-5}")


# Test 5: Generation demo
print("\n--- Test 5: Autoregressive Generation with KV Cache ---")
model.eval()

def generate(model, prompt_ids, max_new_tokens=20, temperature=1.0):
    """Generate text autoregressively with KV caching."""
    input_ids = prompt_ids.unsqueeze(0)  # Add batch dim
    
    # Prefill: process entire prompt
    with torch.no_grad():
        logits, kv_caches = model(input_ids, use_cache=True)
    
    # Get last token prediction
    next_token_logits = logits[0, -1] / temperature
    probs = F.softmax(next_token_logits, dim=-1)
    next_token = torch.multinomial(probs, 1)
    
    generated = [next_token.item()]
    
    # Decode: generate one token at a time
    for _ in range(max_new_tokens - 1):
        with torch.no_grad():
            logits, kv_caches = model(
                next_token.unsqueeze(0),
                past_kv_caches=kv_caches,
                use_cache=True
            )
        
        next_token_logits = logits[0, -1] / temperature
        probs = F.softmax(next_token_logits, dim=-1)
        next_token = torch.multinomial(probs, 1)
        generated.append(next_token.item())
    
    return generated

prompt = torch.randint(0, config.vocab_size, (10,))
generated_ids = generate(model, prompt, max_new_tokens=20)
print(f"Prompt length: {len(prompt)}")
print(f"Generated tokens: {len(generated_ids)}")
print(f"Prompt IDs: {prompt.tolist()}")
print(f"Generated IDs: {generated_ids}")


# ═══════════════════════════════════════════════════════════
# Parameter Breakdown (like real model analysis)
# ═══════════════════════════════════════════════════════════

print("\n" + "=" * 70)
print("PARAMETER BREAKDOWN")
print("=" * 70)

def count_params(module, name=""):
    total = sum(p.numel() for p in module.parameters())
    return total

print(f"\n{'Component':<30} {'Parameters':>12}")
print("-" * 44)
print(f"{'Token Embedding':<30} {count_params(model.token_embedding):>12,}")

for i, layer in enumerate(model.layers):
    attn_params = count_params(layer.attention)
    ffn_params = count_params(layer.ffn)
    norm_params = count_params(layer.attention_norm) + count_params(layer.ffn_norm)
    print(f"{'Layer ' + str(i) + ' Attention':<30} {attn_params:>12,}")
    print(f"{'Layer ' + str(i) + ' FFN':<30} {ffn_params:>12,}")
    print(f"{'Layer ' + str(i) + ' Norms':<30} {norm_params:>12,}")

print(f"{'Final Norm':<30} {count_params(model.norm):>12,}")
print(f"{'LM Head (tied)':<30} {'(tied)':>12}")
print("-" * 44)
print(f"{'TOTAL':<30} {total_params:>12,}")


# ═══════════════════════════════════════════════════════════
# Scaling Comparison: Real Models
# ═══════════════════════════════════════════════════════════

print("\n" + "=" * 70)
print("SCALING COMPARISON: Real Model Architectures")
print("=" * 70)

real_models = [
    ("GPT-2 Small",    768,  12, 12, 12, 3072,   50257),
    ("GPT-2 Medium",   1024, 24, 16, 16, 4096,   50257),
    ("Llama-3 8B",     4096, 32, 32, 8,  14336,  128256),
    ("Llama-3 70B",    8192, 80, 64, 8,  28672,  128256),
    ("Mistral 7B",     4096, 32, 32, 8,  14336,  32000),
]

print(f"\n{'Model':<18} {'d_model':>7} {'Layers':>7} {'Q Heads':>8} {'KV Heads':>9} {'d_ff':>7} {'~Params':>10}")
print("-" * 70)

for name, d, layers, q_h, kv_h, ff, vocab in real_models:
    d_k = d // q_h
    # Approximate param count
    embedding = vocab * d
    per_layer_attn = d * (q_h * d_k) + 2 * d * (kv_h * d_k) + d * d  # QKV + O
    per_layer_ffn = 3 * d * ff  # SwiGLU: W1, W2, W3
    per_layer_norm = 2 * d  # Two RMSNorms
    total = embedding + layers * (per_layer_attn + per_layer_ffn + per_layer_norm)
    
    if total > 1e9:
        param_str = f"{total/1e9:.1f}B"
    else:
        param_str = f"{total/1e6:.0f}M"
    
    print(f"{name:<18} {d:>7} {layers:>7} {q_h:>8} {kv_h:>9} {ff:>7} {param_str:>10}")
```

---

# 🔟 Deep Reflection Questions (Research Mindset) 🔬🧠

> 💡 These questions are designed to push beyond memorization into **true understanding**. Try to answer each one BEFORE looking up the answer — that's where the real learning happens!

1. 🤔 The original Transformer used post-norm: $\text{Norm}(x + \text{Attention}(x))$.
   Modern LLMs use pre-norm: $x + \text{Attention}(\text{Norm}(x))$.
   Why does pre-norm train better? What's the gradient flow difference?
   > *Hint: Think about what happens to the gradient when it passes through LayerNorm vs. through a residual shortcut.*

2. 🔄 RoPE encodes position through **ROTATION**. Sinusoidal PE uses **ADDITION**.
   Why is rotation superior? What happens when position info and content info
   are added (potential interference) vs. rotated (orthogonal encoding)?
   > *Hint: Addition mixes the information; rotation preserves the original magnitudes.*

3. 🧩 GQA uses fewer KV heads than Q heads. This saves memory but:
   what information is LOST when multiple query heads share one KV head?
   Can different query heads still attend to different things?
   > *Hint: Queries are still unique per head — they just search the same "database."*

4. 💾 KV caching makes inference $O(T^2)$ total instead of $O(T^3)$.
   But the MEMORY grows as $O(T)$. For very long contexts (1M tokens),
   how much memory does the KV cache need for Llama-3 70B?
   Is this feasible? What solutions exist?
   > *Hint: Calculate using the formula: $2 \times L \times n_{\text{kv}} \times d_k \times T \times 2$ bytes.*

5. 🚪 SwiGLU has a GATING mechanism: $\text{Swish}(W_1 x) \odot W_3 x$.
   Why does gating help? Connect this to the gating principle from LSTMs.
   What would happen if you replaced SwiGLU with standard ReLU FFN?
   > *Hint: Gating = selective information flow. LSTMs gate with forget/input gates. SwiGLU gates with a learned multiplicative mask.*

6. 🔗 Weight tying (embedding = output projection) reduces parameters significantly.
   Why does this work? What does it mean that the "input representation" of a token
   is the same as its "output prediction" representation?
   > *Hint: If token A's embedding is close to token B's embedding, the model naturally considers B as a likely next token after A-like contexts.*

7. 🏗️ Transformers with 32 layers have 32 residual connections.
   The output is: $x + \text{layer}_1(x) + \text{layer}_2(\ldots) + \ldots + \text{layer}_{32}(\ldots)$.
   Some research shows REMOVING certain layers barely affects performance.
   What does this say about transformer depth and redundancy?
   > *Hint: Think about the ensemble interpretation — most paths are short.*

8. 📏 RoPE uses base $\theta = 10{,}000$. Llama-3.1 uses $\theta = 500{,}000$ for 128K context.
   Why does increasing $\theta$ extend context length? What's the mathematical relationship
   between base frequency and maximum effective position?
   > *Hint: Higher $\theta$ = lower frequencies = slower rotation = positions stay "distinguishable" for longer.*

---

# 🚀 Startup Founder Insight — Why This Is Worth Millions 💰

You just implemented the **CORE architecture** that powers GPT-4, Claude, Gemini, LLaMA, and every major LLM. This isn't academic — this is the production architecture of the most valuable technology in the world. 🌍

What this knowledge enables for a startup founder:

### 🏗️ Architecture Decisions
Should you use GQA or MHA? How many KV heads? This directly affects inference cost (your biggest expense).

### ⚡ Inference Optimization
KV caching, quantization, batching strategies — all require understanding the architecture at the level you now have. The difference between naive and optimized inference can be **10-100× in cost**.

### 🎯 Model Selection
When choosing between Llama, Mistral, or Qwen for your product, you can read the architecture specs and predict performance characteristics. *"Mistral uses sliding window attention → great for long conversations but may miss very long-range dependencies."*

### 🔧 Fine-tuning Strategy
Understanding the attention mechanism tells you exactly WHICH layers to fine-tune (LoRA adapters on Q, K, V projections), how to add custom positional encodings, and why certain fine-tuning approaches fail.

### 💵 Cost Estimation
You can now calculate exact compute and memory costs:

| Cost Factor | Formula |
|------------|---------|
| FLOPs per token (forward) | $2 \times n_{\text{params}}$ |
| FLOPs per token (training) | $\approx 6 \times n_{\text{params}}$ |
| KV cache per user | $2 \times n_{\text{kv\_heads}} \times d_k \times n_{\text{layers}} \times \text{context\_len} \times \text{dtype\_size}$ |
| $/request for your API | Directly derived from above! |

The transformer IS the engine of the AI revolution. You now understand every component inside it. 🔥

---

# 📚 Deep Research — Key Papers & Timeline 🗓️

| Year | Paper | Authors | Key Contribution |
|------|-------|---------|-----------------|
| 2017 | *"Attention Is All You Need"* | Vaswani et al. | ⭐ Invented the Transformer architecture |
| 2018 | *"Improving Language Understanding by Generative Pre-Training"* | Radford et al. (OpenAI) | ⭐ GPT-1: decoder-only pre-training |
| 2019 | *"BERT: Pre-training of Deep Bidirectional Transformers"* | Devlin et al. (Google) | ⭐ Encoder-only transformer for NLP |
| 2020 | *"On Layer Normalization in the Transformer Architecture"* | Xiong et al. | Pre-LN proven superior for stability |
| 2020 | *"Scaling Laws for Neural Language Models"* | Kaplan et al. (OpenAI) | ⭐ Power-law scaling of loss vs. compute/data/params |
| 2020 | *"GLU Variants Improve Transformer"* | Shazeer (Google) | SwiGLU FFN outperforms ReLU variants |
| 2021 | *"RoFormer: Enhanced Transformer with Rotary Position Embedding"* | Su et al. | RoPE: rotational positional encoding |
| 2022 | *"PaLM: Scaling Language Modeling with Pathways"* | Chowdhery et al. (Google) | 540B parameter model scaling insights |
| 2023 | *"GQA: Training Generalized Multi-Query Transformer"* | Ainslie et al. (Google) | GQA: memory-efficient attention |
| 2023 | *"LLaMA: Open and Efficient Foundation Language Models"* | Touvron et al. (Meta) | Open-source reference architecture |

---

# 📝 Homework (Required) ✏️

1. 🏗️ **Scale test**: Modify the config to create a ~50M parameter model. Train it on a character-level text corpus (Project Gutenberg). How does it compare to the LSTM from Day 38?

2. 📏 **RoPE extrapolation**: Train with max_seq_len=128, then test with seq_len=256 and 512. Does RoPE extrapolate? Try NTK-aware scaling (multiply theta by a factor) and compare.

3. 🧩 **GQA ablation**: Compare n_kv_heads = [1, 2, 4, 8, 32] (MQA → MHA). Plot perplexity vs. KV cache size. Where's the sweet spot?

4. ⏱️ **KV cache profiling**: Measure wall-clock time for generating 100 tokens WITH and WITHOUT KV caching. What's the actual speedup on your hardware?

5. ⚡ **Implement FlashAttention pseudocode**: Write a block-by-block attention computation that doesn't materialize the full $n \times n$ matrix. Compare memory usage against the naive implementation.

6. 🔀 **Architecture comparison**: Build both a POST-norm and PRE-norm version of the same transformer. Train both on the same data for 1000 steps. Which converges faster? Which is more stable at high learning rates?

7. 🔍 **Real model inspection**: Use HuggingFace to load Llama-3-8B's config. Verify that every parameter you learned about today (n_heads, n_kv_heads, d_ff, RoPE theta, RMSNorm epsilon) matches your implementation's structure.

---

# 🏁 CHEAT SHEET — Full Transformer Block ⚡

> **Print this. Pin it to your wall. Reference it while reading papers.** 📌

## Core Transformer Block (Pre-Norm — Modern Standard)

$$x = x + \text{MultiHeadAttention}\bigl(\text{LayerNorm}(x)\bigr)$$
$$x = x + \text{FFN}\bigl(\text{LayerNorm}(x)\bigr)$$

## Key Formulas at a Glance

| Component | Formula | Notes |
|-----------|---------|-------|
| **Residual** | $\text{output} = x + \text{sublayer}(x)$ | Gradient highway — identity ensures flow |
| **LayerNorm** | $\gamma \odot \frac{x - \mu}{\sqrt{\sigma^2 + \epsilon}} + \beta$ | Normalize per-sample across features |
| **RMSNorm** | $\gamma \odot \frac{x}{\sqrt{\frac{1}{d}\sum x_i^2}}$ | Faster — no mean subtraction |
| **FFN (original)** | $\text{GELU}(xW_1 + b_1)W_2 + b_2$ | Expand → activate → compress |
| **SwiGLU** | $(\text{Swish}(W_1 x) \odot W_3 x) W_2$ | Gated FFN — used in LLaMA/Mistral |
| **RoPE** | $\theta_i = \frac{m}{10000^{2i/d}}$, rotate Q & K | Relative positional encoding |
| **GQA Attention** | $\text{softmax}\!\left(\frac{Q_{h_i} K_g^T}{\sqrt{d_k}}\right) V_g$ | Share K,V across Q head groups |
| **KV Cache** | $\text{softmax}\!\left(\frac{Q_t \cdot K_{\text{cache}}^T}{\sqrt{d_k}}\right) V_{\text{cache}}$ | Append new K,V; reuse old ones |
| **Param count** | $\approx 12d^2 \cdot L$ | Total transformer parameters |

## Architecture Variants

| Type | Flow | Examples |
|------|------|---------|
| Encoder-only 📖 | Input → Bidirectional attention → Output | BERT, RoBERTa |
| Decoder-only ✍️ | Input → Causal (masked) attention → Output | GPT-4, Claude, LLaMA, Gemini |
| Encoder-Decoder 📖→✍️ | Encoder reads → Cross-attention → Decoder writes | T5, Whisper, BART |

## Normalization Quick Reference

| Pre-Norm (Modern) ✅ | Post-Norm (Original) ❌ |
|---------------------|----------------------|
| $x + \text{Attn}(\text{Norm}(x))$ | $\text{Norm}(x + \text{Attn}(x))$ |
| Stable training, no warmup needed | May need warmup, less stable |
| GPT-3+, LLaMA, Mistral, Claude | Original Transformer (2017) |

## Memory & Speed Quick Reference

| Optimization | Benefit | Cost |
|-------------|---------|------|
| KV Caching | $O(T^3) \to O(T^2)$ inference ⚡ | Memory grows as $O(T)$ |
| GQA (8 vs 32 heads) | 4× less KV cache memory 💾 | ~0.5% quality trade-off |
| RMSNorm (vs LayerNorm) | 10-15% faster normalization 🚀 | Negligible quality difference |
| SwiGLU (vs ReLU FFN) | ~1-2% better quality 📈 | 3 weight matrices instead of 2 |
| Weight Tying | Save $d \times V$ parameters 💾 | None (usually helps!) |

## Beginner Analogy Summary 🎒

| Component | One-Line Analogy |
|-----------|-----------------|
| Transformer Block | One **floor** of a smart building (stack floors = stack blocks) |
| Attention | **Group discussion** — all words talk to each other |
| FFN | **Private think time** — each word reflects alone |
| Residual Connection | **Express elevator** — information bypasses floors directly |
| LayerNorm/RMSNorm | **Volume equalizer** — keeps numbers at comfortable levels |
| RoPE | **Clock angles** — nearby words = similar angles = high similarity |
| GQA | **Shared evidence rooms** — detectives ask unique questions, share files |
| KV Cache | **Sticky notes** — remember past work instead of re-reading everything |
| Pre-Norm | **Prep before each step** — organize before you work, not after |
| Encoder | 📖 **Reading** and understanding |
| Decoder | ✍️ **Writing** the answer |
| Cross-Attention | 👀 **Peeking back** at the encoder's understanding |
