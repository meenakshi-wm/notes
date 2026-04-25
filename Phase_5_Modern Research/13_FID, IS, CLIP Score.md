📘 DAY 13 — Evaluating Diffusion Models: FID, IS, CLIP Score

Goal:

Master the evaluation metrics for generative models — understand what FID, Inception Score, and CLIP Score measure, how to compute them, and their limitations.

---

## 🌟 Beginner Welcome — What Is This Page About?

> **Imagine you hired three different artists to paint portraits. How do you decide which artist is "best"?** 🎨
>
> - Artist A paints incredibly realistic faces, but every face looks the same
> - Artist B paints all kinds of diverse faces, but they look cartoonish
> - Artist C paints faces that match your description perfectly, but they're blurry
>
> **This is the exact problem in AI image generation!** We need ways to *measure* if an AI model creates images that are realistic, diverse, AND match what you asked for.
>
> FID, IS, and CLIP Score are the three most important "report cards" 📊 for AI image generators like DALL-E, Midjourney, and Stable Diffusion.

### 🗺️ Page Roadmap

| Section | What You'll Learn | Difficulty |
|---------|-------------------|------------|
| 1️⃣ Why Evaluation is Hard | Why we can't just use "accuracy" | ⭐ Easy |
| 2️⃣ FID | The #1 metric — comparing photo albums | ⭐⭐ Medium |
| 3️⃣ IS | Quality + variety checker | ⭐⭐ Medium |
| 4️⃣ CLIP Score | Does the image match the text? | ⭐⭐ Medium |
| 5️⃣ Additional Metrics | The full toolkit | ⭐⭐⭐ Advanced |
| 6️⃣ Best Practices | How the pros do it | ⭐⭐ Medium |

---

# 1️⃣ Why Evaluation is Hard

> 🧠 **Beginner Analogy**: In a spelling test, there's one right answer — "cat" is correct, "cta" is wrong. But if you ask an AI to "draw a sunset over mountains," there are *millions* of correct answers! Every sunset is different. So how do you grade the AI? That's why evaluating generative models is fundamentally harder than grading a classifier.

Unlike discriminative models (accuracy on test set), generative models have no single ground truth.

> ⭐ **Key Insight**: A discriminative model says "this is a cat" (one right answer). A generative model *creates* a cat image (infinite right answers). You can't just check if the output matches a single correct answer!

We need to measure:
- **Quality**: Are samples realistic? 🖼️ *(Does it look like a real photo?)*
- **Diversity**: Does the model cover all modes? 🌈 *(Can it make dogs, cats, cars — not just dogs?)*
- **Text alignment**: Do images match the prompt? 📝 *(You asked for "red car" — is it red? Is it a car?)*
- **Novelty**: Is the model creating new images, not memorizing? 🆕 *(Is it creative, or just copying training data?)*

### 🍕 The Pizza Restaurant Analogy

| What We Measure | Pizza Restaurant Version | AI Image Generation Version |
|----------------|--------------------------|-----------------------------|
| **Quality** | Does each pizza taste good? | Does each image look realistic? |
| **Diversity** | Can they make many types of pizza? | Can it generate many types of images? |
| **Text Alignment** | Did I get what I ordered? | Does the image match the text prompt? |
| **Novelty** | Are they creative with toppings? | Is it creating new images, not copying? |

---

# 2️⃣ Frechet Inception Distance (FID)

> 🧠 **Beginner Analogy — The Wine Expert** 🍷
>
> Imagine a master wine expert who has tasted thousands of wines from **Vineyard A** (real images). They've built a mental model of what Vineyard A wines taste like: the *average* flavor profile, and the *range* of flavors.
>
> Now someone hands them wines from **Vineyard B** (AI-generated images). The expert compares their mental model of Vineyard B against Vineyard A. If both vineyards produce wines with similar average taste and similar variety → **low FID** (good!). If Vineyard B tastes completely different → **high FID** (bad!).
>
> **FID is literally this: a mathematical way to compare two "collections" of things.**

> 🎒 **Even Simpler**: FID = comparing two photo albums statistically. If your AI's album looks like it could belong to the same collection as the real photos, FID is low (good). If it looks obviously different, FID is high (bad).

The most widely used metric. Compares the distribution of generated images to real images.

> 📜 **Research Origin**: FID was introduced by **Heusel et al. (2017)** in *"GANs Trained by a Two Time-Scale Update Rule Converge to a Local Nash Equilibrium"* (NeurIPS 2017). It quickly became THE standard metric for generative model evaluation.

## How FID Works

> ⭐ **Step-by-step for beginners**:
>
> 1. **Feed real photos into a pre-trained neural network** (Inception-v3) — this network converts each image into a list of 2,048 numbers (a "feature vector" — think of it as a numerical fingerprint of the image)
> 2. **Feed AI-generated images into the same network** — get their fingerprints too
> 3. **Summarize each collection** by computing the average fingerprint (mean $\mu$) and how spread out the fingerprints are (covariance matrix $\Sigma$) — this fits a "bell curve" (Gaussian) to each set
> 4. **Measure how different the two bell curves are** — that difference is the FID score!

1. Extract features from real images using Inception-v3 (pool3 layer, 2048-dim)
2. Extract features from generated images using same network
3. Fit a Gaussian to each set: $\mathcal{N}(\mu_r, \Sigma_r)$ and $\mathcal{N}(\mu_g, \Sigma_g)$
4. Compute Fréchet distance between the Gaussians:

$$\text{FID} = \|\mu_r - \mu_g\|^2 + \text{Tr}\Big(\Sigma_r + \Sigma_g - 2\big(\Sigma_r \Sigma_g\big)^{1/2}\Big)$$

Where:
- $\mu_r, \mu_g$ = mean feature vectors of real and generated images
- $\Sigma_r, \Sigma_g$ = covariance matrices of real and generated features
- $\|\cdot\|^2$ = squared Euclidean distance (how far apart the averages are)
- $\text{Tr}(\cdot)$ = trace of a matrix (sum of diagonal elements — captures spread differences)

> 🔍 **Breaking down the formula for beginners**:
> - **First term** $\|\mu_r - \mu_g\|^2$: Are the *average* images similar? (Like comparing average flavor of two vineyards)
> - **Second term** $\text{Tr}(\ldots)$: Is the *variety* similar? (Like comparing the range of flavors)
> - Together they give a single number: **0 = identical distributions, higher = more different**

**Lower FID = Better** (0 = identical distributions) ⭐

### 🏭 Production Use — Where FID Shows Up in the Real World

| Company/Product | How They Use FID |
|-----------------|------------------|
| **Stability AI** (Stable Diffusion) | Every new model version reports FID on COCO/ImageNet to prove improvement |
| **OpenAI** (DALL-E) | FID benchmarks in research papers to compare against competitors |
| **Midjourney** | Internal quality tracking across model versions |
| **Google** (Imagen) | Reports FID on MS-COCO 256×256 as primary quality metric |
| **Academic Papers** | Nearly every diffusion/GAN paper since 2017 reports FID |

```python
from scipy.linalg import sqrtm
import numpy as np

def calculate_fid(real_features, gen_features):
    """Compute FID between two sets of Inception features."""
    mu_r = np.mean(real_features, axis=0)
    mu_g = np.mean(gen_features, axis=0)
    sigma_r = np.cov(real_features, rowvar=False)
    sigma_g = np.cov(gen_features, rowvar=False)
    
    diff = mu_r - mu_g
    covmean = sqrtm(sigma_r @ sigma_g)
    if np.iscomplexobj(covmean):
        covmean = covmean.real
    
    fid = diff @ diff + np.trace(sigma_r + sigma_g - 2 * covmean)
    return fid

def extract_inception_features(images, batch_size=50):
    """Extract features using pretrained Inception-v3."""
    from torchvision.models import inception_v3
    model = inception_v3(pretrained=True, transform_input=False)
    model.fc = nn.Identity()  # Remove classification head
    model.eval().cuda()
    
    features = []
    for i in range(0, len(images), batch_size):
        batch = images[i:i+batch_size].cuda()
        # Resize to 299x299 for Inception
        batch = F.interpolate(batch, size=299, mode='bilinear')
        with torch.no_grad():
            feat = model(batch)
        features.append(feat.cpu().numpy())
    return np.concatenate(features)
```

## FID Guidelines

| FID Score | Interpretation |
|-----------|---------------|
| < 5 | Excellent (near-photorealistic) |
| 5-20 | Very good |
| 20-50 | Decent |
| 50-100 | Noticeable artifacts |
| > 100 | Poor quality |

State-of-the-art (2024): FID < 2 on ImageNet 256×256.

### ⚠️ FID Pitfalls & Limitations

| Pitfall | Why It Matters | What To Do |
|---------|---------------|------------|
| **Sample size sensitivity** | FID changes a LOT with fewer than 10K samples | Always use 10K–50K generated samples |
| **Inception-v3 bias** | The feature extractor was trained on ImageNet — it might not understand medical images, art, etc. | Consider domain-specific feature extractors |
| **Single number hides details** | FID = 30 could mean "slightly blurry but diverse" OR "sharp but mode collapsed" | Always pair with Precision/Recall |
| **Not a true distance** | Different random seeds give slightly different FID values | Report mean ± std over multiple runs |

> 📜 **Research Note**: Parmar et al. (2022) introduced **Clean-FID** to address inconsistencies in how different papers compute FID (different resizing methods, JPEG compression, etc.). The `clean-fid` Python library standardizes FID computation. Jayasumana et al. (2023) proposed **CMMD** (CLIP Maximum Mean Discrepancy) as a more robust alternative.

---

# 3️⃣ Inception Score (IS)

> 🧠 **Beginner Analogy — The Art Classroom** 🎨
>
> Imagine grading a classroom of student artists. You want to check TWO things:
> 1. **Quality**: When you look at each individual drawing, can you clearly tell what it is? (A dog should look obviously like a dog, not a blurry blob)
> 2. **Variety**: Across ALL drawings in the class, are students drawing many different things? (All dogs = boring. Dogs, cats, cars, trees = diverse!)
>
> Inception Score checks BOTH at once:
> - Each image should make the classifier **very confident** ("I'm 99% sure this is a dog!") → ⭐ Quality
> - Across all images, the classifier should see **many different categories** equally → 🌈 Diversity

> 📜 **Research Origin**: Inception Score was introduced by **Salimans et al. (2016)** in *"Improved Techniques for Training GANs"* (NeurIPS 2016). It was one of the first quantitative metrics for GAN evaluation.

Measures quality AND diversity using the Inception classifier.

$$\text{IS} = \exp\Big(\mathbb{E}_{\mathbf{x}}\big[D_{KL}\big(p(y|\mathbf{x}) \;\|\; p(y)\big)\big]\Big)$$

Where:
- $p(y|\mathbf{x})$ = the Inception classifier's prediction for a single generated image $\mathbf{x}$ ("what class is this image?")
- $p(y) = \mathbb{E}_{\mathbf{x}}[p(y|\mathbf{x})]$ = the average prediction across ALL generated images ("what classes are represented overall?")
- $D_{KL}$ = KL divergence — measures how different two probability distributions are
- $\exp$ = exponential function — converts the KL divergence into a more interpretable score

> 🔍 **Breaking down for beginners**:
> - If **each image is clear** → $p(y|\mathbf{x})$ is "peaked" (one class gets ~100% probability) → high KL → high IS ✅
> - If **images are diverse** → $p(y)$ is "flat/uniform" (all classes equally represented) → high KL → high IS ✅
> - If images are blurry → $p(y|\mathbf{x})$ is spread across many classes → low KL → low IS ❌
> - If all images look the same → $p(y)$ is peaked at one class → low KL → low IS ❌

- $p(y|\mathbf{x})$ should be peaked (model generates recognizable objects) → quality
- $p(y)$ should be uniform (model generates diverse objects) → diversity

**Higher IS = Better** (max ≈ 1000 for 1000 ImageNet classes) ⭐

### 📊 IS Score Interpretation

| IS Score | Interpretation |
|----------|----------------|
| ~1 | Terrible — all images look random/same |
| ~10 | Decent quality but limited diversity |
| ~50–100 | Good quality and diversity |
| ~200+ | Excellent (state of the art for GANs was ~250+) |
| ~1000 | Theoretical maximum (perfect on all 1000 ImageNet classes) |

```python
def inception_score(images, model, splits=10):
    preds = []
    for batch in DataLoader(images, batch_size=50):
        with torch.no_grad():
            pred = F.softmax(model(batch.cuda()), dim=1)
        preds.append(pred.cpu().numpy())
    preds = np.concatenate(preds)
    
    scores = []
    for k in range(splits):
        part = preds[k * len(preds)//splits : (k+1) * len(preds)//splits]
        py = np.mean(part, axis=0, keepdims=True)
        kl = part * (np.log(part + 1e-10) - np.log(py + 1e-10))
        scores.append(np.exp(np.mean(np.sum(kl, axis=1))))
    
    return np.mean(scores), np.std(scores)
```

## IS Limitations

> ⚠️ **Why IS fell out of favor** — and why FID replaced it as the primary metric:

- Only measures ImageNet classes *(what about medical images, art, faces?)*
- Doesn't compare to real data distribution *(FID compares to real images; IS only looks at generated images in isolation!)*
- Can be gamed (generate one perfect image per class) *(a model that memorizes exactly 1000 images gets a perfect score)*
- Sensitive to the Inception model's biases and quirks
- Not meaningful for non-ImageNet domains

> ⭐ **Bottom line**: IS is historically important (you'll see it in older papers) but **FID is now the standard**. Think of IS as the "first draft" of generative evaluation metrics.

---

# 4️⃣ CLIP Score (Text-Image Alignment)

> 🧠 **Beginner Analogy — The Bilingual Translator** 🌐
>
> Imagine you have a friend who speaks BOTH "image language" and "text language" perfectly. You show them a photo 🖼️ and give them a text description 📝, and they tell you: "These are 95% the same thing!" or "These are only 20% related..."
>
> That's exactly what CLIP does! CLIP (Contrastive Language-Image Pretraining, by OpenAI) learned to understand BOTH images and text in a shared "language" (embedding space). CLIP Score = how similar the image and text are in that shared space.
>
> 🎯 **Real-world example**: You type "a golden retriever wearing sunglasses on a beach." The AI generates an image. CLIP Score tells you how well that image actually matches your description.

> 📜 **Research Origin**: CLIPScore was formalized by **Hessel et al. (2022)** in *"CLIPScore: A Reference-free Evaluation Metric for Image Captioning"* (EMNLP 2022). It builds on the CLIP model from **Radford et al. (2021)**.

For text-conditional models, measure how well images match prompts:

$$\text{CLIP Score} = \cos\Big(\text{CLIP}_{\text{image}}(\mathbf{x}),\; \text{CLIP}_{\text{text}}(t)\Big) = \frac{\text{CLIP}_{\text{image}}(\mathbf{x}) \cdot \text{CLIP}_{\text{text}}(t)}{\|\text{CLIP}_{\text{image}}(\mathbf{x})\| \;\|\text{CLIP}_{\text{text}}(t)\|}$$

Where:
- $\text{CLIP}_{\text{image}}(\mathbf{x})$ = CLIP's embedding (numerical representation) of the generated image
- $\text{CLIP}_{\text{text}}(t)$ = CLIP's embedding of the text prompt
- $\cos(\cdot, \cdot)$ = cosine similarity — measures the angle between two vectors (1 = identical direction, 0 = perpendicular/unrelated)

> 🔍 **Cosine similarity for beginners**: Think of two arrows. If they point in the **same direction** → similarity = 1 (perfect match). If they point in **opposite directions** → similarity = -1 (complete mismatch). If they're at **90°** → similarity = 0 (unrelated).

```python
import clip

def clip_score(images, texts, model, preprocess):
    """Compute average CLIP similarity between images and texts."""
    image_features = model.encode_image(preprocess(images))
    text_features = model.encode_text(clip.tokenize(texts))
    
    image_features = image_features / image_features.norm(dim=-1, keepdim=True)
    text_features = text_features / text_features.norm(dim=-1, keepdim=True)
    
    similarity = (image_features * text_features).sum(dim=-1)
    return similarity.mean().item()
```

Higher CLIP Score ≈ better text adherence. ⭐

### 📊 CLIP Score Interpretation

| CLIP Score Range | Interpretation |
|-----------------|----------------|
| > 0.35 | Excellent text-image alignment |
| 0.28–0.35 | Good alignment |
| 0.20–0.28 | Moderate alignment |
| < 0.20 | Poor alignment — image doesn't match text |

> 💡 **Note**: Exact values depend on the CLIP model version (ViT-B/32 vs ViT-L/14) and normalization.

**Trade-off**: Higher CFG scale → higher CLIP score but lower diversity/FID.

> 🧠 **Beginner Analogy — The Obedient but Boring Artist** 🎨
>
> Imagine telling an artist "paint a red car." With low guidance (low CFG), they might paint a beautiful blue truck — creative but not what you asked for. With high guidance (high CFG), they paint a red car every single time — obedient but repetitive.
>
> This is the **quality-diversity-alignment tradeoff** — one of the most important concepts in generative AI:
> - **Crank up text guidance** → CLIP Score goes UP ↑ (matches text better) but FID goes DOWN ↓ (less diverse) 
> - **Lower text guidance** → more creative and diverse images, but they might not match your prompt

### 🏭 Production Use — CLIP Score in the Real World

| Use Case | How CLIP Score Helps |
|----------|---------------------|
| **Midjourney** | Filters out images that don't match user prompts before showing results |
| **DALL-E** | Internal quality gate — images below threshold are regenerated |
| **Content moderation** | Check if generated images match the *intended* description (flag mismatches) |
| **A/B testing models** | Compare which model version better follows user instructions |
| **Prompt engineering** | Measure which prompt phrasing produces the most aligned results |

---

# 5️⃣ Additional Metrics

> 🧠 **Think of these as different lenses** 🔍 — each metric reveals a different aspect of your generative model. No single metric tells the whole story!

| Metric | Measures | Higher/Lower Better | Beginner Analogy |
|--------|----------|-------------------|------------------|
| FID | Quality + Diversity | Lower ⬇️ | Comparing two photo albums — how similar are they overall? |
| IS | Quality + Diversity | Higher ⬆️ | Can you recognize what's in each photo, AND are the photos diverse? |
| CLIP Score | Text alignment | Higher ⬆️ | Does the painting match what the client asked for? |
| LPIPS | Perceptual diversity | Higher ⬆️ | How perceptually different are pairs of generated images? |
| Precision | Quality (% real-looking) | Higher ⬆️ | What % of generated images could fool a human? |
| Recall | Diversity (% modes covered) | Higher ⬆️ | What % of real-world variety does the model capture? |
| KID | Like FID but unbiased | Lower ⬇️ | Like FID but works better with fewer samples |

## Precision & Recall for Generative Models

> 🧠 **Beginner Analogy — The Party Invitation** 🎉
>
> **Precision**: You invited 100 AI-generated "people" to a party. How many of them actually look like real humans? If 90 look real → 90% precision.
>
> **Recall**: There are 100 types of real people in the world (different ages, ethnicities, styles). How many types did your AI generate? If it only generates young white males → low recall (even if they look perfect).

Precision = fraction of generated samples that fall in the real data manifold
Recall = fraction of real data that is covered by generated samples

```
High Precision + Low Recall = mode collapse (quality without diversity)  🎯❌🌈
Low Precision + High Recall = mode coverage but poor quality              ❌🎯🌈
High Precision + High Recall = the goal                                    🎯✅🌈
```

> ⭐ **Why this matters**: FID combines quality and diversity into ONE number, which hides failure modes. Precision/Recall SEPARATES them so you can diagnose what's wrong.

---

# 6️⃣ Evaluation Best Practices

> 🧠 **Why best practices matter**: Subtle differences in how you compute FID can change the score by 10-20 points! This has caused confusion in published papers where teams unknowingly used different computation methods and claimed improvements that weren't real.

1. **Sample size**: At least 10K-50K samples for reliable FID
   > 💡 *Using only 1K samples? Your FID could be off by ±20 points. With 50K, the variance drops dramatically.*
2. **Reference set**: Use the same preprocessing as training
   > 💡 *JPEG compression alone can change FID by several points! The `clean-fid` library handles this correctly.*
3. **Resolution**: Evaluate at generation resolution
   > 💡 *Upscaling 64×64 images to 256×256 before computing FID will give misleadingly better scores.*
4. **Random seeds**: Report mean ± std over multiple seeds (3-5 runs)
   > 💡 *A single FID run might say 15.2 — but the true range might be 14.0–16.5.*
5. **FID is not everything**: Always show visual samples alongside numbers
   > 💡 *A paper claiming FID=5 means nothing without showing actual generated images.*
6. **Human evaluation**: Still the gold standard — run A/B tests or preference ranking
   > 💡 *After all, humans are the real judges! Companies like Midjourney do extensive human preference testing.*

### 🏭 How Top AI Companies Evaluate Their Models

| Company | Primary Metric | Secondary Metrics | Human Eval? |
|---------|---------------|-------------------|------------|
| **OpenAI** (DALL-E 3) | FID on COCO | CLIP Score, human preference ratings | Yes — ELO-style comparisons |
| **Google** (Imagen) | FID on COCO-256 | CLIP Score, DrawBench evaluation | Yes — side-by-side human study |
| **Stability AI** | FID on COCO | CLIP Score, human preference | Yes — Discord community feedback |
| **Midjourney** | Human preference (primary!) | FID, CLIP Score internally | Yes — this IS their primary metric |

> ⭐ **Key insight**: The best companies use automatic metrics (FID, CLIP) for fast iteration and human evaluation for final decisions. **Automatic metrics are necessary but not sufficient.**

---

# 📝 Summary

| Metric | What it captures | When to use | Beginner Analogy |
|--------|-----------------|-------------|------------------|
| FID | Overall quality + diversity | Primary metric for unconditional models | Comparing two photo albums — lower = more similar = better 📷 |
| CLIP Score | Text-image alignment | Primary for text-to-image | How well does the painting match the description? 🎨📝 |
| IS | Quality + diversity (ImageNet) | Secondary, historically important | Can you tell what's in each image + are they varied? 🔍🌈 |
| Precision/Recall | Disentangled quality vs diversity | Understanding failure modes | What % look real? What % of variety is covered? 🎯🌈 |

---

# 🧾 Cheat Sheet — Quick Reference Card

```
┌─────────────────────────────────────────────────────────────────┐
│                   GENERATIVE MODEL METRICS                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  FID (Fréchet Inception Distance)                              │
│  ├─ Formula: ||μ_r - μ_g||² + Tr(Σ_r + Σ_g - 2(Σ_rΣ_g)^½)  │
│  ├─ Direction: LOWER is better (0 = perfect)                   │
│  ├─ Use: Primary metric for all generative models              │
│  └─ Needs: 10K-50K samples minimum                             │
│                                                                 │
│  IS (Inception Score)                                          │
│  ├─ Formula: exp(E[KL(p(y|x) || p(y))])                       │
│  ├─ Direction: HIGHER is better (max ~1000)                    │
│  ├─ Use: Legacy metric, ImageNet only                          │
│  └─ Limitation: Doesn't compare to real data!                  │
│                                                                 │
│  CLIP Score                                                    │
│  ├─ Formula: cos(CLIP_image(x), CLIP_text(t))                 │
│  ├─ Direction: HIGHER is better (max = 1.0)                   │
│  ├─ Use: Text-to-image alignment                               │
│  └─ Note: Trade-off with diversity (high CFG → high CLIP)      │
│                                                                 │
│  QUICK DECISION GUIDE:                                         │
│  ├─ Unconditional generation? → Report FID                     │
│  ├─ Text-to-image? → Report FID + CLIP Score                  │
│  ├─ Debugging quality vs diversity? → Use Precision/Recall     │
│  └─ Final decision? → Always include human evaluation          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

# 📚 Resources

## 📄 Key Papers (Must-Read)

| Paper | Authors | Year | Why Read It |
|-------|---------|------|------------|
| *GANs Trained by a Two Time-Scale Update Rule Converge to a Local Nash Equilibrium* | Heusel et al. | 2017 | **Introduced FID** — the paper that defined the standard metric |
| *Improved Techniques for Training GANs* | Salimans et al. | 2016 | **Introduced IS** — the first quantitative GAN metric |
| *CLIPScore: A Reference-free Evaluation Metric for Image Captioning* | Hessel et al. | 2022 | **Formalized CLIP Score** — the standard for text-image alignment |
| *Learning Transferable Visual Models From Natural Language Supervision* | Radford et al. | 2021 | **Introduced CLIP** — the foundation model behind CLIP Score |
| *Assessing Generative Models via Precision and Recall* | Sajjadi et al. | 2018 | **Precision/Recall for generators** — separating quality from diversity |
| *On Aliased Resizing and Surprising Subtleties in GAN Evaluation* | Parmar et al. | 2022 | **Clean-FID** — why FID computation details matter enormously |
| *Rethinking FID: Towards a Better Evaluation Metric for Image Generation* | Jayasumana et al. | 2023 | **CMMD** — a proposed improvement over FID using CLIP features |

## 📖 Books

| Book | Author(s) | What It Covers |
|------|-----------|----------------|
| *Deep Learning* (Chapter 20) | Goodfellow, Bengio, Courville | Generative models foundations — the classic textbook |
| *Probabilistic Machine Learning: Advanced Topics* | Kevin Murphy | Modern coverage of generative model evaluation |
| *Understanding Deep Learning* | Simon Prince | Accessible introduction to generative models and their evaluation |

## 🛠️ Libraries & Tools

| Library | What It Does | Install |
|---------|-------------|----------|
| **`clean-fid`** | Standardized FID computation (avoids subtle bugs) | `pip install clean-fid` |
| **`torch-fidelity`** | FID, IS, KID computation with GPU acceleration | `pip install torch-fidelity` |
| **`torchmetrics`** | FID, IS, CLIP Score as PyTorch metrics | `pip install torchmetrics` |
| **`openai/clip`** | Official CLIP model for computing CLIP Score | `pip install git+https://github.com/openai/CLIP.git` |

### Example: Computing FID with `clean-fid` (Recommended)

```python
from cleanfid import fid

# Compare a folder of generated images to a folder of real images
score = fid.compute_fid(
    fdir1="path/to/real/images",
    fdir2="path/to/generated/images",
    mode="clean"  # Uses standardized resizing — avoids subtle bugs!
)
print(f"FID: {score:.2f}")
```

### Example: Computing IS with `torch-fidelity`

```python
import torch_fidelity

metrics = torch_fidelity.calculate_metrics(
    input1="path/to/generated/images",
    cuda=True,
    isc=True,   # Compute Inception Score
    fid=True,   # Also compute FID
    kid=True,   # Also compute KID
    input2="path/to/real/images",  # Needed for FID/KID
)
print(f"IS: {metrics['inception_score_mean']:.2f} ± {metrics['inception_score_std']:.2f}")
print(f"FID: {metrics['frechet_inception_distance']:.2f}")
```

## 🔗 Online Resources

- [Papers With Code — FID Leaderboard](https://paperswithcode.com/sota/image-generation-on-imagenet-256x256) — see current state-of-the-art FID scores
- [Lil'Log — Evaluation Metrics for Generative Models](https://lilianweng.github.io/) — Lilian Weng's excellent blog
- [The FID Score Explained](https://machinelearningmastery.com/how-to-implement-the-frechet-inception-distance-fid-from-scratch/) — step-by-step tutorial

---

**Tomorrow**: Consistency Models & Flow Matching — the next generation.
