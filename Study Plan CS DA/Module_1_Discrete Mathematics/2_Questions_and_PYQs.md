# Discrete Mathematics — Questions & GATE PYQs

## Total Questions: 80+ (Easy → Medium → GATE Level)

---

# SECTION A: PROPOSITIONAL LOGIC

---

### Q1. [Easy]
Determine the truth value of $p \rightarrow q$ when $p$ is FALSE and $q$ is FALSE.

---

### Q2. [Easy]
Which of the following is a tautology?
(A) $p \rightarrow q$  
(B) $p \vee \neg p$  
(C) $p \wedge \neg p$  
(D) $p \oplus q$

---

### Q3. [Medium]
Simplify: $(p \rightarrow q) \wedge (p \rightarrow r)$

---

### Q4. [Medium]
Show that $p \rightarrow (q \rightarrow r) \equiv (p \wedge q) \rightarrow r$

---

### Q5. [GATE 2012 - CS]
Which one of the following is NOT logically equivalent to $\neg \exists x (\forall y(\alpha) \wedge \forall z(\beta))$?

(A) $\forall x (\neg \forall y(\alpha) \vee \neg \forall z(\beta))$  
(B) $\forall x (\exists y(\neg \alpha) \vee \exists z(\neg \beta))$  
(C) $\forall x (\forall y(\neg \alpha) \vee \forall z(\neg \beta))$  
(D) $\forall x (\neg \forall y(\alpha) \vee \exists z(\neg \beta))$

---

### Q6. [GATE 2015 - CS]
Consider the following two statements:
- S1: If a candidate is known to be corrupt, then he will not be elected.
- S2: If a candidate is not known to be corrupt, he will be elected.

Which one of the following is correct?
(A) S1 implies S2  
(B) S2 implies S1  
(C) S1 is the contrapositive of S2  
(D) S1 and S2 are independent

---

### Q7. [GATE 2017 - CS]
Let $p$ and $q$ be propositions. Using only the truth table for $\neg$ and $\rightarrow$, the value of $p \wedge q$ is:
(Derive AND using only NOT and IMPLIES)

---

### Q8. [GATE Style]
"You can access the website only if you pay the subscription fee."

Let $A$ = "You can access the website", $P$ = "You pay the subscription fee".

The correct logical representation is:
(A) $A \rightarrow P$  
(B) $P \rightarrow A$  
(C) $\neg P \rightarrow \neg A$  
(D) Both (A) and (C)

---

### Q9. [GATE 2014 - CS]
Consider the statement: "Not all that glitters is gold." Predicate: glitters(x), gold(x).

Which of the following is the correct representation?
(A) $\forall x(\text{glitters}(x) \rightarrow \neg \text{gold}(x))$  
(B) $\exists x(\text{glitters}(x) \wedge \neg \text{gold}(x))$  
(C) $\neg \exists x(\text{glitters}(x) \wedge \text{gold}(x))$  
(D) $\exists x(\neg \text{glitters}(x) \wedge \text{gold}(x))$

---

### Q10. [Medium]
Determine if the following argument is valid:
- Premise 1: $p \rightarrow q$
- Premise 2: $q \rightarrow r$
- Premise 3: $\neg r$
- Conclusion: $\neg p$

---

### Q11. [GATE 2019 - CS]
For the propositional formula $(p \leftrightarrow q) \rightarrow (\neg r \rightarrow p)$, determine the number of models (satisfying assignments) over $\{p, q, r\}$.

---

### Q12. [GATE Style]
Which of the following is/are valid first-order logic formulas?
(A) $\forall x P(x) \rightarrow \exists x P(x)$  
(B) $\exists x P(x) \rightarrow \forall x P(x)$  
(C) $\forall x (P(x) \vee \neg P(x))$  
(D) $\exists x P(x) \vee \exists x \neg P(x)$

---

### Q13. [GATE 2018 - CS]
Consider the first-order logic sentence: $\forall x \exists y (P(x,y) \rightarrow Q(x,y))$.

Which one is equivalent?
(A) $\forall x \exists y (\neg P(x,y) \vee Q(x,y))$  
(B) $\forall x \forall y (\neg P(x,y) \vee Q(x,y))$  
(C) $\exists x \exists y (P(x,y) \rightarrow Q(x,y))$  
(D) $\forall x \exists y (Q(x,y) \rightarrow P(x,y))$

---

# SECTION B: SET THEORY & RELATIONS

---

### Q14. [Easy]
If $A = \{1, 2, 3\}$, find $|\mathcal{P}(\mathcal{P}(\emptyset))|$.

---

### Q15. [Easy]
If $|A| = 3$ and $|B| = 4$, how many relations are possible from $A$ to $B$?

---

### Q16. [Medium]
Let $A = \{1, 2, 3, 4\}$ and $R = \{(1,1), (2,2), (3,3), (4,4), (1,2), (2,1)\}$.

Is $R$ an equivalence relation?

---

### Q17. [GATE 2010 - CS]
The number of equivalence relations on a set of 5 elements is _____.

---

### Q18. [GATE 2014 - CS]
Consider the set $\{1, 2, 3, \ldots, 1000\}$. How many arithmetic progressions can be formed from this set that have 4 as the common difference and contain 1001 as the sum of elements?

---

### Q19. [GATE 2016 - CS]
Let $R$ be a relation on the set of ordered pairs of positive integers such that $((p,q), (r,s)) \in R$ iff $ps = qr$. Then $R$ is:

(A) Reflexive, symmetric, transitive  
(B) Only reflexive  
(C) Only transitive  
(D) An equivalence relation

---

### Q20. [GATE Style]
How many relations on a set of $n$ elements are both reflexive and symmetric?

---

### Q21. [GATE 2015 - CS]
Let $R$ be a relation on $\{1, 2, 3, 4, 5\}$ defined by $R = \{(a,b) : |a - b| \leq 1\}$. Is $R$ an equivalence relation? If not, which properties fail?

---

### Q22. [Medium]
In the POSET $(\{1, 2, 3, 4, 6, 8, 12, 24\}, |)$ where $|$ means divides:
(a) Find all maximal and minimal elements.
(b) Find LUB and GLB of $\{3, 4\}$.
(c) Is this a lattice?

---

### Q23. [GATE 2013 - CS]
Which one of the following is NOT a partial order on positive integers?
(A) $\{(a,b) : a = b\}$  
(B) $\{(a,b) : a \text{ divides } b\}$  
(C) $\{(a,b) : a \leq b\}$  
(D) $\{(a,b) : gcd(a,b) = a\}$ [Note: same as divides]

---

### Q24. [GATE 2017 - CS]
Consider the Hasse diagram of a POSET. Let max denote the number of maximal elements, and min denote the number of minimal elements. For the divisibility relation on $\{2, 3, 5, 6, 10, 15, 30\}$, what are max and min?

---

# SECTION C: FUNCTIONS

---

### Q25. [Easy]
The number of one-to-one functions from a set of 3 elements to a set of 5 elements is _____.

---

### Q26. [Medium]
How many onto functions are there from a set of 4 elements to a set of 2 elements?

---

### Q27. [GATE 2015 - CS]
Let $f: \{1,2,3,4,5\} \rightarrow \{1,2,3,4,5\}$ be such that $f(f(x)) = x$ for all $x$. How many such functions are there?

---

### Q28. [GATE Style]
If $f: A \rightarrow B$ is a bijection, $g: B \rightarrow C$ and $g \circ f$ is surjective, what can we say about $g$?

---

### Q29. [GATE 2016 - CS]
The number of functions $f$ from $\{1,2,\ldots,n\}$ to itself such that $f(f(i)) = i$ for all $i$ is _____.

---

# SECTION D: GROUP THEORY

---

### Q30. [Easy]
Is $(\mathbb{Z}_6, +_6)$ a group? Is it cyclic? Find all generators.

---

### Q31. [Medium]
Find all subgroups of $\mathbb{Z}_{12}$.

---

### Q32. [GATE 2014 - CS]
If $G$ is a group of order 35, how many subgroups can $G$ have?

---

### Q33. [GATE 2017 - CS]
Let $G$ be a group of order 49. Then $G$ is:
(A) necessarily abelian  
(B) necessarily cyclic  
(C) not necessarily abelian  
(D) abelian but not necessarily cyclic

---

### Q34. [GATE Style]
In $\mathbb{Z}_{20}^*$ (integers coprime to 20 under multiplication mod 20), find the order of the group and the order of element 3.

---

### Q35. [Medium]
Prove that every group of prime order is cyclic.

---

### Q36. [GATE 2012 - CS]
If $G$ is a finite group and $H$ is a subgroup of $G$ with $|H| = |G|/2$, then $H$ is normal in $G$. (True/False with justification)

---

# SECTION E: COMBINATORICS

---

### Q37. [Easy]
How many 4-letter words can be formed from the letters A, B, C, D, E with repetition allowed?

---

### Q38. [Easy]
In how many ways can 8 people be seated around a circular table?

---

### Q39. [Medium]
How many ways can we distribute 10 identical balls into 4 distinct boxes such that each box has at least 1 ball?

---

### Q40. [GATE 2013 - CS]
The number of onto functions from a set A with 6 elements to a set B with 2 elements is _____.

---

### Q41. [GATE 2018 - CS]
The number of permutations of the word "MISSISSIPPI" is _____.

---

### Q42. [GATE 2020 - CS]
In how many ways can we place 5 non-attacking rooks on a $5 \times 5$ chessboard?

---

### Q43. [GATE Style]
Find the number of derangements of $\{1, 2, 3, 4, 5\}$.

---

### Q44. [GATE 2014 - CS]
How many integers from 1 to 1000 are divisible by 3 or 5 or 7?

---

### Q45. [Medium]
Solve the recurrence: $a_n = 5a_{n-1} - 6a_{n-2}$, with $a_0 = 1, a_1 = 4$.

---

### Q46. [GATE 2015 - CS]
The number of binary strings of length $n$ with no two consecutive 1's is given by a Fibonacci-like recurrence. If $f(1) = 2, f(2) = 3$, find $f(6)$.

---

### Q47. [GATE Style]
Using PIE, find the number of integers from 1 to 120 that are NOT divisible by any of 2, 3, or 5.

---

### Q48. [Medium]
How many integers between 1 and 1000 (inclusive) are not divisible by 2, not divisible by 3, and not divisible by 5?

---

### Q49. [GATE 2019 - CS]
The number of ways to partition $\{1, 2, 3, 4\}$ into exactly 2 non-empty subsets is _____.

---

# SECTION F: GRAPH THEORY

---

### Q50. [Easy]
A simple graph has 6 vertices with degree sequence $(2, 2, 2, 3, 3, 4)$. How many edges does it have?

---

### Q51. [Easy]
Does $K_4$ have an Euler circuit? Euler path?

---

### Q52. [Medium]
The number of edges in a tree with 20 vertices is _____.

---

### Q53. [GATE 2013 - CS]
What is the minimum number of edges that must be removed to make $K_5$ planar?

---

### Q54. [GATE 2014 - CS]
The chromatic number of the cycle graph $C_5$ is _____.

---

### Q55. [GATE 2016 - CS]
A graph has 12 vertices and 19 edges. If it is planar, the number of faces is _____.

---

### Q56. [GATE 2018 - CS]
Consider the graph with vertices $\{1, 2, 3, 4, 5, 6\}$ and edges $\{(1,2), (1,3), (2,3), (4,5), (4,6), (5,6)\}$. The number of connected components is _____.

---

### Q57. [GATE 2017 - CS]
The number of labeled trees on 4 vertices is _____.

---

### Q58. [GATE Style]
A connected planar graph has 6 vertices and 10 edges. Find the number of faces.

---

### Q59. [GATE 2019 - CS]
For a graph $G$ with $n$ vertices that is a tree, what is the maximum number of vertices that can have degree greater than $n/2$?

---

### Q60. [GATE 2015 - CS]
Which of the following graphs is NOT planar?
(A) $K_4$  
(B) $K_{2,3}$  
(C) $K_{3,3}$  
(D) $Q_3$ (3-dimensional hypercube)

---

### Q61. [Medium]
The Petersen graph has 10 vertices and 15 edges. Is it planar?

---

### Q62. [GATE 2020 - CS]
A graph $G$ has 10 vertices, each of degree 3. The number of edges in $G$ is _____.

---

### Q63. [GATE Style]
For the graph $K_{3,3}$, verify using Euler's formula that it is non-planar.

---

### Q64. [GATE 2021 - CS]
The minimum number of colors required to color the vertices of a bipartite graph (with at least one edge) such that no two adjacent vertices have the same color is _____.

---

# SECTION G: PROOF TECHNIQUES

---

### Q65. [Easy]
Prove by mathematical induction: $1 + 2 + 3 + \cdots + n = \frac{n(n+1)}{2}$

---

### Q66. [Medium]
Prove by induction that $n^3 - n$ is divisible by 6 for all $n \geq 1$.

---

### Q67. [GATE Style]
Prove or disprove: For all $n \geq 1$, $2^n > n^2$.

---

### Q68. [Medium]
Use strong induction to prove: Every integer $n \geq 2$ can be written as a product of primes.

---

# SECTION H: MIXED GATE PYQs

---

### Q69. [GATE 2022 - CS]
Let $R$ be a relation on a set $A = \{a, b, c, d\}$ defined as $R = \{(a,b), (b,c), (c,d), (d,a)\}$. The transitive closure of $R$ has how many elements?

---

### Q70. [GATE 2023 - CS]
Consider the proposition: $(p \wedge q) \rightarrow (p \vee r)$. The number of truth assignments (models) for which this formula evaluates to TRUE over $\{p, q, r\}$ is _____.

---

### Q71. [GATE 2020 - CS]
In a group $G$, $a^5 = e$ (identity). The order of $a^{-1}$ is _____.

---

### Q72. [GATE 2021 - CS]
In how many ways can 3 men and 3 women be seated in a row such that no two women sit adjacent?

---

### Q73. [GATE 2022 - CS]
The number of spanning trees of a complete graph $K_5$ is _____.

---

### Q74. [GATE 2023 - CS]
A partial order on $\{1, 2, 3, 4, 5, 6\}$ is defined by divisibility. The number of edges in the Hasse diagram is _____.

---

### Q75. [GATE 2019 - CS]
Consider a graph with 4 vertices and 5 edges. Is it planar?

---

### Q76. [GATE Style]
Determine the chromatic polynomial of the path graph $P_3$ (3 vertices in a line) and evaluate it for $k = 3$ colors.

---

### Q77. [GATE 2020 - CS]
In the poset $(\{1,2,3,4,6,12\}, |)$, what is the LUB of $\{4, 6\}$?

---

### Q78. [GATE Style]
How many distinct equivalence relations exist on $\{1, 2, 3, 4\}$?

---

### Q79. [GATE 2023 - CS]
The number of Hamilton circuits in $K_5$ is _____.

---

### Q80. [GATE 2024 Mixed]
Consider the FOL statement: $\exists x \forall y (P(x) \rightarrow Q(y))$. Which is equivalent?
(A) $\exists x P(x) \rightarrow \forall y Q(y)$  
(B) $\forall y \exists x (P(x) \rightarrow Q(y))$  
(C) $\exists x (\neg P(x)) \vee \forall y Q(y)$  
(D) All of the above
