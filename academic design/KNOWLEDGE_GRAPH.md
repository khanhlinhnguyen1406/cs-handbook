# KNOWLEDGE_GRAPH.md

> Knowledge Dependency Specification for the Master Computer Science Handbook (MCSH)

---

# Metadata

| Field | Value |
|-------|-------|
| Document | KNOWLEDGE_GRAPH.md |
| Version | 1.0.0 |
| Status | Frozen |
| Language | English |
| Purpose | Define the knowledge dependency graph for the entire handbook |
| Used By | MASTER_SYSTEM_PROMPT.md, Volume Generation, Course Design |

---

# Purpose

This document defines the knowledge dependency graph of the Master Computer Science Handbook.

Unlike the official curriculum, which is organized by academic courses, the Knowledge Graph organizes the handbook by conceptual dependencies.

It ensures that every concept is introduced at the appropriate time, reused consistently, and expanded progressively throughout the handbook.

The Knowledge Graph is the conceptual backbone of the entire project.

---

# Design Principles

The graph follows six principles:

1. Foundations precede applications.
2. Mathematics supports Computer Science.
3. Computer Science supports Artificial Intelligence.
4. Engineering builds upon theory.
5. Research builds upon engineering.
6. Knowledge should be introduced exactly once and reused thereafter.

---

# Graph Levels

The handbook is organized into five conceptual levels.

```text
Level 0
General Mathematics

↓

Level 1
Computer Science Foundations

↓

Level 2
Algorithms & Systems

↓

Level 3
Artificial Intelligence & Data

↓

Level 4
Research & Thesis
```

---

# Domain Graph

## Level 0 — Mathematics

Core domains:

- Logic
- Set Theory
- Proof Techniques
- Discrete Mathematics
- Calculus
- Linear Algebra
- Probability
- Statistics
- Optimization
- Information Theory

These subjects provide the mathematical language required by all later domains.

---

## Level 1 — Computer Science Foundations

Domains:

- Programming Paradigms
- Data Structures
- Computer Organization
- Computer Architecture
- Operating Systems
- Computer Networks
- Database Systems
- Software Engineering
- Theory of Computation
- Formal Languages

Prerequisite:

Level 0

---

## Level 2 — Algorithms & Systems

Domains:

- Algorithm Design
- Complexity Analysis
- Graph Algorithms
- Dynamic Programming
- Distributed Systems
- Parallel Computing
- Cloud Computing
- Data Engineering
- Information Retrieval

Prerequisite:

Level 1

---

## Level 3 — Artificial Intelligence

Domains:

- Artificial Intelligence
- Machine Learning
- Deep Learning
- Computer Vision
- Natural Language Processing
- Reinforcement Learning
- Knowledge Representation
- Recommendation Systems

Prerequisite:

Levels 0–2

---

## Level 4 — Research

Domains:

- Research Methodology
- Scientific Writing
- Experimental Design
- Reproducibility
- Thesis Preparation
- Academic Presentation

Prerequisite:

All previous levels

---

# Core Knowledge Dependency

```text
Logic
    ↓
Discrete Mathematics
    ↓
Data Structures
    ↓
Algorithms
    ↓
Advanced Algorithms
```

---

```text
Calculus
    ↓
Linear Algebra
    ↓
Optimization
    ↓
Machine Learning
    ↓
Deep Learning
```

---

```text
Probability
    ↓
Statistics
    ↓
Machine Learning
    ↓
Model Evaluation
```

---

```text
Computer Architecture
    ↓
Operating Systems
    ↓
Distributed Systems
    ↓
Cloud Computing
```

---

```text
Database Systems
    ↓
Data Engineering
    ↓
Big Data
    ↓
Data Mining
```

---

```text
Machine Learning
    ↓
Computer Vision
```

---

```text
Machine Learning
    ↓
Natural Language Processing
```

---

```text
Machine Learning
    ↓
Recommendation Systems
```

---

```text
Deep Learning
    ↓
Large Language Models
```

---

```text
Large Language Models
    ↓
AI Agents
```

---

# Reusable Knowledge Units

The following concepts are shared across multiple courses and should be introduced once, then referenced later.

## Mathematical Concepts

- Vector
- Matrix
- Eigenvalue
- Derivative
- Gradient
- Probability Distribution
- Random Variable
- Entropy
- Optimization

---

## Algorithmic Concepts

- Recursion
- Divide and Conquer
- Dynamic Programming
- Greedy Strategy
- Graph Traversal
- Search
- Complexity Analysis

---

## Systems Concepts

- Process
- Thread
- Memory
- Synchronization
- Network Protocol
- Storage
- Concurrency

---

## AI Concepts

- Feature
- Label
- Loss Function
- Gradient Descent
- Regularization
- Embedding
- Attention
- Transformer

---

## Research Concepts

- Hypothesis
- Baseline
- Experiment
- Evaluation Metric
- Statistical Significance
- Reproducibility

---

# Cross-Volume Relationships

Knowledge should be reused across volumes.

Example:

```text
Gradient Descent

Defined:

Volume 1

↓

Used:

Volume 5

↓

Extended:

Volume 6
```

---

```text
Graph Theory

Defined:

Volume 3

↓

Applied:

Volume 4

↓

Applied:

Volume 5
```

---

```text
Probability

Defined:

Volume 1

↓

Used:

Volume 5

↓

Extended:

Volume 6
```

---

# Learning Progression

Learners should advance through the handbook as follows.

```text
Mathematics

↓

Computer Science

↓

Algorithms

↓

Systems

↓

Artificial Intelligence

↓

Advanced AI

↓

Research

↓

Master's Thesis
```

No learner should skip foundational levels.

---

# Graph Rules

The Knowledge Graph must satisfy:

- Directed
- Acyclic
- Hierarchical
- Reusable
- Extensible

Circular dependencies are prohibited.

---

# Future Expansion

The graph should support future domains such as:

- Robotics
- Cybersecurity
- Bioinformatics
- Quantum Computing
- High Performance Computing
- Human–Computer Interaction

without modifying existing relationships.

---

# Relationship with Other Documents

```text
BOOK_STRUCTURE.md
        │
        ▼
UIT_MASTER_CURRICULUM.md
        │
        ▼
CURRICULUM_MAPPING.md
        │
        ▼
KNOWLEDGE_GRAPH.md
        │
        ▼
MASTER_SYSTEM_PROMPT.md
        │
        ▼
Volume Generation
```

---

# Validation Checklist

Before freezing the graph:

□ Every handbook volume is represented.

□ Every course maps to at least one domain.

□ Every prerequisite is represented.

□ No circular dependency exists.

□ Reusable concepts are identified.

□ Cross-volume references are defined.

□ Future extensions are possible.

---

# Guiding Principle

Knowledge should not exist in isolation.

Every concept must have:

- prerequisites,
- applications,
- extensions,
- and relationships with other concepts.

The Knowledge Graph exists to make those relationships explicit.

---

# Related Documents

- PROJECT.md
- BOOK_STRUCTURE.md
- COURSE_SCHEMA.md
- UIT_MASTER_CURRICULUM.md
- CURRICULUM_MAPPING.md
- MASTER_SYSTEM_PROMPT.md