📘 DAY 37 — Paper Reproduction: DPO (Direct Preference Optimization)

Goal:

Reproduce DPO (Rafailov et al., 2023) — the simplified alternative to RLHF that eliminates the reward model and RL training loop while achieving comparable alignment quality.

---

# 🌟 Beginner's Guide — What Is DPO and Why Should You Care?

> **One-sentence summary:** DPO is a way to teach an AI which answers humans prefer — *without* the complicated machinery that older methods (RLHF) required.

## 🍳 The Kitchen Analogy

| Concept | Kitchen Analogy |
|---|---|
| **RLHF (old way)** | You hire a **food critic** 🧑‍🍳📝 (reward model) to score every dish, then train the chef using those review scores (PPO reinforcement learning) |
| **DPO (new way)** | You **skip the critic entirely** — you just show the chef two dishes and tell them "customers liked *this one* better" 👈🏆. The chef learns directly from preferences! |
| **β (beta)** | How much freedom the chef has to experiment 🎛️. Low β = "follow the original cookbook closely." High β = "feel free to get creative!" |
| **Reference policy (π_ref)** | The **original cookbook** 📖 the chef started with (the pre-trained model before DPO) |
| **Preferred response (y_w)** | The dish that **won** the taste test 🥇 |
| **Rejected response (y_l)** | The dish that **lost** the taste test 🥈 |

## ⭐ Why DPO Matters (Big Picture)

- 🏭 **Production use**: LLaMA 2 (Meta), Zephyr (HuggingFace), Tulu 2, Neural Chat, and many open-source models use DPO
- 🧩 **Simplicity**: No reward model to train, no PPO instability to debug — just a single supervised loss
- 📉 **Resource efficient**: Only 2 models in memory (policy + reference) vs. 4 for RLHF
- 🔬 **Active research**: Spawned variants like IPO, KTO, ORPO, and more

---

# 1️⃣ Paper Summary

| Aspect | Detail |
|--------|--------|
| Problem | RLHF is complex (reward model + PPO + critic) |
| Key insight | The optimal policy under RLHF has a closed-form solution |
| Method | Directly optimize policy from preference pairs |
| Loss | Binary cross-entropy on log-probability margins |
| 📄 Paper | Rafailov et al. (2023) — *"Direct Preference Optimization: Your Language Model is Secretly a Reward Model"* |
| 🏗️ Builds on | Ouyang et al. (2022) — InstructGPT / RLHF pipeline |

> 💡 **Beginner note:** Think of "preference pairs" as a simple A/B test — "Do you like response A or response B better?" DPO uses thousands of these comparisons to teach the model.

---

# 2️⃣ From RLHF to DPO

### RLHF Pipeline
1. Train reward model on preferences: r(x, y)
2. Optimize policy with PPO: max E[r(x,y)] - β·KL(π‖π_ref)

> 🍳 *Analogy: Step 1 = hire the food critic. Step 2 = train the chef using the critic's scores, while making sure the chef doesn't stray too far from the original cookbook (KL penalty).*

### DPO Insight  
The optimal policy satisfies:

π*(y|x) = (1/Z(x)) · π_ref(y|x) · exp(r(y,x)/β)

$$\pi^*(y|x) = \frac{1}{Z(x)} \pi_{ref}(y|x) \exp\!\left(\frac{r(y,x)}{\beta}\right)$$

Rearranging: r(y,x) = β · log(π*(y|x)/π_ref(y|x)) + β·log Z(x)

$$r(y,x) = \beta \log \frac{\pi^*(y|x)}{\pi_{ref}(y|x)} + \beta \log Z(x)$$

> 💡 **Key insight (plain English):** If we know the perfect policy, we can *recover* the reward just by comparing it to the reference. So why learn a separate reward model at all? Just learn the policy directly!

Substituting into the Bradley-Terry preference model:

**L_DPO = -E[(x,y_w,y_l)] [ log σ(β · log(π_θ(y_w|x)/π_ref(y_w|x)) - β · log(π_θ(y_l|x)/π_ref(y_l|x))) ]**

### ⭐ DPO Loss (LaTeX)

$$\mathcal{L}_{DPO} = -\mathbb{E}_{(x, y_w, y_l)}\left[\log\sigma\!\left(\beta\log\frac{\pi_\theta(y_w|x)}{\pi_{ref}(y_w|x)} - \beta\log\frac{\pi_\theta(y_l|x)}{\pi_{ref}(y_l|x)}\right)\right]$$

where $\sigma$ is the sigmoid function, $y_w$ = preferred (winner), $y_l$ = rejected (loser).

### ⭐ Implicit Reward

DPO defines an **implicit reward** — no separate reward model needed:

$$r(x,y) = \beta \log \frac{\pi_\theta(y|x)}{\pi_{ref}(y|x)}$$

> 🍳 *Analogy: The "reward" is just how differently the trained chef cooks compared to the original cookbook. If the chef deviates a lot for a specific dish, that dish has high implicit reward.*

No reward model needed! Just compare log-probabilities of preferred vs rejected.

---

# 3️⃣ Implementation

```python
import torch
import torch.nn.functional as F

def dpo_loss(policy_chosen_logps, policy_rejected_logps,
             ref_chosen_logps, ref_rejected_logps,
             beta=0.1):
    """
    Compute DPO loss.
    
    Args:
        policy_chosen_logps: log π_θ(y_w|x) for the chosen responses
        policy_rejected_logps: log π_θ(y_l|x) for the rejected responses
        ref_chosen_logps: log π_ref(y_w|x) frozen reference model
        ref_rejected_logps: log π_ref(y_l|x) frozen reference  
        beta: temperature parameter
    """
    # Log-ratios
    chosen_logratios = policy_chosen_logps - ref_chosen_logps
    rejected_logratios = policy_rejected_logps - ref_rejected_logps
    
    # DPO loss
    logits = beta * (chosen_logratios - rejected_logratios)
    loss = -F.logsigmoid(logits).mean()
    
    # Metrics
    chosen_rewards = beta * chosen_logratios.detach()
    rejected_rewards = beta * rejected_logratios.detach()
    reward_margin = (chosen_rewards - rejected_rewards).mean()
    accuracy = (chosen_rewards > rejected_rewards).float().mean()
    
    return loss, {
        'reward_margin': reward_margin.item(),
        'accuracy': accuracy.item(),
        'chosen_reward': chosen_rewards.mean().item(),
        'rejected_reward': rejected_rewards.mean().item(),
    }
```

---

# 4️⃣ Computing Sequence Log-Probabilities

```python
def get_batch_logps(logits, labels, attention_mask):
    """Compute per-sequence log-probabilities.
    
    Args:
        logits: (B, T, V) model output logits
        labels: (B, T) token ids
        attention_mask: (B, T) mask for valid tokens
    """
    # Shift for autoregressive
    logits = logits[:, :-1, :]
    labels = labels[:, 1:]
    mask = attention_mask[:, 1:]
    
    # Per-token log probs
    log_probs = F.log_softmax(logits, dim=-1)
    per_token_logps = torch.gather(log_probs, 2, labels.unsqueeze(2)).squeeze(2)
    
    # Sum over sequence (masked)
    return (per_token_logps * mask).sum(dim=-1)
```

---

# 5️⃣ Full DPO Training Loop

```python
class DPOTrainer:
    def __init__(self, model, ref_model, tokenizer, beta=0.1, lr=5e-7):
        self.model = model
        self.ref_model = ref_model.eval()
        for p in self.ref_model.parameters():
            p.requires_grad_(False)
        
        self.tokenizer = tokenizer
        self.beta = beta
        self.optimizer = torch.optim.AdamW(model.parameters(), lr=lr)
    
    def compute_logps(self, model, input_ids, attention_mask, labels):
        with torch.no_grad() if not model.training else torch.enable_grad():
            outputs = model(input_ids=input_ids, attention_mask=attention_mask)
        return get_batch_logps(outputs.logits, labels, attention_mask)
    
    def train_step(self, batch):
        """Process a batch of preference pairs."""
        device = next(self.model.parameters()).device
        
        # Get policy log-probs
        self.model.train()
        policy_chosen_logps = self.compute_logps(
            self.model, batch['chosen_ids'].to(device),
            batch['chosen_mask'].to(device), batch['chosen_ids'].to(device))
        
        policy_rejected_logps = self.compute_logps(
            self.model, batch['rejected_ids'].to(device),
            batch['rejected_mask'].to(device), batch['rejected_ids'].to(device))
        
        # Get reference log-probs (frozen)
        with torch.no_grad():
            ref_chosen_logps = self.compute_logps(
                self.ref_model, batch['chosen_ids'].to(device),
                batch['chosen_mask'].to(device), batch['chosen_ids'].to(device))
            ref_rejected_logps = self.compute_logps(
                self.ref_model, batch['rejected_ids'].to(device),
                batch['rejected_mask'].to(device), batch['rejected_ids'].to(device))
        
        # DPO loss
        loss, metrics = dpo_loss(
            policy_chosen_logps, policy_rejected_logps,
            ref_chosen_logps, ref_rejected_logps,
            beta=self.beta
        )
        
        self.optimizer.zero_grad()
        loss.backward()
        torch.nn.utils.clip_grad_norm_(self.model.parameters(), 1.0)
        self.optimizer.step()
        
        return loss.item(), metrics
```

---

# 6️⃣ Key Hyperparameters

| Parameter | Typical Value | Effect |
|-----------|--------------|--------|
| β (beta) | 0.1 - 0.5 | Lower = more aggressive optimization 🎛️ |
| Learning rate | 1e-7 to 5e-6 | Much lower than SFT! ⚠️ |
| Epochs | 1-3 | Overfitting risk is high 🔥 |
| Batch size | 32-64 | Larger = more stable 📊 |
| Max length | 512-2048 | Task dependent 📏 |

> 🍳 **β explained for beginners:** Imagine $\beta$ as a leash on a dog 🐕. Small $\beta$ (0.1) = short leash — the model stays very close to its original behavior. Large $\beta$ (0.5) = long leash — the model can change its behavior more freely. If you let the leash too loose, the model might "overfit" (memorize the training preferences instead of generalizing).
>
> **Learning rate tip:** DPO uses rates 10–100× smaller than normal fine-tuning because the preference signal is subtle — like adjusting seasoning, not changing the whole recipe.

---

# 7️⃣ DPO vs RLHF Comparison

| Aspect | RLHF (PPO) | DPO |
|--------|-----------|-----|
| Components | SFT + Reward Model + PPO + Critic | SFT + DPO |
| Training | 4 models in memory | 2 models (policy + ref) |
| Stability | Tricky (PPO hyperparameters) | Very stable ✅ |
| Performance | Strong | Comparable |
| Implementation | ~500 lines | ~50 lines |
| Memory | 4× model size | 2× model size |
| 🍳 Analogy | Hire critic → train chef from reviews | Show chef which dish won directly |

> 🏭 **Production reality:** Meta used RLHF for LLaMA 2-Chat but the community quickly adopted DPO for fine-tuning open models (Zephyr-7B, OpenHermes, Neural Chat). The simplicity of DPO makes it the **default choice** for most open-source alignment today.

### ⭐ DPO Variants & Extensions

| Variant | Key Idea | Citation |
|---------|----------|----------|
| **IPO** (Identity Preference Optimization) | Avoids overfitting by regularizing directly in preference space | Azar et al. (2023) |
| **KTO** (Kahneman-Tversky Optimization) | Works with *unpaired* preferences (just thumbs-up / thumbs-down per response) | Ethayarajh et al. (2024) |
| **ORPO** (Odds Ratio Preference Optimization) | Merges SFT and preference optimization into a single stage | Hong et al. (2024) |
| **SimPO** | Simplifies DPO further by removing the reference model | Meng et al. (2024) |

---

# 8️⃣ Verification Experiment

```python
# Expected results on a preference dataset:
# Epoch 1: accuracy ~55% → ~70%, reward_margin: 0 → 0.5
# Epoch 2: accuracy ~75%, reward_margin: ~1.0
# Epoch 3: accuracy ~80%, reward_margin: ~1.5

# If accuracy doesn't improve → beta too low or LR too high
# If reward_margin diverges → overfitting, reduce epochs
```

---

# 📝 Summary

| DPO Advantage | Detail |
|--------------|--------|
| Simplicity | No reward model, no RL |
| Stability | Standard supervised loss |
| Efficiency | 2 models instead of 4 |
| Quality | Matches RLHF on most benchmarks |
| Key equation | L = -log σ(β(log π_θ(y_w)/π_ref(y_w) - log π_θ(y_l)/π_ref(y_l))) |

**Tomorrow**: Reproducing Mixture of Experts (MoE).

---

# 🎯 DPO Cheat Sheet

| What | Formula / Rule |
|------|----------------|
| **DPO Loss** | $\mathcal{L}_{DPO} = -\mathbb{E}\!\left[\log\sigma\!\left(\beta\log\frac{\pi_\theta(y_w|x)}{\pi_{ref}(y_w|x)} - \beta\log\frac{\pi_\theta(y_l|x)}{\pi_{ref}(y_l|x)}\right)\right]$ |
| **Implicit Reward** | $r(x,y) = \beta \log \frac{\pi_\theta(y|x)}{\pi_{ref}(y|x)}$ |
| **Gradient signal** | Increase $\pi_\theta(y_w|x)$, decrease $\pi_\theta(y_l|x)$, weighted by how "surprising" each is relative to $\pi_{ref}$ |
| **β low (0.1)** | Conservative — stay close to reference 🐕 short leash |
| **β high (0.5)** | Aggressive — allow more deviation 🐕 long leash |
| **When to use DPO** | You have paired preference data $(x, y_w, y_l)$ and want simple, stable alignment |
| **When to use KTO** | You only have thumbs-up/down (unpaired) feedback |
| **When to use RLHF** | You need online exploration or have a strong reward model |

---

# 🏭 Production Deployment Notes

| Scenario | Recommendation |
|----------|----------------|
| **Open-source 7B model alignment** | DPO with UltraFeedback or Nectar dataset, β=0.1, 1 epoch |
| **Enterprise chatbot** | SFT → DPO on internal human-labeled preference pairs |
| **Safety alignment** | DPO on safety-specific preference data (harmful vs safe) |
| **Scaling to 70B+** | Use LoRA + DPO (QLoRA) to fit in limited GPU memory |
| **No paired data** | Use KTO (only needs per-response quality labels) |

> ⚠️ **Common pitfalls:** (1) Forgetting to freeze the reference model, (2) Learning rate too high → reward hacking, (3) Training too many epochs → overfitting to preference data, (4) Poor-quality preference data → garbage in, garbage out.

---

# 📚 Research Citations

| # | Paper | Authors | Year | Key Contribution |
|---|-------|---------|------|------------------|
| 1 | *Direct Preference Optimization: Your Language Model is Secretly a Reward Model* | Rafailov, Sharma, Mitchell, Manning, Ermon, Finn | 2023 | ⭐ The DPO paper — shows closed-form solution eliminates reward modeling |
| 2 | *Training language models to follow instructions with human feedback* (InstructGPT) | Ouyang, Wu, Jiang, Almeida et al. | 2022 | Established the RLHF pipeline DPO improves upon |
| 3 | *A General Theoretical Paradigm to Understand Learning from Human Feedback* (IPO) | Azar, Rowland, Piot, Guo, Calandriello et al. | 2023 | Identity-based alternative avoiding DPO overfitting |
| 4 | *KTO: Model Alignment as Prospect Theoretic Optimization* | Ethayarajh, Xu, Muennighoff, Jurafsky, Kiela | 2024 | Aligns with unpaired preferences using Kahneman-Tversky theory |
| 5 | *ORPO: Monolithic Preference Optimization without Reference Model* | Hong, Lee, Thorne et al. | 2024 | Merges SFT + preference optimization |

---

# 📖 Resources

### Books
- 📕 *Reinforcement Learning from Human Feedback* — a rapidly evolving field; see survey papers below
- 📗 *Deep Learning* by Goodfellow, Bengio, Courville — Ch. 6 (optimization) provides the math foundations
- 📘 *Speech and Language Processing* by Jurafsky & Martin — LLM alignment chapters (online edition)

### Papers (Must-Read)
- ⭐ [DPO Paper](https://arxiv.org/abs/2305.18290) — Rafailov et al. (2023)
- ⭐ [InstructGPT / RLHF](https://arxiv.org/abs/2203.02155) — Ouyang et al. (2022)
- [IPO Paper](https://arxiv.org/abs/2310.12036) — Azar et al. (2023)
- [KTO Paper](https://arxiv.org/abs/2402.01306) — Ethayarajh et al. (2024)
- [RLHF Survey: Open Problems and Fundamental Limitations](https://arxiv.org/abs/2307.15217) — Casper et al. (2023)

### Libraries & Tools
- 🛠️ [TRL (Transformer Reinforcement Learning)](https://github.com/huggingface/trl) — HuggingFace's library with `DPOTrainer` class
- 🛠️ [OpenRLHF](https://github.com/OpenRLHF/OpenRLHF) — Scalable RLHF/DPO framework
- 🛠️ [Axolotl](https://github.com/OpenAccess-AI-Collective/axolotl) — Fine-tuning framework supporting DPO

### Datasets
- 📦 [UltraFeedback](https://huggingface.co/datasets/openbmb/UltraFeedback) — 64K preference pairs
- 📦 [Anthropic HH-RLHF](https://huggingface.co/datasets/Anthropic/hh-rlhf) — Helpfulness & harmlessness preferences
- 📦 [Nectar](https://huggingface.co/datasets/berkeley-nest/Nectar) — 183K preference rankings
