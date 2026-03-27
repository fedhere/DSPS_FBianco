# Lecture 11: Convolutional Neural Networks – Learning from Images

**Instructor:** Dr. Federica Bianco

This lecture builds on our introduction to neural networks and focuses on Convolutional Neural Networks (CNNs), a specialized architecture designed for processing grid-like data, most notably images. We will explore the core operations that make CNNs so powerful—convolution, ReLU activation, and pooling—and discuss strategies for training deep networks effectively.

---

## 1. Recap: From Perceptrons to Deep Networks

Before diving into CNNs, let's quickly recap the fundamentals.

- **Perceptron:** The basic building block. It computes a weighted sum of inputs, adds a bias, and passes the result through a non-linear activation function: $y = f(\sum_i w_i x_i + b)$.
- **Multi-Layer Perceptron (MLP):** Stacks multiple layers of perceptrons. With enough neurons and layers, it can approximate any function, making it a universal approximator. However, for image data, this fully connected architecture quickly becomes impractical due to the enormous number of parameters (e.g., a 100x100 image would have 10,000 inputs, leading to millions of weights in the first hidden layer alone).

### Optimization: Gradient Descent
We train neural networks by minimizing a loss function (e.g., $L_2 = \frac{1}{N}\sum (y_i - \hat{y}_i)^2$) using gradient descent. The update rule for a parameter (like weight $w$) is:

$w_{\text{new}} = w_{\text{old}} - \frac{\partial f}{\partial w} \cdot \frac{\alpha}{N}$

- $\alpha$: **learning rate** (a crucial hyperparameter).
- $\frac{\partial f}{\partial w}$: the partial derivative of the loss function with respect to $w$.

---

## 2. The Inspiration: The Visual Cortex

Research in neuroscience shows that the visual cortex processes information hierarchically:

1.  **Simple Features:** Early layers detect simple patterns like edges, lines, and corners.
2.  **Complex Features:** Deeper layers combine these simple features to detect more complex structures like shapes, textures, and eventually whole objects (e.g., eyes, faces, cars).

CNNs are designed to mimic this hierarchical learning process, making them exceptionally good at tasks like image classification, object detection, and segmentation.

---

## 3. The Core Operations of a CNN

A CNN is composed of several key layers stacked together. The first layers are **not** fully connected; instead, they use three specialized operations.

### 3.1 Convolutional Layer
This is the heart of a CNN. Instead of connecting every input pixel to every neuron, we slide a small **filter** (or kernel) across the image.

- **Filter:** A small matrix of weights (e.g., 3x3, 5x5) that acts as a feature detector.
- **Convolution Operation:** At each position, the filter performs an element-wise multiplication with the overlapping image pixels and sums the results. This produces a single number.
- **Feature Map:** Sliding the filter across the entire image produces a new 2D grid of numbers, called a feature map. Each filter generates one feature map, representing the presence of a specific feature (e.g., a vertical edge) at each location.

**Key Idea:** The same filter is used across the entire image. This is **weight sharing**, which drastically reduces the number of parameters compared to a fully connected layer and makes the network translation-invariant (a feature detected in one part of the image can be detected anywhere).

### 3.2 Activation Function (ReLU)
After convolution, an activation function is applied element-wise to introduce non-linearity. The most common choice is the **Rectified Linear Unit (ReLU):**

$f(z) = \max(0, z)$

ReLU sets all negative values to zero. This makes the network sparse (many neurons are "off") and helps mitigate the vanishing gradient problem, allowing for deeper networks.

### 3.3 Pooling Layer
Pooling layers reduce the spatial dimensions (width and height) of the feature maps. This serves several purposes:
- **Reduces Computational Load:** Fewer parameters for subsequent layers.
- **Increases Translation Invariance:** Small shifts or distortions in the input have a smaller effect on the pooled output.
- **Controls Overfitting:** By summarizing local regions, it provides a form of regularization.

The most common type is **Max Pooling**, which takes the maximum value from a small window (e.g., 2x2) and discards the others.

---

## 4. Putting It All Together: A CNN Architecture

A typical CNN architecture stacks these layers to build a hierarchy of features.

1.  **Input Layer:** The raw image (e.g., 28x28 pixels).
2.  **Convolutional Layer + ReLU:** Applies several filters to detect basic features (edges, colors). Output: multiple feature maps.
3.  **Pooling Layer:** Reduces the size of the feature maps.
4.  **More Convolution + Pooling Layers:** As we go deeper, the network learns more complex, abstract features. The number of filters (feature maps) often increases, while the spatial dimensions decrease.
5.  **Flatten Layer:** Converts the final 2D feature maps into a 1D vector.
6.  **Fully Connected (Dense) Layers:** One or more traditional MLP layers that use the extracted high-level features to perform the final classification (e.g., "cat" vs. "dog").
7.  **Output Layer:** Uses a **softmax** activation function to output a probability distribution over the target classes.

---

## 5. Training Deep CNNs: Challenges and Solutions

Training deep networks presents unique challenges that must be addressed.

### Overfitting
With millions of parameters, CNNs are prone to overfitting, where they memorize the training data but fail to generalize to new data.

- **Solution 1: Minibatch Gradient Descent.** Instead of calculating the gradient on the entire training set (slow) or on a single example (noisy), we use **minibatches**. We split the training data into small subsets (e.g., 32 or 64 images), calculate the loss and gradient on the batch, and update the parameters. This is efficient and provides a stable training signal.
- **Solution 2: Dropout.** During training, a fraction of neurons (e.g., 50%) are randomly "dropped out" (set to zero) for each minibatch. This prevents neurons from co-adapting too much and forces the network to learn more robust features. Dropout is a powerful regularizer.

### Class Imbalance
If the dataset has many more examples of one class than another (e.g., 90% "dog", 10% "cat"), the network can learn to simply always predict the majority class and achieve 90% accuracy. This is a failure. Solutions include:
- **Weighted Loss:** Giving higher weight to the minority class in the loss function.
- **Resampling:** Oversampling the minority class or undersampling the majority class.

---

## 6. Key Takeaways

- **CNNs** are specialized neural networks designed for grid-like data (images, time series, spectra) that learn hierarchical features.
- **Convolution** uses weight-sharing filters to detect features efficiently and translation-invariantly.
- **ReLU** introduces non-linearity and helps with training stability.
- **Pooling** reduces dimensionality and provides robustness to small spatial shifts.
- **Deep CNN architectures** stack many convolutional, pooling, and fully connected layers.
- **Training deep networks** requires careful techniques like **minibatch gradient descent** and **dropout** to prevent overfitting and ensure convergence.
- **Class imbalance** is a critical issue that must be addressed through weighting or resampling to avoid a biased, useless model.

---

## Your Tasks

- **Homework:** Participate in the Galaxy Zoo challenge on Kaggle, applying the CNN concepts you've learned to classify galaxy morphologies from images.
- **Resources:** Explore the Keras/TensorFlow documentation to build and train your own CNN architectures.

CNNs are a cornerstone of modern computer vision. In the next lecture, we will explore generative models and how they can be used to create new, realistic data.
