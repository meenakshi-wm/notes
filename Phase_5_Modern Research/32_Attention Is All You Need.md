📘 DAY 32 — Paper Reproduction: Attention Is All You Need

Goal:

Reproduce the core results of the original Transformer paper (Vaswani et al., 2017). Implement the full architecture from scratch with careful attention to the details that matter.

---

# 🌟 Why This Paper Matters (Beginner Start Here!)

> ⭐ **This is THE most important paper in modern AI.** Every system you've heard of — ChatGPT, Google Translate, BERT, LLaMA, Claude — is built on the Transformer architecture introduced here. With **90,000+ citations**, it fundamentally changed how machines understand language.

🎯 **One-sentence summary**: Instead of reading words one-by-one (like old models), the Transformer looks at ALL words at once and figures out which ones are related — using a mechanism called **attention**.

### 🏠 Beginner Analogy: The Meeting Room

Imagine you're in a **meeting room** 🏢 with 10 people. Old AI models (RNNs) were like passing a note down a line — by the time it reached the end, the message was garbled. The Transformer is like giving **everyone a microphone** 🎤 — every person can listen to every other person directly and decide who is most relevant to what they're saying.

| Old Way (RNNs) | New Way (Transformers) |
|---|---|
| 🐌 Process words one at a time | ⚡ Process ALL words simultaneously |
| 😵 Forget long-ago words | 🧠 Direct access to every word |
| 🚫 Hard to parallelize (slow) | ✅ Fully parallelizable (fast on GPUs) |

### 🏭 Production Impact

| System | Uses Transformers? | Notes |
|---|---|---|
| GPT-4 / ChatGPT | ✅ Decoder-only Transformer | OpenAI |
| BERT | ✅ Encoder-only Transformer | Google |
| Google Translate | ✅ Encoder-Decoder Transformer | Original use case |
| LLaMA | ✅ Decoder-only Transformer | Meta |
| Stable Diffusion | ✅ Transformers in diffusion | Image generation |

📖 **Citation**: Vaswani, A., Shazeer, N., Parmar, N., et al. (2017). *"Attention Is All You Need"*. NeurIPS 2017. arXiv:1706.03762

---

# 1️⃣ Paper Summary

| Aspect | Detail |
|--------|--------|
| Problem | Sequence-to-sequence modeling (machine translation) |
| Key insight | Replace RNN recurrence entirely with attention |
| Architecture | Encoder-decoder with multi-head self-attention |
| Result | SOTA on WMT translation, faster to train than RNNs |

### 🔑 The Core Idea in Plain English

> 🧩 Before this paper, the best language models used **Recurrent Neural Networks (RNNs)** — they read a sentence word-by-word, left to right, like reading aloud. This was slow and forgetful. Vaswani et al. asked: *what if we throw away recurrence entirely and use ONLY attention?* It worked spectacularly.

📖 **Historical context**: The attention mechanism itself was introduced by Bahdanau, Cho & Bengio (2014) in *"Neural Machine Translation by Jointly Learning to Align and Translate"* (arXiv:1409.0473). That paper added attention ON TOP of RNNs. This paper removed the RNNs entirely.

---

# 2️⃣ Architecture Implementation

### 📐 Core Math: Scaled Dot-Product Attention

⭐ **The single most important equation in modern AI:**

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

| Symbol | Name | 🏠 Analogy |
|---|---|---|
| $Q$ (Query) | What am I looking for? | 📋 Your **question** at a library desk |
| $K$ (Key) | What does each item advertise? | 🏷️ The **label** on each book |
| $V$ (Value) | What's the actual content? | 📖 The **answer** inside the book |
| $d_k$ | Dimension of keys | Controls scale so numbers don't explode |
| $\sqrt{d_k}$ | Scaling factor | Without this, softmax saturates → tiny gradients 😵 |

> 🏠 **Library Analogy**: You walk into a library with a **question** ($Q$). You compare your question against every book's **label** ($K$) to find relevance scores ($QK^T$). Then you read the **content** ($V$) of the most relevant books, weighted by how relevant each one was.

### 📐 Multi-Head Attention

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \ldots, \text{head}_h)W^O$$

where each $\text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V)$

> 🏠 **Expert Panel Analogy**: Instead of one person examining a sentence, imagine **8 experts** 👩‍🔬👨‍🏫👩‍⚕️👨‍🍳 each looking at the same sentence but focusing on **different aspects** — one notices grammar, another notices meaning, another notices tone. Their insights are then combined. That's multi-head attention with $h=8$ heads!

### 📐 Feed-Forward Network (FFN)

$$\text{FFN}(x) = \max(0,\; xW_1 + b_1)W_2 + b_2$$

> 🏠 The FFN is like a **personal notebook** 📓 — after the attention layer decides what's relevant, the FFN processes and transforms that information independently for each position.

### 📐 Residual Connections & Layer Normalization

$$\text{output} = \text{LayerNorm}(x + \text{Sublayer}(x))$$

> 🏠 **Residual Connection Analogy**: Imagine you're in a lecture 🎓. You **take notes** (sublayer output) AND **keep listening** (original input $x$). The final understanding is both combined. This prevents the "vanishing gradient" problem — information can always flow through the shortcut.

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
import math

class MultiHeadAttention(nn.Module):
    def __init__(self, d_model=512, n_heads=8, dropout=0.1):
        super().__init__()
        assert d_model % n_heads == 0
        self.d_k = d_model // n_heads
        self.n_heads = n_heads
        
        self.W_q = nn.Linear(d_model, d_model)
        self.W_k = nn.Linear(d_model, d_model)
        self.W_v = nn.Linear(d_model, d_model)
        self.W_o = nn.Linear(d_model, d_model)
        self.dropout = nn.Dropout(dropout)
    
    def forward(self, query, key, value, mask=None):
        B = query.size(0)
        
        Q = self.W_q(query).view(B, -1, self.n_heads, self.d_k).transpose(1, 2)
        K = self.W_k(key).view(B, -1, self.n_heads, self.d_k).transpose(1, 2)
        V = self.W_v(value).view(B, -1, self.n_heads, self.d_k).transpose(1, 2)
        
        scores = Q @ K.transpose(-2, -1) / math.sqrt(self.d_k)
        if mask is not None:
            scores = scores.masked_fill(mask == 0, -1e9)
        
        attn = self.dropout(torch.softmax(scores, dim=-1))
        context = (attn @ V).transpose(1, 2).contiguous().view(B, -1, self.n_heads * self.d_k)
        return self.W_o(context)

class FeedForward(nn.Module):
    def __init__(self, d_model=512, d_ff=2048, dropout=0.1):
        super().__init__()
        self.linear1 = nn.Linear(d_model, d_ff)
        self.linear2 = nn.Linear(d_ff, d_model)
        self.dropout = nn.Dropout(dropout)
    
    def forward(self, x):
        return self.linear2(self.dropout(F.relu(self.linear1(x))))

class EncoderLayer(nn.Module):
    def __init__(self, d_model=512, n_heads=8, d_ff=2048, dropout=0.1):
        super().__init__()
        self.self_attn = MultiHeadAttention(d_model, n_heads, dropout)
        self.ffn = FeedForward(d_model, d_ff, dropout)
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.dropout1 = nn.Dropout(dropout)
        self.dropout2 = nn.Dropout(dropout)
    
    def forward(self, x, src_mask=None):
        # Pre-LN would put norm first (modern practice), but paper uses Post-LN
        attn_out = self.self_attn(x, x, x, src_mask)
        x = self.norm1(x + self.dropout1(attn_out))
        ff_out = self.ffn(x)
        x = self.norm2(x + self.dropout2(ff_out))
        return x

class DecoderLayer(nn.Module):
    def __init__(self, d_model=512, n_heads=8, d_ff=2048, dropout=0.1):
        super().__init__()
        self.self_attn = MultiHeadAttention(d_model, n_heads, dropout)
        self.cross_attn = MultiHeadAttention(d_model, n_heads, dropout)
        self.ffn = FeedForward(d_model, d_ff, dropout)
        self.norm1 = nn.LayerNorm(d_model)
        self.norm2 = nn.LayerNorm(d_model)
        self.norm3 = nn.LayerNorm(d_model)
        self.dropout1 = nn.Dropout(dropout)
        self.dropout2 = nn.Dropout(dropout)
        self.dropout3 = nn.Dropout(dropout)
    
    def forward(self, x, enc_output, src_mask=None, tgt_mask=None):
        attn_out = self.self_attn(x, x, x, tgt_mask)
        x = self.norm1(x + self.dropout1(attn_out))
        cross_out = self.cross_attn(x, enc_output, enc_output, src_mask)
        x = self.norm2(x + self.dropout2(cross_out))
        ff_out = self.ffn(x)
        x = self.norm3(x + self.dropout3(ff_out))
        return x
```

---

# 3️⃣ Positional Encoding

### 📐 Positional Encoding Math

$$PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right)$$
$$PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d_{\text{model}}}}\right)$$

where $pos$ = position in the sequence, $i$ = dimension index, $d_{\text{model}}$ = 512.

> 🏠 **Theater Seat Analogy** 🎭: Attention treats all words equally — it doesn't know word order! Positional encoding is like giving each word a **seat number** in a theater. "The cat sat on the mat" needs to be different from "The mat sat on the cat". The sin/cos pattern creates unique "addresses" that also encode relative distances.

> ⭐ **Why sin/cos?** The authors hypothesized the model could learn to attend to relative positions, since $PE_{pos+k}$ can be represented as a linear function of $PE_{pos}$.

```python
class SinusoidalPositionalEncoding(nn.Module):
    def __init__(self, d_model=512, max_len=5000, dropout=0.1):
        super().__init__()
        self.dropout = nn.Dropout(dropout)
        
        pe = torch.zeros(max_len, d_model)
        position = torch.arange(0, max_len).unsqueeze(1).float()
        div_term = torch.exp(torch.arange(0, d_model, 2).float() * (-math.log(10000.0) / d_model))
        
        pe[:, 0::2] = torch.sin(position * div_term)
        pe[:, 1::2] = torch.cos(position * div_term)
        pe = pe.unsqueeze(0)
        self.register_buffer('pe', pe)
    
    def forward(self, x):
        x = x + self.pe[:, :x.size(1)]
        return self.dropout(x)
```

---

# 4️⃣ Full Transformer

> 🏠 **Factory Analogy** 🏭: The full Transformer is like a **translation factory**. Raw materials (source words) enter the **Encoder** (understanding department — 6 floors). The understood meaning is sent to the **Decoder** (production department — 6 floors) which generates the output one word at a time. Each floor has an attention meeting room + a processing office (FFN).

| Component | Count | Purpose |
|---|---|---|
| Encoder layers | $N=6$ | Understand input |
| Decoder layers | $N=6$ | Generate output |
| Attention heads | $h=8$ | Multiple perspectives |
| Model dimension | $d_{\text{model}}=512$ | Size of representations |
| FFN inner dimension | $d_{ff}=2048$ | Processing capacity |
| Parameters | ~65M | Tiny by today's standards! |

```python
class Transformer(nn.Module):
    def __init__(self, src_vocab, tgt_vocab, d_model=512, n_heads=8,
                 n_enc_layers=6, n_dec_layers=6, d_ff=2048, dropout=0.1, max_len=5000):
        super().__init__()
        self.d_model = d_model
        
        self.src_embed = nn.Embedding(src_vocab, d_model)
        self.tgt_embed = nn.Embedding(tgt_vocab, d_model)
        self.pos_enc = SinusoidalPositionalEncoding(d_model, max_len, dropout)
        
        self.encoder = nn.ModuleList([EncoderLayer(d_model, n_heads, d_ff, dropout) for _ in range(n_enc_layers)])
        self.decoder = nn.ModuleList([DecoderLayer(d_model, n_heads, d_ff, dropout) for _ in range(n_dec_layers)])
        
        self.output_proj = nn.Linear(d_model, tgt_vocab)
        self._init_weights()
    
    def _init_weights(self):
        for p in self.parameters():
            if p.dim() > 1:
                nn.init.xavier_uniform_(p)
    
    def encode(self, src, src_mask=None):
        x = self.pos_enc(self.src_embed(src) * math.sqrt(self.d_model))
        for layer in self.encoder:
            x = layer(x, src_mask)
        return x
    
    def decode(self, tgt, enc_output, src_mask=None, tgt_mask=None):
        x = self.pos_enc(self.tgt_embed(tgt) * math.sqrt(self.d_model))
        for layer in self.decoder:
            x = layer(x, enc_output, src_mask, tgt_mask)
        return x
    
    def forward(self, src, tgt, src_mask=None, tgt_mask=None):
        enc_output = self.encode(src, src_mask)
        dec_output = self.decode(tgt, enc_output, src_mask, tgt_mask)
        return self.output_proj(dec_output)

def causal_mask(size):
    mask = torch.triu(torch.ones(size, size), diagonal=1).bool()
    return ~mask
```

---

# 5️⃣ Training with Paper's Schedule

### 📐 Learning Rate Schedule

$$lr = d_{\text{model}}^{-0.5} \cdot \min(\text{step}^{-0.5},\; \text{step} \cdot \text{warmup}^{-1.5})$$

> 🏠 **Driving Analogy** 🚗: When you start driving, you accelerate slowly (warmup) to avoid crashing. Once at cruising speed, you gradually slow down. The warmup of 4000 steps prevents early training instability.

```python
class TransformerLRScheduler:
    """The warmup + inverse sqrt schedule from the paper."""
    def __init__(self, optimizer, d_model=512, warmup_steps=4000):
        self.optimizer = optimizer
        self.d_model = d_model
        self.warmup_steps = warmup_steps
        self.step_num = 0
    
    def step(self):
        self.step_num += 1
        lr = self.d_model ** (-0.5) * min(
            self.step_num ** (-0.5),
            self.step_num * self.warmup_steps ** (-1.5)
        )
        for param_group in self.optimizer.param_groups:
            param_group['lr'] = lr
        return lr

# Training
model = Transformer(src_vocab=32000, tgt_vocab=32000)
optimizer = torch.optim.Adam(model.parameters(), betas=(0.9, 0.98), eps=1e-9)
scheduler = TransformerLRScheduler(optimizer, d_model=512, warmup_steps=4000)
criterion = nn.CrossEntropyLoss(ignore_index=0, label_smoothing=0.1)
```

---

# 6️⃣ Key Paper Details Often Missed

| Detail | Value | Impact |
|--------|-------|--------|
| Embedding scale | × √d_model | Significant! Without it, embeddings are too small |
| Label smoothing | 0.1 | Hurts perplexity, helps BLEU |
| Post-LN vs Pre-LN | Post-LN in original | Pre-LN is more stable, used now |
| Dropout | 0.1 everywhere | Residual dropout crucial |
| Weight sharing | Embed weights = output proj | Reduces params, improves quality |
| Warmup steps | 4000 | Crucial for training stability |

> ⭐ **Beginner tip**: These "small" details (scaling, label smoothing, weight init) are often the difference between a model that trains and one that doesn't. Reproducing a paper means getting ALL of these right.

---

# 🧾 Cheat Sheet: Transformer at a Glance

| What | Formula / Value | Beginner Note |
|---|---|---|
| Scaled Dot-Product Attention | $\text{softmax}(QK^T / \sqrt{d_k})V$ | Core mechanism — "who should I pay attention to?" |
| Multi-Head Attention | $\text{Concat}(\text{head}_1..\text{head}_h)W^O$ | 8 experts looking at different things |
| Positional Encoding | $\sin(pos/10000^{2i/d})$, $\cos(...)$ | Seat numbers so order matters |
| FFN | $\max(0, xW_1+b_1)W_2+b_2$ | Personal processing step |
| Residual + LayerNorm | $\text{LN}(x + \text{Sublayer}(x))$ | Shortcut to prevent information loss |
| LR Schedule | Warmup 4000 steps + decay | Start slow, then coast |
| Label Smoothing | $\epsilon = 0.1$ | Don't be overconfident |
| Optimizer | Adam ($\beta_1=0.9, \beta_2=0.98$) | Standard but specific betas |

---

# 📝 Summary

| Component | Paper Spec | Modern Practice |
|-----------|-----------|-----------------|
| Norm placement | Post-LN | Pre-LN (more stable) |
| Activation | ReLU | SwiGLU / GELU |
| Position | Sinusoidal | RoPE / Learned |
| LR schedule | Warmup + inv sqrt | Warmup + cosine |
| Architecture | Encoder-decoder | Decoder-only (for LLMs) |

---

# 📚 Resources

### 📄 Key Papers
| Paper | Authors | Year | Why Read It |
|---|---|---|---|
| *Attention Is All You Need* | Vaswani et al. | 2017 | ⭐ THE paper — introduces the Transformer |
| *Neural Machine Translation by Jointly Learning to Align and Translate* | Bahdanau, Cho & Bengio | 2014 | Origin of the attention mechanism |
| *Formal Algorithms for Transformers* | Phuong & Hutter | 2022 | Rigorous mathematical treatment |
| *BERT: Pre-training of Deep Bidirectional Transformers* | Devlin et al. | 2019 | Encoder-only Transformer revolution |
| *Language Models are Few-Shot Learners* (GPT-3) | Brown et al. | 2020 | Decoder-only Transformer at scale |

### 📖 Books & Tutorials
- 🌐 **"The Illustrated Transformer"** by Jay Alammar — Best visual explanation for beginners (jalammar.github.io)
- 📖 **"Deep Learning"** by Goodfellow, Bengio & Courville — Chapter on attention & sequence models
- 📖 **"Speech and Language Processing"** by Jurafsky & Martin — Chapter on Transformers (free online)
- 🎥 **Stanford CS224N** — Lecture on Transformers (free on YouTube)
- 💻 **"The Annotated Transformer"** by Harvard NLP — Line-by-line code walkthrough

**Tomorrow**: Reproducing the Scaling Laws paper.
