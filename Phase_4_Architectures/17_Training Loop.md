📘 DAY 17 — Complete Training Loop: From Init to Checkpoint

Goal:

Build a production-quality training loop for GPT pre-training — initialization, forward/backward pass, gradient accumulation, logging, checkpointing, and resumption. This is the engineering that makes training actually work.

If this is deeply understood:
You can train any LLM from scratch, debug training runs, resume from failures, and understand every component of the training infrastructure that companies like OpenAI and Meta use.

---

## 🌟 Welcome, Absolute Beginner! Start Here

> **What even IS a training loop?**
>
> Imagine you're learning to cook a new dish 🍳. Every attempt, you:
> 1. **Cook the dish** (forward pass)
> 2. **Taste it** (compute the loss — how bad is it?)
> 3. **Figure out what went wrong** (backward pass — too much salt? undercooked?)
> 4. **Adjust your recipe** (optimizer step — use less salt next time)
> 5. **Clear your notes** from this round so you start fresh (zero_grad)
>
> You repeat this hundreds of thousands of times. **That's a training loop.** The neural network is the recipe, and training is the process of making the recipe better, one iteration at a time.

### 🏋️ The Workout Analogy

Think of training a model like a **workout routine**:

| Workout Concept | Training Concept | What It Means |
|---|---|---|
| 🏋️ One rep | One forward + backward pass | Process one batch of data |
| 🔄 One set (e.g., 10 reps) | One optimizer step (possibly with gradient accumulation) | Update the model's weights once |
| 📅 One full workout | One epoch | Go through all your training data once |
| 📊 Checking your progress | Validation | Test on data you *didn't* train on |
| 💾 Taking a progress photo | Checkpointing | Save the model so you can resume later |
| 📈 Tracking your fitness app | Logging (W&B, TensorBoard) | Record metrics to spot trends |

### 🔄 The Training Loop at a Glance

```
┌─────────────────────────────────────────────────────────┐
│                    TRAINING LOOP                         │
│                                                         │
│  for each epoch:                                        │
│    for each batch of data:                              │
│                                                         │
│      1. 🔮 FORWARD PASS     → model(input) = prediction│
│      2. 📏 COMPUTE LOSS     → how wrong are we?        │
│      3. 🔙 BACKWARD PASS    → loss.backward()          │
│      4. ✂️ CLIP GRADIENTS   → prevent explosions        │
│      5. 📐 OPTIMIZER STEP   → optimizer.step()          │
│      6. 🧹 ZERO GRADIENTS  → optimizer.zero_grad()     │
│                                                         │
│    📊 Validate   💾 Checkpoint   📈 Log metrics         │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 📚 What You'll Learn Today

| # | Topic | Beginner Analogy |
|---|---|---|
| 1️⃣ | Training Loop Architecture | The full workout plan |
| 2️⃣ | Core Training Step | One rep: cook → taste → adjust |
| 3️⃣ | Gradient Accumulation | Doing push-ups in sets |
| 4️⃣ | Validation Loop | Weekly weigh-in |
| 5️⃣ | Checkpointing & Resumption | Save game / load game |
| 6️⃣ | Logging & Monitoring | Your fitness tracker |
| 7️⃣ | Main Training Function | Putting it all together |
| 8️⃣ | Mixed Precision Training | Rough drafts vs. final copy |
| 9️⃣ | Training Diagnostics | Reading your blood work |

---

# 1️⃣ The Training Loop Architecture

> 🎯 **Beginner Analogy**: This section sets up the **entire gym** before you start working out — choosing your equipment (optimizer), planning your warm-up schedule (learning rate scheduler), and deciding where to save your progress photos (checkpoint directory).

### ⭐ Key Concepts Before the Code

| Term | Plain English | In Code |
|---|---|---|
| **Model** | The neural network (our "recipe") | `self.model` |
| **DataLoader** | Feeds data in bite-sized batches | `self.train_dataloader` |
| **Optimizer** | The algorithm that adjusts model weights | `self.optimizer` (AdamW) |
| **Scheduler** | Controls how fast the optimizer adjusts | `self.scheduler` (cosine + warmup) |
| **Device** | CPU or GPU | `self.device = 'cuda'` |
| **Gradient Accumulation** | Fake a big batch with small GPU | `self.grad_accum_steps` |

### 🔬 Why AdamW and Not Plain SGD?

AdamW = Adam optimizer + decoupled weight decay. It maintains **per-parameter learning rates** and **momentum**, making it the go-to for transformer training. The weight decay term acts as regularization — it gently pushes weights toward zero to prevent overfitting.

> ⭐ **Production Note**: The parameter group trick below is critical. We **don't** apply weight decay to biases or layer norm parameters because they have different optimization dynamics. Every serious training codebase (GPT-2, GPT-3, LLaMA) does this.

### 📐 The Cosine Learning Rate Schedule (LaTeX)

The learning rate at step $t$ follows:

$$\eta(t) = \begin{cases} \eta_{\max} \cdot \frac{t}{T_{\text{warmup}}} & \text{if } t < T_{\text{warmup}} \quad \text{(linear warmup)} \\ \eta_{\min} + \frac{1}{2}(\eta_{\max} - \eta_{\min})\left(1 + \cos\left(\pi \cdot \frac{t - T_{\text{warmup}}}{T_{\text{total}} - T_{\text{warmup}}}\right)\right) & \text{otherwise} \end{cases}$$

Where:
- $\eta_{\max}$ = peak learning rate (e.g., $3 \times 10^{-4}$)
- $\eta_{\min}$ = minimum learning rate (typically $0.1 \times \eta_{\max}$)
- $T_{\text{warmup}}$ = warmup steps (e.g., 2000)
- $T_{\text{total}}$ = total training steps

> 🍳 **Cooking Analogy**: Warmup = slowly preheating the oven. You don't blast it to 500°F immediately — you'd burn everything. Cosine decay = gradually turning down the heat as the dish finishes, so you don't overshoot.

```python
import torch
import torch.nn.functional as F
import math
import time
import os
import json
from pathlib import Path

class Trainer:
    def __init__(self, model, train_dataloader, val_dataloader, config):
        self.model = model
        self.train_dataloader = train_dataloader
        self.val_dataloader = val_dataloader
        self.config = config
        self.device = config.get('device', 'cuda')
        
        # Move model to device
        self.model = self.model.to(self.device)
        
        # Setup optimizer with parameter groups
        self.optimizer = self._create_optimizer()
        
        # Setup scheduler
        self.scheduler = self._create_scheduler()
        
        # Training state
        self.global_step = 0
        self.epoch = 0
        self.best_val_loss = float('inf')
        self.train_losses = []
        
        # Gradient accumulation
        self.grad_accum_steps = config.get('gradient_accumulation_steps', 1)
        
        # Checkpointing
        self.checkpoint_dir = Path(config.get('checkpoint_dir', './checkpoints'))
        self.checkpoint_dir.mkdir(parents=True, exist_ok=True)
    
    def _create_optimizer(self):
        """Create AdamW optimizer with proper parameter groups."""
        decay_params = []
        no_decay_params = []
        
        for name, param in self.model.named_parameters():
            if not param.requires_grad:
                continue
            if param.ndim <= 1 or 'bias' in name or 'norm' in name or 'ln' in name:
                no_decay_params.append(param)
            else:
                decay_params.append(param)
        
        param_groups = [
            {'params': decay_params, 'weight_decay': self.config.get('weight_decay', 0.1)},
            {'params': no_decay_params, 'weight_decay': 0.0},
        ]
        
        return torch.optim.AdamW(
            param_groups,
            lr=self.config['learning_rate'],
            betas=self.config.get('betas', (0.9, 0.95)),
            eps=1e-8,
        )
    
    def _create_scheduler(self):
        """Create cosine schedule with linear warmup."""
        warmup_steps = self.config.get('warmup_steps', 2000)
        total_steps = self.config['total_steps']
        min_lr_ratio = self.config.get('min_lr_ratio', 0.1)
        
        def lr_lambda(step):
            if step < warmup_steps:
                return step / warmup_steps
            progress = (step - warmup_steps) / (total_steps - warmup_steps)
            progress = min(progress, 1.0)
            return min_lr_ratio + 0.5 * (1.0 - min_lr_ratio) * (1 + math.cos(math.pi * progress))
        
        return torch.optim.lr_scheduler.LambdaLR(self.optimizer, lr_lambda)
```

---

# 2️⃣ The Core Training Step

> 🎯 **Beginner Analogy — One Rep in the Gym**:
>
> This is the **heart** of training — one single repetition. Here's what happens in plain English:
>
> | Step | Analogy | Code | What Happens |
> |---|---|---|---|
> | 1. 🔮 Forward Pass | 🍳 Cook the dish | `model(input_ids, labels)` | Data flows through the network, producing predictions |
> | 2. 📏 Compute Loss | 👅 Taste it — how bad? | `loss = ...` | Compare predictions to correct answers. Lower = better |
> | 3. ➗ Scale Loss | Divide effort across sets | `loss / grad_accum_steps` | If accumulating gradients, each mini-batch contributes a fraction |
> | 4. 🔙 Backward Pass | 🤔 Figure out what went wrong | `loss.backward()` | PyTorch walks backward through every operation, computing gradients |
> | 5. ✂️ Clip Gradients | Don't overreact! | `clip_grad_norm_()` | Cap the magnitude of gradients so training stays stable |
> | 6. 📐 Optimizer Step | ✏️ Adjust the recipe | `optimizer.step()` | Actually change the model weights based on gradients |
> | 7. 🧹 Zero Gradients | 📝 Clear your notes | `optimizer.zero_grad()` | Reset gradients to zero so they don't pile up from last step |
>
> **Why zero_grad?** PyTorch **accumulates** gradients by default. If you don't clear them, step 2's gradients add on top of step 1's — like writing over your notes without erasing first. 🧹

### 📐 The Math Behind Each Step

**Forward pass** computes predictions:

$$\hat{y} = f_\theta(x)$$

**Loss** measures error (cross-entropy for language models):

$$\mathcal{L} = -\frac{1}{N}\sum_{i=1}^{N} \log P_\theta(y_i \mid x_i)$$

**Backward pass** computes gradients via chain rule:

$$\frac{\partial \mathcal{L}}{\partial \theta} = \nabla_\theta \mathcal{L}$$

**Gradient clipping** caps the gradient norm:

$$\text{if } \|\nabla_\theta \mathcal{L}\|_2 > c, \quad \nabla_\theta \mathcal{L} \leftarrow c \cdot \frac{\nabla_\theta \mathcal{L}}{\|\nabla_\theta \mathcal{L}\|_2}$$

**Optimizer step** (simplified AdamW):

$$\theta_{t+1} = \theta_t - \eta \cdot \left(\frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon} + \lambda \theta_t \right)$$

Where $\hat{m}_t$ and $\hat{v}_t$ are bias-corrected first and second moment estimates, $\eta$ is the learning rate, and $\lambda$ is the weight decay.

> ⭐ **set_to_none=True**: Instead of filling gradients with zeros, this sets them to `None`. Saves memory because PyTorch doesn't allocate a zero tensor. Every production codebase uses this.

```python
    def train_step(self, batch):
        """Single training step with gradient accumulation."""
        self.model.train()
        
        input_ids = batch['input_ids'].to(self.device)
        labels = batch['labels'].to(self.device)
        
        # Forward pass
        logits, loss = self.model(input_ids, labels)
        
        # Scale loss for gradient accumulation
        loss = loss / self.grad_accum_steps
        
        # Backward pass
        loss.backward()
        
        return loss.item() * self.grad_accum_steps  # Return unscaled loss
    
    def optimizer_step(self):
        """Perform optimizer step after gradient accumulation."""
        # Gradient clipping
        grad_norm = torch.nn.utils.clip_grad_norm_(
            self.model.parameters(), 
            max_norm=self.config.get('max_grad_norm', 1.0)
        )
        
        # Optimizer step
        self.optimizer.step()
        self.scheduler.step()
        self.optimizer.zero_grad(set_to_none=True)  # More memory efficient
        
        self.global_step += 1
        
        return grad_norm.item()
```

---

# 3️⃣ Gradient Accumulation: Simulating Large Batches

> 🎯 **Beginner Analogy — Push-ups in Sets** 🏋️
>
> Say your trainer wants you to do **64 push-ups** in a row, but you can only do **4** before your arms give out.
>
> **Solution**: Do 4 push-ups × 16 sets = 64 total. You rest (don't update weights) between the mini-sets, and only count it as "one real set" after all 16 mini-sets.
>
> That's **gradient accumulation**! Your GPU can only fit 4 samples at a time, but you want the training effect of 64 samples. So you:
> 1. Process 4 samples → compute gradients → **DON'T** update weights yet
> 2. Process 4 more → gradients **add** to the previous ones
> 3. Repeat 16 times
> 4. NOW update weights (optimizer.step)
>
> The model "sees" all 64 samples worth of gradient information before making a single weight update. Mathematically identical to using batch_size=64! 🎯

## Why Gradient Accumulation?

Large batch sizes improve training stability and speed.
But GPU memory limits actual batch size.

Solution: accumulate gradients over multiple mini-batches before updating.

### 📐 The Math (LaTeX)

$$\text{Effective Batch Size} = \underbrace{B_{\text{micro}}}_{\text{micro-batch}} \times \underbrace{K}_{\text{accum steps}} \times \underbrace{N_{\text{GPU}}}_{\text{num GPUs}}$$

The accumulated gradient over $K$ steps approximates the true gradient:

$$\nabla_\theta \mathcal{L}_{\text{accum}} = \frac{1}{K} \sum_{k=1}^{K} \nabla_\theta \mathcal{L}_k \approx \nabla_\theta \mathcal{L}_{\text{large batch}}$$

> ⭐ **Why does this work?** Because `loss.backward()` **adds** to existing gradients by default. So if you call `.backward()` 4 times before `.step()`, the gradients accumulate. We divide the loss by $K$ (`loss / grad_accum_steps`) so the total is correctly averaged.

Effective batch size = micro_batch_size × gradient_accumulation_steps × num_GPUs

Example:
- GPU can fit batch_size=4, seq_len=2048
- We want effective batch_size=64
- Use gradient_accumulation_steps = 64/4 = 16
- Process 16 micro-batches, accumulate gradients, then update once

> 🏭 **Production Scenario**: Training LLaMA-7B requires effective batch sizes of ~4M tokens. With a single A100 (80GB), you can fit ~batch_size=2 at seq_len=2048 (≈4K tokens). To reach 4M tokens:
>
> $$K = \frac{4{,}000{,}000}{2 \times 2048} = 976 \text{ accumulation steps (single GPU)}$$
>
> Or with 128 GPUs: $K = \frac{4{,}000{,}000}{2 \times 2048 \times 128} = 7.6 ≈ 8$ steps. Much more practical! 🚀

### 📊 Gradient Accumulation Cheat Sheet

| Scenario | Micro Batch | Accum Steps | GPUs | Effective Batch |
|---|---|---|---|---|
| Student laptop (RTX 3060) | 2 | 32 | 1 | 64 |
| Single A100 | 16 | 4 | 1 | 64 |
| 8× A100 cluster | 16 | 1 | 8 | 128 |
| Meta LLaMA training | 4 | 8 | 512 | 16,384 |

### 🔬 Deep Research: Large-Batch Training

> **Goyal et al. (2017)** — *"Accurate, Large Minibatch SGD: Training ImageNet in 1 Hour"* (Facebook AI Research)
>
> Key finding: You *can* scale batch size linearly if you also **scale the learning rate linearly** (the "linear scaling rule"):
>
> $$\eta_{\text{new}} = \eta_{\text{base}} \times \frac{B_{\text{new}}}{B_{\text{base}}}$$
>
> BUT you need a **gradual warmup** period (5 epochs in their case) to avoid early training instability. This paper is why every modern training loop has a warmup phase.
>
> 📖 *Citation*: Goyal, P., Dollár, P., Girshick, R., et al. (2017). "Accurate, Large Minibatch SGD: Training ImageNet in 1 Hour." *arXiv:1706.02677*.

```python
    def train_epoch(self):
        """Train for one epoch with gradient accumulation."""
        data_iter = iter(self.train_dataloader)
        accumulated_loss = 0.0
        micro_step = 0
        
        for batch in data_iter:
            loss = self.train_step(batch)
            accumulated_loss += loss
            micro_step += 1
            
            # Update weights after accumulating enough gradients
            if micro_step % self.grad_accum_steps == 0:
                grad_norm = self.optimizer_step()
                
                avg_loss = accumulated_loss / self.grad_accum_steps
                
                # Logging
                if self.global_step % self.config.get('log_interval', 10) == 0:
                    self._log_metrics(avg_loss, grad_norm)
                
                # Validation
                if self.global_step % self.config.get('eval_interval', 500) == 0:
                    self._evaluate()
                
                # Checkpointing
                if self.global_step % self.config.get('save_interval', 1000) == 0:
                    self.save_checkpoint()
                
                accumulated_loss = 0.0
                
                if self.global_step >= self.config['total_steps']:
                    break
```

> 💡 **Step vs. Epoch — What's the Difference?**
>
> | Term | Definition | Analogy |
> |---|---|---|
> | **Micro-step** | Process one mini-batch (forward + backward) | One push-up |
> | **Step** (global_step) | One optimizer update (may include multiple micro-steps if accumulating) | One complete set |
> | **Epoch** | One full pass through the entire dataset | One complete workout (all exercises) |
>
> In LLM pre-training, we usually think in **steps**, not epochs. GPT-3 was trained for ~300B tokens — they didn't even complete 1 epoch on their data!

---

# 4️⃣ Validation Loop

> 🎯 **Beginner Analogy — The Weekly Weigh-In** ⚖️
>
> You've been hitting the gym (training) every day. But are you *actually* getting stronger, or just memorizing the exercises?
>
> **Validation** = stepping on the scale / testing your strength on exercises you *haven't* practiced. If your gym performance improves but your "weigh-in" scores get worse, you're **overfitting** (memorizing the training data without truly learning).
>
> We run validation periodically (not every step — that would be slow!) to check if the model is genuinely learning.

### 📐 Key Metrics Explained

**Validation Loss** — Lower is better. Same loss function as training, but measured on held-out data:

$$\mathcal{L}_{\text{val}} = -\frac{1}{N_{\text{val}}} \sum_{i=1}^{N_{\text{val}}} \log P_\theta(y_i \mid x_i)$$

**Perplexity (PPL)** — "How surprised is the model?" Exponential of the loss:

$$\text{PPL} = e^{\mathcal{L}_{\text{val}}}$$

| PPL Value | Meaning |
|---|---|
| ~1 | Perfect (model always knows the next token) |
| ~10-30 | Excellent (GPT-3 territory) |
| ~100 | Decent (early training) |
| ~1000+ | Poor (random-ish guessing) |

> ⭐ **Why `@torch.no_grad()`?** During validation, we don't need gradients (we're not updating weights). This decorator tells PyTorch to skip gradient computation, saving **~50% memory** and running faster. Always use it for eval!

```python
    @torch.no_grad()
    def _evaluate(self):
        """Run validation and log metrics."""
        self.model.eval()
        total_loss = 0.0
        total_tokens = 0
        
        for batch in self.val_dataloader:
            input_ids = batch['input_ids'].to(self.device)
            labels = batch['labels'].to(self.device)
            
            logits, loss = self.model(input_ids, labels)
            
            valid_tokens = (labels != -100).sum().item()
            total_loss += loss.item() * valid_tokens
            total_tokens += valid_tokens
        
        val_loss = total_loss / total_tokens
        val_ppl = math.exp(val_loss)
        
        print(f"  [Eval] Step {self.global_step}: val_loss={val_loss:.4f}, val_ppl={val_ppl:.2f}")
        
        # Track best model
        if val_loss < self.best_val_loss:
            self.best_val_loss = val_loss
            self.save_checkpoint(tag='best')
            print(f"  ★ New best model! val_loss={val_loss:.4f}")
        
        self.model.train()
        return val_loss
```

---

# 5️⃣ Checkpointing & Resumption

> 🎯 **Beginner Analogy — Save Game / Load Game** 💾🎮
>
> Imagine you're playing a 100-hour RPG and your power goes out at hour 87. Without a save file, you start from scratch. **Devastating.** 😱
>
> **Checkpointing** = saving your game. You save:
> - 🧠 The model weights (your character's stats)
> - 📐 The optimizer state (your inventory and buffs)
> - 📅 The scheduler state (what quest you're on)
> - 📊 The training step number (progress counter)
> - 🏆 The best validation loss (your high score)
>
> **Resumption** = loading your save file and continuing from exactly where you left off.

### 🏭 Production Scenario: Why Checkpointing is Non-Negotiable

> Training GPT-3 took **~34 days** on a cluster of **thousands** of GPUs. Hardware failures, preemptions, and crashes are **guaranteed** over that time.
>
> Without checkpointing:
> - ❌ A crash at day 30 = **$10M+ in compute** lost
> - ❌ A bug found at day 20 = start completely over
>
> With checkpointing every 1000 steps:
> - ✅ A crash at day 30 → resume from 30 minutes ago
> - ✅ A bug at day 20 → rewind to a known-good checkpoint
>
> **Rule of thumb**: Save every 10-30 minutes of training time. Keep the last 3-5 checkpoints (rotating) plus the "best" one.

> ⭐ **Why save optimizer state?** AdamW maintains running averages ($m_t$, $v_t$) for every parameter. If you restart training without loading these, the optimizer "forgets" its momentum — your learning rate effectively restarts, causing a loss spike. Always save and reload optimizer state!

```python
    def save_checkpoint(self, tag=None):
        """Save model, optimizer, scheduler, and training state."""
        checkpoint = {
            'model_state_dict': self.model.state_dict(),
            'optimizer_state_dict': self.optimizer.state_dict(),
            'scheduler_state_dict': self.scheduler.state_dict(),
            'global_step': self.global_step,
            'epoch': self.epoch,
            'best_val_loss': self.best_val_loss,
            'config': self.config,
        }
        
        filename = f"checkpoint_step{self.global_step}.pt" if tag is None else f"checkpoint_{tag}.pt"
        path = self.checkpoint_dir / filename
        torch.save(checkpoint, path)
        print(f"  Saved checkpoint: {path}")
        
        # Clean old checkpoints (keep last 3)
        if tag is None:
            checkpoints = sorted(self.checkpoint_dir.glob("checkpoint_step*.pt"))
            for old_ckpt in checkpoints[:-3]:
                old_ckpt.unlink()
    
    def load_checkpoint(self, path):
        """Resume training from checkpoint."""
        print(f"Loading checkpoint: {path}")
        checkpoint = torch.load(path, map_location=self.device)
        
        self.model.load_state_dict(checkpoint['model_state_dict'])
        self.optimizer.load_state_dict(checkpoint['optimizer_state_dict'])
        self.scheduler.load_state_dict(checkpoint['scheduler_state_dict'])
        self.global_step = checkpoint['global_step']
        self.epoch = checkpoint['epoch']
        self.best_val_loss = checkpoint['best_val_loss']
        
        print(f"Resumed from step {self.global_step}")
```

---

# 6️⃣ Logging & Monitoring

> 🎯 **Beginner Analogy — Your Fitness Tracker** 📈⌚
>
> Would you go to the gym for 6 months and *never* check your progress? Of course not! You'd track reps, weight lifted, body weight, etc.
>
> **Logging** is the same for model training. We track:
>
> | Metric | What It Tells You | Healthy Range |
> |---|---|---|
> | `train_loss` | How well the model fits training data | Decreasing over time |
> | `val_loss` | How well the model generalizes | Should track train_loss |
> | `ppl` (perplexity) | "How surprised is the model?" ($e^{\text{loss}}$) | Lower = better |
> | `lr` | Current learning rate | Should follow warmup→cosine schedule |
> | `grad_norm` | Magnitude of gradients | 0.1–10 is healthy; >100 = 🚨 |
> | `tokens/sec` | Training throughput | Higher = faster training |

### 🏭 Production Logging Tools

| Tool | Best For | Key Feature |
|---|---|---|
| **Weights & Biases (W&B)** | Team experiments, hyperparameter sweeps | Beautiful dashboards, auto-comparison of runs |
| **TensorBoard** | Quick local visualization | Free, integrates with PyTorch |
| **MLflow** | Model registry, deployment pipelines | Tracks models + metadata |
| **Neptune.ai** | Large-scale experiment management | Great filtering & search |

> ⭐ **W&B in 3 Lines** (the industry standard):
> ```python
> import wandb
> wandb.init(project="my-llm", config=config)
> wandb.log({"loss": loss, "lr": lr, "grad_norm": grad_norm}, step=step)
> ```
> This gives you real-time loss curves, GPU utilization, system metrics, and automatic alerts if training goes wrong. **Every serious ML team uses something like this.**

```python
    def _log_metrics(self, train_loss, grad_norm):
        """Log training metrics."""
        lr = self.scheduler.get_last_lr()[0]
        ppl = math.exp(train_loss) if train_loss < 20 else float('inf')
        
        # Tokens per second
        elapsed = time.time() - self._last_log_time if hasattr(self, '_last_log_time') else 0
        tokens_per_sec = (
            self.config.get('log_interval', 10) * 
            self.config['batch_size'] * 
            self.config['seq_length'] / max(elapsed, 1e-6)
        )
        
        print(
            f"Step {self.global_step:6d} | "
            f"loss {train_loss:.4f} | "
            f"ppl {ppl:.2f} | "
            f"lr {lr:.2e} | "
            f"grad_norm {grad_norm:.2f} | "
            f"tok/s {tokens_per_sec:.0f}"
        )
        
        self._last_log_time = time.time()
        
        # Optional: wandb logging
        # wandb.log({
        #     'train/loss': train_loss,
        #     'train/ppl': ppl,
        #     'train/lr': lr,
        #     'train/grad_norm': grad_norm,
        #     'train/tokens_per_sec': tokens_per_sec,
        # }, step=self.global_step)
```

---

# 7️⃣ The Main Training Function

> 🎯 **Beginner Analogy — The Full Workout Program** 📋
>
> Sections 1–6 defined individual exercises. This function is the **complete workout program** that stitches them together:
>
> 1. 📂 Optionally load a previous save (resume)
> 2. 🔁 Loop through epochs until we hit our target step count
> 3. 📊 Final evaluation when done
> 4. 💾 Save the final model
> 5. ⏱️ Report total training time and best score
>
> Notice the outer loop is `while global_step < total_steps` — this means training is controlled by **step count**, not epoch count. This is standard for LLM pre-training because datasets are so large you rarely complete a full epoch.

```python
    def train(self, resume_from=None):
        """Full training loop."""
        if resume_from:
            self.load_checkpoint(resume_from)
        
        print(f"Starting training from step {self.global_step}")
        print(f"Total steps: {self.config['total_steps']}")
        print(f"Gradient accumulation: {self.grad_accum_steps}")
        print(f"Effective batch size: {self.config['batch_size'] * self.grad_accum_steps}")
        
        self._last_log_time = time.time()
        start_time = time.time()
        
        while self.global_step < self.config['total_steps']:
            self.epoch += 1
            self.train_epoch()
        
        # Final evaluation
        final_loss = self._evaluate()
        self.save_checkpoint(tag='final')
        
        total_time = time.time() - start_time
        print(f"\nTraining complete!")
        print(f"Total time: {total_time/3600:.1f} hours")
        print(f"Best val loss: {self.best_val_loss:.4f}")
        print(f"Best val PPL: {math.exp(self.best_val_loss):.2f}")


# Usage
config = {
    'learning_rate': 3e-4,
    'batch_size': 32,
    'seq_length': 512,
    'total_steps': 10000,
    'warmup_steps': 500,
    'weight_decay': 0.1,
    'max_grad_norm': 1.0,
    'gradient_accumulation_steps': 4,
    'log_interval': 10,
    'eval_interval': 500,
    'save_interval': 1000,
    'device': 'cuda',
    'checkpoint_dir': './checkpoints',
}

# trainer = Trainer(model, train_loader, val_loader, config)
# trainer.train()
```

---

# 8️⃣ Mixed Precision Training

> 🎯 **Beginner Analogy — Rough Drafts vs. Final Copy** ✏️📝
>
> When you write an essay, you don't write every draft in perfect calligraphy. You scribble rough drafts (**fp16** — half precision, 16-bit) to work fast, then do your **final check in ink** (**fp32** — full precision, 32-bit) to make sure nothing is wrong.
>
> **Mixed precision** training works the same way:
> - **Forward pass & most computations** → fp16 (fast, half the memory)
> - **Loss scaling & weight master copy** → fp32 (accurate, prevents errors)
>
> Result: **~2× faster training** and **~half the memory**, with virtually no quality loss! 🚀

### 📐 Why fp16 Needs Help (The Math Problem)

In fp16, the smallest representable positive number is:

$$\epsilon_{\text{fp16}} \approx 6 \times 10^{-8}$$

But gradients during training can be as small as $10^{-10}$ — they'd round down to **zero** (underflow)! 😱

**GradScaler** fixes this by:
1. **Scaling the loss up** by a large factor (e.g., 1024) before backward pass
2. The gradients also scale up proportionally (chain rule)
3. **Unscaling** gradients back down before the optimizer step

$$\text{scaled\_loss} = \text{loss} \times S$$
$$\nabla_\theta(\text{scaled\_loss}) = S \times \nabla_\theta(\text{loss})$$
$$\text{true\_gradient} = \frac{\nabla_\theta(\text{scaled\_loss})}{S}$$

This keeps small gradients above the fp16 floor. If any gradient becomes `inf` or `nan` (overflow), the scaler skips that optimizer step and reduces $S$. Self-healing! 🔧

### 🏭 Production Impact of Mixed Precision

| Metric | fp32 Only | Mixed Precision (AMP) | Improvement |
|---|---|---|---|
| Memory per batch | 100% | ~50-60% | 🔽 40-50% less |
| Training speed | Baseline | ~1.5-2× faster | 🔼 50-100% faster |
| Model quality | Baseline | ~Same (within noise) | ✅ No degradation |
| GPU utilization | Lower | Higher (Tensor Cores) | 🔼 Better hardware use |

> ⭐ **Modern GPUs (A100, H100) have dedicated Tensor Cores** that process fp16 matrix multiplications at 2-3× the speed of fp32. AMP lets you use them automatically.

### 🔬 Deep Research: Mixed Precision Origins

> **Micikevicius et al. (2018)** — *"Mixed Precision Training"* (NVIDIA & Baidu Research)
>
> This foundational paper demonstrated that mixed precision training works across a wide range of tasks (image classification, detection, translation, speech, GANs) with **no loss in accuracy** when using:
> 1. fp32 master weights
> 2. Loss scaling to preserve small gradient values
> 3. fp16 arithmetic for forward/backward passes
>
> Their key insight: most of the neural network's computation is **tolerant** of lower precision, and the few sensitive parts (weight updates, reductions) can stay in fp32 at minimal cost.
>
> 📖 *Citation*: Micikevicius, P., Narang, S., Alben, J., et al. (2018). "Mixed Precision Training." *ICLR 2018. arXiv:1710.03740*.

```python
# Using torch.cuda.amp for automatic mixed precision
from torch.cuda.amp import GradScaler, autocast

class AMPTrainer(Trainer):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.scaler = GradScaler()
    
    def train_step(self, batch):
        input_ids = batch['input_ids'].to(self.device)
        labels = batch['labels'].to(self.device)
        
        # Autocast: automatically use fp16 where safe
        with autocast():
            logits, loss = self.model(input_ids, labels)
            loss = loss / self.grad_accum_steps
        
        # Scaled backward (prevents fp16 underflow)
        self.scaler.scale(loss).backward()
        
        return loss.item() * self.grad_accum_steps
    
    def optimizer_step(self):
        # Unscale gradients for clipping
        self.scaler.unscale_(self.optimizer)
        
        grad_norm = torch.nn.utils.clip_grad_norm_(self.model.parameters(), 1.0)
        
        # Step with scaler (skips step if gradients have inf/nan)
        self.scaler.step(self.optimizer)
        self.scaler.update()
        self.scheduler.step()
        self.optimizer.zero_grad(set_to_none=True)
        
        self.global_step += 1
        return grad_norm.item()
```

---

# 9️⃣ Training Diagnostics

> 🎯 **Beginner Analogy — Reading Your Blood Work** 🩺
>
> After a checkup, the doctor looks at your numbers to see if things are healthy. Training diagnostics are the same — you "read the numbers" to decide if training is going well or if something needs intervention.

## Healthy Training Signs
- ✅ Loss consistently decreasing
- ✅ Gradient norm stable (1-10 range)
- ✅ Learning rate following expected schedule
- ✅ Val loss tracking train loss (not diverging)

## Red Flags
- **Loss spikes** 🚨: reduce LR, increase warmup
- **Gradient explosion** (norm > 100) 💥: reduce LR, add gradient clipping
- **Loss NaN** ❌: check for numerical issues, reduce LR
- **Val loss increasing** (train decreasing) 📈📉: overfitting, add regularization
- **Loss plateau early** ⏸️: LR too low, or data quality issues

### 🔍 Diagnostic Decision Tree

```
Loss going down?
├── YES → Grad norm stable (1-10)?
│   ├── YES → Val loss also going down?
│   │   ├── YES ✅ → Everything healthy! Keep training.
│   │   └── NO 📈 → Overfitting! Add dropout/weight decay/more data
│   └── NO 💥 → Gradient issues!
│       ├── grad_norm > 100 → Reduce LR or increase clip value
│       └── grad_norm → 0 → Vanishing gradients — check architecture
├── NO → Loss stuck (plateau)?
│   ├── YES ⏸️ → LR too low? Data quality? Try LR warmup.
│   └── NO (increasing) 🚨 → LR too high! Reduce immediately.
└── NaN ❌ → Numerical instability
    → Reduce LR, check data for bad values, enable AMP with GradScaler
```

### 📊 Gradient Norm — Your Most Important Vital Sign

| Grad Norm Range | Interpretation | Action |
|---|---|---|
| 0 – 0.01 | Vanishing gradients | Check skip connections, LR too low |
| 0.1 – 1.0 | Very stable training | Ideal for fine-tuning |
| 1.0 – 10.0 | Normal for pre-training | ✅ Healthy |
| 10 – 100 | Somewhat unstable | Monitor closely, maybe reduce LR |
| > 100 | Exploding gradients 💥 | Clip harder, reduce LR, add warmup |

---

# 🏭 10. Distributed Training (Bonus Production Section)

> When one GPU isn't enough, you need to train across **multiple GPUs** (or even multiple machines). Here's how the training loop changes:

### 📊 Distributed Training Strategies

| Strategy | What It Distributes | Best For | Framework |
|---|---|---|---|
| **DataParallel (DP)** | Data across GPUs on 1 machine | Quick prototyping | `torch.nn.DataParallel` |
| **DistributedDataParallel (DDP)** | Data across GPUs, any # of machines | Standard multi-GPU training | `torch.nn.parallel.DistributedDataParallel` |
| **FSDP** | Shards model weights + gradients + optimizer states | Very large models (>10B params) | `torch.distributed.fsdp.FullyShardedDataParallel` |
| **DeepSpeed ZeRO** | 3 stages of memory optimization | Maximum memory efficiency | Microsoft DeepSpeed |

> 🏋️ **Analogy**: DDP = multiple people each lifting a full barbell but doing different exercises (different data). FSDP = multiple people **each holding one piece of the barbell** because the full barbell is too heavy for anyone alone.

### ⭐ DDP in 5 Lines (The Most Common Approach)

```python
import torch.distributed as dist
from torch.nn.parallel import DistributedDataParallel as DDP

dist.init_process_group("nccl")  # Initialize communication
model = DDP(model.to(local_rank), device_ids=[local_rank])
# ... rest of training loop is IDENTICAL — DDP handles gradient sync!
```

> The key insight: DDP **synchronizes gradients** across GPUs after each backward pass using all-reduce. Each GPU trains on different data, but they all end up with the **same model weights** because gradients are averaged. The training loop code barely changes!

---

# 📝 Summary

| Component | Key Detail |
|-----------|-----------|
| Gradient accumulation | Simulate large batches on small GPUs |
| Checkpointing | Save model + optimizer + scheduler state |
| Resumption | Load all state to continue training seamlessly |
| Mixed precision | AMP for 2× speed, GradScaler prevents underflow |
| Logging | Loss, PPL, LR, grad_norm, tokens/sec |
| Validation | Periodic eval, track best model |
| Gradient clipping | max_norm=1.0 prevents divergence |

---

## 🧾 Complete Cheat Sheet — The Training Loop

### 🔄 The 6-Step Core Loop (Memorize This!)

```
1. output = model(input)          # 🔮 Forward: cook the dish
2. loss = criterion(output, target) # 📏 Loss: taste it
3. loss.backward()                 # 🔙 Backward: what went wrong?
4. clip_grad_norm_(params, 1.0)    # ✂️ Clip: don't overreact
5. optimizer.step()                # 📐 Step: adjust the recipe
6. optimizer.zero_grad()           # 🧹 Zero: clear your notes
```

### 📊 Quick Reference: Key Hyperparameters

| Hyperparameter | Typical Range (LLM Pre-training) | What It Controls |
|---|---|---|
| Learning rate | $3 \times 10^{-4}$ to $1 \times 10^{-3}$ | How big each weight update is |
| Warmup steps | 500 – 2000 | Gradual LR ramp-up at start |
| Weight decay | 0.01 – 0.1 | Regularization strength |
| Max grad norm | 1.0 | Gradient clipping threshold |
| Batch size (effective) | 256 – 4M tokens | Samples per weight update |
| Adam $\beta_1$ | 0.9 | Momentum (first moment) |
| Adam $\beta_2$ | 0.95 – 0.999 | Adaptive LR (second moment) |

### 🏭 Production Checklist

- [ ] ✅ AdamW optimizer with param groups (decay vs no-decay)
- [ ] ✅ Cosine LR schedule with linear warmup
- [ ] ✅ Gradient accumulation for effective batch size
- [ ] ✅ Mixed precision (AMP) with GradScaler
- [ ] ✅ Gradient clipping (max_norm=1.0)
- [ ] ✅ Checkpointing every N steps (save ALL state)
- [ ] ✅ Checkpoint rotation (keep last 3 + best)
- [ ] ✅ Validation loop with @torch.no_grad()
- [ ] ✅ Logging to W&B or TensorBoard
- [ ] ✅ Track tokens/sec for throughput monitoring
- [ ] ✅ Distributed training (DDP/FSDP) for multi-GPU

### 📚 Key Research Papers

| Paper | Year | Key Contribution |
|---|---|---|
| Goyal et al. — "Accurate, Large Minibatch SGD" | 2017 | Linear scaling rule + warmup for large batches |
| Micikevicius et al. — "Mixed Precision Training" | 2018 | fp16/fp32 mixed training with loss scaling |
| Loshchilov & Hutter — "Decoupled Weight Decay" (AdamW) | 2019 | Fixed weight decay in Adam; now the standard |
| Rajbhandari et al. — "ZeRO" (DeepSpeed) | 2020 | Memory-efficient distributed training |
| Zhao et al. — "PyTorch FSDP" | 2023 | Fully sharded data parallelism in PyTorch |

### 🎯 Analogy Recap

| Concept | Analogy | Why It Helps |
|---|---|---|
| Training loop | 🏋️ Workout routine | Repeated practice makes perfect |
| Forward pass | 🍳 Cooking the dish | Input → prediction |
| Loss | 👅 Tasting — how bad? | Measures error |
| Backward pass | 🤔 Figuring out what went wrong | Computes gradients |
| optimizer.step() | ✏️ Adjusting the recipe | Updates weights |
| zero_grad() | 🧹 Clearing your notes | Prevents gradient accumulation |
| Gradient accumulation | 🏋️ Push-ups in sets of 4 | Small GPU → big effective batch |
| Mixed precision | ✏️ Rough draft (fp16) → final check (fp32) | 2× speed, half memory |
| Checkpointing | 💾 Save game | Crash recovery |
| Validation | ⚖️ Weekly weigh-in | Check generalization |
| Gradient clipping | 🛑 Speed limit sign | Prevents explosions |
| Warmup | 🔥 Preheating the oven | Stable early training |

**Tomorrow**: Text Generation Strategies — Greedy, Temperature, Top-k, Top-p (Nucleus Sampling) and Beam Search.
