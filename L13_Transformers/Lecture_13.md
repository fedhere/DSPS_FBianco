# Lecture 13: Transformers – Attention is All You Need

**Instructor:** Dr. Federica Bianco

This lecture introduces Transformers, a neural network architecture that has revolutionized natural language processing and is increasingly applied to time series and scientific data. We will explore the limitations of previous architectures, the attention mechanism, and the encoder-decoder structure that makes Transformers so powerful.

---

## 1. Recap: From MLPs to CNNs to RNNs

Before Transformers, we had several architectures for sequential data:

- **Multi-Layer Perceptrons (MLPs):** Fully connected networks that treat inputs as independent. They have no notion of sequence or order.
- **Convolutional Neural Networks (CNNs):** Learn local relationships (e.g., nearby pixels) but cannot capture long-range dependencies. The receptive field grows only with depth.
- **Recurrent Neural Networks (RNNs):** Process sequences step-by-step, maintaining a hidden state. They are designed for time series but suffer from the **vanishing gradient problem** (Bengio et al., 1994), making it difficult to learn long-range dependencies (e.g., connections between words far apart in a sentence). LSTMs provided a partial solution but still faced limitations.

---

## 2. What Do We Want from a Sequence Model?

To handle language or time series effectively, a model needs to:

- **Respect Sequential Nature:** Understand that order matters ("Willow talked to Federica" vs. "Federica talked to Willow").
- **Capture Long-Range Context:** Remember relationships across long distances (e.g., the subject of a sentence 20 words earlier).
- **Understand Context-Dependent Semantics:** Recognize that a word's meaning changes based on surrounding words (e.g., "bank" in "river bank" vs. "savings bank").
- **Train in Parallel:** To scale to massive datasets, the architecture must allow for parallel processing, unlike RNNs which are inherently sequential.

**None of the earlier architectures satisfied all these requirements.** This motivated the development of the Transformer.

---

## 3. The Transformer: Attention is All You Need

Introduced by Vaswani et al. in 2017, the Transformer relies solely on an **attention mechanism** to draw global dependencies between input and output. It eliminates recurrence and convolution entirely.

### The Attention Mechanism

Attention computes a weighted sum of values, where the weights are determined by the compatibility of a query with keys.

- **Query (Q):** What we are looking for (e.g., the current word).
- **Key (K):** What we have (e.g., all other words in the sequence).
- **Value (V):** The information we retrieve.

The attention output is: $\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$

This allows the model to focus on relevant parts of the input when producing each output. For a sentence, it can directly relate a word to another word far away, regardless of distance.

### Multi-Head Attention

Instead of a single attention function, Transformers use **multiple attention heads** running in parallel. Each head learns different types of relationships (e.g., one head may learn syntactic dependencies, another semantic associations). Their outputs are concatenated and projected.

### Positional Encoding

Because attention has no built-in notion of order, we must inject information about the position of tokens. This is done by adding **positional encodings** to the input embeddings. These can be fixed sine/cosine functions or learned embeddings, allowing the model to distinguish between "Willow talked to Federica" and "Federica talked to Willow."

---

## 4. The Transformer Architecture

The Transformer follows an encoder-decoder structure, both composed of a stack of identical layers.

### Encoder
- **Input:** Token embeddings + positional encodings.
- **Multi-Head Self-Attention:** Each token attends to all tokens in the input sequence (including itself). This builds contextualized representations.
- **Feed-Forward Network:** A position-wise MLP applied to each token independently.
- **Residual Connections & Layer Normalization:** Applied after each sub-layer to stabilize training.

### Decoder
The decoder is similar but with two key differences:
1.  **Masked Self-Attention:** When generating output, the decoder cannot look at future tokens (to prevent cheating). It uses a mask to zero out attention to future positions.
2.  **Encoder-Decoder Attention:** Queries come from the decoder, while keys and values come from the encoder output. This allows the decoder to focus on relevant parts of the input sequence.

The final output is passed through a linear layer and softmax to produce probabilities over the vocabulary (for language) or target values (for time series).

---

## 5. Beyond Language: Time Series and Vision

### Time Series
Transformers are well-suited for time series because they can:
- Capture complex patterns (trends, seasonality, irregular events) without being limited by a fixed window.
- Model relationships between any two time points, regardless of distance.
- Be trained in parallel, unlike RNNs.

### Vision Transformers (ViT)
Images can be treated as sequences by dividing them into fixed-size **patches** (e.g., 16x16 pixels). These patches are flattened and linearly projected to create token embeddings, similar to word embeddings. A special `[CLS]` token is added to the sequence, and its final representation is used for classification. With positional encodings added, a standard Transformer encoder can achieve state-of-the-art results on image classification tasks.

---

## 6. Ethical Considerations: The Dangers of Stochastic Parrots

The power of large language models (LLMs) like GPT-3/4 comes with significant risks, as highlighted by Bender, Gebru, et al. (2021).

- **Environmental Cost:** Training large models can emit hundreds of tons of CO₂, disproportionately affecting marginalized communities already vulnerable to climate change.
- **Financial Cost:** The immense computational resources create barriers to entry, concentrating power in wealthy corporations and limiting who can contribute to AI research.
- **Encoding Bias:** Models trained on internet data inherit and amplify societal biases. GPT-3 has been shown to encode racist, sexist, and other harmful stereotypes. Because the data over-represents certain demographics (e.g., younger, Western, male), the models do not represent the diversity of human experience.
- **Harms from Ersatz Fluency:** When humans encounter coherent-seeming model output, they may mistake it for the words of a person or organization with accountability. This can lead to the spread of misinformation, extremist ideology, and even wrongful arrest.

**Responsibility:** As data scientists, we must perform risk/benefit analyses, consider who bears the costs and who reaps the benefits, and question whether ever-larger models are the only or best path forward.

---

## 7. Key Takeaways

- **Transformers** use **self-attention** to capture long-range dependencies in sequences without recurrence or convolution.
- **Multi-head attention** allows the model to learn multiple types of relationships in parallel.
- **Positional encoding** injects order information into the model.
- The **encoder-decoder** architecture enables tasks like translation, summarization, and time series forecasting.
- **Vision Transformers** adapt the same principles to image data by treating patches as tokens.
- **Ethical considerations** are paramount: large models carry significant environmental, financial, and social costs, and they can amplify harmful biases.

---

## Your Tasks

- **Reading:** The "Attention is All You Need" paper (Vaswani et al., 2017). "On the Dangers of Stochastic Parrots" (Bender et al., 2021).
- **Resources:** Explore the `transformers` library (Hugging Face) to experiment with pretrained models.
- **Final Exam:** Prepare to discuss the strengths, weaknesses, and ethical implications of different neural network architectures, including Transformers.

Transformers represent a paradigm shift in how we model sequences. Their ability to learn global context has made them the foundation of modern AI, but with that power comes the responsibility to use them thoughtfully and ethically.
