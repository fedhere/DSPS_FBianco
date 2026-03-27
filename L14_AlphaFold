# Lecture 14: AlphaFold & Fine-Tuning Foundation Models

**Instructor:** Dr. Federica Bianco

This final lecture brings together many of the concepts we've explored throughout the semester—from statistical inference to deep learning—and shows how they are applied in one of the most significant scientific breakthroughs of recent years: AlphaFold. We will also discuss the practical and ethical dimensions of adapting large, pre-trained models to new tasks through fine-tuning.

---

## 1. Final Exam Reminder

Before we dive in, a quick reminder about the final exam:

- **Written Final:** 72-hour take-home (Dec 11 8AM – Dec 14 8AM). Work alone. AI use is at your own risk; you are responsible for your work.
- **Oral Final:** A 30-minute individual session where you will explain and defend your choices.

**Key tasks for the exam:**
1.  **Get your data** (reproducibly!).
2.  **Explore your data** (datatypes, missing values, distributions, class balance, correlations).
3.  **Prepare your data** (scaling, imputation, encoding).
4.  **Build your model** (justify hyperparameters).
5.  **Train your model**.
6.  **Assess performance** (with appropriate metrics).
7.  **Compare models** (if applicable).

**Crucially:** All steps requiring a "**" need justification, explanation, and interpretation. All plots need captions that describe the **what**, the **how**, and the **"wow"**. Exploratory data analysis is critical—it guides every subsequent decision.

---

## 2. The Protein Folding Problem

Proteins are polymers of amino acids. Their sequence (encoded in DNA) determines how they fold into a three-dimensional structure. That structure, in turn, determines their biological function (e.g., transporting oxygen, signaling, catalyzing reactions).

**Why does structure matter?** Because structure is function. Understanding protein folding is essential for drug design, understanding diseases, and engineering new proteins.

**The Challenge (Levinthal's Paradox):** A protein could theoretically adopt an astronomical number of conformations. Searching them all would take longer than the age of the universe. Yet proteins fold in milliseconds.

**The Traditional Approach:** Experimental structure determination (X-ray crystallography, cryo-EM) is slow, expensive, and not always possible. By 2014, only ~140,000 protein structures had been solved.

---

## 3. AlphaFold: A Deep Learning Revolution

AlphaFold, developed by DeepMind, uses deep learning to predict protein structure from amino acid sequence alone. It was a breakthrough in the biennial Critical Assessment of Protein Structure Prediction (CASP) competition.

### Key Innovations

**1. Input: Multiple Sequence Alignment (MSA)**
Instead of using a single sequence, AlphaFold takes a **Multiple Sequence Alignment** of evolutionarily related proteins. If two amino acids co-vary across species (i.e., when one changes, the other tends to change in a correlated way), it suggests they are close together in the folded structure (co-evolution).

**2. Evoformer Architecture**
The Evoformer block processes both the MSA (a 3D tensor of shape `[N_seq, N_res, c_m]`) and a pairwise feature tensor (`[N_res, N_res, c_z]`). It exchanges information between these representations, enabling reasoning about both evolutionary relationships and spatial proximity. Key innovations include new mechanisms to share information between the MSA and pair representations.

**3. Structure Module**
Instead of directly predicting 3D coordinates, AlphaFold first predicts **intermediate representations**:
- **Distogram:** A matrix of pairwise distances between every residue (the "fold topology").
- **Torsion Angles (phi, psi):** The internal rotation angles of the protein backbone.

The structure module then uses these to build an explicit 3D structure, representing each residue's position with a rotation and translation (a global rigid body frame). This module incorporates physical and geometric constraints.

**AlphaFold2 (2020)** used a transformer-like architecture with these innovations, achieving near-experimental accuracy. **AlphaFold3** introduced a **diffusion model** (generative AI) to predict the 3D coordinates directly, further improving accuracy and enabling predictions of complexes (multiple interacting proteins and molecules).

### Impact and Costs
- **Impact:** AlphaFold has predicted structures for over 200 million proteins, democratizing structural biology. It has enabled new science in areas from drug discovery to understanding disease mutations.
- **Cost:** Training AlphaFold 2 cost an estimated $1–2 million in direct compute. While significant, this is an order of magnitude less than training the largest large language models (GPT-3 cost ~$4–5 million; GPT-4 likely >$100 million).

### Limitations
- Predicting multiple conformations (dynamics) is challenging.
- Thermodynamic properties are not predicted.
- It struggles with **disordered regions**, which make up about one-third of eukaryotic proteins.

---

## 4. Foundation Models & Fine-Tuning

Training a model like AlphaFold or GPT-4 from scratch is prohibitively expensive for most research groups. **Foundation models**—large models pre-trained on vast, general datasets—offer a solution. We can **fine-tune** them for our specific task with much less data and computation.

### The Concept
A foundation model is pre-trained (often with self-supervision) on a massive dataset. This pre-training teaches the model low-level features that are broadly useful (e.g., edges and shapes for vision models, syntax for language models). Fine-tuning adapts the model to a new, often smaller, dataset.

### How Fine-Tuning Works
1.  **Start with a pre-trained model:** Load the saved weights and biases.
2.  **"Chop the Head":** Remove the original prediction head (e.g., the final classification layer) and replace it with a new head suited to your task (e.g., a new classifier with a different number of classes, or a regressor).
3.  **Freeze Early Layers:** Set the weights of the early, general-feature layers as **non-trainable** (frozen). These have already learned useful features and do not need to be adjusted.
4.  **Retrain the Head and Later Layers:** Train only the new head and possibly the last few layers for a few epochs (often 10-20). Because the early layers are frozen, training is much faster and requires far less data.

### Fine-Tuning in Code (PyTorch/Keras)
- In Keras: `layer.trainable = False`
- In PyTorch: `parameter.requires_grad = False`

### Why Fine-Tune?
- **Small Labeled Datasets:** Leverage knowledge from large datasets where labels are scarce for your specific task.
- **Limited Computational Resources:** Fine-tuning is orders of magnitude cheaper than pre-training.
- **Environmental Impact:** Reusing pre-trained models reduces the carbon footprint of AI research.

---

## 5. Ethical Considerations: Bias in Data

A model is only as good—and as unbiased—as the data it is trained on. The Faces95 dataset example shows a stark imbalance: 43 White males, 6 White females, 12 non-White males, 1 non-White female. A model trained on this data will not generalize to other demographics, and its predictions may be actively harmful.

**The lesson:** Data bias leads to model bias, which leads to non-transferable, potentially unjust systems. We have a responsibility to examine our data, understand its limitations, and when using foundation models, be aware of the biases embedded in their pre-training data.

---

## 6. Course Recap: What We've Learned

Over the semester, we have built a foundation in data science for the physical sciences:

- **Epistemology:** Falsifiability, reproducibility, parsimony.
- **Probability & Statistics:** Distributions, NHRT, Bayes' theorem, descriptive statistics, central limit theorem.
- **Uncertainty:** Statistical vs. systematic errors, error propagation, Monte Carlo methods.
- **Machine Learning:**
    - **Unsupervised:** Clustering (k-means, DBSCAN, hierarchical).
    - **Supervised:** Regression (linear, polynomial), classification (trees, forests, gradient boosting), neural networks (perceptrons, MLPs, CNNs, transformers).
    - **Generative:** Autoencoders, GANs, diffusion models.
    - **Advanced:** Physics-Informed Neural Networks (PINNs).
- **Visualization:** Principles of honest, clear, and effective scientific communication.
- **Ethics:** The responsibility to consider bias, environmental impact, and societal consequences of our models.

---

## 7. Final Takeaways

- **AlphaFold** exemplifies how deep learning, when combined with domain knowledge (evolution, physics), can solve grand scientific challenges.
- **Foundation models** and **fine-tuning** democratize access to powerful AI, but require careful consideration of bias and transferability.
- The **data science workflow** is iterative: from problem formulation to data exploration, modeling, evaluation, and deployment. Every step requires justification and interpretation.
- As data scientists, our skills are powerful tools. We must wield them with **critical thinking, honesty, and a commitment to ethical practice.**

Thank you for a great semester. Good luck on your finals!
