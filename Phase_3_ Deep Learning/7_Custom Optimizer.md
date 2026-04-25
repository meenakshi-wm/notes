📘 DAY 7 — Custom Optimizer — Deep Control Over Parameter Updates 🏗️⚡🔧

---

## 🎯 Goal

Understand optimizers not as "black boxes that update weights," but as **carefully designed update rules** with deep mathematical foundations. Learn HOW to write a **custom optimizer** in PyTorch by implementing SGD with momentum, Adam, and a novel optimizer from scratch. Understand parameter groups, state management, the `step()` interface, and why the choice of optimizer can **make or break** your model's convergence. A founder who controls the optimizer controls the training dynamics.

---

## 🧠 If This Is Deeply Understood

You can implement **ANY** optimizer from a paper, debug optimization failures by inspecting optimizer state, create custom update rules for specific architectures, and understand why Adam works for transformers but SGD + momentum works for CNNs.

> 🎒 **Beginner Analogy — What is an Optimizer?**
>
> Imagine you're trying to find the lowest point in a dark, hilly landscape while blindfolded 🏔️👀. You can only feel the slope under your feet (that's the **gradient**). The **optimizer** is your **driving instructor** — it tells you:
> - **Which direction** to step (downhill!)
> - **How big** a step to take (learning rate)
> - **Whether to remember** past steps (momentum)
>
> Different optimizers are like different instructors: some are cautious (SGD), some are adaptive and clever (Adam), and some are experimental (Lion). Building a **custom optimizer** means becoming your OWN instructor! 🚗💨

---

## 🏭 Production Context — Why This Matters in Industry

| Company / System | Optimizer Choice | Why |
|---|---|---|
| 🔵 **OpenAI (GPT-4)** | AdamW + cosine schedule + warmup | Standard LLM training recipe |
| 🟣 **Meta (LLaMA 2/3)** | AdamW with β₁=0.9, β₂=0.95 | Lower β₂ for training stability |
| 🔴 **Google (Gemini)** | Adafactor / Adam variants | Memory efficiency at trillion-param scale |
| 🟢 **NVIDIA (Megatron-LM)** | Adam + FSDP sharding | Distributed optimizer state |
| 🟡 **Stability AI** | Lion optimizer | 2× lower memory than Adam |
| 🔵 **Hugging Face fine-tuning** | AdamW + differential LR | Low LR for backbone, high LR for head |

---

# 1️⃣ The Optimizer Interface — What Happens in `optimizer.step()` 🎛️🔄

## ⭐ The Universal Update Equation

Every optimizer implements a variant of:

$$\theta_{t+1} = \theta_t - \eta \cdot g(\nabla L(\theta_t), \text{state})$$

Where:
- $\theta$ are the **parameters** (weights and biases of your neural network)
- $\eta$ is the **learning rate** (how big each step is)
- $\nabla L(\theta_t)$ is the **gradient** (computed by `backward()` — tells you which direction is "uphill")
- $g(\cdot)$ is the **optimizer-specific transformation** (the secret sauce!)
- **state** includes momentum buffers, moving averages, step counts, etc.

> 🎒 **Beginner Analogy:** Think of $\theta$ as your current GPS location 📍, the gradient $\nabla L$ as a compass pointing uphill ⬆️, and $\eta$ as your stride length 🦶. The optimizer function $g(\cdot)$ decides whether to just walk downhill (SGD), or use memory of past steps to be smarter about the path (momentum/Adam).

## ⭐ PyTorch Optimizer Structure

```python
class Optimizer:
    def __init__(self, params, defaults):
        self.param_groups = [...]  # List of parameter group dicts
        self.state = {}            # Per-parameter state (momentum, etc.)
    
    def zero_grad(self):
        """Zero all parameter gradients."""
        for group in self.param_groups:
            for p in group['params']:
                if p.grad is not None:
                    p.grad.zero_()
    
    def step(self):
        """Perform one optimization step."""
        raise NotImplementedError
```

| Component | What It Stores | Analogy |
|---|---|---|
| `param_groups` | List of dicts — each group has `params` + hyperparams (lr, momentum, etc.) | Different classrooms with different teaching speeds 🏫 |
| `state` | Per-parameter dictionary — momentum buffer, step count, etc. | Each student's personal notebook 📓 |
| `zero_grad()` | Resets all `.grad` tensors to zero | Erasing the whiteboard before the next lesson 🧹 |
| `step()` | Applies the update rule to all parameters | The actual learning step 🚶 |

## ⭐ Parameter Groups — Different Settings for Different Layers

Different parts of the model can have different hyperparameters:

```python
optimizer = torch.optim.Adam([
    {'params': model.backbone.parameters(), 'lr': 1e-4},      # Pretrained: small LR
    {'params': model.classifier.parameters(), 'lr': 1e-2},     # New layers: large LR
], weight_decay=1e-5)
```

> 🎒 **Beginner Analogy — Parameter Groups:** Imagine a school where different students learn at different speeds 🏫. The pretrained backbone layers are like **experienced students** — they already know a lot, so you give them gentle nudges (low learning rate = $10^{-4}$). The new classifier layers are **beginners** — they need big, aggressive updates (high learning rate = $10^{-2}$). Parameter groups let you teach each "student" at their optimal pace!

This is **essential for fine-tuning**: pretrained layers need gentle updates, new layers need aggressive updates.

> 🔬 **Deep Research — μP (Yang et al., 2022):** The Maximal Update Parameterization (μP) paper showed that **optimal hyperparameters (including per-layer learning rates) can transfer across model sizes**. You tune on a small model, then scale up without re-tuning! This is how Microsoft trained large models efficiently. Paper: *"Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer"*.

---

# 2️⃣ SGD with Momentum — Full Derivation 🏃‍♂️💨

## ⭐ Vanilla SGD (Stochastic Gradient Descent)

The simplest possible optimizer — just walk downhill:

$$\theta_{t+1} = \theta_t - \eta \nabla L(\theta_t)$$

Or equivalently: $w \leftarrow w - \alpha g$ where $g = \nabla L$ is the gradient and $\alpha = \eta$ is the learning rate.

> 🎒 **Beginner Analogy:** Vanilla SGD is like walking downhill with **no memory** 🧠❌. Every step, you look at the current slope and walk that direction. You never remember what happened before. This makes you:
> - **Zigzag** back and forth in narrow valleys (like a drunk person walking between walls) 🍺
> - **Crawl** painfully slowly across flat plateaus 🐌
> - **Get stuck** in sharp valleys where the walls are steep but the floor slopes gently

**Problems with Vanilla SGD:**

| Problem | What Happens | Visual |
|---|---|---|
| 🔀 **Oscillation** | Bounces back and forth in steep directions | Like a pinball between walls |
| 🐌 **Slow convergence** | Tiny progress in flat directions | Crawling through mud |
| 🕳️ **Sharp valleys** | Overshoots across the valley, undershoots along it | Zigzag instead of straight line |

## ⭐ Adding Momentum (Polyak, 1964) — The Key Insight

Maintain a **velocity vector** — remember where you've been going:

$$v_t = \mu \cdot v_{t-1} + \nabla L(\theta_t)$$

$$\theta_{t+1} = \theta_t - \eta \cdot v_t$$

Or in simpler notation: $v \leftarrow \beta v + g$, then $w \leftarrow w - \alpha v$

Where $\mu \in [0, 1)$ is the **momentum coefficient** (typically 0.9).

> 🎒 **Beginner Analogy — The Rolling Ball ⚽:** Imagine a ball rolling down a hilly landscape:
> - $v_t$ is the ball's **velocity** (speed + direction)
> - $\mu$ is like $(1 - \text{friction})$ — how much velocity carries over from the last step (0.9 means the ball keeps 90% of its speed!)
> - $\nabla L$ is the new **push from gravity** (the gradient)
>
> The ball builds up speed going downhill consistently, but the sideways oscillations **cancel out** because they alternate directions! 🎯

## ⭐ Why Momentum Helps — The Mathematics of Cancellation

In a valley with steep walls and a gentle slope toward the minimum:

| Direction | Gradient Behavior | Momentum Effect |
|---|---|---|
| **Across the valley** (steep walls) | Alternates sign each step: $+g, -g, +g, -g...$ | Cancels out! $v \approx 0$ 🎯 |
| **Along the valley** (gentle slope) | Consistent same direction: $+g, +g, +g, +g...$ | Accumulates! $v$ grows large 🚀 |

**Effective step size** in the consistent direction:

$$\text{effective step} = \frac{\eta}{1 - \mu} \approx 10\times \text{ with } \mu = 0.9$$

So momentum gives you a **10× speedup** in the right direction while suppressing oscillation! 🔥

## ⭐ Nesterov Momentum (Nesterov, 1983) — "Look Ahead"

Standard momentum: compute gradient at current position, then move.
Nesterov: **look ahead** to where momentum would take you, compute gradient THERE:

$$v_t = \mu \cdot v_{t-1} + \nabla L(\theta_t - \eta \cdot \mu \cdot v_{t-1})$$

$$\theta_{t+1} = \theta_t - \eta \cdot v_t$$

The gradient is evaluated at the **"predicted" next position**, not the current one.
This gives a **corrective term** that reduces overshooting.

> 🎒 **Beginner Analogy:** Regular momentum is like driving while looking at the road **under your car** 🚗. Nesterov momentum is like looking **ahead through the windshield** 🔭 — you see what's coming and can correct before you overshoot!

**PyTorch implementation** (reparameterized for efficiency):

$$v_t = \mu \cdot v_{t-1} + \nabla L(\theta_t)$$

$$\theta_{t+1} = \theta_t - \eta \cdot (\mu \cdot v_t + \nabla L(\theta_t))$$

> 🔬 **Deep Research — SGD vs Adam Generalization:** Numerous studies (Wilson et al., 2017; Keskar & Socher, 2017) found that **SGD with momentum often generalizes better than Adam** on vision tasks (CNNs). The hypothesis: SGD finds **flatter minima** in the loss landscape, which generalize better to unseen data. This is still an active area of research. Paper: *"The Marginal Value of Adaptive Gradient Methods in Machine Learning"*.

---

# 3️⃣ Adam — The Transformer Default 🤖👑

## ⭐ The Algorithm (Kingma & Ba, 2015)

Adam combines **momentum** (first moment) with **per-parameter adaptive learning rates** (second moment):

**Step 1 — Update first moment (momentum / direction memory):**

$$m_t = \beta_1 \cdot m_{t-1} + (1 - \beta_1) \cdot g_t$$

**Step 2 — Update second moment (gradient magnitude memory):**

$$v_t = \beta_2 \cdot v_{t-1} + (1 - \beta_2) \cdot g_t^2$$

**Step 3 — Bias correction (fix the initialization problem):**

$$\hat{m}_t = \frac{m_t}{1 - \beta_1^t} \qquad \hat{v}_t = \frac{v_t}{1 - \beta_2^t}$$

**Step 4 — Update parameters:**

$$\theta_{t+1} = \theta_t - \eta \cdot \frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}$$

**Default hyperparameters:** $\beta_1 = 0.9$, $\beta_2 = 0.999$, $\epsilon = 10^{-8}$

> 🎒 **Beginner Analogy — Adam as a Smart GPS Navigator 🗺️:**
>
> | Adam Component | What It Does | GPS Analogy |
> |---|---|---|
> | $m_t$ (first moment) | Remembers the **average direction** of past gradients | GPS remembers which direction you've been heading 🧭 |
> | $v_t$ (second moment) | Remembers the **average magnitude** (how bumpy the road is) | GPS knows whether you're on a highway or a dirt road 🛣️ |
> | $\hat{m}_t / \sqrt{\hat{v}_t}$ | **Normalize**: big gradients get smaller steps, small gradients get bigger steps | GPS adjusts speed: slow on bumpy roads, fast on smooth highways 🚗 |
> | $\epsilon$ | Prevents division by zero | Safety net so you don't divide by nothing 🛡️ |

## ⭐ Why Bias Correction? The Cold Start Problem

At $t=1$: $m_1 = (1 - \beta_1) \cdot g_1 \approx 0.1 \cdot g_1$ — **biased toward 0!** 😰

The **true** first moment estimate should be $\approx g_1$.

Division by $(1 - \beta_1^t)$ corrects for the initialization at $m_0 = 0$:

$$\hat{m}_1 = \frac{m_1}{1 - 0.9^1} = \frac{0.1 \cdot g_1}{0.1} = g_1 \quad \checkmark$$

> 🎒 **Beginner Analogy:** Imagine you're computing a running average of temperatures 🌡️, but you started your thermometer at 0°C instead of the actual temperature. Your first few readings will be biased toward zero. Bias correction "undoes" this cold-start effect!

**When can we ignore bias correction?** For large $t$, $\beta_1^t \to 0$, so $(1 - \beta_1^t) \to 1$. Practically:
- At $t = 100$: $1 - 0.9^{100} \approx 1.0$ — correction for $m$ is negligible ✅
- At $t = 1000$: $1 - 0.999^{1000} \approx 0.63$ — correction for $v$ still matters! ⚠️
- At $t = 3000$: $1 - 0.999^{3000} \approx 0.95$ — now safe to ignore for $v$ too ✅

## ⭐ Why Adam Works for Transformers

Transformers have **wildly different gradient magnitudes** across parameters:

| Layer Type | Gradient Magnitude | Why |
|---|---|---|
| 🔤 **Embedding layers** | Large gradients | Sparse updates, few tokens active |
| 🎯 **Attention weights** ($W_Q, W_K, W_V$) | Small gradients | Distributed across many heads |
| 📊 **LayerNorm parameters** | Very small gradients | Small scale/shift values |
| 🔢 **Final projection** | Medium gradients | Dense, output-facing |

**SGD treats all parameters equally** — bad for this heterogeneity! 😫
**Adam gives each parameter its own effective learning rate** — adapts automatically! 🎯

$$\text{Effective LR for parameter } i = \frac{\eta}{\sqrt{\hat{v}_{t,i}} + \epsilon}$$

Parameters with large gradients get small effective LR. Parameters with small gradients get large effective LR. **Automatic adaptation!** 🧠

## ⭐ AdamW — Weight Decay Done Right (Loshchilov & Hutter, 2019) 🔑

**The problem with standard Adam + L2 regularization:**

Standard L2 adds $\frac{\lambda}{2}\|\theta\|^2$ to the loss:

$$L_{\text{total}} = L_{\text{data}} + \frac{\lambda}{2}\|\theta\|^2 \implies \text{gradient includes } \lambda\theta$$

But Adam **normalizes** this regularization gradient by the second moment $\sqrt{\hat{v}}$ → the weight decay effect is **diminished!** 😱

**AdamW decouples weight decay from the gradient:**

$$\theta_{t+1} = \theta_t - \eta \cdot \frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon} - \eta \cdot \lambda \cdot \theta_t$$

Or equivalently: $w \leftarrow (1 - \lambda\alpha)w - \alpha \cdot \frac{\hat{m}}{\sqrt{\hat{v}} + \epsilon}$

The weight decay term $-\eta \lambda \theta_t$ is **NOT normalized** by the adaptive learning rate.

> 🎒 **Beginner Analogy — Weight Decay as a "Forgetting" Force 🧹:**
> Weight decay is like a gentle spring pulling all weights back toward zero. In standard Adam, this spring force gets weakened by the adaptive scaling (imagine the spring being stretched differently for each weight). AdamW keeps the spring force CONSTANT — every weight shrinks by the same percentage each step, regardless of gradient history.

This is what PyTorch's `torch.optim.AdamW` implements. **This is the standard for ALL modern LLM training (GPT-4, LLaMA, Gemini, Claude).** 🏆

> 🔬 **Deep Research — AdamW is Not Optional:** Loshchilov & Hutter (2019) proved that L2 regularization inside Adam is mathematically NOT equivalent to weight decay. The decoupled version (AdamW) consistently outperforms. Paper: *"Decoupled Weight Decay Regularization"*. Every major LLM uses AdamW — it's considered a solved design choice.

---

# 4️⃣ Other Important Optimizers 🗂️🧪

> 🎒 **Beginner Context:** Adam is the most popular optimizer, but it's not the only one! Each optimizer below was designed to solve a **specific problem** — understanding them helps you pick the right tool for the job. Think of it like a toolbox 🧰: you wouldn't use a hammer to screw in a bolt!

## ⭐ RMSprop (Hinton, unpublished lecture notes, 2012) — Adam's Grandfather

$$v_t = \beta \cdot v_{t-1} + (1 - \beta) \cdot g_t^2$$

$$\theta_{t+1} = \theta_t - \eta \cdot \frac{g_t}{\sqrt{v_t} + \epsilon}$$

Adam **without momentum** and **without bias correction**. Still useful for RNNs.

> 🎒 **Analogy:** If Adam is a modern car with GPS + cruise control 🚗, RMSprop is a car with just cruise control — it adapts speed but doesn't remember direction.

## ⭐ Adagrad (Duchi et al., 2011) — The Sparse Gradient Specialist

$$v_t = v_{t-1} + g_t^2 \quad \text{(accumulate ALL past squared gradients)}$$

$$\theta_{t+1} = \theta_t - \eta \cdot \frac{g_t}{\sqrt{v_t} + \epsilon}$$

**Key property:** Learning rate **monotonically decreases** over time → eventually stops learning! 🛑

| Aspect | Adagrad Behavior |
|---|---|
| ✅ **Good for** | Sparse gradients (NLP with embeddings, recommender systems) |
| ❌ **Bad for** | Long training runs (LR decays to near zero) |
| 🧠 **Intuition** | Parameters that get updated rarely keep a high LR; frequently updated parameters get a low LR |

> 🏭 **Production:** Google used Adagrad extensively in their ad click prediction systems where feature vectors are extremely sparse — most features are zero.

## ⭐ LARS / LAMB — Large-Batch Training Heroes 🦸

**LARS (You et al., 2017):** Layer-wise Adaptive Rate Scaling

$$\theta_{t+1} = \theta_t - \eta \cdot \frac{\|\theta_t\|}{\|\nabla L_t\|} \cdot \nabla L_t$$

Normalizes the update by the **ratio of weight norm to gradient norm per layer**.

**LAMB (You et al., 2020):** LARS applied to Adam — Layer-wise Adaptive Moments for large Batch training.

Used to train **BERT with batch size 65,536** without loss of accuracy! 🚀

> 🎒 **Analogy:** When driving with a huge convoy (large batch) 🚛🚛🚛, normal driving rules don't work. LARS/LAMB adjust the steering for each truck independently based on its size and load.

> 🔬 **Deep Research:** LAMB was critical for reducing BERT pretraining from 3 days on 16 TPUs to **76 minutes on 1024 TPUs** — a 60× speedup through large-batch training. Paper: *"Large Batch Optimization for Deep Learning: Training BERT in 76 Minutes"*.

## ⭐ Lion (Chen et al., 2024) — Memory-Efficient Sign-Based Optimizer 🦁

Found by **neural architecture search** (using AI to design optimizers!):

$$m_t = \beta_1 \cdot m_{t-1} + (1 - \beta_1) \cdot g_t$$

$$\theta_{t+1} = \theta_t - \eta \cdot (\text{sign}(m_t) + \lambda \cdot \theta_t)$$

| Feature | Adam | Lion |
|---|---|---|
| 💾 **Memory per parameter** | 2 extra buffers ($m$ and $v$) | 1 extra buffer ($m$ only) |
| 🧮 **Computation** | Needs $\sqrt{\cdot}$ and division | Only `sign()` — cheaper! |
| 📊 **Update magnitude** | Variable (depends on $\hat{v}$) | Always $\pm \eta$ (uniform) |
| 🏆 **Performance** | Gold standard | Competitive with AdamW on transformers |

Only uses the **SIGN** of the momentum — no division, no square root.
Lower memory than Adam (no $v$ buffer). Competitive with AdamW on transformers. 🏆

> 🏭 **Production:** Stability AI and other companies have adopted Lion for training diffusion models — the 50% memory savings allows training larger models on the same hardware.

## ⭐ Sophia (Liu et al., 2023) — Second-Order Optimizer for LLMs 🧠

Sophia uses **diagonal Hessian estimates** (second-order information) for per-parameter pre-conditioning:

$$\theta_{t+1} = \theta_t - \eta \cdot \text{clip}\left(\frac{m_t}{h_t}, \rho\right)$$

Where $h_t$ is a diagonal Hessian estimate (how curved the loss surface is).

**Key insight:** Divides by **curvature** instead of gradient magnitude. Parameters in flat regions get large updates; parameters in curved regions get small updates.

> 🔬 **Deep Research:** Sophia achieves **2× speedup** over Adam for LLM pre-training by using Hessian information. The clipping bound $\rho$ prevents catastrophically large updates in flat regions. Paper: *"Sophia: A Scalable Stochastic Second-order Optimizer for Language Model Pre-training"*.

## ⭐ Schedule-Free Optimizer (Defazio et al., 2024) — No LR Schedule Needed! 🎉

Removes the need for learning rate scheduling entirely by maintaining two sequences of iterates:

$$y_t = (1 - \beta) z_t + \beta x_t$$

$$z_{t+1} = z_t - \gamma \nabla f(y_t)$$

$$x_{t+1} = (1 - \frac{1}{t+1}) x_t + \frac{1}{t+1} z_{t+1}$$

> 🔬 **Deep Research:** This removes one of the most annoying hyperparameters in deep learning — the LR schedule. In experiments, it matches or beats carefully tuned cosine schedules. Paper: *"The Road Less Scheduled"* (Defazio et al., 2024).

---

# 5️⃣ Optimizer State — What Lives Inside 📦🧠

> 🎒 **Beginner Analogy:** Every optimizer has a **personal notebook** for each parameter 📓. SGD's notebook is thin (maybe just velocity). Adam's notebook is thick (momentum $m$, squared gradient $v$, and step count). The bigger the notebook, the **more memory** the optimizer needs!

## ⭐ Adam State Per Parameter

```python
# For each parameter p, Adam maintains:
state = {
    'step': 0,         # Number of updates (for bias correction)
    'exp_avg': zeros(), # mₜ — first moment (same shape as p)
    'exp_avg_sq': zeros()  # vₜ — second moment (same shape as p)
}
```

## ⭐ The Memory Problem — Optimizer State is HUGE! 💰

Total state memory for Adam: **2× the model parameters** (one buffer for $m$, one for $v$).

| Model Size | Weight Memory (FP32) | Adam State Memory | Total |
|---|---|---|---|
| 🟢 **125M** (GPT-2 Small) | 0.5 GB | 1.0 GB | 1.5 GB |
| 🟡 **1.3B** (GPT-2 XL) | 5.2 GB | 10.4 GB | 15.6 GB |
| 🟠 **7B** (LLaMA-7B) | 28 GB | 56 GB | 84 GB |
| 🔴 **70B** (LLaMA-70B) | 280 GB | 560 GB | 840 GB |
| 💀 **175B** (GPT-3) | 700 GB | 1.4 TB | 2.1 TB |

For a **7B parameter model**: $7B \times 2 \times 4 \text{ bytes} = 56 \text{ GB}$ just for optimizer state! 😱

## ⭐ Why Optimizer State Matters in Practice

### 1. 💾 Checkpoint Saving — You MUST Save Optimizer State
Must save optimizer state to resume training correctly. Loading only model weights → momentum and adaptive rates are lost.

```python
# ✅ Correct: save everything
torch.save({
    'model': model.state_dict(),
    'optimizer': optimizer.state_dict(),  # DON'T FORGET THIS!
    'epoch': epoch,
}, 'checkpoint.pt')
```

### 2. 📊 Memory Budgeting — Often the Bottleneck
For large models, optimizer state is often the memory bottleneck.

**Solutions:**

| Solution | Memory Savings | How It Works |
|---|---|---|
| 🔢 **8-bit Adam** (bitsandbytes) | 75% reduction | Quantize $m$, $v$ to 8-bit integers |
| 📐 **Adafactor** | ~67% reduction | Factorize $v$ as outer product of row/column vectors |
| 🌐 **ZeRO Stage 1** (DeepSpeed) | Divide by GPU count | Shard optimizer state across GPUs |
| 🦁 **Lion optimizer** | 50% reduction | No $v$ buffer needed — only $m$ |

### 3. 🔄 Fine-tuning — Fresh Start Usually Better
When fine-tuning, should you inherit optimizer state from pretraining? **Usually NO** — the new task has different gradient statistics. Start with fresh optimizer state! 🆕

> 🔬 **Deep Research — ZeRO Optimizer (Rajbhandari et al., 2020):** DeepSpeed's ZeRO (Zero Redundancy Optimizer) **shards optimizer state across GPUs** so each GPU only stores $1/N$ of the state. ZeRO Stage 1 shards optimizer state, Stage 2 adds gradients, Stage 3 adds parameters. This is how models like GPT-3 (175B) are trained across thousands of GPUs. Paper: *"ZeRO: Memory Optimizations Toward Training Trillion Parameter Models"*.

> 🔬 **Deep Research — 8-bit Optimizers (Dettmers et al., 2022):** The `bitsandbytes` library quantizes Adam's $m$ and $v$ buffers from FP32 to INT8, reducing optimizer memory by **75%**. Uses block-wise quantization with dynamic exponent to maintain accuracy. This enables fine-tuning 7B models on a single consumer GPU! Paper: *"8-bit Optimizers via Block-wise Quantization"*.

---

# 6️⃣ Learning Rate Scheduling 📈📉🕐

## ⭐ Why Schedule the Learning Rate?

> 🎒 **Beginner Analogy — Driving Speed Limits 🚗:**
>
> Imagine driving to a destination on different roads:
> - **Highway (early training):** Drive FAST 🏎️ — you're far from the destination, need to cover ground quickly → **high learning rate**
> - **City streets (mid training):** Slow down ⚠️ — getting closer, need more control → **medium learning rate**
> - **Parking lot (late training):** Crawl 🐌 — precise maneuvering into the parking spot (minimum) → **low learning rate**
>
> **LR scheduling** automatically changes your "speed limit" as training progresses!

**The intuition:**
- $\eta$ **high early** → fast exploration of the loss landscape 🔍
- $\eta$ **low later** → fine-grained convergence to a sharp minimum 🎯

## ⭐ Common Schedules

### 📉 Step Decay

LR drops by a constant factor every $N$ epochs:

$$\eta_t = \eta_0 \cdot \gamma^{\lfloor t / N \rfloor}$$

Where $\gamma \in (0, 1)$ is the decay factor (e.g., 0.1) and $N$ is the step size in epochs.

> Example: Start at $\eta = 0.1$, multiply by 0.1 every 30 epochs → $0.1 \to 0.01 \to 0.001$

### 📉 Exponential Decay

LR decays smoothly every step:

$$\eta_t = \eta_0 \cdot \gamma^t$$

### 🌊 Cosine Annealing — The Most Popular!

LR follows a **cosine curve** from max to min:

$$\eta_t = \eta_{\min} + \frac{1}{2}(\eta_{\max} - \eta_{\min})\left(1 + \cos\left(\frac{\pi t}{T}\right)\right)$$

> 🎒 **Beginner Analogy — The Pendulum 🎢:** Cosine annealing is like a pendulum gradually losing energy. It starts with high energy (high LR), smoothly decelerates, and comes to rest (low LR). The smooth curve avoids the jarring "jumps" of step decay.

### 🔥 Warmup + Cosine Decay — The LLM Standard! 🏆

This is what **GPT-4, LLaMA, Gemini, Claude** all use:

$$\eta(t) = \begin{cases} \eta_{\max} \cdot \frac{t}{T_{\text{warmup}}} & \text{if } t < T_{\text{warmup}} \\[8pt] \eta_{\min} + \frac{1}{2}(\eta_{\max} - \eta_{\min})\left(1 + \cos\left(\pi \cdot \frac{t - T_{\text{warmup}}}{T_{\text{total}} - T_{\text{warmup}}}\right)\right) & \text{otherwise} \end{cases}$$

| Phase | What Happens | Why |
|---|---|---|
| 🌅 **Warmup** ($t < T_{\text{warmup}}$) | LR linearly ramps from 0 to $\eta_{\max}$ | Prevents gradient explosion at start when Adam's $m$, $v$ aren't warmed up |
| 🌊 **Cosine decay** ($t \geq T_{\text{warmup}}$) | LR smoothly decays following cosine curve | Gradual convergence to minimum |

> 🎒 **Beginner Analogy — Warming Up Before Exercise 🏋️:**
> You don't start running at full sprint — you warm up first! Same with neural networks. The initial random weights produce wild gradients. Warmup lets the optimizer "stabilize" before going full speed.

> 🏭 **Production Recipe (used by OpenAI, Meta, Google):**
> - Warmup: 1-5% of total training steps
> - Peak LR: $3 \times 10^{-4}$ (for most LLMs)
> - Min LR: $3 \times 10^{-5}$ (10% of peak)
> - $\beta_1 = 0.9$, $\beta_2 = 0.95$ (slightly lower $\beta_2$ for stability)

### 🚀 1cycle Policy (Smith & Topin, 2019) — Super-Convergence

**Super-convergence:** LR goes **UP**, then **DOWN**.

$$\eta(t) = \begin{cases} \eta_{\min} + (\eta_{\max} - \eta_{\min}) \cdot \frac{t}{T/2} & \text{if } t \leq T/2 \\[6pt] \eta_{\max} - (\eta_{\max} - \eta_{\min}) \cdot \frac{t - T/2}{T/2} & \text{if } t > T/2 \end{cases}$$

Often trains **10× faster** than standard schedules! 🤯

> 🎒 **Analogy:** It's like revving the engine hard to escape a valley (high LR helps jump over local minima), then gently coasting to the best spot (low LR for precise convergence).

### ⭐ Gradient Clipping Inside the Optimizer 🛡️

Not technically a schedule, but critical for training stability:

$$g_{\text{clipped}} = g \cdot \min\left(1, \frac{\text{max\_norm}}{\|g\|}\right)$$

If the gradient norm exceeds `max_norm`, scale it down. Prevents **gradient explosion** 💥.

```python
# Standard practice for LLM training
torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)
optimizer.step()
```

> 🏭 **Production:** Every LLM training pipeline uses gradient clipping. Without it, a single bad batch can produce enormous gradients that destroy weeks of training.

---

# 7️⃣ Writing a Custom Optimizer — Step by Step 🏗️🔧💻

> 🎒 **Beginner Analogy — Building Your Own Engine 🔧:**
> Using `torch.optim.Adam` is like buying a car from a dealership — it works great, but you can't customize the engine. Writing a **custom optimizer** is like building your own engine from parts. You understand every bolt, every cylinder, and you can modify anything. When a research paper proposes a new optimizer, you can implement it in **30 minutes** while others wait for library support! 🏎️

## ⭐ The Pattern — Every Custom Optimizer Follows This Template

```python
class MyOptimizer(torch.optim.Optimizer):
    def __init__(self, params, lr=0.01, **kwargs):
        defaults = dict(lr=lr, **kwargs)       # Store default hyperparams
        super().__init__(params, defaults)      # Let PyTorch handle param_groups
    
    @torch.no_grad()                            # No gradients during update!
    def step(self, closure=None):
        for group in self.param_groups:         # Iterate parameter groups
            lr = group['lr']
            for p in group['params']:           # Iterate parameters
                if p.grad is None:
                    continue
                grad = p.grad.data
                state = self.state[p]           # Get/create per-param state
                # ... YOUR UPDATE RULE HERE ...
                p.data.add_(update, alpha=-lr)  # Apply update
```

## ⭐ Implementation 1: Custom SGD with Momentum

```python
import torch
from torch.optim import Optimizer


class CustomSGDMomentum(Optimizer):
    """SGD with Momentum — implemented from scratch."""
    
    def __init__(self, params, lr=0.01, momentum=0.9, weight_decay=0.0):
        if lr < 0:
            raise ValueError(f"Invalid learning rate: {lr}")
        if momentum < 0:
            raise ValueError(f"Invalid momentum: {momentum}")
        
        defaults = dict(lr=lr, momentum=momentum, weight_decay=weight_decay)
        super().__init__(params, defaults)
    
    @torch.no_grad()
    def step(self, closure=None):
        """Perform a single optimization step."""
        loss = None
        if closure is not None:
            with torch.enable_grad():
                loss = closure()
        
        for group in self.param_groups:
            lr = group['lr']
            momentum = group['momentum']
            weight_decay = group['weight_decay']
            
            for p in group['params']:
                if p.grad is None:
                    continue
                
                grad = p.grad.data
                
                # Weight decay (L2 regularization)
                if weight_decay != 0:
                    grad = grad + weight_decay * p.data
                
                # Initialize state
                state = self.state[p]
                if len(state) == 0:
                    state['velocity'] = torch.zeros_like(p.data)
                
                v = state['velocity']
                
                # Momentum update: v ← βv + g
                v.mul_(momentum).add_(grad)
                
                # Parameter update: w ← w - α·v
                p.data.add_(v, alpha=-lr)
        
        return loss
```

> 💡 **Code Walkthrough for Beginners:**
> 1. `@torch.no_grad()` — We don't want PyTorch to track gradients of our update operations!
> 2. `self.state[p]` — Each parameter gets its own state dictionary (velocity buffer for momentum)
> 3. `v.mul_(momentum).add_(grad)` — This IS the momentum equation: $v \leftarrow \beta v + g$
> 4. `p.data.add_(v, alpha=-lr)` — This IS the update: $w \leftarrow w - \alpha v$

## ⭐ Implementation 2: Custom Adam (AdamW Style)

```python
class CustomAdam(Optimizer):
    """Adam optimizer — implemented from scratch."""
    
    def __init__(self, params, lr=1e-3, betas=(0.9, 0.999), eps=1e-8,
                 weight_decay=0.0):
        defaults = dict(lr=lr, betas=betas, eps=eps, weight_decay=weight_decay)
        super().__init__(params, defaults)
    
    @torch.no_grad()
    def step(self, closure=None):
        loss = None
        if closure is not None:
            with torch.enable_grad():
                loss = closure()
        
        for group in self.param_groups:
            lr = group['lr']
            beta1, beta2 = group['betas']
            eps = group['eps']
            weight_decay = group['weight_decay']
            
            for p in group['params']:
                if p.grad is None:
                    continue
                
                grad = p.grad.data
                
                # Initialize state
                state = self.state[p]
                if len(state) == 0:
                    state['step'] = 0
                    state['exp_avg'] = torch.zeros_like(p.data)
                    state['exp_avg_sq'] = torch.zeros_like(p.data)
                
                m = state['exp_avg']
                v = state['exp_avg_sq']
                state['step'] += 1
                t = state['step']
                
                # Decoupled weight decay (AdamW style)
                if weight_decay != 0:
                    # w ← (1 - lr·λ)·w   (shrink weights directly)
                    p.data.mul_(1 - lr * weight_decay)
                
                # Update first moment (momentum): m ← β₁·m + (1-β₁)·g
                m.mul_(beta1).add_(grad, alpha=1 - beta1)
                
                # Update second moment (RMSprop): v ← β₂·v + (1-β₂)·g²
                v.mul_(beta2).addcmul_(grad, grad, value=1 - beta2)
                
                # Bias correction: m̂ = m/(1-β₁ᵗ), v̂ = v/(1-β₂ᵗ)
                m_hat = m / (1 - beta1 ** t)
                v_hat = v / (1 - beta2 ** t)
                
                # Update parameters: θ ← θ - η·m̂/(√v̂ + ε)
                p.data.addcdiv_(m_hat, v_hat.sqrt() + eps, value=-lr)
        
        return loss
```

> 💡 **Code Walkthrough for Beginners:**
> 1. **Weight decay** first: shrink weights before the gradient update (AdamW-style decoupling)
> 2. **First moment** ($m$): exponential moving average of gradients → direction memory 🧭
> 3. **Second moment** ($v$): exponential moving average of squared gradients → magnitude memory 📏
> 4. **Bias correction**: divide by $(1 - \beta^t)$ to fix the cold-start bias
> 5. **Update**: $\hat{m} / (\sqrt{\hat{v}} + \epsilon)$ → normalized, adaptive step

## ⭐ Implementation 3: SignSGD — Lion-Inspired, Memory Efficient 🦁

```python
class SignSGD(Optimizer):
    """
    SignSGD with Momentum — inspired by Lion optimizer.
    Uses only the SIGN of the gradient for updates.
    Memory efficient: no second moment storage.
    """
    
    def __init__(self, params, lr=1e-4, momentum=0.9, weight_decay=0.0):
        defaults = dict(lr=lr, momentum=momentum, weight_decay=weight_decay)
        super().__init__(params, defaults)
    
    @torch.no_grad()
    def step(self, closure=None):
        loss = None
        if closure is not None:
            with torch.enable_grad():
                loss = closure()
        
        for group in self.param_groups:
            lr = group['lr']
            momentum = group['momentum']
            weight_decay = group['weight_decay']
            
            for p in group['params']:
                if p.grad is None:
                    continue
                
                grad = p.grad.data
                
                state = self.state[p]
                if len(state) == 0:
                    state['exp_avg'] = torch.zeros_like(p.data)
                
                m = state['exp_avg']
                
                # Update using sign of interpolation between gradient and momentum
                update = m * momentum + grad * (1 - momentum)
                
                # Weight decay (decoupled)
                if weight_decay != 0:
                    p.data.mul_(1 - lr * weight_decay)
                
                # Sign update: only use direction, not magnitude!
                p.data.add_(update.sign(), alpha=-lr)
                
                # Update momentum for next step
                m.mul_(momentum).add_(grad, alpha=1 - momentum)
        
        return loss
```

> 💡 **Why sign() works:**
> The `sign()` function returns only $+1$, $-1$, or $0$ — it throws away gradient **magnitude** and keeps only **direction**. This means:
> - Every parameter gets updated by exactly $\pm \eta$ each step (uniform updates)
> - No need for second moment $v$ → **50% less memory** than Adam!
> - Surprisingly, direction is often more informative than magnitude for training 🤔

| Optimizer | Buffers per Param | Memory Multiplier | Key Operation |
|---|---|---|---|
| 🟢 SGD | 0 (no momentum) or 1 ($v$) | 1-2× | $w - \alpha g$ |
| 🟡 SGD + Momentum | 1 ($v$) | 2× | $w - \alpha v$ |
| 🔴 Adam | 2 ($m$ + $v$) | 3× | $\hat{m}/(\sqrt{\hat{v}} + \epsilon)$ |
| 🟢 SignSGD/Lion | 1 ($m$) | 2× | $\text{sign}(m)$ |

---

# 8️⃣ Testing Custom Optimizers 🧪✅

> 🎒 **Why Test?** When you build your own optimizer, you MUST verify it works correctly! The best test: compare your implementation against PyTorch's official version. If they produce the same results, your code is right! 🎯

```python
import torch
import torch.nn as nn

# ═══════════════════════════════════════════════════════════
# 🧪 Optimizer Comparison Experiment
# ═══════════════════════════════════════════════════════════

torch.manual_seed(42)

# Problem: learn a nonlinear function
X = torch.randn(500, 10)
y = torch.sin(X[:, :3]).sum(dim=1, keepdim=True) + \
    X[:, 3:5].sum(dim=1, keepdim=True) ** 2

# Simple model
def make_model():
    torch.manual_seed(42)
    return nn.Sequential(
        nn.Linear(10, 64),
        nn.ReLU(),
        nn.Linear(64, 32),
        nn.ReLU(),
        nn.Linear(32, 1)
    )

criterion = nn.MSELoss()

# Test each optimizer
optimizers = {
    'PyTorch SGD': lambda m: torch.optim.SGD(m.parameters(), lr=0.01, momentum=0.9),
    'Custom SGD': lambda m: CustomSGDMomentum(m.parameters(), lr=0.01, momentum=0.9),
    'PyTorch Adam': lambda m: torch.optim.Adam(m.parameters(), lr=0.001),
    'Custom Adam': lambda m: CustomAdam(m.parameters(), lr=0.001),
    'SignSGD': lambda m: SignSGD(m.parameters(), lr=0.001, momentum=0.9),
}

print("=" * 70)
print("🏁 OPTIMIZER COMPARISON")
print("=" * 70)

for name, opt_fn in optimizers.items():
    model = make_model()
    optimizer = opt_fn(model)
    
    losses = []
    for epoch in range(500):
        y_pred = model(X)
        loss = criterion(y_pred, y)
        
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
        
        losses.append(loss.item())
    
    print(f"  {name:20s} | Final Loss: {losses[-1]:.6f} | "
          f"Best Loss: {min(losses):.6f}")


# ═══════════════════════════════════════════════════════════
# ✅ Verify Custom Adam matches PyTorch Adam
# ═══════════════════════════════════════════════════════════

print("\n" + "=" * 70)
print("🔍 VERIFICATION: Custom Adam vs PyTorch Adam")
print("=" * 70)

torch.manual_seed(42)
model1 = make_model()
model2 = make_model()

opt1 = torch.optim.Adam(model1.parameters(), lr=0.001)
opt2 = CustomAdam(model2.parameters(), lr=0.001)

for epoch in range(100):
    loss1 = criterion(model1(X), y)
    opt1.zero_grad()
    loss1.backward()
    opt1.step()
    
    loss2 = criterion(model2(X), y)
    opt2.zero_grad()
    loss2.backward()
    opt2.step()

# Compare parameters — should be nearly identical!
max_diff = 0
for p1, p2 in zip(model1.parameters(), model2.parameters()):
    diff = (p1 - p2).abs().max().item()
    max_diff = max(max_diff, diff)

print(f"  Max parameter difference after 100 steps: {max_diff:.2e}")
print(f"  Loss (PyTorch Adam): {loss1.item():.6f}")
print(f"  Loss (Custom Adam):  {loss2.item():.6f}")
print(f"  {'✅ MATCH' if max_diff < 1e-4 else '❌ MISMATCH'}")


# ═══════════════════════════════════════════════════════════
# 🎓 Parameter Groups — Different LR per Layer
# ═══════════════════════════════════════════════════════════

print("\n" + "=" * 70)
print("📊 PARAMETER GROUPS: Differential Learning Rates")
print("=" * 70)

model = make_model()

# Layer-wise learning rates (like fine-tuning a pretrained model!)
optimizer = CustomAdam([
    {'params': model[0].parameters(), 'lr': 1e-4},   # First layer: slow (pretrained)
    {'params': model[2].parameters(), 'lr': 1e-3},   # Second layer: medium
    {'params': model[4].parameters(), 'lr': 1e-2},   # Output layer: fast (new)
])

losses = []
for epoch in range(500):
    loss = criterion(model(X), y)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    losses.append(loss.item())

print(f"  Differential LR — Final Loss: {losses[-1]:.6f}")
```

> 🏭 **Production Scenario — Fine-tuning at Hugging Face 🤗:**
> When fine-tuning a pretrained BERT/LLaMA model, companies use exactly this pattern:
> - **Backbone layers** (pretrained): $\eta = 10^{-5}$ (barely nudge them)
> - **Task head** (new): $\eta = 10^{-3}$ (learn aggressively)
> - **Middle adapters** (LoRA): $\eta = 10^{-4}$ (moderate learning)

---

# 9️⃣ Optimizer Internals — State Inspection 🔬🔍

> 🎒 **Why Inspect Optimizer State?** When training goes wrong (loss spikes, NaN values, slow convergence), the optimizer state often reveals the problem. It's like checking a car's dashboard 🚗📊 — the gauges tell you if something is overheating!

```python
# ═══════════════════════════════════════════════════════════
# 🔬 Inspecting Optimizer State
# ═══════════════════════════════════════════════════════════

print("\n" + "=" * 70)
print("🔬 OPTIMIZER STATE INSPECTION")
print("=" * 70)

model = make_model()
optimizer = CustomAdam(model.parameters(), lr=0.001)

# Train a few steps
for _ in range(10):
    loss = criterion(model(X), y)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

# Inspect state — what's inside Adam's "notebook" for each parameter?
for name, param in model.named_parameters():
    if param in optimizer.state:
        state = optimizer.state[param]
        m = state['exp_avg']
        v = state['exp_avg_sq']
        t = state['step']
        
        print(f"\n  📌 {name}:")
        print(f"    Shape: {list(param.shape)}")
        print(f"    Step: {t}")
        print(f"    First moment  (m) — mean: {m.mean():.6f}, std: {m.std():.6f}")
        print(f"    Second moment (v) — mean: {v.mean():.6f}, std: {v.std():.6f}")
        
        # Effective learning rate per parameter
        m_hat = m / (1 - 0.9 ** t)
        v_hat = v / (1 - 0.999 ** t)
        effective_lr = 0.001 * m_hat.abs() / (v_hat.sqrt() + 1e-8)
        print(f"    Effective LR — mean: {effective_lr.mean():.6f}, "
              f"max: {effective_lr.max():.6f}")
```

> 💡 **What to Look For When Debugging:**
>
> | Symptom | What State Shows | Fix |
> |---|---|---|
> | 💥 **Loss explodes** | $v$ values near zero → effective LR is huge | Add warmup, lower LR, clip gradients |
> | 🐌 **Loss stagnates** | $m$ values near zero → no gradient signal | Check if gradients are flowing (vanishing gradient?) |
> | 🔄 **Loss oscillates** | Large $m$ values with alternating signs | Increase $\beta_1$ (more smoothing) or decrease LR |
> | 🎭 **Different layers converge differently** | Huge variance in effective LR across layers | Use parameter groups with different LR |

---

# 🔟 Deep Reflection Questions (Research Mindset) 🧠💭

> These questions go beyond surface understanding — they're the kind of questions that distinguish a researcher from a practitioner. Try to answer them before reading the hints!

1. ⭐ **Adam's Gradient Normalization Trap:** Adam normalizes by $\sqrt{\hat{v}_t}$, making the update roughly $\pm \eta$ regardless of gradient magnitude. When is this "gradient normalization" harmful? *(Hint: think about the loss landscape near a sharp minimum — in sharp minima, gradient magnitudes carry important information about the loss surface curvature!)*

2. ⭐ **SGD vs Adam Generalization Mystery:** SGD with momentum often generalizes better than Adam on image tasks. The hypothesis is that Adam converges to **SHARPER minima**. Why might sharp minima generalize worse? How does the **SAM (Sharpness-Aware Minimization)** optimizer address this? *(SAM paper: Foret et al., 2021)*

3. ⭐ **Bias Correction Vanishing Point:** The bias correction in Adam divides by $(1 - \beta^t)$. For very large $t$, this factor $\to 1$ and correction vanishes. At what step count $t$ can we safely ignore bias correction? *(Calculate when $(1 - \beta_2^t) > 0.99$ for $\beta_2 = 0.999$)*

4. ⭐ **Lion's Sign Trick:** Lion optimizer uses $\text{sign}(m)$ instead of $m/\sqrt{v}$. This **loses magnitude information**. Why doesn't this hurt performance? *(Think about what aspect of the gradient is most informative — direction vs. magnitude. Also consider: in high dimensions, gradient directions are more stable than magnitudes.)*

5. ⭐ **AdamW vs Adam+L2 — The Math:** Weight decay in AdamW is decoupled from the gradient. Show mathematically why L2 regularization inside Adam (standard) is **NOT equivalent** to weight decay:
   - L2 adds $\lambda\theta$ to gradient → Adam scales this by $1/\sqrt{\hat{v}}$
   - AdamW applies $-\lambda\theta$ directly → NOT scaled by adaptive term
   - Result: different effective regularization per parameter!

6. ⭐ **Memory at Scale:** For a large language model with 70B parameters, Adam requires **560 GB** of optimizer state ($2 \text{ buffers} \times 70B \times 4 \text{ bytes}$). How do different solutions reduce this?
   - **8-bit optimizers**: quantize $m$, $v$ from FP32 → INT8 (4× reduction)
   - **Adafactor**: factorize $v$ as row×column vectors (from $O(mn)$ to $O(m+n)$)
   - **ZeRO Stage 1**: shard across $N$ GPUs ($N$× reduction)
   - **Lion**: eliminate $v$ entirely (2× reduction)

---

# 🔥 Startup Founder Insight 🚀💰

> **Your optimizer choice has equivalent impact to your architecture choice.** A poorly chosen optimizer can **double** your training time. A well-tuned one can **halve** your compute budget.

## ⭐ The Startup Optimizer Playbook

| Scenario | Optimizer Choice | Why |
|---|---|---|
| 🏆 **Default (90% of cases)** | AdamW + warmup + cosine decay | Industry standard, works everywhere |
| 🚛 **Large batch distributed** | LAMB or Adam + linear scaling rule | Maintains accuracy at huge batch sizes |
| 💾 **Memory constrained** | Adafactor, 8-bit Adam, or Lion | Cuts optimizer memory by 50-75% |
| 🔧 **Fine-tuning pretrained** | AdamW + differential LR (low backbone, high head) | Don't destroy pretrained features |
| 🧪 **Novel architectures** | Custom optimizer — you now know how! | When papers propose new update rules |
| ⚡ **Fastest convergence** | 1cycle policy + SGD | Super-convergence for vision tasks |

## 🏭 Real-World Production Recipes

**OpenAI GPT-4 / Meta LLaMA recipe:**
```
Optimizer: AdamW
β₁ = 0.9, β₂ = 0.95 (lower β₂ for stability)
Weight decay: 0.1
Peak LR: 3e-4
Warmup: 2000 steps
Schedule: Cosine decay to 3e-5
Gradient clipping: max_norm = 1.0
```

**Google Gemini / PaLM recipe:**
```
Optimizer: Adafactor (memory efficient)
No second moment storage (factorized)
Schedule: Inverse square root decay
Warmup: 10,000 steps
```

**The engineering advantage**: When a paper proposes a new optimizer, you can implement it in **<30 minutes** by following the pattern above. While others wait for library support, you're already running experiments. 🏎️💨

---

# 📝 Homework (Required) 🏋️‍♂️

1. 🔢 **Adagrad Implementation**: Implement Adagrad from scratch. Show how the effective learning rate $\eta_{\text{eff}} = \frac{\eta}{\sqrt{v_t} + \epsilon}$ decreases over time. Why is this good for sparse gradients?

2. 📈 **Learning Rate Finder**: Implement the LR finder from *"Cyclical Learning Rates"* (Smith, 2017): train for one epoch with exponentially increasing LR, plot loss vs. LR. The optimal LR is where loss decreases fastest (steepest negative slope).

3. 🌅 **Warmup Implementation**: Add warmup support to your CustomAdam: linearly ramp LR from 0 to max over $N$ steps: $\eta_t = \eta_{\max} \cdot \frac{t}{N}$ for $t < N$. Test on a transformer-like model.

4. 🎲 **Gradient Noise Injection**: Add gradient noise injection to your optimizer: $\tilde{g} = g + \mathcal{N}\left(0, \frac{\sigma^2}{(1+t)^2}\right)$. Show it can help escape local minima.

5. 🔄 **Optimizer Switching**: Train for 200 epochs with Adam, then switch to SGD+momentum for 200 more epochs. Compare against Adam-only and SGD-only. This is a real technique used in practice (*"Improving Generalization Performance by Switching from Adam to SGD"*, Keskar & Socher, 2017).

---

# 📋 Comprehensive Summary — Optimizer Cheat Sheet 🗺️

## ⭐ Master Formula Reference

| Formula | LaTeX | What It Does |
|---|---|---|
| SGD | $w \leftarrow w - \alpha g$ | Simplest update — walk downhill |
| Momentum | $v \leftarrow \beta v + g$, $w \leftarrow w - \alpha v$ | Remember past directions |
| Nesterov | $v \leftarrow \beta v + \nabla L(\theta - \alpha \beta v)$ | Look ahead, then correct |
| Adam 1st moment | $m \leftarrow \beta_1 m + (1-\beta_1)g$ | Exponential moving avg of gradients |
| Adam 2nd moment | $v \leftarrow \beta_2 v + (1-\beta_2)g^2$ | Exponential moving avg of squared gradients |
| Adam bias correction | $\hat{m} = \frac{m}{1-\beta_1^t}$, $\hat{v} = \frac{v}{1-\beta_2^t}$ | Fix cold-start bias |
| Adam update | $w \leftarrow w - \alpha \frac{\hat{m}}{\sqrt{\hat{v}}+\epsilon}$ | Adaptive per-parameter step |
| AdamW | $w \leftarrow (1-\lambda\alpha)w - \alpha\frac{\hat{m}}{\sqrt{\hat{v}}+\epsilon}$ | Decoupled weight decay |
| Lion | $w \leftarrow w - \alpha(\text{sign}(m) + \lambda w)$ | Sign-based, memory efficient |
| Cosine schedule | $\eta = \eta_{\min} + \frac{1}{2}(\eta_{\max}-\eta_{\min})(1+\cos(\frac{\pi t}{T}))$ | Smooth LR decay |
| Warmup | $\eta = \eta_{\max} \cdot \frac{t}{T_{\text{warmup}}}$ | Linear LR ramp-up |
| Gradient clipping | $g_{\text{clip}} = g \cdot \min(1, \frac{c}{\|g\|})$ | Prevent gradient explosion |

## ⭐ Optimizer Comparison Table

| Optimizer | Memory per Param | Adaptive LR? | Best For | Default Hyperparams |
|---|---|---|---|---|
| 🟢 **SGD** | 0 extra | ❌ | Simple tasks, baselines | $\alpha = 0.01$ |
| 🟡 **SGD + Momentum** | +1 buffer | ❌ | CNNs, vision tasks | $\alpha = 0.01, \beta = 0.9$ |
| 🔵 **RMSprop** | +1 buffer | ✅ | RNNs | $\alpha = 0.001, \beta = 0.99$ |
| 🔴 **Adam** | +2 buffers | ✅ | General purpose | $\alpha = 0.001, \beta_1 = 0.9, \beta_2 = 0.999$ |
| 🟣 **AdamW** | +2 buffers | ✅ | **LLMs, Transformers** 🏆 | $\alpha = 3 \times 10^{-4}, \lambda = 0.1$ |
| 🟡 **Adafactor** | +1 buffer (factored) | ✅ | Very large models | Factorized second moment |
| 🦁 **Lion** | +1 buffer | ❌ (sign only) | Memory-limited training | $\alpha = 10^{-4}, \beta_1 = 0.9$ |
| 🧠 **Sophia** | +1 buffer (Hessian) | ✅ (2nd order) | LLM pre-training (2× faster) | Diagonal Hessian estimator |
| 🚛 **LAMB** | +2 buffers | ✅ + layer-wise | Large batch distributed | Trust ratio per layer |

## ⭐ LR Schedule Comparison

| Schedule | Formula | When to Use | Used By |
|---|---|---|---|
| 📉 Step decay | $\eta \cdot \gamma^{\lfloor t/N \rfloor}$ | Simple, predictable | ResNet papers |
| 📉 Exponential | $\eta \cdot \gamma^t$ | Smooth continuous decay | Classic training |
| 🌊 Cosine | $\frac{1}{2}(\eta_{\max}-\eta_{\min})(1+\cos(\frac{\pi t}{T}))$ | **Most popular** | GPT-4, LLaMA, Gemini |
| 🌅 Warmup + cosine | Linear ramp → cosine decay | **LLM standard** 🏆 | All modern LLMs |
| 🚀 1cycle | Up then down (triangle) | Super-convergence | FastAI, vision |
| 🎉 Schedule-free | No schedule needed! | Cutting-edge research | Defazio et al., 2024 |

## ⭐ Memory Budget at Scale

| Component | 7B Model (FP32) | 70B Model (FP32) |
|---|---|---|
| 📦 Model weights | 28 GB | 280 GB |
| 📊 Gradients | 28 GB | 280 GB |
| 🧠 Adam state ($m + v$) | 56 GB | 560 GB |
| 💾 **Total** | **112 GB** | **1,120 GB** |
| 🦁 With Lion (no $v$) | 84 GB | 840 GB |
| 🔢 With 8-bit Adam | 42 GB | 420 GB |
| 🌐 With ZeRO-1 (8 GPUs) | 14 GB/GPU | 140 GB/GPU |

## ⭐ Research Paper Quick Reference 📚

| Paper | Year | Key Contribution |
|---|---|---|
| SGD with Momentum | Polyak, 1964 | Velocity-based acceleration |
| Adagrad | Duchi et al., 2011 | Per-parameter adaptive LR |
| RMSprop | Hinton, 2012 | Leaky Adagrad (fixes LR decay) |
| **Adam** | Kingma & Ba, 2015 | Momentum + adaptive LR + bias correction |
| Cyclical LR | Smith, 2017 | LR finder + 1cycle policy |
| **AdamW** | Loshchilov & Hutter, 2019 | Decoupled weight decay |
| LAMB | You et al., 2020 | Large-batch training (BERT in 76 min) |
| SAM | Foret et al., 2021 | Sharpness-aware → better generalization |
| **μP** | Yang et al., 2022 | Transfer hyperparams across model sizes |
| 8-bit Adam | Dettmers et al., 2022 | Quantized optimizer state |
| **Sophia** | Liu et al., 2023 | 2nd-order optimizer for LLMs |
| **Lion** | Chen et al., 2024 | Sign-based, found by AI |
| **Schedule-Free** | Defazio et al., 2024 | Removes need for LR scheduling |

## ⭐ Decision Flowchart 🗺️

```
Start → What are you training?
  │
  ├─ 🖼️ CNN/Vision → SGD + Momentum (0.9) + Step LR
  │
  ├─ 🤖 Transformer/LLM → AdamW + Warmup + Cosine
  │    ├─ Memory OK? → Standard AdamW
  │    ├─ Memory tight? → 8-bit Adam or Lion
  │    └─ Multi-GPU? → AdamW + ZeRO Stage 1
  │
  ├─ 🔄 Fine-tuning → AdamW + Differential LR
  │    ├─ Backbone: 1e-5
  │    ├─ Adapters: 1e-4
  │    └─ Task head: 1e-3
  │
  ├─ 🚛 Large batch (>8K) → LAMB
  │
  └─ 🧪 Research/Novel → Custom optimizer (you know how!)
```

---

> 🎓 **Final Takeaway:** The optimizer is the **engine** of your training pipeline 🏎️. Understanding it deeply — not just calling `Adam()` — gives you the power to debug training failures, implement cutting-edge research, and make informed engineering decisions that save your company millions in compute costs. You now have the tools to build ANY optimizer from scratch. Use this power wisely! 💪🔥
