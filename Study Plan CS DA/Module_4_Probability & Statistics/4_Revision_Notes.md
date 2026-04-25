# Probability & Statistics — Last-Day Revision Notes for GATE 2027

---

## 1. Probability Basics

- **Sample space** $S$: set of all outcomes
- $P(A^c) = 1 - P(A)$
- **Inclusion-Exclusion:** $P(A \cup B) = P(A) + P(B) - P(A \cap B)$
- **For 3 events:** add singles, subtract pairs, add triple
- **De Morgan:** $\overline{A \cup B} = \bar{A} \cap \bar{B}$; $\overline{A \cap B} = \bar{A} \cup \bar{B}$
- $P(\text{at least one}) = 1 - P(\text{none})$
- $P(\text{exactly one of } A, B) = P(A) + P(B) - 2P(A \cap B)$

---

## 2. Conditional Probability & Bayes'

- $P(A|B) = \frac{P(A \cap B)}{P(B)}$
- **Multiplication rule:** $P(A \cap B) = P(A|B) \cdot P(B)$
- **Chain rule:** $P(A \cap B \cap C) = P(A) \cdot P(B|A) \cdot P(C|A \cap B)$
- ⭐ **Total Probability:** $P(A) = \sum P(A|B_i)P(B_i)$ ($B_i$ partition $S$)
- ⭐⭐ **Bayes' Theorem:**

$$P(B_j|A) = \frac{P(A|B_j)P(B_j)}{\sum_i P(A|B_i)P(B_i)}$$

- **Tree diagram:** branch → multiply along path → add across paths

---

## 3. Independence

- $A, B$ independent: $P(A \cap B) = P(A)P(B)$
- Independent ⟹ $P(A|B) = P(A)$
- Pairwise ≠ Mutual independence (need $P(A \cap B \cap C) = P(A)P(B)P(C)$ too)
- Independence does NOT imply conditional independence (and vice versa)

---

## 4. Expectation & Variance

- $E[X] = \sum x \cdot p(x)$ or $\int x f(x)dx$
- ⭐ **Linearity:** $E[aX + bY + c] = aE[X] + bE[Y] + c$ **(always, even if dependent!)**
- **LOTUS:** $E[g(X)] = \sum g(x)p(x)$
- **Recursive trick:** Set up $E = \ldots + (\text{prob})(\text{cost} + E)$
- $\text{Var}(X) = E[X^2] - (E[X])^2$
- $\text{Var}(aX + b) = a^2 \text{Var}(X)$
- $\text{Var}(X + Y) = \text{Var}(X) + \text{Var}(Y) + 2\text{Cov}(X,Y)$
- If INDEPENDENT: $\text{Var}(X+Y) = \text{Var}(X) + \text{Var}(Y)$

---

## 5. CDF

- $F(x) = P(X \leq x)$, non-decreasing, right-continuous
- $P(a < X \leq b) = F(b) - F(a)$
- $P(X = a) = F(a) - F(a^-)$ (jump at $a$)
- Continuous: $f(x) = F'(x)$

---

## ⭐ 6. Distribution Summary Table

### Discrete Distributions

| Distribution | PMF $P(X=k)$ | $E[X]$ | $\text{Var}(X)$ | MGF |
|---|---|---|---|---|
| **Bernoulli($p$)** | $p^k(1-p)^{1-k}$, $k \in \{0,1\}$ | $p$ | $p(1-p)$ | $1-p+pe^t$ |
| **Binomial($n,p$)** | $\binom{n}{k}p^k(1-p)^{n-k}$ | $np$ | $np(1-p)$ | $(1-p+pe^t)^n$ |
| **Poisson($\lambda$)** | $\frac{e^{-\lambda}\lambda^k}{k!}$ | $\lambda$ | $\lambda$ | $e^{\lambda(e^t-1)}$ |
| **Geometric($p$)** | $(1-p)^{k-1}p$, $k \geq 1$ | $\frac{1}{p}$ | $\frac{1-p}{p^2}$ | $\frac{pe^t}{1-(1-p)e^t}$ |
| **Discrete Uniform** | $\frac{1}{n}$, $n=b-a+1$ | $\frac{a+b}{2}$ | $\frac{n^2-1}{12}$ | — |

### Continuous Distributions

| Distribution | PDF $f(x)$ | $E[X]$ | $\text{Var}(X)$ | MGF |
|---|---|---|---|---|
| **Uniform($a,b$)** | $\frac{1}{b-a}$ | $\frac{a+b}{2}$ | $\frac{(b-a)^2}{12}$ | $\frac{e^{tb}-e^{ta}}{t(b-a)}$ |
| **Normal($\mu,\sigma^2$)** | $\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$ | $\mu$ | $\sigma^2$ | $e^{\mu t + \sigma^2 t^2/2}$ |
| **Exponential($\lambda$)** | $\lambda e^{-\lambda x}$, $x \geq 0$ | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^2}$ | $\frac{\lambda}{\lambda-t}$ |

### Key Distribution Facts
- **Poisson:** Mean = Variance = $\lambda$
- **Exponential:** Only continuous memoryless distribution; $P(X > s+t | X > s) = P(X > t)$
- **Geometric:** Only discrete memoryless distribution
- **Poisson approx:** $\text{Bin}(n,p) \approx \text{Poisson}(np)$ when $n$ large, $p$ small
- **Normal:** $aX + bY \sim N(a\mu_1 + b\mu_2, a^2\sigma_1^2 + b^2\sigma_2^2)$ if independent

---

## 7. Normal Distribution Quick Reference

- **Standardize:** $Z = \frac{X - \mu}{\sigma}$
- $\Phi(-z) = 1 - \Phi(z)$
- $\Phi(1) = 0.8413$, $\Phi(1.645) = 0.95$, $\Phi(1.96) = 0.975$, $\Phi(2) = 0.9772$, $\Phi(2.576) = 0.995$
- **68-95-99.7 Rule:** Within $1\sigma$: 68%, $2\sigma$: 95%, $3\sigma$: 99.7%
- Mean = Median = Mode

---

## 8. Mean, Mode, Median

| Measure | Definition |
|---|---|
| Mean | $E[X]$ |
| Median | $F(m) = 0.5$ |
| Mode | $\arg\max f(x)$ or $\arg\max P(X=x)$ |

- Right-skewed: Mode < Median < Mean
- Exponential: Mode = 0, Median = $\ln 2 / \lambda$, Mean = $1/\lambda$

---

## 9. Joint Distributions

- **Joint PMF:** $p(x,y) = P(X=x, Y=y)$
- **Marginals:** $p_X(x) = \sum_y p(x,y)$
- **Independence:** $p(x,y) = p_X(x) \cdot p_Y(y)$ for ALL $x,y$
- **Joint PDF:** $P((X,Y) \in A) = \iint_A f(x,y)\,dx\,dy$
- **Marginal PDF:** $f_X(x) = \int f(x,y)\,dy$
- **Conditional:** $f_{X|Y}(x|y) = \frac{f(x,y)}{f_Y(y)}$

---

## 10. Conditional Expectation

- $E[X|Y=y] = \sum x \cdot P(X=x|Y=y)$
- ⭐ **Law of Total Expectation:** $E[X] = E[E[X|Y]]$
- **Law of Total Variance:** $\text{Var}(X) = E[\text{Var}(X|Y)] + \text{Var}(E[X|Y])$

---

## 11. Covariance & Correlation

- $\text{Cov}(X,Y) = E[XY] - E[X]E[Y]$
- $\text{Cov}(X,X) = \text{Var}(X)$
- $\text{Cov}(aX, bY) = ab\,\text{Cov}(X,Y)$
- $\text{Cov}(X+Y, Z) = \text{Cov}(X,Z) + \text{Cov}(Y,Z)$
- Independent → $\text{Cov} = 0$, but **$\text{Cov} = 0 \not\Rightarrow$ independent**
- ⭐ **Correlation:** $\rho = \frac{\text{Cov}(X,Y)}{\sigma_X \sigma_Y}$, $-1 \leq \rho \leq 1$
- $|\rho| = 1$ iff perfect linear relationship

### Covariance Matrix

$$\Sigma = \begin{pmatrix} \text{Var}(X) & \text{Cov}(X,Y) \\ \text{Cov}(Y,X) & \text{Var}(Y) \end{pmatrix}$$

Symmetric, positive semi-definite.

---

## ⭐⭐ 12. Central Limit Theorem (CLT)

$$\bar{X} \xrightarrow{d} N\left(\mu, \frac{\sigma^2}{n}\right) \quad \text{as } n \to \infty$$

$$Z = \frac{\bar{X} - \mu}{\sigma/\sqrt{n}} \to N(0,1)$$

- Works for ANY distribution with finite mean and variance
- Rule of thumb: $n \geq 30$
- **Sum version:** $S_n \approx N(n\mu, n\sigma^2)$

---

## 13. Confidence Intervals

| Setting | Formula | Use When |
|---|---|---|
| $\sigma$ known | $\bar{X} \pm z_{\alpha/2} \cdot \frac{\sigma}{\sqrt{n}}$ | Large $n$ or $\sigma$ known |
| $\sigma$ unknown | $\bar{X} \pm t_{\alpha/2, n-1} \cdot \frac{S}{\sqrt{n}}$ | Small $n$, $\sigma$ unknown |

| Confidence | $z_{\alpha/2}$ |
|---|---|
| 90% | 1.645 |
| 95% | 1.96 |
| 99% | 2.576 |

**Sample size:** $n = \left(\frac{z_{\alpha/2} \cdot \sigma}{E}\right)^2$ (round UP)

**Standard Error:** $\text{SE} = \frac{\sigma}{\sqrt{n}}$ (or $S/\sqrt{n}$)

---

## ⭐⭐ 14. Hypothesis Testing Flowchart

```
State H₀ and H₁
       ↓
Choose α (usually 0.05)
       ↓
Is σ known?
 ├─ YES → Z-test: Z = (X̄ - μ₀)/(σ/√n)
 └─ NO  → t-test: T = (X̄ - μ₀)/(S/√n), df = n-1
       ↓
Compute test statistic
       ↓
Compare with critical value OR compute p-value
       ↓
 p-value < α → REJECT H₀
 p-value ≥ α → FAIL TO REJECT H₀
```

### Test Types

| $H_1$ | Reject Region |
|---|---|
| $\mu \neq \mu_0$ (two-tailed) | $|Z| > z_{\alpha/2}$ |
| $\mu > \mu_0$ (right-tailed) | $Z > z_\alpha$ |
| $\mu < \mu_0$ (left-tailed) | $Z < -z_\alpha$ |

### Type I & II Errors

| | $H_0$ True | $H_0$ False |
|---|---|---|
| Reject $H_0$ | **Type I** ($\alpha$) | Correct (**Power** $= 1-\beta$) |
| Fail to Reject | Correct | **Type II** ($\beta$) |

- Type I = False Positive = $\alpha$ (significance level)
- Type II = False Negative = $\beta$
- Power = $1 - \beta$
- ↑ $n$ → ↓ both errors
- ↓ $\alpha$ → ↑ $\beta$

---

## 15. Chi-Squared Distribution & Test

- $\chi^2_k$: sum of $k$ squared standard normals
- $E[\chi^2_k] = k$, $\text{Var}(\chi^2_k) = 2k$

### Goodness-of-Fit Test

$$\chi^2 = \sum \frac{(O_i - E_i)^2}{E_i}, \quad df = k - 1$$

### Test of Independence (Contingency Table)

$$E_{ij} = \frac{R_i \times C_j}{N}, \quad df = (r-1)(c-1)$$

- Reject $H_0$ if $\chi^2 > \chi^2_{\alpha, df}$

---

## 16. Quick-Fire Formulas to Memorize

| Formula | Value |
|---|---|
| $\sum_{k=0}^{\infty} x^k$ | $\frac{1}{1-x}$, $|x|<1$ |
| $\sum_{k=1}^{\infty} kx^k$ | $\frac{x}{(1-x)^2}$ |
| $\sum_{k=0}^{\infty} \frac{x^k}{k!}$ | $e^x$ |
| $e^{-1}$ | $\approx 0.3679$ |
| $e^{-2}$ | $\approx 0.1353$ |
| $e^{-3}$ | $\approx 0.0498$ |
| $\ln 2$ | $\approx 0.6931$ |
| $\sqrt{2\pi}$ | $\approx 2.5066$ |
| $1/\sqrt{2\pi}$ | $\approx 0.3989$ |

---

## ⭐ 17. Top GATE Traps to Avoid

1. **$P(A \cup B) \neq P(A) + P(B)$** unless mutually exclusive
2. **$\text{Var}(X+Y) \neq \text{Var}(X) + \text{Var}(Y)$** unless independent
3. **$\text{Cov} = 0$** does NOT mean independent
4. **Always standardize** for Normal distribution problems
5. **$P(A|B) \neq P(B|A)$** — use Bayes to flip
6. **Standard error** $= \sigma/\sqrt{n}$, not $\sigma$
7. **p-value ≠ $P(H_0$ true$)$**
8. **"Fail to reject"** ≠ "accept $H_0$"
9. **Geometric**: check if starting from $k=0$ or $k=1$
10. **Poisson**: mean EQUALS variance; Binomial: mean > variance

---

## 18. Sample Variance Note

$$S^2 = \frac{1}{n-1}\sum(X_i - \bar{X})^2 \quad \text{(unbiased, uses } n-1\text{)}$$

$$\frac{(n-1)S^2}{\sigma^2} \sim \chi^2_{n-1}$$

---

## 19. Indicator Variable Trick

For counting-type expectations, define $X_i = \begin{cases} 1 & \text{if event } i \text{ occurs} \\ 0 & \text{otherwise} \end{cases}$

Then $X = \sum X_i$ and $E[X] = \sum E[X_i] = \sum P(\text{event } i)$

**Works even without independence!**

---

## 20. Properties Comparison

| Property | Expectation | Variance |
|---|---|---|
| Constant | $E[c] = c$ | $\text{Var}(c) = 0$ |
| Scaling | $E[aX] = aE[X]$ | $\text{Var}(aX) = a^2\text{Var}(X)$ |
| Shift | $E[X+b] = E[X]+b$ | $\text{Var}(X+b) = \text{Var}(X)$ |
| Sum | $E[X+Y] = E[X]+E[Y]$ always | $\text{Var}(X+Y) = \text{Var}(X)+\text{Var}(Y)$ only if indep |
| Product | $E[XY] = E[X]E[Y]$ only if indep | — |

---

> **Exam Day Tip:** Read every probability question twice. Identify: (1) what distribution, (2) what's given, (3) what's asked. Draw a tree diagram for conditional probability. Standardize for Normal. Use linearity of expectation whenever possible.
