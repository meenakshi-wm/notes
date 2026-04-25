📘 DAY 11 — GPT Architecture: The Complete Decoder-Only Transformer

> 🎯 **One-Line Summary**: GPT is the "autocomplete engine" behind ChatGPT — it reads text left-to-right and predicts the next word, one token at a time. That's it. Everything else is engineering to make that prediction *really* good.

---

## 🌟 Why Should You Care? (Even If You Know ZERO About AI)

Imagine your phone's autocomplete — when you type "I'm going to the", it suggests "store" or "gym." Now imagine that autocomplete read the *entire internet* and can write essays, code, poetry, and pass law exams. **That's GPT.**

Every time you use ChatGPT, Claude, Gemini, or any AI chatbot, a GPT-style architecture is running under the hood. Understanding GPT means understanding the engine that powers the AI revolution. 🚀

---

## 🗺️ Beginner's Roadmap — What We'll Build Today

| Step | What We're Building | Beginner Analogy |
|------|---------------------|------------------|
| 1️⃣ | GPT vs Original Transformer | Why one design won over the other |
| 2️⃣ | Architecture Overview | The full blueprint of the machine |
| 3️⃣ | Token Embeddings | Converting words to numbers the AI understands |
| 4️⃣ | Causal Self-Attention | How the AI "pays attention" to earlier words |
| 5️⃣ | Feed-Forward Network | The AI's "thinking" step |
| 6️⃣ | Transformer Block | Combining attention + thinking into one unit |
| 7️⃣ | Complete GPT Model | Stacking everything into a full model |
| 8️⃣ | Model Configurations | Real-world GPT sizes |
| 9️⃣ | Weight Tying | A clever trick to save memory |
| 🔟 | Testing | Making sure it works! |

---

## 🎯 Goal

Build a complete GPT (Generative Pre-trained Transformer) architecture from scratch, understanding every component: token embeddings, positional encoding, causal self-attention, feed-forward networks, layer normalization, and how they compose into the decoder-only transformer that powers GPT, LLaMA, Mistral, and all modern autoregressive LLMs.

## ⭐ If this is deeply understood:
You can read ANY LLM architecture paper, modify architectures for your needs, and debug training issues at the component level. This is the single most important architecture in modern AI.

---

## 🧠 The Big Picture Formula

The entire GPT in one equation:

$$\text{logits} = \text{LM\_head}\Big(\text{TransformerBlocks}\big(\text{Embed}(\text{tokens}) + \text{PE}\big)\Big)$$

And the training objective — **next token prediction**:

$$P(x_t \mid x_{<t}) = \text{softmax}\Big(W_{\text{vocab}} \cdot h_t\Big)$$

> 🍕 **Pizza Analogy**: Think of it like a pizza factory assembly line. Raw ingredients (tokens) go in → they get prepared (embedding) → they go through multiple cooking stations (transformer blocks) → and out comes a prediction of what topping goes next!

---

# 1️⃣ GPT vs Original Transformer

> 🏠 **Beginner Analogy**: The original Transformer is like a **bilingual translator** — it reads the full French sentence (encoder), then writes the English version (decoder). GPT is like a **storyteller** — it only writes forward, predicting each next word based on what came before. No "reading" step needed!

## Original Transformer (Vaswani 2017): Encoder-Decoder
- Encoder: bidirectional self-attention (sees all tokens)
- Decoder: causal self-attention + cross-attention to encoder
- Used for: translation, seq2seq

## GPT Architecture: Decoder-Only
- ONLY the decoder (no encoder, no cross-attention)
- Causal self-attention (each token sees only previous tokens)
- Used for: language modeling, text generation

### ⭐ Why decoder-only won:
1. **Simpler**: one stack instead of two
2. **Flexible**: any task can be framed as text generation
3. **Scales better**: same architecture, just make it bigger
4. **Emergent abilities**: large decoder-only models show unexpected capabilities

### 📊 Architecture Comparison Table

| Feature | Encoder-Decoder (T5, BART) | Decoder-Only (GPT) | Encoder-Only (BERT) |
|---------|---------------------------|--------------------|--------------------|
| Attention Direction | Bi + Causal | Causal only (← look back) | Bidirectional (← →) |
| Training Task | Seq2Seq | Next token prediction | Masked token prediction |
| Best For | Translation, Summarization | Generation, Chat, Code | Classification, NER |
| Examples | T5, BART, mBART | GPT-2/3/4, LLaMA, Mistral | BERT, RoBERTa |
| Scalability | Moderate | ⭐ Excellent | Limited for generation |

> 📚 **Research Note**: The original Transformer was introduced in *"Attention Is All You Need"* (Vaswani et al., 2017). GPT-1 adapted just the decoder half in *"Improving Language Understanding by Generative Pre-Training"* (Radford et al., 2018), proving that a simpler design could be more powerful when scaled up.

---

# 2️⃣ Architecture Overview

> 🏗️ **Beginner Analogy**: Imagine a **factory assembly line** for writing. A word enters the factory → it gets a name tag (embedding) and a position badge (positional encoding) → then it passes through multiple "thinking stations" (transformer blocks) where it gathers context from all previous words → finally it exits as a prediction of the next word.

> 🔑 **Key Insight**: The entire GPT model is just this pipeline repeated billions of times during training: **Token → Embed → Transform → Predict Next Token**

```
Input tokens: [The, cat, sat, on]
       ↓
[Token Embeddings + Positional Encoding]     ← 📛 Give each word a number + position
       ↓
[Transformer Block 1]                         ← 🧠 First round of "thinking"
  ├── LayerNorm
  ├── Causal Multi-Head Self-Attention        ← 👀 Look at previous words
  ├── Residual Connection                     ← 🔗 Don't forget the original!
  ├── LayerNorm
  ├── Feed-Forward Network (MLP)              ← 💭 Process what you learned
  └── Residual Connection                     ← 🔗 Don't forget again!
       ↓
[Transformer Block 2]
  ├── ... (same structure)                    ← 🧠 Second round of "thinking"
       ↓
  ... (N blocks)                              ← 🧠 N rounds of deeper thinking
       ↓
[Final LayerNorm]                             ← 🧹 Clean up the numbers
       ↓
[Linear (Unembedding) → Vocabulary logits]    ← 📊 Score every possible next word
       ↓
Output: probability distribution over next token  ← 🎯 "sat" → P("on")=0.12, P("down")=0.08, ...
```

> 🔢 **The Math** — for the entire forward pass of a GPT:
>
> $$h_0 = \text{Embed}(x) + \text{PE}$$
> $$h_l = \text{TransformerBlock}_l(h_{l-1}) \quad \text{for } l = 1, \ldots, L$$
> $$\text{logits} = W_{\text{vocab}} \cdot \text{LayerNorm}(h_L)$$
> $$P(x_{t+1} \mid x_{\leq t}) = \text{softmax}(\text{logits}_t)$$

---

# 3️⃣ Component 1: Embeddings

> 🏷️ **Beginner Analogy**: Computers don't understand words — they only understand numbers. **Token embedding** is like giving every word in the dictionary a unique GPS coordinate in a high-dimensional space. Words with similar meanings (like "happy" and "joyful") end up at nearby coordinates!
>
> **Positional encoding** is like adding a "seat number" so the model knows word order. Without it, "dog bites man" and "man bites dog" would look identical!

### ⭐ How Embeddings Work — Step by Step

| Step | What Happens | Example |
|------|-------------|---------|
| 1. Tokenize | Split text into tokens | "The cat sat" → `[The, cat, sat]` |
| 2. Token → ID | Look up each token's number | `[The→464, cat→9246, sat→3332]` |
| 3. ID → Vector | Look up each ID in embedding table | `464 → [0.12, -0.33, 0.87, ...]` (768 numbers!) |
| 4. Add Position | Add positional encoding to each vector | Vector + position info |
| 5. Output | Rich vector with meaning + position | Ready for transformer blocks! |

> 🔢 **The Math**:
>
> $$h_0^{(t)} = W_{\text{embed}}[x_t] + W_{\text{pos}}[t]$$
>
> Where $W_{\text{embed}} \in \mathbb{R}^{V \times d}$ is the token embedding matrix (V = vocab size, d = model dimension), $W_{\text{pos}} \in \mathbb{R}^{T_{\max} \times d}$ is the learned positional embedding, and $x_t$ is the token ID at position $t$.

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
import math

class GPTEmbedding(nn.Module):
    def __init__(self, vocab_size, d_model, max_seq_len, dropout=0.1):
        super().__init__()
        self.token_emb = nn.Embedding(vocab_size, d_model)
        self.pos_emb = nn.Embedding(max_seq_len, d_model)  # Learned positions
        self.dropout = nn.Dropout(dropout)
        self.d_model = d_model
    
    def forward(self, input_ids):
        B, T = input_ids.shape
        
        tok_emb = self.token_emb(input_ids)  # (B, T, d_model)
        positions = torch.arange(T, device=input_ids.device)
        pos_emb = self.pos_emb(positions)    # (T, d_model)
        
        x = tok_emb + pos_emb  # Broadcast: (B, T, d_model)
        return self.dropout(x)
```

> ⭐ **GPT-2 vs Modern Models — Positional Encodings**:
> | Model | Position Method | Max Length |
> |-------|----------------|-----------|
> | GPT-2 | Learned absolute embeddings | 1,024 tokens |
> | GPT-3 | Learned absolute embeddings | 2,048 tokens |
> | LLaMA / LLaMA-2 | RoPE (Rotary Position Embedding) | 4,096+ tokens |
> | Mistral | RoPE + Sliding Window | 32,768 tokens |
> | GPT-4 | Unknown (likely RoPE variant) | 128,000 tokens |
>
> Modern models use **RoPE** (Su et al., 2021) because it enables length extrapolation — the model can handle longer sequences than it was trained on! 🚀

---

# 4️⃣ Component 2: Causal Multi-Head Self-Attention

> 🎭 **Beginner Analogy**: Imagine you're writing a mystery novel. At any point in the story, you can look back at everything you've written before — but you **cannot** peek at future pages. That's **causal** (masked) self-attention!
>
> The **"multi-head"** part? It's like having 12 editors reading your story simultaneously, each focusing on something different: one tracks character names, another tracks emotions, another tracks plot references. Then they combine their notes. 🧠

### ⭐ Self-Attention in Plain English

For each word in the sentence, the model asks three questions:
1. **Query (Q)**: "What am I looking for?" 🔍
2. **Key (K)**: "What do I contain?" 🔑
3. **Value (V)**: "What information should I share?" 💎

Then: **Match queries to keys → get attention weights → collect values**

> 🔢 **The Math — Scaled Dot-Product Attention**:
>
> $$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}} + M\right)V$$
>
> Where:
> - $Q, K, V \in \mathbb{R}^{T \times d_k}$ are the query, key, value matrices
> - $d_k = d_{\text{model}} / h$ is the dimension per head ($h$ = number of heads)
> - $\sqrt{d_k}$ is a scaling factor to prevent dot products from getting too large
> - $M$ is the **causal mask** (0 for allowed positions, $-\infty$ for future positions)
>
> **Multi-head version**:
> $$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \ldots, \text{head}_h) \cdot W^O$$
> $$\text{where head}_i = \text{Attention}(XW_i^Q, XW_i^K, XW_i^V)$$

```python
class CausalSelfAttention(nn.Module):
    def __init__(self, d_model, num_heads, max_seq_len, dropout=0.1):
        super().__init__()
        assert d_model % num_heads == 0
        
        self.num_heads = num_heads
        self.head_dim = d_model // num_heads
        self.d_model = d_model
        
        # Combined QKV projection (more efficient)
        self.qkv_proj = nn.Linear(d_model, 3 * d_model, bias=False)
        self.out_proj = nn.Linear(d_model, d_model, bias=False)
        self.attn_dropout = nn.Dropout(dropout)
        self.resid_dropout = nn.Dropout(dropout)
        
        # Causal mask: lower triangular matrix
        # Registered as buffer (not a parameter, but moves with model to GPU)
        mask = torch.tril(torch.ones(max_seq_len, max_seq_len))
        self.register_buffer("causal_mask", mask.view(1, 1, max_seq_len, max_seq_len))
    
    def forward(self, x):
        B, T, C = x.shape
        
        # Project to Q, K, V
        qkv = self.qkv_proj(x)  # (B, T, 3*d_model)
        q, k, v = qkv.chunk(3, dim=-1)  # Each: (B, T, d_model)
        
        # Reshape for multi-head: (B, T, d_model) → (B, num_heads, T, head_dim)
        q = q.view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        k = k.view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        v = v.view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        
        # Attention scores: (B, num_heads, T, T)
        scores = torch.matmul(q, k.transpose(-2, -1)) / math.sqrt(self.head_dim)
        
        # Apply causal mask
        scores = scores.masked_fill(
            self.causal_mask[:, :, :T, :T] == 0, 
            float('-inf')
        )
        
        # Softmax and dropout
        attn_weights = F.softmax(scores, dim=-1)
        attn_weights = self.attn_dropout(attn_weights)
        
        # Weighted sum of values
        out = torch.matmul(attn_weights, v)  # (B, num_heads, T, head_dim)
        
        # Concatenate heads: (B, T, d_model)
        out = out.transpose(1, 2).contiguous().view(B, T, self.d_model)
        
        return self.resid_dropout(self.out_proj(out))
```

### Why Causal Masking?

During TRAINING, we process the entire sequence at once for efficiency.
But each token should only attend to PREVIOUS tokens (autoregressive).

The mask ensures:
- Token 0 sees only token 0
- Token 1 sees tokens 0, 1
- Token t sees tokens 0, 1, ..., t

```
Mask:
1 0 0 0     ← "The" can only see itself
1 1 0 0     ← "cat" can see "The" and itself
1 1 1 0     ← "sat" can see "The", "cat", and itself
1 1 1 1     ← "on" can see everything before it
```

Positions with 0 get $-\infty$ BEFORE softmax → 0 attention weight → no information leakage.

> 🚫 **Why not let tokens see the future?** Because during generation, the future doesn't exist yet! If the model learned to "cheat" by looking ahead during training, it would fail at actual text generation. The causal mask ensures training matches the generation setting (this is called **teacher forcing**).

> ⭐ **Production Optimization — Flash Attention**: In production, standard attention requires $O(T^2)$ memory. **FlashAttention** (Dao et al., 2022) fuses the softmax computation into a single GPU kernel, reducing memory to $O(T)$ and speeding up training by 2-4×. GPT-4, LLaMA-2, and Mistral all use FlashAttention. 🏎️

---

# 5️⃣ Component 3: Feed-Forward Network (MLP)

> 🧪 **Beginner Analogy**: If attention is like **gathering information from a meeting** (who said what?), the feed-forward network is like **going back to your desk and thinking about it**. It's the "processing" step where the model digests what it learned from attention and transforms it into deeper understanding. 💡

```python
class FeedForward(nn.Module):
    """Position-wise feed-forward network.
    
    Each token is independently transformed through:
    Linear(d_model → 4*d_model) → Activation → Linear(4*d_model → d_model)
    
    This is where most parameters and "computation" happens.
    Attention routes information; FFN processes it.
    """
    def __init__(self, d_model, dropout=0.1):
        super().__init__()
        self.fc1 = nn.Linear(d_model, 4 * d_model)
        self.fc2 = nn.Linear(4 * d_model, d_model)
        self.dropout = nn.Dropout(dropout)
    
    def forward(self, x):
        x = self.fc1(x)
        x = F.gelu(x)  # GPT-2 uses GELU, not ReLU
        x = self.fc2(x)
        return self.dropout(x)
```

### Why 4× expansion?

The FFN expands dimensions by 4×, processes, then compresses back.
Think of it as: project to a higher-dimensional space where non-linear processing is easier, then project back.

> 🎨 **Beginner Analogy**: Imagine you have a blurry photo (768 pixels wide). You blow it up to 4× size (3,072 pixels) where you can see fine details and fix them, then shrink it back to 768. The "expand → process → compress" pattern lets the model work with richer representations!

> 🔢 **The Math**:
>
> $$\text{FFN}(x) = W_2 \cdot \text{GELU}(W_1 x + b_1) + b_2$$
>
> Where $W_1 \in \mathbb{R}^{4d \times d}$, $W_2 \in \mathbb{R}^{d \times 4d}$, and the 4× expansion creates $8d^2$ parameters per block (2/3 of all block parameters!).

### GELU vs ReLU

GELU(x) = x · Φ(x) where Φ is the standard Gaussian CDF.
Unlike ReLU (hard cutoff at 0), GELU smoothly gates values.
Better for language modeling — no "dead neurons."

> 🔢 **GELU in LaTeX**:
>
> $$\text{GELU}(x) = x \cdot \Phi(x) = x \cdot \frac{1}{2}\left[1 + \text{erf}\left(\frac{x}{\sqrt{2}}\right)\right]$$
>
> $$\text{ReLU}(x) = \max(0, x)$$

> ⭐ **Modern FFN Variants** — Production models have moved beyond vanilla FFN:
>
> | Model | FFN Type | Key Difference |
> |-------|----------|---------------|
> | GPT-2 | Standard FFN + GELU | Original design |
> | LLaMA | SwiGLU (Gated Linear Unit) | Uses gating: $\text{SwiGLU}(x) = (\text{Swish}(xW_1)) \odot (xV)$ |
> | Mistral | SwiGLU | Same as LLaMA, ~30% fewer FLOPs for same quality |
> | PaLM | SwiGLU | Google's choice too |
>
> SwiGLU (Shazeer, 2020) is now the default — it outperforms GELU at the same parameter count! 🏆

---

# 6️⃣ Component 4: Transformer Block

> 🧱 **Beginner Analogy**: A single transformer block is like **one round of group discussion**. First, everyone shares notes with each other (attention). Then, each person goes away and thinks independently (FFN). After each step, they keep their original thoughts too (residual connection) — so nothing important gets lost. Stack 12-96 of these blocks, and you get increasingly deep understanding! 🏗️

```python
class TransformerBlock(nn.Module):
    """Single transformer block with Pre-LayerNorm (modern style).
    
    GPT-2 and modern LLMs use Pre-LN (normalize before attention/FFN):
    x → LN → Attention → + → LN → FFN → +
    
    Original transformer used Post-LN (normalize after):
    x → Attention → + → LN → FFN → + → LN
    
    Pre-LN is more stable for deep networks.
    """
    def __init__(self, d_model, num_heads, max_seq_len, dropout=0.1):
        super().__init__()
        self.ln1 = nn.LayerNorm(d_model)
        self.attn = CausalSelfAttention(d_model, num_heads, max_seq_len, dropout)
        self.ln2 = nn.LayerNorm(d_model)
        self.ffn = FeedForward(d_model, dropout)
    
    def forward(self, x):
        # Pre-LN: normalize BEFORE the sub-layer
        x = x + self.attn(self.ln1(x))  # Residual + Attention
        x = x + self.ffn(self.ln2(x))   # Residual + FFN
        return x
```

### The Residual Connection is Crucial

Without residuals: x → f(x)
With residuals: x → x + f(x)

This means each block only needs to learn the DELTA — what to ADD to the representation.
Gradients flow directly through the skip connection: ∂L/∂x = ∂L/∂(x+f(x)) · (1 + ∂f/∂x).
The "1+" prevents vanishing gradients.

> 🔢 **Residual Connection Math**:
>
> $$x_{\text{out}} = x + \text{SubLayer}(\text{LayerNorm}(x))$$
>
> **Why this saves training**: The gradient through a residual:
>
> $$\frac{\partial \mathcal{L}}{\partial x} = \frac{\partial \mathcal{L}}{\partial x_{\text{out}}} \cdot \left(1 + \frac{\partial f(x)}{\partial x}\right)$$
>
> That "$1 +$" term means gradients can always flow — even if $\frac{\partial f}{\partial x} \approx 0$! Without it, stacking 96 layers would be impossible.

> 🏗️ **Beginner Analogy**: Residual connections are like a **highway bypass**. Imagine driving through 96 small towns (layers). Without a highway, you'd get stuck in traffic and never make it through. The residual "highway" lets information bypass each town while still stopping at the ones that are useful.

> ⭐ **LayerNorm Explained for Beginners**:
> 
> LayerNorm normalizes each token's vector to have mean=0 and variance=1. Think of it as "resetting the volume" after each step so numbers don't explode or vanish.
>
> $$\text{LayerNorm}(x) = \gamma \cdot \frac{x - \mu}{\sqrt{\sigma^2 + \epsilon}} + \beta$$
>
> Where $\mu, \sigma^2$ are the mean and variance across the $d$ dimensions, and $\gamma, \beta$ are learnable scale/shift parameters.

> 📚 **Pre-LN vs Post-LN**: The original Transformer (Vaswani, 2017) used **Post-LN** (normalize after attention). GPT-2 and all modern LLMs switched to **Pre-LN** (normalize before attention) because it makes training far more stable — you can train deeper models without learning rate warmup hacks (Xiong et al., 2020, *"On Layer Normalization in the Transformer Architecture"*).

---

# 7️⃣ The Complete GPT Model

> 🏭 **Beginner Analogy**: Now we assemble the whole factory! Think of building a GPT like building a skyscraper:
> - **Foundation** = Embeddings (convert words to numbers)
> - **Floors 1-N** = Transformer blocks (each floor refines understanding)
> - **Rooftop antenna** = LM Head (broadcasts the prediction)
>
> The more floors (layers), the "smarter" the building. GPT-2 has 12 floors. GPT-3 has 96. 🏢

```python
class GPT(nn.Module):
    def __init__(self, vocab_size, d_model, num_heads, num_layers, max_seq_len, dropout=0.1):
        super().__init__()
        
        self.embedding = GPTEmbedding(vocab_size, d_model, max_seq_len, dropout)
        
        # Stack of transformer blocks
        self.blocks = nn.ModuleList([
            TransformerBlock(d_model, num_heads, max_seq_len, dropout)
            for _ in range(num_layers)
        ])
        
        self.final_ln = nn.LayerNorm(d_model)
        
        # Language model head: project back to vocabulary
        self.lm_head = nn.Linear(d_model, vocab_size, bias=False)
        
        # Weight tying: share embedding weights with output projection
        # This is standard practice — saves parameters and improves quality
        self.lm_head.weight = self.embedding.token_emb.weight
        
        # Initialize weights
        self.apply(self._init_weights)
        
        # Report parameter count
        n_params = sum(p.numel() for p in self.parameters())
        print(f"GPT Model: {n_params/1e6:.1f}M parameters")
    
    def _init_weights(self, module):
        if isinstance(module, nn.Linear):
            torch.nn.init.normal_(module.weight, mean=0.0, std=0.02)
            if module.bias is not None:
                torch.nn.init.zeros_(module.bias)
        elif isinstance(module, nn.Embedding):
            torch.nn.init.normal_(module.weight, mean=0.0, std=0.02)
    
    def forward(self, input_ids, targets=None):
        """
        input_ids: (B, T) token indices
        targets: (B, T) target token indices for training
        """
        # Embed tokens
        x = self.embedding(input_ids)  # (B, T, d_model)
        
        # Pass through transformer blocks
        for block in self.blocks:
            x = block(x)
        
        # Final layer norm
        x = self.final_ln(x)
        
        # Project to vocabulary
        logits = self.lm_head(x)  # (B, T, vocab_size)
        
        # Compute loss if targets provided
        loss = None
        if targets is not None:
            loss = F.cross_entropy(
                logits.view(-1, logits.size(-1)),  # (B*T, vocab_size)
                targets.view(-1),                   # (B*T,)
                ignore_index=-100
            )
        
        return logits, loss
    
    @torch.no_grad()
    def generate(self, input_ids, max_new_tokens, temperature=1.0, top_k=50):
        """Autoregressive generation."""
        for _ in range(max_new_tokens):
            # Crop to max sequence length
            idx_cond = input_ids[:, -self.embedding.pos_emb.num_embeddings:]
            
            # Forward pass
            logits, _ = self(idx_cond)
            
            # Take logits at the last position
            logits = logits[:, -1, :] / temperature
            
            # Top-k filtering
            if top_k is not None:
                v, _ = torch.topk(logits, min(top_k, logits.size(-1)))
                logits[logits < v[:, [-1]]] = float('-inf')
            
            # Sample
            probs = F.softmax(logits, dim=-1)
            next_token = torch.multinomial(probs, num_samples=1)
            
            # Append
            input_ids = torch.cat([input_ids, next_token], dim=-1)
        
        return input_ids
```

> 🔢 **The Complete Forward Pass — In Math**:
>
> $$h_0 = W_e[x] + W_p[\text{pos}]$$
> $$h_l = h_{l-1} + \text{Attn}(\text{LN}(h_{l-1})) \quad \text{(attention + residual)}$$
> $$h_l = h_l + \text{FFN}(\text{LN}(h_l)) \quad \text{(FFN + residual)}$$
> $$\text{logits} = W_e^T \cdot \text{LN}(h_L) \quad \text{(weight-tied output)}$$
>
> **Training loss** — cross-entropy over next-token prediction:
>
> $$\mathcal{L} = -\frac{1}{T}\sum_{t=1}^{T} \log P(x_t \mid x_{<t}) = -\frac{1}{T}\sum_{t=1}^{T} \log \text{softmax}(\text{logits}_t)[x_t]$$

> ⭐ **How Generation Works — Step by Step for Beginners**:
>
> | Step | Input | Model Does | Output |
> |------|-------|-----------|--------|
> | 1 | "The" | Forward pass → scores all 50,257 possible next words | P("cat")=0.003, P("quick")=0.002, ... |
> | 2 | "The cat" | Forward pass again with 2 tokens | P("sat")=0.04, P("ran")=0.02, ... |
> | 3 | "The cat sat" | Forward pass with 3 tokens | P("on")=0.12, P("down")=0.06, ... |
> | ... | Keep going! | Each step adds one token | Full sentence emerges! |
>
> This is **autoregressive generation** — one token at a time, each conditioned on all previous tokens. It's like how you write: each word depends on what you've already written. ✍️

> 🌡️ **Temperature & Top-k — Controlling Creativity**:
> - **Temperature = 0.1**: Very confident, repetitive ("The cat sat on the mat. The cat sat on the mat.")
> - **Temperature = 1.0**: Balanced, natural text
> - **Temperature = 2.0**: Wild, creative, sometimes nonsensical
> - **Top-k = 50**: Only consider the top 50 most likely next tokens (ignore unlikely noise)

---

# 8️⃣ Model Configurations

> 🎚️ **Beginner Analogy**: Think of these configs like car models. They're all "cars" (GPT architecture), but the engine size varies. A 117M model is like a compact car — fast, cheap, fits anywhere. A 1.5B model is a truck — powerful but needs more gas (compute). A 175B model? That's a rocket ship. 🚀

```python
# GPT-2 Small (117M)
config_small = dict(vocab_size=50257, d_model=768, num_heads=12, num_layers=12, max_seq_len=1024)

# GPT-2 Medium (345M)
config_medium = dict(vocab_size=50257, d_model=1024, num_heads=16, num_layers=24, max_seq_len=1024)

# GPT-2 Large (774M)
config_large = dict(vocab_size=50257, d_model=1280, num_heads=20, num_layers=36, max_seq_len=1024)

# GPT-2 XL (1.5B)
config_xl = dict(vocab_size=50257, d_model=1600, num_heads=25, num_layers=48, max_seq_len=1024)

# Create model
model = GPT(**config_small)
```

## Parameter Counting

For a transformer with L layers, d model dimensions, V vocab size:
- Embedding: V × d
- Per block attention: 4 × d² (Q, K, V, O projections)
- Per block FFN: 2 × d × 4d = 8d²
- Per block total: ~12d²
- Total: V × d + L × 12d² + d (final LN)

GPT-2 Small: 50257 × 768 + 12 × 12 × 768² ≈ 117M ✓

> 🔢 **Parameter Count Formula in LaTeX**:
>
> $$N_{\text{params}} \approx V \cdot d + L \cdot 12d^2$$
>
> Where:
> - $V$ = vocabulary size (50,257 for GPT-2)
> - $d$ = model dimension ($d_{\text{model}}$)
> - $L$ = number of transformer layers
> - The $12d^2$ comes from: $4d^2$ (attention QKV+O) + $8d^2$ (FFN two matrices)
>
> **Quick estimation**: For GPT-2 Small: $50257 \times 768 + 12 \times 12 \times 768^2 \approx 38.6M + 81.3M \approx 120M$ ✓

> ⭐ **Production GPT Models — The Full Landscape**:
>
> | Model | Parameters | Layers | $d_{\text{model}}$ | Heads | Context | Year | Organization |
> |-------|-----------|--------|-----|-------|---------|------|-------------|
> | GPT-1 | 117M | 12 | 768 | 12 | 512 | 2018 | OpenAI |
> | GPT-2 Small | 117M | 12 | 768 | 12 | 1,024 | 2019 | OpenAI |
> | GPT-2 XL | 1.5B | 48 | 1,600 | 25 | 1,024 | 2019 | OpenAI |
> | GPT-3 | 175B | 96 | 12,288 | 96 | 2,048 | 2020 | OpenAI |
> | GPT-4 | ~1.8T (rumored MoE) | ? | ? | ? | 128K | 2023 | OpenAI |
> | LLaMA-2 7B | 7B | 32 | 4,096 | 32 | 4,096 | 2023 | Meta |
> | LLaMA-2 70B | 70B | 80 | 8,192 | 64 | 4,096 | 2023 | Meta |
> | Mistral 7B | 7.3B | 32 | 4,096 | 32 | 32K | 2023 | Mistral AI |
> | Falcon 40B | 40B | 60 | 8,192 | 64 | 2,048 | 2023 | TII |
>
> 📚 **Scaling Laws** (Kaplan et al., 2020, *"Scaling Laws for Neural Language Models"*): Performance improves predictably as a **power law** with model size, data size, and compute:
>
> $$L(N) \propto N^{-0.076}$$
>
> This means: 10× more parameters → loss decreases by ~40%. This finding drove the race from GPT-2 (1.5B) to GPT-3 (175B) to GPT-4 (~1T+). 📈

> 🏭 **The ChatGPT Pipeline — From GPT to Chatbot**:
>
> | Stage | What Happens | Method |
> |-------|-------------|--------|
> | 1. Pre-training | Train GPT on internet text (next-token prediction) | Self-supervised, ~trillions of tokens |
> | 2. SFT | Fine-tune on instruction-following examples | Supervised Fine-Tuning on human demos |
> | 3. RLHF | Train a reward model + PPO optimization | Reinforcement Learning from Human Feedback |
> | 4. Deployment | Add safety filters, rate limiting, API | Engineering + Red teaming |
>
> The base GPT is a **text completer**. RLHF turns it into a **helpful assistant**. This pipeline was first described in Ouyang et al. (2022), *"Training language models to follow instructions with human feedback"* (InstructGPT paper).

---

# 9️⃣ Weight Tying: Why Share Embeddings?

> 💡 **Beginner Analogy**: Imagine you have a phone book where you look up names → phone numbers (embedding: word → vector). Weight tying says: "The reverse lookup — phone numbers → names (vector → word) — should use the **same** phone book, just read backwards!" This saves memory and makes both lookups more consistent.

```python
self.lm_head.weight = self.embedding.token_emb.weight
```

The token embedding maps: token_id → d_model vector
The output head maps: d_model vector → token_id logits

These are inverse operations! Tying them:
1. Saves ~V × d parameters (~38M for GPT-2!)
2. Ensures consistent representations
3. Provides regularization
4. Empirically improves perplexity

> 🔢 **The Math**:
>
> - **Embedding**: $h = W_e[x_t]$ where $W_e \in \mathbb{R}^{V \times d}$
> - **Output projection**: $\text{logits} = h \cdot W_e^T$ (same matrix, transposed!)
> - **Savings**: For GPT-2, $50257 \times 768 = 38.6M$ parameters saved — that's 33% of the model! 💰

> 📚 **Research**: Weight tying was proposed in Press & Wolf (2017), *"Using the Output Embedding to Improve Language Models"*. It's now universal in all LLMs.

---

# 🔟 Testing the Model

> 🧪 **What's happening here**: We create a tiny GPT (4 layers, 256 dimensions — way smaller than real GPT-2) and feed it random numbers to make sure the shapes work. The initial loss of ~10.82 makes sense because $-\ln(1/50257) \approx 10.82$ — the model starts by guessing randomly among 50,257 tokens!

```python
# Quick test
model = GPT(vocab_size=50257, d_model=256, num_heads=4, num_layers=4, max_seq_len=128)

# Create dummy input
input_ids = torch.randint(0, 50257, (2, 32))  # batch=2, seq_len=32
targets = torch.randint(0, 50257, (2, 32))

# Forward pass
logits, loss = model(input_ids, targets)
print(f"Logits shape: {logits.shape}")  # (2, 32, 50257)
print(f"Loss: {loss.item():.4f}")        # ~10.82 (= -ln(1/50257))

# Generation
start_tokens = torch.tensor([[1, 2, 3]])  # Some starting tokens
generated = model.generate(start_tokens, max_new_tokens=20)
print(f"Generated: {generated}")
```

> ⭐ **Expected Output Explanation**:
> | Output | Value | Why |
> |--------|-------|-----|
> | Logits shape | `(2, 32, 50257)` | 2 sequences × 32 positions × 50,257 vocab scores |
> | Loss | ~10.82 | Random guessing: $-\ln(1/V) = -\ln(1/50257) \approx 10.82$ |
> | Generated | Random tokens | Untrained model = random babbling! 🎲 |
>
> After training on real text, the loss drops to ~3.0 (GPT-2) or ~1.7 (GPT-3), and generation produces coherent text!

---

# 📝 Summary

| Component | Purpose | Key Detail |
|-----------|---------|------------|
| Token Embedding | Token → vector | Weight-tied with output head |
| Position Encoding | Inject order | Learned (GPT-2) or RoPE (modern) |
| Causal Mask | Prevent future leakage | Lower triangular, -inf fill |
| Multi-Head Attention | Route information | Split into h heads, each d/h dims |
| Feed-Forward | Process information | 4× expansion, GELU activation |
| LayerNorm | Stabilize training | Pre-LN (modern) vs Post-LN (original) |
| Residual Connection | Enable depth | x + f(x) at every sub-layer |
| LM Head | Predict next token | Linear to vocab size, weight-tied |

---

# 🏆 GPT Architecture Cheat Sheet

> **Print this. Pin it. Refer to it whenever you read an LLM paper.**

## ⭐ The One Formula That Rules Them All

$$\boxed{P(x_t \mid x_{<t}) = \text{softmax}\!\Big(W_e^T \cdot \text{LN}\big(\text{TransformerBlocks}(W_e[x] + W_p[\text{pos}])\big)\Big)}$$

## ⭐ Parameter Count Rule of Thumb

$$\boxed{N \approx 12 \cdot d^2 \cdot L}$$

(Ignoring embedding — this dominates for large models)

## ⭐ Key Numbers to Remember

| Fact | Value |
|------|-------|
| GPT-2 vocab size | 50,257 (BPE tokens) |
| FFN expansion ratio | 4× (standard) or 8/3× (SwiGLU) |
| Attention complexity | $O(T^2 \cdot d)$ per layer |
| Residual stream dimension | Constant $d$ throughout all layers |
| Initial loss (random) | $-\ln(1/V) \approx 10.82$ |
| Good loss (trained) | ~2.5-3.5 (perplexity 12-33) |

## ⭐ Beginner Analogy Cheat Sheet

| Concept | Analogy |
|---------|---------|
| GPT | 🔮 The world's best autocomplete — it read the whole internet |
| Decoder-only | ✍️ A writer who can only look backward, never peek ahead |
| Token → Embed → Transform → Predict | 📖 Look up a word → Think about it → Guess the next word |
| Stacking N blocks | 🧠 N rounds of deeper thinking — more layers = deeper reasoning |
| Training | 📚 Learning to predict the next word from billions of internet sentences |
| Causal mask | 🚧 A one-way mirror — you can see the past but not the future |
| Residual connections | 🛣️ Highway bypasses so information doesn't get stuck |
| LayerNorm | 🔊 Volume control — keeps numbers from exploding or vanishing |
| Weight tying | 📞 Using the same phone book for lookup and reverse-lookup |
| Temperature | 🌡️ Creativity dial — low = safe & repetitive, high = wild & creative |

## ⭐ GPT Family Tree

```
GPT-1 (2018) — 117M — "Pre-training works!"
    ↓
GPT-2 (2019) — 1.5B — "Scale improves everything!"
    ↓
GPT-3 (2020) — 175B — "In-context learning emerges!"
    ↓
InstructGPT (2022) — RLHF — "Now it follows instructions!"
    ↓
ChatGPT (Nov 2022) — GPT-3.5 + RLHF + Chat format — "The world changes!"
    ↓
GPT-4 (Mar 2023) — ~1T+ MoE (rumored) — "Multimodal, state of the art"
```

## ⭐ Key Research Papers

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|-----------------|
| *Attention Is All You Need* | Vaswani et al. | 2017 | Invented the Transformer |
| *Improving Language Understanding by Generative Pre-Training* | Radford et al. | 2018 | GPT-1: pre-train + fine-tune paradigm |
| *Language Models are Unsupervised Multitask Learners* | Radford et al. | 2019 | GPT-2: scale up, zero-shot capabilities |
| *Language Models are Few-Shot Learners* | Brown et al. | 2020 | GPT-3: in-context learning, 175B params |
| *Scaling Laws for Neural Language Models* | Kaplan et al. | 2020 | Power-law relationship between scale and performance |
| *Training Compute-Optimal Large Language Models* | Hoffmann et al. | 2022 | Chinchilla scaling laws — data matters as much as params |
| *LLaMA: Open and Efficient Foundation Language Models* | Touvron et al. | 2023 | Open-source GPT-class models |
| *Mistral 7B* | Jiang et al. | 2023 | Sliding window attention + grouped-query attention |

---

# 🔬 Deep Dive: Why GPT Works So Well

> For the curious reader who wants to go deeper.

### 1. The Bitter Lesson (Rich Sutton, 2019)
GPT is the ultimate proof of Sutton's Bitter Lesson: **general methods that leverage computation are ultimately the most effective.** GPT doesn't have specialized linguistic rules — it's just next-token prediction + scale.

### 2. Emergence Through Scale
GPT-3 showed **emergent abilities** — capabilities that appear suddenly at scale:
- **Few-shot learning**: Learns new tasks from just a few examples in the prompt
- **Chain-of-thought reasoning**: Can solve multi-step problems when prompted with "Let's think step by step"
- **Code generation**: Writes working code despite not being explicitly trained for it

These abilities were not present in GPT-2 (1.5B) but emerged in GPT-3 (175B), supporting the scaling hypothesis (Wei et al., 2022).

### 3. The Compute Cost Reality

| Model | Training Compute | Estimated Cost | Training Data |
|-------|-----------------|---------------|--------------|
| GPT-2 | ~$50K | ~256 GPU-days | 40GB (WebText) |
| GPT-3 | ~$4.6M | ~3,640 petaflop-days | 570GB (filtered CommonCrawl) |
| GPT-4 | ~$100M+ (est.) | Unknown | Unknown (likely >10TB) |
| LLaMA-2 70B | ~$2M | 1,720,320 GPU-hours | 2T tokens |

---

**Tomorrow**: Causal (Masked) Self-Attention deep dive — understanding the autoregressive property and its implications.
