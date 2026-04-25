📘 DAY 5 — Momentum — Accelerating Gradient Descent 🚀⚡

> *"Momentum is the bowling ball of optimization — once it gets rolling, nothing stops it!"* 🎳

## 🎯 Goal

Understand momentum not as "a faster optimizer," but as a **physical simulation** that fundamentally changes how optimization navigates loss landscapes. Learn why vanilla SGD oscillates in narrow valleys, how exponential moving averages smooth the trajectory, how momentum escapes saddle points, and why this single idea — **carrying velocity through parameter space** — is embedded in every modern optimizer from SGD+Momentum to Adam.

### ✅ If this is deeply understood:
You understand why modern optimizers converge faster, why Nesterov momentum outperforms classical momentum, and how these ideas form the foundation of Adam (Day 22). 🧠

---

# 1️⃣ The Problem with Vanilla SGD — Oscillation in Narrow Valleys 🌊😵

> 🎯 **Beginner Analogy:** Imagine you're walking down a very narrow, twisting valley between two mountains. Every step you take, you bump into one wall, then the other wall. You're zigzagging back and forth instead of just walking straight down the valley floor. That's what vanilla SGD does! 🏔️➡️🏔️➡️🏔️

Consider a loss surface that is:
- 📐 Very steep in one direction (large eigenvalue of Hessian)
- 📏 Very shallow in another direction (small eigenvalue of Hessian)

This creates **elongated elliptical contours** — common in practice when features have different scales or curvature varies across dimensions.

### 😰 What Goes Wrong with Vanilla SGD

Vanilla SGD with constant learning rate:
- ⬆️⬇️ Takes large steps in the steep direction (overshoots and bounces back)
- 🐌 Takes tiny steps in the shallow direction (makes slow progress)
- 💥 Result: **ZIG-ZAG trajectory** that wastes most of its movement on oscillations

⭐ **The condition number** $\kappa = \frac{\lambda_{\max}}{\lambda_{\min}}$ determines how bad the oscillation is.

For $\kappa = 100$: SGD wastes **~99%** of its movement on oscillating! 😱

| Condition Number ($\kappa$) | Oscillation Severity | Wasted Movement |
|:---:|:---:|:---:|
| 1 | None (circular contours) ✅ | 0% |
| 10 | Mild 😐 | ~90% |
| 100 | Severe 😰 | ~99% |
| 1000 | Catastrophic 🤯 | ~99.9% |

⭐ **Momentum smooths out the oscillations and accelerates along consistent gradient directions.** Think of switching from walking (zigzag) to ice skating (smooth glide)! ⛸️

---

# 2️⃣ Exponential Moving Average (EMA) — The Smoothing Tool 📊🧹

> 🎯 **Beginner Analogy:** Imagine calculating your grade in a class. Instead of only looking at your LAST test score (which might be a fluke), you average your recent scores — but your most recent scores count MORE than older ones. That's EMA! Your grade from 3 weeks ago matters, but yesterday's quiz matters WAY more. 📝

Before understanding momentum, understand EMA — it appears **everywhere** in ML.

Given a sequence of values $g_1, g_2, g_3, \ldots$:

$$v_t = \beta \cdot v_{t-1} + (1 - \beta) \cdot g_t$$

where $\beta \in [0, 1)$ is the decay factor.

## 🔍 Unrolling the recurrence:

$$v_t = (1-\beta) g_t + \beta(1-\beta) g_{t-1} + \beta^2(1-\beta) g_{t-2} + \ldots$$

This is a **weighted average** of all past values, with **EXPONENTIALLY DECAYING** weights. 📉

✨ Recent values matter more. Ancient values are nearly forgotten.

## 🪟 Effective window size

⭐ The effective number of values averaged:

$$\text{window} \approx \frac{1}{1 - \beta}$$

| $\beta$ Value | Effective Window | What It Means 🧠 |
|:---:|:---:|:---|
| 0.5 | ~2 values | Almost no memory — like only remembering yesterday 🧓 |
| 0.9 | ~10 values | Short-term memory — like remembering this week 📅 |
| 0.99 | ~100 values | Medium memory — like remembering this semester 📚 |
| 0.999 | ~1000 values | Long memory — like remembering several years 🗓️ |

## 🌐 Where EMA Appears in Deep Learning

| # | Application | What Gets Averaged | Why It Helps |
|:---:|:---|:---|:---|
| 1 | 🏃 **Momentum** | Gradients → velocity | Smooths optimization path |
| 2 | 🧮 **Adam (RMSProp part)** | Squared gradients → adaptive LR | Per-parameter learning rates |
| 3 | 📊 **Batch Normalization** | Mean/variance during training | Stable inference statistics |
| 4 | ⚖️ **Model EMA (Polyak averaging)** | Model weights | Better final model |
| 5 | 🎮 **Target networks (DQN)** | Policy network weights | Stable training targets |
| 6 | 🤖 **EMA in transformers** | Attention weights | Recent research frontier |

⭐ **Understanding EMA = understanding the smoothing backbone of modern ML.** 🏗️

---

# 3️⃣ Classical Momentum — The Heavy Ball Method 🎳💨

> 🎯 **Beginner Analogy:** Imagine a bowling ball rolling down a bumpy hill. Even when the hill has little bumps and dips, the ball keeps rolling forward because it's HEAVY and has built up speed. A ping pong ball would bounce everywhere (that's vanilla SGD). The bowling ball smoothly rolls past the bumps and heads straight to the bottom! 🎳⬇️

⭐ Introduced by **Polyak (1964)** — the "Heavy Ball Method." Think of a ball rolling on the loss surface.

### 📖 Deep Research: Polyak's Original Insight (1964)
> Boris Polyak realized that optimization could borrow from Newtonian physics. A particle moving through a potential field doesn't just respond to the local force — it has **inertia**. This simple physical analogy led to one of the most important ideas in optimization history. The method is mathematically equivalent to a **damped oscillator** (spring-mass system) — the same physics that governs shock absorbers in your car! 🚗

The update has **TWO components**:
1. 📐 The **gradient** tells us the current slope
2. 🏃 The **velocity** carries the ball's history

## ⚙️ Algorithm

Initialize: $v_0 = 0$

At each step:

$$v_t = \beta v_{t-1} + \nabla L(w_t) \quad \text{[update velocity]}$$

$$w_{t+1} = w_t - \alpha v_t \quad \text{[update weights]}$$

Alternative (equivalent) formulation:

$$v_t = \beta v_{t-1} + \alpha \nabla L(w_t)$$
$$w_{t+1} = w_t - v_t$$

Where:
| Symbol | Meaning | Typical Value |
|:---:|:---|:---:|
| $\alpha$ | Learning rate (step size) | 0.01 – 0.1 |
| $\beta$ | Momentum coefficient ("heaviness" of the ball 🎳) | **0.9** |
| $v$ | Velocity (accumulated gradient direction) | Starts at 0 |

## 🧠 Why It Works — Physical Intuition

> 🎯 **Beginner Analogy:** Think of $\beta$ as how "heavy" your ball is:
> - $\beta = 0.9$ → Heavy bowling ball 🎳 (keeps rolling, hard to stop)
> - $\beta = 0.5$ → Light tennis ball 🎾 (easily changes direction)
> - $\beta = 0.99$ → Massive boulder 🪨 (VERY hard to change direction — sometimes too much!)

Imagine a ball rolling in a bowl:
- 💨 Ball **accumulates speed** going downhill
- 🏃 The ball's **inertia** carries it through flat regions and small bumps
- ⚖️ In **oscillating directions**, velocity averages out (positive and negative gradients cancel)
- 🚀 In **consistent directions**, velocity builds up (gradients reinforce each other)

⭐ After $k$ steps in a consistent direction:

$$\text{effective step size} = \frac{\alpha}{1 - \beta}$$

For $\beta = 0.9$: effective step is **10× the learning rate** in the gradient's direction! 🔥

In oscillating directions:
Velocity oscillates but with amplitude **reduced by factor** $(1-\beta)$.
For $\beta = 0.9$: oscillation amplitude is only **10%** of vanilla SGD. 📉

## 📈 Convergence Rate

For quadratic with condition number $\kappa$:

| Method | Convergence Rate (per step) | Steps for $\kappa = 100$ |
|:---|:---:|:---:|
| 🐢 Vanilla SGD | $\frac{\kappa - 1}{\kappa + 1}$ | ~500 steps |
| 🚀 With Momentum | $\frac{\sqrt{\kappa} - 1}{\sqrt{\kappa} + 1}$ | ~50 steps |

If $\kappa = 100$:
- SGD: $\frac{99}{101} = 0.98$ → need ~500 steps 🐢
- Momentum: $\frac{9}{11} \approx 0.82$ → need ~50 steps 🚀

⭐ **10× speedup** for ill-conditioned problems! Even better for larger $\kappa$. 🎉

Optimal momentum:

$$\beta^* = \left(\frac{\sqrt{\kappa} - 1}{\sqrt{\kappa} + 1}\right)^2$$

For $\kappa = 100$: $\beta^* \approx 0.67$. For $\kappa = 10000$: $\beta^* \approx 0.96$.

In practice, $\beta = 0.9$ is a **robust default**. ✅

### 🔬 Deep Research: Connection to PID Control
> Momentum is equivalent to the **derivative term (D)** in PID control theory from engineering! In PID controllers (used in thermostats, cruise control, drones), the D-term predicts future error by looking at how fast the error is changing. Similarly, momentum predicts where the optimizer should go based on the **trend** of past gradients. Engineers have been using this idea for decades in completely different contexts! 🎛️

---

# 4️⃣ Nesterov Accelerated Gradient (NAG) — Look Before You Leap 🔭👀

> 🎯 **Beginner Analogy:** Imagine you're driving a car with momentum. Classical momentum is like driving forward and THEN checking the map 🗺️. Nesterov is like checking the map from WHERE YOU'RE HEADING FIRST, so you can adjust your steering BEFORE you get there! It's like a chess player thinking one move ahead ♟️.

- **Classical momentum:** Compute gradient at current position, then move.
- **Nesterov (1983):** First make a tentative step in the velocity direction, THEN compute gradient at the new position.

### 📖 Deep Research: Nesterov's Breakthrough (1983)
> Yurii Nesterov proved something remarkable: his "accelerated gradient method" achieves the **theoretically optimal convergence rate** for first-order optimization methods. This means no matter how clever your algorithm is, if it only uses gradient information, it literally CANNOT beat Nesterov's method. This result shook the optimization community! 🏆

## ⚙️ Algorithm

$$v_t = \beta v_{t-1} + \nabla L(w_t - \alpha \beta v_{t-1})$$

$$w_{t+1} = w_t - \alpha v_t$$

Equivalent reformulation (as implemented in practice):

$$v_t = \beta v_{t-1} + \alpha \nabla L(w_t + \beta v_{t-1})$$
$$w_{t+1} = w_t - v_t$$

⭐ **The key insight:** Evaluate the gradient at the **"lookahead" position**. 🔭

## 🤔 Why NAG Is Better

Consider approaching the minimum:

| Scenario | Classical Momentum 🎳 | Nesterov Momentum 🔭 |
|:---|:---|:---|
| Approaching minimum | Ball has built up velocity... | "Look ahead" to where momentum takes us... |
| What happens? | ...overshoots the minimum! 😰 | ...compute gradient THERE ✅ |
| Correction | Too late — already overshot | If we've overshot, lookahead gradient points us back 🎯 |
| Result | Oscillation near minimum | Earlier correction, less oscillation! 🎉 |

NAG **"corrects"** the momentum BEFORE it causes overshooting. 🛡️

## 📊 Convergence Rate

For smooth convex functions:

| Method | Convergence Rate | Optimal? |
|:---|:---:|:---:|
| 🐢 Gradient Descent | $O(1/t)$ | No |
| 🏃 GD with Momentum | $O(1/t^2)$ — but not always achieved | No |
| 🚀 **NAG** | $O(1/t^2)$ | **YES! Provably optimal!** ⭐ |

⭐ Nesterov proved this is the **BEST possible rate** for any algorithm that only uses gradient information. You literally **cannot do better.** 🏆

This is why NAG is called an **"accelerated"** method. 🚀

### 📖 Deep Research: Sutskever et al. (2013)
> Ilya Sutskever (later co-founder of OpenAI!) and colleagues showed how to properly implement Nesterov momentum for deep learning in practice. Their reformulation made it easy to plug into existing SGD code, which is why it's now a simple `nesterov=True` flag in PyTorch! 🔧

---

# 5️⃣ Momentum and Saddle Points — Escaping Flat Regions 🏔️💨

> 🎯 **Beginner Analogy:** Imagine a skateboard at the top of a hill that flattens out into a plateau (like a tabletop mountain). Without momentum, the skateboard stops on the flat part and gets stuck. WITH momentum, the skateboard's speed carries it ACROSS the flat section and down the other side! 🛹💨

Neural network loss surfaces have many **saddle points** — places where the gradient is zero but it's NOT a minimum (some directions curve up, others curve down).

## ⚠️ Why Saddle Points Are a Problem

At a saddle point: $\nabla L \approx 0$.

Vanilla SGD with a perfect gradient estimate would **STOP here.** 🛑
The gradient provides no information about which direction to escape.

> 💡 **Fun Fact:** In high-dimensional spaces (like neural networks with millions of parameters), saddle points are FAR more common than local minima! A random critical point in 1,000,000 dimensions is almost certainly a saddle point, not a minimum. 🤯

## 🚀 How Momentum Helps

Even at a saddle point ($\nabla L = 0$):

$$v_t = \beta v_{t-1} + 0 = \beta v_{t-1}$$

⭐ **The velocity is NOT zero!** Past gradients still push the optimizer forward. 💪
Momentum carries the ball **THROUGH** the saddle point.

After several steps near a saddle:
- 📡 If there's a downhill direction, tiny gradient components accumulate
- 🔊 Momentum **amplifies** small gradient signals over time
- 🏃 Eventually escapes the saddle region

## 🔬 Empirical Evidence

⭐ **Sutskever et al. (2013)** showed that momentum is **CRITICAL** for training deep networks.

| Without Momentum 😰 | With Momentum 🚀 |
|:---|:---|
| Networks get stuck at saddle points | Same networks converge to good solutions |
| Training stalls on plateau regions | Momentum carries through plateaus |
| Poor final accuracy | Consistently better accuracy |

---

# 6️⃣ Momentum in Practice — How PyTorch Does It 🏭🔧

> 🎯 **Production Insight:** SGD with momentum ($\beta = 0.9$) is the **gold standard** for training computer vision models. ResNet, EfficientNet, and most ImageNet models use it. It's battle-tested at scale by Google, Meta, and every major AI lab! 🏢

PyTorch's SGD with momentum uses a slightly different formulation:

$$v_t = \beta v_{t-1} + \nabla L(w_t) \quad \text{[Note: no } (1-\beta) \text{ factor]}$$
$$w_{t+1} = w_t - \alpha v_t$$

This differs from the EMA formulation by a constant factor.
The effective learning rate is scaled by $\frac{1}{1-\beta}$ compared to the EMA version.
Just adjust $\alpha$ accordingly. 🔧

With Nesterov:

$$v_t = \beta v_{t-1} + \nabla L(w_t)$$
$$w_{t+1} = w_t - \alpha(\beta v_t + \nabla L(w_t))$$

### 🎛️ Key Hyperparameters

| Parameter | Symbol | Range | Default | Notes |
|:---|:---:|:---:|:---:|:---|
| Learning rate | $\alpha$ | 0.01 – 0.1 | 0.01 | Lower for momentum than vanilla SGD |
| Momentum | $\beta$ | 0.8 – 0.99 | **0.9** ⭐ | Works for 90%+ of problems |
| Nesterov | — | True/False | **True** ✅ | Almost always better |

```python
# PyTorch equivalent — this is what you use in production! 🏭
optimizer = torch.optim.SGD(model.parameters(), lr=0.01, momentum=0.9, nesterov=True)
```

### 🏭 Production Scenarios

| Scenario | Optimizer | Why? |
|:---|:---|:---|
| 🖼️ ResNet on ImageNet | SGD + Momentum ($\beta=0.9$) | Industry standard, best generalization |
| 🤖 BERT/GPT training | Adam (has momentum built-in) | Adaptive LR + momentum combined |
| 🎨 Stable Diffusion | SGD + Momentum + EMA weights | EMA for smooth generation quality |
| 📊 When momentum fails | Very noisy gradients | Too much inertia carries you past minimum 😵 |

### 🔬 Deep Research: Why $\beta = 0.9$?
> Empirically, $\beta = 0.9$ is near-optimal for most loss landscapes encountered in deep learning. It averages over ~10 recent gradients — long enough to smooth noise, short enough to react to landscape changes. Think of it as the "Goldilocks zone" 🐻: not too heavy, not too light, juuust right!

---

# 7️⃣ Momentum's Role in Modern Optimizers 🧩🔗

> 🎯 **Big Picture:** Momentum isn't just a standalone trick — it's a **KEY COMPONENT** of Adam!

⭐ **Adam = Momentum (first moment) + RMSProp (second moment)** 🤝

Specifically:
- 🏃 **First moment** ($m_t$): EMA of gradients = **MOMENTUM**
- 📏 **Second moment** ($v_t$): EMA of squared gradients = **ADAPTIVE LEARNING RATE**

Understanding momentum is **prerequisite** for understanding Adam (Day 22). 🎓

## 📊 Relationship to Other Methods

| Method | First Moment (momentum) | Second Moment (adaptation) | Used For |
|:---|:---:|:---:|:---|
| 🐢 SGD | ❌ No | ❌ No | Baseline |
| 🏃 SGD + Momentum | ✅ Yes ($\beta$) | ❌ No | Vision models (ResNet) |
| 📏 RMSProp | ❌ No | ✅ Yes | RNNs, some RL |
| 🚀 **Adam** | ✅ Yes ($\beta_1$) | ✅ Yes ($\beta_2$) | Most deep learning |
| 💪 AdamW | ✅ Yes ($\beta_1$) | ✅ Yes ($\beta_2$) + decoupled WD | Transformers, LLMs |
| 🦣 LAMB | ✅ Yes ($\beta_1$) | ✅ Yes ($\beta_2$) + layer-wise | Large batch training |

---

# 8️⃣ Advanced: Polyak Averaging — EMA of Weights ⚖️✨

> 🎯 **Beginner Analogy:** Imagine a photographer taking photos of a moving target. Each individual photo might be slightly blurry (the target moved!). But if you AVERAGE all the photos together, the blurriness cancels out and you get a sharp image! That's what Polyak averaging does with model weights. 📸

Instead of using the LAST iterate $w_T$, use the **AVERAGE** of all iterates:

$$w_{\text{avg}} = \frac{1}{T} \sum_{t=1}^{T} w_t$$

Or better, **exponential moving average** of weights:

$$w_{\text{ema},t} = \alpha_{\text{ema}} \cdot w_{\text{ema},t-1} + (1 - \alpha_{\text{ema}}) \cdot w_t$$

with $\alpha_{\text{ema}} = 0.999$ or $0.9999$.

## 🧠 Why This Works

Individual iterates **oscillate** around the optimum.
The average cancels out oscillations and converges **FASTER** than any individual iterate. 📈

For convex problems:

| Method | Convergence Rate | Improvement |
|:---|:---:|:---|
| Last iterate | $O(1/\sqrt{T})$ | Baseline |
| ⭐ **Averaged iterate** | $O(1/T)$ | **Quadratically better!** 🚀 |

## 🏭 In Practice

⭐ **Stochastic Weight Averaging (SWA):** Average weights from the last few epochs.
Often improves test accuracy by **0.5–1.5%** with **zero extra training cost**. FREE performance! 🎁

EMA models are used in:

| Application | How EMA is Used | Impact |
|:---|:---|:---|
| 🎨 **Stable Diffusion** | EMA of diffusion model weights for generation | Smoother, higher-quality images |
| 🎓 **Semi-supervised learning** | Teacher model is EMA of student | Stable pseudo-labels |
| 📚 **Knowledge distillation** | EMA teacher provides stable targets | Better student training |

---

# 9️⃣ Implementation — Momentum from Scratch 💻🔨

> 🎯 **Hands-on time!** Let's build everything from scratch to truly understand how momentum works under the hood. 🛠️

## 🧪 Task 1: SGD vs Momentum vs Nesterov

```python
import numpy as np
import matplotlib.pyplot as plt

def sgd_vanilla(grad_fn, w0, lr, n_iters):
    """Vanilla SGD."""
    w = w0.copy()
    path = [w.copy()]
    for _ in range(n_iters):
        g = grad_fn(w)
        w = w - lr * g
        path.append(w.copy())
    return np.array(path)

def sgd_momentum(grad_fn, w0, lr, beta, n_iters):
    """SGD with classical momentum."""
    w = w0.copy()
    v = np.zeros_like(w)
    path = [w.copy()]
    for _ in range(n_iters):
        g = grad_fn(w)
        v = beta * v + g
        w = w - lr * v
        path.append(w.copy())
    return np.array(path)

def sgd_nesterov(grad_fn, w0, lr, beta, n_iters):
    """SGD with Nesterov momentum."""
    w = w0.copy()
    v = np.zeros_like(w)
    path = [w.copy()]
    for _ in range(n_iters):
        # Lookahead position
        w_look = w - lr * beta * v
        g = grad_fn(w_look)
        v = beta * v + g
        w = w - lr * v
        path.append(w.copy())
    return np.array(path)

# Ill-conditioned quadratic: L(w) = 0.5 * w^T A w
# Eigenvalues differ by 100x
A = np.array([[100, 0],
              [0, 1]])

def loss_fn(w):
    return 0.5 * w @ A @ w

def grad_fn(w):
    return A @ w

w0 = np.array([1.0, 10.0])
n_iters = 100

path_sgd = sgd_vanilla(grad_fn, w0, lr=0.015, n_iters=n_iters)
path_mom = sgd_momentum(grad_fn, w0, lr=0.015, beta=0.9, n_iters=n_iters)
path_nag = sgd_nesterov(grad_fn, w0, lr=0.015, beta=0.9, n_iters=n_iters)

# Visualize paths on loss surface
w0r = np.linspace(-1.5, 1.5, 200)
w1r = np.linspace(-12, 12, 200)
W0, W1 = np.meshgrid(w0r, w1r)
L = 0.5 * (100 * W0**2 + W1**2)

fig, axes = plt.subplots(1, 3, figsize=(16, 5))
titles = ['Vanilla SGD (zig-zags)', 'Momentum (smoother)', 'Nesterov (fastest)']
paths = [path_sgd, path_mom, path_nag]
colors = ['red', 'blue', 'green']

for ax, path, title, color in zip(axes, paths, titles, colors):
    ax.contour(W0, W1, L, levels=np.logspace(-1, 3, 20), cmap='viridis', alpha=0.6)
    ax.plot(path[:, 0], path[:, 1], color=color, marker='.', markersize=2, linewidth=0.8, alpha=0.7)
    ax.plot(0, 0, 'k*', markersize=15)
    ax.plot(w0[0], w0[1], 'gs', markersize=10)
    ax.set_title(title)
    ax.set_xlabel('w0 (high curvature)')
    ax.set_ylabel('w1 (low curvature)')
    ax.set_xlim(-1.5, 1.5)
    ax.set_ylim(-12, 12)

plt.tight_layout()
plt.show()

# Compare loss convergence
losses_sgd = [loss_fn(w) for w in path_sgd]
losses_mom = [loss_fn(w) for w in path_mom]
losses_nag = [loss_fn(w) for w in path_nag]

plt.figure(figsize=(8, 5))
plt.semilogy(losses_sgd, label='Vanilla SGD', linewidth=2)
plt.semilogy(losses_mom, label='Momentum (β=0.9)', linewidth=2)
plt.semilogy(losses_nag, label='Nesterov (β=0.9)', linewidth=2)
plt.xlabel('Iteration')
plt.ylabel('Loss (log scale)')
plt.title(f'Convergence: Condition Number κ = {100}')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

## 🧪 Task 2: Momentum on Linear Regression with SGD

```python
def sgd_momentum_regression(X, y, lr=0.01, beta=0.9, n_epochs=50, batch_size=32):
    """Mini-batch SGD with momentum for linear regression."""
    N, d = X.shape
    w = np.zeros(d)
    v = np.zeros(d)
    losses = []
    
    for epoch in range(n_epochs):
        loss = np.mean((X @ w - y)**2)
        losses.append(loss)
        
        indices = np.random.permutation(N)
        for start in range(0, N, batch_size):
            end = min(start + batch_size, N)
            X_b = X[indices[start:end]]
            y_b = y[indices[start:end]]
            B = end - start
            
            grad = (2.0/B) * X_b.T @ (X_b @ w - y_b)
            v = beta * v + grad
            w = w - lr * v
    
    return w, losses

# Generate data
np.random.seed(42)
N = 1000
d = 10
X = np.random.randn(N, d)
w_true = np.random.randn(d)
y = X @ w_true + 0.3 * np.random.randn(N)
X = np.column_stack([np.ones(N), X])

# Compare SGD, Momentum, Nesterov
def sgd_plain_regression(X, y, lr=0.01, n_epochs=50, batch_size=32):
    N, d = X.shape
    w = np.zeros(d)
    losses = []
    for epoch in range(n_epochs):
        loss = np.mean((X @ w - y)**2)
        losses.append(loss)
        indices = np.random.permutation(N)
        for start in range(0, N, batch_size):
            end = min(start + batch_size, N)
            X_b = X[indices[start:end]]
            y_b = y[indices[start:end]]
            B = end - start
            grad = (2.0/B) * X_b.T @ (X_b @ w - y_b)
            w = w - lr * grad
    return w, losses

_, losses_plain = sgd_plain_regression(X, y, lr=0.005, n_epochs=80, batch_size=32)
_, losses_mom = sgd_momentum_regression(X, y, lr=0.002, beta=0.9, n_epochs=80, batch_size=32)
_, losses_mom99 = sgd_momentum_regression(X, y, lr=0.001, beta=0.99, n_epochs=80, batch_size=32)

plt.figure(figsize=(10, 6))
plt.plot(losses_plain, label='Vanilla SGD', linewidth=2)
plt.plot(losses_mom, label='Momentum β=0.9', linewidth=2)
plt.plot(losses_mom99, label='Momentum β=0.99', linewidth=2)
plt.xlabel('Epoch')
plt.ylabel('MSE Loss')
plt.title('SGD vs Momentum on Linear Regression')
plt.legend()
plt.yscale('log')
plt.grid(True, alpha=0.3)
plt.show()
```

## 🧪 Task 3: EMA of Weights (Polyak Averaging) ⚖️

```python
def sgd_with_ema(X, y, lr=0.01, beta=0.9, ema_decay=0.999, n_epochs=100, batch_size=32):
    """SGD with momentum + EMA of weights."""
    N, d = X.shape
    w = np.zeros(d)
    w_ema = np.zeros(d)
    v = np.zeros(d)
    losses_current = []
    losses_ema = []
    
    step = 0
    for epoch in range(n_epochs):
        loss_curr = np.mean((X @ w - y)**2)
        loss_ema = np.mean((X @ w_ema - y)**2)
        losses_current.append(loss_curr)
        losses_ema.append(loss_ema)
        
        indices = np.random.permutation(N)
        for start in range(0, N, batch_size):
            end = min(start + batch_size, N)
            X_b = X[indices[start:end]]
            y_b = y[indices[start:end]]
            B = end - start
            
            grad = (2.0/B) * X_b.T @ (X_b @ w - y_b)
            v = beta * v + grad
            w = w - lr * v
            
            # Update EMA of weights
            w_ema = ema_decay * w_ema + (1 - ema_decay) * w
            step += 1
    
    return w, w_ema, losses_current, losses_ema

w_final, w_ema, losses_w, losses_ema = sgd_with_ema(
    X, y, lr=0.002, beta=0.9, ema_decay=0.999, n_epochs=100, batch_size=32
)

plt.figure(figsize=(10, 5))
plt.plot(losses_w, label='Current weights', alpha=0.7)
plt.plot(losses_ema, label='EMA weights (smoother)')
plt.xlabel('Epoch')
plt.ylabel('MSE Loss')
plt.title('Weight EMA: Smoother Convergence')
plt.legend()
plt.yscale('log')
plt.grid(True, alpha=0.3)
plt.show()

print(f"Final loss (current):  {losses_w[-1]:.6f}")
print(f"Final loss (EMA):      {losses_ema[-1]:.6f}")
```

## 🧪 Task 4: Saddle Point Escape Demonstration 🏔️💨

```python
def loss_with_saddle(w):
    """Loss function with a saddle point at origin.
    L(w1, w2) = w1^2 - w2^2 + 0.1*(w1^2 + w2^2)^2
    Saddle at (0,0), minima along w2 axis.
    """
    return w[0]**2 - w[1]**2 + 0.1*(w[0]**2 + w[1]**2)**2

def grad_saddle(w):
    """Gradient of saddle loss."""
    g0 = 2*w[0] + 0.4*w[0]*(w[0]**2 + w[1]**2)
    g1 = -2*w[1] + 0.4*w[1]*(w[0]**2 + w[1]**2)
    return np.array([g0, g1])

# Start near saddle point
w0_saddle = np.array([0.5, 0.01])

path_sgd_s = sgd_vanilla(grad_saddle, w0_saddle, lr=0.05, n_iters=200)
path_mom_s = sgd_momentum(grad_saddle, w0_saddle, lr=0.05, beta=0.9, n_iters=200)

w0r = np.linspace(-2, 2, 200)
w1r = np.linspace(-2, 2, 200)
W0, W1 = np.meshgrid(w0r, w1r)
L_saddle = W0**2 - W1**2 + 0.1*(W0**2 + W1**2)**2

fig, axes = plt.subplots(1, 2, figsize=(12, 5))
for ax, path, title in zip(axes, [path_sgd_s, path_mom_s], ['Vanilla SGD', 'Momentum']):
    ax.contour(W0, W1, L_saddle, levels=30, cmap='coolwarm', alpha=0.7)
    ax.plot(path[:, 0], path[:, 1], 'k-', linewidth=1, alpha=0.7)
    ax.plot(path[0, 0], path[0, 1], 'gs', markersize=10, label='Start')
    ax.plot(path[-1, 0], path[-1, 1], 'r*', markersize=15, label='End')
    ax.plot(0, 0, 'kx', markersize=10, markeredgewidth=2, label='Saddle')
    ax.set_title(title)
    ax.legend()

plt.suptitle('Escaping Saddle Points: Momentum Helps', fontsize=14)
plt.tight_layout()
plt.show()
```

---

# 🔟 Deep Reflection Questions (Research Mindset) 🧠🔬

## 🤔 Why is $\beta = 0.9$ such a robust default?

$\beta = 0.9$ averages over ~10 recent gradients (window $\approx \frac{1}{1-\beta} = 10$).
This is long enough to smooth oscillations but short enough to respond to landscape changes.

> 🎯 **Beginner Analogy:** It's like a weather forecast that averages the last 10 days of temperature. That's enough to know the season's trend without being thrown off by one random cold day! 🌤️

For most problems, the "memory" of 10 gradients captures the consistent descent direction without lagging too far behind when the gradient changes.

| $\beta$ Value | Window Size | Behavior | Verdict |
|:---:|:---:|:---|:---:|
| 0.5 | ~2 | 😐 Barely any smoothing | Too light |
| 0.8 | ~5 | 🙂 Some smoothing | OK for simple problems |
| **0.9** | ~10 | 🎯 **Sweet spot for most problems** | ⭐ **Default** |
| 0.95 | ~20 | 🏃 More aggressive momentum | Large-batch training |
| 0.99 | ~100 | 🐌 Too sluggish for rapid changes | Rarely used alone |

⭐ The sweet spot is around **0.9** for a wide range of problems. ✅

## 🤔 How does momentum relate to second-order optimization?

Second-order methods (Newton's method) use the Hessian $H$:

$$w_{t+1} = w_t - H^{-1} \nabla L$$

This automatically adjusts step size based on curvature.
- High curvature → small steps 🐌
- Low curvature → large steps 🚀

⭐ **Momentum approximates this:** In directions with consistent gradients (low effective curvature for the path), momentum accelerates. In oscillating directions (high curvature), momentum dampens.

> 🎯 **Beginner Analogy:** Newton's method is like having a GPS with perfect satellite imagery — it knows exactly how curvy the road is. Momentum is like having a rear-view mirror — you can't see ahead perfectly, but by looking at where you've BEEN, you get a good idea of where to GO! 🪞🚗

**Momentum is a "poor man's second-order method"** — cheaper than computing $H^{-1}$ but captures some of its benefits. 💰

## 🤔 Why does Nesterov outperform classical momentum?

Classical momentum: Evaluate gradient at current position, then add to velocity.
If the velocity is already too large, the gradient at the current position **doesn't know this**.

Nesterov: First go where velocity would take you, THEN evaluate gradient.
This gives a **"corrective" signal**: if you've overshot, the gradient at the lookahead tells you to slow down. 🛑

> 🎯 **Beginner Analogy:** It's like the difference between driving forward and THEN checking the map 🗺️, versus checking the map from where you're HEADING 🔭. The latter lets you adjust course **before** you arrive!

---

# 1️⃣1️⃣ Startup Founder Insight 💼🚀

Momentum directly affects **model training speed** and **cost** 💰:

| # | Insight | Impact | 💰 Value |
|:---:|:---|:---|:---|
| 1 | ⏱️ **Training time** | Momentum reduces training time by 2-5× on ill-conditioned problems | 1 week → 2 days = thousands of $$ saved |
| 2 | 🎛️ **Hyperparameter simplicity** | SGD+Momentum has only 2 params (lr, β). Adam has 4 | Faster iteration cycles for your ML team |
| 3 | 📊 **Production stability** | Momentum smooths training curves | Easier to monitor and detect problems |
| 4 | 🛡️ **Robustness** | SGD+Momentum is STILL the optimizer of choice for CV | Know when each tool is appropriate |
| 5 | 🎁 **Weight averaging for FREE** | EMA of weights (Polyak averaging) gives extra test accuracy | 0.5–1.5% improvement at zero cost! |

### 🏭 When to Use What in Production

| Your Situation | Recommended Optimizer | Why? |
|:---|:---|:---|
| 🖼️ Training a vision model (ResNet, EfficientNet) | SGD + Momentum ($\beta=0.9$) + Nesterov | Best generalization, industry standard |
| 📝 Training a language model (BERT, GPT) | AdamW | Handles sparse gradients better |
| 🚀 Quick prototyping | Adam (default settings) | Works out of the box |
| 🎯 Maximum performance on known architecture | SGD + Momentum + cosine LR schedule | Proven recipe at Google, Meta |
| ⚡ Very large batch training ($>$8K) | LAMB or SGD with momentum warmup | Layer-wise scaling prevents divergence |

---

# 1️⃣2️⃣ Homework (Required) 📝✏️

1. 🧪 **Implement SGD, SGD+Momentum, and SGD+Nesterov from scratch.** Compare convergence on an ill-conditioned quadratic with $\kappa = 100$. Visualize paths on the loss surface.

2. 📈 **Apply momentum to mini-batch SGD on a linear regression problem.** Show that momentum converges faster than vanilla SGD at the same learning rate.

3. ⚖️ **Implement EMA of weights.** Show that the EMA weights produce a lower loss than the final iterate, especially with small batch sizes.

4. 🏔️ **Create a loss function with a saddle point.** Show that momentum escapes while vanilla SGD stalls.

5. 🎛️ **Experiment with different $\beta$ values** (0.5, 0.8, 0.9, 0.95, 0.99). Plot convergence curves. Identify when high momentum helps and when it hurts (overshooting).

---

# 📚 Summary Cheat Sheet 🗂️

| Concept | Formula | Beginner Analogy |
|:---|:---:|:---|
| ⭐ Momentum Update | $v_t = \beta v_{t-1} + \nabla L(w_t)$ | Ball rolling downhill accumulates speed 🎳 |
| ⭐ Weight Update | $w_{t+1} = w_t - \alpha v_t$ | Ball moves in the direction of accumulated velocity |
| ⭐ Nesterov Update | $v_t = \beta v_{t-1} + \nabla L(w_t - \alpha\beta v_{t-1})$ | Look ahead before stepping 🔭 |
| Effective Step Size | $\frac{\alpha}{1-\beta}$ | How far the ball actually moves per step |
| EMA Window | $\frac{1}{1-\beta}$ | How many "memories" the ball keeps |
| SGD Convergence | $\frac{\kappa - 1}{\kappa + 1}$ | Slow zigzag walker 🚶 |
| Momentum Convergence | $\frac{\sqrt{\kappa} - 1}{\sqrt{\kappa} + 1}$ | Smooth ice skater ⛸️ |
| Optimal $\beta$ | $\left(\frac{\sqrt{\kappa}-1}{\sqrt{\kappa}+1}\right)^2$ | Theory says... but 0.9 works! ✅ |

> 🎓 **Key Takeaway:** Momentum is the simplest idea that makes the biggest difference in optimization. Master it, and you'll understand the foundation of EVERY modern optimizer! 🚀
