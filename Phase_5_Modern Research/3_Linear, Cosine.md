📘 DAY 3 — Noise Schedules: Linear, Cosine & Their Impact 🎛️🔊

Goal:

Master noise schedule design — how the choice of β_t values affects training dynamics, sample quality, and the distribution of noise levels. Implement and compare linear and cosine schedules.

> 🧭 **Why this matters:** Every AI image generator you've ever seen — Stable Diffusion, DALL·E, Midjourney — works by *destroying* images with noise and then learning to *undo* that destruction. The **noise schedule** is the recipe that says *how fast* to destroy the image. Get it wrong, and your AI produces blurry messes. Get it right, and you get photorealistic art. This is one of the most impactful design choices in all of diffusion models.

> 💡 **Zero-knowledge analogy:** Imagine you're an art teacher. You take a student's painting and gradually smudge it until it's pure static. The **noise schedule** is your smudging plan — do you smudge evenly from start to finish? Or do you go gently at first (preserving big shapes), then faster, then gently again at the end (when only fine details remain)? That plan *completely* changes how well the student can learn to restore the painting.

---

# 1️⃣ What is a Noise Schedule? 🤔

The noise schedule {β_1, ..., β_T} controls HOW FAST we add noise.

β_t small → slow corruption → more steps needed → slow sampling
β_t large → fast corruption → detail lost too quickly → poor quality

The schedule determines ᾱ_t = Π(1-β_s), which controls the SNR at each step.

## ⭐ Beginner Breakdown: What's Actually Happening?

**Think of it like dimming a light** 💡:
- You have a beautiful photo (the "signal") and you're gradually replacing it with TV static ("noise")
- At step $t = 0$: 100% photo, 0% noise (light fully on)
- At step $t = T$: 0% photo, 100% noise (light fully off)
- The **noise schedule** controls *how you dim the light* from full brightness to off

**Key variables explained for beginners:**

| Symbol | What it means | Analogy |
|--------|--------------|--------|
| $\beta_t$ | How much NEW noise to add at step $t$ | How much you turn the dimmer knob at step $t$ |
| $\bar{\alpha}_t$ | How much ORIGINAL signal remains at step $t$ | How bright the light still is at step $t$ |
| $T$ | Total number of steps (typically 1000) | Total time from fully bright to fully dark |
| $\text{SNR}(t)$ | Signal-to-Noise Ratio at step $t$ | Ratio of "real image" to "static" at step $t$ |

**The math in LaTeX:**

$$\bar{\alpha}_t = \prod_{s=1}^{t}(1 - \beta_s)$$

$$\text{SNR}(t) = \frac{\bar{\alpha}_t}{1 - \bar{\alpha}_t}$$

> ⭐ **Key insight:** The schedule isn't just a detail — it's one of the most impactful hyperparameters in diffusion models. A bad schedule wastes compute on "easy" noise levels and rushes through the "hard" ones (Ho et al., 2020).

---

# 2️⃣ Linear Schedule (Original DDPM, Ho et al. 2020) 📏

> 📜 **Historical context:** This is the schedule from the paper that started the modern diffusion revolution: *"Denoising Diffusion Probabilistic Models"* by Jonathan Ho, Ajay Jain, and Pieter Abbeel (NeurIPS 2020). It's the simplest possible choice — and it works surprisingly well, but has clear limitations.

β_t linearly interpolated from β_start to β_end:

β_t = β_start + (β_end - β_start) × (t-1)/(T-1)

**In LaTeX:**

$$\beta_t = \beta_{\min} + \frac{(\beta_{\max} - \beta_{\min}) \cdot t}{T}$$

Standard values: β_start = 0.0001, β_end = 0.02, T = 1000

> 💡 **Analogy: Walking at a constant speed** 🚶
> Imagine you're walking from your house to a store at a perfectly constant speed. You don't speed up on the easy sidewalk or slow down at the tricky crosswalk. It's simple and predictable — but it's not *smart*. You waste time on the easy parts and rush through the parts that need care. That's exactly what a linear noise schedule does.

```python
def linear_schedule(T=1000, beta_start=1e-4, beta_end=0.02):
    return torch.linspace(beta_start, beta_end, T)
```

### Problems with Linear Schedule 🚨
- At low t: ᾱ_t ≈ 1 (very little noise) → model doesn't learn much here
- At high t: ᾱ_t → 0 too fast → fine details destroyed too quickly
- Uneven distribution of "useful" noise levels

### ⭐ Why These Problems Matter (Beginner Explanation)

Imagine you have 1000 steps to learn something:

| Step range | What happens (Linear) | Problem |
|-----------|----------------------|--------|
| Steps 1–200 | Almost no noise added | 🥱 Model is bored — image barely changed, nothing to learn |
| Steps 200–600 | Moderate noise | ✅ This is the "sweet spot" where learning happens |
| Steps 600–900 | Noise increases rapidly | ⚡ Model is rushed — fine details vanish too fast |
| Steps 900–1000 | Image is nearly pure noise | 🥱 Model is bored again — everything looks the same |

> 🔬 **Research note (Ho et al., 2020):** The authors chose $\beta_{\text{start}} = 10^{-4}$ and $\beta_{\text{end}} = 0.02$ empirically. These exact values became the de facto standard for 256×256 image generation and are still used in many codebases today.

> 🏭 **Production note:** Despite its limitations, the linear schedule is still used in **Stable Diffusion v1** (via the latent diffusion framework by Rombach et al., 2022). For latent spaces (~64×64), its shortcomings are less severe.

---

# 3️⃣ Cosine Schedule (Nichol & Dhariwal 2021) 🌊

> 📜 **Paper:** *"Improved Denoising Diffusion Probabilistic Models"* by Alex Nichol & Prafulla Dhariwal (ICML 2021). This paper is a masterclass in "small changes, big impact" — the cosine schedule alone boosted image quality significantly.

Designed to produce a smoother ᾱ_t curve:

ᾱ_t = f(t) / f(0)  where  f(t) = cos((t/T + s)/(1+s) × π/2)²

**In LaTeX:**

$$\bar{\alpha}_t = \frac{f(t)}{f(0)}, \quad \text{where } f(t) = \cos^2\!\left(\frac{t/T + s}{1 + s} \cdot \frac{\pi}{2}\right)$$

s is a small offset (default 0.008) to prevent β_t from being too small at t=0.

β_t = 1 - ᾱ_t/ᾱ_{t-1}, clipped to [0, 0.999]

**In LaTeX:**

$$\beta_t = 1 - \frac{\bar{\alpha}_t}{\bar{\alpha}_{t-1}}, \quad \text{clipped to } [0, 0.999]$$

> 💡 **Analogy: The smart runner** 🏃‍♂️
> Now imagine an *experienced* runner on the same route. On the easy sidewalk, they sprint (adding noise fast when it doesn't matter). At the tricky crosswalk, they slow down carefully (adding noise gently when fine details are at stake). At the very end, they ease to a stop (transitioning smoothly to pure noise). That's the cosine schedule — it's *adaptive*, spending compute where it matters most.

> 💡 **Another analogy: Dimming a light smoothly** 💡
> - **Linear** = a straight-line dim from bright to dark. Harsh transitions at both ends.
> - **Cosine** = a gentle S-curve dim. Soft at the start (preserving the image longer), soft at the end (approaching pure noise gracefully), with a smooth transition in between.

```python
def cosine_schedule(T=1000, s=0.008):
    steps = torch.arange(T + 1, dtype=torch.float32) / T
    f_t = torch.cos((steps + s) / (1 + s) * torch.pi / 2) ** 2
    alpha_bars = f_t / f_t[0]
    betas = 1 - alpha_bars[1:] / alpha_bars[:-1]
    return torch.clamp(betas, 0.0001, 0.999)
```

### Why Cosine is Better ✅
- Smoother ᾱ_t decay → more uniform noise level distribution
- Signal preserved longer at low t → better fine detail learning
- Especially good for lower-resolution images (32×32, 64×64)

### ⭐ Deep Dive: Why Does the Cosine Shape Work So Well?

The cosine function has a beautiful mathematical property: its derivative is zero at both endpoints ($t=0$ and $t=T$). This means:

1. **Near $t=0$:** $\bar{\alpha}_t$ barely changes → the image stays clean for longer → the model gets more time to learn fine details 🎨
2. **In the middle:** $\bar{\alpha}_t$ drops steadily → efficient learning across a wide range of noise levels ⚡
3. **Near $t=T$:** $\bar{\alpha}_t$ approaches 0 gently → smooth transition to pure noise instead of a harsh cliff 🌅

> 🔬 **Research insight (Nichol & Dhariwal, 2021):** The authors discovered the cosine schedule by observing that $\bar{\alpha}_t$ for the linear schedule looked like a steep cliff when plotted. They asked: "What if we designed $\bar{\alpha}_t$ *directly* to be a nice smooth curve?" The cosine function was a natural choice. The small offset $s = 0.008$ prevents $\beta_t$ from being too small near $t=0$, which would cause numerical issues.

### ⭐ The Offset $s$ Explained

| $s$ value | Effect | When to use |
|-----------|--------|------------|
| $s = 0$ | $\beta_0 = 0$ exactly — causes division-by-zero issues ❌ | Never |
| $s = 0.008$ | Default — ensures $\beta_t > 0$ everywhere ✅ | Almost always |
| $s > 0.008$ | Shifts the curve — more noise at early steps | Experimental only |

> 🏭 **Production spotlight:**
> - **Improved DDPM** (Nichol & Dhariwal, 2021): Uses cosine schedule + learned variance → SOTA on CIFAR-10, ImageNet 64×64
> - **DALL·E 2** (Ramesh et al., 2022): Uses a cosine variant for its diffusion prior
> - **Stable Diffusion** (Rombach et al., 2022): Uses a *learned* schedule in latent space, but cosine-like schedules are common in fine-tuning

---

# 4️⃣ Comparison & Visualization 📊

> 💡 **What to look for in these plots:**
> - **Plot 1 ($\beta_t$):** How much noise is added at each step. Linear = straight line. Cosine = curved.
> - **Plot 2 ($\bar{\alpha}_t$):** How much original image remains. You want a *smooth S-curve*, not a cliff.
> - **Plot 3 ($\log \text{SNR}$):** The "information content" at each step. Ideally a smooth, roughly linear decline.

```python
def compare_schedules(T=1000):
    beta_linear = linear_schedule(T)
    beta_cosine = cosine_schedule(T)
    
    alpha_bar_lin = torch.cumprod(1 - beta_linear, dim=0)
    alpha_bar_cos = torch.cumprod(1 - beta_cosine, dim=0)
    
    fig, axes = plt.subplots(1, 3, figsize=(15, 4))
    
    axes[0].plot(beta_linear.numpy(), label='Linear')
    axes[0].plot(beta_cosine.numpy(), label='Cosine')
    axes[0].set_title('β_t (noise added per step)')
    axes[0].legend()
    
    axes[1].plot(alpha_bar_lin.numpy(), label='Linear')
    axes[1].plot(alpha_bar_cos.numpy(), label='Cosine')
    axes[1].set_title('ᾱ_t (cumulative signal)')
    axes[1].legend()
    
    snr_lin = alpha_bar_lin / (1 - alpha_bar_lin + 1e-8)
    snr_cos = alpha_bar_cos / (1 - alpha_bar_cos + 1e-8)
    axes[2].plot(torch.log(snr_lin).numpy(), label='Linear')
    axes[2].plot(torch.log(snr_cos).numpy(), label='Cosine')
    axes[2].set_title('log(SNR)')
    axes[2].legend()
    
    plt.tight_layout()
    plt.show()
```

### ⭐ Reading the Plots: A Beginner's Guide 🔍

**Plot 1 — $\beta_t$ (noise per step):**
| What you see | What it means |
|-------------|---------------|
| Linear: straight diagonal line | Same acceleration of noise at every step — simple but wasteful |
| Cosine: gentle curve, rises then falls | Adapts noise rate — more noise in the middle, gentler at edges |

**Plot 2 — $\bar{\alpha}_t$ (signal remaining):**
| What you see | What it means |
|-------------|---------------|
| Linear: steep cliff around step 600–800 | Signal suddenly crashes — the model has very few steps to learn fine → coarse transition |
| Cosine: smooth S-curve from 1 to 0 | Signal fades gracefully — every noise level gets represented fairly |

**Plot 3 — $\log \text{SNR}(t)$ (the gold standard):**
| What you see | What it means |
|-------------|---------------|
| Linear: kinked curve | Uneven learning — some noise levels get overrepresented, others underrepresented |
| Cosine: nearly linear decline | 🎯 Ideal! Every noise level gets roughly equal training attention |

> 🔬 **Research insight (Kingma et al., 2021 — Variational Diffusion Models):** The authors proved that the *optimal* noise schedule produces a $\log \text{SNR}$ curve that is *monotonically and smoothly decreasing*. The cosine schedule naturally approximates this, which is a big reason why it works better than linear.

$$\text{SNR}(t) = \frac{\bar{\alpha}_t}{1 - \bar{\alpha}_t}$$

> ⭐ **Rule of thumb:** If your $\log \text{SNR}$ plot has sharp bends or flat regions, your schedule is suboptimal.

---

# 5️⃣ Schedule Impact on Training 🎯

| Schedule | Low $t$ behavior | High $t$ behavior | Best for |
|----------|---------------|-----------------|----------|
| Linear | SNR drops slowly | Falls off cliff | Higher-res |
| Cosine | More gradual | Still reaches noise | Lower-res |
| Scaled linear | Adjusted for resolution | - | Flexible |

### ⭐ The Big Picture: Why Schedule Choice Depends on Resolution

> 💡 **Analogy:** Think of image resolution like the *complexity* of a puzzle 🧩
> - **Low-res (32×32):** A small puzzle — you need gentle handling because every pixel matters. Cosine's smoothness preserves each piece.
> - **High-res (256×256+):** A huge puzzle — the big picture is obvious even with lots of noise. Linear's aggression is less harmful because there's so much redundancy.

| Resolution | Recommended Schedule | Why | Used in |
|-----------|---------------------|-----|--------|
| 32×32 | Cosine ✅ | Every pixel matters — need smooth noise curve | CIFAR-10 experiments |
| 64×64 | Cosine ✅ | Still low enough that cosine's smoothness helps | ImageNet 64×64, DALL·E prior |
| 128×128 | Either 🤷 | Transition zone — test both | Research experiments |
| 256×256+ | Linear or Scaled Linear ✅ | Enough redundancy to tolerate aggressive schedules | Stable Diffusion, Imagen |
| Latent (~32×32) | Cosine-like ✅ | Latent space is low-dim — similar to small images | Latent Diffusion Models |

## Practical Guidance 🛠️
- 32×32 to 64×64: cosine schedule
- 256×256+: linear or scaled linear  
- Latent diffusion (typical latent ~32×32): cosine-like schedules
- Always verify by plotting ᾱ_t and checking sample quality at different t

### ⭐ Production Decision Matrix

| Question | If Yes → | If No → |
|----------|---------|--------|
| Working in pixel space? | Consider resolution-matched schedule | Use cosine (latent is small) |
| Resolution ≤ 64×64? | Use cosine | Consider linear/scaled linear |
| Need maximum quality? | Try cosine + learned variance (Nichol & Dhariwal) | Linear is fine |
| Training is unstable? | Check $\log \text{SNR}$ smoothness — consider cosine | Problem is elsewhere |
| Using pre-trained model? | Keep the original schedule! | Choose based on above |

> 🏭 **Real production examples:**
> - **Stable Diffusion v1/v2:** Linear schedule with $\beta_{\text{start}}=0.00085$, $\beta_{\text{end}}=0.012$ (scaled for latent space)
> - **Improved DDPM:** Cosine schedule — achieved new SOTA on ImageNet 64×64
> - **DALL·E 2:** Cosine variant for diffusion prior, linear for decoder
> - **Variational Diffusion Models (VDM, Kingma et al. 2021):** *Learned* schedule via neural network — ultimate flexibility but harder to train

---

# 6️⃣ Advanced: Continuous-Time Formulation ⚙️

Instead of discrete β_t, use a continuous noise schedule:

dx = f(x,t)dt + g(t)dw  (SDE)

**In LaTeX:**

$$d\mathbf{x} = f(\mathbf{x}, t)\,dt + g(t)\,d\mathbf{w}$$

Where β(t) is now a continuous function.

log(SNR(t)) should be roughly linear for good training.

### ⭐ Beginner Explanation: What's an SDE and Why Should I Care?

> 💡 **Analogy:** Imagine dropping a ball into a swirling river 🌊
> - The ball's position changes because of two things: the river's current (the *drift* $f(\mathbf{x}, t)$) and random turbulence (the *diffusion* $g(t) \, d\mathbf{w}$)
> - In discrete time: we update the ball position at step 1, step 2, step 3...
> - In continuous time: we describe the ball's motion as a smooth trajectory
> - The **noise schedule** $\beta(t)$ controls how much turbulence there is at each moment

**Why go continuous?**

| Aspect | Discrete (β_t) | Continuous (β(t)) |
|--------|----------------|-------------------|
| Flexibility | Fixed T steps | Any number of steps at inference |
| Theory | Hard to analyze | Clean mathematical framework |
| Schedule design | Manual tuning | Can be *learned* end-to-end |
| Used in | DDPM, Improved DDPM | Score SDE (Song et al., 2021), VDM |

> 🔬 **Research deep dive (Kingma et al., 2021 — VDM):** The Variational Diffusion Models paper showed that in continuous time, the *only* thing that matters for the ELBO (training objective) is the endpoints of the SNR: $\text{SNR}(0)$ and $\text{SNR}(1)$. Everything in between cancels out! This stunning result means the schedule shape affects *variance* of training (convergence speed) but not the *optimal solution*. This is why learned schedules converge to smooth $\log \text{SNR}$ curves — exactly as the cosine schedule approximates.

### Schedule Design Principles 📐

> ⭐ **The 5 Golden Rules of Noise Schedule Design:**

1. **$\log \text{SNR}(t)$ should decrease smoothly** — no cliffs, no plateaus
2. **$\text{SNR}(0)$ should be high** — at $t=0$ the image should be nearly clean
3. **$\text{SNR}(T)$ should be very low** — at $t=T$ the image should be nearly pure noise
4. **All noise levels should be represented** — don't waste steps on "boring" noise levels
5. **Match the schedule to your data dimensionality** — higher-dim data tolerates more aggressive schedules

---

# 📝 Summary

| Schedule | Formula | Key Property |
|----------|---------|-------------|
| Linear | β_t = linear interpolation | Simple, fast decay |
| Cosine | ᾱ_t from cosine function | Smoother, better for small images |
| SNR | ᾱ_t/(1-ᾱ_t) | Should decay smoothly |

---

# 🏁 Cheat Sheet: Noise Schedules at a Glance

| | Linear 📏 | Cosine 🌊 | Learned 🧠 |
|---|-----------|-----------|------------|
| **Formula** | $\beta_t = \beta_{\min} + \frac{(\beta_{\max} - \beta_{\min})t}{T}$ | $\bar{\alpha}_t = \frac{\cos^2\left(\frac{t/T+s}{1+s}\cdot\frac{\pi}{2}\right)}{\cos^2\left(\frac{s}{1+s}\cdot\frac{\pi}{2}\right)}$ | Neural network output |
| **$\bar{\alpha}_t$ shape** | Cliff (steep drop) | Smooth S-curve | Optimized |
| **$\log \text{SNR}$ shape** | Kinked | Nearly linear ✅ | Linear ✅✅ |
| **Strengths** | Simple, well-tested | Smooth, good for small images | Optimal by construction |
| **Weaknesses** | Wastes steps at extremes | Slightly worse for high-res | Harder to train |
| **Typical use** | Stable Diffusion v1-v2 | Improved DDPM, DALL·E | VDM, research |
| **Key paper** | Ho et al. (2020) | Nichol & Dhariwal (2021) | Kingma et al. (2021) |
| **Beginner analogy** | Constant-speed walker 🚶 | Smart adaptive runner 🏃 | GPS-guided drone 🤖 |

### ⭐ Quick Decision Flowchart

```
Starting a diffusion model?
├── Working in latent space? (e.g., Stable Diffusion)
│   └── YES → Use cosine-like or the pre-trained model's schedule
├── Image resolution ≤ 64×64?
│   └── YES → Use cosine schedule (s=0.008)
├── Image resolution ≥ 256×256 in pixel space?
│   └── YES → Use linear or scaled linear
└── Want absolute best quality & have compute?
    └── YES → Try learned schedule (VDM approach)
```

---

# 📚 Resources

## 📄 Key Papers

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|------------------|
| *Denoising Diffusion Probabilistic Models* | Ho, Jain, Abbeel | 2020 | Introduced the linear schedule; started the modern DDPM era |
| *Improved Denoising Diffusion Probabilistic Models* | Nichol & Dhariwal | 2021 | Introduced the cosine schedule + learned variance; significant quality gains |
| *Variational Diffusion Models* | Kingma, Salimans, Poole, Ho | 2021 | Continuous-time formulation; proved learned schedules are optimal; SNR analysis |
| *Score-Based Generative Modeling through SDEs* | Song, Sohl-Dickstein, Kingma, et al. | 2021 | Unified SDE framework connecting noise schedules to continuous processes |
| *High-Resolution Image Synthesis with Latent Diffusion Models* | Rombach, Blattmann, Lorenz, Esser, Ommer | 2022 | Latent diffusion (Stable Diffusion); practical schedule choices for latent spaces |

## 📖 Books & Surveys

- **"Understanding Deep Learning"** — Simon J.D. Prince (2023) — Chapter on diffusion models covers schedules clearly
- **"Probabilistic Machine Learning: Advanced Topics"** — Kevin Murphy (2023) — Rigorous treatment of noise schedules and SDEs
- **"A Survey on Generative Diffusion Model"** — Yang et al. (2023) — Comprehensive survey covering all schedule variants

## 🌐 Blog Posts & Tutorials

- **Lilian Weng — "What are Diffusion Models?"** — Outstanding visual explanation of schedules with plots and intuitions
- **Calvin Luo — "Understanding Diffusion Models: A Unified Perspective"** (2022) — Math-heavy but definitive tutorial connecting VAEs to diffusion schedules
- **Hugging Face Diffusion Course** — Hands-on code for implementing and comparing schedules

## 🔑 Key Takeaways for Absolute Beginners

> 1. 🎛️ The **noise schedule** is the plan for how fast to corrupt an image with noise
> 2. 📏 **Linear** = simple constant-rate corruption. Works but wastes steps.
> 3. 🌊 **Cosine** = smart variable-rate corruption. Smooth and efficient.
> 4. 📉 **SNR** tells you how much "real image" vs. "noise" exists at each step
> 5. ⭐ A good schedule makes $\log \text{SNR}$ decrease *smoothly* — that's the golden rule
> 6. 🏭 In practice: use cosine for small images, linear for big images, or just keep the pre-trained model's default

**Tomorrow**: Reverse Diffusion Process — learning to denoise step by step.
