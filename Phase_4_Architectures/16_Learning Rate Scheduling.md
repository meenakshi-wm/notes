📘 DAY 16 — Learning Rate Scheduling: Warmup + Cosine Decay

> 🚗 **The Road Trip Analogy**: Imagine driving across the country. You wouldn't floor the gas from a dead stop (you'd crash!), cruise at 120 mph the whole way (dangerous!), or crawl at 5 mph (you'd never arrive). Instead, you **slowly accelerate**, **cruise at a comfortable speed**, then **gently brake** as you approach your destination. That's EXACTLY what a learning rate schedule does for AI training!

---

## 🎯 Goal:

Master learning rate scheduling for transformer training — WHY warmup prevents early divergence, HOW cosine decay works mathematically, and the specific schedule (linear warmup + cosine decay to 10% of peak) that has become the universal standard for LLM pre-training.

## ✅ If this is deeply understood:
You can configure training for any model size, debug training instabilities caused by learning rate issues, and understand why the learning rate schedule is one of the most important hyperparameters for LLM training.

---

> 🧠 **Zero-to-Hero Quick Context**: When an AI model "learns," it adjusts millions of tiny numbers (called **weights**) to get better at its task. The **learning rate** controls HOW MUCH those numbers change with each update. Too much change = chaos. Too little change = painfully slow. A **schedule** changes this rate over time — like a speed plan for a road trip!

---

# 1️⃣ Why Learning Rate Scheduling Matters

## Constant Learning Rate: The Problem

> 🚦 **Analogy — Cruise Control on a Mountain Road**: Using a constant learning rate is like setting cruise control on a winding mountain highway. It works on flat stretches, but you'll fly off a cliff on sharp turns and stall going uphill. You NEED to vary your speed!

A fixed learning rate faces a dilemma:
- **Too high**: training diverges (loss spikes, NaN) 💥
- **Too low**: training converges slowly, may get stuck in bad optima 🐌
- **Just right**: works early but becomes too aggressive later ⚠️

The loss landscape CHANGES during training:
- Early: loss surface is steep, gradients are large → need SMALL steps 🏔️
- Middle: found a good basin, gradients are moderate → can take BIGGER steps 🏃
- Late: fine-tuning within a minimum → need SMALL, precise steps 🎯

A SCHEDULE adapts the learning rate to the training phase.

### ⭐ The Five Main Schedule Families at a Glance

| Schedule | Metaphor | Formula | When to Use |
|----------|----------|---------|-------------|
| **Constant** | Cruise control | $\alpha_t = \alpha$ | Quick experiments only |
| **Step Decay** | Speed bumps every few miles | $\alpha_t = \alpha_0 \cdot \gamma^{\lfloor t/S \rfloor}$ | Classic CNNs (ResNet era) |
| **Exponential Decay** | Slowly letting off the gas | $\alpha_t = \alpha_0 \cdot e^{-\lambda t}$ | Smooth but decays too fast |
| **Linear Decay** | Steady gentle braking | $\alpha_t = \alpha_0 \cdot (1 - t/T)$ | Fine-tuning, BERT |
| **Cosine Decay** | Graceful highway exit ramp | $\alpha_t = \alpha_{min} + \frac{1}{2}(\alpha_{max} - \alpha_{min})(1 + \cos(\frac{\pi t}{T}))$ | ⭐ Modern LLM standard |
| **OneCycleLR** | Full road trip arc | Warmup → peak → anneal | Fast convergence (Smith 2017) |
| **WSD** | Warmup-cruise-brake | 3-phase piecewise | Continual pre-training (Hu et al. 2024) |

---

# 2️⃣ Warmup: Preventing Early Divergence

## Why Warmup?

> 🚗 **Analogy — Starting from a Dead Stop**: Imagine you're at a red light in a sports car with 700 horsepower. If you SLAM the gas pedal, the tires spin, you fishtail, and maybe crash. Instead, you gently ease onto the gas, let the tires grip, and THEN accelerate to full speed. Warmup does exactly this for AI training — it lets the model's internal "moment estimates" stabilize before cranking up the learning rate.

At initialization:
- Weights are random → activations have high variance 🎲
- Gradients are large and noisy 📡
- Adam's moment estimates (m, v) are initialized to zero → biased ⚠️

Taking large steps with noisy, biased gradients = training collapse. 💥

> ⭐ **Why Adam's Moments Need Time**: The Adam optimizer tracks a running average of gradients ($m$) and squared gradients ($v$). At step 1, these are nearly zero because they've only seen one batch. The bias-corrected estimates ($\hat{m}, \hat{v}$) are unreliable early on. Warmup gives Adam time to build accurate statistics before taking big steps.

## Linear Warmup

```
LR
 |      /\
 |     /  \____
 |    /         \____
 |   /                \____
 |  /                      \____
 | /                            \__
 |/________________________________\___
 |
 |--------|---------------------------|
  warmup        cosine decay
```

During warmup (first $W$ steps):

$$\alpha_t = \alpha_{max} \times \frac{t}{T_{warmup}}$$

Start from ~0, linearly increase to $\alpha_{max}$ over $W$ steps.

> 🔍 **What's happening step by step**: At step 1 with 2000 warmup steps, the LR is $\alpha_{max} \times \frac{1}{2000} = 0.0005 \times \alpha_{max}$ — basically zero. At step 1000, it's half of peak. At step 2000, you've reached full speed. The model has "warmed up its engine."

## How Many Warmup Steps?

Rule of thumb: 0.1-2% of total training steps.

| Total Steps | Warmup Steps | % of Training | Real-World Example |
|-------------|-------------|---------------|-------------------|
| 10,000 | 100-200 | 1-2% | Small fine-tune job |
| 100,000 | 1,000-2,000 | 1-2% | Medium model pre-training |
| 1,000,000 | 2,000-5,000 | 0.2-0.5% | Large LLM pre-training |

LLaMA-2: 2,000 warmup steps out of ~2M total
GPT-3: 375 warmup steps

> ⭐ **Production Insight**: Notice that as training gets longer, the warmup PERCENTAGE gets smaller. For very large runs, even 0.1% of steps is thousands of updates — plenty for Adam to stabilize.

---

# 3️⃣ Cosine Decay: The Standard Schedule

> 🚗 **Analogy — The Graceful Highway Exit**: You're cruising at 70 mph and your exit is 2 miles away. You don't SLAM the brakes (that's step decay). You don't let off the gas and coast (that's linear decay). Instead, you smoothly ease off — fast at first, then more and more gently until you're at the perfect exit speed. That's cosine decay! The mathematical shape of a cosine curve naturally gives this "fast then gentle" braking profile.

## Mathematics

After warmup, decay learning rate following a cosine curve:

$$\alpha_t = \alpha_{min} + \frac{1}{2}(\alpha_{max} - \alpha_{min})\left(1 + \cos\left(\pi \cdot \frac{t - W}{T - W}\right)\right)$$

Where:
- $t$ = current step
- $W$ = warmup steps
- $T$ = total steps
- $\alpha_{max}$ = peak learning rate
- $\alpha_{min}$ = minimum learning rate (usually $0.1 \times \alpha_{max}$)

> 🔍 **Breaking Down the Math for Beginners**:
> - When $t = W$ (just finished warmup): $\cos(0) = 1$, so $\alpha_t = \alpha_{min} + \frac{1}{2}(\alpha_{max} - \alpha_{min})(1+1) = \alpha_{max}$ ✅ Full speed!
> - When $t = T$ (end of training): $\cos(\pi) = -1$, so $\alpha_t = \alpha_{min} + \frac{1}{2}(\alpha_{max} - \alpha_{min})(1-1) = \alpha_{min}$ ✅ Minimum speed!
> - When $t$ is halfway: $\cos(\pi/2) = 0$, so $\alpha_t = \alpha_{min} + \frac{1}{2}(\alpha_{max} - \alpha_{min})$ — exactly in the middle!
> 
> The cosine function slides smoothly from 1 to -1, creating a beautiful S-shaped deceleration curve. 📉

## Why Cosine?

1. **Smooth**: no sudden changes that could destabilize training 🌊
2. **Slow start of decay**: stays near $\alpha_{max}$ longer after warmup ⏳
3. **Slow finish**: gentle approach to $\alpha_{min}$ prevents overshooting 🎯
4. **Empirically best**: outperforms linear decay, step decay, exponential decay 🏆

### ⭐ Head-to-Head Schedule Comparison

| Property | Step Decay | Linear Decay | Exponential Decay | Cosine Decay |
|----------|-----------|-------------|-------------------|-------------|
| Smoothness | ❌ Discontinuous | ✅ Smooth | ✅ Smooth | ✅ Very smooth |
| Stays near peak | ❌ Drops early | ❌ Drops immediately | ❌ Drops fast | ✅ Lingers |
| Final approach | ❌ Sudden | ✅ Gradual | ❌ Too slow | ✅ Gradual |
| Tuning effort | 🔧 Many choices | ✅ Simple | 🔧 λ sensitive | ✅ Simple |
| LLM standard? | ❌ Legacy | ⚠️ Fine-tune only | ❌ No | ⭐ **YES** |

> 📚 **Research Deep Dive — Loshchilov & Hutter (2017)**: The cosine annealing schedule was introduced in *"SGDR: Stochastic Gradient Descent with Warm Restarts"* (ICLR 2017). Their key insight was that cosine decay naturally provides a "warm restart" friendly shape — you can repeatedly cycle the cosine to escape local minima. While modern LLM training uses a single cosine cycle, the paper's schedule became the foundation of all modern LR scheduling. [arXiv:1608.03983]

## Implementation

```python
import math
import torch

class CosineScheduleWithWarmup:
    """Linear warmup + cosine decay learning rate schedule."""
    
    def __init__(self, optimizer, warmup_steps, total_steps, min_lr_ratio=0.1):
        self.optimizer = optimizer
        self.warmup_steps = warmup_steps
        self.total_steps = total_steps
        self.base_lr = optimizer.param_groups[0]['lr']
        self.min_lr = self.base_lr * min_lr_ratio
        self.current_step = 0
    
    def step(self):
        self.current_step += 1
        lr = self.get_lr()
        for param_group in self.optimizer.param_groups:
            param_group['lr'] = lr
    
    def get_lr(self):
        if self.current_step < self.warmup_steps:
            # Linear warmup
            return self.base_lr * (self.current_step / self.warmup_steps)
        
        # Cosine decay
        progress = (self.current_step - self.warmup_steps) / (self.total_steps - self.warmup_steps)
        progress = min(progress, 1.0)
        
        return self.min_lr + 0.5 * (self.base_lr - self.min_lr) * (1 + math.cos(math.pi * progress))
    
    def get_last_lr(self):
        return [self.get_lr()]

# Usage:
# optimizer = torch.optim.AdamW(model.parameters(), lr=3e-4, weight_decay=0.1)
# scheduler = CosineScheduleWithWarmup(optimizer, warmup_steps=2000, total_steps=100000)
```

## Using PyTorch Built-in

```python
from torch.optim.lr_scheduler import CosineAnnealingLR, LinearLR, SequentialLR

optimizer = torch.optim.AdamW(model.parameters(), lr=3e-4, weight_decay=0.1)

warmup_scheduler = LinearLR(optimizer, start_factor=1e-8, end_factor=1.0, total_iters=2000)
cosine_scheduler = CosineAnnealingLR(optimizer, T_max=98000, eta_min=3e-5)

scheduler = SequentialLR(optimizer, schedulers=[warmup_scheduler, cosine_scheduler], milestones=[2000])
```

---

## ⭐ Bonus: OneCycleLR — The "Super-Convergence" Schedule

> 📚 **Research Deep Dive — Leslie Smith (2017)**: In *"Super-Convergence: Very Fast Training of Neural Networks Using Large Learning Rates"*, Smith discovered that using a **single cycle** — warming up to a VERY high LR then annealing back down — could train models **5-10x faster** than conventional schedules. This became PyTorch's `OneCycleLR`. [arXiv:1708.07120]

The OneCycleLR schedule has three phases:

$$\text{Phase 1 (warmup):} \quad \alpha_t = \alpha_{min} \to \alpha_{max} \quad \text{over } \sim30\%\text{ of steps}$$
$$\text{Phase 2 (anneal):} \quad \alpha_t = \alpha_{max} \to \alpha_{min} \quad \text{over } \sim70\%\text{ of steps}$$

> 🚗 **Analogy**: OneCycleLR is like a drag race — you accelerate HARD to a very high speed for a short time, then decelerate for the rest of the race. It's aggressive but it WORKS for short training runs.

```python
# PyTorch OneCycleLR — great for fine-tuning and short training
scheduler = torch.optim.lr_scheduler.OneCycleLR(
    optimizer, max_lr=3e-4, total_steps=10000,
    pct_start=0.3,       # 30% of training is warmup
    anneal_strategy='cos' # cosine annealing in decay phase
)
```

## ⭐ Bonus: WSD — Warmup-Stable-Decay (The New Kid on the Block)

> 📚 **Research Deep Dive — Hu et al. (2024)**: The *"MiniCPM: Unveiling the Potential of Small Language Models with Scalable Training Strategies"* paper introduced the **WSD (Warmup-Stable-Decay)** schedule. Unlike cosine decay, WSD keeps the LR at its peak value for most of training, then rapidly decays at the end. This is particularly powerful for **continual pre-training** — you can stop "mid-cruise," save a checkpoint, and resume training with another decay phase later. [arXiv:2404.06395]

$$\alpha_t = \begin{cases} \alpha_{max} \cdot \frac{t}{T_w} & \text{if } t < T_w \quad \text{(warmup)} \\ \alpha_{max} & \text{if } T_w \le t < T_s \quad \text{(stable)} \\ \alpha_{max} \cdot \frac{T - t}{T - T_s} & \text{if } t \ge T_s \quad \text{(decay)} \end{cases}$$

> 🚗 **Analogy**: WSD is like a long highway drive — you accelerate to cruising speed, hold it for hundreds of miles, then only brake when you see your exit sign. This lets you "extend the trip" by just adding more highway miles before the exit.

### ⭐ When to Use Which Schedule?

| Scenario | Best Schedule | Why |
|----------|--------------|-----|
| Pre-training LLM from scratch | Warmup + Cosine | Battle-tested, GPT/LLaMA standard |
| Fine-tuning a model | Linear decay or OneCycleLR | Short training, fast convergence |
| Continual pre-training | WSD | Can pause and resume flexibly |
| Quick experiment | Constant with warmup | Simple, good enough to test ideas |
| Image classification (legacy) | Step decay | Proven for ResNets, VGGs |

---

# 4️⃣ Learning Rate Selection by Model Size

## The Scaling Relationship

> 🧠 **Beginner Intuition**: Think of it this way — a small boat (small model) can make sharp turns easily. A massive oil tanker (large model) needs to steer gently or it'll capsize. Bigger models need smaller learning rates for the same reason!

Larger models need SMALLER learning rates:

| Model Size | Peak LR | Source | Analogy |
|-----------|---------|--------|---------|
| 125M | 6e-4 | GPT-3 | 🚤 Speedboat — nimble, sharp turns OK |
| 350M | 3e-4 | GPT-3 | 🛥️ Yacht |
| 1.3B | 2e-4 | GPT-3 | 🚢 Cargo ship |
| 6.7B | 1.2e-4 | GPT-3 | 🚢 Large cargo ship |
| 13B | 1e-4 | LLaMA | 🛳️ Cruise liner |
| 70B | 1.5e-5 | LLaMA-2 | 🚢 Aircraft carrier |
| 175B | 6e-5 | GPT-3 | 🛢️ Supertanker — VERY gentle turns |

Approximate rule:

$$\alpha \propto \frac{1}{\sqrt{N_{params}}}$$

> 📚 **Research Deep Dive — Hoffmann et al. (2022) "Chinchilla"**: The DeepMind paper *"Training Compute-Optimal Large Language Models"* (NeurIPS 2022) established that for a given compute budget $C$, the optimal model size $N$ and number of training tokens $D$ should scale equally: $N \propto C^{0.5}$ and $D \propto C^{0.5}$. They also showed that previously trained models (like GPT-3) were significantly **undertrained** — they used too many parameters and too few tokens. The Chinchilla-optimal recipe pairs model size with both data AND learning rate choices. [arXiv:2203.15556]

## Why Smaller LR for Bigger Models?

Bigger models have more parameters contributing to each gradient update.
The effective step size is: $\text{effective step} = \alpha \times \|\nabla L\|$.
With more parameters, gradient updates are more "influential" → need smaller step.

> 💡 **Formal Intuition**: If a model has $N$ parameters, a single gradient update moves the parameter vector by $\alpha \cdot g$ where $g \in \mathbb{R}^N$. The L2 norm of this update scales as $\alpha \cdot \|g\| \propto \alpha \cdot \sqrt{N}$. To keep the update magnitude constant across model sizes, we need $\alpha \propto 1/\sqrt{N}$.

---

# 5️⃣ The Full Training Configuration

> ⭐ **Production Reality Check**: The code below is essentially the recipe used to train models like LLaMA, GPT, and Mistral. The specific numbers (betas, weight decay, warmup ratio) have been validated across hundreds of training runs at companies like Meta, OpenAI, and Google. When in doubt, use THESE defaults — they're battle-tested.

```python
def configure_training(model, total_tokens, batch_size, seq_length):
    """Configure optimizer and scheduler for LLM training."""
    
    num_params = sum(p.numel() for p in model.parameters())
    
    # Select learning rate based on model size
    if num_params < 500e6:
        peak_lr = 3e-4
    elif num_params < 2e9:
        peak_lr = 2e-4
    elif num_params < 10e9:
        peak_lr = 1e-4
    else:
        peak_lr = 3e-5
    
    # Calculate steps
    tokens_per_step = batch_size * seq_length
    total_steps = total_tokens // tokens_per_step
    warmup_steps = min(2000, int(0.01 * total_steps))
    
    print(f"Model: {num_params/1e6:.0f}M params")
    print(f"Peak LR: {peak_lr}")
    print(f"Total steps: {total_steps:,}")
    print(f"Warmup steps: {warmup_steps:,}")
    
    # AdamW optimizer
    optimizer = torch.optim.AdamW(
        model.parameters(),
        lr=peak_lr,
        betas=(0.9, 0.95),      # LLaMA-style betas
        weight_decay=0.1,        # Standard for pre-training
        eps=1e-8,
    )
    
    # Cosine schedule with warmup
    scheduler = CosineScheduleWithWarmup(
        optimizer,
        warmup_steps=warmup_steps,
        total_steps=total_steps,
        min_lr_ratio=0.1  # Decay to 10% of peak
    )
    
    return optimizer, scheduler
```

---

# 6️⃣ Weight Decay (AdamW)

## Why Weight Decay?

> 🚗 **Analogy — Friction on the Road**: Without friction, a car would accelerate indefinitely and never stop. Weight decay is like friction for model weights — it constantly pulls them back toward zero, preventing any single weight from growing too powerful. This keeps the model "humble" and generalizable.

Weight decay adds L2 penalty:

$$\mathcal{L}_{total} = \mathcal{L} + \frac{\lambda}{2} \|\theta\|^2$$

This:
1. Prevents weights from growing too large 📏
2. Provides regularization 🛡️
3. Improves generalization 🌐

## AdamW vs Adam + L2

Adam with L2 regularization:

$$g \leftarrow g + \lambda \cdot \theta \quad \text{(then apply Adam)}$$

AdamW (decoupled):

$$\theta \leftarrow \theta - \alpha \cdot \lambda \cdot \theta \quad \text{(AFTER Adam update)}$$

AdamW is mathematically different and empirically better for transformers.

> ⭐ **Why the Difference Matters**: In standard Adam, the L2 gradient gets divided by $\sqrt{v}$ (the running variance of gradients), which means the regularization strength is ADAPTIVE — weights with large gradients get less regularization, which is not what we want. AdamW applies weight decay DIRECTLY to the weights, giving uniform regularization regardless of gradient history. This was the key insight of Loshchilov & Hutter's AdamW paper (2019). [arXiv:1711.05101]

## What to Decay

```python
# DO decay: dense weights (Linear layers)
# DON'T decay: biases, LayerNorm parameters, embeddings

def get_parameter_groups(model, weight_decay=0.1):
    """Separate parameters into decay and no-decay groups."""
    decay_params = []
    no_decay_params = []
    
    for name, param in model.named_parameters():
        if not param.requires_grad:
            continue
        
        # Don't decay biases, LayerNorm, embeddings
        if param.ndim <= 1 or 'bias' in name or 'ln' in name or 'norm' in name or 'embedding' in name:
            no_decay_params.append(param)
        else:
            decay_params.append(param)
    
    param_groups = [
        {'params': decay_params, 'weight_decay': weight_decay},
        {'params': no_decay_params, 'weight_decay': 0.0},
    ]
    
    num_decay = sum(p.numel() for p in decay_params)
    num_no_decay = sum(p.numel() for p in no_decay_params)
    print(f"Decay params: {num_decay:,}, No-decay params: {num_no_decay:,}")
    
    return param_groups
```

---

# 7️⃣ Gradient Clipping

## Why Clip Gradients?

> 🚗 **Analogy — Speed Limiter**: Some sports cars have electronic speed limiters that cap the top speed at, say, 155 mph. Even if the engine COULD go faster, it's dangerous. Gradient clipping works the same way — if gradients are trying to make a HUGE update (like a loss spike caused by a bad data batch), the clipper caps it to a safe maximum.

During training, occasionally a batch produces extremely large gradients.
Without clipping, this causes:
- Huge parameter updates 💥
- Loss spike 📈
- Potentially unrecoverable divergence 🔥

## Max Gradient Norm Clipping

Scale gradients so their total L2 norm $\le$ max_norm:

$$\text{If } \|g\| > \text{max\_norm}: \quad g \leftarrow g \cdot \frac{\text{max\_norm}}{\|g\|}$$

```python
# Before optimizer.step():
grad_norm = torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)

# If grad_norm > 1.0: all gradients scaled by (1.0 / grad_norm)
# If grad_norm ≤ 1.0: no change
```

> ⭐ **The max_norm=1.0 Standard**: Nearly every modern LLM training run uses `max_norm=1.0`. This value was established empirically and appears in the training configs of GPT-3, LLaMA, Mistral, and most open-source models. It provides a good safety net without being so aggressive that it slows convergence.

## Monitoring Gradient Norms

```python
def log_gradient_stats(model):
    """Log gradient statistics for debugging."""
    total_norm = 0.0
    max_grad = 0.0
    
    for name, param in model.named_parameters():
        if param.grad is not None:
            param_norm = param.grad.data.norm(2).item()
            total_norm += param_norm ** 2
            max_grad = max(max_grad, param.grad.data.abs().max().item())
    
    total_norm = total_norm ** 0.5
    return {'grad_norm': total_norm, 'max_grad': max_grad}
```

---

# 8️⃣ Visualizing the Schedule

```python
import matplotlib.pyplot as plt

def visualize_schedule(total_steps=100000, warmup_steps=2000, peak_lr=3e-4, min_lr_ratio=0.1):
    """Visualize the learning rate schedule."""
    steps = range(total_steps)
    lrs = []
    
    for t in steps:
        if t < warmup_steps:
            lr = peak_lr * (t / warmup_steps)
        else:
            progress = (t - warmup_steps) / (total_steps - warmup_steps)
            lr = peak_lr * min_lr_ratio + 0.5 * peak_lr * (1 - min_lr_ratio) * (1 + math.cos(math.pi * progress))
        lrs.append(lr)
    
    plt.figure(figsize=(12, 5))
    plt.plot(steps, lrs, linewidth=2)
    plt.axvline(x=warmup_steps, color='r', linestyle='--', label=f'End warmup ({warmup_steps})')
    plt.xlabel('Training Step')
    plt.ylabel('Learning Rate')
    plt.title('Linear Warmup + Cosine Decay Schedule')
    plt.legend()
    plt.grid(True, alpha=0.3)
    plt.tight_layout()
    plt.show()

# visualize_schedule()
```

---

# 9️⃣ Common Pitfalls

> ⚠️ **These pitfalls aren't theoretical — they've cost companies thousands of GPU-hours**. Learning to recognize and fix them quickly is a superpower for ML engineers.

## 1. LR Too High → Loss Spikes 💥
If you see sudden loss spikes mid-training:
- Reduce peak LR by 2-3×
- Increase warmup steps
- Reduce batch size temporarily

> 🚗 **Analogy**: Your car hit a pothole because you were going too fast. Slow down!

## 2. LR Too Low → Slow/Stalled Training 🐌
If loss plateaus early:
- Increase peak LR
- Check for dead gradients (activation issues)

> 🚗 **Analogy**: You're driving 10 mph on the highway. You'll never get there!

## 3. Forgetting to Step the Scheduler 😱
```python
# WRONG: scheduler never called
for batch in dataloader:
    loss = train_step(batch)
    optimizer.step()  # Missing: scheduler.step()

# CORRECT:
for batch in dataloader:
    loss = train_step(batch)
    optimizer.step()
    scheduler.step()  # MUST be called every step
```

> ⭐ **This is the #1 beginner mistake!** Without `scheduler.step()`, your learning rate stays constant for the entire run. The schedule only exists in your head — the code never changes the LR.

## 4. Wrong Warmup Duration ⏱️
Too short: still diverges. Too long: wastes training budget at low LR.

## 5. Not Matching Schedule to Total Steps ❌

> ⭐ **Sneaky Bug**: If you set `total_steps=100000` in your scheduler but your training actually runs for 150,000 steps, the cosine will "bottom out" at step 100K and stay at $\alpha_{min}$ for the remaining 50K steps. Always make sure `total_steps` matches your actual training length!

---

# 📝 Summary

| Concept | Key Detail |
|---------|-----------|
| Warmup | Linear increase from ~0 to $\alpha_{max}$ over 0.1-2% of training |
| Cosine decay | Smooth decay to $\alpha_{min}$ after warmup |
| Min LR | Usually 10% of peak LR ($\alpha_{min} = 0.1 \cdot \alpha_{max}$) |
| Peak LR | ~3e-4 for <500M params, ~1e-4 for 1-10B, ~3e-5 for >10B |
| Weight decay | 0.1, applied only to weight matrices (not biases/norms) |
| Gradient clipping | max_norm=1.0, prevents training divergence |
| Betas | (0.9, 0.95) for LLM training (not the default 0.9, 0.999) |

---

# 🔬 Deep Research: Key Papers & Citations

| Paper | Authors | Year | Key Contribution | Impact |
|-------|---------|------|-----------------|--------|
| *SGDR: Stochastic Gradient Descent with Warm Restarts* | Loshchilov & Hutter | 2017 | Introduced **cosine annealing** with warm restarts | Foundation of all modern LR scheduling [arXiv:1608.03983] |
| *Super-Convergence* | Leslie Smith | 2017 | Discovered **1cycle policy** for 5-10x faster training | Became PyTorch's `OneCycleLR` [arXiv:1708.07120] |
| *Decoupled Weight Decay Regularization* | Loshchilov & Hutter | 2019 | Proved **AdamW** is superior to Adam+L2 for transformers | AdamW is now the universal optimizer [arXiv:1711.05101] |
| *Training Compute-Optimal LLMs (Chinchilla)* | Hoffmann et al. | 2022 | Established **compute-optimal** scaling of model vs data | Changed how industry sizes models [arXiv:2203.15556] |
| *MiniCPM (WSD Schedule)* | Hu et al. | 2024 | Introduced **Warmup-Stable-Decay** for continual training | Enables flexible checkpoint-resume [arXiv:2404.06395] |

---

# 🏭 Production Training Recipes

### ⭐ GPT-3 / LLaMA-style Pre-training (The Gold Standard)
```
Optimizer:     AdamW
Betas:         (0.9, 0.95)
Weight Decay:  0.1
Peak LR:       Scales with model size (see table above)
Schedule:      Linear warmup → Cosine decay to 10% of peak
Warmup Steps:  ~2000 (or 0.1-1% of total)
Grad Clip:     max_norm = 1.0
```

### ⭐ Chinchilla-Optimal Schedule
```
Key insight:   Train for ~20 tokens per parameter
               (70B model → ~1.4T tokens)
Schedule:      Same warmup + cosine, but LONGER training
LR tuned to:   Match the compute-optimal token count
```

### ⭐ WSD for Continual Pre-training
```
Phase 1:       Warmup to peak LR (same as cosine)
Phase 2:       HOLD at peak LR for bulk of training
Phase 3:       Rapid decay in final 10-20% of steps
Advantage:     Save "Phase 2" checkpoint, resume later with new data
```

---

# 📋 Cheat Sheet: Learning Rate Scheduling

```
┌─────────────────────────────────────────────────────────┐
│           ⚡ LR SCHEDULING CHEAT SHEET ⚡               │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  🎯 THE UNIVERSAL RECIPE (just use this):               │
│     Warmup + Cosine Decay + AdamW                       │
│                                                         │
│  📐 KEY FORMULAS:                                       │
│     Warmup:  α_t = α_max · (t / T_warmup)              │
│     Cosine:  α_t = α_min + ½(α_max-α_min)(1+cos(πt/T))│
│     Rule:    α ∝ 1/√(N_params)                          │
│                                                         │
│  🔧 DEFAULT HYPERPARAMETERS:                            │
│     Betas:        (0.9, 0.95)                           │
│     Weight decay: 0.1                                   │
│     Grad clip:    1.0                                   │
│     Min LR:       10% of peak                           │
│     Warmup:       ~2000 steps                           │
│                                                         │
│  🚨 DEBUGGING CHECKLIST:                                │
│     □ Loss spiking?     → Lower peak LR                 │
│     □ Loss stuck?       → Raise peak LR                 │
│     □ NaN loss?         → Check grad clip + warmup      │
│     □ LR not changing?  → Did you call scheduler.step()? │
│     □ Decay too early?  → Check total_steps matches run │
│                                                         │
│  📏 PEAK LR BY MODEL SIZE:                              │
│     <500M:  3e-4    1-10B: 1e-4     >10B: 3e-5         │
│                                                         │
│  🗺️ SCHEDULE SELECTION:                                 │
│     Pre-training:       Warmup + Cosine (standard)      │
│     Fine-tuning:        Linear decay or OneCycleLR      │
│     Continual training: WSD (Warmup-Stable-Decay)       │
│     Quick experiment:   Constant + warmup               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

**Tomorrow**: The Complete Training Loop — putting optimizer, scheduler, data pipeline, logging, and checkpointing together into a production training script.
