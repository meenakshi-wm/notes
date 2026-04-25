📘 DAY 43 — Paper Reproduction: Knowledge Distillation

Goal:

Reproduce Knowledge Distillation (Hinton et al., 2015) — train a small "student" model to match a large "teacher" model's soft predictions. Understand temperature scaling and why soft targets carry more information than hard labels.

---

# 🌟 Beginner's Intuition — What Is Knowledge Distillation?

> 🧑‍🍳 **Analogy: Master Chef → Apprentice**
>
> Imagine a **master chef** (teacher model) who has cooked 10,000 dishes. They don't just hand the apprentice (student model) a recipe book — they share *intuition*: "This looks like a soufflé, but it's slightly closer to a mousse because of the texture." That nuanced understanding — not just the final answer — is what **knowledge distillation** transfers.

⭐ **In one sentence:** Knowledge distillation compresses a large, powerful AI model into a smaller, faster one by teaching the small model to mimic the large model's *reasoning*, not just its answers.

| Concept | Analogy | AI Meaning |
|---------|---------|------------|
| 🎓 Teacher model | Master chef with years of experience | Large, accurate (but slow/expensive) model |
| 🧑‍🎓 Student model | Apprentice learning the craft | Small, fast (but less accurate) model |
| 📋 Hard labels | Exam answer key: "C is correct" | One-hot ground truth `[0, 0, 1, 0]` |
| 🌡️ Soft labels | Teacher explaining: "C is best, B is plausible, A/D unlikely" | Probability distribution `[0.02, 0.08, 0.85, 0.05]` |
| 🔥 Temperature $T$ | How detailed the teaching is (T=1 just gives answers, T=20 explains all reasoning) | Controls softness of probability distribution |
| ⚖️ $\alpha$ (alpha) | Balance between textbook study and teacher guidance | Weights hard loss vs. soft loss |

---

# 1️⃣ Core Idea

Hard labels: [0, 0, 1, 0] → "it's a cat"
Soft labels (teacher): [0.02, 0.08, 0.85, 0.05] → "it's a cat, somewhat dog-like"

Soft labels encode inter-class similarities — dark knowledge!

> 🧠 **Why does this matter?** When the teacher says "85% cat, 8% dog, 5% deer, 2% car" — those small probabilities are *gold*. They tell the student that cats look somewhat like dogs (both furry animals) but nothing like cars. Hard labels throw away all that relational knowledge.

⭐ **Dark knowledge** = the information hidden in the *wrong* answers' probabilities. A teacher's "mistakes" are actually rich signals about how classes relate to each other (Hinton et al., 2015).

---

# 2️⃣ Distillation Loss

$$L = \alpha \cdot T^2 \cdot KL(\text{softmax}(z_t/T) \| \text{softmax}(z_s/T)) + (1-\alpha) \cdot CE(y, z_s)$$

Where:
- $z_t$: teacher logits, $z_s$: student logits
- $T$: temperature (typically 2-20)
- $\alpha$: balance between soft and hard targets

### 📐 Key Formulas Explained for Beginners

**Soft targets** — The teacher's "softened" predictions at temperature $T$:

$$p_i^T = \frac{\exp(z_i / T)}{\sum_j \exp(z_j / T)}$$

> 🌡️ **Temperature analogy:** At $T=1$ (cold), the teacher says "It's a cat. Period." At $T=20$ (hot), the teacher says "It's mostly cat, a bit dog-like, slightly deer-ish..." — revealing much richer information.

**Full distillation loss:**

$$\mathcal{L} = \alpha \cdot \mathcal{L}_{CE}(y, p_S) + (1-\alpha) \cdot T^2 \cdot \text{KL}(p_S^T \| p_T^T)$$

Where:
- $\mathcal{L}_{CE}(y, p_S)$ = cross-entropy with ground truth ("textbook learning" 📖)
- $\text{KL}(p_S^T \| p_T^T)$ = KL divergence between student and teacher soft predictions ("learning from teacher" 🧑‍🏫)
- $T^2$ = correction factor because softening reduces gradient magnitudes
- $\alpha$ = how much to trust the textbook vs. the teacher (typically 0.5–0.9)

> ⚖️ **Alpha analogy:** $\alpha = 0.9$ means "mostly listen to the teacher's soft explanations"; $\alpha = 0.1$ means "mostly study the textbook answers." In practice, leaning toward the teacher ($\alpha \geq 0.5$) works best.

---

# 3️⃣ Implementation

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

def distillation_loss(student_logits, teacher_logits, labels, temperature=4.0, alpha=0.7):
    """
    Knowledge distillation loss.
    
    Combines soft target (KD) loss with hard target (CE) loss.
    """
    # Soft targets: teacher's softened predictions
    soft_teacher = F.log_softmax(teacher_logits / temperature, dim=-1)
    soft_student = F.log_softmax(student_logits / temperature, dim=-1)
    
    # KL divergence on soft targets (T² scaling)
    kd_loss = F.kl_div(soft_student, soft_teacher.exp(), reduction='batchmean') * (temperature ** 2)
    
    # Hard target loss
    ce_loss = F.cross_entropy(student_logits, labels)
    
    # Combined loss
    return alpha * kd_loss + (1 - alpha) * ce_loss

class DistillationTrainer:
    def __init__(self, teacher, student, temperature=4.0, alpha=0.7, lr=1e-3):
        self.teacher = teacher.eval()
        for p in self.teacher.parameters():
            p.requires_grad_(False)
        
        self.student = student
        self.temperature = temperature
        self.alpha = alpha
        self.optimizer = torch.optim.Adam(student.parameters(), lr=lr)
    
    def train_step(self, images, labels):
        self.student.train()
        
        # Get teacher predictions (no grad)
        with torch.no_grad():
            teacher_logits = self.teacher(images)
        
        # Get student predictions
        student_logits = self.student(images)
        
        # Distillation loss
        loss = distillation_loss(
            student_logits, teacher_logits, labels,
            self.temperature, self.alpha
        )
        
        self.optimizer.zero_grad()
        loss.backward()
        self.optimizer.step()
        
        acc = (student_logits.argmax(dim=-1) == labels).float().mean()
        return loss.item(), acc.item()
```

---

# 4️⃣ Concrete Experiment: CIFAR-10

```python
# Teacher: ResNet-50 (~23M params) → 95% accuracy
# Student: ResNet-18 (~11M params) → 93% without distillation

# With distillation:
# Student (distilled): 94.2% — closes the gap!

def run_distillation_experiment():
    teacher = torchvision.models.resnet50(num_classes=10)
    teacher.load_state_dict(torch.load('teacher_cifar10.pt'))
    
    student = torchvision.models.resnet18(num_classes=10)
    
    trainer = DistillationTrainer(teacher, student, temperature=4.0, alpha=0.7)
    
    for epoch in range(200):
        for images, labels in train_loader:
            loss, acc = trainer.train_step(images.cuda(), labels.cuda())
        
        val_acc = evaluate(student, val_loader)
        print(f"Epoch {epoch+1}: loss={loss:.4f}, val_acc={val_acc:.2f}%")
```

---

# 5️⃣ Temperature Effect

| Temperature | Soft Distribution | Effect |
|-------------|-------------------|--------|
| T = 1 | Same as normal softmax | No extra info |
| T = 4 | Smoothed, reveals structure | Standard KD |
| T = 20 | Very uniform | Too much smoothing |

Higher T → softer distribution → more dark knowledge transferred.

> 🎯 **Rule of thumb:** Start with $T=4$. If the student isn't learning enough from the teacher, increase $T$. If the student's predictions become too vague, decrease $T$.

---

# 5.5️⃣ 🏭 Production Success Stories

| Model | Teacher → Student | Size Reduction | Performance Retained | Use Case |
|-------|-------------------|---------------|----------------------|----------|
| **DistilBERT** (Sanh et al., 2019) | BERT-base → DistilBERT | 40% smaller, 60% faster | 97% of BERT performance | NLP on mobile/edge |
| **TinyBERT** (Jiao et al., 2020) | BERT-base → TinyBERT | 7.5x smaller | 96% on GLUE | Edge deployment |
| **DeiT** (Touvron et al., 2021) | CNN teacher → ViT student | Comparable size | Competitive with CNNs | Vision transformers without massive data |
| **TinyLlama** (Zhang et al., 2024) | LLaMA architecture | 1.1B params | Strong for size | Lightweight LLM for devices |
| **Alpaca/Vicuna** | GPT-4 / ChatGPT → LLaMA | 70B → 7B | Surprisingly good | Open-source chat models |

⭐ **Real-world impact:**
- 📱 **Mobile deployment:** DistilBERT runs on smartphones where BERT cannot
- 💰 **Cost reduction:** Distilling from GPT-4 API into a self-hosted 7B model can cut inference costs by 10-100x
- ⚡ **Latency:** A distilled 7B model serves responses in ~50ms vs. ~500ms for a 70B model
- 🌍 **Accessibility:** Smaller models democratize AI for resource-constrained settings

---

# 6️⃣ LLM Distillation

```python
# For language models, distill on next-token predictions
def llm_distillation_loss(student_logits, teacher_logits, labels, T=2.0, alpha=0.5):
    """
    student_logits: (B, T_seq, V) student output
    teacher_logits: (B, T_seq, V) teacher output
    labels: (B, T_seq) ground truth tokens
    """
    V = student_logits.size(-1)
    
    # Flatten for loss computation
    s_flat = student_logits.view(-1, V)
    t_flat = teacher_logits.view(-1, V)
    l_flat = labels.view(-1)
    
    # KD loss on vocabulary distribution
    kd_loss = F.kl_div(
        F.log_softmax(s_flat / T, dim=-1),
        F.softmax(t_flat / T, dim=-1),
        reduction='batchmean'
    ) * (T ** 2)
    
    # CE loss on ground truth
    ce_loss = F.cross_entropy(s_flat, l_flat, ignore_index=-100)
    
    return alpha * kd_loss + (1 - alpha) * ce_loss
```

---

# 7️⃣ Startup Applications

| Use Case | Teacher → Student |
|----------|-------------------|
| On-device models | GPT-4 → 1B param model |
| Low-latency serving | 70B → 7B (10x faster) |
| Cost reduction | API model → self-hosted small model |
| Edge deployment | Cloud model → mobile model |

---

# 📝 Summary

| Aspect | Detail |
|--------|--------|
| Core idea | Soft labels carry inter-class information |
| Temperature | T=4 is a good starting point |
| Alpha | 0.5-0.9 (more weight on soft targets) |
| Expected gain | 1-3% accuracy over training from scratch |
| T² scaling | Compensates for gradient magnitude change |

---

# 📚 Research Citations

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|-------------------|
| *Distilling the Knowledge in a Neural Network* | Hinton, Vinyals & Dean | 2015 | ⭐ Foundational paper — introduced temperature scaling and soft target distillation |
| *DistilBERT, a distilled version of BERT* | Sanh, Debut, Chaumond & Wolf | 2019 | Showed 40% compression of BERT with 97% performance retention |
| *Training data-efficient image transformers & distillation through attention* (DeiT) | Touvron et al. | 2021 | Distillation token for vision transformers, no need for massive datasets |
| *Knowledge Distillation: A Survey* | Gou, Yu, Maybank & Tao | 2021 | Comprehensive taxonomy of distillation methods |
| *TinyBERT: Distilling BERT for Natural Language Understanding* | Jiao et al. | 2020 | Two-stage distillation with intermediate layer matching |

---

# 🏁 Cheat Sheet — Knowledge Distillation at a Glance

| What to Remember | Detail |
|------------------|--------|
| 🎯 **Goal** | Compress large teacher → small student |
| 🔑 **Core formula** | $\mathcal{L} = \alpha \mathcal{L}_{CE} + (1-\alpha) T^2 \cdot \text{KL}(p_S^T \| p_T^T)$ |
| 🌡️ **Temperature** | Start with $T=4$; higher = softer distribution |
| ⚖️ **Alpha** | $\alpha \in [0.5, 0.9]$; higher = more weight on soft targets |
| 📋 **Soft targets** | $p_i^T = \frac{\exp(z_i/T)}{\sum_j \exp(z_j/T)}$ |
| 💡 **Dark knowledge** | Small probabilities on wrong classes encode class relationships |
| 🏭 **Hero example** | DistilBERT: 40% smaller, 60% faster, 97% BERT performance |
| ⚠️ **Common pitfall** | Too high $T$ → student learns uniform distribution (useless) |
| ✅ **When to use** | Deploying to mobile/edge, reducing API costs, latency-critical apps |

---

# 📖 Resources

### Papers
- ⭐ **Hinton, Vinyals & Dean (2015)** — *Distilling the Knowledge in a Neural Network* — [arXiv:1503.02531](https://arxiv.org/abs/1503.02531)
- **Sanh et al. (2019)** — *DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter* — [arXiv:1910.01108](https://arxiv.org/abs/1910.01108)
- **Gou et al. (2021)** — *Knowledge Distillation: A Survey* — [arXiv:2006.05525](https://arxiv.org/abs/2006.05525)
- **Touvron et al. (2021)** — *Training data-efficient image transformers & distillation through attention* (DeiT) — [arXiv:2012.12877](https://arxiv.org/abs/2012.12877)
- **Jiao et al. (2020)** — *TinyBERT: Distilling BERT for Natural Language Understanding* — [arXiv:1909.10351](https://arxiv.org/abs/1909.10351)

### Books
- 📕 **"Deep Learning"** by Goodfellow, Bengio & Courville — Chapter 7 (Regularization) covers model compression context
- 📗 **"Pattern Recognition and Machine Learning"** by Bishop — foundational probability/KL divergence background
- 📘 **"Natural Language Processing with Transformers"** by Tunstall, von Werra & Wolf — Chapter on model compression & DistilBERT

---

**Tomorrow**: Reproducing Chinchilla / compute-optimal training.
