# Lecture 10: Artificial Neural Networks – From Perceptrons to Deep Learning

**Instructor:** Dr. Federica Bianco

This lecture introduces artificial neural networks (ANNs), a powerful class of machine learning models inspired by the brain. We will trace their history from the simple perceptron to modern deep neural networks, explore their architecture, and understand the key concepts of learning and optimization.

---

## 1. Recap: Machine Learning Fundamentals

Before diving into neural networks, let's situate them within the broader ML landscape.

- **Supervised Learning:** We have labeled data. The goal is to learn a mapping from inputs to outputs for prediction (regression/classification). ANNs are a core supervised method.
- **Unsupervised Learning:** All features are observed; the goal is to find hidden structure (clustering, dimensionality reduction).
- **Loss Function:** The metric that quantifies how well the model is performing. Learning is the process of minimizing this loss (e.g., $L_1$ or $L_2$ error).
- **Distance:** The definition of similarity (Euclidean, Manhattan, Jaccard) is fundamental to defining loss.
- **Overfitting:** A model that fits the training data too closely (memorizing noise) and generalizes poorly. Controlled via hyperparameters (e.g., tree depth, network architecture, regularization).
- **Class Imbalance:** When one class dominates the dataset, models can learn to simply predict the majority class. This requires careful handling (e.g., weighting, resampling).

---

## 2. The Origins: The McCulloch-Pitts Neuron (1943)

The first mathematical model of a neuron was proposed by Warren McCulloch and Walter Pitts.

- **Concept:** A neuron receives multiple inputs ($x_i$), sums them, and "fires" (outputs 1) if the sum exceeds a threshold ($\theta$); otherwise, it does not fire (outputs 0).
- **Logic:** $y = 1 \text{ if } \sum_i x_i \geq \theta, \text{ else } 0$.
- **AND/OR:** This simple model could perform logical operations like AND and OR. However, it famously could not solve the XOR problem (non-linearly separable).

---

## 3. The Perceptron (1958)

Frank Rosenblatt's perceptron introduced the idea of **learning** and remains the foundation of modern neural networks.

### Architecture
A single-layer perceptron computes a weighted sum of its inputs, adds a bias, and passes the result through an **activation function**.

- **Formula:** $y = f(\sum_i w_i x_i + b)$
    - $w_i$: **weights** (parameters) that determine the sensitivity of the neuron.
    - $b$: **bias** (parameter) that shifts the activation threshold.
    - $f$: **activation function** that introduces non-linearity.

### Activation Functions
These functions are crucial for enabling networks to learn complex patterns.

- **Step Function:** The original M-P neuron activation (0 or 1). Used for binary classification.
- **Sigmoid:** $\sigma(z) = 1 / (1 + e^{-z})$. Smooth, differentiable, outputs between 0 and 1. Common in early perceptrons.
- **ReLU (Rectified Linear Unit):** $f(z) = \max(0, z)$. The current default for deep networks. It is simple, computationally efficient, and helps mitigate vanishing gradients.
- **Tanh:** Outputs between -1 and 1; similar to sigmoid but zero-centered.

### The Widrow-Hoff Rule (1962)
The perceptron learns by updating its weights based on the error: `Weight Change = (Pre-Weight Line Value) * (Error / Number of Inputs)`. This is an early form of gradient descent.

### The Limitation
A single-layer perceptron can only learn **linearly separable** patterns. It cannot solve XOR. This limitation, highlighted in the 1969 book *Perceptrons*, led to a temporary decline in interest in neural networks.

---

## 4. Deep Learning: The Multi-Layer Perceptron (MLP)

The key to solving non-linear problems was to stack multiple layers of perceptrons, creating the **Multi-Layer Perceptron (MLP)** or deep neural network.

### Architecture
- **Input Layer:** The raw features ($x_1, x_2, ..., x_N$).
- **Hidden Layers:** One or more layers of neurons, each fully connected to the next layer. These layers learn increasingly abstract representations of the data.
- **Output Layer:** Produces the final prediction (e.g., class probabilities, a continuous value).

### The Power of Depth
Each layer applies a linear transformation (weights + bias) followed by a non-linear activation function. The composition of these layers allows the network to approximate any complex, non-linear function.

---

## 5. Training Deep Networks: Backpropagation

Training a network with millions of parameters requires a systematic method to adjust all weights and biases. This is achieved through **backpropagation**.

1.  **Forward Pass:** Data is fed through the network, layer by layer, to compute the prediction ($\hat{y}$).
2.  **Loss Calculation:** A cost function (e.g., $C = \frac{1}{2} \sum (y - \hat{y})^2$) measures the error.
3.  **Backward Pass:** The algorithm calculates the gradient of the cost function with respect to every weight and bias in the network, using the **chain rule** of calculus. This shows how much each parameter contributed to the error.
4.  **Parameter Update:** The parameters are updated by taking a small step in the direction that reduces the error (gradient descent).

### Common Pitfalls
- **Vanishing Gradient:** Gradients become extremely small as they are propagated back through many layers, especially with sigmoid or tanh activations. This causes the early layers to learn very slowly or not at all. ReLU activations help mitigate this.
- **Exploding Gradient:** Gradients become extremely large, causing numerical instability and wildly erratic updates.

---

## 6. Neural Networks in Context

### Hyperparameters
The number of hyperparameters is vast, including:
- **Architecture:** Number of layers, number of neurons per layer, activation functions, connectivity.
- **Training:** Loss function, optimization algorithm (e.g., Adam, SGD), learning rate, number of epochs, batch size.
- **Regularization:** Techniques to prevent overfitting (e.g., dropout, weight decay).

### Interpretability
Deep networks are often called "black boxes" because it is difficult to extract simple feature importances or interpret the learned representations directly. This is acceptable for tasks where predictive performance is the primary goal, but it is a significant concern in high-stakes domains (e.g., healthcare, criminal justice) where explainability is critical.

### DeepDream
Google's DeepDream algorithm visualizes what a network has learned. By running an image through a trained network and then modifying the image to maximize the activation of certain layers, the network "manifests" the features it has learned to recognize (e.g., dog faces, eyes, patterns), creating surreal, dream-like images. This illustrates the hierarchical feature learning (from simple edges to complex objects) that occurs in deep networks.

---

## 7. Key Takeaways

- **Perceptrons** are the building blocks of neural networks, combining weighted inputs, a bias, and a non-linear activation function.
- **Single-layer perceptrons** are limited to linear problems.
- **Multi-layer perceptrons (deep networks)** overcome this limitation by stacking layers, allowing them to learn complex, non-linear representations.
- **Backpropagation** is the algorithm that efficiently computes gradients and enables training of deep networks.
- **Activation functions** like ReLU are crucial for enabling deep learning and mitigating issues like vanishing gradients.
- While powerful, deep networks can be **uninterpretable** and require careful tuning to avoid **overfitting** and other training pathologies.

---

## Your Tasks

- **Homework:** Build and train a multi-layer perceptron for classification (e.g., on the MNIST digits dataset). Experiment with different numbers of hidden layers, neurons, and activation functions. Monitor training and validation loss to diagnose overfitting.
- **Resources:** *Neural Networks and Deep Learning* by Michael Nielsen (a free, accessible online book) and *Deep Learning* by Goodfellow, Bengio, and Courville (a more advanced reference).

Neural networks are a cornerstone of modern AI. In the next lectures, we will explore specialized architectures like Convolutional Neural Networks (CNNs) for image data and Generative Adversarial Networks (GANs) for generating new data.
