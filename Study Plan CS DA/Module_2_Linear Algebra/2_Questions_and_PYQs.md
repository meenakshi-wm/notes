# Linear Algebra — Questions & GATE PYQs

## Total Questions: 60+ (Easy → Medium → GATE Level)

---

# SECTION A: LINEAR INDEPENDENCE & DEPENDENCE

---

### Q1. [Easy]
Determine whether the vectors $\vec{v}_1 = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$ and $\vec{v}_2 = \begin{bmatrix} 2 \\ 4 \end{bmatrix}$ are linearly independent or dependent.

---

### Q2. [Easy]
Are the vectors $\begin{bmatrix} 1\\0\\0 \end{bmatrix}$, $\begin{bmatrix} 0\\1\\0 \end{bmatrix}$, $\begin{bmatrix} 0\\0\\1 \end{bmatrix}$ linearly independent?

---

### Q3. [Medium]
For what values of $k$ are the vectors $\begin{bmatrix} 1\\2\\3 \end{bmatrix}$, $\begin{bmatrix} 4\\5\\6 \end{bmatrix}$, $\begin{bmatrix} 7\\8\\k \end{bmatrix}$ linearly dependent?

---

### Q4. [Medium]
Show that any 4 vectors in $\mathbb{R}^3$ must be linearly dependent.

---

### Q5. [GATE 2017 - CS]
Let $u$ and $v$ be two vectors in $\mathbb{R}^2$ whose Euclidean norms satisfy $\|u\| = 2\|v\|$. What is the value of $\alpha$ such that $w = u + \alpha v$ bisects the angle between $u$ and $v$?

(A) 2  (B) 1/2  (C) 1  (D) -1/2

---

### Q6. [GATE Style]
If $S = \{v_1, v_2, v_3\}$ is a linearly independent set, is $S' = \{v_1 + v_2, v_2 + v_3, v_3 + v_1\}$ also linearly independent?

---

### Q7. [Medium]
Let $V_1, V_2, V_3$ be vectors in $\mathbb{R}^3$ such that $V_3 = 2V_1 - 3V_2$. What is the maximum possible rank of the matrix $[V_1 | V_2 | V_3]$?

---

# SECTION B: SYSTEM OF LINEAR EQUATIONS (Ax = b)

---

### Q8. [Easy]
Solve the system:
$$x + y = 3$$
$$2x + 3y = 8$$

---

### Q9. [Medium]
For what value of $k$ does the system have no solution?
$$x + 2y + 3z = 4$$
$$2x + 4y + 6z = k$$

---

### Q10. [GATE 2016 - CS]
Consider the system of equations:
$$x_1 + x_2 + x_3 = 1$$
$$2x_1 + 2x_2 + 2x_3 = 3$$
The number of solutions is:

(A) 0  (B) 1  (C) 2  (D) ∞

---

### Q11. [GATE 2014 - CS]
The system of linear equations:
$$2x_1 + x_2 + 3x_3 = 1$$
$$3x_1 + 2x_2 + 5x_3 = 2$$
$$-x_1 - 4x_2 + x_3 = 3$$

has:
(A) a unique solution  (B) no solution  (C) infinitely many solutions with one parameter  (D) infinitely many solutions with two parameters

---

### Q12. [GATE 2021 - CS]
Consider the following system of linear equations:
$$x_1 + x_2 = 2$$
$$x_1 + x_2 + x_3 = 6$$
$$x_1 + x_2 + x_3 + x_4 = 12$$

The number of free variables in the solution is _____.

---

### Q13. [GATE Style]
For the system $A\vec{x} = \vec{b}$ where $A$ is $4 \times 5$ and $\text{rank}(A) = 3$, $\text{rank}([A|b]) = 3$, how many free variables are in the solution?

---

### Q14. [GATE Style]
If $A$ is a $3 \times 4$ matrix with rank 2, what is the dimension of the solution space of $A\vec{x} = \vec{0}$?

---

### Q15. [Medium]
The system $A\vec{x} = \vec{b}$ has a solution for every $\vec{b} \in \mathbb{R}^m$. What can you conclude about $A$?

---

# SECTION C: RANK, GAUSSIAN ELIMINATION, ECHELON FORM

---

### Q16. [Easy]
Find the rank of:
$$A = \begin{bmatrix} 1 & 2 \\ 3 & 6 \end{bmatrix}$$

---

### Q17. [Medium]
Find the rank of:
$$A = \begin{bmatrix} 1 & 2 & 3 \\ 2 & 4 & 7 \\ 3 & 6 & 10 \end{bmatrix}$$

---

### Q18. [GATE 1994 - CS]
The rank of the matrix $\begin{bmatrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{bmatrix}$ is:

(A) 0  (B) 1  (C) 2  (D) 3

---

### Q19. [GATE Style]
If $A$ is an $n \times n$ matrix with $\text{rank}(A) = n - 1$, what is $\text{nullity}(A)$?

---

### Q20. [GATE Style]
Let $A$ be a $5 \times 7$ matrix with rank 3. Find:
(a) Dimension of null space
(b) Dimension of column space
(c) Dimension of row space
(d) Dimension of left null space

---

### Q21. [Medium]
Reduce to RREF and find rank:
$$A = \begin{bmatrix} 1 & 2 & 1 & 3 \\ 2 & 4 & 3 & 7 \\ 3 & 6 & 4 & 10 \end{bmatrix}$$

---

### Q22. [GATE Style]
If $A$ is $m \times n$, $B$ is $n \times p$, $\text{rank}(A) = n$, what is $\text{rank}(AB)$?

---

# SECTION D: DETERMINANTS

---

### Q23. [Easy]
Find $\det\begin{bmatrix} 3 & 1 \\ 2 & 4 \end{bmatrix}$.

---

### Q24. [Medium]
Find $\det\begin{bmatrix} 1 & 2 & 3 \\ 0 & 4 & 5 \\ 0 & 0 & 6 \end{bmatrix}$.

---

### Q25. [Medium]
If $A$ is $3 \times 3$ and $\det(A) = 5$, find $\det(2A)$.

---

### Q26. [GATE Style]
If $A$ is a $4 \times 4$ matrix with $\det(A) = -2$, find:
(a) $\det(A^T)$
(b) $\det(A^{-1})$
(c) $\det(A^2)$
(d) $\det(3A)$
(e) $\det(\text{adj}(A))$

---

### Q27. [GATE 2015 - CS]
Let $A$ be a $3 \times 3$ matrix with $\det(A) = 4$. Then $\det(\text{adj}(A))$ is _____.

---

### Q28. [GATE Style]
Find $\det\begin{bmatrix} a+b & b+c & c+a \\ b+c & c+a & a+b \\ c+a & a+b & b+c \end{bmatrix}$.

---

### Q29. [GATE Style]
A $3 \times 3$ matrix $A$ has eigenvalues 1, 2, 3. What is $\det(A)$?

---

# SECTION E: INVERSE OF A MATRIX & CRAMER'S RULE

---

### Q30. [Easy]
Find the inverse of $A = \begin{bmatrix} 2 & 1 \\ 5 & 3 \end{bmatrix}$.

---

### Q31. [Medium]
Using Cramer's rule, solve:
$$3x + 2y = 7$$
$$x - y = 1$$

---

### Q32. [GATE Style]
If $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$, find $A^{-1}$ using the Cayley-Hamilton theorem.

---

### Q33. [Medium]
If $A$ is orthogonal and $\det(A) = 1$, what is $A^{-1}$?

---

# SECTION F: EIGENVALUES AND EIGENVECTORS

---

### Q34. [Easy]
Find the eigenvalues of $A = \begin{bmatrix} 3 & 0 \\ 0 & 5 \end{bmatrix}$.

---

### Q35. [Medium]
Find the eigenvalues and eigenvectors of $A = \begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}$.

---

### Q36. [GATE 2016 - CS]
If the characteristic polynomial of a $3 \times 3$ matrix $A$ over reals is $\lambda^3 - 3\lambda^2 + 3\lambda - 1 = 0$, then the trace and determinant of $A$ respectively are:

(A) (3, 1)  (B) (1, 3)  (C) (3, 3)  (D) (1, 1)

---

### Q37. [GATE 2017 - CS]
Let $A$ be $n \times n$ real matrix such that $A^2 = I$ and $Y = A - I$ is a non-null matrix. Which of the following is true?

(A) $Y$ is invertible  (B) $\text{rank}(Y) = n$  (C) $\det(Y) \neq 0$  (D) $Y$ is not invertible

---

### Q38. [GATE Style]
If $A$ has eigenvalues 2, 3, 5, find the eigenvalues of:
(a) $A^2$
(b) $A^{-1}$
(c) $A + 3I$
(d) $A^3 - 2A + I$
(e) $\text{adj}(A)$

---

### Q39. [GATE 2018 - CS]
Consider a matrix $A = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 4 & -2 \\ 0 & 1 & 1 \end{bmatrix}$. Find the eigenvalues of $A$.

---

### Q40. [GATE Style]
If $A$ is $3 \times 3$ with eigenvalues 1, 1, 2, is $A$ necessarily diagonalizable?

---

### Q41. [Medium]
The matrix $A = \begin{bmatrix} 2 & 1 \\ 0 & 2 \end{bmatrix}$ has eigenvalue $\lambda = 2$ with algebraic multiplicity 2. Find the geometric multiplicity. Is $A$ diagonalizable?

---

### Q42. [GATE Style]
Which of the following matrices is NOT diagonalizable?

(A) $\begin{bmatrix} 1 & 0 \\ 0 & 2 \end{bmatrix}$ (B) $\begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}$ (C) $\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$ (D) $\begin{bmatrix} 3 & 0 \\ 0 & 3 \end{bmatrix}$

---

# SECTION G: CAYLEY-HAMILTON THEOREM

---

### Q43. [Medium]
Verify Cayley-Hamilton for $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$.

---

### Q44. [GATE Style]
Using Cayley-Hamilton, express $A^3$ in terms of $A$ and $I$ for $A = \begin{bmatrix} 1 & 2 \\ 0 & 3 \end{bmatrix}$.

---

### Q45. [Medium]
If $A$ is $2 \times 2$ with eigenvalues 3 and 5, find $A^{100}$ in terms of $A$ and $I$ (assuming diagonalizable).

---

# SECTION H: TYPES OF MATRICES

---

### Q46. [Easy]
If $A$ is idempotent ($A^2 = A$) and $3 \times 3$ with $\text{rank}(A) = 2$, what is $\text{trace}(A)$?

---

### Q47. [Medium]
If $A$ is nilpotent with $A^3 = 0$ and $A \neq 0$, what are the eigenvalues of $A$? What is $\det(A)$?

---

### Q48. [GATE Style]
If $A$ is a $3 \times 3$ skew-symmetric matrix, what is $\det(A)$?

---

### Q49. [GATE Style]
Let $A$ be a $4 \times 4$ orthogonal matrix. Which of the following is always true?
(A) $\det(A) = 1$  (B) $|\det(A)| = 1$  (C) All eigenvalues are 1  (D) $A$ is symmetric

---

### Q50. [Medium]
If $P$ is an idempotent matrix, show that $I - P$ is also idempotent.

---

# SECTION I: VECTOR SPACES, SUBSPACES, BASIS

---

### Q51. [Medium]
Is $W = \{(x, y, z) \in \mathbb{R}^3 : x + y + z = 0\}$ a subspace of $\mathbb{R}^3$? If yes, find its dimension and a basis.

---

### Q52. [GATE DA 2024]
Which of the following is a subspace of $\mathbb{R}^3$?
(A) $\{(x, y, z) : x + y = 1\}$
(B) $\{(x, y, z) : x^2 + y^2 + z^2 \leq 1\}$
(C) $\{(x, y, z) : 2x - 3y + z = 0\}$
(D) $\{(x, y, z) : x \geq 0\}$

---

### Q53. [Medium]
Find the dimension of the vector space of all $2 \times 2$ symmetric matrices.

---

### Q54. [GATE Style]
What is the dimension of the subspace of $\mathbb{P}_3$ (polynomials of degree ≤ 3) consisting of polynomials $p(x)$ such that $p(0) = 0$?

---

# SECTION J: FOUR FUNDAMENTAL SUBSPACES

---

### Q55. [Medium]
For $A = \begin{bmatrix} 1 & 2 & 3 \\ 2 & 4 & 6 \end{bmatrix}$, find bases for all four fundamental subspaces.

---

### Q56. [GATE Style]
Let $A$ be a $4 \times 6$ matrix with rank 3. What are the dimensions of the column space, null space, row space, and left null space?

---

# SECTION K: LINEAR TRANSFORMATIONS

---

### Q57. [Medium]
Is $T: \mathbb{R}^2 \to \mathbb{R}^2$ defined by $T(x, y) = (2x + y, x - 3y)$ a linear transformation? Find its matrix representation.

---

### Q58. [GATE Style]
$T: \mathbb{R}^3 \to \mathbb{R}^2$ is defined by $T(x, y, z) = (x + y, y + z)$. Find:
(a) Matrix of $T$
(b) Kernel of $T$
(c) Rank of $T$
(d) Is $T$ one-to-one? Onto?

---

### Q59. [GATE DA Style]
Let $T: \mathbb{R}^2 \to \mathbb{R}^2$ be a linear transformation such that $T(1, 0) = (2, 3)$ and $T(0, 1) = (1, -1)$. Find $T(3, 5)$.

---

### Q60. [GATE Style]
The linear transformation $T: \mathbb{R}^2 \to \mathbb{R}^2$ defined by $T(x, y) = (x + y, x + y)$ is:
(A) One-to-one and onto  (B) One-to-one but not onto  (C) Onto but not one-to-one  (D) Neither one-to-one nor onto

---

# SECTION L: DIAGONALIZATION & SIMILAR MATRICES

---

### Q61. [Medium]
Diagonalize $A = \begin{bmatrix} 4 & 1 \\ 2 & 3 \end{bmatrix}$ and find $A^{10}$.

---

### Q62. [GATE Style]
If $A$ and $B$ are similar matrices, which of the following need NOT be equal?
(A) $\det(A)$ and $\det(B)$
(B) $\text{tr}(A)$ and $\text{tr}(B)$
(C) Eigenvalues of $A$ and $B$
(D) Eigenvectors of $A$ and $B$

---

### Q63. [GATE 2019 - CS]
A real $n \times n$ matrix $A$ is said to be positive definite if $\vec{x}^T A\vec{x} > 0$ for all non-zero column vectors $\vec{x}$. Which one of the following is NOT a positive definite matrix?

(A) $\begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix}$ (B) $\begin{bmatrix} 1 & 2 \\ 2 & 1 \end{bmatrix}$ (C) $\begin{bmatrix} 3 & 1 \\ 1 & 2 \end{bmatrix}$ (D) $\begin{bmatrix} 4 & 3 \\ 3 & 5 \end{bmatrix}$

---

# SECTION M: PROJECTION & GRAM-SCHMIDT

---

### Q64. [Medium]
Find the projection of $\vec{b} = \begin{bmatrix} 1\\2\\3 \end{bmatrix}$ onto $\vec{a} = \begin{bmatrix} 1\\1\\1 \end{bmatrix}$.

---

### Q65. [Medium]
Apply Gram-Schmidt to $\vec{v}_1 = \begin{bmatrix} 1\\1\\0 \end{bmatrix}$, $\vec{v}_2 = \begin{bmatrix} 1\\2\\1 \end{bmatrix}$ to get orthogonal vectors.

---

### Q66. [GATE Style]
Find the projection matrix $P$ that projects onto the column space of $A = \begin{bmatrix} 1\\2 \end{bmatrix}$.

---

### Q67. [Medium]
If $P$ is a projection matrix, find $P^{100}$.

---

# SECTION N: SVD

---

### Q68. [Medium]
Find the singular values of $A = \begin{bmatrix} 3 & 0 \\ 4 & 0 \end{bmatrix}$.

---

### Q69. [GATE Style]
If the singular values of a $3 \times 2$ matrix $A$ are 5 and 3, what is $\text{rank}(A)$?

---

### Q70. [Medium]
Compute the full SVD of $A = \begin{bmatrix} 1 & 0 \\ 0 & 1 \\ 0 & 0 \end{bmatrix}$.

---

# SECTION O: POSITIVE DEFINITE MATRICES & BLOCK MATRICES

---

### Q71. [Medium]
Is $A = \begin{bmatrix} 2 & -1 \\ -1 & 2 \end{bmatrix}$ positive definite?

---

### Q72. [GATE Style]
A $2 \times 2$ symmetric matrix has eigenvalues 3 and -1. Is it positive definite?

---

### Q73. [Medium]
Compute $\det\begin{bmatrix} A & 0 \\ C & D \end{bmatrix}$ where $A = \begin{bmatrix} 2 & 1 \\ 0 & 3 \end{bmatrix}$ and $D = \begin{bmatrix} 1 & 0 \\ 0 & 4 \end{bmatrix}$.

---

# SECTION P: MIXED GATE PYQs & CONCEPTUAL QUESTIONS

---

### Q74. [GATE 2020 - CS]
Let $M$ be a real $4 \times 4$ matrix. Consider the following statements:
S1: $M$ has 4 linearly independent eigenvectors.
S2: $M$ has 4 distinct eigenvalues.
Which one is correct?
(A) S2 implies S1  (B) S1 implies S2  (C) Both are equivalent  (D) Neither implies the other

---

### Q75. [GATE 2022 - CS]
Let $A$ be a $3 \times 3$ matrix with eigenvalues 1, -1, and 2. The determinant of $(A^2 - I)(A - 2I)$ is _____.

---

### Q76. [GATE Style]
If $A$ is $n \times n$ and $A^2 = 0$, prove that $\text{rank}(A) \leq n/2$.

---

### Q77. [GATE 2023 - CS]
Let $A$ be a $3 \times 3$ matrix such that $\det(A - I) = 0$, $\det(A - 2I) = 0$, $\det(A + 1) = 0$, where $I$ is the identity matrix. Then $\det(A)$ is _____.

---

### Q78. [GATE Style]
Let $A$ be a $3 \times 3$ matrix with rank 2. Then the rank of $\text{adj}(A)$ is:
(A) 0  (B) 1  (C) 2  (D) 3

---

### Q79. [GATE Style]
If $A$ is a $3 \times 3$ matrix with $\det(A) = 0$ and $\text{rank}(A) = 2$, how many linearly independent solutions does $A\vec{x} = \vec{0}$ have?

---

### Q80. [GATE PYQ Style]
Let $A = \begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 2 \\ 1 & 2 & 1 \end{bmatrix}$. The number of positive eigenvalues of $A$ is _____.

---

### Q81. [GATE Style]
If $A$ and $B$ are $n \times n$ matrices such that $AB = 0$ and $B$ is invertible, what is $A$?

---

### Q82. [GATE 2024 - DA]
Let $T: \mathbb{R}^3 \to \mathbb{R}^3$ be a linear transformation with $\text{rank}(T) = 2$. Then $\text{nullity}(T)$ = _____.

---

### Q83. [GATE Style]
For a $2 \times 2$ matrix $A$, if $\text{tr}(A) = 5$ and $\det(A) = 6$, find the eigenvalues.

---

### Q84. [GATE Style - LU Decomposition]
Find the LU decomposition of $A = \begin{bmatrix} 2 & 1 & 1 \\ 4 & 3 & 3 \\ 8 & 7 & 9 \end{bmatrix}$.

---

### Q85. [GATE Style]
The eigenvalues of $AB$ and $BA$ (where $A$ is $2 \times 3$ and $B$ is $3 \times 2$):
(A) Are always identical  
(B) Have the same non-zero eigenvalues  
(C) Are unrelated  
(D) Sum to the same value

---

---

*End of Questions & PYQs — Linear Algebra for GATE 2027*
