# Lecture 9: Scientific Visualizations – Honesty, Clarity, and Beauty

**Instructor:** Dr. Federica Bianco

This lecture explores the art and science of data visualization. We will examine why visualization is essential, learn from historical masterpieces, understand the principles that separate good graphics from bad, and consider the ethical responsibility we have to present data clearly and honestly.

---

## 1. Why Visualize?

Computers understand data as numbers; **people do not**. Visualization is the critical bridge that transforms raw data into insight.

A powerful example is **Anscombe's Quartet** (1973): four datasets with nearly identical statistical properties (mean, variance, correlation) look radically different when plotted. The moral: **always look at your data.**

Modern challenges include:
- **Too many points:** Overplotting obscures structure. Solutions include subsampling, transparency (`alpha`), and density histograms.
- **Too many dimensions:** We can reduce dimensionality using techniques like Principal Component Analysis (PCA), t-SNE, or UMAP to project high-dimensional data into 2D for visualization.

---

## 2. A Brief History of Great Visualizations

Great visualizations are not new. They have been used for over a century to reveal truth, drive policy, and change minds.

### W.E.B. Du Bois (1900)
At the Paris Exposition, Du Bois presented a series of elegant hand-drawn charts and graphs to visualize the social and economic conditions of Black Americans. He understood that "unmoving prose" would not reach the masses; visualizations could make injustice tangible and undeniable.

### Charles Joseph Minard (1869)
Minard's famous map of Napoleon's 1812 Russian campaign combines **six dimensions** of data on a single 2D surface: army size (width of the line), geographic location (path on map), time (from left to right), direction (color), temperature, and location of major river crossings. It is widely considered a masterpiece of clarity and information density.

### Florence Nightingale (1858)
Nightingale's "coxcomb" diagrams (polar area charts) visualized that the majority of British soldiers in the Crimean War died not from battle wounds, but from preventable diseases. Her compelling graphics were instrumental in advocating for sanitary reforms.

### Other Landmark Visualizations
- **Hertzsprung-Russell Diagram:** Revealed the life cycle of stars by plotting luminosity against temperature.
- **Feynman Diagrams:** Provided an intuitive visual language for particle interactions, transforming theoretical physics.

---

## 3. What Makes a Bad Visualization?

Bad visualizations distort, confuse, or mislead. Common pitfalls include:

- **Ambiguity:** Unclear labels, missing scales, or confusing color maps that leave the viewer uncertain of what they are seeing.
- **Distortion:** Visual effects that exaggerate or misrepresent the true magnitude of the data.
- **Distraction:** "Chart junk"—unnecessary decorations, 3D effects, and redundant graphical elements that add no information but clutter the image.

---

## 4. Tufte’s Principles of Good Visualization

Edward Tufte, a pioneer in data visualization, laid out a set of enduring principles.

1.  **Lie Factor = 1:** The visual effect (the size of a bar, line, or shape) must be directly proportional to the numerical effect it represents. Any deviation is a lie.
2.  **Maximize the Data-Ink Ratio:** The ink (or pixels) used to display the data should be the overwhelming majority of the graphic. Minimize non-data ink used for decoration.
3.  **Avoid Chart Junk:** Eliminate unnecessary shading, 3D effects, and extraneous patterns that do not convey information.
4.  **Use Small Multiples:** Show changes or comparisons by repeating a simple graphic in a grid, allowing the viewer to make direct comparisons without complex mental gymnastics.
5.  **Provide Clear, Detailed Labeling:** Label important events directly on the graph. Write out explanations. Do not force the reader to search through a separate caption.

---

## 5. Munzner’s Rules for Modern Visualization

Tamara Munzner's work focuses on the practical design of visualizations, especially for complex, interactive systems.

- **Function First, Form Next:** Aesthetics should never compromise clarity. Design with purpose.
- **Get it Right in Black & White:** A good visualization must be interpretable without color. Color should be used to enhance, not to rescue, a poor design.
- **No Unjustified 3D:** Do not use 3D unless the third dimension is *necessary* for understanding (e.g., spatial data). 3D often introduces obstruction, clutter, and distortion. Use color, small multiples, or transparency instead.
- **Resolution over Immersion:** In interactive systems, prioritize showing data at high resolution over creating an "immersive" but low-information environment.
- **Details on Demand:** Allow users to drill down for more information, but do not overwhelm them with it in the initial view.

---

## 6. The Science of Perception: Psychophysics

Effective visualization leverages how our brains process visual stimuli.

- **Weber’s Law:** The detectable difference in a stimulus (e.g., length, brightness) is a *proportional* change, not an absolute one. We judge based on relative differences.
- **Stevens’ Power Law:** Our perception of intensity (S) is a power function of the actual stimulus (I): $S = I^n$.
    - For **length**, $n \approx 1$ (perception is linear).
    - For **brightness**, $n \approx 0.5$ (we are less sensitive to changes; brightness must increase significantly to be perceived as doubling).
    - For **saturation**, $n \approx 1.7$ (we are hyper-sensitive; small changes appear large).

**Implication:** Using area (e.g., in pie charts or bubble plots) or brightness to represent data requires careful consideration of these perceptual biases.

---

## 7. Color: A Critical Tool

Color is powerful but must be used thoughtfully.

- **Consider Color Blindness (CVD):** Affects ~8% of men and ~0.5% of women. Use tools like **Color Oracle** to simulate how your graphics appear to people with protanopia (red-blind), deuteranopia (green-blind), and tritanopia (blue-blind).
- **Use Perceptually Uniform Colormaps:** Avoid the "rainbow" colormap. It introduces artificial boundaries and is not perceptually linear. Use scientific colormaps like `viridis`, `plasma`, or `cividis` (which is designed to be colorblind-friendly).
- **Maximally Contrasting Colors:** When needing discrete color categories, the "Kelly colors" provide a set of 22 colors designed for maximum contrast, even for colorblind viewers.

---

## 8. A Spectrum of Purposes

Visualizations serve two main roles, and their design should reflect the goal.

- **Explanatory Visualization (Communication):** The goal is to tell a clear, compelling story to a specific audience. Graphics should be polished, labeled, and focused on the key message (e.g., Minard's map, Du Bois's charts).
- **Exploratory Visualization (Analysis):** The goal is for the *analyst* to understand their own data. These graphics can be more messy, interactive, and iterative. They are used to discover patterns, identify outliers, and generate hypotheses. The goal is to "see" what the numbers hide.

---

## 9. Key Takeaways

- **Honesty is paramount.** The *lie factor* must be 1.
- **Clarity is achieved** by maximizing the data-ink ratio and avoiding chart junk.
- **Consider your audience.** Are you communicating a result or exploring your data? Design accordingly.
- **Respect human perception.** Understand how we perceive length, brightness, and color.
- **Design for inclusivity.** Check your work for colorblind accessibility.

A great visualization is not just beautiful; it is honest, clear, and impactful.

---

## Your Tasks

- **Quiz:** Complete the visualization quiz linked in the course materials.
- **Homework:** Apply these principles to your own work. Critique and improve visualizations from your own projects or from published literature.

The next lecture will introduce neural networks and deep learning.
