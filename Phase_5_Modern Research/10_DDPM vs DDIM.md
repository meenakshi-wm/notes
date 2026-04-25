📘 DAY 10 — Sampling Algorithms: DDPM vs DDIM & Fast Sampling

Goal:

Master the different sampling strategies for diffusion models — understand why DDPM is slow, how DDIM enables deterministic fast sampling, and implement both with configurable step counts.

---

> 🌟 **Absolute Beginner? Start Here!**
>
> Imagine you have a photo that has been completely scrambled into TV static (pure noise). A diffusion model's job is to **un-scramble** that noise back into a beautiful image — step by step.
>
> The big question of this lesson: **How many steps do we need, and can we take shortcuts?**
>
> - **DDPM** 🐢 = Taking a random, winding hiking trail down a mountain — 1000 tiny steps, and every trip down is slightly different because you wander randomly.
> - **DDIM** 🏎️ = Taking a direct highway straight down — only 50 steps, and you arrive at the *exact same place* every time.
>
> That's literally the core idea. Everything below is the "how" and "why."

---

> 📖 **Key Research Paper:** Ho, Sohl-Dickstein & Abbeel, *"Denoising Diffusion Probabilistic Models"* (NeurIPS 2020) — the paper that started the modern diffusion revolution.

---

# 1️⃣ DDPM Sampling: The Baseline (1000 Steps)

> 🏔️ **Beginner Analogy — The Winding Mountain Path:**
> You're at the top of a foggy mountain (pure noise). DDPM says: "Take 1000 tiny steps downhill. At each step, I'll point you roughly toward the valley (the clean image), but I'll also add a small random nudge." Because of those random nudges, if you climb back up and come down again, you'll take a *slightly different path* and end up at a *slightly different spot* in the valley. That randomness is what makes DDPM **stochastic** (non-deterministic).

Review from DAY 10:

⭐ **DDPM Reverse Step Formula (LaTeX):**

$$x_{t-1} = \frac{1}{\sqrt{\alpha_t}} \left( x_t - \frac{\beta_t}{\sqrt{1 - \bar{\alpha}_t}} \cdot \epsilon_\theta(x_t, t) \right) + \sigma_t \cdot z$$

x_{t-1} = (1/√α_t)(x_t - β_t/√(1-ᾱ_t) · ε_θ(x_t, t)) + σ_t · z

Where $z \sim \mathcal{N}(0, I)$ and $\sigma_t = \sqrt{\beta_t}$.

Where z ~ N(0,I) and σ_t = √β_t.

> 🔍 **Breaking Down the Formula for Beginners:**
>
> | Symbol | What It Means | Plain English |
> |--------|--------------|---------------|
> | $x_t$ | Current noisy image at step $t$ | The blurry photo you're holding right now |
> | $\epsilon_\theta(x_t, t)$ | Neural network's noise prediction | The AI's best guess at "what noise was added" |
> | $\alpha_t, \bar{\alpha}_t$ | Noise schedule parameters | How much signal vs. noise is left at step $t$ |
> | $\sigma_t \cdot z$ | Random noise injection | The "random nudge" that makes each path different |
> | $x_{t-1}$ | Slightly cleaner image | One step closer to the final picture! |

**Problem**: Requires T=1000 sequential forward passes = 1000 neural network evaluations.
For a 6B parameter model on a GPU: ~30 seconds per image.

> ⚠️ **Why This Is a Problem in Practice:**
> - 💰 At cloud GPU rates (~$0.50/hr for an A100), generating 10,000 images costs real money
> - ⏱️ 30 seconds per image means you **cannot** use DDPM for real-time applications
> - 📱 Mobile/edge deployment? Completely impossible at 1000 steps
> - 🏭 Production services like Midjourney or DALL·E would be unusably slow with raw DDPM

---

# 2️⃣ DDIM: Denoising Diffusion Implicit Models (Song et al., 2020)

> 📖 **Key Research Paper:** Song, Meng & Ermon, *"Denoising Diffusion Implicit Models"* (ICLR 2021) — the breakthrough that made diffusion models practical for production use.

> 🏎️ **Beginner Analogy — The Direct Highway:**
> Remember DDPM was the winding hiking trail? DDIM says: "Forget the random wandering. I'll build a **straight highway** from the mountaintop to the valley. No random nudges — just a clean, direct path." Because there's no randomness, you always end up at the *exact same spot*. And because the path is straight, you can take **bigger steps** — maybe only 50 instead of 1000!

Key insight: The forward marginals q(x_t|x_0) don't change, but we can choose a DIFFERENT reverse process — one that's deterministic and supports subsampling.

> 💡 **What does that mean in plain English?**
> Both DDPM and DDIM start from the same noisy image. The magic is that DDIM finds a *different route* back to the clean image — one that doesn't require random noise at each step. Think of it like this: two GPS apps giving you different routes to the same destination. The roads are different, but you arrive at the same place.

## Non-Markovian Forward Process

> 📝 **"Non-Markovian" for Beginners:** In DDPM, each step only looks at the immediately previous step (like only remembering yesterday). DDIM can "peek ahead" — it considers both the current noisy image AND its prediction of the final clean image. It's like navigating with both a compass AND a map of your destination.

DDIM uses q_σ(x_{t-1}|x_t, x_0) with a tunable noise parameter η:

⭐ **DDIM General Reverse Step (LaTeX):**

$$x_{t-1} = \sqrt{\bar{\alpha}_{t-1}} \cdot \hat{x}_0 + \sqrt{1 - \bar{\alpha}_{t-1} - \sigma_t^2} \cdot \epsilon_\theta(x_t, t) + \sigma_t \cdot z$$

x_{t-1} = √ᾱ_{t-1} · x̂_0 + √(1-ᾱ_{t-1}-σ²_t) · ε_θ(x_t,t) + σ_t · z

Where:
- x̂_0 = (x_t - √(1-ᾱ_t)·ε_θ(x_t,t)) / √ᾱ_t (predicted clean image)
- σ_t = η · √((1-ᾱ_{t-1})/(1-ᾱ_t)) · √(1-ᾱ_t/ᾱ_{t-1})
- η=1 → DDPM, η=0 → deterministic DDIM

> 🎛️ **The η (Eta) Parameter — Your Randomness Dial:**
>
> Think of $\eta$ as a **dial on a mixing board** that goes from 0 to 1:
>
> | $\eta$ Value | What Happens | Analogy |
> |:---:|---|---|
> | $\eta = 0$ | **Fully deterministic** — no randomness at all | Highway with GPS: same route every time 🛣️ |
> | $\eta = 0.5$ | **Mild randomness** — mostly predictable with slight variation | Highway with occasional scenic detours 🌄 |
> | $\eta = 1.0$ | **Fully stochastic** — recovers original DDPM | The winding mountain trail again 🥾 |
>
> ⭐ **Key insight:** $\eta$ lets you smoothly interpolate between DDPM and DDIM with a single parameter!

**Predicted clean image (LaTeX):**

$$\hat{x}_0 = \frac{x_t - \sqrt{1 - \bar{\alpha}_t} \cdot \epsilon_\theta(x_t, t)}{\sqrt{\bar{\alpha}_t}}$$

**Noise coefficient (LaTeX):**

$$\sigma_t = \eta \cdot \sqrt{\frac{1 - \bar{\alpha}_{t-1}}{1 - \bar{\alpha}_t}} \cdot \sqrt{1 - \frac{\bar{\alpha}_t}{\bar{\alpha}_{t-1}}}$$

## DDIM Sampling (η=0)

⭐ **DDIM Deterministic Reverse Step (LaTeX):**

$$x_{t-1} = \sqrt{\bar{\alpha}_{t-1}} \cdot \hat{x}_0 + \sqrt{1 - \bar{\alpha}_{t-1}} \cdot \epsilon_\theta(x_t, t)$$

x_{t-1} = √ᾱ_{t-1} · x̂_0 + √(1-ᾱ_{t-1}) · ε_θ(x_t,t)

No noise added! Deterministic mapping from x_T to x_0.

> 🎯 **Why Deterministic Matters:**
> - 🔁 **Reproducibility:** Same noise input → same image output. Critical for debugging and testing in production.
> - 🧪 **Scientific rigor:** You can compare different models fairly since the sampling doesn't add variability.
> - 🔄 **Inversion:** You can go *backwards* — given a real image, find the noise that would produce it (impossible with stochastic DDPM!).
> - 🎨 **Image editing:** Invert a photo to noise-space, modify it, then denoise — this is how many AI image editors work.

---

# 3️⃣ Fast Sampling with Subsequences

> 🛗 **Beginner Analogy — The Express Elevator:**
> Imagine a 1000-floor building. DDPM takes the stairs — every single floor, one at a time (floor 1000 → 999 → 998 → ... → 1 → ground). DDIM says: "Why not take the **express elevator** that only stops at floors 1000, 980, 960, ..., 20, ground?" You still get from top to bottom, but with only **50 stops** instead of 1000!
>
> The key insight: because DDIM's path is deterministic (no randomness), skipping floors doesn't cause you to "miss" anything important. With DDPM's random wandering, skipping steps would cause chaos.

DDIM can skip timesteps. Instead of t = 999, 998, ..., 1, 0, use a subsequence:

Example with 50 steps: t = 999, 979, 959, ..., 19, 0

> 📊 **Step Count vs. Quality Trade-off:**
>
> | Steps | Quality | Speed | Best For |
> |:-----:|---------|-------|----------|
> | 1000 | ⭐⭐⭐⭐⭐ Perfect | 🐢 Very slow | Research benchmarks |
> | 100 | ⭐⭐⭐⭐ Near-perfect | 🚶 Moderate | High-quality batch generation |
> | 50 | ⭐⭐⭐⭐ Great | 🏃 Fast | Production sweet spot |
> | 20 | ⭐⭐⭐ Good | 🚗 Very fast | Real-time previews |
> | 10 | ⭐⭐ Acceptable | 🏎️ Blazing | Interactive apps / thumbnails |
> | 5 | ⭐ Rough | ✈️ Instant | Rough drafts only |

```python
def get_ddim_schedule(T, num_steps):
    """Create evenly-spaced timestep subsequence."""
    c = T // num_steps
    return list(range(0, T, c))[::-1]  # descending order
```

> 🏭 **Production Note:** Stable Diffusion's default scheduler uses DDIM-style step skipping. When you set `num_inference_steps=50` in the Hugging Face `diffusers` library, this is exactly what happens under the hood!

---

# 4️⃣ Complete Implementation

```python
class DiffusionSampler:
    def __init__(self, model, diffusion, device='cuda'):
        self.model = model
        self.diff = diffusion
        self.device = device
    
    @torch.no_grad()
    def ddpm_sample(self, shape, verbose=True):
        """Standard DDPM: T steps, stochastic."""
        x = torch.randn(shape, device=self.device)
        steps = list(range(self.diff.T))[::-1]
        
        for t in tqdm(steps, disable=not verbose):
            t_batch = torch.full((shape[0],), t, device=self.device, dtype=torch.long)
            eps = self.model(x, t_batch)
            
            alpha = self.diff.alphas[t]
            alpha_bar = self.diff.alpha_bars[t]
            beta = self.diff.betas[t]
            
            mean = (1/torch.sqrt(alpha)) * (x - beta/torch.sqrt(1-alpha_bar) * eps)
            
            if t > 0:
                x = mean + torch.sqrt(beta) * torch.randn_like(x)
            else:
                x = mean
        return x
    
    @torch.no_grad()
    def ddim_sample(self, shape, num_steps=50, eta=0.0, verbose=True):
        """DDIM: configurable steps, deterministic (eta=0) or stochastic."""
        x = torch.randn(shape, device=self.device)
        
        # Create timestep subsequence
        step_size = self.diff.T // num_steps
        timesteps = list(range(0, self.diff.T, step_size))[::-1]
        
        for i in tqdm(range(len(timesteps)), disable=not verbose):
            t = timesteps[i]
            t_prev = timesteps[i+1] if i+1 < len(timesteps) else 0
            
            t_batch = torch.full((shape[0],), t, device=self.device, dtype=torch.long)
            eps = self.model(x, t_batch)
            
            alpha_bar_t = self.diff.alpha_bars[t]
            alpha_bar_prev = self.diff.alpha_bars[t_prev] if t_prev > 0 else torch.tensor(1.0)
            
            # Predict x_0
            x0_pred = (x - torch.sqrt(1-alpha_bar_t) * eps) / torch.sqrt(alpha_bar_t)
            x0_pred = x0_pred.clamp(-1, 1)
            
            # DDIM noise coefficient
            sigma = eta * torch.sqrt((1-alpha_bar_prev)/(1-alpha_bar_t) * (1-alpha_bar_t/alpha_bar_prev))
            
            # Direction pointing to x_t
            dir_xt = torch.sqrt(1 - alpha_bar_prev - sigma**2) * eps
            
            # Noise
            noise = sigma * torch.randn_like(x) if sigma > 0 else 0
            
            x = torch.sqrt(alpha_bar_prev) * x0_pred + dir_xt + noise
        
        return x
```

---

# 5️⃣ Speed Comparison

> ⏱️ **Beginner Analogy:** Imagine you're baking a cake. DDPM insists on checking the oven 1000 times. DDIM says: "Just check 50 times — the cake turns out basically the same." That's a **20x speedup** for nearly identical results!

| Method | Steps | Quality (FID) | Time |
|--------|-------|--------------|------|
| DDPM | 1000 | Best | 30s |
| DDIM (η=0) | 100 | Near-DDPM | 3s |
| DDIM (η=0) | 50 | Good | 1.5s |
| DDIM (η=0) | 20 | Acceptable | 0.6s |
| DDIM (η=0) | 10 | Degraded | 0.3s |

DDIM at 50 steps ≈ DDPM at 1000 steps quality. 20x speedup!

> 🏭 **Real-World Production Impact:**
>
> | Scenario | DDPM (1000 steps) | DDIM (50 steps) | Savings |
> |----------|-------------------|-----------------|----------|
> | Generate 1 image | 30 seconds | 1.5 seconds | 28.5s saved |
> | Batch of 1,000 images | 8.3 hours | 25 minutes | ~8 hours saved |
> | 1M images/day API service | 347 A100 GPUs | 17 A100 GPUs | ~$480K/month savings 💰 |
> | Mobile app (on-device) | ❌ Impossible | ✅ Feasible with quantization | Enables new products |
>
> ⭐ **This single improvement (DDPM → DDIM) is what made commercial diffusion products viable.**
> - 🎨 **Stable Diffusion** uses DDIM-based schedulers (typically 20-50 steps)
> - 🖼️ **Midjourney** uses customized fast samplers derived from DDIM principles
> - 📱 **On-device generation** (Apple's Core ML Stable Diffusion) relies on DDIM's low step count
>
> 📖 *Reference: Song et al. (2021) showed DDIM at 50 steps matches DDPM at 1000 steps on CIFAR-10 FID scores.*

---

# 6️⃣ DDIM Properties

> 🧭 **Beginner Analogy — GPS vs. Wandering:**
> DDPM is like exploring a city without GPS — every walk is a unique adventure, and you never retrace the same path. DDIM is like using Google Maps with a fixed route — type in the same destination, get the **exact same directions** every time. This predictability unlocks powerful capabilities!

## Deterministic Encoding (η=0)
Since DDIM is deterministic, the mapping x_T → x_0 is fixed:
- Same x_T always produces same x_0
- Enables latent space interpolation between images
- Enables inversion: find x_T that gives a specific x_0

> 🔑 **Why Each Property Matters:**
>
> | Property | What It Enables | Real-World Use Case |
> |----------|----------------|--------------------|
> | **Same output every time** | Reproducible generation | A/B testing, debugging, scientific research |
> | **Latent interpolation** | Smooth morphing between images | Video generation, creative tools, animation |
> | **Inversion** | Encode real images into noise-space | Image editing (SDEdit), style transfer, inpainting |

## Latent Interpolation

> 🎨 **Beginner Analogy — Mixing Paint Colors:**
> Imagine Image A's noise-code is "red paint" and Image B's noise-code is "blue paint." Interpolation is like mixing them in different ratios: 100% red → 75/25 → 50/50 → 25/75 → 100% blue. Each mixture, when decoded, gives you a smooth blend between Image A and Image B!
>
> **Spherical interpolation** (SLERP) is used instead of simple averaging because the noise vectors live on a high-dimensional sphere (they all have roughly the same length). Linear averaging would "cut through" the sphere, producing vectors that are too short.

```python
def interpolate(sampler, x_T_1, x_T_2, alpha, num_steps=50):
    """Spherical interpolation in noise space."""
    theta = torch.arccos(torch.sum(x_T_1 * x_T_2) / (x_T_1.norm() * x_T_2.norm()))
    x_T_interp = (torch.sin((1-alpha)*theta)/torch.sin(theta)) * x_T_1 + \
                 (torch.sin(alpha*theta)/torch.sin(theta)) * x_T_2
    return sampler.ddim_sample(x_T_interp.unsqueeze(0).shape, num_steps=num_steps)
```

> 🏭 **Production Application — DDIM Inversion:**
> This is the backbone of image-to-image editing in Stable Diffusion:
> 1. Take a real photo → run DDIM *forward* (add noise deterministically) → get the latent code $x_T$
> 2. Modify the text prompt (e.g., "cat" → "dog")
> 3. Run DDIM *reverse* from the same $x_T$ with the new prompt → get an edited image
>
> The structure of the original image is preserved because the latent code encodes spatial layout!

---

# 7️⃣ Advanced Samplers

> 🚀 **Beginner Analogy — Evolution of Transportation:**
> DDPM is walking (slow, 1000 steps). DDIM is driving (faster, 50 steps). The advanced samplers below are like bullet trains, planes, and teleporters — each one uses cleverer math to get the same result in fewer steps!

| Sampler | Type | Steps | Key Idea |
|---------|------|-------|----------|
| DDPM | Stochastic | 1000 | Original |
| DDIM | Deterministic | 50-100 | Skip steps, no noise |
| PLMS | Linear Multi-step | 20-50 | Use multiple previous predictions |
| DPM-Solver | ODE solver | 10-25 | Higher-order solver |
| DPM-Solver++ | Improved | 10-20 | Data prediction formulation |
| UniPC | Unified Predictor-Corrector | 5-20 | Combines both strategies |
| Euler | First-order | 20-50 | Simple ODE integration |
| Heun | Second-order | 20-50 | Predictor-corrector |

Modern systems use DPM-Solver++ or Euler for production speed.

> 📖 **Research Citations for Advanced Samplers:**
> - **DPM-Solver:** Lu et al., *"DPM-Solver: A Fast ODE Solver for Diffusion Probabilistic Model Sampling in Around 10 Steps"* (NeurIPS 2022)
> - **EDM:** Karras et al., *"Elucidating the Design Space of Diffusion-Based Generative Models"* (NeurIPS 2022) — unified framework showing many samplers are special cases
> - **UniPC:** Zhao et al., *"UniPC: A Unified Predictor-Corrector Framework for Fast Sampling of Diffusion Models"* (NeurIPS 2023)

> 🏭 **What Production Systems Actually Use (2024-2026):**
>
> | Product | Default Sampler | Steps | Why |
> |---------|----------------|:-----:|-----|
> | Stable Diffusion (HuggingFace) | DPM-Solver++ | 20-30 | Best quality/speed balance |
> | Stable Diffusion XL | Euler / Euler Ancestral | 25-40 | Tuned for SDXL architecture |
> | Midjourney | Proprietary (DPM-based) | ~50 | Quality-focused |
> | DALL·E 3 | Undisclosed | ~30-50 | Optimized for their model |
> | On-device (phones) | LCM / DDIM | 4-8 | Speed is king on mobile 📱 |

---

# 📝 Summary

| Aspect | DDPM | DDIM |
|--------|------|------|
| Steps | T (1000) | Configurable (10-100) |
| Noise | Stochastic | Deterministic (η=0) |
| Quality at few steps | Degrades fast | Graceful degradation |
| Inversion | Not possible | Yes (deterministic) |
| Interpolation | Not meaningful | Smooth latent space |

---

# 🧠 DDPM vs DDIM Cheat Sheet

> **Print this. Pin it. Refer to it often.** ⭐

| Question | DDPM 🐢 | DDIM 🏎️ |
|----------|---------|----------|
| **How many steps?** | Always 1000 | You choose! (10-100) |
| **Is the output random?** | Yes — different each time | No (η=0) — same noise → same image |
| **Can I skip steps?** | No — quality falls apart | Yes — built for it! |
| **Can I go backwards (inversion)?** | No | Yes — encode real images to noise |
| **Can I blend two images?** | Not meaningfully | Yes — smooth latent interpolation |
| **How fast?** | ~30 sec/image | ~1.5 sec/image (50 steps) |
| **Production ready?** | ❌ Too slow | ✅ Foundation of all fast samplers |
| **When η=1?** | — | Becomes DDPM! |

> 🔑 **The One-Line Summary:**
> DDIM = DDPM but with a **deterministic shortcut** and a **speed dial** ($\eta$). Set $\eta = 0$ for maximum speed, $\eta = 1$ to recover original DDPM.

---

# ⭐ Key Formulas Quick Reference (LaTeX)

| Formula | LaTeX | What It Does |
|---------|-------|-------------|
| DDPM reverse step | $x_{t-1} = \frac{1}{\sqrt{\alpha_t}} \left( x_t - \frac{\beta_t}{\sqrt{1-\bar{\alpha}_t}} \epsilon_\theta \right) + \sigma_t z$ | One noisy step backward (stochastic) |
| DDIM reverse step | $x_{t-1} = \sqrt{\bar{\alpha}_{t-1}} \hat{x}_0 + \sqrt{1 - \bar{\alpha}_{t-1} - \sigma_t^2} \epsilon_\theta + \sigma_t z$ | One step backward (tunable noise via $\eta$) |
| DDIM deterministic ($\eta=0$) | $x_{t-1} = \sqrt{\bar{\alpha}_{t-1}} \hat{x}_0 + \sqrt{1 - \bar{\alpha}_{t-1}} \epsilon_\theta$ | One step backward (no noise!) |
| Predicted clean image | $\hat{x}_0 = \frac{x_t - \sqrt{1-\bar{\alpha}_t} \epsilon_\theta}{\sqrt{\bar{\alpha}_t}}$ | Neural net's best guess at final image |
| Noise coefficient | $\sigma_t = \eta \sqrt{\frac{1-\bar{\alpha}_{t-1}}{1-\bar{\alpha}_t}} \sqrt{1 - \frac{\bar{\alpha}_t}{\bar{\alpha}_{t-1}}}$ | Controls randomness ($\eta$ dial) |

---

# 📚 Resources

## 📄 Essential Papers

| Paper | Authors | Year | Why Read It |
|-------|---------|:----:|------------|
| [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239) | Ho, Jain, Abbeel | 2020 | ⭐ The foundational DDPM paper — started the modern diffusion era |
| [Denoising Diffusion Implicit Models](https://arxiv.org/abs/2010.02502) | Song, Meng, Ermon | 2021 | ⭐ The DDIM paper — deterministic sampling + fast generation |
| [DPM-Solver](https://arxiv.org/abs/2206.00927) | Lu et al. | 2022 | Fast ODE solver — 10-step high-quality generation |
| [Elucidating the Design Space of Diffusion-Based Generative Models (EDM)](https://arxiv.org/abs/2206.00364) | Karras, Aittala, Aila, Laine | 2022 | Unified framework, best practices for sampler design |
| [Pseudo Numerical Methods for Diffusion Models (PNDM/PLMS)](https://arxiv.org/abs/2202.09778) | Liu et al. | 2022 | Multi-step methods for faster sampling |
| [UniPC](https://arxiv.org/abs/2302.04867) | Zhao et al. | 2023 | Unified predictor-corrector, state-of-art few-step sampling |

## 📖 Books & Surveys

| Resource | Type | Best For |
|----------|------|----------|
| *Understanding Deep Learning* — Simon J.D. Prince (Ch. 18: Diffusion) | Textbook | Rigorous mathematical foundation |
| *Deep Learning* — Goodfellow, Bengio, Courville | Textbook | Prerequisite ML/DL knowledge |
| [Lil'Log: What Are Diffusion Models?](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/) | Blog | Best intuitive explanation on the internet |
| [Hugging Face Diffusion Models Course](https://huggingface.co/learn/diffusion-course) | Course | Hands-on coding with `diffusers` library |
| *A Survey on Diffusion Models* — Yang et al. (2023) | Survey | Comprehensive overview of all sampler variants |

## 🛠️ Practical Implementation Resources

| Resource | What You'll Learn |
|----------|------------------|
| [HuggingFace `diffusers` library](https://github.com/huggingface/diffusers) | Production-grade DDIM, DPM-Solver, Euler implementations |
| [Lucidrains `denoising-diffusion-pytorch`](https://github.com/lucidrains/denoising-diffusion-pytorch) | Clean PyTorch DDPM/DDIM from scratch |
| [Stable Diffusion WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) | See samplers in action with a GUI |

---

**Tomorrow**: Latent Diffusion & Stable Diffusion Architecture.
