📘 DAY 40 — Paper Reproduction: Vision Transformer (ViT)

Goal:

Reproduce the Vision Transformer (Dosovitskiy et al., 2020) — the model that proved transformers work for images. Implement patch embedding, train on CIFAR-10, and verify classification accuracy.

---

# 🌟 Beginner's Guide: What is ViT?

> **No AI knowledge needed!** This section explains ViT using everyday analogies.

## 🧩 The Big Idea in Plain English

Imagine you have a **photo of a cat** 🐱 and you want a computer to recognize it.

**Old way (CNNs):** Like examining an image with a **magnifying glass sliding across** 🔍 — you look at small regions one at a time, gradually building up understanding from edges → textures → shapes → "it's a cat!"

**New way (ViT):** Like **cutting a photo into puzzle pieces** 🧩, then having a **team of experts examine ALL pieces together** at the same time to understand the whole image!

## 🎯 Core Analogies

| Concept | Analogy | What It Does |
|---------|---------|-------------|
| 🧩 **Patches** | Puzzle pieces | The image is cut into small square tiles (e.g., 16×16 pixels) |
| 👔 **[CLS] Token** | Team leader who summarizes findings | A special token that collects info from all patches to make the final decision |
| 🔢 **Position Embeddings** | Numbering puzzle pieces so you know where they go | Tells the model where each patch came from in the original image |
| 🔄 **Self-Attention** | All team members discussing with each other | Every patch looks at every other patch to understand context |
| 📐 **Patch Embedding** | Converting puzzle pieces into a language the team understands | Turns raw pixel squares into numerical vectors |

⭐ **Key Insight:** ViT showed that you DON'T need convolutions for vision — just split the image into patches and use a Transformer!

## 📐 Core Math (Don't Panic! 🧮)

**Step 1: Split image into patches**

For an image of height $H$ and width $W$ with patch size $P$:

$$N = \frac{H \times W}{P^2}$$

> 🧩 *Example:* A 224×224 image with 16×16 patches → $N = \frac{224 \times 224}{16^2} = \frac{50176}{256} = 196$ patches

**Step 2: Create patch embeddings**

Each patch is flattened and projected into dimension $D$:

$$z_0 = [x_{\text{class}};\; x_p^1 E;\; x_p^2 E;\; \ldots;\; x_p^N E] + E_{\text{pos}}$$

where:
- $E \in \mathbb{R}^{(P^2 \cdot C) \times D}$ is the patch projection matrix
- $P^2 \cdot C$ = pixels per patch × color channels (e.g., $16^2 \times 3 = 768$)
- $x_{\text{class}}$ = the special [CLS] token prepended to the sequence
- $E_{\text{pos}} \in \mathbb{R}^{(N+1) \times D}$ = learnable position embeddings

> 🎯 *Analogy:* This is like numbering your puzzle pieces (position) and translating them into a common language (projection) so the team can discuss them!

**Step 3: Self-Attention on patches**

Standard Transformer self-attention applied to patch tokens:

$$\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^T}{\sqrt{d_k}}\right) V$$

> 🔄 Every patch "looks at" every other patch — unlike CNNs which only see local neighborhoods!

**Step 4: Classification from [CLS] token**

$$y = \text{MLP}(\text{LayerNorm}(z_L^0))$$

where $z_L^0$ is the [CLS] token output from the final Transformer layer.

> 👔 The team leader ([CLS]) has listened to everyone's discussion and now gives the final verdict: "It's a cat!"

---

# 1️⃣ Paper Summary

| Aspect | Detail |
|--------|--------|
| Problem | Can Transformers replace CNNs for vision? |
| Key insight | Split image into patches, treat as sequence of tokens |
| Method | Standard Transformer encoder + classification head |
| Result | SOTA on ImageNet when pretrained on large datasets |

---

# 2️⃣ Architecture

```
Image (224×224×3)
    ↓ Split into 16×16 patches
14×14 = 196 patches, each 16×16×3 = 768 dims
    ↓ Linear projection to d_model
196 token embeddings (each d_model dims)
    ↓ Prepend [CLS] token + add position embeddings
197 tokens
    ↓ Transformer Encoder (12 layers)
197 output tokens
    ↓ Take [CLS] token output → MLP head
Class prediction
```

---

# 3️⃣ Implementation

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class PatchEmbedding(nn.Module):
    """Split image into patches and project to embeddings."""
    def __init__(self, img_size=224, patch_size=16, in_channels=3, embed_dim=768):
        super().__init__()
        self.num_patches = (img_size // patch_size) ** 2
        self.proj = nn.Conv2d(in_channels, embed_dim, kernel_size=patch_size, stride=patch_size)
    
    def forward(self, x):
        # x: (B, C, H, W) → (B, embed_dim, H/P, W/P) → (B, num_patches, embed_dim)
        x = self.proj(x)
        x = x.flatten(2).transpose(1, 2)
        return x

class TransformerEncoderBlock(nn.Module):
    def __init__(self, embed_dim=768, num_heads=12, mlp_ratio=4, dropout=0.1):
        super().__init__()
        self.norm1 = nn.LayerNorm(embed_dim)
        self.attn = nn.MultiheadAttention(embed_dim, num_heads, dropout=dropout, batch_first=True)
        self.norm2 = nn.LayerNorm(embed_dim)
        self.mlp = nn.Sequential(
            nn.Linear(embed_dim, embed_dim * mlp_ratio),
            nn.GELU(),
            nn.Dropout(dropout),
            nn.Linear(embed_dim * mlp_ratio, embed_dim),
            nn.Dropout(dropout),
        )
    
    def forward(self, x):
        h = self.norm1(x)
        x = x + self.attn(h, h, h)[0]
        x = x + self.mlp(self.norm2(x))
        return x

class VisionTransformer(nn.Module):
    def __init__(self, img_size=32, patch_size=4, in_channels=3, num_classes=10,
                 embed_dim=192, depth=12, num_heads=3, mlp_ratio=4, dropout=0.1):
        super().__init__()
        self.patch_embed = PatchEmbedding(img_size, patch_size, in_channels, embed_dim)
        num_patches = self.patch_embed.num_patches
        
        # CLS token and position embeddings
        self.cls_token = nn.Parameter(torch.randn(1, 1, embed_dim) * 0.02)
        self.pos_embed = nn.Parameter(torch.randn(1, num_patches + 1, embed_dim) * 0.02)
        self.pos_drop = nn.Dropout(dropout)
        
        # Transformer encoder
        self.blocks = nn.ModuleList([
            TransformerEncoderBlock(embed_dim, num_heads, mlp_ratio, dropout)
            for _ in range(depth)
        ])
        
        # Classification head
        self.norm = nn.LayerNorm(embed_dim)
        self.head = nn.Linear(embed_dim, num_classes)
        
        self._init_weights()
    
    def _init_weights(self):
        for m in self.modules():
            if isinstance(m, nn.Linear):
                nn.init.trunc_normal_(m.weight, std=0.02)
                if m.bias is not None:
                    nn.init.zeros_(m.bias)
            elif isinstance(m, nn.LayerNorm):
                nn.init.ones_(m.weight)
                nn.init.zeros_(m.bias)
    
    def forward(self, x):
        B = x.shape[0]
        
        # Patch embedding
        x = self.patch_embed(x)  # (B, num_patches, embed_dim)
        
        # Prepend CLS token
        cls_tokens = self.cls_token.expand(B, -1, -1)
        x = torch.cat([cls_tokens, x], dim=1)  # (B, num_patches+1, embed_dim)
        
        # Add position embeddings
        x = self.pos_drop(x + self.pos_embed)
        
        # Transformer blocks
        for block in self.blocks:
            x = block(x)
        
        # Classification: use CLS token
        x = self.norm(x[:, 0])
        return self.head(x)
```

---

# 4️⃣ Training on CIFAR-10

```python
from torchvision import datasets, transforms

def train_vit_cifar10(epochs=100, batch_size=128, lr=3e-4):
    device = torch.device('cuda')
    
    transform_train = transforms.Compose([
        transforms.RandomCrop(32, padding=4),
        transforms.RandomHorizontalFlip(),
        transforms.ToTensor(),
        transforms.Normalize([0.4914, 0.4822, 0.4465], [0.2470, 0.2435, 0.2616]),
    ])
    transform_test = transforms.Compose([
        transforms.ToTensor(),
        transforms.Normalize([0.4914, 0.4822, 0.4465], [0.2470, 0.2435, 0.2616]),
    ])
    
    train_set = datasets.CIFAR10('.', train=True, download=True, transform=transform_train)
    test_set = datasets.CIFAR10('.', train=False, transform=transform_test)
    train_loader = DataLoader(train_set, batch_size=batch_size, shuffle=True, num_workers=4)
    test_loader = DataLoader(test_set, batch_size=256)
    
    # Small ViT for CIFAR-10 (32×32 images, 4×4 patches)
    model = VisionTransformer(
        img_size=32, patch_size=4, num_classes=10,
        embed_dim=192, depth=12, num_heads=3
    ).to(device)
    
    print(f"Parameters: {sum(p.numel() for p in model.parameters()):,}")
    
    optimizer = torch.optim.AdamW(model.parameters(), lr=lr, weight_decay=0.05)
    scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=epochs)
    
    best_acc = 0
    for epoch in range(epochs):
        model.train()
        total_loss = 0
        for x, y in train_loader:
            x, y = x.to(device), y.to(device)
            logits = model(x)
            loss = F.cross_entropy(logits, y)
            
            optimizer.zero_grad()
            loss.backward()
            torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
            optimizer.step()
            total_loss += loss.item()
        
        scheduler.step()
        acc = evaluate(model, test_loader, device)
        best_acc = max(best_acc, acc)
        print(f"Epoch {epoch+1}: loss={total_loss/len(train_loader):.4f}, acc={acc:.2f}%, best={best_acc:.2f}%")

def evaluate(model, loader, device):
    model.eval()
    correct = total = 0
    with torch.no_grad():
        for x, y in loader:
            x, y = x.to(device), y.to(device)
            pred = model(x).argmax(dim=-1)
            correct += (pred == y).sum().item()
            total += y.size(0)
    return correct / total * 100
```

Expected: ~90-92% on CIFAR-10 with this small ViT (vs ~95% for CNN baselines).

> ⭐ **Why lower than CNNs on small data?** ViT lacks the "inductive bias" of CNNs (locality, translation invariance). It needs MORE data to learn what CNNs get for free from their architecture. With enough data (JFT-300M), ViT surpasses CNNs!

---

# 🏭 Production & Real-World Impact

| Application | How ViT Powers It | Scale |
|-------------|-------------------|-------|
| 🔍 **Google Image Search** | ViT encodes images into embeddings for similarity search | Billions of images |
| 🎨 **DALL·E** (OpenAI) | Uses ViT as image encoder in text-to-image generation | Millions of users |
| 📎 **CLIP** (OpenAI) | ViT as visual backbone for text-image alignment | Zero-shot classification |
| ✂️ **SAM** (Meta, 2023) | ViT-H as image encoder for Segment Anything Model | Segment any object in any image |
| 🏥 **Medical Imaging** | ViT for pathology slide analysis, X-ray classification | Hospital-scale deployment |

### ⭐ Production Considerations

- **Data requirement:** ViT needs large-scale pretraining (JFT-300M, ImageNet-21K) to shine. Without it, CNNs win on smaller datasets.
- **DeiT solution:** Touvron et al. (2021) showed data augmentation + distillation can train ViT competitively on ImageNet-1K alone — no JFT needed!
- **Scaling advantage:** ViT scales better than CNNs — doubling compute gives more improvement with ViT than with ResNets.
- **Inference cost:** 196 tokens per image means $O(N^2)$ attention — manageable, but watch out with smaller patches (more tokens).

---

# 📚 Research Citations

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|------------------|
| **An Image is Worth 16×16 Words** | Dosovitskiy et al. | 2021 (ICLR) | Original ViT — proved Transformers work for vision at scale |
| **DeiT: Training Data-Efficient Image Transformers** | Touvron et al. | 2021 (ICML) | Made ViT trainable on ImageNet-1K via distillation + augmentation |
| **Segment Anything (SAM)** | Kirillov et al. | 2023 (ICCV) | ViT-H as backbone for universal segmentation |
| **CLIP: Learning Transferable Visual Models** | Radford et al. | 2021 (ICML) | ViT for contrastive text-image pretraining |
| **BEiT: BERT Pre-Training of Image Transformers** | Bao et al. | 2022 (ICLR) | Self-supervised ViT pretraining via masked image modeling |

---

# 5️⃣ Key Paper Insights

| Finding | Detail |
|---------|--------|
| Data hungry | ViT needs JFT-300M or ImageNet-21K pretraining to beat CNNs |
| Patch size matters | Smaller patches = more tokens = better but slower |
| CLS vs average pooling | Both work, CLS is standard for ViT |
| Position embeddings | Learned works as well as 2D sinusoidal |
| Scale is key | ViT-L/16 > ViT-B/16 > ViT-S/16 |

---

# 6️⃣ ViT Variants

| Model | Params | Patch | Heads | Layers | Top-1 (ImageNet) |
|-------|--------|-------|-------|--------|-------------------|
| ViT-Ti | 5M | 16 | 3 | 12 | 72.2% |
| ViT-S | 22M | 16 | 6 | 12 | 79.9% |
| ViT-B | 86M | 16 | 12 | 12 | 84.0% |
| ViT-L | 307M | 16 | 16 | 24 | 87.1% |

---

# 🆚 ViT vs CNN Comparison

| Feature | CNN (e.g., ResNet) 🔍 | ViT 🧩 |
|---------|----------------------|--------|
| **How it sees** | Sliding magnifying glass (local → global) | All puzzle pieces discussed at once (global from start) |
| **Inductive bias** | Locality + translation invariance built-in | Minimal — learns everything from data |
| **Small data** | ✅ Wins (built-in priors help) | ❌ Needs more data or augmentation |
| **Large data** | Good but plateaus | ⭐ Scales better — keeps improving |
| **Compute scaling** | Diminishing returns | Better return per FLOP increase |
| **Architecture** | Convolution layers + pooling | Patch embed + Transformer encoder |
| **Interpretability** | Feature maps, Grad-CAM | Attention maps show which patches attend to which |

---

# 📋 ViT Cheat Sheet

| Symbol | Meaning |
|--------|---------|
| $H, W$ | Image height and width |
| $P$ | Patch size (e.g., 16) |
| $C$ | Number of color channels (3 for RGB) |
| $N = HW/P^2$ | Number of patches |
| $D$ | Embedding dimension |
| $E \in \mathbb{R}^{(P^2 C) \times D}$ | Patch projection matrix |
| $E_{\text{pos}}$ | Position embedding matrix |
| $x_{\text{class}}$ | Learnable [CLS] token |
| $z_0$ | Input sequence (CLS + patch embeddings + positions) |
| $z_L^0$ | [CLS] token output at final layer $L$ |

⭐ **One-Line Summary:** ViT = split image into patches → project to embeddings → add [CLS] + positions → Transformer encoder → classify from [CLS].

---

# 📖 Resources

## Papers
- 📄 [An Image is Worth 16×16 Words (ViT)](https://arxiv.org/abs/2010.11929) — Dosovitskiy et al., 2021
- 📄 [DeiT: Training Data-Efficient Image Transformers](https://arxiv.org/abs/2012.12877) — Touvron et al., 2021
- 📄 [Segment Anything (SAM)](https://arxiv.org/abs/2304.02643) — Kirillov et al., 2023
- 📄 [CLIP: Learning Transferable Visual Models](https://arxiv.org/abs/2103.00020) — Radford et al., 2021

## Books
- 📘 *Dive into Deep Learning* — d2l.ai, Chapter on Vision Transformers
- 📘 *Deep Learning for Vision Systems* — Mohamed Elgendy (Manning)
- 📘 *Transformers for Natural Language Processing and Computer Vision* — Denis Rothman

## Tutorials & Blogs
- 🎓 [HuggingFace ViT Tutorial](https://huggingface.co/docs/transformers/model_doc/vit) — Fine-tune ViT with 🤗 Transformers
- 📝 [Google AI Blog: An Image is Worth 16×16 Words](https://ai.googleblog.com/2020/12/transformers-for-image-recognition-at.html)
- 🎥 [Yannic Kilcher — ViT Paper Explained](https://www.youtube.com/watch?v=TrdevFK_am4)
- 💻 [timm library (PyTorch Image Models)](https://github.com/huggingface/pytorch-image-models) — Pretrained ViT weights

---

# 📝 Summary: Paper Reproduction Sprint (Days 91-105)

| Day | Paper | Core Insight |
|-----|-------|-------------|
| 91 | How to Read Papers | Three-pass method |
| 92 | Attention Is All You Need | Transformer architecture |
| 93 | Scaling Laws | Power law relationships |
| 94 | DDPM | Denoising diffusion |
| 95 | LoRA | Low-rank adaptation |
| 96 | Flash Attention | IO-aware tiled attention |
| 97 | DPO | Direct preference optimization |
| 98 | Mixture of Experts | Sparse conditional computation |
| 99 | Mamba | Selective state space models |
| 100 | **Vision Transformer** | Patches as tokens |

**Tomorrow**: Reproducing GAN training (DCGAN / StyleGAN concepts).
