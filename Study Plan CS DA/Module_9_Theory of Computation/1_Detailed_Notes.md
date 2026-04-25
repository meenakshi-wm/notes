# 🧠 Theory of Computation — Detailed Notes for GATE 2027 🎯

> 🌟 **"TOC answers the deepest question in CS: What CAN and CANNOT be computed? It's the philosophy of Computer Science!"**

> 💡 **Beginner Analogy — The Power Levels of Machines 🎮:**
> Think of computational models as characters in a video game with increasing power levels:
> 
> | Level 🎮 | Machine | What It Can Do | Real-World Example |
> |---|---|---|---|
> | 🟢 Level 1 | **DFA/NFA** (Finite Automata) | Simple pattern matching | Vending machines, Traffic lights, Regex |
> | 🟡 Level 2 | **PDA** (Pushdown Automata) | Matching brackets, parsing | Programming language compilers |
> | 🟠 Level 3 | **Turing Machine** | ANYTHING computable! | Your laptop, every computer ever |
> | 🔴 Level ??? | **Beyond TM** | The IMPOSSIBLE! | Halting problem (no computer can solve!) |
>
> Each level is STRICTLY more powerful than the previous. And the ultimate discovery: there are problems that **NO COMPUTER CAN EVER SOLVE**, no matter how powerful! 🤯

---

## 🏭 Where is TOC Used in the Real World?

| 🌍 Application | 🔧 How | 📌 TOC Concept |
|---|---|---|
| **Regex in every IDE/editor** 📝 | Find & Replace, syntax highlighting | Regular languages, Finite Automata |
| **grep/awk/sed (Linux tools)** 🐧 | Text pattern matching | Regular expressions |
| **Every programming language compiler** ⚙️ | Parsing source code | CFGs, PDAs (LL/LR parsers) |
| **XML/HTML validation** 🌐 | Checking well-formed documents | Context-Free Grammars |
| **Network protocol verification** 📶 | TCP state machine | Finite State Machines |
| **Cybersecurity** 🔒 | Proving system correctness | Decidability, Verification |
| **AI Limitation Theory** 🤖 | What AI fundamentally CANNOT do | Undecidability, Rice's theorem |
| **Cryptocurrency** ₿ | Smart contract verification | Halting problem (why bugs can't be fully auto-detected) |

> **GATE Weightage:** 4–6 marks (2–3 questions) | **Priority:** High 🔥
> **Time to Spend:** 5–6 weeks | **Difficulty:** Hard ⚠️
> **Topics:** Regular Languages, CFLs, Turing Machines, Decidability

---

## Table of Contents
1. [Finite Automata & Regular Languages](#1-finite-automata--regular-languages)
2. [Regular Expressions](#2-regular-expressions)
3. [Non-Regular Languages & Pumping Lemma](#3-non-regular-languages--pumping-lemma)
4. [Context-Free Grammars & Languages](#4-context-free-grammars--languages)
5. [Pushdown Automata](#5-pushdown-automata)
6. [Closure & Decision Properties](#6-closure--decision-properties)
7. [Turing Machines & Decidability](#7-turing-machines--decidability)

---

## 1. Finite Automata & Regular Languages 🔄

### DFA (Deterministic Finite Automaton) 🎯

> 💡 **Beginner Analogy — The Vending Machine 🍫:**
> A DFA is EXACTLY like a vending machine!
> - **States** = The different "modes" the machine is in (idle, received ₹10, received ₹20, dispensing...)
> - **Inputs** = Coins you insert (₹5, ₹10) or buttons you press
> - **Transitions** = "If I'm in state 'received ₹10' and I get another ₹10, go to state 'received ₹20'"
> - **Final state** = "Dispense the item!" 🍾
>
> The machine reacts DETERMINISTICALLY — for every state and every input, there's EXACTLY ONE thing it does. No confusion, no choices!
>
> 🏭 **Production Examples:**
> - **TCP/IP Protocol** 🌐: Network connections follow a state machine (SYN, SYN-ACK, ACK...)
> - **UI Navigation** 📱: Mobile app screens = states, button presses = transitions
> - **Game Characters** 🎮: Enemy AI (patrol → alert → chase → attack) = FSM!
> - **Turnstiles** 🚶: Locked/Unlocked states based on coin/push inputs

**Definition:** M = (Q, Σ, δ, q₀, F)
- Q: finite set of states
- Σ: input alphabet
- δ: Q × Σ → Q (transition function — exactly one transition per symbol per state)
- q₀: start state
- F ⊆ Q: set of accepting (final) states

A string w is **accepted** if δ*(q₀, w) ∈ F.

**Key Property:** For every state and every input symbol, there is **exactly one** next state.

### DFA Design Techniques ⭐⭐⭐

> 💡 **GATE Insight:** DFA design is one of the most frequently tested topics. The trick is to figure out what information the DFA needs to "remember" — that becomes your states!

#### Technique 1: "What does the DFA need to remember?"

The states of a DFA represent the **essential information** needed to decide acceptance. Think about what finite information suffices.

**Example 1:** L = strings over {0,1} where the number of 1's is divisible by 3

What to remember: count of 1's **mod 3** (only 3 possible values: 0, 1, 2)

```
States: q₀ (count≡0), q₁ (count≡1), q₂ (count≡2)
Start: q₀, Final: {q₀}

Transitions:
δ(q₀, 0) = q₀    δ(q₀, 1) = q₁
δ(q₁, 0) = q₁    δ(q₁, 1) = q₂
δ(q₂, 0) = q₂    δ(q₂, 1) = q₀

    ┌──0──┐   ┌──0──┐   ┌──0──┐
    ↓     │   ↓     │   ↓     │
→ ((q₀)) ──1──→ (q₁) ──1──→ (q₂)
    ↑                          │
    └──────────1───────────────┘
```

**Minimum states = 3** (one per equivalence class mod 3)

**Example 2:** L = strings over {a,b} ending in "ab"

What to remember: the last 1-2 characters seen

```
States: q₀ (haven't seen relevant suffix), q₁ (last char was 'a'), q₂ (last two chars were 'ab')
Start: q₀, Final: {q₂}

δ(q₀, a) = q₁    δ(q₀, b) = q₀
δ(q₁, a) = q₁    δ(q₁, b) = q₂
δ(q₂, a) = q₁    δ(q₂, b) = q₀
```

**Minimum states = 3**

**Example 3:** L = strings over {0,1} containing substring "101"

What to remember: longest suffix matching a prefix of "101"

```
States: q₀ (no match), q₁ (seen "1"), q₂ (seen "10"), q₃ (seen "101" — ACCEPT!)
Start: q₀, Final: {q₃}

δ(q₀, 0) = q₀    δ(q₀, 1) = q₁
δ(q₁, 0) = q₂    δ(q₁, 1) = q₁
δ(q₂, 0) = q₀    δ(q₂, 1) = q₃    ← found "101"!
δ(q₃, 0) = q₃    δ(q₃, 1) = q₃    ← trap state (already accepted)
```

**Minimum states = 4** (this is a string-search automaton — related to KMP!)

> 🎯 **GATE Trick:** For "contains substring w" problems, the states track the longest suffix of input matching a prefix of w. The number of states = |w| + 1.

**Example 4:** L = strings over {0,1} where every 0 is immediately followed by 11

Valid: ε, 11, 011, 011011, 111
Invalid: 0, 01, 010, 0110

```
States: q₀ (OK, expecting any), q₁ (just saw 0, need first 1), q₂ (saw 01, need second 1), q_dead (invalid)
Start: q₀, Final: {q₀}

δ(q₀, 0) = q₁    δ(q₀, 1) = q₀
δ(q₁, 0) = q_dead δ(q₁, 1) = q₂
δ(q₂, 0) = q₁    δ(q₂, 1) = q₀
δ(q_dead, 0) = q_dead  δ(q_dead, 1) = q_dead
```

**Minimum states = 4**

#### Technique 2: Product Construction for Complex Conditions

When L involves **multiple conditions** (AND/OR), build DFAs for each condition separately, then combine.

**Example:** L = strings over {0,1} where #0's is even AND #1's is odd

DFA₁ for "even 0's": 2 states (e₀, o₀)
DFA₂ for "odd 1's": 2 states (e₁, o₁)

Product: 4 states = (e₀,e₁), (e₀,o₁), (o₀,e₁), (o₀,o₁)

| State | 0 → | 1 → |
|---|---|---|
| (e₀,e₁) start | (o₀,e₁) | (e₀,o₁) |
| (e₀,o₁) **final** | (o₀,o₁) | (e₀,e₁) |
| (o₀,e₁) | (e₀,e₁) | (o₀,o₁) |
| (o₀,o₁) | (e₀,o₁) | (o₀,e₁) |

Final state: (e₀,o₁) — even 0's AND odd 1's

> 🎯 **GATE Trick:** Product construction for AND = intersection of final states. For OR = union of final states. Number of states in product = |Q₁| × |Q₂|.

#### DFA State Count Quick Reference ⭐

| Language Description | Min DFA States | Reasoning |
|---|---|---|
| Σ* (accept everything) | 1 | Single accepting state |
| ∅ (reject everything) | 1 | Single non-accepting state |
| {ε} (only empty string) | 2 | Start is final, all input → dead |
| Strings ending in 'a' | 2 | Seen 'a' last or not |
| Strings with #a mod k = r | k | One state per residue |
| Strings containing w (|w|=m) | m+1 | Track prefix match |
| Strings NOT containing w | m+1 | Same DFA, swap final/non-final |
| Even length strings | 2 | Track parity |
| Multiples of n in binary | n | Track value mod n |
| #a mod p = r₁ AND #b mod q = r₂ | p × q | Product construction |

### NFA Design Techniques ⭐⭐

> 💡 **Analogy:** NFA is like "guessing" the right path. Imagine you're in a maze and can clone yourself at every fork — if ANY clone reaches the exit, you win! 🎮

**Key advantages of NFA over DFA for design:**
- Can have **multiple transitions** from one state on same symbol
- Can have **ε-transitions** (move without reading input)
- Can "guess" where a pattern starts

**Example 1:** L = strings over {a,b} ending in "aba"

NFA (much simpler than DFA):
```
→ q₀ ──a──→ q₁ ──b──→ q₂ ──a──→ ((q₃))
  │                                  
  └──a,b──→ q₀  (self-loop on q₀)
```

- q₀: stay here reading anything (self-loop)
- When you see 'a', "guess" this might be start of "aba" → go to q₁
- If guess is wrong, die on that branch (no transition)

**NFA has 4 states.** Equivalent DFA also needs 4 states (for this particular language).

**Example 2:** L = strings containing "01" OR "10" (union)

Build NFA for "contains 01" and NFA for "contains 10", connect with ε-transitions:
```
                ε
→ q₀ ──────→ [NFA for "01"]
  │
  └────ε───→ [NFA for "10"]
```

> 🎯 **GATE Trick:** NFA for union: add new start state with ε-transitions to both sub-NFAs. This is Thompson's construction!

### NFA to DFA Conversion — Complete Worked Example ⭐⭐⭐

**Given NFA:**
- States: {q₀, q₁, q₂}
- Σ = {a, b}
- Start: q₀, Final: {q₂}
- Transitions:
  - δ(q₀, a) = {q₀, q₁}
  - δ(q₀, b) = {q₀}
  - δ(q₁, a) = ∅
  - δ(q₁, b) = {q₂}
  - δ(q₂, a) = ∅
  - δ(q₂, b) = ∅

**Subset Construction:**

| DFA State | On 'a' | On 'b' | Final? |
|---|---|---|---|
| {q₀} | {q₀, q₁} | {q₀} | No |
| {q₀, q₁} | {q₀, q₁} | {q₀, q₂} | No |
| {q₀, q₂} | {q₀, q₁} | {q₀} | **Yes** (contains q₂) |

**Result:** 3 DFA states (not 2³=8). Language: strings ending in "ab".

**Step-by-step process:**
1. Start with {q₀}
2. Compute {q₀} on 'a': ∪{δ(q₀,a)} = {q₀,q₁} — new state!
3. Compute {q₀} on 'b': ∪{δ(q₀,b)} = {q₀} — already exists
4. Compute {q₀,q₁} on 'a': δ(q₀,a) ∪ δ(q₁,a) = {q₀,q₁} ∪ ∅ = {q₀,q₁}
5. Compute {q₀,q₁} on 'b': δ(q₀,b) ∪ δ(q₁,b) = {q₀} ∪ {q₂} = {q₀,q₂} — new state!
6. Compute {q₀,q₂} on 'a': δ(q₀,a) ∪ δ(q₂,a) = {q₀,q₁} ∪ ∅ = {q₀,q₁}
7. Compute {q₀,q₂} on 'b': δ(q₀,b) ∪ δ(q₂,b) = {q₀} ∪ ∅ = {q₀}
8. No new states → done!

### ε-Closure: Complete Worked Example ⭐⭐

**Given ε-NFA:**
- States: {A, B, C, D}
- ε-transitions: A→ε→B, B→ε→C

**Computing ε-closures:**
- ε-closure(A) = {A, B, C} (A →ε→ B →ε→ C)
- ε-closure(B) = {B, C} (B →ε→ C)
- ε-closure(C) = {C}
- ε-closure(D) = {D}

**For a set:** ε-closure({A,D}) = ε-closure(A) ∪ ε-closure(D) = {A,B,C,D}

**NFA with ε-transitions to DFA:**
After computing move(state_set, symbol), always take ε-closure of the result!

δ'(S, a) = ε-closure(∪_{q∈S} δ(q, a))

### DFA Minimization — Complete Worked Example ⭐⭐⭐

**Given DFA:**
- States: {A, B, C, D, E, F}
- Σ = {0, 1}
- Start: A, Final: {C, D, F}

| State | δ(·,0) | δ(·,1) |
|---|---|---|
| A | B | C |
| B | A | D |
| C | E | F |
| D | E | F |
| E | B | C |
| F | B | C |

**Table-Filling Algorithm:**

**Step 1:** Mark pairs where one is final and other is non-final:
- Final: {C, D, F}, Non-final: {A, B, E}
- Mark: (A,C), (A,D), (A,F), (B,C), (B,D), (B,F), (E,C), (E,D), (E,F)

**Step 2:** Process unmarked pairs:

**(A,B):** δ(A,0)=B, δ(B,0)=A → pair (A,B) — unmarked. δ(A,1)=C, δ(B,1)=D → pair (C,D) — unmarked. So (A,B) stays unmarked.

**(A,E):** δ(A,0)=B, δ(E,0)=B → same. δ(A,1)=C, δ(E,1)=C → same. (A,E) unmarked.

**(B,E):** δ(B,0)=A, δ(E,0)=B → pair (A,B) — unmarked. δ(B,1)=D, δ(E,1)=C → pair (C,D) — unmarked. (B,E) unmarked.

**(C,D):** δ(C,0)=E, δ(D,0)=E → same. δ(C,1)=F, δ(D,1)=F → same. (C,D) unmarked.

**(C,F):** δ(C,0)=E, δ(F,0)=B → pair (B,E) — unmarked. δ(C,1)=F, δ(F,1)=C → pair (C,F) — this is the pair itself! (C,F) unmarked.

**(D,F):** δ(D,0)=E, δ(F,0)=B → pair (B,E) — unmarked. δ(D,1)=F, δ(F,1)=C → pair (C,F) — unmarked. (D,F) unmarked.

**Step 3:** No more pairs to mark. Merge indistinguishable pairs:
- {A, B, E} all equivalent → merge into one state
- {C, D, F} all equivalent → merge into one state

**Minimized DFA:** 2 states! {ABE} and {CDF}
- Start: {ABE}, Final: {CDF}
- δ({ABE}, 0) = {ABE}, δ({ABE}, 1) = {CDF}
- δ({CDF}, 0) = {ABE}, δ({CDF}, 1) = {CDF}

**Language:** strings ending in 1, or starting with 1, or... Actually: strings where last symbol is 1 → L = (0+1)*1

> 🎯 **GATE Trick:** In DFA minimization, if the table-filling algorithm leaves many pairs unmarked, the minimal DFA is very small. If almost all pairs get marked, the DFA is already near-minimal.

### Moore & Mealy Machines ⭐⭐

> 💡 **Note:** These are finite automata **with output** (transducers), not recognizers. They don't accept/reject — they produce output!

| Feature | Moore Machine | Mealy Machine |
|---|---|---|
| Output depends on | **Current state only** | **Current state + input** |
| Output function | λ: Q → Δ | λ: Q × Σ → Δ |
| Output timing | Associated with state | Associated with transition |
| Output delay | 1 clock delay (output after entering state) | No delay (output with transition) |
| States needed | Usually MORE | Usually FEWER |
| Equivalence | For every Moore, ∃ equivalent Mealy | For every Mealy, ∃ equivalent Moore |

**Moore Machine Definition:** M = (Q, Σ, Δ, δ, λ, q₀)
- Δ: output alphabet
- λ: Q → Δ (output function — depends on state only)

**Mealy Machine Definition:** M = (Q, Σ, Δ, δ, λ, q₀)
- λ: Q × Σ → Δ (output function — depends on state AND input)

**Conversion: Moore → Mealy**
For each transition δ(q, a) = q': Mealy output λ'(q, a) = λ_Moore(q')
(Output of Mealy = output of the destination state in Moore)

**Conversion: Mealy → Moore**
For each state q receiving different outputs on different incoming transitions: split q into multiple states (one per distinct output).
- If state q has k distinct incoming outputs, create k copies: q/o₁, q/o₂, ...

**Worked Example:** Design Moore machine that outputs 1 whenever input sequence "01" is detected (on {0,1})

**Moore Machine:**
| State | Meaning | Output |
|---|---|---|
| S₀ | Initial/no relevant suffix | 0 |
| S₁ | Last input was '0' | 0 |
| S₂ | Last two inputs were '01' | 1 |

| State | δ(·,0) | δ(·,1) |
|---|---|---|
| S₀ | S₁ | S₀ |
| S₁ | S₁ | S₂ |
| S₂ | S₁ | S₀ |

Input: 0 1 1 0 1 0 0 1
States: S₀→S₁→S₂→S₀→S₁→S₂→S₁→S₁→S₂
Output: 0  0  1  0  0  1  0  0  1

> 🎯 **GATE Trick:** Moore outputs are delayed by one step compared to Mealy. For sequence detection, Moore "announces" the detection one step after the pattern completes.

### Complement, Reversal, and Other Operations on DFA ⭐⭐

**Complement of DFA:** Simply swap final and non-final states!
- L' recognized by DFA with same transitions, F' = Q − F
- **Only works for DFA (not NFA!)** — swapping final states of NFA does NOT give complement

**Reversal of DFA:**
1. Reverse all transitions
2. Make old start state the only final state
3. Create new start state with ε-transitions to all old final states
4. Result is an NFA (may need to determinize)

**Right quotient:** L₁/L₂ = {x | ∃y ∈ L₂ such that xy ∈ L₁}
- If L₁ is regular, L₁/L₂ is regular (even if L₂ is not regular!)

**Left quotient:** L₂\L₁ = {y | ∃x ∈ L₂ such that xy ∈ L₁}
- If L₁ is regular, L₂\L₁ is regular

> 🎯 **GATE Trick:** Quotient with ANY language (even non-regular, non-RE) preserves regularity! This is a powerful closure property.

### Minimum Number of States — Counting Arguments ⭐⭐⭐

**Myhill-Nerode Approach for counting:**

For language L, define equivalence relation ≡_L on Σ*:
x ≡_L y iff ∀z ∈ Σ*: xz ∈ L ↔ yz ∈ L

**Minimum DFA states = number of equivalence classes of ≡_L**

**Worked Example:** L = {w ∈ {0,1}* | w has an even number of 0's}

Equivalence classes:
- Class 1: strings with even #0's → any suffix z: wz has even #0's iff z has even #0's
- Class 2: strings with odd #0's → any suffix z: wz has even #0's iff z has odd #0's

These two classes are distinguishable (z = 0 distinguishes them). No finer partition needed.
**Minimum states = 2** ✓

**Worked Example:** L = {aⁿ | n mod 5 = 0}

Equivalence classes: ε ≡ aaaaa, a ≡ aaaaaa, aa ≡ aaaaaaa, ...
5 classes: {aⁿ | n mod 5 = 0}, {aⁿ | n mod 5 = 1}, ..., {aⁿ | n mod 5 = 4}
**Minimum states = 5** ✓

**Worked Example:** L = binary numbers divisible by n
**Minimum states = n** (one per remainder mod n)

> 🎯 **GATE Trick:** For "mod k" type languages, the minimum DFA always has exactly k states. For "contains substring w of length m", the minimum DFA has m+1 states.

### Important Counting Results ⭐

| Question | Answer |
|---|---|
| Number of DFAs with n states over alphabet Σ | n^(n|Σ|) × 2ⁿ (transitions × final state choices) |
| Number of strings of length n over Σ | \|Σ\|ⁿ |
| Number of languages over Σ | Uncountable (2^(Σ*)) |
| Number of regular languages over Σ | Countably infinite |
| Max DFA states from n-state NFA | 2ⁿ (tight — achievable) |
| Min DFA states is unique? | **Yes** (Myhill-Nerode theorem) |

### NFA (Non-deterministic Finite Automaton)
**Definition:** M = (Q, Σ, δ, q₀, F)
- δ: Q × (Σ ∪ {ε}) → P(Q) (power set — multiple/zero next states, ε-transitions)

A string is accepted if **any** computation path leads to a final state.

### NFA to DFA Conversion (Subset Construction)
**Key Idea:** Each DFA state = subset of NFA states.
- DFA states ⊆ P(Q) → worst case: 2ⁿ DFA states for n-NFA states
- Start state: ε-closure(q₀)
- Transition: For subset S and input a, new state = ε-closure(∪_{q∈S} δ(q,a))
- Final: Any DFA state containing an NFA final state

**Important:** An NFA with n states → DFA may need **up to 2ⁿ** states (not always, but worst case is tight).

### ε-Closure
ε-closure(q) = set of all states reachable from q following only ε-transitions (including q itself).

### DFA Minimization (Myhill-Nerode / Table-Filling)

**Table-Filling Algorithm:**
1. Mark all pairs (p,q) where one is final and other is non-final as **distinguishable**
2. For each unmarked pair (p,q): if δ(p,a) and δ(q,a) are marked as distinguishable for any a ∈ Σ, mark (p,q) as distinguishable
3. Repeat step 2 until no more pairs can be marked
4. Merge all unmarked (indistinguishable) pairs

**Minimum states:** The quotient automaton M/≡ is the unique minimum DFA (up to isomorphism).

### Product Automata (Combining DFAs)
Given DFA₁ = (Q₁, Σ, δ₁, q₁, F₁) and DFA₂ = (Q₂, Σ, δ₂, q₂, F₂):

**Product DFA:** States = Q₁ × Q₂, transition δ((p,q), a) = (δ₁(p,a), δ₂(q,a))

| Operation | Final States |
|-----------|-------------|
| L₁ ∩ L₂ | F₁ × F₂ |
| L₁ ∪ L₂ | (F₁ × Q₂) ∪ (Q₁ × F₂) |
| L₁ − L₂ | F₁ × (Q₂ − F₂) |
| L₁ ⊕ L₂ (symmetric diff) | (F₁ × (Q₂−F₂)) ∪ ((Q₁−F₁) × F₂) |

**States in product:** |Q₁| × |Q₂| (not all may be reachable)

---

## 2. Regular Expressions

### Notation
| Symbol | Meaning |
|--------|---------|
| ε | Empty string |
| ∅ | Empty set/language |
| a | Single character |
| R₁R₂ | Concatenation |
| R₁ + R₂ (or R₁|R₂) | Union |
| R* | Kleene star (0 or more) |
| R⁺ | Kleene plus (1 or more) = RR* |

### Identities
- R + ∅ = R, R · ε = R
- R · ∅ = ∅, ∅* = ε
- R + R = R (idempotent)
- (R*)* = R*
- ε* = ε
- R⁺ = RR* = R*R

### Arden's Theorem
If R = Q + RP (where P doesn't contain ε), then **R = QP***.

**If P contains ε:** R = Q + RP has **infinitely many solutions**. The smallest solution is still QP*, but there are others of the form QP* + S for any S ⊆ Σ*.

Used to convert FA to regular expression:
1. Write state equations (one per state)
2. Solve using substitution and Arden's theorem

### Arden's Theorem — Complete Worked Example ⭐⭐⭐

**Given DFA:**
- States: {q₁, q₂, q₃}
- Start: q₁, Final: {q₃}
- Transitions: δ(q₁,a)=q₂, δ(q₁,b)=q₃, δ(q₂,a)=q₁, δ(q₂,b)=q₃, δ(q₃,a)=q₃, δ(q₃,b)=q₃

**Step 1: Write state equations**
q₁ = ε + q₂a (q₁ is start, reached from q₂ on 'a')
q₂ = q₁a (reached from q₁ on 'a')
q₃ = q₁b + q₂b + q₃a + q₃b (reached from q₁ on b, q₂ on b, or self on a,b)

**Step 2: Simplify q₃**
q₃ = q₁b + q₂b + q₃(a+b)
Apply Arden's: q₃ = (q₁b + q₂b)(a+b)*

**Step 3: Substitute q₂ = q₁a**
q₃ = (q₁b + q₁ab)(a+b)* = q₁(b + ab)(a+b)*

**Step 4: Solve q₁**
q₁ = ε + q₂a = ε + q₁aa
Apply Arden's: q₁ = (aa)*

**Step 5: Final answer**
q₃ = (aa)*(b + ab)(a+b)*

**RE for the language: (aa)*(b + ab)(a + b)***

**Verification:** String "ab" → q₁→q₂→q₃ ✓, RE: (aa)⁰(ab)(a+b)⁰ = ab ✓

### State Elimination Method — Complete Example ⭐⭐

**State elimination** is often easier than Arden's for GATE:

**Algorithm:**
1. Add new start state qs with ε-transition to original start
2. Add new final state qf with ε-transitions from all original final states
3. Eliminate states one by one (not qs or qf)
4. When eliminating state q: for each pair (qi, qj) going through q, replace with direct transition labeled with combined RE

**Elimination of state q with incoming edge R₁ from qi, self-loop R₂, outgoing edge R₃ to qj:**
New direct edge from qi to qj: **R₁ · R₂* · R₃**

**Worked Example:** Same DFA as above

Add qs →ε→ q₁, q₃ →ε→ qf

**Eliminate q₂:**
- Incoming: q₁ →a→ q₂
- Self-loop: none (∅)
- Outgoing: q₂ →a→ q₁, q₂ →b→ q₃

New edges: q₁ →aa→ q₁, q₁ →ab→ q₃

After eliminating q₂:
- qs →ε→ q₁, q₁ →aa→ q₁ (self-loop), q₁ →b→ q₃, q₁ →ab→ q₃, q₃ →(a+b)→ q₃, q₃ →ε→ qf

**Eliminate q₁:**
- Incoming: qs →ε→ q₁
- Self-loop: aa
- Outgoing: q₁ →b→ q₃, q₁ →ab→ q₃ → combined: q₁ →(b+ab)→ q₃

New edge: qs → ε·(aa)*·(b+ab) → q₃ = **(aa)*(b+ab)**

**Eliminate q₃:**
- Incoming: qs →(aa)*(b+ab)→ q₃
- Self-loop: (a+b)
- Outgoing: q₃ →ε→ qf

New edge: qs → (aa)*(b+ab)·(a+b)*·ε → qf = **(aa)*(b+ab)(a+b)***

**Same answer!** ✓

> 🎯 **GATE Trick:** State elimination is generally faster for exam. The order of elimination doesn't matter — you'll get an equivalent (possibly different-looking) RE each time.

### Thompson's Construction: RE → ε-NFA ⭐⭐

**Rules (each produces an NFA with one start and one final state):**

**Base cases:**
```
Empty string ε:  → (q₀) ──ε──→ ((qf))
Single char a:   → (q₀) ──a──→ ((qf))
```

**Union R₁ + R₂:**
```
        ε → [NFA₁] → ε
→ (q₀)                  → ((qf))
        ε → [NFA₂] → ε
```

**Concatenation R₁R₂:**
```
→ (q₀) → [NFA₁] → [NFA₂] → ((qf))
```
(Final of NFA₁ connects to start of NFA₂ via ε)

**Kleene Star R₁*:**
```
→ (q₀) ──ε──→ [NFA₁] ──ε──→ ((qf))
   │          ← ε (back loop) ← │
   └──────────ε (skip)───────────┘
```

**Properties of Thompson's NFA:**
- Exactly one start state (no incoming edges) and one final state (no outgoing edges)
- Each state has at most 2 outgoing edges
- Number of states ≤ 2 × (length of RE)

### RE Simplification Rules (Algebraic Laws) ⭐⭐

| Law | Rule |
|---|---|
| Commutativity of + | R + S = S + R |
| Associativity of + | (R + S) + T = R + (S + T) |
| Associativity of · | (RS)T = R(ST) |
| Identity for + | R + ∅ = R |
| Identity for · | Rε = εR = R |
| Annihilator for · | R∅ = ∅R = ∅ |
| Idempotency of + | R + R = R |
| Distributivity (left) | R(S + T) = RS + RT |
| Distributivity (right) | (S + T)R = SR + TR |
| Star identities | ∅* = ε, ε* = ε, (R*)* = R* |
| Star expansion | R* = ε + RR* = ε + R*R |
| Absorption | R*R* = R*, R + R*R = R*R = R⁺ |
| Star of union | (R + S)* = (R*S*)* |

**Important non-obvious identities:**
- (R*S)* = ε + (R + S)*S
- R(SR)* = (RS)*R
- (R + S)* = R*(SR*)* (key for proofs!)

### Common Regex Patterns for GATE ⭐⭐⭐

| Language | Regular Expression |
|---|---|
| All strings over {a,b} | (a+b)* |
| Strings starting with 'a' | a(a+b)* |
| Strings ending with 'b' | (a+b)*b |
| Strings containing 'ab' | (a+b)*ab(a+b)* |
| Strings NOT containing 'ab' | b*a* |
| Even length strings | ((a+b)(a+b))* |
| Odd length strings | (a+b)((a+b)(a+b))* |
| At least one 'a' | (a+b)*a(a+b)* = b*a(a+b)* |
| Exactly one 'a' | b*ab* |
| At most one 'a' | b* + b*ab* = b*(ε + ab*) |
| #a is even | (b*ab*ab*)* b* = (b + ab*a)* |
| Strings of form aⁿbⁿ | **NOT regular!** No RE exists |
| Binary multiples of 3 | Look up state-elimination on 3-state DFA |

> 🎯 **GATE Trick:** "Not containing pattern P" is often the complement. Design DFA for "contains P", then complement (swap final/non-final), then convert to RE using state elimination.

### Regular Expression ↔ FA — GATE Problem Types ⭐⭐

**Type 1: "How many strings of length n are accepted?"**
Use the DFA + matrix method:
- Transition matrix A where A[i][j] = number of symbols taking state i to j
- Number of strings of length n = (row vector for start) × Aⁿ × (column vector for final states)

**Worked Example:** DFA with 2 states {q₀, q₁}, Σ={a,b}, start q₀, final q₁
δ(q₀,a)=q₁, δ(q₀,b)=q₀, δ(q₁,a)=q₀, δ(q₁,b)=q₁

Transition matrix: A = [[1,1],[1,1]] (from each state, 1 symbol goes to each state)

Strings of length n accepted = number of paths from q₀ to q₁ of length n = 2ⁿ⁻¹ (for n≥1)

**Type 2: "Given RE, find minimum DFA states"**
Convert RE → NFA (Thompson's) → DFA (subset construction) → minimize (table filling)

Or use Myhill-Nerode directly: enumerate equivalence classes

### Regular Expression ↔ FA

**RE → NFA:** Thompson's construction (ε-NFA)
**FA → RE:** State elimination method or Arden's theorem

---

## 3. Non-Regular Languages & Pumping Lemma 💪

> 💡 **Analogy:** The pumping lemma is like testing a rubber band 🪢. If a language is "regular," any long enough string can be "stretched" (pumped) and still stay in the language. If stretching breaks it, the language isn't regular!

### Pumping Lemma for Regular Languages ⭐⭐⭐
If L is regular, then ∃ pumping length p such that for every string w ∈ L with |w| ≥ p:
w = xyz where:
1. |y| > 0 (y is non-empty)
2. |xy| ≤ p (prefix constraint — y is within first p chars!)
3. ∀i ≥ 0: xy^i z ∈ L (pumping up AND down works)

**Used to PROVE non-regularity** (by contradiction). **Cannot prove regularity!**

> 🎯 **GATE Trick:** The pumping lemma is a NECESSARY condition for regularity, not SUFFICIENT. Passing the pumping lemma doesn't mean a language IS regular!

### How to Use Pumping Lemma — Step-by-Step ⭐⭐⭐
1. **Assume** L is regular → ∃ pumping length p
2. **Choose** a specific w ∈ L with |w| ≥ p (**YOU pick strategically!**)
3. For **ALL** possible splits w = xyz with |y|>0, |xy|≤p:
4. **Find** some i (usually i=0 or i=2) where xy^i z ∉ L
5. **Contradiction** → L is not regular

> 🎯 **GATE Trick:** The adversary (pumping lemma) picks p and picks the split xyz. YOU choose w and choose i. Your job is to find a w that works against ANY split!

### Pumping Lemma Proofs — Complete Examples ⭐⭐⭐

**Example 1:** L = {aⁿbⁿ | n ≥ 0} ⭐ (Classic!)

1. Assume L is regular with pumping length p
2. Choose w = aᵖbᵖ ∈ L (length 2p ≥ p)
3. Any split xyz: since |xy| ≤ p, both x and y are entirely within the 'a' section
   - x = aˢ, y = aᵗ (t ≥ 1), z = aᵖ⁻ˢ⁻ᵗbᵖ
4. Pump i=0: xz = aᵖ⁻ᵗbᵖ, with t ≥ 1, so p−t < p → aᵖ⁻ᵗbᵖ ∉ L ✗
5. **Contradiction → L is not regular** ✓

**Example 2:** L = {w ∈ {a,b}* | #a(w) = #b(w)} ⭐⭐

1. Assume regular, pumping length p
2. Choose w = aᵖbᵖ (equal a's and b's)
3. |xy| ≤ p → y = aᵗ for some t ≥ 1
4. Pump i=2: xy²z = aᵖ⁺ᵗbᵖ, #a = p+t > p = #b → ∉ L ✗
5. **Not regular** ✓

**Example 3:** L = {aⁿ | n is prime} ⭐⭐⭐ (Hard!)

1. Assume regular, pumping length p
2. Choose w = aᵐ where m is a prime ≥ p+2 (such prime always exists)
3. Any split: y = aᵗ, t ≥ 1
4. Pump i = m+1: |xy^(m+1)z| = |w| + m·t = m + m·t = m(1+t)
5. Since m ≥ 2 and t ≥ 1: 1+t ≥ 2, so m(1+t) is composite → ∉ L ✗
6. **Not regular** ✓

> 🎯 **GATE Trick:** For L = {aⁿ | n has property P}, pump with i such that the resulting length is NOT in P. For primes, i=m+1 makes the length m(1+|y|) which is composite.

**Example 4:** L = {0ⁿ1ⁿ | n ≥ 0} ∪ {0ⁿ1²ⁿ | n ≥ 0} ⭐⭐

Is this regular? **No!** But the pumping lemma proof needs care because the language is a union.

Choose w = 0ᵖ1ᵖ ∈ L. Then y = 0ᵗ, pump i=0: 0ᵖ⁻ᵗ1ᵖ. Is this in L?
- For {0ⁿ1ⁿ}: need p−t = p? No (t≥1)
- For {0ⁿ1²ⁿ}: need p = 2(p−t) → p = 2t → only possible for specific t, not all t!
- Since this must work for ALL splits: adversary can pick t such that p ≠ 2t.
- Actually, if p is odd: p = 2t is impossible for integer t → ∉ L ✗

Better approach: Choose w = 0ᵖ1²ᵖ. Then y = 0ᵗ, pump i=0: 0ᵖ⁻ᵗ1²ᵖ.
- For {0ⁿ1ⁿ}: need p−t = 2p → impossible
- For {0ⁿ1²ⁿ}: need 2(p−t) = 2p → t = 0, but t ≥ 1 ✗
- **Not regular** ✓

**Example 5:** L = {aⁿbⁿcⁿ | n ≥ 0} ⭐

This is not even CFL! But prove non-regularity:
Choose w = aᵖbᵖcᵖ. y within first p chars → y = aᵗ. Pump i=0: aᵖ⁻ᵗbᵖcᵖ ∉ L ✗ (unequal counts)

### When Pumping Lemma Fails — Other Methods ⭐⭐

Sometimes the pumping lemma **cannot** prove non-regularity (even for non-regular languages):

**L = {ww | w ∈ {a,b}*}** — The pumping lemma is hard to apply directly!

**Alternative methods:**
1. **Myhill-Nerode theorem** (always works): Show ≡_L has infinitely many classes
   - For L = {ww}: the strings a, aa, aaa, ... are all pairwise distinguishable
   - aⁱ and aʲ (i≠j) are distinguished by suffix aⁱ: aⁱaⁱ ∈ L but aʲaⁱ ∉ L (since i≠j)
   - Infinitely many equivalence classes → not regular ✓

2. **Closure properties** (often elegant):
   If L were regular, then L ∩ a*b*a*b* would be regular. But:
   L ∩ a*b*a*b* = {aⁿbᵐaⁿbᵐ | n,m ≥ 0}
   Apply homomorphism h(a)=a, h(b)=ε: get {a²ⁿ | n ≥ 0}... this approach needs care.

> 🎯 **GATE Tip:** If pumping lemma proof seems impossible, try Myhill-Nerode or closure properties instead! GATE sometimes tests whether you know these alternatives.

### Myhill-Nerode Theorem — Complete Treatment ⭐⭐⭐

**Statement:** L is regular if and only if ≡_L has finitely many equivalence classes. Moreover, the number of equivalence classes = minimum DFA states.

**For proving non-regularity:**
Find an infinite set of strings that are pairwise distinguishable.

**Worked Example:** L = {aⁿbⁿ | n ≥ 0}

Consider strings: ε, a, aa, aaa, ...
- aⁱ and aʲ (i ≠ j) are distinguishable by z = bⁱ:
  - aⁱbⁱ ∈ L ✓
  - aʲbⁱ ∉ L (since j ≠ i) ✗
- So infinitely many equivalence classes → **not regular** ✓

**Worked Example:** L = set of palindromes over {a,b}

Consider: a, aa, aaa, aba, ab, ba, ...
- aⁱ and aʲ (i ≠ j) are distinguishable by z = baaⁱ:
  - aⁱbaaⁱ is a palindrome iff... hmm
  
Better: Consider strings aⁱb for different i.
- aⁱb and aʲb (i≠j) distinguished by z = aⁱ: aⁱbaⁱ is a palindrome (reverse = aⁱbaⁱ) ✓, aʲbaⁱ reverse = aⁱbaʲ ≠ aʲbaⁱ when i≠j ✗
- Infinitely many classes → palindromes are not regular ✓

### Common Non-Regular Languages — Proof Techniques ⭐⭐

| Language | Best Proof Method | Key Idea |
|---|---|---|
| {aⁿbⁿ} | Pumping Lemma | y in a-section, pump to break count |
| {ww} | Myhill-Nerode | Infinitely many distinguishable prefixes |
| {aⁿ \| n prime} | Pumping Lemma | Pump to get composite length |
| {aⁿ \| n = k²} | Pumping Lemma | Pump; gap between consecutive squares |
| Palindromes | Myhill-Nerode | aⁱb strings are pairwise distinguishable |
| {aⁿbᵐ \| n ≤ m} | Pumping Lemma | Choose aᵖbᵖ, pump y in a-section, i=2 |
| {aⁿ \| n = 2ᵏ} | Pumping Lemma | Pump; powers of 2 become too sparse |

### Proving a Language IS Regular ⭐⭐

Methods to show L is regular:
1. **Construct a DFA/NFA** for L
2. **Give a regular expression** for L
3. **Use closure properties:** build from known regular languages
4. **Myhill-Nerode:** show finitely many equivalence classes

**Example:** L = {w ∈ {0,1}* | w does NOT contain "001"}

This is the complement of "contains 001". Since regular languages are closed under complement, and "contains 001" = (0+1)*001(0+1)* is regular, L is regular.

We can also build a DFA directly with 4 states (tracking longest suffix matching prefix of "001").

---

## 4. Context-Free Grammars & Languages 🌳

> 💡 **Analogy:** A CFG is like a recipe book 📖. The start symbol S is the "main dish." Each production rule is a recipe step that replaces one ingredient (non-terminal) with a combination of simpler ingredients. The final dish (string) is made entirely of basic ingredients (terminals)!

### CFG Definition
G = (V, T, P, S)
- V: non-terminals (variables)
- T: terminals
- P: production rules (A → α, where A ∈ V, α ∈ (V∪T)*)
- S ∈ V: start symbol

**Key constraint:** Left side of every production is a **single** non-terminal (this is what makes it "context-free" — the replacement doesn't depend on surrounding context!)

### Derivation Trees (Parse Trees) ⭐⭐
- Root = start symbol
- Internal nodes = non-terminals
- Leaves = terminals or ε
- **Yield:** string formed by reading leaves left to right

**Leftmost derivation:** Always expand the leftmost non-terminal first
**Rightmost derivation:** Always expand the rightmost non-terminal first (also called "canonical derivation")

**Worked Example:**
Grammar: S → AB, A → aA | a, B → bB | b
Derive "aabb":

Leftmost: S → AB → aAB → aaB → aabB → aabb
Rightmost: S → AB → AbB → Abb → aAbb → aabb

Both produce the same parse tree:
```
        S
       / \
      A   B
     / \  / \
    a   A b   B
        |     |
        a     b
```

### Ambiguity ⭐⭐⭐

A grammar is **ambiguous** if some string has **two or more** distinct parse trees (equivalently, two different leftmost derivations).

**Classic Example:** E → E+E | E×E | a
String "a+a×a" has two parse trees:

**Tree 1 (+ first):**
```
      E
    / | \
   E  +  E
   |    / | \
   a   E  ×  E
       |     |
       a     a
```
Meaning: a + (a × a)

**Tree 2 (× first):**
```
      E
    / | \
   E  ×  E
  / | \   |
 E  +  E  a
 |     |
 a     a
```
Meaning: (a + a) × a

**Resolving ambiguity:** Use precedence and associativity rules:
```
E → E + T | T       (+ is left-associative, lower precedence)
T → T × F | F       (× is left-associative, higher precedence)
F → (E) | a         (parentheses and atoms)
```
This grammar is **unambiguous** and generates the same language!

### Inherently Ambiguous Languages ⭐⭐

A CFL is **inherently ambiguous** if EVERY grammar generating it is ambiguous.

**Classic Example:** L = {aⁱbʲcᵏ | i=j or j=k}

**Why inherently ambiguous:**
- Strings where i=j=k (like aabbcc) must be generated by BOTH "i=j rules" and "j=k rules"
- Any grammar must have "i=j" productions and "j=k" productions
- For strings where i=j=k, both derivation paths apply → always ambiguous

**Another example:** L = {aⁱbʲcᵏdˡ | i=j, k=l} ∪ {aⁱbʲcᵏdˡ | i=l, j=k}

> 🎯 **GATE Trick:** "Is L inherently ambiguous?" is UNDECIDABLE in general. But for specific known examples (like the above), you should know the answer.

### CFG Design Techniques ⭐⭐⭐

**Technique 1: Count matching**
For L = {aⁿbⁿ}: S → aSb | ε (each 'a' matched with a 'b')
For L = {aⁿb²ⁿ}: S → aSbb | ε
For L = {aⁿbᵐ | n ≤ m}: S → aSb | Sb | ε (extra b's at end)
For L = {aⁿbᵐ | n ≥ m}: S → aSb | aS | ε (extra a's at start)
For L = {aⁿbᵐ | n ≠ m}: S → S₁ | S₂; S₁ → aS₁b | aS₁ | a; S₂ → aS₂b | S₂b | b

**Technique 2: Structural recursion**
For balanced parentheses: S → SS | (S) | ε
For matching brackets: S → SS | (S) | [S] | ε

**Technique 3: "Equal counts"**
For L = {w ∈ {a,b}* | #a = #b}: S → aSbS | bSaS | ε
(This is a famous grammar — generates all strings with equal a's and b's)

**Technique 4: Union**
For L₁ ∪ L₂: S → S₁ | S₂ (with S₁ generating L₁, S₂ generating L₂)

**Technique 5: Concatenation**
For L₁L₂: S → S₁S₂

**Technique 6: Kleene star**
For L₁*: S → SS₁ | ε

**More Design Examples:**

L = {aⁱbʲcᵏ | i + k = j}:
- Think: j = i + k, so we need i a's, then i b's, then k b's, then k c's
- S → aSb | B, B → bBc | ε
- Derivation for a²b³c¹: S → aSb → aaSbb → aaBbb → aabBcbb → aabb c bb... hmm
- Actually: S → aSb | B; B → bBc | ε
- For a²b³c¹: S → aSb → aaSbb → aaBbb → aabBcbb → aabbcbb... 
- Wait, that gives a²b²c then need 1 more b... Let me reconsider.
- S → aSb | T, T → bTc | ε
- For a²b³c¹: S → aSb → aaSbb → aaTbb → aabTcbb → aabcbb = a²b¹c¹b² → No, order is wrong!
- Correct: The b's from the first part and second part are adjacent:
  - S → aSb: gives aⁱ...bⁱ (a's and matching b's on outside)
  - T → bTc: gives bᵏ...cᵏ (b's and c's in middle)
  - So S generates aⁱbᵏcᵏbⁱ = aⁱbᵏcᵏbⁱ... that's not right either.
  
**Correct grammar:** S → aSb | T; T → bTc | ε produces L = {aⁱbⁱ⁺ᵏcᵏ | i,k ≥ 0}
which IS {aⁱbʲcᵏ | j = i+k} ✓

Derivation for a²b³c¹ (i=2, k=1, j=3): S → aSb → aaSbb → aaTbb → aabTcbb → aabcbb
This gives a²bcb² = aabcbb ≠ a²b³c¹!

Actually the structure gives: a's first, then T's b's, then T's c's, then S's b's
= aⁱ bᵏ cᵏ bⁱ — which is NOT the right order!

**Correct approach:** S → aSb | T; T → bTc | ε
- This generates {aⁿ bⁿ⁺ᵐ cᵐ} where n from S-rule and m from T-rule
- String form: aⁿ bᵐ cᵐ bⁿ... NO.
- Actually: trace S → aSb → ... → aⁿTbⁿ → aⁿbᵐTcᵐbⁿ → aⁿbᵐcᵐbⁿ
- This gives aⁿbᵐcᵐbⁿ, not what we want.

**True correct grammar for {aⁱbʲcᵏ | j = i+k}:**
S → AC; A → aAb | ε; C → bCc | ε
- A generates aⁱbⁱ, C generates bᵏcᵏ
- S = aⁱbⁱbᵏcᵏ = aⁱbⁱ⁺ᵏcᵏ ✓

Derivation for a²b³c¹: A = a²b², C = bc → S = aabbbbc = a²b³c¹ ✓

> 🎯 **GATE Lesson:** When designing CFGs, be careful about the ORDER of symbols. Always trace a derivation to verify!

### Grammar Simplification ⭐⭐⭐

**Order of simplification matters!**

**Step 1: Remove ε-productions** (A → ε, except if S → ε and ε ∈ L)
- Find all nullable variables: A is nullable if A →* ε
- For each production with nullable variables, add versions with/without them
- Remove all A → ε productions (keep S → ε if needed)

**Step 2: Remove unit productions** (A → B)
- Find all unit pairs: (A,B) where A →* B using only unit productions
- For each unit pair (A,B) and each non-unit production B → α, add A → α
- Remove all unit productions

**Step 3: Remove useless symbols**
- **Generating:** Variable A is generating if A →* w for some terminal string w
- **Reachable:** Variable A is reachable if S →* αAβ for some α,β
- **Useful = generating AND reachable**
- First remove non-generating, then remove non-reachable

**Worked Example:**
```
S → AB | a
A → aAb | ε
B → bB
```

**Step 1: Find nullable variables**
A → ε, so A is nullable. S → AB → εB = B (S not directly nullable from this).
Nullable = {A}

Add productions considering nullability:
- S → AB | B | a (A can be ε)
- A → aAb | ab (A in aAb can be ε)
- B → bB
Remove ε: A → ε removed.

**Step 2: Remove unit productions**
S → B is a unit production. Non-unit productions from B: B → bB.
Add: S → bB. Remove S → B.

Result: S → AB | bB | a; A → aAb | ab; B → bB

**Step 3: Remove useless symbols**
Generating? S → a (generates terminal) ✓. A → ab ✓. B → bB → bB → ... (never produces terminal string!) ✗

Remove B and all productions using B:
S → a; A → aAb | ab

Reachable from S? S → a. A is not reachable!
Remove A: S → a

**Final grammar:** S → a (generates language {a}) — that's correct because original grammar generated {a, ε, aⁿbⁿ} but B was useless.

Wait, actually B → bB generates no terminal string (it's stuck in a loop), so anything requiring B is dead. And S → a is the only surviving production.

### Normal Forms ⭐⭐⭐

**Chomsky Normal Form (CNF):** ⭐⭐⭐
Every production is either:
- A → BC (two non-terminals)
- A → a (single terminal)
- S → ε (only if ε ∈ L, and S doesn't appear on RHS)

**CNF Conversion Algorithm:**
1. Start with simplified grammar (no ε, no unit, no useless — except S→ε if needed)
2. For each production A → X₁X₂...Xₙ (n ≥ 2):
   - Replace each terminal a with new variable Ca where Ca → a
3. For productions with RHS length > 2 (A → B₁B₂...Bₙ):
   - Replace with: A → B₁D₁, D₁ → B₂D₂, ..., Dₙ₋₂ → Bₙ₋₁Bₙ
   (Chain of binary productions)

**CNF Worked Example:**
Grammar: S → aAB, A → bBb, B → A | a

Step 1 (already simplified, no ε or useless):
Unit production B → A: add B → bBb, remove B → A.
Result: S → aAB, A → bBb, B → bBb | a

Step 2: Replace terminals in mixed productions:
- Ca → a, Cb → b
- S → CaAB, A → CbBCb, B → CbBCb | a

Step 3: Break long productions:
- S → CaD₁, D₁ → AB
- A → CbD₂, D₂ → BCb
- B → CbD₃ | a, D₃ → BCb
- Ca → a, Cb → b

Final CNF:
```
S → CaD₁     D₁ → AB
A → CbD₂     D₂ → BCb
B → CbD₃ | a D₃ → BCb
Ca → a        Cb → b
```

**Key CNF Facts:**
- Every CNF derivation of string length n takes exactly **2n−1 steps** (n−1 binary splits + n terminal replacements)
- This is why CYK algorithm (bottom-up DP) works — it knows the structure!
- Every CFL (except {ε}) has a CNF grammar

**Greibach Normal Form (GNF):** ⭐⭐
Every production is: A → aα where a ∈ T, α ∈ V* (starts with terminal, followed by variables only)

**GNF conversion is complex** (involves left-recursion elimination and substitution). For GATE, just know:
- Every CFL (except {ε}) has a GNF grammar
- GNF derivation consumes exactly one terminal per step → derivation length = |string|
- GNF is used to construct PDAs: one step per input symbol, push variables onto stack
- **GNF parsing is O(n) per step, total O(n²) worst case**

### Pumping Lemma for CFLs ⭐⭐⭐
If L is CFL, ∃ pumping length p such that for every w ∈ L with |w| ≥ p:
w = uvxyz where:
1. |vy| > 0 (v and y can't both be empty)
2. |vxy| ≤ p (the "pump" section is bounded)
3. ∀i ≥ 0: uv^i xy^i z ∈ L (pump v and y together!)

> 💡 **Key Difference from Regular PL:** Here we pump TWO substrings (v and y) by the same amount. This makes sense because a PDA has a stack — it can "match" counts.

### CFL Pumping Lemma — Complete Proofs ⭐⭐⭐

**Example 1:** L = {aⁿbⁿcⁿ | n ≥ 0} (Classic non-CFL!)

1. Assume CFL, pumping length p
2. Choose w = aᵖbᵖcᵖ ∈ L (length 3p ≥ p)
3. Any split uvxyz with |vxy| ≤ p:
   - Since |vxy| ≤ p, vxy can span at most 2 of the 3 sections {a's, b's, c's}
   - Case 1: vxy within a's and b's → pumping changes #a or #b but not #c → ∉ L ✗
   - Case 2: vxy within b's and c's → pumping changes #b or #c but not #a → ∉ L ✗
   - Case 3: vxy within just one section → same argument ✗
4. All cases give contradiction → **L is not CFL** ✓

**Example 2:** L = {ww | w ∈ {a,b}*} (Not CFL!)

1. Assume CFL, pumping length p
2. Choose w = aᵖbᵖaᵖbᵖ (of the form "ww" where w = aᵖbᵖ)
3. |vxy| ≤ p, so vxy fits within one of these regions (approximately):
   - If in first aᵖ: pumping grows first half, doesn't match second half
   - If spanning first aᵖbᵖ boundary: disrupts structure
   - If in first bᵖ or spanning into second aᵖ: similar argument
4. In all cases, pumped string ≠ form ww → **L is not CFL** ✓

**Example 3:** L = {aⁿ | n is a perfect square} (Not CFL!)

1. Assume CFL, pumping length p
2. Choose w = aᵖ² (length p²)
3. Split: u = aˢ, v = aᵗ, x = aʳ, y = aᵘ, z = aᵛ where s+t+r+u+v = p²
4. |vy| = t+u ≥ 1, |vxy| = t+r+u ≤ p
5. Pump i=2: length = p² + (t+u) where 1 ≤ t+u ≤ p
6. Need p² < p²+(t+u) ≤ p²+p < (p+1)² = p²+2p+1 (since t+u ≤ p < 2p+1)
7. So p² < pumped length < (p+1)² → NOT a perfect square → ∉ L ✗
8. **Not CFL** ✓

> 🎯 **GATE Trick:** For "n is a perfect square/prime/power_of_2" languages, show the gap between consecutive valid lengths grows, but pumping adds a fixed amount — eventually falling in a gap!

### CYK Algorithm (Cocke–Younger–Kasami) ⭐⭐⭐

Membership testing for CFLs. **Requires grammar in CNF.**

**Input:** Grammar G in CNF, string w = a₁a₂...aₙ
**Output:** Whether w ∈ L(G)

**Algorithm (Bottom-up DP):**
1. Initialize: For each aᵢ, set T[i,i] = {A | A → aᵢ is a production}
2. For substring length ℓ = 2 to n:
   - For start position i = 1 to n-ℓ+1:
     - j = i + ℓ - 1
     - For split point k = i to j-1:
       - T[i,j] = T[i,j] ∪ {A | A → BC, B ∈ T[i,k], C ∈ T[k+1,j]}
3. Accept if S ∈ T[1,n]

**Complexity:** $O(n^3 \cdot |G|)$ time, $O(n^2)$ space.

**Worked Example:**
Grammar: S → AB | BC, A → BA | a, B → CC | b, C → AB | a
String: w = "baaba" (n = 5)

**Step 1: Base cases (length 1):**
- T[1,1] = {B} (B → b)
- T[2,2] = {A, C} (A → a, C → a)
- T[3,3] = {A, C}
- T[4,4] = {B}
- T[5,5] = {A, C}

**Step 2: Length 2 substrings:**
- T[1,2]: split k=1: B∈T[1,1], {A,C}∈T[2,2] → check BA (→A) → {A}. Check BC (→S) → {S}. T[1,2] = {S, A}
- T[2,3]: split k=2: {A,C}∈T[2,2], {A,C}∈T[3,3] → AA? CA? AC? CC → CC→B. T[2,3] = {B}
- T[3,4]: split k=3: {A,C}∈T[3,3], B∈T[4,4] → AB→{S,C}. CB→∅. T[3,4] = {S, C}
- T[4,5]: split k=4: B∈T[4,4], {A,C}∈T[5,5] → BA→A. BC→S. T[4,5] = {S, A}

**Step 3: Length 3:**
- T[1,3]: k=1: T[1,1]×T[2,3] = B×B → BB? ∅. k=2: T[1,2]×T[3,3] = {S,A}×{A,C} → SA? SC? AA? AC? → ∅.
  T[1,3] = ∅
- T[2,4]: k=2: T[2,2]×T[3,4] = {A,C}×{S,C} → AS? AC? CS? CC → CC→B. k=3: T[2,3]×T[4,4] = B×B → ∅.
  T[2,4] = {B} (from CC→B... wait, A→a and C→a, so A,C are in T[2,2]. S,C in T[3,4]. CC → B ✓)
  Actually checking more carefully: AS→∅, AC→∅ (no such rule), CS→∅, CC→B ✓. T[2,4] = {B}
- T[3,5]: k=3: T[3,3]×T[4,5] = {A,C}×{S,A} → AS? AA? CS? CA? → ∅. k=4: T[3,4]×T[5,5] = {S,C}×{A,C} → SA? SC? CA? CC → CC→B. T[3,5] = {B}

**Step 4: Length 4:**
- T[1,4]: k=1: B×{B} → ∅. k=2: {S,A}×{B} → SB? AB → AB→{S,C}. k=3: ∅×B → ∅.
  T[1,4] = {S, C}
- T[2,5]: k=2: {A,C}×{B} → AB→{S,C}. CB→∅. k=3: B×{S,A} → BS? BA→A. k=4: B×{A,C} → BA→A. BC→S.
  T[2,5] = {S, A, C}

**Step 5: Length 5:**
- T[1,5]: k=1: B×{S,A,C} → BS? BA→A. BC→S. k=2: {S,A}×{B} → SB? AB→{S,C}. k=3: ∅×{S,A} → ∅. k=4: {S,C}×{A,C} → SA? SC? CA? CC→B.
  T[1,5] = {S, A, C, B} (from various splits)

**S ∈ T[1,5]** → "baaba" ∈ L(G) ✓

> 🎯 **GATE Trick:** CYK table is filled diagonally (length 1, then 2, etc.). For GATE, practice filling small tables (n ≤ 5). The answer is in the top-right cell T[1,n].

**CYK Key Facts:**
- Works ONLY with CNF grammars (convert first!)
- For ambiguity detection: if multiple derivations found → grammar is ambiguous for that string
- Earley parser is alternative: O(n³) worst case, O(n²) for unambiguous, O(n) for LR(k)

### Chomsky Hierarchy Level 2 Summary ⭐⭐

| Property | Regular (Type 3) | Context-Free (Type 2) |
|---|---|---|
| Machine | DFA/NFA | PDA (nondeterministic) |
| Grammar | A → aB, A → a | A → α |
| Memory | None (just state) | Stack (LIFO) |
| Parsing | O(n) | O(n³) CYK, O(n) for LL/LR |
| Closure: ∪, ·, * | All ✓ | All ✓ |
| Closure: ∩ | ✓ | ✗ |
| Closure: complement | ✓ | ✗ |
| Emptiness decidable | ✓ | ✓ |
| Equivalence decidable | ✓ | ✗ |
| Examples | a*b*, (ab)*, ... | aⁿbⁿ, palindromes, ... |

### Left Recursion Elimination ⭐⭐

> 💡 **Why needed:** LL parsers (top-down) can't handle left-recursive grammars — they'd loop forever trying to expand the same non-terminal.

**Direct left recursion:** A → Aα | β (A → Aα₁ | Aα₂ | ... | β₁ | β₂ | ...)

**Elimination:**
```
A → β₁A' | β₂A' | ...
A' → α₁A' | α₂A' | ... | ε
```

**Worked Example:**
```
E → E + T | T
```
Left recursive! (E → E...)

Eliminate:
```
E → TE'
E' → +TE' | ε
```

**Two-step elimination for indirect:**
```
Original: S → Aa | b
          A → Sd | ε
```
Substitute S's production into A: A → Aad | bd | ε
Now A → Aad | bd | ε is directly left-recursive! Eliminate:
```
A → bdA' | A'
A' → adA' | ε
```

Final: S → Aa | b; A → bdA' | A'; A' → adA' | ε

### Left Factoring ⭐⭐

> 💡 **Why needed:** LL(1) parsers need to decide which production to use by looking at one lookahead symbol. If two productions start with the same symbol, the parser is confused.

**General form:** A → αβ₁ | αβ₂ (common prefix α)

**Left factor:**
```
A → αA'
A' → β₁ | β₂
```

**Worked Example:**
```
S → iEtS | iEtSeS | a
E → b
```
(if-then-else grammar, i=if, E=expression, t=then, S=statement, e=else)

Left factor (common prefix "iEtS"):
```
S → iEtSS' | a
S' → eS | ε
E → b
```

> 🎯 **GATE Trick:** Left factoring resolves the "dangling else" ambiguity in programming languages!

### First and Follow Sets ⭐⭐

**FIRST(α)** = set of terminals that can appear as the first symbol of any string derived from α. If α →* ε, then ε ∈ FIRST(α).

**FOLLOW(A)** = set of terminals that can appear immediately after A in any sentential form. $ ∈ FOLLOW(S) always (end marker).

**Computing FIRST:**
1. If X is terminal: FIRST(X) = {X}
2. If X → ε: add ε to FIRST(X)
3. If X → Y₁Y₂...Yₖ: add FIRST(Y₁) − {ε} to FIRST(X). If ε ∈ FIRST(Y₁), add FIRST(Y₂) − {ε}, etc.

**Computing FOLLOW:**
1. $ ∈ FOLLOW(S)
2. If A → αBβ: add FIRST(β) − {ε} to FOLLOW(B)
3. If A → αB or A → αBβ where ε ∈ FIRST(β): add FOLLOW(A) to FOLLOW(B)

**Worked Example:**
```
E → TE'
E' → +TE' | ε
T → FT'
T' → *FT' | ε
F → (E) | id
```

| Symbol | FIRST | FOLLOW |
|---|---|---|
| E | {(, id} | {$, )} |
| E' | {+, ε} | {$, )} |
| T | {(, id} | {+, $, )} |
| T' | {*, ε} | {+, $, )} |
| F | {(, id} | {*, +, $, )} |

### LL(1) Parsing Table Construction ⭐⭐

For each production A → α:
1. For each terminal a ∈ FIRST(α): M[A, a] = A → α
2. If ε ∈ FIRST(α): for each b ∈ FOLLOW(A): M[A, b] = A → α
3. If ε ∈ FIRST(α) and $ ∈ FOLLOW(A): M[A, $] = A → α

**Grammar is LL(1) iff no cell has two entries.**

| | id | + | * | ( | ) | $ |
|---|---|---|---|---|---|---|
| E | E→TE' | | | E→TE' | | |
| E' | | E'→+TE' | | | E'→ε | E'→ε |
| T | T→FT' | | | T→FT' | | |
| T' | | T'→ε | T'→*FT' | | T'→ε | T'→ε |
| F | F→id | | | F→(E) | | |

No conflicts → Grammar IS LL(1) ✓

> 🎯 **GATE Connection:** This is more Compiler Design than TOC, but LL/LR concepts connect to DCFLs — every LL(k) and LR(k) language is a DCFL.

### Grammar Equivalence Checking ⭐

Two grammars are equivalent if they generate the same language.

**For regular grammars:** Convert both to DFAs → minimize → check isomorphism. **Decidable!**

**For CFGs:** **Undecidable!** (One of the most important undecidability results for GATE)

**For DCFGs:** Also decidable! (Sénizergues 2001 — advanced result, just know it's decidable for GATE)

### Unambiguous Grammars and Parsing ⭐⭐

| Grammar Class | Parsing Method | Time | Deterministic? | GATE Importance |
|---|---|---|---|---|
| LL(1) | Predictive (top-down) | O(n) | Yes | High |
| LR(0) | Shift-reduce | O(n) | Yes | High |
| SLR(1) | Shift-reduce + FOLLOW | O(n) | Yes | High |
| LALR(1) | Shift-reduce + lookahead | O(n) | Yes | High |
| LR(1) | Canonical LR | O(n) | Yes | Medium |
| Unambiguous CFG | Earley/CYK | O(n²)/O(n³) | Can be | Low |
| Ambiguous CFG | GLR/CYK | O(n³)+  | No | Low |

**Containment:** LL(1) ⊂ LR(1) ⊃ SLR(1) ⊃ LR(0)
All LL(1), SLR(1), LALR(1), LR(1) languages are DCFLs.

### Common CFGs for GATE ⭐⭐

| Language | CFG | Notes |
|---|---|---|
| {aⁿbⁿ \| n≥0} | S → aSb \| ε | DCFL |
| {aⁿbⁿ \| n≥1} | S → aSb \| ab | DCFL |
| {aⁿb²ⁿ \| n≥0} | S → aSbb \| ε | DCFL |
| {aⁿbᵐ \| n≤m} | S → aSb \| Sb \| ε | DCFL |
| {aⁿbᵐ \| n≥m} | S → aSb \| aS \| ε | DCFL |
| {aⁿbᵐ \| n≠m} | S → A \| B; A → aAb \| aA \| a; B → aBb \| Bb \| b | CFL, not DCFL |
| Balanced parens | S → (S)S \| ε | DCFL |
| Palindromes | S → aSa \| bSb \| a \| b \| ε | CFL, not DCFL |
| {w \| #a=#b} | S → aSbS \| bSaS \| ε | CFL, not DCFL |
| {aⁱbʲcᵏ \| i+k=j} | S → AC; A → aAb \| ε; C → bCc \| ε | CFL |
| {aⁿbⁿcᵐdᵐ \| n,m≥0} | S → AB; A → aAb \| ε; B → cBd \| ε | CFL |
| {aⁿbᵐcⁿdᵐ \| n,m≥0} | Not CFL! | Fails CFL PL |

### Ogden's Lemma ⭐

A stronger version of the CFL pumping lemma that can prove languages non-CFL when the standard pumping lemma is hard to apply.

**Statement:** If L is CFL, ∃ p such that for any w ∈ L with |w| ≥ p and any choice of marking ≥ p positions of w as "distinguished," there exists a split w = uvxyz where:
1. vxy contains ≤ p distinguished positions
2. vy contains ≥ 1 distinguished position
3. ∀i ≥ 0: uvⁱxyⁱz ∈ L

**Use:** When standard pumping lemma fails because the adversary can choose vxy to be in an unhelpful region, Ogden's lemma lets YOU choose which positions matter.

**Example:** Prove {aⁱbʲcᵏ | i≠j, j≠k, i≠k} is not CFL.
Standard PL is tricky here, but Ogden's lemma with strategic marking of positions works.

> 🎯 **GATE note:** Ogden's lemma is rarely asked directly, but knowing it exists helps with tricky "prove not CFL" problems.

---

## 5. Pushdown Automata 📚

> 💡 **Analogy:** A PDA is like a person reading a book while keeping track on a notepad 📝. The DFA is the "reader" (finite control), and the stack is the "notepad" (can write and erase from the top). The notepad gives the PDA the power to COUNT — which DFAs cannot do!

### PDA Definition
M = (Q, Σ, Γ, δ, q₀, Z₀, F)
- Q: finite set of states
- Σ: input alphabet
- Γ: stack alphabet
- δ: Q × (Σ ∪ {ε}) × Γ → finite subsets of Q × Γ*
- q₀: start state
- Z₀: initial stack symbol (bottom marker)
- F: set of final/accepting states

**Transition:** δ(q, a, X) = {(p₁, γ₁), (p₂, γ₂), ...}
Meaning: In state q, reading input 'a', with X on top of stack:
- Move to state p₁ and replace X with γ₁ on stack (push/pop/replace)

**Stack operations encoded:**
| Stack Operation | Transition Form | Example |
|---|---|---|
| **Push B** | δ(q, a, X) = {(p, BX)} | Push B, keep X below |
| **Pop** | δ(q, a, X) = {(p, ε)} | Remove X from stack |
| **Replace** | δ(q, a, X) = {(p, Y)} | Replace X with Y |
| **No change** | δ(q, a, X) = {(p, X)} | Read input, don't touch stack |
| **Push multiple** | δ(q, a, X) = {(p, ABC)} | Push A, B, C (A on top) |

### PDA Acceptance Modes ⭐⭐

**By final state:** Accept if in final state after reading entire input (stack can be anything)
**By empty stack:** Accept if stack is empty after reading entire input (no final states needed)

**Equivalence theorem:** For every PDA accepting by final state, there exists a PDA accepting by empty stack for the same language, and vice versa.

**Conversion: Final state → Empty stack:**
- Add transitions: from each final state, pop everything from stack
- Use new bottom marker to detect when original stack is empty

**Conversion: Empty stack → Final state:**
- Add new initial state that pushes special marker
- Add new final state
- When original Z₀ is exposed, move to new final state via ε-transition

### PDA Design Techniques ⭐⭐⭐

**Technique 1: "Push-then-Match" (Count matching)**

For L = {aⁿbⁿ | n ≥ 1}:
```
Start: q₀, Z₀ on stack

δ(q₀, a, Z₀) = {(q₀, AZ₀)}    Push A for each 'a'
δ(q₀, a, A)  = {(q₀, AA)}      Keep pushing A's
δ(q₀, b, A)  = {(q₁, ε)}       Start popping: switch to q₁
δ(q₁, b, A)  = {(q₁, ε)}       Keep popping for each 'b'
δ(q₁, ε, Z₀) = {(q₂, ε)}      Stack empty → accept

Accept by final state: F = {q₂}
(Or accept by empty stack with the Z₀ → ε transition)
```

**Trace for "aabb":**
| Step | State | Remaining Input | Stack | Action |
|---|---|---|---|---|
| 0 | q₀ | aabb | Z₀ | |
| 1 | q₀ | abb | AZ₀ | Push A |
| 2 | q₀ | bb | AAZ₀ | Push A |
| 3 | q₁ | b | AZ₀ | Pop A (switch state) |
| 4 | q₁ | ε | Z₀ | Pop A |
| 5 | q₂ | ε | ε | Accept! ✓ |

**Technique 2: "Guess the middle" (Palindromes)**

For L = {ww^R | w ∈ {a,b}*} (even-length palindromes):
```
Phase 1 (state q₀): Push each symbol onto stack
Phase 2 (state q₁): Pop and match

Key: WHEN to switch from push to pop?
Answer: Nondeterministically GUESS the middle!

δ(q₀, a, Z₀) = {(q₀, AZ₀)}
δ(q₀, b, Z₀) = {(q₀, BZ₀)}
δ(q₀, a, A) = {(q₀, AA)}
δ(q₀, a, B) = {(q₀, AB)}
δ(q₀, b, A) = {(q₀, BA)}
δ(q₀, b, B) = {(q₀, BB)}
δ(q₀, ε, A) = {(q₁, A)}    ← Guess: we're at the middle!
δ(q₀, ε, B) = {(q₁, B)}    
δ(q₁, a, A) = {(q₁, ε)}    ← Pop and match
δ(q₁, b, B) = {(q₁, ε)}
δ(q₁, ε, Z₀) = {(q₂, ε)}  ← Accept!
```

> 🎯 **GATE Insight:** Palindromes REQUIRE nondeterminism (can't know the middle without reading ahead). This is why {ww^R} is CFL but not DCFL. Odd-length palindromes {wcw^R} ARE DCFL (c marks the middle)!

**Technique 3: "Two-counter comparison"**

For L = {aⁱbʲcᵏ | i = j or j = k}:
- This needs nondeterminism: GUESS whether to match i=j or j=k
- Branch 1: Push a's, pop with b's, ignore c's
- Branch 2: Ignore a's, push b's, pop with c's

```
δ(q₀, ε, Z₀) = {(q₁, Z₀), (q₃, Z₀)}  ← Nondeterministic choice!

Branch 1 (match i=j):
δ(q₁, a, Z₀) = {(q₁, AZ₀)}
δ(q₁, a, A) = {(q₁, AA)}
δ(q₁, b, A) = {(q₂, ε)}
δ(q₂, b, A) = {(q₂, ε)}
δ(q₂, c, Z₀) = {(q₂, Z₀)}    ← ignore c's
δ(q₂, ε, Z₀) = {(qf, ε)}

Branch 2 (match j=k):
δ(q₃, a, Z₀) = {(q₃, Z₀)}    ← ignore a's
δ(q₃, a, A) = {(q₃, A)}       
δ(q₃, b, Z₀) = {(q₄, BZ₀)}
...similar push b pop c pattern
```

### PDA Design — More Examples ⭐⭐

**Example: L = {a²ⁿbⁿ | n ≥ 0} (twice as many a's as b's)**

Strategy: Push one A for every two a's, then pop one A for each b.

```
δ(q₀, a, Z₀) = {(q₁, Z₀)}     Read first a (don't push yet)
δ(q₁, a, Z₀) = {(q₀, AZ₀)}    Read second a → push A
δ(q₀, a, A)  = {(q₁, A)}       Read first a of pair
δ(q₁, a, A)  = {(q₀, AA)}      Read second a → push A
δ(q₀, b, A)  = {(q₂, ε)}       Start matching b's
δ(q₂, b, A)  = {(q₂, ε)}       Keep popping
δ(q₂, ε, Z₀) = {(qf, ε)}      Accept
δ(q₀, ε, Z₀) = {(qf, ε)}      Accept ε
```

**Example: L = {w ∈ {a,b}* | #a = #b}**

Strategy: Stack keeps track of the "excess" count.

```
δ(q, a, Z₀) = {(q, AZ₀)}    First a: push A
δ(q, b, Z₀) = {(q, BZ₀)}    First b: push B
δ(q, a, A) = {(q, AA)}       a when excess a's: push more A
δ(q, b, B) = {(q, BB)}       b when excess b's: push more B
δ(q, a, B) = {(q, ε)}        a cancels excess b → pop B
δ(q, b, A) = {(q, ε)}        b cancels excess a → pop A
δ(q, ε, Z₀) = {(qf, Z₀)}    Stack empty → equal counts → accept

Accept by final state: F = {qf}
```

### CFG ↔ PDA Conversion ⭐⭐⭐

**CFG → PDA (top-down simulation):**
Given G = (V, T, P, S), construct PDA:

1. Push S onto stack
2. If top of stack is variable A: non-deterministically choose A → α, replace A with α
3. If top of stack is terminal a: match with next input symbol (pop if match, reject if mismatch)
4. Accept when stack is empty and input is consumed

**PDA states:** {q₀, q₁, q₂}
```
δ(q₀, ε, Z₀) = {(q₁, SZ₀)}           Push start symbol
For each A → α:
  δ(q₁, ε, A) = {(q₁, α)} ∪ ...       Replace variable with RHS
For each a ∈ T:
  δ(q₁, a, a) = {(q₁, ε)}             Match terminal
δ(q₁, ε, Z₀) = {(q₂, ε)}             Accept
```

**PDA → CFG conversion:**
For PDA M = (Q, Σ, Γ, δ, q₀, Z₀):
Create variables [qAp] representing "computation from state q to state p that pops A from stack"

For each δ(q, a, A) containing (r, B₁B₂...Bₖ):
Add production: [qAp] → a[rB₁s₁][s₁B₂s₂]...[sₖ₋₁Bₖp]
for all possible intermediate states s₁, s₂, ..., sₖ₋₁

Start symbol: [q₀Z₀qf] for final state qf (or [q₀Z₀q] for all q if accepting by empty stack)

> 🎯 **GATE Trick:** The number of variables in the converted grammar can be O(|Q|²|Γ|), which can be very large. GATE usually asks about the conversion concept, not the full computation.

### DPDA vs NPDA — Key Differences ⭐⭐⭐

| Feature | DPDA | NPDA |
|---|---|---|
| Transitions | At most ONE choice per config | Multiple choices allowed |
| Languages | DCFL | CFL |
| Power | Strictly weaker | Full CFL power |
| Complement closure | **✓ (closed!)** | **✗ (not closed!)** |
| Parsing | LL, LR parsers | General CFG |
| Acceptance | Final state ≠ empty stack | Final state = empty stack |
| Ambiguity | Always unambiguous grammar | May need ambiguous grammar |

**Languages separating DPDA from NPDA:**
- {ww^R | w ∈ {a,b}*} — CFL but NOT DCFL (need nondeterminism to find middle)
- {aⁱbʲ | i = j or i = 2j} — CFL but NOT DCFL (need to guess which condition)

**Languages that ARE DCFL:**
- {aⁿbⁿ | n ≥ 0} — deterministic (switch from push to pop on first 'b')
- {wcw^R | w ∈ {a,b}*} — deterministic (center marker 'c' tells when to switch)
- All LR(k) languages — DCFL
- {aⁿbᵐ | n ≤ m} — deterministic

### DCFL Properties ⭐⭐

- DCFLs are closed under **complementation** (by modifying acceptance conditions)
- DCFLs are **NOT closed under** union, intersection, concatenation, Kleene star, reversal
- Every DCFL has an **unambiguous grammar**
- DCFL ⊂ CFL (proper subset)
- All LR(k) languages are DCFL
- DCFL ∩ Regular = DCFL (intersection with regular is DCFL)
- DCFL is closed under **inverse homomorphism**

### Two-Stack PDA = Turing Machine ⭐⭐

A PDA with **two stacks** is as powerful as a Turing Machine!

**Proof sketch:**
- One stack simulates the tape contents LEFT of the head
- Other stack simulates the tape contents RIGHT of the head
- Moving head left = pop from left stack, push to right stack
- Moving head right = pop from right stack, push to left stack

**Consequence:** Adding a second stack to a PDA jumps from CFL power to TM power! There's no intermediate level.

**Similarly:** A PDA with a queue instead of a stack = TM power (queue automaton can simulate TM)

---

## 6. Closure & Decision Properties 🔒

> 💡 **Analogy:** Closure properties are like asking "if I combine foods from the same restaurant, do I still get something from that restaurant?" 🍕 If regular languages are "Pizza Hut" and you combine two Pizza Hut items (union), you still get Pizza Hut (closed!). But if you take CFL items and intersect them, you might get something from a completely different restaurant!

### Closure Properties Summary ⭐⭐⭐

| Operation | Regular | CFL | DCFL | CSL | Recursive | RE | co-RE |
|-----------|---------|-----|------|-----|-----------|-----|-------|
| Union | ✓ | ✓ | ✗ | ✓ | ✓ | ✓ | ✗ |
| Intersection | ✓ | ✗ | ✗ | ✓ | ✓ | ✓ | ✓ |
| Complement | ✓ | ✗ | ✓ | ✓ | ✓ | ✗ | ✗ |
| Concatenation | ✓ | ✓ | ✗ | ✓ | ✓ | ✓ | ✗ |
| Kleene star | ✓ | ✓ | ✗ | ✓ | ✓ | ✓ | ✗ |
| Intersection w/ Regular | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Homomorphism | ✓ | ✓ | ✗ | ✗ | ✗ | ✓ | ✗ |
| Inverse Homomorphism | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Reversal | ✓ | ✓ | ✗ | ✓ | ✓ | ✓ | ✗ |

> 🎯 **GATE Trick for memorizing:** 
> - **Regular languages** are closed under EVERYTHING (they're the "MVP" of closure)
> - **CFLs** lose ∩ and complement (because PDA can't "intersect" two stacks)
> - **DCFLs** gain complement back (determinism allows complementation!) but lose ∪, ·, *
> - **Recursive** (decidable) languages are closed under everything except homomorphism
> - **RE** loses complement (you can't negate an infinite loop!)
> - **co-RE** is the "mirror" of RE: gains ∩, loses ∪, ·, *

### Closure Property Proofs & Constructions ⭐⭐

**Why CFL is NOT closed under intersection:**
- L₁ = {aⁿbⁿcᵐ | n,m ≥ 0} is CFL (push a's, match with b's, accept c's)
- L₂ = {aᵐbⁿcⁿ | n,m ≥ 0} is CFL (accept a's, push b's, match with c's)
- L₁ ∩ L₂ = {aⁿbⁿcⁿ | n ≥ 0} is **NOT CFL!**
- Therefore CFLs are not closed under intersection ✓

**Why CFL is NOT closed under complement:**
If CFLs were closed under complement, they'd be closed under intersection (by De Morgan: L₁ ∩ L₂ = complement(complement(L₁) ∪ complement(L₂))). But we just showed they're not closed under intersection. Contradiction!

**Key Result: CFL ∩ Regular = CFL ⭐⭐⭐**
Construction: Run PDA and DFA simultaneously. PDA tracks CFL recognition, DFA tracks regular language. Accept when BOTH accept. The combined machine is still a PDA (finite states of DFA × PDA states, same stack).

**Applications of CFL ∩ Regular = CFL:**
- To check if a CFL satisfies an additional "regular" constraint
- To prove a language is CFL: show it's CFL ∩ Regular
- To simplify pumping lemma proofs: intersect with a*b*c* first

### Using Closure Properties to Determine Language Class ⭐⭐⭐

**Technique 1: "If L were [class], then ... would also be [class], contradiction"**

**Example:** Is L = {aⁿbⁿcⁿ | n ≥ 0} context-free?
Direct proof via CFL pumping lemma works, but also:
- If L were CFL, then L ∩ a*b*c* = L would be CFL ∩ Regular = CFL
- We need a different approach since L already is a subset of a*b*c*
- Better: Use pumping lemma directly

**Example:** Is L = complement of {aⁿbⁿ | n ≥ 0} context-free?
- {aⁿbⁿ} is CFL but not DCFL? Actually {aⁿbⁿ} IS DCFL!
- Complement of DCFL is DCFL, which is ⊂ CFL
- So YES, L is CFL (in fact, DCFL) ✓

**Example:** Is L = {w | w is NOT a palindrome over {a,b}} context-free?
- Set of palindromes over {a,b} is CFL (not DCFL)
- CFL is NOT closed under complement
- So we can't conclude from closure... need direct construction or proof
- Actually, L = complement of palindromes IS CFL! (can be shown by direct CFG construction)
- Grammar: S → aSa | bSb | aXb | bXa; X → aX | bX | ε
(generates strings where two corresponding positions differ)

**Technique 2: Reduction and construction**

**Example:** L = {xy | |x| = |y|, x ≠ y} over {a,b}
- This is CFL! Grammar: S → aSa | aSb | bSa | bSb | aTb | bTa; T → aTa | aTb | bTa | bTb | ε
- Generates strings of even length where left and right halves differ at some position

### Homomorphism and Inverse Homomorphism ⭐⭐

**Homomorphism h:** Maps each symbol to a string. h: Σ → Δ*
- Extended to strings: h(a₁a₂...aₙ) = h(a₁)h(a₂)...h(aₙ)
- Applied to language: h(L) = {h(w) | w ∈ L}

**Example:** h(a) = 01, h(b) = 1. Then h(aba) = 01·1·01 = 01101.

**Inverse homomorphism:** h⁻¹(L) = {w ∈ Σ* | h(w) ∈ L}
- Maps strings BACK: find all strings whose image is in L

**Key fact:** Inverse homomorphism preserves ALL language classes!
(Regular, CFL, DCFL, CSL, Recursive, RE, co-RE — all closed under h⁻¹)

**Why?** To compute h⁻¹(L), apply h to input on-the-fly and feed to machine for L.

### Decision Properties ⭐⭐⭐

| Problem | Regular | DCFL | CFL | Recursive | RE |
|---------|---------|------|-----|-----------|-----|
| Membership (w ∈ L?) | ✓ O(n) | ✓ O(n) | ✓ O(n³) CYK | ✓ | ✗ (RE: semi-decidable) |
| Emptiness (L = ∅?) | ✓ | ✓ | ✓ | ✗ | ✗ |
| Finiteness (L finite?) | ✓ | ✓ | ✓ | ✗ | ✗ |
| Equivalence (L₁ = L₂?) | ✓ | ✓ (!) | **✗** | ✗ | ✗ |
| Inclusion (L₁ ⊆ L₂?) | ✓ | ✓ | **✗** | ✗ | ✗ |
| Universality (L = Σ*?) | ✓ | ✓ | **✗** | ✗ | ✗ |
| Ambiguity | N/A | N/A | **✗** | N/A | N/A |
| Regularity (is CFL regular?) | N/A | N/A | **✗** | N/A | N/A |
| Disjointness (L₁ ∩ L₂ = ∅?) | ✓ | ✗ | **✗** | ✗ | ✗ |

> 🎯 **GATE Trick:** For regular languages, EVERYTHING is decidable! For CFLs, only membership, emptiness, and finiteness are decidable. Equivalence and inclusion are undecidable even for DCFLs... wait, no!

**Important correction:** DCFL equivalence IS decidable! (Proved by Sénizergues in 2001 — very hard theorem). But CFL equivalence is undecidable.

### Decision Problems — How They Work ⭐⭐

**Emptiness for CFG:**
1. Find all generating variables (variables that can derive a terminal string)
2. If S is generating → L ≠ ∅
  - Forward: find generating variables by repeatedly marking
  - Y is generating if Y → α where all symbols in α are terminals or already-marked generators

**Finiteness for CFG:**
1. Remove useless symbols
2. Create dependency graph of variables
3. L is infinite iff the graph has a cycle that is reachable from S and can reach a terminal string

**Emptiness for Regular (DFA):**
1. Check if any final state is reachable from start state (BFS/DFS)

**Equivalence for DFA:**
1. Minimize both DFAs → check if they're isomorphic
2. Alternative: check if symmetric difference L₁ ⊕ L₂ = ∅ (build product DFA)

### Using Decision Properties in GATE ⭐⭐

**Common GATE question type:** "Is property P decidable?"

**Framework:**
1. Identify the language class (Regular? CFL? RE?)
2. Look up the decision property table
3. If about TM languages: likely undecidable (check if Rice's theorem applies)

**Example:** "Given two CFGs G₁ and G₂, is L(G₁) = L(G₂)?"
- CFL equivalence → **Undecidable** ✓

**Example:** "Given a DFA M, is L(M) = Σ*?"
- Regular universality → **Decidable** (check if all reachable states are final, or equivalently check if complement is empty) ✓

**Example:** "Given a CFG G, is L(G) regular?"
- "Is CFL regular?" → **Undecidable** ✓

---

## 7. Turing Machines & Decidability 🖥️

> 💡 **Analogy:** A Turing Machine is like a person with a paper tape, a pencil, and an eraser ✏️. The tape is infinitely long (unlimited scratch paper), and the person follows a fixed set of rules. This simple model can compute ANYTHING that any computer can compute! The miracle is that such a simple device is universal.

> 🏭 **Real-World Connection:** Every computer, smartphone, cloud server — they're all just fancy Turing Machines. The theoretical limits of TMs are the limits of ALL computation!

### Turing Machine Definition ⭐⭐
M = (Q, Σ, Γ, δ, q₀, B, F)
- Q: finite set of states
- Σ: input alphabet (⊂ Γ, doesn't include blank)
- Γ: tape alphabet (includes Σ and blank B)
- δ: Q × Γ → Q × Γ × {L, R} (for deterministic TM)
- q₀: start state
- B ∈ Γ: blank symbol
- F ⊆ Q: set of final/accepting states

**Configuration:** (q, α₁α₂...αₙ) where q is current state and head is positioned on tape
Notation: α₁...αᵢ₋₁ **q** αᵢαᵢ₊₁...αₙ (q between symbols = head position)

### TM Design Techniques ⭐⭐⭐

#### Technique: "Mark and Scan"

**Example 1:** TM for L = {aⁿbⁿ | n ≥ 1}

Strategy: Repeatedly mark one 'a' and one 'b' until all are matched.

```
1. Scan right from left end
2. Find leftmost unmarked 'a' → mark it as 'X'
3. Scan right to find leftmost unmarked 'b' → mark it as 'Y'
4. Scan left back to first unmarked 'a'
5. Repeat until all a's are marked
6. Check: if all b's are also marked → ACCEPT
```

**Formal transitions:**
```
δ(q₀, a) = (q₁, X, R)    Mark 'a' as X, go right to find 'b'
δ(q₁, a) = (q₁, a, R)    Skip unmarked a's
δ(q₁, Y) = (q₁, Y, R)    Skip already-matched Y's
δ(q₁, b) = (q₂, Y, L)    Found 'b', mark as Y, go left
δ(q₂, Y) = (q₂, Y, L)    Skip Y's going left
δ(q₂, a) = (q₂, a, L)    Skip a's going left
δ(q₂, X) = (q₀, X, R)    Found marked X, move right to start new match
δ(q₀, Y) = (q₃, Y, R)    All a's matched! Check remaining
δ(q₃, Y) = (q₃, Y, R)    Skip all Y's
δ(q₃, B) = (q_accept, B, R)  All done → ACCEPT
```

**Trace for "aabb":**
```
q₀ aabb → Xq₁abb → Xaq₁bb → XaYq₂b → Xq₂aYb → q₂XaYb → Xq₀aYb
→ XXq₁Yb → XXYq₁b → XXYYq₂ → XXYq₂Y → XXq₂YY → Xq₂XYY → XXq₀YY
→ XXYq₃Y → XXYYq₃B → ACCEPT!
```

**Time complexity:** O(n²) — each match requires scanning across the tape

**Example 2:** TM for L = {ww | w ∈ {a,b}*}

This is much harder! Strategy:
1. Check string has even length (mark alternating positions)
2. Compare first half with second half character by character
3. Mark current first-half char, scan to corresponding second-half position, compare, mark

This TM requires ~8-10 states. **Time: O(n²)**

**Example 3:** TM as a function computer — compute f(n) = n+1 in unary

Input: 1ⁿ (n ones), Output: 1ⁿ⁺¹

```
δ(q₀, 1) = (q₀, 1, R)    Scan to end of input
δ(q₀, B) = (q₁, 1, L)    At blank, write one more '1'
δ(q₁, 1) = (q₁, 1, L)    Scan back to start
δ(q₁, B) = (q_halt, B, R) Halt at start
```

### TM Variants (All Equivalent in Power) ⭐⭐⭐

**Crucial theorem:** All these variants recognize the SAME class of languages (RE) and compute the SAME class of functions!

#### Multi-tape Turing Machine ⭐⭐
- Has k tapes, each with its own head
- δ: Q × Γᵏ → Q × Γᵏ × {L,R,S}ᵏ (S = stay)
- **Simulation by 1-tape DTM:** Encode all k tapes on a single tape separated by delimiters. Mark head positions with special symbols.
- **Time overhead:** If k-tape TM runs in time T(n), 1-tape DTM simulates in $O(T(n)^2)$ time.
- **Use:** Simplifies TM constructions (e.g., universal TM, simulation arguments).

#### Non-deterministic Turing Machine (NDTM) ⭐⭐
- δ: Q × Γ → P(Q × Γ × {L,R}) — multiple possible transitions
- **Accepts** if ANY computation branch accepts
- **Simulation by DTM:** BFS over all computation branches
- **Time overhead:** If NDTM runs in time T(n), DTM simulates in $O(c^{T(n)})$ (exponential!)
- This exponential gap is the P vs NP question!
- **NDTM does NOT run faster** — it "guesses" correct answer

**Key for GATE:** NDTM and DTM recognize the SAME languages (RE). But they might differ in TIME complexity.

#### Linear Bounded Automaton (LBA) ⭐⭐⭐
- TM restricted to tape cells of the input (can't go past endmarkers)
- **Recognizes exactly CSL (Context-Sensitive Languages)**

**Key decidability results for LBAs:**
| Problem | Decidable? | Reason |
|---|---|---|
| Membership (w ∈ L(M)?) | ✓ | Finite configurations: |Q| × n × |Γ|ⁿ |
| Emptiness (L(M) = ∅?) | ✗ | Reduces from halting problem |
| Equivalence | ✗ | Undecidable |
| Universality (L = Σ*?) | ✗ | Undecidable |

**Why membership is decidable:** For input of length n, there are at most $|Q| \cdot n \cdot |\Gamma|^n$ configurations. If the LBA enters a repeated configuration, it's in a loop → reject. So just simulate for that many steps.

**DLBA vs NLBA:** Open problem! Not known if deterministic LBA = nondeterministic LBA.

#### Other Variants (All TM-equivalent) ⭐

| Variant | Equivalence Proof Idea |
|---|---|
| Multi-tape TM | Interleave tapes on single tape |
| 2-way infinite tape | Fold tape: even positions = right half, odd = left half |
| Multi-track TM | k tracks on one tape = k-symbol alphabet |
| Multi-head TM | Track head positions on tape |
| 2-stack PDA | Left stack = left of head, right stack = right of head |
| Counter machine (≥2 counters) | Encode tape as number using prime factorization |
| Queue automaton | Can simulate tape operations |
| Random Access Machine | Standard simulation both ways |

### Universal Turing Machine (UTM) ⭐⭐⭐

> 💡 **Analogy:** A UTM is like a computer that can run ANY program! Just give it a DESCRIPTION of a TM and an input, and it simulates that TM on that input. This is exactly what your computer does — it runs programs described in software!

**UTM takes input: ⟨M, w⟩ (encoding of TM M and input w)**
**UTM simulates M on w:**
- If M accepts w → UTM accepts
- If M rejects w → UTM rejects
- If M loops on w → UTM loops

**Encoding TMs:** Every TM can be encoded as a binary string (its "Gödel number" or "description"). TMs are countable!

**Existence of UTM proves:** There exists a single TM that can simulate any other TM. This is the theoretical foundation of stored-program computers!

### Chomsky Hierarchy ⭐⭐⭐

| Type | Language Class | Machine | Grammar Form | Example |
|------|---------------|---------|-------------|---------|
| 3 | Regular | DFA/NFA | A → aB, A → a | a*b*, (ab)* |
| 2 | Context-Free | PDA | A → α (A ∈ V) | aⁿbⁿ, palindromes |
| 1 | Context-Sensitive | LBA | αAβ → αγβ (|γ| ≥ 1) | aⁿbⁿcⁿ |
| 0 | Recursively Enumerable | TM | α → β (unrestricted) | Halting TMs |

**Complete hierarchy (with intermediate classes):**
```
Regular ⊂ DCFL ⊂ CFL ⊂ CSL ⊂ Recursive ⊂ RE ⊂ co-RE ∪ RE ⊂ ALL Languages

Each ⊂ is PROPER (strictly contained):
- DFA < PDA: {aⁿbⁿ} separates
- DPDA < NPDA: {ww^R} separates
- PDA < LBA: {aⁿbⁿcⁿ} separates
- LBA < DTM: halting problem for LBAs separates
- Recursive < RE: halting problem separates
- RE < ALL: diagonal language separates
```

### Countability & Diagonalization ⭐⭐⭐

> 💡 **The most beautiful argument in CS:** Most problems are UNSOLVABLE. Here's why:

**Countable set:** Can be listed: x₁, x₂, x₃, ... (bijection with ℕ)

| Set | Countable? | Why |
|-----|-----------|-----|
| ℕ (naturals) | ✓ | By definition |
| ℤ (integers) | ✓ | 0, 1, -1, 2, -2, ... |
| ℚ (rationals) | ✓ | Cantor's zigzag enumeration |
| Σ* (all strings over finite Σ) | ✓ | Enumerate by length, then lexicographic |
| All TMs / All C programs | ✓ | Each has a finite encoding ∈ Σ* |
| All languages over Σ | **✗ Uncountable** | = P(Σ*) = 2^|Σ*| (power set of countable) |
| ℝ (reals) | **✗ Uncountable** | Cantor's diagonal argument |

**The counting argument:**
- TMs are countable (each TM = finite string encoding)
- Languages are uncountable (power set of countable set)
- Therefore: MOST languages have NO TM that recognizes them!
- The set of decidable languages is a tiny, tiny fraction of all languages

### Church-Turing Thesis ⭐⭐

**Statement:** Every effectively computable function is computable by a Turing Machine.

**Key points:**
- NOT a theorem — it's a thesis/hypothesis (cannot be formally proved within mathematics)
- Supported by enormous evidence: λ-calculus, μ-recursive functions, Post systems, RAM machines, quantum TMs, cellular automata — ALL equivalent to TMs
- **Implication:** If we can't build a TM for it, NO computation model can solve it
- **Extended CTT:** Physical computation ≤ TM computation with polynomial overhead

### Decidability & Recognizability ⭐⭐⭐

> 💡 **Analogy — The Detective 🕵️:**
> - **Decidable** language: The detective ALWAYS tells you "guilty" or "not guilty" — guaranteed answer in finite time
> - **RE** language: The detective says "guilty" if you are, but might investigate FOREVER if you're innocent — may never give an answer
> - **co-RE** language: The detective says "not guilty" if you're innocent, but might investigate forever if you're guilty
> - **Neither RE nor co-RE:** The detective can't even START a meaningful investigation

| Class | TM Behavior on YES | TM Behavior on NO | Relationship |
|---|---|---|---|
| **Decidable** (Recursive) | Halt & Accept | Halt & Reject | RE ∩ co-RE |
| **RE** (Recognizable) | Halt & Accept | May loop forever | |
| **co-RE** (co-Recognizable) | May loop forever | Halt & Reject | |
| **Neither** | — | — | Outside RE ∪ co-RE |

**Fundamental Theorem:** L is decidable iff L is **BOTH** RE and co-RE.

**Proof:**
- (→) If L is decidable, the TM always halts, so both L and complement(L) are RE
- (←) If L is RE (TM₁ accepts L) and co-RE (TM₂ accepts complement of L):
  - Run TM₁ and TM₂ in parallel on input w
  - If TM₁ accepts → accept (w ∈ L)
  - If TM₂ accepts → reject (w ∉ L)
  - One of them MUST accept (since w is either in L or not) → always halts!

### Enumerator ⭐⭐

- A TM with a "printer" — writes strings to output tape
- **Theorem:** L is RE iff some enumerator enumerates L (possibly with repetitions, any order)
- **Theorem:** L is decidable iff some enumerator enumerates L in **lexicographic order**

**Why lexicographic ordering implies decidability:**
To decide if w ∈ L: run the enumerator. If it outputs w, accept. If it outputs something lexicographically past w, reject. Since it's in order, we'll eventually reach w's position.

### The Halting Problem ⭐⭐⭐

**Definition:** HALT = {⟨M, w⟩ | TM M halts on input w}

**Theorem:** HALT is RE but NOT decidable (undecidable).

**Proof by Diagonalization:**

Assume HALT is decidable → ∃ TM H that decides HALT:
- H(⟨M, w⟩) = accept if M halts on w
- H(⟨M, w⟩) = reject if M doesn't halt on w

Construct TM D (the "diagonalizer"):
```
D(⟨M⟩):
  Run H(⟨M, ⟨M⟩⟩)         -- Does M halt on its own description?
  If H accepts → LOOP FOREVER
  If H rejects → HALT (accept)
```

Now run D on ⟨D⟩:
- If D halts on ⟨D⟩ → H(⟨D, ⟨D⟩⟩) accepts → D loops ✗ CONTRADICTION
- If D doesn't halt on ⟨D⟩ → H(⟨D, ⟨D⟩⟩) rejects → D halts ✗ CONTRADICTION

Both cases contradict → H cannot exist → HALT is undecidable. □

**HALT is RE:** Simply simulate M on w. If M halts, accept. (But if M doesn't halt, we loop forever — that's OK for RE.)

**HALT complement is NOT RE:** If both HALT and its complement were RE → HALT would be decidable (contradiction). Since HALT is RE but not decidable, its complement is NOT RE.

### Rice's Theorem ⭐⭐⭐

> 💡 **The most powerful theorem in TOC for GATE!** It lets you instantly classify problems as undecidable without doing a reduction each time.

**Statement:** Any **non-trivial** property of the **language** recognized by a TM is undecidable.

**Conditions:**
1. **Non-trivial:** The property is true for some TMs and false for others (not vacuously true or false)
2. **Language property:** The property depends only on L(M), not on M's structure. If L(M₁) = L(M₂), then P(M₁) = P(M₂).

**What Rice's theorem says is undecidable:**

| Question | Language Property? | Non-trivial? | Undecidable? |
|---|---|---|---|
| Is L(M) empty? | ✓ | ✓ (some TMs: yes, some: no) | **✓ Undecidable** |
| Is L(M) finite? | ✓ | ✓ | **✓ Undecidable** |
| Is L(M) regular? | ✓ | ✓ | **✓ Undecidable** |
| Is L(M) = Σ*? | ✓ | ✓ | **✓ Undecidable** |
| Does L(M) contain "hello"? | ✓ | ✓ | **✓ Undecidable** |
| Is L(M) a CFL? | ✓ | ✓ | **✓ Undecidable** |
| Is L(M) = {aⁿbⁿ}? | ✓ | ✓ | **✓ Undecidable** |
| Is L(M₁) = L(M₂)? | ✓ | ✓ | **✓ Undecidable** |
| Is L(M₁) ⊆ L(M₂)? | ✓ | ✓ | **✓ Undecidable** |

**What Rice's theorem does NOT cover:**

| Question | Why not? | Decidable? |
|---|---|---|
| Does M have ≤ 100 states? | Structural, not language property | **Decidable** (just count) |
| Does M halt on empty input? | About behavior, but specific input, not language | **Undecidable** (halting problem) |
| Does M halt within 1000 steps on w? | Bounded computation | **Decidable** (simulate 1000 steps) |
| Does M ever write symbol X? | Structural/behavioral, not language | **Undecidable** (but not by Rice's) |
| Does M have an infinite loop? | About M's behavior, not its language | **Undecidable** (but not by Rice's) |

> 🎯 **GATE Trick for Rice's Theorem:** Ask THREE questions:
> 1. Is it about the LANGUAGE L(M)? (not M's structure)
> 2. Is it non-trivial? (not always true/false)
> 3. If both yes → **UNDECIDABLE by Rice's**

### Rice's Theorem — Proof ⭐⭐

Let P be a non-trivial property of RE languages. WLOG assume P(∅) = false (if P(∅) = true, work with ¬P instead).

Since P is non-trivial: ∃ TM M_P with P(L(M_P)) = true.

**Reduction from HALT to P:**
Given ⟨M, w⟩ (halting problem instance), construct TM M':
```
M'(x):
  1. Run M on w (ignoring x)
  2. If M halts, run M_P on x and accept/reject as M_P does
  (If M doesn't halt, M' never reaches step 2 → L(M') = ∅)
```

Analysis:
- If M halts on w: M' simulates M_P on all inputs → L(M') = L(M_P) → P(L(M')) = true
- If M doesn't halt on w: M' loops on everything → L(M') = ∅ → P(L(M')) = false

So: deciding P would decide HALT → **P is undecidable.** □

### Reductions ⭐⭐⭐

**Many-one (mapping) reduction:** A ≤_m B if ∃ computable function f such that:
x ∈ A ↔ f(x) ∈ B

**Properties:**
- If A ≤_m B and B is decidable → A is decidable
- If A ≤_m B and A is undecidable → B is undecidable
- If A ≤_m B and B is RE → A is RE
- If A ≤_m B and A is not RE → B is not RE

**Common Reduction Chain:**
```
HALT → State-Entry Problem → Emptiness → Finiteness → Regularity → ...
HALT → MPCP → PCP → CFG Ambiguity
HALT → Rice's Theorem (for all language properties)
```

**How to reduce A to B (to show B is undecidable):**
1. Take an instance of A (known undecidable)
2. Transform it into an instance of B (using a computable function)
3. Show: YES instance of A ↔ YES instance of B

### Post Correspondence Problem (PCP) ⭐⭐

**Definition:** Given pairs (u₁,v₁), ..., (uₙ,vₙ) over alphabet Σ:
Does there exist a sequence of indices i₁, i₂, ..., iₖ (k ≥ 1) such that:
$u_{i_1} u_{i_2} \cdots u_{i_k} = v_{i_1} v_{i_2} \cdots v_{i_k}$?

**Theorem:** PCP is undecidable (for |Σ| ≥ 2). Decidable for |Σ| = 1.

**Modified PCP (MPCP):** Same as PCP but must START with pair 1. Also undecidable.

**Reduction chain:** HALT → MPCP → PCP

**PCP Worked Example 1 (Has solution):**
Pairs: (a, ab), (b, ca), (ca, a), (abc, c)

Try: pair 1,2,3,4: abca·abc vs. abcaa·ac = ? No.
Try: pair 1,3,2,4: a·ca·b·abc = acababc vs. ab·a·ca·c = abacac. No.

Finding PCP solutions is hard — that's the whole point! It's undecidable in general.

**PCP Worked Example 2 (No solution):**
Pairs: (a, aa)
- Always: top has length k, bottom has length 2k → can NEVER be equal!
- No solution exists ✓ (but in general, we can't always determine this)

**PCP Applications:**
PCP is used to prove undecidability of:
- CFG ambiguity: "Is this CFG ambiguous?"
- CFG equivalence: "Do G₁ and G₂ generate the same language?"
- CFL complement: "Is complement of CFL also CFL?"

### Classifying Languages — The Big Picture ⭐⭐⭐

| Language | Class | How to Prove |
|---|---|---|
| {aⁿbⁿ \| n ≥ 0} | CFL (also DCFL) | PDA construction |
| {ww^R} | CFL (not DCFL) | NPDA (guess middle) |
| {wcw^R} | DCFL | DPDA (c marks middle) |
| {aⁿbⁿcⁿ} | CSL (not CFL) | CFL pumping lemma fails |
| {ww} | CSL (not CFL) | CFL pumping lemma fails |
| HALT | RE (not decidable) | Diagonalization |
| complement(HALT) | Not RE | If it were RE, HALT would be decidable |
| "L(M) = ∅?" | Not RE (by Rice's extended) | co-RE actually |
| {(M) \| M halts on all inputs} | co-RE (not decidable) | Complement is RE |
| A_TM = {⟨M,w⟩ \| M accepts w} | RE (not decidable) | Same as HALT essentially |
| EQ_TM = {⟨M₁,M₂⟩ \| L(M₁)=L(M₂)} | Neither RE nor co-RE | Rice's + more |
| L_d (diagonal language) | Not RE | Diagonalization (no TM for it) |

### RE, co-RE, and Decidable — Venn Diagram ⭐⭐⭐

```
┌───────────────────────────────────────────┐
│              ALL Languages                │
│  ┌──────────────────────────────────┐     │
│  │              RE                  │     │
│  │  ┌─────────────────────┐        │     │
│  │  │      Decidable      │        │     │
│  │  │   (RE ∩ co-RE)      │        │     │
│  │  └─────────────────────┘        │     │
│  └──────────────────────────────────┘     │
│  ┌──────────────────────────────────┐     │
│  │             co-RE                │     │
│  │  ┌─────────────────────┐        │     │
│  │  │      Decidable      │        │     │
│  │  │   (same set)        │        │     │
│  │  └─────────────────────┘        │     │
│  └──────────────────────────────────┘     │
│                                           │
│  Neither RE nor co-RE (e.g., EQ_TM)      │
└───────────────────────────────────────────┘
```

**Examples in each region:**
- **Decidable:** Regular languages, CFLs, {aⁿbⁿcⁿ}
- **RE but not decidable:** HALT, A_TM, PCP solutions
- **co-RE but not decidable:** complement(HALT), "M halts on all inputs"
- **Neither RE nor co-RE:** EQ_TM, "is L(M) regular?" (for RE index set version)

### Complexity Theory Basics (GATE Relevant) ⭐⭐

| Class | Definition | Relationship |
|---|---|---|
| **P** | Decidable in polynomial time by DTM | P ⊆ NP |
| **NP** | Decidable in polynomial time by NDTM | NP ⊆ PSPACE |
| **NP** (equivalent) | Verifiable in polynomial time (given certificate) | |
| **co-NP** | Complement is in NP | P ⊆ co-NP |
| **NP-complete** | In NP AND NP-hard | SAT, 3-SAT, Clique, etc. |
| **NP-hard** | At least as hard as every NP problem | May not be in NP |
| **PSPACE** | Decidable in polynomial space | |
| **EXPTIME** | Decidable in exponential time | |

**Key relationships:**
P ⊆ NP ∩ co-NP ⊆ NP ⊆ PSPACE ⊆ EXPTIME

**The P = NP? question:** P ≠ NP is widely believed but unproven. It's the most famous open problem in CS/math.

**NP-completeness (Cook-Levin Theorem):**
SAT (Boolean satisfiability) is NP-complete.
Proof idea: Any NDTM computation can be encoded as a Boolean formula that is satisfiable iff the NDTM accepts.

**Common NP-complete problems:**
| Problem | Description |
|---|---|
| SAT | Is Boolean formula satisfiable? |
| 3-SAT | SAT with clauses of size exactly 3 |
| Clique | Does graph have clique of size k? |
| Vertex Cover | Can k vertices cover all edges? |
| Hamiltonian Path | Does graph have path visiting all vertices? |
| Subset Sum | Does subset of numbers sum to target? |
| 3-Coloring | Can graph be colored with 3 colors? |
| Traveling Salesman | Is there tour of cost ≤ k? |

**Reduction between NP-complete problems:**
```
SAT → 3-SAT → Clique
              → Vertex Cover → Hamiltonian Cycle → TSP
              → Subset Sum → Partition → Bin Packing
3-SAT → 3-Coloring → k-Coloring
```

### Recursion Theorem (Self-Reference) ⭐

**Statement:** For any computable function t, there exists a TM M such that M on input w computes the same as t(⟨M⟩, w).

**Implication:** A TM can "know its own description"! It can modify its own code or reason about itself.

**Applications:**
- Proves that certain problems are undecidable (by constructing self-referential TMs)
- The "quine" concept: programs that print their own source code
- Underlies the proof of Rice's theorem and many other undecidability results

---

## Worked Examples — GATE Level ⭐⭐⭐

### Example 1: NFA to DFA Conversion (Easy)
NFA with states {q₀, q₁}, Σ = {a, b}, start = q₀, final = {q₁}.
δ(q₀,a) = {q₀,q₁}, δ(q₀,b) = {q₀}, δ(q₁,a) = ∅, δ(q₁,b) = {q₁}

DFA states: subsets of {q₀, q₁}:
- {q₀}: start. On a → {q₀,q₁}. On b → {q₀}.
- {q₀,q₁}: **final**. On a → {q₀,q₁}. On b → {q₀,q₁}.
- Dead state never reached.

**Minimum DFA: 2 states.** Accept any string containing 'a'.

### Example 2: Pumping Lemma (Medium)
**Prove:** L = {aⁿbⁿ | n ≥ 0} is not regular.

Assume L is regular. Let p be pumping length.
Choose w = aᵖbᵖ ∈ L, |w| = 2p ≥ p.
Any split w = xyz with |xy| ≤ p, |y| > 0:
- xy is entirely within the 'a' section (since |xy| ≤ p)
- y = aᵏ for some k ≥ 1
- Pump i=0: xz = aᵖ⁻ᵏbᵖ ∉ L (unequal a's and b's)

**Contradiction.** L is not regular. ✓

### Example 3: CFG Design (Medium)
**Design:** CFG for L = {aⁿb²ⁿ | n ≥ 1}

S → aSbb | abb

Derivation for n=3: S → aSbb → aaSbbbb → aaabbbbbb = a³b⁶ ✓

### Example 4: Rice's Theorem Application (GATE Level)
**Q:** Is "L(M) is a CFL" decidable?

Check Rice's conditions:
1. Language property? Yes — depends only on L(M)
2. Non-trivial? Yes — some TMs recognize CFLs, some don't

By Rice's theorem: **Undecidable** ✓

### Example 5: DFA Minimization (GATE Level)
**Q:** Minimize DFA with states {A,B,C,D,E}, Σ={0,1}, start=A, final={C,E}

| State | δ(·,0) | δ(·,1) |
|---|---|---|
| A | B | C |
| B | A | D |
| C | E | A |
| D | E | A |
| E | B | C |

Step 1: Initial partition: {A,B,D} (non-final) | {C,E} (final)

Step 2: Refine {A,B,D}:
- A: 0→B∈{A,B,D}, 1→C∈{C,E}
- B: 0→A∈{A,B,D}, 1→D∈{A,B,D}
- D: 0→E∈{C,E}, 1→A∈{A,B,D}
A goes to different groups than B on input 1 → split: {A}, {B,D}

Wait: B on 1→D (non-final group), A on 1→C (final group). So A separates from B,D.
B: 0→A (non-final), 1→D (non-final). D: 0→E (final!), 1→A (non-final).
B and D differ on input 0! → split further.

Final partition: {A}, {B}, {C}, {D}, {E}
But let's check C and E: C: 0→E∈{C,E}, 1→A. E: 0→B, 1→C∈{C,E}.
C on 0 goes to {C,E}, E on 0 goes to {B} → different → C ≠ E.

Hmm, this DFA is already minimal (5 states). Different transition tables give different results.

### Example 6: PDA Design (Medium)

**Design PDA for L = {aⁱbʲ | i ≤ j ≤ 2i}**

Strategy: Push one symbol for each 'a'. For each 'b', nondeterministically pop or don't pop.
- Phase 1 (reading a's): Push A for each 'a'
- Phase 2 (reading b's): For each 'b':
  - If stack has AA on top: pop both (consuming 2 symbols per b → handles j close to i)
  - Or pop one A (1-to-1 matching → handles j close to 2i)
  - Actually simpler: Push two A's for each 'a'. Pop one A for each 'b'. Then j must satisfy 0 ≤ remaining stack ≤ ... 

Better approach: For each 'a', push A. When reading b's, for each 'b' nondeterministically either pop one A or "half-pop" (mark one A). This is hard.

**Simplest correct PDA:**
- Push one A per 'a'
- On transition to b-reading phase: for each A on stack, nondeterministically "double" it to AA (using ε-transitions)
- Then pop one A per 'b'
- Accept when stack is empty

Actually, the cleanest way: Push 2 A's for each 'a' (so stack has 2i A's). Then pop 1-2 A's per 'b' nondeterministically. Accept when stack empty.

No wait: j ranges from i to 2i. If we push i A's:
- Pop 1 A per 'b', need exactly i b's to empty (j=i case)
- Need j=2i too — but we only have i A's!

**Correct approach:** Push TWO A's per 'a'. Stack has 2i A's. For each 'b', pop either 1 or 2 A's (nondeterministically). Accept by empty stack.
- If always pop 2: need 2i/2 = i b's → j=i ✓
- If always pop 1: need 2i b's → j=2i ✓
- Mix: any j between i and 2i ✓

### Example 7: Closure Properties (GATE Level)

**Q:** L₁ is regular, L₂ is CFL. Is L₁ ∩ L₂ necessarily:
(a) Regular (b) CFL (c) CSL (d) RE

**Answer:** (b) CFL

CFL ∩ Regular = CFL (known closure property). L₁ ∩ L₂ = L₂ ∩ L₁ where L₂ is CFL and L₁ is regular, so the result is CFL. ✓

**Q:** L₁ is CFL, L₂ is CFL. Is L₁ ∩ L₂ necessarily CFL?
**Answer:** NO! Counter-example: L₁ = {aⁿbⁿcᵐ}, L₂ = {aᵐbⁿcⁿ}, L₁ ∩ L₂ = {aⁿbⁿcⁿ} (not CFL!)

But L₁ ∩ L₂ IS always CSL (CSL is closed under intersection).

### Example 8: Decidability Classification (GATE Level)

**Classify each as Decidable, RE (not decidable), co-RE (not decidable), or Neither:**

| Problem | Classification | Reasoning |
|---|---|---|
| "Does DFA M accept string w?" | Decidable | Simulate DFA (O(n)) |
| "Is L(CFG) = ∅?" | Decidable | Check if start symbol generates terminal string |
| "Does TM M accept w?" | RE (not decidable) | A_TM — simulate, accept if M accepts |
| "Does TM M reject w?" | co-RE (not decidable) | Complement of A_TM |
| "Is L(TM) = Σ*?" | co-RE (not decidable) | Rice's; actually neither... |

Wait: "Is L(M) = Σ*?" — this is E_ALL_TM. 
- If L(M) = Σ* → TM accepts all inputs
- To verify "NOT universal": find a w that M doesn't accept → RE (enumerate w and check)
- To verify "IS universal": need to check all inputs → can't enumerate
- Actually: complement = "∃ w: M doesn't accept w" — this is RE (search for witness)
- So "L(M) = Σ*" is co-RE but not decidable

### Example 9: CFL Pumping Lemma (GATE Level)

**Prove {aⁿbⁿcⁿdⁿ | n ≥ 0} is not CFL:**

1. Assume CFL, pumping length p
2. Choose w = aᵖbᵖcᵖdᵖ
3. |vxy| ≤ p → vxy spans at most 2 of 4 sections
4. Pumping changes counts of at most 2 symbols → other 2 unchanged
5. Equal counts broken → ∉ L ✗
6. **Not CFL** ✓

### Example 10: Reduction (GATE Level)

**Show that "Does TM M accept at least 2 strings?" is undecidable.**

Method 1: Rice's Theorem
- Language property? Yes — depends on L(M)
- Non-trivial? Yes — TM that accepts nothing → false; TM that accepts {0,1} → true
- **Undecidable by Rice's** ✓

Method 2: Direct reduction from HALT
Given ⟨M, w⟩, construct M':
```
M'(x):
  If x = 0 or x = 1:
    Run M on w
    If M halts, accept
  Else reject
```
- If M halts on w: L(M') = {0, 1} → has ≥ 2 strings → YES
- If M doesn't halt: L(M') = ∅ → NO

Deciding "≥ 2 strings" would decide HALT → **Undecidable** ✓

### Example 11: Myhill-Nerode (GATE Level)

**Find minimum DFA states for L = {w ∈ {0,1}* | w interpreted as binary number is divisible by 5}**

The equivalence classes of ≡_L correspond to remainders mod 5: {0, 1, 2, 3, 4}

**Minimum states = 5**

Transitions (reading binary MSB first):
- State r, read bit b → new state (2r + b) mod 5

| State | On '0' | On '1' |
|---|---|---|
| 0 | 0 | 1 |
| 1 | 2 | 3 |
| 2 | 4 | 0 |
| 3 | 1 | 2 |
| 4 | 3 | 4 |

Start: state 0 (reading from left, ε represents 0)
Final: {0}

Verify: "1010" (=10): 0→1→2→4→3 ✗ (10 mod 5 = 0... hmm)
Wait: 1010₂ = 10₁₀. 10 mod 5 = 0. Let me retrace:
- Start 0, read '1': (2×0+1) mod 5 = 1
- State 1, read '0': (2×1+0) mod 5 = 2
- State 2, read '1': (2×2+1) mod 5 = 0
- State 0, read '0': (2×0+0) mod 5 = 0 ✓ (end in state 0 = final)

Hmm I got state 0 at position 2 (after reading "101" = 5), then reading "0" stays at 0. So "1010" → state 0 → accept ✓

### Example 12: TM Design (GATE Level)

**Design TM for L = {aⁿbⁿcⁿ | n ≥ 1}**

Strategy: Mark one of each (a, b, c) in each pass.

```
δ(q₀, a) = (q₁, X, R)     Mark leftmost 'a' as X
δ(q₁, a) = (q₁, a, R)     Skip remaining a's
δ(q₁, Y) = (q₁, Y, R)     Skip already-marked b's
δ(q₁, b) = (q₂, Y, R)     Mark leftmost unmarked 'b' as Y
δ(q₂, b) = (q₂, b, R)     Skip remaining b's
δ(q₂, Z) = (q₂, Z, R)     Skip marked c's
δ(q₂, c) = (q₃, Z, L)     Mark leftmost unmarked 'c' as Z
δ(q₃, a/b/X/Y/Z) = (q₃, same, L)  Go back to start
δ(q₃, B) = (q₀, B, R)     Back at beginning, start new round

// Termination check:
δ(q₀, X) = (q₄, X, R)     All a's marked → check others
δ(q₄, Y) = (q₄, Y, R)     Skip Y's
δ(q₄, Z) = (q₄, Z, R)     Skip Z's
δ(q₄, B) = (q_accept)      All marked → ACCEPT
```

**Time: O(n²)** (n passes, each scanning O(n) symbols)

---

## 📊 GATE Year-Wise PYQ Analysis for TOC ⭐

| Year | Questions | Topics Covered | Key Difficulty |
|---|---|---|---|
| GATE 2025 | 2-3 | DFA minimization, Pumping Lemma, Rice's Theorem | Medium-Hard |
| GATE 2024 | 2-3 | NFA→DFA, CFG Ambiguity, Decidability | Medium |
| GATE 2023 | 3 | Closure Properties, CFL PL, TM Design | Medium-Hard |
| GATE 2022 | 2-3 | Minimum DFA states, PDA, Undecidability | Medium |
| GATE 2021 | 3 | Regular expressions, CFG→PDA, Rice's | Easy-Medium |
| GATE 2020 | 2-3 | DFA design, Pumping Lemma, Halting Problem | Medium |
| GATE 2019 | 2-3 | NFA, Closure properties, CFL membership | Medium |

### Topic Frequency Analysis ⭐⭐

| Topic | GATE Frequency | Marks | Priority |
|---|---|---|---|
| DFA/NFA (design, minimization, conversion) | Very High (every year) | 1-2 marks | 🔥🔥🔥 |
| Regular Expressions | High | 1-2 marks | 🔥🔥🔥 |
| Pumping Lemma (Regular) | High | 2 marks | 🔥🔥🔥 |
| Closure Properties | Very High | 1-2 marks | 🔥🔥🔥 |
| CFG (design, ambiguity, CNF) | High | 1-2 marks | 🔥🔥 |
| PDA Design | Medium | 2 marks | 🔥🔥 |
| Pumping Lemma (CFL) | Medium | 2 marks | 🔥🔥 |
| Decision Properties | High | 1 mark | 🔥🔥🔥 |
| Rice's Theorem | High | 1-2 marks | 🔥🔥🔥 |
| Decidability/RE/co-RE | High | 1-2 marks | 🔥🔥🔥 |
| TM Design | Low-Medium | 2 marks | 🔥 |
| NP-completeness | Low | 1 mark | 🔥 |
| CYK Algorithm | Low | 2 marks | 🔥 |
| Myhill-Nerode | Medium | 1-2 marks | 🔥🔥 |

---

## 📝 Practice Set 1: DFA/NFA Design (10 Problems) ⭐⭐

**P1.** Design minimum-state DFA for L = {w ∈ {0,1}* | w has an odd number of 1's and an even number of 0's}

**Solution:** Product construction. Need to track (#1 mod 2, #0 mod 2).
States: (E0,E1), (E0,O1), (O0,E1), (O0,O1) — 4 states total.
Start: (E0,E1). Final: {(E0,O1)} (even 0's AND odd 1's)

| State | On 0 | On 1 |
|---|---|---|
| (E0,E1) | (O0,E1) | (E0,O1) |
| (E0,O1)** | (O0,O1) | (E0,E1) |
| (O0,E1) | (E0,E1) | (O0,O1) |
| (O0,O1) | (E0,O1) | (O0,E1) |

**Minimum states = 4** ✓

---

**P2.** How many states does the minimum DFA for L = {w ∈ {a,b}* | |w| mod 3 = 0 and w ends with 'a'} have?

**Solution:** Track (length mod 3, last char). States: (0,none), (0,a), (0,b), (1,a), (1,b), (2,a), (2,b) = 7 states? Let's think more carefully:
- Actually: states = (count mod 3) × (last char ∈ {a,b,start})
- But DFA minimization might merge some.
- Start: (0, start). After first 'a': (1,a). After first 'b': (1,b).
- Final: {(0,a)} — length divisible by 3 AND ended with 'a'

Transitions change both length mod 3 AND last char.
**Minimum states: 7** (start, and 6 combinations of mod 3 × last char)

Wait, the 'start' state is (0, none). From (0, none): on a → (1,a), on b → (1,b). This (0,none) only occurs at the very start (ε has length 0). After reading any character, we never return to "none" state.

Check if (0,none) is equivalent to any other state:
- (0,none): accepts ε? Length 0 mod 3 = 0, but doesn't end with 'a' → No.
- (0,b): Length ≡ 0 mod 3, ends with b → No.
- These are equivalent! Both reject, and transitions: (0,none) on a→(1,a), on b→(1,b). (0,b) on a→(1,a), on b→(1,b). Same! → Merge!

**Minimum states = 6** (3 × 2, tracking mod 3 and last char)

---

**P3.** Given NFA with 4 states, what is the maximum possible number of states in the equivalent minimum DFA?

**Solution:** Maximum DFA states from n-state NFA = 2ⁿ. For n=4: 2⁴ = **16** (including possible dead state).
But if we ask for reachable states: at most 2⁴ = 16 - 1 = 15 (excluding empty set if dead state not needed).
Actually, 2⁴ includes the empty set (dead state), so **maximum = 16**.

**This bound is tight!** The language Lₙ = {w ∈ {a,b}* | the n-th symbol from the end is 'a'} requires exactly 2ⁿ DFA states.

---

**P4.** An NFA has 3 states, all are final states. How many states does the complement DFA have at minimum?

**Solution:** 
1. Convert NFA to DFA: up to 2³ = 8 states
2. Complement DFA: swap final and non-final states
3. In the DFA, a state is final if it contains ANY NFA final state.
   Since ALL NFA states are final, every non-empty DFA state is final!
4. Only the dead state (∅) is non-final in the original DFA.
5. Complement: ∅ becomes the only final state → accepts nothing... unless ∅ is reachable!

Actually, the complement accepts strings that are NOT accepted by the original NFA. If all NFA states are final, then any string reaching a live state is accepted. The complement consists of strings that lead to the dead state only.

The answer depends on the specific NFA. **Minimum possible: 1 state** (if NFA accepts Σ*, complement is ∅, DFA for ∅ has 1 state).

---

**P5.** Design DFA for binary numbers divisible by 6.

**Solution:** Divisible by 6 = divisible by 2 AND divisible by 3.
- Div by 2: last bit is 0. DFA₁ has 2 states.
- Div by 3: track value mod 3. DFA₂ has 3 states.
- Product: 2 × 3 = **6 states** (track remainder mod 6 directly, or use product construction)

| State (mod 6) | On '0' | On '1' |
|---|---|---|
| 0 | 0 | 1 |
| 1 | 2 | 3 |
| 2 | 4 | 5 |
| 3 | 0 | 1 |
| 4 | 2 | 3 |
| 5 | 4 | 5 |

Start: 0, Final: {0}. Formula: δ(q, b) = (2q + b) mod 6

---

**P6.** DFA has 8 states over Σ = {a,b}. The number of possible transition functions is:

**Solution:** Each state has 2 transitions (one per symbol), each → 8 possible states.
Total: 8^(8×2) = 8^16 = 2^48

If also choosing final states (2⁸ choices) and start state (8 choices):
Total distinct DFAs = 8 × 2⁸ × 8^16

---

**P7.** ε-NFA with states {A,B,C}. ε-transitions: A→B, B→C. Regular transitions: δ(A,0)={A}, δ(B,1)={B}, δ(C,0)={C}, δ(C,1)={A}. Start=A, Final={C}. Convert to DFA.

**Solution:**
ε-closure(A) = {A,B,C} (A→ε→B→ε→C)
Start of DFA: {A,B,C}

δ'({A,B,C}, 0) = ε-closure(δ(A,0) ∪ δ(B,0) ∪ δ(C,0)) = ε-closure({A} ∪ ∅ ∪ {C}) = ε-closure({A,C}) = {A,B,C}

δ'({A,B,C}, 1) = ε-closure(δ(A,1) ∪ δ(B,1) ∪ δ(C,1)) = ε-closure(∅ ∪ {B} ∪ {A}) = ε-closure({A,B}) = {A,B,C}

**DFA has 1 state: {A,B,C}** which is final (contains C). This DFA accepts Σ*!

---

**P8.** What is the minimum number of states in a DFA accepting L = {w ∈ {0,1}* | w does not contain "00"}?

**Solution:** States track suffix:
- q₀: Start/last char was '1' (or empty)
- q₁: Last char was '0'
- q_dead: Saw "00" → reject forever

| State | On '0' | On '1' |
|---|---|---|
| q₀ | q₁ | q₀ |
| q₁ | q_dead | q₀ |
| q_dead | q_dead | q_dead |

Start: q₀, Final: {q₀, q₁}
**Minimum states = 3** ✓

---

**P9.** The language L = {aᵏ | k is a multiple of 2 or 3} is recognized by a DFA with minimum how many states?

**Solution:** Multiples of 2: {0,2,4,6,...}. Multiples of 3: {0,3,6,9,...}. Union: {0,2,3,4,6,8,9,10,...}
Missing: {1, 5, 7, 11, 13, ...} = numbers ≡ 1 or 5 (mod 6).

Track k mod 6: states 0,1,2,3,4,5
- Accept: {0,2,3,4} (0,2,4 are multiples of 2; 0,3 are multiples of 3)
- Reject: {1,5}

Can we merge any states? Check: states 1 and 5 both reject.
- δ(1,a)=2, δ(5,a)=0. State 2 accepts, state 0 accepts. Same.
- δ(2,a)=3, δ(0,a)=1. State 3 accepts, state 1 rejects. Different!
So 1 and 5 go to different-type states after TWO steps, but after one step both go to accepting states. Need to check if 2 ≡ 0... no, check deeper.

Actually 1→2→3→4→5→0→1 and 5→0→1→2→3→4→5. Let's see:
- From 1: a→2(accept), aa→3(accept), aaa→4(accept), aaaa→5(reject),aaaaa→0(accept)
- From 5: a→0(accept), aa→1(reject)

With suffix "a": both go to accepting states (2 and 0). With suffix "aa": from 1→3(accept), from 5→1(reject). DIFFERENT! → 1 and 5 are distinguishable.

**Minimum states = 6** (no merges possible)

---

**P10.** Design NFA for L = {w ∈ {a,b}* | the third symbol from the end is 'a'}

**Solution:** NFA "guesses" when there are 3 symbols left:
```
→ q₀ ──a──→ q₁ ──a,b──→ q₂ ──a,b──→ ((q₃))
  │
  └──a,b──→ q₀ (self-loop)
```

**NFA: 4 states.** Equivalent minimum DFA: 2³ = **8 states** (this is a known tight example!).

---

## 📝 Practice Set 2: Regular Expressions and Pumping Lemma (10 Problems) ⭐⭐

**P11.** Write a regular expression for L = {w ∈ {0,1}* | every 0 in w is followed by at least one 1}

**Solution:** The string cannot end with 0, and no "00" can appear without a 1 between them.
Actually: every 0 must be followed by at least one 1.
Valid strings: start with 1's, then blocks of (0 followed by 1+).
**RE: 1*(01⁺)*  = (1 + 01⁺)***

Or equivalently: **(1 | 01)*** — any string from {1, 01}*

Hmm, (1|01)* works: it generates strings where every 0 is immediately followed by 1. But we need "followed by AT LEAST one 1" not "immediately followed." If there are multiple 0's in a row, each needs a following 1.

Actually "every 0 is followed by at least one 1" means: there's no 0 that is the last character AND there's no 0 with only 0's after it... Actually it means: for every position i where w[i]=0, ∃ j>i where w[j]=1.

Simplest: **no 0 at the end!** Plus every 0 eventually has a 1 after it → **the string doesn't end in 0** → **(0+1)*1 + ε** → **but also must not have 0 at the very end.**

Wait: actually the condition "every 0 is followed by at least one 1" is equivalent to "the string does not end with 0" since if the last 0 has a 1 after it, we're fine.

**RE: (0+1)*1 + ε = (0|1)*1 | ε**

Or more cleanly: **(1+01)*** ← this is wrong, it doesn't allow sequences like "011".

Let me reconsider: The condition is simply that w doesn't end with '0' (or w = ε).
**RE: ε + (0+1)*1**

---

**P12.** Are the following regular expressions equivalent? 
R₁ = (a+b)*a(a+b)*  and  R₂ = (a+b)*a

**Solution:** 
- R₁ accepts any string containing at least one 'a' (then anything after)
- R₂ accepts any string ending with 'a'
- String "ab" ∈ R₁ (the 'a' is followed by 'b') but "ab" ∉ R₂ (ends with 'b')
  Wait: R₂ = (a+b)*a. "ab" — does it match? (a+b)* matches 'a', then we need an 'a', but we have 'b'. So we try: (a+b)* matches ε, then 'a' matches 'a', but we still have 'b' remaining → no match. Or (a+b)* matches "a", then need 'a' but see 'b' → no.
  
**R₁ ≠ R₂** ("ab" ∈ R₁ but ∉ R₂) ✓

---

**P13.** Use the pumping lemma to prove L = {0ⁿ1ᵐ | n > m ≥ 0} is not regular.

**Solution:**
1. Assume regular, pumping length p
2. Choose w = 0^(p+1) 1^p ∈ L (p+1 zeros, p ones; p+1 > p ✓)
3. Split w=xyz with |y|>0, |xy|≤p: y = 0ᵗ for some t ≥ 1 (entirely in 0 section)
4. Pump i=0: xz = 0^(p+1-t) 1^p. Need p+1-t > p → 1 > t. But t ≥ 1 → 1 ≤ t → p+1-t ≤ p.
   - If t=1: 0^p 1^p → n=m → NOT n>m → ∉ L ✗
   - If t>1: n < m → ∉ L ✗
5. **Not regular** ✓

---

**P14.** Is L = {w ∈ {a,b}* | |w| is even} regular? If yes, give RE.

**Solution:** YES, it's regular. DFA has 2 states (even/odd length).
**RE: ((a+b)(a+b))***

---

**P15.** Prove L = {aⁿ | n ≥ 0 and n is not a perfect square} is not regular.

**Solution:** Instead of proving L directly, note that if L were regular, then L' = complement(L) = {aⁿ | n IS a perfect square} would also be regular (regular languages closed under complement). But we know {aⁿ | n is a perfect square} is not regular (gap between consecutive squares grows). **So L is not regular.** ✓

Note: This uses closure under complement rather than pumping lemma!

---

**P16.** Convert DFA to RE using state elimination:
States {A,B}, Σ={0,1}, Start=A, Final={B}
δ(A,0)=A, δ(A,1)=B, δ(B,0)=B, δ(B,1)=A

**Solution:**
Add new start qs →ε→ A, B →ε→ qf

Eliminate A:
- Incoming: qs →ε→ A, B →1→ A
- Self-loop: 0
- Outgoing: A →1→ B

New edges:
- qs → 0*1 → B
- B → 10*1 → B

After eliminating A:
qs → 0*1 → B → (10*1)* → qf

But B also has self-loop 0:
B total self-loop: 0 + 10*1

Eliminate B:
qs → 0*1 → B, B self-loop = (0 + 10*1), B →ε→ qf

**RE: 0*1(0 + 10*1)***

---

**P17.** How many equivalence classes does L = {w ∈ {a}* | |w| mod 4 ≠ 2} have under ≡_L?

**Solution:** Equivalence classes based on length mod 4: {0, 1, 2, 3}
- Class 0 (length ≡ 0 mod 4): Accept
- Class 1 (length ≡ 1 mod 4): Accept
- Class 2 (length ≡ 2 mod 4): Reject
- Class 3 (length ≡ 3 mod 4): Accept

Are any of the accepting classes equivalent?
- Class 0 vs Class 1: Distinguished by suffix "aa" (0+2≡2 reject, 1+2≡3 accept) → Different
- Class 0 vs Class 3: Distinguished by suffix "aaa" (0+3≡3 accept, 3+3≡2 reject) → Different

Wait: 3+3=6≡2 mod 4 → reject, 0+3=3 → accept. So yes, different.

**4 equivalence classes → minimum 4 DFA states** ✓

---

**P18.** Given RE R = (0+1)*00(0+1)*, how many states does the minimum DFA have?

**Solution:** L = strings containing "00". Track progress toward "00":
- q₀: Haven't started matching (or last char was 1)
- q₁: Last char was 0
- q₂: Seen "00" — ACCEPT (absorbing state)

| State | On 0 | On 1 |
|---|---|---|
| q₀ | q₁ | q₀ |
| q₁ | q₂ | q₀ |
| q₂ | q₂ | q₂ |

**Minimum states = 3** ✓

---

**P19.** If L is regular and L = L¹ (L concatenated with itself gives L), what can you say about L?

**Solution:** L = LL means every string in L can be written as concatenation of two strings from L.
- ε ∈ L (because if w ∈ L then w = εw, so ε must be the "left half" for some decomposition)
  Wait, that's not necessarily true. L = LL means w ∈ L iff ∃ x,y ∈ L: w = xy.
  
If ε ∈ L: then for any w ∈ L, w = ε·w ∈ LL = L ✓ and w = w·ε ∈ LL = L ✓.
So L ⊆ LL is automatic. For LL ⊆ L: concatenation of two L-strings must be in L.

This means L is closed under concatenation AND L = LL.
Examples: L = Σ* satisfies this. L = {ε} does (εε = ε). L = Σ⁺ doesn't (ε ∉ Σ⁺... wait Σ⁺Σ⁺ = Σ⁺Σ⁺ = strings of length ≥ 2 ≠ Σ⁺).

**If L = LL then L = L* (the language is its own Kleene star minus ε... actually L = L⁺ at minimum).**

This is actually: L = L* if ε ∈ L, since L ⊆ L² ⊆ L³ ⊆ ... all equal L.

---

**P20.** Star height of (a+b)*a(a+b)* — what is it?

**Solution:** Star height = maximum nesting depth of Kleene star operations.
In (a+b)*a(a+b)*: the star depth is 1 (single level of *). **Star height = 1** ✓

---

## 📝 Practice Set 3: CFG and PDA (10 Problems) ⭐⭐

**P21.** Design an unambiguous CFG for the set of balanced parentheses {(), (()), ()(), (())(), ...}

**Solution:**
S → (S)S | ε

This is unambiguous because:
- The first '(' always matches with its corresponding ')' found by counting
- The S inside handles nesting, the S after handles seqeuncing
- Unique leftmost derivation for each string

Example: "(()())" → S → (S)S → ((S)S)S → (()S)S → (()(S)S)S → (()()S)S → (()())S → (()())

---

**P22.** Convert to CNF: S → aAB, A → bB | a, B → aA | BAa

**Solution:**
Step 1: No ε or unit productions.

Step 2: Replace terminals in mixed productions:
Cₐ → a, C_b → b
S → CₐAB → CₐD₁ where D₁ → AB
A → C_bB | a
B → CₐA | BAD₂ where... wait: B → BAa. Need to handle BAa.
B → BAa → BD₃ where D₃ → ACₐ? No: BAa → BD₄ where D₄ → ACₐ.

Let me redo: B → BAa. Replace a → Cₐ: B → BACₐ. Break: B → BD₅, D₅ → ACₐ.

Also B → aA → CₐA — already binary.

**CNF:**
```
S → CₐD₁      D₁ → AB
A → C_bB | a
B → CₐA | BD₅  D₅ → ACₐ
Cₐ → a         C_b → b
```

---

**P23.** Is the grammar S → SS | (S) | ε ambiguous? 

**Solution:** Check string "(())":
- Derivation 1: S → SS → (S)S → ((S))S → (())S → (())
  Wait: S → SS → εS = S? Actually S → SS → (S)S → ((S))S → (())S → (())ε = (())
  Hmm, but how does the second S become ε?

- Derivation 1: S → (S) → ((S)) → (())
- Derivation 2: S → SS → (S)S → (())S → (())  (second S → ε)
  This gives two leftmost derivations! So **YES, ambiguous** for "(())".

**Unambiguous version:** S → (S)S | ε (same grammar but... wait, this IS the same grammar we said was unambiguous in P21! Let me recheck.)

The grammar S → SS | (S) | ε has TWO ways to derive "(())": directly via S→(S) or via S→SS→S·ε=S. 

But S → (S)S | ε has only ONE derivation of "(())": S → (S)S → ((S)S)S → (()S)S → (())S → (()) → (()). Actually: S → (S)S → ((S)S)S → needed for "(())" let me trace:

S → (S)S → ((S)S)S → ... this gives "(...)..." and is getting complicated.

Actually for "(())":
S → (S)S → ((S)S)S ? No, the inner S should generate "()" not "((..."
S = (S)S. The first S (inside parens) = () and second S = ε.
Inner: S → (S)S → (ε)S → ()S → ()ε = ()... but I need "(())" not "()()"

For "(())": S → (S)S where inner S = "()" and outer S = ε:
S → (S)S → ((S)S)S → ((ε)S)S → (()S)S → (()ε)S → (())S → (())ε = "(())" ✓

Only one derivation! So S → (S)S | ε is **unambiguous**. But S → SS | (S) | ε is **ambiguous** ✓.

---

**P24.** Prove that L = {aⁱbʲcᵏ | i ≠ j AND j ≠ k} is CFL.

**Solution:** Hmm, this is tricky. 
L = {aⁱbʲcᵏ | i ≠ j ∧ j ≠ k}
= {aⁱbʲcᵏ | i ≠ j} ∩ {aⁱbʲcᵏ | j ≠ k}

But CFL ∩ CFL is not necessarily CFL! So this approach doesn't work.

Actually, L = complement of ({aⁱbʲcᵏ | i=j} ∪ {aⁱbʲcᵏ | j=k}) intersected with a*b*c*.

This is complement of a CFL intersected with a regular language... but CFL is not closed under complement.

**Actually, L = {aⁱbʲcᵏ | i ≠ j ∧ j ≠ k} might NOT be CFL.** Let me check.

Using the CFL pumping lemma on this language is complex. Actually, this is a well-known example: **it IS context-free** but the proof is non-trivial.

L = (L₁ ∩ a*b*c*) where... actually let me look at it differently.

{aⁱbʲcᵏ | i≠j, j≠k} can be decomposed as:
= {aⁱbʲcᵏ | i>j, j>k} ∪ {aⁱbʲcᵏ | i>j, j<k} ∪ {aⁱbʲcᵏ | i<j, j>k} ∪ {aⁱbʲcᵏ | i<j, j<k}

Each of these four languages IS CFL (each involves comparing only two of the three counts at a time in the same direction). CFL is closed under union → **L is CFL** ✓

---

**P25.** Design a PDA for L = {aⁿbᵐ | n ≠ m, n,m ≥ 1}

**Solution:** n ≠ m means n > m OR n < m. Design PDA for each and combine with ε-transitions (union).

**PDA for n > m:** Push a's. Pop one per b. After all b's, stack still has A's → accept.
**PDA for n < m:** Push a's. Pop one per b. Stack empties before all b's read, still more b's → accept.

Combined PDA uses nondeterminism at the start to "guess" which case.

---

**P26.** For CFG: S → AB, A → 0A1 | 01, B → 2B | 2, what language does this generate?

**Solution:**
A generates: 0ⁿ1ⁿ for n ≥ 1
B generates: 2ᵐ for m ≥ 1

S = AB generates: {0ⁿ1ⁿ2ᵐ | n ≥ 1, m ≥ 1}

---

**P27.** Is the following grammar ambiguous? S → aS | Sa | a

Derive "aa":
- Leftmost 1: S → aS → aa
- Leftmost 2: S → Sa → aa

Two different leftmost derivations → **YES, ambiguous** ✓

---

**P28.** Design DPDA for L = {aⁿb²ⁿ | n ≥ 0}

**Solution:** Push TWO symbols per 'a', pop one per 'b'. Accept when stack empty.

```
δ(q₀, a, Z₀) = (q₀, AAZ₀)    Push 2 A's for first 'a'
δ(q₀, a, A) = (q₀, AAA)       Push 2 A's (net +2, but top is A so push 3 keep 1 popped... )
```

Actually: δ(q₀, a, Z₀) = (q₀, AAZ₀) means replace Z₀ with AAZ₀ (push 2 A's above Z₀).
δ(q₀, a, A) = (q₀, AAA) means replace top A with AAA (push net 2 more A's).

```
δ(q₀, b, A) = (q₁, ε)       First b → switch state, pop 1 A
δ(q₁, b, A) = (q₁, ε)       Continue popping
δ(q₁, ε, Z₀) = (qf, Z₀)    Stack empty → accept
δ(q₀, ε, Z₀) = (qf, Z₀)    Accept ε (n=0)
```

This is **deterministic** — no nondeterministic choices! ✓

---

**P29.** How many variables and productions does the CNF grammar have if original grammar has k variables and p productions?

**Solution:** 
- New terminal variables: up to |Σ| new variables (one per terminal)
- Breaking long productions: a production of length n on RHS creates n-2 new variables and n-1 new productions
- **Rough bound:** O(p × max_production_length) variables and productions

For GATE: the exact number depends on the specific grammar. Key formula: CNF derivation of string length n takes **2n − 1** steps.

---

**P30.** Earley vs CYK comparison:

| Feature | CYK | Earley |
|---|---|---|
| Grammar requirement | CNF only | Any CFG |
| Worst-case time | O(n³|G|) | O(n³|G|) |
| Unambiguous grammar | O(n³) | O(n²) |
| Deterministic (LR) | O(n³) | O(n) |
| Space | O(n²) | O(n²) |
| Approach | Bottom-up DP | Top-down with memoization |
| GATE relevance | High (algorithm asked) | Low (just know it exists) |

---

## 📝 Practice Set 4: TM, Decidability, Rice's Theorem (15 Problems) ⭐⭐⭐

**P31.** Classify each problem as Decidable (D), RE-not-Recursive (RE), co-RE-not-Recursive (co-RE), or Neither (N):

| Problem | Answer | Reasoning |
|---|---|---|
| a) Does DFA M have exactly 5 states? | D | Count states (structural) |
| b) Does TM M accept string "hello"? | RE | A_TM: simulate M on "hello" |
| c) Is L(M) = ∅ for TM M? | co-RE | "∃ w M accepts" is RE → complement is co-RE |
| d) Is L(M) finite for TM M? | Neither | Rice's; L_fin is co-RE, complement is co-RE too |
| e) Is L(M₁) = L(M₂) for TMs M₁, M₂? | Neither | Neither RE nor co-RE |
| f) Does TM M halt on empty string? | RE | Specific case of halting problem |
| g) Does TM M have exactly 100 transitions? | D | Structural property (examine description) |
| h) Does TM M halt on ALL inputs? | co-RE | If not universal, find counterexample |
| i) Is L(M) a CFL? | Neither | Rice's — about language property |
| j) Does PDA P accept "abc"? | D | Simulate PDA (finite computation per input) |

> 🎯 **GATE Trick:** 
> - **Structural questions about M** (states, transitions) → usually Decidable
> - **"Does M do X on specific input"** → usually RE (simulate)
> - **"Does M do X on ALL inputs"** → usually co-RE or undecidable
> - **"What is L(M) like?"** → usually undecidable (Rice's)

---

**P32.** Which of the following are decidable?
a) Is L(G₁) ∩ L(G₂) = ∅ for regular grammars G₁, G₂?
b) Is L(G₁) ∩ L(G₂) = ∅ for CFGs G₁, G₂?
c) Is L(G) = Σ* for CFG G?
d) Is L(G) = Σ* for regular grammar G?

**Solution:**
a) Yes — intersection of two regular languages is regular. Emptiness of regular language is decidable. ✓
b) No — CFL intersection might not be CFL, and intersection emptiness for two CFLs is undecidable. ✗
c) No — CFL universality is undecidable. ✗
d) Yes — Regular universality is decidable (complement and check emptiness). ✓

---

**P33.** Using Rice's theorem, which of the following are undecidable?
a) L(M) contains exactly 42 strings
b) L(M) is a subset of {0,1}*
c) L(M) = L(M) (trivially true)
d) |L(M)| is even
e) L(M) is closed under reversal

**Solutions:**
a) Non-trivial language property → **Undecidable** ✓
b) Since Σ = {0,1}, L(M) ⊆ {0,1}* is trivially TRUE for all TMs over this alphabet → TRIVIAL → **Rice's doesn't apply → Decidable** (always YES)
c) Trivially TRUE for all TMs → TRIVIAL → **Decidable** (always YES)
d) Non-trivial language property → **Undecidable** ✓
e) Non-trivial language property → **Undecidable** ✓

---

**P34.** Prove that "Does TM M accept at least one palindrome?" is undecidable.

**Solution:** Rice's Theorem!
- Language property? Yes — "L(M) contains a palindrome" depends only on L(M)
- Non-trivial? 
  - TM that accepts nothing: L(M) = ∅ → no palindrome → false
  - TM that accepts "aba": "aba" is a palindrome → true
- Non-trivial ✓ → **Undecidable by Rice's** ✓

---

**P35.** Design a TM that DECIDES (not just recognizes!) {w#w | w ∈ {0,1}*}

**Strategy:**
1. Scan for '#'. If not found, reject.
2. Mark first unmarked symbol before '#'. Remember it (use states to track 0 vs 1).
3. Cross it out (mark as X).
4. Scan right past '#' to first unmarked symbol after '#'.
5. Compare. If different → reject. If same → mark and go back.
6. Repeat until all symbols before '#' are marked.
7. Check that all symbols after '#' are also marked. If yes → accept. If no → reject.

This TM always halts: it either accepts or rejects. It never loops because each pass marks two more symbols.

**States:** q_start, q_seek#(0), q_seek#(1) (remembering which symbol to match), q_match(0), q_match(1), q_return, q_check, q_accept, q_reject

**Time: O(n²)** — each match takes O(n) time, n/2 matches needed.

---

**P36.** The complement of a RE language that is not recursive is:
(a) RE  (b) Not RE  (c) Recursive  (d) Could be any

**Answer: (b) Not RE**

If L is RE but not recursive, and complement(L) were RE, then L would be both RE and co-RE → L would be recursive. Contradiction! So complement(L) is NOT RE. ✓

---

**P37.** A language L is such that both L and L' (complement) are RE. Then L is:
(a) Regular  (b) CFL  (c) Recursive  (d) RE but not recursive

**Answer: (c) Recursive** — by the fundamental theorem (L is decidable iff both L and L' are RE).

---

**P38.** If A is RE, B is recursive, which of the following is always RE?
a) A ∪ B  b) A ∩ B  c) A − B  d) All of the above

**Answer: (d) All of the above**
- RE is closed under ∪ and ∩ with RE (and recursive ⊂ RE)
- A − B = A ∩ B'. B is recursive → B' is recursive → B' is RE. A ∩ B' = RE ∩ RE = RE ✓

---

**P39.** What is the minimum number of tapes needed for a TM to recognize {aⁿbⁿcⁿ | n ≥ 1} in O(n) time?

**Answer:** 2 tapes suffice:
- Tape 1: input
- Tape 2: counter
- Pass 1: copy a's to tape 2, verify b count = tape 2 length, then c count = tape 2 length
- Each pass is O(n) → total O(n)

With 1 tape: O(n log n) best known (using crossing sequence argument, Ω(n log n) lower bound).

---

**P40.** Reduction: Show "Is L(M) infinite?" is undecidable for TMs.

**By Rice's Theorem:** 
- Language property? "L(M) is infinite" depends only on L(M) ✓
- Non-trivial? Some TMs have finite languages, some infinite ✓
- **Undecidable** ✓

---

**P41.** Which of the following is NOT undecidable?
a) Given CFG G, is L(G) = ∅?
b) Given CFG G₁, G₂, is L(G₁) ⊆ L(G₂)?
c) Given CFG G, is G ambiguous?
d) Given two DFAs M₁, M₂, is L(M₁) = L(M₂)?

**Answer: (a) and (d)** are decidable.
- (a) CFG emptiness is decidable (check if start symbol generates terminal string)
- (b) CFL inclusion is undecidable
- (c) CFG ambiguity is undecidable
- (d) DFA equivalence is decidable (minimize and compare, or check symmetric difference)

---

**P42.** Prove that PCP is undecidable using a reduction from the Halting Problem.

**Sketch:**
Given ⟨M, w⟩, construct PCP instance where:
- Pairs encode the computation history of M on w
- A solution to the PCP corresponds to a valid halting computation
- If M halts on w → PCP has a solution
- If M doesn't halt → no valid computation → no PCP solution

The actual reduction goes through MPCP first (Modified PCP where first pair must be used first), then MPCP → PCP.

---

**P43.** True or False:
a) Every RE language has a recursive subset → **TRUE** (at minimum, the finite subsets are recursive)
b) Every infinite RE language has an infinite recursive subset → **TRUE** (can enumerate elements and accept the first n)
c) The set of TMs that halt on all inputs is RE → **FALSE** (it's co-RE)
d) If L₁ ≤_m L₂ and L₂ is RE, then L₁ is RE → **TRUE** (definition of many-one reduction)
e) If L is RE and infinite, L must contain a regular subset that is infinite → **TRUE** (any infinite RE language contains an infinite regular subset — enumerate and accept specific patterns)

---

**P44.** In the Chomsky hierarchy, where does {aⁿbⁿcⁿdⁿ | n ≥ 0} belong?

**Answer:** Context-Sensitive Language (CSL = Type 1)
- Not CFL (fails CFL pumping lemma — pump can affect at most 2 of 4 sections)
- Is CSL (can be recognized by an LBA that marks and matches four sections)
- Is recursive (any CSL is recursive — LBA membership is decidable)

---

**P45.** A language L is accepted by a 2-counter machine. What can you say about L?

**Answer:** 2-counter machines are equivalent to Turing Machines! So L is recursively enumerable (RE).

If L is accepted by a 3-counter machine that always halts → L is recursive.

---

## 📝 Practice Set 5: Mixed GATE-Style MCQs (20 Questions)

**Q1.** The minimum number of states in a DFA that accepts binary strings divisible by 8 is:
(a) 4  (b) 8  (c) 9  (d) 16

**Answer: (c) 9** — divisible by 8 means last 3 bits are 000. Need to track the last 3 bits → 2³ = 8 states + 1 for leading zeros/start = 9 states... Actually, tracking value mod 8 gives 8 states. But we also need to handle leading zeros. If reading MSB first: 8 states suffice (track n mod 8). If reading unary: different.

For binary MSB first: δ(q, b) = (2q + b) mod 8. Start: state 0. Final: {0}. **8 states.** So answer is (b) 8.

Actually, for binary numbers: leading zeros don't matter for the mod computation, so 8 states suffice.

**Answer: (b) 8** ✓

---

**Q2.** If L₁ is recursive and L₂ is RE but not recursive, then L₁ ∪ L₂ is:
(a) Always recursive  (b) Always RE  (c) Always not RE  (d) Could be anything

**Answer: (b) Always RE** — RE is closed under union, and recursive ⊂ RE.

---

**Q3.** Which of the following is closed under complement?
(a) RE  (b) CFL  (c) DCFL  (d) CSL

**Answer: (c) DCFL and (d) CSL** — but if only one answer: **(c) DCFL** is the most notable one since CFL is NOT closed under complement but DCFL is.

---

**Q4.** A grammar G has productions: S → aB | bA, A → aS | bAA | a, B → bS | aBB | b
The language L(G) is:
(a) {w | #a = #b}  (b) {w | #a > #b}  (c) {w | #a ≠ #b}  (d) Σ*

**Answer: (a) {w ∈ {a,b}* | #a(w) = #b(w)}**

This is a classic grammar where S generates strings with equal a's and b's. Every production maintains the invariant: S → strings with equal counts, A → strings with one extra 'a', B → strings with one extra 'b'.

---

**Q5.** The number of parse trees for string "aaa" in grammar S → SS | a is:
(a) 1  (b) 2  (c) 3  (d) 5

**Answer: (b) 2**

Parse tree 1: S → SS → aS → aSS → aaS → aaa (left-heavy)
Actually, systematic enumeration:
- S → SS → (S)(SS) → a(SS) → a(a)(S) → aaa
- S → SS → (SS)(S) → (SS)a → (a)(S)a → (a)(a)a → aaa

With 3 leaves, Catalan number C₂ = 2. **Answer: (b) 2** ✓

For n leaves, number of parse trees = Catalan number C_{n-1} = $\binom{2(n-1)}{n-1}/(n)$

---

**Q6.** Let L = {⟨M⟩ | M is a TM that accepts at most 10 strings}. L is:
(a) Decidable  (b) RE not decidable  (c) Not RE  (d) Neither RE nor co-RE

**Answer:** Rice's theorem: "L(M) has at most 10 strings" is a non-trivial language property → undecidable.
More specifically: 
- To show L is co-RE: if |L(M)| > 10, we can enumerate and find 11 accepted strings → complement is RE → L is co-RE
- But complement of L ("L(M) has > 10 strings") is RE
- So L is co-RE but not decidable → **(c) Not RE** (it's co-RE, which means it's not RE since it's not decidable)

Wait: co-RE is not RE only if the language is not decidable. So L is co-RE but not RE → **answer (c)** ✓

---

**Q7.** DFA M₁ has 3 states, DFA M₂ has 5 states. Maximum states in minimum DFA for L(M₁) ∩ L(M₂)?
(a) 8  (b) 15  (c) 16  (d) 2¹⁵

**Answer: (b) 15** — product DFA has 3 × 5 = 15 states maximum. After minimization, might be fewer, but 15 is the maximum possible.

---

**Q8.** If DPDA P accepts L by final state, which is TRUE?
(a) L must be regular  (b) L must be CFL  (c) Complement of L is also accepted by some DPDA  (d) Both (b) and (c)

**Answer: (d)** — DPDA languages are DCFL, which is a CFL. DCFLs are closed under complement. ✓

---

**Q9.** An LBA has n states, m tape symbols, and input of length k. The maximum number of distinct configurations is:
(a) n × m^k × k  (b) n × k × m^k  (c) n^k × m  (d) (nm)^k

**Answer: (b) n × k × m^k** — configurations = state (n choices) × head position (k choices) × tape contents (m^k options per cell, k cells).

---

**Q10.** The language {aⁿ | n is a Fibonacci number} is:
(a) Regular  (b) CFL  (c) CSL  (d) RE but not CSL

**Answer: (c) CSL** — Fibonacci numbers can be computed by an LBA. Actually, it's decidable (we can check if n is Fibonacci in finite time), so it's recursive. Being recursive means it's CSL too... actually recursive ⊃ CSL. But the question asks which it IS.

Since Fibonacci numbers can be computed, membership is decidable:
- Given aⁿ, compute Fibonacci numbers up to n
- Check if n is among them
- This is decidable → it's recursive

But is it regular? It's not regular (Fibonacci gaps grow, failing pumping lemma). Is it CFL? Likely not (gaps grow super-linearly). Is it CSL? Yes (recursive → CSL? No, recursive ⊋ CSL).

Actually: Recursive properly contains CSL in the Chomsky hierarchy. But for the purpose of this question, {aⁿ | n ∈ Fibonacci} is decidable and we need to find the tightest class.

The tightest correct answer is probably **(c) CSL** — it can be recognized by an LBA (compute Fibonacci numbers on bounded tape). Being unary (over {a}), it's likely not CFL.

---

**Q11.** For two CFLs L₁ and L₂, which is always decidable?
(a) L₁ = L₂  (b) L₁ ⊆ L₂  (c) L₁ ∩ L₂ = ∅  (d) None of the above

**Answer: (d) None of the above** — all three are undecidable for CFLs. ✓

---

**Q12.** If A ≤_m B and B ≤_m A, then:
(a) A and B are the same language  (b) A and B have the same complexity  (c) A is decidable iff B is decidable  (d) A = B'

**Answer: (c)** — many-one equivalent languages have the same decidability status. They're not necessarily equal or complements, but they're equivalent in terms of decidability/recognizability.

---

**Q13.** How many states does the minimum DFA for L = {w ∈ {a,b}* | |w| ≥ 3} have?
(a) 3  (b) 4  (c) 5  (d) 6

**Answer: (c) 5** — states track length: 0, 1, 2, 3+.
Wait: to accept strings of length ≥ 3:
- q₀ (read 0 chars), q₁ (read 1), q₂ (read 2), q₃ (read ≥ 3, final, absorbing)
- That's 4 states. **Answer: (b) 4** ✓

---

**Q14.** The language accepted by a Turing Machine that always moves its head to the right is:
(a) Regular  (b) CFL  (c) CSL  (d) RE

**Answer: (a) Regular** — a TM that only moves right is essentially a DFA (finite control, reads input once left to right, no going back). ✓

---

**Q15.** Which is equivalent to a PDA?
(a) DFA with one queue  (b) DFA with one stack  (c) DFA with two stacks  (d) NFA with one counter

**Answer: (b) DFA with one stack** — that's literally what a DPDA is!
Note: (c) = TM, (a) is also TM-equivalent, (d) is weaker than PDA (counter ⊂ stack).

> 🎯 Actually (b) gives DPDA, not NPDA. A PDA is nondeterministic by default. So the answer depends on interpretation. If "PDA" means NPDA: none of the above is exactly right. If DPDA: (b) ✓.

---

**Q16-Q20: True/False Rapid Fire**

| # | Statement | T/F | Explanation |
|---|---|---|---|
| Q16 | Every NFA can be simulated by a DFA with the same number of states | F | May need exponentially more |
| Q17 | L₁ is regular, L₂ is not regular → L₁ ∪ L₂ is not regular | F | If L₂ ∪ L₁ = Σ* (regular), possible |
| Q18 | {ww^R | w ∈ {a,b}*} is DCFL | F | Needs nondeterminism (guess middle) |
| Q19 | If L is CFL and R is regular, then L - R is CFL | T | L - R = L ∩ R'. R' is regular. CFL ∩ Regular = CFL |
| Q20 | Every RE language has a decidable superset | T | Σ* is decidable and contains everything |

---

## 🎯 20 GATE Tricks for TOC

1. **NFA to DFA MAX states:** n-state NFA → at most 2ⁿ DFA states (tight for "n-th from end" languages)
2. **Minimum DFA for mod k:** Language {w | f(w) mod k = r} has minimum k states
3. **Pumping lemma for RL:** y must be within first p characters (|xy| ≤ p)
4. **Pumping lemma for CFL:** v and y span at most 2 sections of the string
5. **CFL ∩ Regular = CFL:** Most useful closure property for GATE!
6. **DCFL closed under complement, CFL is NOT:** Key differentiator
7. **Rice's Theorem Shortcut:** Language property? Non-trivial? → Undecidable!
8. **CFG emptiness:** Decidable! (Find generating variables bottom-up)
9. **CFG equivalence:** Undecidable! (Don't confuse with DFA equivalence which IS decidable)
10. **HALT is RE not decidable; complement(HALT) is not RE:** Classic example
11. **L decidable ↔ L and L' both RE:** The fundamental theorem
12. **Complement of NFA is NOT "swap finals":** Must convert to DFA first!
13. **CYK needs CNF:** Always convert grammar to CNF before applying CYK
14. **CNF derivation length:** Exactly 2n-1 for string of length n
15. **GNF derivation length:** Exactly n for string of length n
16. **Two stacks = TM:** Adding a second stack to a PDA gives TM power
17. **Quotient preserves regularity:** L/R is regular if L is regular (R can be anything!)
18. **Star height:** Maximum nesting of * in RE — GATE rarely tests this
19. **Myhill-Nerode for counting:** Number of equivalence classes = minimum DFA states
20. **Cantor's argument:** TMs are countable, languages are uncountable → most problems are unsolvable

---

## 📋 Formula Card / Quick Reference ⭐⭐⭐

### Automata Size Bounds

| Conversion | Size Bound |
|---|---|
| NFA (n states) → DFA | At most 2ⁿ states |
| ε-NFA (n states) → NFA | Same n states (just compute ε-closures) |
| DFA → Minimum DFA | ≤ original states (table-filling) |
| RE → NFA | ≤ 2|RE| states (Thompson's) |
| CFG → PDA | O(|G|) states |
| PDA → CFG | O(|Q|²|Γ|) variables |

### Language Class Relationships

```
Regular ⊂ DCFL ⊂ CFL ⊂ CSL ⊂ Recursive ⊂ RE ⊂ All Languages
                                              ↕
                                           co-RE
                                    RE ∩ co-RE = Recursive
```

### Decidability Quick Reference

| Problem | Regular | DCFL | CFL | Recursive |
|---|---|---|---|---|
| w ∈ L? | ✓ O(n) | ✓ O(n) | ✓ O(n³) | ✓ |
| L = ∅? | ✓ | ✓ | ✓ | ✗ |
| L finite? | ✓ | ✓ | ✓ | ✗ |
| L = Σ*? | ✓ | ✓ | ✗ | ✗ |
| L₁ = L₂? | ✓ | ✓ | ✗ | ✗ |
| L₁ ⊆ L₂? | ✓ | ✓ | ✗ | ✗ |
| L₁ ∩ L₂ = ∅? | ✓ | ✗ | ✗ | ✗ |
| Is L regular? | — | ✗ | ✗ | ✗ |

### Parsing Algorithm Comparison

| Algorithm | Grammar Type | Time | GATE Importance |
|---|---|---|---|
| DFA simulation | Regular | O(n) | High |
| CYK | CFG (CNF) | O(n³) | High |
| Earley | Any CFG | O(n³), O(n) for LR | Low |
| LL(1) | LL(1) subset of CFG | O(n) | High (Compiler) |
| LR(1) | LR(1) subset of CFG | O(n) | High (Compiler) |
| GLR | Any CFG | O(n^p), p=ambiguity | Very Low |

---

## 📋 Commonly Confused Pairs in TOC ⭐⭐

| Concept A | Concept B | Key Difference |
|---|---|---|
| DFA complement (swap finals) | NFA complement | ✓ for DFA, ✗ for NFA (must convert first!) |
| CFL closed under ∪ | CFL closed under ∩ | ✓ for ∪, ✗ for ∩ |
| Decidable | RE | Decidable always halts; RE may loop on NO |
| Rice's: language property | Rice's: TM structure | Only language properties are undecidable |
| RE language | Regular language | Very different! RE = Turing-recognizable |
| PDA by final state | PDA by empty stack | Equivalent for NPDA, NOT for DPDA! |
| DPDA | NPDA | DPDA ⊂ NPDA (strictly weaker) |
| CFL pumping: \|vy\| > 0 | Regular pumping: \|y\| > 0 | CFL pumps TWO parts, Regular pumps ONE |
| Ambiguous grammar | Inherently ambiguous language | Grammar: specific grammar has 2 parse trees. Language: ALL grammars do |
| Myhill-Nerode classes | DFA states | They are EQUAL for minimum DFA |
| Halting problem | Rice's theorem | Halting is about specific (M,w); Rice's is about L(M) properties |
| Recursive | Recursively Enumerable | Recursive ⊂ RE. Recursive = always halts |
| co-RE | Not RE | co-RE = complement is RE. "Not RE" = no TM recognizes it |
| CSL | CFL | CSL: αAβ→αγβ. CFL: A→α. CSL has context, CFL doesn't |
| LBA | TM | LBA = restricted tape (input length). TM = unlimited tape |

---

## 🔄 Last-Minute Revision Sheet

### The 10 Most Important Facts for GATE TOC:

1. **NFA = DFA in power** (but NFA may be exponentially smaller)
2. **CFL ∩ Regular = CFL** (the closure property you'll use most)
3. **CFL is NOT closed under ∩ and complement** (unlike Regular)
4. **DCFL IS closed under complement** (but NOT ∪, ∩, ·, *)
5. **Rice's Theorem:** Non-trivial language property of TM → Undecidable
6. **L decidable ↔ L and L' are both RE**
7. **HALT is RE but not decidable** (the canonical undecidable problem)
8. **Pumping Lemma proves NON-membership in a class** (never proves membership)
9. **Minimum DFA states = Myhill-Nerode equivalence classes** (uniquely determined)
10. **Two stacks / queue / 2+ counters = TM power** (the jump from PDA to TM)

### Common GATE Traps:

| Trap | Correct Answer |
|---|---|
| "Regular expression means RE (recognizable)" | NO — "RE" in RE language = recursively enumerable |
| "Every subset of a CFL is CFL" | NO — {aⁿbⁿcⁿ} ⊂ {aⁱbʲcᵏ} but not CFL |
| "Complement of RE is always RE" | NO — complement of RE may not be RE |
| "NFA complement: just swap final states" | NO — only works for DFA |
| "If grammar is unambiguous, language is not inherently ambiguous" | TRUE — but some languages have NO unambiguous grammar |
| "L₁ regular, L₂ irregular → L₁ ∪ L₂ is irregular" | NOT NECESSARILY — could be Σ* |
| "CFG emptiness is undecidable" | NO — it's decidable! (Rice's applies to TMs, not CFGs) |
| "Every CFL is DCFL" | NO — {ww^R} is CFL but not DCFL |

---

## 📝 Practice Set 6: GATE PYQ-Style Problems (10 Problems)

**P46.** (GATE 2023 style) Consider the language L = {aⁿbⁿ | n ≥ 0} ∪ {aⁿb²ⁿ | n ≥ 0}. Which of the following is true?
(a) L is regular  (b) L is DCFL but not regular  (c) L is CFL but not DCFL  (d) L is not CFL

**Solution:** 
- {aⁿbⁿ} is DCFL
- {aⁿb²ⁿ} is DCFL
- Union of two DCFLs: DCFLs not closed under union → might not be DCFL
- But it IS CFL (CFLs closed under union)
- Is it DCFL? For DPDA: after pushing a's, when reading b's, how to decide between matching 1:1 or 1:2? Can't decide deterministically!
- **Answer: (c) CFL but not DCFL** ✓

---

**P47.** (GATE 2022 style) The minimum number of states in DFA for L = {w ∈ {0,1}* | the number of 01 substrings in w is even} is:

**Solution:** States = (parity, last_char_was_0)
4 states: (Even, not-0), (Even, is-0), (Odd, not-0), (Odd, is-0)
After minimization: all 4 are distinguishable.
**Answer: 4 states** ✓

---

**P48.** (GATE 2024 style) Which of the following problems is decidable?
I. Given CFG G, is L(G) = {a}*?
II. Given DFA M, is L(M) finite?
III. Given TM M, does M halt on string ε?
IV. Given CFG G₁, G₂, is L(G₁) ∩ L(G₂) = ∅?

**Answer: Only II** — DFA finiteness is decidable. I is CFL universality (undecidable). III is halting problem. IV is CFL intersection emptiness (undecidable). ✓

---

**P49.** The minimum DFA for palindromes over {a,b} has how many states?

**Answer:** Palindromes are NOT regular → **no finite DFA exists** ✓

---

**P50.** (GATE 2020 style) Minimum DFA states for L = (a+b)*abb?

**Solution:** Track suffix toward "abb": q₀(none), q₁(a), q₂(ab), q₃(abb-accept).
**Answer: 4 states** ✓

---

**P51.** How many strings of length 4 are accepted by DFA for "contains at least one 1" over {0,1}?

**Solution:** Total 2⁴=16. Only "0000" rejected. **Answer: 15** ✓

---

**P52.** Design CFG for L = {aⁱbʲ | 2i = 3j}

**Solution:** i=3k, j=2k. Grammar: **S → aaaSbb | ε** ✓

---

**P53.** Is L = {aⁱbʲcᵏ | i=j or i=k} CFL?

**Solution:** Yes — L₁ = {aⁱbⁱcᵏ} ∪ L₂ = {aⁱbʲcⁱ}, both CFL, union is CFL.
Not DCFL (nondeterministic choice needed). **Answer: CFL but not DCFL** ✓

---

**P54.** NFA has 3 accepting computations on "ab". Is "ab" accepted?

**Solution:** YES — even one accepting path suffices. ✓

---

**P55.** h(a)=01, h(b)=10. If L={0ⁿ1ⁿ | n≥1}, what is h⁻¹(L)?

**Solution:** Only w="a" maps to h(a)=01=0¹1¹ ∈ L. **h⁻¹(L) = {a}** (finite, regular) ✓

---

## 📝 Practice Set 7: Advanced Closure and Classification (15 Problems)

**P56.** L₁ = {aⁿbⁿ | n≥0}, L₂ = {bⁿcⁿ | n≥0}. Classify L₁L₂ (concatenation).

**Solution:** L₁L₂ = {aⁿbⁿbᵐcᵐ | n,m ≥ 0} = {aⁿbⁿ⁺ᵐcᵐ | n,m ≥ 0}
CFG: S → AB; A → aAb | ε; B → bBc | ε. **CFL** ✓

---

**P57.** L is CFL, R is regular. Is L/R (right quotient) CFL?

**Solution:** Yes! L/R = {x | ∃y ∈ R: xy ∈ L}. For PDAs: CFL is closed under right quotient with regular languages (simulate PDA, check if remaining input can be accepted by the DFA for R). **CFL** ✓

---

**P58.** L₁ = {aⁿbⁿ | n≥0}, L₂ = {aⁿbⁿ | n≥0}. Is L₁ ∩ L₂ = L₁ CFL?

**Solution:** L₁ ∩ L₂ = L₁ = {aⁿbⁿ} which IS CFL! The fact that CFL is not closed under ∩ doesn't mean every intersection is non-CFL — just that it's not guaranteed. ✓

---

**P59.** Classify: L = {w | |w| is prime} over {a,b}

**Solution:**  
- Not regular (pumping lemma: pump changes length by |y|, adding |y| to a prime doesn't always give a prime)
- Is it CFL? No — over unary alphabet {a}, {aⁿ | n prime} is not CFL (gap between primes). Over {a,b}: L = {w ∈ {a,b}* | |w| is prime}. Apply intersection with a*: L ∩ a* = {aⁿ | n prime} which is not CFL. If L were CFL, L ∩ a* would be CFL (CFL ∩ Regular = CFL). Contradiction!
- **Not CFL, but decidable (recursive)** — we can check if |w| is prime in finite time. ✓

---

**P60.** True or False: If L₁ is regular and L₂ is RE but not recursive, then L₁ − L₂ is always RE.

**Solution:** L₁ − L₂ = L₁ ∩ L₂'. L₂ is RE not recursive → L₂' is NOT RE.
L₁ ∩ L₂' = ? Regular ∩ not-RE is not necessarily RE!

Actually: L₁ is recursive (all regular languages are). L₂' is not RE.
L₁ ∩ L₂' = strings in L₁ that are NOT in L₂.

If L₁ is finite: L₁ ∩ L₂' requires checking finitely many strings against L₂', but L₂' is not RE...

Actually, L₁ ∩ L₂' could be anything. Consider L₁ = Σ* (regular): then L₁ ∩ L₂' = L₂' which is NOT RE!

**FALSE** — L₁ − L₂ is not always RE. ✓

---

**P61.** If L is recursive and infinite, which is true?
(a) L must contain a regular infinite subset
(b) L must contain a CFL infinite subset  
(c) Both  (d) Neither

**Solution:** (c) Both! Any infinite recursive language contains infinite regular subsets.
Proof: Enumerate elements of L as w₁, w₂, w₃, ... The set {w₁, w₃, w₅, ...} is infinite and decidable.
Any decidable language over a finite alphabet that is infinite contains an infinite regular subset
(e.g., take every other string in length-sorted order → regular pattern). ✓

---

**P62.** GATE-style: Let n_min(L) be the minimum DFA states for language L. If n_min(L) = 5, what is n_min(complement(L))?

**Solution:** Complement of DFA: same DFA, swap final/non-final states. The minimum DFA for L and complement(L) have the **same number of states**!

**n_min(complement(L)) = 5** ✓

(Because Myhill-Nerode equivalence classes are the same for L and complement(L) — two strings are distinguishable for L iff they're distinguishable for complement(L))

---

**P63.** The language L = {aⁱbʲ | gcd(i,j) = 1} is:
(a) Regular  (b) CFL  (c) CSL  (d) RE but not CSL

**Solution:** This is decidable (can compute gcd in finite time), so it's recursive. Is it CFL?

Consider L ∩ {aᵖbʲ | j ≥ 0} for prime p: this gives {aᵖbʲ | gcd(p,j) = 1} = {aᵖbʲ | p ∤ j} which IS regular for fixed p.

But is L itself CFL? This is complex. The condition gcd(i,j)=1 involves a relationship between i and j that a PDA can't track (it's multiplicative, not additive). Likely NOT CFL.

**Answer: (c) CSL** (decidable, likely not CFL) ✓

---

**P64.** How many distinct languages over Σ = {a} can be recognized by DFAs with exactly 2 states?

**Solution:** With 2 states {q₀, q₁}, alphabet {a}:
- Transitions: δ(q₀,a) ∈ {q₀,q₁} and δ(q₁,a) ∈ {q₀,q₁} → 2² = 4 transition functions
- Final states: 4 choices (∅, {q₀}, {q₁}, {q₀,q₁})
- Total DFAs: 4 × 4 = 16

But many generate the same language. The languages are:
- ∅ (empty)
- {ε} (only empty string)
- a* (everything)
- {aⁿ | n is even} (including ε)
- {aⁿ | n is odd}
- a⁺ (non-empty)
- {ε, a} maybe? Let's see: δ(q₀,a)=q₁, δ(q₁,a)=q_dead... no, only 2 states.

With 2 states over {a}, the DFA forms a simple cycle or path. Possible languages:
1. ∅ — final states = ∅, or unreachable finals
2. Σ* = a* — both states final
3. {ε} — only start is final, transitions loop to non-final
4. a⁺ — non-start is final  
5. {aⁿ | n even} — start=final, alternating
6. {aⁿ | n odd} — non-start=final, alternating

Plus: start final + stay at start on a: a* (already counted).

After careful enumeration: **6 distinct languages** (∅, {ε}, a*, a⁺, even-length, odd-length)

Actually: {ε} requires start=final and δ(q₀,a)=q₁ (non-final), δ(q₁,a)=q₁ (stay non-final). ✓
a⁺ requires start=non-final, δ(q₀,a)=q₁ (final), δ(q₁,a)=q₁. ✓

**Answer: 6 distinct languages** ✓

---

**P65.** If A ≤_m B and B ∈ P (polynomial time decidable), can we conclude A ∈ P?

**Solution:** Only if the reduction f is computable in polynomial time! Many-one reduction ≤_m only guarantees computability, not polynomial time.

For polynomial-time conclusion, we need **polynomial-time many-one reduction** (≤_p or ≤_m^P).

If the reduction is computable (but not necessarily in polynomial time): A is decidable (guaranteed), but NOT necessarily in P.

**Answer:** No, not necessarily. Need polynomial-time reduction for P-class conclusions. ✓

---

**P66.** The complement of {⟨M⟩ | M is a TM and L(M) is finite} is:
(a) RE  (b) co-RE  (c) Neither  (d) Decidable

**Solution:** {⟨M⟩ | L(M) is finite} — by Rice's theorem, undecidable.
- FIN = {⟨M⟩ | L(M) is finite} is co-RE (if L(M) is infinite, enumerate to find witness)
  Wait: To show L(M) is infinite, we need to find infinitely many accepted strings — can't do in finite time!
  
  Actually: complement of FIN = INF = {⟨M⟩ | L(M) is infinite}. 
  INF: To verify "L(M) is infinite" — can we enumerate? Yes, find strings in L(M): run M on all strings, if we find k+1 accepted strings, we know |L(M)| ≥ k+1. For any k, if L(M) is truly infinite, we'll eventually find k+1 strings. So INF is RE!
  
  Therefore: FIN = complement of INF. INF is RE → FIN is co-RE.
  
  **Complement of FIN = INF = RE** → Answer: **(a) RE** ✓

---

**P67.** Total number of CFGs over V={S,A}, T={a,b} where each production has RHS of length ≤ 2?

**Solution:** Each production is of form X → α where X ∈ {S,A} and |α| ≤ 2, α ∈ {S,A,a,b,ε}*

Possible RHS of length ≤ 2: ε (1), single symbols (4: S,A,a,b), pairs (4²=16)
Total RHS options: 1 + 4 + 16 = 21

Each of S and A can have any subset of these 21 as productions.
Total grammars: 2²¹ × 2²¹ = 2⁴² ≈ 4.4 × 10¹²

(This is a theoretical count — most are garbage. But it shows CFGs are countable!)

---

**P68.** Given Turing Machine M with 5 states and tape alphabet {0,1,B}, what is the maximum number of transitions in δ?

**Solution:** δ: Q × Γ → Q × Γ × {L,R}
- Domain: |Q| × |Γ| = 5 × 3 = 15 possible (state, symbol) pairs
- Each maps to one of |Q| × |Γ| × 2 = 5 × 3 × 2 = 30 options
- But some (state, symbol) pairs might be undefined (halting)

**Maximum transitions: 15** (one per domain element, all defined)

If counting total possible TMs: 31^15 (30 options + undefined for each of 15 entries)

---

**P69.** Prove: The set of valid C programs is decidable.

**Proof:** A valid C program is a finite string that conforms to C grammar and type rules. A compiler can check this in finite time:
1. Lexical analysis (regular)
2. Syntax analysis (context-free → decidable)
3. Type checking (decidable additional checks)
4. All steps halt in finite time on any input

Therefore, the set of valid C programs is **decidable** ✓

**However:** "Does this C program halt?" is NOT decidable (halting problem)!
"Does this C program have bugs?" is NOT decidable (Rice's theorem — property of L(M))!

---

**P70.** Given DFA M₁ with p states and DFA M₂ with q states. The minimum DFA for L(M₁) ⊕ L(M₂) (symmetric difference) has at most how many states?

**Solution:** Symmetric difference = (L₁ − L₂) ∪ (L₂ − L₁) = (L₁ ∩ L₂') ∪ (L₂ ∩ L₁')

Using product construction: states = p × q = **pq states** maximum.

Final states for ⊕: {(f₁, n₂) | f₁ ∈ F₁, n₂ ∉ F₂} ∪ {(n₁, f₂) | n₁ ∉ F₁, f₂ ∈ F₂}

**Maximum: pq states** (might be less after minimization) ✓

---

## 📝 Practice Set 8: Comprehensive Mixed Problems (20 Problems)

**P71.** A DFA has 100 states. After minimization, minimum possible states is:
**Answer: 1** (if L = Σ* or L = ∅, minimum DFA has 1 state)

---

**P72.** The number of binary strings of length 10 accepted by DFA for "divisible by 3" is:
**Solution:** Strings of length exactly 10 representing numbers ≡ 0 mod 3:
- Total binary strings of length 10: 2¹⁰ = 1024
- Strings starting with leading zeros are just smaller numbers
- Numbers from 0 to 2¹⁰-1 = 1023 divisible by 3: ⌊1023/3⌋ + 1 = 342
- But that's numbers 0-1023. In 10-bit representation: includes leading zeros.
- Actually, every number from 0 to 1023 has a unique 10-bit representation.
- Numbers ≡ 0 mod 3: 0, 3, 6, ..., 1023. Count: (1023-0)/3 + 1 = 342.

**Answer: 342** ✓

---

**P73.** Minimum pumping length for L = {aⁿbⁿ | n ≥ 0} if we know the minimum DFA for L doesn't exist (L is not regular)?
**Answer:** The pumping lemma doesn't apply because L is not regular! The pumping lemma is about WHAT WOULD BE TRUE IF L were regular. Since L isn't regular, there's no actual pumping length. The question is moot.

But if asking "what's the minimum p such that the pumping lemma condition fails?": There IS no valid p (that's the whole point — the lemma is vacuously inapplicable in reverse).

---

**P74.** How many strings does the CFL {aⁿbⁿ | 0 ≤ n ≤ 100} contain?
**Answer:** n can be 0, 1, 2, ..., 100. That's **101 strings** (ε, ab, aabb, ..., a¹⁰⁰b¹⁰⁰).
This language is actually **finite** and therefore **regular**! ✓

---

**P75.** A PDA has 3 states, stack alphabet {A,B,Z}, input alphabet {0,1}. Maximum transitions?
**Solution:** δ: Q × (Σ ∪ {ε}) × Γ → P(Q × Γ*)
- Domain: 3 × 3 × 3 = 27 (state × input/ε × stack top)
- For DPDA: at most 27 transitions
- For NPDA: each can go to multiple (q, γ) pairs, so the number of transition entries is limited only by the finite description, but the maximum distinct "transition rules" is bounded by the number of possible (state, stack_string) targets.

For GATE purposes: **27 possible transition entries** for deterministic case. ✓

---

**P76.** L = {⟨M⟩ | M is a TM with exactly 5 states}. Is L decidable?
**Solution:** YES! This is a structural property of the TM (not a language property). We can examine the encoding ⟨M⟩ and count states. **Decidable** ✓

---

**P77.** L = {⟨M⟩ | M is a TM that makes at most 100 moves on any input of length ≤ 10}. Decidable?
**Solution:** YES! For each input of length ≤ 10 (finitely many), simulate M for 100 steps. If M always halts within 100 steps → accept. If any input causes >100 steps → reject. **Decidable** (finite computation) ✓

---

**P78.** True/False: Every infinite CFL contains an infinite regular subset.
**Answer: TRUE** — by the pumping lemma, we can find a pumpable string uvxyz. The set {uvⁱxyⁱz | i ≥ 0} is infinite and regular (it's generated by a regular pattern). ✓

---

**P79.** Rank these languages by their level in Chomsky hierarchy (lowest to highest):
A: {aⁿ | n ≥ 0}  B: {aⁿbⁿ | n ≥ 0}  C: {aⁿbⁿcⁿ | n ≥ 0}  D: HALT

**Answer:** A (Regular, Type 3) < B (CFL, Type 2) < C (CSL, Type 1) < D (RE, Type 0)

---

**P80.** If L₁ ≤_m L₂ and L₁ is not co-RE, which is true about L₂?
(a) L₂ is decidable  (b) L₂ is RE  (c) L₂ is not co-RE  (d) L₂ is not RE

**Answer: (c) L₂ is not co-RE**
If L₂ were co-RE, then L₁ (reducible to L₂) would also be co-RE. But L₁ is not co-RE. Contradiction.

Note: L₂ might or might not be RE — we can only conclude it's not co-RE. ✓

---

**P81.** DFA with states {s₀,s₁,s₂,s₃}, start=s₀, final={s₃}:
δ(s₀,a)=s₁, δ(s₀,b)=s₂, δ(s₁,a)=s₃, δ(s₁,b)=s₀, δ(s₂,a)=s₀, δ(s₂,b)=s₃, δ(s₃,a)=s₂, δ(s₃,b)=s₁

What language does this accept?

**Solution:** Trace some strings:
- aa: s₀→s₁→s₃ ✓
- bb: s₀→s₂→s₃ ✓  
- aabb: s₀→s₁→s₃→s₁→s₃ Hmm wait: s₁ on b → s₀. 
  Wait: aabb: s₀→ₐs₁→ₐs₃→ᵦs₁→ᵦs₀ ✗
- ab: s₀→s₁→s₀ ✗
- ba: s₀→s₂→s₀ ✗
- aab: s₀→s₁→s₃→s₁ ✗
- aba: s₀→s₁→s₀→s₁ ✗
- abba: s₀→s₁→s₀→s₂→s₃ ✓
- bab: s₀→s₂→s₀→s₁ ✗
- bba: s₀→s₂→s₃→s₂ ✗
- abab: s₀→s₁→s₀→s₁→s₀ ✗
- aaa: s₀→s₁→s₃→s₂ ✗
- aaaa: s₀→s₁→s₃→s₂→s₀ ✗

Pattern: strings of length 2 where both chars same (aa, bb). Length 4: abba ✓.
The states track something about parity and character pattern...

Actually, with 4 states and Σ={a,b}, this DFA tracks position (mod 4) in some sense. The language appears to be **{w | |w| mod 2 = 0 and position-based condition}**.

After more analysis: this DFA accepts strings where the "a-count minus b-count" is ≡ 2 mod 4... The exact characterization requires more traces. For GATE, you'd compute the specific strings or find the pattern.

---

**P82.** Is {⟨M⟩ | M is a TM and L(M) contains at least one palindrome} decidable?
**Answer:** Rice's theorem — "L(M) contains a palindrome" is a non-trivial language property. **Undecidable** ✓

---

**P83.** Is {⟨M⟩ | M is a DFA and L(M) contains at least one palindrome} decidable?
**Answer:** YES! For DFAs, we can decide everything. Check: does L(M) ∩ {palindromes} ≠ ∅? Palindromes over {a,b} aren't regular, so we can't directly intersect... 

But we CAN check: for each string length n, enumerate all palindromes of length n and check if any is accepted by M. Since we need to find at least one:  
- If L(M) is infinite → it contains strings of all lengths → at least one palindrome of some length  
- If L(M) is finite → check each string in L(M) for palindromicity  

Actually, even simpler: L(M) is regular. Palindromes aren't regular. But we can still decide emptiness:
The set of palindromes up to any length n can be enumerated. If L(M) is non-empty, we can test strings in L(M) one by one for palindromicity. Since L(M) is decidable (DFA), and palindrome testing is decidable, and we can enumerate L(M): **Decidable** ✓

---

**P84.** For the grammar S → aSa | bSb | a | b | ε, find the language.

**Answer:** L = set of all palindromes over {a,b} (including ε).
- S → aSa wraps with 'a' on both sides
- S → bSb wraps with 'b' on both sides
- S → a | b for odd-length palindromes (single center)
- S → ε for even-length palindromes (empty center)

---

**P85.** The language L = {a²ⁿ | n ≥ 0} = {a, aa, aaaa, aaaaaaaa, ...} (powers of 2) is:
(a) Regular  (b) CFL  (c) CSL  (d) Recursive  

**Answer: (c) CSL** and also (d) Recursive — but CSL is the tightest class.
- Not regular: pump aᵖ where p is first power of 2 ≥ p. Pumping by |y| doesn't preserve powers of 2.
- Not CFL: pump a^(2^p). |vy|≤p but pumped length = 2^p + i×|vy|. For i=2: 2^p + |vy|. Need this to be a power of 2. But 2^p < 2^p + |vy| ≤ 2^p + p < 2^(p+1). Not a power of 2!
- IS CSL: LBA can check if input length is a power of 2 (repeatedly halve).
- IS Recursive: trivially.

---

**P86-P90:** Quick True/False

| # | Statement | Answer |
|---|---|---|
| P86 | Σ* is both regular and CFL | TRUE |
| P87 | ∅ is both regular and CFL | TRUE |
| P88 | Every finite language is regular | TRUE |
| P89 | Every co-finite language (finite complement) is regular | TRUE (complement of finite = regular, closed under complement) |
| P90 | {ε} is a DCFL | TRUE (1-state DFA: start=final, all transitions to dead state) |

---

## 📝 Practice Set 9: Rapid-Fire Identification (classify each language)

| # | Language | Regular? | CFL? | Decidable? |
|---|---|---|---|---|
| 1 | {aⁿ \| n ≤ 100} | ✓ | ✓ | ✓ |
| 2 | {aⁿbⁿ \| n ≥ 0} | ✗ | ✓ | ✓ |
| 3 | {aⁿbⁿcⁿ \| n ≥ 0} | ✗ | ✗ | ✓ |
| 4 | {ww \| w ∈ {a,b}*} | ✗ | ✗ | ✓ |
| 5 | {w \| w is a valid C program} | ✗ | ✗ | ✓ |
| 6 | Palindromes over {a,b} | ✗ | ✓ | ✓ |
| 7 | {ww^R \| w ∈ {a,b}*} | ✗ | ✓ | ✓ |
| 8 | HALT | ✗ | ✗ | ✗ (RE) |
| 9 | {aⁿ \| n is prime} | ✗ | ✗ | ✓ |
| 10 | {⟨M⟩ \| L(M) = Σ*} | ✗ | ✗ | ✗ (co-RE) |
| 11 | Σ* \ {aⁿbⁿ} | ✗ | ✓ (DCFL) | ✓ |
| 12 | {a^(2ⁿ) \| n ≥ 0} | ✗ | ✗ | ✓ |
| 13 | ∅ | ✓ | ✓ | ✓ |
| 14 | {w \| w has equal #a and #b} | ✗ | ✓ | ✓ |
| 15 | {⟨M₁,M₂⟩ \| L(M₁)=L(M₂)} | ✗ | ✗ | ✗ (neither) |

> 🎯 **GATE Trick:** For "decidable?" — if you can write a terminating algorithm to check membership, it's decidable. For "CFL?" — if you can design a PDA or CFG, it is. For "regular?" — if a finite-state machine suffices, it is.

---

## 📝 Practice Set 10: Tricky Edge Cases (10 Problems)

**P91.** L₁ is NOT RE. L₂ is finite. Is L₁ ∪ L₂ RE?
**Answer:** Not necessarily. If L₁ is not RE and L₂ is finite (hence decidable, hence RE): L₁ ∪ L₂ might still not be RE. Example: L₁ = complement(HALT) (not RE), L₂ = {ε}. L₁ ∪ {ε} = complement(HALT) ∪ {ε} is still not RE (adding finitely many strings to a non-RE language doesn't make it RE).

**Actually:** if L₂ ⊆ L₁, then L₁ ∪ L₂ = L₁ (not RE). If L₂ ⊄ L₁: we're adding strings to a non-RE set. The result is still not RE in general (would need to semi-decide membership — but can't for L₁ elements outside L₂).

**Answer: NOT RE** ✓

---

**P92.** Can a regular language be inherently ambiguous?
**Answer:** NO! Every regular language has an unambiguous right-linear grammar (and in fact a DFA, which corresponds to an unambiguous grammar). Regular languages are never inherently ambiguous. ✓

---

**P93.** Is {⟨M⟩ | M halts on ⟨M⟩} decidable?
**Answer:** This is the "self-halting" problem: does M halt on its own encoding?
This is a special case of the halting problem → **Undecidable**!

But is it RE? Yes — simulate M on ⟨M⟩. If M halts, accept. **RE but not decidable** ✓

---

**P94.** Can a Turing Machine recognize the empty language ∅?
**Answer:** YES! A TM that immediately rejects (or loops forever) on every input recognizes ∅. In fact, a TM that loops forever on every input has L(M) = ∅ (by the definition of recognition).

Is ∅ decidable? YES — a TM that always outputs REJECT decides ∅. ✓

---

**P95.** If we add a "stay" option (head doesn't move) to a TM, does it gain power?
**Answer:** NO — a TM with L, R, S (stay) is equivalent to a TM with just L, R.
To simulate "stay": move R then L (or L then R). Same computation power. ✓

---

**P96.** A 1-counter automaton (PDA with stack alphabet {Z₀, A} — just counts) can recognize:
(a) All regular languages  (b) {aⁿbⁿ}  (c) Palindromes  (d) Both (a) and (b)

**Answer: (d)** — 1-counter automaton is strictly between DFA and PDA.
- Can count → recognizes {aⁿbⁿ} ✓
- Can simulate any DFA (ignore counter) ✓
- Cannot recognize palindromes (needs to "remember" content, not just count) ✗

---

**P97.** Is the Post Correspondence Problem decidable over a 1-symbol alphabet?
**Answer:** YES! Over {a}: pairs are (aⁱ, aʲ). PCP asks: does sequence i₁,...,iₖ exist with i_{i₁}+...+i_{iₖ} = j_{i₁}+...+j_{iₖ}?

This is a linear equation over integers — decidable! (Actually a variant of subset-sum-like problem, but PCP over unary is decidable.) ✓

---

**P98.** Does every decidable language have a regular superset?
**Answer:** YES — Σ* is regular and contains every language. (Trivially true!) ✓

---

**P99.** Does every decidable language have a CFL superset that is NOT Σ*?
**Answer:** For any decidable L ≠ Σ*: choose some w ∉ L. Then L ∪ {strings of length ≤ 1000} is CFL (actually regular) and ≠ Σ*... hmm, that might equal Σ* if L already contains long strings.

Better: L ∪ {aⁿbⁿ | n ≥ 0} is CFL (CFL ∪ CFL = CFL if L is decidable hence CFL). But this might equal Σ* in some edge case... 

Actually Σ* is also a valid CFL superset. The question becomes trivial: YES, Σ* works (and it IS CFL). ✓

---

**P100.** The diagonal language L_d = {wᵢ | wᵢ ∉ L(Mᵢ)} where Mᵢ is the i-th TM and wᵢ is the i-th string is:
(a) RE  (b) co-RE  (c) Neither  (d) Decidable

**Answer: (c) Neither RE nor co-RE**
- L_d ∉ RE: by diagonalization (if TM M recognizes L_d, then M = Mⱼ for some j, and wⱼ ∈ L_d ↔ wⱼ ∉ L(Mⱼ) = L_d → contradiction)
- L_d ∉ co-RE: complement(L_d) = {wᵢ | wᵢ ∈ L(Mᵢ)}. If this were RE, we could semi-decide it... actually complement(L_d) IS RE (simulate Mᵢ on wᵢ, accept if Mᵢ accepts). So if L_d were co-RE, it would mean complement is RE, which it is. So L_d COULD be co-RE if both L_d and complement(L_d) satisfy the right conditions.

Let me reconsider: complement(L_d) = {wᵢ | wᵢ ∈ L(Mᵢ)} = A_TM on the diagonal, which IS RE but not decidable.
So L_d = complement of (something RE but not decidable) → L_d is co-RE but not decidable? No wait.

If complement(L_d) is RE → L_d is co-RE. But L_d itself is not RE (diagonal argument). So L_d is co-RE but not RE.

**Answer: (b) co-RE** (but not RE, and not decidable) ✓

---

## ✅ Common Mistakes in TOC ⭐

| # | Mistake | Correct |
|---|---------|---------|
| 1 | NFA to DFA always gives 2ⁿ states | Only worst case; many states may be unreachable |
| 2 | Pumping lemma proves regularity | PL can only prove NON-regularity |
| 3 | All CFLs are closed under intersection | CFLs NOT closed under intersection |
| 4 | DPDA = NPDA | DPDA ⊂ NPDA (strictly weaker) |
| 5 | Rice's applies to TM structural properties | Only to LANGUAGE properties, not structural |
| 6 | RE ∩ co-RE = decidable for all | L is decidable iff both L and L' are RE |
| 7 | CFL membership is undecidable | It's decidable (CYK in O(n³)) |
| 8 | CFG equivalence is decidable | Undecidable for CFLs |
| 9 | CFL ∩ Regular might not be CFL | CFL ∩ Regular is ALWAYS CFL |
| 10 | Halting problem is not RE | It IS RE (TM can accept halting instances) |
| 11 | Complement of NFA = swap finals | Must convert to DFA first! |
| 12 | All undecidable problems are the same | They have different degrees of undecidability |
| 13 | if L₁ is regular and L₂ is not, L₁∪L₂ is not regular | Not necessarily! (Could be Σ*) |
| 14 | Grammar ambiguity = language ambiguity | Grammar ambiguity: property of GRAMMAR. Inherent ambiguity: property of LANGUAGE |
| 15 | Rice's applies to CFGs/PDAs | Only to Turing Machines |

---

## 📐 Topic Interconnection Map

```
                    ┌──────────────────┐
                    │  Turing Machines  │
                    │   (Decidability)  │
                    └────────┬─────────┘
                             │
                    ┌────────┴─────────┐
                    │                  │
               ┌────┴────┐      ┌─────┴─────┐
               │   RE    │      │  co-RE    │
               │Languages│      │ Languages │
               └────┬────┘      └───────────┘
                    │
            ┌───────┴───────┐
            │   Recursive   │
            │   (Decidable) │
            └───────┬───────┘
                    │
         ┌──────────┼──────────┐
         │          │          │
    ┌────┴───┐ ┌───┴────┐ ┌──┴──────┐
    │  CSL   │ │  Rice's │ │ Halting │
    │  (LBA) │ │ Theorem │ │ Problem │
    └────┬───┘ └────────┘ └─────────┘
         │
    ┌────┴────┐
    │   CFL   │──── Closure Properties
    │  (PDA)  │──── Pumping Lemma (CFL)
    └────┬────┘──── Decision Properties
         │
    ┌────┴────┐
    │  DCFL   │──── Complement Closure
    │ (DPDA)  │──── LR Parsing Connection
    └────┬────┘
         │
    ┌────┴────┐
    │ Regular │──── DFA/NFA/RE equivalence
    │  (FA)   │──── Pumping Lemma (Regular)
    └─────────┘──── Myhill-Nerode Theorem
```

---

## � Deep Dive: NP-Completeness and Computational Complexity

### P vs NP — The Big Question 🤔

> **Analogy:** P problems are like solving a puzzle yourself. NP problems are like checking someone else's solution. P = NP asks: "Is every puzzle that's easy to check also easy to solve?"

**Class P (Polynomial Time):**
- Problems solvable by a deterministic TM in O(nᵏ) time for some constant k
- Examples: sorting, shortest path (Dijkstra), 2-SAT, 2-coloring, matching

**Class NP (Nondeterministic Polynomial Time):**
- Problems where a "yes" answer can be VERIFIED in polynomial time given a certificate
- Equivalently: solvable by nondeterministic TM in polynomial time
- Examples: SAT, 3-coloring, Hamiltonian cycle, clique, traveling salesman (decision)

**Class co-NP:**
- Problems where a "no" answer can be verified in polynomial time
- Example: "Is this formula unsatisfiable?" (certificate: would be a proof of unsatisfiability)
- Example: "Is this graph NOT 3-colorable?"

### Key Relationships 📊

```
┌──────────────────────────────────────────────┐
│                   PSPACE                      │
│  ┌──────────────────────────────────────┐    │
│  │              NP ∩ co-NP               │    │
│  │  ┌──────────────────────────────┐    │    │
│  │  │             P                 │    │    │
│  │  │  ┌─────────────────────┐    │    │    │
│  │  │  │    NC (parallel)     │    │    │    │
│  │  │  └─────────────────────┘    │    │    │
│  │  └──────────────────────────────┘    │    │
│  └──────┬──────────────────┬────────────┘    │
│         │                  │                  │
│    ┌────┴────┐        ┌───┴────┐             │
│    │   NP    │        │ co-NP  │             │
│    └────┬────┘        └───┬────┘             │
│         │                  │                  │
│    NP-Complete        co-NP-Complete          │
│         │                  │                  │
│         └────────┬─────────┘                  │
│            NP-Hard                            │
│                                               │
└──────────────────────────────────────────────┘
```

### NP-Completeness — The Hardest Problems in NP 🏆

**Definition:** Language L is NP-Complete if:
1. L ∈ NP (can verify solutions in polynomial time)
2. Every language in NP is polynomial-time reducible to L (L is NP-Hard)

> **GATE Trick:** To prove a NEW problem is NP-Complete:
> 1. Show it's in NP (find a polynomial-time verifier)
> 2. Show a KNOWN NP-Complete problem reduces to it in polynomial time

### Cook-Levin Theorem (The First NP-Complete Problem) ⭐

**Theorem:** SAT (Boolean Satisfiability) is NP-Complete.

**Proof Intuition:**
- Any NP problem can be solved by an NDTM in polynomial time
- The computation of an NDTM can be encoded as a Boolean formula
- The formula is satisfiable ⟺ the NDTM has an accepting computation
- The encoding takes polynomial time

### Classic NP-Complete Problems & Reduction Chain 🔗

```
              SAT
             / | \
          3-SAT   CIRCUIT-SAT
         /  |  \
   CLIQUE  VERTEX-COVER  3-COLORING
     |         |              |
  INDEPENDENT  SET-COVER   k-COLORING
  SET               |
     |         HITTING-SET
  SUBGRAPH
  ISOMORPHISM
```

**Key Reduction Chain for GATE:**
```
SAT ≤_p 3-SAT ≤_p CLIQUE ≤_p VERTEX-COVER ≤_p INDEPENDENT-SET
SAT ≤_p 3-SAT ≤_p 3-COLORING
3-SAT ≤_p HAMILTONIAN-CYCLE ≤_p TSP
3-SAT ≤_p SUBSET-SUM ≤_p PARTITION ≤_p BIN-PACKING
```

### Comprehensive NP-Complete Problem Table 📋

| Problem | Input | Question | Certificate | Verification |
|---|---|---|---|---|
| SAT | Boolean formula φ | Is φ satisfiable? | Truth assignment | Evaluate φ: O(n) |
| 3-SAT | CNF with ≤3 literals/clause | Is φ satisfiable? | Truth assignment | Check each clause: O(m) |
| CLIQUE | Graph G, integer k | Has k-clique? | Set of k vertices | Check all edges exist: O(k²) |
| VERTEX COVER | Graph G, integer k | Has k-vertex cover? | Set of k vertices | Check all edges covered: O(m) |
| INDEPENDENT SET | Graph G, integer k | Has k-independent set? | Set of k vertices | Check no edges between them: O(k²) |
| 3-COLORING | Graph G | Is G 3-colorable? | Color assignment | Check all edges: O(m) |
| HAM-CYCLE | Graph G | Has Hamiltonian cycle? | Ordering of vertices | Check it's a valid cycle: O(n) |
| TSP | Weighted graph, bound B | Tour of cost ≤ B? | Tour | Sum weights, compare: O(n) |
| SUBSET SUM | Set S, target t | Subset summing to t? | Subset | Sum and compare: O(n) |
| PARTITION | Multiset S | Split into equal halves? | Partition | Sum both halves: O(n) |
| KNAPSACK (0/1) | Items, capacity W | Value ≥ V achievable? | Item selection | Check weight and value: O(n) |
| SET COVER | Universe U, subsets Sᵢ, k | Cover with ≤k subsets? | k subsets | Check union = U: O(nm) |
| GRAPH COLORING | Graph G, integer k | Is G k-colorable? | Color assignment | Check all edges: O(m) |

### Worked Example: Proving INDEPENDENT-SET is NP-Complete ⭐

**Step 1: Show INDEPENDENT-SET ∈ NP**
- Certificate: A set S of k vertices
- Verification: Check |S| = k AND no two vertices in S are adjacent
- Time: O(k²) comparisons → polynomial ✓

**Step 2: Show CLIQUE ≤_p INDEPENDENT-SET**
- Given: instance (G, k) of CLIQUE
- Construct: complement graph Ḡ (edge ↔ no edge)
- Claim: G has k-clique ⟺ Ḡ has k-independent-set
- Proof: If S is a k-clique in G → all pairs in S have edges in G → no pair in S has edge in Ḡ → S is independent in Ḡ ✓
- Reduction time: O(n²) to compute complement → polynomial ✓

**Therefore: INDEPENDENT-SET is NP-Complete** ✓

### Worked Example: 3-SAT to CLIQUE Reduction

**Input 3-SAT:** φ = (x₁ ∨ x₂ ∨ ¬x₃) ∧ (¬x₁ ∨ x₃ ∨ x₄) ∧ (x₂ ∨ ¬x₃ ∨ ¬x₄)

**Construction of Graph G:**
1. Create one vertex per literal per clause (3 vertices per clause = 9 vertices)
2. Connect two vertices if they are NOT in the same clause AND NOT complementary
3. k = number of clauses = 3

**Vertices:**
```
Clause 1: v₁(x₁), v₂(x₂), v₃(¬x₃)
Clause 2: v₄(¬x₁), v₅(x₃), v₆(x₄)
Clause 3: v₇(x₂), v₈(¬x₃), v₉(¬x₄)
```

**Edges (connect if different clauses AND not complementary):**
- v₁(x₁) — v₅(x₃) ✓ (diff clause, not complement)
- v₁(x₁) — v₆(x₄) ✓
- v₁(x₁) — v₇(x₂) ✓
- v₁(x₁) — v₈(¬x₃) ✓
- v₁(x₁) — v₉(¬x₄) ✓
- v₁(x₁) — v₄(¬x₁) ✗ (complementary!)
- ... (continue for all pairs)

**Key Idea:** k-clique in G → one TRUE literal per clause → φ satisfiable ✓

### Problems in P (Important for GATE) 📗

| Problem | Best Known Algorithm | Time Complexity |
|---|---|---|
| Sorting | Merge Sort, etc. | O(n log n) |
| Shortest Path | Dijkstra/Bellman-Ford | O(V² or VE) |
| Maximum Matching | Edmonds' | O(V³) |
| 2-SAT | Implication Graph (SCC) | O(n + m) |
| 2-Coloring | BFS/DFS | O(V + E) |
| MST | Kruskal/Prim | O(E log V) |
| Primality Testing | AKS | O(log^6 n) |
| Linear Programming | Ellipsoid/Interior Point | Polynomial |
| Maximum Flow | Ford-Fulkerson variants | O(VE²) |
| Euler Path/Circuit | Hierholzer | O(E) |

### Problems NOT Known to be in P or NP-Complete 🔍

| Problem | Known Status |
|---|---|
| Graph Isomorphism | NP, not known NP-Complete (quasi-polynomial) |
| Factoring | NP ∩ co-NP, not known in P (quantum: polynomial) |
| Discrete Logarithm | NP ∩ co-NP, not known in P |

### PSPACE and Beyond 🌌

**PSPACE:** Problems solvable with polynomial space (but possibly exponential time)
- PSPACE-Complete: QBF (Quantified Boolean Formula), TQBF
- **Savitch's Theorem:** NSPACE(f(n)) ⊆ DSPACE(f(n)²) → NPSPACE = PSPACE

**EXPTIME:** Problems solvable in 2^(nᵏ) time
- EXPTIME-Complete: Chess (on n×n board), Go

**Full Hierarchy:**
```
P ⊆ NP ⊆ PSPACE ⊆ EXPTIME ⊆ EXPSPACE ⊆ ...
P ⊆ co-NP ⊆ PSPACE
```

**Known strict containments (by hierarchy theorems):**
- P ⊊ EXPTIME (time hierarchy theorem)
- NP ⊊ EXPTIME (if P ≠ NP) — but NP ⊊ EXPTIME is provable regardless!
- PSPACE ⊊ EXPSPACE (space hierarchy theorem)

### Space Complexity for GATE ⭐

**L (Logarithmic Space):** O(log n) space beyond input
- Examples: graph reachability in directed graphs → NL-Complete
- Examples: palindrome checking with index → L

**NL (Nondeterministic Log Space):**
- NL-Complete: Graph Reachability (PATH)
- **NL = co-NL** (Immerman–Szelepcsényi theorem)

**Key Facts:**
```
L ⊆ NL = co-NL ⊆ P ⊆ NP ∩ co-NP ⊆ NP ∪ co-NP ⊆ PSPACE = NPSPACE ⊆ EXPTIME
```

> 🎯 **GATE Trick:** For complexity class questions, remember:
> - All these classes collapse to the same thing IF P = NP
> - Use the standard chain: L ⊆ NL ⊆ P ⊆ NP ⊆ PSPACE ⊆ EXPTIME
> - At least one strict containment must exist in P ⊆ NP ⊆ PSPACE ⊆ EXPTIME

---

## 🔬 Deep Dive: Advanced TM Topics

### Multi-Tape TM Simulation — Detailed

**Theorem:** Every k-tape TM can be simulated by a single-tape TM with at most quadratic time overhead.

**Simulation Method:**
```
k-tape TM tape contents (k=3):

Tape 1: ...□ a b c □ ...     (head at 'b')
Tape 2: ...□ 1 0 1 1 □ ...   (head at '1')
Tape 3: ...□ x y □ ...       (head at 'y')

Single-tape encoding:
# a b̂ c # 1̂ 0 1 1 # x ŷ #
       ↑        ↑          ↑
   (dotted = head position)
```

**Steps per simulated step:**
1. Scan right to read all k head symbols: O(n) where n = total tape content
2. Scan left back to start: O(n)
3. Make modifications based on transition function: O(n) per head move potentially
4. Shifting for insertion: possible O(n) shift

**Total:** O(n) per simulated step. If original runs for T(n) steps with tape usage S(n):
- Total tape content: ≤ k × S(n)
- Total simulation time: O(T(n) × k × S(n)) = O(T(n)²) in worst case (since S(n) ≤ T(n))

### Linear Bounded Automaton — Detailed Treatment

**Definition:** LBA = TM that uses only the tape cells containing the input (plus possibly constant extra cells on each end).

**Formal:** LBA is a TM with tape alphabet Γ containing left endmarker ⊢ and right endmarker ⊣. The head never moves past these markers.

**Power:** Exactly recognizes Type 1 (Context-Sensitive) languages!

**Key Facts for GATE:**

| Question about LBA | Answer |
|---|---|
| Emptiness: Is L(M) = ∅? | **Undecidable** ⭐ |
| Membership: Is w ∈ L(M)? | **Decidable** (LBA has finitely many configs on input w) |
| Equivalence: L(M₁) = L(M₂)? | **Undecidable** |
| Containment: L(M₁) ⊆ L(M₂)? | **Undecidable** |
| Finiteness: Is L(M) finite? | **Undecidable** |
| Universality: L(M) = Σ*? | **Undecidable** |

**LBA Membership Decidability Proof:**
- On input w of length n, LBA has at most |Q| × n × |Γ|ⁿ configurations
- This is finite! So simulate LBA for this many steps.
- If it accepts → accept. If it hasn't accepted → reject (it must be in a loop).

**LBA Configuration Count:**
```
Input length n
States: |Q|
Head positions: n
Tape contents: |Γ|ⁿ
Total configs: |Q| × n × |Γ|ⁿ
```

This is exponential in n but finite → decidable!

### Nondeterministic TM vs Deterministic TM

**Simulation:** DTM can simulate NDTM using BFS of configuration tree.

```
                    (start config)
                    /     |     \
            config₁  config₂  config₃     ← after 1 step
            / \        |      / | \
          c4   c5     c6    c7  c8 c9      ← after 2 steps
```

**BFS approach:**
1. Enumerate all possible computation paths level by level
2. If any path reaches accept → ACCEPT
3. If all paths halt without accepting → REJECT
4. Might not terminate if NDTM has infinite paths!

**Time Overhead:** If NDTM runs in T(n) steps with branching factor b:
- DTM explores up to b^T(n) paths
- Each path takes O(T(n)) steps to simulate
- Total: O(b^T(n) × T(n)) — exponential blowup!

**Key Theorem:** NDTM running in time T(n) → DTM running in time 2^O(T(n))
This is why NP is contained in EXPTIME!

### Oracle Turing Machines 🔮

**Definition:** TM with access to an "oracle" for some language A. The TM can query "is x ∈ A?" and get an instant yes/no answer.

**Notation:** M^A denotes machine M with oracle A.

**Relativization:**
- There exists oracle A such that P^A = NP^A
- There exists oracle B such that P^B ≠ NP^B

**Implication:** Any proof that P ≠ NP (or P = NP) cannot relativize — must use non-relativizing techniques.

### Enumeration and Dovetailing 📋

**Dovetailing Technique:** Simulate multiple infinite computations in parallel.

**Method (to enumerate all strings in an RE language):**
```
Step 1: Run M on w₁ for 1 step
Step 2: Run M on w₁ for 2 steps, w₂ for 1 step
Step 3: Run M on w₁ for 3 steps, w₂ for 2 steps, w₃ for 1 step
...
Step n: Run M on wᵢ for (n-i+1) steps, i=1..n
```

**Visual:**
```
        w₁   w₂   w₃   w₄   w₅
Step 1:  1
Step 2:  2    1
Step 3:  3    2    1
Step 4:  4    3    2    1
Step 5:  5    4    3    2    1
```

If M accepts wᵢ in t steps, it will be found at step i+t-1. Every accepted string is eventually found!

**Applications:**
- Proving RE ∩ co-RE = Decidable
- Enumerating elements of RE language
- Proving equivalence of TM and enumerator

---

## 🔬 Deep Dive: Advanced Regular Language Topics

### State Complexity of Operations

The **state complexity** of an operation is the number of states needed in the worst case.

| Operation | Input States | Output States (worst case) |
|---|---|---|
| Union (L₁ ∪ L₂) | m, n | mn |
| Intersection (L₁ ∩ L₂) | m, n | mn |
| Complement (L̄) | n | n |
| Concatenation (L₁L₂) | m, n | m × 2ⁿ - 2^(n-1) |
| Kleene star (L*) | n | 2^(n-1) + 2^(n-2) |
| Reversal (L^R) | n | 2ⁿ |
| Difference (L₁ - L₂) | m, n | mn |
| Symmetric Diff (L₁ ⊕ L₂) | m, n | mn |

> 🎯 **GATE Trick:** Reversal causes the WORST blowup (exponential). That's because NFA reversal ↔ DFA reversal, and DFA→NFA is free but NFA→DFA costs 2ⁿ in worst case.

### Worked Example: State Complexity of Reversal

**L = {w ∈ {0,1}* | w starts with 110}** — minimum DFA has 4 states (tracks prefix progress).

**L^R = {w ∈ {0,1}* | w ends with 011}** — minimum DFA has 4 states.

Here reversal preserved the state count (both need 4 states). The exponential blowup is worst-case, not always achieved.

**Worst-case example:** L_n = {strings of length ≥ n whose n-th-from-last symbol is 1}
- Min DFA for L_n: 2ⁿ states (need to remember last n bits)
- L_n^R = {strings of length ≥ n whose n-th symbol is 1}: only n+1 states!
- So reversal can DECREASE state count too!

### Two-Way Finite Automata (2DFA) 🔄

**Definition:** Like DFA, but head can move LEFT or RIGHT (or stay).

**Key Theorem:** 2DFA ≡ DFA (same power!) — proves that two-way access doesn't help for regular languages.

**But:** The conversion from 2DFA to DFA may cause exponential state blowup.

**For GATE:** Just know that 2DFA = 1DFA = NFA in power. Two-way access is syntactic sugar.

### Weighted/Probabilistic Finite Automata

**Probabilistic FA (PFA):** Instead of deterministic transitions, each state-input pair gives a probability distribution over next states.

- cut-point language: L = {w | Pr[accept w] > λ} for some cutoff λ
- Some PFA languages are NOT regular (with isolated cutpoints)!
- But PFA with isolated cutpoints → equivalent to regular languages

For GATE: Usually not tested, but know the concept exists.

### Büchi Automata and ω-Languages

**Büchi Automaton:** Accepts infinite strings (ω-words). Accepts if accepting state visited INFINITELY often.

- Deterministic Büchi < Nondeterministic Büchi (strictly less powerful!)
- This is unlike regular FA where DFA = NFA!
- Used in model checking (verification of systems)

For GATE: Usually not tested, included for completeness.

---

## 🔬 Deep Dive: Context-Free Language Advanced Topics

### Parikh's Theorem 📐

**Theorem:** For every CFL L over Σ = {a₁, ..., aₖ}, the Parikh image (set of letter-count vectors) is a semilinear set — and equals the Parikh image of some regular language!

**Meaning:** If L is CFL, then the "counting behavior" of L is as simple as a regular language.

**Consequence:** Over a unary alphabet (|Σ|=1), every CFL IS regular!

**GATE Application:** If someone asks "Is {aⁿ | some condition on n} CFL but not regular?" — the answer is ALWAYS "impossible over unary alphabet."

**Example:**
- {aⁿbⁿ}: Parikh image = {(n,n) | n ≥ 0} which is semilinear
- {aⁿ | n is a power of 2}: Over unary, if this were CFL, it would be regular (Parikh). But it's not regular → it's NOT CFL (over unary, not CFL).

Wait: {a^(2ⁿ)} is NOT unary CFL — confirmed by Parikh's theorem!

### Ogden's Lemma — Detailed Application

**Ogden's Lemma (Generalization of CFL Pumping Lemma):**

For CFL L, ∃p such that: for any z ∈ L with |z|≥p, if we "mark" at least p positions in z, then z = uvwxy where:
1. v and x together contain at least one marked position
2. vwx contains at most p marked positions
3. uvⁱwxⁱy ∈ L for all i ≥ 0

**Power:** Ogden's lemma lets us CONTROL which part of the string gets pumped by choosing which positions to mark. The standard pumping lemma is a special case where ALL positions are marked.

**Application: Proving L = {a^i b^j c^k | i ≠ j ≠ k ≠ i} is not CFL**

Standard pumping lemma might fail (hard to handle three-way inequality). With Ogden's lemma:
1. Choose z = aᵖbᵖ⁺ᵖ!cᵖ⁺²ᵖ! (carefully chosen)
2. Mark only the a's
3. Then v and x must contain some a's (marked positions)
4. Pumping increases a's without properly adjusting b's and c's → contradiction

(This is technically involved; for GATE, know that Ogden's lemma is more powerful than standard pumping lemma.)

### Intersection of CFL with Regular: Detailed Construction

**Theorem:** If L₁ is CFL and L₂ is regular, then L₁ ∩ L₂ is CFL.

**Construction (Product PDA):**
- Take PDA P for L₁ with states Q_P
- Take DFA D for L₂ with states Q_D
- Build new PDA with states Q_P × Q_D
- Transitions: simultaneously follow PDA transition AND DFA transition on input
- Stack operations: same as original PDA
- Accept: both PDA and DFA components accept

**Worked Example:**
- L₁ = {aⁿbⁿ | n ≥ 0} (CFL)
- L₂ = {w | |w| ≡ 0 mod 3} (Regular — DFA with 3 states)
- L₁ ∩ L₂ = {aⁿbⁿ | 2n ≡ 0 mod 3} = {aⁿbⁿ | n ≡ 0 mod 3} = {a³ᵏb³ᵏ | k ≥ 0}
- This IS CFL: S → aaaSbbb | ε

**Construction of Product PDA:**
- PDA states: {(q₀,0), (q₀,1), (q₀,2), (q₁,0), (q₁,1), (q₁,2), (qf,0), (qf,1), (qf,2)}
- Start: (q₀, 0)
- Accept: (qf, 0) — both components accept
- On input a: PDA pushes A, DFA advances modular counter
- On input b: PDA pops A, DFA advances modular counter

### Deterministic Context-Free Languages (DCFL) — Deep Treatment

**DCFL Definition:** Languages recognized by DPDA (deterministic PDA).

**DCFL Properties Table:**

| Property | Closed? | Why? |
|---|---|---|
| Complement | ✓ | Modify DPDA acceptance ⭐ |
| Intersection with Regular | ✓ | Product construction |
| Union | ✗ | {aⁿbⁿcᵐ} ∪ {aⁿbᵐcᵐ} is CFL not DCFL |
| Intersection | ✗ | Same counterexample as CFL |
| Concatenation | ✗ | |
| Kleene star | ✗ | |
| Reversal | ✗ | Even-length palindromes: DCFL^R might not be DCFL |
| Homomorphism | ✗ | |
| Inverse homomorphism | ✓ | |
| Prefix | ✓ | Modify DPDA |
| Suffix | ✗ | |

**Important DCFL Languages:**
- {aⁿbⁿ | n ≥ 0} — DCFL ✓
- {aⁿbⁿ | n ≥ 1} — DCFL ✓
- {w | #a(w) = #b(w)} — DCFL ✓ (prefix-based checking)
- Balanced parentheses — DCFL ✓
- Every regular language — DCFL ✓
- LR(k) languages for any k — DCFL ✓

**Important NON-DCFL Languages:**
- {wwᴿ | w ∈ {a,b}*} — even palindromes: needs to guess middle → NOT DCFL
- {aⁿbⁿ | n ≥ 0} ∪ {aⁿb²ⁿ | n ≥ 0} — needs nondeterministic choice
- {aⁱbʲcᵏ | i = j or j = k} — needs nondeterministic choice

**The "DCFL = LR(1)" Connection:**
- Every DCFL has an LR(1) grammar
- Every LR(1) grammar generates a DCFL
- DCFL ≡ LR(1) languages ≡ languages recognized by DPDA

> 🎯 **GATE Trick:** "Can you parse it with a shift-reduce parser (LR)?" = "Is it DCFL?"

### Unambiguous CFL (UCFL)

**Definition:** A CFL is unambiguous if it has at least one unambiguous grammar.

| Class | Relation | Example |
|---|---|---|
| Regular | ⊂ DCFL | a*b* |
| DCFL | ⊂ Unambiguous CFL | {aⁿbⁿ} |
| Unambiguous CFL | ⊂ CFL | {wwᴿ} is unambiguous CFL (S→aSa\|bSb\|ε) |
| CFL | ⊃ Inherently Ambiguous CFL | {aⁿbⁿcᵐ} ∪ {aⁿbᵐcᵐ} |

**Hierarchy:**
```
Regular ⊊ DCFL ⊊ Unambiguous CFL ⊊ CFL
```

### Greibach Normal Form (GNF) — Detailed

**GNF:** Every production has form A → aα where a ∈ Σ and α ∈ V* (starts with terminal, rest are variables).

**Conversion Steps:**
1. First convert to CNF
2. Order variables A₁, A₂, ..., Aₖ
3. Ensure Aᵢ → Aⱼγ implies j > i (no left recursion or backward references)
4. Eliminate left recursion
5. Substitute back

**GNF Properties:**
- Every non-empty CFL (not containing ε) has a GNF grammar
- Useful for PDA construction: each production becomes exactly one PDA transition
- GNF grammar → |w| derivation steps for string w of length |w| (exactly one terminal per step)

**Worked Example: Converting to GNF**

Start with CNF: S → AB | a; A → BS | b; B → SA | a

Step 1: Order variables: A₁ = S, A₂ = A, A₃ = B

Step 2: Ensure productions go "forward" (Aᵢ → Aⱼ... with j > i):
- S → AB | a ✓ (S→AB: A is A₂, forward)
- A → BS | b → A → SAB | b (substituting B → SA): hmm, A → BS is backward (B is A₃, but S is A₁)

Actually, we need: for A₂ = A: A → BS means first symbol is B = A₃ > A₂ ✓. and S = A₁ < A₂ ✗.

Actually the requirement is for Aᵢ → Aⱼγ, j > i (leftmost symbol). 
- S (A₁) → A (A₂)B: A₂ > A₁ ✓
- A (A₂) → B (A₃)S: A₃ > A₂ ✓  
- B (A₃) → S (A₁)A: A₁ < A₃ ✗ ← need to fix!

Substitute: B → SA → (AB|a)A → ABA | aA
Now B → ABA | aA
- B → A(A₂)BA: A₂ < A₃ ✗ ← still backward!

Substitute A: B → (BS|b)BA | aA → BSBA | bBA | aA
- B → BSBA is left-recursive! Eliminate:
  - B → bBA B' | aA B' | bBA | aA
  - B' → SBA B' | SBA | ε → B' → SBAB' | SBA

Hmm, this gets complex. The key point for GATE: know that GNF exists and its properties, not necessarily the full conversion algorithm.

---

## 🔬 Deep Dive: Undecidability Zoo — Complete Reference

### Comprehensive Undecidable Problems Table

| Problem | Formal | RE? | co-RE? | Rice's? |
|---|---|---|---|---|
| Halting | {⟨M,w⟩ \| M halts on w} | ✓ | ✗ | N/A (not just L(M)) |
| Acceptance | {⟨M,w⟩ \| M accepts w} | ✓ | ✗ | N/A |
| Emptiness | {⟨M⟩ \| L(M) = ∅} | ✗ | ✓ | ✓ (trivial property) |
| Non-emptiness | {⟨M⟩ \| L(M) ≠ ∅} | ✓ | ✗ | ✓ |
| Finiteness | {⟨M⟩ \| L(M) is finite} | ✗ | ✓ | ✓ |
| Infiniteness | {⟨M⟩ \| L(M) is infinite} | ✓ | ✗ | ✓ |
| Universality | {⟨M⟩ \| L(M) = Σ*} | ✗ | ✓ | ✓ |
| Non-universality | {⟨M⟩ \| L(M) ≠ Σ*} | ✓ | ✗ | ✓ |
| Regularity | {⟨M⟩ \| L(M) is regular} | ✗ | ✗ | ✓ |
| Non-regularity | {⟨M⟩ \| L(M) is NOT regular} | ✗ | ✗ | ✓ |
| CFL-ness | {⟨M⟩ \| L(M) is CFL} | ✗ | ✗ | ✓ |
| Equality | {⟨M₁,M₂⟩ \| L(M₁) = L(M₂)} | ✗ | ✓ | N/A |
| Subset | {⟨M₁,M₂⟩ \| L(M₁) ⊆ L(M₂)} | ✗ | ✓ | N/A |
| Disjointness | {⟨M₁,M₂⟩ \| L(M₁)∩L(M₂)=∅} | ✗ | ✓ | N/A |
| Totality | {⟨M⟩ \| M halts on all inputs} | ✗ | ✓ | N/A |
| Equivalence to TM₀ | {⟨M⟩ \| L(M) = L(M₀)} | depends on L(M₀) | depends | ✓ |

### RE vs co-RE Pattern Recognition 🎯

**Quick Rule for "about L(M)" problems:**

| If the property P is... | {⟨M⟩ \| P(L(M))} is... | Complement is... |
|---|---|---|
| "Positive" (some string in/out of L(M)) | RE | co-RE |
| "Negative/universal" (all strings must...) | co-RE | RE |

**Intuition:**
- "L(M) ≠ ∅" — positive, existential → **RE** (find one accepted string by dovetailing)
- "L(M) = ∅" — negative, universal → **co-RE** (you'd need to verify no string is accepted)
- "L(M) is regular" — can't even semi-decide → **neither RE nor co-RE**

### Detailed Reduction Examples

**Example 1: Proving HALT ≤_m A_TM**

A_TM = {⟨M,w⟩ | M accepts w}
HALT = {⟨M,w⟩ | M halts on w}

**Reduction function f:**
On input ⟨M,w⟩, construct M':
```
M' on input x:
  1. Run M on w
  2. If M halts and accepts → ACCEPT
  3. If M halts and rejects → loop forever
```

Then: M halts on w → M' accepts w (if M accepts) or M' loops (if M rejects)

Wait, this doesn't quite work. Let me redo:

**Better reduction:** Actually, A_TM ≤_m HALT is more standard:
Build M': "On input x: run M on w. If M accepts, halt. If M rejects, loop."
Then M accepts w → M' halts on x. M doesn't accept w → M' loops on x.
So ⟨M,w⟩ ∈ A_TM ↔ ⟨M',x⟩ ∈ HALT. ✓

**Example 2: Proving E_TM (emptiness) is undecidable using Rice's Theorem**

E_TM = {⟨M⟩ | L(M) = ∅}

Check Rice's theorem conditions:
1. Is this a property of L(M)? YES — it's about the language being empty.
2. Is it non-trivial? YES — some TMs have empty language (always loops), some don't (e.g., TM accepting Σ*).

Therefore by Rice's theorem: **E_TM is undecidable** ✓

For RE/co-RE classification:
- E_TM asks "for ALL strings w, M doesn't accept w" — universal quantifier → co-RE
- Complement (NON-EMPTINESS) asks "EXISTS string w that M accepts" → existential → RE

**Example 3: Proving REGULAR_TM is neither RE nor co-RE**

REGULAR_TM = {⟨M⟩ | L(M) is regular}

Rice's theorem tells us it's undecidable. But is it RE? co-RE?

**Neither!** Proof sketch:
- If REGULAR_TM were RE, we could enumerate all TMs with regular languages
- If complement were RE, we could enumerate all TMs with non-regular languages
- Both lead to contradictions via reduction from known "neither" problems

(In fact: the property "regular" separates both trivial and non-trivial languages, creating problems for both RE and co-RE semi-decision procedures.)

---

## 🔬 Deep Dive: Formal Language Hierarchy — Complete Picture

### Languages Between Regular and CFL

```
Regular ⊊ LIN ⊊ DCFL ⊊ CFL

Wait, actually:
Regular ⊊ DPDA ⊊ CFL
Regular ⊊ Linear CFL ⊊ CFL
DPDA ∩ Linear CFL ⊋ Regular (but DPDA ⊄ Linear and Linear ⊄ DPDA)
```

**Linear Grammars:** Each production has at most one variable on RHS.
- A → wBx | w (where w, x ∈ Σ*)

**Linear Languages ⊊ CFL:**
- All regular languages are linear
- {aⁿbⁿ} is linear (S → aSb | ε)
- {wwᴿ} is linear (S → aSa | bSb | ε)
- {aⁿbⁿcᵐdᵐ} is NOT linear (needs two independent counts simultaneously)

### Languages Between CFL and Decidable

```
CFL ⊊ DCSL ⊊ CSL ⊊ Recursive

Where:
CSL = Context-Sensitive Languages = LBA languages
DCSL = Deterministic CSL = DLBA languages
```

**Examples of CSL but not CFL:**
- {aⁿbⁿcⁿ | n ≥ 0} — the classic
- {ww | w ∈ {a,b}*} — string duplication
- {a^(n²) | n ≥ 0} — perfect squares
- {a^(2ⁿ) | n ≥ 0} — powers of 2

**Examples of Recursive but not CSL:**
- This is hard to construct! Most "natural" decidable languages are CSL.
- Any language decidable in more than linear space but decidable overall.

### The Arithmetic Hierarchy (Beyond RE) 🏔️

For advanced understanding (rarely tested in GATE but good to know):

```
Σ₀ = Π₀ = Δ₀ = Decidable
Σ₁ = RE
Π₁ = co-RE
Δ₁ = Decidable = Σ₁ ∩ Π₁

Σ₂ = languages of form {x | ∃y ∀z R(x,y,z)} with R decidable
Π₂ = complement of Σ₂
Δ₂ = Σ₂ ∩ Π₂

...and so on...
```

**Examples:**
- Σ₁ (RE): A_TM, HALT, NON-EMPTY_TM
- Π₁ (co-RE): EMPTY_TM, TOTAL_TM, complement of HALT
- Σ₂: TOT = {⟨M⟩ | M halts on ALL inputs} — Wait, TOT is Π₂!
  - Actually: TOT = {⟨M⟩ | ∀w: M halts on w} → Π₂
  - complement(TOT) = {⟨M⟩ | ∃w: M doesn't halt on w} → Σ₂

For GATE: Just know that Decidable ⊊ RE ⊊ "all languages" and that there exist languages beyond RE that form an infinite hierarchy.

---

## 📝 Practice Set 11: NP-Completeness and Complexity (15 Problems)

**P101.** Is the following in P? "Given graph G and integer k, does G have a vertex cover of size EXACTLY k?"
**Answer:** This is NP-complete (even deciding ≤ k is NP-complete, and "exactly k" can be reduced from "≤ k" by checking ≤ k and NOT ≤ k-1). **NP-Complete** ✓

---

**P102.** An NP-Complete problem A is reduced to problem B in polynomial time. Which is TRUE?
(a) B ∈ NP  (b) B is NP-Hard  (c) B is NP-Complete  (d) B ∈ P

**Answer: (b) B is NP-Hard** — we only know every NP problem reduces to B (through A). B might not be in NP at all, so we can't say NP-Complete. ✓

---

**P103.** If P = NP, which of the following is TRUE?
(a) Every NP-Complete problem is in P
(b) NP-Complete = P — {∅, Σ*}
(c) SAT can be solved in polynomial time
(d) All of the above

**Answer: (d) All of the above**
- (a) ✓ by definition (if P=NP, NP-Complete ⊆ NP = P)
- (b) ✓ (in P=NP world, NP-Complete = all non-trivial problems in P)
- (c) ✓ (SAT is NP-Complete, hence in P)

---

**P104.** 2-SAT is:
(a) NP-Complete  (b) In P  (c) Undecidable  (d) In NP but not P

**Answer: (b) In P** — 2-SAT is solved by implication graph + SCC in O(n+m) time. ✓

---

**P105.** The complement of an NP-Complete problem is:
(a) Always NP-Complete  (b) Always in co-NP  (c) Sometimes not in NP  (d) Both (b) and (c)

**Answer: (d)** — Complement of NP-Complete is co-NP-Complete. It's in co-NP by definition. It's NOT known to be in NP (if it were, NP = co-NP). ✓

---

**P106.** If problem A is in NP ∩ co-NP, can A be NP-Complete?
**Answer:** If A is NP-Complete AND in co-NP, then NP ⊆ co-NP (since every NP problem reduces to A, which is in co-NP). This would imply NP = co-NP. So if NP ≠ co-NP (believed), then **no NP-Complete problem is in co-NP**, hence in NP ∩ co-NP. ✓

---

**P107.** Which is the correct inclusion: 
(a) P ⊆ NP ⊆ PSPACE ⊆ EXPTIME
(b) P ⊆ PSPACE ⊆ NP ⊆ EXPTIME
(c) NP ⊆ P ⊆ PSPACE ⊆ EXPTIME
(d) P ⊆ NP ⊆ EXPTIME ⊆ PSPACE

**Answer: (a)** — Standard chain: P ⊆ NP ⊆ PSPACE ⊆ EXPTIME ✓

---

**P108.** If L₁ ≤_p L₂ and L₂ ∈ NP, can we conclude L₁ ∈ NP?
**Answer:** YES! If f is a polynomial-time reduction and L₂ has a polynomial-time verifier V₂:
- Verifier for L₁: On input (x, c), compute f(x), run V₂(f(x), c).
- f is polynomial, V₂ is polynomial → composition is polynomial.
- So L₁ ∈ NP ✓

---

**P109.** The traveling salesman OPTIMIZATION problem (find minimum cost tour) is:
(a) In NP  (b) NP-Hard  (c) NP-Complete  (d) In P

**Answer: (b) NP-Hard** but NOT NP-Complete (not even in NP as a decision problem!). The optimization version asks for the actual minimum, which requires computing an answer, not just verifying. It's NP-Hard because solving it would solve the decision version. ✓

Wait: actually the OPTIMIZATION problem is in the class FP^NP (function problems). It's NP-Hard under Turing reductions. The DECISION version ("is there a tour of cost ≤ k?") is NP-Complete.

---

**P110.** Which problem is in P?
(a) 3-SAT  (b) 2-Coloring  (c) Hamiltonian Path  (d) 3-Coloring

**Answer: (b) 2-Coloring** — equivalent to checking bipartiteness, done by BFS in O(V+E). ✓

---

**P111.** If CLIQUE is solvable in O(n^5) time, then:
(a) P = NP  (b) NP ⊆ P  (c) All NP problems solvable in polynomial time  (d) All of the above

**Answer: (d) All of the above** — CLIQUE is NP-Complete. If it's in P, then every NP problem reduces to it in polynomial time → every NP problem is in P → P = NP. ✓

---

**P112.** Savitch's theorem states:
(a) NTIME(f(n)) ⊆ DTIME(2^f(n))
(b) NSPACE(f(n)) ⊆ DSPACE(f(n)²)  
(c) NP = P
(d) NSPACE(f(n)) ⊆ DTIME(f(n)²)

**Answer: (b)** — Savitch's theorem: nondeterministic space f(n) can be simulated deterministically in f(n)² space. ✓

**Corollary:** NPSPACE = PSPACE (since f(n)² is still polynomial if f(n) is polynomial).

---

**P113.** The language {⟨G,s,t⟩ | there is a path from s to t in directed graph G} is:
(a) NL-Complete  (b) P-Complete  (c) NP-Complete  (d) L

**Answer: (a) NL-Complete** — Graph reachability (PATH) is the canonical NL-Complete problem. ✓

---

**P114.** If A is NP-Complete and A ∈ co-NP, then:
(a) P = NP  (b) NP = co-NP  (c) NP = PSPACE  (d) Nothing can be concluded

**Answer: (b) NP = co-NP** — If A ∈ co-NP and A is NP-Hard: for any L ∈ NP, L ≤_p A. Since A ∈ co-NP and reductions preserve co-NP membership, L ∈ co-NP. So NP ⊆ co-NP. By symmetry, co-NP ⊆ NP. Therefore NP = co-NP. ✓

---

**P115.** Sort by complexity class (easiest to hardest):
A: 2-SAT   B: 3-SAT   C: QBF   D: Halting Problem

**Answer:** A (P) < B (NP-Complete) < C (PSPACE-Complete) < D (Undecidable) ✓

---

## 📝 Practice Set 12: Comprehensive Mixed Final (20 Problems)

**P116.** A DFA for "binary number divisible by 5" has how many states?
**Answer: 5 states** (states track remainder mod 5: {0, 1, 2, 3, 4}, MSB first) ✓

---

**P117.** Given NFA with states {A,B,C}, start A, final {C}, transitions: δ(A,0)={A,B}, δ(A,1)={A}, δ(B,0)={C}, δ(B,1)={C}, δ(C,0)=∅, δ(C,1)=∅. 
The equivalent DFA has how many reachable states?

**Solution:**
Start: {A}
- {A} on 0 → {A,B}; {A} on 1 → {A}
- {A,B} on 0 → {A,B,C}; {A,B} on 1 → {A,C}
- {A,B,C} on 0 → {A,B,C}; {A,B,C} on 1 → {A,C}
- {A,C} on 0 → {A,B}; {A,C} on 1 → {A}

Reachable states: {A}, {A,B}, {A,B,C}, {A,C} = **4 states** ✓
Final states (containing C): {A,B,C}, {A,C}

---

**P118.** The regular expression for "strings over {a,b} with second-to-last symbol = a" is:
**Answer:** (a+b)*a(a+b) ✓

---

**P119.** Minimum DFA states for L = a*b* (strings of a's followed by b's)?

**Solution:** States need to track: have we started seeing b's?
- q₀: only a's seen (or start). On a→q₀, on b→q₁
- q₁: seeing b's. On b→q₁, on a→q₂ (dead — a after b not allowed)
- q₂: dead state. On a→q₂, on b→q₂

All of q₀, q₁ are accepting. q₂ is non-accepting. **3 states** ✓

---

**P120.** CYK algorithm runs in what time complexity?
**Answer:** O(n³|G|) where n = string length and |G| = grammar size (in CNF). ✓

---

**P121.** The language L = {aⁱbʲcᵏ | i < j < k} is:
(a) Regular  (b) CFL  (c) CSL  (d) Recursive but not CSL

**Solution:**
- Not CFL: intersecting with a* b* c* and applying pumping arguments shows violation.
  Actually: L = {aⁱbʲcᵏ | i < j < k}. This is more restrictive than {aⁿbⁿcⁿ}.
  L ∩ {aⁿbⁿcⁿ} = ∅ (since i<j<k means can't have i=j=k unless i=j=k=0, but 0<0<0 is false).
  
  Better approach: L is NOT CFL because if it were, L ∩ (a*b*c*) ∩ complement(stuff) would give us {aⁿbⁿ⁺¹cⁿ⁺²} or similar non-CFL language... 

  Actually, the pumping approach: take z = aᵖbᵖ⁺¹cᵖ⁺² ∈ L. Pump vx: if both v and x contain only one type of symbol, pumping can violate the strict inequality. Standard argument applies.

- IS CSL: LBA can compare counts.

**Answer: (c) CSL** ✓

---

**P122.** How many minimum states in DFA for {ε}?
**Answer:** 2 states — start state (accepting), dead state (non-accepting). Any input from start goes to dead state. ✓

---

**P123.** Regular expression for "all strings over {0,1} EXCEPT ε"?
**Answer:** (0+1)(0+1)* or equivalently (0+1)⁺ ✓

---

**P124.** If a grammar has n variables and each production has RHS length ≤ 2 (CNF), what is the maximum depth of a parse tree for a string of length m?
**Answer:** In CNF, each internal node has 2 children. A binary tree with m leaves has depth at least ⌈log₂ m⌉ and at most m-1. The maximum depth is **m-1** (completely skewed tree), but minimum depth is **⌈log₂ m⌉**. ✓

---

**P125.** The number of parse trees for string "aaaa" using grammar S → SS | a is:
**Solution:** This is the Catalan number C₃ = 5.

For n letters with S → SS | a, the number of parse trees = Catalan number C_{n-1}.
For n=4: C₃ = 1/(3+1) × C(6,3) = (1/4) × 20 = **5** ✓

The five trees correspond to the five ways to parenthesize: ((aa)(aa)), (a((aa)a)), (a(a(aa))), ((a(aa))a), (((aa)a)a).

---

**P126.** Which of these is NOT decidable about DFAs?
(a) Is L(M₁) = L(M₂)?  (b) Is L(M) regular?  (c) Is L(M) infinite?  (d) None of the above

**Answer: (d) None** — ALL properties of DFAs are decidable! (b) is trivially true (DFA always gives regular language), so it's decidable (always output YES). ✓

---

**P127.** A 2-stack PDA is equivalent to:
(a) DFA  (b) PDA  (c) TM  (d) LBA

**Answer: (c) TM** — 2-stack PDA = Turing Machine in computational power. ✓

---

**P128.** Is {⟨M⟩ | M is a TM and |L(M)| = 42} decidable?
**Answer:** Rice's theorem — "L(M) has exactly 42 elements" is a non-trivial language property (some TMs accept exactly 42 strings, others don't). **Undecidable** ✓

---

**P129.** Is {⟨M⟩ | M is a TM with exactly 42 states} decidable?
**Answer:** YES — this is a structural property of M, not a property of L(M). Just count states in the encoding. **Decidable** ✓

---

**P130.** For the grammar S → aB | bA, A → aS | bAA | a, B → bS | aBB | b:
This generates the language of all strings with equal #a and #b. True or False?

**Answer: TRUE** — This is the classic grammar for equal a's and b's. ✓

---

**P131-P135:** Quick Classification

| # | Language | Type |
|---|---|---|
| P131 | {a²ⁿ \| n ≥ 0} = {a,aa,aaaa,a⁸,...} | CSL (not CFL) |
| P132 | {aⁿ! \| n ≥ 0} | CSL (not CFL) |
| P133 | {w \| w is a valid regex} | CFL (balanced parentheses) |
| P134 | {a^p \| p is a twin prime} | Decidable (check twin prime) |
| P135 | {⟨M⟩ \| M accepts ⟨M⟩} | RE but not decidable |

---

## 🧮 Formula Cheat Sheet — Extended Edition

### DFA/NFA Size Bounds

| Structure | Bound |
|---|---|
| NFA to DFA | ≤ 2ⁿ states (tight in worst case) |
| ε-NFA to NFA | Same number of states |
| DFA minimization | Unique minimum (up to isomorphism) |
| Union DFA | m × n states (product construction) |
| Intersection DFA | m × n states (product construction) |
| Complement DFA | Same states (swap final/non-final) |
| Concatenation NFA | m + n states |
| Kleene star NFA | n + 1 states (or n with ε-transitions) |
| DFA for "divisible by k" | k states |
| DFA for "mod k = r" | k states |
| DFA for "length mod k" | k states |
| DFA for "contains substring w" | \|w\| + 1 states |
| DFA for "ends with w" | \|w\| + 1 states |

### Grammar Size Bounds

| Conversion | Size Impact |
|---|---|
| CFG to CNF | O(n²) productions |
| CNF to GNF | Can be exponential |
| CFG to PDA | |V| + 1 states (typically) |
| PDA to CFG | O(|Q|² × |Γ|) variables |
| CYK parsing | O(n³ \|G\|) time |
| CYK parsing | O(n² \|V\|) space |
| Earley parsing | O(n³) worst, O(n²) unambiguous, O(n) LR(k) |

### Chomsky Hierarchy Quick Reference

| Type | Grammar | Automaton | Closure | Decision |
|---|---|---|---|---|
| 3 (Regular) | Right/left-linear | DFA/NFA | ALL operations | ALL decidable |
| 2 (CFL) | A → α | PDA (NPDA) | ∪, ·, *, ∩Reg, h, h⁻¹ | Membership, emptiness, finiteness |
| 2' (DCFL) | LR(1) | DPDA | complement, ∩Reg, h⁻¹ | ALL decidable ⭐ |
| 1 (CSL) | α → β, \|α\|≤\|β\| | LBA | ∪, ∩, ·, *, complement | Membership |
| 0 (RE) | Unrestricted | TM | ∪, ∩, ·, *, h, h⁻¹ | ONLY membership (semi-) |

### Key Decidability Results Summary

| Model | Empty? | Finite? | Infinite? | Equiv? | Membership? | Universal? |
|---|---|---|---|---|---|---|
| DFA | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| NFA | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| CFG | ✓ | ✓ | ✓ | ✗ | ✓ | ✗ |
| DPDA | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| PDA | ✓ | ✓ | ✓ | ✗ | ✓ | ✗ |
| LBA | ✗ | ✗ | ✗ | ✗ | ✓ | ✗ |
| TM | ✗ | ✗ | ✗ | ✗ | ✗ (semi) | ✗ |

### Pumping Lemma Quick Comparison

| | Regular Pumping | CFL Pumping |
|---|---|---|
| Form | xyz | uvwxy |
| Conditions | \|xy\| ≤ p, \|y\| ≥ 1 | \|vxy\| ≤ p, \|vy\| ≥ 1 |
| Pumped to | xyⁱz ∈ L | uvⁱwxⁱy ∈ L |
| Segments pumped | 1 (y only) | 2 (v and x simultaneously) |
| Position constraint | First p characters | Consecutive p characters |
| Common use | Prove not regular | Prove not CFL |
| Adversary controls | p, split xyz | p, split uvwxy |
| You control | w (string choice), i | z (string choice), i |

---

## 📝 Practice Set 13: Ultimate Challenge Problems (10 Problems)

**P136.** Is the following decidable: "Given CFG G, does L(G) contain a string of length ≤ 100?"
**Answer:** YES! Enumerate all strings of length ≤ 100 (finite set), check each using CYK (decidable for CFG). **Decidable** ✓

---

**P137.** Is the following decidable: "Given TM M, does M accept any string of length ≤ 100?"
**Answer:** NO! This looks like a specific case but it's still undecidable.

Actually: let's think carefully. We only need to check finitely many strings (all strings of length ≤ 100). For each string w (|w| ≤ 100), we need to check "does M accept w?" — which is the halting/acceptance problem (undecidable for individual strings).

BUT: we could run M on ALL strings of length ≤ 100 IN PARALLEL (dovetailing). If ANY is accepted, we detect it. If NONE is accepted... we never know (might run forever).

So this problem is **RE** (semi-decidable) but **not decidable**. ✓

Wait, actually there's a subtlety: {⟨M⟩ | ∃w, |w|≤100, M accepts w} = {⟨M⟩ | L(M) ∩ {w : |w|≤100} ≠ ∅}. By Rice's theorem: "L(M) contains a string of length ≤ 100" is a non-trivial semantic property → **undecidable**. It's RE by dovetailing. ✓

---

**P138.** L₁ is decidable, L₂ is RE but not decidable. Is L₁ ∪ L₂:
(a) Always decidable  (b) Always RE  (c) Sometimes not RE  (d) Always RE, sometimes decidable

**Answer: (b) Always RE** — union of RE languages is RE. It might or might not be decidable. ✓

---

**P139.** The minimum DFA for {w ∈ {a,b}* | w has an even number of a's and an odd number of b's} has how many states?
**Solution:** Track (parity of a's, parity of b's) = 2×2 = 4 states. All are distinguishable (different combinations).
**Answer: 4 states** ✓

---

**P140.** A DPDA accepts by empty stack. Which languages can it accept?
**Answer:** DPDA by empty stack accepts exactly the prefix-free DCFLs! (No string in L is a prefix of another string in L.) This is because once the stack empties, computation halts — can't process further input. ✓

---

**P141.** The class of languages accepted by 2-way DFA (head can move left/right on read-only input tape) equals:
(a) Regular languages  (b) CFL  (c) CSL  (d) RE

**Answer: (a) Regular languages** — 2DFA = DFA in expressive power (Shepherdson's theorem). ✓

---

**P142.** Is {⟨M⟩ | M is a TM that halts on at least one input} RE?
**Answer:** YES — this is equivalent to NON-EMPTINESS: {⟨M⟩ | L(M) ≠ ∅}. 
By dovetailing: run M on all inputs, if any halts and accepts, we detect it. **RE** ✓

(Note: "halts" vs "accepts" — if the question means "halts" specifically, we need to detect halting on any input. Still RE by dovetailing.)

---

**P143.** If L is CFL and R is regular: L − R is:
(a) Always CFL  (b) Always regular  (c) Sometimes not CFL  (d) Not always CFL

**Answer: (a) Always CFL** — L − R = L ∩ R̄. R̄ is regular (complement). CFL ∩ Regular = CFL. ✓

---

**P144.** Given DFA with n states and alphabet of size k, the maximum number of edges in the transition diagram is:
**Answer:** Each state has exactly k transitions (DFA is total), so **n × k edges** ✓

---

**P145.** A language L ⊆ {a}* is CFL. Then L is:
(a) Always regular  (b) Sometimes CFL but not regular  (c) Always CSL  (d) Can be not CSL

**Answer: (a) Always regular** — by Parikh's theorem, every CFL over a unary alphabet is regular! ✓

---

## 📝 30 GATE Tricks and Shortcuts — Extended Edition

1. **DFA states for "mod k":** Always exactly k states
2. **NFA with n states → DFA:** At most 2ⁿ states, but usually much fewer (compute reachable states only)
3. **Complement of DFA:** Same machine, swap final/non-final — O(1) construction!
4. **Union/intersection of DFAs:** Product construction — states multiply
5. **Pumping lemma:** Choose w carefully to maximize constraint on adversary's xy split
6. **Rice's theorem:** Only for properties of L(M), not structural properties of M
7. **"All/every" in decidability:** Usually makes it co-RE or harder (universal quantifier)
8. **"Exists/some" in decidability:** Usually RE (existential quantifier)
9. **Finite languages are regular:** Always! Every finite set has a DFA.
10. **Co-finite languages are regular:** Complement of finite = regular, complement of regular = regular
11. **CFL over unary alphabet = Regular:** Parikh's theorem!
12. **DCFL closed under complement:** Unique among non-regular grammar classes
13. **Ambiguity ≠ non-determinism ≠ non-context-free:** These are different concepts!
14. **CYK works for any CNF grammar:** O(n³) — memorize this!
15. **2-stack PDA = TM:** Important equivalence
16. **LBA = CSL:** Linear bounded automata recognize exactly context-sensitive languages
17. **Regular ∩ CFL = CFL:** Very useful for proving non-CFL
18. **Diagonal language:** Neither RE nor co-RE — strongest possible undecidability
19. **A_TM is RE not decidable; complement of A_TM is co-RE not decidable**
20. **DFA minimization:** O(n log n) with Hopcroft's algorithm
21. **NP-Complete via reduction:** Always reduce FROM known NP-Complete TO new problem
22. **2-SAT ∈ P, 3-SAT is NP-Complete:** The jump from 2 to 3!
23. **2-Coloring ∈ P, 3-Coloring is NP-Complete:** Same jump!
24. **Euler Path ∈ P, Hamilton Path is NP-Complete:** Circuit vs. path difference
25. **Savitch: NPSPACE = PSPACE:** Nondeterministic space can be squared
26. **Products of DFAs preserve: union (OR finals), intersection (AND finals), XOR (XOR finals)**
27. **For "minimum DFA states" → count Myhill-Nerode equivalence classes**
28. **Proof by Rice's theorem has 3 steps:** (i) property of L(M)? (ii) non-trivial? (iii) done!
29. **DCFL ⊂ LR(1) languages:** Same class exactly
30. **All decidable problems about TMs that don't involve L(M) are decidable** (#states, #transitions, etc.)

---

## 🔬 Deep Dive: Automata Construction Cookbook

### Complete DFA Design Patterns 📖

**Pattern 1: Divisibility by k (Binary, MSB First)**

For "binary number divisible by k", use k states representing remainders {0, 1, ..., k-1}.

**Worked Example: Divisible by 3 (MSB first)**

Current value tracking: if current value is v and we read bit b → new value = 2v + b.
So remainder: r' = (2r + b) mod 3.

| State (r) | On 0 (2r mod 3) | On 1 ((2r+1) mod 3) |
|---|---|---|
| 0 | 0 | 1 |
| 1 | 2 | 0 |
| 2 | 1 | 2 |

Start: 0. Final: {0}. **3 states.**

**Reading LSB First?** Different DFA! Track (remainder_so_far, position_power_mod_3).

**Pattern 2: Counting Occurrences mod k**

For "number of a's ≡ r mod k":
- k states: {0, 1, ..., k-1} representing count mod k
- On a: advance to next state (i → (i+1) mod k)
- On any other symbol: stay in same state
- Start: 0. Final: {r}.

**Worked Example: |w|_a ≡ 2 mod 3 over {a,b}**

| State | On a | On b |
|---|---|---|
| 0 | 1 | 0 |
| 1 | 2 | 1 |
| 2 | 0 | 2 |

Start: 0. Final: {2}. **3 states.**

**Pattern 3: Fixed-Distance-From-End**

For "k-th-from-last symbol is σ":
- DFA needs to remember last k symbols → O(|Σ|ᵏ) states
- NFA: much simpler! Guess "k symbols remain" then verify.

**Example: 3rd-from-last is 1 over {0,1}:**
- NFA: states q₀→q₁→q₂→q₃. δ(q₀,0)=q₀, δ(q₀,1)={q₀,q₁}, δ(q₁,0/1)=q₂, δ(q₂,0/1)=q₃ (final).
- 4 NFA states → up to 2³ = 8 DFA states (since only last 3 symbols matter after the guess).

**Pattern 4: Substring/Pattern Matching**

For "w contains substring pat":
- Build NFA: start state → any input loops → on first char of pat, branch → continue matching → accept.
- Convert to DFA using KMP-like failure function.
- States ≈ |pat| + 1.

**Pattern 5: Two Independent Conditions (Product)**

For "condition A AND condition B":
- Build DFA_A with states Q_A and DFA_B with states Q_B
- Product DFA: states = Q_A × Q_B
- Transition: δ((p,q), a) = (δ_A(p,a), δ_B(q,a))
- Final states for AND: F_A × F_B
- Final states for OR: (F_A × Q_B) ∪ (Q_A × F_B)

**Worked Example:** L = "even number of a's AND number of b's divisible by 3" over {a,b}

DFA_A: 2 states (parity of a's) — {Even_a, Odd_a}
DFA_B: 3 states (count_b mod 3) — {0_b, 1_b, 2_b}

Product: 2 × 3 = **6 states**
- Start: (Even_a, 0_b)
- Final: {(Even_a, 0_b)} (even a's AND 0 mod 3 b's)

Transitions:
| State | On a | On b |
|---|---|---|
| (E,0) | (O,0) | (E,1) |
| (E,1) | (O,1) | (E,2) |
| (E,2) | (O,2) | (E,0) |
| (O,0) | (E,0) | (O,1) |
| (O,1) | (E,1) | (O,2) |
| (O,2) | (E,2) | (O,0) |

### Complete PDA Design Patterns 📖

**Pattern 1: Push-Match (aⁿbⁿ style)**
```
Read a's → push onto stack
Read b's → pop from stack (one per b)
Accept if stack is empty at end
```

**Pattern 2: Guess-the-Middle (palindromes)**
```
Read symbols → push onto stack
At some point (nondeterministically) → stop pushing, start matching
Read remaining symbols → pop matching symbols from stack
```

**Pattern 3: Counter Comparison (unequal counts)**
```
For L = {aⁿbᵐ | n ≠ m}:
Nondeterministically guess n > m or n < m:
  Case n > m: push a's, pop with b's, accept if stack non-empty at end
  Case n < m: push a's, pop with b's, if stack empty before b's done → accept
```

**Pattern 4: Interleaved Processing**
```
For L = {aⁱbʲcᵏ | i = j + k}:
Push i a's → pop j with b's → pop k with c's → accept if empty
(This works because stack handles the sum j+k = i check!)
```

### DFA Minimization — Complete Algorithm with Trace

**Table-Filling Algorithm:**

1. Create a table with entry (p,q) for all state pairs
2. Mark (p,q) if one is final and other isn't (clearly distinguishable)
3. Repeat: mark (p,q) if (δ(p,a), δ(q,a)) is marked for some a ∈ Σ
4. Unmarked pairs → equivalent states → merge

**Complete Worked Example:**

DFA: states {A,B,C,D,E}, start A, final {C,E}
| | a | b |
|---|---|---|
| A | B | C |
| B | D | E |
| C | C | C |
| D | D | E |
| E | C | C |

**Step 1: Initial marking (final vs non-final):**
```
    A    B    C    D    E
A   -
B   .    -
C   X    X    -          ← C is final, A/B/D are not
D   .    .    X    -
E   X    X    .    X    -   ← E is final too
```

**Step 2: Process unmarked pairs:**

(A,B): δ(A,a)=B, δ(B,a)=D → check (B,D)=unmarked. δ(A,b)=C, δ(B,b)=E → check (C,E)=unmarked. Don't mark yet.

(A,D): δ(A,a)=B, δ(D,a)=D → check (B,D)=unmarked. δ(A,b)=C, δ(D,b)=E → check (C,E)=unmarked. Don't mark.

(B,D): δ(B,a)=D, δ(D,a)=D → (D,D)=same. δ(B,b)=E, δ(D,b)=E → (E,E)=same. Don't mark.

(C,E): δ(C,a)=C, δ(E,a)=C → (C,C)=same. δ(C,b)=C, δ(E,b)=C → (C,C)=same. Don't mark.

**Step 3: No more changes. Final table:**
```
    A    B    C    D    E
A   -
B   .    -
C   X    X    -
D   .    .    X    -
E   X    X    .    X    -
```

**Unmarked pairs: (A,B), (A,D), (B,D), (C,E)**

**Equivalent classes: {A,B,D} and {C,E}**

**Minimized DFA: 2 states!**
- State [ABD] (start, non-final): δ([ABD],a) = [ABD], δ([ABD],b) = [CE]
- State [CE] (final): δ([CE],a) = [CE], δ([CE],b) = [CE]

**Language:** Everything that reaches [CE] → strings containing at least one 'b'! (But verify with original.)
Actually: from A, reading 'b' → C (accept). Reading 'a' → B, then 'b' → E (accept). So yes — at least one 'b' in the string → accept.

Wait, reading 'aa': A→B→D. Then 'b': D→E (accept). Reading just 'a...a' stays in {A,B,D} forever.
And strings starting with b: A→C (accept).

So L = {w ∈ {a,b}* | w contains at least one b}. Minimum DFA: 2 states ✓

---

## 🔬 Deep Dive: Arden's Theorem and State Elimination — Extended

### Arden's Theorem — Formal Statement

**Theorem:** If X = AX + B where A and B are regular expressions and ε ∉ L(A), then X = A*B is the unique solution.

**If ε ∈ L(A):** X = A*B is still a solution, but not unique.

### State Elimination Method — Extended Example

**DFA:**
```
States: {1, 2, 3}, start: 1, final: {3}
δ(1,a) = 2, δ(1,b) = 3
δ(2,a) = 2, δ(2,b) = 3
δ(3,a) = 3, δ(3,b) = 3
```

**Step 1: Add new start (s) and new final (f) states**
```
s --ε-→ 1
3 --ε-→ f
```

**Step 2: Eliminate state 2**
Paths through 2: from 1 to 2 (on a), self-loop on 2 (on a), from 2 to 3 (on b).
Combined: 1 →(a)→ 2 →(a*)→ 2 →(b)→ 3 = aa*b = a⁺b

Update: add transition from 1 to 3 labeled a⁺b
Keep existing 1→3 on b.
1→3 now has label: b + a⁺b = (ε + a⁺)b = a*b

**Step 3: Eliminate state 1**
Paths through 1: from s to 1 (on ε), from 1 to 3 (on a*b), self-loop on 1: none explicitly (state 1 only goes to 2 and 3).

Combined: s →(ε)→ 1 →(a*b)→ 3 = a*b

**Step 4: Eliminate state 3**
Paths through 3: from s (effective) to 3 (on a*b), self-loop on 3 (on a+b), from 3 to f (on ε).
Combined: s →(a*b)→ 3 →((a+b)*)→ 3 →(ε)→ f = a*b(a+b)*

**Result: RE = a*b(a+b)***

**Verification:** 
- "b" → a⁰b(a+b)⁰ ✓
- "ab" → a¹b(a+b)⁰ ✓
- "aab" → a²b(a+b)⁰ ✓
- "bab" → a⁰b·ab ✓ (in state 3 after reading b, then read ab)

L = strings that contain at least one 'b' = a*b(a+b)* ✓

### Converting RE to ε-NFA (Thompson's Construction) — Detailed

**Base Cases:**
```
Symbol a:    →(q₀) --a-→ (q₁)

Empty ε:     →(q₀) --ε-→ (q₁)
```

**Union (R₁ | R₂):**
```
              ε→ [NFA for R₁] →ε
→(new_start)                      (new_accept)
              ε→ [NFA for R₂] →ε
```

**Concatenation (R₁R₂):**
```
→[NFA for R₁] →ε→ [NFA for R₂]→
```

**Kleene Star (R₁*):**
```
                    ε
              ┌──────────┐
              ↓           |
→(new_s) →ε→ [NFA for R₁] →ε→ (new_f)
    |                              ↑
    └──────────────ε───────────────┘
```

**Worked Example: RE = (a|b)*abb**

Build bottom-up:
1. NFA for 'a': states 1→2 on 'a'
2. NFA for 'b': states 3→4 on 'b'
3. NFA for (a|b): states 5→{1,3} (ε), {2,4}→6 (ε)
4. NFA for (a|b)*: states 7→5 (ε), 6→7 (ε), 6→8 (ε), 7→8 (ε)
5. NFA for second 'a': 9→10 on 'a'
6. NFA for first 'b': 11→12 on 'b'
7. NFA for second 'b': 13→14 on 'b'
8. Concatenate: 8→9 (ε), 10→11 (ε), 12→13 (ε)

Start: 7, Final: 14. Total: ~14 states (compare to 4-state DFA!)

> 🎯 **GATE Trick:** Thompson's construction always produces O(|RE|) states with at most 2 transitions per state. The resulting NFA may have many states but is structurally simple.

---

## 🔬 Deep Dive: Important Proof Techniques for GATE

### Proof: Every NFA has an Equivalent DFA (Subset Construction Correctness)

**Claim:** For NFA N = (Q, Σ, δ, q₀, F), the DFA D = (Q', Σ, δ', q₀', F') where:
- Q' = P(Q) (power set)
- q₀' = ε-closure({q₀})
- δ'(S, a) = ε-closure(⋃_{q∈S} δ(q, a))
- F' = {S ∈ Q' | S ∩ F ≠ ∅}

accepts L(D) = L(N).

**Proof sketch:**
- Show by induction on |w| that δ̂'({q₀}, w) = δ̂(q₀, w) (extended transition function)
- Base: |w|=0 (w=ε): δ̂'({q₀}, ε) = {q₀} = ε-closure({q₀}) ✓
- Inductive step: w = xa. δ̂'(S, xa) = δ'(δ̂'(S,x), a) = ε-closure(⋃ δ(q,a)) for q in δ̂'(S,x) = δ̂(q₀, xa) ✓

### Proof: Pumping Lemma for Regular Languages

**Proof by pigeonhole:**
- Let M be a DFA for L with p states
- Take any w ∈ L with |w| ≥ p
- Consider computation: q₀, q₁, q₂, ..., qₙ (where n = |w| ≥ p)
- The first p+1 states (q₀ through qₚ) include p+1 states but only p are available → by pigeonhole, some state repeats: qᵢ = qⱼ where 0 ≤ i < j ≤ p
- Split: x = w[1..i], y = w[i+1..j], z = w[j+1..n]
- Then |xy| = j ≤ p ✓, |y| = j-i ≥ 1 ✓
- Pumping: xyⁱz follows the same computation with the loop at qᵢ=qⱼ traversed i times → reaches same final state → accepted ✓

### Proof: CFL is Closed Under Union

**Given:** CFGs G₁ = (V₁, Σ, R₁, S₁) and G₂ = (V₂, Σ, R₂, S₂) (assume V₁ ∩ V₂ = ∅)
**Construct:** G = (V₁ ∪ V₂ ∪ {S}, Σ, R₁ ∪ R₂ ∪ {S → S₁ | S₂}, S)
**Correctness:** S ⇒ S₁ ⇒* w (if w ∈ L(G₁)) or S ⇒ S₂ ⇒* w (if w ∈ L(G₂)). ✓

### Proof: RE ∩ co-RE = Decidable

**Forward (Decidable ⊆ RE ∩ co-RE):** If L is decidable, it's RE (run decider, accept if it accepts). Complement is also decidable hence RE. So L is co-RE too. ✓

**Backward (RE ∩ co-RE ⊆ Decidable):** Suppose L is RE (TM M₁ accepts L) and co-RE (TM M₂ accepts L̄).
Build decider D: "On input w, run M₁ and M₂ in parallel (interleaving steps). 
- If M₁ accepts → ACCEPT (w ∈ L)
- If M₂ accepts → REJECT (w ∈ L̄)
One of them MUST accept (since w ∈ L or w ∈ L̄). So D always halts!" ✓

### Proof: Halting Problem is Undecidable (Complete Diagonalization)

**Claim:** HALT = {⟨M, w⟩ | M halts on w} is undecidable.

**Proof by contradiction:**
1. Assume TM H decides HALT: H(⟨M,w⟩) = accept if M halts on w, reject otherwise.
2. Construct TM D: "On input ⟨M⟩: run H(⟨M,⟨M⟩⟩). If H accepts (M halts on ⟨M⟩) → loop forever. If H rejects → halt."
3. Consider D on input ⟨D⟩:
   - If D halts on ⟨D⟩ → H accepts ⟨D,⟨D⟩⟩ → D loops forever. Contradiction!
   - If D doesn't halt on ⟨D⟩ → H rejects ⟨D,⟨D⟩⟩ → D halts. Contradiction!
4. Both cases lead to contradiction → H cannot exist → HALT is undecidable. ✓

**The Diagonalization Matrix:**
```
        ⟨M₁⟩  ⟨M₂⟩  ⟨M₃⟩  ⟨M₄⟩  ...
M₁:      H     L     H     H    ...    (H=halts, L=loops)
M₂:      L     L     H     L    ...
M₃:      H     H     L     H    ...
M₄:      L     H     H     H    ...
...

D flips the diagonal:
D:        L     H     H     L    ...    (opposite of each Mᵢ on ⟨Mᵢ⟩)

D cannot appear as any Mᵢ in the list → contradiction with assumption that H exists!
```

---

## 📝 Last-Minute Quick Reference Card — Extended

### 10 Most Common GATE Questions in TOC (by frequency)

1. **Minimum DFA states** → Myhill-Nerode or direct construction + minimization
2. **NFA to DFA conversion** → Subset construction, count reachable states
3. **Is language regular/CFL/decidable?** → Use closure properties or pumping lemma
4. **Rice's Theorem application** → Is it about L(M)? Is it non-trivial? → Undecidable
5. **DFA design** → Figure out what information to track, states = combinations
6. **CFG design** → Match structure to recursive rules
7. **Closure properties** → Memorize the table!
8. **Regular expression** → Convert DFA ↔ RE via Arden's or state elimination
9. **Pumping lemma proof** → Choose string at pumping length, argue all splits fail
10. **Decidability classification** → Apply hierarchy: structural → always decidable; L(M) property → Rice's or specific

### Critical Numbers to Remember

| Item | Value |
|---|---|
| DFA for "divisible by n" | n states |
| DFA for "length mod n = r" | n states |
| NFA → DFA worst case | 2ⁿ states |
| DFA minimization time | O(n log n) |
| CYK time | O(n³\|G\|) |
| CNF conversion | O(\|G\|²) productions |
| Catalan number C_n | (2n choose n)/(n+1) |
| Number of REs over {a}: length n | Exponential in n |
| Undecidable problems start at | Turing machines |
| Everything decidable about | DFA, NFA, RE |

### Emergency Disambiguation Table

| "It sounds like..." | But actually... |
|---|---|
| "Is L(M) = ∅?" for TM | Undecidable (Rice's) |
| "Is L(M) = ∅?" for DFA | Decidable (reachability) |
| "Does M have 5 states?" | Decidable (structural) |
| "Does M accept ε?" | Undecidable if TM (Rice's), Decidable if DFA |
| "Is L(M) = L(M')?" for CFG | Undecidable |
| "Is L(M) = L(M')?" for DFA | Decidable (minimization) |
| "Is L regular?" for CFG | Undecidable |
| "Is L CFL?" for TM | Undecidable (Rice's) |
| "Is this string in L?" for PDA | Decidable (CYK or simulate) |
| "Is this string in L?" for TM | Undecidable (halting) |

---

## �📝 Study Strategy for TOC

### Priority Order ⭐⭐⭐

| Priority | Topic | Est. Marks | Time |
|---|---|---|---|
| 1 | DFA/NFA design & minimization | 2 marks | 2 weeks |
| 2 | Closure & Decision properties | 1-2 marks | 1 week |
| 3 | Rice's Theorem & Decidability | 1-2 marks | 1 week |
| 4 | Regular expressions & Pumping Lemma | 1-2 marks | 1 week |
| 5 | CFG design & normal forms | 1-2 marks | 1 week |
| 6 | PDA design | 1 mark | 3 days |
| 7 | CFL Pumping Lemma | 1 mark | 3 days |
| 8 | TM design | 1 mark | 3 days |
| 9 | CYK Algorithm | 0-1 mark | 2 days |
| 10 | NP-completeness basics | 0-1 mark | 2 days |

### 10 Exam-Day Tips:

1. For "is L regular?" → try building a DFA first (if you can, it's regular!)
2. For "prove not regular" → pumping lemma with strategic choice of w
3. For "is it decidable?" → check if Rice's theorem applies (language property? non-trivial?)
4. For CFL problems → check closure properties before trying construction
5. For minimum DFA states → Myhill-Nerode (count equivalence classes)
6. For NFA→DFA → subset construction, don't expand unreachable states
7. For CFG ambiguity → find a string with two different leftmost derivations
8. Read ALL options before choosing (especially for "which is TRUE" questions)
9. For Rice's theorem: structural questions about M are NOT covered (they may be decidable!)
10. Draw transition diagrams with examples — don't just stare at formal definitions

---

## 📝 Practice Set 14: GATE 2024-2025 Style Advanced Problems (15 Problems)

**P146.** An NFA has 5 states. The equivalent minimum DFA can have at most:
(a) 5 states  (b) 25 states  (c) 32 states  (d) 10 states
**Answer: (c) 32 = 2⁵** ✓

---

**P147.** Consider L = {aⁿbᵐ | n ≥ 1, m ≥ 1, n ≠ m}. The minimum DFA/NFA that accepts L:
(a) Has finite states  (b) Doesn't exist (L isn't regular)  (c) Has 3 states  (d) Has n+m states

**Solution:** L = {aⁿbᵐ | n≥1, m≥1} - {aⁿbⁿ | n≥1}. First part is regular (a⁺b⁺), but {aⁿbⁿ} is not regular. Difference: regular - non-regular = ???

Actually, let's check: is L regular? If L were regular, then complement(L) ∩ a⁺b⁺ = {aⁿbⁿ | n≥1} would be regular (regular ∩ regular = regular). But {aⁿbⁿ} is NOT regular. Contradiction! So **L is NOT regular → (b)** ✓

Is L CFL? L = {aⁿbᵐ | n≥1, m≥1, n>m} ∪ {aⁿbᵐ | n≥1, m≥1, n<m}. Both parts are CFL. Union of CFLs is CFL. **L is CFL** ✓

---

**P148.** The total number of distinct languages over Σ = {a} that can be recognized by DFAs with at most 3 states is:

**Solution:** Must enumerate all DFAs with 1, 2, or 3 states over {a} and find distinct languages:

1-state DFAs: ∅ or {a}* → **2 languages**

2-state DFAs (over {a}, 6 distinct languages from previous problem P64): ∅, {ε}, a*, a⁺, even-length, odd-length → **6 languages** 

3-state DFAs: adds languages like "length mod 3 = 0", "length mod 3 = 1", "length mod 3 = 2", unions of these with previous... 

Total over unary: with k states, we can recognize languages of form "n mod p ∈ S" for p ≤ k. The distinct new languages from 3 states add mod-3 patterns.

Distinct languages: ∅, {ε}, {a}, a*, a⁺, even, odd, {aⁿ|n≡0 mod 3}, {aⁿ|n≡1 mod 3}, {aⁿ|n≡2 mod 3}, {aⁿ|n≡0 or 1 mod 3}, {aⁿ|n≡0 or 2 mod 3}, {aⁿ|n≡1 or 2 mod 3}, and more with mixed initial conditions...

This gets complex — for GATE, the answer counting requires careful enumeration. Key insight: with 3 states over unary alphabet, you get languages determined by (tail length, cycle length, accepting states in cycle) where cycle ≤ 3.

---

**P149.** Let G be a CFG with 10 productions, all in CNF. For a string of length 20, how many cells does the CYK table have?

**Solution:** CYK table is triangular: for string of length n = 20:
- Row 1 (length 1 substrings): 20 cells
- Row 2 (length 2): 19 cells
- ...
- Row 20 (length 20): 1 cell
- Total cells: 20 + 19 + ... + 1 = n(n+1)/2 = **210 cells** ✓

Each cell stores a set of variables, computed using the grammar's productions.

---

**P150.** Given Turing Machine M, which of the following is decidable?
(a) Does M accept at least 2 strings?
(b) Does M halt within 100 steps on input "aaa"?
(c) Is L(M) a subset of {a,b}*?
(d) Does M use at most 50 tape cells on all inputs of length ≤ 10?

**Solution:**
(a) Rice's theorem — "L(M) contains at least 2 strings" is non-trivial property of L(M) → **Undecidable**
(b) Simulate M on "aaa" for 100 steps → finite computation → **Decidable** ✓
(c) If Σ = {a,b}: L(M) ⊆ {a,b}* is always TRUE → trivially decidable. If input alphabet is larger: "L(M) ⊆ {a,b}*" is a property of L(M), non-trivial → **Undecidable**. (Depends on interpretation!)
(d) Finitely many inputs of length ≤ 10. For each, simulate M for all possible 50-cell configurations. If M ever uses more → reject. If all halt within space 50 → accept. If some don't halt in bounded configs → detect loop. **Decidable** ✓

**Best answer: (b)** ✓

---

**P151.** A language L is recognized by a 2-counter machine (2 counters that can increment, decrement, and test for zero). Its power equals:
(a) DFA  (b) PDA  (c) LBA  (d) TM

**Answer: (d) TM** — A 2-counter machine is Turing-complete! (One counter can simulate binary tape content, the other assists in computation.) ✓

---

**P152.** The number of strings of length exactly 5 in the language L(G) where G: S → aS | bS | a | b is:
**Solution:** L(G) = {a,b}⁺ (all non-empty strings over {a,b}). Strings of length 5: 2⁵ = **32** ✓

---

**P153.** Is the following computable: "Given two DFAs M₁ and M₂, output the minimum DFA for L(M₁) ∩ L(M₂)"?
**Answer: YES** — Product construction gives DFA for intersection, then minimize using standard algorithm. All steps are algorithmic and terminate. ✓

---

**P154.** Consider the language L = {0ⁱ1ʲ | i ≥ 0, j ≥ 0, gcd(i,j) > 1 or i = j = 0}. This is:
(a) Regular  (b) CFL  (c) Not CFL but decidable  (d) RE but not decidable

**Answer: (c) Not CFL but decidable** — gcd condition can't be tracked by PDA (multiplicative relationships), but can be computed algorithmically (Euclidean algorithm). ✓

---

**P155.** In converting a regular grammar to NFA: the grammar S → aA | bB, A → aA | a, B → bB | b gives an NFA with how many states?

**Solution:** Variables become states + one extra final state:
- S (start), A, B → 3 states from variables
- Terminal productions (A → a, B → b) go to new final state F
- Total: **4 states** {S, A, B, F}

Transitions: δ(S,a)=A, δ(S,b)=B, δ(A,a)={A,F}, δ(B,b)={B,F}
Start: S. Final: {F}.

Language: a⁺ ∪ b⁺ (one or more a's, or one or more b's) ✓

---

**P156.** Let L₁ = {aⁿbⁿ | n ≥ 0} and L₂ = {cⁿdⁿ | n ≥ 0}. What is L₁ · L₂?

**Solution:** L₁L₂ = {aⁿbⁿcᵐdᵐ | n, m ≥ 0}

Is this CFL? YES — CFG: S → AB, A → aAb | ε, B → cBd | ε. ✓

Is this DCFL? YES — DPDA: push a's, match b's, push c's, match d's. The transition from "matching b's" to "reading c's" is deterministic (seeing c after all b's matched). ✓

---

**P157.** The minimum number of states in NFA (without ε-transitions) for the language "all binary strings of length ≥ 3" is:

**Solution:** NFA: count at least 3 symbols
- q₀ →(0,1)→ q₁ →(0,1)→ q₂ →(0,1)→ q₃ (final, self-loop on 0,1)
- **4 states** ✓

---

**P158.** Which closure property distinguishes DCFL from CFL?
(a) Union  (b) Complement  (c) Intersection with regular  (d) Kleene star

**Answer: (b) Complement** — DCFL is closed under complement, CFL is NOT. This is the key distinguishing closure property. ✓

---

**P159.** If P ≠ NP, which is TRUE?
(a) NP-Complete ∩ P = ∅
(b) There exist languages in NP that are neither in P nor NP-Complete (Ladner's theorem)
(c) Both (a) and (b)
(d) Only (a)

**Answer: (c) Both** — Ladner's theorem (1975) proves that if P ≠ NP, there exist NP-intermediate problems (in NP but not in P and not NP-Complete). And if any NP-Complete problem were in P, then P = NP (contradiction). ✓

---

**P160.** The minimum DFA for the language {w ∈ {a,b}* | every 'a' in w is immediately followed by 'b'} has:

**Solution:** Track whether we've just seen an 'a' needing a 'b':
- q₀: normal state (no pending 'a'). Accept state.
  - On 'a' → q₁ (pending 'a')  
  - On 'b' → q₀ (still normal)
- q₁: just read 'a', need 'b' next.
  - On 'b' → q₀ (constraint satisfied). 
  - On 'a' → q_dead (two consecutive a's without b between)
- q_dead: dead state. On anything → q_dead.

Final states: {q₀} — must end in valid state (no pending 'a' at end).

Wait: can string end with 'a'? "every a followed by b" — if string ends in 'a', the last 'a' isn't followed by anything → constraint violated. So q₁ is NOT final. ✓

**3 states: {q₀ (start, final), q₁, q_dead}** ✓

Language recognized: (b | ab)* = strings that are sequences of 'b' and 'ab' ✓

---

## 🔬 Common GATE Traps in TOC — Detailed

### Trap 1: "Regular" Does NOT Mean "Described by Regex in Programming"
Programming regex (with backreferences, lookahead) is MORE powerful than theoretical regex!
- Programming: `(a+)\1` matches aⁿaⁿ → NOT regular!
- Theory: regular expressions use only ∪, ·, * → exactly regular languages

### Trap 2: "Decidable" ≠ "Efficiently Decidable"
- Decidable just means "some TM always halts with correct answer"
- Could take O(2^(2^(2^n))) time — still decidable!
- For efficiency questions → complexity theory (P, NP, etc.)

### Trap 3: Rice's Theorem Doesn't Apply to Everything
- Only applies to **non-trivial properties of L(M)**
- Does NOT apply to: structural properties of M, properties of specific inputs, non-semantic questions
- "Does M have an even number of states?" → Decidable (structural, NOT Rice's)

### Trap 4: CFL Closed Under ∩ with Regular, NOT ∩ with CFL
- L_CFL ∩ L_Regular = CFL ✓
- L_CFL ∩ L_CFL = ??? (might not be CFL!)
- Classic trap: {aⁿbⁿcᵐ} ∩ {aᵐbⁿcⁿ} = {aⁿbⁿcⁿ} which is NOT CFL

### Trap 5: Nondeterminism ≠ Randomness
- NFA: tries ALL paths simultaneously (or magically picks the right one)
- Probabilistic automaton: chooses randomly with probabilities
- These are completely different computational models!

### Trap 6: "Infinite" Language Can Still Be Regular
- a* = {ε, a, aa, aaa, ...} is infinite AND regular!
- "Infinite" doesn't automatically mean complex
- Even Σ* (all strings) is regular!

### Trap 7: Empty Language vs Language Containing Empty String  
- ∅ = empty language (NO strings, not even ε) — Regular!
- {ε} = language with ONE string (the empty string) — Regular!
- These are different: |∅| = 0, |{ε}| = 1

### Trap 8: DPDA vs NPDA Acceptance 
- DPDA by final state: accepts exactly DCFL
- DPDA by empty stack: accepts exactly PREFIX-FREE DCFL (strictly less!)
- NPDA by final state = NPDA by empty stack = all CFL

### Trap 9: Intersection of Regular Languages
- Regular ∩ Regular = Regular ✓ (always!)
- But: min DFA for intersection can be much larger (up to m × n states)
- Don't confuse "regular" with "small DFA"

### Trap 10: TM That "Ignores Its Input"
- TM M that always prints "hello" and halts: L(M) = Σ* (accepts everything)
- TM M that always loops forever: L(M) = ∅ 
- The behavior is about whether M enters accept state, not what it "does"

---

## 🧩 Topic Interconnection Map — Detailed

### How Topics Connect for GATE Questions

```
┌─────────────────────────────────────────────────────────────────────┐
│                    THEORY OF COMPUTATION                            │
│                                                                     │
│   FINITE AUTOMATA ←→ REGULAR EXPRESSIONS ←→ REGULAR GRAMMARS       │
│        │                    │                      │                │
│        │ (Pumping Lemma)    │ (Arden's)            │ (Right-linear) │
│        │                    │                      │                │
│        ▼                    ▼                      ▼                │
│   MYHILL-NERODE ←→ NON-REGULAR PROOFS ←→ CLOSURE PROPERTIES        │
│        │                                           │                │
│        │                                           │ (CFL ∩ Reg)   │
│        ▼                                           ▼                │
│   DFA MINIMIZATION            CONTEXT-FREE GRAMMARS ←→ PDA         │
│        │                          │          │                      │
│        │                          │          │ (CFG↔PDA)            │
│        │                          ▼          ▼                      │
│        │                     CNF/GNF    DPDA vs NPDA                │
│        │                       │              │                     │
│        │                       ▼              ▼                     │
│        │                    CYK          DCFL (LR parsing)          │
│        │                                      │                     │
│        │                                      │ (Compiler Design!)  │
│        │                                      ▼                     │
│        │            PUMPING LEMMA (CFL) → PARSING THEORY            │
│        │                    │                                       │
│        │                    ▼                                       │
│        │         CLOSURE & DECISION PROPERTIES                      │
│        │                    │                                       │
│        ▼                    ▼                                       │
│   CHOMSKY HIERARCHY → TURING MACHINES ←→ UNDECIDABILITY             │
│                            │         │                              │
│                            │         │ (Reductions)                 │
│                            ▼         ▼                              │
│                    LBA (CSL) ← RICE'S THEOREM                       │
│                            │         │                              │
│                            ▼         ▼                              │
│                    HALTING → DIAGONALIZATION                         │
│                    PROBLEM        │                                  │
│                            │      ▼                                 │
│                            ▼   PCP                                  │
│                    COMPLEXITY THEORY                                 │
│                    (P, NP, PSPACE)                                   │
│                            │                                        │
│                            ▼                                        │
│                    NP-COMPLETENESS                                   │
│                    (SAT, 3-SAT, CLIQUE, etc.)                       │
└─────────────────────────────────────────────────────────────────────┘
```

### Cross-Topic Questions Strategy

| If GATE asks about... | Also think about... |
|---|---|
| DFA minimization | Myhill-Nerode, equivalence classes |
| Regular expressions | Arden's theorem, NFA conversion |
| Pumping lemma | Closure properties as alternative proof |
| CFG design | PDA design (they're equivalent!) |
| CNF | CYK algorithm (requires CNF) |
| DPDA | LR(1) parsing, compiler design |
| Rice's theorem | Structural vs. semantic properties |
| Decidability | Which automaton model is involved? |
| NP-completeness | Polynomial-time reductions |
| Closure properties | Quick language classification |

### Year-wise Topic Distribution (GATE CS)

| Year | FA/DFA | RE | PL | CFG | PDA | TM/Rice's | Decidability | Complexity | Total |
|---|---|---|---|---|---|---|---|---|---|
| 2025 | 2 | 1 | 1 | 1 | 0 | 1 | 1 | 1 | 8 |
| 2024 | 1 | 1 | 0 | 1 | 1 | 1 | 1 | 0 | 6 |
| 2023 | 2 | 0 | 1 | 1 | 0 | 2 | 1 | 0 | 7 |
| 2022 | 1 | 1 | 1 | 1 | 1 | 1 | 0 | 1 | 7 |
| 2021 | 2 | 1 | 0 | 2 | 0 | 1 | 1 | 0 | 7 |
| 2020 | 1 | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 7 |
| **Avg** | **1.5** | **0.7** | **0.7** | **1.2** | **0.5** | **1.2** | **0.8** | **0.5** | **~7** |

> 🎯 **Key Insight:** TOC carries ~7 questions per paper (14 marks average). Focus areas: DFA/NFA (most frequent), CFG+Decidability (close second), then the rest.

### Subject Preparation Checklist

- DFA/NFA Design & Minimization: Must master (2+ questions)
- Regular Expressions & Arden's: Important (1 question)
- Pumping Lemma: Know the technique (0-1 questions)
- CFG & CNF: Must master (1-2 questions)
- PDA Design: Important (0-1 questions)
- Rice's Theorem: Must master — quick marks! (1 question)
- Closure Properties: Must memorize table (0-1 questions)
- NP-Completeness: Know basics (0-1 questions)

---

*End of Comprehensive Notes — Theory of Computation for GATE 2027* 🎓🏆
