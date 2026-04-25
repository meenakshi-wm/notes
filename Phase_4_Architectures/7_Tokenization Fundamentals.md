📘 DAY 7 — Tokenization Fundamentals: How Text Becomes Numbers 🔤➡️🔢

---

> 🧠 **Big Picture Analogy**: Imagine you're sending a message to an alien 👽 who only understands numbered puzzle pieces. You can't just hand them a sentence — you first need to **chop it into pieces** they recognize, then **convert each piece to a number**. That chopping process? That's **tokenization**. It's literally how AI reads.

---

## 🎯 Goal

Understand tokenization not as "splitting text into words," but as the **critical information-theoretic bridge** between raw text and mathematical representations that neural networks can process. Master **WHY** subword tokenization won, **HOW** different algorithms work, and their profound impact on model performance.

## ✅ If This Is Deeply Understood

You'll understand why LLMs sometimes struggle with simple arithmetic 🧮, why multilingual models have uneven performance 🌍, and how tokenization choices directly affect training efficiency, model capacity, and inference cost 💰.

---

> ⭐ **Why Should I Care?** Tokenization is the **very first step** in how any language AI processes text. Get it wrong, and everything downstream — understanding, generation, translation — suffers. It's like trying to cook with badly chopped ingredients 🔪🥕 — the recipe can be perfect, but the result will be a mess.

---

# 1️⃣ Why Tokenization Matters More Than You Think 🤯

## The Fundamental Problem

Neural networks operate on **numbers** (tensors).
Language is **sequences of characters**.
We need a mapping:

$$\text{text} \xrightarrow{\text{tokenize}} \text{integers} \xrightarrow{\text{embed}} \text{vectors} \xrightarrow{\text{process}} \text{model output}$$

> 🎯 **Beginner Analogy**: Think of the tokenizer as a **translator at a border checkpoint** 🛂. The sentence is a foreign traveler, and the model is a country that only accepts people with numeric ID cards. The tokenizer assigns every piece of the sentence an ID number so the model can let it in.

⭐ **The tokenizer IS the model's perception of language.** If the tokenizer is bad, the model is essentially wearing the wrong prescription glasses 👓 — it simply can't see language clearly.

A bad tokenizer can:
- 🗑️ **Waste model capacity** on common patterns
- 🔨 **Fragment rare words** into too many pieces (hurting understanding)
- 🔄 **Create different token counts** for semantically equivalent text
- 🧮 **Make arithmetic nearly impossible** (each digit might be a separate token!)

---

## Three Approaches (Historical → Modern) 📊

### 📚 Approach 1: Word-Level Tokenization

```
"I love machine learning" → ["I", "love", "machine", "learning"]
```

**Vocabulary**: every unique word in the language.

> 🧩 **Analogy**: This is like having a **pre-printed jigsaw puzzle** where every piece is a whole word. Works great if you have all the words... but what happens when someone says a word you've never seen? 😱

**Problems:**
- 🚫 **OOV (Out of Vocabulary)**: "unforgettable" → `<UNK>` ("I've NEVER seen this word before!")
- 📖 **Huge vocabulary**: English has 170,000+ words. With morphology: **millions**.
- 🧱 **No morphological sharing**: "run", "running", "runner" are **completely separate** tokens — the model has no idea they're related!
- 💾 **Memory explosion**: each token needs an embedding vector (~768–4096 dims):

$$500{,}000 \text{ words} \times 4096 \text{ dims} \times 4 \text{ bytes} = \mathbf{8 \text{ GB}}$$

just for the embedding table! 🤯

### 🔡 Approach 2: Character-Level Tokenization

```
"hello" → ["h", "e", "l", "l", "o"]
```

**Vocabulary size**: ~256 (ASCII) or ~65K (Unicode)

> 🧩 **Analogy**: This is like chopping a pizza into **crumb-sized pieces** 🍕. Sure, you can represent ANY pizza this way, but good luck eating it — there are way too many tiny pieces and none of them taste like anything on their own!

**Problems:**
- 📏 **Sequence length explosion**: a 100-word sentence becomes ~500 characters
- ⏱️ **Attention is** $O(n^2)$: 5x longer sequence = **25x more compute!**
- 🧠 **Lost semantic chunking**: "un" + "break" + "able" vs "u" + "n" + "b" + "r" + "e" + ...
- 🔇 Characters don't carry meaning individually

### 🏆 Approach 3: Subword Tokenization (The Winner!) ✓

```
"unforgettable" → ["un", "forget", "table"]  or  ["un", "for", "get", "table"]
```

> 🧩 **Analogy**: This is like using **LEGO bricks** 🧱! You have a smart set of building blocks — some small ("un-", "-ing", "-ed"), some big ("forget", "table"). Common words stay whole, rare words get built from smaller, reusable pieces. Brilliant! ✨

**Why this wins:**
- 📦 **Compact vocabulary** (32K–100K tokens)
- ✅ **No OOV**: any word can be decomposed into known subwords
- 🔗 **Morphological sharing**: "un-" prefix shared across "unable", "unfair", "undo"
- ⚖️ **Balanced sequence length**: shorter than characters, longer than words

| Approach | Vocab Size | Handles New Words? | Sequence Length | Meaning per Token |
|---|---|---|---|---|
| 📚 Word-Level | 100K–500K+ | ❌ No (`<UNK>`) | Short ✅ | High ✅ |
| 🔡 Character-Level | ~256 | ✅ Yes | Very Long ❌ | Very Low ❌ |
| 🏆 **Subword** | **32K–100K** | **✅ Yes** | **Medium ✅** | **Medium–High ✅** |

---

# 2️⃣ Byte Pair Encoding (BPE) — The Dominant Algorithm 👑

> 🧩 **Beginner Analogy**: Imagine you're a librarian 📚 who notices people always borrow "Harry" and "Potter" together. So you tape those two cards together into one "HarryPotter" card. Then you notice "HarryPotter" and "Book" always appear together... and you keep combining the most popular pairs. That's BPE — **merging the most popular neighbors, over and over!**

## Origin: Data Compression (1994) 📜

Philip Gage invented BPE as a **compression algorithm**.
**Sennrich et al. (2016)** adapted it for Neural Machine Translation (NMT) — a landmark paper! 📄
GPT-2, GPT-3, GPT-4, LLaMA **all** use variants of BPE.

> ⭐ **Key Insight**: BPE was born to make files smaller, but it turned out to be the perfect way to build vocabularies for language models. One of AI's best "borrowed ideas"! 🎁

## Algorithm: Bottom-Up Merging 🔄

**Start with**: individual characters (or bytes)
**Repeatedly**: merge the most frequent adjacent pair

> 🏗️ **Think of it like building with blocks**: You start with individual letter blocks. You keep gluing together the pair of blocks that sit next to each other most often. Eventually you have bigger and bigger blocks — some becoming whole words!

### Step-by-Step Example 📝

Training corpus: `"low low low low low lower lower newest newest newest widest"`

**Step 0**: Character vocabulary
```
{'l', 'o', 'w', 'e', 'r', 'n', 's', 't', 'i', 'd', ' ', ...}
```

**Step 1**: Count adjacent pairs
```
('l', 'o'): 7    ← 🏆 Most frequent!
('o', 'w'): 7
('w', 'e'): 6
('e', 'r'): 2
('e', 's'): 4
('s', 't'): 4
('n', 'e'): 3
('e', 'w'): 3
```

**Step 2**: Merge most frequent pair: `('l', 'o')` → `'lo'` ✂️➡️🔗
New vocab: `{..., 'lo'}`
Corpus updates: every `l o` becomes `lo`

**Step 3**: Merge `('lo', 'w')` → `'low'`
**Step 4**: Merge `('e', 's')` → `'es'`
**Step 5**: Merge `('es', 't')` → `'est'`
**Step 6**: Merge `('n', 'e')` → `'ne'`
**Step 7**: Merge `('ne', 'w')` → `'new'`
**Step 8**: Merge `('new', 'est')` → `'newest'`
...

🔁 **Continue until desired vocabulary size reached.**

### ⭐ Key Properties of BPE

| Property | What It Means | Analogy |
|---|---|---|
| 🔒 **Deterministic** | Same corpus + same merge count = same result | Same recipe, same cake 🎂 |
| 📊 **Frequency-driven** | Common patterns get merged first | Popular songs get radio play 📻 |
| 🏃 **Greedy** | Locally optimal merges (not globally optimal) | Taking the nearest exit, not the best one 🚗 |
| ➡️ **No backtracking** | Once merged, never un-merged | Glued LEGO — can't un-glue! 🧱 |

## BPE Encoding (Inference Time) ⚡

Given trained merge list, encode new text:
1. Split into characters
2. Apply merges in **priority order** (most frequent first)

```
"lowest" → ['l','o','w','e','s','t']
        → ['lo','w','e','s','t']      (merge #1: l+o)
        → ['low','e','s','t']          (merge #2: lo+w)
        → ['low','e','st']             (merge #4: e+s? No — s+t)
        → ['low','est']               (merge #5: es+t)
```

> 💡 **Key**: The merge list acts like a **priority queue**. The model doesn't re-count frequencies — it just applies the rules it learned during training, in order.

---

# 3️⃣ WordPiece — BERT's Tokenizer 🧩

> 🧩 **Beginner Analogy**: If BPE is like merging the most **popular** couples at a dance 💃🕺, WordPiece is like merging the most **surprising** couples — pairs of people who are almost NEVER seen apart. It's not about popularity, it's about "these two practically live together!"

## ⭐ Key Difference from BPE

| | BPE | WordPiece |
|---|---|---|
| Merges by... | Most **FREQUENT** pair | Pair that maximizes **LIKELIHOOD** of data |

**WordPiece merge score:**

$$\text{Score}(a, b) = \frac{\text{freq}(ab)}{\text{freq}(a) \times \text{freq}(b)}$$

> 💡 **What this means**: Prefer merging pairs where the two pieces **rarely appear independently**. If "un" and "able" almost ALWAYS appear together as "unable," merge them — even if they're not the most frequent pair overall.

## The "##" Prefix Convention 🏷️

WordPiece marks **continuation tokens** with `##`:

```
"unbelievable" → ["un", "##believ", "##able"]
```

> 🎯 The `##` tells the model: **"this token continues the previous word"** — like a hyphen that says "I'm not a new word, I'm still part of the last one!"

## WordPiece vs BPE in Practice 📊

| Feature | BPE 🔵 | WordPiece 🟢 |
|---------|---------|-----------|
| **Used by** | GPT, LLaMA, Mistral | BERT, DistilBERT |
| **Merge criterion** | Frequency count | Likelihood ratio |
| **Subword marker** | None (spaces encoded) | `##` prefix |
| **Implementation** | Relatively simple | More complex |
| **Philosophy** | "What pairs appear most?" | "What pairs are most dependent?" |

---

# 4️⃣ SentencePiece — Language-Agnostic Tokenization 🌍

> 🧩 **Beginner Analogy**: BPE and WordPiece are like English-speaking librarians — they sort books by splitting titles at spaces. But what about Chinese 🇨🇳, Japanese 🇯🇵, or Thai 🇹🇭 books where there ARE no spaces? SentencePiece is the **multilingual librarian** who doesn't care about spaces — it just looks at the raw characters!

## The Problem with Word-Based Preprocessing ❌

BPE and WordPiece assume **PRE-TOKENIZED text** (split by spaces).
But many languages (Chinese, Japanese, Thai) **don't use spaces!**

```
English:  "I love cats"     → Words are obvious (split by spaces)
Chinese:  "我喜欢猫"        → Where do words begin and end? 🤷
Japanese: "私は猫が好きです"  → Same problem!
Thai:     "ฉันรักแมว"       → And here too!
```

## SentencePiece Solution ✅

Treat the input as a **RAW byte stream**.
No language-specific preprocessing needed.
Spaces are treated as regular characters (replaced with `▁`).

```
"Hello world" → "▁Hello ▁world" → ["▁He", "llo", "▁world"]
```

> 💡 The `▁` is a **regular token** — it represents "a space came before this." This way, **spaces are data, not delimiters!**

⭐ **Used by**: LLaMA, T5, mBART, ALBERT, XLNet — basically any model that needs to work across languages!

## Two Modes in SentencePiece 🔀

### Mode 1: BPE Mode
Applies BPE on raw byte/char stream (same algorithm, different input)

### Mode 2: Unigram Language Model (Unique to SentencePiece!) 🌟

This is the opposite approach to BPE:

| | BPE | Unigram |
|---|---|---|
| **Starts with** | Small vocab (characters) | LARGE vocab (all substrings) |
| **Process** | Adds tokens by merging | **Removes** tokens that aren't useful |
| **Direction** | Bottom-up 🔼 | Top-down 🔽 |

### How Unigram Works 📐

Start with a **LARGE** vocabulary (all substrings up to some length).
Iteratively **REMOVE** tokens that least affect the likelihood.

**Probability of a sentence** (assumes tokens are independent):

$$P(\text{sentence}) = \prod_{i=1}^{n} P(x_i)$$

For each token $x_i$:

$$P(x_i) = \frac{\text{count}(x_i)}{\sum_j \text{count}(x_j)}$$

**Remove tokens** whose removal causes the **smallest decrease** in total log-likelihood:

$$\mathcal{L} = \sum_{\text{sentence}} \log P(\text{sentence})$$

> 🎯 The result: a vocabulary where **every token is "earning its place"** — like a sports team where every player contributes, no bench warmers! 🏀

---

# 5️⃣ Byte-Level BPE (Modern Standard) 🚀

## Used by: GPT-2, GPT-3, GPT-4, LLaMA-2/3 🏭

> 🧩 **Beginner Analogy**: Regular BPE starts with letters (a, b, c...). Byte-level BPE goes even deeper — it starts with the **raw bytes** (the 0s and 1s the computer actually stores). It's like instead of working with pre-cut puzzle pieces, you're working with the raw cardboard itself. You can make ANY shape piece! 🧩

Instead of characters, start from **BYTES** (0–255).

**Why?**
- ✅ **Complete coverage**: ANY text can be represented — **no `<UNK>` EVER!**
- 🌐 **Handles anything**: any language, emoji 😀, code `{}`, even binary
- 📦 **Vocabulary starts at 256** (byte values) instead of 65K+ (Unicode codepoints)

**Process:**
1. Encode text as UTF-8 bytes
2. Run BPE on byte sequences
3. Merged byte patterns become tokens

**Example:**
```
"café" in UTF-8: [99, 97, 102, 195, 169]
                   c   a    f    é (2 bytes)
After BPE: might merge [195, 169] → single token for "é"
```

> ⭐ **Why this is the modern standard**: It's the most universal approach. No matter what you throw at it — Hindi, emoji, Python code, mathematical symbols — byte-level BPE can handle it. That's why all the GPT models use it! 💪

## GPT-2 Trick: Regex Pre-Splitting 🧠

Before BPE, split text with regex:
```python
pat = r"""'s|'t|'re|'ve|'m|'ll|'d| ?\p{L}+| ?\p{N}+| ?[^\s\p{L}\p{N}]+|\s+(?!\S)|\s+"""
```

This **prevents merges across word boundaries**:
```
"don't" → ["don", "'t"]   ✅ (not "do", "n't")
```

> 💡 Without this trick, BPE might merge characters across unrelated words, creating tokens like `"e t"` (end of "the" + start of "tree"). The regex keeps things sensible!

---

# 6️⃣ Special Tokens — Control Signals for the Model 🚦

> 🧩 **Beginner Analogy**: Think of special tokens as **road signs** 🛣️ for the model. Just like a driver needs "STOP" 🛑, "YIELD" ⚠️, and "ONE WAY" ➡️ signs to navigate, the model needs special markers to know where sentences begin, end, what's padding, and what structure to expect. These signs carry no "language meaning" — they're purely **navigational**.

Every tokenizer includes special tokens:

| Token | Purpose | Used by | Analogy |
|-------|---------|---------|---------|
| `[PAD]` | Padding sequences to equal length | BERT | Empty seats on a bus 🚌 (filling space) |
| `[CLS]` | Classification token (sentence representation) | BERT | The "summary card" 📋 at the front |
| `[SEP]` | Separator between segments | BERT | A wall between rooms 🧱 |
| `[MASK]` | Masked token for MLM training | BERT | A blank in a fill-in-the-blank quiz _____ |
| `<\|endoftext\|>` | End of document | GPT-2 | "THE END" at a movie 🎬 |
| `<bos>` | Beginning of sequence | LLaMA | "Once upon a time..." 📖 |
| `<eos>` | End of sequence | LLaMA | "...happily ever after." 📖 |
| `[INST]` | Instruction start | LLaMA-chat | "Dear AI, please do this:" ✉️ |

> ⭐ **Key Point**: These tokens have **dedicated embedding vectors**. They carry **NO linguistic meaning** — they are purely structural/control signals. The model LEARNS what they mean during training!

### 🔍 Why Special Tokens Matter in Production

```
Input:   "What is AI?"
BERT:    [CLS] What is AI ? [SEP]              → 6 tokens (2 are special!)
GPT-2:   What is AI ? <|endoftext|>             → 5 tokens
LLaMA:   <bos> What is AI ? <eos>              → 6 tokens
```

> 💡 **Production Gotcha**: If you forget to add special tokens during inference but they were present during training, your model's predictions will be **garbage**! Always match the training format. 🎯

---

# 7️⃣ Vocabulary Size Trade-offs ⚖️

> 🧩 **Beginner Analogy**: Vocabulary size is like choosing the **size of your dictionary** 📖. A tiny dictionary (8K words) is lightweight but you'll need lots of lookups for complex words. A massive dictionary (256K words) has every word you could ever need, but it weighs a ton and most pages are barely used!

## ⭐ The Core Formula: Embedding Parameters

$$\text{Embedding Parameters} = V \times d_{\text{model}}$$

Where:
- $V$ = vocabulary size (number of unique tokens)
- $d_{\text{model}}$ = embedding dimension (typically 768–4096)

| Vocab Size ($V$) | $d_{\text{model}}$ | Embedding Parameters | Memory (FP32) |
|---|---|---|---|
| 32,000 | 4,096 | 131M | ~524 MB |
| 100,000 | 4,096 | 410M | ~1.6 GB |
| 128,256 | 4,096 | 525M | ~2.1 GB |

> 💡 Those embedding parameters are a **fixed cost** you pay regardless of input length. Bigger vocab = bigger "dictionary" the model always carries!

## 📦 Small Vocabulary (8K–16K tokens)

| ✅ Pros | ❌ Cons |
|---|---|
| Smaller embedding table | Longer sequences (more tokens per text) |
| Better subword sharing | More computation (attention is $O(n^2)$!) |
| Fewer undertrained tokens | Less "meaning per token" |

## 📚 Large Vocabulary (64K–256K tokens)

| ✅ Pros | ❌ Cons |
|---|---|
| Shorter sequences | Huge embedding table (memory) |
| More whole words → better meaning per token | Rare tokens undertrained |
| Faster inference (fewer steps) | Less subword sharing |

## 🏭 Production Choices: What Real Models Use

| Model | Vocab Size | Tokenizer Type | Why This Choice? |
|---|---|---|---|
| GPT-2 | 50,257 | Byte-level BPE | Balanced for English text |
| GPT-4 | ~100,000 | Byte-level BPE (`cl100k_base`) | Better multilingual + code |
| BERT | 30,522 | WordPiece | Sufficient for English NLU |
| LLaMA-2 | 32,000 | SentencePiece | Compact, efficient |
| LLaMA-3 | 128,256 | SentencePiece BPE | **Massive** — supports 30+ languages! 🌍 |

> ⭐ **Trend**: Vocabulary sizes are **growing** over time. LLaMA went from 32K → 128K because multilingual support demands more tokens. GPT went from 50K → 100K for the same reason.

---

# 8️⃣ The Tokenization → Model Performance Connection 🔗

## Why LLMs Struggle with Math 🧮

> 🧩 **Beginner Analogy**: Imagine trying to add 1234 + 5678, but someone ripped the numbers apart: "12" and "34" on one card, "56" and "78" on another. You'd have to mentally re-assemble them before you could even start adding! That's what the model has to do. 😰

```
"1234 + 5678" might tokenize as:
["12", "34", " +", " 56", "78"]
```

⭐ **The model never sees "1234" as a single number!**
It must **learn multi-digit arithmetic from fragmented representations** — like trying to do math with torn-up flash cards! 🃏

> 🔬 **Deep Research**: This is an active area of research! Some approaches include:
> - Training with number-aware tokenization
> - Adding calculator tools to LLMs
> - Representing numbers digit-by-digit with positional encoding

## Why Multilingual Models Have Uneven Quality 🌍

```
English: "hello"     = 1 token   ✅ Efficient!
Hindi:   "नमस्ते"    = 3-4 tokens 😐
Thai:    "สวัสดี"    = 5-6 tokens 😟
```

**Same meaning, vastly different token counts.** This means:
- ⏱️ **More compute** for non-English languages (more tokens to process)
- 📏 **Less effective context window** for non-English text (8K tokens of English ≠ 8K tokens of Thai)
- 📊 **Training data imbalance amplified** by tokenization

> 💡 This is why GPT-4 is "smarter" in English than in Thai — tokenization gives English an unfair head start! 🏃‍♂️

## ⭐ Fertility: Tokens per Word — The Fairness Metric 📏

$$\text{Fertility} = \frac{\text{total tokens}}{\text{total words}}$$

> 🧩 **Analogy**: Fertility measures how many "puzzle pieces" each word gets broken into. Lower = better (fewer pieces = more efficient processing).

| Language | Tokenizer | Fertility (tokens/word) | Meaning |
|---|---|---|---|
| 🇺🇸 English | GPT-4 | ~1.3 | Almost 1 token = 1 word ✅ |
| 🇨🇳 Chinese | GPT-4 | ~2.0 | Each character ≈ 2 tokens |
| 🇮🇳 Hindi | GPT-4 | ~3.0 | Complex script, very fragmented ❌ |
| 🇯🇵 Japanese | GPT-4 | ~2.5 | Mix of scripts adds complexity |

## ⭐ Compression Ratio: Characters per Token 📊

$$\text{Compression Ratio} = \frac{\text{total characters}}{\text{total tokens}}$$

| Tokenizer | Compression Ratio (English) | Meaning |
|---|---|---|
| GPT-2 (50K vocab) | ~3.5 chars/token | Each token ≈ 3.5 characters |
| GPT-4 (100K vocab) | ~4.0 chars/token | Larger vocab → more compression |
| LLaMA-3 (128K vocab) | ~4.2 chars/token | Best compression |

> 💡 **Higher compression = fewer tokens = lower API costs = more context fits in the window!** This is why vocab size keeps growing. 💰

---

# 9️⃣ Practical Implementation: BPE from Scratch 💻

> 🎯 **Why build it yourself?** Understanding the algorithm by reading about it is one thing. Building it yourself makes you truly GET it. This is like learning to drive — reading the manual helps, but nothing beats sitting behind the wheel! 🚗

```python
import re
from collections import Counter

def get_stats(vocab):
    """Count frequency of adjacent pairs in vocabulary. 📊"""
    pairs = Counter()
    for word, freq in vocab.items():
        symbols = word.split()
        for i in range(len(symbols) - 1):
            pairs[(symbols[i], symbols[i+1])] += freq
    return pairs

def merge_vocab(pair, vocab):
    """Merge all occurrences of the most frequent pair. 🔗"""
    new_vocab = {}
    bigram = ' '.join(pair)
    replacement = ''.join(pair)
    for word, freq in vocab.items():
        new_word = word.replace(bigram, replacement)
        new_vocab[new_word] = freq
    return new_vocab

def train_bpe(text, num_merges=100):
    """Train BPE tokenizer on text. 🏋️"""
    # Step 1: Get word frequencies
    words = text.strip().split()
    word_freqs = Counter(words)
    
    # Step 2: Initialize vocabulary as character sequences
    vocab = {}
    for word, freq in word_freqs.items():
        # Add end-of-word token
        char_seq = ' '.join(list(word)) + ' </w>'
        vocab[char_seq] = freq
    
    # Step 3: Iteratively merge most frequent pairs
    merges = []
    for i in range(num_merges):
        pairs = get_stats(vocab)
        if not pairs:
            break
        best_pair = max(pairs, key=pairs.get)
        vocab = merge_vocab(best_pair, vocab)
        merges.append(best_pair)
        print(f"Merge {i+1}: {best_pair} → {''.join(best_pair)} (freq: {pairs[best_pair]})")
    
    return vocab, merges

# Train on sample text
text = "low low low low low lower lower newest newest newest widest"
vocab, merges = train_bpe(text, num_merges=10)

print("\nFinal vocabulary:")
for word, freq in sorted(vocab.items(), key=lambda x: -x[1]):
    print(f"  {word}: {freq}")
```

> 💡 **What's happening step by step**:
> 1. We count how often each word appears
> 2. We split every word into individual characters: `"low"` → `"l o w </w>"`
> 3. We count adjacent character pairs and merge the most frequent one
> 4. Repeat! Each merge creates a new "token" in our vocabulary

## Using HuggingFace Tokenizers 🤗

```python
from transformers import AutoTokenizer

# Load GPT-2 tokenizer (BPE)
tokenizer = AutoTokenizer.from_pretrained("gpt2")

text = "Transformers are incredible neural networks!"
tokens = tokenizer.tokenize(text)
ids = tokenizer.encode(text)

print(f"Text: {text}")
print(f"Tokens: {tokens}")      # Human-readable pieces
print(f"IDs: {ids}")             # Numbers the model actually sees
print(f"Token count: {len(ids)}")

# Decode back (the reverse journey!)
decoded = tokenizer.decode(ids)
print(f"Decoded: {decoded}")

# Examine vocabulary
print(f"\nVocab size: {tokenizer.vocab_size}")
print(f"Special tokens: {tokenizer.special_tokens_map}")
```

> ⭐ **The Encoding/Decoding Pipeline:**
>
> $$\text{Text} \xrightarrow{\text{tokenize}} \text{Tokens} \xrightarrow{\text{encode}} \text{IDs} \xrightarrow{\text{model}} \text{Output IDs} \xrightarrow{\text{decode}} \text{Text}$$
>
> 🔑 **Tokenizer training vs Model training**: The tokenizer is trained SEPARATELY from the model! It's trained on a text corpus (no gradient descent, no neural network). The model is then trained WITH the frozen tokenizer. They're two different "training" processes!

---

# 🔟 Founder Edge: Tokenizer as Strategic Choice 🚀💼

## For Your AI Startup

> 🧩 **Analogy**: Choosing a tokenizer is like choosing the **language your company speaks** 🗣️. Pick the wrong one, and you'll spend forever translating — wasting time, money, and confusing your customers!

1. 💰 **Cost is per-token**: tokenization efficiency = cost efficiency
   - A tokenizer that uses 2x more tokens = 2x the API bill!
2. 📏 **Context window is in tokens**: efficient tokenizer = more context
   - 8K tokens of efficient English ≈ 4K tokens of inefficient Hindi
3. 🎯 **Training a custom tokenizer** on your domain data can improve efficiency **30–50%**
   - Medical AI? Train on medical text. Code AI? Train on code.
4. 🌍 **Multilingual products**: tokenizer bias toward English hurts non-English users
   - Your Hindi or Arabic users pay more AND get worse results!

## ⭐ Key Decision: Train vs Reuse Tokenizer

| Scenario | Recommendation | Why |
|---|---|---|
| **Fine-tuning existing model** | MUST use its tokenizer | Embeddings are trained for that tokenizer |
| **Training from scratch** | Train your own tokenizer on your domain | Domain-specific tokens = efficiency |
| **Domain-specific** (code, medical) | Domain tokenizer significantly helps | "pneumonoultramicroscopicsilicovolcanoconiosis" → 1 token vs 8! |

---

# 📊 Complete Tokenizer Comparison Table

| Feature | BPE 🔵 | WordPiece 🟢 | Unigram 🟡 | Byte-Level BPE 🔴 |
|---|---|---|---|---|
| **Algorithm** | Bottom-up merge | Bottom-up merge | Top-down pruning | Bottom-up merge on bytes |
| **Merge criterion** | Frequency | Likelihood ratio | Likelihood loss | Frequency |
| **Starting point** | Characters | Characters | All substrings | Bytes (0–255) |
| **OOV handling** | Rare: fallback to chars | `[UNK]` token | Rare: fallback to chars | **Never** (any byte representable) |
| **Used by** | GPT-2/3/4, LLaMA | BERT, DistilBERT | T5, ALBERT | GPT-2/3/4, LLaMA-2/3 |
| **Language-agnostic?** | Needs pre-tokenization | Needs pre-tokenization | ✅ Yes (via SentencePiece) | ✅ Yes |
| **Deterministic encoding?** | ✅ Yes | ✅ Yes | ❌ No (multiple valid segmentations) | ✅ Yes |

---

# 🔬 Deep Research Corner: Landmark Papers & Frontier Work 📚

## Seminal Papers 📜

| Year | Paper | Contribution | Impact |
|---|---|---|---|
| **1994** | Philip Gage — "A New Algorithm for Data Compression" | Original BPE algorithm | Foundation of modern tokenization |
| **2016** | **Sennrich et al.** — "Neural Machine Translation of Rare Words with Subword Units" | Adapted BPE for NMT | ⭐ The paper that started it all for NLP tokenization |
| **2018** | **Kudo** — "SentencePiece: A simple and language independent subword tokenizer" | Language-agnostic tokenization | Enabled truly multilingual models |
| **2018** | **Kudo** — "Subword Regularization" | Unigram language model approach | Alternative to BPE, used in T5 |
| **2019** | **Radford et al.** — "Language Models are Unsupervised Multitask Learners" (GPT-2) | Byte-level BPE | Modern standard for LLMs |

## 🌟 Frontier Research: What's Next?

### Tokenization-Free Models 🔮
- **Researchers are exploring**: Can we skip tokenization entirely?
- **ByT5** (Xue et al., 2022): Operates directly on UTF-8 bytes — no tokenizer!
- **Tradeoff**: Sequences are ~4x longer, but no tokenization artifacts
- **MEGABYTE** (Yu et al., 2023): Patches of bytes → efficient byte-level modeling

### Adaptive Tokenization 🧬
- What if the tokenizer could **adapt** to different domains or languages?
- Research on **dynamic vocabulary** that expands/contracts based on input

### The Tokenization Tax 💸
- **Clark et al. (2022)**: Showed tokenization choices can affect model performance by up to **20%** on downstream tasks
- Non-English languages pay a "tokenization tax" — more tokens for same information

> ⭐ **Big Open Question**: Will future models be tokenization-free? Byte-level models show promise, but attention's $O(n^2)$ cost makes long byte sequences expensive. The race is on! 🏁

---

# 📝 Summary

| Concept | Key Insight | Beginner Analogy |
|---------|-------------|------------------|
| 🔤 **Tokenization** | The bridge between text and tensors — model's "perception" | Chopping sentences into numbered puzzle pieces 🧩 |
| 🔵 **BPE** | Bottom-up merging by frequency. Used in GPT family | Gluing the most popular letter pairs together 🔗 |
| 🟢 **WordPiece** | Merges by likelihood ratio. Used in BERT | Gluing pairs that are never seen apart 💑 |
| 🌍 **SentencePiece** | Language-agnostic, handles raw bytes | The multilingual librarian who doesn't need spaces 📚 |
| 🔴 **Byte-level BPE** | Start from bytes, never `<UNK>`. Modern standard | Working with raw cardboard, not pre-cut pieces 🧩 |
| ⚖️ **Vocab size** | Trade-off: sequence length vs embedding size ($V \times d_{\text{model}}$) | Dictionary size: thin & quick vs thick & complete 📖 |
| 🚦 **Special tokens** | Control signals with no linguistic meaning | Road signs for the model 🛣️ |
| 📏 **Fertility** | Tokens/word — measures tokenizer efficiency per language | How many pieces each word breaks into |
| 📊 **Compression ratio** | Characters/token — measures overall efficiency | How much text fits in one token |

---

# 🏁 Quick-Reference Cheat Sheet

```
┌─────────────────────────────────────────────────────────────────┐
│                TOKENIZATION CHEAT SHEET 📋                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  WHAT IS TOKENIZATION?                                          │
│  Text → Pieces → Numbers → Model can read it!                   │
│                                                                 │
│  THREE APPROACHES:                                              │
│  • Word-level:  "hello world" → ["hello", "world"]    ❌ OOV   │
│  • Char-level:  "hello" → ["h","e","l","l","o"]       ❌ Long  │
│  • Subword:     "unhappy" → ["un","happy"]            ✅ Best! │
│                                                                 │
│  FOUR ALGORITHMS:                                               │
│  • BPE:         Merge most frequent pairs (GPT)                 │
│  • WordPiece:   Merge most dependent pairs (BERT)               │
│  • Unigram:     Remove least useful tokens (T5)                 │
│  • Byte BPE:    BPE on raw bytes — no <UNK>! (GPT-2+)          │
│                                                                 │
│  KEY FORMULAS:                                                  │
│  • Embedding params = V × d_model                               │
│  • Fertility = tokens / words                                   │
│  • Compression = characters / tokens                            │
│  • WordPiece score = freq(ab) / (freq(a) × freq(b))            │
│                                                                 │
│  SPECIAL TOKENS:                                                │
│  [CLS] = start marker    [SEP] = separator                     │
│  [PAD] = filler           [MASK] = blank for training           │
│  <bos> = begin sequence   <eos> = end sequence                  │
│  [UNK] = "never seen this before!"                              │
│                                                                 │
│  PRODUCTION VOCAB SIZES:                                        │
│  GPT-4: ~100K │ BERT: 30K │ LLaMA-2: 32K │ LLaMA-3: 128K     │
│                                                                 │
│  GOLDEN RULES:                                                  │
│  1. Tokenizer ≠ Model (trained separately!)                     │
│  2. Bigger vocab = shorter sequences but more memory            │
│  3. Non-English languages pay a "tokenization tax"              │
│  4. Fine-tuning? MUST use the model's original tokenizer        │
│  5. Cost is per-token — tokenizer efficiency = cost efficiency  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

**Tomorrow**: We implement a complete Byte Pair Encoding tokenizer from scratch, including training and encoding/decoding. 🔧👨‍💻
