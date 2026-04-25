📘 DAY 15 — Diffusion Models Capstone: Complete System

> *"The whole is greater than the sum of its parts."* — Aristotle (also true for diffusion systems) 🏛️

## Goal:

Synthesize everything from Days 61-74 into a complete, working diffusion model system. Train on a real dataset, evaluate with proper metrics, and implement multiple sampling strategies.

---

## 🚗 The Big Picture Analogy: Assembling a Complete Car

Imagine you've spent the last 14 days building **individual car parts**:

| Day | What We Built | Car Part Analogy 🚗 |
|-----|--------------|---------------------|
| Day 1 | Generative landscape | **Blueprints** — understanding what kind of car we're building |
| Day 2 | Forward process | **Paint stripper** — learning how to remove paint layer by layer |
| Day 3 | Noise schedules | **Paint removal speed** — how fast/slow to strip each layer |
| Day 4 | Reverse process | **Paint sprayer** — learning to add paint back perfectly |
| Day 5 | DDPM Loss | **Quality inspector** — measuring if the paint job is good |
| Day 6 | Score matching | **GPS navigation** — knowing which direction to spray |
| Day 7 | U-Net architecture | **The engine** — the core brain that does the work |
| Day 8 | Conditioning (CFG) | **Steering wheel** — controlling WHERE the car goes |
| Day 9 | Training pipeline | **Assembly line** — the process of building the car |
| Day 10 | DDPM vs DDIM | **Turbo mode** — making the car go 20x faster |
| Day 11 | Latent diffusion | **Engine upgrade** — smaller, more efficient engine |
| Day 12 | Advanced sampling | **Transmission** — smooth gear shifts for better performance |
| Day 13 | Evaluation | **Test track** — measuring how well the car performs |
| Day 14 | Next-gen models | **Concept car features** — bleeding-edge innovations |

⭐ **TODAY (Day 15)**: We take ALL these parts and **wire them together into a fully working car** — a complete diffusion system you can train, sample from, evaluate, and deploy!

> 🎯 **For absolute beginners**: Think of today as the final episode of a cooking show where you've learned individual techniques (chopping, sautéing, baking) and now you're making a **complete five-course meal** from start to finish.

---

## 🧠 What Is a "Complete Diffusion System"?

A complete diffusion system has **5 major components** working together:

```
┌─────────────────────────────────────────────────────────┐
│                 COMPLETE DIFFUSION SYSTEM                │
│                                                         │
│  ┌───────────┐   ┌───────────┐   ┌──────────────────┐  │
│  │  SCHEDULE  │──▶│   MODEL   │──▶│    TRAINING      │  │
│  │ (how much  │   │ (U-Net    │   │ (teach model to  │  │
│  │  noise)    │   │  brain)   │   │  remove noise)   │  │
│  └───────────┘   └───────────┘   └──────────────────┘  │
│                                          │              │
│                                          ▼              │
│  ┌───────────┐   ┌──────────────────────────────────┐  │
│  │ EVALUATION │◀──│         SAMPLING                 │  │
│  │ (how good  │   │ (generate new images from noise) │  │
│  │  are we?)  │   └──────────────────────────────────┘  │
│  └───────────┘                                          │
└─────────────────────────────────────────────────────────┘
```

### 📖 The Math Behind It All (Complete Picture)

The **entire diffusion pipeline** can be summarized in two equations:

**Forward Process** (adding noise — the "destruction" phase):

$$q(x_t | x_0) = \mathcal{N}\left(x_t; \sqrt{\bar{\alpha}_t} \, x_0, \, (1 - \bar{\alpha}_t) \mathbf{I}\right)$$

> 🍕 **Analogy**: This is like progressively blurring a photo. At $t=0$ you have a crisp photo; at $t=T$ it's pure static on a TV.

**Reverse Process** (removing noise — the "creation" phase):

$$p_\theta(x_{t-1} | x_t) = \mathcal{N}\left(x_{t-1}; \mu_\theta(x_t, t), \, \sigma_t^2 \mathbf{I}\right)$$

> 🍕 **Analogy**: This is like an artist looking at TV static and gradually painting a picture, one tiny refinement at a time.

**Training Objective** (what we optimize):

$$\mathcal{L} = \mathbb{E}_{x_0, \epsilon, t}\left[\|\epsilon - \epsilon_\theta(x_t, t)\|^2\right]$$

> 🍕 **Analogy**: We show the model a noisy photo and ask "what noise was added?" — the loss measures how wrong the model's guess was.

**Classifier-Free Guidance** (controlling generation):

$$\tilde{\epsilon}_\theta(x_t, t, c) = \epsilon_\theta(x_t, t, \varnothing) + w \cdot \left(\epsilon_\theta(x_t, t, c) - \epsilon_\theta(x_t, t, \varnothing)\right)$$

> 🍕 **Analogy**: $w$ is like a "creativity dial." At $w=1$, the model draws what it wants. At $w=7$, it strictly follows your instructions. Too high ($w>15$) and it becomes obsessive/distorted.

📚 **Research Foundation**: Ho et al. (2020) — *"Denoising Diffusion Probabilistic Models"*; Song et al. (2021) — *"Score-Based Generative Modeling through SDEs"*; Ho & Salimans (2022) — *"Classifier-Free Diffusion Guidance"*

---

# 1️⃣ Project: Class-Conditional DDPM on Fashion-MNIST

> 🎯 **What we're building**: A system that generates clothing images (shirts, shoes, bags, etc.) on demand — you tell it "make a shoe" and it creates a brand-new shoe image from pure noise!

> ⭐ **Why Fashion-MNIST?** It's like the "Hello World" of image generation — simple enough to train on a single GPU in minutes, complex enough to demonstrate all diffusion concepts.

### 🔧 Component A: The Diffusion Schedule (How Much Noise at Each Step)

> 🚗 **Car analogy**: The diffusion schedule is like the **speedometer** — it controls how fast noise is added/removed at each timestep. A cosine schedule is like smooth acceleration; a linear schedule is like slamming the gas pedal.

**The math**: At each timestep $t$, we define noise level $\beta_t$ and cumulative signal retention $\bar{\alpha}_t$:

$$\bar{\alpha}_t = \prod_{s=1}^{t} \alpha_s = \prod_{s=1}^{t} (1 - \beta_s)$$

For the **cosine schedule** (Nichol & Dhariwal, 2021):

$$\bar{\alpha}_t = \frac{f(t)}{f(0)}, \quad f(t) = \cos\left(\frac{t/T + s}{1 + s} \cdot \frac{\pi}{2}\right)^2$$

where $s = 0.008$ is a small offset to prevent $\beta_t$ from being too small near $t = 0$.

| Schedule Type | Pros | Cons | Best For |
|:---:|:---:|:---:|:---:|
| Linear | Simple, well-studied | Wastes steps on low-noise region | DDPM paper reproduction |
| Cosine | More uniform SNR usage | Slightly more compute | **Production systems** ⭐ |
| Sigmoid | Smooth transitions | Newer, less tested | Flow matching |

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
from torch.utils.data import DataLoader
from torchvision import datasets, transforms
import numpy as np
from tqdm import tqdm

# ====== DIFFUSION SCHEDULE ======
class DiffusionSchedule:
    """
    🎯 BEGINNER EXPLANATION:
    This class controls HOW MUCH noise is added at each of T=1000 timesteps.
    
    Think of it like a recipe:
    - Step 0: Original image (0% noise, 100% signal) 
    - Step 500: Half noise, half signal  
    - Step 1000: Pure random noise (100% noise, 0% signal)
    
    The 'cosine' schedule adds noise gradually (like slowly turning up 
    static on a TV), while 'linear' adds it more aggressively at the end.
    """
    def __init__(self, T=1000, schedule='cosine'):
        self.T = T
        if schedule == 'cosine':
            # 📐 Cosine schedule: more gradual noise addition
            # Formula: αbar_t = cos²((t/T + 0.008) / 1.008 · π/2)
            steps = torch.arange(T + 1, dtype=torch.float32) / T
            f_t = torch.cos((steps + 0.008) / 1.008 * torch.pi / 2) ** 2
            alpha_bars = f_t / f_t[0]
            self.betas = torch.clamp(1 - alpha_bars[1:] / alpha_bars[:-1], 1e-4, 0.999)
        else:
            # 📐 Linear schedule: β goes from 0.0001 to 0.02
            self.betas = torch.linspace(1e-4, 0.02, T)
        
        self.alphas = 1 - self.betas              # α_t = 1 - β_t
        self.alpha_bars = torch.cumprod(self.alphas, dim=0)  # ᾱ_t = ∏α_s
        self.sqrt_ab = torch.sqrt(self.alpha_bars)           # √ᾱ_t (signal scaling)
        self.sqrt_1_ab = torch.sqrt(1 - self.alpha_bars)    # √(1-ᾱ_t) (noise scaling)
    
    def to(self, device):
        for attr in ['betas','alphas','alpha_bars','sqrt_ab','sqrt_1_ab']:
            setattr(self, attr, getattr(self, attr).to(device))
        return self
    
    def q_sample(self, x0, t, noise=None):
        """
        🎯 The forward process: add noise to a clean image.
        Formula: x_t = √ᾱ_t · x₀ + √(1-ᾱ_t) · ε
        
        Think of it as MIXING: 
        - √ᾱ_t controls how much ORIGINAL image remains
        - √(1-ᾱ_t) controls how much NOISE is added
        """
        if noise is None:
            noise = torch.randn_like(x0)
        return self.sqrt_ab[t][:,None,None,None] * x0 + \
               self.sqrt_1_ab[t][:,None,None,None] * noise, noise
```

### 🔧 Component B: Model Building Blocks (The Engine Parts)

> 🚗 **Car analogy**: These are the **engine components** — pistons (ResBlocks), fuel injectors (TimeEmbed), and turbocharger (Attention). Each does one job really well, and together they make the engine run.

#### ⏰ Time Embedding — Telling the Model "What Step Are We On?"

The model needs to know the current timestep $t$ to decide how much noise to predict. We use **sinusoidal positional encoding** (borrowed from Transformers!):

$$\text{emb}(t) = \left[\sin\left(\frac{t}{10000^{2i/d}}\right), \cos\left(\frac{t}{10000^{2i/d}}\right)\right]_{i=0}^{d/2-1}$$

> 🍕 **Analogy**: It's like telling a chef "we're on step 5 of 10 in the recipe" — the chef adjusts their technique based on how far along they are.

#### 🧱 ResBlock — The Workhorse Processing Unit

The residual block processes features while incorporating timestep information:

$$h = \text{Conv}\bigl(\text{SiLU}(\text{GroupNorm}(x))\bigr) + \text{Linear}(t_{\text{emb}})$$
$$\text{output} = \text{Conv}\bigl(\text{SiLU}(\text{GroupNorm}(h))\bigr) + \text{skip}(x)$$

> 🍕 **Analogy**: The skip connection ($+\text{skip}(x)$) is like always keeping the original recipe as backup — if the modification doesn't help, we don't lose the original information.

#### 👁️ Attention — Looking at the Whole Picture

Self-attention lets every pixel look at every other pixel:

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V$$

> 🍕 **Analogy**: When painting an eye, the model can look at the other eye to make them match. Without attention, each pixel only sees its local neighbors.

```python
# ====== MODEL COMPONENTS ======
class TimeEmbed(nn.Module):
    """
    🎯 BEGINNER EXPLANATION:
    Converts a simple number (timestep t=0,1,...,999) into a rich 
    vector that the model can use. Uses sin/cos waves at different 
    frequencies — like how a radio uses different frequencies to 
    carry different information.
    """
    def __init__(self, dim):
        super().__init__()
        self.dim = dim
        self.mlp = nn.Sequential(nn.Linear(dim, dim*4), nn.SiLU(), nn.Linear(dim*4, dim*4))
    
    def forward(self, t):
        half = self.dim // 2
        freqs = torch.exp(-torch.log(torch.tensor(10000.0)) * torch.arange(half, device=t.device) / half)
        emb = t[:, None].float() * freqs[None, :]
        emb = torch.cat([torch.sin(emb), torch.cos(emb)], dim=-1)
        return self.mlp(emb)

class ResBlock(nn.Module):
    """
    🎯 BEGINNER EXPLANATION:
    A "residual block" that processes image features. The magic trick: 
    it adds the INPUT back to the OUTPUT (skip connection). This means 
    the block only needs to learn the DIFFERENCE, which is much easier.
    
    Also receives timestep info so it knows how noisy the current image is.
    """
    def __init__(self, in_ch, out_ch, t_dim, dropout=0.1):
        super().__init__()
        self.norm1 = nn.GroupNorm(8, in_ch)
        self.conv1 = nn.Conv2d(in_ch, out_ch, 3, padding=1)
        self.t_proj = nn.Linear(t_dim, out_ch)
        self.norm2 = nn.GroupNorm(8, out_ch)
        self.drop = nn.Dropout(dropout)
        self.conv2 = nn.Conv2d(out_ch, out_ch, 3, padding=1)
        self.skip = nn.Conv2d(in_ch, out_ch, 1) if in_ch != out_ch else nn.Identity()
    
    def forward(self, x, t):
        h = self.conv1(F.silu(self.norm1(x)))
        h = h + self.t_proj(F.silu(t))[:,:,None,None]  # ← inject timestep info!
        h = self.conv2(self.drop(F.silu(self.norm2(h))))
        return h + self.skip(x)  # ← skip connection: add input back!

class Attention(nn.Module):
    """
    🎯 BEGINNER EXPLANATION:
    Self-attention lets every pixel in the image "look at" every other 
    pixel. Without this, a pixel only sees its 3×3 neighborhood. With 
    attention, a pixel drawing a left eye can check the right eye for 
    consistency.
    
    Multi-head attention = multiple "perspectives" looking at the image 
    simultaneously (like having 4 different quality inspectors).
    """
    def __init__(self, ch, heads=4):
        super().__init__()
        self.norm = nn.GroupNorm(8, ch)
        self.qkv = nn.Conv2d(ch, ch*3, 1)
        self.proj = nn.Conv2d(ch, ch, 1)
        self.heads = heads
    
    def forward(self, x):
        B, C, H, W = x.shape
        h = self.norm(x)
        qkv = self.qkv(h).reshape(B, 3, self.heads, C//self.heads, H*W)
        q, k, v = qkv[:, 0], qkv[:, 1], qkv[:, 2]
        attn = torch.softmax((q @ k.transpose(-2,-1)) / (C//self.heads)**0.5, dim=-1)
        out = (attn @ v).reshape(B, C, H, W)
        return x + self.proj(out)
```

### 🔧 Component C: The Full U-Net (The Complete Engine)

> 🚗 **Car analogy**: The U-Net is the **fully assembled engine**. It takes in a noisy image + timestep + class label and outputs the predicted noise. The "U" shape means it compresses the image (encoder), processes it (middle), and expands it back (decoder), with shortcut wires (skip connections) between matching layers.

```
INPUT: noisy image (28×28)
    │
    ▼
┌──────────────────┐
│   ENCODER (Down)  │  ← Compress: 28×28 → 14×14 → 7×7
│   "Understand"    │     (like zooming out to see the big picture)
└────────┬─────────┘
         │ skip connections ──────────┐
         ▼                            │
┌──────────────────┐                  │
│     MIDDLE        │  ← Process at   │
│   "Think Deep"    │    lowest res    │
└────────┬─────────┘                  │
         ▼                            │
┌──────────────────┐                  │
│   DECODER (Up)    │  ← Expand: 7×7 → 14×14 → 28×28
│   "Reconstruct"   │     (zoom back in with details from skip)
└────────┬─────────┘◀─────────────────┘
         │
         ▼
OUTPUT: predicted noise (28×28)
```

**The conditioning mechanism** combines timestep and class embeddings:

$$\text{combined\_embedding} = \text{TimeEmbed}(t) + \text{ClassEmbed}(y)$$

This tells the U-Net **two crucial things**: (1) how noisy the image is, and (2) what class to generate.

⭐ **Key insight**: For classifier-free guidance, we add a special "null class" (class 10 in a 10-class problem). During training, we randomly replace the real class with this null class 10% of the time. This teaches the model to generate BOTH conditionally and unconditionally.

📚 **Reference**: Ronneberger et al. (2015) — *"U-Net: Convolutional Networks for Biomedical Image Segmentation"*; Dhariwal & Nichol (2021) — *"Diffusion Models Beat GANs on Image Synthesis"*

```python
# ====== FULL U-NET ======
class ConditionalUNet(nn.Module):
    """
    🎯 BEGINNER EXPLANATION:
    This is the COMPLETE model that learns to remove noise from images.
    
    It takes THREE inputs:
    1. x: A noisy image (28×28 pixels)
    2. t: Which timestep (0-999) — tells it how noisy the image is
    3. y: Which class (0-9) — tells it what to generate (shoe, shirt, etc.)
    
    It outputs: The predicted noise that was added to the image.
    
    Structure: Encoder (shrink) → Middle (process) → Decoder (expand)
    with skip connections linking encoder to decoder (like shortcuts).
    """
    def __init__(self, in_ch=1, base=64, num_classes=10, t_dim=256):
        super().__init__()
        self.t_embed = TimeEmbed(t_dim)
        self.c_embed = nn.Embedding(num_classes + 1, t_dim * 4)  # +1 for unconditional
        self.null_class = num_classes  # unconditional token
        
        self.init_conv = nn.Conv2d(in_ch, base, 3, padding=1)
        
        # Encoder — progressively shrinks spatial resolution
        self.d1 = ResBlock(base, base, t_dim*4)
        self.d1_attn = Attention(base)
        self.pool1 = nn.Conv2d(base, base, 3, stride=2, padding=1)  # 28→14
        self.d2 = ResBlock(base, base*2, t_dim*4)
        self.pool2 = nn.Conv2d(base*2, base*2, 3, stride=2, padding=1)  # 14→7
        
        # Middle — deepest processing at smallest resolution
        self.mid1 = ResBlock(base*2, base*2, t_dim*4)
        self.mid_attn = Attention(base*2)
        self.mid2 = ResBlock(base*2, base*2, t_dim*4)
        
        # Decoder — progressively expands back to original size
        self.up2 = nn.Upsample(scale_factor=2)  # 7→14
        self.u2 = ResBlock(base*4, base, t_dim*4)  # base*4 because we concat skip!
        self.up1 = nn.Upsample(scale_factor=2)  # 14→28
        self.u1 = ResBlock(base*2, base, t_dim*4)
        self.u1_attn = Attention(base)
        
        self.out = nn.Sequential(nn.GroupNorm(8, base), nn.SiLU(), nn.Conv2d(base, in_ch, 3, padding=1))
    
    def forward(self, x, t, y):
        # Combine time embedding + class embedding
        te = self.t_embed(t) + self.c_embed(y)
        
        # ENCODER path (going down)
        h0 = self.init_conv(x)
        h1 = self.d1_attn(self.d1(h0, te))     # 28×28
        h2 = self.d2(self.pool1(h1), te)         # 14×14
        h3 = self.pool2(h2)                       # 7×7
        
        # MIDDLE (deepest processing)
        m = self.mid1(h3, te)
        m = self.mid_attn(m)
        m = self.mid2(m, te)
        
        # DECODER path (going up) with skip connections via torch.cat
        u = self.up2(m)                            # 7→14
        u = self.u2(torch.cat([u, h2], 1), te)    # concat with encoder's h2!
        u = self.up1(u)                            # 14→28
        u = self.u1_attn(self.u1(torch.cat([u, h1], 1), te))  # concat with encoder's h1!
        
        return self.out(u)  # Output: predicted noise ε_θ
```

---

# 2️⃣ Training with CFG

> 🚗 **Car analogy**: Training is like **teaching a new mechanic**. We deliberately mess up engines (add noise), then ask the trainee to diagnose exactly what went wrong (predict the noise). After millions of practice rounds, the mechanic can fix any engine perfectly.

### 🎓 The Training Pipeline Explained Step by Step

```
FOR each epoch (full pass through dataset):
  FOR each batch of images:
    ┌─────────────────────────────────────────────────┐
    │ 1. Load real images x₀ and their class labels y  │
    │                                                   │
    │ 2. Randomly DROP 10% of labels → null class       │
    │    (teaches model to work without conditions)     │
    │                                                   │
    │ 3. Pick random timestep t for each image          │
    │                                                   │
    │ 4. Generate random noise ε                        │
    │                                                   │
    │ 5. Create noisy image: x_t = √ᾱ_t·x₀ + √(1-ᾱ_t)·ε │
    │                                                   │
    │ 6. Model predicts: ε_θ = UNet(x_t, t, y)         │
    │                                                   │
    │ 7. Loss = ||ε - ε_θ||²  (how wrong was the guess?)│
    │                                                   │
    │ 8. Backpropagate and update weights               │
    │                                                   │
    │ 9. Update EMA model (slowly averaged weights)     │
    └─────────────────────────────────────────────────┘
```

### 📊 Key Training Concepts

| Concept | What It Does | Why It Matters |
|:--------|:-------------|:---------------|
| **CFG dropout** ($p = 0.1$) | Randomly replaces class label with null | Enables classifier-free guidance at inference |
| **EMA** (Exponential Moving Average) | Keeps a slowly-updated copy of weights | Produces smoother, higher-quality samples ⭐ |
| **Gradient clipping** (max norm = 1.0) | Limits how large gradients can be | Prevents training from exploding/diverging |
| **AdamW optimizer** | Adam with weight decay | Better generalization than vanilla Adam |
| **Cosine schedule** | Non-uniform noise levels | Better image quality (Nichol & Dhariwal, 2021) |

**The EMA formula** (updated after every training step):

$$\theta_{\text{EMA}} \leftarrow \alpha \cdot \theta_{\text{EMA}} + (1 - \alpha) \cdot \theta_{\text{model}}$$

where $\alpha = 0.9999$ typically. This means the EMA model is a **very slow-moving average** of the training model's weights.

> 🍕 **Analogy**: EMA is like judging a restaurant by its **average food quality over months**, not by one random visit. The average is much more reliable than any single snapshot.

📚 **Reference**: Nichol & Dhariwal (2021) — *"Improved Denoising Diffusion Probabilistic Models"*; Song & Ermon (2019) — *"Generative Modeling by Estimating Gradients of the Data Distribution"*

```python
def train(epochs=100, batch_size=128, lr=2e-4, p_uncond=0.1):
    """
    🎯 BEGINNER EXPLANATION:
    This function trains our diffusion model. Here's the high-level loop:
    
    1. Load Fashion-MNIST images (70,000 clothing images)
    2. For each image:
       a. Add random noise to it
       b. Ask the model: "what noise did I add?"
       c. Measure how wrong the model was (MSE loss)
       d. Adjust the model's weights to be less wrong next time
    3. Repeat for 100 epochs (~7 million training steps)
    
    🚗 Like teaching someone to restore old cars:
    - We damage cars in known ways (add noise)
    - Trainee tries to identify the damage (predict noise)
    - We grade their diagnosis (compute loss)
    - They learn from mistakes (backprogation)
    """
    device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
    
    # 📦 Load Fashion-MNIST: 28×28 grayscale images of clothing
    transform = transforms.Compose([transforms.ToTensor(), transforms.Normalize([0.5], [0.5])])
    dataset = datasets.FashionMNIST('.', train=True, download=True, transform=transform)
    loader = DataLoader(dataset, batch_size=batch_size, shuffle=True, num_workers=2, pin_memory=True)
    
    model = ConditionalUNet().to(device)
    sched = DiffusionSchedule(T=1000, schedule='cosine').to(device)
    opt = torch.optim.AdamW(model.parameters(), lr=lr)
    ema_model = create_ema(model)  # 🎯 Slowly-averaged copy for better quality
    
    print(f"Params: {sum(p.numel() for p in model.parameters()):,}")
    
    for epoch in range(epochs):
        model.train()
        losses = []
        for x, y in loader:
            x, y = x.to(device), y.to(device)
            
            # 🎲 Random condition dropout for CFG training
            # 10% of the time, replace label with "null" class
            # This teaches the model to generate WITHOUT a class label too
            drop = torch.rand(y.shape[0], device=device) < p_uncond
            y_input = y.clone()
            y_input[drop] = model.null_class
            
            # 🎯 Core training step:
            t = torch.randint(0, sched.T, (x.shape[0],), device=device)  # random timestep
            noise = torch.randn_like(x)                                    # random noise
            x_t, _ = sched.q_sample(x, t, noise)                          # make noisy image
            
            pred = model(x_t, t, y_input)        # model predicts the noise
            loss = F.mse_loss(pred, noise)         # how wrong was it?
            
            opt.zero_grad()
            loss.backward()
            nn.utils.clip_grad_norm_(model.parameters(), 1.0)  # prevent explosion
            opt.step()
            update_ema(ema_model, model)  # update slow-moving average
            losses.append(loss.item())
        
        print(f"Epoch {epoch+1}: loss={np.mean(losses):.4f}")
    
    return ema_model, sched
```

### 🏭 Production Training: Stable Diffusion Scale

In production systems like **Stable Diffusion** (Rombach et al., 2022), the training pipeline is MUCH larger:

| Aspect | Our Toy System | Stable Diffusion v1.5 | SDXL |
|:-------|:---------------|:---------------------|:-----|
| **Dataset** | Fashion-MNIST (60K) | LAION-5B (5 billion) | LAION-5B subset |
| **Resolution** | $28 \times 28$ | $512 \times 512$ | $1024 \times 1024$ |
| **Parameters** | ~2M | 860M | 3.5B |
| **Training time** | Minutes (1 GPU) | ~150K GPU-hours (A100) | ~300K GPU-hours |
| **Latent space** | None (pixel) | $64 \times 64 \times 4$ | $128 \times 128 \times 4$ |
| **Conditioning** | Class label (10) | CLIP text embeddings | Dual CLIP encoders |
| **Cost** | ~$0 | ~$600K | ~$1.2M |

> ⭐ **Key production insight**: Stable Diffusion doesn't work in pixel space — it compresses images through a VAE first (e.g., $512 \times 512 \times 3 \rightarrow 64 \times 64 \times 4$), making computation $64\times$ cheaper!

📚 **Reference**: Rombach et al. (2022) — *"High-Resolution Image Synthesis with Latent Diffusion Models"*

---

# 3️⃣ Sampling: DDPM, DDIM, CFG

> 🚗 **Car analogy**: Sampling is **driving the car** you just built! You start with a tank full of noise (random static) and the engine (U-Net) gradually converts it into a beautiful image. Different sampling methods are like different **transmission modes**: DDPM is a smooth automatic (1000 steps), DDIM is a sporty manual (50 steps), and CFG is the **steering** that keeps you on the road to the image you want.

### 🧮 The Math Behind Each Sampling Method

**DDPM Sampling** (original, 1000 steps):

$$x_{t-1} = \frac{1}{\sqrt{\alpha_t}} \left(x_t - \frac{\beta_t}{\sqrt{1 - \bar{\alpha}_t}} \epsilon_\theta(x_t, t)\right) + \sigma_t z$$

where $z \sim \mathcal{N}(0, I)$ and $\sigma_t = \sqrt{\beta_t}$.

> 🍕 **Analogy**: Like removing paint one thin layer at a time. Very thorough, very slow (1000 steps).

**DDIM Sampling** (deterministic, any number of steps):

$$x_{t-1} = \sqrt{\bar{\alpha}_{t-1}} \underbrace{\left(\frac{x_t - \sqrt{1-\bar{\alpha}_t} \cdot \epsilon_\theta}{\sqrt{\bar{\alpha}_t}}\right)}_{\text{predicted } x_0} + \sqrt{1 - \bar{\alpha}_{t-1}} \cdot \epsilon_\theta$$

> 🍕 **Analogy**: Like a power washer that skips steps — you jump from step 1000 → 980 → 960 → ... instead of going one at a time. Gets the same result 20x faster!

**CFG-enhanced prediction** (used in both DDPM and DDIM):

$$\tilde{\epsilon} = \underbrace{\epsilon_\theta(x_t, t, \varnothing)}_{\text{unconditional}} + w \cdot \left(\underbrace{\epsilon_\theta(x_t, t, c)}_{\text{conditional}} - \underbrace{\epsilon_\theta(x_t, t, \varnothing)}_{\text{unconditional}}\right)$$

> 🍕 **Analogy for the guidance scale $w$**:
> - $w = 0$: "Make whatever you want" (random, diverse)
> - $w = 1$: "Make something like this class" (balanced)
> - $w = 5-7$: "I really mean this class!" (high quality) ⭐
> - $w = 20$: "ONLY this class, nothing else!" (over-saturated, distorted)

### Sampling Method Comparison Table

| Method | Steps | Quality | Diversity | Speed | Deterministic? | Best For |
|:-------|:-----:|:-------:|:---------:|:-----:|:--------------:|:---------|
| **DDPM** | 1000 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 🐌 | No | Research, quality-first |
| **DDIM** (50 steps) | 50 | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 🚀 | Yes | **Production default** ⭐ |
| **DDIM** (20 steps) | 20 | ⭐⭐⭐ | ⭐⭐⭐ | 🚀🚀 | Yes | Real-time apps |
| **DPM-Solver++** | 20-25 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 🚀🚀 | Yes | **Best for production** ⭐ |
| **LCM** | 4-8 | ⭐⭐⭐⭐ | ⭐⭐⭐ | 🚀🚀🚀 | Yes | Real-time interactive |

📚 **Reference**: Song et al. (2021) — *"Denoising Diffusion Implicit Models"*; Lu et al. (2022) — *"DPM-Solver: A Fast ODE Solver for Diffusion Probabilistic Model Sampling"*

```python
@torch.no_grad()
def sample(model, sched, labels, method='ddim', steps=50, w=3.0, device='cuda'):
    """
    🎯 BEGINNER EXPLANATION:
    This function generates NEW images from pure noise!
    
    Process:
    1. Start with random noise (like TV static)
    2. Ask the model: "what noise is in this image?"
    3. Subtract some of that noise
    4. Repeat 50-1000 times until a clean image appears!
    
    The 'w' parameter (guidance scale) controls how strictly the model
    follows the class label:
    - w=1.0: model has creative freedom
    - w=5.0: model strongly follows the class (recommended) ⭐
    - w=15.0: model is obsessive about the class (can look weird)
    
    🚗 Like developing a photograph in a darkroom:
    the image slowly appears from nothing as you process it.
    """
    n = len(labels)
    x = torch.randn(n, 1, 28, 28, device=device)  # Start from pure noise!
    null_labels = torch.full_like(labels, model.null_class)
    
    if method == 'ddim':
        # DDIM: skip steps (e.g., 1000/50 = every 20th step)
        timesteps = list(range(0, sched.T, sched.T // steps))[::-1]
    else:
        # DDPM: every single step from T-1 to 0
        timesteps = list(range(sched.T))[::-1]
    
    for i, t in enumerate(timesteps):
        tb = torch.full((n,), t, device=device, dtype=torch.long)
        
        # 🎛️ Classifier-Free Guidance: run model TWICE
        eps_cond = model(x, tb, labels)        # "what noise if label is shoe?"
        eps_uncond = model(x, tb, null_labels)  # "what noise with no label?"
        eps = eps_uncond + w * (eps_cond - eps_uncond)  # amplify the difference!
        
        ab = sched.alpha_bars[t]
        a = sched.alphas[t]
        b = sched.betas[t]
        
        if method == 'ddim':
            # DDIM: deterministic, can skip steps
            x0_pred = (x - torch.sqrt(1-ab) * eps) / torch.sqrt(ab)  # predict x₀
            x0_pred = x0_pred.clamp(-1, 1)  # keep pixel values reasonable
            t_prev = timesteps[i+1] if i+1 < len(timesteps) else 0
            ab_prev = sched.alpha_bars[t_prev] if t_prev > 0 else torch.tensor(1.0).to(device)
            x = torch.sqrt(ab_prev) * x0_pred + torch.sqrt(1-ab_prev) * eps
        else:
            # DDPM: stochastic, one step at a time
            mean = (1/torch.sqrt(a)) * (x - b/torch.sqrt(1-ab) * eps)
            x = mean + (torch.sqrt(b)*torch.randn_like(x) if t > 0 else 0)
    
    return x.clamp(-1, 1)

# 🎨 Generate samples for each Fashion-MNIST class
# Classes: T-shirt, Trouser, Pullover, Dress, Coat, Sandal, Shirt, Sneaker, Bag, Boot
labels = torch.arange(10, device='cuda').repeat(4)  # 4 per class = 40 images
samples = sample(ema_model, sched, labels, method='ddim', steps=50, w=5.0)
```

### 🏭 Production Sampling: ComfyUI & Serving Infrastructure

In production (like Stable Diffusion deployments), sampling is **heavily optimized**:

```
ComfyUI Pipeline (Visual Node-Based Workflow):
┌──────────┐   ┌──────────┐   ┌──────────────┐   ┌───────────┐
│  Text     │──▶│  CLIP     │──▶│  KSampler    │──▶│  VAE      │──▶ Image
│  Prompt   │   │  Encode   │   │  (DPM++ 2M)  │   │  Decode   │
└──────────┘   └──────────┘   │  20 steps     │   └───────────┘
                               │  CFG=7.0      │
                               └──────────────┘
```

| Production Optimization | What It Does | Speedup |
|:------------------------|:-------------|:--------|
| **xFormers** | Memory-efficient attention | 2x less VRAM |
| **torch.compile** | JIT-compile the model | 1.3-2x faster |
| **Half precision (FP16)** | Use 16-bit floats | 2x less VRAM, 1.5x faster |
| **Batched CFG** | Run conditional + unconditional in one batch | 1.5x faster |
| **Token merging (ToMe)** | Merge similar attention tokens | 2x faster |
| **TensorRT** | NVIDIA's inference compiler | 3-5x faster |

> ⭐ **Production tip**: Use DPM-Solver++ with 20-25 steps for the best quality/speed tradeoff. Combined with FP16 + xFormers, you can generate a $512 \times 512$ image in ~2 seconds on an RTX 3090.

📚 **Reference**: Rombach et al. (2022) — *"High-Resolution Image Synthesis with Latent Diffusion Models"*; ComfyUI — open-source node-based Stable Diffusion interface

---

# 4️⃣ Evaluation

> 🚗 **Car analogy**: Evaluation is the **test track** — you've built the car, now let's see how it performs! FID score is like a lap time: lower is better. We compare generated images to real images across thousands of samples to get a reliable measurement.

### 📊 Understanding FID (Fréchet Inception Distance)

FID is the **gold standard metric** for image generation quality. Here's what it measures:

$$\text{FID} = \|\mu_r - \mu_g\|^2 + \text{Tr}\left(\Sigma_r + \Sigma_g - 2(\Sigma_r \Sigma_g)^{1/2}\right)$$

where:
- $\mu_r, \Sigma_r$ = mean and covariance of **real** image features
- $\mu_g, \Sigma_g$ = mean and covariance of **generated** image features
- Features are extracted from a pretrained Inception-v3 network

> 🍕 **Analogy for absolute beginners**: Imagine you're comparing two restaurants:
> - Take 10,000 dishes from Restaurant A (real data)
> - Take 10,000 dishes from Restaurant B (generated data)
> - Rate each dish on 2,048 qualities (Inception features)
> - FID measures how **different** the overall quality distributions are
> - FID = 0 means the restaurants are identical; FID = 300 means completely different

### FID Score Interpretation Guide

| FID Score | Quality Level | Real-World Example |
|:---------:|:-------------|:-------------------|
| 0-5 | 🟢 Excellent | State-of-the-art (SDXL on trained domain) |
| 5-15 | 🟢 Good | Production-ready Stable Diffusion |
| 15-30 | 🟡 Decent | Fine-tuned models, LoRA output |
| 30-50 | 🟡 Okay | Basic training, small models |
| 50-100 | 🟠 Poor | Under-trained or wrong hyperparameters |
| 100+ | 🔴 Bad | Random noise or broken model |

⭐ **Important**: FID requires **at least 10,000 samples** to be statistically meaningful! Using fewer samples gives unreliable scores.

📚 **Reference**: Heusel et al. (2017) — *"GANs Trained by a Two Time-Scale Update Rule Converge to a Local Nash Equilibrium"*; Parmar et al. (2022) — *"On Aliased Resizing and Surprising Subtleties in GAN Evaluation"*

```python
def evaluate_model(model, sched, dataset, n_samples=10000, device='cuda'):
    """
    🎯 BEGINNER EXPLANATION:
    This function measures HOW GOOD our model is at generating images.
    
    Process:
    1. Generate 10,000 fake images using our model
    2. Take 10,000 real images from Fashion-MNIST
    3. Extract "features" from both sets using a pretrained network
    4. Compute FID: a statistical distance between real & fake distributions
    
    🚗 Like comparing two car factories:
    - Factory A makes real cars (training data)  
    - Factory B makes our generated cars
    - FID = how different are the two factories' outputs?
    - Lower FID = our factory is closer to the real thing!
    """
    """Compute FID and per-class quality."""
    # Generate samples
    all_samples = []
    for i in range(0, n_samples, 100):
        labels = torch.randint(0, 10, (100,), device=device)
        batch = sample(model, sched, labels, method='ddim', steps=50, w=5.0)
        all_samples.append(batch.cpu())
    gen_images = torch.cat(all_samples)
    
    # Get real images
    real_images = torch.stack([dataset[i][0] for i in range(n_samples)])
    
    # Compute FID
    gen_feats = extract_features(gen_images)
    real_feats = extract_features(real_images)
    fid = compute_fid(real_feats, gen_feats)
    
    print(f"FID: {fid:.2f}")
    print(f"Generated {n_samples} samples")
    return fid
```

### 🏭 Production Evaluation: A/B Testing Image Quality

In production, you don't just measure FID — you run **A/B tests** to see if users prefer the new model:

```
Production Evaluation Pipeline:
┌─────────────────────────────────────────────────┐
│                A/B TEST FRAMEWORK                │
│                                                   │
│  Model A (current) ──┐                           │
│                       ├──▶ Show to random users   │
│  Model B (new)    ───┘    ├──▶ Track clicks      │
│                            ├──▶ Track shares     │
│                            ├──▶ Track saves      │
│                            └──▶ Track complaints │
│                                                   │
│  Automated Metrics:                               │
│  ├── FID (distribution quality)                  │
│  ├── CLIP Score (text-image alignment)           │
│  ├── Aesthetic Score (prettiness predictor)       │
│  └── Inference Latency (speed)                   │
└─────────────────────────────────────────────────┘
```

| Metric | What It Measures | Production Target |
|:-------|:----------------|:-----------------|
| **FID** ↓ | Overall distribution match | < 15 |
| **CLIP Score** ↑ | Text-image alignment | > 0.28 |
| **Aesthetic Score** ↑ | Subjective beauty | > 5.5 / 10 |
| **Latency** (P95) ↓ | Time per image | < 3 seconds |
| **User preference** ↑ | Human side-by-side tests | > 50% vs baseline |

> ⭐ **Production reality**: FID correlates with user preference about 70% of the time. The other 30% is captured by CLIP Score and human evaluation. **Always do human evaluation** for product launches!

📚 **Reference**: Radford et al. (2021) — *"Learning Transferable Visual Models From Natural Language Supervision"* (CLIP); Schuhmann et al. (2022) — *"LAION-5B: An Open Large-Scale Dataset for Training Next Generation Image-Text Models"*

---

# 5️⃣ What We Built (Days 61-75)

> 🚗 **The fully assembled car!** Here's every part we built and how it contributes to the complete system:

| Day | Topic | Key Contribution | Math | Car Part 🚗 |
|:---:|:------|:-----------------|:-----|:------------|
| 61 | Generative Landscape | GANs, VAEs, Flows vs Diffusion | — | 📋 Blueprints |
| 62 | Forward Process | $q(x_t \| x_0) = \mathcal{N}(\sqrt{\bar{\alpha}_t} x_0, (1-\bar{\alpha}_t)\mathbf{I})$ | Noise addition | 🔧 Paint stripper |
| 63 | Noise Schedules | Linear vs Cosine, SNR analysis | $\text{SNR}(t) = \bar{\alpha}_t / (1 - \bar{\alpha}_t)$ | ⚡ Speed control |
| 64 | Reverse Process | $p_\theta(x_{t-1} \| x_t)$, noise parameterization | Denoising step | 🎨 Paint sprayer |
| 65 | DDPM Loss | ELBO → $\mathbb{E}[\|\epsilon - \epsilon_\theta\|^2]$ | Simple MSE loss | 📏 Quality tester |
| 66 | Score Matching | Score = $\nabla_x \log p(x)$, SDE unification | Score function | 🧭 GPS navigation |
| 67 | U-Net | ResBlocks, attention, time embed | Architecture | 🔧 Engine block |
| 68 | Conditioning | CFG: $\tilde{\epsilon} = \epsilon_\varnothing + w(\epsilon_c - \epsilon_\varnothing)$ | Guidance | 🎮 Steering wheel |
| 69 | Training | Complete DDPM training pipeline | Optimization | 🏭 Assembly line |
| 70 | DDPM vs DDIM | 20x speedup with DDIM | Deterministic ODE | 🏎️ Turbo mode |
| 71 | Latent Diffusion | VAE + U-Net + CLIP = Stable Diffusion | Latent space | 🔋 Engine upgrade |
| 72 | Advanced Sampling | DPM-Solver, Karras, production techniques | ODE solvers | ⚙️ Transmission |
| 73 | Evaluation | FID, IS, CLIP Score | $\text{FID} = \|\mu_r - \mu_g\|^2 + ...$ | 🏁 Test track |
| 74 | Next Gen | Consistency Models, Flow Matching | One-step generation | 🚀 Concept features |
| 75 | **Capstone** | **Complete class-conditional system** | **All combined** | **🚗 Full car!** |

### 🏗️ System Architecture Diagram (Everything Connected)

```
┌─────────────────────────── COMPLETE DIFFUSION SYSTEM ────────────────────────────┐
│                                                                                   │
│  ┌──── TRAINING PIPELINE ────────────────────────────────────────────────────┐    │
│  │                                                                            │    │
│  │  Dataset ──▶ DataLoader ──▶ Forward Process ──▶ U-Net ──▶ Loss ──▶ Adam  │    │
│  │  (Day 69)    (batches)     (Day 62-63)        (Day 67)  (Day 65)         │    │
│  │                                                  ↑                         │    │
│  │                              Conditioning ───────┘                         │    │
│  │                              (Day 68: CFG)                                 │    │
│  └────────────────────────────────────────────────────────────────────────────┘    │
│                                          │                                         │
│                                    Trained Weights                                 │
│                                    + EMA Weights                                   │
│                                          │                                         │
│  ┌──── INFERENCE PIPELINE ──────────────▼────────────────────────────────────┐    │
│  │                                                                            │    │
│  │  Random Noise ──▶ Sampling Loop ──▶ Denoised Image ──▶ Post-Processing   │    │
│  │  z ~ N(0,I)       (Day 70-72)       (generated!)       (clip, scale)     │    │
│  │                      │                                                     │    │
│  │              ┌───────┼───────┐                                             │    │
│  │              DDPM   DDIM  DPM++                                            │    │
│  │              1000    50    20  steps                                        │    │
│  └────────────────────────────────────────────────────────────────────────────┘    │
│                                          │                                         │
│  ┌──── EVALUATION ──────────────────────▼────────────────────────────────────┐    │
│  │                                                                            │    │
│  │  Generated Images ──▶ Feature Extraction ──▶ FID / CLIP / IS              │    │
│  │  Real Images ────────▶ Feature Extraction ──┘   (Day 73)                  │    │
│  └────────────────────────────────────────────────────────────────────────────┘    │
│                                                                                   │
└───────────────────────────────────────────────────────────────────────────────────┘
```

---

# 📝 Founder Edge

> 🎯 **This section is for entrepreneurs** — how to turn diffusion model knowledge into a real product or startup.

**If building an image generation startup:**
- Use Stable Diffusion / SDXL as your base (don't train from scratch)
- Fine-tune with LoRA for your domain (100-1000 images)
- Use LCM or Consistency distillation for speed (4-step generation)
- CFG scale 5-7 for best quality/diversity balance
- DPM-Solver++ with 20-25 steps for production
- Measure FID on your domain data, not ImageNet

### 🏭 Production Deployment Architecture (Stable Diffusion at Scale)

```
┌───────────────────── PRODUCTION DEPLOYMENT ──────────────────────┐
│                                                                   │
│  Users ──▶ Load Balancer ──▶ API Gateway ──▶ Queue (Redis/SQS)  │
│                                                   │               │
│                                ┌──────────────────┼──────────┐   │
│                                ▼                  ▼          ▼   │
│                          ┌─────────┐        ┌─────────┐    ...  │
│                          │ GPU Pod  │        │ GPU Pod  │         │
│                          │ (A100)   │        │ (A100)   │         │
│                          │          │        │          │         │
│                          │ SD Model │        │ SD Model │         │
│                          │ +TensorRT│        │ +TensorRT│         │
│                          │ +FP16    │        │ +FP16    │         │
│                          └────┬─────┘        └────┬─────┘         │
│                               │                    │              │
│                               ▼                    ▼              │
│                          ┌─────────────────────────────┐         │
│                          │    Object Storage (S3)       │         │
│                          │    Generated Images          │         │
│                          └──────────────┬──────────────┘         │
│                                         │                         │
│                                         ▼                         │
│                          ┌─────────────────────────────┐         │
│                          │    CDN (CloudFront)          │         │
│                          │    Serve to Users            │         │
│                          └─────────────────────────────┘         │
└───────────────────────────────────────────────────────────────────┘
```

### 💰 Cost Optimization Table

| Strategy | Impact | Implementation Difficulty |
|:---------|:-------|:-------------------------|
| **FP16 inference** | 50% VRAM savings | Easy (one line of code) |
| **TensorRT compilation** | 3-5x speedup | Medium |
| **Batch requests** | 2-4x throughput | Medium |
| **Model caching** | Eliminate cold starts | Easy |
| **Spot/preemptible GPUs** | 70% cost savings | Medium |
| **Progressive resolution** | 2x cheaper for thumbnails | Easy |
| **Async generation + queue** | Handle traffic spikes | Medium |
| **LoRA only (no full fine-tune)** | 100x less training cost | Easy ⭐ |

### 🧪 A/B Testing Framework for Image Generation

When deploying a new model version, **always A/B test**:

1. **Safety gate**: Run automated safety classifier on outputs
2. **Quality gate**: Measure FID and CLIP Score on sample batch
3. **Speed gate**: Verify P95 latency is within SLA
4. **User gate**: Show 50/50 split to users, measure engagement
5. **Rollout**: If all gates pass, gradually increase to 100%

> ⭐ **Startup insight**: Your competitive advantage is NOT the model — it's your **pipeline**: data curation, fine-tuning automation, safety filtering, and user experience. The model is a commodity; the system around it is the moat.

📚 **Reference**: Rombach et al. (2022) — *"High-Resolution Image Synthesis with Latent Diffusion Models"*; Podell et al. (2023) — *"SDXL: Improving Latent Diffusion Models for High-Resolution Image Synthesis"*

**Next Phase**: Days 76-90 — Systems & Hardware for AI.

---

# 📋 Complete Diffusion System Cheat Sheet

> ⭐ **Pin this!** A single-page reference for everything in the diffusion pipeline.

## Core Equations

| What | Equation | When Used |
|:-----|:---------|:----------|
| **Forward process** | $x_t = \sqrt{\bar{\alpha}_t} \, x_0 + \sqrt{1 - \bar{\alpha}_t} \, \epsilon$ | Training (adding noise) |
| **Training loss** | $\mathcal{L} = \mathbb{E}\left[\|\epsilon - \epsilon_\theta(x_t, t)\|^2\right]$ | Training (optimizing) |
| **DDPM reverse** | $x_{t-1} = \frac{1}{\sqrt{\alpha_t}}\left(x_t - \frac{\beta_t}{\sqrt{1-\bar{\alpha}_t}}\epsilon_\theta\right) + \sigma_t z$ | Sampling (slow) |
| **DDIM reverse** | $x_{t-1} = \sqrt{\bar{\alpha}_{t-1}} \hat{x}_0 + \sqrt{1-\bar{\alpha}_{t-1}} \cdot \epsilon_\theta$ | Sampling (fast) |
| **CFG** | $\tilde{\epsilon} = \epsilon_\varnothing + w(\epsilon_c - \epsilon_\varnothing)$ | Sampling (guided) |
| **FID** | $\|\mu_r - \mu_g\|^2 + \text{Tr}(\Sigma_r + \Sigma_g - 2\sqrt{\Sigma_r\Sigma_g})$ | Evaluation |

## Hyperparameter Quick Reference

| Parameter | Recommended Value | Notes |
|:----------|:-----------------|:------|
| Timesteps $T$ | 1000 | Standard for DDPM |
| Schedule | Cosine | Better than linear for most tasks |
| Learning rate | $2 \times 10^{-4}$ | With AdamW |
| EMA decay | 0.9999 | Higher = smoother but slower adaptation |
| CFG dropout $p$ | 0.1 | 10% unconditional during training |
| CFG scale $w$ | 5.0 - 7.5 | Higher = more faithful, less diverse |
| DDIM steps | 50 | Good quality/speed tradeoff |
| DPM++ steps | 20-25 | **Best for production** ⭐ |
| Batch size | 128-256 | Larger = more stable training |
| Gradient clip | 1.0 | Prevents divergence |

## Quick Debugging Guide

| Symptom | Likely Cause | Fix |
|:--------|:-------------|:----|
| 🔴 Loss = NaN | Learning rate too high | Reduce LR by 10x, add grad clipping |
| 🔴 All samples look the same | Mode collapse or CFG too high | Lower $w$, check CFG dropout is working |
| 🟡 Blurry samples | Not enough training or steps | Train longer, use more sampling steps |
| 🟡 Wrong class generated | Conditioning not working | Check class embedding, verify CFG dropout |
| 🟡 High FID score | Under-trained model | More epochs, larger model, better schedule |
| 🟢 Good FID but ugly images | FID doesn't capture aesthetics | Add CLIP Score + human eval |

---

# 📚 Resources

## 📖 Books

| Book | Author(s) | Why Read It |
|:-----|:----------|:------------|
| *Understanding Deep Learning* | Simon Prince (2023) | Ch. 18 covers diffusion models with clear math derivations ⭐ |
| *Probabilistic Machine Learning: Advanced Topics* | Kevin Murphy (2023) | Ch. 25 — rigorous treatment of score matching & SDEs |
| *Deep Learning* | Goodfellow, Bengio, Courville (2016) | Foundation chapters on probability, optimization, VAEs |
| *Generative Deep Learning* (2nd ed.) | David Foster (2023) | Practical guide with Keras code for diffusion models |
| *Foundations of Computer Vision* | Torralba, Isola, Freeman (2023) | Image understanding context for generation |

## 📄 Key Papers (Reading Order)

| # | Paper | Year | Key Contribution |
|:-:|:------|:----:|:----------------|
| 1 | Ho et al. — *Denoising Diffusion Probabilistic Models* | 2020 | The foundational DDPM paper ⭐ |
| 2 | Song et al. — *Score-Based Generative Modeling through SDEs* | 2021 | Unified SDE framework |
| 3 | Nichol & Dhariwal — *Improved Denoising Diffusion Probabilistic Models* | 2021 | Cosine schedule, improved training |
| 4 | Song et al. — *Denoising Diffusion Implicit Models (DDIM)* | 2021 | 10-50x faster sampling |
| 5 | Dhariwal & Nichol — *Diffusion Models Beat GANs on Image Synthesis* | 2021 | Classifier guidance, U-Net improvements |
| 6 | Ho & Salimans — *Classifier-Free Diffusion Guidance* | 2022 | CFG — the standard for control ⭐ |
| 7 | Rombach et al. — *High-Resolution Image Synthesis with Latent Diffusion Models* | 2022 | Stable Diffusion / Latent Diffusion ⭐ |
| 8 | Lu et al. — *DPM-Solver: A Fast ODE Solver for Diffusion* | 2022 | Production-grade fast sampling |
| 9 | Podell et al. — *SDXL: Improving Latent Diffusion Models* | 2023 | SDXL architecture improvements |
| 10 | Song et al. — *Consistency Models* | 2023 | Single-step generation |
| 11 | Lipman et al. — *Flow Matching for Generative Modeling* | 2023 | Next-gen diffusion alternative |
| 12 | Esser et al. — *Scaling Rectified Flow Transformers* (SD3) | 2024 | Stable Diffusion 3 architecture |

## 🔗 Implementation Repositories

| Repository | Description | Stars |
|:-----------|:------------|:------|
| [CompVis/stable-diffusion](https://github.com/CompVis/stable-diffusion) | Original Stable Diffusion v1 | 60K+ |
| [Stability-AI/generative-models](https://github.com/Stability-AI/generative-models) | SDXL & SD2 official code | 20K+ |
| [huggingface/diffusers](https://github.com/huggingface/diffusers) | **Best library for diffusion models** ⭐ | 25K+ |
| [comfyanonymous/ComfyUI](https://github.com/comfyanonymous/ComfyUI) | Node-based UI for production workflows | 40K+ |
| [lucidrains/denoising-diffusion-pytorch](https://github.com/lucidrains/denoising-diffusion-pytorch) | Clean DDPM implementation for learning | 10K+ |
| [openai/consistency_models](https://github.com/openai/consistency_models) | Official consistency models code | 5K+ |

## 🎓 Courses & Tutorials

| Resource | Format | Level |
|:---------|:-------|:------|
| Hugging Face Diffusion Models Course | Online (free) | Beginner ⭐ |
| Lilian Weng — *"What are Diffusion Models?"* (blog) | Blog post | Intermediate |
| Stanford CS236 — Deep Generative Models | Lecture videos | Advanced |
| fast.ai — Practical Deep Learning Part 2 | Video course | Intermediate |
| Yannic Kilcher — Diffusion paper walkthroughs | YouTube | Intermediate |

## 🏢 Stability AI & Industry Papers

| Paper/Resource | What It Covers |
|:---------------|:---------------|
| Stability AI Technical Reports | Architecture decisions behind SD 1.5, 2.0, XL, 3.0 |
| LAION-5B Dataset Paper | How the training data was collected and filtered |
| DeepFloyd IF Technical Report | Pixel-space diffusion at scale |
| Midjourney (no public paper) | Proprietary aesthetic fine-tuning (reverse-engineered in community) |

---

> 🎉 **Congratulations!** You've completed the entire diffusion models curriculum. You now understand every component from mathematical foundations to production deployment. Go build something amazing! 🚀
