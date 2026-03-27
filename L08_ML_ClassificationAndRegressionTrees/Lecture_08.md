# Lecture 8: Tree Methods – Interpretable Machine Learning

**Instructor:** Dr. Federica Bianco

This lecture introduces tree-based methods for supervised learning. We will explore how single decision trees work, their strengths and weaknesses, and how ensemble methods like Random Forests and Gradient Boosted Trees overcome their limitations. We will also cover essential topics like feature importance, encoding categorical variables, and evaluating classifier performance.

---

## 1. Classification vs. Clustering: A Quick Recap

Before diving into trees, it's useful to distinguish supervised from unsupervised learning.

- **Clustering (Unsupervised):** All features are observed for all points. The goal is to discover natural groupings in the feature space itself. The model returns a cluster label for each point.
- **Classification (Supervised):** A target feature (e.g., color) is observed only for a subset of points. The goal is to learn a rule that partitions the space to predict the target for new, unlabeled points.

Tree methods are a powerful form of **supervised learning**.

---

## 2. How Decision Trees Work

A decision tree partitions the feature space by making a series of binary splits along individual features.

- **Root Node:** The top-most node, containing all data.
- **Branches:** The outcome of a split decision (e.g., `gender == male`).
- **Leaf Nodes:** Terminal nodes that make the final prediction.

### A Classic Example: Predicting Titanic Survival

Starting with 714 passengers (424 survived, 290 died), the algorithm finds the split that maximizes "purity" at each node.

1.  **Root Split:** Which feature best separates survivors from non-survivors?
    - **Gender:** Split on `gender` yields a "male" node with 93 survivors / 360 dead (purity ~79% dead) and a "female" node with 197 survivors / 64 dead (purity ~75% alive). This is a good split.
2.  **Next Split:** For the "female" node (197 survivors, 64 dead), which feature next best separates the groups?
    - **Ticket Class:** Splitting females by `1st class` vs. `2nd+3rd` further increases purity in the sub-nodes.
3.  **Continue:** The process continues recursively, creating branches for `age` and other features until a stopping criterion is met.

The final tree can be visualized as a **dendrogram**, where each leaf represents a highly pure group (e.g., "first-class females under 6.5 years old").

---

## 3. Single Trees: The Good and The Bad

### Strengths
- **Non-Parametric:** Makes no assumptions about the data distribution.
- **White-Box:** Highly interpretable; the decision path for any prediction can be traced.
- **Versatile:** Works with any feature type (numeric, categorical, mixed) and can handle missing data.
- **Robust:** Relatively insensitive to outliers.

### Weaknesses
- **High Variance:** Small changes in the data can result in a completely different tree. This is because finding the globally optimal split is computationally intractable ($\infty^2$ for continuous variables), so algorithms use greedy, local optimization.
- **Overfitting:** Can grow very deep, memorizing the training data and generalizing poorly.

### Hyperparameters to Control Overfitting
- **Maximum Depth:** Limits how many times a node can split.
- **Minimum Samples per Leaf:** Prevents splits that create very small groups.
- **Pruning:** Growing the tree fully and then cutting back branches that don't add predictive power.

---

## 4. Ensemble Methods: The Cure for Variance

Ensemble methods combine multiple models to reduce variance and improve stability. For trees, two dominant approaches exist.

### Random Forest (Parallel)
- **How it Works:** Many trees are trained **independently** and **in parallel**.
- **Bagging (Bootstrap Aggregating):** Each tree is trained on a random subset (bootstrap sample) of the data.
- **Random Feature Selection:** At each split, only a random subset of features is considered. This decorrelates the trees.
- **Prediction:** For classification, the class predicted by the majority of trees is chosen (majority vote). For regression, the predictions are averaged.

### Gradient Boosted Trees (Sequential)
- **How it Works:** Trees are trained **sequentially**, each new tree attempts to correct the errors made by the previous one.
- **Learning from Mistakes:** The algorithm focuses on data points that were poorly predicted in earlier trees, adjusting weights accordingly.
- **Prediction:** The final prediction is the weighted sum of all the trees' outputs.

---

## 5. Evaluating Classifiers

To assess a classification model, we use the **confusion matrix**, which compares predicted labels to true labels.

| | **Predicted Positive** | **Predicted Negative** |
| :--- | :--- | :--- |
| **True Positive** | True Positive (TP) | False Negative (FN) – Type II Error |
| **True Negative** | False Positive (FP) – Type I Error | True Negative (TN) |

From this, we derive key metrics:
- **Accuracy:** $(TP + TN) / (TP + TN + FP + FN)$ – Overall correctness.
- **Precision:** $TP / (TP + FP)$ – Of points predicted as positive, how many were actually positive? (Avoid false alarms).
- **Recall (Sensitivity):** $TP / (TP + FN)$ – Of actual positive points, how many did we catch? (Avoid missing positives).

### ROC Curve
Many classifiers output a probability (e.g., a point is 70% likely to be class A). A threshold (e.g., 0.5) converts probability to a hard label.

The **Receiver Operating Characteristic (ROC)** curve plots the **True Positive Rate** (Recall) against the **False Positive Rate** ($FP / (FP + TN)$) as the threshold varies. The area under the ROC curve (AUC) is a holistic measure of model performance; a higher AUC indicates a better model that can achieve high recall without many false positives.

---

## 6. Feature Importance

Tree methods are valued for interpretability. We can measure how much each feature contributes to the final prediction by aggregating:
- **How early** the feature was used in splits (closer to the root is more important).
- **How often** the feature was used across all splits in the ensemble.

**Note:** Feature importance can be misleading if features are **correlated**. The importance may be arbitrarily split between correlated features.

---

## 7. Encoding Categorical Variables

Machine learning models require numerical inputs. Categorical variables (e.g., `species: dog, cat, bird`) must be encoded.

- **Numerical Encoding:** Assigning integers (dog=1, cat=2, bird=3). **Problem:** It implies an order (dog < cat < bird) that does not exist, which can mislead the model.
- **One-Hot Encoding:** Creating a new binary column for each category.
    - `dog` → columns: `is_dog`, `is_cat`, `is_bird` (1 for true, 0 for false).
    - **Advantage:** No artificial order is imposed.
    - **Disadvantage:** Increases the feature space dimensionality and introduces perfect correlations between new features (if `is_dog` is 1, the others must be 0). This is the standard approach but can complicate feature importance interpretation.

---

## 8. Key Takeaways

- **Tree Methods** are powerful, non-parametric supervised learning algorithms that partition space with binary splits.
- **Single trees** are interpretable but prone to high variance and overfitting.
- **Ensemble methods** like Random Forests (parallel, bagging) and Gradient Boosted Trees (sequential, error-correcting) dramatically improve performance and stability.
- **Model performance** for classification is evaluated using metrics like accuracy, precision, recall, and the ROC curve.
- **Feature importance** can be extracted, but caution is needed when features are correlated.
- **Categorical variables** should generally be **one-hot encoded** to avoid imposing an artificial order.

---

## Your Tasks

- **Homework:** Apply Random Forest and Gradient Boosted Tree classifiers and regressors to the Higgs Boson dataset from Kaggle. You will:
    - Split the data, train models, and compare training/test scores.
    - Produce confusion matrices and calculate L1/L2 metrics.
    - Identify feature importances and perform hyperparameter tuning using `RandomizedSearchCV`.
    - Generate and interpret ROC curves.

- **Reading:** "Interpretability Methods in Machine Learning: A Brief Survey" by Two Sigma.

Tree methods are a foundational tool in the data scientist's toolkit. Their interpretability and strong performance make them a go-to for many real-world problems.
