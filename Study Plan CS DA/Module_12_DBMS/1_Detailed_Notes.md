# 🗄️ Database Management Systems (DBMS) — Detailed Notes for GATE 2027 CS/IT/DA 🎯

> 🌟 **"Every app you use — Instagram, Amazon, Zomato, Paytm, your bank — has a database behind it. DBMS is the technology that makes sure your data is safe, fast, and always available!"**

> 💡 **Beginner Analogy — The Super-Organized Library 📚:**
> Imagine a massive library with millions of books:
> - **Without DBMS (File System)** = Books piled randomly on the floor 😱 — finding anything takes HOURS, books get lost, duplicated, damaged
> - **With DBMS** = Books cataloged, indexed, shelved by category 📚 — librarian can find any book in SECONDS, ensures no duplicates, controls who can borrow what
>
> A DBMS does for DATA what a librarian does for BOOKS — but billions of times faster! 🚀
>
> Fun fact: The world generates **2.5 quintillion bytes of data EVERY DAY**. Without DBMS, none of this could be stored or retrieved efficiently! 🌍

---

## 🏭 Where is DBMS Used in the Real World?

| 🌍 Company/System | 🔧 What They Store | 📌 DBMS Used |
|---|---|---|
| **Instagram** 📸 | 2+ billion users, 100M+ photos/day | PostgreSQL, Cassandra |
| **Amazon** 🛒 | Product catalogs, orders, reviews | DynamoDB, Aurora, RDS |
| **Zomato/Swiggy** 🍜 | Restaurants, menus, orders, delivery tracking | MySQL, MongoDB |
| **Paytm/PhonePe** 💳 | Transactions, wallets, KYC data | PostgreSQL, Oracle |
| **IRCTC** 🚂 | Train schedules, bookings, passenger data | Oracle |
| **Aadhaar** 🇮🇳 | 1.4 billion biometric records | MySQL + Custom solutions |
| **Netflix** 🎬 | 260M+ user profiles, watch history | Cassandra, MySQL |
| **WhatsApp** 💬 | Message metadata, user data | Custom distributed DB |
| **Banking (SBI, HDFC)** 🏦 | Account data, transactions | Oracle, DB2 |
| **Healthcare** 🏥 | Patient records (EMR systems) | PostgreSQL, Oracle |

---

## 📊 GATE Weightage & Exam Strategy

| Parameter | Details |
|---|---|
| **Expected Marks** | 8–12 marks every year |
| **Questions** | 3–5 questions (mix of 1-mark and 2-mark) |
| **High-Yield Topics** | Normalization & FDs, SQL queries, Relational Algebra, B/B+ Trees, Transaction & Concurrency |
| **Moderate-Yield** | ER Model, Indexing (primary/secondary/clustering), TRC |
| **Low-Yield** | File Organization, Disk structure (more for CS than DA) |

**Topic-wise Frequency (last 10 years):**

| Topic | Avg Questions/Year | Typical Marks |
|---|---|---|
| Normalization & FDs | 1–2 | 2–4 |
| SQL | 1–2 | 2–4 |
| Relational Algebra | 1 | 1–2 |
| B-Tree / B+ Tree | 1 | 1–2 |
| Transactions & Concurrency | 1 | 1–2 |
| ER Model | 0–1 | 1–2 |
| Indexing | 0–1 | 1–2 |
| TRC | 0–1 | 1–2 |

---

## Module 1: Introduction to DBMS & Relational Model

### 1.1 What is a DBMS? 🤔

> 🏭 **Deep Research Insight — The DBMS Revolution:**
> - Edgar F. Codd invented the **relational model** in 1970 at IBM 🏆 — this single paper changed computing forever!
> - Before DBMS, programmers spent **80% of their time** managing data files manually. After DBMS, they could focus on building actual applications!
> - Today's databases handle:
>   - **Google Spanner**: Globally distributed, serves Gmail, YouTube, Google Cloud (processes trillions of rows!) 🌍
>   - **Amazon Aurora**: 5x faster than MySQL, handles Amazon.com's peak traffic during Prime Day
>   - **Facebook TAO**: Handles 1 BILLION reads/second for social graph data 🤯

A **Database Management System** is software that manages a collection of interrelated data (database) and provides mechanisms to **define**, **construct**, **manipulate**, and **share** data among users and applications.

**Advantages over File Systems:**
- Data redundancy control
- Data consistency
- Data integrity (constraints)
- Concurrent access
- Security & authorization
- Backup & recovery
- Data independence

### 1.2 Data Models

| Data Model | Description | Example |
|---|---|---|
| **Hierarchical** | Tree structure, parent-child | IBM IMS |
| **Network** | Graph structure, many-to-many | CODASYL |
| **Relational** | Tables (relations) | MySQL, PostgreSQL |
| **Object-Oriented** | Objects with methods | O2, ObjectStore |
| **Object-Relational** | Relational + OO features | PostgreSQL |

### 1.3 Three-Schema Architecture & Data Independence

```
External Schema (View Level)
        ↕  Logical Data Independence
Conceptual Schema (Logical Level)
        ↕  Physical Data Independence
Internal Schema (Physical Level)
```

- **Logical Data Independence**: Ability to change the conceptual schema without changing external schemas or application programs
- **Physical Data Independence**: Ability to change the internal schema without changing the conceptual schema

### 1.4 The Relational Model

**Key Terminology:**

| Term | Also Called | Meaning |
|---|---|---|
| **Relation** | Table | A set of tuples |
| **Tuple** | Row / Record | A single entry |
| **Attribute** | Column / Field | A property |
| **Domain** | — | Set of allowed values for an attribute |
| **Degree** | Arity | Number of attributes |
| **Cardinality** | — | Number of tuples |

**Properties of Relations:**
1. Each cell contains a single (atomic) value
2. Each column has a distinct name
3. Values in a column are from the same domain
4. Order of rows is immaterial
5. Order of columns is immaterial
6. No two rows are identical (set semantics)

### 1.5 NULL Values

- **NULL** represents unknown, unavailable, or not applicable
- $\text{NULL} \neq 0$, $\text{NULL} \neq \text{empty string}$, $\text{NULL} \neq \text{NULL}$
- Any arithmetic with NULL → NULL
- Any comparison with NULL → **UNKNOWN** (three-valued logic)

**Three-Valued Logic Truth Table:**

| $A$ | $B$ | $A \land B$ | $A \lor B$ | $\neg A$ |
|---|---|---|---|---|
| T | U | U | T | F |
| F | U | F | U | T |
| U | U | U | U | U |

### 1.6 Keys

| Key Type | Definition |
|---|---|
| **Super Key** | Any set of attributes that uniquely identifies a tuple |
| **Candidate Key** | A minimal super key (no proper subset is a super key) |
| **Primary Key** | The chosen candidate key (cannot be NULL) |
| **Alternate Key** | Candidate keys not chosen as primary key |
| **Foreign Key** | Attribute(s) in one relation referencing the primary key of another |
| **Composite Key** | A key consisting of two or more attributes |

**Finding Candidate Keys from FDs — Algorithm:**
1. Find attributes that appear **only on LHS** (or neither side) → these must be in every candidate key
2. Compute closure of these attributes
3. If closure = all attributes → this is the only candidate key
4. Otherwise, add attributes that appear on **both sides** one at a time, check closure
5. Attributes appearing **only on RHS** are never part of any candidate key

> **Example:** $R(A,B,C,D,E)$, FDs: $\{A \to B, BC \to D, D \to E\}$
> - Only on LHS: $A, C$ (C appears nowhere on RHS alone)
> - $(AC)^+ = \{A,C,B,D,E\} = R$ → **AC is the only candidate key**

### 1.7 Integrity Constraints

1. **Domain Constraint**: Values must belong to the attribute's domain
2. **Key Constraint**: No two tuples can have the same value for key attributes
3. **Entity Integrity**: Primary key attributes cannot be NULL
4. **Referential Integrity**: A foreign key value must either be NULL or match a primary key value in the referenced relation

**Referential Integrity — Actions on Violation:**

| Action | On DELETE | On UPDATE |
|---|---|---|
| **RESTRICT / NO ACTION** | Reject deletion | Reject update |
| **CASCADE** | Delete referencing tuples | Update foreign key values |
| **SET NULL** | Set FK to NULL | Set FK to NULL |
| **SET DEFAULT** | Set FK to default value | Set FK to default value |

---

## Module 1 (contd.): Functional Dependencies & Normalization

### 1.8 Functional Dependencies (FDs)

A functional dependency $X \to Y$ means: if two tuples agree on attributes $X$, they must agree on attributes $Y$.

**Trivial FD**: $X \to Y$ where $Y \subseteq X$

### 1.9 Armstrong's Axioms (Sound & Complete)

| Rule | Statement | Formal |
|---|---|---|
| **Reflexivity** | If $Y \subseteq X$, then $X \to Y$ | Trivial FDs |
| **Augmentation** | If $X \to Y$, then $XZ \to YZ$ | Add attributes to both sides |
| **Transitivity** | If $X \to Y$ and $Y \to Z$, then $X \to Z$ | Chain rule |

**Derived Rules:**

| Rule | Statement |
|---|---|
| **Union** | If $X \to Y$ and $X \to Z$, then $X \to YZ$ |
| **Decomposition** | If $X \to YZ$, then $X \to Y$ and $X \to Z$ |
| **Pseudotransitivity** | If $X \to Y$ and $WY \to Z$, then $WX \to Z$ |

### 1.10 Closure of Attribute Set

$X^+$ = set of all attributes functionally determined by $X$ under a set of FDs $F$.

**Algorithm to compute $X^+$:**
```
result = X
repeat:
    for each FD (A → B) in F:
        if A ⊆ result:
            result = result ∪ B
until result does not change
return result
```

### 1.11 Closure of FD Set & Equivalence

- **Closure of FD set**: $F^+$ = set of all FDs logically implied by $F$
- Two FD sets $F$ and $G$ are **equivalent** ($F \equiv G$) if $F^+ = G^+$
- To check: verify every FD in $F$ is in $G^+$ AND every FD in $G$ is in $F^+$

### 1.12 Minimal Cover (Canonical Cover)

A minimal cover $F_c$ of $F$ satisfies:
1. **Single attribute on RHS** of every FD
2. **No extraneous attributes** on LHS (removing any attribute from LHS changes $F_c^+$)
3. **No redundant FDs** (removing any FD changes $F_c^+$)

**Algorithm:**
1. Decompose all FDs to have single attribute on RHS
2. Remove extraneous attributes from LHS: for each FD $XA \to B$, check if $B \in X^+$ under remaining FDs. If yes, replace $XA \to B$ with $X \to B$
3. Remove redundant FDs: for each FD $X \to A$, check if $A \in X^+$ under $(F_c - \{X \to A\})$. If yes, remove it

### 1.13 Normal Forms

#### Normal Form Identification Flowchart

```
Start with relation R and FDs F
          │
          ▼
  Does R have any non-atomic attributes?
     YES → NOT in 1NF
     NO ↓
  R is in 1NF
          │
          ▼
  Does any non-prime attribute have a 
  PARTIAL dependency on any candidate key?
     YES → In 1NF but NOT in 2NF
     NO ↓
  R is in 2NF
          │
          ▼
  Does any non-prime attribute have a 
  TRANSITIVE dependency on any candidate key?
     YES → In 2NF but NOT in 3NF
     NO ↓
  R is in 3NF
          │
          ▼
  For every non-trivial FD X→A:
  Is X a superkey OR is A a prime attribute?
  (3NF allows A to be prime)
  
  For BCNF: Is X a superkey for ALL non-trivial FDs?
     NO → In 3NF but NOT in BCNF
     YES ↓
  R is in BCNF
```

#### Definitions

| Normal Form | Condition |
|---|---|
| **1NF** | All attribute values are atomic (no multi-valued or composite attributes) |
| **2NF** | 1NF + No partial dependency (non-prime attribute depends on a proper subset of a candidate key) |
| **3NF** | 2NF + No transitive dependency. Equivalently: For every non-trivial FD $X \to A$, either $X$ is a superkey OR $A$ is a prime attribute |
| **BCNF** | For every non-trivial FD $X \to A$, $X$ must be a superkey |

**Key Definitions:**
- **Prime attribute**: An attribute that is part of some candidate key
- **Non-prime attribute**: An attribute not part of any candidate key
- **Partial dependency**: $X \to A$ where $X$ is a proper subset of a candidate key and $A$ is non-prime
- **Transitive dependency**: If $X \to Y$ and $Y \to A$ where $Y$ is not a superkey and $A$ is non-prime

> **Critical GATE fact:** BCNF ⊂ 3NF ⊂ 2NF ⊂ 1NF. A relation in BCNF is always in 3NF but NOT vice versa.
> 3NF allows non-trivial FDs where LHS is not a superkey **only if** RHS is a prime attribute.

#### Example: Identifying Normal Forms

$R(A, B, C, D)$, FDs: $\{AB \to C, C \to D, D \to A\}$

**Step 1: Find candidate keys**
- $(AB)^+ = \{A,B,C,D\}$ → AB is a candidate key
- $(BC)^+ = \{B,C,D,A\}$ → BC is a candidate key  
- $(BD)^+ = \{B,D,A,C\}$ → BD is a candidate key
- Prime attributes: $\{A, B, C, D\}$ — all attributes are prime!

**Step 2:** Since all attributes are prime, there can be no partial or transitive dependency on non-prime attributes → **R is in 3NF**

**Step 3: Check BCNF**
- $C \to D$: Is $C$ a superkey? $C^+ = \{C,D,A\} \neq R$ → **NOT BCNF**
- $D \to A$: Is $D$ a superkey? $D^+ = \{D,A\} \neq R$ → **NOT BCNF**

**Result:** R is in **3NF but not BCNF**.

### 1.14 Decomposition

#### Lossless-Join Decomposition (Binary)

For decomposition of $R$ into $R_1$ and $R_2$:

$$R_1 \bowtie R_2 = R \iff (R_1 \cap R_2 \to R_1 - R_2) \in F^+ \text{ OR } (R_1 \cap R_2 \to R_2 - R_1) \in F^+$$

The **common attributes** must be a superkey of at least one of the decomposed relations.

#### Chase Test (for n-ary decomposition)

Used to check lossless join for decomposition into **more than two** relations:

1. Create a tableau with rows = decomposed relations, columns = all attributes
2. For attributes in the decomposed relation, use $a_j$ (distinguished symbol)
3. For attributes NOT in the decomposed relation, use $b_{ij}$ (subscripted)
4. Apply FDs repeatedly: if two rows agree on LHS, make RHS symbols equal (prefer $a$ over $b$)
5. If any row becomes all $a$'s → **lossless**; otherwise → **lossy**

#### Dependency Preservation

Decomposition of $R$ into $R_1, R_2, \ldots, R_n$ is **dependency preserving** if:

$$(F_1 \cup F_2 \cup \ldots \cup F_n)^+ = F^+$$

where $F_i$ = projection of $F$ onto attributes of $R_i$.

> **GATE fact:** BCNF decomposition is always lossless but may **not** preserve all dependencies. 3NF decomposition can be both lossless and dependency preserving.

### 1.15 Number of Super Keys Formula

If a candidate key has $k$ attributes and the relation has $n$ attributes, then:

$$\text{Super keys from this CK} = 2^{n-k}$$

For multiple candidate keys, use **inclusion-exclusion**.

### 1.16 Multivalued Dependencies & 4NF ⭐

**Multivalued Dependency (MVD):** $X \twoheadrightarrow Y$ means: for every pair of tuples $t_1, t_2$ with $t_1[X] = t_2[X]$, there exist tuples $t_3, t_4$ such that:
- $t_3[X] = t_4[X] = t_1[X]$
- $t_3[Y] = t_1[Y]$, $t_3[R-X-Y] = t_2[R-X-Y]$
- $t_4[Y] = t_2[Y]$, $t_4[R-X-Y] = t_1[R-X-Y]$

Intuitively: Y values associated with X are **independent** of $R-X-Y$ values.

**Every FD is also an MVD:** $X \to Y$ implies $X \twoheadrightarrow Y$, but not vice versa.

**4NF:** A relation is in 4NF if for every non-trivial MVD $X \twoheadrightarrow Y$, $X$ is a superkey.
- 4NF ⊂ BCNF (4NF is stricter)
- Eliminates redundancy from independent multivalued facts

**Example:** R(Student, Course, Hobby) with a student taking multiple courses and having multiple hobbies independently.
- MVDs: Student ↠ Course, Student ↠ Hobby
- Student is NOT a superkey → violates 4NF
- Decompose: R1(Student, Course), R2(Student, Hobby)

### 1.17 Join Dependencies & 5NF

**Join Dependency (JD):** $*(R_1, R_2, \ldots, R_n)$ holds if $R = R_1 \bowtie R_2 \bowtie \ldots \bowtie R_n$ (lossless)

**5NF (Project-Join Normal Form):** A relation is in 5NF if every join dependency is implied by the candidate keys.
- 5NF ⊂ 4NF
- Rarely needed in practice
- Also called PJNF

**Normal Form Hierarchy:**
$$1NF \supset 2NF \supset 3NF \supset BCNF \supset 4NF \supset 5NF$$

---

### 🧮 Worked Example Bank: Functional Dependencies & Normalization

#### WE1: Attribute Closure — Step by Step

**Given:** $R(A,B,C,D,E,F,G)$, FDs: $\{A \to BC, CD \to E, B \to D, E \to A, D \to F\}$

**Find:** $(A)^+$

```
Step 0: result = {A}
Step 1: A → BC  ⟹ result = {A, B, C}
Step 2: B → D   ⟹ result = {A, B, C, D}
Step 3: CD → E  ⟹ result = {A, B, C, D, E}
Step 4: D → F   ⟹ result = {A, B, C, D, E, F}
Step 5: No new attributes added → STOP
```

$(A)^+ = \{A, B, C, D, E, F\}$ — A determines everything except G → **A is NOT a candidate key**

**Find:** $(AG)^+$
```
Step 0: result = {A, G}
Step 1: A → BC ⟹ {A, B, C, G}
Step 2: B → D  ⟹ {A, B, C, D, G}
Step 3: CD → E ⟹ {A, B, C, D, E, G}
Step 4: D → F  ⟹ {A, B, C, D, E, F, G} = R ✅
```

$(AG)^+ = R$ → **AG is a candidate key!**

> 🎯 **Finding ALL candidate keys systematically:**
> 1. G appears ONLY on the left/neither side → must be in every CK
> 2. A determines most things → try AG → works!
> 3. Since E → A: try EG → $(EG)^+ = \{E,G,A,B,C,D,F\} = R$ ✅ → EG is also a CK
> 4. Since CD → E: try CDG → works but not minimal? $(CG)^+$ doesn't give R, $(DG)^+$ doesn't give R → CDG is minimal CK
> 5. Since B → D: try BCG → $(BCG)^+ = \{B,C,G,D,E,A,F\} = R$ → BCG works, but is BG enough? $(BG)^+ = \{B,G,D,F\} ≠ R$ → No. Is CG? $(CG)^+ = \{C,G\} ≠ R$ → No. So BCG is minimal.
> **Candidate Keys: {AG, EG, CDG, BCG}**

#### WE2: Minimal Cover Computation

**Given:** FDs $F = \{A \to BC, B \to C, AB \to D, D \to BC\}$

**Step 1: Decompose RHS to single attributes**
- $A \to B$, $A \to C$, $B \to C$, $AB \to D$, $D \to B$, $D \to C$

**Step 2: Remove extraneous LHS attributes**
- $AB \to D$: Is $B$ extraneous? Check if $A \to D$ using remaining FDs.
  - $(A)^+ = \{A, B, C, D\}$ (using $A \to B$, $B \to C$, $A \to C$) → wait, we need D. $A \to B$, and then? We don't have $B \to D$. So $(A)^+$ without $AB \to D$... Hmm let me recompute.
  - Using $\{A \to B, A \to C, B \to C, D \to B, D \to C\}$: $(A)^+ = \{A,B,C\}$ — no D! → B is NOT extraneous
- $AB \to D$: Is $A$ extraneous? Check if $B \to D$ using remaining FDs.
  - $(B)^+ = \{B, C\}$ — no D! → A is NOT extraneous
- No changes.

**Step 3: Remove redundant FDs**
- $A \to C$: Is it redundant? Check $(A)^+$ using $\{A \to B, B \to C, AB \to D, D \to B, D \to C\}$
  - $(A)^+ = \{A, B, C, D\}$ → Yes, C is derived! → **Remove $A \to C$** ✅
- $D \to C$: Is it redundant? Check $(D)^+$ using $\{A \to B, B \to C, AB \to D, D \to B\}$
  - $(D)^+ = \{D, B, C\}$ → Yes, C derived via $D \to B$, $B \to C$! → **Remove $D \to C$** ✅

**Minimal Cover:** $F_c = \{A \to B, B \to C, AB \to D, D \to B\}$

Wait — recheck $AB \to D$: Since $A \to B$, then $AB \to D$ means $A \to D$ (B is extraneous now after removing $A \to C$ and $D \to C$). Let me recheck:
- With $\{A \to B, B \to C, AB \to D, D \to B\}$: Is B extraneous in $AB \to D$?
- $(A)^+$ using $\{A \to B, B \to C, D \to B\}$: $A^+ = \{A, B, C\}$ — no D → B NOT extraneous

**Final Minimal Cover:** $F_c = \{A \to B, B \to C, AB \to D, D \to B\}$

> 🎯 **GATE Trick:** Order of removing redundant FDs matters! Always recheck after each removal.

#### WE3: Normal Form Identification — Complete Walkthrough

**Given:** $R(A, B, C, D)$, FDs: $\{A \to B, B \to C\}$

**Step 1: Find candidate keys**
- $(A)^+ = \{A, B, C\} ≠ R$ → Need D
- $(AD)^+ = \{A, D, B, C\} = R$ → **AD is CK**
- D appears nowhere on RHS → D must be in every CK
- A appears nowhere on RHS → A must be in every CK
- **Only candidate key: AD**
- Prime attributes: $\{A, D\}$, Non-prime: $\{B, C\}$

**Step 2: Check 2NF**
- $A \to B$: A is a proper subset of CK(AD), B is non-prime → **Partial dependency!** ❌
- **R is in 1NF but NOT 2NF**

**Step 3: 2NF Decomposition**
- $R_1(A, B)$ with $A \to B$ → CK = A
- $R_2(A, C)$ — wait, we also have $B \to C$. Let's be systematic:
  - Group by LHS of FDs involving partial dependencies: $A \to B$ → $R_1(A, B)$
  - Remaining + key: $R_2(A, D, C)$ with $B \to C$ but B not in $R_2$... 
  
  Actually, proper 2NF decomposition: remove partial dependencies:
  - $R_1(A, B)$ [FD: $A \to B$]
  - $R_2(A, D, C)$ [FDs that don't cause partial dep]
  
  But $B \to C$ is lost! We need: $R_1(A, B)$, $R_3(B, C)$, $R_2(A, D)$
  
  Check: $R_1 \cap R_3 = \{B\}$, $B \to C$: B is key of $R_3$ → lossless ✅

#### WE4: 3NF Synthesis Algorithm — Full Example

**Given:** $R(A, B, C, D, E)$, FDs: $F = \{A \to B, BC \to D, D \to E, E \to A\}$

**Step 1: Find minimal cover** (already minimal — single RHS, no extraneous, no redundant)

**Step 2: Group FDs by LHS**
- $A \to B$ → $R_1(A, B)$
- $BC \to D$ → $R_2(B, C, D)$
- $D \to E$ → $R_3(D, E)$
- $E \to A$ → $R_4(E, A)$

**Step 3: Check if any relation contains a candidate key**
- CKs: $(AC)^+ = \{A,C,B,D,E\} = R$ → AC is CK
- $(EC)^+ = \{E,C,A,B,D\} = R$ → EC is CK
- $(DC)^+ = \{D,C,E,A,B\} = R$ → DC is CK
- $(BC)^+ = \{B,C,D,E,A\} = R$ → BC is CK

Does any $R_i$ contain a CK? $R_2(B,C,D)$ contains BC → Yes! ✅

**Step 4: No need to add extra relation for CK**

**Final 3NF decomposition:** $\{R_1(A,B), R_2(B,C,D), R_3(D,E), R_4(E,A)\}$
- Lossless: ✅ (guaranteed by algorithm since CK is in some $R_i$)
- Dependency preserving: ✅ (all FDs preserved in their respective relations)

#### WE5: BCNF Decomposition — With Dependency Loss

**Given:** $R(A, B, C)$, FDs: $\{AB \to C, C \to A\}$

**Candidate Keys:** AB and BC (verify: $(AB)^+ = R$, $(BC)^+ = \{B,C,A\} = R$)

**Check BCNF:**
- $AB \to C$: AB is superkey ✅
- $C \to A$: C is NOT superkey ($C^+ = \{C,A\} ≠ R$) ❌ → Violates BCNF!

**BCNF Decomposition (using $C \to A$):**
- $R_1(C, A)$ — the violating FD
- $R_2(B, C)$ — remaining attributes + common

**Check:** Is $AB \to C$ preserved?
- $F_1: C \to A$, $F_2:$ no FDs → $(AB)^+_{F_1 \cup F_2} = \{A, B\}$ — C NOT derived → **Dependency NOT preserved!** ❌

> 🎯 **Key GATE fact:** BCNF decomposition guarantees lossless join but may LOSE dependencies. 3NF synthesis guarantees BOTH lossless + dependency preserving.

#### WE6: Number of Super Keys — Inclusion-Exclusion

**Given:** $R(A, B, C, D, E)$, Candidate keys: $\{AB, AC, BCD\}$

**Method:** Super keys = those containing at least one CK

- Super keys containing AB: $2^{5-2} = 2^3 = 8$ (choose any subset of {C,D,E} to add)
- Super keys containing AC: $2^{5-2} = 8$
- Super keys containing BCD: $2^{5-3} = 4$
- Super keys containing AB AND AC = containing ABC: $2^{5-3} = 4$
- Super keys containing AB AND BCD = containing ABCD: $2^{5-4} = 2$
- Super keys containing AC AND BCD = containing ABCD: $2^{5-4} = 2$
- Super keys containing AB AND AC AND BCD = containing ABCDE: $2^{5-5} = 1 $... Actually containing ABCD: $2^{5-4} = 2$

By inclusion-exclusion:
$$|S| = 8 + 8 + 4 - 4 - 2 - 2 + 1 = 13$$

Wait, let me redo. AB ∪ AC = ABC (3 attrs), AB ∪ BCD = ABCD (4 attrs), AC ∪ BCD = ABCD (4 attrs), AB ∪ AC ∪ BCD = ABCDE (5 attrs)?? No, ABCD (4 attrs since ABC ∪ BCD = ABCD).

$$|S| = 2^3 + 2^3 + 2^2 - 2^2 - 2^1 - 2^1 + 2^{5-4} = 8 + 8 + 4 - 4 - 2 - 2 + 2 = 14$$

Hmm, let me be more careful. AB ∪ AC ∪ BCD = ABCD, so super keys containing all three = those containing ABCD = $2^{5-4} = 2$.

$$|S| = 8 + 8 + 4 - 4 - 2 - 2 + 2 = \boxed{14}$$

#### WE7: Chase Test Example

**Given:** $R(A,B,C,D)$, FDs: $\{A \to B, C \to D\}$, Decomposition: $R_1(A,C)$, $R_2(A,B)$, $R_3(C,D)$

**Tableau:**

| | A | B | C | D |
|---|---|---|---|---|
| $R_1$ | $a$ | $b_{12}$ | $c$ | $d_{14}$ |
| $R_2$ | $a$ | $b$ | $c_{23}$ | $d_{24}$ |
| $R_3$ | $a_{31}$ | $b_{32}$ | $c$ | $d$ |

**Apply $A \to B$:** Rows 1 and 2 agree on A → make B equal. $b_{12}$ becomes $b$ ✅

| | A | B | C | D |
|---|---|---|---|---|
| $R_1$ | $a$ | $b$ | $c$ | $d_{14}$ |
| $R_2$ | $a$ | $b$ | $c_{23}$ | $d_{24}$ |
| $R_3$ | $a_{31}$ | $b_{32}$ | $c$ | $d$ |

**Apply $C \to D$:** Rows 1 and 3 agree on C → make D equal. $d_{14}$ becomes $d$ ✅

| | A | B | C | D |
|---|---|---|---|---|
| $R_1$ | $a$ | $b$ | $c$ | $d$ |
| $R_2$ | $a$ | $b$ | $c_{23}$ | $d_{24}$ |
| $R_3$ | $a_{31}$ | $b_{32}$ | $c$ | $d$ |

**Row 1 is all $a$'s/unsubscripted → Lossless!** ✅

#### WE8: Multivalued Dependency & 4NF

**Given:** R(Student, Course, Sport)

| Student | Course | Sport |
|---|---|---|
| Ram | DBMS | Cricket |
| Ram | DBMS | Football |
| Ram | CN | Cricket |
| Ram | CN | Football |

**MVDs:** Student ↠ Course, Student ↠ Sport (courses and sports are independent)

**Why this is bad:** Adding a new sport for Ram requires adding a row for EVERY course Ram takes → massive redundancy!

**4NF violation:** Student is NOT a superkey, but Student ↠ Course is non-trivial MVD.

**4NF decomposition:**
- $R_1$(Student, Course): {(Ram, DBMS), (Ram, CN)}
- $R_2$(Student, Sport): {(Ram, Cricket), (Ram, Football)}

Redundancy eliminated! Adding a new sport = 1 row in $R_2$, not N rows.

> 🎯 **GATE fact about MVDs:**
> - Every FD $X \to Y$ implies $X \twoheadrightarrow Y$ (FD is a special case of MVD)
> - **Complementation rule:** $X \twoheadrightarrow Y$ ⟺ $X \twoheadrightarrow R - X - Y$
> - MVD $X \twoheadrightarrow Y$ is **trivial** if $Y \subseteq X$ or $X \cup Y = R$

---

### 🔥 GATE Tricks: Normalization

1. **Quick NF identification:** If ALL attributes are prime → at least 3NF (no partial/transitive deps on non-prime possible)
2. **BCNF vs 3NF:** Only difference is when RHS is a prime attribute — 3NF allows it, BCNF doesn't
3. **Single candidate key?** → 3NF = BCNF (the exception only arises with multiple overlapping CKs)
4. **CK with single attribute + no composite FDs on LHS?** → already in 2NF (no partial deps possible)
5. **Binary relation (2 attributes)?** → always in BCNF
6. **All attributes in a single CK?** → BCNF (every FD has superkey on LHS)
7. **Number of FDs:** From $n$ attributes, max non-trivial FDs = $n \times 2^n - n$ (but GATE usually gives specific FDs)
8. **Lossless test shortcut:** For binary decomposition into $R_1$ and $R_2$: check if $(R_1 \cap R_2)^+ \supseteq R_1$ or $(R_1 \cap R_2)^+ \supseteq R_2$

---

## Module 2: Relational Algebra (RA)

### 2.1 Fundamental Operations

| Operator | Symbol | Description | Output Degree |
|---|---|---|---|
| **Selection** | $\sigma_{\text{condition}}(R)$ | Select rows satisfying condition | Same as $R$ |
| **Projection** | $\pi_{A_1, A_2, \ldots}(R)$ | Select columns (removes duplicates) | Number of projected attrs |
| **Union** | $R \cup S$ | All tuples in $R$ or $S$ (no duplicates) | Same as $R$ (or $S$) |
| **Set Difference** | $R - S$ | Tuples in $R$ but not in $S$ | Same as $R$ |
| **Cartesian Product** | $R \times S$ | All combinations of tuples | $\text{deg}(R) + \text{deg}(S)$ |
| **Rename** | $\rho_{S(A_1,A_2,...)}(R)$ | Rename relation/attributes | Same as $R$ |

### 2.2 Derived Operations

| Operator | Symbol | Equivalent |
|---|---|---|
| **Intersection** | $R \cap S$ | $R - (R - S)$ |
| **Natural Join** | $R \bowtie S$ | $\pi_{\text{all}}(\sigma_{R.A=S.A}(R \times S))$ on common attributes |
| **Theta Join** | $R \bowtie_\theta S$ | $\sigma_\theta(R \times S)$ |
| **Equi-Join** | $R \bowtie_{R.A=S.B} S$ | Theta join where $\theta$ uses only $=$ |
| **Semi-Join** | $R \ltimes S$ | $\pi_{R.*}(R \bowtie S)$ |
| **Division** | $R \div S$ | See below |

### 2.3 Division Operation

$R(A, B) \div S(B) = \{t[A] \mid \forall s \in S, (t[A], s[B]) \in R\}$

Returns tuples in $\pi_A(R)$ that are associated with **every** tuple in $S$.

$$R \div S = \pi_A(R) - \pi_A((\pi_A(R) \times S) - R)$$

### 2.4 Outer Joins

| Type | Symbol | Keeps |
|---|---|---|
| **Left Outer Join** | $R \,⟕\, S$ | All tuples of $R$, NULL-pad unmatched from $S$ |
| **Right Outer Join** | $R \,⟖\, S$ | All tuples of $S$, NULL-pad unmatched from $R$ |
| **Full Outer Join** | $R \,⟗\, S$ | All tuples from both, NULL-pad unmatched |

### 2.5 RA Operator Cheat Sheet

| Query Type | RA Expression |
|---|---|
| "Find X such that condition" | $\pi_X(\sigma_{\text{cond}}(R))$ |
| "Find X related to all Y" | Division: $R \div S$ |
| "Find X not in Y" | $\pi_A(R) - \pi_A(S)$ or set difference |
| "Find X with max value" | $R - \pi_R(R \bowtie_{R.A < S.A} \rho_S(R))$ |
| "Count/Aggregate" | Use $\mathcal{G}$ (aggregate): ${}_G\mathcal{G}_{F(A)}(R)$ |

### 2.6 Cardinality Results

| Operation | Min Tuples | Max Tuples |
|---|---|---|
| $\sigma_c(R)$ | 0 | $\|R\|$ |
| $\pi_A(R)$ | 1 | $\|R\|$ |
| $R \cup S$ | $\max(\|R\|,\|S\|)$ | $\|R\| + \|S\|$ |
| $R \cap S$ | 0 | $\min(\|R\|,\|S\|)$ |
| $R - S$ | $\|R\| - \|S\|$ (or 0) | $\|R\|$ |
| $R \times S$ | $\|R\| \cdot \|S\|$ | $\|R\| \cdot \|S\|$ |
| $R \bowtie S$ | 0 | $\|R\| \cdot \|S\|$ |

> **For union/intersection/difference:** $R$ and $S$ must be **union-compatible** (same degree, corresponding domains compatible).

---

### 🧮 Worked Examples: Relational Algebra

**Setup — Sample Relations:**

**Employee(EmpID, Name, DeptID, Salary)**

| EmpID | Name | DeptID | Salary |
|---|---|---|---|
| 101 | Alice | D1 | 50000 |
| 102 | Bob | D2 | 60000 |
| 103 | Charlie | D1 | 45000 |
| 104 | Diana | D3 | 70000 |
| 105 | Eve | D2 | 55000 |

**Department(DeptID, DeptName, Location)**

| DeptID | DeptName | Location |
|---|---|---|
| D1 | CS | Delhi |
| D2 | IT | Mumbai |
| D3 | ECE | Chennai |
| D4 | EE | Kolkata |

**Enrollment(EmpID, CourseID)**

| EmpID | CourseID |
|---|---|
| 101 | C1 |
| 101 | C2 |
| 102 | C1 |
| 103 | C1 |
| 103 | C2 |
| 104 | C1 |

**Course(CourseID, CourseName)**

| CourseID | CourseName |
|---|---|
| C1 | DBMS |
| C2 | CN |

#### RA-WE1: Find names of employees in CS department

$$\pi_{\text{Name}}(\sigma_{\text{DeptName}='CS'}(Employee \bowtie Department))$$

**Step-by-step:**
1. $Employee \bowtie Department$ (natural join on DeptID):

| EmpID | Name | DeptID | Salary | DeptName | Location |
|---|---|---|---|---|---|
| 101 | Alice | D1 | 50000 | CS | Delhi |
| 102 | Bob | D2 | 60000 | IT | Mumbai |
| 103 | Charlie | D1 | 45000 | CS | Delhi |
| 104 | Diana | D3 | 70000 | ECE | Chennai |
| 105 | Eve | D2 | 55000 | IT | Mumbai |

2. $\sigma_{\text{DeptName}='CS'}$: Rows 1, 3
3. $\pi_{\text{Name}}$: **{Alice, Charlie}**

#### RA-WE2: Find employees earning more than every employee in D2

$$\pi_{\text{Name}}(\sigma_{\text{Salary} > \text{MaxD2}}(Employee))$$

Alternative using RA division/anti-join:
$$\pi_{\text{EmpID, Name, Salary}}(Employee) - \pi_{\text{EmpID, Name, Salary}}(Employee \bowtie_{\text{E.Salary} \leq \text{E2.Salary}} \sigma_{\text{DeptID}='D2'}(\rho_{E2}(Employee)))$$

Simpler: Use aggregate → find max salary in D2 = 60000, then select Salary > 60000. **Result: {Diana}**

#### RA-WE3: Find employees enrolled in ALL courses (Division!)

$$Enrollment \div Course_{\text{IDs}}$$

where $Course_{\text{IDs}} = \pi_{\text{CourseID}}(Course) = \{C1, C2\}$

**Division formula:** $\pi_{\text{EmpID}}(Enrollment) - \pi_{\text{EmpID}}((\pi_{\text{EmpID}}(Enrollment) \times Course_{\text{IDs}}) - Enrollment)$

**Step 1:** $\pi_{\text{EmpID}}(Enrollment) = \{101, 102, 103, 104\}$

**Step 2:** Cross product with $\{C1, C2\}$:
$\{(101,C1), (101,C2), (102,C1), (102,C2), (103,C1), (103,C2), (104,C1), (104,C2)\}$

**Step 3:** Subtract Enrollment:
$(102,C2), (104,C2)$ — these are the "missing" enrollments

**Step 4:** Project EmpID from step 3: $\{102, 104\}$

**Step 5:** $\{101, 102, 103, 104\} - \{102, 104\} = \{101, 103\}$

**Result:** EmpIDs **101 (Alice)** and **103 (Charlie)** are enrolled in ALL courses ✅

#### RA-WE4: Find departments with NO employees

$$\pi_{\text{DeptID}}(Department) - \pi_{\text{DeptID}}(Employee)$$

$= \{D1, D2, D3, D4\} - \{D1, D2, D3\} = \{D4\}$

Department D4 (EE, Kolkata) has no employees.

#### RA-WE5: Find the employee with highest salary (without aggregate)

**Idea:** Find employees who are NOT the maximum (there exists someone earning more), then subtract.

$$\pi_{\text{EmpID, Name}}(Employee) - \pi_{\text{E1.EmpID, E1.Name}}(Employee \bowtie_{\text{E1.Salary} < \text{E2.Salary}} \rho_{E2}(Employee))$$

**Step 1:** Self-join where E1.Salary < E2.Salary gives all employees who are NOT the max:
- Alice(50k) < Bob(60k), Alice < Diana(70k), Alice < Eve(55k) → Alice is not max
- Bob(60k) < Diana(70k) → Bob is not max
- Charlie(45k) < everyone except none with lower → Charlie is not max
- Eve(55k) < Bob(60k), Eve < Diana(70k) → Eve is not max
- Diana: no one has higher salary → Diana IS the max

**Step 2:** $\{101,102,103,105\}$ are not max. Subtract from all: $\{104\}$ → **Diana** ✅

#### RA-WE6: Cardinality Bounds Practice

**Q:** $|R| = 100$, $|S| = 50$, both union-compatible. What are the bounds for $|R \cup S|$?

- **Min:** If $S \subseteq R$, then $|R \cup S| = 100$
- **Max:** If $R \cap S = \emptyset$, then $|R \cup S| = 150$
- **Answer:** $100 \leq |R \cup S| \leq 150$

**Q:** $|R| = 100$, $|S| = 200$. Bounds for $|R \bowtie S|$ (natural join)?

- **Min:** 0 (no common values on join attributes)
- **Max:** $100 \times 200 = 20,000$ (every tuple in R matches every tuple in S)
- If join is on a **key of S**: max = 100, min = 0
- If join is on a **key of S** with **referential integrity** from R: exact = 100

> 🎯 **GATE Trick:** If the join attribute is a primary key of one table and foreign key of the other, the natural join gives exactly as many tuples as the referencing table.

---

### 🔥 GATE Tricks: Relational Algebra

1. **Division** = "for all" queries → if question says "all", "every", think division
2. **Finding max/min without aggregates:** Self-join anti-pattern (described above)
3. **NOT EXISTS in RA = Set Difference:** $\pi_A(R) - \pi_A(S)$ = "in R but not in S"
4. **Natural Join on no common attributes = Cartesian Product**
5. **Selection then Projection ≠ Projection then Selection** (selection may use attributes dropped by projection)
6. **Projection removes duplicates** (RA uses set semantics, SQL uses bag semantics by default)
7. **Semi-join trick:** $R \ltimes S = \pi_{R.*}(R \bowtie S)$ — useful for reducing data transfer in distributed DBs

---

## Module 3: Tuple Relational Calculus (TRC)

### 3.1 Syntax

A TRC query has the form:

$$\{t \mid P(t)\}$$

where $t$ is a tuple variable and $P$ is a formula (predicate).

### 3.2 Atoms

- $t \in R$ — tuple $t$ belongs to relation $R$
- $t[A] \;\theta\; s[B]$ — comparison between attributes ($\theta \in \{=, \neq, <, >, \leq, \geq\}$)
- $t[A] \;\theta\; c$ — comparison with constant $c$

### 3.3 Formulas

Built using:
- Atoms
- $\land$ (AND), $\lor$ (OR), $\neg$ (NOT)
- $\exists t \in R (P(t))$ — existential quantifier
- $\forall t \in R (P(t))$ — universal quantifier

**Important equivalence:** $\forall t \in R(P(t)) \equiv \neg \exists t \in R(\neg P(t))$

### 3.4 Safe Expressions

A TRC expression is **safe** if it guarantees a finite result. The domain of the expression must be limited to values appearing in the database (active domain).

### 3.5 Common TRC Patterns

| Query | TRC Expression |
|---|---|
| Select all from R where cond | $\{t \mid t \in R \land \text{cond}(t)\}$ |
| Projection on A,B | $\{t \mid \exists s \in R(t[A]=s[A] \land t[B]=s[B])\}$ |
| Join R and S | $\{t \mid \exists r \in R \exists s \in S(r[A]=s[A] \land \ldots)\}$ |
| "For ALL" queries | $\{t \mid t \in R \land \forall s \in S(\text{condition})\}$ |

> **GATE TRC tip**: GATE often asks to interpret a given TRC expression. Convert quantifiers step by step and match to English.

### Domain Relational Calculus (DRC)

**Syntax:** $\{\langle x_1, x_2, \ldots, x_n \rangle \mid P(x_1, x_2, \ldots, x_n)\}$

Variables range over **domain values** (individual attributes), not entire tuples.

**Example:** Find names of students with GPA > 8:
$$\{\langle n \rangle \mid \exists r, \exists d, \exists g (\text{Student}(r, n, d, g) \wedge g > 8)\}$$

**Key Differences: TRC vs DRC**

| | TRC | DRC |
|--|-----|-----|
| Variables | Tuple variables | Domain variables |
| Binding | $t \in R$ | $R(x_1, x_2, \ldots)$ |
| Access | $t[A]$ or $t.A$ | Direct variable |
| Expressiveness | Equivalent | Equivalent |

**Safe expressions:** DRC expressions must also be safe (finite result guaranteed).

**Equivalence:** RA = TRC (safe) = DRC (safe) = SQL (core)

---

## Module 4: SQL

### 4.1 DDL (Data Definition Language)

```sql
CREATE TABLE Students (
    roll_no INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    dept_id INT,
    gpa DECIMAL(3,2) CHECK (gpa >= 0 AND gpa <= 10),
    FOREIGN KEY (dept_id) REFERENCES Department(dept_id)
        ON DELETE SET NULL ON UPDATE CASCADE
);

ALTER TABLE Students ADD email VARCHAR(100);
DROP TABLE Students;
```

### 4.2 DML Core — SELECT Statement

```sql
SELECT [DISTINCT] column_list
FROM table_list
[WHERE condition]
[GROUP BY column_list]
[HAVING condition]
[ORDER BY column_list [ASC|DESC]];
```

**Execution Order:** FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY

### 4.3 Aggregate Functions

| Function | Description | Ignores NULL? |
|---|---|---|
| `COUNT(*)` | Count all rows | No |
| `COUNT(col)` | Count non-NULL values | Yes |
| `SUM(col)` | Sum of values | Yes |
| `AVG(col)` | Average | Yes |
| `MAX(col)` | Maximum | Yes |
| `MIN(col)` | Minimum | Yes |

> **GATE trap:** `COUNT(*)` counts all rows including rows with NULLs. `COUNT(col)` ignores NULLs.

### 4.4 NULL Handling in SQL

- `WHERE col = NULL` → **wrong** (always UNKNOWN)
- `WHERE col IS NULL` → **correct**
- `WHERE col IS NOT NULL` → **correct**
- Aggregate functions (except `COUNT(*)`) ignore NULL values
- `GROUP BY` treats all NULLs as a single group

### 4.5 GROUP BY and HAVING

```sql
-- Find departments with average GPA > 8
SELECT dept_id, AVG(gpa) AS avg_gpa
FROM Students
WHERE gpa IS NOT NULL
GROUP BY dept_id
HAVING AVG(gpa) > 8;
```

**Rules:**
- Every column in SELECT (that is not an aggregate) must appear in GROUP BY
- WHERE filters **before** grouping; HAVING filters **after** grouping
- Cannot use aliases defined in SELECT within WHERE/HAVING (implementation-dependent)

### 4.6 Nested Queries

```sql
-- Students with GPA above average
SELECT name FROM Students
WHERE gpa > (SELECT AVG(gpa) FROM Students);

-- Using IN
SELECT name FROM Students
WHERE dept_id IN (SELECT dept_id FROM Department WHERE location = 'Delhi');
```

### 4.7 Correlated Nested Queries

The inner query references the outer query — executed **once per outer row**.

```sql
-- Students who have the highest GPA in their department
SELECT s1.name, s1.gpa
FROM Students s1
WHERE s1.gpa = (
    SELECT MAX(s2.gpa)
    FROM Students s2
    WHERE s2.dept_id = s1.dept_id  -- correlation
);
```

### 4.8 EXISTS and NOT EXISTS

```sql
-- Departments that have at least one student
SELECT dept_name FROM Department d
WHERE EXISTS (SELECT 1 FROM Students s WHERE s.dept_id = d.dept_id);

-- Division equivalent: Students enrolled in ALL courses
SELECT s.name FROM Students s
WHERE NOT EXISTS (
    SELECT c.course_id FROM Courses c
    WHERE NOT EXISTS (
        SELECT 1 FROM Enrollment e
        WHERE e.student_id = s.student_id AND e.course_id = c.course_id
    )
);
```

### 4.9 Set Operations in SQL

```sql
SELECT col FROM R UNION SELECT col FROM S;       -- removes duplicates
SELECT col FROM R UNION ALL SELECT col FROM S;    -- keeps duplicates
SELECT col FROM R INTERSECT SELECT col FROM S;
SELECT col FROM R EXCEPT SELECT col FROM S;       -- R - S
```

### 4.10 SQL vs RA Equivalence

| SQL | RA |
|---|---|
| `SELECT DISTINCT A FROM R WHERE cond` | $\pi_A(\sigma_{\text{cond}}(R))$ |
| `SELECT * FROM R, S WHERE R.A = S.A` | $R \bowtie_{R.A=S.A} S$ |
| `R UNION S` | $R \cup S$ |
| `R EXCEPT S` | $R - S$ |

### 4.11 Views

```sql
CREATE VIEW high_gpa AS
    SELECT name, gpa FROM Students WHERE gpa > 8.0;

-- Views are virtual tables (query stored, not data)
-- Updatable if: single table, no aggregates, no GROUP BY, no DISTINCT
DROP VIEW high_gpa;
```

### 4.12 Triggers

```sql
CREATE TRIGGER check_gpa
BEFORE INSERT ON Students
FOR EACH ROW
BEGIN
    IF NEW.gpa < 0 THEN SET NEW.gpa = 0; END IF;
END;
```

- **BEFORE/AFTER** triggers
- **INSERT/UPDATE/DELETE** events
- **FOR EACH ROW** (row-level) or **FOR EACH STATEMENT** (statement-level)
- Access NEW and OLD row values

### 4.13 Assertions

```sql
CREATE ASSERTION min_gpa
    CHECK (NOT EXISTS (SELECT * FROM Students WHERE gpa < 0));
```

- Global constraints checked after every modification
- Very expensive → rarely supported in practice

---

### 🧮 Worked Examples: SQL

**Using the same Employee, Department, Enrollment, Course tables from RA section.**

#### SQL-WE1: Tricky GROUP BY with HAVING

**Q:** Find departments where the average salary is above the company-wide average salary.

```sql
SELECT d.DeptName, AVG(e.Salary) AS AvgSalary
FROM Employee e JOIN Department d ON e.DeptID = d.DeptID
GROUP BY d.DeptName
HAVING AVG(e.Salary) > (SELECT AVG(Salary) FROM Employee);
```

**Trace:**
- Company-wide avg = (50000+60000+45000+70000+55000)/5 = 280000/5 = **56000**
- CS avg = (50000+45000)/2 = 47500 → NOT > 56000 ❌
- IT avg = (60000+55000)/2 = 57500 → > 56000 ✅
- ECE avg = 70000/1 = 70000 → > 56000 ✅
- **Result:** IT (57500), ECE (70000)

#### SQL-WE2: Division using NOT EXISTS (Employees enrolled in ALL courses)

```sql
SELECT e.Name
FROM Employee e
WHERE NOT EXISTS (
    SELECT c.CourseID
    FROM Course c
    WHERE NOT EXISTS (
        SELECT 1
        FROM Enrollment en
        WHERE en.EmpID = e.EmpID AND en.CourseID = c.CourseID
    )
);
```

**Reading this query:** "Find employees for whom there does NOT EXIST a course that they are NOT enrolled in" = "enrolled in ALL courses"

**Result:** Alice (101) and Charlie (103) ✅ — same as RA division!

> 🎯 **GATE Pattern:** Division = double NOT EXISTS. Inner NOT EXISTS = "not enrolled in this course". Outer NOT EXISTS = "no such course exists" = "enrolled in all".

#### SQL-WE3: NULL Traps

**Q:** Table T has values {1, 2, NULL, 4, NULL}. What does each query return?

```sql
SELECT COUNT(*) FROM T;        -- 5 (counts ALL rows including NULLs)
SELECT COUNT(val) FROM T;      -- 3 (counts non-NULL values only)
SELECT SUM(val) FROM T;        -- 7 (1+2+4, ignores NULLs)
SELECT AVG(val) FROM T;        -- 7/3 = 2.33 (NOT 7/5!)
SELECT val FROM T WHERE val != 2; -- Returns {1, 4} (NULLs not returned!)
SELECT val FROM T WHERE val = NULL; -- Returns NOTHING (always UNKNOWN)
SELECT val FROM T WHERE val IS NULL; -- Returns {NULL, NULL}
```

> ⚠️ **GATE Trap #1:** `AVG` ignores NULLs in BOTH numerator and denominator!
> ⚠️ **GATE Trap #2:** `WHERE val != 2` does NOT return NULL rows (UNKNOWN is not TRUE)
> ⚠️ **GATE Trap #3:** `val = NULL` never works — must use `IS NULL`

#### SQL-WE4: Correlated vs Non-Correlated Subquery

**Non-correlated** (independent inner query):
```sql
SELECT Name FROM Employee
WHERE Salary > (SELECT AVG(Salary) FROM Employee);
-- Inner query runs ONCE, returns 56000
-- Result: Bob(60000), Diana(70000)
```

**Correlated** (inner references outer):
```sql
SELECT e1.Name FROM Employee e1
WHERE e1.Salary > (
    SELECT AVG(e2.Salary) FROM Employee e2
    WHERE e2.DeptID = e1.DeptID  -- correlation!
);
-- Inner query runs ONCE PER OUTER ROW
-- Alice: 50000 > avg(D1)=47500 → YES ✅
-- Bob: 60000 > avg(D2)=57500 → YES ✅
-- Charlie: 45000 > avg(D1)=47500 → NO ❌
-- Diana: 70000 > avg(D3)=70000 → NO ❌ (not strictly greater)
-- Eve: 55000 > avg(D2)=57500 → NO ❌
-- Result: Alice, Bob
```

#### SQL-WE5: UNION vs UNION ALL

```sql
SELECT DeptID FROM Employee      -- {D1, D2, D1, D3, D2}
UNION
SELECT DeptID FROM Department;   -- {D1, D2, D3, D4}
-- Result (no duplicates): {D1, D2, D3, D4}

SELECT DeptID FROM Employee      -- {D1, D2, D1, D3, D2}
UNION ALL
SELECT DeptID FROM Department;   -- {D1, D2, D3, D4}
-- Result (all 9 rows): {D1, D2, D1, D3, D2, D1, D2, D3, D4}
```

#### SQL-WE6: Tricky ORDER BY with LIMIT

**Q:** Find the 2nd highest salary.

```sql
-- Method 1: Subquery
SELECT MAX(Salary) FROM Employee
WHERE Salary < (SELECT MAX(Salary) FROM Employee);
-- Inner: 70000. Outer: MAX where < 70000 = 60000 ✅

-- Method 2: LIMIT/OFFSET
SELECT DISTINCT Salary FROM Employee
ORDER BY Salary DESC
LIMIT 1 OFFSET 1;
-- Sorted: 70000, 60000, 55000, 50000, 45000
-- Skip 1, take 1: 60000 ✅

-- Method 3: Dense Rank (window function)
SELECT Salary FROM (
    SELECT Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) AS rnk
    FROM Employee
) t WHERE rnk = 2;
```

#### SQL-WE7: Self Join — Find Employees in Same Department

```sql
SELECT e1.Name AS Emp1, e2.Name AS Emp2
FROM Employee e1 JOIN Employee e2 
ON e1.DeptID = e2.DeptID AND e1.EmpID < e2.EmpID;
```

Result: (Alice, Charlie) — both in D1; (Bob, Eve) — both in D2

> `e1.EmpID < e2.EmpID` avoids duplicates like (Alice,Alice) and (Charlie,Alice)

#### SQL-WE8: Views — Updatability Rules

```sql
-- This view IS updatable (single table, no aggregates/GROUP BY/DISTINCT):
CREATE VIEW cs_employees AS
    SELECT EmpID, Name, Salary FROM Employee WHERE DeptID = 'D1';

-- This view is NOT updatable (aggregate + GROUP BY):
CREATE VIEW dept_stats AS
    SELECT DeptID, COUNT(*) AS cnt, AVG(Salary) AS avg_sal
    FROM Employee GROUP BY DeptID;
```

**View Updatability Conditions:**
- Single base table
- No aggregates, DISTINCT, GROUP BY, HAVING
- No subqueries in WHERE referencing same table
- No set operations (UNION, etc.)
- All NOT NULL columns of base table present (for INSERT)

---

### 🔥 GATE SQL Tricks

1. **Execution order:** FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY (not the written order!)
2. **HAVING vs WHERE:** WHERE filters rows, HAVING filters groups. Can't use aggregates in WHERE.
3. **COUNT(*) vs COUNT(col):** * counts all rows; col ignores NULLs
4. **ALL vs ANY:** `Salary > ALL(subquery)` = greater than maximum; `Salary > ANY(subquery)` = greater than minimum
5. **IN vs EXISTS:** IN materializes subquery result; EXISTS short-circuits on first match (EXISTS often faster for large subqueries)
6. **DELETE vs TRUNCATE:** DELETE is DML (can rollback, fires triggers); TRUNCATE is DDL (can't rollback, no triggers, faster)
7. **EXCEPT = set difference:** `R EXCEPT S` = tuples in R but not in S
8. **Natural Join gotcha:** Joins on ALL common column names — may join on unintended columns!

---

### 📊 SQL vs RA vs TRC Quick Conversion Table

| English | SQL | RA | TRC |
|---|---|---|---|
| All CS employees | `WHERE dept='CS'` | $\sigma_{\text{dept}='CS'}(E)$ | $\{t \mid t \in E \land t[\text{dept}]='CS'\}$ |
| Employee names | `SELECT DISTINCT Name` | $\pi_{\text{Name}}(E)$ | $\{t \mid \exists s \in E(t[1]=s[\text{Name}])\}$ |
| In ALL courses | `NOT EXISTS(NOT EXISTS)` | $R \div S$ | $\forall c \in C(\exists e \in Enrl(...))$ |
| Not in any | `NOT IN` or `NOT EXISTS` | $R - S$ | $\neg \exists$ |
| Max salary | `SELECT MAX(Salary)` | Anti-join approach | $\neg \exists s(s[\text{Sal}] > t[\text{Sal}])$ |

---

## Module 4: SQL

### 5.1 File Organization Types

| Type | Description | Search Cost |
|---|---|---|
| **Heap (Unordered)** | Records placed anywhere with space | $O(n)$ linear |
| **Sequential (Ordered)** | Records sorted on a search key | $O(\log n)$ binary search |
| **Hashed** | Hash function determines block | $O(1)$ average |

### 5.2 Index Types

| Index Type | Ordering Key? | Unique Key? | Records Sorted? | Dense/Sparse |
|---|---|---|---|---|
| **Primary** | Yes | Yes | Yes (on indexing key) | Sparse (1 entry per block) |
| **Clustering** | Yes | No | Yes (on indexing key) | Sparse (1 entry per distinct value) |
| **Secondary** | No | — | No | Dense (1 entry per record, or per unique value with bucket pointers) |

### 5.3 Key Formulas for Indexing

**Number of index entries:**
- Primary index: $\lceil n_r / f_r \rceil$ entries (= number of data blocks)
  - where $n_r$ = number of records, $f_r$ = blocking factor (records per block)
- Clustering index: number of distinct values of the indexing attribute
- Secondary (dense): $n_r$ entries

**Blocking factor:** $f_r = \lfloor B / R \rfloor$, where $B$ = block size, $R$ = record size

**Index blocking factor:** $f_i = \lfloor B / (V_i + P) \rfloor$, where $V_i$ = index field size, $P$ = pointer size

**Number of index blocks:** $b_i = \lceil \text{index entries} / f_i \rceil$

**Levels of multilevel index:** $\lceil \log_{f_i}(b_1) \rceil + 1$ where $b_1$ = first-level index blocks

**Search cost (multilevel index):** Number of levels + 1 (for data block access)

### 5.4 ISAM (Indexed Sequential Access Method)

- Static multilevel index structure
- Overflow blocks used for insertions
- Good for static data, degrades with many insertions
- Does NOT reorganize itself (unlike B/B+ trees)

### 5.5 B-Trees

**Properties of a B-tree of order $m$ (or $p$):**
- Each node has at most $m$ children (or $p$ pointers)
- Each node (except root) has at least $\lceil m/2 \rceil$ children
- Root has at least 2 children (if not a leaf)
- All leaves are at the same level
- A node with $k$ children contains $k-1$ keys
- Data pointers exist at EVERY node (internal + leaf)

**B-Tree Formulas (order $p$):**

| Property | Formula |
|---|---|
| Max keys per node | $p - 1$ |
| Min keys (non-root internal) | $\lceil p/2 \rceil - 1$ |
| Min keys (root) | 1 |
| Max keys at level $h$ | $p^h - 1$ |
| Min height for $n$ keys | $\lceil \log_p(n+1) \rceil$ |
| Max height for $n$ keys | $\lfloor \log_{\lceil p/2 \rceil}((n+1)/2) \rfloor + 1$ |

**Finding order $p$ of B-tree:**

Given: key size $V$, data pointer $P_d$, block pointer $P_b$, block size $B$

$$(p-1)(V + P_d) + p \cdot P_b \leq B$$

$$p = \left\lfloor \frac{B + V + P_d}{V + P_d + P_b} \right\rfloor$$

### 5.6 B+ Trees

**Key Differences from B-Tree:**
- Data pointers **only at leaf** nodes
- Internal nodes have only keys and child pointers (→ higher fan-out)
- Leaf nodes are linked (linked list) for range queries
- All keys appear at the leaf level (some duplicated in internal nodes)

**B+ Tree Formulas:**

**Internal node order $p$:**

$$(p-1) \cdot V + p \cdot P_b \leq B$$

$$p = \left\lfloor \frac{B + V}{V + P_b} \right\rfloor$$

**Leaf node order $p_l$:**

$$(p_l)(V + P_d) + P_b \leq B$$

$$p_l = \left\lfloor \frac{B - P_b}{V + P_d} \right\rfloor$$

(The extra $P_b$ in leaf is for the next-leaf pointer)

| Property | Internal Node | Leaf Node |
|---|---|---|
| Max pointers | $p$ | $p_l$ data pointers + 1 next pointer |
| Min pointers (non-root) | $\lceil p/2 \rceil$ | $\lceil p_l/2 \rceil$ |
| Max keys | $p - 1$ | $p_l$ |
| Min keys (non-root) | $\lceil p/2 \rceil - 1$ | $\lceil p_l/2 \rceil$ |

**Number of leaf nodes needed for $n$ records:**

$$\text{Leaf nodes} = \lceil n / p_l \rceil \quad (\text{worst case: } \lceil n / \lceil p_l/2 \rceil \rceil)$$

### 5.7 B-Tree vs B+ Tree Comparison

| Feature | B-Tree | B+ Tree |
|---|---|---|
| Data pointers | At all nodes | Only at leaves |
| Duplicate keys | No | Yes (internal copies) |
| Range queries | Inefficient | Efficient (linked leaves) |
| Fan-out | Lower | Higher (more keys per node) |
| Search | May end at any level | Always goes to leaf |
| Height | Generally taller | Generally shorter |

### 5.8 Hashing ⭐

#### Static Hashing
- Hash function $h(K)$ maps key $K$ to bucket address
- **Collision:** Two keys hash to same bucket
- Overflow handled by: chaining (linked lists), open addressing, or overflow buckets
- **Problem:** Fixed number of buckets → performance degrades as data grows

#### Extendible Hashing
- Uses a **directory** of $2^d$ entries (d = global depth)
- Each bucket has a **local depth** $d'$
- When bucket overflows:
  1. If local depth < global depth: split bucket, redistribute
  2. If local depth = global depth: **double the directory** (d++), then split
- No overflow chains
- **Directory size:** $2^d$ pointers
- At most ONE disk access for lookup (plus directory access if not in memory)

**Key formula:** After splitting, the two new buckets use the $(d'+1)$-th bit to distinguish entries.

#### Linear Hashing
- No directory; uses split pointer $n$
- Buckets 0 to $N-1$. Split pointer starts at 0.
- Two hash functions: $h_0(K) = K \mod N$, $h_1(K) = K \mod 2N$
- If $h_0(K) < n$, use $h_1(K)$; else use $h_0(K)$
- When load factor exceeds threshold: split bucket at pointer $n$, increment $n$
- When $n = N$: reset $n = 0$, $N = 2N$
- May have temporary overflow chains for unsplit buckets

### 5.9 Index Types Summary

| Index Type | Key = Ordering Key? | One per table? | Entries |
|-----------|---------------------|---------------|--------|
| Primary | Yes, on primary key | Yes (max 1) | One per block (sparse possible) |
| Clustering | Yes, on non-key ordering | Yes (max 1) | One per distinct value |
| Secondary | No (any attribute) | Multiple OK | Must be dense |

---

### 🧮 Worked Examples: Indexing & B/B+ Trees

#### IDX-WE1: Complete Indexing Calculation

**Given:**
- File: 300,000 records
- Record size: 100 bytes
- Block size: 4096 bytes
- Key field: 10 bytes
- Block pointer: 6 bytes
- Data pointer: 6 bytes

**Step 1: Blocking factor & data blocks**
$$f_r = \lfloor 4096/100 \rfloor = 40 \text{ records/block}$$
$$b = \lceil 300000/40 \rceil = 7500 \text{ data blocks}$$

**Step 2: Primary Index (sparse — one entry per block)**
- Index entries = 7500
- Index entry size = key + pointer = 10 + 6 = 16 bytes
- Index BF: $f_i = \lfloor 4096/16 \rfloor = 256$ entries/block
- Index blocks: $\lceil 7500/256 \rceil = 30$ blocks
- **Binary search on index:** $\lceil \log_2(30) \rceil = 5$ block accesses + 1 data block = **6 total**
- **Without index (binary search on file):** $\lceil \log_2(7500) \rceil = 13$ block accesses

**Step 3: Secondary Index (dense — one entry per record)**
- Index entries = 300,000
- Index blocks: $\lceil 300000/256 \rceil = 1172$ blocks
- **Binary search:** $\lceil \log_2(1172) \rceil = 11$ + 1 = **12 total**

**Step 4: Multilevel Primary Index**
- Level 1: 30 blocks
- Level 2: $\lceil 30/256 \rceil = 1$ block
- Total levels = 2
- **Access cost:** 2 + 1 = **3 block accesses** (best!)

> 🎯 **Comparison Table:**

| Method | Block Accesses |
|---|---|
| Linear scan | 7500 (worst case) |
| Binary search (file) | 13 |
| Binary search (primary index) | 6 |
| Multilevel primary index | 3 |
| B+ tree (see below) | ~3 |

#### IDX-WE2: B-Tree Order Calculation

**Given:** Block size = 512 bytes, Key = 8 bytes, Data pointer = 8 bytes, Block pointer = 4 bytes

**B-tree node:** $(p-1)(V + P_d) + p \cdot P_b \leq B$

$$(p-1)(8+8) + 4p \leq 512$$
$$16p - 16 + 4p \leq 512$$
$$20p \leq 528$$
$$p = \lfloor 528/20 \rfloor = \lfloor 26.4 \rfloor = 26$$

**B-tree of order 26:**
- Max keys per node = 25
- Min keys (non-root) = $\lceil 26/2 \rceil - 1 = 12$
- Max entries at height $h$: $26^h - 1$

#### IDX-WE3: B+ Tree — Complete Calculation

**Given:** Block size = 4096 bytes, Key = 10 bytes, Data pointer = 8 bytes, Block pointer = 6 bytes, Records = 1,000,000

**Internal node order $p$:**
$$(p-1) \times 10 + p \times 6 \leq 4096$$
$$16p \leq 4106$$
$$p = \lfloor 4106/16 \rfloor = 256$$

**Leaf node order $p_l$:**
$$p_l \times (10 + 8) + 6 \leq 4096$$
$$p_l = \lfloor 4090/18 \rfloor = 227$$

**Leaf nodes needed:** $\lceil 1000000/227 \rceil = 4406$

**Height calculation (best case):**
- Level 0 (leaves): 4406 nodes
- Level 1: $\lceil 4406/256 \rceil = 18$ internal nodes
- Level 2: $\lceil 18/256 \rceil = 1$ (root)
- **Height = 3** → search cost = **4 block accesses** (3 index + 1 data)

> 🎯 **Why B+ beats B-tree:** No data pointers in internal nodes → higher fan-out (256 vs 26) → much shorter tree!

#### IDX-WE4: B+ Tree Insertion Trace (Order 3)

**Max 2 keys in internal, max 2 keys in leaf. Insert: 5, 8, 1, 7, 3, 12**

```
After 5,8:     [5|8]  (root = leaf)

Insert 1 → [1|5|8] OVERFLOW → Split leaf, copy up middle(5):
                 [5]
                /   \
            [1]   [5|8]

Insert 7 → goes to [5|8] → [5|7|8] OVERFLOW → Split, copy up 7:
                [5|7]
               / | \
           [1] [5] [7|8]
           
Wait... let me be precise. Leaf [5,8] gets 7 → [5,7,8] overflow.
Split into [5] and [7,8]. Copy up first key of right = 7.
Parent was [5], becomes [5,7]:
                [5|7]
               / | \
           [1] [5] [7|8]

Insert 3 → goes to [1] → [1|3] OK ✅

Insert 12 → goes to [7|8] → [7|8|12] OVERFLOW → Split into [7] and [8|12], copy up 8.
Parent [5|7] becomes [5|7|8] OVERFLOW → Split internal! Push up 7.

                    [7]
                   /    \
               [5]      [8]
              / \       /  \
          [1|3] [5] [7] [8|12]
```

> 🎯 **B+ insertion rules:** Leaf split → COPY middle up | Internal split → PUSH middle up (move, not copy)

#### IDX-WE5: Extendible Hashing

**Insert: 2, 5, 7, 14, 22, 12. Bucket size = 2. Hash = key mod 8 (use last 3 bits)**

| Key | Binary | Last 3 bits |
|---|---|---|
| 2 | 010 | 010 |
| 5 | 101 | 101 |
| 7 | 111 | 111 |
| 14 | 1110 | 110 |
| 22 | 10110 | 110 |
| 12 | 1100 | 100 |

**Start: d=1 (2 buckets)**

```
d=1
[0] → Bucket A [2] (ld=1)       ← even last bit
[1] → Bucket B [5, 7] (ld=1)    ← odd last bit

Insert 14 (110, last bit=0) → Bucket A [2, 14] ✅

Insert 22 (110, last bit=0) → Bucket A [2, 14, 22] OVERFLOW!
ld=1 = d=1 → DOUBLE directory to d=2

d=2
[00] → Bucket A1 [?]       After redistribution using last 2 bits:
[01] → Bucket C  [?]       2=10→10, 14=10→10, 22=10→10 — all go to same bucket!
[10] → Bucket A2 [?]       Need to increase depth again for bucket 10...
[11] → Bucket B  [5,7]     

Actually: ld becomes 2. Split bucket A on 2nd-to-last bit:
2=010(last 2=10), 14=110(last 2=10), 22=110(last 2=10) → ALL have last 2 bits = 10!
Bucket still overflows → set ld=3, double directory to d=3...

This shows extendible hashing can require multiple doublings for skewed data.
```

> 🎯 **GATE fact:** Extendible hashing worst case: all keys hash to same bucket → directory keeps doubling.

---

### 🔥 GATE Tricks: Indexing

1. **Primary: sparse possible.** Secondary: ALWAYS dense.
2. **B+ height** ≈ $\lceil \log_{\lceil p/2 \rceil}(\text{leaf nodes}) \rceil + 1$
3. **B-tree can find key at internal level** (shorter search sometimes). B+ always goes to leaf.
4. **One primary/clustering index per table** (file sorted on only one attribute). Multiple secondary OK.
5. **Composite index (A,B):** helps query on A, or (A,B). Does NOT help query on B alone.
6. **Hash index:** Best for equality searches. Terrible for range queries.
7. **B+ tree:** Good for both equality AND range queries.

---

## Module 6B: Query Processing & Optimization ⭐

### Query Processing Steps
1. **Parsing & Translation:** SQL → RA expression
2. **Optimization:** Choose efficient evaluation plan (rewrite + cost estimation)
3. **Evaluation:** Execute the plan

### Join Algorithms & Cost

Let $b_r, b_s$ = blocks, $n_r, n_s$ = tuples, $M$ = memory buffers.

| Algorithm | Block Transfers | When Best |
|-----------|----------------|----------|
| **Nested Loop (tuple)** | $n_r \times b_s + b_r$ | Small outer relation |
| **Block Nested Loop** | $\lceil b_r/M \rceil \times b_s + b_r$ | $M$ large, small outer |
| **Indexed Nested Loop** | $b_r + n_r \times c$ ($c$ = index lookup cost) | Index on inner join attr |
| **Sort-Merge Join** | $b_r + b_s + \text{sort cost}$ | Both sortable, large |
| **Hash Join** | $3(b_r + b_s)$ | Sufficient memory for partitioning |

**Sort-Merge cost:** Sort each relation: $2b \cdot \lceil\log_{M-1}(\lceil b/M\rceil)\rceil$ block transfers per relation.

### Equivalence Rules for Query Optimization

1. **Selection cascade:** $\sigma_{c_1 \wedge c_2}(R) = \sigma_{c_1}(\sigma_{c_2}(R))$
2. **Selection commutative:** $\sigma_{c_1}(\sigma_{c_2}(R)) = \sigma_{c_2}(\sigma_{c_1}(R))$
3. **Projection cascade:** Only outermost matters: $\pi_{L_1}(\pi_{L_2}(R)) = \pi_{L_1}(R)$ if $L_1 \subseteq L_2$
4. **Selection-Join:** $\sigma_c(R \bowtie S) = \sigma_c(R) \bowtie S$ if $c$ uses only $R$'s attributes
5. **Join commutative:** $R \bowtie S = S \bowtie R$
6. **Join associative:** $(R \bowtie S) \bowtie T = R \bowtie (S \bowtie T)$
7. **Push selection before join** (most important optimization!)
8. **Push projection before join** (reduce tuple width early)

### Optimization Heuristics
- **Apply selections as early as possible** (reduces relation size)
- **Apply projections early** (reduce width)
- **Perform most restrictive joins first**
- **Avoid Cartesian products** (rewrite as joins)

---

## Module 7: Transactions & Concurrency Control

### 7.1 Transaction & ACID Properties

A **transaction** is a logical unit of work: a sequence of read/write operations.

| Property | Meaning | Ensured By |
|---|---|---|
| **Atomicity** | All or nothing — either all operations execute or none | Recovery system (undo/redo) |
| **Consistency** | DB moves from one consistent state to another | Application + integrity constraints |
| **Isolation** | Concurrent transactions don't interfere | Concurrency control |
| **Durability** | Once committed, changes persist despite failures | Recovery system (write-ahead logging) |

### 7.2 Transaction States

```
Active → Partially Committed → Committed
  ↓                ↓
Failed          Failed
  ↓                ↓
  └──→ Aborted ←───┘
```

### 7.3 Schedules

A **schedule** is an ordering of operations from concurrent transactions.

- **Serial schedule**: Transactions execute one after another (no interleaving)
- **Serializable schedule**: Equivalent to some serial schedule

### 7.4 Conflict Serializability

**Conflicting operations:** Two operations conflict if:
1. They belong to **different** transactions
2. They access the **same** data item
3. At least one is a **write**

| Op1 | Op2 | Conflict? |
|---|---|---|
| R(X) | R(X) | No |
| R(X) | W(X) | **Yes** |
| W(X) | R(X) | **Yes** |
| W(X) | W(X) | **Yes** |

**Conflict Equivalence:** Two schedules are conflict equivalent if one can be obtained from the other by swapping non-conflicting adjacent operations.

**Testing Conflict Serializability — Precedence Graph:**
1. Create a node for each transaction
2. Add directed edge $T_i \to T_j$ if there's a conflicting pair where $T_i$'s operation comes before $T_j$'s
3. **If the graph is acyclic → Conflict Serializable**
4. Topological sort gives the equivalent serial order

### 7.5 View Serializability

Schedule $S$ is view serializable if it is view equivalent to some serial schedule.

**View Equivalence conditions:** For every data item $X$:
1. **Initial read**: If $T_i$ reads the initial value of $X$ in $S$, then $T_i$ must also read the initial value of $X$ in $S'$
2. **Read-from**: If $T_i$ reads value written by $T_j$ in $S$, then $T_i$ must read value written by $T_j$ in $S'$
3. **Final write**: If $T_i$ performs the final write on $X$ in $S$, then $T_i$ must also do the final write on $X$ in $S'$

> **Key relationship:**
> - Conflict Serializable $\subset$ View Serializable $\subset$ Serializable
> - Every conflict serializable schedule is view serializable, but NOT vice versa
> - **Blind writes** can make a schedule view serializable but not conflict serializable

### 7.6 Recoverability

| Schedule Type | Condition | Cascade? |
|---|---|---|
| **Recoverable** | $T_j$ reads from $T_i$ → $T_i$ commits before $T_j$ commits | May cascade |
| **Cascadeless (ACA)** | $T_j$ reads from $T_i$ → $T_i$ commits before $T_j$ reads | No cascading aborts |
| **Strict** | $T_j$ reads/writes item written by $T_i$ → $T_i$ commits/aborts first | No cascading, simple undo |

**Hierarchy:** Strict $\subset$ Cascadeless $\subset$ Recoverable

> **Irrecoverable schedule**: $T_j$ reads from $T_i$ and $T_j$ commits before $T_i$. If $T_i$ later aborts, $T_j$ cannot be undone — irrecoverable!

### 7.7 Lock-Based Concurrency Control

**Lock Types:**
- **Shared Lock (S)** — for reading. Multiple transactions can hold S locks on same item
- **Exclusive Lock (X)** — for writing. Only one transaction can hold X lock

**Lock Compatibility Matrix:**

|  | S | X |
|---|---|---|
| **S** | ✓ (Compatible) | ✗ |
| **X** | ✗ | ✗ |

### 7.8 Two-Phase Locking (2PL)

**Rule:** A transaction has two phases:
1. **Growing phase**: May acquire locks, may NOT release any
2. **Shrinking phase**: May release locks, may NOT acquire any

**2PL guarantees conflict serializability** but may have deadlocks.

| 2PL Variant | Additional Rule | Guarantees |
|---|---|---|
| **Basic 2PL** | Just growing and shrinking phases | Conflict serializable |
| **Conservative (Static) 2PL** | Lock ALL items before starting | Deadlock-free + Conflict serializable |
| **Strict 2PL** | Hold all **exclusive** locks until commit/abort | Conflict serializable + Strict (cascadeless) |
| **Rigorous 2PL** | Hold **all** locks until commit/abort | Conflict serializable + Strict + Serialization order = commit order |

### 7.9 Deadlock

**Conditions** (all must hold):
1. Mutual exclusion
2. Hold and wait
3. No preemption
4. Circular wait

**Deadlock Handling:**
- **Prevention**: Conservative 2PL, Wait-Die, Wound-Wait
- **Detection**: Wait-for graph (cycle = deadlock)
- **Recovery**: Abort one transaction (victim selection)

| Protocol | Older → Younger | Younger → Older |
|---|---|---|
| **Wait-Die** | Wait | Die (abort younger) |
| **Wound-Wait** | Wound (abort younger) | Wait |

> Both are **deadlock-free** and **starvation-free** (aborted transactions restart with same timestamp).

---

### 🧮 Worked Examples: Transactions & Concurrency

#### TX-WE1: Precedence Graph — Conflict Serializability Test

**Schedule S:**
```
T1: R(A)          W(A)                    R(B)     W(B)
T2:      R(A)          R(B)     W(B)
T3:                                             R(A)     W(A)
```

Timeline: T1:R(A), T2:R(A), T1:W(A), T2:R(B), T2:W(B), T1:R(B), T1:W(B), T3:R(A), T3:W(A)

**Find conflicts:**
1. T1:R(A) vs T2:R(A) → both reads → NO conflict
2. T1:R(A) before T1:W(A)? Same transaction → NO
3. T2:R(A) before T1:W(A) → different trans, same item, one write → **T2 → T1** (on A)
4. T1:W(A) before T3:R(A) → **T1 → T3** (on A)
5. T1:W(A) before T3:W(A) → **T1 → T3** (on A, already have this edge)
6. T2:R(B) before T1:R(B) → both reads → NO
7. T2:W(B) before T1:R(B) → **T2 → T1** (on B)
8. T2:W(B) before T1:W(B) → **T2 → T1** (on B, already have this edge)
9. T2:R(A) before T3:R(A) → both reads → NO
10. T2:R(A) before T3:W(A) → **T2 → T3** (on A)

**Precedence Graph:**
```
T2 → T1 → T3
T2 → T3
```

**Topological sort:** T2, T1, T3 → **Conflict Serializable!** ✅
Equivalent serial schedule: T2 → T1 → T3

#### TX-WE2: View Serializability (Blind Write Example)

**Schedule S:**
```
T1: R(A)      W(A)
T2:      W(A)
```

S = R1(A), W2(A), W1(A)

**Precedence graph for conflict serializability:**
- W2(A) before W1(A) → T2 → T1
- R1(A) before W2(A) → T1 → T2
- **Cycle! NOT conflict serializable** ❌

**Check view serializability against serial schedules:**

Serial S1: T1, T2 → R1(A) reads initial, W2(A) is final write
Serial S2: T2, T1 → R1(A) reads initial, W1(A) is final write

In schedule S:
- Initial read: R1(A) reads initial value of A ✅ (same as S1 and S2)
- W2(A) is a **blind write** (T2 never read A)
- Final write on A: T1 does W1(A) last

Compare with S2 (T2, T1): R1 reads initial ✅, final write by T1 ✅. But in S2, R1(A) reads initial (after W2 overwrites? No — in S2: W2(A), R1(A), W1(A) → R1 reads T2's value!)

Compare with S1 (T1, T2): R1 reads initial ✅. Final write by T2 — but in S, final write is T1 ❌

Neither matches perfectly. Let me re-examine...

Actually S = R1(A), W2(A), W1(A):
- Initial read: R1(A) reads initial value (before any write) ✅ → matches both serial orders where T1 reads first
- Final write: W1(A) is the final write on A
- In serial T2,T1: W2(A) happens, then R1(A) — but R1 would read T2's value, not initial! ❌

So S is view equivalent to T1,T2? 
- T1,T2 serial: R1(A) reads initial, W1(A), W2(A). Final write = T2.
- In S: Final write = T1. ❌

**S is NOT view serializable either!**

Let me give a proper example:

**Schedule S:** W1(A), W2(A), W2(B), W1(B)

- Conflicts: W1(A)→W2(A): T1→T2; W2(B)→W1(B): T2→T1 → **Cycle → NOT conflict serializable** ❌
- View serializability: No reads at all! Check final writes: A's final = T2, B's final = T1.
  - Serial T1,T2: final A=T2 ✅, final B=T2 ❌
  - Serial T2,T1: final A=T1 ❌, final B=T1 ✅
  - Neither matches → **NOT view serializable** ❌

Better blind write example: **S = W1(A), W2(A), W3(A)**
- All blind writes, no reads. Final write = T3.
- Serial T1,T2,T3: final write on A = T3 ✅. No reads to check. **View serializable!** ✅
- But conflict graph: T1→T2→T3 AND T1→T3 → **acyclic → also conflict serializable!** 

> 🎯 **GATE Key Insight:** View serializability is strictly more general than conflict serializability. The difference only matters when blind writes are involved. Testing view serializability is NP-complete!

#### TX-WE3: 2PL Schedule Analysis

**Schedule:**
```
T1: Lock-S(A) Read(A) Lock-X(B)        Write(B) Unlock(A) Unlock(B) Commit
T2:                         Lock-S(A)  Read(A), Lock-X(C) Write(C) Unlock(A) Unlock(C) Commit
```

**Is T1 following 2PL?**
- Growing: Lock-S(A), Lock-X(B) ✅ (acquiring locks)
- Shrinking: Unlock(A), Unlock(B) ✅ (releasing locks)
- No lock acquired after any unlock → **Yes, 2PL** ✅

**Is T2 following 2PL?**
- Lock-S(A), then Lock-X(C) → growing
- Unlock(A), Unlock(C) → shrinking
- No lock after unlock → **Yes, 2PL** ✅

> 🎯 **Lock point** = moment when transaction holds maximum locks = transition from growing to shrinking

#### TX-WE4: Timestamp Ordering Protocol

**Given:** TS(T1) = 5, TS(T2) = 10, TS(T3) = 15

**Initial:** R_TS(A) = 0, W_TS(A) = 0

| Operation | Check | Result |
|---|---|---|
| T1: Read(A) | TS(T1)=5 ≥ W_TS(A)=0 ✅ | Execute, R_TS(A)=5 |
| T3: Read(A) | TS(T3)=15 ≥ W_TS(A)=0 ✅ | Execute, R_TS(A)=15 |
| T2: Write(A) | TS(T2)=10 ≥ R_TS(A)=15? NO! ❌ | **Abort T2** (late write after later read) |
| T1: Write(A) | TS(T1)=5 ≥ R_TS(A)=15? NO! ❌ | **Abort T1** (same reason) |
| T3: Write(A) | TS(T3)=15 ≥ R_TS(A)=15 ✅, ≥ W_TS(A)=0 ✅ | Execute, W_TS(A)=15 |

**With Thomas Write Rule for T2: Write(A):**
- TS(T2)=10 < R_TS(A)=15 → still abort (TWR only helps when TS < W_TS but ≥ R_TS)

#### TX-WE5: Recoverability Classification

**Schedule S1:** R1(A), W1(A), R2(A), W2(A), C1, C2
- T2 reads from T1 (R2(A) after W1(A))
- T1 commits before T2 commits → **Recoverable** ✅
- T1 commits before T2 reads? No (R2(A) before C1) → **NOT cascadeless** ❌

**Schedule S2:** R1(A), W1(A), C1, R2(A), W2(A), C2
- T2 reads A after T1 commits → **Cascadeless** ✅
- T2 writes A after T1 commits → **Strict** ✅

**Schedule S3:** R1(A), W1(A), R2(A), W2(A), C2, C1
- T2 reads from T1 (dirty read), T2 commits before T1 → **Irrecoverable!** ❌
- If T1 aborts, T2's commit can't be undone

> 🎯 **Hierarchy:** Strict ⊂ Cascadeless ⊂ Recoverable. Irrecoverable schedules must be avoided!

#### TX-WE6: Number of Possible Schedules

**Q:** 3 transactions with 2 operations each. How many possible schedules (interleavings)?

Total operations = 6, but order within each transaction is fixed.

$$\text{Schedules} = \frac{(n_1 + n_2 + n_3)!}{n_1! \cdot n_2! \cdot n_3!} = \frac{6!}{2! \cdot 2! \cdot 2!} = \frac{720}{8} = 90$$

Of these, **serial schedules** = $3! = 6$

> 🎯 **General formula:** $n$ transactions with $k_1, k_2, \ldots, k_n$ operations: $\frac{(\sum k_i)!}{\prod k_i!}$

### 🔥 GATE Tricks: Transactions

1. **Precedence graph acyclic ↔ conflict serializable.** Topological sort gives equivalent serial order.
2. **2PL ensures conflict serializability** but allows deadlocks. Conservative 2PL prevents deadlocks.
3. **Strict 2PL** = hold X locks till commit → most practical (used in real DBMSs)
4. **Wait-Die:** Older waits, younger dies. **Wound-Wait:** Older wounds(aborts) younger, younger waits.
   - Mnemonic: In Wait-Die, the TS comparison verb is what happens. "Wait" means older process waits. "Die" means younger dies.
5. **Thomas Write Rule:** Skip obsolete writes (don't abort). Only for write-write conflicts where TS < W_TS.
6. **View serializable but not conflict serializable** = involves blind writes + NP-complete to check.
7. **Isolation levels:** Read Uncommitted is fastest but dirtiest. Serializable is safest but slowest.
8. **# serial schedules for n transactions = n!**

---

### 7.10 Timestamp-Based Protocol

Each transaction $T_i$ gets timestamp $TS(T_i)$ at start. Each data item $X$ has:
- $R\_TS(X)$: largest timestamp of any transaction that read $X$
- $W\_TS(X)$: largest timestamp of any transaction that wrote $X$

**Rules:**
- **$T_i$ wants to READ $X$:**
  - If $TS(T_i) < W\_TS(X)$: Reject (abort $T_i$, restart with new TS)
  - Else: Execute, set $R\_TS(X) = \max(R\_TS(X), TS(T_i))$

- **$T_i$ wants to WRITE $X$:**
  - If $TS(T_i) < R\_TS(X)$: Reject (abort $T_i$)
  - If $TS(T_i) < W\_TS(X)$: **Thomas Write Rule** → skip write (don't abort)
  - Else: Execute, set $W\_TS(X) = TS(T_i)$

**Properties:**
- Deadlock-free (no waiting)
- May not be recoverable (need commit bit or other mechanism)
- Ensures conflict serializability (without Thomas Write Rule)
- Thomas Write Rule allows more schedules (view serializable but may not be conflict serializable)

### 7.11 Isolation Levels ⭐

| Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|-------|-----------|--------------------|--------------|
| **Read Uncommitted** | Possible | Possible | Possible |
| **Read Committed** | ✗ | Possible | Possible |
| **Repeatable Read** | ✗ | ✗ | Possible |
| **Serializable** | ✗ | ✗ | ✗ |

**Anomalies:**
- **Dirty Read:** T1 reads uncommitted data of T2; T2 later aborts
- **Non-Repeatable Read:** T1 reads X twice; T2 modifies X between reads
- **Phantom Read:** T1 does range query twice; T2 inserts/deletes rows in range

**MVCC (Multi-Version Concurrency Control):**
- Each write creates a new version of the data item
- Readers see an appropriate snapshot (don't need locks)
- Writers don't block readers, readers don't block writers
- Used by PostgreSQL, Oracle, MySQL InnoDB
- Garbage collection needed for old versions

---

## Module 7B: Recovery System ⭐⭐

### Why Recovery?
After a crash, DB must be restored to a consistent state. Recovery uses **log records** to undo/redo operations.

### Write-Ahead Logging (WAL)
**Rule:** Before a data page is flushed to disk, ALL log records for that page must be flushed to stable storage.
- Ensures we can always undo incomplete transactions
- Log records are written sequentially (fast)

### Log Record Types
| Record | Format | Meaning |
|--------|--------|---------|
| Update | $\langle T_i, X, V_{old}, V_{new}\rangle$ | T_i changed X from old to new |
| Commit | $\langle T_i, \text{commit}\rangle$ | T_i completed |
| Abort | $\langle T_i, \text{abort}\rangle$ | T_i aborted |
| Checkpoint | $\langle \text{checkpoint}, L\rangle$ | L = list of active transactions |
| Compensation (CLR) | $\langle T_i, X, V_{old}\rangle$ | Undo action logged |

### Undo/Redo Logging

| Protocol | When to flush data | When to flush log | Recovery |
|----------|-------------------|-------------------|----------|
| **Undo only** | Before commit | Before data flush (WAL) | Undo uncommitted |
| **Redo only** | After commit | Before commit | Redo committed |
| **Undo-Redo** | Any time | Before data flush (WAL) | Undo uncommitted + Redo committed |

### Recovery Process (after crash)

1. **Analysis phase:** Scan log from last checkpoint. Identify:
   - Active transactions at crash (undo list)
   - Committed transactions since checkpoint (redo list)
   - Dirty pages

2. **Redo phase:** Scan forward from checkpoint. Redo ALL updates (both committed and uncommitted) to restore to crash state.

3. **Undo phase:** Scan backward. Undo all updates from uncommitted transactions.

### ARIES (Algorithm for Recovery and Isolation Exploiting Semantics)
Industry-standard recovery algorithm:
1. **Analysis:** Determine dirty pages + active transactions using DPT (Dirty Page Table) and ATT (Active Transaction Table)
2. **Redo:** Redo from earliest LSN in DPT (repeating history)
3. **Undo:** Undo losers from end of log backward using CLRs

**Key ARIES principles:**
- WAL (Write-Ahead Logging)
- Repeat history during redo
- Logging changes during undo (CLR records)
- **LSN (Log Sequence Number):** Unique, monotonically increasing ID for each log record
- **pageLSN:** LSN of most recent log record affecting that page

### Checkpointing
- **Purpose:** Limit the amount of log to scan during recovery
- **Fuzzy checkpoint:** Don't halt transactions; just record active transactions + dirty pages
- **Sharp checkpoint:** Halt all transactions, flush all dirty pages (expensive)

---

## Module 8: ER Model

### 8.1 Components

| Component | Symbol | Description |
|---|---|---|
| **Entity** | Rectangle | A real-world object |
| **Attribute** | Ellipse | Property of an entity |
| **Relationship** | Diamond | Association between entities |
| **Key Attribute** | Underlined ellipse | Uniquely identifies entity |
| **Multivalued Attr** | Double ellipse | Can have multiple values |
| **Derived Attr** | Dashed ellipse | Computed from other attrs |
| **Composite Attr** | Ellipse with sub-ellipses | Has sub-attributes |
| **Weak Entity** | Double rectangle | No key of its own |
| **Identifying Rel** | Double diamond | Relates weak entity to identifying entity |

### 8.2 Relationship Degree & Cardinality

**Degree:** Number of entity sets in a relationship
- Unary (recursive): 1
- Binary: 2
- Ternary: 3

**Cardinality Ratios (Binary):**

| Type | Notation | Example |
|---|---|---|
| **One-to-One** | 1:1 | Person – Passport |
| **One-to-Many** | 1:N | Department – Employee |
| **Many-to-One** | N:1 | Employee – Department |
| **Many-to-Many** | M:N | Student – Course |

### 8.3 Participation Constraints

- **Total participation** (double line): Every entity must participate
- **Partial participation** (single line): Some entities may not participate

### 8.4 Weak Entity Sets

- Cannot be uniquely identified by its own attributes alone
- Depends on an **identifying (owner) entity set** via an **identifying relationship**
- Has a **partial key** (discriminator) that distinguishes weak entities under the same owner
- Identifying relationship always has **total participation** on the weak entity side

### 8.5 ER to Relational Model Conversion

| ER Component | Relational Mapping |
|---|---|
| **Strong Entity** | Create a relation with all simple attributes; PK = key attribute |
| **Weak Entity** | Create relation with partial key + owner's PK as FK; PK = partial key + owner's PK |
| **1:1 Relationship** | Add FK to either side (prefer total participation side), or merge |
| **1:N Relationship** | Add FK on the N-side (many side) |
| **M:N Relationship** | Create new relation with PKs of both entities as FK; PK = combination of both FKs |
| **Multivalued Attr** | Create separate relation with FK to owning entity |
| **Composite Attr** | Include only simple component attributes |
| **Derived Attr** | Do not store (compute on query) |

### 8.6 Minimum Number of Tables

| Scenario | Min Tables |
|---|---|
| $n$ strong entities, no relationships | $n$ |
| Binary 1:1 (both total) | Can merge into 1 table |
| Binary 1:N | N-side gets FK (no extra table) |
| Binary M:N | Must create relationship table |
| Weak entity | Always creates its own table |
| Multivalued attribute | Always creates its own table |
| Ternary relationship | Always creates relationship table |

### 8.7 Generalization, Specialization & Aggregation ⭐

**Generalization:** Bottom-up — combine lower-level entities into a higher-level entity (e.g., Car, Truck → Vehicle)

**Specialization:** Top-down — divide higher-level entity into lower-level sub-entities (e.g., Employee → Manager, Engineer)

**ISA Hierarchy:**
- Sub-entities inherit ALL attributes of super-entity
- Sub-entities may have additional attributes
- Represented by triangle labeled "ISA"

**Constraints on Specialization:**

| Constraint | Meaning |
|-----------|--------|
| **Disjoint** | Entity can belong to at most ONE sub-entity |
| **Overlapping** | Entity can belong to MULTIPLE sub-entities |
| **Total** | Every super-entity must belong to some sub-entity |
| **Partial** | Some super-entities may not belong to any sub-entity |

**ER-to-Relational Mapping for ISA:**

| Strategy | Tables Created | When to Use |
|---------|---------------|-------------|
| Separate table per entity | Super + each sub (sub has FK to super PK) | Overlapping, partial |
| Merge into super | Only super table (nullable sub-attributes) | Few extra attributes |
| Separate tables only for subs | Each sub has all super attributes | Disjoint, total |

**Aggregation:** Treats a relationship as a higher-level entity for participation in another relationship. Used when a relationship needs to participate in another relationship.

---

### 🧮 Worked Examples: ER Model

#### ER-WE1: Complete ER-to-Relational Conversion

**Scenario:** University Database

```
[Student]----(enrolls)----[Course]
   |                         |
   PK: SID                  PK: CID
   Name                     CName
   Phone (multivalued)      Credits
   Age (derived)            
   
[Student]---total---(advised_by)---partial---[Professor]
                                              PK: PID
                                              PName
                                              Salary

[Course]----(has)----[[Lab]]   (Lab is weak entity)
                      Partial key: LabNo
                      LabName
```

**Conversion:**

| ER Component | Relation | Attributes | Primary Key |
|---|---|---|---|
| Student (strong) | Student(SID, Name) | No derived (Age), no MV (Phone separate) | SID |
| Phone (multivalued) | Student_Phone(SID, Phone) | FK: SID → Student | (SID, Phone) |
| Course (strong) | Course(CID, CName, Credits) | | CID |
| Lab (weak, owner=Course) | Lab(CID, LabNo, LabName) | FK: CID → Course | (CID, LabNo) |
| enrolls (M:N) | Enrollment(SID, CID, Grade) | FK: SID→Student, CID→Course | (SID, CID) |
| advised_by (N:1) | Add PID to Student table | FK: PID → Professor, NOT NULL (total) | — |
| Professor (strong) | Professor(PID, PName, Salary) | | PID |

**Final tables:**
1. Student(SID, Name, **PID**) — PID NOT NULL (total participation)
2. Student_Phone(**SID**, **Phone**)
3. Course(CID, CName, Credits)
4. Lab(**CID**, **LabNo**, LabName)
5. Enrollment(**SID**, **CID**, Grade)
6. Professor(PID, PName, Salary)

**Total: 6 tables**

#### ER-WE2: Minimum Tables Problem

**Q:** Given 3 entity sets A, B, C with: A—(R1, M:N)—B, B—(R2, 1:N)—C, C—(R3, 1:1)—A where both sides of R3 have total participation. What is the minimum number of tables?

**Analysis:**
- A: 1 table
- B: 1 table
- C: 1 table
- R1 (M:N): MUST create separate table → 1 table
- R2 (1:N): FK on N-side (B or C? C is the many side) → NO extra table
- R3 (1:1, both total): CAN merge A and C into 1 table!

If we merge A and C: A_C table, B table, R1 table + R2 FK in merged table
But wait — R2 is between B and C (now merged with A). FK goes on the many side.

**Minimum = 3 tables** (A merged with C, B, R1)

> Actually: A has attributes + C's attributes + FK from R2 (or vice versa). R1(M:N) needs separate table. B is separate. So minimum = **3 tables**.

#### ER-WE3: ISA Hierarchy Conversion

**Scenario:**
```
        [Person]
        PK: PID
        Name
          |
        (ISA)
       /     \
  [Student]  [Employee]
   GPA        Salary
              |
            (ISA) disjoint, total
           /       \
     [Faculty]  [Staff]
      Rank       Hours
```

**Strategy 1: Separate tables (most general)**
- Person(PID, Name)
- Student(PID, GPA) — FK to Person
- Employee(PID, Salary) — FK to Person
- Faculty(PID, Rank) — FK to Employee
- Staff(PID, Hours) — FK to Employee
- **5 tables.** Works for any constraint combo.

**Strategy 2: Push to leaves (disjoint + total on Employee subtree)**
Since Employee specialization is disjoint + total, every employee is exactly one of Faculty or Staff:
- Person(PID, Name)
- Student(PID, GPA) — FK to Person
- Faculty(PID, Salary, Rank) — FK to Person (absorb Employee attrs)
- Staff(PID, Salary, Hours) — FK to Person (absorb Employee attrs)
- **4 tables.** Employee table eliminated!

> 🎯 **GATE Rule:** Disjoint + Total specialization allows merging super-entity into sub-entities. Overlapping or partial requires keeping super-entity table.

#### ER-WE4: Ternary Relationship

**Scenario:** Supplier—supplies—Part to Project (ternary relationship)

```
[Supplier]---+
  PK: SID    |
             (Supplies)----Quantity (attribute of relationship)
[Part]-------+     
  PK: PartID |
             +
[Project]----+
  PK: ProjID
```

**Conversion:** Create a relationship table:
- Supplies(**SID**, **PartID**, **ProjID**, Quantity)
- PK = (SID, PartID, ProjID) — all three FKs form composite PK
- **ALWAYS creates a new table for ternary relationships!**

**Cardinality constraints on ternary:**
- If each (Supplier, Part) pair supplies to at most one Project: ProjID is functionally determined → PK = (SID, PartID)
- This creates an FD: SID, PartID → ProjID

#### ER-WE5: Aggregation Example

**Scenario:** Employee works_on Project. The works_on relationship is then monitored_by a Manager.

```
[Employee]----(works_on)----[Project]
                  |
              (Aggregation)
                  |
            (monitored_by)
                  |
             [Manager]
```

**Conversion:**
1. Employee(EID, ...)
2. Project(PID, ...)
3. Works_On(**EID**, **PID**, hours) — M:N relationship table
4. Monitored_By(**EID**, **PID**, MID) — FK: (EID,PID) → Works_On, MID → Manager
5. Manager(MID, ...)

The key insight: Monitored_By references the Works_On relationship as if it were an entity.

---

### 🔥 GATE Tricks: ER Model

1. **M:N always creates a new table.** 1:N never does (FK on N-side). 1:1 can merge.
2. **Weak entity always creates a table** with owner's PK included.
3. **Multivalued attribute always creates a table** with parent's PK as FK.
4. **Derived attributes are NOT stored** (computed on query).
5. **Minimum tables formula:**
   - Count entities + M:N relationships + multivalued attributes + weak entities
   - Subtract for 1:1 merges possible
6. **Total participation → NOT NULL FK.** Partial participation → nullable FK.
7. **Ternary can't be replaced by 3 binary** in general (information is lost).
8. **Cardinality of ternary:** If (A,B) → C, then key of relationship = (A,B) not (A,B,C).

---

## Quick Reference: Maximum & Minimum Results

### Number of FDs possible

Over $n$ attributes: $2^n \times 2^n$ (including trivial), but number of **non-trivial** FDs depends on context.

### Maximum Candidate Keys

For $n$ attributes: $\binom{n}{\lfloor n/2 \rfloor}$ (maximum antichain by Dilworth's theorem)

### Minimum Number of Tables from ER

- Count each M:N relationship as a table
- Each entity set is a table (1:1 may merge, 1:N uses FK)
- Each multivalued attribute and weak entity adds a table

---

## Important GATE Formulae Summary

| Formula | Expression |
|---|---|
| Blocking factor | $f = \lfloor B/R \rfloor$ |
| Blocks needed | $b = \lceil n/f \rceil$ |
| B-tree order | $p = \lfloor \frac{B+V+P_d}{V+P_d+P_b} \rfloor$ |
| B+ tree internal order | $p = \lfloor \frac{B+V}{V+P_b} \rfloor$ |
| B+ tree leaf order | $p_l = \lfloor \frac{B-P_b}{V+P_d} \rfloor$ |
| Min height B-tree | $\lceil \log_p(n+1) \rceil$ |
| Super keys from CK of size $k$ | $2^{n-k}$ |
| Lossless binary decomp | $R_1 \cap R_2 \to R_1 - R_2 \text{ or } R_2 - R_1$ |

---

## 📝 Practice Set 1: Functional Dependencies & Normalization (P1 – P20)

**P1.** $R(A,B,C,D)$, FDs: $\{A \to B, B \to C, C \to D\}$. Find all candidate keys.
- $(A)^+ = \{A,B,C,D\} = R$ → **A is the only CK**

**P2.** $R(A,B,C,D)$, FDs: $\{AB \to C, C \to D, D \to A\}$. Highest normal form?
- CKs: AB, BC, BD (all prime) → 3NF (C→D violates BCNF since C is not superkey) → **3NF**

**P3.** $R(A,B,C)$, FDs: $\{A \to B, B \to C\}$. Decompose into BCNF.
- CK = A. $B \to C$ violates BCNF → $R_1(B,C)$, $R_2(A,B)$. Both in BCNF ✅. Lossless ✅. Dependency preserving ✅.

**P4.** How many super keys for $R(A,B,C,D,E)$ with CK = $\{AB\}$?
- $2^{5-2} = 2^3 = \boxed{8}$

**P5.** $R(A,B,C,D)$, FDs: $\{A \to B, B \to C, C \to D, D \to A\}$. Number of CKs?
- Every single attribute determines all others → **4 CKs: A, B, C, D**

**P6.** $R(A,B,C)$, FDs: $\{AB \to C, C \to B\}$. Highest NF?
- CKs: AB, AC. Prime: {A,B,C} — all prime → 3NF. $C \to B$: C not superkey → NOT BCNF → **3NF**

**P7.** Is the decomposition of $R(A,B,C)$ with $A \to B$ into $R_1(A,B)$ and $R_2(A,C)$ lossless?
- $R_1 \cap R_2 = \{A\}$. $(A)^+ = \{A,B\} \supseteq R_1$. → **Lossless** ✅

**P8.** $R(A,B,C,D,E)$, FDs: $\{A \to BC, CD \to E, B \to D, E \to A\}$. Find minimal cover.
- Decompose: $A \to B$, $A \to C$, $CD \to E$, $B \to D$, $E \to A$
- $A \to C$: $(A)^+$ without $A \to C$ = $\{A,B,D,E,C\}$ (via $A \to B$, $B \to D$, ... wait: $B \to D$, then CD? Not yet C) Actually $(A)^+ = \{A,B,D\}$ — no C without $A \to C$. Then $CD \to E$ can't fire. So $A \to C$ NOT redundant.
- Check each... **Minimal cover:** $\{A \to B, A \to C, CD \to E, B \to D, E \to A\}$ (or $A \to C$ can be derived if we can get C... actually it can't. So this IS the minimal cover.)

**P9.** In BCNF decomposition, is dependency preservation guaranteed?
- **No.** BCNF decomposition guarantees lossless join but may lose dependencies.

**P10.** $R(A,B,C,D)$, FDs: $\{A \to B, C \to D\}$. CKs?
- $(AC)^+ = \{A,C,B,D\} = R$ → AC is CK. No other minimal set works → **Only CK: AC**

**P11.** Number of super keys when CKs are $\{AB, CD\}$ over $R(A,B,C,D,E)$?
- |containing AB| = $2^3$ = 8. |containing CD| = $2^3$ = 8. |containing ABCD| = $2^1$ = 2.
- By inclusion-exclusion: $8 + 8 - 2 = \boxed{14}$

**P12.** Which normal form eliminates partial dependencies?
- **2NF**

**P13.** Which normal form eliminates transitive dependencies?
- **3NF**

**P14.** If all FDs have superkey on LHS, relation is in ___?
- **BCNF**

**P15.** $R(A,B,C)$ with no FDs at all. Highest NF?
- No non-trivial FDs → all conditions trivially satisfied → **BCNF** (and also 4NF, 5NF)

**P16.** FD set $F = \{A \to BC, D \to E\}$. Is $AD \to BCE$ in $F^+$?
- $(AD)^+ = \{A,D,B,C,E\} \supseteq \{B,C,E\}$ → **Yes** ✅

**P17.** $R(A,B,C,D)$, FD: $\{AB \to C, D \to C\}$, CK = $\{ABD\}$. What NF?
- Non-prime: $\{C\}$. $D \to C$: D is proper subset of CK(ABD) → **partial dependency!** → **1NF** (not even 2NF)

**P18.** Relation $R(A,B)$ with FD $\{A \to B\}$. Highest NF?
- CK = A. Only FD has superkey on LHS → **BCNF**

**P19.** 3NF synthesis always produces _____ and _____ decomposition.
- **Lossless-join** and **dependency-preserving**

**P20.** MVD: Student ↠ Course, Student ↠ Hobby. R(Student, Course, Hobby) is in which NF?
- Student not a superkey → violates 4NF → **R is in BCNF but not 4NF** (assuming no FD violations)

---

## 📝 Practice Set 2: SQL (P21 – P40)

**P21.** `SELECT COUNT(*) FROM R WHERE A > 5;` — does this count NULLs in column A?
- COUNT(*) counts rows where A > 5. If A is NULL, then A > 5 is UNKNOWN → row NOT counted. **No.**

**P22.** Table T(X) with values {1, 1, 2, 3, NULL}. What is `SELECT COUNT(DISTINCT X) FROM T;`?
- Distinct non-NULL values: {1, 2, 3} → **3**

**P23.** `SELECT * FROM R WHERE X NOT IN (SELECT X FROM S);` — S contains NULL in X. What happens?
- NOT IN with NULL → any comparison with NULL gives UNKNOWN → **returns empty set!**
- 🎯 This is a classic GATE trap. Use NOT EXISTS instead when NULLs are possible.

**P24.** Write SQL for: "Find employees who earn more than the average salary of their department."
```sql
SELECT e1.Name FROM Employee e1
WHERE e1.Salary > (SELECT AVG(e2.Salary) FROM Employee e2 WHERE e2.DeptID = e1.DeptID);
```

**P25.** What's wrong with: `SELECT DeptID, Name, COUNT(*) FROM Employee GROUP BY DeptID;`?
- Name is in SELECT but NOT in GROUP BY and NOT an aggregate → **Error!**

**P26.** `HAVING COUNT(*) > 1` without GROUP BY — valid?
- Entire table is treated as one group → **Valid** (returns 1 row or empty)

**P27.** Difference between `DELETE FROM T;` and `TRUNCATE TABLE T;`?
- DELETE: DML, row-by-row, can rollback, fires triggers, slower
- TRUNCATE: DDL, drops all pages, can't rollback (in most DBs), no triggers, faster

**P28.** `SELECT A FROM R EXCEPT SELECT A FROM S;` is equivalent to which RA?
- $\pi_A(R) - \pi_A(S)$ — **Set difference**

**P29.** Can you use `ORDER BY` in a subquery?
- Generally **no** (unless with LIMIT/TOP). Subquery result is a set, not ordered.

**P30.** `SELECT * FROM R NATURAL JOIN S;` — what if R and S have no common columns?
- **Cartesian product** (same as CROSS JOIN)

**P31.** Write SQL for division: "Students enrolled in ALL courses"
```sql
SELECT s.SID FROM Student s
WHERE NOT EXISTS (
    SELECT c.CID FROM Course c
    WHERE NOT EXISTS (
        SELECT 1 FROM Enrollment e WHERE e.SID = s.SID AND e.CID = c.CID
    )
);
```

**P32.** `SELECT * FROM R WHERE A = ANY (SELECT A FROM S);` is same as?
- `WHERE A IN (SELECT A FROM S)` — **IN and = ANY are equivalent**

**P33.** `SELECT * FROM R WHERE A > ALL (SELECT A FROM S);` means?
- A is **greater than the maximum** value in subquery result

**P34.** What does `SELECT * FROM R WHERE EXISTS (SELECT * FROM S);` return?
- If S is non-empty → returns ALL rows of R (EXISTS is TRUE for every outer row)
- If S is empty → returns nothing

**P35.** `INSERT INTO T(A) VALUES (NULL);` when A has UNIQUE constraint — allowed?
- **Yes!** UNIQUE allows multiple NULLs (NULLs are considered distinct from each other)

**P36.** In SQL, `NULL = NULL` evaluates to?
- **UNKNOWN** (not TRUE, not FALSE)

**P37.** Execution order: FROM → ? → ? → ? → ? → ?
- FROM → **WHERE** → **GROUP BY** → **HAVING** → **SELECT** → **ORDER BY**

**P38.** Can a view be referenced in another view's definition?
- **Yes** — views can be stacked/nested

**P39.** `SELECT DISTINCT` on a table with 100 rows and 5 distinct values returns how many rows?
- **5 rows**

**P40.** `UNION` eliminates duplicates. What about `INTERSECT`?
- **Yes**, INTERSECT also eliminates duplicates (set semantics)

---

## 📝 Practice Set 3: Relational Algebra (P41 – P55)

**P41.** $\sigma_{A>5 \wedge B<10}(R)$ where $|R| = 100$. Min and max result tuples?
- Min: 0, Max: 100 → **[0, 100]**

**P42.** $\pi_A(R)$ where $|R| = 100$ and A has 20 distinct values. Result size?
- **20** (projection removes duplicates)

**P43.** $R(A,B)$ has 5 tuples, $S(B,C)$ has 3 tuples. Max tuples in $R \bowtie S$?
- **15** (every R tuple matches every S tuple)

**P44.** $R(A,B)$ has 5 tuples, $S(B,C)$ has 3 tuples. B is primary key of S and foreign key of R. Tuples in $R \bowtie S$?
- Exactly **5** (every R tuple matches exactly one S tuple)

**P45.** Express "Find students NOT enrolled in course C1" in RA.
- $\pi_{\text{SID}}(\text{Student}) - \pi_{\text{SID}}(\sigma_{\text{CID}='C1'}(\text{Enrollment}))$

**P46.** Is $\pi_A(\sigma_{B>5}(R))$ same as $\sigma_{B>5}(\pi_A(R))$?
- **No!** If B ∉ {A}, then $\pi_A(R)$ drops B, and $\sigma_{B>5}$ can't apply → second expression is invalid!

**P47.** $R \bowtie S$ vs $S \bowtie R$?
- **Same result** (natural join is commutative)

**P48.** What does $R \div \pi_B(R)$ return?
- Tuples of A-attributes that appear with EVERY B-value in R

**P49.** Left outer join: $R(A,B) \,⟕\, S(B,C)$. If tuple (1, 5) in R has no match in S?
- Result includes (1, 5, NULL) — A=1, B=5, C=NULL

**P50.** $R \cap S$ vs $R - (R - S)$?
- **Identical** — intersection defined using set difference

**P51.** Aggregate: ${}_{\text{DeptID}} \mathcal{G}_{\text{MAX(Salary)}}(\text{Employee})$ returns?
- One tuple per DeptID with the maximum salary in that department

**P52.** Can RA express recursive queries (like finding all ancestors)?
- **No!** RA has limited expressive power. Recursive queries need Datalog or SQL recursive CTEs.

**P53.** $\sigma_{c}(R \times S)$ where $c$ is $R.A = S.A$ is equivalent to?
- **Equi-join** $R \bowtie_{R.A = S.A} S$

**P54.** $|R| = 10$, $|S| = 5$. $|R - S|$ range?
- Min: $10 - 5 = 5$ (all of S is in R). Max: 10 (S and R disjoint). → **[5, 10]**

**P55.** Semi-join $R \ltimes S$ returns which tuples?
- Tuples of **R** that have a matching tuple in S (projects only R's attributes)

---

## 📝 Practice Set 4: B-Trees & Indexing (P56 – P70)

**P56.** B+ tree order 4 (max 3 keys). Insert 10, 20, 5, 15, 25, 30. How many leaf splits?
- After 5,10,20 → full. Insert 15 → split. After split + insert 25 → another split. Ans: **2 splits**

**P57.** B+ tree internal order p=200, leaf order p_l=150. Records = 10^6. Min height?
- Leaf nodes = ⌈10^6/150⌉ = 6667. Level 1: ⌈6667/200⌉ = 34. Level 2: ⌈34/200⌉ = 1. **Height = 3**

**P58.** File: 10^5 records, record=200B, block=4KB, key=20B, ptr=10B. Levels in B+ tree?
- BF = ⌊4096/200⌋ = 20. Data blocks = 5000.
- Internal: p = ⌊(4096+20)/(20+10)⌋ = ⌊4116/30⌋ = 137
- Leaf: p_l = ⌊(4096-10)/(20+10)⌋ = ⌊4086/30⌋ = 136
- Leaf nodes = ⌈100000/136⌉ = 736. Level 1: ⌈736/137⌉ = 6. Level 2: 1. **Height = 3**

**P59.** In a B-tree of order 5, minimum keys in a non-root node?
- $\lceil 5/2 \rceil - 1 = 3 - 1 = \boxed{2}$

**P60.** Primary index on 50,000 records, BF=10, index entry=20B, block=1KB. Search cost with 2-level index?
- Data blocks = 5000. Index entries = 5000. Index BF = ⌊1024/20⌋ = 51.
- Level 1 blocks = ⌈5000/51⌉ = 99. Level 2 blocks = ⌈99/51⌉ = 2. **Cost = 2 + 1 = 3**

**P61.** Secondary index on non-key attribute with 500 distinct values, 10000 records. Index entries?
- Dense: **10,000 entries** (one per record)

**P62.** Hash file with 20 buckets, each holding 5 records. Max records without overflow?
- $20 \times 5 = \boxed{100}$

**P63.** Extendible hash directory has global depth 4. How many directory entries?
- $2^4 = \boxed{16}$

**P64.** B-tree order 100. Max keys at level 3 (root=level 1)?
- Level 1: 99. Level 2: 100 × 99 = 9900. Level 3: $100^2 \times 99 = 990,000$. Max keys up to level 3: $100^3 - 1 = 999,999$

**P65.** Clustering index on attribute with 50 distinct values, 100 blocks. Index entries?
- **50 entries** (one per distinct value)

**P66.** File sorted on attribute A has primary index. Can it also have a secondary index on B?
- **Yes!** One primary/clustering + multiple secondary indexes allowed.

**P67.** B+ tree with all leaf nodes full, p_l = 10. 100 records. If 1 record inserted, how many new nodes?
- Currently 10 full leaf nodes. Insert causes 1 split → 1 new leaf + possibly 1 new internal → **1 or 2 new nodes**

**P68.** Linear hashing starts with N=4 buckets, split pointer n=0. After first split, how many buckets?
- Bucket 0 splits into 0 and 4 → **5 buckets**, n=1

**P69.** ISAM vs B+ tree: which handles insertions better?
- **B+ tree** — self-balancing. ISAM uses overflow chains that degrade.

**P70.** Order of B-tree given block=1024B, key=8B, data_ptr=8B, block_ptr=4B?
- $(p-1)(8+8) + 4p \leq 1024 → 20p \leq 1040 → p = 52$. **Order = 52**

---

## 📝 Practice Set 5: Transactions & Concurrency (P71 – P90)

**P71.** Schedule: R1(A) R2(A) W1(A) W2(A). Conflict serializable?
- Conflicts: R1(A)<W2(A): T1→T2; R2(A)<W1(A): T2→T1 → Cycle! **Not CS** ❌

**P72.** 3 transactions, 3 operations each. Number of serial schedules?
- $3! = \boxed{6}$

**P73.** 2 transactions with 3 and 2 operations. Total possible schedules?
- $\binom{5}{3} = \boxed{10}$

**P74.** Schedule: R1(A) W1(A) R2(A) W2(A) C2 C1. Recoverable?
- T2 reads from T1 (R2(A) after W1(A)). T2 commits before T1 → **Irrecoverable!** ❌

**P75.** Is Strict 2PL deadlock-free?
- **No.** Strict 2PL prevents cascading aborts but can still deadlock.

**P76.** Which 2PL variant is deadlock-free?
- **Conservative (Static) 2PL** — locks everything before starting

**P77.** Wait-Die: T1 (TS=5) requests lock held by T2 (TS=10). What happens?
- T1 is older (smaller TS). Older waits → **T1 waits**

**P78.** Wound-Wait: T1 (TS=5) requests lock held by T2 (TS=10). What happens?
- T1 is older. Older wounds younger → **T2 is aborted (wounded)**

**P79.** Thomas Write Rule: T1 wants to write X. TS(T1)=5, W_TS(X)=10. What happens?
- TS(T1) < W_TS(X) → Write is obsolete → **Skip write (don't abort)**

**P80.** Which isolation level allows dirty reads?
- **Read Uncommitted**

**P81.** Phantom reads are prevented at which isolation level?
- **Serializable** (only level that prevents all anomalies)

**P82.** MVCC: Do readers block writers?
- **No!** Readers see old version, writers create new version → no blocking

**P83.** Schedule: W1(A) R2(A) C2 A1 (A1 = abort T1). Recoverable?
- T2 reads from uncommitted T1, commits. T1 aborts → T2 read dirty data and can't be undone → **Irrecoverable** ❌

**P84.** If precedence graph has n nodes and no edges, how many topological sorts?
- $n!$ (any permutation is valid) → **n! equivalent serial schedules**

**P85.** WAL (Write-Ahead Logging) means?
- Log records must be written to stable storage **before** the corresponding data pages are flushed to disk.

**P86.** In ARIES recovery, what are the 3 phases?
- **Analysis** → **Redo** → **Undo**

**P87.** Checkpoint purpose?
- Limit the amount of log that needs to be scanned during recovery

**P88.** Can a schedule be conflict serializable but not recoverable?
- **Yes!** Serializability and recoverability are independent properties.

**P89.** Deadlock detection method?
- Maintain **wait-for graph**. Cycle in wait-for graph = deadlock.

**P90.** Lock upgrade: S-lock to X-lock. When is it safe?
- When no other transaction holds an S-lock on the same item.

---

## 📝 Practice Set 6: ER Model (P91 – P105)

**P91.** Entity with attributes: Name, Phone (can have multiple), Age (calculated from DOB). How many tables?
- 1 for entity + 1 for multivalued Phone. Age is derived (not stored). **2 tables**

**P92.** M:N relationship between Student and Course. Minimum tables?
- Student + Course + Enrollment → **3 tables**

**P93.** 1:1 relationship between Person and Passport (both total participation). Min tables?
- Can merge into **1 table** (Person with Passport attributes) → **1 table**

**P94.** 1:N between Department and Employee. Where does FK go?
- FK on the **Many side** (Employee table gets DeptID)

**P95.** Weak entity Lab with owner entity Course. Lab's PK?
- (Course's PK + Lab's partial key) = **(CourseID, LabNo)**

**P96.** 5 entity sets, 3 M:N relationships, 2 1:N relationships, 1 multivalued attribute. Min tables?
- 5 + 3 + 0 + 1 = **9 tables** (1:N uses FK, no extra table)

**P97.** Ternary M:N:N relationship. PK of relationship table?
- Combination of **all three entity PKs** (unless cardinality constraints reduce it)

**P98.** Can a weak entity participate in a M:N relationship?
- **Yes**, but it keeps its composite PK (owner's PK + partial key)

**P99.** ISA: Person → Student, Employee (overlapping, partial). Min tables?
- Must keep Person table (partial — not all persons are students/employees). 
- Must keep sub-entity tables (overlapping — a person can be both).
- **3 tables**: Person, Student, Employee

**P100.** Aggregation: when is it needed?
- When a **relationship needs to participate in another relationship**

**P101.** Self-referencing relationship: Employee manages Employee (1:N). How to map?
- Add ManagerID as FK in Employee table (referencing same table's PK) → **No extra table**

**P102.** Binary M:N with attribute. Where is the attribute stored?
- In the **relationship table** (not in either entity table)

**P103.** Total participation double line means?
- Every entity in the set **must** participate in the relationship (maps to NOT NULL FK)

**P104.** Can a relationship have attributes?
- **Yes!** Attributes of a relationship describe the association, not the entities.

**P105.** Composite attribute (Address → Street, City, Zip). How to map?
- Store only the **simple/leaf components** (Street, City, Zip) as separate columns

---

## 📝 Practice Set 7: Mixed GATE-Style MCQs (P106 – P130)

**P106.** Which of the following is always true?
(a) BCNF ⊂ 3NF (b) 3NF ⊂ BCNF (c) BCNF = 3NF (d) None
- **(a)** — every BCNF relation is in 3NF but not vice versa

**P107.** Strict 2PL prevents:
(a) Deadlock (b) Cascading rollback (c) Starvation (d) All
- **(b)** Cascading rollback (holds X locks till commit → no dirty reads)

**P108.** B+ tree is preferred for range queries because:
(a) Less height (b) More keys per node (c) Leaf nodes are linked (d) All
- **(d)** All contribute, but **(c)** is the primary reason

**P109.** Natural join of R(A,B,C) and S(B,C,D) produces a relation with how many attributes?
- Common: B, C. Result: A, B, C, D → **4 attributes**

**P110.** Which isolation level is the default in most databases?
- **Read Committed** (PostgreSQL, Oracle) or **Repeatable Read** (MySQL InnoDB)

**P111.** SELECT * FROM R CROSS JOIN S is same as?
- $R \times S$ — **Cartesian Product**

**P112.** If $F = \{A \to B\}$ and $G = \{A \to B, B \to A\}$, are $F$ and $G$ equivalent?
- Is every FD in $F$ in $G^+$? $A \to B$ ∈ $G^+$ ✅. Is every FD in $G$ in $F^+$? $B \to A$: $(B)^+ = \{B\}$ under $F$ → ❌. **NOT equivalent**

**P113.** Hash index lookup: best case block accesses?
- **1** (direct bucket access with no overflow)

**P114.** In DRC, variables range over:
- **Domain values** (individual attributes, not tuples)

**P115.** A relation with all attributes forming the PK is in which NF?
- Every attribute is prime → no partial/transitive deps on non-prime → at least **3NF** (need to check BCNF, but with PK being all attributes, any FD X→A where X ≠ all attrs and A is prime is allowed in 3NF)

**P116.** Two schedules are conflict equivalent if:
- They have the **same set of conflicting operation pairs** in the **same order**

**P117.** Maximum number of candidate keys in R with n attributes?
- $\binom{n}{\lfloor n/2 \rfloor}$ — by **Dilworth's theorem**

**P118.** Secondary index on a sorted file is still called secondary because:
- The **sort order** of the file doesn't match the index key → still secondary

**P119.** `SELECT A, COUNT(*) FROM R GROUP BY A HAVING A > 5;` — is this valid?
- **Yes.** HAVING can use GROUP BY attributes (not just aggregates).

**P120.** ARIES redo phase replays:
- **ALL updates** (both committed and uncommitted) — "repeat history"

**P121.** Foreign key can reference:
- **Primary key or UNIQUE key** of the referenced table

**P122.** A relation in 4NF is always in:
- **BCNF** (4NF ⊂ BCNF)

**P123.** Functional dependency $A \to A$ is called:
- **Trivial FD** (RHS is subset of LHS)

**P124.** Which RA operation is most expensive to compute?
- **Cartesian Product** ($R \times S$) — $O(|R| \times |S|)$

**P125.** In SQL, `BETWEEN 5 AND 10` is:
- **Inclusive** on both ends ($\geq 5$ AND $\leq 10$)

**P126.** Conflict serializable ⊂ View serializable ⊂ Serializable. True?
- **True** ✅

**P127.** Number of possible non-trivial FDs over n attributes?
- $(2^n - 1) \times (2^n - 1) - \text{trivial ones}$... For simple FDs: $n \times (n-1) = n^2 - n$ single-attribute FDs

**P128.** Relation R(A,B,C,D,E) with CK={A}. How many super keys?
- $2^{5-1} = \boxed{16}$

**P129.** View serializability is checked by:
- NP-complete algorithm (no polynomial-time test known for general case)

**P130.** Which normal form allows prime attributes on RHS of non-superkey FDs?
- **3NF** (3NF: for $X \to A$, either X is superkey OR A is prime)

---

## 📝 Practice Set 8: Numerical GATE Problems (P131 – P160)

**P131.** R(A,B,C,D,E,F), FDs: {AB→C, C→D, E→F}. CK?
- C,D,F derived → must have A,B,E. $(ABE)^+ = \{A,B,E,C,D,F\} = R$ → **CK = ABE**

**P132.** File: 10^4 records, R=500B, B=2KB. Number of data blocks?
- BF = ⌊2048/500⌋ = 4. Blocks = ⌈10000/4⌉ = **2500**

**P133.** B+ tree: block=8KB, key=12B, data_ptr=8B, block_ptr=8B. Internal order?
- $(p-1) \times 12 + p \times 8 \leq 8192 → 20p ≤ 8204 → p = 410$. **Order = 410**

**P134.** B+ tree leaf order from P133?
- $p_l \times (12+8) + 8 \leq 8192 → p_l \leq 8184/20 = 409.2 → p_l = 409$. **Leaf order = 409**

**P135.** P133 B+ tree, 10^6 records. Height?
- Leaves = ⌈10^6/409⌉ = 2445. L1: ⌈2445/410⌉ = 6. L2: 1. **Height = 3**

**P136.** 3 transactions with 2, 3, 4 operations. Total schedules?
- $\frac{9!}{2! \times 3! \times 4!} = \frac{362880}{2 \times 6 \times 24} = \frac{362880}{288} = \boxed{1260}$

**P137.** Precedence graph with 4 nodes, edges: T1→T2, T1→T3, T2→T4, T3→T4. How many topological sorts?
- T1 first (only option). Then T2 or T3. If T2 first: T3, T4. If T3 first: T2, T4. **2 topological sorts**

**P138.** Hash file: 5 buckets, capacity 3 each. h(k) = k mod 5. Insert 3,8,13,18,23,28. How many overflow records?
- All hash to bucket 3! Bucket holds 3 → 3 overflow records. **3 overflow**

**P139.** Table R has 1000 tuples. After $\pi_A(R)$, result has 50 tuples. After $\sigma_{B>10}(\pi_{A,B}(R))$, max result?
- $\pi_{A,B}(R)$ can have up to 1000 tuples (if (A,B) pairs are all distinct). Then σ filters → max **1000**

**P140.** R has 100 tuples, S has 50 tuples. R LEFT OUTER JOIN S on R.A=S.A. Min result tuples?
- **100** (all R tuples appear, NULL-padded if no match)

**P141.** Query: "Departments with more than 5 employees." SQL uses which clause after GROUP BY?
- **HAVING COUNT(*) > 5**

**P142.** B-tree of order 5. After inserting 1,2,3,4 into empty tree, what happens on inserting 5?
- [1,2,3,4] — full (max 4 keys). Insert 5 → split. Root splits into: root=[3], left=[1,2], right=[3,4,5]. Actually: [1,2] and [4,5] with 3 pushed up.

**P143.** Extendible hash, global depth = 3. Max records before any split (bucket capacity=2)?
- 8 buckets × 2 = **16 records** (if perfectly distributed)

**P144.** CKs = {AB, BC, CD}. How many superkeys? (R has 5 attrs A-E)
- |⊇AB|=8, |⊇BC|=8, |⊇CD|=8, |⊇ABC|=4, |⊇BCD|=4, |⊇ABD|=4 (AB∩CD=ABCD→4? Wait AB∪CD=ABCD), |⊇ABCD|=2, |⊇ABCD|=2 (from BC∪CD=BCD→4)
- AB∪BC=ABC(3 attrs)→2², AB∪CD=ABCD(4)→2¹, BC∪CD=BCD(3)→2², ABC∪CD=ABCD→2¹, AB∪BCD=ABCD→2¹, all three=ABCD→2¹
- IE: 8+8+8-4-2-4+2 = **16**

**P145.** $R \bowtie S$ where R has 5 attrs, S has 4 attrs, 2 common. Result degree?
- $5 + 4 - 2 = \boxed{7}$

**P146.** Schedule S: R1(X) R2(X) W2(X) R1(Y) R2(Y) W1(Y). Conflict serializable?
- Conflicts: R1(X)<W2(X)→T1→T2, R2(X)<(nothing from T1 on X after)→none, R2(Y)<W1(Y)→T2→T1 → Cycle T1→T2→T1 → **NOT CS** ❌

**P147.** In 2PL, lock point of T is at time t. All T_i with lock point before t appear before T in equivalent serial order. True?
- **True** — serialization order in 2PL = order of lock points

**P148.** R(A,B,C), FDs: {A→B, B→C}. Decompose into R1(A,B) and R2(B,C). Lossless?
- R1∩R2 = {B}. $(B)^+ = \{B,C\} \supseteq R2$. → **Lossless** ✅

**P149.** R(A,B,C,D) with FDs {A→B, C→D}. Is decomposition into R1(A,B,C) and R2(C,D) dependency preserving?
- F1: A→B (✅ in R1). F2: C→D (✅ in R2). **All preserved** ✅

**P150.** Block nested loop join: R has 100 blocks, S has 50 blocks, Memory = 12 blocks. Cost?
- Use R as outer (smaller): ⌈100/(12-2)⌉ × 50 + 100 = 10 × 50 + 100 = **600 block transfers**

**P151.** Hash join cost for R(100 blocks) and S(50 blocks)?
- $3 \times (100 + 50) = \boxed{450}$ block transfers

**P152.** Sort-merge join, R=100 blocks, S=50 blocks, M=10 buffers. External sort cost for R?
- Runs = ⌈100/10⌉ = 10. Passes = ⌈log_9(10)⌉ = 2. Sort cost R = 2×100×(1+2) = 600... Actually: initial runs produce sorted runs, then merge passes. Cost = 2 × blocks × (1 + ⌈log_{M-1}(⌈blocks/M⌉)⌉) = 2×100×(1+⌈log_9(10)⌉) = 2×100×3 = **600 for R**

**P153.** R(A,B), S(A,C), T(A,D). Which join order is best if |R|<|S|<|T|?
- Join smallest first: **(R ⋈ S) ⋈ T** (intermediate result is smaller)

**P154.** Pushing selection before join: $\sigma_{R.A=5}(R \bowtie S)$ is equivalent to?
- $\sigma_{A=5}(R) \bowtie S$ → reduces R before join → **much faster**

**P155.** Number of ER tables: 4 strong entities, 2 weak entities, 3 M:N, 2 1:N, 1 multivalued attr?
- 4 + 2 + 3 + 0 + 1 = **10 tables**

**P156.** Ternary relationship between Professor, Student, Course. PK of relationship table?
- **(ProfID, StudentID, CourseID)** — all three FKs

**P157.** Total + disjoint specialization: Person→Student, Employee. Can a table have PID=1 in both Student and Employee?
- **No!** Disjoint means each person is in AT MOST one sub-entity.

**P158.** R(A,B,C,D), FDs: {AB→CD, A→C}. 3NF but not BCNF?
- CK = AB. A→C: A is not superkey but C is non-prime → partial dep? A is proper subset of CK, C is non-prime → NOT 2NF! So... Actually: A→C means partial dependency → **NOT even 2NF!**

**P159.** File has 10000 records, 5 records/block. Binary search cost?
- Blocks = 2000. Cost = ⌈log2(2000)⌉ = **11 block accesses**

**P160.** B+ tree with 10^6 records, leaf order 100, internal order 200. Height?
- Leaves = 10000. L1 = ⌈10000/200⌉ = 50. L2 = 1. **Height = 3, search cost = 4**

---

## 📝 Practice Set 9: Rapid-Fire True/False (P161 – P190)

**P161.** Every 2NF relation is in 1NF. → **True** ✅
**P162.** Every BCNF relation is in 3NF. → **True** ✅
**P163.** A relation with 2 attributes is always in BCNF. → **True** ✅
**P164.** 3NF decomposition can always be lossless AND dependency preserving. → **True** ✅
**P165.** BCNF decomposition is always dependency preserving. → **False** ❌
**P166.** A schedule can be view serializable but not conflict serializable. → **True** ✅
**P167.** Strict 2PL prevents deadlocks. → **False** ❌
**P168.** 2PL guarantees conflict serializability. → **True** ✅
**P169.** Timestamp ordering is deadlock-free. → **True** ✅
**P170.** Primary index can be dense. → **True** ✅ (but typically sparse)
**P171.** A file can have multiple clustering indexes. → **False** ❌
**P172.** B+ tree leaves are linked in a linked list. → **True** ✅
**P173.** In B-tree, data pointers exist only at leaves. → **False** ❌ (that's B+ tree)
**P174.** COUNT(*) ignores NULLs. → **False** ❌ (counts all rows)
**P175.** COUNT(col) ignores NULLs. → **True** ✅
**P176.** NULL = NULL is TRUE in SQL. → **False** ❌ (UNKNOWN)
**P177.** UNION ALL removes duplicates. → **False** ❌ (keeps all)
**P178.** Views are stored as actual data on disk. → **False** ❌ (virtual, stored as query)
**P179.** Foreign key can be NULL. → **True** ✅ (unless explicitly constrained)
**P180.** Every relation has at least one candidate key. → **True** ✅ (worst case: all attributes)
**P181.** Division in RA handles "for all" queries. → **True** ✅
**P182.** Semi-join returns tuples from both relations. → **False** ❌ (only from left/first)
**P183.** Left outer join always returns at least |R| tuples. → **True** ✅
**P184.** SQL EXCEPT = RA set difference. → **True** ✅
**P185.** Cascadeless schedule is always recoverable. → **True** ✅
**P186.** Recoverable schedule is always cascadeless. → **False** ❌
**P187.** Thomas Write Rule ensures conflict serializability. → **False** ❌ (view serializability)
**P188.** ARIES redo phase only redoes committed transactions. → **False** ❌ (redoes ALL — "repeat history")
**P189.** Extendible hashing has no overflow chains. → **True** ✅ (splits instead)
**P190.** Linear hashing may have temporary overflow chains. → **True** ✅

---

## 📝 Practice Set 10: GATE PYQ-Style Simulation (P191 – P220)

**P191.** R(A,B,C,D,E), FDs: {A→B, A→C, CD→E, AB→D}. Number of super keys?
- CK: $(A)^+ = \{A,B,C,D,E\} = R$ → A is CK! Superkeys = $2^{5-1} = \boxed{16}$

**P192.** Schedule: R1(A) R2(B) W1(A) R1(B) W2(B) W1(B). Number of conflicts?
- R1(A) vs W1(A): same TX → no. R2(B) vs W1(B): diff TX, same item, one write → T2→T1. R2(B) vs R1(B): both read → no. W2(B) vs W1(B): diff TX → T2→T1. W2(B) vs R1(B)? R1(B) after W2(B)? Order: R2(B), W1(A), R1(B), W2(B), W1(B). 
- R1(B) before W2(B): T1→T2 on B (read-write). W2(B) before W1(B): T2→T1 on B (write-write).
- Also R2(B) before W1(B): T2→T1 on B. **Total conflicts: 3**
- Edges: T1→T2 AND T2→T1 → Cycle → **Not conflict serializable**

**P193.** B+ tree, block=4KB, key=20B, block_ptr=8B, data_ptr=8B. Records=5×10^6. Search cost?
- p = ⌊(4096+20)/(20+8)⌋ = ⌊4116/28⌋ = 147
- p_l = ⌊(4096-8)/(20+8)⌋ = ⌊4088/28⌋ = 146
- Leaves = ⌈5M/146⌉ = 34247. L1 = ⌈34247/147⌉ = 233. L2 = ⌈233/147⌉ = 2. L3 = 1.
- **Height = 4 → search cost = 5** (4 index + 1 data)

**P194.** R(A,B,C,D), FDs: {A→B, A→C, C→D}. Decompose: R1(A,B), R2(A,C), R3(C,D). Lossless?
- Use Chase test or check pairwise. R1∩R2 = {A}. $(A)^+ = \{A,B,C,D\} \supseteq R1$ or $R2$ → lossless for R1 and R2 join. Then (R1⋈R2)∩R3 = includes C. $(C)^+ = \{C,D\} = R3$ → **Lossless** ✅

**P195.** SELECT COUNT(*) FROM R WHERE EXISTS (SELECT * FROM S WHERE 1=0);
- Inner query: 1=0 is always FALSE → S filtered to empty → EXISTS returns FALSE for every row → **0**

**P196.** R has 10 tuples. S has 5 tuples. Tuples in $R \times S - R \bowtie S$?
- Max: $50 - 0 = 50$ (if no matches in join). Min: $50 - 50 = 0$ (if every combo matches)

**P197.** Which of these might NOT be recoverable? R1(A) W2(A) R2(B) W1(B) C1 C2
- T1 writes B: no one reads T1's B later... wait: R2(B) is before W1(B) → T2 reads B before T1 writes B → T2 does NOT read from T1. And W2(A): does T1 read T2's A? T1 reads A first (R1(A) before W2(A)) → no dirty read. **Recoverable** ✅

**P198.** 2PL schedule. Lock point of T1=time 5, T2=time 3, T3=time 7. Equivalent serial order?
- **T2, T1, T3** (order of lock points)

**P199.** NNL join R(100 blocks, 1000 tuples) and S(50 blocks). Cost of tuple-based nested loop?
- $1000 \times 50 + 100 = \boxed{50100}$ block transfers

**P200.** R(A,B,C) all FDs: {A→B, B→C, C→A}. Number of CKs?
- Each single attribute is a CK (each determines all) → **3 CKs**

**P201-P210** (Quick answers):
- **P201.** $R \bowtie S = S \bowtie R$? → **Yes** (commutative)
- **P202.** $\sigma_{c}(R \cup S) = \sigma_c(R) \cup \sigma_c(S)$? → **Yes** ✅
- **P203.** $\pi_A(R \cup S) = \pi_A(R) \cup \pi_A(S)$? → **Yes** ✅
- **P204.** $\sigma_c(R-S) = \sigma_c(R) - \sigma_c(S)$? → **Yes** ✅
- **P205.** $\sigma_c(R \cap S) = \sigma_c(R) \cap S$? → **Yes** ✅
- **P206.** Is $\pi_A(R - S) = \pi_A(R) - \pi_A(S)$? → **No!** ❌ (projection may create new matches)
- **P207.** $(R \bowtie S) \bowtie T = R \bowtie (S \bowtie T)$? → **Yes** (associative)
- **P208.** $\sigma_{c1 \wedge c2}(R) = \sigma_{c1}(\sigma_{c2}(R))$? → **Yes** (selection cascade)
- **P209.** $R \times \emptyset = \emptyset$? → **Yes** ✅
- **P210.** $R - R = \emptyset$? → **Yes** ✅

**P211.** Relation R(A,B,C,D,E), FDs: {AB→CD, D→E, C→B}. CKs?
- $(AC)^+ = \{A,C,B,D,E\} = R$ → AC is CK
- $(AD)^+ = \{A,D,E\}$ + ... D→E, but no B or C → not CK
- $(AB)^+ = \{A,B,C,D,E\} = R$ → AB is CK
- C→B, so AC→ABC... AC is CK (already found). Are there others?
- A appears only on LHS → must be in every CK
- $(A)^+ = \{A\}$ → need more. AB works, AC works. AD doesn't.
- **CKs: AB, AC**

**P212.** File: 50000 records, R=120B, block=4096B. Sorted file. Binary search cost?
- BF = ⌊4096/120⌋ = 34. Blocks = ⌈50000/34⌉ = 1471. Cost = ⌈log2(1471)⌉ = **11**

**P213.** SQL: `SELECT COUNT(*) FROM R WHERE A IN (SELECT A FROM S WHERE B IS NULL);` — if S has no NULL B values?
- Inner query returns empty → IN matches nothing → **0**

**P214.** In 2PL, growing phase includes?
- Only **lock acquisitions** (no releases)

**P215.** Cascading rollback means?
- Abort of one transaction causes abort of other transactions that read its uncommitted data

**P216.** Recoverable but not cascadeless: give example.
- R1(A) W1(A) R2(A) C1 C2 → T2 reads dirty data but T1 commits before T2 commits → recoverable. But T2 read before T1 committed → NOT cascadeless.

**P217.** Multiple granularity locking: Intention locks (IS, IX, SIX). SIX means?
- **Shared + Intention Exclusive** — reading the whole table but writing some rows

**P218.** Phantom problem occurs because:
- New rows inserted/deleted in the range between two reads by the same transaction

**P219.** WAL ensures:
- Log records flushed before data pages. Required for **undo** operations during recovery.

**P220.** 3NF synthesis algorithm steps (in order)?
1. Find minimal cover
2. Group FDs by LHS → create relation for each group
3. If no relation contains a CK → add a new relation with CK attributes

---

## 🔥 30 DBMS GATE Tricks & Tips

1. BCNF decomposition: always lossless, may lose dependencies
2. 3NF synthesis: always lossless AND dependency preserving
3. Single CK → 3NF = BCNF (no exception possible)
4. Binary relation (2 attrs) → always BCNF
5. All attributes prime → at least 3NF
6. Count superkeys: $2^{n-k}$ per CK, use inclusion-exclusion for multiple CKs
7. Division = double NOT EXISTS in SQL = "for all"
8. COUNT(*) counts all rows; COUNT(col) ignores NULLs
9. AVG ignores NULLs in BOTH numerator and denominator
10. NOT IN with NULLs → returns empty set (use NOT EXISTS instead!)
11. B+ tree height ≈ 3-4 for millions of records (high fan-out)
12. B+ tree leaf split: copy middle up. Internal split: push middle up.
13. Primary/clustering: max 1 per table. Secondary: unlimited.
14. Hash index: O(1) lookup, terrible for range
15. Precedence graph acyclic ↔ conflict serializable
16. 2PL ensures CS but allows deadlock; Conservative 2PL prevents deadlock
17. Strict 2PL prevents cascading rollback
18. Wait-Die: old waits, young dies. Wound-Wait: old wounds, young waits.
19. Thomas Write Rule: skip obsolete writes (doesn't abort)
20. MVCC: readers don't block writers and vice versa
21. ARIES: Analysis → Redo (ALL) → Undo (uncommitted)
22. Checkpoint limits recovery log scan
23. M:N relationship always creates extra table
24. 1:N FK goes on N-side. No extra table needed.
25. Weak entity PK = owner PK + partial key
26. Multivalued attr → separate table
27. Derived attr → not stored
28. Total participation → NOT NULL FK
29. Push selection before join (most important optimization!)
30. Execution order: FROM→WHERE→GROUP BY→HAVING→SELECT→ORDER BY

---

## 📊 Commonly Confused DBMS Pairs

| Pair | Key Difference |
|---|---|
| Super Key vs Candidate Key | CK is minimal super key |
| 3NF vs BCNF | 3NF allows RHS to be prime when LHS isn't superkey |
| Lossless vs Dependency Preserving | Lossless = no data loss on join; DP = all FDs checkable locally |
| Conflict vs View Serializable | Conflict = no cycle in precedence graph; View = harder (NP-complete) |
| Recoverable vs Cascadeless | Recoverable = commit order; Cascadeless = don't read uncommitted |
| B-tree vs B+ tree | B has data ptrs everywhere; B+ only at leaves |
| Primary vs Secondary Index | Primary on sort key (sparse); Secondary on any key (dense) |
| Dense vs Sparse Index | Dense = 1 entry/record; Sparse = 1 entry/block or value |
| WHERE vs HAVING | WHERE before grouping; HAVING after grouping |
| IN vs EXISTS | IN materializes set; EXISTS short-circuits |
| DELETE vs TRUNCATE | DELETE = DML, logged; TRUNCATE = DDL, faster |
| TRC vs DRC | TRC uses tuple vars; DRC uses domain vars |
| Strict vs Rigorous 2PL | Strict holds X locks; Rigorous holds ALL locks till commit |
| Wait-Die vs Wound-Wait | WD: old waits; WW: old kills young |
| Undo vs Redo logging | Undo: flush data before commit; Redo: flush data after commit |

---

## 📊 DBMS Formula Quick Reference Card

| # | Formula | Context |
|---|---|---|
| 1 | $f = \lfloor B/R \rfloor$ | Blocking factor |
| 2 | $b = \lceil n/f \rceil$ | Number of blocks |
| 3 | Binary search: $\lceil \log_2 b \rceil$ | Sorted file/index |
| 4 | B+ internal: $p = \lfloor (B+V)/(V+P_b) \rfloor$ | Internal node order |
| 5 | B+ leaf: $p_l = \lfloor (B-P_b)/(V+P_d) \rfloor$ | Leaf node order |
| 6 | B-tree: $p = \lfloor (B+V+P_d)/(V+P_d+P_b) \rfloor$ | B-tree order |
| 7 | Super keys: $2^{n-k}$ per CK | With inclusion-exclusion for multiple CKs |
| 8 | Total schedules: $\frac{(\sum k_i)!}{\prod k_i!}$ | n transactions interleaving |
| 9 | Serial schedules: $n!$ | n transactions |
| 10 | Max CKs: $\binom{n}{\lfloor n/2 \rfloor}$ | Dilworth's theorem |
| 11 | Block NLJ: $\lceil b_r/(M-2) \rceil \cdot b_s + b_r$ | Block nested loop join |
| 12 | Hash join: $3(b_r + b_s)$ | Hash join cost |
| 13 | Extendible hash directory: $2^d$ | Global depth $d$ |
| 14 | Primary index entries: $\lceil n_r/f_r \rceil$ | One per block |
| 15 | Clustering index entries: # distinct values | One per value |

---

## 🏁 DBMS Exam Day Checklist

- **Normalization:** Know 1NF→2NF→3NF→BCNF→4NF chain. Know when they differ.
- **Candidate keys:** Compute closure systematically. Check all combinations.
- **Decomposition:** Binary test (common attrs → superkey of one). 3NF synthesis steps. BCNF algorithm.
- **RA:** Know all 6 basic + division. Division = for all.
- **SQL:** NULL traps (COUNT, AVG, IN). Execution order. GROUP BY/HAVING rules.
- **B+ tree:** Know formulas for internal order, leaf order, height. Distinguish from B-tree.
- **Transactions:** Draw precedence graph. Check 2PL phases. Know isolation levels.
- **Recovery:** WAL rule. ARIES 3 phases. Difference between undo and redo.
- **ER:** M:N = extra table. 1:N = FK. Weak entity = composite PK. Multivalued = extra table.

---

## 🔬 Deep Dive: Query Optimization — Cost Estimation

### 📝 Estimating Selection Size

| Condition | Estimated Size |
|---|---|
| $\sigma_{A=v}(R)$ | $n_r / V(A,R)$ where $V(A,R)$ = distinct values of A |
| $\sigma_{A>v}(R)$ (uniform) | $n_r \times (A_{max} - v)/(A_{max} - A_{min})$ |
| $\sigma_{c_1 \wedge c_2}$ | $n_r \times s_1 \times s_2$ (independence assumption) |
| $\sigma_{c_1 \vee c_2}$ | $n_r \times (s_1 + s_2 - s_1 \times s_2)$ |
| $\sigma_{\neg c}$ | $n_r \times (1 - s)$ |

### 📝 Estimating Join Size

- $|R \bowtie S|$ estimate: If join on $A$ which is key of $S$ and FK of $R$: = $n_r$
- General: $\frac{n_r \times n_s}{\max(V(A,R), V(A,S))}$

### 📝 Materialization vs Pipelining

| Approach | Method | Pros | Cons |
|---|---|---|---|
| Materialization | Write intermediate results to disk | Simple | High I/O |
| Pipelining | Pass tuples directly to next operator | Low I/O, no temp files | Not always possible (e.g., sort) |

> Using pipeline: σ → ⋈ → π can be done without writing intermediate tables → much faster!

### 📝 Index Selection for Query

| Query Type | Best Index |
|---|---|
| Point query (A = 5) | Hash index or B+ tree |
| Range query (A > 5) | B+ tree (hash is useless for range!) |
| Pattern match (LIKE 'abc%') | B+ tree (prefix matching) |
| Join queries | B+ tree on join attributes |
| ORDER BY | B+ tree on sort column |

---

## 🔬 Deep Dive: Advanced SQL Constructs

### 📝 Window Functions (Analytic Functions)

```sql
SELECT Name, Salary, DeptID,
    RANK() OVER (PARTITION BY DeptID ORDER BY Salary DESC) AS dept_rank,
    DENSE_RANK() OVER (ORDER BY Salary DESC) AS overall_dense_rank,
    ROW_NUMBER() OVER (ORDER BY Salary DESC) AS row_num,
    SUM(Salary) OVER (PARTITION BY DeptID) AS dept_total,
    AVG(Salary) OVER () AS company_avg
FROM Employee;
```

| Function | Ties | Gaps |
|---|---|---|
| RANK() | Same rank for ties | Gaps after ties (1,1,3) |
| DENSE_RANK() | Same rank for ties | No gaps (1,1,2) |
| ROW_NUMBER() | No ties (arbitrary) | No gaps (1,2,3) |

### 📝 Common Table Expressions (CTE) & Recursive Queries

```sql
-- Non-recursive CTE
WITH dept_stats AS (
    SELECT DeptID, AVG(Salary) AS avg_sal
    FROM Employee GROUP BY DeptID
)
SELECT e.Name, d.avg_sal
FROM Employee e JOIN dept_stats d ON e.DeptID = d.DeptID;

-- Recursive CTE (find all ancestors in hierarchy)
WITH RECURSIVE ancestors(EmpID, ManagerID, Level) AS (
    -- Base case
    SELECT EmpID, ManagerID, 0 FROM Employee WHERE EmpID = 101
    UNION ALL
    -- Recursive case
    SELECT e.EmpID, e.ManagerID, a.Level + 1
    FROM Employee e JOIN ancestors a ON e.EmpID = a.ManagerID
)
SELECT * FROM ancestors;
```

> 🎯 **GATE note:** Recursive queries can't be expressed in RA but can in SQL (with CTEs) and Datalog.

### 📝 CASE Expression

```sql
SELECT Name,
    CASE
        WHEN Salary > 60000 THEN 'High'
        WHEN Salary > 45000 THEN 'Medium'
        ELSE 'Low'
    END AS SalaryBand
FROM Employee;
```

### 📝 String Functions in SQL

| Function | Example | Result |
|---|---|---|
| `UPPER(s)` | UPPER('abc') | 'ABC' |
| `LOWER(s)` | LOWER('ABC') | 'abc' |
| `SUBSTRING(s,pos,len)` | SUBSTRING('Hello',2,3) | 'ell' |
| `CONCAT(s1,s2)` | CONCAT('A','B') | 'AB' |
| `LENGTH(s)` | LENGTH('Hello') | 5 |
| `TRIM(s)` | TRIM('  hi  ') | 'hi' |
| `LIKE pattern` | 'abc' LIKE 'a%' | TRUE |

> `%` = any string, `_` = any single character in LIKE patterns

---

## 🔬 Deep Dive: Concurrency Control — Advanced Topics

### 📝 Multiple Granularity Locking

```
Database
   └── Table
        └── Page/Block
             └── Tuple/Row
                  └── Attribute/Field
```

**Intention Lock Types:**

| Lock | Meaning | Requested when |
|---|---|---|
| IS | Intend to acquire S lock at finer granularity | Reading some rows |
| IX | Intend to acquire X lock at finer granularity | Writing some rows |
| SIX | S on entire + IX for some | Read all, write some |

**Compatibility Matrix:**

| | IS | IX | S | SIX | X |
|---|---|---|---|---|---|
| **IS** | ✓ | ✓ | ✓ | ✓ | ✗ |
| **IX** | ✓ | ✓ | ✗ | ✗ | ✗ |
| **S** | ✓ | ✗ | ✓ | ✗ | ✗ |
| **SIX** | ✓ | ✗ | ✗ | ✗ | ✗ |
| **X** | ✗ | ✗ | ✗ | ✗ | ✗ |

> 🎯 **GATE Tip:** To lock a node in S mode: must hold IS or IX on all ancestors. To lock in X: must hold IX or SIX on all ancestors.

### 📝 Snapshot Isolation

- Each transaction sees a snapshot of the DB at its start time
- Writers create new versions (no overwrite)
- **Write skew anomaly:** Two transactions read overlapping data, write disjoint data → neither detects the other's write → inconsistency possible
- Not equivalent to serializable (weaker guarantee)
- Used by PostgreSQL (MVCC), Oracle

### 📝 Optimistic Concurrency Control

**Three phases:**
1. **Read phase:** Execute transaction, read from DB, write to private workspace
2. **Validation phase:** Check if transaction conflicts with committed ones
3. **Write phase:** If validation succeeds, apply changes to DB; else abort

- Good when conflicts are rare (low contention)
- Bad when conflicts are frequent (many aborts)
- No locking overhead → better throughput for read-heavy workloads

---

## 🔬 Deep Dive: Disk Structure & Buffer Management

### 📝 Disk Access Time

$$T_{access} = T_{seek} + T_{rotation} + T_{transfer}$$

| Component | Formula | Typical Value |
|---|---|---|
| Seek time | Time to move arm to track | 4-10 ms |
| Rotational latency | $\frac{1}{2} \times \frac{60}{RPM}$ seconds | 2-5 ms (7200 RPM → 4.17 ms) |
| Transfer time | $\frac{\text{Block size}}{\text{Transfer rate}}$ | 0.01-0.1 ms |

**Example:** RPM = 10000, seek = 5ms, block = 4KB, transfer rate = 100 MB/s
- Rotation = $\frac{1}{2} \times \frac{60}{10000} = 3$ ms
- Transfer = $\frac{4096}{100 \times 10^6} = 0.04$ ms
- Total = $5 + 3 + 0.04 = $ **8.04 ms per block**

### 📝 Buffer Replacement Policies

| Policy | Description | Used in |
|---|---|---|
| LRU | Evict least recently used page | General purpose |
| MRU | Evict most recently used | When sequential flood |
| Clock | Approximation of LRU using reference bit | Most real systems |
| FIFO | Evict oldest page | Simple but poor |
| Pinned pages | Never evict (being processed) | During operations |

> **Sequential flooding:** Full table scan loads pages sequentially, evicting useful pages. LRU performs worst here! MRU is better for sequential scans.

---

## 📝 Practice Set 11: Cross-Topic GATE Simulation (P221 – P250)

**P221.** R(A,B,C,D), FDs: {A→B, B→C, C→D}. Number of non-trivial FDs in F+?
- $(A)^+ = R$, $(B)^+ = \{B,C,D\}$, $(C)^+ = \{C,D\}$, $(AB)^+ = R$, etc.
- A→B, A→C, A→D, A→BC, A→BD, A→CD, A→BCD, B→C, B→D, B→CD, C→D, AB→C, AB→D, AB→CD... (and all combinations)
- Single-attr RHS non-trivial: A→B, A→C, A→D, B→C, B→D, C→D = **6** (plus multi-attr RHS variants). For GATE, usually ask for single-attr RHS count.

**P222.** SQL: Table R(A INT, B INT) with rows (1,2), (2,3), (NULL,4), (1,NULL).
`SELECT A, SUM(B) FROM R GROUP BY A;`
- Groups: A=1: SUM(2+NULL)=2; A=2: SUM(3)=3; A=NULL: SUM(4)=4
- **Result:** (1,2), (2,3), (NULL,4)

**P223.** Extendible hash: global depth=3, bucket capacity=3. Insert 10 keys all hashing to same 3-bit suffix. What's the final global depth?
- Each split doubles if local depth catches up. After 3 keys: 1 bucket full. 4th: split (ld=4>gd=3→double to gd=4). Still all same 4-bit suffix... keeps going. Need 4-bit distinction. If all 10 keys share same bit pattern indefinitely → can't split! Real hashing would distribute better. With k collisions in one bucket of capacity b: need ⌈log₂(k/b)⌉ additional depth. **gd = 3 + ⌈log₂(10/3)⌉ = 3 + 2 = 5** (approximately)

**P224.** Block NLJ: R=200 blocks, S=100 blocks, M=22 blocks. Join cost?
- ⌈200/20⌉ = 10 passes over S. Cost = 10 × 100 + 200 = **1200**

**P225.** Sort-merge join: R=200 blocks, S=100 blocks, M=20. Total I/O cost including sort?
- Sort R: 2×200×⌈1+log₁₉(10)⌉ = 400×(1+⌈1.08⌉) = 400×2 = 800
- Sort S: 2×100×⌈1+log₁₉(5)⌉ = 200×(1+1) = 400
- Merge: 200+100 = 300
- Total = 800 + 400 + 300 = **1500**

**P226.** R(A,B,C,D,E,F), FDs: {AB→C, BC→D, CD→E, DE→F}. Is ABCDEF the only possible candidate key?
- $(AB)^+ = \{A,B,C,D,E,F\}$ = R → AB is CK → **No**, AB is a much smaller CK!

**P227.** Which index type is best for: `WHERE age BETWEEN 25 AND 35`?
- **B+ tree** (range query, hash is useless)

**P228.** After checkpoint, 3 transactions: T1 committed, T2 active, T3 not started. On crash:
- T1: **nothing needed** (committed before crash, changes durable if redo needed they're redone)
- T2: **undo** (was active, not committed → roll back)
- T3: hadn't started → **nothing** (or if started after checkpoint: undo if uncommitted)

**P229.** Snapshot isolation anomaly: T1 reads X=50, Y=50 (total=100). T2 reads X=50, Y=50. T1 sets X=40. T2 sets Y=40. Both commit. Final: X=40, Y=40, total=80. But constraint says X+Y≥100!
- **Write skew** — snapshot isolation doesn't prevent this; serializable does.

**P230.** R(A,B,C) with CK=AB. Possible superkeys: AB, ABC → **2 superkeys**. $2^{3-2}=2$ ✅

**P231-P240** (Quick numerical):
- **P231.** log₂(1024) = **10**
- **P232.** ⌈1000/7⌉ = **143**
- **P233.** File: 10^7 records, R=50B, B=4KB. BF = 81. Blocks = 123,457.
- **P234.** Primary index entries from P233: **123,457**
- **P235.** B+ tree p=500, height 3. Max records at leaves: 500² × 499 ≈ **1.25×10⁸**
- **P236.** 4 transactions, 1 op each. Schedules: 4!/1!⁴ = **24** (same as serial since 1 op each)
- **P237.** R(A,B) has 100 tuples, S(B,C) has 80 tuples. B is key of S. |R⋈S|=? **100** (assuming referential integrity). Without RI: **max 100** (or less if some R.B not in S)
- **P238.** SQL: `SELECT * FROM R WHERE A > 5 UNION ALL SELECT * FROM R WHERE A < 10;` — rows where 5<A<10 appear how many times? **Twice** (matched by both SELECTs)
- **P239.** Cascadeless ⊂ Strict? → **No, Strict ⊂ Cascadeless** (Strict implies cascadeless, not vice versa)
- **P240.** BCNF + no non-trivial MVDs with non-superkey LHS → **4NF**

**P241-P250** (Advanced):
- **P241.** R(A,B,C,D,E), CK={A,B}. How many FDs possible with single attr on both sides?
  - Each non-trivial single-attr FD X→Y where X≠Y: 5×4=20 possible. Whether they hold depends on data/constraints.

- **P242.** Schedule: R1(A) R2(B) R3(C) W1(B) W2(C) W3(A). Conflict serializable?
  - R2(B)<W1(B): T2→T1. R3(C)<W2(C): T3→T2. R1(A)<W3(A): T1→T3.
  - Graph: T1→T3→T2→T1 → **Cycle → NOT CS** ❌

- **P243.** 5 transactions, each with 2 ops. Serial schedules = 120. Total schedules = $\frac{10!}{(2!)^5} = \frac{3628800}{32} = 113400$

- **P244.** B+ tree: insert causes root split. How does height change?
  - Height **increases by 1** (new root created above old root)

- **P245.** Relation in 5NF: every JD implied by CKs. True or False?
  - **True** ✅ (definition of 5NF/PJNF)

- **P246.** SQL DELETE with FK CASCADE: deleting parent row also deletes?
  - All **child rows** that reference the deleted parent

- **P247.** Hash join requires?
  - Smaller relation fits in memory (M > $\sqrt{b_s}$) for no recursive partitioning

- **P248.** GRANT SELECT ON Employee TO user1 WITH GRANT OPTION; — user1 can?
  - Read Employee table AND grant SELECT to others

- **P249.** Multiversion 2PL — each write creates new version. Reads get latest version before start. Benefits?
  - Readers don't block writers (see old version) while maintaining serializability

- **P250.** R(A,B,C), FDs: {A→B}. Is R in 3NF? Depends on CK. If CK=AC: A→B is partial dep → NOT 2NF. If CK=A: A is superkey → BCNF!

---

## 📝 Practice Set 12: Speed Round — 1-Line Answers (P251 – P300)

**P251.** Key that can be NULL? → **Foreign key** (if not constrained NOT NULL)
**P252.** Key that cannot be NULL? → **Primary key**
**P253.** Minimal super key? → **Candidate key**
**P254.** ACID — I stands for? → **Isolation**
**P255.** In RA, ×  is? → **Cartesian Product**
**P256.** SQL keyword for set difference? → **EXCEPT**
**P257.** B+ tree leaf nodes connected via? → **Linked list (next pointer)**
**P258.** BCNF condition? → **Every non-trivial FD has superkey on LHS**
**P259.** 4NF condition? → **Every non-trivial MVD has superkey on LHS**
**P260.** Armstrong's 3 axioms? → **Reflexivity, Augmentation, Transitivity**
**P261.** Lossless decomposition test for 2 tables? → **Common attrs form superkey of at least one**
**P262.** Serializable schedule equivalent to? → **Some serial schedule**
**P263.** 2PL has which 2 phases? → **Growing (acquire) and Shrinking (release)**
**P264.** Conflict: two ops on same item by diff transactions, at least one is? → **Write**
**P265.** WAL stands for? → **Write-Ahead Logging**
**P266.** ARIES phases (in order)? → **Analysis, Redo, Undo**
**P267.** Extendible hash: directory size? → $2^{\text{global depth}}$
**P268.** Dirty read means? → **Reading uncommitted data from another transaction**
**P269.** Phantom read involves? → **INSERT/DELETE between two range queries**
**P270.** SQL execution order (first 3 clauses)? → **FROM, WHERE, GROUP BY**
**P271.** RA division is equivalent to SQL? → **Double NOT EXISTS**
**P272.** Clustering index: how many per table? → **At most 1**
**P273.** Dense index has entry for every? → **Record**
**P274.** Sparse index has entry for every? → **Block (or distinct value for clustering)**
**P275.** B-tree has data pointers at? → **All nodes (internal + leaf)**
**P276.** B+ tree has data pointers at? → **Only leaf nodes**
**P277.** Weak entity depends on? → **Identifying/owner entity**
**P278.** Partial key also called? → **Discriminator**
**P279.** ISA represents? → **Generalization/Specialization hierarchy**
**P280.** Aggregation in ER treats? → **Relationship as entity for another relationship**
**P281.** SQL GROUP BY with NULL values: NULLs grouped as? → **Single group**
**P282.** VIEW is? → **Virtual table (query stored, not data)**
**P283.** Trigger fires on? → **INSERT, UPDATE, or DELETE events**
**P284.** CHECK constraint enforces? → **Domain/condition on attribute values**
**P285.** Assertion is? → **Global constraint checked after every modification**
**P286.** Deadlock in DB detected via? → **Wait-for graph (cycle = deadlock)**
**P287.** Starvation in DB means? → **Transaction repeatedly aborted/delayed indefinitely**
**P288.** Recovery after crash restores? → **Last consistent state before failure**
**P289.** Log record for update contains? → **Transaction ID, data item, old value, new value**
**P290.** Checkpoint saves? → **List of active transactions + dirty pages**
**P291.** Query optimization pushes _____ early? → **Selections (σ)**
**P292.** Most expensive join algorithm? → **Tuple-level Nested Loop**
**P293.** Cheapest join algorithm (when memory is sufficient)? → **Hash join**
**P294.** Sort-merge join requires? → **Both relations sorted on join attribute**
**P295.** Pipelining avoids? → **Writing intermediate results to disk**
**P296.** Functional dependency X→Y means? → **X uniquely determines Y**
**P297.** Multivalued dependency X↠Y means? → **Y values for X are independent of other attrs**
**P298.** RA is equivalent to (safe)? → **TRC and DRC (all equivalent in expressiveness)**
**P299.** MVCC stands for? → **Multi-Version Concurrency Control**
**P300.** Normal form hierarchy (strongest to weakest)? → **5NF ⊂ 4NF ⊂ BCNF ⊂ 3NF ⊂ 2NF ⊂ 1NF**

---

## 🔬 Deep Dive: Normalization — Advanced Worked Examples

### WE9: Complex Candidate Key Finding

**Given:** $R(A,B,C,D,E,F,G,H)$, FDs: $\{AB \to C, A \to DE, B \to F, F \to GH\}$

**Step 1: Classify attributes**
- Only on LHS (or neither side): A, B (never on RHS)
- Only on RHS: C, D, E, G, H
- Both sides: F

Since A and B never appear on RHS → must be in every CK.

**Step 2:** $(AB)^+ = \{A,B,C,D,E,F,G,H\} = R$ → **AB is the only CK**

**Superkeys:** $2^{8-2} = 64$

**Normal Form:**
- Non-prime: {C, D, E, F, G, H}, Prime: {A, B}
- $A \to DE$: A is proper subset of CK(AB), D and E are non-prime → **Partial dependency!** → NOT 2NF
- **R is in 1NF only.**

**2NF decomposition:**
1. $R_1(A, D, E)$ — from $A \to DE$
2. $R_2(B, F, G, H)$ — from $B \to F$, $F \to GH$
3. $R_3(A, B, C)$ — remaining with full key dependency $AB \to C$

Check $R_2$: CK=B. $F \to GH$: F not superkey → transitive dep → NOT 3NF. Need further decomposition:
- $R_{2a}(B, F)$ and $R_{2b}(F, G, H)$

Check $R_{2b}$: CK=F. $F \to GH$: F is superkey → BCNF ✅

**Final BCNF decomposition:** $\{R_1(A,D,E), R_{2a}(B,F), R_{2b}(F,G,H), R_3(A,B,C)\}$

### WE10: Checking Dependency Preservation

**Given:** $R(A,B,C)$, FDs: $\{A \to B, B \to C, C \to A\}$
- CKs: A, B, C (each determines all)
- BCNF: All FDs have superkey on LHS → **Already BCNF** ✅

But what if FDs were $\{AB \to C, C \to A\}$ on $R(A,B,C)$?
- CKs: AB, BC (verify: $(AB)^+ = R$, $(BC)^+ = \{B,C,A\} = R$)
- $C \to A$: C not superkey → NOT BCNF

BCNF decomposition using $C \to A$: $R_1(C,A)$, $R_2(B,C)$

Is $AB \to C$ preserved?
- $F_1 = \{C \to A\}$, $F_2 = \{\}$
- $(AB)^+_{F_1 \cup F_2} = \{A, B\}$ — C not reachable → **NOT preserved!** ❌

> 🎯 This is the classic example of why BCNF decomposition may lose dependencies!

### WE11: Canonical Cover — Tricky Example

**Given:** $F = \{A \to BC, B \to AC, C \to AB\}$

**Step 1: Single RHS:** $A \to B, A \to C, B \to A, B \to C, C \to A, C \to B$

**Step 2: Extraneous LHS check:** All LHS are single attributes → no extraneous.

**Step 3: Remove redundant FDs:**
- $A \to C$: $(A)^+$ without $A \to C$, using $\{A \to B, B \to A, B \to C, C \to A, C \to B\}$: $(A)^+ = \{A,B,C\}$ (via $A \to B$, $B \to C$) → C derived! **Redundant → Remove**
- $B \to A$: $(B)^+$ without $B \to A$, using $\{A \to B, B \to C, C \to A, C \to B\}$: $(B)^+ = \{B,C,A\}$ (via $B \to C$, $C \to A$) → A derived! **Redundant → Remove**
- $C \to B$: $(C)^+$ without $C \to B$, using $\{A \to B, B \to C, C \to A\}$: $(C)^+ = \{C,A,B\}$ (via $C \to A$, $A \to B$) → B derived! **Redundant → Remove**

**Minimal Cover:** $F_c = \{A \to B, B \to C, C \to A\}$ — clean cycle!

### WE12: Lossy Decomposition Example

**Given:** $R(A,B,C)$, FDs: $\{A \to B\}$. Decompose into $R_1(A,C)$ and $R_2(B,C)$.

**Lossless test:** $R_1 \cap R_2 = \{C\}$. $(C)^+ = \{C\}$ → not superkey of $R_1$ or $R_2$ → **LOSSY!** ❌

**Proof by example:**

| A | B | C |
|---|---|---|
| 1 | x | 10 |
| 2 | y | 10 |

$R_1(A,C)$: {(1,10), (2,10)}. $R_2(B,C)$: {(x,10), (y,10)}.

$R_1 \bowtie R_2$ (on C):

| A | B | C |
|---|---|---|
| 1 | x | 10 |
| 1 | y | 10 | ← **spurious tuple!**
| 2 | x | 10 | ← **spurious tuple!**
| 2 | y | 10 |

Original had 2 rows, join gives 4 → **data corrupted!**

---

## 🔬 Deep Dive: SQL — Advanced Query Patterns

### Pattern 1: Find Nth Highest Value

**3rd highest salary:**

```sql
-- Method 1: Subquery with COUNT
SELECT DISTINCT Salary FROM Employee e1
WHERE 3 = (SELECT COUNT(DISTINCT Salary) FROM Employee e2 WHERE e2.Salary >= e1.Salary);

-- Method 2: DENSE_RANK
SELECT Salary FROM (
    SELECT Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) AS rnk
    FROM Employee
) ranked WHERE rnk = 3;

-- Method 3: LIMIT OFFSET
SELECT DISTINCT Salary FROM Employee
ORDER BY Salary DESC LIMIT 1 OFFSET 2;
```

### Pattern 2: Find Duplicate Records

```sql
SELECT Name, COUNT(*) 
FROM Employee 
GROUP BY Name 
HAVING COUNT(*) > 1;
```

### Pattern 3: Running Total / Cumulative Sum

```sql
SELECT Name, Salary,
    SUM(Salary) OVER (ORDER BY EmpID ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS RunningTotal
FROM Employee;
```

### Pattern 4: Pivot / Cross-Tab (Department-wise Count)

```sql
SELECT 
    SUM(CASE WHEN DeptID = 'D1' THEN 1 ELSE 0 END) AS CS_Count,
    SUM(CASE WHEN DeptID = 'D2' THEN 1 ELSE 0 END) AS IT_Count,
    SUM(CASE WHEN DeptID = 'D3' THEN 1 ELSE 0 END) AS ECE_Count
FROM Employee;
```

### Pattern 5: Self Join — Employees Earning More Than Their Manager

```sql
SELECT e.Name AS Employee, m.Name AS Manager
FROM Employee e JOIN Employee m ON e.ManagerID = m.EmpID
WHERE e.Salary > m.Salary;
```

### Pattern 6: Find Gaps in Sequence

```sql
SELECT t1.ID + 1 AS GapStart, MIN(t2.ID) - 1 AS GapEnd
FROM T t1 JOIN T t2 ON t1.ID < t2.ID
WHERE NOT EXISTS (SELECT 1 FROM T t3 WHERE t3.ID = t1.ID + 1)
GROUP BY t1.ID;
```

### Pattern 7: Correlated DELETE

```sql
-- Delete duplicates (keep lowest ID)
DELETE FROM Employee e1
WHERE EXISTS (
    SELECT 1 FROM Employee e2
    WHERE e2.Name = e1.Name AND e2.EmpID < e1.EmpID
);
```

### 📊 SQL Join Types Visual

```
A = {1,2,3}, B = {2,3,4}

INNER JOIN:      {2, 3}     ← only matches
LEFT JOIN:       {1, 2, 3}  ← all of A, NULL-pad from B
RIGHT JOIN:      {2, 3, 4}  ← all of B, NULL-pad from A
FULL OUTER JOIN: {1, 2, 3, 4} ← all, NULL-pad both sides
CROSS JOIN:      {(1,2),(1,3),(1,4),(2,2),(2,3),(2,4),(3,2),(3,3),(3,4)} ← all combos
```

### 📊 NULL Behavior Summary

| Operation | NULL Handling |
|---|---|
| $x + \text{NULL}$ | NULL |
| $x > \text{NULL}$ | UNKNOWN |
| $x = \text{NULL}$ | UNKNOWN |
| $\text{NULL} = \text{NULL}$ | UNKNOWN |
| NOT UNKNOWN | UNKNOWN |
| TRUE OR UNKNOWN | TRUE |
| FALSE AND UNKNOWN | FALSE |
| COUNT(*) | Counts NULLs ✅ |
| COUNT(col) | Skips NULLs |
| SUM, AVG, MAX, MIN | Skip NULLs |
| GROUP BY | NULLs form one group |
| DISTINCT | NULLs treated as equal |
| UNION | NULLs treated as equal |
| ORDER BY | NULLs first or last (implementation-defined) |

---

## 🔬 Deep Dive: Transaction Schedule Analysis — More Examples

### Schedule Analysis Method (Step-by-Step)

**Given schedule S:** R1(A) W2(A) R2(B) W1(B) R1(C) W3(C) R3(A) W3(A)

**Step 1: List all operations per transaction**
- T1: R1(A), W1(B), R1(C)
- T2: W2(A), R2(B)
- T3: W3(C), R3(A), W3(A)

**Step 2: Find all conflicting pairs (different TX, same data, ≥1 write)**
| Pair | Items | Type | Edge |
|---|---|---|---|
| R1(A) < W2(A) | A | R-W | T1→T2 |
| R1(A) < W3(A) | A | R-W | T1→T3 |
| W2(A) < R3(A) | A | W-R | T2→T3 |
| W2(A) < W3(A) | A | W-W | T2→T3 |
| R2(B) < W1(B) | B | R-W | T2→T1 |
| R1(C) < W3(C)? | C | Order: R1(C) at pos 5, W3(C) at pos 6 | T1→T3 |

Wait, let me re-read the schedule order: R1(A), W2(A), R2(B), W1(B), R1(C), W3(C), R3(A), W3(A)

So R1(C) is at position 5, W3(C) at position 6 → T1→T3 ✅

**Step 3: Precedence Graph**
- T1→T2 (on A)
- T2→T1 (on B) → **Cycle T1→T2→T1!**
- T1→T3, T2→T3

**Result: NOT conflict serializable** ❌ (due to T1↔T2 cycle)

### Recoverability Analysis

**Same schedule:** R1(A) W2(A) R2(B) W1(B) R1(C) W3(C) R3(A) W3(A)

**Check "reads from" relationships:**
- Does any TX read a value written by an uncommitted TX?
- R2(B): B was not written by anyone yet (only W1(B) comes later) → R2 reads initial B → safe
- R1(C): C not written yet (W3(C) comes later) → safe
- R3(A): A was written by W2(A) → T3 reads from T2!
  - If T2 commits before T3 commits → recoverable for this pair
  - If T3 commits before T2 → irrecoverable!

**Without seeing commit order, we can't classify recoverability. Need commit operations.**

> 🎯 **GATE Pattern:** Schedule alone doesn't determine recoverability — need commit/abort order!

---

## 🔬 Deep Dive: File Organization — Detailed Analysis

### Heap File Operations

| Operation | Cost (in block accesses) |
|---|---|
| Insert | 2 (read last block + write) |
| Search (equality, no index) | b/2 average, b worst |
| Search (range) | b (full scan) |
| Delete (with search) | Search cost + 1 (write) |
| Update (with search) | Search cost + 1 (write) |

### Sorted File Operations

| Operation | Cost |
|---|---|
| Search (equality) | $\lceil \log_2 b \rceil$ |
| Search (range) | $\lceil \log_2 b \rceil + $ additional blocks in range |
| Insert | $\lceil \log_2 b \rceil + b$ (find position + shift all records!) |
| Delete | $\lceil \log_2 b \rceil + b$ (find + shift/mark) |

> ⚠️ Sorted files have expensive inserts! That's why we use B+ trees instead.

### Hash File Operations

| Operation | Static Hashing | Extendible | Linear |
|---|---|---|---|
| Search | 1–2 (if no overflow) | 1–2 | 1–2 |
| Insert | 1–2 | 1–2 (may split/double) | 1–2 (may split) |
| Delete | 1–2 | 1–2 | 1–2 |
| Range query | **b** (full scan!) | **b** | **b** |

### When to Use Which File Organization?

| If the workload is... | Best File Org | Best Index |
|---|---|---|
| Mostly point queries (=) | Hash file | Hash index |
| Mostly range queries (<, >, BETWEEN) | Sorted file | B+ tree |
| Mostly inserts | Heap file | — |
| Mixed reads and writes | Heap with B+ tree index | B+ tree |
| OLAP (analytical queries) | Sorted/Columnar | B+ tree |

---

## 🔬 Deep Dive: Relational Algebra — Complex Expressions

### RA Expression for "Find suppliers who supply ALL red parts"

**Relations:** Supplier(SID, SName), Part(PID, Color), Supplies(SID, PID)

$$\text{RedParts} = \pi_{\text{PID}}(\sigma_{\text{Color}='Red'}(\text{Part}))$$

$$\text{Result} = \text{Supplies} \div \text{RedParts}$$

Expanded:
$$\pi_{\text{SID}}(\text{Supplies}) - \pi_{\text{SID}}((\pi_{\text{SID}}(\text{Supplies}) \times \text{RedParts}) - \text{Supplies})$$

### RA Expression for "Find departments with ALL employees earning > 50K"

$$\text{Dept\_with\_low} = \pi_{\text{DeptID}}(\sigma_{\text{Salary} \leq 50000}(\text{Employee}))$$
$$\text{Result} = \pi_{\text{DeptID}}(\text{Department}) - \text{Dept\_with\_low}$$

> 🎯 "ALL satisfy condition" = total set minus those that violate

### RA Simplification Rules (for GATE)

| Before Simplification | After |
|---|---|
| $\sigma_{c1}(\sigma_{c2}(R))$ | $\sigma_{c1 \wedge c2}(R)$ |
| $\pi_A(\pi_{A,B}(R))$ | $\pi_A(R)$ |
| $\sigma_c(R \times S)$ where c is $R.A = S.A$ | $R \bowtie_{R.A=S.A} S$ |
| $\sigma_c(R \bowtie S)$ where c uses only R's attrs | $\sigma_c(R) \bowtie S$ |
| $\pi_{A,B}(R \bowtie_c S)$ where A ⊂ R, B ⊂ S | $\pi_A(R) \bowtie_c \pi_{B \cup \text{join attrs}}(S)$ |

---

## 🔬 Deep Dive: B+ Tree Operations — Complete Reference

### Insertion Algorithm (Pseudocode)

```
INSERT(key, record_ptr):
1. Find leaf node L where key belongs
2. If L has room: insert (key, record_ptr) in sorted order → DONE
3. If L is full (overflow):
   a. Split L into L and L'
   b. Redistribute: first ⌈(p_l+1)/2⌉ in L, rest in L'
   c. COPY UP smallest key of L' to parent
   d. If parent overflows → split parent:
      - PUSH UP middle key (not copy)
      - Recurse upward
   e. If root splits → create new root (height +1)
```

### Deletion Algorithm (Pseudocode)

```
DELETE(key):
1. Find leaf L containing key
2. Remove key from L
3. If L has ≥ ⌈p_l/2⌉ entries → DONE
4. If L underflows:
   a. Try REDISTRIBUTION: borrow from sibling
      - Move one entry from sibling to L
      - Update parent separator key
   b. If redistribution not possible (sibling also at minimum):
      - MERGE L with sibling
      - Remove separator from parent
      - If parent underflows → recurse upward
      - If root has 0 keys → delete root, child becomes new root (height -1)
```

### B+ Tree Properties Quick Card

| Property | Value |
|---|---|
| All leaves at same level | Yes (balanced) |
| Height for n records | $\lceil \log_{\lceil p/2 \rceil}(\lceil n/\lceil p_l/2 \rceil \rceil) \rceil + 1$ (worst case) |
| All search paths same length | Yes |
| Range query support | Excellent (linked leaves) |
| Point query cost | Height + 1 |
| Insert worst case | Height + splits |
| Delete worst case | Height + merges |
| Space utilization | ≥ 50% (each node at least half full) |
| Fan-out advantage over B-tree | No data pointers in internal → more keys → shorter tree |

### B+ Tree vs B-Tree: When to Choose

| Scenario | Better Choice |
|---|---|
| Range queries | **B+ tree** (linked leaves) |
| Exact match, key near root | **B-tree** (can find without going to leaf) |
| General DBMS use | **B+ tree** (always preferred, used in MySQL, PostgreSQL, Oracle) |
| Memory-constrained | **B+ tree** (higher fan-out = shallower) |

---

## 🔬 Deep Dive: Recovery System — Extended

### Undo vs Redo vs Undo-Redo Comparison

| | Undo Only | Redo Only | Undo-Redo |
|---|---|---|---|
| **Flush data to disk** | BEFORE commit | AFTER commit | Any time |
| **Flush log to disk** | Before data flush (WAL) | Before commit | Before data flush (WAL) |
| **On crash: committed TX** | Nothing to do | Redo | Redo |
| **On crash: uncommitted TX** | Undo | Nothing to do | Undo |
| **Log must contain** | Old values | New values | Both old and new |
| **Flexibility** | Low | Low | **High** (most practical) |
| **Real systems use** | — | — | **This one** (ARIES) |

### ARIES Recovery — Detailed Walkthrough

**Scenario:** Log after crash:

```
LSN | TX  | Type    | Item | Old | New
1   | T1  | UPDATE  | A    | 10  | 20
2   | T2  | UPDATE  | B    | 30  | 40
3   | —   | CKPT    | {T1, T2 active}
4   | T1  | UPDATE  | C    | 50  | 60
5   | T1  | COMMIT  |
6   | T2  | UPDATE  | D    | 70  | 80
7   | T3  | UPDATE  | E    | 90  | 100
--- CRASH ---
```

**Phase 1 — Analysis (from checkpoint LSN=3):**
- Active at checkpoint: {T1, T2}
- Scan forward:
  - LSN 4: T1 updates C
  - LSN 5: T1 commits → remove T1 from active list
  - LSN 6: T2 updates D
  - LSN 7: T3 starts, updates E → add T3 to active list
- **Undo list (losers, active at crash):** {T2, T3}
- **Redo list (all updates from LSN 3):** {4, 6, 7} (and potentially 1,2 if dirty pages)

**Phase 2 — Redo (from earliest in DPT):**
- Redo LSN 1: A = 20 (if pageLSN < 1)
- Redo LSN 2: B = 40 (if pageLSN < 2)
- Redo LSN 4: C = 60
- Redo LSN 6: D = 80
- Redo LSN 7: E = 100
- **All operations replayed (even for losers T2, T3)**

**Phase 3 — Undo (losers T2, T3, from end of log backward):**
- Undo LSN 7: E = 90 (restore old value), write CLR
- Undo LSN 6: D = 70, write CLR
- Undo LSN 2: B = 30, write CLR
- **Final state: A=20 (T1 committed), B=30, C=60 (T1 committed), D=70, E=90**

### Compensation Log Records (CLR)

- Written during undo phase
- Records the undo action itself
- CLRs are **never undone** (undo of undo = original → skip)
- Each CLR has an **UndoNextLSN** pointing to the next log record to undo
- Ensures recovery is idempotent (can crash during recovery and re-recover!)

### Fuzzy Checkpoint vs Sharp Checkpoint

| | Sharp | Fuzzy |
|---|---|---|
| Halt transactions? | **Yes** | No |
| Flush dirty pages? | **Yes** | No (just record DPT + ATT) |
| Availability impact | **High** (DB paused) | Low |
| Recovery work | Less | More (may need to redo further back) |
| Used in practice | No (too expensive) | **Yes** (ARIES uses fuzzy) |

---

## 🔬 Deep Dive: Distributed Databases (GATE Basics)

### Key Concepts

| Concept | Description |
|---|---|
| **Fragmentation** | Split table across sites (horizontal = by rows, vertical = by columns) |
| **Replication** | Copy data to multiple sites for availability |
| **2PC (Two-Phase Commit)** | Coordinator asks all → if all vote YES → commit; if any votes NO → abort |
| **CAP Theorem** | Can only have 2 of 3: Consistency, Availability, Partition tolerance |
| **Semi-join** | Reduce data transfer: send projection, get matching tuples back |

### Two-Phase Commit Protocol

```
Phase 1 (VOTE):
  Coordinator → all participants: "PREPARE"
  Each participant: votes YES or NO
  
Phase 2 (DECISION):
  If all YES → Coordinator: "COMMIT" → all commit
  If any NO  → Coordinator: "ABORT"  → all abort
```

**Problem:** If coordinator crashes after receiving all YES votes but before sending decision → **blocking protocol** (participants wait forever)

**3PC** solves blocking but has higher message overhead.

---

## 🔬 Deep Dive: Relational Database Design Theory — Advanced

### Dependency Preservation Test Algorithm

**For each FD $X \to Y$ in $F$:**
1. $result = X$
2. For each $R_i$ in decomposition:
   - Compute $t = (result \cap R_i)^+_{F_i}$ (closure using FDs local to $R_i$)
   - $result = result \cup (t \cap R_i)$
3. Repeat step 2 until no change
4. If $Y \subseteq result$ → FD preserved ✅

**This avoids computing $F_i^+$ (which could be exponential)**

### Lossless Join Test for n-ary Decomposition (Chase Algorithm)

**Full Chase Example:**

$R(A,B,C,D)$, FDs: $\{A \to B, C \to D\}$, Decomposition: $\{R_1(A,B), R_2(B,C), R_3(C,D)\}$

**Initial tableau:**

| | A | B | C | D |
|---|---|---|---|---|
| $R_1$ | $a_1$ | $a_2$ | $b_{13}$ | $b_{14}$ |
| $R_2$ | $b_{21}$ | $a_2$ | $a_3$ | $b_{24}$ |
| $R_3$ | $b_{31}$ | $b_{32}$ | $a_3$ | $a_4$ |

**Apply $A \to B$:** Rows 1 & 2 agree on A? No ($a_1 \neq b_{21}$). No rows agree on A → no change.
Rows 1 & 3 on A? No. Rows 2 & 3 on A? No.

**Apply $C \to D$:** Rows 2 & 3 agree on C ($a_3 = a_3$) → Make D equal: $b_{24}$ becomes $a_4$.

| | A | B | C | D |
|---|---|---|---|---|
| $R_1$ | $a_1$ | $a_2$ | $b_{13}$ | $b_{14}$ |
| $R_2$ | $b_{21}$ | $a_2$ | $a_3$ | $a_4$ |
| $R_3$ | $b_{31}$ | $b_{32}$ | $a_3$ | $a_4$ |

**Apply $A \to B$ again:** Still no rows agree on A.

No row has all $a$'s → **LOSSY decomposition!** ❌

> 🎯 Intuition: $R_2(B,C)$ bridges $R_1$ and $R_3$ but B is not a key of anything meaningful. The information connecting A to D through B and C is lost.

### Decompositon into 3NF (Synthesis Algorithm) — Complete Steps

1. Find minimal cover $F_c$
2. For each FD group with same LHS in $F_c$: create relation $R_i$ = LHS ∪ RHS
3. If no $R_i$ contains a candidate key of R → add new relation with attributes of any CK
4. Remove redundant relations: if $R_i$ attrs $\subseteq$ $R_j$ attrs → drop $R_i$

**Full Example:**
$R(A,B,C,D,E,F)$, $F = \{A \to BC, C \to DE, E \to F, F \to A\}$

**Step 1:** Minimal cover = $\{A \to B, A \to C, C \to D, C \to E, E \to F, F \to A\}$

Wait, check redundancies:
- $A \to B$: $(A)^+$ without $A \to B$ = $\{A, C, D, E, F\}$ (via $A \to C, C \to D, C \to E, E \to F, F \to A$) → B not reachable → NOT redundant
- $A \to C$: $(A)^+$ without $A \to C$ = $\{A, B\}$ → NOT redundant
- All FDs are needed.

**Step 2:** Group by LHS:
- A → {B, C} → $R_1(A, B, C)$
- C → {D, E} → $R_2(C, D, E)$
- E → {F} → $R_3(E, F)$
- F → {A} → $R_4(F, A)$

**Step 3:** Check if any $R_i$ contains a CK.
- $(A)^+ = \{A,B,C,D,E,F\} = R$ → A is CK. $R_1$ contains A → ✅

**Step 4:** No redundant relations (no $R_i$ ⊂ $R_j$).

**Final 3NF:** $\{R_1(A,B,C), R_2(C,D,E), R_3(E,F), R_4(F,A)\}$

All FDs preserved ✅. Lossless ✅ (A CK in $R_1$).

---

## 📝 Practice Set 13: More Numerical Problems (P301 – P340)

**P301.** R(A,B,C,D,E), FD: {A→BCDE}. How many superkeys? → $2^{5-1} = \boxed{16}$

**P302.** R(A,B,C), FDs: {A→B, B→A, C→C}. CKs?
- $(AC)^+ = \{A,C,B\} = R$ → AC is CK. $(BC)^+ = \{B,C,A\} = R$ → BC is CK. **CKs: AC, BC**

**P303.** B+ tree: 10^8 records, leaf order 500, internal order 600. Height?
- Leaves = 200,000. L1 = 334. L2 = 1. **Height = 3.** (If worst case half-full: leaves = 400,000 → L1 = 667 → L2 = 2 → L3 = 1 → **Height = 4**)

**P304.** Block NLJ: R=1000 blocks, S=500 blocks, M=102 blocks. Cost?
- ⌈1000/100⌉ = 10 passes × 500 + 1000 = **6000**

**P305.** Hash join: R=600 blocks, S=400 blocks. Cost?
- $3(600+400) = \boxed{3000}$

**P306.** 4 transactions, each with 3 operations. Number of possible schedules?
- $\frac{12!}{(3!)^4} = \frac{479001600}{1296} = \boxed{369600}$

**P307.** R(A,B,C,D), FDs: {A→B, A→C, A→D}. Is R in BCNF? CK? 
- CK = A. All FDs have A (superkey) on LHS → **BCNF** ✅

**P308.** Schedule: R1(X) R2(Y) W1(Y) W2(X) C1 C2. Cascadeless?
- T2 reads Y before T1 writes Y? No, R2(Y) before W1(Y) → T2 reads initial Y, not T1's. T1 reads X before T2 writes X? R1(X) before W2(X) → T1 reads initial X, not T2's. **No dirty reads → Cascadeless** ✅

**P309.** R with 2 CKs of size 3 sharing 1 attribute. n=6 total attributes. Super keys?
- CK1 ∪ CK2 has 5 attrs. |⊇CK1| = 2³ = 8. |⊇CK2| = 2³ = 8. |⊇CK1∪CK2| = $2^{6-5} = 2$. IE: 8+8-2 = **14**

**P310.** Index on Salary with 1000 distinct values, 100,000 records. Secondary dense index entries?
- **100,000** (dense = one per record)

**P311.** SQL: `SELECT A FROM R WHERE A > ALL (SELECT A FROM S);` — if S is empty?
- ALL with empty set is **TRUE** for every comparison → returns **all rows** of R!

**P312.** Extendible hashing: bucket capacity 3, all keys hash to same bucket. After inserting 7 keys, how many directory doublings?
- 3 ok (no split). 4th: split (ld=1, but may need to check bits). In worst case (all identical hash): need splits until keys separate. If truly identical: infinite splits → pathological case. Assuming keys differ in higher bits: $\lceil \log_2(7/3) \rceil = 1$ → **1 doubling**. But may need more if bit patterns collide.

**P313.** R(A,B,C,D), FDs: {AB→C, C→D, D→B}. CKs?
- $(AB)^+ = \{A,B,C,D\} = R$ ✅. $(AC)^+ = \{A,C,D,B\} = R$ ✅. $(AD)^+ = \{A,D,B,C\} = R$ ✅.
- Are there size-1 CKs? $(A)^+ = \{A\}$ → no. 
- All size-2 with A: AB, AC, AD all work. Is any non-A key possible? $(BC)^+ = \{B,C,D\}$ → no. $(BD)^+ = \{B,D\}$ → no.
- **CKs: AB, AC, AD**. Prime attrs = {A,B,C,D} = all → **3NF**. Check BCNF: C→D where C ≠ superkey → **NOT BCNF**.

**P314.** Relation with 10 attributes, CK has 3 attributes. How many possible superkeys at least?
- $2^{10-3} = \boxed{128}$

**P315.** Two-Phase Commit: coordinator sends PREPARE to 5 participants. 4 vote YES, 1 votes NO. Decision?
- **ABORT** (must be unanimous YES for commit)

**P316.** SQL: `UPDATE Employee SET Salary = Salary * 1.1 WHERE DeptID = 'D1';` — atomicity means?
- Either ALL D1 employees get raise or NONE do (no partial update)

**P317.** Cascading rollback in schedule S: T1 writes, T2 reads from T1, T3 reads from T2. If T1 aborts?
- T2 must abort (read dirty). T3 must abort (read from aborted T2). **2 cascade aborts**

**P318.** R(A,B,C), FD: {A→B}. Decompose into R1(A,B) and R2(A,C). Is this in BCNF?
- R1: CK=A, A→B has superkey on LHS → BCNF ✅
- R2: CK=AC (no FDs projected), all attributes prime → BCNF ✅
- **Both in BCNF** ✅

**P319.** File: 500,000 records on disk. Sequential scan reads all. With 5ms/block and 25 records/block:
- Blocks = 20,000. Time = 20,000 × 5ms = **100 seconds**

**P320.** B+ tree on primary key. Height 4. Point query cost with 1 disk access per node?
- 4 (traverse) + 1 (data block) = **5 disk accesses**

**P321-P340** (Rapid fire):
- **P321.** RA equivalent of `SELECT DISTINCT`? → $\pi$ (projection removes duplicates)
- **P322.** RA equivalent of `WHERE`? → $\sigma$ (selection)
- **P323.** RA equivalent of `FROM R, S`? → $R \times S$ (Cartesian product)
- **P324.** RA equivalent of `FROM R NATURAL JOIN S`? → $R \bowtie S$
- **P325.** SQL to check "at least one"? → `EXISTS`
- **P326.** SQL to check "for all"? → `NOT EXISTS .... NOT EXISTS`
- **P327.** `UNIQUE` constraint allows NULLs? → **Yes** (multiple NULLs OK)
- **P328.** `PRIMARY KEY` = `UNIQUE` + ? → `NOT NULL`
- **P329.** CHECK constraint checked when? → On INSERT and UPDATE
- **P330.** Can FOREIGN KEY reference a non-primary UNIQUE key? → **Yes**
- **P331.** ON DELETE CASCADE deletes? → Child rows referencing deleted parent
- **P332.** 3 entity sets in ternary relationship → always **1 extra table** for relationship
- **P333.** Composite attribute mapped to? → Individual simple components as columns
- **P334.** Derived attribute mapped to? → **Not stored** (computed)
- **P335.** Identifying relationship for weak entity is always? → **Total participation** on weak side
- **P336.** In ER, double rectangle = ? → **Weak entity**
- **P337.** In ER, double diamond = ? → **Identifying relationship**
- **P338.** In ER, double ellipse = ? → **Multivalued attribute**
- **P339.** In ER, dashed ellipse = ? → **Derived attribute**
- **P340.** ISA hierarchy: underlined attribute = ? → **Key attribute** (well, actually it's just regular underline for key)

---

## 📝 Practice Set 14: GATE 2-Mark Simulation (P341 – P370)

**P341.** R(A,B,C,D), FDs: {A→B, B→C, C→D, D→A}. Number of superkeys?
- CKs: A, B, C, D (4 CKs, each single attribute)
- |⊇A| = 2³=8, |⊇B| = 8, |⊇C| = 8, |⊇D| = 8
- |⊇A∪B=AB| = 4, |⊇AC| = 4, |⊇AD| = 4, |⊇BC| = 4, |⊇BD| = 4, |⊇CD| = 4
- |⊇ABC| = 2, |⊇ABD| = 2, |⊇ACD| = 2, |⊇BCD| = 2, |⊇ABCD| = 1
- IE: 4×8 - 6×4 + 4×2 - 1 = 32 - 24 + 8 - 1 = **15**

**P342.** Block size=2048, record=100, key=10, block_ptr=6, data_ptr=6. B+ tree height for 500,000 records?
- Data BF = ⌊2048/100⌋ = 20, data blocks = 25,000
- Internal p = ⌊(2048+10)/(10+6)⌋ = ⌊2058/16⌋ = 128
- Leaf p_l = ⌊(2048-6)/(10+6)⌋ = ⌊2042/16⌋ = 127
- Leaves = ⌈500000/127⌉ = 3937. L1 = ⌈3937/128⌉ = 31. L2 = 1.
- **Height = 3, search cost = 4**

**P343.** Schedule: R1(A) R2(A) W2(A) W1(A) C1 C2. Is this strict 2PL possible?
- For strict 2PL: T1 holds X-lock on A till C1. T2 needs X-lock on A for W2(A) but T1 holds... wait, T1 first does R1(A) → S-lock on A. T2 does R2(A) → S-lock on A (compatible). T2 does W2(A) → needs X-lock, but T1 holds S-lock → **T2 blocks!** This schedule is NOT possible under strict 2PL.

**P344.** R(A,B,C,D,E), FDs: {A→B, BC→D, D→E}. Find attribute closure of {A,C}:
- $(AC)^+ = \{A,C\} → A→B → \{A,B,C\} → BC→D → \{A,B,C,D\} → D→E → \{A,B,C,D,E\} = R$
- **AC is a CK!**

**P345.** Left outer join: R has 100 tuples, S has 50 tuples. R.A FK → S.A (not null). |R ⟕ S|?
- Every R tuple matches exactly one S tuple (FK constraint) → **100 tuples** (same as inner join since FK is not null)

**P346.** Sort cost for 1000 blocks with M=10 memory buffers using external merge sort:
- Initial runs = ⌈1000/10⌉ = 100
- Merge passes = ⌈log₉(100)⌉ = ⌈2.10⌉ = 3
- Total I/O = 2 × 1000 × (1 + 3) = **8000 block transfers**

**P347.** R(A,B,C,D), FDs: {AB→C, D→A}. Decompose R into R1 and R2 using D→A. Is it lossless?
- R1(D,A), R2(A,B,C,D)... wait, that's not a proper decomposition.
- Using FD D→A violating BCNF: R1(D,A), R2(B,C,D) — R1∪R2 should cover all attributes. R1∪R2 = {A,B,C,D} = R ✅. R1∩R2 = {D}. $(D)^+ = \{D,A\} = R1$. D→R1 ✅ → **Lossless** ✅

**P348.** SQL: T1(X) = {1,2,3,NULL}, T2(X) = {2,3,4,NULL}. 
- T1 UNION T2 = {1,2,3,4,NULL}. Count = **5**
- T1 INTERSECT T2 = {2,3,NULL}. Count = **3** (NULLs treated as equal for set ops)
- T1 EXCEPT T2 = {1}. Count = **1**

**P349.** R(Employee, Department, Project) with MVDs: Employee↠Department, Employee↠Project. In which NF?
- Employee not superkey, non-trivial MVDs → **violates 4NF. In BCNF (no FD violations) but not 4NF.**

**P350.** Indexed NLJ: R(100 tuples, 10 blocks), S(10000 tuples, index height 3). Cost?
- For each R tuple: 3 (index) + 1 (data) = 4. Total = 10 (read R) + 100 × 4 = **410**

**P351.** Given: view V created on table T. T has 3 columns, V selects 2. Is V updatable?
- Single table, no aggregates, no DISTINCT, no GROUP BY → **Yes** (for UPDATE and DELETE; INSERT might fail if missing column is NOT NULL without default)

**P352.** Two schedules have same set of operations but different execution orders. When are they conflict equivalent?
- When one can be obtained from the other by **swapping non-conflicting adjacent operations**

**P353.** Rigorous 2PL vs Strict 2PL?
- Strict: hold X locks till commit. Rigorous: hold ALL locks (S and X) till commit.
- Rigorous guarantees **serialization order = commit order**

**P354.** SQL: `WHERE X IN (1, 2, NULL)` when X=3? 
- 3=1→F, 3=2→F, 3=NULL→UNKNOWN. F OR F OR UNKNOWN = UNKNOWN → Row **not returned**

**P355.** When `COUNT(*)` = 0? → **When the table/group is empty**. COUNT(*) is the only aggregate that can return 0 and counts all rows.

**P356.** R(A,B,C,D), FDs: {AB→CD, C→A}. 3NF synthesis produces?
- Min cover: AB→C, AB→D, C→A
- Groups: R1(A,B,C,D) from AB→{C,D}, R2(C,A) from C→A
- R1 contains CK(AB) → no extra table needed
- But R1 ⊃ R2 (R2's attrs in R1) → **drop R2**
- Result: **just R(A,B,C,D)** — already 3NF! CKs: AB, BC. C→A: C not superkey but A is prime → 3NF ✅

**P357-P370** (Quick GATE style):
- **P357.** Table with 10^6 rows, B+ tree height 3. How many I/O for point query? → **4** (3 index + 1 data)
- **P358.** Which join needs both inputs sorted? → **Sort-merge join**
- **P359.** Deadlock requires at least how many transactions? → **2**
- **P360.** Checkpoint records list of? → **Active transactions** (and dirty page table in ARIES)
- **P361.** View serializability allows? → **Blind writes** that conflict serializability doesn't
- **P362.** LSN in ARIES stands for? → **Log Sequence Number**
- **P363.** CLR in ARIES stands for? → **Compensation Log Record**
- **P364.** DPT in ARIES stands for? → **Dirty Page Table**
- **P365.** ATT in ARIES stands for? → **Active Transaction Table**
- **P366.** Fuzzy checkpoint vs sharp? → Fuzzy doesn't halt transactions, sharp does
- **P367.** Why is conflict serializability preferred over view? → **Polynomial time** to check (vs NP-complete)
- **P368.** Lock conversion from S to X is called? → **Lock upgrade**
- **P369.** MAX(NULL, 5, 3) in SQL? → **5** (aggregates ignore NULLs)
- **P370.** Foreign key on same table = ? → **Self-referencing FK** (e.g., ManagerID in Employee)

---

## 🏁 Final DBMS Revision: 50 One-Liners

1. Every relation has at least one CK (worst case: all attributes)
2. CK = minimal superkey. No proper subset of CK is a superkey.
3. Binary relation → always BCNF
4. All attributes prime → at least 3NF
5. Single CK → 3NF = BCNF
6. BCNF decomposition: lossless always, dependency preserving not guaranteed
7. 3NF synthesis: lossless + dependency preserving always
8. 4NF: every non-trivial MVD has superkey on LHS
9. FD is a special case of MVD
10. Armstrong's axioms: reflexivity, augmentation, transitivity
11. $F^+ = $ all FDs implied by $F$
12. Minimal cover: single RHS, no extraneous LHS, no redundant FDs
13. RA: σ = WHERE, π = SELECT DISTINCT, × = FROM R,S
14. RA division = SQL double NOT EXISTS = "for all"
15. Set difference = NOT IN (careful with NULLs!) / NOT EXISTS / EXCEPT
16. Natural join on no common columns = Cartesian product
17. NULL comparisons → UNKNOWN, use IS NULL / IS NOT NULL
18. COUNT(*) counts all; COUNT(col) ignores NULLs; AVG ignores NULLs
19. NOT IN with NULLs → empty result! Use NOT EXISTS instead.
20. Execution order: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
21. B+ tree: data pointers only at leaves, leaves linked
22. B-tree: data pointers at all nodes
23. Primary/clustering index: max 1 per table. Secondary: unlimited.
24. Hash for equality. B+ tree for equality + range.
25. Conflict pair: different TX, same item, ≥1 write
26. Precedence graph acyclic ↔ conflict serializable
27. 2PL: growing + shrinking phases → conflict serializability
28. Strict 2PL: hold X locks till commit → cascadeless
29. Conservative 2PL: lock everything first → deadlock-free
30. Wait-Die: old waits, young dies. Wound-Wait: old wounds, young waits.
31. Timestamp ordering: deadlock-free, may not be recoverable
32. Thomas Write Rule: skip obsolete writes
33. Isolation levels: RU < RC < RR < Serializable
34. MVCC: readers never block writers
35. WAL: flush log before data
36. ARIES: Analysis → Redo ALL → Undo losers
37. CLR: logged during undo, never undone themselves
38. Checkpoint: limits recovery scan range
39. M:N relationship → always new table
40. 1:N → FK on N-side, no new table
41. 1:1 → FK on either side, or merge
42. Weak entity PK = owner PK + partial key
43. Multivalued attribute → separate table
44. Derived attribute → not stored
45. Total participation → NOT NULL FK
46. Aggregation: relationship participates in relationship
47. ISA: disjoint+total → can push to sub-entities
48. Push Selection early = most important optimization
49. Block NLJ cost: ⌈b_r/(M-2)⌉ × b_s + b_r
50. Hash join cost: 3(b_r + b_s)

---

## 🔬 Deep Dive: Functional Dependency Theory — Proofs & Edge Cases

### Proof: Armstrong's Axioms are Sound and Complete

**Soundness:** Every FD derived using AA is valid.
- **Reflexivity:** If $Y \subseteq X$ and two tuples agree on X, they trivially agree on Y ✅
- **Augmentation:** If $X \to Y$ and tuples agree on XZ, they agree on X (⟹ agree on Y) and on Z, so agree on YZ ✅
- **Transitivity:** If $X \to Y$ and $Y \to Z$, tuples agreeing on X agree on Y (by first FD), then agree on Z (by second FD) ✅

**Completeness proof sketch:** If $Y \notin X^+$, we can construct a 2-tuple relation that satisfies all FDs in F but violates $X \to Y$. Thus any FD not derivable from AA is not in $F^+$.

### Edge Cases in Normal Form Identification

**Case 1: No non-trivial FDs**
- If F = {} (or only trivial FDs), then EVERY attribute set is a superkey? No — only the set of ALL attributes.
- Actually with no FDs: CK = all attributes. No partial/transitive deps. → **BCNF** (even 5NF!)

**Case 2: Every pair of attributes determines all others**
- Example: R(A,B,C), FDs: {A→BC, B→AC, C→AB}
- CKs: A, B, C (each is a CK)
- Every FD has superkey on LHS → **BCNF** ✅

**Case 3: Circular FDs**
- $A \to B$, $B \to C$, $C \to A$
- CKs: A, B, C (each derives all via transitivity)
- BCNF ✅ (same as case 2)

**Case 4: Only RHS attribute in FD**
- R(A,B), FD: {→ B} (empty LHS, B is constant)
- This means B has the same value in all tuples
- CK = A (assuming A distinguishes tuples)
- {} → B means {} is superkey? No — only if {}+ = R. Here {}+ = {B} ≠ R.
- A → B: A is superkey ✅ → BCNF

### Super Key Counting — Advanced Examples

**Example 1: Overlapping CKs**
$R(A,B,C,D)$, CKs: {AB, CD}

- |⊇AB| = $2^2$ = 4: {AB, ABC, ABD, ABCD}
- |⊇CD| = $2^2$ = 4: {CD, ACD, BCD, ABCD}
- |⊇ABCD| = 1
- Total = 4 + 4 - 1 = **7**

**Example 2: Three non-overlapping CKs**
$R(A,B,C,D,E,F)$, CKs: {AB, CD, EF}

- |⊇AB| = $2^4$ = 16
- |⊇CD| = 16
- |⊇EF| = 16
- |⊇ABCD| = $2^2$ = 4
- |⊇ABEF| = 4
- |⊇CDEF| = 4
- |⊇ABCDEF| = 1
- Total = 16+16+16 - 4-4-4 + 1 = **37**

**Example 3: Subset CKs**
$R(A,B,C,D)$, CKs: {A, AB}

Wait — if A is a CK, AB cannot be a CK (it's a superkey of A, not minimal). This is **impossible**.

> 🎯 **GATE fact:** If CK₁ ⊂ CK₂, then CK₂ is NOT a candidate key (not minimal). CKs must be incomparable under set inclusion.

---

## 🔬 Deep Dive: SQL Joins — Complete Visual Guide

### Inner Join

```
Table A          Table B          A INNER JOIN B  
| id | val |     | id | cat |     | A.id | val | cat |
|----|-----|     |----|-----|     |------|-----|-----|
| 1  | a   |     | 1  | X   |     | 1    | a   | X   |
| 2  | b   |     | 3  | Y   |     | 3    | c   | Y   |
| 3  | c   |     | 4  | Z   |
```
Only matching rows from both tables.

### Left Outer Join

```
A LEFT JOIN B on A.id = B.id
| A.id | val | cat  |
|------|-----|------|
| 1    | a   | X    |
| 2    | b   | NULL |  ← no match in B → NULL
| 3    | c   | Y    |
```
All rows from A. NULL where B doesn't match.

### Right Outer Join

```
A RIGHT JOIN B on A.id = B.id
| B.id | val  | cat |
|------|------|-----|
| 1    | a    | X   |
| 3    | c    | Y   |
| 4    | NULL | Z   |  ← no match in A → NULL
```

### Full Outer Join

```
A FULL OUTER JOIN B on A.id = B.id
| id   | val  | cat  |
|------|------|------|
| 1    | a    | X    |
| 2    | b    | NULL |
| 3    | c    | Y    |
| 4    | NULL | Z    |
```

### Anti Join (LEFT JOIN WHERE NULL)

```sql
-- Find rows in A that have NO match in B
SELECT A.* FROM A LEFT JOIN B ON A.id = B.id WHERE B.id IS NULL;
```
Result: {(2, b)} — only rows in A with no B match.

> 🎯 **GATE pattern:** Anti-join = set difference. Very useful for "find X NOT in Y" queries.

---

## 🔬 Deep Dive: Indexing — Practical Scenarios

### Scenario 1: When Index HURTS Performance

```
Query: SELECT * FROM Employee;  -- full table scan
```
Index is useless here — we need ALL rows. Index actually HURTS because:
- With index: random I/O (jump to each record via pointer) → slow
- Without index: sequential scan (read blocks in order) → fast

> 🎯 **Rule:** If query returns > ~10-15% of rows, sequential scan beats index access!

### Scenario 2: Covering Index

```sql
CREATE INDEX idx ON Employee(DeptID, Salary);
SELECT DeptID, Salary FROM Employee WHERE DeptID = 'D1';
```
All needed columns are IN the index → **no need to access data file at all!**
This is called a **covering index** — answers query entirely from index.

### Scenario 3: Composite Index Column Order Matters

```
Index on (A, B, C)
```
- `WHERE A = 5` → can use index ✅
- `WHERE A = 5 AND B = 10` → can use index ✅
- `WHERE A = 5 AND B = 10 AND C = 20` → can use index ✅
- `WHERE B = 10` → **cannot use index** ❌ (leftmost prefix not available)
- `WHERE B = 10 AND C = 20` → **cannot use index** ❌
- `WHERE A = 5 AND C = 20` → can use index for A=5, then scan for C=20

> 🎯 **Leftmost prefix rule:** Composite index (A,B,C) supports queries on A, (A,B), or (A,B,C). NOT B alone, NOT C alone, NOT (B,C).

### Scenario 4: Index on Foreign Key

FK columns should almost always be indexed because:
1. JOINs on FK are very common → index speeds up join
2. FK constraint checking (on parent DELETE/UPDATE) needs to scan child table → index speeds this up

### Index Cost-Benefit Summary

| Operation | Effect of Index |
|---|---|
| Point SELECT (=) | **Much faster** (log n vs n) |
| Range SELECT | **Faster** with B+ tree |
| Full table scan | **No help** (may hurt) |
| INSERT | **Slower** (must update index too) |
| UPDATE on indexed col | **Slower** (update index) |
| DELETE | **Slightly slower** (update index) |
| JOIN on indexed col | **Much faster** |
| ORDER BY on indexed col | **Free** (already sorted in B+ tree) |
| GROUP BY on indexed col | **Faster** (sorted → easy grouping) |

---

## 🔬 Deep Dive: Concurrency — Locking Protocols & Deadlock Algorithms

### Lock Upgrade & Downgrade

```
T1: Lock-S(A) → Read(A) → [needs to write A] → Lock-Upgrade(A) to X → Write(A) → Unlock(A)
```

- **Upgrade:** S → X (only if no other TX holds S on same item)
- **Downgrade:** X → S (allowed, reduces contention)

### Deadlock Detection — Wait-For Graph Example

```
T1 holds X(A), waiting for S(B) [held by T2]
T2 holds S(B), waiting for X(C) [held by T3]
T3 holds X(C), waiting for X(A) [held by T1]

Wait-For Graph:
T1 → T2 → T3 → T1  ← CYCLE = DEADLOCK!
```

**Victim Selection Criteria:**
1. Choose the youngest transaction (least work lost)
2. Choose the one holding fewest locks
3. Choose the one closest to completion (controversial)
4. Consider starvation: don't always abort the same one

### Deadlock Prevention Scheme Comparison

| Scheme | Method | Pros | Cons |
|---|---|---|---|
| **Conservative 2PL** | Lock everything before start | No deadlock | Low concurrency, hard to predict needs |
| **Wait-Die** | Older waits, younger dies (restart) | No deadlock, no starvation | Unnecessary aborts |
| **Wound-Wait** | Older wounds younger, younger waits | No deadlock, fewer aborts than WD | Preemptive (aborts a running TX) |
| **Timeout** | Abort if wait exceeds threshold | Simple to implement | False positives, parameter tuning |
| **No waiting** | Never wait, abort immediately | Simple | Very low throughput (many aborts) |

### Wait-Die vs Wound-Wait: Detailed Example

**T1 (TS=5, older), T2 (TS=10, younger)**

**Scenario A: T2 requests lock held by T1**
- Wait-Die: T2 is younger → **T2 dies** (aborted, restarts with TS=10)
- Wound-Wait: T2 is younger → **T2 waits** (younger waits for older)

**Scenario B: T1 requests lock held by T2**
- Wait-Die: T1 is older → **T1 waits** (older may wait for younger)
- Wound-Wait: T1 is older → **T1 wounds T2** (T2 aborted, T1 gets lock)

> 🎯 **Mnemonic:**
> - Wait-Die: "Let the elders **wait**, make the youngsters **die**"
> - Wound-Wait: "Elders **wound** youngsters, youngsters **wait** for elders"
> - Both: the transaction that gets aborted is always the **younger** one!

---

## 🔬 Deep Dive: ER Model — Complex Conversion Problems

### Problem 1: University with ISA + Weak Entity + Aggregation

```
[Student] (SID, Name)
    |
  (ISA) overlapping, total
    |        \
[UG_Student]  [PG_Student]
(Year)         (Thesis)
    |
  ((takes)) weak relationship
    |
[[Assignment]] (AssNo, Marks)  — weak entity, owner = UG_Student
    
[Student]----(enrolls)----[Course] (CID, Title)
     |
  (Aggregation on enrolls)
     |
  (grades)
     |
[Professor] (PID, Name)
```

**Conversion:**
1. Student(**SID**, Name) — base table
2. UG_Student(**SID**, Year) — FK to Student (total+overlapping → keep both)
3. PG_Student(**SID**, Thesis) — FK to Student
4. Assignment(**SID**, **AssNo**, Marks) — FK: SID → UG_Student
5. Course(**CID**, Title)
6. Enrollment(**SID**, **CID**, Grade) — FKs to Student and Course
7. Grades(**SID**, **CID**, **PID**, FinalGrade) — FK: (SID,CID) → Enrollment, PID → Professor
8. Professor(**PID**, Name)

**Total: 8 tables**

### Problem 2: Minimum vs Maximum Tables

Given same ER diagram, what if ISA is **disjoint + total**?

- Can merge Student into UG/PG: UG_Student(SID, Name, Year), PG_Student(SID, Name, Thesis)
- Drop Student table → **saves 1 table → 7 tables**

What if ISA is **overlapping + partial**?
- Must keep Student (partial: not everyone specializes)
- Must keep sub-tables (overlapping: can be both UG and PG)
- **All 8 tables needed**

### Problem 3: Self-Referencing Relationship

```
[Employee]---(manages, 1:N)---[Employee]
              ManagerID
```

Maps to: Employee(**EID**, Name, Salary, **ManagerID**) where ManagerID is FK to Employee(EID).

**Self-join for "find all managers":**
```sql
SELECT DISTINCT m.Name
FROM Employee e JOIN Employee m ON e.ManagerID = m.EID;
```

### Problem 4: N-ary Relationship with Identifying Attributes

```
[Student]---+
[Course]----+---(RegisteredIn, Ternary, with 'Section' attribute)
[Semester]--+
```

Registered_In(**SID**, **CID**, **SemID**, Section)

Reducing ternary to binary: NOT always possible without information loss!
- If each (Student, Course) in at most one Semester → SemID functionally determined → PK = (SID, CID)
- Otherwise all three are needed in PK

> 🎯 **GATE Rule:** Ternary relationships cannot always be decomposed into three binary relationships without losing information!

---

## 🔬 Deep Dive: Query Optimization — More Algorithms

### Pipelining vs Materialization — Worked Example

**Query:** $\pi_{\text{Name}}(\sigma_{\text{Salary}>50000}(\text{Employee} \bowtie \text{Department}))$

**Materialization approach:**
1. Compute Employee ⋈ Department → write temp T1 to disk
2. Compute σ(T1) → write temp T2 to disk
3. Compute π(T2) → write result to disk
- Total I/O: read E + read D + write T1 + read T1 + write T2 + read T2 + write result

**Pipelining approach:**
1. For each tuple from Employee ⋈ Department (computed on-the-fly):
   - Apply σ immediately → if passes, apply π → output directly
- Total I/O: read E + read D + write result
- **Huge saving — no temp files!**

### Operators That Block Pipelining

| Operator | Pipelineable? | Why |
|---|---|---|
| Selection (σ) | ✅ Yes | Tuple-by-tuple filter |
| Projection (π) | ✅ Yes (if no DISTINCT) | Tuple-by-tuple |
| Nested Loop Join | ✅ Yes | Probe inner for each outer tuple |
| Sort | ❌ No | Must see all tuples before outputting |
| Hash Join (build phase) | ❌ No (during build) | Must build hash table first |
| GROUP BY with aggregate | ❌ No | Must see all tuples in group |
| DISTINCT | ❌ No | Must see all to remove duplicates |

### Join Order Optimization

**Given:** 3 relations R, S, T. Possible join orders:
1. (R ⋈ S) ⋈ T
2. (R ⋈ T) ⋈ S
3. (S ⋈ T) ⋈ R

**Heuristics:**
- Join smallest intermediate result first
- Push selections before joins (reduce input size)
- If R.A is selective (few matches), start with R

**Example:** R has 1000 tuples, S has 10000, T has 100.
- (R ⋈ T) first: small intermediate → then ⋈ S
- Better than (R ⋈ S) first: large intermediate → then ⋈ T

**Number of join orders for n relations:** $(2(n-1))! / (n-1)!$ (Catalan number related)
- 3 relations: 12 orders to consider
- 4 relations: 120 orders
- 10 relations: billions → need dynamic programming (System R optimizer)

---

## 📝 Practice Set 15: Final Mega Practice (P371 – P420)

**P371.** R(A,B,C,D), FDs: {A→B, B→C}. What's the highest NF? CK=AD. Non-prime={B,C}. A→B: partial dep (A⊂CK, B non-prime) → **1NF**

**P372.** R(A,B,C), FDs: {AB→C}. CK=AB. Only FD has superkey on LHS → **BCNF**

**P373.** R(A,B,C,D), FDs: {A→BCD, BC→A}. CKs?
- $(A)^+ = R$ → A is CK. $(BC)^+ = \{B,C,A,D\} = R$ → BC is CK. **CKs: A, BC**

**P374.** From P373: How many superkeys?
- |⊇A| = $2^3$ = 8, |⊇BC| = $2^2$ = 4, |⊇ABC| = $2^1$ = 2
- IE: 8 + 4 - 2 = **10**

**P375.** SQL: `SELECT A, B FROM R UNION SELECT A, B FROM S ORDER BY A;`
- Valid ✅ (ORDER BY applies to final UNION result)

**P376.** B-tree order 3 (max 2 keys per node). Min keys in non-root internal node?
- ⌈3/2⌉ - 1 = 2 - 1 = **1**

**P377.** B+ tree with 3 levels (root + 1 internal + leaves). Internal order 100, leaf order 50. Max records?
- Root: 1 node, max 100 pointers
- Internal level: 100 nodes, each with 100 pointers → 10000 leaf pointers
- Leaves: 10000 nodes × 50 records = **500,000 records**

**P378.** Schedule: W1(A) W2(A) W3(A) C3 C2 C1. Recoverable?
- No reads at all! No "reads from" relationship → **trivially recoverable** ✅ (and cascadeless and strict)

**P379.** Schedule: R1(A) W2(A) C2 R1(B) C1. Is T1 consistent?
- T1 reads A (old value), then T2 writes A and commits, then T1 reads B → T1 sees inconsistent snapshot (A from before T2, B from after T2) → **Non-repeatable read anomaly** at Read Committed level

**P380.** Hash join: when does it degrade to nested loop?
- When the hash table doesn't fit in memory → **recursive partitioning** needed. Worst case: all keys hash to same bucket → degrades to nested loop.

**P381.** R(A,B,C,D,E,F), FDs: {AB→C, BC→AD, D→E, CF→B}. Find CK.
- $(AB)^+ = \{A,B,C,D,E\}$ → missing F → ABF is potential.
- $(ABF)^+ = \{A,B,F,C,D,E\} = R$ → **ABF is CK**
- Are there smaller? $(AF)^+ = \{A,F\}$ → no. $(BF)^+ = \{B,F\}$ → no. $(CF)^+ = \{C,F,B,A,D,E\} = R$ → **CF is CK!**
- $(ACF)^+$: contains CF → yes, but CF alone works → ACF not minimal.
- Check: is there single-attr CK? No. Size-2 CKs: CF works. AB doesn't (missing F). Others?
- $(DF)^+ = \{D,F,E\}$ → no. $(EF)^+ = \{E,F\}$ → no. $(AF)^+ = \{A,F\}$ → no. $(BF)^+ = \{B,F\}$ → no.
- **CKs: CF (and need to check if ABF is also minimal — is AB sufficient? No, F required. So ABF is CK too.)**

**P382.** Indexed NLJ vs Block NLJ: when is indexed better?
- **When inner relation has index on join attribute** and outer is small → indexed NLJ
- When no index available → block NLJ better

**P383.** `SELECT * FROM R WHERE A = 5 OR B = 10;` — can B+ tree index on A help?
- **Partially** — can use index for A=5 part, then union with sequential scan for B=10. 
- Or: index on A + index on B → merge results (bitmap index merge)

**P384.** Assertion vs Trigger?
- Assertion: declarative, checked after every operation, global
- Trigger: procedural, fired on specific events, action-oriented

**P385.** Deferred constraint checking: constraints checked when?
- At **commit** time (not after each statement). Useful for circular FKs.

**P386.** Phantom prevention using predicate locking?
- Lock the predicate (range condition) instead of individual rows → no phantom inserts in that range

**P387.** Multiversion timestamp ordering: how many versions maintained?
- One per write by a committed transaction + one by each active transaction → potentially many → **garbage collection** needed

**P388.** When is hash index better than B+ tree?
- When **only equality queries** (no range, no ORDER BY, no LIKE prefix)
- Hash: O(1) average. B+ tree: O(log n).

**P389.** R(A,B,C,D,E), FDs: {A→B, C→D}. 3NF synthesis result?
- Min cover: {A→B, C→D}. Groups: R1(A,B), R2(C,D). Neither contains CK (ACE or similar).
- Need CK: $(ACE)^+ = \{A,C,E,B,D\} = R$ → ACE is CK → add R3(A,C,E)
- **Result: R1(A,B), R2(C,D), R3(A,C,E)**

**P390.** File with 200,000 records, 50 records/block. How many block accesses for sequential scan to find 100 matching records?
- Worst case: scan all blocks = 200000/50 = 4000. Average: depends on distribution. If uniformly distributed: scan about 4000 × 100/200000 = **2 blocks per match** ≈ full scan in worst case. **4000 block accesses**

**P391.** `GRANT SELECT ON R TO user1 WITH GRANT OPTION;` — user1 can do what?
- SELECT from R + grant SELECT on R to other users

**P392.** `REVOKE SELECT ON R FROM user1 CASCADE;` — what happens to user2 who got privilege from user1?
- **user2 also loses privilege** (CASCADE effect)

**P393.** SQL Injection: `WHERE name = '${input}'` with input = `' OR '1'='1`. Result?
- Query becomes: `WHERE name = '' OR '1'='1'` → always TRUE → **returns all rows!**
- **Prevention:** Use parameterized queries / prepared statements

**P394.** ACID: which property is the programmer's responsibility?
- **Consistency** — programmer must write correct transactions. DB enforces constraints but logic is programmer's job.

**P395.** What does `EXPLAIN ANALYZE` do in PostgreSQL?
- Shows the **query execution plan** with actual timing and row counts (for optimization debugging)

**P396-P420** (Lightning round):
- **P396.** LIKE '_a%' matches? → 2nd char is 'a', any first char, any ending
- **P397.** COALESCE(NULL, NULL, 5, NULL) = ? → **5** (first non-NULL)
- **P398.** NVL(NULL, 0) = ? → **0** (Oracle's COALESCE for 2 args)
- **P399.** `WHERE age BETWEEN 20 AND 30` includes 20 and 30? → **Yes** (inclusive)
- **P400.** GROUP_CONCAT / STRING_AGG aggregates what? → Concatenates string values
- **P401.** Can SELECT * work with GROUP BY? → Only if all non-aggregated columns in GROUP BY
- **P402.** What's a materialized view? → Pre-computed view stored on disk (refreshed periodically)
- **P403.** Bitmap index best for? → Low-cardinality columns (gender, status) in OLAP
- **P404.** R-tree indexes what? → Spatial data (polygons, rectangles)
- **P405.** GiST index? → Generalized Search Tree (PostgreSQL, supports many data types)
- **P406.** Isolation level that allows dirty reads? → **Read Uncommitted**
- **P407.** Isolation level that prevents phantom reads? → **Serializable**
- **P408.** MVCC stands for? → Multi-Version Concurrency Control
- **P409.** WAL stands for? → Write-Ahead Logging
- **P410.** ACID — D stands for? → **Durability** (committed changes survive crashes)
- **P411.** Clustered index physically orders? → **Data rows** on disk
- **P412.** Non-clustered index? → Separate structure with pointers to data rows
- **P413.** B+ tree: all operations O(?)  → **O(log n)**
- **P414.** Hash table: average operations O(?) → **O(1)**
- **P415.** Denormalization is? → Adding redundancy (violating NF) for performance
- **P416.** OLTP vs OLAP? → OLTP = transactions (normalized), OLAP = analytics (denormalized)
- **P417.** Star schema has? → Central fact table + dimension tables (OLAP)
- **P418.** Snowflake schema? → Normalized dimension tables in star schema
- **P419.** Horizontal fragmentation? → Split table by rows (different sites store different rows)
- **P420.** Vertical fragmentation? → Split table by columns (PK replicated in each fragment)

---

## 📊 DBMS Concept Map — Topic Connections

```
                    ┌──────────┐
                    │ ER Model │
                    └────┬─────┘
                         │ maps to
                    ┌────▼─────┐
              ┌─────┤ Relations├─────┐
              │     └────┬─────┘     │
              │          │           │
        ┌─────▼───┐ ┌───▼────┐ ┌───▼──────┐
        │   FDs   │ │  Keys  │ │   SQL    │
        │& Normal │ │& Super │ │  Queries │
        │  Forms  │ │  Keys  │ │          │
        └────┬────┘ └───┬────┘ └────┬─────┘
             │          │           │
        ┌────▼──────────▼───┐ ┌────▼─────┐
        │   Decomposition   │ │  Indexing │
        │ (3NF, BCNF)      │ │ B+, Hash │
        └───────────────────┘ └────┬─────┘
                                   │
                    ┌──────────────▼──────┐
                    │ Query Processing    │
                    │ & Optimization      │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │ Transactions &      │
                    │ Concurrency Control │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │ Recovery System     │
                    │ (WAL, ARIES)        │
                    └─────────────────────┘
```

---

## 📊 Topic-wise GATE Distribution (DBMS — Last 10 Years)

| Topic | 2016 | 2017 | 2018 | 2019 | 2020 | 2021 | 2022 | 2023 | 2024 | 2025 |
|---|---|---|---|---|---|---|---|---|---|---|
| Normalization/FDs | 2 | 1 | 2 | 1 | 2 | 1 | 2 | 1 | 2 | 1 |
| SQL | 1 | 2 | 1 | 2 | 1 | 1 | 1 | 2 | 1 | 2 |
| RA/TRC | 1 | 1 | 0 | 1 | 1 | 1 | 0 | 1 | 1 | 0 |
| B/B+ Trees | 1 | 0 | 1 | 1 | 0 | 1 | 1 | 0 | 1 | 1 |
| Transactions | 1 | 1 | 1 | 0 | 1 | 1 | 1 | 1 | 0 | 1 |
| ER Model | 0 | 1 | 0 | 1 | 0 | 0 | 1 | 0 | 1 | 0 |
| Recovery | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 1 | 0 | 0 |

> 🎯 **Total marks per year: 8–12.** Focus on Normalization + SQL + Transactions = 70% of marks!

---

## 📊 SQL Quick Reference Card

### DDL Commands

| Command | Purpose |
|---|---|
| CREATE TABLE | Create new table |
| ALTER TABLE | Modify table structure |
| DROP TABLE | Delete table and data |
| CREATE INDEX | Create index |
| CREATE VIEW | Create virtual table |
| TRUNCATE TABLE | Remove all rows (DDL) |

### DML Commands

| Command | Purpose |
|---|---|
| SELECT | Query data |
| INSERT | Add new rows |
| UPDATE | Modify existing rows |
| DELETE | Remove rows (DML) |

### DCL Commands

| Command | Purpose |
|---|---|
| GRANT | Give privileges |
| REVOKE | Remove privileges |

### TCL Commands

| Command | Purpose |
|---|---|
| COMMIT | Save changes |
| ROLLBACK | Undo changes |
| SAVEPOINT | Set rollback point |

### SQL Data Types (Common)

| Type | Description |
|---|---|
| INT / INTEGER | Whole numbers |
| DECIMAL(p,s) | Fixed-point (p digits, s after decimal) |
| FLOAT / REAL | Floating-point |
| CHAR(n) | Fixed-length string |
| VARCHAR(n) | Variable-length string |
| DATE | Date only |
| TIMESTAMP | Date + time |
| BOOLEAN | TRUE/FALSE |
| BLOB | Binary large object |
| TEXT | Large text |

### Aggregate Functions Quick Reference

| Function | NULL behavior | With DISTINCT |
|---|---|---|
| COUNT(*) | Counts all rows | N/A |
| COUNT(col) | Ignores NULLs | COUNT(DISTINCT col) |
| SUM(col) | Ignores NULLs | SUM(DISTINCT col) |
| AVG(col) | Ignores NULLs (both num & denom) | AVG(DISTINCT col) |
| MAX(col) | Ignores NULLs | No effect |
| MIN(col) | Ignores NULLs | No effect |

---

## 🏁 DBMS Study Strategy for GATE 2027

### Priority Order (by marks/difficulty ratio)

1. **Normalization & FDs** (2-4 marks, very predictable) — Practice 20+ problems
2. **SQL queries** (2-4 marks) — Practice NULL traps, correlated subqueries, division
3. **Transactions** (1-2 marks) — Precedence graph, 2PL, isolation levels
4. **B+ Trees** (1-2 marks) — Know formulas cold, practice order calculations
5. **Relational Algebra** (1-2 marks) — Division is key, cardinality bounds
6. **ER Model** (0-2 marks) — Conversion rules, min tables
7. **Recovery** (0-1 marks) — WAL, ARIES phases
8. **Query Optimization** (0-1 marks) — Push selections, join costs

### Common Mistakes to Avoid

| Mistake | Correct Approach |
|---|---|
| Forgetting to check ALL CKs | Systematically compute closures for all attribute sets |
| Confusing 3NF and BCNF | 3NF allows prime RHS; BCNF doesn't |
| COUNT(*) vs COUNT(col) | * counts NULLs, col doesn't |
| AVG with NULLs | NULLs excluded from both numerator AND denominator |
| NOT IN with NULLs | Always returns empty! Use NOT EXISTS |
| Lossy → lossless confusion | Common attrs must be superkey of at least one part |
| B-tree vs B+ tree formulas | B has data ptrs in formula; B+ doesn't (internal) |
| 2PL ensures recovery | No! 2PL only ensures CS. Need Strict 2PL for cascadeless |
| Thomas Write Rule = CS | No! TWR gives view serializability, not conflict |
| WHERE with aggregates | Use HAVING (WHERE is pre-group, HAVING is post-group) |

---

## 🔬 Deep Dive: Relational Calculus — TRC & DRC Explained

### Tuple Relational Calculus (TRC)

**Syntax:** $\{t \mid P(t)\}$ where $t$ is a tuple variable and $P$ is a formula.

**Safe expression:** Result is a subset of $\text{dom}(P)$ (domain of P) — always finite.

**Example 1: Simple selection**
"Find employees with salary > 50000"
$$\{t \mid \text{Employee}(t) \land t[\text{Salary}] > 50000\}$$

**Example 2: Projection**
"Find names of all employees"
$$\{t[\text{Name}] \mid \text{Employee}(t)\}$$
(Shorthand — formally need new tuple variable for result.)

**Example 3: Join equivalent**
"Find names of employees in department 'CS'"
$$\{t[\text{Name}] \mid \text{Employee}(t) \land \exists d(\text{Department}(d) \land d[\text{DeptID}] = t[\text{DeptID}] \land d[\text{DeptName}] = \text{'CS'})\}$$

**Example 4: Division (Universal quantifier)**
"Find students enrolled in ALL courses"
$$\{t[\text{SID}] \mid \text{Student}(t) \land \forall c(\text{Course}(c) \Rightarrow \exists e(\text{Enrollment}(e) \land e[\text{SID}] = t[\text{SID}] \land e[\text{CID}] = c[\text{CID}]))\}$$

Note: $\forall x(P \Rightarrow Q) \equiv \forall x(\neg P \lor Q) \equiv \neg \exists x(P \land \neg Q)$

> 🎯 **GATE trap:** TRC $\forall$ is converted to double negation: "for all X, Q(X)" = "NOT exists X such that NOT Q(X)"

### Domain Relational Calculus (DRC)

**Syntax:** $\{<x_1, x_2, ..., x_n> \mid P(x_1, ..., x_n)\}$ where $x_i$ are domain variables.

**Example: Same as TRC Example 3**
$$\{<n> \mid \exists e, s, d, dn(\text{Employee}(e, n, s, d) \land \text{Department}(d, dn) \land dn = \text{'CS'})\}$$

### TRC vs RA vs SQL Conversion

| TRC | RA | SQL |
|---|---|---|
| $\{t \mid R(t) \land P(t)\}$ | $\sigma_P(R)$ | `SELECT * FROM R WHERE P` |
| $\{t[A] \mid R(t)\}$ | $\pi_A(R)$ | `SELECT A FROM R` |
| $\exists s(R(s) \land ...)$ | $R \bowtie ...$ | `FROM R, ... WHERE ...` |
| $\forall s(R(s) \Rightarrow ...)$ | $R \div ...$ | `NOT EXISTS (... EXCEPT ...)` |
| $\{t \mid R(t) \lor S(t)\}$ | $R \cup S$ | `SELECT * FROM R UNION SELECT * FROM S` |
| $\{t \mid R(t) \land \neg S(t)\}$ | $R - S$ | `SELECT * FROM R EXCEPT SELECT * FROM S` |

> 🎯 **TRC, DRC, and RA are all equivalent in expressive power** (Codd's theorem). SQL is strictly MORE powerful (aggregates, GROUP BY, recursion).

---

## 🔬 Deep Dive: File Organization — Detailed Cost Analysis

### Heap File (Unordered)

```
|Record 5|Record 2|Record 8|Record 1|Record 4|...
  Block 1          Block 2          Block 3
```

| Operation | Cost (blocks) | Notes |
|---|---|---|
| Search (equality, key) | b/2 avg, b worst | Linear scan |
| Search (equality, non-key) | b | Must scan all (multiple matches) |
| Search (range) | b | Always full scan |
| Insert | 1 read + 1 write = 2 | Add to last block |
| Delete (after search) | 1 read + 1 write | Mark as deleted / compact |

Where b = total blocks = ⌈r/bfr⌉ (records / blocking factor)

### Sorted File (Ordered on some attribute)

| Operation | Cost (blocks) | Notes |
|---|---|---|
| Binary search | ⌈log₂(b)⌉ | On ordering attribute |
| Search (non-ordering attr) | b | Same as heap |
| Range (on ordering attr) | ⌈log₂(b)⌉ + extra blocks | Find start, then scan |
| Insert | ⌈log₂(b)⌉ + 2b/2 avg | Must shift records → expensive |
| Delete | ⌈log₂(b)⌉ + 1 | Use deletion markers |

### Indexed Sequential (with Primary Index)

| Operation | Cost (blocks) | Notes |
|---|---|---|
| Search (on indexed attr) | ⌈log₂(b_i)⌉ + 1 | Binary search on index + 1 data block |
| Search (non-indexed attr) | b | Full scan |
| Insert | ⌈log₂(b_i)⌉ + 2b/2 | Like sorted file + update index |
| With overflow: | +overflow chain length | Degrades over time → reorganize |

### B+ Tree Index

| Operation | Cost (blocks) | Notes |
|---|---|---|
| Search | h + 1 | h = height, 1 data block |
| Range | h + leaf blocks + data blocks | Follow leaf chain |
| Insert | h + (splits) + data write | Usually h + 2 or 3 |
| Delete | h + (merges) + data write | Usually h + 2 |

Where h = ⌈log_⌈p/2⌉(K)⌉ for internal nodes, K = number of search key values

### Hash File

| Operation | Cost (blocks) | Notes |
|---|---|---|
| Search (equality) | 1 (no overflow) | Direct to bucket |
| Search (range) | All buckets = b | Hash destroys order |
| Insert | 1 (no overflow) | Hash + write to bucket |
| With overflow | 1 + overflow chain | Degrades → rehash |

### Complete Comparison Table

| | Heap | Sorted | Indexed | B+ Tree | Hash |
|---|---|---|---|---|---|
| Equality search | O(n) | O(log n) | O(log n) | O(log n) | **O(1)** |
| Range search | O(n) | O(log n + k) | O(log n + k) | O(log n + k) | **O(n)** |
| Insert | **O(1)** | O(n) | O(n) | O(log n) | **O(1)** |
| Delete | O(n) | O(n) | O(n) | O(log n) | O(1) |
| Best for | Write-heavy, append | Read-heavy, sorted output | Static data, equality | **General purpose** | Equality-only |

---

## 🔬 Deep Dive: Integrity Constraints — Complete Reference

### Types of Constraints in SQL

**1. NOT NULL**
```sql
CREATE TABLE Student (
    SID INT NOT NULL,
    Name VARCHAR(50) NOT NULL
);
```

**2. UNIQUE**
```sql
ALTER TABLE Student ADD CONSTRAINT uq_email UNIQUE(Email);
```
Note: UNIQUE allows ONE NULL but no duplicate non-NULL values.

**3. PRIMARY KEY = NOT NULL + UNIQUE**

**4. FOREIGN KEY**
```sql
FOREIGN KEY (DeptID) REFERENCES Department(DeptID)
    ON DELETE CASCADE        -- delete child if parent deleted
    ON UPDATE SET NULL       -- set child FK to NULL if parent PK updated
```

**Referential Actions:**
| Action | Effect |
|---|---|
| CASCADE | Propagate change to child |
| SET NULL | Set FK to NULL in child |
| SET DEFAULT | Set FK to default in child |
| NO ACTION | Reject if child exists (default) |
| RESTRICT | Same as NO ACTION (checked immediately) |

> 🎯 **NO ACTION vs RESTRICT:** NO ACTION is deferred (checked at end of statement), RESTRICT is immediate. In practice, often the same.

**5. CHECK**
```sql
CHECK (Salary > 0 AND Salary < 10000000)
CHECK (EndDate > StartDate)
CHECK (Grade IN ('A','B','C','D','F'))
```

**6. ASSERTION (Global constraint)**
```sql
CREATE ASSERTION CheckTotalSalary
CHECK (
    (SELECT SUM(Salary) FROM Employee) < 100000000
);
```
Checked after EVERY db modification (expensive, rarely supported).

**7. TRIGGER (Procedural enforcement)**
```sql
CREATE TRIGGER CheckSalary
BEFORE UPDATE ON Employee
FOR EACH ROW
BEGIN
    IF NEW.Salary > OLD.Salary * 1.5 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Raise too large!';
    END IF;
END;
```

### Constraint Checking Timeline

```
Statement begins
    ↓
BEFORE triggers fire
    ↓
Statement executes (INSERT/UPDATE/DELETE)
    ↓
AFTER triggers fire
    ↓
IMMEDIATE constraints checked (CHECK, NOT NULL, FK with RESTRICT)
    ↓
Statement completes
    ↓
... more statements in transaction ...
    ↓
COMMIT
    ↓
DEFERRED constraints checked (FK with NO ACTION, assertions)
    ↓
If any fail → ROLLBACK
```

---

## 🔬 Deep Dive: MVCC — Multi-Version Concurrency Control

### How MVCC Works

**Key idea:** Writers don't block readers, readers don't block writers.

Each row has hidden columns: `xmin` (TX that created it), `xmax` (TX that deleted/updated it).

**Example in PostgreSQL:**
```
Transaction T1 (XID=100):  INSERT INTO Account VALUES (1, 1000);
    → Creates tuple with xmin=100, xmax=null

Transaction T2 (XID=101):  UPDATE Account SET balance=900 WHERE id=1;
    → Old tuple: xmin=100, xmax=101 (marked deleted by T2)
    → New tuple: xmin=101, xmax=null

Transaction T3 (XID=102) starts BEFORE T2 commits:
    T3 reads Account WHERE id=1
    → T3 sees tuple with xmin=100 (T1 committed before T3 started)
    → Does NOT see T2's update (T2 not committed in T3's snapshot)
    → Returns balance=1000 (SNAPSHOT ISOLATION)
```

### Snapshot Isolation vs Serializable

| Property | Snapshot Isolation | Serializable |
|---|---|---|
| Write-write conflict | Detects (first committer wins) | Detects |
| Write skew anomaly | **Not prevented** ❌ | Prevented ✅ |
| Phantom reads | Prevented (snapshot) | Prevented |
| Performance | Better | More overhead |
| Implementation | MVCC | MVCC + predicate locking |

**Write Skew Example:**
```
Constraint: Doctor1.onCall + Doctor2.onCall >= 1

T1: reads both on-call (both TRUE)
T2: reads both on-call (both TRUE)
T1: sets Doctor1.onCall = FALSE (still valid: Doctor2 is on-call in T1's snapshot)
T2: sets Doctor2.onCall = FALSE (still valid: Doctor1 is on-call in T2's snapshot)
Both commit → BOTH off-call → CONSTRAINT VIOLATED!
```
This doesn't happen under true serializability but DOES happen under snapshot isolation.

> 🎯 **GATE fact:** Snapshot isolation is NOT serializable. PostgreSQL's SERIALIZABLE mode adds Serializable Snapshot Isolation (SSI) with dependency tracking to prevent write skew.

---

## 📝 Practice Set 16: Advanced GATE Simulation (P421 – P470)

**P421.** R(A,B,C,D), FDs: {A→B, B→C, C→D, D→A}. How many CKs?
- $(A)^+$ = {A,B,C,D} = R → A is CK
- $(B)^+$ = {B,C,D,A} = R → B is CK
- $(C)^+$ = {C,D,A,B} = R → C is CK
- $(D)^+$ = {D,A,B,C} = R → D is CK
- **4 CKs** (every attribute is a CK, no non-prime attributes → **BCNF**)

**P422.** From P421: How many super keys? → Each attr is CK. By IE: |⊇A| + |⊇B| + |⊇C| + |⊇D| - |⊇AB| - ... + ... = but since every single attr is CK, EVERY non-empty subset is a superkey. **Total = $2^4 - 1 = 15$**

**P423.** R(A,B,C,D,E), FDs: {AB→C, C→D, D→B, D→E}. Find all CKs.
- A appears only on LHS → A must be in every CK.
- $(A)^+$ = {A} → not enough. $(AB)^+$ = {A,B,C,D,E} = R → AB is CK.
- $(AC)^+$ = {A,C,D,B,E} = R → AC is CK. $(AD)^+$ = {A,D,B,E,C} → need C: D→B, AB→C... $(AD)^+$ = {A,D,B,E}... AB→C → {A,B,C,D,E} = R → AD is CK.
- AB, AC, AD all CKs. Any with just A + one other? AE: $(AE)^+$ = {A,E} → no. So **CKs: AB, AC, AD**

**P424.** From P423: highest NF?
- Prime attrs: {A,B,C,D}. Non-prime: {E}.
- AB→C: LHS=superkey ✅ OR C is prime ✅ (3NF ok)
- C→D: C is prime, D is prime → both prime ✅ (3NF ok, but C not superkey → not BCNF)
- D→B: D is prime, B is prime → both prime ✅ (3NF ok, but D not superkey → not BCNF)
- D→E: D not superkey, E non-prime → **violates 3NF!** Wait: D is prime...E is non-prime... In 3NF, either LHS is superkey OR RHS is prime. E is NOT prime. D is NOT superkey. → **Violates 3NF** → **Highest NF = 2NF**
- Wait, let's check for partial deps: AB→C, is A→C? $(A)^+$ = {A}: NO. B→C? $(B)^+$ = {B}: NO. → No partial deps. → **2NF** ✅

**P425.** B+ tree: order p=4 (max 4 ptrs per internal). What's minimum keys in internal (non-root)?
- ⌈4/2⌉ = 2 pointers minimum → **1 key minimum**

**P426.** B+ tree: order p=4, leaf order p_leaf = 3. If there are 100 search key values, minimum height?
- Each leaf holds max 3-1 = 2 keys (wait, leaf order q: max q-1 record pointers? No.)
- Actually for B+ leaves with order $p_{\text{leaf}}$: max $p_{\text{leaf}}-1$ key-pointer pairs + 1 next-leaf pointer
- Min fill in leaf: ⌈(p_leaf-1)/2⌉ = ⌈2/2⌉ = 1 key per leaf
- Max keys per leaf: 3-1 = 2? Or is leaf order = max keys? (Convention varies)
- Using convention "leaf holds max $p_{\text{leaf}} - 1$ keys": leaves hold max 2 keys.
- Min leaves: ⌈100/2⌉ = 50
- Root has min 2, max 4 children. Internal has min ⌈4/2⌉=2 children.
- Height to hold 50 leaves: root→2 children→2 each = 4... need 50 → ⌈log₂(50)⌉ = 6 levels with min fan-out
- With max fan-out 4: root→4→4×4=16→16×4=64 > 50 → **height = 3** (root at level 0)

**P427.** Schedule: R1(A), W2(A), R1(A), W1(A), C1, C2. Recoverable?
- T1 reads A after W2(A) → T1 reads from T2. For recoverable: T2 must commit before T1.
- But C1 comes before C2 → **NOT recoverable!**

**P428.** Two-phase locking guarantees conflict serializability but allows what problem?
- **Deadlocks** and **cascading rollbacks** (unless strict 2PL)

**P429.** In conservative 2PL, a transaction must:
- Request ALL locks **before** starting execution → **no deadlocks** but requires knowing the read/write set in advance

**P430.** Given index: primary dense on 10,000 records, block size = 1024 bytes, record size = 100 bytes, pointer = 6 bytes, key = 10 bytes.
- bfr = ⌊1024/100⌋ = 10 records/block → total data blocks = 1000
- Index entry: key + pointer = 16 bytes. Index entries per block = ⌊1024/16⌋ = 64
- Dense: 10000 entries → ⌈10000/64⌉ = 157 index blocks
- Binary search on index: ⌈log₂(157)⌉ = 8
- Total for search: 8 + 1 = **9 block accesses**

**P431.** Same setup, sparse primary index:
- Sparse: one entry per block → 1000 entries → ⌈1000/64⌉ = 16 index blocks
- Binary search: ⌈log₂(16)⌉ = 4
- Total: 4 + 1 = **5 block accesses**

**P432.** Same setup, multilevel index on top of sparse (from P431):
- Level 1: 16 blocks → Level 2: ⌈16/64⌉ = 1 block
- Total: 1 + 1 + 1 = **3 block accesses** (Level 2 + Level 1 + data)

**P433.** 3NF synthesis for R(A,B,C,D), FDs: {A→B, B→C, A→C}.
- Minimal cover: A→C is redundant (A→B→C). Min cover: {A→B, B→C}
- Groups: R1(A,B), R2(B,C). CK = AD. Neither contains AD.
- Add R3(A,D). **Result: R1(A,B), R2(B,C), R3(A,D)**

**P434.** BCNF decomposition for same R. A→B violates BCNF (A not superkey). Decompose:
- R1(A,B), R2(A,C,D). In R2: A→C (via A→B→C)? $(A)^+$ w.r.t. projected FDs on {A,C,D}: A→C (derived). A→C: A not superkey of R2 (CK=AD). Decompose:
- R21(A,C), R22(A,D). Now R22: CK=AD, no non-trivial FDs → BCNF ✅
- **Result: R1(A,B), R21(A,C), R22(A,D)** — dependency B→C is **LOST**

**P435.** `SELECT DISTINCT A FROM R WHERE B > ANY (SELECT B FROM S);`
→ Returns A values where B is greater than AT LEAST ONE value in S.B
→ Equivalent to: B > (SELECT MIN(B) FROM S)

**P436.** `SELECT A FROM R WHERE B > ALL (SELECT B FROM S);`
→ B must be greater than EVERY value in S.B
→ Equivalent to: B > (SELECT MAX(B) FROM S)

> 🎯 ANY returns TRUE if condition is true for at least one row. ALL returns TRUE if condition is true for every row.

**P437.** `SELECT A FROM R WHERE B > ALL (SELECT B FROM S WHERE 1=0);`
→ Subquery returns empty set. **ALL over empty set = TRUE.** → Returns ALL values of A from R.

**P438.** Extendible hashing with global depth 2, hash function mod 4:
- Insert keys: 4, 8, 12, 16 – all go to bucket 0 (hash mod 4 = 0)
- Bucket overflow! Split bucket 0: increase local depth from 2 to 3, hash mod 8
- 4 mod 8 = 4, 8 mod 8 = 0, 12 mod 8 = 4, 16 mod 8 = 0
- If still overflow and local depth > global depth → **double directory** (global depth increases to 3)

**P439.** Thomas Write Rule: T1 wants to write A, but A was last written by T2 where TS(T2) > TS(T1).
- Under basic TO: T1 would be rolled back (write timestamp of A > TS(T1))
- Under TWR: T1's write is **ignored** (obsolete — T2's later value already there) → Skip T1's write, T1 continues
- TWR achieves **view serializability** (not conflict serializability)

**P440.** Log sequence for ARIES recovery:
```
LSN | TX  | Type    | PageID | PrevLSN | UndoNextLSN
1   | T1  | Update  | P1     | nil     | -
2   | T2  | Update  | P2     | nil     | -
3   | T1  | Update  | P3     | 1       | -
4   | T2  | Commit  | -      | 2       | -
5   |     | CKPT    | -      | -       | -    (active: T1)
6   | T1  | Update  | P1     | 3       | -
--- CRASH ---
```
**Analysis phase (from CKPT):** Active: {T1}. Dirty pages: {P1, P3, P1}.
**Redo phase:** Redo LSN 6 (T1 update P1) — LSN 1,3 might be already on disk if flushed before checkpoint.
**Undo phase:** T1 active → undo LSN 6, then LSN 3, then LSN 1. Write CLRs for each.

**P441–P450** — Quick-fire SQL:

**P441.** `SELECT 1+NULL` → **NULL**
**P442.** `SELECT NULL = NULL` → **NULL** (not TRUE, not FALSE)
**P443.** `SELECT NULL <> NULL` → **NULL**
**P444.** `SELECT NULL IS NULL` → **TRUE** ✅
**P445.** `SELECT NULL AND TRUE` → **NULL**
**P446.** `SELECT NULL AND FALSE` → **FALSE** (FALSE dominates AND)
**P447.** `SELECT NULL OR TRUE` → **TRUE** (TRUE dominates OR)
**P448.** `SELECT CASE WHEN NULL THEN 'yes' ELSE 'no' END` → **'no'** (NULL is not TRUE)
**P449.** `SELECT * FROM R WHERE NOT (A > 5)` when A is NULL → row excluded (NOT NULL = NULL)
**P450.** `INSERT INTO R VALUES (NULL)` on column with UNIQUE constraint → allowed (multiple NULLs depend on DBMS)

**P451.** Two transactions T1, T2 both doing: READ(A), A=A-1, WRITE(A). Initial A=10. If concurrent without control, what's the lost update problem?
- Both read A=10. Both compute 9. Both write 9. **Final A=9 instead of 8.** One update is lost!

**P452.** Same as P451 with 2PL:
- T1: Lock-X(A), Read(A)=10, A=9, Write(A)=9, Unlock(A)
- T2 must wait for T1's unlock → Read(A)=9, A=8, Write(A)=8
- **Final A=8** ✅ Correct!

**P453–P470** — True/False Rapid Fire:

| # | Statement | Answer |
|---|---|---|
| P453 | Every BCNF relation is in 3NF | **TRUE** |
| P454 | Every 3NF relation is in BCNF | **FALSE** (3NF allows prime attr on RHS) |
| P455 | BCNF decomposition always preserves FDs | **FALSE** |
| P456 | 3NF synthesis always preserves FDs | **TRUE** |
| P457 | 3NF synthesis is always lossless | **TRUE** (if CK added) |
| P458 | A relation with 2 attributes is always in BCNF | **TRUE** |
| P459 | Natural join is commutative | **TRUE** |
| P460 | Natural join is associative | **TRUE** |
| P461 | Set difference is commutative | **FALSE** |
| P462 | Selection is commutative (σ₁(σ₂(R)) = σ₂(σ₁(R))) | **TRUE** |
| P463 | B+ tree allows duplicate keys | **TRUE** |
| P464 | B-tree stores data in internal nodes | **TRUE** |
| P465 | B+ tree stores data only in leaves | **TRUE** |
| P466 | Every view is updatable | **FALSE** |
| P467 | Strict 2PL prevents cascading rollbacks | **TRUE** |
| P468 | Rigorous 2PL releases all locks at commit | **TRUE** |
| P469 | Two conflict-equivalent schedules have same precedence graph | **TRUE** |
| P470 | If precedence graph is acyclic → schedule is view serializable | **TRUE** (CS ⊂ VS) |

---

## 📊 DBMS — All Key Formulas At A Glance

### Normalization Formulas

| Formula | Use |
|---|---|
| $X^+ = \text{closure}(X, F)$ | Find all attrs determined by X |
| Super keys with CKs $K_1...K_n$: Inclusion-Exclusion | Count super keys |
| $\|⊇K_i\| = 2^{n - \|K_i\|}$ | Super keys containing $K_i$ |
| Min cover: remove extraneous + simplify | Canonical cover |

### Indexing Formulas

| Formula | Use |
|---|---|
| $bfr = \lfloor B / R \rfloor$ | Blocking factor (records per block) |
| $b = \lceil r / bfr \rceil$ | Number of data blocks |
| Index entries/block = $\lfloor B / (K+P) \rfloor$ | Index blocking factor |
| Dense index blocks = $\lceil r / \text{index\_bfr} \rceil$ | Dense index size |
| Sparse index blocks = $\lceil b / \text{index\_bfr} \rceil$ | Sparse index size |
| B+ tree height = $\lceil \log_{\lceil p/2 \rceil}(K) \rceil$ | B+ tree levels |
| B+ internal order: $p$, where $p + (p-1) \cdot K \leq B$ | Max pointers per node |
| B+ leaf order: $q$, where $(q-1)(K+P_r) + P \leq B$ | Max records per leaf |

### Transaction Formulas

| Formula | Use |
|---|---|
| $n!$ schedules for $n$ serial TXs | Serial schedule count |
| $\frac{(n_1 + n_2 + ...)!}{n_1! \cdot n_2! \cdot ...}$ | Interleaved schedules (given op counts) |
| Precedence graph cycle → not CS | Conflict serializability test |
| 2PL: growing + shrinking phases | Ensure CS |
| $\text{TS}(T_i) < \text{TS}(T_j)$: $T_i$ should appear before $T_j$ | Timestamp order |

### Query Cost Formulas

| Formula | Use |
|---|---|
| Linear search: $b_r$ blocks | Scan full relation |
| Binary search: $\lceil \log_2(b_r) \rceil$ | Sorted file search |
| Index search: $h_i + 1$ | B+ tree point query |
| NLJ: $b_r + b_r \cdot b_s$ | Block nested loop worst case |
| NLJ with M buffers: $b_r + \lceil b_r/(M-2) \rceil \cdot b_s$ | With memory |
| Sort-merge join: $b_r \log(b_r) + b_s \log(b_s) + b_r + b_s$ | Sort both + merge |
| Hash join: $3(b_r + b_s)$ | Partition + probe |

### ER-to-Relational Rules

| ER Element | Table Rule |
|---|---|
| Strong entity | One table with PK |
| Weak entity | Table with owner PK + partial key as composite PK |
| 1:1 relationship | Merge into one table OR add FK in either side |
| 1:N relationship | Add FK in N-side table |
| M:N relationship | Separate relationship table with composite PK |
| Ternary relationship | Separate table, PK = all three PKs (unless FD reduces) |
| ISA (disjoint, total) | Can push parent into children |
| ISA (overlapping) | Must keep parent + child tables |
| Aggregation | Treat aggregate as entity: PK of aggregated relationship |
| Multivalued attribute | Separate table with FK + value as PK |

---

## 🎓 Final DBMS Exam Preparation Strategy

### 2 Weeks Before Exam

| Day | Activity |
|---|---|
| Day 14 | Review FD theory, practice 10 normalization problems |
| Day 13 | Review B+ tree formulas, practice 5 tree calculations |
| Day 12 | SQL deep practice: 15 queries including NULL traps |
| Day 11 | Transactions: draw 5 precedence graphs, classify 5 schedules |
| Day 10 | ER model: convert 3 diagrams to tables |
| Day 9 | Relational algebra: practice division, find-max patterns |
| Day 8 | Mixed practice: 20 random MCQs from all topics |
| Day 7 | GATE PYQ: solve last 3 years DBMS questions |
| Day 6 | Review mistakes from Day 7-8 |
| Day 5 | Formula sheet memorization (index, B+ tree, NLJ costs) |
| Day 4 | SQL + normalization rapid practice (20 min each) |
| Day 3 | Full mock: 10 DBMS questions in 25 minutes |
| Day 2 | Review all GATE tricks, common traps |
| Day 1 | Light review of formula card only |

### Exam Day Quick Check

- NULL + anything = NULL (except AND FALSE = FALSE, OR TRUE = TRUE)
- COUNT(*) counts NULLs, COUNT(col) doesn't
- NOT IN fails with NULLs → use NOT EXISTS
- BCNF ⊂ 3NF ⊂ 2NF ⊂ 1NF (BCNF is stricter)
- 3NF synthesis preserves FDs + lossless; BCNF decomposition is lossless only
- B+ tree: data ONLY in leaves; B-tree: data in all nodes
- 2PL → CS but allows deadlock; Conservative 2PL → no deadlock
- ARIES: Analysis → Redo → Undo (Always Redo, Undo if needed)
- Hash index: O(1) equality, useless for range
- $|R \bowtie S| \leq |R| \times |S|$; with FK: $|R \bowtie S| = |R|$ (if FK side)

---

## 🔬 Deep Dive: Normalization Decision Flowchart

### How to Check Normal Form of a Relation — Step by Step

```
Step 1: Start with R(attributes), FDs: F
        ↓
Step 2: Compute minimal cover F_c
        ↓
Step 3: Find ALL candidate keys
        → Compute closure for all subsets (start from smallest)
        → Identify prime and non-prime attributes
        ↓
Step 4: Check each non-trivial FD X → Y in F_c:
        ↓
    ┌─ Is X a superkey?
    │   YES → This FD is fine for ANY normal form
    │   NO  ↓
    │   ┌─ Is Y a set of prime attributes only?
    │   │   YES → Fine for 3NF (violates BCNF)
    │   │   NO  ↓
    │   │   ┌─ Is X a proper subset of some candidate key?
    │   │   │   YES → Violates 2NF (partial dependency)
    │   │   │   NO  → It's a transitive dependency
    │   │   │          (Violates 3NF but satisfies 2NF)
    │   │   └─
    │   └─
    └─
        ↓
Step 5: The LOWEST violation determines NF:
    - All FDs have superkey LHS → BCNF
    - Some non-superkey LHS but RHS is prime → 3NF (not BCNF)
    - Some transitive dep (non-superkey LHS, non-prime RHS, but LHS not partial) → 2NF (not 3NF)
    - Some partial dep (LHS is proper subset of CK, RHS is non-prime) → 1NF (not 2NF)
```

### Decision Table

| LHS is superkey? | RHS all prime? | LHS ⊂ some CK? | Violation |
|---|---|---|---|
| YES | - | - | No violation |
| NO | YES | - | BCNF violated (but 3NF ok) |
| NO | NO | NO | 3NF violated (transitive dep) |
| NO | NO | YES | 2NF violated (partial dep) |

### Worked Example: Complete NF Analysis

$R(A, B, C, D, E)$, FDs: $\{AB \to CD, A \to E, B \to D\}$

**Step 1:** Find CKs.
- Only A and B appear only on LHS → AB must be in every CK (no other attrs can derive A or B).
- $(AB)^+ = \{A,B,C,D,E\} = R$ → **CK = AB**

**Step 2:** Prime = {A, B}, Non-prime = {C, D, E}

**Step 3:** Check each FD:
- $AB \to CD$: AB is CK (superkey) → **OK** ✅
- $A \to E$: A is NOT superkey. E is non-prime. A ⊂ AB (proper subset of CK) → **Partial dependency!** → Violates 2NF
- $B \to D$: B is NOT superkey. D is non-prime. B ⊂ AB (proper subset of CK) → **Partial dependency!** → Violates 2NF

**Answer: 1NF (not 2NF)** due to partial dependencies.

---

## 🔬 Deep Dive: SQL Window Functions — Complete Guide

### Syntax

```sql
function_name(args) OVER (
    [PARTITION BY col1, col2, ...]
    [ORDER BY col3, col4 ...]
    [frame_clause]
)
```

### ROW_NUMBER, RANK, DENSE_RANK

```
Data: (Dept=CS, Sal=90), (CS, 80), (CS, 80), (CS, 70)

ROW_NUMBER() OVER (PARTITION BY Dept ORDER BY Sal DESC):
    90 → 1, 80 → 2, 80 → 3, 70 → 4  (always unique)

RANK() OVER (PARTITION BY Dept ORDER BY Sal DESC):
    90 → 1, 80 → 2, 80 → 2, 70 → 4  (skip after tie)

DENSE_RANK() OVER (PARTITION BY Dept ORDER BY Sal DESC):
    90 → 1, 80 → 2, 80 → 2, 70 → 3  (no skip)
```

> 🎯 **GATE fact:** ROW_NUMBER assigns unique sequential integers. RANK skips ranks. DENSE_RANK doesn't skip.

### LEAD and LAG

```sql
-- Previous row's salary
LAG(Salary, 1, 0) OVER (ORDER BY EmpID)

-- Next row's salary
LEAD(Salary, 1, 0) OVER (ORDER BY EmpID)
```
Use case: compare current row with previous/next row (e.g., day-over-day change).

### Running Aggregates with Windows

```sql
-- Running total
SUM(Sales) OVER (ORDER BY Date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)

-- Moving average (last 3 rows)
AVG(Sales) OVER (ORDER BY Date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)
```

### Frame Specification

| Frame | Meaning |
|---|---|
| ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW | All rows from start to current |
| ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING | Window of 3 rows |
| ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING | Current to end |
| RANGE BETWEEN ... | Value-based (not position-based) |

### Complete Example: Rank Within Group

```sql
-- Find 2nd highest salary in each department
SELECT * FROM (
    SELECT Name, Dept, Salary,
           DENSE_RANK() OVER (PARTITION BY Dept ORDER BY Salary DESC) as rnk
    FROM Employee
) sub
WHERE rnk = 2;
```

---

## 🔬 Deep Dive: NoSQL vs SQL (Bonus Topic for GATE DA)

### Comparison

| Feature | SQL (RDBMS) | NoSQL |
|---|---|---|
| Data Model | Tables (fixed schema) | JSON/Document, Key-Value, Graph, Column-family |
| Schema | Fixed (schema-on-write) | Flexible (schema-on-read) |
| Scaling | Vertical (scale up) | Horizontal (scale out) |
| ACID | ✅ Full support | Eventual consistency (BASE) |
| Joins | Supported | Usually not (denormalized) |
| Query Language | SQL | API-specific / no standard |
| Best for | Structured, relational | Unstructured, high-volume, flexible |

### CAP Theorem

Distributed systems can guarantee only 2 of 3:
- **C**onsistency (all nodes see same data)
- **A**vailability (every request gets a response)
- **P**artition tolerance (system works despite network splits)

```
         C
        / \
       /   \
  CA(impossible)  CP
     /       \
    A ─── AP ─── P
```

> 🎯 In practice, P is unavoidable in distributed systems → choose between CP (consistency, e.g., HBase, MongoDB with majority read concern) and AP (availability, e.g., Cassandra, DynamoDB).

### BASE vs ACID

| ACID | BASE |
|---|---|
| Atomicity | **B**asically **A**vailable |
| Consistency | **S**oft state |
| Isolation | **E**ventual consistency |
| Durability | |

---

## 📝 Practice Set 17: DBMS Comprehensive Final (P471 – P540)

**P471.** R(A,B,C,D,E,F), FDs: {A→B, C→D, E→F}. CK? Neither A,C,E appears on RHS → ACE must be in CK. $(ACE)^+ = R$ → **CK = ACE**

**P472.** From P471: how many super keys? One CK of size 3, total 6 attrs → $2^{6-3} = 2^3 = $ **8**

**P473.** From P471: 3NF synthesis? Min cover = {A→B, C→D, E→F}. R1(A,B), R2(C,D), R3(E,F). None contains CK → add R4(A,C,E). **4 tables.**

**P474.** If a relation has only ONE candidate key, can it fail to be in BCNF?
- Only if there's an FD X→Y where X is not a superkey. With one CK, prime attrs = CK attrs.
- If X contains non-prime attrs that determine something → possible.
- Example: R(A,B,C), CK=A, FD: B→C. B not superkey → **YES, can fail BCNF**

**P475.** $\pi_{A,B}(R) \bowtie \pi_{B,C}(R)$ — is this always equal to $R$?
- **NO!** Extra spurious tuples may appear (lossy join). Equal only if B→A or B→C.

**P476.** Given: Table R has 500 blocks. Hash table S has 200 blocks. Buffer has 52 frames. Block NLJ cost?
- R as outer: 500 + ⌈500/(52-2)⌉ × 200 = 500 + 10 × 200 = **2500**
- S as outer: 200 + ⌈200/50⌉ × 500 = 200 + 4 × 500 = **2200** ← cheaper, use S as outer

**P477.** Sort-merge join cost for R (500 blocks) and S (200 blocks)?
- Sort R: 2 × 500 × ⌈log₅₀(500/52)⌉ passes... simplified: $\approx 2 \times 500 \times \lceil\log_{52}(500)\rceil$
- Sort S: similar for 200 blocks
- Final merge: 500 + 200 = 700
- Approximate total: $\sim 500 \times 4 + 200 \times 4 + 700 = 3500$ (varies with memory)

**P478.** Which SQL clause prevents duplicate rows? → **DISTINCT**

**P479.** `WHERE EXISTS (SELECT NULL FROM ...)` — does this work? → **YES!** EXISTS just checks if subquery returns any rows; the SELECT list doesn't matter.

**P480.** Can a view with GROUP BY be updated? → **NO** (not updatable)

**P481.** Can a view on single table with no aggregates, no DISTINCT, no GROUP BY be updated? → **YES** (simple view)

**P482.** `TRUNCATE TABLE R` vs `DELETE FROM R` — key difference?
- TRUNCATE: DDL, cannot be rolled back (in most DBMS), faster, resets auto-increment
- DELETE: DML, can be rolled back, triggers fire, slower

**P483.** What's a self-referencing foreign key? → FK that references PK of the **same table** (e.g., Employee.ManagerID → Employee.EmpID)

**P484.** Natural join R(A,B) ⋈ S(B,C) with R having NULLs in B — what happens?
- **NULL ≠ NULL** in joins → NULL B values NEVER match → those tuples are excluded!

**P485.** `SELECT A FROM R GROUP BY A HAVING COUNT(*) > (SELECT AVG(cnt) FROM (SELECT COUNT(*) as cnt FROM R GROUP BY A) sub);`
- This finds groups where count exceeds the AVERAGE group count. **Valid SQL** ✅

**P486.** Relation with n tuples. Max tuples after CROSS JOIN with itself? → $n^2$

**P487.** Relation with n tuples. Max tuples after NATURAL JOIN with itself? → $n$ (same table, matches on all columns)

**P488.** R(A,B) has 5 tuples. S(A,C) has 8 tuples. Min tuples in R ⋈ S (natural join)?
- If no matching A values → **0**

**P489.** Same as P488, but A is FK from S to R. Min tuples?
- Each S tuple's A must exist in R → every S tuple matches at least one R tuple → Min = **8** (if A is key of R, exactly 8)

**P490.** Index on column with only 2 distinct values (e.g., Gender M/F). Which index type?
- **Bitmap index** (low cardinality → bitmap is optimal). B+ tree would be wasteful (each leaf covers 50% of data).

**P491-P510** — Normalization Speed Drill:

| # | R, FDs | CK | Highest NF |
|---|---|---|---|
| P491 | R(A,B), {} | AB | BCNF |
| P492 | R(A,B), {A→B} | A | BCNF |
| P493 | R(A,B), {A→B, B→A} | A, B | BCNF |
| P494 | R(A,B,C), {A→BC} | A | BCNF |
| P495 | R(A,B,C), {AB→C, C→A} | AB, BC | 3NF (C→A: C not SK, A prime → 3NF ok) |
| P496 | R(A,B,C), {A→B, B→C} | A | 2NF (transitive B→C, but A is CK not composite → actually 2NF ok → check: B→C, B not SK, C non-prime → 3NF violated → **2NF**) |
| P497 | R(A,B,C,D), {AB→C, AB→D} | AB | BCNF |
| P498 | R(A,B,C,D), {A→B, C→D} | AC | BCNF (A→B: A not SK ❌ — wait, CK=AC, A⊂AC → partial dep! → **1NF**) |
| P499 | R(A,B,C,D), {AB→CD, C→A} | AB, BC | 3NF (C→A: C not SK, A prime → allowed in 3NF) |
| P500 | R(A,B,C,D,E), {A→BCDE} | A | BCNF (single CK, only FD has CK as LHS) |
| P501 | R(A,B,C), {A→B, A→C, B→A} | A, B | BCNF (A→C: A is CK ✅, B→A: B is CK ✅) |
| P502 | R(A,B,C), {AB→C, A→C} | AB | BCNF? A→C: A not SK → NOT BCNF. C non-prime, A⊂AB → partial → **1NF** |
| P503 | R(A,B,C,D), {A→B, AB→C, C→D} | A (since A→B→AB→C→D) | 2NF (A→B OK A is CK, AB→C trivial since A→C, C→D: C not SK, D non-prime → **2NF not 3NF**) |
| P504 | R(A,B,C), {C→A, C→B} | C | BCNF |
| P505 | R(A,B,C,D), {A→BC, D→BC} | AD | BCNF? A→BC: A not SK → ❌ → **1NF** (partial dep, A⊂AD) |

> 🎯 **Pattern recognition:** When CK is composite (like AB) and a single attr from it determines something non-prime → **1NF** (partial dep). Very common GATE trap!

**P506-P510** — Schedule Analysis:

**P506.** S: R1(A) R2(A) W1(A) W2(A)
- Conflicts: R1W2(A), R2W1(A), W1W2(A) → T1→T2, T2→T1 → **CYCLE → Not CS**

**P507.** S: R1(A) W1(A) R2(A) W2(A)
- Conflicts: W1R2(A): T1→T2, W1W2(A): T1→T2 → No cycle → **CS, equivalent to T1T2**

**P508.** S: R1(A) R2(B) W2(A) W1(B)
- Conflicts: R1W2(A): T1→T2, R2W1(B): T2→T1 → **CYCLE → Not CS**

**P509.** S: R1(A) W2(B) W1(A) R2(B).  Conflicts: None between T1 and T2 on same item? R1(A), W1(A) both T1. W2(B), R2(B) both T2. No inter-TX conflicts on same item → **trivially CS** (equivalent to both T1T2 and T2T1)

**P510.** S: W1(A) W2(A) W3(A). "Blind writes" — precedence: T1→T2→T3. **CS, equivalent to T1T2T3**. Also view serializable. But is it recoverable? No reads → no "reads from" → trivially recoverable, cascadeless, and strict.

**P511-P520** — Index Calculations:

**P511.** 50,000 records, record size = 200 bytes, block size = 4096 bytes. bfr? → ⌊4096/200⌋ = **20**
**P512.** From P511, total blocks? → ⌈50000/20⌉ = **2500**
**P513.** Binary search on sorted file from P512? → ⌈log₂(2500)⌉ = **12**
**P514.** Primary sparse index, key=10B, ptr=6B. Index bfr? → ⌊4096/16⌋ = **256**
**P515.** From P514, index blocks? → ⌈2500/256⌉ = **10**
**P516.** Binary search on index from P515 + data access? → ⌈log₂(10)⌉ + 1 = 4 + 1 = **5**
**P517.** Multilevel index: ⌈10/256⌉ = 1 block at level 2. Levels: 1 + 1 = 2.  Search: 2 + 1 = **3**
**P518.** B+ tree on same data. Order p where p×6 + (p-1)×10 ≤ 4096 → 16p - 10 ≤ 4096 → p ≤ 256. p = **256**
**P519.** B+ leaf order: (q-1)(10+6) + 6 ≤ 4096 → 16q - 10 ≤ 4096 → q ≤ 256. q = **256**
**P520.** B+ tree height for 50000 keys: root→256→256²=65536 > 50000 → **height = 2** (root + leaf). Search = 2 + 1 = **3 block accesses**

**P521-P530** — SQL Output Prediction:

**P521.**
```sql
SELECT COUNT(*) FROM (SELECT DISTINCT A FROM R) T;
```
→ Count of distinct A values in R.

**P522.**
```sql
SELECT A, SUM(B) FROM R GROUP BY A ORDER BY SUM(B) DESC LIMIT 1;
```
→ Group with highest sum of B. (**LIMIT** is MySQL/PostgreSQL; use TOP 1 in SQL Server)

**P523.**
```sql
SELECT A FROM R WHERE B = (SELECT MAX(B) FROM R);
```
→ A value(s) of row(s) with maximum B.

**P524.**
```sql
SELECT R.A, S.A FROM R, S WHERE R.A > S.A;
```
→ All (R.A, S.A) pairs where R.A > S.A (self-cross with filter). Not a self-join since R ≠ S.

**P525.**
```sql
UPDATE R SET B = B * 1.1 WHERE A IN (SELECT A FROM S);
```
→ Increase B by 10% for all R rows whose A value appears in S.

**P526.**
```sql
DELETE FROM R WHERE A NOT IN (SELECT A FROM S WHERE A IS NOT NULL);
```
→ Delete R rows whose A is not in S (safely handles NULLs with IS NOT NULL filter).

**P527.**
```sql
SELECT A, B, SUM(B) OVER (ORDER BY A) as running_sum FROM R;
```
→ Running sum of B ordered by A (window function, no GROUP BY needed).

**P528.**
```sql
INSERT INTO R SELECT * FROM S WHERE NOT EXISTS (SELECT 1 FROM R WHERE R.A = S.A);
```
→ Insert rows from S into R only if they don't already exist (upsert-like).

**P529.**
```sql
SELECT COALESCE(A, B, C, 'default') FROM R;
```
→ Returns first non-NULL among A, B, C; returns 'default' if all NULL.

**P530.**
```sql
SELECT A FROM R INTERSECT SELECT A FROM S;
```
→ A values present in BOTH R and S. Equivalent to: `SELECT DISTINCT R.A FROM R JOIN S ON R.A = S.A`

**P531-P540** — Mixed Conceptual:

**P531.** In ARIES, what are the 3 phases?
- **Analysis** (determine dirty pages, active TXs at crash), **Redo** (redo all logged operations), **Undo** (undo uncommitted TXs)

**P532.** What is a CLR (Compensation Log Record)?
- Written during UNDO phase. Records the undo of an operation. CLRs are **never undone** (redo-only).

**P533.** Fuzzy checkpoint vs sharp checkpoint?
- **Sharp:** Flush ALL dirty pages to disk, pause all TXs → expensive, simpler recovery
- **Fuzzy:** Record dirty page table and active TXs without flushing → cheaper, more complex recovery

**P534.** Two-phase commit (2PC) in distributed DB:
Phase 1: Coordinator sends PREPARE. Each site votes YES/NO.
Phase 2: If all YES → COMMIT. If any NO → ABORT.

**P535.** What is the blocking problem in 2PC?
- If coordinator crashes after sending PREPARE but before COMMIT/ABORT → sites that voted YES are **blocked** (can't decide alone).

**P536.** Solution to 2PC blocking? → **3PC** (adds pre-commit phase) — non-blocking but more expensive.

**P537.** Semi-join in distributed databases?
- Send only JOINING ATTRIBUTES from one site → other site does join → send matching tuples back
- Reduces data transfer significantly

**P538.** What's a conflict-equivalent schedule?
- Two schedules are conflict-equivalent if one can be transformed into the other by swapping **non-conflicting** operations.

**P539.** Two operations conflict if: (1) belong to different TXs, (2) access same data item, (3) at least one is a **WRITE**.

**P540.** What's the difference between a schedule being conflict serializable and being serializable?
- **Conflict serializable ⊂ View serializable ⊂ Serializable**
- CS is testable (polynomial time via precedence graph). VS is NP-complete to test.

---

## 🎯 75 Ultimate GATE DBMS Quick-Recall Facts

1. Attribute closure finds all attrs determinable from a set
2. Minimal cover: no extraneous attrs, no redundant FDs, single attr RHS
3. CKs must be incomparable under set inclusion
4. Prime attribute = member of ANY candidate key
5. 2NF: no partial dependency on any CK
6. 3NF: no transitive dependency (or RHS is prime)
7. BCNF: every non-trivial FD has superkey LHS
8. 3NF synthesis: always lossless + dependency-preserving
9. BCNF decomposition: always lossless, may lose FDs
10. Chase algorithm: tests if decomposition is lossless
11. Armstrong's axioms: reflexivity, augmentation, transitivity
12. Union, decomposition, pseudo-transitivity are derived rules
13. RA has 6 basic operations: σ, π, ∪, −, ×, ρ
14. Division answers "for all" queries
15. θ-join = cross product + selection
16. Equi-join = θ-join with = condition
17. Natural join = equi-join on common attributes + remove duplicates
18. TRC ≡ DRC ≡ RA in expressive power (Codd's theorem)
19. SQL > RA (SQL has aggregates, grouping, recursion)
20. COUNT(*) counts NULLs; COUNT(col) doesn't
21. AVG ignores NULLs in both numerator and denominator
22. NOT IN fails if subquery returns NULL → use NOT EXISTS
23. GROUP BY + HAVING: WHERE filters rows, HAVING filters groups
24. UNION removes duplicates; UNION ALL keeps them
25. View with aggregates/GROUP BY/DISTINCT is NOT updatable
26. Dense index: one entry per record
27. Sparse index: one entry per block (only for ordered files)
28. Multilevel index: index on index (reduces search levels)
29. B-tree: data pointers in all nodes
30. B+ tree: data pointers only in leaves, leaves linked
31. B+ tree internal order p: (p-1) keys, p pointers
32. B+ tree min keys (non-root internal): ⌈p/2⌉ - 1
33. B+ tree min pointers (non-root internal): ⌈p/2⌉
34. Extendible hashing: directory doubles on overflow
35. Linear hashing: controlled bucket splits
36. Heap file: O(1) insert, O(n) search
37. Hash index: O(1) equality, O(n) range
38. B+ tree: O(log n) for everything
39. Conflict: same item, different TXs, at least one write
40. Precedence graph acyclic ↔ conflict serializable
41. 2PL guarantees conflict serializability
42. Strict 2PL: hold X-locks till commit → prevents cascading rollback
43. Conservative 2PL: get all locks upfront → no deadlock
44. Timestamp ordering: older TX gets priority
45. Thomas Write Rule: skip obsolete writes → view serializable
46. Wait-Die: older waits, younger aborts
47. Wound-Wait: older wounds younger, younger waits
48. Recoverable: if Ti reads from Tj, Tj commits before Ti
49. Cascadeless: only read committed data
50. Strict: don't read OR write uncommitted data
51. ARIES: Analysis, Redo, Undo (steal/no-force)
52. WAL: write log to disk before data page
53. Force policy: flush to disk at commit (slow, no redo needed)
54. No-force: don't flush at commit (fast, need redo)
55. Steal: allow uncommitted pages to be flushed (need undo)
56. No-steal: don't flush uncommitted (no undo needed, limits buffer)
57. ER strong entity: has own PK
58. ER weak entity: depends on owner, uses partial key + owner PK
59. 1:1: put FK on either side (or merge tables)
60. 1:N: put FK on N-side
61. M:N: separate table with composite PK
62. ISA disjoint total: can merge parent into children
63. ISA overlapping: must keep parent table
64. Aggregation: treat relationship as entity
65. Multivalued attribute → separate table
66. Composite attribute → flatten into main table
67. Derived attribute → don't store (compute on query)
68. NLJ cost: $b_r + b_r \times b_s$ (worst case)
69. Hash join cost: $3(b_r + b_s)$
70. Pipelining avoids temp files; materialization creates them
71. Push selections before joins (equivalence rule)
72. Projection push-down reduces tuple width
73. Join ordering matters: smallest intermediate first
74. CAP theorem: distributed systems can have only 2 of C, A, P
75. ACID = Atomicity, Consistency, Isolation, Durability

---

## 📝 Practice Set 18: Last-Minute Drill (P541 – P580)

**P541.** R(A,B,C), F = {A→B, B→C, C→A}. How many super keys? CKs: A, B, C. All are size 1.
- |⊇A| = $2^2$ = 4, |⊇B| = 4, |⊇C| = 4
- |⊇AB| = 2, |⊇AC| = 2, |⊇BC| = 2
- |⊇ABC| = 1
- IE: 4+4+4 - 2-2-2 + 1 = **7** (out of $2^3-1 = 7$) → **every non-empty subset is a superkey!**

**P542.** R(A,B,C,D), F = {AB→C, D→B}. CK?
- $(ABD)^+$? We need D for sure (only on LHS). $(AD)^+$ = {A,D,B,C} = R → **CK = AD**
- Check: $(AB)^+$ = {A,B,C} ≠ R. $(BD)^+$ = {B,D} ≠ R. So **CK = AD** is the only one.

**P543.** From P542: NF? Prime={A,D}, Non-prime={B,C}.
- AB→C: AB not superkey, C non-prime, is AB partial? A⊂AD but B∉AD → AB ⊄ AD → NOT partial. It's neither partial nor transitive in the usual sense... but AB→C where AB is not superkey, C non-prime, AB not proper subset of any CK → **transitive dependency via extended reasoning → violates 3NF → 2NF**

**P544.** Hash index with 20 entries in each bucket and 5 buckets. Load factor?
- $\alpha = n / (b \times c)$ where n=records, b=buckets, c=capacity. If 100 records total: $\alpha = 100/(5 \times 20) = 1.0$ → fully loaded. Overflow likely!

**P545.** When should you prefer clustered index over non-clustered?
- When frequent **range queries** on the indexed column (data physically sorted → sequential reads)

**P546.** Maximum number of clustered indexes on a table? → **ONE** (data can be physically sorted only one way)

**P547.** Maximum number of non-clustered indexes? → **Multiple** (limited by DBMS, typically 999 in SQL Server)

**P548.** `SELECT * FROM R NATURAL JOIN S` vs `SELECT * FROM R JOIN S USING(common_cols)` — difference?
- **NATURAL:** automatically joins on ALL common column names (dangerous if unexpected matches)
- **USING:** explicitly specifies which columns to join on (safer)

**P549.** What happens with `SELECT * FROM R CROSS JOIN S WHERE R.A = S.A`?
- **Same as inner join!** Cross join + where = theta join = inner join when condition is equality

**P550.** In RA, can Cartesian product produce fewer tuples than either input?
- **NO!** $|R \times S| = |R| \times |S|$. Always ≥ both (assuming neither is empty). If R is empty → result is 0.

**P551.** R has 100 tuples, S has 0 tuples. $|R \times S|$ = ? → **0**

**P552.** R has 100 tuples, S has 0 tuples. $|R \cup S|$ = ? → **100**

**P553.** R has 100 tuples, S has 0 tuples. $|R - S|$ = ? → **100**

**P554.** R has 100 tuples, S has 0 tuples. $|R \cap S|$ = ? → **0**

**P555.** R has 100 tuples, S has 0 tuples. $|R \bowtie S|$ = ? → **0** (no matching tuples)

**P556.** R has 100 tuples, S has 100 tuples, both have identical schema and rows. $|R \cup S|$ = ? → **100** (set union removes duplicates)

**P557.** Same as P556: $|R \cup_{all} S|$ = ? → **200** (multiset/bag union keeps all)

**P558.** What's the difference between $\sigma_{A>5}(R \cup S)$ and $\sigma_{A>5}(R) \cup \sigma_{A>5}(S)$?
- **Same result!** Selection distributes over union: $\sigma_p(R \cup S) = \sigma_p(R) \cup \sigma_p(S)$

**P559.** Does selection distribute over set difference?
- **YES:** $\sigma_p(R - S) = \sigma_p(R) - \sigma_p(S) = \sigma_p(R) - S$

**P560.** Can projection be pushed below join?
- Only if projecting attrs include join attrs: $\pi_{A,B}(R \bowtie_{R.B=S.B} S)$ can be computed as $\pi_{A,B}(\pi_{A,B}(R) \bowtie \pi_{B}(S))$

**P561-P570** — True/False Quick:

| # | Statement | T/F |
|---|---|---|
| P561 | Empty relation is in BCNF | **TRUE** |
| P562 | A relation with only 1 tuple is in BCNF | **TRUE** |
| P563 | A relation with all key attributes is in BCNF | **TRUE** (no non-prime attrs to worry about) |
| P564 | 4NF ⊂ BCNF ⊂ 3NF ⊂ 2NF ⊂ 1NF | **TRUE** (stricter ⊂ relaxed) |
| P565 | Every binary relation (2 attrs) is in BCNF | **TRUE** |
| P566 | Primary index is always sparse | **TRUE** (one entry per block for dense key) — actually convention: primary index can be sparse since file is sorted. Standard answer: sparse ✅ |
| P567 | Secondary index must be dense | **TRUE** (data file not sorted on this attr) |
| P568 | Clustering index is on non-key attribute | **TRUE** (groups of records with same value) |
| P569 | B+ tree can have variable-length keys | **TRUE** (in practice, though GATE assumes fixed) |
| P570 | Hash-based index supports ORDER BY | **FALSE** (hash destroys order) |

**P571-P580** — Fill in the blank:

| # | Question | Answer |
|---|---|---|
| P571 | Relation R with n attributes has at most _____ superkeys | $2^n - 1$ |
| P572 | Two candidate keys of sizes p and q (no overlap) → min super keys | $2^{n-p} + 2^{n-q} - 2^{n-p-q}$ |
| P573 | B+ tree with p=100, height 3: max leaf-level pointers | $100^2 = 10000$ leaves |
| P574 | Sort operation on b blocks with M buffer frames takes _____ passes | $1 + \lceil\log_{M-1}(\lceil b/M \rceil)\rceil$ |
| P575 | In 2PL, the point where a TX holds maximum locks is called _____ | **Lock point** |
| P576 | Serial schedule of n transactions has _____ permutations | $n!$ |
| P577 | A schedule is _____ if no uncommitted data is read by another TX | **Cascadeless** |
| P578 | In ARIES, the log record written during undo is called a _____ | **CLR** (Compensation Log Record) |
| P579 | A weak entity's PK consists of owner PK + _____ | **Partial key** (discriminator) |
| P580 | In CAP theorem, P stands for _____ | **Partition tolerance** |

---

## 🏆 DBMS Module Summary Statistics

| Section | Practice Problems |
|---|---|
| FDs & Normalization | P1-P20, P301-P340, P421-P424, P491-P505, P541-P543 |
| SQL | P21-P40, P441-P450, P521-P530 |
| Relational Algebra | P41-P55, P550-P560 |
| B/B+ Trees & Indexing | P56-P70, P376-P377, P511-P520, P544-P547 |
| Transactions | P71-P90, P427-P430, P506-P510 |
| ER Model | P91-P105 |
| Mixed/Cross-Topic | P106-P220, P341-P370, P381-P420, P471-P490 |
| True/False & Speed | P161-P190, P251-P300, P453-P470, P561-P580 |
| **Total** | **580 practice problems** |

| Content Type | Count |
|---|---|
| Worked Examples | 30+ (WE1-WE12 + topic-specific WEs) |
| Deep Dives | 15+ (covering all major topics) |
| GATE Tricks | 100+ individual tips |
| Comparison Tables | 20+ |
| Formula References | 3 comprehensive cards |
| ASCII Diagrams | 10+ |

---

## 🎯 Parting Wisdom — DBMS for GATE 2027

> "Normalization is not about making things normal — it's about making them **predictable**."

**Remember the hierarchy:**
- **1NF:** Atomic values, no repeating groups
- **2NF:** No partial dependencies on composite CK
- **3NF:** No transitive dependencies (or RHS is prime)  
- **BCNF:** Every determinant is a superkey — period.
- **4NF:** No non-trivial multivalued dependencies
- **5NF:** No join dependencies that aren't implied by CKs

**The three pillars of DBMS in GATE:**
1. 📐 **Design** — ER modeling → Relational schema → Normalization
2. 🔍 **Querying** — RA, TRC, SQL (know all three and conversion)
3. 🔒 **Reliability** — Transactions, Concurrency, Recovery

**If you mastered this module, you can confidently solve ANY GATE DBMS question.**

Good luck! 🍀

---

*End of Detailed Notes — DBMS for GATE 2027*
