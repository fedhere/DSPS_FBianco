# Lecture 7: Clustering – Unsupervised Learning

**Instructor:** Dr. Federica Bianco

This lecture introduces clustering, a core unsupervised learning technique. We will explore what clustering is, why it matters, the critical role of distance metrics and data preprocessing, and a taxonomy of clustering algorithms including partitioning (k-means, EM), density-based (DBSCAN), and hierarchical methods.

---

## 1. What is Clustering?

Clustering is an **unsupervised learning** method. Unlike supervised learning, where we have labeled data to train a model, clustering seeks to discover the inherent structure in unlabeled data.

- **Goal:** Partition data into **maximally homogeneous** groups (clusters) that are **maximally distinguished** from each other.
- **Key Idea:** Members of a cluster should be similar (intra-cluster compactness), while objects outside a cluster should be dissimilar (inter-cluster separation).
- **The Challenge:** The "optimal" clustering depends entirely on how you define similarity and the purpose of your analysis. A zoologist and a photographer will cluster animals very differently.

---

## 2. Distance Metrics: The Foundation of Similarity

All clustering algorithms require a definition of "distance" or "similarity." A valid distance metric must satisfy:
- $D(i,j) > 0$ (non-negativity)
- $D(i,j) = D(j,i)$ (symmetry)
- $D(i,j) \le D(i,k) + D(k,j)$ (triangle inequality)
- $D(i,i) = 0$

### For Continuous Variables: The Minkowski Family

The Minkowski distance of order $p$ between two points $i$ and $j$ is:
$$D(i,j) = \left( \sum_{k=1}^{N} |x_{ik} - x_{jk}|^p \right)^{1/p}$$

- **Manhattan Distance ($p=1$):** $D_{Man} = \sum |x_{ik} - x_{jk}|$ (good for high-dimensional spaces).
- **Euclidean Distance ($p=2$):** $D_{Euc} = \sqrt{\sum (x_{ik} - x_{jk})^2}$ (the most common, but sensitive to scale).

### For Categorical (Binary) Variables

Define a contingency table for features:

| | 1 (present in j) | 0 (absent in j) | sum |
| :--- | :--- | :--- | :--- |
| **1 (present in i)** | $M_{11}$ | $M_{10}$ | $M_{11}+M_{10}$ |
| **0 (absent in i)** | $M_{01}$ | $M_{00}$ | $M_{01}+M_{00}$ |
| **sum** | $M_{11}+M_{01}$ | $M_{10}+M_{00}$ | $M_{11}+M_{00}+M_{01}+M_{10}$ |

- **Simple Matching Distance (SMD):** $SMD = 1 - \frac{M_{11} + M_{00}}{M_{11}+M_{10}+M_{01}+M_{00}}$ (treats 0-0 matches as similar).
- **Jaccard Distance:** $D_{Jaccard} = 1 - \frac{M_{11}}{M_{11} + M_{01} + M_{10}}$ (ignores 0-0 matches; common in biology and text mining).

---

## 3. Scaling and Whitening: Preprocessing is Essential

Most distance metrics are sensitive to the scale of the features. Without preprocessing, the feature with the largest spread will dominate the clustering.

### Scaling (Normalization)
Adjusts the range or variance of features to ensure they contribute equally.

- **Min-Max Scaling:** $X' = \frac{X - \min(X)}{\max(X) - \min(X)}$ (maps to [0,1]).
- **Standardization (Z-score):** $X' = \frac{X - \mu(X)}{\sigma(X)}$ (maps to mean=0, std=1). **This is the most common choice for clustering.**

### Whitening (Sphering)
Removes **correlations** between features. It transforms the data so that the covariance matrix becomes the identity matrix. While powerful, it can be computationally expensive or unstable for high-dimensional data.

---

## 4. A Taxonomy of Clustering Algorithms

We can categorize clustering algorithms by how they partition the data.

| **Category** | **Methods** | **Key Characteristics** |
| :--- | :--- | :--- |
| **Partitioning** | k-means, k-medoids, EM | Divides data into a pre-specified number of clusters ($k$). Iteratively refines clusters. |
| **Density-based** | DBSCAN, OPTICS | Defines clusters as dense regions separated by sparse regions. Can find arbitrary shapes and identify noise. |
| **Hierarchical** | Agglomerative (bottom-up), Divisive (top-down) | Creates a tree (dendrogram) of clusters. Does not require specifying $k$ upfront. |

---

## 5. Partitioning Methods

### k-means (Hard Clustering)
Each point belongs to exactly one cluster.

**Algorithm:**
1.  Choose $k$ initial cluster centers (centroids) randomly.
2.  **Assignment:** Assign each data point to the nearest centroid.
3.  **Update:** Recalculate centroids as the mean of all points in the cluster.
4.  Repeat steps 2-3 until convergence (centroids no longer move).

**Objective:** Minimize the total intra-cluster variance (sum of squared distances to the centroid).

**Pros:** Scales linearly $O(k \cdot d \cdot N)$; simple and fast.
**Cons:** Must choose $k$ upfront; sensitive to initialization; assumes clusters are spherical and of similar size; only works when the mean is defined.

**Choosing $k$: The Elbow Method** – Run k-means for multiple $k$ and plot the total intra-cluster variance. The "elbow" (bend) in the curve suggests a good $k$.

### Expectation-Maximization (EM) for Gaussian Mixture Models (Soft Clustering)
Assigns a **probability** that each point belongs to each cluster (e.g., it's 70% likely to be in Cluster A, 30% in Cluster B).

**Algorithm:**
1.  Initialize parameters for $k$ Gaussian distributions ($\mu_j, \sigma_j$).
2.  **E-step (Expectation):** For each point, calculate the probability of belonging to each Gaussian (using Bayes' theorem).
3.  **M-step (Maximization):** Update the Gaussian parameters ($\mu_j, \sigma_j$) as the probability-weighted mean and variance of the points.
4.  Repeat until convergence.

**Pros:** Provides probabilistic memberships; can model clusters as ellipses (not just spheres) by allowing full covariance matrices.
**Cons:** Must choose $k$ and a distribution shape; more computationally intensive than k-means.

---

## 6. Density-Based Clustering: DBSCAN

DBSCAN (Density-Based Spatial Clustering of Applications with Noise) is one of the most widely used clustering algorithms, particularly in the natural sciences.

**Key Concepts:**
- **Core Point:** A point with at least `minPts` neighbors within a distance $\epsilon$.
- **Directly Reachable:** A point is directly reachable from a core point if it is within $\epsilon$.
- **Reachable:** A point is reachable if there is a chain of directly reachable points connecting it to a core point.
- **Outlier (Noise):** Any point that is not reachable from any core point.

**Algorithm:**
1.  Identify all core points.
2.  Form clusters by connecting core points and their reachable neighbors.
3.  Label all remaining points as noise/outliers.

**Pros:**
- Does not require specifying the number of clusters ($k$).
- Can find arbitrarily shaped clusters.
- Robust to noise and can identify outliers.

**Cons:**
- Highly sensitive to the hyperparameters $\epsilon$ and `minPts`.
- Struggles with clusters of varying densities.

---

## 7. Hierarchical Clustering

Hierarchical clustering builds a tree of clusters, called a **dendrogram**, which allows you to choose the number of clusters by cutting the tree at a desired level.

### Agglomerative (Bottom-up) – *Deterministic*
- **Algorithm:** Start with each point as its own cluster. Iteratively merge the two *closest* clusters until all points are in one cluster.
- **Linkage Criteria:** Defines how "distance between clusters" is measured.
    - **Single Linkage:** $D = \min d(a,b)$ (minimum distance between points in clusters; tends to produce "chaining").
    - **Complete Linkage:** $D = \max d(a,b)$ (maximum distance; tends to produce compact clusters).
    - **Average Linkage:** $D = \text{mean} \, d(a,b)$.
    - **Ward's Method:** Minimizes the total intra-cluster variance (like k-means).

### Divisive (Top-down) – *Non-deterministic*
- **Algorithm:** Start with all points in one cluster. Recursively split the cluster into two (often using k-means) until each point is its own cluster.
- **Complexity:** Exhaustive search is $O(2^N)$, but can be approximated.

---

## 8. Key Takeaways

- **Clustering** is unsupervised learning for discovering groups in unlabeled data.
- **Distance metrics** (Euclidean, Manhattan, Jaccard) define similarity and are algorithm-dependent.
- **Scaling** (standardization) is **mandatory** before clustering to prevent features with large ranges from dominating.
- **Algorithm Choice:**
    - **k-means:** Fast, but requires $k$ and assumes spherical clusters.
    - **EM:** Soft clustering, can model ellipsoidal clusters, but requires $k$ and a distribution assumption.
    - **DBSCAN:** Finds arbitrary shapes, robust to noise, but sensitive to parameters.
    - **Agglomerative:** Provides a dendrogram, deterministic, but computationally expensive.

- **Validation:** There is no single "right" clustering. Interpretation requires domain knowledge and careful consideration of the objective.

---

## Your Tasks

- **Reading:** "Data Clustering: A Review" by Jain, Murty, and Flynn (1999).
- **Homework:** Apply these clustering methods (k-means, DBSCAN, hierarchical) to a dataset of your choice. Explore the impact of scaling and different distance metrics on the results.

Clustering is a powerful tool for exploratory data analysis. In the next lectures, we will move on to supervised learning methods for classification and regression.
