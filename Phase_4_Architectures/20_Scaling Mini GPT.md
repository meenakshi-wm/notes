📘 DAY 20 — Scaling Mini GPT: Training on Real Data 🚀🏗️

> **"The bitter lesson is that general methods that leverage computation are ultimately the most effective."** — Rich Sutton, 2019

Goal:

Put everything together and actually TRAIN a mini GPT model on real text data. Go through the complete pipeline: prepare data, configure the model, train, evaluate, generate text, and analyze the results. Understand scaling behavior — how performance improves with more data, compute, and parameters.

---

## 🎯 Why This Day Matters — The Big Picture for Beginners

Imagine you're building a **factory that manufactures sentences** 🏭. So far, you've designed the blueprints (architecture), built the machines (attention heads, feed-forward layers), and tested individual parts. **Today, you turn the factory ON and actually produce text.**

Scaling is about answering: *"If I build a BIGGER factory, how much better will my sentences be?"* Remarkably, researchers discovered that this question has a **precise mathematical answer** — called **scaling laws** — that let you predict performance *before spending millions of dollars on training*.

### 🧭 What You'll Learn (Beginner Roadmap)

| # | Topic | Beginner Analogy | Why It Matters |
|---|-------|-------------------|----------------|
| 1️⃣ | Training Pipeline | Assembly line setup | You need data flowing through the model |
| 2️⃣ | Model Configurations | Factory sizes (small/medium/large) | Different sizes for different budgets |
| 3️⃣ | Training Script | Running the factory | The actual learning process |
| 4️⃣ | Scaling Laws | "How much better if we go bigger?" formulas | Predict performance before training |
| 5️⃣ | Training Tips | Operator's manual for each factory size | Practical know-how |
| 6️⃣ | Diagnostics | Factory dashboard and alarms | Catch problems early |

### ⭐ Key Vocabulary for Absolute Beginners

| Term | Plain English | Analogy |
|------|--------------|---------|
| **Token** | A word-piece (word or part of a word) | A single LEGO brick 🧱 |
| **Parameters** | The model's adjustable knobs | Dials on a mixing board 🎛️ |
| **Loss** | How wrong the model's predictions are | Score on a test (lower = better) 📉 |
| **Perplexity** | How "surprised" the model is by text | Confusion level — lower = smarter 🤔 |
| **Epoch** | One full pass through the training data | Reading the textbook once cover-to-cover 📖 |
| **Batch** | A chunk of examples processed together | A tray of cookies in the oven 🍪 |
| **Learning Rate** | How big each adjustment step is | How far you turn each knob per try |
| **FLOPs** | Floating-point operations (math steps) | Counting bricks needed to build a wall 🧮 |

---

# 1️⃣ Setting Up the Training Pipeline 🔧

> 🏭 **Beginner Analogy**: Think of this as setting up an assembly line. Raw materials (text) come in one end, get processed into machine-readable pieces (tokens), and are sorted into training batches. Without a solid pipeline, even the best model design is useless — garbage in, garbage out!

### ⭐ What's Happening Here (Plain English)

1. **Tokenization** = Chopping text into numbered pieces the model understands
2. **Train/Val Split** = Keeping 10% of data aside to test if the model actually learned (not just memorized)
3. **Binary Storage** = Saving tokens as raw numbers for blazing-fast loading

### 🏢 Production Insight: How OpenAI & Meta Do It

| Company | Dataset | Tokens | Tokenizer | Storage Format |
|---------|---------|--------|-----------|---------------|
| OpenAI (GPT-3) | WebText, Books, Wikipedia | ~300B | BPE (50,257 vocab) | Shuffled binary shards |
| Meta (LLaMA) | CommonCrawl, C4, GitHub, ArXiv | 1.4T | SentencePiece (32K vocab) | Memory-mapped binary |
| Google (Chinchilla) | MassiveText | 1.4T | SentencePiece (32K vocab) | TFRecord shards |
| Mistral (7B) | Undisclosed web crawl | ~2T+ | BPE (32K vocab) | Distributed binary |

> 📖 **Research Note**: Touvron et al. (2023) showed that LLaMA-13B trained on 1T tokens outperformed GPT-3 (175B) trained on 300B tokens — proving that **more data on smaller models beats less data on huge models** (the Chinchilla insight in action).

## Step 1: Prepare Data

```python
import torch
import numpy as np
from transformers import AutoTokenizer
from pathlib import Path

def prepare_data(text_file, tokenizer_name='gpt2', output_dir='data'):
    """Tokenize text and split into train/val."""
    tokenizer = AutoTokenizer.from_pretrained(tokenizer_name)
    
    with open(text_file, 'r', encoding='utf-8') as f:
        text = f.read()
    
    # Tokenize
    token_ids = tokenizer.encode(text)
    token_ids = np.array(token_ids, dtype=np.uint16)
    
    # Split 90/10
    n = len(token_ids)
    train_ids = token_ids[:int(0.9 * n)]
    val_ids = token_ids[int(0.9 * n):]
    
    # Save as binary
    Path(output_dir).mkdir(exist_ok=True)
    train_ids.tofile(f'{output_dir}/train.bin')
    val_ids.tofile(f'{output_dir}/val.bin')
    
    print(f"Total tokens: {n:,}")
    print(f"Train tokens: {len(train_ids):,}")
    print(f"Val tokens: {len(val_ids):,}")
    print(f"Vocab size: {tokenizer.vocab_size}")
    
    return tokenizer

# Datasets to try:
# - Shakespeare (~1M tokens): https://raw.githubusercontent.com/karpathy/char-rnn/master/data/tinyshakespeare/input.txt
# - OpenWebText subset
# - Your own text corpus
```

## Step 2: Dataset Class

> 🍕 **Beginner Analogy**: Imagine you have a giant scroll of text. The dataset class is like a pizza cutter — it slices the scroll into equal-sized pieces (`seq_length`), and for each piece, it creates an input (everything except the last word) and a target (everything except the first word). The model learns: *"Given these words, predict the next one."*

> ⭐ **Why `memmap`?** Memory-mapping lets you work with datasets **larger than your RAM**. The file stays on disk, but Python can read it as if it were in memory. This is how production systems handle terabyte-scale datasets — you don't load 1TB into 16GB of RAM!

```python
class PreTokenizedDataset(torch.utils.data.Dataset):
    def __init__(self, data_path, seq_length):
        self.data = np.memmap(data_path, dtype=np.uint16, mode='r')
        self.seq_length = seq_length
    
    def __len__(self):
        return len(self.data) // self.seq_length - 1
    
    def __getitem__(self, idx):
        start = idx * self.seq_length
        chunk = self.data[start:start + self.seq_length + 1].astype(np.int64)
        x = torch.from_numpy(chunk[:-1])
        y = torch.from_numpy(chunk[1:])
        return x, y
```

### ⭐ Visual: How Input/Target Pairs Work

```
Text:    "The cat sat on the mat"
Tokens:  [464, 3797, 3332, 319, 262, 2603]

Input  (x): [464, 3797, 3332, 319, 262]      ← "The cat sat on the"
Target (y): [3797, 3332, 319, 262, 2603]      ← "cat sat on the mat"
                                                  (shifted by 1!)
```

> 💡 The model sees "The" and must predict "cat", sees "The cat" and must predict "sat", etc. This is **next-token prediction** — the core of ALL GPT models!

---

# 2️⃣ Model Configurations for Different Scales 📐

> 🏗️ **Beginner Analogy**: These configurations are like blueprints for **different-sized factories**. A tiny factory (10M params) fits in your garage and builds simple things. A medium factory (125M params) fills a warehouse and can produce sophisticated products. The same design principles apply at every scale — you just make everything bigger!

### ⭐ Understanding the Parameters (What Each Knob Controls)

| Parameter | What It Does | Analogy |
|-----------|-------------|---------|
| `vocab_size` (50,257) | How many unique word-pieces the model knows | Dictionary size 📚 |
| `d_model` | Width of the "thinking space" | Width of the conveyor belt |
| `num_heads` | How many things to pay attention to simultaneously | Number of security cameras 📹 |
| `num_layers` | How many times information gets refined | Floors in the factory building 🏢 |
| `max_seq_len` | Longest text the model can see at once | How far back the model can remember |
| `dropout` | Randomly disable neurons during training | Cross-training workers on multiple jobs |

### ⭐ How to Count Parameters (The Formula)

For a standard Transformer decoder:

$$N \approx 12 \times L \times d^2$$

Where $L$ = number of layers, $d$ = `d_model`. This comes from each layer having:
- Self-attention: $4d^2$ parameters (Q, K, V, Output projections)
- Feed-forward: $8d^2$ parameters (two linear layers with $4d$ hidden dim)
- Layer norms and biases: small, ~$4d$

Plus the embedding layer: $V \times d$ (where $V$ = vocab size)

**Quick verification:**
- Tiny: $12 \times 4 \times 256^2 \approx 3.1M$ + embeddings $\approx$ **~16M** ✅
- Small: $12 \times 8 \times 512^2 \approx 25M$ + embeddings $\approx$ **~51M** ✅
- Medium: $12 \times 12 \times 768^2 \approx 85M$ + embeddings $\approx$ **~124M** ✅

### 🏢 Production Reference: Famous Model Configurations

| Model | Params | Layers | d_model | Heads | Context | Training Tokens |
|-------|--------|--------|---------|-------|---------|----------------|
| GPT-2 Small | 124M | 12 | 768 | 12 | 1024 | ~40B |
| GPT-2 Large | 774M | 36 | 1280 | 20 | 1024 | ~40B |
| GPT-3 | 175B | 96 | 12288 | 96 | 2048 | 300B |
| LLaMA-7B | 6.7B | 32 | 4096 | 32 | 2048 | 1T |
| LLaMA-2 70B | 70B | 80 | 8192 | 64 | 4096 | 2T |
| Mistral 7B | 7.3B | 32 | 4096 | 32 | 32768 | ~2T+ |

> 📖 **Research Note**: Brown et al. (2020) showed that GPT-3's performance improved smoothly with scale — each 10× increase in parameters yielded roughly the same improvement in loss, following an almost perfectly straight line on a log-log plot.


```python
# Tiny: for testing (10M params, trains in minutes)
config_tiny = {
    'vocab_size': 50257,
    'd_model': 256,
    'num_heads': 4,
    'num_layers': 4,
    'max_seq_len': 256,
    'dropout': 0.1,
}

# Small: for learning (50M params, trains in hours)
config_small = {
    'vocab_size': 50257,
    'd_model': 512,
    'num_heads': 8,
    'num_layers': 8,
    'max_seq_len': 512,
    'dropout': 0.1,
}

# Medium: serious model (125M params, trains in days)  
config_medium = {
    'vocab_size': 50257,
    'd_model': 768,
    'num_heads': 12,
    'num_layers': 12,
    'max_seq_len': 1024,
    'dropout': 0.1,
}
```

### ⭐ Cost Estimates: What Each Scale Actually Costs

| Config | Params | GPU Needed | Training Time | Cloud Cost (approx) |
|--------|--------|-----------|---------------|-------------------|
| Tiny | ~16M | Any GPU / CPU | 5–30 minutes | Free (Colab) |
| Small | ~51M | GTX 1080 / T4 | 2–8 hours | $1–5 |
| Medium | ~124M | RTX 3090 / A100 | 1–3 days | $50–200 |
| GPT-2 Large (774M) | 774M | 4× A100 | 1–2 weeks | $5K–15K |
| GPT-3 scale (175B) | 175B | 1000+ A100s | 2–3 months | $4M–12M 💸 |

> 💡 **Pro Tip**: Always start with `config_tiny` to debug your code! Finding a bug after 3 days of training on a medium model is heartbreaking 💔. Find bugs in 5 minutes on tiny, THEN scale up.

---

# 3️⃣ Complete Training Script 🏋️

> 🎮 **Beginner Analogy**: This is the **main game loop**. Just like a video game runs frame-by-frame (read input → update state → render), training runs step-by-step:
>
> 1. **Feed data** → Give the model a batch of text
> 2. **Compute loss** → Measure how wrong the predictions are
> 3. **Backpropagate** → Figure out which knobs to adjust
> 4. **Update weights** → Turn those knobs a tiny bit
> 5. **Repeat** → Millions of times!
>
> After each epoch (full pass through data), we check on validation data — like a pop quiz to see if the student actually learned, not just memorized.

### ⭐ Key Training Components Explained

| Component | What It Does | Why It Matters |
|-----------|-------------|----------------|
| **AdamW optimizer** | Smart weight updater with momentum + decay | State-of-the-art for transformers; better than plain SGD |
| **Cosine schedule** | Gradually reduces learning rate | Big steps early (explore), small steps later (refine) |
| **Warmup** | Start with tiny LR, ramp up | Prevents exploding gradients at the start |
| **Gradient clipping** | Cap gradient magnitude at 1.0 | Emergency brake — prevents catastrophic updates |
| **Mixed precision** | Use float16 for speed, float32 for accuracy | 2× faster training, half the memory |


```python
import torch
import torch.nn.functional as F
import math
import time

def train_gpt(model_config, train_config):
    """Complete GPT training pipeline."""
    
    # Setup
    device = 'cuda' if torch.cuda.is_available() else 'cpu'
    print(f"Using device: {device}")
    
    # Data
    train_dataset = PreTokenizedDataset('data/train.bin', model_config['max_seq_len'])
    val_dataset = PreTokenizedDataset('data/val.bin', model_config['max_seq_len'])
    
    train_loader = torch.utils.data.DataLoader(
        train_dataset, batch_size=train_config['batch_size'], 
        shuffle=True, pin_memory=True, num_workers=2
    )
    val_loader = torch.utils.data.DataLoader(
        val_dataset, batch_size=train_config['batch_size'],
        shuffle=False, pin_memory=True
    )
    
    # Model
    model = GPT(**model_config).to(device)
    n_params = sum(p.numel() for p in model.parameters())
    print(f"Model parameters: {n_params:,} ({n_params/1e6:.1f}M)")
    
    # Optimizer
    optimizer = torch.optim.AdamW(
        model.parameters(),
        lr=train_config['learning_rate'],
        betas=(0.9, 0.95),
        weight_decay=0.1,
    )
    
    # Scheduler
    total_steps = train_config['num_epochs'] * len(train_loader)
    warmup_steps = min(1000, total_steps // 10)
    
    def get_lr(step):
        if step < warmup_steps:
            return step / warmup_steps
        progress = (step - warmup_steps) / (total_steps - warmup_steps)
        return 0.1 + 0.45 * (1 + math.cos(math.pi * progress))
    
    scheduler = torch.optim.lr_scheduler.LambdaLR(optimizer, get_lr)
    
    # Training loop
    best_val_loss = float('inf')
    global_step = 0
    
    for epoch in range(train_config['num_epochs']):
        model.train()
        epoch_loss = 0
        epoch_tokens = 0
        start_time = time.time()
        
        for batch_idx, (x, y) in enumerate(train_loader):
            x, y = x.to(device), y.to(device)
            
            logits, loss = model(x, y)
            loss.backward()
            
            # Gradient clipping
            torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
            
            optimizer.step()
            scheduler.step()
            optimizer.zero_grad(set_to_none=True)
            
            epoch_loss += loss.item() * x.shape[0]
            epoch_tokens += x.shape[0] * x.shape[1]
            global_step += 1
            
            if global_step % 100 == 0:
                lr = scheduler.get_last_lr()[0] * train_config['learning_rate']
                elapsed = time.time() - start_time
                tokens_per_sec = epoch_tokens / elapsed
                print(f"  Step {global_step:5d} | loss {loss.item():.4f} | "
                      f"lr {lr:.2e} | {tokens_per_sec:.0f} tok/s")
        
        # Validation
        val_loss = evaluate(model, val_loader, device)
        val_ppl = math.exp(val_loss)
        
        print(f"\nEpoch {epoch+1}/{train_config['num_epochs']}: "
              f"train_loss={epoch_loss/len(train_dataset):.4f} | "
              f"val_loss={val_loss:.4f} | val_ppl={val_ppl:.2f}")
        
        # Save best model
        if val_loss < best_val_loss:
            best_val_loss = val_loss
            torch.save(model.state_dict(), 'best_model.pt')
            print(f"  ★ Saved best model (val_loss={val_loss:.4f})")
        
        # Generate sample
        generate_sample(model, tokenizer, device)
    
    return model

@torch.no_grad()
def evaluate(model, dataloader, device):
    model.eval()
    total_loss = 0
    total_tokens = 0
    for x, y in dataloader:
        x, y = x.to(device), y.to(device)
        _, loss = model(x, y)
        total_loss += loss.item() * x.shape[0] * x.shape[1]
        total_tokens += x.shape[0] * x.shape[1]
    return total_loss / total_tokens

def generate_sample(model, tokenizer, device, prompt="The ", max_tokens=100):
    model.eval()
    input_ids = torch.tensor([tokenizer.encode(prompt)]).to(device)
    output = model.generate(input_ids, max_new_tokens=max_tokens, temperature=0.8, top_k=40)
    text = tokenizer.decode(output[0].tolist())
    print(f"\n--- Generated Sample ---\n{text}\n---\n")
```

### ⭐ Understanding the Learning Rate Schedule (Visual)

```
LR
↑
max_lr ─────────╮
                │╲  ← cosine decay
                │  ╲
                │    ╲
    warmup → ╱  │      ╲
           ╱    │        ╲───── min_lr (10% of max)
──────────┴─────┴──────────────→ Steps
  0    warmup          total_steps
```

> 💡 **Why cosine?** Loshchilov & Hutter (2017) showed that cosine annealing with warm restarts matches or beats step-decay schedules. Nearly all modern LLM training uses cosine schedules. The warmup prevents early divergence when the model's random weights produce wild gradients.

### 🏢 Production Training Practices at Scale

| Practice | Small Scale (You) | Large Scale (OpenAI/Meta) |
|----------|-------------------|---------------------------|
| Batch size | 32–64 | 1M–4M tokens per batch |
| Gradient accumulation | 2–8 steps | 32–512 steps |
| Precision | fp32 or mixed fp16 | bf16 + fp32 master weights |
| Checkpointing | Save best model | Save every 1000 steps + optimizer state |
| Distributed | Single GPU | 100s–1000s of GPUs (FSDP/DeepSpeed) |
| Data loading | Single DataLoader | Distributed streaming from object storage |
| Cost per run | $0–200 | $1M–$100M+ 💸 |

> 📖 **Research Note**: Rajbhandari et al. (2020) introduced ZeRO (Zero Redundancy Optimizer) in DeepSpeed, enabling training of models with 100B+ parameters by partitioning optimizer states across GPUs. This is how most large-scale training is done today.

---

# 4️⃣ Scaling Laws: How Performance Grows 📈🔬

> 🔮 **Beginner Analogy — The Crystal Ball of AI**: Imagine you want to build a skyscraper. Before pouring concrete, architects can **predict exactly** how strong the building will be based on the materials and size. Scaling laws are the same thing for AI — they let you **predict how good a model will be BEFORE spending months training it**. This is arguably the most important discovery in modern AI.

> ⭐ **Why This Matters**: Training GPT-4 cost ~$100M. You don't want to spend that and then discover it's not good enough. Scaling laws let you train tiny models ($100), plot the trend, and extrapolate whether the $100M model will meet your target. They're the **business plan of AI training**.

## Kaplan et al. (2020) / Chinchilla (2022)

Key finding: Loss scales as a power law with three variables:

$$L(N) = \left(\frac{N_c}{N}\right)^{\alpha_N}$$  (model size scaling)

$$L(D) = \left(\frac{D_c}{D}\right)^{\alpha_D}$$  (data scaling)

$$L(C) = \left(\frac{C_c}{C}\right)^{\alpha_C}$$  (compute scaling)

Where:
- $N$ = number of parameters
- $D$ = number of training tokens
- $C$ = compute budget (FLOPs)

### ⭐ What "Power Law" Means (For Beginners)

A **power law** means: *"Every time you multiply the input by 10, the output changes by a fixed amount."*

```
                    Scaling Law on Log-Log Plot
Loss (log)
  ↑
  4 ● 
  3   ●
  2     ●          ← Straight line on log-log = power law!
  1       ●
  0         ●
  ──┬──┬──┬──┬──→ Parameters (log)
   1M 10M 100M 1B
```

On a **log-log plot**, a power law looks like a **straight line**. This means you can:
1. Train 3 tiny models (cheap)
2. Plot their losses on a log-log chart
3. Draw a straight line through them
4. **Extend the line** to predict how a 100× bigger model will perform

> 💡 This is exactly what companies do! Train at $10K, $50K, $200K → predict the $50M model's performance.

### ⭐ The Three Scaling Law Perspectives

| Scaling Axis | Formula | What You're Asking | Typical $\alpha$ |
|---|---|---|---|
| **Model size** ($N$) | $L(N) = (N_c/N)^{\alpha_N}$ | "If I make the model bigger..." | $\alpha_N \approx 0.076$ |
| **Data** ($D$) | $L(D) = (D_c/D)^{\alpha_D}$ | "If I train on more data..." | $\alpha_D \approx 0.095$ |
| **Compute** ($C$) | $L(C) = (C_c/C)^{\alpha_C}$ | "If I spend more FLOPs..." | $\alpha_C \approx 0.050$ |

### 🧮 FLOPs: Counting the Computational Cost

The approximate FLOPs for training a transformer:

$$\text{FLOPs} \approx 6 \times N \times D$$

Where $N$ = parameters, $D$ = training tokens. The factor of 6 comes from:
- **2** for the forward pass (multiply-add = 2 ops per parameter per token)
- **4** for the backward pass (~2× the forward pass)

**Example**: Training a 7B model on 1T tokens:
$$\text{FLOPs} = 6 \times 7 \times 10^9 \times 1 \times 10^{12} = 4.2 \times 10^{22}$$

On an A100 GPU (~300 TFLOPS effective):
$$\text{Time} = \frac{4.2 \times 10^{22}}{300 \times 10^{12}} \approx 1.4 \times 10^8 \text{ seconds} \approx 1,620 \text{ GPU-days}$$

That's **~1,620 GPU-days** or about **50 days on 32 A100s** ⏱️

## The Chinchilla Insight 🐭

> 📖 **Key Paper**: Hoffmann et al. (2022) — *"Training Compute-Optimal Large Language Models"* (the "Chinchilla paper")

For compute-optimal training:

$$N_{\text{opt}} \propto C^{0.5}$$
$$D_{\text{opt}} \propto C^{0.5}$$

Equal scaling of model size AND data!

### ⭐ What Chinchilla Changed (The Revolution)

Before Chinchilla, the conventional wisdom (from Kaplan et al., 2020) was:
- **"Make models bigger, data doesn't matter as much"**
- GPT-3 (175B params) was trained on "only" 300B tokens → massively **undertrained**!

Chinchilla showed:
- **"Scale model AND data equally"**
- A 70B model trained on 1.4T tokens (Chinchilla) **beat** the 280B Gopher trained on 300B tokens
- **4× smaller** but trained on **4.7× more data** → better performance AND cheaper to serve!

> 🏢 **Industry Impact**: After Chinchilla, the entire industry shifted:
> - Meta's LLaMA (7B–65B): Trained on 1T–1.4T tokens ✅
> - Mistral 7B: ~2T+ tokens ✅
> - Google's Gemma: Chinchilla-optimal ratios ✅
> - Every serious lab now uses Chinchilla-optimal or **over-trains** (more tokens than optimal for cheaper inference)

### ⭐ The Over-Training Trend (Post-Chinchilla)

Sardana & Frankle (2023) and Touvron et al. (2023) showed a new trend: **intentionally over-training smaller models**. Why?

$$\text{Inference cost} \propto N \quad \text{(parameters, not data!)}$$

A 7B model trained on 2T tokens costs **10× less to serve** than a 70B model trained on 200B tokens, even if the 70B model is slightly better. When you'll serve billions of queries, **inference cost dominates**.

| Strategy | Model | Tokens | Chinchilla-Optimal? | Reason |
|----------|-------|--------|---------------------|--------|
| Under-trained | GPT-3 175B | 300B | ❌ (needs ~3.5T) | Pre-Chinchilla era |
| Chinchilla-optimal | Chinchilla 70B | 1.4T | ✅ | Proved the theory |
| Over-trained | LLaMA-2 7B | 2T | ❌ (optimal ~200B) | Cheaper inference |
| Over-trained | Mistral 7B | ~2T+ | ❌ (optimal ~200B) | Cheaper inference |

| Compute Budget | Optimal Model Size | Optimal Tokens |
|---------------|-------------------|----------------|
| $10^{18}$ FLOPs | ~400M | ~8B tokens |
| $10^{20}$ FLOPs | ~4B | ~80B tokens |
| $10^{22}$ FLOPs | ~40B | ~800B tokens |
| $10^{24}$ FLOPs | ~400B | ~8T tokens |

### 🔬 Deep Research: The Full Scaling Law Equation

The complete Chinchilla scaling law (Hoffmann et al., 2022) combines both $N$ and $D$:

$$L(N, D) = E + \frac{A}{N^{\alpha}} + \frac{B}{D^{\beta}}$$

Where:
- $E \approx 1.69$ = irreducible loss (entropy of natural language — can't go below this!)
- $A \approx 406.4$, $\alpha \approx 0.34$
- $B \approx 410.7$, $\beta \approx 0.28$

**This single equation predicts the loss of ANY model size trained on ANY amount of data!** 🤯

> 📖 **Research Citations for Scaling Laws**:
> - **Kaplan et al. (2020)** — *"Scaling Laws for Neural Language Models"* — First comprehensive scaling laws; found $L(N) \propto N^{-0.076}$
> - **Hoffmann et al. (2022)** — *"Training Compute-Optimal Large Language Models"* (Chinchilla) — Corrected compute-optimal ratios; showed $N_{\text{opt}} \propto C^{0.5}$
> - **Muennighoff et al. (2023)** — *"Scaling Data-Constrained Language Models"* — What to do when you run out of data: repeat data up to 4× with minimal loss
> - **Sardana & Frankle (2023)** — *"Beyond Chinchilla-Optimal: Accounting for Inference"* — When to intentionally over-train

### 🌟 Emergent Abilities: When Scale Creates Magic

> 📖 **Key Paper**: Wei et al. (2022) — *"Emergent Abilities of Large Language Models"*

Some abilities **don't exist at small scale and suddenly appear at large scale**:

```
Ability
  ↑
  │                    ╱ ← Sudden jump! 🚀
  │                   ╱
  │        ──────────╱
  │ ──────/   (random performance)
  ──┬──┬──┬──┬──┬──→ Model Size
   1M 100M 1B 10B 100B
```

| Emergent Ability | Appears Around | Example |
|-----------------|---------------|---------|
| Few-shot arithmetic | ~13B params | "What is 234 + 567?" |
| Chain-of-thought reasoning | ~100B params | Step-by-step math |
| Code generation | ~10B params | Write working Python |
| Multi-step reasoning | ~60B+ params | Complex word problems |

> ⚠️ **Debate**: Schaeffer et al. (2023) argued that emergence might be a **mirage** caused by nonlinear metrics — when you use linear metrics, the improvement is smooth. This is an active research debate!

## Running Your Own Scaling Experiment

```python
def scaling_experiment(data_path, tokenizer):
    """Train models at different scales and plot loss vs params."""
    configs = [
        {'d_model': 64,  'num_heads': 2, 'num_layers': 2},   # ~0.5M
        {'d_model': 128, 'num_heads': 4, 'num_layers': 4},   # ~4M
        {'d_model': 256, 'num_heads': 4, 'num_layers': 6},   # ~20M
        {'d_model': 384, 'num_heads': 6, 'num_layers': 8},   # ~50M
        {'d_model': 512, 'num_heads': 8, 'num_layers': 12},  # ~120M
    ]
    
    results = []
    for config in configs:
        config.update({'vocab_size': 50257, 'max_seq_len': 256})
        model = GPT(**config)
        n_params = sum(p.numel() for p in model.parameters())
        
        # Train for fixed number of tokens
        val_loss = train_and_evaluate(model, data_path, num_tokens=10_000_000)
        
        results.append({'params': n_params, 'val_loss': val_loss})
        print(f"Params: {n_params/1e6:.1f}M, Val Loss: {val_loss:.4f}")
    
    # Plot: should see roughly linear relationship on log-log scale
    plot_scaling_law(results)
    return results
```

### ⭐ How to Read Your Scaling Plot

```python
# After running scaling_experiment(), you'll get something like:
#
# Log-Log Plot:
#   Val Loss (log)
#     ↑
#   2.0 ●  0.5M params
#   1.5    ● 4M
#   1.0       ● 20M
#   0.8          ● 50M
#   0.7             ● 120M
#     └──┬──┬──┬──┬──→ Params (log)
#
# If the points form a straight line → you've verified the power law! 🎉
# The SLOPE of the line tells you α (how fast loss improves with scale)
# 
# To predict a 1B model's loss: extend the line to 1B on the x-axis
```

> 💡 **Pro Tip**: If your points DON'T form a straight line, something is wrong:
> - Curve flattens? → Not enough training data (data bottleneck)
> - Curve steepens? → Bug in smaller models (maybe too high dropout)
> - Points are noisy? → Need more consistent training (same tokens per model)

---

# 5️⃣ Training Tips for Different Scales 🎛️📋

> 🚗 **Beginner Analogy**: Driving a go-kart, a sedan, and a semi-truck require different skills even though they all have steering wheels and gas pedals. Similarly, training a 10M model vs. a 1B model requires different settings — even though the algorithm is the same. This section is your **driver's manual for each vehicle size**.

### ⭐ The Golden Rule of Scaling

> **"Get it working small, then scale up."** — Andrej Karpathy

Every large model was first debugged as a small model. If your tiny model can't overfit on 100 examples, your large model won't either. Fix the bug at $0.01, not at $10,000.

## Tiny Model (<10M params) 🏎️
- Train on small datasets (Shakespeare, articles)
- 1-5 epochs
- Batch size 32-64
- LR 3e-4
- No mixed precision needed
- Trains in minutes on single GPU

> 💡 **Use case**: Debugging code, testing hypotheses, learning. Karpathy's nanoGPT trains a tiny model on Shakespeare in ~5 minutes.

## Small Model (10-100M params) 🚙
- Need 100M+ tokens
- 1-2 epochs
- Effective batch size 128-256
- LR 3e-4
- Use gradient accumulation
- Mixed precision recommended
- Trains in hours

> 💡 **Use case**: Research experiments, paper reproductions, fine-tuning for specific tasks.

> ⭐ **Gradient Accumulation Explained**: If your GPU can only handle batch_size=16 but you want effective batch_size=256, accumulate gradients over 16 steps before updating: `if step % 16 == 0: optimizer.step()`. This simulates a bigger batch without needing more memory!

## Medium Model (100M-1B params) 🚛
- Need 1B+ tokens
- Often single pass through data
- Effective batch size 256-512
- LR 1e-4 to 3e-4
- AMP essential
- Trains in days

> 💡 **Use case**: Production fine-tuning, domain-specific models, startup MVPs.

### ⭐ Complete Training Hyperparameter Cheat Sheet

| Hyperparameter | Tiny (<10M) | Small (10-100M) | Medium (100M-1B) | Large (1B+) |
|---------------|-------------|-----------------|-------------------|-------------|
| Learning rate | 3e-4 | 3e-4 | 1e-4 to 3e-4 | 1e-4 to 2e-4 |
| Warmup steps | 100 | 500–1,000 | 1,000–2,000 | 2,000–5,000 |
| Batch size (tokens) | 8K–16K | 32K–128K | 128K–512K | 512K–4M |
| Weight decay | 0.1 | 0.1 | 0.1 | 0.1 |
| Adam β₁ | 0.9 | 0.9 | 0.9 | 0.9 |
| Adam β₂ | 0.95–0.999 | 0.95 | 0.95 | 0.95 |
| Gradient clip | 1.0 | 1.0 | 1.0 | 1.0 |
| Dropout | 0.1 | 0.1 | 0.0–0.1 | 0.0 |
| Precision | fp32 | fp16/bf16 | bf16 | bf16 |
| Min LR | 1e-5 | 1e-5 | 1e-5 | 1e-5 |

> 📖 **Research Note**: Most of these values converged across labs. Touvron et al. (2023, LLaMA), Jiang et al. (2023, Mistral), and Google DeepMind (Gemma) all use remarkably similar settings — AdamW, cosine schedule, β₂=0.95, weight_decay=0.1, grad_clip=1.0. The "recipe" is largely standardized.

---

# 6️⃣ Training Diagnostics Monitoring 📊🔍

> 🏥 **Beginner Analogy**: This is the **hospital dashboard** for your model during training. Just like doctors monitor heart rate, blood pressure, and oxygen levels, you monitor loss, learning rate, and gradient norms. **If you don't watch the dashboard, you won't know your model is "sick" until it's too late!**

### ⭐ What Each Metric Tells You

| Metric | Healthy Sign ✅ | Danger Sign ❌ | What To Do |
|--------|----------------|----------------|------------|
| **Train loss** | Steady decrease | Flat or increasing | Check LR, data loading |
| **Val loss** | Tracks train loss | Diverges upward from train | Overfitting → more data, more dropout |
| **Learning rate** | Smooth warmup + decay | Jumps or stays flat | Check scheduler code |
| **Gradient norm** | Stable (0.1–10) | Spikes >100 or =0 | Clip gradients, lower LR |
| **Train/val gap** | Small difference | Large gap | Regularize or get more data |

### ⭐ Common Training Failures and Fixes

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| 🔥 Loss = NaN | Exploding gradients | Lower LR, add grad clipping, check for division by zero |
| 📈 Loss increases | LR too high | Reduce LR by 3–10× |
| 📊 Loss plateaus | LR too low or stuck in local minimum | Increase LR or use warmup restarts |
| 🔄 Loss oscillates | Batch size too small | Increase effective batch size |
| 📉 Val loss rises while train drops | Overfitting | Add dropout, reduce model size, or add data |
| 🐌 Training very slow | No GPU utilization or poor data loading | Check `nvidia-smi`, use `pin_memory=True`, more `num_workers` |


```python
class TrainingMonitor:
    def __init__(self):
        self.metrics = {
            'train_loss': [], 'val_loss': [], 'train_ppl': [],
            'val_ppl': [], 'lr': [], 'grad_norm': []
        }
    
    def log(self, step, **kwargs):
        for key, value in kwargs.items():
            self.metrics[key].append((step, value))
    
    def plot(self):
        import matplotlib.pyplot as plt
        fig, axes = plt.subplots(2, 2, figsize=(14, 10))
        
        for ax, key in zip(axes.flat, ['train_loss', 'val_loss', 'lr', 'grad_norm']):
            if self.metrics[key]:
                steps, values = zip(*self.metrics[key])
                ax.plot(steps, values)
                ax.set_title(key)
                ax.set_xlabel('Step')
                ax.grid(True, alpha=0.3)
        
        plt.suptitle('Training Diagnostics')
        plt.tight_layout()
        plt.savefig('training_diagnostics.png', dpi=150)
        plt.show()
```

---

# 📝 Summary

| Component | Key Choice |
|-----------|-----------|
| Dataset | Pre-tokenize to binary, memmap for loading |
| Model size | Start tiny (10M), scale up as you learn |
| Training | AdamW, cosine schedule, gradient clipping |
| Evaluation | Perplexity + generation quality + benchmarks |
| Scaling | Loss $\propto N^{-\alpha}$ — power law with parameters |
| Chinchilla | Scale data and params equally for fixed compute |

---

# 🧾 CHEAT SHEET: Scaling Mini GPT — Everything on One Page

## 🔑 Core Formulas (LaTeX)

| Formula | Equation | What It Predicts |
|---------|----------|-----------------|
| Power law (params) | $L(N) = (N_c / N)^{\alpha_N}$ | Loss from model size |
| Power law (data) | $L(D) = (D_c / D)^{\alpha_D}$ | Loss from data amount |
| Power law (compute) | $L(C) = (C_c / C)^{\alpha_C}$ | Loss from total compute |
| Full Chinchilla law | $L(N,D) = E + A/N^{\alpha} + B/D^{\beta}$ | Loss from both size + data |
| Compute-optimal (size) | $N_{\text{opt}} \propto C^{0.5}$ | Best model size for budget |
| Compute-optimal (data) | $D_{\text{opt}} \propto C^{0.5}$ | Best data amount for budget |
| FLOPs estimate | $\text{FLOPs} \approx 6ND$ | Training compute cost |
| Parameter count | $N \approx 12 L d^2$ | Params from architecture |

## ⭐ Decision Tree: How Big Should My Model Be?

```
START: What's your compute budget?
  │
  ├── $0–10 (Free tier/Colab)
  │     → Tiny: 10M params, Shakespeare, learn the basics
  │
  ├── $10–500 (Personal/student)
  │     → Small: 50–125M params, 100M–1B tokens
  │
  ├── $500–50K (Startup/research lab)
  │     → Medium: 125M–1B params, 1B–20B tokens
  │     → Or fine-tune an existing open model (often better!)
  │
  ├── $50K–5M (Well-funded startup)
  │     → 1B–13B params, use Chinchilla-optimal ratios
  │     → Consider: is pretraining better than fine-tuning?
  │
  └── $5M+ (Big tech)
        → 13B–200B+ params, 1T–15T tokens
        → Full scaling law analysis before committing
```

## 📚 Must-Read Papers (Ordered by Importance)

| # | Paper | Authors | Year | Key Contribution |
|---|-------|---------|------|-----------------|
| 1 | Scaling Laws for Neural Language Models | Kaplan et al. | 2020 | Discovered clean power laws for LLM scaling |
| 2 | Training Compute-Optimal LLMs (Chinchilla) | Hoffmann et al. | 2022 | Corrected compute allocation: scale data + params equally |
| 3 | Emergent Abilities of LLMs | Wei et al. | 2022 | Documented abilities that appear suddenly at scale |
| 4 | LLaMA: Open and Efficient LLMs | Touvron et al. | 2023 | Proved smaller + more data > bigger + less data |
| 5 | Scaling Data-Constrained LMs | Muennighoff et al. | 2023 | What to do when you run out of unique data |
| 6 | Beyond Chinchilla-Optimal | Sardana & Frankle | 2023 | When to intentionally over-train for inference savings |
| 7 | Are Emergent Abilities a Mirage? | Schaeffer et al. | 2023 | Challenges to the emergence narrative |

## 🏭 Quick Reference: Scaling Analogies

| AI Concept | Real-World Analogy |
|-----------|-------------------|
| Scaling | Building bigger factories 🏗️ |
| Scaling laws | Predicting output quality before building the factory |
| Compute-optimal (Chinchilla) | Perfect balance between factory size and raw materials |
| FLOPs | Counting bricks needed to build a wall 🧱 |
| Emergence | Water suddenly boiling at 100°C — gradual heat, sudden change 💨 |
| Over-training | Using a small efficient car instead of a gas-guzzling truck 🚗 vs 🚛 |
| Power law | Compound interest — predictable growth pattern 📈 |
| Loss | Golf score — lower is better ⛳ |

---

**Tomorrow**: Advanced Training Techniques — Gradient Accumulation patterns, Loss Spikes handling, and Training Stability.
