# Machine Learning — Questions and PYQs for GATE 2027

> **Total Questions:** 75 | **Sections:** 10
> **Difficulty Mix:** Easy (25%) | Medium (40%) | GATE PYQ level (35%)

---

## Section A: Linear Regression (8 Questions)

**Q1.** Given data points (1,2), (2,4), (3,5), (4,4), (5,5). Find the least-squares regression line $\hat{y} = w_0 + w_1 x$.

**Q2.** For the model $\hat{y} = Xw$, derive the normal equation.

**Q3.** Compute $R^2$ for the model in Q1.

**Q4.** If $X^TX$ is singular, what does that imply about the features? What are two ways to handle it?

**Q5.** [GATE] In polynomial regression of degree d with n data points, what is the minimum n required for a unique solution?

**Q6.** Given the normal equation solution $w = (X^TX)^{-1}X^Ty$, what is the time complexity of computing $w$ for n samples and d features?

**Q7.** If you add a feature perfectly correlated with an existing feature to a linear regression, what happens to $X^TX$?

**Q8.** [GATE DA] A linear regression model has $R^2 = 0.85$. Adding a new irrelevant feature, what happens to $R^2$ and adjusted $R^2$?

---

## Section B: Gradient Descent & Optimization (8 Questions)

**Q9.** Write the gradient descent update rule for MSE cost in multiple linear regression.

**Q10.** If learning rate $\alpha = 0.01$ and gradient = [2, -3, 1], what are the new weights after one step from $w = [0.5, 1.0, 0.3]$?

**Q11.** Compare batch GD, mini-batch GD, and SGD in terms of convergence smoothness and computation per step.

**Q12.** [GATE] For a convex cost function, does gradient descent always converge to the global minimum? What about non-convex?

**Q13.** What happens if the learning rate is too large for gradient descent on MSE?

**Q14.** Feature scaling (standardization) speeds up gradient descent. Explain why.

**Q15.** Derive the gradient of the cost function $J(w) = \frac{1}{2n}\|Xw - y\|^2$.

**Q16.** [GATE DA] In SGD with n training examples, how many gradient computations per epoch compared to batch GD?

---

## Section C: Regularization (7 Questions)

**Q17.** Write the cost functions for Ridge and LASSO regression.

**Q18.** For Ridge regression with $\lambda = 2$, compute the closed-form weights given $X^TX = \begin{pmatrix} 4 & 2 \\ 2 & 3 \end{pmatrix}$ and $X^Ty = \begin{pmatrix} 8 \\ 5 \end{pmatrix}$.

**Q19.** Why does LASSO produce sparse solutions while Ridge does not? (Geometric explanation)

**Q20.** [GATE] As $\lambda \to \infty$ in Ridge regression, what happens to the weights? What is the training error?

**Q21.** If you have 10 features but suspect only 3 are important, would you use Ridge or LASSO? Why?

**Q22.** [GATE DA] A model has training MSE = 0.02 and test MSE = 0.5. Is this overfitting or underfitting? Which regularization technique would help?

**Q23.** In elastic net, what are the benefits of combining L1 and L2 penalties?

---

## Section D: Logistic Regression & Softmax (8 Questions)

**Q24.** For logistic regression, derive the gradient of the binary cross-entropy loss.

**Q25.** If $w^Tx = 0$, what is the predicted probability $P(y=1|x)$?

**Q26.** Why is MSE not suitable as a loss function for logistic regression?

**Q27.** [GATE] Plot the sigmoid function. At what value of $z$ is the derivative maximum?

**Q28.** For 3-class classification with softmax outputs [0.7, 0.2, 0.1] and true class = 1 (first class), compute the cross-entropy loss.

**Q29.** [GATE DA] A logistic regression model with threshold 0.5 predicts class 1 for all points. How would you adjust the threshold to reduce false positives?

**Q30.** Logistic regression decision boundary is linear in input space. Can it handle non-linear boundaries? How?

**Q31.** [GATE] In logistic regression, if a feature is multiplied by 100, how does it affect the model?

---

## Section E: SVM (8 Questions)

**Q32.** For data points: class +1: (1,1), (2,2); class −1: (0,0), (1,0). Find the maximum margin hyperplane.

**Q33.** What is the margin of a hard-margin SVM in terms of $w$?

**Q34.** In soft-margin SVM, what does the parameter C control?

**Q35.** [GATE] Identify the support vectors for: class +1: (3,3), (4,3), (3,4); class −1: (1,1), (0,1), (1,0).

**Q36.** What is the hinge loss? When is it zero?

**Q37.** [GATE DA] Why can't regular SVM classify XOR data? How does kernel SVM solve this?

**Q38.** For RBF kernel with $\gamma = 0.1$, compute $K(x_1, x_2)$ where $x_1 = [1,0]$ and $x_2 = [0,1]$.

**Q39.** [GATE] In SVM, increasing C tends to (increase/decrease) the margin. Does this lead to overfitting or underfitting?

---

## Section F: KNN, Naive Bayes, Decision Trees (10 Questions)

**Q40.** For data: (+): (1,1), (2,1), (1,2); (−): (4,4), (5,4). Classify (3,3) using 3-NN with Euclidean distance.

**Q41.** What is the effect of choosing k=1 vs k=n in KNN?

**Q42.** [GATE DA] A Naive Bayes classifier has $P(C_1)=0.4$, $P(C_2)=0.6$. Given features: $P(x_1|C_1)=0.7$, $P(x_2|C_1)=0.3$, $P(x_1|C_2)=0.4$, $P(x_2|C_2)=0.8$. Classify a sample with $x_1=1, x_2=1$.

**Q43.** Why is Naive Bayes called "naive"?

**Q44.** Compute entropy of a dataset with 5 positive and 5 negative examples.

**Q45.** [GATE] A dataset has 10 samples: 6 positive, 4 negative. Feature A splits into: Left (4+, 1−), Right (2+, 3−). Compute information gain.

**Q46.** What is the Gini impurity for a node with class distribution [0.3, 0.7]?

**Q47.** [GATE DA] Compare information gain and Gini impurity as split criteria. When might they give different results?

**Q48.** Explain pre-pruning vs post-pruning in decision trees.

**Q49.** [GATE] How many leaf nodes does a decision tree with depth d have at most?

---

## Section G: PCA & LDA (7 Questions)

**Q50.** Given covariance matrix $\Sigma = \begin{pmatrix} 4 & 2 \\ 2 & 3 \end{pmatrix}$, find the principal components and variance explained by each.

**Q51.** After PCA, the first 3 components explain 55%, 25%, 12% of variance. How many components capture ≥90%?

**Q52.** [GATE DA] Why must data be centered (mean-subtracted) before applying PCA?

**Q53.** In LDA for 5 classes, what is the maximum number of discriminant components?

**Q54.** [GATE] PCA is unsupervised. True or false? Justify.

**Q55.** A dataset has 100 features. After PCA, 95% of variance is captured by 10 components. What does this tell you about the data?

**Q56.** [GATE DA] Compare PCA and LDA in terms of objective, supervision, and max components.

---

## Section H: Clustering (6 Questions)

**Q57.** Given points {1, 2, 8, 9, 10} and initial centroids {2, 9}, perform 2 iterations of k-means.

**Q58.** Why is k-means sensitive to initialization? What is k-means++?

**Q59.** [GATE DA] K-means is applied with k=3. After convergence, cluster sizes are 100, 5, and 2. What might have gone wrong?

**Q60.** Compare single-linkage, complete-linkage, and Ward's method in hierarchical clustering.

**Q61.** [GATE] K-means minimizes which objective function?

**Q62.** Can k-means handle non-convex clusters? Why or why not?

---

## Section I: Neural Networks (6 Questions)

**Q63.** A perceptron has weights $w = [0.5, -0.3]$ and bias $b = 0.1$. Classify input $x = [1, 1]$ using sign activation.

**Q64.** Why can't a single perceptron solve XOR?

**Q65.** [GATE DA] A neural network has 3 inputs, a hidden layer with 4 neurons, and 2 outputs. How many weights (including biases)?

**Q66.** Derive the backpropagation update for a single weight in a 2-layer network with sigmoid activation and MSE loss.

**Q67.** [GATE] Compare sigmoid, tanh, and ReLU activations. Which solves vanishing gradient?

**Q68.** [GATE DA] What does the universal approximation theorem state?

---

## Section J: Evaluation & Bias-Variance (7 Questions)

**Q69.** A model has: TP=80, FP=20, FN=10, TN=90. Compute accuracy, precision, recall, F1-score.

**Q70.** [GATE DA] In a medical diagnosis where missing a disease is critical, should you optimize precision or recall?

**Q71.** Explain the ROC curve. What does AUC = 0.5 mean?

**Q72.** In 5-fold cross-validation with 1000 samples, how many samples in each training and test fold?

**Q73.** [GATE] A model has low training error and high test error. Is this high bias or high variance? How to fix it?

**Q74.** Write the bias-variance decomposition of expected test error.

**Q75.** [GATE DA] A linear model underfits the data. You have two options: (a) add polynomial features (b) increase regularization. Which should you choose and why?

---

*End of Questions — Machine Learning for GATE 2027*

---

## Section K: Ensemble Methods (Q76–Q85)

**Q76.** [GATE DA] In a Random Forest with 50 features, how many features are considered at each split for classification? For regression?

**Q77.** In bagging, approximately what fraction of training samples are NOT included in a single bootstrap sample? What are these called?

**Q78.** [GATE] Compare bagging and boosting: which one primarily reduces bias? Which reduces variance?

**Q79.** In AdaBoost, after iteration t a weak learner has weighted error $\epsilon_t = 0.3$. Compute the model weight $\alpha_t$.

**Q80.** [GATE DA] In gradient boosting with learning rate $\eta = 0.1$, how does reducing $\eta$ (closer to 0) affect: (a) number of trees needed, (b) overfitting risk, (c) training time?

**Q81.** Why does Random Forest NOT overfit as you add more trees, while boosting can?

**Q82.** [GATE] In AdaBoost, if a weak learner achieves exactly 50% error (random), what is $\alpha_t$? What does this mean?

**Q83.** [GATE DA] What key innovation does XGBoost add compared to standard gradient boosting? (Name at least 3 features.)

**Q84.** A stacking ensemble uses Linear Regression, SVM, and Decision Tree as level-0 learners. What is the input to the level-1 meta-learner?

**Q85.** [GATE] In Random Forest, each tree is grown without pruning. Why doesn't this cause overfitting?

---

## Section L: Preprocessing, DBSCAN, EM & Evaluation (Q86–Q100)

**Q86.** [GATE DA] Feature A ranges [0, 1000] and Feature B ranges [0, 1]. You apply KNN. What problem occurs? How to fix it?

**Q87.** A categorical feature "Color" has values {Red, Blue, Green}. Show one-hot encoding. Why is label encoding (R=0, B=1, G=2) problematic for linear models?

**Q88.** [GATE] Given a dataset with 100 features, LASSO regularization sets 80 coefficients to zero. How many features does the model use?

**Q89.** Given points A(0,0), B(1,0), C(1,1), D(4,4), E(4,5), F(5,4). With ε=1.5 and MinPts=2, classify each point as core, border, or noise using DBSCAN.

**Q90.** [GATE DA] Compare K-Means and DBSCAN on: (a) number of clusters, (b) outlier handling, (c) cluster shape.

**Q91.** In the EM algorithm for GMM, what is computed in the E-step? What is updated in the M-step?

**Q92.** [GATE] EM algorithm guarantees convergence to: (a) global maximum, (b) local maximum, (c) global minimum. Which?

**Q93.** [GATE DA] How is K-Means a special case of EM with GMM?

**Q94.** For a clustering with two clusters, point $i$ has $a(i) = 2$ (avg distance to own cluster) and $b(i) = 5$ (avg distance to nearest other cluster). Compute the silhouette score $s(i)$.

**Q95.** [GATE] Silhouette score ranges from ___ to ___. A score close to -1 means ___.

**Q96.** [GATE DA] You have a dataset with class distribution: 95% negative, 5% positive. A model predicts all negative. What is the accuracy? Why is accuracy misleading? What metric should you use?

**Q97.** [GATE] What is the dummy variable trap? How do you avoid it?

**Q98.** [GATE DA] Explain SMOTE (Synthetic Minority Over-sampling Technique). Why is it preferred over simple random oversampling?

**Q99.** Compare filter, wrapper, and embedded feature selection methods with one example each.

**Q100.** [GATE DA] For gradient boosting, what are "pseudo-residuals"? For MSE loss, what do they simplify to?



# 📝 Practice Set 1: Conceptual Questions (P1 – P100)

| # | Question | Answer |
|---|---|---|
| P1 | ML learns from? | Data (experience) |
| P2 | Supervised learning needs? | Labeled data |
| P3 | Unsupervised learning needs? | Unlabeled data |
| P4 | Classification output type? | Discrete (categories) |
| P5 | Regression output type? | Continuous (numbers) |
| P6 | Clustering is? | Unsupervised |
| P7 | Linear regression minimizes? | Sum of squared errors (SSE/MSE) |
| P8 | Logistic regression outputs? | Probability (0 to 1) |
| P9 | Logistic regression loss? | Binary cross-entropy |
| P10 | SVM maximizes? | Margin ($\frac{2}{\|w\|}$) |
| P11 | SVM support vectors are? | Points closest to decision boundary (on margin) |
| P12 | KNN is lazy or eager? | Lazy (no training!) |
| P13 | KNN stores? | Entire training dataset |
| P14 | Naive Bayes assumes? | Feature independence (conditional) |
| P15 | Decision tree split criterion (ID3)? | Information Gain |
| P16 | Decision tree split criterion (CART)? | Gini Impurity |
| P17 | PCA is supervised? | No (unsupervised) |
| P18 | LDA is supervised? | Yes |
| P19 | K-means is supervised? | No (unsupervised) |
| P20 | Ensemble methods combine? | Multiple weak learners |
| P21 | Bagging reduces? | Variance |
| P22 | Boosting reduces? | Bias |
| P23 | Random Forest is based on? | Bagging + feature subsampling |
| P24 | AdaBoost is based on? | Boosting |
| P25 | Gradient descent direction? | Negative gradient (steepest descent) |
| P26 | Large learning rate risk? | Divergence (overshooting) |
| P27 | Small learning rate risk? | Slow convergence, stuck in local min |
| P28 | Overfitting = ? | Low training error, high test error |
| P29 | Underfitting = ? | High training AND test error |
| P30 | Regularization prevents? | Overfitting |
| P31 | L1 regularization name? | LASSO |
| P32 | L2 regularization name? | Ridge |
| P33 | L1 can make weights exactly 0? | Yes (feature selection!) |
| P34 | L2 can make weights exactly 0? | No (only shrinks toward 0) |
| P35 | Elastic Net combines? | L1 + L2 |
| P36 | $R^2 = 1$ means? | Perfect fit |
| P37 | $R^2 = 0$ means? | Model = predicting mean |
| P38 | $R^2 < 0$ possible? | Yes (model worse than mean!) |
| P39 | Adjusted $R^2$ accounts for? | Number of features (penalizes extras) |
| P40 | Confusion matrix diagonal? | Correct predictions |
| P41 | Precision = ? | $\frac{TP}{TP + FP}$ |
| P42 | Recall = ? | $\frac{TP}{TP + FN}$ |
| P43 | F1 score = ? | $\frac{2 \times P \times R}{P + R}$ (harmonic mean) |
| P44 | ROC curve plots? | TPR vs FPR |
| P45 | AUC = 1 means? | Perfect classifier |
| P46 | AUC = 0.5 means? | Random classifier |
| P47 | Accuracy misleading when? | Imbalanced classes |
| P48 | Type I error = ? | False Positive (FP) |
| P49 | Type II error = ? | False Negative (FN) |
| P50 | Specificity = ? | $\frac{TN}{TN + FP}$ = 1 - FPR |
| P51 | Sensitivity = Recall = ? | TPR = $\frac{TP}{TP + FN}$ |
| P52 | Gradient of MSE w.r.t. $\hat{y}$? | $\frac{2}{n}(\hat{y} - y)$ |
| P53 | Normal equation formula? | $\theta = (X^T X)^{-1} X^T y$ |
| P54 | Normal equation time complexity? | $O(d^3)$ (matrix inversion) |
| P55 | Normal equation limitation? | $X^T X$ must be invertible |
| P56 | Regularization fixes singularity? | Yes — $(X^T X + \lambda I)^{-1}$ always invertible |
| P57 | Sigmoid function range? | (0, 1) |
| P58 | Softmax ensures? | Outputs sum to 1 |
| P59 | Cross-entropy loss for multi-class? | $-\sum_c y_c \log \hat{y}_c$ |
| P60 | Perceptron convergence guarantee? | Only for linearly separable data |
| P61 | Perceptron can learn XOR? | No |
| P62 | MLP can learn XOR? | Yes (with hidden layer) |
| P63 | Universal approximation needs? | 1 hidden layer (but may need many neurons) |
| P64 | Vanishing gradient affects? | Deep sigmoid/tanh networks |
| P65 | ReLU solves vanishing gradient? | Partially (but dying ReLU issue) |
| P66 | Dying ReLU = ? | Neuron always outputs 0 (gradient = 0) |
| P67 | Leaky ReLU fixes? | Dying ReLU (small slope for negative) |
| P68 | Batch normalization helps? | Training speed, reduces internal covariate shift |
| P69 | Dropout is? | Regularization (randomly disable neurons) |
| P70 | Dropout rate 0.5 means? | 50% of neurons disabled each step |
| P71 | Epoch = ? | One pass through entire dataset |
| P72 | Batch size = ? | Samples per gradient update |
| P73 | Iteration = ? | One gradient update step |
| P74 | If 1000 samples, batch=100, iterations per epoch? | 10 |
| P75 | K-means algorithm converges to? | Local optimum (not global!) |
| P76 | K-means++ advantage? | Better initialization → better convergence |
| P77 | Elbow method finds? | Optimal k by plotting k vs inertia |
| P78 | Silhouette score range? | [-1, 1] (1 = perfect) |
| P79 | DBSCAN needs k? | No! Finds clusters automatically |
| P80 | DBSCAN parameters? | $\epsilon$ (radius), MinPts (min points) |
| P81 | DBSCAN can find? | Arbitrary shape clusters + outliers |
| P82 | Hierarchical clustering: agglomerative is? | Bottom-up (merge) |
| P83 | Hierarchical clustering: divisive is? | Top-down (split) |
| P84 | Dendrogram shows? | Merging sequence in hierarchical clustering |
| P85 | GMM models clusters as? | Gaussian distributions |
| P86 | EM algorithm alternates? | E-step (assign soft) and M-step (update params) |
| P87 | EM convergence? | Guaranteed to local optimum |
| P88 | K-means is special case of? | GMM (hard EM with equal spherical Gaussians) |
| P89 | Curse of dimensionality means? | Distance metrics fail in high-D |
| P90 | Fix for curse of dimensionality? | PCA, feature selection, regularization |
| P91 | Feature scaling important for? | KNN, SVM, K-means, gradient descent |
| P92 | Feature scaling NOT important for? | Decision trees, Random Forest, Naive Bayes |
| P93 | Min-max scaling range? | [0, 1] |
| P94 | Standardization (Z-score) gives? | Mean=0, Std=1 |
| P95 | One-hot encoding converts? | Categories to binary vectors |
| P96 | Label encoding converts? | Categories to integers |
| P97 | Label encoding problem? | Implies ordering among categories! |
| P98 | Missing data: mean imputation bias? | Reduces variance, can distort distribution |
| P99 | SMOTE is for? | Oversampling minority class (imbalanced data) |
| P100 | Stratified sampling preserves? | Class proportions |

---

# 📝 Practice Set 2: Mathematical Problems (P101 – P200)

### Regression Problems

**P101.** Given $X = [1, 2, 3, 4, 5]$, $Y = [2, 4, 5, 4, 5]$. Find $\beta_1$ and $\beta_0$.
→ $\bar{x}=3, \bar{y}=4$. $\beta_1 = \frac{\sum(x-\bar{x})(y-\bar{y})}{\sum(x-\bar{x})^2} = \frac{6}{10} = 0.6$. $\beta_0 = 4 - 0.6(3) = 2.2$
**Answer:** $y = 2.2 + 0.6x$

**P102.** For the above, predict y when x = 7.
→ $y = 2.2 + 0.6(7) = 6.4$. **Answer: 6.4**

**P103.** For the above, what is $R^2$?
→ From worked example: **$R^2 = 0.6$**

**P104.** Ridge regression cost with $\lambda = 0.5$, $w = [2, 3]$?
→ Penalty = $\lambda \sum w_i^2 = 0.5 \times (4 + 9) = 6.5$. Added to MSE.

**P105.** LASSO cost with $\lambda = 0.5$, $w = [2, 3]$?
→ Penalty = $\lambda \sum |w_i| = 0.5 \times (2 + 3) = 2.5$. Added to MSE.

**P106-P120** — Quick regression computations:

| # | Given | Find | Answer |
|---|---|---|---|
| P106 | $\hat{y} = 3x + 2$, $x = 5$ | $\hat{y}$ | 17 |
| P107 | $\hat{y} = -2x + 10$, $x = 3$ | $\hat{y}$ | 4 |
| P108 | $SS_{res} = 20$, $SS_{tot} = 100$ | $R^2$ | 0.8 |
| P109 | $R^2 = 0.75$, $SS_{tot} = 200$ | $SS_{res}$ | 50 |
| P110 | $\beta_1 = 2, \beta_0 = 1$, actual $y = 7$ at $x = 3$ | Residual | $7 - 7 = 0$ |
| P111 | $y = [3, 5, 7]$, $\hat{y} = [2.5, 5.5, 7]$ | MSE | $\frac{0.25+0.25+0}{3} = 0.167$ |
| P112 | MAE for above | | $\frac{0.5+0.5+0}{3} = 0.333$ |
| P113 | RMSE for above | | $\sqrt{0.167} = 0.408$ |
| P114 | $n = 100, d = 10$. Normal eq. or GD? | | GD (d is small, but normal eq also OK since $d^3=1000$) |
| P115 | $n = 100, d = 10000$. Normal eq. or GD? | | GD (d too large for $d^3$ inversion!) |
| P116 | $\bar{x} = 5, \bar{y} = 10, \beta_1 = 2$ | $\beta_0$ | $10 - 2(5) = 0$ |
| P117 | Correlation $r = 0.8$ | $R^2$ (simple regression) | $0.8^2 = 0.64$ |
| P118 | $R^2 = 0.81$ | $r$ (if positive slope) | $\sqrt{0.81} = 0.9$ |
| P119 | Adding feature, $R^2$ decreases? | | Impossible! $R^2$ never decreases with more features |
| P120 | Adding feature, adjusted $R^2$ decreases? | | Yes possible! If feature is useless |

### Probability & Bayes Problems

**P121.** P(Spam) = 0.3, P("free"|Spam) = 0.8, P("free"|Not Spam) = 0.1. P(Spam|"free") = ?
→ $P(S|f) = \frac{P(f|S)P(S)}{P(f|S)P(S) + P(f|\bar{S})P(\bar{S})} = \frac{0.8 \times 0.3}{0.8 \times 0.3 + 0.1 \times 0.7} = \frac{0.24}{0.31} = 0.774$
**Answer: 0.774**

**P122.** Two features: $x_1$, $x_2$. P(C1) = 0.6, P(C2) = 0.4.
P($x_1$|C1) = 0.7, P($x_2$|C1) = 0.5. P($x_1$|C2) = 0.3, P($x_2$|C2) = 0.8.
→ P(C1|$x_1,x_2$) $\propto$ 0.6 × 0.7 × 0.5 = 0.21
→ P(C2|$x_1,x_2$) $\propto$ 0.4 × 0.3 × 0.8 = 0.096
→ Normalized: P(C1) = 0.21/(0.21+0.096) = 0.686. **Classify as C1.**

### Entropy & Information Theory

**P123.** Entropy of coin with P(H) = 0.75.
→ $H = -0.75\log_2 0.75 - 0.25\log_2 0.25 = 0.311 + 0.5 = 0.811$ bits

**P124.** Entropy of fair 4-sided die.
→ $H = -4 \times 0.25 \log_2 0.25 = -4 \times 0.25 \times (-2) = 2$ bits

**P125.** 10 samples: 7 Yes, 3 No. Entropy?
→ $H = -\frac{7}{10}\log_2\frac{7}{10} - \frac{3}{10}\log_2\frac{3}{10} = 0.361 + 0.521 = 0.882$ bits

**P126.** After split: Left = (5 Yes, 1 No), Right = (2 Yes, 2 No). Weighted entropy?
→ $H_L = -\frac{5}{6}\log_2\frac{5}{6} - \frac{1}{6}\log_2\frac{1}{6} = 0.650$
→ $H_R = 1.0$ (exact 50-50)
→ $H_{split} = \frac{6}{10}(0.650) + \frac{4}{10}(1.0) = 0.39 + 0.4 = 0.79$

**P127.** Information gain = ?
→ $IG = H_{parent} - H_{split} = 0.882 - 0.79 = 0.092$ bits

**P128.** Gini of Left node (5 Yes, 1 No)?
→ $Gini = 1 - (5/6)^2 - (1/6)^2 = 1 - 0.694 - 0.028 = 0.278$

**P129.** Gini of parent (7 Yes, 3 No)?
→ $Gini = 1 - (0.7)^2 - (0.3)^2 = 1 - 0.49 - 0.09 = 0.42$

**P130.** KL Divergence: P = [0.5, 0.5], Q = [0.75, 0.25]?
→ $D_{KL}(P\|Q) = 0.5\log\frac{0.5}{0.75} + 0.5\log\frac{0.5}{0.25} = 0.5(-0.585) + 0.5(1) = 0.207$ bits

### SVM Problems

**P131.** SVM: $\mathbf{w} = [1, 2]$, $b = -3$. Margin?
→ $\|\mathbf{w}\| = \sqrt{1+4} = \sqrt{5}$. Margin = $\frac{2}{\sqrt{5}} = 0.894$

**P132.** Point (3, 2) classification with above boundary?
→ $f(x) = 1(3) + 2(2) - 3 = 4 > 0$. **Class: +1**

**P133.** Point (1, 0) classification?
→ $f(x) = 1(1) + 2(0) - 3 = -2 < 0$. **Class: -1**

**P134.** Distance of (3, 2) from boundary?
→ $d = \frac{|1(3) + 2(2) - 3|}{\sqrt{5}} = \frac{4}{\sqrt{5}} = 1.789$

**P135.** Is (2, 0.5) a support vector? (margin = 0.894)
→ $d = \frac{|1(2) + 2(0.5) - 3|}{\sqrt{5}} = \frac{|0|}{\sqrt{5}} = 0$. ON the boundary! Not support vector.
→ Support vector: $d = \frac{1}{\sqrt{5}} = 0.447$ (half-margin distance). So NO.

### Sigmoid & Logistic Regression

**P136-P145** — Sigmoid computations:

| # | $z$ | $\sigma(z)$ | Class (threshold=0.5) |
|---|---|---|---|
| P136 | 0 | 0.5 | Boundary |
| P137 | 1 | 0.731 | 1 |
| P138 | -1 | 0.269 | 0 |
| P139 | 2 | 0.881 | 1 |
| P140 | -2 | 0.119 | 0 |
| P141 | 5 | 0.993 | 1 |
| P142 | -5 | 0.007 | 0 |
| P143 | 0.5 | 0.622 | 1 |
| P144 | -0.5 | 0.378 | 0 |
| P145 | 10 | ≈1 | 1 |

### Confusion Matrix Problems

**P146.** Given confusion matrix:

|  | Predicted + | Predicted − |
|---|---|---|
| **Actual +** | 40 | 10 |
| **Actual −** | 5 | 45 |

→ TP=40, FN=10, FP=5, TN=45, Total=100

| Metric | Calculation | Answer |
|---|---|---|
| P147: Accuracy | (40+45)/100 | 0.85 |
| P148: Precision | 40/(40+5) | 0.889 |
| P149: Recall | 40/(40+10) | 0.80 |
| P150: F1 | 2×0.889×0.8/(0.889+0.8) | 0.842 |
| P151: Specificity | 45/(45+5) | 0.90 |
| P152: FPR | 5/(5+45) | 0.10 |
| P153: FNR | 10/(10+40) | 0.20 |

**P154.** Cancer screening: 100 patients, 5 have cancer.
Model predicts: TP=4, FP=10, TN=85, FN=1.
- Accuracy = 89/100 = 89% (looks great!)
- Recall = 4/5 = 80% (missed 1 cancer patient!)
- Precision = 4/14 = 28.6% (many false alarms)
→ **Accuracy misleading for imbalanced data!** Use Recall for medical tests.

### PCA Problems

**P155.** Eigenvalues of covariance: $\lambda = [5, 3, 1.5, 0.5]$. Variance explained by top 2?
→ Total = 10. Top 2 = 8. **80%**

**P156.** Need ≥ 95% explained variance. How many components?
→ Top 3 = 9.5. 9.5/10 = 95%. **3 components.**

**P157.** Original: 100 features. After PCA: 10 components. Reduced weight matrix dimensions?
→ **100 × 10** (projection matrix)

### K-Means Problems

**P158.** Points: (0,0), (1,0), (0,1), (10,10), (11,10), (10,11). k=2. Final centroids?
→ Cluster 1: {(0,0), (1,0), (0,1)} → centroid $(1/3, 1/3)$
→ Cluster 2: {(10,10), (11,10), (10,11)} → centroid $(31/3, 31/3)$

**P159.** K-means inertia (within-cluster sum of squares) for above?
→ Cluster 1: $(0-1/3)^2 + (0-1/3)^2 + (1-1/3)^2 + (0-1/3)^2 + (0-1/3)^2 + (1-1/3)^2 = 2/9+1/9+4/9+1/9+1/9+4/9 = 4/3$
→ Similarly for Cluster 2. **Total inertia ≈ 2.67**

**P160-P200** — Rapid fire calculations:

| # | Problem | Answer |
|---|---|---|
| P160 | MSE: $y=[1,2,3], \hat{y}=[1.5,2,2.5]$ | $\frac{0.25+0+0.25}{3}=0.167$ |
| P161 | MAE for above | $\frac{0.5+0+0.5}{3}=0.333$ |
| P162 | RMSE for above | $\sqrt{0.167}=0.408$ |
| P163 | Euclidean distance: (1,2) to (4,6) | $\sqrt{9+16}=5$ |
| P164 | Manhattan distance: (1,2) to (4,6) | $3+4=7$ |
| P165 | Cosine similarity: [1,0] and [1,1] | $\frac{1}{\sqrt{2}}=0.707$ |
| P166 | Cosine similarity: [1,0] and [0,1] | 0 (perpendicular!) |
| P167 | Cosine similarity: [1,0] and [-1,0] | -1 (opposite!) |
| P168 | Entropy: [50%, 50%] | 1 bit |
| P169 | Entropy: [100%, 0%] | 0 bits |
| P170 | Entropy: [25%, 25%, 25%, 25%] | 2 bits |
| P171 | Gini: [50%, 50%] | 0.5 |
| P172 | Gini: [100%, 0%] | 0 |
| P173 | Gini: [80%, 20%] | 0.32 |
| P174 | Gini: [33%, 33%, 34%] | $\approx 0.667$ |
| P175 | NN params: [10, 5, 1] | $(10×5+5)+(5×1+1)=56$ |
| P176 | NN params: [784, 256, 10] | $(784×256+256)+(256×10+10)=203,530$ |
| P177 | Sigmoid(0) | 0.5 |
| P178 | Sigmoid derivative at z=0 | $0.5×0.5=0.25$ |
| P179 | Softmax([1,1,1]) | [1/3, 1/3, 1/3] |
| P180 | Softmax([10,0,0]) | [≈1, ≈0, ≈0] |
| P181 | ReLU(-5) | 0 |
| P182 | ReLU(3) | 3 |
| P183 | Leaky ReLU(-5, α=0.01) | -0.05 |
| P184 | Tanh(0) | 0 |
| P185 | Norm of [3,4] | 5 |
| P186 | Norm of [1,1,1,1] | 2 |
| P187 | 5-fold CV: train size if n=100? | 80 per fold |
| P188 | LOOCV: train size if n=100? | 99 per fold |
| P189 | Elastic Net penalty ($\alpha=0.5, \lambda=1$) for $w=[1,2]$ | $0.5×(1+4) + 0.5×(1+2) = 2.5+1.5=4$ |
| P190 | Min-Max scale: $x=5$, min=0, max=10 | 0.5 |
| P191 | Z-score: $x=5$, $\mu=3$, $\sigma=2$ | 1.0 |
| P192 | One-hot encode: 3 categories → vector size? | 3 |
| P193 | KNN: k=1, training accuracy? | 100% (always!) |
| P194 | KNN: odd k prevents? | Ties in binary classification |
| P195 | Decision tree depth for XOR? | 2 |
| P196 | Information Gain: H(parent)=1.0, H(split)=0.6 | IG = 0.4 |
| P197 | Gain Ratio: IG=0.4, SplitInfo=0.8 | GR = 0.5 |
| P198 | Silhouette: $a=2, b=5$ | $\frac{5-2}{\max(2,5)} = 0.6$ |
| P199 | Silhouette: $a=5, b=2$ | $\frac{2-5}{\max(5,2)} = -0.6$ |
| P200 | AUC of random line (diagonal) | 0.5 |

---

# 📝 Practice Set 3: True/False & Common Traps (P201 – P300)

| # | Statement | T/F | Explanation |
|---|---|---|---|
| P201 | Linear regression assumes linear relationship | **True** | Between features and output |
| P202 | Linear reg assumes features are independent | **True** | Multicollinearity violates this |
| P203 | Linear reg assumes homoscedasticity | **True** | Equal variance of errors |
| P204 | Linear reg assumes normal errors | **True** | For confidence intervals |
| P205 | Logistic regression is a regression algorithm | **False** | It's for classification! |
| P206 | Logistic regression uses sigmoid | **True** | Maps to probability |
| P207 | Logistic regression loss is MSE | **False** | Cross-entropy loss |
| P208 | SVM works only for 2 classes | **False** | OvA/OvO for multiclass |
| P209 | SVM margin = $\frac{1}{\|w\|}$ | **False** | Margin = $\frac{2}{\|w\|}$ ⚠️ |
| P210 | Kernel trick maps to higher dimension | **True** | Implicitly, via kernel function |
| P211 | RBF kernel has finite-dimensional feature space | **False** | Infinite dimensions! |
| P212 | KNN training time = O(1) | **True** | Lazy — no training! |
| P213 | KNN prediction time = O(n) | **True** | Must check all points |
| P214 | KNN with k=n always predicts majority class | **True** | |
| P215 | Naive Bayes requires feature independence | **True** | (Conditional independence) |
| P216 | Naive Bayes is always worse than other classifiers | **False** | Often works well despite assumption! |
| P217 | Decision tree can overfit perfectly | **True** | Depth = dataset → 100% train acc |
| P218 | Decision tree is stable | **False** | Small data changes → different tree |
| P219 | Random Forest reduces variance | **True** | Bagging effect |
| P220 | Random Forest can overfit | **True** | But less than single tree |
| P221 | AdaBoost reduces bias | **True** | Focuses on hard examples |
| P222 | AdaBoost is immune to overfitting | **False** | Can overfit with noise |
| P223 | PCA can increase dimensions | **False** | Only reduces (or keeps same) |
| P224 | PCA components are orthogonal | **True** | Eigenvectors of covariance |
| P225 | PCA preserves class info | **Not necessarily** | Unsupervised — ignores labels |
| P226 | LDA max components = d | **False** | Max = c - 1 (c = classes) |
| P227 | K-means always converges | **True** | But to local optimum |
| P228 | K-means is deterministic | **False** | Random initialization |
| P229 | K-means with k=n gives SSE=0 | **True** | Each point = one cluster |
| P230 | K-means works for non-convex clusters | **False** | Assumes spherical/convex |
| P231 | DBSCAN needs k parameter | **False** | Needs ε and MinPts |
| P232 | DBSCAN can handle outliers | **True** | Labels as noise |
| P233 | GMM gives soft assignments | **True** | Probability of belonging |
| P234 | K-means gives soft assignments | **False** | Hard assignments |
| P235 | More data always helps | **Not always** | If model is too simple (underfit), more data won't help |
| P236 | More features always helps | **False** | Curse of dimensionality |
| P237 | Regularization always hurts training accuracy | **True** | Adds constraint |
| P238 | Regularization always helps test accuracy | **Not always** | Too much → underfitting |
| P239 | Cross-validation is only for classification | **False** | Also for regression |
| P240 | LOOCV has lowest bias among k-fold | **True** | Trains on n-1 samples |
| P241 | LOOCV has lowest variance | **False** | Highest variance! ⚠️ |
| P242 | High variance = overfitting | **True** | Complex model |
| P243 | High bias = underfitting | **True** | Simple model |
| P244 | Bagging helps high-variance models | **True** | (Like decision trees) |
| P245 | Boosting helps high-bias models | **True** | (Like stumps) |
| P246 | Ensemble always better than single model | **Not guaranteed** | Usually, but not always |
| P247 | Feature scaling needed for decision trees | **False** | Splits based on thresholds |
| P248 | Feature scaling needed for KNN | **True** | Distance-based! |
| P249 | Feature scaling needed for SVM | **True** | Margin depends on scale |
| P250 | Feature scaling needed for Naive Bayes | **False** | Probability-based |

| # | Statement | T/F | Explanation |
|---|---|---|---|
| P251 | Precision and recall always trade off | **True** | Adjusting threshold changes both |
| P252 | F1 = accuracy always | **False** | Different metrics |
| P253 | ROC AUC = 0 means horrible | **False** | Flip predictions → AUC = 1.0! |
| P254 | Precision-Recall curve better for imbalanced | **True** | More informative than ROC |
| P255 | Batch GD uses all data per update | **True** | |
| P256 | SGD uses 1 sample per update | **True** | |
| P257 | Mini-batch GD is most common | **True** | Balance of speed and stability |
| P258 | Momentum helps escape local minima | **True** | Carries velocity |
| P259 | Adam optimizer combines? | **True** | Momentum + RMSProp |
| P260 | Gradient descent finds global min for convex | **True** | |
| P261 | MSE is convex for linear regression | **True** | |
| P262 | Cross-entropy is convex for log. regression | **True** | |
| P263 | NN loss is convex | **False** | Non-convex landscape |
| P264 | Backpropagation is an optimization algorithm | **False** | It's gradient computation! (GD is the optimizer) |
| P265 | Vanishing gradient: gradients → 0 | **True** | Deep sigmoid networks |
| P266 | Exploding gradient: gradients → ∞ | **True** | Deep networks without controls |
| P267 | Weight initialization matters | **True** | Xavier/He initialization |
| P268 | Dropout used during testing? | **False** | Only during training |
| P269 | Batch normalization used during testing? | **True** | With running averages |
| P270 | Data augmentation is regularization | **True** | Prevents overfitting |
| P271 | L1 gives sparse solutions | **True** | Some weights exactly 0 |
| P272 | L2 gives sparse solutions | **False** | Weights shrink but ≠ 0 |
| P273 | MAP with Gaussian prior = Ridge | **True** | |
| P274 | MAP with Laplacian prior = LASSO | **True** | |
| P275 | MLE of variance is biased | **True** | Uses 1/n instead of 1/(n-1) |
| P276 | Decision tree is non-parametric | **True** | No fixed number of parameters |
| P277 | SVM is parametric | **True** | Fixed by support vectors |
| P278 | KNN is non-parametric | **True** | No parameters learned |
| P279 | Naive Bayes is generative | **True** | Models P(X|Y) |
| P280 | Logistic regression is discriminative | **True** | Models P(Y|X) directly |
| P281 | SVM is discriminative | **True** | Finds decision boundary |
| P282 | k in KNN: large k → simpler model | **True** | Smoother boundary |
| P283 | Tree depth: deeper → more complex | **True** | More splits |
| P284 | NN layers: more → more complex | **True** | More parameters |
| P285 | Polynomial degree: higher → more complex | **True** | More flexible |
| P286 | Ridge λ: larger → simpler model | **True** | More regularization |
| P287 | SVM C: larger → more complex model | **True** | Less tolerance for errors |
| P288 | RBF γ: larger → more complex | **True** | Tighter Gaussians |
| P289 | Training error always decreases with complexity | **True** | |
| P290 | Test error always decreases with complexity | **False** | U-shaped curve! |
| P291 | Linear reg can model quadratic with feature eng? | **True** | Add $x^2$ feature |
| P292 | Correlation = causation | **False** | Famous fallacy |
| P293 | More trees in Random Forest always helps | **Diminishing** | Plateaus eventually |
| P294 | Random Forest uses all features at each split | **False** | Random subset √d |
| P295 | Gradient Boosting is sequential | **True** | Each tree corrects errors |
| P296 | Bagging trees are independent | **True** | Can parallelize |
| P297 | XGBoost uses regularization | **True** | L1 + L2 on leaf weights |
| P298 | One-hot creates d-1 or d columns? | **d (or d-1)** | d-1 avoids dummy variable trap |
| P299 | Multicollinearity affects coefficients | **True** | Unstable, high variance |
| P300 | VIF > 10 indicates multicollinearity | **True** | Variance Inflation Factor |

---

# 📝 Practice Set 4: GATE PYQ Style (P301 – P400)

**P301.** (GATE DA 2024 style, 2-mark) A linear regression model trained on 100 points gives $R^2 = 0.85$ on training. On 20 test points, $R^2 = 0.45$. What is the most likely issue?

**(a)** Underfitting **(b)** Overfitting **(c)** Perfect fit **(d)** Data is random

→ **Answer: (b) Overfitting** — High train, low test = classic overfit.

**P302.** (GATE, 1-mark) Which of the following is TRUE about PCA?
(a) PCA is supervised
(b) PCA maximizes between-class variance
(c) PCA finds directions of maximum variance
(d) PCA components may be correlated

→ **Answer: (c)** — PCA finds max variance directions. a=LDA, b=LDA, d=False (orthogonal).

**P303.** (GATE, 1-mark) SVM with $\mathbf{w} = [3, 4]$. What is the margin?
→ $\frac{2}{\sqrt{9+16}} = \frac{2}{5} = \mathbf{0.4}$

**P304.** (GATE, 2-mark) Training set: 100 samples, 90 class A, 10 class B. A model predicts all as A. What is the accuracy and F1 for class B?
→ Accuracy = 90/100 = 90%. For class B: TP=0, FP=0, FN=10. Precision=0/0 (undefined), Recall=0/10=0. **F1 = 0.**
⚠️ 90% accuracy but F1=0 for minority class!

**P305.** (GATE, 1-mark) Entropy of a fair coin?
→ $H = -2 \times 0.5 \log_2 0.5 = 1$ bit.

**P306.** (GATE, 2-mark) 3 clusters found by K-means. Cluster sizes: 50, 30, 20. Inertias: 100, 80, 60. Total inertia?
→ $100 + 80 + 60 = 240$

**P307.** (GATE, 1-mark) Which is NOT an advantage of Random Forest?
(a) Low variance (b) Handles missing values (c) Interpretable (d) Robust to outliers

→ **Answer: (c)** — Random Forest is not interpretable (black box).

**P308-P350** — GATE-format questions:

| # | Question | Answer |
|---|---|---|
| P308 | Perceptron convergence theorem requires? | Data be linearly separable |
| P309 | XOR requires minimum how many hidden neurons? | 2 |
| P310 | Sigmoid derivative maximum value? | 0.25 (at z=0) |
| P311 | Softmax with 5 classes, output size? | 5 |
| P312 | Batch GD with n=1000, batch=100, iterations per epoch? | 10 |
| P313 | K-means: if k=n then SSE=? | 0 |
| P314 | DBSCAN: point with < MinPts in ε-neighborhood is? | Noise (or border point) |
| P315 | Which reduces to K-means? GMM with equal spherical Gaussians + hard EM | K-means! |
| P316 | Silhouette of -0.5 means? | Likely in wrong cluster |
| P317 | Feature importance in Random Forest comes from? | Mean decrease in Gini/information gain |
| P318 | Out-of-bag (OOB) error in bagging? | Error on samples not in bootstrap sample (~37%) |
| P319 | Why ~37% out-of-bag? | $P(\text{not selected}) = (1 - 1/n)^n \approx 1/e \approx 0.368$ |
| P320 | AdaBoost base learner? | Decision stump (depth-1 tree) |
| P321 | Gradient Boosting fits each new tree to? | Residuals (negative gradient) |
| P322 | XGBoost regularizes by? | L1+L2 on leaf weights + tree complexity |
| P323 | Learning rate in boosting is called? | Shrinkage |
| P324 | Early stopping in boosting prevents? | Overfitting |
| P325 | Stacking uses what to combine? | A meta-learner trained on base predictions |
| P326 | Bias of MLE estimator for variance? | Biased (uses 1/n) |
| P327 | Consistent estimator means? | Converges to true value as n→∞ |
| P328 | MLE is consistent? | Yes |
| P329 | MAP vs MLE: with infinite data? | They converge (prior irrelevant) |
| P330 | Ridge closed form? | $(X^TX + \lambda I)^{-1}X^Ty$ |
| P331 | LASSO has closed form? | No (L1 not differentiable at 0) |
| P332 | How to solve LASSO? | Coordinate descent, subgradient methods |
| P333 | Cross-entropy is always ≥ entropy? | Yes: $H(P,Q) = H(P) + D_{KL}(P\|Q) \geq H(P)$ |
| P334 | Multi-class log regression uses? | Softmax function |
| P335 | One-vs-All: 5 classes → how many classifiers? | 5 |
| P336 | One-vs-One: 5 classes → how many classifiers? | $\binom{5}{2} = 10$ |
| P337 | Feature scaling: Min-Max maps to? | [0, 1] |
| P338 | Feature scaling: Z-score maps to? | Mean=0, Std=1 |
| P339 | Robust scaling uses? | Median and IQR (robust to outliers) |
| P340 | Missing Completely at Random (MCAR)? | Missingness unrelated to any data |
| P341 | Missing at Random (MAR)? | Missingness depends on observed data |
| P342 | Missing Not at Random (MNAR)? | Missingness depends on missing value itself |
| P343 | SMOTE generates? | Synthetic minority samples (interpolation) |
| P344 | Undersampling issue? | Loses information from majority class |
| P345 | Class weights in loss function? | Higher weight for minority → model pays more attention |
| P346 | Stratified split ensures? | Same class proportions in train/test |
| P347 | Data leakage from test set means? | Test info leaked into training → inflated performance |
| P348 | Apply scaling: fit on train, transform on? | Both train and test! (But fit ONLY on train!) |
| P349 | Polynomial regression degree 1 = ? | Linear regression |
| P350 | Polynomial regression degree too high → ? | Overfitting |

**P351-P400** — Numerical Answer Type (NAT):

| # | Question | Answer |
|---|---|---|
| P351 | Entropy of [1/4, 1/4, 1/4, 1/4]? | 2.0 bits |
| P352 | Entropy of [1/2, 1/4, 1/4]? | 1.5 bits |
| P353 | Gini of [0.5, 0.5]? | 0.5 |
| P354 | Gini of [0.9, 0.1]? | 0.18 |
| P355 | Gini of [1/3, 1/3, 1/3]? | 0.667 |
| P356 | $R^2$ if SSres=30, SStot=100? | 0.7 |
| P357 | MSE if errors = [1, -1, 2, -2]? | $(1+1+4+4)/4 = 2.5$ |
| P358 | MAE for above? | $(1+1+2+2)/4 = 1.5$ |
| P359 | Sigmoid(-1)? | $\approx 0.269$ |
| P360 | How many support vectors minimum for 2-class linearly separable? | 2 (one per class) |
| P361 | NN [5, 10, 3] total parameters? | $(5×10+10)+(10×3+3) = 93$ |
| P362 | NN [100, 50, 20, 10] total parameters? | $5050+1020+210 = 6280$ |
| P363 | 5-fold CV on 200 samples: test size per fold? | 40 |
| P364 | LOOCV on 50 samples: number of folds? | 50 |
| P365 | Cosine similarity of [1,0,0] and [0,1,0]? | 0 |
| P366 | Euclidean distance: (0,0,0) to (1,1,1)? | $\sqrt{3} \approx 1.732$ |
| P367 | Manhattan distance: (0,0,0) to (1,1,1)? | 3 |
| P368 | KNN k=5: neighbors are [+,+,−,+,−]. Prediction? | + (3 vs 2) |
| P369 | Min-Max scale: x=30, min=20, max=40? | 0.5 |
| P370 | Z-score: x=80, μ=70, σ=5? | 2.0 |
| P371 | $\sigma(z) \times (1 - \sigma(z))$ at z=0? | $0.5 \times 0.5 = 0.25$ |
| P372 | ReLU gradient at x=5? | 1 |
| P373 | ReLU gradient at x=-5? | 0 |
| P374 | How many trees in Random Forest with sqrt selection, d=100 features, selected per tree? | $\sqrt{100} = 10$ features per split |
| P375 | Margin if \|w\| = 4? | 2/4 = 0.5 |
| P376 | Margin if \|w\| = 0.5? | 2/0.5 = 4 |
| P377 | Silhouette: a=1, b=3? | $(3-1)/3 = 0.667$ |
| P378 | Silhouette: a=3, b=1? | $(1-3)/3 = -0.667$ |
| P379 | Information Gain: parent entropy=1.0, split entropy=0.7? | 0.3 |
| P380 | Gain Ratio: IG=0.3, SplitInfo=1.5? | 0.2 |
| P381 | Log-loss: y=1, ŷ=0.9? | $-\log(0.9) = 0.105$ |
| P382 | Log-loss: y=0, ŷ=0.1? | $-\log(0.9) = 0.105$ |
| P383 | Log-loss: y=1, ŷ=0.01? | $-\log(0.01) = 4.605$ (very high!) |
| P384 | Accuracy: 80 correct out of 100? | 80% |
| P385 | Recall: TP=45, FN=5? | 45/50 = 0.9 |
| P386 | Precision: TP=45, FP=15? | 45/60 = 0.75 |
| P387 | F1: P=0.75, R=0.9? | $2(0.75)(0.9)/(0.75+0.9) = 0.818$ |
| P388 | AUC of perfect classifier? | 1.0 |
| P389 | P(C1\|x) = 0.7, P(C2\|x) = 0.3. Classification? | C1 |
| P390 | Bias²+Variance+Noise = ? | Expected test error |
| P391 | High bias, low variance model? | Underfitting |
| P392 | K-means: 5 clusters, 2D. Centroid parameters? | 5×2 = 10 |
| P393 | GMM: 5 components, 2D. Parameters (mean+cov+weight)? | 5×2 + 5×3 + 4 = 29 |
| P394 | PCA: 50 features → 5 components. Reduction %? | 90% |
| P395 | LDA: 4 classes → max dimensions? | 3 (c-1) |
| P396 | Decision tree: 3 features, max depth 2, max splits? | 3 (root) + 6 (level 2?) = depends on branching |
| P397 | Bootstrap sample size from n=100? | 100 (with replacement) |
| P398 | % unique in bootstrap sample? | $\approx 63.2\%$ |
| P399 | OOB fraction? | $\approx 36.8\%$ |
| P400 | Elastic Net: α=0.5 means? | Equal L1 and L2 mixing |

---

# 📋 ML Algorithm Comparison Master Table

| Algorithm | Type | Parametric? | Scalability | Interpretable? | Feature Scaling? |
|---|---|---|---|---|---|
| Linear Regression | Supervised, Regression | Yes | O(d³) or O(nd) | High | Not required |
| Logistic Regression | Supervised, Classification | Yes | O(nd) | High | Required |
| SVM | Supervised, Both | Yes | O(n²d)–O(n³) | Medium | Required |
| KNN | Supervised, Both | No | O(nd) per query | Medium | Required |
| Naive Bayes | Supervised, Classification | Yes | O(nd) | High | Not required |
| Decision Tree | Supervised, Both | No | O(nd log n) | High | Not required |
| Random Forest | Supervised, Both | No | O(T × nd log n) | Low | Not required |
| Gradient Boosting | Supervised, Both | No | O(T × nd log n) | Low | Not required |
| PCA | Unsupervised | N/A | O(d²n + d³) | Medium | Required |
| K-Means | Unsupervised | N/A | O(nkdI) | Medium | Required |
| DBSCAN | Unsupervised | N/A | O(n log n) | Medium | Required |
| GMM/EM | Unsupervised | N/A | O(nkd²I) | Low | Required |
| Neural Network | Supervised, Both | Yes | Varies | Low | Required |

---

# 📋 Hyperparameter Tuning Quick Reference

| Algorithm | Key Hyperparameters | Low Value Effect | High Value Effect |
|---|---|---|---|
| **Ridge/LASSO** | λ (regularization) | Less regularization (may overfit) | More regularization (may underfit) |
| **SVM** | C | Wide margin (underfit) | Narrow margin (overfit) |
| **SVM** | γ (RBF kernel) | Smooth boundary (underfit) | Complex boundary (overfit) |
| **KNN** | k | Complex boundary (overfit) | Smooth boundary (underfit) |
| **Decision Tree** | max_depth | Simple tree (underfit) | Complex tree (overfit) |
| **Random Forest** | n_estimators | Fewer trees (higher variance) | More trees (diminishing returns) |
| **Random Forest** | max_features | Less randomness | More randomness |
| **Gradient Boost** | learning_rate | Slower, needs more trees | Faster, risk overshoot |
| **Gradient Boost** | n_estimators | Underfit | Overfit |
| **Neural Network** | learning_rate | Slow convergence | Divergence |
| **Neural Network** | layers/neurons | Underfit | Overfit |
| **K-Means** | k | Underspecify clusters | Overspecify clusters |
| **DBSCAN** | ε | More noise, fewer clusters | Fewer noise, larger clusters |
| **DBSCAN** | MinPts | More clusters | Fewer, denser clusters |

---

# 🏆 10 Golden Rules for GATE ML

> 📌 **Rule 1:** SVM margin = $\frac{2}{\|\mathbf{w}\|}$, NOT $\frac{1}{\|\mathbf{w}\|}$.

> 📌 **Rule 2:** PCA is unsupervised. LDA is supervised. Max LDA components = c-1.

> 📌 **Rule 3:** Ridge = L2 = shrinks weights. LASSO = L1 = zeros out weights (feature selection).

> 📌 **Rule 4:** K-means converges to LOCAL optimum (not global). Use K-means++ for better init.

> 📌 **Rule 5:** MLE of variance uses $\frac{1}{n}$ (biased). Unbiased uses $\frac{1}{n-1}$.

> 📌 **Rule 6:** Bagging reduces variance. Boosting reduces bias.

> 📌 **Rule 7:** LOOCV has LOW bias but HIGH variance!

> 📌 **Rule 8:** Accuracy is misleading for imbalanced data. Use F1, AUC, precision, recall.

> 📌 **Rule 9:** Decision boundary: Logistic=linear, DT=axis-parallel, SVM-RBF=non-linear, NN=any.

> 📌 **Rule 10:** Sigmoid(0)=0.5. Sigmoid'(0)=0.25. $\sigma(-z) = 1 - \sigma(z)$.

---

*🏆 End of Machine Learning — Comprehensive Notes for GATE 2027 (CS/DA)*

*📅 Last Updated: April 2026 | Covers: GATE DA Syllabus | 400 Practice Problems | 100% Exam Coverage*

---

# 📝 Practice Set 5: Algorithm Selection & Scenario-Based (P401 – P500)

**P401.** You have 1000 samples, 50 features, binary target. Which is best first try?
→ **Logistic Regression or Random Forest.** LogReg is simple baseline. RF for non-linearity.

**P402.** You have 1M samples, 10 features, continuous target. Algorithm?
→ **Gradient Boosting (XGBoost/LightGBM)** — handles scale, best regression performance.

**P403.** You have 100 samples, 5000 features (gene data). Algorithm?
→ **LASSO** (feature selection) or **SVM with linear kernel** (works well in high-D).

**P404.** Unstructured text classification. Algorithm?
→ **Naive Bayes** (fast, good for text) or **Logistic Regression with TF-IDF features**.

**P405.** Need interpretable model for medical diagnosis. Algorithm?
→ **Decision Tree** (fully interpretable) or **Logistic Regression** (coefficient interpretation).

**P406-P450** — "What Should You Track?" Decision Table:

| # | Scenario | Key Metric | Why |
|---|---|---|---|
| P406 | Cancer detection (don't miss positives) | **Recall** | FN = missed cancer = dangerous! |
| P407 | Spam filter (don't block good emails) | **Precision** | FP = lost legitimate email! |
| P408 | Balanced classification | **Accuracy / F1** | Both classes matter equally |
| P409 | Imbalanced binary (99% negative) | **F1, AUC-PR** | Accuracy misleading |
| P410 | Regression — house price prediction | **RMSE, MAE** | Continuous output |
| P411 | Regression — R² interpretation | **$R^2$** | Proportion of variance explained |
| P412 | Ranking (search results) | **NDCG, MAP** | Order matters |
| P413 | Multi-class balanced | **Macro F1** | Equal weight to all classes |
| P414 | Multi-class imbalanced | **Weighted F1** | Weight by class frequency |
| P415 | Clustering quality (no labels) | **Silhouette** | Internal evaluation |
| P416 | Clustering quality (with labels) | **ARI, NMI** | External evaluation |
| P417 | Cost-sensitive (FP costs $100, FN costs $10) | **Custom loss** | Weight by cost |
| P418 | Need probability calibration | **Brier Score, Reliability Diagram** | Well-calibrated probabilities |
| P419 | Compare two models | **Paired t-test on CV scores** | Statistical significance |
| P420 | Feature importance needed | **Random Forest, SHAP** | Explain model |

**P421-P450** — Bias-Variance Diagnostics:

| # | Symptom | Diagnosis | Fix |
|---|---|---|---|
| P421 | Training error: 2%, Test error: 20% | Overfitting (high variance) | More data, regularization, simpler model |
| P422 | Training error: 15%, Test error: 16% | Underfitting (high bias) | More complex model, more features |
| P423 | Training error: 15%, Test error: 30% | Both high bias AND high variance | Different algorithm entirely |
| P424 | Training error: 1%, Test error: 2% | Good fit! | Deploy |
| P425 | Large gap between train/test | High variance | Regularize, more data |
| P426 | Both errors high and close | High bias | Increase complexity |
| P427 | Adding more data helps? | If high variance: YES | If high bias: NO |
| P428 | Adding features helps? | If high bias: possibly | If high variance: NO (might hurt) |
| P429 | Increasing regularization? | Reduces variance, increases bias | Trade-off |
| P430 | Reducing model complexity? | Reduces variance, increases bias | Trade-off |
| P431 | Learning curve: train and test converge high | High bias | Need more complex model |
| P432 | Learning curve: large gap, doesn't close | High variance | Need more data or regularize |

**P433-P450** — Algorithm properties:

| # | Property | Algorithms with this property |
|---|---|---|
| P433 | Linear decision boundary | Logistic Reg, Linear SVM, Perceptron |
| P434 | Non-linear boundary (inherently) | Decision Tree, KNN, Neural Network |
| P435 | Probabilistic output | Logistic Reg, Naive Bayes, Softmax NN |
| P436 | No training phase | KNN (lazy learner) |
| P437 | Handles missing values natively | Decision Tree (some implementations) |
| P438 | Feature selection built-in | LASSO, Decision Tree (implicit), RF |
| P439 | Can model interactions | Decision Tree, NN, polynomial features |
| P440 | Works with small datasets | SVM, Naive Bayes |
| P441 | Needs large datasets | Deep Learning, KNN |
| P442 | Sensitive to outliers | Linear Reg, KNN, K-means |
| P443 | Robust to outliers | Decision Tree, Median-based methods |
| P444 | Can handle multi-class natively | Decision Tree, KNN, NB, NN, Softmax |
| P445 | Binary only (needs OvA/OvO) | SVM, basic Logistic Reg |
| P446 | Assumes feature independence | Naive Bayes |
| P447 | Assumes Gaussian features | Gaussian NB, LDA |
| P448 | Non-parametric | KNN, Decision Tree, Random Forest |
| P449 | Generative model | Naive Bayes, GMM |
| P450 | Discriminative model | Logistic Reg, SVM, NN |

---

# 📝 Practice Set 6: Ensemble Methods Deep Dive (P451 – P550)

### Bagging Worked Example

**P451.** Dataset: 8 samples. Bootstrap sample (with replacement) of size 8.
Possible sample: {1,1,3,4,4,5,7,8}. Out-of-bag: {2,6}.
→ OOB predictions used to estimate test error without a separate test set!

**P452.** Random Forest: 100 trees, 16 features. Features per split?
→ $\sqrt{16} = 4$ features randomly selected at each split.

**P453.** Random Forest: why decorrelate trees?
→ In bagging pure DTs, if one feature dominates, all trees look similar (correlated).
→ Random feature subset → different trees → lower variance when averaged!

### Boosting Worked Examples

**P454.** AdaBoost: 3 samples, initially $w = [1/3, 1/3, 1/3]$.
Stump misclassifies sample 2. Error $\epsilon_1 = 1/3$.
$\alpha_1 = \frac{1}{2}\ln\frac{1-\epsilon_1}{\epsilon_1} = \frac{1}{2}\ln\frac{2/3}{1/3} = \frac{1}{2}\ln 2 = 0.347$

Update weights: sample 2 weight increases, others decrease.
$w_2 \propto \frac{1}{3} \times e^{0.347} = \frac{1}{3} \times 1.414 = 0.471$
$w_{1,3} \propto \frac{1}{3} \times e^{-0.347} = \frac{1}{3} \times 0.707 = 0.236$

Normalize: total = 0.236 + 0.471 + 0.236 = 0.943.
$w' = [0.25, 0.50, 0.25]$ → Misclassified sample now has double weight!

**P455.** Gradient Boosting for regression: initial prediction = mean(y) = 5.
Residuals = [2, -1, 3, -2, 0]. Fit tree to residuals. Each new tree corrects errors.

**P456-P500** — Ensemble rapid drill:

| # | Question | Answer |
|---|---|---|
| P456 | Bagging base learner? | Typically deep decision tree |
| P457 | Why deep trees in bagging? | Low bias but high variance → bagging reduces variance |
| P458 | Boosting base learner? | Typically decision stump (shallow tree) |
| P459 | Why shallow in boosting? | High bias → boosting reduces bias |
| P460 | Can bagging use linear regression? | Yes, but won't help much (already low variance) |
| P461 | Random Forest OOB error purpose? | Free cross-validation estimate! |
| P462 | AdaBoost: final prediction? | Weighted majority vote: $H(x) = \text{sign}(\sum \alpha_t h_t(x))$ |
| P463 | Gradient Boosting: loss for regression? | MSE (or MAE, Huber) |
| P464 | Gradient Boosting: loss for classification? | Log-loss (cross-entropy) |
| P465 | XGBoost vs Gradient Boosting? | XGBoost adds regularization + parallelism |
| P466 | LightGBM advantage? | Histogram-based splits → faster |
| P467 | CatBoost advantage? | Handles categorical features natively |
| P468 | Stacking: level-0 models? | Base learners (diverse algorithms) |
| P469 | Stacking: level-1 model? | Meta-learner (often Logistic Reg or Linear Reg) |
| P470 | Why stacking works? | Combines strengths of diverse models |
| P471 | Voting: hard vs soft? | Hard: majority labels. Soft: average probabilities |
| P472 | Soft voting better when? | Models output well-calibrated probabilities |
| P473 | Boosting overfitting? | Yes, especially on noisy data |
| P474 | Bagging overfitting? | Rarely, reduced by averaging |
| P475 | AdaBoost: $\alpha_t$ when $\epsilon_t = 0$? | $\alpha_t \to \infty$ (perfect classifier!) |
| P476 | AdaBoost: $\alpha_t$ when $\epsilon_t = 0.5$? | $\alpha_t = 0$ (random, ignored!) |
| P477 | AdaBoost: $\epsilon_t > 0.5$? | Worse than random → negative weight (flip!) |
| P478 | Boosting: learning rate (shrinkage) effect? | Smaller → more trees needed but better generalization |
| P479 | Feature importance in RF? | Mean decrease in impurity across trees |
| P480 | Feature importance in GBM? | Gain-based or SHAP values |
| P481 | Number of trees in RF: too many risk? | Plateau (no harm, just slower) |
| P482 | Number of trees in boosting: too many risk? | Overfitting! |
| P483 | Why RF better than single tree? | Lower variance, similar bias |
| P484 | Why GBM better than single tree? | Lower bias (and variance with tuning) |
| P485 | Bootstrap = ? | Sampling with replacement, same size |
| P486 | Without replacement sampling is? | Subagging / subsampling |
| P487 | Variance reduction formula (bagging, ρ = avg correlation)? | $\frac{1 + \rho(B-1)}{B}\sigma^2$ |
| P488 | If trees fully uncorrelated (ρ=0)? | Variance = $\sigma^2/B$ (ideal!) |
| P489 | If trees fully correlated (ρ=1)? | Variance = $\sigma^2$ (bagging doesn't help!) |
| P490 | That's why RF decorrelates trees! | Feature subsampling reduces ρ |
| P491 | Gradient Boosting updates: $F_m(x) = F_{m-1}(x) + \alpha h_m(x)$? | Yes, additive model |
| P492 | $h_m$ fits what? | Negative gradient (pseudo-residuals) |
| P493 | For MSE loss, negative gradient = ? | $y_i - F_{m-1}(x_i)$ = residuals |
| P494 | For log-loss, negative gradient = ? | $y_i - p_i$ where $p_i = \sigma(F_{m-1}(x_i))$ |
| P495 | Early stopping: monitor what? | Validation loss |
| P496 | Patience in early stopping? | Number of epochs/rounds without improvement |
| P497 | RF can parallelize? | Yes (trees are independent) |
| P498 | GBM can parallelize? | Within each tree yes; across trees NO (sequential) |
| P499 | XGBoost column subsampling? | Yes (like RF feature subsampling) |
| P500 | XGBoost regularization term? | $\Omega(f) = \gamma T + \frac{1}{2}\lambda \sum w_j^2$ (T=leaves, w=weights) |

---

# 📝 Practice Set 7: Neural Network Deep Dive (P501 – P600)

### Forward & Backward Pass Worked Example

**P501.** 2-layer NN: input [0.5], hidden [2 neurons, sigmoid], output [1, sigmoid].

Weights: $w_1^{(1)} = 0.3, w_2^{(1)} = 0.7, b_1^{(1)} = 0.1, b_2^{(1)} = -0.2$
Output weights: $w_1^{(2)} = 0.5, w_2^{(2)} = -0.4, b^{(2)} = 0.3$

**Forward pass:**
- $z_1 = 0.3(0.5) + 0.1 = 0.25$ → $h_1 = \sigma(0.25) = 0.562$
- $z_2 = 0.7(0.5) - 0.2 = 0.15$ → $h_2 = \sigma(0.15) = 0.537$
- $z_{out} = 0.5(0.562) - 0.4(0.537) + 0.3 = 0.281 - 0.215 + 0.3 = 0.366$
- $\hat{y} = \sigma(0.366) = 0.590$

**P502.** If target $y = 1$, loss (binary CE)?
→ $L = -[1 \cdot \log(0.590) + 0 \cdot \log(0.410)] = -\log(0.590) = 0.528$

### Activation Function Deep Analysis

**P503-P520** — When to use each:

| # | Question | Answer |
|---|---|---|
| P503 | Default hidden layer activation? | **ReLU** |
| P504 | Binary classification output? | **Sigmoid** |
| P505 | Multi-class classification output? | **Softmax** |
| P506 | Regression output activation? | **None (linear)** |
| P507 | Regression output range [0,1]? | **Sigmoid** |
| P508 | Regression output range (-1,1)? | **Tanh** |
| P509 | Problem with sigmoid in deep networks? | Vanishing gradient |
| P510 | Problem with ReLU? | Dying ReLU (always 0 for negative inputs) |
| P511 | Fix for dying ReLU? | Leaky ReLU, PReLU, ELU |
| P512 | GELU used in? | Transformers (BERT, GPT) |
| P513 | Swish function: $x \cdot \sigma(x)$ advantage? | Smooth, non-monotonic |
| P514 | Sigmoid output range? | (0, 1) |
| P515 | Tanh output range? | (-1, 1) |
| P516 | Tanh vs sigmoid relation? | $\tanh(x) = 2\sigma(2x) - 1$ |
| P517 | Softmax guarantees? | Outputs sum to 1 |
| P518 | Softmax temperature T: high T? | More uniform distribution |
| P519 | Softmax temperature T: low T? | More peaked (approaches argmax) |
| P520 | Which activation is its own derivative? | None exactly, but $\sigma'=\sigma(1-\sigma)$ is clean |

### Weight Initialization

| Method | Distribution | When |
|---|---|---|
| **Xavier (Glorot)** | $\mathcal{N}(0, 2/(n_{in}+n_{out}))$ | Sigmoid/Tanh activations |
| **He** | $\mathcal{N}(0, 2/n_{in})$ | ReLU activations |
| **Zero** | All 0 | NEVER (symmetry breaking fails!) |
| **Random uniform** | $U(-1/\sqrt{n}, 1/\sqrt{n})$ | Simple baseline |

> ⚠️ **GATE Trap:** Never initialize all weights to zero! All neurons would compute same gradients → never learn different features.

### Optimizers Reference

| Optimizer | Update Rule | Key Feature |
|---|---|---|
| **SGD** | $w = w - \alpha \nabla L$ | Simple, baseline |
| **Momentum** | $v = \beta v + \nabla L; w = w - \alpha v$ | Accelerates convergence |
| **Nesterov** | Look-ahead gradient | Faster than momentum |
| **AdaGrad** | Per-parameter learning rate (decreases) | Good for sparse features |
| **RMSProp** | Moving average of squared gradients | Fixes AdaGrad decay |
| **Adam** | Momentum + RMSProp | Default choice! |
| **AdamW** | Adam + decoupled weight decay | Better generalization |

**P521-P600** — NN rapid drill:

| # | Question | Answer |
|---|---|---|
| P521 | Epoch vs iteration? | Epoch = full dataset pass. Iteration = 1 batch update |
| P522 | Batch size 1 = ? | SGD |
| P523 | Batch size = n = ? | Batch GD |
| P524 | Batch size between? | Mini-batch GD (most common: 32, 64, 128, 256) |
| P525 | Large batch size effect? | More stable gradients, less noise, may not generalize well |
| P526 | Small batch size effect? | Noisier gradients, regularization effect, better generalization |
| P527 | Dropout rate 0 means? | No dropout |
| P528 | Dropout rate 1 means? | Drop all neurons (model learns nothing!) |
| P529 | Typical dropout? | 0.2-0.5 |
| P530 | Batch normalization normalizes? | Each layer's inputs to zero mean, unit variance |
| P531 | BN parameters? | $\gamma$ (scale) and $\beta$ (shift) — learnable! |
| P532 | BN at test time uses? | Running mean/variance from training |
| P533 | L2 regularization on NN? | Weight decay: penalize large weights |
| P534 | Early stopping monitors? | Validation loss → stop when it increases |
| P535 | Data augmentation examples? | Rotation, flip, crop, noise, color jitter |
| P536 | Transfer learning? | Use pre-trained model, fine-tune on new task |
| P537 | Freezing layers? | Don't update weights of earlier layers |
| P538 | Fine-tuning? | Unfreeze and train with small learning rate |
| P539 | Gradient clipping? | Cap gradient magnitude → prevent explosion |
| P540 | Residual connections (skip connections)? | $y = F(x) + x$ → enables very deep nets |
| P541 | Why residual connections work? | Gradient flows directly through skip → no vanishing |
| P542 | Number of parameters in dense layer? | $n_{in} \times n_{out} + n_{out}$ |
| P543 | 1D convolution: input 10, kernel 3, stride 1, no padding? | Output = 10-3+1 = 8 |
| P544 | 2D conv: input 28×28, kernel 5×5, stride 1, no padding? | Output = 24×24 |
| P545 | Pooling 2×2, stride 2 on 28×28? | Output = 14×14 |
| P546 | Why pooling? | Reduce spatial dimensions, some translation invariance |
| P547 | CNN vs MLP for images? | CNN: parameter sharing, spatial structure |
| P548 | RNN processes? | Sequential data (text, time series) |
| P549 | LSTM vs vanilla RNN? | LSTM handles long-term dependencies (forget gate) |
| P550 | Transformer advantage? | Parallelizable, attention mechanism, handles long sequences |
| P551 | Attention mechanism? | Learn which parts of input to focus on |
| P552 | Self-attention in Transformer? | Each token attends to all other tokens |
| P553 | Multi-head attention? | Multiple parallel attention operations |
| P554 | Positional encoding? | Add position info (Transformers have no inherent order) |
| P555 | BERT is? | Bidirectional Encoder from Transformers (pre-trained) |
| P556 | GPT is? | Generative Pre-trained Transformer (autoregressive) |
| P557 | GAN components? | Generator + Discriminator |
| P558 | GAN objective? | Generator fools discriminator in minimax game |
| P559 | Autoencoder purpose? | Learn compressed representation (encoding) |
| P560 | Variational Autoencoder (VAE)? | Learns latent distribution, can generate new data |
| P561 | Convolution output size formula? | $\lfloor\frac{n + 2p - k}{s}\rfloor + 1$ |
| P562 | Padding "same" means? | Output = input size (add padding) |
| P563 | Padding "valid" means? | No padding (output < input) |
| P564 | $1 \times 1$ convolution purpose? | Channel-wise linear combination (reduce/expand channels) |
| P565 | Global Average Pooling? | Average each feature map → one value per channel |
| P566 | Batch size effect on GPU? | Larger = better GPU utilization (up to memory limit) |
| P567 | Learning rate warmup? | Start small, increase gradually, then decay |
| P568 | Cosine annealing? | Learning rate follows cosine curve |
| P569 | Step decay? | Reduce LR by factor every n epochs |
| P570 | Label smoothing? | Replace hard 0/1 with soft 0.05/0.95 → regularization |
| P571 | Mixup augmentation? | Blend two samples + their labels |
| P572 | Knowledge distillation? | Train small model to mimic large model |
| P573 | Model pruning? | Remove unimportant weights/neurons |
| P574 | Quantization? | Reduce precision (float32 → int8) |
| P575 | NN universal approximation for? | Continuous functions |
| P576 | NN not guaranteed to learn? | Optimization might fail (local minima, saddle points) |
| P577 | Saddle point? | Point where gradient=0 but not min or max |
| P578 | In high-D, most critical points are? | Saddle points (not local minima!) |
| P579 | Learning rate too high symptom? | Loss oscillates or diverges |
| P580 | Learning rate too low symptom? | Loss decreases very slowly |
| P581 | Catastrophic forgetting? | NN forgets old task when learning new one |
| P582 | Continual learning? | Strategies to avoid catastrophic forgetting |
| P583 | Gradient accumulation? | Sum gradients over micro-batches before update |
| P584 | Mixed precision training? | Use float16 for speed, float32 for stability |
| P585 | Overfitting signs in NN? | Train loss↓, val loss↑ after some epochs |
| P586 | Regularization techniques for NN? | Dropout, L2, early stopping, data augmentation, BN |
| P587 | Cross-entropy gradient for softmax output? | $\hat{y}_i - y_i$ (elegant simplification!) |
| P588 | Number of parameters: Conv2D 3×3, 64 input channels, 128 output? | $3 \times 3 \times 64 \times 128 + 128 = 73,856$ |
| P589 | Dense equivalent of above conv (28×28 input, 26×26 output)? | Much more (no weight sharing!) |
| P590 | NN depth vs width trade-off? | Deeper: more abstract features. Wider: more features per level |
| P591 | Skip/residual connection invented by? | He et al., ResNet (2015) |
| P592 | ImageNet record architecture? | Various: ResNet, EfficientNet, Vision Transformer |
| P593 | NLP breakthrough architecture? | Transformer (2017: "Attention Is All You Need") |
| P594 | Embedding layer? | Maps discrete tokens to dense vectors |
| P595 | Word2Vec is? | Word embedding model (Skip-gram or CBOW) |
| P596 | Cosine similarity for word vectors? | Measures semantic similarity |
| P597 | Loss landscape of NN? | Non-convex with many local minima and saddle points |
| P598 | Why deep learning works despite non-convexity? | Most local minima are "good enough" in high-D |
| P599 | Double descent phenomenon? | Test error decreases, then increases, then decreases again |
| P600 | Interpolation threshold? | Model complexity where it can memorize training data |

---

# 📝 Practice Set 8: Dimensionality Reduction & Clustering (P601 – P700)

### PCA Additional Worked Examples

**P601.** Given eigenvalues: $\lambda = [10, 5, 3, 1, 0.5, 0.3, 0.1, 0.1]$
Total variance = 20. Want 95% explained variance.
- 1 PC: 10/20 = 50%
- 2 PCs: 15/20 = 75%
- 3 PCs: 18/20 = 90%
- 4 PCs: 19/20 = **95%** ✅
→ **Answer: 4 principal components**

**P602.** PCA on standardized vs non-standardized data?
→ ALWAYS standardize first if features have different scales! Otherwise large-scale features dominate.

**P603.** Scree plot shows what?
→ Eigenvalues vs component number. Look for "elbow" → choose components before plateau.

### Clustering Advanced

**P604.** DBSCAN: $\epsilon = 1$, MinPts = 3. Points: (0,0), (0.5,0), (1,0), (3,3), (3.5,3), (10,10).
- (0,0): neighbors within ε=1 → (0.5,0), (1,0). Count=3 (include self) → **core point**
- (0.5,0): neighbors → (0,0), (1,0). Count=3 → **core point**
- (1,0): neighbors → (0,0), (0.5,0). Count=3 → **core point**
- (3,3): neighbors → (3.5,3). Count=2 < 3 → needs check
- (3.5,3): neighbors → (3,3). Count=2 < 3
- (3,3) and (3.5,3): less than MinPts → **border or noise**
- (10,10): no neighbors → **noise**

→ Cluster 1: {(0,0), (0.5,0), (1,0)}. Points (3,3), (3.5,3): border/noise. Point (10,10): noise.

**P605-P650** — Clustering questions:

| # | Question | Answer |
|---|---|---|
| P605 | K-means: k=3 on 2D data. How many centroid parameters? | 6 (3×2) |
| P606 | K-means: what if cluster becomes empty? | Reinitialize centroid randomly |
| P607 | K-means++: first centroid? | Random uniform from data |
| P608 | K-means++: subsequent centroids? | Probability proportional to distance² from nearest centroid |
| P609 | Mini-batch K-means advantage? | Faster for large datasets |
| P610 | K-medoids vs K-means? | K-medoids uses actual data points as centers (robust to outliers) |
| P611 | Agglomerative linkage: single? | Min distance between clusters |
| P612 | Agglomerative linkage: complete? | Max distance between clusters |
| P613 | Agglomerative linkage: average? | Mean pairwise distance |
| P614 | Agglomerative linkage: Ward's? | Minimize total within-cluster variance |
| P615 | Single linkage problem? | Chaining effect (long thin clusters) |
| P616 | Complete linkage tendency? | Compact, spherical clusters |
| P617 | Ward's linkage most similar to? | K-means (minimizes variance) |
| P618 | Dendrogram cut at height h gives? | Clusters where merge distance < h |
| P619 | DBSCAN: core point? | ≥ MinPts neighbors within ε |
| P620 | DBSCAN: border point? | < MinPts neighbors but near a core point |
| P621 | DBSCAN: noise point? | Neither core nor border |
| P622 | DBSCAN advantage over K-means? | Arbitrary shapes, handles noise, no k needed |
| P623 | DBSCAN disadvantage? | Sensitive to ε and MinPts, fails with varying density |
| P624 | GMM: soft assignment means? | Each point has probability of belonging to each cluster |
| P625 | EM algorithm: E-step? | Compute posterior P(cluster|point) using current params |
| P626 | EM algorithm: M-step? | Update params (mean, cov, weight) using posteriors |
| P627 | EM guaranteed convergence? | Log-likelihood monotonically increases → local optimum |
| P628 | BIC for model selection? | $BIC = -2\ln L + k\ln n$ (lower is better) |
| P629 | AIC for model selection? | $AIC = -2\ln L + 2k$ (lower is better) |
| P630 | BIC vs AIC? | BIC penalizes complexity more (stricter) |
| P631 | Spectral clustering uses? | Graph Laplacian eigenvalues |
| P632 | Spectral clustering advantage? | Can find non-convex clusters |
| P633 | Mean Shift clustering? | Finds modes of density (no k needed) |
| P634 | OPTICS extends? | DBSCAN to varying densities |
| P635 | Silhouette for k=1? | Undefined (need ≥ 2 clusters) |
| P636 | Calinski-Harabasz: higher or lower better? | Higher |
| P637 | Davies-Bouldin: higher or lower better? | Lower |
| P638 | Gap statistic method? | Compare within-cluster dispersion to null reference |
| P639 | K-means assuming which distance? | Euclidean |
| P640 | K-medoids can use? | Any distance metric |
| P641 | Fuzzy C-means? | Soft assignment (like GMM but simpler) |
| P642 | Cluster validity: internal vs external? | Internal: no labels. External: compare with ground truth |
| P643 | Rand Index range? | [0, 1] |
| P644 | Adjusted Rand Index range? | [-1, 1] (0 = random) |
| P645 | NMI range? | [0, 1] |
| P646 | Purity range? | [0, 1] |
| P647 | Purity limitation? | Always increases with more clusters (k=n → purity=1) |
| P648 | K-means objective function? | Minimize within-cluster sum of squares |
| P649 | K-means is equivalent to? | EM for GMM with equal, spherical, fixed-variance Gaussians |
| P650 | Choosing k: elbow + silhouette + gap statistic | Best practice: use multiple methods |

### Feature Selection & Engineering

**P651-P700** — Feature engineering drill:

| # | Question | Answer |
|---|---|---|
| P651 | Filter methods examples? | Correlation, chi-squared, mutual info, ANOVA |
| P652 | Wrapper methods examples? | Forward selection, backward elimination, RFE |
| P653 | Embedded methods examples? | LASSO, Ridge, tree-based importance |
| P654 | Filter advantage? | Fast, model-agnostic |
| P655 | Wrapper advantage? | Considers feature interactions |
| P656 | Wrapper disadvantage? | Slow (trains many models) |
| P657 | Forward selection? | Start empty, add best feature iteratively |
| P658 | Backward elimination? | Start with all, remove worst iteratively |
| P659 | RFE (Recursive Feature Elimination)? | Train model, remove least important, repeat |
| P660 | Variance threshold? | Remove features with variance < threshold |
| P661 | Correlation-based: remove features with $|r| > 0.9$? | One of the correlated pair |
| P662 | One-hot vs label encoding for trees? | Either works (trees handle both) |
| P663 | One-hot vs label encoding for linear models? | One-hot (label implies ordering) |
| P664 | Target encoding? | Replace category with mean of target |
| P665 | Target encoding leakage risk? | Can overfit to target → use with CV |
| P666 | Log transform when? | Right-skewed features |
| P667 | Box-Cox transform? | Generalized power transform for normality |
| P668 | Polynomial features: d features, degree 2? | $\binom{d+2}{2} = \frac{(d+1)(d+2)}{2}$ features |
| P669 | 5 features, degree 2 polynomial → how many? | $\frac{6 \times 7}{2} = 21$ features |
| P670 | Interaction features? | Products of pairs: $x_1 x_2, x_1 x_3, ...$ |
| P671 | Binning/discretization? | Convert continuous to categorical |
| P672 | Feature hashing? | Map features to fixed-size vector (large vocab) |
| P673 | TF-IDF? | Term Frequency × Inverse Document Frequency |
| P674 | TF-IDF penalizes? | Common words (high DF) |
| P675 | Bag of Words limitation? | Ignores word order |
| P676 | N-grams? | Consecutive word sequences (bigrams, trigrams) |
| P677 | Word embeddings advantage? | Captures semantic meaning, dense vectors |
| P678 | Imputation: mean? | Simple, reduces variance |
| P679 | Imputation: median? | Robust to outliers |
| P680 | Imputation: KNN? | Uses similar samples (better but slower) |
| P681 | Imputation: MICE? | Multiple Imputation by Chained Equations |
| P682 | Missing indicator feature? | Binary column: is_missing → captures info |
| P683 | Outlier detection: IQR method? | Outlier if $x < Q1 - 1.5 \times IQR$ or $x > Q3 + 1.5 \times IQR$ |
| P684 | Outlier detection: Z-score? | Outlier if $|z| > 3$ |
| P685 | Winsorization? | Cap extreme values at percentile |
| P686 | Feature engineering for dates? | Day of week, month, year, is_weekend, season |
| P687 | Feature engineering for addresses? | City, state, zip, latitude, longitude |
| P688 | Feature engineering for text? | Word count, sentiment, TF-IDF, embeddings |
| P689 | Handling class imbalance: oversampling? | SMOTE, random oversampling |
| P690 | Handling class imbalance: undersampling? | Random undersampling, Tomek links |
| P691 | Handling class imbalance: algorithmic? | Class weights, cost-sensitive learning |
| P692 | Cross-validation with SMOTE: correct order? | SMOTE inside CV fold (not before split!) |
| P693 | Scaling before or after split? | After! Fit on train only |
| P694 | Why not fit scaler on test? | Data leakage! |
| P695 | Pipeline purpose? | Chains preprocessing + model, prevents leakage |
| P696 | One-hot encoding: drop first? | Optional for linear models (avoid dummy trap) |
| P697 | Ordinal encoding when? | Ordered categories (low, medium, high) |
| P698 | Frequency encoding? | Replace category with frequency |
| P699 | Leave-one-out encoding? | Like target encoding with LOO to reduce overfitting |
| P700 | Feature stores? | Centralized repo of features for ML pipelines |

---

# 📝 Practice Set 9: GATE Exam Simulation (P701 – P800)

## Section A: 1-Mark Questions

**P701.** Ridge regression adds which penalty?
(a) $\lambda\sum|w_i|$ (b) $\lambda\sum w_i^2$ (c) $\lambda\sum w_i$ (d) $\lambda\sum w_i^3$
→ **Answer: (b)**

**P702.** K-means always converges to:
(a) Global optimum (b) Local optimum (c) Saddle point (d) Does not converge
→ **Answer: (b)**

**P703.** If 3 classes, LDA can produce at most how many discriminant axes?
→ **Answer: 2** (c-1 = 3-1)

**P704.** Naive Bayes is called "naive" because:
(a) It's simple (b) Assumes feature independence (c) Uses priors (d) None
→ **Answer: (b)**

**P705.** Which metric is best for imbalanced data?
(a) Accuracy (b) MSE (c) F1-Score (d) R²
→ **Answer: (c)**

**P706-P740** — 1-mark rapid:

| # | Question | Answer |
|---|---|---|
| P706 | PCA eigenvalues tell us? | Variance along each PC |
| P707 | Dropout used at test time? | No |
| P708 | LOOCV has higher bias or variance than 5-fold? | Higher variance, lower bias |
| P709 | Feature scaling needed for decision trees? | No |
| P710 | SVM margin formula? | $2/\|\mathbf{w}\|$ |
| P711 | Sigmoid(0) = ? | 0.5 |
| P712 | Entropy of 100% certain event? | 0 bits |
| P713 | Gini of pure node? | 0 |
| P714 | Perceptron can learn AND? | Yes (linearly separable) |
| P715 | Perceptron can learn XOR? | No |
| P716 | K-means converges if using? | Euclidean distance + mean update |
| P717 | Random Forest feature selection per split? | $\sqrt{d}$ (classification) or $d/3$ (regression) |
| P718 | AdaBoost weight of perfect classifier? | $\infty$ |
| P719 | Gradient Boosting fits trees to? | Residuals (negative gradient) |
| P720 | MAP with no prior = ? | MLE |
| P721 | $R^2$ can be negative? | Yes |
| P722 | Precision = 1 means? | No false positives |
| P723 | Recall = 1 means? | No false negatives |
| P724 | Type I error = ? | False Positive |
| P725 | Type II error = ? | False Negative |
| P726 | Bagging reduces? | Variance |
| P727 | Boosting reduces? | Bias |
| P728 | MLE of mean for Normal? | Sample mean |
| P729 | MLE of variance for Normal? | $\frac{1}{n}\sum(x_i-\bar{x})^2$ (biased!) |
| P730 | LASSO does feature selection because? | L1 penalty creates sparse solutions |
| P731 | Decision boundary of logistic regression? | Linear (hyperplane) |
| P732 | KNN time complexity (prediction)? | O(nd) per query |
| P733 | SGD advantage over batch GD? | Faster updates, can escape local minima |
| P734 | Multi-class: 10 classes, OvO classifiers? | $\binom{10}{2} = 45$ |
| P735 | Multi-class: 10 classes, OvA classifiers? | 10 |
| P736 | Silhouette score range? | [-1, 1] |
| P737 | DBSCAN can handle? | Arbitrary shapes + noise |
| P738 | EM algorithm monotonically increases? | Log-likelihood |
| P739 | Cross-entropy always ≥ entropy? | Yes |
| P740 | KL divergence symmetric? | No |

## Section B: 2-Mark Problems

**P741.** Confusion matrix:

|  | Pred A | Pred B | Pred C |
|---|---|---|---|
| True A | 40 | 5 | 5 |
| True B | 3 | 42 | 5 |
| True C | 2 | 3 | 45 |

Compute: (a) Overall accuracy (b) Per-class precision for A (c) Macro recall.

→ (a) Accuracy = (40+42+45)/150 = 127/150 = **84.7%**
(b) Precision(A) = 40/(40+3+2) = 40/45 = **0.889**
(c) Recall(A) = 40/50 = 0.8, R(B) = 42/50 = 0.84, R(C) = 45/50 = 0.9.
Macro Recall = (0.8+0.84+0.9)/3 = **0.847**

**P742.** K-means: 3 data points in 1D: x = {1, 5, 9}. k=2, initial centroids: $\mu_1=1, \mu_2=5$.

Iter 1: d(1,μ1)=0, d(1,μ2)=4 → C1. d(5,μ1)=4, d(5,μ2)=0 → C2. d(9,μ1)=8, d(9,μ2)=4 → C2.
Update: μ1=1, μ2=(5+9)/2=7.

Iter 2: d(1,1)=0, d(1,7)=6 → C1. d(5,1)=4, d(5,7)=2 → C2. d(9,1)=8, d(9,7)=2 → C2.
Same assignments → **converged! μ1=1, μ2=7**

**P743-P800** — 2-mark problems:

| # | Problem | Answer |
|---|---|---|
| P743 | Entropy: 6 true, 4 false out of 10? | $-0.6\log_2 0.6 - 0.4\log_2 0.4 = 0.971$ bits |
| P744 | Split: L=(5T,1F), R=(1T,3F). Weighted entropy? | $\frac{6}{10}(0.650) + \frac{4}{10}(0.811) = 0.714$ |
| P745 | IG for above? | 0.971 - 0.714 = **0.257** |
| P746 | SVM: w=[1,2,2], b=-9. Distance of (3,4,0) from boundary? | $\frac{|3+8+0-9|}{3} = \frac{2}{3} = 0.667$ |
| P747 | NN: [4, 3, 2, 1] parameters? | $(4×3+3)+(3×2+2)+(2×1+1) = 15+8+3 = 26$ |
| P748 | Linear reg: $\hat{y}=2x+1$. Residuals for (1,4),(2,5),(3,6)? | 4-3=1, 5-5=0, 6-7=-1. MSE = (1+0+1)/3 = 0.667 |
| P749 | Ridge: $(X^TX + \lambda I)^{-1}X^Ty$. If $\lambda \to 0$? | Normal equation (OLS) |
| P750 | Ridge: $\lambda \to \infty$? | All weights → 0 (intercept only) |
| P751 | Logistic reg: $P(y=1) = 0.73$. Log-odds? | $\ln\frac{0.73}{0.27} = \ln 2.704 = 0.995$ |
| P752 | Decision boundary: $w_1 x_1 + w_2 x_2 + b = 0$ with $w=[2,3], b=-6$. x₂-intercept? | $3x_2 = 6 \Rightarrow x_2 = 2$ |
| P753 | PCA: $\lambda = [8, 5, 2, 0.5]$. Explained by PC1? | 8/15.5 = **51.6%** |
| P754 | PCA: above, explained by first 2? | 13/15.5 = **83.9%** |
| P755 | K-means: SSE=100 with k=3. Add k=4. SSE will? | Decrease (more clusters → lower SSE) |
| P756 | But is k=4 better? | Not necessarily (elbow method, complexity trade-off) |
| P757 | Cosine sim: [1,1,0] and [1,0,1]? | $\frac{1}{\sqrt{2}\sqrt{2}} = 0.5$ |
| P758 | NB: P(C1)=0.7, P(x|C1)=0.3, P(C2)=0.3, P(x|C2)=0.8. P(C1|x)? | $\frac{0.21}{0.21+0.24} = \frac{0.21}{0.45} = 0.467$ → Classify C2! |
| P759 | Min-max: $x=[2,5,10,7,1]$. Scale $x=7$? | $(7-1)/(10-1) = 6/9 = 0.667$ |
| P760 | 5-fold CV: average accuracies = [85, 82, 88, 84, 86]%. Mean ± std? | Mean=85%, std≈2.0% |
| P761 | Bias² = 4, Variance = 3, Noise = 1. Expected error? | 4+3+1 = **8** |
| P762 | AUC=0.5. What does model achieve? | Random performance |
| P763 | AUC=0.92. Classification quality? | Very good (>0.9 excellent) |
| P764 | F1: P=0.6, R=0.9? | $\frac{2(0.54)}{1.5} = 0.72$ |
| P765 | Softmax([1,2,3]) approximately? | $[e^1, e^2, e^3]/\Sigma = [2.72, 7.39, 20.09]/30.2 = [0.09, 0.24, 0.67]$ |
| P766 | Cross-entropy: y=[0,0,1], ŷ=[0.1,0.2,0.7]? | $-\log(0.7) = 0.357$ |
| P767 | Cross-entropy: y=[0,0,1], ŷ=[0.1,0.2,0.7] vs ŷ=[0.01,0.01,0.98]? | Second has lower loss (more confident correct) |
| P768 | Gradient of sigmoid at z=1? | $\sigma(1)(1-\sigma(1)) = 0.731 \times 0.269 = 0.197$ |
| P769 | ReLU gradient at z=0? | Undefined (subgradient: typically 0) |
| P770 | Why ReLU preferred over sigmoid? | No vanishing gradient for positive inputs; faster |
| P771 | Regularization parameter too large? | Underfitting (model too constrained) |
| P772 | Decision tree: pure leaf entropy? | 0 |
| P773 | GainRatio fixes what? | Information Gain's bias toward many-valued features |
| P774 | Random Forest: why √d features? | Good trade-off: enough diversity + enough good features |
| P775 | Boosting learning rate 0.01: need more or fewer trees? | More trees (slower learning) |
| P776 | AdaBoost: sample weight after 3 misclassifications? | Much higher (exponentially increases) |
| P777 | DBSCAN time complexity? | O(n log n) with spatial index, O(n²) without |
| P778 | GMM with 1 component = ? | Single Gaussian (mean + covariance) |
| P779 | OOB error approximates? | Test error (no separate test set needed!) |
| P780 | Stacking: why use different base models? | Diversity → meta-learner can combine strengths |
| P781 | CNN: 32 filters, 3×3, 3 input channels. Parameters? | $3×3×3×32 + 32 = 896$ |
| P782 | Max pool 2×2 parameters? | 0 (no learnable parameters!) |
| P783 | Why BatchNorm after conv but before activation? | Normalize before non-linearity |
| P784 | ResNet: $y = F(x) + x$. Gradient of identity $x$? | 1 (flows directly → no vanishing!) |
| P785 | LSTM: forget gate range? | (0, 1) via sigmoid |
| P786 | Attention score: $\text{score}(Q, K) = QK^T / \sqrt{d_k}$. Why $\sqrt{d_k}$? | Prevents softmax saturation for large $d_k$ |
| P787 | Transformer has recurrence? | No (fully parallel!) |
| P788 | Training time: O(n²) for self-attention means? | Quadratic in sequence length |
| P789 | Two models: A has 90% accuracy, B has 92%. Is B significantly better? | Need statistical test (paired t-test on CV folds) |
| P790 | Occam's Razor in ML? | Prefer simpler model if performance is similar |
| P791 | No Free Lunch theorem says? | No single algorithm is best for all problems |
| P792 | Bias-variance decomposition: $E[(y - \hat{f})^2] = ?$ | $\text{Bias}^2 + \text{Variance} + \text{Irreducible noise}$ |
| P793 | Interpolation: model fits training perfectly. Good or bad? | Depends (double descent theory: can be good if very complex!) |
| P794 | Occam's Razor and regularization connection? | Regularization = mathematical form of Occam's Razor |
| P795 | Grid search: 3 hyperparams, 5 values each, 5-fold CV. Total fits? | $5^3 × 5 = 625$ |
| P796 | Random search advantage? | More efficient than grid in high dimensions |
| P797 | Bayesian optimization? | Builds surrogate model of hyperparameter space |
| P798 | AutoML goal? | Automate model selection and hyperparameter tuning |
| P799 | Cross-validation should be done? | Before test set evaluation (never peek at test!) |
| P800 | Final model evaluation on? | Held-out test set (used only once!) |

---

# 📝 Practice Set 10: Comprehensive Final Review (P801 – P900)

## ML Formulas Quick Card

| Formula | Equation |
|---|---|
| **MSE** | $\frac{1}{n}\sum(y_i - \hat{y}_i)^2$ |
| **MAE** | $\frac{1}{n}\sum|y_i - \hat{y}_i|$ |
| **RMSE** | $\sqrt{MSE}$ |
| **$R^2$** | $1 - \frac{SS_{res}}{SS_{tot}}$ |
| **Adjusted $R^2$** | $1 - \frac{(1-R^2)(n-1)}{n-d-1}$ |
| **$\beta_1$ (SLR)** | $\frac{\sum(x_i-\bar{x})(y_i-\bar{y})}{\sum(x_i-\bar{x})^2}$ |
| **Normal equation** | $\theta = (X^TX)^{-1}X^Ty$ |
| **Ridge** | $\theta = (X^TX + \lambda I)^{-1}X^Ty$ |
| **Sigmoid** | $\sigma(z) = \frac{1}{1+e^{-z}}$ |
| **Cross-entropy (binary)** | $-[y\log\hat{y} + (1-y)\log(1-\hat{y})]$ |
| **Softmax** | $\frac{e^{z_i}}{\sum_j e^{z_j}}$ |
| **Entropy** | $-\sum p_i \log_2 p_i$ |
| **Gini** | $1 - \sum p_i^2$ |
| **Information Gain** | $H(parent) - H(children)$ |
| **SVM margin** | $\frac{2}{\|\mathbf{w}\|}$ |
| **Precision** | $\frac{TP}{TP+FP}$ |
| **Recall** | $\frac{TP}{TP+FN}$ |
| **F1** | $\frac{2PR}{P+R}$ |
| **Specificity** | $\frac{TN}{TN+FP}$ |
| **Euclidean distance** | $\sqrt{\sum(x_i-y_i)^2}$ |
| **Cosine similarity** | $\frac{x \cdot y}{\|x\|\|y\|}$ |
| **Silhouette** | $\frac{b-a}{\max(a,b)}$ |
| **MLE (Normal mean)** | $\hat{\mu} = \bar{x}$ |
| **MLE (Normal var)** | $\hat{\sigma}^2 = \frac{1}{n}\sum(x_i-\bar{x})^2$ |
| **Bayes' theorem** | $P(A|B) = \frac{P(B|A)P(A)}{P(B)}$ |
| **KL divergence** | $\sum P(x)\log\frac{P(x)}{Q(x)}$ |
| **Bias-Variance** | $E[error] = bias^2 + variance + noise$ |
| **Conv output size** | $\lfloor\frac{n+2p-k}{s}\rfloor + 1$ |
| **NN params (dense)** | $n_{in} \times n_{out} + n_{out}$ |

## 7-Day GATE ML Revision Plan

| Day | Topic | Focus |
|---|---|---|
| 1 | Linear & Logistic Regression | Formulas, $R^2$, gradient descent, sigmoid |
| 2 | SVM, KNN, Naive Bayes | Margin, kernel trick, Bayes calculations |
| 3 | Decision Trees, Entropy, Gini | Worked examples, ID3/C4.5/CART |
| 4 | Ensemble Methods | Bagging vs Boosting, RF, AdaBoost, GBM |
| 5 | PCA, LDA, Clustering | Eigenvalue problems, K-means traces, DBSCAN |
| 6 | Neural Networks | Forward/backward pass, activations, architectures |
| 7 | Evaluation & Integration | Confusion matrix, F1, AUC, bias-variance, CV |

## Final Self-Assessment (P801-P900)

| # | Can You...? | Difficulty |
|---|---|---|
| P801 | Compute $\beta_0, \beta_1$ for simple linear regression? | ⭐⭐ |
| P802 | Calculate $R^2$ from residuals? | ⭐⭐ |
| P803 | Perform one step of gradient descent? | ⭐⭐ |
| P804 | Compute sigmoid for any z? | ⭐ |
| P805 | Calculate binary cross-entropy loss? | ⭐⭐ |
| P806 | Compute SVM margin from weight vector? | ⭐⭐ |
| P807 | Classify a point using SVM decision boundary? | ⭐⭐ |
| P808 | Apply Bayes theorem for NB classification? | ⭐⭐⭐ |
| P809 | Calculate entropy, Gini, information gain? | ⭐⭐⭐ |
| P810 | Build a decision tree for small dataset? | ⭐⭐⭐ |
| P811 | Trace K-means iterations? | ⭐⭐ |
| P812 | Compute PCA step by step? | ⭐⭐⭐ |
| P813 | Calculate confusion matrix metrics? | ⭐⭐ |
| P814 | Know when accuracy is misleading? | ⭐⭐ |
| P815 | Identify overfitting vs underfitting? | ⭐⭐ |
| P816 | Choose right algorithm for scenario? | ⭐⭐⭐ |
| P817 | Count NN parameters? | ⭐⭐ |
| P818 | Trace forward pass of simple NN? | ⭐⭐⭐ |
| P819 | Compare bagging vs boosting? | ⭐⭐ |
| P820 | Explain curse of dimensionality? | ⭐⭐ |
| P821 | Calculate cosine similarity? | ⭐⭐ |
| P822 | Compute silhouette coefficient? | ⭐⭐ |
| P823 | Know DBSCAN vs K-means trade-offs? | ⭐⭐ |
| P824 | Explain MLE vs MAP? | ⭐⭐⭐ |
| P825 | Know when to use which metric? | ⭐⭐⭐ |
| P826 | Identify bias-variance from learning curve? | ⭐⭐ |
| P827 | Know effect of hyperparameters? | ⭐⭐ |
| P828 | Apply cross-validation correctly? | ⭐⭐ |
| P829 | Know feature scaling requirements? | ⭐ |
| P830 | Handle imbalanced data? | ⭐⭐ |

## GATE ML Common Traps Summary

| Trap | Correct Answer |
|---|---|
| SVM margin = 1/\|\|w\|\| | **2/\|\|w\|\|** |
| PCA is supervised | **Unsupervised** |
| KNN has training phase | **No training (lazy!)** |
| Accuracy is best for imbalanced | **F1 or AUC** |
| MLE variance uses 1/(n-1) | **1/n (biased!)** |
| LOOCV has low variance | **High variance!** |
| Bagging reduces bias | **Reduces variance** |
| LASSO shrinks to near-zero | **Exactly zero** |
| Decision trees need scaling | **No scaling needed** |
| $R^2 < 0$ is impossible | **Possible (worse than mean prediction)** |
| More features always helps | **Curse of dimensionality** |
| K-means finds global optimum | **Local optimum only** |
| Gradient descent always finds minimum | **Only for convex problems** |
| Perceptron solves any classification | **Only linearly separable** |
| Naive Bayes assumes feature = independent | **Conditional independence** |

P831-P900: Review all practice sets above. Focus on problems you got wrong. Redo worked examples without looking at solutions.

---

# 🏅 Final Motivational Notes

> 📌 "ML in GATE DA is 8-12 marks. Master the formulas + common traps = full marks in ML section."

> 🎯 "Every worked example above mirrors actual GATE question patterns. Practice until traces are automatic."

> 🧠 "The exam tests understanding, not memorization. If you can explain WHY each formula works, you'll ace it."

---

---

# 📘 Section 28: Regularization Paths — Complete Analysis

## Ridge Regularization Path

As $\lambda$ increases from 0 → ∞:

| $\lambda$ | Weights | Bias | Model Complexity |
|---|---|---|---|
| 0 | OLS solution | High variance | Full |
| 0.01 | Slightly shrunk | Some reduction | Near-full |
| 0.1 | Moderately shrunk | Moderate | Medium |
| 1 | Significantly shrunk | Low variance | Reduced |
| 10 | Very small | High bias | Very low |
| 100 | Near zero | Underfitting | Minimal |
| ∞ | All zero | Only intercept | None |

> ⚠️ **Key Insight:** Ridge NEVER makes weights exactly zero → no feature selection!

## LASSO Regularization Path

| $\lambda$ | Effect | Features |
|---|---|---|
| 0 | OLS (if $p < n$) | All features active |
| 0.001 | Mild shrinkage | Most features active |
| 0.01 | Some features → 0 | Feature selection begins |
| 0.1 | Many features → 0 | Sparse model |
| 1 | Most features → 0 | Very sparse |
| 10 | Only strongest remain | 1-2 features |
| ∞ | All zero | Intercept only |

> 💡 **Why LASSO selects:** L1 has "corners" in constraint region → solution hits axes (zero coordinates).

## Elastic Net ($\alpha$ = L1 ratio)

| $\alpha$ | Behavior |
|---|---|
| 0 | Pure Ridge (L2 only) |
| 0.5 | Balanced L1 + L2 |
| 1 | Pure LASSO (L1 only) |

> 🎯 **GATE Trick:** Elastic Net groups correlated features (selects together), LASSO picks one randomly.

### Regularization Path Visualization (ASCII)

```
Weight Value
  ^
  |  w₁ ━━━━━━━━━━━━━╲___________  → Ridge: smooth decay
  |  w₂ ━━━━━━━━╲________________  → Ridge: all approach 0
  |  w₃ ━━━━╲____________________  → Ridge: never reach 0
  |
  |  w₁ ━━━━━━━━━━━━━╲____| ZERO  → LASSO: hits 0 at λ₁
  |  w₂ ━━━━━━━╲_________| ZERO   → LASSO: hits 0 at λ₂
  |  w₃ ━━━━╲____________| ZERO   → LASSO: hits 0 at λ₃
  └──────────────────────────────→ λ
```

---

# 📘 Section 29: Learning Curves — Comprehensive Guide

## Definition

Learning curve: plot of **training and validation performance** as a function of **training set size** (or number of iterations).

## Diagnosing from Learning Curves

### High Bias (Underfitting)

```
Error
  ^
  |  Train error ——————————————————— (high, flat)
  |  Val error   ——————————————————— (high, flat, close to train)
  |
  └────────────────────────────→ Training set size
```
- Both errors converge to a HIGH value
- Gap between curves: SMALL
- More data WON'T help
- **Fix:** More complex model, more features

### High Variance (Overfitting)

```
Error
  ^
  |  Val error   ——————→ ————→ ———→  (high, slowly decreasing)
  |                     ↗
  |  Train error —————→ ——→ ——→ ——→  (low, slowly increasing)
  |
  └────────────────────────────→ Training set size
```
- Large GAP between train and validation
- Training error very low
- More data WILL help (gap closes)
- **Fix:** Regularization, simpler model, more data

### Good Fit

```
Error
  ^
  |  Val error   ——→——→——→ (decreasing, close to train)
  |  Train error ——→——→——→ (low, stable)
  |
  └────────────────────────────→ Training set size
```
- Both errors low
- Gap is small
- Converge at acceptable error level

## Learning Curve Decision Table

| Observation | Diagnosis | Action |
|---|---|---|
| Both errors high, close | High bias | Increase complexity |
| Large gap, train is low | High variance | Regularize, more data |
| Adding data helps | High variance | Get more data |
| Adding data doesn't help | High bias | Change model |
| Train increases, val decreases | Normal early behavior | Continue training |
| Val increases | Overfitting starting | Early stopping |

---

# 📘 Section 30: ML ↔ Mathematics Connections

## How Linear Algebra Powers ML

| LA Concept | ML Application |
|---|---|
| Matrix multiplication | Forward pass in NN, feature transformation |
| Transpose | Weight matrices, gradient computation |
| Inverse | Normal equation $(X^TX)^{-1}X^Ty$ |
| Eigenvalues/vectors | PCA, spectral clustering, PageRank |
| SVD | Dimensionality reduction, recommendation |
| Positive definite | Covariance matrices (always PSD) |
| Rank | Feature independence (rank < d → multicollinearity) |
| Norm | Regularization (L1, L2), SVM margin |
| Trace | Total variance in PCA |
| Determinant | Volume, singularity check |
| Orthogonality | PCA axes are orthogonal |
| Projection | PCA projects onto eigenvectors |
| Gradient | Direction of steepest ascent/descent |
| Hessian | Second-order optimization, Newton's method |

## How Probability Powers ML

| Probability Concept | ML Application |
|---|---|
| Bayes' theorem | Naive Bayes, MAP estimation |
| Conditional probability | Classification P(class|features) |
| Prior probability | Prior belief (regularization = prior!) |
| Likelihood | MLE, generative models |
| Joint distribution | GMM, probabilistic models |
| Independence | Naive Bayes assumption |
| Expected value | Loss function = expected loss |
| Variance | Model uncertainty, bias-variance |
| Bernoulli | Binary classification, Bernoulli NB |
| Multinomial | Multi-class, multinomial NB |
| Normal/Gaussian | GMM, LDA, many assumptions |
| Poisson | Count data models |

## How Calculus Powers ML

| Calculus Concept | ML Application |
|---|---|
| Derivative | Gradient descent, backpropagation |
| Chain rule | Backpropagation (THE key insight!) |
| Partial derivatives | Multi-variable optimization |
| Gradient | Direction of update in parameter space |
| Convexity | Guarantees of global optimum |
| Optimization | Training = loss minimization |
| Taylor series | Understanding learning rate, approximations |
| Integral | Expected values, probability densities |
| Lagrange multipliers | SVM dual formulation, constrained optimization |

## How Statistics Powers ML

| Stats Concept | ML Application |
|---|---|
| Mean, variance | Feature statistics, normalization |
| Covariance | PCA, multivariate analysis |
| Correlation | Feature selection, multicollinearity |
| Hypothesis testing | Model comparison, A/B testing |
| Confidence interval | Prediction uncertainty |
| Central Limit Theorem | Why ensembles work, large sample theory |
| Law of Large Numbers | Training converges, consistency |
| Sampling | Bootstrap, cross-validation |
| Type I/II errors | FP, FN in classification |
| Maximum Likelihood | Training criterion |
| Sufficient statistics | Efficient parameter estimation |

---

# 📘 Section 31: Model Selection Decision Flowchart

## When to Use What — Decision Guide

```
START → How many labeled samples?
│
├── < 100 → Simple models
│   ├── Classification? → Logistic Regression or SVM (linear)
│   └── Regression? → Linear Regression or Ridge
│
├── 100 - 10K → Medium models
│   ├── Need interpretability?
│   │   ├── Yes → Decision Tree, Logistic Reg
│   │   └── No → Random Forest, SVM (RBF), Gradient Boosting
│   ├── High-dimensional (features > samples)?
│   │   ├── Yes → LASSO, Linear SVM, PCA first
│   │   └── No → Any of the above
│   └── Text data? → Naive Bayes, Logistic Reg with TF-IDF
│
├── 10K - 100K → Complex models
│   ├── Tabular? → XGBoost, LightGBM (KING of tabular!)
│   ├── Image? → CNN
│   ├── Text? → Transformers (BERT)
│   └── Sequence? → LSTM, Transformer
│
└── > 100K → Deep learning shines
    ├── Tabular? → Gradient Boosting STILL competitive!
    ├── Image? → CNN (ResNet, EfficientNet)
    ├── Text? → Transformers
    └── Audio? → CNN + RNN, Transformers
```

## Quick Algorithm Selector

| Data Type | First Try | Second Try | Advanced |
|---|---|---|---|
| Small tabular | LogReg / Ridge | SVM | Ensemble |
| Large tabular | XGBoost | LightGBM | CatBoost |
| Images | CNN | Transfer Learning | EfficientNet |
| Text | NB + TF-IDF | LogReg + TF-IDF | BERT |
| Time series | ARIMA | LSTM | Transformer |
| Anomaly | Isolation Forest | DBSCAN | Autoencoder |
| Clustering | K-means | DBSCAN | GMM |
| Rec systems | Collaborative filtering | Matrix factorization | Deep learning |

---

# 📘 Section 32: Complete Sklearn API Reference for GATE

## Classification

| Algorithm | Sklearn Class | Key Parameters |
|---|---|---|
| Logistic Reg | `LogisticRegression` | `C, penalty, solver` |
| SVM | `SVC, LinearSVC` | `C, kernel, gamma` |
| Decision Tree | `DecisionTreeClassifier` | `max_depth, criterion` |
| Random Forest | `RandomForestClassifier` | `n_estimators, max_features` |
| Gradient Boosting | `GradientBoostingClassifier` | `n_estimators, learning_rate, max_depth` |
| KNN | `KNeighborsClassifier` | `n_neighbors, metric` |
| Naive Bayes | `GaussianNB, MultinomialNB` | `var_smoothing, alpha` |
| AdaBoost | `AdaBoostClassifier` | `n_estimators, learning_rate` |
| XGBoost | `xgb.XGBClassifier` | `n_estimators, learning_rate, reg_lambda` |

## Regression

| Algorithm | Sklearn Class | Key Parameters |
|---|---|---|
| Linear Reg | `LinearRegression` | None (no regularization) |
| Ridge | `Ridge` | `alpha` |
| LASSO | `Lasso` | `alpha` |
| Elastic Net | `ElasticNet` | `alpha, l1_ratio` |
| SVR | `SVR` | `C, kernel, epsilon` |
| Decision Tree | `DecisionTreeRegressor` | `max_depth` |
| Random Forest | `RandomForestRegressor` | `n_estimators` |
| Gradient Boosting | `GradientBoostingRegressor` | `n_estimators, learning_rate` |

## Clustering

| Algorithm | Sklearn Class | Key Parameters |
|---|---|---|
| K-Means | `KMeans` | `n_clusters, init, n_init` |
| DBSCAN | `DBSCAN` | `eps, min_samples` |
| GMM | `GaussianMixture` | `n_components, covariance_type` |
| Agglomerative | `AgglomerativeClustering` | `n_clusters, linkage` |
| Mean Shift | `MeanShift` | `bandwidth` |
| Spectral | `SpectralClustering` | `n_clusters` |

## Dimensionality Reduction

| Algorithm | Sklearn Class | Key Parameters |
|---|---|---|
| PCA | `PCA` | `n_components` |
| LDA | `LinearDiscriminantAnalysis` | `n_components` |
| t-SNE | `TSNE` | `perplexity, n_components` |
| UMAP | `umap.UMAP` | `n_neighbors, min_dist` |

## Preprocessing

| Transform | Sklearn Class | When |
|---|---|---|
| Standard scaling | `StandardScaler` | SVM, KNN, NN, PCA |
| Min-max | `MinMaxScaler` | When need [0,1] range |
| Robust | `RobustScaler` | When outliers present |
| One-hot | `OneHotEncoder` | Nominal categories |
| Label | `LabelEncoder` | Target variable |
| Ordinal | `OrdinalEncoder` | Ordered categories |
| Imputation | `SimpleImputer` | Missing values |
| Polynomial | `PolynomialFeatures` | Non-linear features |

## Model Selection

| Tool | Sklearn Function | Purpose |
|---|---|---|
| Cross-validation | `cross_val_score` | Estimate performance |
| Grid search | `GridSearchCV` | Exhaustive hyperparameter tuning |
| Random search | `RandomizedSearchCV` | Efficient tuning |
| Train-test split | `train_test_split` | Basic split |
| k-Fold | `KFold, StratifiedKFold` | CV splitting |

## Metrics

| Metric | Sklearn Function | Task |
|---|---|---|
| Accuracy | `accuracy_score` | Classification |
| Precision/Recall/F1 | `precision_score, recall_score, f1_score` | Classification |
| Confusion matrix | `confusion_matrix` | Classification |
| AUC-ROC | `roc_auc_score` | Classification |
| MSE | `mean_squared_error` | Regression |
| MAE | `mean_absolute_error` | Regression |
| $R^2$ | `r2_score` | Regression |
| Silhouette | `silhouette_score` | Clustering |
| ARI | `adjusted_rand_score` | Clustering |

---

# 📘 Section 33: Curse of Dimensionality — Deep Dive

## What Happens in High Dimensions

### Volume Explosion

The volume of a unit hypercube (side length 1) is always 1. But the volume of the **inscribed hypersphere** shrinks to zero!

| Dimension d | Hypersphere Volume (radius 0.5) |
|---|---|
| 1 | 1.000 |
| 2 | 0.785 (circle) |
| 3 | 0.524 |
| 5 | 0.164 |
| 10 | 0.00249 |
| 20 | 0.0000000258 |
| 100 | ≈ 0 |

> 🤯 Almost all data in high-D cube is in the "corners" — far from center!

### Distance Concentration

In high dimensions, all pairwise distances become nearly equal!

$$\frac{d_{max} - d_{min}}{d_{min}} \to 0 \text{ as } d \to \infty$$

> ⚠️ **Impact on KNN:** If all distances are similar, nearest neighbor is meaningless!

### Required Samples

To maintain same density of data:
- 1D: 10 samples
- 2D: 100 samples (10²)
- 3D: 1000 samples (10³)
- d-D: $10^d$ samples!

**Rule of thumb:** Need at least 5-10× samples per feature for reliable learning.

### How Algorithms Cope

| Algorithm | Curse Effect | Coping Strategy |
|---|---|---|
| KNN | All distances equal → bad | Feature selection, PCA first |
| Decision Tree | Splits on irrelevant features | Built-in feature selection |
| Linear Reg | Overfitting with many features | Regularization (Ridge/LASSO) |
| SVM | Actually works in high-D! | Kernel trick, margin |
| Neural Network | Overfitting | Dropout, regularization |
| Naive Bayes | Feature independence helps! | Low parameters |

---

# 📘 Section 34: Advanced Topics — PAC Learning & VC Dimension

## PAC Learning (Probably Approximately Correct)

A concept class C is **PAC learnable** if there exists a learning algorithm that, for any:
- $\epsilon > 0$ (accuracy parameter)
- $\delta > 0$ (confidence parameter)

produces a hypothesis h with:
$$P[\text{error}(h) \leq \epsilon] \geq 1 - \delta$$

using polynomially many samples: $m \geq \frac{1}{\epsilon}\left(\ln|H| + \ln\frac{1}{\delta}\right)$

> 🎯 **GATE Insight:** "Probably" = with probability $1-\delta$. "Approximately" = within $\epsilon$ of true error.

## VC Dimension

The VC (Vapnik-Chervonenkis) dimension of H = **maximum number of points that can be shattered by H**.

"Shattered" = for all possible labelings of those points, H has a hypothesis that correctly classifies them.

### VC Dimensions of Common Classifiers

| Classifier | VC Dimension |
|---|---|
| Linear classifier in $\mathbb{R}^d$ | $d + 1$ |
| Linear classifier in $\mathbb{R}^2$ | 3 (can shatter 3 points, not 4) |
| Perceptron in $\mathbb{R}^d$ | $d + 1$ |
| 1-NN | $\infty$ (can memorize anything!) |
| Finite hypothesis space |H| | $\leq \log_2|H|$ |
| Decision stump | 2 |
| Polynomial of degree k in 1D | $k + 1$ |
| Neural networks | Roughly O(# parameters) |

### Sample Complexity Bound

Using VC dimension $d_{VC}$:
$$m \geq \frac{1}{\epsilon}\left(4\log_2\frac{2}{\delta} + 8d_{VC}\log_2\frac{13}{\epsilon}\right)$$

> 💡 Higher VC dimension → need more samples → more risk of overfitting.

## Structural Risk Minimization (SRM)

Balance between:
1. **Empirical risk** (training error)
2. **Model complexity** (VC dimension)

$$R(h) \leq R_{emp}(h) + \sqrt{\frac{d_{VC}(\log(2n/d_{VC}) + 1) - \log(\delta/4)}{n}}$$

> 🎯 **Connection:** SRM ≈ regularization (adding complexity penalty = reducing effective VC dimension).

---

# 📘 Section 35: Complete Gradient Descent Variants

## Comparison Table

| Variant | Update | Batch | Memory |
|---|---|---|---|
| Batch GD | $w = w - \alpha \nabla L_{all}$ | All data | High |
| SGD | $w = w - \alpha \nabla L_i$ | 1 sample | Low |
| Mini-batch | $w = w - \alpha \nabla L_{batch}$ | b samples | Medium |
| Momentum | $v = \beta v + \nabla L; w = w - \alpha v$ | Any | $v$ per param |
| Nesterov | $v = \beta v + \nabla L(w - \beta v)$ | Any | $v$ per param |
| AdaGrad | $w = w - \frac{\alpha}{\sqrt{G+\epsilon}} \nabla L$ | Any | $G$ per param |
| RMSProp | $G = \gamma G + (1-\gamma)(\nabla L)^2$ | Any | $G$ per param |
| Adam | $m, v$ estimates + bias correction | Any | $m, v$ per param |
| AdamW | Adam + decoupled weight decay | Any | $m, v$ per param |

### Adam Detailed

**Equations:**
$$m_t = \beta_1 m_{t-1} + (1-\beta_1) g_t$$
$$v_t = \beta_2 v_{t-1} + (1-\beta_2) g_t^2$$
$$\hat{m}_t = \frac{m_t}{1-\beta_1^t}, \quad \hat{v}_t = \frac{v_t}{1-\beta_2^t}$$
$$w_t = w_{t-1} - \frac{\alpha \hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}$$

**Default hyperparameters:**
$\alpha = 0.001,\ \beta_1 = 0.9,\ \beta_2 = 0.999,\ \epsilon = 10^{-8}$

> ⚠️ **Why bias correction?** $m_0 = v_0 = 0$ → early estimates biased toward zero!

### GD Convergence Speed

| Method | Convex | Non-Convex | Best For |
|---|---|---|---|
| Batch GD | $O(1/T)$ | Gets stuck | Small datasets |
| SGD (+ momentum) | $O(1/\sqrt{T})$ | Can escape saddles | NN training |
| Adam | Fast initial, then slow | Very practical | Default choice |
| L-BFGS (quasi-Newton) | Superlinear | Not used in deep learning | Small-medium convex |

---

# 📘 Section 36: Advanced Evaluation — ROC & Precision-Recall

## ROC Curve Construction Step-by-Step

Given 10 samples with prediction scores:

| Sample | True Label | Score |
|---|---|---|
| A | 1 | 0.95 |
| B | 1 | 0.87 |
| C | 0 | 0.82 |
| D | 1 | 0.75 |
| E | 0 | 0.60 |
| F | 1 | 0.55 |
| G | 0 | 0.40 |
| H | 0 | 0.30 |
| I | 1 | 0.20 |
| J | 0 | 0.10 |

Total: 5 positive, 5 negative.

### Threshold Analysis

| Threshold | Predicted Positive | TP | FP | FN | TN | TPR (Recall) | FPR |
|---|---|---|---|---|---|---|---|
| > 0.95 | 0 | 0 | 0 | 5 | 5 | 0.0 | 0.0 |
| > 0.87 | 1 (A) | 1 | 0 | 4 | 5 | 0.2 | 0.0 |
| > 0.82 | 2 (A,B) | 2 | 0 | 3 | 5 | 0.4 | 0.0 |
| > 0.75 | 3 (A,B,C) | 2 | 1 | 3 | 4 | 0.4 | 0.2 |
| > 0.60 | 4 (A-D) | 3 | 1 | 2 | 4 | 0.6 | 0.2 |
| > 0.55 | 5 (A-E) | 3 | 2 | 2 | 3 | 0.6 | 0.4 |
| > 0.40 | 6 (A-F) | 4 | 2 | 1 | 3 | 0.8 | 0.4 |
| > 0.30 | 7 (A-G) | 4 | 3 | 1 | 2 | 0.8 | 0.6 |
| > 0.20 | 8 (A-H) | 4 | 4 | 1 | 1 | 0.8 | 0.8 |
| > 0.10 | 9 (A-I) | 5 | 4 | 0 | 1 | 1.0 | 0.8 |
| > 0.0 | 10 (all) | 5 | 5 | 0 | 0 | 1.0 | 1.0 |

### AUC Calculation (Trapezoidal Rule)

Plot (FPR, TPR) and compute area:
Points: (0,0), (0,0.2), (0,0.4), (0.2,0.4), (0.2,0.6), (0.4,0.6), (0.4,0.8), (0.6,0.8), (0.8,0.8), (0.8,1.0), (1.0,1.0)

AUC ≈ **0.84** (good classifier!)

### AUC Interpretation

| AUC Range | Quality |
|---|---|
| 0.5 | Random (diagonal line) |
| 0.5-0.6 | Poor |
| 0.6-0.7 | Moderate |
| 0.7-0.8 | Acceptable |
| 0.8-0.9 | Good |
| 0.9-1.0 | Excellent |
| 1.0 | Perfect |

### When ROC vs Precision-Recall?

| Situation | Use | Why |
|---|---|---|
| Balanced classes | ROC | Both performing well |
| Imbalanced classes | PR curve | ROC overly optimistic |
| Care about positive class | PR curve | Focuses on positives |
| Need threshold-free metric | AUC-ROC | Standard |

---

# 📘 Section 37: Complete SVM Dual & Kernel Analysis

## Primal vs Dual Formulation

**Primal (Hard Margin):**
$$\min_{w,b} \frac{1}{2}\|w\|^2 \text{ s.t. } y_i(w^Tx_i + b) \geq 1$$

**Dual:**
$$\max_\alpha \sum_i \alpha_i - \frac{1}{2}\sum_{i,j} \alpha_i \alpha_j y_i y_j x_i^T x_j$$
$$\text{s.t. } \alpha_i \geq 0, \quad \sum_i \alpha_i y_i = 0$$

> 🎯 **Key Insight:** Dual depends on $x_i^T x_j$ (dot products) → can replace with kernel K(xᵢ, xⱼ)!

## Support Vectors

- Points with $\alpha_i > 0$ are **support vectors**
- They lie ON or INSIDE the margin
- Removing non-support vectors doesn't change the boundary!
- Typically small fraction of training data

## Kernel Functions Complete Reference

| Kernel | $K(x, x')$ | Hyperparams | VC Dim |
|---|---|---|---|
| Linear | $x^Tx'$ | None | $d+1$ |
| Polynomial | $(x^Tx' + c)^p$ | $c, p$ | $\binom{d+p}{p}$ |
| RBF/Gaussian | $\exp(-\gamma\|x-x'\|^2)$ | $\gamma$ | $\infty$ |
| Sigmoid | $\tanh(\alpha x^Tx' + c)$ | $\alpha, c$ | Varies |
| Laplacian | $\exp(-\gamma\|x-x'\|_1)$ | $\gamma$ | $\infty$ |

### Kernel Selection Guide

| Data Pattern | Best Kernel | Why |
|---|---|---|
| Linearly separable | Linear | Simplest, fastest, no overfitting risk |
| Somewhat non-linear | Polynomial (p=2,3) | Moderate non-linearity |
| Complex boundaries | RBF | Universal approximator |
| Not sure | RBF (default!) | Works in most cases |
| Very large dataset | Linear | RBF too slow (O(n²) kernel matrix) |

### C Parameter Analysis

| C Value | Misclassification Tolerance | Margin | Model |
|---|---|---|---|
| Very small (0.001) | High | Wide | High bias |
| Small (0.1) | Moderate | Wide | Underfitting possible |
| Medium (1) | Moderate | Medium | Balanced (default!) |
| Large (10) | Low | Narrow | Low bias |
| Very large (1000) | Nearly zero | Very narrow | Overfitting |

### γ (gamma) for RBF Analysis

| γ Value | σ = 1/√(2γ) | Each point's influence | Model |
|---|---|---|---|
| Very small (0.001) | Large radius | Far-reaching | Smooth, underfit |
| Small (0.01) | Large | Wide | Simple boundary |
| Medium (0.1) | Medium | Medium | Balanced |
| Large (1) | Small | Nearby only | Complex boundary |
| Very large (10) | Very small | Very local | Overfitting |

> ⚠️ **GATE Trap:** Large $\gamma$ = overfitting (each point creates a "bump"), NOT underfitting!

---

# 📘 Section 38: Probability Review for ML

## Quick Reference for GATE ML Problems

### Bayes' Theorem — Extended Form

$$P(C_k|x) = \frac{P(x|C_k)P(C_k)}{\sum_{j} P(x|C_j)P(C_j)}$$

### Common Distributions in ML

| Distribution | PMF/PDF | Mean | Variance | ML Use |
|---|---|---|---|---|
| Bernoulli(p) | $p^x(1-p)^{1-x}$ | $p$ | $p(1-p)$ | Binary classification |
| Binomial(n,p) | $\binom{n}{k}p^k(1-p)^{n-k}$ | $np$ | $np(1-p)$ | Count of successes |
| Multinomial | $\frac{n!}{x_1!...x_k!}\prod p_i^{x_i}$ | $np_i$ | $np_i(1-p_i)$ | Text classification |
| Poisson(λ) | $\frac{\lambda^k e^{-\lambda}}{k!}$ | $\lambda$ | $\lambda$ | Event counts |
| Normal(μ,σ²) | $\frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$ | $\mu$ | $\sigma^2$ | Everything! |
| Exponential(λ) | $\lambda e^{-\lambda x}$ | $1/\lambda$ | $1/\lambda^2$ | Time between events |
| Uniform(a,b) | $\frac{1}{b-a}$ | $\frac{a+b}{2}$ | $\frac{(b-a)^2}{12}$ | Random init |
| Beta(α,β) | $\frac{x^{\alpha-1}(1-x)^{\beta-1}}{B(\alpha,\beta)}$ | $\frac{\alpha}{\alpha+\beta}$ | — | Prior for probabilities |
| Multivariate Normal | $\frac{1}{\sqrt{(2\pi)^d|\Sigma|}}e^{-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)}$ | $\mu$ | $\Sigma$ | GMM, PCA |

### Conjugate Priors (Important for MAP)

| Likelihood | Conjugate Prior | Posterior |
|---|---|---|
| Bernoulli | Beta | Beta |
| Multinomial | Dirichlet | Dirichlet |
| Normal (known σ²) | Normal | Normal |
| Normal (known μ) | Inv-Gamma | Inv-Gamma |
| Normal (unknown μ, σ²) | Normal-Inv-Gamma | Normal-Inv-Gamma |
| Poisson | Gamma | Gamma |
| Exponential | Gamma | Gamma |

> 💡 **Why conjugate priors matter:** Posterior has same form as prior → closed-form MAP solution!

---

# 📘 Section 39: Time Series ML Basics

## Relevant for GATE DA

### Key Concepts

| Concept | Definition |
|---|---|
| **Stationarity** | Statistical properties don't change over time |
| **Trend** | Long-term increase/decrease |
| **Seasonality** | Regular periodic pattern |
| **Autocorrelation** | Correlation of series with its own lags |
| **White Noise** | Zero mean, constant variance, no autocorrelation |

### ML Models for Time Series

| Model | Type | Use |
|---|---|---|
| ARIMA | Statistical | Univariate forecasting |
| SARIMA | Statistical | Seasonal time series |
| Exponential Smoothing | Statistical | Simple forecasting |
| Linear Regression | ML | With time features |
| Random Forest | ML | With lag features |
| LSTM | Deep Learning | Complex patterns |
| Transformer | Deep Learning | State-of-the-art |

### Feature Engineering for Time Series

| Feature Type | Examples |
|---|---|
| **Lag features** | $x_{t-1}, x_{t-2}, ..., x_{t-k}$ |
| **Rolling statistics** | Rolling mean, std, min, max |
| **Time features** | Hour, day, month, quarter, is_weekend |
| **Calendar features** | Holiday, day_of_year |
| **Difference features** | $x_t - x_{t-1}$ |

> ⚠️ **Critical for CV:** Don't use random splits for time series! Use **time-based** splits (train on past, test on future).

---

# 📘 Section 40: Anomaly Detection

### Methods Summary

| Method | Type | Key Idea |
|---|---|---|
| Z-Score | Statistical | Flag if \|z\| > 3 |
| IQR | Statistical | Flag if outside Q1-1.5IQR to Q3+1.5IQR |
| Isolation Forest | Tree-based | Anomalies isolated quickly (shorter path) |
| One-Class SVM | SVM | Learn boundary around normal data |
| LOF (Local Outlier Factor) | Density | Compare local density to neighbors |
| DBSCAN | Clustering | Noise points are anomalies |
| Autoencoder | Neural | High reconstruction error = anomaly |
| GMM | Statistical | Low probability under model |

### Isolation Forest Worked Intuition

Normal point: needs many splits to isolate (deep in tree).
Anomaly: isolated quickly (shallow in tree).

Average path length = anomaly score. Short path → likely anomaly!

| Point Type | Avg Path Length | Score |
|---|---|---|
| Normal | Long (close to $\log_2 n$) | Low score (~0.5) |
| Anomaly | Short (much < $\log_2 n$) | High score (~1.0) |
| Boundary | Medium | Medium (~0.5-0.7) |

---


# 📝 Practice Set 11: Speed Round (P901 – P1000)

**Complete in < 10 seconds each!**

| # | Flash Question | Instant Answer |
|---|---|---|
| P901 | $\sigma(0)$ = ? | 0.5 |
| P902 | $\sigma(-\infty)$ = ? | 0 |
| P903 | $\sigma(+\infty)$ = ? | 1 |
| P904 | $\log_2 8$ = ? | 3 |
| P905 | $\log_2 1$ = ? | 0 |
| P906 | $-\log_2(0.5)$ = ? | 1 |
| P907 | $-\log_2(0.25)$ = ? | 2 |
| P908 | $e^0$ = ? | 1 |
| P909 | $\ln(1)$ = ? | 0 |
| P910 | $\ln(e)$ = ? | 1 |
| P911 | Entropy of (0.5, 0.5)? | 1 bit |
| P912 | Entropy of (1.0, 0.0)? | 0 bits |
| P913 | Entropy of (0.25, 0.25, 0.25, 0.25)? | 2 bits |
| P914 | Gini of (0.5, 0.5)? | 0.5 |
| P915 | Gini of (1.0, 0.0)? | 0 |
| P916 | Precision from (TP=80, FP=20)? | 0.8 |
| P917 | Recall from (TP=80, FN=20)? | 0.8 |
| P918 | F1 from P=0.8, R=0.8? | 0.8 |
| P919 | R² = 0 means? | Model predicts mean only |
| P920 | R² = 1 means? | Perfect fit |
| P921 | AUC = 1 means? | Perfect ranking |
| P922 | AUC = 0.5 means? | Random |
| P923 | SVM margin, \|w\| = 2? | Margin = 1 |
| P924 | SVM margin, \|w\| = 0.5? | Margin = 4 |
| P925 | 10 classes, OvA = ? | 10 classifiers |
| P926 | 10 classes, OvO = ? | 45 classifiers |
| P927 | k-fold, k=10, 1000 samples: test size? | 100 |
| P928 | Bootstrap: P(not selected) for 1 sample? | $(1-1/n)^n \approx 1/e \approx 0.368$ |
| P929 | So OOB fraction ≈ ? | 36.8% |
| P930 | LASSO penalty? | L1: $\lambda\sum|w_i|$ |
| P931 | Ridge penalty? | L2: $\lambda\sum w_i^2$ |
| P932 | PCA: d features, keep k components. k ≤ ? | min(n-1, d) |
| P933 | LDA: c classes, keep k components. k ≤ ? | c-1 |
| P934 | K-means iterations guarantee? | Finite (always converges) |
| P935 | DBSCAN: need k? | No! |
| P936 | GMM: E-step computes? | Responsibilities |
| P937 | GMM: M-step updates? | Parameters |
| P938 | AdaBoost: misclassified weight direction? | Increases |
| P939 | RF: each tree sees all features? | No (random subset per split) |
| P940 | Gradient of $x^2$ at $x=3$? | 6 |
| P941 | Gradient descent with $\alpha=0.1$: $x' = 3 - 0.1(6)$? | 2.4 |
| P942 | Conv output: input 32, kernel 5, stride 1, pad 0? | 28 |
| P943 | Conv output: input 32, kernel 3, stride 2, pad 1? | 16 |
| P944 | Dense layer params: 100→50? | 5050 (100×50+50) |
| P945 | Dropout 0.5 at test: scale weights by? | 0.5 (or use inverted dropout during training) |
| P946 | BN learnable params per layer of width w? | 2w (γ and β) |
| P947 | Adam default $\beta_1$? | 0.9 |
| P948 | Adam default $\beta_2$? | 0.999 |
| P949 | Cosine sim of orthogonal vectors? | 0 |
| P950 | L2 norm of [3,4]? | 5 |
| P951 | L1 norm of [3,-4]? | 7 |
| P952 | Softmax of [0,0,0]? | [1/3, 1/3, 1/3] |
| P953 | Cross-entropy for correct class prob 1.0? | 0 |
| P954 | Cross-entropy for correct class prob 0.01? | $-\ln(0.01) = 4.605$ |
| P955 | KL(P\|\|Q) = 0 when? | P = Q |
| P956 | Vanishing gradient: which activation? | Sigmoid, Tanh |
| P957 | Exploding gradient fix? | Gradient clipping |
| P958 | ReLU derivative for positive x? | 1 |
| P959 | ReLU derivative for negative x? | 0 |
| P960 | Leaky ReLU negative slope? | Small constant (e.g., 0.01) |
| P961 | Mean of Uniform(0,1)? | 0.5 |
| P962 | Variance of Uniform(0,1)? | 1/12 |
| P963 | MLE of Bernoulli p? | $\hat{p} = \bar{x}$ (sample proportion) |
| P964 | Chain rule: $\frac{\partial L}{\partial w} = \frac{\partial L}{\partial y} \cdot \frac{\partial y}{\partial w}$? | Yes (backpropagation!) |
| P965 | Bias term purpose? | Shift decision boundary (intercept) |
| P966 | Epochs = 20, dataset = 1000, batch = 100: iterations? | 200 (20 × 10) |
| P967 | Multicollinearity affects? | Linear regression (unstable coefficients) |
| P968 | VIF > 5 indicates? | Significant multicollinearity |
| P969 | VIF > 10 indicates? | Severe multicollinearity |
| P970 | Ridge helps multicollinearity? | Yes! Stabilizes coefficients |
| P971 | LogReg: odds ratio for $w_1 = 0.5$? | $e^{0.5} = 1.649$ |
| P972 | LogReg: unit increase in $x_1$ multiplies odds by? | $e^{w_1}$ |
| P973 | Decision tree depth d: max leaf nodes? | $2^d$ |
| P974 | Full binary tree: n leaves → n-1 internal nodes? | Yes |
| P975 | Ensemble of m models, each accuracy p (independent): majority vote? | Better! (by CLT) |
| P976 | Boosting: base model must be better than? | Random (error < 0.5) |
| P977 | Weak learner = ? | Slightly better than random |
| P978 | Strong learner = ? | Arbitrarily good accuracy |
| P979 | Boosting: weak → strong? | Yes (Schapire's theorem) |
| P980 | PAC learning: samples needed ∝ ? | $1/\epsilon$ and $\log(1/\delta)$ |
| P981 | VC dim of line in 2D? | 3 |
| P982 | VC dim of hyperplane in $\mathbb{R}^d$? | $d+1$ |
| P983 | VC dim of 1-NN? | ∞ |
| P984 | Higher VC dim → more or less complex? | More complex |
| P985 | t-SNE preserves? | Local structure (neighborhoods) |
| P986 | t-SNE can be used for classification? | No! Only visualization |
| P987 | UMAP advantage over t-SNE? | Faster, preserves more global structure |
| P988 | SMOTE creates? | Synthetic minority samples (interpolation) |
| P989 | Stratified sampling ensures? | Same class proportions in each fold |
| P990 | Leakage: target info in features? | Data leakage → artificially high performance |
| P991 | How to detect leakage? | Unrealistically high cross-val scores |
| P992 | Correlation ≠ ? | Causation |
| P993 | Simpson's paradox? | Trend reverses when data aggregated |
| P994 | Confounding variable? | Affects both X and Y, causing spurious correlation |
| P995 | ML model is only as good as? | Its training data |
| P996 | Bias in data → ? | Bias in model |
| P997 | Feature importance ≠ ? | Causal importance |
| P998 | Ensemble > single model usually? | Yes (wisdom of crowds) |
| P999 | No single best algorithm for all? | No Free Lunch theorem! |
| P1000 | Done with 1000 ML problems? | 🎉 **CONGRATULATIONS!** |

---

# 🏅 Ultimate ML Summary — One-Page Quick Revision Card

## The 5 ML Paradigms
1. **Supervised** → Input + label → predict label
2. **Unsupervised** → Input only → find structure
3. **Semi-supervised** → Some labels → use unlabeled too
4. **Reinforcement** → Actions → rewards → policy
5. **Self-supervised** → Create labels from data itself

## Top 10 Algorithms for GATE
1. Linear/Logistic Regression
2. SVM (Linear + RBF kernel)
3. Decision Tree (ID3/C4.5/CART)
4. Random Forest
5. Gradient Boosting / XGBoost
6. KNN
7. Naive Bayes
8. PCA / LDA
9. K-Means / DBSCAN
10. Neural Networks (Perceptron, MLP)

## The 3 Most Common GATE Traps
1. **MLE of variance is biased** (uses 1/n not 1/(n-1))
2. **SVM margin is 2/||w||** (not 1/||w||)
3. **LASSO can give exact zeros** (Ridge cannot)

## Critical Thresholds
- Sigmoid(0) = 0.5
- Sigmoid(1) ≈ 0.731
- Sigmoid(-1) ≈ 0.269
- $\ln 2 \approx 0.693$
- $1/e \approx 0.368$
- $\sqrt{2} \approx 1.414$

## Golden Formula: Bias-Variance
$$\text{Expected Error} = \text{Bias}^2 + \text{Variance} + \text{Irreducible Noise}$$

---

# 📝 Practice Set 12: Complete Mathematical Derivations (P1001 – P1100)

## Linear Regression Derivation from Scratch

**P1001.** Derive the Normal Equation for Simple Linear Regression.

Given: Data points $(x_1, y_1), \ldots, (x_n, y_n)$. Model: $\hat{y} = w_0 + w_1 x$.

**Step 1:** Define loss function (MSE):
$$L(w_0, w_1) = \frac{1}{n}\sum_{i=1}^{n}(y_i - w_0 - w_1 x_i)^2$$

**Step 2:** Take partial derivatives and set to zero:
$$\frac{\partial L}{\partial w_0} = \frac{-2}{n}\sum_{i=1}^{n}(y_i - w_0 - w_1 x_i) = 0$$
$$\frac{\partial L}{\partial w_1} = \frac{-2}{n}\sum_{i=1}^{n}x_i(y_i - w_0 - w_1 x_i) = 0$$

**Step 3:** From first equation:
$$\sum y_i = n w_0 + w_1 \sum x_i$$
$$w_0 = \bar{y} - w_1 \bar{x}$$

**Step 4:** Substitute into second equation:
$$w_1 = \frac{\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^{n}(x_i - \bar{x})^2} = \frac{S_{xy}}{S_{xx}}$$

> 🎯 **GATE Formula:** $w_1 = \frac{n\sum x_i y_i - \sum x_i \sum y_i}{n\sum x_i^2 - (\sum x_i)^2}$, $w_0 = \bar{y} - w_1\bar{x}$

**P1002.** Derive the Normal Equation in matrix form.

Model: $\hat{y} = Xw$ where $X$ is $n \times p$ design matrix, $w$ is $p \times 1$.

$$L(w) = (y - Xw)^T(y - Xw) = y^Ty - 2w^TX^Ty + w^TX^TXw$$

$$\frac{\partial L}{\partial w} = -2X^Ty + 2X^TXw = 0$$

$$\boxed{w^* = (X^TX)^{-1}X^Ty}$$

> ⚠️ **GATE Trap:** $(X^TX)^{-1}$ must exist! If features are linearly dependent → singular → use regularization.

**P1003.** When does the Normal Equation fail?
- When $X^TX$ is singular (not invertible)
- When $n < p$ (more features than samples)
- When features are perfectly correlated (multicollinearity)
→ **Solution:** Ridge regression adds $\lambda I$: $(X^TX + \lambda I)^{-1}X^Ty$ — always invertible!

**P1004.** Prove that Ridge Regression solution is $w^* = (X^TX + \lambda I)^{-1}X^Ty$.

$$L(w) = \|y - Xw\|^2 + \lambda\|w\|^2$$
$$= y^Ty - 2w^TX^Ty + w^TX^TXw + \lambda w^Tw$$
$$\frac{\partial L}{\partial w} = -2X^Ty + 2X^TXw + 2\lambda w = 0$$
$$(X^TX + \lambda I)w = X^Ty$$
$$\boxed{w^* = (X^TX + \lambda I)^{-1}X^Ty}$$

**P1005.** Show that Gradient Descent update for MSE loss with learning rate $\eta$:

$$w_{t+1} = w_t - \eta \cdot \frac{\partial L}{\partial w} = w_t + \frac{2\eta}{n}\sum_{i=1}^{n}(y_i - w_t \cdot x_i)x_i$$

For a single weight: $w_{t+1} = w_t + \eta(y_i - \hat{y}_i)x_i$ (Perceptron-like update!)

## Logistic Regression Derivation

**P1006.** Derive the Logistic Regression loss function (Binary Cross-Entropy).

Given: $p(y=1|x) = \sigma(w^Tx) = \frac{1}{1+e^{-w^Tx}}$

For single sample: $P(y|x) = \hat{p}^y(1-\hat{p})^{1-y}$ where $\hat{p} = \sigma(w^Tx)$

Log-likelihood:
$$\ell(w) = \sum_{i=1}^{n}[y_i\log\hat{p}_i + (1-y_i)\log(1-\hat{p}_i)]$$

Loss = negative log-likelihood:
$$\boxed{L(w) = -\frac{1}{n}\sum_{i=1}^{n}[y_i\log\hat{p}_i + (1-y_i)\log(1-\hat{p}_i)]}$$

> 🎯 **GATE Key:** This is the **Binary Cross-Entropy** (BCE) loss. It's CONVEX = guaranteed global minimum!

**P1007.** Gradient of logistic regression loss:

$$\frac{\partial L}{\partial w_j} = -\frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{p}_i)x_{ij}$$

Key property: Same form as linear regression gradient! But $\hat{p}$ is sigmoid, not linear.

**P1008.** Why is Logistic Regression convex?

The Hessian $H = X^T \text{diag}(\hat{p}(1-\hat{p}))X$ is positive semi-definite because:
- $\hat{p}(1-\hat{p}) > 0$ for all points (sigmoid output ∈ (0,1))
- $\text{diag}$ creates PSD matrix
- $X^TAX$ is PSD if $A$ is PSD

→ **No local minima! GD always finds global minimum!** ✅

## SVM Derivation

**P1009.** Derive the SVM margin = $\frac{2}{\|w\|}$.

For a hyperplane $w^Tx + b = 0$, the distance from point $x_0$ is:
$$d = \frac{|w^Tx_0 + b|}{\|w\|}$$

For support vectors: $|w^Tx + b| = 1$ (by convention).

Margin = distance between the two support vector planes:
$$\text{Margin} = \frac{2}{\|w\|}$$

Maximizing margin = minimizing $\|w\|$ = minimizing $\frac{1}{2}\|w\|^2$ (easier to optimize).

**P1010.** SVM Primal Formulation (Hard margin):
$$\min_{w,b} \frac{1}{2}\|w\|^2 \quad \text{s.t.} \quad y_i(w^Tx_i + b) \geq 1 \quad \forall i$$

**P1011.** Soft Margin SVM (allowing misclassification):
$$\min_{w,b,\xi} \frac{1}{2}\|w\|^2 + C\sum_{i=1}^{n}\xi_i$$
$$\text{s.t.} \quad y_i(w^Tx_i + b) \geq 1 - \xi_i, \quad \xi_i \geq 0$$

- $C$ large → hard margin (less tolerance for errors)
- $C$ small → soft margin (more tolerance)

**P1012.** SVM Dual Formulation:
$$\max_{\alpha} \sum_{i=1}^{n}\alpha_i - \frac{1}{2}\sum_{i,j}\alpha_i\alpha_j y_i y_j (x_i \cdot x_j)$$
$$\text{s.t.} \quad 0 \leq \alpha_i \leq C, \quad \sum \alpha_i y_i = 0$$

> 🎯 **GATE Key:** Dual uses dot products $x_i \cdot x_j$ → can replace with kernel $K(x_i, x_j)$!

## PCA Derivation

**P1013.** Derive PCA as maximizing variance.

Goal: Find direction $u$ that maximizes variance of projected data.

Projection of $x_i$ onto $u$: $z_i = u^Tx_i$

Variance of projected data (assuming centered): $\text{Var}(z) = u^T\Sigma u$ where $\Sigma = \frac{1}{n}X^TX$.

Maximize $u^T\Sigma u$ subject to $\|u\| = 1$:
$$\mathcal{L} = u^T\Sigma u - \lambda(u^Tu - 1)$$
$$\frac{\partial \mathcal{L}}{\partial u} = 2\Sigma u - 2\lambda u = 0$$
$$\boxed{\Sigma u = \lambda u}$$

→ $u$ is an eigenvector of $\Sigma$! Variance = $u^T\Sigma u = u^T\lambda u = \lambda$.

→ **Maximum variance = largest eigenvalue. Direction = corresponding eigenvector!**

**P1014.** Variance explained by top-$k$ components:
$$\text{Variance explained} = \frac{\sum_{i=1}^{k}\lambda_i}{\sum_{i=1}^{d}\lambda_i}$$

**P1015.** Worked example: Given $\Sigma = \begin{pmatrix} 5 & 2 \\ 2 & 3 \end{pmatrix}$, find PC1 direction and variance explained.

Eigenvalues: $\det(\Sigma - \lambda I) = 0$
$(5-\lambda)(3-\lambda) - 4 = 0$
$\lambda^2 - 8\lambda + 11 = 0$
$\lambda = \frac{8 \pm \sqrt{64-44}}{2} = \frac{8 \pm \sqrt{20}}{2} = 4 \pm \sqrt{5}$
$\lambda_1 = 4 + \sqrt{5} \approx 6.236$, $\lambda_2 = 4 - \sqrt{5} \approx 1.764$

Variance explained by PC1: $\frac{6.236}{6.236 + 1.764} = \frac{6.236}{8} \approx 77.95\%$

## Naive Bayes Derivation

**P1016.** Derive the Naive Bayes classifier from Bayes' theorem.

$$P(C_k|x) = \frac{P(x|C_k)P(C_k)}{P(x)} \propto P(x|C_k)P(C_k)$$

Naive assumption: features are conditionally independent given class:
$$P(x|C_k) = \prod_{j=1}^{d}P(x_j|C_k)$$

Classification rule:
$$\hat{y} = \arg\max_{C_k} P(C_k)\prod_{j=1}^{d}P(x_j|C_k)$$

In log space:
$$\hat{y} = \arg\max_{C_k} \left[\log P(C_k) + \sum_{j=1}^{d}\log P(x_j|C_k)\right]$$

> ⚠️ **GATE Trap:** Laplace smoothing: $P(x_j|C_k) = \frac{\text{count}(x_j, C_k) + 1}{\text{count}(C_k) + |V|}$ where $|V|$ = vocabulary size.

**P1017.** When does Naive Bayes outperform more complex models?
- Small training set (fewer parameters to estimate)
- High-dimensional data (text classification)
- When features are approximately independent
- When you need a baseline quickly

**P1018-P1030** — Derivation quick drills:

| # | Question | Key Result |
|---|---|---|
| P1018 | MSE gradient w.r.t. single weight $w_j$ | $\frac{-2}{n}\sum(y_i - \hat{y}_i)x_{ij}$ |
| P1019 | Sigmoid derivative $\sigma'(z)$ | $\sigma(z)(1-\sigma(z))$ |
| P1020 | Sigmoid property: $\sigma(-z)$ | $1 - \sigma(z)$ |
| P1021 | Log-odds in logistic regression | $\log\frac{p}{1-p} = w^Tx$ |
| P1022 | Information Gain formula | $IG = H(\text{parent}) - \sum \frac{|S_v|}{|S|}H(S_v)$ |
| P1023 | Entropy of fair coin | $H = -2 \times 0.5 \log_2 0.5 = 1$ bit |
| P1024 | Entropy of biased coin (p=0.9) | $-0.9\log_2 0.9 - 0.1\log_2 0.1 \approx 0.469$ bits |
| P1025 | Gini of pure node | 0 |
| P1026 | Gini of balanced binary class | $1 - 0.5^2 - 0.5^2 = 0.5$ |
| P1027 | K-Means objective | $\min \sum_{i=1}^{k}\sum_{x \in C_i}\|x - \mu_i\|^2$ |
| P1028 | Kernel trick: map $x \to \phi(x)$, compute $\phi(x)^T\phi(z)$ as? | $K(x,z)$ without computing $\phi$ |
| P1029 | RBF kernel: $K(x,z) = \exp(-\gamma\|x-z\|^2)$. As $\gamma \to \infty$? | Each point = own cluster (overfit) |
| P1030 | Perceptron convergence theorem: converges if? | Data is linearly separable |

## Cross-Entropy for Multi-class

**P1031.** Multi-class Cross-Entropy (Softmax):

$$L = -\frac{1}{n}\sum_{i=1}^{n}\sum_{k=1}^{K}y_{ik}\log(\hat{p}_{ik})$$

where $\hat{p}_{ik} = \frac{e^{z_{ik}}}{\sum_{j=1}^{K}e^{z_{ij}}}$ (softmax)

**P1032.** Softmax properties:
- All outputs sum to 1
- All outputs ∈ (0,1)
- Invariant to constant shift: $\text{softmax}(z) = \text{softmax}(z - c)$
- Gradient: $\frac{\partial L}{\partial z_k} = \hat{p}_k - y_k$ (elegant!)

**P1033.** Why softmax + cross-entropy, not sigmoid for multi-class?
- Softmax ensures outputs sum to 1 (proper probability distribution)
- Sigmoid treats each class independently (good for multi-label, not multi-class)

**P1034-P1050** — Worked numerical problems:

**P1034.** Given $z = [2.0, 1.0, 0.1]$, compute softmax.
$$e^{2.0} = 7.389, \quad e^{1.0} = 2.718, \quad e^{0.1} = 1.105$$
$$\text{sum} = 7.389 + 2.718 + 1.105 = 11.212$$
$$\hat{p} = [0.659, 0.242, 0.099]$$
→ Class 1 has highest probability ✅

**P1035.** Given true label = class 1 (one-hot: [1,0,0]), what is the cross-entropy loss?
$$L = -[1 \cdot \log(0.659) + 0 + 0] = -\log(0.659) \approx 0.417$$

**P1036.** Given true label = class 3 (one-hot: [0,0,1]):
$$L = -\log(0.099) \approx 2.313$$
→ Higher loss because model is very wrong! ✅

**P1037.** Perceptron weight update:
- Initial: $w = [0, 0]$, $b = 0$, learning rate $\eta = 1$
- Sample: $x = [1, 1]$, $y = 1$
- Prediction: $\hat{y} = \text{sign}(w \cdot x + b) = \text{sign}(0) = 0$ (wrong!)
- Update: $w = w + \eta \cdot y \cdot x = [0,0] + 1 \cdot 1 \cdot [1,1] = [1,1]$, $b = 0 + 1 = 1$

**P1038.** K-Means iteration:
- Points: (1,1), (2,1), (4,3), (5,4)
- Initial centroids: $\mu_1 = (1,1)$, $\mu_2 = (5,4)$

Step 1 — Assign:
| Point | Distance to $\mu_1$ | Distance to $\mu_2$ | Cluster |
|---|---|---|---|
| (1,1) | 0 | $\sqrt{16+9}=5$ | C1 |
| (2,1) | 1 | $\sqrt{9+9}=4.24$ | C1 |
| (4,3) | $\sqrt{9+4}=3.61$ | $\sqrt{1+1}=1.41$ | C2 |
| (5,4) | $\sqrt{16+9}=5$ | 0 | C2 |

Step 2 — Update centroids:
$\mu_1 = \frac{(1,1)+(2,1)}{2} = (1.5, 1)$
$\mu_2 = \frac{(4,3)+(5,4)}{2} = (4.5, 3.5)$

Step 3 — Reassign (check if anything changed):
| Point | Distance to $\mu_1'$ | Distance to $\mu_2'$ | Cluster |
|---|---|---|---|
| (1,1) | 0.5 | 4.30 | C1 |
| (2,1) | 0.5 | 3.54 | C1 |
| (4,3) | 3.04 | 0.71 | C2 |
| (5,4) | 4.61 | 0.71 | C2 |

No change → **Converged!** Final centroids: (1.5, 1) and (4.5, 3.5).

**P1039.** Decision Tree — Entropy Calculation:
- Dataset: 10 samples, 6 positive, 4 negative
- $H(S) = -\frac{6}{10}\log_2\frac{6}{10} - \frac{4}{10}\log_2\frac{4}{10}$
- $= -0.6 \times (-0.737) - 0.4 \times (-1.322)$
- $= 0.442 + 0.529 = 0.971$ bits

**P1040.** Split on feature A: Left = [4+, 1-], Right = [2+, 3-]
- $H(\text{Left}) = -\frac{4}{5}\log_2\frac{4}{5} - \frac{1}{5}\log_2\frac{1}{5} = 0.722$
- $H(\text{Right}) = -\frac{2}{5}\log_2\frac{2}{5} - \frac{3}{5}\log_2\frac{3}{5} = 0.971$
- $\text{IG}(A) = 0.971 - \frac{5}{10}(0.722) - \frac{5}{10}(0.971) = 0.971 - 0.361 - 0.486 = 0.124$

**P1041.** Split on feature B: Left = [5+, 0-], Right = [1+, 4-]
- $H(\text{Left}) = 0$ (pure!)
- $H(\text{Right}) = -\frac{1}{5}\log_2\frac{1}{5} - \frac{4}{5}\log_2\frac{4}{5} = 0.722$
- $\text{IG}(B) = 0.971 - \frac{5}{10}(0) - \frac{5}{10}(0.722) = 0.971 - 0 - 0.361 = 0.610$

→ **Feature B has higher IG (0.610 > 0.124) → Split on B first!** ✅

**P1042.** Gini Index calculation:
$$\text{Gini}(S) = 1 - \sum_{k}p_k^2$$

For [6+, 4-]: $\text{Gini} = 1 - (0.6)^2 - (0.4)^2 = 1 - 0.36 - 0.16 = 0.48$

**P1043.** Gini for split on B:
- Left [5+, 0-]: $\text{Gini} = 1 - 1^2 = 0$
- Right [1+, 4-]: $\text{Gini} = 1 - (0.2)^2 - (0.8)^2 = 0.32$
- Weighted: $\frac{5}{10}(0) + \frac{5}{10}(0.32) = 0.16$
- Gini gain: $0.48 - 0.16 = 0.32$

**P1044-P1050** — Quick numerical drill:

| # | Problem | Answer |
|---|---|---|
| P1044 | $R^2 = 1 - \frac{SS_{res}}{SS_{tot}}$. If $SS_{res}=20, SS_{tot}=100$? | $R^2 = 0.8$ |
| P1045 | MSE = 4, what is RMSE? | $\sqrt{4} = 2$ |
| P1046 | If $w = [3, 4]$, what is the SVM margin? | $\frac{2}{\sqrt{9+16}} = \frac{2}{5} = 0.4$ |
| P1047 | Eigenvalues: [5, 3, 1, 1]. Variance by top-2 PCs? | $\frac{5+3}{10} = 80\%$ |
| P1048 | Precision = 0.9, Recall = 0.6. F1-score? | $2 \times \frac{0.9 \times 0.6}{0.9 + 0.6} = \frac{1.08}{1.5} = 0.72$ |
| P1049 | TP=80, FP=20, FN=10, TN=90. Accuracy? | $\frac{80+90}{200} = 85\%$ |
| P1050 | Same: Precision? Recall? | P=$\frac{80}{100}=0.8$, R=$\frac{80}{90}=0.889$ |

---

# 📝 Practice Set 13: Kernel Methods & Feature Mapping (P1051 – P1120)

**P1051.** Why do we need kernels?
→ Data not linearly separable in original space. Map to higher dimension where it IS separable. But computing the mapping $\phi(x)$ explicitly is expensive. Kernel computes $\phi(x) \cdot \phi(z)$ directly!

**P1052.** XOR problem: Points $(0,0)→-1$, $(0,1)→+1$, $(1,0)→+1$, $(1,1)→-1$.
Not linearly separable in 2D! Map $x = (x_1, x_2) \to \phi(x) = (x_1, x_2, x_1 x_2)$:
- $(0,0) \to (0,0,0)→-1$
- $(0,1) \to (0,1,0)→+1$
- $(1,0) \to (1,0,0)→+1$
- $(1,1) \to (1,1,1)→-1$

Now separable with $w = [-1, 2, -2], b = 0$ (check: plane $-x_1 + 2x_2 - 2x_1x_2 = 0$) ✅

**P1053.** Polynomial kernel: $K(x,z) = (x \cdot z + c)^d$

For $d=2, c=1, x=(x_1,x_2), z=(z_1,z_2)$:
$$K(x,z) = (x_1z_1 + x_2z_2 + 1)^2$$
$$= x_1^2z_1^2 + x_2^2z_2^2 + 1 + 2x_1z_1x_2z_2 + 2x_1z_1 + 2x_2z_2$$

Implicit feature map: $\phi(x) = (x_1^2, x_2^2, 1, \sqrt{2}x_1x_2, \sqrt{2}x_1, \sqrt{2}x_2)$

→ Maps 2D → 6D automatically! No need to compute $\phi$ explicitly!

**P1054.** RBF (Gaussian) kernel: $K(x,z) = \exp(-\gamma\|x-z\|^2)$

| $\gamma$ | Effect | Risk |
|---|---|---|
| Very small | Smooth decision boundary | Underfit |
| Medium | Balanced | Good |
| Very large | Complex/wiggly boundary | Overfit |

> 🎯 **GATE Key:** RBF maps to INFINITE-dimensional space! But we never compute $\phi$ — just the kernel value.

**P1055.** Mercer's condition: A function $K(x,z)$ is a valid kernel if:
- Symmetric: $K(x,z) = K(z,x)$
- The Gram matrix $K_{ij} = K(x_i, x_j)$ is positive semi-definite for ALL datasets.

**P1056-P1080** — Kernel computation drill:

| # | Problem | Answer |
|---|---|---|
| P1056 | Linear kernel: $K(x,z) = x \cdot z$. $x=[1,2], z=[3,4]$? | $3+8=11$ |
| P1057 | Polynomial (d=2, c=0): $K = (x \cdot z)^2$. Same x,z? | $11^2 = 121$ |
| P1058 | Polynomial (d=2, c=1): $K = (x \cdot z + 1)^2$. Same x,z? | $12^2 = 144$ |
| P1059 | RBF: $\gamma=0.5, x=[0], z=[1]$. $K = e^{-0.5 \times 1} =?$ | $e^{-0.5} \approx 0.607$ |
| P1060 | RBF: $\gamma=0.5, x=[0], z=[0]$. $K =?$ | $e^0 = 1$ (identical points → K=1!) |
| P1061 | RBF: $\gamma=0.5, x=[0], z=[10]$. $K =?$ | $e^{-50} \approx 0$ (far apart → K≈0) |
| P1062 | Sigmoid kernel: $K = \tanh(\alpha x \cdot z + c)$. Valid kernel? | NOT always valid (violates Mercer for some $\alpha,c$) |
| P1063 | If $K_1, K_2$ are valid kernels, is $K_1 + K_2$ valid? | Yes! |
| P1064 | Is $K_1 \times K_2$ valid? | Yes! |
| P1065 | Is $cK_1$ valid for $c > 0$? | Yes! |
| P1066 | Is $K(x,z) = \|x - z\|^2$ a valid kernel? | No! (not PSD) |
| P1067 | What makes SVM with RBF kernel a universal approximator? | Infinite-dimensional feature space |
| P1068 | Kernel PCA vs regular PCA? | Kernel PCA captures nonlinear structure |
| P1069 | Number of support vectors affects? | Model complexity, prediction speed |
| P1070 | Fewer support vectors means? | Better generalization, simpler model |
| P1071 | If ALL training points are support vectors? | Model is too complex → likely overfitting |
| P1072 | SVM with linear kernel equivalent to? | Hard-margin or soft-margin linear classifier |
| P1073 | What happens if $C \to \infty$ in SVM? | Hard margin (no slack allowed) |
| P1074 | What happens if $C \to 0$ in SVM? | All misclassified → model ignores data |
| P1075 | For linearly separable data, what kernel? | Linear (simplest, fastest) |
| P1076 | For circular decision boundary? | RBF or Polynomial |
| P1077 | For text classification? | Linear (high-dimensional, often separable) |
| P1078 | One-vs-All for K classes creates? | K binary classifiers |
| P1079 | One-vs-One for K classes creates? | $\binom{K}{2} = \frac{K(K-1)}{2}$ classifiers |
| P1080 | Which is faster for many classes? | One-vs-All (fewer classifiers) |

---

# 📝 Practice Set 14: Ensemble Methods — Advanced (P1081 – P1180)

**P1081.** Complete Random Forest Algorithm:
1. For $b = 1, \ldots, B$ (number of trees):
   a. Draw bootstrap sample of size $n$ from training data
   b. Build decision tree, at each node:
      - Select $m = \lfloor\sqrt{p}\rfloor$ random features (classification) or $m = p/3$ (regression)
      - Find best split among those $m$ features
      - Split (no pruning)
2. For prediction: majority vote (classification) or average (regression)

**P1082.** Why $\sqrt{p}$ features?
→ If $p$ features and one is very strong, every tree would split on it first → high correlation between trees → high variance. Random feature selection decorrelates trees!

**P1083.** Out-of-Bag (OOB) Error:
- Each bootstrap sample uses ~63.2% of training data
- Remaining ~36.8% = OOB samples
- OOB error ≈ test error → free cross-validation!

**P1084.** Prove ~63.2%: Probability a sample is NOT picked in $n$ draws:
$$P(\text{not picked}) = \left(1 - \frac{1}{n}\right)^n \xrightarrow{n \to \infty} \frac{1}{e} \approx 0.368$$
So probability of being picked: $1 - \frac{1}{e} \approx 0.632$ ✅

**P1085.** AdaBoost iteration:

| Step | Action | Formula |
|---|---|---|
| Initialize | Equal weights | $w_i = \frac{1}{n}$ |
| Train | Weighted classifier | Minimize weighted error $\epsilon$ |
| Compute alpha | Classifier weight | $\alpha = \frac{1}{2}\ln\frac{1-\epsilon}{\epsilon}$ |
| Update weights | Increase for misclassified | $w_i \leftarrow w_i \cdot e^{-\alpha y_i h(x_i)}$ |
| Normalize | Sum to 1 | $w_i \leftarrow \frac{w_i}{\sum w_j}$ |

**P1086.** Worked AdaBoost example:
- 5 samples, initially $w = [0.2, 0.2, 0.2, 0.2, 0.2]$
- Weak learner misclassifies sample 3: $\epsilon = 0.2$
- $\alpha = \frac{1}{2}\ln\frac{0.8}{0.2} = \frac{1}{2}\ln 4 = \frac{1}{2} \times 1.386 = 0.693$

Update weights:
- Correctly classified: $w_i \times e^{-0.693} = 0.2 \times 0.5 = 0.1$
- Misclassified (sample 3): $w_i \times e^{0.693} = 0.2 \times 2 = 0.4$

After normalizing: $w = [0.1, 0.1, 0.4, 0.1, 0.1] / 0.8 = [0.125, 0.125, 0.5, 0.125, 0.125]$

→ Misclassified sample now has 4× weight of correctly classified ones! ✅

**P1087.** Gradient Boosting vs AdaBoost:

| Feature | AdaBoost | Gradient Boosting |
|---|---|---|
| Strategy | Reweight samples | Fit residuals |
| Loss function | Exponential | Any differentiable |
| Weak learner | Decision stumps | Decision trees (deeper) |
| Update rule | Sample weights | Gradient of loss |
| Robustness | Sensitive to outliers | More flexible |

**P1088.** XGBoost advantages over regular Gradient Boosting:
- Regularization built-in (L1 + L2)
- Handles missing values natively
- Parallelized tree building
- Pruning with depth-first approach
- Column subsampling (like Random Forest)

**P1089-P1120** — Ensemble concepts:

| # | Question | Answer |
|---|---|---|
| P1089 | Bagging reduces? | Variance |
| P1090 | Boosting reduces? | Bias (primarily) |
| P1091 | Stacking uses? | Meta-learner on base model predictions |
| P1092 | Random Forest = Bagging + ? | Random feature selection at each node |
| P1093 | More trees in Random Forest → overfit? | No! (error converges, doesn't increase) |
| P1094 | More rounds in AdaBoost → overfit? | Eventually yes (but slowly) |
| P1095 | OOB error is similar to? | Leave-one-out cross-validation error |
| P1096 | Feature importance in Random Forest | Mean decrease in Gini/entropy across trees |
| P1097 | Permutation importance | Shuffle feature, measure accuracy drop |
| P1098 | Gradient Boosting learning rate? | Shrinkage: scale each tree's contribution |
| P1099 | Low learning rate needs? | More trees (but better generalization) |
| P1100 | Max tree depth in Gradient Boosting? | Usually 3-8 (shallow trees = weak learners) |
| P1101 | Subsample ratio in Gradient Boosting? | Stochastic GB: use fraction of data per tree |
| P1102 | Early stopping in boosting? | Stop when validation error increases |
| P1103 | Voting ensemble types? | Hard voting (majority) vs Soft voting (avg prob) |
| P1104 | When is soft voting better? | When base classifiers output calibrated probabilities |
| P1105 | Bootstrap aggregating = ? | Bagging |
| P1106 | Why bagging works? | Reduces variance by averaging independent estimates |
| P1107 | If base models are correlated, bagging effect? | Less variance reduction |
| P1108 | Random Forest tree count: too few? | High variance, unstable |
| P1109 | Random Forest tree count: enough? | Error plateaus → diminishing returns |
| P1110 | AdaBoost weights: $\alpha$ when $\epsilon = 0.5$? | $\alpha = 0$ (random guessing → zero weight!) |
| P1111 | AdaBoost weights: $\alpha$ when $\epsilon = 0$? | $\alpha = \infty$ (perfect classifier → max weight!) |
| P1112 | AdaBoost weights: $\alpha$ when $\epsilon > 0.5$? | $\alpha < 0$ (worse than random → flip predictions) |
| P1113 | Bagging + Decision Trees = ? | Random Forest (approximately) |
| P1114 | Boosting + Decision Stumps = ? | AdaBoost |
| P1115 | Boosting + Full Trees = ? | Gradient Boosted Trees |
| P1116 | Ensemble of SVM, LR, KNN = ? | Voting/Stacking classifier |
| P1117 | Bias of Random Forest vs single tree? | Similar (RF doesn't reduce bias significantly) |
| P1118 | Variance of Random Forest vs single tree? | Much lower (averaging decorrelated trees) |
| P1119 | Can Random Forest handle missing values? | Not natively (some implementations can) |
| P1120 | Can XGBoost handle missing values? | Yes! (learns optimal default direction) |

---

# 📝 Practice Set 15: Dimensionality Reduction — Advanced (P1121 – P1200)

**P1121.** PCA Step-by-Step Algorithm:
1. Center data: $X \leftarrow X - \bar{X}$
2. Compute covariance matrix: $\Sigma = \frac{1}{n-1}X^TX$
3. Find eigenvalues and eigenvectors of $\Sigma$
4. Sort by eigenvalue (descending)
5. Select top-$k$ eigenvectors → projection matrix $W$ ($d \times k$)
6. Project: $Z = XW$ ($n \times k$)

**P1122.** LDA vs PCA:

| Feature | PCA | LDA |
|---|---|---|
| Type | Unsupervised | Supervised |
| Goal | Max variance | Max class separability |
| Matrix used | Covariance $\Sigma$ | $S_W^{-1}S_B$ |
| Max components | $d$ | $C-1$ (classes − 1) |
| Assumption | Linear variance structure | Normal distribution per class |
| Works when | No labels available | Labels available |

**P1123.** LDA formulation:
- Between-class scatter: $S_B = \sum_{k=1}^{C}n_k(\mu_k - \mu)(\mu_k - \mu)^T$
- Within-class scatter: $S_W = \sum_{k=1}^{C}\sum_{x \in C_k}(x - \mu_k)(x - \mu_k)^T$
- Objective: Maximize $J(w) = \frac{w^TS_Bw}{w^TS_Ww}$ (Fisher criterion)
- Solution: Eigenvectors of $S_W^{-1}S_B$

**P1124.** Fisher LDA for 2 classes:
$$w^* = S_W^{-1}(\mu_1 - \mu_2)$$

**P1125.** Worked LDA example:
- Class 1: (1,2), (2,3), (3,3) → $\mu_1 = (2, 2.67)$
- Class 2: (5,5), (6,5), (7,6) → $\mu_2 = (6, 5.33)$
- $\mu_1 - \mu_2 = (-4, -2.67)$
- Compute $S_W$ and apply formula for best projection direction

**P1126.** t-SNE (for visualization, not GATE core but good to know):
- Nonlinear dimensionality reduction
- Preserves local structure (neighbors stay neighbors)
- Cannot project new data (no learned transformation)
- Used for 2D/3D visualization only

**P1127-P1160** — Dimensionality Reduction drill:

| # | Question | Answer |
|---|---|---|
| P1127 | PCA preserves? | Global variance structure |
| P1128 | LDA preserves? | Class discriminability |
| P1129 | If 3 classes, max LDA components? | 2 ($C-1$) |
| P1130 | PCA eigenvalues all equal → what? | All directions equally important |
| P1131 | PCA first eigenvalue >> others → what? | Data mostly 1-dimensional |
| P1132 | Sum of all eigenvalues = ? | Total variance of data |
| P1133 | PCA on standardized vs raw data? | Different results! Standardize if scales differ |
| P1134 | When NOT to use PCA? | When features have different units (standardize first!) |
| P1135 | PCA reconstruction error for $k$ components? | $\sum_{i=k+1}^{d}\lambda_i$ |
| P1136 | Scree plot shows? | Eigenvalues vs component number |
| P1137 | Elbow in scree plot → ? | Choose $k$ at the elbow |
| P1138 | 95% variance explained means? | $\frac{\sum_{i=1}^{k}\lambda_i}{\sum_{i=1}^{d}\lambda_i} \geq 0.95$ |
| P1139 | PCA loadings are? | Eigenvector components (feature contributions) |
| P1140 | Kernel PCA uses? | Kernel trick for nonlinear PCA |
| P1141 | Eigenvalue = 0 means? | Feature is redundant (linear combination of others) |
| P1142 | Eigenvalues: [10, 5, 3, 1, 0.5, 0.5]. $k$ for 90%? | $\frac{10+5+3}{20} = 90\%$ → $k=3$ |
| P1143 | Covariance matrix size for $d$ features? | $d \times d$ |
| P1144 | Time complexity of eigendecomposition? | $O(d^3)$ |
| P1145 | PCA alternative for large $d$? | Randomized PCA, Incremental PCA |
| P1146 | PCA on $n < d$ case? | At most $n-1$ nonzero eigenvalues |
| P1147 | PCA directions are? | Orthogonal (perpendicular) |
| P1148 | PCA is sensitive to? | Scale! (standardize first) |
| P1149 | PCA is invariant to? | Rotation of coordinate system |
| P1150 | Feature selection vs feature extraction? | Selection: choose subset. Extraction: create new features (PCA) |
| P1151 | Filter method for feature selection? | Correlation, mutual information (independent of model) |
| P1152 | Wrapper method? | Train model with subsets (computationally expensive) |
| P1153 | Embedded method? | L1 regularization (LASSO) — built into training |
| P1154 | LASSO for feature selection because? | Drives coefficients to exactly zero |
| P1155 | L1 vs L2 for feature selection? | L1 (LASSO) → sparse. L2 (Ridge) → small but nonzero |
| P1156 | Curse of dimensionality: as $d$ increases? | Data becomes sparse, distances become similar |
| P1157 | Volume of unit hypersphere as $d \to \infty$? | $\to 0$! Most volume in corners |
| P1158 | Nearest neighbor distance ratio as $d$ increases? | $\frac{d_{max} - d_{min}}{d_{min}} \to 0$ (all points equidistant!) |
| P1159 | Rule of thumb: samples needed? | At least $10d$ to $20d$ samples for $d$ features |
| P1160 | Best solution for high $d$, low $n$? | Regularization + dimensionality reduction |

**P1161-P1200** — SVD and PCA Connection:

**P1161.** SVD: $X = U\Sigma V^T$

Where:
- $U$ ($n \times n$): left singular vectors
- $\Sigma$ ($n \times d$): singular values (diagonal)
- $V$ ($d \times d$): right singular vectors

**P1162.** Connection to PCA:
- Covariance: $\frac{1}{n-1}X^TX = V\frac{\Sigma^2}{n-1}V^T$
- PCA eigenvectors = columns of $V$ (right singular vectors!)
- PCA eigenvalues = $\frac{\sigma_i^2}{n-1}$ where $\sigma_i$ are singular values

**P1163.** Why use SVD for PCA?
- Numerically more stable than eigendecomposition of $X^TX$
- Works for $n < d$ (tall-skinny or short-fat matrices)
- Most PCA implementations use SVD internally

**P1164-P1200** — Quick Review:

| # | Question | Answer |
|---|---|---|
| P1164 | SVD always exists? | Yes! (for any matrix) |
| P1165 | Eigendecomposition requires? | Square matrix |
| P1166 | Truncated SVD keeps? | Top-$k$ singular values |
| P1167 | Low-rank approximation by SVD? | Best rank-$k$ approximation (Eckart-Young theorem) |
| P1168 | PCA = Truncated SVD of? | Centered data matrix |
| P1169 | Singular values are always? | Non-negative |
| P1170 | Rank of matrix = ? | Number of nonzero singular values |
| P1171 | Left singular vectors capture? | Row space structure |
| P1172 | Right singular vectors capture? | Column space structure (features) |
| P1173 | Latent Semantic Analysis (LSA) uses? | Truncated SVD on term-document matrix |
| P1174 | Matrix completion (recommender systems) uses? | Low-rank matrix factorization (related to SVD) |
| P1175 | Autoencoders for dim reduction? | Nonlinear PCA (neural network) |
| P1176 | Linear autoencoder = ? | PCA (mathematically equivalent) |
| P1177 | Variational Autoencoder (VAE)? | Probabilistic generative model + dim reduction |
| P1178 | PCA is optimal for? | Gaussian data (maximizes variance = maximizes info) |
| P1179 | ICA (Independent Component Analysis)? | Find statistically independent components (not just uncorrelated) |
| P1180 | ICA vs PCA? | ICA: independent. PCA: uncorrelated. Independent ⊂ Uncorrelated |
| P1181 | Factor Analysis vs PCA? | FA: models latent factors + noise. PCA: no noise model |
| P1182 | MDS (Multidimensional Scaling)? | Preserves pairwise distances in lower dimension |
| P1183 | Isomap? | MDS on geodesic distances (manifold learning) |
| P1184 | Locally Linear Embedding (LLE)? | Preserves local linear structure |
| P1185 | UMAP advantages over t-SNE? | Faster, preserves global structure better |
| P1186 | Manifold hypothesis? | High-dimensional data lies on low-dimensional manifold |
| P1187 | When PCA fails? | Nonlinear structure, non-Gaussian data |
| P1188 | Whitening in PCA? | Transform so each PC has unit variance |
| P1189 | ZCA whitening? | Whiten then rotate back to original space |
| P1190 | PCA on images? | Eigenfaces (each eigenface captures a pattern) |
| P1191 | How many PCs for 99% variance in images? | Usually much fewer than pixel count |
| P1192 | PCA for noise reduction? | Keep top PCs, discard noisy low-variance PCs |
| P1193 | PCA + Classification pipeline? | PCA → reduce dims → train classifier |
| P1194 | Should PCA be fit on test data? | NO! Fit on train, transform test |
| P1195 | Data leakage with PCA? | If PCA fitted on all data including test → leakage! |
| P1196 | Pipeline prevents leakage by? | Applying transformations only within CV folds |
| P1197 | Sparse PCA? | Adds L1 penalty → sparse loadings → interpretable |
| P1198 | Incremental PCA? | For large datasets that don't fit in memory |
| P1199 | Mini-batch PCA? | Even more scalable version |
| P1200 | PCA interview question: "Why center data?" | So that covariance matrix accurately captures variance around mean |

---

# 📝 Practice Set 16: Model Evaluation Advanced (P1201 – P1300)

## Confusion Matrix Deep Dive

**P1201.** Complete confusion matrix for 3-class problem (Cat/Dog/Bird):

|  | Pred Cat | Pred Dog | Pred Bird |
|---|---|---|---|
| **Actual Cat** | 45 | 3 | 2 |
| **Actual Dog** | 5 | 40 | 5 |
| **Actual Bird** | 1 | 4 | 45 |

**P1202.** Per-class metrics:
- Cat: Precision = $\frac{45}{45+5+1} = 0.882$, Recall = $\frac{45}{45+3+2} = 0.900$
- Dog: Precision = $\frac{40}{3+40+4} = 0.851$, Recall = $\frac{40}{5+40+5} = 0.800$
- Bird: Precision = $\frac{45}{2+5+45} = 0.865$, Recall = $\frac{45}{1+4+45} = 0.900$

**P1203.** Macro-average F1:
$$F1_{\text{macro}} = \frac{F1_{\text{cat}} + F1_{\text{dog}} + F1_{\text{bird}}}{3}$$
$$= \frac{0.891 + 0.825 + 0.882}{3} = 0.866$$

**P1204.** Weighted-average F1 (weights by class support):
- Cat: 50 samples, Dog: 50 samples, Bird: 50 samples (balanced → same as macro)

**P1205.** When to use which averaging?
- Macro: All classes equally important (class imbalance exists)
- Weighted: Account for class sizes
- Micro: Overall accuracy (for multi-label)

## ROC & AUC Worked Example

**P1206.** Given predictions (sorted by probability):

| Sample | True Label | Predicted Prob | 
|---|---|---|
| 1 | + | 0.95 |
| 2 | + | 0.88 |
| 3 | - | 0.80 |
| 4 | + | 0.70 |
| 5 | - | 0.55 |
| 6 | + | 0.45 |
| 7 | - | 0.35 |
| 8 | - | 0.20 |

Total: 4 positive, 4 negative.

**P1207.** Construct ROC curve (threshold from high to low):

| Threshold | TP | FP | TPR | FPR |
|---|---|---|---|---|
| 1.0 | 0 | 0 | 0.00 | 0.00 |
| 0.95 | 1 | 0 | 0.25 | 0.00 |
| 0.88 | 2 | 0 | 0.50 | 0.00 |
| 0.80 | 2 | 1 | 0.50 | 0.25 |
| 0.70 | 3 | 1 | 0.75 | 0.25 |
| 0.55 | 3 | 2 | 0.75 | 0.50 |
| 0.45 | 4 | 2 | 1.00 | 0.50 |
| 0.35 | 4 | 3 | 1.00 | 0.75 |
| 0.20 | 4 | 4 | 1.00 | 1.00 |

**P1208.** AUC = area under the ROC curve:
Using trapezoidal rule on points:
$(0,0) → (0,0.25) → (0,0.50) → (0.25,0.50) → (0.25,0.75) → (0.50,0.75) → (0.50,1.0) → (0.75,1.0) → (1.0,1.0)$

$AUC = 0 + 0 + 0.25 \times 0.50 + 0 + 0.25 \times 0.75 + 0 + 0.25 \times 1.0 + 0.25 \times 1.0 = 0.125 + 0.1875 + 0.25 + 0.25 = 0.8125$

> 🎯 **AUC = 0.8125** → Good classifier!

**P1209.** AUC interpretation:

| AUC | Interpretation |
|---|---|
| 1.0 | Perfect classifier |
| 0.9-1.0 | Excellent |
| 0.8-0.9 | Good |
| 0.7-0.8 | Fair |
| 0.5-0.7 | Poor |
| 0.5 | Random (diagonal line) |
| < 0.5 | Worse than random (flip predictions!) |

**P1210.** AUC = probability that random positive ranked higher than random negative.

**P1211-P1260** — Evaluation metrics rapid fire:

| # | Metric | Formula | When to Use |
|---|---|---|---|
| P1211 | Accuracy | $\frac{TP+TN}{TP+TN+FP+FN}$ | Balanced classes |
| P1212 | Precision | $\frac{TP}{TP+FP}$ | Cost of false positive high (spam marking) |
| P1213 | Recall | $\frac{TP}{TP+FN}$ | Cost of false negative high (cancer detection!) |
| P1214 | F1 Score | $\frac{2 \cdot P \cdot R}{P + R}$ | Balance precision & recall |
| P1215 | F-beta | $\frac{(1+\beta^2) \cdot P \cdot R}{\beta^2 \cdot P + R}$ | Custom P-R tradeoff |
| P1216 | F2 Score | $\beta = 2$: weighs recall 2× | When recall matters more |
| P1217 | F0.5 Score | $\beta = 0.5$: weighs precision 2× | When precision matters more |
| P1218 | Specificity | $\frac{TN}{TN+FP}$ | True negative rate |
| P1219 | FPR | $\frac{FP}{FP+TN} = 1 - \text{Specificity}$ | ROC x-axis |
| P1220 | MCC | $\frac{TP \cdot TN - FP \cdot FN}{\sqrt{(TP+FP)(TP+FN)(TN+FP)(TN+FN)}}$ | Best single metric for binary (even imbalanced) |
| P1221 | Cohen's Kappa | $\kappa = \frac{p_o - p_e}{1 - p_e}$ | Agreement adjusted for chance |
| P1222 | Log Loss | $-\frac{1}{n}\sum[y\log\hat{p} + (1-y)\log(1-\hat{p})]$ | Probabilistic evaluation |
| P1223 | Brier Score | $\frac{1}{n}\sum(y - \hat{p})^2$ | Calibration measure |
| P1224 | MAE | $\frac{1}{n}\sum|y_i - \hat{y}_i|$ | Regression, robust to outliers |
| P1225 | MSE | $\frac{1}{n}\sum(y_i - \hat{y}_i)^2$ | Regression, penalizes large errors |
| P1226 | RMSE | $\sqrt{MSE}$ | Same units as target |
| P1227 | $R^2$ | $1 - \frac{SS_{res}}{SS_{tot}}$ | Proportion of variance explained |
| P1228 | Adjusted $R^2$ | $1 - \frac{(1-R^2)(n-1)}{n-p-1}$ | Penalizes extra features |
| P1229 | MAPE | $\frac{1}{n}\sum\|\frac{y_i - \hat{y}_i}{y_i}\| \times 100$ | Percentage error |
| P1230 | Silhouette Score (clustering) | $\frac{b-a}{\max(a,b)}$ | Cluster quality [-1, 1] |

**P1231-P1260** — Scenario-based evaluation:

| # | Scenario | Best Metric | Why |
|---|---|---|---|
| P1231 | Cancer screening | Recall | Missing positive is life-threatening! |
| P1232 | Spam detection | Precision | Don't want to lose important emails |
| P1233 | Fraud detection in banking | Recall + Precision-Recall curve | Imbalanced + costly false negatives |
| P1234 | Self-driving car: pedestrian detection | Recall (must be ~1.0) | Missing a pedestrian = fatal |
| P1235 | Movie recommendation | Precision | Bad recommendations annoy users |
| P1236 | Predicting house prices | RMSE or MAE | Regression problem |
| P1237 | 99% negative class | AUC or MCC | Accuracy misleading (99% by predicting all negative!) |
| P1238 | Comparing two models overall | AUC | Threshold-independent |
| P1239 | Probability calibration quality | Brier Score | Measures calibration |
| P1240 | Multi-class balanced dataset | Macro F1 | Treats all classes equally |
| P1241 | Multi-class imbalanced | Weighted F1 or Macro F1 | Either works |
| P1242 | Clustering: number of clusters? | Elbow method (SSE) or Silhouette | Compare different $k$ |
| P1243 | Regression with outliers | MAE | Robust (MSE overweights outliers) |
| P1244 | Regression: compare models with different #features | Adjusted $R^2$ | Penalizes model complexity |
| P1245 | Need threshold-free comparison | AUC-ROC | Independent of classification threshold |
| P1246 | Imbalanced + varying thresholds | Precision-Recall curve (AUC-PR) | Better than ROC for imbalanced |
| P1247 | Multiclass + confusion analysis | Per-class confusion matrix | Identify which classes confused |
| P1248 | Model comparison: paired test | McNemar's test | Tests if models disagree systematically |
| P1249 | Cross-validation stability | Mean ± std of CV scores | Measures generalization |
| P1250 | Is model significantly better? | Paired t-test on CV folds or Wilcoxon | Statistical significance |
| P1251 | Overfitting detection: train acc 99%, test acc 60% | Large train-test gap | High variance → regularize |
| P1252 | Underfitting detection: train acc 60%, test acc 58% | Both low, small gap | High bias → more complex model |
| P1253 | Good fit: train acc 92%, test acc 89% | Small gap, both high | Good generalization ✅ |
| P1254 | Learning curve: train error high, test error high | High bias | Need more complex model or features |
| P1255 | Learning curve: train error low, test error high | High variance | Need more data or regularization |
| P1256 | Learning curve: both converge to low error | Good fit | ✅ |
| P1257 | Validation error decreasing, then increasing | Overfitting starting | Stop training (early stopping) |
| P1258 | Adding features: train improves, test worsens | Curse of dimensionality / overfitting | Feature selection needed |
| P1259 | Adding data: train worsens, test improves | Reducing variance | Keep collecting data! |
| P1260 | $k$-fold CV for small dataset? | LOOCV ($k=n$): least bias, high variance | If $n$ very small |

## Cross-Validation Complete Guide

**P1261.** Types of Cross-Validation:

| Type | How | When |
|---|---|---|
| Hold-out (70/30) | Single split | Large dataset, quick |
| $k$-fold CV | $k$ rotations | Standard (usually $k$=5 or 10) |
| Stratified $k$-fold | Preserves class ratios | Imbalanced data |
| LOOCV ($k=n$) | Leave one out | Very small dataset |
| Repeated $k$-fold | Multiple $k$-fold rounds | More stable estimate |
| Group $k$-fold | Groups don't leak between folds | Grouped data (e.g., patients) |
| Time series split | Only forward splits | Time-dependent data |
| Nested CV | Inner CV for tuning, outer for evaluation | When tuning + evaluating |

**P1262.** Nested CV explained:
```
Outer loop (5-fold): Estimates generalization error
├── Fold 1: Train on 80%, Test on 20%
│   └── Inner loop (5-fold on the 80%):
│       └── Grid search for best hyperparameters
│       └── Train final model with best params on full 80%
│       └── Evaluate on held-out 20%
├── Fold 2: ...
└── ...
Final: Average test scores across outer folds
```

**P1263-P1300** — CV & Evaluation scenarios:

| # | Question | Answer |
|---|---|---|
| P1263 | Why not use test set for hyperparameter tuning? | Biases toward test set → overoptimistic |
| P1264 | 3-way split: train/validation/test. Purpose of each? | Train: fit. Val: tune. Test: final evaluation |
| P1265 | $k$-fold vs hold-out: which has lower variance? | $k$-fold (averages over multiple splits) |
| P1266 | $k$-fold vs LOOCV: which has lower bias? | LOOCV (trains on $n-1$ samples, closest to full data) |
| P1267 | $k$-fold vs LOOCV: which has lower variance? | $k$-fold (LOOCV folds are very correlated → high variance) |
| P1268 | Ideal $k$ for most problems? | 5 or 10 (good bias-variance tradeoff) |
| P1269 | Time series: can we shuffle? | NO! Must respect temporal order |
| P1270 | Data leakage from scaling before split? | Yes! Test info leaks via mean/std |
| P1271 | Correct pipeline order? | Split → Scale → Train → Predict → Evaluate |
| P1272 | Using Pipeline in sklearn prevents? | Data leakage (transforms fit only on train fold) |
| P1273 | GridSearchCV uses what internally? | Cross-validation for each parameter combination |
| P1274 | RandomizedSearchCV advantage? | Faster for large search spaces; finds good params quicker |
| P1275 | Bayesian optimization for hyperparameters? | Models objective function, efficient search |
| P1276 | If model performs well on CV but poorly in production? | Distribution shift, data leakage, or non-representative CV |
| P1277 | Stratified split for regression? | Split by quantiles of target variable |
| P1278 | Bootstrapping for CI? | Resample with replacement, compute statistic many times |
| P1279 | Permutation test for feature importance? | Shuffle one feature, measure performance drop |
| P1280 | Feature importance from tree-based models? | Based on split criteria reduction |
| P1281 | SHAP values? | Shapley values from game theory → feature contributions |
| P1282 | LIME? | Local Interpretable Model-agnostic Explanations |
| P1283 | Partial dependence plot? | Marginal effect of a feature |
| P1284 | Calibration curve? | Plots predicted probability vs actual frequency |
| P1285 | Well-calibrated model? | Predicted 80% → actually correct 80% of the time |
| P1286 | Platt scaling? | Logistic regression on model outputs for calibration |
| P1287 | Isotonic regression for calibration? | Non-parametric, more flexible than Platt |
| P1288 | Train error always ≤ test error? | Generally yes (model optimized for train data) |
| P1289 | Exception: when train error > test error? | Small test set, lucky test split, regularization |
| P1290 | Perfect CV score = perfect model? | No! Could be overfitting to CV, or leakage |
| P1291 | Statistical test for comparing models? | Paired t-test, Wilcoxon, McNemar's |
| P1292 | Effect size vs p-value? | p-value: significant? Effect size: how much? |
| P1293 | Type I error in model comparison? | Falsely concluding models differ |
| P1294 | Type II error in model comparison? | Missing real difference between models |
| P1295 | Bonferroni correction when? | Comparing multiple models (multiple testing) |
| P1296 | Information criterion alternatives: AIC, BIC? | AIC: $2k - 2\ln L$. BIC: $k\ln n - 2\ln L$ |
| P1297 | AIC vs BIC? | BIC penalizes complexity more for large $n$ |
| P1298 | Lower AIC/BIC = ? | Better model |
| P1299 | Test-time augmentation? | Average predictions on augmented test inputs |
| P1300 | Ensemble of CV folds? | Train model on each fold, average predictions |

---

# 📝 Practice Set 17: GATE-Level Numerical Problems (P1301 – P1400)

**P1301.** Linear Regression: Points (1,2), (2,4), (3,5), (4,4), (5,5). Find $w_1, w_0$ and predict $y$ for $x=6$.

$n=5$, $\sum x = 15$, $\sum y = 20$, $\sum xy = 67$, $\sum x^2 = 55$

$w_1 = \frac{n\sum xy - \sum x \sum y}{n\sum x^2 - (\sum x)^2} = \frac{5(67) - 15(20)}{5(55) - 225} = \frac{335-300}{275-225} = \frac{35}{50} = 0.7$

$w_0 = \bar{y} - w_1\bar{x} = 4 - 0.7(3) = 4 - 2.1 = 1.9$

$\hat{y}(6) = 1.9 + 0.7(6) = 6.1$

$R^2$: $SS_{tot} = \sum(y_i - \bar{y})^2 = (2-4)^2 + (4-4)^2 + (5-4)^2 + (4-4)^2 + (5-4)^2 = 4+0+1+0+1 = 6$

Predictions: 2.6, 3.3, 4.0, 4.7, 5.4. $SS_{res} = (2-2.6)^2 + (4-3.3)^2 + (5-4)^2 + (4-4.7)^2 + (5-5.4)^2 = 0.36+0.49+1+0.49+0.16 = 2.5$

$R^2 = 1 - \frac{2.5}{6} = 0.583$

**P1302.** Logistic Regression: If $w = [0.5, -0.3]$, $b = 0.1$, what is $P(y=1)$ for $x = [2, 1]$?

$z = 0.5(2) + (-0.3)(1) + 0.1 = 1.0 + (-0.3) + 0.1 = 0.8$

$\hat{p} = \sigma(0.8) = \frac{1}{1+e^{-0.8}} = \frac{1}{1+0.449} = \frac{1}{1.449} = 0.690$

Predict class 1 (> 0.5). **Answer: P(y=1) = 0.690**

**P1303.** Naive Bayes: Given:
- $P(\text{spam}) = 0.3$, $P(\text{ham}) = 0.7$
- $P(\text{"free"}|\text{spam}) = 0.8$, $P(\text{"free"}|\text{ham}) = 0.1$
- $P(\text{"money"}|\text{spam}) = 0.6$, $P(\text{"money"}|\text{ham}) = 0.05$

Classify email: "free money"

$P(\text{spam}|\text{"free money"}) \propto 0.3 \times 0.8 \times 0.6 = 0.144$
$P(\text{ham}|\text{"free money"}) \propto 0.7 \times 0.1 \times 0.05 = 0.0035$

Normalize: $P(\text{spam}) = \frac{0.144}{0.144+0.0035} = \frac{0.144}{0.1475} = 0.976$

**Answer: P(spam) = 97.6%** → Classify as spam ✅

**P1304.** SVM: Given $w = [2, 3]$, what is the margin?

$\|w\| = \sqrt{4+9} = \sqrt{13}$

Margin $= \frac{2}{\sqrt{13}} = \frac{2}{3.606} = 0.555$

**P1305.** KNN: Points and classes:
| Point | x1 | x2 | Class |
|---|---|---|---|
| A | 1 | 1 | + |
| B | 2 | 2 | + |
| C | 4 | 4 | - |
| D | 5 | 5 | - |

Query: $(3, 3)$, $k=3$.

Distances: $d(A) = \sqrt{8} = 2.83$, $d(B) = \sqrt{2} = 1.41$, $d(C) = \sqrt{2} = 1.41$, $d(D) = \sqrt{8} = 2.83$

3 nearest: B(+), C(-), and tie between A(+) and D(-). 

If break tie by closest: A(+) chosen. 3-NN: B(+), C(-), A(+) → **Predict +**

**P1306.** PCA: Eigenvalues = [8, 4, 2, 1, 0.5].
- Total variance: 15.5
- Variance by top-2: $\frac{8+4}{15.5} = \frac{12}{15.5} = 77.4\%$
- Variance by top-3: $\frac{8+4+2}{15.5} = \frac{14}{15.5} = 90.3\%$
- Need 3 components for 90%+ ✅

**P1307-P1350** — Quick numerical answers:

| # | Problem | Answer |
|---|---|---|
| P1307 | Entropy of [50%, 50%]? | 1.0 bit |
| P1308 | Entropy of [100%, 0%]? | 0 bits (pure!) |
| P1309 | Entropy of [75%, 25%]? | $-0.75\log_2 0.75 - 0.25\log_2 0.25 = 0.811$ bits |
| P1310 | Gini of [50%, 50%]? | $1 - 0.25 - 0.25 = 0.5$ |
| P1311 | Gini of [100%, 0%]? | 0 |
| P1312 | Gini of [75%, 25%]? | $1 - 0.5625 - 0.0625 = 0.375$ |
| P1313 | MAE of predictions [2,4,6] vs actual [3,3,7]? | $\frac{1+1+1}{3} = 1.0$ |
| P1314 | MSE of same? | $\frac{1+1+1}{3} = 1.0$ |
| P1315 | RMSE of same? | $\sqrt{1.0} = 1.0$ |
| P1316 | TP=90, FP=10, FN=30, TN=70. Accuracy? | $\frac{160}{200} = 80\%$ |
| P1317 | Same. Precision? | $\frac{90}{100} = 90\%$ |
| P1318 | Same. Recall? | $\frac{90}{120} = 75\%$ |
| P1319 | Same. F1? | $\frac{2 \times 0.9 \times 0.75}{1.65} = 0.818$ |
| P1320 | Same. Specificity? | $\frac{70}{80} = 87.5\%$ |
| P1321 | Ridge with $\lambda=0$? | Ordinary Least Squares |
| P1322 | Ridge with $\lambda \to \infty$? | All weights → 0 (except bias) |
| P1323 | LASSO with $\lambda \to \infty$? | All weights = exactly 0 |
| P1324 | Sigmoid(0)? | 0.5 |
| P1325 | Sigmoid(Large positive)? | ≈ 1 |
| P1326 | Sigmoid(Large negative)? | ≈ 0 |
| P1327 | Sigmoid'(0)? | $0.5 \times 0.5 = 0.25$ (maximum gradient) |
| P1328 | Information Gain = 0 for a feature? | Feature provides no useful split |
| P1329 | All K-Means points in one cluster? | Restart with different initialization! |
| P1330 | K-Means guaranteed to converge? | Yes (to local minimum) |
| P1331 | K-Means guaranteed global optimum? | No! Run multiple times |
| P1332 | K-Means sensitive to outliers? | Yes! (uses mean) |
| P1333 | K-Medoids advantage? | Uses actual data points as centers (robust to outliers) |
| P1334 | K-Means: SSE always decreases with more $k$? | Yes (at $k=n$, SSE=0) — but overfitting! |
| P1335 | Elbow method: plot what vs what? | SSE vs $k$ |
| P1336 | Silhouette score range? | [-1, 1] |
| P1337 | Silhouette = 1 means? | Point is perfectly clustered |
| P1338 | Silhouette = 0 means? | Point is on boundary between clusters |
| P1339 | Silhouette = -1 means? | Point is in wrong cluster |
| P1340 | Davies-Bouldin: lower = ? | Better clustering |
| P1341 | Calinski-Harabasz: higher = ? | Better clustering |
| P1342 | Adjusted Rand Index range? | [-1, 1] (1 = perfect, 0 = random) |
| P1343 | NMI range? | [0, 1] |
| P1344 | Perplexity in t-SNE? | Related to effective number of neighbors |
| P1345 | Higher perplexity in t-SNE? | More global structure preserved |
| P1346 | Learning rate too high in GD? | Diverges (overshoots) |
| P1347 | Learning rate too low in GD? | Very slow convergence |
| P1348 | Mini-batch size effect? | Small: noisy/fast. Large: smooth/slow per epoch |
| P1349 | Adam optimizer combines? | Momentum + RMSProp |
| P1350 | Batch norm purpose? | Normalize layer inputs → faster training, regularization |

---

# 📋 Complete ML Formulas Cheat Sheet (Exam-Ready)

## Regression
| Formula | Expression |
|---|---|
| Simple LR slope | $w_1 = \frac{n\sum x_iy_i - \sum x_i\sum y_i}{n\sum x_i^2 - (\sum x_i)^2}$ |
| Simple LR intercept | $w_0 = \bar{y} - w_1\bar{x}$ |
| Normal Equation | $w = (X^TX)^{-1}X^Ty$ |
| Ridge | $w = (X^TX + \lambda I)^{-1}X^Ty$ |
| MSE | $\frac{1}{n}\sum(y_i - \hat{y}_i)^2$ |
| $R^2$ | $1 - \frac{SS_{res}}{SS_{tot}}$ |
| Adj $R^2$ | $1 - \frac{(1-R^2)(n-1)}{n-p-1}$ |

## Classification
| Formula | Expression |
|---|---|
| Sigmoid | $\sigma(z) = \frac{1}{1+e^{-z}}$ |
| Sigmoid derivative | $\sigma'(z) = \sigma(z)(1-\sigma(z))$ |
| BCE Loss | $-\frac{1}{n}\sum[y\log\hat{p} + (1-y)\log(1-\hat{p})]$ |
| Softmax | $\hat{p}_k = \frac{e^{z_k}}{\sum_j e^{z_j}}$ |
| SVM Margin | $\frac{2}{\|w\|}$ |
| Precision | $\frac{TP}{TP+FP}$ |
| Recall | $\frac{TP}{TP+FN}$ |
| F1 | $\frac{2PR}{P+R}$ |
| Accuracy | $\frac{TP+TN}{TP+TN+FP+FN}$ |

## Trees
| Formula | Expression |
|---|---|
| Entropy | $H = -\sum p_k \log_2 p_k$ |
| Gini | $G = 1 - \sum p_k^2$ |
| Information Gain | $IG = H(\text{parent}) - \sum\frac{|S_v|}{|S|}H(S_v)$ |

## PCA
| Formula | Expression |
|---|---|
| Covariance matrix | $\Sigma = \frac{1}{n-1}X^TX$ (centered) |
| Eigendecomposition | $\Sigma u = \lambda u$ |
| Variance explained | $\frac{\sum_{i=1}^{k}\lambda_i}{\sum_{i=1}^{d}\lambda_i}$ |

## Clustering
| Formula | Expression |
|---|---|
| K-Means objective | $\min\sum_{i=1}^{k}\sum_{x\in C_i}\|x-\mu_i\|^2$ |
| Silhouette | $\frac{b(i)-a(i)}{\max(a(i),b(i))}$ |

## Regularization
| Type | Penalty |
|---|---|
| L1 (LASSO) | $\lambda\sum|w_j|$ |
| L2 (Ridge) | $\lambda\sum w_j^2$ |
| Elastic Net | $\lambda_1\sum|w_j| + \lambda_2\sum w_j^2$ |

## Probability
| Formula | Expression |
|---|---|
| Bayes' theorem | $P(A|B) = \frac{P(B|A)P(A)}{P(B)}$ |
| Naive Bayes | $\hat{y} = \arg\max_k P(C_k)\prod_j P(x_j|C_k)$ |
| MAP estimate | $\hat{\theta} = \arg\max_\theta P(\theta|D) = \arg\max_\theta P(D|\theta)P(\theta)$ |
| MLE | $\hat{\theta} = \arg\max_\theta P(D|\theta)$ |

---

# 📋 Complete ML Error Analysis Decision Table

| Symptom | Diagnosis | Solution |
|---|---|---|
| Train ↑ Test ↑ (both high error) | High Bias (Underfitting) | More complex model, more features, less regularization |
| Train ↓ Test ↑ (gap increasing) | High Variance (Overfitting) | More data, regularization, simpler model, dropout |
| Train ↓ Test ↓ (gap small) | Good Fit | ✅ Keep current approach |
| Train ↓↓ Test ≈ (test plateaus) | Capacity limit | Try different model family |
| Train ↓ Test oscillates | Unstable model | Reduce learning rate, increase batch size |
| All metrics bad | Bad features | Feature engineering, domain knowledge |
| One class good, others bad | Class imbalance | Resampling, class weights, different metric |

---

# 📝 Practice Set 18: Bias-Variance & Regularization Mastery (P1351 – P1450)

**P1351.** Bias-Variance Tradeoff Visualization:

| Model Complexity | Bias | Variance | Total Error | Status |
|---|---|---|---|---|
| Very Low (e.g., line for curve) | Very High | Very Low | High | Underfitting |
| Low | High | Low | Moderate | Slightly underfit |
| Optimal | Medium | Medium | **Minimum** | ✅ Best |
| High | Low | High | Moderate | Slightly overfit |
| Very High (e.g., poly degree=n) | Very Low | Very High | High | Overfitting |

> 🎯 **GATE Key:** The sweet spot is where Bias² + Variance is minimized. This is NOT where either is minimum individually!

**P1352.** How does training set size affect bias and variance?

| More Data | Effect on Bias | Effect on Variance |
|---|---|---|
| For complex model | Unchanged | Decreases ↓ |
| For simple model | Unchanged | Unchanged |

→ More data helps high-variance models! It CAN'T fix high bias (need more complex model).

**P1353.** Regularization as Bias-Variance control:

| $\lambda$ | Bias | Variance | Model |
|---|---|---|---|
| 0 | Low | High | OLS (no regularization) |
| Small | Slightly higher | Lower | Mild regularization |
| Optimal | Medium | Medium | Best generalization |
| Large | High | Very Low | Heavy regularization |
| $\infty$ | Maximum | Zero | Predicts mean always |

**P1354.** L1 vs L2 Regularization — Geometric Interpretation:

| Property | L1 (LASSO) | L2 (Ridge) |
|---|---|---|
| Constraint shape | Diamond (corners on axes) | Circle |
| Solution tends to | Corners (sparse: $w_j = 0$) | Anywhere on circle (small but nonzero) |
| Feature selection | Yes! | No |
| Unique solution | Not always (can have many) | Always unique |
| Handles correlated features | Picks one, drops others | Shrinks both |
| Computation | Subgradient methods (no closed form) | Closed form: $(X^TX + \lambda I)^{-1}X^Ty$ |
| Bayesian interpretation | Laplace prior | Gaussian prior |

**P1355.** Elastic Net combines both:
$$L = \|y - Xw\|^2 + \lambda_1\sum|w_j| + \lambda_2\sum w_j^2$$

→ Gets sparsity of L1 + stability of L2 for correlated features.

**P1356.** LASSO path: As $\lambda$ increases from 0:
- Least important features → 0 first
- More features → 0 as $\lambda$ grows
- Eventually all features = 0

This creates a natural feature ranking! ✅

**P1357-P1400** — Regularization & Bias-Variance drill:

| # | Question | Answer |
|---|---|---|
| P1357 | Ridge regression is equivalent to MAP with what prior? | Gaussian prior on weights |
| P1358 | LASSO is equivalent to MAP with what prior? | Laplace (double exponential) prior |
| P1359 | MLE with no prior is equivalent to? | OLS (no regularization) |
| P1360 | Regularization is a form of? | Inductive bias |
| P1361 | Other forms of regularization besides L1/L2? | Dropout, early stopping, data augmentation |
| P1362 | Dropout regularization: randomly disables? | Neurons during training (not during testing!) |
| P1363 | Dropout rate of 0.5 means? | 50% of neurons dropped each training step |
| P1364 | At test time with dropout? | Use all neurons, scale by (1 - dropout_rate) |
| P1365 | Early stopping monitors? | Validation error → stop when it starts increasing |
| P1366 | Early stopping is like? | L2 regularization (mathematically similar effect) |
| P1367 | Data augmentation reduces? | Overfitting (effectively increases training set) |
| P1368 | Batch Normalization is a form of? | Regularization (slight noise from batch statistics) |
| P1369 | Weight decay = ? | L2 regularization (in optimizer context) |
| P1370 | $R^2$ can be negative? | Yes! (model worse than predicting mean) |
| P1371 | $R^2 = 0$ means? | Model = predicting mean |
| P1372 | $R^2 = 1$ means? | Perfect fit (or overfitting!) |
| P1373 | Adjusted $R^2$ penalizes? | Adding useless features |
| P1374 | Adjusted $R^2$ can decrease when adding features? | Yes! (if feature doesn't help enough) |
| P1375 | AIC selects model that? | Minimizes $2k - 2\ln L$ |
| P1376 | BIC vs AIC for large $n$? | BIC penalizes complexity more → simpler models |
| P1377 | Cross-validation vs information criteria? | CV: empirical. IC: theoretical. Both valid for selection |
| P1378 | Bagging reduces bias or variance? | Variance (by averaging) |
| P1379 | Boosting reduces bias or variance? | Primarily bias (sequential correction) |
| P1380 | Stacking reduces bias or variance? | Both (if done well) |
| P1381 | KNN with $k=1$: bias/variance? | Zero bias, high variance → overfitting |
| P1382 | KNN with $k=n$: bias/variance? | High bias, zero variance → always predicts majority |
| P1383 | Decision tree (no pruning): bias/variance? | Low bias, high variance → overfitting |
| P1384 | Decision tree (heavy pruning): bias/variance? | Higher bias, lower variance |
| P1385 | Linear model for nonlinear data: bias/variance? | High bias (can't capture nonlinearity) |
| P1386 | Polynomial degree 20 for 30 points: bias/variance? | Very low bias, extremely high variance |
| P1387 | SVM with large $C$: bias/variance? | Low bias, high variance (hard margin) |
| P1388 | SVM with small $C$: bias/variance? | Higher bias, lower variance (soft margin) |
| P1389 | RBF kernel with large $\gamma$: bias/variance? | Low bias, high variance (complex boundary) |
| P1390 | RBF kernel with small $\gamma$: bias/variance? | Higher bias, lower variance (smooth boundary) |
| P1391 | Neural network with many layers: bias/variance? | Low bias, high variance (without regularization) |
| P1392 | Ensemble of diverse models: effect? | Reduces variance (if models are uncorrelated) |
| P1393 | Adding irrelevant features: effect? | Increases variance (curse of dimensionality) |
| P1394 | Feature selection: effect? | Reduces variance |
| P1395 | PCA before training: effect? | Reduces variance (reduces effective dimensionality) |
| P1396 | More training data: always helps? | Only if model has high variance |
| P1397 | More training data for high bias model? | Doesn't help! Need more complex model |
| P1398 | The "no free lunch" theorem states? | No single algorithm is best for all problems |
| P1399 | Occam's razor in ML? | Simpler model preferred (if similar performance) |
| P1400 | Model selection: the right complexity? | Match model complexity to data complexity |

---

# 📝 Practice Set 19: Complete Algorithm Walkthrough Problems (P1401 – P1500)

**P1401.** Complete Gradient Descent Walkthrough:

$f(x) = x^2 + 4x + 4 = (x+2)^2$. Minimum at $x = -2$.

$f'(x) = 2x + 4$. Learning rate $\eta = 0.1$. Start: $x_0 = 5$.

| Iteration | $x$ | $f'(x)$ | $x_{new} = x - \eta f'(x)$ | $f(x)$ |
|---|---|---|---|---|
| 0 | 5.000 | 14.0 | 3.600 | 49.0 |
| 1 | 3.600 | 11.2 | 2.480 | 31.36 |
| 2 | 2.480 | 8.96 | 1.584 | 20.07 |
| 3 | 1.584 | 7.17 | 0.867 | 12.85 |
| 4 | 0.867 | 5.73 | 0.294 | 8.22 |
| ... | ... | ... | → -2.0 | → 0 |

Converges to $x = -2$, $f = 0$ ✅. Each step reduces $f$ monotonically (convex!).

**P1402.** Backpropagation on simple network:

Network: Input $x=1$, weight $w_1=0.5$, hidden $h = \sigma(w_1 \cdot x)$, weight $w_2=0.8$, output $\hat{y} = w_2 \cdot h$, target $y=1$.

Forward pass:
- $z_h = w_1 \cdot x = 0.5$
- $h = \sigma(0.5) = 0.622$
- $\hat{y} = w_2 \cdot h = 0.8 \times 0.622 = 0.498$

Loss: $L = \frac{1}{2}(y - \hat{y})^2 = \frac{1}{2}(1 - 0.498)^2 = \frac{1}{2}(0.502)^2 = 0.126$

Backward pass:
- $\frac{\partial L}{\partial \hat{y}} = -(y - \hat{y}) = -(0.502) = -0.502$
- $\frac{\partial \hat{y}}{\partial w_2} = h = 0.622$
- $\frac{\partial L}{\partial w_2} = -0.502 \times 0.622 = -0.312$
- $\frac{\partial \hat{y}}{\partial h} = w_2 = 0.8$
- $\frac{\partial h}{\partial z_h} = \sigma'(0.5) = 0.622 \times 0.378 = 0.235$
- $\frac{\partial z_h}{\partial w_1} = x = 1$
- $\frac{\partial L}{\partial w_1} = -0.502 \times 0.8 \times 0.235 \times 1 = -0.094$

Update ($\eta = 0.5$):
- $w_2' = 0.8 - 0.5 \times (-0.312) = 0.956$
- $w_1' = 0.5 - 0.5 \times (-0.094) = 0.547$

**P1403.** Decision Tree Construction — Complete:

| Sample | Weather | Temp | Play? |
|---|---|---|---|
| 1 | Sunny | Hot | No |
| 2 | Sunny | Mild | Yes |
| 3 | Rainy | Mild | Yes |
| 4 | Rainy | Cool | No |
| 5 | Cloudy | Hot | Yes |
| 6 | Cloudy | Mild | Yes |

Root entropy: 4 Yes, 2 No → $H = -\frac{4}{6}\log_2\frac{4}{6} - \frac{2}{6}\log_2\frac{2}{6} = 0.918$

Split on Weather:
- Sunny: {1(No), 2(Yes)} → H = 1.0
- Rainy: {3(Yes), 4(No)} → H = 1.0
- Cloudy: {5(Yes), 6(Yes)} → H = 0.0 (pure!)

$IG(\text{Weather}) = 0.918 - \frac{2}{6}(1.0) - \frac{2}{6}(1.0) - \frac{2}{6}(0.0) = 0.918 - 0.667 = 0.251$

Split on Temp:
- Hot: {1(No), 5(Yes)} → H = 1.0
- Mild: {2(Yes), 3(Yes), 6(Yes)} → H = 0.0 (pure!)
- Cool: {4(No)} → H = 0.0 (pure!)

$IG(\text{Temp}) = 0.918 - \frac{2}{6}(1.0) - \frac{3}{6}(0.0) - \frac{1}{6}(0.0) = 0.918 - 0.333 = 0.585$

**Split on Temp first!** (higher IG)

Tree: Root = Temp
- Mild → Yes
- Cool → No
- Hot → split on Weather: Sunny → No, Cloudy → Yes

**P1404-P1450** — Algorithm application scenarios:

| # | Scenario | Best Algorithm | Reason |
|---|---|---|---|
| P1404 | 100 samples, 10 features, linear relationship | Linear Regression | Simple, sufficient data |
| P1405 | 1M samples, 1000 features, classification | Gradient Boosted Trees / XGBoost | Handles large-scale, high accuracy |
| P1406 | Text classification, 50K vocabulary | Naive Bayes or Linear SVM | High-dim, sparse features |
| P1407 | Image classification | CNN (Deep Learning) | Spatial patterns |
| P1408 | Time series prediction | ARIMA or LSTM | Temporal dependencies |
| P1409 | Anomaly detection, no labels | Isolation Forest / LOF | Unsupervised |
| P1410 | Clustering with unknown $k$ | DBSCAN | Auto-determines clusters |
| P1411 | Clustering with known $k$, spherical | K-Means | Fast, assumes spherical |
| P1412 | Feature selection needed | LASSO or Random Forest | Built-in selection |
| P1413 | Interpretable model required | Decision Tree or Linear/Logistic Regression | Transparent decisions |
| P1414 | Small dataset, many features | SVM with RBF / Ridge | Handles high $d$, regularized |
| P1415 | Missing values common | XGBoost or KNN (with distance) | Handles natively or can impute |
| P1416 | Streaming data (online learning) | SGD-based models, Perceptron | Update incrementally |
| P1417 | Need probability estimates | Logistic Regression, Random Forest | Output calibrated probabilities |
| P1418 | Non-linear decision boundary needed | SVM-RBF, Random Forest, Neural Net | Capture nonlinearity |
| P1419 | Predicting house prices | Linear Regression / Gradient Boosting | Regression task |
| P1420 | Spam detection | Naive Bayes / SVM | Classic text classification |
| P1421 | Customer segmentation | K-Means / Hierarchical | Clustering task |
| P1422 | Dimensionality reduction for visualization | t-SNE / UMAP | 2D/3D embeddings |
| P1423 | Dimensionality reduction for preprocessing | PCA | Reduces noise, speeds up training |
| P1424 | Recommender system | Collaborative filtering / Matrix factorization | User-item interaction |
| P1425 | Detect credit card fraud | Ensemble + anomaly detection | Imbalanced + streaming |
| P1426 | Medical diagnosis | Logistic Regression / Random Forest | Interpretable + accurate |
| P1427 | Self-driving car | Deep RL + CNN | Complex sequential decisions |
| P1428 | Language translation | Transformer / Attention | Sequence-to-sequence |
| P1429 | Game playing AI | Reinforcement Learning / MCTS | Trial-and-error |
| P1430 | Drug discovery | Graph Neural Networks | Molecular structure |

**P1431-P1450** — Quick scenario drill:

| # | Question | Answer |
|---|---|---|
| P1431 | Dataset: 50 samples, 500 features. Biggest risk? | Overfitting (curse of dimensionality) |
| P1432 | Fix for P1431? | PCA + regularized model (Ridge/LASSO) |
| P1433 | Train accuracy: 99%, Test accuracy: 55%. Diagnosis? | Severe overfitting |
| P1434 | Fix for P1433? | More data, regularization, simpler model, dropout |
| P1435 | Train accuracy: 50%, Test accuracy: 48%. Diagnosis? | Underfitting |
| P1436 | Fix for P1435? | More complex model, more features, less regularization |
| P1437 | Two models: A (90% acc, 1M params), B (89% acc, 10K params). Prefer? | B (similar accuracy, much simpler → better generalization) |
| P1438 | When to prefer accuracy over F1? | Balanced classes, equal cost of errors |
| P1439 | When irreducible error is high? | Collect better features, reduce noise, accept the floor |
| P1440 | Feature has low correlation with target. Drop it? | Not always! May be useful in combination with others |
| P1441 | Multicollinearity in regression: problem? | Unstable coefficients, inflated standard errors |
| P1442 | VIF > 10 for a feature? | High multicollinearity → consider removing or PCA |
| P1443 | Gradient vanishing in deep networks? | Use ReLU, batch norm, skip connections (ResNet) |
| P1444 | Gradient exploding? | Gradient clipping, proper initialization |
| P1445 | Learning rate schedule: step decay? | Reduce LR by factor every $k$ epochs |
| P1446 | Learning rate schedule: cosine annealing? | LR follows cosine curve from high to low |
| P1447 | Warm restarts? | Reset LR periodically for exploration |
| P1448 | Transfer learning: when? | Small dataset + related pretrained model available |
| P1449 | Fine-tuning vs feature extraction? | Fine-tune: update all weights. Feature extract: freeze base |
| P1450 | Domain adaptation? | Source domain ≠ target domain. Adapt model |

---

# 📝 Practice Set 20: Ultimate GATE Drill (P1451 – P1550)

## Quick Answer — 100 Rapid Fire

| # | Question | Answer |
|---|---|---|
| P1451 | Supervised learning uses? | Labeled data |
| P1452 | Unsupervised learning uses? | Unlabeled data |
| P1453 | Semi-supervised uses? | Mix of labeled + unlabeled |
| P1454 | Reinforcement learning uses? | Rewards/penalties |
| P1455 | Self-supervised learning? | Creates labels from data itself |
| P1456 | Regression predicts? | Continuous value |
| P1457 | Classification predicts? | Discrete class |
| P1458 | Clustering does? | Groups similar data |
| P1459 | Dimensionality reduction does? | Reduces features |
| P1460 | Anomaly detection does? | Finds outliers |
| P1461 | Parametric model? | Fixed number of parameters (doesn't grow with data) |
| P1462 | Non-parametric model? | Parameters grow with data (KNN, kernel SVM) |
| P1463 | Discriminative model? | Models $P(y|x)$ directly (LR, SVM, NN) |
| P1464 | Generative model? | Models $P(x|y)P(y)$ (Naive Bayes, GMM) |
| P1465 | Lazy learner? | No training, stores data (KNN) |
| P1466 | Eager learner? | Trains model upfront (most algorithms) |
| P1467 | Linear model? | Decision boundary is hyperplane |
| P1468 | Nonlinear model? | Decision boundary can be curved |
| P1469 | Convex loss function guarantee? | Global minimum exists |
| P1470 | Non-convex loss (neural nets)? | Local minima (but SGD often finds good ones) |
| P1471 | Epoch = ? | One pass through entire training data |
| P1472 | Batch = ? | Subset of training data used in one update |
| P1473 | Iteration = ? | One parameter update step |
| P1474 | If 1000 samples, batch size 100: iterations per epoch? | 10 |
| P1475 | Hyperparameter vs parameter? | Hyper: set before training. Param: learned during |
| P1476 | Examples of hyperparameters? | Learning rate, $\lambda$, $k$, tree depth, C |
| P1477 | Examples of parameters? | Weights $w$, biases $b$, centroids $\mu$ |
| P1478 | Inductive bias? | Assumptions model makes about the data |
| P1479 | SVM inductive bias? | Maximum margin between classes |
| P1480 | KNN inductive bias? | Similar points have similar labels |
| P1481 | Decision tree inductive bias? | Shorter trees preferred (Occam's razor) |
| P1482 | Feature scaling needed for? | KNN, SVM, PCA, gradient descent, K-Means |
| P1483 | Feature scaling NOT needed for? | Decision trees, Random Forest, Naive Bayes |
| P1484 | Min-Max scaling formula? | $x' = \frac{x - x_{min}}{x_{max} - x_{min}}$ |
| P1485 | Standardization formula? | $x' = \frac{x - \mu}{\sigma}$ |
| P1486 | When Min-Max vs standardization? | Min-Max: bounded range needed. Standard: outliers exist |
| P1487 | One-hot encoding for? | Nominal categorical variables (no order) |
| P1488 | Label encoding for? | Ordinal variables (with natural order) |
| P1489 | Target encoding? | Replace category with mean of target (risk of leakage!) |
| P1490 | Handling missing values: dropping? | Only if very few missing (< 5%) |
| P1491 | Imputation: mean? | For numerical, approximately normal |
| P1492 | Imputation: median? | For numerical, skewed data |
| P1493 | Imputation: mode? | For categorical |
| P1494 | KNN imputation? | Use K nearest neighbors to estimate missing |
| P1495 | MICE imputation? | Multiple chained equations (iterative) |
| P1496 | Handling class imbalance: oversampling? | SMOTE (synthetic minority oversampling) |
| P1497 | Handling class imbalance: undersampling? | Remove some majority class samples |
| P1498 | Handling class imbalance: cost-sensitive? | Increase misclassification cost for minority |
| P1499 | Handling class imbalance: class weights? | `class_weight='balanced'` in sklearn |
| P1500 | Train/Val/Test split typically? | 60/20/20 or 70/15/15 or 80/10/10 |

---

# 📋 Complete ML Glossary for GATE

| Term | Definition |
|---|---|
| **Bias** | Error from simplifying assumptions (underfitting) |
| **Variance** | Error from sensitivity to training data (overfitting) |
| **Bagging** | Bootstrap Aggregating: train on bootstrap samples, average |
| **Boosting** | Sequential ensemble: each learner corrects previous errors |
| **Stacking** | Use predictions of base models as features for meta-model |
| **Cross-entropy** | Loss function measuring divergence between distributions |
| **Decision boundary** | Surface separating different class predictions |
| **Epoch** | One complete pass through training data |
| **Feature engineering** | Creating new features from existing data |
| **Gradient descent** | Optimization by following negative gradient |
| **Heuristic** | Rule of thumb for algorithm design |
| **Hyperparameter** | Setting configured before training begins |
| **Imputation** | Filling in missing values |
| **Kernel trick** | Computing in high-dim space without explicit mapping |
| **Likelihood** | Probability of data given parameters |
| **Margin** | Distance from decision boundary to nearest point |
| **Multicollinearity** | Correlated features causing instability |
| **Normalization** | Scaling features to common range |
| **Overfitting** | Model memorizes training data, fails on new data |
| **Prior** | Belief about parameter before seeing data (Bayesian) |
| **Posterior** | Belief about parameter after seeing data |
| **Regularization** | Penalty on model complexity to prevent overfitting |
| **Support vector** | Training point on or within the margin boundary |
| **Underfitting** | Model too simple to capture data patterns |

---

# 🏅 Final GATE ML Exam Tips

> 📌 **Tip 1:** For regression problems, always check if the question asks for the closed-form solution or gradient descent. Know both!

> 📌 **Tip 2:** Entropy/Gini calculations are the most common numerical GATE ML questions. Practice computing them fast!

> 📌 **Tip 3:** SVM margin = $\frac{2}{\|w\|}$. Many students forget the 2. It's the distance BETWEEN both support planes.

> 📌 **Tip 4:** PCA eigenvalue = variance explained by that component. Questions always ask "how many components for X% variance?"

> 📌 **Tip 5:** Precision vs Recall: Precision = "Of predicted positives, how many are correct?" Recall = "Of actual positives, how many found?"

> 📌 **Tip 6:** K-Means ALWAYS converges but NOT to global optimum. Run multiple times!

> 📌 **Tip 7:** Naive Bayes with zero probability → use Laplace smoothing. This appears in GATE regularly.

> 📌 **Tip 8:** Sigmoid(0) = 0.5. This threshold determines binary classification. Know the sigmoid graph!

> 📌 **Tip 9:** LASSO → sparse (L1, diamond). Ridge → small (L2, circle). Elastic Net → both.

> 📌 **Tip 10:** If a question mentions "overfitting", think: regularization, cross-validation, more data, simpler model, dropout. If "underfitting": more features, more complex model, less regularization.

> 🧠 "ML in GATE tests conceptual clarity + mathematical precision. No vague answers — know the formulas, know the tradeoffs, know the traps."

> 🔥 "1800 practice problems. 50+ sections. Every formula derived. Every trap documented. You're more prepared than 99% of GATE candidates."

---

# 📝 Practice Set 21: True or False Marathon (P1551 – P1650)

| # | Statement | T/F | Explanation |
|---|---|---|---|
| P1551 | Linear regression minimizes squared error | T | MSE is the objective |
| P1552 | Linear regression requires features to be independent | F | Works with dependent features (but coefficients become unstable) |
| P1553 | Ridge regression performs feature selection | F | Only LASSO does (L1) |
| P1554 | PCA requires standardized data | T | Otherwise dominated by high-variance features |
| P1555 | K-Means always converges | T | But to local optimum only |
| P1556 | K-Means works with categorical data | F | Requires distance metric → use K-Modes |
| P1557 | Naive Bayes assumes feature independence | T | "Naive" = conditional independence given class |
| P1558 | SVM can only do binary classification | F | Can do multi-class via OvA or OvO |
| P1559 | SVM finds the maximum margin hyperplane | T | Core principle |
| P1560 | KNN requires training phase | F | Lazy learner — no training |
| P1561 | Decision trees are invariant to feature scaling | T | Split based on thresholds, not distances |
| P1562 | Random Forest can overfit | T | But much less than single deep tree |
| P1563 | AdaBoost is sensitive to outliers | T | Hard-to-classify points get high weight |
| P1564 | Gradient Descent always finds global minimum | F | Only for convex functions |
| P1565 | Logistic Regression loss is convex | T | Cross-entropy with sigmoid is convex |
| P1566 | Cross-validation prevents all overfitting | F | Reduces risk but doesn't eliminate |
| P1567 | More data always helps | F | Doesn't help with high bias (underfitting) |
| P1568 | Deep neural networks are linear models | F | Nonlinear due to activation functions |
| P1569 | Without activation, deep NN = linear model | T | Composition of linear = linear |
| P1570 | Sigmoid output is always between 0 and 1 | T | $\sigma(z) \in (0, 1)$ for any real $z$ |
| P1571 | Softmax outputs sum to 1 | T | By definition of softmax |
| P1572 | ReLU can cause dead neurons | T | If $z < 0$ always → gradient = 0 forever |
| P1573 | Leaky ReLU solves dead neuron problem | T | Small gradient for $z < 0$ |
| P1574 | Batch gradient descent uses all data per update | T | Hence slow for large datasets |
| P1575 | SGD uses one sample per update | T | Noisy but fast convergence |
| P1576 | Mini-batch GD is compromise of both | T | Batch size typically 32-256 |
| P1577 | Adam optimizer adapts learning rate | T | Per-parameter adaptive learning rate |
| P1578 | Momentum helps escape local minima | T | Accumulates velocity through flat regions |
| P1579 | F1 score is arithmetic mean of precision and recall | F | It's the harmonic mean |
| P1580 | AUC = 0.5 means random classifier | T | Same as coin flip |
| P1581 | High recall implies high precision | F | They often trade off |
| P1582 | Accuracy is always reliable | F | Misleading for imbalanced data |
| P1583 | DBSCAN requires specifying number of clusters | F | Determines automatically |
| P1584 | Hierarchical clustering produces dendrogram | T | Can cut at any level |
| P1585 | PCA components are orthogonal | T | Eigenvectors of covariance matrix |
| P1586 | First PCA component has maximum variance | T | By definition |
| P1587 | t-SNE preserves global structure | F | Focuses on local structure |
| P1588 | t-SNE is good for visualization | T | Designed for 2D/3D visualization |
| P1589 | LDA is supervised dimensionality reduction | T | Uses class labels |
| P1590 | PCA is supervised dimensionality reduction | F | Unsupervised |
| P1591 | Covariance matrix is always symmetric | T | $\text{Cov}(X,Y) = \text{Cov}(Y,X)$ |
| P1592 | Eigenvalues of covariance matrix can be negative | F | Covariance matrix is positive semi-definite |
| P1593 | GMM is a hard clustering method | F | Soft clustering (probabilities) |
| P1594 | K-Means is a special case of GMM | T | When covariances are equal and spherical |
| P1595 | EM algorithm guarantees global optimum | F | Local optimum (non-convex) |
| P1596 | EM algorithm always converges | T | Monotonically increases likelihood |
| P1597 | Perceptron can learn XOR | F | XOR is not linearly separable |
| P1598 | MLP with hidden layer can learn XOR | T | Hidden layer adds nonlinearity |
| P1599 | Universal approximation theorem: 1 hidden layer enough? | T | Can approximate any continuous function |
| P1600 | Universal approximation means 1 layer is always best? | F | May need exponentially many neurons → depth helps |

**P1601-P1650** — More True/False:

| # | Statement | T/F | Explanation |
|---|---|---|---|
| P1601 | Bayes' theorem: $P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}$ | T | Foundation of Bayesian ML |
| P1602 | MAP estimate = MLE when prior is uniform | T | Uniform prior → prior cancels |
| P1603 | MLE is biased for variance estimation | T | $(n-1)$ instead of $n$ for unbiased |
| P1604 | Bias of MLE decreases with more data | T | Asymptotically unbiased |
| P1605 | K-fold CV uses 100% data for training (across folds) | T | Each point is in training set $(k-1)$ times |
| P1606 | LOOCV has high variance | T | Each fold trains on $n-1$ similar sets |
| P1607 | LOOCV has low bias | T | Training on nearly all data |
| P1608 | 5-fold CV has higher bias than LOOCV | T | Uses only 80% per fold |
| P1609 | 5-fold CV has lower variance than LOOCV | T | Folds are more different → less correlated |
| P1610 | In practice, 5-fold or 10-fold CV is preferred | T | Good bias-variance tradeoff |
| P1611 | Log loss is same as cross-entropy | T | Different names, same thing |
| P1612 | Hinge loss is used in SVM | T | $\max(0, 1 - y \cdot f(x))$ |
| P1613 | Huber loss combines MSE and MAE | T | Quadratic near 0, linear far away |
| P1614 | MAE is more robust to outliers than MSE | T | Linear penalty vs quadratic |
| P1615 | Gradient of MSE exists everywhere | T | Differentiable |
| P1616 | Gradient of MAE exists everywhere | F | Not differentiable at 0 |
| P1617 | L1 regularization is differentiable everywhere | F | Not differentiable at $w_j = 0$ |
| P1618 | Coordinate descent works for LASSO | T | Efficient for L1 optimization |
| P1619 | Random Forest: all trees see all features | F | Random subset at each split |
| P1620 | Random Forest: all trees see all data | F | Bootstrap samples |
| P1621 | Bagging: sampling with replacement | T | Bootstrap = with replacement |
| P1622 | Pasting: sampling without replacement | T | Similar to bagging but w/o replacement |
| P1623 | Out-of-bag samples ≈ 36.8% of data | T | $\approx 1/e$ not selected |
| P1624 | SMOTE creates synthetic minority samples | T | Interpolation between existing minority points |
| P1625 | SMOTE can cause overfitting | T | If applied to noise/overlapping regions |
| P1626 | Stratified split maintains class proportions | T | Important for imbalanced data |
| P1627 | Information gain is entropy minus weighted avg entropy | T | $IG = H(S) - \sum\frac{|S_v|}{|S|}H(S_v)$ |
| P1628 | Gini impurity and entropy always agree on splits | F | Usually agree but can differ |
| P1629 | $\text{Gini} \in [0, 0.5]$ for binary | T | Max at $p = 0.5$: $2 \times 0.5 \times 0.5 = 0.5$ |
| P1630 | $\text{Entropy} \in [0, 1]$ for binary | T | Max at $p = 0.5$: $\log_2 2 = 1$ |
| P1631 | Information gain ratio fixes bias toward many-valued attrs | T | Used in C4.5 algorithm |
| P1632 | ID3 uses information gain | T | Original decision tree algorithm |
| P1633 | CART produces binary trees | T | Classification And Regression Trees |
| P1634 | CART uses Gini impurity for classification | T | Default in sklearn |
| P1635 | CART uses MSE for regression trees | T | Variance reduction |
| P1636 | Pruning: pre-pruning stops tree growth early | T | Max depth, min samples, etc. |
| P1637 | Pruning: post-pruning grows full tree then cuts | T | Cost complexity pruning |
| P1638 | Cost complexity pruning uses $\alpha$ parameter | T | Trade-off: accuracy vs tree size |
| P1639 | Silhouette score range: $[-1, 1]$ | T | 1 = perfect, 0 = overlap, -1 = wrong cluster |
| P1640 | Davies-Bouldin index: lower is better | T | Measures cluster compactness/separation ratio |
| P1641 | Calinski-Harabasz: higher is better | T | Between-cluster / within-cluster variance |
| P1642 | Elbow method: pick $k$ where WCSS drops sharply | T | Subjective but commonly used |
| P1643 | Dunn index: higher is better | T | Min inter-cluster / max intra-cluster distance |
| P1644 | Adjusted Rand Index corrected for chance | T | ARI: 0 = random, 1 = perfect |
| P1645 | Normalized Mutual Information: $[0, 1]$ | T | 1 = perfect match |
| P1646 | SVM soft margin: $C$ controls slack | T | Higher $C$ = less slack allowed |
| P1647 | SVM: $\sum \alpha_i y_i = 0$ is a KKT condition | T | Dual constraint |
| P1648 | Kernel SVM: never compute $\phi(x)$ explicitly | T | Kernel trick computes $\phi(x_i) \cdot \phi(x_j)$ |
| P1649 | RBF kernel projects to infinite dimensions | T | Proved via Taylor expansion |
| P1650 | Linear kernel = no kernel (dot product) | T | $K(x_i, x_j) = x_i^T x_j$ |

---

# 📝 Practice Set 22: Numerical Speed Drill (P1651 – P1750)

**Compute these quickly — GATE exam speed practice!**

**Information Theory & Trees:**

| # | Question | Answer |
|---|---|---|
| P1651 | $H(0.5, 0.5) = ?$ | 1.0 bit |
| P1652 | $H(0.25, 0.75) = ?$ | $0.811$ bits |
| P1653 | $H(1, 0) = ?$ | 0 bits (pure) |
| P1654 | $H(0.1, 0.9) = ?$ | $0.469$ bits |
| P1655 | $H(1/3, 1/3, 1/3) = ?$ | $\log_2 3 = 1.585$ bits |
| P1656 | $\text{Gini}(0.5, 0.5) = ?$ | $1 - 0.25 - 0.25 = 0.5$ |
| P1657 | $\text{Gini}(0.25, 0.75) = ?$ | $1 - 0.0625 - 0.5625 = 0.375$ |
| P1658 | $\text{Gini}(1, 0) = ?$ | 0 (pure) |
| P1659 | $\text{Gini}(0.1, 0.9) = ?$ | $1 - 0.01 - 0.81 = 0.18$ |
| P1660 | $\text{Gini}(1/3, 1/3, 1/3) = ?$ | $1 - 3(1/9) = 2/3 = 0.667$ |

**Probability & Bayes:**

| # | Question | Answer |
|---|---|---|
| P1661 | P(A)=0.3, P(B|A)=0.8, P(B|¬A)=0.1. P(A|B)=? | $\frac{0.3 \times 0.8}{0.3 \times 0.8 + 0.7 \times 0.1} = \frac{0.24}{0.31} = 0.774$ |
| P1662 | P(Spam)=0.4, P("free"|Spam)=0.6, P("free"|Ham)=0.1. P(Spam|"free")=? | $\frac{0.4 \times 0.6}{0.4 \times 0.6 + 0.6 \times 0.1} = \frac{0.24}{0.30} = 0.80$ |
| P1663 | P(Disease)=0.01, P(+|Disease)=0.99, P(+|Healthy)=0.05. P(Disease|+)=? | $\frac{0.01 \times 0.99}{0.01 \times 0.99 + 0.99 \times 0.05} = \frac{0.0099}{0.0594} = 0.167$ |

> 🎯 **GATE Lesson from P1663:** Even with 99% accurate test, only 16.7% chance of disease! Base rate matters!

**Sigmoid & Logistic:**

| # | Question | Answer |
|---|---|---|
| P1664 | $\sigma(0) = ?$ | 0.5 |
| P1665 | $\sigma(2) \approx ?$ | 0.881 |
| P1666 | $\sigma(-2) \approx ?$ | 0.119 (= $1 - 0.881$) |
| P1667 | $\sigma(10) \approx ?$ | $\approx 1$ (0.99995) |
| P1668 | $\sigma(-10) \approx ?$ | $\approx 0$ (0.00005) |
| P1669 | $\sigma(z) + \sigma(-z) = ?$ | Always 1 |
| P1670 | $\sigma'(z) = ?$ | $\sigma(z)(1 - \sigma(z))$ |
| P1671 | $\sigma'(0) = ?$ | $0.5 \times 0.5 = 0.25$ (maximum!) |

**SVM Margin:**

| # | Question | Answer |
|---|---|---|
| P1672 | $w = [3, 4]$, $\|w\| = ?$ | 5 |
| P1673 | Margin = $\frac{2}{\|w\|} = ?$ | 0.4 |
| P1674 | $w = [1, 0]$, margin = ? | 2 |
| P1675 | $w = [1, 1]$, margin = ? | $\frac{2}{\sqrt{2}} = \sqrt{2} \approx 1.414$ |
| P1676 | $w = [2, 2, 1]$, $\|w\| = ?$ | 3, margin = $\frac{2}{3}$ |

**PCA Variance:**

| # | Question | Answer |
|---|---|---|
| P1677 | Eigenvalues: 5, 3, 1, 1. Total variance? | 10 |
| P1678 | Variance explained by PC1? | 5/10 = 50% |
| P1679 | Variance by PC1 + PC2? | 8/10 = 80% |
| P1680 | Components for 90% variance? | 3 (9/10 = 90%) |
| P1681 | Eigenvalues: 10, 5, 2, 1, 0.5. PC1% ? | 10/18.5 = 54.1% |
| P1682 | Components for 80% with eigenvalues above? | 2 (15/18.5 = 81.1%) |

**K-Means:**

| # | Question | Answer |
|---|---|---|
| P1683 | Points: {1, 2, 8, 9}. K=2. Initial centroids: 1, 8. After 1 iteration? | C1={1,2}→1.5, C2={8,9}→8.5 |
| P1684 | WCSS for above? | $(1-1.5)^2 + (2-1.5)^2 + (8-8.5)^2 + (9-8.5)^2 = 0.25+0.25+0.25+0.25 = 1.0$ |
| P1685 | Points: {1,3,5,7,9}. K=2. Init: 1, 9. Iteration 1? | C1={1,3,5}→3, C2={7,9}→8 |
| P1686 | Iteration 2? | C1={1,3,5}→3, C2={7,9}→8 (converged!) |

**Distance Metrics:**

| # | Question | Answer |
|---|---|---|
| P1687 | Euclidean: (0,0) to (3,4) | 5 |
| P1688 | Manhattan: (0,0) to (3,4) | 7 |
| P1689 | Chebyshev (L∞): (0,0) to (3,4) | 4 |
| P1690 | Minkowski $p=3$: (0,0) to (3,4) | $(27+64)^{1/3} = 91^{1/3} \approx 4.50$ |
| P1691 | Cosine similarity: (1,0) and (0,1) | 0 (orthogonal) |
| P1692 | Cosine similarity: (1,1) and (2,2) | 1 (same direction) |
| P1693 | Cosine similarity: (1,0) and (1,1) | $\frac{1}{\sqrt{2}} \approx 0.707$ |
| P1694 | Hamming distance: 10110 vs 10011 | 2 (positions 3 and 4 differ) |
| P1695 | Jaccard similarity: {a,b,c} vs {b,c,d} | $\frac{|{b,c}|}{|{a,b,c,d}|} = \frac{2}{4} = 0.5$ |

**KNN Quick:**

| # | Question | Answer |
|---|---|---|
| P1696 | Query: (5,5). Neighbors: (4,4,A), (6,6,B), (5,4,A). K=3. Predict? | A (2 votes A, 1 vote B) |
| P1697 | K=1, nearest is B at distance 1.414. Predict? | B |
| P1698 | K=2, nearest: A(1.414), B(1.414). Tie? | Use distance-weighted: both equal → use lower class label or random |
| P1699 | Weighted KNN: weight = 1/d². Closer A at d=1, farther B at d=3. Predict? | A: weight=1, B: weight=1/9. A wins. |
| P1700 | Best $k$ for 100 training samples? | Try odd values: 3, 5, 7, ... validate with CV |

**Linear Regression:**

| # | Question | Answer |
|---|---|---|
| P1701 | Points: (1,2), (2,4), (3,6). Slope? | $b = \frac{n\sum xy - \sum x \sum y}{n\sum x^2 - (\sum x)^2} = \frac{3(28) - 6(12)}{3(14) - 36} = \frac{84-72}{42-36} = \frac{12}{6} = 2$ |
| P1702 | Intercept? | $a = \bar{y} - b\bar{x} = 4 - 2(2) = 0$ |
| P1703 | Equation? | $y = 2x$ (perfect fit!) |
| P1704 | $R^2$ for above? | 1.0 (perfect linear relationship) |
| P1705 | Predict $y$ at $x = 5$? | $y = 2(5) = 10$ |
| P1706 | Points: (1,1), (2,2), (3,4). Slope? | $b = \frac{3(1+4+12) - 6 \times 7}{3(14) - 36} = \frac{3(17)-42}{42-36} = \frac{51-42}{6} = \frac{9}{6} = 1.5$ |
| P1707 | Intercept? | $a = 7/3 - 1.5(2) = 2.333 - 3 = -0.667$ |
| P1708 | MSE for $y = 1.5x - 0.667$? | Predictions: 0.833, 2.333, 3.833. Errors: 0.167, 0.333, 0.167. MSE = $(0.028+0.111+0.028)/3 = 0.056$ |

**Confusion Matrix Speed:**

| # | Scenario | TP | FP | TN | FN | Precision | Recall | F1 |
|---|---|---|---|---|---|---|---|---|
| P1709 | 90 correct pos, 10 false alarm, 5 missed | 90 | 10 | 895 | 5 | 90% | 94.7% | 92.3% |
| P1710 | 50 correct pos, 50 false alarm, 50 missed | 50 | 50 | 850 | 50 | 50% | 50% | 50% |
| P1711 | 100 correct pos, 0 false alarm, 0 missed | 100 | 0 | 900 | 0 | 100% | 100% | 100% |

**P1712-P1750** — Speed Round Answers:

| # | Question | Answer |
|---|---|---|
| P1712 | Softmax([1,2,3]): largest output? | For class 3: $e^3 / (e^1+e^2+e^3) = 0.665$ |
| P1713 | Cross-entropy: true=[1,0,0], pred=[0.7,0.2,0.1]? | $-\ln(0.7) = 0.357$ |
| P1714 | Cross-entropy: true=[1,0,0], pred=[0.9,0.05,0.05]? | $-\ln(0.9) = 0.105$ (better prediction → lower loss) |
| P1715 | KL divergence $D_{KL}(P\|Q) = 0$ means? | P = Q (identical distributions) |
| P1716 | Sum of all attention weights? | 1 (softmax output) |
| P1717 | Perceptron update: $w = [1,1]$, $x = [1,-1]$, $y = +1$, $\hat{y} = -1$. $\eta = 1$. New $w$? | $w' = w + \eta y x = [1,1] + [1,-1] = [2,0]$ |
| P1718 | Number of parameters in fully connected layer: 100 inputs → 50 outputs? | $100 \times 50 + 50 = 5050$ |
| P1719 | Conv layer: 3×3 kernel, 32 filters, 3 input channels? | $(3 \times 3 \times 3 + 1) \times 32 = 28 \times 32 = 896$ |
| P1720 | Max pooling 2×2 on 4×4 input? | 2×2 output |
| P1721 | Output size: input 28×28, kernel 5×5, stride 1, no padding? | $(28-5)/1 + 1 = 24$. Output: 24×24 |
| P1722 | Same padding for 3×3 kernel? | Pad = 1 on each side |
| P1723 | Random Forest: 100 trees, 16 features. Features per split? | $\sqrt{16} = 4$ (classification) |
| P1724 | AdaBoost: if classifier gets all correct, $\alpha = ?$ | $\alpha \to \infty$ (very high weight) |
| P1725 | AdaBoost: if classifier gets 50% error, $\alpha = ?$ | $\alpha = 0$ (random → ignored) |
| P1726 | XGBoost default max_depth? | 6 |
| P1727 | Gini for 3 equal classes (1/3 each)? | $1 - 3(1/9) = 2/3$ |
| P1728 | Entropy for 4 equal classes? | $\log_2 4 = 2$ bits |
| P1729 | Variance of [2,4,4,4,5,5,7,9]? | Mean=5, Var = $\frac{34}{8} = 4.25$ |
| P1730 | Covariance sign when X↑ and Y↑? | Positive |
| P1731 | Correlation vs covariance? | Correlation = standardized covariance $\in [-1,1]$ |
| P1732 | TP=80, FN=20. Recall? | 80/100 = 80% |
| P1733 | TP=80, FP=40. Precision? | 80/120 = 66.7% |
| P1734 | F1 for above? | $\frac{2 \times 0.667 \times 0.8}{0.667 + 0.8} = \frac{1.067}{1.467} = 0.727$ |
| P1735 | Specificity if TN=900, FP=40? | 900/940 = 95.7% |
| P1736 | FPR = ? | 1 - Specificity = 4.3% |
| P1737 | AUC of perfect classifier? | 1.0 |
| P1738 | AUC of random classifier? | 0.5 |
| P1739 | Matthews Correlation Coefficient range? | $[-1, +1]$. Best for imbalanced. |
| P1740 | Cohen's Kappa: 0 means? | Agreement no better than chance |
| P1741 | Cohen's Kappa: 1 means? | Perfect agreement |
| P1742 | SSE formula? | $\sum_{i=1}^{n}(y_i - \hat{y}_i)^2$ |
| P1743 | MSE formula? | $\frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_i)^2$ |
| P1744 | RMSE formula? | $\sqrt{MSE}$ |
| P1745 | MAE formula? | $\frac{1}{n}\sum_{i=1}^{n}|y_i - \hat{y}_i|$ |
| P1746 | MAPE formula? | $\frac{1}{n}\sum\frac{|y_i - \hat{y}_i|}{|y_i|} \times 100\%$ |
| P1747 | $R^2$ formula? | $1 - \frac{SSE}{SST}$ where $SST = \sum(y_i - \bar{y})^2$ |
| P1748 | Adjusted $R^2$ when adding useless feature? | Decreases (penalty for extra parameter) |
| P1749 | Logloss for binary: $y=1$, $\hat{p}=0.9$? | $-\ln(0.9) = 0.105$ |
| P1750 | Logloss for binary: $y=1$, $\hat{p}=0.1$? | $-\ln(0.1) = 2.303$ (very high — bad prediction!) |

---

# 📝 Practice Set 23: Algorithm Pseudocode Reference (P1751 – P1800)

**P1751.** K-Means Pseudocode:
```
FUNCTION KMeans(data, K):
    Initialize K centroids randomly from data
    REPEAT:
        FOR each point x in data:
            Assign x to nearest centroid
        FOR each cluster j:
            centroid_j = mean of points in cluster j
    UNTIL centroids don't change (or max iterations)
    RETURN clusters, centroids
```
Time: $O(n \cdot K \cdot d \cdot I)$ where $I$ = iterations, $d$ = dimensions.

**P1752.** KNN Pseudocode:
```
FUNCTION KNN_Predict(train_data, query, K):
    FOR each point (x, y) in train_data:
        Compute distance(query, x)
    Sort by distance, take K nearest
    Classification: Return majority vote of K labels
    Regression: Return mean of K values
```
Time: $O(n \cdot d)$ per query (no training).

**P1753.** ID3 Decision Tree:
```
FUNCTION ID3(data, features):
    IF all labels same: RETURN leaf(label)
    IF no features left: RETURN leaf(majority_label)
    best = feature with max Information Gain
    node = create decision node(best)
    FOR each value v of best:
        subset = data where best = v
        IF subset empty: child = leaf(majority of data)
        ELSE: child = ID3(subset, features - {best})
        Add child to node
    RETURN node
```

**P1754.** Gradient Descent:
```
FUNCTION GradientDescent(f, grad_f, x0, lr, max_iter):
    x = x0
    FOR i = 1 to max_iter:
        g = grad_f(x)
        x = x - lr * g
        IF norm(g) < epsilon: BREAK
    RETURN x
```

**P1755.** AdaBoost:
```
FUNCTION AdaBoost(data, T):
    Initialize weights: w_i = 1/n for all i
    FOR t = 1 to T:
        Train weak learner h_t on weighted data
        error_t = sum(w_i * I(h_t(x_i) != y_i))
        alpha_t = 0.5 * ln((1 - error_t) / error_t)
        Update: w_i *= exp(-alpha_t * y_i * h_t(x_i))
        Normalize: w_i /= sum(w)
    RETURN H(x) = sign(sum(alpha_t * h_t(x)))
```

**P1756.** PCA Algorithm:
```
FUNCTION PCA(X, k):
    Center: X = X - mean(X)
    Covariance: C = X^T X / (n-1)
    Eigendecomposition: C = V Λ V^T
    Sort eigenvalues descending
    Select top k eigenvectors → W (d×k)
    Project: Z = X W (n×k)
    RETURN Z, W, eigenvalues
```

**P1757.** Naive Bayes:
```
FUNCTION NaiveBayes_Predict(x, classes):
    FOR each class c:
        score_c = log P(c)   // Prior
        FOR each feature j:
            score_c += log P(x_j | c)  // Likelihood
    RETURN argmax(score_c)
```

**P1758-P1780** — Algorithm comparison table (THE definitive GATE table):

| Property | LR | LogReg | SVM | KNN | DT | RF | NB | k-Means | PCA |
|---|---|---|---|---|---|---|---|---|---|
| Type | Regression | Classification | Both | Both | Both | Both | Classification | Clustering | DR |
| Parametric? | Yes | Yes | Yes | No | No | No | Yes | Yes | N/A |
| Linear? | Yes | Yes | Kernel: No | No | No | No | Yes | N/A | Linear |
| Training | Fast | Medium | Slow | None | Fast | Medium | Very Fast | Medium | Fast |
| Prediction | Fast | Fast | Fast | Slow | Fast | Medium | Fast | Fast | Fast |
| Scaling needed? | Yes (for GD) | Yes | Yes | Yes | No | No | No | Yes | Yes |
| Handles missing? | No | No | No | No | Yes (CART) | Yes | No (impute first) | No | No |
| Handles categorical? | Encode | Encode | Encode | Distance func | Natively | Natively | Natively | Encode | Encode |
| Interpretable? | Yes | Yes | No (kernel) | No | Yes | No | Yes | Yes | Medium |
| GATE frequency | ★★★★★ | ★★★★★ | ★★★★★ | ★★★★ | ★★★★★ | ★★★ | ★★★★★ | ★★★★★ | ★★★★★ |

**P1781-P1800** — GATE Previous Year Question Patterns:

| # | Pattern | Example | Method |
|---|---|---|---|
| P1781 | "Calculate entropy of..." | Given probabilities → compute $H$ | $H = -\sum p \log_2 p$ |
| P1782 | "Which split has highest IG?" | Multiple attributes, compute IG for each | IG = parent_H - weighted_child_H |
| P1783 | "Find margin of SVM with w=..." | Given weight vector | $\frac{2}{\|w\|}$ |
| P1784 | "Classify using Naive Bayes..." | Given priors and likelihoods | Bayes rule, compare posteriors |
| P1785 | "After 1 iteration of K-Means..." | Points + initial centroids | Assign + update centroids |
| P1786 | "How many PCA components for X%?" | Eigenvalues given | Sum until cumulative ≥ X% |
| P1787 | "Predict using KNN with K=..." | Points with labels + query | Find K nearest, majority vote |
| P1788 | "Gradient descent after 1 step..." | Function + initial point + LR | $x' = x - \eta \nabla f(x)$ |
| P1789 | "Precision/Recall from confusion matrix" | 2×2 matrix given | P = TP/(TP+FP), R = TP/(TP+FN) |
| P1790 | "Bias-Variance tradeoff: increase K in KNN?" | Conceptual | Higher K → higher bias, lower variance |
| P1791 | "L1 vs L2 regularization difference?" | Conceptual | L1: sparse, L2: small weights |
| P1792 | "Perceptron can/cannot learn..." | XOR, AND, OR | XOR: cannot (not linearly separable) |
| P1793 | "Sigmoid of linear combination gives..." | Model identification | Logistic regression |
| P1794 | "OOB error in Random Forest..." | Conceptual | ~36.8% samples not in bootstrap |
| P1795 | "Feature importance from tree..." | Compute from Gini decrease | Sum of weighted impurity decreases |
| P1796 | "Cross-validation: what is $k$ in $k$-fold?" | Conceptual | # of folds (5 or 10 typical) |
| P1797 | "Bayes optimal classifier?" | Conceptual | Predicts class with highest posterior |
| P1798 | "PAC learning: sample complexity?" | Given $\epsilon, \delta, |H|$ | $m \geq \frac{1}{\epsilon}(\ln|H| + \ln\frac{1}{\delta})$ |
| P1799 | "VC dimension of linear classifier in $\mathbb{R}^d$?" | Direct | $d + 1$ |
| P1800 | "Which algo minimizes hinge loss?" | Identification | SVM |

---

# 🔥 ML One-Page Summary for GATE (Print This!)

| Topic | Key Formula | Remember |
|---|---|---|
| **Linear Regression** | $w = (X^TX)^{-1}X^Ty$ | Normal equation; GD alternative |
| **Ridge** | $w = (X^TX + \lambda I)^{-1}X^Ty$ | L2; Gaussian prior; no sparsity |
| **LASSO** | $\min \|y-Xw\|^2 + \lambda\|w\|_1$ | L1; Laplace prior; sparsity |
| **Logistic Reg** | $P(y=1|x) = \sigma(w^Tx+b)$ | Sigmoid; cross-entropy loss |
| **SVM** | Margin = $\frac{2}{\|w\|}$ | Max margin; support vectors; kernel trick |
| **KNN** | Predict = majority of K nearest | Lazy; no training; $O(nd)$ per query |
| **Naive Bayes** | $P(c|x) \propto P(c)\prod P(x_j|c)$ | Conditional independence; Laplace smoothing |
| **Decision Tree** | $IG = H(S) - \sum\frac{|S_v|}{|S|}H(S_v)$ | Entropy/Gini; greedy; prune to avoid overfit |
| **Random Forest** | Bag + feature subset + majority | Reduces variance; $\sqrt{d}$ features |
| **AdaBoost** | $\alpha_t = 0.5\ln\frac{1-\epsilon}{\epsilon}$ | Reweights misclassified; reduces bias |
| **K-Means** | $\arg\min \sum\|x_i - \mu_{c_i}\|^2$ | Always converges; local optimum; elbow method |
| **PCA** | Top $k$ eigenvectors of $\text{Cov}(X)$ | Max variance; orthogonal; unsupervised |
| **Metrics** | Precision, Recall, F1, AUC | F1 = harmonic mean; AUC for imbalanced |

> 📌 **Final words:** ML in GATE is about precision — know every formula, every tradeoff, every edge case. This document covers it ALL. 1800 problems, 50+ sections, complete derivations, and every GATE pattern documented.

---

*🏆 End of Machine Learning — Comprehensive Notes for GATE 2027 (CS/DA)*

*📅 Last Updated: April 2026 | Covers: GATE DA Syllabus | 1800 Practice Problems | 100% Exam Coverage*


---

*End of Questions — Machine Learning for GATE 2027 — 100 Questions*
