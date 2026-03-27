# Lecture 4: Uncertainties, Monte Carlo Methods, and Machine Learning Foundations

**Instructor:** Dr. Federica Bianco

This lecture bridges the gap between statistical inference and practical data analysis. We will explore the nature of uncertainties, how to combine them, and powerful computational methods like Monte Carlo simulations. Finally, we'll lay the groundwork for machine learning by introducing key concepts like objective functions, model validation, and gradient descent.

---

## 1. The Nature of Uncertainty in Science

Uncertainty in measurements comes in two fundamental types, each with distinct properties and mitigation strategies.

### Statistical (Random) Uncertainties
- **Characteristics:**
    - Unpredictable variation with **no preferred direction**.
    - Caused by lack of sensitivity or inherent stochasticity in the process.
    - Shrinks with sample size (typically as $1/\sqrt{N}$).
    - Often follows a known distribution (e.g., Gaussian, Poisson).
- **Examples:** Reading a ruler, counting radioactive decays, atmospheric turbulence.
- **Mitigation:** Take more measurements; the uncertainty decreases.

### Systematic Uncertainties (Bias)
- **Characteristics:**
    - Reproducible inaccuracy with a **preferred direction** (biases the measurement).
    - Caused by faulty equipment, calibration errors, or unrepresentative sampling.
    - **Does not shrink with sample size**; it affects all measurements.
    - Distribution is often unknown (we often assume Gaussian for convenience, but this is risky).
- **Examples:** A ruler calibrated at the wrong temperature, survey undercoverage, publication bias, p-hacking (data dredging).
- **Mitigation:** Careful experimental design, cross-calibration, understanding the data-generating process.

**Key Distinction:** Accuracy is closeness to the true value (affected by systematic error), while Precision is the reproducibility of a measurement (affected by statistical error).

---

## 2. Combining Uncertainties

When a final quantity $f$ is a function of multiple independent measurements $(x, y, ..., w)$, each with its own uncertainty ($\Delta_x, \Delta_y, ..., \Delta_w$), the uncertainty in $f$ is given by the **quadratic sum** (error propagation):

$$\Delta_f = \sqrt{\left(\frac{\partial f}{\partial x}\right)^2 \Delta_x^2 + \left(\frac{\partial f}{\partial y}\right)^2 \Delta_y^2 + ... + \left(\frac{\partial f}{\partial w}\right)^2 \Delta_w^2}$$

This formula assumes the measurements are independent and the uncertainties are random. For a simple linear combination $f = ax + by$, this reduces to $\Delta_f = \sqrt{a^2\Delta_x^2 + b^2\Delta_y^2}$.

---

## 3. Monte Carlo Methods

Named after the famous casino, Monte Carlo methods use repeated random sampling to obtain numerical results. They are invaluable when analytical solutions are intractable.

### History & Core Idea
- **Ulam & Metropolis (1940s):** While working on the Manhattan Project, they realized that simulating random processes (like neutron diffusion) could solve complex problems. Enrico Fermi used an early version ("Fermiac") to study neutron transport.
- **Simple Example: Calculating π:** Randomly throw points into a square containing a quarter-circle. The ratio of points inside the circle to the total approximates $\pi/4$.

### Resampling Methods (Bootstrap & Jackknife)
These techniques allow us to estimate the uncertainty of a statistic without assuming a theoretical distribution.

- **Bootstrap:**
    - Repeatedly draw *M* random samples *with replacement* from the original dataset (same size as original).
    - Calculate the statistic of interest (mean, regression coefficient, etc.) for each bootstrap sample.
    - The distribution of these *M* estimates approximates the sampling distribution of the statistic.
- **Jackknife:**
    - Sequentially delete one observation at a time, creating *N* subsamples (each of size N-1).
    - Calculate the statistic for each subsample.
    - This method is deterministic and can reduce bias.

---

## 4. Introduction to Machine Learning

Machine Learning (ML) is the discipline of creating models where the parameters are **learned from data**. It sits at the intersection of statistics, computer science, and domain knowledge.

### Supervised vs. Unsupervised Learning

| | **Supervised Learning** | **Unsupervised Learning** |
| :--- | :--- | :--- |
| **Goal** | Predict a known target (label) from features. | Find hidden structure or patterns in data. |
| **Data** | Labeled data (e.g., images with "cat"/"dog" labels). | Unlabeled data (no target variable). |
| **Examples** | Regression (predict a number), Classification (predict a category). | Clustering (grouping similar data), Dimensionality Reduction (PCA). |
| **Algorithms** | Linear Regression, k-Nearest Neighbors, Neural Networks, SVMs. | k-Means, Hierarchical Clustering, Principal Component Analysis. |

**Semi-supervised learning** uses a small amount of labeled data with a large amount of unlabeled data. **Active learning** allows the model to query the user for labels on the most informative data points.

---

## 5. The Mechanics of Model Fitting

### The Objective Function
To find the "best" model parameters, we define a function to **minimize** (or maximize). Common choices for regression:

- **$L_1$ (Absolute Error):** $\sum_{i=1}^N |f(x_i) - y_i|$ (Robust to outliers)
- **$L_2$ (Squared Error):** $\sum_{i=1}^N (f(x_i) - y_i)^2$ (Punishes large errors heavily)
- **$\chi^2$ (Chi-Squared):** $\sum_{i=1}^N \frac{(f(x_i) - y_i)^2}{\sigma_i^2}$ (Weights by uncertainty; relates to the Gaussian likelihood)

### The Normal Equation (Analytical Solution)
For linear regression without uncertainties, the optimal parameters can be found analytically:

$$\begin{pmatrix} a \\ b \end{pmatrix} = (X^T X)^{-1} X^T \vec{y}$$

Where $X$ is the design matrix (first column of ones for the intercept, second column of feature values).

### Gradient Descent (Numerical Optimization)
When an analytical solution doesn't exist (most real-world models), we use iterative optimization.

1.  **Initialize** parameters (e.g., a, b for a line).
2.  **Calculate** the objective function value.
3.  **Compute the gradient** (direction of steepest ascent). Move in the **opposite direction** (downhill) to minimize the function.
4.  **Step** in that direction by a small amount (the **learning rate**).
5.  **Repeat** until the change is negligible (convergence).

**Stochastic Gradient Descent (SGD)** is a variation that uses a random subset of the data (a mini-batch) at each iteration, making it much faster for large datasets.

**Dangers:**
- **Local Minima:** The algorithm can get stuck in a suboptimal valley.
- **Learning Rate:** Too small leads to slow convergence; too large can cause divergence.
- **Initialization:** A poor starting point can lead to a poor solution.

---

## 6. Model Validation: Training, Testing, and Metrics

A good model must **generalize** to new, unseen data.

### The Train/Test Split
1.  Split the data into a **training set** (e.g., 70-80%) and a **test set** (e.g., 20-30%).
2.  Train the model (find parameters) using the **training set**.
3.  Evaluate its performance on the **test set**.
4.  If performance is much worse on the test set, the model is **overfit** (it memorized noise in the training data).

### Regression Performance Metrics
- **Mean Squared Error (MSE):** $\frac{1}{N}\sum (y_i - \hat{y}_i)^2$ (L2)
- **Root Mean Squared Error (RMSE):** $\sqrt{MSE}$ (Error in the same units as y)
- **$R^2$ (Coefficient of Determination):** $1 - \frac{\sum (y_i - \hat{y}_i)^2}{\sum (y_i - \bar{y})^2}$ (Fraction of variance explained by the model; ranges from 0 to 1).

A more robust workflow uses a **validation set** to tune hyperparameters, ensuring the test set is only used once for the final, unbiased performance estimate.

---

## 7. Case Study: Dark Matter and Rotation Curves

A classic physics application that integrates many of these concepts is the study of galaxy rotation curves.

- **Observation:** The rotational velocity ($v$) of stars in galaxies, measured via the Doppler shift of spectral lines.
- **Newtonian Prediction:** $v \propto \frac{1}{\sqrt{r}}$; velocity should decrease with distance from the galactic center.
- **Actual Observation:** The velocity remains flat or even increases with distance.
- **Inference:** There must be unseen mass—**Dark Matter**—extending far beyond the visible galaxy.

This inference relies on understanding uncertainties in velocity measurements, modeling the gravitational potential, and fitting models (like Navarro-Frenk-White profiles) to the rotation curve data—a perfect example of the data science pipeline we are building.

---

## Your Tasks

- **Homework:** Complete the earthquake notebook (KS test) and the galaxy rotational velocity notebook.
- **Reading:** Sessions 1 and 2 of Hogg & Foreman-Mackey (2017), "Data Analysis Recipes: Fitting a Model to Data" (arXiv:1710.06068).

This lecture concludes the fundamentals. Next, we will dive deeper into specific machine learning algorithms, starting with linear and multiple regression.
