# Artificial Intelligence — Questions and PYQs for GATE 2027

> **Total Questions:** 65 | **Sections:** 7
> **Difficulty Mix:** Easy (25%) | Medium (40%) | GATE PYQ level (35%)

---

## Section A: Propositional Logic (10 Questions)

**Q1.** Determine if the following is a tautology: (p∧(p→q))→q

**Q2.** Write the contrapositive, converse, and inverse of: "If it rains, then the ground is wet."

**Q3.** Convert to CNF: (p→q)∧(r→s)

**Q4.** Using resolution, prove that {p∨q, ¬p∨r, ¬q∨r} ⊨ r

**Q5.** [GATE] How many rows are in the truth table for a formula with 5 propositional variables?

**Q6.** Simplify: ¬(p→q) to a simpler form.

**Q7.** [GATE] Determine if the following argument is valid: "If I study, I pass. I did not pass. Therefore, I did not study."

**Q8.** Convert to CNF: ¬(p∧q)→(r∨s)

**Q9.** [GATE DA] Show that p→(q→r) is equivalent to (p∧q)→r.

**Q10.** The formula p↔q has how many satisfying assignments?

---

## Section B: Predicate Logic (10 Questions)

**Q11.** Translate to FOL: "Every student who studies hard passes the exam."

**Q12.** Translate: "There exists a person who likes everyone."

**Q13.** Negate: ∀x ∃y (Loves(x,y))

**Q14.** Identify free and bound variables: ∀x(P(x,y) → ∃y Q(x,y))

**Q15.** [GATE] Translate: "No one who is both a programmer and a manager."

**Q16.** Unify: P(x, f(y)) and P(g(z), f(h(z))).

**Q17.** [GATE DA] Is ∀x P(x) → ∃x P(x) valid? Justify.

**Q18.** Translate: "For every real number, there exists a greater real number."

**Q19.** [GATE] Which is the negation of "Everyone passed or someone failed"?
(a) No one passed and everyone succeeded (b) Someone failed and everyone passed 
(c) Not everyone passed and no one failed (d) None of the above

**Q20.** Convert to prenex normal form: ∀x P(x) → ∃x Q(x)

---

## Section C: Uninformed Search (10 Questions)

**Q21.** Compare BFS and DFS in terms of completeness, optimality, time, and space.

**Q22.** In BFS with branching factor b=3 and depth d=4, how many nodes are generated in the worst case?

**Q23.** Why is IDS preferred over BFS for large search spaces?

**Q24.** [GATE] A graph has branching factor 4 and goal at depth 6. What is the space complexity of IDS?

**Q25.** UCS is exploring a graph. Node A has cost 5, Node B has cost 3, Node C has cost 7. Which is expanded first?

**Q26.** [GATE DA] Can DFS find the optimal solution? Under what conditions?

**Q27.** In IDS, the total nodes generated up to depth d with branching factor b is approximately?

**Q28.** [GATE] A search tree has b=2, depth 3 (goal at depth 3). Showing the order of node expansion for BFS, DFS, and IDS.

**Q29.** What is the relationship between UCS and Dijkstra's algorithm?

**Q30.** [GATE] In a graph with cycles, what modification makes DFS complete?

---

## Section D: Informed Search (10 Questions)

**Q31.** Define admissible and consistent heuristics. Give examples.

**Q32.** Graph: S→A(2), S→B(5), A→C(3), B→C(1), C→G(4). h(S)=7, h(A)=5, h(B)=3, h(C)=2, h(G)=0. Trace A* search.

**Q33.** Is h₁(n) = 0 for all n admissible? What does A* become with this heuristic?

**Q34.** [GATE] If h₁ is consistent and h₂ is consistent, is max(h₁, h₂) consistent?

**Q35.** [GATE DA] A* with admissible h is applied. Can it expand a node with f(n) > f*(optimal cost)?

**Q36.** Compare Greedy Best-First and A* search.

**Q37.** Explain the limitations of hill climbing. How does random restart help?

**Q38.** [GATE] For 8-puzzle, compare h₁ (misplaced tiles) and h₂ (Manhattan distance). Which is more informed?

**Q39.** What is IDA*? When is it preferred over A*?

**Q40.** [GATE DA] If h(n) > h*(n) for some n, is A* guaranteed to find the optimal solution?

---

## Section E: Adversarial Search (10 Questions)

**Q41.** Explain the minimax algorithm for a two-player game.

**Q42.** A game tree has:
```
        MAX
       / | \
    MIN MIN MIN
    /\  /\  /\
   3 5 2 8 1 7
```
Find the minimax value.

**Q43.** Apply alpha-beta pruning to the tree in Q42. Which nodes are pruned?

**Q44.** [GATE] In alpha-beta pruning, what is the best-case time complexity with optimal move ordering?

**Q45.** After alpha-beta pruning, does the minimax value change?

**Q46.** [GATE DA] A game tree has branching factor b=3 and depth m=4. How many leaf nodes does minimax evaluate? With perfect alpha-beta pruning?

**Q47.** A game tree:
```
        MAX
       /    \
     MIN    MIN
    / \    / \
   5   3  8   2
```
Apply alpha-beta pruning with left-to-right evaluation.

**Q48.** [GATE] What is the "horizon effect" in game-playing programs?

**Q49.** In a non-zero-sum game, can minimax be directly applied? Why or why not?

**Q50.** [GATE DA] Explain how move ordering affects alpha-beta pruning efficiency.

---

## Section F: Bayesian Networks (10 Questions)

**Q51.** Given P(A)=0.3, P(B)=0.6, P(A|B)=0.5. Find P(B|A).

**Q52.** A Bayesian network has: A→C, B→C, C→D. Write the joint probability expression.

**Q53.** [GATE] In the network A→B→C, is A independent of C given B?

**Q54.** In the network A→C←B, are A and B independent? Are they independent given C?

**Q55.** [GATE DA] A network has 4 Boolean variables A, B, C, D. Full joint table needs how many parameters? If the network is A→B→C→D, how many parameters?

**Q56.** What is the Markov blanket of node B in: A→B, B→C, B→D, E→C?

**Q57.** [GATE] For d-separation: In A→B→C←D, is A⊥D? Is A⊥D|C?

**Q58.** Given: P(Disease)=0.01, P(TestPos|Disease)=0.95, P(TestPos|¬Disease)=0.05. Find P(Disease|TestPos).

**Q59.** [GATE DA] A Bayesian network represents: Flu→Fever, Cold→Fever, Fever→MissWork. If we observe MissWork=T, does this make Flu and Cold dependent?

**Q60.** How many independent parameters in a CPT for a node with 3 Boolean parents?

---

## Section G: Probabilistic Inference (5 Questions)

**Q61.** Explain inference by enumeration with an example.

**Q62.** [GATE] What is the advantage of variable elimination over enumeration?

**Q63.** In likelihood weighting, how are samples generated and weighted?

**Q64.** [GATE DA] A Bayesian network has the structure X₁→X₂→X₃→X₄. What is the optimal variable elimination order for computing P(X₁|X₄)?

**Q65.** Explain the concept of "explaining away" with a concrete example.

---

*End of Questions — Artificial Intelligence for GATE 2027*

---

## Section H: CSPs & Advanced Logic (Q66–Q80)

**Q66.** [GATE DA] In a map coloring CSP with 4 regions and 3 colors, (a) what is the total domain size? (b) If region A is colored Red, how does Forward Checking reduce domains of A's neighbors?

**Q67.** In AC-3, you process arc (X→Y). X has domain {1,2,3}, Y has domain {1,2}. Constraint: X ≠ Y. What values are removed from X's domain?

**Q68.** [GATE] What is the worst-case complexity of AC-3 for a CSP with $e$ arcs and maximum domain size $d$?

**Q69.** Why is MRV (Minimum Remaining Values) also called the "fail-first" heuristic?

**Q70.** [GATE DA] Given: all-different constraint on X1, X2, X3 with domains D1={1,2}, D2={1,2,3}, D3={1}. Apply AC-3. What are the final domains?

**Q71.** Convert to Skolem normal form: $\forall x \exists y \forall z [P(x,y) \lor \neg Q(y,z)]$

**Q72.** [GATE] Unify the following or explain why unification fails:
(a) $P(x, f(y))$ and $P(a, f(b))$
(b) $P(x, x)$ and $P(a, b)$
(c) $P(f(x))$ and $P(x)$

**Q73.** Using resolution refutation, prove that $C$ follows from: (1) $A \to B$, (2) $B \to C$, (3) $A$.

**Q74.** [GATE DA] Is 2-SAT in P or NP-complete? What about 3-SAT?

**Q75.** Given Horn clauses: (1) $A$, (2) $B$, (3) $A \land B \to C$, (4) $C \to D$. Use forward chaining to derive all facts.

**Q76.** [GATE] Why is Horn-SAT solvable in linear time while general SAT is NP-complete?

**Q77.** In backward chaining, to prove $D$ from the KB: $A$, $B$, $A \land B \to C$, $C \to D$, what is the trace?

**Q78.** Skolemize: $\exists x \forall y \exists z [P(x, y, z)]$

**Q79.** [GATE DA] In the Blocks World STRIPS formulation, give the preconditions, add list, and delete list for Stack(X, Y).

**Q80.** What is the frame problem in planning? How does STRIPS handle it?

---

## Section I: HMMs, MDPs & Reinfortic Learning (Q81–Q95)

**Q81.** [GATE DA] An HMM has 2 states {H, C}, observations {1,2,3}. Given:
$\pi = [0.6, 0.4]$, $A = \begin{pmatrix} 0.7 & 0.3 \\ 0.4 & 0.6 \end{pmatrix}$, $B = \begin{pmatrix} 0.2 & 0.4 & 0.4 \\ 0.5 & 0.4 & 0.1 \end{pmatrix}$.
Compute $\alpha_1(H)$ and $\alpha_1(C)$ for observation $o_1 = 1$.

**Q82.** [GATE] In the Viterbi algorithm, what is the key difference from the forward algorithm?

**Q83.** For the HMM in Q81, compute $\alpha_2(H)$ and $\alpha_2(C)$ for observation sequence $(o_1=1, o_2=2)$.

**Q84.** [GATE DA] What are the three fundamental HMM problems? Name the algorithm for each.

**Q85.** [GATE] The Baum-Welch algorithm is a special case of which general algorithm?

**Q86.** An MDP has states {A, B}, actions {go}, transitions: T(A, go, B)=0.8, T(A, go, A)=0.2: R(A,go,B)=10, R(A,go,A)=0. γ=0.9. Compute one iteration of value iteration starting from V(A)=V(B)=0.

**Q87.** [GATE DA] Write the Bellman equation for the optimal value function $V^*(s)$.

**Q88.** [GATE] Compare Value Iteration and Policy Iteration in terms of: (a) what is updated, (b) per-iteration cost, (c) number of iterations.

**Q89.** In Q-Learning, what does "off-policy" mean? How does the Q-update rule differ from SARSA?

**Q90.** [GATE DA] Given: states {s1, s2}, actions {a1, a2}, γ=0.5. 
T(s1,a1,s1)=0.5, T(s1,a1,s2)=0.5, R(s1,a1)=5
T(s1,a2,s1)=1.0, R(s1,a2)=1
T(s2,a1,s2)=1.0, R(s2,a1)=2
T(s2,a2,s1)=1.0, R(s2,a2)=0
Perform one iteration of value iteration starting from V=0.

**Q91.** [GATE] What is the discount factor γ? What happens when γ=0 and γ→1?

**Q92.** In ε-greedy exploration with ε=0.1, what is the probability of choosing the best known action?

**Q93.** [GATE DA] In STRIPS, why must we check preconditions before applying an action? What happens if we don't?

**Q94.** [GATE] For situation calculus, what is a fluent? Give an example.

**Q95.** [GATE DA] Can policy iteration converge in fewer iterations than value iteration? Explain why.

---

*End of Questions — Artificial Intelligence for GATE 2027 — 95 Questions*
