# 🧠💯 Aptitude — Detailed Notes for GATE 2027 🚀🎯

> *"Pure mathematics is, in its way, the poetry of logical ideas."* — **Albert Einstein** 🌟

> Covers: Summation, Sequences & Series (AP/GP/AGP), Modular Arithmetic, Logarithms, Counting Principles, Speed-Time-Distance, Trains & Boats, Percentages, Ratio & Proportion, Absolute Value, Floor & Ceiling, SI/CI, Profit/Loss, Time & Work, Number System, Spatial & Analytical Aptitude.

---

### 🎯 Why Aptitude Matters? — A Beginner's Perspective

> 🍼 **Think of Aptitude as your Mathematical Common Sense:**
> - Every GATE paper has **15 marks of aptitude** (10 GA questions) — that's FREE marks if you prepare well! 🏆
> - These topics are also asked in placements at **Google, Amazon, Microsoft, TCS, Infosys**!
> - Aptitude = the math you use in daily life but formalized with formulas 📝
> - If you can calculate a restaurant tip or figure out a train's arrival time, you already know aptitude! 🚄

### 🏭 Real-World Aptitude Applications — Math is Everywhere!

| 🏢 Domain | 📊 Application | 💡 Topic Used | 🔥 Example |
|---|---|---|---|
| **Banking** 🏦 | EMI & Interest calculation | SI/CI, Percentages | Home loan at 8.5% for 20 years |
| **E-commerce** 🛒 | Discount calculations | Percentages, Profit/Loss | "Flat 40% off + extra 10% = ?" |
| **Logistics** 🚚 | Delivery time estimation | Speed-Time-Distance | Amazon Prime same-day delivery |
| **Cryptography** 🔐 | RSA encryption | Modular Arithmetic | Every HTTPS website uses this! |
| **Stock Market** 📈 | Growth calculations | GP, Percentages, Logarithms | "Company grew at 15% CAGR" |
| **Engineering** ⚙️ | Gear ratios, mixtures | Ratio & Proportion | Car transmission systems |
| **Project Management** 📋 | Task scheduling | Time & Work | Sprint planning in Agile teams |
| **Data Science** 📊 | Combinatorial analysis | Counting Principles | A/B testing combinations |
| **Exams** 🎓 | GATE, CAT, GRE, GMAT, Placements | ALL topics | **15 marks guaranteed** in GATE! |

> 🔑 **GATE Tip:** Aptitude is the **highest ROI section** in GATE — minimal effort, maximum marks. Most questions are from Percentages, Time & Work, and Series!

---

## 1. ∑ Summation — Sigma Notation

> 🍼 **Beginner Analogy — The Piggy Bank:**
> Imagine you save money every day 🐷💰:
> - Day 1: ₹1, Day 2: ₹2, Day 3: ₹3... Day 100: ₹100
> - "How much total after 100 days?" = $\sum_{i=1}^{100} i = \frac{100 \times 101}{2} = \text{₹}5050$
> - Sigma (∑) is just a **fancy way of writing "add up all these numbers"** ➕➕➕
> - Instead of writing 1+2+3+...+100, we write $\sum_{i=1}^{100} i$ — clean and compact! ✨

> 🔬 **Fun Fact:** The story goes that 7-year-old **Carl Friedrich Gauss** computed $1+2+3+...+100 = 5050$ in seconds by pairing numbers: $(1+100) + (2+99) + ... = 50 \times 101 = 5050$. His teacher was stunned! 🤯

### 1.1 Sigma Notation Basics
$$\sum_{i=m}^{n} f(i) = f(m) + f(m+1) + \cdots + f(n)$$

**Properties:**
1. Linearity: $\sum (af(i) + bg(i)) = a\sum f(i) + b\sum g(i)$
2. Constant factor: $\sum_{i=1}^{n} c = nc$
3. Splitting: $\sum_{i=1}^{n} f(i) = \sum_{i=1}^{k} f(i) + \sum_{i=k+1}^{n} f(i)$
4. Index shift: $\sum_{i=m}^{n} f(i) = \sum_{j=0}^{n-m} f(j+m)$

### 1.2 Standard Summation Formulas
| Sum | Formula |
|-----|---------|
| $\sum_{i=1}^{n} i$ | $\frac{n(n+1)}{2}$ |
| $\sum_{i=1}^{n} i^2$ | $\frac{n(n+1)(2n+1)}{6}$ |
| $\sum_{i=1}^{n} i^3$ | $\left[\frac{n(n+1)}{2}\right]^2$ |
| $\sum_{i=0}^{n} r^i$ | $\frac{r^{n+1}-1}{r-1}$ (r≠1) |
| $\sum_{i=0}^{\infty} r^i$ | $\frac{1}{1-r}$ (|r|<1) |

### 1.3 Telescoping Sums
If $f(i) = g(i) - g(i-1)$, then:
$$\sum_{i=1}^{n} f(i) = g(n) - g(0)$$

**Example:** $\sum_{i=1}^{n} \frac{1}{i(i+1)} = \sum_{i=1}^{n}\left(\frac{1}{i}-\frac{1}{i+1}\right) = 1 - \frac{1}{n+1} = \frac{n}{n+1}$

---

## 2. 📈 Sequences & Series

> 🍼 **Beginner Analogy — Patterns in Daily Life:**
> - **AP (Arithmetic):** Like climbing stairs 🪤 — each step adds the SAME height. Salary increment: ₹30K, ₹35K, ₹40K, ₹45K... (+₹5K each year)
> - **GP (Geometric):** Like a viral video 📱 — each share MULTIPLIES. 1 person shares to 3, those 3 share to 9, then 27, then 81... (×3 each time)
> - **AGP:** Like a growing investment with regular deposits — a combination of both!

> 🏭 **Real-World Applications:**
> - 🏦 **Bank FD:** Compound interest follows GP! ₹1,00,000 at 10% = ₹1,10,000 → ₹1,21,000 → ...
> - 🧬 **Bacteria Growth:** 1 → 2 → 4 → 8 (GP with r=2, doubles every 20 min!)
> - 📉 **Depreciation:** Car value decreases in GP: ₹10L → ₹8L → ₹6.4L (r=0.8)

### 2.1 Arithmetic Progression (AP)
$$a_n = a + (n-1)d$$

**Sum of n terms:**
$$S_n = \frac{n}{2}[2a + (n-1)d] = \frac{n}{2}(a + l)$$
where a = first term, d = common difference, l = last term.

**Properties:**
- AM of a and b: $(a+b)/2$
- If a, b, c are in AP: $2b = a + c$
- Inserting k AM between a and b: $d = \frac{b-a}{k+1}$
- Sum of AP is quadratic in n

**Example:** Find sum of first 50 odd numbers.
a=1, d=2, n=50. $S = \frac{50}{2}[2(1)+49(2)] = 25(100) = 2500 = 50^2$

### 2.2 Geometric Progression (GP)
$$a_n = ar^{n-1}$$

**Sum of n terms:**
$$S_n = a\cdot\frac{r^n - 1}{r - 1} \quad (r \neq 1)$$

**Infinite GP (|r| < 1):**
$$S_\infty = \frac{a}{1-r}$$

**Properties:**
- GM of a and b: $\sqrt{ab}$
- If a, b, c are in GP: $b^2 = ac$
- Inserting k GM between a and b: $r = (b/a)^{1/(k+1)}$

**Example:** Sum of 1 + 1/2 + 1/4 + 1/8 + ... = $\frac{1}{1-1/2} = 2$

### 2.3 Arithmetico-Geometric Progression (AGP)
Each term = (AP term)(GP term): $a_n = [a + (n-1)d] \cdot r^{n-1}$

**Sum technique:** Multiply $S_n$ by $r$, subtract from $S_n$:
$$S_n - rS_n = a + d(r + r^2 + \cdots + r^{n-1}) - [a + (n-1)d]r^n$$

**Infinite sum (|r| < 1):**
$$S_\infty = \frac{a}{1-r} + \frac{dr}{(1-r)^2}$$

**Example:** Find $S = 1 + 3x + 5x^2 + 7x^3 + ...$, |x|<1
Here a=1, d=2, r=x. $S = \frac{1}{1-x} + \frac{2x}{(1-x)^2} = \frac{1+x}{(1-x)^2}$

---

## 3. 🔢 Modular Arithmetic

> 🍼 **Beginner Analogy — The Clock Analogy:**
> Think of a clock 🕐:
> - After 12, it goes back to 1 (not 13!). That's **mod 12**!
> - 15 o'clock = 3 o'clock (because 15 mod 12 = 3)
> - Days of the week: Today is Monday, what day is it after 100 days? 100 mod 7 = 2 → **Wednesday!** 📅
> - **Modular arithmetic = math with wraparound** — like an odometer resetting at 99999! 🚗

> 🏭 **Critical Production Uses:**
> - 🔐 **RSA Encryption:** The ENTIRE internet's security is based on modular arithmetic! Every HTTPS connection uses $m^e \mod n$
> - #️⃣ **Hash Functions:** `hashCode % tableSize` — every HashMap in Java/Python uses mod!
> - 💻 **Checksums:** ISBN, Credit Card (Luhn algorithm), Aadhaar — all use mod for error detection
> - 🎮 **Random Number Generators:** Most use Linear Congruential Generator: $X_{n+1} = (aX_n + c) \mod m$

### 3.1 Basics
$a \equiv b \pmod{m}$ means $m | (a-b)$

**Properties:**
- $(a+b) \bmod m = [(a \bmod m) + (b \bmod m)] \bmod m$
- $(a \cdot b) \bmod m = [(a \bmod m) \cdot (b \bmod m)] \bmod m$
- $(a^n) \bmod m$: use repeated squaring

### 3.2 Key Theorems

**Fermat's Little Theorem:** If p is prime and $\gcd(a,p) = 1$:
$$a^{p-1} \equiv 1 \pmod{p}$$

**Euler's Theorem:** If $\gcd(a,m) = 1$:
$$a^{\phi(m)} \equiv 1 \pmod{m}$$

**Euler's Totient $\phi(n)$:**
$$\phi(n) = n\prod_{p|n}\left(1 - \frac{1}{p}\right)$$

| n | φ(n) | Rule |
|---|------|------|
| p (prime) | p−1 | |
| p^k | p^k − p^(k−1) | |
| pq (p,q prime) | (p−1)(q−1) | |
| 2m (m odd) | φ(m) | |

### 3.3 Divisibility Rules
| By | Rule |
|----|------|
| 2 | Last digit even |
| 3 | Digit sum divisible by 3 |
| 4 | Last 2 digits div by 4 |
| 5 | Last digit 0 or 5 |
| 8 | Last 3 digits div by 8 |
| 9 | Digit sum div by 9 |
| 11 | Alternating digit sum div by 11 |

### 3.4 Finding Last Digits / Remainders
**Pattern of last digits** for powers of a:
- Cyclicity of 2: {2,4,8,6} → period 4
- Cyclicity of 3: {3,9,7,1} → period 4
- Cyclicity of 7: {7,9,3,1} → period 4

**Example:** Last digit of $7^{100}$?
$100 \bmod 4 = 0$, so last digit = 4th in cycle = **1**

**Example:** $2^{100} \bmod 7$?
By Fermat: $2^6 \equiv 1 \pmod 7$. $100 = 6(16)+4$. So $2^{100} \equiv 2^4 = 16 \equiv 2 \pmod 7$.

---

## 4. 📊 Logarithms

> 🍼 **Beginner Analogy — The "How Many Times" Counter:**
> Logarithm answers: **"How many times do I multiply?"** 🤔
> - $\log_2 8 = 3$ means "2 × 2 × 2 = 8" → you multiplied **3 times**
> - $\log_{10} 1000 = 3$ means "10 × 10 × 10 = 1000" → **3 times**
> - Think of it as the **reverse of exponentiation** ↔️
> - If exponentiation is like climbing UP a ladder, logarithm is counting HOW MANY rungs you climbed! 🪤

> 🏭 **Where Logarithms Show Up:**
> - 💻 **Binary Search:** $O(\log n)$ — finding a word in a dictionary of 1 million words in just 20 steps!
> - 📢 **Decibels:** Sound is measured on a logarithmic scale (every +10 dB = 10× louder!)
> - 🌍 **Richter Scale:** Each earthquake magnitude = 10× more energy ($\log_{10}$ scale)
> - 📊 **Data Science:** Log transformations make skewed data normal (income distribution)

### 4.1 Definition & Properties
$\log_b a = c$ means $b^c = a$ (b > 0, b ≠ 1, a > 0)

| Property | Formula |
|----------|---------|
| Product | $\log(xy) = \log x + \log y$ |
| Quotient | $\log(x/y) = \log x - \log y$ |
| Power | $\log(x^n) = n\log x$ |
| Change of base | $\log_b a = \frac{\log_c a}{\log_c b}$ |
| Reciprocal | $\log_b a = \frac{1}{\log_a b}$ |
| Chain | $\log_a b \cdot \log_b c = \log_a c$ |
| Identity | $b^{\log_b a} = a$ |

### 4.2 Common Values
| Expression | Value |
|------------|-------|
| $\log_2 1$ | 0 |
| $\log_2 2$ | 1 |
| $\log_2 1024$ | 10 |
| $\log_{10} 10$ | 1 |
| $\ln e$ | 1 |
| $\log_2 n \approx$ | $3.32 \log_{10} n$ |

### 4.3 Solving Equations
**Type 1:** $\log_2(x-1) + \log_2(x+1) = 3 \Rightarrow \log_2(x^2-1) = 3 \Rightarrow x^2 = 9 \Rightarrow x = 3$ (reject −3: domain constraint)

**Type 2:** $2^x = 5 \Rightarrow x = \log_2 5 = \frac{\ln 5}{\ln 2} \approx 2.32$

**GATE Trap:** Always check domain: argument must be positive, base must be positive and ≠1.

---

## 5. 🧩 Counting Principles

> 🍼 **Beginner Analogy — The Outfit Chooser:**
> You have 4 shirts 👕 and 3 pants 👖:
> - **Product Rule (AND):** How many outfits? 4 × 3 = **12** (1 shirt AND 1 pant)
> - **Sum Rule (OR):** Going to a party in EITHER a shirt OR a dress? 4 + 5 = **9** choices
> - **Inclusion-Exclusion:** How many students play cricket OR football? Don't double-count those who play BOTH! 🏏⚽

### 5.1 Sum Rule (OR)
If task can be done in $n_1$ or $n_2$ ways (mutually exclusive):
Total ways = $n_1 + n_2$

### 5.2 Product Rule (AND)
If task needs step 1 ($n_1$ ways) AND step 2 ($n_2$ ways):
Total ways = $n_1 \times n_2$

### 5.3 Inclusion-Exclusion Principle (IEP)
$$|A \cup B| = |A| + |B| - |A \cap B|$$
$$|A \cup B \cup C| = |A|+|B|+|C| - |A\cap B| - |A\cap C| - |B\cap C| + |A\cap B\cap C|$$

**Example:** How many integers from 1 to 100 are divisible by 2 or 3?
|A|=50 (div by 2), |B|=33 (div by 3), |A∩B|=16 (div by 6).
Answer = 50 + 33 − 16 = **67**

### 5.4 Subtraction Rule (Complement)
Count of desired = Total − Count of undesired

**Example:** 4-digit numbers with at least one digit 5?
Total 4-digit = 9000 (1000-9999). Without any 5: 8×9×9×9 = 5832. Answer = 9000−5832 = **3168**

---

## 6. 🚗💨 Speed, Time & Distance

> 🍼 **Beginner Analogy — The Road Trip:**
> Planning a road trip from Delhi to Jaipur (280 km)? 🗺️
> - At 70 km/h, it takes 280/70 = **4 hours**
> - Want to reach in 2 hours? You need 280/2 = **140 km/h** (don't try this! 😅)
> - **The golden formula:** Distance = Speed × Time (D = S × T)
> - **GATE Trap:** Average speed is NOT the average of two speeds! If you go at 30 km/h and return at 60 km/h, average is NOT 45 but $\frac{2 \times 30 \times 60}{30+60} = 40$ km/h! ⚠️

### 6.1 Basic Formula
$$\text{Distance} = \text{Speed} \times \text{Time}$$
$$\text{Speed} = \frac{D}{T}, \quad \text{Time} = \frac{D}{S}$$

### 6.2 Average Speed
$$\text{Average Speed} = \frac{\text{Total Distance}}{\text{Total Time}}$$

**Same distance at speeds a and b:**
$$\text{Avg Speed} = \frac{2ab}{a+b} \quad \text{(Harmonic Mean)}$$

**Same time at speeds a and b:**
$$\text{Avg Speed} = \frac{a+b}{2} \quad \text{(Arithmetic Mean)}$$

### 6.3 Relative Speed
- Same direction: $|v_1 - v_2|$
- Opposite direction: $v_1 + v_2$

### 6.4 Unit Conversions
- km/h to m/s: multiply by $\frac{5}{18}$
- m/s to km/h: multiply by $\frac{18}{5}$

**Example:** A travels 60 km at 30 km/h, then 60 km at 60 km/h.
Time₁ = 2h, Time₂ = 1h. Avg speed = 120/3 = **40 km/h** (NOT 45!)

---

## 7. Problems on Trains

### 7.1 Key Concepts
- A train of length L passes a pole: **D = L**, Time = L/S
- A train of length L passes a platform of length P: **D = L + P**
- Two trains (lengths L₁, L₂) crossing each other:
  - Same direction: $D = L_1 + L_2$, relative speed = $|S_1 - S_2|$
  - Opposite direction: $D = L_1 + L_2$, relative speed = $S_1 + S_2$
- A train passes a person: treat person as point (length 0)
  - Same direction: relative speed = S_train − S_person
  - Opposite: relative speed = S_train + S_person

**Example:** 200m train at 72 km/h passes bridge of 300m.
D = 200+300 = 500m. Speed = 72 × 5/18 = 20 m/s. Time = 500/20 = **25s**

---

## 8. Boats & Streams

### 8.1 Formulas
- Still water speed = b, Stream speed = s
- **Downstream speed** = b + s
- **Upstream speed** = b − s

From downstream (D) and upstream (U) speeds:
$$b = \frac{D+U}{2}, \quad s = \frac{D-U}{2}$$

**Example:** Boat goes 24 km downstream in 2h and 24 km upstream in 3h.
D-speed = 12, U-speed = 8. b = 10 km/h, s = 2 km/h.

---

## 9. Percentages

### 9.1 Basics
$$\text{Percentage} = \frac{\text{Part}}{\text{Whole}} \times 100$$

### 9.2 Percentage Change
$$\text{% change} = \frac{\text{New} - \text{Old}}{\text{Old}} \times 100$$

### 9.3 Successive Percentage Changes
If value changes by a% then b%:
$$\text{Net change} = a + b + \frac{ab}{100} \%$$

**Example:** Increase 20%, then decrease 20%?
Net = 20 + (−20) + (20)(−20)/100 = 0 − 4 = **−4%** (NOT 0%!)

### 9.4 Fraction-Percentage Equivalents
| Fraction | % | Fraction | % |
|----------|---|----------|---|
| 1/2 | 50 | 1/6 | 16.67 |
| 1/3 | 33.33 | 1/7 | 14.28 |
| 1/4 | 25 | 1/8 | 12.5 |
| 1/5 | 20 | 1/9 | 11.11 |

---

## 10. Ratio & Proportion

### 10.1 Ratio
a:b = a/b. Properties:
- ka : kb = a : b (multiply both by same number)
- a:b and b:c → a:b:c by making b common

### 10.2 Proportion
a:b = c:d means ad = bc (cross multiply)

**Types:**
- Direct proportion: $y = kx$
- Inverse proportion: $y = k/x$
- Componendo-Dividendo: If $\frac{a}{b} = \frac{c}{d}$, then $\frac{a+b}{a-b} = \frac{c+d}{c-d}$

### 10.3 Mixtures / Alligation
$$\frac{q_1}{q_2} = \frac{A_2 - A_{mean}}{A_{mean} - A_1}$$
where $A_1, A_2$ are individual attributes, $A_{mean}$ is mixture attribute.

---

## 11. Absolute Value

### 11.1 Definition
$$|x| = \begin{cases} x & \text{if } x \geq 0 \\ -x & \text{if } x < 0 \end{cases}$$

### 11.2 Properties
- $|x| \geq 0$ always
- $|xy| = |x||y|$
- $|x+y| \leq |x| + |y|$ (Triangle Inequality)
- $|x-y| \geq ||x| - |y||$ (Reverse Triangle Inequality)
- $|x| < a \Leftrightarrow -a < x < a$
- $|x| > a \Leftrightarrow x > a$ or $x < -a$

### 11.3 Solving |f(x)| = g(x)
Method: Case split. f(x) = g(x) OR f(x) = −g(x). Check each solution satisfies g(x) ≥ 0.

**Example:** |2x−3| = 5 → 2x−3=5 (x=4) or 2x−3=−5 (x=−1). Both valid.

---

## 12. Floor & Ceiling Functions

### 12.1 Definitions
- $\lfloor x \rfloor$ = greatest integer ≤ x (floor)
- $\lceil x \rceil$ = least integer ≥ x (ceiling)

### 12.2 Properties
| Property | Statement |
|----------|-----------|
| Range | $x-1 < \lfloor x \rfloor \leq x \leq \lceil x \rceil < x+1$ |
| Integer | $\lfloor n \rfloor = \lceil n \rceil = n$ |
| Relation | $\lceil x \rceil = \lfloor x \rfloor + 1$ (if x ∉ ℤ); $= \lfloor x \rfloor$ (if x ∈ ℤ) |
| Negation | $\lfloor -x \rfloor = -\lceil x \rceil$; $\lceil -x \rceil = -\lfloor x \rfloor$ |
| Addition | $\lfloor x+n \rfloor = \lfloor x \rfloor + n$ (n integer) |
| Sum | $\lfloor x \rfloor + \lfloor y \rfloor \leq \lfloor x+y \rfloor \leq \lfloor x \rfloor + \lfloor y \rfloor + 1$ |

### 12.3 Applications
- Multiples of k in [1, n]: $\lfloor n/k \rfloor$
- $\lceil n/k \rceil = \lfloor (n+k-1)/k \rfloor = \lfloor (n-1)/k \rfloor + 1$

**Example:** Multiples of 7 in [1, 100] = ⌊100/7⌋ = **14**

---

## 13. 💰 Simple & Compound Interest

> 🍼 **Beginner Analogy — The Growing Piggy Bank:**
> - **Simple Interest:** Like a fixed salary bonus — you earn the SAME ₹100 every year, no matter what! 💵
> - **Compound Interest:** Like a snowball rolling downhill ❄️ — it grows, and the GROWTH itself grows!
> - ₹10,000 at 10% SI: Year 1 = ₹1000, Year 2 = ₹1000, Year 3 = ₹1000 (always ₹1000)
> - ₹10,000 at 10% CI: Year 1 = ₹1000, Year 2 = ₹1100, Year 3 = ₹1210 (interest on interest!)
> - **Einstein reportedly called compound interest the "8th wonder of the world"** 🌍

> 🏭 **Real-World:** Every bank loan (home, car, education) uses CI. Warren Buffett's wealth is 99% due to CI — he earned $108B of his $110B after age 50! 📈

### 13.1 Simple Interest (SI)
$$SI = \frac{P \cdot R \cdot T}{100}, \quad A = P + SI = P\left(1 + \frac{RT}{100}\right)$$

### 13.2 Compound Interest (CI)
$$A = P\left(1 + \frac{R}{100}\right)^T, \quad CI = A - P$$

**Half-yearly:** Rate = R/2, Time = 2T
**Quarterly:** Rate = R/4, Time = 4T

### 13.3 CI vs SI Difference
For 2 years: $CI - SI = P\left(\frac{R}{100}\right)^2$
For 3 years: $CI - SI = P\frac{R^2(R+300)}{10^6}$

**Example:** P=10000, R=10%, T=2.
SI = 2000. CI = 10000(1.1²)−10000 = 2100. Diff = 100.

---

## 14. Profit, Loss & Discount

### 14.1 Formulas
- Profit = SP − CP; Loss = CP − SP
- Profit% = $\frac{SP-CP}{CP} \times 100$
- SP = CP × (1 + P%/100) [profit]; SP = CP × (1 − L%/100) [loss]
- Marked Price (MP): Discount = MP − SP
- Discount% = $\frac{MP-SP}{MP} \times 100$

### 14.2 Successive Discounts
Discounts of a% and b%: equivalent single discount = $a + b - \frac{ab}{100}$%

### 14.3 Dishonest Dealer
Uses less weight/quantity: Gain% = $\frac{\text{True weight} - \text{False weight}}{\text{False weight}} \times 100$

**Example:** CP=400, SP=500. Profit = 100. Profit% = 100/400 × 100 = **25%**

---

## 15. ⏰ Time & Work

> 🍼 **Beginner Analogy — The Pizza Eating Contest:**
> Imagine eating a whole pizza 🍕:
> - You can finish it in **6 hours** (rate = 1/6 pizza per hour) 👤
> - Your friend can finish it in **3 hours** (rate = 1/3 pizza per hour) 👤
> - Together? NOT 4.5 hours! It's $\frac{1}{1/6 + 1/3} = \frac{1}{1/2} = 2$ hours! 🎉
> - **Key insight:** ADD the rates, not the times!
> - **LCM method** makes calculations much easier — take total work = LCM of individual days ✨

> 🏭 **Real-World Application:** Project managers at Google/Amazon use these exact concepts for sprint planning — "If Developer A takes 5 days and Developer B takes 3 days, how long together?" 💻

### 15.1 Basic Concept
If A completes work in $d$ days: A's rate = $1/d$ per day.

### 15.2 Combined Work
A (rate 1/a) and B (rate 1/b) together:
$$\text{Rate} = \frac{1}{a} + \frac{1}{b}, \quad \text{Time} = \frac{ab}{a+b}$$

### 15.3 LCM Method
Take total work = LCM of individual days.
- A=12 days, B=15 days. LCM=60 units.
- A's rate = 5 units/day, B's rate = 4 units/day.
- Together: 9 units/day. Time = 60/9 = **6⅔ days**

### 15.4 Pipes & Cisterns
- Inlet fills in a hours: rate = +1/a
- Outlet empties in b hours: rate = −1/b
- Combined time = 1/(1/a − 1/b) = ab/(b−a)

---

## 16. Number System

### 16.1 Types of Numbers
- Natural: {1, 2, 3, ...}
- Whole: {0, 1, 2, ...}
- Integers: {..., −2, −1, 0, 1, 2, ...}
- Rational: p/q (q≠0, p,q integers)
- Irrational: √2, π, e
- Prime: divisible only by 1 and itself (2 is smallest, only even prime)

### 16.2 Divisors
If $n = p_1^{a_1} p_2^{a_2} \cdots p_k^{a_k}$:
- Number of divisors: $(a_1+1)(a_2+1)\cdots(a_k+1)$
- Sum of divisors: $\prod \frac{p_i^{a_i+1}-1}{p_i-1}$

### 16.3 GCD & LCM
- $\gcd(a,b) \times \text{lcm}(a,b) = a \times b$
- Euclidean algorithm: $\gcd(a,b) = \gcd(b, a \bmod b)$

### 16.4 Factorial Properties
- Highest power of prime p in n!: $\sum_{i=1}^{\infty}\lfloor n/p^i \rfloor$ (Legendre's formula)

**Example:** Trailing zeros in 100! = ⌊100/5⌋ + ⌊100/25⌋ + ⌊100/125⌋ = 20+4+0 = **24**

---

## 17. Spatial & Analytical Aptitude

### 17.1 Spatial Aptitude
- **Mirror images:** Left-right reversal; letters/numbers flip horizontally
- **Water images:** Top-bottom reversal; letters/numbers flip vertically
- **Figure counting:** Count triangles, squares, lines in complex figures
  - Triangles in figure: count individual → pairs → triples → ...
- **Paper folding & cutting:** Trace folds and cuts, unfold mentally
  - Each fold doubles the cuts when unfolded
- **Dice problems:** Opposite faces, adjacent faces; unfold cube to check
  - Standard die: opposite faces sum to 7 (1-6, 2-5, 3-4)

### 17.2 Analytical Aptitude

#### Blood Relations
- Father/Mother, Son/Daughter, Brother/Sister
- Paternal: father's side; Maternal: mother's side
- **Key trick:** Draw family tree, use gender symbols (M/F)
- "A is the brother of B's father" → A is B's uncle

#### Seating Arrangements
- **Circular:** n people → (n−1)! arrangements. Fix one person, arrange rest.
- **Linear:** n! arrangements for n distinct people
- Constraints: "A next to B" → treat as single unit → (n−1)! × 2!

#### Coding-Decoding
- **Letter shift:** Each letter shifted by fixed amount (A→D, B→E, etc.)
- **Reverse order:** Word reversed then coded
- **Number-letter mapping:** A=1, B=2, ..., Z=26

#### Direction Sense
- Start at origin, follow turns:
  - Left turn from N → W; Right turn from N → E
  - 180° = opposite direction
- Net displacement: $\sqrt{x^2 + y^2}$

#### Calendar Problems ⭐
**Odd Days Method:**
- **Odd days** = remainder when total days ÷ 7
- 1 ordinary year = 365 days = 52 weeks + **1 odd day**
- 1 leap year = 366 days = 52 weeks + **2 odd days**
- 100 years = 76 ordinary + 24 leap = 76 + 48 = **124 odd days** ≡ 5 odd days
- 200 years ≡ 3 odd days; 300 years ≡ 1 odd day; 400 years ≡ 0 odd days

**Day coding:** Sun=0, Mon=1, Tue=2, Wed=3, Thu=4, Fri=5, Sat=6

**Leap Year Rules:**
- Divisible by 4: leap year
- Divisible by 100: NOT leap (exception)
- Divisible by 400: IS leap (exception to exception)
- Examples: 1900 NOT leap, 2000 IS leap, 2024 IS leap

**Worked Example:** What day was 15 August 1947?
- Count odd days from reference (1 Jan 0001 = Monday)
- 1600 years = 0 odd days; 300 years = 1 odd day
- 46 years (1901-1946) = 46 + 11 (leap years) = 57 ≡ 1 odd day
- Jan(31)+Feb(28)+Mar(31)+Apr(30)+May(31)+Jun(30)+Jul(31)+Aug(15) = 227 days ≡ 3
- Total = 0+1+1+3 = 5 odd days → **Friday** ✅ (Independence Day was a Friday!)

#### Clock Problems ⭐
**Angle between hands:** $\theta = |30H - 5.5M|$

where H = hours, M = minutes. Take the smaller angle if > 180°.

**Key facts:**
- Hour hand moves: 0.5° per minute (360° in 12 hours)
- Minute hand moves: 6° per minute (360° in 60 minutes)
- Relative speed: 5.5° per minute
- Hands overlap: every **12/11 hours** ≈ 65.45 minutes
- Hands at 180°: every 12/11 hours (offset from overlap by 6/11 hours)
- 22 times in 24 hours hands overlap (not 24, because 11 times in 12 hours)

**Worked Example:** At 3:20, what is the angle?
$\theta = |30(3) - 5.5(20)| = |90 - 110| = 20°$

### 17.3 Data Interpretation ⭐

#### Types
1. **Tables:** Row-column data; compute percentages, ratios, growth rates
2. **Bar Charts:** Compare magnitudes; watch for scale and starting point
3. **Pie Charts:** Percentage of total; central angle = (value/total) × 360°
4. **Line Graphs:** Trends over time; slope = rate of change

#### Common DI Calculations
| Calculation | Formula |
|---|---|
| Percentage of total | $\frac{\text{part}}{\text{total}} \times 100$ |
| Growth rate | $\frac{\text{new} - \text{old}}{\text{old}} \times 100\%$ |
| CAGR | $\left(\frac{V_f}{V_i}\right)^{1/n} - 1$ |
| Ratio | Simplify by dividing both by GCD |
| Average | $\frac{\text{sum}}{\text{count}}$ |

#### DI Tips
- Read **title, axis labels, units** first
- Approximate aggressively: 31% ≈ 30%, 49/100 ≈ 50%
- For comparisons, often ratios or differences suffice without exact values
- Watch for different scales on dual-axis charts

---

## 18. Verbal Ability

### 18.1 Grammar Rules for GATE

| Rule | Example |
|---|---|
| Subject-verb agreement | "Either A or B **is**" (closest noun) |
| Collective nouns | "The committee **has** decided" (singular) |
| Neither...nor | Verb agrees with nearer subject |
| Each/every | Always singular: "Each of them **is**" |
| Parallel structure | "She likes **reading** and **swimming**" (not "to swim") |
| Tense consistency | Don't mix past and present in same context |

### 18.2 Commonly Confused Words

| Word Pair | Meaning |
|---|---|
| Affect / Effect | Affect = verb (influence); Effect = noun (result) |
| Principal / Principle | Principal = chief/head; Principle = rule/law |
| Complement / Compliment | Complement = complete; Compliment = praise |
| Stationary / Stationery | Stationary = still; Stationery = paper/pens |
| Discrete / Discreet | Discrete = distinct/separate; Discreet = careful/prudent |
| Eminent / Imminent | Eminent = famous; Imminent = about to happen |
| Accept / Except | Accept = receive; Except = exclude |
| Precede / Proceed | Precede = come before; Proceed = move forward |

### 18.3 Reading Comprehension Strategy
1. **Read questions first** — know what to look for
2. **Skim passage** for structure and main idea
3. **Locate keywords** from questions in passage
4. **Don't assume** — answer must be supported by text
5. **Main idea** = what the author is trying to convey overall
6. **Inference** = what logically follows (not stated directly)
7. **Tone:** positive/negative/neutral/critical/satirical

### 18.4 Sentence Correction Patterns
1. **Subject-verb disagreement:** "The group of students **are** studying" → **is**
2. **Dangling modifier:** "Walking down the road, **the tree fell**" → Who was walking?
3. **Pronoun ambiguity:** "A told B that **he** failed" → Who failed?
4. **Misplaced modifier:** "She only ate vegetables" vs "She ate only vegetables"
5. **Run-on sentences:** Two independent clauses joined without proper punctuation

---

*End of Detailed Notes — Aptitude for GATE 2027*
