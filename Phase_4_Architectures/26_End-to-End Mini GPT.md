📘 DAY 26 — End-to-End Mini GPT: Complete LLM Project 🏗️🚀🧠

> *"The best way to understand how something works is to build it from scratch."* — Andrej Karpathy

Goal:

Synthesize everything from Days 41-59 into a complete, working mini GPT project. Build, train, evaluate, fine-tune, and deploy a small language model from scratch. This is your capstone project for the LLM Training phase.

---

## 🌟 Why This Day Matters — The "Capstone Moment"

⭐ **This is THE most important day in the entire LLM track.** Everything you've learned — tokenization, attention, feed-forward networks, normalization, training loops, generation — comes together into ONE working system.

### 🚗 The Car Assembly Analogy

Imagine you've spent weeks learning about individual car parts:
- **Day 41-43 (Tokenizer)** = The fuel system — converts raw material (text) into something the engine can use (numbers)
- **Day 44 (Positional Encoding)** = The GPS — tells the engine WHERE each part of the input is
- **Day 45 (Attention)** = The steering system — decides what to focus on at each moment
- **Day 46 (FFN / SwiGLU)** = The engine — does the heavy computation that transforms inputs into outputs
- **Day 47 (Normalization)** = The oil system — keeps everything running smoothly without overheating
- **Day 48-55 (Training)** = The test track — where you actually drive the car and tune its performance
- **Day 56-59 (Fine-tuning)** = Custom modifications — adapting your car for specific road conditions

**Today, you're the mechanic putting the entire car together on the assembly line.** Every bolt, every wire, every component — assembled into a vehicle that actually drives! 🏎️

### 🎯 What Makes This Different from Just Reading About GPT

| Before Today | After Today |
|:---|:---|
| "I understand attention in theory" | "I built attention and watched it learn patterns" |
| "I know what a transformer is" | "I can code one from scratch, train it, and generate text" |
| "GPT seems like magic" | "GPT is just matrix multiplications arranged cleverly" |
| "I read papers about LLMs" | "I OWN the entire pipeline end-to-end" |

> 💡 **For absolute beginners**: Think of GPT like a very sophisticated autocomplete on your phone. You type "I'm going to the..." and it predicts "store" or "park". Our mini GPT does the same thing, but with a deep understanding of language patterns learned from millions of text examples.

---

## 📚 Deep Research Context: The "Build It From Scratch" Movement

⭐ **Key Insight**: The idea of building GPT from scratch was popularized by **Andrej Karpathy's nanoGPT** (2023), a ~300-line implementation that trains a GPT-2 scale model. Our mini GPT follows this philosophy.

| Project | Parameters | Training Time | Creator | Year |
|:---|:---|:---|:---|:---|
| **nanoGPT** | ~124M | ~45 min (1× A100) | Karpathy | 2023 |
| **minGPT** | ~Configurable | Hours on consumer GPU | Karpathy | 2020 |
| **GPT-2** | 1.5B | Weeks on 256 TPUs | OpenAI | 2019 |
| **GPT-3** | 175B | ~$4.6M compute | OpenAI | 2020 |
| **LLaMA** | 7B–65B | 2048 A100s for 21 days | Meta | 2023 |
| **Our Mini GPT** | ~85M | Hours on 1 GPU | You! 🎉 | Today |

> 📖 **References**:
> - Karpathy, A. (2023). *nanoGPT*. GitHub. https://github.com/karpathy/nanoGPT
> - Radford, A. et al. (2019). *Language Models are Unsupervised Multitask Learners*. OpenAI.
> - Brown, T. et al. (2020). *Language Models are Few-Shot Learners*. NeurIPS.
> - Touvron, H. et al. (2023). *LLaMA: Open and Efficient Foundation Language Models*. Meta AI.

---

# 1️⃣ Project Structure 📁

> 🎨 **Analogy**: This is the **blueprint of our car factory**. Each file is a department that handles one specific job. Just like a car factory has separate areas for engines, bodies, and painting — our project separates concerns cleanly.

```
mini_gpt/
├── config.py          # Model & training configurations
├── tokenizer.py       # BPE tokenizer wrapper
├── model.py           # GPT architecture
├── dataset.py         # Data loading & preprocessing
├── train.py           # Training loop
├── evaluate.py        # Evaluation & benchmarks
├── generate.py        # Text generation
├── finetune.py        # LoRA fine-tuning
└── utils.py           # Logging, checkpointing
```

### 🧩 How Each File Maps to What You've Learned

| File | What It Does | Car Analogy 🚗 | Day(s) Covered |
|:---|:---|:---|:---|
| `config.py` | Defines all settings (model size, learning rate, etc.) | The specification sheet — "4-cylinder, automatic, red paint" | All |
| `tokenizer.py` | Converts text ↔ numbers | Fuel injection — converts gasoline into combustible form | Day 41-43 |
| `model.py` | The actual GPT neural network | The complete engine assembly | Day 44-47 |
| `dataset.py` | Loads and batches training data | The fuel delivery pipeline | Day 48 |
| `train.py` | The training loop that teaches the model | The test track where you drive and tune | Day 49-55 |
| `evaluate.py` | Measures how good the model is | Quality inspection — "does the car actually drive well?" | Day 52-53 |
| `generate.py` | Produces text from the trained model | Driving the finished car! 🏎️ | Day 52-53 |
| `finetune.py` | Adapts the model for specific tasks | Custom modifications for racing / off-road / luxury | Day 56-59 |
| `utils.py` | Helper functions (logging, saving) | The toolbox and maintenance equipment | All |

> 💡 **For beginners**: You don't need to understand every line today. The goal is to see HOW all the pieces connect. It's like seeing the full assembly line — you'll understand each station better once you see the whole picture.

### 🏭 Production Reality Check

⭐ **At companies like OpenAI, Anthropic, or Google**, the project structure is far more complex:

```
# Real production LLM training codebase (~thousands of files)
llm_training/
├── infra/              # Distributed computing, cluster management
├── data/               # Data pipelines, filtering, deduplication
│   ├── crawlers/       # Web scraping at petabyte scale
│   ├── filters/        # Quality filtering, deduplication
│   └── tokenizers/     # Multiple tokenizer implementations
├── model/              # Model architecture (modular, configurable)
├── training/           # Distributed training orchestration
│   ├── fsdp/           # Fully Sharded Data Parallel
│   ├── megatron/       # Tensor/pipeline parallelism
│   └── checkpointing/  # Distributed checkpoint management
├── eval/               # Hundreds of evaluation benchmarks
├── serving/            # Model serving infrastructure
│   ├── quantization/   # INT8/INT4 quantization
│   ├── triton/         # GPU kernel optimization
│   └── api/            # REST/gRPC endpoints
└── monitoring/         # Training dashboards, alerts
```

**Our 9-file project captures the ESSENCE of all of this.** The concepts are identical — the scale is different.

---

# 2️⃣ Configuration ⚙️

> 🎨 **Analogy**: The configuration is like the **recipe card** before you start cooking. It lists every ingredient (hyperparameter) and every measurement (value). Change one number and you get a completely different dish!

> 💡 **For beginners**: A "hyperparameter" is a setting YOU choose before training starts. The model can't learn these — you have to decide them. It's like choosing the oven temperature and cooking time before baking a cake.

```python
# config.py
from dataclasses import dataclass

@dataclass
class ModelConfig:
    vocab_size: int = 50257
    d_model: int = 512
    num_heads: int = 8
    num_layers: int = 8
    max_seq_len: int = 512
    dropout: float = 0.1
    bias: bool = False
    
    @property
    def num_params(self):
        # Approximate parameter count
        emb = self.vocab_size * self.d_model
        per_block = 12 * self.d_model ** 2  # ~4d² attn + 8d² FFN
        return emb + self.num_layers * per_block

@dataclass
class TrainConfig:
    # Data
    data_dir: str = "./data"
    batch_size: int = 32
    
    # Optimization
    learning_rate: float = 3e-4
    weight_decay: float = 0.1
    betas: tuple = (0.9, 0.95)
    max_grad_norm: float = 1.0
    
    # Schedule
    total_steps: int = 10000
    warmup_steps: int = 500
    min_lr_ratio: float = 0.1
    
    # Training
    gradient_accumulation_steps: int = 4
    mixed_precision: bool = True
    gradient_checkpointing: bool = False
    
    # Logging
    log_interval: int = 10
    eval_interval: int = 250
    save_interval: int = 1000
    
    # Paths
    checkpoint_dir: str = "./checkpoints"
    log_dir: str = "./logs"
```

### 🔍 Hyperparameter Deep Dive — What Each One Means (and WHY)

#### 📐 Model Architecture Hyperparameters

| Parameter | Value | What It Means (Plain English) | Analogy 🚗 |
|:---|:---|:---|:---|
| `vocab_size = 50257` | 50,257 "words" the model knows | The dictionary size — how many unique word-pieces the model can recognize | How many different parts the car can identify |
| `d_model = 512` | Each token is a 512-dimensional vector | The "richness" of each word's representation — more dimensions = more nuance | Engine displacement — bigger = more powerful |
| `num_heads = 8` | 8 attention heads | 8 different "perspectives" looking at the same text simultaneously | 8 side mirrors, each showing a different angle |
| `num_layers = 8` | 8 transformer blocks stacked | Depth of reasoning — each layer refines understanding further | 8 rounds of review — like proofreading 8 times |
| `max_seq_len = 512` | Processes up to 512 tokens at once | The "memory window" — how much text the model can see at once | How far ahead the GPS can plan the route |
| `dropout = 0.1` | Randomly turns off 10% of neurons | Prevents memorization (overfitting) — forces generalization | Practicing driving with random roads blocked — makes you adaptable |
| `bias = False` | No bias terms in linear layers | Modern LLMs skip biases — slightly fewer parameters, works fine | Removing unnecessary trim pieces to save weight |

#### ⭐ The Parameter Count Formula Explained

The model's total parameter count is approximately:

$$\text{Total Params} \approx \underbrace{V \times d}_{\text{token embeddings}} + \underbrace{L \times 12d^2}_{\text{transformer blocks}}$$

Where:
- $V = 50{,}257$ (vocabulary size)
- $d = 512$ (model dimension)
- $L = 8$ (number of layers)

Breaking down the $12d^2$ per block:
- **Attention QKV projection**: $3 \times d \times d = 3d^2$
- **Attention output projection**: $d \times d = d^2$
- **FFN (SwiGLU has 3 matrices)**: $\approx 3 \times d \times \frac{8d}{3} = 8d^2$
- **Total per block**: $3d^2 + d^2 + 8d^2 = 12d^2$

$$\text{Our model} \approx 50{,}257 \times 512 + 8 \times 12 \times 512^2 \approx 25.7M + 25.2M \approx \boxed{85M \text{ parameters}}$$

> 💡 **For beginners**: A "parameter" is a single number the model learns during training. 85 million parameters means 85 million knobs being tuned simultaneously. GPT-3 has 175 **billion** — that's 2,000× more knobs!

#### 🏋️ Training Hyperparameters Explained

| Parameter | Value | What It Means | Why This Value? |
|:---|:---|:---|:---|
| `learning_rate = 3e-4` | 0.0003 | How big each optimization step is | Karpathy's "magic number" — works reliably for Adam on transformers |
| `weight_decay = 0.1` | Penalizes large weights | Prevents overfitting by keeping weights small | Standard for transformers (Loshchilov & Hutter, 2019) |
| `betas = (0.9, 0.95)` | Adam momentum parameters | $\beta_1$ = short-term momentum, $\beta_2$ = long-term variance tracking | GPT-3 used (0.9, 0.95) — we follow that |
| `max_grad_norm = 1.0` | Clip gradients if too large | Prevents "exploding gradients" that destabilize training | Standard safety net |
| `warmup_steps = 500` | Gradually increase LR for 500 steps | Cold engine needs warming up — too fast at start = unstable | ~5% of total training is typical |
| `gradient_accumulation = 4` | Accumulate 4 mini-batches before updating | Simulates a 4× larger batch without 4× memory | Tricks a small GPU into acting bigger |
| `mixed_precision = True` | Use FP16 for forward pass | Cuts memory usage in half, trains ~2× faster | Essentially free performance — always use it |

#### 🔬 The Learning Rate — The Most Important Hyperparameter

⭐ **The learning rate (LR) is the single most impactful hyperparameter.** Too high and training explodes. Too low and training takes forever.

The cosine schedule with warmup looks like this:

$$\text{LR}(t) = \begin{cases} \text{lr}_{\max} \times \frac{t}{T_{\text{warmup}}} & \text{if } t < T_{\text{warmup}} \\ \text{lr}_{\min} + \frac{1}{2}(\text{lr}_{\max} - \text{lr}_{\min})\left(1 + \cos\left(\frac{t - T_{\text{warmup}}}{T_{\text{total}} - T_{\text{warmup}}} \pi\right)\right) & \text{otherwise} \end{cases}$$

> 🎨 **Analogy**: Imagine learning to ride a bike:
> - **Warmup phase** (steps 0-500): You start with training wheels, going slowly — building confidence
> - **Peak phase** (step 500): Training wheels off! Full speed learning
> - **Cosine decay** (steps 500-10000): Gradually slowing down to fine-tune your balance — making smaller and smaller corrections

### 🏭 Production Config Comparison

| Hyperparameter | Our Mini GPT | GPT-3 (175B) | LLaMA-2 (70B) |
|:---|:---|:---|:---|
| `d_model` | 512 | 12,288 | 8,192 |
| `num_heads` | 8 | 96 | 64 |
| `num_layers` | 8 | 96 | 80 |
| `max_seq_len` | 512 | 2,048 | 4,096 |
| `batch_size` (effective) | 128 | 3.2M tokens | 4M tokens |
| `learning_rate` | 3e-4 | 6e-5 | 1.5e-4 |
| `total_tokens` | ~65M | 300B | 2T |
| **Parameters** | **~85M** | **175B** | **70B** |

> 📖 **Key Insight**: Notice that larger models use SMALLER learning rates. The scaling law relationship is approximately $\text{lr} \propto \frac{1}{\sqrt{\text{params}}}$ — bigger models need gentler updates.
>
> Reference: Hoffmann, J. et al. (2022). *Training Compute-Optimal Large Language Models* (Chinchilla paper).

---

# 3️⃣ Complete Model (Combining All Components) 🧠⚡

> 🎨 **THE BIG PICTURE ANALOGY**: This is the **engine assembly**. You're taking individual parts (attention, normalization, feed-forward) and bolting them together into one powerful machine. Each class below is a sub-assembly, and the `MiniGPT` class at the bottom is the complete engine.

> 💡 **For absolute beginners**: Don't panic at the code length! Each piece does ONE simple thing. Together, they form a system that reads text and predicts the next word. That's it. That's GPT.

### 🏗️ Architecture Overview — How Data Flows Through Mini GPT

```
Input Text: "The cat sat on the"
        ↓
[Tokenizer] → [101, 2045, 2067, 2006, 1996]     ← Convert words to numbers
        ↓
[Token Embedding] → 5 vectors of size 512         ← Each number becomes a rich vector
        ↓
[Position Embedding] → Add position info           ← "I'm the 1st word, 2nd word..."
        ↓
[Dropout] → Regularization                         ← Random masking for robustness
        ↓
┌─────────────────────────────────────┐
│     Transformer Block × 8           │  ← The core reasoning engine
│  ┌───────────────────────────────┐  │
│  │ RMSNorm → Attention → +Residual│  │  ← "What should I focus on?"
│  └───────────────────────────────┘  │
│  ┌───────────────────────────────┐  │
│  │ RMSNorm → SwiGLU FFN → +Resid │  │  ← "What does this mean?"
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
        ↓
[Final RMSNorm] → Normalize one last time
        ↓
[Linear Head] → 50,257 scores (one per vocab word)
        ↓
[Softmax] → Probability: "mat" 45%, "rug" 20%, ...
        ↓
Output: "mat" (or sample from distribution)
```

### 🔧 Component-by-Component Breakdown

```python
# model.py
import torch
import torch.nn as nn
import torch.nn.functional as F
import math

class RMSNorm(nn.Module):
    """Root Mean Square Layer Normalization (LLaMA-style)."""
    def __init__(self, dim, eps=1e-6):
        super().__init__()
        self.weight = nn.Parameter(torch.ones(dim))
        self.eps = eps
    
    def forward(self, x):
        norm = torch.rsqrt(x.pow(2).mean(-1, keepdim=True) + self.eps)
        return x * norm * self.weight
```

#### 📏 RMSNorm — The "Stabilizer"

> 🎨 **Analogy**: Imagine you're mixing paint colors. Without normalization, one color (dimension) might dominate — turning everything red. RMSNorm ensures all colors are balanced, producing the shade you intended.

⭐ **Why RMSNorm instead of LayerNorm?** RMSNorm skips the mean-centering step, which makes it ~10-15% faster with nearly identical performance. LLaMA introduced this choice, and it's now standard.

The math behind RMSNorm:

$$\text{RMSNorm}(\mathbf{x}) = \frac{\mathbf{x}}{\text{RMS}(\mathbf{x})} \odot \boldsymbol{\gamma}$$

Where:

$$\text{RMS}(\mathbf{x}) = \sqrt{\frac{1}{d}\sum_{i=1}^{d} x_i^2 + \epsilon}$$

- $\mathbf{x}$ = input vector (one token's representation)
- $d$ = dimension (512 in our case)
- $\boldsymbol{\gamma}$ = learnable scale parameter
- $\epsilon$ = tiny number (1e-6) to prevent division by zero
- $\odot$ = element-wise multiplication

> 💡 **For beginners**: Think of it as "auto-brightness" on your phone camera. No matter how bright or dark the scene, it normalizes to a consistent exposure level.

```python
class SwiGLU(nn.Module):
    def __init__(self, d_model):
        super().__init__()
        d_ff = int(8 / 3 * d_model)
        d_ff = 256 * ((d_ff + 255) // 256)
        self.w_gate = nn.Linear(d_model, d_ff, bias=False)
        self.w_up = nn.Linear(d_model, d_ff, bias=False)
        self.w_down = nn.Linear(d_ff, d_model, bias=False)
    
    def forward(self, x):
        return self.w_down(F.silu(self.w_gate(x)) * self.w_up(x))
```

#### 🚀 SwiGLU — The "Thinking Engine"

> 🎨 **Analogy**: If attention is "what should I look at?", then the FFN is "what does this mean?". SwiGLU is a fancy version of the feed-forward network that uses a **gating mechanism** — like a bouncer at a club deciding which information gets through.

The SwiGLU formula:

$$\text{SwiGLU}(\mathbf{x}) = \mathbf{W}_{\text{down}} \cdot \left[\text{SiLU}(\mathbf{W}_{\text{gate}} \mathbf{x}) \odot (\mathbf{W}_{\text{up}} \mathbf{x})\right]$$

Where $\text{SiLU}(z) = z \cdot \sigma(z)$ and $\sigma$ is the sigmoid function.

**Why the weird dimension** $d_{\text{ff}} = \frac{8}{3}d$?
- Standard FFN uses $4d$ with 2 matrices = $8d^2$ parameters
- SwiGLU uses $\frac{8}{3}d$ with 3 matrices = $3 \times \frac{8}{3}d^2 = 8d^2$ parameters
- **Same parameter count, better performance!** 📈

> 📖 **Reference**: Shazeer, N. (2020). *GLU Variants Improve Transformer*. arXiv:2002.05202

The rounding to multiples of 256: `d_ff = 256 * ((d_ff + 255) // 256)` ensures the matrix dimensions are GPU-friendly (tensor cores work best with multiples of 8, 16, or 256).

```python
class Attention(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.num_heads = config.num_heads
        self.head_dim = config.d_model // config.num_heads
        
        self.qkv = nn.Linear(config.d_model, 3 * config.d_model, bias=config.bias)
        self.out = nn.Linear(config.d_model, config.d_model, bias=config.bias)
        self.attn_dropout = nn.Dropout(config.dropout)
        self.resid_dropout = nn.Dropout(config.dropout)
        
        mask = torch.tril(torch.ones(config.max_seq_len, config.max_seq_len))
        self.register_buffer("mask", mask.view(1, 1, config.max_seq_len, config.max_seq_len))
    
    def forward(self, x):
        B, T, C = x.shape
        q, k, v = self.qkv(x).chunk(3, dim=-1)
        
        q = q.view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        k = k.view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        v = v.view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        
        # Use scaled_dot_product_attention if available (Flash Attention)
        if hasattr(F, 'scaled_dot_product_attention'):
            out = F.scaled_dot_product_attention(q, k, v, is_causal=True, dropout_p=self.attn_dropout.p if self.training else 0.0)
        else:
            scores = torch.matmul(q, k.transpose(-2, -1)) / math.sqrt(self.head_dim)
            scores = scores.masked_fill(self.mask[:, :, :T, :T] == 0, float('-inf'))
            attn = self.attn_dropout(F.softmax(scores, dim=-1))
            out = torch.matmul(attn, v)
        
        out = out.transpose(1, 2).contiguous().view(B, T, C)
        return self.resid_dropout(self.out(out))
```

#### 👁️ Attention — The "Focus System"

> 🎨 **Analogy**: Imagine reading a mystery novel. When you encounter "The detective found **the weapon** that **the suspect** had hidden in **the garden**." Your brain automatically connects "weapon" with "suspect" and "garden" — attention does exactly this for the model!

⭐ **The Core Attention Formula**:

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right) V$$

Where:
- $Q$ = Query matrix — "What am I looking for?"
- $K$ = Key matrix — "What do I contain?"
- $V$ = Value matrix — "What information do I carry?"
- $d_k$ = dimension of each head = $\frac{d_{\text{model}}}{\text{num\_heads}} = \frac{512}{8} = 64$
- The $\sqrt{d_k}$ scaling prevents the dot products from getting too large (which would make softmax too "peaky")

**Multi-Head Attention** splits the 512-dim space into 8 heads of 64 dims each:

$$\text{MultiHead}(X) = \text{Concat}(\text{head}_1, \ldots, \text{head}_8) \mathbf{W}^O$$

> 💡 **Why multiple heads?** Each head can learn to focus on different things:
> - Head 1 might track subject-verb agreement
> - Head 2 might track co-reference ("he" → "John")
> - Head 3 might track positional patterns
> - Head 8 might track sentiment

**The Causal Mask** (`torch.tril`) is critical — it's a lower-triangular matrix that prevents the model from "cheating" by looking at future tokens:

$$\text{Mask} = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 1 & 1 & 0 & 0 \\ 1 & 1 & 1 & 0 \\ 1 & 1 & 1 & 1 \end{bmatrix}$$

Where 0 = "can't see" ($-\infty$ before softmax). Token 3 can see tokens 1, 2, 3 but NOT token 4.

⭐ **Flash Attention**: The code checks for `scaled_dot_product_attention` — this is PyTorch's built-in Flash Attention, which is 2-4× faster and uses $O(N)$ memory instead of $O(N^2)$.

> 📖 **Reference**: Dao, T. et al. (2022). *FlashAttention: Fast and Memory-Efficient Exact Attention*. NeurIPS.

```python
class TransformerBlock(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.norm1 = RMSNorm(config.d_model)
        self.attn = Attention(config)
        self.norm2 = RMSNorm(config.d_model)
        self.ffn = SwiGLU(config.d_model)
    
    def forward(self, x):
        x = x + self.attn(self.norm1(x))
        x = x + self.ffn(self.norm2(x))
        return x
```

#### 🧱 Transformer Block — The "Repeating Unit"

> 🎨 **Analogy**: A transformer block is like a **floor in a building**. Each floor has:
> 1. A **meeting room** (attention) — people discuss and share information
> 2. A **processing office** (FFN) — each person goes back and thinks deeply about what they heard
>
> Our model has 8 floors (layers). Information enters at the ground floor and gets refined as it goes up!

The key pattern is **Pre-Norm + Residual**:

$$\mathbf{x} = \mathbf{x} + \text{Attention}(\text{RMSNorm}(\mathbf{x}))$$
$$\mathbf{x} = \mathbf{x} + \text{FFN}(\text{RMSNorm}(\mathbf{x}))$$

⭐ **Why residual connections (`x + ...`)?** Without them, gradients vanish as they pass through many layers. The residual creates a "highway" for gradients to flow directly back:

$$\frac{\partial \mathcal{L}}{\partial \mathbf{x}_\ell} = \frac{\partial \mathcal{L}}{\partial \mathbf{x}_L} \prod_{i=\ell}^{L-1} \left(1 + \frac{\partial f_i}{\partial \mathbf{x}_i}\right)$$

The "1 +" term means the gradient is always at least 1 — it never vanishes! 🎉

```python
class MiniGPT(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.config = config
        
        self.tok_emb = nn.Embedding(config.vocab_size, config.d_model)
        self.pos_emb = nn.Embedding(config.max_seq_len, config.d_model)
        self.drop = nn.Dropout(config.dropout)
        
        self.blocks = nn.ModuleList([TransformerBlock(config) for _ in range(config.num_layers)])
        self.norm = RMSNorm(config.d_model)
        self.head = nn.Linear(config.d_model, config.vocab_size, bias=False)
        
        # Weight tying
        self.head.weight = self.tok_emb.weight
        
        self.apply(self._init_weights)
        # Scale residual projections
        for block in self.blocks:
            nn.init.normal_(block.attn.out.weight, std=0.02/math.sqrt(2*config.num_layers))
            nn.init.normal_(block.ffn.w_down.weight, std=0.02/math.sqrt(2*config.num_layers))
    
    def _init_weights(self, module):
        if isinstance(module, nn.Linear):
            nn.init.normal_(module.weight, std=0.02)
        elif isinstance(module, nn.Embedding):
            nn.init.normal_(module.weight, std=0.02)
    
    def forward(self, idx, targets=None):
        B, T = idx.shape
        tok = self.tok_emb(idx)
        pos = self.pos_emb(torch.arange(T, device=idx.device))
        x = self.drop(tok + pos)
        
        for block in self.blocks:
            x = block(x)
        
        x = self.norm(x)
        logits = self.head(x)
        
        loss = None
        if targets is not None:
            loss = F.cross_entropy(logits.view(-1, logits.size(-1)), targets.view(-1), ignore_index=-100)
        
        return logits, loss
    
    @torch.no_grad()
    def generate(self, idx, max_new_tokens, temperature=1.0, top_k=50, top_p=0.9):
        for _ in range(max_new_tokens):
            idx_cond = idx[:, -self.config.max_seq_len:]
            logits, _ = self(idx_cond)
            logits = logits[:, -1, :] / max(temperature, 1e-5)
            
            if top_k > 0:
                v, _ = torch.topk(logits, min(top_k, logits.size(-1)))
                logits[logits < v[:, [-1]]] = float('-inf')
            
            if top_p < 1.0:
                sorted_logits, sorted_indices = torch.sort(logits, descending=True)
                cumprobs = torch.cumsum(F.softmax(sorted_logits, dim=-1), dim=-1)
                mask = (cumprobs - F.softmax(sorted_logits, dim=-1)) > top_p
                sorted_logits[mask] = float('-inf')
                logits = logits.scatter(-1, sorted_indices, sorted_logits)
            
            probs = F.softmax(logits, dim=-1)
            next_tok = torch.multinomial(probs, num_samples=1)
            idx = torch.cat([idx, next_tok], dim=-1)
        
        return idx
```

#### 🏛️ MiniGPT — The Complete Architecture

> 🎨 **Analogy**: This is the **fully assembled car**. All the sub-assemblies (wheels, engine, transmission) are connected and it's ready to drive!

⭐ **Three Critical Design Decisions Explained**:

**1. Weight Tying** (`self.head.weight = self.tok_emb.weight`):
The input embedding and output prediction layer share the SAME weights. This means the model's understanding of "what a word means" (embedding) is the same as its understanding of "what word should come next" (prediction).

> 💡 **Analogy**: It's like using the same dictionary to both READ words (input) and WRITE words (output). You don't need two separate dictionaries!
>
> **Saves**: $V \times d = 50{,}257 \times 512 \approx 25.7M$ parameters — that's ~30% of the total model!
>
> 📖 **Reference**: Press, O. & Wolf, L. (2017). *Using the Output Embedding to Improve Language Models*. EACL.

**2. Weight Initialization** (scaled by $\frac{0.02}{\sqrt{2L}}$):
Residual stream accumulates contributions from all layers. Without scaling, the variance grows linearly with depth:

$$\text{Var}[\mathbf{x}_L] = \text{Var}[\mathbf{x}_0] + \sum_{\ell=1}^{L} \text{Var}[f_\ell(\mathbf{x}_\ell)]$$

Scaling residual projections by $\frac{1}{\sqrt{2L}}$ (where the 2 accounts for both attention and FFN residuals) keeps the variance stable.

> 📖 **Reference**: GPT-2 paper (Radford et al., 2019) introduced this initialization scheme.

**3. The Generate Method — How Text is Born**:
Generation is an **autoregressive loop** — the model predicts one token, appends it, and predicts the next:

```
Input:  "The cat"
Step 1: Predict "sat" → "The cat sat"
Step 2: Predict "on" → "The cat sat on"
Step 3: Predict "the" → "The cat sat on the"
Step 4: Predict "mat" → "The cat sat on the mat"
```

The **temperature**, **top-k**, and **top-p** parameters control the creativity:

| Parameter | Low Value | High Value | Analogy |
|:---|:---|:---|:---|
| `temperature` | Conservative, predictable | Creative, wild | Dial from "textbook answer" to "jazz improvisation" 🎷 |
| `top_k` | Only consider top few words | Consider many words | Restaurant menu: "top 3 dishes" vs "everything" |
| `top_p` | Only high-probability words | Include unlikely words | Picking from the "90th percentile" vs "anything goes" |

The mathematical relationship:

$$P(\text{token}_i) = \frac{\exp(\text{logit}_i / \tau)}{\sum_j \exp(\text{logit}_j / \tau)}$$

Where $\tau$ is temperature. As $\tau \to 0$, the distribution becomes a **spike** (argmax/greedy). As $\tau \to \infty$, it becomes **uniform** (random).

---

# 4️⃣ Training Script 🏋️‍♂️🔥

> 🎨 **Analogy**: This is the **driving school**. You've built the car (model). Now you need to teach it to drive (learn language). The training script is the instructor — it shows the model millions of examples and adjusts the engine after every lap.

> 💡 **For absolute beginners**: Training a neural network is like teaching a child to complete sentences. You show them "The cat sat on the ___" and they guess "car". Wrong! The correct answer is "mat". You tell them the correction, and next time they guess better. Repeat this millions of times, and eventually they become fluent.

### 🔄 The Training Loop — Step by Step

```
For each batch of text:
  1. 📥 Load a batch of token sequences (x) and their targets (y)
     x = "The cat sat on"  →  y = "cat sat on the"  (shifted by 1!)
  
  2. 🧮 Forward pass: model predicts next tokens
     predictions = model(x)  →  probabilities for each position
  
  3. 📉 Compute loss: how wrong were the predictions?
     loss = cross_entropy(predictions, y)
  
  4. ⬅️ Backward pass: compute gradients
     loss.backward()  →  "which knobs should turn and how much?"
  
  5. ✂️ Clip gradients: prevent explosive updates
     clip_grad_norm_(model.parameters(), 1.0)
  
  6. 🔧 Update weights: turn those knobs!
     optimizer.step()  →  parameters get adjusted
  
  7. 📊 Log & evaluate: check progress
     print(loss, perplexity, sample_generations)
  
  Repeat 10,000 times! 🔁
```

```python
# train.py
def main():
    # Config
    model_config = ModelConfig()
    train_config = TrainConfig()
    
    print(f"Model: ~{model_config.num_params/1e6:.1f}M parameters")
    
    # Data
    tokenizer = AutoTokenizer.from_pretrained("gpt2")
    train_ds = PreTokenizedDataset("data/train.bin", model_config.max_seq_len)
    val_ds = PreTokenizedDataset("data/val.bin", model_config.max_seq_len)
    
    train_loader = DataLoader(train_ds, batch_size=train_config.batch_size, shuffle=True, pin_memory=True)
    val_loader = DataLoader(val_ds, batch_size=train_config.batch_size, pin_memory=True)
    
    # Model
    model = MiniGPT(model_config).cuda()
    
    # Optimizer & Scheduler
    optimizer = torch.optim.AdamW(get_param_groups(model), lr=train_config.learning_rate, betas=train_config.betas)
    scheduler = CosineScheduleWithWarmup(optimizer, train_config.warmup_steps, train_config.total_steps)
    scaler = torch.cuda.amp.GradScaler(enabled=train_config.mixed_precision)
    
    # Train
    step = 0
    for epoch in range(100):  # Will break at total_steps
        for x, y in train_loader:
            x, y = x.cuda(), y.cuda()
            
            with torch.cuda.amp.autocast(enabled=train_config.mixed_precision):
                _, loss = model(x, y)
                loss = loss / train_config.gradient_accumulation_steps
            
            scaler.scale(loss).backward()
            
            if (step + 1) % train_config.gradient_accumulation_steps == 0:
                scaler.unscale_(optimizer)
                torch.nn.utils.clip_grad_norm_(model.parameters(), train_config.max_grad_norm)
                scaler.step(optimizer)
                scaler.update()
                scheduler.step()
                optimizer.zero_grad(set_to_none=True)
            
            step += 1
            
            if step % train_config.log_interval == 0:
                print(f"Step {step} | Loss {loss.item()*train_config.gradient_accumulation_steps:.4f} | LR {scheduler.get_lr():.2e}")
            
            if step % train_config.eval_interval == 0:
                val_loss = evaluate(model, val_loader)
                print(f"  Val Loss: {val_loss:.4f} | Val PPL: {math.exp(val_loss):.2f}")
                generate_samples(model, tokenizer)
            
            if step >= train_config.total_steps:
                break
        
        if step >= train_config.total_steps:
            break
    
    # Save final model
    torch.save({'model': model.state_dict(), 'config': model_config}, 'mini_gpt_final.pt')
    print("Training complete!")

if __name__ == "__main__":
    main()
```

### 🔍 Training Script Deep Dive — What Every Line Does

#### 📦 Data Pipeline: Text → Tokens → Batches

The training data is **pre-tokenized** and stored as raw binary files (`.bin`). This avoids tokenizing on-the-fly during training:

```
Raw text: "The cat sat on the mat. The dog ran in the park."
    ↓ (tokenize once, save to disk)
Tokens:   [464, 3797, 3332, 319, 262, 2603, 13, 464, 3290, 4966, ...]
    ↓ (pack into sequences of length 512)
Batches:  [batch_size × seq_len] tensors ready for GPU
```

> 💡 **For beginners**: `pin_memory=True` tells PyTorch to use "page-locked" memory for faster CPU→GPU transfer. It's like having a VIP lane at the airport — data gets to the GPU faster.

#### ⚡ Mixed Precision — Training at Half the Cost

⭐ **Mixed precision** (`torch.cuda.amp.autocast`) uses 16-bit floating point (FP16) for the forward pass instead of 32-bit (FP32):

| Precision | Bits | Range | Memory | Speed |
|:---|:---|:---|:---|:---|
| FP32 | 32 | $\pm 3.4 \times 10^{38}$ | Baseline | Baseline |
| FP16 | 16 | $\pm 6.5 \times 10^{4}$ | **50% less** | **~2× faster** |
| BF16 | 16 | $\pm 3.4 \times 10^{38}$ | **50% less** | **~2× faster** |

The `GradScaler` handles the tricky part — it scales the loss UP before backward (to prevent tiny FP16 gradients from becoming zero), then scales gradients DOWN before the optimizer step.

$$\text{scaled\_loss} = \text{loss} \times \text{scale\_factor}$$
$$\text{gradients} = \nabla(\text{scaled\_loss}) = \text{scale\_factor} \times \nabla(\text{loss})$$
$$\text{unscaled\_gradients} = \frac{\text{gradients}}{\text{scale\_factor}} = \nabla(\text{loss})$$

#### 🔄 Gradient Accumulation — Faking a Bigger GPU

With `gradient_accumulation_steps = 4`, the effective batch size is:

$$\text{effective\_batch} = \text{batch\_size} \times \text{accum\_steps} = 32 \times 4 = 128$$

> 🎨 **Analogy**: Imagine you can only carry 32 groceries per trip. With gradient accumulation, you make 4 trips and then cook with all 128 groceries at once. The recipe (model update) sees a bigger "meal" without needing a bigger car (GPU memory)!

The loss is divided by `gradient_accumulation_steps` to average correctly:

$$\mathcal{L}_{\text{total}} = \frac{1}{K}\sum_{k=1}^{K} \mathcal{L}_k$$

#### 📊 What Good Training Looks Like

Here's what you should expect to see during training:

| Step | Loss | Perplexity | What's Happening |
|:---|:---|:---|:---|
| 0 | ~10.8 | ~50,000 | Random model — predicting uniformly over vocab |
| 100 | ~7.0 | ~1,100 | Learning common words ("the", "a", "is") |
| 500 | ~5.0 | ~150 | Learning basic grammar |
| 1,000 | ~4.0 | ~55 | Learning phrases and common patterns |
| 5,000 | ~3.2 | ~25 | Writing coherent sentences |
| 10,000 | ~2.8 | ~16 | Writing coherent paragraphs |

> 💡 **Perplexity** = $e^{\text{loss}}$. It tells you "how many equally likely words the model is choosing between." A perplexity of 16 means the model has narrowed its guesses to about 16 plausible next words (from 50,257!). Human-level text has perplexity around 10-20 on typical benchmarks.

#### 🚨 Common Training Failures & Fixes

| Symptom | Cause | Fix |
|:---|:---|:---|
| Loss = NaN 💀 | Learning rate too high or numerical instability | Lower LR, check for inf values, ensure grad clipping |
| Loss plateau at ~4.5 | Model capacity too small or LR schedule issue | Increase model size, check warmup, increase data |
| Loss spikes randomly | Bad data batches or gradient explosion | Better data filtering, lower grad clip norm |
| Val loss increasing while train loss decreasing | Overfitting! 🎭 | More data, higher dropout, weight decay, early stopping |
| Loss decreasing but generations are garbage | Temperature/sampling issue OR not enough training | Check generation params, train longer |

### 🏭 Production Training: What Changes at Scale

⭐ **Going from our mini GPT to GPT-3/4 scale, here's what changes**:

| Aspect | Our Mini GPT | Production (GPT-3 scale) |
|:---|:---|:---|
| **Hardware** | 1× consumer GPU (16GB) | 256-1024× A100/H100 GPUs |
| **Parallelism** | None | Data + Tensor + Pipeline parallel |
| **Training time** | Hours | Weeks to months |
| **Data size** | ~100MB | 300B+ tokens (~1TB+) |
| **Checkpointing** | Simple `torch.save` | Distributed checkpoint, async saves |
| **Monitoring** | Print statements | W&B dashboards, PagerDuty alerts |
| **Cost** | ~$5 in cloud compute | $4.6M (GPT-3) to $100M+ (GPT-4) |
| **Data pipeline** | Single file | Petabyte-scale with quality filters |
| **Failure handling** | Restart from scratch | Auto-resume from checkpoint |

> 📖 **References**:
> - Narayanan, D. et al. (2021). *Efficient Large-Scale Language Model Training on GPU Clusters Using Megatron-LM*. SC'21.
> - Rajbhandari, S. et al. (2020). *ZeRO: Memory Optimizations Toward Training Trillion Parameter Models*. SC'20.

---

# 5️⃣ Project Milestones & Checklist 📋✅

> 🎨 **Analogy**: This is your **car assembly instruction manual**. Each phase is a station on the assembly line. You don't jump to painting before the chassis is built!

> 💡 **For beginners**: Don't worry if you didn't complete everything perfectly. The goal is understanding — you can always come back and strengthen weak areas.

```
Phase 1: Setup (Day 41-43) 🔧
  ✓ Tokenizer: understand BPE, use GPT-2 tokenizer
  ✓ Vocabulary: special tokens, chat template
  ✓ Data: download, tokenize, save as binary

Phase 2: Architecture (Day 44-47) 🏗️
  ✓ Positional encoding (learned positions)
  ✓ Causal self-attention with mask
  ✓ SwiGLU feed-forward network
  ✓ RMSNorm normalization
  ✓ Residual connections
  ✓ Complete GPT model

Phase 3: Training (Day 48-55) 🏋️
  ✓ Cross-entropy loss with label shifting
  ✓ Data pipeline (packed dataset, DataLoader)
  ✓ Cosine LR schedule with warmup
  ✓ Training loop with gradient accumulation
  ✓ Mixed precision training
  ✓ Gradient clipping
  ✓ Checkpointing & monitoring

Phase 4: Generation & Eval (Day 52-53) 📊
  ✓ Greedy / Temperature / Top-k / Top-p sampling
  ✓ Perplexity evaluation
  ✓ Generation quality assessment

Phase 5: Fine-tuning & Alignment (Day 56-59) 🎯
  ✓ LoRA implementation
  ✓ Instruction fine-tuning
  ✓ DPO preference optimization
```

### 🗺️ Visual Journey Map

| Phase | What You Built | Car Part 🚗 | Difficulty | Key Concept |
|:---|:---|:---|:---|:---|
| Phase 1 | Tokenizer + Data | Fuel system | ⭐⭐ | BPE, byte-pair encoding |
| Phase 2 | Complete Architecture | Engine + Chassis | ⭐⭐⭐⭐ | Attention, residuals, norms |
| Phase 3 | Training Pipeline | Test Track | ⭐⭐⭐ | Optimization, schedules |
| Phase 4 | Generation + Eval | Dashboard + Gauges | ⭐⭐ | Sampling, perplexity |
| Phase 5 | Fine-tuning | Custom Mods | ⭐⭐⭐ | LoRA, DPO |

### ⭐ Self-Assessment Checklist

Rate yourself honestly (1-5) on each skill:

| Skill | Can You... | 1️⃣ | 2️⃣ | 3️⃣ | 4️⃣ | 5️⃣ |
|:---|:---|:---|:---|:---|:---|:---|
| **Tokenization** | Explain what BPE does and why it's needed? | ☐ | ☐ | ☐ | ☐ | ☐ |
| **Embeddings** | Explain why words become vectors? | ☐ | ☐ | ☐ | ☐ | ☐ |
| **Attention** | Write attention from scratch without looking at code? | ☐ | ☐ | ☐ | ☐ | ☐ |
| **Architecture** | Explain why pre-norm + residuals are used? | ☐ | ☐ | ☐ | ☐ | ☐ |
| **Training** | Debug a training run that's not converging? | ☐ | ☐ | ☐ | ☐ | ☐ |
| **Generation** | Explain the difference between top-k and top-p? | ☐ | ☐ | ☐ | ☐ | ☐ |
| **Fine-tuning** | Implement LoRA and explain why it works? | ☐ | ☐ | ☐ | ☐ | ☐ |

> 🎯 **Target**: 3+ on everything, 4+ on attention and training. If you're below 3 on any skill, revisit that day before moving forward.

---

# 6️⃣ What You've Learned 🎓🏆

> 🎨 **Analogy**: You didn't just learn to drive — you learned to BUILD the car, TUNE the engine, TEST it on the track, and CUSTOMIZE it for different terrains. You're not just a driver — you're a **full-stack automotive engineer**! 🏗️🚗

By completing Days 41-60, you now understand:

1. **How text becomes numbers** (tokenization, BPE, vocabulary design) 📝→🔢
2. **How transformers process sequences** (attention, FFN, residuals) 🧠
3. **How models learn language** (cross-entropy, perplexity, scaling laws) 📈
4. **How to train efficiently** (schedule, AMP, gradient accumulation) ⚡
5. **How to generate text** (sampling strategies, repetition control) ✍️
6. **How to adapt models** (LoRA, instruction tuning, DPO alignment) 🎯

This is the foundation for everything that follows — diffusion models, systems engineering, deployment, and research.

### 🌟 The "Aha!" Moments You Should Have Had

| # | Insight | Why It's Profound |
|:---|:---|:---|
| 1 | GPT is "just" next-token prediction | The simplest objective produces the most capable models — emergent abilities arise from scale |
| 2 | Attention is a soft database lookup | Query-Key-Value is literally: "search for relevant info and retrieve it" |
| 3 | Residual connections are essential | Without them, deep networks can't learn — gradients vanish |
| 4 | Weight tying saves 30% of parameters | The embedding and prediction share the same understanding of words |
| 5 | The learning rate matters more than architecture | A bad LR can ruin even the best model; a good LR can save a mediocre one |
| 6 | LoRA proves most parameters are redundant | You can adapt a model by changing <1% of its weights |
| 7 | Alignment is about steering, not capability | The base model already "knows" things — RLHF/DPO just teaches it to express knowledge helpfully |

### 🧪 The Knowledge Stack You've Built

⭐ Think of your knowledge as a **technology stack** — each layer builds on the one below:

```
┌─────────────────────────────────────────────┐
│  🎯 ALIGNMENT (DPO, RLHF)                   │  ← Making models helpful & safe
├─────────────────────────────────────────────┤
│  🔧 FINE-TUNING (LoRA, Instruction Tuning)  │  ← Adapting for specific tasks
├─────────────────────────────────────────────┤
│  ✍️ GENERATION (Top-k, Top-p, Temperature)  │  ← Producing human-like text
├─────────────────────────────────────────────┤
│  📊 EVALUATION (Perplexity, Loss Curves)    │  ← Measuring quality
├─────────────────────────────────────────────┤
│  🏋️ TRAINING (AdamW, LR Schedule, AMP)     │  ← Teaching the model
├─────────────────────────────────────────────┤
│  🧠 ARCHITECTURE (Attention + FFN + Norms)  │  ← The model itself
├─────────────────────────────────────────────┤
│  📝 TOKENIZATION (BPE, Vocabulary)          │  ← Text → Numbers
├─────────────────────────────────────────────┤
│  📐 MATH (Linear Algebra, Calculus, Prob)    │  ← The foundation of it all
└─────────────────────────────────────────────┘
```

### 🏭 How This Relates to Real GPT Production

⭐ **Your mini GPT shares >90% of its design with real production models.** Here's what's identical and what differs:

| Component | Our Mini GPT | GPT-4 / Claude / LLaMA | Same? |
|:---|:---|:---|:---|
| Decoder-only transformer | ✅ | ✅ | ✅ Identical concept |
| Self-attention + causal mask | ✅ | ✅ | ✅ Identical |
| SwiGLU FFN | ✅ | ✅ | ✅ Identical |
| RMSNorm | ✅ | ✅ | ✅ Identical |
| Weight tying | ✅ | Sometimes | ⚠️ Varies |
| Rotary Position Embeddings (RoPE) | ❌ Learned | ✅ RoPE | ❌ We use simpler version |
| Grouped Query Attention (GQA) | ❌ Full MHA | ✅ GQA | ❌ We use more memory |
| Distributed training | ❌ Single GPU | ✅ 1000s of GPUs | ❌ Scale difference |
| Data quality pipeline | ❌ Basic | ✅ Massive filtering | ❌ Data is the moat |
| RLHF/Constitutional AI | ❌ | ✅ | ❌ Alignment infrastructure |

> 💡 **Key takeaway**: You understand the CORE of how every modern LLM works. The differences are engineering optimizations, not fundamental conceptual changes.

### 🚀 Deployment Considerations — From Notebook to Production

If you wanted to deploy your mini GPT as an actual service:

#### Step 1: Quantization (Shrink the Model)

| Method | Size Reduction | Speed Impact | Quality Loss |
|:---|:---|:---|:---|
| FP32 (baseline) | 340MB | Baseline | None |
| FP16 | 170MB (50%) | ~2× faster | Negligible |
| INT8 | 85MB (75%) | ~3× faster | Minor |
| INT4 (GPTQ/AWQ) | 42MB (88%) | ~4× faster | Noticeable on hard tasks |

#### Step 2: Serving Infrastructure

```
User Request → API Gateway → Load Balancer → GPU Server
                                                ↓
                                          [KV Cache] → Stream tokens back
                                                ↓
                                          Token → Detokenize → Response
```

⭐ **KV Cache** is the most important serving optimization — it stores the Key and Value tensors from previous tokens so you don't recompute them. Without it, generating 100 tokens would require 100× more compute!

#### Step 3: API Design

```python
# What a production API might look like
POST /v1/completions
{
    "prompt": "The meaning of life is",
    "max_tokens": 100,
    "temperature": 0.7,
    "top_p": 0.9,
    "stream": true  # Send tokens as they're generated
}
```

> 📖 **References for deployment**:
> - Kwon, W. et al. (2023). *Efficient Memory Management for Large Language Model Serving with PagedAttention* (vLLM). SOSP.
> - Frantar, E. et al. (2023). *GPTQ: Accurate Post-Training Quantization for Generative Pre-trained Transformers*. ICLR.
> - Lin, J. et al. (2024). *AWQ: Activation-aware Weight Quantization for LLM Compression*. MLSys.

---

# 📝 Summary & Next Phase

| Phase | Mastered |
|-------|---------|
| Tokenization | BPE, byte-level, vocabulary design |
| Architecture | Decoder-only transformer, all components |
| Training | Complete pipeline: data → training → eval |
| Generation | All sampling strategies |
| Fine-tuning | LoRA, QLoRA, instruction tuning, DPO |

**Next Phase (Days 61-75)**: Diffusion Models — a completely different approach to generative AI, using noise and denoising to create images and other modalities.

---

# 🧾 Mini GPT Cheat Sheet — Everything on One Page

⭐ **Print this. Pin it to your wall. This is your quick reference for building LLMs from scratch.**

## 📐 Architecture at a Glance

| Component | Formula | Purpose |
|:---|:---|:---|
| Token Embedding | $\mathbf{e} = \text{Embed}(\text{token\_id})$ | Convert integer → vector |
| Position Embedding | $\mathbf{p} = \text{PosEmbed}(\text{position})$ | Add position information |
| Input | $\mathbf{x} = \text{Dropout}(\mathbf{e} + \mathbf{p})$ | Combine token + position |
| RMSNorm | $\hat{\mathbf{x}} = \frac{\mathbf{x}}{\text{RMS}(\mathbf{x})} \odot \gamma$ | Stabilize activations |
| Attention | $\text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right) V$ | Focus on relevant tokens |
| SwiGLU | $W_{\text{down}}[\text{SiLU}(W_{\text{gate}}\mathbf{x}) \odot W_{\text{up}}\mathbf{x}]$ | Non-linear transformation |
| Residual | $\mathbf{x} = \mathbf{x} + f(\mathbf{x})$ | Enable deep networks |
| Output Head | $\text{logits} = \mathbf{x} W_{\text{emb}}^\top$ | Predict next token (weight-tied) |
| Loss | $\mathcal{L} = -\frac{1}{N}\sum \log P(y_i \mid y_{<i})$ | Cross-entropy over vocabulary |

## ⚡ Training Cheat Sheet

| Setting | Value | Rule of Thumb |
|:---|:---|:---|
| Learning Rate | $3 \times 10^{-4}$ | Karpathy's constant for Adam |
| Weight Decay | $0.1$ | Standard for transformers |
| Warmup | 5% of total steps | Prevents early instability |
| LR Schedule | Cosine decay to 10% | Smooth annealing |
| Batch Size | As large as GPU allows | More = smoother gradients |
| Gradient Clip | $1.0$ | Safety net for exploding gradients |
| Optimizer | AdamW with $\beta = (0.9, 0.95)$ | Gold standard for transformers |

## 🎛️ Generation Cheat Sheet

| Parameter | Conservative | Balanced | Creative |
|:---|:---|:---|:---|
| Temperature | 0.3 | 0.7 | 1.2 |
| Top-k | 10 | 50 | 200 |
| Top-p | 0.5 | 0.9 | 0.95 |
| Use case | Code, factual Q&A | General chat | Fiction, brainstorming |

## 🔑 Key Numbers to Remember

| Fact | Number |
|:---|:---|
| Our model size | ~85M parameters |
| GPT-2 size | 1.5B parameters |
| GPT-3 size | 175B parameters |
| Params per transformer block | $\approx 12d^2$ |
| SwiGLU hidden dim | $\frac{8}{3}d$ (rounded to 256) |
| Head dimension | $d_{\text{model}} / \text{num\_heads}$ |
| Random model perplexity | $\approx V$ (50,257) |
| Good model perplexity | 10-20 on standard benchmarks |

## 📚 Essential Papers Referenced

| Paper | Authors | Year | Key Contribution |
|:---|:---|:---|:---|
| *Attention Is All You Need* | Vaswani et al. | 2017 | The transformer architecture |
| *Language Models are Unsupervised Multitask Learners* | Radford et al. | 2019 | GPT-2 — scaling + zero-shot |
| *Language Models are Few-Shot Learners* | Brown et al. | 2020 | GPT-3 — in-context learning |
| *GLU Variants Improve Transformer* | Shazeer | 2020 | SwiGLU activation |
| *Training Compute-Optimal LLMs* | Hoffmann et al. | 2022 | Chinchilla scaling laws |
| *FlashAttention* | Dao et al. | 2022 | Memory-efficient attention |
| *LoRA* | Hu et al. | 2022 | Parameter-efficient fine-tuning |
| *LLaMA* | Touvron et al. | 2023 | Open-source LLM design |
| *nanoGPT* | Karpathy | 2023 | Build GPT from scratch |
| *DPO* | Rafailov et al. | 2023 | Direct preference optimization |

---

# 🔬 Deep Research: Lessons from Building LLMs from Scratch

## What Karpathy's nanoGPT Taught the Community

⭐ **nanoGPT** proved that you don't need a massive team or infrastructure to understand LLMs. Key lessons:

1. **Simplicity wins**: The core GPT architecture is ~300 lines of Python. Everything else is engineering.
2. **Data quality > model size**: A well-curated dataset on a small model beats a large model on noisy data.
3. **Reproducibility matters**: Every hyperparameter decision should be justified by ablations or prior work.
4. **Understanding > usage**: Using an API is not the same as understanding the system. Building from scratch gives you intuition that no amount of API calls can provide.

## The Scaling Journey: From Mini GPT to Production

```
Mini GPT     →    nanoGPT    →    GPT-2     →    GPT-3     →    GPT-4
 ~85M params      ~124M          1.5B           175B          ~1.8T(?)
 Your laptop      1× A100       32× TPUv3      256× A100     ~25K A100s
 Hours            45 min         ~1 week        ~3 months     ~6 months
 ~$5              ~$20           ~$50K          ~$4.6M        ~$100M+
```

> 📖 **Reference**: Epoch AI (2023). *Trends in Machine Learning Hardware and Compute*. epochai.org

## What Would You Change for a "Real" Model?

| Decision in Mini GPT | What Production Does Instead | Why |
|:---|:---|:---|
| Learned position embeddings | RoPE (Rotary Position Embeddings) | Extrapolates to longer sequences |
| Full multi-head attention | Grouped Query Attention (GQA) | 2-4× faster inference, less KV cache |
| Single GPU training | FSDP / Megatron parallelism | Can't fit 70B+ on one GPU |
| GPT-2 tokenizer | Custom BPE (e.g., LLaMA tokenizer) | Better coverage, multilingual support |
| Simple data loading | Streaming + dynamic batching | Petabyte datasets don't fit in RAM |
| Cross-entropy only | Multi-task (code, math, reasoning) | Diverse training improves generalization |
| No safety filters | Red-teaming + safety classifiers | Required for deployment |

---

# 💎 Final Reflection — You Built a Language Model From Scratch!

> 🎉 **Congratulations!** Most people who "work with AI" have never looked inside the black box. You didn't just open it — you built one from the ground up, bolt by bolt.

Here's what sets you apart now:

```
🏆 You can explain HOW GPT works at every level of abstraction
🏆 You can implement attention, normalization, and FFN from memory
🏆 You can train a model and debug when things go wrong
🏆 You can generate text and control the output distribution
🏆 You can fine-tune with LoRA and align with DPO
🏆 You understand the gap between mini and production scale
```

> *"Any sufficiently advanced technology is indistinguishable from magic — unless you've built it yourself."*  
> — Arthur C. Clarke (paraphrased for ML engineers) 🧙‍♂️
