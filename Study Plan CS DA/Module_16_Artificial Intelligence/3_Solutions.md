# Artificial Intelligence — Solutions for GATE 2027

> Complete solutions for all 65 questions.

---

## Section A: Propositional Logic

### Q1. (p∧(p→q))→q tautology?
p→q = ¬p∨q. Premise: p∧(¬p∨q) = (p∧¬p)∨(p∧q) = F∨(p∧q) = p∧q.
So formula = (p∧q)→q. When is this false? p∧q=T but q=F → impossible.
**Tautology ✓ (This is Modus Ponens)**

---

### Q2. Contrapositive, converse, inverse
Original: Rain → WetGround
- **Contrapositive:** ¬WetGround → ¬Rain (logically equivalent)
- **Converse:** WetGround → Rain (NOT equivalent)
- **Inverse:** ¬Rain → ¬WetGround (NOT equivalent)

---

### Q3. Convert (p→q)∧(r→s) to CNF
p→q = ¬p∨q. r→s = ¬r∨s.
**CNF: (¬p∨q)∧(¬r∨s)** (already in CNF)

---

### Q4. Resolution proof: {p∨q, ¬p∨r, ¬q∨r} ⊨ r
Negate conclusion: ¬r. Add to clause set.
1. p∨q (given)
2. ¬p∨r (given)
3. ¬q∨r (given)
4. ¬r (negated conclusion)
5. ¬p (resolve 2,4 on r)
6. ¬q (resolve 3,4 on r)
7. q (resolve 1,5 on p)
8. ⊥ (resolve 6,7 on q — contradiction!)

**r is proven. ✓**

---

### Q5. Truth table rows for 5 variables
**2⁵ = 32 rows**

---

### Q6. Simplify ¬(p→q)
¬(¬p∨q) = p∧¬q (by De Morgan's)
**¬(p→q) ≡ p∧¬q** ("p is true and q is false")

---

### Q7. Modus Tollens validity
"If study → pass. ¬pass. Therefore ¬study."
This is **Modus Tollens**: p→q, ¬q ⊢ ¬p. **Valid. ✓**

---

### Q8. ¬(p∧q)→(r∨s) to CNF
= (p∧q)∨(r∨s) [since A→B = ¬A∨B, and ¬¬(p∧q) = p∧q]
Wait: ¬(p∧q)→(r∨s) = ¬(¬(p∧q))∨(r∨s) = (p∧q)∨(r∨s)
= (p∨r∨s)∧(q∨r∨s) [distribute ∨ over ∧]
**CNF: (p∨r∨s)∧(q∨r∨s)**

---

### Q9. p→(q→r) ≡ (p∧q)→r
LHS = p→(¬q∨r) = ¬p∨¬q∨r
RHS = ¬(p∧q)∨r = ¬p∨¬q∨r
**Same! ✓ (This is the Export law)**

---

### Q10. Satisfying assignments for p↔q
p↔q is true when p=q: (T,T) and (F,F).
**2 satisfying assignments**

---

## Section B: Predicate Logic

### Q11. "Every student who studies hard passes"
**∀x ((Student(x) ∧ StudiesHard(x)) → Passes(x))**

---

### Q12. "There exists a person who likes everyone"
**∃x ∀y Likes(x,y)**

---

### Q13. Negate ∀x ∃y Loves(x,y)
**∃x ∀y ¬Loves(x,y)** ("There exists someone who loves nobody")

---

### Q14. Free and bound variables
∀x(P(x,y) → ∃y Q(x,y))
- x: bound by ∀x
- First y in P(x,y): **free** (not in scope of ∃y)
- Second y in Q(x,y): **bound** by ∃y
- Note: the ∃y only scopes over Q(x,y), not P(x,y)

---

### Q15. "No one is both programmer and manager"
**¬∃x (Programmer(x) ∧ Manager(x))** ≡ **∀x (Programmer(x) → ¬Manager(x))**

---

### Q16. Unify P(x, f(y)) and P(g(z), f(h(z)))
x = g(z), y = h(z)
**θ = {x/g(z), y/h(z)}**

---

### Q17. ∀x P(x) → ∃x P(x) valid?
If domain is non-empty: ∀x P(x) means P holds for all → certainly ∃x P(x).
If domain is empty: ∀x P(x) is vacuously true, but ∃x P(x) is false → implication is F.

In standard FOL (non-empty domains): **Valid. ✓**
With possibly empty domains: **Not valid.**

---

### Q18. "For every real number, there is a greater one"
**∀x ∃y (Real(x) ∧ Real(y) ∧ y > x)**
Or more concisely with implicit domain: **∀x ∃y (y > x)**

---

### Q19. Negation of "Everyone passed or someone failed"
Original: ∀x Passed(x) ∨ ∃x Failed(x)
Negation: ¬(∀x Passed(x) ∨ ∃x Failed(x)) = ¬∀x Passed(x) ∧ ¬∃x Failed(x)
= ∃x ¬Passed(x) ∧ ∀x ¬Failed(x)
= "Someone didn't pass AND no one failed"

**Answer: (c) or closest match — "Not everyone passed and no one failed"**

---

### Q20. Prenex normal form: ∀x P(x) → ∃x Q(x)
= ¬∀x P(x) ∨ ∃x Q(x)
= ∃x ¬P(x) ∨ ∃y Q(y) [rename bound variable to avoid clash]
= **∃x ∃y (¬P(x) ∨ Q(y))**

---

## Section C: Uninformed Search

### Q21. BFS vs DFS comparison
| | BFS | DFS |
|--|-----|-----|
| Complete | Yes | No (infinite spaces) |
| Optimal | Yes (uniform cost) | No |
| Time | O(b^d) | O(b^m) |
| Space | O(b^d) | O(bm) |

---

### Q22. BFS nodes for b=3, d=4
$\sum_{i=0}^{4} 3^i = 1+3+9+27+81 = **121** nodes (worst case)

---

### Q23. IDS over BFS
IDS has **O(bd)** space (like DFS) while being complete and optimal (like BFS). BFS requires O(b^d) space which is prohibitive for large d.

---

### Q24. Space complexity of IDS with b=4, d=6
**O(bd) = O(4×6) = O(24)** — linear in depth!

---

### Q25. UCS expansion order
UCS expands by lowest g(n). B(3) first, then A(5), then C(7).
**Order: B, A, C**

---

### Q26. DFS optimal?
**No** in general. Only optimal if: all paths to goal have equal cost AND the goal is at the shallowest depth of DFS exploration. Practically, never rely on DFS for optimality.

---

### Q27. IDS total nodes
$\sum_{i=0}^{d}(d-i+1)b^i \approx \frac{b}{b-1}b^d$ for large b.
For b=10, d=5: BFS generates ~111,111, IDS generates ~123,456. Only ~11% overhead.

---

### Q28. Expansion order for b=2, depth 3
```
       1
      / \
     2   3
    / \ / \
   4  5 6  7
  /\ /\ /\ /\
 8 9...
```
BFS: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15
DFS: 1, 2, 4, 8, 9, 5, 10, 11, 3, 6, 12, 13, 7, 14, 15
IDS: d=0: 1. d=1: 1,2,3. d=2: 1,2,4,5,3,6,7. d=3: 1,2,4,8,9,5,10,11,3,6,12,13,7,14,15

---

### Q29. UCS vs Dijkstra
UCS IS Dijkstra's algorithm applied to search problems with graph search (avoiding revisiting nodes). Both expand by minimum cumulative cost g(n).

---

### Q30. Making DFS complete with cycles
Use **graph search** (maintain a visited/closed set). Never expand an already-visited node. This prevents infinite loops.

---

## Section D: Informed Search

### Q31. Admissible and consistent
**Admissible:** h(n) ≤ h*(n) — never overestimates.
Example: h=0 (trivially admissible), Manhattan distance for grid problems.

**Consistent:** h(n) ≤ c(n,n') + h(n') — satisfies triangle inequality.
Example: Manhattan distance for 8-puzzle.

---

### Q32. A* trace
S: f=0+7=7. Expand → A(f=2+5=7), B(f=5+3=8)
A: f=7. Expand → C(f=2+3+2=7)
C (via A): f=7. Expand → G(f=5+4+0=9)
B: f=8. Expand → C(f=5+1+2=8) [already visited with lower cost, skip in graph search]
G: f=9. **Goal! Path: S→A→C→G, cost = 9**

---

### Q33. h(n) = 0 admissible?
**Yes** (always underestimates — 0 ≤ actual cost). A* with h=0 becomes **UCS** (Uniform Cost Search).

---

### Q34. max(h₁, h₂) consistent?
**Yes.** If h₁ and h₂ are both consistent, then max(h₁, h₂) is also consistent. And it's a **better (more informed) heuristic** — A* expands fewer nodes.

---

### Q35. Can A* expand node with f(n) > f*?
**No.** With an admissible heuristic, A* never expands a node with f(n) > C* (optimal cost). This is a key property: all expanded nodes satisfy f(n) ≤ C*.

---

### Q36. Greedy vs A*
| | Greedy | A* |
|--|--------|-----|
| Evaluates | h(n) only | g(n)+h(n) |
| Complete | No | Yes |
| Optimal | No | Yes (admissible h) |
| Speed | Often faster | Slower but optimal |

---

### Q37. Hill climbing limitations
- **Local maximum:** All neighbors worse, but not global maximum
- **Plateau:** Flat area, no direction to improve
- **Ridge:** Narrow, hard to navigate

**Random restart:** Run hill climbing from random starting points multiple times. If probability of finding global max per run is p, expected runs = 1/p.

---

### Q38. h₁ vs h₂ for 8-puzzle
h₂ (Manhattan) ≥ h₁ (misplaced tiles) for all states.
**h₂ is more informed** → A* with h₂ expands fewer nodes. h₂ dominates h₁.

---

### Q39. IDA*
Iterative Deepening A*: uses f-value limit instead of depth limit.
- Space: O(bd) like IDS
- Preferred over A* when memory is limited
- Optimal and complete with admissible h

---

### Q40. h(n) > h*(n) for some n
If h is not admissible, A* is **NOT guaranteed** to find optimal solution. It may expand the goal via a suboptimal path.

---

## Section E: Adversarial Search

### Q41. Minimax
Two-player zero-sum game. MAX tries to maximize utility, MIN tries to minimize.
Recursively evaluate: MAX nodes take max of children, MIN nodes take min.
Returns optimal strategy assuming both play perfectly.

---

### Q42. Minimax value
```
        MAX
       / | \
    MIN MIN MIN
    /\  /\  /\
   3 5 2 8 1 7
```
Left MIN: min(3,5)=3. Center MIN: min(2,8)=2. Right MIN: min(1,7)=1.
MAX: max(3,2,1) = **3**

---

### Q43. Alpha-beta pruning
Left branch: MIN returns 3. α at MAX = 3.
Center branch: First child = 2. MIN guarantees ≤2. But α=3 > 2, so **prune** (don't look at 8). β cutoff!

Wait: at center MIN, first child=2, so β=2. Since α(3) ≥ β(2), prune. Node 8 is pruned.

Right branch: First child = 1. MIN has β=1. α(3) ≥ β(1), **prune**. Node 7 is pruned.

**Pruned: 8 and 7. Minimax value still 3.**

---

### Q44. Best-case alpha-beta
With perfect ordering: **O(b^(m/2))**. This effectively doubles the search depth for the same computation.

---

### Q45. Does alpha-beta change minimax value?
**No.** Alpha-beta only prunes branches that cannot affect the final result. The minimax value is always preserved.

---

### Q46. Leaf nodes evaluated
Minimax: $b^m = 3^4 = **81** leaf nodes.
Perfect alpha-beta: $O(b^{m/2}) = 3^2 = **9** in best case.
More precisely: $2b^{m/2} - 1$ for even m. So $2(9)-1 = 17$ approximately.

---

### Q47. Alpha-beta on given tree
```
        MAX (α=−∞, β=+∞)
       /    \
     MIN    MIN
    / \    / \
   5   3  8   2
```
Left MIN: child=5, β=5. child=3, β=3. Returns 3. Update MAX: α=3.
Right MIN: child=8, β=8. child=2, β=2. Since α(3) > β(2)? No, α=3 ≮ β after min gets 8 first. 

Actually: Right MIN starts with α=3:
- child=8: β = min(+∞, 8) = 8. α(3) < β(8), continue.
- child=2: β = min(8, 2) = 2. α(3) ≥ β(2)? Yes! But we already evaluated both children.

Actually, the pruning check happens after evaluating each child. After child=2: β=2 ≤ α=3, so we prune further children (but there are none).

**No pruning occurs** in this small tree (only 2 children per MIN, both evaluated before cutoff condition triggers at the boundary).

MAX: max(3, min(8,2)=2) = **3**

---

### Q48. Horizon effect
A program with limited search depth may miss a critical event just beyond its horizon. For example, a losing position might be hidden by a sequence of delaying moves that pushes the loss beyond the search depth, making it appear favorable.

---

### Q49. Minimax for non-zero-sum?
**No.** Minimax assumes zero-sum (one player's gain = other's loss). In non-zero-sum games, players may both benefit from cooperation. Need different frameworks (Nash equilibrium, etc.).

---

### Q50. Move ordering effect
Good ordering (best moves first) → more cutoffs → fewer nodes evaluated → approaches O(b^(m/2)).
Bad ordering → minimal pruning → approaches O(b^m) (no benefit).
Techniques: killer moves, history heuristic, iterative deepening.

---

## Section F: Bayesian Networks

### Q51. P(B|A) using Bayes
P(B|A) = P(A|B)P(B)/P(A) = 0.5 × 0.6 / 0.3 = **1.0**

(This means whenever A occurs, B has certainly occurred.)

---

### Q52. Joint probability expression
Network: A→C, B→C, C→D
**P(A,B,C,D) = P(A)P(B)P(C|A,B)P(D|C)**

---

### Q53. A→B→C: A⊥C|B?
**Yes.** This is a chain. B blocks the path from A to C when conditioned on. A ⊥ C | B ✓

---

### Q54. A→C←B (collider)
A and B are **independent** (no path unless through C).
Given C: A and B become **dependent** (explaining away). A ⊥̸ B | C.

---

### Q55. Parameters
Full joint (4 Boolean): $2^4 - 1 = **15** parameters.
Network A→B→C→D: P(A):1 + P(B|A):2 + P(C|B):2 + P(D|C):2 = **7 parameters** (huge savings!)

---

### Q56. Markov blanket of B
Network: A→B, B→C, B→D, E→C
MB(B) = Parents(B) ∪ Children(B) ∪ Parents of Children(B)
= {A} ∪ {C, D} ∪ {E} (E is parent of C, a child of B)
**MB(B) = {A, C, D, E}**

---

### Q57. D-separation in A→B→C←D
A⊥D? Path A→B→C←D: C is a collider. Not conditioning on C or descendants.
**A ⊥ D ✓** (collider blocks)

A⊥D|C? Now conditioning on C (the collider). Path is now **active**.
**A ⊥̸ D | C** (explaining away)

---

### Q58. Disease|TestPos (Bayes)
P(D|T+) = P(T+|D)P(D) / P(T+)
P(T+) = P(T+|D)P(D) + P(T+|¬D)P(¬D) = 0.95(0.01) + 0.05(0.99) = 0.0095 + 0.0495 = 0.059
P(D|T+) = 0.0095/0.059 = **0.161 (about 16.1%)**

Despite 95% test accuracy, positive result only means ~16% chance of disease (due to low prior).

---

### Q59. Flu-Cold dependence via MissWork
Flu→Fever, Cold→Fever, Fever→MissWork.
Fever is a collider for Flu and Cold. MissWork is a descendant of Fever.
Observing MissWork=T → activates the path Flu→Fever←Cold.
**Yes, Flu and Cold become dependent** given MissWork (explaining away: if MissWork is explained by Flu, Cold becomes less likely, and vice versa).

---

### Q60. CPT parameters for node with 3 Boolean parents
$2^3 = 8$ parent configurations. Each config has P(node=T|...) as one free parameter.
**8 free parameters.**

---

## Section G: Probabilistic Inference

### Q61. Enumeration example
To compute P(X₁|X₃=true), sum over hidden variable X₂:
P(X₁|X₃=T) = α Σ_{X₂} P(X₁)P(X₂|X₁)P(X₃=T|X₂)

Compute for X₁=T and X₁=F, then normalize (α = 1/sum).

---

### Q62. Variable elimination advantage
VE avoids **redundant computation** by computing and caching intermediate factors. Enumeration recomputes the same sub-expressions multiple times. VE's complexity depends on the **treewidth** of the network, not the total number of variables.

---

### Q63. Likelihood weighting
1. Fix evidence variables to observed values
2. Sample non-evidence variables top-down using CPTs
3. Weight each sample by $\prod P(e_i | parents(e_i))$ (likelihood of evidence given sampled parents)
4. Estimate P(query|evidence) by weighted average of samples

---

### Q64. Optimal elimination for P(X₁|X₄)
Chain: X₁→X₂→X₃→X₄. Query: X₁. Evidence: X₄.
Eliminate hidden variables (X₂, X₃) starting from the ones farthest from query.
**Optimal order: X₃ first, then X₂.** This keeps intermediate factors small.

---

### Q65. Explaining away example
**Scenario:** Your car won't start. Two possible causes: dead battery or faulty starter motor.
- Before observation: battery and starter are independent
- Observe: car won't start → both become more likely
- Then learn: battery is dead → explaining away: P(faulty starter | car won't start, dead battery) DECREASES because the failure is already explained

This is the v-structure (collider) effect: Battery → CarWontStart ← Starter. Conditioning on CarWontStart makes Battery and Starter dependent.

---

*End of Solutions — Artificial Intelligence for GATE 2027*

---

## Section H Solutions: CSPs & Advanced Logic (Q66–Q80)

---

### Q66. Map Coloring CSP
(a) Total domain size = 4 regions × 3 colors = 12 values total (3 per variable).
(b) If A=Red, Forward Checking removes Red from domains of all neighbors of A. If B, C are neighbors of A: D_B = {Green, Blue}, D_C = {Green, Blue}. If D is not adjacent to A, D_D unchanged = {R, G, B}.

---

### Q67. AC-3 arc revision
Arc (X→Y), constraint X ≠ Y, D_X = {1,2,3}, D_Y = {1,2}.
- X=1: Is there y ∈ {1,2} with 1 ≠ y? Yes (y=2). Keep 1.
- X=2: Is there y ∈ {1,2} with 2 ≠ y? Yes (y=1). Keep 2.
- X=3: Is there y ∈ {1,2} with 3 ≠ y? Yes (y=1 or 2). Keep 3.

**No values removed.** D_X remains {1,2,3}. (The constraint is satisfiable for all X values.)

---

### Q68. AC-3 complexity
$O(ed^3)$ where $e$ = number of binary constraint arcs and $d$ = maximum domain size. Each arc can be re-queued at most $d$ times, and checking an arc takes $O(d^2)$.

---

### Q69. MRV = fail-first
MRV chooses the variable with the fewest remaining legal values. If a variable has only 1 value left, we must assign it. If it has 0, we fail immediately. By choosing the most constrained variable first, we **detect failures early** in the search tree, pruning more branches sooner.

---

### Q70. AC-3 with all-different
Initial: D1={1,2}, D2={1,2,3}, D3={1}.
- Arc (X1, X3): X3={1}, so remove 1 from D1. D1={2}.
- Arc (X2, X3): X3={1}, so remove 1 from D2. D2={2,3}.
- Arc (X2, X1): X1={2}, so remove 2 from D2. D2={3}.
- All other arcs: consistent.

**Final: D1={2}, D2={3}, D3={1}**. CSP is solved: X1=2, X2=3, X3=1.

---

### Q71. Skolemization
$\forall x \exists y \forall z [P(x,y) \lor \neg Q(y,z)]$

Step: y is existentially quantified under ∀x, so replace y with Skolem function f(x):
$\forall x \forall z [P(x, f(x)) \lor \neg Q(f(x), z)]$

Drop universal quantifiers:
$P(x, f(x)) \lor \neg Q(f(x), z)$

---

### Q72. Unification
(a) $P(x, f(y))$ and $P(a, f(b))$: $\theta = \{x/a, y/b\}$ ✅
(b) $P(x, x)$ and $P(a, b)$: $x/a$ then $x/b$ but $a \neq b$ → **Fails** ❌
(c) $P(f(x))$ and $P(x)$: $x = f(x)$ → **occurs check fails** — infinite structure ❌

---

### Q73. Resolution refutation
KB: (1) $\neg A \lor B$, (2) $\neg B \lor C$, (3) $A$
Negate goal: $\neg C$

- Resolve (2) $\neg B \lor C$ with $\neg C$: yields $\neg B$
- Resolve (1) $\neg A \lor B$ with $\neg B$: yields $\neg A$
- Resolve (3) $A$ with $\neg A$: yields $\{\}$ (empty clause)

Empty clause derived → **C is proved**.

---

### Q74. SAT complexity
**2-SAT** is in **P** — solvable in $O(n + m)$ using implication graph and SCC.
**3-SAT** is **NP-complete** — Cook-Levin theorem. It's the canonical NP-complete problem.

---

### Q75. Forward chaining
Start with facts: {A, B}
- Rule (3): $A \land B \to C$ — both premises satisfied → derive **C**. Facts: {A, B, C}
- Rule (4): $C \to D$ — premise satisfied → derive **D**. Facts: {A, B, C, D}
- No more rules fire. **Done.** All facts: {A, B, C, D}.

---

### Q76. Horn-SAT in linear time
Horn clauses have at most one positive literal. Forward chaining processes each clause at most once: when all premises become true, derive the conclusion. This is a simple propagation with no backtracking needed — $O(n)$ time. General SAT requires exploring exponentially many assignments because clauses can have multiple positive literals.

---

### Q77. Backward chaining trace
Goal: D
- To prove D: find rule $C \to D$. Need to prove C.
  - To prove C: find rule $A \land B \to C$. Need to prove A and B.
    - To prove A: A is a fact. ✅
    - To prove B: B is a fact. ✅
  - C proved. ✅
- D proved. ✅

---

### Q78. Skolemize
$\exists x \forall y \exists z [P(x, y, z)]$
- x is existentially quantified with no enclosing ∀ → replace x with Skolem constant $c$
- z is existentially quantified under ∀y → replace z with Skolem function $f(y)$

Result: $\forall y \, P(c, y, f(y))$ → drop ∀: $P(c, y, f(y))$

---

### Q79. STRIPS Stack(X, Y)
- **Preconditions:** Holding(X), Clear(Y)
- **Add list:** On(X, Y), Clear(X), HandEmpty
- **Delete list:** Holding(X), Clear(Y)

After stacking X on Y: X is on Y, X is clear (nothing on it), hand is empty; Y is no longer clear, not holding X.

---

### Q80. Frame problem
The frame problem: how to efficiently represent what **doesn't change** when an action is performed. In a naive approach, you need axioms for every unchanged fact.

STRIPS handles it with the **STRIPS assumption**: everything NOT in the add/delete lists remains unchanged. This is the "closed-world assumption" for effects.

---

## Section I Solutions: HMMs, MDPs & RL (Q81–Q95)

---

### Q81. Forward algorithm — α₁
$\alpha_1(H) = \pi_H \cdot b_H(o_1=1) = 0.6 \times 0.2 = 0.12$
$\alpha_1(C) = \pi_C \cdot b_C(o_1=1) = 0.4 \times 0.5 = 0.20$

---

### Q82. Viterbi vs Forward
The **Forward algorithm** uses **sum** over previous states: $\alpha_t(j) = [\sum_i \alpha_{t-1}(i) \cdot a_{ij}] \cdot b_j(o_t)$

The **Viterbi algorithm** uses **max**: $\delta_t(j) = [\max_i \delta_{t-1}(i) \cdot a_{ij}] \cdot b_j(o_t)$

And Viterbi stores backpointers $\psi_t(j) = \arg\max_i$ to recover the best state sequence via backtracking.

---

### Q83. Forward — α₂
$\alpha_2(H) = [\alpha_1(H) \cdot a_{HH} + \alpha_1(C) \cdot a_{CH}] \cdot b_H(o_2=2)$
$= [0.12 \times 0.7 + 0.20 \times 0.4] \times 0.4 = [0.084 + 0.08] \times 0.4 = 0.164 \times 0.4 = 0.0656$

$\alpha_2(C) = [\alpha_1(H) \cdot a_{HC} + \alpha_1(C) \cdot a_{CC}] \cdot b_C(o_2=2)$
$= [0.12 \times 0.3 + 0.20 \times 0.6] \times 0.4 = [0.036 + 0.12] \times 0.4 = 0.156 \times 0.4 = 0.0624$

---

### Q84. Three HMM problems
1. **Evaluation:** P(observation sequence | model) → **Forward algorithm**
2. **Decoding:** Most likely hidden state sequence → **Viterbi algorithm**
3. **Learning:** Estimate model parameters from observations → **Baum-Welch algorithm**

---

### Q85. Baum-Welch
Baum-Welch is a special case of the **Expectation-Maximization (EM)** algorithm applied to HMMs. E-step computes expected state occupancies (using forward-backward); M-step updates A, B, π.

---

### Q86. Value Iteration — one step
Starting from V(A)=V(B)=0:

$V(A) = \max_a \sum_{s'} T(A,a,s')[R(A,a,s') + \gamma V(s')]$

Only action: go.
$V(A) = T(A,go,B)[R(A,go,B) + 0.9 \times 0] + T(A,go,A)[R(A,go,A) + 0.9 \times 0]$
$= 0.8 \times 10 + 0.2 \times 0 = 8.0$

V(B) = 0 (no actions specified from B, or if terminal).

**After 1 iteration: V(A) = 8.0, V(B) = 0.**

---

### Q87. Bellman equation
$$V^*(s) = \max_a \sum_{s'} T(s, a, s') \left[ R(s, a, s') + \gamma V^*(s') \right]$$

This says: the optimal value of state s equals the best action's expected immediate reward plus discounted future value.

---

### Q88. Value Iteration vs Policy Iteration
| | Value Iteration | Policy Iteration |
|---|---|---|
| (a) Updates | V(s) directly via Bellman | Policy π(s), then evaluates V^π |
| (b) Per-iteration cost | O(\|S\| × \|A\| × \|S\|) per sweep | O(\|S\|³) for policy evaluation (solve linear system) |
| (c) # iterations | More (convergence can be slow) | Fewer (often converges very quickly) |

---

### Q89. Q-Learning off-policy
"Off-policy" means Q-Learning updates using the **max** over next actions (greedy policy) regardless of what action was actually taken:
$Q(s,a) \leftarrow Q(s,a) + \alpha[r + \gamma \max_{a'} Q(s',a') - Q(s,a)]$

SARSA is on-policy: updates using the action $a'$ that was **actually taken**:
$Q(s,a) \leftarrow Q(s,a) + \alpha[r + \gamma Q(s',a') - Q(s,a)]$

---

### Q90. Value Iteration
V(s) = max over actions of expected reward.

$V(s_1) = \max(V_{a1}, V_{a2})$
$V_{a1} = T(s_1,a_1,s_1)[R + \gamma V(s_1)] + T(s_1,a_1,s_2)[R + \gamma V(s_2)]$
$= 0.5[5 + 0.5 \times 0] + 0.5[5 + 0.5 \times 0] = 0.5 \times 5 + 0.5 \times 5 = 5$

Wait, R(s1,a1)=5 for both transitions:
$V_{a1} = 0.5 \times 5 + 0.5 \times 5 = 5$ (since V=0 initially)

$V_{a2} = 1.0 \times [1 + 0.5 \times 0] = 1$

$V(s_1) = \max(5, 1) = 5$

$V(s_2) = \max(V_{a1}, V_{a2})$
$V_{a1} = 1.0 \times [2 + 0] = 2$
$V_{a2} = 1.0 \times [0 + 0] = 0$
$V(s_2) = \max(2, 0) = 2$

**After 1 iteration: V(s1)=5, V(s2)=2.**

---

### Q91. Discount factor γ
γ controls the importance of future rewards.
- **γ = 0:** Agent is completely myopic — only cares about immediate reward
- **γ → 1:** Agent values long-term rewards almost as much as immediate (can cause convergence issues)
- Typical: γ = 0.9 or 0.99

---

### Q92. ε-greedy
Probability of choosing best action = $1 - \epsilon + \frac{\epsilon}{|A|}$ (best action gets the remaining probability after exploration plus its share of random choice). If there are many actions, approximately $1 - \epsilon = 0.9 = 90\%$.

---

### Q93. STRIPS preconditions
Preconditions ensure the action is **applicable** in the current state. Without checking, you could:
- Stack a block that isn't being held
- Pick up a block that's under another block
- Create physically impossible states

The plan would be meaningless — violating real-world constraints.

---

### Q94. Fluent in situation calculus
A **fluent** is a predicate or function whose truth value changes across situations (states). Example: $\text{On}(A, B, S_0)$ means "block A is on block B in situation $S_0$." After unstacking, in $S_1 = \text{Result}(\text{Unstack}(A,B), S_0)$, $\text{On}(A, B, S_1)$ is false.

"Fluent" = changes over time. Non-fluent example: Block(A) is always true regardless of what actions occur.

---

### Q95. Policy Iteration convergence
**Yes**, policy iteration often converges in far fewer iterations than value iteration. This is because:
1. Policy iteration makes optimal policy updates — each improvement step moves to a strictly better policy (or stays if already optimal)
2. There are finitely many deterministic policies ($|A|^{|S|}$), so it must terminate
3. In practice, it often converges in 5-10 iterations even for large state spaces
4. Value iteration converges asymptotically and may need many iterations for tight approximation

---

*End of Solutions — Artificial Intelligence for GATE 2027 — 95 Solutions*
