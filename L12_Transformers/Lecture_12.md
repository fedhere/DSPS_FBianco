# Lecture 12: Physics-Informed and Generative Neural Networks

**Instructor:** Dr. Federica Bianco

This lecture concludes our exploration of neural networks by examining two advanced topics: Physics-Informed Neural Networks (PINNs), which integrate physical laws into the learning process, and Generative AI, which learns to create new data. We will also briefly discuss the final exam structure.

---

## 1. Final Exam Information

Before diving into new material, let's review the final exam structure.

- **Written Final:** 72-hour take-home exam (Dec 11, 8AM – Dec 14, 8AM). You must work alone. AI use is permitted but at your own risk; you are responsible for understanding and defending your work.
- **Oral Final:** A 30-minute individual session. You will be asked to explain your choices and demonstrate understanding of your code and the underlying concepts (e.g., "Why did you choose this method? What alternative could you have used?").

---

## 2. Recap: Deep Neural Networks (DNNs)

A deep neural network is a composition of functions:

$\vec{y} = f^{(N)}( \dots (f^{(1)}(\vec{x} \cdot W_1 + \vec{b_1}) \cdot W_2 + \vec{b_2}) \dots )$

- **Parameters:** Weights ($W$) and biases ($b$) are learned from data.
- **Hyperparameters:** Architecture (number of layers, neurons per layer), activation functions, loss function, optimization method, learning rate, batch size.
- **Interpretability:** DNNs are often "black boxes." Understanding why a network made a particular prediction can be difficult.

### Training Challenges
- **Loss Function:** Must match the task. For regression, use `mean_squared_error` with a linear output. For binary classification, use `binary_crossentropy` with a sigmoid output.
- **Learning Curve:** Monitor the loss on the training and validation sets. It should decrease smoothly and flatten out. Jumps can occur (especially with ReLU activations) but are not necessarily a problem. If the validation loss starts increasing while training loss decreases, you are **overfitting**.

---

## 3. Physics-Informed Neural Networks (PINNs)

Traditional machine learning is **data-driven**: we have lots of data and use it to learn associations, often ignoring underlying theory. PINNs offer a hybrid approach for regimes where:

- We have some data, but not enough for purely data-driven methods.
- We have a complex physical theory (e.g., a non-linear partial differential equation) that cannot be solved analytically.

### The Problem: Solving PDEs
Many physical phenomena are described by non-linear partial differential equations (PDEs), which are notoriously hard to solve.

- **Example: Burgers' Equation (with viscosity):**
    $\partial_t u + u \, \partial_x u - (0.01/\pi) \, \partial_{xx} u = 0$
    With boundary conditions: $u(0,x) = -\sin(\pi x)$, $u(t,-1) = u(t,1) = 0$.

### The PINN Solution
PINNs incorporate the physical law directly into the loss function. The network learns to satisfy both the data and the governing equation.

1.  **Network Input:** $(x, t)$ coordinates.
2.  **Network Output:** $u_\theta(x, t)$, the solution at that point.
3.  **Loss Function:** A combination of two terms:
    - **Data Loss:** Standard $L_2$ loss on the (sparse) boundary/initial condition points where the true solution is known.
    - **Physics Loss:** The residual of the PDE. The network's output $u_\theta$ is differentiated using automatic differentiation to compute the PDE residual. The loss encourages this residual to be zero.

$\mathrm{loss} = \sum (u_\theta - u_{\text{true}})^2 + \left( \partial_t u_\theta + u_\theta \, \partial_x u_\theta - \frac{0.01}{\pi} \, \partial_{xx} u_\theta \right)^2$

**Key Advantage:** PINNs can solve forward problems (predict the solution) and inverse problems (discover unknown parameters in the equation) without requiring large, labeled datasets.

---

## 4. Generative AI and Autoencoders

Autoencoders are an unsupervised learning architecture that learns a compressed representation of data.

### Architecture
- **Encoder:** Maps the input $x$ to a lower-dimensional **latent representation** $z$. This is a form of non-linear dimensionality reduction (like PCA but more powerful).
- **Bottleneck:** The layer with the smallest number of neurons, representing the latent space $z$.
- **Decoder:** Maps the latent representation $z$ back to a reconstruction $\hat{x}$ of the original input.

### Training
The network is trained to minimize the reconstruction error (e.g., mean squared error between $x$ and $\hat{x}$). No labels are needed.

**Application:** Once trained, the decoder can generate new data by sampling points from the latent space $z$ and passing them through the decoder.

---

## 5. Generative Adversarial Networks (GANs)

GANs are a powerful framework for generating realistic new data. They consist of two networks trained simultaneously in a **minimax game**.

- **Generator ($G$):** Takes random noise as input and tries to generate realistic-looking data (e.g., images). It wants to fool the discriminator.
- **Discriminator ($D$):** Takes both real data (from the training set) and fake data (from the generator) and tries to correctly classify them as "real" or "fake."

### The Game
- The generator learns to produce increasingly realistic samples.
- The discriminator learns to become a better critic.

### Training Objective (Minimax Loss)
$\min_G \max_D V(D,G) = \mathbb{E}_x [\log D(x)] + \mathbb{E}_z [\log(1 - D(G(z)))]$

- **Generator** tries to **minimize** $\log(1 - D(G(z)))$, making the discriminator think its samples are real.
- **Discriminator** tries to **maximize** its ability to correctly classify real and fake samples.

**Result:** After training, the generator can produce highly realistic, novel samples that are not in the original dataset.

---

## 6. A Spectrum of Generative Models

Different generative models make different assumptions and have different strengths.

- **Autoencoders:** Simple and fast, but outputs can be blurry. They learn a latent space but do not explicitly model its distribution.
- **Variational Autoencoders (VAEs):** Impose a distribution (usually Gaussian) on the latent space, leading to smoother, more structured generation but often still blurry.
- **GANs:** Produce very sharp, realistic outputs. However, they are notoriously difficult to train (mode collapse, non-convergence).
- **Denoising Diffusion Probabilistic Models (DDPMs):** Gradually add noise to data and then learn to reverse the process. They have recently achieved state-of-the-art results in image generation (e.g., DALL-E, Stable Diffusion), surpassing GANs for many tasks.

---

## 7. Transformers

Transformers are a neural network architecture that has revolutionized natural language processing (and is increasingly used in vision and science). Their key innovation is the **attention mechanism**.

- **Attention:** Allows the model to weigh the importance of different parts of the input when making a prediction. For a sentence, it can focus on the most relevant words, regardless of their position.
- **Multi-Head Attention:** Uses multiple attention mechanisms in parallel to capture different types of relationships.
- **Architecture:** Typically an encoder-decoder structure. The encoder processes the input sequence; the decoder generates the output sequence, attending to relevant parts of the encoded input.

**Impact:** Transformers (e.g., GPT, BERT) power most modern large language models and are increasingly used for scientific applications like protein folding (AlphaFold) and climate modeling.

---

## 8. Key Takeaways

- **PINNs** integrate physical laws into the loss function, allowing for data-efficient learning and solving of PDEs.
- **Autoencoders** are unsupervised models that learn a compressed latent representation of data, useful for dimensionality reduction and feature extraction.
- **GANs** use two competing networks to generate highly realistic new data.
- **Transformers** use attention mechanisms to process sequences and have become the dominant architecture for language and beyond.
- **Bias in AI:** Models are not inherently neutral. They can amplify biases present in the training data. Understanding and mitigating these biases is a critical responsibility.

---

## Your Final Tasks

- **Complete the written final exam** within the 72-hour window, working independently.
- **Prepare for the oral final** by reviewing your code and being ready to explain your decisions.
- **Resources:** Michael Nielsen's free online book *Neural Networks and Deep Learning* and Goodfellow et al.'s *Deep Learning* for deeper reference.

This lecture concludes our formal course material. The skills you've developed—from basic probability to deep learning—are the foundation of modern data science. Thank you for a great semester.
