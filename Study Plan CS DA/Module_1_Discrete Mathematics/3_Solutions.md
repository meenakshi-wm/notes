# Discrete Mathematics — Detailed Solutions & Shortcuts

## Solutions for All Questions from File 2

---

# SECTION A: PROPOSITIONAL LOGIC

---

### Q1. Solution
$p \rightarrow q$: When $p = F$, the implication is **vacuously true** regardless of $q$.

**Answer: TRUE**

**Shortcut:** $p \rightarrow q$ is FALSE **only** when $p = T$ and $q = F$.

---

### Q2. Solution
Check each:
- (A) $p \rightarrow q$: FALSE when $p=T, q=F$ → Contingency
- (B) $p \vee \neg p$: Always TRUE → **Tautology** ✓
- (C) $p \wedge \neg p$: Always FALSE → Contradiction
- (D) $p \oplus q$: Contingency

**Answer: (B)**

---

### Q3. Solution
$(p \rightarrow q) \wedge (p \rightarrow r)$
$= (\neg p \vee q) \wedge (\neg p \vee r)$
$= \neg p \vee (q \wedge r)$ [Distributive law: factoring $\neg p$]
$= p \rightarrow (q \wedge r)$

**Answer: $p \rightarrow (q \wedge r)$**

**Shortcut:** "If p then q AND if p then r" = "If p then (q and r)"

---

### Q4. Solution
LHS: $p \rightarrow (q \rightarrow r) = p \rightarrow (\neg q \vee r) = \neg p \vee (\neg q \vee r) = \neg p \vee \neg q \vee r$

RHS: $(p \wedge q) \rightarrow r = \neg(p \wedge q) \vee r = \neg p \vee \neg q \vee r$

LHS = RHS ✓

**Answer: Proved. This is called the Exportation Law.**

---

### Q5. Solution [GATE 2012]
$\neg \exists x (\forall y(\alpha) \wedge \forall z(\beta))$
$= \forall x \neg(\forall y(\alpha) \wedge \forall z(\beta))$ [Negate ∃ → ∀]
$= \forall x (\neg \forall y(\alpha) \vee \neg \forall z(\beta))$ [De Morgan's]
$= \forall x (\exists y(\neg \alpha) \vee \exists z(\neg \beta))$ [Negate ∀ → ∃¬]

This matches (A) and (B). Let's check (C):
(C) $\forall x (\forall y(\neg \alpha) \vee \forall z(\neg \beta))$ → This has $\forall y$ instead of $\exists y$, which is **NOT equivalent**.
(D) $\forall x (\neg \forall y(\alpha) \vee \exists z(\neg \beta))$ → This is same as (B) since $\neg \forall z(\beta) = \exists z(\neg \beta)$.

**Answer: (C)** — This is NOT logically equivalent.

---

### Q6. Solution [GATE 2015]
Let $C$ = "corrupt", $E$ = "elected"
- S1: $C \rightarrow \neg E$ (If corrupt, not elected)
- S2: $\neg C \rightarrow E$ (If not corrupt, will be elected)

S2 is the **inverse** of S1 (negate both parts). S1's contrapositive would be $E \rightarrow \neg C$.

S1 does NOT imply S2, and S2 does NOT imply S1. S1 is NOT the contrapositive of S2 (that would be $\neg E \rightarrow C$).

They are logically independent.

**Answer: (D) S1 and S2 are independent**

---

### Q7. Solution [GATE 2017]
We need: $p \wedge q$ using only $\neg$ and $\rightarrow$.

$p \wedge q = \neg(p \rightarrow \neg q)$

Verification: $p \rightarrow \neg q = \neg p \vee \neg q$. So $\neg(\neg p \vee \neg q) = p \wedge q$ ✓

**Answer: $p \wedge q \equiv \neg(p \rightarrow \neg q)$**

---

### Q8. Solution
"You can access only if you pay" = "Access implies Pay" = $A \rightarrow P$

Contrapositive: $\neg P \rightarrow \neg A$ (equivalent)

**Answer: (D) Both (A) and (C)**

---

### Q9. Solution [GATE 2014]
"Not all that glitters is gold" = $\neg(\forall x(\text{glitters}(x) \rightarrow \text{gold}(x)))$
$= \exists x \neg(\text{glitters}(x) \rightarrow \text{gold}(x))$
$= \exists x \neg(\neg \text{glitters}(x) \vee \text{gold}(x))$
$= \exists x (\text{glitters}(x) \wedge \neg \text{gold}(x))$

**Answer: (B)**

---

### Q10. Solution
1. $p \rightarrow q$ (premise)
2. $q \rightarrow r$ (premise)
3. From 1 & 2: $p \rightarrow r$ (Hypothetical Syllogism)
4. $\neg r$ (premise)
5. From 3 & 4: $\neg p$ (Modus Tollens)

**Answer: VALID** — uses Hypothetical Syllogism + Modus Tollens

---

### Q11. Solution [GATE 2019]
$(p \leftrightarrow q) \rightarrow (\neg r \rightarrow p)$

Build truth table for 3 variables (8 rows):

| $p$ | $q$ | $r$ | $p \leftrightarrow q$ | $\neg r$ | $\neg r \rightarrow p$ | Result |
|---|---|---|---|---|---|---|
| T | T | T | T | F | T | T |
| T | T | F | T | T | T | T |
| T | F | T | F | F | T | T |
| T | F | F | F | T | T | T |
| F | T | T | F | F | T | T |
| F | T | F | F | T | F | T |
| F | F | T | T | F | T | T |
| F | F | F | T | T | F | F |

Count of TRUE: 7

**Answer: 7 models**

---

### Q12. Solution
(A) $\forall x P(x) \rightarrow \exists x P(x)$: If everything satisfies P, then something does. **VALID** ✓ (assuming non-empty domain)
(B) $\exists x P(x) \rightarrow \forall x P(x)$: Some → All? **NOT VALID** ✗
(C) $\forall x (P(x) \vee \neg P(x))$: Always true (excluded middle). **VALID** ✓
(D) $\exists x P(x) \vee \exists x \neg P(x)$: At least one satisfies or at least one doesn't. **VALID** ✓ (unless domain is empty)

**Answer: (A), (C), (D) are valid. (B) is NOT valid.**

---

### Q13. Solution [GATE 2018]
$\forall x \exists y (P(x,y) \rightarrow Q(x,y))$

$P(x,y) \rightarrow Q(x,y) = \neg P(x,y) \vee Q(x,y)$

So: $\forall x \exists y (\neg P(x,y) \vee Q(x,y))$

This matches (A).

**Answer: (A)**

---

# SECTION B: SET THEORY & RELATIONS

---

### Q14. Solution
$\mathcal{P}(\emptyset) = \{\emptyset\}$, so $|\mathcal{P}(\emptyset)| = 1$

$\mathcal{P}(\mathcal{P}(\emptyset)) = \mathcal{P}(\{\emptyset\}) = \{\emptyset, \{\emptyset\}\}$, so $|\mathcal{P}(\mathcal{P}(\emptyset))| = 2$

**Answer: 2**

---

### Q15. Solution
Number of relations from $A$ to $B$ = $2^{|A| \times |B|} = 2^{3 \times 4} = 2^{12} = 4096$

**Answer: 4096**

---

### Q16. Solution
Check:
- **Reflexive?** $(1,1), (2,2), (3,3), (4,4)$ all present ✓
- **Symmetric?** $(1,2) \in R$ and $(2,1) \in R$ ✓, no other non-diagonal pairs ✓
- **Transitive?** $(1,2) \in R$ and $(2,1) \in R$, need $(1,1) \in R$ ✓. All other chains check out ✓

Yes, $R$ is an equivalence relation. Equivalence classes: $\{1, 2\}$ and $\{3\}$ and $\{4\}$.

**Answer: Yes, it is an equivalence relation.**

---

### Q17. Solution [GATE 2010]
Number of equivalence relations on 5 elements = Bell number $B_5 = 52$.

**Shortcut:** Remember Bell numbers: 1, 1, 2, 5, 15, 52, 203, ...

**Answer: 52**

---

### Q19. Solution [GATE 2016]
$((p,q), (r,s)) \in R$ iff $ps = qr$, i.e., $p/q = r/s$.

- **Reflexive:** $(p,q) R (p,q)$? $pq = qp$ ✓
- **Symmetric:** If $ps = qr$, then $rq = sp$, so $(r,s) R (p,q)$ ✓
- **Transitive:** If $ps = qr$ and $ru = sv$, need $pv = qu$.
  From $ps = qr$: $p/q = r/s$. From $ru = sv$: $r/s = v/u$. So $p/q = v/u$, giving $pu = qv$ ✓

**Answer: (D) An equivalence relation** (it's the "same ratio" relation)

---

### Q20. Solution
For $|A| = n$: Relations that are both reflexive AND symmetric.

- Diagonal: All $n$ entries forced to be 1 (reflexive)
- Above diagonal: $\binom{n}{2} = n(n-1)/2$ pairs, each independently 0 or 1
- Below diagonal: Determined by above diagonal (symmetric)

Count = $2^{n(n-1)/2}$

**Answer: $2^{n(n-1)/2}$**

---

### Q22. Solution
POSET: $(\{1, 2, 3, 4, 6, 8, 12, 24\}, |)$

(a) **Minimal:** 1 (divides everything). **Maximal:** 24 (divisible by nothing else in the set).
Actually — minimal elements are those with no element below them: 1 is the only minimal.
Maximal elements are those with no element above: 8 and 24? Let's check: 8 divides 24, so 8 is NOT maximal. 24 is maximal. Also check 12: 12 divides 24, not maximal. So only **maximal = {24}**, **minimal = {1}**.

(b) LUB{3,4}: Need smallest element dividing both...wait, we need LUB = smallest upper bound.
Upper bounds of {3,4}: elements divisible by both 3 and 4, i.e., divisible by lcm(3,4)=12. In the set: 12, 24. LUB = 12.
GLB{3,4}: Greatest lower bound. Lower bounds: elements dividing both 3 and 4, i.e., dividing gcd(3,4)=1. GLB = 1.

(c) **Lattice?** Yes — for the divisibility relation, LUB = lcm and GLB = gcd, and both exist for any pair within this set.

**Answer: (a) Maximal: {24}, Minimal: {1}. (b) LUB = 12, GLB = 1. (c) Yes, it's a lattice.**

---

### Q24. Solution [GATE 2017]
Set: $\{2, 3, 5, 6, 10, 15, 30\}$ with divisibility.

Hasse diagram:
- 30 is divisible by 6, 10, 15
- 6 = 2×3, 10 = 2×5, 15 = 3×5
- Bottom: 2, 3, 5 (minimal elements)

Maximal: 30 (only one). Minimal: 2, 3, 5.

**Answer: max = 1, min = 3**

---

# SECTION C: FUNCTIONS

---

### Q25. Solution
One-to-one functions from 3 elements to 5 elements:
$P(5, 3) = 5 \times 4 \times 3 = 60$

**Answer: 60**

---

### Q26. Solution
Onto functions from 4 elements to 2 elements:
Using PIE: $\sum_{k=0}^{2} (-1)^k \binom{2}{k}(2-k)^4$
$= \binom{2}{0} \cdot 2^4 - \binom{2}{1} \cdot 1^4 + \binom{2}{2} \cdot 0^4$
$= 16 - 2 + 0 = 14$

**Shortcut:** Total functions = $2^4 = 16$. Non-onto = those mapping to only 1 element = 2. Onto = 16 - 2 = 14.

**Answer: 14**

---

### Q27. Solution [GATE 2015]
$f(f(x)) = x$ means $f$ is an **involution** (self-inverse permutation).

Every element is either a fixed point or part of a 2-cycle (swap).

For 5 elements: Choose which elements are in 2-cycles. Possible:
- 0 swaps (all fixed): $\binom{5}{0}$ ways = 1
- 1 swap: Choose 2 to swap: $\binom{5}{2} = 10$
- 2 swaps: Choose 4 to form 2 swaps: $\binom{5}{4} \cdot \frac{1}{2!} \cdot \binom{4}{2} = 5 \cdot 3 = 15$

Wait, more carefully: Choose 4 elements, then pair them: $\binom{5}{4} \cdot 3 = 15$. Actually: $\frac{5!}{2^2 \cdot 2! \cdot 1!} = \frac{120}{8} = 15$.

Total: $1 + 10 + 15 = 26$

**Answer: 26**

---

### Q28. Solution
$f: A \rightarrow B$ is bijective. $g \circ f: A \rightarrow C$ is surjective.

Since $f$ is bijective, every element of $B$ is hit by some $a \in A$. Since $g \circ f$ is surjective, every $c \in C$ has some $a$ with $g(f(a)) = c$. Since $f$ is surjective onto $B$, this means every $c$ has some $b \in B$ with $g(b) = c$.

**Answer: $g$ must be surjective (onto).**

---

### Q29. Solution [GATE 2016]
Same as Q27 but for general $n$. The answer is the number of involutions on $\{1, \ldots, n\}$.

The formula: $a(n) = a(n-1) + (n-1) \cdot a(n-2)$, with $a(0) = 1, a(1) = 1$.

| $n$ | $a(n)$ |
|---|---|
| 0 | 1 |
| 1 | 1 |
| 2 | 2 |
| 3 | 4 |
| 4 | 10 |
| 5 | 26 |

**Answer: Given by involution recurrence $a(n) = a(n-1) + (n-1)a(n-2)$**

---

# SECTION D: GROUP THEORY

---

### Q30. Solution
$(\mathbb{Z}_6, +_6) = \{0, 1, 2, 3, 4, 5\}$ under addition mod 6.

- Closure ✓, Associativity ✓, Identity = 0 ✓, Inverses exist ✓ → It's a **group**.
- Is it cyclic? 1 generates all: $1, 2, 3, 4, 5, 0$. Yes, **cyclic**.
- Generators: elements coprime to 6. $\gcd(k, 6) = 1$ for $k = 1, 5$. So generators are $\{1, 5\}$.

**Answer: Yes, it's a cyclic group. Generators: {1, 5}. $\phi(6) = 2$ generators.**

---

### Q31. Solution
Subgroups of $\mathbb{Z}_{12}$: By Lagrange's theorem, order of subgroup divides 12. Divisors of 12: 1, 2, 3, 4, 6, 12.

Since $\mathbb{Z}_{12}$ is cyclic, there is exactly one subgroup for each divisor:
- Order 1: $\{0\}$
- Order 2: $\langle 6 \rangle = \{0, 6\}$
- Order 3: $\langle 4 \rangle = \{0, 4, 8\}$
- Order 4: $\langle 3 \rangle = \{0, 3, 6, 9\}$
- Order 6: $\langle 2 \rangle = \{0, 2, 4, 6, 8, 10\}$
- Order 12: $\mathbb{Z}_{12}$ itself

**Answer: 6 subgroups (one for each divisor of 12)**

---

### Q32. Solution [GATE 2014]
$|G| = 35 = 5 \times 7$. Since 35 = product of two distinct primes, by the classification theorem, $G$ is cyclic.

Subgroups of a cyclic group of order $n$: one for each divisor of $n$.
Divisors of 35: 1, 5, 7, 35 → **4 subgroups**.

**Answer: 4**

---

### Q33. Solution [GATE 2017]
$|G| = 49 = 7^2$. Every group of order $p^2$ (where $p$ is prime) is **abelian**.

However, it's not necessarily cyclic. It could be $\mathbb{Z}_{49}$ (cyclic) or $\mathbb{Z}_7 \times \mathbb{Z}_7$ (abelian but not cyclic).

**Answer: (D) Abelian but not necessarily cyclic**

---

### Q34. Solution
$\mathbb{Z}_{20}^* = \{k : 1 \leq k < 20, \gcd(k, 20) = 1\} = \{1, 3, 7, 9, 11, 13, 17, 19\}$

$|\mathbb{Z}_{20}^*| = \phi(20) = 20(1 - 1/2)(1 - 1/5) = 8$

Order of 3: $3^1 = 3, 3^2 = 9, 3^3 = 27 \equiv 7, 3^4 = 21 \equiv 1 \pmod{20}$

**Answer: Group order = 8, Order of 3 = 4**

---

### Q35. Solution
Let $|G| = p$ (prime). Take any $a \neq e$. By Lagrange's theorem, order of $a$ divides $|G| = p$. Since $p$ is prime, order of $a$ is 1 or $p$. Since $a \neq e$, order $\neq 1$. So order = $p$, meaning $a$ generates $G$. Thus $G$ is cyclic.

**Answer: Proved using Lagrange's theorem.**

---

### Q36. Solution [GATE 2012]
$[G : H] = |G|/|H| = 2$. There are only 2 cosets: $H$ and $G \setminus H$.

For any $g \in G$: $gH$ is either $H$ (if $g \in H$) or $G \setminus H$ (if $g \notin H$). Same for $Hg$. So $gH = Hg$ for all $g$.

**Answer: TRUE** — any subgroup of index 2 is normal.

---

# SECTION E: COMBINATORICS

---

### Q37. Solution
With repetition: $5^4 = 625$

**Answer: 625**

---

### Q38. Solution
Circular permutation: $(8-1)! = 7! = 5040$

**Answer: 5040**

---

### Q39. Solution
Stars and bars with each box having at least 1: Put 1 ball in each box first (4 balls used, 6 remaining).
$\binom{6 + 4 - 1}{4 - 1} = \binom{9}{3} = 84$

**Answer: 84**

---

### Q40. Solution [GATE 2013]
Onto functions from 6 elements to 2 elements.

Total functions = $2^6 = 64$. Functions mapping to only one element = 2 (all to first, all to second).
Onto = $64 - 2 = 62$.

**Answer: 62**

---

### Q41. Solution [GATE 2018]
MISSISSIPPI: M(1), I(4), S(4), P(2). Total = 11 letters.

$$\frac{11!}{1! \cdot 4! \cdot 4! \cdot 2!} = \frac{39916800}{1 \cdot 24 \cdot 24 \cdot 2} = \frac{39916800}{1152} = 34650$$

**Answer: 34650**

---

### Q42. Solution [GATE 2020]
5 non-attacking rooks = one in each row and each column = a permutation of columns.

$5! = 120$

**Answer: 120**

---

### Q43. Solution
$D_5 = 5!(1 - 1 + 1/2! - 1/3! + 1/4! - 1/5!)$
$= 120(1 - 1 + 0.5 - 0.1667 + 0.0417 - 0.00833)$
$= 120(0.3667) = 44$

**Shortcut:** $D_5 = 44$ (memorize small values: 0, 1, 2, 9, 44)

**Answer: 44**

---

### Q44. Solution [GATE 2014]
By PIE: $|A \cup B \cup C|$ where $A$ = div by 3, $B$ = div by 5, $C$ = div by 7.

$|A| = \lfloor 1000/3 \rfloor = 333$
$|B| = \lfloor 1000/5 \rfloor = 200$
$|C| = \lfloor 1000/7 \rfloor = 142$
$|A \cap B| = \lfloor 1000/15 \rfloor = 66$
$|A \cap C| = \lfloor 1000/21 \rfloor = 47$
$|B \cap C| = \lfloor 1000/35 \rfloor = 28$
$|A \cap B \cap C| = \lfloor 1000/105 \rfloor = 9$

$|A \cup B \cup C| = 333 + 200 + 142 - 66 - 47 - 28 + 9 = 543$

**Answer: 543**

---

### Q45. Solution
$a_n = 5a_{n-1} - 6a_{n-2}$. Characteristic equation: $r^2 - 5r + 6 = 0 \Rightarrow (r-2)(r-3) = 0$

Roots: $r_1 = 2, r_2 = 3$ (distinct).

General: $a_n = A \cdot 2^n + B \cdot 3^n$

$a_0 = 1$: $A + B = 1$
$a_1 = 4$: $2A + 3B = 4$

Solving: $B = 2, A = -1$

$a_n = -2^n + 2 \cdot 3^n = 2 \cdot 3^n - 2^n$

**Answer: $a_n = 2 \cdot 3^n - 2^n$**

---

### Q46. Solution [GATE 2015]
$f(n + 2) = f(n+1) + f(n)$ with $f(1) = 2, f(2) = 3$

$f(3) = 5, f(4) = 8, f(5) = 13, f(6) = 21$

**Answer: 21**

---

### Q47. Solution
Total = 120. Divisible by 2, 3, or 5:
$|A| = 60, |B| = 40, |C| = 24$
$|A \cap B| = 20, |A \cap C| = 12, |B \cap C| = 8$
$|A \cap B \cap C| = 4$

$|A \cup B \cup C| = 60 + 40 + 24 - 20 - 12 - 8 + 4 = 88$

Not divisible by any: $120 - 88 = 32$

**Answer: 32** (Also equals $120 \cdot (1-1/2)(1-1/3)(1-1/5) = 120 \cdot 1/2 \cdot 2/3 \cdot 4/5 = 32$, which is $\phi(120)$!)

---

### Q49. Solution [GATE 2019]
Partition $\{1,2,3,4\}$ into exactly 2 non-empty subsets = Stirling number $S(4, 2) = 7$.

Enumerate: $\{\{1\},\{2,3,4\}\}, \{\{2\},\{1,3,4\}\}, \{\{3\},\{1,2,4\}\}, \{\{4\},\{1,2,3\}\}, \{\{1,2\},\{3,4\}\}, \{\{1,3\},\{2,4\}\}, \{\{1,4\},\{2,3\}\}$

**Answer: 7**

---

# SECTION F: GRAPH THEORY

---

### Q50. Solution
Sum of degrees = $2 + 2 + 2 + 3 + 3 + 4 = 16$
Edges = $16/2 = 8$

**Answer: 8 edges**

---

### Q51. Solution
$K_4$: Every vertex has degree 3 (odd). Number of odd-degree vertices = 4 ≠ 0 and ≠ 2.

No Euler circuit (need all even). No Euler path (need exactly 2 odd).

**Answer: Neither Euler circuit nor Euler path exists.**

---

### Q52. Solution
Tree with $n$ vertices has $n - 1$ edges. $20 - 1 = 19$.

**Answer: 19**

---

### Q53. Solution [GATE 2013]
$K_5$: $V = 5, E = 10$. For planar: $E \leq 3V - 6 = 9$. Need to remove $10 - 9 = 1$ edge minimum.

But removing 1 edge doesn't guarantee planarity! We need to check: $K_5$ minus one edge has 9 edges and 5 vertices. This satisfies $E \leq 3V - 6 = 9$ ✓. And this graph is indeed planar (can be verified by drawing).

**Answer: 1 edge**

---

### Q54. Solution [GATE 2014]
$C_5$ is an odd cycle. Chromatic number of odd cycle = 3.

**Answer: 3**

---

### Q55. Solution [GATE 2016]
Euler's formula: $V - E + F = 2$
$12 - 19 + F = 2$
$F = 9$

**Answer: 9 faces**

---

### Q56. Solution [GATE 2018]
Edges: $(1,2), (1,3), (2,3)$ form one component $\{1,2,3\}$.
Edges: $(4,5), (4,6), (5,6)$ form another component $\{4,5,6\}$.

**Answer: 2 connected components**

---

### Q57. Solution [GATE 2017]
Cayley's formula: Number of labeled trees on $n$ vertices = $n^{n-2}$
$4^{4-2} = 4^2 = 16$

**Answer: 16**

---

### Q58. Solution
$V - E + F = 2$
$6 - 10 + F = 2$
$F = 6$

**Answer: 6 faces**

---

### Q60. Solution [GATE 2015]
(A) $K_4$: $E = 6 \leq 3(4) - 6 = 6$ ✓ (planar)
(B) $K_{2,3}$: $E = 6 \leq 3(5) - 6 = 9$ ✓. No triangles: $6 \leq 2(5) - 4 = 6$ ✓ (planar)
(C) $K_{3,3}$: $E = 9$. No triangles: $9 > 2(6) - 4 = 8$ ✗ (**NOT planar**)
(D) $Q_3$: 8 vertices, 12 edges. $12 \leq 3(8) - 6 = 18$ ✓ (planar — can verify)

**Answer: (C) $K_{3,3}$**

---

### Q61. Solution
Petersen graph: $V = 10, E = 15$. Check: $15 > 3(10) - 6 = 24$? No, $15 \leq 24$. So this test is inconclusive.

No triangles in Petersen graph (girth = 5). Check: $15 > 2(10) - 4 = 16$? No, $15 \leq 16$. Still inconclusive.

However, the Petersen graph contains a $K_{3,3}$ subdivision, so it is **NOT planar**.

**Answer: Not planar (contains $K_{3,3}$ minor)**

---

### Q62. Solution [GATE 2020]
$\sum \deg(v) = 10 \times 3 = 30$. Edges = $30/2 = 15$.

**Answer: 15 edges**

---

### Q64. Solution [GATE 2021]
Bipartite graph with at least one edge: Two parts, vertices in same part never adjacent. Color each part one color.

**Answer: 2**

---

# SECTION G: PROOF TECHNIQUES

---

### Q65. Solution
**Base case:** $n = 1$: LHS = 1, RHS = $1(2)/2 = 1$ ✓

**Inductive step:** Assume $1 + 2 + \cdots + k = k(k+1)/2$.

$1 + 2 + \cdots + k + (k+1) = k(k+1)/2 + (k+1) = (k+1)(k/2 + 1) = (k+1)(k+2)/2$ ✓

**Answer: Proved by induction.**

---

### Q66. Solution
$n^3 - n = n(n-1)(n+1)$ = product of 3 consecutive integers.

Among any 3 consecutive integers, one is divisible by 3 and at least one is divisible by 2. So the product is divisible by $6$.

**Answer: Proved using product of consecutive integers property.**

---

# SECTION H: MIXED GATE PYQs

---

### Q69. Solution [GATE 2022]
$R = \{(a,b), (b,c), (c,d), (d,a)\}$ on $\{a,b,c,d\}$.

Transitive closure: Keep adding pairs until transitivity holds.
- $(a,b)(b,c) \Rightarrow (a,c)$
- $(b,c)(c,d) \Rightarrow (b,d)$
- $(c,d)(d,a) \Rightarrow (c,a)$
- $(d,a)(a,b) \Rightarrow (d,b)$
Now with new pairs: $(a,c)(c,d) \Rightarrow (a,d)$, $(b,d)(d,a) \Rightarrow (b,a)$, $(c,a)(a,b) \Rightarrow (c,b)$, $(d,b)(b,c) \Rightarrow (d,c)$
And: $(a,d)(d,a) \Rightarrow (a,a)$, similarly $(b,b), (c,c), (d,d)$

Total: all 16 pairs in $A \times A$!

**Answer: 16**

---

### Q70. Solution [GATE 2023]
$(p \wedge q) \rightarrow (p \vee r) = \neg(p \wedge q) \vee (p \vee r) = \neg p \vee \neg q \vee p \vee r = T$ (since $\neg p \vee p = T$)

This is a **tautology**!

**Answer: 8 (all assignments satisfy it)**

---

### Q71. Solution [GATE 2020]
$a^5 = e$, so order of $a$ is 5 (or a divisor of 5, i.e., 1 or 5).

Order of $a^{-1}$ = order of $a$ = **5**.

(Since $(a^{-1})^5 = (a^5)^{-1} = e^{-1} = e$)

**Answer: 5**

---

### Q72. Solution [GATE 2021]
3 men, 3 women, no two women adjacent.

First place 3 men: $3! = 6$ ways.
This creates 4 gaps: _M_M_M_
Choose 3 of 4 gaps for women: $\binom{4}{3} = 4$
Arrange women: $3! = 6$

Total: $6 \times 4 \times 6 = 144$

**Answer: 144**

---

### Q73. Solution [GATE 2022]
$K_5$: Cayley's formula: $5^{5-2} = 5^3 = 125$

**Answer: 125**

---

### Q77. Solution [GATE 2020]
In $(\{1,2,3,4,6,12\}, |)$: LUB{4,6} = lcm(4,6) = 12.

**Answer: 12**

---

### Q78. Solution
Bell number $B_4 = 15$.

**Answer: 15**

---

### Q79. Solution [GATE 2023]
Hamilton circuits in $K_5$: $(5-1)!/2 = 24/2 = 12$ (divide by 2 for direction).

**Answer: 12**

---

### Q80. Solution [GATE 2024]
$\exists x \forall y (P(x) \rightarrow Q(y))$
$= \exists x \forall y (\neg P(x) \vee Q(y))$
$= \exists x (\neg P(x) \vee \forall y Q(y))$ [$Q(y)$ with $\forall y$ can be pulled]
$= \exists x \neg P(x) \vee \forall y Q(y)$ [null quantification]

Check (A): It says $\exists x P(x) \rightarrow \forall y Q(y) = \neg \exists x P(x) \vee \forall y Q(y) = \forall x \neg P(x) \vee \forall y Q(y)$

This is NOT the same as $\exists x \neg P(x) \vee \forall y Q(y)$ in general.

So (A) is not always equivalent. (C) is $\exists x(\neg P(x)) \vee \forall y Q(y)$ which matches our derivation.

**Answer: (C)**
