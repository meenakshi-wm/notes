# DBMS — Questions & Previous Year Questions (GATE 2027)

> **75+ questions** covering all modules. Answers in `3_Solutions.md`.
> Difficulty: ★ (Easy/1-mark), ★★ (Medium/2-mark), ★★★ (Hard/2-mark)

---

## Section A: Relational Model & Keys (Q1–Q10)

**Q1.** ★ Consider a relation $R(A, B, C, D, E)$ with FDs: $\{A \to BC, CD \to E, B \to D\}$. Find all candidate keys of $R$. How many super keys does $R$ have?

**Q2.** ★★ [GATE CSE 2023] Consider a relation $R(A, B, C, D, E)$ with FDs $\{AB \to C, C \to D, D \to A\}$. Which of the following is a candidate key?
- (a) AB
- (b) BC
- (c) ABC
- (d) Both (a) and (b)

**Q3.** ★ A relation $R$ has 6 attributes and a candidate key of size 2. How many super keys does $R$ have?

**Q4.** ★★ [GATE CSE 2012] Consider $R(A, B, C, D, E)$ with FDs: $\{A \to B, B \to C, C \to D, D \to E\}$. How many candidate keys does $R$ have?

**Q5.** ★ Which of the following is TRUE about keys?
- (a) Every relation has exactly one candidate key
- (b) A primary key can contain NULL values
- (c) Every super key is a candidate key
- (d) Every candidate key is a super key

**Q6.** ★★ Relation $R(A,B,C,D)$ with FDs: $\{AB \to CD, D \to A\}$. Find all candidate keys and count the super keys.

**Q7.** ★ A relation has $n$ attributes and all $n$ attributes together form the only candidate key. How many super keys exist?

**Q8.** ★★ [GATE CSE 2016] Consider $R(A,B,C,D,E,F)$ with FDs: $\{A \to B, B \to C, C \to A, D \to E, CF \to D\}$. How many candidate keys does $R$ have?

**Q9.** ★ The referential integrity constraint requires that:
- (a) A primary key cannot be NULL
- (b) A foreign key value must match a primary key value in the referenced table or be NULL
- (c) Every table must have a foreign key
- (d) All attributes must have the same domain

**Q10.** ★★ Consider a schema with two tables: `Employee(eid, name, dept_id)` and `Department(dept_id, dname)`. If `dept_id` in Employee is a foreign key referencing Department with `ON DELETE CASCADE`, what happens when a department is deleted?

---

## Section B: Functional Dependencies & Normalization (Q11–Q28)

**Q11.** ★★ Given $R(A,B,C,D)$ with FDs: $F = \{A \to B, B \to C, A \to C, AB \to D\}$. Find the minimal cover of $F$.

**Q12.** ★ [GATE CSE 1987] Let $R(A,B,C)$ with FDs $\{A \to B, B \to C, C \to A\}$. The highest normal form of $R$ is:
- (a) 1NF
- (b) 2NF
- (c) 3NF
- (d) BCNF

**Q13.** ★★ [GATE CSE 2020] Consider $R(A,B,C,D,E)$ with FDs: $\{AB \to CD, C \to E, A \to B\}$. The relation is in:
- (a) 1NF but not 2NF
- (b) 2NF but not 3NF
- (c) 3NF but not BCNF
- (d) BCNF

**Q14.** ★★★ [GATE CSE 2007] Consider $R(A,B,C,D,E)$ with FDs $\{A \to B, BC \to D, D \to E\}$.
- (a) Find all candidate keys.
- (b) What is the highest normal form?

**Q15.** ★★ $R(A,B,C,D)$ with FDs: $\{A \to B, B \to C, C \to D\}$. Identify the normal form and decompose into BCNF.

**Q16.** ★ A relation is in 2NF but not in 3NF. Which type of dependency must exist?
- (a) Partial dependency
- (b) Transitive dependency
- (c) Multivalued dependency
- (d) Join dependency

**Q17.** ★★★ [GATE CSE 2012] Let $R(A,B,C,D,E)$ with FDs: $\{AB \to C, C \to D, D \to A\}$. Which of the following is true?
- (a) R is in BCNF
- (b) R is in 3NF but not BCNF
- (c) R is in 2NF but not 3NF
- (d) R is not in 2NF

**Q18.** ★★ Given FDs: $F = \{A \to BC, B \to C, AB \to D\}$ on $R(A,B,C,D)$. Check if decomposition into $R_1(A,B)$ and $R_2(A,C,D)$ is lossless.

**Q19.** ★★ Is the decomposition of $R(A,B,C)$ with FDs $\{A \to B, B \to C\}$ into $R_1(A,B)$ and $R_2(B,C)$ dependency preserving?

**Q20.** ★★★ [GATE CSE 2014] $R(A,B,C,D,E)$ with FDs: $\{A \to B, BC \to E, ED \to A\}$. How many candidate keys are there?

**Q21.** ★★ A relation $R(A,B,C,D)$ with FDs: $\{AB \to C, C \to D, D \to B\}$. Decompose into 3NF using the synthesis algorithm.

**Q22.** ★ Which of the following is always true?
- (a) Every BCNF relation is in 3NF
- (b) Every 3NF relation is in BCNF
- (c) BCNF decomposition always preserves dependencies
- (d) 2NF eliminates all redundancy

**Q23.** ★★ [GATE IT 2005] Consider $R(A,B,C,D,E,F)$ with FDs: $\{A \to B, A \to C, CD \to E, CD \to F, B \to D\}$. What is the key? What is the highest normal form?

**Q24.** ★★★ Using the Chase test, check if the decomposition of $R(A,B,C,D)$ with FDs $\{A \to B, B \to C, C \to D\}$ into $R_1(A,B)$, $R_2(B,C)$, and $R_3(C,D)$ is lossless.

**Q25.** ★★ [GATE CSE 1990] $R(A,B,C)$ with FDs $\{AB \to C, C \to B\}$. How many candidate keys? What is the highest normal form?

**Q26.** ★★ Are the FD sets $F = \{A \to B, B \to C\}$ and $G = \{A \to B, A \to C, B \to C\}$ equivalent?

**Q27.** ★ In a lossless binary decomposition of $R$ into $R_1$ and $R_2$, which condition must hold?
- (a) $R_1 \cap R_2 = \emptyset$
- (b) $R_1 \cup R_2 = R$
- (c) $R_1 \cap R_2 \to R_1$ or $R_1 \cap R_2 \to R_2$ must be in $F^+$
- (d) Both (b) and (c)

**Q28.** ★★ $R(A,B,C,D,E)$ with FDs $\{A \to BC, C \to DE\}$. How many candidate keys exist? Decompose $R$ into BCNF. Is the decomposition dependency preserving?

---

## Section C: Relational Algebra (Q29–Q42)

**Q29.** ★ Given relations $R(A,B)$ and $S(B,C)$:
- $R = \{(1,2),(3,4)\}$, $S = \{(2,5),(4,6),(4,7)\}$
- Find $R \bowtie S$ (natural join).

**Q30.** ★★ [GATE CSE 2005] Consider $R(A,B,C)$ and $S(B,D,E)$. Express the following in RA: "Find $A$ values where for every $B$ in $S$, there exists a tuple in $R$ with the same $B$ value."

**Q31.** ★ What is the maximum number of tuples in $\pi_A(R)$ if $R$ has 100 tuples?

**Q32.** ★★ Let $R(A,B) = \{(1,2),(1,3),(2,3),(2,4)\}$ and $S(B) = \{(2),(3)\}$. Compute $R \div S$.

**Q33.** ★ $R$ has 5 tuples and $S$ has 3 tuples, with 2 tuples common. What is $|R \cup S|$, $|R \cap S|$, $|R - S|$?

**Q34.** ★★ [GATE CSE 2008] Given:
- `Student(sid, name, dept)`, `Enrolled(sid, cid, grade)`, `Course(cid, cname, credits)`
- Write an RA expression: "Names of students enrolled in all 4-credit courses."

**Q35.** ★★ What is the RA expression for LEFT OUTER JOIN of $R$ and $S$ on $R.A = S.A$ using basic operators and NULL padding?

**Q36.** ★ The Cartesian product of relation $R$ with degree $a$ and cardinality $m$, and relation $S$ with degree $b$ and cardinality $n$, produces a relation with:
- (a) degree $a+b$, cardinality $m+n$
- (b) degree $a+b$, cardinality $m \times n$
- (c) degree $a \times b$, cardinality $m \times n$
- (d) degree $a \times b$, cardinality $m + n$

**Q37.** ★★ Express using RA: "Find students who are enrolled in course C1 but NOT in course C2," given `Enrolled(sid, cid)`.

**Q38.** ★★ [GATE CSE 2015] Given $R(A,B,C)$:

| A | B | C |
|---|---|---|
| 1 | 2 | 3 |
| 4 | 2 | 5 |
| 1 | 3 | 3 |

What is $\pi_A(\sigma_{B=2}(R))$?

**Q39.** ★★ How many rows does $R \bowtie S$ produce if $R$ has 1000 tuples, $S$ has 500 tuples, and the join attribute in $R$ is a foreign key referencing $S$?

**Q40.** ★ Which RA operation corresponds to SQL `SELECT DISTINCT`?

**Q41.** ★★★ Express in RA: "Find the department(s) with the maximum number of employees" using `Employee(eid, name, dept_id)`.

**Q42.** ★★ Given $R(A,B,C)$, write an RA expression to find tuples where attribute $A$ has the maximum value.

---

## Section D: Tuple Relational Calculus (Q43–Q48)

**Q43.** ★★ [GATE CSE 1999] Consider the TRC expression:
$$\{t \mid \exists r \in R (t[A] = r[A] \land \neg \exists s \in S (s[B] = r[B]))\}$$
What does this expression return?

**Q44.** ★★ [GATE CSE 2007] Write a TRC expression: "Find names of students enrolled in every course offered by the CS department."

**Q45.** ★ Convert the following RA to TRC: $\pi_{A}(\sigma_{B>5}(R))$

**Q46.** ★★ [GATE CSE 2002] Which of the following TRC expressions is NOT safe?
- (a) $\{t \mid t \in R\}$
- (b) $\{t \mid \neg(t \in R)\}$
- (c) $\{t \mid t \in R \land t[A] > 5\}$
- (d) $\{t \mid \exists s \in R(t[A] = s[A])\}$

**Q47.** ★★ Write TRC for: "Find all customers who have accounts at every branch located in Mumbai" given: `Account(acct_no, branch_name, balance)`, `Depositor(cust_name, acct_no)`, `Branch(branch_name, city)`.

**Q48.** ★★ [GATE CSE 2008] Interpret the TRC expression:
$$\{t \mid \exists r \in \text{Employee}(\forall s \in \text{Project}(\exists u \in \text{WorksOn}(u[\text{eid}]=r[\text{eid}] \land u[\text{pid}]=s[\text{pid}])) \land t[\text{name}]=r[\text{name}])\}$$

---

## Section E: SQL (Q49–Q62)

**Q49.** ★ What is the output of:
```sql
SELECT COUNT(*), COUNT(salary) FROM Employee;
```
if Employee has 10 rows and 3 rows have NULL salary?

**Q50.** ★★ [GATE CSE 2017] Consider `Employee(eid, name, dept, salary)`. What does this query return?
```sql
SELECT dept, AVG(salary) FROM Employee
GROUP BY dept
HAVING COUNT(*) > 5
ORDER BY AVG(salary) DESC;
```

**Q51.** ★★ What is the result of:
```sql
SELECT * FROM R WHERE A IN (SELECT A FROM S);
```
vs.
```sql
SELECT * FROM R WHERE A = ANY (SELECT A FROM S);
```

**Q52.** ★ `NULL = NULL` evaluates to:
- (a) TRUE
- (b) FALSE
- (c) UNKNOWN
- (d) Error

**Q53.** ★★ [GATE CSE 2019] Given tables `Student(sid, name, age)` and `Enrollment(sid, course_id)`:
```sql
SELECT name FROM Student
WHERE NOT EXISTS (
    SELECT course_id FROM Course
    WHERE NOT EXISTS (
        SELECT * FROM Enrollment
        WHERE Enrollment.sid = Student.sid AND Enrollment.course_id = Course.course_id
    )
);
```
What does this query return?

**Q54.** ★ What is the difference between `WHERE` and `HAVING`?

**Q55.** ★★ Given `R(A,B)` with values $\{(1,\text{NULL}),(2,3),(NULL,4)\}$:
- What is the result of `SELECT SUM(A), SUM(B), COUNT(*), COUNT(A) FROM R`?

**Q56.** ★★ [GATE CSE 2016] What is the output of:
```sql
SELECT A, COUNT(*) FROM R GROUP BY A ORDER BY A;
```
where $R(A,B) = \{(1,2),(1,3),(NULL,4),(NULL,5),(2,NULL)\}$?

**Q57.** ★★ Write a single SQL query to find the second highest salary from `Employee(eid, name, salary)`.

**Q58.** ★ Which SQL clause is used to remove duplicates?
- (a) UNIQUE
- (b) DISTINCT
- (c) REMOVE DUPLICATES
- (d) GROUP BY

**Q59.** ★★★ Express the division operation in SQL: "Find student names enrolled in ALL courses" using `Student(sid, name)`, `Course(cid)`, `Enrollment(sid, cid)`.

**Q60.** ★★ What is the difference in results between:
```sql
SELECT COUNT(DISTINCT dept) FROM Employee;
```
and
```sql
SELECT DISTINCT COUNT(dept) FROM Employee;
```

**Q61.** ★ The SQL keyword for pattern matching is:
- (a) MATCH
- (b) LIKE
- (c) PATTERN
- (d) REGEX

**Q62.** ★★ [GATE CSE 2018] Given `Employee(eid, name, manager_id)` where `manager_id` references `eid`. Write a query to find names of employees whose manager earns more than them. Assume a `salary` attribute exists.

---

## Section F: File Organization & Indexing (Q63–Q70)

**Q63.** ★★ A file has 30,000 records. Each record is 100 bytes. Block size = 1024 bytes. Pointer size = 6 bytes. Key size = 10 bytes.
- (a) How many blocks does the file need?
- (b) How many levels does a multilevel primary index need?
- (c) How many block accesses for a search using the multilevel index?

**Q64.** ★ Which type of index is always dense?
- (a) Primary index
- (b) Clustering index
- (c) Secondary index on non-key
- (d) None of the above

**Q65.** ★★ [GATE CSE 2010] Consider a B+ tree of order $p = 4$ (max 4 pointers in internal node) and leaf order $p_l = 3$. The tree currently has the keys 1 through 10 inserted in order. Draw the B+ tree.

**Q66.** ★★ A B-tree has order $p = 5$. What are the minimum and maximum number of keys in (a) root node, (b) non-root internal node, (c) any node?

**Q67.** ★★★ [GATE CSE 2015] Block size = 4096 bytes, record pointer = 8 bytes, block pointer = 8 bytes, search key = 32 bytes. For a file with 1,000,000 records:
- (a) Find the order of B+ tree
- (b) How many levels?
- (c) How many block accesses for a point query?

**Q68.** ★★ A file is ordered on attribute $A$ and has a primary index. A secondary index is also built on attribute $B$. Which index would you use for:
- (a) Range query on $A$
- (b) Equality query on $B$
- (c) Range query on $B$

**Q69.** ★ In a B+ tree, data pointers are stored in:
- (a) All nodes
- (b) Internal nodes only
- (c) Leaf nodes only
- (d) Root node only

**Q70.** ★★ [GATE CSE 2019] A file has 8192 records, block size = 512 bytes, record size = 128 bytes, key field = 16 bytes, pointer = 8 bytes. Calculate the number of block accesses for a binary search on the ordered file vs. using a single-level primary index.

---

## Section G: Transactions & Concurrency Control (Q71–Q83)

**Q71.** ★ Which ACID property ensures that a transaction takes the database from one consistent state to another?
- (a) Atomicity
- (b) Consistency
- (c) Isolation
- (d) Durability

**Q72.** ★★ [GATE CSE 2009] Consider the schedule:
$S: R_1(A), R_2(A), W_1(A), W_2(A), R_1(B), R_2(B), W_1(B), W_2(B)$
- (a) Is $S$ conflict serializable? (Draw precedence graph)
- (b) Is $S$ recoverable?

**Q73.** ★★★ Consider the schedule:
$S: R_1(X), W_2(X), W_1(X), R_2(X), W_2(Y), R_1(Y)$
- (a) Is $S$ conflict serializable?
- (b) Is $S$ view serializable?
- (c) Is $S$ recoverable?

**Q74.** ★★ [GATE CSE 2014] Three transactions execute concurrently:
- $T_1$: R(A), W(A), R(B), W(B)
- $T_2$: R(A), W(A)
- $T_3$: R(B), W(B)

Schedule $S$: $R_1(A), R_2(A), W_1(A), R_3(B), W_2(A), W_3(B), R_1(B), W_1(B)$

Is $S$ conflict serializable?

**Q75.** ★ In the 2-Phase Locking protocol, the lock point is:
- (a) When the first lock is acquired
- (b) When the last lock is acquired (end of growing phase)
- (c) When the first lock is released
- (d) When the transaction commits

**Q76.** ★★ [GATE CSE 2016] Consider the following schedule with lock operations:
$S: L_1(A), R_1(A), L_2(B), R_2(B), U_1(A), L_2(A), R_2(A), W_2(A), U_2(B), U_2(A), L_1(B), W_1(B), U_1(B)$

Does this schedule follow 2PL? Is it strict 2PL?

**Q77.** ★★ In the timestamp protocol, transaction $T_5$ (TS=5) wants to write item $X$ where $R\_TS(X) = 8$ and $W\_TS(X) = 3$. What happens?

**Q78.** ★★ [GATE CSE 2011] Which of the following is true?
- (a) Every conflict serializable schedule is view serializable
- (b) Every view serializable schedule is conflict serializable
- (c) Conflict serializable = View serializable
- (d) They are incomparable

**Q79.** ★★★ Given the schedule:
$S: W_1(X), W_2(X), W_2(Y), W_3(Y), W_1(Y)$
Is this schedule view serializable? Is it conflict serializable?

**Q80.** ★★ Consider transactions $T_1$ and $T_2$ using the Wait-Die protocol:
- $T_1$ (TS=10) holds lock on $A$, requests lock on $B$
- $T_2$ (TS=5) holds lock on $B$, requests lock on $A$
What happens?

**Q81.** ★ A schedule is cascadeless if:
- (a) No transaction reads data written by an uncommitted transaction
- (b) No transaction writes data written by an uncommitted transaction
- (c) All transactions commit before any other starts
- (d) There are no deadlocks

**Q82.** ★★ [GATE CSE 2013] The schedule $S: R_1(A), W_2(A), \text{Commit}_2, R_1(A)$. Is this schedule:
- (a) Recoverable
- (b) Cascadeless
- (c) Strict

**Q83.** ★★★ Apply the Thomas Write Rule to the following scenario:
$T_1$ (TS=10) wants to write $X$. Currently: $R\_TS(X) = 5$, $W\_TS(X) = 15$. What happens? Does $T_1$ abort?

---

## Section H: ER Model (Q84–Q90)

**Q84.** ★ A weak entity set:
- (a) Has its own primary key
- (b) Depends on an identifying entity set
- (c) Always has total participation in the identifying relationship
- (d) Both (b) and (c)

**Q85.** ★★ [GATE CSE 2018] An ER diagram has entities $A$ and $B$ in a many-to-many relationship $R$ with a relationship attribute. When converted to relational model, how many tables are created?

**Q86.** ★★ Consider an ER model with:
- Entity `Student` with attributes: `sid` (key), `name`, `phone` (multivalued)
- Entity `Course` with attributes: `cid` (key), `title`
- M:N relationship `Enrolled` with attribute `grade`

How many tables are needed after ER-to-relational conversion?

**Q87.** ★ The minimum number of tables required to represent a binary one-to-many relationship (with no relationship attributes and total participation on the many side) is:
- (a) 1
- (b) 2
- (c) 3
- (d) 4

**Q88.** ★★ [GATE CSE 2015] An ER diagram has 3 entities $E_1$, $E_2$, $E_3$ in a ternary relationship. $E_1$ has a multivalued attribute. What is the minimum number of relational tables needed?

**Q89.** ★★ An ER model has entity `Employee` and entity `Dependent` (weak entity, identified by `Employee`). `Dependent` has a partial key `dname`. Convert to relational schema.

**Q90.** ★★ In an ER diagram, entity $A$ (key: $a$) has a 1:1 relationship with entity $B$ (key: $b$), with total participation on both sides. What are the different ways to convert this to relational tables? What is the minimum number of tables?

---

## Bonus: Mixed/Application Questions (Q91–Q95)

**Q91.** ★★★ A database has a B+ tree index on `Employee(salary)`. Internal node order $p = 200$, leaf order $p_l = 150$. The tree has 3 levels. What is the maximum number of records that can be indexed?

**Q92.** ★★★ Given a schedule with 3 transactions on items $X, Y, Z$, determine: (a) conflict serializability using precedence graph, (b) recoverability, (c) whether it follows strict 2PL:

$S: R_1(X), R_2(Y), W_1(Y), R_3(Z), W_2(X), W_3(Y), \text{Commit}_1, \text{Commit}_2, \text{Commit}_3$

**Q93.** ★★ $R(A,B,C,D,E)$ with FDs $\{A \to B, BC \to D, D \to E, E \to A\}$. Find all candidate keys and determine the highest normal form.

**Q94.** ★★ Write an SQL query equivalent to the relational algebra:

$$\pi_{\text{name}}(\sigma_{\text{age}>25}(\text{Employee}) \bowtie_{\text{dept\_id}=\text{dept\_id}} \sigma_{\text{location}=\text{'Delhi'}}(\text{Department}))$$

**Q95.** ★★★ A file has 1,000,000 records. Record size = 200 bytes, block size = 4096 bytes, key size = 20 bytes, pointer = 8 bytes. Compare the number of block accesses for a search using: (a) sequential scan, (b) binary search on ordered file, (c) B+ tree index.

---

*Total: 95 Questions covering all DBMS modules for GATE 2027*

---

## Section I: 4NF/5NF, MVDs, Recovery, Hashing, Query Processing (25 Questions)

**Q96.** [GATE] R(A,B,C) with MVDs: A ↠ B and A ↠ C. A is NOT a superkey. R is in:
(a) 4NF (b) BCNF but not 4NF (c) 3NF but not BCNF (d) 2NF

**Q97.** [Practice — 2 marks] R(Student, Course, Hobby) where each student can take multiple courses and have multiple hobbies independently. Identify the MVDs and decompose into 4NF.

**Q98.** [GATE] The normal form hierarchy is:
(a) 3NF ⊂ BCNF ⊂ 4NF ⊂ 5NF (b) 5NF ⊂ 4NF ⊂ BCNF ⊂ 3NF (c) 4NF ⊂ BCNF ⊂ 3NF ⊂ 5NF (d) 5NF ⊂ 3NF ⊂ BCNF ⊂ 4NF

**Q99.** [GATE] In Domain Relational Calculus, variables range over:
(a) tuples (b) relations (c) domain values (d) attributes

**Q100.** [Practice] Write a DRC expression for: "Find names of students with GPA > 8" given Student(roll, name, dept, gpa).

**Q101.** [GATE] The Write-Ahead Logging (WAL) rule states:
(a) Data must be written before the log (b) Log records must be flushed before modified data pages (c) Commit must happen before any write (d) Checkpoints must be taken before every write

**Q102.** [GATE 2020] After a system crash, the recovery manager finds the following in the log:
```
<T1, start> <T1, A, 10, 20> <T2, start> <T2, B, 30, 40> <T1, commit> <checkpoint {T2}> <T2, C, 50, 60> <crash>
```
Using undo-redo recovery, which transactions are redone and which are undone?

**Q103.** [Practice — 2 marks] In ARIES recovery, what are the three phases? What does "repeating history" mean in the redo phase?

**Q104.** [GATE] A checkpoint in a database recovery system is used to:
(a) save the entire database to disk (b) reduce the amount of log to process during recovery (c) lock all transactions (d) compress the log file

**Q105.** [GATE] At which isolation level can dirty reads occur?
(a) Read Committed (b) Repeatable Read (c) Read Uncommitted (d) Serializable

**Q106.** [Practice] Match: (1) Dirty read, (2) Non-repeatable read, (3) Phantom read with their descriptions and the minimum isolation level that prevents each.

**Q107.** [GATE] In MVCC, readers:
(a) block writers (b) are blocked by writers (c) never block and are never blocked (d) must acquire shared locks

**Q108.** [GATE] In extendible hashing, when does the directory double in size?
(a) When any bucket overflows (b) When a bucket with local depth = global depth overflows (c) When load factor exceeds 0.5 (d) After every 100 insertions

**Q109.** [Practice — 2 marks] An extendible hash table has global depth 2 (4 directory entries). Bucket for '00' has local depth 2 and overflows. What happens? What is the new global depth?

**Q110.** [GATE] In linear hashing, the split pointer:
(a) always points to the overflowing bucket (b) moves sequentially through buckets (c) points to the next bucket to be split (d) Both (b) and (c)

**Q111.** [GATE] Which join algorithm has cost $3(b_r + b_s)$ block transfers?
(a) Nested loop join (b) Sort-merge join (c) Hash join (d) Indexed nested loop join

**Q112.** [Practice — 2 marks] Relations R(5000 blocks) and S(200 blocks), memory M=52 buffers. Calculate the cost of block nested loop join with R as outer and S as inner.

**Q113.** [GATE] The most important query optimization heuristic is:
(a) Push projections early (b) Push selections before joins (c) Use hash join always (d) Avoid indices

**Q114.** [GATE] SQL views are:
(a) stored as separate tables (b) virtual tables (queries stored, not data) (c) always updatable (d) materialized by default

**Q115.** [Practice — 1 mark] Which of these is NOT a condition for a view to be updatable?
(a) Single base table (b) No aggregates (c) No GROUP BY (d) Must have a WHERE clause

**Q116.** [GATE] In an ER model, generalization is:
(a) top-down (b) bottom-up (c) horizontal (d) recursive

**Q117.** [Practice] An ISA hierarchy has Employee as superclass, with Manager and Engineer as disjoint, total specialization. Employee has: eid (key), name, salary. Manager has: bonus. Engineer has: skill. How many tables are needed with (i) separate table per entity, (ii) only sub-entity tables?

**Q118.** [GATE] A trigger in SQL is:
(a) a stored procedure called manually (b) an action automatically fired on a database event (c) a type of view (d) a constraint checked at commit time

**Q119.** [Practice — 2 marks] R(A,B,C,D,E) with MVDs: A ↠ BC and A ↠ DE. FD: A → B. Is R in 4NF? If not, decompose.

**Q120.** [GATE 2017] Consider undo-redo logging. After a crash, the log has: T1 committed, T2 did not commit. Which statement is true?
(a) Redo T1 and undo T2 (b) Redo T2 and undo T1 (c) Redo both (d) Undo both

---

*Total: 120 Questions covering all DBMS modules for GATE 2027*
