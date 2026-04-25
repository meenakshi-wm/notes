# Discrete Mathematics — Short Revision Notes (Last-Day Revision)

## GATE 2027 CS/IT/DA | Quick Reference Sheet

---

## 1. PROPOSITIONAL LOGIC

- **Implication:** $p \rightarrow q$ FALSE **only** when $p=T, q=F$
- $p \rightarrow q \equiv \neg p \vee q \equiv \neg q \rightarrow \neg p$ (contrapositive)
- Converse ($q \rightarrow p$) ≠ Original ($p \rightarrow q$)
- "p only if q" = $p \rightarrow q$
- "p unless q" = $\neg q \rightarrow p$ = $p \vee q$
- "p is necessary for q" = $q \rightarrow p$
- "p is sufficient for q" = $p \rightarrow q$
- **Tautology:** Always T. **Contradiction:** Always F.
- $2^n$ rows in truth table, $2^{2^n}$ possible Boolean functions
- **XOR:** $p \oplus q = (p \vee q) \wedge \neg(p \wedge q) = \neg(p \leftrightarrow q)$

### Key Equivalences
- De Morgan: $\neg(p \wedge q) = \neg p \vee \neg q$, $\neg(p \vee q) = \neg p \wedge \neg q$
- Distribution: $p \wedge (q \vee r) = (p \wedge q) \vee (p \wedge r)$
- Absorption: $p \vee (p \wedge q) = p$
- Exportation: $p \rightarrow (q \rightarrow r) \equiv (p \wedge q) \rightarrow r$

### Rules of Inference
- **Modus Ponens:** $p \rightarrow q, p \vdash q$
- **Modus Tollens:** $p \rightarrow q, \neg q \vdash \neg p$
- **Hypothetical Syllogism:** $p \rightarrow q, q \rightarrow r \vdash p \rightarrow r$
- **Disjunctive Syllogism:** $p \vee q, \neg p \vdash q$
- **Resolution:** $p \vee q, \neg p \vee r \vdash q \vee r$

---

## 2. FIRST ORDER LOGIC

- $\forall$ with → (for "All A are B"): $\forall x(A(x) \rightarrow B(x))$
- $\exists$ with $\wedge$ (for "Some A are B"): $\exists x(A(x) \wedge B(x))$
- **Negation flips quantifier:** $\neg \forall = \exists \neg$, $\neg \exists = \forall \neg$
- $\forall$ distributes over $\wedge$, $\exists$ distributes over $\vee$
- $\forall x \forall y = \forall y \forall x$, $\exists x \exists y = \exists y \exists x$
- But $\forall x \exists y \neq \exists y \forall x$ (latter is stronger)
- $\exists y \forall x P \Rightarrow \forall x \exists y P$ (but not reverse)
- Valid ⟺ true in ALL interpretations
- $\forall x P(x) \rightarrow \exists x P(x)$ is VALID

---

## 3. SET THEORY

- $|A \cup B| = |A| + |B| - |A \cap B|$ (PIE)
- $|\mathcal{P}(A)| = 2^{|A|}$
- $\emptyset \in \mathcal{P}(A)$ always; $\emptyset \subseteq A$ always
- $|A \times B| = |A| \cdot |B|$
- De Morgan: $(A \cup B)' = A' \cap B'$

---

## 4. RELATIONS ⭐⭐

### Properties on set of size $n$
| Property | Condition | Count of such relations |
|---|---|---|
| Reflexive | All $(a,a) \in R$ | $2^{n^2-n}$ |
| Irreflexive | No $(a,a)$ | $2^{n^2-n}$ |
| Symmetric | $(a,b) \in R \Rightarrow (b,a) \in R$ | $2^{n(n+1)/2}$ |
| Antisymmetric | $(a,b),(b,a) \in R \Rightarrow a=b$ | $2^n \cdot 3^{n(n-1)/2}$ |
| Asymmetric | Antisym + Irrefl | $3^{n(n-1)/2}$ |

- **Equivalence Relation** = Reflexive + Symmetric + Transitive
- Equivalence classes = Partition of set
- Number of equiv relations = **Bell number**: 1, 1, 2, 5, 15, 52, 203
- **Partial Order** = Reflexive + Antisymmetric + Transitive

### POSET Special Elements
- **Maximal:** Nothing above it (can be multiple)
- **Maximum:** Above everything (unique, if exists)
- **LUB(S):** Least upper bound. **GLB(S):** Greatest lower bound
- **Lattice:** Every pair has LUB and GLB
- **Total order:** Every pair comparable

### Closures
- Reflexive: add diagonal
- Symmetric: add reverse pairs
- Transitive: $R^+ = R \cup R^2 \cup \cdots \cup R^n$

---

## 5. FUNCTIONS

- Total functions $A \rightarrow B$: $|B|^{|A|}$
- Injections: $P(n,m) = n!/(n-m)!$ (need $m \leq n$)
- Surjections: PIE formula $\sum_{k=0}^n (-1)^k \binom{n}{k}(n-k)^m$
- Bijections: $n!$ (need $|A| = |B| = n$)
- $g \circ f$ injective → $f$ injective
- $g \circ f$ surjective → $g$ surjective
- Involutions ($f \circ f = \text{id}$): $a(n) = a(n-1) + (n-1)a(n-2)$

---

## 6. GROUP THEORY ⭐

- **Group:** Closure + Assoc + Identity + Inverse
- **Abelian:** Group + Commutative
- $(ab)^{-1} = b^{-1}a^{-1}$
- **Lagrange:** $|H|$ divides $|G|$; order of element divides $|G|$
- Group of **prime order** → cyclic
- Group of order $p^2$ → **abelian**
- **Cyclic group** $\mathbb{Z}_n$: generators are $\{k : \gcd(k,n)=1\}$, count = $\phi(n)$
- Cyclic group has exactly one subgroup for each divisor of $n$
- Subgroup of index 2 is always normal
- $(\mathbb{Z}_n^*, \times_n)$: order = $\phi(n)$

---

## 7. COMBINATORICS ⭐⭐

### Counting
- Permutations: $P(n,r) = n!/(n-r)!$
- Combinations: $\binom{n}{r} = n!/(r!(n-r)!)$
- With repetition (multiset): $\binom{n}{r} = \binom{n}{n-r}$
- Circular: $(n-1)!$; With reflection: $(n-1)!/2$
- Multinomial: $n!/(n_1! n_2! \cdots n_k!)$

### Stars and Bars
- $n$ identical → $r$ distinct boxes (≥0 each): $\binom{n+r-1}{r-1}$
- $n$ identical → $r$ distinct boxes (≥1 each): $\binom{n-1}{r-1}$

### Important Formulas
- **Derangements:** $D_n = n!\sum_{k=0}^n (-1)^k/k!$
  - $D_1=0, D_2=1, D_3=2, D_4=9, D_5=44$
  - Recurrence: $D_n = (n-1)(D_{n-1} + D_{n-2})$
- **PIE:** $|A_1 \cup \cdots \cup A_n| = \sum|A_i| - \sum|A_i \cap A_j| + \cdots$
- **Pigeonhole:** $n$ items in $m$ boxes → some box has $\geq \lceil n/m \rceil$

### Recurrences
- Linear homogeneous: $a_n = c_1 a_{n-1} + c_2 a_{n-2}$
- Characteristic eq: $r^2 = c_1 r + c_2$
- Distinct roots $r_1, r_2$: $a_n = Ar_1^n + Br_2^n$
- Repeated root $r$: $a_n = (A + Bn)r^n$

---

## 8. GRAPH THEORY ⭐⭐⭐

### Basics
- **Handshaking:** $\sum \deg(v) = 2|E|$
- Odd-degree vertices: always **even** count
- $K_n$: $\binom{n}{2}$ edges, each vertex degree $n-1$
- $K_{m,n}$: $mn$ edges
- Tree: $|E| = |V| - 1$, connected, acyclic

### Euler
- **Euler CIRCUIT:** All vertices even degree + connected
- **Euler PATH:** Exactly 2 odd-degree vertices + connected

### Hamiltonian
- **Dirac:** $\deg(v) \geq n/2$ for all $v$ → Hamiltonian circuit exists
- Ore: $\deg(u) + \deg(v) \geq n$ for all non-adjacent $u,v$ → Hamiltonian

### Planarity
- **Euler's formula:** $V - E + F = 2$ (connected planar graph)
- $E \leq 3V - 6$ (simple, $V \geq 3$)
- No triangles: $E \leq 2V - 4$
- $K_5$ and $K_{3,3}$ are **NOT** planar
- **Four Color Theorem:** Every planar graph: $\chi \leq 4$

### Coloring
- $\chi(K_n) = n$
- $\chi(\text{bipartite}) = 2$
- $\chi(C_{\text{odd}}) = 3$, $\chi(C_{\text{even}}) = 2$
- $\chi(\text{tree}) = 2$ (if ≥ 2 vertices)
- Brooks: $\chi \leq \Delta$ unless complete or odd cycle

### Trees
- Labeled trees on $n$ vertices: $n^{n-2}$ (Cayley's formula)
- Spanning trees of $K_n$: $n^{n-2}$

### Matching
- **Hall's theorem:** Complete matching exists in bipartite graph iff $|N(S)| \geq |S|$ for all $S$

---

## 9. PROOF TECHNIQUES

- **Direct:** Assume $p$, derive $q$
- **Contrapositive:** Prove $\neg q \rightarrow \neg p$
- **Contradiction:** Assume $\neg p$, reach contradiction
- **Induction:** Base + Assume $P(k)$ → Prove $P(k+1)$
- **Strong Induction:** Assume $P(1), \ldots, P(k)$ → Prove $P(k+1)$
- **Pigeonhole:** Use for existence proofs

---

## QUICK NUMBERS TO MEMORIZE

| Bell numbers | $B_0=1, B_1=1, B_2=2, B_3=5, B_4=15, B_5=52$ |
|---|---|
| Derangements | $D_1=0, D_2=1, D_3=2, D_4=9, D_5=44$ |
| Catalan numbers | $C_0=1, C_1=1, C_2=2, C_3=5, C_4=14, C_5=42$ |
| Euler's totient | $\phi(p)=p-1$, $\phi(p^k)=p^{k-1}(p-1)$, $\phi$ is multiplicative |
| Cayley's formula | Labeled trees on $n$: $n^{n-2}$ |


## J2: Ultra-Important Formulas Quick Reference Card

| Formula | Value/Expression |
|---|---|
| Handshaking: $\sum \deg(v)$ | $= 2|E|$ |
| $K_n$ edges | $\binom{n}{2}$ |
| Tree edges | $n - 1$ |
| Labeled trees on $n$ vertices | $n^{n-2}$ |
| Euler's formula (connected planar) | $V - E + F = 2$ |
| Planar: $E \leq$ | $3V - 6$ (general), $2V - 4$ (no triangles) |
| $\chi(K_n)$ | $n$ |
| $\chi(\text{bipartite})$ | 2 |
| Power set $|\mathcal{P}(A)|$ | $2^n$ |
| Relations on $n$-set | $2^{n^2}$ |
| Equivalence relations on $n$-set | $B_n$ (Bell number) |
| Functions $A \to B$ | $|B|^{|A|}$ |
| Injections | $\frac{|B|!}{(|B|-|A|)!}$ |
| Derangements $D_n$ | $n!\sum_{k=0}^n \frac{(-1)^k}{k!}$ |
| Catalan $C_n$ | $\frac{1}{n+1}\binom{2n}{n}$ |
| Euler's totient $\phi(n)$ | $n\prod_{p|n}(1-\frac{1}{p})$ |
| Euler's theorem | $a^{\phi(n)} \equiv 1 \pmod{n}$ |
| Generators of $\mathbb{Z}_n$ | $\phi(n)$ |
| Chromatic poly of tree | $k(k-1)^{n-1}$ |
| Chromatic poly of $C_n$ | $(k-1)^n + (-1)^n(k-1)$ |
| Stars and bars | $\binom{n+r-1}{r-1}$ |
| PIE for 2 sets | $|A \cup B| = |A|+|B|-|A \cap B|$ |
| GF of $1/(1-x)$ | $\sum x^n = 1, 1, 1, \ldots$ |
| GF of Fibonacci | $x/(1-x-x^2)$ |

### Additional Formulas (Extended)

| Formula | Value/Expression |
|---|---|
| Compositions of $n$ (total) | $2^{n-1}$ |
| Compositions of $n$ into $k$ parts | $\binom{n-1}{k-1}$ |
| Vandermonde's identity | $\sum_k \binom{m}{k}\binom{n}{r-k} = \binom{m+n}{r}$ |
| Hockey stick | $\sum_{i=r}^{n} \binom{i}{r} = \binom{n+1}{r+1}$ |
| Fibonacci closed form | $F_n = (\phi^n - \hat{\phi}^n)/\sqrt{5}$ |
| Perfect matchings in $K_{2n}$ | $(2n-1)!! = (2n-1)(2n-3) \cdots 1$ |
| Floor/ceiling of negative | $\lfloor -x \rfloor = -\lceil x \rceil$ |
| Ramsey $R(3,3)$ | 6 |
| Max edges in triangle-free graph | $\lfloor n^2/4 \rfloor$ |
| $k$-regular graph edges | $nk/2$ |
| Hypercube $Q_n$ vertices | $2^n$ |
| Hypercube $Q_n$ edges | $n \cdot 2^{n-1}$ |
| Self-dual functions on $n$ vars | $2^{2^{n-1}}$ |
| Affine (linear) functions on $n$ vars | $2^{n+1}$ |
| Antisymmetric relations | $2^n \cdot 3^{n(n-1)/2}$ |
| Outerplanar edge bound | $E \leq 2V - 3$ |
| Tournaments on $n$ vertices | $2^{\binom{n}{2}}$ |
