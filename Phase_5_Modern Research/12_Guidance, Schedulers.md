📘 DAY 12 — Advanced Sampling: Guidance, Schedulers & Production Techniques

Goal:

Master production-grade diffusion sampling — advanced guidance strategies, modern ODE/SDE schedulers, and the practical techniques that make real systems fast and high-quality.

---

# 🌟 The Big Picture — What This Day Is Really About

> **One-sentence summary**: Today you learn the *steering wheel* and *GPS* of AI image generation — how to tell the AI what you want, what to avoid, and how to get there fast.

Imagine you hired a brilliant but eccentric artist 🎨. They can paint *anything*, but:
- They need **directions** ("paint a sunset over mountains") — that's the **prompt + guidance**
- They need an **"avoid" list** ("no clouds, no people") — that's the **negative prompt**
- They need a **work plan** ("sketch first, then refine colors, then details") — that's the **scheduler**
- You decide how many **revision rounds** they get (5 quick sketches? 50 careful refinements?) — that's the **step count**

This day teaches you how all of these knobs work together.

---

## 🧭 Why Should I Care?

| If you are… | This day helps you… |
|---|---|
| 🎨 An artist using Stable Diffusion | Pick the right scheduler and CFG for stunning images |
| 💻 A developer building AI products | Choose settings that balance quality vs. speed vs. cost |
| 🔬 A researcher | Understand the math behind ODE solvers for diffusion |
| 🏢 A founder | Know what settings your engineers are tuning and why |

---

## 📋 Prerequisites (Plain English)

| Concept | What it means | Where you learned it |
|---|---|---|
| Diffusion model | An AI that learns to remove noise from images | Day 7-8 |
| Latent space | A compressed "idea space" where images live as small grids of numbers | Day 10 |
| Forward process | Gradually adding noise until an image becomes pure static | Day 7 |
| Reverse process | Gradually removing noise to create an image from static | Day 7 |
| Text conditioning | Telling the model what to generate using words | Day 11 |

---

# 1️⃣ Classifier-Free Guidance: Deep Dive

## 🧒 Beginner Analogy: The Volume Knob on a Radio 📻

Imagine you're listening to two radio stations at once:
- **Station A** plays "random noise" (what the AI would draw with no instructions)
- **Station B** plays "your request" (what the AI would draw following your prompt perfectly)

The **CFG scale** ($w$) is the volume knob:
- $w = 1$: Both stations at equal volume → blurry, vague images (AI barely listens to you)
- $w = 7.5$: Station B much louder → sharp, prompt-following images (sweet spot! ⭐)
- $w = 20$: Station B deafeningly loud → oversaturated, weird artifacts (AI is *trying too hard*)

> ⭐ **Key insight**: CFG scale controls *how strictly the AI follows your text prompt*. Too low = ignores you. Too high = overdoes it.

---

### 🔢 The Math (with LaTeX)

Review: ε̂ = ε_uncond + w · (ε_cond - ε_uncond)

$$\hat{\epsilon} = \epsilon_{\text{uncond}} + w \cdot (\epsilon_{\text{cond}} - \epsilon_{\text{uncond}})$$

Equivalently: ε̂ = (1-w) · ε_uncond + w · ε_cond

$$\hat{\epsilon} = (1 - w) \cdot \epsilon_{\text{uncond}} + w \cdot \epsilon_{\text{cond}}$$

Where:
- $\epsilon_{\text{uncond}}$ = the noise the model predicts with **no text** ("what would I draw if nobody told me anything?")
- $\epsilon_{\text{cond}}$ = the noise the model predicts **given your prompt** ("what would I draw for 'a cat on the moon'?")
- $w$ = **guidance scale** (typically 1.0 to 20.0)
- $\hat{\epsilon}$ = the **final guided noise prediction** used to denoise the image

### 📊 CFG Scale Quick Reference

| CFG Scale ($w$) | Effect | When to use |
|---|---|---|
| 1.0 | No guidance — random, diverse | Never in practice |
| 3.0–5.0 | Soft guidance — creative, loose | Artistic/abstract images |
| ⭐ **7.0–8.5** | **Sweet spot** — sharp + coherent | **Default for most uses** |
| 10.0–15.0 | Strong guidance — very literal | Exact prompt matching |
| 15.0+ | Over-guided — artifacts, saturation | Avoid unless experimenting |

> 📖 **Research**: Ho & Salimans (2022) "Classifier-Free Diffusion Guidance" showed that this simple formula (no separate classifier needed) works better than the original classifier-guided approach from Dhariwal & Nichol (2021).

---

## Dynamic Guidance

### 🧒 Analogy: Coaching a Painter Through Stages 🖌️

Imagine coaching a painter through a mural:
- **Early stage** (rough sketch): Give STRONG directions → "The mountain goes HERE, the river goes THERE" (high $w$)
- **Late stage** (fine details): Back off → "Just add nice leaf textures, you know what looks good" (low $w$)

Dynamic guidance does exactly this — it **changes the CFG scale during the generation process**.

Instead of constant w, vary it across timesteps:

$$w(t) = w_{\min} + (w_{\max} - w_{\min}) \cdot \frac{t}{T}$$

```python
def dynamic_cfg_scale(t, T, w_min=1.0, w_max=7.5, schedule='linear'):
    """Higher guidance early (structure), lower later (detail)."""
    progress = t / T  # 1.0 at start, 0.0 at end
    if schedule == 'linear':
        return w_min + (w_max - w_min) * progress
    elif schedule == 'cosine':
        return w_min + (w_max - w_min) * (1 + np.cos(np.pi * (1-progress))) / 2
```

> ⭐ **Why this helps**: High guidance early locks in the *composition* (where things are). Lower guidance later preserves *fine details* (textures, subtle colors) that high CFG would wash out.

| Schedule | Formula | Effect |
|---|---|---|
| Linear | $w(t) = w_{\min} + (w_{\max} - w_{\min}) \cdot \frac{t}{T}$ | Smooth ramp-down |
| Cosine | $w(t) = w_{\min} + (w_{\max} - w_{\min}) \cdot \frac{1 + \cos(\pi(1 - t/T))}{2}$ | Gentle start and end |

---

## Negative Prompts

### 🧒 Analogy: The "Avoid List" for Your Artist 🚫

You're commissioning a portrait and you tell the artist:
- ✅ **Positive prompt**: "A photorealistic portrait of a woman in sunlight"
- 🚫 **Negative prompt**: "blurry, bad anatomy, extra fingers, low quality, cartoonish"

The negative prompt is like handing the artist a list of things to *actively run away from*. Instead of comparing "your request" vs. "nothing", the AI compares "your request" vs. "the bad stuff" — making the contrast even stronger.

The unconditional prediction can be replaced with a NEGATIVE prompt:

ε̂ = ε_neg + w · (ε_cond - ε_neg)

$$\hat{\epsilon} = \epsilon_{\text{neg}} + w \cdot (\epsilon_{\text{cond}} - \epsilon_{\text{neg}})$$

This steers AWAY from the negative prompt while steering TOWARD the positive prompt.

Where:
- $\epsilon_{\text{neg}}$ = what the model predicts when conditioned on the **negative** (bad) prompt
- $\epsilon_{\text{cond}}$ = what the model predicts for the **positive** (desired) prompt
- The model moves in the direction **from** $\epsilon_{\text{neg}}$ **toward** $\epsilon_{\text{cond}}$

```python
def sample_with_negative(model, z_t, t, pos_emb, neg_emb, guidance_scale=7.5):
    eps_neg = model(z_t, t, neg_emb)
    eps_pos = model(z_t, t, pos_emb)
    return eps_neg + guidance_scale * (eps_pos - eps_neg)
```

### 🏭 Production Tip: Common Negative Prompt Templates

| Use Case | Recommended Negative Prompt |
|---|---|
| Photorealistic portraits | "blurry, bad anatomy, extra fingers, deformed, low quality, watermark" |
| Landscapes | "text, watermark, low resolution, oversaturated, blurry" |
| Product photography | "blurry, distorted, text overlay, low quality, cartoon" |
| Anime/illustration | "realistic, photo, 3d render, blurry, bad proportions" |

> ⭐ **Without a negative prompt**, the model compares your request against "nothing" (empty prompt). **With a negative prompt**, it compares your request against "things you hate" — the contrast is much sharper, producing cleaner results.

---

# 2️⃣ Modern ODE Solvers for Diffusion

## 🧒 Beginner Analogy: GPS Route Planners 🗺️

Imagine you're driving from City A (pure noise) to City B (your final image). A **scheduler** (also called a "solver") is your **GPS route planner**. Different GPS apps suggest different routes:

| GPS App (Scheduler) | Route Style | Analogy |
|---|---|---|
| 🐢 **Euler** | Take tiny baby steps, stop and check the map constantly | Slow but simple — like walking with Google Maps |
| 🚗 **Heun** | Look ahead at the next intersection before deciding | Smoother drive, uses more fuel (compute) per turn |
| 🚀 **DPM-Solver++** | Use a smart shortcut algorithm | Arrives in 15-20 turns instead of 50+! |
| ✈️ **UniPC** | Unified express route | Gets there in 10-20 turns |
| 🎲 **Euler Ancestral** | Scenic random route with surprises | More creative, but takes longer |

> ⭐ **Key insight**: ALL schedulers start from the same place (noise) and end at the same destination (your image). They just take **different paths** to get there. Faster schedulers use math tricks to skip unnecessary stops.

---

### 🔢 The Core Idea (Plain English + Math)

Diffusion sampling = solving an ODE from t=T to t=0.

**In plain English**: The model starts with random TV static and needs to figure out how to remove the noise step by step. This process follows a mathematical equation called an **ODE** (Ordinary Differential Equation). Different schedulers are different methods for solving this equation.

$$\frac{dx}{dt} = f(x_t, t)$$

This says: "The rate at which the image changes ($\frac{dx}{dt}$) depends on what the image currently looks like ($x_t$) and what step we're at ($t$)."

---

### Euler Method (First Order)

🧒 **Analogy**: Taking one step, looking at your compass, taking another step. Very simple but you might zigzag.

x_{t-Δt} = x_t + Δt · f(x_t, t)

$$x_{t - \Delta t} = x_t + \Delta t \cdot f(x_t, t)$$

Where:
- $x_t$ = current noisy image
- $\Delta t$ = step size (how much time we advance)
- $f(x_t, t)$ = the direction the neural network tells us to go (its noise prediction)

Simple but requires many steps (50+).

> 📖 **Named after** Leonhard Euler (1707-1783), the most prolific mathematician in history. This is the simplest possible ODE solver.

---

### Heun's Method (Second Order)

🧒 **Analogy**: Before committing to a step, you "peek ahead" to see where you'd end up, then average the direction. Like test-driving two routes and picking the middle path.

Predictor: x̃ = x_t + Δt · f(x_t, t)
Corrector: x_{t-Δt} = x_t + Δt/2 · [f(x_t, t) + f(x̃, t-Δt)]

$$\tilde{x} = x_t + \Delta t \cdot f(x_t, t) \quad \text{(predict)}$$
$$x_{t - \Delta t} = x_t + \frac{\Delta t}{2} \left[ f(x_t, t) + f(\tilde{x}, t - \Delta t) \right] \quad \text{(correct)}$$

Two model evaluations per step, but better accuracy.

> ⭐ **Tradeoff**: Each step costs 2× the compute (two neural network calls), but you need fewer total steps. Often worth it!

---

### DPM-Solver (Lu et al., 2022)

🧒 **Analogy**: Instead of blindly following the road, DPM-Solver looks at the *shape of the highway* and realizes it can take mathematical shortcuts. It's like knowing the road is a straight line for the next 10 miles, so you can skip checking the GPS every mile.

Exploits the semi-linear structure of the diffusion ODE:

The key insight is that the diffusion ODE can be decomposed into a **linear part** (which can be solved exactly) and a **nonlinear part** (which is approximated). This is why it's so much faster.

$$\lambda_t = \log \frac{\alpha_t}{\sigma_t} \quad \text{(log signal-to-noise ratio)}$$

```python
def dpm_solver_2(model, x, timesteps, diffusion):
    """Second-order DPM-Solver."""
    for i in range(len(timesteps) - 1):
        t_curr, t_next = timesteps[i], timesteps[i+1]
        
        # lambda = log(alpha/sigma) — the log-SNR
        lambda_curr = log_snr(diffusion, t_curr)
        lambda_next = log_snr(diffusion, t_next)
        h = lambda_next - lambda_curr
        
        # First-order estimate
        eps = model(x, t_curr)
        x_1 = transfer(x, eps, t_curr, t_next, diffusion)
        
        # Second-order correction
        t_mid = find_t_for_lambda(lambda_curr + h/2, diffusion)
        eps_mid = model(x_1, t_mid)
        x = transfer(x, eps_mid, t_curr, t_next, diffusion)
    
    return x
```

Achieves DDPM quality in 10-20 steps!

> 📖 **Research**: Lu et al. (2022) "DPM-Solver: A Fast ODE Solver for Diffusion Probabilistic Model Sampling in Around 10 Steps" — This paper revolutionized diffusion speed. Follow-up **DPM-Solver++** (Lu et al., 2022b) added support for guided sampling and is the default in many production systems.

### 🔮 PNDM (Pseudo Numerical Methods for Diffusion)

🧒 **Analogy**: Instead of calculating a fresh direction every step, PNDM looks at its *recent history* of directions and extrapolates. Like a driver who smooths out their steering by remembering their last few turns.

- Uses **multi-step methods** (Adams-Bashforth style) that reuse previous model outputs
- Fewer neural network evaluations needed per step
- Was the default scheduler in early Stable Diffusion releases

> 📖 **Research**: Liu et al. (2022) "Pseudo Numerical Methods for Diffusion Models on Manifolds"

### 🌐 UniPC (Unified Predictor-Corrector)

🧒 **Analogy**: UniPC is the *Swiss Army knife* scheduler. It combines the "predict" and "correct" approach of Heun with the history-reuse of PNDM, and wraps it in a unified framework that automatically adapts.

- Achieves high quality in **10-20 steps**
- Works with both noise-prediction ($\epsilon$-prediction) and data-prediction ($x_0$-prediction) models
- Requires NO model-specific tuning

> 📖 **Research**: Zhao et al. (2023) "UniPC: A Unified Predictor-Corrector Framework for Fast Sampling of Diffusion Models" — Achieved state-of-the-art FID scores with fewer steps than any previous solver.

---

### ⚡ Steps vs. Quality Tradeoff

> ⭐ **The fundamental tradeoff**: More steps = higher quality but slower. Fewer steps = faster but may look blurry or have artifacts.

| Steps | Speed | Quality | Use Case |
|---|---|---|---|
| 1-4 | ⚡⚡⚡⚡ | ⭐ | Real-time previews, distilled models (LCM/Turbo) |
| 10-15 | ⚡⚡⚡ | ⭐⭐⭐ | Fast drafts with DPM-Solver++/UniPC |
| 20-30 | ⚡⚡ | ⭐⭐⭐⭐ | ⭐ **Production sweet spot** |
| 50-100 | ⚡ | ⭐⭐⭐⭐⭐ | Maximum quality for final renders |
| 150+ | 🐢 | ⭐⭐⭐⭐⭐ | Diminishing returns — usually not worth it |

---

# 3️⃣ Scheduler Comparison

### 🧒 What does "Order" mean?

Think of "order" like the *sophistication* of a calculator:
- **1st order** = basic calculator (adds one step at a time)
- **2nd order** = scientific calculator (considers the *curve* of the path)
- **3rd order** = graphing calculator (models even more complex path shapes)

Higher order = fewer steps needed, but each step is more complex (more neural network calls).

### 🧒 What is NFE?

**NFE** = **N**umber of **F**unction **E**valuations = how many times the neural network runs during the entire generation. Each call costs GPU time and money, so lower NFE = cheaper and faster.

| Scheduler | Order | NFE for Good Quality | Best For | 🧒 Plain English |
|-----------|-------|---------------------|----------|---|
| DDPM | 1 | 1000 | Reference quality | The original, painfully slow method |
| DDIM | 1 | 50-100 | Deterministic, interpolation | Same seed → same image, enables smooth morphing |
| Euler | 1 | 50-100 | Simple, reliable | The reliable Honda Civic of schedulers 🚗 |
| Heun | 2 | 25-50 (×2 eval) | Better quality per step | Peek-ahead for smoother results |
| DPM-Solver++ | 2-3 | 15-25 | Fast production sampling | ⭐ **Production workhorse** — fast + high quality |
| UniPC | 2+ | 10-20 | Unified approach | Swiss Army knife, great with few steps |
| PNDM | 2 | 25-50 | Legacy compatibility | Uses history for efficiency |
| Euler Ancestral | 1 | 50-100 | More diverse/creative | Adds randomness for variety 🎲 |
| LCM | 1 | 4-8 | Distilled models | Blazing fast but needs special model ⚡ |

NFE = Number of Function Evaluations (model forward passes).

---

### 🏭 Production: Which Scheduler Should I Use?

**Decision tree for practitioners:**

```
Need < 5 steps?              → LCM / SDXL Turbo (needs distilled model)
Need 10-20 steps?            → DPM-Solver++ or UniPC ⭐
Need deterministic output?   → DDIM or DPM-Solver++ (no ancestral noise)
Need maximum creativity?     → Euler Ancestral (50+ steps)
Need maximum quality?        → DPM-Solver++ with Karras schedule, 25-30 steps
```

### 🔧 Scheduler Settings in Popular Tools

| Tool | Where to set scheduler | Recommended defaults |
|---|---|---|
| **Automatic1111 (A1111)** | "Sampling method" dropdown | DPM++ 2M Karras, 25 steps |
| **ComfyUI** | KSampler node → "sampler_name" + "scheduler" | dpmpp_2m + karras |
| **Stable Diffusion WebUI Forge** | Settings panel | DPM++ 2M SDE Karras |
| **diffusers (Python)** | `DPMSolverMultistepScheduler` | `use_karras_sigmas=True` |
| **InvokeAI** | Generation tab → Scheduler | DPM++ 2M Karras |

> 📖 **Research benchmark**: Karras et al. (2022) "Elucidating the Design Space of Diffusion-Based Generative Models" (EDM) systematically tested schedulers and found that DPM-Solver with their proposed noise schedule consistently outperformed alternatives at low step counts.

---

# 4️⃣ Karras Schedule (Karras et al., 2022)

## 🧒 Beginner Analogy: Spending Time Where It Matters 🎯

Imagine you're cleaning a very messy room:
- **Very messy** (high noise): You can quickly throw big trash bags out — big visible improvement per minute
- **Mostly clean** (low noise): Polishing tiny details takes disproportionate effort
- **Medium messy** (medium noise): This is where your effort **matters most** — this is where careful cleaning transforms "okay" into "great"

The **Karras schedule** is smart about where to spend your "cleaning steps":
- Instead of spacing steps evenly, it **concentrates steps at medium noise levels** where quality gains are largest
- It spends fewer steps at very high noise (big changes are easy) and very low noise (small polishing)

> ⭐ **This is why "Karras" versions** of schedulers (like "DPM++ 2M Karras") are almost always better than their non-Karras counterparts.

---

### 🔢 The Math

Better noise schedule for sampling:

σ_i = (σ_max^(1/ρ) + (i/(N-1)) · (σ_min^(1/ρ) - σ_max^(1/ρ)))^ρ

$$\sigma_i = \left( \sigma_{\max}^{1/\rho} + \frac{i}{N-1} \cdot \left( \sigma_{\min}^{1/\rho} - \sigma_{\max}^{1/\rho} \right) \right)^{\rho}$$

Where:
- $\sigma_i$ = noise level at step $i$
- $\sigma_{\max}$ = starting noise level (default: 80)
- $\sigma_{\min}$ = ending noise level (default: 0.002)
- $\rho$ = controls how much to concentrate steps in the middle (default: 7)
- $N$ = total number of steps

```python
def karras_schedule(n_steps, sigma_min=0.002, sigma_max=80, rho=7):
    ramp = torch.linspace(0, 1, n_steps)
    min_inv_rho = sigma_min ** (1/rho)
    max_inv_rho = sigma_max ** (1/rho)
    sigmas = (max_inv_rho + ramp * (min_inv_rho - max_inv_rho)) ** rho
    return sigmas
```

Concentrates steps where they matter most (medium noise levels).

### 📊 Karras vs. Linear Schedule Comparison

| Property | Linear Schedule | Karras Schedule |
|---|---|---|
| Step distribution | Evenly spaced | Concentrated at medium noise |
| Quality at 20 steps | Good | ⭐ **Better** |
| Extra compute cost | None | None (just different spacing) |
| Default in production? | Old default | ⭐ **New standard** |

> 📖 **Research**: Karras et al. (2022) "Elucidating the Design Space of Diffusion-Based Generative Models" — This NVIDIA paper didn't just propose a schedule, it systematically analyzed *every design choice* in diffusion sampling. A landmark paper for the field.

---

# 5️⃣ Stochastic vs Deterministic Sampling

## 🧒 Beginner Analogy: Two Types of Artists 🎨

| | Deterministic Artist | Stochastic Artist |
|---|---|---|
| **Working style** | Follows a precise plan. Same instructions = same painting every time. | Flips coins during work. Same instructions = different painting each time. |
| **Personality** | Reliable, predictable, consistent | Creative, surprising, varied |
| **Good for** | Reproducible results, editing workflows | Exploration, generating diverse options |
| **Downside** | Can feel "samey" across generations | Needs more steps, less predictable |

---

**Deterministic** (DDIM, Euler, DPM-Solver):
- Same noise → same image
- Enables editing, interpolation
- Fewer steps needed

> 🔑 **Deterministic means**: given the same random seed + same prompt + same settings → you get the **exact same image** every time. This is critical for **reproducibility** and **editing workflows** (like img2img).

**Stochastic** (DDPM, Euler Ancestral, SDE solvers):
- Adds noise at each step
- More diverse outputs
- Can produce more creative/varied results
- Needs more steps for quality

> 🔑 **Stochastic means**: at each denoising step, the scheduler injects a small amount of *fresh random noise*. This is like the artist occasionally doodling a random squiggle — it creates variety but needs more cleanup steps.

### 🔢 The Math Behind Stochastic Noise Injection

At each step, the stochastic sampler splits the noise into two parts:

$$\sigma_{\text{down}} = \sqrt{\sigma_{\text{next}}^2 - \sigma_{\text{up}}^2}$$

$$\sigma_{\text{up}} = \sigma_{\text{next}} \sqrt{1 - \left(\frac{\sigma_{\text{next}}}{\sigma_{\text{curr}}}\right)^2}$$

- $\sigma_{\text{down}}$: the amount of noise *removed* (denoising)
- $\sigma_{\text{up}}$: the amount of *fresh* noise added back (the "stochastic" part)

---

## Ancestral Sampling

🧒 **Analogy**: The artist removes some paint, then splashes a tiny bit of random paint back. Each generation takes a slightly different creative detour.

```python
def euler_ancestral_step(model, x, t_curr, t_next, diffusion):
    eps = model(x, t_curr)
    sigma_curr = get_sigma(diffusion, t_curr)
    sigma_next = get_sigma(diffusion, t_next)
    sigma_up = sigma_next * torch.sqrt(1 - (sigma_next/sigma_curr)**2)
    sigma_down = torch.sqrt(sigma_next**2 - sigma_up**2)
    
    d = (x - pred_x0(x, eps, sigma_curr)) / sigma_curr
    x = x + d * (sigma_down - sigma_curr)
    x = x + sigma_up * torch.randn_like(x)  # ancestral noise
    return x
```

### 🏭 Production: When to Choose Which?

| Scenario | Use Deterministic? | Use Stochastic? | Why |
|---|---|---|---|
| User wants same image from same seed | ✅ | ❌ | Reproducibility required |
| Generating 100 candidates to pick best | ❌ | ✅ | Want maximum diversity |
| Image editing / inpainting | ✅ | ❌ | Need controlled, consistent changes |
| Generating training data | ❌ | ✅ | Diversity improves dataset quality |
| API with "regenerate" button | ✅ | ✅ | Deterministic for "again", stochastic for "surprise me" |

---

# 6️⃣ Production Pipeline

## 🧒 Beginner Analogy: The Assembly Line 🏭

Think of the production pipeline as a **factory assembly line** with 4 stations:

```
📝 Text Encoder    →    🎲 Noise Generator    →    🔄 Denoising Loop    →    🖼️ VAE Decoder
"a cute cat"            Random static            Remove noise step         Uncompress to
→ numbers               in latent space          by step with CFG          full HD image
```

Each station does one job, and the image moves through them in order.

---

### 🔍 Code Walkthrough (Line by Line)

```python
class ProductionSampler:
    def __init__(self, vae, unet, text_encoder, scheduler='dpm_solver_pp'):
        self.vae = vae               # Decoder: converts compressed latent → full image
        self.unet = unet             # The denoising brain: predicts noise to remove
        self.text_enc = text_encoder # Converts text prompt → embedding numbers
        self.scheduler = scheduler   # The GPS route planner (scheduler algorithm)
    
    @torch.no_grad()                 # Don't compute gradients (saves memory, faster)
    @torch.autocast('cuda', dtype=torch.float16)  # Use half precision (2× faster)
    def generate(self, prompt, negative_prompt="", num_steps=25,
                 guidance_scale=7.5, width=512, height=512, seed=None):
        if seed is not None:
            torch.manual_seed(seed)  # Set seed for reproducibility
        
        # STATION 1: Encode text → numbers the model understands
        pos_emb = self.text_enc(prompt)           # "a cute cat" → [0.23, -0.87, ...]
        neg_emb = self.text_enc(negative_prompt)   # "blurry" → [0.11, 0.45, ...]
        
        # STATION 2: Start from pure random noise in latent space
        # height//8 because latent space is 8× compressed
        latent = torch.randn(1, 4, height//8, width//8, device='cuda')
        
        # STATION 3: The denoising loop
        timesteps = self.get_schedule(num_steps)  # [999, 950, 900, ..., 0]
        
        for t in timesteps:
            t_batch = torch.tensor([t], device='cuda')
            
            # CFG: run the model TWICE (negative + positive) in one batch
            latent_input = torch.cat([latent, latent])     # Stack: [neg_input, pos_input]
            t_input = torch.cat([t_batch, t_batch])         
            context = torch.cat([neg_emb, pos_emb])         
            
            eps = self.unet(latent_input, t_input, context) # One batched forward pass
            eps_neg, eps_pos = eps.chunk(2)                 # Split results
            
            # Apply CFG formula: steer toward prompt, away from negative
            eps_guided = eps_neg + guidance_scale * (eps_pos - eps_neg)
            
            # Scheduler step: update the latent using the guided prediction
            latent = self.scheduler_step(latent, eps_guided, t)
        
        # STATION 4: Decode compressed latent → full-resolution image
        image = self.vae.decode(latent)
        return (image.clamp(-1, 1) + 1) / 2  # Normalize to [0, 1] range
```

### 🏭 Production Settings by Use Case

| Use Case | Scheduler | Steps | CFG | Resolution | Speed (A100) |
|---|---|---|---|---|---|
| Real-time preview | LCM | 4-8 | 1.0-2.0 | 512×512 | ~50ms |
| Fast production | DPM++ 2M Karras | 20 | 7.5 | 512×512 | ~1s |
| High quality | DPM++ 2M Karras | 30 | 7.5 | 1024×1024 | ~3s |
| Maximum quality | DPM++ SDE Karras | 50 | 7.5-9.0 | 1024×1024 | ~8s |
| Batch generation | Euler | 25 | 7.5 | 512×512 | ~1.2s |

### 🔧 Diffusers Library Quick Start

```python
# Real production code using HuggingFace diffusers
from diffusers import StableDiffusionPipeline, DPMSolverMultistepScheduler
import torch

pipe = StableDiffusionPipeline.from_pretrained(
    "stabilityai/stable-diffusion-2-1",
    torch_dtype=torch.float16
).to("cuda")

# Swap in the fast scheduler
pipe.scheduler = DPMSolverMultistepScheduler.from_config(
    pipe.scheduler.config,
    use_karras_sigmas=True,   # Use Karras noise schedule
    algorithm_type="dpmsolver++"  # DPM-Solver++ variant
)

# Generate!
image = pipe(
    prompt="a photorealistic sunset over mountains",
    negative_prompt="blurry, low quality, watermark",
    num_inference_steps=25,
    guidance_scale=7.5
).images[0]
```

---

# 📝 Summary

| Technique | Key Impact |
|-----------|-----------|
| CFG | Quality-diversity tradeoff via guidance scale |
| Negative prompts | Steer away from unwanted content |
| DPM-Solver++ | 15-25 step high-quality sampling |
| Karras schedule | Better step distribution |
| Ancestral sampling | More diverse but needs more steps |

---

# 🧾 Cheat Sheet — Copy This for Quick Reference

```
╔══════════════════════════════════════════════════════════════════╗
║          DIFFUSION SAMPLING CHEAT SHEET                        ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                ║
║  CFG SCALE (guidance_scale / w)                                ║
║  ─────────────────────────────                                 ║
║  1.0     = AI ignores your prompt                              ║
║  3-5     = Soft, creative, artistic                            ║
║  7-8.5   = ⭐ SWEET SPOT (start here)                          ║
║  10-15   = Very literal, may over-saturate                     ║
║  15+     = Artifacts likely, avoid                             ║
║                                                                ║
║  SCHEDULER PICK                                                ║
║  ──────────────                                                ║
║  Default:        DPM++ 2M Karras (25 steps)                    ║
║  Fastest:        LCM (4-8 steps, needs distilled model)        ║
║  Most creative:  Euler Ancestral (50+ steps)                   ║
║  Best quality:   DPM++ SDE Karras (30-50 steps)                ║
║                                                                ║
║  NEGATIVE PROMPTS                                              ║
║  ────────────────                                              ║
║  Always use them! Start with:                                  ║
║  "blurry, low quality, bad anatomy, watermark, text"           ║
║                                                                ║
║  STEPS                                                         ║
║  ─────                                                         ║
║  Too few (<10):   Blurry/incomplete                            ║
║  Sweet spot:      20-30 with DPM++/UniPC                       ║
║  Diminishing:     >50 rarely helps                             ║
║                                                                ║
╚══════════════════════════════════════════════════════════════════╝
```

---

# 🗺️ How Everything Connects

```
    Text Prompt ──────┐
                      │
    Negative Prompt ──┤    CFG Scale (w)
                      │    controls the
                      ▼    blend strength
              ┌──────────────┐
              │  CFG Formula │──→ Guided noise prediction (ε̂)
              └──────────────┘
                      │
                      ▼
              ┌──────────────┐
              │  Scheduler   │──→ Updates latent image
              │ (DPM++/Euler │    step by step
              │  /UniPC/...) │
              └──────────────┘
                      │        Karras schedule decides
                      │        WHERE to place each step
                      ▼
              ┌──────────────┐
              │  VAE Decode  │──→ Final image! 🖼️
              └──────────────┘
```

---

# 🔑 Key Equations Summary (LaTeX)

| Name | Equation |
|---|---|
| CFG formula | $\hat{\epsilon} = \epsilon_{\text{uncond}} + w (\epsilon_{\text{cond}} - \epsilon_{\text{uncond}})$ |
| CFG with negative prompt | $\hat{\epsilon} = \epsilon_{\text{neg}} + w (\epsilon_{\text{cond}} - \epsilon_{\text{neg}})$ |
| Euler step | $x_{t-\Delta t} = x_t + \Delta t \cdot f(x_t, t)$ |
| Heun predictor | $\tilde{x} = x_t + \Delta t \cdot f(x_t, t)$ |
| Heun corrector | $x_{t-\Delta t} = x_t + \frac{\Delta t}{2}[f(x_t, t) + f(\tilde{x}, t-\Delta t)]$ |
| Karras schedule | $\sigma_i = \left(\sigma_{\max}^{1/\rho} + \frac{i}{N-1}(\sigma_{\min}^{1/\rho} - \sigma_{\max}^{1/\rho})\right)^{\rho}$ |
| Log-SNR | $\lambda_t = \log \frac{\alpha_t}{\sigma_t}$ |

---

# 📚 Resources

## 📄 Key Papers

| Paper | Authors | Year | Key Contribution |
|---|---|---|---|
| Classifier-Free Diffusion Guidance | Ho & Salimans | 2022 | The CFG formula used everywhere today |
| Elucidating the Design Space of Diffusion-Based Generative Models (EDM) | Karras et al. | 2022 | Karras schedule, systematic sampler analysis (NVIDIA) |
| DPM-Solver: A Fast ODE Solver for Diffusion | Lu et al. | 2022 | 10-step high quality sampling |
| DPM-Solver++: Fast Solver for Guided Sampling | Lu et al. | 2022 | Extended DPM-Solver for CFG, production standard |
| UniPC: A Unified Predictor-Corrector Framework | Zhao et al. | 2023 | Unified framework, 10-step SOTA quality |
| Pseudo Numerical Methods for Diffusion (PNDM) | Liu et al. | 2022 | Multi-step methods for diffusion |
| Denoising Diffusion Implicit Models (DDIM) | Song et al. | 2020 | Deterministic sampling, interpolation |
| Latent Consistency Models (LCM) | Luo et al. | 2023 | 4-8 step generation via distillation |

## 📖 Books

| Book | Authors | Why read it |
|---|---|---|
| *Understanding Deep Learning* | Simon J.D. Prince (2023) | Ch. 18 covers diffusion models with clear diagrams |
| *Probabilistic Machine Learning: Advanced Topics* | Kevin Murphy (2023) | Rigorous treatment of score-based models and SDEs |
| *Deep Learning* | Goodfellow, Bengio, Courville | Foundational — Ch. 20 on generative models |

## 🔗 Practical Guides & Benchmarks

| Resource | Description |
|---|---|
| HuggingFace Diffusers Docs | Scheduler comparison guide with code examples |
| Stable Diffusion Sampler Comparison | Community benchmark images across all schedulers at various step counts |
| ComfyUI Scheduler Guide | Visual node-based workflow examples |
| AUTOMATIC1111 Wiki | Comprehensive sampler documentation |
| Civitai Beginner Guide | Community guides for optimal settings per model |

## 🎥 Video Explanations

| Video | Why watch it |
|---|---|
| "How Diffusion Models Work" — Computerphile | Visual intuition for the entire process |
| "Stable Diffusion Samplers Explained" — Sebastian Kamph | Practical comparison of every sampler |
| "The Math Behind Diffusion" — Yannic Kilcher | Deep dive into the ODE/SDE formulation |

---

**Tomorrow**: Diffusion Evaluation — FID, IS, CLIP Score.
