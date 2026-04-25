# Machine Learning — Solutions for GATE 2027

> Complete solutions for all 75 questions.

---

## Section A: Linear Regression

### Q1. Least-squares line for (1,2),(2,4),(3,5),(4,4),(5,5)
$\bar{x}=3, \bar{y}=4$
$S_{xy} = \sum(x_i-3)(y_i-4) = (-2)(-2)+(-1)(0)+(0)(1)+(1)(0)+(2)(1) = 4+0+0+0+2 = 6$
$S_{xx} = \sum(x_i-3)^2 = 4+1+0+1+4 = 10$
$w_1 = 6/10 = 0.6$, $w_0 = 4 - 0.6(3) = 2.2$
**$\hat{y} = 2.2 + 0.6x$**

---

### Q2. Normal equation derivation
$J(w) = (Xw-y)^T(Xw-y) = w^TX^TXw - 2w^TX^Ty + y^Ty$
$\nabla_w J = 2X^TXw - 2X^Ty = 0$
**$w = (X^TX)^{-1}X^Ty$**

---

### Q3. R² for Q1
$\hat{y}$: 2.8, 3.4, 4.0, 4.6, 5.2
$SS_{res} = (2-2.8)^2+(4-3.4)^2+(5-4)^2+(4-4.6)^2+(5-5.2)^2 = 0.64+0.36+1+0.36+0.04 = 2.4$
$SS_{tot} = (2-4)^2+(4-4)^2+(5-4)^2+(4-4)^2+(5-4)^2 = 4+0+1+0+1 = 6$
$R^2 = 1 - 2.4/6 = 1 - 0.4 = **0.6**$

---

### Q4. Singular X^TX
Implies **multicollinearity** (features are linearly dependent).
Solutions: (1) Remove redundant features, (2) Add regularization (Ridge: $(X^TX+\lambda I)^{-1}$ is always invertible).

---

### Q5. Min n for unique polynomial regression
Degree d polynomial has d+1 parameters. Need **n ≥ d+1** for unique solution.

---

### Q6. Time complexity of normal equation
$(X^TX)^{-1}$: $X^TX$ is $d \times d$ → inversion is $O(d^3)$. $X^TX$ computation: $O(nd^2)$.
**Total: $O(nd^2 + d^3)$**

---

### Q7. Perfectly correlated feature added
$X^TX$ becomes **singular** (columns are linearly dependent, rank deficient). No unique solution.

---

### Q8. Adding irrelevant feature
$R^2$: stays same or **increases slightly** (never decreases with more features).
Adjusted $R^2$: **decreases** (penalizes extra features). This is why adjusted R² is preferred.

---

## Section B: Gradient Descent

### Q9. GD update for MSE
$\nabla_w J = \frac{1}{n}X^T(Xw - y)$ (for $J = \frac{1}{2n}\|Xw-y\|^2$)
**Update: $w \leftarrow w - \frac{\alpha}{n}X^T(Xw-y)$**

---

### Q10. Weight update
$w_{new} = [0.5, 1.0, 0.3] - 0.01 \times [2, -3, 1]$
= $[0.5-0.02, 1.0+0.03, 0.3-0.01]$ = **[0.48, 1.03, 0.29]**

---

### Q11. GD variants comparison
| | Batch GD | Mini-batch | SGD |
|--|---------|-----------|-----|
| Samples/update | All n | b | 1 |
| Convergence | Smooth | Moderate | Noisy |
| Speed/epoch | Slow | Medium | Fast |
| Memory | High | Medium | Low |

---

### Q12. Convex vs non-convex convergence
**Convex:** Yes, GD converges to global minimum (with proper learning rate).
**Non-convex:** Only converges to local minimum; may not find global.

---

### Q13. Large learning rate on MSE
Weights **oscillate and diverge**. Each step overshoots the minimum, and errors grow exponentially.

---

### Q14. Feature scaling helps because
Without scaling, features on different scales create elongated contours → GD zig-zags. With scaling (standardize to mean=0, std=1), contours become more circular → direct path to minimum → **faster convergence**.

---

### Q15. Gradient derivation
$J = \frac{1}{2n}\|Xw-y\|^2 = \frac{1}{2n}(Xw-y)^T(Xw-y)$
$\nabla_w J = \frac{1}{2n} \cdot 2X^T(Xw-y) = **\frac{1}{n}X^T(Xw-y)**$

---

### Q16. SGD gradient computations per epoch
SGD: n gradient computations (one per sample) per epoch.
Batch GD: 1 gradient computation per epoch (using all n samples).
**Both use n samples per epoch, but SGD does n updates while batch GD does 1.**

---

## Section C: Regularization

### Q17. Cost functions
**Ridge:** $J = \frac{1}{n}\|Xw-y\|^2 + \lambda\|w\|_2^2$
**LASSO:** $J = \frac{1}{n}\|Xw-y\|^2 + \lambda\|w\|_1$

---

### Q18. Ridge closed-form
$w = (X^TX + \lambda I)^{-1}X^Ty$
$X^TX + 2I = \begin{pmatrix} 6 & 2 \\ 2 & 5 \end{pmatrix}$
Inverse: $\frac{1}{30-4}\begin{pmatrix} 5 & -2 \\ -2 & 6 \end{pmatrix} = \frac{1}{26}\begin{pmatrix} 5 & -2 \\ -2 & 6 \end{pmatrix}$
$w = \frac{1}{26}\begin{pmatrix} 5(8)+(-2)(5) \\ -2(8)+6(5) \end{pmatrix} = \frac{1}{26}\begin{pmatrix} 30 \\ 14 \end{pmatrix} = **\begin{pmatrix} 1.154 \\ 0.538 \end{pmatrix}**$

---

### Q19. LASSO sparsity (geometric)
L1 constraint region is a **diamond** (corners at axes). The elliptical cost contours are more likely to touch the diamond at a corner (where some $w_j=0$).
L2 constraint is a **circle** — contours touch smoothly, rarely at axis (no exact zeros).

---

### Q20. λ → ∞ in Ridge
All weights → **0**. The model predicts the mean of y for all inputs.
Training error → **$\text{Var}(y)$** (same as $SS_{tot}/n$).

---

### Q21. Ridge or LASSO for feature selection?
**LASSO** — it sets irrelevant feature weights to exactly 0, performing automatic feature selection. Ridge keeps all features with small but nonzero weights.

---

### Q22. Training MSE=0.02, Test MSE=0.5
**Overfitting** (low training, high test error). High variance.
Use regularization (Ridge or LASSO), or get more training data, or simplify model.

---

### Q23. Elastic Net benefits
- L1: feature selection (sparsity)
- L2: handles correlated features well (groups of related features)
- Together: sparse model that handles multicollinearity.

---

## Section D: Logistic Regression & Softmax

### Q24. Gradient of BCE
$J = -\frac{1}{n}\sum[y_i\log\sigma(w^Tx_i) + (1-y_i)\log(1-\sigma(w^Tx_i))]$
$\nabla_w J = \frac{1}{n}\sum(\sigma(w^Tx_i) - y_i)x_i = **\frac{1}{n}X^T(\hat{p} - y)**$
(Same form as linear regression gradient!)

---

### Q25. P(y=1) when w^Tx = 0
$\sigma(0) = \frac{1}{1+e^0} = \frac{1}{2} = **0.5**$

---

### Q26. Why not MSE for logistic regression?
MSE with sigmoid creates a **non-convex** loss surface with many local minima.
Cross-entropy is **convex** → guaranteed convergence to global optimum.

---

### Q27. Sigmoid derivative maximum
$\sigma'(z) = \sigma(z)(1-\sigma(z))$. Maximum when $\sigma(z) = 0.5$, i.e., $z = 0$.
Max value: $0.5 \times 0.5 = **0.25**$

---

### Q28. Cross-entropy for softmax [0.7, 0.2, 0.1], true class 1
$L = -\log(0.7) = **0.357**$
(Only the true class contributes to the loss.)

---

### Q29. Reduce false positives
**Increase the threshold** (e.g., from 0.5 to 0.7). This makes the model more conservative about predicting class 1, reducing FP at the cost of more FN.

---

### Q30. Non-linear boundaries in logistic regression
Yes, by using **polynomial features** (e.g., $x_1^2, x_1x_2, x_2^2$). The decision boundary becomes non-linear in original space while remaining linear in the transformed feature space.

---

### Q31. Feature multiplied by 100
The corresponding weight gets divided by approximately 100 (to maintain the same $w^Tx$). The model predictions stay the same, but convergence of GD may be affected. **Feature scaling is recommended.**

---

## Section E: SVM

### Q32. Maximum margin hyperplane
Class +1: (1,1), (2,2). Class −1: (0,0), (1,0).
The separating hyperplane passes between the closest points.
Support vectors are likely (1,1) from +1 and (1,0) from −1.
Midpoint: (1, 0.5). Normal direction: (0, 1).
**Hyperplane: $x_2 = 0.5$ (or $w = [0,1], b = -0.5$)**
Margin = distance between (1,1) and (1,0) projected onto normal = **1.0**

---

### Q33. SVM margin
**Margin = $\frac{2}{\|w\|}$**

---

### Q34. Parameter C in soft-margin SVM
C controls the **tradeoff between margin width and classification errors.**
Large C → narrow margin, fewer errors (may overfit).
Small C → wide margin, more errors tolerated (may underfit).

---

### Q35. Support vectors
Support vectors are the closest points to the decision boundary.
Class +1 closest: (3,3). Class −1 closest: (1,1).
**Support vectors: (3,3) and (1,1)**

---

### Q36. Hinge loss
$L = \max(0, 1-y \cdot f(x))$
Zero when $y \cdot f(x) \geq 1$, i.e., the point is correctly classified AND beyond the margin.

---

### Q37. Kernel SVM for XOR
XOR: (0,0)→−1, (0,1)→+1, (1,0)→+1, (1,1)→−1.
Not linearly separable in 2D.
RBF kernel maps to infinite-dimensional space where XOR becomes linearly separable. Or polynomial kernel of degree 2: add feature $x_1x_2$, making data separable.

---

### Q38. RBF kernel computation
$K = \exp(-0.1 \times \|[1,0]-[0,1]\|^2) = \exp(-0.1 \times (1+1)) = \exp(-0.2) = **0.819**$

---

### Q39. Increasing C
Increasing C → **decreases margin** (tighter fit to training data) → leads to **overfitting**.

---

## Section F: KNN, Naive Bayes, Decision Trees

### Q40. 3-NN for (3,3)
Distances from (3,3):
(1,1): $\sqrt{8}=2.83$, (2,1): $\sqrt{5}=2.24$, (1,2): $\sqrt{5}=2.24$
(4,4): $\sqrt{2}=1.41$, (5,4): $\sqrt{5}=2.24$

3 nearest: (4,4)→−, (2,1)→+, (1,2)→+  [or (5,4)→−... let me recalculate]
Actually (2,1): $\sqrt{1+4}=2.24$, (5,4): $\sqrt{4+1}=2.24$. Tie.

3 nearest by distance: **(4,4):1.41(−)**, then tie at 2.24: (2,1)→+, (1,2)→+, (5,4)→−
Pick 3: (4,4)−, (2,1)+, (1,2)+. Majority: **+1 (positive class)**

---

### Q41. k=1 vs k=n in KNN
k=1: **Overfitting** — complex, jagged boundary, training accuracy = 100%.
k=n: **Underfitting** — always predicts majority class (entire training set votes).

---

### Q42. Naive Bayes classification
$P(C_1|x) \propto P(C_1) \cdot P(x_1|C_1) \cdot P(x_2|C_1) = 0.4 \times 0.7 \times 0.3 = 0.084$
$P(C_2|x) \propto P(C_2) \cdot P(x_1|C_2) \cdot P(x_2|C_2) = 0.6 \times 0.4 \times 0.8 = 0.192$

$P(C_1|x) = 0.084/(0.084+0.192) = 0.304$
$P(C_2|x) = 0.192/0.276 = 0.696$

**Classify as C₂** (higher posterior probability).

---

### Q43. Why "naive"?
The **conditional independence assumption** — features are assumed independent given the class — is rarely true in practice. This "naive" simplification makes computation tractable.

---

### Q44. Entropy for 5+, 5−
$H = -\frac{5}{10}\log_2\frac{5}{10} - \frac{5}{10}\log_2\frac{5}{10} = -0.5(-1) - 0.5(-1) = **1.0$ bit**

Maximum entropy (most uncertain).

---

### Q45. Information gain
Parent entropy: $H = -\frac{6}{10}\log_2(0.6) - \frac{4}{10}\log_2(0.4) = 0.442 + 0.529 = 0.971$

Left (4+, 1−): $H_L = -\frac{4}{5}\log_2(0.8) - \frac{1}{5}\log_2(0.2) = 0.258 + 0.464 = 0.722$
Right (2+, 3−): $H_R = -\frac{2}{5}\log_2(0.4) - \frac{3}{5}\log_2(0.6) = 0.529 + 0.442 = 0.971$

$IG = 0.971 - \frac{5}{10}(0.722) - \frac{5}{10}(0.971) = 0.971 - 0.361 - 0.486 = **0.124$**

---

### Q46. Gini impurity for [0.3, 0.7]
$Gini = 1 - (0.3^2 + 0.7^2) = 1 - (0.09 + 0.49) = 1 - 0.58 = **0.42**$

---

### Q47. IG vs Gini
Both give similar results in practice. IG uses logarithm (more expensive), Gini uses squares.
They can differ when: multi-way splits (IG biased toward many splits → use gain ratio).
Gini prefers balanced splits. IG prefers pure splits.

---

### Q48. Pre vs Post pruning
**Pre-pruning:** Stop tree growth early (max depth, min samples per leaf, min info gain).
**Post-pruning:** Grow full tree, then remove subtrees that don't improve validation performance. Generally gives better results but more expensive.

---

### Q49. Max leaf nodes at depth d
**$2^d$** (binary tree, each internal node splits into 2).

---

## Section G: PCA & LDA

### Q50. PCA of covariance matrix
$\Sigma = \begin{pmatrix} 4 & 2 \\ 2 & 3 \end{pmatrix}$
$\det(\Sigma - \lambda I) = (4-\lambda)(3-\lambda) - 4 = \lambda^2 - 7\lambda + 8 = 0$
$\lambda = \frac{7 \pm \sqrt{49-32}}{2} = \frac{7 \pm \sqrt{17}}{2}$
$\lambda_1 = 5.56, \lambda_2 = 1.44$

For $\lambda_1=5.56$: $(4-5.56)v_1 + 2v_2 = 0 → v = [0.788, 0.615]$ (normalized)
Variance: PC1 explains $5.56/7 = **79.4\%**$, PC2 explains $1.44/7 = **20.6\%**$

---

### Q51. Components for ≥90% variance
PC1: 55%, PC2: 55+25=80%, PC3: 80+12=92% ≥ 90%
**3 components needed.**

---

### Q52. Why center for PCA?
PCA finds directions of maximum variance. Without centering, the first PC points toward the mean (captures location, not spread). Centering ensures PCA captures **covariance structure**.

---

### Q53. LDA max components for 5 classes
**$k-1 = 4$** discriminant components maximum.

---

### Q54. PCA unsupervised?
**True.** PCA uses only the feature matrix X; class labels are not used. It maximizes variance, which is independent of labels.

---

### Q55. 10 of 100 features capture 95% variance
The data has **high intrinsic dimensionality of ~10** despite 100 features. Most features are redundant/correlated. The data lies near a 10-dimensional linear subspace.

---

### Q56. PCA vs LDA comparison
| | PCA | LDA |
|--|-----|------|
| Objective | Max variance | Max class separation |
| Supervision | Unsupervised | Supervised |
| Max components | min(n, d) | k−1 |
| Criterion | Covariance eigenvectors | $S_W^{-1}S_B$ eigenvectors |

---

## Section H: Clustering

### Q57. K-means iterations
Initial: C₁={1,2} μ₁=2, C₂={8,9,10} μ₂=9

**Iteration 1:**
Assign: 1→C₁(d=1), 2→C₁(d=0), 8→C₂(d=1), 9→C₂(d=0), 10→C₂(d=1)
Update: μ₁=(1+2)/2=1.5, μ₂=(8+9+10)/3=9

**Iteration 2:**
Assign: 1→C₁(d=0.5), 2→C₁(d=0.5), 8→C₂(d=1), 9→C₂(d=0), 10→C₂(d=1)
Update: μ₁=1.5, μ₂=9. **Converged!** (no change)

---

### Q58. K-means initialization sensitivity
Poor initialization can lead to suboptimal local minimum.
**K-means++:** Choose first centroid randomly. Each subsequent centroid chosen with probability proportional to $D(x)^2$ (distance to nearest existing centroid). Spreads centroids evenly.

---

### Q59. Unbalanced clusters (100, 5, 2)
Likely **bad initialization** — some centroids started in one dense region. Could also indicate the data doesn't naturally have 3 equally-sized clusters. Solutions: use k-means++, run multiple times, or consider k=2.

---

### Q60. Linkage comparison
| | Single | Complete | Ward's |
|--|--------|----------|--------|
| Tendency | Chain-like elongated clusters | Compact, spherical | Compact, equal-sized |
| Sensitive to | Noise/outliers (chaining) | Outliers less | Balanced clusters |
| Best for | Non-convex | Compact clusters | Similar-size clusters |

---

### Q61. K-means objective
**Minimize within-cluster sum of squares (WCSS):**
$$\text{Minimize} \sum_{i=1}^{k}\sum_{x \in C_i}\|x - \mu_i\|^2$$

---

### Q62. K-means for non-convex clusters
**No.** K-means uses Euclidean distance and assigns points to nearest centroid, which creates convex (Voronoi) regions. Non-convex clusters require methods like DBSCAN or kernel k-means.

---

## Section I: Neural Networks

### Q63. Perceptron classification
$z = w^Tx + b = 0.5(1) + (-0.3)(1) + 0.1 = 0.3$
$\hat{y} = \text{sign}(0.3) = **+1**$

---

### Q64. Perceptron can't solve XOR
XOR truth table: (0,0)→0, (0,1)→1, (1,0)→1, (1,1)→0. No single line can separate the 1s from the 0s — the classes are NOT linearly separable. Perceptron can only learn linear boundaries.

---

### Q65. Weights in network (3→4→2)
Input→Hidden: 3×4 weights + 4 biases = 16
Hidden→Output: 4×2 weights + 2 biases = 10
**Total: 26 parameters**

---

### Q66. Backprop weight update (conceptual)
For weight $w_{jk}$ connecting hidden unit j to output unit k:
$\frac{\partial L}{\partial w_{jk}} = \frac{\partial L}{\partial a_k} \cdot \frac{\partial a_k}{\partial z_k} \cdot \frac{\partial z_k}{\partial w_{jk}} = (a_k - y_k) \cdot \sigma'(z_k) \cdot h_j$

Update: $w_{jk} \leftarrow w_{jk} - \alpha(a_k - y_k)\sigma'(z_k)h_j$

---

### Q67. Activation comparison
**Sigmoid:** Squashes to (0,1). Suffers from **vanishing gradient** (derivative ≤ 0.25).
**Tanh:** Squashes to (−1,1). Zero-centered but still vanishing gradient.
**ReLU:** $\max(0,z)$. **Solves vanishing gradient** for positive inputs. Fast. Can suffer "dying ReLU" (gradient=0 for z<0).

---

### Q68. Universal approximation theorem
A feedforward neural network with a **single hidden layer** containing a finite number of neurons can approximate any continuous function on compact subsets of $\mathbb{R}^n$ to any desired accuracy, given a suitable activation function (e.g., sigmoid, ReLU).

---

## Section J: Evaluation & Bias-Variance

### Q69. Metrics computation
Accuracy = (80+90)/200 = **85%**
Precision = 80/(80+20) = **80%**
Recall = 80/(80+10) = **88.9%**
F1 = 2(0.8)(0.889)/(0.8+0.889) = 1.422/1.689 = **84.2%**

---

### Q70. Missing disease → optimize recall
**Recall** — we want to minimize false negatives (missing actual disease cases). High recall ensures most sick patients are identified, even at the cost of some false positives.

---

### Q71. ROC curve and AUC
ROC plots TPR (recall) vs FPR at various classification thresholds.
AUC = 0.5 means the model is **no better than random guessing** (diagonal line).

---

### Q72. 5-fold CV splits
Each fold: Test = 1000/5 = **200 samples**, Training = **800 samples**.

---

### Q73. Low training, high test error
**High variance** (overfitting). Fix: more data, regularization, simpler model, dropout, cross-validation.

---

### Q74. Bias-variance decomposition
$$E[(y - \hat{f}(x))^2] = \text{Bias}[\hat{f}(x)]^2 + \text{Var}[\hat{f}(x)] + \sigma^2$$
where $\sigma^2$ is irreducible noise.

---

### Q75. Underfitting: add features or regularization?
**(a) Add polynomial features.** The model underfits → it's too simple (high bias). Adding features increases model complexity. Increasing regularization would make it even simpler → worse.

---

*End of Solutions — Machine Learning for GATE 2027*

---

## Section K Solutions: Ensemble Methods (Q76–Q85)

---

### Q76. Random Forest feature subset
Classification: $m = \lfloor\sqrt{50}\rfloor = 7$ features per split. Regression: $m = \lfloor 50/3 \rfloor \approx 17$ features per split.

---

### Q77. OOB fraction
Probability a sample is NOT picked in one draw = $(1 - 1/n)$. Over $n$ draws: $(1 - 1/n)^n \to 1/e \approx 0.368$. So **~36.8%** are not included. These are called **Out-of-Bag (OOB)** samples — used as free validation data.

---

### Q78. Bagging vs Boosting
- **Bagging** primarily reduces **variance** (by averaging independent models)
- **Boosting** primarily reduces **bias** (by sequentially correcting errors)

Both can reduce overall error, but their primary mechanisms differ.

---

### Q79. AdaBoost α_t

$$\alpha_t = \frac{1}{2} \ln\left(\frac{1 - \epsilon_t}{\epsilon_t}\right) = \frac{1}{2} \ln\left(\frac{0.7}{0.3}\right) = \frac{1}{2} \ln(2.333) \approx \frac{1}{2}(0.847) \approx 0.424$$

---

### Q80. Effect of reducing learning rate
(a) **More trees needed** — smaller steps require more iterations to converge
(b) **Lower overfitting risk** — better generalization (with enough trees)
(c) **More training time** — more trees to train
Rule of thumb: decrease $\eta$ and increase number of trees proportionally.

---

### Q81. Random Forest doesn't overfit with more trees
Each tree is trained on a **different** bootstrap sample with **random features**. Adding more trees = **averaging more independent estimates**. By the law of large numbers, averaging reduces variance. The individual trees don't interact or become more complex.

In boosting, each new tree depends on previous ones and explicitly tries to fit remaining errors → can overfit training data.

---

### Q82. AdaBoost with 50% error
$\alpha_t = \frac{1}{2}\ln\left(\frac{1 - 0.5}{0.5}\right) = \frac{1}{2}\ln(1) = 0$

The model weight is 0 → this learner contributes nothing. A 50% error learner is no better than random, so AdaBoost ignores it.

---

### Q83. XGBoost innovations
1. **Regularized objective** — adds L1 (α) and L2 (λ) penalties on leaf weights
2. **Second-order gradients** — uses both gradient and Hessian (Newton's method) for better optimization
3. **Column (feature) subsampling** — like Random Forest, reduces overfitting
4. **Built-in handling of missing values** — learns optimal default direction
5. **Tree pruning** — grows max-depth tree then prunes back (unlike GBM's greedy growth)
6. **Parallel feature sorting** — efficient implementation

---

### Q84. Stacking meta-learner input
The level-1 meta-learner receives the **predictions** of the three level-0 models as input features: $(\hat{y}_{\text{LR}}, \hat{y}_{\text{SVM}}, \hat{y}_{\text{DT}})$ for each sample. To avoid overfitting, level-0 predictions are typically generated using cross-validation.

---

### Q85. Unpruned trees in Random Forest
Individual unpruned trees have **high variance** (overfit). But: (1) each tree sees different data (bootstrap), (2) each split considers random features → trees are **decorrelated**. Averaging B decorrelated high-variance estimators gives variance $\sigma^2/B$ which is low. So the forest doesn't overfit even though individual trees do.

---

## Section L Solutions: Preprocessing, DBSCAN, EM & Evaluation (Q86–Q100)

---

### Q86. Feature scaling for KNN
Feature A dominates distance: $d = \sqrt{(\Delta A)^2 + (\Delta B)^2}$ where $\Delta A$ up to 1000 but $\Delta B$ up to 1. KNN effectively ignores Feature B. **Fix:** Standardize or normalize both features to same scale.

---

### Q87. One-hot encoding
Red = [1,0,0], Blue = [0,1,0], Green = [0,0,1].

Label encoding (R=0, B=1, G=2) implies ordinal relationship (Green > Blue > Red) which doesn't exist for nominal data. Linear models would learn meaningless numeric relationships.

---

### Q88. LASSO feature selection
The model uses **20 features** (100 - 80 = 20 non-zero coefficients). LASSO's L1 penalty drives coefficients to exactly zero, performing automatic feature selection.

---

### Q89. DBSCAN classification
With ε=1.5, MinPts=2:
- **A(0,0):** Neighbors within 1.5: B(1,0), C(1,1). Count=2 ≥ MinPts → **Core**
- **B(1,0):** Neighbors: A(0,0), C(1,1). Count=2 ≥ MinPts → **Core**
- **C(1,1):** Neighbors: A(0,0), B(1,0). Count=2 ≥ MinPts → **Core**
- **D(4,4):** Neighbors: E(4,5), F(5,4). Count=2 ≥ MinPts → **Core**
- **E(4,5):** Neighbors: D(4,4), F at dist=√2≈1.41. Count=2 → **Core**
- **F(5,4):** Neighbors: D(4,4), E at dist=√2≈1.41. Count=2 → **Core**

Two clusters: {A,B,C} and {D,E,F}, no noise points.

---

### Q90. K-Means vs DBSCAN
| Aspect | K-Means | DBSCAN |
|---|---|---|
| # clusters | Must specify k | Auto-detected |
| Outliers | Assigns all points to clusters | Labels outliers as noise |
| Cluster shape | Convex/spherical only | Arbitrary shapes |

---

### Q91. EM steps for GMM
**E-step:** Compute the **responsibility** $\gamma(z_{nk})$ = posterior probability that point $n$ belongs to cluster $k$, using current parameters.

**M-step:** Update parameters $\mu_k, \Sigma_k, \pi_k$ using weighted averages where weights are the responsibilities from E-step.

---

### Q92. EM convergence
**(b) Local maximum.** EM guarantees the log-likelihood is non-decreasing and converges to a local maximum. It does NOT guarantee the global maximum (depends on initialization, like K-Means).

---

### Q93. K-Means as special case of EM
K-Means = EM with GMM where:
- All covariance matrices are $\sigma^2 I$ (spherical, same variance)
- As $\sigma \to 0$, soft responsibilities become hard assignments (0 or 1)
- M-step becomes simple centroid computation
- Essentially: EM with hard E-step and identity covariance

---

### Q94. Silhouette score
$$s(i) = \frac{b(i) - a(i)}{\max(a(i), b(i))} = \frac{5 - 2}{\max(2, 5)} = \frac{3}{5} = 0.6$$

A score of 0.6 indicates the point is reasonably well-clustered.

---

### Q95. Silhouette range
Silhouette score ranges from **-1** to **+1**. A score close to -1 means the point is **probably assigned to the wrong cluster** (closer to another cluster than its own).

---

### Q96. Accuracy on imbalanced data
Accuracy = 95/100 = **95%**. But the model learned nothing — it just predicts the majority class. Accuracy is misleading because it doesn't account for the 5% positive class that matters most.

Better metrics: **F1-score** (harmonic mean of precision and recall), **AUC-ROC** (threshold-independent), **Precision-Recall curve**, or use stratified evaluation.

---

### Q97. Dummy variable trap
If a categorical variable with k categories is one-hot encoded into k columns, there's **perfect multicollinearity** (any column = 1 - sum of others). Linear regression's $X^TX$ becomes singular.

**Fix:** Drop one column → use k-1 columns. The dropped category becomes the "reference."

---

### Q98. SMOTE
SMOTE creates synthetic minority class samples by:
1. Pick a minority sample $x_i$
2. Find its k nearest minority neighbors
3. Pick one neighbor $x_j$ randomly
4. Create new sample: $x_{\text{new}} = x_i + \lambda(x_j - x_i)$ where $\lambda \in [0,1]$ is random

Better than random oversampling because: random oversampling duplicates existing points → model memorizes them (overfits). SMOTE creates **new** points in feature space → more generalization.

---

### Q99. Feature selection methods comparison
| Type | Example | Approach |
|---|---|---|
| **Filter** | Correlation threshold | Rank features independently; fast |
| **Wrapper** | Recursive Feature Elimination (RFE) | Train model iteratively; expensive but accurate |
| **Embedded** | LASSO (L1) | Feature selection during model training |

---

### Q100. Pseudo-residuals in gradient boosting
Pseudo-residuals are the **negative gradient** of the loss function with respect to the model's prediction:

$$r_{im} = -\frac{\partial L(y_i, F(x_i))}{\partial F(x_i)}\bigg|_{F=F_{m-1}}$$

For **MSE loss** $L = \frac{1}{2}(y - F(x))^2$: the negative gradient is $(y_i - F_{m-1}(x_i))$ = the **actual residuals**. So gradient boosting with MSE literally fits a new tree to the residuals of the current model.

---

*End of Solutions — Machine Learning for GATE 2027 — 100 Solutions*
