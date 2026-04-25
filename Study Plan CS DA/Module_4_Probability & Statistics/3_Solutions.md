# Probability & Statistics — Solutions for GATE 2027

---

> Step-by-step solutions for ALL questions in `2_Questions_and_PYQs.md`

---

## Section A: Probability Basics, Inclusion-Exclusion, De Morgan

---

### Q1. Solution

$P(\text{Red or Green}) = P(R) + P(G) = \frac{5}{12} + \frac{3}{12} = \frac{8}{12} = \frac{2}{3}$

(Mutually exclusive events, so no intersection to subtract.)

---

### Q2. Solution

$P(\text{Tea} \cup \text{Coffee}) = 60 + 40 - 20 = 80$

$\text{Neither} = 100 - 80 = \boxed{20}$

---

### Q3. Solution

By inclusion-exclusion:

$|M \cup P \cup C| = 120 + 90 + 60 - 50 - 40 - 30 + 20 = \boxed{170}$

Students who passed at least one subject = 170.

---

### Q4. Solution

$P(A \cap B) = P(A) + P(B) - P(A \cup B) = 0.6 + 0.5 - 0.8 = \boxed{0.3}$

$P(A^c \cap B^c) = P(\overline{A \cup B}) = 1 - P(A \cup B) = 1 - 0.8 = \boxed{0.2}$

$P(A \cap B^c) = P(A) - P(A \cap B) = 0.6 - 0.3 = \boxed{0.3}$

---

### Q5. Solution

Total outcomes = 36.

$A$ = sum is 7: $\{(1,6),(2,5),(3,4),(4,3),(5,2),(6,1)\}$ → $|A| = 6$

$B$ = at least one 4: $\{(4,1),(4,2),(4,3),(4,4),(4,5),(4,6),(1,4),(2,4),(3,4),(5,4),(6,4)\}$ → $|B| = 11$

$A \cap B = \{(3,4),(4,3)\}$ → $|A \cap B| = 2$

$P(A \cup B) = \frac{6 + 11 - 2}{36} = \frac{15}{36} = \boxed{\frac{5}{12}}$

---

### Q6. Solution

Mutually exclusive: $P(A \cap B) = 0$.

$P(A \cup B) = 0.4 + 0.3 = 0.7$

$P(A^c \cap B^c) = 1 - P(A \cup B) = 1 - 0.7 = \boxed{0.3}$

---

### Q7. Solution

By De Morgan's law:

$P(\overline{A \cup B \cup C}) = P(\bar{A} \cap \bar{B} \cap \bar{C})$

This is the probability that NONE of A, B, C occur.

Numerically: $= 1 - P(A \cup B \cup C)$

---

### Q8. Solution

$P(A \cup B \cup C) = 3 \times \frac{1}{4} - 3 \times \frac{1}{16} + \frac{1}{32}$

$= \frac{3}{4} - \frac{3}{16} + \frac{1}{32} = \frac{24}{32} - \frac{6}{32} + \frac{1}{32} = \boxed{\frac{19}{32}}$

---

## Section B: Conditional Probability, Total Probability, Bayes' Theorem

---

### Q9. Solution

$P(\text{both white}) = P(W_1) \times P(W_2 | W_1) = \frac{4}{10} \times \frac{3}{9} = \frac{12}{90} = \boxed{\frac{2}{15}}$

---

### Q10. Solution

Face cards = 12 (4 Jacks + 4 Queens + 4 Kings).

$P(\text{King} | \text{Face card}) = \frac{P(\text{King} \cap \text{Face card})}{P(\text{Face card})} = \frac{4/52}{12/52} = \frac{4}{12} = \boxed{\frac{1}{3}}$

---

### Q11. Solution

$P(\text{Red}) = P(\text{Box 1}) \times P(R|B_1) + P(\text{Box 2}) \times P(R|B_2)$

$= \frac{1}{2} \times \frac{3}{5} + \frac{1}{2} \times \frac{4}{10} = \frac{3}{10} + \frac{2}{10} = \boxed{\frac{1}{2}}$

---

### Q12. Solution

Using Bayes' theorem:

$P(B_1 | R) = \frac{P(R|B_1) \times P(B_1)}{P(R)} = \frac{\frac{3}{5} \times \frac{1}{2}}{\frac{1}{2}} = \frac{3/10}{1/2} = \boxed{\frac{3}{5}}$

---

### Q13. Solution

$P(D) = 0.5 \times 0.03 + 0.3 \times 0.04 + 0.2 \times 0.05 = 0.015 + 0.012 + 0.010 = 0.037$

$P(M_1 | D) = \frac{0.015}{0.037} = \boxed{\frac{15}{37} \approx 0.4054}$

---

### Q14. Solution

$P(+) = P(+|D) \cdot P(D) + P(+|D^c) \cdot P(D^c)$

$= 0.95 \times 0.02 + 0.10 \times 0.98 = 0.019 + 0.098 = 0.117$

$P(D|+) = \frac{0.019}{0.117} = \boxed{0.1624 \approx 16.24\%}$

---

### Q15. Solution

Let $F$ = fair, $H$ = two-headed, $B$ = biased ($P(H) = 3/4$).

$P(\text{2H} | F) = (1/2)^2 = 1/4$

$P(\text{2H} | H) = 1$

$P(\text{2H} | B) = (3/4)^2 = 9/16$

$P(\text{2H}) = \frac{1}{3}\left(\frac{1}{4} + 1 + \frac{9}{16}\right) = \frac{1}{3} \times \frac{4 + 16 + 9}{16} = \frac{29}{48}$

$P(F | \text{2H}) = \frac{(1/4)(1/3)}{29/48} = \frac{1/12}{29/48} = \frac{48}{348} = \boxed{\frac{4}{29}}$

---

### Q16. Solution

Each child can be represented as (gender, day) — 14 possibilities per child, so 196 total for two children.

"At least one boy born on Tuesday": Total pairs with at least one (Boy, Tue):
- First child is (Boy, Tue): $1 \times 14 = 14$ options for second child
- Second child is (Boy, Tue): $14 \times 1 = 14$ options for first child
- Both (Boy, Tue): 1 (double-counted)

$|\text{at least one B-Tue}| = 14 + 14 - 1 = 27$

Both boys: First is (Boy, Tue) and second is Boy: $1 \times 7 = 7$, plus first is Boy and second is (Boy, Tue): $7 \times 1 = 7$, minus both (Boy, Tue): 1.

$|\text{both boys} \cap \text{at least one B-Tue}| = 7 + 7 - 1 = 13$

$P = \frac{13}{27} \approx \boxed{0.4815}$

---

### Q17. Solution

$P(\text{Pass}) = P(P|S) \cdot P(S) + P(P|S^c) \cdot P(S^c) = 0.9 \times 0.7 + 0.3 \times 0.3 = 0.63 + 0.09 = 0.72$

$P(S|\text{Pass}) = \frac{0.63}{0.72} = \frac{63}{72} = \boxed{\frac{7}{8} = 0.875}$

---

### Q18. Solution

Transfer from A to B, then draw from B.

Case 1: White transferred from A (prob 5/8) → B has 5W, 6B → $P(\text{W from B}) = 5/11$

Case 2: Black transferred from A (prob 3/8) → B has 4W, 7B → $P(\text{W from B}) = 4/11$

$P(\text{W from B}) = \frac{5}{8} \times \frac{5}{11} + \frac{3}{8} \times \frac{4}{11} = \frac{25}{88} + \frac{12}{88} = \boxed{\frac{37}{88}}$

---

### Q19. Solution

Prior: $P(A) = P(B) = P(C) = 1/3$.

If A is pardoned (prob 1/3): guard randomly says B or C with equal probability → $P(\text{says B} | A) = 1/2$

If B is pardoned (prob 1/3): guard cannot say B → $P(\text{says B} | B) = 0$

If C is pardoned (prob 1/3): guard must say B → $P(\text{says B} | C) = 1$

$P(\text{says B}) = \frac{1}{3} \times \frac{1}{2} + \frac{1}{3} \times 0 + \frac{1}{3} \times 1 = \frac{1}{6} + 0 + \frac{1}{3} = \frac{1}{2}$

$P(A | \text{says B}) = \frac{(1/2)(1/3)}{1/2} = \boxed{\frac{1}{3}}$

A's probability hasn't changed — the information doesn't help A.

---

### Q20. Solution

$P(B) = P(B|A)P(A) + P(B|A^c)P(A^c) = 0.6 \times 0.5 + 0.2 \times 0.5 = 0.3 + 0.1 = \boxed{0.4}$

---

### Q21. Solution

$P(\text{recv 1}) = P(\text{recv 1}|\text{sent 0})P(0) + P(\text{recv 1}|\text{sent 1})P(1)$

$= 0.1 \times 0.6 + 0.8 \times 0.4 = 0.06 + 0.32 = 0.38$

$P(\text{sent 1}|\text{recv 1}) = \frac{0.32}{0.38} = \frac{32}{38} = \boxed{\frac{16}{19} \approx 0.842}$

---

### Q22. Solution

By symmetry, each ball is equally likely to be in any position.

$P(\text{3rd ball is red}) = \frac{5}{10} = \boxed{\frac{1}{2}}$

**Key insight:** Without any other information, the probability that any specific position has a red ball equals the proportion of red balls. This is a classic symmetry argument.

---

## Section C: Independence

---

### Q23. Solution

$P(A \cap B) = P(A) \times P(B) = 0.3 \times 0.5 = \boxed{0.15}$

$P(A \cup B) = 0.3 + 0.5 - 0.15 = \boxed{0.65}$

$P(A|B) = P(A) = \boxed{0.3}$ (independent, so conditioning on $B$ doesn't change $A$'s probability)

---

### Q24. Solution

$P(A) = 1/2$ (first die even), $P(B) = 1/2$ (sum even: both even or both odd)

$P(A \cap B) = P(\text{first even AND sum even}) = P(\text{both even}) = (1/2)(1/2) = 1/4$

$P(A) \times P(B) = 1/2 \times 1/2 = 1/4 = P(A \cap B)$ ✓

$\boxed{\text{Yes, A and B are independent.}}$

---

### Q25. Solution

Pairwise independence: Check $P(A \cap B) = P(A)P(B)$:

$P(A \cap B) = 1/4 = (1/2)(1/2) = P(A)P(B)$ ✓ (similarly for other pairs)

Mutual independence requires: $P(A \cap B \cap C) = P(A)P(B)P(C) = (1/2)^3 = 1/8$

But $P(A \cap B \cap C) = 1/4 \neq 1/8$ ✗

$\boxed{\text{Pairwise independent but NOT mutually independent.}}$

---

### Q26. Solution

**Series** (both must work): $P = 0.9 \times 0.9 = \boxed{0.81}$

**Parallel** (at least one works): $P = 1 - P(\text{both fail}) = 1 - 0.1 \times 0.1 = \boxed{0.99}$

---

## Section D: Random Variables, PMF, Expectation

---

### Q27. Solution

$X = |3 - d|$ where $d \in \{1,2,3,4,5,6\}$:

| $d$ | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|
| $X$ | 2 | 1 | 0 | 1 | 2 | 3 |

PMF: $P(X=0) = 1/6$, $P(X=1) = 2/6$, $P(X=2) = 2/6$, $P(X=3) = 1/6$

---

### Q28. Solution

$X \in \{0, 1, 2\}$: $P(X=0) = 1/4$, $P(X=1) = 2/4 = 1/2$, $P(X=2) = 1/4$

$E[X] = 0 \times 1/4 + 1 \times 1/2 + 2 \times 1/4 = 0 + 1/2 + 1/2 = \boxed{1}$

$E[X^2] = 0 + 1/2 + 4/4 = 3/2$

$\text{Var}(X) = 3/2 - 1 = \boxed{1/2}$

---

### Q29. Solution

$\sum P(X=k) = 1$: $c\left(1 + \frac{1}{2} + \frac{1}{3} + \frac{1}{4} + \frac{1}{5}\right) = 1$

$c \times \frac{60 + 30 + 20 + 15 + 12}{60} = c \times \frac{137}{60} = 1 \implies \boxed{c = \frac{60}{137}}$

$E[X] = \sum k \cdot \frac{c}{k} = c \sum_{k=1}^{5} 1 = 5c = \frac{300}{137} \approx \boxed{2.19}$

---

### Q30. Solution (Indicator Variable Method)

Let $X_1 = 1$ if first draw is red, $X_2 = 1$ if second draw is red. $X = X_1 + X_2$.

$E[X_1] = P(\text{1st is red}) = 3/5$

$E[X_2] = P(\text{2nd is red}) = 3/5$ (by symmetry)

$E[X] = E[X_1] + E[X_2] = 3/5 + 3/5 = \boxed{6/5}$

**Shortcut:** Linearity of expectation works even without independence!

---

### Q31. Solution

$E[X] = \sum_{k=1}^{\infty} k \cdot (1/2)^k$

Use the formula $\sum_{k=1}^{\infty} k x^k = \frac{x}{(1-x)^2}$ for $|x| < 1$:

$E[X] = \frac{1/2}{(1/2)^2} = \frac{1/2}{1/4} = \boxed{2}$

---

### Q32. Solution

$\text{Var}(X) = E[X^2] - (E[X])^2 = 30 - 25 = \boxed{5}$

$\text{Var}(2X - 3) = 4 \cdot \text{Var}(X) = 4 \times 5 = \boxed{20}$

---

### Q33. Solution

Let $E$ = expected tosses for 3 consecutive heads.

After each toss:
- T (prob 1/2): restart → contributes $\frac{1}{2}(1 + E)$
- HT (prob 1/4): wasted 2, restart → contributes $\frac{1}{4}(2 + E)$
- HHT (prob 1/8): wasted 3, restart → contributes $\frac{1}{8}(3 + E)$
- HHH (prob 1/8): done → contributes $\frac{1}{8}(3)$

$E = \frac{1}{2}(1+E) + \frac{1}{4}(2+E) + \frac{1}{8}(3+E) + \frac{1}{8}(3)$

$E = \frac{1}{2} + \frac{E}{2} + \frac{1}{2} + \frac{E}{4} + \frac{3}{8} + \frac{E}{8} + \frac{3}{8}$

$E = \frac{1}{2} + \frac{1}{2} + \frac{3}{8} + \frac{3}{8} + E\left(\frac{1}{2} + \frac{1}{4} + \frac{1}{8}\right)$

$E = \frac{7}{4} + \frac{7E}{8}$

$\frac{E}{8} = \frac{7}{4} \implies \boxed{E = 14}$

---

### Q34. Solution

$X \sim \text{Geometric}(1/6)$.

$E[X] = \frac{1}{p} = \frac{1}{1/6} = \boxed{6}$

$\text{Var}(X) = \frac{1-p}{p^2} = \frac{5/6}{1/36} = \boxed{30}$

---

### Q35. Solution (LOTUS)

$X$ uniform on $\{1, 2, 3, 4\}$, each with prob $1/4$.

$E[X^2 + 2X] = E[X^2] + 2E[X]$

$E[X] = (1+2+3+4)/4 = 10/4 = 5/2$

$E[X^2] = (1+4+9+16)/4 = 30/4 = 15/2$

$E[X^2 + 2X] = 15/2 + 5 = \boxed{25/2 = 12.5}$

---

### Q36. Solution

**$P(X = 2)$:** Jump at $x = 2$: $F(2^+) - F(2^-) = (2/4 + 1/4) - (2/4) = 1/4$. So $P(X=2) = \boxed{1/4}$.

**$P(1 < X \leq 2.5)$:** $F(2.5) - F(1) = (2.5/4 + 1/4) - (1/4) = 2.5/4 = \boxed{5/8}$

---

### Q37. Solution

$E[X] = 2$, $E[Y] = 1/2$, $E[XY] = E[X]E[Y] = 1$ (independent → $E[XY] = E[X]E[Y]$)

$\text{Var}(X) = E[X^2] - (E[X])^2 = (1+4+9)/3 - 4 = 14/3 - 4 = 2/3$

$\text{Var}(Y) = E[Y^2] - (E[Y])^2 = (0+1)/2 - 1/4 = 1/2 - 1/4 = 1/4$

$\text{Var}(X + Y) = 2/3 + 1/4 = \boxed{11/12}$ (independent → variances add)

---

### Q38. Solution (Coupon Collector)

Phase 1: Getting 1st new type. Expected draws = 1 (any draw gives a new type).

Phase 2: Getting 2nd new type. Prob of new type = 2/3. Expected draws = 3/2.

Phase 3: Getting 3rd new type. Prob of new type = 1/3. Expected draws = 3.

$E[\text{total}] = 1 + 3/2 + 3 = \frac{2 + 3 + 6}{2} = \boxed{11/2 = 5.5}$

**General formula:** $E = n \sum_{k=1}^{n} \frac{1}{k} = nH_n$ where $H_n$ is the $n$th harmonic number.

---

## Section E: Discrete Distributions

---

### Q39. Solution

$E[X] = np = 10 \times 0.4 = \boxed{4}$

$\text{Var}(X) = np(1-p) = 10 \times 0.4 \times 0.6 = \boxed{2.4}$

$P(X=3) = \binom{10}{3}(0.4)^3(0.6)^7 = 120 \times 0.064 \times 0.0279936 = \boxed{0.2150}$

---

### Q40. Solution

$X \sim \text{Bin}(20, 0.25)$

$P(X \geq 8) = 1 - \sum_{k=0}^{7} \binom{20}{k}(0.25)^k (0.75)^{20-k}$

$E[X] = 5$, so 8 is well above average. This probability would be small (≈ 0.1018).

---

### Q41. Solution

$X \sim \text{Poisson}(2)$

$P(X \leq 1) = P(X=0) + P(X=1) = e^{-2} + 2e^{-2} = 3e^{-2} = \boxed{3/e^2 \approx 0.4060}$

---

### Q42. Solution

$P(X=0) = P(X=1)$: $e^{-\lambda} = \lambda e^{-\lambda} \implies \lambda = 1$

$P(X=2) = \frac{e^{-1} \cdot 1}{2!} = \frac{1}{2e} = \boxed{0.1839}$

---

### Q43. Solution

Rate = 4/min = 2 per 30 seconds. So $X \sim \text{Poisson}(2)$.

$P(X = 0) = e^{-2} \approx \boxed{0.1353}$

---

### Q44. Solution

$P(X = 4) = (2/3)^3 \times (1/3) = \frac{8}{81} \approx \boxed{0.0988}$

$E[X] = 1/p = \boxed{3}$

$P(X > 3) = (1 - 1/3)^3 = (2/3)^3 = \boxed{8/27}$

---

### Q45. Solution

$P(X > n) = (1-p)^n$ (all first $n$ trials are failures).

$P(X > m+n | X > m) = \frac{P(X > m+n)}{P(X > m)} = \frac{(1-p)^{m+n}}{(1-p)^m} = (1-p)^n = P(X > n)$ ∎

---

### Q46. Solution

$P(N \text{ is even}) = P(N=2) + P(N=4) + P(N=6) + \cdots$

$= \sum_{k=1}^{\infty} (1-p)^{2k-1} p = p(1-p) \sum_{k=0}^{\infty} [(1-p)^2]^k = \frac{p(1-p)}{1-(1-p)^2}$

Let $q = 1-p$:

$= \frac{pq}{1-q^2} = \frac{pq}{(1+q)(1-q)} = \frac{pq}{(1+q)p} = \boxed{\frac{q}{1+q} = \frac{1-p}{2-p}}$

---

### Q47. Solution

$P(\text{first success on 4th OR 5th trial})$:

$P(X=4) + P(X=5) = (0.4)^3(0.6) + (0.4)^4(0.6)$

$= 0.6 \times 0.064 + 0.6 \times 0.0256 = 0.0384 + 0.01536 = \boxed{0.05376}$

---

## Section F: Continuous Distributions

---

### Q48. Solution

$E[X] = (2+8)/2 = \boxed{5}$

$\text{Var}(X) = (8-2)^2/12 = 36/12 = \boxed{3}$

$P(3 < X < 6) = \frac{6-3}{8-2} = \frac{3}{6} = \boxed{1/2}$

---

### Q49. Solution

$\int_0^3 cx^2\, dx = c \cdot \frac{27}{3} = 9c = 1 \implies \boxed{c = 1/9}$

$E[X] = \int_0^3 x \cdot \frac{x^2}{9}\, dx = \frac{1}{9} \cdot \frac{x^4}{4}\Big|_0^3 = \frac{1}{9} \cdot \frac{81}{4} = \boxed{9/4 = 2.25}$

$P(X > 2) = \int_2^3 \frac{x^2}{9}\, dx = \frac{1}{9} \cdot \frac{x^3}{3}\Big|_2^3 = \frac{1}{27}(27-8) = \boxed{19/27}$

---

### Q50. Solution

$X \sim N(50, 100)$, $\sigma = 10$.

$P(40 < X < 65) = P\left(\frac{40-50}{10} < Z < \frac{65-50}{10}\right) = P(-1 < Z < 1.5)$

$= \Phi(1.5) - \Phi(-1) = \Phi(1.5) - (1 - \Phi(1)) = 0.9332 - 0.1587 = \boxed{0.7745}$

---

### Q51. Solution

Mean = 500, so $\lambda = 1/500$.

$P(400 < X < 600) = F(600) - F(400) = [1 - e^{-600/500}] - [1 - e^{-400/500}]$

$= e^{-0.8} - e^{-1.2} = 0.4493 - 0.3012 = \boxed{0.1481}$

---

### Q52. Solution

Median $m$: $F(m) = 1/2$

$1 - e^{-\lambda m} = 1/2$

$e^{-\lambda m} = 1/2$

$-\lambda m = -\ln 2$

$m = \boxed{\frac{\ln 2}{\lambda}}$ ∎

---

### Q53. Solution

$P(X > 10) = 0.1587 = 1 - \Phi(1)$, so $\frac{10 - \mu}{\sigma} = 1$ → $10 - \mu = \sigma$ ... (i)

$P(X > 20) = 0.0228 = 1 - \Phi(2)$, so $\frac{20 - \mu}{\sigma} = 2$ → $20 - \mu = 2\sigma$ ... (ii)

From (ii) − (i): $10 = \sigma \implies \boxed{\sigma = 10}$

From (i): $\mu = 10 - 10 = \boxed{0}$

Hmm wait, let's verify: $\mu = 0, \sigma = 10$.
- $P(X > 10) = P(Z > 1) = 0.1587$ ✓
- $P(X > 20) = P(Z > 2) = 0.0228$ ✓ 

---

### Q54. Solution

$X \sim \text{Exp}(2)$. $E[X] = 1/2$, $\text{Var}(X) = 1/4$.

$E[X^2] = \text{Var}(X) + (E[X])^2 = 1/4 + 1/4 = \boxed{1/2}$

$P(X > 1 | X > 0.5) = \frac{P(X > 1)}{P(X > 0.5)} = \frac{e^{-2}}{e^{-1}} = e^{-1} \approx \boxed{0.3679}$

**Alternatively:** By memoryless property, $P(X > 1 | X > 0.5) = P(X > 0.5) = e^{-1}$.

---

### Q55. Solution

$f(x) = F'(x) = \frac{d}{dx}(1 - e^{-x^2/2}) = x \cdot e^{-x^2/2}$ for $x \geq 0$.

This is a **Rayleigh distribution** with parameter $\sigma = 1$.

It is NOT one of the standard distributions in the GATE syllabus (not exponential, not normal).

---

### Q56. Solution

$X \sim N(0,1)$.

$E[X^2] = \text{Var}(X) + (E[X])^2 = 1 + 0 = \boxed{1}$

$E[|X|] = \int_{-\infty}^{\infty} |x| \cdot \frac{1}{\sqrt{2\pi}} e^{-x^2/2}\, dx = 2 \int_0^{\infty} x \cdot \frac{1}{\sqrt{2\pi}} e^{-x^2/2}\, dx$

$= \frac{2}{\sqrt{2\pi}} \left[-e^{-x^2/2}\right]_0^{\infty} = \frac{2}{\sqrt{2\pi}}(0 - (-1)) = \boxed{\sqrt{\frac{2}{\pi}} \approx 0.7979}$

---

### Q57. Solution

$f(x) = 2e^{-2x}$ for $x > 0$. This is $\text{Exp}(2)$.

$F(x) = 1 - e^{-2x}$ for $x \geq 0$.

$P(1 < X < 3) = F(3) - F(1) = (1 - e^{-6}) - (1 - e^{-2}) = e^{-2} - e^{-6}$

$\approx 0.1353 - 0.0025 = \boxed{0.1328}$

---

## Section G: Joint Distributions, Covariance, Correlation

---

### Q58. Solution

Marginals: $p_X(0) = 0.1 + 0.3 = 0.4$, $p_X(1) = 0.2 + 0.4 = 0.6$

$p_Y(0) = 0.1 + 0.2 = 0.3$, $p_Y(1) = 0.3 + 0.4 = 0.7$

Check independence: $p_X(0) \cdot p_Y(0) = 0.4 \times 0.3 = 0.12 \neq 0.1 = p_{X,Y}(0,0)$

$\boxed{\text{Not independent.}}$

---

### Q59. Solution

$E[X] = 0(0.4) + 1(0.6) = 0.6$

$E[Y] = 0(0.3) + 1(0.7) = 0.7$

$E[XY] = \sum_{x,y} xy \cdot p(x,y) = 0 + 0 + 0 + 1 \times 1 \times 0.4 = 0.4$

$\text{Cov}(X,Y) = E[XY] - E[X]E[Y] = 0.4 - 0.42 = \boxed{-0.02}$

$\text{Var}(X) = 0.6 - 0.36 = 0.24$, $\text{Var}(Y) = 0.7 - 0.49 = 0.21$

$\rho = \frac{-0.02}{\sqrt{0.24 \times 0.21}} = \frac{-0.02}{\sqrt{0.0504}} = \frac{-0.02}{0.2245} = \boxed{-0.0891}$

---

### Q60. Solution

$f_X(x) = \int_0^1 4xy\, dy = 4x \cdot \frac{1}{2} = 2x$ for $0 < x < 1$

$f_Y(y) = \int_0^1 4xy\, dx = 4y \cdot \frac{1}{2} = 2y$ for $0 < y < 1$

$f_X(x) \cdot f_Y(y) = 2x \cdot 2y = 4xy = f(x,y)$ ✓

$\boxed{\text{X and Y are independent.}}$

$E[X] = \int_0^1 x \cdot 2x\, dx = 2/3$. By symmetry, $E[Y] = \boxed{2/3}$.

---

### Q61. Solution

Region: $0 < x < y < 1$.

$f_X(x) = \int_x^1 6x\, dy = 6x(1-x)$ for $0 < x < 1$

$f_Y(y) = \int_0^y 6x\, dx = 6 \cdot \frac{y^2}{2} = 3y^2$ for $0 < y < 1$

$E[X] = \int_0^1 x \cdot 6x(1-x)\, dx = 6\int_0^1 (x^2 - x^3)\, dx = 6\left(\frac{1}{3} - \frac{1}{4}\right) = 6 \cdot \frac{1}{12} = \boxed{1/2}$

$E[Y] = \int_0^1 y \cdot 3y^2\, dy = 3 \cdot \frac{1}{4} = \boxed{3/4}$

---

### Q62. Solution

$X, Y \sim \text{Uniform}(0,1)$, independent.

**Part (a):** $P(X + Y > 1)$

Region: unit square where $x + y > 1$ forms a triangle with area $1/2$.

$P(X + Y > 1) = \boxed{1/2}$

**Part (b):** $E[\max(X,Y)]$

Let $M = \max(X,Y)$. CDF: $F_M(m) = P(M \leq m) = P(X \leq m, Y \leq m) = m^2$ for $0 \leq m \leq 1$.

$f_M(m) = 2m$

$E[M] = \int_0^1 m \cdot 2m\, dm = \frac{2}{3}$

**Or using** $E[M] = \int_0^1 [1 - F_M(m)]\, dm = \int_0^1 (1 - m^2)\, dm = 1 - 1/3 = \boxed{2/3}$

---

### Q63. Solution

$N$ = die roll, $X$ = heads in $N$ coin tosses. $X | N \sim \text{Bin}(N, 1/2)$.

$E[X|N] = N/2$, $\text{Var}(X|N) = N/4$

**Law of total variance:** $\text{Var}(X) = E[\text{Var}(X|N)] + \text{Var}(E[X|N])$

$E[\text{Var}(X|N)] = E[N/4] = E[N]/4 = 3.5/4 = 7/8$

$\text{Var}(E[X|N]) = \text{Var}(N/2) = \text{Var}(N)/4$

$\text{Var}(N) = E[N^2] - (E[N])^2 = \frac{1+4+9+16+25+36}{6} - 12.25 = \frac{91}{6} - 12.25 = 15.1\overline{6} - 12.25 = 2.91\overline{6} = 35/12$

$\text{Var}(E[X|N]) = \frac{35/12}{4} = \frac{35}{48}$

$\text{Var}(X) = \frac{7}{8} + \frac{35}{48} = \frac{42 + 35}{48} = \boxed{\frac{77}{48}}$

---

### Q64. Solution

$\text{Cov}(X+Y, X-Y) = \text{Cov}(X,X) - \text{Cov}(X,Y) + \text{Cov}(Y,X) - \text{Cov}(Y,Y)$

$= \text{Var}(X) - \text{Cov}(X,Y) + \text{Cov}(X,Y) - \text{Var}(Y)$

$= \text{Var}(X) - \text{Var}(Y) = 4 - 9 = \boxed{-5}$

---

### Q65. Solution

$\text{Var}(2X - Y) = 4\text{Var}(X) + \text{Var}(Y) - 2 \cdot 2 \cdot 1 \cdot \text{Cov}(X,Y)$

$= 4(4) + 9 - 4(1) = 16 + 9 - 4 = \boxed{21}$

**Detailed:** $\text{Var}(aX + bY) = a^2\text{Var}(X) + b^2\text{Var}(Y) + 2ab\text{Cov}(X,Y)$

Here $a = 2$, $b = -1$: $= 4(4) + 1(9) + 2(2)(-1)(1) = 16 + 9 - 4 = 21$ ✓

---

### Q66. Solution

$X \sim \text{Uniform}(-1,1)$, $Y = X^2$.

$E[X] = 0$ (symmetric about 0)

$E[Y] = E[X^2] = \int_{-1}^{1} x^2 \cdot \frac{1}{2}\, dx = \frac{1}{2} \cdot \frac{2}{3} = 1/3$

$E[XY] = E[X^3] = \int_{-1}^{1} x^3 \cdot \frac{1}{2}\, dx = 0$ (odd function)

$\text{Cov}(X,Y) = 0 - 0 \times 1/3 = \boxed{0}$, $\rho = \boxed{0}$

But $Y = X^2$ means $Y$ is completely determined by $X$, so they are **NOT independent** despite zero covariance.

---

### Q67. Solution

$\text{Var}(X - 2Y) = \text{Var}(X) + 4\text{Var}(Y) - 2(1)(2)\text{Cov}(X,Y)$

$= \sigma_X^2 + 4\sigma_Y^2 - 4\rho\sigma_X\sigma_Y$

$= 4 + 4(9) - 4(0.6)(2)(3) = 4 + 36 - 14.4 = \boxed{25.6}$

---

## Section H: Central Limit Theorem & Confidence Intervals

---

### Q68. Solution

$\bar{X} \sim N(10, 25/100) = N(10, 0.25)$ by CLT.

$Z = \frac{10.5 - 10}{0.5} = 1.0$

$P(\bar{X} > 10.5) = P(Z > 1) = 1 - 0.8413 = \boxed{0.1587}$

---

### Q69. Solution

Single die: $\mu = 3.5$, $\sigma^2 = 35/12$

For 360 rolls: $E[S] = 360 \times 3.5 = 1260$, $\text{Var}(S) = 360 \times 35/12 = 1050$

$\sigma_S = \sqrt{1050} \approx 32.40$

$Z = \frac{1300 - 1260}{32.40} = \frac{40}{32.40} \approx 1.235$

$P(S > 1300) = P(Z > 1.235) \approx 1 - \Phi(1.24) \approx 1 - 0.8925 = \boxed{0.1075}$

---

### Q70. Solution

$n = 64$, $\bar{x} = 1200$, $\sigma = 200$.

95% CI: $\bar{x} \pm z_{0.025} \cdot \frac{\sigma}{\sqrt{n}} = 1200 \pm 1.96 \times \frac{200}{8} = 1200 \pm 49$

$\boxed{(1151, 1249)}$

---

### Q71. Solution

$n = 25$, $\bar{x} = 82$, $s = 12$, $t_{0.005, 24} = 2.797$

99% CI: $82 \pm 2.797 \times \frac{12}{5} = 82 \pm 2.797 \times 2.4 = 82 \pm 6.713$

$\boxed{(75.287, 88.713)}$

---

### Q72. Solution

$n = \left(\frac{z_{\alpha/2} \cdot \sigma}{E}\right)^2 = \left(\frac{1.96 \times 15}{3}\right)^2 = (9.8)^2 = 96.04$

Round up: $\boxed{n = 97}$

---

### Q73. Solution

$n = 100$, $\bar{x} = 1570$, $\sigma = 120$

90% CI: $1570 \pm 1.645 \times \frac{120}{\sqrt{100}} = 1570 \pm 1.645 \times 12 = 1570 \pm 19.74$

$\boxed{(1550.26, 1589.74)}$

---

## Section I: Hypothesis Testing & Chi-Squared

---

### Q74. Solution

$H_0: \mu = 500$, $H_1: \mu \neq 500$ (two-tailed)

$Z = \frac{490 - 500}{40/\sqrt{50}} = \frac{-10}{5.657} = -1.768$

$|Z| = 1.768$. Critical value at $\alpha = 0.05$: $z_{0.025} = 1.96$

$1.768 < 1.96$, so **fail to reject $H_0$**.

p-value $= 2 \times P(Z < -1.768) \approx 2 \times 0.0385 = 0.077 > 0.05$

$\boxed{\text{Fail to reject } H_0.\text{ Insufficient evidence that } \mu \neq 500.}$

---

### Q75. Solution

$H_0: \mu = 30$, $H_1: \mu > 30$ (right-tailed t-test)

$T = \frac{33 - 30}{8/\sqrt{16}} = \frac{3}{2} = 1.5$

$t_{0.05, 15} = 1.753$

$1.5 < 1.753$, so $\boxed{\text{fail to reject } H_0}$

---

### Q76. Solution

**p-value** is the probability of observing a test statistic as extreme as (or more extreme than) the observed value, assuming $H_0$ is true.

Two-tailed: p-value $= 2 \times P(Z > 1.8) = 2 \times (1 - 0.9641) = 2 \times 0.0359 = \boxed{0.0718}$

$0.0718 > 0.05$, so **fail to reject $H_0$** at $\alpha = 0.05$.

---

### Q77. Solution

$\boxed{\text{(c) There is insufficient evidence to reject } H_0}$

Failing to reject $H_0$ does NOT prove $H_0$ is true — we simply don't have enough evidence against it.

---

### Q78. Solution

Power $= 1 - \beta = 1 - 0.15 = \boxed{0.85}$

False positives (Type I errors from 500 true nulls): $500 \times 0.01 = \boxed{5}$

---

### Q79. Solution

Expected: $60/6 = 10$ each.

$\chi^2 = \frac{(8-10)^2}{10} + \frac{(12-10)^2}{10} + \frac{(10-10)^2}{10} + \frac{(14-10)^2}{10} + \frac{(9-10)^2}{10} + \frac{(7-10)^2}{10}$

$= \frac{4 + 4 + 0 + 16 + 1 + 9}{10} = \frac{34}{10} = 3.4$

$df = 5$, $\chi^2_{0.05, 5} = 11.07$

$3.4 < 11.07 \implies \boxed{\text{Fail to reject } H_0.\text{ Die appears fair.}}$

---

### Q80. Solution

$H_0$: fair coin ($p = 0.5$). Expected: H = 100, T = 100.

$\chi^2 = \frac{(115-100)^2}{100} + \frac{(85-100)^2}{100} = \frac{225}{100} + \frac{225}{100} = 4.5$

$df = 1$. $\chi^2_{0.05, 1} = 3.84$

$4.5 > 3.84 \implies \boxed{\text{Reject } H_0.\text{ Coin is likely biased.}}$

---

### Q81. Solution

Expected values:

$E_{11} = 100 \times 140/200 = 70$, $E_{12} = 100 \times 60/200 = 30$

$E_{21} = 70$, $E_{22} = 30$

$\chi^2 = \frac{(60-70)^2}{70} + \frac{(40-30)^2}{30} + \frac{(80-70)^2}{70} + \frac{(20-30)^2}{30}$

$= \frac{100}{70} + \frac{100}{30} + \frac{100}{70} + \frac{100}{30} = 1.429 + 3.333 + 1.429 + 3.333 = 9.524$

$df = (2-1)(2-1) = 1$. $\chi^2_{0.01, 1} = 6.635$

$9.524 > 6.635 \implies \boxed{\text{Reject } H_0.\text{ Methods and outcome are NOT independent.}}$

---

### Q82. Solution

$X_i \sim N(0, 4)$, so $X_i/2 \sim N(0,1)$.

$\sum_{i=1}^{10} X_i^2/4 = \sum_{i=1}^{10} (X_i/2)^2 \sim \boxed{\chi^2_{10}}$

$P(\chi^2_{10} > 18.31) = \boxed{0.05}$ (by definition of $\chi^2_{0.05, 10}$)

---

## Section J: Mixed/Comprehensive

---

### Q83. Solution

**(a)** Urgent messages arrive as Poisson with rate $= 10 \times 0.3 = 3$/min (thinning property).

In 2 minutes: $\lambda = 6$.

$P(\text{no urgent in 2 min}) = e^{-6} \approx \boxed{0.00248}$

**(b)** Inter-arrival of urgent messages $\sim \text{Exp}(3)$ (rate 3/min).

$E[\text{wait}] = 1/3 \text{ minutes} = \boxed{20 \text{ seconds}}$

---

### Q84. Solution

**(a)** $X - Y \sim N(5-3, 4+9) = N(2, 13)$ (independent → variances add)

$P(X > Y) = P(X - Y > 0) = P\left(Z > \frac{0-2}{\sqrt{13}}\right) = P(Z > -0.5547) = \Phi(0.5547) \approx \boxed{0.7106}$

**(b)** $E[2X - Y + 1] = 2(5) - 3 + 1 = \boxed{8}$

**(c)** $\text{Var}(X - 2Y) = \text{Var}(X) + 4\text{Var}(Y) = 4 + 36 = \boxed{40}$ (independent)

---

### Q85. Solution

**(a)** Accept if $|\bar{X} - 50| < 2$, i.e., $48 < \bar{X} < 52$.

$\sigma_{\bar{X}} = 5/\sqrt{25} = 1$

If $\mu = 50$: $P(48 < \bar{X} < 52) = P(-2 < Z < 2) = 2\Phi(2) - 1 = 0.9544$

$\boxed{P(\text{accept}) = 0.9544}$

(So $\alpha = 1 - 0.9544 = 0.0456$)

**(b)** If true $\mu = 52$:

$P(48 < \bar{X} < 52) = P\left(\frac{48-52}{1} < Z < \frac{52-52}{1}\right) = P(-4 < Z < 0) = \Phi(0) - \Phi(-4) \approx 0.5$

$\boxed{\beta = P(\text{accept when } \mu = 52) \approx 0.5}$

---

> **End of Solutions** — All 85 questions solved with detailed steps.
