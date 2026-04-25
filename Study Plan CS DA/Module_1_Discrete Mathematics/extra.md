
### 1.11 Horn Clauses & Definite Clauses (⭐ GATE + AI Overlap) 🐏

> 🏭 **Production Use:** Horn clauses are the foundation of **Prolog** programming language and **Datalog** (used in databases, static analysis tools). Google's Dremel/BigQuery query engine and Soufflé use Datalog internally!

A **Horn clause** is a clause (disjunction of literals) with **at most one positive literal**.

| Type | Form | Example |
|---|---|---|
| **Definite clause** (exactly 1 positive) | $\neg p_1 \vee \neg p_2 \vee \cdots \vee q$ | $p_1 \wedge p_2 \rightarrow q$ (rule) |
| **Fact** (definite clause, no negatives) | $q$ | "It's sunny" |
| **Goal clause** (0 positive literals) | $\neg p_1 \vee \neg p_2 \vee \cdots$ | Query: "Is it raining AND cold?" |
| **Empty clause** | $\square$ | Contradiction |

**Why Horn clauses matter for GATE:**
- Resolution on Horn clauses is **polynomial time** (vs. exponential for general resolution)
- **SAT for Horn clauses** is solvable in $O(n)$ — linear time! (unit propagation)
- This connects to **P vs NP**: Horn-SAT ∈ P, while general SAT is NP-complete
- Prolog resolution uses **SLD resolution** — a special form for definite clauses

**Converting implications to Horn clauses:**
$$p_1 \wedge p_2 \wedge \cdots \wedge p_n \rightarrow q \equiv \neg p_1 \vee \neg p_2 \vee \cdots \vee \neg p_n \vee q$$

This is a clause with exactly one positive literal ($q$) — hence a Horn clause! ✅

### 1.12 SAT Problem (Satisfiability) 🧩

> 🔥 **GATE Connection:** SAT is the FIRST problem proven NP-complete (Cook-Levin theorem, 1971). Understanding SAT connects Discrete Math to Theory of Computation!

**SAT Problem:** Given a propositional formula (usually in CNF), is there an assignment of truth values that makes it TRUE?

| Variant | Complexity | Description |
|---|---|---|
| **General SAT** | NP-complete | Any propositional formula |
| **CNF-SAT** | NP-complete | Formula in CNF |
| **3-SAT** | NP-complete | CNF with exactly 3 literals per clause |
| **2-SAT** | **P** (polynomial!) | CNF with exactly 2 literals per clause |
| **Horn-SAT** | **P** (linear!) | All clauses are Horn clauses |
| **TAUTOLOGY** | co-NP-complete | Is formula always true? |

**Key insights for GATE:**
- 2-SAT can be solved using **implication graphs + SCC** (Tarjan's algorithm) — $O(n+m)$
- $\alpha$ is unsatisfiable iff $\neg \alpha$ is a tautology
- Number of satisfying assignments of a formula with $k$ minterms in PDNF = $k$

### 1.12.1 Complexity Landscape of Logic Problems 🚦

> 🎯 GATE very often tests the **status** of a problem, not just the logic formula. So you must know which logical problems are easy, which are hard, and WHAT discrete structure makes them easy or hard.

| Problem | Input Question | Status | Core Discrete Idea |
|---|---|---|---|
| **SAT** | "Is there a satisfying assignment?" | NP-complete | Search over truth assignments |
| **3-SAT** | CNF with 3 literals/clause | NP-complete | Standard reduction source |
| **2-SAT** | CNF with 2 literals/clause | In **P** | Implication graph + SCC |
| **Horn-SAT** | Horn clauses only | In **P** | Forward chaining / unit propagation |
| **TAUT** | "Is formula always true?" | co-NP-complete | Complement of unsatisfiability |
| **UNSAT** | "Is formula impossible to satisfy?" | co-NP-complete | Refutation / empty clause |

**Decision vs Search vs Optimization ⚠️**

- **Decision:** "Does a satisfying assignment exist?" → SAT
- **Search:** "Find one satisfying assignment" → witness version of SAT
- **Optimization:** "Maximize number of satisfied clauses" → MAX-SAT

> 💡 Exam trap: A problem can be hard in optimization form but easier in decision form, or vice versa. Read the wording carefully.

**Why is 2-SAT easy?**

For each variable $x$, build implications:
- Clause $(x \vee y)$ becomes $(\neg x \rightarrow y)$ and $(\neg y \rightarrow x)$
- Formula is satisfiable iff no variable $x$ and $\neg x$ lie in the **same SCC** of the implication graph

So logic becomes a graph problem. That is a classic GATE crossover between **Logic + Graph Theory + Algorithms** 🔗

**Why are Horn formulas easy?**

- Horn clauses have at most one positive literal
- They support **deterministic forward chaining**
- Once a fact becomes true, it can trigger new facts; no exponential branching is required in the standard decision procedure

**Reduction Awareness for GATE 🧩**

- SAT is the canonical starting NP-complete problem
- 3-SAT is used to reduce to graph problems such as **Clique**, **Vertex Cover**, and **Hamiltonian Cycle**
- So if a question maps literals/clauses to vertices/edges, it is often building a complexity reduction

**Fast equivalences to remember:**

- $\alpha$ is valid iff $\neg \alpha$ is unsatisfiable
- $\alpha \models \beta$ iff $\alpha \wedge \neg \beta$ is unsatisfiable
- Testing validity can often be converted into testing unsatisfiability of a related formula

### 1.13 Consensus Theorem ✊

> **The consensus of $p \wedge q$ and $\neg p \wedge r$ is $q \wedge r$**

$$p \wedge q \vee \neg p \wedge r \vee q \wedge r \equiv p \wedge q \vee \neg p \wedge r$$

The consensus term $q \wedge r$ is **redundant** — it can be removed without changing the function!

**Dual form (for maxterms/CNF):**
$$(p \vee q) \wedge (\neg p \vee r) \wedge (q \vee r) \equiv (p \vee q) \wedge (\neg p \vee r)$$

**Application:** Used in **Quine-McCluskey algorithm** for Boolean function minimization (Digital Logic), and in finding **prime implicants**.

### 1.14 Worked Examples + PYQ-Style Traps (Logic) 🎯

#### Worked Example 1: Validity using unsatisfiability

Check whether

$$((p \rightarrow q) \wedge (q \rightarrow r) \wedge p) \rightarrow r$$

is valid.

**Method:** Show premise with negated conclusion is unsatisfiable.

Set clauses from premise and $\neg r$:

- $(\neg p \vee q)$
- $(\neg q \vee r)$
- $p$
- $\neg r$

From $(\neg q \vee r)$ and $\neg r$, infer $\neg q$. From $(\neg p \vee q)$ and $\neg q$, infer $\neg p$. But $p$ is also present. Contradiction. So formula is **valid** ✅

#### Worked Example 2: 2-SAT satisfiability by implication graph idea

Formula:

$$(x \vee y) \wedge (\neg x \vee y) \wedge (x \vee \neg y)$$

Implications:

- $(x \vee y)$ gives $\neg x \rightarrow y$, $\neg y \rightarrow x$
- $(\neg x \vee y)$ gives $x \rightarrow y$, $\neg y \rightarrow \neg x$
- $(x \vee \neg y)$ gives $\neg x \rightarrow \neg y$, $y \rightarrow x$

No variable is forced into same SCC as its negation, so satisfiable. One model is $(x,y)=(T,T)$ ✅

#### PYQ-Style Trap Box ⚠️

- "If $p$ then $q$" is equivalent to "$q$ only if $p$" ❌ (correct is "$p$ only if $q$")
- "$p$ unless $q$" means $p \vee q$ ✅ (equivalently $\neg q \rightarrow p$)
- "SAT is NP-complete" does **not** mean "UNSAT is NP-complete" ❌ (UNSAT is in co-NP)
- "If formula is satisfiable, then it is valid" ❌ (satisfiable only needs one model)
- "Every CNF formula is hard" ❌ (2-CNF and Horn-CNF have polynomial-time decision procedures)


### 4.4 Resolution — Proof by Refutation (⭐⭐ KEY TECHNIQUE) 🔬

> 🏭 **Resolution is the backbone of automated theorem provers, SAT solvers, and Prolog!**

**Resolution Principle for Propositional Logic:**
1. **Negate the conclusion** and add to premises
2. **Convert everything to CNF** (conjunction of clauses)
3. Repeatedly apply the **resolution rule**: from $(p \vee \alpha)$ and $(\neg p \vee \beta)$, derive $(\alpha \vee \beta)$
4. If the **empty clause** $\square$ (contradiction) is derived → original argument is **VALID** ✅
5. If no new clauses can be derived → argument is **INVALID** ❌

**Worked Example 🎯:**
Prove: From $p \rightarrow q$, $q \rightarrow r$, derive $p \rightarrow r$

1. Negate conclusion: $\neg(p \rightarrow r) \equiv p \wedge \neg r$
2. Premises in CNF: $\neg p \vee q$, $\neg q \vee r$
3. Add negated conclusion: $p$, $\neg r$
4. Clauses: {$\neg p \vee q$}, {$\neg q \vee r$}, {$p$}, {$\neg r$}
5. Resolve {$\neg p \vee q$} with {$p$} → {$q$}
6. Resolve {$\neg q \vee r$} with {$q$} → {$r$}
7. Resolve {$r$} with {$\neg r$} → $\square$ (empty clause!) ✅

**Therefore, $p \rightarrow r$ follows from the premises!**

> 🔥 **GATE Tip:** Resolution is **refutation-complete** — if a set of clauses is unsatisfiable, resolution WILL find the empty clause. But it's NOT complete for finding all consequences.


### 5.7.1 Herbrand Universe & Herbrand Interpretation (⭐ GATE AI Section) 🌿

> These concepts appear in GATE questions on automated reasoning and connect to Prolog/resolution.

**Herbrand Universe $H$:** The set of all **ground terms** (variable-free terms) constructible from:
- Constants in the formula (if none, add a dummy constant $a$)
- Function symbols applied to elements of $H$

**Example:** For $\forall x\, P(x, f(x))$ with constant $a$:
$H = \{a, f(a), f(f(a)), f(f(f(a))), \ldots\}$ (infinite!)

**Herbrand Interpretation:** Domain = Herbrand Universe, each constant maps to itself, each function maps to itself.

**Herbrand's Theorem:** An FOL formula is unsatisfiable iff it's false in ALL Herbrand interpretations. This reduces the problem to checking ground instances — the basis of resolution-based theorem proving!

**Key results:**
- $\forall x\, (P(x) \vee \neg P(x))$ → **Valid** (tautology regardless of P)
- $\forall x\, P(x) \rightarrow \exists x\, P(x)$ → **Valid** (if all satisfy, at least one does)
- $\exists x\, P(x) \rightarrow \forall x\, P(x)$ → **NOT Valid** (some ≠ all)
