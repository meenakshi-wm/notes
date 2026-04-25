# Artificial Intelligence — Revision Notes for GATE 2027

> Quick-reference sheet: formulas, rules, comparison tables, GATE traps.

---

## 1. Propositional Logic — Key Equivalences

| Law | Formula |
|-----|---------|
| De Morgan's | ¬(p∧q) ≡ ¬p∨¬q; ¬(p∨q) ≡ ¬p∧¬q |
| Implication | p→q ≡ ¬p∨q |
| Contrapositive | p→q ≡ ¬q→¬p |
| Biconditional | p↔q ≡ (p→q)∧(q→p) |
| Double Negation | ¬¬p ≡ p |
| Export | p→(q→r) ≡ (p∧q)→r |
| ¬(p→q) | ≡ p∧¬q |

**CNF Conversion Steps:** Eliminate ↔ → Eliminate → → Push ¬ inward → Distribute ∨ over ∧

**Resolution:** Negate conclusion, convert all to CNF, resolve complementary literals until ⊥ (empty clause).

**Truth Table Rows:** n variables → 2ⁿ rows.

---

## 2. Predicate Logic — Quick Rules

| Concept | Rule |
|---------|------|
| ¬∀x P(x) | ≡ ∃x ¬P(x) |
| ¬∃x P(x) | ≡ ∀x ¬P(x) |
| ∀x(A∧B) | ≡ ∀xA ∧ ∀xB |
| ∃x(A∨B) | ≡ ∃xA ∨ ∃xB |
| ∀x(A∨B) | ≢ ∀xA ∨ ∀xB |

**Unification:** Find substitution θ that makes two terms identical. Occurs check: x cannot unify with t containing x.

**Prenex Normal Form:** Move all quantifiers to the front, renaming bound variables to avoid clashes.

**Free vs Bound:** A variable is bound if in scope of a quantifier for that variable; otherwise free.

---

## 3. Search Algorithms — Master Comparison Table

| Algorithm | Complete? | Optimal? | Time | Space | Notes |
|-----------|-----------|----------|------|-------|-------|
| **BFS** | Yes | Yes (unit cost) | O(b^d) | O(b^d) | FIFO queue |
| **DFS** | No | No | O(b^m) | O(bm) | LIFO stack |
| **DLS** | No | No | O(b^ℓ) | O(bℓ) | DFS with depth limit ℓ |
| **IDS** | Yes | Yes (unit cost) | O(b^d) | O(bd) | Best of BFS + DFS |
| **UCS** | Yes | Yes | O(b^(1+⌊C*/ε⌋)) | Same | Priority queue by g(n) |
| **Greedy** | No | No | O(b^m) | O(b^m) | Expands by h(n) only |
| **A*** | Yes | Yes (admissible h) | O(b^d) | O(b^d) | f(n) = g(n) + h(n) |
| **IDA*** | Yes | Yes | O(b^d) | O(bd) | A* with iterative deepening |

b = branching factor, d = solution depth, m = max depth, ℓ = depth limit

**IDS overhead:** ~11% more nodes than BFS for b=10.

---

## 4. A* — Key Properties

| Property | Condition |
|----------|-----------|
| Optimal | h is admissible: h(n) ≤ h*(n) |
| Never reopens nodes | h is consistent: h(n) ≤ c(n,n') + h(n') |
| Consistent → Admissible | Always true |
| Admissible → Consistent | NOT always true |
| f values along path | Non-decreasing (if consistent) |
| Expanded nodes satisfy | f(n) ≤ C* (if admissible) |
| h₁ dominates h₂ | h₁(n) ≥ h₂(n) ∀n → A* with h₁ expands ≤ nodes than h₂ |
| max(h₁,h₂) | Admissible if both admissible, consistent if both consistent |

---

## 5. Adversarial Search

### Minimax
- MAX picks max of children, MIN picks min of children
- Time: O(b^m), Space: O(bm)
- Assumes **perfect play** by both players

### Alpha-Beta Pruning
- **α:** Best value MAX can guarantee (MAX's lower bound) — starts at −∞
- **β:** Best value MIN can guarantee (MIN's upper bound) — starts at +∞
- **Prune when:** α ≥ β

| At MAX node | Update α = max(α, child_value). Prune if α ≥ β |
|-------------|--------------------------------------------------|
| At MIN node | Update β = min(β, child_value). Prune if α ≥ β |

**Best case (perfect ordering):** O(b^(m/2)) — doubles effective search depth!
**Worst case (bad ordering):** O(b^m) — no improvement

**Does NOT change minimax value**, only prunes irrelevant branches.

---

## 6. Bayesian Networks — Core Formulas

### Bayes' Theorem
$$P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}$$

### Chain Rule (BN factorization)
$$P(X_1,...,X_n) = \prod_{i=1}^{n} P(X_i | Parents(X_i))$$

### Total Probability
$$P(B) = \sum_i P(B|A_i)P(A_i)$$

### Parameters
- Full joint (n Boolean): 2ⁿ − 1
- BN: Σᵢ 2^|Parents(Xᵢ)| (one per parent configuration per node)

---

## 7. D-Separation Rules

Three basic structures on path A ── B ── C:

| Structure | Name | Not given B | Given B |
|-----------|------|-------------|---------|
| A → B → C | Chain | Active (dependent) | **Blocked** (independent) |
| A ← B → C | Fork | Active (dependent) | **Blocked** (independent) |
| A → B ← C | Collider | **Blocked** (independent) | Active (dependent) |

**Path is blocked** (d-separated) if ANY node on path is:
- **Non-collider** and in evidence set, OR
- **Collider** and neither it NOR its descendants are in evidence set

**Markov Blanket** of X = Parents(X) ∪ Children(X) ∪ Parents of Children(X)
- X is conditionally independent of ALL other variables given its Markov Blanket

---

## 8. Probabilistic Inference Methods

| Method | Type | Key Idea |
|--------|------|----------|
| **Enumeration** | Exact | Sum over all hidden variables |
| **Variable Elimination** | Exact | Eliminate variables one by one, cache factors |
| **Rejection Sampling** | Approximate | Sample everything, reject if evidence doesn't match |
| **Likelihood Weighting** | Approximate | Fix evidence, weight samples by likelihood |
| **Gibbs Sampling** | Approximate (MCMC) | Resample each non-evidence variable given Markov blanket |

**VE elimination order** matters: affects size of intermediate factors. Optimal order is NP-hard; heuristic = min-degree, min-fill.

---

## 9. Common GATE Traps

| Trap | Correct Answer |
|------|----------------|
| "DFS is complete" | No — infinite loops, infinite depth |
| "A* always optimal" | Only with **admissible** h |
| "Consistent implies admissible" | ✓ True |
| "Admissible implies consistent" | ✗ False |
| "Alpha-beta changes minimax value" | ✗ No — same value, fewer nodes |
| "Collider blocks when observed" | ✗ Collider blocks when NOT observed; becomes active when observed |
| "Parent and child always dependent in BN" | Not necessarily — depends on evidence |
| "P(disease\|positive test) ≈ sensitivity" | ✗ Base rate fallacy — need prior |
| "Greedy search is optimal" | ✗ Never guaranteed optimal |
| "IDS expands many more nodes than BFS" | ✗ Only ~11% overhead for b=10 |

---

## 10. Quick-Solve Templates

### Bayes' Rule Problem
1. Identify P(cause), P(evidence|cause), P(evidence|¬cause)
2. Compute P(evidence) = P(e|c)P(c) + P(e|¬c)P(¬c)
3. Apply: P(cause|evidence) = P(e|c)P(c)/P(e)

### Minimax + Alpha-Beta
1. Draw game tree, label leaves with utilities
2. Bottom-up: MIN takes min, MAX takes max
3. For pruning: pass α,β top-down; prune when α ≥ β

### A* Trace
1. Priority queue sorted by f(n) = g(n) + h(n)
2. Expand lowest f; break ties by lower h
3. If graph search: don't re-expand visited nodes

### D-Separation Check
1. Enumerate all paths between query variables
2. For each path, check each intermediate node
3. Path blocked if any node is blocked (per rules in §7)
4. Independent iff ALL paths are blocked

---

## 11. CSP Quick Reference

| Technique | Purpose |
|---|---|
| MRV | Variable ordering: fewest remaining values first |
| LCV | Value ordering: least constraining value first |
| Forward Checking | Remove inconsistent values from neighbors after assignment |
| AC-3 | Enforce arc consistency; complexity $O(ed^3)$ |
| Backtracking | DFS with constraint checking |

---

## 12. Logic & Resolution Quick Reference

| Concept | Key Point |
|---|---|
| Horn clause | At most 1 positive literal; solvable in linear time |
| 2-SAT | In P; 3-SAT is NP-complete |
| Forward chaining | Data-driven; derive new facts from known facts + rules |
| Backward chaining | Goal-driven; start from query, recursively prove premises |
| Skolemization | ∃x → constant c; ∀x∃y → Skolem function f(x) |
| Unification | Find θ making two terms identical; occurs check prevents infinite terms |
| Resolution refutation | Negate goal, derive empty clause → goal proved |

---

## 13. HMM Quick Reference

| Problem | Algorithm | Replaces |
|---|---|---|
| Evaluation: P(O\|λ) | Forward | — |
| Decoding: best state sequence | Viterbi | sum → max |
| Learning: estimate λ | Baum-Welch (EM) | — |

$\alpha_t(j) = [\sum_i \alpha_{t-1}(i) \cdot a_{ij}] \cdot b_j(o_t)$

---

## 14. MDP Quick Reference

**Bellman:** $V^*(s) = \max_a \sum_{s'} T(s,a,s')[R(s,a,s') + \gamma V^*(s')]$

| | Value Iteration | Policy Iteration |
|---|---|---|
| Updates | V directly | π then V^π |
| Iterations | More | Fewer |
| Per iteration | Cheaper | More expensive |

**Q-Learning:** $Q(s,a) \leftarrow Q(s,a) + \alpha[r + \gamma \max_{a'} Q(s',a') - Q(s,a)]$

γ=0: myopic | γ→1: far-sighted | ε-greedy: explore with prob ε

---

## 15. STRIPS Quick Reference

| Component | Example: Stack(X,Y) |
|---|---|
| Preconditions | Holding(X), Clear(Y) |
| Add effects | On(X,Y), Clear(X), HandEmpty |
| Delete effects | Holding(X), Clear(Y) |

Frame assumption: everything not in add/delete list stays unchanged.

---

*End of Revision Notes — Artificial Intelligence for GATE 2027*
