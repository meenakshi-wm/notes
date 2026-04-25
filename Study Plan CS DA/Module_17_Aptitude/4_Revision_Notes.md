# Aptitude — Revision Notes for GATE 2027

> Quick-reference sheet: all formulas, shortcuts, and GATE traps.

---

## 1. Summation Formulas

| Sum | Formula |
|-----|---------|
| $\sum_{i=1}^{n} 1$ | n |
| $\sum_{i=1}^{n} i$ | $\frac{n(n+1)}{2}$ |
| $\sum_{i=1}^{n} i^2$ | $\frac{n(n+1)(2n+1)}{6}$ |
| $\sum_{i=1}^{n} i^3$ | $\left[\frac{n(n+1)}{2}\right]^2$ |
| $\sum_{i=0}^{n} r^i$ | $\frac{r^{n+1}-1}{r-1}$ |
| $\sum_{i=0}^{\infty} r^i$ (|r|<1) | $\frac{1}{1-r}$ |
| $\sum_{i=0}^{\infty} ir^i$ (|r|<1) | $\frac{r}{(1-r)^2}$ |
| $\sum_{i=1}^{n} (2i-1)$ | $n^2$ |
| Telescoping $\sum f(i)-f(i-1)$ | $f(n)-f(0)$ |

---

## 2. Sequences & Series

### AP
$a_n = a + (n-1)d$, $S_n = \frac{n}{2}(2a+(n-1)d) = \frac{n}{2}(a+l)$

### GP
$a_n = ar^{n-1}$, $S_n = a\frac{r^n-1}{r-1}$, $S_\infty = \frac{a}{1-r}$ (|r|<1)

### AGP (Infinite, |r|<1)
$S_\infty = \frac{a}{1-r} + \frac{dr}{(1-r)^2}$

### Means
| Type | Formula |
|------|---------|
| AM | $(a+b)/2$ |
| GM | $\sqrt{ab}$ |
| HM | $2ab/(a+b)$ |
| Relation | $AM \geq GM \geq HM$; $AM \times HM = GM^2$ |

---

## 3. Modular Arithmetic

| Theorem | Statement |
|---------|-----------|
| Fermat's Little | $a^{p-1} \equiv 1 \pmod{p}$ (p prime, gcd(a,p)=1) |
| Euler's | $a^{\phi(m)} \equiv 1 \pmod{m}$ (gcd(a,m)=1) |
| Totient | $\phi(n) = n\prod(1-1/p)$ |
| Totient (p prime) | $\phi(p) = p-1$ |
| Totient (pq) | $\phi(pq) = (p-1)(q-1)$ |

**Last digit cyclicity:** Period 4 for most digits.
- Powers of 2: {2,4,8,6}
- Powers of 3: {3,9,7,1}
- Powers of 7: {7,9,3,1}

**Trailing zeros in n!:** $\sum_{i=1}^{\infty}\lfloor n/5^i \rfloor$

**Divisors of $n = p_1^{a_1}\cdots p_k^{a_k}$:** $(a_1+1)\cdots(a_k+1)$

---

## 4. Logarithms

| Rule | Formula |
|------|---------|
| Product | $\log(xy) = \log x + \log y$ |
| Quotient | $\log(x/y) = \log x - \log y$ |
| Power | $\log x^n = n\log x$ |
| Base change | $\log_b a = \frac{\log a}{\log b}$ |
| Reciprocal | $\log_b a = 1/\log_a b$ |
| Identity | $b^{\log_b a} = a$ |

**Number of digits** in N: $\lfloor\log_{10}N\rfloor + 1$

---

## 5. Counting

| Concept | Formula |
|---------|---------|
| IEP (2 sets) | $|A \cup B| = |A|+|B|-|A \cap B|$ |
| IEP (3 sets) | $|A \cup B \cup C| = |A|+|B|+|C|-|AB|-|AC|-|BC|+|ABC|$ |
| Permutations | $P(n,r) = \frac{n!}{(n-r)!}$ |
| Combinations | $C(n,r) = \frac{n!}{r!(n-r)!}$ |
| Circular | $(n-1)!$ |
| With repetition | $n^r$ (r items from n choices) |
| Binomial | $\sum \binom{n}{i} = 2^n$ |

---

## 6. Speed, Time & Distance

| Scenario | Formula |
|----------|---------|
| Basic | D = S × T |
| Same distance, 2 speeds | Avg = $\frac{2ab}{a+b}$ (HM) |
| Same time, 2 speeds | Avg = $\frac{a+b}{2}$ (AM) |
| Relative (same dir) | $|v_1-v_2|$ |
| Relative (opposite) | $v_1+v_2$ |
| km/h → m/s | × 5/18 |
| m/s → km/h | × 18/5 |

### Trains
| Event | Distance |
|-------|----------|
| Train passes pole/person | L (train length) |
| Train passes platform | L + P |
| Two trains cross | $L_1 + L_2$ |

### Boats
$b = (D+U)/2$, $s = (D-U)/2$

---

## 7. Percentages

| Formula | |
|---------|--|
| % change | $\frac{\text{New}-\text{Old}}{\text{Old}} \times 100$ |
| Successive changes a%, b% | $a+b+\frac{ab}{100}$% |
| If A is x% more than B | B is $\frac{x}{100+x}\times100$% less than A |

**Key fraction-percent:**
1/2=50%, 1/3≈33.3%, 1/4=25%, 1/5=20%, 1/6≈16.7%, 1/7≈14.3%, 1/8=12.5%

---

## 8. Ratio & Proportion

- a:b = c:d ⟹ ad = bc
- Componendo-Dividendo: $\frac{a+b}{a-b} = \frac{c+d}{c-d}$
- To combine a:b and b:c → make b common by finding LCM

---

## 9. Profit, Loss & Discount

| Formula | |
|---------|--|
| Profit% | $(SP-CP)/CP \times 100$ |
| SP from profit | $CP \times (1+P\%/100)$ |
| Successive discounts a%, b% | Equivalent = $a+b-ab/100$% |
| Dishonest dealer | Gain% = (True−False)/False × 100 |

---

## 10. SI & CI

| | Formula |
|--|---------|
| SI | $PRT/100$ |
| CI Amount | $P(1+R/100)^T$ |
| CI−SI (2 yrs) | $P(R/100)^2$ |
| Doubling time (CI) | ≈ 72/R (Rule of 72) |

---

## 11. Time & Work

| | Formula |
|--|---------|
| Rate | 1/days to finish |
| A+B together | $\frac{ab}{a+b}$ days |
| LCM method | Total work = LCM of days |
| Pipes net rate | Inlet − Outlet |
| M₁D₁ = M₂D₂ | (for same work) |

---

## 12. Floor & Ceiling

| Property | Formula |
|----------|---------|
| $\lfloor x \rfloor$ domain | $x-1 < \lfloor x\rfloor \leq x$ |
| $\lceil x \rceil$ domain | $x \leq \lceil x\rceil < x+1$ |
| Negation | $\lfloor-x\rfloor = -\lceil x\rceil$ |
| Integer shift | $\lfloor x+n\rfloor = \lfloor x\rfloor+n$ |
| Ceiling alt | $\lceil n/k\rceil = \lfloor(n-1)/k\rfloor+1$ |
| Multiples of k in [1,n] | $\lfloor n/k\rfloor$ |

---

## 13. Absolute Value

$|x| < a \iff -a < x < a$
$|x| > a \iff x > a$ or $x < -a$
Triangle inequality: $|a+b| \leq |a|+|b|$

---

## 14. Common GATE Traps

| Trap | Correct |
|------|---------|
| Average of 2 speeds = AM | **HM** if distances are equal |
| +25% then −25% = 0% | Net = −6.25% |
| Profit on SP | Profit% is always on **CP** |
| ⌊−3.7⌋ = −3 | **−4** (goes more negative) |
| ⌈−3.7⌉ = −4 | **−3** (goes toward zero) |
| log₂0 = 0 | **Undefined** |
| n! trailing zeros by /10 | Must count powers of **5** only |
| CI for half-year: same R | R/2 for rate, 2T for time |
| Train passes pole: D = L+0 | Correct: D = L |
| Circular permutation of n | (n−1)!, NOT n! |

---

## 15. Calendar & Clock

### Calendar
- Odd days in: 1 year = 1 (ordinary), 2 (leap)
- 100 yrs = 5; 200 = 3; 300 = 1; 400 = 0 odd days
- Leap: ÷4 yes, ÷100 no, ÷400 yes

### Clock
- Angle: $\theta = |30H - 5.5M|$ (take min with 360−θ)
- Overlap: 22 times / 24 hours (11 per 12 hrs)
- Hour hand: 0.5°/min; Minute hand: 6°/min
- At 3:15 the angle is 7.5° (NOT 0°)

---

## 16. Analytical Quick Reference

| Type | Key Method |
|---|---|
| Blood relations | Draw family tree |
| Seating (circular) | Fix one, arrange rest |
| Direction | Track x, y coordinates |
| Coding | Check reverse, shift, swap |
| Series | Check differences, ratios, $n(n+1)$, $n^2$, etc. |

---

## 17. Verbal Quick Reference

| Rule | Example |
|---|---|
| Each/every → singular verb | "Each **has**" |
| Neither...nor → nearer subject | "Neither A nor B **is**" |
| affect = verb, effect = noun | "It **affects** the **effect**" |
| Dangling modifier | Subject must follow modifier |
| "One of those who" → plural | "who **have**" |

---

## 18. DI Quick Reference

- Pie chart angle = (value/total) × 360°
- Growth% = (new−old)/old × 100
- Always check axis labels and units first
- Approximate: save time on complex fractions

---

*End of Revision Notes — Aptitude for GATE 2027*
