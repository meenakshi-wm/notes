📘 DAY 22 — Fine-tuning LLMs: LoRA, QLoRA & Parameter-Efficient Methods

> *"You don't need to teach a doctor English — they already speak it. You just hand them a medical textbook."* 🩺📖
> That's fine-tuning in a nutshell.

Goal:

Master parameter-efficient fine-tuning (PEFT) — how to adapt large pre-trained models to specific tasks without training all parameters. Understand LoRA's low-rank decomposition, QLoRA's quantized approach, and when to use full fine-tuning vs PEFT.

---

## 🗺️ What You'll Learn Today (Roadmap)

| # | Topic | One-Liner |
|---|-------|-----------|
| 1️⃣ | Why Fine-tuning Matters | Why we can't just use the base model as-is |
| 2️⃣ | LoRA | The "sticky notes on a textbook" trick |
| 3️⃣ | Which Layers to LoRA | Where to place those sticky notes |
| 4️⃣ | QLoRA | Same sticky notes, but on a *compressed* book |
| 5️⃣ | Training with LoRA/QLoRA | Hands-on code walkthrough |
| 6️⃣ | Merging LoRA Weights | Permanently binding the notes into the book |
| 7️⃣ | Other PEFT Methods | Prefix tuning, adapters, prompt tuning |
| 8️⃣ | Hyperparameter Guide | The knobs you actually turn |

---

# 1️⃣ Why Fine-tuning Matters

## 🧠 The Big Picture Analogy

Imagine you've hired someone who is **fluent in English** and has read the entire internet. That's a pre-trained LLM like GPT or LLaMA.

Now you need them to answer **medical questions** like a doctor. You have two options:

| Option | What It Means | Real-World Name |
|--------|--------------|-----------------|
| 🔴 Train from scratch | Teach them English AND medicine from birth | Pre-training (costs millions $$$) |
| 🟢 Give them a medical textbook | They already speak English — just add domain knowledge | **Fine-tuning** (costs hundreds $) |

⭐ **Key insight**: Fine-tuning is teaching **new skills** to someone who already has **general intelligence**.

## Pre-training vs Fine-tuning

**Pre-training**: Learn general language understanding on massive data (expensive)
**Fine-tuning**: Adapt to specific task/domain on smaller data (affordable)

> 🍳 **Cooking analogy**: Pre-training = learning to cook from scratch over 10 years. Fine-tuning = a master chef learning a new cuisine in a weekend workshop.

## 💰 The Cost Problem: Why Not Just Fine-Tune Everything?

Full fine-tuning of a 70B model:
- Memory: ~280GB (fp32) or ~140GB (fp16) just for parameters
- Plus optimizer states: 2× more (Adam m and v)
- Total: ~420GB+ → needs 6+ A100-80GB GPUs

> 💡 At ~$2/hr per A100 on cloud, that's **$12/hour minimum** just for the hardware — and training runs take days!

| What You're Fine-Tuning | GPU Memory Needed | Approx Cloud Cost/hr |
|--------------------------|-------------------|---------------------|
| GPT-2 (124M) | ~2GB | ~$0.50 |
| LLaMA-7B (full FT) | ~56GB | ~$4 |
| LLaMA-70B (full FT) | ~420GB+ | ~$12+ |
| LLaMA-7B with **QLoRA** | ~6GB | ~$0.50 🎉 |

PEFT makes this feasible on a SINGLE GPU.

⭐ **Why this matters**: PEFT democratizes AI. A grad student with a single GPU can now fine-tune models that previously required a Google-scale data center.

---

# 2️⃣ LoRA: Low-Rank Adaptation (Hu et al., 2021)

## 🎯 The "Sticky Notes" Analogy

Imagine you have a **700-page textbook** (the pre-trained model). Full fine-tuning means **rewriting every single page**. That's expensive and slow.

LoRA says: *"What if we just add small sticky notes to the important pages?"* 📝

- The **textbook stays unchanged** (frozen weights)
- The **sticky notes are tiny** (low-rank matrices — a fraction of the original size)
- They **modify what you read** just enough to change the model's behavior
- You can **peel them off** anytime and the original textbook is untouched!

⭐ **This is the most important idea in modern fine-tuning.** Hu et al. (2021) showed that these tiny "notes" capture **97%+ of the performance** of rewriting the entire book.

## The Key Insight

During fine-tuning, the weight update ΔW has LOW RANK.
Instead of updating the full weight matrix, decompose the update:

ΔW = A × B

Where:
- W ∈ ℝ^(d_out × d_in): original weight matrix (frozen)
- A ∈ ℝ^(d_out × r): low-rank matrix (trainable)
- B ∈ ℝ^(r × d_in): low-rank matrix (trainable)
- r << min(d_out, d_in): the rank (typically 4-64)

Forward pass:
h = Wx + (α/r) × ABx

Where α is a scaling factor.

### 📐 The Math in LaTeX

The original weight matrix $W_0 \in \mathbb{R}^{d \times d}$ is **frozen**. We add a low-rank update:

$$W = W_0 + \Delta W = W_0 + BA$$

where:
- $B \in \mathbb{R}^{d \times r}$ — a tall, skinny matrix (trainable)
- $A \in \mathbb{R}^{r \times d}$ — a short, wide matrix (trainable)
- $r \ll d$ — the **rank** (typically 4–64, while $d$ could be 4096+)

The forward pass becomes:

$$h = W_0 x + \frac{\alpha}{r} \cdot B A x$$

where $\alpha$ is a scaling hyperparameter that controls how much the adapter influences the output.

> 🎨 **Visual intuition**: Think of $B$ and $A$ as a **bottleneck**. Information flows through a narrow pipe of width $r$, which forces the update to be simple and structured — not random noise.

### 🔢 How Many Parameters Does LoRA Add?

For a single weight matrix of size $d \times d$:

$$\text{LoRA params per layer} = 2 \times d \times r$$

This is because we have matrix $B$ ($d \times r$) and matrix $A$ ($r \times d$).

**Comparison**:
- Full update: $d^2$ parameters
- LoRA update: $2dr$ parameters
- Ratio: $\frac{2dr}{d^2} = \frac{2r}{d}$

For $d = 4096$ and $r = 16$: ratio $= \frac{2 \times 16}{4096} = 0.0078$ → **less than 1%!** 🎉

## Parameter Savings

Standard: d_out × d_in parameters
LoRA: (d_out + d_in) × r parameters

For LLaMA-7B attention projection (d=4096):
- Full: 4096 × 4096 = 16.7M parameters
- LoRA (r=16): (4096 + 4096) × 16 = 131K parameters (128× fewer!)

> 🏋️ **Gym analogy**: Full fine-tuning trains every muscle in the body. LoRA only trains the specific muscles you need for a new sport — and it turns out that's usually enough.

## Implementation

```python
import torch
import torch.nn as nn
import math

class LoRALinear(nn.Module):
    """Linear layer with LoRA adapter."""
    
    def __init__(self, original_linear, rank=16, alpha=32, dropout=0.05):
        super().__init__()
        self.original = original_linear
        self.original.weight.requires_grad = False  # Freeze original
        if self.original.bias is not None:
            self.original.bias.requires_grad = False
        
        d_out, d_in = original_linear.weight.shape
        self.rank = rank
        self.alpha = alpha
        self.scaling = alpha / rank
        
        # LoRA matrices
        self.lora_A = nn.Parameter(torch.zeros(d_in, rank))
        self.lora_B = nn.Parameter(torch.zeros(rank, d_out))
        self.lora_dropout = nn.Dropout(dropout)
        
        # Initialize A with Kaiming, B with zeros (so ΔW starts at 0)
        nn.init.kaiming_uniform_(self.lora_A, a=math.sqrt(5))
        nn.init.zeros_(self.lora_B)
    
    def forward(self, x):
        # Original forward (frozen)
        original_out = self.original(x)
        
        # LoRA forward
        lora_out = self.lora_dropout(x) @ self.lora_A @ self.lora_B * self.scaling
        
        return original_out + lora_out

def apply_lora(model, rank=16, alpha=32, target_modules=['q_proj', 'v_proj']):
    """Apply LoRA to specified modules in a model."""
    for name, module in model.named_modules():
        if any(target in name for target in target_modules):
            if isinstance(module, nn.Linear):
                parent_name = '.'.join(name.split('.')[:-1])
                child_name = name.split('.')[-1]
                parent = model.get_submodule(parent_name)
                
                lora_module = LoRALinear(module, rank=rank, alpha=alpha)
                setattr(parent, child_name, lora_module)
    
    # Count trainable parameters
    trainable = sum(p.numel() for p in model.parameters() if p.requires_grad)
    total = sum(p.numel() for p in model.parameters())
    print(f"Trainable: {trainable:,} / {total:,} ({100*trainable/total:.2f}%)")
    
    return model
```

> 💡 **Why initialize B to zeros?** So at the start of training, $\Delta W = BA = 0$, meaning the model behaves **exactly** like the original. Training gradually "grows" the adaptation from nothing.

### 🔬 Deep Dive: Why Low-Rank Works

Aghajanyan et al. (2020) showed in *"Intrinsic Dimensionality Explains the Effectiveness of Language Model Fine-Tuning"* that pre-trained models have a **very low intrinsic dimensionality**. This means:

- Even though a 7B model has 7 billion parameters, the useful "directions" for fine-tuning live in a **tiny subspace**
- LoRA exploits this by only learning updates in a low-rank subspace
- This is why rank $r = 16$ works almost as well as $r = 4096$

---

# 3️⃣ Which Layers to LoRA?

## 🎯 The Strategy Question

Think of a transformer like an office building 🏢. Each floor (layer) has different departments:
- **Q, K, V projections** = The "thinking" departments (attention)
- **O projection** = The "summarizing" department
- **FFN (gate, up, down)** = The "doing" departments (feed-forward)

Which departments do you retrain? That depends on your budget and how different the new task is.

## Standard Choices

| Target | Effect | Analogy |
|--------|--------|---------|
| Q, V projections | Standard LoRA (minimal, effective) | 📝 Adding notes to the key pages |
| Q, K, V, O projections | Better for complex tasks | 📝 Notes on every chapter |
| Q, K, V, O + FFN | Most expressive, still much cheaper than full FT | 📝 Notes + new appendices |
| All linear layers | Maximum adaptation | 📝 Notes on literally every page |

> ⭐ **Rule of thumb from practice**: For most instruction-tuning tasks, targeting **Q, K, V, O + FFN** (all linear layers) with rank 16–32 hits the sweet spot.

## Using PEFT Library (HuggingFace)

```python
from peft import LoraConfig, get_peft_model, TaskType

# Configure LoRA
lora_config = LoraConfig(
    task_type=TaskType.CAUSAL_LM,
    r=16,                     # Rank
    lora_alpha=32,            # Scaling factor
    lora_dropout=0.05,        # Dropout on LoRA path
    target_modules=[          # Which modules to adapt
        "q_proj", "k_proj", "v_proj", "o_proj",
        "gate_proj", "up_proj", "down_proj",
    ],
    bias="none",              # Don't train biases
)

# Apply to model
model = get_peft_model(base_model, lora_config)
model.print_trainable_parameters()
# Output: trainable params: 13,631,488 || all params: 6,751,039,488 || trainable%: 0.20%
```

> 🏭 **Production note**: The HuggingFace `peft` library (🤗 PEFT) is the industry standard. It's used by Meta, Google, Microsoft, Stability AI, and thousands of startups. You almost never write LoRA from scratch in production — the library handles all the complexity.

### 🔄 Swapping Adapters in Production

One killer feature: you can have **multiple LoRA adapters** for one base model!

```
Base Model (LLaMA-7B) ← shared by ALL tasks
  ├── LoRA Adapter: Medical QA (~50MB)
  ├── LoRA Adapter: Legal Summarization (~50MB)
  ├── LoRA Adapter: Code Generation (~50MB)
  └── LoRA Adapter: Customer Support (~50MB)
```

> 💡 Instead of deploying **4 separate 14GB models**, you deploy **1 × 14GB base + 4 × 50MB adapters**. That's a **~4× memory saving** in production!

---

# 4️⃣ QLoRA: Quantized LoRA (Dettmers et al., 2023)

## 🎯 The "Compressed Book + Sticky Notes" Analogy

Remember our textbook analogy?

- **Full fine-tuning** = Rewriting the entire textbook (expensive, thorough)
- **LoRA** = Adding sticky notes to the full-size textbook (cheap, effective)
- **QLoRA** = Adding sticky notes to a **pocket-sized compressed edition** of the textbook 📱

The textbook is still readable (the model still works), but it takes up **way less shelf space** (GPU memory). And your sticky notes (LoRA adapters) work just the same!

⭐ **QLoRA is what made it possible for individuals to fine-tune LLaMA-65B on a single 48GB GPU.** This paper single-handedly launched the open-source fine-tuning revolution in 2023.

## The Innovation

LoRA still requires loading the full model in fp16 → 14GB for 7B.
QLoRA loads the base model in 4-bit → much less memory.

Key techniques:
1. **4-bit NormalFloat (NF4)**: information-theoretically optimal 4-bit quantization
2. **Double Quantization**: quantize the quantization constants too
3. **Paged Optimizers**: use CPU memory for optimizer states when GPU is full

### 📐 The Math Behind 4-bit Quantization

Normal 16-bit floating point uses 16 bits per parameter:
$$\text{Memory}_{fp16} = n_{\text{params}} \times 2 \text{ bytes}$$

4-bit quantization compresses each parameter to just 4 bits:
$$\text{Memory}_{4bit} = n_{\text{params}} \times 0.5 \text{ bytes}$$

That's a **4× compression**! For a 7B model:
- fp16: $7 \times 10^9 \times 2 = 14\text{GB}$
- 4-bit: $7 \times 10^9 \times 0.5 = 3.5\text{GB}$

> 🧊 **Compression analogy**: It's like converting a WAV music file to MP3. You lose a tiny bit of quality, but the file is 10× smaller. NF4 is carefully designed so the "quality loss" is minimal for neural network weights.

## Memory Comparison (7B model)

| Method | GPU Memory | Analogy |
|--------|-----------|---------|
| Full fine-tuning (fp16) | ~56GB | 🏠 Need a mansion |
| LoRA (fp16 base) | ~14GB | 🏡 Need a house |
| QLoRA (4-bit base) | ~6GB | 🏕️ Just need a tent! |

> 🎉 **6GB means it fits on a consumer RTX 3060!** That's a ~$300 graphics card.

## Implementation with BitsAndBytes

```python
from transformers import AutoModelForCausalLM, BitsAndBytesConfig
from peft import prepare_model_for_kbit_training

# 4-bit quantization config
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",           # NormalFloat4
    bnb_4bit_compute_dtype=torch.bfloat16, # Compute in bf16
    bnb_4bit_use_double_quant=True,       # Double quantization
)

# Load model in 4-bit
model = AutoModelForCausalLM.from_pretrained(
    "meta-llama/Llama-2-7b-hf",
    quantization_config=bnb_config,
    device_map="auto",
)

# Prepare for training
model = prepare_model_for_kbit_training(model)

# Apply LoRA
model = get_peft_model(model, lora_config)
```

### 🏭 Production Impact: The Alpaca / Vicuna Revolution

QLoRA directly enabled:

| Project | What It Did | Cost |
|---------|------------|------|
| **Stanford Alpaca** | Fine-tuned LLaMA-7B on 52K instruction examples | ~$600 |
| **Vicuna** | Fine-tuned LLaMA-13B on ShareGPT conversations | ~$300 |
| **Guanaco** | QLoRA fine-tuned LLaMA-65B on single GPU | ~$100 in 24hrs |
| **Orca** | Fine-tuned on chain-of-thought reasoning traces | ~$200 |

> Before QLoRA, these experiments would have cost **$10,000+** each with full fine-tuning.

---

# 5️⃣ Training with LoRA/QLoRA

## 📋 Data Formatting: The Instruction/Response Pattern

Before we train, we need to talk about **how the training data looks**. LLMs learn by example, so the formatting matters enormously.

> 🍽️ **Restaurant analogy**: If you're training a waiter, you show them examples of "Customer says X → Waiter responds Y." That's exactly what instruction tuning data looks like.

The standard format (Alpaca-style):

```
### Instruction:
Summarize the following medical report in plain English.

### Input:
Patient presents with acute myocardial infarction...

### Response:
The patient had a heart attack...
```

⭐ **Important**: The model learns to generate ONLY the Response part. The Instruction and Input are the "prompt" — the model learns what kind of outputs to produce given certain inputs.

```python
from transformers import TrainingArguments, Trainer
from datasets import load_dataset

# Load and format dataset
dataset = load_dataset("tatsu-lab/alpaca")

def format_instruction(example):
    if example['input']:
        prompt = f"### Instruction:\n{example['instruction']}\n\n### Input:\n{example['input']}\n\n### Response:\n{example['output']}"
    else:
        prompt = f"### Instruction:\n{example['instruction']}\n\n### Response:\n{example['output']}"
    return {'text': prompt}

dataset = dataset.map(format_instruction)

# Training config
training_args = TrainingArguments(
    output_dir="./lora_output",
    num_train_epochs=3,
    per_device_train_batch_size=4,
    gradient_accumulation_steps=4,
    learning_rate=2e-4,           # Higher LR for LoRA (only training small adapter)
    weight_decay=0.01,
    warmup_steps=100,
    lr_scheduler_type="cosine",
    logging_steps=10,
    save_strategy="epoch",
    fp16=True,
    gradient_checkpointing=True,  # Save memory
    optim="paged_adamw_8bit",     # Memory-efficient optimizer
)

# Train
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=dataset['train'],
    tokenizer=tokenizer,
)
trainer.train()

# Save LoRA weights (just the adapter, ~50MB)
model.save_pretrained("./lora_adapter")
```

### 🔑 Key Hyperparameter Insight: Learning Rate

> ⚠️ **This trips up beginners constantly**: The learning rate for LoRA is **10–100× higher** than for full fine-tuning!

| Method | Typical Learning Rate | Why? |
|--------|----------------------|------|
| Full fine-tuning | 1e-5 to 5e-5 | Updating billions of params — need to be gentle |
| LoRA / QLoRA | 1e-4 to 3e-4 | Only updating ~0.1% of params — can be aggressive |

> 🚗 **Car analogy**: Full fine-tuning is like steering an 18-wheeler — tiny adjustments or you crash. LoRA is like steering a bicycle — you can make sharp turns easily.

### ⚠️ Catastrophic Forgetting: The Hidden Danger

When you fine-tune a model too aggressively, it can **forget** what it already knew!

> 🌍 **Language analogy**: Imagine you spend 6 months learning Italian immersion-style. When you come back, your Spanish is rusty. You "forgot" Spanish by "overwriting" it with Italian. That's catastrophic forgetting.

**How to prevent it**:

| Strategy | How It Works |
|----------|-------------|
| Low learning rate | Gentle updates don't overwrite old knowledge |
| Short training (1–3 epochs) | Less time to forget |
| LoRA / PEFT | Only changes ~0.1% of parameters — the other 99.9% stay intact! ⭐ |
| Data mixing | Include some general data alongside domain data |
| Regularization | Weight decay, dropout keep weights close to original |

⭐ **This is another reason PEFT is so powerful**: By freezing 99.9% of parameters, catastrophic forgetting is almost eliminated by design.

---

# 6️⃣ Merging LoRA Weights

## 🧩 The "Permanent Binding" Analogy

Remember our sticky notes? After you've written them, you have a choice:

1. **Keep them as sticky notes** → Easy to swap, remove, or add more (modular)
2. **Print a new edition with the notes incorporated** → Faster to read, but now they're permanent (merged)

After fine-tuning, you can MERGE LoRA weights into the base model:

### Merging: Bake It In

Mathematically, merging is simple:

$$W_{\text{merged}} = W_0 + \frac{\alpha}{r} \cdot BA$$

After merging, the model is **identical in architecture** to the original — no LoRA overhead at inference. It's just a regular model with slightly different weights.

```python
# Merge for faster inference (no LoRA overhead)
merged_model = model.merge_and_unload()
merged_model.save_pretrained("./merged_model")

# Or keep separate for modularity (swap adapters)
# Load base model + adapter at inference:
from peft import PeftModel
base_model = AutoModelForCausalLM.from_pretrained("meta-llama/Llama-2-7b-hf")
model = PeftModel.from_pretrained(base_model, "./lora_adapter")
```

### 🏭 When to Merge vs Keep Separate

| Scenario | Merge? | Why? |
|----------|--------|------|
| Single task, maximum speed | ✅ Yes | No adapter overhead |
| Multiple tasks, one base model | ❌ No | Swap adapters on the fly |
| A/B testing two fine-tunes | ❌ No | Easy to compare |
| Shipping to production (one model) | ✅ Yes | Simpler deployment |
| Research / experimentation | ❌ No | Keep options open |

> 💾 **Size perspective**: A merged 7B model is ~14GB. An unmerged adapter is ~50MB. If you're serving 10 different tasks, storing adapters saves **~130GB** of disk.

---

# 7️⃣ Other PEFT Methods

> 🧰 LoRA is the most popular PEFT method, but it's not the only one. Here's the full toolbox:

## Prefix Tuning

> 📌 **Analogy**: Instead of modifying the textbook, you add a **custom table of contents** at the beginning that guides how the model reads everything that follows.

Prepend trainable "prefix" tokens to attention keys and values.
```
[TRAINABLE_PREFIX_TOKENS] + [ACTUAL_INPUT_TOKENS]
```
Only prefix parameters are trained.

**How it works**: The prefix tokens create new key-value pairs in attention. The model attends to these "virtual" tokens alongside the real input, steering its behavior.

Introduced by: **Li & Liang (2021)**, *"Prefix-Tuning: Optimizing Continuous Prompts for Generation"*

## Adapters (Houlsby et al., 2019)

> 🔌 **Analogy**: Instead of rewriting chapters, you insert **small summary cards** between chapters that transform the information as it flows through.

Insert small trainable bottleneck layers between transformer blocks:
```
Input → Down-project → Activation → Up-project → Add residual
```

Mathematically, an adapter layer computes:

$$h \leftarrow h + f(h W_{\text{down}}) W_{\text{up}}$$

Where $W_{\text{down}} \in \mathbb{R}^{d \times m}$ and $W_{\text{up}} \in \mathbb{R}^{m \times d}$ with bottleneck dimension $m \ll d$.

> 📚 **Historical note**: Adapters (Houlsby et al., 2019) came **before** LoRA and were the first popular PEFT method. LoRA later showed you could achieve similar results without adding any layers — just modifying existing weights.

## Prompt Tuning (Lester et al., 2021)

> 🏷️ **Analogy**: Instead of modifying the textbook at all, you write a **perfect question** that makes the textbook give the right answers. The question itself is learned!

Learn continuous "soft prompts" (virtual tokens prepended to input).
Even fewer parameters than LoRA.

**How it works**: Instead of discrete words like "Translate this:", you learn **continuous vectors** $[p_1, p_2, ..., p_k]$ that are prepended to the input embeddings. These vectors don't correspond to any real words — they live in continuous embedding space.

> ⭐ **Fun fact**: Liu et al. (2022) showed that with enough prompt tokens (~100), prompt tuning can match full fine-tuning performance on many tasks, with **< 0.01% trainable parameters**.

## Comparison

| Method | Trainable % | Quality | Memory | Inference Cost | Best For |
|--------|------------|---------|--------|---------------|----------|
| Full FT | 100% | Highest | Highest | Same as base | Maximum quality, unlimited budget |
| LoRA | 0.1-1% | Near full FT | Low | ~Same (after merge) | ⭐ Best all-around choice |
| QLoRA | 0.1-1% | Near full FT | Lowest | Same (after merge) | ⭐ When GPU memory is limited |
| Prefix Tuning | <0.1% | Good | Very low | Slight overhead | Lightweight adaptation |
| Prompt Tuning | <0.01% | Moderate | Minimal | Slight overhead | Extreme parameter efficiency |
| Adapters | 1-5% | Good | Low | Slight overhead | When you need layer-level control |

> 🏆 **In 2024-2025, LoRA and QLoRA dominate**. They offer the best quality-to-cost ratio and are supported by all major frameworks.

---

# 8️⃣ LoRA Hyperparameter Guide

> 🎛️ Think of these as the **control knobs on a mixing board**. Each one changes the sound (behavior) in a specific way.

| Parameter | Range | Rule of Thumb | 🔍 What It Actually Does |
|-----------|-------|--------------|--------------------------|
| rank (r) | 4-128 | 16-32 for most tasks; 64+ for complex | Controls how "expressive" the adapter is. Higher = more capacity but more params |
| alpha | 16-64 | Often 2× rank | Scaling factor: $\text{scaling} = \frac{\alpha}{r}$. Higher = stronger adaptation |
| dropout | 0-0.1 | 0.05 standard | Regularization to prevent overfitting the small adapter |
| LR | 1e-4 to 3e-4 | 2e-4 is common (higher than full FT) | How fast to learn. LoRA uses **10×** higher LR than full FT |
| Target modules | Q,V to all | More modules = better but slower | Which weight matrices get adapters |
| Epochs | 1-5 | 3 is common for instruction tuning | How many passes through the data |

### 📐 The Rank-Quality Tradeoff (LaTeX)

The total trainable parameters for LoRA across $L$ layers, each with $k$ target modules of dimension $d$:

$$\text{Total LoRA params} = L \times k \times 2 \times d \times r$$

For LLaMA-7B with 32 layers, 7 target modules, $d = 4096$:
- $r = 8$: $32 \times 7 \times 2 \times 4096 \times 8 = 14.7\text{M}$ (0.21%)
- $r = 16$: $32 \times 7 \times 2 \times 4096 \times 16 = 29.4\text{M}$ (0.42%)
- $r = 64$: $32 \times 7 \times 2 \times 4096 \times 64 = 117.4\text{M}$ (1.68%)

> ⭐ **Biderman et al. (2024)** found that for most tasks, increasing rank beyond 32 gives **diminishing returns**. But for very different domains (e.g., English model → Chinese), higher ranks help.

### 🧪 Quick Decision Guide

```
Is your task similar to what the model already does?
├── YES (e.g., general chat → customer support chat)
│   └── r=8-16, alpha=16-32, 1-2 epochs
├── SOMEWHAT (e.g., general model → medical QA)
│   └── r=16-32, alpha=32-64, 2-3 epochs
└── VERY DIFFERENT (e.g., English model → code generation)
    └── r=32-64, alpha=64-128, 3-5 epochs
```

---

# 📝 Summary

| Concept | Key Insight | 🎯 Beginner Analogy |
|---------|-------------|---------------------|
| LoRA | Decompose weight update as low-rank A×B | 📝 Sticky notes on a textbook |
| QLoRA | 4-bit base model + LoRA = fine-tune 70B on single GPU | 📱 Sticky notes on a pocket edition |
| Rank r | Controls expressiveness; 16-32 for most tasks | 🎚️ Volume knob — higher = louder adaptation |
| Alpha | Scaling factor; usually 2×r | 🔊 Amplifier for the adapter signal |
| Target modules | More = better adaptation, but more parameters | 📍 Which pages get sticky notes |
| Merging | Fold LoRA into base for free inference | 📖 Printing a new edition with notes built in |
| Memory | QLoRA: 4-6GB for 7B model fine-tuning | 🏕️ A tent instead of a mansion |
| Catastrophic Forgetting | Over-training forgets old knowledge | 🌍 Learning Italian, forgetting Spanish |
| PEFT | Modify 0.1% of parameters instead of 100% | 🎯 Laser surgery instead of full-body operation |

---

# 🏭 Production Scenarios & Real-World Applications

## Scenario 1: Medical Domain Adaptation 🏥

**Problem**: A hospital wants GPT-style model to answer medical questions accurately.

```
Base model: LLaMA-2-7B (general knowledge)
Training data: 50K medical QA pairs (PubMedQA, MedQA)
Method: QLoRA (r=32, alpha=64)
Hardware: Single RTX 4090 (24GB)
Training time: ~8 hours
Result: Medical accuracy jumps from 42% → 71%
```

## Scenario 2: Legal Document Summarization ⚖️

**Problem**: Law firm needs model to summarize case briefs in legal language.

```
Base model: Mistral-7B
Training data: 20K case-summary pairs
Method: LoRA (r=16, alpha=32)
Cost: ~$50 on cloud GPU
Result: Summaries match junior associate quality
```

## Scenario 3: GPT-3.5 Fine-Tuning API (OpenAI) 🌐

**Problem**: Startup wants custom chatbot with specific personality/knowledge.

```python
# OpenAI's fine-tuning API (simplified — they handle LoRA internally)
# You just provide training data in JSONL format:

{"messages": [
    {"role": "system", "content": "You are a helpful cooking assistant."},
    {"role": "user", "content": "How do I make pasta?"},
    {"role": "assistant", "content": "Here's my favorite recipe..."}
]}
```

| Provider | Model | Cost to Fine-Tune | Notes |
|----------|-------|-------------------|-------|
| OpenAI | GPT-3.5-turbo | ~$8 per 1M tokens | Easiest, but closed-source |
| OpenAI | GPT-4o-mini | ~$25 per 1M tokens | Better quality |
| Together AI | LLaMA-3-8B | ~$2/hr GPU rental | Open-source, you own the weights |
| Replicate | Mistral-7B | ~$1.50/hr | Easy API |

## Scenario 4: Cost Comparison 💰

Fine-tuning LLaMA-7B on 50K examples:

| Method | Hardware | Time | Total Cost |
|--------|----------|------|------------|
| Full Fine-Tuning | 8× A100-80GB | 6 hrs | ~$96 |
| LoRA (fp16) | 1× A100-40GB | 4 hrs | ~$8 |
| QLoRA (4-bit) | 1× RTX 4090 | 8 hrs | ~$4 |
| QLoRA (4-bit) | Google Colab free | 12 hrs | **$0** 🎉 |

---

# 🔬 Deep Research & Key Papers

## The Essential Reading List

| # | Paper | Authors | Year | Key Contribution | Citation Count* |
|---|-------|---------|------|-----------------|----------------|
| 1 | **LoRA: Low-Rank Adaptation of Large Language Models** | Hu et al. | 2021 | Introduced low-rank decomposition for fine-tuning; showed $r=4$–$16$ matches full FT | ~5,000+ |
| 2 | **QLoRA: Efficient Finetuning of Quantized LLMs** | Dettmers et al. | 2023 | 4-bit NF4 quantization + LoRA; fine-tune 65B on single 48GB GPU | ~3,000+ |
| 3 | **Parameter-Efficient Transfer Learning for NLP** | Houlsby et al. | 2019 | Original adapter layers — bottleneck modules between transformer blocks | ~4,000+ |
| 4 | **Few-Shot Parameter-Efficient Fine-Tuning is Better and Cheaper than In-Context Learning** | Liu et al. | 2022 | Showed PEFT beats few-shot prompting on most benchmarks, even with minimal data | ~1,000+ |
| 5 | **LoRA vs Full Fine-Tuning: An Illusion of Equivalence** | Biderman et al. | 2024 | Showed LoRA and full FT learn **different weight structures**; LoRA may underperform on very different domains | ~200+ |
| 6 | **Intrinsic Dimensionality Explains Fine-Tuning** | Aghajanyan et al. | 2020 | Theoretical foundation: pre-trained models have low intrinsic dimensionality | ~1,500+ |
| 7 | **Prefix-Tuning: Optimizing Continuous Prompts** | Li & Liang | 2021 | Prepend trainable prefix tokens to attention | ~3,000+ |
| 8 | **The Power of Scale for Parameter-Efficient Prompt Tuning** | Lester et al. | 2021 | Soft prompt tuning — < 0.01% parameters | ~2,500+ |

*Approximate citations as of early 2025.

### 🔍 Key Findings from Research

**Hu et al. (2021) — LoRA**:
- Weight updates during fine-tuning have **intrinsically low rank**
- $r = 4$ works for simple tasks; $r = 64$ barely improves over $r = 16$ on most benchmarks
- Applying LoRA to **Q and V** projections is most efficient; adding K, O and FFN improves quality further

**Dettmers et al. (2023) — QLoRA**:
- NF4 quantization is **information-theoretically optimal** for normally distributed weights
- Double quantization saves an additional ~0.4 bits per parameter
- Guanaco (their 65B fine-tune) matches ChatGPT on Vicuna benchmark with **single GPU**

**Biderman et al. (2024) — LoRA vs Full FT**:
- ⚠️ LoRA and full fine-tuning produce **fundamentally different models** — not just "approximate" versions of each other
- LoRA is better when the task is **close to pre-training distribution**
- Full FT is better for **large distribution shifts** (new language, new domain)
- Increasing rank does NOT make LoRA converge to full FT — they learn different things

---

# 📋 Cheat Sheet: Fine-Tuning LLMs

## ⚡ Quick Reference Card

```
┌─────────────────────────────────────────────────────────┐
│           FINE-TUNING DECISION FLOWCHART                │
│                                                         │
│  Do you have >$1000 budget + multi-GPU?                 │
│  ├── YES → Full fine-tuning (maximum quality)           │
│  └── NO                                                 │
│      ├── Do you have a 24GB+ GPU?                       │
│      │   ├── YES → LoRA (fp16 base)                     │
│      │   └── NO → QLoRA (4-bit base) ⭐ MOST COMMON    │
│      └── Do you have ANY GPU?                           │
│          ├── YES (even 8GB) → QLoRA + small model       │
│          └── NO → Use OpenAI/Together API               │
└─────────────────────────────────────────────────────────┘
```

## 🔑 Key Formulas

| Formula | Meaning |
|---------|---------|
| $W = W_0 + BA$ | LoRA: frozen weights + low-rank update |
| $\text{scaling} = \frac{\alpha}{r}$ | How much the adapter affects the output |
| $\text{LoRA params} = 2 \times d \times r$ per module | Total trainable params per weight matrix |
| $\text{Memory}_{4bit} \approx \frac{n}{2}$ bytes | QLoRA memory for $n$ parameters |
| $\text{Trainable \%} = \frac{L \times k \times 2dr}{\text{Total params}} \times 100$ | Percentage of model being trained |

## 🚫 Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using full FT learning rate (1e-5) with LoRA | Use 1e-4 to 3e-4 for LoRA |
| Training too many epochs (10+) | 1–3 epochs for instruction tuning |
| Rank too high (r=256) for simple tasks | Start with r=16, increase if needed |
| Not using gradient checkpointing | Always enable — saves ~50% memory |
| Forgetting to freeze base model | PEFT library handles this automatically |
| Not formatting data properly | Use consistent instruction/response template |

## 🛠️ Essential Tools & Libraries

| Tool | What It Does | Install |
|------|-------------|---------|
| 🤗 PEFT | LoRA, adapters, prefix tuning | `pip install peft` |
| bitsandbytes | 4-bit/8-bit quantization | `pip install bitsandbytes` |
| 🤗 TRL | Training with RLHF, SFT, DPO | `pip install trl` |
| Axolotl | All-in-one fine-tuning framework | `pip install axolotl` |
| LLaMA-Factory | GUI + CLI for fine-tuning | GitHub: hiyouga/LLaMA-Factory |
| Unsloth | 2× faster LoRA training | `pip install unsloth` |

## 📊 Quick Benchmarks to Remember

| Base Model → Fine-tuned | Method | Benchmark Improvement |
|-------------------------|--------|----------------------|
| LLaMA-7B → Alpaca | LoRA | General instruction: +35% |
| LLaMA-65B → Guanaco | QLoRA | Matches ChatGPT (Vicuna bench) |
| Mistral-7B → Medical | QLoRA | Medical QA: +29% |
| GPT-3.5 → Custom | OpenAI API | Task-specific: +20-40% typically |

---

**Tomorrow**: Instruction Tuning & Chat Fine-tuning — turning a base model into an assistant.
