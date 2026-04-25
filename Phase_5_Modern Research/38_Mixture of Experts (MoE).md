📘 DAY 38 — Paper Reproduction: Mixture of Experts (MoE)

Goal:

Reproduce the Mixture of Experts architecture — implement sparse gating, expert routing, and load balancing. Understand why MoE enables models with more parameters at the same compute cost.

---

# 🏥 Beginner Analogy: The Hospital of Specialists

> **Imagine a hospital** 🏥 with 8 specialist doctors (cardiologist, neurologist, dermatologist, etc.).
>
> - **Dense model** = One **general practitioner** (GP) who handles **every** patient alone. They're decent at everything, but expert at nothing.
> - **MoE model** = A hospital with a **receptionist (router)** who reads your symptoms and sends you to the **2 best specialists** for your case.
> - **Experts** = Each specialist doctor (cardiologist 🫀, neurologist 🧠, dermatologist, etc.)
> - **Top-K routing** = The receptionist picks the **K=2 best doctors** for your problem.
> - **Load balancing** = Making sure **no single doctor is overwhelmed** while others sit idle.
>
> ⭐ **Key insight**: The hospital has 8 doctors (massive total capacity!), but each patient only sees 2 (low compute cost!). **More knowledge, same wait time.**

---

# 1️⃣ MoE Core Idea

Standard FFN: Every token uses ALL parameters.
MoE FFN: Each token routes to TOP-K experts (out of N experts).

If N=8 experts and K=2: each token uses 2/8 = 25% of FFN parameters.
But the model has 8× more parameters! More capacity, same compute.

### ⭐ Core MoE Formula

The MoE layer output for input $x$ is:

$$y = \sum_{i=1}^{N} g_i(x) \cdot E_i(x)$$

Where:
- $N$ = number of experts
- $E_i(x)$ = output of expert $i$ (a standard FFN)
- $g_i(x)$ = gating weight for expert $i$ (most are **zero** due to Top-K sparsity!)

> 🏥 **Analogy**: $g_i(x)$ is how strongly the receptionist recommends doctor $i$. Most doctors get a weight of 0 ("you don't need them"). Only the Top-K get nonzero weights.

### ⭐ Router / Gating Function

The router decides which experts handle each token:

$$g(x) = \text{TopK}(\text{softmax}(W_g \cdot x))$$

Where $W_g \in \mathbb{R}^{N \times d}$ is a learned gating matrix. After softmax, only the **Top-K** values are kept; the rest are set to zero.

### ⭐ Total Params vs. Active Params

| Metric | Formula | Example (Mixtral 8×7B) |
|--------|---------|------------------------|
| **Total parameters** | $P_{total} = N \times P_{expert} + P_{shared}$ | ~47B |
| **Active parameters per token** | $P_{active} = K \times P_{expert} + P_{shared}$ | ~12.9B |
| **Compute ratio** | $K / N$ | 2/8 = 25% |

> 💡 You get the **knowledge** of a 47B model at the **speed** of a ~13B model!

---

# 2️⃣ Architecture

```
Standard Transformer Block:
  Attention → FFN (one large FFN)           ← 🧑‍⚕️ One GP sees every patient

MoE Transformer Block:
  Attention → Router → Top-K Experts (K out of N FFNs)  ← 🏥 Receptionist → 2 Specialists
```

> 🏥 **Analogy**: In a standard block, every patient (token) sees the same GP (single FFN). In MoE, the receptionist (router) reads your chart and sends you to just the 2 specialists you need.

---

# 3️⃣ Implementation

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class Expert(nn.Module):
    """Single expert (standard FFN)."""
    def __init__(self, d_model, d_ff, dropout=0.1):
        super().__init__()
        self.w1 = nn.Linear(d_model, d_ff)
        self.w2 = nn.Linear(d_ff, d_model)
        self.dropout = nn.Dropout(dropout)
    
    def forward(self, x):
        return self.w2(self.dropout(F.silu(self.w1(x))))

class TopKRouter(nn.Module):
    """Learned routing: select top-K experts per token."""
    def __init__(self, d_model, num_experts, top_k=2):
        super().__init__()
        self.top_k = top_k
        self.gate = nn.Linear(d_model, num_experts, bias=False)
    
    def forward(self, x):
        # x: (B, T, d) → gate_logits: (B, T, num_experts)
        gate_logits = self.gate(x)
        
        # Select top-K experts
        topk_values, topk_indices = torch.topk(gate_logits, self.top_k, dim=-1)
        topk_weights = F.softmax(topk_values, dim=-1)
        
        return topk_weights, topk_indices, gate_logits

class MoELayer(nn.Module):
    """Mixture of Experts layer replacing standard FFN."""
    def __init__(self, d_model, d_ff, num_experts=8, top_k=2, dropout=0.1):
        super().__init__()
        self.num_experts = num_experts
        self.top_k = top_k
        self.router = TopKRouter(d_model, num_experts, top_k)
        self.experts = nn.ModuleList([Expert(d_model, d_ff, dropout) for _ in range(num_experts)])
    
    def forward(self, x):
        B, T, D = x.shape
        
        # Route tokens to experts
        topk_weights, topk_indices, gate_logits = self.router(x)
        
        # Compute expert outputs
        output = torch.zeros_like(x)
        
        # Flatten for easier indexing
        flat_x = x.view(-1, D)  # (B*T, D)
        flat_topk_indices = topk_indices.view(-1, self.top_k)  # (B*T, K)
        flat_topk_weights = topk_weights.view(-1, self.top_k)  # (B*T, K)
        
        for k in range(self.top_k):
            expert_indices = flat_topk_indices[:, k]  # (B*T,)
            expert_weights = flat_topk_weights[:, k]  # (B*T,)
            
            for e in range(self.num_experts):
                mask = expert_indices == e
                if mask.any():
                    expert_input = flat_x[mask]
                    expert_output = self.experts[e](expert_input)
                    output.view(-1, D)[mask] += expert_weights[mask, None] * expert_output
        
        # Auxiliary load balancing loss
        aux_loss = self.load_balance_loss(gate_logits)
        
        return output, aux_loss
    
    def load_balance_loss(self, gate_logits):
        """Encourage uniform expert utilization."""
        # Fraction of tokens routed to each expert
        routing_probs = F.softmax(gate_logits, dim=-1)  # (B, T, E)
        
        # Fraction of tokens where each expert is in top-K
        topk_indices = torch.topk(gate_logits, self.top_k, dim=-1).indices
        expert_mask = F.one_hot(topk_indices, self.num_experts).sum(dim=-2).float()  # (B, T, E)
        
        # Average over batch and sequence
        f = expert_mask.mean(dim=[0, 1])  # Fraction routed to each expert
        p = routing_probs.mean(dim=[0, 1])  # Mean routing probability
        
        # Load balance loss: f · p should be uniform
        aux_loss = self.num_experts * (f * p).sum()
        return aux_loss
```

### ⭐ Load Balancing Loss — Math

The auxiliary loss that prevents expert collapse:

$$\mathcal{L}_{aux} = \alpha \cdot N \sum_{i=1}^{N} f_i \cdot P_i$$

Where:
- $f_i$ = fraction of tokens **routed** to expert $i$
- $P_i$ = mean **routing probability** assigned to expert $i$
- $\alpha$ = balancing coefficient (typically 0.01)
- $N$ = number of experts (scaling factor)

> 🏥 **Analogy**: If the cardiologist 🫀 sees 80% of all patients while the neurologist 🧠 is idle, the hospital manager (load balancing loss) penalizes this imbalance and forces the receptionist to spread patients more evenly.

> ⭐ **Why $f_i \cdot P_i$?** If expert $i$ gets lots of tokens ($f_i$ high) AND the router gives it high probability ($P_i$ high), the product is large → big penalty. This pushes towards uniform distribution: each expert gets $\approx 1/N$ of tokens.

---

# 4️⃣ Token-Choice vs Expert-Choice Routing

### Token-Choice (Standard, Mixtral)
Each token picks its top-K experts.
Problem: Some experts may get overloaded.

### Expert-Choice (Zhou et al.)
Each expert picks its top-C tokens.
Better load balance, but some tokens might not be processed.

```python
class ExpertChoiceRouter(nn.Module):
    def __init__(self, d_model, num_experts, capacity_factor=1.0):
        super().__init__()
        self.gate = nn.Linear(d_model, num_experts, bias=False)
        self.capacity_factor = capacity_factor
    
    def forward(self, x):
        B, T, D = x.shape
        gate_logits = self.gate(x)  # (B, T, E)
        
        capacity = int(self.capacity_factor * T / self.num_experts)
        
        for e in range(self.num_experts):
            expert_scores = gate_logits[:, :, e]  # (B, T)
            topk_vals, topk_idx = torch.topk(expert_scores, capacity, dim=-1)
            # Process only the top-capacity tokens for this expert
```

---

# 5️⃣ Full MoE Transformer

```python
class MoETransformerBlock(nn.Module):
    def __init__(self, d_model, n_heads, d_ff, num_experts=8, top_k=2):
        super().__init__()
        self.attn = MultiHeadAttention(d_model, n_heads)
        self.norm1 = nn.LayerNorm(d_model)
        self.moe = MoELayer(d_model, d_ff, num_experts, top_k)
        self.norm2 = nn.LayerNorm(d_model)
    
    def forward(self, x, mask=None):
        h = x + self.attn(self.norm1(x), mask=mask)
        moe_out, aux_loss = self.moe(self.norm2(h))
        return h + moe_out, aux_loss

class MoETransformer(nn.Module):
    def __init__(self, vocab_size, d_model=512, n_layers=12, n_heads=8,
                 d_ff=2048, num_experts=8, top_k=2):
        super().__init__()
        self.embed = nn.Embedding(vocab_size, d_model)
        self.layers = nn.ModuleList([
            MoETransformerBlock(d_model, n_heads, d_ff, num_experts, top_k)
            for _ in range(n_layers)
        ])
        self.head = nn.Linear(d_model, vocab_size)
    
    def forward(self, x, mask=None):
        h = self.embed(x)
        total_aux_loss = 0
        
        for layer in self.layers:
            h, aux_loss = layer(h, mask)
            total_aux_loss += aux_loss
        
        logits = self.head(h)
        return logits, total_aux_loss / len(self.layers)

# Training: total_loss = lm_loss + 0.01 * aux_loss
```

---

# 6️⃣ Mixtral-Style MoE & Production Models 🚀

```
Mixtral 8x7B:
- 8 experts per layer, top-2 routing
- Each expert is a 7B-sized FFN
- Total params: ~47B
- Active params per token: ~13B (similar compute to 13B dense)
- But capacity of a much larger model!
```

### 🏭 Production MoE Models

| Model | Architecture | Total Params | Active Params | Key Innovation |
|-------|-------------|-------------|---------------|----------------|
| **Switch Transformer** (Google, 2022) | Top-1 routing, 128+ experts | up to 1.6T | ~1/128 per token | Simplified routing to 1 expert |
| **Mixtral 8×7B** (Mistral, 2024) | Top-2 routing, 8 experts | ~47B | ~12.9B | Open-weights, beats Llama 2 70B |
| **GPT-4** (OpenAI, rumored) | ~8 experts × ~220B each | ~1.8T (rumored) | ~220B (rumored) | Not officially confirmed as MoE |
| **DeepSeek-V2** (2024) | Fine-grained MoE, 160 experts | 236B | 21B active | Shared + routed expert design |

> ⭐ **Why MoE is dominating production**: It lets you **scale total parameters** (more knowledge) **without scaling compute** (same inference speed). Mixtral 8×7B uses ~13B compute but matches 70B dense models!

> 💡 **Memory vs. Compute trade-off**: All 47B params must fit in VRAM, but each forward pass only activates ~13B. You need big GPUs for memory, not for speed.

---

# 7️⃣ MoE Advantages & Challenges

| Advantage | Challenge |
|-----------|-----------|
| More params at same compute | Memory: all experts in VRAM |
| Better scaling | Expert load imbalance |
| Conditional computation | Communication overhead (distributed) |
| Sublinear compute scaling | Token dropping if expert overflow |

> 🏥 **Analogy**: Advantages = more specialists means better care. Challenges = you still need a big hospital building to house all of them, and the receptionist might keep sending everyone to the same popular doctor.

---

# 8️⃣ Key Research Papers 📚

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|------------------|
| **"Outrageously Large Neural Networks: The Sparsely-Gated Mixture-of-Experts Layer"** | Shazeer et al. | 2017 | Introduced modern MoE with sparsely-gated routing for LLMs |
| **"Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity"** | Fedus, Zoph & Shazeer | 2022 | Simplified to Top-1 routing, scaled to 1.6T params |
| **"Mixtral of Experts"** | Jiang et al. (Mistral AI) | 2024 | Open-weights MoE beating much larger dense models |
| **"GShard: Scaling Giant Models with Conditional Computation"** | Lepikhin et al. | 2021 | Efficient distributed MoE training across TPUs |
| **"DeepSeekMoE: Towards Ultimate Expert Specialization"** | Dai et al. | 2024 | Fine-grained expert segmentation + shared experts |

---

# 📝 Summary

| Aspect | Dense Model | MoE Model |
|--------|-----------|-----------|
| FFN params used per token | 100% | K/N (e.g., 25%) |
| Total params | N | N × num_experts |
| Compute per token | Full | K/N × Full |
| Quality at same compute | Baseline | ~1.5-2x equivalent |
| Memory | N | N × num_experts (higher!) |

---

# 🧾 MoE Cheat Sheet

| Concept | Formula / Rule | Beginner Explanation |
|---------|---------------|---------------------|
| MoE Output | $y = \sum_{i=1}^{N} g_i(x) \cdot E_i(x)$ | Weighted mix of specialist outputs |
| Gating / Router | $g(x) = \text{TopK}(\text{softmax}(W_g x))$ | Receptionist picks K best doctors |
| Load Balance Loss | $\mathcal{L}_{aux} = \alpha N \sum f_i \cdot P_i$ | Penalty if one doctor is overwhelmed |
| Total Loss | $\mathcal{L} = \mathcal{L}_{task} + \alpha \mathcal{L}_{aux}$ | Main job + keep doctors balanced |
| Active Params | $K \times P_{expert} + P_{shared}$ | Only K specialists work per patient |
| Compute Savings | $K / N$ of dense model | 2/8 = 25% compute for same quality |
| Typical $\alpha$ | 0.01 – 0.1 | Small enough not to hurt main task |
| Typical Top-K | 1 or 2 | 1 expert (Switch) or 2 (Mixtral) |

---

# 📚 Resources

### Papers
- 📄 Shazeer et al. (2017) — [Outrageously Large Neural Networks: The Sparsely-Gated MoE Layer](https://arxiv.org/abs/1701.06538)
- 📄 Fedus, Zoph & Shazeer (2022) — [Switch Transformers: Scaling to Trillion Parameter Models](https://arxiv.org/abs/2101.03961)
- 📄 Jiang et al. (2024) — [Mixtral of Experts](https://arxiv.org/abs/2401.04088)
- 📄 Lepikhin et al. (2021) — [GShard: Scaling Giant Models with Conditional Computation](https://arxiv.org/abs/2006.16668)
- 📄 Dai et al. (2024) — [DeepSeekMoE: Towards Ultimate Expert Specialization](https://arxiv.org/abs/2401.06066)

### Books & Guides
- 📖 "Efficient Large-Scale Language Model Training" — Narayanan et al. (Megatron-LM team guide)
- 📖 "Designing Machine Learning Systems" — Chip Huyen (Chapter on model scaling)
- 📖 Hugging Face Blog — [Mixture of Experts Explained](https://huggingface.co/blog/moe)

### Video & Tutorials
- 🎥 Yannic Kilcher — "Switch Transformers" paper walkthrough
- 🎥 AI Coffee Break — "Mixture of Experts explained simply"
- 💻 Hugging Face `transformers` — Mixtral implementation & inference guide

---

**Tomorrow**: Reproducing Mamba (State Space Models).
