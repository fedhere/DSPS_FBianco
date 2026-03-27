# Lecture 2: Probability, Statistics, and the Foundations of Inference

**Instructor:** Dr. Federica Bianco

Building on our introduction to data science and the scientific method, this lecture dives into the mathematical and philosophical foundations of statistical inference. We will explore the nature of probability, the tools for describing data, and the frameworks—frequentist and Bayesian—that allow us to learn from data.

---

## 1. Recap: The Pillars of Good Science

Before we proceed, let's briefly recall the three core principles of "good" science we discussed last time:

- **Falsifiability:** A scientific theory must be testable and capable of being proven wrong.
- **Parsimony (Occam's Razor):** When two theories explain the data equally well, the simpler one (with fewer parameters) is preferred.
- **Reproducibility:** A second researcher, using the same raw data and code, should be able to duplicate the results. In practice, this means using open data, coding within notebooks, and—crucially—**setting your random seeds** (e.g., `np.random.seed(302)`) to ensure stochastic processes can be exactly replicated.

---

## 2. Data Types: The Foundation of Analysis

The type of data you have dictates the statistical tests you can use.

- **Categorical Variables:** Represent groups or categories.
    - **Nominal:** No inherent order (e.g., types of fruit, country of origin).
    - **Ordinal:** Categories with a specific order but unequal intervals (e.g., survey responses: "strongly disagree" to "strongly agree").

- **Numerical Variables:** Represent quantities on a numerical scale.
    - **Discrete:** Distinct, separate values, often counted in whole numbers (e.g., number of students).
    - **Continuous:** Can take on an infinite number of values within a range (e.g., height, temperature).
    - **Interval vs. Ratio:** Interval variables have equal intervals but no true zero (e.g., temperature in Celsius). Ratio variables have a meaningful zero point (e.g., weight, age).

**Why does this matter?** The choice between, say, a Pearson correlation (for continuous variables) and a Spearman rank correlation (for ordinal variables) depends entirely on the data type.

---

## 3. Probability: The Language of Uncertainty

Probability is the bedrock of statistics. There are two primary interpretations.

### The Frequentist Interpretation
Probability is the long-run frequency of an event. If I flip a fair coin 10,000 times, the probability of heads is the fraction of times it lands heads. The probability is a property of the coin itself.

### The Bayesian Interpretation
Probability represents a **degree of belief** or certainty about an event. If I *believe* a coin is unfair, my probability for heads reflects that belief and can be updated as I see new data. Probability is a state of knowledge.

### Basic Probability Arithmetic
- Probabilities range from 0 to 1.
- The probability of an event or its complement sums to 1: `P(A) + P(¬A) = 1`.
- For **disjoint** (mutually exclusive) events: `P(A or B) = P(A) + P(B)`.
- For **independent** events: `P(A and B) = P(A) * P(B)`.
- For **dependent** events, we use conditional probability: `P(A|B) = P(A ∩ B) / P(B)`.

---

## 4. Statistics: From Sample to Population

Statistics provides the tools to go from observing a **sample** (a finite subset) to inferring properties of the **population** (the entire group).

### Key Concepts
- **Distribution:** A mathematical model (a formula) that describes the behavior of a variable.
- **Law of Large Numbers:** As the sample size (N) grows, the sample mean gets closer and closer to the true population mean.

### Descriptive Statistics: Summarizing Distributions
We describe distributions using **moments**, which are calculated as integrals of the probability density function.

- **First Moment (Central Tendency):** The **mean** (average). Other measures include the **median** (50th percentile) and the **mode** (most common value).
- **Second Moment (Spread):** The **variance** (average squared deviation from the mean) and its square root, the **standard deviation (σ)** .
    - For a **Gaussian (Normal) distribution**, memorizing these values is essential:
        - 1σ ≈ 68% of the data
        - 2σ ≈ 95% of the data
        - 3σ ≈ 99.7% of the data
        - 5σ ≈ 99.99997% (a 1-in-3.5 million chance, the "gold standard" in particle physics for a discovery).

### Common Probability Distributions
- **Gaussian (Normal):** The famous "bell curve." Symmetric, well-behaved, and the default assumption for many natural uncertainties.
- **Binomial:** Models the number of successes in a fixed number of trials (e.g., coin flips).
- **Poisson:** Models the number of events occurring in a fixed interval of time or space (e.g., photons from a star, rain drops). Its mean and variance are equal.
- **Chi-squared (χ²):** The distribution of a sum of squared normal variables. It is fundamental to many statistical tests.

### The Central Limit Theorem (CLT)
One of the most important theorems in statistics. It states that, regardless of the shape of the original population distribution, the **distribution of sample means** will approach a Normal distribution as the sample size (N) increases (N ≈ 30 is often sufficient).

This is why the Normal distribution is so ubiquitous: it is the distribution of *averages*, even from non-normal data.

---

## 5. Bayes' Theorem: Updating Beliefs

Bayes' Theorem provides a mathematical framework for updating our beliefs in light of new evidence.

`P(θ | D) = P(D | θ) * P(θ) / P(D)`

- **Posterior P(θ|D):** Our updated belief about the model parameters (θ) after seeing the data (D). This is our goal.
- **Likelihood P(D|θ):** The probability of observing the data, given our model parameters. This is the core of frequentist inference.
- **Prior P(θ):** Our initial belief about the parameters *before* seeing the data. This can incorporate domain knowledge (e.g., "flux cannot be negative").
- **Evidence P(D):** The overall probability of the data, averaged over all possible parameter values. It acts as a normalizing constant.

In practice, for parameter estimation, we often work with the unnormalized posterior:
`P(θ | D) ∝ P(D | θ) * P(θ)`

---

## 6. Hypothesis Testing: The Frequentist Approach

The frequentist approach to scientific inference is known as **Null Hypothesis Rejection Testing (NHRT)** . It mirrors the logic of "beyond a reasonable doubt" in a courtroom.

### The Process
1.  **Formulate the Null Hypothesis (H₀):** This is the "assumption of innocence." It is often the opposite of what you hope to prove (e.g., "the coin is fair," "there is no difference between groups").
2.  **Identify Alternative Outcomes (H₁):** All possibilities that contradict the Null.
3.  **Set a Confidence Threshold (α):** Choose a p-value threshold, typically 0.05 (95% confidence) or 0.003 (99.7% confidence). This is the probability you are willing to accept of falsely rejecting the Null.
4.  **Choose a Pivotal Quantity (Test Statistic):** Find a quantity (e.g., χ², t-statistic, **Kolmogorov-Smirnov D-statistic**) that, *under the Null*, has a known distribution. The K-S test, for example, measures the maximum distance between two cumulative distributions to see if they could come from the same parent distribution.
5.  **Calculate the P-value:** Compute the test statistic from your data, then find the probability of obtaining a result as extreme as yours, *assuming the Null Hypothesis is true*.
6.  **Make a Decision:**
    - If `p-value < α`: Reject the Null. The result is "statistically significant." The evidence against H₀ is strong.
    - If `p-value ≥ α`: Fail to reject the Null. The evidence is not strong enough to rule out H₀.

---

## 7. The Scientific Method in a Probabilistic Context

These two approaches—Bayesian and Frequentist—offer different paths to the same goal: inferring the probability of our scientific theory (physics) given the data.

- **Bayesian:** Directly calculates `P(physics | data) = P(data | physics) * P(physics) / P(data)`.
- **Frequentist:** Uses the logic of NHRT. It formulates the Null as the opposite of the theory, and seeks to rule it out. If the Null is rejected, the theory (the alternative) is supported.

Understanding both frameworks is essential for reading and performing modern scientific research. In the coming weeks, we will apply these concepts in code.

---

## Your Tasks

- **Homework 1:** Probability math exercises.
- **Homework 2:** Working with Bayesian distributions.
- **Required Reading:** "The Earth is Round (p < .05)" by Jacob Cohen (1994)—a critical look at the misuse of p-values.
- **Additional Reading:** Stephen Brush's history of statistical mechanics.

This lecture sets the stage for our work in probability and statistics. Next, we will begin coding these concepts in Python.
