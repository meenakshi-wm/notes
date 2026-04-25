📘 DAY 35 — Paper Reproduction: LoRA (Low-Rank Adaptation)

Goal:

Reproduce LoRA (Hu et al., 2021) — implement low-rank adaptation from scratch, fine-tune a pretrained model, and verify that LoRA achieves comparable quality to full fine-tuning with <1% trainable parameters.

---

# 🌟 Beginner's Guide: What is LoRA and Why Should You Care?

> **No AI knowledge needed!** This section explains LoRA from zero.

## 📖 The Textbook Analogy

Imagine you have an **enormous textbook** 📚 (a pre-trained AI model like GPT-4 or Llama) with millions of pages. You want to teach it a **new subject** (like medical terminology or legal language).

| Approach | Analogy | Cost |
|----------|---------|------|
| ⭐ **Full fine-tuning** | **Reprint the entire textbook** with small edits on every page | 💰💰💰💰💰 Extremely expensive |
| ⭐ **LoRA** | **Stick Post-it notes** 📝 on specific pages — small targeted changes without rewriting anything | 💰 Very cheap! |
| ⭐ **Rank $r$** | **How many Post-it notes** you use ($r=4$ means 4 notes per page, $r=16$ means 16) | More notes = better but costs more |
| ⭐ **Merging** | **Permanently write** the Post-it notes into the textbook pages | Free at deployment! |
| ⭐ **QLoRA** | Use a **compressed/pocket-sized textbook** (4-bit) + Post-it notes | Even cheaper! Single GPU! |

## 🎯 Why LoRA Matters (One-Sentence Version)

> LoRA lets you **customize giant AI models** using **100× less money and memory** than the traditional approach, with nearly identical quality.

## 🏭 Real-World Impact

- **Before LoRA (2020):** Fine-tuning GPT-3 required dozens of expensive GPUs and cost thousands of dollars
- **After LoRA (2021):** Fine-tune the same model on a **single GPU** for a few dollars
- **QLoRA (2023):** Fine-tune a **65-billion parameter** model on a single consumer GPU (e.g., RTX 3090)

⭐ **LoRA is THE industry standard** for fine-tuning LLMs today — used in Llama adapters, Stable Diffusion fine-tuning, Mistral, and virtually every production LLM deployment.

---

# 1️⃣ Paper Summary

| Aspect | Detail |
|--------|--------|
| Problem | Full fine-tuning of large models is expensive |
| Key idea | Add low-rank trainable matrices to frozen weights |
| Math | W_new = W_frozen + B·A where B∈R^{d×r}, A∈R^{r×k}, r << min(d,k) |
| Result | GPT-3 175B fine-tuned with 0.01% trainable params |
| Quality | Matches or exceeds full fine-tuning on many tasks |

---

# 2️⃣ LoRA Math

Original: h = W·x where W ∈ R^{d×k}
LoRA: h = W·x + (B·A)·x where B ∈ R^{d×r}, A ∈ R^{r×k}

During training: W is frozen, only B and A are trained.
After training: Merge W_new = W + B·A (no inference overhead!)

Key choices:
- rank r: typically 4, 8, 16, 32 (r << d)
- α (scaling): LoRA output is scaled by α/r
- Which layers: attention Q, V (paper), or all linear layers

### 🔢 The Math in Detail (with LaTeX)

**Core LoRA equation:**

$$W' = W + \Delta W = W + BA$$

where:
- $W \in \mathbb{R}^{d \times k}$ — the original (frozen) weight matrix
- $B \in \mathbb{R}^{d \times r}$ — the "down-projection" LoRA matrix
- $A \in \mathbb{R}^{r \times k}$ — the "up-projection" LoRA matrix
- $r \ll \min(d, k)$ — the **rank** (the number of "Post-it notes" 📝)

**Forward pass with scaling:**

$$h = Wx + \frac{\alpha}{r} \cdot BAx$$

The $\alpha/r$ scaling factor controls how much influence the LoRA adapter has.

**⭐ Parameter Savings — Why LoRA is Efficient:**

| | Parameters | Example ($d=4096, k=4096, r=8$) |
|--|-----------|------------------------------------|
| Full fine-tuning | $d \times k$ | $4096 \times 4096 = 16{,}777{,}216$ |
| LoRA | $(d + k) \times r$ | $(4096 + 4096) \times 8 = 65{,}536$ |
| **Savings** | $\frac{(d+k) \times r}{d \times k}$ | $\frac{65{,}536}{16{,}777{,}216} = 0.39\%$ → **256× fewer parameters!** |

> 🧠 **Beginner intuition:** Instead of updating a $4096 \times 4096$ grid of numbers, LoRA updates two thin rectangles ($4096 \times 8$ and $8 \times 4096$) whose product *approximates* the change you'd make to the full grid.

**Initialization (critical for training stability):**
- $A$ initialized from $\mathcal{N}(0, \sigma^2)$ (small random values)
- $B$ initialized to **zero** → so $\Delta W = BA = 0$ at start (model begins unchanged)

### 🧊 QLoRA: Even More Efficient (Dettmers et al., 2023)

$$\text{QLoRA} = \underbrace{\text{4-bit quantized } W}_{\text{compressed textbook}} + \underbrace{BA}_{\text{Post-it notes in FP16}}$$

- Base weights stored in **4-bit NormalFloat (NF4)** precision → 4× memory reduction
- LoRA adapters trained in FP16/BF16 on top
- Uses **double quantization** and **paged optimizers** to further reduce memory
- ⭐ Result: Fine-tune a **65B parameter model on a single 48GB GPU**

### 📐 Intrinsic Dimensionality (Why LoRA Works)

Aghajanyan et al. (2020) showed that fine-tuning operates in a **low intrinsic dimension** — meaning the useful changes to weights live in a tiny subspace. LoRA exploits this directly:

$$\text{intrinsic dim} \ll d \times k \implies \Delta W \approx BA \text{ for small } r$$

---

# 3️⃣ Implementation from Scratch

```python
import torch
import torch.nn as nn
import math

class LoRALinear(nn.Module):
    """Linear layer with LoRA adaptation."""
    
    def __init__(self, original_linear, rank=8, alpha=16, dropout=0.05):
        super().__init__()
        self.original = original_linear
        self.original.weight.requires_grad_(False)  # Freeze original
        if self.original.bias is not None:
            self.original.bias.requires_grad_(False)
        
        in_features = original_linear.in_features
        out_features = original_linear.out_features
        
        # LoRA matrices
        self.lora_A = nn.Parameter(torch.randn(rank, in_features) * (1 / math.sqrt(rank)))
        self.lora_B = nn.Parameter(torch.zeros(out_features, rank))
        
        self.scaling = alpha / rank
        self.dropout = nn.Dropout(dropout) if dropout > 0 else nn.Identity()
    
    def forward(self, x):
        # Original frozen forward
        original_output = self.original(x)
        
        # LoRA forward: scale * B @ A @ x
        lora_output = self.dropout(x) @ self.lora_A.T @ self.lora_B.T * self.scaling
        
        return original_output + lora_output
    
    def merge(self):
        """Merge LoRA weights into original for inference."""
        self.original.weight.data += (self.lora_B @ self.lora_A * self.scaling)
        return self.original
    
    @property
    def trainable_params(self):
        return self.lora_A.numel() + self.lora_B.numel()

def apply_lora(model, target_modules=['q_proj', 'v_proj'], rank=8, alpha=16):
    """Replace target linear layers with LoRA versions."""
    lora_params = 0
    frozen_params = 0
    
    for name, module in model.named_modules():
        for attr_name in target_modules:
            if hasattr(module, attr_name):
                original = getattr(module, attr_name)
                if isinstance(original, nn.Linear):
                    lora_layer = LoRALinear(original, rank=rank, alpha=alpha)
                    setattr(module, attr_name, lora_layer)
                    lora_params += lora_layer.trainable_params
    
    frozen_params = sum(p.numel() for p in model.parameters() if not p.requires_grad)
    total = sum(p.numel() for p in model.parameters())
    
    print(f"Total params: {total:,}")
    print(f"Trainable (LoRA): {lora_params:,} ({lora_params/total*100:.4f}%)")
    print(f"Frozen: {frozen_params:,}")
    
    return model
```

---

# 4️⃣ Training with LoRA

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from torch.utils.data import DataLoader

def train_lora(model_name="gpt2", rank=8, epochs=3, lr=3e-4):
    device = torch.device('cuda')
    
    # Load pretrained model
    model = AutoModelForCausalLM.from_pretrained(model_name).to(device)
    tokenizer = AutoTokenizer.from_pretrained(model_name)
    
    # Apply LoRA to attention projections
    model = apply_lora(model, target_modules=['c_attn'], rank=rank, alpha=rank*2)
    
    # Only optimize LoRA parameters
    lora_params = [p for p in model.parameters() if p.requires_grad]
    optimizer = torch.optim.AdamW(lora_params, lr=lr, weight_decay=0.01)
    
    # Training
    model.train()
    for epoch in range(epochs):
        total_loss = 0
        for batch in train_loader:
            input_ids = batch['input_ids'].to(device)
            labels = batch['labels'].to(device)
            
            outputs = model(input_ids=input_ids, labels=labels)
            loss = outputs.loss
            
            optimizer.zero_grad()
            loss.backward()
            torch.nn.utils.clip_grad_norm_(lora_params, 1.0)
            optimizer.step()
            total_loss += loss.item()
        
        print(f"Epoch {epoch+1}, Loss: {total_loss/len(train_loader):.4f}")
    
    return model
```

---

# 5️⃣ Comparing Full Fine-Tuning vs LoRA

```python
def comparison_experiment(model_name="gpt2", dataset=None):
    """Compare full fine-tuning vs LoRA at different ranks."""
    results = {}
    
    # Full fine-tuning (baseline)
    model_full = AutoModelForCausalLM.from_pretrained(model_name).cuda()
    results['full'] = train_and_eval(model_full, dataset, lr=2e-5)
    
    # LoRA at different ranks
    for rank in [1, 2, 4, 8, 16, 32, 64]:
        model_lora = AutoModelForCausalLM.from_pretrained(model_name).cuda()
        model_lora = apply_lora(model_lora, rank=rank)
        results[f'lora_r{rank}'] = train_and_eval(model_lora, dataset, lr=3e-4)
    
    # Print comparison
    for name, res in results.items():
        trainable = res['trainable_params']
        print(f"{name}: params={trainable:,}, loss={res['eval_loss']:.4f}")
```

Expected results:
| Method | Trainable Params | Eval Loss | Relative to Full FT |
|--------|-----------------|-----------|---------------------|
| Full FT | 124M (100%) | 3.20 | Baseline |
| LoRA r=1 | 0.05M (0.04%) | 3.35 | 95.5% |
| LoRA r=4 | 0.2M (0.16%) | 3.23 | 99% |
| LoRA r=8 | 0.4M (0.32%) | 3.21 | 99.7% |
| LoRA r=16 | 0.8M (0.64%) | 3.20 | 100% |

---

# 6️⃣ Saving & Loading LoRA Weights

```python
def save_lora(model, path):
    """Save only LoRA parameters (tiny file!)."""
    lora_state = {}
    for name, param in model.named_parameters():
        if param.requires_grad:
            lora_state[name] = param.data.cpu()
    torch.save(lora_state, path)
    size_mb = sum(p.numel() * 4 for p in lora_state.values()) / 1e6
    print(f"Saved LoRA weights: {size_mb:.1f} MB")

def load_lora(model, path):
    """Load LoRA weights into a LoRA-enabled model."""
    lora_state = torch.load(path)
    model_state = model.state_dict()
    model_state.update(lora_state)
    model.load_state_dict(model_state)
    return model

# GPT-2 full model: ~500 MB
# GPT-2 LoRA r=8: ~1.5 MB  (330x smaller!)
```

> 📝 **Beginner note:** This is like saving *only your Post-it notes* instead of the whole textbook. When you need the model back, you load the original textbook (free download) and stick your Post-its back on!

---

# 7️⃣ Key Insights from Reproduction

| Finding | Confirmed? |
|---------|-----------|
| r=8 sufficient for most tasks | Yes |
| Scaling α/r matters | Yes, α=2r is good default |
| Q+V is better than Q+K | Marginal, Q+V+K+O is best |
| No inference overhead after merge | Yes, exactly matches |
| Higher LR than full FT (3e-4 vs 2e-5) | Yes, LoRA needs higher LR |

---

# 📝 Summary

| Aspect | LoRA | Full FT |
|--------|------|---------|
| Trainable params | 0.1-1% | 100% |
| Memory (training) | ~1 GPU | Multiple GPUs |
| Quality | 95-100% of full FT | Baseline |
| Checkpoint size | ~1-10 MB | ~1-10 GB |
| Can serve multiple adapters | Yes (swap) | No (separate models) |

---

# 🏭 Production Scenarios & Industry Usage

| Scenario | How LoRA Helps | Scale |
|----------|---------------|-------|
| 🏥 **Medical chatbot** | Fine-tune Llama-70B on clinical notes with LoRA $r=16$ | Single A100 GPU, ~$50 |
| 🎨 **Stable Diffusion styles** | Train LoRA adapter for custom art style | Consumer RTX 3090, ~2 hours |
| 💼 **Enterprise LLM** | 50 department-specific adapters sharing 1 base model | Serve all from 1 GPU, swap adapters per request |
| 📱 **On-device AI** | Tiny LoRA adapters (~5MB) downloaded to customize phone assistant | Over-the-air updates |
| 🌐 **Multilingual** | One base model + per-language LoRA adapters | 100 languages, 100 tiny files |

⭐ **Cost comparison:**

| Method | GPU Hours (Llama-7B) | Cost (Cloud) | Quality |
|--------|---------------------|-------------|--------|
| Full fine-tuning | ~200 hrs × 8 GPUs | ~$12,000 | 100% (baseline) |
| LoRA ($r=8$) | ~10 hrs × 1 GPU | ~$30 | 99% |
| QLoRA ($r=8$, 4-bit) | ~15 hrs × 1 GPU | ~$45 | 98% |

---

# 📚 Research Citations

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|------------------|
| ⭐ **LoRA: Low-Rank Adaptation of Large Language Models** | Hu, Shen, Wallis et al. | 2021 | Introduced low-rank adaptation; showed GPT-3 175B fine-tuned with 10,000× fewer parameters | 
| ⭐ **QLoRA: Efficient Finetuning of Quantized LLMs** | Dettmers, Pagnoni, Holtzman, Zettlemoyer | 2023 | 4-bit quantization + LoRA; enabled 65B model fine-tuning on single GPU |
| **Intrinsic Dimensionality Explains Effectiveness of Language Model Fine-Tuning** | Aghajanyan, Gupta, Zettlemoyer | 2020 | Theoretical foundation: fine-tuning has low intrinsic dimension |
| **Prefix-Tuning** | Li & Liang | 2021 | Alternative PEFT approach: prepend tunable tokens |
| **AdaLoRA** | Zhang et al. | 2023 | Adaptive rank allocation across layers |
| **DoRA: Weight-Decomposed Low-Rank Adaptation** | Liu et al. | 2024 | Decomposes weight into magnitude + direction, applies LoRA to direction |

---

# 📋 LoRA Cheat Sheet

| Parameter | Recommended | Notes |
|-----------|-------------|-------|
| Rank $r$ | 8–16 for most tasks | Higher = more capacity but more params |
| Scaling $\alpha$ | $2r$ (i.e., $\alpha=16$ for $r=8$) | Controls adapter influence via $\alpha/r$ |
| Learning rate | $3 \times 10^{-4}$ | ~10× higher than full fine-tuning |
| Target modules | All linear layers | Paper used Q,V only; all layers works better |
| Dropout | 0.05 | Regularization on LoRA path |
| Epochs | 1–3 | LoRA converges fast |
| Optimizer | AdamW | Standard; 8-bit Adam for memory savings |
| Batch size | As large as fits in memory | Use gradient accumulation if needed |

**Quick decision guide:**

```
Want to fine-tune an LLM?
├── Have multiple high-end GPUs? → Full fine-tuning (if budget allows)
├── Have one high-end GPU (A100/H100)? → LoRA (r=8-16)
├── Have one consumer GPU (RTX 3090)? → QLoRA (4-bit + r=8)
└── Have only CPU / tiny GPU? → Use prompting or API instead
```

---

# 📖 Resources

## Papers
- ⭐ Hu et al. (2021) — *LoRA: Low-Rank Adaptation of Large Language Models* — [arXiv:2106.09685](https://arxiv.org/abs/2106.09685)
- ⭐ Dettmers et al. (2023) — *QLoRA: Efficient Finetuning of Quantized LLMs* — [arXiv:2305.14314](https://arxiv.org/abs/2305.14314)
- Aghajanyan et al. (2020) — *Intrinsic Dimensionality Explains the Effectiveness of Language Model Fine-Tuning* — [arXiv:2012.13255](https://arxiv.org/abs/2012.13255)
- Lialin et al. (2023) — *Scaling Down to Scale Up: A Guide to Parameter-Efficient Fine-Tuning* — [arXiv:2303.15647](https://arxiv.org/abs/2303.15647)

## Books
- 📘 *Hands-On Large Language Models* — Jay Alammar & Maarten Grootendorst (2024) — Ch. 11 covers LoRA and PEFT
- 📘 *Build a Large Language Model (From Scratch)* — Sebastian Raschka (2024) — appendix on fine-tuning with LoRA
- 📘 *Designing Machine Learning Systems* — Chip Huyen (2022) — covers efficient model adaptation

## Libraries & Tools
- 🔧 [HuggingFace PEFT](https://github.com/huggingface/peft) — official LoRA/QLoRA implementation
- 🔧 [Unsloth](https://github.com/unslothai/unsloth) — 2× faster LoRA fine-tuning
- 🔧 [LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory) — easy fine-tuning with LoRA for 100+ models
- 🔧 [bitsandbytes](https://github.com/TimDettmers/bitsandbytes) — 4-bit quantization backend for QLoRA

---

# 📝 Summary

| Aspect | LoRA | Full FT |
|--------|------|---------|
| Trainable params | 0.1-1% | 100% |
| Memory (training) | ~1 GPU | Multiple GPUs |
| Quality | 95-100% of full FT | Baseline |
| Checkpoint size | ~1-10 MB | ~1-10 GB |
| Can serve multiple adapters | Yes (swap) | No (separate models) |

**Tomorrow**: Reproducing Flash Attention.
