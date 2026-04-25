📘 DAY 9 — Vocabulary Design & Special Tokens for LLM Training 📖🔤✨

---

## 🎯 Goal

Master the art and science of vocabulary design — how to choose vocab size, what special tokens to include, how chat templates work, and how tokenizer design decisions propagate through the entire model training and inference pipeline.

## ✅ If This Is Deeply Understood

You can design tokenizer configurations for any LLM use case, understand why different models have different special tokens, and debug subtle issues in fine-tuning and inference that stem from tokenizer misconfigs.

---

## 🧒 The "Absolute Beginner" Big Picture

Imagine you're writing a **dictionary** for a robot 🤖. Before the robot can read *anything*, it needs a list of every word (and word-piece) it will ever recognize. That list is the **vocabulary**.

| Real-World Analogy | AI Equivalent |
|---|---|
| 📚 A massive unabridged dictionary (every word ever written) | A **large vocabulary** — the model knows many words, but the dictionary itself is heavy to carry around |
| 📕 A pocket travel phrase-book | A **small vocabulary** — lightweight, but you'll often hit words that aren't in the book |
| 🚏 Road signs like STOP, YIELD, ONE WAY | **Special tokens** — fixed signals that tell the model "start here", "stop here", "this is padding" |
| 🤷 Encountering a word not in your phrase-book | **Out-of-vocabulary (OOV)** — the model has no idea what that token means |
| 🏥 A medical textbook vs. an everyday dictionary | **Domain-specific vocabulary** — specialized terms for specialized tasks |

> ⭐ **Key Insight**: The vocabulary is baked into the model at birth. Changing it later is like telling a human "forget English, speak Klingon" — technically possible, but *extremely* painful.

---

# 1️⃣ Vocabulary Size: The Critical Trade-off 📐⚖️

## 🧒 Beginner Analogy — The Dictionary Dilemma

Picture two students studying for a foreign-language exam:

- **Student A** carries a *50-pound unabridged dictionary* 📚. She can look up any word instantly — but her backpack weighs a ton, and many pages are for words she'll never see.
- **Student B** carries a *slim pocket dictionary* 📕. His bag is light and fast to search — but every other sentence has a word he can't find, so he has to spell it out letter-by-letter.

The **vocabulary size** of a language model is exactly this choice: *how big should the model's dictionary be?*

## 📊 The Mathematical View

Given text corpus $C$ with total characters $N$:
- Vocabulary $V$ with $|V|$ tokens
- Average tokens per text: $T(|V|)$

$$\text{Embedding parameters} = |V| \times d_{\text{model}}$$

$$\text{Sequence length} = T(|V|) \quad \text{(decreases as } |V| \text{ increases)}$$

$$\text{Attention compute} = O\!\bigl(T(|V|)^{2}\bigr) \text{ per layer}$$

> ⭐ **Why this matters**: The embedding table is a giant matrix of size $|V| \times d_{\text{model}}$. Every row is a trainable vector that represents one token. More tokens → more rows → more GPU memory.

### 🔢 Quick Numbers

| Quantity | Formula | Example ($|V|=50{,}000$, $d=4096$) |
|---|---|---|
| Embedding parameters | $|V| \times d_{\text{model}}$ | $50{,}000 \times 4{,}096 = 204{,}800{,}000$ (~205 M) |
| Memory (fp16) | $|V| \times d \times 2\text{ bytes}$ | $\approx 390\text{ MB}$ |
| Memory (fp32) | $|V| \times d \times 4\text{ bytes}$ | $\approx 780\text{ MB}$ |

## 🎯 Optimal Vocabulary Size

There's a sweet spot where:

$$\text{Total compute} = \underbrace{\text{Embedding cost}}_{\propto\, |V|} + \underbrace{\text{Attention cost}}_{\propto\, T(|V|)^{2}} + \text{Processing cost}$$

- **Too small** → long sequences → $O(n^{2})$ attention dominates 🐢
- **Too large** → huge embedding table → memory dominates + rare tokens undertrained 💸

> 🧒 **Analogy**: It's like choosing how many shelves your bookstore has. Too few shelves → books are piled high and hard to find (long sequences). Too many shelves → most are empty and you're paying rent for unused space (wasted parameters).

### 📈 Zipf's Law — Why Most Tokens Are Rare

In any natural language corpus, word frequency follows **Zipf's Law**:

$$f(r) \propto \frac{1}{r^{\alpha}} \qquad (\alpha \approx 1)$$

where $r$ is the rank of the word (1 = most common, 2 = second most common, …).

| Rank | Example Word | Relative Frequency |
|---|---|---|
| 1 | "the" | $\sim 7\%$ |
| 10 | "it" | $\sim 0.7\%$ |
| 100 | "world" | $\sim 0.07\%$ |
| 1,000 | "algorithm" | $\sim 0.007\%$ |
| 10,000 | "serendipity" | $\sim 0.0007\%$ |

> ⭐ **Implication**: The top 10 % of your vocabulary covers ~90 % of text. The bottom 50 % of tokens might each appear fewer than 100 times in the entire training set — meaning their embeddings are barely trained and largely useless. This is why **bigger is not always better**.

### 📏 Vocabulary Coverage

$$\text{Coverage} = \frac{\text{tokens recognized by vocab}}{\text{total tokens in corpus}}$$

With **byte-level BPE** (used by GPT-2 and later), coverage is always $100\%$ because any unknown byte sequence can be represented character-by-character. The real question is **compression efficiency** — how many tokens does it take to encode a sentence?

## 📋 Empirical Sweet Spots — What Real Models Use

| Model | Vocab Size | Reasoning |
|-------|-----------|-----------|
| GPT-2 | 50,257 | English-focused, moderate size |
| GPT-4 | ~100,000 | Multilingual, code, broad coverage |
| LLaMA-2 | 32,000 | Compact, efficient for English |
| LLaMA-3 | 128,256 | Multilingual support needed |
| Mistral | 32,000 | Efficiency-focused |
| Gemma | 256,000 | Massive multilingual |
| BERT | 30,522 | WordPiece, English-centric |
| T5 | 32,100 | SentencePiece unigram |

### 🏭 Production Rule of Thumb

| Use Case | Recommended Vocab Size | Why |
|---|---|---|
| English-only chat model | **32 K** | Fast, small embedding table, good compression |
| Multilingual (5-10 langs) | **64–128 K** | Each language needs its own common tokens |
| Universal / 100+ languages | **256 K+** | "Multilingual tax" — you pay for coverage |
| Code-heavy model | **50–100 K** | Need tokens for keywords, operators, indentation |

> ⭐ **"Multilingual Tax"**: Supporting more languages forces a larger vocabulary. This steals embedding capacity from each individual language, so a 128 K multilingual model may encode English *less efficiently* than a 32 K English-only model. This is a fundamental trade-off every production team wrestles with.

## 🔬 Deep Research: How Vocab Size Is Chosen in Practice

1. **Empirical ablation**: Teams train small-scale models with different vocab sizes (e.g., 16 K, 32 K, 64 K, 128 K) and measure downstream perplexity per FLOP.
2. **Compression ratio**: Measure average tokens-per-word on a held-out corpus. Lower is better (a 32 K BPE on English typically gives ~1.3 tokens/word; on Japanese it might be ~2.5).
3. **Marginal token utility**: Plot the frequency of the $n$-th most common merge. Once new merges cover vanishingly rare substrings, adding more hurts more than it helps.
4. **Byte-level fallback**: Modern BPE always falls back to individual bytes, so OOV rate is $0\%$ — the question is efficiency, not coverage.

---

# 2️⃣ Special Tokens Deep Dive 🚏🔖

## 🧒 Beginner Analogy — Road Signs for the Model

Think of special tokens as **road signs** on a highway 🛣️:

| Road Sign | Special Token | Purpose |
|---|---|---|
| 🟢 **"Highway Starts Here"** | `<bos>` (beginning of sequence) | Tells the model: "A new text begins now" |
| 🔴 **"Road Ends / Exit"** | `<eos>` (end of sequence) | Tells the model: "Stop generating, you're done" |
| ⬜ **"Empty Parking Space"** | `<pad>` (padding) | Fills empty slots so all texts in a batch are the same length |
| ❓ **"Unknown Road"** | `<unk>` (unknown token) | "I've never seen this word before" (shouldn't appear with byte-level BPE!) |
| 🗣️ **"Speaker Change"** | `<|user|>`, `<|assistant|>` | Marks who is talking in a conversation |

> ⭐ **Key Insight**: Special tokens are **not** regular words. The tokenizer never *merges* them from smaller pieces — it recognizes them as atomic, indivisible symbols, like a traffic light that is always interpreted the same way.

## 🏷️ Categories of Special Tokens

### 🏗️ Structural Tokens
```
<bos>  — Beginning of sequence
<eos>  — End of sequence  
<pad>  — Padding (for batching)
<unk>  — Unknown token (shouldn't appear with byte-level BPE)
```

### 💬 Chat / Instruction Tokens
```
<|system|>     — System prompt start
<|user|>       — User message start
<|assistant|>  — Assistant message start
<|end|>        — Turn end
[INST]         — Instruction start (LLaMA-chat)
[/INST]        — Instruction end (LLaMA-chat)
```

### 🎛️ Task / Control Tokens
```
<|code|>       — Code block indicator
<|tool_call|>  — Function calling
<|tool_result|>— Function result
<|thinking|>   — Chain of thought
```

## ⚙️ How Special Tokens Work Internally

Special tokens:
1. 🎰 Get their own embedding vector (randomly initialized, then trained)
2. 🚫 Are **NEVER** produced by the BPE merge algorithm
3. 🔍 Are matched **literally** during tokenization (before BPE runs)
4. 📌 Usually have fixed IDs (often at the start of vocab)

> 🧒 **Analogy**: Imagine a post office sorting machine 📬. Normally it reads addresses and routes letters. But some envelopes have a bright red "PRIORITY" sticker — the machine doesn't try to read the address; it immediately routes them to the express lane. Special tokens are those bright red stickers.

### 🔬 Deep Dive: Embedding Initialization for New Special Tokens

When you add a new special token, its embedding vector starts as **random noise**. During fine-tuning, the model learns meaningful representations. Two common initialization strategies:

| Strategy | How | When to Use |
|---|---|---|
| Random init | `nn.Embedding` default | Plenty of fine-tuning data |
| Mean-of-existing | Average all existing token embeddings | Little fine-tuning data — safer starting point |

```python
# In HuggingFace:
tokenizer.add_special_tokens({
    "bos_token": "<s>",
    "eos_token": "</s>",
    "pad_token": "<pad>",
    "additional_special_tokens": ["<|system|>", "<|user|>", "<|assistant|>"]
})

# CRITICAL: Resize model embeddings!
model.resize_token_embeddings(len(tokenizer))
```

> ⚠️ **Production Gotcha**: If you add special tokens but **forget** to call `resize_token_embeddings()`, the model will crash with an index-out-of-range error the moment it sees the new token ID. This is one of the most common fine-tuning bugs.

---

# 3️⃣ Chat Templates: The Critical Format Layer 💬📝

## 🧒 Beginner Analogy — The Envelope Format

Imagine you're writing letters to a robot pen pal 💌. The robot *only* understands letters formatted a very specific way:

1. The **envelope** must say who's writing (system? user? assistant?).
2. Each letter must have a clear **start stamp** and **end stamp**.
3. If you use the wrong envelope format, the robot reads gibberish and writes back nonsense.

That's exactly what a **chat template** does — it wraps raw conversation turns into the exact text format the model was trained on.

## 🤔 Why Chat Templates Exist

A base model sees: continuous text.
A chat model expects: structured turns.

| Without Template | With Template |
|---|---|
| The model sees one long blob of text | The model sees clear boundaries between speakers |
| Can't distinguish system instructions from user input | Knows exactly what's a system prompt vs. user message |
| Generates continuation of raw text | Generates a proper *reply* to the user |

The chat template converts:
```
[
  {"role": "system", "content": "You are helpful."},
  {"role": "user", "content": "Hello!"},
  {"role": "assistant", "content": "Hi there!"}
]
```

Into tokenizer-specific format. ⬇️

## 🦙 LLaMA-2 Chat Format
```
<s>[INST] <<SYS>>
You are helpful.
<</SYS>>

Hello! [/INST] Hi there! </s>
```

> 🧒 Think of `[INST]...[/INST]` as quotation marks that say "the user said this." Everything outside is the assistant's reply.

## 🦙 LLaMA-3 Chat Format
```
<|begin_of_text|><|start_header_id|>system<|end_header_id|>

You are helpful.<|eot_id|><|start_header_id|>user<|end_header_id|>

Hello!<|eot_id|><|start_header_id|>assistant<|end_header_id|>

Hi there!<|eot_id|>
```

> ⭐ LLaMA-3 uses **granular header tokens** (`<|start_header_id|>...<|end_header_id|>`) — each role gets its own labeled envelope. This is more flexible for adding new roles (e.g., `tool`, `ipython`).

## 🤖 ChatML Format (OpenAI)
```
<|im_start|>system
You are helpful.<|im_end|>
<|im_start|>user
Hello!<|im_end|>
<|im_start|>assistant
Hi there!<|im_end|>
```

> `im` stands for "input message." This format is simple, clean, and widely adopted by open-source models that mimic the OpenAI API.

### 📊 Template Comparison Table

| Model Family | Start Marker | End Marker | Role Encoding | Complexity |
|---|---|---|---|---|
| LLaMA-2 | `[INST]` | `[/INST]`, `</s>` | Implicit position | Medium |
| LLaMA-3 | `<\|start_header_id\|>` | `<\|eot_id\|>` | Explicit header | High |
| ChatML (OpenAI) | `<\|im_start\|>` | `<\|im_end\|>` | First line after start | Low |
| Mistral | `[INST]` | `[/INST]` | Similar to LLaMA-2 | Medium |

## 💻 Using Templates in Code
```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("meta-llama/Meta-Llama-3-8B-Instruct")

messages = [
    {"role": "system", "content": "You are a helpful AI."},
    {"role": "user", "content": "What is attention?"},
]

# Apply chat template
formatted = tokenizer.apply_chat_template(messages, tokenize=False)
print(formatted)

# Tokenize directly
input_ids = tokenizer.apply_chat_template(messages, return_tensors="pt")
```

> ⚠️ **Production Rule**: **NEVER** hand-format chat strings. Always use `tokenizer.apply_chat_template()`. One misplaced newline or missing special token can tank output quality silently — the model won't error, it'll just produce bad answers.

---

# 4️⃣ Building a Complete Token Pipeline 🏗️🔧

## 🧒 Beginner Analogy — The Assembly Line

Imagine a car factory 🏭:

1. **Raw material** arrives (steel, rubber, glass) → That's your **raw text**.
2. A machine **cuts and shapes** parts → That's the **tokenizer** splitting text into tokens.
3. Parts are **numbered** and put on a conveyor belt → That's **token IDs** in a sequence.
4. Parts are **grouped into batches** for the next station → That's **batching** for the GPU.
5. Short batches get **foam padding** so everything is the same size → That's **padding**.

The whole pipeline: **Raw Text → Tokenize → Numericalize → Batch → Pad → GPU** 🚀

## 🔄 End-to-End: Raw Text → Training-Ready Tensors

```python
import torch
from torch.utils.data import Dataset, DataLoader

class TextDataset(Dataset):
    """Dataset that tokenizes text and creates training sequences."""
    
    def __init__(self, text, tokenizer, max_length=512):
        self.tokenizer = tokenizer
        self.max_length = max_length
        
        # Tokenize entire corpus at once
        self.token_ids = tokenizer.encode(text)
        
        # Create overlapping sequences
        self.examples = []
        for i in range(0, len(self.token_ids) - max_length, max_length):
            input_ids = self.token_ids[i:i + max_length]
            # For language modeling: target = input shifted by 1
            self.examples.append(input_ids)
    
    def __len__(self):
        return len(self.examples)
    
    def __getitem__(self, idx):
        tokens = self.examples[idx]
        x = torch.tensor(tokens[:-1], dtype=torch.long)  # Input
        y = torch.tensor(tokens[1:], dtype=torch.long)    # Target (shifted by 1)
        return x, y

class PaddingCollator:
    """Collate function that pads sequences to same length."""
    
    def __init__(self, pad_token_id=0):
        self.pad_token_id = pad_token_id
    
    def __call__(self, batch):
        xs, ys = zip(*batch)
        
        # Find max length in batch
        max_len = max(len(x) for x in xs)
        
        # Pad sequences
        padded_x = torch.full((len(xs), max_len), self.pad_token_id, dtype=torch.long)
        padded_y = torch.full((len(ys), max_len), -100, dtype=torch.long)  # -100 = ignore in loss
        attention_mask = torch.zeros(len(xs), max_len, dtype=torch.long)
        
        for i, (x, y) in enumerate(zip(xs, ys)):
            padded_x[i, :len(x)] = x
            padded_y[i, :len(y)] = y
            attention_mask[i, :len(x)] = 1
        
        return {
            'input_ids': padded_x,
            'labels': padded_y,
            'attention_mask': attention_mask,
        }

# Usage
dataset = TextDataset(training_text, tokenizer, max_length=512)
dataloader = DataLoader(
    dataset,
    batch_size=8,
    shuffle=True,
    collate_fn=PaddingCollator(pad_token_id=tokenizer.pad_token_id)
)
```

### 🔬 Deep Dive: Why `-100` for Ignored Labels?

PyTorch's `CrossEntropyLoss` has a special parameter `ignore_index=-100` by default. Any label set to $-100$ is **excluded** from the loss calculation. This is how we tell the model:

- "Don't learn to predict padding tokens" (they're meaningless filler)
- "Don't learn to predict the system prompt" (we only want the model to learn the *assistant's* reply)

$$\mathcal{L} = -\frac{1}{N_{\text{valid}}} \sum_{i : y_i \neq -100} \log p(y_i \mid x_{\leq i})$$

where $N_{\text{valid}}$ is the count of non-ignored positions.

### 📊 Padding Strategy Comparison

| Strategy | Direction | Used For | Why |
|---|---|---|---|
| **Right-padding** | `[tok tok tok PAD PAD]` | Training | Standard; attention mask zeroes out padding |
| **Left-padding** | `[PAD PAD tok tok tok]` | Generation/Inference | Last token position is always real content → simpler next-token generation |

> ⭐ **Production Tip**: Mixing these up is a sneaky bug. If you right-pad during generation, the model's last "seen" token is a `<pad>`, and it generates garbage.

---

# 5️⃣ Token Embeddings: From IDs to Vectors 🔢➡️📐

## 🧒 Beginner Analogy — The Coat-Check System

Imagine a fancy theater's **coat check** 🎭:

1. You hand over your coat (raw token, like the word "hello").
2. The attendant gives you a **numbered ticket** (that's the token ID, e.g., `15339`).
3. When you return the ticket, you get back your coat — but instead of a physical coat, the model gets back a **rich mathematical vector** that encodes everything the model "knows" about that token.

The **embedding table** is the coat rack — it has $|V|$ hooks (one per token), and each hook holds a vector of dimension $d_{\text{model}}$.

$$\text{Embedding Table} \in \mathbb{R}^{|V| \times d_{\text{model}}}$$

### 🔢 Size Example

For a model with $|V| = 128{,}256$ and $d_{\text{model}} = 4{,}096$:

$$\text{Parameters} = 128{,}256 \times 4{,}096 = 525{,}352{,}960 \approx 525\text{M parameters}$$

That's **half a billion** parameters just for the dictionary lookup table! 🤯

```python
import torch.nn as nn

class TokenEmbedding(nn.Module):
    """Complete embedding layer: token + position."""
    
    def __init__(self, vocab_size, d_model, max_seq_len, pad_token_id=0):
        super().__init__()
        self.token_embedding = nn.Embedding(vocab_size, d_model, padding_idx=pad_token_id)
        self.position_embedding = nn.Embedding(max_seq_len, d_model)
        self.d_model = d_model
        self.dropout = nn.Dropout(0.1)
        
        # Scale embeddings by sqrt(d_model) (as in original Transformer)
        self.scale = d_model ** 0.5
    
    def forward(self, input_ids):
        seq_len = input_ids.shape[1]
        positions = torch.arange(seq_len, device=input_ids.device).unsqueeze(0)
        
        token_emb = self.token_embedding(input_ids) * self.scale
        pos_emb = self.position_embedding(positions)
        
        return self.dropout(token_emb + pos_emb)
```

### 🧮 Why Scale by $\sqrt{d_{\text{model}}}$?

The original Transformer paper multiplies token embeddings by $\sqrt{d_{\text{model}}}$ so that the magnitude of the token embeddings is comparable to the positional embeddings (which use sine/cosine values in $[-1, 1]$). Without scaling:

$$\text{If } d = 512, \text{ embedding values } \sim \mathcal{N}(0, 1) \Rightarrow \|e\| \approx \sqrt{512} \approx 22.6$$

After scaling: values are in a range where addition with positional encodings is balanced.

> ⭐ **Key Insight**: The embedding table is both the **first** layer (token → vector) and often the **last** layer (vector → token logits) of the model. Many models **tie** the input and output embedding weights to save parameters:
> $$\text{logits} = X \cdot E^{T} \quad \text{where } E \in \mathbb{R}^{|V| \times d_{\text{model}}}$$

---

# 6️⃣ Tokenizer Configuration Best Practices 🛠️✅

## 🧒 Beginner Analogy — Choosing the Right Dictionary for the Job

| Scenario | Dictionary Choice | Tokenizer Choice |
|---|---|---|
| 📖 Learning a brand-new language from scratch | Start with a blank dictionary, build it up | **Pre-training** — train tokenizer on your data |
| 📝 Learning slang for a language you already speak | Add new words to your existing dictionary | **Instruction tuning** — add chat tokens to base tokenizer |
| 🏥 Studying medicine after learning general English | Get a medical supplement to your dictionary | **Domain adaptation** — extend tokenizer with domain tokens |

## 🏗️ For Pre-training
- ✅ Train tokenizer on a **representative sample** of training data
- ✅ Use **byte-level BPE** for zero OOV (every byte can be represented)
- ✅ Vocab size: match your compute budget ($32\text{K}$–$128\text{K}$)
- ✅ Include **minimal** special tokens: `<bos>`, `<eos>`, `<pad>`
- ❌ Don't add chat tokens yet — the base model doesn't need them

## 💬 For Instruction Tuning
- ✅ Start from **base model's tokenizer** (don't retrain from scratch!)
- ✅ Add chat-specific special tokens (`<|user|>`, `<|assistant|>`, etc.)
- ✅ Resize embeddings and initialize new tokens
- ✅ Define a clear **chat template** (Jinja2 format in HuggingFace)
- ❌ Don't change existing token IDs — breaks all pre-trained knowledge

## 🧬 For Domain Adaptation

> 🧒 **Analogy**: You're a general-purpose translator who just got hired by a biotech company. You don't throw out your English dictionary — you *add* a medical terminology supplement on top.

```python
# Option 1: Add domain tokens to existing tokenizer
new_tokens = ["<protein>", "<molecule>", "ATCG", "pH7.4"]
tokenizer.add_tokens(new_tokens)
model.resize_token_embeddings(len(tokenizer))

# Option 2: Train new tokenizer, initialize from old
# Better for very different domains (code, chemistry, etc.)
```

### 🏭 Production Decision Matrix: When to Extend vs. Retrain

| Situation | Action | Risk |
|---|---|---|
| Adding 10–100 domain terms | **Extend** existing tokenizer | Low — minimal embedding disruption |
| Switching to a new language family | **Retrain** tokenizer from scratch | High — need full pre-training again |
| Adding code support to a text model | **Extend** with code tokens + fine-tune | Medium — need substantial code data |
| Building a completely new model | **Train fresh** tokenizer on your corpus | None — clean slate |

> ⭐ **Production Reality**: Most teams **never** train a tokenizer from scratch. They pick an existing model family (LLaMA, Mistral, etc.) and extend its tokenizer with a handful of domain-specific tokens. Full tokenizer retraining means full pre-training — that's millions of dollars 💰.

---

# 7️⃣ Common Tokenization Bugs & Fixes 🐛🔧

> 🧒 **Analogy**: These are like the "check engine" lights on your car dashboard. Ignoring them won't crash your car *immediately*, but you'll get worse gas mileage (model quality) until you fix them — and sometimes the car *will* break down at the worst time (production failure).

## 🐛 Bug 1: Padding Token Not Set
```python
# LLaMA has no pad token by default!
tokenizer.pad_token = tokenizer.eos_token  # Quick fix
# OR
tokenizer.add_special_tokens({"pad_token": "<pad>"})
model.resize_token_embeddings(len(tokenizer))
```

> 💡 **Why this happens**: LLaMA was designed for **continuous generation**, not batched training. Its creators never needed padding, so they didn't set one. When you fine-tune, you *do* need it.

## 🐛 Bug 2: Chat Template Mismatch
```python
# Wrong: using LLaMA-2 format with LLaMA-3 model
# This will produce garbage outputs!
# Always use: tokenizer.apply_chat_template()
```

> ⭐ **Why it's dangerous**: The model doesn't "error" — it silently produces lower-quality outputs. You might not notice until users start complaining. This is a *silent killer* in production.

## 🐛 Bug 3: Off-by-One in Causal LM Labels

In language modeling, the target for position $i$ is the token at position $i+1$ (the model predicts the *next* token):

$$\text{target}_i = \text{input}_{i+1} \quad \forall\, i \in [0, n-2]$$

```python
# WRONG: labels = input_ids (predicting current token from current token)
# RIGHT: labels = input_ids shifted by 1
labels = input_ids[:, 1:]
logits = logits[:, :-1, :]
```

> 🧒 **Analogy**: Imagine a fill-in-the-blank test where the answer key lines up with the *wrong* questions. Every answer is correct — but for the wrong question. That's what happens with off-by-one errors.

## 🐛 Bug 4: Not Masking Pad Tokens in Loss

$$\mathcal{L} = -\frac{1}{N} \sum_{i} \log p(y_i \mid x_{\leq i}) \quad \text{where } y_i \neq \text{PAD}$$

```python
# Use -100 to exclude pad tokens from cross-entropy loss
labels[labels == pad_token_id] = -100
loss = F.cross_entropy(logits.view(-1, vocab_size), labels.view(-1), ignore_index=-100)
```

> 💡 **Why it matters**: If you include padding in the loss, the model spends training time learning to predict padding tokens — which is completely useless. Worse, it dilutes the gradient signal from the actual content tokens.

### 📊 Bug Severity & Detection Cheat Sheet

| Bug | Severity | Silent? | How to Detect |
|---|---|---|---|
| No pad token | 🔴 Crash | ❌ Immediate error | Error message at batch time |
| Template mismatch | 🟠 Degraded quality | ✅ **Yes** — no error! | Compare outputs with gold template |
| Off-by-one labels | 🔴 Won't learn | ✅ **Yes** — loss stays high | Validation loss doesn't decrease |
| Unmasked pad loss | 🟡 Slow training | ✅ **Yes** | Compare loss with/without masking |

---

# 8️⃣ Tokenizer Serialization & Sharing 💾🌐

## 🧒 Beginner Analogy — Packing & Mailing Your Dictionary

Once you've built the perfect dictionary (tokenizer), you need to:
1. 📦 **Pack it** into a box (save to files)
2. 📬 **Mail it** to anyone who needs it (push to the Hub)
3. 📖 **Unpack it** at the destination (load from files)

If someone gets a *different* dictionary than the one you trained with, every token ID maps to the wrong word — the model produces total gibberish. 🗑️

```python
# Save tokenizer
tokenizer.save_pretrained("./my_tokenizer/")

# Creates:
# - tokenizer.json (fast tokenizer, BPE merges + vocab)
# - tokenizer_config.json (settings)
# - special_tokens_map.json

# Load tokenizer
tokenizer = AutoTokenizer.from_pretrained("./my_tokenizer/")

# Share on HuggingFace Hub
tokenizer.push_to_hub("username/my-tokenizer")
```

### 📋 What's Inside Each File?

| File | Contents | Size (typical) |
|---|---|---|
| `tokenizer.json` | Full vocab, merge rules, byte-to-token mappings | 2–15 MB |
| `tokenizer_config.json` | Chat template (Jinja2), special token settings, `model_max_length` | 1–5 KB |
| `special_tokens_map.json` | Mapping of role → token string (`bos_token: "<s>"`) | <1 KB |

> ⭐ **Production Rule**: **Always** version-control your tokenizer alongside your model checkpoint. A model without its exact tokenizer is useless. Many production outages have been caused by deploying a model with a *slightly* different tokenizer version.

---

# 📝 Summary

| Decision | Recommendation |
|----------|---------------|
| Vocab size | 32K (English), 128K (multilingual) |
| Algorithm | Byte-level BPE (modern standard) |
| Special tokens | Minimal for base, chat tokens for instruct |
| Chat template | Use model's built-in, never hand-format |
| Padding | Left-pad for generation, right-pad for training |
| Label masking | -100 for pad/prompt tokens in loss |

---

# 🔬 Deep Research Corner: Advanced Vocabulary Topics 🧪📚

## 📖 Zipf's Law in NLP — The Power Law of Language

Zipf's Law states that in any large enough corpus, the frequency of a word is inversely proportional to its rank:

$$f(r) = \frac{C}{r^{\alpha}}, \qquad \alpha \approx 1.0$$

This means:
- The **1st** most common word appears ~$C$ times
- The **2nd** most common word appears ~$\frac{C}{2}$ times
- The **1000th** most common word appears ~$\frac{C}{1000}$ times

**Implications for vocabulary design**:
- The top $\sim 5{,}000$ tokens cover $\sim 95\%$ of text
- The bottom $\sim 50\%$ of a $100\text{K}$ vocab each appear fewer than $\sim 50$ times in a typical $1\text{T}$ token corpus
- Those rare tokens have **extremely noisy embeddings** — they basically act as random vectors
- This is why models like LLaMA-2 chose a "conservative" $32\text{K}$ — every token actually gets enough training signal

## 🧮 Vocabulary Selection Algorithms

How do you decide which merges to keep?

| Algorithm | Strategy | Used By |
|---|---|---|
| **BPE** (Byte Pair Encoding) | Greedily merge the most frequent pair of adjacent tokens | GPT-2, GPT-3, GPT-4 |
| **Unigram LM** | Start with a huge vocab, prune tokens with low marginal likelihood | T5, ALBERT |
| **WordPiece** | Like BPE but maximizes likelihood of training data | BERT |
| **Byte-level BPE** | BPE on raw bytes (256 base tokens) — zero OOV | GPT-2+, LLaMA, Mistral |

### Byte-Level Fallback Strategy

Modern tokenizers (GPT-2 onward) operate on **bytes** rather than Unicode characters:

$$\text{Base vocabulary} = \{0\text{x00}, 0\text{x01}, \ldots, 0\text{xFF}\} \quad (256 \text{ tokens})$$

Any text can be encoded — worst case, each character becomes 1–4 byte tokens. This gives $100\%$ coverage with a **graceful degradation** curve: common words get single tokens, rare words get broken into subwords, and truly alien text (emojis, rare scripts) gets byte-by-byte encoding.

## 🌍 Multilingual Vocabulary Challenges

| Challenge | Description | Mitigation |
|---|---|---|
| **Fertility imbalance** | Korean needs 2–3× more tokens than English for the same content | Allocate more merges to high-fertility languages |
| **Script overlap** | Chinese, Japanese, Korean share some characters | Shared CJK tokens reduce waste |
| **Latin bias** | BPE trained on English-heavy data over-fragments non-Latin scripts | Balance training corpus by language |
| **Code-switching** | Multilingual text mixes languages mid-sentence | Ensure common cross-language tokens exist |

The **"multilingual tax"** in numbers:

$$\text{Effective English vocab} = \frac{|V|_{\text{total}}}{\text{number of supported languages}} \times \text{language weight}$$

For a $128\text{K}$ vocab supporting $20$ languages with equal weight:

$$\text{Effective English vocab} \approx \frac{128{,}000}{20} = 6{,}400 \text{ tokens}$$

That's *far* less than a $32\text{K}$ English-only model gets! This is why multilingual models need larger vocabularies.

---

# 🧾 CHEAT SHEET — Vocabulary Design & Special Tokens 🏁

## 📐 Core Formulas

| Formula | Expression | What It Means |
|---|---|---|
| Embedding matrix size | $|V| \times d_{\text{model}}$ | Rows = vocabulary size, Columns = embedding dimension |
| Embedding memory | $|V| \times d \times \text{bytes\_per\_param}$ | RAM/VRAM consumed by the embedding table |
| Vocabulary coverage | $\frac{\text{tokens\_known}}{\text{tokens\_total}}$ | What fraction of text the vocab recognizes (100% with byte-level BPE) |
| Zipf's Law | $f(r) \propto \frac{1}{r^{\alpha}}$ | Word frequency is inversely proportional to rank |
| Attention cost | $O(T(|V|)^{2})$ | Shorter sequences (larger vocab) → cheaper attention |
| Loss with masking | $\mathcal{L} = -\frac{1}{N}\sum_{i:y_i \neq -100} \log p(y_i)$ | Ignore padded/masked positions in loss |

## ⚡ Quick Decision Guide

```
Q: What vocab size?
├─ English only?          → 32K
├─ + a few languages?     → 64–128K
├─ Universal / 100+ langs → 256K+
└─ Code-heavy?            → 50–100K

Q: Which tokenizer algorithm?
└─ Almost always: Byte-level BPE (zero OOV, battle-tested)

Q: What special tokens do I need?
├─ Pre-training:  <bos>, <eos>, <pad>
├─ Chat fine-tune: + <|user|>, <|assistant|>, <|system|>
└─ Tool use:       + <|tool_call|>, <|tool_result|>

Q: How to handle domain adaptation?
├─ Few terms (~100)?  → tokenizer.add_tokens() + resize
└─ Whole new domain?  → Retrain tokenizer + full pre-train 💸
```

## 🏭 Production Numbers at a Glance

| Model | Vocab Size | $d_{\text{model}}$ | Embedding Params | Embedding Memory (fp16) |
|---|---|---|---|---|
| GPT-2 | 50,257 | 768 | ~38.6 M | ~77 MB |
| BERT-base | 30,522 | 768 | ~23.4 M | ~47 MB |
| LLaMA-2 7B | 32,000 | 4,096 | ~131 M | ~262 MB |
| LLaMA-3 8B | 128,256 | 4,096 | ~525 M | ~1.0 GB |
| GPT-4 (est.) | ~100,000 | ~12,288 | ~1.2 B | ~2.4 GB |
| Gemma | 256,000 | 3,072 | ~786 M | ~1.6 GB |

## 🐛 Top 4 Bugs to Avoid

| # | Bug | Fix | One-Liner |
|---|---|---|---|
| 1 | No pad token set | `tokenizer.pad_token = tokenizer.eos_token` | LLaMA doesn't ship with one |
| 2 | Wrong chat template | Always use `apply_chat_template()` | Silent quality killer |
| 3 | Off-by-one labels | `labels = input_ids[:, 1:]` | Predict *next* token, not *current* |
| 4 | Pad tokens in loss | `labels[labels == pad_id] = -100` | Don't train on filler |

## 🗺️ Vocabulary Design Mental Map

```
Raw Text
  │
  ▼
┌──────────────────────────┐
│  TOKENIZER (BPE/Unigram) │ ← Vocabulary lives here
│  Vocab: |V| tokens       │    (32K – 256K entries)
│  Special: <bos>,<eos>... │
└──────────┬───────────────┘
           │ token IDs
           ▼
┌──────────────────────────┐
│  EMBEDDING TABLE         │
│  Size: |V| × d_model     │ ← First (and sometimes last)
│  ~100M – 1B+ params      │    layer of the model
└──────────┬───────────────┘
           │ dense vectors
           ▼
┌──────────────────────────┐
│  TRANSFORMER LAYERS      │
│  Attention + FFN × N     │
└──────────┬───────────────┘
           │ hidden states
           ▼
┌──────────────────────────┐
│  OUTPUT HEAD (LM Head)   │
│  Often shares weights    │ ← "Weight tying" with
│  with embedding table    │    embedding table
└──────────┬───────────────┘
           │ logits over |V|
           ▼
       Next Token
```

---

**Tomorrow**: Positional Encoding — Sinusoidal, Learned, and RoPE approaches. 🔜
