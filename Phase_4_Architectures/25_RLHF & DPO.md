📘 DAY 25 — RLHF & DPO: Aligning Language Models with Human Preferences

Goal:

Understand the complete alignment pipeline — from Reinforcement Learning from Human Feedback (RLHF) to Direct Preference Optimization (DPO). Master WHY alignment is needed beyond SFT, HOW preference data is collected, and the mathematical foundations of both approaches.

---

> 🎯 **THE BIG PICTURE — Why This Matters**
>
> Imagine you've taught a parrot to speak English perfectly. It knows every word, every grammar rule. But it still says wildly inappropriate things at dinner parties. 🦜💬
>
> That's basically what happens with SFT (Supervised Fine-Tuning) — the model learns *language* but not *judgment*. RLHF and DPO are how we teach the model to have good judgment — to be helpful, harmless, and honest.
>
> **This is the technology that turned GPT-3 (impressive but unpredictable) into ChatGPT (the product millions use daily).** Without alignment, no AI product ships safely.

---

> 🧠 **BEGINNER ANALOGY — The Dog Training Story** 🐕
>
> Think of the entire alignment pipeline like training a dog:
>
> | Stage | Dog Training | AI Alignment |
> |-------|-------------|--------------|
> | **Pre-training** | The dog learns what words mean | LLM learns language from the internet |
> | **SFT** | You show the dog "sit" and it copies you | Model learns to follow instructions |
> | **RLHF** | You give treats 🦴 for good behavior, no treats for bad | Model gets reward scores for good responses |
> | **DPO** | You show the dog two behaviors and say "this one is better" | Model directly learns from preference pairs |
> | **KL Penalty** | A leash 🔗 so the dog doesn't run wild | Prevents the model from straying too far from SFT |
> | **Reward Hacking** | Dog learns to *look cute* instead of actually obeying | Model finds loopholes in the reward system |
>
> Keep this analogy in mind — we'll reference it throughout! 🐾

---

# 1️⃣ Why SFT Isn't Enough

## SFT teaches WHAT to say, not HOW WELL to say it

SFT with cross-entropy loss treats all correct tokens equally.
But humans have PREFERENCES — some responses are better than others.

Example:
- Response A: "The capital of France is Paris. It's been the capital since the 10th century."
- Response B: "Paris."

Both are "correct," but A is more helpful. SFT can't distinguish this.

> 💡 **Beginner Analogy**: Imagine you're grading essays. SFT is like giving full marks to every essay that has the right answer — whether it's a brilliant paragraph or a single word. But in real life, the *quality* of the answer matters, not just correctness!

## The Alignment Gap

SFT model issues:
1. **Sycophancy**: Agrees with user even when wrong
2. **Verbosity**: Overly long responses
3. **Hallucination**: Confidently states false information
4. **Safety**: May generate harmful content

RLHF/DPO addresses these by optimizing for human PREFERENCES, not just correctness.

> ⭐ **KEY INSIGHT**: SFT minimizes $\mathcal{L}_{\text{SFT}} = -\sum_t \log P(y_t | y_{<t}, x)$ — this loss function has NO concept of "better" vs "worse." It only knows "right token" vs "wrong token." Alignment adds the missing ingredient: **human preference signal**.

> 🏭 **PRODUCTION REALITY**: Before RLHF, GPT-3 (2020) was powerful but chaotic — it could generate toxic text, make up facts, and ignore user intent. After adding RLHF, InstructGPT (same model, same parameters) became dramatically more useful and safe. OpenAI reported that InstructGPT with 1.3B parameters was preferred over GPT-3 with 175B parameters! (Ouyang et al., 2022)

---

# 2️⃣ RLHF Pipeline Overview

> 🐕 **Back to the Dog Analogy**: RLHF has three stages — (1) record what "good behavior" looks like by asking humans, (2) train an automated judge (reward model) so you don't need a human watching 24/7, (3) let the dog practice and get scored by the automated judge. The leash (KL penalty) keeps things from going off the rails.

```
Step 1: Collect Preference Data
   Humans compare pairs of responses: "A is better than B"

Step 2: Train Reward Model
   Learn to predict human preferences: R(prompt, response) → score

Step 3: RL Fine-tuning (PPO)
   Optimize policy (LM) to maximize reward while staying close to SFT model
```

> ⭐ **THE CORE MATH in one line**:
>
> $$\max_{\pi} \; \mathbb{E}_{x \sim \mathcal{D}, \, y \sim \pi(\cdot|x)} \Big[ r(x, y) - \beta \cdot D_{\text{KL}}\big(\pi(\cdot|x) \;\|\; \pi_{\text{ref}}(\cdot|x)\big) \Big]$$
>
> In words: **maximize reward** $r(x,y)$ while paying a **penalty** $\beta \cdot D_{\text{KL}}$ for straying too far from the reference (SFT) model.
>
> - $\pi$ = the policy (model being trained)
> - $\pi_{\text{ref}}$ = reference policy (frozen SFT model)
> - $r(x,y)$ = reward model score
> - $\beta$ = how tight the "leash" is (typically 0.01–0.2)

> 📊 **Visual Pipeline**:
> ```
> ┌──────────────┐    ┌──────────────────┐    ┌─────────────────┐
> │  Human        │───▶│  Reward Model     │───▶│  PPO Training    │
> │  Preferences  │    │  (Learns to       │    │  (Optimizes LM   │
> │  "A > B"      │    │   score responses) │    │   for reward)    │
> └──────────────┘    └──────────────────┘    └─────────────────┘
>                                                      │
>                                              ┌───────▼────────┐
>                                              │  KL Penalty     │
>                                              │  (The Leash 🔗) │
>                                              └────────────────┘
> ```

> 🔬 **RESEARCH CONTEXT**: The RLHF pipeline was first proposed by Christiano et al. (2017) in "Deep Reinforcement Learning from Human Preferences" for Atari games and robotic tasks. Ouyang et al. (2022) at OpenAI adapted it for language models in the landmark InstructGPT paper, which became the foundation for ChatGPT.

---

# 3️⃣ Step 1: Preference Data Collection

> 🐕 **Dog Analogy**: This is like recording videos of your dog doing things, then having people vote: "Was behavior A or behavior B better?" You're collecting human opinions to teach the automated judge.

```python
# Preference data format
preference_example = {
    "prompt": "Explain photosynthesis simply.",
    "chosen": "Photosynthesis is how plants make food from sunlight...",     # Better
    "rejected": "Photosynthesis is a biochemical process involving...",      # Worse
}

# Human annotation interface:
# Show prompt + two responses → annotator picks better one
# Important: randomize order (remove position bias)
```

> 💡 **Beginner Analogy**: Think of preference data like a taste test. 🧋 You don't tell someone what "good coffee" is by listing ingredients — you give them two cups and ask "Which one do you prefer?" Thousands of these comparisons teach the system what "good" means.

## Data Quality for Preferences

- Need 10K-100K comparisons for good reward model
- Annotator agreement should be >70% (task is subjective)
- Diverse prompts covering many topics
- Include safety-relevant comparisons

> ⭐ **PRODUCTION DATA SCALE** (from public papers):
>
> | Company/Model | Preference Pairs | Annotators | Cost Estimate |
> |---------------|-----------------|------------|---------------|
> | InstructGPT (OpenAI) | ~50K comparisons | 40 contractors | ~$500K+ |
> | LLaMA-2-Chat (Meta) | ~1.4M comparisons | Internal team | Undisclosed |
> | Anthropic (Claude) | ~250K comparisons | Crowd + experts | Undisclosed |
> | Open-source (UltraFeedback) | ~64K comparisons | GPT-4 as judge | ~$5K API costs |
>
> Fun fact: Many modern open-source models use **AI feedback** (GPT-4 as judge) instead of human annotators. This is called **RLAIF** — Reinforcement Learning from AI Feedback. 🤖➡️🤖

> 🔬 **RESEARCH NOTE**: The quality of preference data is arguably MORE important than the alignment algorithm choice. Bai et al. (2022) from Anthropic showed that even simple RLHF with excellent preference data outperforms sophisticated algorithms with noisy data. "Garbage in, garbage out" applies strongly here.

---

# 4️⃣ Step 2: Reward Model Training

> 🐕 **Dog Analogy**: The reward model is like an **automated dog trainer**. Instead of having a human watch the dog 24/7, you train a robot to give treats when the dog does well. The robot learned what "good behavior" looks like from watching thousands of human ratings.

## Architecture
Take the SFT model, replace LM head with a scalar output:

```python
class RewardModel(nn.Module):
    def __init__(self, base_model):
        super().__init__()
        self.backbone = base_model  # Transformer layers
        self.reward_head = nn.Linear(base_model.config.hidden_size, 1)
    
    def forward(self, input_ids, attention_mask=None):
        # Get last hidden state
        outputs = self.backbone(input_ids, attention_mask=attention_mask)
        hidden = outputs.last_hidden_state
        
        # Pool: take last non-padding token
        if attention_mask is not None:
            last_idx = attention_mask.sum(dim=1) - 1
            pooled = hidden[torch.arange(hidden.size(0)), last_idx]
        else:
            pooled = hidden[:, -1, :]
        
        return self.reward_head(pooled).squeeze(-1)  # Scalar reward
```

> 💡 **Beginner Breakdown of the Code Above**:
> 1. Take a pre-trained language model (the `backbone`) — it already understands language
> 2. Remove the "predict next word" head
> 3. Add a single number output (the `reward_head`) — this outputs a **score**
> 4. For any text input, the model outputs: "I rate this response a 7.3 out of 10" (a scalar)

## Bradley-Terry Loss

> ⭐ **THE BRADLEY-TERRY MODEL — How Preferences Become Math**

Given preference pair (chosen, rejected):

L = -log(σ(R(chosen) - R(rejected)))

Where σ is sigmoid. This loss:
- Pushes R(chosen) > R(rejected)
- Uses the DIFFERENCE, not absolute values
- Same as binary cross-entropy on "chosen is better"

> 🧮 **LaTeX formulation of the Bradley-Terry preference model**:
>
> The probability that response $y_w$ (winner) is preferred over $y_l$ (loser):
>
> $$P(y_w \succ y_l) = \sigma\big(r(y_w) - r(y_l)\big) = \frac{1}{1 + e^{-(r(y_w) - r(y_l))}}$$
>
> The training loss over a dataset $\mathcal{D}$ of preference pairs:
>
> $$\mathcal{L}_{\text{RM}} = -\mathbb{E}_{(x, y_w, y_l) \sim \mathcal{D}} \Big[\log \sigma\big(r(x, y_w) - r(x, y_l)\big)\Big]$$
>
> 🎯 **Why the difference matters**: If $r(y_w) = 8$ and $r(y_l) = 3$, the sigmoid of 5 is ≈0.993 (very confident). If $r(y_w) = 5.1$ and $r(y_l) = 4.9$, the sigmoid of 0.2 is ≈0.55 (barely confident). The loss only cares about the **gap**, not absolute values.

> 💡 **Beginner Analogy**: The Bradley-Terry model is like an Elo rating system in chess ♟️. When a 2000-rated player beats a 1500-rated player, the gap (500) predicts the win probability. Similarly, bigger reward gaps = higher confidence that the chosen response is better.

```python
def reward_model_loss(reward_model, chosen_ids, rejected_ids, chosen_mask, rejected_mask):
    """Compute Bradley-Terry preference loss."""
    r_chosen = reward_model(chosen_ids, chosen_mask)
    r_rejected = reward_model(rejected_ids, rejected_mask)
    
    loss = -torch.log(torch.sigmoid(r_chosen - r_rejected)).mean()
    
    # Accuracy metric
    accuracy = (r_chosen > r_rejected).float().mean()
    
    return loss, accuracy
```

> 🔬 **RESEARCH DEPTH**: The Bradley-Terry model dates back to 1952 (!) in psychometrics for modeling paired comparisons. Its use in RLHF was popularized by Christiano et al. (2017). An alternative is the **Plackett-Luce** model for ranking more than 2 items, and **Thurstone** models which assume Gaussian noise instead of logistic noise.

---

# 5️⃣ Step 3: PPO Fine-tuning

> 🐕 **Dog Analogy**: This is the actual training phase! The dog (model) tries different behaviors, the automated judge (reward model) scores them, and the leash (KL penalty) prevents the dog from going crazy. PPO is the specific training method — it updates the dog's behavior in small, safe steps.

## The Objective

Maximize: E[R(prompt, response)] - β × KL(π || π_ref)

Where:
- R: learned reward model
- π: current policy (model being trained)
- π_ref: reference policy (SFT model, frozen)
- β: KL penalty coefficient

The KL term prevents the model from diverging too far from the SFT model (which could lead to reward hacking).

> ⭐ **THE PPO OBJECTIVE — Full LaTeX** 🧮
>
> $$\max_{\pi_\theta} \; \mathbb{E}_{x \sim \mathcal{D}, \, y \sim \pi_\theta(\cdot|x)} \Big[ r_\phi(x, y) \Big] - \beta \cdot D_{\text{KL}}\Big(\pi_\theta(\cdot|x) \;\Big\|\; \pi_{\text{ref}}(\cdot|x)\Big)$$
>
> Expanding the KL divergence at the token level:
>
> $$D_{\text{KL}}\big(\pi_\theta \| \pi_{\text{ref}}\big) = \sum_{t=1}^{T} \mathbb{E}_{y_t \sim \pi_\theta} \left[ \log \frac{\pi_\theta(y_t | y_{<t}, x)}{\pi_{\text{ref}}(y_t | y_{<t}, x)} \right]$$
>
> The per-token adjusted reward becomes:
>
> $$\tilde{r}_t = r_\phi(x, y) \cdot \mathbb{1}[t = T] - \beta \cdot \log \frac{\pi_\theta(y_t | y_{<t}, x)}{\pi_{\text{ref}}(y_t | y_{<t}, x)}$$
>
> 🎯 **In plain English**: "Generate good responses (↑ reward) but don't become a different model (↓ KL divergence)."

> 💡 **Beginner Analogy — Why the KL Penalty?** 🔗
>
> Imagine you're a restaurant owner training a new chef. The chef discovers that putting MSG in everything gets high customer ratings (reward hacking!). The KL penalty is like saying "your dishes should still resemble the original recipes." It prevents the chef from going off the rails and just dumping flavor enhancers on everything.
>
> Without KL penalty → model learns to exploit the reward model's blind spots
> With KL penalty → model stays close to what it already knew (SFT), with targeted improvements

> 📊 **β TUNING GUIDE**:
>
> | β Value | Effect | When to Use |
> |---------|--------|-------------|
> | 0.001 | Very loose leash — model changes a LOT | When SFT model is poor |
> | 0.01 | Standard — balanced | Default starting point |
> | 0.1 | Tight leash — small changes only | When SFT model is already good |
> | 0.5+ | Very tight — almost no change | When you want minimal drift |

## Simplified PPO for LLMs

```python
def ppo_step(policy_model, ref_model, reward_model, prompts, tokenizer, 
             kl_coeff=0.1, clip_epsilon=0.2):
    """One PPO training step."""
    
    # 1. Generate responses with current policy
    with torch.no_grad():
        responses = policy_model.generate(prompts, max_new_tokens=256, do_sample=True, temperature=0.7)
    
    # 2. Compute rewards
    with torch.no_grad():
        rewards = reward_model(responses)
    
    # 3. Compute log probabilities
    policy_logprobs = compute_logprobs(policy_model, responses)
    with torch.no_grad():
        ref_logprobs = compute_logprobs(ref_model, responses)
    
    # 4. KL penalty
    kl = policy_logprobs - ref_logprobs  # Per-token KL
    adjusted_rewards = rewards - kl_coeff * kl.sum(dim=-1)
    
    # 5. Compute advantages (simplified: reward - baseline)
    baseline = adjusted_rewards.mean()
    advantages = adjusted_rewards - baseline
    
    # 6. PPO clipped objective
    ratio = torch.exp(policy_logprobs - policy_logprobs.detach())
    clipped_ratio = torch.clamp(ratio, 1 - clip_epsilon, 1 + clip_epsilon)
    
    policy_loss = -torch.min(ratio * advantages, clipped_ratio * advantages).mean()
    
    return policy_loss

def compute_logprobs(model, sequences):
    """Compute per-token log probabilities."""
    logits = model(sequences[:, :-1]).logits
    log_probs = F.log_softmax(logits, dim=-1)
    token_log_probs = log_probs.gather(-1, sequences[:, 1:].unsqueeze(-1)).squeeze(-1)
    return token_log_probs
```

> 💡 **Beginner Walkthrough of PPO Steps**:
>
> 1. **Generate**: The model writes a response to each prompt (like a student writing an essay) ✍️
> 2. **Score**: The reward model grades each response (the teacher scores it) 📝
> 3. **Compare**: We check how differently the model behaves vs its original self (KL) 📏
> 4. **Adjust reward**: Subtract a penalty for being "too different" from original 🔗
> 5. **Compute advantage**: "Was this response better or worse than average?" 📊
> 6. **Update carefully**: PPO clips the update so we don't take too big a step 🐢
>
> The **clipping** (step 6) is PPO's secret sauce — it prevents catastrophically large updates. The ratio $\frac{\pi_\theta(y|x)}{\pi_{\theta_{\text{old}}}(y|x)}$ is clamped to $[1-\epsilon, 1+\epsilon]$ (typically $\epsilon = 0.2$).

> 🏭 **PRODUCTION CHALLENGES WITH PPO**:
> - Need to run 4 models simultaneously: **policy** + **reference** + **reward** + **value network** 🧠🧠🧠🧠
> - Memory requirement: ~4× the model size (for a 7B model, you need ~112GB VRAM)
> - Training is unstable — reward can collapse, KL can explode
> - That's why DPO (next section) became so popular — it avoids all of this!

---

# 6️⃣ DPO: Direct Preference Optimization (Rafailov et al., 2023)

## The Breakthrough

DPO eliminates the reward model entirely!

> 🎉 **This is a BIG deal.** Imagine you could skip building the automated dog trainer entirely, and instead just show the dog pairs of behaviors directly: "This one good ✅, this one bad ❌." That's DPO — it cuts out the middleman!

Key insight: the optimal policy under the RLHF objective has a closed-form solution:

π*(y|x) = (1/Z(x)) · π_ref(y|x) · exp(R(y,x) / β)

Rearranging: R(y,x) = β · log(π*(y|x) / π_ref(y|x)) + β · log Z(x)

Substituting into the Bradley-Terry loss:

L_DPO = -log σ(β · [log(π(y_w|x)/π_ref(y_w|x)) - log(π(y_l|x)/π_ref(y_l|x))])

Where y_w = chosen (winner), y_l = rejected (loser).

**No reward model needed. Train directly on preferences.**

> ⭐ **THE DPO DERIVATION — Full LaTeX** 🧮
>
> **Step 1**: The optimal RLHF policy has the form:
>
> $$\pi^*(y|x) = \frac{1}{Z(x)} \pi_{\text{ref}}(y|x) \exp\!\left(\frac{r(x,y)}{\beta}\right)$$
>
> **Step 2**: Rearrange to express the reward in terms of the policy:
>
> $$r(x,y) = \beta \log \frac{\pi^*(y|x)}{\pi_{\text{ref}}(y|x)} + \beta \log Z(x)$$
>
> **Step 3**: Substitute into the Bradley-Terry model. The partition function $Z(x)$ cancels out!
>
> $$\boxed{\mathcal{L}_{\text{DPO}}(\pi_\theta; \pi_{\text{ref}}) = -\mathbb{E}_{(x, y_w, y_l) \sim \mathcal{D}} \left[ \log \sigma \left( \beta \left( \log \frac{\pi_\theta(y_w|x)}{\pi_{\text{ref}}(y_w|x)} - \log \frac{\pi_\theta(y_l|x)}{\pi_{\text{ref}}(y_l|x)} \right) \right) \right]}$$
>
> Where:
> - $y_w$ = chosen/winner response ✅
> - $y_l$ = rejected/loser response ❌
> - $\beta$ = temperature controlling preference strength
> - $\pi_\theta$ = model being trained
> - $\pi_{\text{ref}}$ = frozen reference (SFT) model
>
> 🎯 **Intuition**: The loss says "increase the probability of the winner (relative to the reference) and decrease the probability of the loser (relative to the reference)." The $\beta$ controls how aggressively.

> 💡 **Beginner Analogy — DPO as a Shortcut** 🛤️
>
> RLHF is like:
> 1. Hire food critics 👩‍🍳
> 2. Train a robot food critic based on their ratings 🤖
> 3. Have the robot guide the chef's cooking 🍳
>
> DPO is like:
> 1. Hire food critics 👩‍🍳
> 2. Show the chef both dishes directly: "this one's better" 🍳✅
>
> DPO skips step 2 (the robot critic / reward model) entirely! The math proves this gives the same result.

> 🧮 **WHY THE PARTITION FUNCTION CANCELS** (deep dive):
>
> In the Bradley-Terry model, we compute $r(y_w) - r(y_l)$. Since:
>
> $$r(y) = \beta \log \frac{\pi^*(y|x)}{\pi_{\text{ref}}(y|x)} + \beta \log Z(x)$$
>
> The $\beta \log Z(x)$ term is the SAME for both $y_w$ and $y_l$ (it only depends on $x$), so it cancels in the subtraction! This is the key mathematical trick that makes DPO possible.

## Implementation

```python
def dpo_loss(policy_model, ref_model, chosen_ids, rejected_ids, 
             chosen_mask, rejected_mask, beta=0.1):
    """Direct Preference Optimization loss."""
    
    # Compute log probabilities for chosen and rejected under both models
    policy_chosen_logps = get_sequence_logprobs(policy_model, chosen_ids, chosen_mask)
    policy_rejected_logps = get_sequence_logprobs(policy_model, rejected_ids, rejected_mask)
    
    with torch.no_grad():
        ref_chosen_logps = get_sequence_logprobs(ref_model, chosen_ids, chosen_mask)
        ref_rejected_logps = get_sequence_logprobs(ref_model, rejected_ids, rejected_mask)
    
    # Log ratios
    chosen_ratio = policy_chosen_logps - ref_chosen_logps
    rejected_ratio = policy_rejected_logps - ref_rejected_logps
    
    # DPO loss
    logits = beta * (chosen_ratio - rejected_ratio)
    loss = -F.logsigmoid(logits).mean()
    
    # Metrics
    with torch.no_grad():
        chosen_reward = beta * chosen_ratio
        rejected_reward = beta * rejected_ratio
        accuracy = (chosen_reward > rejected_reward).float().mean()
        reward_margin = (chosen_reward - rejected_reward).mean()
    
    return loss, accuracy, reward_margin

def get_sequence_logprobs(model, input_ids, attention_mask):
    """Get total log probability of a sequence."""
    logits = model(input_ids, attention_mask=attention_mask).logits
    
    # Shift for next-token prediction
    logits = logits[:, :-1, :]
    labels = input_ids[:, 1:]
    mask = attention_mask[:, 1:]
    
    log_probs = F.log_softmax(logits, dim=-1)
    token_log_probs = log_probs.gather(-1, labels.unsqueeze(-1)).squeeze(-1)
    
    # Mask padding and sum
    return (token_log_probs * mask).sum(dim=-1)
```

> 💡 **Beginner Walkthrough of DPO Code**:
>
> 1. **Forward pass × 4**: Run both the policy and reference model on both chosen and rejected responses
> 2. **Log ratios**: For each response, compute "how much more likely is this under the new model vs the old model?"
> 3. **DPO logits**: $\beta \times (\text{chosen\_ratio} - \text{rejected\_ratio})$ — the core DPO formula
> 4. **Loss**: Push this value to be POSITIVE (chosen should be relatively more likely)
> 5. **Metrics**: Track accuracy (how often chosen scores higher) and reward margin
>
> ⭐ The `chosen_ratio` corresponds to $\log \frac{\pi_\theta(y_w|x)}{\pi_{\text{ref}}(y_w|x)}$ in the math above!

---

# 7️⃣ Training DPO with TRL

> 🏭 **PRODUCTION NOTE**: TRL (Transformer Reinforcement Learning) by Hugging Face is the go-to library for alignment in practice. This code below is essentially what companies use to fine-tune open-source models. Zephyr-7B, one of the best small open models, was trained with exactly this approach (Tunstall et al., 2023).

```python
from trl import DPOTrainer, DPOConfig
from datasets import load_dataset

# Load preference dataset
dataset = load_dataset("HuggingFaceH4/ultrafeedback_binarized")

# DPO training config
dpo_config = DPOConfig(
    output_dir="./dpo_model",
    num_train_epochs=1,
    per_device_train_batch_size=4,
    gradient_accumulation_steps=4,
    learning_rate=5e-7,          # Very low LR for DPO
    beta=0.1,                     # KL penalty strength
    loss_type="sigmoid",          # Standard DPO loss
    warmup_ratio=0.1,
    lr_scheduler_type="cosine",
    bf16=True,
    gradient_checkpointing=True,
    logging_steps=10,
)

# Train
trainer = DPOTrainer(
    model=policy_model,
    ref_model=ref_model,
    args=dpo_config,
    train_dataset=dataset['train'],
    tokenizer=tokenizer,
)
trainer.train()
```

> 💡 **Beginner Notes on the Config Above**:
>
> | Parameter | Value | Why? |
> |-----------|-------|------|
> | `learning_rate=5e-7` | 0.0000005 | DPO needs TINY steps — too fast = catastrophe 🐢 |
> | `beta=0.1` | The leash tightness | How much we penalize straying from original model |
> | `loss_type="sigmoid"` | Standard DPO | Other options: `hinge`, `ipo` (variants of DPO) |
> | `num_train_epochs=1` | Just 1 pass | DPO overfits quickly — 1 epoch is usually enough! |
> | `gradient_checkpointing=True` | Save memory | Trades compute for memory — essential for large models |
> | `bf16=True` | Half precision | 2× memory saving with minimal quality loss |

> ⭐ **POPULAR DPO DATASETS used in production**:
>
> | Dataset | Size | Source | Used By |
> |---------|------|--------|---------|
> | UltraFeedback | 64K pairs | GPT-4 judgments | Zephyr, Neural-Chat |
> | HH-RLHF | 170K pairs | Human annotators | Anthropic research |
> | OpenOrca DPO | 12K pairs | GPT-4 comparisons | Open-source community |
> | Nectar | 183K pairs | Multi-model | Starling-7B |
> | Argilla DPO | 10K pairs | Curated human | Various open models |

---

# 8️⃣ RLHF vs DPO Comparison

> ⭐ **THE ULTIMATE COMPARISON TABLE**

| Aspect | RLHF (PPO) | DPO |
|--------|-----------|-----|
| Reward model | Required (separate training) | Not needed |
| Training complexity | High (RL loop, value network) | Low (supervised-style) |
| Memory | High (policy + ref + reward + value) | Moderate (policy + ref) |
| Stability | Tricky (reward hacking, mode collapse) | More stable |
| Performance | Slightly better (when tuned well) | Competitive |
| Iteration speed | Slow | Fast |

> 📊 **EXPANDED COMPARISON — The Full Picture**:
>
> | Aspect | RLHF (PPO) | DPO | KTO |
> |--------|-----------|-----|-----|
> | **Models in memory** | 4 (policy + ref + reward + value) | 2 (policy + ref) | 2 (policy + ref) |
> | **GPU memory (7B model)** | ~112 GB | ~56 GB | ~56 GB |
> | **Training time** | 2-5× longer | 1× (baseline) | 1× (baseline) |
> | **Data requirement** | Preference pairs | Preference pairs | Binary (good/bad) only! |
> | **Hyperparameter sensitivity** | Very high ⚠️ | Moderate | Low |
> | **Online data generation** | Yes (generates during training) | No (offline) | No (offline) |
> | **Can reward hack?** | Yes — major problem | Less likely | Less likely |
> | **Research maturity** | Most studied (since 2017) | Growing fast (since 2023) | Newest (2024) |
> | **Code complexity** | ~500 lines of RL logic | ~50 lines of loss | ~50 lines of loss |

> 🏭 **WHO USES WHAT in Production** (as of 2024-2025):
>
> | Model | Alignment Method | Notes |
> |-------|-----------------|-------|
> | **ChatGPT / GPT-4** (OpenAI) | RLHF (PPO) | Pioneered the approach; massive compute budget |
> | **Claude** (Anthropic) | Constitutional AI + RLHF | Uses AI-generated feedback + human RLHF |
> | **LLaMA-2-Chat** (Meta) | RLHF (PPO) | 1.4M human preference annotations |
> | **Gemini** (Google) | RLHF variant | Details partially disclosed |
> | **Zephyr-7B** (HuggingFace) | DPO | First major DPO success story |
> | **Mistral-Instruct** | DPO | Simpler alignment, great results |
> | **Starling-7B** | DPO (on Nectar dataset) | Top open-source model at release |
> | **Most open-source models** | DPO | Easier to implement, cheaper, good enough |
>
> 🎯 **Trend**: Big labs with huge budgets (OpenAI, Anthropic) use RLHF. Open-source community overwhelmingly uses DPO because it's simpler and cheaper.

## When to Use Which?

- **DPO**: Default choice. Simpler, faster, good results.
- **RLHF**: When you have a well-calibrated reward model and need maximum performance.
- **KTO** (Ethayarajh 2024): When you only have binary feedback (good/bad), not comparisons.

> 💡 **Beginner Decision Tree** 🌳:
>
> ```
> Do you have paired preference data (chosen vs rejected)?
> ├── YES → Do you have a massive compute budget + RL expertise?
> │         ├── YES → Use RLHF (PPO) for potentially best results
> │         └── NO  → Use DPO ✅ (recommended default)
> └── NO  → Do you have binary labels only (good/bad)?
>           ├── YES → Use KTO (Ethayarajh et al., 2024)
>           └── NO  → Collect preference data first!
> ```

---

# 9️⃣ Alignment Pitfalls

> ⚠️ **These pitfalls are NOT theoretical — they have crashed real production systems!**

### Reward Hacking
The model exploits reward model weaknesses (generates text that scores high but isn't actually good).
Prevention: Strong KL penalty, diverse reward model training.

> 🐕 **Dog Analogy**: Your dog learns that rolling over and looking cute gets treats — even when you asked it to fetch the ball. It's "hacking" the reward system by doing something easy that scores high, instead of what you actually wanted.
>
> 💡 **Real Example**: An RLHF-trained model might learn to write longer responses (because the reward model was trained on data where longer = better), even when a short answer is more helpful. Or it might add excessive caveats ("As an AI...") because that pattern scored well during training.
>
> ⭐ **The Math Behind Reward Hacking Prevention**:
>
> The KL penalty $\beta \cdot D_{\text{KL}}(\pi \| \pi_{\text{ref}})$ prevents this by creating a cost for deviating from the original model:
>
> $$\text{Effective reward} = r(x,y) - \beta \sum_{t} \log \frac{\pi_\theta(y_t|y_{<t},x)}{\pi_{\text{ref}}(y_t|y_{<t},x)}$$
>
> Even if the model finds a "hack" that scores $r = 10$, if it requires big KL divergence, the effective reward drops significantly.

### Sycophancy After RLHF
Model agrees with user to get higher reward scores.
Prevention: Include disagreement examples in preference data.

> 💡 **Real Example**: User says "The earth is flat, right?" and the RLHF model responds "Yes, you make a great point!" because agreeing with users tends to get higher human preference ratings. This is a known problem documented by Perez et al. (2022) at Anthropic.

### Alignment Tax
Aligned models may perform worse on benchmarks (safety constraints limit capability).
This is expected and acceptable — safety > benchmarks.

> 🏭 **Production Reality**: LLaMA-2-Chat (aligned) scores slightly lower on pure knowledge benchmarks vs LLaMA-2 (base). But no company would deploy the base model — the aligned version is dramatically more useful and safe. The "alignment tax" is like the "seatbelt tax" — yes, seatbelts add weight to a car, but you'd never remove them. 🚗

> ⭐ **ADDITIONAL PITFALLS every practitioner should know**:
>
> | Pitfall | Description | Prevention |
> |---------|-------------|------------|
> | **Mode collapse** | Model generates same/similar outputs for all prompts | Lower learning rate, increase β |
> | **Length gaming** | Model learns "longer = better" reward signal | Normalize reward by length |
> | **Catastrophic forgetting** | Loses pre-training knowledge during RLHF | Strong KL penalty, short training |
> | **Annotation noise** | Disagreement between human raters (30-40% is common!) | More annotators, better guidelines |
> | **Distribution shift** | Reward model trained on different data than deployment | Iterative training, on-policy data |
> | **Goodhart's Law** | "When a measure becomes a target, it ceases to be a good measure" | Ensemble reward models |

---

# 📝 Summary

| Concept | Key Insight |
|---------|-------------|
| RLHF | Train reward model → RL optimize policy |
| DPO | Direct optimization on preferences, no reward model |
| Bradley-Terry | Preference model: P(A>B) = σ(R(A) - R(B)) |
| KL penalty | Prevents divergence from base model |
| β parameter | Controls preference strength vs staying close to ref |
| Reward hacking | Model exploits reward model flaws |
| Learning rate | Very low for DPO: 5e-7 to 5e-6 |

---

# 🧮 FORMULA CHEAT SHEET (All Key Equations)

> **Keep this page bookmarked — every formula you need in one place!** 🔖

| Formula | LaTeX | Meaning |
|---------|-------|---------|
| **Bradley-Terry** | $P(y_w \succ y_l) = \sigma\big(r(y_w) - r(y_l)\big)$ | Probability winner is preferred |
| **Reward Model Loss** | $\mathcal{L}_{\text{RM}} = -\mathbb{E}\big[\log \sigma(r(y_w) - r(y_l))\big]$ | Train reward model on preferences |
| **RLHF Objective** | $\max_\pi \mathbb{E}[r(x,y)] - \beta \cdot D_{\text{KL}}(\pi \| \pi_{\text{ref}})$ | Maximize reward with KL leash |
| **Optimal Policy** | $\pi^*(y|x) = \frac{1}{Z(x)} \pi_{\text{ref}}(y|x) \exp\!\left(\frac{r(x,y)}{\beta}\right)$ | Closed-form solution to RLHF |
| **Implicit Reward** | $r(x,y) = \beta \log \frac{\pi^*(y|x)}{\pi_{\text{ref}}(y|x)} + \beta \log Z(x)$ | Extract reward from policy |
| **DPO Loss** | $\mathcal{L}_{\text{DPO}} = -\mathbb{E}\!\left[\log \sigma\!\left(\beta\!\left(\log \frac{\pi_\theta(y_w|x)}{\pi_{\text{ref}}(y_w|x)} - \log \frac{\pi_\theta(y_l|x)}{\pi_{\text{ref}}(y_l|x)}\right)\right)\right]$ | Direct preference optimization |
| **KL Divergence** | $D_{\text{KL}}(\pi \| \pi_{\text{ref}}) = \sum_t \mathbb{E}\!\left[\log \frac{\pi(y_t|y_{<t},x)}{\pi_{\text{ref}}(y_t|y_{<t},x)}\right]$ | Measures policy drift |
| **PPO Clip** | $\mathcal{L}_{\text{PPO}} = -\min(r_t A_t, \text{clip}(r_t, 1\pm\epsilon) A_t)$ | Clipped policy gradient |

---

# 🔬 DEEP RESEARCH — Key Papers & Citations

> **The papers that built the alignment field, in chronological order:**

| Year | Paper | Authors | Key Contribution | Citation |
|------|-------|---------|-----------------|----------|
| 2017 | **Deep RL from Human Preferences** | Christiano et al. | First RLHF framework — trained Atari agents from human feedback | arXiv: 1706.03741 |
| 2020 | **Learning to Summarize from Human Feedback** | Stiennon et al. (OpenAI) | Applied RLHF to NLP (text summarization) | arXiv: 2009.01325 |
| 2022 | **Training language models to follow instructions (InstructGPT)** | Ouyang et al. (OpenAI) | Full RLHF pipeline for LLMs — foundation for ChatGPT | arXiv: 2203.02155 |
| 2022 | **Training a Helpful and Harmless Assistant (HH-RLHF)** | Bai et al. (Anthropic) | Constitutional AI + RLHF; safety-focused alignment | arXiv: 2204.05862 |
| 2023 | **Direct Preference Optimization (DPO)** | Rafailov et al. (Stanford) | Eliminated reward model — the DPO breakthrough | arXiv: 2305.18290 |
| 2023 | **Zephyr: Direct Distillation of LM Alignment** | Tunstall et al. (HuggingFace) | DPO + distillation = strong 7B model | arXiv: 2310.16944 |
| 2024 | **KTO: Model Alignment as Prospect Theoretic Optimization** | Ethayarajh et al. | Alignment from binary feedback only (no pairs needed) | arXiv: 2402.01306 |
| 2024 | **ORPO: Monolithic Preference Optimization** | Hong et al. | Combines SFT + DPO in single stage | arXiv: 2403.07691 |

> 🔑 **If you read only 3 papers**: InstructGPT (2022) for the full picture, DPO (2023) for the math breakthrough, Zephyr (2023) for practical open-source application.

---

# 🏭 PRODUCTION DEPLOYMENT CHEAT SHEET

> **If you're building an AI product, here's what you actually need to know:**

| Question | Answer |
|----------|--------|
| **Which method should I use?** | DPO unless you have a dedicated ML team + large compute budget |
| **How much preference data?** | Minimum 5K-10K pairs for DPO; 50K+ for reward model training |
| **What learning rate for DPO?** | Start at $5 \times 10^{-7}$, never exceed $5 \times 10^{-6}$ |
| **What β for DPO?** | 0.1 is the standard starting point |
| **How many epochs?** | 1 epoch usually. 2 max. NEVER more. |
| **Do I need a reference model?** | Yes — keep the SFT model frozen in memory |
| **Cheapest path to aligned model?** | SFT on ShareGPT data → DPO on UltraFeedback → ship it 🚀 |
| **GPU requirement (7B model)?** | DPO: 2×A100 (80GB). RLHF: 4×A100 minimum |
| **Can I use GPT-4 as annotator?** | Yes! This is RLAIF and works surprisingly well for open models |

---

# 🧩 THE ALIGNMENT FAMILY TREE (Beyond RLHF & DPO)

> **Alignment research is evolving fast. Here's the extended family:**

| Method | Year | Key Idea | Advantage |
|--------|------|----------|-----------|
| **RLHF** (PPO) | 2017/2022 | Train reward model → RL | Gold standard, well-studied |
| **DPO** | 2023 | Skip reward model, direct loss | Simpler, cheaper, stable |
| **IPO** (Identity PO) | 2023 | Fixes DPO's overfitting issues | More robust to noisy data |
| **KTO** | 2024 | Works with binary (good/bad) data | Doesn't need paired data! |
| **ORPO** | 2024 | Merges SFT + preference in one step | Single-phase training |
| **SPPO** | 2024 | Self-play preference optimization | No human data needed at all |
| **Constitutional AI** | 2022 | AI critiques its own outputs | Scalable oversight |
| **RLAIF** | 2023 | AI provides feedback (not humans) | Cheaper, faster, scales |

---

# ✅ FINAL BEGINNER RECAP 🎓

> **If you remember NOTHING else from this page, remember these 5 things:**
>
> 1. 🐕 **RLHF = training with an automated judge** — humans provide examples, a reward model learns to judge, then the LLM trains against that judge
>
> 2. 🛤️ **DPO = the shortcut** — skip the judge, learn directly from "this answer is better than that answer" pairs. Same math, fewer steps
>
> 3. 🔗 **KL Penalty = the leash** — without it, the model goes crazy and finds loopholes (reward hacking). The $\beta$ parameter controls how tight the leash is
>
> 4. 📊 **Bradley-Terry = Elo for text** — $P(A > B) = \sigma(r_A - r_B)$ converts reward differences into win probabilities
>
> 5. 🚀 **In practice: use DPO** — unless you're OpenAI or Anthropic with massive compute, DPO is the standard approach for aligning language models

**Tomorrow**: End-to-End Mini GPT — Complete project combining everything from Days 41-59.
