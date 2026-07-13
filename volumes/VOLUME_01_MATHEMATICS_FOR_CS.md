# VOLUME_01_MATHEMATICS_FOR_CS.md

> Volume Specification for Volume 01 of the Master Computer Science Handbook (MCSH)

---

# Metadata

| Field | Value |
|------|------|
| Volume | 01 |
| English Title | Mathematics for Computer Science |
| Vietnamese Title | Toán học dành cho Khoa học Máy tính |
| Version | 1.0 |
| Status | Frozen |
| Estimated Length | 800–1200 pages |
| Target Level | Graduate Foundation |
| Prerequisite | Basic Programming |

---

# Purpose

This volume establishes the mathematical foundation required for graduate-level Computer Science.

It is specifically designed for software engineers transitioning into Computer Science, Artificial Intelligence, Data Science, and academic research.

Unlike traditional mathematics textbooks, this volume emphasizes **understanding**, **intuition**, and **engineering applications** rather than memorization.

Readers should finish this volume thinking mathematically, not merely knowing mathematical formulas.

---

# Target Audience

This volume is intended for readers who:

- Have software engineering experience.
- Can write programs confidently.
- Have forgotten much of their university mathematics.
- Feel uncomfortable when reading formulas.
- Want to study Artificial Intelligence.
- Want to pursue graduate-level Computer Science.
- Plan to read or write academic papers.

No advanced mathematical background is assumed.

---

# Position in the Handbook

This is the **first volume** of the Master Computer Science Handbook.

It provides the mathematical language that will be used throughout every subsequent volume.

Later volumes must reference concepts introduced here instead of redefining them.

---

# Learning Objectives

After completing this volume, readers should be able to:

- Think mathematically.
- Read mathematical notation confidently.
- Interpret equations in Computer Science literature.
- Understand why algorithms work.
- Follow Machine Learning derivations.
- Build intuition for optimization techniques.
- Understand the mathematics behind AI models.
- Read graduate-level research papers without fear of equations.

---

# Core Philosophy

Mathematics should never be presented as isolated formulas.

Every concept must answer:

- What is it?
- Why does it exist?
- Why was it invented?
- What problem does it solve?
- Why should software engineers care?
- How is it used in Computer Science?
- How will it appear in later volumes?

Readers should understand **why** before learning **how**.

---

# Learning Journey

```text
Programming Experience
        │
        ▼
Mathematical Thinking
        │
        ▼
Discrete Mathematics
        │
        ▼
Linear Algebra
        │
        ▼
Calculus
        │
        ▼
Probability
        │
        ▼
Statistics
        │
        ▼
Information Theory
        │
        ▼
Optimization
        │
        ▼
Machine Learning Mathematics
```

Every topic should naturally prepare readers for the next stage.

---

# Volume Structure

## Part I — Mathematical Thinking

Goal:

Build mathematical intuition and establish the language of Computer Science.

Topics:

- Why Mathematics Matters
- Mathematical Language
- Logic
- Proof Techniques
- Sets
- Functions
- Relations

---

## Part II — Discrete Mathematics

Goal:

Develop the mathematical tools required for algorithms.

Topics:

- Counting Principles
- Combinatorics
- Graph Theory
- Trees
- Recurrence Relations

---

## Part III — Linear Algebra

Goal:

Understand the language of vectors and matrices used throughout AI.

Topics:

- Vectors
- Matrices
- Matrix Operations
- Linear Transformations
- Eigenvalues
- Eigenvectors
- Singular Value Decomposition (SVD)

---

## Part IV — Calculus

Goal:

Understand change, gradients, and optimization.

Topics:

- Functions
- Limits
- Derivatives
- Partial Derivatives
- Gradient
- Optimization

---

## Part V — Probability & Statistics

Goal:

Learn how computers reason under uncertainty.

Topics:

- Probability
- Random Variables
- Probability Distributions
- Expectation
- Variance
- Bayesian Thinking
- Statistical Inference

---

## Part VI — Information Theory

Goal:

Understand the mathematical foundations of information and learning.

Topics:

- Entropy
- Cross Entropy
- KL Divergence
- Mutual Information

---

## Part VII — Optimization for Artificial Intelligence

Goal:

Connect mathematics with Machine Learning.

Topics:

- Convex Optimization
- Gradient Descent
- Stochastic Gradient Descent (SGD)
- Momentum
- Adam Optimizer

---

# Knowledge Dependency

The following dependency order must always be respected.

```text
Logic
    ↓
Sets
    ↓
Functions
    ↓
Discrete Mathematics
    ↓
Linear Algebra
    ↓
Calculus
    ↓
Probability
    ↓
Statistics
    ↓
Information Theory
    ↓
Optimization
    ↓
Machine Learning
```

No advanced topic should be introduced before its prerequisites.

---

# Connection to UIT Curriculum

This volume consolidates the mathematical foundations required by multiple courses in the UIT Master of Computer Science curriculum, including:

- Discrete Mathematics
- Linear Algebra
- Probability and Statistics
- Optimization
- Mathematical Foundations for AI
- Machine Learning (mathematical prerequisites)
- Data Mining (mathematical prerequisites)
- Computer Vision (mathematical prerequisites)
- Natural Language Processing (mathematical prerequisites)

Rather than following individual course boundaries, this volume organizes the knowledge into a coherent learning progression.

---

# Writing Requirements

All generated content must:

- Be written in Vietnamese.
- Preserve important English technical terms.
- Introduce intuition before formal definitions.
- Explain every mathematical symbol.
- Include historical context when appropriate.
- Connect theory with engineering practice.
- Progress naturally toward research-level understanding.

---

# Teaching Methodology

Every major concept should follow this sequence:

1. Motivation
2. Real-world Problem
3. Historical Background
4. Intuition
5. Visualization
6. Formal Definition
7. Mathematical Derivation
8. Worked Examples
9. Programming Implementation
10. Engineering Applications
11. Research Perspective
12. Common Mistakes
13. Summary
14. Exercises
15. Mini Projects
16. Further Reading

---

# Programming Integration

Whenever appropriate, provide implementations using:

- Python (primary)
- TypeScript (when relevant to frontend concepts)
- Pseudocode

Code examples should prioritize understanding over optimization.

---

# Visualization Standards

Every abstract concept should include appropriate visual aids such as:

- ASCII diagrams
- Tables
- Flowcharts
- Knowledge maps
- Timelines
- Geometric intuition
- Step-by-step illustrations

Visualization is considered an essential part of learning, not an optional supplement.

---

# Expected Outcomes

After completing this volume, readers should possess the mathematical maturity required to continue with:

- Volume 02 — Computer Science Foundations
- Volume 03 — Algorithms and Data Structures
- Volume 04 — Computer Systems
- Volume 05 — Artificial Intelligence
- Volume 06 — Advanced AI
- Volume 07 — Research Methodology
- Volume 08 — Master's Thesis

This volume should transform readers from software developers who use technology into computer scientists who understand why technology works.

---

# Generation Instruction

When generating content for the handbook:

- Follow the learning order defined in this specification.
- Never skip prerequisite knowledge.
- Prioritize deep understanding over brevity.
- Frequently connect mathematical ideas to software engineering and research.
- Treat this document as the authoritative specification for Volume 01.