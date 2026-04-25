📘 DAY 13 — Feed-Forward Networks, SwiGLU, and the MLP as Memory

> 🎯 **One-Line Summary**: The FFN is the transformer's "brain" where knowledge is actually stored — attention listens, but the FFN *thinks*.

---

## 🗺️ Where Are We in the Transformer?

```
Input Tokens
    ↓
[ Embedding ]
    ↓
┌──────────────────────────┐
│  Multi-Head Attention     │  ← "Listening layer" 👂 (Who should I pay attention to?)
│  ──────────────────────── │
│  ⭐ Feed-Forward Network  │  ← "Thinking layer" 🧠 (What do I know about this?) ← YOU ARE HERE
│  (FFN / MLP)              │
└──────────────────────────┘
    ↓  (repeat N times)
[ Output Head ]
    ↓
Predictions
```

---

## 🍕 The Pizza Kitchen Analogy (Start Here If You're New!)

Imagine a pizza kitchen:
- **Attention** = the waiter who listens to your order and gathers ingredients from the pantry 👂
- **FFN** = the chef who takes those ingredients and actually *cooks* something 🍳

The waiter (attention) decides *which* ingredients to grab. The chef (FFN) decides *what to do* with them. Without the chef, you'd just have a pile of raw ingredients. Without the waiter, the chef wouldn't know what to cook!

> 💡 **Key Insight**: ~2/3 of ALL parameters in a transformer live in the FFN. The "thinking" part is literally bigger than the "listening" part!

---

Goal:

Understand the Feed-Forward Network (FFN/MLP) component of transformers in depth — why it exists, how it stores knowledge, the evolution from ReLU to GELU to SwiGLU, and why the FFN contains ~2/3 of a transformer's parameters. Master the modern LLaMA-style SwiGLU MLP.

If this is deeply understood:
You'll understand WHERE knowledge is stored in LLMs (hint: mostly in the FFN), why model editing targets FFN weights, and how the activation function choice matters for model quality.

---

# 1️⃣ What Does the FFN Do?

## Attention Routes, FFN Processes

Think of transformer layers as having two functions:
1. **Attention**: determines WHICH tokens to combine (routing/communication)
2. **FFN**: processes the information at each position (computation/memory)

> 🎒 **Beginner Analogy — Study Group vs. Solo Thinking**:
> - **Attention** = a study group where everyone shares notes 📝 (communication)
> - **FFN** = going home and *thinking deeply* about what you learned 🧠 (processing)
>
> After the study group (attention), each student (token) goes home and independently processes what they heard. That's exactly what the FFN does — it works on each token *by itself*.

The FFN is applied INDEPENDENTLY to each token position:

$$\text{FFN}(x) = W_2 \cdot \sigma(W_1 \cdot x + b_1) + b_2$$

Where:
- $W_1 \in \mathbb{R}^{d_{ff} \times d_{model}}$: expand to higher dimension
- $W_2 \in \mathbb{R}^{d_{model} \times d_{ff}}$: compress back
- $\sigma$: activation function
- $d_{ff} = 4 \times d_{model}$ (standard) or $\frac{8}{3} \times d_{model}$ (SwiGLU)

> 🎯 **The Bottleneck Shape — Brainstorm → Filter → Summarize**:
> ```
> d_model (small) ──→ d_ff (BIG, 4x wider) ──→ d_model (small again)
>     768         ──→       3072            ──→       768
> ```
> Think of it like:
> 1. 🧠 **Expand** (first linear $W_1$): brainstorm LOTS of ideas (go from 768 → 3072 dimensions)
> 2. 🔥 **Filter** (activation $\sigma$): keep only the good ideas, throw away the bad ones
> 3. 📦 **Compress** (second linear $W_2$): package the best ideas back into a compact form (3072 → 768)

## FFN as Key-Value Memory

⭐ Geva et al. (2021) showed that FFN layers act as key-value memories:

> 📚 **Research Spotlight**: *"Transformer Feed-Forward Layers Are Key-Value Memories"* (Geva et al., 2021) — This groundbreaking paper showed that each FFN layer works like a database lookup. The first weight matrix stores "questions" (keys) and the second stores "answers" (values).

```
Memory view:
W₁[i, :] = key pattern (what to match)
W₂[:, i] = value (what to output when matched)

match_scores = W₁ · x     → which "keys" match the input?
gated = σ(match_scores)    → how strongly?
output = W₂ · gated        → weighted sum of "values"
```

> 🏨 **Beginner Analogy — Hotel Front Desk**:
> Imagine a hotel with thousands of mailboxes:
> - Each mailbox has a **label** (= row of $W_1$): "Guest who likes sushi", "Guest who speaks French", etc.
> - Each mailbox contains a **message** (= column of $W_2$): the response for matching guests
> - When you arrive (input $x$), the desk clerk checks which labels match you → opens those mailboxes → combines the messages
>
> More mailboxes (bigger $d_{ff}$) = the hotel can remember more things about more types of guests! 🏨

Each row of W₁ is a "pattern detector."
Each column of W₂ is the "output" when that pattern fires.

This is why:
- Model editing (ROME, MEMIT) targets FFN weights
- Larger FFN = more "memory slots" = more knowledge
- FFN contains ~67% of parameters

> 🏭 **Production Insight**: This is why techniques like **ROME** (Rank-One Model Editing) and **MEMIT** (Mass-Editing Memory in a Transformer) specifically target FFN weight matrices to edit facts in LLMs. Want to change "The Eiffel Tower is in Paris" to "The Eiffel Tower is in London"? You edit a few FFN weights. This is used in production to fix factual errors without full retraining.

---

# 2️⃣ Activation Functions: The Evolution

> 🚦 **Beginner Analogy — Traffic Lights for Neurons**:
> An activation function is like a traffic light for information. It decides what gets through (green 🟢), what gets partially through (yellow 🟡), and what gets blocked (red 🔴). Different activation functions are different styles of traffic control — some are harsh (on/off), some are smooth (gradual).

### ⭐ The Activation Function Timeline

| Year | Function | Used By | Key Property | Status |
|------|----------|---------|-------------|--------|
| 2017 | ReLU | Original Transformer | Hard cutoff at 0 | 🪦 Outdated |
| 2018 | GELU | GPT-2, BERT | Smooth, probabilistic | ✅ Still used |
| 2020 | SiLU/Swish | Component of SwiGLU | Self-gating | 🔧 Building block |
| 2020 | SwiGLU | LLaMA, Mistral, GPT-4 | Gated + smooth | ⭐ State of the Art |

## ReLU (Original Transformer)

$$\text{ReLU}(x) = \max(0, x)$$

Problem: hard zero creates "dead neurons" that never activate.

> 🚪 **Analogy**: ReLU is like a door that's either WIDE OPEN or COMPLETELY SHUT. If the input is negative, the door slams shut (output = 0). If positive, the door is wide open (output = input). The problem? Once a door gets stuck shut (dead neuron), it may NEVER open again! 💀

## GELU (GPT-2, BERT)

$$\text{GELU}(x) = x \cdot \Phi(x) \approx x \cdot \sigma(1.702x)$$

Where $\Phi$ is the Gaussian CDF.
Smooth approximation to ReLU. No hard cutoff.

> 🌊 **Analogy**: GELU is like a dimmer switch instead of an on/off switch. Small negative inputs aren't completely blocked — they're *gently* reduced. This smooth behavior helps the model learn more nuanced patterns.
>
> 📚 **Research Note**: GELU was introduced by Hendrycks & Gimpel (2016) and became the default activation for BERT and GPT-2. The name stands for **G**aussian **E**rror **L**inear **U**nit.

## SiLU / Swish (Used in SwiGLU)

$$\text{SiLU}(x) = x \cdot \sigma(x)$$

Where $\sigma$ is the sigmoid function.
Self-gated: the input gates itself.

> 🔄 **Analogy**: SiLU is like the input asking itself: *"How important am I?"* The sigmoid part ($\sigma(x)$) produces a score between 0 and 1, and the input multiplies itself by that score. Big inputs get through mostly unchanged; small/negative inputs suppress themselves. It's like self-grading! 📝

## GLU (Gated Linear Unit)

$$\text{GLU}(x, W, V) = \sigma(xW) \odot xV$$

Split into two paths: one computes GATE, one computes VALUE.
Element-wise product applies the gate.

> 🚧 **Analogy — Quality Control on an Assembly Line**:
> - Path 1 ($\sigma(xW)$) = the **quality inspector** who gives each item a score from 0 to 1 🔍
> - Path 2 ($xV$) = the **actual products** coming down the line 📦
> - The element-wise product ($\odot$) = the inspector lets high-quality items through and blocks low-quality ones

## ⭐ SwiGLU (LLaMA, Mistral, Modern LLMs) ★

$$\text{SwiGLU}(x) = \text{SiLU}(xW_{gate}) \odot (xW_{up})$$

> 🏆 **This is THE activation function used in virtually all modern LLMs (2023+)**: GPT-4, LLaMA 2/3, Mistral, Gemma, and more.

Three projections instead of two:
- W_gate: produces gating signal (passed through SiLU)
- W_up: produces values
- W_down: projects back to d_model

> 🎛️ **Beginner Analogy — The Quality Filter**:
> Imagine you're writing an essay:
> 1. 📝 **W_up** (Up projection): You brainstorm ALL your ideas onto paper
> 2. 🔍 **W_gate** (Gate projection): A critic reads your ideas and scores each one (using SiLU — smooth scoring)
> 3. ✂️ **Element-wise multiply** ($\odot$): Ideas with low scores get removed, good ideas stay
> 4. 📦 **W_down** (Down projection): Package the surviving good ideas into your final essay
>
> The gate is like having a **separate quality filter** — the model learns WHICH features matter!

```
⭐ Visual Flow of SwiGLU:

                    ┌── W_gate ──→ SiLU ──┐
Input x ──────────┤                       ├──→ ⊙ (multiply) ──→ W_down ──→ Output
                    └── W_up ─────────────┘
                    
The gate path DECIDES. The up path PROPOSES. The multiply FILTERS. The down path COMPRESSES.
```

```python
class SwiGLUFFN(torch.nn.Module):
    """SwiGLU Feed-Forward Network (LLaMA style)."""
    
    def __init__(self, d_model, d_ff=None):
        super().__init__()
        # LLaMA uses 8/3 × d_model (then rounded to nearest multiple of 256)
        if d_ff is None:
            d_ff = int(8 / 3 * d_model)
            d_ff = 256 * ((d_ff + 255) // 256)  # Round up
        
        self.w_gate = torch.nn.Linear(d_model, d_ff, bias=False)  # Gate projection
        self.w_up = torch.nn.Linear(d_model, d_ff, bias=False)    # Up projection
        self.w_down = torch.nn.Linear(d_ff, d_model, bias=False)  # Down projection
    
    def forward(self, x):
        gate = torch.nn.functional.silu(self.w_gate(x))  # SiLU activation on gate
        up = self.w_up(x)                                  # Linear projection
        return self.w_down(gate * up)                       # Element-wise product + down project
```

### Why SwiGLU is Better

1. **Gating mechanism**: the model can selectively suppress features
2. **Smoother gradients**: SiLU doesn't have hard cutoffs
3. **Empirically superior**: ~1% perplexity improvement over GELU in ablations (PaLM paper)
4. **More expressive**: two projections provide richer feature interactions

> 📚 **Research Deep Dive**: Shazeer (2020), *"GLU Variants Improve Transformer"*, systematically tested GELU, SiLU, ReLU, and their GLU variants. SwiGLU consistently outperformed alternatives, with ~0.5–1% perplexity improvement. This paper is why every modern LLM switched to SwiGLU. The key insight: **giving the model an explicit gating mechanism is worth the extra parameters.**

### Parameter Count with SwiGLU

Standard FFN: $2 \times d_{model} \times d_{ff} = 2 \times d \times 4d = 8d^2$ per layer
SwiGLU FFN: $3 \times d_{model} \times d_{ff} = 3 \times d \times \frac{8}{3}d = 8d^2$ per layer

Same parameter budget! SwiGLU uses 3 matrices at 2/3 the width instead of 2 matrices at full width.

> 🧮 **Wait, the same number of parameters?!** Yes! This is clever engineering:
> - Standard: 2 big matrices ($d \times 4d$) = $8d^2$
> - SwiGLU: 3 smaller matrices ($d \times \frac{8}{3}d$) = $3 \times \frac{8}{3}d^2 = 8d^2$
>
> You get the gating mechanism **for free** (in terms of parameter count) by making each matrix slightly narrower!

---

# 3️⃣ FFN Dimension Analysis

## Where Are the Parameters?

> 🏗️ **Beginner Analogy — Building Floors**: If a transformer block is a building, attention is the lobby (1/3 of the space) and the FFN is the office floors (2/3 of the space). Most of the "work" (and storage!) happens in the offices.

For a transformer block with $d_{model} = d$:

| Component | Parameters | % of Block | Analogy |
|-----------|-----------|------------|---------|
| Attention Q, K, V, O | $4d^2$ | ~33% | 👂 The "listening" part |
| FFN (standard 4x) | $8d^2$ | ~67% | 🧠 The "thinking" part |
| Layer Norms | $4d$ | ~0% | 🧹 Cleanup (tiny) |
| **Total per block** | **~$12d^2$** | **100%** | 🏢 One transformer floor |

$$\text{FFN fraction} = \frac{8d^2}{12d^2} = \frac{2}{3} \approx \textbf{67\% of each block's parameters!}$$

The FFN IS the memory of the transformer.

> ⭐ **Mind-Blowing Implication**: When people say "GPT-4 has hundreds of billions of parameters," about **two-thirds** of those parameters are in FFN layers. The attention mechanism that everyone talks about? It's only 1/3 of the model. The FFN is the quiet workhorse that nobody talks about but stores most of the knowledge! 🤫

## Why 4× Expansion?

The original paper didn't deeply justify 4×.
Empirically, it works well. The sweet spot is:
- Too small (2×): not enough capacity
- 4× (standard) or 8/3 (SwiGLU): good balance
- Too large (8×): diminishing returns, memory issues

> 📚 **Research Note**: Vaswani et al. (2017), *"Attention Is All You Need"*, set $d_{ff} = 4 \times d_{model} = 2048$ for their base model ($d_{model} = 512$). This 4× ratio has become the de facto standard, though SwiGLU models use $\frac{8}{3}\times$ to maintain the same parameter count with 3 matrices.

### ⭐ Real-World FFN Dimensions

| Model | $d_{model}$ | $d_{ff}$ | Ratio | Activation | Params/Layer |
|-------|-------------|----------|-------|------------|--------------|
| GPT-2 Small | 768 | 3,072 | 4× | GELU | ~4.7M |
| BERT-Base | 768 | 3,072 | 4× | GELU | ~4.7M |
| LLaMA-7B | 4,096 | 11,008 | ~2.69× | SwiGLU | ~135M |
| LLaMA-70B | 8,192 | 28,672 | ~3.5× | SwiGLU | ~704M |
| Mistral-7B | 4,096 | 14,336 | 3.5× | SwiGLU | ~176M |

---

# 4️⃣ Neuron Analysis: What Do FFN Neurons Encode?

> 🔬 **Why This Matters**: Understanding what individual neurons do is the foundation of **mechanistic interpretability** — figuring out HOW neural networks actually work inside. This is one of the hottest research areas in AI safety.

Research (Dai et al., 2022; Gurnee et al., 2023) has found:

## Knowledge Neurons 📖
Individual FFN neurons that activate for specific facts:
- "The Eiffel Tower is in [Paris]" → specific neuron fires
- Suppressing that neuron makes the model "forget" the fact

> 🧠 **Analogy**: Imagine your brain has a tiny cell specifically for "Paris facts." When someone mentions the Eiffel Tower, that cell lights up and says "Paris!" If that cell was damaged, you might suddenly forget where the Eiffel Tower is — but remember everything else perfectly. FFN neurons work the same way!
>
> 📚 **Research**: Dai et al. (2022), *"Knowledge Neurons in Pretrained Transformers"* — identified specific neurons in BERT that store relational facts and showed that suppressing/amplifying them can erase/enhance specific knowledge.

## Feature Neurons 🎯
Neurons that detect linguistic features:
- Neuron 1023 in layer 15: fires for sentences about animals
- Neuron 4567 in layer 20: fires for code syntax

## Polysemantic Neurons 🎭
Many neurons respond to MULTIPLE unrelated concepts:
- Same neuron fires for "baseball" AND "the number 7"
- This is superposition: more features than neurons, so they overlap

> 🎭 **Analogy — Overworked Employee**: Imagine a small company where one person handles BOTH customer support AND accounting. They respond to two completely unrelated things. This is **superposition** — the model needs to represent more concepts than it has neurons, so individual neurons "moonlight" across multiple roles!
>
> 📚 **Research Deep Dive**: Elhage et al. (2022), *"Toy Models of Superposition"* (Anthropic) — showed that neural networks deliberately pack more features than dimensions using superposition, making interpretability fundamentally harder. This is a key open problem in AI safety research.

```python
def find_activated_neurons(model, tokenizer, text, layer_idx=10, threshold=5.0):
    """Find which FFN neurons activate for given text."""
    tokens = tokenizer.encode(text, return_tensors='pt')
    
    activations = []
    def hook_fn(module, input, output):
        activations.append(input[0])  # Input to the down projection
    
    # Hook the down projection in the target layer
    hook = model.layers[layer_idx].ffn.w_down.register_forward_hook(hook_fn)
    
    with torch.no_grad():
        model(tokens)
    
    hook.remove()
    
    # Find strongly activated neurons
    act = activations[0][0, -1]  # Last token position
    top_neurons = torch.where(act > threshold)[0]
    
    print(f"Text: {text}")
    print(f"Layer {layer_idx}: {len(top_neurons)} neurons activated above threshold")
    for idx in top_neurons[:10]:
        print(f"  Neuron {idx}: activation = {act[idx]:.2f}")
```

---

# 5️⃣ FFN as a Two-Layer Network

The FFN is literally a two-layer neural network applied to each token:

```
Token representation → [Linear → Activation → Linear] → Updated representation
```

> 🎓 **Beginner Analogy — A Mini Brain at Every Position**:
> Each token in the sequence gets its own personal "mini brain" (the FFN) that processes it. It's like having a personal tutor for each word in a sentence — they all use the same *skills* (shared weights) but work on different *students* (different token representations).
>
> ```
> "The  cat  sat  on  the  mat"
>   ↓    ↓    ↓    ↓   ↓    ↓
>  FFN  FFN  FFN  FFN  FFN  FFN   ← Same weights, different inputs!
>   ↓    ↓    ↓    ↓   ↓    ↓
> out₁ out₂ out₃ out₄ out₅ out₆
> ```

From universal approximation theorem: with enough hidden units, this can approximate any function.

> 📚 **What's the Universal Approximation Theorem?**  
> In plain English: a two-layer neural network (which is exactly what the FFN is!) can learn to compute *any* mathematical function — as long as it has enough neurons in the hidden layer. This is why making $d_{ff}$ big (4× the model dimension) gives the FFN the capacity to learn complex transformations.

So each FFN layer can:
- Recall facts (key-value memory) 📖
- Apply transformations (function approximation) 🔄
- Filter/gate information (via activation) 🚦

The DEPTH of the model (stacking blocks) allows composing these operations.

> 💡 **Key Insight**: A single FFN layer is powerful but limited. By **stacking** transformer blocks (each with its own FFN), the model can compose simple operations into incredibly complex reasoning chains. Layer 1's FFN might detect "this is a noun," layer 10's FFN might detect "this noun is the subject," and layer 30's FFN might determine "the answer to the question is this subject."

---

# 6️⃣ FFN Variants in Modern Architectures

> 🗺️ **The Evolution Map**: From simple on/off switches to sophisticated gated networks — here's how FFNs have evolved across landmark models.

```
                    Expansion    Activation    # Matrices    Used By
Standard FFN:       4×           ReLU/GELU     2            GPT-2, BERT
SwiGLU:            8/3×         SiLU+Gate      3            LLaMA-2/3, Mistral
GeGLU:             8/3×         GELU+Gate      3            Some research
ReGLU:             8/3×         ReLU+Gate      3            Some research
MoE FFN:           varies       varies         varies       Mixtral, Switch
```

### ⭐ FFN Variant Comparison Table

| Variant | Formula | Pros | Cons | Production Use |
|---------|---------|------|------|---------------|
| Standard + ReLU | $W_2 \cdot \text{ReLU}(W_1 x)$ | Simple, fast | Dead neurons | Original Transformer (2017) |
| Standard + GELU | $W_2 \cdot \text{GELU}(W_1 x)$ | Smooth gradients | No gating | GPT-2, BERT, GPT-3 |
| SwiGLU | $W_{down}(\text{SiLU}(xW_{gate}) \odot xW_{up})$ | Best quality, gated | 3 matrices | ⭐ LLaMA, Mistral, GPT-4 |
| MoE | Router → top-k experts | Massive scale | Complex routing | Mixtral, Switch Transformer |

### ⭐ Mixture of Experts (MoE): The Frontier 🚀

Instead of ONE large FFN, use many SMALL FFNs (experts):
- Router: selects top-k experts for each token
- Only activated experts compute → sparse computation
- Total parameters huge, but compute per token small

> 🍽️ **Beginner Analogy — The Restaurant Kitchen**:
> Instead of one mega-chef who tries to cook everything (standard FFN), imagine a kitchen with 8 specialist chefs 👨‍🍳:
> - Chef 1: Italian specialist 🍝
> - Chef 2: Sushi specialist 🍣
> - Chef 3: BBQ specialist 🥩
> - ... and so on
>
> When an order (token) comes in, a **router** (maître d') looks at it and says: *"This needs Chefs 2 and 5!"* Only those 2 chefs work on it. The kitchen has 8 chefs' worth of knowledge but only uses 2 at a time — so it's fast AND knowledgeable!

```
⭐ MoE Architecture:

Input token ──→ Router (small network)
                    │
                    ├──→ Expert 1 (FFN) ──┐
                    ├──→ Expert 2 (FFN) ──┤  ← Only top-k (usually 2)
                    ├──→ Expert 3 (FFN)   │     are actually computed!
                    ├──→ Expert 4 (FFN)   │
                    ├──→ ...              │
                    └──→ Expert 8 (FFN)   │
                                          ↓
                              Weighted sum of top-k outputs ──→ Output
```

Mixtral-8x7B: 8 experts, top-2 routing → 46.7B params, ~13B active per token.

> 📚 **Research Deep Dive — MoE Papers**:
> - **Fedus et al. (2022)**, *"Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity"* — showed you can scale to 1.6 TRILLION parameters using top-1 routing with a single expert per token. Key innovation: simplified routing for stability.
> - **Jiang et al. (2024)**, *"Mixtral of Experts"* (Mistral AI) — Mixtral-8x7B achieves performance comparable to LLaMA-2 70B while using only ~13B active parameters per token. This is the magic of MoE: **massive knowledge, efficient compute**.
>
> 🏭 **Production Reality**: MoE models are harder to deploy because all expert weights must be in memory even though only a fraction are used per token. This creates engineering challenges around memory management, load balancing across experts, and distributed inference.

---

# 7️⃣ Practical: Compare Activation Functions

```python
import torch
import matplotlib.pyplot as plt

x = torch.linspace(-5, 5, 1000)

fig, axes = plt.subplots(2, 3, figsize=(15, 8))

activations = {
    'ReLU': torch.relu(x),
    'GELU': torch.nn.functional.gelu(x),
    'SiLU (Swish)': torch.nn.functional.silu(x),
    'Sigmoid': torch.sigmoid(x),
    'Tanh': torch.tanh(x),
    'Softplus': torch.nn.functional.softplus(x),
}

for ax, (name, y) in zip(axes.flat, activations.items()):
    ax.plot(x.numpy(), y.numpy(), linewidth=2)
    ax.set_title(name, fontsize=14)
    ax.grid(True, alpha=0.3)
    ax.axhline(y=0, color='k', linewidth=0.5)
    ax.axvline(x=0, color='k', linewidth=0.5)

plt.suptitle('Activation Functions in Transformers', fontsize=16)
plt.tight_layout()
plt.show()
```

---

# 8️⃣ Practical: Implement and Compare FFN Variants

```python
import torch
import torch.nn as nn
import time

def benchmark_ffn(ffn_class, d_model=1024, batch_size=32, seq_len=512, num_iters=100):
    """Benchmark FFN variant."""
    model = ffn_class(d_model).cuda()
    x = torch.randn(batch_size, seq_len, d_model).cuda()
    
    # Warmup
    for _ in range(10):
        _ = model(x)
    torch.cuda.synchronize()
    
    # Benchmark
    start = time.time()
    for _ in range(num_iters):
        _ = model(x)
    torch.cuda.synchronize()
    elapsed = time.time() - start
    
    params = sum(p.numel() for p in model.parameters())
    print(f"{ffn_class.__name__:20s}: {params/1e6:.1f}M params, {elapsed:.3f}s ({num_iters} iters)")

class StandardFFN(nn.Module):
    def __init__(self, d):
        super().__init__()
        self.fc1 = nn.Linear(d, 4*d, bias=False)
        self.fc2 = nn.Linear(4*d, d, bias=False)
    def forward(self, x):
        return self.fc2(torch.nn.functional.gelu(self.fc1(x)))

class SwiGLUFFN(nn.Module):
    def __init__(self, d):
        super().__init__()
        d_ff = int(8/3 * d)
        self.w_gate = nn.Linear(d, d_ff, bias=False)
        self.w_up = nn.Linear(d, d_ff, bias=False)
        self.w_down = nn.Linear(d_ff, d, bias=False)
    def forward(self, x):
        return self.w_down(torch.nn.functional.silu(self.w_gate(x)) * self.w_up(x))

# Compare
# benchmark_ffn(StandardFFN)
# benchmark_ffn(SwiGLUFFN)
```

---

# 📝 Summary

| Concept | Key Insight | Beginner Analogy |
|---------|-------------|-----------------|
| FFN purpose | Process/store information at each position independently | 🧠 The chef who cooks (attention just gathers ingredients) |
| FFN as memory | W₁ rows = keys, W₂ columns = values | 🏨 Hotel mailboxes: labels match guests → return messages |
| % of parameters | ~67% of each transformer block | 🏗️ The offices are 2/3 of the building |
| SwiGLU | Gate + value with SiLU; 3 matrices at 8/3× width | 🔍 Brainstorm ideas → quality filter → keep the good ones |
| Neuron analysis | Individual neurons encode facts, features, concepts | 🧠 Brain cells that fire for specific memories |
| Superposition | More features than neurons → polysemantic encoding | 🎭 Overworked employee doing two unrelated jobs |
| MoE | Multiple expert FFNs, sparse routing → more params, same compute | 🍽️ Specialist chefs, only 2 cook per order |

**Tomorrow**: Language Modeling Objective — Cross-Entropy Loss, Perplexity, and the training target that makes GPT work.

---

# 🧾 Cheat Sheet: Feed-Forward Networks at a Glance

## ⭐ The 5 Things You MUST Remember

1. **FFN = the "thinking layer"** — Attention listens, FFN thinks. Applied independently to each token.
2. **Shape: d → 4d → d** — Expand (brainstorm), activate (filter), compress (summarize).
3. **~67% of parameters** — The FFN is where most of the model's knowledge lives ($8d^2$ out of $12d^2$).
4. **SwiGLU is king** — $\text{SwiGLU}(x) = \text{SiLU}(xW_{gate}) \odot (xW_{up})$ then $W_{down}$. Used by GPT-4, LLaMA, Mistral.
5. **FFN = key-value memory** — $W_1$ rows detect patterns (keys), $W_2$ columns produce outputs (values).

## ⭐ Key Formulas Card

| Formula | What It Means |
|---------|--------------|
| $\text{FFN}(x) = W_2 \cdot \text{GELU}(W_1 x + b_1) + b_2$ | Standard FFN (GPT-2 era) |
| $\text{SwiGLU}(x) = (\ \text{Swish}(xW_{gate}) \odot xW_{up}\ ) W_{down}$ | Modern FFN (LLaMA era) |
| $\text{GELU}(x) = x \cdot \Phi(x)$ | Smooth activation (Gaussian gating) |
| $\text{SiLU}(x) = x \cdot \sigma(x)$ | Self-gated activation (used in SwiGLU) |
| $\text{Parameters} = 2 \times d \times 4d = 8d^2$ | FFN params per layer |
| $d_{ff} = 4 \times d_{model}$ | Standard expansion ratio |
| $\frac{8d^2}{12d^2} = \frac{2}{3}$ | FFN fraction of transformer block |

## 🏭 Production Quick Reference

| What | Who Uses It | Key Detail |
|------|------------|------------|
| SwiGLU FFN | GPT-4, LLaMA 2/3, Mistral, Gemma | 3 matrices, $\frac{8}{3}\times$ width, no bias |
| GELU FFN | GPT-2, GPT-3, BERT | 2 matrices, $4\times$ width |
| MoE FFN | Mixtral-8x7B, Switch Transformer | 8 experts, top-2 routing, sparse |
| Knowledge editing | ROME, MEMIT | Target $W_1$/$W_2$ to change facts |

## 📚 Essential Papers Reference

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|-----------------|
| *Attention Is All You Need* | Vaswani et al. | 2017 | Introduced the transformer; standard FFN with $d_{ff}=4d$ |
| *GLU Variants Improve Transformer* | Shazeer | 2020 | Showed SwiGLU beats GELU/ReLU; now used everywhere |
| *Transformer FFN Layers Are Key-Value Memories* | Geva et al. | 2021 | Proved FFN acts as a memory store |
| *Knowledge Neurons in Pretrained Transformers* | Dai et al. | 2022 | Found individual neurons that store specific facts |
| *Switch Transformers* | Fedus et al. | 2022 | Scaled MoE to 1.6T params with top-1 routing |
| *Toy Models of Superposition* | Elhage et al. | 2022 | Explained polysemantic neurons and superposition |
| *Mixtral of Experts* | Jiang et al. | 2024 | Production MoE: 46.7B params, ~13B active |

---

> 🎯 **Final Takeaway**: The FFN is the unsung hero of transformers. While attention gets all the fame, the FFN is where the model actually *stores and processes knowledge*. Mastering FFN architecture — from the bottleneck shape to SwiGLU gating to MoE routing — is essential for understanding modern LLMs. 🧠
