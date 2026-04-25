📘 DAY 14 — Consistency Models & Flow Matching

Goal:

Master the cutting-edge approaches that are replacing traditional diffusion sampling — Consistency Models for single-step generation and Flow Matching for simpler, more efficient training.

---

## 🌟 Prerequisites (What You Should Know Before Starting)

> If you're brand new to AI, here's the minimum background:
> - **Neural Network**: A computer program that learns patterns from data (like a brain made of math) 🧠
> - **Diffusion Model**: An AI that learns to create images by first destroying them with noise, then learning to reverse the destruction 🎨
> - **Training**: The process of showing a model millions of examples so it learns patterns 📚
> - **Sampling/Generation**: Using the trained model to create NEW images it has never seen 🖼️
> - **Noise**: Random static (think TV snow) that gets added to or removed from images 📺

---

## 🎯 Why This Topic Matters

| Audience | Why You Should Care |
|----------|--------------------|
| 🆕 Complete Beginner | Understand how AI creates images in milliseconds instead of minutes |
| 🎓 Student | Learn the mathematical elegance of consistency functions |
| 💼 Engineer | Build real-time image generation into production apps |
| 🔬 Researcher | Understand the frontier of efficient generative models |
| 💰 Founder | Know why fast generation = better user experience = more revenue |

---

# 1️⃣ The Speed Problem

DDPM: 1000 steps. DDIM: 50 steps. DPM-Solver: 20 steps.
But real-time applications need 1-4 steps. Enter consistency models and flow matching.

### 🚗 Beginner Analogy: The Road Trip Problem

> Imagine you want to get from City A (noise) to City B (a beautiful image):
>
> | Method | Analogy | Steps | Time |
> |--------|---------|-------|------|
> | **DDPM** | Walking step by step along every street | 1000 | ⏱️ Very slow |
> | **DDIM** | Taking a car, skipping some streets | 50-100 | ⏱️ Slow |
> | **DPM-Solver** | Taking a fast car on the highway | 20 | ⏱️ Medium |
> | ⭐ **Consistency Model** | **Teleportation! Jump directly to destination** | **1-4** | ⚡ **Instant!** |
>
> 🎯 **The key insight**: What if we didn't need to follow the road at all? What if we could look at ANY point on the road and instantly know the destination?

### 🏭 Why Speed Matters in Production

| Application | Required Latency | Old Diffusion | With Consistency Models |
|-------------|-----------------|---------------|------------------------|
| Real-time video filters | < 50ms | ❌ Impossible | ✅ Achievable |
| Interactive design tools | < 200ms | ❌ Too slow | ✅ Smooth |
| Game asset generation | < 100ms | ❌ Not viable | ✅ Real-time |
| Chat image replies | < 1 second | ⚠️ Barely | ✅ Fast |
| Batch content creation | < 5 seconds | ✅ OK | ✅ 10x faster |

---

# 2️⃣ Consistency Models (Song et al., 2023)

> 📖 **Paper**: *"Consistency Models"* — Yang Song, Prafulla Dhariwal, Mark Chen, Ilya Sutskever (OpenAI, 2023)
> This paper introduced a completely new family of generative models that can produce high-quality images in a single step.

### 🧭 Beginner Analogy: The GPS That Knows Every Destination

> 🗺️ Imagine a magical GPS system:
> - **Regular diffusion model** = A GPS that says "turn left, then right, then left..." giving you 50-1000 turn-by-turn directions to reach your destination
> - ⭐ **Consistency model** = A GPS that can look at ANY point on the road and instantly tell you "your destination is HERE" — no step-by-step directions needed!
>
> Even better: if you ask this GPS from different points on the SAME road, it always gives you the SAME destination. That's **self-consistency**! 🎯

## Core Idea
Learn a function $f_\theta$ that maps ANY point on a diffusion trajectory directly to $x_0$:

$$f_\theta(x_t, t) = x_0 \quad \text{for all } t \in [0, T]$$

> 💡 **In plain English**: No matter how noisy the image is (whether it's 10% noise or 90% noise), the consistency model can look at it and predict the final clean image in ONE shot!

Key property: **self-consistency** — all points on the same trajectory map to the same $x_0$:

$$f_\theta(x_t, t) = f_\theta(x_{t'}, t') \quad \text{for any } t, t' \text{ on the same trajectory}$$

> ⭐ **Self-Consistency Analogy**: Imagine 5 people standing at different points on the same hiking trail. If you ask each of them "where does this trail end?", they ALL point to the same mountain peak. That's self-consistency — all points on the same path agree on the same destination!

### 🔑 Why Self-Consistency Is Revolutionary

| Property | What It Means | Why It Matters |
|----------|--------------|----------------|
| Same trajectory → Same output | Points on same denoising path give same clean image | Enables single-step generation |
| Any noise level works | Model handles $t = 0.01$ or $t = 0.99$ | No need to step through all noise levels |
| Consistent predictions | No contradictions between steps | Stable, reliable outputs |

## Boundary Condition
$f_\theta(x_0, 0) = x_0$ (identity at $t=0$)

> 💡 **In plain English**: When the image already has zero noise ($t=0$), the model should just return that same image unchanged. Makes sense — why process something that's already clean?

Enforced via skip connection:

$$f_\theta(x_t, t) = c_{\text{skip}}(t) \cdot x_t + c_{\text{out}}(t) \cdot F_\theta(x_t, t)$$

Where $c_{\text{skip}}(0)=1$ and $c_{\text{out}}(0)=0$.

> 🔧 **How the skip connection works**: At $t=0$, the formula becomes $f_\theta(x_0, 0) = 1 \cdot x_0 + 0 \cdot F_\theta(x_0, 0) = x_0$. The neural network output $F_\theta$ is multiplied by zero, so the input passes through unchanged. As $t$ increases, the network's contribution gradually increases. Think of it like a volume knob 🎚️ — at $t=0$ the "pass-through" is at full volume, and as noise increases, the "neural network cleaning" volume goes up.

---

# 3️⃣ Training Consistency Models

> 🎓 **Big Picture**: There are TWO ways to build a consistency model — one uses a pre-trained teacher (distillation), the other learns from scratch (training). Think of it like learning to cook: you can learn from a master chef, or you can experiment on your own!

## Approach 1: Consistency Distillation (CD)
- Start with a pretrained diffusion model (the "teacher" 👨‍🏫)
- Train consistency model (the "student" 🎓) to be self-consistent along ODE trajectories

> 📖 **Citation**: Song et al. (2023) showed consistency distillation achieves FID 3.55 on CIFAR-10 with just 1 step, compared to the teacher's FID 2.93 with 1000 steps.

### 🍳 Beginner Analogy: Learning from a Master Chef

> Imagine a master chef (pretrained diffusion model) who makes perfect dishes in 50 steps.
> The student (consistency model) watches the chef and learns: "If the dish looks like THIS at step 30, and like THAT at step 29, both should produce the SAME final dish."
> After enough training, the student can look at ingredients at ANY stage and predict the final dish instantly! 🍽️

### How Distillation Works Step by Step:

```
1. Take a clean image x₀
2. Add noise to get x_t (noisy version at time t)
3. Use the TEACHER model to take one ODE step: x_t → x_{t-1}
4. Feed BOTH x_t and x_{t-1} to the STUDENT consistency model
5. Both should predict the SAME clean image x₀
6. If they don't match → that's the loss → update the student!
```

The distillation loss in LaTeX:

$$\mathcal{L}_{CD} = \mathbb{E}\left[ d\Big(f_\theta(x_{t_{n+1}}, t_{n+1}),\; f_{\theta^-}(\hat{x}_{t_n}, t_n)\Big) \right]$$

Where:
- $f_\theta$ = student consistency model (being trained)
- $f_{\theta^-}$ = EMA (exponential moving average) copy of student (frozen target)
- $\hat{x}_{t_n}$ = result of one ODE step from $x_{t_{n+1}}$ using the teacher
- $d(\cdot, \cdot)$ = distance metric (e.g., MSE, LPIPS)

```python
def consistency_distillation_loss(model, model_ema, x_0, diffusion, ode_solver):
    # Sample adjacent timesteps
    t = sample_timestep()
    t_next = t - 1  # adjacent step
    
    noise = torch.randn_like(x_0)
    x_t = diffusion.q_sample(x_0, t, noise)
    
    # One ODE step with teacher model to get x_{t-1}
    with torch.no_grad():
        x_t_next = ode_solver.step(x_t, t, t_next)
    
    # Consistency: both should map to same x_0
    pred_t = model(x_t, t)
    with torch.no_grad():
        pred_t_next = model_ema(x_t_next, t_next)
    
    loss = F.mse_loss(pred_t, pred_t_next)
    return loss
```

> 🔍 **Code Walkthrough (for beginners)**:
> - `sample_timestep()` → Pick a random noise level (like picking a random point on our road)
> - `q_sample(x_0, t, noise)` → Add noise to the clean image (blur it to level $t$)
> - `ode_solver.step(x_t, t, t_next)` → Teacher takes ONE step of denoising
> - `model(x_t, t)` → Student predicts clean image from noisier version
> - `model_ema(x_t_next, t_next)` → Frozen student predicts from less noisy version
> - `F.mse_loss(...)` → Penalize if predictions don't match! Both should give same answer ⭐

## Approach 2: Consistency Training (CT — from scratch)
No teacher needed — train directly with consistency loss on pairs of noised samples.

> 🍳 **Analogy**: Instead of learning from a master chef, you experiment on your own. You take the same dish, mess it up two different amounts, and train yourself to recognize the final result from both versions.

The CT loss:

$$\mathcal{L}_{CT} = \mathbb{E}\left[ d\Big(f_\theta(x + t_{n+1} z,\; t_{n+1}),\; f_{\theta^-}(x + t_n z,\; t_n)\Big) \right]$$

> 💡 **Key difference**: No teacher model needed! The model learns self-consistency by comparing its own predictions at adjacent noise levels on the same noised sample.

### ⭐ Distillation vs Training Comparison

| Feature | Consistency Distillation (CD) | Consistency Training (CT) |
|---------|------------------------------|---------------------------|
| Needs pretrained teacher? | ✅ Yes | ❌ No |
| Quality (1 step) | ⭐⭐⭐⭐ Very good | ⭐⭐⭐ Good |
| Quality (2 steps) | ⭐⭐⭐⭐⭐ Excellent | ⭐⭐⭐⭐ Very good |
| Training complexity | Higher (need teacher) | Lower (standalone) |
| Speed of convergence | Faster | Slower |
| Best for | When you have a good teacher model | Training from scratch |

### 🔬 Progressive Distillation (Meng et al., 2023)

> 📖 **Paper**: *"On Distillation of Guided Diffusion Models"* — Chenlin Meng et al. (2023)

Another approach to speed up diffusion: **progressive distillation** halves the number of steps repeatedly:

```
Teacher: 1024 steps → Student: 512 steps
Student becomes teacher: 512 steps → New student: 256 steps
Repeat: 256 → 128 → 64 → 32 → 16 → 8 → 4 → 2 → 1 step!
```

> 🪆 **Analogy**: Like Russian nesting dolls — each generation compresses the knowledge of the previous one into fewer steps, until you get a tiny model that does it all in 1-4 steps!

$$\mathcal{L}_{\text{prog}} = \mathbb{E}\left[\| f_\theta^{(N/2)}(x_t, t) - f_{\text{teacher}}^{(N)}(x_t, t) \|^2\right]$$

---

# 4️⃣ Sampling with Consistency Models

> 🎮 **Beginner Analogy**: Sampling = using the trained model to actually CREATE new images. It's like the difference between learning to paint (training) and actually painting a masterpiece (sampling).

### Single-Step (Fastest) ⚡
```python
z = torch.randn(shape)  # sample noise
x_0 = consistency_model(z, T)  # one forward pass!
```

> 💡 **What's happening**: 
> 1. `torch.randn(shape)` = Generate pure random noise (TV static) 📺
> 2. `consistency_model(z, T)` = The model looks at that noise and teleports directly to a clean image! ✨
> 
> That's it. ONE step. No iteration. No gradual denoising. Pure magic (well, pure math).

### ⭐ Why Single-Step Works

> In regular diffusion, going from full noise to a clean image in one step gives garbage results. But the consistency model was specifically TRAINED to handle this! It learned the mapping from ANY noise level to the final clean image. So even at maximum noise ($t = T$), it can predict $x_0$ directly.

### Multi-Step (Better Quality) 🎨
```python
def multistep_consistency_sample(model, shape, steps=4):
    ts = torch.linspace(T, 0, steps+1)  # e.g., [T, 3T/4, T/2, T/4, 0]
    x = torch.randn(shape)
    
    for i in range(steps):
        x = model(x, ts[i])  # map to x_0
        if i < steps - 1:
            # Add noise back to intermediate level
            noise = torch.randn_like(x)
            x = add_noise(x, ts[i+1], noise)
    return x
```

> 🔍 **Code Walkthrough for Beginners**:
> 1. Start with pure noise
> 2. **Step 1**: Use consistency model to jump to clean image $x_0$
> 3. **Add noise back** to an intermediate level (like going back partway on the road)
> 4. **Step 2**: Jump to clean image again from this new point
> 5. Repeat 2-4 times total
> 
> 🧭 **Analogy**: Like getting multiple opinions! You teleport to the destination, then go back partway and teleport again. Each time you get a slightly different (and often better) view of the destination. After 2-4 teleportations, you have a very accurate picture!

### Quality vs Speed Trade-off

| Steps | Quality | Speed | Best For |
|-------|---------|-------|----------|
| 1 | ⭐⭐⭐ Good | ⚡⚡⚡⚡⚡ Fastest | Real-time apps, previews |
| 2 | ⭐⭐⭐⭐ Very Good | ⚡⚡⚡⚡ Very Fast | Interactive tools |
| 4 | ⭐⭐⭐⭐⭐ Excellent | ⚡⚡⚡ Fast | Production quality |
| 8+ | ⭐⭐⭐⭐⭐ Excellent | ⚡⚡ Moderate | Diminishing returns |

### 🏭 Production Use Case: Latent Consistency Models (LCM)

> 📖 **Paper**: *"Latent Consistency Models"* — Simian Luo, Yiqin Tan, Longbo Huang, Jian Li, Hang Zhao (2023)

LCM applies consistency distillation in the **latent space** of Stable Diffusion:

| Feature | Standard Diffusion | LCM |
|---------|-------------------|-----|
| Steps needed | 20-50 | **2-4** |
| Time per image | 2-10 seconds | **0.1-0.5 seconds** |
| Quality | Excellent | Very Good to Excellent |
| Works with LoRA? | ✅ Yes | ✅ Yes (LCM-LoRA!) |
| Interactive editing? | ❌ Too slow | ✅ Real-time! |

> 💡 **LCM-LoRA** is especially powerful: it's a small adapter (~67MB) that can be plugged into ANY Stable Diffusion model to enable 4-step generation without retraining the base model!

```
🏭 Production Architecture with LCM:

User Request → Text Encoder → Latent Noise (z_T)
      │
      ▼
  LCM Model (4 steps in latent space)
      │
      ▼
  VAE Decoder → Final Image (512x512 in ~200ms!)
```

---

# 5️⃣ Flow Matching (Lipman et al., 2023)

> 📖 **Paper**: *"Flow Matching for Generative Modeling"* — Yaron Lipman, Ricky T.Q. Chen, Heli Ben-Hamu, Maximilian Nickel (Meta AI, 2023)

## Core Idea: Simpler than Diffusion

> 🏗️ **Beginner Analogy**: If diffusion is like building a house by randomly removing bricks and learning to put them back, flow matching is like drawing a straight line from a pile of bricks to a finished house, and learning to follow that line. Much simpler! 🏠

Instead of the complex noise schedule + reverse process framework, just learn a velocity field that transports noise to data along straight paths.

Define a path from noise $z \sim \mathcal{N}(0, I)$ to data $x_0$:

$$x_t = (1-t) \cdot z + t \cdot x_0 \quad \text{(linear interpolation, } t \in [0,1]\text{)}$$

The velocity at time $t$:

$$v_t = x_0 - z$$

Train a network $v_\theta(x_t, t)$ to predict this velocity:

$$\mathcal{L} = \mathbb{E}_{t, x_0, z}\left[\|v_\theta(x_t, t) - (x_0 - z)\|^2\right]$$

That's it. No ELBO, no noise schedules, no complicated math.

> 💡 **In plain English**: Instead of learning "how to remove noise" (diffusion), you learn "which direction to move" (flow). It's like learning the wind direction that blows scattered papers into neat stacks. 💨

---

# 6️⃣ Flow Matching Implementation

> 🔧 **For Beginners**: Don't worry if you don't understand every line of code. Focus on the COMMENTS — they explain what each piece does in plain English.

```python
class FlowMatchingModel(nn.Module):
    def __init__(self, net):
        super().__init__()
        self.net = net  # Any architecture (U-Net, DiT, etc.)
    
    def forward(self, x_t, t):
        return self.net(x_t, t)

def flow_matching_loss(model, x_0):
    batch_size = x_0.shape[0]
    
    # Sample time uniformly
    t = torch.rand(batch_size, 1, 1, 1, device=x_0.device)
    
    # Sample noise
    z = torch.randn_like(x_0)
    
    # Interpolate
    x_t = (1 - t) * z + t * x_0
    
    # Target velocity
    v_target = x_0 - z
    
    # Predict velocity
    v_pred = model(x_t, t.squeeze())
    
    loss = F.mse_loss(v_pred, v_target)
    return loss

@torch.no_grad()
def flow_matching_sample(model, shape, num_steps=50):
    """Sample by integrating the learned velocity field."""
    x = torch.randn(shape)  # start from noise (t=0)
    dt = 1.0 / num_steps
    
    for i in range(num_steps):
        t = torch.full((shape[0],), i * dt)
        v = model(x, t)
        x = x + v * dt  # Euler integration
    
    return x
```

> 🔍 **Line-by-Line for Beginners**:
> - `flow_matching_loss`: The training recipe
>   - Pick a random time $t$ between 0 and 1 (how far along the path are we?)
>   - Create a point $x_t$ that's a mix of noise and clean image
>   - The true velocity is simply: clean image minus noise ($x_0 - z$)
>   - Train the model to predict this velocity
> - `flow_matching_sample`: The generation recipe
>   - Start from pure noise
>   - Take small steps following the predicted velocity
>   - Like following wind directions to reach the destination 🌬️

---

# 7️⃣ Optimal Transport Flow Matching

> 📖 **Citation**: Tong et al. (2023), *"Improving and Generalizing Flow-Based Generative Models with Minibatch Optimal Transport"*

Standard paths are straight lines. Optimal Transport (OT) provides even better paths:

```
Standard: random pairing of z and x_0
OT: pair each z with its "closest" x_0 (minimizes transport cost)
```

OT flow matching produces straighter trajectories → fewer integration steps needed.

> 🚚 **Beginner Analogy**: Imagine you have 100 delivery trucks (noise samples) and 100 houses (target images). 
> - **Standard flow matching** = randomly assign trucks to houses (some trucks drive across the whole city!)
> - **OT flow matching** = use a smart dispatcher to assign each truck to its NEAREST house (shortest total driving distance)
> 
> Result: straighter, shorter paths = fewer steps = faster generation! 🏎️

---

# 8️⃣ Comparison Table

> 🎯 **How to read this table**: Each row is a different method for generating images. "Sampling Steps" = how many times the model has to run to produce one image. Fewer = faster!

| Method | Training | Sampling Steps | Quality | Simplicity | 🚀 Speed Rating |
|--------|----------|---------------|---------|-----------|---------------|
| DDPM | ELBO-derived MSE | 1000 | Excellent | Moderate | 🐢 Very Slow |
| DDIM | Same as DDPM | 50-100 | Excellent | Moderate | 🚶 Slow |
| Consistency (CD) | Distillation | 1-4 | Very Good | Moderate | ⚡ Fast |
| Consistency (CT) | Self-consistency | 1-4 | Good | Moderate | ⚡ Fast |
| Flow Matching | MSE on velocity | 20-50 | Excellent | Very Simple | 🏃 Medium |
| Rectified Flow | OT + Flow | 1-10 | Excellent | Simple | ⚡ Fast |
| LCM | Latent distillation | 2-4 | Very Good | Moderate | ⚡ Very Fast |

### ⭐ When to Use What (Decision Guide)

```
Do you need real-time generation (< 200ms)?
├─ YES → Consistency Models or LCM
└─ NO → Do you want simplest training?
         ├─ YES → Flow Matching
         └─ NO → Do you want absolute best quality?
                  ├─ YES → DDPM/DDIM with many steps
                  └─ NO → Rectified Flow (good balance)
```

---

# 9️⃣ State of the Art (2024)

> 🌐 **Where consistency models and flow matching are used in the real world TODAY**:

- **Stable Diffusion 3**: Flow matching with MMDiT architecture
- **Flux**: Rectified flow matching
- **DALL-E 3**: Consistency model for fast sampling
- **LCM (Latent Consistency Models)**: 4-step generation for Stable Diffusion

The field is moving from DDPM → Flow Matching rapidly.

### 🏭 Production Deployment Examples

| Product | Technology | Impact |
|---------|-----------|--------|
| **Canva AI** | Fast diffusion + consistency | Real-time marketing image generation |
| **Adobe Firefly** | Optimized diffusion | Interactive creative tools |
| **Midjourney v6** | Proprietary fast sampling | High-quality in seconds |
| **Stability AI SDXL Turbo** | Adversarial + distillation | 1-step generation |
| **Playground AI** | LCM integration | Real-time interactive editing |

### 🚀 Why Companies Care About Speed

```
💰 Business Impact of Faster Generation:

  50 steps (2 sec) → User waits, might leave
   4 steps (0.2 sec) → Feels instant, user stays
   1 step  (0.05 sec) → Real-time interaction possible!

  Faster generation = 
    ✅ Lower compute costs (fewer GPU-seconds per image)
    ✅ Better user experience (no waiting)
    ✅ New use cases (video, real-time editing, gaming)
    ✅ More users served per GPU
```

---

# 📝 Summary

| Innovation | Key Insight | Impact |
|-----------|------------|--------|
| Consistency Models | Map any noise level directly to $x_0$ | 1-4 step generation |
| Flow Matching | Learn straight-line velocity field | Simpler training, fewer steps |
| Rectified Flow | OT + straight paths | State of the art |
| LCM | Consistency in latent space | Plug-and-play fast generation |
| Progressive Distillation | Repeatedly halve steps | Systematic speed improvement |

---

# 📚️ Cheat Sheet: Consistency Models in 60 Seconds

```
╔══════════════════════════════════════════════════════════╗
║  ⭐ CONSISTENCY MODELS CHEAT SHEET ⭐                    ║
╠══════════════════════════════════════════════════════════╣
║                                                          ║
║  WHAT: A model that maps ANY noisy image to clean x₀    ║
║  HOW:  f_θ(x_t, t) = x₀  for ALL t                      ║
║  KEY:  Self-consistency = same path → same destination   ║
║                                                          ║
║  TWO TRAINING METHODS:                                   ║
║  1. Distillation (CD) → Learn from teacher model         ║
║  2. Training (CT)     → Learn from scratch                ║
║                                                          ║
║  SAMPLING:                                                ║
║  • 1 step  = fast but acceptable quality                  ║
║  • 2-4 steps = fast AND high quality                      ║
║                                                          ║
║  BOUNDARY: f_θ(x₀, 0) = x₀ (identity at t=0)            ║
║                                                          ║
║  LCM = Consistency in latent space (for Stable Diffusion) ║
║                                                          ║
║  🎯 ANALOGY: GPS that teleports to destination from       ║
║     any point on the road!                                ║
║                                                          ║
╚══════════════════════════════════════════════════════════╝
```

### 💡 Key Equations to Remember

| Equation | Name | Meaning |
|----------|------|---------|
| $f_\theta(x_t, t) = x_0$ | Consistency mapping | Any noisy point maps to clean image |
| $f_\theta(x_t, t) = f_\theta(x_{t'}, t')$ | Self-consistency | Same trajectory = same output |
| $f_\theta(x_0, 0) = x_0$ | Boundary condition | Clean image passes through unchanged |
| $\mathcal{L}_{CD} = d(f_\theta(x_{t_{n+1}}), f_{\theta^-}(\hat{x}_{t_n}))$ | Distillation loss | Adjacent points must agree |
| $x_t = (1-t)z + tx_0$ | Flow matching path | Straight line from noise to data |
| $v_t = x_0 - z$ | Flow velocity | Direction from noise to data |

---

# 📁 Resources

## 📖 Essential Papers

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|------------------|
| [Consistency Models](https://arxiv.org/abs/2303.01469) | Song, Dhariwal, Chen, Sutskever | 2023 | ⭐ Original consistency models paper |
| [Improved Consistency Training](https://arxiv.org/abs/2310.14189) | Song & Dhariwal | 2023 | Better training without distillation |
| [Latent Consistency Models](https://arxiv.org/abs/2310.04378) | Luo, Tan, Huang, Li, Zhao | 2023 | ⭐ LCM for Stable Diffusion (4-step!) |
| [On Distillation of Guided Diffusion](https://arxiv.org/abs/2210.03142) | Meng, Rombach, Gao et al. | 2023 | Progressive distillation |
| [Flow Matching for Generative Modeling](https://arxiv.org/abs/2210.02747) | Lipman, Chen, Ben-Hamu, Nickel | 2023 | Simplified flow-based training |
| [Rectified Flow](https://arxiv.org/abs/2209.03003) | Liu, Gong, Liu | 2023 | OT + straight paths |
| [SDXL Turbo](https://arxiv.org/abs/2311.17042) | Sauer, Lorenz, Blattmann, Rombach | 2023 | Adversarial distillation for 1-step |

## 📕 Books & Courses

| Resource | Type | Level |
|----------|------|-------|
| *Understanding Deep Learning* — Simon J.D. Prince (2023) | 📕 Book | Ch. 18: Diffusion Models |
| *Probabilistic Machine Learning: Advanced Topics* — Kevin Murphy | 📕 Book | Diffusion & Flow chapters |
| Hugging Face Diffusion Models Course | 🎓 Course | Beginner-friendly |
| DeepLearning.AI: How Diffusion Models Work | 🎓 Course | Practical implementation |

## 🌐 Blog Posts & Online Resources

| Resource | Link |
|----------|------|
| OpenAI Blog: Consistency Models | https://openai.com/research/consistency-models |
| Hugging Face: LCM Guide | https://huggingface.co/docs/diffusers/using-diffusers/lcm |
| Lilian Weng: Diffusion Models Survey | https://lilianweng.github.io/posts/2021-07-11-diffusion-models/ |
| Yang Song's Blog on Score-Based Models | https://yang-song.net/blog/ |

---

## 🧪 Quick Self-Test

> Test your understanding! Try to answer these without scrolling up:

1. ❓ What does a consistency model's function $f_\theta$ do?
   - ✅ Maps any noisy image $x_t$ directly to the clean image $x_0$ in one step

2. ❓ What is self-consistency?
   - ✅ All points on the same denoising trajectory produce the same clean output

3. ❓ What's the boundary condition and why?
   - ✅ $f_\theta(x_0, 0) = x_0$ — a clean image should remain unchanged

4. ❓ What are the two ways to train consistency models?
   - ✅ Consistency Distillation (from teacher) and Consistency Training (from scratch)

5. ❓ What is LCM?
   - ✅ Latent Consistency Models — consistency distillation in Stable Diffusion's latent space

6. ❓ How does flow matching differ from diffusion?
   - ✅ Learns a velocity field along straight paths instead of a noise-reversal process

**Tomorrow**: Diffusion Models Capstone — build a complete text-to-image system.
