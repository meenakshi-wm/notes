📘 DAY 33 — Paper Reproduction: Scaling Laws for Neural Language Models

Goal:

Reproduce the key findings of Kaplan et al. (2020) — the power law relationships between model size, dataset size, compute, and loss. Run small-scale experiments to verify the scaling laws.

---

# 🌟 Beginner's Guide: What Are Scaling Laws?

> ⭐ **One-Line Summary**: Scaling laws are like **growth charts for AI models** — they predict how much smarter a model gets when you feed it more data, make it bigger, or train it longer.

## 🎯 Everyday Analogies

| AI Concept | Everyday Analogy |
|---|---|
| **Parameters (N)** | 🧠 Brain size — a bigger brain can store more knowledge |
| **Data (D)** | 📚 Practice — more practice = better skills |
| **Compute (C)** | ⏰ Study time — total hours spent learning |
| **Power law** | 📉 Diminishing returns — your first hour of piano practice helps WAY more than your 1000th hour |
| **Scaling law** | 📊 Growth chart — pediatricians predict a child's height; we predict AI performance |
| **Chinchilla** | ⚖️ Finding the sweet spot between brain size and practice time |

## 🔑 The Big Idea

Imagine you're learning guitar 🎸:
- **More practice (data)** → you get better, but each hour helps a little less
- **Bigger brain (parameters)** → you can learn harder songs
- **Total study time (compute)** → practice hours × brain power

Scaling laws tell us the **exact mathematical relationship** between these factors and performance. This is worth billions of dollars because training a large language model costs $10M–$100M+, and you want to know the result *before* spending that money.

---

# 1️⃣ Paper Summary

**Core Finding**: Language model loss follows power laws:

L(N) = (N_c / N)^α_N  — Loss vs parameters (α_N ≈ 0.076)
L(D) = (D_c / D)^α_D  — Loss vs data (α_D ≈ 0.095)
L(C) = (C_c / C)^α_C  — Loss vs compute (α_C ≈ 0.050)

Where N = parameters, D = dataset tokens, C = compute (FLOPs).

### ⭐ The Three Power Laws in LaTeX

$$L(N) = \left(\frac{N_c}{N}\right)^{\alpha_N}, \quad \alpha_N \approx 0.076$$

$$L(D) = \left(\frac{D_c}{D}\right)^{\alpha_D}, \quad \alpha_D \approx 0.095$$

$$L(C) = \left(\frac{C_c}{C}\right)^{\alpha_C}, \quad \alpha_C \approx 0.050$$

> 🎯 **Beginner analogy**: Loss = how many mistakes the model makes. These formulas say: *"Double the brain size → mistakes drop by ~5%. Double the practice data → mistakes drop by ~6.5%."* Just like how doubling your piano practice doesn't double your skill — you get **diminishing returns** following a precise mathematical curve.

### 📐 Total Compute Formula

The total compute (in FLOPs) for training is approximately:

$$C \approx 6ND$$

where $N$ = number of parameters, $D$ = number of training tokens. The factor of 6 comes from: 2 FLOPs per parameter per token (forward pass) × 3 (forward + backward pass ≈ 3× forward).

> 💡 Think of it like: **Study time = Brain size × Amount of material × Effort per page**

### 📖 Citation
> Kaplan, J., McCandlish, S., Henighan, T., Brown, T. B., Chess, B., Child, R., Gray, S., Radford, A., Wu, J., & Amodei, D. (2020). *Scaling Laws for Neural Language Models.* arXiv:2001.08361.

---

# 2️⃣ Experimental Setup

> 🧪 **What we're doing**: Training the *same type of model* at different sizes (from tiny to large) and seeing how loss changes. It's like testing students with brains of different sizes on the same exam.

```python
import torch
import torch.nn as nn
from torch.utils.data import DataLoader
import numpy as np
import matplotlib.pyplot as plt

# Model configs at different scales
CONFIGS = {
    '768K':   {'d_model': 64,  'n_layers': 2,  'n_heads': 2, 'd_ff': 256},
    '3M':     {'d_model': 128, 'n_layers': 4,  'n_heads': 4, 'd_ff': 512},
    '12M':    {'d_model': 256, 'n_layers': 6,  'n_heads': 8, 'd_ff': 1024},
    '50M':    {'d_model': 512, 'n_layers': 8,  'n_heads': 8, 'd_ff': 2048},
    '125M':   {'d_model': 768, 'n_layers': 12, 'n_heads': 12, 'd_ff': 3072},
    '350M':   {'d_model': 1024,'n_layers': 24, 'n_heads': 16, 'd_ff': 4096},
}

def count_params(config):
    d = config['d_model']
    L = config['n_layers']
    dff = config['d_ff']
    # Approximate: embedding + L * (attention + FFN + norms)
    n_embed = 32000 * d  # vocab embedding
    n_attn = L * (4 * d * d)  # Q, K, V, O projections
    n_ffn = L * (2 * d * dff)  # up and down projections
    return n_embed + n_attn + n_ffn

for name, config in CONFIGS.items():
    print(f"{name}: {count_params(config)/1e6:.1f}M params")
```

---

# 3️⃣ Training at Multiple Scales

> 🏋️ **What's happening here**: We train each model size on the same data and track how the loss (mistakes) decreases. The key insight: $C \approx 6ND$ — compute = 6 × parameters × tokens (forward + backward pass).

```python
def train_model(config, tokens_budget, dataset, seq_len=256, batch_size=32, device='cuda'):
    """Train a model and track loss over compute."""
    model = DecoderOnlyTransformer(**config, vocab_size=32000, seq_len=seq_len).to(device)
    optimizer = torch.optim.Adam(model.parameters(), lr=3e-4, betas=(0.9, 0.95))
    
    num_params = sum(p.numel() for p in model.parameters())
    flops_per_token = 6 * num_params  # Forward + backward ≈ 6N
    
    total_tokens = 0
    losses = []
    compute_points = []
    
    loader = DataLoader(dataset, batch_size=batch_size, shuffle=True)
    
    while total_tokens < tokens_budget:
        for batch in loader:
            x = batch[:, :-1].to(device)
            y = batch[:, 1:].to(device)
            
            logits = model(x)
            loss = F.cross_entropy(logits.view(-1, 32000), y.view(-1))
            
            optimizer.zero_grad()
            loss.backward()
            torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
            optimizer.step()
            
            batch_tokens = x.numel()
            total_tokens += batch_tokens
            
            if total_tokens % 10000 < batch_tokens:
                losses.append(loss.item())
                compute_points.append(total_tokens * flops_per_token)
            
            if total_tokens >= tokens_budget:
                break
    
    return {
        'num_params': num_params,
        'final_loss': np.mean(losses[-10:]),
        'losses': losses,
        'compute': compute_points,
    }
```

---

# 4️⃣ Fitting Power Laws

> 📈 **Beginner note**: A power law means $y = a \cdot x^{-b}$. On a log-log plot, this shows up as a **straight line** — that's how we verify the scaling law holds! If you plot "brain size" vs "mistakes" on log-log paper and get a straight line, the power law is confirmed.

```python
def fit_power_law(x, y):
    """Fit L = a * x^(-b) using log-log linear regression."""
    log_x = np.log(x)
    log_y = np.log(y)
    
    # Linear regression in log space
    coeffs = np.polyfit(log_x, log_y, 1)
    b = -coeffs[0]  # exponent
    a = np.exp(coeffs[1])  # coefficient
    
    return a, b

def verify_scaling_laws(results):
    """Plot and verify power law relationships."""
    params = [r['num_params'] for r in results]
    losses = [r['final_loss'] for r in results]
    
    a, b = fit_power_law(params, losses)
    
    plt.figure(figsize=(10, 6))
    plt.loglog(params, losses, 'bo', markersize=10, label='Measured')
    
    x_fit = np.logspace(np.log10(min(params)), np.log10(max(params)), 100)
    y_fit = a * x_fit ** (-b)
    plt.loglog(x_fit, y_fit, 'r--', label=f'L = {a:.2f} × N^(-{b:.4f})')
    
    plt.xlabel('Parameters (N)')
    plt.ylabel('Loss (L)')
    plt.title(f'Scaling Law: L ∝ N^(-{b:.4f})\n(Paper: α_N ≈ 0.076)')
    plt.legend()
    plt.grid(True, which='both', alpha=0.3)
    plt.show()
    
    print(f"Fitted exponent: {b:.4f}")
    print(f"Paper exponent:  0.076")
```

---

# 5️⃣ Key Findings to Verify

### 1. Loss vs Parameters (fixed data, fixed compute per param)
L(N) ∝ N^{-0.076}

> ⭐ In LaTeX: $L(N) \propto N^{-0.076}$ — doubling parameters reduces loss by $2^{-0.076} \approx 5\%$

### 2. Loss vs Dataset Size (fixed params)
L(D) ∝ D^{-0.095}

> ⭐ In LaTeX: $L(D) \propto D^{-0.095}$ — doubling data reduces loss by $2^{-0.095} \approx 6.4\%$ (data helps more than size!)

### 3. Compute-Optimal Allocation
Given fixed compute C, how to split between N and D?
Paper: N ∝ C^0.73, D ∝ C^0.27 (favor bigger models)
Chinchilla: N ∝ C^0.50, D ∝ C^0.50 (equal scaling)

### ⚖️ Kaplan vs Chinchilla: The Great Debate

| | Kaplan et al. (2020) | Chinchilla / Hoffmann et al. (2022) |
|---|---|---|
| **Optimal model size** | $N_{opt} \propto C^{0.73}$ | $N_{opt} \propto C^{0.50}$ |
| **Optimal data** | $D_{opt} \propto C^{0.27}$ | $D_{opt} \propto C^{0.50}$ |
| **Philosophy** | 🧠 Bigger brain, less practice | ⚖️ Equal brain & practice |
| **Analogy** | Hire a genius, give them one textbook | Hire a smart person, give them a whole library |
| **Real-world result** | GPT-3: 175B params, 300B tokens | Chinchilla: 70B params, 1.4T tokens (same compute, **better!**) |

```python
def compute_optimal_allocation(total_compute_flops):
    """Compare Kaplan vs Chinchilla optimal allocation."""
    # Kaplan: favor larger models
    N_kaplan = (total_compute_flops / 6) ** 0.73
    D_kaplan = total_compute_flops / (6 * N_kaplan)
    
    # Chinchilla: equal scaling
    N_chinchilla = (total_compute_flops / 6) ** 0.50
    D_chinchilla = total_compute_flops / (6 * N_chinchilla)
    
    print(f"Compute: {total_compute_flops:.2e} FLOPs")
    print(f"Kaplan:     N={N_kaplan:.2e}, D={D_kaplan:.2e}, ratio D/N={D_kaplan/N_kaplan:.0f}")
    print(f"Chinchilla: N={N_chinchilla:.2e}, D={D_chinchilla:.2e}, ratio D/N={D_chinchilla/N_chinchilla:.0f}")

compute_optimal_allocation(1e20)  # Mid-scale
compute_optimal_allocation(1e23)  # GPT-3 scale
```

---

# 6️⃣ Chinchilla Correction (Hoffmann et al., 2022)

> 📖 Hoffmann, J., Borgeaud, S., Mensch, A., Buchatskaya, E., Cai, T., Rutherford, E., ... & Sifre, L. (2022). *Training Compute-Optimal Large Language Models.* arXiv:2203.15556.

The Kaplan paper underestimated data scaling. Chinchilla showed:

### ⭐ Chinchilla Optimal Scaling

$$N_{opt} \propto C^{0.5}, \quad D_{opt} \propto C^{0.5}$$

> 🎯 **Beginner translation**: Given a fixed budget (compute $C$), you should **grow the model and data equally**. Chinchilla = finding the right balance between brain size 🧠 and practice time 📚.

### 🏭 Production Impact

- ⭐ **Chinchilla proved data matters more than Kaplan thought** — this single insight redirected billions of dollars in industry investment
- **LLaMA (Meta, 2023)**: Deliberately "over-trained" a 7B model on 1T tokens (far beyond Chinchilla-optimal) because smaller models are cheaper to **serve** at inference time
- **Modern rule of thumb**: ~20 tokens per parameter for compute-optimal training, but production models often use 100–200+ tokens/param for inference efficiency

| Budget | Kaplan (N-heavy) | Chinchilla (Balanced) |
|--------|-----------------|----------------------|
| 10^20 FLOPs | 1B params, 16B tokens | 400M params, 8B tokens |
| 10^22 FLOPs | 10B params, 160B tokens | 4B params, 800B tokens |
| 10^23 FLOPs | 50B params, 300B tokens | 20B params, 3T tokens |

Modern practice follows Chinchilla: ~20 tokens per parameter.

### 🔄 Over-Training: Breaking Chinchilla on Purpose

> In production, companies **intentionally violate** Chinchilla scaling because:
> - Training is a **one-time cost**, but inference runs **millions of times**
> - A smaller model trained on more data → cheaper to deploy
> - Example: LLaMA-7B trained on 1T tokens (143 tokens/param vs Chinchilla's ~20)

> 📖 Muennighoff, N., Rush, A. M., Barak, B., Le Scao, T., Tazi, N., Piktus, A., ... (2023). *Scaling Data-Constrained Language Models.* arXiv:2305.16264.

---

# 7️⃣ Practical Implications for Startups

```
If your compute budget is X:
1. Use Chinchilla scaling to decide model size
2. Don't train a huge model on too little data
3. Quality data > more parameters when budget-constrained

Budget $10K (H100s):
→ ~1B params, ~20B tokens of quality data
→ OR fine-tune existing 7B on your domain

Budget $100K:
→ ~3B params, ~60B tokens
→ OR fine-tune existing 70B

Budget $1M:
→ ~7B params from scratch with 140B tokens
→ OR continued pretraining of 70B on your data
```

---

# 📝 Summary

| Law | Relationship | Exponent |
|-----|-------------|----------|
| Loss vs N | L ∝ N^{-α_N} | α_N ≈ 0.076 |
| Loss vs D | L ∝ D^{-α_D} | α_D ≈ 0.095 |
| Loss vs C | L ∝ C^{-α_C} | α_C ≈ 0.050 |
| Optimal N(C) | N ∝ C^{0.5} | Chinchilla |
| Optimal D(C) | D ∝ C^{0.5} | Chinchilla |

---

# 📋 Cheat Sheet

| Formula | LaTeX | What It Means |
|---|---|---|
| Loss vs params | $L(N) = (N_c/N)^{\alpha_N}$ | Bigger model → lower loss (diminishing returns) |
| Loss vs data | $L(D) = (D_c/D)^{\alpha_D}$ | More data → lower loss (diminishing returns) |
| Loss vs compute | $L(C) = (C_c/C)^{\alpha_C}$ | More compute → lower loss (diminishing returns) |
| Total compute | $C \approx 6ND$ | Compute ≈ 6 × params × tokens |
| Chinchilla optimal N | $N_{opt} \propto C^{0.5}$ | Scale model size as square root of compute |
| Chinchilla optimal D | $D_{opt} \propto C^{0.5}$ | Scale data as square root of compute |
| Chinchilla ratio | ~20 tokens per parameter | Rule of thumb for balanced training |
| Kaplan optimal N | $N_{opt} \propto C^{0.73}$ | Older (superseded): favored bigger models |

> ⭐ **The #1 takeaway**: Before spending millions training a model, use $C \approx 6ND$ and Chinchilla scaling $N_{opt} \propto C^{0.5}$ to plan the optimal size AND data budget.

---

# 🏭 Production Scenarios Summary

| Scenario | Scaling Law Insight | Real-World Example |
|---|---|---|
| Planning a new LLM | Use $N_{opt} \propto C^{0.5}$, $D_{opt} \propto C^{0.5}$ | Google chose 70B Chinchilla over 280B Gopher |
| Inference-optimized model | Over-train smaller model beyond Chinchilla | Meta's LLaMA: 7B on 1T tokens |
| Data-constrained domain | Repeat data with care; diminishing returns apply | Medical/legal AI with limited corpora |
| Budget estimation | $C \approx 6ND$ → estimate GPU hours & cost | $10K → ~1B model; $1M → ~7B model |
| Predicting performance | Extrapolate power law from small runs | Train 100M models to predict 10B behavior |

---

# 📚 Resources

## 📄 Key Papers
1. **Kaplan et al. (2020)** — *Scaling Laws for Neural Language Models* — [arXiv:2001.08361](https://arxiv.org/abs/2001.08361) — The foundational paper establishing power law relationships
2. **Hoffmann et al. (2022)** — *Training Compute-Optimal Large Language Models (Chinchilla)* — [arXiv:2203.15556](https://arxiv.org/abs/2203.15556) — Corrected compute-optimal allocation; showed data matters equally
3. **Muennighoff et al. (2023)** — *Scaling Data-Constrained Language Models* — [arXiv:2305.16264](https://arxiv.org/abs/2305.16264) — What to do when you run out of data (repeat with care)
4. **Touvron et al. (2023)** — *LLaMA: Open and Efficient Foundation Language Models* — [arXiv:2302.13971](https://arxiv.org/abs/2302.13971) — Applied over-training for inference efficiency

## 📖 Books
1. **"Deep Learning"** by Goodfellow, Bengio, & Courville — Chapter 8 (Optimization) for understanding loss landscapes
2. **"The Art of Statistics"** by David Spiegelhalter — Accessible introduction to power laws and statistical relationships
3. **"Designing Machine Learning Systems"** by Chip Huyen — Chapter on model scaling and production deployment

## 🔗 Additional Resources
- Nostalgebraist's blog: *Chinchilla's Wild Implications* — Excellent intuitive explanation
- Epoch AI's *Compute Trends Across Three Eras of Machine Learning* — Historical compute data

**Tomorrow**: Reproducing DDPM (Denoising Diffusion Probabilistic Models).
