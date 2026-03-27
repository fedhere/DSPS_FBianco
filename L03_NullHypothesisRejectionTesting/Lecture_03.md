# Lecture 3: Scaling Laws & Null Hypothesis Rejection Testing

**Instructor:** Dr. Federica Bianco

This lecture builds on our foundation in probability and statistics to explore two interconnected topics: scaling laws, which reveal underlying physical mechanisms, and Null Hypothesis Rejection Testing (NHRT), the formal framework for making statistical decisions. We will also introduce several common statistical tests and the concept of pivotal quantities.

---

## 1. Recap: The Core Concepts

Before we dive in, let's briefly recap the key ideas from previous lectures:

- **Guiding Principles of Science:** Falsifiability, Reproducibility, and Parsimony.
- **Probability:** The Frequentist interpretation (long-run frequency) vs. Bayesian interpretation (degree of belief).
- **Statistics:** The link between samples and populations. Key concepts include the Central Limit Theorem, descriptive statistics (mean, variance), and common distributions (binomial, Poisson, Gaussian).
- **Physics Connection:** Thermodynamics was the first deliberate application of statistics to physics—using knowledge of microscopic systems statistically to predict macroscopic behavior deterministically.

---

## 2. Scaling Laws: The Power of Power Laws

A scaling law describes how a quantity changes as a function of another, typically through a power law relationship (e.g., $y \propto x^a$).

### Why Are Scaling Laws Important?
The existence of a scaling relationship between physical quantities points to an underlying driving mechanism. They reveal deep connections that transcend specific details.

### Examples Across Disciplines

- **Geometry:** For any shape, area scales as the square of a characteristic length ($A \propto L^2$), and volume scales as the cube ($V \propto L^3$). This is a pure geometric scaling law.

- **Astrophysics (Tully-Fisher Relation, 1977):** For spiral galaxies, the intrinsic luminosity is proportional to the rotational velocity raised to the fourth power. This relationship points to gravity as the dominant force shaping galaxies.

- **Biology (Kleiber's Law, 1932):** The basal metabolic rate of mammals scales as mass to the 3/4 power ($B \propto M^{3/4}$). This was later explained by the fractal nature of nutrient distribution networks (West, Brown, Enquist, 1997).

- **Urban Science (Bettencourt et al., 2007):** Cities are networks that obey scaling laws. Quantities like wealth, innovation, and crime scale superlinearly (faster than linear) with population, while infrastructure scales sublinearly (slower than linear). This reveals universal properties of human social networks.

---

## 3. Null Hypothesis Rejection Testing (NHRT)

This is the core frequentist framework for making decisions based on data. It formalizes the logic of falsification: instead of trying to *prove* a theory true, we try to *falsify* its opposite.

The Null Hypothesis ($H_0$) is often the "non-innovative" or "no-effect" hypothesis. For example:
- "The Earth is flat."
- "Diet alone is as effective as diet and exercise for weight loss."
- "There is no difference in commute times between the old and new bus routes."

The Alternative Hypothesis ($H_1$) is what we hope to support.

### The 6 Steps of NHRT

1.  **Formulate the Null Hypothesis ($H_0$):** State the "status quo" or "no effect" hypothesis clearly.
2.  **Identify All Alternative Outcomes ($H_1$):** Define the set of possibilities that contradict the Null.
3.  **Set a Confidence Threshold ($\alpha$):** Choose the p-value threshold for "reasonable doubt." Common thresholds:
    - $\alpha = 0.05$ (95% confidence, typical in social sciences)
    - $\alpha = 0.003$ (99.7% confidence, common in some physical sciences)
    - $\alpha = 1/(3.5 \text{ million})$ (5σ, the "gold standard" for discovery in particle physics)
4.  **Find a Pivotal Quantity:** Identify a measurable quantity (test statistic) that has a *known distribution* under the Null Hypothesis.
5.  **Calculate the Pivotal Quantity:** Compute its value from your data.
6.  **Make a Decision:**
    - If the probability of observing that value (or a more extreme one) **under the Null** is less than $\alpha$, **reject the Null**. The result is "statistically significant."
    - If the probability is greater than or equal to $\alpha$, **fail to reject the Null**. There is not enough evidence to rule it out.

---

## 4. Pivotal Quantities & Common Tests

A pivotal quantity is a statistic whose distribution does not depend on unknown parameters. This allows us to compute the probability of observing a given value under the Null.

### Tests with a Standard Normal Distribution ($Z \sim N(0,1)$)

- **Z-test (for means):** Used to compare a sample mean ($\bar{X}$) to a known population mean ($\mu_0$) when the population standard deviation ($\sigma_0$) is known.
    - Pivotal Quantity: $Z = \frac{\bar{X} - \mu_0}{\sigma_0 / \sqrt{n}}$
    - The Z-value tells you how many standard deviations the sample mean is from the population mean.

- **Z-test (for proportions):** Used to compare two proportions (e.g., the fraction of women vs. men using a service).
    - Pivotal Quantity: $Z = \frac{p_1 - p_2}{SE}$, where $SE$ is the standard error of the difference.

### Test with a t-Distribution

- **t-test:** Used to compare the means of two samples when the population variances are unknown (a more common scenario). The distribution depends on the degrees of freedom.

### Tests with Other Distributions

- **Kolmogorov-Smirnov (KS) Test:** A non-parametric test that asks: "Do two samples come from the same parent distribution?" The test statistic is the maximum distance between the two cumulative distribution functions ($D = \max_x |C_1(x) - C_2(x)|$). Under the Null, $D$ follows a known distribution (related to the Kolmogorov distribution).

- **Chi-squared ($\chi^2$) Test:** Used to ask: "Is the data consistent with a given model?" It is a measure of the discrepancy between observed ($y_i$) and expected ($f(x_i)$) values, weighted by uncertainties ($\sigma_i$).
    - Pivotal Quantity: $\chi^2 = \sum_i \frac{(y_i - f(x_i))^2}{\sigma_i^2}$
    - Under the Null (that the model is correct), $\chi^2$ follows a $\chi^2$ distribution with a certain number of degrees of freedom.

---

## 5. The Logic of NHRT and Falsification

The process is analogous to a courtroom:
- **Null Hypothesis:** The defendant is innocent.
- **Alternative:** The defendant is guilty.
- **Beyond a reasonable doubt ($\alpha$):** We set a threshold for the strength of evidence required to convict.
- **Evidence (Pivotal Quantity):** We present data.
- **Decision:** If the evidence is strong enough (p-value < $\alpha$), we reject innocence (the Null) and deem the evidence for guilt (the Alternative) as statistically significant.

In science, we aim to reject the Null. The more stringent the threshold (e.g., 5σ), the less likely we are to falsely reject a true Null (a Type I error).

---

## 6. The Road Ahead

We have now covered:
- The theoretical foundation: scaling laws as evidence of mechanism.
- The logical framework: NHRT as a method for falsification.
- The practical tools: Z, t, KS, and $\chi^2$ tests.

In the next sessions, we will apply these concepts in coding exercises, starting with a homework assignment on the KS test to demonstrate scaling laws in earthquake frequency.

---

## Your Tasks

- **Homework:** Reproduce the work of Carrell (2018) using a KS-test to demonstrate the existence of a scaling law in the frequency of earthquakes.
- **Resources:**
    - *Statistics in a Nutshell* (Chapters 3, 4, 5) by Boslaugh & Watters.
    - "Scaling laws in physics" by Jones et al.
    - "Urban Scaling and Its Deviations" by Bettencourt, Strumsky, & West.

This lecture concludes the first major unit on probability and statistical inference. Next, we will start applying these concepts to linear and multiple regression.
