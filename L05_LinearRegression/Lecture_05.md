# Lecture 5: The ABC of Machine Learning and Linear Regression

**Instructor:** Dr. Federica Bianco

This lecture introduces the foundational concepts of machine learning, focusing on regression. We will explore what a model is, how to fit a model to data using optimization, the importance of model validation, and the principles of model selection. We will also introduce Monte Carlo Markov Chain (MCMC) methods as a powerful alternative for exploring parameter spaces.

---

## 1. What is a Model?

In science, a model is a mathematical representation of reality. As George Box famously said, "All models are wrong, but some are useful." Models are simplifications that help us:

1.  **Understand:** A model can capture the essential relationship between variables (e.g., $h(t) = v_0 t - \frac{1}{2}gt^2$ for projectile motion).
2.  **Predict:** A model can forecast outcomes (e.g., Kepler's laws of planetary motion, $r(\nu) = \frac{a(1-e^2)}{1+e\cos(\nu)}$).

### Machine Learning Models

Arthur Samuel (1959) defined machine learning as the "field of study that gives computers the ability to learn without being explicitly programmed."

In the ML context:
- **Model:** A mathematical formula with parameters (e.g., $y = ax + b$ for a line).
- **Data:** A set of observations.
- **Learning:** Using the data to determine the best values for the model's parameters.

The goal is to create a **low-dimensional representation** of a higher-dimensional dataset. This representation allows us to generalize beyond the data we have seen.

### Supervised vs. Unsupervised Learning

| | **Supervised Learning** | **Unsupervised Learning** |
| :--- | :--- | :--- |
| **Goal** | Predict a known target (label) from features. | Find hidden structure or patterns in data. |
| **Data** | Labeled data (e.g., images with "cat"/"dog" labels). | Unlabeled data (no target variable). |
| **Examples** | Regression (predict a number), Classification (predict a category). | Clustering (grouping similar data), Dimensionality Reduction (PCA). |
| **Algorithms** | Linear Regression, k-Nearest Neighbors, Neural Networks, SVMs. | k-Means, Hierarchical Clustering, Principal Component Analysis. |

**Self-supervised learning** trains models to reproduce themselves to generate expressive data representations. **Active learning** allows the model to query the user for labels on the most informative data points.

---

## 2. Fitting a Model to Data: Linear Regression

Linear regression is the simplest and most fundamental supervised learning algorithm.

### The Normal Equation (Analytical Solution)

For a line fit to data without uncertainties, the optimal parameters (slope $a$, intercept $b$) can be found directly:

$$\begin{pmatrix} a \\ b \end{pmatrix} = (X^T X)^{-1} X^T \vec{y}$$

Here, $X$ is the **design matrix**:
$$X = \begin{pmatrix} 1 & x_1 \\ 1 & x_2 \\ \vdots & \vdots \\ 1 & x_m \end{pmatrix}$$
The first column of ones accounts for the intercept ($b$). This equation minimizes the sum of squared errors ($L_2$). In practice, we use `numpy.linalg.inv(X.T.dot(X)).dot(X.T).dot(y)` or `sklearn.linear_model.LinearRegression`.

### Objective Functions and Optimization

When an analytical solution doesn't exist, we define an **objective function** to minimize (or maximize). Common choices for regression:

- **$L_1$ (Absolute Error):** $\sum_{i=1}^N |f(x_i) - y_i|$ (robust to outliers)
- **$L_2$ (Squared Error):** $\sum_{i=1}^N (f(x_i) - y_i)^2$ (penalizes large errors heavily)
- **$\chi^2$ (Chi-Squared):** $\sum_{i=1}^N \frac{(f(x_i) - y_i)^2}{\sigma_i^2}$ (weights by uncertainty; relates to the Gaussian likelihood)

### Gradient Descent

Gradient Descent is an iterative optimization algorithm used to find the minimum of an objective function.

1.  **Initialize** parameters (e.g., $a_0$, $b_0$).
2.  **Calculate** the objective function value at the current parameters.
3.  **Compute the gradient** (the slope of the objective function). Move in the **opposite direction** (downhill) to minimize.
4.  **Step** in that direction by a small amount (the **learning rate**, $\eta$).
5.  **Repeat** until the gradient is near zero (convergence).

**Stochastic Gradient Descent (SGD)** uses a random subset (mini-batch) of the data at each iteration. This is much faster for large datasets and can help escape local minima.

**Important Considerations:**
- **Local vs. Global Minima:** Gradient descent can get stuck in a suboptimal valley.
- **Initialization:** A poor starting point can lead to a poor solution.
- **Learning Rate:** Too small leads to slow convergence; too large can cause divergence.
- **Stopping Criterion:** When the change in the objective function becomes very small.

---

## 3. Model Validation and Selection

### Train/Test Split
To assess how well a model generalizes, we split the data:

1.  **Training Set (e.g., 70%):** Used to learn the model parameters.
2.  **Test Set (e.g., 30%):** Used once to evaluate the model's performance on unseen data.

If performance is much worse on the test set, the model is **overfit** (it memorized noise in the training data).

**Cross-Validation (e.g., k-fold):** The data is split into $k$ folds. The model is trained on $k-1$ folds and validated on the remaining fold, rotating through all folds. This provides a more robust estimate of performance.

### Regression Performance Metrics
- **Mean Squared Error (MSE):** $\frac{1}{N}\sum (y_i - \hat{y}_i)^2$ (L2)
- **Root Mean Squared Error (RMSE):** $\sqrt{MSE}$ (error in the same units as $y$)
- **$R^2$ (Coefficient of Determination):** $1 - \frac{\sum (y_i - \hat{y}_i)^2}{\sum (y_i - \bar{y})^2}$ (fraction of variance explained; ranges from 0 to 1).

### Model Selection
The goal is to choose a model that balances fit with complexity. Key principles:

- **Parsimony (Occam's Razor):** When two models fit the data equally well, prefer the simpler one (with fewer parameters).
- **Overfitting:** Fitting data with a model that is too complex. It fits the training data perfectly but does not generalize to new data.

#### Information Criteria for Model Selection
These criteria penalize models for complexity, helping to prevent overfitting.

- **Akaike Information Criterion (AIC):** $AIC = -\frac{2}{N}\log(L) + \frac{2}{N}k$ (where $L$ is the likelihood, $k$ is the number of parameters, $N$ is the sample size).
- **Bayesian Information Criterion (BIC):** $BIC = -2\log(L) + \log(N)k$. Penalizes complexity more strongly than AIC for $N > e^2$.
- **Minimum Description Length (MDL):** $MDL = -\log(L(\theta)) - \log(L(y|X,\theta))$. Minimizes the total number of bits required to encode the model and its predictions.

**Likelihood Ratio Test:** For nested models (one model is a special case of the other), the likelihood ratio statistic follows a $\chi^2$ distribution under the null hypothesis that the simpler model is adequate. This provides a formal statistical test for model selection.

---

## 4. Monte Carlo Markov Chain (MCMC)

MCMC is a powerful alternative to gradient descent for exploring parameter spaces, especially when the likelihood surface is complex, multi-modal, or has correlated parameters.

### The Core Idea
Instead of simply finding the *best* parameters, MCMC **samples the posterior distribution** of the parameters. This gives a full picture of the uncertainty and correlations.

### The Metropolis-Hastings Algorithm
1.  Start at a random point in parameter space ($\theta_{\text{current}}$).
2.  Propose a new point ($\theta_{\text{new}}$) by perturbing the current point.
3.  Calculate the ratio $r = \frac{P(\theta_{\text{new}} | \text{data})}{P(\theta_{\text{current}} | \text{data})}$ (the ratio of the posteriors).
4.  **Decision:**
    - If $r > 1$, always accept $\theta_{\text{new}}$.
    - If $r < 1$, accept $\theta_{\text{new}}$ with probability $r$.
5.  Repeat, building a **chain** of accepted points.

### Key Properties
- **Markovian:** The next state depends only on the current state.
- **Ergodic:** The chain will eventually explore the entire parameter space.
- **Detailed Balance:** The chain is reversible, which is a sufficient condition for ergodicity.

### Advantages Over Gradient Descent
- MCMC can escape local minima due to its probabilistic acceptance step.
- It provides a full distribution of parameters, not just a single "best" value.
- It can handle highly correlated parameters efficiently (especially with algorithms like Hamiltonian Monte Carlo).

### Convergence Diagnostics
Multiple chains are run from different starting points. Convergence is checked using metrics like:
- **Gelman-Rubin (R̂):** Ratio of between-chain to within-chain variance. A value close to 1.0 indicates convergence.
- **Autocorrelation:** Measures how dependent successive samples are. Lower autocorrelation means more efficient sampling.

---

## Key Concepts

- **Model:** A parametrized representation of reality.
- **Objective Function:** A function of the data and parameters to be minimized (e.g., $L_1$, $L_2$, $\chi^2$).
- **Model Fitting:** Determining the best parameters to fit the observations within a chosen model.
- **Cross-Validation:** Assessing model generalization by testing on unseen data.
- **Model Selection:** Choosing between models using principles like parsimony and information criteria (AIC, BIC).
- **MCMC:** A stochastic method for sampling the posterior distribution of parameters.

---

## Your Tasks

- **Homework:** Apply the concepts of linear regression, objective functions, and cross-validation to real data.
- **Reading:** "Data Analysis Recipes: Fitting a Model to Data" by Hogg, Bovy, and Lang (Sections 1 & 2).
- **Midterm:** Covers material up to Lecture 3 (through NHRT). Work individually, disclose all AI use, and adhere to UD policies.

This lecture provides the foundational framework for understanding how machine learning models are built, evaluated, and selected. In the coming weeks, we will build on this framework to explore more complex algorithms like multiple regression, classification, and neural networks.
