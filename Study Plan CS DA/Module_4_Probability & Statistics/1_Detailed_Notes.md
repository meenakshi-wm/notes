# 🎲 Probability & Statistics — Detailed Notes for GATE 2027 CS/IT/DA 🎯

> 🌟 **"Probability is the language of uncertainty — it's how machines learn, doctors diagnose, and Netflix knows what you want to watch next!"**

> 💡 **Beginner Analogy:** Life is full of uncertainty 🎰. Will it rain tomorrow? Will my code have bugs? Will my startup succeed? Probability gives us a **mathematical framework** to reason about uncertainty. It doesn't predict the future — it tells us how to make the **best possible decisions** with incomplete information!

---

## 🏭 Where is Probability Used in the Real World?

| 🌍 Industry | 🔧 Application | 📌 Probability Concept |
|---|---|---|
| **Spam Filters (Gmail)** 📧 | Classifying emails as spam/not-spam | Bayes' Theorem (Naive Bayes classifier) |
| **Medical Diagnosis** 🏥 | COVID test accuracy, cancer screening | Conditional probability, Bayes' rule |
| **Weather Forecasting** ⛈️ | "70% chance of rain" | Probability distributions, Bayesian inference |
| **Insurance (LIC, etc.)** 💰 | Premium calculation, risk assessment | Expected value, Central Limit Theorem |
| **Online Advertising (Google Ads)** 📱 | Click-through rate prediction | Bernoulli/Binomial distributions |
| **Netflix/YouTube** 🎬 | "You might also like..." | Conditional probability, Markov chains |
| **Quality Control (Toyota, Intel)** 🏭 | Defect detection in manufacturing | Normal distribution, Sampling |
| **Fraud Detection (Banks)** 🏦 | Detecting unusual transactions | Anomaly detection, Probability distributions |
| **A/B Testing (Amazon, Facebook)** 🔬 | "Is new button color better?" | Hypothesis testing, p-values |
| **Genomics/Drug Discovery** 🧬 | Gene expression analysis | Statistical testing, Multiple comparisons |
| **Self-Driving Cars** 🚗 | Predicting pedestrian behavior | Probabilistic models, Kalman filters |
| **Stock Trading (Algorithms)** 📈 | Price movement prediction | Stochastic processes, Random walks |

---

## 📊 GATE Weightage & Exam Strategy

| Aspect | Details |
|---|---|
| **Expected Marks** | 5–10 marks every year (2–4 questions) |
| **Question Types** | NAT (numerical), MCQ |
| **Frequent Topics** | ⭐ Bayes' theorem, ⭐ Expectation/Variance, ⭐ Normal distribution, ⭐ Conditional probability, ⭐ CLT |
| **Difficulty** | Moderate — formula-heavy but predictable patterns |
| **Strategy** | Master Bayes + distributions + expectation; most GATE Qs are template-based |

> **Tip:** Probability is one of the **highest ROI** subjects — formulaic, predictable, and high-weightage. Never skip it.

---

# 🎲 MODULE 1: Probability Fundamentals

---

## 1.1 Sample Space and Events 🎯

> 💡 **Beginner Analogy — The Menu of Possibilities 🍽️:**
> A sample space is like a restaurant menu — it lists ALL possible things that can happen! Rolling a die? Menu has {1,2,3,4,5,6}. Tossing two coins? Menu has {HH, HT, TH, TT}. An event is like circling the items you want — "rolling an even number" = circling {2, 4, 6}.
>
> Fun fact: When you shuffle a deck of 52 cards, there are **52! ≈ 8 × 10⁶⁷** possible orderings — more than the number of atoms in the observable universe! 🌌 Each shuffle is almost certainly unique in human history!

### Definitions

- **Experiment:** A procedure with uncertain outcomes (e.g., rolling a die)
- **Sample Space ($S$ or $\Omega$):** The set of ALL possible outcomes
- **Event ($E$):** A subset of the sample space

### Types of Sample Spaces

| Type | Example | $S$ |
|---|---|---|
| Finite | Tossing a coin | $\{H, T\}$ |
| Countably Infinite | Toss until first Head | $\{H, TH, TTH, TTTH, \ldots\}$ |
| Uncountable | Picking a random number in $[0,1]$ | $[0,1]$ |

### Event Operations

| Operation | Symbol | Meaning |
|---|---|---|
| Union | $A \cup B$ | A or B (or both) |
| Intersection | $A \cap B$ | A and B |
| Complement | $A^c$ or $\bar{A}$ | Not A |
| Difference | $A - B$ | A but not B = $A \cap B^c$ |
| Mutually Exclusive | $A \cap B = \emptyset$ | Cannot happen together |

### ⭐ De Morgan's Laws

$$\overline{A \cup B} = \bar{A} \cap \bar{B}$$
$$\overline{A \cap B} = \bar{A} \cup \bar{B}$$

**Analogy:** "Not (raining OR cold)" = "Not raining AND not cold"

### ⭐ Inclusion-Exclusion Principle

**For 2 events:**
$$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$

**For 3 events:**
$$P(A \cup B \cup C) = P(A) + P(B) + P(C) - P(A \cap B) - P(A \cap C) - P(B \cap C) + P(A \cap B \cap C)$$

**General formula for $n$ events:**
$$P\left(\bigcup_{i=1}^{n} A_i\right) = \sum_{i} P(A_i) - \sum_{i<j} P(A_i \cap A_j) + \sum_{i<j<k} P(A_i \cap A_j \cap A_k) - \cdots$$

---

**Example 1 (Easy):** A card is drawn from a standard deck. What is $P(\text{King or Heart})$?

$P(K) = 4/52$, $P(H) = 13/52$, $P(K \cap H) = 1/52$

$P(K \cup H) = 4/52 + 13/52 - 1/52 = 16/52 = 4/13$

---

**Example 2 (Medium):** In a class of 60 students, 30 play cricket, 25 play football, 10 play both. Find $P(\text{plays neither})$.

$P(C \cup F) = 30/60 + 25/60 - 10/60 = 45/60 = 3/4$

$P(\text{neither}) = 1 - 3/4 = 1/4$

---

**Example 3 (GATE Style):** 100 students: 45 know C, 35 know Java, 30 know Python, 10 know C and Java, 8 know Java and Python, 12 know C and Python, 5 know all three. How many know at least one language?

$|C \cup J \cup P| = 45 + 35 + 30 - 10 - 8 - 12 + 5 = 85$

---

### Common GATE Traps

- Forgetting to subtract the intersection in union
- Confusing "at least one" (use complement: $1 - P(\text{none})$) with "exactly one"
- For "exactly one of A, B": $P(A) + P(B) - 2P(A \cap B)$

---

## 1.2 Axioms of Probability

1. $P(A) \geq 0$ for any event $A$
2. $P(S) = 1$
3. For mutually exclusive events $A_1, A_2, \ldots$: $P\left(\bigcup A_i\right) = \sum P(A_i)$

### Key Properties

- $P(\bar{A}) = 1 - P(A)$
- $P(\emptyset) = 0$
- $P(A) \leq 1$
- If $A \subseteq B$, then $P(A) \leq P(B)$

---

## ⭐ 1.3 Conditional Probability

### Definition

$$P(A|B) = \frac{P(A \cap B)}{P(B)}, \quad P(B) > 0$$

**Intuition:** Given that B has occurred, what fraction of B's outcomes also satisfy A?

**Analogy:** You're in a room (B). What fraction of the room is covered by a red carpet (A)?

### Multiplication Rule

$$P(A \cap B) = P(A|B) \cdot P(B) = P(B|A) \cdot P(A)$$

**Chain Rule (extended):**
$$P(A \cap B \cap C) = P(A) \cdot P(B|A) \cdot P(C|A \cap B)$$

---

**Example 4 (Easy):** Two cards drawn without replacement. $P(\text{both Aces})$?

$P(A_1 \cap A_2) = P(A_1) \cdot P(A_2 | A_1) = \frac{4}{52} \cdot \frac{3}{51} = \frac{12}{2652} = \frac{1}{221}$

---

**Example 5 (Medium):** A bag has 5 red, 3 blue balls. Two drawn without replacement. $P(\text{2nd is red} | \text{1st is blue})$?

$P(R_2 | B_1) = \frac{5}{7}$ (after removing one blue: 5 red, 2 blue remain)

---

### Tree Diagrams

Tree diagrams are **the most powerful tool** for conditional probability in GATE.

**Structure:**
1. Branch at each stage with probabilities
2. Multiply along paths for joint probabilities
3. Add across paths for total probability

---

## ⭐ 1.4 Law of Total Probability

If $B_1, B_2, \ldots, B_n$ partition $S$ (mutually exclusive, exhaustive), then:

$$P(A) = \sum_{i=1}^{n} P(A|B_i) \cdot P(B_i)$$

**Analogy:** To find the total number of students who passed, sum up "passed from each section × size of section."

---

**Example 6 (GATE Style):** Factory has 3 machines. Machine 1 produces 50% of items (2% defective), Machine 2 produces 30% (3% defective), Machine 3 produces 20% (5% defective). $P(\text{item is defective})$?

$$P(D) = 0.50 \times 0.02 + 0.30 \times 0.03 + 0.20 \times 0.05$$
$$= 0.010 + 0.009 + 0.010 = 0.029$$

---

## ⭐⭐ 1.5 Bayes' Theorem

$$P(B_i | A) = \frac{P(A|B_i) \cdot P(B_i)}{\sum_{j} P(A|B_j) \cdot P(B_j)}$$

**Intuition:** Reverse the direction of conditioning. "Given the effect, find the most likely cause."

**Analogy:** A patient tests positive. Which disease is most likely? Bayes flips from "test accuracy given disease" to "disease probability given positive test."

---

**Example 7 (Continuing Example 6):** If an item is defective, what is $P(\text{from Machine 2})$?

$$P(M_2 | D) = \frac{P(D|M_2) \cdot P(M_2)}{P(D)} = \frac{0.03 \times 0.30}{0.029} = \frac{0.009}{0.029} \approx 0.3103$$

---

**Example 8 (GATE PYQ Style):** Disease prevalence: 1%. Test sensitivity: 99%. Test specificity: 95%. If positive, $P(\text{actually has disease})$?

$P(D) = 0.01$, $P(+|D) = 0.99$, $P(+|D^c) = 0.05$

$P(+) = 0.99 \times 0.01 + 0.05 \times 0.99 = 0.0099 + 0.0495 = 0.0594$

$P(D|+) = \frac{0.0099}{0.0594} \approx 0.1667$

**Surprising result!** Even with a 99% accurate test, only ~17% chance of having the disease. This is the **base rate fallacy** — a classic GATE conceptual question.

---

## 1.6 Independence

### Definition

Events $A$ and $B$ are **independent** if:
$$P(A \cap B) = P(A) \cdot P(B)$$

Equivalently: $P(A|B) = P(A)$ or $P(B|A) = P(B)$

### Pairwise vs Mutual Independence

For events $A, B, C$ to be **mutually independent**, ALL of these must hold:
- $P(A \cap B) = P(A)P(B)$
- $P(A \cap C) = P(A)P(C)$
- $P(B \cap C) = P(B)P(C)$
- $P(A \cap B \cap C) = P(A)P(B)P(C)$ ← **often forgotten!**

> **GATE Trap:** Pairwise independence does NOT imply mutual independence!

### Conditional Independence

$A$ and $B$ are conditionally independent given $C$ if:
$$P(A \cap B | C) = P(A|C) \cdot P(B|C)$$

> **Key Fact:** Independence does NOT imply conditional independence, and vice versa.

---

**Example 9:** Toss two fair coins. $A$: "1st is H", $B$: "2nd is H", $C$: "both same".

- $P(A) = 1/2$, $P(B) = 1/2$, $P(A \cap B) = 1/4 = P(A) \times P(B)$ → Independent ✓
- $P(A|C) = 1/2$, $P(B|C) = 1/2$, $P(A \cap B|C) = 1/2 \neq 1/4$ → NOT conditionally independent given $C$ ✗

---

## 1.7 Random Variables

### Definition

A **random variable** $X$ is a function from sample space to real numbers: $X: S \to \mathbb{R}$

**Analogy:** A random variable is a "numerical label" we attach to each outcome.

### Types

| Type | Range | Example |
|---|---|---|
| **Discrete** | Countable (finite or countably infinite) | Number of heads in 10 tosses |
| **Continuous** | Uncountable (intervals) | Time to failure, height |

---

## ⭐ 1.8 Probability Mass Function (PMF)

For a discrete RV $X$:

$$p_X(x) = P(X = x)$$

### Properties
1. $p_X(x) \geq 0$ for all $x$
2. $\sum_{\text{all } x} p_X(x) = 1$

---

**Example 10:** Roll two fair dice. Let $X$ = sum. Find the PMF.

| $x$ | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| $p_X(x)$ | $\frac{1}{36}$ | $\frac{2}{36}$ | $\frac{3}{36}$ | $\frac{4}{36}$ | $\frac{5}{36}$ | $\frac{6}{36}$ | $\frac{5}{36}$ | $\frac{4}{36}$ | $\frac{3}{36}$ | $\frac{2}{36}$ | $\frac{1}{36}$ |

Sum = $36/36 = 1$ ✓

---

## ⭐⭐ 1.9 Expectation (Expected Value)

### Definition

$$E[X] = \sum_{x} x \cdot p_X(x) \quad \text{(discrete)}$$

$$E[X] = \int_{-\infty}^{\infty} x \cdot f_X(x)\, dx \quad \text{(continuous)}$$

**Intuition:** The "long-run average" — if you repeat the experiment infinitely many times, the average outcome converges to $E[X]$.

**Analogy:** The center of gravity of the probability distribution.

### ⭐ Linearity of Expectation

$$E[aX + bY + c] = aE[X] + bE[Y] + c$$

> **This works ALWAYS — even if $X$ and $Y$ are NOT independent!** This is the most powerful property.

### LOTUS (Law of the Unconscious Statistician)

$$E[g(X)] = \sum_{x} g(x) \cdot p_X(x) \quad \text{(discrete)}$$

$$E[g(X)] = \int_{-\infty}^{\infty} g(x) \cdot f_X(x)\, dx \quad \text{(continuous)}$$

> You do NOT need the distribution of $g(X)$ — just apply $g$ directly to $x$ and weight by the original PMF/PDF.

### ⭐ Recursive Method for Expectation

For problems like "expected number of tosses to get first Head":

Let $E$ = expected number of tosses.
- With prob $p$: done in 1 toss → contributes $p \cdot 1$
- With prob $1-p$: failed, restart → contributes $(1-p)(1 + E)$

$$E = p \cdot 1 + (1-p)(1 + E) = 1 + (1-p)E$$
$$E \cdot p = 1 \implies E = \frac{1}{p}$$

---

**Example 11 (Easy):** $E[X]$ where $X$ = outcome of a fair die.

$E[X] = \frac{1+2+3+4+5+6}{6} = 3.5$

---

**Example 12 (Medium):** $X \sim \text{Binomial}(n, p)$. Find $E[X]$.

By linearity: $X = X_1 + X_2 + \cdots + X_n$ where $X_i \sim \text{Bernoulli}(p)$.

$E[X] = E[X_1] + \cdots + E[X_n] = np$

---

**Example 13 (GATE Style):** Roll a fair die repeatedly until you get a 6. Expected number of rolls?

Geometric distribution with $p = 1/6$. $E[X] = 1/p = 6$.

---

**Example 14 (Recursive):** Toss a fair coin repeatedly. Expected tosses to get two consecutive Heads?

Let $E$ = answer. After first toss:
- T (prob 1/2): wasted, restart → $1 + E$
- H then T (prob 1/4): wasted 2, restart → $2 + E$  
- HH (prob 1/4): done → $2$

$E = \frac{1}{2}(1+E) + \frac{1}{4}(2+E) + \frac{1}{4}(2)$

$E = \frac{1}{2} + \frac{E}{2} + \frac{1}{2} + \frac{E}{4} + \frac{1}{2} = \frac{3}{2} + \frac{3E}{4}$

$\frac{E}{4} = \frac{3}{2} \implies E = 6$

---

## 1.10 Cumulative Distribution Function (CDF)

$$F_X(x) = P(X \leq x)$$

### Properties
1. $0 \leq F_X(x) \leq 1$
2. Non-decreasing: $x_1 < x_2 \implies F_X(x_1) \leq F_X(x_2)$
3. $\lim_{x \to -\infty} F_X(x) = 0$, $\lim_{x \to \infty} F_X(x) = 1$
4. Right-continuous: $F_X(x) = F_X(x^+)$

### Key Formulas

$$P(a < X \leq b) = F_X(b) - F_X(a)$$
$$P(X > a) = 1 - F_X(a)$$
$$P(X = a) = F_X(a) - F_X(a^-) \quad \text{(jump at } a\text{)}$$

For continuous RV: $f_X(x) = \frac{d}{dx} F_X(x)$

---

## ⭐ 1.11 Variance

### Definition

$$\text{Var}(X) = E[(X - \mu)^2] = E[X^2] - (E[X])^2$$

**Intuition:** Measures the "spread" of the distribution around the mean.

### Key Properties

| Property | Formula |
|---|---|
| $\text{Var}(c) = 0$ | Constant has no spread |
| $\text{Var}(aX + b) = a^2 \text{Var}(X)$ | Shifting doesn't change variance |
| $\text{Var}(X+Y) = \text{Var}(X) + \text{Var}(Y)$ | **Only if $X, Y$ independent** |
| $\text{Var}(X+Y) = \text{Var}(X) + \text{Var}(Y) + 2\text{Cov}(X,Y)$ | General case |
| Standard Deviation | $\sigma = \sqrt{\text{Var}(X)}$ |

> **GATE Trap:** $\text{Var}(X + Y) = \text{Var}(X) + \text{Var}(Y)$ requires independence! If not independent, add $2\text{Cov}(X,Y)$.

---

**Example 15 (Easy):** $X$ is uniform on $\{1, 2, 3, 4, 5\}$.

$E[X] = 3$, $E[X^2] = (1+4+9+16+25)/5 = 55/5 = 11$

$\text{Var}(X) = 11 - 9 = 2$

---

**Example 16 (GATE Style):** If $E[X] = 2$ and $E[X^2] = 8$, find $\text{Var}(3X + 5)$.

$\text{Var}(X) = 8 - 4 = 4$

$\text{Var}(3X + 5) = 9 \times 4 = 36$

---

## ⭐⭐ 1.12 Discrete Distributions

### Summary Table

| Distribution | PMF | $E[X]$ | $\text{Var}(X)$ | Use Case |
|---|---|---|---|---|
| **Bernoulli($p$)** | $P(X=1)=p, P(X=0)=1-p$ | $p$ | $p(1-p)$ | Single trial, success/failure |
| **Binomial($n,p$)** | $\binom{n}{k}p^k(1-p)^{n-k}$ | $np$ | $np(1-p)$ | # successes in $n$ independent trials |
| **Poisson($\lambda$)** | $\frac{e^{-\lambda}\lambda^k}{k!}$ | $\lambda$ | $\lambda$ | Rare events in interval |
| **Geometric($p$)** | $(1-p)^{k-1}p$ | $\frac{1}{p}$ | $\frac{1-p}{p^2}$ | # trials until first success |
| **Discrete Uniform($a,b$)** | $\frac{1}{b-a+1}$ | $\frac{a+b}{2}$ | $\frac{(b-a+1)^2 - 1}{12}$ | Equally likely integer outcomes |

---

### Bernoulli Distribution

$X \sim \text{Bernoulli}(p)$: Binary outcome.

$P(X = 1) = p$, $P(X = 0) = 1 - p = q$

$E[X] = p$, $\text{Var}(X) = pq$

---

### ⭐ Binomial Distribution

$X \sim \text{Bin}(n, p)$: Number of successes in $n$ independent Bernoulli trials.

$$P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}, \quad k = 0, 1, \ldots, n$$

**Key relationships:**
- Sum of $n$ independent $\text{Bernoulli}(p)$ = $\text{Bin}(n, p)$
- $\text{Bin}(n_1, p) + \text{Bin}(n_2, p) = \text{Bin}(n_1 + n_2, p)$ (**same $p$!**)

---

**Example 17:** Toss a fair coin 10 times. $P(\text{exactly 3 heads})$?

$P(X=3) = \binom{10}{3} (0.5)^3 (0.5)^7 = 120 \times \frac{1}{1024} = \frac{120}{1024} = \frac{15}{128}$

---

**Example 18 (GATE Style):** A biased coin has $P(H) = 0.3$. Tossed 5 times. $P(\text{at least 2 heads})$?

$P(X \geq 2) = 1 - P(X=0) - P(X=1)$

$P(X=0) = (0.7)^5 = 0.16807$

$P(X=1) = 5 \times 0.3 \times (0.7)^4 = 5 \times 0.3 \times 0.2401 = 0.36015$

$P(X \geq 2) = 1 - 0.16807 - 0.36015 = 0.47178$

---

### ⭐ Poisson Distribution

$X \sim \text{Poisson}(\lambda)$: Counts rare events in a fixed interval.

$$P(X = k) = \frac{e^{-\lambda} \lambda^k}{k!}, \quad k = 0, 1, 2, \ldots$$

**Key properties:**
- $E[X] = \text{Var}(X) = \lambda$ (mean equals variance!)
- Poisson approximation to Binomial: When $n$ is large, $p$ is small, $\lambda = np$ moderate → $\text{Bin}(n,p) \approx \text{Poisson}(np)$
- Sum: $\text{Poisson}(\lambda_1) + \text{Poisson}(\lambda_2) = \text{Poisson}(\lambda_1 + \lambda_2)$

---

**Example 19:** Average of 3 typos per page. $P(\text{no typos on a page})$?

$P(X=0) = e^{-3} \approx 0.0498$

---

**Example 20:** Emails arrive at rate 5/hour. $P(\text{more than 2 in 30 min})$?

$\lambda_{30\min} = 2.5$

$P(X > 2) = 1 - P(X=0) - P(X=1) - P(X=2)$

$= 1 - e^{-2.5}(1 + 2.5 + 3.125) = 1 - e^{-2.5}(6.625) = 1 - 0.5438 = 0.4562$

---

### Geometric Distribution

$X \sim \text{Geo}(p)$: Number of trials until first success.

$$P(X = k) = (1-p)^{k-1} p, \quad k = 1, 2, 3, \ldots$$

**Memoryless property:** $P(X > m + n | X > m) = P(X > n)$

> **GATE Trap:** Some books define Geometric as "number of failures before first success" with $P(X=k) = (1-p)^k p$ for $k = 0, 1, 2, \ldots$ Mean would be $(1-p)/p$.

---

### Discrete Uniform Distribution

$X \sim \text{DU}(a, b)$: Equally likely on $\{a, a+1, \ldots, b\}$.

$P(X = k) = \frac{1}{n}$ where $n = b - a + 1$

$E[X] = \frac{a+b}{2}$, $\text{Var}(X) = \frac{n^2 - 1}{12}$

---

## 1.13 Continuous Distributions

### ⭐ Summary Table

| Distribution | PDF | $E[X]$ | $\text{Var}(X)$ | Support |
|---|---|---|---|---|
| **Uniform$(a,b)$** | $\frac{1}{b-a}$ | $\frac{a+b}{2}$ | $\frac{(b-a)^2}{12}$ | $[a,b]$ |
| **Normal$(\mu, \sigma^2)$** | $\frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$ | $\mu$ | $\sigma^2$ | $(-\infty, \infty)$ |
| **Exponential$(\lambda)$** | $\lambda e^{-\lambda x}$ | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^2}$ | $[0, \infty)$ |

---

### Continuous Uniform Distribution

$X \sim \text{Uniform}(a, b)$

$$f_X(x) = \begin{cases} \frac{1}{b-a} & a \leq x \leq b \\ 0 & \text{otherwise} \end{cases}$$

$$F_X(x) = \begin{cases} 0 & x < a \\ \frac{x-a}{b-a} & a \leq x \leq b \\ 1 & x > b \end{cases}$$

---

**Example 21:** Bus arrives uniformly at random in $[0, 30]$ minutes. $P(\text{wait} > 20)$?

$P(X > 20) = \frac{30 - 20}{30 - 0} = \frac{1}{3}$

---

### ⭐⭐ Normal (Gaussian) Distribution

$X \sim N(\mu, \sigma^2)$

$$f_X(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)$$

### Standard Normal: $Z \sim N(0, 1)$

**Standardization:** If $X \sim N(\mu, \sigma^2)$, then $Z = \frac{X - \mu}{\sigma} \sim N(0, 1)$

### Key Probability Values (Memorize!)

| $z$ | $\Phi(z) = P(Z \leq z)$ |
|---|---|
| 1.0 | 0.8413 |
| 1.645 | 0.95 |
| 1.96 | 0.975 |
| 2.0 | 0.9772 |
| 2.33 | 0.99 |
| 2.576 | 0.995 |
| 3.0 | 0.9987 |

### Empirical Rule (68-95-99.7)
- $P(\mu - \sigma < X < \mu + \sigma) \approx 68\%$
- $P(\mu - 2\sigma < X < \mu + 2\sigma) \approx 95\%$
- $P(\mu - 3\sigma < X < \mu + 3\sigma) \approx 99.7\%$

### Properties of Normal
- **Symmetry:** $\Phi(-z) = 1 - \Phi(z)$
- **Linear combination:** If $X \sim N(\mu_1, \sigma_1^2)$ and $Y \sim N(\mu_2, \sigma_2^2)$ are independent, then $aX + bY \sim N(a\mu_1 + b\mu_2, a^2\sigma_1^2 + b^2\sigma_2^2)$
- Mean = Median = Mode = $\mu$

---

**Example 22:** $X \sim N(100, 25)$. Find $P(X > 110)$.

$Z = \frac{110 - 100}{5} = 2$

$P(X > 110) = P(Z > 2) = 1 - \Phi(2) = 1 - 0.9772 = 0.0228$

---

**Example 23 (GATE Style):** Lifetimes are $N(1000, 100^2)$ hours. What fraction lasts between 800 and 1200 hours?

$Z_1 = (800-1000)/100 = -2$, $Z_2 = (1200-1000)/100 = 2$

$P(-2 < Z < 2) = 2\Phi(2) - 1 = 2(0.9772) - 1 = 0.9544 \approx 95.44\%$

---

### ⭐ Exponential Distribution

$X \sim \text{Exp}(\lambda)$

$$f_X(x) = \lambda e^{-\lambda x}, \quad x \geq 0$$

$$F_X(x) = 1 - e^{-\lambda x}, \quad x \geq 0$$

$E[X] = 1/\lambda$, $\text{Var}(X) = 1/\lambda^2$

### Memoryless Property

$$P(X > s + t \mid X > s) = P(X > t)$$

> The exponential distribution is the ONLY continuous memoryless distribution.

**Connection to Poisson:** If events occur according to a Poisson process with rate $\lambda$, inter-arrival times are $\text{Exp}(\lambda)$.

---

**Example 24:** Light bulb lifetime $\sim \text{Exp}(1/1000)$. $P(\text{lasts > 1500 hours})$?

$P(X > 1500) = e^{-1500/1000} = e^{-1.5} \approx 0.2231$

---

**Example 25:** Given that a bulb has lasted 500 hours, $P(\text{lasts > 1500 total})$?

By memorylessness: $P(X > 1500 | X > 500) = P(X > 1000) = e^{-1} \approx 0.3679$

---

## 1.14 Mean, Mode, Median

| Measure | Definition |
|---|---|
| **Mean** | $E[X]$ — balance point |
| **Median** | Value $m$ where $P(X \leq m) = 0.5$ |
| **Mode** | Most likely value (peak of PMF/PDF) |

**For Normal:** Mean = Median = Mode

**For Exponential:** Mean $= 1/\lambda$, Median $= \ln(2)/\lambda$, Mode $= 0$

**Relationship for skewed distributions:**
- Right-skewed: Mode < Median < Mean
- Left-skewed: Mean < Median < Mode

---

# MODULE 2: Joint Distributions & Advanced Topics

---

## ⭐ 2.1 Joint PMF

For discrete RVs $X$ and $Y$:

$$p_{X,Y}(x, y) = P(X = x, Y = y)$$

### Properties
1. $p_{X,Y}(x, y) \geq 0$
2. $\sum_x \sum_y p_{X,Y}(x, y) = 1$

### Marginal PMFs
$$p_X(x) = \sum_y p_{X,Y}(x, y), \quad p_Y(y) = \sum_x p_{X,Y}(x, y)$$

### Independence
$X$ and $Y$ are independent iff $p_{X,Y}(x,y) = p_X(x) \cdot p_Y(y)$ for all $x, y$.

---

**Example 26:** Joint PMF table:

| $X \backslash Y$ | 0 | 1 | $p_X(x)$ |
|---|---|---|---|
| 0 | 0.1 | 0.2 | 0.3 |
| 1 | 0.3 | 0.4 | 0.7 |
| $p_Y(y)$ | 0.4 | 0.6 | 1.0 |

Check independence: $p_{X,Y}(0,0) = 0.1$, but $p_X(0) \cdot p_Y(0) = 0.3 \times 0.4 = 0.12 \neq 0.1$

→ NOT independent.

---

## 2.2 Conditional Expectation

$$E[X | Y = y] = \sum_x x \cdot P(X = x | Y = y)$$

$$E[X | Y = y] = \sum_x x \cdot \frac{p_{X,Y}(x,y)}{p_Y(y)}$$

> $E[X|Y]$ is itself a **random variable** — it's a function of $Y$.

---

## ⭐ 2.3 Law of Total Expectation (Tower Property)

$$E[X] = E[E[X|Y]] = \sum_y E[X|Y=y] \cdot P(Y=y)$$

**Continuous version:**
$$E[X] = \int_{-\infty}^{\infty} E[X|Y=y] \cdot f_Y(y)\, dy$$

**Analogy:** To find average income in a country, compute average income per state, then take the weighted average by population.

---

**Example 27 (GATE Style):** Roll a fair die to get $N$. Then toss a fair coin $N$ times. Let $X$ = number of heads. Find $E[X]$.

$E[X|N=n] = n/2$ (Binomial with parameters $n$ and $1/2$)

$E[X] = E[E[X|N]] = E[N/2] = E[N]/2 = 3.5/2 = 1.75$

---

**Example 28: Law of Total Variance**

$$\text{Var}(X) = E[\text{Var}(X|Y)] + \text{Var}(E[X|Y])$$

("Average of variances" + "Variance of averages")

---

## 2.4 Continuous Random Variables — PDFs and CDFs

### Probability Density Function (PDF)

$$P(a \leq X \leq b) = \int_a^b f_X(x)\, dx$$

### Properties
1. $f_X(x) \geq 0$
2. $\int_{-\infty}^{\infty} f_X(x)\, dx = 1$
3. $f_X(x)$ can be > 1 (it's a density, not probability!)
4. $P(X = a) = 0$ for continuous RVs

### CDF ↔ PDF relationship
$$F_X(x) = \int_{-\infty}^{x} f_X(t)\, dt, \quad f_X(x) = \frac{d}{dx} F_X(x)$$

---

**Example 29:** $f_X(x) = 3x^2$ for $0 \leq x \leq 1$. Find $E[X]$ and $\text{Var}(X)$.

$E[X] = \int_0^1 x \cdot 3x^2\, dx = 3 \int_0^1 x^3\, dx = 3 \cdot \frac{1}{4} = \frac{3}{4}$

$E[X^2] = \int_0^1 x^2 \cdot 3x^2\, dx = 3 \cdot \frac{1}{5} = \frac{3}{5}$

$\text{Var}(X) = \frac{3}{5} - \frac{9}{16} = \frac{48 - 45}{80} = \frac{3}{80}$

---

## ⭐ 2.5 Joint PDF and CDF

### Joint PDF

$$P((X,Y) \in A) = \iint_A f_{X,Y}(x,y)\, dx\, dy$$

### Properties
1. $f_{X,Y}(x,y) \geq 0$
2. $\int_{-\infty}^{\infty}\int_{-\infty}^{\infty} f_{X,Y}(x,y)\, dx\, dy = 1$

### Marginal PDFs
$$f_X(x) = \int_{-\infty}^{\infty} f_{X,Y}(x,y)\, dy$$

### Joint CDF
$$F_{X,Y}(x, y) = P(X \leq x, Y \leq y) = \int_{-\infty}^{x}\int_{-\infty}^{y} f_{X,Y}(s,t)\, dt\, ds$$

### Independence
$$f_{X,Y}(x,y) = f_X(x) \cdot f_Y(y) \text{ for all } (x,y)$$

---

**Example 30:** $f_{X,Y}(x,y) = 6(1-y)$ for $0 < x < y < 1$, else 0.

Marginals:

$f_Y(y) = \int_0^y 6(1-y)\, dx = 6y(1-y), \quad 0 < y < 1$

$f_X(x) = \int_x^1 6(1-y)\, dy = 6\left[(1-y) \cdot (-1)\right]_x^1 - \text{wait, let's compute:}$

$= 6\int_x^1 (1-y)\, dy = 6\left[y - \frac{y^2}{2}\right]_x^1 = 6\left[\frac{1}{2} - x + \frac{x^2}{2}\right] = 3(1-x)^2$

---

## 2.6 Conditional PDF

$$f_{X|Y}(x|y) = \frac{f_{X,Y}(x,y)}{f_Y(y)}$$

$E[X|Y=y] = \int x \cdot f_{X|Y}(x|y)\, dx$

---

## ⭐ 2.7 Covariance

### Definition

$$\text{Cov}(X, Y) = E[XY] - E[X]E[Y]$$

Equivalently: $\text{Cov}(X,Y) = E[(X - \mu_X)(Y - \mu_Y)]$

### Properties

| Property | Formula |
|---|---|
| $\text{Cov}(X, X)$ | $= \text{Var}(X)$ |
| $\text{Cov}(X, Y)$ | $= \text{Cov}(Y, X)$ (symmetric) |
| $\text{Cov}(aX, bY)$ | $= ab \cdot \text{Cov}(X, Y)$ |
| $\text{Cov}(X+Y, Z)$ | $= \text{Cov}(X,Z) + \text{Cov}(Y,Z)$ (bilinear) |
| $\text{Cov}(X, c)$ | $= 0$ |
| $\text{Var}(X \pm Y)$ | $= \text{Var}(X) + \text{Var}(Y) \pm 2\text{Cov}(X,Y)$ |

> If $X, Y$ are independent, then $\text{Cov}(X,Y) = 0$. **But $\text{Cov} = 0$ does NOT imply independence!**

---

**Example 31 (Classic Counterexample):** $X \sim \text{Uniform}(-1, 1)$, $Y = X^2$.

$E[XY] = E[X^3] = 0$ (odd function, symmetric dist)

$E[X]E[Y] = 0 \cdot E[X^2] = 0$

$\text{Cov}(X, Y) = 0$. But $Y = X^2$ means they are completely dependent!

---

**Example 32 (GATE Style):** $X$ and $Y$ have joint PMF. Compute $\text{Cov}(X,Y)$.

| $X \backslash Y$ | 1 | 2 |
|---|---|---|
| 0 | 0.2 | 0.3 |
| 1 | 0.3 | 0.2 |

$E[X] = 0(0.5) + 1(0.5) = 0.5$

$E[Y] = 1(0.5) + 2(0.5) = 1.5$

$E[XY] = 0 \cdot 1 \cdot 0.2 + 0 \cdot 2 \cdot 0.3 + 1 \cdot 1 \cdot 0.3 + 1 \cdot 2 \cdot 0.2 = 0.7$

$\text{Cov}(X,Y) = 0.7 - 0.5 \times 1.5 = 0.7 - 0.75 = -0.05$

---

## 2.8 Covariance Matrix

For a random vector $\mathbf{X} = (X_1, X_2, \ldots, X_n)^T$:

$$\Sigma = \text{Cov}(\mathbf{X}) = E[(\mathbf{X} - \boldsymbol{\mu})(\mathbf{X} - \boldsymbol{\mu})^T]$$

$$\Sigma_{ij} = \text{Cov}(X_i, X_j)$$

### Properties
- **Symmetric:** $\Sigma = \Sigma^T$
- **Positive semi-definite:** $\mathbf{a}^T \Sigma \mathbf{a} \geq 0$ for all $\mathbf{a}$
- **Diagonal elements** = variances: $\Sigma_{ii} = \text{Var}(X_i)$
- **Off-diagonal elements** = covariances

For $\mathbf{X} = (X, Y)^T$:

$$\Sigma = \begin{pmatrix} \text{Var}(X) & \text{Cov}(X,Y) \\ \text{Cov}(Y,X) & \text{Var}(Y) \end{pmatrix}$$

---

## ⭐ 2.9 Correlation

### Pearson Correlation Coefficient

$$\rho(X, Y) = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y} = \frac{\text{Cov}(X,Y)}{\sqrt{\text{Var}(X) \cdot \text{Var}(Y)}}$$

### Properties
- $-1 \leq \rho \leq 1$
- $|\rho| = 1$ iff $Y = aX + b$ for some constants (perfect linear relationship)
- $\rho = 0$: uncorrelated (no LINEAR relationship, could still be dependent)
- $\rho > 0$: positive association
- $\rho < 0$: negative association

---

**Example 33:** From Example 32:

$\text{Var}(X) = E[X^2] - (E[X])^2 = 0.5 - 0.25 = 0.25$

$\text{Var}(Y) = E[Y^2] - (E[Y])^2$

$E[Y^2] = 1^2(0.5) + 2^2(0.5) = 2.5$

$\text{Var}(Y) = 2.5 - 2.25 = 0.25$

$\rho = \frac{-0.05}{\sqrt{0.25 \times 0.25}} = \frac{-0.05}{0.25} = -0.2$

---

# MODULE 3: Statistics & Hypothesis Testing

---

## ⭐ 3.1 Sampling Distribution

### Population vs Sample

| Concept | Symbol | Description |
|---|---|---|
| Population mean | $\mu$ | True parameter |
| Population variance | $\sigma^2$ | True parameter |
| Sample mean | $\bar{X} = \frac{1}{n}\sum X_i$ | Statistic (random variable) |
| Sample variance | $S^2 = \frac{1}{n-1}\sum (X_i - \bar{X})^2$ | Unbiased estimator of $\sigma^2$ |

### Properties of Sample Mean

If $X_1, X_2, \ldots, X_n$ are i.i.d. with mean $\mu$ and variance $\sigma^2$:

$$E[\bar{X}] = \mu$$

$$\text{Var}(\bar{X}) = \frac{\sigma^2}{n}$$

$$\text{SD}(\bar{X}) = \frac{\sigma}{\sqrt{n}} \quad (\text{Standard Error})$$

> **Why $n-1$?** Sample variance uses $n-1$ (Bessel's correction) to give an unbiased estimator. $E[S^2] = \sigma^2$.

---

## ⭐⭐ 3.2 Central Limit Theorem (CLT)

### Statement

If $X_1, X_2, \ldots, X_n$ are i.i.d. with mean $\mu$ and variance $\sigma^2$ (any distribution!), then for large $n$:

$$\bar{X} \approx N\left(\mu, \frac{\sigma^2}{n}\right)$$

Equivalently:

$$Z = \frac{\bar{X} - \mu}{\sigma / \sqrt{n}} \xrightarrow{d} N(0, 1)$$

**Intuition:** No matter the shape of the original distribution, the average of many observations looks Normal.

**Rule of thumb:** $n \geq 30$ is usually "large enough" for CLT.

### Sum Version

$$S_n = X_1 + X_2 + \cdots + X_n \approx N(n\mu, n\sigma^2)$$

---

**Example 34:** A factory produces bolts with mean length 10 cm, SD 0.5 cm. Sample of 100 bolts. $P(\bar{X} > 10.1)$?

$\bar{X} \sim N\left(10, \frac{0.25}{100}\right) = N(10, 0.0025)$

$Z = \frac{10.1 - 10}{0.05} = 2$

$P(\bar{X} > 10.1) = P(Z > 2) = 1 - 0.9772 = 0.0228$

---

**Example 35 (GATE Style):** Sum of 36 i.i.d. exponential RVs with $\lambda = 1$ (mean 1, variance 1). $P(S_{36} > 40)$?

By CLT: $S_{36} \approx N(36, 36)$, so $\text{SD} = 6$.

$Z = \frac{40 - 36}{6} = \frac{2}{3} \approx 0.667$

$P(S_{36} > 40) = P(Z > 0.667) \approx 1 - 0.7475 = 0.2525$

---

## ⭐ 3.3 Confidence Intervals

### Concept

A **confidence interval** gives a range of plausible values for a population parameter.

A $100(1-\alpha)\%$ CI means: if we repeat the experiment many times, about $100(1-\alpha)\%$ of the constructed intervals will contain the true parameter.

### CI for Mean ($\sigma$ known)

$$\bar{X} \pm z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}}$$

| Confidence Level | $z_{\alpha/2}$ |
|---|---|
| 90% | 1.645 |
| 95% | 1.96 |
| 99% | 2.576 |

### CI for Mean ($\sigma$ unknown, use $S$)

$$\bar{X} \pm t_{\alpha/2, n-1} \cdot \frac{S}{\sqrt{n}}$$

Use the **t-distribution** with $n-1$ degrees of freedom.

### Sample Size Determination

To achieve margin of error $E$ at confidence level $1-\alpha$:

$$n = \left(\frac{z_{\alpha/2} \cdot \sigma}{E}\right)^2$$

---

**Example 36:** Sample of 49, $\bar{x} = 50$, $\sigma = 7$. Find 95% CI.

$50 \pm 1.96 \times \frac{7}{\sqrt{49}} = 50 \pm 1.96 \times 1 = (48.04, 51.96)$

---

**Example 37 (GATE Style):** How large a sample for margin of error 2 at 99% confidence if $\sigma = 10$?

$n = \left(\frac{2.576 \times 10}{2}\right)^2 = (12.88)^2 = 165.89 \implies n = 166$

---

## 3.4 t-Distribution

### When to Use

Use $t$-distribution when:
- Population variance $\sigma^2$ is **unknown**
- Sample size is small ($n < 30$)
- Population is approximately normal

### Properties

- Symmetric, bell-shaped, centered at 0
- Heavier tails than Normal (more probability in tails)
- As degrees of freedom $\nu \to \infty$, $t_\nu \to N(0,1)$
- $\nu = n - 1$ for one-sample problems

$$T = \frac{\bar{X} - \mu}{S / \sqrt{n}} \sim t_{n-1}$$

---

## ⭐⭐ 3.5 Hypothesis Testing

### Framework

| Step | Description |
|---|---|
| 1 | State null hypothesis $H_0$ and alternative $H_1$ |
| 2 | Choose significance level $\alpha$ (usually 0.05) |
| 3 | Compute test statistic |
| 4 | Find critical value or p-value |
| 5 | Make decision: reject $H_0$ or fail to reject |

### Types of Tests

| Alternative $H_1$ | Type | Reject $H_0$ if |
|---|---|---|
| $\mu \neq \mu_0$ | Two-tailed | $|Z| > z_{\alpha/2}$ |
| $\mu > \mu_0$ | Right-tailed | $Z > z_\alpha$ |
| $\mu < \mu_0$ | Left-tailed | $Z < -z_\alpha$ |

### Z-Test (σ known)

$$Z = \frac{\bar{X} - \mu_0}{\sigma / \sqrt{n}}$$

### T-Test (σ unknown)

$$T = \frac{\bar{X} - \mu_0}{S / \sqrt{n}} \sim t_{n-1}$$

### ⭐ p-Value

The **p-value** is the probability of observing a test statistic as extreme as (or more extreme than) the observed value, assuming $H_0$ is true.

**Decision rule:** Reject $H_0$ if $p\text{-value} < \alpha$

**Interpretation:**
- Small p-value → strong evidence against $H_0$
- p-value is NOT the probability that $H_0$ is true!

---

**Example 38:** Claim: $\mu = 500$. Sample: $n = 36$, $\bar{x} = 510$, $\sigma = 30$. Test at $\alpha = 0.05$.

$H_0: \mu = 500$, $H_1: \mu \neq 500$ (two-tailed)

$Z = \frac{510 - 500}{30/6} = \frac{10}{5} = 2$

$p\text{-value} = 2 \times P(Z > 2) = 2 \times 0.0228 = 0.0456$

Since $0.0456 < 0.05$, **reject $H_0$**.

---

**Example 39 (GATE Style):** $n = 25$, $\bar{x} = 78$, $S = 10$. Test $H_0: \mu = 75$ vs $H_1: \mu > 75$ at $\alpha = 0.05$.

$T = \frac{78 - 75}{10/5} = \frac{3}{2} = 1.5$

$t_{0.05, 24} \approx 1.711$

Since $1.5 < 1.711$, **fail to reject $H_0$**.

---

## ⭐ 3.6 Type I and Type II Errors

| | $H_0$ True | $H_0$ False |
|---|---|---|
| **Reject $H_0$** | Type I Error ($\alpha$) | Correct (Power = $1 - \beta$) |
| **Fail to Reject $H_0$** | Correct | Type II Error ($\beta$) |

### Key Concepts

- **Type I Error ($\alpha$):** False positive — rejecting a true $H_0$
  - *Analogy:* Convicting an innocent person
  - Controlled by significance level $\alpha$
  
- **Type II Error ($\beta$):** False negative — failing to reject a false $H_0$
  - *Analogy:* Acquitting a guilty person
  
- **Power ($1 - \beta$):** Probability of correctly rejecting false $H_0$

### Trade-offs
- Decreasing $\alpha$ → increases $\beta$ (and vice versa)
- Increasing $n$ → decreases both $\alpha$ and $\beta$
- $\alpha$ is chosen by the researcher (typically 0.01, 0.05, or 0.10)

---

**Example 40:** A test has $\alpha = 0.05$. We test 1000 true null hypotheses. Expected false rejections?

$1000 \times 0.05 = 50$ false rejections.

---

## ⭐ 3.7 Chi-Squared Distribution

### Definition

If $Z_1, Z_2, \ldots, Z_k$ are i.i.d. $N(0,1)$, then:

$$\chi^2 = Z_1^2 + Z_2^2 + \cdots + Z_k^2 \sim \chi^2_k$$

### Properties
- $E[\chi^2_k] = k$
- $\text{Var}(\chi^2_k) = 2k$
- Right-skewed, non-negative
- Sum: $\chi^2_{k_1} + \chi^2_{k_2} = \chi^2_{k_1 + k_2}$ (independent)
- As $k \to \infty$, $\chi^2_k \to N(k, 2k)$ by CLT

### Relationship to Sample Variance

$$\frac{(n-1)S^2}{\sigma^2} \sim \chi^2_{n-1}$$

---

## ⭐ 3.8 Chi-Squared Test

### Goodness-of-Fit Test

Tests whether observed frequencies match expected frequencies.

$$\chi^2 = \sum_{i=1}^{k} \frac{(O_i - E_i)^2}{E_i}$$

where $O_i$ = observed frequency, $E_i$ = expected frequency.

**Degrees of freedom:** $k - 1$ (or $k - 1 - p$ if $p$ parameters estimated)

**Reject $H_0$ if** $\chi^2 > \chi^2_{\alpha, k-1}$

### Test of Independence

For a contingency table with $r$ rows and $c$ columns:

$$E_{ij} = \frac{(\text{Row } i \text{ total}) \times (\text{Column } j \text{ total})}{\text{Grand total}}$$

**Degrees of freedom:** $(r-1)(c-1)$

---

**Example 41:** A die is rolled 120 times. Results:

| Face | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|
| Observed | 25 | 17 | 15 | 23 | 24 | 16 |
| Expected | 20 | 20 | 20 | 20 | 20 | 20 |

$\chi^2 = \frac{(25-20)^2}{20} + \frac{(17-20)^2}{20} + \frac{(15-20)^2}{20} + \frac{(23-20)^2}{20} + \frac{(24-20)^2}{20} + \frac{(16-20)^2}{20}$

$= \frac{25 + 9 + 25 + 9 + 16 + 16}{20} = \frac{100}{20} = 5.0$

$df = 6 - 1 = 5$. Critical value at $\alpha = 0.05$: $\chi^2_{0.05, 5} = 11.07$

Since $5.0 < 11.07$, **fail to reject $H_0$** (die appears fair).

---

**Example 42 (GATE Style):** Test independence of gender and preference.

|  | Prefer A | Prefer B | Total |
|---|---|---|---|
| Male | 30 | 20 | 50 |
| Female | 10 | 40 | 50 |
| Total | 40 | 60 | 100 |

Expected values: $E_{11} = 50 \times 40/100 = 20$, $E_{12} = 30$, $E_{21} = 20$, $E_{22} = 30$

$\chi^2 = \frac{(30-20)^2}{20} + \frac{(20-30)^2}{30} + \frac{(10-20)^2}{20} + \frac{(40-30)^2}{30}$

$= \frac{100}{20} + \frac{100}{30} + \frac{100}{20} + \frac{100}{30} = 5 + 3.33 + 5 + 3.33 = 16.67$

$df = (2-1)(2-1) = 1$. $\chi^2_{0.05, 1} = 3.84$

Since $16.67 > 3.84$, **reject $H_0$** — gender and preference are NOT independent.

---

## Additional Important Formulas

### Moment Generating Function (MGF)

$$M_X(t) = E[e^{tX}]$$

Properties:
- $E[X^n] = M_X^{(n)}(0)$ (nth derivative at $t = 0$)
- If $X, Y$ independent: $M_{X+Y}(t) = M_X(t) \cdot M_Y(t)$

### MGFs of Common Distributions

| Distribution | MGF |
|---|---|
| Bernoulli($p$) | $1 - p + pe^t$ |
| Binomial($n,p$) | $(1-p+pe^t)^n$ |
| Poisson($\lambda$) | $e^{\lambda(e^t - 1)}$ |
| Normal($\mu, \sigma^2$) | $e^{\mu t + \sigma^2 t^2/2}$ |
| Exponential($\lambda$) | $\frac{\lambda}{\lambda - t}$ for $t < \lambda$ |

---

## ⭐ Common GATE Mistakes & Traps Summary

| Trap | Correct Approach |
|---|---|
| $P(A \cup B) = P(A) + P(B)$ | Only if mutually exclusive; otherwise subtract $P(A \cap B)$ |
| $\text{Var}(X+Y) = \text{Var}(X) + \text{Var}(Y)$ | Only if independent; add $2\text{Cov}(X,Y)$ otherwise |
| $\text{Cov} = 0 \implies$ independent | FALSE! Uncorrelated ≠ independent |
| Forgetting to standardize in Normal problems | Always convert to Z-score |
| Confusing $P(A|B)$ with $P(B|A)$ | Prosecution fallacy — use Bayes' theorem |
| Using $\sigma$ instead of $\sigma/\sqrt{n}$ | For sample mean, use standard error $\sigma/\sqrt{n}$ |
| p-value = $P(H_0 \text{ is true})$ | FALSE! p-value = probability of data given $H_0$ |
| Type I/II confusion | Type I = reject true $H_0$; Type II = accept false $H_0$ |
| Geometric distribution definition | Check if starting from 0 or 1 |
| Chi-square: using wrong df | Goodness-of-fit: $k-1$; Independence: $(r-1)(c-1)$ |

---

## Frequently Asked GATE Topic Patterns

1. **Bayes' theorem** with defective items from machines ⭐⭐⭐
2. **Expectation** using linearity (e.g., indicator variables) ⭐⭐⭐
3. **Normal distribution** standardization problems ⭐⭐
4. **Conditional probability** with balls in urns ⭐⭐
5. **Variance** of linear combinations ⭐⭐
6. **CLT** application for sums/averages ⭐⭐
7. **Binomial/Poisson** probability calculations ⭐⭐
8. **Confidence interval** construction ⭐
9. **Hypothesis testing** decision ⭐
10. **Chi-squared test** statistic computation ⭐

---

> **Study Plan:** Master Modules 1–2 first (they carry most weight), then Module 3. Practice Bayes' theorem and Expectation problems exhaustively — these appear almost every year.

---

# MODULE 4: Advanced Probability & Stochastic Processes

---

## 4.1 Markov Chains ⛓️

> 💡 **Analogy:** Imagine a frog hopping between lily pads. The next pad depends ONLY on the current pad — not where the frog was 5 hops ago. That's the Markov property!

### Definition
A **Markov chain** is a sequence of random variables $X_0, X_1, X_2, \ldots$ where:
$$P(X_{n+1} = j \mid X_n = i, X_{n-1} = i_{n-1}, \ldots) = P(X_{n+1} = j \mid X_n = i) = P_{ij}$$

This is the **memoryless property** — the future depends only on the present.

### Transition Matrix
$$P = \begin{pmatrix} P_{00} & P_{01} & \cdots \\ P_{10} & P_{11} & \cdots \\ \vdots & \vdots & \ddots \end{pmatrix}$$

- Each row sums to 1 (it's a **stochastic matrix**)
- $P^n$ gives the $n$-step transition probabilities
- $P(X_n = j \mid X_0 = i) = (P^n)_{ij}$

### State Classification
| Property | Definition |
|---|---|
| **Accessible** | State $j$ accessible from $i$ if $(P^n)_{ij} > 0$ for some $n$ |
| **Communicate** | $i \leftrightarrow j$ if $i \to j$ and $j \to i$ |
| **Irreducible** | All states communicate with each other |
| **Absorbing** | State $i$ with $P_{ii} = 1$ (once entered, never leave) |
| **Recurrent** | Return to state $i$ with probability 1 |
| **Transient** | Return probability $< 1$ (might never return) |
| **Period** | $d(i) = \gcd\{n : P_{ii}^{(n)} > 0\}$; aperiodic if $d = 1$ |
| **Ergodic** | Irreducible + aperiodic + positive recurrent |

### Stationary Distribution $\pi$
If $\pi P = \pi$ and $\sum \pi_i = 1$, then $\pi$ is the **stationary distribution**.

For ergodic chains: $\lim_{n\to\infty} P^n$ converges, each row = $\pi$.

**Example:** Weather model — sunny/rainy with transition:
$$P = \begin{pmatrix} 0.8 & 0.2 \\ 0.4 & 0.6 \end{pmatrix}$$
Stationary: $\pi_S(0.8) + \pi_R(0.4) = \pi_S$, $\pi_S + \pi_R = 1$
→ $0.2\pi_S = 0.4\pi_R$ → $\pi_S = 2\pi_R$ → $\pi_S = 2/3$, $\pi_R = 1/3$
→ Long-run: sunny 67% of the time!

### Expected Return Time
If stationary distribution $\pi_i$ exists: $E[\text{return to } i] = 1/\pi_i$

### Absorbing Chains
- **Absorption probabilities**: solve system $a_i = \sum_j P_{ij} a_j$ with boundary conditions
- **Expected absorption time**: $t_i = 1 + \sum_{j \text{ non-absorbing}} P_{ij} t_j$

---

## 4.2 Poisson Process 📊

> 💡 Events arriving randomly in continuous time — calls to a helpline, buses at a stop, decay of atoms.

### Definition
$\{N(t), t \geq 0\}$ is a Poisson process with rate $\lambda$ if:
1. $N(0) = 0$
2. Independent increments
3. $N(t+s) - N(s) \sim \text{Poisson}(\lambda t)$

### Key Properties
| Property | Formula |
|---|---|
| $P(N(t) = k)$ | $\frac{(\lambda t)^k e^{-\lambda t}}{k!}$ |
| $E[N(t)]$ | $\lambda t$ |
| $\text{Var}(N(t))$ | $\lambda t$ |
| Inter-arrival times $T_i$ | $\text{Exp}(\lambda)$, i.i.d. |
| Arrival time $S_n = T_1 + \cdots + T_n$ | $\text{Gamma}(n, \lambda)$ |
| Merging: rates $\lambda_1 + \lambda_2$ | Poisson($\lambda_1 + \lambda_2$) |
| Splitting: each event type A w.p. $p$ | $N_A \sim \text{Poisson}(\lambda p)$, $N_B \sim \text{Poisson}(\lambda(1-p))$ |

### Conditional Arrival Times
Given $N(t) = 1$: the arrival time $S_1 \sim \text{Uniform}(0, t)$.
Given $N(t) = n$: the arrivals are like $n$ uniform points on $(0,t)$, order statistics!

---

## 4.3 Generating Functions & Transforms

### Probability Generating Function (PGF)
For non-negative integer RV $X$: $G_X(z) = E[z^X] = \sum_{k=0}^{\infty} P(X=k) z^k$

| Distribution | PGF |
|---|---|
| Bernoulli($p$) | $q + pz$ |
| Binomial($n,p$) | $(q + pz)^n$ |
| Poisson($\lambda$) | $e^{\lambda(z-1)}$ |
| Geometric($p$) | $\frac{pz}{1-qz}$ |

**Key properties:** $G'(1) = E[X]$, $G''(1) = E[X(X-1)]$

### Moment Generating Function (MGF)
$M_X(t) = E[e^{tX}]$

| Distribution | MGF |
|---|---|
| Bernoulli($p$) | $1-p+pe^t$ |
| Binomial($n,p$) | $(1-p+pe^t)^n$ |
| Poisson($\lambda$) | $e^{\lambda(e^t-1)}$ |
| Normal($\mu,\sigma^2$) | $e^{\mu t + \sigma^2t^2/2}$ |
| Exponential($\lambda$) | $\frac{\lambda}{\lambda-t}$, $t < \lambda$ |
| Gamma($\alpha,\beta$) | $\left(\frac{\beta}{\beta-t}\right)^\alpha$, $t < \beta$ |

**Why MGF matters:**
- $M^{(n)}(0) = E[X^n]$ — gets all moments!
- Uniqueness: if $M_X(t) = M_Y(t)$ in neighborhood of 0, then $X \stackrel{d}{=} Y$
- Sum of independent: $M_{X+Y}(t) = M_X(t) \cdot M_Y(t)$

---

## 4.4 Order Statistics

If $X_1, \ldots, X_n$ are i.i.d. with CDF $F$ and PDF $f$:

- **Min:** $X_{(1)}$ has PDF $f_{(1)}(x) = n[1-F(x)]^{n-1}f(x)$
- **Max:** $X_{(n)}$ has PDF $f_{(n)}(x) = n[F(x)]^{n-1}f(x)$
- **$k$th order statistic:** $f_{(k)}(x) = \frac{n!}{(k-1)!(n-k)!}[F(x)]^{k-1}[1-F(x)]^{n-k}f(x)$

**Example:** $X_1, \ldots, X_n \sim \text{Uniform}(0,1)$.
- $E[X_{(k)}] = k/(n+1)$
- $E[\max] = n/(n+1)$
- $E[\min] = 1/(n+1)$

---

## 4.5 Inequalities 🔔

| Inequality | Statement | When to Use |
|---|---|---|
| **Markov** | $P(X \geq a) \leq \frac{E[X]}{a}$ for $X \geq 0$ | Loose bound, uses only mean |
| **Chebyshev** | $P(|X-\mu| \geq k\sigma) \leq \frac{1}{k^2}$ | Uses mean + variance |
| **Chernoff** | $P(X \geq a) \leq \inf_t \frac{M_X(t)}{e^{at}}$ | Exponentially tight |
| **Hoeffding** | $P(\bar{X}-\mu \geq t) \leq e^{-2nt^2/(b-a)^2}$ | Bounded RVs |
| **Jensen's** | $f(E[X]) \leq E[f(X)]$ for convex $f$ | Convex functions |
| **Cauchy-Schwarz** | $|E[XY]|^2 \leq E[X^2]E[Y^2]$ | Bounding correlations |

**Example (Chebyshev):** $X$ has $\mu = 50$, $\sigma^2 = 25$. Bound $P(|X-50| \geq 15)$.
→ $k = 15/5 = 3$ → $P \leq 1/9 \approx 0.111$

---

## 4.6 Convergence Concepts

| Type | Definition | Notation |
|---|---|---|
| **Sure convergence** | $X_n(\omega) \to X(\omega)$ for all $\omega$ | $X_n \to X$ surely |
| **Almost sure** | $P(\lim X_n = X) = 1$ | $X_n \xrightarrow{a.s.} X$ |
| **In probability** | $P(|X_n - X| > \epsilon) \to 0$ for all $\epsilon$ | $X_n \xrightarrow{P} X$ |
| **In distribution** | $F_{X_n}(x) \to F_X(x)$ at continuity points | $X_n \xrightarrow{d} X$ |
| **In $L^p$** | $E[|X_n - X|^p] \to 0$ | $X_n \xrightarrow{L^p} X$ |

**Hierarchy:** a.s. → in probability → in distribution. Also $L^p \to$ in probability.

### Laws of Large Numbers
**Weak LLN (WLLN):** $\bar{X}_n \xrightarrow{P} \mu$
**Strong LLN (SLLN):** $\bar{X}_n \xrightarrow{a.s.} \mu$

Both say: sample average converges to population mean. Strong version is... stronger.

---

## 4.7 Maximum Likelihood Estimation (MLE) 📐

> 💡 **Analogy:** Given the data you observed, which parameter value makes this data MOST LIKELY?

### Procedure
1. Write likelihood: $L(\theta) = \prod_{i=1}^n f(x_i; \theta)$
2. Take log-likelihood: $\ell(\theta) = \sum \ln f(x_i; \theta)$
3. Differentiate: $\frac{d\ell}{d\theta} = 0$
4. Solve for $\hat{\theta}_{MLE}$
5. Verify: $\frac{d^2\ell}{d\theta^2} < 0$ (maximum, not minimum)

### Common MLEs

| Distribution | Parameter | MLE |
|---|---|---|
| Bernoulli($p$) | $p$ | $\hat{p} = \bar{X}$ (sample proportion) |
| Poisson($\lambda$) | $\lambda$ | $\hat{\lambda} = \bar{X}$ |
| Exponential($\lambda$) | $\lambda$ | $\hat{\lambda} = 1/\bar{X}$ |
| Normal($\mu, \sigma^2$) | $\mu$ | $\hat{\mu} = \bar{X}$ |
| Normal($\mu, \sigma^2$) | $\sigma^2$ | $\hat{\sigma}^2 = \frac{1}{n}\sum(X_i - \bar{X})^2$ (biased!) |
| Uniform($0, \theta$) | $\theta$ | $\hat{\theta} = X_{(n)}$ (max) |

### Properties of MLE
- **Consistent:** $\hat{\theta} \xrightarrow{P} \theta$
- **Asymptotically normal:** $\sqrt{n}(\hat{\theta} - \theta) \xrightarrow{d} N(0, 1/I(\theta))$
- **Asymptotically efficient:** achieves Cramér-Rao bound
- **Invariant:** MLE of $g(\theta)$ is $g(\hat{\theta})$

### Fisher Information
$$I(\theta) = -E\left[\frac{\partial^2}{\partial\theta^2}\ln f(X;\theta)\right] = E\left[\left(\frac{\partial}{\partial\theta}\ln f(X;\theta)\right)^2\right]$$

**Cramér-Rao Lower Bound:** $\text{Var}(\hat{\theta}) \geq \frac{1}{nI(\theta)}$

---

## 4.8 Bayesian Inference 🔔

### Setup
- **Prior:** $\pi(\theta)$ — belief before seeing data
- **Likelihood:** $f(x|\theta)$ — probability of data given parameter
- **Posterior:** $\pi(\theta|x) \propto f(x|\theta) \cdot \pi(\theta)$

### Conjugate Priors

| Likelihood | Conjugate Prior | Posterior |
|---|---|---|
| Bernoulli/Binomial | Beta($\alpha, \beta$) | Beta($\alpha + x, \beta + n - x$) |
| Poisson | Gamma($\alpha, \beta$) | Gamma($\alpha + \sum x_i, \beta + n$) |
| Normal (known $\sigma^2$) | Normal($\mu_0, \sigma_0^2$) | Normal($\mu_n, \sigma_n^2$) weighted avg |
| Exponential | Gamma($\alpha, \beta$) | Gamma($\alpha + n, \beta + \sum x_i$) |

**Why conjugate priors?** Posterior is same family as prior → easy to compute!

### MAP Estimate
$\hat{\theta}_{MAP} = \arg\max_\theta \pi(\theta|x) = \arg\max_\theta [\ln f(x|\theta) + \ln \pi(\theta)]$

Connection to regularization: MAP with Gaussian prior = ridge regression!

---

## 4.9 Sufficient Statistics

### Definition
$T(X)$ is **sufficient** for $\theta$ if the conditional distribution of $X$ given $T(X)$ doesn't depend on $\theta$.

### Fisher-Neyman Factorization
$T$ is sufficient iff $f(x;\theta) = g(T(x), \theta) \cdot h(x)$

### Common Sufficient Statistics

| Distribution | Sufficient Statistic |
|---|---|
| Bernoulli | $\sum X_i$ |
| Poisson | $\sum X_i$ |
| Normal (both unknown) | $(\sum X_i, \sum X_i^2)$ |
| Exponential | $\sum X_i$ |
| Uniform($0, \theta$) | $X_{(n)}$ (max) |

---

# MODULE 5: Distributions Deep Dive

---

## 5.1 Gamma Distribution

$$f(x; \alpha, \beta) = \frac{\beta^\alpha}{\Gamma(\alpha)} x^{\alpha-1} e^{-\beta x}, \quad x > 0$$

| Property | Formula |
|---|---|
| Mean | $\alpha/\beta$ |
| Variance | $\alpha/\beta^2$ |
| MGF | $(\beta/(\beta-t))^\alpha$ |
| Sum of Exponentials | $\text{Gamma}(n, \lambda)$ = sum of $n$ i.i.d. $\text{Exp}(\lambda)$ |

Special cases: $\alpha = 1$ → Exponential; $\alpha = n/2, \beta = 1/2$ → Chi-squared($n$)

---

## 5.2 Beta Distribution

$$f(x; \alpha, \beta) = \frac{x^{\alpha-1}(1-x)^{\beta-1}}{B(\alpha,\beta)}, \quad x \in (0,1)$$

| Property | Formula |
|---|---|
| Mean | $\frac{\alpha}{\alpha+\beta}$ |
| Variance | $\frac{\alpha\beta}{(\alpha+\beta)^2(\alpha+\beta+1)}$ |
| Mode (if $\alpha,\beta > 1$) | $\frac{\alpha-1}{\alpha+\beta-2}$ |
| Special | $B(1,1) = \text{Uniform}(0,1)$ |

Used for: modeling probabilities, Bayesian inference (conjugate prior for Bernoulli).

---

## 5.3 Multivariate Normal Distribution

$$\mathbf{X} \sim N(\boldsymbol{\mu}, \boldsymbol{\Sigma})$$

$$f(\mathbf{x}) = \frac{1}{(2\pi)^{n/2}|\Sigma|^{1/2}} \exp\left(-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^T \Sigma^{-1}(\mathbf{x}-\boldsymbol{\mu})\right)$$

### Key Properties
- Marginals are normal: $X_i \sim N(\mu_i, \Sigma_{ii})$
- Conditionals are normal: $X_1 | X_2 \sim N(\mu_{1|2}, \Sigma_{1|2})$
- Linear transformation: $AX + b \sim N(A\mu + b, A\Sigma A^T)$
- Uncorrelated ↔ Independent (ONLY for normal!)
- Mahalanobis distance: $d^2 = (x-\mu)^T\Sigma^{-1}(x-\mu) \sim \chi^2_n$

---

## 5.4 Log-Normal Distribution

If $X \sim N(\mu, \sigma^2)$, then $Y = e^X \sim \text{LogNormal}(\mu, \sigma^2)$.

| Property | Formula |
|---|---|
| Mean | $e^{\mu + \sigma^2/2}$ |
| Variance | $e^{2\mu+\sigma^2}(e^{\sigma^2}-1)$ |
| Median | $e^\mu$ |
| Mode | $e^{\mu-\sigma^2}$ |

Used for: stock prices, income distributions, file sizes.

---

## 5.5 Negative Binomial Distribution

Number of trials to get $r$th success.

$$P(X = k) = \binom{k-1}{r-1} p^r (1-p)^{k-r}, \quad k = r, r+1, \ldots$$

| Property | Formula |
|---|---|
| Mean | $r/p$ |
| Variance | $r(1-p)/p^2$ |
| Special ($r=1$) | Geometric distribution |

---

## 5.6 Hypergeometric Distribution

Drawing without replacement from a finite population.

$$P(X = k) = \frac{\binom{K}{k}\binom{N-K}{n-k}}{\binom{N}{n}}$$

Where $N$ = population size, $K$ = success states, $n$ = draws, $k$ = observed successes.

| Property | Formula |
|---|---|
| Mean | $nK/N$ |
| Variance | $n\frac{K}{N}\frac{N-K}{N}\frac{N-n}{N-1}$ |

Note: For large $N$, Hypergeometric $\approx$ Binomial (sampling with/without replacement becomes similar).

---

## 5.7 Distribution Relationships Map

```
                    Bernoulli(p)
                        │
                    n trials
                        │
                  Binomial(n,p)
                   /         \
            n→∞,p→0        n large
            np=λ              │
                /         Normal (CLT)
          Poisson(λ)        ▲
              │           anything
         time axis        sum of many
              │              │
       Poisson Process    CLT applies
              │
       inter-arrival      Geometric(p)
              │               │
        Exponential(λ)     r successes
              │               │
         sum of n        Neg Binomial(r,p)
              │
        Gamma(n,λ)  ←── α=n/2,β=1/2 ──→ Chi-squared(n)
              │
         β→∞, →        student-t (via χ²/n)
              │
     for proportions
              │
        Beta(α,β)  ←── α=β=1 ──→ Uniform(0,1)
```

---

# MODULE 6: Information Theory & Entropy

---

## 6.1 Shannon Entropy

$$H(X) = -\sum_{i} p_i \log_2 p_i$$

| Property | Formula/Value |
|---|---|
| $H(X) \geq 0$ | Always non-negative |
| Max entropy (discrete) | $\log_2 n$ (uniform distribution) |
| For fair coin | $H = -2 \cdot 0.5\log_2 0.5 = 1$ bit |
| Binary entropy | $H(p) = -p\log p - (1-p)\log(1-p)$ |

### Joint and Conditional Entropy
- Joint: $H(X,Y) = -\sum_{x,y} p(x,y)\log p(x,y)$
- Conditional: $H(Y|X) = H(X,Y) - H(X)$
- Chain rule: $H(X,Y) = H(X) + H(Y|X)$

### Mutual Information
$$I(X;Y) = H(X) + H(Y) - H(X,Y) = H(X) - H(X|Y) = H(Y) - H(Y|X)$$

- $I(X;Y) \geq 0$ (always!)
- $I(X;Y) = 0 \Leftrightarrow$ $X, Y$ independent
- $I(X;X) = H(X)$

### KL Divergence
$$D_{KL}(P\|Q) = \sum_x P(x)\log\frac{P(x)}{Q(x)} \geq 0$$

- NOT symmetric: $D_{KL}(P\|Q) \neq D_{KL}(Q\|P)$
- NOT a metric (no triangle inequality)
- $= 0$ iff $P = Q$
- Cross-entropy: $H(P,Q) = H(P) + D_{KL}(P\|Q) = -\sum P(x)\log Q(x)$

---

# 📝 Practice Set 1: Fundamentals (P1 – P80)

**P1.** (1-mark) Three coins tossed. $P(\text{exactly 2 heads})$?
→ $\binom{3}{2}/2^3 = 3/8$

**P2.** (1-mark) Deck of 52 cards. $P(\text{red card or face card})$?
→ Red: 26, Face: 12, Red ∩ Face: 6 → $(26+12-6)/52 = 32/52 = 8/13$

**P3.** (1-mark) $P(A) = 0.4$, $P(B) = 0.3$, $P(A \cap B) = 0.1$. $P(A | B)$?
→ $0.1/0.3 = 1/3$

**P4.** (2-mark) Urn: 5 red, 3 blue. Draw 2 without replacement. $P(\text{both red})$?
→ $\frac{5}{8} \cdot \frac{4}{7} = 20/56 = 5/14$

**P5.** (2-mark GATE) Machine A: 60% of products, 2% defective. Machine B: 40%, 3% defective. Random product is defective. $P(\text{from A})$?
→ Bayes: $\frac{0.6 \times 0.02}{0.6 \times 0.02 + 0.4 \times 0.03} = \frac{0.012}{0.012 + 0.012} = \frac{0.012}{0.024} = 1/2$

**P6.** (2-mark) COVID test: sensitivity 95% (P(+|disease) = 0.95), specificity 90% (P(-|no disease) = 0.90), prevalence 1%. Given positive test, P(disease)?
→ $\frac{0.01 \times 0.95}{0.01 \times 0.95 + 0.99 \times 0.10} = \frac{0.0095}{0.0095 + 0.099} = \frac{0.0095}{0.1085} \approx 0.0876 \approx 8.8\%$
→ Even with positive test, only ~9% chance of disease! (Base rate fallacy)

**P7.** (1-mark) $X \sim \text{Binomial}(10, 0.3)$. $E[X]$ and $\text{Var}(X)$?
→ $E[X] = 10 \times 0.3 = 3$, $\text{Var} = 10 \times 0.3 \times 0.7 = 2.1$

**P8.** (2-mark) $X \sim \text{Poisson}(4)$. $P(X \geq 2)$?
→ $1 - P(X=0) - P(X=1) = 1 - e^{-4} - 4e^{-4} = 1 - 5e^{-4} \approx 1 - 0.0916 = 0.9084$

**P9.** (2-mark) $X \sim N(100, 25)$. $P(X > 110)$?
→ $Z = (110-100)/5 = 2$. $P(Z > 2) = 1 - \Phi(2) = 1 - 0.9772 = 0.0228$

**P10.** (2-mark) $X \sim \text{Exp}(\lambda = 0.5)$. $P(X > 3 | X > 1)$?
→ Memoryless: $P(X > 3 | X > 1) = P(X > 2) = e^{-1} \approx 0.3679$

**P11-P30** — Quick Fire:

| # | Q | A |
|---|---|---|
| P11 | $P(A \cup B)$ if ME | $P(A) + P(B)$ |
| P12 | $P(A^c)$ if $P(A) = 0.7$ | $0.3$ |
| P13 | $E[3X + 5]$ if $E[X] = 4$ | $17$ |
| P14 | $\text{Var}(2X + 3)$ if $\text{Var}(X) = 9$ | $36$ |
| P15 | $E[X^2]$ if $\mu = 3$, $\sigma^2 = 4$ | $E[X^2] = \sigma^2 + \mu^2 = 4+9 = 13$ |
| P16 | Fair die: $E[X]$ | $7/2 = 3.5$ |
| P17 | Fair die: $\text{Var}(X)$ | $35/12 \approx 2.917$ |
| P18 | Geometric($p=1/6$): $E[X]$ | $6$ (expected tosses for first 6) |
| P19 | $X,Y$ independent. $\text{Cov}(X,Y)$? | $0$ |
| P20 | $\text{Cov}(X,X)$? | $\text{Var}(X)$ |
| P21 | $\text{Cov}(aX, bY) = ?$ | $ab\text{Cov}(X,Y)$ |
| P22 | Bernoulli($0.6$): $\text{Var}$? | $0.6 \times 0.4 = 0.24$ |
| P23 | $X \sim \text{Uniform}(0, 10)$: $E[X]$? | $5$ |
| P24 | Same: $\text{Var}(X)$? | $(10-0)^2/12 = 100/12 = 25/3$ |
| P25 | $P(Z < 1.96)$ for standard normal? | $0.975$ |
| P26 | $P(-1.96 < Z < 1.96)$? | $0.95$ |
| P27 | 95% CI half-width formula? | $z_{0.025} \cdot \sigma/\sqrt{n} = 1.96\sigma/\sqrt{n}$ |
| P28 | Chi-squared with $k$ df: $E$? $\text{Var}$? | $E = k$, $\text{Var} = 2k$ |
| P29 | Student-t: heavier tails than normal? | YES! Especially for small $n$ |
| P30 | $t$ as $n \to \infty$? | Approaches standard normal |

**P31-P50** — Bayes & Conditional Probability Workout:

**P31.** (2-mark) Bag 1: 4W, 6B. Bag 2: 7W, 3B. Fair coin: Head → Bag 1, Tail → Bag 2. Draw is White. P(Bag 1)?
→ $\frac{0.5 \times 0.4}{0.5 \times 0.4 + 0.5 \times 0.7} = \frac{0.2}{0.2+0.35} = \frac{0.2}{0.55} = 4/11$

**P32.** (2-mark) Three factories A(50%), B(30%), C(20%) produce items. Defective rates: 3%, 4%, 5%. Item defective. P(from C)?
→ $\frac{0.2 \times 0.05}{0.5 \times 0.03 + 0.3 \times 0.04 + 0.2 \times 0.05} = \frac{0.01}{0.015+0.012+0.01} = \frac{0.01}{0.037} \approx 0.2703$

**P33.** (2-mark) Student knows answer (prob 0.7) or guesses (prob 0.3). MCQ with 4 options. Correct answer given. P(knew)?
→ $\frac{0.7 \times 1}{0.7 \times 1 + 0.3 \times 0.25} = \frac{0.7}{0.7+0.075} = \frac{0.7}{0.775} = 28/31 \approx 0.903$

**P34.** (2-mark) Two cards from 52 without replacement. P(both aces)?
→ $\frac{4}{52}\cdot\frac{3}{51} = \frac{12}{2652} = \frac{1}{221}$

**P35.** (2-mark) Birthday problem: P(at least 2 share birthday in group of 23)?
→ $P(\text{all different}) = \frac{365}{365}\cdot\frac{364}{365}\cdots\frac{343}{365} \approx 0.493$
→ P(match) $\approx 0.507 > 50\%$!

**P36-P50** — More Quick Problems:

| # | Q | A |
|---|---|---|
| P36 | 10 Q MCQ, 4 options, all random. E[correct]? | $10 \times 1/4 = 2.5$ |
| P37 | P(at least 1 correct in P36)? | $1-(3/4)^{10} \approx 0.9437$ |
| P38 | Monty Hall: switch or stay? | Switch! (2/3 vs 1/3) |
| P39 | P(full house in poker)? | $\frac{\binom{13}{1}\binom{4}{3}\binom{12}{1}\binom{4}{2}}{\binom{52}{5}} = \frac{3744}{2598960} \approx 0.00144$ |
| P40 | E[|X|] for $X \sim N(0,1)$? | $\sqrt{2/\pi} \approx 0.7979$ |
| P41 | Min of 2 independent Exp($\lambda$)? | Exp($2\lambda$) |
| P42 | Max of 2 independent Uniform(0,1)? | CDF $= x^2$, PDF $= 2x$ |
| P43 | $X \sim \text{Geo}(p)$. P(X is even)? | $\frac{1-p}{2-p}$ |
| P44 | E[X²] for Poisson($\lambda$)? | $\lambda + \lambda^2$ (since $\text{Var} = \lambda = E[X^2]-\mu^2$) |
| P45 | Indicator: $E[I_A]$? | $P(A)$ |
| P46 | Var of indicator $I_A$? | $P(A)(1-P(A))$ |
| P47 | Sum of indicators → E[sum]? | Sum of probabilities (linearity!) |
| P48 | 100 people, hats shuffled. E[people with own hat]? | $100 \times 1/100 = 1$ |
| P49 | $X,Y$ jointly normal, $\rho = 0$. Independent? | YES (only for normal!) |
| P50 | MGF of $X+Y$ (independent)? | $M_X(t) \cdot M_Y(t)$ |

**P51-P80** — Distributions Practice:

**P51.** (2-mark) $X \sim \text{Binomial}(100, 0.05)$. Approximate $P(X = 3)$ using Poisson.
→ $\lambda = np = 5$. $P(X=3) = \frac{5^3 e^{-5}}{3!} = \frac{125 \times 0.00674}{6} = 0.1404$

**P52.** (2-mark) Arrivals follow Poisson process rate 3/hour. P(at least 1 in 30 min)?
→ $\lambda t = 3 \times 0.5 = 1.5$. $P(X \geq 1) = 1 - e^{-1.5} = 1 - 0.2231 = 0.7769$

**P53.** (2-mark) $X_1, \ldots, X_{25}$ i.i.d. with $\mu = 50$, $\sigma^2 = 100$. Using CLT, $P(\bar{X} > 54)$?
→ $\bar{X} \approx N(50, 100/25) = N(50, 4)$. $Z = (54-50)/2 = 2$. $P(Z > 2) = 0.0228$

**P54.** (2-mark) $X$ has PDF $f(x) = 2x$ on $[0,1]$. Find $E[X]$ and $\text{Var}(X)$.
→ $E[X] = \int_0^1 2x^2 dx = 2/3$. $E[X^2] = \int_0^1 2x^3 dx = 1/2$.
→ $\text{Var}(X) = 1/2 - 4/9 = 1/18$

**P55.** (2-mark) $X_1, X_2$ independent Exp(1). PDF of $Y = X_1 + X_2$?
→ $Y \sim \text{Gamma}(2, 1)$: $f_Y(y) = ye^{-y}$, $y > 0$

**P56.** (2-mark) $X \sim N(0,1)$. $E[X^4]$?
→ Standard normal moments: $E[X^{2k}] = (2k-1)!! = (2k)!/(2^k k!)$
→ $E[X^4] = 3!! = 3$. Or: $E[X^4] = 3\sigma^4 = 3$ for standard normal.

**P57.** (2-mark) MLE for Uniform(0, $\theta$) from sample $x_1, \ldots, x_n$.
→ $L(\theta) = \frac{1}{\theta^n} \cdot \prod I(0 \leq x_i \leq \theta) = \frac{1}{\theta^n}$ for $\theta \geq \max(x_i)$
→ $L$ decreasing in $\theta$ for $\theta \geq x_{(n)}$ → MLE = $\hat{\theta} = x_{(n)}$ (sample maximum)

**P58.** (2-mark) 99% CI for mean: $n = 100$, $\bar{x} = 75$, $\sigma = 12$.
→ $z_{0.005} = 2.576$. CI: $75 \pm 2.576 \times 12/10 = 75 \pm 3.09 = (71.91, 78.09)$

**P59.** (2-mark) Hypothesis test: $H_0: \mu = 100$, $H_1: \mu > 100$. $n = 36$, $\bar{x} = 105$, $\sigma = 18$. $\alpha = 0.05$.
→ $Z = (105-100)/(18/6) = 5/3 \approx 1.667$. Critical: $z_{0.05} = 1.645$. Since $1.667 > 1.645$: **Reject $H_0$**.

**P60.** (2-mark) Chi-squared goodness of fit: die rolled 60 times. Observed: 8,12,10,11,7,12. Expected: 10 each.
→ $\chi^2 = \sum(O_i-E_i)^2/E_i = \frac{(-2)^2+2^2+0^2+1^2+(-3)^2+2^2}{10} = \frac{4+4+0+1+9+4}{10} = 2.2$
→ $\chi^2_{0.05, 5} = 11.07$. Since $2.2 < 11.07$: fail to reject $H_0$ (die is fair).

**P61-P80** — Quick Table:

| # | Q | A |
|---|---|---|
| P61 | Entropy of fair coin (bits)? | $1$ bit |
| P62 | Entropy of biased coin ($p = 0.9$)? | $-0.9\log_2 0.9 - 0.1\log_2 0.1 \approx 0.469$ bits |
| P63 | Max entropy discrete dist on $\{1,...,n\}$? | Uniform |
| P64 | Max entropy continuous on $(-\infty,\infty)$ given $\mu,\sigma^2$? | Normal |
| P65 | $I(X;Y) = 0$ means? | Independent |
| P66 | KL divergence symmetric? | NO! |
| P67 | Cross-entropy $H(P,Q) = ?$ | $H(P) + D_{KL}(P\|Q)$ |
| P68 | Markov chain: rows of $P$ sum to? | $1$ |
| P69 | Stationary dist: $\pi P = ?$ | $\pi$ |
| P70 | Period of self-loop state? | $1$ (aperiodic) |
| P71 | Absorption probability: solve $a_i = ?$ | $\sum P_{ij}a_j$ + boundary |
| P72 | PGF of Binomial? | $(q+pz)^n$ |
| P73 | $G'(1)$ gives? | $E[X]$ |
| P74 | $\text{Var}(aX+bY)$ independent? | $a^2\text{Var}(X) + b^2\text{Var}(Y)$ |
| P75 | $\text{Var}(\bar{X})$ for i.i.d.? | $\sigma^2/n$ |
| P76 | Chebyshev: $P(\|X-\mu\| \geq 2\sigma) \leq ?$ | $1/4$ |
| P77 | Markov: $P(X \geq 5)$ if $E[X] = 2$? | $\leq 2/5$ |
| P78 | WLLN requires? | Finite variance (classically) |
| P79 | SLLN requires? | i.i.d. with finite mean |
| P80 | CLT requires? | i.i.d. with finite variance |

---

# 📝 Practice Set 2: GATE Simulation (P81 – P160)

**P81.** (2-mark GATE 2024 Style) A communication channel transmits 0 or 1. P(transmit 0) = 0.4. P(error) = 0.1 (independent of bit). P(received 1 | transmitted 0) = 0.1, P(received 0 | transmitted 1) = 0.1. Given received = 1, P(transmitted = 1)?
→ $P(T=1|R=1) = \frac{P(R=1|T=1)P(T=1)}{P(R=1)} = \frac{0.9 \times 0.6}{0.9 \times 0.6 + 0.1 \times 0.4} = \frac{0.54}{0.54+0.04} = \frac{0.54}{0.58} = 27/29$

**P82.** (2-mark) $X, Y$ jointly distributed. $E[X] = 3$, $E[Y] = 5$, $\text{Var}(X) = 4$, $\text{Var}(Y) = 9$, $\rho = -0.5$. Find $\text{Var}(2X - Y + 3)$.
→ $\text{Cov}(X,Y) = \rho\sigma_X\sigma_Y = -0.5 \times 2 \times 3 = -3$
→ $\text{Var}(2X-Y) = 4(4) + 1(9) - 2(2)(1)(-3) = 16 + 9 + 12 = 37$

**P83.** (2-mark NAT) $X$ takes values $0, 1, 2$ with probabilities $0.3, 0.5, 0.2$. Find $\text{Var}(X)$.
→ $E[X] = 0.3(0) + 0.5(1) + 0.2(2) = 0.9$
→ $E[X^2] = 0.3(0) + 0.5(1) + 0.2(4) = 1.3$
→ $\text{Var}(X) = 1.3 - 0.81 = 0.49$

**P84.** (2-mark) Two dice rolled. $X$ = sum. Find $P(X = 7)$ and $P(X = 7 | \text{at least one die shows 4})$.
→ $P(X=7) = 6/36 = 1/6$
→ $P(\text{at least one 4}) = 11/36$
→ Favorable: (3,4) and (4,3) → 2 outcomes
→ $P(X=7 | \text{at least one 4}) = 2/11$

**P85.** (2-mark) Coupon collector: $n$ types of coupons, equally likely. $E[\text{time to collect all}]$?
→ $E = n \cdot H_n = n\sum_{k=1}^n \frac{1}{k}$
→ For $n = 6$: $E = 6(1 + 1/2 + 1/3 + 1/4 + 1/5 + 1/6) = 6 \times 2.45 = 14.7$

**P86-P100** — Distribution Identification:

| # | Scenario | Distribution |
|---|---|---|
| P86 | Number of emails per hour | Poisson |
| P87 | Time between bus arrivals | Exponential |
| P88 | Number of defective items in batch of 50 | Binomial |
| P89 | Number of attempts until first success | Geometric |
| P90 | Proportion of voters supporting a candidate | Beta |
| P91 | Sum of squared standard normals | Chi-squared |
| P92 | Ratio: standard normal / sqrt(chi-sq/df) | Student-t |
| P93 | Time until 5th customer arrives | Gamma(5, λ) |
| P94 | Number of red balls in 10 drawn from urn (no replacement) | Hypergeometric |
| P95 | Height of adults | Normal |
| P96 | Income distribution | Log-Normal |
| P97 | Lifetime of electronic component | Exponential / Weibull |
| P98 | Number of typos per page | Poisson |
| P99 | Random point on a line segment | Uniform |
| P100 | Number of failures before 3rd success | Negative Binomial |

**P101-P120** — Expectation & Variance Calculations:

**P101.** (2-mark) Random variable $X$ with PDF $f(x) = 3x^2$ on $[0,1]$. Find CDF, mean, variance.
→ $F(x) = x^3$, $E[X] = \int_0^1 3x^3 dx = 3/4$, $E[X^2] = \int_0^1 3x^4 dx = 3/5$
→ $\text{Var}(X) = 3/5 - 9/16 = 48/80 - 45/80 = 3/80$

**P102.** (2-mark) $X_1, \ldots, X_n$ i.i.d. Exp($\lambda$). Distribution of $\bar{X}$?
→ $\sum X_i \sim \text{Gamma}(n, \lambda)$ → $\bar{X} \sim \text{Gamma}(n, n\lambda)$
→ By CLT for large $n$: $\bar{X} \approx N(1/\lambda, 1/(n\lambda^2))$

**P103.** (2-mark) $X \sim N(0,1)$. Find $E[\max(X, 0)]$.
→ $E[\max(X,0)] = \int_0^{\infty} x\phi(x)dx = \int_0^{\infty} \frac{x}{\sqrt{2\pi}}e^{-x^2/2}dx = \frac{1}{\sqrt{2\pi}}[-e^{-x^2/2}]_0^{\infty} = \frac{1}{\sqrt{2\pi}}$

This is the ReLU activation expected value!

**P104-P120** — Quick Table:

| # | Q | A |
|---|---|---|
| P104 | $E[\min(X,Y)]$ for i.i.d. Exp($\lambda$)? | $1/(2\lambda)$ |
| P105 | Conditional expectation: $E[X|Y=y]$ for joint $(X,Y)$ uniform on triangle? | Depends on triangle geometry |
| P106 | $E[X^2]$ for $X \sim \text{Exp}(1)$? | $\Gamma(3)/1 = 2$ |
| P107 | $\text{Var}(XY)$ where $X,Y$ independent, zero-mean? | $E[X^2]E[Y^2]$ (since $E[X]=E[Y]=0$) |
| P108 | Tower property: $E[E[X|Y]] = ?$ | $E[X]$ |
| P109 | $E[X|X > a]$ for $X \sim \text{Exp}(\lambda)$? | $a + 1/\lambda$ (memoryless!) |
| P110 | Law of total variance? | $\text{Var}(X) = E[\text{Var}(X|Y)] + \text{Var}(E[X|Y])$ |
| P111 | $\text{Cov}(X+Y, X-Y)$? | $\text{Var}(X) - \text{Var}(Y)$ |
| P112 | $X,Y$ i.i.d. $N(0,1)$. $X+Y$ and $X-Y$ independent? | YES! ($\text{Cov} = 0$ + jointly normal) |
| P113 | Sample variance $S^2 = \frac{1}{n-1}\sum(X_i-\bar{X})^2$. Why $n-1$? | Makes $E[S^2] = \sigma^2$ (unbiased) |
| P114 | Bessel's correction: biased var formula? | $\frac{1}{n}\sum(X_i-\bar{X})^2$, $E = \frac{n-1}{n}\sigma^2$ |
| P115 | $(n-1)S^2/\sigma^2 \sim ?$ | $\chi^2_{n-1}$ (for normal population) |
| P116 | Fisher info for Bernoulli? | $I(p) = 1/(p(1-p))$ |
| P117 | Fisher info for Normal (known $\sigma^2$)? | $I(\mu) = 1/\sigma^2$ |
| P118 | Cramér-Rao bound for Bernoulli mean? | $\text{Var}(\hat{p}) \geq p(1-p)/n$ |
| P119 | Sample mean achieves CRLB for Normal $\mu$? | YES! $\text{Var}(\bar{X}) = \sigma^2/n = 1/(nI(\mu))$ |
| P120 | Rao-Blackwell theorem? | Conditioning estimator on sufficient stat improves it |

**P121-P160** — Mixed GATE Problems:

**P121.** (2-mark) Transition matrix $P = \begin{pmatrix}0.7 & 0.3 \\ 0.4 & 0.6\end{pmatrix}$. $P(X_2 = 0 | X_0 = 0)$?
→ $(P^2)_{00} = 0.7 \times 0.7 + 0.3 \times 0.4 = 0.49 + 0.12 = 0.61$

**P122.** (2-mark) Same chain. Stationary distribution?
→ $\pi_0(0.7) + \pi_1(0.4) = \pi_0$, $\pi_0 + \pi_1 = 1$
→ $0.3\pi_0 = 0.4\pi_1$ → $\pi_0 = 4/7$, $\pi_1 = 3/7$

**P123.** (2-mark) Random walk on $\{0, 1, 2\}$ absorbing at 0 and 2. $P_{01} = 0.4$, $P_{10} = 0.4$, $P_{12} = 0.6$. Starting at 1, P(absorbed at 2)?
→ Let $a = P(\text{reach 2} | \text{start at 1})$. $a = 0.6 + 0.4 \cdot 0 \cdot a$... No, more carefully:
→ From 1: go to 0 (prob 0.4, absorbed) or 2 (prob 0.6, absorbed). So $a = 0.6$.

**P124.** (2-mark NAT) $X, Y$ independent standard normals. $P(X^2 + Y^2 \leq 1)$?
→ $R^2 = X^2 + Y^2 \sim \chi^2_2 = \text{Exp}(1/2)$.
→ $P(R^2 \leq 1) = 1 - e^{-1/2} \approx 0.3935$

**P125.** (2-mark) $X_i$ i.i.d. Bernoulli($p$). MLE of $p(1-p)$?
→ MLE of $p$ is $\hat{p} = \bar{X}$. By invariance: MLE of $p(1-p) = \hat{p}(1-\hat{p}) = \bar{X}(1-\bar{X})$

**P126.** (2-mark) 95% CI for proportion: $n = 400$, $\hat{p} = 0.6$.
→ SE $= \sqrt{0.6 \times 0.4/400} = \sqrt{0.0006} = 0.0245$
→ CI: $0.6 \pm 1.96 \times 0.0245 = 0.6 \pm 0.048 = (0.552, 0.648)$

**P127.** (2-mark) Type II error: $H_0: \mu = 100$, $H_1: \mu = 105$, $\sigma = 15$, $n = 25$, $\alpha = 0.05$.
→ Reject $H_0$ if $\bar{X} > 100 + 1.645 \times 15/5 = 100 + 4.935 = 104.935$
→ $\beta = P(\bar{X} < 104.935 | \mu = 105) = P(Z < (104.935-105)/3) = P(Z < -0.022) \approx 0.491$
→ Power $= 1 - \beta \approx 0.509$

**P128-P160** — Speed Round:

| # | Q | A |
|---|---|---|
| P128 | $P(A \cap B) = P(A)P(B)$ means? | Independent |
| P129 | $P(A|B) = P(A)$ means? | Independent (same thing) |
| P130 | $E[\text{Binomial}(n,p)]$ | $np$ |
| P131 | Var of Binomial? | $np(1-p)$ |
| P132 | Poisson as limit of Binomial: conditions? | $n \to \infty$, $p \to 0$, $np = \lambda$ |
| P133 | $P(X+Y = k)$ for independent Poisson($\lambda_1$), Poisson($\lambda_2$)? | Poisson($\lambda_1+\lambda_2$) |
| P134 | Sum of normals (independent)? | Normal: $N(\mu_1+\mu_2, \sigma_1^2+\sigma_2^2)$ |
| P135 | Sum of chi-squared (independent)? | $\chi^2_{n_1+n_2}$ |
| P136 | $F$-distribution: ratio of? | $\frac{\chi^2_{m}/m}{\chi^2_{n}/n}$ |
| P137 | When to use $F$-test? | Comparing two variances |
| P138 | ANOVA uses? | $F$-test for comparing means |
| P139 | Sufficient stat for Poisson? | $\sum X_i$ |
| P140 | Completeness means? | No unbiased estimator of 0 except 0 itself |
| P141 | UMVUE via? | Rao-Blackwell + Lehmann-Scheffé |
| P142 | Likelihood ratio test statistic? | $\Lambda = \frac{\max_{H_0} L(\theta)}{\max_\Theta L(\theta)}$ |
| P143 | Wilks' theorem: $-2\ln\Lambda$ large sample? | $\chi^2_k$ where $k$ = df difference |
| P144 | Neyman-Pearson lemma: best test for? | Simple vs simple hypothesis |
| P145 | Power function $\beta(\theta) = ?$ | $P_\theta(\text{reject } H_0)$ |
| P146 | Size of test = $\sup_{\theta \in H_0} \beta(\theta) = ?$ | $\alpha$ |
| P147 | Consistent test? | Power → 1 as $n \to \infty$ |
| P148 | Bonferroni correction: $k$ tests? | Use $\alpha/k$ for each |
| P149 | FDR (Benjamini-Hochberg)? | Controls expected proportion of false discoveries |
| P150 | Bootstrap: idea? | Resample WITH replacement from data |
| P151 | Jackknife: idea? | Leave-one-out resampling |
| P152 | Kernel density estimation? | $\hat{f}(x) = \frac{1}{nh}\sum K(\frac{x-X_i}{h})$ |
| P153 | EM algorithm: E-step? | Compute expected log-likelihood given current params |
| P154 | EM algorithm: M-step? | Maximize expected log-likelihood |
| P155 | EM always converges? | Monotonically increases likelihood; converges to local max |
| P156 | $R^2$ in regression? | $1 - \frac{SS_{res}}{SS_{tot}}$ — proportion of variance explained |
| P157 | Adjusted $R^2$? | Penalizes for number of predictors |
| P158 | AIC criterion? | $2k - 2\ln L$ — balance fit & complexity |
| P159 | BIC criterion? | $k\ln n - 2\ln L$ — stronger penalty for complexity |
| P160 | Cross-validation purpose? | Estimate out-of-sample prediction error |

---

# 📝 Practice Set 3: Advanced & Tricky (P161 – P250)

**P161.** (2-mark) $X \sim \text{Gamma}(3, 2)$. $P(X > 1)$ using CDF?
→ $F(x) = 1 - e^{-2x}(1 + 2x + 2x^2)$ (incomplete gamma for integer $\alpha$)
→ $P(X > 1) = e^{-2}(1 + 2 + 2) = 5e^{-2} \approx 0.677$

**P162.** (2-mark) $X \sim \text{Beta}(2, 3)$. Find mean, variance, mode.
→ Mean = $2/5 = 0.4$, Var = $\frac{6}{25 \times 6} = 1/25 = 0.04$. No wait: $\frac{2 \times 3}{5^2 \times 6} = 6/150 = 1/25$.
→ Mode = $(2-1)/(2+3-2) = 1/3$

**P163.** (2-mark) Generate $X \sim \text{Exp}(\lambda)$ from $U \sim \text{Uniform}(0,1)$.
→ Inverse CDF method: $F(x) = 1 - e^{-\lambda x}$ → $x = -\frac{1}{\lambda}\ln(1-U) = -\frac{1}{\lambda}\ln U$

**P164.** (2-mark) Memoryless property: show only Geometric (discrete) and Exponential (continuous) have it.
→ $P(X > s+t | X > s) = P(X > t)$
→ Geometric: $P(X > s+t)/P(X > s) = (1-p)^{s+t}/(1-p)^s = (1-p)^t = P(X > t)$ ✅
→ Exponential: $e^{-\lambda(s+t)}/e^{-\lambda s} = e^{-\lambda t}$ ✅

**P165.** (2-mark) Prove: $E[X^2] \geq (E[X])^2$ (Jensen's inequality for $f(x) = x^2$).
→ $f(x) = x^2$ is convex ($f'' = 2 > 0$). Jensen: $f(E[X]) \leq E[f(X)]$.
→ $(E[X])^2 \leq E[X^2]$ ✅ (This is just $\text{Var}(X) \geq 0$!)

**P166-P200** — GATE PYQ Style:

| # | Q | A |
|---|---|---|
| P166 | 2 fair dice. $E[\text{max}]$? | $E[\max] = 7 - E[\min] = 7 - \sum_{k=1}^6\frac{(7-k)^2 - (6-k)^2}{36}$... Direct: $\sum_{k=1}^6 k P(\max = k) = 161/36 \approx 4.472$ |
| P167 | Random walk: $S_n = X_1+\cdots+X_n$, $P(X = +1) = P(X=-1) = 0.5$. $E[S_{100}]$? $\text{Var}(S_{100})$? | $E = 0$, $\text{Var} = 100$ |
| P168 | Gambler's ruin: start at $k$, absorbing at $0$ and $N$. $P(\text{ruin at 0})$? (fair game) | $(N-k)/N$ |
| P169 | Same, unfair ($p \neq 0.5$)? | $\frac{(q/p)^k - (q/p)^N}{1-(q/p)^N}$ |
| P170 | Balls into bins: $n$ balls, $n$ bins. $E[\text{empty bins}]$? | $n(1-1/n)^n \approx n/e$ |
| P171 | Same: $E[\text{max load}]$? | $\Theta(\ln n/\ln\ln n)$ |
| P172 | Records in permutation: $E[\text{number of records}]$? | $H_n = 1 + 1/2 + \cdots + 1/n \approx \ln n$ |
| P173 | Derangement: $P(\text{no fixed point})$? | $\sum_{k=0}^n \frac{(-1)^k}{k!} \approx 1/e$ |
| P174 | $n$ people at table. $P(\text{specific two adjacent})$? | $2/(n-1)$ |
| P175 | Matching birthday in 50 people? | $P \approx 0.97$ (very likely!) |
| P176 | Secretary problem: optimal stopping? | Reject first $n/e$, then pick first better | 
| P177 | Multivariate CLT? | $\sqrt{n}(\bar{X}-\mu) \xrightarrow{d} N(0, \Sigma)$ |
| P178 | Delta method: $g(\bar{X}) \approx ?$ | $N(g(\mu), (g'(\mu))^2\sigma^2/n)$ |
| P179 | $\hat\theta$ consistent + $g$ continuous → $g(\hat\theta)$ consistent? | YES (continuous mapping theorem) |
| P180 | Slutsky's theorem? | $X_n \xrightarrow{d} X$, $Y_n \xrightarrow{P} c$ → $X_nY_n \xrightarrow{d} cX$ |
| P181 | M-estimator? | Minimizes $\sum \rho(X_i, \theta)$ for some $\rho$ |
| P182 | Robust estimator of location? | Median (not affected by outliers) |
| P183 | MAD = ? | Median Absolute Deviation |
| P184 | Breakdown point of mean? | $1/n$ (one outlier ruins it) |
| P185 | Breakdown point of median? | $1/2$ (robust!) |
| P186 | qq-plot checks? | If data follows assumed distribution |
| P187 | Kolmogorov-Smirnov test? | Tests if data follows specific CDF |
| P188 | Anderson-Darling test? | Like KS but more weight on tails |
| P189 | Pearson vs Spearman correlation? | Pearson: linear; Spearman: monotonic (rank-based) |
| P190 | Kendall's tau? | Concordance-based rank correlation |
| P191 | Simpson's paradox? | Aggregate trend reverses within subgroups |
| P192 | Example: treatment helps in each subgroup but hurts overall? | Confounding variable (lurking variable) |
| P193 | Regression to the mean? | Extreme values tend to be followed by less extreme |
| P194 | Multicollinearity? | Predictors highly correlated → unstable coefficients |
| P195 | VIF > 10 means? | Serious multicollinearity |
| P196 | Heteroscedasticity? | Non-constant variance of errors |
| P197 | White's test for? | Heteroscedasticity |
| P198 | Durbin-Watson test for? | Autocorrelation in residuals |
| P199 | Gauss-Markov theorem? | OLS is BLUE under assumptions |
| P200 | BLUE stands for? | Best Linear Unbiased Estimator |

**P201-P250** — Computational Practice:

**P201.** (2-mark) $X \sim \text{Poisson}(3)$. $E[X(X-1)]$?
→ $E[X(X-1)] = \lambda^2 = 9$ (second factorial moment of Poisson)

**P202.** (2-mark) $X \sim \text{Geometric}(p)$. $\text{Var}(X)$ where $X$ = # trials until first success?
→ $\text{Var}(X) = (1-p)/p^2$

**P203.** (2-mark) Moment method estimator for Uniform$(0, \theta)$?
→ $E[X] = \theta/2 = \bar{X}$ → $\hat{\theta}_{MOM} = 2\bar{X}$

**P204.** (2-mark NAT) $X_1, X_2$ i.i.d. $\text{Exp}(1)$. $P(X_1 > 2X_2)$?
→ $P(X_1 > 2X_2) = \int_0^{\infty} P(X_1 > 2y)e^{-y}dy = \int_0^{\infty} e^{-2y}e^{-y}dy = \int_0^{\infty} e^{-3y}dy = 1/3$

**P205-P250** — Quick Problems:

| # | Q | A |
|---|---|---|
| P205 | $n=100$, $\hat{p} = 0.3$. Test $H_0: p = 0.5$ at $\alpha = 0.05$ | $Z = (0.3-0.5)/\sqrt{0.5 \times 0.5/100} = -4$. Reject. |
| P206 | Mann-Whitney U test? | Non-parametric test for two independent samples |
| P207 | Kruskal-Wallis test? | Non-parametric ANOVA (3+ groups) |
| P208 | Sign test? | Non-parametric test for matched pairs |
| P209 | Wilcoxon signed-rank test? | Sign test with magnitude information |
| P210 | Paired t-test: when? | Same subjects, two conditions |
| P211 | Two-sample t-test assumes? | Normal populations, equal variance (pooled) |
| P212 | Welch's t-test? | Unequal variance two-sample test |
| P213 | Effect size (Cohen's d)? | $(\mu_1-\mu_2)/\sigma_{pooled}$; small: 0.2, medium: 0.5, large: 0.8 |
| P214 | Power analysis: need to know? | $\alpha$, effect size, sample size, power (solve for 4th) |
| P215 | n for 95% CI width $\leq w$? | $n \geq (2z_{\alpha/2}\sigma/w)^2$ |
| P216 | Linear regression: $Y = \beta_0 + \beta_1 X + \epsilon$. $\hat\beta_1 = ?$ | $\frac{\sum(X_i-\bar{X})(Y_i-\bar{Y})}{\sum(X_i-\bar{X})^2}$ |
| P217 | $\hat\beta_0 = ?$ | $\bar{Y} - \hat\beta_1\bar{X}$ |
| P218 | $\hat\beta_1$ sampling distribution? | $N(\beta_1, \sigma^2/S_{xx})$ where $S_{xx} = \sum(X_i-\bar{X})^2$ |
| P219 | SST = SSR + SSE? | Total = Regression + Error |
| P220 | $R^2 = ?$ | $SSR/SST = 1 - SSE/SST$ |
| P221 | Random forest reduces? | Variance (by averaging) |
| P222 | Bias-variance decomposition? | $\text{MSE} = \text{Bias}^2 + \text{Variance} + \text{Irreducible noise}$ |
| P223 | Bayesian credible interval? | $P(\theta \in [a,b] | \text{data}) = 0.95$ |
| P224 | Frequentist CI interpretation? | 95% of such intervals contain true $\theta$ |
| P225 | Prior × Likelihood ∝ Posterior? | Bayes' theorem for continuous |
| P226 | Jeffreys prior? | $\pi(\theta) \propto \sqrt{I(\theta)}$ — non-informative |
| P227 | Proper vs improper prior? | Proper: integrates to 1; Improper: doesn't |
| P228 | Posterior predictive distribution? | $p(x_{new}|\text{data}) = \int p(x_{new}|\theta)\pi(\theta|\text{data})d\theta$ |
| P229 | Exponential family form? | $f(x|\theta) = h(x)\exp(\eta(\theta)T(x) - A(\theta))$ |
| P230 | Natural parameter? | $\eta(\theta)$ in exponential family |
| P231 | Sufficient statistic from exp family? | $T(x)$ |
| P232 | Gamma conjugate for Poisson $\lambda$: posterior mean? | $\frac{\alpha + \sum x_i}{\beta + n}$ (weighted avg of prior and MLE) |
| P233 | Bayesian point estimate: posterior mean vs MAP vs median? | Depends on loss: quadratic → mean, 0-1 → MAP, absolute → median |
| P234 | Minimax estimator? | Minimizes maximum risk over $\theta$ |
| P235 | Admissible estimator? | No other estimator dominates it everywhere |
| P236 | James-Stein estimator? | Shrinks toward grand mean; dominates MLE for $p \geq 3$ |
| P237 | Stein's paradox? | MLE inadmissible for $\geq 3$ normal means! |
| P238 | Empirical Bayes? | Estimate prior from data |
| P239 | MCMC for Bayesian? | Markov chain Monte Carlo to sample from posterior |
| P240 | Metropolis-Hastings? | Accept/reject proposals based on density ratio |
| P241 | Gibbs sampling? | Special MCMC: sample each variable conditionally |
| P242 | Convergence diagnostic? | Trace plots, Gelman-Rubin $\hat{R}$ |
| P243 | Monte Carlo integration? | $E[f(X)] \approx \frac{1}{n}\sum f(X_i)$ |
| P244 | Importance sampling? | Reweight samples from proposal $q$: $E_p[f] = E_q[f \cdot w]$ where $w = p/q$ |
| P245 | Rejection sampling? | Accept $X \sim q$ with prob $\propto p(X)/Mq(X)$ |
| P246 | Inverse CDF method? | $X = F^{-1}(U)$ where $U \sim \text{Uniform}(0,1)$ |
| P247 | Box-Muller transform? | Two uniforms → two standard normals |
| P248 | Law of total cov? | $\text{Cov}(X,Y) = E[\text{Cov}(X,Y|Z)] + \text{Cov}(E[X|Z], E[Y|Z])$ |
| P249 | Exchangeable RVs? | Joint distribution invariant under permutation |
| P250 | De Finetti's theorem? | Exchangeable ↔ conditionally i.i.d. given some parameter |

---

# 📝 Practice Set 4: Markov Chains & Stochastic Processes (P251 – P320)

**P251.** (2-mark) Three-state Markov chain with transition matrix:
$$P = \begin{pmatrix} 0 & 1 & 0 \\ 0.5 & 0 & 0.5 \\ 0 & 1 & 0 \end{pmatrix}$$
Is it irreducible?
→ State 0 → 1 → 0 (yes); 0 → 1 → 2 (yes); 2 → 1 → 0 (yes). All communicate → **Irreducible** ✅

**P252.** (2-mark) Same chain: period of state 1?
→ State 1 can return in 2 steps (1→0→1 or 1→2→1). Can it in 3? 1→0→1→0 no, 1→0→1→2 no... $d(1) = \gcd\{2, 4, \ldots\} = 2$.
→ Period $= 2$ → **NOT ergodic** (periodic)

**P253.** (2-mark) Absorbing chain: 3 states, 0 absorbing, 2 absorbing.
$$P = \begin{pmatrix} 1 & 0 & 0 \\ 0.3 & 0.4 & 0.3 \\ 0 & 0 & 1 \end{pmatrix}$$
Starting at state 1, P(absorbed at 0)?
→ $a = 0.3 + 0.4a + 0.3 \cdot 0 = 0.3 + 0.4a$ → $0.6a = 0.3$ → $a = 0.5$

**P254.** (2-mark) Same: expected absorption time from state 1?
→ $t = 1 + 0.4t$ → $0.6t = 1$ → $t = 5/3 \approx 1.667$ steps

**P255-P270** — Poisson Process Problems:

| # | Q | A |
|---|---|---|
| P255 | Customers arrive at rate 10/hr. P(3 in 30 min)? | $\lambda t = 5$. $P = 5^3e^{-5}/6 \approx 0.1404$ |
| P256 | Same: expected time between arrivals? | $1/10$ hr = 6 min |
| P257 | Time to 3rd arrival distribution? | Gamma(3, 10): $E = 3/10$ hr = 18 min |
| P258 | Split: each customer male w.p. 0.6. Male arrivals rate? | $10 \times 0.6 = 6$/hr |
| P259 | Merge two Poisson(3) and Poisson(5). Result? | Poisson(8) |
| P260 | Given 1 arrival in [0,1], distribution of arrival time? | Uniform(0,1) |
| P261 | Given $N(1) = 5$, expected time of 3rd arrival? | $3/6 = 3/5$ of the interval, expected $= 3/6$ |
| P262 | Arrivals in non-overlapping intervals independent? | YES (independent increments) |
| P263 | P(more arrivals in [0,1] than [1,2])? | By symmetry: $< 0.5$ (actually complicated; equal distr → $P = (1 - P(\text{equal}))/2$) |
| P264 | Inter-arrival times for Poisson process are? | i.i.d. Exponential($\lambda$) |
| P265 | Non-homogeneous Poisson: $\lambda(t) = 2t$. $E[N(3)]$? | $\int_0^3 2t\,dt = 9$ |
| P266 | Compound Poisson process? | $Y = \sum_{i=1}^{N} X_i$ where $N$ is Poisson |
| P267 | $E[Y]$ in compound Poisson? | $E[N]E[X] = \lambda E[X]$ |
| P268 | $\text{Var}(Y)$ in compound Poisson? | $\lambda E[X^2]$ |
| P269 | Thinning: each event kept w.p. $p(t)$? | Non-homogeneous with rate $\lambda p(t)$ |
| P270 | Superposition + thinning? | Key operations preserving Poisson structure |

**P271-P320** — Mixed Advanced:

| # | Q | A |
|---|---|---|
| P271 | Renewal theory: $E[\text{number of renewals by } t]$? | $m(t) = E[N(t)] \approx t/\mu$ (elementary renewal theorem) |
| P272 | Inspection paradox? | Current inter-renewal period tends to be LONGER than average |
| P273 | Google PageRank model? | Markov chain on webpages; stationary dist = importance |
| P274 | Hidden Markov Model components? | States (hidden), observations, transition probs, emission probs |
| P275 | Viterbi algorithm? | Most likely hidden state sequence |
| P276 | Forward algorithm? | $P(\text{observations})$ efficiently |
| P277 | Baum-Welch? | EM for HMM parameter estimation |
| P278 | Gibbs sampling for Ising model? | Update each spin conditioned on neighbors |
| P279 | Simulated annealing? | Random search with decreasing temperature |
| P280 | Boltzmann distribution? | $p_i \propto e^{-E_i/kT}$ |
| P281 | Ergodic theorem for MC? | Time average = space average (for ergodic chains) |
| P282 | Reversible MC? | Detailed balance: $\pi_i P_{ij} = \pi_j P_{ji}$ |
| P283 | Birth-death chain? | Transitions only to neighbors: $i \to i\pm 1$ |
| P284 | M/M/1 queue: service rate $\mu$, arrival $\lambda$. Stable when? | $\lambda < \mu$ (utilization $\rho = \lambda/\mu < 1$) |
| P285 | M/M/1 steady state: $P(n\text{ in system})$? | $(1-\rho)\rho^n$ (geometric!) |
| P286 | $E[\text{number in M/M/1}]$? | $\rho/(1-\rho)$ |
| P287 | $E[\text{time in M/M/1}]$? | $1/(\mu-\lambda)$ (Little's law: $L = \lambda W$) |
| P288 | Little's law? | $L = \lambda W$ (avg number = arrival rate × avg time) |
| P289 | M/M/c queue? | $c$ servers, Erlang C formula |
| P290 | Brownian motion? | Continuous-time random walk limit; $B(t) \sim N(0,t)$ |
| P291 | $\text{Var}(B(t))$? | $t$ |
| P292 | $\text{Cov}(B(s), B(t))$ for $s < t$? | $s$ (= $\min(s,t)$) |
| P293 | Martingale? | $E[X_{n+1}|X_1,\ldots,X_n] = X_n$ (fair game) |
| P294 | Example martingale? | Cumulative sum of fair coin flips |
| P295 | Optional stopping theorem? | Under conditions: $E[X_\tau] = E[X_0]$ |
| P296 | Wald's identity? | $E[\sum_{i=1}^N X_i] = E[N]E[X]$ for stopping time $N$ |
| P297 | Kolmogorov forward equation? | $P'(t) = P(t)Q$ where $Q$ = rate matrix |
| P298 | Kolmogorov backward equation? | $P'(t) = QP(t)$ |
| P299 | Generator matrix $Q$: off-diagonal? | $q_{ij} \geq 0$ transition rates |
| P300 | Generator diagonal? | $q_{ii} = -\sum_{j \neq i} q_{ij}$ (rows sum to 0) |
| P301 | Mixing time of MC? | Time to approximately reach stationarity |
| P302 | Spectral gap → mixing time? | Larger gap → faster mixing |
| P303 | Coupling argument? | Bound mixing by coupling two chains |
| P304 | MCMC: burn-in period? | Discard initial samples before stationarity |
| P305 | Effective sample size? | Accounts for autocorrelation in MCMC |
| P306 | Thinning in MCMC? | Keep every $k$th sample to reduce autocorrelation |
| P307 | Convergence in total variation? | $\|P^n(x, \cdot) - \pi\|_{TV} \to 0$ |
| P308 | Perron-Frobenius theorem? | Positive matrix has unique largest eigenvalue (= 1 for stochastic) |
| P309 | Second eigenvalue → mixing? | $|\lambda_2|$ close to 1 → slow mixing |
| P310 | Random walk on graph: stationary? | $\pi_i \propto \deg(i)$ |
| P311 | Hitting time: expected? | Solve $h_i = 1 + \sum_j P_{ij} h_j$ for target state |
| P312 | Commute time? | $h_{ij} + h_{ji}$ (not symmetric!) |
| P313 | Cover time? | Expected time to visit all states |
| P314 | Eigenvalues of doubly stochastic? | Stationary = uniform |
| P315 | PageRank damping factor $d$? | Typically $d = 0.85$ |
| P316 | Teleportation in PageRank? | With prob $1-d$, jump to random page |
| P317 | Katz centrality? | $(I - \alpha A)^{-1}\mathbf{1}$ — resolvent-based |
| P318 | HITS algorithm? | Authority + Hub scores (eigenvector-based) |
| P319 | CDF of max of $n$ i.i.d. Uniform? | $F(x) = x^n$ on $[0,1]$ |
| P320 | Hazard rate: $h(t) = ?$ | $f(t)/(1-F(t))$ — instantaneous failure rate |

---

## ⚡ Quick Reference Formula Card

### Essential Distributions

| Distribution | PMF/PDF | Mean | Variance |
|---|---|---|---|
| Bernoulli($p$) | $p^x(1-p)^{1-x}$ | $p$ | $p(1-p)$ |
| Binomial($n,p$) | $\binom{n}{x}p^x(1-p)^{n-x}$ | $np$ | $np(1-p)$ |
| Geometric($p$) | $(1-p)^{x-1}p$ | $1/p$ | $(1-p)/p^2$ |
| Poisson($\lambda$) | $e^{-\lambda}\lambda^x/x!$ | $\lambda$ | $\lambda$ |
| Uniform($a,b$) | $1/(b-a)$ | $(a+b)/2$ | $(b-a)^2/12$ |
| Exponential($\lambda$) | $\lambda e^{-\lambda x}$ | $1/\lambda$ | $1/\lambda^2$ |
| Normal($\mu,\sigma^2$) | $\frac{1}{\sigma\sqrt{2\pi}}e^{-(x-\mu)^2/(2\sigma^2)}$ | $\mu$ | $\sigma^2$ |
| Gamma($\alpha,\beta$) | $\frac{\beta^\alpha}{\Gamma(\alpha)}x^{\alpha-1}e^{-\beta x}$ | $\alpha/\beta$ | $\alpha/\beta^2$ |
| Beta($\alpha,\beta$) | $\frac{x^{\alpha-1}(1-x)^{\beta-1}}{B(\alpha,\beta)}$ | $\frac{\alpha}{\alpha+\beta}$ | $\frac{\alpha\beta}{(\alpha+\beta)^2(\alpha+\beta+1)}$ |
| Chi-squared($k$) | special Gamma | $k$ | $2k$ |

### Standard Normal Table (Key Values)

| $z$ | $\Phi(z)$ | $P(Z > z)$ |
|---|---|---|
| 0.00 | 0.5000 | 0.5000 |
| 0.67 | 0.7486 | 0.2514 |
| 1.00 | 0.8413 | 0.1587 |
| 1.28 | 0.8997 | 0.1003 |
| 1.645 | 0.9500 | 0.0500 |
| 1.96 | 0.9750 | 0.0250 |
| 2.00 | 0.9772 | 0.0228 |
| 2.33 | 0.9901 | 0.0099 |
| 2.576 | 0.9950 | 0.0050 |
| 3.00 | 0.9987 | 0.0013 |

### Key Identities

- $\text{Var}(X) = E[X^2] - (E[X])^2$
- $\text{Var}(aX+bY) = a^2\text{Var}(X) + b^2\text{Var}(Y) + 2ab\text{Cov}(X,Y)$
- $E[XY] = E[X]E[Y] + \text{Cov}(X,Y)$
- $\text{Cov}(X,Y) = E[XY] - E[X]E[Y]$
- $\rho_{XY} = \frac{\text{Cov}(X,Y)}{\sigma_X \sigma_Y}$
- Tower: $E[E[X|Y]] = E[X]$
- Law of total variance: $\text{Var}(X) = E[\text{Var}(X|Y)] + \text{Var}(E[X|Y])$

---

## 🎯 Commonly Confused Pairs

| Concept A | Concept B | Difference |
|---|---|---|
| Uncorrelated | Independent | Independent ⊂ Uncorrelated (reverse only for Normal) |
| Permutations $P(n,r)$ | Combinations $C(n,r)$ | Permutation = order matters |
| With replacement | Without replacement | With: independent draws; Without: dependent |
| Population $\sigma$ | Sample $s$ | Use $s$ when $\sigma$ unknown; adds uncertainty |
| One-tailed | Two-tailed test | One: direction specified; Two: both directions |
| Type I error | Type II error | I: false positive ($\alpha$); II: false negative ($\beta$) |
| Confidence interval | Credible interval | CI: frequentist; Credible: Bayesian |
| MLE | MAP | MLE: no prior; MAP: with prior |
| Bias | Variance | Bias: systematic error; Variance: scatter |
| $\sigma$ | $\sigma/\sqrt{n}$ | Population SD vs Standard Error of mean |
| Geometric (from 1) | Geometric (from 0) | Mean: $1/p$ vs $(1-p)/p$ — check convention! |
| PMF | PDF | PMF: discrete (prob); PDF: continuous (density, can > 1) |
| CDF | Survival | $F(x) = P(X \leq x)$; $S(x) = P(X > x) = 1-F(x)$ |
| Hazard | CDF | $h(t) = f(t)/(1-F(t))$ — instantaneous rate |

---

## 📊 GATE Score Maximization Strategy

### Priority Topics (decreasing order)
1. **Bayes' Theorem** — appears 90% of years, mechanical, high reward
2. **Expectation/Variance** — linear combination problems, indicator trick
3. **Normal distribution** — standardization, CLT applications
4. **Conditional probability** — urn problems, sequential draws
5. **Binomial/Poisson** — computation with approximation
6. **Random variables** — CDF/PDF, transformations
7. **Hypothesis testing** — decision framework
8. **Chi-squared test** — goodness-of-fit, independence

### Time Allocation per Question Type
| Type | Expected Time | Strategy |
|---|---|---|
| Bayes' theorem (2-mark) | 3 min | Draw tree, apply formula |
| E[X] calculation | 2 min | Check for linearity trick |
| Normal standardization | 2 min | Convert to Z, use table |
| Distribution identification | 1 min | Pattern match from table |
| Hypothesis test | 3 min | State $H_0$, compute statistic, compare critical |
| Proof/conceptual | 3-4 min | Only if confident; else skip |

---

## 🏅 50 One-Liner Rapid Recall

1. $P(A \cup B) = P(A) + P(B) - P(A \cap B)$
2. $P(A|B) = P(A \cap B)/P(B)$
3. Bayes: $P(A|B) = P(B|A)P(A)/P(B)$
4. Total probability: $P(B) = \sum P(B|A_i)P(A_i)$
5. Independent: $P(A \cap B) = P(A)P(B)$
6. $E[aX+bY] = aE[X] + bE[Y]$ (ALWAYS)
7. $\text{Var}(aX+b) = a^2\text{Var}(X)$
8. $\text{Var}(X) = E[X^2]-(E[X])^2$
9. $\text{Cov}(X,Y) = E[XY]-E[X]E[Y]$
10. Independent → $\text{Cov} = 0$ (not reverse!)
11. Bernoulli: $E = p$, $V = p(1-p)$
12. Binomial: $E = np$, $V = np(1-p)$
13. Poisson: $E = V = \lambda$
14. Geometric: $E = 1/p$, $V = (1-p)/p^2$
15. Uniform: $E = (a+b)/2$, $V = (b-a)^2/12$
16. Exponential: $E = 1/\lambda$, $V = 1/\lambda^2$, memoryless
17. Normal: 68% within $1\sigma$, 95% within $2\sigma$, 99.7% within $3\sigma$
18. CLT: $\bar{X} \approx N(\mu, \sigma^2/n)$ for large $n$
19. $Z = (X-\mu)/\sigma$ for standardization
20. CI: $\bar{X} \pm z_{\alpha/2} \cdot \sigma/\sqrt{n}$
21. Chi-squared df for GOF: $k-1$
22. Chi-squared df for independence: $(r-1)(c-1)$
23. Reject $H_0$ if p-value $< \alpha$
24. Type I = $\alpha$ (false positive), Type II = $\beta$ (false negative)
25. Power = $1 - \beta$
26. MLE: maximize $L(\theta) = \prod f(x_i;\theta)$
27. Fisher info = $-E[l''(\theta)]$
28. CRLB: $\text{Var} \geq 1/(nI(\theta))$
29. Sufficient stat via factorization: $f(x;\theta) = g(T(x),\theta)h(x)$
30. MGF: $M(t) = E[e^{tX}]$, $M^{(n)}(0) = E[X^n]$
31. PGF: $G(z) = E[z^X]$, $G'(1) = E[X]$
32. Markov property: future depends only on present
33. Stationary: $\pi P = \pi$, $\sum \pi_i = 1$
34. Ergodic = irreducible + aperiodic + positive recurrent
35. Poisson process: inter-arrivals are Exp($\lambda$)
36. Merge Poisson: $\lambda_1 + \lambda_2$
37. Split Poisson by $p$: Poisson($\lambda p$)
38. Order stats of Uniform: $E[X_{(k)}] = k/(n+1)$
39. Min of Exp($\lambda_i$) = Exp($\sum\lambda_i$)
40. Entropy: $H(X) = -\sum p_i\log p_i$
41. MI: $I(X;Y) = H(X) + H(Y) - H(X,Y) \geq 0$
42. KL: $D_{KL}(P\|Q) \geq 0$, not symmetric
43. Chebyshev: $P(|X-\mu| \geq k\sigma) \leq 1/k^2$
44. Markov: $P(X \geq a) \leq E[X]/a$
45. Jensen: $f$ convex → $f(E[X]) \leq E[f(X)]$
46. WLLN: $\bar{X} \xrightarrow{P} \mu$
47. SLLN: $\bar{X} \xrightarrow{a.s.} \mu$
48. M/M/1: $L = \rho/(1-\rho)$, $W = 1/(\mu-\lambda)$
49. Little's law: $L = \lambda W$
50. Gamma(n,$\lambda$) = sum of $n$ i.i.d. Exp($\lambda$)

---

# 📝 Practice Set 5: Expectation & Variance Deep Dive (P321 – P420)

**P321.** (2-mark) $X$ uniformly chosen from $\{1, 2, \ldots, n\}$. $E[X]$ and $\text{Var}(X)$?
→ $E[X] = (n+1)/2$. $E[X^2] = (n+1)(2n+1)/6$.
→ $\text{Var}(X) = \frac{(n+1)(2n+1)}{6} - \frac{(n+1)^2}{4} = \frac{n^2-1}{12}$

**P322.** (2-mark) Indicator variable method: $n$ people randomly seated at round table. $E[\text{couples sitting together}]$ for $k$ couples?
→ $I_i = 1$ if couple $i$ adjacent. $P(I_i = 1) = \frac{2}{n-1}$ (fix one, other has 2 of $n-1$ spots).
→ $E[\sum I_i] = k \cdot \frac{2}{n-1}$

**P323.** (2-mark) 10 dice rolled. $E[\text{number of distinct values}]$?
→ $E = \sum_{v=1}^6 P(\text{value } v \text{ appears}) = 6(1-(5/6)^{10}) = 6(1-0.1615) \approx 5.031$

**P324.** (2-mark) $X \sim \text{Binomial}(n, p)$. $E[X(X-1)(X-2)]$?
→ Third factorial moment: $n(n-1)(n-2)p^3$

**P325.** (2-mark) $X \sim \text{Poisson}(\lambda)$. $E[1/(X+1)]$?
→ $\sum_{k=0}^{\infty} \frac{1}{k+1} \frac{e^{-\lambda}\lambda^k}{k!} = \frac{e^{-\lambda}}{\lambda}\sum_{k=0}^{\infty}\frac{\lambda^{k+1}}{(k+1)!} = \frac{e^{-\lambda}}{\lambda}(e^{\lambda}-1) = \frac{1-e^{-\lambda}}{\lambda}$

**P326.** (2-mark) Negative hypergeometric: draw without replacement until $r$th red. $E[\text{draws}]$?
→ For urn with $K$ red, $N-K$ blue, draw until $r$ red: $E = r(N+1)/(K+1)$

**P327.** (2-mark) Coupon collector variance: $n$ types. $\text{Var}[\text{time to complete}]$?
→ $\text{Var} = \sum_{i=1}^n \frac{i(n-i)}{(n-i+1)^2 \cdot (\text{no, let's use:})} = n^2\sum_{k=1}^n \frac{1}{k^2} - n\sum_{k=1}^n\frac{1}{k}$... 
→ Actually: $\text{Var} = \sum_{j=0}^{n-1} \frac{n^2 j}{(n-j)^2} = n^2\sum_{k=1}^n \frac{1}{k^2} \approx n^2\pi^2/6$

**P328.** (2-mark) $X, Y$ i.i.d. $N(0,1)$. $E[|X-Y|]$?
→ $X - Y \sim N(0, 2)$. $E[|X-Y|] = \sqrt{2} \cdot E[|Z|]$ where $Z \sim N(0,1)$. $E[|Z|] = \sqrt{2/\pi}$.
→ $E[|X-Y|] = \sqrt{2} \cdot \sqrt{2/\pi} = 2/\sqrt{\pi}$

**P329.** (2-mark) $X_1, X_2, X_3$ i.i.d. Uniform(0,1). $E[X_{(2)}]$ (median)?
→ $E[X_{(k)}] = k/(n+1) = 2/4 = 1/2$

**P330.** (2-mark) $E[\max(X_1, X_2)]$ for i.i.d. Exp(1)?
→ $E[\max] = E[X_1 + X_2] - E[\min] = 2 - 1/2 = 3/2$
→ Or directly: $E[X_{(2)}] = 1/2 + 1/1 = 3/2$ (using order stats of exponential: $\sum_{k=1}^n \frac{1}{n-k+1}$, wait)
→ Actually for Exp(1), $E[X_{(k)}] = \sum_{i=1}^k \frac{1}{n-i+1}$. $E[X_{(2)}] = 1/2 + 1/1 = 3/2$ ✅

**P331-P360** — Conditional Expectation Practice:

**P331.** (2-mark) $N \sim \text{Poisson}(\lambda)$, $X_i$ i.i.d. with mean $\mu$. $E[\sum_{i=1}^N X_i]$?
→ Tower: $E[\sum X_i | N] = N\mu$ → $E[\sum X_i] = E[N\mu] = \lambda\mu$

**P332.** (2-mark) Same setup. $\text{Var}[\sum_{i=1}^N X_i]$?
→ Law of total variance: $\text{Var} = E[N]\text{Var}(X) + \text{Var}(N)(E[X])^2 = \lambda\sigma^2 + \lambda\mu^2 = \lambda E[X^2]$

**P333.** (2-mark) Random sum: $N$ geometric($p$), $X_i$ Bernoulli($q$). $E[\sum X_i]$?
→ $E = E[N]E[X] = q/p$

**P334.** (2-mark) A stick of length 1 is broken at uniform point $U$. Then left piece broken at uniform point on $[0, U]$. $E[\text{shortest piece}]$?
→ Three pieces: $V, U-V, 1-U$ where $V | U \sim \text{Uniform}(0,U)$.
→ $E[\text{shortest}]$ requires careful case analysis. Common GATE result: expected shortest = $1/4$ for 2 uniform breaks.

**P335.** (2-mark) First passage time: simple random walk starting at 0. $E[T_1]$ where $T_1 = \inf\{n : S_n = 1\}$ (fair walk)?
→ $E[T_1] = \infty$! (The walk is recurrent but expected return time is infinite)

**P336-P360** — Probability Computations:

| # | Q | A |
|---|---|---|
| P336 | $X, Y$ i.i.d. Uniform(0,1). $P(X+Y > 1)$? | $1/2$ (by symmetry of square diagonal) |
| P337 | Same: $P(XY < 1/2)$? | $1/2 + 1/2\ln 2 \approx 0.847$ |
| P338 | $P(\max(X,Y) < 1/2)$? | $(1/2)^2 = 1/4$ |
| P339 | $P(\min(X,Y) > 1/2)$? | $(1/2)^2 = 1/4$ |
| P340 | $P(X > 2Y)$? | Integral: $\int_0^{1/2}\int_{2y}^1 dxdy + 0 = 1/4$ |
| P341 | $E[\max(X,Y)]$ for Uniform(0,1)? | $2/3$ |
| P342 | $E[\min(X,Y)]$? | $1/3$ |
| P343 | $E[X^2Y]$ (independent)? | $E[X^2]E[Y] = (1/3)(1/2) = 1/6$ |
| P344 | Joint density $(X,Y)$ on unit disk: $f = 1/\pi$. Marginal of $X$? | $f_X(x) = \frac{2\sqrt{1-x^2}}{\pi}$ for $x \in [-1,1]$ |
| P345 | Correlation of $X$ and $X^2$ for $X \sim N(0,1)$? | $\text{Cov}(X, X^2) = E[X^3] - E[X]E[X^2] = 0$ (odd moment). $\rho = 0$! |
| P346 | But $X$ and $X^2$ independent? | NO! ($X^2$ determined by $X$). Uncorrelated ≠ independent! |
| P347 | Bivariate normal: $E[Y|X=x]$? | $\mu_Y + \rho\frac{\sigma_Y}{\sigma_X}(x - \mu_X)$ (linear!) |
| P348 | Var$(Y|X=x)$ for bivariate normal? | $\sigma_Y^2(1-\rho^2)$ (doesn't depend on $x$!) |
| P349 | Multinomial: $E[X_iX_j]$ for $i \neq j$? | $n(n-1)p_ip_j$ → $\text{Cov}(X_i,X_j) = -np_ip_j$ |
| P350 | Dirichlet distribution: multivariate Beta? | $f(x_1,\ldots,x_k) \propto \prod x_i^{\alpha_i-1}$ on simplex |
| P351 | Transformation: $Y = g(X)$, PDF? | $f_Y(y) = f_X(g^{-1}(y)) \cdot |dg^{-1}/dy|$ |
| P352 | $X \sim N(0,1)$, $Y = X^2$. Distribution? | $\chi^2_1$ |
| P353 | $X \sim \text{Exp}(1)$, $Y = \sqrt{X}$. PDF of $Y$? | $f_Y(y) = 2ye^{-y^2}$, $y > 0$ (Rayleigh-like) |
| P354 | $X \sim \text{Uniform}(0,1)$, $Y = -\ln X$. Distribution? | Exp(1)! |
| P355 | Convolution: $f_{X+Y}(z) = ?$ | $\int f_X(z-y)f_Y(y)dy$ |
| P356 | $X_1 + X_2$ for Uniform(0,1) i.i.d.? | Triangular on [0,2]: $f(z) = z$ for $0 \leq z \leq 1$, $2-z$ for $1 < z \leq 2$ |
| P357 | Characteristic function: $\phi_X(t) = ?$ | $E[e^{itX}]$ (always exists!) |
| P358 | Inversion formula? | $f(x) = \frac{1}{2\pi}\int e^{-itx}\phi(t)dt$ |
| P359 | Lévy continuity theorem? | $\phi_{X_n} \to \phi_X$ pointwise → $X_n \xrightarrow{d} X$ |
| P360 | CLT via characteristic functions? | $\phi_{\bar X}(t) \to e^{-t^2/2}$ = CF of $N(0,1)$ |

**P361-P420** — GATE Previous Year Pattern Questions:

**P361.** (2-mark) A box has 3 red, 4 blue, 5 green balls. Draw 3 without replacement. $P(\text{one of each color})$?
→ $\frac{\binom{3}{1}\binom{4}{1}\binom{5}{1}}{\binom{12}{3}} = \frac{60}{220} = 3/11$

**P362.** (2-mark) In a group of 12 people, 5 are from city A. Committee of 4. $P(\text{at least 2 from A})$?
→ $P = \frac{\binom{5}{2}\binom{7}{2} + \binom{5}{3}\binom{7}{1} + \binom{5}{4}\binom{7}{0}}{\binom{12}{4}} = \frac{210+70+5}{495} = \frac{285}{495} = 19/33$

**P363.** (2-mark GATE 2023 Style) Two independent random variables: $X \sim N(3, 4)$, $Y \sim N(1, 9)$. $P(X > Y)$?
→ $X - Y \sim N(3-1, 4+9) = N(2, 13)$. $P(X > Y) = P(X-Y > 0) = P\left(Z > \frac{-2}{\sqrt{13}}\right) = P(Z > -0.555) = \Phi(0.555) \approx 0.711$

**P364.** (2-mark) $X$ has PDF $f(x) = ce^{-|x|}$. Find $c$, $E[X]$, $\text{Var}(X)$.
→ $\int_{-\infty}^{\infty} ce^{-|x|}dx = 2c = 1$ → $c = 1/2$ (Laplace distribution with $b=1$, $\mu = 0$)
→ $E[X] = 0$ (symmetric). $E[X^2] = \int_0^{\infty}x^2 e^{-x}dx = 2$. $\text{Var}(X) = 2$.

**P365.** (2-mark NAT) $X$ has CDF $F(x) = 1 - (1+x)e^{-x}$ for $x \geq 0$. $E[X]$?
→ $f(x) = F'(x) = -(e^{-x}) + (1+x)e^{-x} = xe^{-x}$ → $\text{Gamma}(2, 1)$
→ $E[X] = 2$

**P366.** (2-mark) A fair coin is tossed repeatedly. $E[\text{tosses until first HH}]$?
→ Let $E_0$ = expected from start, $E_H$ = expected given last was H.
→ $E_0 = 1 + 0.5 E_H + 0.5 E_0$ → $0.5 E_0 = 1 + 0.5 E_H$ → $E_0 = 2 + E_H$
→ $E_H = 1 + 0.5 \cdot 0 + 0.5 E_0 = 1 + 0.5 E_0$
→ $E_0 = 2 + 1 + 0.5 E_0 = 3 + 0.5 E_0$ → $E_0 = 6$

**P367.** (2-mark) $n$ balls thrown uniformly into $n$ bins. $P(\text{bin 1 empty})$?
→ $((n-1)/n)^n \to 1/e \approx 0.368$

**P368.** (2-mark) Random variable $X$: $P(X = k) = (1/2)^k$ for $k = 1, 2, 3, \ldots$ $E[2^X]$?
→ $E[2^X] = \sum_{k=1}^{\infty} 2^k (1/2)^k = \sum_{k=1}^{\infty} 1 = \infty$!

**P369-P400** — Quick Round:

| # | Q | A |
|---|---|---|
| P369 | $E[\text{Hypergeometric}]$ = ? | $nK/N$ (same as Binomial with $p = K/N$) |
| P370 | $\text{Var(Hypergeometric)}$ vs Binomial? | Multiplied by $(N-n)/(N-1)$ (finite pop correction) |
| P371 | $X \sim \text{Neg Binomial}(r,p)$. MGF? | $(pe^t/(1-qe^t))^r$ for $t < -\ln q$ |
| P372 | Logistic distribution: CDF? | $F(x) = 1/(1+e^{-x})$ = sigmoid! |
| P373 | Weibull: hazard increasing with $k > 1$? | YES (aging effect) |
| P374 | Weibull $k = 1$? | Exponential (constant hazard, no aging) |
| P375 | Pareto distribution: power law? | $P(X > x) = (x_m/x)^\alpha$ for $x \geq x_m$ |
| P376 | Heavy-tailed: finite mean but infinite variance? | Pareto with $1 < \alpha < 2$ |
| P377 | Cauchy distribution: mean exists? | NO! (neither mean nor variance) |
| P378 | Cauchy = ratio of i.i.d. normals? | YES: $X/Y$ where $X,Y \sim N(0,1)$ |
| P379 | Stable distributions: CLT generalizes to? | Stable laws (Lévy, Cauchy, Normal are examples) |
| P380 | Multivariate t-distribution? | Heavier tails than multivariate normal |
| P381 | Wishart distribution? | Distribution of $\sum X_iX_i^T$ for multivariate normal samples |
| P382 | Hotelling $T^2$? | Multivariate generalization of t-test |
| P383 | Mahalanobis distance for outlier detection? | $d^2 = (x-\mu)^T\Sigma^{-1}(x-\mu)$ |
| P384 | Copula: models? | Dependence structure separately from marginals |
| P385 | Quantile function: $Q(p) = F^{-1}(p)$? | Value below which $p$ fraction of data falls |
| P386 | Median = $Q(0.5)$? | YES |
| P387 | IQR = $Q(0.75) - Q(0.25)$? | Interquartile range |
| P388 | Box plot shows? | Median, Q1, Q3, whiskers, outliers |
| P389 | Skewness $> 0$ means? | Right-skewed (long right tail), mean > median |
| P390 | Kurtosis $> 3$ means? | Leptokurtic (heavier tails than normal) |
| P391 | Normal has kurtosis? | 3 (or excess kurtosis = 0) |
| P392 | Poisson limit theorem: conditions? | $n$ large, $p$ small, $np \to \lambda$ |
| P393 | De Moivre-Laplace: Binomial → Normal when? | $np \geq 5$ and $n(1-p) \geq 5$ |
| P394 | Continuity correction? | Add $\pm 0.5$ when approximating discrete with continuous |
| P395 | $P(X \leq 10)$ for Bin(20,0.5) with continuity correction? | $P(Z \leq (10.5-10)/\sqrt{5}) = P(Z \leq 0.224) \approx 0.589$ |
| P396 | Empirical CDF: $\hat{F}(x) = ?$ | $\frac{1}{n}\sum I(X_i \leq x)$ |
| P397 | Glivenko-Cantelli: $\sup_x|\hat{F}(x)-F(x)| \to ?$ | $0$ a.s. |
| P398 | DKW inequality? | $P(\sup|\hat{F}-F| > \epsilon) \leq 2e^{-2n\epsilon^2}$ |
| P399 | How many samples for $\epsilon$-accurate CDF? | $n = O(1/\epsilon^2)$ |
| P400 | Two-sample KS test? | $D = \sup|\hat{F}_1 - \hat{F}_2|$ |

**P401-P420** — Matrix & Multivariate Problems:

**P401.** (2-mark) Covariance matrix: $\Sigma = \begin{pmatrix} 4 & 2 \\ 2 & 9 \end{pmatrix}$. Correlation $\rho$?
→ $\rho = 2/\sqrt{4 \times 9} = 2/6 = 1/3$

**P402.** (2-mark) Same $\Sigma$. Eigenvalues?
→ $\lambda^2 - 13\lambda + 32 = 0$ → $\lambda = (13 \pm \sqrt{169-128})/2 = (13 \pm \sqrt{41})/2$
→ $\lambda_1 \approx 9.70$, $\lambda_2 \approx 3.30$

**P403.** (2-mark) First PC direction?
→ Eigenvector for $\lambda_1$: $(4 - 9.70)v_1 + 2v_2 = 0$ → $v_2 = 2.85v_1$ → $(1, 2.85)/\|(1,2.85)\|$

**P404.** (2-mark) Proportion of variance explained by PC1?
→ $9.70/(9.70+3.30) = 9.70/13 \approx 0.746 = 74.6\%$

**P405.** (2-mark) Whitening transform: $W = \Sigma^{-1/2}X$. $\text{Cov}(W) = ?$
→ $I$ (identity). Whitening decorrelates and normalizes variance.

**P406-P420** — Statistical Tests Practice:

| # | Scenario | Test |
|---|---|---|
| P406 | Compare mean to known value, $\sigma$ known | Z-test |
| P407 | Compare mean, $\sigma$ unknown, small $n$ | t-test |
| P408 | Compare two means, independent samples | Two-sample t-test |
| P409 | Compare two means, paired data | Paired t-test |
| P410 | Compare $> 2$ means | One-way ANOVA (F-test) |
| P411 | Compare proportions | Z-test for proportions |
| P412 | Test if data fits distribution | Chi-squared GOF |
| P413 | Test independence (contingency table) | Chi-squared independence |
| P414 | Compare two variances | F-test |
| P415 | Non-parametric 2-sample | Mann-Whitney U |
| P416 | Non-parametric paired | Wilcoxon signed-rank |
| P417 | Non-parametric ANOVA | Kruskal-Wallis |
| P418 | Test normality | Shapiro-Wilk |
| P419 | Test correlation significance | t-test with $r\sqrt{n-2}/\sqrt{1-r^2}$ |
| P420 | Multiple testing (20 tests at $\alpha = 0.05$)? | Use Bonferroni: $\alpha' = 0.05/20 = 0.0025$ |

---

# 📝 Practice Set 6: Distributions & Transforms (P421 – P500)

**P421.** (2-mark) $X \sim N(0,1)$. Derive MGF.
→ $M(t) = E[e^{tX}] = \int_{-\infty}^{\infty} \frac{e^{tx}}{\sqrt{2\pi}}e^{-x^2/2}dx = \int \frac{1}{\sqrt{2\pi}}e^{-(x^2-2tx)/2}dx$
→ Complete square: $x^2 - 2tx = (x-t)^2 - t^2$
→ $= e^{t^2/2}\int \frac{1}{\sqrt{2\pi}}e^{-(x-t)^2/2}dx = e^{t^2/2}$ ✅

**P422.** (2-mark) $X \sim N(\mu, \sigma^2)$. Use MGF to find $E[X^3]$.
→ $M(t) = e^{\mu t + \sigma^2t^2/2}$
→ $M'(t) = (\mu + \sigma^2 t)M(t)$
→ $M''(t) = \sigma^2 M(t) + (\mu+\sigma^2 t)^2 M(t)$
→ $M'''(t)$ at $t=0$: after differentiation, $E[X^3] = \mu^3 + 3\mu\sigma^2$

**P423.** (2-mark) PGF of sum of Poisson: $X \sim \text{Poi}(\lambda_1)$, $Y \sim \text{Poi}(\lambda_2)$, independent. PGF of $X+Y$?
→ $G_{X+Y}(z) = G_X(z)G_Y(z) = e^{\lambda_1(z-1)}e^{\lambda_2(z-1)} = e^{(\lambda_1+\lambda_2)(z-1)}$
→ $X + Y \sim \text{Poi}(\lambda_1+\lambda_2)$ ✅

**P424.** (2-mark) $X \sim \text{Geometric}(p)$. $E[X]$ using PGF.
→ $G(z) = pz/(1-qz)$. $G'(z) = p/(1-qz)^2$. $G'(1) = p/(1-q)^2 = p/p^2 = 1/p$ ✅

**P425-P460** — Transform Problems:

| # | Q | A |
|---|---|---|
| P425 | MGF of Exp($\lambda$) at $t = 0$? | $1$ (always! $M(0) = 1$) |
| P426 | $M'(0)$ for Exp($\lambda$)? | $E[X] = 1/\lambda$ |
| P427 | $M''(0)$ for Exp($\lambda$)? | $E[X^2] = 2/\lambda^2$ |
| P428 | CF of $N(0,1)$? | $e^{-t^2/2}$ |
| P429 | CF always exists? | YES (bounded integrand) |
| P430 | MGF always exists? | NO (may diverge) |
| P431 | Cumulant generating function? | $K(t) = \ln M(t)$; $K'(0) = \mu$, $K''(0) = \sigma^2$ |
| P432 | Cumulants of Normal? | $\kappa_1 = \mu$, $\kappa_2 = \sigma^2$, all others $= 0$! |
| P433 | Cumulants of Poisson? | ALL cumulants $= \lambda$! |
| P434 | Sum of $n$ i.i.d.: MGF? | $[M(t)]^n$ |
| P435 | Max of 2 Exp(1): CDF? | $F(x) = (1-e^{-x})^2$ |
| P436 | PDF of max? | $f(x) = 2(1-e^{-x})e^{-x}$ |
| P437 | $E[\max]$ for 2 Exp(1)? | $3/2$ (from order stats) |
| P438 | Min of 3 Exp(2)? | Exp(6), $E = 1/6$ |
| P439 | $Z = X/Y$ for independent: distribution? | $f_Z(z) = \int |y|f_X(zy)f_Y(y)dy$ |
| P440 | $X + Y$ for independent: convolution? | $f_{X+Y}(z) = (f_X * f_Y)(z)$ |
| P441 | Moment problem: do moments determine distribution? | For bounded RVs: YES. In general: usually yes (Carleman) |
| P442 | Log-normal moments? | $E[X^k] = e^{k\mu + k^2\sigma^2/2}$ |
| P443 | Tail behavior of Normal vs Exponential? | Normal: $e^{-x^2/2}$; Exp: $e^{-x}$. Normal thinner tails! |
| P444 | Sub-Gaussian? | $P(|X| > t) \leq ce^{-t^2/C}$; tail like Normal |
| P445 | Sub-Exponential? | $P(|X| > t) \leq ce^{-t/C}$; heavier than sub-Gaussian |
| P446 | Bernstein's inequality? | Tail bound for bounded RVs (sharper than Hoeffding) |
| P447 | McDiarmid's inequality? | Concentration for functions of independent RVs |
| P448 | VC dimension relates to? | Sample complexity in learning theory |
| P449 | PAC learning sample bound? | $n \geq \frac{1}{\epsilon}(\ln\frac{1}{\delta} + d\ln\frac{1}{\epsilon})$ roughly |
| P450 | Rademacher complexity? | Measures richness of function class |
| P451 | Union bound ($P(\cup A_i) \leq ?$) | $\sum P(A_i)$ (Boole's inequality) |
| P452 | Bonferroni: tighter than union bound? | YES (includes subtraction terms) |
| P453 | $E[\max_{i=1}^n X_i]$ for i.i.d. $N(0,1)$? | $\Theta(\sqrt{\log n})$ |
| P454 | $E[\max]$ for i.i.d. Exp(1)? | $H_n = \sum 1/k \approx \ln n$ |
| P455 | Extreme value theory: max of i.i.d. normals → ? | Gumbel distribution |
| P456 | Max of Exp → ? | Gumbel |
| P457 | Max of Uniform → ? | Weibull (Type III extreme value) |
| P458 | Fisher-Tippett theorem? | Max of i.i.d. → one of 3 types (Gumbel, Fréchet, Weibull) |
| P459 | Multivariate delta method? | $g(\bar{X}) \approx N(g(\mu), \nabla g^T \Sigma \nabla g / n)$ |
| P460 | Profile likelihood? | Maximize over nuisance parameters at each $\theta$ value |

**P461-P500** — Final Problems:

**P461.** (2-mark) A lab test has 5% false positive rate and 1% false negative rate. Disease prevalence is 0.1%. P(disease | positive)?
→ $\frac{0.001 \times 0.99}{0.001 \times 0.99 + 0.999 \times 0.05} = \frac{0.00099}{0.00099 + 0.04995} = \frac{0.00099}{0.05094} \approx 0.0194 \approx 2\%$
→ Even with very accurate test, low prevalence makes positive predictive value low!

**P462.** (2-mark) Random graph $G(n,p)$: each edge present w.p. $p$. $E[\text{triangles}]$?
→ $\binom{n}{3}p^3$ (each triple of vertices forms triangle w.p. $p^3$)

**P463.** (2-mark) Matching problem: $n$ letters to $n$ envelopes randomly. $E[\text{correct matchings}] = 1$. $\text{Var}[\text{correct matchings}]$?
→ $\text{Var} = 1$ (for any $n \geq 2$!). Each indicator has variance $1/n - 1/n^2$, pair covariance adds to total 1.

**P464.** (2-mark) Stirling's approximation: $n! \approx ?$
→ $\sqrt{2\pi n}(n/e)^n$

**P465.** (2-mark) Entropy of Geometric($p$)?
→ $H = -\frac{(1-p)\ln(1-p) + p\ln p}{p}$ (in nats) or divide by $\ln 2$ for bits

**P466-P500**:

| # | Q | A |
|---|---|---|
| P466 | Differential entropy of $N(\mu, \sigma^2)$? | $\frac{1}{2}\ln(2\pi e\sigma^2)$ |
| P467 | Differential entropy of Uniform(a,b)? | $\ln(b-a)$ |
| P468 | Differential entropy of Exp($\lambda$)? | $1 + \ln(1/\lambda) = 1 - \ln\lambda$ (in nats) |
| P469 | Max entropy continuous on $[a,b]$? | Uniform (bounded support) |
| P470 | Max entropy with mean $\mu$, support $[0,\infty)$? | Exponential |
| P471 | Max entropy with mean $\mu$, variance $\sigma^2$? | Normal (on $\mathbb{R}$) |
| P472 | Data processing inequality? | $X \to Y \to Z$ → $I(X;Z) \leq I(X;Y)$ |
| P473 | Fano's inequality? | $H(X|Y) \leq H(P_e) + P_e \log(|X|-1)$ |
| P474 | Channel capacity? | $C = \max_{p(x)} I(X;Y)$ |
| P475 | BSC capacity? | $C = 1 - H(p)$ bits (binary symmetric channel) |
| P476 | Joint entropy $\leq$ sum of marginals? | YES: $H(X,Y) \leq H(X) + H(Y)$, equality iff independent |
| P477 | Conditional entropy $\leq$ unconditional? | YES: $H(X|Y) \leq H(X)$, conditioning reduces entropy |
| P478 | MLE for Geometric($p$): $\hat{p} = ?$ | $1/\bar{X}$ |
| P479 | MLE for Normal variance: biased by factor? | $\frac{n-1}{n}$ (MLE = $\frac{1}{n}\sum(X_i-\bar{X})^2$) |
| P480 | Unbiased estimator of $\sigma^2$? | $S^2 = \frac{1}{n-1}\sum(X_i-\bar{X})^2$ |
| P481 | MSE of biased MLE vs unbiased $S^2$? | MLE has LOWER MSE! (bias-variance tradeoff) |
| P482 | Consistent estimator: definition? | $\hat\theta_n \xrightarrow{P} \theta$ as $n \to \infty$ |
| P483 | Sample median consistent for median? | YES |
| P484 | Asymptotic relative efficiency? | Ratio of variances of two estimators |
| P485 | ARE of median vs mean for Normal? | $\pi/2 \approx 1.57$. Mean is 57% more efficient! |
| P486 | Minimax rate for mean estimation? | $\sigma^2/n$ (achieved by sample mean) |
| P487 | Nonparametric density estimation rate? | $O(n^{-4/5})$ (MISE for optimal bandwidth) |
| P488 | Bandwidth selection for KDE? | Silverman: $h = 1.06\hat\sigma n^{-1/5}$ |
| P489 | Nadaraya-Watson estimator? | Kernel regression: $\hat{m}(x) = \frac{\sum K_h(x-X_i)Y_i}{\sum K_h(x-X_i)}$ |
| P490 | $k$-nearest neighbor density estimate? | $\hat f(x) = k/(n V_k(x))$ where $V_k$ = volume containing $k$ nearest |
| P491 | Mixture model: $f(x) = \sum w_k f_k(x)$? | Weighted sum of component densities |
| P492 | Gaussian mixture model (GMM)? | $f(x) = \sum w_k N(x; \mu_k, \Sigma_k)$ |
| P493 | EM for GMM: E-step? | Compute responsibility: $r_{ik} = w_k N(x_i|\mu_k,\Sigma_k)/f(x_i)$ |
| P494 | EM for GMM: M-step? | Update $\mu_k, \Sigma_k, w_k$ using weighted averages |
| P495 | Number of parameters in GMM ($K$ components, $d$ dims)? | $K(1+d+d(d+1)/2)-1$ |
| P496 | BIC selects $K$ by? | Minimize $-2\ln L + k\ln n$ |
| P497 | Silhouette score for clustering? | Measures how well-separated clusters are |
| P498 | Rand index? | Agreement between two clusterings |
| P499 | Mutual information for clustering? | $I(\text{true labels}; \text{predicted labels})$ |
| P500 | Normalized MI? | $\text{NMI} = 2I/(H_1+H_2)$ |

---

# 📝 Practice Set 7: GATE Predicted & Previous Year Style (P501 – P600)

**P501.** (2-mark) A bag has 6 red, 4 blue balls. 3 balls drawn without replacement. Find $P(X = 2)$ where $X$ = red balls drawn.
→ Hypergeometric: $P(X=2) = \frac{\binom{6}{2}\binom{4}{1}}{\binom{10}{3}} = \frac{15 \times 4}{120} = \frac{60}{120} = 1/2$

**P502.** (2-mark) A coin gives H with prob $p$ and T with prob $1-p$. After observing HHTHT, what is the MLE of $p$?
→ $L(p) = p^3(1-p)^2$. $\ell'(p) = 3/p - 2/(1-p) = 0$ → $3(1-p) = 2p$ → $5p = 3$ → $\hat{p} = 3/5$

**P503.** (2-mark) In a Markov chain with state space $\{0,1,2\}$, the transition matrix is:
$$P = \begin{pmatrix}0.5 & 0.5 & 0 \\ 0.25 & 0.5 & 0.25 \\ 0 & 0.5 & 0.5\end{pmatrix}$$
Find the stationary distribution.
→ $\pi P = \pi$: $0.5\pi_0 + 0.25\pi_1 = \pi_0$ → $0.25\pi_1 = 0.5\pi_0$ → $\pi_1 = 2\pi_0$
→ $0.25\pi_1 + 0.5\pi_2 = \pi_2$ → $\pi_1 = 2\pi_2$ → $\pi_2 = \pi_0$
→ $\pi_0 + 2\pi_0 + \pi_0 = 1$ → $\pi_0 = 1/4$, $\pi_1 = 1/2$, $\pi_2 = 1/4$

**P504.** (2-mark NAT) $X$ has PMF $P(X=k) = \frac{e^{-3}3^k}{k!}$ for non-negative $k$. $P(1 \leq X \leq 3)$?
→ $P = e^{-3}(3 + 9/2 + 27/6) = e^{-3}(3 + 4.5 + 4.5) = 12e^{-3} \approx 12 \times 0.0498 = 0.5972$

**P505.** (2-mark) $X$, $Y$ independent. $X \sim \text{Exp}(1)$, $Y \sim \text{Exp}(2)$. $P(X < Y)$?
→ $P(X < Y) = \int_0^{\infty}\int_x^{\infty} e^{-x} \cdot 2e^{-2y}dy\,dx = \int_0^{\infty} e^{-x}e^{-2x}dx = \int_0^{\infty}e^{-3x}dx = 1/3$
→ Or: competing exponentials: $P(\text{Exp}(\lambda_1) < \text{Exp}(\lambda_2)) = \lambda_1/(\lambda_1+\lambda_2) = 1/3$

**P506-P560** — MCQ Challenge:

| # | Q | Options | Answer |
|---|---|---|---|
| P506 | $X \sim N(5, 4)$. $P(X < 1)$? | (A) 0.023 (B) 0.159 (C) 0.5 | (A): $Z = (1-5)/2 = -2$, $P(Z<-2) = 0.023$ |
| P507 | Variance of sample mean of 16 obs from $N(0,4)$? | (A) 4 (B) 1/4 (C) 1 | (B): $4/16 = 1/4$ |
| P508 | If $E[X] = 2$, $E[Y] = 3$, $E[XY] = 8$, are $X,Y$ independent? | Yes/No | No: $\text{Cov} = 8-6 = 2 \neq 0$ |
| P509 | $P(A) = 0.6$, $P(B) = 0.5$. Can $A,B$ be mutually exclusive? | Yes/No | No: $P(A)+P(B) = 1.1 > 1$ |
| P510 | Mode of Binomial($10, 0.3$)? | 2, 3, or 4? | $\lfloor (n+1)p \rfloor = \lfloor 3.3 \rfloor = 3$ |
| P511 | Sufficient stat for Exp($\theta$) = mean? | $\sum X_i$ or $\bar{X}$? | Both (since $\bar{X}$ is 1-1 function of $\sum X_i$) |
| P512 | UMVUE of $\mu$ for Normal? | $\bar{X}$ | Correct (complete sufficient stat + unbiased) |
| P513 | Method of moments for Gamma($\alpha, \beta$)? | $\hat\alpha = \bar{X}^2/S^2$, $\hat\beta = \bar{X}/S^2$ | Equate first two moments |
| P514 | Anderson-Darling vs KS? | AD better for tails | AD gives more weight to tails |
| P515 | $\chi^2$ test statistic for 2×2 table? | $n(ad-bc)^2/[(a+b)(c+d)(a+c)(b+d)]$ | Quick formula for 2×2 |
| P516 | Fisher's exact test: when? | Small expected counts (< 5) | Use instead of chi-squared |
| P517 | McNemar's test: for? | Paired nominal data | Before/after with binary outcome |
| P518 | Cochran's theorem? | Decomposes $\sum(X_i-\bar{X})^2$ into independent chi-squareds | Foundation of ANOVA |
| P519 | $F_{1,n}$ related to? | $t_n^2$ (F with 1 numerator df = square of t) |
| P520 | Pivotal quantity? | Distribution doesn't depend on parameters | Used for CI construction |

**P521-P560** — More Problems:

| # | Q | A |
|---|---|---|
| P521 | $X_1, X_2$ i.i.d. $N(\mu, 1)$. Sufficient stat? | $\bar{X} = (X_1+X_2)/2$ |
| P522 | Rao-Blackwell: condition on sufficient stat → ? | Reduces variance |
| P523 | Ancillary statistic? | Distribution doesn't depend on $\theta$ |
| P524 | Basu's theorem? | Complete sufficient + ancillary → independent |
| P525 | $\bar{X}$ and $S^2$ independent for Normal? | YES (Basu's theorem) |
| P526 | $T = \bar{X}/(S/\sqrt{n}) \sim ?$ | $t_{n-1}$ |
| P527 | $\chi^2_m/m$ divided by $\chi^2_n/n$ (independent)? | $F_{m,n}$ |
| P528 | One-way ANOVA: $F = ?$ | $\frac{MS_{between}}{MS_{within}}$ |
| P529 | Total SS partition: $SS_T = ?$ | $SS_B + SS_W$ (between + within) |
| P530 | Reject $H_0$ in ANOVA if F > ? | $F_{\alpha, k-1, N-k}$ |
| P531 | Post-hoc test after ANOVA? | Tukey HSD, Scheffé, Bonferroni |
| P532 | Two-way ANOVA tests? | Main effects A, B + interaction AB |
| P533 | Interaction significant means? | Effect of A depends on level of B |
| P534 | Regression: $\hat{Y} = \hat\beta_0 + \hat\beta_1 X$. $\sum(Y_i - \hat{Y}_i) = ?$ | $0$ (residuals sum to 0, always) |
| P535 | Residual plot: pattern means? | Model assumption violated |
| P536 | Cook's distance? | Influence measure: effect of deleting observation $i$ |
| P537 | Leverage: $(h_{ii})$ high means? | Observation far from mean of $X$ |
| P538 | Studentized residual? | $e_i/(S\sqrt{1-h_{ii}})$ — follows $t_{n-p-1}$ under $H_0$ |
| P539 | Multiple regression: $Y = X\beta + \epsilon$. $\hat\beta = ?$ | $(X^TX)^{-1}X^TY$ (OLS) |
| P540 | Gauss-Markov: BLUE requires? | $E[\epsilon] = 0$, $\text{Var}(\epsilon) = \sigma^2I$, linear model |
| P541 | Ridge regression: $\hat\beta = ?$ | $(X^TX + \lambda I)^{-1}X^TY$ |
| P542 | LASSO: $\hat\beta$ minimizes? | $\|Y-X\beta\|^2 + \lambda\|\beta\|_1$ |
| P543 | LASSO produces? | Sparse solutions (some $\beta_j = 0$) |
| P544 | Ridge vs LASSO: which does variable selection? | LASSO |
| P545 | Elastic Net? | $\lambda_1\|\beta\|_1 + \lambda_2\|\beta\|_2^2$ |
| P546 | Logistic regression: link function? | $\log\frac{p}{1-p} = X\beta$ (logit link) |
| P547 | GLM: three components? | Random component, systematic, link function |
| P548 | Deviance in GLM? | $-2(\ell_{fitted} - \ell_{saturated})$ |
| P549 | AIC = $-2\ell + 2k$ minimization → ? | Balances fit and complexity |
| P550 | Leave-one-out CV estimate? | $\frac{1}{n}\sum L(Y_i, \hat{f}_{-i}(X_i))$ |
| P551 | Bias of LOOCV? | Low bias, high variance |
| P552 | $k$-fold CV: standard $k$? | 5 or 10 |
| P553 | Bootstrap: standard error estimate? | $SE = \sqrt{\frac{1}{B-1}\sum(\hat\theta_b - \bar{\hat\theta})^2}$ |
| P554 | Bootstrap CI: percentile method? | $(\hat\theta^*_{\alpha/2}, \hat\theta^*_{1-\alpha/2})$ |
| P555 | BCa bootstrap? | Bias-corrected and accelerated — most accurate |
| P556 | Permutation test: idea? | Shuffle labels, recompute statistic, compare |
| P557 | Kaplan-Meier estimator: for? | Survival function with censored data |
| P558 | Log-rank test? | Compare survival curves between groups |
| P559 | Cox proportional hazards? | $h(t|X) = h_0(t)\exp(X\beta)$ — semi-parametric |
| P560 | Hazard ratio interpretation? | HR = 2: rate of event is 2× in treatment vs control |

**P561-P600** — Computation Express:

| # | Q | A |
|---|---|---|
| P561 | $X \sim \text{Poi}(2)$. $P(X = 0)$? | $e^{-2} \approx 0.135$ |
| P562 | $P(X \leq 1)$? | $e^{-2}(1+2) = 3e^{-2} \approx 0.406$ |
| P563 | $E[X^3]$ for Poi($\lambda$)? | $\lambda^3 + 3\lambda^2 + \lambda$ |
| P564 | Geometric: $P(X > 10)$ for $p = 0.1$? | $(1-p)^{10} = 0.9^{10} \approx 0.349$ |
| P565 | Negative Binomial: $r = 3$, $p = 0.5$. $P(X = 6)$? | $\binom{5}{2}(0.5)^3(0.5)^3 = 10/64 = 5/32$ |
| P566 | Multinomial: 3 categories, 5 trials. $P(2,2,1)$ with $p = (0.3,0.5,0.2)$? | $\frac{5!}{2!2!1!}(0.3)^2(0.5)^2(0.2) = 30 \times 0.09 \times 0.25 \times 0.2 = 0.135$ |
| P567 | $\chi^2_5$: mean and variance? | $E = 5$, $\text{Var} = 10$ |
| P568 | $t_{10}$ vs $N(0,1)$: which has wider 95% CI? | $t_{10}$ (heavier tails; $t_{0.025, 10} = 2.228 > 1.96$) |
| P569 | $F_{5,10}$ at 5% critical? | $F_{0.05,5,10} \approx 3.33$ |
| P570 | Power of test: increase $n$ →? | Power increases (easier to detect difference) |
| P571 | Increase $\alpha$ →? | Power increases (but more Type I error) |
| P572 | Increase effect size →? | Power increases |
| P573 | Two-sided test: $H_0: \mu = 100$, reject if $|Z| > ?$ at 5%? | $1.96$ |
| P574 | One-sided: reject if $Z > ?$ at 5%? | $1.645$ |
| P575 | Sample size for proportion: margin $E = 0.03$, $\hat{p} = 0.5$? | $n = z^2\hat{p}(1-\hat{p})/E^2 = (1.96)^2(0.25)/(0.0009) \approx 1068$ |
| P576 | Normal approx to Binomial: $P(X \leq 55)$ for $B(100, 0.5)$? | $Z = (55.5-50)/5 = 1.1$ (with cont. correction). $P \approx 0.864$ |
| P577 | Poisson approx: $B(1000, 0.002)$, $P(X = 3)$? | $\lambda = 2$, $P = 8e^{-2}/6 = 4e^{-2}/3 \approx 0.180$ |
| P578 | Exponential: median? | $\ln 2/\lambda$ (since $e^{-\lambda m} = 0.5$) |
| P579 | Exponential: $P(X > E[X])$? | $e^{-1} \approx 0.368$ |
| P580 | Normal: $P(X > \mu + 3\sigma)$? | $0.0013$ (99.7 rule) |
| P581 | Process capability: $C_p = ?$ | $(USL - LSL)/(6\sigma)$ |
| P582 | Six Sigma means? | $C_p = 2$; 3.4 defects per million |
| P583 | Control chart: $\bar{X}$ chart limits? | $\bar{\bar{X}} \pm 3\sigma/\sqrt{n}$ |
| P584 | $R$-chart monitors? | Process variability (range) |
| P585 | OC curve? | Operating Characteristic: $P(\text{accept}) = f(\text{true proportion defective})$ |
| P586 | Bayes factor: $BF_{10} = ?$ | $P(\text{data}|H_1)/P(\text{data}|H_0)$ |
| P587 | $BF > 10$: evidence? | Strong evidence for $H_1$ |
| P588 | Credible interval: 95% highest posterior density? | Shortest interval containing 95% posterior probability |
| P589 | Informative vs non-informative prior? | Informative: encodes strong belief; Non-informative: "let data speak" |
| P590 | Flat prior for Bernoulli $p$? | Beta(1,1) = Uniform(0,1) |
| P591 | Conjugate prior advantage? | Posterior in closed form — no MCMC needed |
| P592 | Posterior predictive check? | Simulate data from posterior, compare to real data |
| P593 | WAIC? | Widely Applicable Information Criterion (Bayesian AIC) |
| P594 | LOO-CV for Bayesian? | $\text{elpd} = \sum \log p(y_i|\text{data}_{-i})$ |
| P595 | Bayesian model averaging? | Weight models by posterior probability |
| P596 | Empirical Bayes: hybrid? | Estimate prior hyperparameters from data, then do Bayes |
| P597 | Stein's phenomenon? | MLE inadmissible for $\geq 3$ normal means |
| P598 | James-Stein shrinks toward? | Grand mean (or any fixed point) |
| P599 | Shrinkage in ridge regression? | Coefficients pulled toward 0 |
| P600 | Bias-variance optimal: risk = $n^{-2\beta/(2\beta+d)}$? | Nonparametric minimax rate (curse of dimensionality!) |

---

## 📋 Error Type Decision Matrix

| Reality → Decision ↓ | $H_0$ TRUE | $H_0$ FALSE |
|---|---|---|
| **Don't reject $H_0$** | ✅ Correct | ❌ Type II ($\beta$) |
| **Reject $H_0$** | ❌ Type I ($\alpha$) | ✅ Correct (Power $= 1-\beta$) |

Remember: Type I = **I** falsely reject = False Positive. Type II = **II** = fail to reject false = False Negative.

Analogy: Court trial. $H_0$: innocent.
- Type I: Convict an innocent person (very bad!)
- Type II: Let a guilty person go free (bad but less severe)
- $\alpha = 0.05$: willing to wrongly convict 5% of the time

---

## 📋 Revision Roadmap — 4-Week Plan

### Week 1: Probability Foundations (Days 1-7)
- Day 1-2: Sample space, events, axioms, conditional probability (Sections 1.1-1.3)
- Day 3-4: Bayes' theorem, total probability + 30 problems
- Day 5-6: Random variables, PMF, CDF, expectation, variance (Sections 1.7-1.11)
- Day 7: Practice Set 1 (P1-P80), review all mistakes

### Week 2: Distributions & Joint (Days 8-14)
- Day 8-9: Discrete distributions (Binomial, Poisson, Geometric) + calculations
- Day 10-11: Continuous distributions (Normal, Exponential, Gamma) + CLT
- Day 12-13: Joint distributions, covariance, correlation, conditional expectation
- Day 14: Practice Set 2 (P81-P160), timed GATE simulation

### Week 3: Statistics & Inference (Days 15-21)
- Day 15-16: MLE, sufficient statistics, Fisher information
- Day 17-18: Hypothesis testing, confidence intervals, Type I/II errors
- Day 19-20: Chi-squared tests, t-tests, ANOVA
- Day 21: Practice Sets 3-5 (P161-P420), identify weak spots

### Week 4: Advanced & Review (Days 22-28)
- Day 22-23: Markov chains, Poisson process, information theory
- Day 24-25: GATE PYQ practice (all years), timed
- Day 26-27: Formula card review, rapid recall, speed drills
- Day 28: Full mock test, final review of traps & confused pairs

---

## 🎪 GATE 2025-2027 Predicted Hot Topics

| # | Topic | Prediction | Prep Priority |
|---|---|---|---|
| 1 | Bayes' theorem (machine / disease / test) | 99% chance | 🔴 MUST DO |
| 2 | Expectation using linearity / indicators | 95% | 🔴 MUST DO |
| 3 | Normal distribution standardization | 90% | 🔴 MUST DO |
| 4 | Conditional probability (urn / cards) | 85% | 🔴 MUST DO |
| 5 | Variance of linear combinations | 80% | 🟡 HIGH |
| 6 | Poisson distribution calculation | 75% | 🟡 HIGH |
| 7 | CLT application | 70% | 🟡 HIGH |
| 8 | MLE computation | 65% | 🟡 HIGH |
| 9 | Markov chains (DA-specific) | 60% | 🟡 MEDIUM |
| 10 | Hypothesis testing | 55% | 🟡 MEDIUM |

---

# 📝 Practice Set 8: Complete GATE Marathon (P601 – P750)

**P601.** (2-mark) Five letters addressed to 5 different persons are put randomly into 5 envelopes. What is the probability that at least one letter is in the correct envelope?
→ Complement: P(no letter correct) = $D_5/5! = (5!)(1 - 1 + 1/2 - 1/6 + 1/24)/5!$
→ $D_5 = 44$. $P(\text{no match}) = 44/120 = 11/30$
→ $P(\text{at least 1 correct}) = 1 - 11/30 = 19/30$

**P602.** (2-mark) A system has 3 components in series. Lifetimes are independent Exp with rates 1, 2, 3. System lifetime distribution?
→ Series: min of components. $\min(\text{Exp}(1), \text{Exp}(2), \text{Exp}(3)) = \text{Exp}(6)$
→ $E[\text{system lifetime}] = 1/6$

**P603.** (2-mark) Parallel system (works if ANY works): 2 components Exp(1). Lifetime distribution?
→ $P(\max > t) = 1 - P(\text{both fail by } t) = 1 - (1-e^{-t})^2 = 2e^{-t} - e^{-2t}$
→ $E[\text{lifetime}] = 2 \times 1 - 1/2 = 3/2$

**P604.** (2-mark) $n = 400$ voters, 220 support candidate A. Test $H_0: p = 0.5$ vs $H_1: p > 0.5$ at $\alpha = 0.05$.
→ $\hat{p} = 0.55$. $Z = (0.55 - 0.5)/\sqrt{0.5 \times 0.5/400} = 0.05/0.025 = 2.0$
→ $z_{0.05} = 1.645$. Since $2.0 > 1.645$: **Reject $H_0$**. Significant support.

**P605.** (2-mark) Bayesian update: Prior $\theta \sim \text{Beta}(2, 2)$. Observe 3 heads in 5 tosses. Posterior?
→ $\text{Beta}(2+3, 2+2) = \text{Beta}(5, 4)$
→ Posterior mean: $5/9 \approx 0.556$

**P606.** (2-mark) Random walk: start at 3, step +1 (p=0.6) or -1 (p=0.4). P(reach 5 before 0)?
→ Gambler's ruin with $p \neq 0.5$: $r = q/p = 2/3$.
→ $P(\text{reach 5} | \text{start 3}) = \frac{1 - r^3}{1 - r^5} = \frac{1 - 8/27}{1 - 32/243} = \frac{19/27}{211/243} = \frac{19 \times 9}{211} = \frac{171}{211} \approx 0.810$

**P607.** (2-mark) 10 people seated randomly in a row. P(tallest and shortest not adjacent)?
→ Total arrangements: $10!$. Adjacent: treat as block: $2 \times 9!$
→ $P(\text{adjacent}) = 2 \times 9!/10! = 2/10 = 1/5$
→ $P(\text{not adjacent}) = 4/5$

**P608.** (2-mark NAT) $X$ and $Y$ jointly normal with $\mu_X = 1$, $\mu_Y = 2$, $\sigma_X = 2$, $\sigma_Y = 3$, $\rho = 0.5$. $E[Y | X = 3]$?
→ $E[Y|X=x] = \mu_Y + \rho\frac{\sigma_Y}{\sigma_X}(x - \mu_X) = 2 + 0.5 \times \frac{3}{2}(3-1) = 2 + 1.5 = 3.5$

**P609.** (2-mark) $X \sim \text{Uniform}\{1,2,\ldots,n\}$. $E[1/X]$?
→ $E[1/X] = \frac{1}{n}\sum_{k=1}^n \frac{1}{k} = \frac{H_n}{n}$ where $H_n$ = harmonic number.
→ For $n = 6$: $(1 + 1/2 + 1/3 + 1/4 + 1/5 + 1/6)/6 = 2.45/6 \approx 0.408$

**P610.** (2-mark) Balls and urns: 5 balls into 3 urns, each urn getting at least one ball. Number of ways (distinguishable balls, distinguishable urns)?
→ Inclusion-exclusion: $3^5 - \binom{3}{1}2^5 + \binom{3}{2}1^5 = 243 - 96 + 3 = 150$
→ These are **surjections** from 5-set to 3-set.

**P611-P650** — Speed Problems:

| # | Q | A |
|---|---|---|
| P611 | $\binom{10}{3}$? | $120$ |
| P612 | $P_{10,3}$? | $720$ |
| P613 | Stars and bars: $x_1+x_2+x_3 = 10$, $x_i \geq 0$? | $\binom{12}{2} = 66$ |
| P614 | Same with $x_i \geq 1$? | $\binom{9}{2} = 36$ |
| P615 | Multinomial: $\frac{10!}{3!3!4!}$? | $4200$ |
| P616 | Catalan number $C_3$? | $5$ (number of valid parenthesizations) |
| P617 | $n$th Catalan: $C_n = \frac{1}{n+1}\binom{2n}{n}$? | $C_4 = \frac{1}{5}\binom{8}{4} = 14$ |
| P618 | Expected value of $|X|$ for Laplace(0,1)? | $E[|X|] = 1$ (by integration) |
| P619 | Entropy of Bernoulli(0.5) in bits? | $1$ bit (maximum) |
| P620 | Entropy of Bernoulli(0.01)? | $\approx 0.081$ bits (very low uncertainty) |
| P621 | KL divergence from Bernoulli(0.3) to Bernoulli(0.5)? | $0.3\ln(0.3/0.5) + 0.7\ln(0.7/0.5) = 0.3\ln 0.6 + 0.7\ln 1.4$ |
| P622 | MI between X and Y = X + N where N independent noise? | $I(X;Y) = H(Y) - H(Y|X) = H(Y) - H(N)$ |
| P623 | Poisson process: given 5 arrivals in [0,10], conditional distribution of each? | Each $\sim \text{Uniform}(0,10)$ independently |
| P624 | Brownian motion: $P(B(1) > 0)$? | $0.5$ (symmetric) |
| P625 | $P(\max_{0 \leq t \leq 1} B(t) > a)$ for $a > 0$? | $2P(B(1) > a) = 2(1-\Phi(a))$ (reflection principle) |
| P626 | Simple random walk: $E[S_n^2]$? | $n$ (since $E[S_n] = 0$, $\text{Var}(S_n) = n$) |
| P627 | Ballot problem: A gets $a$ votes, B gets $b$ ($a > b$). P(A always ahead)? | $(a-b)/(a+b)$ |
| P628 | Stirling numbers of 2nd kind $S(n,k)$? | Number of partitions of $n$-set into $k$ nonempty subsets |
| P629 | Bell number $B_n$? | Total partitions: $B_n = \sum_{k=0}^n S(n,k)$ |
| P630 | Generating function for Poisson PMF? | $G(z) = e^{\lambda(z-1)}$ |
| P631 | Cumulant: $\kappa_3$ of Poisson($\lambda$)? | $\lambda$ (all cumulants equal $\lambda$!) |
| P632 | Skewness of Poisson? | $1/\sqrt{\lambda}$ (approaches 0 as $\lambda \to \infty$) |
| P633 | Kurtosis of Poisson? | $3 + 1/\lambda$ (excess kurtosis = $1/\lambda$) |
| P634 | Variance stabilizing transform for Poisson? | $\sqrt{X}$ (makes variance $\approx$ constant) |
| P635 | For Binomial? | $\arcsin\sqrt{X/n}$ |
| P636 | Fisher info for Poisson? | $I(\lambda) = 1/\lambda$ |
| P637 | CRLB for Poisson mean estimator? | $\text{Var} \geq \lambda/n$ |
| P638 | $\bar{X}$ achieves CRLB for Poisson? | YES ($\text{Var}(\bar{X}) = \lambda/n$) |
| P639 | MLE for $\lambda$ in Poisson? | $\hat\lambda = \bar{X}$ |
| P640 | Score function $U(\theta) = ?$ | $d\ell/d\theta$ — derivative of log-likelihood |
| P641 | $E[U(\theta)] = ?$ | $0$ (at true parameter) |
| P642 | $\text{Var}(U(\theta)) = ?$ | $I(\theta)$ = Fisher information! |
| P643 | Score test (Rao test)? | Evaluate score at $H_0$ value |
| P644 | Wald test? | $(\hat\theta - \theta_0)^2/\text{Var}(\hat\theta) \sim \chi^2_1$ |
| P645 | LRT statistic? | $-2\ln(\Lambda) \sim \chi^2_k$ approximately |
| P646 | Which is most powerful for simple hypotheses? | LRT (Neyman-Pearson lemma) |
| P647 | UMP test: exists when? | One-parameter exponential family, one-sided |
| P648 | UMP doesn't exist for two-sided because? | Need different rejection regions for different alternatives |
| P649 | Unbiased test? | Power $\geq \alpha$ for all $\theta \in H_1$ |
| P650 | UMPU? | Uniformly most powerful unbiased — exists for exp family, two-sided |

**P651-P700** — Advanced Distributions & Information Theory:

**P651.** (2-mark) $X \sim \text{Gamma}(\alpha, \beta)$. If $Y = cX$, distribution of $Y$?
→ $Y \sim \text{Gamma}(\alpha, \beta/c)$. Scaling changes rate parameter.

**P652.** (2-mark) $X_1 \sim \chi^2_m$, $X_2 \sim \chi^2_n$ independent. $\frac{X_1/m}{X_2/n} \sim ?$
→ $F_{m,n}$ distribution

**P653.** (2-mark) $X \sim t_n$. $X^2 \sim ?$
→ $F_{1,n}$

**P654.** (2-mark) If $Z \sim N(0,1)$ and $V \sim \chi^2_n$ independent, $T = Z/\sqrt{V/n} \sim ?$
→ $t_n$ (definition of t-distribution)

**P655.** (2-mark) Noncentral chi-squared: $\sum (X_i + \mu_i)^2$ where $X_i$ i.i.d. $N(0,1)$.
→ Noncentrality parameter $\lambda = \sum \mu_i^2$. Mean = $n + \lambda$.

**P656.** (2-mark) In power analysis, noncentrality parameter increases with?
→ Effect size, sample size → larger NCP → higher power

**P657-P700** — Quick Fire:

| # | Q | A |
|---|---|---|
| P657 | Exponential family: $f(x|\theta) = h(x)\exp(\eta T - A(\eta))$. $E[T] = ?$ | $A'(\eta)$ |
| P658 | $\text{Var}(T) = ?$ | $A''(\eta)$ |
| P659 | Normal as exp family: $T(x) = ?$ | $(x, x^2)$ (2-parameter) |
| P660 | Canonical link in GLM for Bernoulli? | Logit: $\log(p/(1-p))$ |
| P661 | For Poisson? | Log: $\log\mu$ |
| P662 | For Normal? | Identity: $\mu$ |
| P663 | Deviance residual? | $d_i = \text{sign}(y_i - \hat\mu_i)\sqrt{d_i^2}$ where $d_i^2$ = contribution to deviance |
| P664 | Information criterion comparison: AIC vs BIC? | BIC penalizes more for large $n$ |
| P665 | Consistent model selection? | BIC is consistent; AIC is not |
| P666 | Prediction-optimal? | AIC (asymptotically equivalent to LOOCV) |
| P667 | Mallows' $C_p$? | $SSE_p/\hat\sigma^2 + 2p - n$; minimize for best subset |
| P668 | Stepwise regression: forward? | Start empty, add best predictor one at a time |
| P669 | Backward? | Start full, remove worst predictor |
| P670 | Best subset? | Try all $2^p$ subsets (exponential!) |
| P671 | Regularization path: LASSO? | Piecewise linear in $\lambda$ |
| P672 | LARS algorithm? | Least Angle Regression — efficient LASSO path |
| P673 | Group LASSO? | Penalize groups of variables together |
| P674 | Fused LASSO? | Penalize differences of adjacent coefficients |
| P675 | Total variation denoising? | $\min \|y-\beta\|^2 + \lambda \sum|\beta_i-\beta_{i-1}|$ |
| P676 | Reproducing kernel Hilbert space? | Function space for kernel methods |
| P677 | Representer theorem? | Solution lives in span of kernel evaluations at data |
| P678 | Gaussian process: prior on functions? | $f \sim GP(\mu(x), k(x,x'))$ |
| P679 | GP posterior mean? | $\hat f(x_*) = k_*^T(K+\sigma^2I)^{-1}y$ |
| P680 | GP uncertainty? | Full posterior: $\text{Var}(f_*) = k_{**} - k_*^T(K+\sigma^2I)^{-1}k_*$ |
| P681 | Dirichlet process? | Nonparametric Bayesian: infinite mixture model |
| P682 | Chinese restaurant process? | Seating metaphor for Dirichlet process |
| P683 | Stick-breaking: $\pi_k = V_k\prod_{j<k}(1-V_j)$? | Constructive definition of DP |
| P684 | Indian buffet process? | Nonparametric prior for binary features |
| P685 | Variational inference? | Approximate posterior by minimizing KL divergence |
| P686 | ELBO = ? | Evidence Lower BOund: $\mathcal{L} = E_q[\log p(x,z)] - E_q[\log q(z)]$ |
| P687 | $\log p(x) = \text{ELBO} + ?$ | $D_{KL}(q\|p_{z|x}) \geq 0$ |
| P688 | Mean-field approximation? | $q(z) = \prod_i q_i(z_i)$ (factored) |
| P689 | Stochastic VI? | Subsample data for scalable variational inference |
| P690 | VAE: encoder + decoder? | Encoder: $q(z|x)$; Decoder: $p(x|z)$; optimize ELBO |
| P691 | Reparameterization trick? | $z = \mu + \sigma\epsilon$, $\epsilon \sim N(0,1)$ — enables backprop |
| P692 | Normalizing flows? | Series of invertible transforms for flexible $q(z)$ |
| P693 | Score function estimator (REINFORCE)? | $\nabla E_q[f] = E_q[f\nabla\log q]$ |
| P694 | Control variate? | Reduce variance of MC estimator by subtracting known quantity |
| P695 | Antithetic sampling? | Use $U$ and $1-U$ for variance reduction |
| P696 | Stratified sampling? | Divide space into strata, sample each |
| P697 | Latin hypercube? | Stratified in each marginal dimension |
| P698 | Quasi-Monte Carlo? | Low-discrepancy sequences instead of random |
| P699 | Central limit theorem for MC? | $\hat\mu \pm z_{\alpha/2}\hat\sigma/\sqrt{n}$ after $n$ samples |
| P700 | Effective sample size for MCMC? | $n_{\text{eff}} = n/\tau$ where $\tau$ = integrated autocorrelation time |

**P701-P750** — Final Stretch:

**P701.** (2-mark) A lot of 100 items has 5 defective. Inspector takes sample of 10. P(finding at least 1 defective)?
→ Hypergeometric: $P(X \geq 1) = 1 - P(X=0) = 1 - \frac{\binom{5}{0}\binom{95}{10}}{\binom{100}{10}}$
→ $= 1 - \frac{95!/(85!\cdot 10!)}{100!/(90!\cdot 10!)} = 1 - \frac{90 \times 89 \times \cdots \times 86}{100 \times 99 \times \cdots \times 96} \cdot \frac{95!/(90!\cdot5!)}{1}$
→ Approximate: $P \approx 1 - (0.95)^{10} \approx 1 - 0.5987 = 0.4013$ (binomial approx)
→ Exact hypergeometric: $\approx 0.417$

**P702.** (2-mark) Two players A, B take turns rolling a die. A wins if 6 appears on A's turn. A goes first. P(A wins)?
→ $P(A) = 1/6 + (5/6)(5/6)P(A)$ → $P(A)(1 - 25/36) = 1/6$ → $P(A) = 6/11$

**P703.** (2-mark) Random variable $X$ with $E[X] = 10$, $E[X^2] = 120$. Using Chebyshev, bound $P(X \leq 5 \text{ or } X \geq 15)$.
→ $\text{Var}(X) = 120 - 100 = 20$. $\sigma = \sqrt{20}$.
→ $P(|X-10| \geq 5)$: here $k = 5/\sqrt{20} = 5/(2\sqrt{5}) = \sqrt{5}/2 \approx 1.118$
→ $P \leq 1/k^2 = 4/5 = 0.8$

**P704.** (2-mark NAT) In a family with 2 children, what is P(both boys | at least one boy)?
→ Sample space (equal prob): BB, BG, GB, GG. Given at least one boy: BB, BG, GB (3 outcomes).
→ $P(\text{BB} | \text{at least 1 B}) = 1/3$

**P705.** (2-mark) Cards: 5 cards from 52. P(exactly one pair)?
→ $\frac{\binom{13}{1}\binom{4}{2}\binom{12}{3}\binom{4}{1}^3}{\binom{52}{5}} = \frac{13 \times 6 \times 220 \times 64}{2598960} = \frac{1098240}{2598960} \approx 0.4226$

**P706.** (2-mark) Sample of 50 from normal population: $\bar{x} = 82$, $s = 12$. 90% CI for $\mu$?
→ $t_{0.05, 49} \approx 1.677$ (or use $z = 1.645$ for large $n$)
→ CI: $82 \pm 1.645 \times 12/\sqrt{50} = 82 \pm 2.79 = (79.21, 84.79)$

**P707.** (2-mark) Two samples: $n_1 = 50$, $\bar{x}_1 = 75$, $s_1 = 8$; $n_2 = 60$, $\bar{x}_2 = 72$, $s_2 = 10$. Test $H_0: \mu_1 = \mu_2$ at $\alpha = 0.05$.
→ $Z = \frac{75-72}{\sqrt{64/50 + 100/60}} = \frac{3}{\sqrt{1.28 + 1.667}} = \frac{3}{\sqrt{2.947}} = \frac{3}{1.717} = 1.747$
→ For two-tailed at 0.05: $z_{crit} = 1.96$. Since $1.747 < 1.96$: **Fail to reject $H_0$**.

**P708-P750** — Express Questions:

| # | Q | A |
|---|---|---|
| P708 | 95% CI using $z$: width? | $2 \times 1.96 \times \sigma/\sqrt{n}$ |
| P709 | Double $n$ → CI width? | Divided by $\sqrt{2}$ (not halved!) |
| P710 | Quadruple $n$ → CI width? | Halved |
| P711 | Paired vs unpaired: which has more power? | Paired (removes between-subject variability) |
| P712 | One-tailed vs two-tailed: which more powerful? | One-tailed (lower critical value) |
| P713 | $p$-value = 0.03: reject at $\alpha = 0.05$? | YES |
| P714 | $p$-value = 0.03: reject at $\alpha = 0.01$? | NO |
| P715 | Effect of increasing $\alpha$? | More Type I errors, fewer Type II (more power) |
| P716 | Bonferroni for 5 tests: adjusted $\alpha$? | $0.05/5 = 0.01$ |
| P717 | FDR vs FWER: which less conservative? | FDR (controls proportion, not probability) |
| P718 | Multiple comparisons: $k$ independent tests at $\alpha$. P(at least one false positive)? | $1 - (1-\alpha)^k$ |
| P719 | For $k = 20$, $\alpha = 0.05$: P(≥1 FP)? | $1 - 0.95^{20} \approx 0.64$ (very high!) |
| P720 | Regression: $R^2 = 0.85$ means? | 85% of variance in $Y$ explained by model |
| P721 | $R^2$ always increases with more predictors? | YES → use adjusted $R^2$ instead |
| P722 | Adjusted $R^2$ can decrease? | YES (penalizes unnecessary predictors) |
| P723 | Heteroscedasticity affects? | Standard errors (not point estimates) |
| P724 | White standard errors fix? | Yes (robust to heteroscedasticity) |
| P725 | Autocorrelation in time series? | Use Newey-West standard errors |
| P726 | Durbin-Watson $\approx 2$: no autocorrelation? | YES ($DW \approx 2(1-r)$ where $r$ = 1st order autocorr) |
| P727 | Logistic regression: odds ratio? | $\exp(\beta_j)$ for unit increase in $X_j$ |
| P728 | OR = 2 means? | Odds double for unit increase |
| P729 | Probit model: link? | $\Phi^{-1}(p) = X\beta$ |
| P730 | Probit vs Logit: similar? | Very similar in practice (logit has fatter tails) |
| P731 | Poisson regression for? | Count data: $\log\mu = X\beta$ |
| P732 | Overdispersion in Poisson? | $\text{Var} > \text{Mean}$: use negative binomial or quasi-Poisson |
| P733 | Zero-inflated Poisson? | Many zeros: mixture of point mass at 0 + Poisson |
| P734 | Time series: AR(1) model? | $X_t = \phi X_{t-1} + \epsilon_t$ |
| P735 | Stationary AR(1) requires? | $|\phi| < 1$ |
| P736 | MA(1) model? | $X_t = \epsilon_t + \theta\epsilon_{t-1}$ |
| P737 | ARMA(p,q)? | Combines AR($p$) and MA($q$) |
| P738 | ARIMA: "I" for? | Integrated (differencing for non-stationary) |
| P739 | ACF for AR(1)? | Exponential decay: $\rho_k = \phi^k$ |
| P740 | PACF for AR(1)? | Cuts off after lag 1 |
| P741 | ACF for MA(1)? | Cuts off after lag 1 |
| P742 | PACF for MA(1)? | Exponential decay |
| P743 | Ljung-Box test? | Tests if autocorrelations are collectively zero |
| P744 | ADF test? | Augmented Dickey-Fuller: tests for unit root (non-stationarity) |
| P745 | Granger causality? | $X$ Granger-causes $Y$ if past $X$ helps predict $Y$ |
| P746 | VAR model? | Vector AR: multivariate time series |
| P747 | Exponential smoothing? | $\hat{X}_t = \alpha X_t + (1-\alpha)\hat{X}_{t-1}$ |
| P748 | Holt-Winters? | Exponential smoothing with trend + seasonality |
| P749 | Prophet model? | Additive: trend + seasonality + holidays + noise |
| P750 | Stationarity: mean, variance, autocorrelation constant? | Weakly/covariance stationary |

---

# 📝 Practice Set 9: Conceptual True/False (P751 – P830)

| # | Statement | T/F | Explanation |
|---|---|---|---|
| P751 | $P(A) + P(B) \geq P(A \cup B)$ | T | From inclusion-exclusion |
| P752 | $P(A \cap B) = P(A)P(B)$ always | F | Only if independent |
| P753 | $\text{Var}(X) \geq 0$ always | T | Definition ($E[(X-\mu)^2]$) |
| P754 | $\text{Var}(X) = 0$ iff $X$ is constant | T | Equality case |
| P755 | If $X \leq Y$ always, then $E[X] \leq E[Y]$ | T | Monotonicity of expectation |
| P756 | $E[XY] = E[X]E[Y]$ always | F | Only if uncorrelated (for independent, it suffices) |
| P757 | Uncorrelated → Independent | F | Counterexample: $X \sim N(0,1)$, $Y = X^2$ |
| P758 | For normal: uncorrelated → independent | T | Special property of normal |
| P759 | MLE always unbiased | F | Example: MLE of $\sigma^2$ is biased for normal |
| P760 | MLE always consistent | T | Under regularity conditions |
| P761 | Sufficient stat always exists | T | $X$ itself is always sufficient (trivially) |
| P762 | Minimal sufficient stat is unique | T | Up to 1-1 transformation |
| P763 | $S^2$ is unbiased for $\sigma^2$ | T | By construction ($n-1$ denominator) |
| P764 | $S$ is unbiased for $\sigma$ | F | Jensen's: $E[\sqrt{X}] < \sqrt{E[X]}$ for $X = S^2$ |
| P765 | CLT requires normality of population | F | Works for any distribution with finite variance |
| P766 | CLT works for $n = 30$ always | F | Depends on population shape; 30 is rule of thumb |
| P767 | $t$-distribution = normal distribution | F | $t$ has heavier tails (depends on df) |
| P768 | As $n \to \infty$: $t_n \to N(0,1)$ | T | Yes, exactly |
| P769 | Bayesian CI (credible interval) = Frequentist CI | F | Different interpretation and often different values |
| P770 | Prior doesn't matter with enough data | T | Posterior dominated by likelihood as $n \to \infty$ |
| P771 | Chi-squared test requires large expected counts | T | Each cell $\geq 5$ (rule of thumb) |
| P772 | Fisher exact test works for any sample size | T | That's the point of "exact" |
| P773 | Correlation implies causation | F | Classic fallacy! |
| P774 | Zero correlation implies no relationship | F | Could be nonlinear relationship |
| P775 | $R^2 = 1$ means perfect prediction | T | All variance explained |
| P776 | $R^2 = 0.01$ means model is useless | F | Could still be statistically significant |
| P777 | p-value = P(H₀ is true) | F | p-value = P(data as extreme | H₀ true) |
| P778 | Small p → large effect | F | p depends on sample size too |
| P779 | Large p → H₀ is true | F | Could be low power |
| P780 | Confidence level 95% means P(µ in CI) = 0.95 | F | The 95% refers to the procedure, not a single CI |
| P781 | Exponential distribution is memoryless | T | $P(X > s+t|X>s) = P(X>t)$ |
| P782 | Geometric distribution is memoryless | T | Only discrete memoryless distribution |
| P783 | Normal distribution is memoryless | F | Only Geometric (discrete) and Exp (continuous) |
| P784 | Poisson process has independent increments | T | Defining property |
| P785 | Merge of 2 Poisson processes is Poisson | T | With sum of rates |
| P786 | Thinning of Poisson process is Poisson | T | With rate $\lambda p$ |
| P787 | Stationary distribution always exists for MC | F | Needs positive recurrence |
| P788 | Irreducible + finite state MC has stationary dist | T | Always |
| P789 | Ergodic MC converges to stationary regardless of initial state | T | Definition of ergodic |
| P790 | PageRank is a Markov chain application | T | Stationary dist on web graph |
| P791 | The birthday problem: need $> 365$ people for guaranteed match | F | Need 367 (pigeonhole); but 50% chance at just 23 |
| P792 | If $X \sim N(0,1)$, then $X^2 \sim \chi^2_1$ | T | Definition |
| P793 | Sum of independent $\chi^2_m$ and $\chi^2_n$ is $\chi^2_{m+n}$ | T | Additivity |
| P794 | $\bar{X}$ and $S^2$ are independent for normal populations | T | Cochran's theorem |
| P795 | Bootstrap is parametric | F | Nonparametric (resamples from data) |
| P796 | EM always converges to global max | F | Can converge to local max |
| P797 | Gibbs sampling is a special case of Metropolis-Hastings | T | With acceptance probability 1 |
| P798 | MCMC samples are i.i.d. | F | They're correlated (Markov chain) |
| P799 | Variational inference gives lower bound on evidence | T | ELBO ≤ log P(x) |
| P800 | KL divergence is a metric | F | Not symmetric, no triangle inequality |

**P801-P830** — Computation Speed Round:

| # | Q | A |
|---|---|---|
| P801 | $5! = ?$ | $120$ |
| P802 | $\binom{8}{4} = ?$ | $70$ |
| P803 | $e^{-1} \approx ?$ | $0.3679$ |
| P804 | $e^{-2} \approx ?$ | $0.1353$ |
| P805 | $e^{-3} \approx ?$ | $0.0498$ |
| P806 | $\ln 2 \approx ?$ | $0.693$ |
| P807 | $\sqrt{2\pi} \approx ?$ | $2.507$ |
| P808 | $\Phi(1) \approx ?$ | $0.8413$ |
| P809 | $\Phi(2) \approx ?$ | $0.9772$ |
| P810 | $\Phi(3) \approx ?$ | $0.9987$ |
| P811 | $\Phi^{-1}(0.95) \approx ?$ | $1.645$ |
| P812 | $\Phi^{-1}(0.975) \approx ?$ | $1.96$ |
| P813 | $\Phi^{-1}(0.995) \approx ?$ | $2.576$ |
| P814 | Harmonic number $H_6 \approx ?$ | $2.45$ |
| P815 | $\pi^2/6 \approx ?$ | $1.6449$ |
| P816 | $\Gamma(0.5) = ?$ | $\sqrt{\pi} \approx 1.7725$ |
| P817 | $\Gamma(3/2) = ?$ | $\sqrt{\pi}/2 \approx 0.886$ |
| P818 | $B(2,3) = ?$ | $\frac{\Gamma(2)\Gamma(3)}{\Gamma(5)} = \frac{1 \times 2}{24} = 1/12$ |
| P819 | $\zeta(2) = \sum 1/n^2 = ?$ | $\pi^2/6$ |
| P820 | Catalan $C_5 = ?$ | $42$ |
| P821 | $10^{10}$ has how many digits? | $11$ (since $\log_{10}(10^{10}) = 10$, so 11 digits) |
| P822 | $\log_2 1024 = ?$ | $10$ |
| P823 | Standard deviation of Uniform(0,12)? | $12/\sqrt{12} = 2\sqrt{3} \approx 3.46$ |
| P824 | MAD of standard normal? | $\Phi^{-1}(0.75) = 0.6745$ |
| P825 | IQR of $N(0,1)$? | $2 \times 0.6745 = 1.349$ |
| P826 | Skewness of symmetric distribution? | $0$ |
| P827 | Kurtosis of $t_5$? | $9$ (excess = $6$) for $\nu > 4$: $3 + 6/(\nu-4) = 3 + 6 = 9$ |
| P828 | Shannon entropy of $N(0, e)$ in nats? | $\frac{1}{2}\ln(2\pi e \cdot e) = \frac{1}{2}\ln(2\pi e^2) = 1 + \frac{1}{2}\ln(2\pi)$ |
| P829 | Capacity of AWGN channel? | $C = \frac{1}{2}\log_2(1 + \text{SNR})$ bits |
| P830 | Shannon's source coding theorem: min bits per symbol? | $H(X)$ (entropy of source) |

---

# 📝 Practice Set 10: Final Express Round (P831 – P900)

| # | Q | A |
|---|---|---|
| P831 | Toss 3 coins: $P(\text{all same})$? | $2/8 = 1/4$ |
| P832 | Roll 2 dice: $P(\text{sum} = 7)$? | $6/36 = 1/6$ |
| P833 | Pick card: $P(\text{ace of spades})$? | $1/52$ |
| P834 | $P(A|B) = 1$ means? | $A \supseteq B$ (whenever B occurs, A must too) |
| P835 | $P(A|B) = 0$ means? | $A \cap B = \emptyset$ (mutually exclusive) |
| P836 | If $P(A) = 0$, is $A$ impossible? | Not necessarily (for continuous RVs, point has prob 0 but isn't impossible) |
| P837 | CDF always right-continuous? | YES |
| P838 | CDF is non-decreasing? | YES |
| P839 | PDF can be > 1? | YES! (e.g., Uniform(0, 0.5) has PDF = 2) |
| P840 | PMF values sum to? | $1$ |
| P841 | $E[c] = ?$ for constant $c$ | $c$ |
| P842 | $\text{Var}(c) = ?$ | $0$ |
| P843 | $E[X + Y] = ?$ | $E[X] + E[Y]$ (ALWAYS!) |
| P844 | $E[XY] = ?$ if independent | $E[X]E[Y]$ |
| P845 | $\text{Var}(X - Y) = ?$ if independent | $\text{Var}(X) + \text{Var}(Y)$ (not minus!) |
| P846 | Standardization: $Z = ?$ | $(X - \mu)/\sigma$ |
| P847 | $P(Z \leq 0)$ for std normal | $0.5$ |
| P848 | Binomial mode formula? | $\lfloor (n+1)p \rfloor$ or $\lfloor (n+1)p \rfloor - 1$ (check both) |
| P849 | Poisson: $P(X = 0) = e^{-\lambda}$. If $\lambda = 5$? | $e^{-5} \approx 0.0067$ |
| P850 | Geometric: $P(X = 1) = ?$ | $p$ (success on first try) |
| P851 | Exponential: $P(X > E[X]) = ?$ | $e^{-1} \approx 0.368$ |
| P852 | Normal: $P(\mu - \sigma < X < \mu + \sigma) = ?$ | $0.6827$ |
| P853 | 99% CI: $z$-value? | $2.576$ |
| P854 | Degrees of freedom for $t$-test (one sample, $n$ obs)? | $n - 1$ |
| P855 | $df$ for two-sample $t$ (equal var, $n_1, n_2$)? | $n_1 + n_2 - 2$ |
| P856 | $\chi^2$ GOF with 6 categories: $df$? | $5$ |
| P857 | $\chi^2$ independence ($3 \times 4$ table): $df$? | $2 \times 3 = 6$ |
| P858 | ANOVA: $F = MS_B/MS_W$. Reject if F is? | Large (right-tailed test) |
| P859 | Regression: $\hat\beta_1$ sign interpretation? | Positive → Y increases with X |
| P860 | $R^2 = 1 - SSE/SST$. If SSE = 0? | $R^2 = 1$ (perfect fit) |
| P861 | MLE of Bernoulli $p$ from $n$ trials, $k$ successes? | $k/n$ |
| P862 | Bayesian: flat prior + lots of data ≈? | MLE |
| P863 | Markov chain: $n$-step transition? | $(P^n)_{ij}$ |
| P864 | Absorbing state: $P_{ii} = ?$ | $1$ |
| P865 | Entropy of deterministic RV? | $0$ (no uncertainty) |
| P866 | MI of independent RVs? | $0$ |
| P867 | $P(A \cup B \cup C)$ if all mutually exclusive? | $P(A) + P(B) + P(C)$ |
| P868 | Conditional independence: $P(A \cap B | C)$? | $P(A|C)P(B|C)$ if $A \perp B | C$ |
| P869 | Naive Bayes assumption? | Features conditionally independent given class |
| P870 | Bayes optimal classifier minimizes? | Expected misclassification rate |
| P871 | LDA assumes? | Gaussian class-conditionals with equal covariance |
| P872 | QDA allows? | Different covariance per class |
| P873 | Logistic regression: decision boundary? | Linear (hyperplane where $p = 0.5$) |
| P874 | $k$-NN: bias-variance with $k$? | Large $k$ → more bias, less variance |
| P875 | Leave-one-out equivalent for $k$-NN? | $k = 1$ has highest variance |
| P876 | Curse of dimensionality: volume of hypersphere? | Goes to 0 relative to hypercube as $d \to \infty$ |
| P877 | In high-$d$: most volume is near? | Surface (shell)! |
| P878 | Concentration of measure: distances become? | More uniform (all points roughly equidistant) |
| P879 | Johnson-Lindenstrauss: can embed $n$ points in $O(\log n/\epsilon^2)$ dims? | YES, preserving distances within $(1\pm\epsilon)$ |
| P880 | Random projection: $A \in \mathbb{R}^{k \times d}$ with i.i.d. entries? | JL with high probability |
| P881 | PCA: first $k$ PCs capture? | Maximum variance in $k$ dimensions |
| P882 | PCA is equivalent to minimizing? | Reconstruction error ($\|X - X_k\|_F^2$) |
| P883 | Eigenvalues of $\Sigma$ = ? | Variances along PC directions |
| P884 | Scree plot helps? | Choose number of components (elbow) |
| P885 | Factor analysis vs PCA? | FA models latent variables; PCA is just variance decomposition |
| P886 | ICA: assumes? | Non-Gaussian sources, independence |
| P887 | t-SNE for? | Visualization (non-linear dimensionality reduction) |
| P888 | UMAP advantages over t-SNE? | Faster, preserves global structure better |
| P889 | Kernel PCA? | PCA in feature space via kernel trick |
| P890 | Spectral clustering uses? | Eigenvalues of graph Laplacian |
| P891 | Graph Laplacian: $L = D - A$ | $D$ = degree matrix, $A$ = adjacency |
| P892 | Number of 0 eigenvalues of $L$ = ? | Number of connected components |
| P893 | Fiedler vector? | Eigenvector for 2nd smallest eigenvalue of $L$ |
| P894 | Random matrix theory: Marchenko-Pastur? | Eigenvalue distribution of sample covariance |
| P895 | Tracy-Widom distribution? | Largest eigenvalue of random matrix |
| P896 | Spiked model? | Signal eigenvalues + noise bulk |
| P897 | When can you detect signal? | Signal $> \sqrt{n/p}$ threshold (BBP transition) |
| P898 | Wishart distribution: $n$ samples, $p$ dims | $W = X^TX$ where $X$ is $n \times p$ with rows $\sim N(0, \Sigma)$ |
| P899 | Multivariate CLT? | $\sqrt{n}(\bar{X} - \mu) \xrightarrow{d} N(0, \Sigma)$ |
| P900 | Final formula: Bayes' theorem! | $P(A|B) = \frac{P(B|A)P(A)}{P(B)}$ — the MOST important formula in this module! |

---

## 📊 Complete Topic Difficulty & Frequency Table

| Topic | Difficulty (1-5) | GATE Frequency | DA Extra? | Priority |
|---|---|---|---|---|
| Counting & Combinatorics | 2 | Medium | No | 🟡 |
| Basic Probability | 2 | High | No | 🔴 |
| Conditional Probability | 3 | Very High | No | 🔴 |
| Bayes' Theorem | 3 | Very High | No | 🔴 |
| Random Variables & PMF/PDF | 2 | High | No | 🔴 |
| Expectation & Variance | 3 | Very High | No | 🔴 |
| Discrete Distributions | 3 | High | No | 🔴 |
| Continuous Distributions | 3 | High | No | 🔴 |
| Normal Distribution & CLT | 3 | Very High | No | 🔴 |
| Joint Distributions | 3 | Medium | No | 🟡 |
| Covariance & Correlation | 3 | High | Yes | 🔴 |
| MLE & Estimation | 4 | Medium | Yes | 🟡 |
| Hypothesis Testing | 3 | Medium | Yes | 🟡 |
| Chi-Squared Tests | 3 | Low-Med | Yes | 🟡 |
| Markov Chains | 4 | Medium | Yes | 🟡 |
| Information Theory | 4 | Low | Yes | 🟢 |
| Bayesian Inference | 4 | Low-Med | Yes | 🟡 |
| Advanced (Stochastic, etc.) | 5 | Low | Yes | 🟢 |

---

## 🏅 Final Mastery Self-Assessment

Rate 1-5 per topic. If < 3, restudy the relevant sections:

| Topic | Self-Rating | Review Section |
|---|---|---|
| Basic Probability & Events | __ /5 | Module 1 (1.1-1.2) |
| Conditional Probability | __ /5 | Module 1 (1.3-1.5) |
| Bayes' Theorem | __ /5 | Module 1 (1.5) + P5, P31-P33, P81 |
| Expectation/Variance | __ /5 | Module 1 (1.9-1.11) + P321-P360 |
| Discrete Distributions | __ /5 | Module 1 (1.12) + distribution table |
| Continuous Distributions | __ /5 | Module 1 (1.13) + Module 5 |
| Joint Distributions & Covariance | __ /5 | Module 2 |
| CLT & Sampling | __ /5 | Module 3 (3.1-3.2) |
| Hypothesis Testing | __ /5 | Module 3 (3.5-3.6) |
| MLE & Estimation | __ /5 | Module 4 (4.7) |
| Markov Chains | __ /5 | Module 4 (4.1) |
| Information Theory | __ /5 | Module 6 |

---

## 🎯 Index of All Practice Problems

| Set | Problems | Topics | Count |
|---|---|---|---|
| Set 1 | P1 – P80 | Fundamentals, Bayes, Distributions | 80 |
| Set 2 | P81 – P160 | GATE Simulation | 80 |
| Set 3 | P161 – P250 | Advanced, Tricky, Statistical Concepts | 90 |
| Set 4 | P251 – P320 | Markov Chains, Stochastic Processes | 70 |
| Set 5 | P321 – P420 | Expectation, Variance, Statistical Tests | 100 |
| Set 6 | P421 – P500 | Distributions, Transforms, Information Theory | 80 |
| Set 7 | P501 – P600 | GATE Predicted, ML Connections | 100 |
| Set 8 | P601 – P750 | GATE Marathon | 150 |
| Set 9 | P751 – P830 | True/False Conceptual + Speed | 80 |
| Set 10 | P831 – P900 | Express Final + ML/Stats Connections | 70 |
| **TOTAL** | | | **~900 problems** |

---

# 📝 Practice Set 11: Worked Examples — Full Solutions (P901 – P950)

> These are detailed step-by-step GATE-style solutions. Study the METHOD, not just the answer.

---

**P901.** ⭐ (2-mark) A class has 60% boys and 40% girls. 70% of boys and 50% of girls passed an exam. A student is selected randomly and found to have passed. What is the probability the student is a boy?

**Full Solution:**
- Let $B$ = boy, $G$ = girl, $P$ = passed
- $P(B) = 0.6$, $P(G) = 0.4$
- $P(P|B) = 0.7$, $P(P|G) = 0.5$
- $P(P) = P(P|B)P(B) + P(P|G)P(G) = 0.7 \times 0.6 + 0.5 \times 0.4 = 0.42 + 0.20 = 0.62$

$$P(B|P) = \frac{P(P|B)P(B)}{P(P)} = \frac{0.42}{0.62} = \frac{21}{31} \approx 0.677$$

🎯 **Answer: $21/31 \approx 0.677$**

---

**P902.** ⭐ (2-mark NAT) Given $X \sim \text{Poisson}(3)$, find $P(X = 2)$.

**Full Solution:**
$$P(X = k) = \frac{e^{-\lambda}\lambda^k}{k!} = \frac{e^{-3} \times 3^2}{2!} = \frac{e^{-3} \times 9}{2}$$

$e^{-3} \approx 0.0498$

$$P(X = 2) = \frac{0.0498 \times 9}{2} = \frac{0.4481}{2} = 0.2240$$

🎯 **Answer: 0.224**

---

**P903.** ⭐ (2-mark) A communication channel has crossover probability $p = 0.1$. Calculate the channel capacity.

**Full Solution:**
Binary symmetric channel capacity:
$$C = 1 - H(p) = 1 - [-p\log_2 p - (1-p)\log_2(1-p)]$$
$$= 1 - [-0.1\log_2 0.1 - 0.9\log_2 0.9]$$
$$= 1 - [0.1 \times 3.322 + 0.9 \times 0.152]$$
$$= 1 - [0.332 + 0.137]$$
$$= 1 - 0.469 = 0.531 \text{ bits/channel use}$$

🎯 **Answer: 0.531 bits/use**

---

**P904.** ⭐ (2-mark) $X$ has PDF $f(x) = 2x$ for $0 \leq x \leq 1$. Find $E[X]$ and $\text{Var}(X)$.

**Full Solution:**
$$E[X] = \int_0^1 x \cdot 2x \, dx = 2\int_0^1 x^2 \, dx = 2 \times \frac{1}{3} = \frac{2}{3}$$

$$E[X^2] = \int_0^1 x^2 \cdot 2x \, dx = 2\int_0^1 x^3 \, dx = 2 \times \frac{1}{4} = \frac{1}{2}$$

$$\text{Var}(X) = E[X^2] - (E[X])^2 = \frac{1}{2} - \frac{4}{9} = \frac{9 - 8}{18} = \frac{1}{18}$$

🎯 **Answer: $E[X] = 2/3$, $\text{Var}(X) = 1/18$**

---

**P905.** ⭐ (2-mark) Transition matrix of Markov chain:
$$P = \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix}$$
Find stationary distribution.

**Full Solution:**
$\pi P = \pi$, $\pi_1 + \pi_2 = 1$

$\pi_1 = 0.7\pi_1 + 0.4\pi_2$
$0.3\pi_1 = 0.4\pi_2$
$\pi_1/\pi_2 = 4/3$

$\pi_1 = 4/7$, $\pi_2 = 3/7$

🎯 **Answer: $\pi = (4/7, 3/7)$**

---

**P906.** ⭐ (2-mark) $X \sim \text{Exp}(\lambda = 2)$. Find $P(X > 3 | X > 1)$.

**Full Solution:**
By memoryless property:
$$P(X > 3 | X > 1) = P(X > 2) = e^{-2 \times 2} = e^{-4} \approx 0.0183$$

Alternative verification:
$$P(X > 3 | X > 1) = \frac{P(X > 3)}{P(X > 1)} = \frac{e^{-6}}{e^{-2}} = e^{-4}$$

🎯 **Answer: $e^{-4} \approx 0.0183$**

---

**P907.** ⭐ (2-mark) 8 identical balls into 3 distinct boxes. Number of ways?

**Full Solution:**
Stars and bars: $\binom{n+r-1}{r-1} = \binom{8+3-1}{3-1} = \binom{10}{2} = 45$

🎯 **Answer: 45**

---

**P908.** ⭐ (2-mark NAT) A biased coin: $P(H) = 0.3$. Toss 10 times. $E[\text{number of heads}]$ and $\text{Var}(\text{heads})$?

**Full Solution:**
$X \sim \text{Binomial}(10, 0.3)$

$E[X] = np = 10 \times 0.3 = 3$

$\text{Var}(X) = np(1-p) = 10 \times 0.3 \times 0.7 = 2.1$

🎯 **Answer: $E = 3$, $\text{Var} = 2.1$**

---

**P909.** ⭐ (2-mark) $X$ and $Y$ i.i.d. $\text{Uniform}(0,1)$. Find $P(X + Y > 1)$.

**Full Solution:**
Geometric probability: unit square, region where $X + Y > 1$ is a triangle.
$$P(X + Y > 1) = \text{area of triangle} = \frac{1}{2}$$

Alternative: $Z = X + Y$ has triangular distribution on $[0,2]$.
$$P(Z > 1) = 1/2 \text{ (by symmetry of triangular about 1)}$$

🎯 **Answer: 1/2**

---

**P910.** ⭐ (2-mark) MLE for $\theta$ given sample $x_1, \ldots, x_n$ from $\text{Uniform}(0, \theta)$.

**Full Solution:**
$L(\theta) = \prod_{i=1}^n \frac{1}{\theta} \cdot I(0 \leq x_i \leq \theta) = \frac{1}{\theta^n} \cdot I(\theta \geq x_{(n)})$

Likelihood is $1/\theta^n$ for $\theta \geq x_{(n)}$, which is **decreasing** in $\theta$.

So maximize at smallest valid $\theta$: $\hat\theta_{MLE} = x_{(n)} = \max(x_1, \ldots, x_n)$

⚠️ **GATE Trap:** This MLE is BIASED! $E[x_{(n)}] = \frac{n}{n+1}\theta \neq \theta$. Unbiased estimator: $\frac{n+1}{n}x_{(n)}$.

🎯 **Answer: $\hat\theta = x_{(n)}$ (maximum of sample)**

---

**P911.** ⭐ (2-mark) A bag has 4 red and 6 blue balls. Draw 3 without replacement. $P(\text{all red})$?

**Full Solution (Hypergeometric):**
$$P = \frac{\binom{4}{3}\binom{6}{0}}{\binom{10}{3}} = \frac{4 \times 1}{120} = \frac{4}{120} = \frac{1}{30}$$

🎯 **Answer: 1/30**

---

**P912.** ⭐ (2-mark) $X \sim N(100, 25)$. $P(95 < X < 110)$?

**Full Solution:**
$\mu = 100$, $\sigma = 5$ (since variance = 25)
$$P(95 < X < 110) = P\left(\frac{95-100}{5} < Z < \frac{110-100}{5}\right) = P(-1 < Z < 2)$$
$$= \Phi(2) - \Phi(-1) = 0.9772 - 0.1587 = 0.8185$$

🎯 **Answer: 0.8185**

---

**P913.** ⭐ (2-mark) Coupon collector: 4 different coupons. Expected draws to get all?

**Full Solution:**
$$E = n \cdot H_n = 4\left(1 + \frac{1}{2} + \frac{1}{3} + \frac{1}{4}\right) = 4 \times \frac{25}{12} = \frac{25}{3} \approx 8.33$$

Breakdown: 1st new = 1 draw, 2nd new = 4/3 draws, 3rd new = 2 draws, 4th new = 4 draws.

🎯 **Answer: 25/3 ≈ 8.33**

---

**P914.** ⭐ (2-mark) Chi-squared GOF test: Observed = (25, 30, 45) for 3 categories. Expected if uniform ($n = 100$)?

**Full Solution:**
Expected: $(100/3, 100/3, 100/3) = (33.33, 33.33, 33.33)$

$$\chi^2 = \frac{(25-33.33)^2}{33.33} + \frac{(30-33.33)^2}{33.33} + \frac{(45-33.33)^2}{33.33}$$
$$= \frac{69.39}{33.33} + \frac{11.09}{33.33} + \frac{135.99}{33.33} = 2.082 + 0.333 + 4.080 = 6.495$$

$df = 3 - 1 = 2$. $\chi^2_{0.05, 2} = 5.991$.

Since $6.495 > 5.991$: **Reject uniformity at 5%**.

🎯 **Answer: Reject $H_0$ ($\chi^2 = 6.495 > 5.991$)**

---

**P915-P950** — Mixed GATE Simulation:

**P915.** (1-mark) Geometric($p = 0.2$): $P(X > 5)$?
→ $P(X > 5) = (1-p)^5 = 0.8^5 = 0.32768$

**P916.** (1-mark) Negative Binomial: Mean of $\text{NB}(r, p)$?
→ $r(1-p)/p$ (waiting for $r$th success)

**P917.** (2-mark) If $X \sim \text{Gamma}(3, 2)$, $E[X]$?
→ $E[X] = \alpha/\beta = 3/2 = 1.5$ (shape/rate parameterization)
→ ⚠️ Check convention: if $\text{Gamma}(\alpha, \theta)$ with scale: $E = \alpha\theta = 6$

**P918.** (2-mark) $P(A) = 0.4$, $P(B) = 0.5$, $P(A \cup B) = 0.7$. Are A, B independent?
→ $P(A \cap B) = P(A) + P(B) - P(A \cup B) = 0.2$
→ $P(A)P(B) = 0.2$. Since both equal: YES, independent!

**P919.** (2-mark) Monty Hall: stay vs switch?
→ Stay: P(car) = 1/3. Switch: P(car) = 2/3. **Always switch!**

**P920.** (2-mark) Random sample of 100: mean = 50, 95% CI is (48.04, 51.96).
→ If 99% CI: $\bar{x} \pm 2.576 \times \sigma/\sqrt{n}$
→ From 95%: $1.96\sigma/10 = 1.96 \implies \sigma/10 = 1 \implies \sigma = 10$
→ 99%: $50 \pm 2.576 = (47.424, 52.576)$

**P921.** (1-mark) Mode of $\text{Binomial}(10, 0.3)$?
→ $\lfloor (n+1)p \rfloor = \lfloor 3.3 \rfloor = 3$

**P922.** (2-mark) Moment generating function $M_X(t) = e^{3t + 2t^2}$. What distribution?
→ $M_X(t) = e^{\mu t + \frac{\sigma^2 t^2}{2}}$ for Normal
→ Compare: $\mu = 3$, $\sigma^2/2 = 2 \implies \sigma^2 = 4$
→ $X \sim N(3, 4)$

**P923.** (2-mark) $X_1, \ldots, X_{25}$ i.i.d. from population with $\mu = 50$, $\sigma^2 = 100$. $P(\bar{X} > 54)$?
→ By CLT: $\bar{X} \sim N(50, 100/25) = N(50, 4)$
→ $P(\bar{X} > 54) = P(Z > (54-50)/2) = P(Z > 2) = 1 - 0.9772 = 0.0228$

**P924.** (2-mark) Indicator RV trick: $n$ people, each has birthday uniformly on 365 days. Expected number of pairs with same birthday?
→ Number of pairs: $\binom{n}{2}$. Each pair matches with prob $1/365$.
→ $E[\text{matching pairs}] = \binom{n}{2}/365$
→ For $n = 30$: $\binom{30}{2}/365 = 435/365 \approx 1.19$

**P925.** (1-mark) CDF of $\max(X_1, X_2)$ where $X_i$ i.i.d. with CDF $F$?
→ $F_{\max}(x) = P(\max \leq x) = P(X_1 \leq x)P(X_2 \leq x) = [F(x)]^2$

**P926.** (1-mark) CDF of $\min(X_1, X_2)$?
→ $F_{\min}(x) = 1 - [1-F(x)]^2$

**P927.** (2-mark) Rejection sampling: proposal $g$, target $f$. Acceptance condition?
→ Draw $X \sim g$, $U \sim \text{Uniform}(0,1)$. Accept if $U \leq \frac{f(X)}{Mg(X)}$ where $M = \sup \frac{f}{g}$

**P928.** (2-mark) Importance sampling estimate of $E_f[h(X)]$?
→ $\hat\mu = \frac{1}{n}\sum_{i=1}^n h(X_i)\frac{f(X_i)}{g(X_i)}$ where $X_i \sim g$

**P929.** (2-mark) What is the optimal importance distribution?
→ $g^*(x) \propto |h(x)|f(x)$ (proportional to $|h|f$) → zero variance!

**P930.** (2-mark NAT) $X \sim \text{Uniform}\{1,2,3,4,5\}$. Find $E[X^2]$.
→ $E[X^2] = \frac{1}{5}(1+4+9+16+25) = \frac{55}{5} = 11$

**P931.** (2-mark) Bivariate normal: marginals are normal, BUT normal marginals don't guarantee bivariate normality!
→ Counterexample: $X \sim N(0,1)$, $Y = X$ if $|X| > 1$, $Y = -X$ if $|X| \leq 1$. Both margins $N(0,1)$ but joint NOT normal!

**P932.** (1-mark) Law of total variance: $\text{Var}(Y) = ?$
→ $\text{Var}(Y) = E[\text{Var}(Y|X)] + \text{Var}(E[Y|X])$
→ **EVVE law**: "Eve's law" 🍎

**P933.** (2-mark) Box-Muller transform: $U_1, U_2$ i.i.d. $\text{Uniform}(0,1)$. Generate standard normals?
→ $Z_1 = \sqrt{-2\ln U_1}\cos(2\pi U_2)$
→ $Z_2 = \sqrt{-2\ln U_1}\sin(2\pi U_2)$
→ $Z_1, Z_2$ i.i.d. $N(0,1)$!

**P934.** (2-mark) Marsaglia polar method: faster alternative?
→ Generate $U, V$ uniform on $(-1,1)$. Let $S = U^2 + V^2$. If $S < 1$:
→ $Z_1 = U\sqrt{-2\ln S/S}$, $Z_2 = V\sqrt{-2\ln S/S}$

**P935.** (2-mark) Inverse CDF method: generate $X \sim F$?
→ $X = F^{-1}(U)$ where $U \sim \text{Uniform}(0,1)$. Works for any continuous CDF!

**P936.** (2-mark) Inverse for Exponential($\lambda$)?
→ $F(x) = 1 - e^{-\lambda x}$, so $F^{-1}(u) = -\ln(1-u)/\lambda$
→ Since $1-U \sim U$: shortcut $X = -\ln U/\lambda$

**P937.** (1-mark) Acceptance probability in MCMC Metropolis: $\alpha = ?$
→ $\alpha = \min\left(1, \frac{f(x')q(x|x')}{f(x)q(x'|x)}\right)$

**P938.** (1-mark) For symmetric proposal ($q(x'|x) = q(x|x')$)?
→ $\alpha = \min(1, f(x')/f(x))$ — Metropolis (simpler!)

**P939.** (2-mark) Ergodic theorem for MCMC?
→ $\frac{1}{n}\sum_{i=1}^n g(X_i) \xrightarrow{a.s.} E_\pi[g(X)]$ — sample averages converge to expected values

**P940.** (2-mark) Burn-in period?
→ Discard initial samples before chain reaches stationary distribution. Typically 10-50% of chain.

**P941.** (2-mark) Hamiltonian Monte Carlo (HMC)?
→ Uses gradient information: $\nabla \log f(x)$. Proposals follow Hamiltonian dynamics.
→ Less random walk behavior → more efficient exploration.

**P942.** (2-mark) NUTS (No U-Turn Sampler)?
→ Adaptive HMC: automatically selects trajectory length. Used in Stan.

**P943.** (2-mark) Conjugate prior for normal mean (known variance)?
→ Prior: $\mu \sim N(\mu_0, \tau^2)$
→ Posterior: $\mu | \bar{x} \sim N\left(\frac{\tau^2}{\tau^2 + \sigma^2/n}\bar{x} + \frac{\sigma^2/n}{\tau^2 + \sigma^2/n}\mu_0, \frac{1}{1/\tau^2 + n/\sigma^2}\right)$

**P944.** (1-mark) Conjugate prior for Poisson rate?
→ Gamma! Prior: $\lambda \sim \text{Gamma}(\alpha, \beta)$
→ Posterior: $\lambda | x_1, \ldots, x_n \sim \text{Gamma}(\alpha + \sum x_i, \beta + n)$

**P945.** (1-mark) Conjugate for exponential rate?
→ Gamma! $\lambda | x_1,\ldots,x_n \sim \text{Gamma}(\alpha + n, \beta + \sum x_i)$

**P946.** (2-mark) Jeffreys prior for Bernoulli $p$?
→ $\pi(p) \propto p^{-1/2}(1-p)^{-1/2} = \text{Beta}(1/2, 1/2)$ — "arcsine" distribution

**P947.** (2-mark) Predictive distribution (Bayesian)?
→ $p(x_{n+1} | x_1, \ldots, x_n) = \int p(x_{n+1}|\theta)\pi(\theta|x_1,\ldots,x_n)d\theta$
→ Average over posterior uncertainty!

**P948.** (2-mark) For Normal-Normal conjugate: predictive is?
→ Student's $t$ distribution (heavier tails than normal — accounts for parameter uncertainty!)

**P949.** (2-mark) Bayesian model comparison: Bayes factor?
→ $BF_{12} = \frac{P(\text{data}|M_1)}{P(\text{data}|M_2)} = \frac{\int L(\theta_1)p(\theta_1|M_1)d\theta_1}{\int L(\theta_2)p(\theta_2|M_2)d\theta_2}$
→ $BF > 10$: strong evidence for $M_1$

**P950.** (2-mark) BIC approximation to log Bayes factor?
→ $\log BF \approx -\frac{1}{2}(\text{BIC}_2 - \text{BIC}_1)$

---

# 📝 Practice Set 12: GATE PYQ Reproductions (P951 – P1020)

> These mirror actual GATE CS/DA question patterns. ⚡ Solve under timed conditions!

**P951.** (GATE style, 1-mark) A fair die is thrown twice. The probability that the first throw is at least as large as the second throw is ___

→ Count favorable: $\sum_{k=1}^6 k = 21$ (first = k, second ≤ k)
→ $P = 21/36 = 7/12$

**P952.** (GATE style, 2-mark) Let $X$ be a random variable with probability density function:
$$f(x) = \begin{cases} cx^2 & 0 \leq x \leq 2 \\ 0 & \text{otherwise} \end{cases}$$
The value of $E[X]$ is ___

→ First find $c$: $\int_0^2 cx^2 dx = c \times 8/3 = 1 \implies c = 3/8$
→ $E[X] = \int_0^2 x \cdot \frac{3}{8}x^2 dx = \frac{3}{8}\int_0^2 x^3 dx = \frac{3}{8} \times 4 = \frac{3}{2} = 1.5$

**P953.** (GATE style, 1-mark) The number of onto functions from a set of 4 elements to a set of 3 elements is ___

→ Surjections: $3! \times S(4,3)$ where $S(4,3) = 6$. So $6 \times 6 = 36$.
→ Alternatively: $\sum_{k=0}^3 (-1)^k \binom{3}{k}(3-k)^4 = 81 - 48 + 3 = 36$

**P954.** (GATE style, 2-mark) Suppose $P(A) = 0.5$, $P(B) = 0.4$, $P(A \cap B) = 0.3$. Then $P(A | A \cup B)$ is ___

→ $P(A \cup B) = 0.5 + 0.4 - 0.3 = 0.6$
→ $P(A | A \cup B) = P(A \cap (A \cup B))/P(A \cup B) = P(A)/P(A \cup B) = 0.5/0.6 = 5/6$
→ (since $A \cap (A \cup B) = A$)

**P955.** (GATE style, 2-mark) In a communication system, $P(0$ sent$) = 0.4$, $P(1$ sent$) = 0.6$. When 0 is sent, it is received as 0 with prob 0.9. When 1 is sent, it is received as 1 with prob 0.8. What is $P(1$ sent $|$ 1 received$)$?

→ $P(1 \text{ recv}) = P(1\text{r}|0\text{s})P(0\text{s}) + P(1\text{r}|1\text{s})P(1\text{s})$
→ $= 0.1 \times 0.4 + 0.8 \times 0.6 = 0.04 + 0.48 = 0.52$
→ $P(1\text{s}|1\text{r}) = 0.48/0.52 = 12/13 \approx 0.923$

**P956.** (GATE style, 2-mark) The lifetime (in years) of a machine is exponentially distributed with mean 5. If the machine has already been working for 3 years, the expected remaining lifetime is ___

→ Memoryless! Expected remaining = 5 years (same as original mean)
⚠️ Common trap: students subtract 3. Don't!

**P957.** (GATE style, 2-mark) Let $X_1, X_2, \ldots, X_{100}$ be i.i.d. random variables with mean 0 and variance 1. Then $P\left(X_1^2 + X_2^2 + \cdots + X_{100}^2 > 120 \right)$ is approximately ___

→ $Y = \sum X_i^2$. $E[X_i^2] = \text{Var}(X_i) + (E[X_i])^2 = 1$
→ $E[Y] = 100$, $\text{Var}(Y) = 100 \times \text{Var}(X_i^2)$
→ If $X_i \sim N(0,1)$: $Y \sim \chi^2_{100}$, $\text{Var} = 200$.
→ $P(Y > 120) = P\left(Z > \frac{120-100}{\sqrt{200}}\right) = P(Z > 1.414) \approx 0.0786$

**P958.** (GATE style, 1-mark) A random variable $X$ takes values $-1, 0, 1$ with probabilities $1/4, 1/2, 1/4$. Then $E[X]$ is ___

→ $E[X] = -1(1/4) + 0(1/2) + 1(1/4) = 0$

**P959.** (GATE style, 2-mark) Same $X$ as P958. What is $E[X^2]$?
→ $E[X^2] = 1(1/4) + 0(1/2) + 1(1/4) = 1/2$
→ $\text{Var}(X) = 1/2 - 0 = 1/2$

**P960.** (GATE style, 2-mark) 3 white and 5 black balls in an urn. 2 drawn without replacement. $P(\text{both same color})$?

→ $P(\text{both white}) = \frac{3}{8} \times \frac{2}{7} = \frac{6}{56}$
→ $P(\text{both black}) = \frac{5}{8} \times \frac{4}{7} = \frac{20}{56}$
→ $P(\text{same}) = \frac{26}{56} = \frac{13}{28}$

**P961.** (GATE style, 2-mark) $X \sim \text{Binomial}(20, 0.05)$. Approximate $P(X = 0)$ using Poisson.
→ $\lambda = np = 1$. $P(X = 0) \approx e^{-1} \approx 0.3679$
→ Exact: $(0.95)^{20} = 0.3585$. Poisson is close!

**P962.** (GATE style, 2-mark) $X$ and $Y$ independent, both $\text{Uniform}(0,1)$. PDF of $Z = X + Y$?
→ $Z$ has triangular distribution on $[0,2]$:
$$f_Z(z) = \begin{cases} z & 0 \leq z \leq 1 \\ 2-z & 1 < z \leq 2 \end{cases}$$

**P963.** (GATE style, 1-mark) Covariance of $X$ and $2X + 3$?
→ $\text{Cov}(X, 2X+3) = 2\text{Var}(X)$ (since $\text{Cov}(X, aX+b) = a\text{Var}(X)$)

**P964.** (GATE style, 2-mark) $n = 16$ observations from $N(\mu, 64)$, $\bar{x} = 50$. Test $H_0: \mu = 45$ at $\alpha = 0.05$ (two-tailed).
→ $Z = \frac{50-45}{8/4} = 5/2 = 2.5$. $|Z| = 2.5 > 1.96$: **Reject $H_0$**.

**P965.** (GATE style, 2-mark) Entropy of a source with probabilities $(1/2, 1/4, 1/8, 1/8)$?
→ $H = -\frac{1}{2}\log_2\frac{1}{2} - \frac{1}{4}\log_2\frac{1}{4} - \frac{1}{8}\log_2\frac{1}{8} - \frac{1}{8}\log_2\frac{1}{8}$
→ $= \frac{1}{2}(1) + \frac{1}{4}(2) + \frac{1}{8}(3) + \frac{1}{8}(3) = 0.5 + 0.5 + 0.375 + 0.375 = 1.75$ bits

**P966-P1020** — Rapid PYQ Style:

| # | Type | Question | Answer |
|---|---|---|---|
| P966 | NAT | $X \sim \text{Geometric}(1/3)$. $E[X]$? | $1/p = 3$ |
| P967 | NAT | $\text{Var}(X)$ for above? | $(1-p)/p^2 = 6$ |
| P968 | MCQ | Which is NOT a valid PMF? (a) all 1/n (b) all 0.3 (c) geometric series | (b) — sums to > 1 for $n>3$ |
| P969 | NAT | Correlation between $X$ and $-X$? | $-1$ |
| P970 | NAT | $P(A^c \cap B^c)$ if $P(A \cup B) = 0.8$? | $0.2$ (De Morgan) |
| P971 | MCQ | Unbiased estimator for $\sigma^2$? | $S^2 = \sum(X_i-\bar{X})^2/(n-1)$ |
| P972 | NAT | 6 people in a line, P(specific 2 are adjacent)? | $2 \times 5!/6! = 1/3$ |
| P973 | MCQ | What does CLT NOT require? | Normality of population |
| P974 | NAT | $E[X]$ for $X \sim \text{Hypergeometric}(N=20, K=8, n=5)$? | $nK/N = 2$ |
| P975 | MCQ | Which test for independence? | $\chi^2$ test of independence |
| P976 | NAT | Entropy of fair coin in bits? | $1$ |
| P977 | NAT | MI of $X$ and $Y = g(X)$ where $g$ is 1-1? | $H(X)$ (= $H(Y)$) |
| P978 | MCQ | MCMC converges to? | Stationary distribution |
| P979 | NAT | Birthday problem: min people for P > 1/2? | $23$ |
| P980 | NAT | Derangements: $D_4$? | $9$ |
| P981 | NAT | $P(\text{at least 1 six in 4 rolls})$? | $1-(5/6)^4 = 1 - 625/1296 \approx 0.518$ |
| P982 | NAT | Median of $\text{Exp}(\lambda)$? | $\ln 2/\lambda$ |
| P983 | MCQ | MSE = Bias² + ? | Variance |
| P984 | NAT | Fisher info for $N(\mu, 1)$: $I(\mu)$? | $1$ |
| P985 | NAT | CRLB for unbiased $\hat\mu$ from $N(\mu,1)$ with $n$ samples? | $1/n$ |
| P986 | NAT | Power of a test if $\beta = 0.15$? | $1 - \beta = 0.85$ |
| P987 | MCQ | Type I error is? | Rejecting true $H_0$ |
| P988 | MCQ | Type II error is? | Failing to reject false $H_0$ |
| P989 | NAT | Poisson: $P(X \geq 1)$ if $\lambda = 2$? | $1 - e^{-2} \approx 0.865$ |
| P990 | NAT | Normal: $P(|Z| > 1.96)$? | $0.05$ (by definition of 95% CI) |
| P991 | MCQ | $t$-test vs $z$-test: use $t$ when? | $\sigma$ unknown and $n$ small |
| P992 | NAT | $\chi^2_{10}$: mean? | $10$ |
| P993 | NAT | $\chi^2_{10}$: variance? | $20$ |
| P994 | NAT | $F_{5,10}$: $E[F]$? | $10/(10-2) = 10/8 = 1.25$ |
| P995 | MCQ | Sufficient statistic: $(\bar{X}, S^2)$ for? | Normal with unknown $\mu, \sigma^2$ |
| P996 | NAT | Rao-Blackwell improves an estimator by? | Conditioning on sufficient statistic |
| P997 | MCQ | Complete statistic ensures? | Unique unbiased estimator (Lehmann-Scheffé) |
| P998 | NAT | Exponential family: dim of sufficient stat? | Number of parameters |
| P999 | MCQ | Ancillary statistic? | Distribution doesn't depend on parameter |
| P1000 | NAT | Basu's theorem links? | Complete sufficient + ancillary → independent! |
| P1001 | NAT | 52 cards, 5 dealt. P(flush in spades)? | $\binom{13}{5}/\binom{52}{5} = 1287/2598960$ |
| P1002 | NAT | Total flushes (any suit)? | $4 \times 1287/2598960 \approx 0.00198$ |
| P1003 | MCQ | Which dist has equal mean & variance? | Poisson($\lambda$) |
| P1004 | NAT | Max entropy for discrete on $\{1,...,n\}$? | $\log n$ (uniform distribution) |
| P1005 | NAT | Max entropy continuous on $[a,b]$? | $\ln(b-a)$ (uniform) |
| P1006 | NAT | Max entropy on $(0,\infty)$ with given mean? | Exponential |
| P1007 | NAT | Max entropy on $\mathbb{R}$ with given mean & variance? | Normal |
| P1008 | MCQ | What is a pivotal quantity? | Function of data & parameter whose distribution is parameter-free |
| P1009 | NAT | Example: $\frac{\bar{X}-\mu}{S/\sqrt{n}} \sim ?$ | $t_{n-1}$ (pivot!) |
| P1010 | NAT | $P(\text{Royal flush in 5-card poker})$? | $4/\binom{52}{5} \approx 1.54 \times 10^{-6}$ |
| P1011 | MCQ | Delta method: $\sqrt{n}(g(\bar{X}) - g(\mu)) \xrightarrow{d}$? | $N(0, [g'(\mu)]^2\sigma^2)$ |
| P1012 | NAT | If $\bar{X} \sim N(\mu, 1/n)$, variance of $e^{\bar{X}}$? | $\approx e^{2\mu}/n$ by delta method |
| P1013 | MCQ | Consistency means? | $\hat\theta_n \xrightarrow{P} \theta$ as $n \to \infty$ |
| P1014 | MCQ | Efficiency of estimator? | Ratio of CRLB to actual variance |
| P1015 | NAT | Asymptotic relative efficiency of median vs mean (normal)? | $\pi/2 \approx 0.637$ (mean is better) |
| P1016 | MCQ | Robust estimator? | Resistant to outliers (e.g., median) |
| P1017 | NAT | Breakdown point of median? | $50\%$ (best possible!) |
| P1018 | NAT | Breakdown point of mean? | $1/n \to 0$ (worst!) |
| P1019 | MCQ | M-estimator? | Minimize $\sum \rho(x_i - \theta)$ for some $\rho$ |
| P1020 | MCQ | Huber's $\rho$? | Quadratic near center, linear in tails (combines mean & median) |

---

# 📝 Practice Set 13: Cross-Topic Integration (P1021 – P1100)

> These problems connect Probability & Statistics to ML, AI, Algorithms, and other GATE subjects.

**P1021.** (2-mark) In Naive Bayes classifier, the likelihood for a document is:
$$P(d|c) = \prod_{i} P(w_i|c)$$
This assumes all words are conditionally independent given class. True/False?

→ TRUE. This is the "naive" assumption. Often violated but works well in practice!

**P1022.** (2-mark) A hash function maps $n$ keys to $m$ slots uniformly at random. Expected number of collisions?
→ $E[\text{collisions}] = \binom{n}{2} \times \frac{1}{m} = \frac{n(n-1)}{2m}$

**P1023.** (2-mark) In randomized QuickSort, expected number of comparisons?
→ $E[\text{comparisons}] = 2(n+1)H_n - 4n \approx 2n\ln n$

**P1024.** (2-mark) Randomized algorithm succeeds with prob $\geq 2/3$. After $k$ independent runs (take majority), failure prob?
→ By Chernoff: failure decreases exponentially in $k$
→ After $k$ runs: $P(\text{fail}) \leq e^{-\Omega(k)}$

**P1025.** (2-mark) PAC learning: with probability $\geq 1-\delta$, error $\leq \epsilon$. Sample complexity for finite hypothesis class $|H|$?
→ $m \geq \frac{1}{\epsilon}\left(\ln|H| + \ln\frac{1}{\delta}\right)$

**P1026.** (2-mark) VC dimension of linear classifiers in $\mathbb{R}^d$?
→ $d + 1$

**P1027.** (2-mark) Bias-variance decomposition: $\text{MSE} = ?$
→ $\text{Bias}^2 + \text{Variance} + \text{Irreducible noise}$

**P1028.** (2-mark) In information gain (ID3/C4.5), we choose attribute that maximizes?
→ $IG(S, A) = H(S) - \sum_v \frac{|S_v|}{|S|}H(S_v)$

**P1029.** (2-mark) Cross-entropy loss for binary classification:
→ $L = -[y\log\hat{p} + (1-y)\log(1-\hat{p})]$
→ This equals KL divergence from true to predicted + constant!

**P1030.** (2-mark) Softmax converts logits to probabilities:
$$P(y=k|x) = \frac{e^{z_k}}{\sum_j e^{z_j}}$$
→ This is the **Gibbs/Boltzmann distribution**! (Connection to statistical mechanics.)

**P1031.** (2-mark) SGD convergence: for $\eta = O(1/\sqrt{t})$, expected regret?
→ $O(\sqrt{T})$ — sublinear in $T$

**P1032.** (2-mark) Dropout with rate $p$: equivalent to?
→ Approximate Bayesian inference! Multiplying by $1/(1-p)$ at test time = taking expectation.

**P1033.** (2-mark) Batch norm: why does it work?
→ Reduces internal covariate shift. Standardizes layer inputs: $\hat{x} = (x - \mu_B)/\sigma_B$

**P1034-P1060** — Probability in Algorithms:

| # | Topic | Problem | Key Result |
|---|---|---|---|
| P1034 | Hashing | Expected probes in open addressing (load $\alpha$)? | $1/(1-\alpha)$ for linear probing |
| P1035 | Hashing | Universal hashing: $P(h(x) = h(y)) \leq ?$ | $1/m$ |
| P1036 | Skip List | Expected search time? | $O(\log n)$ |
| P1037 | Treap | Expected depth? | $O(\log n)$ |
| P1038 | Bloom Filter | False positive prob with $m$ bits, $n$ elements, $k$ hashes? | $(1-e^{-kn/m})^k$ |
| P1039 | Reservoir Sampling | Keep $k$ items from stream, each with prob $k/n$? | YES — reservoir sampling |
| P1040 | Min-Cut | Karger's: P(finding min cut) per trial? | $\geq 2/(n(n-1))$ |
| P1041 | Max-Cut | GATE: random assignment achieves? | $\geq 1/2$ of optimal (simple!) |
| P1042 | Randomized Median | Expected comparisons? | $O(n)$ |
| P1043 | Balls into Bins | Max load with $n$ balls, $n$ bins? | $\Theta(\log n/\log\log n)$ w.h.p. |
| P1044 | Power of 2 | Choose least loaded of 2 random bins? | Max load $\Theta(\log\log n)$! (exponential improvement) |
| P1045 | Cuckoo Hashing | Expected insertion time? | $O(1)$ amortized |
| P1046 | Count-Min Sketch | Space for $(\epsilon, \delta)$ approximation? | $O((1/\epsilon)\log(1/\delta))$ |
| P1047 | HyperLogLog | Space for cardinality estimation within $\epsilon$? | $O(1/\epsilon^2)$ |
| P1048 | Locality-Sensitive Hashing | For cosine similarity: random hyperplane? | Sign of $w^Tx$ preserves angle |
| P1049 | Random Graphs | $G(n, p)$: threshold for connectivity? | $p = \ln n/n$ |
| P1050 | Erdos-Renyi | Giant component appears at? | $p = 1/n$ |
| P1051 | Chernoff application | Load balancing: $P(\text{overload} > (1+\delta)\mu) \leq ?$ | $\left(\frac{e^\delta}{(1+\delta)^{(1+\delta)}}\right)^\mu$ |
| P1052 | Occupancy | Expected empty bins ($n$ balls, $n$ bins)? | $n(1-1/n)^n \approx n/e$ |
| P1053 | Occupancy | Expected singletons? | $n(1/n)(1-1/n)^{n-1} \approx n/e$ (same!) |
| P1054 | Coupon Collector | Variance of time to collect all $n$? | $\sum_{k=1}^n \frac{n^2}{k^2} \approx \frac{\pi^2 n^2}{6}$ — variance is $\Theta(n^2)$! |
| P1055 | Random Permutation | Expected cycles? | $H_n \approx \ln n$ |
| P1056 | Longest increasing subseq | Expected length in random perm? | $\approx 2\sqrt{n}$ (Tracy-Widom limit!) |
| P1057 | Secretary Problem | Optimal strategy: reject first $n/e$, then pick? | P(best) $\to 1/e \approx 0.368$ |
| P1058 | Multi-Armed Bandit | UCB regret? | $O(\sqrt{nK\log n})$ |
| P1059 | Thompson Sampling | Bayesian approach: sample from posterior? | Often best in practice |
| P1060 | A/B Testing | Needed sample size for effect $\delta$, power $1-\beta$? | $n \approx \frac{(z_\alpha + z_\beta)^2 \times 2\sigma^2}{\delta^2}$ |

**P1061-P1100** — Statistics in Machine Learning:

| # | Topic | Connection | Key Formula |
|---|---|---|---|
| P1061 | Linear Regression | OLS estimator | $\hat\beta = (X^TX)^{-1}X^Ty$ |
| P1062 | Ridge Regression | Bayesian: Gaussian prior on $\beta$ | $\hat\beta = (X^TX + \lambda I)^{-1}X^Ty$ |
| P1063 | LASSO | Bayesian: Laplace prior | $\min \|y-X\beta\|^2 + \lambda\|\beta\|_1$ |
| P1064 | Elastic Net | Mixture of L1 + L2 | $\lambda_1\|\beta\|_1 + \lambda_2\|\beta\|_2^2$ |
| P1065 | Logistic Loss | Negative log-likelihood | $-\sum[y_i\log\sigma(x_i^T\beta) + (1-y_i)\log(1-\sigma(x_i^T\beta))]$ |
| P1066 | Softmax Loss | Cross-entropy | $-\sum_k y_k \log p_k$ |
| P1067 | KL in VAE | Evidence lower bound | $\text{ELBO} = E_q[\log p(x|z)] - D_{KL}(q(z|x)\|p(z))$ |
| P1068 | GAN | Minimax game | $\min_G \max_D E[\log D(x)] + E[\log(1-D(G(z)))]$ |
| P1069 | Wasserstein GAN | Earth mover's distance | Overcomes mode collapse |
| P1070 | EM Algorithm | E-step: expected sufficient stats | $Q(\theta|\theta^{(t)}) = E_{Z|X,\theta^{(t)}}[\log P(X,Z|\theta)]$ |
| P1071 | K-Means | Special case of EM for? | Gaussian mixture, equal spherical covariance |
| P1072 | PCA | Max variance = max eigenvalue | First PC: $\arg\max_{\|w\|=1} w^T\Sigma w$ |
| P1073 | CCA | Canonical Correlation Analysis | Maximize correlation between projections |
| P1074 | ICA | Non-Gaussian assumption | Maximize non-Gaussianity of components |
| P1075 | t-SNE | KL between joint distributions | $\min D_{KL}(P\|Q)$ where $P$ = high-dim, $Q$ = low-dim |
| P1076 | Random Forest | Bagging + random feature subset | Variance reduction via decorrelation |
| P1077 | Bootstrap | Resample with replacement | Approximate sampling distribution |
| P1078 | Jackknife | Leave-one-out | Bias estimation |
| P1079 | Permutation Test | Nonparametric | No distribution assumption |
| P1080 | Wilcoxon Test | Rank-based | Robust alternative to $t$-test |
| P1081 | Mann-Whitney | Two-sample rank | $P(X > Y)$ estimation |
| P1082 | Kolmogorov-Smirnov | CDF comparison | $D_n = \sup|F_n - F_0|$ |
| P1083 | Anderson-Darling | Weighted CDF comparison | More sensitive at tails |
| P1084 | QQ-Plot | Visual normality check | Points on line ↔ normal |
| P1085 | Shapiro-Wilk | Best normality test for small $n$ | Based on order statistics |
| P1086 | Box-Cox Transform | $Y^{(\lambda)} = (Y^\lambda - 1)/\lambda$ | Makes data more normal |
| P1087 | Rank Transform | Replace by ranks | Robust to outliers |
| P1088 | Winsorizing | Cap extreme values | Reduce outlier impact |
| P1089 | Trimmed Mean | Remove top/bottom $\alpha$% | Compromise mean/median |
| P1090 | Influence Function | Measures effect of single obs | $IF(x) = \lim_{\epsilon \to 0} \frac{T((1-\epsilon)F + \epsilon\delta_x) - T(F)}{\epsilon}$ |
| P1091 | Cook's Distance | Regression outlier | $D_i = \frac{(\hat{y}-\hat{y}_{(i)})^T(\hat{y}-\hat{y}_{(i)})}{p \cdot MSE}$ |
| P1092 | Leverage | Hat matrix diagonal | $h_{ii}$ = influence of $y_i$ on $\hat{y}_i$ |
| P1093 | DFFITS | Scaled influence | $\frac{\hat{y}_i - \hat{y}_{(i)}}{\hat\sigma_{(i)}\sqrt{h_{ii}}}$ |
| P1094 | AIC | Model selection | $-2\ell + 2k$ |
| P1095 | BIC | Model selection (consistent) | $-2\ell + k\ln n$ |
| P1096 | Cross-Validation | $K$-fold: bias-variance tradeoff | $K = n$ (LOOCV): low bias, high variance |
| P1097 | Stratified CV | Preserve class proportions | Important for imbalanced data |
| P1098 | Time Series CV | Forward chaining | Never use future data for training |
| P1099 | Calibration | $P(\hat{p} = p | Y = 1) = p$? | Platt scaling, isotonic regression |
| P1100 | Conformal Prediction | Distribution-free prediction sets | Guaranteed coverage: $P(Y \in C(X)) \geq 1 - \alpha$ |

---

# 🧮 Module 7: Complete Derivations Reference

## 7.1 Derivation of Bayesian Posterior for Normal-Normal Conjugate

**Setup:** Prior $\mu \sim N(\mu_0, \tau^2)$, Likelihood $X_i | \mu \sim N(\mu, \sigma^2)$

**Step 1:** Write joint log-likelihood:
$$\log p(\mu | x_1, \ldots, x_n) \propto -\frac{1}{2\tau^2}(\mu - \mu_0)^2 - \frac{1}{2\sigma^2}\sum(x_i - \mu)^2$$

**Step 2:** Expand and collect $\mu$ terms:
$$= -\frac{1}{2}\left[\left(\frac{1}{\tau^2} + \frac{n}{\sigma^2}\right)\mu^2 - 2\left(\frac{\mu_0}{\tau^2} + \frac{n\bar{x}}{\sigma^2}\right)\mu + \text{const}\right]$$

**Step 3:** Complete the square:
$$\mu | \mathbf{x} \sim N\left(\frac{\frac{\mu_0}{\tau^2} + \frac{n\bar{x}}{\sigma^2}}{\frac{1}{\tau^2} + \frac{n}{\sigma^2}}, \frac{1}{\frac{1}{\tau^2} + \frac{n}{\sigma^2}}\right)$$

**Key Insight:** Posterior mean = weighted average of prior mean and sample mean, with weights = precisions!

$$\mu_{\text{post}} = w \cdot \mu_0 + (1-w) \cdot \bar{x}, \quad w = \frac{\sigma^2/n}{\tau^2 + \sigma^2/n}$$

As $n \to \infty$: $w \to 0$, posterior $\to$ MLE. Prior washes out!

---

## 7.2 Derivation of MLE for Normal Distribution

**Data:** $x_1, \ldots, x_n$ i.i.d. $N(\mu, \sigma^2)$

**Log-likelihood:**
$$\ell(\mu, \sigma^2) = -\frac{n}{2}\ln(2\pi) - \frac{n}{2}\ln\sigma^2 - \frac{1}{2\sigma^2}\sum(x_i - \mu)^2$$

**For $\mu$:**
$$\frac{\partial\ell}{\partial\mu} = \frac{1}{\sigma^2}\sum(x_i - \mu) = 0 \implies \hat\mu = \bar{x}$$

**For $\sigma^2$:**
$$\frac{\partial\ell}{\partial\sigma^2} = -\frac{n}{2\sigma^2} + \frac{1}{2(\sigma^2)^2}\sum(x_i - \bar{x})^2 = 0$$
$$\implies \hat\sigma^2 = \frac{1}{n}\sum(x_i - \bar{x})^2$$

⚠️ **Trap:** MLE of $\sigma^2$ has divisor $n$, not $n-1$. It's **biased**: $E[\hat\sigma^2] = \frac{n-1}{n}\sigma^2$.

---

## 7.3 Derivation of Chi-Squared Test Statistic

**Multinomial setup:** $n$ observations, $k$ categories, expected proportions $p_1, \ldots, p_k$.

**Observed:** $O_i$, **Expected:** $E_i = np_i$

**Under $H_0$:** by CLT for multinomial:
$$\frac{O_i - E_i}{\sqrt{E_i}} \approx N(0, 1) \text{ (approximately, each marginal)}$$

$$\chi^2 = \sum_{i=1}^k \frac{(O_i - E_i)^2}{E_i} \approx \chi^2_{k-1}$$

The df is $k-1$ (not $k$) because $\sum O_i = n$ is fixed.

---

## 7.4 Derivation of CLT (Sketch via MGFs)

**Goal:** Show $Z_n = \frac{\bar{X}_n - \mu}{\sigma/\sqrt{n}} \xrightarrow{d} N(0,1)$

**Step 1:** $M_{Z_n}(t) = \left[M_X\left(\frac{t}{\sigma\sqrt{n}}\right) \cdot e^{-t\mu/(\sigma\sqrt{n})}\right]^n$

**Step 2:** Taylor expand around 0:
$$M_X(s) = 1 + \mu s + \frac{E[X^2]s^2}{2} + O(s^3)$$

**Step 3:** For $s = t/(\sigma\sqrt{n})$:
$$M_X(s) \cdot e^{-\mu s} \approx 1 + \frac{t^2}{2n} + O(n^{-3/2})$$

**Step 4:** Taking $n$th power:
$$\left(1 + \frac{t^2}{2n} + \cdots\right)^n \to e^{t^2/2}$$

This is the MGF of $N(0,1)$! By Lévy's continuity theorem, $Z_n \xrightarrow{d} N(0,1)$.

---

## 7.5 Key Derivation: Fisher Information

For a parametric family $f(x|\theta)$:

$$I(\theta) = -E\left[\frac{\partial^2}{\partial\theta^2}\log f(X|\theta)\right] = E\left[\left(\frac{\partial}{\partial\theta}\log f(X|\theta)\right)^2\right]$$

Both expressions are equal (under regularity conditions). The **score function** $U = \partial\log f/\partial\theta$ has:
- $E[U] = 0$
- $\text{Var}(U) = I(\theta)$

**Cramér-Rao Lower Bound:**
$$\text{Var}(\hat\theta) \geq \frac{[1 + b'(\theta)]^2}{nI(\theta)}$$

For unbiased ($b(\theta) = 0$): $\text{Var}(\hat\theta) \geq 1/(nI(\theta))$

---

# 📋 Complete Conjugate Prior Reference Table

| Likelihood | Parameter | Conjugate Prior | Posterior |
|---|---|---|---|
| $\text{Bernoulli}(p)$ | $p$ | $\text{Beta}(\alpha, \beta)$ | $\text{Beta}(\alpha + \sum x_i, \beta + n - \sum x_i)$ |
| $\text{Binomial}(n, p)$ | $p$ | $\text{Beta}(\alpha, \beta)$ | $\text{Beta}(\alpha + \sum x_i, \beta + \sum(n_i - x_i))$ |
| $\text{Poisson}(\lambda)$ | $\lambda$ | $\text{Gamma}(\alpha, \beta)$ | $\text{Gamma}(\alpha + \sum x_i, \beta + n)$ |
| $\text{Exp}(\lambda)$ | $\lambda$ | $\text{Gamma}(\alpha, \beta)$ | $\text{Gamma}(\alpha + n, \beta + \sum x_i)$ |
| $N(\mu, \sigma^2)$ known $\sigma$ | $\mu$ | $N(\mu_0, \tau^2)$ | $N\left(\frac{\tau^{-2}\mu_0 + n\sigma^{-2}\bar{x}}{\tau^{-2} + n\sigma^{-2}}, (\tau^{-2}+n\sigma^{-2})^{-1}\right)$ |
| $N(\mu, \sigma^2)$ known $\mu$ | $\sigma^2$ | $\text{Inv-Gamma}(\alpha, \beta)$ | $\text{Inv-Gamma}(\alpha + n/2, \beta + \sum(x_i-\mu)^2/2)$ |
| $\text{Multinomial}(n, \mathbf{p})$ | $\mathbf{p}$ | $\text{Dirichlet}(\boldsymbol{\alpha})$ | $\text{Dirichlet}(\boldsymbol{\alpha} + \mathbf{x})$ |
| $\text{Geometric}(p)$ | $p$ | $\text{Beta}(\alpha, \beta)$ | $\text{Beta}(\alpha + n, \beta + \sum x_i - n)$ |
| $\text{NegBin}(r, p)$ known $r$ | $p$ | $\text{Beta}(\alpha, \beta)$ | $\text{Beta}(\alpha + nr, \beta + \sum x_i - nr)$ |
| $N(\mu, \sigma^2)$ both unknown | $(\mu, \sigma^2)$ | $\text{Normal-Inv-Gamma}$ | Normal-Inv-Gamma (updated) |

---

# 🎓 50 Essential Theorems & Results

| # | Theorem/Result | Statement |
|---|---|---|
| 1 | Bayes' Theorem | $P(A|B) = P(B|A)P(A)/P(B)$ |
| 2 | Law of Total Probability | $P(B) = \sum P(B|A_i)P(A_i)$ |
| 3 | Inclusion-Exclusion | $P(\bigcup A_i) = \sum P(A_i) - \sum P(A_i \cap A_j) + \cdots$ |
| 4 | CLT | $\sqrt{n}(\bar{X}-\mu)/\sigma \xrightarrow{d} N(0,1)$ |
| 5 | WLLN | $\bar{X}_n \xrightarrow{P} \mu$ |
| 6 | SLLN | $\bar{X}_n \xrightarrow{a.s.} \mu$ |
| 7 | Borel-Cantelli 1 | $\sum P(A_n) < \infty \implies P(\text{i.o.}) = 0$ |
| 8 | Borel-Cantelli 2 | If independent, $\sum P(A_n) = \infty \implies P(\text{i.o.}) = 1$ |
| 9 | Continuous Mapping | $X_n \xrightarrow{d} X \implies g(X_n) \xrightarrow{d} g(X)$ |
| 10 | Slutsky | $X_n \xrightarrow{d} X$, $Y_n \xrightarrow{P} c \implies X_n + Y_n \xrightarrow{d} X + c$ |
| 11 | Delta Method | $\sqrt{n}(g(\bar{X})-g(\mu)) \xrightarrow{d} N(0, [g'(\mu)]^2\sigma^2)$ |
| 12 | Cramér-Rao | $\text{Var}(\hat\theta) \geq 1/(nI(\theta))$ for unbiased |
| 13 | Rao-Blackwell | Condition on sufficient stat improves estimator |
| 14 | Lehmann-Scheffé | UMVUE via complete sufficient statistic |
| 15 | Neyman-Pearson | LRT is most powerful for simple $H_0$ vs simple $H_1$ |
| 16 | Neyman Factorization | $T$ sufficient iff $f(\mathbf{x}|\theta) = g(T,\theta)h(\mathbf{x})$ |
| 17 | Basu's Theorem | Complete sufficient ⊥ ancillary |
| 18 | Fisher-Neyman | Minimal sufficient via likelihood ratio |
| 19 | Cochran's Theorem | Quadratic forms decomposition for normal |
| 20 | Gauss-Markov | OLS is BLUE under Gauss-Markov conditions |
| 21 | Glivenko-Cantelli | $\sup|F_n - F| \xrightarrow{a.s.} 0$ |
| 22 | Donsker | Empirical process converges to Brownian bridge |
| 23 | Kolmogorov 0-1 | Tail events have probability 0 or 1 |
| 24 | Markov Inequality | $P(X \geq a) \leq E[X]/a$ |
| 25 | Chebyshev | $P(|X-\mu| \geq k\sigma) \leq 1/k^2$ |
| 26 | Chernoff | $P(X \geq a) \leq \inf_t e^{-ta}M_X(t)$ |
| 27 | Hoeffding | $P(\bar{X}-\mu \geq t) \leq \exp(-2nt^2/(b-a)^2)$ |
| 28 | Jensen's Inequality | $E[g(X)] \geq g(E[X])$ for convex $g$ |
| 29 | Cauchy-Schwarz | $(E[XY])^2 \leq E[X^2]E[Y^2]$ |
| 30 | Data Processing | $I(X;Y) \geq I(X;g(Y))$ — processing can only lose info |
| 31 | Fano's Inequality | $H(X|Y) \leq H(P_e) + P_e\log(|X|-1)$ |
| 32 | Shannon Source Coding | Can compress at rate $\geq H(X)$ |
| 33 | Channel Coding | Can communicate at rate $\leq C$ |
| 34 | Ergodic Theorem (MC) | Time averages = space averages for ergodic chain |
| 35 | Perron-Frobenius | Irreducible nonneg matrix: unique max eigenvalue |
| 36 | Coupling Inequality | $\|P_t - \pi\|_{TV} \leq P(X_t \neq Y_t)$ |
| 37 | Optional Stopping | $E[M_\tau] = E[M_0]$ for martingale under conditions |
| 38 | Doob's Martingale | $E[X|\mathcal{F}_1], E[X|\mathcal{F}_2], \ldots$ is martingale |
| 39 | Kolmogorov Extension | Consistent finite-dim distributions define a process |
| 40 | De Finetti | Exchangeable $\implies$ mixture of i.i.d. |
| 41 | Savage | Subjective expected utility exists |
| 42 | Bernstein-von Mises | Posterior $\to$ normal centered at MLE |
| 43 | Bootstrap Consistency | Bootstrap approximation works asymptotically |
| 44 | Wilks' Theorem | $-2\log\Lambda \xrightarrow{d} \chi^2_k$ |
| 45 | Fisher Consistency | $T(F_\theta) = \theta$ |
| 46 | Karlin-Rubin | MLR gives UMP one-sided tests |
| 47 | Completeness | $E_\theta[g(T)] = 0 \; \forall\theta \implies g = 0$ a.s. |
| 48 | Sufficient ⊃ Minimal ⊃ Complete | Hierarchy |
| 49 | Exponential Family Properties | MLEs, sufficient stats, conjugate priors all "nice" |
| 50 | Information Inequality | Fisher info = rate of convergence of estimators |

---

# 🎯 GATE 2025-2027: Score Maximization Flowchart

```
START QUESTION
    │
    ├─ Read carefully (30 sec)
    │
    ├─ Is it 1-mark or 2-mark?
    │   ├─ 1-mark: Spend ≤ 2 min
    │   └─ 2-mark: Spend ≤ 4 min
    │
    ├─ Identify topic:
    │   ├─ Basic Prob/Bayes → Formula sheet
    │   ├─ Distribution → Parameter table
    │   ├─ Hypothesis test → Z/t/chi-sq table
    │   ├─ Markov chain → Matrix method
    │   └─ Information theory → Entropy formula
    │
    ├─ MCQ: Try elimination!
    │   ├─ Extreme cases (n=1, p=0, p=1)
    │   ├─ Dimensional analysis
    │   └─ Boundary checks
    │
    ├─ NAT: Double-check computation
    │   ├─ Recompute from scratch
    │   ├─ Verify with different method
    │   └─ Check if answer is reasonable
    │
    └─ STUCK? → Mark & Move ON!
        - Never spend > 5 min on one question
        - Come back if time permits
```

---

# 📊 Final Words of Wisdom 🏆

> "Probability is the mathematics of uncertainty, and statistics is the practice of learning from data."

## 🎯 The 5 Golden Rules for GATE Probability

1. **Draw a Venn diagram / tree / table** before computing
2. **Check if events are independent** — it changes everything
3. **Condition, condition, condition** — law of total probability is your best friend
4. **Match the distribution** — Binomial, Poisson, Normal, Exponential cover 80% of problems
5. **Verify with extreme cases** — does your answer make sense when $p = 0$, $p = 1$, $n = 1$, $n \to \infty$?

## 🎯 The 3 Most Common GATE Mistakes

1. **Confusing WITH and WITHOUT replacement** → Hypergeometric vs Binomial
2. **Forgetting to square in Chebyshev** → It's $1/k^2$, not $1/k$!
3. **Using wrong df in $\chi^2$** → Always subtract constraints!

## 📝 One Last Formula You MUST Know

$$\boxed{P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}}$$

This one formula connects **everything**: from basic probability to Bayesian ML to information theory. Master it, and you master this subject.

**Good luck on GATE 2027! You've got this! 🚀🎯💪**

---

# 📝 Practice Set 14: Distribution Identification Drill (P1101 – P1180)

> ⚡ Given the scenario, identify the CORRECT distribution. This is the #1 skill for GATE!

| # | Scenario | Distribution | Parameters |
|---|---|---|---|
| P1101 | Number of defectives in a batch of 20, each defective with prob 0.1 | Binomial | $n=20, p=0.1$ |
| P1102 | Number of emails per hour (avg 5/hr) | Poisson | $\lambda = 5$ |
| P1103 | Time until first customer arrives (avg 10 min between) | Exponential | $\lambda = 1/10$ |
| P1104 | Tosses until first head with $P(H) = 0.3$ | Geometric | $p = 0.3$ |
| P1105 | Tosses until 5th head with $P(H) = 0.3$ | Negative Binomial | $r=5, p=0.3$ |
| P1106 | Height of adults in a city | Normal | $\mu, \sigma$ from data |
| P1107 | 3 red drawn from urn of 10 red, 20 blue (sample of 8, no replacement) | Hypergeometric | $N=30, K=10, n=8$ |
| P1108 | Waiting time for 3rd event (Poisson process, rate 2/hr) | Gamma | $\alpha=3, \beta=2$ |
| P1109 | Fraction of time a machine is idle (between 0 and 1) | Beta | $\alpha, \beta$ estimated |
| P1110 | Sum of squared standard normals ($n = 5$) | Chi-squared | $df = 5$ |
| P1111 | Ratio of two chi-squared over their df | F | $df_1, df_2$ |
| P1112 | $Z/\sqrt{V/n}$ where $Z \sim N(0,1)$, $V \sim \chi^2_n$ | Student's $t$ | $df = n$ |
| P1113 | Arrival times uniform on [0,T] given $n$ arrivals in Poisson process | Uniform | each $U_i \sim U(0,T)$ |
| P1114 | Lifetime of component with constant failure rate | Exponential | $\lambda$ = failure rate |
| P1115 | Lifetime with increasing failure rate | Weibull | $k > 1$ |
| P1116 | Lifetime with decreasing failure rate | Weibull | $k < 1$ |
| P1117 | Number of misprints per page (rare event, large text) | Poisson | $\lambda$ = avg per page |
| P1118 | Stock price ratios (multiplicative) | Log-Normal | $\ln X \sim N(\mu, \sigma^2)$ |
| P1119 | Direction on a circle | Von Mises / Uniform$(0, 2\pi)$ | depends on concentration |
| P1120 | Category probabilities (Dirichlet-Multinomial) | Multinomial | $n, (p_1, \ldots, p_k)$ |

**P1121-P1180** — Express Distribution Matching:

| # | Clue | Distribution |
|---|---|---|
| P1121 | Memoryless, continuous | Exponential |
| P1122 | Memoryless, discrete | Geometric |
| P1123 | Mean = Variance | Poisson |
| P1124 | Fixed $n$ trials, binary outcome | Binomial |
| P1125 | Sum of independent Exp($\lambda$)'s | Gamma or Erlang |
| P1126 | $n$ identical independent Bernoulli trials | Binomial |
| P1127 | Sampling without replacement from finite pop | Hypergeometric |
| P1128 | Maximum entropy on $(0,\infty)$ with fixed mean | Exponential |
| P1129 | Maximum entropy on $\mathbb{R}$ with fixed mean & variance | Normal |
| P1130 | Maximum entropy on $\{1, \ldots, k\}$ | Discrete Uniform |
| P1131 | Conjugate prior for Bernoulli | Beta |
| P1132 | Conjugate prior for Poisson | Gamma |
| P1133 | Conjugate prior for Normal mean | Normal |
| P1134 | Conjugate prior for Multinomial | Dirichlet |
| P1135 | $X^2$ where $X \sim N(0,1)$ | $\chi^2_1$ |
| P1136 | $\sum Z_i^2$ for $n$ i.i.d. standard normals | $\chi^2_n$ |
| P1137 | $Z/\sqrt{\chi^2_n/n}$ | $t_n$ |
| P1138 | $(\chi^2_m/m)/(\chi^2_n/n)$ | $F_{m,n}$ |
| P1139 | Waiting for $r$th success in Bernoulli trials | Negative Binomial |
| P1140 | $-\ln(U)$ where $U \sim \text{Uniform}(0,1)$ | $\text{Exp}(1)$ |
| P1141 | $\max(X_1, \ldots, X_n)$ for i.i.d. Exp | Not standard (use order stats) |
| P1142 | $\min(X_1, \ldots, X_n)$ for i.i.d. Exp($\lambda$) | $\text{Exp}(n\lambda)$ |
| P1143 | Mixture of normals with unknown number of components | Infinite GMM (Dirichlet process) |
| P1144 | Binary data with overdispersion | Beta-Binomial |
| P1145 | Count data with overdispersion | Negative Binomial |
| P1146 | Count data with excess zeros | Zero-inflated Poisson |
| P1147 | Survival time with bathtub hazard | Mixture of Weibulls |
| P1148 | Time between events in Poisson process | Exponential |
| P1149 | Number of events in interval for Poisson process | Poisson |
| P1150 | $e^X$ where $X \sim N(\mu, \sigma^2)$ | Log-Normal |
| P1151 | Ratio of two Exp(1) | Standard Cauchy (pathological!) |
| P1152 | Sum of $n$ Uniform(0,1) for large $n$ | $\approx N(n/2, n/12)$ by CLT |
| P1153 | Product of uniform RVs: $-\sum\ln U_i$ | $\text{Gamma}(n, 1)$ |
| P1154 | Standardized max of $n$ i.i.d. | Gumbel / Frechet / Weibull (EVT) |
| P1155 | Spherical direction in $\mathbb{R}^d$ | Uniform on $S^{d-1}$; project = Beta |
| P1156 | Eigenvalues of random matrix | Wigner semicircle law |
| P1157 | Largest eigenvalue of Wishart | Tracy-Widom distribution |
| P1158 | Number of records in permutation | $\text{Bernoulli}(1/k)$ at position $k$; total $\sim H_n$ |
| P1159 | Run length in binary sequence | Geometric |
| P1160 | Balls into bins: occupancy of one bin | Binomial → Poisson (for large $n$) |
| P1161 | Random walk first passage time | Inverse Gaussian |
| P1162 | Branching process: total population | Depends on offspring distribution |
| P1163 | Random graph degree distribution | Binomial → Poisson |
| P1164 | Power-law degree distribution | Scale-free network |
| P1165 | Interarrival times in renewal process | Any positive continuous distribution |
| P1166 | Age and excess life in renewal | Related to inter-renewal CDF |
| P1167 | Signal + Gaussian noise | Shifted Normal |
| P1168 | Sum of signal + Rayleigh noise | Rice distribution |
| P1169 | RSS of two independent normals | Rayleigh |
| P1170 | RSS of $n$ independent normals | Chi distribution |
| P1171 | Squared RSS of $n$ normals ($\sigma = 1$) | $\chi^2_n$ |
| P1172 | Ratio of adjacent order stats | Beta(1, n-k) like |
| P1173 | Spacings of uniform order stats | Independent Exp! (after transform) |
| P1174 | $F^{-1}(U)$ where $U \sim U(0,1)$ | Distribution with CDF $F$! |
| P1175 | $F(X)$ where $X$ has CDF $F$ (continuous) | $U(0,1)$! (probability integral transform) |
| P1176 | $-2\log(\text{p-value})$ under $H_0$ | $\chi^2_2$ (Fisher's method) |
| P1177 | Combined $-2\sum\log(p_i)$ for $k$ independent tests | $\chi^2_{2k}$ |
| P1178 | QQ-plot: points on line $y = x$ | Data matches theoretical distribution |
| P1179 | PP-plot: CDF vs theoretical CDF | Cumulative comparison |
| P1180 | Empirical CDF: $\hat{F}_n(x) = \frac{1}{n}\sum I(X_i \leq x)$ | Step function; Glivenko-Cantelli: $\to F$ |

---

# 📝 Practice Set 15: Ultra-Quick One-Liners (P1181 – P1300)

| # | Q | A |
|---|---|---|
| P1181 | $0! = ?$ | $1$ |
| P1182 | $\binom{n}{0} = ?$ | $1$ |
| P1183 | $\binom{n}{n} = ?$ | $1$ |
| P1184 | $\sum_{k=0}^n \binom{n}{k} = ?$ | $2^n$ |
| P1185 | $\sum_{k=0}^n (-1)^k\binom{n}{k} = ?$ | $0$ |
| P1186 | $\binom{n}{k} = \binom{n}{n-k}$? | YES (symmetry) |
| P1187 | Vandermonde: $\sum_k \binom{m}{k}\binom{n}{r-k} = ?$ | $\binom{m+n}{r}$ |
| P1188 | Hockey stick: $\sum_{i=r}^n \binom{i}{r} = ?$ | $\binom{n+1}{r+1}$ |
| P1189 | $P(\emptyset) = ?$ | $0$ |
| P1190 | $P(\Omega) = ?$ | $1$ |
| P1191 | $P(A^c) = ?$ | $1 - P(A)$ |
| P1192 | $0 \leq P(A) \leq ?$ | $1$ (axiom) |
| P1193 | $P(A \cup B) \leq ?$ | $P(A) + P(B)$ (union bound) |
| P1194 | Total prob with $n$ partitions? | $P(B) = \sum_{i=1}^n P(B|A_i)P(A_i)$ |
| P1195 | Independent events: $P(A \cap B) = ?$ | $P(A)P(B)$ |
| P1196 | Mutually exclusive: $P(A \cap B) = ?$ | $0$ |
| P1197 | Can events be both independent AND mutually exclusive? | Only if $P(A) = 0$ or $P(B) = 0$ |
| P1198 | $E[aX + b] = ?$ | $aE[X] + b$ |
| P1199 | $\text{Var}(aX + b) = ?$ | $a^2\text{Var}(X)$ |
| P1200 | $E[X + Y] = ?$ (always) | $E[X] + E[Y]$ |
| P1201 | $\text{Var}(X + Y) = ?$ if independent | $\text{Var}(X) + \text{Var}(Y)$ |
| P1202 | $\text{Var}(X + Y)$ general? | $\text{Var}(X) + \text{Var}(Y) + 2\text{Cov}(X,Y)$ |
| P1203 | MGF: $M_X(t) = ?$ | $E[e^{tX}]$ |
| P1204 | $M_X'(0) = ?$ | $E[X]$ |
| P1205 | $M_X''(0) = ?$ | $E[X^2]$ |
| P1206 | $M_{aX+b}(t) = ?$ | $e^{bt}M_X(at)$ |
| P1207 | MGF of sum (independent)? | Product of MGFs |
| P1208 | PGF: $G(z) = ?$ | $E[z^X]$ |
| P1209 | $G'(1) = ?$ | $E[X]$ |
| P1210 | $G''(1) + G'(1) - (G'(1))^2 = ?$ | $\text{Var}(X)$ |
| P1211 | Bernoulli(p) mean? | $p$ |
| P1212 | Bernoulli(p) variance? | $p(1-p)$ |
| P1213 | Binomial(n,p) mean? | $np$ |
| P1214 | Binomial(n,p) variance? | $np(1-p)$ |
| P1215 | Poisson($\lambda$) mean? | $\lambda$ |
| P1216 | Poisson($\lambda$) variance? | $\lambda$ |
| P1217 | Geometric($p$) mean? | $1/p$ |
| P1218 | Geometric($p$) variance? | $(1-p)/p^2$ |
| P1219 | NegBin($r,p$) mean? | $r/p$ |
| P1220 | NegBin($r,p$) variance? | $r(1-p)/p^2$ |
| P1221 | Uniform($a,b$) mean? | $(a+b)/2$ |
| P1222 | Uniform($a,b$) variance? | $(b-a)^2/12$ |
| P1223 | Exp($\lambda$) mean? | $1/\lambda$ |
| P1224 | Exp($\lambda$) variance? | $1/\lambda^2$ |
| P1225 | Normal($\mu, \sigma^2$) mean? | $\mu$ |
| P1226 | Normal($\mu, \sigma^2$) variance? | $\sigma^2$ |
| P1227 | Gamma($\alpha, \beta$) mean (rate)? | $\alpha/\beta$ |
| P1228 | Gamma($\alpha, \beta$) variance (rate)? | $\alpha/\beta^2$ |
| P1229 | Beta($\alpha, \beta$) mean? | $\alpha/(\alpha+\beta)$ |
| P1230 | Beta($\alpha, \beta$) variance? | $\frac{\alpha\beta}{(\alpha+\beta)^2(\alpha+\beta+1)}$ |
| P1231 | $\chi^2_n$ mean? | $n$ |
| P1232 | $\chi^2_n$ variance? | $2n$ |
| P1233 | $t_n$ mean? | $0$ (for $n > 1$) |
| P1234 | $t_n$ variance? | $n/(n-2)$ (for $n > 2$) |
| P1235 | $F_{m,n}$ mean? | $n/(n-2)$ (for $n > 2$) |
| P1236 | Hypergeometric mean? | $nK/N$ |
| P1237 | Hypergeometric variance? | $n\frac{K}{N}\frac{N-K}{N}\frac{N-n}{N-1}$ |
| P1238 | Cauchy mean? | UNDEFINED! (no moments) |
| P1239 | Log-Normal mean? | $e^{\mu + \sigma^2/2}$ |
| P1240 | Log-Normal variance? | $(e^{\sigma^2}-1)e^{2\mu+\sigma^2}$ |
| P1241 | Weibull mean? | $\lambda\Gamma(1+1/k)$ |
| P1242 | Pareto($\alpha, x_m$) mean? | $\alpha x_m/(\alpha-1)$ for $\alpha > 1$ |
| P1243 | $\text{Cov}(X, X) = ?$ | $\text{Var}(X)$ |
| P1244 | $\text{Cov}(X, c) = ?$ | $0$ |
| P1245 | $\text{Cov}(aX, bY) = ?$ | $ab\text{Cov}(X,Y)$ |
| P1246 | Correlation range? | $[-1, 1]$ |
| P1247 | $\rho = 1$ means? | Perfect positive linear |
| P1248 | $\rho = 0$ means? | Uncorrelated (NOT necessarily independent!) |
| P1249 | Entropy of discrete $X$ | $H(X) = -\sum p_i\log p_i$ |
| P1250 | Entropy of continuous $X$ | $h(X) = -\int f\log f$ (differential entropy) |
| P1251 | Joint entropy | $H(X,Y) = -\sum\sum p_{ij}\log p_{ij}$ |
| P1252 | Conditional entropy | $H(Y|X) = H(X,Y) - H(X)$ |
| P1253 | Chain rule for entropy | $H(X,Y) = H(X) + H(Y|X)$ |
| P1254 | MI formula | $I(X;Y) = H(X) + H(Y) - H(X,Y) = H(X) - H(X|Y)$ |
| P1255 | $I(X;X) = ?$ | $H(X)$ (self-information = entropy) |
| P1256 | $KL(p\|q) \geq ?$ | $0$ (Gibbs' inequality) |
| P1257 | $KL(p\|q) = 0$ iff? | $p = q$ |
| P1258 | $H(X) \leq ?$ (discrete, $n$ values) | $\log n$ (achieved by uniform) |
| P1259 | Type I error | $\alpha = P(\text{reject}|H_0 \text{ true})$ |
| P1260 | Type II error | $\beta = P(\text{fail to reject}|H_1 \text{ true})$ |
| P1261 | Power | $1 - \beta$ |
| P1262 | p-value definition | $P(\text{data as extreme or more}|H_0)$ |
| P1263 | Confidence level $1-\alpha$: 95% uses $z$ = ? | $1.96$ |
| P1264 | 90% uses $z$ = ? | $1.645$ |
| P1265 | 99% uses $z$ = ? | $2.576$ |
| P1266 | Standard error of $\bar{X}$? | $\sigma/\sqrt{n}$ |
| P1267 | $df$ for chi-sq GOF with $k$ cells? | $k - 1$ |
| P1268 | $df$ for chi-sq independence $r \times c$? | $(r-1)(c-1)$ |
| P1269 | $df$ for one-sample $t$? | $n-1$ |
| P1270 | $df$ for pooled two-sample $t$? | $n_1+n_2-2$ |
| P1271 | Stationary distribution: $\pi P = ?$ | $\pi$ |
| P1272 | Detailed balance: $\pi_i P_{ij} = ?$ | $\pi_j P_{ji}$ (reversibility) |
| P1273 | Ergodic = ? | Irreducible + aperiodic + positive recurrent |
| P1274 | Absorbing MC: expected steps to absorption? | $(I - Q)^{-1}\mathbf{1}$ where $Q$ = transient block |
| P1275 | Random walk on line: recurrent in 1D? | YES (Polya) |
| P1276 | Random walk: recurrent in 2D? | YES |
| P1277 | Random walk: recurrent in 3D? | NO (transient!) |
| P1278 | PageRank damping factor? | $d \approx 0.85$ |
| P1279 | Markov property: future depends only on? | Present (not past) |
| P1280 | Hidden Markov Model: emissions depend on? | Hidden state only |
| P1281 | Viterbi algorithm does? | Most likely state sequence (dynamic programming) |
| P1282 | Forward algorithm does? | $P(\text{observations})$ |
| P1283 | Baum-Welch is? | EM for HMM parameter estimation |
| P1284 | MCMC target? | Sample from complex posterior |
| P1285 | Gibbs: sample each variable from? | Full conditional distribution |
| P1286 | MH acceptance rate guideline? | $\approx 23.4\%$ for high-dim normal targets |
| P1287 | ESS/second: what to maximize? | Effective sample size per second of computation |
| P1288 | Thinning MCMC samples: necessary? | Not usually (wastes samples); diagnose autocorrelation instead |
| P1289 | Gelman-Rubin diagnostic: $\hat{R} \approx 1$? | Chain has converged |
| P1290 | Geweke test? | Compare early vs late portions of chain |
| P1291 | Trace plot: what to look for? | Stationarity, good mixing |
| P1292 | Autocorrelation plot: want? | Rapid decay to 0 |
| P1293 | Sufficient statistic: reduces data without? | Loss of information about parameter |
| P1294 | Minimal sufficient: coarsest sufficient? | YES — most data reduction |
| P1295 | Ancillary: no info about parameter? | Distribution free of $\theta$ |
| P1296 | MLE: invariance property | $\hat{g(\theta)} = g(\hat\theta)$ |
| P1297 | MLE: asymptotically? | Normal, efficient, consistent |
| P1298 | Method of moments: match? | Population moments = sample moments |
| P1299 | Bayesian point estimate: posterior mean minimizes? | MSE (squared error loss) |
| P1300 | Bayesian point estimate: posterior median minimizes? | MAE (absolute error loss) |

---

# 📝 Practice Set 16: Final Exam Simulation — Complete Paper (P1301 – P1340)

> ⏱️ Time yourself: 40 questions, ~60 minutes. Mix of 1-mark and 2-mark.

**SECTION A: 1-mark questions (P1301 – P1320)**

**P1301.** $P(A) = 0.6$, $P(A^c \cap B) = 0.1$. Find $P(A \cup B)$.
→ $P(A \cup B) = P(A) + P(A^c \cap B) = 0.6 + 0.1 = 0.7$

**P1302.** $X \sim \text{Exp}(3)$. Find $P(X > 1)$.
→ $e^{-3} \approx 0.0498$

**P1303.** Entropy of a source: $(1/2, 1/4, 1/8, 1/8)$ in bits.
→ $1.75$ bits (computed in P965)

**P1304.** Mode of Poisson(4.7)?
→ $\lfloor\lambda\rfloor = 4$ (for non-integer $\lambda$, mode $= \lfloor\lambda\rfloor$)

**P1305.** If $X \sim N(0,1)$, $P(X^2 < 3.841)$?
→ $X^2 \sim \chi^2_1$. $P(\chi^2_1 < 3.841) = 0.95$

**P1306.** Rank of a $3 \times 4$ contingency table chi-squared test $df$?
→ $(3-1)(4-1) = 6$

**P1307.** MLE of $p$ in Bernoulli from 20 trials, 8 successes?
→ $\hat{p} = 8/20 = 0.4$

**P1308.** Covariance of $X$ and $3-2X$?
→ $\text{Cov}(X, 3-2X) = -2\text{Var}(X)$

**P1309.** Mean of $F_{5, 20}$?
→ $20/(20-2) = 20/18 = 10/9 \approx 1.111$

**P1310.** If MCMC chain satisfies detailed balance with $\pi$, then?
→ $\pi$ is stationary distribution

**P1311.** Sample size for margin of error $E$ at 95% confidence?
→ $n = (1.96\sigma/E)^2$

**P1312.** MGF of Poisson($\lambda$)?
→ $M(t) = e^{\lambda(e^t - 1)}$

**P1313.** $P(\text{at least 2 heads in 3 fair tosses})$?
→ $P(2) + P(3) = 3/8 + 1/8 = 4/8 = 1/2$

**P1314.** Median of continuous uniform on $[3, 9]$?
→ $(3+9)/2 = 6$

**P1315.** Third central moment is 0 for?
→ Symmetric distributions

**P1316.** Is Cauchy distribution stable?
→ Yes! Sum of two Cauchy's is Cauchy (with adjusted parameter)

**P1317.** Exponential(λ) CDF?
→ $F(x) = 1 - e^{-\lambda x}$ for $x \geq 0$

**P1318.** $E[\max(X, 0)]$ for $X \sim N(0,1)$?
→ $= \int_0^\infty x\phi(x)dx = \phi(0) = 1/\sqrt{2\pi} \approx 0.3989$

**P1319.** Sufficient statistic for Exp($\lambda$) from i.i.d. sample?
→ $T = \sum X_i$ (or equivalently $\bar{X}$)

**P1320.** Bias of $\hat\sigma^2 = \frac{1}{n}\sum(X_i - \bar{X})^2$?
→ $E[\hat\sigma^2] = \frac{n-1}{n}\sigma^2$. Bias $= -\sigma^2/n$

---

**SECTION B: 2-mark questions (P1321 – P1340)**

**P1321.** Three machines produce 30%, 45%, 25% of items. Defect rates: 2%, 3%, 1%. Item found defective → P(from machine 2)?
→ $P(M_2|D) = \frac{0.45 \times 0.03}{0.30 \times 0.02 + 0.45 \times 0.03 + 0.25 \times 0.01}$
→ $= \frac{0.0135}{0.006 + 0.0135 + 0.0025} = \frac{0.0135}{0.022} = \frac{135}{220} = \frac{27}{44} \approx 0.614$

**P1322.** $X, Y$ jointly normal, $\mu_X = 0$, $\sigma_X = 1$, $\mu_Y = 0$, $\sigma_Y = 1$, $\rho = 0.6$. Find $\text{Var}(Y|X = 2)$.
→ $\text{Var}(Y|X) = \sigma_Y^2(1-\rho^2) = 1(1-0.36) = 0.64$
→ Note: does NOT depend on the value of $X$!

**P1323.** 6 people sit at a round table. P(two specific people are adjacent)?
→ Fix one person: remaining 5 in $(5-1)! = 24$ arrangements (linear for remaining). 
→ Actually: circular permutations of 6 = $5! = 120$. Treat pair as block: $(5-1)! \times 2! = 48$.
→ $P = 48/120 = 2/5$

**P1324.** $X_1, \ldots, X_9$ i.i.d. $N(50, 81)$. $P(\bar{X} > 56)$?
→ $\bar{X} \sim N(50, 81/9) = N(50, 9)$. $\sigma_{\bar{X}} = 3$.
→ $P(Z > (56-50)/3) = P(Z > 2) = 0.0228$

**P1325.** Markov chain: states $\{1, 2, 3\}$, $P = \begin{pmatrix} 0.5 & 0.25 & 0.25 \\ 0.5 & 0 & 0.5 \\ 0.25 & 0.25 & 0.5 \end{pmatrix}$. Start at state 1. P(state 2 after 1 step)?
→ $P_{12} = 0.25$

**P1326.** Same chain. P(state 2 after 2 steps, starting at 1)?
→ $(P^2)_{12} = P_{11}P_{12} + P_{12}P_{22} + P_{13}P_{32}$
→ $= 0.5(0.25) + 0.25(0) + 0.25(0.25) = 0.125 + 0 + 0.0625 = 0.1875$

**P1327.** Test at $\alpha = 0.05$: $\chi^2_{obs} = 10.5$ with $df = 4$. $\chi^2_{0.05, 4} = 9.488$. Decision?
→ $10.5 > 9.488$: **Reject $H_0$**

**P1328.** Paired $t$-test: 8 pairs, $\bar{d} = 3.2$, $s_d = 4.5$. $t_{obs}$?
→ $t = \bar{d}/(s_d/\sqrt{n}) = 3.2/(4.5/\sqrt{8}) = 3.2/1.591 = 2.011$
→ $df = 7$, $t_{0.025, 7} = 2.365$. Since $2.011 < 2.365$: fail to reject at 5%.

**P1329.** Derive P(full house in poker)?
→ Full house = 3 of one rank + 2 of another
→ $\frac{\binom{13}{1}\binom{4}{3}\binom{12}{1}\binom{4}{2}}{\binom{52}{5}} = \frac{13 \times 4 \times 12 \times 6}{2598960} = \frac{3744}{2598960} \approx 0.00144$

**P1330.** $X \sim \text{Gamma}(2, 1)$. Find $P(X < 3)$.
→ $f(x) = xe^{-x}$. $F(3) = 1 - e^{-3}(1 + 3) = 1 - 4e^{-3} \approx 1 - 0.199 = 0.801$

**P1331.** Random sample of 200: 120 support policy. 95% CI for $p$?
→ $\hat{p} = 0.6$. $SE = \sqrt{0.6 \times 0.4/200} = \sqrt{0.0012} = 0.0346$
→ CI: $0.6 \pm 1.96 \times 0.0346 = (0.532, 0.668)$

**P1332.** Two Poisson processes: $\lambda_1 = 3/hr$, $\lambda_2 = 5/hr$. Merged: P(next event from process 1)?
→ Merged: Poisson($\lambda_1 + \lambda_2$) = Poisson(8). P(from process 1) $= \lambda_1/(\lambda_1+\lambda_2) = 3/8 = 0.375$

**P1333.** $X$ has CDF $F(x) = 1 - e^{-x^2}$ for $x > 0$. Find PDF and $E[X]$.
→ $f(x) = 2xe^{-x^2}$ (This is Rayleigh-like!)
→ $E[X] = \int_0^\infty 2x^2 e^{-x^2}dx = \sqrt{\pi}/2 \approx 0.886$

**P1334.** Law of total variance: $Y = $ return, $X = $ market condition (good/bad). $E[\text{Var}(Y|X)] = 10$, $\text{Var}(E[Y|X]) = 5$. $\text{Var}(Y) = ?$
→ $\text{Var}(Y) = 10 + 5 = 15$

**P1335.** Sample correlation $r = 0.45$, $n = 30$. Test $H_0: \rho = 0$ at 5%.
→ $t = r\sqrt{n-2}/\sqrt{1-r^2} = 0.45\sqrt{28}/\sqrt{1-0.2025} = 0.45(5.292)/0.893 = 2.667$
→ $t_{0.025, 28} \approx 2.048$. Since $2.667 > 2.048$: **Reject $H_0$** (significant correlation)

**P1336.** ANOVA: 3 groups, $n_1 = n_2 = n_3 = 10$. $SSB = 120$, $SSW = 270$. $F_{obs}$?
→ $MSB = 120/2 = 60$. $MSW = 270/27 = 10$. $F = 60/10 = 6.0$
→ $F_{0.05, 2, 27} \approx 3.35$. Since $6 > 3.35$: **Reject** (groups differ)

**P1337.** If $X \sim \text{Uniform}(0, \theta)$ and we observe $X_1=2, X_2=5, X_3=3$. MLE of $\theta$?
→ $\hat\theta = \max(X_i) = 5$
→ Method of moments: $E[X] = \theta/2 = \bar{X} = 10/3 \implies \hat\theta_{MOM} = 20/3 \approx 6.67$

**P1338.** Beta(1,1) = ?
→ Uniform(0,1)! (The simplest beta distribution)

**P1339.** Dirichlet(1,1,1) = ?
→ Uniform on the 2-simplex! (Uniform over probability vectors of length 3)

**P1340.** If prior is Beta(1,1) [uniform] and we observe 7 heads in 10 tosses:
→ Posterior: Beta(1+7, 1+3) = Beta(8, 4)
→ Posterior mean: $8/12 = 2/3$
→ MAP: $(8-1)/(8+4-2) = 7/10 = 0.7$ = MLE (from uniform prior!)

---

# 📝 Practice Set 17: Error Analysis & Debugging (P1341 – P1400)

> 🐛 Find the mistake in each "solution"!

**P1341.** "Student answer": $\text{Var}(X - Y) = \text{Var}(X) - \text{Var}(Y)$
→ ❌ WRONG! $\text{Var}(X - Y) = \text{Var}(X) + \text{Var}(Y) - 2\text{Cov}(X,Y)$
→ If independent: $\text{Var}(X) + \text{Var}(Y)$ (PLUS, not minus!)

**P1342.** "Student answer": Poisson is good approximation to Binomial when $n$ is large and $p$ is large.
→ ❌ WRONG! Need $p$ SMALL (and $np = \lambda$ moderate). For large $p$, use Normal approximation.

**P1343.** "Student answer": $P(A \cup B) = P(A) + P(B)$
→ ❌ Only if $A, B$ mutually exclusive! General: $P(A) + P(B) - P(A \cap B)$

**P1344.** "Student answer": MLE is always unbiased.
→ ❌ Counterexample: $\hat\sigma^2 = \frac{1}{n}\sum(x_i-\bar{x})^2$ for Normal. $E[\hat\sigma^2] = \frac{n-1}{n}\sigma^2 \neq \sigma^2$.

**P1345.** "Student answer": If $P(A|B) > P(A)$, then $P(B|A) > P(B)$.
→ ✅ Actually CORRECT! If learning B increases chance of A, then learning A increases chance of B. (Follows from $P(A|B)/P(A) = P(B|A)/P(B)$)

**P1346.** "Student answer": $E[1/X] = 1/E[X]$
→ ❌ WRONG! By Jensen's inequality (1/x is convex): $E[1/X] \geq 1/E[X]$

**P1347.** "Student answer": The CDF can decrease.
→ ❌ WRONG! CDF is always non-decreasing.

**P1348.** "Student answer": Normal PDF integrates to $\sqrt{2\pi}\sigma$.
→ ❌ Any PDF integrates to exactly 1!

**P1349.** "Student answer": For Exp($\lambda$), $P(X > s+t | X > s) = P(X > t) \times P(X > s)$
→ ❌ WRONG! Memoryless: $P(X > s+t | X > s) = P(X > t)$. No multiplication!

**P1350.** "Student answer": $\chi^2$ test needs $n > 30$.
→ ❌ $\chi^2$ needs EXPECTED counts $\geq 5$ per cell, not total $n > 30$.

**P1351-P1400** — Quick Error Spotting:

| # | Claimed Statement | Correct? | Fix |
|---|---|---|---|
| P1351 | $E[XY] = E[X]E[Y]$ always | ❌ | Only if uncorrelated |
| P1352 | $P(A|B) + P(A|B^c) = 1$ | ❌ | Not generally true; $P(A|B) + P(A^c|B) = 1$ |
| P1353 | Median of Exp(1) = 1 | ❌ | $\ln 2 \approx 0.693$ |
| P1354 | $t$-distribution is symmetric | ✅ | Symmetric about 0 |
| P1355 | $\chi^2$ distribution is symmetric | ❌ | Right-skewed |
| P1356 | $F$ distribution can be negative | ❌ | Always $\geq 0$ |
| P1357 | Normal is the only bell-shaped distribution | ❌ | $t$, Cauchy, Laplace also bell-shaped |
| P1358 | $P(A \cap B) \leq \min(P(A), P(B))$ | ✅ | intersection ⊆ each set |
| P1359 | Sample median is always unbiased for pop median | ❌ | True for symmetric distributions only |
| P1360 | Likelihood = Probability of parameter | ❌ | Likelihood = probability of DATA given parameter |
| P1361 | Prior must be proper (integrate to 1) | ❌ | Improper priors are fine if posterior is proper |
| P1362 | Bayesian = requiring prior, Frequentist = no prior | ✅ | Core philosophical difference |
| P1363 | Bootstrap always works | ❌ | Fails for extremes, heavy tails, dependent data |
| P1364 | Permutation test is exact | ✅ | Under $H_0$, all permutations equally likely |
| P1365 | AIC selects true model as $n \to \infty$ | ❌ | AIC is not consistent; BIC is |
| P1366 | Lower AIC is better | ✅ | AIC = $-2\ell + 2k$; lower = better fit-complexity |
| P1367 | $R^2$ can be negative | ✅ | For models without intercept! |
| P1368 | Adjusted $R^2$ can be negative | ✅ | If model is very poor |
| P1369 | Regression proves causation | ❌ | Only association; need experiments for causation |
| P1370 | Adding predictors always improves predictions | ❌ | Overfitting! Test error can increase |
| P1371 | Cross-validation is always better than AIC | ❌ | Depends on context; AIC ≈ LOOCV asymptotically |
| P1372 | LASSO does variable selection | ✅ | Sets some coefficients exactly to 0 |
| P1373 | Ridge does variable selection | ❌ | Ridge shrinks but never to exactly 0 |
| P1374 | Exponential family includes Cauchy | ❌ | Cauchy is NOT exponential family |
| P1375 | Sufficient stat always reduces dimension | ❌ | For non-exp-family, may need entire sample |
| P1376 | Neyman-Pearson applies to composite hypothesis | ❌ | Only simple vs simple |
| P1377 | Power increases with sample size | ✅ | More data → better detection |
| P1378 | CI width decreases with $n$ | ✅ | Width $\propto 1/\sqrt{n}$ |
| P1379 | CI for $\mu$ always contains $\bar{X}$ | ✅ | $\bar{X} \pm$ something → always centered at $\bar{X}$ |
| P1380 | Bayesian credible interval always contains posterior mode | ❌ | HPD does, equal-tail doesn't always |
| P1381 | MCMC is faster than exact computation | ❌ | MCMC is approximate; exact is better when feasible |
| P1382 | Ergodic chain always converges in finite time | ❌ | Converges asymptotically; in practice, need enough samples |
| P1383 | Variance of MCMC estimator = $\sigma^2/n$ | ❌ | MCMC samples correlated → variance = $\sigma^2 \times \tau/n$ |
| P1384 | Score function has mean 0 | ✅ | $E[\nabla\log f] = 0$ under regularity |
| P1385 | Fisher info is always positive | ✅ | $I(\theta) = E[U^2] \geq 0$ (= 0 only if degenerate) |
| P1386 | Two independent chi-sq can make an F | ✅ | $F = (\chi^2_m/m)/(\chi^2_n/n)$ |
| P1387 | Welch's $t$-test requires equal variances | ❌ | Welch's does NOT require equal variances (that's its point!) |
| P1388 | ANOVA assumes equal variances | ✅ | Homoscedasticity assumption |
| P1389 | Non-parametric tests are always less powerful | ❌ | Can be more robust; only slightly less powerful under exact assumptions |
| P1390 | $p = 0.049$ is "just barely significant at 5%" | ✅ | But this shouldn't change interpretation much |
| P1391 | Statistical significance = practical importance | ❌ | Effect size matters more! |
| P1392 | Large sample → normal approximation always safe | ❌ | Not for heavy tails (Cauchy) or extreme quantiles |
| P1393 | Confidence interval is a random interval | ✅ | Before data collection; after, it either contains $\theta$ or doesn't |
| P1394 | Posterior predictive check: can detect model misfit | ✅ | Simulate from model, compare to observed |
| P1395 | Leave-one-out CV: unbiased but high variance | ✅ | $K = n$ gives least bias, most variance |
| P1396 | 5-fold CV: more bias but less variance than LOOCV | ✅ | Good bias-variance tradeoff |
| P1397 | Stratified CV better for imbalanced data | ✅ | Preserves class ratios |
| P1398 | Time series data: random split CV is OK | ❌ | Must use forward-chaining / rolling window |
| P1399 | Elbow method always gives clear $k$ in clustering | ❌ | Often ambiguous; use silhouette or gap statistic |
| P1400 | Gaussian mixture model: EM can find global optimum | ❌ | EM converges to local optimum; use multiple restarts |

---

# 🧮 Quick Reference: Standard Normal Table (Key Values)

| $z$ | $\Phi(z)$ | $1-\Phi(z)$ | Context |
|---|---|---|---|
| 0 | 0.5000 | 0.5000 | Center |
| 0.5 | 0.6915 | 0.3085 | |
| 0.674 | 0.7500 | 0.2500 | Q1/Q3 |
| 1.0 | 0.8413 | 0.1587 | 68% rule |
| 1.282 | 0.9000 | 0.1000 | 80% CI |
| 1.645 | 0.9500 | 0.0500 | 90% CI |
| 1.96 | 0.9750 | 0.0250 | 95% CI ⭐ |
| 2.0 | 0.9772 | 0.0228 | |
| 2.326 | 0.9900 | 0.0100 | 98% CI |
| 2.576 | 0.9950 | 0.0050 | 99% CI |
| 3.0 | 0.9987 | 0.0013 | 99.7% rule |
| 3.29 | 0.9995 | 0.0005 | 99.9% CI |

---

# 🧮 Quick Reference: Chi-Squared Critical Values

| $df$ | $\chi^2_{0.05}$ | $\chi^2_{0.01}$ |
|---|---|---|
| 1 | 3.841 | 6.635 |
| 2 | 5.991 | 9.210 |
| 3 | 7.815 | 11.345 |
| 4 | 9.488 | 13.277 |
| 5 | 11.070 | 15.086 |
| 6 | 12.592 | 16.812 |
| 7 | 14.067 | 18.475 |
| 8 | 15.507 | 20.090 |
| 9 | 16.919 | 21.666 |
| 10 | 18.307 | 23.209 |

---

# 🧮 Quick Reference: $t$-Distribution Critical Values

| $df$ | $t_{0.05}$ (one-tail) | $t_{0.025}$ (two-tail 5%) |
|---|---|---|
| 1 | 6.314 | 12.706 |
| 2 | 2.920 | 4.303 |
| 3 | 2.353 | 3.182 |
| 4 | 2.132 | 2.776 |
| 5 | 2.015 | 2.571 |
| 10 | 1.812 | 2.228 |
| 15 | 1.753 | 2.131 |
| 20 | 1.725 | 2.086 |
| 25 | 1.708 | 2.060 |
| 30 | 1.697 | 2.042 |
| $\infty$ | 1.645 | 1.960 |

---

# 📊 Updated Index of All Practice Problems

| Set | Problems | Topics | Count |
|---|---|---|---|
| Set 1 | P1 – P80 | Fundamentals, Bayes, Distributions | 80 |
| Set 2 | P81 – P160 | GATE Simulation | 80 |
| Set 3 | P161 – P250 | Advanced, Tricky, Statistical Concepts | 90 |
| Set 4 | P251 – P320 | Markov Chains, Stochastic Processes | 70 |
| Set 5 | P321 – P420 | Expectation, Variance, Statistical Tests | 100 |
| Set 6 | P421 – P500 | Distributions, Transforms, Information Theory | 80 |
| Set 7 | P501 – P600 | GATE Predicted, ML Connections | 100 |
| Set 8 | P601 – P750 | GATE Marathon | 150 |
| Set 9 | P751 – P830 | True/False Conceptual + Speed | 80 |
| Set 10 | P831 – P900 | Express Final + ML/Stats Connections | 70 |
| Set 11 | P901 – P950 | Worked Examples — Full Solutions | 50 |
| Set 12 | P951 – P1020 | GATE PYQ Reproductions | 70 |
| Set 13 | P1021 – P1100 | Cross-Topic Integration (Prob + ML/Algo) | 80 |
| Set 14 | P1101 – P1180 | Distribution Identification Drill | 80 |
| Set 15 | P1181 – P1300 | Ultra-Quick One-Liners | 120 |
| Set 16 | P1301 – P1340 | Final Exam Simulation | 40 |
| Set 17 | P1341 – P1400 | Error Analysis & Debugging | 60 |
| **TOTAL** | | | **~1400 problems** |

---

# 📝 Practice Set 18: Tricky GATE Traps & Edge Cases (P1401 – P1500)

> ⚠️ These are designed to catch the most common mistakes. Master these = avoid losing easy marks!

**P1401.** (TRAP) "The probability that A OR B occurs is $P(A) + P(B)$"
→ ONLY if mutually exclusive! Always check: $P(A \cap B) = 0$?

**P1402.** (TRAP) Geometric distribution: two conventions!
→ Convention 1: $P(X = k) = (1-p)^{k-1}p$, $k = 1, 2, \ldots$ (number of TRIALS)
→ Convention 2: $P(X = k) = (1-p)^k p$, $k = 0, 1, 2, \ldots$ (number of FAILURES)
→ ⚠️ Mean differs: $1/p$ vs $(1-p)/p$. **Always check which convention the question uses!**

**P1403.** (TRAP) Gamma distribution parameterization:
→ Shape-Rate: $f(x) = \frac{\beta^\alpha}{\Gamma(\alpha)}x^{\alpha-1}e^{-\beta x}$, mean $= \alpha/\beta$
→ Shape-Scale: $f(x) = \frac{1}{\Gamma(\alpha)\theta^\alpha}x^{\alpha-1}e^{-x/\theta}$, mean $= \alpha\theta$
→ ⚠️ $\beta = 1/\theta$. Check WHICH the question uses!

**P1404.** (TRAP) "Variance of difference = difference of variances"
→ WRONG! $\text{Var}(X-Y) = \text{Var}(X) + \text{Var}(Y) - 2\text{Cov}(X,Y)$
→ If independent: $\text{Var}(X) + \text{Var}(Y)$ — the sign is ALWAYS plus!

**P1405.** (TRAP) Normal approximation to Binomial:
→ With continuity correction: $P(X \leq k) \approx \Phi\left(\frac{k+0.5 - np}{\sqrt{np(1-p)}}\right)$
→ Without: $P(X \leq k) \approx \Phi\left(\frac{k - np}{\sqrt{np(1-p)}}\right)$
→ ⚠️ Continuity correction matters for small $n$!

**P1406.** (TRAP) $E[\min(X,Y)]$ for independent Exp:
→ $\min(\text{Exp}(\lambda_1), \text{Exp}(\lambda_2)) = \text{Exp}(\lambda_1 + \lambda_2)$
→ So $E[\min] = 1/(\lambda_1 + \lambda_2)$

**P1407.** (TRAP) "Sample mean = population mean"
→ NO! $E[\bar{X}] = \mu$ (unbiased), but any particular $\bar{x}$ is usually $\neq \mu$.

**P1408.** (TRAP) "95% CI means P(µ in CI) = 0.95"
→ FREQUENTIST: Incorrect interpretation! The interval is random, not $\mu$.
→ CORRECT: If we repeat the experiment many times, 95% of CIs will contain $\mu$.

**P1409.** (TRAP) Chi-squared test with small expected counts:
→ If ANY cell has expected count $< 5$: combine cells or use Fisher exact test.
→ ⚠️ Expected, not observed! Some students check observed counts.

**P1410.** (TRAP) Two-tailed vs one-tailed p-value:
→ Two-tailed p-value = $2 \times$ one-tailed p-value (for symmetric distributions)
→ ⚠️ Students often forget to double!

**P1411-P1450** — Tricky Computation Problems:

**P1411.** (2-mark) $X \sim \text{Binomial}(100, 0.01)$. $P(X \geq 3)$?
→ Use Poisson ($\lambda = 1$): $P(X \geq 3) = 1 - P(0) - P(1) - P(2)$
→ $= 1 - e^{-1}(1 + 1 + 0.5) = 1 - 2.5e^{-1} = 1 - 0.9197 = 0.0803$

**P1412.** (2-mark) $X$ has PDF $f(x) = 3x^2$ on $[0,1]$. Find the 75th percentile.
→ $F(x) = x^3$. Set $F(q) = 0.75$: $q = 0.75^{1/3} = (3/4)^{1/3} \approx 0.909$

**P1413.** (2-mark) $X \sim N(0,1)$. Find $E[X^4]$.
→ Fourth moment of standard normal: $E[X^4] = 3$ (use MGF or integration)
→ General: $E[X^{2n}] = (2n-1)!! = 1 \times 3 \times 5 \times \cdots \times (2n-1)$

**P1414.** (2-mark) Transform: $X \sim \text{Uniform}(0,1)$, $Y = -2\ln X$. Distribution of $Y$?
→ $F_Y(y) = P(Y \leq y) = P(-2\ln X \leq y) = P(X \geq e^{-y/2}) = 1 - e^{-y/2}$
→ $Y \sim \text{Exp}(1/2)$ or equivalently $\text{Gamma}(1, 1/2)$ or $\chi^2_2$

**P1415.** (2-mark) $X, Y$ i.i.d. Exp(1). Distribution of $X/(X+Y)$?
→ $X/(X+Y) \sim \text{Beta}(1,1) = \text{Uniform}(0,1)$!
→ More generally: if $X \sim \text{Gamma}(a, 1)$, $Y \sim \text{Gamma}(b, 1)$ indep, then $X/(X+Y) \sim \text{Beta}(a,b)$.

**P1416.** (2-mark) Conditional: $X|N \sim \text{Binomial}(N, p)$, $N \sim \text{Poisson}(\lambda)$. Distribution of $X$?
→ $X \sim \text{Poisson}(\lambda p)$! (thinning of Poisson)

**P1417.** (2-mark) Stein's lemma: If $X \sim N(\mu, \sigma^2)$ and $g$ differentiable with $E[|g'(X)|] < \infty$:
→ $\text{Cov}(X, g(X)) = \sigma^2 E[g'(X)]$
→ Application: $\text{Cov}(X, X^2) = \sigma^2 E[2X] = 2\sigma^2\mu$

**P1418.** (2-mark) Polya urn: Start with 1 red, 1 blue. Draw a ball, return it + 1 of same color. After $n$ draws, distribution of fraction of red balls?
→ $\text{Uniform}(0,1)$! (De Finetti's theorem / Beta(1,1) posterior)

**P1419.** (2-mark) Random partition: 4 people into 2 groups of 2. Number of ways?
→ $\binom{4}{2}/2! = 6/2 = 3$ (divide by $2!$ because groups unlabeled)

**P1420.** (2-mark) Secretary problem: 100 candidates, interview sequentially. Optimal strategy?
→ Skip first $100/e \approx 37$ candidates, then hire first who beats all skipped.
→ P(hiring best) $\to 1/e \approx 0.368$

**P1421.** (2-mark NAT) Matching problem: $n$ letters in $n$ envelopes randomly. $E[\text{matches}]$?
→ By indicator method: $E[\text{matches}] = \sum_{i=1}^n P(\text{letter } i \text{ correct}) = n \times 1/n = 1$
→ AMAZING: Expected matches = 1, regardless of $n$!

**P1422.** (2-mark) Same setup. $\text{Var}(\text{matches})$?
→ Also $= 1$! (by computing second moment via indicators)
→ For large $n$: number of matches $\approx \text{Poisson}(1)$

**P1423.** (2-mark) Capture-recapture: Pond has $N$ fish. Capture 100, tag them, release. Recapture 200, find 20 tagged. Estimate $N$?
→ Lincoln-Petersen: $\hat{N} = \frac{100 \times 200}{20} = 1000$
→ Chapman's correction: $\hat{N} = \frac{(100+1)(200+1)}{20+1} - 1 \approx 959$

**P1424.** (2-mark) Waiting paradox: Bus comes every 10 min (exactly). You arrive at random. Expected wait?
→ 5 min (midpoint of uniform waiting time)
→ BUT if buses arrive by Poisson process (avg every 10 min): expected wait = 10 min!
→ This is the **inspection paradox**: random arrival is more likely during longer inter-arrival intervals!

**P1425-P1450** — Express Traps:

| # | Common Trap | Correct Answer |
|---|---|---|
| P1425 | Confusing $P(A|B)$ and $P(B|A)$ (prosecutor's fallacy) | They're usually VERY different! |
| P1426 | Using $z$-test when $\sigma$ unknown and $n$ small | Use $t$-test! |
| P1427 | Forgetting continuity correction for discrete→continuous approx | Add/subtract 0.5 |
| P1428 | "Independent" vs "mutually exclusive": same thing? | NO! Opposite for nonzero-prob events |
| P1429 | $P(A \cap B)$ can be 0 with independent events? | Only if $P(A) = 0$ or $P(B) = 0$ |
| P1430 | Negative Binomial: "number of trials" or "number of failures"? | Check convention! |
| P1431 | Hypergeometric: "with" or "without" replacement? | WITHOUT (with → Binomial) |
| P1432 | F-test: always one-tailed? | Yes (right-tail for $F > 1$) |
| P1433 | $\chi^2$ test: always one-tailed? | Yes (right-tail: large $\chi^2$ = bad fit) |
| P1434 | Adding constant to RV: changes variance? | NO! $\text{Var}(X+c) = \text{Var}(X)$ |
| P1435 | Multiplying RV by constant: $\text{Var}(cX) = c\text{Var}(X)$? | NO! $c^2\text{Var}(X)$ |
| P1436 | $E[X^2] = (E[X])^2$? | NO! Unless $\text{Var}(X) = 0$ |
| P1437 | Sample from small population: use FPC? | Yes: $\text{SE} \times \sqrt{\frac{N-n}{N-1}}$ if $n/N > 0.05$ |
| P1438 | Two types of "error": rounding vs statistical | Don't confuse numerical error with Type I/II |
| P1439 | Confidence interval width: halve → need $4\times$ sample size | Width $\propto 1/\sqrt{n}$ → quadratic! |
| P1440 | p-value $= 0.001$ means effect is large? | NO! Might be small effect with huge $n$ |
| P1441 | 100 tests at $\alpha = 0.05$: expected false positives? | $0.05 \times 100 = 5$ |
| P1442 | Bonferroni for 20 tests: $\alpha$ per test? | $0.05/20 = 0.0025$ |
| P1443 | Correlation $= 0.99$: strong LINEAR relationship | Could still miss nonlinear |
| P1444 | $R^2 = 0.01$: model useless? | Maybe; depends on context (stock returns: $R^2 = 0.01$ is great!) |
| P1445 | Adding noise variable: $R^2$ never decreases | True! Use adjusted $R^2$ or test significance |
| P1446 | Multicollinearity: estimates wrong? | Estimates unbiased but unstable (high variance) |
| P1447 | VIF > 10: serious multicollinearity | Common rule of thumb |
| P1448 | Standardized residual > 2: outlier? | Roughly; formal test uses Bonferroni |
| P1449 | Normality of residuals needed for OLS? | Not for point estimates; needed for CI/tests |
| P1450 | Homoscedasticity needed for OLS? | Not for unbiasedness; needed for efficiency |

---

# 📝 Practice Set 19: Multi-Step GATE Simulation (P1501 – P1560)

**P1501.** ⭐ (2-mark) A hospital has 3 diagnostic tests for a disease (prevalence 5%). Sensitivities: 90%, 85%, 95%. Specificities: 95%, 90%, 98%. A patient tests positive on ALL THREE tests (assumed independent given disease status). P(disease)?

**Step 1:** $P(D) = 0.05$, $P(D^c) = 0.95$

**Step 2:** $P(\text{all pos}|D) = 0.90 \times 0.85 \times 0.95 = 0.72675$

**Step 3:** $P(\text{all pos}|D^c) = (1-0.95)(1-0.90)(1-0.98) = 0.05 \times 0.10 \times 0.02 = 0.0001$

**Step 4:** $P(D|\text{all pos}) = \frac{0.72675 \times 0.05}{0.72675 \times 0.05 + 0.0001 \times 0.95}$
$= \frac{0.0363375}{0.0363375 + 0.000095} = \frac{0.0363375}{0.0364325} \approx 0.9974$

🎯 Three positive tests raise posterior to 99.7%!

---

**P1502.** ⭐ (2-mark) Markov chain with transition matrix:
$$P = \begin{pmatrix} 0 & 1 & 0 \\ 0.5 & 0 & 0.5 \\ 0 & 1 & 0 \end{pmatrix}$$

(a) Is the chain irreducible? (b) Find stationary distribution. (c) Is it periodic?

**(a)** From state 1: 1→2→1 (or 2→3→2). All states communicate. **YES, irreducible.**

**(b)** $\pi P = \pi$:
- $\pi_1 = 0.5\pi_2$
- $\pi_2 = \pi_1 + \pi_3$
- $\pi_3 = 0.5\pi_2$
- $\pi_1 = \pi_3$. Let $\pi_1 = x$, then $\pi_2 = 2x$, $\pi_3 = x$.
- $x + 2x + x = 1 \implies x = 1/4$
- **$\pi = (1/4, 1/2, 1/4)$**

**(c)** Period of state 1: returns possible at steps 2, 4, 6, ... GCD = 2. **Period = 2.**
Chain is **periodic** (not aperiodic), so does NOT converge to stationary from any initial state.

---

**P1503.** ⭐ (2-mark) MLE for Uniform($0, \theta$): Sample = {2.1, 3.5, 1.8, 4.2, 2.7}. 

(a) MLE of $\theta$? (b) Is it biased? (c) Find UMVUE.

**(a)** $\hat\theta_{MLE} = \max(x_i) = 4.2$

**(b)** $E[X_{(n)}] = \frac{n}{n+1}\theta = \frac{5}{6}\theta \neq \theta$. **Biased** (underestimates).

**(c)** UMVUE: $\hat\theta_{UMVUE} = \frac{n+1}{n}X_{(n)} = \frac{6}{5}(4.2) = 5.04$

---

**P1504.** ⭐ (2-mark) A queueing system: arrivals Poisson(5/hr), service Exp(mean 10 min = rate 6/hr). M/M/1 queue.

(a) Utilization? (b) Expected queue length? (c) Expected waiting time?

**(a)** $\rho = \lambda/\mu = 5/6 \approx 0.833$

**(b)** $L = \frac{\rho}{1-\rho} = \frac{5/6}{1/6} = 5$ customers in system

**(c)** $W = \frac{1}{\mu - \lambda} = \frac{1}{6-5} = 1$ hour

---

**P1505-P1560** — Mixed GATE-style problems:

**P1505.** (1-mark) $P(A) = 0.3$, $P(B|A) = 0.8$. $P(A \cap B)$?
→ $P(A \cap B) = P(B|A)P(A) = 0.24$

**P1506.** (2-mark) 10 items: 3 defective. Inspector tests one by one (without replacement) until finding first defective. Expected number of tests?
→ First defective at position $k$: $P(k) = \frac{\binom{7}{k-1}}{\binom{10}{k}} \times \frac{3}{10-k+1}$... 
→ Simpler: $E[\text{first defective}] = (10+1)/(3+1) = 11/4 = 2.75$
→ (For $N$ items, $D$ defective: $E = (N+1)/(D+1)$)

**P1507.** (2-mark) Two i.i.d. $U(0,1)$ random variables. $E[\max(X_1, X_2)]$?
→ $E[\max] = \frac{n}{n+1} = \frac{2}{3}$ for $n = 2$ i.i.d. Uniform(0,1)
→ Similarly: $E[\min] = \frac{1}{n+1} = \frac{1}{3}$

**P1508.** (2-mark NAT) Suppose $\text{Cov}(X,Y) = 3$, $\text{Var}(X) = 4$, $\text{Var}(Y) = 9$. Find $\text{Var}(2X - 3Y + 5)$.
→ $\text{Var}(2X-3Y+5) = 4\text{Var}(X) + 9\text{Var}(Y) - 12\text{Cov}(X,Y)$
→ $= 16 + 81 - 36 = 61$

**P1509.** (1-mark) Correlation of $X$ and $aX+b$ for $a > 0$?
→ $\rho = 1$ (perfect positive correlation)

**P1510.** (1-mark) Correlation of $X$ and $aX+b$ for $a < 0$?
→ $\rho = -1$ (perfect negative correlation)

**P1511.** (2-mark) $X \sim \text{Poisson}(\lambda)$. Show $P(X = k+1) = \frac{\lambda}{k+1}P(X=k)$.
→ $\frac{P(X=k+1)}{P(X=k)} = \frac{e^{-\lambda}\lambda^{k+1}/(k+1)!}{e^{-\lambda}\lambda^k/k!} = \frac{\lambda}{k+1}$ ✓
→ This is the **recurrence relation** — useful for computing Poisson tables iteratively!

**P1512.** (2-mark) Inverse: Given $P(X \geq 1) = 0.95$ for Poisson($\lambda$). Find $\lambda$.
→ $P(X=0) = e^{-\lambda} = 0.05$
→ $\lambda = -\ln(0.05) = \ln 20 \approx 3.0$

**P1513.** (2-mark) Law of Iterated Expectations applied: $E[X] = E[E[X|Y]]$
→ If $Y = 1$ w.p. 0.3 and $Y = 2$ w.p. 0.7:
→ $E[X|Y=1] = 5$, $E[X|Y=2] = 10$
→ $E[X] = 0.3(5) + 0.7(10) = 1.5 + 7.0 = 8.5$

**P1514.** (2-mark) Jacobian transform: $X \sim \text{Exp}(1)$, $Y = X^2$. PDF of $Y$?
→ $x = \sqrt{y}$, $|dx/dy| = 1/(2\sqrt{y})$
→ $f_Y(y) = f_X(\sqrt{y}) \times |dx/dy| = e^{-\sqrt{y}}/(2\sqrt{y})$ for $y > 0$

**P1515.** (2-mark) Moment problem: find distribution with $E[X] = 1$, $E[X^2] = 2$, support $\{0, 1, 2\}$.
→ Let $P(X=0) = a$, $P(X=1) = b$, $P(X=2) = c$
→ $a + b + c = 1$, $b + 2c = 1$, $b + 4c = 2$
→ From last two: $2c = 1 \implies c = 1/2$, $b = 0$, $a = 1/2$
→ $P(X=0) = P(X=2) = 1/2$, $P(X=1) = 0$!

**P1516.** (2-mark) Bivariate transform: $U = X+Y$, $V = X-Y$ where $(X,Y)$ jointly normal standard, $\rho = 0$.
→ $U \sim N(0, 2)$, $V \sim N(0, 2)$ and $\text{Cov}(U,V) = \text{Var}(X) - \text{Var}(Y) = 0$
→ Since jointly normal and uncorrelated: $U \perp V$!

**P1517.** (2-mark NAT) Reliability: 3 components in PARALLEL, each with reliability 0.9. System reliability?
→ $R_{sys} = 1 - (1-0.9)^3 = 1 - 0.001 = 0.999$

**P1518.** (2-mark) SERIES of the same 3 components:
→ $R_{sys} = 0.9^3 = 0.729$

**P1519.** (2-mark) 2-out-of-3 system (works if ≥2 of 3 work), each reliability $p = 0.9$:
→ $R = \binom{3}{2}p^2(1-p) + \binom{3}{3}p^3 = 3(0.81)(0.1) + 0.729 = 0.243 + 0.729 = 0.972$

**P1520-P1560** — Final Express Round:

| # | Q | A |
|---|---|---|
| P1520 | Generating function of Bernoulli($p$)? | $G(z) = 1-p+pz$ |
| P1521 | PGF of Binomial($n,p$)? | $G(z) = (1-p+pz)^n$ |
| P1522 | PGF of Poisson($\lambda$)? | $G(z) = e^{\lambda(z-1)}$ |
| P1523 | PGF of Geometric($p$)? | $G(z) = pz/(1-(1-p)z)$ |
| P1524 | Convolution of two Poissons? | Poisson($\lambda_1 + \lambda_2$) |
| P1525 | Sum of independent normals? | Normal($\mu_1+\mu_2$, $\sigma_1^2+\sigma_2^2$) |
| P1526 | Sum of gammas (same rate)? | Gamma($\alpha_1+\alpha_2$, $\beta$) |
| P1527 | Sum of chi-squared? | $\chi^2_{n_1+n_2}$ |
| P1528 | Sample mean of $n$ normals: distribution? | $N(\mu, \sigma^2/n)$ (exact, not just CLT) |
| P1529 | $(n-1)S^2/\sigma^2$ for normal pop? | $\chi^2_{n-1}$ |
| P1530 | $\bar{X}$ and $S^2$ independent for normal? | YES (Cochran's theorem) |
| P1531 | Total prob: $P(A) = \sum P(A|B_i)P(B_i)$? | Requires $B_i$ partition $\Omega$ |
| P1532 | Birthday problem ($n$ people, 365 days): formula? | $P(\text{no match}) = \prod_{k=0}^{n-1}(1-k/365)$ |
| P1533 | For $n=23$: $P(\text{match}) \approx$? | $0.507$ |
| P1534 | For $n=50$: $P(\text{match}) \approx$? | $0.970$ |
| P1535 | Runs test: tests for? | Randomness of sequence |
| P1536 | Sign test: tests? | Median; nonparametric |
| P1537 | Spearman rank correlation vs Pearson? | Spearman for monotone; Pearson for linear |
| P1538 | Kendall's $\tau$ vs Spearman's $\rho_s$? | Kendall: concordant/discordant pairs |
| P1539 | Point-biserial correlation? | Between binary and continuous variable |
| P1540 | Phi coefficient? | Between two binary variables |
| P1541 | Cramér's V? | Generalization of phi for larger tables |
| P1542 | Cohen's $d$ (effect size)? | $d = (\bar{X}_1 - \bar{X}_2)/S_{pooled}$ |
| P1543 | Small/medium/large $d$? | $0.2/0.5/0.8$ (Cohen's guidelines) |
| P1544 | Odds ratio = 1 means? | No association |
| P1545 | Relative risk vs odds ratio? | RR = $P(D|E)/P(D|\neg E)$; OR uses odds |
| P1546 | Simpson's paradox? | Aggregate trend reverses within subgroups |
| P1547 | Ecological fallacy? | Inferring individual from aggregate |
| P1548 | Berkson's paradox? | Conditioning on collider creates spurious association |
| P1549 | Regression to the mean? | Extreme observations tend to regress toward mean |
| P1550 | Galton's insight: what's the correlation with height of parents? | $\rho \approx 0.5$; children's height regresses to mean |
| P1551 | Stratification removes? | Confounding (by balancing groups) |
| P1552 | Propensity score matching? | Match treated/control by P(treatment|covariates) |
| P1553 | Instrumental variable: 3 conditions? | Relevant, excludable, independent of confounders |
| P1554 | Difference-in-differences? | Compare change in treatment vs control group |
| P1555 | Regression discontinuity? | Exploit cutoff in treatment assignment |
| P1556 | Survival analysis: Kaplan-Meier? | Nonparametric survival curve with censoring |
| P1557 | Cox proportional hazards? | $h(t|x) = h_0(t)\exp(\beta^Tx)$ (semiparametric) |
| P1558 | Hazard rate for Exp($\lambda$)? | $\lambda$ (constant!) |
| P1559 | Hazard rate for Weibull? | $\lambda k t^{k-1}$ (increasing if $k>1$) |
| P1560 | Median survival time? | Solve $S(t) = 0.5$ |

---

# 🎓 Module 8: Probability Distributions — Master Comparison Table

| Distribution | Type | Support | Parameters | Mean | Variance | PMF/PDF |
|---|---|---|---|---|---|---|
| Bernoulli | Discrete | $\{0,1\}$ | $p$ | $p$ | $p(1-p)$ | $p^x(1-p)^{1-x}$ |
| Binomial | Discrete | $\{0,...,n\}$ | $n,p$ | $np$ | $np(1-p)$ | $\binom{n}{x}p^x(1-p)^{n-x}$ |
| Geometric | Discrete | $\{1,2,...\}$ | $p$ | $1/p$ | $(1-p)/p^2$ | $(1-p)^{x-1}p$ |
| NegBin | Discrete | $\{r,r+1,...\}$ | $r,p$ | $r/p$ | $r(1-p)/p^2$ | $\binom{x-1}{r-1}p^r(1-p)^{x-r}$ |
| Poisson | Discrete | $\{0,1,...\}$ | $\lambda$ | $\lambda$ | $\lambda$ | $e^{-\lambda}\lambda^x/x!$ |
| Hypergeom | Discrete | $\{\max(0,n-N+K),...,\min(n,K)\}$ | $N,K,n$ | $nK/N$ | complex | $\binom{K}{x}\binom{N-K}{n-x}/\binom{N}{n}$ |
| Uniform(a,b) | Continuous | $[a,b]$ | $a,b$ | $(a+b)/2$ | $(b-a)^2/12$ | $1/(b-a)$ |
| Exponential | Continuous | $[0,\infty)$ | $\lambda$ | $1/\lambda$ | $1/\lambda^2$ | $\lambda e^{-\lambda x}$ |
| Normal | Continuous | $\mathbb{R}$ | $\mu,\sigma^2$ | $\mu$ | $\sigma^2$ | $\frac{1}{\sigma\sqrt{2\pi}}e^{-(x-\mu)^2/(2\sigma^2)}$ |
| Gamma | Continuous | $(0,\infty)$ | $\alpha,\beta$ | $\alpha/\beta$ | $\alpha/\beta^2$ | $\frac{\beta^\alpha}{\Gamma(\alpha)}x^{\alpha-1}e^{-\beta x}$ |
| Beta | Continuous | $(0,1)$ | $\alpha,\beta$ | $\alpha/(\alpha+\beta)$ | formula | $\frac{x^{\alpha-1}(1-x)^{\beta-1}}{B(\alpha,\beta)}$ |
| $\chi^2$ | Continuous | $(0,\infty)$ | $n$ | $n$ | $2n$ | Gamma($n/2$, $1/2$) |
| $t$ | Continuous | $\mathbb{R}$ | $\nu$ | $0$ | $\nu/(\nu-2)$ | via formula |
| $F$ | Continuous | $(0,\infty)$ | $m,n$ | $n/(n-2)$ | complex | via formula |
| Cauchy | Continuous | $\mathbb{R}$ | $x_0, \gamma$ | UNDEF | UNDEF | $\frac{1}{\pi\gamma[1+((x-x_0)/\gamma)^2]}$ |
| Weibull | Continuous | $(0,\infty)$ | $\lambda,k$ | $\lambda\Gamma(1+1/k)$ | complex | $\frac{k}{\lambda}(\frac{x}{\lambda})^{k-1}e^{-(x/\lambda)^k}$ |
| LogNormal | Continuous | $(0,\infty)$ | $\mu,\sigma$ | $e^{\mu+\sigma^2/2}$ | $(e^{\sigma^2}-1)e^{2\mu+\sigma^2}$ | via $\ln X \sim N$ |
| Pareto | Continuous | $[x_m,\infty)$ | $x_m,\alpha$ | $\alpha x_m/(\alpha-1)$ | if $\alpha>2$ | $\alpha x_m^\alpha/x^{\alpha+1}$ |

---

# 📊 Final Updated Problem Index

| Set | Problems | Topics | Count |
|---|---|---|---|
| Set 1 | P1 – P80 | Fundamentals, Bayes, Distributions | 80 |
| Set 2 | P81 – P160 | GATE Simulation | 80 |
| Set 3 | P161 – P250 | Advanced, Tricky, Statistical Concepts | 90 |
| Set 4 | P251 – P320 | Markov Chains, Stochastic Processes | 70 |
| Set 5 | P321 – P420 | Expectation, Variance, Statistical Tests | 100 |
| Set 6 | P421 – P500 | Distributions, Transforms, Info Theory | 80 |
| Set 7 | P501 – P600 | GATE Predicted, ML Connections | 100 |
| Set 8 | P601 – P750 | GATE Marathon | 150 |
| Set 9 | P751 – P830 | True/False Conceptual + Speed | 80 |
| Set 10 | P831 – P900 | Express Final + ML/Stats Connections | 70 |
| Set 11 | P901 – P950 | Worked Examples — Full Solutions | 50 |
| Set 12 | P951 – P1020 | GATE PYQ Reproductions | 70 |
| Set 13 | P1021 – P1100 | Cross-Topic Integration (Prob + ML/Algo) | 80 |
| Set 14 | P1101 – P1180 | Distribution Identification Drill | 80 |
| Set 15 | P1181 – P1300 | Ultra-Quick One-Liners | 120 |
| Set 16 | P1301 – P1340 | Final Exam Simulation | 40 |
| Set 17 | P1341 – P1400 | Error Analysis & Debugging | 60 |
| Set 18 | P1401 – P1500 | Tricky GATE Traps & Edge Cases | 100 |
| Set 19 | P1501 – P1560 | Multi-Step GATE Simulation | 60 |
| **GRAND TOTAL** | | | **~1560 problems** |

---

# 📝 Practice Set 20: Extreme Value & Tail Probability (P1561 – P1640)

> 🎯 These cover the hardest topics that appear in GATE DA — tail bounds, convergence, and extreme distributions.

**P1561.** (2-mark) Using Markov: $X \geq 0$, $E[X] = 4$. Upper bound on $P(X \geq 20)$?
→ $P(X \geq 20) \leq E[X]/20 = 4/20 = 0.2$

**P1562.** (2-mark) Using Chebyshev: $E[X] = 10$, $\text{Var}(X) = 9$. Upper bound on $P(|X - 10| \geq 6)$?
→ $P(|X - \mu| \geq k\sigma) \leq 1/k^2$. Here $k = 6/3 = 2$.
→ $P \leq 1/4 = 0.25$

**P1563.** (2-mark) One-sided Chebyshev (Cantelli): $P(X - \mu \geq t) \leq ?$
→ $P(X - \mu \geq t) \leq \frac{\sigma^2}{\sigma^2 + t^2}$
→ For $\mu = 10$, $\sigma^2 = 9$, $t = 6$: $P \leq 9/(9+36) = 9/45 = 1/5 = 0.2$

**P1564.** (2-mark) Hoeffding for bounded RV: $X_i \in [0,1]$ i.i.d., $E[X_i] = 0.5$, $n = 100$. $P(\bar{X} > 0.6)$?
→ $P(\bar{X} - \mu > t) \leq \exp(-2nt^2/(b-a)^2)$
→ $= \exp(-2 \times 100 \times 0.01/1) = e^{-2} \approx 0.135$

**P1565.** (2-mark) Chernoff for Poisson: $X \sim \text{Poisson}(10)$. Bound $P(X > 20)$?
→ $P(X > (1+\delta)\mu) \leq \left(\frac{e^\delta}{(1+\delta)^{(1+\delta)}}\right)^\mu$
→ $\delta = 1$: $P(X > 20) \leq (e/4)^{10} = (0.6798)^{10} \approx 0.0215$

**P1566.** (2-mark) Berry-Esseen bound: CLT approximation error?
→ $\sup_x |F_n(x) - \Phi(x)| \leq \frac{C\rho}{\sigma^3\sqrt{n}}$ where $\rho = E[|X-\mu|^3]$, $C \leq 0.4748$

**P1567.** (2-mark) Extreme Value Theory: $\max(X_1, \ldots, X_n)$ for i.i.d. draws.
→ Three limit distributions (Fisher-Tippett-Gnedenko):
→ Gumbel (light tails, e.g., Normal): $\exp(-e^{-x})$
→ Fréchet (heavy tails, e.g., Cauchy): $\exp(-x^{-\alpha})$
→ Weibull (bounded above, e.g., Uniform): $\exp(-(-x)^\alpha)$

**P1568.** (2-mark) For $n$ i.i.d. Exp(1): $\max \approx \ln n$ (grows like $\ln n$!).
→ Normalized: $P(\max - \ln n \leq x) \to \exp(-e^{-x})$ (Gumbel)

**P1569.** (2-mark) For i.i.d. $N(0,1)$: $\max \approx \sqrt{2\ln n}$
→ Much slower than exponential since normal tails are thinner

**P1570.** (2-mark) Generalized Pareto Distribution (GPD): for exceedances over threshold $u$:
→ $H(y) = 1 - (1 + \xi y/\sigma)^{-1/\xi}$ for $\xi \neq 0$
→ $\xi > 0$: heavy tail (Fréchet domain)
→ $\xi = 0$: exponential tail (Gumbel domain)
→ $\xi < 0$: bounded tail (Weibull domain)

**P1571-P1600** — Convergence Concepts:

| # | Concept | Definition | Notation | Implies? |
|---|---|---|---|---|
| P1571 | Almost sure | $P(\lim X_n = X) = 1$ | $X_n \xrightarrow{a.s.} X$ | Strongest |
| P1572 | In probability | $\forall\epsilon: P(|X_n - X| > \epsilon) \to 0$ | $X_n \xrightarrow{P} X$ | Implied by a.s. |
| P1573 | In distribution | $F_n(x) \to F(x)$ at continuity points | $X_n \xrightarrow{d} X$ | Weakest |
| P1574 | In $L^p$ ($r$th mean) | $E[|X_n - X|^p] \to 0$ | $X_n \xrightarrow{L^p} X$ | Implies in prob |
| P1575 | a.s. → in prob? | YES | | |
| P1576 | in prob → a.s.? | NO (counterexample exists) | | |
| P1577 | in prob → in dist? | YES | | |
| P1578 | in dist → in prob? | NO (unless limit is constant!) | | |
| P1579 | $L^2$ → $L^1$? | YES (by Jensen) | | |
| P1580 | $L^1$ → in prob? | YES (by Markov) | | |

**P1581.** WLLN conditions: $X_i$ i.i.d., $E[|X|] < \infty$ → $\bar{X}_n \xrightarrow{P} \mu$
**P1582.** SLLN conditions: Same → $\bar{X}_n \xrightarrow{a.s.} \mu$
**P1583.** CLT conditions: $\text{Var}(X) < \infty$ → $\sqrt{n}(\bar{X}_n - \mu)/\sigma \xrightarrow{d} N(0,1)$
**P1584.** Lindeberg CLT: non-i.i.d. version, Lindeberg condition suffices
**P1585.** Lyapunov CLT: easier condition, $(s_n^{-2-\delta})\sum E[|X_i - \mu_i|^{2+\delta}] \to 0$
**P1586.** Continuous Mapping: $X_n \xrightarrow{d} X$, $g$ continuous → $g(X_n) \xrightarrow{d} g(X)$
**P1587.** Slutsky: $X_n \xrightarrow{d} X$, $Y_n \xrightarrow{P} c$ → $X_n + Y_n \xrightarrow{d} X + c$
**P1588.** Portmanteau Lemma: equivalent conditions for convergence in distribution
**P1589.** Helly's Theorem: tight sequences have convergent subsequences
**P1590.** Prohorov's Theorem: tightness ↔ relative compactness (in metric space of distributions)

**P1591-P1600** — Convergence TRUE/FALSE:

| # | Statement | T/F |
|---|---|---|
| P1591 | $X_n = 1/n$ converges a.s. to 0 | T |
| P1592 | $X_n \sim N(0, 1/n)$ converges in probability to 0 | T |
| P1593 | Same: converges almost surely to 0 | T |
| P1594 | $X_n \sim N(0, 1)$ converges in distribution to $N(0,1)$ | T (trivially) |
| P1595 | Same: converges in probability to $N(0,1)$? | Not to a specific RV; meaningless unless target is constant |
| P1596 | $X_n \sim \text{Uniform}(0, 1/n)$: converges in prob to 0 | T ($P(X_n > \epsilon) = \max(0, 1-n\epsilon) \to 0$) |
| P1597 | $X_n \sim \text{Cauchy}(0, 1/n)$: $\bar{X}_n$ converges to 0? | NO! (WLLN fails: no finite mean) |
| P1598 | $X_n \sim t_{n}$: converges in distribution to $N(0,1)$ | T |
| P1599 | $\text{Poisson}(n)/n$ converges in prob to 1 | T (by WLLN, or $E = 1$, $\text{Var} = 1/n \to 0$) |
| P1600 | $\chi^2_n/n$ converges in prob to 1 | T (same reasoning) |

---

# 📝 Practice Set 21: Bayesian Deep Dive (P1601 – P1680)

**P1601.** (2-mark) Prior: $\theta \sim \text{Uniform}(0,1)$. Data: 3 heads in 5 tosses (Binomial). Posterior?
→ Prior = Beta(1,1). Posterior: Beta(1+3, 1+2) = Beta(4,3)
→ Mean: 4/7 ≈ 0.571. Mode: 3/5 = 0.6 = MLE!

**P1602.** (2-mark) Same data, prior Beta(2,2) (informative). Posterior?
→ Beta(2+3, 2+2) = Beta(5,4). Mean: 5/9 ≈ 0.556.
→ Informative prior pulled estimate toward 0.5!

**P1603.** (2-mark) Same data, prior Beta(10,10) (strongly informative). Posterior?
→ Beta(13, 12). Mean: 13/25 = 0.52.
→ Strong prior dominates the 5 observations!

**P1604.** (2-mark) With $n = 1000$, 600 heads. Prior Beta(10,10). Posterior?
→ Beta(610, 410). Mean: 610/1020 ≈ 0.598.
→ With lots of data, prior doesn't matter much! (Bernstein-von Mises)

**P1605.** (2-mark) Bayesian hypothesis testing: compute $P(H_0 | \text{data})$ directly.
→ Prior odds $\times$ Bayes Factor = Posterior odds
→ If $P(H_0)/P(H_1) = 1$ (equal prior): posterior odds = Bayes factor

**P1606.** (2-mark) For model comparison: $BF_{01} = 15$ means?
→ Data is 15 times more likely under $H_0$ than $H_1$. Strong evidence for $H_0$.

**P1607-P1640** — Bayesian Express:

| # | Topic | Q | A |
|---|---|---|---|
| P1607 | Conjugate | Prior for Normal variance? | Inverse-Gamma |
| P1608 | Conjugate | Prior for multivariate Normal mean? | Multivariate Normal |
| P1609 | Conjugate | Prior for regression coefficients? | Normal (Ridge = Gaussian prior!) |
| P1610 | Shrinkage | Bayesian Ridge = ? prior | $\beta_j \sim N(0, \tau^2)$ |
| P1611 | Shrinkage | Bayesian LASSO = ? prior | $\beta_j \sim \text{Laplace}(0, b)$ |
| P1612 | Shrinkage | Horseshoe prior for? | Sparse signals; heavy tails + spike at 0 |
| P1613 | Computation | Conjugate advantage? | Closed-form posterior |
| P1614 | Computation | Non-conjugate: need? | MCMC, VI, or Laplace approximation |
| P1615 | Laplace approx | Posterior ≈ ? | $N(\hat\theta, [nI(\hat\theta)]^{-1})$ |
| P1616 | Model evidence | $P(\text{data}) = \int P(\text{data}|\theta)\pi(\theta)d\theta$ | Marginal likelihood |
| P1617 | Evidence | Hard to compute because? | High-dimensional integral |
| P1618 | Harmonic mean | Estimator of evidence: $\hat{P} = [n^{-1}\sum L_i^{-1}]^{-1}$? | Known to be TERRIBLE (infinite variance!) |
| P1619 | Bridge sampling | Better evidence estimator? | Uses samples from prior AND posterior |
| P1620 | Savage-Dickey | BF from posterior at point null? | $BF_{01} = \pi(\theta_0|\text{data})/\pi(\theta_0)$ |
| P1621 | Credible interval | 95% HPD vs equal-tail? | HPD = shortest; equal-tail = 2.5% each side |
| P1622 | For skewed posterior? | HPD is narrower (asymmetric) |
| P1623 | Bayesian $p$-value | $P(\theta > 0 | \text{data})$? | Posterior probability of hypothesis |
| P1624 | Decision theory | Bayes estimator under squared loss? | Posterior mean |
| P1625 | Under absolute loss? | Posterior median |
| P1626 | Under 0-1 loss? | Posterior mode (MAP) |
| P1627 | Minimax | Best worst-case estimator? | Often equals Bayes with least favorable prior |
| P1628 | Admissibility | Bayes rules are admissible? | Yes (under mild conditions, Stein 1955) |
| P1629 | James-Stein | Shrinking $\bar{X}_i$ toward grand mean improves? | For $d \geq 3$ dimensions! (Paradox!) |
| P1630 | Empirical Bayes | Estimate prior from data? | Use marginal distribution to estimate hyperparameters |
| P1631 | Morris | EB for Normal means: shrinkage factor? | $1 - (d-2)/(nS)$ where $S = \sum(\bar{X}_i - \bar{\bar{X}})^2$ |
| P1632 | Hierarchical | $Y|\theta \sim f$, $\theta|\phi \sim g$, $\phi \sim h$? | Full Bayesian: infer all levels |
| P1633 | Exchangeability | $P(X_1,...,X_n)$ invariant to permutation? | Weaker than i.i.d.; implies mixture representation |
| P1634 | De Finetti | Infinite exchangeability implies? | Mixture of i.i.d. sequences |
| P1635 | Predictive | $P(X_{n+1}|X_1,...,X_n)$? | Integrate out parameter: weights by posterior |
| P1636 | For Beta-Binomial | After $s$ successes in $n$ trials (Beta(1,1) prior)? | $P(\text{next success}) = (s+1)/(n+2)$ — Laplace rule of succession! |
| P1637 | Cromwell's rule | Don't assign prior probability 0 (or 1) to anything? | Because Bayesian update can't overcome it |
| P1638 | Lindley's paradox | Bayesian and frequentist disagree for large $n$? | With point null and diffuse prior, BF can favor $H_0$ despite small p-value! |
| P1639 | Jeffreys-Lindley | Resolution? | Bayesian should use reasonable priors, not infinitely diffuse |
| P1640 | Objective Bayes | Jeffreys, reference, max entropy priors? | Attempt to be "non-informative" |

**P1641-P1680** — Bayesian Practice Problems:

**P1641.** (2-mark) Data: $x = 7$ from Poisson($\lambda$). Prior: $\lambda \sim \text{Gamma}(3, 1)$. Posterior?
→ $\lambda | x \sim \text{Gamma}(3 + 7, 1 + 1) = \text{Gamma}(10, 2)$
→ Mean: $10/2 = 5$. Mode: $(10-1)/2 = 4.5$

**P1642.** (2-mark) Data: $n = 20$ samples from Exp($\lambda$), $\sum x_i = 40$. Prior: $\lambda \sim \text{Gamma}(2, 1)$. Posterior?
→ $\lambda | \text{data} \sim \text{Gamma}(2+20, 1+40) = \text{Gamma}(22, 41)$
→ Mean: $22/41 \approx 0.537$. MLE: $n/\sum x_i = 20/40 = 0.5$

**P1643.** (2-mark) Bayesian A/B testing: Treatment A: 45 conversions in 500 (Beta(46,456)). Treatment B: 60 conversions in 500 (Beta(61,441)). $P(\theta_B > \theta_A)$?
→ Approximate: $\mu_A = 46/502 = 0.0916$, $\mu_B = 61/502 = 0.1215$
→ $\sigma_A^2 \approx \mu_A(1-\mu_A)/502 \approx 0.000166$, similar for B
→ $P(\theta_B > \theta_A) \approx \Phi\left(\frac{0.1215-0.0916}{\sqrt{0.000166+0.000214}}\right) = \Phi(1.53) \approx 0.937$

**P1644.** (2-mark) Predictive: Bayesian coin with Beta(3,3) prior. 4 heads in 6 tosses. P(heads on next toss)?
→ Posterior: Beta(7,5). Predictive: $P(H) = 7/12 \approx 0.583$

**P1645-P1680** — Bayesian Quick Fire:

| # | Q | A |
|---|---|---|
| P1645 | MAP with flat prior = ? | MLE |
| P1646 | Posterior ∝ ? | Likelihood $\times$ Prior |
| P1647 | As $n \to \infty$: posterior concentrates at? | True parameter (consistency) |
| P1648 | As $n \to \infty$: posterior shape? | $\approx N(\hat\theta_{MLE}, 1/(nI(\hat\theta)))$ |
| P1649 | Improper prior example? | $\pi(\theta) = 1$ on $\mathbb{R}$ (doesn't integrate to 1) |
| P1650 | Is $\pi(\sigma^2) = 1/\sigma^2$ (Jeffreys for scale) proper? | No (improper), but posterior is proper |
| P1651 | Jeffreys prior for Bernoulli? | Beta(1/2, 1/2) |
| P1652 | Reference prior for Normal mean? | Flat: $\pi(\mu) \propto 1$ |
| P1653 | Reference prior for Normal variance? | $\pi(\sigma^2) \propto 1/\sigma^2$ |
| P1654 | Bayes factor interpretation: $BF > 100$? | Decisive evidence |
| P1655 | $BF \in [3, 10]$? | Moderate evidence |
| P1656 | $BF \in [1, 3]$? | Weak evidence |
| P1657 | BIC approximation: $\log BF \approx ?$ | $-\frac{1}{2}\Delta BIC$ |
| P1658 | DIC (Deviance IC)? | Bayesian alternative to AIC |
| P1659 | WAIC? | Widely Applicable IC (fully Bayesian) |
| P1660 | LOO-CV in Bayesian? | PSIS-LOO (Pareto-smoothed importance sampling) |
| P1661 | Stan language for? | Bayesian MCMC (HMC/NUTS) |
| P1662 | PyMC for? | Bayesian modeling in Python |
| P1663 | BUGS/JAGS for? | Bayesian via Gibbs sampling |
| P1664 | Edward/TFP? | Bayesian deep learning |
| P1665 | Posterior predictive check: simulate from? | $p(y^{rep}|y) = \int p(y^{rep}|\theta)\pi(\theta|y)d\theta$ |
| P1666 | Bayesian $p$-value: $P(T(y^{rep}) \geq T(y_{obs}))$? | Check model fit |
| P1667 | Calibration: well-calibrated 95% CI should contain truth? | 95% of the time! |
| P1668 | Bayesian neural network? | Priors on weights → posterior → uncertainty! |
| P1669 | MC dropout ≈ ? | Approximate Bayesian inference |
| P1670 | Gaussian process regression is Bayesian? | YES — prior on functions |
| P1671 | Bayesian optimization uses? | GP + acquisition function (EI, UCB) |
| P1672 | Multi-armed bandit: Thompson sampling? | Sample from posterior, play best arm |
| P1673 | Bayesian clinical trial? | Adaptive: update P(treatment works) continuously |
| P1674 | Bayes' rule in spam filter? | $P(\text{spam}|\text{words})$ via Naive Bayes |
| P1675 | Bayesian networks: joint factorization? | $P(X_1,...,X_n) = \prod P(X_i | \text{parents}(X_i))$ |
| P1676 | d-separation? | Graphical criterion for conditional independence |
| P1677 | Markov blanket? | Parents + children + co-parents; makes $X$ independent of rest |
| P1678 | Belief propagation? | Message passing on graphical model |
| P1679 | Junction tree? | Exact inference on tree-width graphs |
| P1680 | Variational Bayes: minimize? | $KL(q\|p(\theta|x))$ → maximize ELBO |

---

# 📝 Practice Set 22: Numerical Constants Every Student Must Know (P1681 – P1720)

| # | Constant | Value | Where Used |
|---|---|---|---|
| P1681 | $e$ | $2.71828$ | Exponential, Poisson |
| P1682 | $e^{-1}$ | $0.36788$ | $P(X=0)$ for Poisson(1), secretary problem |
| P1683 | $e^{-2}$ | $0.13534$ | Poisson $\lambda=2$ |
| P1684 | $\pi$ | $3.14159$ | Normal, circular |
| P1685 | $\sqrt{2\pi}$ | $2.50663$ | Normal PDF normalizing constant |
| P1686 | $\ln 2$ | $0.69315$ | Median of Exp(1), doubling time |
| P1687 | $\ln 10$ | $2.30259$ | Converting natural ↔ common log |
| P1688 | $\sqrt{2}$ | $1.41421$ | $\sigma$ of $\chi^2_1$ |
| P1689 | $\sqrt{3}$ | $1.73205$ | $\sigma$ of Uniform(0,12) = $2\sqrt{3}$ |
| P1690 | $1/\sqrt{2\pi}$ | $0.39894$ | $\phi(0)$ — standard normal PDF at 0 |
| P1691 | $\Phi(1) = 0.8413$ | 68% rule one-sided | |
| P1692 | $\Phi(1.645) = 0.95$ | 90% CI one-sided | |
| P1693 | $\Phi(1.96) = 0.975$ | 95% CI one-sided | |
| P1694 | $\Phi(2.576) = 0.995$ | 99% CI one-sided | |
| P1695 | $\chi^2_{0.05, 1} = 3.841$ | Most common critical value | |
| P1696 | $t_{0.025, \infty} = 1.96$ | $t$ approaches $z$ | |
| P1697 | $F_{0.05, 1, \infty} = 3.84$ | $\approx \chi^2_{0.05,1}$! | |
| P1698 | $\Gamma(1/2) = \sqrt{\pi}$ | Beta, chi-sq calculations | |
| P1699 | $B(1,1) = 1$ | Beta function | |
| P1700 | $H_{10} \approx 2.93$ | Coupon collector with 10 types | |
| P1701 | $1/e \approx 0.368$ | Secretary problem optimal | |
| P1702 | $1 - 1/e \approx 0.632$ | Bootstrap: P(selecting an item) | |
| P1703 | $\pi^2/6 \approx 1.645$ | $\sum 1/n^2$ (Basel problem) | |
| P1704 | Catalan $C_5 = 42$ | Binary trees, parenthesizations | |
| P1705 | $23$ people for birthday match | 50% probability | |
| P1706 | $\sqrt{n}$ heuristic for birthday-type | Collision in $O(\sqrt{N})$ trials | |
| P1707 | $D_5 = 44$ derangements | Hat-check problem | |
| P1708 | $n! \approx \sqrt{2\pi n}(n/e)^n$ | Stirling's approximation | |
| P1709 | Entropy bits: $H(\text{fair coin}) = 1$ | Maximum for binary | |
| P1710 | Entropy nats: $H = \ln 2 \approx 0.693$ | Same in nats | |
| P1711 | BSC capacity at $p=0.1 \approx 0.531$ | Information theory | |
| P1712 | BSC capacity at $p=0.5 = 0$ | No info can be transmitted | |
| P1713 | $R^2 = r^2$ for simple regression | Square of correlation | |
| P1714 | Bonferroni: $\alpha/m$ for $m$ tests | Family-wise error control | |
| P1715 | Cohen's $d$: 0.2 small, 0.5 medium | Effect size benchmarks | |
| P1716 | $n = 30$ rule of thumb for CLT | Depends on skewness | |
| P1717 | AIC = $-2\ell + 2k$ | Information criterion | |
| P1718 | BIC = $-2\ell + k\ln n$ | Bayesian IC | |
| P1719 | LOOCV ≈ AIC asymptotically | Prediction-optimal | |
| P1720 | Adjusted $R^2 = 1 - \frac{n-1}{n-p-1}(1-R^2)$ | Penalized goodness of fit | |

---

# 📝 Practice Set 23: Decision Theory & Game Theory (P1721 – P1780)

| # | Concept | Definition | Key Result |
|---|---|---|---|
| P1721 | Risk of estimator | $R(\theta, \hat\theta) = E_\theta[L(\theta, \hat\theta)]$ | Expected loss |
| P1722 | Bayes risk | $r(\pi, \hat\theta) = \int R(\theta, \hat\theta)\pi(\theta)d\theta$ | Integrated risk |
| P1723 | Minimax estimator | $\min_{\hat\theta} \max_\theta R(\theta, \hat\theta)$ | Best worst-case |
| P1724 | Admissible | No estimator dominates it everywhere | Efficient |
| P1725 | Complete class | Set $C$ s.t. every estimator outside has a better one inside | Admissible ⊆ complete class |
| P1726 | Bayes estimator: squared loss? | Posterior mean | $E[\theta|\text{data}]$ |
| P1727 | Bayes estimator: absolute loss? | Posterior median | |
| P1728 | Bayes estimator: 0-1 loss? | Posterior mode (MAP) | |
| P1729 | James-Stein paradox | For $d \geq 3$: $\bar{X}$ is inadmissible for $\mu$! | Shrinkage estimator dominates |
| P1730 | Stein estimator | $\hat\mu_S = (1 - \frac{d-2}{\|X\|^2})X$ | Shrinks toward origin |
| P1731 | Minimax for Normal mean | $\hat\mu = X$ (sample mean) is minimax | Because constant risk |
| P1732 | Minimax for Binomial $p$ | $\hat{p} = \frac{X + \sqrt{n}/2}{n + \sqrt{n}}$ | Minimax under squared loss |
| P1733 | Neyman-Pearson lemma | LRT is most powerful | For simple hypotheses |
| P1734 | UMP test | Exists for one-sided in exp family | Via monotone LR |
| P1735 | Karlin-Rubin | Monotone LR → UMP exists | |
| P1736 | UMP for two-sided? | Usually doesn't exist | Need UMPU |

**P1737-P1780** — Game Theory & Info Theory Quick:

| # | Q | A |
|---|---|---|
| P1737 | Nash equilibrium: no player can? | Unilaterally improve by changing strategy |
| P1738 | Zero-sum game: my gain = ? | Your loss |
| P1739 | Minimax theorem (von Neumann)? | In zero-sum: $\max\min = \min\max$ (value exists) |
| P1740 | Mixed strategy? | Randomize over pure strategies |
| P1741 | Matching pennies: Nash? | Each plays H/T with prob 1/2 |
| P1742 | Prisoner's dilemma: Nash? | (Defect, Defect) — but (Cooperate, Cooperate) is better! |
| P1743 | Mechanism design? | Design rules so rational agents achieve desired outcome |
| P1744 | Auction theory: second-price? | Truthful bidding is dominant strategy |
| P1745 | Data processing inequality? | $X \to Y \to Z$: $I(X;Z) \leq I(X;Y)$ |
| P1746 | Fano's inequality? | Lower bounds error probability |
| P1747 | Rate-distortion theory? | Minimum bits for given distortion level |
| P1748 | Typical set? | $2^{nH(X)}$ sequences, prob $\approx 1$ |
| P1749 | AEP? | $-\frac{1}{n}\log P(X^n) \to H(X)$ |
| P1750 | Source coding: can compress to? | $H(X)$ bits/symbol (optimal) |
| P1751 | Huffman coding? | Optimal prefix code |
| P1752 | Arithmetic coding? | Near-optimal, approaches entropy |
| P1753 | Lempel-Ziv? | Universal compression (dictionary-based) |
| P1754 | Channel capacity of BSC($p$)? | $1 - H(p)$ bits |
| P1755 | Channel capacity of BEC($\epsilon$)? | $1 - \epsilon$ bits |
| P1756 | AWGN capacity? | $\frac{1}{2}\log(1 + SNR)$ bits |
| P1757 | Shannon limit? | Can communicate reliably at $R < C$ |
| P1758 | Turbo/LDPC codes? | Near Shannon limit |
| P1759 | Polar codes? | Achieve capacity provably |
| P1760 | Differential entropy of $N(0, \sigma^2)$? | $\frac{1}{2}\ln(2\pi e\sigma^2)$ nats |
| P1761 | Differential entropy of Exp($\lambda$)? | $1 + \ln(1/\lambda)$ nats |
| P1762 | Differential entropy of Uniform$(a,b)$? | $\ln(b-a)$ |
| P1763 | Maximum entropy: fixed mean on $[0,\infty)$? | Exponential |
| P1764 | Fixed mean & variance on $\mathbb{R}$? | Normal |
| P1765 | KL divergence: $N(\mu_1, \sigma_1^2) \| N(\mu_2, \sigma_2^2)$? | $\ln\frac{\sigma_2}{\sigma_1} + \frac{\sigma_1^2+(\mu_1-\mu_2)^2}{2\sigma_2^2} - \frac{1}{2}$ |
| P1766 | $KL(N(0,1) \| N(0,1)) = ?$ | $0$ (same distribution) |
| P1767 | Fisher info from KL? | $I(\theta) = \frac{\partial^2}{\partial\theta^2}KL(f_{\theta_0}\|f_\theta)\big|_{\theta=\theta_0}$ |
| P1768 | Stein's identity for exponential family? | Connects score, Fisher info, CRLB |
| P1769 | Cramér's large deviations? | $P(\bar{X}_n > a) \approx e^{-nI(a)}$ where $I$ = rate function |
| P1770 | Rate function = ? | Fenchel-Legendre transform of log-MGF |
| P1771 | Sanov's theorem? | $P(\hat{P}_n \in E) \approx e^{-n\inf_{Q\in E}KL(Q\|P)}$ |
| P1772 | Large deviations principle? | Fine-grained tail behavior beyond CLT |
| P1773 | Moderate deviations? | Between CLT and large deviations regimes |
| P1774 | Concentration inequality: sub-Gaussian? | $P(|X| > t) \leq 2e^{-t^2/(2\sigma^2)}$ |
| P1775 | Sub-exponential? | $P(|X| > t) \leq 2e^{-ct}$ for large $t$ |
| P1776 | Bounded RV is sub-Gaussian? | YES (Hoeffding's lemma) |
| P1777 | $\chi^2_n$ is sub-exponential? | YES |
| P1778 | Matrix concentration (Bernstein)? | Spectral norm of random matrix sum |
| P1779 | Random matrix: semicircle law? | $\frac{1}{2\pi}\sqrt{4-x^2}$ for Wigner matrices |
| P1780 | Free probability? | Non-commutative probability for random matrices |

---

# 🎯 Ultimate GATE 2027 Probability & Statistics Checklist

✅ **Core Topics (MUST master — worth 5-10 marks):**
- Bayes' theorem & total probability
- Expectation, variance, covariance properties
- All standard distributions (Binomial, Poisson, Normal, Exponential, Geometric)
- CLT and its applications
- Conditional expectation and law of iterated expectation
- Hypothesis testing (one-sample, two-sample, chi-squared)
- Confidence intervals

✅ **DA-Specific Topics (worth 3-7 additional marks for DA):**
- MLE and method of moments
- Bayesian inference (conjugate priors)
- Markov chains (stationary distributions)
- Information theory (entropy, MI, KL divergence)
- Regression (OLS, Ridge, LASSO basics)

✅ **Practice Done:**
- ~1780 problems across 23 practice sets
- Worked examples with full solutions
- True/False conceptual drills
- Error analysis & common trap identification
- Multi-step GATE simulation papers
- Distribution identification drills
- Cross-topic integration (Prob ↔ ML/Algorithms)

---

# 📊 FINAL Updated Problem Index

| Set | Problems | Topics | Count |
|---|---|---|---|
| Set 1 | P1 – P80 | Fundamentals, Bayes, Distributions | 80 |
| Set 2 | P81 – P160 | GATE Simulation | 80 |
| Set 3 | P161 – P250 | Advanced, Tricky, Statistical Concepts | 90 |
| Set 4 | P251 – P320 | Markov Chains, Stochastic Processes | 70 |
| Set 5 | P321 – P420 | Expectation, Variance, Statistical Tests | 100 |
| Set 6 | P421 – P500 | Distributions, Transforms, Info Theory | 80 |
| Set 7 | P501 – P600 | GATE Predicted, ML Connections | 100 |
| Set 8 | P601 – P750 | GATE Marathon | 150 |
| Set 9 | P751 – P830 | True/False Conceptual + Speed | 80 |
| Set 10 | P831 – P900 | Express Final + ML/Stats Connections | 70 |
| Set 11 | P901 – P950 | Worked Examples — Full Solutions | 50 |
| Set 12 | P951 – P1020 | GATE PYQ Reproductions | 70 |
| Set 13 | P1021 – P1100 | Cross-Topic Integration (Prob + ML/Algo) | 80 |
| Set 14 | P1101 – P1180 | Distribution Identification Drill | 80 |
| Set 15 | P1181 – P1300 | Ultra-Quick One-Liners | 120 |
| Set 16 | P1301 – P1340 | Final Exam Simulation | 40 |
| Set 17 | P1341 – P1400 | Error Analysis & Debugging | 60 |
| Set 18 | P1401 – P1500 | Tricky GATE Traps & Edge Cases | 100 |
| Set 19 | P1501 – P1560 | Multi-Step GATE Simulation | 60 |
| Set 20 | P1561 – P1640 | Extreme Value & Tail Probability | 80 |
| Set 21 | P1601 – P1680 | Bayesian Deep Dive | 80 |
| Set 22 | P1681 – P1720 | Numerical Constants | 40 |
| Set 23 | P1721 – P1780 | Decision Theory & Game Theory | 60 |
| **GRAND TOTAL** | | | **~1780 problems** |

---

# 📝 Practice Set 24: Simulation & Computational Probability (P1781 – P1860)

> 🖥️ Understanding these concepts is essential for GATE DA — connects probability to programming.

**P1781.** (2-mark) Monte Carlo estimation of $\pi$: throw $n$ points uniformly in $[0,1]^2$. Estimate $\pi$?
→ $\hat\pi = 4 \times \frac{\text{points inside quarter-circle}}{n}$
→ Variance: $\text{Var}(\hat\pi) \approx \frac{\pi(4-\pi)}{n} \approx \frac{2.69}{n}$

**P1782.** (2-mark) MC integration: estimate $\int_0^1 e^{-x^2} dx$.
→ $\hat{I} = \frac{1}{n}\sum_{i=1}^n e^{-U_i^2}$ where $U_i \sim U(0,1)$
→ True value $\approx 0.7468$

**P1783.** (2-mark) Variance reduction: antithetic variates for above?
→ Use pairs $(U_i, 1-U_i)$: $\hat{I} = \frac{1}{n}\sum_{i=1}^{n/2}[e^{-U_i^2} + e^{-(1-U_i)^2}]/2$
→ Reduces variance because $e^{-x^2}$ is monotone on $[0,1]$

**P1784.** (2-mark) Control variate: know $E[X] = \mu_X$. Estimate $E[Y]$?
→ $\hat{Y}_{CV} = \bar{Y} - c(\bar{X} - \mu_X)$ where $c^* = \text{Cov}(X,Y)/\text{Var}(X)$
→ Variance reduction: $\text{Var} = (1-\rho^2)\text{Var}(\bar{Y})$

**P1785.** (2-mark) Stratified sampling: divide $[0,1]$ into $k$ strata. Why better?
→ Within-strata variance < overall variance
→ Optimal allocation: sample proportional to $n_j \propto N_j\sigma_j$

**P1786.** (2-mark) Importance sampling can have INFINITE variance. When?
→ When proposal $g$ has thinner tails than $f \times h$
→ Solution: use proposals with heavier tails

**P1787-P1820** — Simulation Theory Quick:

| # | Topic | Q | A |
|---|---|---|---|
| P1787 | LCG | $X_{n+1} = (aX_n + c) \mod m$? | Linear congruential generator |
| P1788 | Period of LCG? | At most $m$ (choose $m$ large prime) |
| P1789 | Mersenne Twister | Period? | $2^{19937} - 1$ (very long!) |
| P1790 | Inverse CDF method works for? | Any continuous distribution with known CDF inverse |
| P1791 | Box-Muller: inputs? | Two $U(0,1)$ → two $N(0,1)$ |
| P1792 | Generating Poisson($\lambda$) via? | Count Exp arrivals until exceeding 1 |
| P1793 | Generating Gamma($\alpha, 1$) for $\alpha \geq 1$? | Marsaglia's method or acceptance-rejection |
| P1794 | Generating Beta($a,b$)? | $X/(X+Y)$ where $X \sim \text{Gamma}(a)$, $Y \sim \text{Gamma}(b)$ |
| P1795 | Generating Binomial($n,p$)? | Sum of $n$ Bernoulli($p$); or inverse CDF for large $n$ |
| P1796 | Generating multivariate normal? | Cholesky: $X = \mu + Lz$ where $\Sigma = LL^T$, $z$ i.i.d. $N(0,1)$ |
| P1797 | Generating Wishart? | $W = X^TX$ where rows of $X$ are i.i.d. $N(0,\Sigma)$ |
| P1798 | Permutation test: reshuffle labels? | Under $H_0$, labels exchangeable |
| P1799 | Bootstrap: resample $n$ with replacement? | Estimate sampling distribution |
| P1800 | Parametric bootstrap? | Resample from fitted model |
| P1801 | Wild bootstrap? | For heteroscedastic regression |
| P1802 | Block bootstrap? | For dependent (time series) data |
| P1803 | Jackknife bias? | $\hat{B} = (n-1)(\bar{\hat\theta}_{(\cdot)} - \hat\theta)$ |
| P1804 | Jackknife variance? | $\hat{V} = \frac{n-1}{n}\sum(\hat\theta_{(i)} - \bar{\hat\theta}_{(\cdot)})^2$ |
| P1805 | Bootstrap CI: percentile method? | $(\hat\theta^*_{\alpha/2}, \hat\theta^*_{1-\alpha/2})$ |
| P1806 | BCa bootstrap CI? | Bias-corrected and accelerated (more accurate) |
| P1807 | Double bootstrap? | Bootstrap the bootstrap for calibration |
| P1808 | Cross-validation: LOOCV for linear regression? | $CV = \frac{1}{n}\sum\left(\frac{y_i - \hat{y}_i}{1 - h_{ii}}\right)^2$ — no refitting! |
| P1809 | Randomization test vs permutation test? | Same idea; randomization is design-based |
| P1810 | Exact test: Fisher's for $2 \times 2$? | Hypergeometric under $H_0$ |
| P1811 | Power simulation? | Simulate data under $H_1$, check rejection rate |
| P1812 | Sample size determination by simulation? | Iterate: increase $n$ until power $\geq 0.8$ |
| P1813 | Markov chain convergence diagnostic: $\hat{R}$? | Run multiple chains; $\hat{R} \approx 1$ = converged |
| P1814 | Autocorrelation time: $\tau_{int} = 1 + 2\sum \rho_k$? | Effective samples $= n/\tau_{int}$ |
| P1815 | Gibbs sampling: full conditional for normal model? | Each coordinate from conditional normal |
| P1816 | Slice sampling? | Auxiliary variable method; robust to tuning |
| P1817 | Parallel tempering? | Run chains at different temperatures; swap |
| P1818 | Simulated annealing? | MCMC with decreasing temperature → optimization |
| P1819 | Approximate Bayesian Computation (ABC)? | Likelihood-free inference; simulate & compare |
| P1820 | Synthetic likelihood? | Gaussian approximation to ABC |

**P1821-P1860** — Mixed Computational:

| # | Q | A |
|---|---|---|
| P1821 | $n$ for MC estimate of $\pi$ with $\epsilon = 0.01$, 95% confidence? | $n = (1.96)^2 \times p(1-p)/\epsilon^2 \approx 10000$ |
| P1822 | MC standard error formula? | $SE = \hat\sigma/\sqrt{n}$ |
| P1823 | How many bootstrap samples? | $B = 1000$ typically; $10000$ for CIs |
| P1824 | Permutation test: how many permutations? | All $\binom{n}{k}$ or subsample ($\geq 10000$) |
| P1825 | Rejection sampling: acceptance rate? | $1/M$ where $M = \sup f/g$ |
| P1826 | Better proposal → higher acceptance rate? | YES — $M$ closer to 1 |
| P1827 | MCMC: proposal variance too large? | Low acceptance, chain jumps erratically |
| P1828 | MCMC: proposal variance too small? | High acceptance, but slow exploration (random walk) |
| P1829 | Optimal MCMC acceptance: random walk Metropolis? | $\approx 23.4\%$ for $d$-dim normal target |
| P1830 | Optimal for MALA? | $\approx 57.4\%$ |
| P1831 | HMC: leapfrog steps $L$? | Too few: random walk. Too many: U-turns |
| P1832 | NUTS: No U-Turn Sampler? | Automatically selects optimal $L$ |
| P1833 | Effective sample size (ESS)? | $n_{eff} = n \times \frac{1}{1 + 2\sum_{k=1}^\infty \rho_k}$ |
| P1834 | When is ESS ratio $= 1$? | When samples are independent (no autocorrelation) |
| P1835 | $\hat{R} > 1.1$: what to do? | Run chains longer, check initialization |
| P1836 | Trace plot shows trend: problem? | Chain hasn't converged; extend burn-in |
| P1837 | Bimodal target: challenge for MCMC? | May get stuck in one mode; use tempering |
| P1838 | Rao-Blackwellization in MCMC? | Replace MC estimate with conditional expectation → reduces variance |
| P1839 | Quasi-Monte Carlo: advantage? | Faster convergence: $O(1/n)$ vs $O(1/\sqrt{n})$ |
| P1840 | Sobol sequences? | Low-discrepancy sequence for QMC |
| P1841 | Halton sequence? | Another low-discrepancy sequence; simpler |
| P1842 | Randomized QMC? | Random shift of deterministic QMC → unbiased CI |
| P1843 | Multilevel MC? | Use coarse + fine simulations; reduces cost |
| P1844 | Particle filter? | Sequential MC for state-space models |
| P1845 | Sequential importance sampling? | Weights: $w_t = p(y_t|x_t)$ |
| P1846 | Resampling in particle filter? | Prevent weight degeneracy |
| P1847 | Conditional particle filter? | Ancestor sampling for PMCMC |
| P1848 | SMC sampler? | Generalized particle filter for static models |
| P1849 | Normalizing constant estimation via SMC? | Unbiased! (unlike MCMC) |
| P1850 | Neural network as proposal in MCMC? | Normalizing flows, neural importance sampling |
| P1851 | Amortized inference? | Learn proposal mapping: $x \to q(\theta|x)$ |
| P1852 | Variational autoencoder loss? | $-\text{ELBO} = -E_q[\log p(x|z)] + KL(q(z|x)\|p(z))$ |
| P1853 | Reparameterization trick purpose? | Low-variance gradient through stochastic layer |
| P1854 | Score function estimator (REINFORCE)? | $\nabla E_q[f] = E_q[f \nabla\log q]$ — high variance |
| P1855 | Baseline in REINFORCE? | Subtract $b$: $\nabla E_q[f] = E_q[(f-b)\nabla\log q]$ — reduces variance |
| P1856 | Stein variational gradient descent? | Deterministic particles approximate posterior |
| P1857 | Kernel Stein discrepancy? | Measure quality of approximate posterior |
| P1858 | Optimal transport? | Wasserstein distance between distributions |
| P1859 | Sinkhorn algorithm? | Efficient computation of regularized OT |
| P1860 | Sliced Wasserstein? | Project to 1D, compute Wasserstein there |

---

# 📝 Practice Set 25: The Last 100 Express Problems (P1861 – P1960)

| # | Q | A |
|---|---|---|
| P1861 | $P(\text{straight in poker})$? | $10 \times 4^5/\binom{52}{5} = 10240/2598960 \approx 0.00394$ (includes straight flush) |
| P1862 | Expected value of geometric series $\sum_{k=0}^\infty r^k$? | $1/(1-r)$ for $|r| < 1$ |
| P1863 | $\sum_{k=0}^\infty kr^k = ?$ | $r/(1-r)^2$ |
| P1864 | $\sum_{k=0}^\infty k^2 r^k = ?$ | $r(1+r)/(1-r)^3$ |
| P1865 | Indicator trick: $E[X] = \sum P(X \geq k)$? | For non-negative integer RV |
| P1866 | Survival function: $E[X] = \int_0^\infty \bar{F}(x)dx$? | For non-negative continuous RV |
| P1867 | LOTUS: $E[g(X)]$? | $\int g(x)f_X(x)dx$ (no need for distribution of $g(X)$!) |
| P1868 | Tower property: $E[E[Y|X]] = ?$ | $E[Y]$ |
| P1869 | Smoothing: $E[Y] = E_X[E[Y|X]]$? | Another name for tower property |
| P1870 | $E[\text{Var}(Y|X)]$: within-group variance? | Part of law of total variance |
| P1871 | $\text{Var}(E[Y|X])$: between-group variance? | Other part |
| P1872 | Both sum to? | $\text{Var}(Y)$ — EVVE law |
| P1873 | Conditional PMF: $P(X=x|Y=y) = ?$ | $P(X=x,Y=y)/P(Y=y)$ |
| P1874 | Conditional PDF: $f_{X|Y}(x|y) = ?$ | $f_{X,Y}(x,y)/f_Y(y)$ |
| P1875 | Marginal from joint: $f_X(x) = ?$ | $\int f_{X,Y}(x,y)dy$ |
| P1876 | Change of variables (1D): $Y = g(X)$, $f_Y(y) = ?$ | $f_X(g^{-1}(y))|dg^{-1}/dy|$ |
| P1877 | Jacobian for 2D transform? | $|J| = |\partial(x,y)/\partial(u,v)|$ |
| P1878 | Convolution formula: $Z = X+Y$? | $f_Z(z) = \int f_X(x)f_Y(z-x)dx$ |
| P1879 | Product: $Z = XY$ for positive RVs? | $f_Z(z) = \int f_X(x)f_Y(z/x)(1/|x|)dx$ |
| P1880 | Ratio: $Z = X/Y$ for positive RVs? | $f_Z(z) = \int |y|f_X(zy)f_Y(y)dy$ |
| P1881 | Order statistics: $f_{X_{(k)}}(x) = ?$ | $\frac{n!}{(k-1)!(n-k)!}[F(x)]^{k-1}[1-F(x)]^{n-k}f(x)$ |
| P1882 | $X_{(1)}$ for $n$ i.i.d. Exp($\lambda$)? | Exp($n\lambda$) |
| P1883 | Spacing $D_k = X_{(k+1)} - X_{(k)}$ for Exp? | $(n-k)D_k \sim \text{Exp}(\lambda)$ — Rényi representation |
| P1884 | Sample range $R = X_{(n)} - X_{(1)}$? | Distribution depends on parent; for Uniform$(0,1)$: $\text{Beta}(n-1, 1)$ |
| P1885 | Probability integral transform: $F(X) \sim ?$ | $U(0,1)$ for continuous $X$! |
| P1886 | QQ-plot principle? | Compare quantiles of data vs theoretical |
| P1887 | PP-plot principle? | Compare CDFs |
| P1888 | Kernel density estimation: $\hat{f}(x) = ?$ | $\frac{1}{nh}\sum K\left(\frac{x-X_i}{h}\right)$ |
| P1889 | Optimal bandwidth $h$ for KDE? | $h \propto n^{-1/5}$ (Silverman's rule) |
| P1890 | Bias-variance of KDE? | Large $h$: bias ↑, variance ↓. Small $h$: bias ↓, variance ↑ |
| P1891 | Empirical CDF: $\hat{F}_n(x) = ?$ | $\frac{1}{n}\sum I(X_i \leq x)$ |
| P1892 | DKW inequality: $P(\sup|\hat{F}_n - F| > \epsilon) \leq ?$ | $2e^{-2n\epsilon^2}$ |
| P1893 | This implies confidence band for CDF? | Width $\epsilon = \sqrt{\ln(2/\alpha)/(2n)}$ |
| P1894 | KS test: $D_n = ?$ | $\sup_x|\hat{F}_n(x) - F_0(x)|$ |
| P1895 | KS limitation? | Less sensitive at tails than Anderson-Darling |
| P1896 | Cramér-von Mises: $W^2 = ?$ | $\int[\hat{F}_n(x) - F_0(x)]^2 dF_0(x)$ |
| P1897 | Lilliefors test? | KS test when parameters estimated from data |
| P1898 | Shapiro-Wilk $W$ for normality? | Best for small samples ($n \leq 50$) |
| P1899 | D'Agostino-Pearson? | Combines skewness and kurtosis tests |
| P1900 | Jarque-Bera test? | $JB = \frac{n}{6}(S^2 + K^2/4) \sim \chi^2_2$ |
| P1901 | Runs test for randomness? | Count runs up/down; compare to expected |
| P1902 | Wald-Wolfowitz? | Runs test for two-sample |
| P1903 | McNemar's test? | Paired binary data; $\chi^2$ on discordant pairs |
| P1904 | Cochran's Q test? | Extension of McNemar to $k$ treatments |
| P1905 | Friedman test? | Nonparametric two-way ANOVA |
| P1906 | Kruskal-Wallis? | Nonparametric one-way ANOVA (ranks) |
| P1907 | Dunn's test? | Post-hoc for Kruskal-Wallis |
| P1908 | Tukey's HSD? | Post-hoc for one-way ANOVA |
| P1909 | Scheffé's method? | Most conservative post-hoc; for all contrasts |
| P1910 | Dunnett's test? | Compare each treatment to control only |
| P1911 | Levene's test? | Equality of variances (robust to non-normality) |
| P1912 | Bartlett's test? | Equality of variances (assumes normality) |
| P1913 | Brown-Forsythe? | Like Levene but uses median instead of mean |
| P1914 | Welch's ANOVA? | ANOVA without equal variance assumption |
| P1915 | Two-way ANOVA: interaction? | Effect of A depends on level of B |
| P1916 | Mixed effects model? | Fixed + random effects |
| P1917 | Random effect: variance component? | $Y_{ij} = \mu + \alpha_i + \epsilon_{ij}$, $\alpha_i \sim N(0, \sigma_\alpha^2)$ |
| P1918 | ICC (Intraclass Correlation)? | $\sigma_\alpha^2/(\sigma_\alpha^2 + \sigma_\epsilon^2)$ |
| P1919 | Repeated measures ANOVA? | Same subjects measured multiple times |
| P1920 | Greenhouse-Geisser correction? | For violated sphericity assumption |
| P1921 | MANOVA: multivariate ANOVA? | Tests multiple DVs simultaneously |
| P1922 | Wilks' $\Lambda$ in MANOVA? | $|E|/|E+H|$ where $E$ = error, $H$ = hypothesis |
| P1923 | Discriminant analysis: Fisher's? | Find linear combination maximizing group separation |
| P1924 | Canonical correlation: CCA? | Max correlation between linear combos of two sets |
| P1925 | PLS (Partial Least Squares)? | Like CCA but for regression; handles multicollinearity |
| P1926 | Factor analysis: common vs unique? | Common factors + unique factors |
| P1927 | Eigenvalue $> 1$ rule (Kaiser)? | Retain factors with eigenvalue > 1 |
| P1928 | Scree plot: look for? | Elbow (bend) in eigenvalue plot |
| P1929 | Varimax rotation? | Orthogonal rotation for interpretable factors |
| P1930 | Oblimin rotation? | Oblique (correlated factors allowed) |
| P1931 | Cronbach's $\alpha$? | Internal consistency reliability |
| P1932 | $\alpha > 0.7$: acceptable? | Yes (rule of thumb) |
| P1933 | Test-retest reliability? | Correlation of two administrations |
| P1934 | Cohen's $\kappa$? | Inter-rater agreement (corrects for chance) |
| P1935 | $\kappa > 0.8$: almost perfect? | Yes (Landis & Koch) |
| P1936 | Bland-Altman plot? | Method comparison (difference vs mean) |
| P1937 | ROC curve: plots? | TPR vs FPR at varying thresholds |
| P1938 | AUC interpretation? | P(random positive ranks higher than random negative) |
| P1939 | AUC = 0.5: random? | Yes (no discrimination) |
| P1940 | AUC = 1.0: perfect? | Yes |
| P1941 | Precision-recall curve? | Better for imbalanced data |
| P1942 | F1 score? | $2 \times \text{Precision} \times \text{Recall}/(\text{Precision} + \text{Recall})$ (harmonic mean) |
| P1943 | Matthews correlation coefficient? | Good for imbalanced; $[-1, 1]$ |
| P1944 | Log-loss? | $-\frac{1}{n}\sum[y_i\log p_i + (1-y_i)\log(1-p_i)]$ |
| P1945 | Brier score? | $\frac{1}{n}\sum(y_i - p_i)^2$ — MSE of probabilities |
| P1946 | Calibration: reliability diagram? | Plot observed freq vs predicted prob |
| P1947 | Hosmer-Lemeshow test? | GOF for logistic regression (group by deciles) |
| P1948 | Nagelkerke $R^2$? | Pseudo-$R^2$ for logistic regression |
| P1949 | McFadden's $R^2$? | $1 - \ell(\hat\beta)/\ell(\hat\beta_0)$ |
| P1950 | Deviance in GLM? | $D = 2[\ell(\text{saturated}) - \ell(\text{fitted})]$ |
| P1951 | Overdispersion in GLM? | Deviance/df $\gg 1$; use quasi-likelihood |
| P1952 | Link function for Binomial in GLM? | Logit (canonical), probit, cloglog |
| P1953 | Complementary log-log? | $\log(-\log(1-p)) = X\beta$; for extreme probabilities |
| P1954 | Offset in Poisson regression? | $\log(\text{exposure})$ as known offset |
| P1955 | Exposure vs rate? | Rate = count/exposure; model rate via offset |
| P1956 | Negative binomial regression: overdispersed? | Adds dispersion parameter to Poisson |
| P1957 | Hurdle model? | Separate model for zero vs positive counts |
| P1958 | Tobit model? | Censored regression (truncated below) |
| P1959 | Truncated regression? | Only observe values above threshold |
| P1960 | Selection model (Heckman)? | Two-stage: selection equation + outcome equation |

---

# 🏆 GRAND FINALE: The 10 Commandments of GATE Probability

1. **Thou shalt draw a diagram** before computing any probability
2. **Thou shalt check independence** before multiplying probabilities
3. **Thou shalt use the complement** ($P(A^c) = 1 - P(A)$) whenever "at least one" appears
4. **Thou shalt identify the distribution** before plugging into formulas
5. **Thou shalt verify parameterization** (rate vs scale, success vs failure count)
6. **Thou shalt apply CLT only when $n$ is large enough** and variance is finite
7. **Thou shalt not confuse $P(A|B)$ with $P(B|A)$** (Bayes' theorem exists for a reason!)
8. **Thou shalt check if the estimator is biased** before calling it "best"
9. **Thou shalt use the correct degrees of freedom** for every test
10. **Thou shalt verify boundary cases** ($n=1$, $p=0$, $p=1$, $\lambda=0$) to sanity-check answers

---

# 📊 ULTIMATE Final Problem Index

| Set | Problems | Topics | Count |
|---|---|---|---|
| Set 1 | P1 – P80 | Fundamentals, Bayes, Distributions | 80 |
| Set 2 | P81 – P160 | GATE Simulation | 80 |
| Set 3 | P161 – P250 | Advanced, Tricky, Statistical Concepts | 90 |
| Set 4 | P251 – P320 | Markov Chains, Stochastic Processes | 70 |
| Set 5 | P321 – P420 | Expectation, Variance, Statistical Tests | 100 |
| Set 6 | P421 – P500 | Distributions, Transforms, Info Theory | 80 |
| Set 7 | P501 – P600 | GATE Predicted, ML Connections | 100 |
| Set 8 | P601 – P750 | GATE Marathon | 150 |
| Set 9 | P751 – P830 | True/False Conceptual + Speed | 80 |
| Set 10 | P831 – P900 | Express Final + ML/Stats Connections | 70 |
| Set 11 | P901 – P950 | Worked Examples — Full Solutions | 50 |
| Set 12 | P951 – P1020 | GATE PYQ Reproductions | 70 |
| Set 13 | P1021 – P1100 | Cross-Topic Integration | 80 |
| Set 14 | P1101 – P1180 | Distribution Identification Drill | 80 |
| Set 15 | P1181 – P1300 | Ultra-Quick One-Liners | 120 |
| Set 16 | P1301 – P1340 | Final Exam Simulation | 40 |
| Set 17 | P1341 – P1400 | Error Analysis & Debugging | 60 |
| Set 18 | P1401 – P1500 | Tricky GATE Traps & Edge Cases | 100 |
| Set 19 | P1501 – P1560 | Multi-Step Simulation | 60 |
| Set 20 | P1561 – P1640 | Extreme Value & Tail Bounds | 80 |
| Set 21 | P1601 – P1680 | Bayesian Deep Dive | 80 |
| Set 22 | P1681 – P1720 | Numerical Constants | 40 |
| Set 23 | P1721 – P1780 | Decision Theory & Info Theory | 60 |
| Set 24 | P1781 – P1860 | Simulation & Computational | 80 |
| Set 25 | P1861 – P1960 | The Last 100 Express | 100 |
| **GRAND TOTAL** | | | **~1960 problems** |

---

# 📝 Practice Set 26: Bonus — Advanced Statistical Tests & Methods (P1961 – P2060)

> 🎓 The ultimate bonus round for GATE DA aspirants who want to go beyond!

| # | Test/Method | Purpose | When to Use |
|---|---|---|---|
| P1961 | One-sample $z$-test | Test mean ($\sigma$ known) | Large $n$, $\sigma$ known |
| P1962 | One-sample $t$-test | Test mean ($\sigma$ unknown) | Small $n$, normal population |
| P1963 | Paired $t$-test | Test mean difference (paired data) | Before/after, matched pairs |
| P1964 | Independent $t$-test (equal var) | Compare 2 means | $\sigma_1 = \sigma_2$ assumed |
| P1965 | Welch's $t$-test | Compare 2 means | $\sigma_1 \neq \sigma_2$ |
| P1966 | One-way ANOVA ($F$-test) | Compare $k > 2$ means | Equal variances, normal |
| P1967 | Welch's ANOVA | Compare $k$ means | Unequal variances |
| P1968 | Two-way ANOVA | Two factors + interaction | Balanced/unbalanced designs |
| P1969 | Repeated measures ANOVA | Within-subject factor | Same subjects measured $k$ times |
| P1970 | ANCOVA | ANOVA with covariate | Adjust for confounding continuous var |
| P1971 | MANOVA | Multiple DVs | Test vector of means simultaneously |
| P1972 | $\chi^2$ GOF | Does data fit distribution? | Categorical data, expected ≥ 5 |
| P1973 | $\chi^2$ independence | Association in $r \times c$ table? | Expected counts ≥ 5 |
| P1974 | $\chi^2$ homogeneity | Same proportions across groups? | Multiple populations |
| P1975 | Fisher exact test | $2 \times 2$ table, small counts | Any sample size |
| P1976 | McNemar test | Paired binary data change | Before/after binary outcomes |
| P1977 | Cochran-Mantel-Haenszel | Stratified $2 \times 2$ tables | Control for confounder |
| P1978 | Breslow-Day test | Homogeneity of OR across strata | Before pooling strata |
| P1979 | Wilcoxon signed-rank | Paired nonparametric | Symmetric differences |
| P1980 | Mann-Whitney $U$ | Two-sample nonparametric | Ordinal or non-normal |
| P1981 | Kruskal-Wallis | $k$-sample nonparametric ANOVA | Ordinal or non-normal |
| P1982 | Friedman test | Repeated measures nonparametric | Blocked/paired ordinal |
| P1983 | Spearman rank correlation | Monotone association | Ordinal data |
| P1984 | Kendall's $\tau$ | Concordance/discordance | Ordinal, robust |
| P1985 | Runs test | Randomness | Binary sequence |
| P1986 | Sign test | Median | Paired, no symmetry needed |
| P1987 | Binomial test | Test proportion | Exact, small $n$ |
| P1988 | Proportion $z$-test | Test proportion | Large $n$ |
| P1989 | Two-proportion $z$-test | Compare proportions | Large $n_1$, $n_2$ |
| P1990 | Likelihood ratio test | General nested models | Any parametric model |
| P1991 | Wald test | Test single parameter | Asymptotic normal |
| P1992 | Score (Rao) test | Test at null | Only need fit under $H_0$! |
| P1993 | Deviance test (GLM) | Model fit | Nested GLMs |
| P1994 | Hosmer-Lemeshow | Logistic regression GOF | Grouped calibration |
| P1995 | Kolmogorov-Smirnov | Distribution fit | Continuous data |
| P1996 | Anderson-Darling | Distribution fit (tail-sensitive) | Better than KS for tails |
| P1997 | Shapiro-Wilk | Normality | Best for small $n$ |
| P1998 | Jarque-Bera | Normality (skewness + kurtosis) | Large $n$ |
| P1999 | Lilliefors | Normality (estimated params) | Modified KS |
| P2000 | Durbin-Watson | Autocorrelation in regression | Time series residuals |

**P2001-P2060** — Statistical Methods Matching:

| # | Scenario | Best Method |
|---|---|---|
| P2001 | Compare mean blood pressure before & after treatment (same patients) | Paired $t$-test |
| P2002 | Same, but data is clearly non-normal | Wilcoxon signed-rank |
| P2003 | Compare 3 drug dosages on blood pressure | One-way ANOVA |
| P2004 | Same, but non-normal data | Kruskal-Wallis |
| P2005 | Is gender associated with voting preference? | $\chi^2$ independence |
| P2006 | 2×2 table with cells having count < 5 | Fisher exact test |
| P2007 | Does a die have fair probabilities? | $\chi^2$ GOF |
| P2008 | Is residual autocorrelated in regression? | Durbin-Watson test |
| P2009 | Is data normally distributed ($n = 25$)? | Shapiro-Wilk |
| P2010 | Are variances equal across 3 groups? | Levene's test |
| P2011 | Compare survival curves of 2 treatments | Log-rank test |
| P2012 | Regression with censored outcomes | Cox proportional hazards |
| P2013 | Binary outcome, multiple predictors | Logistic regression |
| P2014 | Count outcome, exposure known | Poisson regression with offset |
| P2015 | Overdispersed counts | Negative binomial regression |
| P2016 | Many zero counts | Zero-inflated Poisson/NB |
| P2017 | Ordinal outcome | Ordinal logistic regression |
| P2018 | Multinomial outcome | Multinomial logistic regression |
| P2019 | Clustered (grouped) data | Mixed effects / multilevel model |
| P2020 | Repeated measures, missing data | Mixed effects model (handles missing) |
| P2021 | High-dimensional ($p > n$) regression | LASSO / Elastic Net |
| P2022 | Feature selection importance | Random forest variable importance |
| P2023 | Nonlinear regression with interactions | Gradient boosting (XGBoost) |
| P2024 | Interpretable nonlinear | GAM (Generalized Additive Model) |
| P2025 | Spatial correlation | Kriging / spatial mixed effects |
| P2026 | Time series forecasting (univariate) | ARIMA / exponential smoothing |
| P2027 | Time series with exogenous variables | ARIMAX / VAR |
| P2028 | Detecting changepoints | CUSUM / Binary segmentation |
| P2029 | Testing for unit root | ADF / KPSS test |
| P2030 | Cointegration between 2 series | Engle-Granger / Johansen test |
| P2031 | Causal effect of treatment | Randomized experiment → $t$-test/ANOVA |
| P2032 | Observational study causal inference | Propensity score matching |
| P2033 | Instrumental variable estimation | Two-stage least squares (2SLS) |
| P2034 | Policy change effect | Difference-in-differences |
| P2035 | Sharp cutoff in treatment | Regression discontinuity |
| P2036 | Mediation analysis | Sobel test / Baron-Kenny |
| P2037 | Moderation analysis | Interaction term in regression |
| P2038 | Path analysis | SEM (Structural Equation Model) |
| P2039 | Latent variables with indicators | Confirmatory Factor Analysis |
| P2040 | Discover hidden structure | Exploratory Factor Analysis |
| P2041 | Cluster number selection | Silhouette / Gap statistic / Elbow |
| P2042 | Dense, convex clusters | K-means |
| P2043 | Arbitrary shape clusters | DBSCAN |
| P2044 | Hierarchical cluster structure | Agglomerative clustering + dendrogram |
| P2045 | Soft clustering (probabilities) | Gaussian Mixture Model (EM) |
| P2046 | Dimensionality reduction for visualization | t-SNE / UMAP |
| P2047 | Linear dimensionality reduction | PCA |
| P2048 | Sparse PCA | $\ell_1$ penalty on loadings |
| P2049 | Robust PCA (outliers) | RPCA / MAD-based |
| P2050 | Anomaly detection (univariate) | Grubbs / Dixon / MAD |
| P2051 | Anomaly detection (multivariate) | Mahalanobis distance / Isolation Forest |
| P2052 | Network community detection | Modularity optimization / Louvain |
| P2053 | A/B testing minimum sample size | Power analysis: $n = (z_\alpha+z_\beta)^2 \times 2\sigma^2/\delta^2$ |
| P2054 | Sequential A/B testing | Group sequential design / always-valid p-values |
| P2055 | Bayesian A/B testing | Thompson sampling / posterior P(B > A) |
| P2056 | Multi-armed bandit | UCB / Thompson sampling |
| P2057 | Contextual bandit | LinUCB / neural contextual |
| P2058 | Reinforcement learning connection | MDP → policy optimization |
| P2059 | Meta-analysis | Random effects model (DerSimonian-Laird) |
| P2060 | Publication bias detection | Funnel plot / Egger's test |

---

# 🧮 Essential Approximations for Quick GATE Computation

| Expression | Approximation | When to Use |
|---|---|---|
| $e^x \approx 1 + x$ | Small $|x|$ | Prob simplifications |
| $\ln(1+x) \approx x$ | Small $|x|$ | Log-probability |
| $(1+x)^n \approx e^{nx}$ | Large $n$, small $x$ | Compound interest type |
| $(1-1/n)^n \approx 1/e$ | Large $n$ | Birthday, occupancy |
| $\binom{n}{k} \approx \frac{n^k}{k!}$ | $k \ll n$ | Poisson approximation |
| $n! \approx \sqrt{2\pi n}(n/e)^n$ | Stirling | Large factorials |
| $H_n = \sum_{k=1}^n 1/k \approx \ln n + 0.577$ | Euler-Mascheroni | Coupon collector |
| $\binom{2n}{n} \approx 4^n/\sqrt{\pi n}$ | Central binomial | Catalan, walks |
| $\Phi(x) \approx 1 - \phi(x)/x$ | Mill's ratio | Large $x$ tail |
| $1 - \Phi(x) \approx \phi(x)/x$ for $x > 2$ | Tail approximation | Quick tail probs |

---

# 🎯 7-Day Final Revision Schedule

| Day | Focus Area | Problems to Redo | Time |
|---|---|---|---|
| Day 1 | Basics: Counting, Probability Rules, Bayes | Sets 1, 12 | 3 hrs |
| Day 2 | Distributions: All standard discrete + continuous | Sets 2, 14, 15 | 4 hrs |
| Day 3 | Expectation, Variance, Joint, Conditional | Sets 5, 11 | 3 hrs |
| Day 4 | CLT, Hypothesis Testing, CI | Sets 7, 16 | 3 hrs |
| Day 5 | Markov Chains, MLE, Bayesian | Sets 4, 21 | 3 hrs |
| Day 6 | Traps Review + Error Analysis + Speed Drill | Sets 9, 17, 18 | 4 hrs |
| Day 7 | Full Mock: Set 16 (timed) + Formula Review | Set 16 + Tables | 3 hrs |

---

# 📝 Practice Set 27: The Final 100 — Lightning Round (P2061 – P2160)

| # | Q | A |
|---|---|---|
| P2061 | $P(A) = 0$. Is $A$ impossible? | Not necessarily (continuous probability) |
| P2062 | $P(A) = 1$. Is $A$ certain? | Almost surely — may exclude measure-zero events |
| P2063 | $\sigma$-algebra: closed under? | Complement, countable union |
| P2064 | Borel $\sigma$-algebra on $\mathbb{R}$? | Smallest $\sigma$-algebra containing open sets |
| P2065 | Measurable function? | Pre-image of Borel set is measurable |
| P2066 | Lebesgue integral vs Riemann? | Lebesgue: partition range, not domain |
| P2067 | Dominated convergence theorem? | $|f_n| \leq g$, $g$ integrable → $\int\lim = \lim\int$ |
| P2068 | Monotone convergence? | $f_n \uparrow f$ → $\int f_n \uparrow \int f$ |
| P2069 | Fatou's lemma? | $\int\liminf f_n \leq \liminf\int f_n$ |
| P2070 | Radon-Nikodym derivative? | $dP/dQ$ — density of one measure w.r.t. another |
| P2071 | Why is Normal distribution special? | CLT, max entropy, stable, reproductive, tractable |
| P2072 | Why is Exponential special? | Memoryless, Poisson connection, max entropy on $(0,\infty)$ |
| P2073 | Why is Poisson special? | Rare events, mean=variance, reproductive |
| P2074 | Why is Beta special? | Conjugate for Bernoulli, flexible on (0,1) |
| P2075 | Why is Gamma special? | Conjugate for Poisson/Exp, sum of Exp |
| P2076 | $\text{Bin}(n,p) \to \text{Poisson}(\lambda)$ when? | $n \to \infty$, $p \to 0$, $np = \lambda$ |
| P2077 | $\text{Bin}(n,p) \to N(np, np(1-p))$ when? | $n$ large, $np > 5$, $n(1-p) > 5$ |
| P2078 | $\text{Poisson}(\lambda) \to N(\lambda, \lambda)$ when? | $\lambda > 20$ approximately |
| P2079 | $\text{Gamma}(n, \lambda) \to N(n/\lambda, n/\lambda^2)$ when? | $n$ large (by CLT on sum of Exp) |
| P2080 | $t_n \to N(0,1)$ when? | $n \to \infty$ |
| P2081 | $\chi^2_n/n \to 1$ when? | $n \to \infty$ (WLLN) |
| P2082 | $F_{m,n} \to \chi^2_m/m$ when? | $n \to \infty$ |
| P2083 | Hypergeometric → Binomial when? | $N \to \infty$, $K/N \to p$ |
| P2084 | Beta $(n\alpha, n\beta)$ concentrates at $\alpha/(\alpha+\beta)$ | As $n \to \infty$ (Bayesian certainty) |
| P2085 | Dirichlet concentrates at mode | With large concentration parameters |
| P2086 | Laplace distribution: PDF? | $\frac{1}{2b}e^{-|x-\mu|/b}$ — double exponential |
| P2087 | Laplace vs Normal: heavier tails? | Laplace (exponential tails vs Gaussian) |
| P2088 | LASSO prior = Laplace | Connection to $\ell_1$ penalty |
| P2089 | Ridge prior = Normal | Connection to $\ell_2$ penalty |
| P2090 | Elastic Net prior? | Mixture of Normal + Laplace |
| P2091 | Spike-and-slab prior? | Point mass at 0 + continuous (true sparsity) |
| P2092 | Horseshoe prior? | Half-Cauchy on local scales → heavy tails + sparsity |
| P2093 | $R$-squared and $F$-test relationship? | $F = \frac{R^2/p}{(1-R^2)/(n-p-1)}$ |
| P2094 | Residual = ? | $e_i = y_i - \hat{y}_i$ |
| P2095 | Standardized residual? | $r_i = e_i/(\hat\sigma\sqrt{1-h_{ii}})$ |
| P2096 | Studentized (externally)? | $t_i = e_i/(\hat\sigma_{(i)}\sqrt{1-h_{ii}})$ |
| P2097 | Hat matrix: $H = X(X^TX)^{-1}X^T$? | $\hat{y} = Hy$ — "hat" because it puts hat on $y$ |
| P2098 | $\text{trace}(H) = ?$ | $p$ (number of parameters including intercept) |
| P2099 | High leverage points: $h_{ii} > ?$ | $2p/n$ or $3p/n$ (rules of thumb) |
| P2100 | Variance-covariance of $\hat\beta$ in OLS? | $\sigma^2(X^TX)^{-1}$ |
| P2101 | Gauss-Markov conditions? | $E[\epsilon] = 0$, $\text{Var}(\epsilon) = \sigma^2I$, $X$ fixed |
| P2102 | BLUE = ? | Best Linear Unbiased Estimator (OLS under GM) |
| P2103 | GLS when $\text{Var}(\epsilon) = \sigma^2\Omega$? | $\hat\beta_{GLS} = (X^T\Omega^{-1}X)^{-1}X^T\Omega^{-1}y$ |
| P2104 | WLS: special case of GLS? | $\Omega$ diagonal → different weights per observation |
| P2105 | Sandwich estimator? | Robust SE: $\hat{V} = (X^TX)^{-1}X^T\hat\Sigma X(X^TX)^{-1}$ |
| P2106 | Newey-West? | HAC robust SE (heteroscedasticity + autocorrelation) |
| P2107 | Cluster-robust SE? | Account for within-cluster correlation |
| P2108 | Wild bootstrap? | Resample residuals with random signs |
| P2109 | Prediction interval vs CI? | PI wider (includes individual variation) |
| P2110 | For simple regression: PI width? | $\hat{y} \pm t_{\alpha/2}\hat\sigma\sqrt{1 + 1/n + (x-\bar{x})^2/S_{xx}}$ |
| P2111 | Extrapolation danger? | Model may not hold outside training range |
| P2112 | Influential observation? | Changes fit substantially when removed |
| P2113 | Multicollinearity: VIF formula? | $VIF_j = 1/(1-R_j^2)$ where $R_j^2$ = $R^2$ of $X_j$ on other $X$'s |
| P2114 | VIF > 10: serious problem | Remove variable or use Ridge |
| P2115 | Condition number of $X^TX > 30$? | Multicollinearity |
| P2116 | Stepwise regression: issues? | Multiple testing, biased $R^2$, unstable |
| P2117 | L-curve for regularization? | Plot $\|\hat\beta\|$ vs $\|y-X\hat\beta\|$; choose corner |
| P2118 | Cross-validation: choose $\lambda$? | Minimize CV error |
| P2119 | One-standard-error rule? | Choose simplest model within 1 SE of best |
| P2120 | Elastic net: $\alpha$ parameter? | Mixing of L1 and L2: $\alpha=1$ = LASSO, $\alpha=0$ = Ridge |
| P2121 | $E[S^2] = ?$ | $\sigma^2$ (by construction) |
| P2122 | $\text{Var}(S^2) = ?$ for normal | $2\sigma^4/(n-1)$ |
| P2123 | $E[\bar{X}^2] = ?$ | $\mu^2 + \sigma^2/n$ |
| P2124 | Sample median: variance for large $n$? | $\approx \frac{1}{4nf^2(m)}$ where $m$ = population median |
| P2125 | For Normal: median vs mean efficiency? | Median efficiency = $2/\pi \approx 0.637$ |
| P2126 | Trimmed mean ($\alpha = 0.1$) estimator? | Remove 10% from each tail, average rest |
| P2127 | MAD = ? | Median Absolute Deviation: $\text{med}(|X_i - \text{med}(X)|)$ |
| P2128 | MAD-based $\sigma$ estimate? | $\hat\sigma = 1.4826 \times MAD$ (for normal) |
| P2129 | Interquartile range? | $IQR = Q_3 - Q_1$ |
| P2130 | Outlier by IQR rule? | $< Q_1 - 1.5\times IQR$ or $> Q_3 + 1.5\times IQR$ |
| P2131 | Box plot whiskers? | To most extreme non-outlier point |
| P2132 | Violin plot? | Box plot + kernel density on sides |
| P2133 | Error bar for mean? | $\bar{X} \pm SE$ or $\bar{X} \pm 1.96 \times SE$ |
| P2134 | Confidence band for regression line? | Wider at $x$ values far from $\bar{X}$ |
| P2135 | Bootstrap percentile CI? | $(q_{0.025}^*, q_{0.975}^*)$ from bootstrap distribution |
| P2136 | Pivotal bootstrap CI? | $2\hat\theta - q_{0.975}^*$ to $2\hat\theta - q_{0.025}^*$ (reversed!) |
| P2137 | BCa CI? | Bias-corrected accelerated: adjusts for skewness |
| P2138 | Non-overlapping CIs → significant? | YES (conservative: actually too strict) |
| P2139 | Overlapping CIs → not significant? | NOT necessarily! Can still be significant |
| P2140 | $p$-value hacking? | Manipulating analysis until $p < 0.05$ |
| P2141 | Pre-registration prevents? | $p$-hacking, HARKing |
| P2142 | HARKING? | Hypothesizing After Results are Known |
| P2143 | Multiple testing: Holm's method? | Step-down Bonferroni (more powerful) |
| P2144 | Benjamini-Hochberg? | Controls FDR (False Discovery Rate) |
| P2145 | $q$-value? | Minimum FDR at which hypothesis is rejected |
| P2146 | Storey's method? | Estimates proportion of true nulls |
| P2147 | Replication crisis? | Many published results don't replicate |
| P2148 | Power of typical study? | Often only $\approx 50\%$ (underpowered!) |
| P2149 | Effect size reporting? | Always report alongside p-value |
| P2150 | Confidence intervals > $p$-values? | Recommended by ASA |
| P2151 | Likelihood principle? | All evidence from data is in likelihood |
| P2152 | Sufficiency principle? | If T sufficient, inference should use only T |
| P2153 | Conditionality principle? | Condition on ancillary statistics |
| P2154 | Birnbaum's theorem? | Sufficiency + Conditionality → Likelihood principle |
| P2155 | Fiducial inference (Fisher)? | Historical: "distribution of parameter" without prior |
| P2156 | Objective Bayes? | Reference/Jeffreys priors aim for objectivity |
| P2157 | Robust Bayes? | Study sensitivity to prior choice |
| P2158 | Imprecise probability? | Sets of priors → sets of posteriors |
| P2159 | Conformal prediction? | Distribution-free prediction sets with coverage guarantee |
| P2160 | The LAST problem: what's the most important thing? | **PRACTICE. Do problems. Read solutions. Repeat. 🔁** |

---

# 📊 ULTIMATE FINAL Problem Index (All 26 Sets + Lightning Round)

| Set | Problems | Topics | Count |
|---|---|---|---|
| Set 1 | P1 – P80 | Fundamentals, Bayes, Distributions | 80 |
| Set 2 | P81 – P160 | GATE Simulation | 80 |
| Set 3 | P161 – P250 | Advanced, Tricky, Statistical Concepts | 90 |
| Set 4 | P251 – P320 | Markov Chains, Stochastic Processes | 70 |
| Set 5 | P321 – P420 | Expectation, Variance, Statistical Tests | 100 |
| Set 6 | P421 – P500 | Distributions, Transforms, Info Theory | 80 |
| Set 7 | P501 – P600 | GATE Predicted, ML Connections | 100 |
| Set 8 | P601 – P750 | GATE Marathon | 150 |
| Set 9 | P751 – P830 | True/False Conceptual + Speed | 80 |
| Set 10 | P831 – P900 | Express Final + ML/Stats Connections | 70 |
| Set 11 | P901 – P950 | Worked Examples — Full Solutions | 50 |
| Set 12 | P951 – P1020 | GATE PYQ Reproductions | 70 |
| Set 13 | P1021 – P1100 | Cross-Topic Integration | 80 |
| Set 14 | P1101 – P1180 | Distribution Identification Drill | 80 |
| Set 15 | P1181 – P1300 | Ultra-Quick One-Liners | 120 |
| Set 16 | P1301 – P1340 | Final Exam Simulation | 40 |
| Set 17 | P1341 – P1400 | Error Analysis & Debugging | 60 |
| Set 18 | P1401 – P1500 | Tricky GATE Traps & Edge Cases | 100 |
| Set 19 | P1501 – P1560 | Multi-Step Simulation | 60 |
| Set 20 | P1561 – P1640 | Extreme Value & Tail Bounds | 80 |
| Set 21 | P1601 – P1680 | Bayesian Deep Dive | 80 |
| Set 22 | P1681 – P1720 | Numerical Constants | 40 |
| Set 23 | P1721 – P1780 | Decision Theory & Info Theory | 60 |
| Set 24 | P1781 – P1860 | Simulation & Computational | 80 |
| Set 25 | P1861 – P1960 | The Last 100 Express | 100 |
| Set 26 | P1961 – P2060 | Advanced Statistical Tests & Methods | 100 |
| Set 27 | P2061 – P2160 | Lightning Round | 100 |
| **GRAND TOTAL** | | | **~2160 problems** |

---

> 📌 **Remember:** In GATE, probability & statistics problems are among the most scoring — they follow fixed patterns. Master the distributions, practice Bayes' theorem until it's second nature, and always verify with boundary cases.

> 💡 **Pro Tip:** Before the exam, review the conjugate prior table, the distribution comparison table, and the statistical table values ($z_{0.025} = 1.96$, $\chi^2$ critical values). These alone can save 5+ minutes during the exam.

> 🧠 **Mindset:** Every GATE probability question has been asked before in some form. If you've solved 2160 problems, you've seen every pattern. Trust your preparation!

---

**🏆 2160 problems. Complete derivations. All formula tables. All statistical tables. Every distribution. Every GATE trap identified. A 7-day revision plan. YOU ARE READY. 🚀🎯💪**

*🏆 End of Probability & Statistics — Comprehensive Notes for GATE 2027 (CS/IT/DA)*

*📅 Last Updated: April 2026 | Covers: GATE CS + GATE DA Syllabus | 100% Exam Coverage*
