📘 DAY 1 — Generative Models Landscape: GANs, VAEs, Flows & Diffusion

Goal:

Survey the complete landscape of generative models — understand the mathematical foundations, trade-offs, and historical evolution that led to diffusion models becoming the dominant paradigm for image generation.

---

# 1️⃣ What Are Generative Models?

> 🎯 **Beginner Analogy**: Imagine you've seen thousands of cat photos. A generative model is like training your brain so well that you can **dream up brand-new cats** that never existed — but look totally real! Instead of just *recognizing* cats (that's classification), you're *creating* them from scratch. 🐱✨

Given data distribution $p_{\text{data}}(x)$, learn a model $p_\theta(x)$ that can:
1. **Sample**: Generate new $x \sim p_\theta(x)$
2. **Evaluate**: Compute $p_\theta(x)$ for any $x$ (not always possible)
3. **Learn features**: Extract useful representations

### 🧠 Why Should You Care? (Even If You're Not a Researcher)

Generative models power some of the most mind-blowing technology you use today:

| 🏭 Real-World Application | 🤖 Model Behind It |
|---|---|
| Stable Diffusion / Midjourney (image generation) | VAE + Diffusion |
| DALL-E (text-to-image) | VAE + Transformer |
| StyleGAN (photorealistic faces) | GAN |
| Drug discovery (novel molecules) | VAE / Flow-based |
| deepfakes, voice cloning | GAN-based |
| GitHub Copilot code suggestions | Transformer (generative) |

### ⭐ The Three Big Families (Mental Map)

Think of generative models as **three rival art schools**, each with a different philosophy:

| Art School 🎨 | Philosophy | Real Name |
|---|---|---|
| "The Rivalry" | Two artists compete — one forges, one detects | **GAN** |
| "The Compressor" | Squeeze art into a tiny code, then reconstruct | **VAE** |
| "The Shape-Shifter" | Smoothly morph a simple shape into a masterpiece | **Normalizing Flow** |
| "The Noise Whisperer" | Start with static TV noise, gradually refine into art | **Diffusion** |

---

# 2️⃣ Generative Adversarial Networks (GANs)

> 🎨 **Beginner Analogy — The Art Forger vs. The Detective**: Imagine an art forger 🎭 (the **Generator**) trying to create fake Picasso paintings to fool a detective 🔍 (the **Discriminator**). The detective studies real Picassos and tries to catch fakes. Over time, the forger gets SO good that even the detective can't tell the difference. That's a GAN! Both get better by competing with each other.

## Core Idea: Two-player game
- **Generator G**: maps noise $z$ → fake data $G(z)$
- **Discriminator D**: classifies real vs fake

### ⭐ The GAN Objective (The "Rules of the Game")

$$\min_G \max_D \; \mathbb{E}_{x \sim p_{\text{data}}}[\log D(x)] + \mathbb{E}_{z \sim p_z}[\log(1 - D(G(z)))]$$

> 🧩 **Breaking this down for beginners:**
> - $D(x)$ = Discriminator's confidence that $x$ is **real** (0 to 1)
> - $G(z)$ = Generator takes random noise $z$ and produces a fake image
> - $\log D(x)$ → Discriminator wants this **HIGH** (correctly say "real!" for real images)
> - $\log(1 - D(G(z)))$ → Discriminator wants this **HIGH** (correctly say "fake!" for generated images)  
> - Generator wants $D(G(z)) \to 1$ — fool the detective!
> - Discriminator wants $D(G(z)) \to 0$ — catch the forger!
>
> 📝 It's like a **seesaw**: when one player gets better, the other must improve too. Training ends when neither can gain an advantage — a **Nash equilibrium**.

### 🔬 Deep Research: The Original GAN Paper

> 📄 **Goodfellow, I., Pouget-Abadie, J., Mirza, M., Xu, B., Warde-Farley, D., Ozair, S., Courville, A., & Bengio, Y. (2014).** *"Generative Adversarial Nets."* NeurIPS 2014.
>
> This landmark paper proved that when $G$ and $D$ have enough capacity, the GAN game has a **global optimum** where $p_G = p_{\text{data}}$ — meaning the generator's output distribution perfectly matches the real data. The optimal discriminator at that point is $D^*(x) = \frac{1}{2}$ everywhere (it literally can't tell real from fake — a coin flip!).
>
> ⭐ **Key Insight**: GANs implicitly minimize the **Jensen-Shannon Divergence** between $p_{\text{data}}$ and $p_G$:
> $$\text{JSD}(p_{\text{data}} \| p_G) = \frac{1}{2} D_{KL}(p_{\text{data}} \| m) + \frac{1}{2} D_{KL}(p_G \| m), \quad m = \frac{p_{\text{data}} + p_G}{2}$$

## Implementation Sketch
```python
class Generator(nn.Module):
    def __init__(self, z_dim=100, img_dim=784):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(z_dim, 256), nn.ReLU(),
            nn.Linear(256, 512), nn.ReLU(),
            nn.Linear(512, img_dim), nn.Tanh(),
        )
    def forward(self, z):
        return self.net(z)

class Discriminator(nn.Module):
    def __init__(self, img_dim=784):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(img_dim, 512), nn.LeakyReLU(0.2),
            nn.Linear(512, 256), nn.LeakyReLU(0.2),
            nn.Linear(256, 1), nn.Sigmoid(),
        )
    def forward(self, x):
        return self.net(x)
```

> 💡 **Code Walkthrough for Beginners:**
> - `z_dim=100`: The Generator starts with 100 random numbers (like rolling 100 dice 🎲)
> - `nn.Tanh()`: Squishes output to [-1, 1] range (pixel values)
> - `nn.Sigmoid()`: Squishes Discriminator output to [0, 1] (probability: real or fake?)
> - `nn.LeakyReLU(0.2)`: Lets a tiny bit of signal through even for negative values — prevents "dead neurons" 💀

### 🏭 Production Spotlight: StyleGAN2

> 📄 **Karras, T., Laine, S., Aittala, M., Hellsten, J., Lehtinen, J., & Aila, T. (2020).** *"Analyzing and Improving the Image Quality of StyleGAN."* CVPR 2020.
>
> StyleGAN2 (by NVIDIA) generates **photorealistic human faces at 1024×1024** resolution that are indistinguishable from real photos. Used in:
> - 🎮 Game character generation
> - 🎬 Film VFX and digital doubles  
> - 🛍️ Fashion: virtual try-on and model generation
> - ⚠️ Deepfakes (raises ethical concerns!)
>
> Visit [thispersondoesnotexist.com](https://thispersondoesnotexist.com) — every face is GAN-generated!

## Pros & Cons
✓ Sharp, high-quality samples  
✓ Fast generation (single forward pass)  
✗ Mode collapse (generates limited variety)  
✗ Training instability (delicate G/D balance)  
✗ No likelihood evaluation  
✗ No latent space for manipulation  

> 🔄 **Mode Collapse Explained Simply**: Imagine the forger discovers that painting **only sunflowers** fools the detective 90% of the time. So the forger stops trying other subjects and ONLY paints sunflowers. The GAN gets "stuck" generating one type of output. This is mode collapse — the generator loses diversity. 🌻🌻🌻

---

# 3️⃣ Variational Autoencoders (VAEs)

> 📦 **Beginner Analogy — The ZIP File for Images**: Imagine you want to email a photo but the file is too big. You **compress** it (ZIP it) into a tiny file, send it, and the receiver **decompresses** it. A VAE does the same thing for data:
> 1. **Encoder** 🔒: Compresses the image into a tiny "code" (called the **latent vector** $z$) — like creating a ZIP file
> 2. **Latent Space** 🌌: The compressed representation — a magical space where similar images live nearby 
> 3. **Decoder** 🔓: Decompresses the code back into an image — like unzipping
>
> The twist? Once trained, you can **sample random points** in the latent space and decode them into **brand-new images** that never existed! 🎉

## Core Idea: Learn latent space + decoder
- **Encoder**: $q_\phi(z|x)$ — compress data to latent
- **Decoder**: $p_\theta(x|z)$ — reconstruct from latent

### ⭐ The ELBO (Evidence Lower BOund) — The VAE's Training Objective

$$\text{ELBO} = \underbrace{\mathbb{E}_{q_\phi(z|x)}[\log p_\theta(x|z)]}_{\text{Reconstruction Quality}} - \underbrace{D_{KL}(q_\phi(z|x) \| p(z))}_{\text{Regularization}}$$

> 🧩 **Breaking this down for beginners:**
> - **First term** (Reconstruction): "How well can we rebuild the original image from the compressed code?" → Higher is better! Think: "Does the unzipped file look like the original?" 📸
> - **Second term** (KL Divergence penalty): "Keep the latent space organized and smooth!" → Forces the encoder to produce codes that follow a nice bell-curve (Gaussian) shape. Without this, the latent space becomes a chaotic mess where nearby points produce wildly different images. 🎯
> - **Trade-off**: Too much reconstruction focus → messy latent space. Too much KL penalty → blurry images. Finding the sweet spot is key! ⚖️

### ⭐ The Reparameterization Trick (A Brilliant Hack! 🪄)

The encoder outputs $\mu$ (mean) and $\sigma$ (standard deviation), then we sample:

$$z = \mu + \sigma \odot \varepsilon, \quad \varepsilon \sim \mathcal{N}(0, I)$$

> 🧩 **Why is this needed?** You can't backpropagate gradients through random sampling (randomness "breaks" the gradient chain). The trick: instead of sampling $z$ directly, we sample $\varepsilon$ from a fixed distribution and compute $z$ deterministically from $\mu$, $\sigma$, and $\varepsilon$. Now gradients can flow through $\mu$ and $\sigma$! It's like saying: "The randomness isn't in our model — it's in this separate dice roll $\varepsilon$ that we just multiply in."

### 🔬 Deep Research: The Original VAE Paper

> 📄 **Kingma, D. P. & Welling, M. (2014).** *"Auto-Encoding Variational Bayes."* ICLR 2014.
>
> This foundational paper introduced the VAE framework — combining **variational inference** (from Bayesian statistics) with **deep neural networks**. Key contributions:
> - The reparameterization trick enabling end-to-end gradient training
> - Showed that the ELBO is a **lower bound** on the true log-likelihood: $\text{ELBO} \leq \log p(x)$
> - The gap between ELBO and true log-likelihood equals $D_{KL}(q_\phi(z|x) \| p(z|x))$ — how well the encoder approximates the true posterior

```python
class VAE(nn.Module):
    def __init__(self, input_dim=784, latent_dim=20):
        super().__init__()
        self.encoder = nn.Sequential(nn.Linear(input_dim, 400), nn.ReLU())
        self.mu_layer = nn.Linear(400, latent_dim)
        self.logvar_layer = nn.Linear(400, latent_dim)
        self.decoder = nn.Sequential(
            nn.Linear(latent_dim, 400), nn.ReLU(),
            nn.Linear(400, input_dim), nn.Sigmoid(),
        )
    
    def encode(self, x):
        h = self.encoder(x)
        return self.mu_layer(h), self.logvar_layer(h)
    
    def reparameterize(self, mu, logvar):
        std = torch.exp(0.5 * logvar)
        eps = torch.randn_like(std)
        return mu + eps * std
    
    def forward(self, x):
        mu, logvar = self.encode(x)
        z = self.reparameterize(mu, logvar)
        x_recon = self.decoder(z)
        recon_loss = F.binary_cross_entropy(x_recon, x, reduction='sum')
        kl_loss = -0.5 * torch.sum(1 + logvar - mu.pow(2) - logvar.exp())
        return x_recon, recon_loss + kl_loss
```

> 💡 **Code Walkthrough for Beginners:**
> - `latent_dim=20`: The entire image gets squeezed into just 20 numbers! (784 pixels → 20 numbers = huge compression 🗜️)
> - `mu_layer` & `logvar_layer`: Instead of encoding to a single point, we encode to a **cloud** (mean + spread). This is what makes a VAE different from a regular autoencoder!
> - `reparameterize`: The magic trick — $z = \mu + \sigma \cdot \varepsilon$ — makes training possible
> - `kl_loss = -0.5 * sum(1 + logvar - mu² - exp(logvar))`: This is the **closed-form KL divergence** between two Gaussians. No sampling needed! 📐

### 🏭 Production Spotlight: VAEs in the Wild

| Product | How VAE is Used |
|---|---|
| **Stable Diffusion** 🖼️ | VAE encoder compresses 512×512 images → 64×64 latent space. Diffusion happens in latent space (faster!). VAE decoder converts back to pixel space. |
| **DALL-E** (original) 🎨 | Discrete VAE tokenizes images → feeds to Transformer for text-to-image generation |
| **Drug Discovery** 💊 | VAE learns latent space of molecular structures. Sample new points → decode to novel drug candidates! (Gómez-Bombarelli et al., 2018) |
| **Music Generation** 🎵 | Google Magenta's MusicVAE compresses melodies into latent space, enables smooth interpolation between tunes |

## Pros & Cons
✓ Stable training  
✓ Latent space for manipulation  
✓ Likelihood estimation (ELBO)  
✗ Blurry samples (due to Gaussian decoder)  
✗ Limited sample quality  

> 🤔 **Why are VAE samples blurry?** The Gaussian decoder assumption means the model optimizes for the **average** of all possible outputs — and the average of many sharp images is a blurry image! Imagine overlaying 100 slightly different cat photos — the result is a blurry cat. Newer approaches (VQ-VAE, VAE-GAN) fix this.

---

# 4️⃣ Normalizing Flows

> 🌊 **Beginner Analogy — Stretching & Squishing Play-Doh**: Imagine you start with a perfect sphere of Play-Doh 🟡 (a simple Gaussian distribution). Now you **stretch, twist, squish, and fold** it through a series of steps — but here's the rule: every step must be **reversible** (you can always undo it). After many transformations, that simple sphere becomes a complex shape (like a dinosaur 🦕). That's a normalizing flow! You transform a simple, boring distribution into a complex, interesting one — step by step.

## Core Idea: Chain of invertible transformations
$z_0 \sim p(z)$ → $f_1$ → $f_2$ → ... → $f_K$ → $x$

### ⭐ Exact Likelihood via Change of Variables

$$\log p(x) = \log p(z_0) - \sum_{k=1}^{K} \log \left| \det \frac{\partial f_k}{\partial z_{k-1}} \right|$$

> 🧩 **Breaking this down for beginners:**
> - $\log p(z_0)$: How likely is the starting point in the simple distribution? (easy to compute — it's just a Gaussian!)
> - $\det \frac{\partial f_k}{\partial z_{k-1}}$: The **Jacobian determinant** — measures how much each transformation **stretches or compresses** space at that point
> - If you stretch space (spread things out), probability density goes **down** ⬇️
> - If you compress space (pack things together), probability density goes **up** ⬆️
> - Think of it like this: if you stretch a rubber sheet 🧘, the ink dots spread apart (less dense). The Jacobian tracks exactly how much stretching/compression happens.
>
> ⭐ **The Magic**: Unlike GANs and VAEs, flows give you the **exact** probability of any data point — no approximations needed!

### 🔬 Deep Research: The Normalizing Flows Paper

> 📄 **Rezende, D. J. & Mohamed, S. (2015).** *"Variational Inference with Normalizing Flows."* ICML 2015.
>
> This paper showed how to use a chain of invertible transformations to create flexible posterior distributions for variational inference. Key insight: by stacking simple, invertible transforms (like **planar flows** and **radial flows**), you can turn a simple Gaussian into an arbitrarily complex distribution.
>
> Later work expanded this with more powerful architectures:
> - **RealNVP** (Dinh et al., 2017): Affine coupling layers — fast and parallelizable
> - **Glow** (Kingma & Dhariwal, 2018): Invertible 1×1 convolutions — used for face manipulation
> - **Neural Spline Flows** (Durkan et al., 2019): Monotonic rational-quadratic splines — very flexible

### 🏭 Production Applications of Flows

| Application | How Flows Help |
|---|---|
| 🔊 **WaveGlow** (NVIDIA) | Generates speech audio in real-time using flow-based architecture |
| 🧬 **Molecular generation** | Exact likelihoods help evaluate how "realistic" generated molecules are |
| 📊 **Density estimation** | Financial risk modeling — exact probability of extreme market events |
| 🖼️ **Glow** (OpenAI) | Realistic face manipulation: aging, adding glasses, changing expressions |

✓ Exact likelihood  
✓ Invertible (encode AND decode)  
✗ Restrictive architectures (must be invertible)  
✗ Lower quality than GANs/Diffusion  

> 💡 **Key Trade-off**: The requirement that every transformation be invertible with a computable Jacobian determinant severely limits what architectures you can use. This is why flows generally produce lower-quality samples than GANs — they sacrifice expressiveness for mathematical exactness.

---

# 5️⃣ Diffusion Models: Why They Won

> 📺 **Beginner Analogy — The Noise Whisperer**: Imagine you have a beautiful painting 🖼️. Day by day, someone sprinkles a tiny bit of dust on it. After 1000 days, it's completely buried under random dust — unrecognizable. Now imagine training an AI to look at any dusty painting and predict: "What should I wipe away to make it slightly cleaner?" If the AI learns this single step really well, you can start with **pure dust** (random noise) and clean it step-by-step to reveal a painting that never existed before. That's diffusion! 🪄

## Core Idea: Gradually add noise, learn to reverse

Forward: $x_0 \to x_1 \to \dots \to x_T \approx \mathcal{N}(0, I)$ (easy, fixed)
Reverse: $x_T \to x_{T-1} \to \dots \to x_0$ (learned)

> 🧩 **Why this works so well:**
> - **Forward process**: Dead simple. Just add Gaussian noise at each step. No learning needed! 
> - **Reverse process**: A neural network learns to **predict and remove** the noise at each step
> - **Key insight**: Each individual denoising step is a **tiny, easy** problem. But chaining 1000 tiny steps together solves the **massive, hard** problem of generating realistic images from scratch!

## Why Diffusion Won

| Property | GAN | VAE | Flow | Diffusion |
|----------|-----|-----|------|-----------|
| Sample quality | High | Low | Medium | Highest |
| Training stability | Poor | Good | Good | Very Good |
| Mode coverage | Poor | Good | Good | Excellent |
| Likelihood | No | ELBO | Exact | ELBO |
| Sample speed | ⚡ Fast | ⚡ Fast | ⚡ Fast | 🐌 Slow (improving) |
| Scalability | Moderate | Good | Limited | Excellent |

The killer advantage: **quality + diversity + stability** simultaneously.

> ⭐ **Why did GANs lose the crown?** GANs ruled image generation from 2014-2020. But their Achilles heel was **mode collapse** (limited diversity) and **training instability** (the Generator-Discriminator balance is notoriously fragile). Diffusion models achieved **better quality AND better diversity AND stable training** — the holy trinity. The only downside? Speed. But techniques like DDIM, consistency models, and latent diffusion are rapidly closing that gap. 🏎️

---

# 6️⃣ The Diffusion Model Family

> 🌳 **Think of this as a family tree** — all diffusion models share the same core idea (add noise → learn to remove it), but they branched into different approaches. In 2021, Song et al. showed they're all actually the **same thing** viewed from different angles! 🤯

```
                    Diffusion Models
                    /              \
          Score-Based           Denoising
          (Song & Ermon)        (Ho et al.)
              |                     |
         NCSN, SDE             DDPM, DDIM
              \                   /
               \                /
              Unified Framework
              (Score SDE, Song 2021)
                     |
              ┌──────┼──────┐
              |      |      |
           Latent  Guided  Consistency
           Diffusion  (CFG)  Models
```

> 📝 **Quick Decoder Ring:**
> - **DDPM** = Denoising Diffusion Probabilistic Models — the paper that kicked off the revolution (Ho et al., 2020)
> - **DDIM** = Deterministic version — same quality, fewer steps (faster! ⚡)
> - **Score-based** = Instead of predicting noise, predict the "gradient" pointing toward data
> - **Latent Diffusion** = Do diffusion in compressed (VAE) space — this is how **Stable Diffusion** works! 🖼️
> - **CFG** = Classifier-Free Guidance — makes outputs match text prompts more closely
> - **Consistency Models** = Skip the iterative process — generate in **one step** (Luo et al., 2023)

---

# 7️⃣ Mathematical Framework Comparison

> ⭐ **This is the section to bookmark.** Each generative model family has a fundamentally different mathematical objective. Understanding these equations is understanding the **soul** of each approach. 🎯

**GAN**: $\min_G \max_D \; V(G, D)$ — adversarial objective, no explicit density

$$V(G, D) = \mathbb{E}_{x \sim p_{\text{data}}}[\log D(x)] + \mathbb{E}_{z \sim p_z}[\log(1 - D(G(z)))]$$

**VAE**: $\max \; \text{ELBO}$ — variational inference

$$\text{ELBO} = \mathbb{E}_{q(z|x)}[\log p(x|z)] - D_{KL}(q(z|x) \| p(z))$$

**Flow**: $\max \log p(x)$ — change of variables (exact!)

$$\log p(x) = \log p(f^{-1}(x)) + \log \left| \det J_{f^{-1}} \right|$$

**Diffusion**: $\max \; \text{ELBO}$ — denoising score matching

$$\text{ELBO} \approx \mathbb{E}_t \left[ \| \varepsilon - \varepsilon_\theta(x_t, t) \|^2 \right]$$

> 🧩 **Plain English Translation:**
> - **GAN**: "Let two networks fight it out. The Generator tries to fool; the Discriminator tries to detect." 🥊
> - **VAE**: "Compress data into a small code, reconstruct it, and keep the code space well-organized." 📦
> - **Flow**: "Transform a simple shape into a complex one through reversible stretches. Track exactly how the volume changes." 🌊
> - **Diffusion**: "Add noise gradually. Train a network to predict what noise was added at each step." 🧹

---

# 8️⃣ Timeline of Generative AI Breakthroughs

> 🕰️ **A decade of revolution.** Generative AI went from "blurry MNIST digits" to "photorealistic images from text prompts" in just 10 years. Here's every landmark moment:

| Year | Model | Innovation | 💡 Why It Mattered |
|------|-------|-----------|-------------------|
| 2013 | VAE | Variational inference for generation | First principled probabilistic generative model with neural nets |
| 2014 | GAN | Adversarial training | Introduced the "two-player game" paradigm — sharp images! |
| 2015 | DCGAN | Convolutional GAN | Proved GANs work with CNNs — first really convincing images |
| 2017 | WGAN | Wasserstein distance for stable training | Fixed GAN training with better math — Wasserstein distance |
| 2018 | BigGAN | Large-scale GAN | Showed scaling up GANs = dramatically better quality |
| 2019 | StyleGAN | Style-based generation | Photorealistic faces — "this person does not exist" 🤯 |
| 2020 | DDPM | Revived diffusion models | Ho et al. showed diffusion can match GAN quality |
| 2021 | DALL-E | Text-to-image (discrete VAE + transformer) | First viral text-to-image model — "avocado armchair" moment 🥑 |
| 2022 | Stable Diffusion | Open-source latent diffusion | Brought image generation to everyone — ran on consumer GPUs! |
| 2022 | DALL-E 2 | CLIP + diffusion | CLIP text understanding + diffusion image generation |
| 2023 | SDXL | Better Stable Diffusion | Higher resolution, better text rendering, improved aesthetics |
| 2023 | Consistency Models | Single-step diffusion | Generate in 1-2 steps instead of 50+ — massive speedup ⚡ |
| 2024 | Flux, SD3 | Flow matching + MMDiT | New paradigm: flow matching replaces traditional diffusion |

> 🔬 **Research Trend to Watch**: The field is moving from iterative denoising (slow) toward **flow matching** and **consistency distillation** (fast). The goal: GAN-speed generation with diffusion-quality results. We're almost there! 🚀

---

# 📝 Summary

| Model | Key Math | Best For | Analogy |
|-------|----------|----------|---------|
| GAN | Adversarial game | Fast, sharp samples | Art forger vs detective 🎭🔍 |
| VAE | ELBO maximization | Latent space, representation | ZIP file for images 📦 |
| Flow | Change of variables | Exact likelihood | Shape-shifting Play-Doh 🌊 |
| Diffusion | Denoise score matching | Best quality + diversity | Noise whisperer 🧹✨ |

---

# 🧾 Quick-Reference Cheat Sheet

## ⭐ Core Equations at a Glance

| Model | Objective | What It Optimizes |
|-------|-----------|-------------------|
| **GAN** | $\min_G \max_D \; \mathbb{E}[\log D(x)] + \mathbb{E}[\log(1-D(G(z)))]$ | Generator fools Discriminator |
| **VAE** | $\max \; \mathbb{E}_{q}[\log p(x \mid z)] - D_{KL}(q(z \mid x) \| p(z))$ | Reconstruction + regularized latent space |
| **Flow** | $\max \; \log p(z_0) - \sum_k \log \lvert \det J_{f_k} \rvert$ | Exact log-likelihood via invertible transforms |
| **Diffusion** | $\min \; \mathbb{E}_t[\|\varepsilon - \varepsilon_\theta(x_t, t)\|^2]$ | Predict & remove noise at each step |

## ⭐ Key Concepts Glossary

| Term | Plain English | 🎯 |
|------|--------------|-----|
| **Latent Space** | A compressed representation of data — like a ZIP file for images. Similar things are near each other. | Think: GPS coordinates for the "world of all possible images" 🗺️ |
| **Mode Collapse** | GAN gets stuck generating only one type of output | The forger only paints sunflowers 🌻 |
| **KL Divergence** | Measures how different two probability distributions are | How "surprised" you'd be using one distribution when the truth is another 😲 |
| **ELBO** | Evidence Lower BOund — a tractable approximation to the true likelihood | A "floor" estimate — real value is at least this high |
| **Reparameterization Trick** | $z = \mu + \sigma \odot \varepsilon$ — makes random sampling differentiable | Move the randomness outside the model so gradients can flow 🔀 |
| **Jacobian Determinant** | How much a transformation stretches/compresses space | Rubber sheet stretching factor 🧘 |
| **Nash Equilibrium** | Neither GAN player can improve without the other getting worse | A perfectly balanced seesaw ⚖️ |
| **Jensen-Shannon Divergence** | Symmetric measure of distribution difference (what GANs minimize) | A "fair" version of KL divergence |

## ⭐ Decision Guide: Which Model to Use?

```
Need exact likelihoods? ──────────────────→ Normalizing Flow
         │ No
Need structured latent space? ────────────→ VAE
         │ No
Need best possible quality + diversity? ──→ Diffusion Model
         │ No, need speed
Need fast single-pass generation? ────────→ GAN (or Consistency Model)
```

## 📚 Essential Papers (Reading List)

| # | Paper | Authors | Year | Key Contribution |
|---|-------|---------|------|-----------------|
| 1 | *Generative Adversarial Nets* | Goodfellow et al. | 2014 | Invented GANs — adversarial training framework |
| 2 | *Auto-Encoding Variational Bayes* | Kingma & Welling | 2014 | Invented VAEs — reparameterization trick + ELBO |
| 3 | *Variational Inference with Normalizing Flows* | Rezende & Mohamed | 2015 | Normalizing flows for flexible posteriors |
| 4 | *Analyzing and Improving the Image Quality of StyleGAN* | Karras et al. | 2020 | StyleGAN2 — photorealistic face generation |
| 5 | *Denoising Diffusion Probabilistic Models* | Ho et al. | 2020 | Revived diffusion — matched GAN quality |
| 6 | *High-Resolution Image Synthesis with Latent Diffusion Models* | Rombach et al. | 2022 | Latent diffusion — basis for Stable Diffusion |
| 7 | *Consistency Models* | Song et al. | 2023 | Single-step generation from diffusion |

---

**Tomorrow**: The Forward Diffusion Process — the mathematics of progressively adding noise.
