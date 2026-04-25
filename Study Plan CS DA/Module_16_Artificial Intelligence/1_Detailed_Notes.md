# 🤖🧠 Artificial Intelligence — Detailed Notes for GATE 2027 🚀✨

> *"The question of whether a computer can think is no more interesting than the question of whether a submarine can swim."* — **Edsger Dijkstra** 🎓

> **GATE Weightage:** 5–8 marks (2–4 questions) | **Priority:** Very High (GATE DA)
> **Time to Spend:** 6–8 weeks | **Difficulty:** Medium-Hard

---

### 🎯 What is Artificial Intelligence? — A Beginner's Analogy

> 🍼 **Imagine building a robot butler 🤖:**
> - **Propositional Logic** = Teaching it basic rules: "IF the door is locked AND you have a key, THEN open the door" 🔐
> - **Predicate Logic** = Teaching it about TYPES of things: "ALL keys open SOME doors" 📚
> - **Search Algorithms** = Teaching it to find the best route through your house (BFS, DFS, A*) 📍
> - **Adversarial Search** = Teaching it to play chess against you (Minimax!) ♟️
> - **Bayesian Networks** = Teaching it to reason under uncertainty: "It's cloudy, so probably rain — take an umbrella!" ☂️
> - AI is about making machines that can **reason, search, learn, and decide** like humans (but faster!) ⚡

### 🏭 Real-World AI Applications — AI is Everywhere!

| 🏢 Company/Domain | 🤖 AI Application | 💡 Technique Used | 💰 Impact |
|---|---|---|---|
| **Google Maps** 🗺️ | Best route finding | A* Search + Dijkstra | Saves **1B hours of travel** annually |
| **Chess.com** ♟️ | Stockfish engine | Alpha-Beta Pruning + Minimax | ELO rating **3600+** (superhuman) |
| **Google** 🔍 | Knowledge Graph reasoning | Predicate Logic + Inference | Powers **5.9B searches/day** |
| **Hospitals** 🏥 | Disease diagnosis | Bayesian Networks | **95%+ accuracy** in diagnosis |
| **Video Games** 🎮 | NPC behavior | Search + Game Trees | $180B gaming industry |
| **Siri/Alexa** 🗣️ | Voice understanding | Logic + Probabilistic Inference | 500M+ devices |
| **GPS Navigation** 🚗 | Real-time rerouting | Informed Search (A*) | Used by **1B+ people** daily |
| **Fraud Detection** 🔒 | Suspicious activity | Bayesian reasoning | Saves **$20B/year** globally |

> 🔑 **Fun Fact:** The term "Artificial Intelligence" was coined at the **Dartmouth Conference in 1956** by John McCarthy! The conference proposal boldly stated: *"Every aspect of learning can in principle be so precisely described that a machine can be made to simulate it."* 🌟

---

## Table of Contents
1. [Propositional Logic](#1-propositional-logic)
2. [Predicate (First-Order) Logic](#2-predicate-logic)
3. [Search Algorithms](#3-search-algorithms)
4. [Informed Search](#4-informed-search)
5. [Adversarial Search](#5-adversarial-search)
6. [Bayesian Networks](#6-bayesian-networks)
7. [Probabilistic Inference](#7-probabilistic-inference)

---

## 1. 💡 Propositional Logic

> 🍼 **Beginner Analogy — Light Switches in Your House:**
> Think of propositional logic as **light switches** 💡:
> - Each switch is either **ON (True)** or **OFF (False)** — nothing in between!
> - **AND (∧):** Both kitchen AND bedroom lights must be ON for the hallway to light up
> - **OR (∨):** Either the lamp OR the tube light being ON lights the room
> - **NOT (¬):** A dimmer switch that FLIPS the state
> - **IMPLICATION (→):** "IF it rains, THEN I carry an umbrella" ☔ (only a LIE if it rains and you DON'T carry one!)
> - Computers make EVERY decision using these tiny TRUE/FALSE building blocks! 🧱

> 🏭 **Where Propositional Logic is Used:**
> - 🖥️ **Circuit Design:** Every CPU gate (AND, OR, NOT) is propositional logic in hardware!
> - 🔒 **Cybersecurity:** Firewall rules: IF (source=untrusted AND port=22) THEN block
> - 🤖 **AI Agents:** Robot decision-making: IF obstacle_ahead AND NOT path_right THEN turn_left

### Connectives
| Name | Symbol | Notation | Truth |
|------|--------|----------|-------|
| Negation | ¬ | NOT p | Inverts |
| Conjunction | ∧ | p AND q | Both true |
| Disjunction | ∨ | p OR q | At least one |
| Implication | → | p IMPLIES q | False only if p=T, q=F |
| Biconditional | ↔ | p IFF q | Both same |

### Implication Truth Table (MOST IMPORTANT)
| p | q | p → q |
|---|---|-------|
| T | T | **T** |
| T | F | **F** |
| F | T | **T** |
| F | F | **T** |

**Key:** p→q is false ONLY when p is true and q is false.
**Equivalent forms:** p→q ≡ ¬p∨q ≡ ¬q→¬p (contrapositive)

### Tautology, Contradiction, Contingency
- **Tautology:** Always TRUE for all truth value assignments (e.g., p∨¬p)
- **Contradiction:** Always FALSE (e.g., p∧¬p)
- **Contingency:** Neither (depends on values)

### Logical Equivalences
| Law | Equivalence |
|-----|-----------|
| De Morgan's | ¬(p∧q) ≡ ¬p∨¬q, ¬(p∨q) ≡ ¬p∧¬q |
| Implication | p→q ≡ ¬p∨q |
| Contrapositive | p→q ≡ ¬q→¬p |
| Biconditional | p↔q ≡ (p→q)∧(q→p) |
| Double negation | ¬¬p ≡ p |
| Distribution | p∧(q∨r) ≡ (p∧q)∨(p∧r) |
| Absorption | p∨(p∧q) ≡ p |
| Export | (p∧q)→r ≡ p→(q→r) |

### Arguments & Validity
An argument is **valid** if the conclusion is true whenever all premises are true.

**Common valid argument forms:**
| Name | Form |
|------|------|
| Modus Ponens | p, p→q ⊢ q |
| Modus Tollens | ¬q, p→q ⊢ ¬p |
| Hypothetical Syllogism | p→q, q→r ⊢ p→r |
| Disjunctive Syllogism | p∨q, ¬p ⊢ q |
| Resolution | p∨q, ¬p∨r ⊢ q∨r |

### CNF (Conjunctive Normal Form)
A formula in CNF is a conjunction (AND) of clauses, where each clause is a disjunction (OR) of literals.
Example: (p∨¬q) ∧ (¬p∨r∨s) ∧ (q∨¬r)

**Conversion to CNF:**
1. Eliminate ↔: replace with (→)∧(←)
2. Eliminate →: replace with ¬p∨q
3. Push negations inward (De Morgan's)
4. Distribute ∨ over ∧

### Resolution Rule
From clauses (A∨p) and (B∨¬p), derive (A∨B).
**Used for:** Proof by refutation — negate conclusion, convert to CNF, apply resolution until empty clause (⊥) or no new clauses.

---

## 2. 🌐 Predicate Logic

> 🍼 **Beginner Analogy — From Simple Rules to Rich Language:**
> - **Propositional Logic** is like texting in abbreviations: "rain=yes, umbrella=yes" 📱
> - **Predicate Logic** is like writing full sentences: "EVERY student WHO studies PASSES the exam" 📝
> - It adds **variables** (x = any student), **predicates** (Studies(x), Passes(x)), and **quantifiers** (∀ = "all", ∃ = "some")
> - Think of it as upgrading from a calculator to a full programming language! 🚀

> 🏭 **Where Predicate Logic is Used:**
> - 🗄️ **SQL is Predicate Logic!** `SELECT * FROM students WHERE grade > 90` = ∃x(Student(x) ∧ Grade(x) > 90)
> - 🤖 **Prolog Language:** Entire programming language based on predicate logic (used in AI, expert systems)
> - 📚 **Knowledge Graphs:** Google's Knowledge Graph, Wikidata, and DBpedia all represent facts using predicate logic

### First-Order Logic (FOL)
Extends propositional logic with:
- **Constants:** specific objects (Alice, 5)
- **Variables:** x, y, z
- **Predicates:** properties/relations (Human(x), Loves(x,y))
- **Functions:** mappings (father(x), plus(x,y))
- **Quantifiers:** ∀ (for all), ∃ (there exists)

### Quantifiers
- **Universal (∀):** ∀x P(x) = "P holds for all x"
- **Existential (∃):** ∃x P(x) = "P holds for some x"

**Negation of quantifiers:**
$$¬∀x\; P(x) ≡ ∃x\; ¬P(x)$$
$$¬∃x\; P(x) ≡ ∀x\; ¬P(x)$$

### Nested Quantifiers
| Expression | Meaning |
|-----------|---------|
| ∀x ∀y P(x,y) | P holds for all x,y pairs |
| ∃x ∃y P(x,y) | P holds for some x,y pair |
| ∀x ∃y P(x,y) | For every x, there exists a y (y may depend on x) |
| ∃x ∀y P(x,y) | There is an x that works for ALL y |

**Order matters:** ∀x∃y ≠ ∃y∀x (generally)
- ∀x∃y Loves(x,y) = "Everyone loves someone" (different person possible)
- ∃y∀x Loves(x,y) = "There exists someone loved by everyone"

### Free and Bound Variables
- **Bound:** Within scope of a quantifier (∀x or ∃x)
- **Free:** Not bound by any quantifier
- A **sentence** (closed formula) has no free variables

### FOL Translation Examples
- "All humans are mortal": ∀x (Human(x) → Mortal(x))
- "Some students like AI": ∃x (Student(x) ∧ Likes(x, AI))
- "No bird is a mammal": ¬∃x (Bird(x) ∧ Mammal(x)) ≡ ∀x (Bird(x) → ¬Mammal(x))
- "Everyone has a mother": ∀x ∃y Mother(y, x)

### Validity & Satisfiability (FOL)
- **Valid:** True in every interpretation (e.g., ∀x P(x) → ∃x P(x))
- **Satisfiable:** True in at least one interpretation
- **Unsatisfiable:** False in every interpretation
- Valid = negation is unsatisfiable

### Unification
Finding substitution θ such that two expressions become identical.
- Unify P(x, f(y)) and P(a, f(b)): θ = {x/a, y/b}
- Unify P(x, x) and P(a, b): fails if a ≠ b (x can't be both a and b)

---

## 3. 🔍 Search Algorithms

> 🍼 **Beginner Analogy — Finding Your Way in a Maze:**
> Imagine you're lost in a giant maze 🌿 and trying to find the exit:
> - **BFS (Breadth-First):** You check ALL paths 1 step away, then ALL paths 2 steps away... like a ripple in water 🌊 — guaranteed to find the SHORTEST path but explores A LOT!
> - **DFS (Depth-First):** You go as DEEP as possible down one path until you hit a wall, then backtrack 🔙 — uses less memory but might go in circles!
> - **A* Search:** You're in the maze BUT someone tells you "the exit is roughly NORTH" 🧭 — you combine actual distance walked + estimated distance remaining. **The SMARTEST searcher!**
> - **Hill Climbing:** You can see the exit on a hill and always walk uphill... but might get stuck in a valley! ⛰️

> 🏭 **Production Examples:**
> - 🎮 **Video Games:** Pathfinding for NPCs in games like Civilization, StarCraft (A* is the industry standard!)
> - 🗺️ **Google Maps:** Uses A*/Dijkstra to find optimal routes among millions of intersections
> - 🤖 **Robot Navigation:** Roomba vacuum cleaners use search algorithms to clean your room efficiently
> - 🧩 **Puzzle Solvers:** Rubik's Cube solvers use IDA* to find solutions in 20 moves or less!

### Search Problem Formulation
- **State space:** All possible states
- **Initial state:** Starting configuration
- **Actions/Successors:** Possible transitions from each state
- **Goal test:** Is this a goal state?
- **Path cost:** Cost of a sequence of actions

### Uninformed Search

#### BFS (Breadth-First Search)
- **Strategy:** Expand shallowest node first (FIFO queue)
- **Complete?** Yes (if branching factor b is finite)
- **Optimal?** Yes (if all step costs equal)
- **Time:** O(b^d)
- **Space:** O(b^d) ← main drawback

#### DFS (Depth-First Search)
- **Strategy:** Expand deepest node first (LIFO stack)
- **Complete?** No (may loop in infinite spaces; yes if finite and no cycles)
- **Optimal?** No
- **Time:** O(b^m) where m = max depth
- **Space:** O(bm) ← main advantage

#### DLS (Depth-Limited Search)
DFS with depth limit l. Complete if l ≥ d. Not optimal.

#### IDS (Iterative Deepening Search)
Run DLS with increasing limits (l=0,1,2,...).
- **Complete?** Yes
- **Optimal?** Yes (if step costs equal)
- **Time:** O(b^d) [redundant work is only O(b)]
- **Space:** O(bd) ← combines BFS optimality with DFS space!
- **Best uninformed search** when search space is large and depth unknown.

#### UCS (Uniform Cost Search)
- **Strategy:** Expand lowest-cost node first (priority queue by path cost g(n))
- **Complete?** Yes (if step costs > 0)
- **Optimal?** Yes (always)
- **Time:** O(b^(1+⌊C*/ε⌋)) where C* = optimal cost, ε = min step cost
- **Same as Dijkstra's** for graph search

### Search Summary Table

| Algorithm | Complete | Optimal | Time | Space |
|-----------|----------|---------|------|-------|
| BFS | Yes | Yes* | O(b^d) | O(b^d) |
| DFS | No | No | O(b^m) | O(bm) |
| DLS | If l≥d | No | O(b^l) | O(bl) |
| IDS | Yes | Yes* | O(b^d) | O(bd) |
| UCS | Yes | Yes | O(b^(C*/ε)) | O(b^(C*/ε)) |

(*when all step costs are equal)

---

## 4. 🧭 Informed Search

> 🍼 **Beginner Analogy — The Smart GPS vs Dumb Wanderer:**
> - **Uninformed Search (BFS/DFS):** Like wandering a city with NO map — you check every street! 😵‍💫
> - **Informed Search (A*):** Like using Google Maps 🗺️ — you know the approximate direction and distance to your destination!
> - **Heuristic h(n)** = your estimate of how far you are from the goal (like "the restaurant is roughly 2 km north")
> - **A* = how far you've walked + how far you think is left** (f = g + h) 🧠
> - A* is guaranteed to find the **optimal path** if your estimate never overestimates (admissible heuristic)!

> 🔬 **Fun Fact:** A* was invented by **Peter Hart, Nils Nilsson, and Bertram Raphael** at Stanford Research Institute in 1968. It's STILL the gold standard for pathfinding in games and navigation 56 years later! 🏆

### Heuristic Function
h(n) = estimated cost from node n to goal.
- **Admissible:** h(n) ≤ h*(n) (never overestimates true cost)
- **Consistent (Monotone):** h(n) ≤ c(n,n') + h(n') (triangle inequality)
- Consistent ⟹ Admissible (but not vice versa)

### Greedy Best-First Search
- Expands node with smallest h(n)
- **Complete?** No (can get stuck in loops)
- **Optimal?** No
- **Time/Space:** O(b^m) (worst case)

### A* Search
- **Evaluation function:** f(n) = g(n) + h(n)
  - g(n) = actual cost from start to n
  - h(n) = estimated cost from n to goal
- Expands node with smallest f(n) (priority queue)

**Properties:**
- **Complete?** Yes (if h is admissible)
- **Optimal?** Yes (if h is admissible for tree search; consistent for graph search)
- **Optimally efficient:** No algorithm expanding fewer nodes can guarantee optimality

**A* variants:**
- IDA* (Iterative Deepening A*): memory-efficient A*
- Beam Search: keeps only k best nodes at each level

### Hill Climbing
- **Simple hill climbing:** Move to first neighbor better than current
- **Steepest ascent:** Move to best neighbor
- **Problems:** local maxima, plateaus, ridges
- **Solutions:** random restarts, simulated annealing, stochastic hill climbing

---

## 5. ♟️ Adversarial Search

> 🍼 **Beginner Analogy — Chess with a Smarty-Pants Friend:**
> Imagine playing chess against your smartest friend 🧠:
> - Before moving, you think: "If I move here, they'll probably move THERE, then I'll go HERE..."
> - You're imagining a **tree of possibilities** — YOUR move, THEIR move, YOUR move...
> - **Minimax:** You pick the move that's BEST for you assuming your friend plays PERFECTLY against you
> - **Alpha-Beta Pruning:** "Wait, this branch is already worse than what I found — no need to look further!" ✂️ CUT!
> - It's like a chess grandmaster thinking 10 moves ahead but SKIPPING obviously bad lines! 🚀

> 🔬 **Deep Research Insight:** IBM's **Deep Blue** defeated world chess champion Garry Kasparov in 1997 using minimax with alpha-beta pruning, evaluating **200 million positions per second**! Google's **AlphaGo** defeated Go champion Lee Sedol in 2016 using Monte Carlo Tree Search + deep learning — Go has more possible positions than atoms in the universe ($10^{170}$ vs $10^{80}$)! 🌌

### Minimax Algorithm
For two-player zero-sum games (MAX vs MIN).

**Principle:** MAX picks action maximizing utility; MIN picks action minimizing utility.

```
MINIMAX(node):
  if terminal: return utility
  if MAX's turn: return max of MINIMAX(children)
  if MIN's turn: return min of MINIMAX(children)
```

- **Complete?** Yes (if tree is finite)
- **Optimal?** Yes (against optimal opponent)
- **Time:** O(b^m)
- **Space:** O(bm)

### Alpha-Beta Pruning
Optimization of minimax that prunes branches that can't affect the final decision.

**Parameters:**
- α = best value MAX can guarantee (initially −∞)
- β = best value MIN can guarantee (initially +∞)
- **Prune when α ≥ β**

**At MAX node:** Update α = max(α, child_value). Prune if α ≥ β.
**At MIN node:** Update β = min(β, child_value). Prune if α ≥ β.

**Effectiveness:**
- **Best case (perfect ordering):** O(b^(m/2)) — effectively doubles search depth!
- **Worst case (no pruning):** O(b^m) — same as minimax
- **Average:** O(b^(3m/4))

**Move ordering:** Examining best moves first improves pruning dramatically.

---

## 6. 🔗 Bayesian Networks

> 🍼 **Beginner Analogy — The Detective's Reasoning:**
> Imagine you're a detective 🕵️ solving a case:
> - The grass is wet 🌿💧. Why? Either it **rained** 🌧️ OR the **sprinkler** was on 💦
> - You know: "It's cloudy season → 60% chance of rain" and "sprinkler runs 30% of the time"
> - If BOTH rain AND sprinkler happened, grass is 99% wet. Only rain? 90%. Only sprinkler? 80%
> - Now you see wet grass — **what's the probability it rained?** This is **Bayesian inference!** 🧠
> - Bayesian Networks let you **chain these cause-effect relationships** in a graph and reason backwards! 🔄

> 🏭 **Real-World Bayesian Networks:**
> - 🏥 **Medical Diagnosis:** Systems like **QMR-DT** model 600+ diseases and 4000+ symptoms as a Bayesian Network
> - 🚗 **Self-Driving Cars:** Estimating pedestrian behavior: P(crossing | walking toward road, green signal)
> - 📱 **Spam Filters:** P(spam | contains "free", from unknown, has link) — classic Bayesian reasoning!
> - 🎰 **Risk Assessment:** Insurance companies model claim probability using Bayesian Networks

### Joint Probability & Independence
**Product rule:** P(A,B) = P(A|B)P(B) = P(B|A)P(A)

**Bayes' Rule:**
$$P(A|B) = \frac{P(B|A)P(A)}{P(B)}$$

**Independence:** P(A,B) = P(A)P(B) ↔ P(A|B) = P(A)
**Conditional Independence:** P(A,B|C) = P(A|C)P(B|C) ↔ P(A|B,C) = P(A|C)

### Bayesian Network Structure
- **DAG (Directed Acyclic Graph)** where:
  - Nodes = random variables
  - Edges = direct dependencies
  - Each node has a CPT (Conditional Probability Table)

**Joint distribution:**
$$P(X_1,...,X_n) = \prod_{i=1}^{n}P(X_i | Parents(X_i))$$

### D-Separation
Determines conditional independence from graph structure.

**Three connection types:**

1. **Chain (Causal):** A → B → C
   - A and C are independent given B: A ⊥ C | B ✓
   - A and C are NOT independent marginally

2. **Fork (Common Cause):** A ← B → C
   - A and C are independent given B: A ⊥ C | B ✓
   - Not independent marginally

3. **Collider (V-structure):** A → B ← C
   - A and C ARE independent marginally
   - A and C are NOT independent given B (or any descendant of B): **"explaining away"**

**D-separation rule:** X and Y are d-separated given Z if every undirected path between X and Y is **blocked** by Z.
A path is blocked if it contains:
- A chain or fork node in Z, OR
- A collider node NOT in Z (and no descendant in Z)

### Markov Blanket
For node X: Parents(X) ∪ Children(X) ∪ Parents of Children(X).
X is conditionally independent of all other nodes given its Markov blanket.

### Number of Parameters
For a Boolean variable with k parents: $2^k$ entries in CPT (but only $2^k - ...$ free since they sum to 1 per parent config, so $2^k$ entries, $2^k/2$ free for binary).

Actually: for binary variable with k Boolean parents → $2^k$ rows, each with 1 free parameter → $2^k$ free parameters.

**Full joint table:** $2^n - 1$ parameters for n Boolean variables.
**Bayesian network:** $\sum_i 2^{|Parents(X_i)|}$ parameters.

---

## 7. 🎲 Probabilistic Inference

> 🍼 **Beginner Analogy — The Doctor's Reasoning:**
> A doctor 👨‍⚕️ sees a patient with fever and cough:
> - "Fever + Cough could be Flu (common) or Pneumonia (rare) or COVID (depends on exposure)"
> - The doctor combines **evidence** (symptoms) with **prior knowledge** (disease frequency) to diagnose
> - **Variable Elimination** = the doctor efficiently rules out diseases one by one instead of considering ALL combinations
> - **Sampling methods** = when the math is too complex, run thousands of simulations (like a Monte Carlo casino! 🎰)

> 🏭 **Production Applications:**
> - 🏥 **IBM Watson Health:** Uses probabilistic inference over medical literature to suggest diagnoses
> - 🚗 **Self-Driving Cars:** Fusing data from cameras, LIDAR, and radar using probabilistic inference
> - 📱 **Autocomplete/Predictive Text:** P(next word | previous words) — used by every smartphone keyboard!

### Inference by Enumeration
Compute P(query | evidence) by summing over hidden variables:

$$P(X|e) = \alpha \sum_{y} P(X, e, y)$$

where α is normalization constant, y = hidden variables.

**Exponential in number of hidden variables.**

### Variable Elimination
More efficient: eliminate hidden variables one at a time.
1. Choose an elimination ordering
2. For each hidden variable: create factor by joining relevant CPTs, then sum out the variable
3. Multiply remaining factors and normalize

**Complexity depends on elimination order** → finding optimal order is NP-hard, but good heuristics exist.

### Types of Inference
- **Prediction:** P(effect | cause) = forward, follow arrows
- **Diagnosis:** P(cause | effect) = backward, against arrows (use Bayes' rule)
- **Inter-causal:** P(cause₁ | cause₂, effect) = explaining away

### Sampling Methods (Monte Carlo)
- **Prior sampling:** Generate samples from network top-down
- **Rejection sampling:** Generate, keep only those consistent with evidence
- **Likelihood weighting:** Fix evidence, weight samples by likelihood
- **Gibbs sampling (MCMC):** Sample each non-evidence variable given its Markov blanket

---

## Worked Examples

### Example 1: Propositional Logic (Easy)
**Q:** Is (p→q)∧(q→r)→(p→r) a tautology?
**A:** This is hypothetical syllogism. Check: when premise is T and conclusion F.
p→r = F means p=T, r=F. Then q→r = F when q=T. But p→q with p=T, q=T → T.
Premise: T∧F = F. So whenever conclusion is F, premise is also F.
**Tautology** ✓

### Example 2: FOL Translation (Medium)
**Q:** "Every dog that has an owner is happy."
**A:** ∀x ((Dog(x) ∧ ∃y Owner(y,x)) → Happy(x))

### Example 3: A* Search (Medium)
**Q:** Graph with start S, goal G. h values: h(S)=6, h(A)=4, h(B)=2, h(G)=0.
Edges: S→A(1), S→B(5), A→B(2), A→G(7), B→G(1).

A* expands by f = g + h:
1. S: f=0+6=6. Expand → A(f=1+4=5), B(f=5+2=7)
2. A: f=5. Expand → B(f=1+2+2=5), G(f=1+7+0=8)
3. B (via A): f=5. Expand → G(f=3+1+0=4)
4. G: f=4. **Goal! Path: S→A→B→G, cost=4**

### Example 4: Alpha-Beta Pruning (GATE Level)
```
        MAX
       /    \
     MIN    MIN
    / \    / \
   3   5  6   2
```
Left MIN: min(3,5) = 3. α at MAX = 3.
Right MIN: first child = 6, but β=6 > α=3 → no prune yet. Second child = 2, min(6,2) = 2.
MAX: max(3, 2) = **3**.

### Example 5: Bayesian Network (GATE Level)
Network: Rain → Wet, Sprinkler → Wet.
P(Rain=T)=0.2, P(Sprinkler=T)=0.3.
P(Wet=T|R=T,S=T)=0.99, P(Wet=T|R=T,S=F)=0.9, P(Wet=T|R=F,S=T)=0.8, P(Wet=T|R=F,S=F)=0.0

P(Rain=T|Wet=T) = ?

P(Wet=T) = Σ P(W|R,S)P(R)P(S)
= 0.99×0.2×0.3 + 0.9×0.2×0.7 + 0.8×0.8×0.3 + 0.0×0.8×0.7
= 0.0594 + 0.126 + 0.192 + 0 = 0.3774

P(R=T,Wet=T) = 0.99×0.2×0.3 + 0.9×0.2×0.7 = 0.0594+0.126 = 0.1854

P(R=T|Wet=T) = 0.1854/0.3774 = **0.491**

---

## 8. 📐 Constraint Satisfaction Problems (CSPs)

### CSP Formulation

A CSP consists of:
- **Variables:** $X_1, X_2, \ldots, X_n$
- **Domains:** $D_i$ = set of possible values for $X_i$
- **Constraints:** Restrictions on variable combinations

**Example — Map Coloring:**
- Variables: WA, NT, Q, NSW, V, SA, T (Australian states)
- Domains: {Red, Green, Blue} for each
- Constraints: Adjacent states must differ in color

### Backtracking Search

```
function BACKTRACK(assignment, csp):
    if assignment is complete: return assignment
    var = SELECT-UNASSIGNED-VARIABLE(csp)
    for value in ORDER-DOMAIN-VALUES(var, assignment, csp):
        if value is consistent with assignment:
            add {var = value} to assignment
            result = BACKTRACK(assignment, csp)
            if result ≠ failure: return result
            remove {var = value} from assignment
    return failure
```

### Improving Backtracking

| Technique | Strategy | Benefit |
|---|---|---|
| **MRV (Minimum Remaining Values)** | Choose variable with fewest legal values | Fail-first: detect failures early |
| **Degree Heuristic** | Choose variable involved in most constraints | Tiebreaker for MRV |
| **LCV (Least Constraining Value)** | Choose value that eliminates fewest choices for neighbors | Succeed-first |
| **Forward Checking** | After assigning X, remove inconsistent values from X's neighbors | Early pruning |
| **Arc Consistency (AC-3)** | For every constraint arc (Xi→Xj), ensure every value in Di has support in Dj | More aggressive pruning |

### AC-3 Algorithm

```
function AC-3(csp):
    queue = all arcs in csp
    while queue not empty:
        (Xi, Xj) = REMOVE-FIRST(queue)
        if REVISE(Xi, Xj):
            if Di is empty: return false    // No solution
            for each Xk neighbor of Xi, k≠j:
                add (Xk, Xi) to queue
    return true

function REVISE(Xi, Xj):
    revised = false
    for each x in Di:
        if no y in Dj satisfies constraint(Xi, Xj):
            remove x from Di
            revised = true
    return revised
```

**Complexity:** $O(ed^3)$ where $e$ = number of arcs, $d$ = max domain size.

### Types of Consistency

| Level | Definition |
|---|---|
| **Node consistency** | Every value in $D_i$ satisfies unary constraints |
| **Arc consistency** | For every arc $(X_i, X_j)$, every value in $D_i$ has a supporting value in $D_j$ |
| **Path consistency** | For every pair of values, some consistent extension to third variable |
| **k-consistency** | Any consistent assignment to k-1 variables can be extended to k-th |

---

## 9. 🧮 Advanced Logic — SAT, Horn Clauses & FOL Resolution

### SAT Problem & Horn Clauses

**SAT:** Given a propositional formula in CNF, is there an assignment that satisfies it?
- SAT is **NP-complete** (Cook's theorem)
- 2-SAT is in **P** (polynomial)
- 3-SAT is **NP-complete**

**Horn Clauses:** A clause with **at most one positive literal**
- **Definite clause:** Exactly one positive literal: $(\neg p_1 \lor \neg p_2 \lor \ldots \lor q)$ ≡ $(p_1 \land p_2 \land \ldots) \to q$
- **Goal clause:** No positive literal: $(\neg p_1 \lor \neg p_2)$ ≡ query "is $p_1 \land p_2$ true?"
- **Fact:** Single positive literal: $q$

**Key property:** Horn-SAT is solvable in **linear time** by Forward/Backward Chaining.

### Forward Chaining (Data-Driven)

```
Start with known facts.
Repeat:
  If all premises of a rule are satisfied, conclude the head.
  Add new facts to knowledge base.
Until: query proved OR no new facts.
```

**Sound and complete** for Horn clauses. Runs in $O(n)$ time where $n$ = size of KB.

### Backward Chaining (Goal-Driven)

```
To prove query q:
  If q is a known fact: success
  For each rule with q as head:
    Try to prove all premises (recursively)
  If all premises proved: q is proved
```

Used in Prolog. Avoids irrelevant computation (only explores rules relevant to query).

### Skolemization (Eliminating ∃)

To convert FOL to clause form for resolution:

1. **Eliminate →, ↔** using equivalences
2. **Move ¬ inward** (De Morgan's, double negation)
3. **Standardize variables** (rename so each quantifier uses unique variable)
4. **Skolemize:** Replace existentially quantified variables:
   - $\exists x \, P(x)$ → $P(c)$ where $c$ is a new **Skolem constant**
   - $\forall x \exists y \, P(x,y)$ → $\forall x \, P(x, f(x))$ where $f$ is a new **Skolem function**
5. **Drop universal quantifiers** (all remaining variables are universal)
6. **Convert to CNF**

**Example:**
$\forall x [\exists y \, \text{Loves}(x, y)]$
→ Skolemize: $\forall x \, \text{Loves}(x, f(x))$ where $f$ = "the one $x$ loves"
→ Drop ∀: $\text{Loves}(x, f(x))$

### FOL Resolution & Unification

**Unification:** Find substitution $\theta$ such that two terms become identical.

| Expression 1 | Expression 2 | Unifier $\theta$ |
|---|---|---|
| $P(x)$ | $P(A)$ | $\{x/A\}$ |
| $P(x, f(x))$ | $P(A, f(A))$ | $\{x/A\}$ |
| $P(x, x)$ | $P(A, B)$ | Fails (A ≠ B) |
| $P(x, f(y))$ | $P(g(z), f(z))$ | $\{x/g(z), y/z\}$ |
| $P(x)$ | $P(f(x))$ | Fails (**occurs check**: $x$ in $f(x)$) |

**Resolution Refutation:**
1. Convert KB and ¬(query) to CNF
2. Repeatedly resolve pairs of clauses (one with positive literal, one with negative)
3. If empty clause {} derived → query is **proved** (contradiction with ¬query)

---

## 10. 📋 Planning (STRIPS)

### STRIPS Representation

| Component | Description | Example (PickUp block A) |
|---|---|---|
| **Preconditions** | What must be true before action | OnTable(A), Clear(A), HandEmpty |
| **Add effects** | What becomes true after action | Holding(A) |
| **Delete effects** | What becomes false after action | OnTable(A), Clear(A), HandEmpty |

### Blocks World Example

```
Initial state: On(A, Table), On(B, Table), On(C, A), Clear(B), Clear(C), HandEmpty

Goal: On(A, B), On(B, C)

Actions:
  PickUp(x): Pre: Clear(x), OnTable(x), HandEmpty
             Add: Holding(x)
             Del: OnTable(x), Clear(x), HandEmpty

  PutDown(x): Pre: Holding(x)
              Add: OnTable(x), Clear(x), HandEmpty
              Del: Holding(x)

  Stack(x,y): Pre: Holding(x), Clear(y)
              Add: On(x,y), Clear(x), HandEmpty
              Del: Holding(x), Clear(y)

  Unstack(x,y): Pre: On(x,y), Clear(x), HandEmpty
                Add: Holding(x), Clear(y)
                Del: On(x,y), Clear(x), HandEmpty
```

**Plan:** Unstack(C,A) → PutDown(C) → PickUp(B) → Stack(B,C) → PickUp(A) → Stack(A,B)

### Situation Calculus

Represents the world as a sequence of **situations** (states):
- $S_0$ = initial situation
- $\text{Result}(a, s)$ = situation after action $a$ in situation $s$
- **Frame axioms** describe what doesn't change (frame problem)
- Fluents: predicates that vary across situations, e.g., $\text{On}(A, B, S_0)$

---

## 11. 🔮 Hidden Markov Models (HMMs)

### HMM Definition

An HMM consists of:
- **States:** $S = \{s_1, s_2, \ldots, s_N\}$ (hidden)
- **Observations:** $O = \{o_1, o_2, \ldots, o_M\}$ (visible)
- **Transition probabilities:** $A = [a_{ij}]$, where $a_{ij} = P(q_{t+1}=s_j \mid q_t=s_i)$
- **Emission probabilities:** $B = [b_j(k)]$, where $b_j(k) = P(o_t=v_k \mid q_t=s_j)$
- **Initial distribution:** $\pi = [\pi_i]$, where $\pi_i = P(q_1 = s_i)$

### Three Fundamental Problems

| Problem | Question | Algorithm | Complexity |
|---|---|---|---|
| **Evaluation** | $P(O \mid \lambda)$ — probability of observation sequence | Forward algorithm | $O(N^2 T)$ |
| **Decoding** | Most likely state sequence given observations | Viterbi algorithm | $O(N^2 T)$ |
| **Learning** | Find best $\lambda = (A, B, \pi)$ from observations | Baum-Welch (EM) | Iterative |

### Forward Algorithm

$$\alpha_t(j) = P(o_1, o_2, \ldots, o_t, q_t = s_j \mid \lambda)$$

**Recursion:**
$$\alpha_1(j) = \pi_j \cdot b_j(o_1)$$
$$\alpha_t(j) = \left[\sum_{i=1}^{N} \alpha_{t-1}(i) \cdot a_{ij}\right] \cdot b_j(o_t)$$

**Answer:** $P(O \mid \lambda) = \sum_{j=1}^{N} \alpha_T(j)$

### Viterbi Algorithm

Same as Forward but replace $\sum$ with $\max$ and keep track of argmax for backtracking:

$$\delta_t(j) = \max_{q_1,\ldots,q_{t-1}} P(q_1,\ldots,q_{t-1}, q_t=s_j, o_1,\ldots,o_t \mid \lambda)$$

$$\delta_t(j) = \max_i[\delta_{t-1}(i) \cdot a_{ij}] \cdot b_j(o_t)$$

Backtrack from $\arg\max_j \delta_T(j)$ to find optimal state sequence.

---

## 12. 🎯 Markov Decision Processes (MDPs)

### MDP Definition

| Component | Symbol | Description |
|---|---|---|
| States | $S$ | Set of states |
| Actions | $A$ | Set of actions |
| Transition | $T(s, a, s') = P(s' \mid s, a)$ | Probability of reaching $s'$ from $s$ via $a$ |
| Reward | $R(s, a, s')$ or $R(s)$ | Immediate reward |
| Discount | $\gamma \in [0, 1)$ | How much we value future rewards |

### Bellman Equation

**Optimal value function:**
$$V^*(s) = \max_a \sum_{s'} T(s, a, s') \left[ R(s, a, s') + \gamma V^*(s') \right]$$

**Optimal policy:**
$$\pi^*(s) = \arg\max_a \sum_{s'} T(s, a, s') \left[ R(s, a, s') + \gamma V^*(s') \right]$$

### Value Iteration

```
Initialize V(s) = 0 for all s
Repeat until convergence:
    For each state s:
        V(s) ← max_a Σ_s' T(s,a,s')[R(s,a,s') + γV(s')]
```

Converges to $V^*$. Then extract policy: $\pi(s) = \arg\max_a \ldots$

### Policy Iteration

```
Initialize π randomly
Repeat until π unchanged:
    Policy Evaluation: Solve V^π(s) = Σ_s' T(s,π(s),s')[R(s,π(s),s') + γV^π(s')]
    Policy Improvement: π(s) ← argmax_a Σ_s' T(s,a,s')[R(s,a,s') + γV^π(s')]
```

**Converges in fewer iterations than Value Iteration** but each iteration is more expensive (must solve linear system).

### Value Iteration vs Policy Iteration

| Aspect | Value Iteration | Policy Iteration |
|---|---|---|
| Updates | Value function directly | Policy + value |
| Each iteration | One Bellman update per state | Full policy evaluation |
| # iterations | More | Fewer |
| Cost per iteration | Lower | Higher (solve linear system) |
| Convergence | To $V^*$ | To $\pi^*$ |

### Reinforcement Learning (Brief)

RL = solving MDPs **without knowing** $T$ and $R$ (learning from interaction).

| Algorithm | Type | Key Idea |
|---|---|---|
| **Q-Learning** | Model-free, off-policy | $Q(s,a) \leftarrow Q(s,a) + \alpha[r + \gamma \max_{a'} Q(s',a') - Q(s,a)]$ |
| **SARSA** | Model-free, on-policy | $Q(s,a) \leftarrow Q(s,a) + \alpha[r + \gamma Q(s',a') - Q(s,a)]$ |
| **Exploration** | ε-greedy | With prob ε: random action; else: greedy |

> 💡 **Q-Learning converges to optimal Q* regardless of policy used** (off-policy). SARSA converges to Q for the policy being followed.

---

## Common Mistakes

| # | Mistake | Correct |
|---|---------|---------|
| 1 | p→q means "p causes q" | p→q is only false when p=T, q=F |
| 2 | ∀x∃y = ∃y∀x | Order of quantifiers matters |
| 3 | "All S are P" = ∀x(S(x)∧P(x)) | Should be ∀x(S(x)→P(x)) |
| 4 | "Some S are P" = ∃x(S(x)→P(x)) | Should be ∃x(S(x)∧P(x)) |
| 5 | DFS is complete | Not complete in infinite spaces |
| 6 | Greedy search is optimal | Never optimal |
| 7 | A* optimal with any h | Only with admissible h |
| 8 | Collider: conditioning makes independent | Conditioning on collider CREATES dependence |
| 9 | Alpha-beta changes minimax result | Only prunes — same result |
| 10 | BFS is memory efficient | BFS uses O(b^d) space — very expensive |

---

*End of Detailed Notes — Artificial Intelligence for GATE 2027*
