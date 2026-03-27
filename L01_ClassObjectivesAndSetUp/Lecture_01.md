# Lecture 1: What is Data Science and the Scientific Method

**Instructor:** Dr. Federica Bianco

Welcome to Data Science for Physical Scientists. This first lecture establishes the foundation for our course. We will explore what data science is, place it in the context of the scientific method, and introduce the key tools we will use to practice reproducible, rigorous research.

---

## 1. What is Data Science?

At its heart, data science is the field of study dedicated to extracting information from data. However, it's more than just algorithms. It sits at the intersection of three critical areas:

1.  **Coding/Programming:** The technical skill to manipulate data and implement analyses.
2.  **Statistics & Machine Learning:** The mathematical frameworks for finding patterns, making inferences, and building predictive models. This includes everything from traditional statistics to modern machine learning and deep learning.
3.  **Domain Knowledge:** Understanding the context of the data—in our case, the physical sciences. Without this, you cannot correctly interpret your results or ask meaningful questions.

Data science combines these three pillars to enable the interpretation and prediction of phenomena within a specific domain.

---

## 2. The Scientific Method & Epistemology

Before we write a single line of code, we must consider what makes a scientific inquiry valid. We will examine three core principles of "good" science.

### Falsifiability
Proposed by philosopher Karl Popper, this is the idea that for a theory to be considered scientific, it must be possible to conceive of an observation or experiment that could prove it wrong. A theory that can explain *any* outcome is not testable. For example, Einstein's theory of General Relativity made a specific, falsifiable prediction: light from distant stars would be bent by the sun's gravity. An observation during a solar eclipse could either support it or falsify it. This demarcation—what is and isn't science—is a fundamental concept we'll return to throughout the course.

### Reproducibility
A cornerstone of modern science. Reproducibility means that a second researcher, using the **same raw data and the same analytical methods**, should be able to duplicate the original study's results. This is crucial because it:
- Assures that a finding is grounded in evidence, not chance or error.
- Facilitates scientific progress by allowing others to build upon existing work.
- Enables effective collaboration and teamwork.

In this class, we will achieve reproducibility through our tools: sharing raw data and the complete code used to generate every figure and numeric result.

### Parsimony (Occam's Razor)
This principle, often stated as "entities should not be multiplied without necessity," guides us to prefer simpler explanations over more complex ones. When two theories explain the data equally well, the one with fewer assumptions or parameters is generally preferred. As statistician George E. P. Box famously said, "All models are wrong, but some are useful." Parsimony helps us choose the most useful, economical model without overcomplicating it.

---

## 3. Tools of the Trade

To put these principles into practice, we will use a set of industry-standard, open-source tools.

- **Python:** Our primary programming language. It is chosen for its readability, its vast open-source ecosystem, and powerful scientific libraries like `numpy`, `scipy`, `scikit-learn`, and `statsmodels`.
- **Git & GitHub:** These are essential for version control and collaboration. Git tracks changes to your code, allowing you to revert to previous versions. GitHub is a platform for hosting these repositories, enabling you to share your code, collaborate with classmates, and—most importantly—provide the complete, reproducible record of your analysis.
- **Jupyter Notebooks / Google Colab:** These are our interactive computing environments. They allow us to combine code, visualizations, and explanatory text (Markdown) in a single, shareable document, which is perfect for creating reproducible analyses and homework assignments.
- **Stack Overflow (and AI):** A key skill in data science is knowing how to find help. Stack Overflow is a vast Q&A community for programmers. AI tools (like GitHub Copilot or ChatGPT) can also be used to help guide analysis and coding. **Crucially, if you use AI, you must explicitly declare it in your homework and exams.** The responsibility for the final work remains yours.

---

## 4. Course Logistics and Expectations

To succeed in this class, we need to work together effectively. Please keep these expectations in mind:

- **Communication:** We will use **UD Canvas** for official announcements and final grades. The primary communication hub, however, will be our **class Slack workspace**. I am far more responsive on Slack. We will have dedicated channels for each homework assignment (#hw1, #hw2, etc.) for Q&A. If you find a bug in the course materials, report it on the #bugsandissues channel for extra credit.
- **Code of Conduct:** Our classroom is a space for respectful collaboration. Harassment, bullying, and marginalization will not be tolerated. We are all here to learn, and a supportive environment is essential for that. Please read and adhere to the full Code of Conduct.
- **What I Expect from You:**
    - Show up on time and be prepared.
    - Review material after class and do the required readings.
    - Participate actively—ask and answer questions, and get up to code during lab sessions.
    - Turn in homework on time. Use the single 72-hour exception wisely.
    - Work collaboratively! Homework can be done in groups of 3-5. Each group turns in one assignment, but every member must upload it to their own repository and include a `README` file explaining their contributions and reflecting on what they learned.

---

## 5. Assessment Overview

Your final grade will be based on:

- **5% Pre-class questions:** Short quizzes at the start of class to ensure you're keeping up with the material.
- **20% Class performance & participation:** Being present, engaged, and actively coding.
- **30% Homework:** The core of the course. Assignments will be skeleton notebooks you must complete, focusing on code, interpretation, and reflection.
- **15% Midterm:** An in-class, individual exam to test your understanding under time constraints.
- **25% Final Exam:** A take-home exam followed by a short, individual oral session where you will explain and reproduce a part of your analysis.

---

## Your First Tasks

1.  **Create a GitHub account** if you don't have one.
2.  **Create a repository** named `DSPS_<firstinitialLastname>` (e.g., `DSPS_fBianco`).
3.  **Fill out the class form** (linked in the slides) with your GitHub repository link. This is your first assignment and also confirms you have read the syllabus and code of conduct.

**Required Reading:** "What is the Question?" by Jeff Leek and Roger Peng.

---

This lecture sets the stage. In our next session, we will dive into our first coding environment and begin exploring data. Welcome to the course.
