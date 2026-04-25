# 🧮 Discrete Mathematics — Comprehensive Notes for GATE 2027 (CS/IT/DA) 🎯

> 🌟 **"Discrete Mathematics is the backbone of Computer Science — every algorithm, every database query, every network protocol, and every AI decision tree is built on these foundations!"**

> 💡 **Beginner Analogy:** Imagine you're building with LEGO blocks 🧱. Discrete Math gives you the rules for how blocks can connect, what shapes you can build, and how to count all possible creations. Unlike continuous math (smooth curves), discrete math deals with **individual, countable things** — like pixels on a screen, friends in a social network, or steps in a recipe!

---



## 📑 Table of Contents

| Part | Chapter | Topics |
|---|---|---|
| **PART 1: Mathematical Logic** 🧠 | Ch 1: Propositions & Connectives | Truth tables, Implication, Unless, Horn clauses, SAT, Consensus |
| | Ch 2: Formulas & Classifications | WFF, Tautology/Contradiction, CNF/DNF, PCNF/PDNF, Satisfiability |
| | Ch 3: Logical Equivalence | Laws, Functional Completeness, Post's Theorem, Duality |
| | Ch 4: Rules of Inference | Modus Ponens/Tollens, Resolution, Proof by Refutation |
| | Ch 5: Predicates & Quantifiers | FOL, Nested Quantifiers, PNF, Skolemization, Unification, Herbrand |
| **PART 2: Set Theory & Relations** 🌐 | Ch 6: Sets | Operations, Power Set, Countable/Uncountable, Cantor's Diagonal, Russell's Paradox |
| | Ch 7: Relations | Properties, Equivalence, POSET, Hasse, Lattice, Closure, Warshall, Dilworth, Chains/Antichains |
| | Ch 8: Functions | Injection/Surjection/Bijection, Floor/Ceiling, Composition, Fixed Points, Cardinality |
| **PART 3: Group Theory** 🏗️ | Ch 9: Groups | Semigroup→Monoid→Group, Cayley Table, Lagrange, Cyclic, Cosets, Permutation Groups, Euler's Totient, Rings/Fields |
| **PART 4: Combinatorics** 🎲 | Ch 10: Counting | P&C, Binomial Identities, Stars & Bars, PIE, Derangements, Pigeonhole, Recurrences, Generating Functions, Catalan, Stirling, Partitions, Burnside, Twelvefold Way |
| **PART 5: Graph Theory** 🌐 | Ch 11: Graphs | Degree, Types, Euler/Hamilton, Trees, Coloring, Planarity, Matching, Chromatic Polynomial, Connectivity, Directed Graphs, Line Graph, Prüfer, Turán |
| **Proof Techniques** 📝 | Ch 12: Proofs | Direct, Contrapositive, Contradiction, Induction (Weak/Strong/Structural), WOP |
| **Appendix** 🏆 | | GATE PYQ Map, Formula Reference Card, GATE Traps Master List |

---

## 🏭 Where is Discrete Math Used in the Real World?

| 🌍 Industry | 🔧 Application | 📌 DM Concept Used |
|---|---|---|
| **Google Search** 🔍 | PageRank algorithm ranks billions of pages | Graph Theory (directed graphs, random walks) |
| **Facebook/Instagram** 📱 | Friend suggestions, social circles | Graph Theory (shortest paths, clustering) |
| **Amazon** 🛒 | Recommendation engine | Relations, Set Theory, Combinatorics |
| **Uber/Ola** 🚗 | Shortest route for drivers | Graph Theory (Dijkstra's algorithm) |
| **WhatsApp** 💬 | End-to-end encryption (RSA) | Group Theory, Modular Arithmetic |
| **Netflix** 🎬 | Content delivery network optimization | Tree structures, Graph coloring |
| **Aadhaar System** 🇮🇳 | Unique ID verification | Functions (bijections), Set Theory |
| **Railway Reservation (IRCTC)** 🚂 | Seat allocation, scheduling | Combinatorics, Graph coloring |
| **Compiler Design** ⚙️ | Syntax trees, grammar parsing | Trees, Formal Logic |
| **Database Systems (SQL)** 🗄️ | Query optimization, joins | Set operations, Relations |

---

## 📊 GATE Weightage & Strategy

| Parameter | Details |
|---|---|
| **Expected Marks** | 8–15 marks every year (3–5 questions across subtopics) |
| **Frequency** | Multiple questions every single year — highest weightage in Engineering Math |
| **Difficulty** | Easy to Hard — wide range |
| **Time to Master** | 6–8 weeks (first pass) + 2 weeks revision |
| **ROI** | Extremely High — broad syllabus but predictable question patterns |

---

# 🧠 PART 1: MATHEMATICAL LOGIC

> 🎭 **Think of Logic as the "Grammar of Reasoning"** — just like English has rules for forming correct sentences, Logic has rules for forming correct arguments. Every `if-else` statement you write in code, every SQL `WHERE` clause, every circuit in your computer — all powered by logic!

---

> 🏭 **Production Use Case:** Every time you write `if (age >= 18 && hasLicense == true)` in your code, you're using propositional logic! Google's search filters, Amazon's product filters, and Netflix's recommendation rules all use Boolean logic at their core.

---

## Chapter 1: Propositions and Logical Connectives 🔗

### 1.1 What is a Proposition? 🤔

> 💡 **Beginner Analogy — The Yes/No Game 🎮:** Imagine playing a game where you can only answer "Yes" (True) or "No" (False). A proposition is any statement that fits this game — it MUST have a definite answer. "Is the sky blue?" ✅ works. "What's your name?" ❌ doesn't work because it's not yes/no!

A **proposition** (or statement) is a declarative sentence that is either **TRUE** or **FALSE**, but **not both**.

**Examples of propositions:**
- "2 + 3 = 5" → TRUE
- "Delhi is the capital of France" → FALSE
- "Every even number greater than 2 is the sum of two primes" → TRUE or FALSE (Goldbach's conjecture — we don't know, but it still has a truth value)

**NOT propositions:**
- "What time is it?" (question)
- "Close the door!" (command)
- "x + 1 = 5" (open sentence — truth depends on x; this is a predicate, not a proposition)
- "This statement is false" (paradox — neither true nor false)

**Real-world application:** Propositions form the backbone of **Boolean logic in circuits**, **SQL WHERE clauses**, **if-else conditions in programming**, and **formal verification** of software systems.

> 🌍 **Real-World Spotlight — How Google Uses Propositions:**
> When you search on Google with filters like `"Python" AND "tutorial" NOT "paid"`, each word/phrase is a proposition, and AND/NOT are logical connectives! Google's entire search infrastructure processes **billions** of such Boolean queries every day. The same logic powers Elasticsearch, Apache Solr, and every database query engine in production today.

> 🏗️ **Industry Insight — Formal Verification at Intel:**
> After the infamous **Pentium FDIV bug** (1994) that cost Intel $475 million 💸, they started using **propositional logic** and **model checking** to formally verify chip designs. Every new Intel processor is now verified using billions of logical propositions to ensure correctness before manufacturing!

### 1.2 Propositional Variables

We use letters like $p, q, r, s$ to denote propositional variables. Each can be assigned T (True) or F (False).

### 1.3 Atomic vs Compound Propositions

- **Atomic proposition:** Cannot be broken down further (e.g., "It is raining")
- **Compound proposition:** Built from atomic propositions using **logical connectives** (e.g., "It is raining AND it is cold")

### 1.4 Logical Connectives (⭐ FREQUENTLY ASKED)

| Connective | Symbol | Name | English | Truth Condition |
|---|---|---|---|---|
| NOT | $\neg p$ | Negation | "not p" | Opposite of $p$ |
| AND | $p \wedge q$ | Conjunction | "p and q" | True only when both true |
| OR | $p \vee q$ | Disjunction | "p or q" | False only when both false |
| XOR | $p \oplus q$ | Exclusive OR | "p or q but not both" | True when exactly one is true |
| IMPLICATION | $p \rightarrow q$ | Conditional | "if p then q" | False only when p=T, q=F |
| BI-IMPLICATION | $p \leftrightarrow q$ | Biconditional | "p if and only if q" | True when both same |
| NAND | $p \uparrow q$ | Sheffer stroke | "not (p and q)" | False only when both true |
| NOR | $p \downarrow q$ | Pierce arrow | "not (p or q)" | True only when both false |

### 1.5 Truth Tables of All Connectives 📋

> 💡 **Beginner Analogy — The Light Switch Board 💡🔌:**
> Imagine a room with two light switches (p and q):
> - **AND (∧):** Room lights up ONLY when BOTH switches are ON 🔆🔆 = 💡
> - **OR (∨):** Room lights up when ANY switch is ON 🔆 = 💡
> - **XOR (⊕):** Room lights up when EXACTLY ONE switch is ON (like a staircase light — flip either switch to toggle!)
> - **NAND (↑):** Used in EVERY computer chip! Intel, AMD, Apple's M-series — all built from NAND gates because NAND alone can create ANY other gate. This is why it's called a **"universal gate"** 🏭

| $p$ | $q$ | $\neg p$ | $p \wedge q$ | $p \vee q$ | $p \oplus q$ | $p \rightarrow q$ | $p \leftrightarrow q$ | $p \uparrow q$ | $p \downarrow q$ |
|---|---|---|---|---|---|---|---|---|---|
| T | T | F | T | T | F | T | T | F | F |
| T | F | F | F | T | T | **F** | F | T | F |
| F | T | T | F | T | T | T | F | T | F |
| F | F | T | F | F | F | T | T | T | T |

### 1.6 Implication — The Most Tricky Connective (⭐⭐ CRITICAL) 🎭

> 💡 **Beginner Analogy — The Promise 🤝:**
> Think of implication as a **promise**. "If you score 90+, I'll buy you a phone" 📱
> - You score 90+, I buy phone → Promise KEPT ✅
> - You score 90+, I DON'T buy → Promise BROKEN ❌ (the ONLY way implication is FALSE!)
> - You score below 90, I buy phone anyway → Promise NOT broken ✅ (I was generous!)
> - You score below 90, I don't buy → Promise NOT broken ✅ (it was never triggered!)
>
> The last two are **"vacuously true"** — the condition was never met, so the promise can't be broken! This is the #1 source of confusion in GATE exams! 🚨

$p \rightarrow q$ reads "if p then q" or "p implies q"

- $p$ = **hypothesis/antecedent/premise**
- $q$ = **conclusion/consequent**

**Key insight:** The implication is FALSE only when the **premise is true but conclusion is false**. When the premise is false, the implication is **vacuously true** (true by default).

### 1.7 Related Statements from an Implication

| Original | Form | Notation |
|---|---|---|
| Implication | $p \rightarrow q$ | "If p then q" |
| Converse | $q \rightarrow p$ | "If q then p" |
| Inverse | $\neg p \rightarrow \neg q$ | "If not p then not q" |
| Contrapositive | $\neg q \rightarrow \neg p$ | "If not q then not p" |

> ⭐ **Critical:** $p \rightarrow q \equiv \neg q \rightarrow \neg p$ (original ≡ contrapositive)  
> Also: Converse ≡ Inverse (they are contrapositives of each other)  
> But: Original $\not\equiv$ Converse (common trap!)

### 1.8 Conditional Variants & English Translations Cheat Sheet 📖

> 🎯 **GATE loves testing English-to-logic translations. Master these patterns!**

| English Phrase | Logical Form |
|---|---|
| "If p then q" | $p \rightarrow q$ |
| "p implies q" | $p \rightarrow q$ |
| "p only if q" | $p \rightarrow q$ |
| "q if p" | $p \rightarrow q$ |
| "p is sufficient for q" | $p \rightarrow q$ |
| "q is necessary for p" | $p \rightarrow q$ |
| "q whenever p" | $p \rightarrow q$ |
| "p unless q" | $\neg q \rightarrow p \equiv p \vee q$ |
| "Neither p nor q" | $\neg p \wedge \neg q \equiv \neg(p \vee q)$ |
| "p but not q" | $p \wedge \neg q$ |
| "p or q but not both" | $p \oplus q$ |
| "p if and only if q" | $p \leftrightarrow q$ |
| "p is necessary and sufficient for q" | $p \leftrightarrow q$ |
| "Not both p and q" | $\neg(p \wedge q) \equiv \neg p \vee \neg q$ |

### 1.9 Unless Word (⭐ GATE FAVORITE)

> **"p unless q"** translates to: $\neg q \rightarrow p$ or equivalently $p \vee q$  
Which derives: $\neg q \rightarrow p \equiv \neg p \rightarrow q$

**Example:** "You will fail unless you study" = "If you don't study, you will fail" = $\neg S \rightarrow F \equiv F \vee S$

### 1.10 Necessary and Sufficient Conditions

- "$p$ is **sufficient** for $q$" means $p \rightarrow q$
- "$p$ is **necessary** for $q$" means $q \rightarrow p$ (equivalently $\neg p \rightarrow \neg q$)
- "$p$ is **necessary and sufficient** for $q$" means $p \leftrightarrow q$

---

## Chapter 2: Propositional Formulas & Classifications 📝

> 🏭 **Production Scenario — Circuit Design at TSMC/Samsung:**
> When engineers at TSMC (the company that makes chips for Apple, NVIDIA, AMD) design circuits, they write Boolean formulas for every gate combination. The formula MUST be a valid Well-Formed Formula — just like code must compile! Invalid formulas = chip design errors = millions of dollars wasted 💸.

### 2.1 Well-Formed Formulas (WFF) ✅

Rules for building valid formulas:
1. Every propositional variable is a WFF
2. If $\alpha$ is WFF, then $\neg \alpha$ is WFF
3. If $\alpha, \beta$ are WFF, then $(\alpha \wedge \beta)$, $(\alpha \vee \beta)$, $(\alpha \rightarrow \beta)$, $(\alpha \leftrightarrow \beta)$ are WFF

### 2.2 Operator Precedence

| Priority | Operator |
|---|---|
| 1 (highest) | $\neg$ |
| 2 | $\wedge$ |
| 3 | $\vee$ |
| 4 | $\rightarrow$ |
| 5 (lowest) | $\leftrightarrow$ |

So $p \vee q \wedge r \rightarrow s$ means $p \vee (q \wedge r) \rightarrow s$ which means $(p \vee (q \wedge r)) \rightarrow s$

> **Note:** $\rightarrow$ is **right associative**: $p \rightarrow q \rightarrow r$ means $p \rightarrow (q \rightarrow r)$

### 2.3 Classification of Formulas (⭐ FREQUENTLY ASKED) 🏷️

> 💡 **Beginner Analogy — Weather Predictions 🌦️:**
> - **Tautology** = "Tomorrow it will either rain or not rain" — Always TRUE no matter what! Like saying "you'll either pass GATE or not" 😄
> - **Contradiction** = "It's raining AND not raining at the same time" — IMPOSSIBLE! Like saying "I scored 100% and 0% simultaneously" 🤯
> - **Contingency** = "It will rain tomorrow" — Could be true or false, depends on actual weather! Most real statements are contingencies.

| Type | Definition | Example |
|---|---|---|
| **Tautology** ✅ | Always TRUE for all assignments | $p \vee \neg p$ |
| **Contradiction** ❌ | Always FALSE for all assignments | $p \wedge \neg p$ |
| **Contingency** 🔄 | Sometimes TRUE, sometimes FALSE | $p \vee q$ |

**Key results:**
- Negation of tautology = contradiction
- Negation of contradiction = tautology
- Tautology ∧ anything = anything
- Tautology ∨ anything = tautology
- Contradiction ∧ anything = contradiction
- Contradiction ∨ anything = anything

### 2.4 Normal Forms — CNF & DNF (⭐⭐ ASKED ALMOST EVERY YEAR)

> 🏭 **Why Normal Forms Matter:** Every SAT solver (used in hardware verification at Intel, AMD), every circuit minimization tool (Karnaugh maps, Quine-McCluskey), and every automated theorem prover works by converting formulas into normal forms.

> 💡 **Decision procedures for tautology checking:**
> 1. **Truth table method** — Enumerate all $2^n$ rows. Exponential but guaranteed. 📋
> 2. **Simplification using laws** — Apply equivalence laws to reduce. Requires skill. 🧠
> 3. **Resolution** — Negate the formula, convert to CNF, derive empty clause. ⚡
> 4. **Semantic Tableaux (Truth Trees)** — Systematic decomposition. Used in automated provers. 🌳
> 5. **BDD (Binary Decision Diagrams)** — Canonical representation used in model checking. 🔀

**Terminology:**
- **Literal:** A propositional variable or its negation. E.g., $p$, $\neg q$
- **Clause:** A disjunction (OR) or conjunction (AND) of literals

**Conjunctive Normal Form (CNF) — Product of Sums:**

A formula is in **CNF** if it is a **conjunction of disjunctions** (AND of ORs):

$$(l_1 \vee l_2 \vee \cdots) \wedge (l_3 \vee l_4 \vee \cdots) \wedge \cdots$$

**Example:** $(p \vee \neg q) \wedge (\neg p \vee q \vee r) \wedge (q \vee \neg r)$. Each parenthesized group is a **clause (maxterm).**

**Disjunctive Normal Form (DNF) — Sum of Products:**

A formula is in **DNF** if it is a **disjunction of conjunctions** (OR of ANDs):

$$(l_1 \wedge l_2 \wedge \cdots) \vee (l_3 \wedge l_4 \wedge \cdots) \vee \cdots$$

**Example:** $(p \wedge \neg q) \vee (\neg p \wedge q \wedge r) \vee (q \wedge \neg r)$. Each parenthesized group is a **minterm.**

**Conversion to CNF/DNF — Step-by-step method:**
1. Eliminate $\rightarrow$ and $\leftrightarrow$: ($p \rightarrow q \equiv \neg p \vee q$; $p \leftrightarrow q \equiv (p \rightarrow q) \wedge (q \rightarrow p)$)
2. Push negations inward using De Morgan's laws until negation applies only to variables
3. Apply distributive laws:
   - For CNF: distribute $\vee$ over $\wedge$: $p \vee (q \wedge r) \equiv (p \vee q) \wedge (p \vee r)$
   - For DNF: distribute $\wedge$ over $\vee$: $p \wedge (q \vee r) \equiv (p \wedge q) \vee (p \wedge r)$

### 2.5 Principal/Canonical Normal Forms — PCNF & PDNF (⭐⭐ GATE FAVORITE)

**PDNF (Principal Disjunctive Normal Form):** Every minterm contains ALL propositional variables (each exactly once, either complemented or uncomplemented).

**PCNF (Principal Conjunctive Normal Form):** Every maxterm contains ALL propositional variables (each exactly once).

**Minterms and Maxterms for 2 variables ($p, q$):**

| Row | $p$ | $q$ | Minterm ($m_i$) | Maxterm ($M_i$) |
|---|---|---|---|---|
| 0 | F | F | $\neg p \wedge \neg q$ | $p \vee q$ |
| 1 | F | T | $\neg p \wedge q$ | $p \vee \neg q$ |
| 2 | T | F | $p \wedge \neg q$ | $\neg p \vee q$ |
| 3 | T | T | $p \wedge q$ | $\neg p \vee \neg q$ |

**Key Relationships:**
- PDNF = OR of minterms where formula is TRUE
- PCNF = AND of maxterms where formula is FALSE
- $m_i = \neg M_i$ (minterm is negation of corresponding maxterm)

**Reading from Truth Table (⭐ QUICK GATE TRICK):**
1. **PDNF:** Look at rows where output = T. Write the minterm for each. OR them together.
2. **PCNF:** Look at rows where output = F. Write the maxterm for each. AND them together.

> **Minterm convention:** Variable appears UN-negated if its value is T in that row, negated if F.
> **Maxterm convention:** Variable appears NEGATED if its value is T in that row, UN-negated if F. (Opposite!)

**GATE-Level Example:** Find PCNF and PDNF of $p \rightarrow q$

Truth table: $p \rightarrow q$ is FALSE only when $p = T, q = F$ (row 2).

- PDNF = $m_0 \vee m_1 \vee m_3 = (\neg p \wedge \neg q) \vee (\neg p \wedge q) \vee (p \wedge q)$
- PCNF = $M_2 = \neg p \vee q$

**Counting with normal forms:**
- A formula in PDNF with $k$ minterms corresponds to exactly one Boolean function
- No two distinct PDNFs represent the same function (PDNF is **canonical/unique**)
- CNF/DNF are NOT unique; only PCNF/PDNF are canonical

### 2.6 Satisfiability, Validity & Number of Models

- A satisfiable formula is one that is true for at least one row in its truth table.
- An unsatisfiable formula is false for all rows (a contradiction).
- Formula $\alpha$ is **valid (tautology)** iff $\neg \alpha$ is **unsatisfiable**
- Formula $\alpha$ is **satisfiable** iff $\neg \alpha$ is **not valid**
- If $\alpha$ has $n$ variables and is satisfiable, it has between 1 and $2^n$ satisfying assignments (models)
- If PDNF of a formula on $n$ variables has $k$ minterms → the formula has exactly $k$ models out of $2^n$ total

**Entailment (Logical Consequence):**
$\alpha \models \beta$ ($\alpha$ entails $\beta$) iff every model of $\alpha$ is also a model of $\beta$. Equivalent to: $\alpha \rightarrow \beta$ is a tautology.

### 2.7 Counting Formulas
a way to calculate how many different logical possibilities exist for a given number of variables

For $n$ propositional variables:
- Total possible truth table rows: $2^n$
- Total possible Boolean functions: $2^{2^n}$ (Number of distinct truth tables)  
Eg: If n = 2 → 4 rows → each row has 2 choices → Total = $2^{2^2}$ = 16 = 16 possible logical formulas
- Number of tautologies: 1 (all T in output column)
- Number of contradictions: 1 (all F in output column)
- Number of contingencies: $2^{2^n} - 2$
- Number of satisfiable formulas: $2^{2^n} - 1$

---

## Chapter 3: Logical Equivalence (⭐⭐ MOST IMPORTANT) ⚖️

> 🌍 **Real-World Connection — Query Optimization in Databases 🗄️:**
> When you write a SQL query, the database engine uses logical equivalence laws to **rewrite your query** into a faster version! For example:
> ```sql
> -- Your query:
> SELECT * FROM users WHERE NOT (age < 18 AND country = 'India')
> -- Database rewrites using De Morgan's Law:
> SELECT * FROM users WHERE age >= 18 OR country != 'India'
> ```
> Both queries return IDENTICAL results, but the rewritten version might run **10x faster** using available indexes! This is exactly what PostgreSQL, MySQL, and Oracle do behind the scenes using De Morgan's Law. 🚀

### 3.1 Definition

Two formulas $\alpha$ and $\beta$ are **logically equivalent** (written $\alpha \equiv \beta$) if they have **identical truth tables**.

> 💡 **Beginner Analogy — Different Routes, Same Destination 🗺️:**
> Logical equivalence is like having two different Google Maps routes to the same place. "Take the highway then turn left" and "Go through the city then turn right" — different instructions, same destination! In math, $\neg p \vee q$ and $p \rightarrow q$ look different but ALWAYS give the same answer.

### 3.2 Important Logical Laws

| Law | Formula |
|---|---|
| **Identity** | $p \wedge T \equiv p$, $p \vee F \equiv p$ |
| **Domination** | $p \vee T \equiv T$, $p \wedge F \equiv F$ |
| **Idempotent** | $p \wedge p \equiv p$, $p \vee p \equiv p$ |
| **Double Negation** | $\neg(\neg p) \equiv p$ |
| **Commutative** | $p \wedge q \equiv q \wedge p$, $p \vee q \equiv q \vee p$ |
| **Associative** | $(p \wedge q) \wedge r \equiv p \wedge (q \wedge r)$ |
| **Distributive** | $p \wedge (q \vee r) \equiv (p \wedge q) \vee (p \wedge r)$ |
| | $p \vee (q \wedge r) \equiv (p \vee q) \wedge (p \vee r)$ |
| **De Morgan's** | $\neg(p \wedge q) \equiv \neg p \vee \neg q$ |
| | $\neg(p \vee q) \equiv \neg p \wedge \neg q$ |
| **Absorption** | $p \vee (p \wedge q) \equiv p$, $p \wedge (p \vee q) \equiv p$ |
| **Implication** | $p \rightarrow q \equiv \neg p \vee q$ |
| **Contrapositive** | $p \rightarrow q \equiv \neg q \rightarrow \neg p$ |
| **Biconditional** | $p \leftrightarrow q \equiv (p \rightarrow q) \wedge (q \rightarrow p)$ |
| **XOR** | $p \oplus q \equiv (p \vee q) \wedge \neg(p \wedge q)$ |
| | $p \oplus q \equiv \neg(p \leftrightarrow q)$ |

### 🔗 Additional Important Equivalences (⭐ GATE Tested)

| Law | Formula |
|---|---|
| **Exportation** 🚀 | $(p \wedge q) \rightarrow r \equiv p \rightarrow (q \rightarrow r)$ |
| **Material Implication** | $p \rightarrow q \equiv \neg p \vee q$ |
| **Negation of Implication** | $\neg(p \rightarrow q) \equiv p \wedge \neg q$ |
| **Negation of Biconditional** | $\neg(p \leftrightarrow q) \equiv p \oplus q$ |
| **Implication as Disjunction** | $p \rightarrow q \equiv \neg q \rightarrow \neg p \equiv \neg p \vee q$ |
| **Biconditional Expansion** | $p \leftrightarrow q \equiv (p \wedge q) \vee (\neg p \wedge \neg q)$ |
| **XOR Properties** | $p \oplus p \equiv F$, $p \oplus \neg p \equiv T$, $p \oplus F \equiv p$, $p \oplus T \equiv \neg p$ |
| **XOR Associativity** | $(p \oplus q) \oplus r \equiv p \oplus (q \oplus r)$ |
| **Consensus** | $pq \vee \neg p r \vee qr \equiv pq \vee \neg p r$ |
| **Resolution** | $(p \vee q) \wedge (\neg p \vee r) \Rightarrow q \vee r$ |

### 🧮 Boolean Algebra ↔ Propositional Logic Correspondence

> **Every propositional logic equivalence has a Boolean algebra counterpart!** This connects directly to Digital Logic questions in GATE.

| Propositional Logic | Boolean Algebra | Set Theory |
|---|---|---|
| $T$ (True) | $1$ | $U$ (Universal set) |
| $F$ (False) | $0$ | $\emptyset$ (Empty set) |
| $\neg p$ | $\overline{A}$ (complement) | $A'$ |
| $p \wedge q$ | $A \cdot B$ (AND) | $A \cap B$ |
| $p \vee q$ | $A + B$ (OR) | $A \cup B$ |
| $p \oplus q$ | $A \oplus B$ (XOR) | $A \triangle B$ |

> 🎯 **GATE Insight:** Questions may test the SAME concept across different representations. Recognizing this correspondence saves time!

### 3.3 Simplification Techniques

**Method 1: By Case Method**
- If we can show a formula is TRUE when p=T and also when p=F, it's a tautology
- Substitute p=T and p=F separately and simplify

**Method 2: Using Laws**
- Apply equivalence laws step by step

**Example (GATE Style):** Simplify $(p \rightarrow q) \wedge (p \rightarrow r)$

$$= (\neg p \vee q) \wedge (\neg p \vee r) = \neg p \vee (q \wedge r) = p \rightarrow (q \wedge r)$$

**Common Mistakes:**
1. ❌ Confusing $\rightarrow$ with $\leftrightarrow$
2. ❌ Thinking $p \rightarrow q \equiv q \rightarrow p$ (this is WRONG — that's the converse)
3. ❌ Forgetting that $\rightarrow$ is right-associative
4. ❌ De Morgan's: forgetting to flip AND ↔ OR when negating

### 3.4 Functional Completeness (⭐⭐⭐ ASKED VERY FREQUENTLY IN GATE)

A set of connectives $S$ is **functionally complete** if every Boolean function can be expressed using ONLY the connectives in $S$.

> 🔥 **Intuition — Think of it like**:
> You want to build any circuit / logical formula. If your set of operators is enough to build ALL possible truth tables, it is functionally complete

**Known Functionally Complete Sets (MUST MEMORIZE):**

| Set | Complete? | Why |
|---|---|---|
| $\{\neg, \wedge\}$ | ✅ YES | $p \vee q \equiv \neg(\neg p \wedge \neg q)$ |
| $\{\neg, \vee\}$ | ✅ YES | $p \wedge q \equiv \neg(\neg p \vee \neg q)$ |
| $\{\neg, \rightarrow\}$ | ✅ YES | $p \vee q \equiv \neg p \rightarrow q$ |
| $\{\uparrow\}$ (NAND alone) | ✅ YES | $\neg p \equiv p \uparrow p$; $p \wedge q \equiv (p \uparrow q) \uparrow (p \uparrow q)$ |
| $\{\downarrow\}$ (NOR alone) | ✅ YES | $\neg p \equiv p \downarrow p$; $p \vee q \equiv (p \downarrow q) \downarrow (p \downarrow q)$ |
| $\{\wedge, \vee\}$ | ❌ NO | Cannot express $\neg$ |
| $\{\rightarrow\}$ alone | ❌ NO | Cannot generate $\neg p$ from $p$ alone |
| $\{\vee, \rightarrow\}$ | ❌ NO | Cannot negate |
| $\{\wedge, \vee, \rightarrow, \leftrightarrow\}$ | ❌ NO | None of these can flip T→F when all inputs are T |

### 3.5 Post's Theorem — Systematic Test for Functional Completeness (⭐⭐)

A set $S$ of Boolean functions is functionally complete iff $S$ is NOT a subset of any of these five **closed classes (Post's classes)**:

| Class | Name | Preserved Property | Limitation |
|---|---|---|---|
| $T_0$ | 0/false-preserving | $f(0,0,\ldots,0) = 0$ | Can't produce 1 from all 0s |
| $T_1$ | 1/truth-preserving | $f(1,1,\ldots,1) = 1$ | Can't produce 0 from all 1s |
| $S$ | Self-dual | $f(\neg x_1, \ldots, \neg x_n) = \neg f(x_1, \ldots, x_n)$ | Symmetry restriction |
| $M$ | Monotone | $x \leq y \Rightarrow f(x) \leq f(y)$ | Cannot invert |
| $L$ | Linear (affine) | $f = a_0 \oplus a_1 x_1 \oplus \cdots \oplus a_n x_n$ | Too linear |

**To check:** For EACH of the 5 classes, verify that $S$ contains at least one function NOT in that class.

---

#### Deep Focus: $S$, $M$, and $L$ Classes (Most Confusing Part)

### A) Self-Dual Class $S$ (check Duality Principle for more details)

**Definition (Formal):**

A Boolean function $f(x_1, x_2, \ldots, x_n)$ is self-dual if:

$$
f(\neg x_1, \ldots, \neg x_n) = \neg f(x_1, \ldots, x_n)
$$

**Meaning (Very Important):**
- Flip all inputs → output must also flip.

**Intuition:**
- Input row and its complement row must have opposite outputs.
- Perfect complement symmetry in the truth table.

**Example 1 (Self-dual):**

Let $f(A) = A$.

Check:
- LHS: $f(\neg A) = \neg A$
- RHS: $\neg f(A) = \neg A$

LHS = RHS, so $f(A)=A$ is self-dual. ✅

**Example 2 (Not self-dual):**

Let $f(A,B)=A \wedge B$.

Check:
- LHS: $f(\neg A,\neg B)=\neg A \wedge \neg B$
- RHS: $\neg(A\wedge B)=\neg A \vee \neg B$

Not equal, so NOT self-dual. ❌

**Important Properties:**
- In a self-dual truth table, outputs come in complementary pairs.
- Number of self-dual functions on $n$ variables:
$$
2^{2^{n-1}}
$$
- Example for $n=2$: self-dual functions $=2^{2}=4$. ($A$, $\neg A$, $B$, $\neg B$)
- Common self-dual examples on $A,B,C$: $A$, $\neg A$, $B$, $\neg B$, $C$, $\neg C$, $A\oplus B\oplus C$, $\neg(A\oplus B\oplus C)$, $\operatorname{Maj}(A,B,C)$, $\neg\operatorname{Maj}(A,B,C)$.
- Why: There are $2^n$ input rows, grouped into $2^{n-1}$ complement pairs $(x,\neg x)$. Choose output for one row in each pair freely; the other is forced to be opposite.

**Do Not Confuse:**
- **Dual of an expression**: swap $\wedge \leftrightarrow \vee$ (and 0/1 constants).
- **Self-dual function**: input complement implies output complement.

**Quick Test Trick:**
1. Pick any row $x$.
2. Find complement row $\neg x$.
3. Check outputs are opposite.

---

### B) Monotone Class $M$

**Definition (Formal):**

A Boolean function $f$ is monotone if:

$$
x \le y \Rightarrow f(x) \le f(y)
$$

where $x \le y$ means each bit of $x$ is less than or equal to corresponding bit of $y$.

**Meaning:**
- If you change inputs from 0 to 1, output cannot go from 1 to 0.

**Intuition:**
- "More true inputs" cannot make the output "less true."

**Monotone examples:**
- $A \wedge B$
- $A \vee B$
- Majority function

**Non-monotone examples:**
- $\neg A$
- $A \oplus B$
- $A \rightarrow B$ (fails monotonicity in $A$)

**Important Point for Post's theorem:**
- If all operators in your set are monotone, you can never build negation behavior.
- So such a set cannot be functionally complete.

**Quick Test Trick:**
- Try one pair like $(0,1)$ and $(1,1)$ or $(0,0)$ and $(1,0)$.
- If output drops from 1 to 0 after increasing an input, function is not monotone.

---

### C) Linear/Affine Class $L$

**Definition (Formal):**

$$
f(x_1,\ldots,x_n)=a_0 \oplus a_1x_1 \oplus \cdots \oplus a_nx_n,
\quad a_i\in\{0,1\}
$$

These are exactly XOR-combinations of variables plus an optional constant.

**Meaning:**
- No AND-type interaction terms like $x_1x_2$ are allowed.

**Examples in $L$:**
- $A$
- $\neg A = A \oplus 1$
- $A \oplus B$
- $A \oplus B \oplus 1$

**Examples not in $L$:**
- $A \wedge B$
- $A \vee B$
- $A \rightarrow B$

**Important Property:**
- Number of affine (linear) Boolean functions on $n$ variables:
$$
2^{n+1}
$$

For $n=2$: $2^{3}=8$ affine functions.
- $0$
- $1$
- $A$
- $B$
- $A \oplus B$
- $A \oplus 1 = \neg A$
- $B \oplus 1 = \neg B$
- $A \oplus B \oplus 1 = \neg(A \oplus B)$

**Quick Test Trick:**
- If expression requires product/interactions (like $AB$), it is not linear.
- If expression can be written using only XOR and constant 1/0, it is linear.

---

🎯 **Mini Summary (Memory Line):**
- **Self-dual:** flip all inputs, output flips.
- **Monotone:** raising inputs cannot lower output.
- **Linear:** XOR + constant only, no AND interaction.

**Quick classification of basic connectives:**

| Connective | $T_0$? | $T_1$? | Self-dual? | Monotone? | Linear? |
|---|---|---|---|---|---|
| $\neg$ | No | No | Yes | No | Yes |
| $\wedge$ | Yes | Yes | No | Yes | No |
| $\vee$ | Yes | Yes | No | Yes | No |
| $\rightarrow$ | No | Yes | No | No | No |
| $\oplus$ | Yes | No | No | No | Yes |
| $\uparrow$ (NAND) | No | No | No | No | No |
| $\downarrow$ (NOR) | No | No | No | No | No |
| $\leftrightarrow$ | No | Yes | No | No | Yes |

**Example (GATE PYQ pattern):** Is $\{\vee, \oplus\}$ functionally complete?
- $T_0$: $\vee \in T_0$ ✓, $\oplus \in T_0$ ✓ → All in $T_0$ → $S \subseteq T_0$ → **NOT complete** ❌

**Example:** Is $\{\rightarrow, \oplus\}$ functionally complete?
- $T_0$: $\rightarrow \notin T_0$ ✓ | $T_1$: $\oplus \notin T_1$ ✓ | Self-dual: both not self-dual ✓ | Monotone: $\rightarrow$ not monotone ✓ | Linear: $\rightarrow$ not linear ✓
- All 5 classes escaped → **YES, functionally complete** ✅

### 3.6 Duality Principle

The **dual** of a formula is obtained by swapping $\wedge \leftrightarrow \vee$ and $T$(true/1) $\leftrightarrow F$(false/0) (constants only, don't touch $\neg$).

- If $\alpha \equiv \beta$, then $\alpha^d \equiv \beta^d$ (If two Boolean expressions α and β are equivalent, then their duals are also equivalent. **Note:** $\alpha \not\equiv \alpha^d$)
- The dual of a tautology is a contradiction (and vice versa)

- $\neg \alpha \equiv \alpha^{d}(\neg p_1, \neg p_2, \ldots)$ (**What it Means**: To find the negation of a Boolean expression (α), take the dual of α. Then replace every variable with its negation.)  
   **Example:**
   | Step | Formula |
   |---|---|
   | Let | $\alpha = A \wedge B$ |
   | Step 1: Dual | $\alpha^d = A \vee B$ |
   | Step 2: Replace variables | $\alpha^d(\neg A, \neg B) = \neg A \vee \neg B$ |
   | Final | $\neg(A \wedge B) = \neg A \vee \neg B$ (De Morgan's Law) |


**Why duality works:** Because Boolean algebra is symmetrical in structure. AND and OR behave like “mirror operations”. 0 and 1 are also opposites. So every law has a mirror version

**Example**:
| Original | Dual |
|---|---|
| A ∧ 1 = A | A ∨ 0 = A |
| A ∧ 0 = 0 | A ∨ 1 = 1 |
| A ∧ (B ∨ C) = (A ∧ B) ∨ (A ∧ C) | A ∨ (B ∧ C) = (A ∨ B) ∧ (A ∨ C) |


🎯 **Final Summary**
- Instead of memorizing all Boolean laws: Learn half the laws. Generate the other half using duality
- Duality = swap AND/OR and 0/1. Variables remain same
- If original is true → dual is also true
- Duality applies to: Entire expression, All constants, Both sides of equation (eg: Dual of (A ∧ B) ∨ 1 = C is not (A ∨ B) ∧ 0 = C because C unchanged, which breaks duality)
- for n variables, number of self-dual functions is: $2^{2^{(n-1)}}$  
Example:  
Number of self-dual Boolean functions on 1 variable = 2  
Identity function: $f(x)$  
Negation function: $\neg f(x)$

---

## Chapter 4: Logical Arguments and Rules of Inference (⭐ GATE IMPORTANT) 🧑‍⚖️

> 💡 **Beginner Analogy — The Detective's Reasoning 🕵️:**
> Think of logical arguments like a detective solving a case!
> - **Premises** = Clues you've gathered: "The suspect was at the crime scene" + "Anyone at the crime scene is a suspect"
> - **Conclusion** = Your deduction: "Therefore, this person is a suspect"
> - **Valid argument** = Your logic chain is unbreakable, even if the clues might be wrong!
> - **Sound argument** = Valid logic + ALL clues are actually TRUE
>
> Fun fact: Sherlock Holmes' famous "deductions" are actually **Modus Ponens** and **Modus Tollens** in disguise! 🔍

> 🏭 **Production Use — AI & Expert Systems:**
> Rule-based AI systems (like medical diagnosis systems MYCIN, or fraud detection at banks 🏦) use these exact inference rules! When your bank blocks a suspicious transaction, it's applying chains of Modus Ponens: "IF transaction > $10,000 AND from new device THEN flag as suspicious" → Transaction matches → FLAGGED! 🚩

### 4.1 What is a Logical Argument?

An argument consists of:
- **Premises:** $P_1, P_2, \ldots, P_n$ (given statements)
- **Conclusion:** $Q$

The argument is **valid** if whenever ALL premises are true, the conclusion MUST be true.

$$P_1 \wedge P_2 \wedge \cdots \wedge P_n \rightarrow Q \text{ is a tautology}$$

### 4.2 Rules of Inference (⭐ MUST MEMORIZE)

| Rule | Premises | Conclusion | In English |
|---|---|---|---|
| **Modus Ponens** | $p \rightarrow q, p$ | $q$ | If p→q and p is true, then q |
| **Modus Tollens** | $p \rightarrow q, \neg q$ | $\neg p$ | If p→q and q is false, then p is false |
| **Hypothetical Syllogism** | $p \rightarrow q, q \rightarrow r$ | $p \rightarrow r$ | Chain rule |
| **Disjunctive Syllogism** | $p \vee q, \neg p$ | $q$ | If p∨q and p is false, then q |
| **Addition** | $p$ | $p \vee q$ | Can always OR something |
| **Simplification** | $p \wedge q$ | $p$ | Can extract from AND |
| **Conjunction** | $p, q$ | $p \wedge q$ | Can combine with AND |
| **Resolution** | $p \vee q, \neg p \vee r$ | $q \vee r$ | Key for automated reasoning |
| **Constructive Dilemma** | $(p \rightarrow q) \wedge (r \rightarrow s), p \vee r$ | $q \vee s$ | |
| **Destructive Dilemma** | $(p \rightarrow q) \wedge (r \rightarrow s), \neg q \vee \neg s$ | $\neg p \vee \neg r$ | |

### 4.3 Common Fallacies

| Fallacy | Wrong Argument | Why it's Wrong |
|---|---|---|
| **Affirming the consequent** | $p \rightarrow q, q \therefore p$ | Converse is not equivalent |
| **Denying the antecedent** | $p \rightarrow q, \neg p \therefore \neg q$ | Inverse is not equivalent |

**Example (GATE Level):** Determine if the argument is valid:
"If it rains, the road is wet. The road is wet. Therefore, it rained."

Let $p$ = "it rains", $q$ = "road is wet"
Premises: $p \rightarrow q$, $q$. Conclusion: $p$.

This is **Affirming the Consequent** — **INVALID**! (The road could be wet from a sprinkler.)

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

### 4.5 Proof Strategies Quick Reference 📋

| Strategy | When to Use | Method |
|---|---|---|
| **Truth Table** | Small number of variables ($\leq 4$) | Check all $2^n$ rows |
| **Laws of Equivalence** | Simplification problems | Apply laws step by step |
| **Resolution** | "Prove this argument is valid" | Negate conclusion, derive $\square$ |
| **Rules of Inference** | Step-by-step formal proofs | Chain Modus Ponens, etc. |
| **Conditional Proof** | Prove $p \rightarrow q$ | Assume $p$, derive $q$ |
| **Indirect Proof** | Prove $p$ | Assume $\neg p$, derive contradiction |

---

> 🌍 **Why This Matters in the Real World:**
> Predicate logic is the language of **databases (SQL)**, **AI knowledge bases**, **Prolog programming**, and **semantic web**. Every SQL query is actually a predicate logic statement! When you write `SELECT * FROM students WHERE GPA > 3.5`, you're really saying $\exists x(Student(x) \wedge GPA(x) > 3.5)$.
>
> 🤖 **AI Spotlight:** OpenAI's GPT models, Google's Knowledge Graph, and IBM Watson all use predicate logic internally to represent and reason about knowledge!

---

## Chapter 5: Predicates and Quantifiers 🌐

### 5.1 Why First Order Logic? 🤔

> 💡 **Beginner Analogy — The Limitation of Yes/No 🚫:**
> Propositional logic is like a light switch — ON or OFF. But what if you want to say "ALL lights in the building are ON" or "SOME lights are flickering"? You need a way to talk about **many things at once** with patterns — that's predicate logic!
>
> Think of it this way:
> - Propositional Logic = "The door is locked" (specific, one thing) 🚪
> - Predicate Logic = "ALL doors on floor 3 are locked" (pattern about many things) 🏢

Propositional logic can't express statements like "All humans are mortal" or "There exists a prime number greater than 100." We need **predicates** and **quantifiers**.

### 5.2 Predicates

A **predicate** is a statement containing variables that becomes a proposition when variables are given values.

- $P(x)$: "x is a prime number" — this is a predicate
- $P(7)$: "7 is a prime number" — this is a proposition (TRUE)
- $P(4)$: "4 is a prime number" — this is a proposition (FALSE)

**Domain (Universe of Discourse):** The set of all possible values that variables can take.

### 5.3 Quantifiers (⭐⭐ CRITICAL)

| Quantifier | Symbol | Meaning | Negation(⭐ GATE FAVORITE) |
|---|---|---|---|
| **Universal** | $\forall x\, P(x)$ | "For all x, P(x) is true" | $\exists x\, \neg P(x)$ |
| **Existential** | $\exists x\, P(x)$ | "There exists an x such that P(x) is true" | $\forall x\, \neg P(x)$ |

**Key insight:** Negation **flips** the quantifier and **negates** the predicate.

For **nested quantifiers:**
$$\neg(\forall x\, \exists y\, P(x,y)) \equiv \exists x\, \forall y\, \neg P(x,y)$$

### 5.4 Nested Quantifiers (⭐⭐ TRICKY) 🌀

> 💡 **Beginner Analogy — The Restaurant Analogy 🍽️:**
> - $\forall x\ \exists y$: "For every customer, there EXISTS a dish they'll like" — Different customers can like different dishes. This is like a restaurant with a diverse menu! 🍕🍣🍔
> - $\exists y\ \forall x$: "There EXISTS one dish that EVERY customer likes" — One magical dish that satisfies everyone! Much harder to achieve. Like finding one song everyone at a party enjoys! 🎵
>
> See the difference? The first is easy (diverse options), the second is hard (universal satisfaction). That's why $\exists y\ \forall x \Rightarrow \forall x\ \exists y$ but NOT the reverse!

| Statement | Meaning |
|---|---|
| $\forall x\, \forall y\, P(x,y)$ | For every x and every y, P(x,y) holds |
| $\forall x\, \exists y\, P(x,y)$ | For every x, there exists some y (possibly different for each x) |
| $\exists x\, \forall y\, P(x,y)$ | There exists a fixed x that works for ALL y |
| $\exists x\, \exists y\, P(x,y)$ | There exist some x and some y |

> ⭐ **Order matters!** $\forall x\, \exists y\, P(x,y) \not\equiv \exists y\, \forall x\, P(x,y)$
> 
> $\forall x\, \exists y$ (y can depend on x) is WEAKER than $\exists y\, \forall x$ (one y works for all x)
> 
> However: $\exists y\, \forall x\, P(x,y) \Rightarrow \forall x\, \exists y\, P(x,y)$ (the stronger implies the weaker)

### 5.5 English to FOL(First-Order Logic) Translation (⭐ GATE CRITICAL)

**Example 1:** "Every student likes some course"
- Domain: Students, Courses
- $S(x)$: x is a student, $C(y)$: y is a course, $L(x,y)$: x likes y
- $\forall x\, (S(x) \rightarrow \exists y\, (C(y) \wedge L(x,y)))$

**Example 2:** "There is a student who likes every course"
- $\exists x\, (S(x) \wedge \forall y\, (C(y) \rightarrow L(x,y)))$

**Template Rules:**
- "All A are B" → $\forall x\, (A(x) \rightarrow B(x))$ — use **implication** with $\forall$
- "Some A are B" → $\exists x\, (A(x) \wedge B(x))$ — use **conjunction** with $\exists$
- NEVER: $\forall x\, (A(x) \wedge B(x))$ for "All A are B" (this says everything is both A and B)
- NEVER: $\exists x\, (A(x) \rightarrow B(x))$ for "Some A are B" (this is almost always true vacuously)

### 5.6 Free and Bound Variables

- A variable is **bound** if it's within the scope of a quantifier
- A variable is **free** if it's not bound by any quantifier
- A sentence with no free variables is a **closed formula** (has a definite truth value)

### 5.7 Validity and Satisfiability of FOL Expressions

| Property | Definition |
|---|---|
| **Valid** | True in EVERY interpretation (for every domain and every predicate meaning) |
| **Satisfiable** | True in at LEAST ONE interpretation |
| **Unsatisfiable** | False in EVERY interpretation |

**🔍 What is an Interpretation?**

An **interpretation** (or model) $I$ of an FOL formula consists of:
1. A **non-empty domain** $D$ (the "universe")
2. An assignment of each **constant** to an element of $D$
3. An assignment of each **predicate** to a relation on $D$
4. An assignment of each **function symbol** to a function on $D$

> 💡 **Example:** For $\forall x\, P(x)$:
> - **Interpretation 1:** $D = \{1, 2\}$, $P(x)$ means "$x > 0$" → TRUE (both 1, 2 > 0) ✅
> - **Interpretation 2:** $D = \{-1, 0, 1\}$, $P(x)$ means "$x > 0$" → FALSE ($P(-1)$ fails) ❌
>
> Since there exists an interpretation where it's false, $\forall x\, P(x)$ is **NOT valid** (but it IS satisfiable!)

**To show a formula is NOT valid:** Find ONE counterexample interpretation 🔍
**To show a formula IS valid:** Must prove for ALL possible interpretations 🌍

### 5.8 Distributive Properties of Quantifiers

| Property | Valid? |
|---|---|
| $\forall x\, (P(x) \wedge Q(x)) \equiv \forall x\, P(x) \wedge \forall x\, Q(x)$ | ✓ YES |
| $\forall x\, (P(x) \vee Q(x)) \equiv \forall x\, P(x) \vee \forall x\, Q(x)$ | ✗ NO (→ only) |
| $\exists x\, (P(x) \vee Q(x)) \equiv \exists x\, P(x) \vee \exists x\, Q(x)$ | ✓ YES |
| $\exists x\, (P(x) \wedge Q(x)) \equiv \exists x\, P(x) \wedge \exists x\, Q(x)$ | ✗ NO (← only) |

> **Mnemonic:** $\forall$ distributes over $\wedge$, $\exists$ distributes over $\vee$. The "matching" pairs distribute.

### 5.9 Rules of Inference for Quantified Statements (⭐ IMPORTANT)

| Rule | Statement | Conclusion | Condition |
|---|---|---|---|
| **Universal Instantiation (UI)** | $\forall x\, P(x)$ | $P(c)$ for any $c$ | $c$ in domain |
| **Universal Generalization (UG)** | $P(c)$ for arbitrary $c$ | $\forall x\, P(x)$ | $c$ must be truly arbitrary |
| **Existential Instantiation (EI)** | $\exists x\, P(x)$ | $P(c)$ for some specific $c$ | $c$ must be a NEW constant |
| **Existential Generalization (EG)** | $P(c)$ for some specific $c$ | $\exists x\, P(x)$ | Always valid |

**Proof Strategy with Quantifiers:**
1. Remove universal quantifiers using UI
2. Remove existential quantifiers using EI (use fresh constants!)
3. Apply propositional inference rules
4. Re-introduce quantifiers using UG or EG

**GATE Pattern:** "All men are mortal. Socrates is a man. Therefore Socrates is mortal."
- $\forall x(Man(x) \rightarrow Mortal(x))$, $Man(Socrates)$
- **UI:** $Man(Socrates) \rightarrow Mortal(Socrates)$, $Man(Socrates)$
- **Modus Ponens:** $Mortal(Socrates)$

### 5.10 Prenex Normal Form (PNF) (⭐ ASKED IN GATE)

A formula is in **Prenex Normal Form** if ALL quantifiers are at the front (prefix), followed by a quantifier-free formula (matrix):

$$Q_1 x_1 \, Q_2 x_2 \cdots Q_n x_n \, \phi(x_1, x_2, \ldots, x_n)$$

**Conversion to PNF:**
1. Eliminate $\rightarrow$ and $\leftrightarrow$
2. Push $\neg$ inward (flip quantifiers): $\neg \forall x\, P(x) \equiv \exists x\, \neg P(x)$; $\neg \exists x\, P(x) \equiv \forall x\, \neg P(x)$
3. Rename bound variables to avoid conflicts
4. Move quantifiers to front using:
   - $(\forall x\, P(x)) \wedge Q \equiv \forall x\, (P(x) \wedge Q)$ (if $x$ not free in $Q$)
   - $(\exists x\, P(x)) \vee Q \equiv \exists x\, (P(x) \vee Q)$ (if $x$ not free in $Q$)

**GATE-Level Example:** Convert $\neg(\forall x\, P(x) \rightarrow \exists y\, Q(y))$ to PNF
1. $\neg(\neg\forall x\, P(x) \vee \exists y\, Q(y))$  
2. $\forall x\, P(x) \wedge \forall y\, \neg Q(y)$  
3. $\forall x\, \forall y\, (P(x) \wedge \neg Q(y))$ ✓

### 5.11 Resolution in Predicate Logic — Skolemization & Unification (⭐ GATE)

**Clausal Form / Skolem Normal Form:**
1. Convert to PNF
2. **Skolemization:** Replace existentially quantified variables with Skolem functions:
   - $\exists x\, P(x)$ → $P(c)$ (Skolem constant)
   - $\forall x\, \exists y\, P(x,y)$ → $\forall x\, P(x, f(x))$ (Skolem function $f$ depends on $x$)
3. Drop universal quantifiers (they're implicit)
4. Convert matrix to CNF

**Unification (⭐ ASKED IN GATE):**

**Unification** finds a substitution that makes two expressions identical. **Most General Unifier (MGU)** is the simplest such substitution.

| Unify | MGU |
|---|---|
| $P(x, a)$ with $P(b, a)$ | $\{x/b\}$ |
| $P(x, f(x))$ with $P(a, y)$ | $\{x/a, y/f(a)\}$ |
| $P(x, x)$ with $P(a, b)$ | **FAILS** ($x=a$ and $x=b$ conflict) |
| $P(f(x))$ with $P(x)$ | **FAILS** (occurs check — $x$ cannot unify with $f(x)$) |

**Resolution Principle for FOL:**
1. Negate the conclusion and add to premises
2. Convert everything to clausal form
3. Repeatedly apply resolution with unification
4. If empty clause ($\square$) is derived → original conclusion is valid

### 5.12 Important FOL Equivalences (⭐ GATE TRICKY)

| Equivalence | Valid? |
|---|---|
| $\forall x\, (P(x) \rightarrow Q(x)) \equiv \forall x\, P(x) \rightarrow \forall x\, Q(x)$ | ❌ NO (← only) |
| $\forall x\, (P(x) \rightarrow Q(x)) \Rightarrow \forall x\, P(x) \rightarrow \forall x\, Q(x)$ | ✅ YES |
| $\exists x\, (P(x) \rightarrow Q(x)) \equiv \forall x\, P(x) \rightarrow \exists x\, Q(x)$ | ✅ YES |
| $\forall x\, P(x) \vee \forall x\, Q(x) \Rightarrow \forall x\, (P(x) \vee Q(x))$ | ✅ YES (but NOT ←) |
| $\exists x\, (P(x) \wedge Q(x)) \Rightarrow \exists x\, P(x) \wedge \exists x\, Q(x)$ | ✅ YES (but NOT ←) |

### 5.13 Scope of Quantifiers — Edge Cases (⭐ GATE Trap) ⚠️

> 🚨 **GATE often tests whether students understand quantifier scope correctly!**

**Rules for quantifier scope manipulation:**

| Rule | Condition |
|---|---|
| $\forall x\, P(x) \wedge Q \equiv \forall x\, (P(x) \wedge Q)$ | $x$ NOT free in $Q$ |
| $\exists x\, P(x) \vee Q \equiv \exists x\, (P(x) \vee Q)$ | $x$ NOT free in $Q$ |
| $\forall x\, P(x) \vee Q \equiv \forall x\, (P(x) \vee Q)$ | $x$ NOT free in $Q$ |
| $\exists x\, P(x) \wedge Q \equiv \exists x\, (P(x) \wedge Q)$ | $x$ NOT free in $Q$ |

**⚠️ When $x$ IS free in $Q$, you CANNOT move the quantifier!** Always **rename bound variables** first to avoid capture.

**Variable Capture Example 🪤:**
$\forall x\, P(x) \wedge Q(x)$ — here the second $x$ is FREE (not bound by $\forall$)!
This is NOT the same as $\forall x\, (P(x) \wedge Q(x))$.

**Renaming fix:** $\forall y\, P(y) \wedge Q(x)$ — now $y$ is bound, $x$ is free, no ambiguity!

### 5.14 Unique Existential Quantifier ($\exists!$) 🎯

**"There exists exactly one $x$ such that $P(x)$"**:
$$\exists! x\, P(x) \equiv \exists x\, (P(x) \wedge \forall y\, (P(y) \rightarrow y = x))$$

Equivalent to: "There exists an $x$ with $P(x)$, and any $y$ with $P(y)$ must equal $x$."

> 🏭 **Application:** Database constraints like PRIMARY KEY enforce $\exists!$ — exactly one record with that key value!

### 5.15 Equality in FOL (⭐ GATE Awareness) ⚖️

**FOL with equality** adds the special predicate $=$ with axioms:
- **Reflexivity:** $\forall x\, (x = x)$
- **Symmetry:** $\forall x\, \forall y\, (x = y \rightarrow y = x)$
- **Transitivity:** $\forall x\, \forall y\, \forall z\, (x = y \wedge y = z \rightarrow x = z)$
- **Substitution:** $x = y \rightarrow (P(x) \leftrightarrow P(y))$

**Useful for expressing cardinality constraints:**
- "There are at least 2 elements": $\exists x\, \exists y\, (x \neq y)$
- "There are exactly 2 elements satisfying $P$": $\exists x\, \exists y\, (x \neq y \wedge P(x) \wedge P(y) \wedge \forall z\, (P(z) \rightarrow z = x \vee z = y))$

### 5.16 Worked Examples + PYQ-Style Traps (Quantifiers) 🌐

#### Worked Example 1: Correct formalization

Statement: "Every student likes at least one subject."

Let $Student(x)$ and $Likes(x,y)$ with domain of $y$ as subjects.

Correct form:

$$\forall x\, (Student(x) \rightarrow \exists y\, Likes(x,y))$$

Not correct: $\exists y\, \forall x\,(Student(x) \rightarrow Likes(x,y))$ (this says there is one universal favorite subject).

#### Worked Example 2: Negation of quantified formula

Negate:

$$\forall x\, \exists y\, (P(x,y) \rightarrow Q(y))$$

Step-by-step:

$$\neg \forall x\, \exists y\, (P(x,y) \rightarrow Q(y))$$
$$\equiv \exists x\, \forall y\, \neg (P(x,y) \rightarrow Q(y))$$
$$\equiv \exists x\, \forall y\, (P(x,y) \wedge \neg Q(y))$$

✅ Final negation: $\exists x\, \forall y\, (P(x,y) \wedge \neg Q(y))$.

#### PYQ-Style Trap Box ⚠️

- "Some A are B" as $\exists x (A(x) \rightarrow B(x))$ ❌ (use $\exists x(A(x) \wedge B(x))$)
- Swapping $\forall$ and $\exists$ keeps meaning ❌
- Negation of $\forall x\, P(x)$ is $\forall x\, \neg P(x)$ ❌ (correct is $\exists x\, \neg P(x)$)
- Variable capture is harmless ❌ (always rename bound variables when moving quantifiers)

---

# 🌐 PART 2: SET THEORY & RELATIONS

> 🎭 **"Sets are the building blocks of ALL mathematics — and by extension, all of Computer Science!"**

---

> 🏭 **Production Use Cases:**
> - **Redis Sets** 🟥: The popular in-memory database Redis has a dedicated `SET` data type used by Twitter, GitHub, and StackOverflow for storing unique items, computing intersections (common followers), unions (combined feeds), and differences
> - **Elasticsearch** 🔍: Every search query uses set operations — intersection for AND queries, union for OR queries
> - **Spotify Playlists** 🎵: "Songs you AND your friend both like" = Set Intersection! "Songs either of you likes" = Set Union!
> - **Git** 🐙: `git diff` computes set difference between commits!

---

## Chapter 6: Sets — Fundamentals 🧱

### 6.1 Definition 📌

> 💡 **Beginner Analogy — The Bag of Unique Marbles 🩩:**
> A set is like a bag where:
> 1. Every marble is **unique** (no duplicates!) — {1, 1, 2} is actually just {1, 2}
> 2. **Order doesn't matter** — {1, 2, 3} = {3, 1, 2} (shake the bag!)
> 3. You can clearly tell if a marble belongs to the bag or not ("well-defined")
>
> In Python, this is EXACTLY what the `set()` data type does! 🐍
> ```python
> my_set = {1, 2, 2, 3}  # Python automatically makes it {1, 2, 3}
> ```

A **set** is a well-defined collection of distinct objects (called **elements** or **members**).

- $a \in A$ means "a is an element of A"
- $a \notin A$ means "a is not an element of A"

### 6.2 Set Representations

| Method | Example |
|---|---|
| **Roster/Tabular** | $A = \{1, 2, 3, 4\}$ |
| **Set-builder** | $A = \{x \in \mathbb{Z} \mid 1 \leq x \leq 4\}$ |

### 6.3 Important Sets

| Symbol | Set |
|---|---|
| $\mathbb{N}$ | Natural numbers $\{0, 1, 2, 3, \ldots\}$ or $\{1, 2, 3, \ldots\}$ |
| $\mathbb{Z}$ | Integers $\{\ldots, -2, -1, 0, 1, 2, \ldots\}$ |
| $\mathbb{Q}$ | Rational numbers |
| $\mathbb{I}$ | Irrational numbers (numbers that cannot be written as a fraction. Non-terminating, non-repeating decimals like $\sqrt{2}, \pi, e$)|
| $\mathbb{R}$ | Real numbers |
| $\mathbb{C}$ | Complex numbers |
| $\emptyset$ or $\{\}$ | Empty set |
| $\mathbb{U}$ | Universal set |

### 6.4 Subset and Proper Subset

- $A \subseteq B$ (subset): Every element of A is in B. Formally: $\forall x\, (x \in A \rightarrow x \in B)$
- $A \subset B$ (proper subset): $A \subseteq B$ and $A \neq B$
- $\emptyset \subseteq A$ for any set A (vacuously true)
- $A \subseteq A$ (every set is a subset of itself)

### 6.5 Power Set (⭐ FREQUENTLY ASKED) 💥

> 💡 **Beginner Analogy — Pizza Toppings 🍕:**
> If you have toppings {Cheese, Mushroom}, the power set is ALL possible pizza combinations:
> - No toppings (plain) = $\emptyset$
> - Just Cheese = {Cheese}
> - Just Mushroom = {Mushroom}
> - Both = {Cheese, Mushroom}
> That's $2^2 = 4$ options! With 10 toppings, you'd have $2^{10} = 1024$ possible pizzas! 🤯
>
> 🏭 **Production Connection:** This is exactly why feature flags in software (like LaunchDarkly, used by Netflix) grow exponentially — with 20 flags, there are over 1 million ($2^{20}$) possible configurations to test!

$\mathcal{P}(A)$ = set of all subsets of A

If $|A| = n$, then $|\mathcal{P}(A)| = 2^n$

**Example:** $A = \{1, 2\}$ → $\mathcal{P}(A) = \{\emptyset, \{1\}, \{2\}, \{1,2\}\}$

**Key results:**
- $\emptyset \in \mathcal{P}(A)$ always
- $A \in \mathcal{P}(A)$ always
- $|\mathcal{P}(\mathcal{P}(\emptyset))| = |\mathcal{P}(\{\emptyset\})| = 2^1 = 2 \; \{\emptyset, \{\emptyset\}\}$
- $|\mathcal{P}(\mathcal{P}(\mathcal{P}(\emptyset)))| = 2^2 = 4 \; \{\emptyset, \{\emptyset\}, \{\{\emptyset\}\}, \{\emptyset, \{\emptyset\}\}\}$

### 6.6 Set Operations

| Operation | Notation | Definition |
|---|---|---|
| **Union** | $A \cup B$ | $\{x \mid x \in A \text{ or } x \in B\}$ |
| **Intersection** | $A \cap B$ | $\{x \mid x \in A \text{ and } x \in B\}$ |
| **Difference** | $A - B$ or $A \setminus B$ | $\{x \mid x \in A \text{ and } x \notin B\}$ |
| **Symmetric Difference** | $A \oplus B$ or $A \Delta B$ | $(A - B) \cup (B - A)$ |
| **Complement** | $A'$ or $\overline{A}$ | $\{x \in U \mid x \notin A\}$ |

**Cardinality formulas:**
- $|A \cup B| = |A| + |B| - |A \cap B|$ (**Inclusion-Exclusion for 2 sets**)
- $|A \cup B \cup C| = |A| + |B| + |C| - |A \cap B| - |A \cap C| - |B \cap C| + |A \cap B \cap C|$
- $|A - B| = |A| - |A \cap B|$
- $|A \oplus B| = |A| + |B| - 2|A \cap B|$

### 6.6.1 Symmetric Difference — Deep Dive (⭐ GATE Tested) 🔀

> The symmetric difference $A \oplus B = (A - B) \cup (B - A) = (A \cup B) - (A \cap B)$

**Key Properties of Symmetric Difference:**

| Property | Formula |
|---|---|
| **Commutative** | $A \oplus B = B \oplus A$ |
| **Associative** | $(A \oplus B) \oplus C = A \oplus (B \oplus C)$ |
| **Identity** | $A \oplus \emptyset = A$ |
| **Self-inverse** | $A \oplus A = \emptyset$ |
| **Distributive with $\cap$** | $A \cap (B \oplus C) = (A \cap B) \oplus (A \cap C)$ |
| **Complement** | $A \oplus U = A'$ (complement of A) |

> 🎯 **GATE Insight:** $(\mathcal{P}(S), \oplus, \cap)$ forms a **Boolean ring**! The symmetric difference acts like addition and intersection like multiplication. This structure appears in coding theory and error-correcting codes.

### 6.6.2 Partition of a Set 🧩

> 💡 **Beginner Analogy:** Dividing a class of 30 students into project groups where every student is in exactly one group!

A **partition** of set $A$ is a collection of non-empty subsets $\{A_1, A_2, \ldots, A_k\}$ such that:
1. $A_i \neq \emptyset$ for all $i$ (no empty blocks)
2. $A_i \cap A_j = \emptyset$ for $i \neq j$ (pairwise disjoint)
3. $A_1 \cup A_2 \cup \cdots \cup A_k = A$ (covers everything)

**Examples for $A = \{1, 2, 3\}$:**
- $\{\{1\}, \{2\}, \{3\}\}$ ✅ (3 blocks)
- $\{\{1, 2\}, \{3\}\}$ ✅ (2 blocks)
- $\{\{1, 3\}, \{2\}\}$ ✅ (2 blocks)
- $\{\{2, 3\}, \{1\}\}$ ✅ (2 blocks)
- $\{\{1, 2, 3\}\}$ ✅ (1 block)
- Total = **5** = $B_3$ (Bell number!)

**⭐ Key Connection:** Every partition ↔ unique equivalence relation, and vice versa.

**Counting partitions:**
- Number of partitions of $n$-element set into exactly $k$ blocks = **Stirling number** $S(n, k)$
- Total partitions of $n$-element set = **Bell number** $B_n = \sum_{k=0}^{n} S(n, k)$

### 6.6.3 Russell's Paradox & Axiomatic Set Theory 🤯

> This is asked at an awareness level in GATE — understanding why "the set of all sets" is problematic.

**Russell's Paradox:** Consider $R = \{x \mid x \notin x\}$ (the set of all sets that don't contain themselves).
- Is $R \in R$? If yes → by definition $R \notin R$. Contradiction! 💥
- Is $R \notin R$? If yes → by definition $R \in R$. Contradiction! 💥

**Resolution:** Modern set theory (ZFC axioms) restricts what can be a set — you can't form "the set of all sets." This motivates the **axiom of separation** (only subsets of existing sets can be formed).

> 🎯 **GATE Connection:** This is why we always specify a **Universal Set $U$** — to avoid paradoxes!

### 6.7 Set Identities (Parallel to Logical Laws)

| Law | Set Form |
|---|---|
| **De Morgan's** | $(A \cup B)' = A' \cap B'$, $(A \cap B)' = A' \cup B'$ |
| **Distributive** | $A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$ |
| **Absorption** | $A \cup (A \cap B) = A$ |
| **Complement** | $A \cup A' = U$, $A \cap A' = \emptyset$ |

### 6.8 Cartesian Product

$A \times B = \{(a, b) \mid a \in A, b \in B\}$

- $|A \times B| = |A| \cdot |B|$
- $A \times B \neq B \times A$ in general (unless $A = B$ or one is empty)
- $A \times \emptyset = \emptyset$

### 6.9 Countable and Uncountable Sets (⭐ ASKED IN GATE)

- A set is **finite** if it has $n$ elements for some $n \in \mathbb{N}$
- A set is **countably infinite** if there exists a bijection with $\mathbb{N}$ (can be listed as $a_1, a_2, a_3, \ldots$)
- A set is **countable** if it is finite or countably infinite
- A set is **uncountable** if it is not countable

**Important Results (MUST KNOW):**

| Set | Countable? |
|---|---|
| $\mathbb{N}$ (natural numbers) | ✅ Countably infinite |
| $\mathbb{Z}$ (integers) | ✅ Countably infinite (list: 0, 1, -1, 2, -2, ...) |
| $\mathbb{Q}$ (rationals) | ✅ Countably infinite (Cantor's zigzag) |
| $\mathbb{N} \times \mathbb{N}$ | ✅ Countably infinite (Cantor pairing) |
| Set of all finite strings over finite alphabet | ✅ Countably infinite |
| Set of all C programs / Turing machines | ✅ Countably infinite |
| $\mathbb{R}$ (real numbers) | ❌ Uncountable |
| $(0, 1)$ interval | ❌ Uncountable |
| $\mathcal{P}(\mathbb{N})$ (power set of naturals) | ❌ Uncountable |
| Set of all functions $\mathbb{N} \rightarrow \{0,1\}$ | ❌ Uncountable |
| Set of all languages over $\{0,1\}$ | ❌ Uncountable |

### 6.10 Cantor's Diagonal Argument (⭐ PROOF TECHNIQUE)

**Theorem:** $\mathbb{R}$ is uncountable (equivalently, $(0,1)$ is uncountable).

**Proof:** Assume all reals in $(0,1)$ can be listed: $r_1, r_2, r_3, \ldots$ with decimal expansions. Construct $x = 0.e_1 e_2 e_3 \ldots$ where $e_i \neq d_{ii}$ (diagonal digit). Then $x \neq r_i$ for every $i$ — contradiction!

### 6.11 Cantor's Theorem

For ANY set $A$: $|A| < |\mathcal{P}(A)|$ (power set is always strictly larger).

**Consequence for GATE/TOC:** Since all Turing machines are countable but all languages are uncountable, undecidable languages MUST exist.

**Properties of Countable/Uncountable Sets:**
- Countable union of countable sets is countable
- Subset of a countable set is countable
- $A$ countable and $B$ countable $\Rightarrow A \times B$ countable
- If $A$ is uncountable and $A \subseteq B$, then $B$ is uncountable

### 6.12 Multisets

A **multiset** (bag) allows repeated elements. E.g., $\{1, 1, 2, 3, 3, 3\}$.

**Number of multisets of size $r$ from $n$ types = $\binom{n+r-1}{r}$** (same as stars and bars!)

---

## Chapter 7: Relations (⭐⭐ VERY HIGH WEIGHTAGE) 🔗

> 💡 **Beginner Analogy — Social Media Connections 📱:**
> Relations are like connections on social media!
> - **"follows" on Instagram** 📸 = A relation (you follow someone, they may not follow back — it's directed!)
> - **"friends" on Facebook** 👥 = A relation that's SYMMETRIC (if you're my friend, I'm your friend)
> - **"blocked" on WhatsApp** 🚫 = An ASYMMETRIC relation
>
> In a database, EVERY foreign key is a relation! When your `orders` table references `customers` table — that's a mathematical relation in action! 🗄️

### 7.1 Definition

A **relation** $R$ from set $A$ to set $B$ is a subset of $A \times B$.

If $(a, b) \in R$, we write $aRb$.

A **relation on a set A** is a relation from A to A (subset of $A \times A$).

**Number of relations from A to B:** $2^{|A| \times |B|}$ (each element of $A \times B$ is either in R or not)

### 7.2 Properties of Relations on a Set A (⭐⭐⭐ MOST ASKED)

Let $R$ be a relation on set $A$ where $|A| = n$.

| Property | Definition | Matrix Condition | Count |
|---|---|---|---|
| **Reflexive** | $\forall a \in A: (a,a) \in R$ | All diagonal entries = 1 | $2^{n^2 - n}$ |
| **Irreflexive** | $\forall a \in A: (a,a) \notin R$ | All diagonal entries = 0 | $2^{n^2 - n}$ |
| **Symmetric** | $aRb \Rightarrow bRa$ | $M = M^T$ | $2^{n(n+1)/2}$ |
| **Antisymmetric** | $aRb \wedge bRa \Rightarrow a = b$ | If $M_{ij} = 1$ and $i \neq j$, then $M_{ji} = 0$ | $2^n \cdot 3^{n(n-1)/2}$ |
| **Asymmetric** | $aRb \Rightarrow \neg(bRa)$ | Antisymmetric + Irreflexive | $3^{n(n-1)/2}$ |
| **Transitive** | $aRb \wedge bRc \Rightarrow aRc$ | $M^2 \leq M$ (Boolean) | No closed formula |

**Key relationships between properties:**
- Asymmetric = Antisymmetric + Irreflexive
- A relation can be BOTH symmetric AND antisymmetric (only if it's a subset of the identity relation)
- A relation can be NEITHER symmetric NOR antisymmetric
- Reflexive and Irreflexive are NOT complementary (a relation can be neither)

### 7.3 Equivalence Relation (⭐⭐ VERY IMPORTANT) 🟰

> 💡 **Beginner Analogy — Sorting Laundry 🧹:**
> Equivalence relations are like sorting your laundry by color:
> - **Reflexive:** Every shirt is the same color as itself ✅
> - **Symmetric:** If shirt A is the same color as shirt B, then B is the same color as A ✅
> - **Transitive:** If A matches B's color, and B matches C's color, then A matches C's color ✅
>
> The piles of sorted laundry = **Equivalence Classes**! Each pile (whites, darks, colors) is a partition of all your clothes! 👗👔🧥
>
> 🏭 **Production Example:** In compilers, **type equivalence** in programming languages uses equivalence relations. When C checks if `int` and `signed int` are the same type — that's an equivalence class!
> 
> 🌍 **Real-World:** India's PIN code system is an equivalence relation — addresses in the same PIN code "belong together" (same class), and these classes partition ALL addresses in India! 🇮🇳

A relation that is **Reflexive + Symmetric + Transitive** is an **equivalence relation**.

**Equivalence Class:** For element $a$, the equivalence class is $[a] = \{x \in A \mid xRa\}$

**Key Theorem:** Equivalence classes form a **partition** of the set A.
- Every element belongs to exactly one equivalence class
- Equivalence classes are non-empty, pairwise disjoint, and their union = A

**Counting:** Number of equivalence relations on a set of $n$ elements = **Bell number** $B_n$

| $n$ | $B_n$ |
|---|---|
| 0 | 1 |
| 1 | 1 |
| 2 | 2 |
| 3 | 5 |
| 4 | 15 |
| 5 | 52 |

**Partition ↔ Equivalence Relation:** There's a **bijection** between equivalence relations on A and partitions of A.

### 7.4 Partial Order Relation (POSET) (⭐⭐ VERY IMPORTANT) 📏

> 💡 **Beginner Analogy — Task Dependencies in a Project 📊:**
> Think of a software project with tasks:
> - "Write code" must come before "Test code" must come before "Deploy"
> - But "Write documentation" and "Write code" can happen in any order (incomparable!)
> - This is a **partial order** — some tasks have a clear sequence, others don't!
>
> 🏭 **Production Examples:**
> - **Make/CMake build systems** 🛠️: File dependencies form a partial order (compile A before B)
> - **Package managers (npm, pip)** 📦: Dependency resolution = topological sort of a POSET
> - **Apache Airflow** ✈️: Data pipeline DAGs are POSETs! Tasks have dependencies
> - **Git commit history** 🐙: Parent-child commits form a partial order

A relation that is **Reflexive + Antisymmetric + Transitive** is a **partial order**.

A set with a partial order is called a **Partially Ordered Set (POSET)**, denoted $(A, \preceq)$.

**Examples:**
- $(\mathbb{Z}, \leq)$ — integers with "less than or equal to"
- $(\mathcal{P}(S), \subseteq)$ — power set with subset relation
- $(\mathbb{Z}^+, |)$ — positive integers with divisibility

### 7.5 Hasse Diagrams (⭐ FREQUENTLY ASKED)

A Hasse diagram is a simplified digraph of a POSET:
1. Remove all self-loops (reflexive edges)
2. Remove all transitive edges (if $a \preceq b \preceq c$, remove $a \preceq c$)
3. If $a \preceq b$, draw $b$ higher than $a$ (remove arrows, direction is "upward")

### 7.6 Special Elements in a POSET

| Element | Definition |
|---|---|
| **Maximum** | $a$ such that $\forall x: x \preceq a$ (greatest element — unique if exists) |
| **Minimum** | $a$ such that $\forall x: a \preceq x$ (least element — unique if exists) |
| **Maximal** | $a$ such that no element is strictly greater than $a$ (can be multiple) |
| **Minimal** | $a$ such that no element is strictly less than $a$ (can be multiple) |
| **Upper Bound of S** | $a$ such that $\forall s \in S: s \preceq a$ |
| **Lower Bound of S** | $a$ such that $\forall s \in S: a \preceq s$ |
| **LUB / Supremum** | Least upper bound (smallest among all upper bounds) |
| **GLB / Infimum** | Greatest lower bound (largest among all lower bounds) |

### 7.7 Total Order (Linear Order)

A partial order where **every pair** of elements is comparable: $\forall a, b: a \preceq b$ or $b \preceq a$.

**Example:** $(\mathbb{Z}, \leq)$ is a total order. $(\mathcal{P}(\{1,2\}), \subseteq)$ is NOT (since $\{1\}$ and $\{2\}$ are incomparable).

### 7.7.1 Chains and Antichains (⭐ GATE IMPORTANT) ⛓️

> 💡 **Chain = totally ordered subset; Antichain = mutually incomparable subset**

**Chain:** A subset $C$ of a POSET where every pair of elements is comparable.
- In $(\mathcal{P}(\{1,2,3\}), \subseteq)$: $\{\emptyset, \{1\}, \{1,2\}, \{1,2,3\}\}$ is a chain of length 4

**Antichain:** A subset $A$ of a POSET where NO pair of elements is comparable.
- In $(\mathcal{P}(\{1,2,3\}), \subseteq)$: $\{\{1\}, \{2\}, \{3\}\}$ is an antichain of size 3

### 7.7.2 Dilworth's Theorem (⭐ GATE) 🎯

> **In any finite POSET, the maximum size of an antichain equals the minimum number of chains needed to partition the POSET.**

**Dual (Mirsky's Theorem):** The maximum length of a chain = minimum number of antichains needed to partition the POSET.

**GATE Application:** Given a POSET with $n$ elements:
- If the longest chain has length $k$, then the POSET can be partitioned into $k$ antichains
- If the largest antichain has size $m$, then the POSET can be partitioned into $m$ chains
- **Minimum chain decomposition** = max antichain size
- **Minimum antichain decomposition** = max chain length

> 🏭 **Production Use:** Dilworth's theorem is used in **parallel scheduling** — the minimum number of processors needed to execute tasks with dependencies = size of the largest set of independent (incomparable) tasks!

**Example:** In $(\{1, 2, 3, 4, 6, 12\}, |)$ (divisibility):
- Longest chain: $1 | 2 | 4 | 12$ (length 4) or $1 | 2 | 6 | 12$ (length 4)
- Largest antichain: $\{4, 3\}$ or $\{4, 6\}$ (neither divides the other) — size 2
- So min chains to cover = 2 ✅

### 7.7.3 Well-Order 📐

A total order where **every non-empty subset has a least element**.
- $(\mathbb{N}, \leq)$ is well-ordered ✅
- $(\mathbb{Z}, \leq)$ is NOT well-ordered ❌ (e.g., $\mathbb{Z}$ has no least element)
- $(\mathbb{Q}^+, \leq)$ is NOT well-ordered ❌ (open intervals have no minimum)

> 🎯 **GATE Connection:** Well-ordering is equivalent to the Axiom of Choice and Zorn's Lemma — foundational in mathematics.

### 7.7.4 Topological Sort from POSET 📊

A **topological sort** of a POSET $(A, \preceq)$ is a total (linear) order $\leq_L$ on $A$ such that $a \preceq b \Rightarrow a \leq_L b$.

- Every finite POSET has at least one topological sort
- A POSET has a **unique** topological sort iff it is already a total order
- **Number of topological sorts** = number of linear extensions (can be exponential)
- This directly connects to **topological sorting of DAGs** in Algorithms!

### 7.8 Lattice

A POSET where every pair of elements has both a **LUB** and a **GLB**.

- **Bounded lattice:** Has both a maximum (top, $\top$) and minimum (bottom, $\bot$)
- **Distributive lattice:** LUB and GLB distribute over each other
- **Complemented lattice:** Every element has a complement
- **Boolean lattice:** Complemented + Distributive + Bounded

### 7.9 Closure of Relations

| Closure | How to compute |
|---|---|
| **Reflexive closure** | $R \cup \{(a,a) \mid a \in A\}$ = $R \cup I$ (add diagonal) |
| **Symmetric closure** | $R \cup R^{-1}$ (add reverse of each pair) |
| **Transitive closure** | $R^+ = R \cup R^2 \cup R^3 \cup \cdots \cup R^n$ (Warshall's algorithm) |

### 7.10 Composition of Relations (⭐ FREQUENTLY ASKED)

If $R$ is a relation from $A$ to $B$ and $S$ is a relation from $B$ to $C$:

$$S \circ R = \{(a, c) \mid \exists b \in B: (a,b) \in R \text{ and } (b,c) \in S\}$$

**Matrix Method:** $M_{S \circ R} = M_R \odot M_S$ (Boolean matrix multiplication: AND for ×, OR for +)

**Powers of a Relation:**
- $R^1 = R$, $R^{n+1} = R^n \circ R$
- $R$ is transitive iff $R^n \subseteq R$ for all $n \geq 1$

### 7.10.1 Matrix Verification of Relation Properties 🧮

If relation $R$ on $A = \{a_1, \ldots, a_n\}$ has Boolean matrix $M$, then many relation questions become **pattern recognition on the matrix**.

| Property | Matrix Test |
|---|---|
| **Reflexive** | All diagonal entries are 1 |
| **Irreflexive** | All diagonal entries are 0 |
| **Symmetric** | $M = M^T$ |
| **Antisymmetric** | For $i \neq j$, never both $M[i][j] = 1$ and $M[j][i] = 1$ |
| **Transitive** | Boolean product $M^2 \leq M$ |
| **Equivalence relation** | Reflexive + Symmetric + Transitive |
| **Partial order** | Reflexive + Antisymmetric + Transitive |

Here, $M^2 \leq M$ means every 1 created by Boolean matrix multiplication is already present in $M$.

**Worked Example 🔍**

Take

$$M = \begin{bmatrix}
1 & 1 & 0 \\
1 & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}$$

- Diagonal all 1 → reflexive ✅
- $M = M^T$ → symmetric ✅
- The only non-trivial pair is between 1 and 2; because $(1,2)$ and $(2,1)$ exist and self-loops exist, transitivity also holds ✅

Hence this is an **equivalence relation** with equivalence classes:

$$\{a_1, a_2\}, \quad \{a_3\}$$

> 🎯 GATE tip: When a matrix question asks for "which properties hold?" check in this order: diagonal, transpose symmetry, mirrored off-diagonal conflict, then Boolean-square test.

### 7.11 Warshall's Algorithm for Transitive Closure (⭐ GATE IMPORTANT)

Given relation $R$ on set $\{a_1, \ldots, a_n\}$ with Boolean matrix $M$:

```
W(0) = M
For k = 1 to n:
    For i = 1 to n:
        For j = 1 to n:
            W(k)[i][j] = W(k-1)[i][j] OR (W(k-1)[i][k] AND W(k-1)[k][j])
```

Result $W^{(n)}$ is the matrix of $R^+$. **Time complexity:** $O(n^3)$

**Intuition:** At step $k$: "Can we get from $i$ to $j$ using intermediate vertices $\{1, 2, \ldots, k\}$?"

**Reflexive-Transitive Closure:** $R^* = R^+ \cup I$ (transitive closure plus identity/diagonal)

### 7.12 Representation of Relations

**Matrix Representation:** Boolean matrix $M$ where $M[i][j] = 1$ iff $(a_i, a_j) \in R$.
- Reflexive ↔ all diagonal entries = 1
- Symmetric ↔ $M = M^T$
- Antisymmetric ↔ for $i \neq j$: if $M[i][j] = 1$ then $M[j][i] = 0$

**Digraph Representation:** Each element = vertex; $(a, b) \in R$ = directed edge $a \to b$.
- Reflexive ↔ loop at every vertex
- Symmetric ↔ every edge has a reverse edge
- Transitive ↔ if path of length 2 from $a$ to $c$, direct edge also exists

### 7.13 Additional Counting Results for Relations (⭐ GATE NUMERICAL)

For $|A| = n$:

| Property | Number of Relations |
|---|---|
| Total relations on $A$ | $2^{n^2}$ |
| Reflexive | $2^{n^2 - n}$ |
| Irreflexive | $2^{n^2 - n}$ |
| Symmetric | $2^{n(n+1)/2}$ |
| Antisymmetric | $2^n \cdot 3^{n(n-1)/2}$ |
| Reflexive AND Symmetric | $2^{n(n-1)/2}$ |
| Reflexive AND Antisymmetric | $3^{n(n-1)/2}$ |
| Both Symmetric AND Antisymmetric | $2^n$ (only subsets of identity) |
| Equivalence relations | Bell number $B_n$ |
| Total/Linear orders | $n!$ |

### 7.13.1 N-ary Relations 📦

An **n-ary relation** is a subset of $A_1 \times A_2 \times \cdots \times A_n$.

- **Binary relation** ($n=2$): Most common — what we've been studying
- **Ternary relation** ($n=3$): e.g., "Student $x$ takes Course $y$ in Semester $z$"
- **n-ary relation** = a table with $n$ columns in a **relational database**!

> 🏭 **Database Connection:** Every table in SQL is an n-ary relation! `SELECT * FROM Students` returns a set of n-tuples. Relational algebra operations (σ, π, ⋈) operate on n-ary relations! This is WHY it's called a "relational" database.

**Operations on n-ary relations:**
- **Selection** (σ): Choose tuples satisfying a condition
- **Projection** (π): Choose specific columns
- **Join** (⋈): Combine two relations on matching attributes

### 7.13.2 Compatibility Relation 🤝

A relation that is **Reflexive + Symmetric** (but NOT necessarily transitive) is called a **compatibility relation** (or tolerance relation).

**Example:** "Lives within 5 km of" is compatible — you're within 5 km of yourself (reflexive), if A is within 5 km of B then B is within 5 km of A (symmetric), but A near B and B near C doesn't mean A near C (NOT transitive).

> **Compatibility classes** replace equivalence classes — but they can OVERLAP (unlike equivalence classes which partition). A maximal compatibility class is called a **clique** in the compatibility graph.

### 7.13.3 Worked Counting Examples for Relations 🔢

> Memorizing formulas is not enough. GATE usually hides the formula inside a tiny numerical question. Practice converting the wording into free choices. 🧩

Let $A = \{a,b,c\}$, so $|A| = 3$ and $|A \times A| = 9$.

**Example 1: Number of reflexive relations on $A$**

- Diagonal pairs $(a,a), (b,b), (c,c)$ are forced
- Remaining $9-3=6$ pairs are free

So the count is:

$$2^6 = 64$$

**Example 2: Number of symmetric relations on $A$**

- 3 diagonal entries are chosen independently
- Off-diagonal unordered pairs are: $\{a,b\}, \{a,c\}, \{b,c\}$
- Each unordered pair contributes either both directed pairs or neither

So total choices = $3 + 3 = 6$ binary decisions:

$$2^6 = 64$$

**Example 3: Number of relations that are both reflexive and symmetric**

- Diagonal is forced to 1, so no freedom there
- Only the 3 unordered off-diagonal pairs are free

Hence:

$$2^3 = 8$$

**Example 4: Number of equivalence relations on a 3-element set**

This equals the Bell number $B_3 = 5$.

The 5 partitions are:
- $\{\{a\},\{b\},\{c\}\}$
- $\{\{a,b\},\{c\}\}$
- $\{\{a,c\},\{b\}\}$
- $\{\{b,c\},\{a\}\}$
- $\{\{a,b,c\}\}$

> 💡 Equivalence relations on $A$ are in one-to-one correspondence with partitions of $A$.

**Example 5: Number of linear orders on an $n$-element set**

Every linear order is just an arrangement of the $n$ distinct elements from smallest to largest.

Therefore:

$$n!$$

### 7.13.4 PYQ-Style Relation Trap Set 🧠

#### Trap 1: Symmetric + transitive implies reflexive?

No ❌. Counterexample on $A=\{1,2\}$:

$$R=\emptyset$$

is symmetric and transitive, but not reflexive.

#### Trap 2: Antisymmetric means "not symmetric"?

No ❌. A relation can be both symmetric and antisymmetric (only possible with identity-type pairs).

Example:

$$R = \{(1,1),(2,2)\}$$

is both symmetric and antisymmetric.

#### Trap 3: Equivalence relation count confusion

On 3 elements, number of equivalence relations is Bell number $B_3=5$, **not** $2^9$ and not $3!$.

#### Mini PYQ Practice ✅

For $|A|=4$, number of reflexive relations is:

$$2^{4^2-4}=2^{12}$$

For $|A|=4$, number of symmetric relations is:

$$2^{4(5)/2}=2^{10}$$

> 🎯 Revision shortcut: if question gives a relation matrix, first classify property; if question gives only set size, move directly to counting formula.

### 7.14 Advanced Lattice Theory (⭐⭐ ASKED IN GATE)

**Lattice Properties** — for a lattice $(L, \preceq)$ with LUB ($\vee$, join) and GLB ($\wedge$, meet):

| Property | Formula |
|---|---|
| Commutative | $a \vee b = b \vee a$, $a \wedge b = b \wedge a$ |
| Associative | $(a \vee b) \vee c = a \vee (b \vee c)$ |
| Absorption | $a \vee (a \wedge b) = a$, $a \wedge (a \vee b) = a$ |
| Idempotent | $a \vee a = a$, $a \wedge a = a$ |

**Distributive Lattice (⭐ IMPORTANT):**
$$a \wedge (b \vee c) = (a \wedge b) \vee (a \wedge c)$$

**Key test:** A lattice is **NOT distributive** iff it contains a sublattice isomorphic to $M_3$ (diamond) or $N_5$ (pentagon).
- $M_3$: Diamond lattice — top, bottom, and 3 incomparable middle elements
- $N_5$: Pentagon lattice

**Complemented Lattice:** In a bounded lattice (with $\top$ and $\bot$), element $b$ is a complement of $a$ iff $a \vee b = \top$ and $a \wedge b = \bot$. In a distributive lattice, complements are **unique**.

**Boolean Algebra** = complemented distributive lattice.
- Isomorphic to $(\mathcal{P}(S), \subseteq)$ for some set $S$
- Always has $2^n$ elements (power of 2)
- **Number of atoms** = $n$ (where $|B| = 2^n$). An **atom** is an element $a$ with $\bot \prec a$ and nothing between.

**Modular Lattice:** if $a \preceq c$, then $a \vee (b \wedge c) = (a \vee b) \wedge c$.
- Every distributive lattice is modular
- $M_3$ is modular but not distributive; $N_5$ is NOT modular
- No $N_5$ sublattice → modular; modular + no $M_3$ → distributive

### 7.14.1 Complete Lattice & Fixed Points (⭐ GATE + Compiler Design) 🔒

A lattice is **complete** if EVERY subset (not just pairs) has a LUB and GLB.
- Every finite lattice is complete ✅
- $(\mathcal{P}(S), \subseteq)$ is a complete lattice with $\top = S$, $\bot = \emptyset$

**Knaster-Tarski Fixed-Point Theorem:**
> Every monotone function on a complete lattice has a **least fixed point** and a **greatest fixed point**.

> 🏭 **This theorem is FUNDAMENTAL in CS:**
> - **Dataflow analysis** (compiler optimization) — computing reaching definitions, live variables
> - **Abstract interpretation** (static analysis) — tools like Facebook Infer
> - **Denotational semantics** — defining meaning of recursive programs
> - **Database query evaluation** — fixed-point computation for recursive queries (Datalog)

### 7.14.2 Lattice Counting Results (⭐ GATE Numerical) 🔢

| Structure | Elements | Atoms | Complement of each element |
|---|---|---|---|
| $\mathcal{P}(\{1,...,n\})$ | $2^n$ | $n$ (singleton sets) | Unique (set complement) |
| Divisors of $n$ under $|$ | # divisors of $n$ | prime factors of $n$ | May not be unique |
| Boolean algebra $B_n$ | $2^n$ | Always $n$ | Always unique |

**Is a lattice distributive? Quick check:**
- If it has $\leq 4$ elements → always distributive
- Check for $M_3$ or $N_5$ sublattice (non-distributive indicators)

### 7.14.3 Boolean Algebra = Logic ∩ Lattice 🌉

> This is one of the most beautiful unifications in the whole syllabus: the SAME algebra describes sets, logic gates, and lattice operations. ✨

| Logic | Set Theory | Lattice / Boolean Algebra |
|---|---|---|
| $p \wedge q$ | $A \cap B$ | $a \wedge b$ |
| $p \vee q$ | $A \cup B$ | $a \vee b$ |
| $\neg p$ | $A^c$ | $a'$ |
| T | Universal set $U$ | $\top$ |
| F | Empty set $\emptyset$ | $\bot$ |

So:
- De Morgan's laws in logic and set theory are actually the **same Boolean algebra law**
- NAND and NOR are functionally complete because Boolean algebra can express every Boolean function
- Finite Boolean algebras are exactly complemented distributive lattices

**Atoms and Size Rule 💎**

If a finite Boolean algebra has $n$ atoms, then it has exactly:

$$2^n$$

elements.

This is because every element can be written as a join of some subset of the atoms.

**Finite Representation Theorem (practical version):**

Every finite Boolean algebra is isomorphic to a power-set algebra:

$$(\mathcal{P}(S), \cap, \cup, {}^c)$$

for some finite set $S$.

> 🎯 GATE shortcut: If you see a finite complemented distributive lattice, immediately think "power set of some set" and use $2^n$-style reasoning.

**Is a lattice Boolean? Quick check:**
- Must be complemented + distributive + bounded
- Size must be a power of 2
- Every element must have a unique complement

---

## Chapter 8: Functions (⭐ IMPORTANT) 🔧

### 8.1 Definition

A function $f: A \rightarrow B$ is a relation where **every element of A** maps to **exactly one element of B**.

- $A$ = domain, $B$ = codomain
- $f(A)$ = range (set of actual output values)
- Range $\subseteq$ Codomain

> 💡 **Beginner Analogy — A Vending Machine 🥤:**
> A function is like a vending machine:
> - **Domain** = buttons you can press (A1, A2, B1, B2...)
> - **Codomain** = all items the machine COULD dispense
> - **Range** = items it ACTUALLY dispenses (some slots might be empty!)
> - **Key rule:** Each button gives EXACTLY ONE item (never ambiguous!)
>
> 🏭 **Production:** Every API endpoint is a function — given input (request), it produces exactly one output (response). `getUser(id)` → one user object!

### 8.2 Types of Functions

| Type | Definition | Condition |
|---|---|---|
| **Injective (One-to-one)** | Different inputs → different outputs | $f(a) = f(b) \Rightarrow a = b$ |
| **Surjective (Onto)** | Every element of codomain is hit | Range = Codomain |
| **Bijective** | Both injective and surjective | One-to-one correspondence |

**Quick Visualization 🔍:**
- **Injective:** No output has more than 1 arrow pointing to it (but some outputs may have 0)
- **Surjective:** Every output has at least 1 arrow pointing to it
- **Bijective:** Every output has EXACTLY 1 arrow pointing to it

**Key conditions for existence:**
- Injection exists from $A$ to $B$ iff $|A| \leq |B|$
- Surjection exists from $A$ to $B$ iff $|A| \geq |B|$
- Bijection exists from $A$ to $B$ iff $|A| = |B|$

### 8.3 Counting Functions (⭐ FREQUENTLY ASKED)

From set $A$ ($|A| = m$) to set $B$ ($|B| = n$):

| Type | Count | Condition |
|---|---|---|
| Total functions | $n^m$ | Each of m elements has n choices |
| Injective functions | $n \cdot (n-1) \cdot (n-2) \cdots (n-m+1) = \frac{n!}{(n-m)!}$ = $P(n,m)$ | Need $m \leq n$ |
| Surjective functions | $\sum_{k=0}^{n} (-1)^k \binom{n}{k}(n-k)^m$ | Need $m \geq n$ |
| Bijective functions | $n!$ | Need $m = n$ |
| Non-injective functions | $n^m - P(n,m)$ | $m \leq n$ |
| Non-surjective functions | $n^m - \sum_{k=0}^{n}(-1)^k\binom{n}{k}(n-k)^m$ | $m \geq n$ |
| Monotonically increasing | $\binom{n}{m}$ | From totally ordered domain to ordered codomain |
| Monotonically non-decreasing | $\binom{n+m-1}{m}$ | Stars and bars! |

### 8.4 Composition and Inverse

- **Composition:** $(g \circ f)(x) = g(f(x))$
- **Inverse:** $f^{-1}$ exists iff $f$ is **bijective**
- If $f: A \rightarrow B$ and $g: B \rightarrow C$:
  - $g \circ f$ injective → $f$ injective
  - $g \circ f$ surjective → $g$ surjective
  - $g \circ f$ bijective → $f$ injective AND $g$ surjective
- $(g \circ f)^{-1} = f^{-1} \circ g^{-1}$ (when both inverses exist — shoes-socks!) 👟🧦
- Composition is **associative**: $h \circ (g \circ f) = (h \circ g) \circ f$
- Composition is generally **NOT commutative**: $g \circ f \neq f \circ g$

**Image and Pre-image:**
- $f(S) = \{f(x) \mid x \in S\}$ — image of set $S$
- $f^{-1}(T) = \{x \mid f(x) \in T\}$ — pre-image of set $T$ (exists even when $f^{-1}$ as function doesn't!)

### 8.4.1 Image / Pre-image Laws, Restriction and Corestriction 🧭

These identities are tiny, but GATE uses them in tricky true/false questions.

**Always true pre-image laws ✅**

For $T_1, T_2 \subseteq B$:

- $f^{-1}(T_1 \cup T_2) = f^{-1}(T_1) \cup f^{-1}(T_2)$
- $f^{-1}(T_1 \cap T_2) = f^{-1}(T_1) \cap f^{-1}(T_2)$
- $f^{-1}(B \setminus T_1) = A \setminus f^{-1}(T_1)$

So pre-image preserves **union, intersection, and complement exactly**.

**Image laws ⚠️**

For $S_1, S_2 \subseteq A$:

- $f(S_1 \cup S_2) = f(S_1) \cup f(S_2)$ always
- $f(S_1 \cap S_2) \subseteq f(S_1) \cap f(S_2)$ always
- Equality in the second line need **not** hold unless $f$ is injective on the relevant part

**Classic counterexample:**

Let $f(x) = x^2$ on $\mathbb{Z}$, $S_1 = \{-1\}$, $S_2 = \{1\}$.

- $S_1 \cap S_2 = \emptyset$, so $f(S_1 \cap S_2) = \emptyset$
- But $f(S_1) = f(S_2) = \{1\}$, so $f(S_1) \cap f(S_2) = \{1\}$

Hence $f(S_1 \cap S_2) \neq f(S_1) \cap f(S_2)$.

**Restriction and Corestriction**

- **Restriction:** If $A' \subseteq A$, then $f|_{A'}: A' \to B$ is defined by the same rule, but only on $A'$
- **Corestriction:** If $f(A) \subseteq B' \subseteq B$, we may view $f$ as a map $f: A \to B'$

> 💡 Restriction changes the **domain**. Corestriction changes the **codomain**, not the actual output values.

**Exam warning 🚨**

- A function inverse $f^{-1}: B \to A$ exists only when $f$ is bijective
- But the pre-image set $f^{-1}(T)$ exists for **every** function and every subset $T \subseteq B$
- These two meanings of $f^{-1}$ are different and often confused

### 8.5 Floor and Ceiling Functions (⭐⭐ VERY FREQUENTLY ASKED) 🏠

> 🔥 These appear EVERYWHERE in GATE — in algorithms (loop analysis), data structures (tree height), OS (page calculations), and discrete math directly!

**Floor function $\lfloor x \rfloor$**: Greatest integer $\leq x$ (round DOWN) ⬇️
**Ceiling function $\lceil x \rceil$**: Least integer $\geq x$ (round UP) ⬆️

| $x$ | $\lfloor x \rfloor$ | $\lceil x \rceil$ |
|---|---|---|
| 3.7 | 3 | 4 |
| -2.3 | -3 | -2 |
| 5 | 5 | 5 |
| -1 | -1 | -1 |
| 0.99 | 0 | 1 |

**Essential Properties (⭐ MUST KNOW):**

| Property | Formula |
|---|---|
| **Definition** | $\lfloor x \rfloor = n \iff n \leq x < n+1$ |
| **Definition** | $\lceil x \rceil = n \iff n-1 < x \leq n$ |
| **Relationship** | $\lceil x \rceil = \lfloor x \rfloor + 1$ if $x \notin \mathbb{Z}$; $\lceil x \rceil = \lfloor x \rfloor = x$ if $x \in \mathbb{Z}$ |
| **Negation** ⚠️ | $\lfloor -x \rfloor = -\lceil x \rceil$ and $\lceil -x \rceil = -\lfloor x \rfloor$ |
| **Integer add** | $\lfloor x + n \rfloor = \lfloor x \rfloor + n$ for $n \in \mathbb{Z}$ |
| **Inequality** | $x - 1 < \lfloor x \rfloor \leq x \leq \lceil x \rceil < x + 1$ |
| **Division** | $\lceil n/k \rceil = \lfloor (n+k-1)/k \rfloor = \lfloor (n-1)/k \rfloor + 1$ for positive integers |
| **Product** | $\lfloor x \rfloor \cdot \lfloor y \rfloor \leq \lfloor xy \rfloor$ (for positive $x, y$) |

**GATE Applications:**
- Binary tree of $n$ nodes: height $\geq \lfloor \log_2 n \rfloor$
- $n$ items in $k$ bins: at least one bin has $\geq \lceil n/k \rceil$ items (Pigeonhole!)
- Pages needed for $n$ bytes with page size $p$: $\lceil n/p \rceil$
- Levels in complete binary tree with $n$ leaves: $\lceil \log_2 n \rceil$
- Number of digits of $n$ in base $b$: $\lfloor \log_b n \rfloor + 1$

### 8.6 Special Functions (⭐ GATE Knowledge) 🎭

**Characteristic (Indicator) Function:**
For set $A \subseteq U$: $\chi_A(x) = \begin{cases} 1 & \text{if } x \in A \\ 0 & \text{if } x \notin A \end{cases}$

**Properties:** $\chi_{A \cap B} = \chi_A \cdot \chi_B$, $\chi_{A \cup B} = \chi_A + \chi_B - \chi_A \cdot \chi_B$, $\chi_{A'} = 1 - \chi_A$

> 🏭 **Production:** Boolean flags in programming are exactly characteristic functions! `isAdmin`, `isActive`, `isVerified` — each is $\chi_{AdminSet}$, etc.

**Identity Function:** $id_A: A \rightarrow A$ where $id_A(x) = x$ for all $x$
- $f \circ id_A = f$ and $id_B \circ f = f$ (identity is the neutral element for composition)

**Constant Function:** $f: A \rightarrow B$ where $f(x) = c$ for all $x \in A$ (always surjective onto $\{c\}$)

**Permutation Function:** A bijection from a set to itself. The set of all permutations of $\{1,\ldots,n\}$ forms $S_n$ (symmetric group — connects to Chapter 9!)

### 8.7 Recursively Defined Functions 🔄

> 💡 Functions can be defined by specifying base cases and recursive rules — just like in programming!

**Example 1 — Factorial:**
$f(0) = 1$, $f(n) = n \cdot f(n-1)$ for $n \geq 1$

**Example 2 — Fibonacci:**
$f(0) = 0$, $f(1) = 1$, $f(n) = f(n-1) + f(n-2)$ for $n \geq 2$

**Example 3 — Ackermann Function** (grows INSANELY fast 🚀):
$$A(0, n) = n + 1$$
$$A(m, 0) = A(m-1, 1)$$
$$A(m, n) = A(m-1, A(m, n-1))$$

> 🎯 **GATE Relevance:** The Ackermann function is total and computable but NOT primitive recursive — it grows faster than any primitive recursive function! This appears in Theory of Computation.

### 8.8 Fixed Points of Functions 📌

A **fixed point** of $f: A \rightarrow A$ is an element $a \in A$ such that $f(a) = a$.

**Key results for GATE:**
- Number of functions $f: \{1,\ldots,n\} \rightarrow \{1,\ldots,n\}$ with NO fixed point = $D_n$ (derangements!)
- Number of functions with AT LEAST ONE fixed point = $n^n - D_n$
- For monotone functions on a lattice: **Knaster-Tarski Theorem** — every monotone function on a complete lattice has a fixed point

> 🏭 **Production:** Fixed-point iteration is used in: dataflow analysis (compilers), abstract interpretation (static analyzers), DNS resolution, and iterative solving of equations. Lattice-based fixed points power tools like Facebook's Infer!

### 8.9 Functions and Cardinality (⭐ Connects to Countability) 🔢

| Statement | Meaning |
|---|---|
| $|A| \leq |B|$ | There exists an injection $A \hookrightarrow B$ |
| $|A| = |B|$ | There exists a bijection $A \leftrightarrow B$ |
| $|A| < |B|$ | Injection exists but NO bijection |

**Cantor-Schröder-Bernstein Theorem:** If $|A| \leq |B|$ and $|B| \leq |A|$, then $|A| = |B|$.
(If injection from A→B and injection from B→A exist, then bijection exists!)

**Cardinal Arithmetic:**
- $|A| + |B| = |A \cup B|$ when $A \cap B = \emptyset$ (disjoint union)
- $|A| \cdot |B| = |A \times B|$
- $|A|^{|B|}$ = number of functions from $B$ to $A$
- For infinite: $\aleph_0 + \aleph_0 = \aleph_0$, $\aleph_0 \cdot \aleph_0 = \aleph_0$, $2^{\aleph_0} = \mathfrak{c}$ (continuum)

---

# 🏗️ PART 3: GROUP THEORY

> 🎭 **"Group Theory is the mathematics of symmetry — and symmetry is EVERYWHERE, from snowflakes to encryption algorithms!"**

---

## Chapter 9: Groups (⭐ GATE CS IMPORTANT) 🔐

> 💡 **Beginner Analogy — A Rubik's Cube 🧩:**
> A Rubik's cube is the PERFECT example of a group!
> - **Closure:** Any sequence of moves is still a valid position ✅
> - **Associativity:** (Turn top then turn right) then turn front = Turn top then (turn right then turn front) ✅
> - **Identity:** "Do nothing" is a valid move that changes nothing ✅
> - **Inverse:** Every move can be undone (turn right → turn left) ✅
>
> The Rubik's cube group has exactly **43,252,003,274,489,856,000 elements** (43 quintillion positions!) and was fully characterized using group theory! 🤯

> 🏭 **Production Use Cases:**
> - **RSA Encryption** 🔒 (used in HTTPS, WhatsApp, banking): Relies on group theory over $\mathbb{Z}_n^*$ — the difficulty of factoring large numbers keeps your data safe!
> - **Elliptic Curve Cryptography** 🛡️ (used in Bitcoin, Signal app): Points on elliptic curves form a group!
> - **Error-Correcting Codes** 📡 (used in 4G/5G, QR codes, space communication): Hamming codes and Reed-Solomon codes use finite groups
> - **Crystal Structure Analysis** 🔬 (materials science): 230 space groups classify all possible crystal structures

### 9.1 Binary Operation

A **binary operation** $*$ on set $S$ is a function $*: S \times S \rightarrow S$ (closed by definition).

Properties:
- **Closure:** $\forall a, b \in S: a * b \in S$
- **Associativity:** $\forall a, b, c: (a * b) * c = a * (b * c)$
- **Identity:** $\exists e \in S: \forall a: a * e = e * a = a$
- **Inverse:** $\forall a \in S, \exists a^{-1}: a * a^{-1} = a^{-1} * a = e$
- **Commutativity:** $\forall a, b: a * b = b * a$

### 9.2 Algebraic Structures Hierarchy

| Structure | Properties |
|---|---|
| **Groupoid/Magma** | Closure |
| **Semigroup** | Closure + Associativity |
| **Monoid** | Closure + Associativity + Identity |
| **Group** | Closure + Associativity + Identity + Inverse |
| **Abelian Group** | Group + Commutativity |

### 9.2.1 Semigroups & Monoids — Detailed (⭐ EXPLICITLY in GATE Syllabus!) 🏗️

> 🔥 The GATE CS syllabus explicitly mentions "**Monoids, Groups**" — semigroups and monoids are part of the syllabus!

**Semigroup $(S, *)$:** Closed + Associative.

**Examples:**
| Structure | Semigroup? | Monoid? | Group? |
|---|---|---|---|
| $(\mathbb{N}, +)$ with $\mathbb{N} = \{1,2,3,...\}$ | ✅ | ❌ (no 0) | ❌ |
| $(\mathbb{N}_0, +)$ with $\mathbb{N}_0 = \{0,1,2,...\}$ | ✅ | ✅ ($e=0$) | ❌ (no inverse for 1) |
| $(\mathbb{Z}, +)$ | ✅ | ✅ ($e=0$) | ✅ |
| $(\Sigma^*, \cdot)$ strings with concatenation | ✅ | ✅ ($e = \varepsilon$) | ❌ (no inverse for "a") |
| $(\mathbb{Z}, -)$ subtraction | ❌ (not assoc!) | ❌ | ❌ |
| $(\{true, false\}, \wedge)$ | ✅ | ✅ ($e = true$) | ❌ |

> 🏭 **Production:** String monoid $(\Sigma^*, \cdot)$ is fundamental in **Theory of Computation** — every language is a subset of this monoid! The identity element $\varepsilon$ (empty string) is its identity.

**Free Monoid:** $\Sigma^*$ = all finite strings over alphabet $\Sigma$ with concatenation. This is the **most important monoid** in CS — it connects Discrete Math to Formal Languages!

**Semigroup/Monoid Homomorphism:** $f: (S, *) \rightarrow (T, \circ)$ where $f(a * b) = f(a) \circ f(b)$.
- For monoids: additionally $f(e_S) = e_T$

### 9.2.2 Cayley Table (Operation Table) Analysis (⭐ GATE PYQ) 📊

> GATE frequently gives an operation table and asks: "Is this a group?"

**Reading a Cayley table for a set $\{a, b, c\}$ with operation $*$:**

| $*$ | a | b | c |
|---|---|---|---|
| **a** | a | b | c |
| **b** | b | c | a |
| **c** | c | a | b |

**Checklist:**
1. ✅ **Closure:** All entries are from $\{a, b, c\}$
2. ✅ **Associativity:** Must check all triples (tedious — shortcut: if it's isomorphic to $\mathbb{Z}_3$, done!)
3. ✅ **Identity:** Row for $a$ and column for $a$ reproduce the header → $a$ is identity
4. ✅ **Inverse:** Each row/column is a permutation of $\{a, b, c\}$ (Latin square property!) → every element has an inverse
5. ✅ **Commutativity:** Table is symmetric across the diagonal → Abelian

> 🎯 **Quick Tests:**
> - **Latin Square Property:** If every element appears exactly once in each row and each column, the structure is a **quasigroup** (close to being a group!)
> - A finite quasigroup with identity = group ✅
> - If the table is symmetric → commutative (Abelian)

### 9.2.3 Latin Squares (⭐ GATE Awareness) 🔲

A **Latin square** of order $n$ is an $n \times n$ array filled with $n$ different symbols, each occurring exactly once in each row and exactly once in each column.

- The Cayley table of a finite group is ALWAYS a Latin square ✅ (cancellation property!)
- Not every Latin square is a group table (may fail associativity)
- Number of Latin squares of order $n$: $L(1) = 1$, $L(2) = 2$, $L(3) = 12$, $L(4) = 576$
- **Sudoku** is a Latin square with additional box constraints! 🧩

> 🏭 **Application:** Latin squares are used in **experimental design** (agriculture, clinical trials), **error-correcting codes**, and scheduling (round-robin tournaments).

### 9.3 Important Group Properties

- Identity element is **unique**
- Inverse of each element is **unique**
- $(a^{-1})^{-1} = a$
- $(a * b)^{-1} = b^{-1} * a^{-1}$ (shoes-socks property)
- **Cancellation:** $a * b = a * c \Rightarrow b = c$

### 9.4 Examples of Groups

| Set | Operation | Group? | Identity | Abelian? |
|---|---|---|---|---|
| $(\mathbb{Z}, +)$ | Addition | Yes | 0 | Yes |
| $(\mathbb{Z}, \times)$ | Multiplication | No (no inverse for 2) | — | — |
| $(\mathbb{Q} \setminus \{0\}, \times)$ | Multiplication | Yes | 1 | Yes |
| $(\mathbb{Z}_n, +_n)$ | Addition mod n | Yes | 0 | Yes |
| $(\mathbb{Z}_n^*, \times_n)$ | Multiplication mod n | Yes (if n prime) | 1 | Yes |

### 9.5 Order of a Group and Order of an Element

- **Order of group $G$**: $|G|$ = number of elements
- **Order of element $a$**: Smallest positive integer $k$ such that $a^k = e$

### 9.6 Subgroups

$H$ is a **subgroup** of $G$ (written $H \leq G$) if $H \subseteq G$ and $H$ is a group under the same operation.

**Subgroup Test:** $H \leq G$ iff $\forall a, b \in H: a * b^{-1} \in H$ (one-step test)

### 9.7 Lagrange's Theorem (⭐ VERY IMPORTANT)

> If $H$ is a subgroup of finite group $G$, then $|H|$ divides $|G|$.

**Corollaries:**
- Order of any element divides $|G|$
- A group of prime order $p$ has no proper subgroups (only $\{e\}$ and $G$)
- A group of prime order is **cyclic**
- Number of subgroups divides... no, only the ORDER of subgroups divides $|G|$
- For prime $p$: $a^p = a$ in $\mathbb{Z}_p$ (**Fermat's Little Theorem**)

### 9.8 Cyclic Groups

A group $G$ is **cyclic** if there exists an element $g$ (called a **generator**) such that every element of $G$ can be written as $g^k$ for some integer $k$.

**Properties of cyclic groups:**
- Every cyclic group is abelian
- $\mathbb{Z}_n$ under addition mod $n$ is cyclic (generator = 1)
- If $|G| = n$, then $a$ is a generator iff $\gcd(\text{order}(a), n) = n$ iff order$(a) = n$
- Number of generators of $\mathbb{Z}_n = \phi(n)$ (Euler's totient)
- Every subgroup of a cyclic group is cyclic
- For each divisor $d$ of $n$, there is exactly one subgroup of order $d$

### 9.9 Cosets and Quotient Groups

**Left coset** of $H$ in $G$: $aH = \{a * h \mid h \in H\}$

- All cosets have the same size: $|aH| = |H|$
- Cosets partition $G$
- Number of distinct cosets = $|G|/|H|$ (called the **index** of $H$ in $G$)

### 9.10 Normal Subgroups and Homomorphisms

- $H$ is **normal** in $G$ if $\forall g \in G: gH = Hg$
- Every subgroup of an abelian group is normal
- **Homomorphism:** $f: G \rightarrow G'$ where $f(a * b) = f(a) * f(b)$
- **Isomorphism:** Bijective homomorphism

### 9.10.1 Direct Product of Groups (⭐ GATE) 🔗

**Definition:** Given groups $(G_1, *_1)$ and $(G_2, *_2)$, the **direct product** $G_1 \times G_2$ is:
- Set: $\{(g_1, g_2) \mid g_1 \in G_1, g_2 \in G_2\}$
- Operation: $(a_1, a_2) * (b_1, b_2) = (a_1 *_1 b_1, a_2 *_2 b_2)$
- Identity: $(e_1, e_2)$
- Inverse: $(a_1, a_2)^{-1} = (a_1^{-1}, a_2^{-1})$

**Order:** $|G_1 \times G_2| = |G_1| \cdot |G_2|$

**⭐ KEY RESULT:** $\mathbb{Z}_m \times \mathbb{Z}_n \cong \mathbb{Z}_{mn}$ **iff** $\gcd(m, n) = 1$

**Example:** $\mathbb{Z}_2 \times \mathbb{Z}_3 \cong \mathbb{Z}_6$ ✅ (since $\gcd(2,3) = 1$)
But $\mathbb{Z}_2 \times \mathbb{Z}_2 \ncong \mathbb{Z}_4$ ❌ (since $\gcd(2,2) = 2 \neq 1$)

### 9.10.2 Klein Four-Group $V_4$ (⭐ GATE CLASSIC) 🔲

$V_4 = \mathbb{Z}_2 \times \mathbb{Z}_2 = \{(0,0), (0,1), (1,0), (1,1)\}$

| $+$ | (0,0) | (0,1) | (1,0) | (1,1) |
|---|---|---|---|---|
| **(0,0)** | (0,0) | (0,1) | (1,0) | (1,1) |
| **(0,1)** | (0,1) | (0,0) | (1,1) | (1,0) |
| **(1,0)** | (1,0) | (1,1) | (0,0) | (0,1) |
| **(1,1)** | (1,1) | (1,0) | (0,1) | (0,0) |

**Properties:**
- Order 4, Abelian, NOT cyclic (no element has order 4 — all non-identity elements have order 2!)
- Every element is its own inverse
- $V_4$ is the **smallest non-cyclic group**
- Two groups of order 4: $\mathbb{Z}_4$ (cyclic) and $V_4$ (non-cyclic)

### 9.10.3 Classification of Small Groups (⭐ GATE Quick Reference) 📋

| Order | Number of Groups | Groups (up to isomorphism) |
|---|---|---|
| 1 | 1 | $\{e\}$ (trivial) |
| 2 | 1 | $\mathbb{Z}_2$ |
| 3 | 1 | $\mathbb{Z}_3$ |
| 4 | 2 | $\mathbb{Z}_4$, $V_4 = \mathbb{Z}_2 \times \mathbb{Z}_2$ |
| 5 | 1 | $\mathbb{Z}_5$ |
| 6 | 2 | $\mathbb{Z}_6$, $S_3$ (smallest non-abelian!) |
| 7 | 1 | $\mathbb{Z}_7$ |
| 8 | 5 | $\mathbb{Z}_8$, $\mathbb{Z}_4 \times \mathbb{Z}_2$, $\mathbb{Z}_2^3$, $D_4$, $Q_8$ |

> 🎯 **Pattern:** Groups of prime order $p$ are ALWAYS cyclic → only $\mathbb{Z}_p$!
> Groups of order $p^2$ ($p$ prime) are ALWAYS abelian!

### 9.10.4 Cauchy's Theorem & Sylow Theorems (⭐ Awareness Level) 🏛️

**Cauchy's Theorem:** If $p$ is a prime dividing $|G|$, then $G$ has an element of order $p$.

> **Converse of Lagrange?** Lagrange says subgroup order divides group order. But the CONVERSE is FALSE in general — $A_4$ (order 12) has no subgroup of order 6! However, **Cauchy guarantees** subgroups of prime order always exist.

**Sylow's First Theorem:** If $p^k$ divides $|G|$, then $G$ has a subgroup of order $p^k$.

> 🎯 **For GATE:** You mainly need Lagrange's theorem and Cauchy's theorem. Sylow is rarely tested directly but good to know at conceptual level.

### 9.11 Permutation Groups (⭐ ASKED IN GATE)

A permutation of $\{1, 2, \ldots, n\}$ is a bijection from the set to itself.

**Cycle Notation:** $\sigma = (1\ 3\ 4\ 2)$ means $1 \to 3 \to 4 \to 2 \to 1$.
- **Disjoint cycles** commute: $(1\ 2)(3\ 4) = (3\ 4)(1\ 2)$
- Every permutation decomposes uniquely (up to order) into disjoint cycles
- A $k$-cycle has order $k$
- **Order of a permutation = LCM of lengths of its disjoint cycles**

**Example:** $\sigma = (1\ 2\ 3)(4\ 5)$ has order $\text{lcm}(3, 2) = 6$

**Transpositions:** A 2-cycle $(i\ j)$.
- Every permutation = product of transpositions
- **Even permutation:** even number of transpositions; **Odd:** odd number
- A $k$-cycle = $(k-1)$ transpositions. So $k$-cycle is even iff $k$ is odd!

**Symmetric Group $S_n$:** All permutations of $\{1, \ldots, n\}$, $|S_n| = n!$, non-abelian for $n \geq 3$.
**Alternating Group $A_n$:** Even permutations only, $|A_n| = n!/2$.

### 9.12 Euler's Totient Function (⭐ GATE FAVORITE)

$\phi(n)$ = number of integers from 1 to $n$ that are coprime to $n$.

$$\phi(n) = n \prod_{p | n} \left(1 - \frac{1}{p}\right)$$

**Special cases:** $\phi(1) = 1$, $\phi(p) = p - 1$ for prime $p$, $\phi(p^k) = p^{k-1}(p-1)$, $\phi(mn) = \phi(m)\phi(n)$ when $\gcd(m,n) = 1$.

| $n$ | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 12 | 15 | 20 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| $\phi(n)$ | 1 | 1 | 2 | 2 | 4 | 2 | 6 | 4 | 6 | 4 | 4 | 8 | 8 |

**Euler's Theorem:** If $\gcd(a, n) = 1$, then $a^{\phi(n)} \equiv 1 \pmod{n}$

**Fermat's Little Theorem (prime $p$):** $a^{p-1} \equiv 1 \pmod{p}$ when $\gcd(a,p) = 1$. Equivalently: $a^p \equiv a \pmod{p}$ for all $a$.

**Generators of $\mathbb{Z}_n$:** $a$ is a generator of $(\mathbb{Z}_n, +)$ iff $\gcd(a, n) = 1$. Number of generators = $\phi(n)$.

### 9.12.1 Cryptography via Group Theory & Modular Arithmetic 🔐

> 🏭 RSA, Diffie-Hellman, digital signatures, blockchain wallets, HTTPS handshakes — all of them depend on these discrete math facts.

**Multiplicative Order**

If $a \in \mathbb{Z}_n^*$, the **order** of $a$ modulo $n$ is the smallest positive integer $k$ such that:

$$a^k \equiv 1 \pmod{n}$$

By Lagrange's theorem applied to the group $\mathbb{Z}_n^*$:

$$\operatorname{ord}_n(a) \mid |\mathbb{Z}_n^*| = \phi(n)$$

So the order of an element always divides $\phi(n)$.

**Fast Modular Exponentiation (Square-and-Multiply) ⚡**

To compute $a^m \bmod n$ efficiently:

1. Write $m$ in binary
2. Repeatedly square modulo $n$
3. Multiply only for the 1-bits

This reduces computation from roughly $m$ multiplications to $O(\log m)$ multiplications.

> Example: $3^{13} \bmod 7$. Since $13 = 1101_2$, compute $3, 3^2, 3^4, 3^8 \pmod 7$ and combine the needed powers.

**RSA in Discrete-Math Language 🧩**

1. Choose distinct primes $p, q$
2. Set $n = pq$
3. Compute $\phi(n) = (p-1)(q-1)$
4. Choose $e$ with $\gcd(e, \phi(n)) = 1$
5. Find $d$ such that:

$$ed \equiv 1 \pmod{\phi(n)}$$

6. Public key = $(n,e)$, private key = $d$
7. Encryption: $c \equiv m^e \pmod n$
8. Decryption: $m \equiv c^d \pmod n$

Why does decryption work? Because Euler/Fermat structure ensures $m^{ed} \equiv m \pmod n$ under the standard RSA conditions.

**Primitive Roots & Cyclicity 🌱**

- The multiplicative group $\mathbb{Z}_p^*$ is cyclic for prime $p$
- Any generator of $\mathbb{Z}_p^*$ is called a **primitive root modulo $p$**
- If $g$ is a generator, every nonzero residue mod $p$ is some power of $g$

This is the basis of **discrete logarithm** problems used in Diffie-Hellman and ElGamal.

**CRT Speedup Idea 🚀**

In RSA, instead of computing modulo $n = pq$ directly, one often computes separately modulo $p$ and modulo $q$, then combines answers using the Chinese Remainder Theorem. This makes decryption much faster.

> 🎯 GATE pattern: If a question mixes modular inverses, Euler's theorem, and encryption/decryption steps, it is testing **group structure**, not just arithmetic tricks.

### 9.13 Rings and Fields (Brief — Occasionally Asked in GATE)

**Ring** $(R, +, \cdot)$: $(R, +)$ is abelian group + multiplication is associative + distributive.
- **Commutative ring:** multiplication is commutative. **Ring with unity:** has multiplicative identity (1).

**Integral Domain:** Commutative ring with unity and **no zero divisors** ($ab = 0 \Rightarrow a = 0$ or $b = 0$).
- $\mathbb{Z}$ is an integral domain. $\mathbb{Z}_6$ is NOT ($2 \times 3 = 0 \pmod{6}$). $\mathbb{Z}_n$ is integral domain iff $n$ is prime.

**Field:** Commutative ring with unity where every nonzero element has multiplicative inverse.
- Examples: $\mathbb{Q}, \mathbb{R}, \mathbb{C}, \mathbb{Z}_p$ (for prime $p$). Every finite integral domain is a field.

**Hierarchy:** $\text{Field} \subset \text{Integral Domain} \subset \text{Commutative Ring with Unity} \subset \text{Ring} \subset \text{Group (under +)}$

### 9.14 Homomorphism & Isomorphism — Advanced (⭐ GATE)

If $f: G \rightarrow G'$ is a homomorphism:
- $f(e_G) = e_{G'}$, $f(a^{-1}) = (f(a))^{-1}$, $f(a^n) = (f(a))^n$
- **Kernel:** $\ker(f) = \{a \in G \mid f(a) = e_{G'}\}$ — a normal subgroup of $G$
- **Image:** $\text{Im}(f)$ is a subgroup of $G'$
- $f$ is injective iff $\ker(f) = \{e_G\}$
- **First Isomorphism Theorem:** $G / \ker(f) \cong \text{Im}(f)$

**Important Isomorphism Results:**
- $\mathbb{Z}_m \times \mathbb{Z}_n \cong \mathbb{Z}_{mn}$ iff $\gcd(m,n) = 1$
- All groups of prime order $p$ are isomorphic to $\mathbb{Z}_p$ (and cyclic)
- 2 groups of order 4: $\mathbb{Z}_4$ and $\mathbb{Z}_2 \times \mathbb{Z}_2$ (Klein four-group)
- 2 groups of order 6: $\mathbb{Z}_6$ and $S_3$

### 9.15 Number Theory Essentials (⭐ CONNECTS TO GROUP THEORY)

**GCD Properties:**
- $\gcd(a, b) = \gcd(b, a \mod b)$ (Euclidean algorithm), $\gcd(a, 0) = a$
- $\gcd(a, b) \cdot \text{lcm}(a, b) = |a \cdot b|$
- **Bézout's identity:** $\exists x, y \in \mathbb{Z}: ax + by = \gcd(a, b)$

**Extended Euclidean Algorithm:** Finds $x, y$ such that $ax + by = \gcd(a, b)$ — used for modular inverses.

**Modular Arithmetic:**
- $(a + b) \mod n = ((a \mod n) + (b \mod n)) \mod n$
- $(a \cdot b) \mod n = ((a \mod n) \cdot (b \mod n)) \mod n$
- $a$ has multiplicative inverse mod $n$ iff $\gcd(a, n) = 1$

**Chinese Remainder Theorem:** If $n_1, n_2, \ldots, n_k$ are pairwise coprime:
$$x \equiv a_1 \pmod{n_1}, \quad x \equiv a_2 \pmod{n_2}, \quad \ldots$$
has a unique solution modulo $N = n_1 n_2 \cdots n_k$.

---

# 🎲 PART 4: COMBINATORICS

> 🎭 **"Combinatorics answers the question every programmer asks: HOW MANY ways can this happen?"**

> 🌍 **Combinatorics powers:** Password strength calculators 🔑, lottery probability 🎰, A/B testing at Netflix 🎬, genome sequencing 🧬, compiler optimization, network routing possibilities, and even the shuffle algorithm on Spotify! 🎵

---

## Chapter 10: Counting Principles (⭐⭐ HIGH WEIGHTAGE) 📊

> 💡 **Beginner Analogy — The Ice Cream Shop 🍦:**
> Combinatorics is like figuring out how many ice cream combinations you can make:
> - 3 flavors × 2 cones × 2 toppings = 12 combos (Product Rule!) 🍦
> - "Want a cone OR a cup?" = 6 + 6 = 12 (Sum Rule if sizes differ!)
> - "How many sundaes with at least one topping from 5?" = $2^5 - 1 = 31$ (Subtract the "no toppings" case!)

### 10.1 Basic Counting Principles

| Principle | When to Use | Formula |
|---|---|---|
| **Sum Rule** | Choose from A OR B (disjoint) | $|A| + |B|$ |
| **Product Rule** | Choose from A AND B (independent) | $|A| \times |B|$ |
| **Subtraction Rule** | Total minus unwanted | $|A \cup B| = |A| + |B| - |A \cap B|$ |
| **Division Rule** | Objects counted multiple times | Total / overcounting factor |

### 10.2 Permutations

**Permutation** = arrangement where **order matters**

| Type | Formula |
|---|---|
| $n$ distinct objects, arrange all | $n!$ |
| $n$ distinct objects, choose $r$ | $P(n,r) = \frac{n!}{(n-r)!}$ |
| With repetition ($n_1$ alike, $n_2$ alike, ...) | $\frac{n!}{n_1! \cdot n_2! \cdots n_k!}$ |
| Circular permutations of $n$ | $(n-1)!$ |
| Circular + reflection (necklace) | $\frac{(n-1)!}{2}$ |

### 10.3 Combinations

**Combination** = selection where **order doesn't matter**

$$\binom{n}{r} = C(n,r) = \frac{n!}{r!(n-r)!}$$

**Key Properties:**
- $\binom{n}{0} = \binom{n}{n} = 1$
- $\binom{n}{r} = \binom{n}{n-r}$ (symmetry)
- $\binom{n}{r} = \binom{n-1}{r-1} + \binom{n-1}{r}$ (**Pascal's identity**)
- $\sum_{r=0}^{n} \binom{n}{r} = 2^n$
- $\sum_{r=0}^{n} (-1)^r \binom{n}{r} = 0$

### 10.4 Binomial Theorem

$$(x + y)^n = \sum_{k=0}^{n} \binom{n}{k} x^{n-k} y^k$$

### 10.4.1 Important Binomial Coefficient Identities (⭐⭐ GATE TESTED) 🧮

| Identity | Formula | Name |
|---|---|---|
| **Symmetry** | $\binom{n}{r} = \binom{n}{n-r}$ | Reflection |
| **Pascal's** | $\binom{n}{r} = \binom{n-1}{r-1} + \binom{n-1}{r}$ | Recursive |
| **Vandermonde's** ⭐ | $\sum_{k=0}^{r} \binom{m}{k}\binom{n}{r-k} = \binom{m+n}{r}$ | Convolution |
| **Hockey Stick** 🏒 | $\sum_{i=r}^{n} \binom{i}{r} = \binom{n+1}{r+1}$ | Diagonal sum |
| **Sum of all** | $\sum_{r=0}^{n} \binom{n}{r} = 2^n$ | Total subsets |
| **Alternating sum** | $\sum_{r=0}^{n} (-1)^r \binom{n}{r} = 0$ | Cancellation |
| **Sum of squares** | $\sum_{k=0}^{n} \binom{n}{k}^2 = \binom{2n}{n}$ | Vandermonde special case |
| **Weighted sum** | $\sum_{k=0}^{n} k\binom{n}{k} = n \cdot 2^{n-1}$ | Differentiation trick |
| **Upper summation** | $\sum_{j=0}^{r} \binom{n+j}{j} = \binom{n+r+1}{r}$ | Parallel summation |

> 💡 **Vandermonde's Identity — Intuition 🎯:**
> Choose $r$ people from a group of $m$ men and $n$ women.
> LHS: choose $k$ men and $r-k$ women, sum over all $k$.
> RHS: directly choose $r$ from $m+n$ total.
> Both count the same thing! ✅

> 🏒 **Hockey Stick — Intuition:**
> In Pascal's triangle, sum along a diagonal = the entry one step below and to the right. Looks like a hockey stick! 🏒

### 10.4.2 Lucas' Theorem (⭐ For competitive + GATE DA) 🔢

> For prime $p$: $\binom{m}{n} \equiv \prod_{i=0}^{k} \binom{m_i}{n_i} \pmod{p}$
>
> where $m = m_k p^k + \cdots + m_1 p + m_0$ and $n = n_k p^k + \cdots + n_1 p + n_0$ are base-$p$ representations.

**Special case:** $\binom{m}{n} \equiv 0 \pmod{p}$ iff any digit of $n$ in base $p$ exceeds the corresponding digit of $m$.

**Quick application:** $\binom{10}{3} \pmod{2}$: $10 = 1010_2$, $3 = 0011_2$. Since third digit of $n$ (1) > third digit of $m$ (0), $\binom{10}{3} \equiv 0 \pmod{2}$. Indeed $\binom{10}{3} = 120$ is even! ✅

### 10.4.3 Double Counting (Counting in Two Ways) 🔄

> 💡 **Principle:** If you can count the same quantity in two different ways, the two expressions must be equal!

**Example — Handshaking Lemma proof:**
Count pairs $(v, e)$ where vertex $v$ is an endpoint of edge $e$.
- Counting by vertices: $\sum_{v} \deg(v)$ (each vertex contributes its degree)
- Counting by edges: $2|E|$ (each edge contributes 2 endpoints)
- Therefore: $\sum_{v} \deg(v) = 2|E|$ ✅

**Example — Vandermonde's Identity proof** by double counting: Choose $r$ from $m+n$ people = choose $k$ from group 1 ($m$) and $r-k$ from group 2 ($n$), sum over $k$.

### 10.5 Stars and Bars (⭐ IMPORTANT)

**Number of ways to distribute $n$ identical objects into $r$ distinct boxes:**
- Each box can have 0 or more: $\binom{n+r-1}{r-1}$
- Each box has at least 1: $\binom{n-1}{r-1}$

**Distributing Objects into Boxes (DODB Framework):**

| Objects | Boxes | No Restriction | At least 1 per box |
|---|---|---|---|
| Distinct | Distinct | $r^n$ | Surjections = $\sum(-1)^k\binom{r}{k}(r-k)^n$ |
| Identical | Distinct | $\binom{n+r-1}{r-1}$ | $\binom{n-1}{r-1}$ |
| Distinct | Identical | Bell number $B_n$ (for $r \geq n$) / Stirling | Stirling numbers $S(n,r)$ |
| Identical | Identical | Partition of $n$ into at most $r$ parts | Partition of $n$ into exactly $r$ parts |

### 10.6 Inclusion-Exclusion Principle (PIE) (⭐⭐ KEY TECHNIQUE)

$$|A_1 \cup A_2 \cup \cdots \cup A_n| = \sum|A_i| - \sum|A_i \cap A_j| + \sum|A_i \cap A_j \cap A_k| - \cdots + (-1)^{n-1}|A_1 \cap \cdots \cap A_n|$$

**Applications:**
- Counting surjections (onto functions)
- Counting derangements
- Euler's totient function

### 10.7 Derangements (⭐ GATE FAVORITE) 🔀

> 💡 **Beginner Analogy — Secret Santa Gone Wrong 🎅:**
> Imagine 5 friends doing Secret Santa. A **derangement** is when NO ONE gets their own name! Everyone gets someone else's gift. What's the probability?
> - Total arrangements: $5! = 120$
> - Derangements: $D_5 = 44$
> - Probability = 44/120 ≈ 36.67%
>
> Fun fact: As n gets large, the probability of a derangement approaches $1/e ≈ 36.79\%$ — this is one of the most beautiful results in combinatorics! ✨
>
> 🏭 **Production Use:** Derangement concepts appear in **cryptographic permutations**, **random shuffling algorithms** (Fisher-Yates shuffle used in every card game app), and **testing random number generators**!

A **derangement** is a permutation where **no element appears in its original position**.

$$D_n = n! \sum_{k=0}^{n} \frac{(-1)^k}{k!} = n!\left(1 - 1 + \frac{1}{2!} - \frac{1}{3!} + \cdots + \frac{(-1)^n}{n!}\right)$$

| $n$ | $D_n$ |
|---|---|
| 1 | 0 |
| 2 | 1 |
| 3 | 2 |
| 4 | 9 |
| 5 | 44 |

**Recurrence:** $D_n = (n-1)(D_{n-1} + D_{n-2})$, with $D_1 = 0, D_2 = 1$

### 10.7.1 Permutations with Forbidden Positions (⭐ Generalized Derangement) 🚫

> Derangements are a special case where position $i$ is forbidden for element $i$. The general problem: given a set of forbidden (element, position) pairs, count valid permutations.

**Method — Inclusion-Exclusion on forbidden positions:**
Let $A_i$ = set of permutations where element $i$ occupies its forbidden position.
$$\text{Valid permutations} = n! - |A_1 \cup A_2 \cup \cdots \cup A_n|$$
Apply PIE to compute $|A_1 \cup \cdots \cup A_n|$.

**GATE Example:** Place rooks on an $n \times n$ chessboard (one per row, one per column) avoiding certain squares.
- This is exactly counting permutations with forbidden positions!
- **Rook polynomial** $r_k$ = number of ways to place $k$ non-attacking rooks on forbidden squares
- Valid permutations = $\sum_{k=0}^{n} (-1)^k r_k (n-k)!$

> 🎯 For basic GATE, focus on **derangements** (all diagonal forbidden) and simple rook polynomial problems.

### 10.8 Pigeonhole Principle (⭐ TRICKY IN GATE) 🐦

> 💡 **Beginner Analogy — Musical Chairs 🪑:**
> If there are 10 children and 9 chairs, at least ONE chair MUST have more than one child trying to sit! That's the Pigeonhole Principle — simple but POWERFUL!
>
> 🌍 **Fun Real-World Applications:**
> - **Birthday Paradox** 🎂: In a group of 367 people, at least 2 MUST share a birthday (only 366 possible birthdays!). But amazingly, you only need **23 people** for a 50% chance!
> - **Hash Collisions** 🔑: If you have more data items than hash table slots, collisions MUST happen (this is why hash tables need collision handling!)
> - **Data Compression** 📁: Not all files can be compressed — by pigeonhole, if compression maps n-bit files to (n-1)-bit files, some files MUST map to the same compressed output!
> - **Lossless compression limit** 💾: ZIP can't compress every file — pigeonhole proves it!

> If $n$ items are placed into $m$ containers and $n > m$, then at least one container has more than one item.

**Generalized:** If $n$ items → $m$ containers, at least one has $\geq \lceil n/m \rceil$ items.

**Examples (GATE Level):**
1. In any group of 13 people, at least 2 share a birth month.
2. In 5 integers, at least 2 have the same remainder mod 4 (since only 4 possible remainders).

### 10.9 Recurrence Relations

A **recurrence relation** defines a sequence where each term depends on previous terms.

**Solving methods:**
1. **Iteration/Substitution:** Expand and find pattern
2. **Characteristic equation** (for linear homogeneous with constant coefficients)
3. **Master theorem** (for divide-and-conquer)
4. **Generating functions** (convert to algebra — most powerful!)

### 10.9.1 Substitution (Iteration) Method — Worked Example 🔍

> 💡 Expand the recurrence repeatedly until you see the pattern!

**Example:** $T(n) = T(n-1) + n$, $T(0) = 0$

| Step | Expansion |
|---|---|
| $T(n)$ | $= T(n-1) + n$ |
| | $= [T(n-2) + (n-1)] + n = T(n-2) + (n-1) + n$ |
| | $= T(n-3) + (n-2) + (n-1) + n$ |
| After $k$ steps | $= T(n-k) + \sum_{i=n-k+1}^{n} i$ |
| Set $k = n$ | $= T(0) + \sum_{i=1}^{n} i = \frac{n(n+1)}{2}$ |

**Verify:** $T(3) = T(2) + 3 = [T(1) + 2] + 3 = [T(0) + 1 + 2] + 3 = 6 = \frac{3 \times 4}{2}$ ✅

> 🎯 **GATE Tip:** After finding the pattern by iteration, ALWAYS verify with the original recurrence + initial condition!

**Solving linear homogeneous recurrences:** $a_n = c_1 a_{n-1} + c_2 a_{n-2}$

Characteristic equation: $r^2 = c_1 r + c_2$

- **Distinct roots** $r_1, r_2$: $a_n = A r_1^n + B r_2^n$
- **Repeated root** $r$: $a_n = (A + Bn) r^n$

### 10.9.2 Recurrence Relations — Which Method to Use? 🧭

| Recurrence Type | Best Method | Example |
|---|---|---|
| $a_n = c_1 a_{n-1} + c_2 a_{n-2}$ (linear, homogeneous) | **Characteristic equation** | Fibonacci |
| $a_n = c_1 a_{n-1} + f(n)$ (non-homogeneous) | **Undetermined coefficients** or GF | $a_n = 2a_{n-1} + 3^n$ |
| $T(n) = aT(n/b) + f(n)$ (divide-and-conquer) | **Master theorem** | Merge sort |
| Any recurrence, need exact form | **Generating functions** | Most general method |
| Simple, small terms | **Iteration + guess + verify** | $T(n) = T(n-1) + n$ |

> 🎯 **For GATE:** Master the characteristic equation (90% of questions), then GFs for the rest!

### 10.10 Generating Functions (⭐⭐ EXPLICITLY IN GATE SYLLABUS)

> 🏭 **Generating functions appear EXPLICITLY in the GATE CS syllabus and have been asked multiple times since 2015.**

**Ordinary Generating Function (OGF):** For sequence $a_0, a_1, a_2, \ldots$:

$$A(x) = \sum_{n=0}^{\infty} a_n x^n = a_0 + a_1 x + a_2 x^2 + \cdots$$

**Important Generating Functions (MUST KNOW):**

| Sequence | OGF |
|---|---|
| $1, 1, 1, 1, \ldots$ | $\frac{1}{1-x}$ |
| $1, r, r^2, r^3, \ldots$ | $\frac{1}{1-rx}$ |
| $\binom{n}{0}, \binom{n}{1}, \ldots, \binom{n}{n}$ (finite) | $(1+x)^n$ |
| $\binom{n+0}{0}, \binom{n+1}{1}, \binom{n+2}{2}, \ldots$ | $\frac{1}{(1-x)^{n+1}}$ |
| $1, 1, \frac{1}{2!}, \frac{1}{3!}, \ldots$ | $e^x$ |
| $1, -1, 1, -1, \ldots$ | $\frac{1}{1+x}$ |
| Fibonacci: $0, 1, 1, 2, 3, 5, \ldots$ | $\frac{x}{1-x-x^2}$ |
| Catalan: $1, 1, 2, 5, 14, \ldots$ | $\frac{1 - \sqrt{1-4x}}{2x}$ |

**Operations on Generating Functions:**

| Operation on sequence | Operation on GF |
|---|---|
| $\{c \cdot a_n\}$ | $c \cdot A(x)$ |
| $\{a_n + b_n\}$ | $A(x) + B(x)$ |
| Convolution $\{a_0 b_n + a_1 b_{n-1} + \cdots\}$ | $A(x) \cdot B(x)$ |
| Left shift $\{a_{n+1}\}$ | $(A(x) - a_0)/x$ |
| Right shift $\{0, a_0, a_1, \ldots\}$ | $x \cdot A(x)$ |
| $\{n \cdot a_n\}$ | $x \cdot A'(x)$ |
| Partial sums $\{\sum_{k=0}^{n} a_k\}$ | $A(x)/(1-x)$ |

**Solving Recurrences Using GFs (⭐⭐ KEY TECHNIQUE):**
1. Multiply recurrence by $x^n$, sum over all valid $n$
2. Express in terms of $A(x)$
3. Solve for $A(x)$, use partial fractions, expand to read off $a_n$

**GF Applications for Counting:**
- Ways to select $r$ items from $n$ types (at most $k$ each): Coefficient of $x^r$ in $\left(\frac{1-x^{k+1}}{1-x}\right)^n$
- Ways to make change for $n$ cents with denominations $d_1, d_2, \ldots$: Coefficient of $x^n$ in $\frac{1}{(1-x^{d_1})(1-x^{d_2})\cdots}$

### 10.10.1 Exponential Generating Functions (EGF) 🧪

> For **labeled** counting (where order/labeling matters), use EGF instead of OGF!

**Exponential Generating Function:** $\hat{A}(x) = \sum_{n=0}^{\infty} a_n \frac{x^n}{n!}$

| Sequence | EGF |
|---|---|
| $1, 1, 1, 1, \ldots$ ($a_n = 1$) | $e^x$ |
| $0, 1, 1, 1, \ldots$ ($a_n = 1$ for $n \geq 1$) | $e^x - 1$ |
| $1, 0, 1, 0, 1, \ldots$ ($a_n = 1$ if $n$ even) | $\cosh(x) = \frac{e^x + e^{-x}}{2}$ |
| $0, 1, 0, 1, 0, \ldots$ ($a_n = 1$ if $n$ odd) | $\sinh(x) = \frac{e^x - e^{-x}}{2}$ |
| Derangements $D_n$ | $\frac{e^{-x}}{1-x}$ |
| Bell numbers $B_n$ | $e^{e^x - 1}$ |
| Permutations $n!$ | $\frac{1}{1-x}$ |

**Key operation:** Product of EGFs corresponds to "labeled merge" — $\hat{C}(x) = \hat{A}(x) \cdot \hat{B}(x)$ gives $c_n = \sum_{k=0}^{n} \binom{n}{k} a_k b_{n-k}$ (binomial convolution).

> 🎯 **When to use OGF vs EGF:**
> - **OGF:** Unlabeled objects (identical coins, indistinguishable partitions)
> - **EGF:** Labeled objects (distinct people, distinguishable items)

### 10.11 Solving Non-Homogeneous Recurrences (⭐ GATE)

$a_n = c_1 a_{n-1} + c_2 a_{n-2} + \cdots + c_k a_{n-k} + f(n)$

**Solution = Homogeneous solution + Particular solution**

**Method of Undetermined Coefficients:**

| $f(n)$ | Try particular solution |
|---|---|
| Constant $c$ | $p_n = A$ |
| $n$ | $p_n = An + B$ |
| $n^2$ | $p_n = An^2 + Bn + C$ |
| $r^n$ | $p_n = Ar^n$ |
| $n \cdot r^n$ | $p_n = (An + B)r^n$ |

> If trial overlaps with homogeneous solution, multiply by $n$ (or $n^2$, etc.) until independent.

**Repeated Roots:** For $r^2 - 4r + 4 = 0 \Rightarrow$ repeated root $r = 2$: $a_n = (A + Bn) \cdot 2^n$. For multiplicity $m$: $a_n = (A_0 + A_1 n + \cdots + A_{m-1} n^{m-1}) r^n$

**Common Recurrences:**

| Recurrence | Solution |
|---|---|
| $T(n) = 2T(n-1) + 1$, $T(0) = 0$ | $T(n) = 2^n - 1$ (Tower of Hanoi) |
| $T(n) = T(n-1) + n$, $T(0) = 0$ | $T(n) = n(n+1)/2$ |
| $a_n = a_{n-1} + a_{n-2}$ | Fibonacci: $\frac{\phi^n - \hat{\phi}^n}{\sqrt{5}}$ |
| $a_n = 2a_{n-1}$, $a_0 = 1$ | $a_n = 2^n$ |

### 10.11.1 Master Theorem for Divide-and-Conquer (⭐ Connects to Algorithms) 🏆

> While primarily an algorithms topic, the Master Theorem solves recurrences that appear in combinatorics too!

For $T(n) = aT(n/b) + O(n^d)$ where $a \geq 1, b > 1, d \geq 0$:

$$T(n) = \begin{cases} O(n^d) & \text{if } d > \log_b a \\ O(n^d \log n) & \text{if } d = \log_b a \\ O(n^{\log_b a}) & \text{if } d < \log_b a \end{cases}$$

**Classic examples:**

| Algorithm | Recurrence | Solution |
|---|---|---|
| Binary Search | $T(n) = T(n/2) + O(1)$ | $O(\log n)$ |
| Merge Sort | $T(n) = 2T(n/2) + O(n)$ | $O(n \log n)$ |
| Strassen's | $T(n) = 7T(n/2) + O(n^2)$ | $O(n^{\log_2 7}) \approx O(n^{2.807})$ |
| Karatsuba | $T(n) = 3T(n/2) + O(n)$ | $O(n^{\log_2 3}) \approx O(n^{1.585})$ |

### 10.11.2 Recurrence Pattern Recognition Guide 🧭

> In the exam hall, half the battle is not solving the recurrence — it is recognizing WHICH template it belongs to within 10 seconds. ⏱️

**Homogeneous linear recurrences**

- Form: $a_n = c_1 a_{n-1} + \cdots + c_k a_{n-k}$
- Use the **characteristic equation**
- Distinct roots $r_1, \ldots, r_t$ give terms like $A_i r_i^n$
- Repeated root $r$ of multiplicity $m$ gives:

$$(A_0 + A_1 n + \cdots + A_{m-1}n^{m-1})r^n$$

**Non-homogeneous forcing-term guide 🎯**

| Forcing term $f(n)$ | First trial for particular solution |
|---|---|
| Constant $c$ | $A$ |
| Polynomial in $n$ of degree $d$ | Degree-$d$ polynomial |
| $r^n$ | $Ar^n$ |
| Polynomial $\times r^n$ | $(\text{polynomial}) \cdot r^n$ |
| $\sin(\theta n)$ or $\cos(\theta n)$ | Trig-form trial |

If the trial overlaps with the homogeneous solution, multiply by $n$, or by a higher power of $n$ until it becomes independent.

**Fast recognition library ⚡**

| Recurrence | Pattern | Final shape |
|---|---|---|
| $a_n = a_{n-1} + c$ | Arithmetic growth | Linear in $n$ |
| $a_n = r a_{n-1}$ | Geometric growth | $Cr^n$ |
| $a_n = a_{n-1} + n$ | Summation of first $n$ integers | Quadratic |
| $a_n = a_{n-1} + a_{n-2}$ | Fibonacci type | Combination of two roots |
| $T(n) = aT(n/b) + f(n)$ | Divide-and-conquer | Master theorem / recursion tree |

**Most common GATE mistakes 🚨**

- Forgetting to use all initial conditions when solving constants
- Using $Ar^n$ even when $r$ is already a characteristic root
- Writing only one term for a repeated root
- Mixing up sequence recurrences with divide-and-conquer recurrences

### 10.11.3 Worked Examples + PYQ Traps (Recurrences) 📉

#### Worked Example 1: Repeated characteristic root

Solve:

$$a_n = 6a_{n-1} - 9a_{n-2}, \quad a_0=2,\; a_1=6$$

Characteristic equation:

$$r^2 - 6r + 9 = 0 \Rightarrow (r-3)^2=0$$

Repeated root $r=3$, so:

$$a_n = (A+Bn)3^n$$

From $a_0=2$, $A=2$. From $a_1=6$:

$$3(A+B)=6 \Rightarrow A+B=2 \Rightarrow B=0$$

Hence:

$$a_n = 2\cdot 3^n$$

#### Worked Example 2: Non-homogeneous with overlap

Solve:

$$a_n = 2a_{n-1}+2^n$$

Homogeneous part gives $a_n^{(h)}=C2^n$.

Trial $A2^n$ overlaps with homogeneous root, so multiply by $n$:

$$a_n^{(p)}=An2^n$$

Substitute:

$$An2^n = 2[A(n-1)2^{n-1}] + 2^n = A(n-1)2^n + 2^n$$

So $A=1$. Therefore:

$$a_n = C2^n + n2^n = (C+n)2^n$$

#### PYQ-Style Trap Box ⚠️

- Repeated root but using only $Ar^n$ ❌ (must use $(A+Bn)r^n$)
- Trial term equals homogeneous term and not corrected ❌ (multiply by $n$)
- Applying Master theorem to $a_n = a_{n-1}+n$ ❌ (not divide-and-conquer form)

### 10.12 Catalan Numbers (⭐ GATE IMPORTANT)

$$C_n = \frac{1}{n+1}\binom{2n}{n} = \frac{(2n)!}{(n+1)! \cdot n!}$$

| $n$ | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|---|
| $C_n$ | 1 | 1 | 2 | 5 | 14 | 42 | 132 | 429 |

**Recurrence:** $C_0 = 1, \quad C_{n+1} = \sum_{i=0}^{n} C_i \cdot C_{n-i} = \frac{2(2n+1)}{n+2} C_n$

**What Catalan Numbers Count (⭐ MUST KNOW — DIRECT GATE QUESTIONS):**

$C_n$ counts ALL of the following:
1. **Valid parenthesizations** of $n$ pairs of parentheses
2. **Full binary trees** with $n+1$ leaves (or $n$ internal nodes)
3. Ways to **triangulate** a convex polygon with $n+2$ sides
4. **Monotonic lattice paths** from $(0,0)$ to $(n,n)$ that don't cross above diagonal
5. **Different BSTs** with $n$ keys
6. Ways to **multiply $n+1$ matrices** (order of parenthesization)
7. **Stack-sortable permutations** of $\{1, 2, \ldots, n\}$
8. **Non-crossing partitions** of $\{1, 2, \ldots, n\}$

### 10.13 Stirling Numbers (⭐ OCCASIONALLY ASKED)

**Stirling Numbers of the Second Kind** $S(n, k)$ = ways to partition $n$ elements into exactly $k$ non-empty subsets.

$$S(n, k) = k \cdot S(n-1, k) + S(n-1, k-1)$$

| | $k=1$ | $k=2$ | $k=3$ | $k=4$ |
|---|---|---|---|---|
| $n=1$ | 1 | | | |
| $n=2$ | 1 | 1 | | |
| $n=3$ | 1 | 3 | 1 | |
| $n=4$ | 1 | 7 | 6 | 1 |

- **Bell numbers:** $B_n = \sum_{k=0}^{n} S(n, k)$
- **Surjections:** onto functions from $n$-set to $k$-set = $k! \cdot S(n, k)$

### 10.13.1 Integer Partitions (⭐ GATE DA) 🔢

> Not to be confused with set partitions! Integer partitions = ways to write $n$ as sum of positive integers (order doesn't matter).

**Partition of integer $n$:** Write $n$ as $n = n_1 + n_2 + \cdots + n_k$ where $n_1 \geq n_2 \geq \cdots \geq n_k > 0$.

| $n$ | Partitions | $p(n)$ |
|---|---|---|
| 1 | 1 | 1 |
| 2 | 2, 1+1 | 2 |
| 3 | 3, 2+1, 1+1+1 | 3 |
| 4 | 4, 3+1, 2+2, 2+1+1, 1+1+1+1 | 5 |
| 5 | 5, 4+1, 3+2, 3+1+1, 2+2+1, 2+1+1+1, 1+1+1+1+1 | 7 |
| 6 | 11 | 11 |
| 7 | 15 | 15 |
| 10 | — | 42 |

**🎯 Key Distinction:**
- **Integer partition** of $n$: write $n$ as sum (ORDER DOESN'T MATTER) — counted by $p(n)$
- **Composition** of $n$: write $n$ as sum (ORDER MATTERS) — see below
- **Set partition** of $\{1,...,n\}$: divide into groups — counted by Bell number $B_n$

**Generating function for $p(n)$:**
$$\sum_{n=0}^{\infty} p(n) x^n = \prod_{k=1}^{\infty} \frac{1}{1-x^k}$$

### 10.13.2 Compositions of Integers 📝

A **composition** of $n$ into $k$ parts is an ordered tuple $(c_1, c_2, \ldots, c_k)$ with $c_i \geq 1$ and $\sum c_i = n$.

| Type | Count | Method |
|---|---|---|
| Compositions of $n$ into $k$ positive parts | $\binom{n-1}{k-1}$ | Stars and bars (balls in row, choose dividers) |
| Compositions of $n$ into $k$ non-negative parts | $\binom{n+k-1}{k-1}$ | Stars and bars |
| **Total compositions of $n$** (any number of parts) | $2^{n-1}$ | Choose which of $n-1$ gaps get dividers |

**Example:** Compositions of 4 into 2 positive parts: $(1,3), (2,2), (3,1)$ → $\binom{3}{1} = 3$ ✅

### 10.13.3 Twelvefold Way — Complete Classification 📊

> 🔥 The ultimate framework for counting problems! Every distribution problem falls into one of these 12 categories.

| Objects (balls) | Containers (boxes) | No restriction | Injective ($\leq 1$ per box) | Surjective ($\geq 1$ per box) |
|---|---|---|---|---|
| **Distinct** → **Distinct** | | $k^n$ | $\frac{k!}{(k-n)!}$ | $k! \cdot S(n,k)$ |
| **Distinct** → **Identical** | | $\sum_{i=1}^{k} S(n,i)$ (≤ $k$ blocks) | $\begin{cases} 1 & n \leq k \\ 0 & n > k \end{cases}$ | $S(n,k)$ (Stirling) |
| **Identical** → **Distinct** | | $\binom{n+k-1}{k-1}$ | $\binom{k}{n}$ | $\binom{n-1}{k-1}$ |
| **Identical** → **Identical** | | $p_k(n)$ (partitions into ≤ $k$ parts) | $\begin{cases} 1 & n \leq k \\ 0 & n > k \end{cases}$ | $p(n,k)$ (partitions into exactly $k$ parts) |

> Here $n$ = number of objects, $k$ = number of containers. "$n^k$" should be "$k^n$" — each of $n$ objects chooses from $k$ boxes.

### 10.13.4 Burnside's Lemma (Counting under Symmetry) (⭐ GATE Awareness) 🔄

> 💡 **When objects that are "the same up to rotation/reflection" should be counted once!**

$$|X/G| = \frac{1}{|G|} \sum_{g \in G} |Fix(g)|$$

Where:
- $X$ = set of all colorings (without symmetry)
- $G$ = symmetry group (rotations, reflections)
- $Fix(g)$ = colorings unchanged by symmetry $g$
- $|X/G|$ = number of truly distinct colorings

**Classic Example — Coloring a square's vertices with 2 colors 🎨:**
Symmetry group = rotations {0°, 90°, 180°, 270°} (order 4)

| Rotation | Fixed colorings |
|---|---|
| 0° (identity) | All $2^4 = 16$ colorings are fixed |
| 90° | All 4 vertices same color: $2$ |
| 180° | Opposite pairs same: $2^2 = 4$ |
| 270° | Same as 90°: $2$ |

Distinct colorings = $\frac{16 + 2 + 4 + 2}{4} = \frac{24}{4} = 6$ 🎯

> 🏭 **Application:** Counting distinct necklaces with $n$ beads and $k$ colors, counting distinct chemical isomers, counting distinct graph colorings up to automorphism.

### 10.13.5 Fibonacci Numbers — Advanced Properties (⭐ GATE) 🌻

> Fibonacci appears in MANY GATE questions across different topics!

**Fibonacci sequence:** $F_0 = 0, F_1 = 1, F_n = F_{n-1} + F_{n-2}$

| $n$ | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| $F_n$ | 0 | 1 | 1 | 2 | 3 | 5 | 8 | 13 | 21 | 34 | 55 |

**Closed form (Binet's formula):**
$$F_n = \frac{\phi^n - \hat{\phi}^n}{\sqrt{5}} \quad \text{where } \phi = \frac{1+\sqrt{5}}{2} \approx 1.618, \quad \hat{\phi} = \frac{1-\sqrt{5}}{2} \approx -0.618$$

**Key identities:**
| Identity | Formula |
|---|---|
| **Cassini's** | $F_{n-1} F_{n+1} - F_n^2 = (-1)^n$ |
| **Sum** | $\sum_{i=0}^{n} F_i = F_{n+2} - 1$ |
| **GCD** | $\gcd(F_m, F_n) = F_{\gcd(m,n)}$ |
| **Divisibility** | $F_m \mid F_n \iff m \mid n$ (for $m \geq 1$) |
| **Zeckendorf** | Every positive integer has a unique representation as sum of non-consecutive Fibonacci numbers |

**What Fibonacci counts:**
- Binary strings of length $n$ with no two consecutive 1s: $F_{n+2}$
- Tilings of a $2 \times n$ board with $1 \times 2$ dominoes: $F_{n+1}$
- Subsets of $\{1,...,n\}$ with no two consecutive elements: $F_{n+2}$
- Compositions of $n$ using only 1s and 2s: $F_n$

### 10.14 Advanced Pigeonhole Principle (⭐ GATE PYQ PATTERNS)

**Erdős–Szekeres Theorem:** Any sequence of more than $mn$ distinct numbers has an increasing subsequence of length $> m$ or a decreasing subsequence of length $> n$.

**GATE consequence:** In a sequence of $n^2 + 1$ distinct numbers, there's a monotone subsequence of length $\geq n + 1$.

**Classic GATE Pigeonhole Problems:**
1. **Ramsey:** Among any 6 people, either 3 mutually know each other or 3 mutually don't. ($R(3,3) = 6$)
2. **Sum pairs:** In any $n+1$ numbers from $\{1, \ldots, 2n\}$, some pair sums to $2n+1$.
3. **Divisibility:** Among any 52 integers, two have difference divisible by 51.

### 10.14.1 Ramsey Theory — Extended (⭐ GATE Awareness) 🌈

> **Ramsey's Theorem:** Complete disorder is impossible — in any sufficiently large structure, some order must exist!

**Ramsey Number $R(s,t)$:** Minimum $n$ such that any 2-coloring of edges of $K_n$ contains a red $K_s$ or a blue $K_t$.

| $R(s,t)$ | Known values |
|---|---|
| $R(3,3)$ | **6** (most famous — "party problem") |
| $R(3,4)$ | 9 |
| $R(3,5)$ | 14 |
| $R(4,4)$ | 18 |
| $R(3,3,3)$ (3-color) | 17 |

**Upper bound:** $R(s,t) \leq \binom{s+t-2}{s-1}$ (Erdős-Szekeres)
**Lower bound:** $R(s,t) \geq 2^{s/2}$ for $s = t$ (probabilistic method)

> 🎯 **For GATE:** You mainly need $R(3,3) = 6$ and the concept. The party problem with 6 people is a classic MCQ!

### 10.15 Multinomial Theorem

$$(x_1 + x_2 + \cdots + x_k)^n = \sum_{\substack{n_1 + n_2 + \cdots + n_k = n \\ n_i \geq 0}} \binom{n}{n_1, n_2, \ldots, n_k} x_1^{n_1} x_2^{n_2} \cdots x_k^{n_k}$$

where $\binom{n}{n_1, n_2, \ldots, n_k} = \frac{n!}{n_1! n_2! \cdots n_k!}$ is the **multinomial coefficient**.

**Application:** Number of ways to distribute $n$ distinct objects into $k$ groups of sizes $n_1, \ldots, n_k = \frac{n!}{n_1! \cdots n_k!}$.

---

# 🌐 PART 5: GRAPH THEORY

> 🎭 **"If Computer Science had a favorite mathematical tool, it would be Graph Theory!"**

> 🌍 **Graph Theory is EVERYWHERE:**
> | 🌐 Application | 📈 Who Uses It |
> |---|---|
> | Social networks (who follows whom) | Facebook, Instagram, LinkedIn, Twitter/X |
> | Maps & navigation | Google Maps, Waze, Uber, Ola |
> | Internet routing | Every ISP, Cloudflare, Akamai |
> | Recommendation systems | Netflix, Amazon, YouTube |
> | Molecular structures | Drug discovery, chemistry tools |
> | Flight/train networks | Airlines, IRCTC |
> | Circuit design | Intel, AMD, TSMC |
> | Compiler register allocation | GCC, LLVM, Clang |
> | Game maps & AI pathfinding | Unity, Unreal Engine, every video game 🎮 |
> | Pandemic spread modeling | WHO, epidemiologists 🦠 |

---

## Chapter 11: Graphs — Fundamentals (⭐⭐⭐ VERY HIGH WEIGHTAGE) 📊

> 💡 **Beginner Analogy — A Road Map 🗺️:**
> A graph is just a map!
> - **Vertices (nodes)** = Cities 🏙️
> - **Edges** = Roads connecting cities 🛣️
> - **Directed graph** = One-way streets ➡️
> - **Undirected graph** = Two-way streets ⇆
> - **Weighted graph** = Roads with distances/tolls 💰
>
> That's it! Everything else in graph theory is just asking smart questions about this "map"!

### 11.1 Definitions

A **graph** $G = (V, E)$ consists of:
- $V$ = set of vertices (nodes)
- $E$ = set of edges (pairs of vertices)

| Type | Edges |
|---|---|
| **Undirected graph** | $E \subseteq \{\{u,v\} \mid u, v \in V\}$ |
| **Directed graph (digraph)** | $E \subseteq \{(u,v) \mid u, v \in V\}$ (ordered pairs) |
| **Simple graph** | No self-loops, no parallel edges |
| **Multigraph** | Allows parallel edges |

### 11.1.1 Graph Representations & Matrix View 🧮

The SAME graph can be represented in multiple ways, and GATE often asks properties using one representation while expecting reasoning in another.

| Representation | What it stores | Best for |
|---|---|---|
| **Adjacency list** | Neighbors of each vertex | Sparse graphs, traversal |
| **Adjacency matrix $A$** | $A[i][j]=1$ iff edge exists | Dense graphs, algebraic reasoning |
| **Incidence matrix $B$** | Vertex-edge incidence | Degree/cut structure |
| **Laplacian $L=D-A$** | Degree minus adjacency | Spanning trees, cuts, spectra |

**Adjacency Matrix Facts (must know) ⭐**

- For an undirected simple graph, $A$ is symmetric
- In a digraph, row sum = out-degree, column sum = in-degree
- Diagonal entry $A[i][i]$ indicates a self-loop
- For simple graphs, $A[i][i] = 0$

**Power of adjacency matrix:**

$$\left(A^k\right)_{ij} = \text{number of walks of length } k \text{ from } i \text{ to } j$$

This is extremely important:
- $(A^2)_{ij}$ counts common-neighbor-based 2-step walks
- Diagonal entries of $A^k$ count closed walks of length $k$

**Incidence Matrix Insight 📌**

For an undirected graph with vertices as rows and edges as columns:
- Each non-loop edge column has exactly two 1s
- Row sum gives degree of the corresponding vertex

**Laplacian Matrix**

$$L = D - A$$

where $D$ is the diagonal degree matrix.

- Used in **Kirchhoff's Matrix Tree Theorem**
- Encodes connectivity and cut structure
- For connected graphs, rank of $L$ is $n-1$

> 🎯 Quick exam shortcut: adjacency powers count walks, Boolean powers track reachability, and Laplacians count spanning trees.

### 11.2 Degree of a Vertex

- **Undirected:** $\deg(v)$ = number of edges incident to $v$. Self-loop contributes 2.
- **Directed:** in-degree $\deg^-(v)$ and out-degree $\deg^+(v)$

**Handshaking Theorem (⭐ MUST KNOW):**
$$\sum_{v \in V} \deg(v) = 2|E|$$

**Corollary:** Number of vertices with odd degree is **always even**.

### 11.3 Types of Graphs

| Graph | Definition | Vertices | Edges |
|---|---|---|---|
| **Complete graph $K_n$** | Every pair connected | $n$ | $\binom{n}{2} = n(n-1)/2$ |
| **Complete bipartite $K_{m,n}$** | Two parts, all cross-edges | $m + n$ | $mn$ |
| **Cycle $C_n$** | Single cycle of length $n$ | $n$ | $n$ |
| **Path $P_n$** | Single path | $n$ | $n-1$ |
| **Tree** | Connected + Acyclic | $n$ | $n-1$ |
| **Wheel $W_n$** | $C_n$ + one hub vertex | $n+1$ | $2n$ |
| **Hypercube $Q_n$** | $2^n$ vertices, bit-string adjacency | $2^n$ | $n \cdot 2^{n-1}$ |
| **Petersen graph** | Famous 3-regular graph | 10 | 15 |

### 11.4 Graph Isomorphism

Two graphs are **isomorphic** if there exists a bijection between their vertex sets that preserves adjacency.

**Necessary conditions (not sufficient):**
- Same number of vertices
- Same number of edges
- Same degree sequence
- Same number of cycles of each length
- Same number of connected components
- Same chromatic number
- Same chromatic polynomial

> ⚠️ **GATE Trap:** Satisfying ALL necessary conditions does NOT guarantee isomorphism! To PROVE isomorphism, you must exhibit the explicit bijection. To DISPROVE, find any property that differs.

**Graph Invariants (preserved by isomorphism):**
$|V|$, $|E|$, degree sequence, $\chi(G)$, $\omega(G)$, $\alpha(G)$, number of spanning trees, girth, diameter, radius, connectivity, planarity

### 11.4.1 Graph Measurements (⭐ GATE ASKED) 📏

> These concepts appear directly in GATE and are used extensively in network analysis!

| Measure | Definition |
|---|---|
| **Distance** $d(u,v)$ | Length of shortest path from $u$ to $v$ ($\infty$ if no path) |
| **Eccentricity** $e(v)$ | $\max_{u \in V} d(v, u)$ — farthest any vertex is from $v$ |
| **Diameter** $\text{diam}(G)$ | $\max_{v \in V} e(v) = \max_{u,v} d(u,v)$ — maximum distance between any pair |
| **Radius** $\text{rad}(G)$ | $\min_{v \in V} e(v)$ — minimum eccentricity |
| **Center** | Set of vertices $v$ with $e(v) = \text{rad}(G)$ |
| **Periphery** | Set of vertices $v$ with $e(v) = \text{diam}(G)$ |
| **Girth** | Length of the shortest cycle ($\infty$ if acyclic) |

**Key relationships:**
- $\text{rad}(G) \leq \text{diam}(G) \leq 2 \cdot \text{rad}(G)$
- Every tree has center consisting of 1 or 2 adjacent vertices 🌳
- $K_n$: diameter = 1, radius = 1 (for $n \geq 2$)
- $C_n$: diameter = $\lfloor n/2 \rfloor$, radius = $\lfloor n/2 \rfloor$
- $P_n$: diameter = $n-1$, center = middle vertex/vertices
- Girth of $K_n$ ($n \geq 3$) = 3, Girth of $K_{m,n}$ ($m,n \geq 2$) = 4, Girth of Petersen = 5

> 🏭 **Production:** Network diameter determines worst-case latency. Facebook proved their social graph has diameter ~4 ("4 degrees of separation")! Data center networks (fat trees, Clos networks) are designed to minimize diameter.

### 11.5 Connectivity

- **Walk:** Sequence of vertices with edges between consecutive vertices (vertices/edges can repeat)
- **Trail:** Walk with **no repeated edges** (vertices can repeat)
- **Path:** Walk with **no repeated vertices** (and hence no repeated edges)
- **Circuit:** Closed trail (starts and ends at same vertex, no repeated edges)
- **Cycle:** Closed path (starts and ends at same vertex, no repeated vertices except start=end)
- **Connected:** There's a path between every pair of vertices
- **Connected components:** Maximal connected subgraphs

> ⚠️ **GATE Trap — Know the hierarchy:**
> Path ⊂ Trail ⊂ Walk, and Cycle ⊂ Circuit ⊂ Closed Walk
> - Every path is a trail, but not vice versa!
> - Every trail is a walk, but not vice versa!

| Term | Repeated Vertices? | Repeated Edges? | Closed? |
|---|---|---|---|
| **Walk** | Yes | Yes | Maybe |
| **Trail** | Yes | ❌ No | Maybe |
| **Path** | ❌ No | ❌ No | No |
| **Circuit** | Yes | ❌ No | Yes |
| **Cycle** | ❌ No (except start=end) | ❌ No | Yes |

**For trees:** Between any two vertices, there is **exactly one path**.

### 11.6 Euler Graphs (⭐⭐ VERY FREQUENTLY ASKED) 🚶

> 💡 **Beginner Analogy — The Königsberg Bridge Problem 🌉:**
> In 1736, mathematician Euler asked: "Can you walk across all 7 bridges of Königsberg exactly once?" He proved it was IMPOSSIBLE — and invented Graph Theory in the process! This is considered the **birth of Graph Theory**! 🎂
>
> 🌍 **Real-World Uses:**
> - **Garbage truck routes** 🚛: Find a route that covers every street exactly once (Chinese Postman Problem = Euler path variant!)
> - **DNA fragment assembly** 🧬: In genome sequencing, finding Euler paths through De Bruijn graphs helps reconstruct DNA!
> - **PCB circuit tracing** 🔌: Drawing circuit traces without crossings = finding Euler paths!
> - **Snow plow routing** ❄️: City planners use Euler circuits to optimize snow removal routes!

| Euler Path | Euler Circuit |
|---|---|
| Visits every **edge** exactly once | Euler path that starts and ends at same vertex |
| Exists iff exactly **0 or 2 vertices** have odd degree | Exists iff **all vertices** have even degree |
| (If 2 odd vertices: path between them) | AND graph is connected |

### 11.7 Hamiltonian Graphs (⭐ ASKED OFTEN)

| Hamiltonian Path | Hamiltonian Circuit |
|---|---|
| Visits every **vertex** exactly once | Hamiltonian path that starts and ends at same vertex |
| No easy necessary and sufficient condition | **NP-complete** problem |

**Sufficient conditions (Dirac's theorem):** For simple graph with $n \geq 3$: if $\deg(v) \geq n/2$ for all $v$, then graph has Hamiltonian circuit.

**Ore's Theorem (⭐ STRONGER than Dirac's):** For simple graph with $n \geq 3$: if $\deg(u) + \deg(v) \geq n$ for every pair of **non-adjacent** vertices $u, v$, then graph has Hamiltonian circuit.

> 🎯 Dirac's is a special case of Ore's (if every vertex has degree $\geq n/2$, then any pair has sum $\geq n$).

**Necessary condition:** If removing $k$ vertices disconnects graph into $> k$ components → no Hamiltonian circuit.

**Quick checklist for Hamiltonian graphs:**

| Graph | Hamiltonian Circuit? | Hamiltonian Path? |
|---|---|---|
| $K_n$ ($n \geq 3$) | ✅ YES | ✅ YES |
| $K_{m,n}$ ($m = n \geq 2$) | ✅ YES | ✅ YES |
| $K_{m,n}$ ($m \neq n$) | ❌ NO | ✅ only if $|m-n| \leq 1$ |
| $C_n$ | ✅ YES | ✅ YES |
| $P_n$ | ❌ NO | ✅ YES |
| Petersen graph | ❌ NO | ✅ YES |
| Hypercube $Q_n$ | ✅ YES ($n \geq 2$) | ✅ YES |

> **Comparison: Euler vs Hamiltonian** ⚡
> | | Euler | Hamiltonian |
> |---|---|---|
> | Visits | Every EDGE exactly once | Every VERTEX exactly once |
> | Easy to check? | YES (degree condition) | NO (NP-complete!) |
> | Necessary & sufficient? | YES | Only sufficient conditions known |

### 11.8 Trees (⭐⭐ CRITICAL)

**Properties of a tree with $n$ vertices:**
- Connected and has $n-1$ edges
- Connected and removing any edge disconnects it
- Acyclic and adding any edge creates exactly one cycle
- Between any two vertices, there is exactly one path
- Every tree with $n \geq 2$ has at least **2 leaves** (vertices of degree 1)
- A tree is **bipartite** (in fact, every tree is 2-colorable)
- A tree on $n$ vertices has exactly $n-1$ edges (necessary AND sufficient with connected or acyclic)

**Equivalent definitions of a tree (any one implies all others):**
A graph $G$ with $n$ vertices is a tree iff:
1. Connected + $n-1$ edges
2. Acyclic + $n-1$ edges
3. Connected + acyclic
4. Connected + removing any edge disconnects
5. Acyclic + adding any edge creates exactly one cycle
6. Unique path between every pair of vertices

**Types of Trees (⭐ GATE):**

| Type | Definition |
|---|---|
| **Rooted tree** | Tree with a designated root vertex |
| **Binary tree** | Each node has at most 2 children |
| **Full binary tree** | Each node has exactly 0 or 2 children |
| **Complete binary tree** | All levels full except possibly last (filled left to right) |
| **Perfect binary tree** | All internal nodes have 2 children, all leaves at same level |
| **m-ary tree** | Each node has at most $m$ children |
| **Ordered tree** | Children of each node have a fixed order |

**Binary tree formulas ($n$ = internal nodes, $L$ = leaves, $h$ = height):**
- Full binary tree: $L = n + 1$, total nodes = $2n + 1$
- Height $h$: max leaves = $2^h$, max total nodes = $2^{h+1} - 1$
- $n$ nodes: $h \geq \lfloor \log_2 n \rfloor$
- Number of distinct binary trees with $n$ nodes = $C_n$ (Catalan number!)
- Number of distinct labeled trees on $n$ vertices = $n^{n-2}$ (Cayley's formula)

**Counting labeled trees:** Number of labeled trees on $n$ vertices = $n^{n-2}$ (**Cayley's formula**)

**Spanning tree:** A subgraph that is a tree containing all vertices.
- Number of spanning trees of $K_n = n^{n-2}$

### 11.9 Graph Coloring (⭐ FREQUENTLY ASKED) 🎨

> 💡 **Beginner Analogy — Coloring a Map 🌍:**
> Remember coloring maps in school where no two neighboring countries share a color? That's EXACTLY graph coloring! Countries = nodes, shared borders = edges. The **Four Color Theorem** proved that ANY map can be colored with just 4 colors! 🎨🎨🎨🎨
>
> 🏭 **Production Use Cases:**
> - **Compiler Register Allocation** ⚙️: Variables that are "alive" at the same time can't share a CPU register → model as graph coloring! GCC and LLVM use this technique. If your program needs more colors (registers) than available, the compiler "spills" to memory (slower!).
> - **Exam Scheduling** 📅: Courses with common students can't have exams at the same time → graph coloring!
> - **Frequency Assignment** 📶: Cell towers close together can't use the same frequency (interference!) → graph coloring with frequencies as colors!
> - **Sudoku** 🧩: A 9×9 Sudoku is actually a graph coloring problem with 9 colors!
> - **Map coloring in GIS** 🗺️: Google Maps, Apple Maps use graph coloring for rendering!

**Vertex coloring:** Assign colors to vertices such that no two adjacent vertices have the same color.

**Chromatic number $\chi(G)$:** Minimum number of colors needed.

| Graph | $\chi(G)$ |
|---|---|
| $K_n$ | $n$ |
| Bipartite (non-empty) | 2 |
| Odd cycle $C_{2k+1}$ | 3 |
| Even cycle $C_{2k}$ | 2 |
| Tree | 2 (if $n \geq 2$) |
| Wheel $W_n$ ($n$ odd) | 3 |
| Wheel $W_n$ ($n$ even) | 4 |
| Petersen graph | 3 |
| Planar graph | $\leq 4$ (Four Color Theorem) |

**Bounds:**
- $\chi(G) \geq \omega(G)$ (clique number)
- $\chi(G) \leq \Delta(G) + 1$ (max degree + 1, by greedy)
- **Brooks' theorem:** $\chi(G) \leq \Delta(G)$ unless $G$ is complete or odd cycle

### 11.10 Planar Graphs (⭐ GATE IMPORTANT) 📄

> 💡 **Beginner Analogy — Drawing Without Crossing Lines ✍️:**
> Can you connect 3 houses to 3 utilities (water, gas, electricity) without any pipes crossing? This is the famous **Utilities Problem** ($K_{3,3}$) — and the answer is NO! It's non-planar! 🚧
>
> 🏭 **Production Relevance:**
> - **PCB Design** 🔌: Circuit boards ideally want no wire crossings (planar graphs). When it's impossible, you need multiple layers = more expensive! This is why $K_{3,3}$ non-planarity is taught to electrical engineers.
> - **VLSI Chip Design** 🩸: Modern chips have 10+ metal layers precisely because circuits aren't planar!

A graph is **planar** if it can be drawn on a plane without edge crossings.

**Euler's Formula for Connected Planar Graphs:**
$$V - E + F = 2$$
where $F$ = number of faces (regions), including the outer (unbounded) face.

**Corollaries (for simple connected planar graph with $V \geq 3$):**
- $E \leq 3V - 6$
- If no triangles: $E \leq 2V - 4$
- Minimum degree $\leq 5$

**Kuratowski's Theorem:** A graph is planar iff it contains no subdivision of $K_5$ or $K_{3,3}$.

**$K_5$ is non-planar:** $V=5, E=10$. Check: $10 > 3(5) - 6 = 9$. ✓

**$K_{3,3}$ is non-planar:** $V=6, E=9$. No triangles, so check: $9 > 2(6) - 4 = 8$. ✓

### 11.10.1 Worked Examples + PYQ Traps (Planarity & Matching) 🧩

#### Worked Example 1: Fast planar elimination

Question-style setup: Simple connected planar graph has $V=8$ and $E=19$. Is it possible?

For simple planar with $V \geq 3$:

$$E \leq 3V-6 = 18$$

But $19>18$, impossible. So graph is non-planar ❌

#### Worked Example 2: Hall's theorem check

Let bipartite sets $A=\{a_1,a_2,a_3\}$ and $B=\{b_1,b_2,b_3\}$ with neighborhoods:

- $N(a_1)=\{b_1,b_2\}$
- $N(a_2)=\{b_2\}$
- $N(a_3)=\{b_1,b_3\}$

Check $S=\{a_1,a_2\}$:

$$N(S)=\{b_1,b_2\},\; |N(S)|=2=|S|$$

Check $S=\{a_2,a_3\}$:

$$N(S)=\{b_1,b_2,b_3\},\; |N(S)|=3\ge 2$$

All subsets satisfy Hall, so complete matching from $A$ exists ✅

#### PYQ-Style Trap Box ⚠️

- Using $E \leq 3V-6$ for multigraphs with loops ❌ (bound is for simple planar graphs)
- "Maximal matching = Maximum matching" ❌
- Hall's theorem for non-bipartite graphs ❌
- "If all degrees are even then Hamiltonian circuit exists" ❌ (that condition is for Euler circuit)

#### 30-Second Graph Decision Grid 🚀

- Euler circuit: connected + all degrees even
- Euler path only: connected + exactly 2 odd-degree vertices
- Bipartite: no odd cycle
- Tree: connected + $E=V-1$
- Impossible planar candidate (simple): reject if $E>3V-6$

### 11.11 Matching

A **matching** is a set of edges with no common vertex.

- **Maximum matching:** Matching with maximum number of edges
- **Perfect matching:** Every vertex is matched (requires even $|V|$)
- **Maximal matching:** No more edges can be added (weaker than maximum!)

> ⚠️ **GATE Trap:** Maximal ≠ Maximum! A maximal matching just can't grow; a maximum matching has the most edges possible.

**Key results about matching:**

| Result | Statement |
|---|---|
| **Hall's Marriage Theorem** | Bipartite $G = (A \cup B, E)$: complete matching from $A$ to $B$ exists iff $\forall S \subseteq A: |N(S)| \geq |S|$ |
| **König's Theorem** | In bipartite: max matching = min vertex cover |
| **Gallai's Theorem** | $\alpha(G) + \beta(G) = |V|$ (independence # + vertex cover # = $n$) |
| **Edge cover + matching** | In graph with no isolated vertices: max matching + min edge cover = $|V|$ |
| **Berge's Theorem** | Matching $M$ is maximum iff no augmenting path exists |
| **Perfect matching in $K_{2n}$** | Number of perfect matchings = $(2n-1)!! = (2n-1)(2n-3) \cdots 3 \cdot 1$ |
| **Perfect matching in $K_{n,n}$** | Number of perfect matchings = $n!$ |

**Augmenting path:** A path that alternates between non-matching and matching edges, starting and ending at unmatched vertices. Finding one allows increasing matching size by 1.

**Hall's Marriage Theorem (for bipartite graphs):** A complete matching from $A$ to $B$ exists iff for every subset $S \subseteq A$: $|N(S)| \geq |S|$ (neighborhood condition).

> 💡 **Intuition:** If every group of $k$ women collectively know at least $k$ men, then every woman can be matched to a distinct man she knows! 💒

### 11.12 Chromatic Polynomial (⭐⭐ ASKED IN GATE)

The **chromatic polynomial** $P(G, k)$ gives the number of proper $k$-colorings of graph $G$. $\chi(G) = $ minimum $k$ such that $P(G, k) > 0$.

| Graph | $P(G, k)$ |
|---|---|
| $K_n$ | $k(k-1)(k-2)\cdots(k-n+1) = k^{\underline{n}}$ |
| $\overline{K_n}$ (no edges) | $k^n$ |
| Tree on $n$ vertices | $k(k-1)^{n-1}$ |
| Cycle $C_n$ | $(k-1)^n + (-1)^n (k-1)$ |
| $K_{1,n}$ (star) | $k(k-1)^n$ |

**Deletion-Contraction Method (⭐ KEY TECHNIQUE):** For any edge $e = (u,v)$:
$$P(G, k) = P(G - e, k) - P(G / e, k)$$

**Properties:** $P(G, k)$ is a polynomial of degree $|V|$ with leading coefficient 1, coefficient of $k^{|V|-1} = -|E|$, and $P(G, 0) = 0$ always.

### 11.12.1 Chromatic Number — Complete Bounds Summary (⭐ GATE Quick Reference) 🎨

| Bound | Formula | Type |
|---|---|---|
| **Clique bound** | $\chi(G) \geq \omega(G)$ | Lower |
| **Fractional chromatic** | $\chi(G) \geq |V| / \alpha(G)$ | Lower |
| **Greedy upper** | $\chi(G) \leq \Delta(G) + 1$ | Upper |
| **Brooks' theorem** | $\chi(G) \leq \Delta(G)$ (unless $K_n$ or odd cycle) | Upper |
| **Four Color Theorem** | $\chi(G) \leq 4$ for planar $G$ | Upper (planar) |
| **Five Color Theorem** | $\chi(G) \leq 5$ for planar $G$ (easier proof!) | Upper (planar) |
| **Bipartite** | $\chi(G) = 2$ iff bipartite (and non-empty) | Exact |
| **Perfect graph** | $\chi(G) = \omega(G)$ for perfect graphs | Exact |

> 🎯 **GATE Strategy:** For finding $\chi(G)$: (1) Check bipartite → 2, (2) Find largest clique $\omega$ → lower bound, (3) Try to color with $\omega$ colors → if possible, $\chi = \omega$!

### 11.13 Cut Vertices, Cut Edges & Biconnectivity (⭐ GATE PYQs)

**Cut Vertex (Articulation Point):** Removing vertex $v$ (and its edges) disconnects $G$.
- Leaf (degree 1) is NEVER a cut vertex
- DFS root is cut vertex iff it has $\geq 2$ children in DFS tree

**Cut Edge (Bridge):** Removing edge $e$ disconnects $G$.
- An edge is a bridge iff it does NOT lie on any cycle
- In a tree, EVERY edge is a bridge

**Biconnected Graph:** Connected + no cut vertex. Equivalently: between any two vertices, $\geq 2$ vertex-disjoint paths exist.

### 11.14 Edge & Vertex Connectivity (⭐ GATE)

- **Vertex connectivity $\kappa(G)$:** Min vertices to remove to disconnect $G$
- **Edge connectivity $\lambda(G)$:** Min edges to remove to disconnect $G$

**Whitney's inequality:** $\kappa(G) \leq \lambda(G) \leq \delta(G)$ (minimum degree)

$\kappa(K_n) = \lambda(K_n) = n - 1$. A graph is **$k$-connected** iff $\kappa(G) \geq k$.

### 11.14.1 Max-Flow Min-Cut Intuition 🌊

Even though full flow algorithms are studied in Algorithms, the underlying objects are purely discrete-math structures.

**$s$-$t$ cut:** A partition $(S, T)$ of vertices such that $s \in S$ and $t \in T$.

**Capacity of cut:** Sum of capacities of directed edges from $S$ to $T$.

**Max-Flow Min-Cut Theorem:**

$$\max(\text{value of feasible } s\text{-}t \text{ flow}) = \min(\text{capacity of an } s\text{-}t \text{ cut})$$

**Why this matters in GATE:**

- Matching in bipartite graphs can be converted into a flow problem
- Min-cut gives a certificate that no larger flow is possible
- Edge cuts, vertex cuts, Menger's theorem, and matching all live in the same conceptual family

**Tiny example 💧**

If every path from $s$ to $t$ must cross one of edges $e_1, e_2$ with capacities 2 and 3, then any flow is at most $2+3=5$. If you can actually send flow 5, then by the theorem the max flow is exactly 5 and that cut is minimum.

> 💡 Mental model: A cut is a bottleneck. Max flow can never exceed the narrowest bottleneck.

### 11.15 Directed Graphs — Advanced (⭐ GATE)

**Strongly Connected Components (SCC):** Maximal set of vertices with a path from every vertex to every other. Found by Kosaraju/Tarjan in $O(V+E)$.

**Condensation Graph:** Contract each SCC to a single vertex → always a DAG! 🎯
- Number of SCCs = number of vertices in condensation graph
- Useful for reachability analysis in digraphs

**DAGs (Directed Acyclic Graphs):**
- No directed cycles
- Has at least one source (in-degree 0) and one sink (out-degree 0)
- **Topological ordering** exists iff directed graph is a DAG
- Number of edges in a DAG on $n$ vertices: max = $\binom{n}{2}$ (transitive tournament)
- **Longest path in a DAG** can be found in $O(V+E)$ using topological sort + DP

> 🏭 **DAG Applications:**
> - **Git commit graph** 🐙 — each commit points to parent(s)
> - **Build systems** (Make, Gradle) — dependency ordering
> - **Spreadsheet evaluation** — cell dependencies form a DAG
> - **Course prerequisites** — must take CS101 before CS201!
> - **Bayesian Networks** in ML — probabilistic DAGs for inference

**Tournament:** Directed graph from assigning direction to every edge of $K_n$.
- Every tournament has a **Hamiltonian path** (proved by induction!)
- **Transitive tournament** = no directed 3-cycles = unique Hamiltonian path
- A tournament is transitive iff it's a DAG (equivalent for tournaments)
- **King vertex:** A vertex from which every other vertex is reachable within 2 steps
- Every tournament has at least one king vertex

**In-degree and Out-degree in Digraphs:**
$$\sum_{v \in V} \deg^+(v) = \sum_{v \in V} \deg^-(v) = |E|$$

**Counting directed graphs on $n$ labeled vertices:**
- With self-loops: $2^{n^2}$ (each ordered pair $(u,v)$ including $(u,u)$ is either an edge or not)
- Without self-loops: $2^{n(n-1)}$ (each ordered pair $(u,v)$ with $u \neq v$ is either an edge or not)
- Oriented graphs (no self-loops, at most one direction per pair): $3^{\binom{n}{2}}$ (each unordered pair: $u \to v$, or $v \to u$, or no edge)

**Tournaments on $n$ vertices:** $2^{\binom{n}{2}}$ (each pair has exactly one direction)

### 11.16 Complement & Self-Complementary Graphs (⭐ GATE PYQs)

**Complement** $\overline{G}$: same vertices, edge $(u,v)$ in $\overline{G}$ iff NOT in $G$.
- $|E(G)| + |E(\overline{G})| = \binom{n}{2}$
- $G$ connected OR $\overline{G}$ connected (at least one)
- If $G$ disconnected, then $\overline{G}$ has diameter $\leq 2$

**Self-Complementary:** $G \cong \overline{G}$. Requires $n \equiv 0$ or $1 \pmod{4}$.
- $n = 4$: path $P_4$ ✅; $n = 5$: cycle $C_5$ ✅

### 11.17 Degree Sequence (⭐⭐ FREQUENTLY ASKED IN GATE)

**Degree sequence:** list of vertex degrees in non-increasing order.

**Hakimi's Algorithm (Easiest for GATE):**
1. All degrees 0 → YES. Any degree $< 0$ or $d_1 \geq n$ → NO.
2. Remove $d_1$. Subtract 1 from next $d_1$ largest remaining.
3. Sort. Repeat.

**Example:** Is $(3, 3, 3, 3, 2)$ graphical?
$(3,3,3,2) \to (2,2,2,2) \to (1,1,2) \to (2,1,1) \to (0,0) \to$ **YES** ✅

**Erdős–Gallai Theorem:** $d_1 \geq \cdots \geq d_n$ is graphical iff $\sum d_i$ is even and for each $k$: $\sum_{i=1}^{k} d_i \leq k(k-1) + \sum_{i=k+1}^{n} \min(d_i, k)$.

### 11.18 Edge Coloring & Vizing's Theorem (⭐ GATE AWARENESS)

**Edge coloring:** No two edges sharing a vertex have the same color. **Chromatic index $\chi'(G)$:** minimum edge colors.

**Vizing's Theorem:** $\Delta(G) \leq \chi'(G) \leq \Delta(G) + 1$
- **Class 1** (bipartite): $\chi'(G) = \Delta(G)$
- **Class 2** (odd cycles): $\chi'(G) = \Delta(G) + 1$

### 11.19 Independent Set, Vertex Cover & Clique (⭐ GATE)

- **Independent set:** No edges between them
- **Vertex cover:** Every edge has $\geq 1$ endpoint in the set
- **Clique:** Every pair connected (complete subgraph)

**Key Relationships:**
- $\alpha(G) + \beta(G) = n$ (independence # + vertex cover # = total vertices, Gallai)
- $\omega(G) = \alpha(\overline{G})$ (clique in $G$ = independent set in complement)
- $\chi(G) \geq \omega(G)$
- **König's (bipartite only):** max matching = min vertex cover

### 11.20 Bipartite Graph Properties (⭐⭐ GATE CRITICAL)

**Characterization:** Bipartite iff **no odd-length cycles** iff $\chi(G) \leq 2$.

> 💡 **Testing bipartiteness:** Run BFS/DFS and try 2-coloring — $O(V+E)$!

**More properties of bipartite graphs:**
- Every tree is bipartite ✅
- Every even cycle is bipartite ✅
- $K_{m,n}$ is bipartite by definition ✅
- Maximum edges in a bipartite graph on $n$ vertices = $\lfloor n^2/4 \rfloor$ (Turán's theorem for $r=2$, achieved by $K_{\lfloor n/2 \rfloor, \lceil n/2 \rceil}$)
- Bipartite graph with parts $A, B$: edges $\leq |A| \cdot |B|$
- Every bipartite graph is **2-colorable** and has **chromatic number ≤ 2** (= 1 if no edges, 2 otherwise)
- Edge chromatic number of bipartite graph = $\Delta(G)$ (always Class 1 by König's theorem!)

**König's Theorems (bipartite only):**
1. Max matching = min vertex cover
2. Max independent set = $n$ – max matching
3. Min edge cover = $n$ – max matching (no isolated vertices)

### 11.20.1 Turán's Theorem (⭐ GATE Awareness) 📐

> **What is the maximum number of edges in a graph on $n$ vertices with no $(r+1)$-clique?**

$$\text{ex}(n, K_{r+1}) = \left(1 - \frac{1}{r}\right)\frac{n^2}{2}$$

The extremal graph is the **complete $r$-partite graph** $T(n, r)$ (Turán graph) with parts as equal as possible.

**Special cases:**
- No $K_3$ (triangle-free): max edges = $\lfloor n^2/4 \rfloor$ — achieved by $K_{\lfloor n/2 \rfloor, \lceil n/2 \rceil}$
- No $K_4$: max edges = $(1 - 1/3) \cdot n^2/2 = n^2/3$

> 🎯 **GATE Application:** "What is the maximum number of edges in a simple graph on 10 vertices with no triangle?" Answer: $\lfloor 100/4 \rfloor = 25$ ✅

### 11.21 Regular Graphs, Edge Bounds & Euler's Formula Variants

**Regular Graphs:** $k$-regular = every vertex has degree $k$.
- Edges: $|E| = nk/2$ (so $nk$ even)
- Complement of $k$-regular on $n$ vertices is $(n-1-k)$-regular

**Edge Bounds:** Simple: $0 \leq |E| \leq \binom{n}{2}$. Connected: $|E| \geq n-1$. Forest with $k$ components: $|E| = n - k$.

**Euler's Formula Variants:**
- Connected planar: $V - E + F = 2$
- Planar with $c$ components: $V - E + F = c + 1$
- Surface of genus $g$: $V - E + F = 2 - 2g$

**Graph Properties Quick Reference:**

| Property | $K_n$ | $K_{m,n}$ | $C_n$ | $P_n$ | Tree |
|---|---|---|---|---|---|
| Edges | $\frac{n(n-1)}{2}$ | $mn$ | $n$ | $n-1$ | $n-1$ |
| $\chi(G)$ | $n$ | 2 | 2(even)/3(odd) | 2 | 2 |
| Planar? | $n \leq 4$ | $\min(m,n) \leq 2$ | Yes | Yes | Yes |
| Euler circuit? | $n$ odd | $m,n$ even | Yes | No | No |
| Hamilton circuit? | $n \geq 3$ | $m=n \geq 2$ | $n \geq 3$ | No | No |

### 11.22 Spanning Trees — Advanced (⭐ GATE)

**Kirchhoff's Matrix Tree Theorem:** # spanning trees = any cofactor of Laplacian matrix $L = D - A$.

| Graph | # Spanning Trees |
|---|---|
| $K_n$ | $n^{n-2}$ (Cayley's formula) |
| $C_n$ | $n$ |
| $K_4$ | 16 |
| $K_5$ | 125 |
| Petersen graph | 2000 |

**Properties:** Connected graph has $\geq 1$ spanning tree. Unique spanning tree iff the graph IS a tree. Adding a non-tree edge creates exactly one cycle.

**Minimum Spanning Tree (MST) — Brief (⭐ Primarily in Algorithms, but connects here)** 🌲
- **MST** = spanning tree with minimum total edge weight
- **Kruskal's:** Sort edges by weight, add lightest edge that doesn't create a cycle → $O(E \log E)$
- **Prim's:** Grow tree greedily from a vertex, always adding cheapest crossing edge → $O(E \log V)$
- **Cut Property:** The lightest edge crossing any cut MUST be in every MST (if unique)
- **Cycle Property:** The heaviest edge on any cycle is NOT in any MST (if unique)
- Number of spanning trees of a graph can be computed using Kirchhoff's theorem (above)

> 🎯 **GATE Link:** MST questions appear in both Discrete Math (counting/existence) and Algorithms (finding MST). Know the connection!

### 11.22.0 Kirchhoff's Matrix Tree Theorem — Worked Example 🌳

Take the complete graph $K_4$.

Its adjacency matrix is:

$$A = \begin{bmatrix}
0 & 1 & 1 & 1 \\
1 & 0 & 1 & 1 \\
1 & 1 & 0 & 1 \\
1 & 1 & 1 & 0
\end{bmatrix}$$

Each vertex has degree 3, so:

$$D = \begin{bmatrix}
3 & 0 & 0 & 0 \\
0 & 3 & 0 & 0 \\
0 & 0 & 3 & 0 \\
0 & 0 & 0 & 3
\end{bmatrix}$$

Hence the Laplacian is:

$$L = D-A = \begin{bmatrix}
3 & -1 & -1 & -1 \\
-1 & 3 & -1 & -1 \\
-1 & -1 & 3 & -1 \\
-1 & -1 & -1 & 3
\end{bmatrix}$$

Delete any one row and the corresponding column, for example row 4 and column 4:

$$L' = \begin{bmatrix}
3 & -1 & -1 \\
-1 & 3 & -1 \\
-1 & -1 & 3
\end{bmatrix}$$

Now compute the determinant:

$$\det(L') = 16$$

Therefore, the number of spanning trees of $K_4$ is:

$$\tau(K_4)=16$$

This matches Cayley's formula $n^{n-2}$ because for $n=4$:

$$4^{4-2}=4^2=16$$

> 🎯 Exam use: if a question gives you a small graph and asks for number of spanning trees, build $L=D-A$, delete one row and column, and take the determinant of the cofactor.

### 11.22.1 Prüfer Sequence (⭐ GATE — Cayley's Formula) 🔢

> The Prüfer sequence provides a **bijection** between labeled trees on $\{1, ..., n\}$ and sequences of length $n-2$ over $\{1, ..., n\}$ — proving Cayley's formula $n^{n-2}$!

**Tree → Prüfer sequence (encoding):**
1. Find the **leaf** (degree-1 vertex) with the **smallest label**
2. Record its **neighbor's label** in the sequence
3. Remove the leaf and its edge
4. Repeat until 2 vertices remain

**Prüfer sequence → Tree (decoding):**
1. Start with sequence $s_1, s_2, \ldots, s_{n-2}$ and vertices $\{1, \ldots, n\}$
2. Make a list of vertices NOT in the remaining sequence
3. Connect the smallest such vertex to $s_1$, remove $s_1$ from sequence, update
4. Repeat; connect the last two remaining vertices

**Key properties:**
- Vertex $v$ appears in the Prüfer sequence **exactly $\deg(v) - 1$ times**
- Leaves (degree 1) DON'T appear in the sequence
- Number of labeled trees where vertex $v$ has degree $d_v$: $\frac{(n-2)!}{\prod_{v} (d_v - 1)!}$ (multinomial coefficient!)

### 11.22.2 Graph Operations (⭐ GATE Awareness) 🔧

| Operation | Definition | Notation |
|---|---|---|
| **Subgraph** | Subset of vertices and edges | $H \subseteq G$ |
| **Induced subgraph** | All edges between chosen vertices | $G[S]$ |
| **Edge deletion** | Remove an edge | $G - e$ |
| **Vertex deletion** | Remove vertex and all its edges | $G - v$ |
| **Edge contraction** | Merge endpoints of an edge | $G / e$ |
| **Graph union** | Disjoint union of two graphs | $G_1 \cup G_2$ |
| **Graph join** | Union + all edges between $G_1$ and $G_2$ | $G_1 + G_2$ |
| **Cartesian product** | $(u_1, v_1) \sim (u_2, v_2)$ iff $u_1 = u_2, v_1 \sim v_2$ or $u_1 \sim u_2, v_1 = v_2$ | $G_1 \square G_2$ |
| **Subdivision** | Replace edge with path (add vertex on edge) | |
| **Homeomorphic** | Same up to subdivision | |

**Key examples:**
- $K_1 + C_n = W_n$ (wheel = join of single vertex with cycle) 🎡
- $P_2 \square P_3$ = grid graph (2×3 grid)
- $Q_n = \underbrace{K_2 \square K_2 \square \cdots \square K_2}_{n \text{ times}}$ (hypercube = repeated Cartesian product!)

### 11.22.3 Line Graph $L(G)$ (⭐ GATE) 📈

The **line graph** $L(G)$ of $G$ has:
- Vertices = edges of $G$
- Two vertices in $L(G)$ are adjacent iff the corresponding edges in $G$ share an endpoint

**Properties:**
- $|V(L(G))| = |E(G)|$
- $|E(L(G))| = \sum_{v \in V(G)} \binom{\deg(v)}{2}$ (each vertex $v$ of degree $d$ contributes $\binom{d}{2}$ edges to $L(G)$)
- $L(K_n) = $ triangular graph, which is $\frac{n(n-1)}{2}$ vertices, each with degree $2(n-2)$
- $L(K_{1,n}) = K_n$ (line graph of a star is a complete graph!) ⭐

> 🎯 **GATE Relevance:** Edge coloring of $G$ = vertex coloring of $L(G)$! This connects chromatic index and chromatic number.

### 11.22.4 Outerplanar Graphs 🔲

A graph is **outerplanar** if it has a planar embedding with ALL vertices on the outer face.

**Properties:**
- Every outerplanar graph is planar ✅
- $E \leq 2V - 3$ (for outerplanar with $V \geq 2$)
- $K_4$ is NOT outerplanar (but $K_3$ is)
- $K_{2,3}$ is NOT outerplanar
- Outerplanar iff no subdivision of $K_4$ or $K_{2,3}$
- $\chi(G) \leq 3$ for outerplanar graphs
- Every outerplanar graph has a vertex of degree $\leq 2$

### 11.22.5 Dual of a Planar Graph 🪞

For a connected planar graph $G$, its **dual** $G^*$ has:
- Vertices = faces of $G$
- Two vertices in $G^*$ adjacent iff their faces share an edge in $G$

**Properties:**
- $(G^*)^* \cong G$ (for 3-connected planar graphs)
- $|V(G^*)| = |F(G)|$ (faces of $G$), $|E(G^*)| = |E(G)|$, $|F(G^*)| = |V(G)|$
- Euler's formula preserved: $V^* - E^* + F^* = V^* - E + V = F - E + V = 2$ ✅
- Spanning tree of $G$ ↔ complement is spanning tree of $G^*$

> 🏭 **Application:** Dual graphs are used in VLSI design, mesh generation for finite element analysis, and Geographic Information Systems (GIS) for map coloring!

### 11.22.6 Network Flow — Brief (⭐ Connects to Max Matching) 🌊

> Network flow is primarily in the Algorithms syllabus, but its connection to graph theory results appears in GATE DM questions.

**Max-Flow Min-Cut Theorem (Ford-Fulkerson):** Maximum flow from $s$ to $t$ = Minimum capacity of an $s$-$t$ cut.

**Connections to combinatorics:**
- **König's theorem** (max matching = min vertex cover in bipartite) is proved using max-flow!
- **Menger's theorem:** Max number of vertex-disjoint $s$-$t$ paths = min vertex cut between $s$ and $t$
- **Hall's Marriage theorem** follows from max-flow in bipartite graphs

---

## Chapter 12: Proof Techniques 📝

> 💡 **Beginner Analogy — Building a Legal Case 🧑‍⚖️:**
> Proof techniques are like different strategies a lawyer uses:
> - **Direct Proof** = "Here's the evidence, step by step" 📄
> - **Proof by Contradiction** = "If my client is guilty, then [something impossible], therefore NOT guilty!" 🚨
> - **Proof by Contrapositive** = "Instead of proving A→B, let me prove NOT-B→NOT-A" (equivalent!) 🔄
> - **Mathematical Induction** = "The first domino falls ↓, and each domino knocks the next ↓↓↓, so ALL dominoes fall!" 🎲
>
> 🏭 **Production Use:** **Formal verification** tools like TLA+ (used by Amazon AWS to verify distributed systems) and Coq/Lean (used in verified compilers) ALL use these exact proof techniques! Amazon confirmed that TLA+ found critical bugs in DynamoDB that testing alone couldn't catch! 🐛

### 12.1 Direct Proof

To prove $p \rightarrow q$: Assume $p$ is true, derive that $q$ is true.

**Example:** Prove "If $n$ is even, then $n^2$ is even."
- Assume $n$ is even, so $n = 2k$.
- Then $n^2 = 4k^2 = 2(2k^2)$, which is even. ✓

### 12.2 Proof by Contrapositive

To prove $p \rightarrow q$: Prove $\neg q \rightarrow \neg p$.

**Example:** Prove "If $n^2$ is even, then $n$ is even."
- Contrapositive: "If $n$ is odd, then $n^2$ is odd."
- If $n = 2k+1$, then $n^2 = 4k^2 + 4k + 1 = 2(2k^2 + 2k) + 1$, which is odd. ✓

### 12.3 Proof by Contradiction

To prove $p$: Assume $\neg p$ and derive a contradiction.

**Example:** Prove $\sqrt{2}$ is irrational.
- Assume $\sqrt{2} = a/b$ in lowest terms
- Then $2b^2 = a^2$, so $a^2$ is even, so $a$ is even: $a = 2c$
- Then $2b^2 = 4c^2$, so $b^2 = 2c^2$, so $b$ is even
- Both $a$ and $b$ are even — contradicts "lowest terms." ✓

### 12.4 Mathematical Induction (⭐ FREQUENTLY ASKED)

**Steps:**
1. **Base case:** Prove $P(n_0)$ is true (usually $n_0 = 0$ or $1$)
2. **Inductive hypothesis:** Assume $P(k)$ is true for some arbitrary $k \geq n_0$
3. **Inductive step:** Prove $P(k+1)$ is true using the hypothesis

**Strong Induction:** Assume $P(n_0), P(n_0+1), \ldots, P(k)$ are all true, then prove $P(k+1)$.

**Common induction pitfalls ⚠️:**
- Forgetting the base case (the "All horses are the same color" fallacy!)
- Using $P(k)$ to prove $P(k+1)$ but the step doesn't work for the base→base+1 transition
- Proving $P(k) \Rightarrow P(k+1)$ but starting from the wrong base case

**Classic GATE Induction Proofs:**
- $\sum_{i=1}^{n} i = \frac{n(n+1)}{2}$
- $\sum_{i=1}^{n} i^2 = \frac{n(n+1)(2n+1)}{6}$
- $\sum_{i=0}^{n} 2^i = 2^{n+1} - 1$
- $n! > 2^n$ for $n \geq 4$
- A tree with $n$ vertices has $n-1$ edges
- Every integer $n \geq 2$ has a prime factorization (strong induction)

### 12.4.1 Proof by Exhaustion / Cases 📋

> Divide the proof into a finite number of cases, prove each separately.

**Example:** Prove that $n^2 + n$ is even for all integers $n$.
- **Case 1:** $n$ is even, say $n = 2k$. Then $n^2 + n = 4k^2 + 2k = 2(2k^2 + k)$ — even! ✅
- **Case 2:** $n$ is odd, say $n = 2k+1$. Then $n^2 + n = (2k+1)^2 + (2k+1) = 4k^2 + 4k + 2 = 2(2k^2 + 2k + 1)$ — even! ✅

> 🎯 **GATE Tip:** Proof by cases is especially useful for modular arithmetic problems (split by remainder mod $k$) and graph theory (split by degree parity).

### 12.4.2 Constructive vs Non-Constructive Proofs 🏗️ vs 👻

**Constructive proof:** Actually exhibits/constructs the object whose existence is claimed.
- "There exists an even prime" → Proof: $2$ is prime and even. ✅ (We SHOWED it!)

**Non-constructive proof:** Proves existence without constructing the object.
- "Among any 6 people, 3 are mutual acquaintances or 3 are mutual strangers" → Pigeonhole argument proves existence but doesn't tell you WHICH 3! 👻

> 🔥 **Big CS Connection:** The existence of Turing-undecidable languages is proved non-constructively (countability argument). The P ≠ NP conjecture, if true, would mean certain algorithms EXIST but we can't find them efficiently!

### 12.5 Strong Induction — Detailed (⭐ GATE USES THIS)

**When to use Strong Induction:**
- When proving $P(k+1)$ requires multiple previous cases, not just $P(k)$
- Classic example: Every integer $n \geq 2$ has a prime factorization

**Template:**
1. **Base:** Verify $P(n_0)$ (and possibly $P(n_0+1), \ldots, P(n_0+m)$)
2. **Hypothesis:** Assume $P(n_0), P(n_0+1), \ldots, P(k)$ all true
3. **Step:** Prove $P(k+1)$ using ANY of $P(n_0) \ldots P(k)$

**Example (GATE-style):** Prove every $n \geq 2$ can be written as product of primes.
- Base: $n = 2$ is prime ✅
- Hypothesis: Every $m$ with $2 \leq m \leq k$ has a prime factorization
- Step for $k+1$: If $k+1$ is prime, done. If composite, $k+1 = a \cdot b$ with $2 \leq a, b \leq k$. By hypothesis both have prime factorizations. Combine them. ✅

### 12.6 Structural Induction (⭐ GATE — Trees, Strings, Formulas)

Used to prove properties of **recursively defined structures** (trees, formulas, strings).

**Template:**
1. **Base:** Prove for base elements of the recursive definition
2. **Hypothesis:** Assume property holds for sub-structures
3. **Step:** Prove for structures built using recursive rules

**Example:** Prove for any full binary tree: $\text{leaves} = \text{internal nodes} + 1$.
- Base: Single node (leaf) → $1 = 0 + 1$ ✅
- If tree $T$ has subtrees $T_L, T_R$: leaves$(T) =$ leaves$(T_L) +$ leaves$(T_R)$; internal$(T) = 1 +$ internal$(T_L) +$ internal$(T_R)$
- By hypothesis: leaves$(T_L) =$ internal$(T_L) + 1$ and same for $T_R$
- So leaves$(T) =$ (internal$(T_L) + 1) +$ (internal$(T_R) + 1) = $ internal$(T) - 1 + 2 =$ internal$(T) + 1$ ✅

### 12.7 Well-Ordering Principle (⭐ GATE AWARENESS)

**Every non-empty subset of $\mathbb{N}$ has a smallest element.**

Equivalent to mathematical induction but sometimes easier to apply. Proof by WOP uses contradiction:
1. Assume property $P(n)$ fails for some $n$
2. Let $S = \{n \in \mathbb{N} : \neg P(n)\}$. By assumption $S \neq \emptyset$
3. By WOP, $S$ has a smallest element $m$
4. Derive a contradiction by showing $P(m)$ must hold (using $P(k)$ for all $k < m$)

**GATE Tip:** WOP is the foundation underlying induction proofs. If a GATE question asks "which proof technique guarantees a minimal counterexample," the answer is the Well-Ordering Principle.

### 12.7.1 Disproof, Invariants & Extremal Principle 🛠️

Not every statement should be proved. Some should be destroyed with one sharp counterexample. 😎

**Counterexample (fastest disproof) ❌**

To disprove a universal statement like "for all $n$ ...", it is enough to find ONE value where it fails.

Examples:
- "Every prime is odd" is false because $2$ is prime and even
- "Every connected graph is Hamiltonian" is false; many trees are connected but not Hamiltonian

**Invariant Method 🔒**

An **invariant** is a quantity that does not change under the allowed moves of a process.

Typical invariants:
- Parity (odd/even)
- Number of inversions mod 2
- Color class of a chessboard square
- Sum modulo $k$

If initial and target states have different invariant values, the transformation is impossible.

**Monovariant Method 📉**

A **monovariant** is a quantity that moves only in one direction (always increasing or always decreasing).

This is useful to prove:
- an algorithm terminates
- a process cannot cycle forever
- a greedy procedure makes progress

**Extremal Principle 🏔️**

Choose an element that is smallest, largest, leftmost, rightmost, earliest, or otherwise extreme, then reason from that.

This appears in proofs such as:
- choosing the smallest counterexample
- choosing a vertex of minimum degree
- choosing the shortest path or smallest violating set

> 💡 Strong link: the well-ordering principle is exactly what justifies choosing a smallest counterexample in many proofs.

---

## 🚨 Common Mistakes in Discrete Mathematics

> ⚠️ **GATE Aspirants: These are the MOST common mistakes that cost marks every year! Read this section before every mock test!**

| Mistake | Correct Approach |
|---|---|
| $p \rightarrow q \equiv q \rightarrow p$ | NO! Converse ≠ Original |
| $\forall x(A(x) \wedge B(x))$ for "All A are B" | Should be $\forall x(A(x) \rightarrow B(x))$ |
| Confusing reflexive with symmetric | Reflexive: $(a,a)$; Symmetric: $(a,b) \Rightarrow (b,a)$ |
| Equivalence relation ⟹ partial order | NO! Sym ↔ Antisym are opposite in general |
| $K_{3,3}$ is planar | NO! $K_{3,3}$ is non-planar |
| Euler path needs all even degrees | Euler CIRCUIT needs all even; PATH needs exactly 2 odd |
| $P(n,r) = \binom{n}{r}$ | $P(n,r) = r! \cdot \binom{n}{r}$ |
| Pigeonhole gives exact count | It gives existence of at least one with $\geq \lceil n/m \rceil$ |
| Maximal matching = Maximum matching | Maximal: can't grow; Maximum: largest possible! |
| $\lfloor -2.3 \rfloor = -2$ | NO! $\lfloor -2.3 \rfloor = -3$ (floor rounds DOWN, not toward zero!) |
| Path and Trail are the same thing | Trail: no repeated edges; Path: no repeated vertices |
| $\exists x(A(x) \rightarrow B(x))$ for "Some A are B" | Almost always vacuously true! Use $\exists x(A(x) \wedge B(x))$ |
| "All groups have subgroups of every divisor order" | Lagrange gives necessary condition, NOT sufficient! ($A_4$ has no subgroup of order 6) |
| "Every connected graph has a Hamiltonian path" | FALSE — this is NP-complete to decide! |
| "Trees have no edges" | Trees on $n$ vertices have exactly $n-1$ edges! |

---

## 🏆 GATE Weightage by Sub-topic

> 💡 **Pro Tip:** Graph Theory + Combinatorics alone account for 50-60% of Discrete Math marks. Master these two and you're already ahead of 80% of aspirants! 🚀

| Topic | Expected Questions | Marks |
|---|---|---|
| Propositional Logic | 1 | 1–2 |
| First Order Logic | 1 | 1–2 |
| Set Theory / Relations | 1–2 | 2–4 |
| Group Theory | 0–1 | 1–2 |
| Combinatorics | 1–2 | 2–4 |
| Graph Theory | 1–2 | 2–4 |
| **Total** | **5–8** | **8–15** |

### 🎯 GATE DA Specific Focus (Data Science & AI)

> GATE DA has the **SAME Discrete Mathematics syllabus** as GATE CS, but questions tend to emphasize:

| Topic | DA Emphasis | Why |
|---|---|---|
| **Combinatorics** | ⭐⭐⭐ Higher weightage | Probability foundations for ML require strong counting skills |
| **Graph Theory** | ⭐⭐⭐ Higher weightage | Network analysis, Bayesian networks (DAGs), social graphs |
| **Relations & Functions** | ⭐⭐ Standard | Equivalence classes for clustering, function properties for mappings |
| **Logic** | ⭐⭐ Standard | Knowledge representation, rule-based systems |
| **Group Theory** | ⭐ Lower weightage | Less emphasis compared to CS |

**Key DA crossover topics:**
1. 📊 **Generating functions** → connect to probability generating functions in statistics
2. 🌳 **Trees/DAGs** → Bayesian networks, decision trees in ML
3. 🔢 **Counting/Combinatorics** → Probability calculations (equally likely outcomes = counting!)
4. 📐 **Lattices & Partial Orders** → Concept lattices in Formal Concept Analysis
5. 🔗 **Graph coloring & matching** → Scheduling, assignment problems in optimization

---
---

# 🎯 GATE QUICK SOLVER STRATEGIES

> **Time-saving techniques for the exam hall! Each strategy saves 30–90 seconds per question.**

### Strategy 1: Small Cases First 🔬
For "which of these is always true?" questions:
1. **Try $n = 1$ or smallest non-trivial case** before attempting a general proof
2. If ANY small case fails → option is FALSE (eliminate!) ❌
3. If all small cases pass → likely TRUE, then verify reasoning ✅

### Strategy 2: Counting Verification 🔢
When asked "how many relations/functions/graphs...":
1. Compute for $n = 1$ and $n = 2$ (trivial cases)
2. Check which answer option matches BOTH values
3. Often eliminates all but one option!

### Strategy 3: Degree Sequence Quick Check 📊
For graph problems:
- Sum of degrees must be even ($= 2|E|$)
- If a degree sequence has odd sum → impossible, eliminate!
- Max degree in simple graph on $n$ vertices is $n-1$

### Strategy 4: Contrapositive for Implications ↩️
When checking implications like $P \Rightarrow Q$:
- Instead of proving $P \Rightarrow Q$, check if $\neg Q \Rightarrow \neg P$
- Often easier to find a counterexample to the converse than to prove directly

### Strategy 5: Boundary Values for Floor/Ceiling 🏠
- Test with integers AND non-integers (especially negative!)
- $\lfloor -2.3 \rfloor = -3$ (NOT $-2$!) — most common GATE trap

### Strategy 6: Pigeonhole for Existence ✨
If question asks "must there exist..." → think Pigeonhole!
- Count items (pigeons) and categories (holes)
- $\lceil n/k \rceil$ gives the minimum guarantee

### Strategy 7: Graph Theory Power Moves ⚡
| Question Type | Quick Approach |
|---|---|
| "Is this planar?" | Check $E \leq 3V - 6$ (or $2V - 4$ if no triangles) |
| "Euler path/circuit?" | Count odd-degree vertices: 0 → circuit, 2 → path |
| "Chromatic number?" | Check bipartite (2), then odd cycle (3), then clique |
| "Connected?" | Check $E \geq V - 1$ (necessary) |
| "How many spanning trees?" | Kirchhoff's Matrix Tree Theorem |

---
---

# 📚 APPENDIX

## J1: Common GATE Traps & Pitfalls (Master List)

| # | Trap | Correct Understanding |
|---|---|---|
| 1 | "CNF and DNF are unique" | CNF/DNF are NOT unique; only PCNF/PDNF are canonical |
| 2 | "$\{\wedge, \vee\}$ is functionally complete" | NO — cannot express $\neg$ |
| 3 | "Self-complementary graph exists for any $n$" | Only when $n \equiv 0$ or $1 \pmod{4}$ |
| 4 | "$\forall x(A(x) \wedge B(x))$" for "all A are B" | Must use $\rightarrow$: $\forall x(A(x) \rightarrow B(x))$ |
| 5 | "Euler circuit ↔ Hamiltonian circuit" | Euler: visit all EDGES; Hamilton: visit all VERTICES |
| 6 | "Every lattice is distributive" | NO — $M_3$ and $N_5$ are non-distributive lattices |
| 7 | "$K_{3,3}$ fails $E \leq 3V-6$" | NO — $K_{3,3}$ has no triangles, use $E \leq 2V-4$ instead |
| 8 | "Generating functions are infinite series — only for infinite sequences" | GF works for finite sequences too (polynomial GF) |
| 9 | "Symmetric relation can't be antisymmetric" | YES it can — the identity relation is both |
| 10 | "Group of order $n$ always has subgroup of every divisor" | Lagrange says divisors are NECESSARY for subgroup orders, not SUFFICIENT (converse false in general; true for abelian groups by Cauchy/Sylow) |
| 11 | "Catalan number counts binary trees" | $C_n$ counts FULL binary trees with $n$ internal nodes, or BINARY TREES with $n$ nodes (different interpretations — be careful with the problem statement) |
| 12 | "Adjacency matrix of undirected graph can be asymmetric" | Always symmetric for undirected graphs |
| 13 | "Complement of connected graph is disconnected" | Complement of disconnected graph is connected; complement of connected may or may not be connected |
| 14 | "$\phi(n)$ is always even" | False: $\phi(1) = 1$, $\phi(2) = 1$ (odd). For $n \geq 3$, $\phi(n)$ is always even |
| 15 | "Resolution is complete for propositional logic" | YES — resolution is refutation-complete (can derive empty clause from unsatisfiable set) |

### J2: Additional GATE Traps & Pitfalls 🚨

| # | Trap | Correct Understanding |
|---|---|---|
| 16 | "$\lfloor -2.7 \rfloor = -2$" | NO! $\lfloor -2.7 \rfloor = -3$ (floor rounds DOWN toward $-\infty$, not toward 0!) |
| 17 | "Maximal matching = Maximum matching" | Maximal just means you can't add more edges; maximum means the LARGEST possible |
| 18 | "All lattices are distributive" | NO — $M_3$ (diamond) and $N_5$ (pentagon) are non-distributive |
| 19 | "Path = Trail = Walk" | Walk allows repeats; Trail: no repeated edges; Path: no repeated vertices |
| 20 | "Composition of $n$ = Partition of $n$" | Composition: ORDER MATTERS ($2^{n-1}$); Partition: order doesn't matter ($p(n)$) |
| 21 | "Hall's theorem works for non-bipartite" | Hall's marriage theorem is ONLY for bipartite graphs |
| 22 | "Dilworth = something about chains" | Dilworth: max antichain = min chain decomposition. Mirsky: max chain = min antichain decomposition |
| 23 | "$\mathbb{Z}_2 \times \mathbb{Z}_2 \cong \mathbb{Z}_4$" | NO! $\gcd(2,2) \neq 1$, so product is NOT cyclic. Klein four-group ≠ $\mathbb{Z}_4$ |
| 24 | "Every tournament has a Hamiltonian CIRCUIT" | NO! Every tournament has a Hamiltonian PATH, not necessarily a circuit |
| 25 | "For functions: non-surjective implies non-injective" | Only true when $|A| > |B|$. When $|A| = |B|$: injective iff surjective iff bijective |
| 26 | "Euler's formula works for disconnected graphs" | For $c$ components: $V - E + F = c + 1$ (NOT $c + 2$!) |
| 27 | "Characteristic function and indicator function are different" | They are the SAME thing — $\chi_A(x) = 1$ iff $x \in A$ |
| 28 | "Complete binary tree with $n$ nodes has Catalan number BSTs" | $C_n$ counts STRUCTURALLY different binary trees (or BSTs with $n$ keys), not complete ones |
