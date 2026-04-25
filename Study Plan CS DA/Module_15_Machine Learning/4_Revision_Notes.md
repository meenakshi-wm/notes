# Machine Learning — Revision Notes for GATE 2027

> **Quick-reference sheet** | **GATE Weightage:** 8–12 marks (highest for GATE DA)

---

## 1. Linear Regression

| Formula | Expression |
|---------|-----------|
| Model | $\hat{y} = Xw$ |
| Normal equation | $w = (X^TX)^{-1}X^Ty$ |
| $R^2$ | $1 - SS_{res}/SS_{tot}$ |
| MSE gradient | $\frac{1}{n}X^T(Xw-y)$ |
| GD update | $w \leftarrow w - \alpha \nabla J$ |

---

## 2. Regularization

| | Ridge (L2) | LASSO (L1) |
|--|-----------|-----------|
| Penalty | $\lambda\sum w_j^2$ | $\lambda\sum\|w_j\|$ |
| Closed form | $(X^TX+\lambda I)^{-1}X^Ty$ | None |
| Sparsity | No | **Yes** |
| Feature selection | No | **Yes** |

**λ → ∞:** all weights → 0 (predicts mean)
**λ → 0:** ordinary least squares

---

## 3. Logistic Regression

$$P(y=1|x) = \sigma(w^Tx) = \frac{1}{1+e^{-w^Tx}}$$

| Property | Value |
|----------|-------|
| σ(0) | 0.5 |
| σ'(z) | σ(z)(1−σ(z)) |
| Max σ' | 0.25 at z=0 |
| Loss | BCE: $-[y\log p + (1-y)\log(1-p)]$ |
| Decision boundary | Linear: $w^Tx = 0$ |

**Softmax:** $P(k) = e^{z_k}/\sum e^{z_j}$ (multi-class)

---

## 4. SVM

$$\text{Margin} = \frac{2}{\|w\|}$$

| | Hard-margin | Soft-margin |
|--|------------|------------|
| Data | Linearly separable | Any |
| Slack | None | $\xi_i \geq 0$ |
| Objective | min $\frac{1}{2}\|w\|^2$ | min $\frac{1}{2}\|w\|^2 + C\sum\xi_i$ |

**Hinge loss:** $\max(0, 1-y\cdot f(x))$
**Kernels:** Linear, Polynomial $(x^Ty+c)^d$, RBF $e^{-\gamma\|x-y\|^2}$
**Large C:** narrow margin, overfit. **Small C:** wide margin, underfit.

---

## 5. KNN & Naive Bayes

**KNN:** k=1 overfits, k=n underfits. Scale features! Curse of dimensionality.

**Naive Bayes:** $P(C|x) \propto P(C)\prod P(x_j|C)$
- Conditional independence assumption
- Works well high-dimensional, small data

---

## 6. Decision Trees

| Criterion | Formula | Range |
|-----------|---------|-------|
| Entropy | $-\sum p_k \log_2 p_k$ | [0, log₂k] |
| Gini | $1 - \sum p_k^2$ | [0, 1−1/k] |
| Info Gain | $H(parent) - \sum\frac{|S_v|}{|S|}H(S_v)$ | ≥ 0 |

Max leaf nodes at depth d: $2^d$

---

## 7. PCA & LDA

| | PCA | LDA |
|--|-----|-----|
| Type | Unsupervised | Supervised |
| Objective | Max variance | Max class separation |
| Criterion | Eigenvectors of Σ | Eigenvectors of $S_W^{-1}S_B$ |
| Max components | min(n,d) | **k−1** |

**PCA steps:** Center → Cov matrix → Eigendecompose → Top k vectors → Project

---

## 8. Clustering

**K-means objective:** $\min \sum_i\sum_{x\in C_i}\|x-\mu_i\|^2$
- Local minimum only; sensitive to init → use k-means++
- Assumes convex, spherical clusters

**Hierarchical:** Single (chaining), Complete (compact), Ward's (balanced)

---

## 9. Neural Networks

**Perceptron:** $\hat{y} = \text{sign}(w^Tx+b)$. Only linear boundaries. Fails XOR.

**Parameters (layers $n_0 \to n_1 \to ... \to n_L$):** $\sum_{l=1}^{L}(n_{l-1}+1)n_l$

| Activation | Formula | Range | Issue |
|-----------|---------|-------|-------|
| Sigmoid | $1/(1+e^{-z})$ | (0,1) | Vanishing gradient |
| Tanh | $(e^z-e^{-z})/(e^z+e^{-z})$ | (−1,1) | Vanishing gradient |
| **ReLU** | $\max(0,z)$ | [0,∞) | **Fixes vanishing gradient** |

**Universal approx theorem:** Single hidden layer can approximate any continuous function.

---

## 10. Evaluation Metrics

| Metric | Formula |
|--------|---------|
| Accuracy | (TP+TN)/Total |
| Precision | TP/(TP+FP) |
| Recall | TP/(TP+FN) |
| F1 | 2PR/(P+R) |
| FPR | FP/(FP+TN) |

**ROC:** TPR vs FPR. AUC=1 perfect, AUC=0.5 random.
**Imbalanced data:** Use F1 or AUC, not accuracy.

---

## 11. Bias-Variance

$$\text{Error} = \text{Bias}^2 + \text{Variance} + \text{Noise}$$

| | High Bias | High Variance |
|--|----------|---------------|
| Symptom | Underfitting | Overfitting |
| Train err | High | Low |
| Test err | High | High |
| Fix | Complex model, more features | Regularize, more data, simpler model |

---

## 12. GATE Traps

1. **SVM margin = 1/||w||** → WRONG: margin = **2/||w||**
2. **Ridge does feature selection** → WRONG: LASSO does
3. **PCA is supervised** → WRONG: PCA is unsupervised
4. **K-means finds global optimum** → WRONG: local only
5. **Perceptron solves XOR** → WRONG: not linearly separable
6. **More features always help** → WRONG: curse of dimensionality
7. **High accuracy = good classifier** → Check class imbalance
8. **LDA max components = d** → WRONG: max = k−1
9. **σ'(z) max = 0.5** → WRONG: max = 0.25
10. **Underfitting → increase regularization** → WRONG: decrease it

---

## 13. Ensemble Methods

| Method | Bagging | Boosting |
|---|---|---|
| Training | Parallel | Sequential |
| Reduces | Variance | Bias |
| Key | Bootstrap + averaging | Reweight errors |
| Overfitting | Resistant (more trees OK) | Can overfit |

**Random Forest:** Bagging + random feature subset at each split. m = √p (classification), p/3 (regression). OOB ≈ 36.8% free validation.

**AdaBoost:** $\alpha_t = \frac{1}{2}\ln\frac{1-\epsilon_t}{\epsilon_t}$. Upweights misclassified samples. Reduces bias.

**Gradient Boosting:** Fits trees to negative gradients (pseudo-residuals). XGBoost adds regularization + 2nd order gradients.

---

## 14. DBSCAN & EM

**DBSCAN:** ε-neighborhood, MinPts. Core/Border/Noise points. Finds arbitrary shapes. Auto-detects k. Identifies outliers.

**EM (GMM):**
- E-step: Compute responsibilities (soft assignments)
- M-step: Update μ, Σ, π using weighted averages
- Converges to local maximum
- K-Means = hard EM with spherical Gaussians

---

## 15. Preprocessing Quick Reference

| Need | Scale | Encode | Select |
|---|---|---|---|
| **Method** | Standardize (Z-score), Min-Max, Robust | One-Hot (nominal), Label (ordinal) | LASSO, RFE, Correlation |
| **Needs scaling** | KNN, SVM, NN, PCA, K-Means | | |
| **No scaling needed** | Trees, Random Forest | | |

**Imbalanced data:** SMOTE, class weights, threshold tuning, F1/AUC metrics.

---

## 16. Clustering Evaluation

| Metric | Type | Range | Better |
|---|---|---|---|
| Silhouette | Internal | [-1, +1] | Higher |
| Davies-Bouldin | Internal | [0, ∞) | Lower |
| ARI | External | [-1, +1] | Higher |
| Elbow | Heuristic | — | Knee point |

Silhouette: $s(i) = \frac{b(i) - a(i)}{\max(a(i), b(i))}$

---

*End of Revision Notes — Machine Learning for GATE 2027*
