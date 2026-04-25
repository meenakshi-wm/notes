📘 DAY 22 — Quantization: Making Models Smaller & Faster

> 🎯 **One-Line Summary**: Model compression makes AI models smaller and faster so they can run on phones, laptops, and cheap hardware — without losing much intelligence.

Goal:

Master model quantization — reduce model size and increase inference speed by converting weights (and activations) to lower precision. Understand INT8, INT4, GPTQ, AWQ, and GGUF.

---

## 🌍 Why Does This Matter? (The Big Picture)

Imagine you've trained an incredibly smart AI model — but it's **14 gigabytes** in size. That's like trying to fit an encyclopedia into your pocket. It won't run on your phone. It barely fits on your laptop's GPU. And running it in the cloud costs a fortune.

**Model compression** is the art of making these models smaller, faster, and cheaper — while keeping them almost as smart. It's one of the most important practical skills in AI today.

> 💡 **Real-World Impact**: ChatGPT-style models that once needed $10,000 GPUs can now run on a $200 gaming card or even a MacBook, thanks to compression techniques.

### 🏢 Who Uses This?
| Company/Project | What They Do | Compression Used |
|----------------|-------------|------------------|
| Meta (LLaMA) | Open-source LLMs | GPTQ, GGUF quantization |
| Apple (CoreML) | On-device AI | Quantization + Pruning |
| Google (TFLite) | Mobile AI | Quantization + Distillation |
| llama.cpp | CPU inference | GGUF 4-bit quantization |
| Tesla | Self-driving | Pruning + Quantization |

### 🗺️ The Five Main Compression Techniques

| Technique | Analogy | What It Does |
|-----------|---------|-------------|
| **Quantization** | Rounding $19.99 → $20 💰 | Uses fewer bits to store numbers |
| **Pruning** | Trimming dead branches from a tree 🌳 | Removes unimportant connections |
| **Knowledge Distillation** | Expert teacher training a smaller student 👨‍🏫→👦 | Trains a small model to mimic a big one |
| **Low-Rank Factorization** | Summarizing a long book into key points 📖 | Replaces big matrices with smaller ones |
| **Weight Sharing** | Apartment building where units share walls 🏢 | Multiple connections reuse the same values |

> ⭐ **Key Insight**: These techniques can be **combined**! Han et al. (2015) showed in "Deep Compression" that using pruning + quantization + Huffman coding together achieved **35–49x compression** on AlexNet with no accuracy loss.

---

# 1️⃣ What Is Quantization? 🔢

> 🎯 **Beginner Analogy**: Imagine a store where everything costs something like \$19.99, \$7.43, \$12.87. Now imagine rounding ALL prices to the nearest dollar: \$20, \$7, \$13. You lose a tiny bit of precision, but your cash register can work WAY faster and your receipts are shorter. That's quantization!

**Quantization** means converting the numbers inside an AI model from high-precision (lots of decimal places) to low-precision (fewer decimal places or even whole numbers).

Map floating-point values to lower-precision integers:

FP16 weight (2 bytes) → INT8 (1 byte) → INT4 (0.5 bytes)

### 📊 What Does This Actually Save?

A 7B model:
- FP16: 14 GB
- INT8: 7 GB  
- INT4: 3.5 GB

Fits on consumer GPUs! RTX 3060 12GB can run a 7B INT4 model.

> 💡 **Why It Works**: Most of the numbers (called "weights") in a neural network don't need to be precise to 16 decimal places. Just like you don't need to know the temperature is 72.384729°F — saying "72°F" is perfectly fine for deciding what to wear.

### 🔍 Bits Explained for Beginners

| Data Type | Bits | Decimal Places | Memory per Number | Analogy |
|-----------|------|---------------|-------------------|----------|
| FP32 | 32 | ~7 digits | 4 bytes | Measuring with a laser ruler |
| FP16 | 16 | ~3 digits | 2 bytes | Measuring with a regular ruler |
| INT8 | 8 | 0 (integer) | 1 byte | Measuring with your hand span |
| INT4 | 4 | 0 (coarse integer) | 0.5 bytes | Estimating "about 3 feet" |

> ⭐ **Production Reality**: Most LLM deployments today use **4-bit quantization** (INT4). Libraries like GPTQ (Frantar et al., 2022) and AWQ (Lin et al., 2023) make this nearly lossless for chat/generation tasks.

The general quantization formula:

$$q = \text{round}\left(\frac{x}{\text{scale}}\right) + \text{zero\_point}$$

Where:
- $x$ = original floating-point weight
- $\text{scale}$ = determines the step size between quantized values
- $\text{zero\_point}$ = offset to handle asymmetric ranges
- $q$ = the quantized (compressed) integer value

---

# 2️⃣ Quantization Math 🧮

> 🎯 **Don't Panic!** The math here is just about converting big numbers to small numbers and back. Think of it like converting dollars to cents and back — you just multiply or divide by 100.

## Symmetric Quantization

> 💡 **Analogy**: Like a thermometer centered at 0° — same range in both directions.

$$x_{\text{quant}} = \text{round}\left(\frac{x}{\text{scale}}\right)$$

$$x_{\text{dequant}} = x_{\text{quant}} \times \text{scale}$$

$$\text{scale} = \frac{\max(|x|)}{2^{b-1} - 1}$$

Where $b$ = number of bits (e.g., 8 for INT8).

## Asymmetric Quantization

> 💡 **Analogy**: Like a thermometer where the range isn't centered — maybe 10° to 90°. We need to shift (zero_point) before scaling.

$$x_{\text{quant}} = \text{round}\left(\frac{x - \text{zero\_point}}{\text{scale}}\right)$$

$$x_{\text{dequant}} = x_{\text{quant}} \times \text{scale} + \text{zero\_point}$$

$$\text{scale} = \frac{\max(x) - \min(x)}{2^b - 1}$$

$$\text{zero\_point} = \text{round}\left(\frac{-\min(x)}{\text{scale}}\right)$$

### 🔢 Worked Example for Beginners

Suppose we have 4 weights: $[1.2, -0.5, 3.1, -2.8]$ and want INT8 symmetric quantization ($b = 8$):

1. Find $\max(|x|) = 3.1$
2. Compute $\text{scale} = \frac{3.1}{127} = 0.0244$
3. Quantize: $[1.2/0.0244, -0.5/0.0244, 3.1/0.0244, -2.8/0.0244] = [49, -20, 127, -115]$
4. Dequantize: $[49 \times 0.0244, ...] = [1.196, -0.488, 3.099, -2.806]$ ← Very close to originals!

> ⭐ **The error is tiny** — that's why quantization works so well in practice.

```python
def quantize_symmetric(tensor, bits=8):
    """Symmetric per-tensor quantization."""
    qmin, qmax = -(2**(bits-1)), 2**(bits-1) - 1
    scale = tensor.abs().max() / qmax
    quantized = torch.clamp(torch.round(tensor / scale), qmin, qmax).to(torch.int8)
    return quantized, scale

def dequantize(quantized, scale):
    return quantized.float() * scale
```

---

# 3️⃣ Granularity of Quantization

| Granularity | Shared Scale | Quality | Speed |
|-------------|-------------|---------|-------|
| Per-tensor | 1 scale for all | Lowest | Fastest |
| Per-channel | 1 scale per output channel | Good | Fast |
| Per-group | 1 scale per g elements (g=128) | Best | Moderate |
| Per-token | 1 scale per token | Good for activations | Fast |

Per-group (group size 128) is the standard for weight-only quantization.

---

# 4️⃣ Post-Training Quantization (PTQ)

Quantize after training. No retraining needed.

### GPTQ (Frantar et al., 2023)
Layer-by-layer quantization using calibration data:

```python
# Pseudo-code for GPTQ
def gptq_quantize_layer(W, X_cal, bits=4, group_size=128):
    """Quantize weight matrix W using calibration inputs X_cal."""
    H = X_cal.T @ X_cal  # Hessian approximation
    
    for col in range(W.shape[1]):
        # Quantize one column at a time
        w = W[:, col]
        q = quantize_to_grid(w, bits, group_size)
        
        # Compute quantization error
        error = (w - q) / H[col, col]
        
        # Compensate: adjust remaining columns
        W[:, col+1:] -= error[:, None] * H[col, col+1:]
        W[:, col] = q
```

### AWQ (Lin et al., 2023)
Keep salient weights in higher precision based on activation magnitudes:

```python
# AWQ key insight: not all weights are equally important
# Weight channels connected to high-magnitude activations matter more
def awq_quantize(W, X_cal, bits=4):
    # Compute activation-aware importance
    importance = X_cal.abs().mean(dim=0)  # per-channel importance
    
    # Scale important weights up before quantization
    scale = (importance / importance.mean()).clamp(min=1.0)
    W_scaled = W * scale[None, :]
    
    # Quantize the scaled weights
    W_q = standard_quantize(W_scaled, bits)
    
    # At inference: W_q / scale to compensate
    return W_q, scale
```

---

# 5️⃣ Practical Quantization with Libraries

### bitsandbytes (Dettmers)
```python
from transformers import AutoModelForCausalLM, BitsAndBytesConfig

# 4-bit quantization (NF4)
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_compute_dtype=torch.bfloat16,
    bnb_4bit_quant_type="nf4",  # Normal Float 4
    bnb_4bit_use_double_quant=True,  # Quantize the quantization constants
)

model = AutoModelForCausalLM.from_pretrained(
    "meta-llama/Llama-2-7b-hf",
    quantization_config=bnb_config,
    device_map="auto",
)
# 7B model now uses ~3.5GB VRAM
```

### GPTQ with AutoGPTQ
```python
from auto_gptq import AutoGPTQForCausalLM

model = AutoGPTQForCausalLM.from_quantized(
    "TheBloke/Llama-2-7B-GPTQ",
    device="cuda:0",
    use_triton=True,
)
```

### GGUF (llama.cpp)
```bash
# Convert to GGUF for CPU inference
python convert.py model/ --outtype f16
./quantize model-f16.gguf model-q4_K_M.gguf Q4_K_M
# Runs on CPU with AVX2/ARM NEON acceleration!
```

---

# 6️⃣ Quality vs Size Tradeoff

| Quantization | Size (7B) | Perplexity (Wiki) | Relative Quality |
|-------------|-----------|-------------------|-----------------|
| FP16 | 14 GB | 5.68 (baseline) | 100% |
| INT8 | 7 GB | 5.70 | ~99.5% |
| INT4 (GPTQ) | 3.5 GB | 5.85 | ~97% |
| INT4 (AWQ) | 3.5 GB | 5.78 | ~98% |
| INT3 | 2.6 GB | 6.50 | ~90% |
| INT2 | 1.75 GB | 8.0+ | <80% |

Sweet spot: **INT4** with group quantization. Minimal quality loss.

---

# 7️⃣ Key Quantization Formats

| Format | Description | Use Case |
|--------|------------|----------|
| GPTQ | Layer-wise weight PTQ | GPU inference |
| AWQ | Activation-aware weight quant | GPU inference |
| GGUF | CPU-optimized format | llama.cpp, CPU/Mac |
| NF4 | Normal Float 4-bit | QLoRA training |
| FP8 | 8-bit float (H100) | Training + inference |
| SmoothQuant | Activation smoothing for W8A8 | INT8 inference |

---

# 8️⃣ Pruning: Trimming the Fat ✂️🌳

> 🎯 **Beginner Analogy**: Imagine a tree with hundreds of branches. Some branches are dead — they have no leaves and produce no fruit. Trimming those dead branches doesn't hurt the tree at all, but makes it lighter and healthier. **Pruning** does the same for neural networks!

**Pruning** removes unnecessary connections (weights) from a neural network. Many weights are very close to zero and contribute almost nothing to the output.

### How Pruning Works

1. **Train** the model normally
2. **Identify** unimportant weights (usually the smallest ones)
3. **Remove** them (set to zero)
4. **Fine-tune** the remaining weights

### 📐 Pruning Sparsity Formula

$$s = \frac{\text{number of zeros}}{\text{total parameters}}$$

Where $s$ = sparsity ratio. A sparsity of 0.9 means 90% of weights are zero!

> 💡 **Example**: A model with 1 billion parameters at 90% sparsity only has 100 million non-zero parameters — **10x fewer** computations needed.

### Types of Pruning

| Type | What's Removed | Granularity | Hardware Support |
|------|---------------|-------------|------------------|
| **Unstructured** | Individual weights | Fine-grained | Needs sparse hardware |
| **Structured** | Entire neurons/filters | Coarse-grained | Works on any GPU |
| **Block sparse** | Small blocks (e.g., 2:4) | Medium | NVIDIA A100/H100 |

> ⭐ **NVIDIA 2:4 Sparsity**: The A100 and H100 GPUs have hardware support for "2:4 sparsity" — out of every 4 weights, 2 must be zero. This gives **2x speedup** with minimal quality loss. (Mishra et al., 2021)

### 🏭 Production Example
```python
import torch.nn.utils.prune as prune

# Remove 40% of smallest weights in a linear layer
prune.l1_unstructured(model.layer, name='weight', amount=0.4)

# Check sparsity
sparsity = (model.layer.weight == 0).float().mean()
print(f"Sparsity: {sparsity:.1%}")  # → Sparsity: 40.0%
```

> 📖 **Research**: Han et al. (2015) "Learning both Weights and Connections for Efficient Neural Networks" showed that pruning can remove **90%** of weights in AlexNet and VGG with no accuracy loss.

---

# 9️⃣ Knowledge Distillation: Teacher → Student 👨‍🏫→👦

> 🎯 **Beginner Analogy**: Imagine a brilliant professor who has spent 40 years mastering physics. Now imagine they train a bright student — not by making the student read every book they ever read, but by explaining the key insights, intuitions, and shortcuts. The student becomes quite good, much faster, and is "smaller" (younger, less experienced) but captures most of the professor's knowledge. That's **knowledge distillation**!

**Knowledge distillation** trains a small "student" model to mimic the behavior of a large "teacher" model.

### How It Works

1. **Teacher**: A large, powerful model (e.g., GPT-4-sized)
2. **Student**: A smaller model (e.g., 10x fewer parameters)
3. **Training**: The student learns to match the teacher's output **probabilities** (not just the correct answer)

### 📐 Distillation Loss Formula

$$\mathcal{L} = \alpha \mathcal{L}_{\text{hard}} + (1-\alpha) T^2 \cdot \text{KL}(p_S \| p_T)$$

Where:
- $\mathcal{L}_{\text{hard}}$ = standard cross-entropy loss with true labels
- $\text{KL}(p_S \| p_T)$ = KL divergence between student ($p_S$) and teacher ($p_T$) soft predictions
- $T$ = temperature (higher = softer probabilities, more information shared)
- $\alpha$ = balance between hard labels and teacher knowledge (typically 0.1–0.5)

> 💡 **Why Soft Labels?** When a teacher classifies a picture as "cat" with 90% confidence but also says "tiger" 8% and "lion" 2%, those "wrong" answers contain valuable information — cats DO look like small tigers! Hard labels ("it's a cat, period") throw away this rich information.

### 🔥 Temperature Explained

| Temperature $T$ | Effect | When to Use |
|-----------------|--------|-------------|
| $T = 1$ | Normal probabilities | Default |
| $T = 2\text{–}5$ | Softer, more informative | Standard distillation |
| $T = 10+$ | Very soft, almost uniform | Very large teachers |

### 🏭 Production Example
```python
import torch.nn.functional as F

def distillation_loss(student_logits, teacher_logits, labels, T=4.0, alpha=0.3):
    """Combined hard + soft distillation loss."""
    # Hard loss: student vs true labels
    hard_loss = F.cross_entropy(student_logits, labels)
    
    # Soft loss: student vs teacher (with temperature)
    soft_student = F.log_softmax(student_logits / T, dim=-1)
    soft_teacher = F.softmax(teacher_logits / T, dim=-1)
    soft_loss = F.kl_div(soft_student, soft_teacher, reduction='batchmean')
    
    return alpha * hard_loss + (1 - alpha) * T**2 * soft_loss
```

> 📖 **Research**: Hinton et al. (2015) "Distilling the Knowledge in a Neural Network" — the foundational paper that introduced this technique. Google used it to compress large ensemble models into single small models for production.

### ⭐ Real-World Distillation Success Stories

| Teacher | Student | Task | Quality Retained |
|---------|---------|------|------------------|
| BERT-Large (340M) | DistilBERT (66M) | NLP | 97% of BERT on GLUE |
| GPT-4 | Various | Chat | Used for many open-source models |
| Whisper Large | Whisper Small | Speech | ~95% accuracy |

---

# 🔟 Low-Rank Factorization: Simplify the Matrices 📖

> 🎯 **Beginner Analogy**: Imagine a 500-page book. Most of it is repetitive — the same themes, characters, and plot points appear over and over. You could write a **50-page summary** that captures 95% of the content. **Low-rank factorization** does this for the weight matrices inside neural networks — it finds a compact summary that captures most of the information.

Neural network layers store their knowledge in **matrices** (grids of numbers). Low-rank factorization replaces one big matrix with two smaller matrices that approximately reconstruct it.

### 📐 The Math

$$W \approx UV^T$$

Where:
- $W \in \mathbb{R}^{m \times n}$ = original weight matrix ($m \times n$ numbers)
- $U \in \mathbb{R}^{m \times r}$ = first small matrix
- $V \in \mathbb{R}^{n \times r}$ = second small matrix
- $r$ = rank (how many "key points" we keep), with $r \ll \min(m, n)$

### 💰 Parameter Savings

| | Original | Low-Rank | Savings |
|---|---------|----------|----------|
| **Parameters** | $mn$ | $(m + n)r$ | Huge when $r \ll m, n$ |
| **Example** ($m=n=4096$, $r=64$) | 16,777,216 | 524,288 | **32x fewer!** |

> ⭐ **This is how LoRA works!** LoRA (Low-Rank Adaptation) — the most popular fine-tuning method — uses exactly this idea. Instead of updating all $mn$ parameters, it only trains the low-rank matrices $U$ and $V$.

### 🏭 Production Example: LoRA
```python
# Instead of updating W (4096 x 4096 = 16M params)
# LoRA trains A (4096 x 64) and B (64 x 4096) = 524K params

class LoRALayer(torch.nn.Module):
    def __init__(self, in_features, out_features, rank=64):
        super().__init__()
        self.W = torch.nn.Linear(in_features, out_features, bias=False)
        self.W.weight.requires_grad = False  # Freeze original
        
        self.A = torch.nn.Linear(in_features, rank, bias=False)   # Down-project
        self.B = torch.nn.Linear(rank, out_features, bias=False)  # Up-project
    
    def forward(self, x):
        return self.W(x) + self.B(self.A(x))  # W*x + B*A*x ≈ (W + BA)*x
```

> 📖 **Research**: Hu et al. (2021) "LoRA: Low-Rank Adaptation of Large Language Models" — showed that fine-tuning with rank $r = 8$ to $64$ captures most task-specific knowledge.

---

# 1️⃣1️⃣ Weight Sharing: Reuse, Don't Repeat 🏢

> 🎯 **Beginner Analogy**: In an apartment building, each unit doesn't have its own separate walls — neighboring apartments **share walls**. This saves materials (and money!) while still giving everyone a functional living space. **Weight sharing** lets different parts of a neural network reuse the same weight values.

### How It Works

Instead of every connection having its own unique number, we create a small "codebook" of shared values and assign each weight to one of them.

### Types of Weight Sharing

| Type | How It Works | Example |
|------|-------------|----------|
| **K-means clustering** | Group similar weights, replace each group with its centroid | Han et al., 2015 |
| **Hash-based** | Hash function maps weights to shared buckets | HashedNets (Chen et al., 2015) |
| **Tied embeddings** | Input/output embeddings share the same matrix | Most modern LLMs |

> ⭐ **Fun Fact**: Almost every modern language model (GPT, LLaMA, etc.) uses **tied embeddings** — the input word embedding matrix and the output prediction matrix are literally the same matrix. This saves hundreds of millions of parameters!

### 🏭 Production Example: Tied Embeddings
```python
class LanguageModel(torch.nn.Module):
    def __init__(self, vocab_size, d_model):
        super().__init__()
        self.embedding = torch.nn.Embedding(vocab_size, d_model)
        self.lm_head = torch.nn.Linear(d_model, vocab_size, bias=False)
        
        # 🔗 Tie weights — share the SAME matrix!
        self.lm_head.weight = self.embedding.weight
        # Saves vocab_size × d_model parameters
        # For LLaMA-7B: 32000 × 4096 = 131M params saved!
```

---

# 1️⃣2️⃣ Compression Ratio & Combined Techniques 📏

### 📐 Compression Ratio Formula

$$CR = \frac{\text{Original Size}}{\text{Compressed Size}}$$

> 💡 A compression ratio of 4x means the model is 4 times smaller than the original.

### 🔥 Combining Techniques (The "Deep Compression" Pipeline)

Han et al. (2015) "Deep Compression" combined **three techniques** sequentially:

| Step | Technique | Compression | Running Total |
|------|----------|-------------|---------------|
| 1 | Pruning (90% sparsity) | 10x | 10x |
| 2 | Quantization (8-bit) | 4x | 40x |
| 3 | Huffman Coding | ~1.5x | **35–49x** |

> ⭐ **Result**: AlexNet went from **240MB → 6.9MB** (35x compression) with **zero accuracy loss**! This showed that neural networks are massively over-parameterized.

### 📊 Modern Compression Comparison

| Technique | Typical CR | Quality Loss | Training Needed? | Best For |
|-----------|-----------|-------------|-----------------|----------|
| INT4 Quantization | 4x | < 3% | No (PTQ) | LLM deployment |
| Pruning (90%) | 10x | 1–5% | Fine-tuning | Edge devices |
| Distillation | 5–10x | 3–10% | Full training | Mobile models |
| Low-Rank (LoRA) | 10–100x* | Varies | Adaptation only | Fine-tuning |
| Weight Sharing | 2–4x | < 1% | Optional | Embedding layers |

\*LoRA compresses the fine-tuning update, not the base model.

---

# 📝 Summary

| Method | Bits | Quality | Speed | Best For |
|--------|------|---------|-------|----------|
| FP16 | 16 | Baseline | 1x | Training |
| INT8 | 8 | ~99.5% | 1.5-2x | Production inference |
| INT4 GPTQ/AWQ | 4 | ~97-98% | 2-3x | Consumer GPU inference |
| GGUF Q4_K_M | 4 | ~97% | CPU-fast | CPU/Mac deployment |

### 📊 Complete Compression Techniques Summary

| Technique | What It Does | Analogy | Key Formula |
|-----------|-------------|---------|-------------|
| **Quantization** | Reduces number precision | Rounding prices 💰 | $q = \text{round}(x / \text{scale}) + \text{zero\_point}$ |
| **Pruning** | Removes unimportant weights | Trimming dead branches 🌳 | $s = \frac{\text{zeros}}{\text{total params}}$ |
| **Distillation** | Small model mimics large one | Teacher → student 👨‍🏫 | $\mathcal{L} = \alpha \mathcal{L}_{\text{hard}} + (1-\alpha) T^2 \cdot \text{KL}(p_S \| p_T)$ |
| **Low-Rank** | Factorizes weight matrices | Summarizing a book 📖 | $W \approx UV^T$ |
| **Weight Sharing** | Reuses weight values | Shared walls in apartments 🏢 | Codebook lookup |

---

# 🔖 Cheat Sheet: Model Compression Decision Guide

```
❓ "I need to deploy an LLM on a consumer GPU"
   → Use GPTQ or AWQ (4-bit quantization)
   
❓ "I need to run on CPU/Mac"
   → Use llama.cpp with GGUF Q4_K_M format
   
❓ "I need a smaller model, not just compressed weights"
   → Use knowledge distillation (train a student model)
   
❓ "I need to fine-tune cheaply"
   → Use LoRA (low-rank adaptation)
   
❓ "I need maximum compression"
   → Combine: Pruning → Quantization → Huffman coding (Han et al.)
   
❓ "I need real-time inference on mobile"
   → Quantize to INT8 + TFLite (Android) or CoreML (iOS)

❓ "I need fast GPU inference in production"
   → TensorRT with INT8 or FP8 quantization
```

### ⚡ Quick Reference: Tools & Libraries

| Tool | Best For | Platform |
|------|---------|----------|
| **bitsandbytes** | Quick 4/8-bit loading | GPU (CUDA) |
| **AutoGPTQ** | GPTQ quantized models | GPU |
| **llama.cpp** | CPU inference (GGUF) | CPU/Mac/Linux |
| **TensorRT** | Production GPU serving | NVIDIA GPUs |
| **TFLite** | Mobile deployment | Android |
| **CoreML** | Apple deployment | iOS/macOS |
| **ONNX Runtime** | Cross-platform | CPU/GPU |

---

# 📚 Resources

### 📖 Books
- **"Efficient Deep Learning"** by Menghani (2023) — Comprehensive guide to model compression and efficient inference
- **"TinyML"** by Warden & Situnayake (O'Reilly) — Machine learning on microcontrollers, heavy on quantization and compression
- **"Neural Network Compression"** survey — Covers pruning, quantization, distillation, and architecture search

### 📄 Key Research Papers
| Paper | Authors | Year | Contribution |
|-------|---------|------|-------------|
| "Deep Compression" | Han, Mao & Dally | 2015 | Combined pruning + quantization + Huffman coding for 35–49x compression |
| "Distilling the Knowledge in a Neural Network" | Hinton, Vinyals & Dean | 2015 | Introduced knowledge distillation with temperature scaling |
| "GPTQ: Accurate Post-Training Quantization" | Frantar et al. | 2022 | Layer-wise quantization using Hessian information; standard for LLM quantization |
| "AWQ: Activation-aware Weight Quantization" | Lin et al. | 2023 | Protects salient weights based on activation magnitudes |
| "LoRA: Low-Rank Adaptation of Large Language Models" | Hu et al. | 2021 | Parameter-efficient fine-tuning via low-rank decomposition |
| "Learning both Weights and Connections" | Han et al. | 2015 | Foundational pruning paper showing 90% sparsity with no accuracy loss |
| "QLoRA" | Dettmers et al. | 2023 | 4-bit quantization + LoRA for efficient fine-tuning |

### 🔗 Tools & Libraries
- **llama.cpp**: https://github.com/ggerganov/llama.cpp — CPU inference with GGUF quantized models
- **AutoGPTQ**: https://github.com/AutoGPTQ/AutoGPTQ — GPTQ quantization and loading
- **bitsandbytes**: https://github.com/TimDettmers/bitsandbytes — 4/8-bit quantization for training/inference
- **TensorRT**: https://developer.nvidia.com/tensorrt — NVIDIA's production inference optimizer

### 🎓 Further Learning
- Hugging Face Quantization Guide: https://huggingface.co/docs/transformers/quantization
- "A Survey of Model Compression and Acceleration for Deep Neural Networks" (Cheng et al., 2020)

**Tomorrow**: Training Infrastructure — Cloud, Clusters & Cost.
