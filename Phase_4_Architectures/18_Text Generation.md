📘 DAY 18 — Text Generation: Greedy, Temperature, Top-k, and Top-p Sampling

> 🎯 **One-Liner**: Text generation is how AI "writes" — picking one word at a time from a menu of possibilities, and the STRATEGY it uses to pick completely changes whether you get boring, brilliant, or bizarre output.

Goal:

Master every text generation strategy used in modern LLMs — from simple greedy decoding to sophisticated nucleus sampling. Understand WHY different strategies produce different quality outputs, HOW temperature controls creativity, and WHEN to use each method.

If this is deeply understood:
You can configure generation parameters for any LLM application (chatbots, code generation, creative writing), debug quality issues in generated text, and implement custom sampling strategies.

---

## 🌍 Why This Matters in the Real World

Every time you use ChatGPT, Claude, or any AI chatbot, text generation strategies are running behind the scenes. When ChatGPT gives you a creative story vs. a precise code answer, the ONLY difference is the generation parameters. Understanding this topic means you understand the "last mile" of every language model — the part that turns math into words.

⭐ **Beginner Analogy — The Word Buffet** 🍽️:
Imagine you're at an all-you-can-eat buffet with 50,000 dishes (that's roughly how many "words" an LLM knows). After eating your appetizer ("The cat sat on the"), you need to pick the next dish. Do you:
- **Always pick the most popular dish?** (Greedy — safe but boring 😴)
- **Pick randomly with some preference for popular ones?** (Temperature sampling 🌡️)
- **Only look at the top 50 dishes?** (Top-k sampling 🔝)
- **Only look at dishes until you're 90% sure you've seen the best?** (Top-p sampling 🎯)

That's literally what this entire lesson is about!

---

# 1️⃣ The Generation Problem

Given a trained LLM and a prompt, generate continuation text.

At each step:
1. Model produces logits $\in \mathbb{R}^V$ (score for each vocabulary token)
2. Convert logits to probability distribution
3. **Choose** the next token from this distribution
4. Append to sequence, repeat

The CHOICE strategy dramatically affects output quality.

> 🧠 **Think of it this way**: The model has ALREADY learned what words are likely. The generation strategy is NOT about making the model smarter — it's about deciding HOW to pick from the options the model gives us. It's like the model is a chef who prepares a ranked menu, and the generation strategy is the customer deciding how to order.

⭐ **Key Insight**: The model always produces the SAME probability distribution for a given input. What changes with different strategies is WHICH token gets selected from that distribution.

---

# 2️⃣ Greedy Decoding

## Strategy: Always pick the highest-probability token.

> 🍽️ **Beginner Analogy**: Greedy decoding is like ALWAYS going to the most popular restaurant on Yelp. You'll never have a bad meal, but you'll never discover a hidden gem either. After a while, every dinner feels the same. That's greedy decoding — safe, predictable, and eventually... boring. 😴

**Mathematically**: At each step, pick the token with the highest probability:

$$x_t = \arg\max_{x \in V} P(x \mid x_{1:t-1})$$

This simply means: "Pick the word that the model thinks is MOST likely to come next."

```python
def greedy_decode(model, input_ids, max_length=100):
    for _ in range(max_length):
        logits = model(input_ids)[:, -1, :]  # Last position logits
        next_token = logits.argmax(dim=-1, keepdim=True)
        input_ids = torch.cat([input_ids, next_token], dim=-1)
        if next_token.item() == eos_token_id:
            break
    return input_ids
```

## Pros
- Deterministic (same input → same output)
- Fast (no sampling overhead)
- Good for factual/structured output

## Cons
- **Repetitive**: often enters degenerate loops ("the the the the...")
- **Boring**: always picks "safe" words, misses interesting continuations
- **Suboptimal**: the highest-probability SEQUENCE is NOT the sequence of highest-probability TOKENS

### Why Greedy ≠ Optimal

Consider: $P(\text{"The cat"}) = 0.4$, $P(\text{"The dog"}) = 0.3$, $P(\text{"A cat"}) = 0.5$

Greedy picks "The" (highest first token) then "cat":
$P(\text{"The cat"}) = 0.4$

But "A cat" has higher sequence probability: $0.5$

Greedy is locally optimal but globally suboptimal.

> 🗺️ **Analogy — GPS Greedy vs. Optimal**: Imagine driving in a city. Greedy decoding is like ALWAYS turning onto the widest road at every intersection. You might end up on a scenic boulevard... that leads to a dead end. The ACTUAL shortest route might start on a narrow side street. Greedy makes the best LOCAL decision but often misses the best GLOBAL path.

⭐ **Production Reality**: Despite its flaws, greedy decoding IS used in production for tasks where determinism matters — like structured data extraction, classification labels, or when you need the exact same output every time (e.g., SQL generation pipelines at companies like Snowflake and Databricks). It's also used as a component in speculative decoding (more on this below ⬇️).

---

# 3️⃣ Temperature Sampling

## Strategy: Scale logits, then sample from the distribution.

> 🌡️ **Beginner Analogy — The Creativity Dial**: Imagine a volume knob on a speaker, but instead of loudness, it controls CREATIVITY. Turn it all the way down (T→0) and the AI is a cautious accountant — always picking the "safest" next word. Turn it all the way up (T→∞) and the AI is a wild poet on espresso — picking words almost randomly, creating beautiful chaos (or gibberish). The sweet spot? Somewhere in the middle! 🎛️

**The Core Formula** ⭐:

$$P(x_i) = \frac{\exp(z_i / T)}{\sum_{j} \exp(z_j / T)}$$

Where:
- $z_i$ = the raw logit (score) for token $i$
- $T$ = temperature parameter
- The numerator is the exponential of the scaled logit for token $i$
- The denominator sums over ALL tokens to normalize into a valid probability distribution

> 💡 **In plain English**: We take the model's raw scores, divide them ALL by temperature $T$, then convert to probabilities using softmax. Dividing by a small $T$ makes big scores even MORE dominant. Dividing by a large $T$ makes everything more equal.

```python
def temperature_sample(logits, temperature=1.0):
    """Apply temperature and sample."""
    scaled_logits = logits / temperature
    probs = F.softmax(scaled_logits, dim=-1)
    return torch.multinomial(probs, num_samples=1)
```

## Temperature Effects

**T = 1.0**: Original distribution (as trained)
**T < 1.0** (e.g., 0.3): Sharper distribution → more deterministic → more "focused"
**T > 1.0** (e.g., 1.5): Flatter distribution → more random → more "creative"
**T → 0**: Equivalent to greedy decoding
**T → ∞**: Uniform random over vocabulary

### 🔬 Mathematical Intuition: Why Does Division by T Work?

When $T < 1$, dividing by $T$ AMPLIFIES differences between logits:
- Logits $[5.0, 3.0]$ → divided by $0.5$ → $[10.0, 6.0]$
- The gap went from $2$ to $4$ — the winner wins by MORE

When $T > 1$, dividing by $T$ SHRINKS differences:
- Logits $[5.0, 3.0]$ → divided by $2.0$ → $[2.5, 1.5]$
- The gap went from $2$ to $1$ — the winner barely wins

After softmax, amplified gaps → one token dominates. Shrunk gaps → all tokens are close.

## Example

Original logits: $[5.0, 3.0, 2.0, 1.0]$
Probabilities at different temperatures:

| Token | T=0.5 | T=1.0 | T=2.0 |
|-------|-------|-------|-------|
| A | 0.88 | 0.64 | 0.44 |
| B | 0.09 | 0.18 | 0.26 |
| C | 0.02 | 0.11 | 0.19 |
| D | 0.01 | 0.07 | 0.11 |

Low temperature makes the model "more confident."
High temperature makes the model "explore more."

> 🎨 **Visual Mental Model**: Think of temperature as the "flatness" of a mountain range. At $T=0.5$, you have one ENORMOUS peak (Mount Everest) and tiny hills — the model almost always picks the peak. At $T=2.0$, the mountains are all roughly the same height — the model wanders freely between them.

⭐ **Production Deep-Dive** 🏭:
| Product | Temperature Setting | Why |
|---------|-------------------|-----|
| ChatGPT (default chat) | ~0.7 | Balanced creativity + coherence |
| GitHub Copilot (code) | 0.0–0.2 | Code needs precision, not creativity |
| AI Dungeon (stories) | 0.9–1.2 | Creative writing needs variety |
| Google Search AI Overview | ~0.0 | Factual answers must be deterministic |
| Claude (Anthropic) | ~0.7 | Similar balance to ChatGPT |

---

# 4️⃣ Top-k Sampling (Fan et al., 2018)

## Strategy: Only consider the top k most likely tokens.

> 🔝 **Beginner Analogy — The Restaurant Shortlist** 🍕: Imagine you're choosing dinner in a city with 50,000 restaurants. Instead of considering ALL of them (overwhelming!), you only look at the top 50 on Yelp. This is top-k sampling — you set a hard cutoff and ONLY consider the k best options. No matter how the rankings shift, you always look at exactly k restaurants.

**Mathematically**: Redistribute probability over only the top-$k$ tokens:

$$V^{(k)} = \text{argtop-}k(P(x \mid x_{1:t-1}))$$

$$P'(x_i) = \begin{cases} \frac{P(x_i)}{\sum_{x_j \in V^{(k)}} P(x_j)} & \text{if } x_i \in V^{(k)} \\ 0 & \text{otherwise} \end{cases}$$

> 💡 **In plain English**: Pick the $k$ tokens with the highest probabilities, throw away everything else, then re-normalize so the remaining probabilities add up to 1.

```python
def top_k_sample(logits, k=50, temperature=1.0):
    """Sample from top-k tokens only."""
    # Apply temperature
    logits = logits / temperature
    
    # Keep only top-k
    top_k_logits, top_k_indices = torch.topk(logits, k=k, dim=-1)
    
    # Set all other logits to -inf
    logits_filtered = torch.full_like(logits, float('-inf'))
    logits_filtered.scatter_(-1, top_k_indices, top_k_logits)
    
    # Sample
    probs = F.softmax(logits_filtered, dim=-1)
    return torch.multinomial(probs, num_samples=1)
```

## Why Top-k?

Temperature alone has a problem: even with low temperature, ALL tokens have non-zero probability. With 50K vocabulary, rare tokens occasionally get sampled → gibberish.

Top-k eliminates low-probability tokens entirely.
Only the k most likely tokens can be generated.

> ⚠️ **The Long Tail Problem**: With a vocabulary of 50,000 tokens and temperature sampling alone, even tokens with 0.001% probability will occasionally be selected. Over a 500-token generation, that means ~5 gibberish words sneaking in. Top-k cuts the tail off entirely.

## Choosing k

- **k=1**: Greedy decoding
- **k=10-50**: Good for focused generation
- **k=100-500**: More diverse generation
- **k=V**: Pure temperature sampling (no filtering)

## Problem with Fixed k

k=50 is too many when the distribution is peaked (model is confident):
$P = [0.9, 0.05, 0.01, ...]$ → tokens 4-50 are all noise

k=50 is too few when the distribution is flat (genuine ambiguity):
$P = [0.03, 0.03, 0.03, ...]$ → there might be 200 valid continuations

We need ADAPTIVE filtering → Top-p!

> 🎯 **The Core Flaw Visualized**: Imagine sometimes the restaurant list has ONE clear winner (5 stars, everything else is 2 stars). Looking at 50 options is silly — just pick the winner! But other times, there are 200 restaurants all rated between 4.0-4.5 stars. Looking at only 50 means you miss 150 perfectly good options! Fixed $k$ can't handle BOTH scenarios. This is why top-p was invented ⬇️

---

# 5️⃣ Top-p (Nucleus) Sampling (Holtzman et al., 2020)

## Strategy: Keep the smallest set of tokens whose cumulative probability ≥ p.

> 🎯 **Beginner Analogy — The Confidence Threshold** 🏆: Instead of looking at a FIXED number of restaurants, you look at restaurants from best to worst UNTIL you're 90% confident you've seen all the good ones. If the #1 restaurant already has a 92% rating dominance, you stop there (just 1 option!). If ratings are spread evenly, you might look at 200 restaurants before reaching 90% cumulative confidence. The list automatically adjusts!

⭐ **This is the most important sampling strategy in modern AI.** OpenAI, Anthropic, Google, and Meta all default to top-p sampling.

**The Core Idea** — Given probability distribution $P$, find the smallest set $V^{(p)}$ such that:

$$\sum_{x_i \in V^{(p)}} P(x_i) \geq p$$

Then sample only from tokens in $V^{(p)}$.

> 💡 **In plain English**: Sort all tokens by probability (highest first). Add them up one by one. Stop when the running total reaches $p$ (e.g., 0.9). Those tokens you've collected? That's your "nucleus." Only sample from THOSE.

### 📊 Step-by-Step Example

Prompt: "The cat sat on the ___"

Model probabilities (sorted): mat(0.35), floor(0.25), bed(0.15), couch(0.10), table(0.05), roof(0.03), ...

With **top-p = 0.9**:
| Token | Probability | Cumulative | In Nucleus? |
|-------|------------|------------|-------------|
| mat | 0.35 | 0.35 | ✅ |
| floor | 0.25 | 0.60 | ✅ |
| bed | 0.15 | 0.75 | ✅ |
| couch | 0.10 | 0.85 | ✅ |
| table | 0.05 | 0.90 | ✅ (crosses threshold!) |
| roof | 0.03 | 0.93 | ❌ |

Result: Nucleus = {mat, floor, bed, couch, table} — 5 tokens adaptively selected!

```python
def top_p_sample(logits, p=0.9, temperature=1.0):
    """Nucleus sampling: keep tokens until cumulative prob >= p."""
    # Apply temperature
    logits = logits / temperature
    
    # Sort probabilities in descending order
    sorted_logits, sorted_indices = torch.sort(logits, descending=True, dim=-1)
    sorted_probs = F.softmax(sorted_logits, dim=-1)
    cumulative_probs = torch.cumsum(sorted_probs, dim=-1)
    
    # Remove tokens with cumulative probability above p
    # Shift right so the token that crosses p is INCLUDED
    sorted_indices_to_remove = cumulative_probs - sorted_probs > p
    sorted_logits[sorted_indices_to_remove] = float('-inf')
    
    # Unsort and sample
    logits_filtered = torch.zeros_like(logits).scatter_(-1, sorted_indices, sorted_logits)
    
    probs = F.softmax(logits_filtered, dim=-1)
    return torch.multinomial(probs, num_samples=1)
```

## Why Top-p is Better than Top-k

Top-p ADAPTS to the distribution:

When model is confident: $P = [0.9, 0.05, 0.03, ...]$
→ Top-p=0.9 keeps only the TOP TOKEN (adaptive k=1)

When model is uncertain: $P = [0.1, 0.09, 0.08, 0.07, ...]$
→ Top-p=0.9 keeps ~15 TOKENS (adaptive k≈15)

The "nucleus" dynamically adjusts to include exactly the right number of tokens.

### ⭐ Top-k vs Top-p: Side-by-Side Comparison

| Scenario | Distribution | Top-k (k=50) | Top-p (p=0.9) | Winner |
|----------|-------------|-------------|---------------|--------|
| Model very confident | $[0.95, 0.02, 0.01, ...]$ | Considers 50 tokens (49 are noise!) | Considers 1 token | 🏆 Top-p |
| Model slightly uncertain | $[0.4, 0.3, 0.2, 0.1]$ | Considers 4 tokens (fine) | Considers 3 tokens (fine) | 🤝 Tie |
| Model very uncertain | $[0.01, 0.01, 0.01, ...]$ (100 equal) | Considers only 50 (misses 50!) | Considers 90 tokens | 🏆 Top-p |

## Choosing p

- **p=0.1**: Very focused (almost greedy)
- **p=0.5**: Moderate diversity
- **p=0.9**: Standard for most applications (OpenAI default)
- **p=0.95**: More creative
- **p=1.0**: No filtering (pure temperature sampling)

> 📚 **Research Deep-Dive**: The nucleus sampling paper (Holtzman et al., 2020 — *"The Curious Case of Neural Text Degeneration"*) showed that human-written text has a striking property: at each position, the "true" next word almost always falls within the top-p=0.95 nucleus, but rarely at the very top. Humans write with MODERATE surprisal — not always the most predictable word, but never a truly random one. This is why greedy text sounds robotic and pure random sampling sounds insane — neither matches human writing patterns. Nucleus sampling was designed specifically to match this "moderate surprisal" zone.
>
> 📄 Citation: Holtzman, A., Buys, J., Du, L., Forbes, M., & Choi, Y. (2020). "The Curious Case of Neural Text Degeneration." *ICLR 2020*.

---

# 6️⃣ Combining Strategies

⭐ **This is what real products actually use.** No production system uses just ONE strategy in isolation.

In practice, combine temperature + top-p (+ optionally top-k):

> 🍳 **Beginner Analogy — The Recipe**: Think of generation as cooking:
> 1. **Temperature** = how many spices you add (creativity level) 🌶️
> 2. **Top-k** = you only buy from the top 50 ingredients at the store (hard filter)
> 3. **Top-p** = you keep adding ingredients to your cart until you have 90% of what you need (smart filter)
> 4. **Then you cook** = sample one token from what's left!
>
> The ORDER matters: temperature first (adjust scores), then top-k (hard cutoff), then top-p (smart cutoff), then sample.

```python
def generate(model, input_ids, max_new_tokens=100, temperature=0.7, top_p=0.9, top_k=50):
    """Full generation with combined sampling strategies."""
    
    for _ in range(max_new_tokens):
        # Get logits
        with torch.no_grad():
            logits = model(input_ids)[:, -1, :]
        
        # Step 1: Temperature
        logits = logits / temperature
        
        # Step 2: Top-k filter
        if top_k > 0:
            top_k_values, _ = torch.topk(logits, min(top_k, logits.size(-1)))
            logits[logits < top_k_values[:, [-1]]] = float('-inf')
        
        # Step 3: Top-p filter
        if top_p < 1.0:
            sorted_logits, sorted_indices = torch.sort(logits, descending=True)
            cumulative_probs = torch.cumsum(F.softmax(sorted_logits, dim=-1), dim=-1)
            
            sorted_to_remove = cumulative_probs - F.softmax(sorted_logits, dim=-1) > top_p
            sorted_logits[sorted_to_remove] = float('-inf')
            logits = logits.scatter(-1, sorted_indices, sorted_logits)
        
        # Step 4: Sample
        probs = F.softmax(logits, dim=-1)
        next_token = torch.multinomial(probs, num_samples=1)
        
        # Append and check EOS
        input_ids = torch.cat([input_ids, next_token], dim=-1)
        if next_token.item() == eos_token_id:
            break
    
    return input_ids
```

### 🏭 What Real Products Use (2024-2025)

| Product | Temperature | Top-p | Top-k | Other | Source |
|---------|------------|-------|-------|-------|--------|
| ChatGPT (GPT-4o) | 0.7 | 0.9 | — | Freq + Presence penalty | OpenAI API docs |
| Claude 3.5 | 0.7 | 0.9 | — | — | Anthropic API docs |
| Gemini | 0.7 | 0.95 | 40 | — | Google AI docs |
| Llama 3 (Meta) | 0.6 | 0.9 | — | Repetition penalty | Meta docs |
| GitHub Copilot | 0.0–0.2 | 0.95 | — | Multiple completions | OpenAI Codex |

> 💡 **Notice the pattern**: Almost EVERYONE uses temperature ~0.7 + top-p ~0.9. This combination has emerged as the "industry standard" through extensive A/B testing across billions of conversations.

---

# 7️⃣ Beam Search

## Strategy: Maintain top-B candidate sequences at each step.

> 🔦 **Beginner Analogy — Exploring Multiple Paths** 🗺️: Imagine you're in a maze. Greedy decoding picks ONE path and never looks back. Beam search is like sending out B scouts simultaneously, each exploring a different path. At every fork, each scout evaluates the options, and you KEEP the B best-looking routes overall, pruning the rest. At the end, the scout who found the best complete path wins!

**Mathematically**: At each step $t$, maintain $B$ hypothesis sequences and expand each:

$$\text{score}(y_{1:t}) = \sum_{i=1}^{t} \log P(y_i \mid y_{1:i-1})$$

Keep the top-$B$ sequences by cumulative log-probability, optionally with length normalization:

$$\text{score}_{\text{normalized}}(y_{1:t}) = \frac{1}{t^\alpha} \sum_{i=1}^{t} \log P(y_i \mid y_{1:i-1})$$

Where $\alpha$ is the length penalty (typically 0.6–1.0). Without length normalization, beam search prefers shorter sequences (fewer negative log-probs to accumulate).

```python
def beam_search(model, input_ids, beam_width=5, max_length=100):
    """Beam search decoding."""
    B = beam_width
    
    # Initialize beams: (sequence, cumulative_log_prob)
    beams = [(input_ids.clone(), 0.0)]
    completed = []
    
    for step in range(max_length):
        all_candidates = []
        
        for seq, score in beams:
            if seq[0, -1].item() == eos_token_id:
                completed.append((seq, score))
                continue
            
            with torch.no_grad():
                logits = model(seq)[:, -1, :]
            
            log_probs = F.log_softmax(logits, dim=-1)
            top_log_probs, top_indices = torch.topk(log_probs, B, dim=-1)
            
            for i in range(B):
                new_seq = torch.cat([seq, top_indices[:, i:i+1]], dim=-1)
                new_score = score + top_log_probs[0, i].item()
                all_candidates.append((new_seq, new_score))
        
        # Keep top-B sequences
        all_candidates.sort(key=lambda x: x[1], reverse=True)
        beams = all_candidates[:B]
        
        if len(completed) >= B:
            break
    
    # Return best completed sequence (or best beam if none completed)
    completed.extend(beams)
    completed.sort(key=lambda x: x[1] / x[0].shape[1], reverse=True)  # Length normalize
    
    return completed[0][0]
```

## Beam Search: When to Use

- **Machine translation**: beam search is standard (B=4-8)
- **Summarization**: beam search with length penalty
- **Code generation**: sometimes (deterministic outputs needed)
- **Chat/creative**: NEVER (produces bland, generic text)

> ⚠️ **Why NOT for Chat/Creative**: Beam search optimizes for the HIGHEST PROBABILITY sequence. But the highest probability sequence is usually the most GENERIC one. "I don't know" has higher probability than any specific creative answer. For machine translation, there's usually ONE correct translation, so high probability = correct. For creative writing, high probability = boring.

### ⭐ Beam Search Complexity

| Beam Width ($B$) | Sequences Tracked | Quality vs. Greedy | Speed vs. Greedy |
|---------|-------------------|-------------------|-----------------|
| 1 | 1 | = Greedy decoding | 1x |
| 4 | 4 | Much better translations | ~4x slower |
| 8 | 8 | Diminishing returns | ~8x slower |
| 16+ | 16+ | Barely better, sometimes worse! | Very slow |

> 📚 **Research Note**: Surprisingly, larger beam widths can actually HURT quality for open-ended generation. This is called the "beam search curse" — see Stahlberg & Byrne (2019), *"On NMT Search Errors and Model Errors."* The highest-probability sequence is often degenerate (empty or repetitive), and larger beams are better at FINDING that degenerate optimum!

---

# 8️⃣ Repetition Penalties

## Problem: LLMs tend to repeat themselves

> 🔁 **Beginner Analogy — The "Don't Repeat Yourself" Rule** 📝: Imagine you're telling a story and you keep saying "and then... and then... and then..." Your friend would stop you and say "You already said that! Say something new!" That's exactly what repetition penalties do — they tell the model "you already used that word, pick something else!"

**Why do LLMs repeat?** During training, common phrases appear thousands of times. The model learns STRONG preferences for common words and phrases. Without penalties, it can get "stuck" in a loop where each repeated word makes the NEXT repetition even more likely (a positive feedback loop 📈➡️💥).

### Frequency Penalty
Reduce logit based on how many times a token has appeared:

$$z'_i = z_i - \alpha \cdot \text{count}(x_i)$$

Where $\alpha$ is the penalty strength and $\text{count}(x_i)$ is how many times token $x_i$ appeared in the generated text so far.

> 💡 **Plain English**: The MORE times a word has been used, the MORE its score gets reduced. Saying "the" once is fine. Saying it 10 times? Big penalty!

```python
def apply_frequency_penalty(logits, generated_ids, penalty=0.5):
    token_counts = Counter(generated_ids.tolist())
    for token_id, count in token_counts.items():
        logits[0, token_id] -= penalty * count
    return logits
```

### Presence Penalty
Reduce logit if token has appeared at all:

$$z'_i = z_i - \beta \cdot \mathbf{1}[x_i \in \text{generated}]$$

Where $\beta$ is the penalty, and $\mathbf{1}[\cdot]$ is 1 if the token has appeared, 0 otherwise.

> 💡 **Plain English**: If a word has been used even ONCE before, subtract a fixed penalty. This encourages the model to use new vocabulary rather than recycling the same words.

```python
def apply_presence_penalty(logits, generated_ids, penalty=0.5):
    unique_tokens = set(generated_ids.tolist())
    for token_id in unique_tokens:
        logits[0, token_id] -= penalty
    return logits
```

### No-Repeat N-Gram
Prevent exact repetition of n-grams:
```python
def prevent_ngram_repeat(logits, generated_ids, n=3):
    if len(generated_ids[0]) < n:
        return logits
    last_ngram = tuple(generated_ids[0, -(n-1):].tolist())
    for i in range(len(generated_ids[0]) - n + 1):
        if tuple(generated_ids[0, i:i+n-1].tolist()) == last_ngram:
            banned_token = generated_ids[0, i+n-1].item()
            logits[0, banned_token] = float('-inf')
    return logits
```

### ⭐ Frequency vs. Presence Penalty — When to Use Which?

| Penalty Type | Effect | Best For | Example |
|-------------|--------|----------|---------|
| **Frequency** | Proportional to count | Reducing overused words gradually | "The the the" → each "the" gets harder |
| **Presence** | Binary (used or not) | Encouraging topic diversity | Used "Python" once? Try "JavaScript" |
| **N-gram** | Hard ban on exact repeats | Preventing loops | "I think I think I think" → BLOCKED |

> 🏭 **Production Note**: OpenAI's API exposes both `frequency_penalty` (range -2.0 to 2.0) and `presence_penalty` (range -2.0 to 2.0) as separate parameters. Negative values ENCOURAGE repetition — useful for tasks like poetry with refrains or songs with choruses!

---

# 9️⃣ Generation Parameter Guide

> 🎮 **Think of this as your "Settings Menu" for AI text generation**. Just like a video game has difficulty settings, AI has generation settings. Here's your cheat sheet for every scenario:

| Use Case | Temperature | Top-p | Top-k | Notes |
|----------|------------|-------|-------|-------|
| Factual Q&A | 0.0-0.3 | 0.5 | 10 | Deterministic preferred |
| Code generation | 0.0-0.2 | 0.8 | 40 | Low creativity, high precision |
| Chat | 0.7 | 0.9 | 50 | Balanced |
| Creative writing | 0.9-1.2 | 0.95 | 100 | More diversity |
| Brainstorming | 1.0-1.5 | 0.99 | — | Maximum diversity |

### 🎯 Decision Flowchart (How to Pick Parameters)

```
Is the output factual/structured?
├── YES → Temperature 0.0-0.3, Top-p 0.5-0.8
│         (Math, code, data extraction, classification)
└── NO → Is creativity important?
         ├── YES → Temperature 0.8-1.2, Top-p 0.95
         │         (Stories, poems, brainstorming)
         └── SOMEWHAT → Temperature 0.6-0.8, Top-p 0.9
                        (Chat, email drafting, explanations)
```

### 🚨 Common Mistakes & Fixes

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Output is repetitive/boring | Temperature too low | Increase T to 0.7+ |
| Output is gibberish/nonsensical | Temperature too high | Decrease T to 0.5-0.7 |
| Output keeps repeating phrases | No repetition penalty | Add frequency_penalty ~0.5 |
| Output is safe/generic | Top-p too low | Increase top-p to 0.9-0.95 |
| Output has random wrong words | Top-k too high or no top-p | Reduce top-k, add top-p=0.9 |
| Output cuts off mid-sentence | max_tokens too low | Increase max_tokens |

---

# 📝 Summary

| Strategy | Key Property | Analogy |
|----------|-------------|---------|
| Greedy | Deterministic, repetitive, locally optimal | 🍽️ Always picking the most popular restaurant |
| Temperature | Controls distribution sharpness (creativity dial) | 🌡️ Volume knob for creativity |
| Top-k | Fixed-size candidate pool | 🔝 Only looking at top-k restaurants on Yelp |
| Top-p (Nucleus) | Adaptive candidate pool based on cumulative probability | 🎯 Looking at restaurants until 90% confident |
| Beam search | Explores multiple sequences, good for translation | 🔦 Sending multiple scouts through a maze |
| Repetition penalty | Prevents degenerate repetition loops | 🔁 "Don't repeat yourself" rule |

---

# 🔬 10️⃣ Advanced Topics & Frontier Research

## Speculative Decoding ⚡ (Leviathan et al., 2023)

> 🏎️ **Beginner Analogy**: Imagine you have a slow, brilliant expert (the big model) and a fast, decent assistant (the small model). Instead of waiting for the expert to write every word, the assistant DRAFTS several words quickly, and the expert just checks "yep, yep, yep, nope — I'd change that one." This is 2-3x faster because the assistant is often right!

**How it works**:
1. A small "draft" model generates $K$ candidate tokens quickly
2. The large "target" model verifies all $K$ tokens in ONE forward pass (parallel!)
3. Accepted tokens are kept; the first rejected token is resampled from the target model
4. Mathematically IDENTICAL output to the target model alone — just faster!

$$\text{Speedup} \approx \frac{1}{1 - \gamma} \text{ where } \gamma = \text{acceptance rate}$$

> 📄 Citation: Leviathan, Y., Kalman, M., & Matias, Y. (2023). "Fast Inference from Transformers via Speculative Decoding." *ICML 2023*.

⭐ **Production Usage**: Google uses speculative decoding in Gemini, and it's implemented in vLLM and TensorRT-LLM for serving open-source models.

---

## AI Text Watermarking 🔏 (Kirchenbauer et al., 2023)

> 🔍 **Beginner Analogy**: Imagine secretly marking every 5th word in a way that's invisible to readers but detectable by a computer. That's text watermarking — the AI subtly biases its word choices so that a detector can later say "this was AI-generated" with high confidence, even if someone slightly edits the text.

**How it works**:
1. At each generation step, split the vocabulary into a "green list" and "red list" using a hash of the previous token
2. Add a small bias $\delta$ to green-list token logits: $z'_i = z_i + \delta$ if $i \in \text{green}$
3. The text reads naturally, but statistically has MORE green-list tokens than expected
4. A detector counts green-list tokens: if significantly above 50%, the text is watermarked

$$\text{z-score} = \frac{|s|_G - T/2}{\sqrt{T/4}}$$

Where $|s|_G$ is the number of green-list tokens and $T$ is total tokens.

> 📄 Citation: Kirchenbauer, J., Geiping, J., Wen, Y., Katz, J., Mironov, I., & Goldstein, T. (2023). "A Watermark for Large Language Models." *ICML 2023*.

---

## Contrastive Decoding (Li et al., 2023)

A newer approach: Use a LARGE model and a SMALL model together. Generate tokens that the large model likes but the small model doesn't — this tends to produce more interesting, less generic text:

$$\text{score}(x) = \log P_{\text{large}}(x) - \log P_{\text{small}}(x)$$

> 📄 Citation: Li, X. L., Holtzman, A., Fried, D., Liang, P., Eisner, J., Hashimoto, T., ... & Zettlemoyer, L. (2023). "Contrastive Decoding: Open-ended Text Generation as Optimization." *ACL 2023*.

---

# 📋 11️⃣ ULTIMATE CHEAT SHEET

## ⚡ Quick Reference Card

```
┌─────────────────────────────────────────────────────┐
│           TEXT GENERATION CHEAT SHEET                │
├─────────────────────────────────────────────────────┤
│                                                     │
│  TEMPERATURE (T):                                   │
│    T → 0   = Greedy (safe, boring) ❄️               │
│    T = 0.7 = Sweet spot (most apps) 🎯              │
│    T = 1.0 = As trained 📊                          │
│    T > 1.0 = Creative/risky 🔥                      │
│                                                     │
│  TOP-P (nucleus):                                   │
│    p = 0.9 = Industry default ⭐                    │
│    p < 0.5 = Very focused                           │
│    p > 0.95 = Very diverse                          │
│                                                     │
│  TOP-K:                                             │
│    k = 50  = Reasonable default                     │
│    k = 1   = Greedy                                 │
│    Usually top-p is preferred over top-k            │
│                                                     │
│  REPETITION PENALTY:                                │
│    frequency_penalty = 0.0-0.5 for most tasks       │
│    presence_penalty = 0.0-0.5 for diversity         │
│    no_repeat_ngram = 3 to prevent loops             │
│                                                     │
│  BEAM SEARCH:                                       │
│    Use for: Translation, summarization              │
│    B = 4-8 is standard                              │
│    NEVER for chat or creative writing               │
│                                                     │
│  GOLDEN RULE:                                       │
│    Start with T=0.7, top-p=0.9                      │
│    Adjust based on output quality                   │
│    Lower T for precision, raise for creativity      │
└─────────────────────────────────────────────────────┘
```

## 🧪 Formula Reference

| Formula | LaTeX | What It Does |
|---------|-------|-------------|
| Temperature Scaling | $P(x_i) = \frac{\exp(z_i / T)}{\sum_j \exp(z_j / T)}$ | Adjusts randomness of token selection |
| Greedy Decoding | $x_t = \arg\max_x P(x \mid x_{1:t-1})$ | Always picks highest-probability token |
| Top-k | Keep top-$k$ tokens, zero out rest | Fixed-size candidate filtering |
| Top-p | $\sum_{x_i \in V^{(p)}} P(x_i) \geq p$ | Adaptive candidate filtering |
| Beam Score | $\frac{1}{t^\alpha} \sum_{i=1}^t \log P(y_i \mid y_{1:i-1})$ | Length-normalized sequence scoring |
| Frequency Penalty | $z'_i = z_i - \alpha \cdot \text{count}(x_i)$ | Penalizes proportional to usage count |
| Presence Penalty | $z'_i = z_i - \beta \cdot \mathbf{1}[x_i \in \text{generated}]$ | Binary penalty for any prior usage |

## 📚 Key Research Papers

| Paper | Authors | Year | Contribution |
|-------|---------|------|-------------|
| *The Curious Case of Neural Text Degeneration* | Holtzman et al. | 2020 (ICLR) | Introduced nucleus (top-p) sampling |
| *Fast Inference via Speculative Decoding* | Leviathan et al. | 2023 (ICML) | 2-3x speedup with draft+verify |
| *A Watermark for Large Language Models* | Kirchenbauer et al. | 2023 (ICML) | Invisible statistical watermarking |
| *Hierarchical Neural Story Generation* | Fan et al. | 2018 (ACL) | Introduced top-k sampling for generation |
| *Contrastive Decoding* | Li et al. | 2023 (ACL) | Large-vs-small model scoring |
| *On NMT Search Errors and Model Errors* | Stahlberg & Byrne | 2019 (EMNLP) | Beam search curse analysis |

## 🧠 Key Takeaways for Absolute Beginners

1. 🎯 **LLMs don't "think" about what to write** — they produce a probability for EVERY word and then pick one
2. 🌡️ **Temperature is the #1 parameter** — it's the creativity dial. Low = precise, High = creative
3. 🏆 **Top-p is the industry standard** — it adapts to the model's confidence at each step
4. 🔁 **Repetition penalties are essential** — without them, models loop embarrassingly
5. 🔦 **Beam search is for translation, NOT chat** — it finds "safe" text, which is boring for conversation
6. ⚡ **Speculative decoding is the future** — same output, 2-3x faster. All major companies are adopting it
7. 🎮 **Start with T=0.7, top-p=0.9** — this is the "normal mode" that works for 80% of use cases

**Tomorrow**: Evaluating Language Models — Perplexity on benchmarks, human evaluation, and automated metrics.
