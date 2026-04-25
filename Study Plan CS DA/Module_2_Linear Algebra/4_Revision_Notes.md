# Linear Algebra — Short Revision Notes (Last-Day Revision)

## GATE 2027 CS/IT/DA | Quick Reference Sheet

---

## 1. LINEAR INDEPENDENCE / DEPENDENCE

- **LI:** Only trivial solution to $c_1v_1 + \cdots + c_kv_k = 0$
- **LD:** At least one $c_i \neq 0$ satisfying the above
- $k > n$ vectors in $\mathbb{R}^n$ → **always LD**
- Zero vector in set → always LD
- Two vectors LD ⟺ one is scalar multiple of other
- $n$ vectors in $\mathbb{R}^n$ are LI ⟺ $\det \neq 0$
- **LI vectors → unique linear combination**

---

## 2. SPAN, BASIS, DIMENSION

- **Span** = all linear combinations
- **Basis** = LI + Spans the space
- **dim($\mathbb{R}^n$)** = $n$
- **dim($\mathbb{P}_n$)** = $n + 1$
- **dim($\mathbb{M}_{m \times n}$)** = $mn$
- dim(symmetric $n \times n$) = $n(n+1)/2$
- dim(skew-symmetric $n \times n$) = $n(n-1)/2$

---

## 3. MATRIX MULTIPLICATION

- $(AB)_{ij}$ = Row $i$ of $A$ · Column $j$ of $B$
- $AB \neq BA$ in general
- $(AB)^T = B^T A^T$
- $AB = 0 \not\Rightarrow A = 0$ or $B = 0$

---

## 4. SYSTEM Ax = b ⭐⭐

| Condition | Solutions |
|---|---|
| $r(A) = r([A|b]) = n$ | **Unique** |
| $r(A) = r([A|b]) < n$ | **Infinite** (n − r free vars) |
| $r(A) < r([A|b])$ | **No solution** |

- Homogeneous Ax = 0: always has trivial soln; non-trivial iff rank < n
- $m < n$ (more unknowns) → Ax = 0 always has non-trivial solutions
- **General solution:** $x = x_{particular} + x_{homogeneous}$

---

## 5. RANK ⭐⭐

- rank = # pivots in REF = # non-zero rows in REF
- $\text{rank}(A) = \text{rank}(A^T)$
- $\text{rank}(A) \leq \min(m, n)$
- $\text{rank}(AB) \leq \min(\text{rank}(A), \text{rank}(B))$
- $\text{rank}(A + B) \leq \text{rank}(A) + \text{rank}(B)$
- $\text{rank}(A^TA) = \text{rank}(AA^T) = \text{rank}(A)$
- **Rank-Nullity:** $\text{rank}(A) + \text{nullity}(A) = n$ (# columns)
- If A has full column rank → $\text{rank}(AB) = \text{rank}(B)$
- If $A^2 = 0$ → $\text{rank}(A) \leq n/2$

---

## 6. DETERMINANT ⭐⭐

- $2 \times 2$: $ad - bc$
- Triangular/Diagonal: product of diagonal entries
- $\det(kA) = k^n \det(A)$
- $\det(AB) = \det(A)\det(B)$
- $\det(A^T) = \det(A)$
- $\det(A^{-1}) = 1/\det(A)$
- $\det(A^m) = [\det(A)]^m$
- $\det(\text{adj}(A)) = [\det(A)]^{n-1}$
- Row swap → sign of det changes
- $R_i + kR_j$ → det unchanged
- $kR_i$ → det multiplied by $k$
- Row of zeros or two identical rows → det = 0
- $\det \neq 0 \iff$ invertible
- Block triangular: $\det \begin{bmatrix} A & B \\ 0 & D \end{bmatrix} = \det(A)\det(D)$

---

## 7. INVERSE

- Exists iff $\det(A) \neq 0$
- $A^{-1} = \frac{1}{\det(A)} \text{adj}(A)$
- $2 \times 2$: $\frac{1}{ad-bc}\begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$
- $(AB)^{-1} = B^{-1}A^{-1}$
- $(A^T)^{-1} = (A^{-1})^T$
- Orthogonal: $A^{-1} = A^T$

---

## 8. CRAMER'S RULE

- $x_i = \det(A_i)/\det(A)$ where $A_i$ has column $i$ replaced by $b$
- Only when $\det(A) \neq 0$; practical for $2 \times 2$, $3 \times 3$

---

## 9. EIGENVALUES & EIGENVECTORS ⭐⭐⭐

- $Av = \lambda v$ ($v \neq 0$)
- Characteristic eq: $\det(A - \lambda I) = 0$
- $2 \times 2$ shortcut: $\lambda^2 - \text{tr}(A)\lambda + \det(A) = 0$

### Key Eigenvalue Properties

| Matrix/Operation | Eigenvalues |
|---|---|
| $A$ | $\lambda$ |
| $A^k$ | $\lambda^k$ |
| $A^{-1}$ | $1/\lambda$ |
| $A + cI$ | $\lambda + c$ |
| $kA$ | $k\lambda$ |
| $A^T$ | Same as A |
| $f(A)$ | $f(\lambda)$ |
| $\text{adj}(A)$ | $\det(A)/\lambda$ |

- $\sum \lambda_i = \text{tr}(A)$
- $\prod \lambda_i = \det(A)$
- $1 \leq \text{GM} \leq \text{AM}$
- Diagonalizable iff GM = AM for all eigenvalues
- Distinct eigenvalues → always diagonalizable
- **Symmetric matrix:** all real eigenvalues, orthogonal eigenvectors, always diagonalizable
- **AB and BA** have same non-zero eigenvalues

---

## 10. SPECIAL MATRICES — Eigenvalue Rules

| Type | Definition | Eigenvalues |
|---|---|---|
| Symmetric | $A = A^T$ | All real |
| Skew-symmetric | $A = -A^T$ | Pure imaginary or 0; det = 0 for odd $n$ |
| Orthogonal | $A^TA = I$ | $|\lambda| = 1$; $\det = \pm 1$ |
| Idempotent | $A^2 = A$ | $\{0, 1\}$; rank = trace |
| Nilpotent | $A^k = 0$ | All 0; det = 0, tr = 0 |
| Involutory | $A^2 = I$ | $\{-1, 1\}$ |
| Positive Definite | $x^TAx > 0$ | All > 0 |

---

## 11. CAYLEY-HAMILTON

- Every matrix satisfies its own characteristic equation: $p(A) = 0$
- Use to find $A^{-1}$, $A^n$, polynomials of $A$
- For $2\times 2$: $A^2 - \text{tr}(A) \cdot A + \det(A) \cdot I = 0$

---

## 12. LU DECOMPOSITION

- $A = LU$: L = lower triangular (1s on diagonal), U = upper triangular (echelon form)
- Multipliers in elimination go into $L$
- Works when no row swaps needed; otherwise $PA = LU$

---

## 13. FOUR FUNDAMENTAL SUBSPACES

For $m \times n$ matrix A with rank $r$:

| Space | Dimension | Lives in |
|---|---|---|
| Column space $C(A)$ | $r$ | $\mathbb{R}^m$ |
| Null space $N(A)$ | $n - r$ | $\mathbb{R}^n$ |
| Row space $C(A^T)$ | $r$ | $\mathbb{R}^n$ |
| Left null space $N(A^T)$ | $m - r$ | $\mathbb{R}^m$ |

- $C(A^T) \perp N(A)$ in $\mathbb{R}^n$
- $C(A) \perp N(A^T)$ in $\mathbb{R}^m$
- Column space basis: pivot columns of **original** A
- Row space basis: non-zero rows of **REF**

---

## 14. SUBSPACE QUICK TEST

1. Contains $\vec{0}$?
2. Closed under addition?
3. Closed under scalar multiplication?

- Intersection of subspaces → always subspace
- Union → subspace ONLY if one ⊆ other
- Solution of $Ax = 0$ → subspace; solution of $Ax = b$ ($b \neq 0$) → NOT subspace

---

## 15. LINEAR TRANSFORMATION

- $T(au + bv) = aT(u) + bT(v)$
- $T(\vec{0}) = \vec{0}$ always
- Matrix: Columns = images of basis vectors
- Rank-Nullity: $\dim(V) = \text{rank}(T) + \text{nullity}(T)$
- Injective ⟺ nullity = 0
- Surjective ⟺ rank = dim(codomain)
- $[T]_B = P^{-1}AP$ (change of basis for linear transformation)

---

## 16. SIMILAR MATRICES & DIAGONALIZATION

- $B = P^{-1}AP$: same eigenvalues, det, trace, rank, char. poly
- **Diagonalization:** $A = PDP^{-1}$ where D = diag eigenvalues, P = eigenvector matrix
- $A^k = PD^kP^{-1}$
- Diagonalizable iff $n$ LI eigenvectors iff GM = AM for all $\lambda$

---

## 17. PROJECTION & GRAM-SCHMIDT

- Projection onto $\vec{a}$: $\text{proj}_a(b) = \frac{a^Tb}{a^Ta}a$
- Projection matrix onto $C(A)$: $P = A(A^TA)^{-1}A^T$
- If columns of Q orthonormal: $P = QQ^T$
- $P^2 = P$, $P^T = P$
- $I - P$ projects onto orthogonal complement
- **Gram-Schmidt:** $u_k = v_k - \sum_{j<k} \frac{v_k \cdot u_j}{u_j \cdot u_j}u_j$

---

## 18. SVD

- $A = U\Sigma V^T$ (any matrix)
- Singular values $\sigma_i = \sqrt{\lambda_i(A^TA)}$
- rank(A) = # non-zero singular values
- $V$ = eigenvectors of $A^TA$
- $U$: $u_i = \frac{1}{\sigma_i}Av_i$
- $A = \sum \sigma_i u_i v_i^T$ (sum of rank-1 matrices)
- Best rank-$k$ approx: keep top $k$ terms

---

## 19. POSITIVE DEFINITE

- Symmetric with $x^TAx > 0$ for all $x \neq 0$
- Tests: all eigenvalues > 0 | all pivots > 0 | all leading minors > 0
- $\det(A) > 0$, all diagonal entries > 0

---

## 20. BLOCK MATRICES

- Block multiplication: standard rules with submatrices
- Block triangular det: $\det(A)\det(D)$
- Schur complement: $\det\begin{bmatrix} A & B \\ C & D \end{bmatrix} = \det(A)\det(D - CA^{-1}B)$

---

## 21. RANK OF adj(A)

| rank(A) | rank(adj(A)) |
|---|---|
| $n$ (full rank) | $n$ |
| $n - 1$ | $1$ |
| $< n - 1$ | $0$ |

---

## 22. KEY EQUIVALENCES (Square $n \times n$ Matrix)

**A is invertible** ⟺ ALL of these:
- $\det(A) \neq 0$
- rank(A) = n
- nullity(A) = 0
- Ax = b has unique solution ∀ b
- Ax = 0 has only trivial solution
- Columns/rows are LI
- 0 is NOT an eigenvalue
- All eigenvalues ≠ 0
- $A^{-1}$ exists

---

## 23. COMMON TRAPS TO REMEMBER

- ❌ $\det(A+B) \neq \det(A) + \det(B)$
- ❌ $\det(kA) = k^n\det(A)$, NOT $k\det(A)$
- ❌ AB = 0 does NOT imply A = 0 or B = 0
- ❌ Eigenvectors are NEVER zero
- ❌ $A$ and $A^T$ have same eigenvalues but DIFFERENT eigenvectors
- ❌ Pivot columns for column space: from ORIGINAL matrix, not REF
- ❌ Row operations change column space (but preserve row space)
- ❌ rank ≠ # non-zero rows of original (must row reduce first!)

---

## 24. QUICK FORMULAS CHEAT SHEET

$$\boxed{\text{rank}(A) + \text{nullity}(A) = n}$$

$$\boxed{\lambda_1 + \lambda_2 + \cdots = \text{tr}(A), \quad \lambda_1 \cdot \lambda_2 \cdots = \det(A)}$$

$$\boxed{A^{-1} = \frac{1}{\det(A)}\text{adj}(A)}$$

$$\boxed{P = A(A^TA)^{-1}A^T}$$

$$\boxed{A = U\Sigma V^T}$$

$$\boxed{A = PDP^{-1} \implies A^k = PD^kP^{-1}}$$

$$\boxed{\hat{x} = (A^TA)^{-1}A^Tb \text{ (least squares)}}$$

$$\boxed{p(A) = 0 \text{ (Cayley-Hamilton)}}$$

---

*Best of luck for GATE 2027! Revise this sheet the night before.*
