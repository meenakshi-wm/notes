📘 DAY 8 — Byte Pair Encoding: Full Implementation from Scratch 🔤✂️🧩

---

## 🎯 Goal

Build a complete BPE tokenizer from raw text — training the merge table, encoding text to token IDs, decoding back to text. Understand every detail of how GPT-family models convert language into numbers.

## 🧠 If This Is Deeply Understood

You can train custom tokenizers for any domain, debug tokenization issues in production, and understand the computational bottleneck at the interface between language and model.

---

## 🌍 The Big Picture: Why Tokenization Matters

> ⭐ **Beginner Analogy — Text Compression (Like ZIP for Language)**
>
> Imagine you're texting your friend and you both agree on shortcuts:
> - "btw" = "by the way"
> - "idk" = "I don't know"
> - "lol" = "laughing out loud"
>
> You just **compressed** your language! BPE does the EXACT same thing but automatically. It looks at a huge pile of text, finds the letter combos that appear together the most, and gives them a single shortcut symbol. The more common the combo, the earlier it gets a shortcut. That's it. That's BPE. 🎉

### 🤔 Why Can't We Just Use Words?

| Approach | Problem | Example |
|----------|---------|---------|
| **Whole words** | Vocabulary explodes (millions of words) | "unhappiness", "unhappily", "unhappy" = 3 separate entries |
| **Single characters** | Sequences become absurdly long | "hello" = 5 tokens → slow & expensive |
| **BPE (subwords)** ✅ | Best of both worlds — common words stay whole, rare words split into known pieces | "unhappiness" = ["un", "happi", "ness"] |

> ⭐ **Think of it like LEGO blocks 🧱**: Instead of having a unique brick for every possible shape (words) or only using 1×1 dots (characters), BPE finds the most useful mid-sized brick shapes that let you build ANY word efficiently.

---

## 📐 Core Mathematics of BPE

### 🔑 Key Formula: Merge Rule Selection

At each step, BPE picks the **most frequent adjacent pair** to merge:

$$\text{best\_pair} = \arg\max_{(a, b) \in \text{Pairs}} \sum_{w \in \text{corpus}} \text{count}(a, b \mid w)$$

> Translation: "Scan every pair of neighbors in our data. Whichever pair shows up the most? Merge those two into one new symbol."

### 📊 Compression Ratio

$$\text{Compression Ratio} = \frac{\text{Number of characters in text}}{\text{Number of tokens after encoding}}$$

- English with GPT-2 tokenizer: ~$3.5$ to $4.5$ chars/token
- Code: ~$2.5$ to $3.5$ chars/token (less compressible)
- Chinese/Japanese: ~$1.5$ to $2.5$ chars/token (each character is already dense)

### 📈 Vocabulary Size Growth

$$|\text{Vocab after } k \text{ merges}| = 256 + k$$

> We start with $256$ byte-level tokens (every possible single byte). Each merge adds exactly **one** new token. So after $k$ merges, the vocab has $256 + k$ entries. GPT-2 uses $k = 50,000$ merges → vocab size $= 50,257$ (with special tokens).

### ⏱️ Time Complexity of BPE Training

$$O(N \times k)$$

where $N$ = total number of tokens in the corpus (initially bytes), $k$ = number of merge operations.

> Each of the $k$ merge steps requires scanning all pair frequencies (proportional to $N$). Production implementations use hash maps and incremental updates to make this much faster in practice.

---

# 1️⃣ BPE Training: The Complete Pipeline 🏗️

## Phase 1: Corpus Preprocessing 📋

Before BPE training, text is preprocessed:

```python
import regex as re
from collections import Counter, defaultdict

class BPETokenizer:
    def __init__(self):
        self.merges = {}      # (pair) -> merged token
        self.vocab = {}       # token -> id
        self.inverse_vocab = {}  # id -> token
        
        # GPT-2 style regex for pre-tokenization
        # This splits text into "words" before BPE
        self.pat = re.compile(
            r"""'s|'t|'re|'ve|'m|'ll|'d| ?\p{L}+| ?\p{N}+| ?[^\s\p{L}\p{N}]+|\s+(?!\S)|\s+"""
        )
    
    def _pre_tokenize(self, text):
        """Split text into words using regex pattern."""
        return re.findall(self.pat, text)
```

> ⭐ **Beginner Analogy — Pre-tokenization Regex = Putting Up Fences 🏡🪵**
>
> Imagine you have a long street of houses (your text). Before you start combining houses, you put up **fences** between properties. The regex is the fence rule — it says "this is where one word ends and another begins." Now when BPE starts merging letters, it **can't merge across fences**. The letter "e" at the end of "the" will NEVER merge with the "d" at the start of "dog".
>
> Without fences? BPE might learn "e d" as a common pair and create a nonsense token "ed" that spans two words. Chaos! 🚫

### Why pre-tokenize?
- ✅ Prevents merging across word boundaries ("the" + " dog" → "the dog")
- ✅ Each "word" (regex match) is BPE-encoded independently
- ✅ Spaces are PART of the word: " hello" is one pre-token

### 🔬 Regex Pattern Breakdown

| Pattern | What It Matches | Example |
|---------|----------------|---------|
| `'s\|'t\|'re\|'ve\|'m\|'ll\|'d` | English contractions | "don't" → ["don", "'t"] |
| `?\p{L}+` | Unicode letters (optional leading space) | " hello", "world" |
| `?\p{N}+` | Unicode numbers (optional leading space) | " 42", "3.14" |
| `?[^\s\p{L}\p{N}]+` | Punctuation & symbols | " !", " @#" |
| `\s+(?!\S)` | Trailing whitespace | Whitespace at end of text |
| `\s+` | Any remaining whitespace | Spaces between words |

## Phase 2: Initialize with Bytes 🔢

```python
    def _text_to_bytes(self, text):
        """Convert text to list of byte values."""
        return list(text.encode('utf-8'))
    
    def _get_pair_counts(self, sequences):
        """Count frequency of adjacent byte/token pairs across all sequences."""
        counts = Counter()
        for seq, freq in sequences:
            for i in range(len(seq) - 1):
                counts[(seq[i], seq[i+1])] += freq
        return counts
    
    def _merge_pair(self, sequences, pair, new_token):
        """Replace all occurrences of pair with new_token in all sequences."""
        new_sequences = []
        for seq, freq in sequences:
            new_seq = []
            i = 0
            while i < len(seq):
                if i < len(seq) - 1 and seq[i] == pair[0] and seq[i+1] == pair[1]:
                    new_seq.append(new_token)
                    i += 2
                else:
                    new_seq.append(seq[i])
                    i += 1
            new_sequences.append((new_seq, freq))
        return new_sequences
```

> ⭐ **Beginner Analogy — Byte-Level = Starting from Raw Atoms ⚛️**
>
> Think of every piece of text as ultimately being stored as numbers $0$-$255$ (bytes) in your computer. English letters, Chinese characters, emojis 🤖, Arabic script العربية — **everything** becomes bytes.
>
> By starting from bytes, BPE can handle ANY language, ANY symbol, ANY emoji without special rules. It's like having universal LEGO pieces that work with every set ever made.
>
> - `"h"` → byte $104$
> - `"é"` → bytes $195, 169$ (2 bytes in UTF-8!)
> - `"🤖"` → bytes $240, 159, 164, 150$ (4 bytes!)

## Phase 3: Training Loop 🔄

```python
    def train(self, text, vocab_size=500):
        """Train BPE on text corpus."""
        # Step 1: Pre-tokenize
        words = self._pre_tokenize(text)
        word_freqs = Counter(words)
        
        # Step 2: Convert to byte sequences with frequencies
        sequences = []
        for word, freq in word_freqs.items():
            byte_seq = self._text_to_bytes(word)
            sequences.append((byte_seq, freq))
        
        # Step 3: Initialize vocab with single bytes (0-255)
        self.vocab = {bytes([i]): i for i in range(256)}
        next_token_id = 256
        
        # Step 4: Iteratively merge most frequent pairs
        num_merges = vocab_size - 256
        self.merge_list = []  # ordered list of merges
        
        for merge_idx in range(num_merges):
            # Count pairs
            pair_counts = self._get_pair_counts(sequences)
            if not pair_counts:
                break
            
            # Find most frequent pair
            best_pair = max(pair_counts, key=pair_counts.get)
            best_count = pair_counts[best_pair]
            
            # Create new token
            new_token = next_token_id
            self.merges[best_pair] = new_token
            self.merge_list.append(best_pair)
            
            # Merge in all sequences
            sequences = self._merge_pair(sequences, best_pair, new_token)
            
            next_token_id += 1
            
            if merge_idx % 50 == 0:
                print(f"Merge {merge_idx}: {best_pair} → {new_token} (count: {best_count})")
        
        # Build inverse vocab
        self.inverse_vocab = {v: k for k, v in self.vocab.items()}
        for pair, token_id in self.merges.items():
            self.inverse_vocab[token_id] = pair
        
        print(f"\nTraining complete. Vocab size: {next_token_id}")
```

### 🎬 Step-by-Step Walkthrough: BPE Training in Action

Let's trace through a tiny example to make this crystal clear:

**Corpus**: `"low low low lower lowest"`

**Step 0 — Start with bytes** (showing as characters for readability):

```
"low"   (freq: 3) → [l, o, w]
"lower" (freq: 1) → [l, o, w, e, r]
"lowest"(freq: 1) → [l, o, w, e, s, t]
```

**Step 1 — Count all adjacent pairs**:

| Pair | Count | How? |
|------|-------|------|
| (l, o) | $3 + 1 + 1 = 5$ | Appears in all 3 words |
| (o, w) | $3 + 1 + 1 = 5$ | Appears in all 3 words |
| (w, e) | $1 + 1 = 2$ | "lower" + "lowest" |
| (e, r) | $1$ | "lower" only |
| (e, s) | $1$ | "lowest" only |
| (s, t) | $1$ | "lowest" only |

**Tie at 5!** BPE picks one (implementation-dependent). Say we pick **(l, o) → token 256**:

```
"low"   → [256, w]         ← "lo" became one token!
"lower" → [256, w, e, r]
"lowest"→ [256, w, e, s, t]
```

**Step 2 — Re-count pairs** with the new token:

| Pair | Count |
|------|-------|
| (256, w) | $5$ | ← "lo" + "w" in every word |
| (w, e) | $2$ |
| ... | ... |

**Merge (256, w) → token 257** (this represents "low"):

```
"low"   → [257]            ← Entire word is ONE token!
"lower" → [257, e, r]
"lowest"→ [257, e, s, t]
```

> 🎯 **Key Insight**: After just 2 merges, the common word "low" became a single token! Rare words like "lowest" still use pieces. This is the magic of BPE — common things get compressed, rare things stay decomposed.

---

# 2️⃣ BPE Encoding (Text → Token IDs) 📝➡️🔢

```python
    def encode(self, text):
        """Encode text to token IDs."""
        # Pre-tokenize
        words = re.findall(self.pat, text)
        
        all_tokens = []
        for word in words:
            # Start with bytes
            tokens = self._text_to_bytes(word)
            
            # Apply merges in order
            for pair in self.merge_list:
                new_tokens = []
                i = 0
                while i < len(tokens):
                    if i < len(tokens) - 1 and tokens[i] == pair[0] and tokens[i+1] == pair[1]:
                        new_tokens.append(self.merges[pair])
                        i += 2
                    else:
                        new_tokens.append(tokens[i])
                        i += 1
                tokens = new_tokens
            
            all_tokens.extend(tokens)
        
        return all_tokens
```

### ⭐ Why Apply Merges in Order? 🔑

The merge list is ordered by when merges were learned.
Earlier merges = more frequent patterns.
Order matters because later merges may depend on earlier ones.

> **Analogy — Building a House 🏠**: You MUST pour the foundation before you build the walls, and walls before the roof. BPE merges are the same — later merges are built ON TOP of earlier ones.

Example:
```
Merge 1: ('l', 'o') → 256         ← Foundation
Merge 2: (256, 'w') → 257         ← This is ('lo', 'w') → 'low' — depends on Merge 1!
```

If we applied Merge 2 first, we'd never find the pair $(256, w)$ because token $256$ doesn't exist yet! The whole thing collapses. 💥

### 📊 The Encoding Pipeline Visualized

```
Input:  "The transformer uses attention"
         │
         ▼
[Pre-tokenize with regex]
         │
         ▼
Words:  ["The", " transformer", " uses", " attention"]
         │
         ▼
[Convert each word to bytes]
         │
         ▼
Bytes:  [[84,104,101], [32,116,114,97,110,115,...], ...]
         │
         ▼
[Apply merge 1, then merge 2, ..., then merge k]
         │
         ▼
Tokens: [464, 48647, 3544, 3241]   ← Final token IDs!
```

---

# 3️⃣ BPE Decoding (Token IDs → Text) 🔢➡️📝

```python
    def decode(self, token_ids):
        """Decode token IDs back to text."""
        byte_values = []
        for token_id in token_ids:
            byte_values.extend(self._token_to_bytes(token_id))
        return bytes(byte_values).decode('utf-8', errors='replace')
    
    def _token_to_bytes(self, token_id):
        """Recursively decompose a token into its byte values."""
        if token_id < 256:
            return [token_id]
        # This token was created by merging a pair
        pair = self.inverse_vocab[token_id]
        return self._token_to_bytes(pair[0]) + self._token_to_bytes(pair[1])
```

> ⭐ **Beginner Analogy — Unpacking a Russian Nesting Doll 🪆**
>
> Decoding is like opening nested dolls. Token $257$ ("low") opens into tokens $256$ ("lo") + $119$ ("w"). Then token $256$ opens into $108$ ("l") + $111$ ("o"). Once you've got only base bytes ($< 256$), you convert them back to text characters. Every merge can be perfectly undone!

### ✅ Round-Trip Guarantee

$$\text{decode}(\text{encode}(\text{text})) = \text{text}$$

This is **always** true for byte-level BPE. No information is ever lost. Every possible UTF-8 string can be encoded and perfectly decoded.

---

# 4️⃣ Efficient Encoding with Priority Queue ⚡

The naive "apply merges in order" approach is $O(n \times m)$ where $n$ = sequence length, $m$ = number of merges.

Production tokenizers use a priority queue:

```python
import heapq

def encode_efficient(self, text):
    """Efficient encoding using priority queue."""
    words = re.findall(self.pat, text)
    all_tokens = []
    
    for word in words:
        tokens = self._text_to_bytes(word)
        
        if len(tokens) <= 1:
            all_tokens.extend(tokens)
            continue
        
        # Build priority queue of mergeable pairs
        # Priority = merge rank (position in merge_list, lower = higher priority)
        while True:
            # Find the pair with the lowest merge rank
            best_pair = None
            best_rank = float('inf')
            best_pos = -1
            
            for i in range(len(tokens) - 1):
                pair = (tokens[i], tokens[i+1])
                if pair in self.merges:
                    # Find rank of this merge
                    rank = self.merge_list.index(pair) if pair in self.merge_list else float('inf')
                    if rank < best_rank:
                        best_rank = rank
                        best_pair = pair
                        best_pos = i
            
            if best_pair is None:
                break
            
            # Apply the merge
            new_token = self.merges[best_pair]
            tokens = tokens[:best_pos] + [new_token] + tokens[best_pos + 2:]
        
        all_tokens.extend(tokens)
    
    return all_tokens
```

### ⚡ Complexity Comparison

| Approach | Time Complexity | Used By |
|----------|----------------|---------|
| Naive (apply all merges in order) | $O(n \times m)$ | Educational implementations |
| Priority queue (apply best available merge) | $O(n \log n)$ | tiktoken, HuggingFace tokenizers |
| Precompiled regex + lookup table | $O(n)$ | tiktoken (production mode) |

> Where $n$ = input length in bytes, $m$ = number of merge rules (e.g., $50{,}000$).

> ⭐ **Why the priority queue works**: Instead of scanning through all $50{,}000$ merge rules for each position, we only look at the pairs that actually exist in our current sequence and pick the one with the smallest merge rank (= learned earliest = highest priority). This is the greedy strategy, and it gives the same result as applying all merges in order.

---

# 5️⃣ Complete Working Example 🚀

```python
# Full training and usage example
text = """
The transformer architecture has revolutionized natural language processing.
Transformers use self-attention mechanisms to process sequences in parallel.
The key innovation is the attention mechanism which allows the model to focus
on different parts of the input sequence. Self-attention computes query, key,
and value projections for each token. The scaled dot-product attention
is the core operation. Multi-head attention allows the model to attend to
information from different representation subspaces. The transformer
consists of encoder and decoder stacks, each with multi-head attention
and feed-forward layers. Layer normalization and residual connections
help with training deep transformer networks.
"""

# Train tokenizer
tokenizer = BPETokenizer()
tokenizer.train(text, vocab_size=350)

# Encode
test_text = "The transformer uses attention"
token_ids = tokenizer.encode(test_text)
print(f"\nOriginal: {test_text}")
print(f"Token IDs: {token_ids}")
print(f"Num tokens: {len(token_ids)}")

# Decode
decoded = tokenizer.decode(token_ids)
print(f"Decoded: {decoded}")
assert decoded == test_text, "Round-trip failed!"
print("✓ Round-trip encoding/decoding successful!")
```

---

# 6️⃣ Comparing with HuggingFace Tokenizers 🤗

```python
from tokenizers import Tokenizer, models, trainers, pre_tokenizers

# Train using HuggingFace's optimized library
tokenizer_hf = Tokenizer(models.BPE())
tokenizer_hf.pre_tokenizer = pre_tokenizers.ByteLevel(add_prefix_space=False)

trainer = trainers.BpeTrainer(
    vocab_size=1000,
    special_tokens=["<|endoftext|>", "<|padding|>"]
)

# Train from files
tokenizer_hf.train_from_iterator([text], trainer=trainer)

# Encode
output = tokenizer_hf.encode("The transformer uses attention")
print(f"Tokens: {output.tokens}")
print(f"IDs: {output.ids}")
```

### 🏭 Production Tokenizer Libraries Comparison

| Library | Language | Speed | Used By | Notes |
|---------|----------|-------|---------|-------|
| **tiktoken** (OpenAI) | Rust + Python | ⚡⚡⚡ Fastest | GPT-2, GPT-3, GPT-4, ChatGPT | Precompiled regex, no training API |
| **HuggingFace tokenizers** | Rust + Python | ⚡⚡ Very Fast | BERT, RoBERTa, LLaMA, Mistral | Full training + inference, most flexible |
| **SentencePiece** (Google) | C++ + Python | ⚡⚡ Fast | T5, ALBERT, XLNet, Gemma | Trains directly on raw text (no pre-tokenization needed) |
| **Custom Python** | Python | 🐢 Slow | Education | Great for learning, terrible for production |

> ⭐ **Production Reality**: tiktoken can tokenize ~$3.5$ million tokens/second on a single CPU core. A pure Python implementation does ~$10{,}000$ tokens/second. That's a **$350\times$** difference. Always use a compiled library in production!

---

# 7️⃣ Training a Domain-Specific Tokenizer 🎯

```python
from tokenizers import Tokenizer, models, trainers, pre_tokenizers, decoders

def train_domain_tokenizer(corpus_files, vocab_size=32000, output_path="tokenizer.json"):
    """Train a production-quality BPE tokenizer for your domain."""
    
    tokenizer = Tokenizer(models.BPE(unk_token="<unk>"))
    tokenizer.pre_tokenizer = pre_tokenizers.ByteLevel(add_prefix_space=False)
    tokenizer.decoder = decoders.ByteLevel()
    
    trainer = trainers.BpeTrainer(
        vocab_size=vocab_size,
        special_tokens=[
            "<unk>", "<s>", "</s>", "<pad>",
            "<|system|>", "<|user|>", "<|assistant|>",
        ],
        min_frequency=2,
        show_progress=True,
    )
    
    tokenizer.train(files=corpus_files, trainer=trainer)
    tokenizer.save(output_path)
    
    return tokenizer

# Usage:
# tok = train_domain_tokenizer(["domain_corpus.txt"], vocab_size=32000)
```

> ⭐ **When to Train Your Own Tokenizer** 🤔
>
> | Scenario | Use GPT-4 Tokenizer? | Train Custom? |
> |----------|----------------------|---------------|
> | General English chatbot | ✅ Yes | ❌ Overkill |
> | Medical/legal domain app | ⚠️ Okay-ish | ✅ 20-50% shorter sequences |
> | Code-heavy application | ⚠️ Suboptimal | ✅ Code tokens compress better |
> | Non-English language (e.g., Tamil, Thai) | ❌ Very wasteful | ✅ **Huge** improvement |
> | Mixed language + domain | ❌ Bad | ✅ Essential |

---

# 8️⃣ Tokenization Quality Metrics 📏

```python
def analyze_tokenizer(tokenizer, test_texts):
    """Analyze tokenizer quality on test texts."""
    total_tokens = 0
    total_chars = 0
    total_words = 0
    
    for text in test_texts:
        tokens = tokenizer.encode(text)
        total_tokens += len(tokens.ids) if hasattr(tokens, 'ids') else len(tokens)
        total_chars += len(text)
        total_words += len(text.split())
    
    print(f"Compression ratio (chars/tokens): {total_chars / total_tokens:.2f}")
    print(f"Fertility (tokens/word): {total_tokens / total_words:.2f}")
    print(f"Average token length: {total_chars / total_tokens:.1f} chars")
    
    # Good targets:
    # English compression: 3.5-4.5 chars/token
    # English fertility: 1.2-1.5 tokens/word
```

### 📊 Quality Metrics Explained

| Metric | Formula | Good Value (English) | What It Means |
|--------|---------|---------------------|---------------|
| **Compression Ratio** | $\frac{\text{chars}}{\text{tokens}}$ | $3.5 - 4.5$ | Higher = more efficient. Each token represents more characters |
| **Fertility** | $\frac{\text{tokens}}{\text{words}}$ | $1.2 - 1.5$ | Lower = better. How many tokens per word on average |
| **Unknown Rate** | $\frac{\text{UNK tokens}}{\text{total tokens}}$ | $0\%$ (byte-level) | Byte-level BPE = ZERO unknowns, ever! |
| **Coverage** | $\frac{\text{unique tokens used}}{\text{vocab size}}$ | $> 50\%$ | Higher = less wasted vocabulary space |

> ⭐ **Why Fertility Matters Financially** 💰: If your tokenizer has fertility $1.5$ (English) vs. $3.0$ (Thai with a bad tokenizer), Thai users pay **$2\times$** as much for the same meaning. This is a real equity issue in AI — poorly tokenized languages = more expensive to use.

---

# 9️⃣ Edge Cases & Gotchas ⚠️

## Unicode Handling 🌐
```python
# Emoji handling
text = "I love AI! 🤖🧠"
tokens = tokenizer.encode(text)
# Emoji are multi-byte in UTF-8: 🤖 = 4 bytes
# May become 1-4 tokens depending on training data
```

### 🔬 How Multi-Byte Characters Work in BPE

| Character | UTF-8 Bytes | # of Initial BPE Tokens | After Training |
|-----------|-------------|--------------------------|----------------|
| `"a"` | `97` | 1 | 1 (always) |
| `"é"` | `195, 169` | 2 | Often 1 (common in French) |
| `"中"` | `228, 184, 173` | 3 | Often 1-2 (if Chinese in training) |
| `"🤖"` | `240, 159, 164, 150` | 4 | 1-4 (depends on emoji frequency) |

> **Key insight**: Rare emoji might stay as 4 separate tokens. Common ones (like ❤️) often merge into 1. This is why sending obscure emoji to GPT costs more tokens!

## Whitespace Sensitivity 🔍
```python
# Leading space matters!
tokens_with_space = tokenizer.encode(" hello")   # [" hello"] or [" he", "llo"]
tokens_no_space = tokenizer.encode("hello")      # ["hello"] or ["he", "llo"]
# These produce DIFFERENT tokenizations!
```

> ⭐ **Why spaces matter**: In GPT-2's tokenizer, `" hello"` (with leading space) and `"hello"` (no space) are DIFFERENT tokens. The space is literally baked into the token. This is by design — words usually have a space before them, so `" the"` is more common than `"the"`. Token `" the"` = ID $262$, while `"the"` = ID $1169$ in GPT-2!

## The "Trailing Whitespace" Bug 🐛
When generating text token by token, be careful:
"hello " ends with a space that may merge with the next token.
This is why streaming outputs can look jumpy — a token like `" world"` includes its leading space, so the display might seem to "jump" when that token arrives.

---

# 🔟 Founder Application: Token Economics 💰📊

## Cost Calculation
Most APIs charge per token:
- GPT-4: ~\$0.03/1K input tokens, ~\$0.06/1K output tokens
- GPT-3.5: ~\$0.0005/1K input tokens

For your startup:
```
Avg user query: 50 tokens
System prompt: 500 tokens  
Avg response: 200 tokens
Total per request: 750 tokens

10K users × 20 requests/day = 200K requests/day
200K × 750 = 150M tokens/day
At $0.03/1K = $4,500/day for input alone
```

A custom tokenizer that reduces tokens by 20% = \$900/day savings = \$328K/year.

### 💡 Token Optimization Strategies for Startups

| Strategy | Token Savings | Effort | Example |
|----------|--------------|--------|---------|
| Shorten system prompts | 10-30% | Low | Remove verbose instructions |
| Custom tokenizer for domain | 20-50% | High | Medical, legal, code |
| Prompt caching (same prefix) | 50-90% on repeated prefixes | Medium | OpenAI prompt caching |
| Batch similar requests | 10-20% | Medium | Common system prompt |
| Compress few-shot examples | 15-25% | Low | Use shorter examples |

---

# 🔬 Deep Research: The History & Science of BPE 📚

## 📜 Timeline of Key Papers

| Year | Paper | Contribution |
|------|-------|-------------|
| **1994** | **Gage, "A New Algorithm for Data Compression"** | Original BPE for byte-level compression. Simple, greedy, effective. Used for file compression, NOT NLP. |
| **2016** | **Sennrich, Haddow & Birch, "Neural Machine Translation of Rare Words with Subword Units"** ⭐ | **Watershed moment**: Applied BPE to NLP. Showed subword tokenization crushes word-level for translation. Solved the rare/unknown word problem. |
| **2018** | **Kudo & Richardson, "SentencePiece"** | Language-independent subword tokenizer. Trains directly on raw text. Used by Google's T5, mBART. |
| **2019** | **Radford et al., "Language Models are Unsupervised Multitask Learners" (GPT-2)** ⭐ | Introduced **byte-level BPE** — start from bytes instead of characters. Eliminated UNK tokens entirely. Every model since uses this idea. |
| **2020** | **Wang et al., "Neural Machine Translation with Byte-Level Subwords"** | Showed byte-level models can match character-level quality with much shorter sequences. |

### 🧬 Why Sennrich et al. (2016) Changed Everything

Before 2016, neural translation models used fixed word vocabularies (~$50{,}000$ words). Problem:

$$P(\text{"Bundesgrenzschutz"}) = 0 \quad \text{(not in vocabulary → UNK token → translation fails)}$$

Sennrich's insight: Apply BPE from compression to split rare words into known subwords:

$$\text{"Bundesgrenzschutz"} \rightarrow \text{["Bundes", "grenz", "schutz"]}$$

Now the model can translate each piece! BLEU scores jumped by $+1.1$ to $+1.5$ on English↔German.

### 🔑 Radford et al. (2019): The Byte-Level Revolution

GPT-2 made one crucial change: instead of starting BPE from **characters** (Unicode codepoints), start from **bytes** (0-255).

Why this matters:

| Starting Point | Vocab Base | Handles Any Language? | UNK Possible? |
|---------------|-----------|----------------------|---------------|
| Words | ~50,000+ | ❌ No | ✅ Yes (OOV) |
| Characters (Unicode) | ~130,000 | ⚠️ Most | ⚠️ Rare chars |
| **Bytes** ✅ | **256** | **✅ ALL** | **❌ Never** |

> "Every possible string is representable." — There is no text in any language, script, or encoding that byte-level BPE cannot handle. This single design choice made GPT-2 and all its successors truly universal.

---

# 🔗 BPE vs. Other Tokenization Methods

| Method | How It Works | Pros | Cons | Used By |
|--------|-------------|------|------|---------|
| **BPE** | Bottom-up merging of frequent pairs | Simple, effective, lossless | Greedy (not globally optimal) | GPT-2/3/4, RoBERTa |
| **WordPiece** | Like BPE but picks merges by likelihood gain, not raw frequency | Slightly better for some tasks | More complex training | BERT, DistilBERT |
| **Unigram LM** | Top-down: start with huge vocab, prune least useful tokens | Globally optimized probabilities | Slower training | SentencePiece, T5, XLNet |
| **Character-level** | Each character = one token | Tiny vocab, no OOV | Very long sequences | Some research models |
| **Byte-level** | Each byte = one token (no merging) | Universal, simplest | Extremely long sequences | ByT5 |

> ⭐ The BPE selection criterion vs. WordPiece:
>
> **BPE**: $\arg\max_{(a,b)} \text{count}(a, b)$ — pure frequency
>
> **WordPiece**: $\arg\max_{(a,b)} \frac{\text{count}(ab)}{\text{count}(a) \times \text{count}(b)}$ — pointwise mutual information (PMI)
>
> WordPiece prefers pairs that are **surprisingly common together**, not just common overall. This sometimes learns slightly better merges but is more expensive to compute.

---

# 📝 Summary & Key Takeaways

| Component | Implementation Detail |
|-----------|----------------------|
| Pre-tokenization | Regex split before BPE (prevents cross-word merges) |
| Byte-level init | Start from 256 bytes, not characters |
| Training loop | Count pairs → merge most frequent → repeat |
| Encoding | Apply merges in learned order |
| Decoding | Recursively decompose to bytes → UTF-8 decode |
| Efficiency | Priority queue for $O(n \log n)$ encoding |
| Domain training | Custom tokenizer on your corpus for 20-50% efficiency gain |

---

# 🧾 BPE Cheat Sheet — Rip This Out & Pin It! 📌

```
╔══════════════════════════════════════════════════════════════════╗
║                   BYTE PAIR ENCODING (BPE)                      ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  WHAT: Subword tokenization — splits text into "pieces" that    ║
║        are bigger than characters but smaller than words          ║
║                                                                  ║
║  WHY:  Handles ANY text (no unknown tokens), compresses well,   ║
║        balances vocab size vs. sequence length                   ║
║                                                                  ║
║  ─── TRAINING ───────────────────────────────────────────────── ║
║  1. Pre-tokenize (split into words with regex)                   ║
║  2. Convert all words → byte sequences (0-255)                   ║
║  3. Count all adjacent pairs                                     ║
║  4. Merge the most frequent pair → new token                     ║
║  5. Repeat steps 3-4 until vocab_size reached                    ║
║                                                                  ║
║  ─── ENCODING (Text → IDs) ─────────────────────────────────── ║
║  1. Pre-tokenize text                                            ║
║  2. Convert to bytes                                             ║
║  3. Apply learned merges IN ORDER (earlier = higher priority)    ║
║  4. Return token IDs                                             ║
║                                                                  ║
║  ─── DECODING (IDs → Text) ─────────────────────────────────── ║
║  1. For each token ID:                                           ║
║     • If < 256: it's a raw byte                                  ║
║     • If ≥ 256: recursively split into the pair that made it     ║
║  2. Concatenate all bytes → decode as UTF-8                      ║
║                                                                  ║
║  ─── KEY FORMULAS ───────────────────────────────────────────── ║
║  Merge selection: argmax over pair frequencies                   ║
║  Vocab size after k merges: 256 + k                              ║
║  Training complexity: O(N × k)                                   ║
║  Encoding complexity: O(n log n) with priority queue             ║
║  Compression ratio: chars / tokens (target: 3.5-4.5 for EN)     ║
║                                                                  ║
║  ─── PRODUCTION TOOLS ───────────────────────────────────────── ║
║  • tiktoken (OpenAI) — fastest, GPT models                      ║
║  • HuggingFace tokenizers — most flexible, trainable             ║
║  • SentencePiece (Google) — language-agnostic, T5/Gemma          ║
║                                                                  ║
║  ─── GOTCHAS ────────────────────────────────────────────────── ║
║  • " hello" ≠ "hello" (leading space = different token!)         ║
║  • Emoji = 4 bytes = up to 4 tokens if rare                     ║
║  • Merge order matters (later merges depend on earlier ones)     ║
║  • Non-English = more tokens/word = higher API cost              ║
║                                                                  ║
╚══════════════════════════════════════════════════════════════════╝
```

### 🧠 One-Liner Memory Aids

| Concept | Remember It As... |
|---------|-------------------|
| BPE | "ZIP for language — compress common combos into single symbols" |
| Byte-level | "Start from atoms (bytes) — handles ANYTHING" |
| Pre-tokenization | "Fences between words — merges can't jump the fence" |
| Merge order | "Foundation first, roof last — order matters!" |
| Vocab size | "$256 + \text{merges}$ — one new token per merge, always" |
| Encoding | "Replay the merge tape from the beginning" |
| Decoding | "Unpack the nesting dolls back to bytes" |
| Fertility | "Tokens per word — lower is cheaper" |

---

**Tomorrow**: Vocabulary Design, Special Tokens, and Building a Complete Token Pipeline for GPT Training. 🚀
