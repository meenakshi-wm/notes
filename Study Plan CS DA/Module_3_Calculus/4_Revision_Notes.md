# Calculus & Optimization — Short Revision Notes (Last-Day Revision)

## GATE 2027 CS/IT/DA | Quick Reference Sheet

---

## 1. STANDARD LIMITS (⭐ MUST MEMORIZE)

| Limit | Value |
|---|---|
| $\lim_{x \to 0} \frac{\sin x}{x}$ | $1$ |
| $\lim_{x \to 0} \frac{\tan x}{x}$ | $1$ |
| $\lim_{x \to 0} \frac{1 - \cos x}{x^2}$ | $\frac{1}{2}$ |
| $\lim_{x \to 0} \frac{e^x - 1}{x}$ | $1$ |
| $\lim_{x \to 0} \frac{a^x - 1}{x}$ | $\ln a$ |
| $\lim_{x \to 0} \frac{\ln(1+x)}{x}$ | $1$ |
| $\lim_{x \to 0} (1+x)^{1/x}$ | $e$ |
| $\lim_{x \to \infty}\left(1+\frac{1}{x}\right)^x$ | $e$ |
| $\lim_{x \to 0} \frac{\sin^{-1}x}{x}$ | $1$ |
| $\lim_{x \to 0} \frac{\tan^{-1}x}{x}$ | $1$ |
| $\lim_{x \to 0} \frac{x - \sin x}{x^3}$ | $\frac{1}{6}$ |
| $\lim_{x \to 0} \frac{x - \ln(1+x)}{x^2}$ | $\frac{1}{2}$ |
| $\lim_{x \to \infty} \frac{x^n}{e^x}$ | $0$ |
| $\lim_{x \to \infty} \frac{\ln x}{x^p}$ $(p > 0)$ | $0$ |

**Quick pattern:** $\lim_{x\to 0}\frac{\sin(ax)}{bx} = \frac{a}{b}$, $\lim_{x\to 0}\frac{e^{ax}-1}{bx} = \frac{a}{b}$

**Growth hierarchy:** $\ln x \ll x^p \ll a^x \ll x! \ll x^x$

---

## 2. L'HÔPITAL'S RULE

- Applies **only** to $\frac{0}{0}$ or $\frac{\infty}{\infty}$ forms
- $\lim \frac{f(x)}{g(x)} = \lim \frac{f'(x)}{g'(x)}$ (differentiate num & den **separately**, NOT quotient rule)
- Can apply repeatedly if result is still indeterminate
- **Always verify** the indeterminate form before applying

---

## 3. INDETERMINATE FORMS

| Form | Technique |
|---|---|
| $\frac{0}{0}$ | Factor, L'Hôpital, Taylor series |
| $\frac{\infty}{\infty}$ | L'Hôpital, divide by highest power |
| $0 \cdot \infty$ | Rewrite as $\frac{0}{0}$ or $\frac{\infty}{\infty}$ |
| $\infty - \infty$ | Combine into single fraction |
| $1^\infty$ | $e^{\lim g(x)[f(x)-1]}$ |
| $0^0, \infty^0$ | Take $\ln$, evaluate, then exponentiate |

**$1^\infty$ shortcut:** $\lim [f(x)]^{g(x)} = e^{\lim g(x)[f(x)-1]}$ when $f \to 1$, $g \to \infty$

**General power form:** $\lim [f(x)]^{g(x)} = e^{\lim g(x)\ln f(x)}$

---

## 4. CONTINUITY

- **Three conditions at $x = a$:** (1) $f(a)$ defined, (2) $\lim_{x\to a} f(x)$ exists, (3) $\lim = f(a)$
- **Removable discontinuity:** limit exists but $\neq f(a)$
- **Jump discontinuity:** LHL $\neq$ RHL
- **Infinite discontinuity:** limit is $\pm\infty$
- Polynomials, $e^x$, $\sin x$, $\cos x$ → continuous everywhere
- Sum, product, composition of continuous functions → continuous

---

## 5. DIFFERENTIABILITY

- $f'(a) = \lim_{h\to 0}\frac{f(a+h)-f(a)}{h}$ (LHD must $=$ RHD)
- **Differentiable $\implies$ Continuous** (converse FALSE)
- Not differentiable at: corners ($|x|$), cusps, vertical tangents ($x^{1/3}$), discontinuities

### Key Derivative Formulas

| $f(x)$ | $f'(x)$ |
|---|---|
| $x^n$ | $nx^{n-1}$ |
| $e^x$ | $e^x$ |
| $a^x$ | $a^x \ln a$ |
| $\ln x$ | $1/x$ |
| $\sin x$ | $\cos x$ |
| $\cos x$ | $-\sin x$ |
| $\tan x$ | $\sec^2 x$ |
| $\sin^{-1}x$ | $\frac{1}{\sqrt{1-x^2}}$ |
| $\tan^{-1}x$ | $\frac{1}{1+x^2}$ |

**Rules:** Product $(fg)' = f'g + fg'$ | Quotient $\left(\frac{f}{g}\right)' = \frac{f'g-fg'}{g^2}$ | Chain $[f(g(x))]' = f'(g(x))\cdot g'(x)$

---

## 6. MEAN VALUE THEOREMS ⭐⭐

### Rolle's Theorem
$f$ continuous on $[a,b]$, differentiable on $(a,b)$, $f(a) = f(b)$ $\implies \exists\, c \in (a,b): f'(c) = 0$

### LMVT (Lagrange's Mean Value Theorem)
$f$ continuous on $[a,b]$, differentiable on $(a,b)$ $\implies \exists\, c \in (a,b)$:
$$f'(c) = \frac{f(b) - f(a)}{b - a}$$

- Rolle's = special case of LMVT when $f(a) = f(b)$
- **LMVT application:** $|f(b)-f(a)| \leq M|b-a|$ where $M = \max|f'|$
- **Classic:** $|\sin a - \sin b| \leq |a - b|$

### Intermediate Value Theorem (IVT)
$f$ continuous on $[a,b]$, $k$ between $f(a)$ and $f(b)$ $\implies \exists\, c \in (a,b): f(c) = k$

- Sign change $\implies$ root exists

---

## 7. MAXIMA & MINIMA ⭐⭐

### Critical Points
$f'(c) = 0$ or $f'(c)$ DNE

### Second Derivative Test (Single Variable)
| $f''(c)$ | Type |
|---|---|
| $> 0$ | Local minimum |
| $< 0$ | Local maximum |
| $= 0$ | Inconclusive → use first derivative test |

### First Derivative Test
| Sign change of $f'$ at $c$ | Type |
|---|---|
| $+ \to -$ | Local maximum |
| $- \to +$ | Local minimum |
| No change | Neither (inflection) |

### Closed Interval: Always check endpoints!

---

## 8. INTEGRATION FORMULAS ⭐⭐

### Standard Integrals

| Integral | Result |
|---|---|
| $\int x^n\,dx$ | $\frac{x^{n+1}}{n+1} + C$ $(n \neq -1)$ |
| $\int \frac{1}{x}\,dx$ | $\ln|x| + C$ |
| $\int e^x\,dx$ | $e^x + C$ |
| $\int a^x\,dx$ | $\frac{a^x}{\ln a} + C$ |
| $\int \sin x\,dx$ | $-\cos x + C$ |
| $\int \cos x\,dx$ | $\sin x + C$ |
| $\int \sec^2 x\,dx$ | $\tan x + C$ |
| $\int \frac{1}{1+x^2}\,dx$ | $\tan^{-1}x + C$ |
| $\int \frac{1}{\sqrt{1-x^2}}\,dx$ | $\sin^{-1}x + C$ |

### Integration by Parts
$$\int u\,dv = uv - \int v\,du$$
**LIATE priority** for $u$: **L**og → **I**nverse trig → **A**lgebraic → **T**rig → **E**xponential

### Definite Integral Properties ⭐

| Property | Formula |
|---|---|
| **Even function** | $\int_{-a}^{a}f(x)\,dx = 2\int_0^a f(x)\,dx$ |
| **Odd function** | $\int_{-a}^{a}f(x)\,dx = 0$ |
| **King's rule** | $\int_0^a f(x)\,dx = \int_0^a f(a-x)\,dx$ |
| **Reversal** | $\int_a^b = -\int_b^a$ |
| **Splitting** | $\int_a^b = \int_a^c + \int_c^b$ |

### Leibniz Rule
$$\frac{d}{dx}\int_{a(x)}^{b(x)} f(t)\,dt = f(b(x))\cdot b'(x) - f(a(x))\cdot a'(x)$$

### Gamma Function Shortcut
$$\int_0^\infty x^n e^{-x}\,dx = n!$$

---

## 9. TAYLOR / MACLAURIN SERIES ⭐

$$f(x) = \sum_{n=0}^{\infty}\frac{f^{(n)}(a)}{n!}(x-a)^n$$

### Must-Know Series

| $f(x)$ | Maclaurin Series |
|---|---|
| $e^x$ | $1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots$ |
| $\sin x$ | $x - \frac{x^3}{3!} + \frac{x^5}{5!} - \cdots$ |
| $\cos x$ | $1 - \frac{x^2}{2!} + \frac{x^4}{4!} - \cdots$ |
| $\ln(1+x)$ | $x - \frac{x^2}{2} + \frac{x^3}{3} - \cdots$ |
| $\frac{1}{1-x}$ | $1 + x + x^2 + x^3 + \cdots$ $(|x|<1)$ |
| $(1+x)^n$ | $1 + nx + \frac{n(n-1)}{2!}x^2 + \cdots$ $(|x|<1)$ |
| $\tan^{-1}x$ | $x - \frac{x^3}{3} + \frac{x^5}{5} - \cdots$ $(|x|\leq 1)$ |

**Trick:** To evaluate $\lim_{x\to 0}\frac{f(x)}{g(x)}$, expand both using Taylor and cancel leading powers. Often faster than L'Hôpital for complicated limits.

---

## 10. PARTIAL DERIVATIVES & CHAIN RULE ⭐

- $f_x$: differentiate treating $y$ as constant
- $f_y$: differentiate treating $x$ as constant
- **Clairaut:** $f_{xy} = f_{yx}$ (if both continuous)
- **Chain rule:** $\frac{df}{dt} = f_x\frac{dx}{dt} + f_y\frac{dy}{dt}$

---

## 11. GRADIENT ⭐⭐

$$\nabla f = \left(\frac{\partial f}{\partial x_1}, \frac{\partial f}{\partial x_2}, \ldots, \frac{\partial f}{\partial x_n}\right)$$

- **Direction of steepest ascent**
- $-\nabla f$: direction of steepest **descent**
- $\nabla f \perp$ level curves (contour lines)
- $\|\nabla f\|$: maximum rate of change

### Directional Derivative
$$D_{\hat{u}}f = \nabla f \cdot \hat{u} = \|\nabla f\|\cos\theta$$

- Maximum when $\hat{u} \parallel \nabla f$: value $= \|\nabla f\|$
- Zero when $\hat{u} \perp \nabla f$ (along level curve)
- **IMPORTANT:** $\hat{u}$ must be a **unit vector**!

---

## 12. HESSIAN MATRIX & SECOND ORDER TEST ⭐⭐

$$H = \begin{bmatrix} f_{xx} & f_{xy} \\ f_{xy} & f_{yy} \end{bmatrix}$$

At a critical point $(\nabla f = 0)$:

| Condition | Conclusion |
|---|---|
| $\det(H) > 0$ and $f_{xx} > 0$ | **Local minimum** |
| $\det(H) > 0$ and $f_{xx} < 0$ | **Local maximum** |
| $\det(H) < 0$ | **Saddle point** |
| $\det(H) = 0$ | **Inconclusive** |

**Remember:** $\det(H) = f_{xx}f_{yy} - (f_{xy})^2$

For $n$ variables: check eigenvalues of $H$:
- All positive → minimum
- All negative → maximum
- Mixed signs → saddle

---

## 13. MATRIX CALCULUS (for DA) ⭐

| $f(\mathbf{x})$ | $\nabla f$ |
|---|---|
| $\mathbf{a}^T\mathbf{x}$ | $\mathbf{a}$ |
| $\mathbf{x}^T\mathbf{x}$ | $2\mathbf{x}$ |
| $\mathbf{x}^T A\mathbf{x}$ ($A$ symmetric) | $2A\mathbf{x}$ |
| $\mathbf{x}^T A\mathbf{x}$ (general) | $(A+A^T)\mathbf{x}$ |
| $\|A\mathbf{x}-\mathbf{b}\|^2$ | $2A^T(A\mathbf{x}-\mathbf{b})$ |

**Least Squares:** $\hat{\mathbf{x}} = (A^TA)^{-1}A^T\mathbf{b}$

---

## 14. CONVEX FUNCTIONS ⭐

### Definition
$f$ is convex if: $f(\lambda x + (1-\lambda)y) \leq \lambda f(x) + (1-\lambda)f(y)$ for $\lambda \in [0,1]$

### Tests for Convexity

| Test | Condition for Convexity |
|---|---|
| **First-order** | $f(y) \geq f(x) + \nabla f(x)^T(y-x)$ for all $x, y$ |
| **Second-order (1D)** | $f''(x) \geq 0$ for all $x$ |
| **Second-order (nD)** | $H(x)$ is positive semi-definite for all $x$ |

### Key Properties
- Convex + convex = convex
- $\max(f, g)$ is convex if both convex
- $f(Ax+b)$ is convex if $f$ is convex
- **Every local minimum of a convex function is a global minimum**
- Strictly convex → at most one global minimum

### Common Convex/Concave Functions

| Convex | Concave |
|---|---|
| $x^2$, $|x|$, $e^x$ | $\ln x$ $(x > 0)$, $\sqrt{x}$ $(x \geq 0)$ |
| $x^p$ $(p \geq 1$ or $p \leq 0$, $x > 0)$ | $x^p$ $(0 \leq p \leq 1$, $x > 0)$ |
| $\|x\|$ (any norm) | $-\|x\|$ |

---

## 15. UNCONSTRAINED OPTIMIZATION SUMMARY

### Steps
1. Find critical points: $\nabla f = \mathbf{0}$
2. Compute Hessian $H$ at each critical point
3. Classify using eigenvalues or $\det(H)$ test

For quadratic $f(\mathbf{x}) = \frac{1}{2}\mathbf{x}^T A\mathbf{x} - \mathbf{b}^T\mathbf{x}$:
- Minimizer: $\mathbf{x}^* = A^{-1}\mathbf{b}$ (if $A$ is positive definite)

---

## 16. LAGRANGE MULTIPLIERS (Equality Constraints) ⭐⭐

Optimize $f(\mathbf{x})$ subject to $g(\mathbf{x}) = 0$.

$$\mathcal{L}(\mathbf{x}, \lambda) = f(\mathbf{x}) - \lambda\, g(\mathbf{x})$$

### Conditions
$$\nabla f = \lambda\, \nabla g \quad \text{and} \quad g(\mathbf{x}) = 0$$

### Geometric Insight
At the optimum, $\nabla f$ is parallel to $\nabla g$ (the objective gradient is perpendicular to the constraint surface).

### Multiple Constraints
$g_1 = 0, g_2 = 0, \ldots$:
$$\nabla f = \lambda_1 \nabla g_1 + \lambda_2 \nabla g_2 + \cdots$$

---

## 17. KKT CONDITIONS (Inequality Constraints) ⭐⭐

Minimize $f(\mathbf{x})$ subject to $g_i(\mathbf{x}) \leq 0$, $h_j(\mathbf{x}) = 0$.

### The 4 KKT Conditions

| Condition | Formula |
|---|---|
| **1. Stationarity** | $\nabla f + \sum \mu_i \nabla g_i + \sum \lambda_j \nabla h_j = \mathbf{0}$ |
| **2. Primal feasibility** | $g_i(\mathbf{x}) \leq 0$, $h_j(\mathbf{x}) = 0$ |
| **3. Dual feasibility** | $\mu_i \geq 0$ |
| **4. Complementary slackness** | $\mu_i \cdot g_i(\mathbf{x}) = 0$ for each $i$ |

### Complementary Slackness Intuition
For each inequality constraint: either the constraint is active ($g_i = 0$) or the multiplier is zero ($\mu_i = 0$). "You don't pay for constraints that aren't binding."

### Sufficient Conditions
KKT conditions are **necessary** for optimality (under constraint qualifications). If $f$ is convex and $g_i$ are convex → KKT is also **sufficient**.

---

## 18. GRADIENT DESCENT ⭐

### Update Rule
$$\mathbf{x}_{k+1} = \mathbf{x}_k - \alpha\, \nabla f(\mathbf{x}_k)$$

where $\alpha > 0$ is the **learning rate** (step size).

### Key Facts
- Moves in $-\nabla f$ direction (steepest descent)
- **$\alpha$ too large** → diverges / oscillates
- **$\alpha$ too small** → very slow convergence
- For convex $f$: converges to global minimum
- For quadratic $f(\mathbf{x}) = \frac{1}{2}\mathbf{x}^TA\mathbf{x} - \mathbf{b}^T\mathbf{x}$:
  - Optimal $\alpha = \frac{2}{\lambda_{\max} + \lambda_{\min}}$
  - Convergence rate depends on **condition number** $\kappa = \frac{\lambda_{\max}}{\lambda_{\min}}$
  - Large $\kappa$ → slow convergence (ill-conditioned)

### Variants
| Variant | Description |
|---|---|
| Batch GD | Full gradient at each step |
| Stochastic GD (SGD) | Random subset (mini-batch) |
| Momentum | Adds velocity term to dampen oscillations |
| Adam | Adaptive learning rate per parameter |

---

## 19. QUICK DECISION FLOWCHART

```
Optimization Problem?
├── No constraints → Unconstrained
│   ├── Set ∇f = 0, get critical points
│   └── Hessian test to classify
├── Equality constraints only → Lagrange Multipliers
│   └── ∇f = λ∇g, solve with constraint
└── Inequality constraints → KKT
    └── 4 conditions: Stationarity, Primal, Dual, Comp. Slackness
```

---

## 20. COMMON GATE TRAPS & TIPS

| Trap | Fix |
|---|---|
| L'Hôpital without checking $\frac{0}{0}$ or $\frac{\infty}{\infty}$ | **Always verify** the indeterminate form |
| Forgetting to check endpoints on $[a,b]$ | Global extrema need boundary check |
| $f'' = 0 \implies$ inflection point | Need $f''$ to **change sign** |
| Direction derivative with non-unit vector | **Always normalize** the direction vector |
| Mixing up max/min: $\det(H) > 0$ alone isn't enough | Must also check sign of $f_{xx}$ |
| KKT: wrong sign convention | Stick with $\mu_i \geq 0$ for $g_i \leq 0$ |
| Forgetting complementary slackness | Each constraint: either active or $\mu = 0$ |
| $\frac{\sin x}{x} \to 1$ only when $x \to 0$ | As $x \to \infty$: $\frac{\sin x}{x} \to 0$ |
| Integration by parts: wrong LIATE choice | Practice the priority: Log > Inv trig > Algebraic > Trig > Exp |
| Odd/even symmetry applied to wrong interval | Works only for **symmetric** intervals $[-a, a]$ |
