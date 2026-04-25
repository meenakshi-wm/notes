📘 DAY 24 — Advanced SFT: Data Mixing, Packing & Multi-Task Training

> *"A well-trained chat model isn't just smart — it was trained smart."* 🎯

## Goal

Master advanced Supervised Fine-Tuning techniques — efficient data packing, multi-task training, catastrophic forgetting prevention, and the data mixing strategies that produce high-quality instruct models.

---

## 🌍 Why This Matters (The Big Picture)

When companies like OpenAI, Google, or Meta build chat models (ChatGPT, Gemini, LLaMA-Chat), they don't just throw data at a model and hope for the best. They use **advanced SFT techniques** to train efficiently, avoid wasting compute, and produce models that can handle diverse tasks.

Think of it this way: a base language model is like a **brilliant student who has read every book in the library** 📚 but has never practiced answering questions, having conversations, or following instructions. **Supervised Fine-Tuning (SFT)** is the "practice phase" — and *advanced* SFT is about making that practice as efficient and effective as possible.

### 🧠 Beginner Analogy: The Restaurant Kitchen

Imagine you're training a new chef 👨‍🍳:
- **Basic SFT** = teaching them one recipe at a time
- **Advanced SFT** = teaching them multiple recipes simultaneously, making sure they don't forget old recipes when learning new ones, and organizing the kitchen so nothing goes to waste

This lesson covers the techniques that make that training process **2-3× faster**, **higher quality**, and **production-ready**.

### ⭐ Key Topics at a Glance

| Topic | What It Does | Beginner Analogy |
|-------|-------------|-----------------|
| 🧩 Data Packing | Fits multiple short examples into one training window | Tetris — fill every gap |
| 🎭 Multi-Task Training | Train on diverse tasks simultaneously | Balanced diet for the model |
| 🧠 Catastrophic Forgetting Prevention | Keep old knowledge while learning new | Studying for finals without forgetting midterm material |
| 📈 Curriculum Learning | Easy examples first, hard ones later | Learning to walk before you run |
| 🎭 Loss Masking | Only grade the model's answers, not the questions | Grading only the student's answers |
| 🔊 NEFTune | Add noise to embeddings for robustness | Practicing in slightly harder conditions |
| 🥗 Data Mixing | Balance different types of training data | A balanced diet |
| 🧹 Quality Filtering & Decontamination | Remove bad/contaminated data | Quality control in a factory |

---

# 1️⃣ Data Packing for Efficiency

## 🎯 The Core Idea

> **Packing** = fitting multiple short training examples into a single training sequence, like playing **Tetris** 🧩 with your data.

### 🧠 Beginner Analogy: The Moving Truck

Imagine you're loading a moving truck 🚚:
- **Without packing**: You put one small box in the truck, drive to the new house, unload, drive back, load another small box... Incredibly wasteful!
- **With packing**: You carefully arrange boxes of different sizes to fill the truck completely. One trip, maximum efficiency!

In AI training, the "truck" is your GPU's memory, and the "boxes" are training examples of different lengths.

## The Padding Problem

Without packing, each example is padded to max_length:
```
Example 1 (100 tokens): [tokens...] [PAD PAD PAD ... PAD]  ← 90% waste 😱
Example 2 (500 tokens): [tokens...] [PAD PAD PAD ... PAD]  ← 50% waste 😰
Example 3 (50 tokens):  [tokens...] [PAD PAD PAD ... PAD]  ← 95% waste 😭
```

Average GPU utilization: 20-50%. Massive waste.

> ⭐ **Why this is a huge problem**: GPUs cost $2-8/hour. If you're wasting 50-80% of each sequence on padding, you're literally burning money. For a training run costing $100K, that's $50-80K wasted on processing empty tokens!

## Solution: Pack Multiple Examples

```
Packed sequence: [Example1 <eos> Example3 <eos> Example7 <eos> Padding]
```

Pack short examples together to fill sequences efficiently.

> 🧠 **Visual**: Think of it like a **Tetris board** — you're fitting differently-shaped pieces together to leave as few gaps as possible:
> ```
> WITHOUT PACKING:          WITH PACKING:
> |███░░░░░░░|              |███████████|
> |██████░░░░|              |██████████░|
> |█░░░░░░░░░|              |███████████|
> ░ = wasted padding         Much less waste!
> ```

```python
def pack_examples(examples, max_length, eos_token_id):
    """Pack multiple examples into single sequences."""
    packed = []
    current_ids = []
    current_labels = []
    current_attention = []
    
    for ex in examples:
        ids = ex['input_ids'].tolist()
        labels = ex['labels'].tolist()
        
        # Check if this example fits in current sequence
        if len(current_ids) + len(ids) + 1 <= max_length:
            # Add separator
            if current_ids:
                current_ids.append(eos_token_id)
                current_labels.append(-100)  # Don't predict EOS
                current_attention.append(0)   # Block cross-example attention
            
            current_ids.extend(ids)
            current_labels.extend(labels)
            current_attention.extend([1] * len(ids))
        else:
            # Save current and start new
            if current_ids:
                packed.append(finalize_sequence(current_ids, current_labels, 
                                                current_attention, max_length))
            current_ids = ids
            current_labels = labels
            current_attention = [1] * len(ids)
    
    if current_ids:
        packed.append(finalize_sequence(current_ids, current_labels,
                                        current_attention, max_length))
    
    return packed

def finalize_sequence(ids, labels, attention, max_length):
    """Pad and finalize a packed sequence."""
    pad_length = max_length - len(ids)
    return {
        'input_ids': torch.tensor(ids + [0] * pad_length),
        'labels': torch.tensor(labels + [-100] * pad_length),
        'attention_mask': torch.tensor(attention + [0] * pad_length),
    }
```

## Performance Impact

With packing:
- GPU utilization: 90%+ (vs 30-50% without)
- Training ~2-3× faster for same number of examples
- Less memory wasted on padding

### 📊 Packing Efficiency: Real Numbers

| Metric | Without Packing | With Packing | Improvement |
|--------|----------------|--------------|-------------|
| GPU Utilization | 20-50% | 90%+ | **2-4× better** |
| Training Speed | Baseline | 2-3× faster | **Huge savings** |
| Memory Waste | 50-80% padding | <10% padding | **Massive reduction** |
| Cost (at $4/GPU-hr for 100hr run) | $400 | ~$150-200 | **Save $200+** |

> 📖 **Research**: Xu et al. (2024) — *"Packing Strategies for Efficient LLM Fine-Tuning"* showed that **bin-packing algorithms** (First-Fit Decreasing) can achieve >95% sequence utilization, compared to naive concatenation which achieves ~85%. The key insight is treating this as a **classic bin-packing optimization problem** from computer science.

### ⭐ Attention Masking in Packed Sequences

One crucial detail: when you pack multiple examples together, you need to make sure the model **doesn't "cheat" by looking across examples**. This is done with **attention masking** — telling self-attention to treat each packed example as independent.

```
Packed: [Example_A tokens] [EOS] [Example_B tokens] [EOS] [PAD...]
Attn:   [1 1 1 1 1 1 1 1 ] [0]  [2 2 2 2 2 2 2 2 ] [0]  [0 0...]

Tokens with mask=1 only attend to other mask=1 tokens
Tokens with mask=2 only attend to other mask=2 tokens
```

> 🧠 **Analogy**: It's like putting multiple students in one exam room 🏫 but giving each one a privacy screen — they're in the same room (GPU) for efficiency, but they can't look at each other's papers!

### 🏭 Production Note

> ⭐ **In Production**: Every major chat model (GPT-4, Claude, Gemini, LLaMA-Chat) uses packing during SFT. The Hugging Face `trl` library has built-in packing support via `ConstantLengthDataset`. If you're training without packing, you're leaving **2-3× speedup on the table** for free.

---

# 1.5️⃣ Loss Masking — Only Grade the Answers 📝

## 🎯 The Core Idea

> **Loss Masking** = computing the training loss **only on the model's response tokens**, not on the user's prompt/question tokens.

### 🧠 Beginner Analogy: Grading a Test

Imagine a teacher grading a student's exam 📋:
- The **question** on the paper: "What is 2+2?" ← *The teacher wrote this, don't grade it!*
- The **student's answer**: "4" ← *THIS is what we grade!*

In SFT, each training example has:
- A **prompt** (user's question) — the model should READ this but NOT be graded on it
- A **response** (assistant's answer) — the model should be graded on GENERATING this

```
Input:  [User: What is the capital of France?] [Assistant: The capital of France is Paris.]
Labels: [-100  -100  -100  -100  -100  -100  -100] [The  capital  of  France  is  Paris  .]

-100 = "ignore this token in loss computation" (PyTorch convention)
```

### Why Loss Masking Is Essential

Without loss masking, the model wastes training effort trying to "predict" the user's question — which it already received as input! This:
1. **Wastes gradient updates** on irrelevant predictions
2. **Dilutes the training signal** for actual response generation
3. **Can hurt response quality** significantly

### Mathematical Formulation

Standard language model loss (without masking):

$$\mathcal{L} = -\frac{1}{T} \sum_{t=1}^{T} \log P(x_t | x_{<t})$$

With loss masking (only response tokens):

$$\mathcal{L}_{\text{masked}} = -\frac{1}{|R|} \sum_{t \in R} \log P(x_t | x_{<t})$$

Where $R$ is the set of **response token positions** and $|R|$ is the number of response tokens.

```python
def create_labels_with_masking(input_ids, response_start_idx):
    """Create labels that only compute loss on response tokens."""
    labels = input_ids.clone()
    # Mask everything before the response (set to -100)
    labels[:response_start_idx] = -100
    return labels

# Example with chat template:
# [BOS] [USER] What is AI? [/USER] [ASSISTANT] AI is... [/ASSISTANT] [EOS]
# Labels: [-100] [-100] [-100 -100 -100] [-100]  [-100]       [AI is...] [/ASSISTANT] [EOS]
```

> ⭐ **Production Impact**: Loss masking is **non-negotiable** for training high-quality chat models. Without it, models tend to produce lower-quality responses and sometimes "echo" parts of the prompt. Every production SFT pipeline uses loss masking.

### 📊 Impact of Loss Masking

| Setting | MT-Bench Score | Response Quality | Training Efficiency |
|---------|---------------|-----------------|-------------------|
| No masking (loss on everything) | ~5.5 | Lower quality, prompt echoing | Wasted gradients |
| Loss masking (response only) | ~7.0+ | Higher quality, focused responses | Efficient gradients |

---

# 1.7️⃣ NEFTune — Free Performance Boost 🔊

## 🎯 The Core Idea

> **NEFTune** (Noisy Embedding Fine-Tuning) = adding a small amount of **random noise** to the input embeddings during training.

### 🧠 Beginner Analogy: Training in Slightly Harder Conditions

Think of an athlete training at **high altitude** 🏔️:
- The air is thinner, making practice harder
- But when they compete at normal altitude, they perform **better than athletes who only trained in easy conditions**

NEFTune works the same way: by adding a tiny bit of "noise" (random perturbation) to the model's input during training, the model becomes more **robust** and actually generates **better text** at inference time!

### How NEFTune Works

During training, after converting tokens to embeddings, add uniform noise:

$$\tilde{e} = e + \frac{\alpha}{\sqrt{L \cdot d}} \cdot \mathcal{U}(-1, 1)$$

Where:
- $e$ = original embedding vector
- $\alpha$ = noise scale (typically 5-15, default 5)
- $L$ = sequence length
- $d$ = embedding dimension
- $\mathcal{U}(-1, 1)$ = uniform random noise between -1 and 1

```python
def neftune_forward(model, input_ids, noise_alpha=5.0):
    """NEFTune: add noise to embeddings during training."""
    embeddings = model.get_input_embeddings()(input_ids)
    
    if model.training:
        # Add uniform noise scaled by alpha
        dims = torch.tensor(embeddings.shape[1] * embeddings.shape[2])  # L * d
        mag_norm = noise_alpha / torch.sqrt(dims)
        noise = torch.zeros_like(embeddings).uniform_(-mag_norm, mag_norm)
        embeddings = embeddings + noise
    
    return embeddings
```

### 📖 Research: The NEFTune Paper

> **Jain et al. (2023)** — *"NEFTune: Noisy Embeddings Improve Instruction Finetuning"*
> - Adding noise with $\alpha = 5$ improved **AlpacaEval** scores by **+15%** on LLaMA-2-7B
> - Improved **MT-Bench** scores from 5.15 → 5.96 (LLaMA-2-7B with Alpaca data)
> - **Zero extra compute cost** — just one line of code change!
> - Works across model sizes (7B, 13B, 70B) and datasets (Alpaca, ShareGPT, Evol-Instruct)
> - Theory: noise acts as implicit **regularization**, preventing overfitting to surface patterns

### 📊 NEFTune Results

| Model + Data | Without NEFTune | With NEFTune ($\alpha=5$) | Improvement |
|-------------|----------------|--------------------------|-------------|
| LLaMA-2-7B + Alpaca | 29.8% (AlpacaEval) | 64.7% | **+34.9%** 🤯 |
| LLaMA-2-7B + ShareGPT | 5.15 (MT-Bench) | 5.96 | **+0.81** |
| LLaMA-2-13B + Alpaca | 42.4% | 68.2% | **+25.8%** |

> ⭐ **Why this is remarkable**: NEFTune is essentially **free performance**. No extra data, no extra compute, no architectural changes — just one line of noise injection. It's one of the highest "return on effort" tricks in SFT.

### 🏭 Production Note

> Hugging Face `trl` library's `SFTTrainer` has a built-in `neftune_noise_alpha` parameter. Just set `neftune_noise_alpha=5` and you get this improvement for free. There's **no reason not to use it**.

---

# 2️⃣ Multi-Task Training

## 🎯 The Core Idea

> **Multi-Task Training** = teaching the model many different skills at once, instead of specializing in just one thing.

### 🧠 Beginner Analogy: The Balanced Diet 🥗

Training a model on diverse tasks is like eating a **balanced diet**:
- Only eating protein 🥩 = only training on code → great at code, terrible at conversation
- Only eating carbs 🍞 = only training on chat → great at chatting, can't solve math
- **Balanced diet** 🥗 = training on code + chat + math + writing → well-rounded model!

The **proportions** matter enormously — just like a nutritionist would tell you to eat 30% protein, 40% carbs, 30% fats, we need to carefully choose how much of each task type to include.

## Why Multi-Task?

Training on diverse tasks simultaneously:
- Improves generalization
- Prevents overfitting to one task type
- Creates more capable models

> ⭐ **Real-world example**: When Meta trained LLaMA-2-Chat, they used a carefully curated mix of instruction following, coding, math, safety, and conversation data. The exact proportions were actively researched and tuned.

## Data Mixing Strategy

```python
class MultiTaskDataset:
    """Mix multiple task datasets with specified proportions."""
    
    TASK_CONFIGS = {
        'instruction_following': {'weight': 0.30, 'lr_scale': 1.0},
        'code_generation': {'weight': 0.20, 'lr_scale': 1.0},
        'math_reasoning': {'weight': 0.15, 'lr_scale': 1.2},
        'creative_writing': {'weight': 0.10, 'lr_scale': 0.8},
        'summarization': {'weight': 0.10, 'lr_scale': 1.0},
        'conversation': {'weight': 0.15, 'lr_scale': 1.0},
    }
    
    def __init__(self, task_datasets):
        self.datasets = task_datasets
        self.weights = [self.TASK_CONFIGS[t]['weight'] for t in task_datasets]
    
    def __iter__(self):
        iterators = {task: iter(ds) for task, ds in self.datasets.items()}
        
        while True:
            # Sample task based on weights
            task = random.choices(
                list(self.datasets.keys()),
                weights=self.weights
            )[0]
            
            try:
                yield next(iterators[task])
            except StopIteration:
                iterators[task] = iter(self.datasets[task])
                yield next(iterators[task])
```

### 📊 Typical Data Mix for a Strong Chat Model

| Task Category | Weight | Why This Amount | Example Data |
|--------------|--------|----------------|-------------|
| 📝 Instruction Following | 30% | Core skill — must follow directions well | Alpaca, OpenAssistant |
| 💻 Code Generation | 20% | High demand, improves reasoning too | CodeAlpaca, Magicoder |
| 🔢 Math Reasoning | 15% | Chain-of-thought reasoning transfer | GSM8K, MATH |
| 🎨 Creative Writing | 10% | Fluency and style variety | Writing prompts |
| 📋 Summarization | 10% | Information compression skill | CNN/DailyMail |
| 💬 Conversation | 15% | Multi-turn dialogue capability | ShareGPT, UltraChat |

> 📖 **Research Insight**: The exact mix ratios significantly impact model capabilities. Too much code data can make conversations feel "robotic." Too much chat data reduces reasoning ability. Companies often run **hundreds of mixing ratio experiments** to find the optimal balance.

### ⭐ Data Mixing Anti-Patterns (Common Mistakes)

| ❌ Anti-Pattern | 🤔 What Happens | ✅ Fix |
|----------------|-----------------|--------|
| 100% instruction-following | Model becomes a Q&A bot, can't hold conversations | Add 15%+ conversation data |
| 90% code | Great at coding, terrible personality | Balance with chat + writing |
| No math data | Can't do basic reasoning | Add at least 10-15% reasoning tasks |
| All English | Catastrophic for multilingual users | Include multilingual data proportionally |

---

# 2.5️⃣ Quality Filtering & Decontamination 🧹

## 🎯 The Core Idea

> **Quality Filtering** = removing low-quality examples from your training data
> **Decontamination** = removing examples that overlap with your evaluation benchmarks

### 🧠 Beginner Analogy: Quality Control in a Factory

Imagine a cookie factory 🍪:
- **Quality filtering** = removing burned, broken, or undersized cookies before packaging
- **Decontamination** = making sure you're not accidentally shipping cookies that were supposed to be test samples

If bad data gets into training, the model learns bad habits. If benchmark data leaks into training, your evaluation scores are meaningless (the student memorized the test answers!).

### Quality Filtering Methods

```python
def quality_filter(examples):
    """Multi-stage quality filtering pipeline."""
    filtered = []
    for ex in examples:
        response = ex['output']
        
        # Length filters
        if len(response.split()) < 10:
            continue  # Too short — probably low quality
        if len(response.split()) > 5000:
            continue  # Too long — probably noise/dump
        
        # Content quality
        if response.count('...') > 5:
            continue  # Lazy/incomplete responses
        if response.startswith("I cannot") or response.startswith("I'm sorry"):
            continue  # Refusals (unless safety-relevant)
        
        # Repetition check
        sentences = response.split('.')
        unique_ratio = len(set(sentences)) / max(len(sentences), 1)
        if unique_ratio < 0.5:
            continue  # Too repetitive
        
        # Formatting check
        if response.count('\n\n\n') > 3:
            continue  # Excessive whitespace/bad formatting
        
        filtered.append(ex)
    
    return filtered
```

### Decontamination: Why It Matters

```python
def decontaminate(train_data, eval_benchmarks, n_gram_size=13):
    """Remove training examples that overlap with evaluation benchmarks."""
    # Build n-gram index from benchmarks
    benchmark_ngrams = set()
    for bench_example in eval_benchmarks:
        text = bench_example['text'].lower()
        words = text.split()
        for i in range(len(words) - n_gram_size + 1):
            ngram = ' '.join(words[i:i+n_gram_size])
            benchmark_ngrams.add(ngram)
    
    # Filter training data
    clean_data = []
    contaminated_count = 0
    for train_example in train_data:
        text = train_example['output'].lower()
        words = text.split()
        is_contaminated = False
        
        for i in range(len(words) - n_gram_size + 1):
            ngram = ' '.join(words[i:i+n_gram_size])
            if ngram in benchmark_ngrams:
                is_contaminated = True
                break
        
        if not is_contaminated:
            clean_data.append(train_example)
        else:
            contaminated_count += 1
    
    print(f"Removed {contaminated_count} contaminated examples")
    return clean_data
```

> ⭐ **Why 13-grams?** OpenAI's GPT-4 technical report uses 13-gram overlap detection. This strikes a balance: long enough to avoid false positives (common phrases), short enough to catch actual benchmark contamination. Some researchers use 10-grams or even exact match depending on the benchmark.

> 📖 **Research**: The contamination problem is widespread. Studies have shown that many popular fine-tuning datasets contain verbatim copies of benchmark test sets (MMLU, HellaSwag, etc.). Models trained on contaminated data show inflated benchmark scores that don't reflect real capabilities.

---

# 3️⃣ Preventing Catastrophic Forgetting

## 🎯 The Core Idea

> **Catastrophic Forgetting** = when a model FORGETS previously learned knowledge because new training overwrites the old knowledge.

### 🧠 Beginner Analogy: The Musician's Dilemma 🎵

Imagine a musician who's mastered piano 🎹. They decide to intensively learn guitar 🎸 for 6 months, practicing 8 hours a day. When they go back to the piano... they've forgotten half of what they knew! Their fingers have been "reprogrammed" for guitar.

This is **exactly** what happens to language models during fine-tuning:
- The model spent months learning general knowledge during pre-training (the "piano")
- During SFT, it intensively learns to follow instructions (the "guitar")
- If we're not careful, it **forgets** how to do things it could do before (like writing poetry, doing math, or speaking other languages)

## The Problem

Fine-tuning on task-specific data can make the model FORGET general knowledge.

> ⭐ **Real example**: A model fine-tuned only on English instruction data may forget how to generate coherent text in French or Chinese — even though it could before fine-tuning!

## Solution 1: Replay Buffer

Mix some pre-training data into fine-tuning:

> 🧠 **Analogy**: Like a student preparing for a new exam while periodically **reviewing flashcards** from previous courses. You spend 90% studying new material, 10% refreshing old knowledge.

```python
class ReplayMixedDataset:
    def __init__(self, finetune_data, pretrain_data, replay_ratio=0.1):
        self.finetune = finetune_data
        self.pretrain = pretrain_data
        self.replay_ratio = replay_ratio
    
    def __iter__(self):
        ft_iter = iter(self.finetune)
        pt_iter = iter(self.pretrain)
        
        for ft_example in ft_iter:
            yield ft_example
            
            # Occasionally yield a pre-training example
            if random.random() < self.replay_ratio:
                try:
                    yield next(pt_iter)
                except StopIteration:
                    pt_iter = iter(self.pretrain)
                    yield next(pt_iter)
```

## Solution 2: Elastic Weight Consolidation (EWC)

Penalize changing weights that were important for pre-training:

> 🧠 **Analogy**: Imagine each brain connection has a "stiffness rating" 🔧. Connections that were crucial for old skills are **stiff** (hard to change), while less important ones are **flexible** (free to adapt to new tasks). This way, learning guitar doesn't overwrite your piano muscle memory!

### The Math Behind EWC

The EWC loss adds a penalty term:

$$\mathcal{L}_{\text{EWC}} = \mathcal{L}_{\text{SFT}} + \frac{\lambda}{2} \sum_{i} F_i (\theta_i - \theta_i^*)^2$$

Where:
- $\mathcal{L}_{\text{SFT}}$ = normal fine-tuning loss
- $\lambda$ = EWC strength (how much to protect old knowledge)
- $F_i$ = **Fisher Information** for parameter $i$ (how important it was for pre-training)
- $\theta_i$ = current parameter value
- $\theta_i^*$ = original pre-trained parameter value

> ⭐ **Key Insight**: Parameters with HIGH Fisher Information were crucial for pre-training → we penalize changes to them heavily. Parameters with LOW Fisher Information were not doing much → free to change for the new task.

```python
class EWCRegularizer:
    def __init__(self, model, pretrain_dataloader, lambda_ewc=1000):
        self.lambda_ewc = lambda_ewc
        
        # Compute Fisher Information Matrix (importance of each parameter)
        self.fisher = self._compute_fisher(model, pretrain_dataloader)
        
        # Store original parameters
        self.original_params = {n: p.clone() for n, p in model.named_parameters()}
    
    def penalty(self, model):
        """EWC penalty: penalize changes to important parameters."""
        loss = 0
        for name, param in model.named_parameters():
            if name in self.fisher:
                loss += (self.fisher[name] * (param - self.original_params[name]) ** 2).sum()
        return self.lambda_ewc * loss
```

## Solution 3: Low Learning Rate + Short Training

The simplest approach: use low LR (1e-5 to 5e-5) and train for only 1-3 epochs.

> 🧠 **Analogy**: Instead of immersing yourself in guitar 8 hours/day (which overwrites piano), you practice guitar gently — 1 hour/day, with light touch. You learn slower but **keep your piano skills intact**.

### 📊 Forgetting Prevention: Method Comparison

| Method | Complexity | Effectiveness | Compute Overhead | Best For |
|--------|-----------|---------------|-----------------|----------|
| 🟢 Low LR + Short Training | Very Low | Good | None | Quick experiments |
| 🟡 Replay Buffer (10%) | Low | Very Good | +10% data | Production models |
| 🔴 EWC | High | Excellent | +30-50% compute | Research, critical apps |
| 🟡 LoRA (only train adapters) | Low | Very Good | None (less params) | Resource-constrained |

> ⭐ **Production Recommendation**: In practice, most teams use **Replay Buffer + Low LR** as the default strategy. EWC is powerful but expensive. LoRA sidesteps the problem entirely by only training a small number of new parameters (covered in Day 23).

---

# 3.5️⃣ Multi-Turn Conversation Training 💬

## 🎯 The Core Idea

> **Multi-turn training** = teaching the model to handle **back-and-forth conversations**, not just single question-answer pairs.

### 🧠 Beginner Analogy: The Difference Between a Quiz and a Tutorial

- **Single-turn** (basic SFT): Like a pop quiz 📝 — one question, one answer, done
- **Multi-turn** (advanced SFT): Like a **tutoring session** 👩‍🏫 — "Let me explain that differently... Now do you understand? Good, let's build on that..."

Real conversations have **context** that carries forward. A user might say "make it shorter" — and the model needs to remember what "it" refers to from 3 messages ago!

### Multi-Turn Chat Template

```
<|system|>You are a helpful assistant.<|end|>
<|user|>What is machine learning?<|end|>
<|assistant|>Machine learning is a subset of AI where...<|end|>
<|user|>Can you give me a simple example?<|end|>
<|assistant|>Sure! Imagine you have a spam filter...<|end|>
<|user|>How does it learn from data?<|end|>
<|assistant|>The spam filter learns by...<|end|>
```

### Loss Masking in Multi-Turn Conversations

> ⭐ **Critical Detail**: In multi-turn training, we apply loss masking to **every user turn AND system prompt**, computing loss ONLY on assistant responses:

```
Tokens:  [system prompt] [user msg 1] [asst reply 1] [user msg 2] [asst reply 2]
Labels:  [-100 -100 ...]  [-100 ...]   [COMPUTE LOSS]  [-100 ...]   [COMPUTE LOSS]
```

This means the model is graded on ALL its replies in the conversation, while treating all user messages and system prompts as context.

```python
def create_multiturn_labels(messages, tokenizer):
    """Create labels for multi-turn conversation — loss only on assistant turns."""
    full_text = ""
    labels = []
    
    for msg in messages:
        role_text = f"<|{msg['role']}|>{msg['content']}<|end|>"
        tokens = tokenizer.encode(role_text, add_special_tokens=False)
        
        if msg['role'] == 'assistant':
            # Compute loss on assistant tokens
            labels.extend(tokens)
        else:
            # Mask user/system tokens
            labels.extend([-100] * len(tokens))
        
        full_text += role_text
    
    input_ids = tokenizer.encode(full_text, add_special_tokens=True)
    return input_ids, labels
```

### 🏭 Production Note: Why Multi-Turn Matters

> Every chat model (ChatGPT, Claude, Gemini) is trained on multi-turn conversations. Single-turn-only training produces models that:
> - Lose context mid-conversation
> - Can't handle follow-up questions like "explain that more simply"
> - Feel robotic and stateless
> 
> **Industry standard**: At least 40-60% of SFT data should be multi-turn conversations (2-10+ turns).

---

# 4️⃣ Curriculum Learning for SFT

## 🎯 The Core Idea

> **Curriculum Learning** = teaching the model **easy examples first, then gradually introducing harder ones** — just like how school works!

### 🧠 Beginner Analogy: Learning to Swim 🏊

You wouldn't throw a beginner into the deep end of the ocean! Instead:
1. **Week 1**: Splash around in the shallow end (easy examples — short Q&A)
2. **Week 2**: Swim laps in the pool (medium — paragraph-length responses)
3. **Week 3**: Open water swimming (hard — multi-step reasoning, code generation)

The model learns basic patterns first, then builds on that foundation for harder tasks. Research shows this leads to **faster convergence** and sometimes **better final quality**.

Train on easier examples first, harder examples later:

```python
class CurriculumSampler:
    """Progressively increase difficulty during training."""
    
    def __init__(self, dataset, difficulty_fn, total_steps):
        self.dataset = dataset
        self.total_steps = total_steps
        
        # Score each example by difficulty
        self.difficulties = [difficulty_fn(ex) for ex in dataset]
        self.sorted_indices = sorted(range(len(dataset)), 
                                     key=lambda i: self.difficulties[i])
    
    def get_batch_indices(self, current_step, batch_size):
        """Get indices for current training phase."""
        # Linear curriculum: start with easy, end with all
        progress = current_step / self.total_steps
        available = int(len(self.dataset) * min(0.2 + 0.8 * progress, 1.0))
        
        # Sample from available (easiest) examples
        available_indices = self.sorted_indices[:available]
        return random.sample(available_indices, min(batch_size, len(available_indices)))

def estimate_difficulty(example):
    """Heuristic difficulty estimation."""
    response = example['output']
    score = 0
    score += len(response.split()) / 100  # Longer = harder
    score += response.count('```') * 0.5   # Code = harder
    score += sum(1 for c in response if c in '∫∑∏√') * 1.0  # Math = harder
    return score
```

### 📊 Curriculum Schedule Visualization

```
Training Progress →
100% |                              ████████
     |                      ████████
     |              ████████
Data |      ████████
Used |██████
  0% |________________________________________
     Easy                              Hard
     "What is Python?"         "Implement a B-tree
                                with concurrent
                                access support"
```

> 📖 **Research Insight**: Curriculum learning was popularized by Bengio et al. (2009) — *"Curriculum Learning."* For SFT specifically, starting with clean, short, high-quality examples and gradually introducing complex multi-step reasoning tasks leads to **5-15% faster convergence** in practice.

---

# 5️⃣ Handling Long Conversations

## 🎯 The Core Idea

> Most models have a **maximum context window** (e.g., 2048 or 4096 tokens). But real conversations can be much longer! We need a strategy to **chunk** long conversations into trainable pieces.

### 🧠 Beginner Analogy: Reading a Long Book 📖

If your desk can only hold 3 pages at a time (= context window), but you need to study a 20-page chapter:
- You read pages 1-3, take notes
- Then pages 3-6 (with some overlap for context!)
- Then pages 6-9...

The **overlap** ensures you don't lose track of the story. Similarly, when chunking conversations, we keep the last user message as context for the next chunk.

```python
def chunk_long_conversation(conversation, max_length, tokenizer):
    """Split long conversations into trainable chunks."""
    chunks = []
    current_chunk = []
    current_length = 0
    
    for message in conversation:
        msg_tokens = len(tokenizer.encode(message['content']))
        
        if current_length + msg_tokens > max_length and current_chunk:
            # Save current chunk
            chunks.append(current_chunk)
            
            # Start new chunk with last user message as context
            last_user = next((m for m in reversed(current_chunk) 
                            if m['role'] == 'user'), None)
            current_chunk = [last_user] if last_user else []
            current_length = msg_tokens
        
        current_chunk.append(message)
        current_length += msg_tokens
    
    if current_chunk:
        chunks.append(current_chunk)
    
    return chunks
```

---

# 6️⃣ SFT Best Practices Checklist

> ⭐ **Print this out and check every box before your next SFT run!**

```
✅ Data Quality
  □ Diverse task types (not all Q&A)
  □ Detailed, structured responses
  □ Consistent formatting across examples
  □ Deduplication performed
  □ Quality filtering applied
  □ Decontamination against eval benchmarks
  □ Multi-turn conversations included (40-60% of data)
  □ Balanced data mix across task categories

✅ Training Setup
  □ Learning rate: 1e-5 to 5e-5 (full FT) or 1e-4 to 3e-4 (LoRA)
  □ Label masking: only train on response tokens
  □ Chat template: matches model/tokenizer format
  □ Gradient accumulation for effective batch ≥16
  □ Cosine schedule with warmup
  □ NEFTune enabled (noise_alpha=5)
  □ Replay buffer (5-10% pre-training data)

✅ Memory Optimization
  □ Gradient checkpointing enabled
  □ Mixed precision (bf16/fp16)
  □ Data packing (reduce padding waste)
  □ set_to_none=True for zero_grad

✅ Evaluation
  □ Validation set perplexity
  □ Generation quality on test prompts
  □ Task-specific metrics
  □ Check for catastrophic forgetting
  □ Decontamination verified
```

---

# 7️⃣ 🧾 Advanced SFT Cheat Sheet

> **Quick reference for everything covered in this lesson** — pin this!

### ⭐ The Five Pillars of Advanced SFT

| # | Technique | One-Line Summary | Impact | Difficulty |
|---|-----------|-----------------|--------|-----------|
| 1 | 🧩 **Packing** | Pack multiple examples per sequence like Tetris | 2-3× training speedup | Easy |
| 2 | 📝 **Loss Masking** | Only compute loss on response tokens | Essential for quality | Easy |
| 3 | 🔊 **NEFTune** | Add noise to embeddings during training | +15% on benchmarks, FREE | Trivial |
| 4 | 🥗 **Data Mixing** | Balance task types like a balanced diet | Well-rounded model | Medium |
| 5 | 💬 **Multi-Turn** | Train on conversations, not just Q&A | Chat model quality | Medium |

### ⭐ Key Hyperparameters

| Parameter | Recommended Value | Notes |
|-----------|------------------|-------|
| Learning Rate (full FT) | $1 \times 10^{-5}$ to $5 \times 10^{-5}$ | Lower = less forgetting |
| Learning Rate (LoRA) | $1 \times 10^{-4}$ to $3 \times 10^{-4}$ | Higher ok since fewer params |
| Epochs | 1-3 | More = more forgetting risk |
| Batch Size (effective) | ≥16 | Use gradient accumulation |
| NEFTune $\alpha$ | 5 | 5-15 range, 5 is safe default |
| Replay Ratio | 5-10% | Pre-training data mixed in |
| Warmup Ratio | 3-5% of steps | Standard practice |
| Max Sequence Length | 2048-4096 | Depends on use case |

### ⭐ Decision Flowchart

```
Starting SFT?
├── Do you have multi-turn conversation data?
│   ├── YES → Format with chat template, mask non-assistant turns ✅
│   └── NO → Collect multi-turn data! Single-turn only = weak chat model ⚠️
├── Are training examples different lengths?
│   ├── YES → Enable packing (2-3× speedup FREE) ✅
│   └── NO → Packing helps less but still worth it
├── Is training data >10K examples?
│   ├── YES → Quality filter + decontaminate first ✅
│   └── NO → Manual review is feasible, do it
├── Worried about forgetting?
│   ├── YES → Use replay buffer (5-10%) + low LR ✅
│   └── NO → You should be! Add replay buffer anyway
└── Want free improvement?
    └── Enable NEFTune (alpha=5) ✅ — literally no downside
```

### 📚 Key Research Papers

| Paper | Authors | Year | Key Contribution |
|-------|---------|------|-----------------|
| *NEFTune: Noisy Embeddings Improve Instruction Finetuning* | Jain et al. | 2023 | Noise in embeddings = free +15% improvement |
| *Packing Strategies for Efficient LLM Fine-Tuning* | Xu et al. | 2024 | Bin-packing algorithms for >95% utilization |
| *Curriculum Learning* | Bengio et al. | 2009 | Easy-to-hard training schedule |
| *Overcoming Catastrophic Forgetting* | Kirkpatrick et al. | 2017 | EWC (Elastic Weight Consolidation) |
| *LIMA: Less Is More for Alignment* | Zhou et al. | 2023 | 1000 high-quality examples can match GPT-4 |
| *UltraChat: Large-scale Multi-turn Data* | Ding et al. | 2023 | Importance of multi-turn training data |

---

# 📝 Summary

| Technique | Benefit |
|-----------|---------|
| Data packing | 2-3× training speedup, less memory waste |
| Multi-task mixing | Better generalization, more capable model |
| Replay buffer | Prevents catastrophic forgetting |
| Curriculum learning | Faster convergence, better final quality |
| Long conversation chunking | Handle arbitrary-length dialogues |
| Label masking | Only train on response, not prompt |
| NEFTune | Free +15% improvement on benchmarks |
| Quality filtering | Remove bad examples, cleaner training signal |
| Decontamination | Honest benchmark scores, no data leakage |
| Multi-turn training | Natural conversation ability |

### 🎓 Key Takeaways for Beginners

1. 🧩 **Packing** is like Tetris — fill every gap in your GPU memory for 2-3× speedup
2. 📝 **Loss masking** = grade only the student's answers, not the teacher's questions
3. 🔊 **NEFTune** = practice in slightly harder conditions → perform better when it counts
4. 🥗 **Data mixing** = balanced diet → well-rounded model
5. 💬 **Multi-turn** = train through conversations, not just isolated Q&A
6. 🧠 **Forgetting prevention** = study for new exams while reviewing old flashcards
7. 🧹 **Quality > quantity** — 1000 perfect examples can beat 100K mediocre ones (LIMA paper)

> 💡 **The #1 takeaway**: Advanced SFT isn't about one magic trick — it's about **combining all these techniques together**. The best chat models use ALL of them: packing + loss masking + NEFTune + data mixing + multi-turn training + quality filtering + decontamination. Each one adds incremental improvement, and together they compound into dramatically better models.

**Tomorrow**: RLHF & DPO — The alignment techniques that make models helpful and safe.
