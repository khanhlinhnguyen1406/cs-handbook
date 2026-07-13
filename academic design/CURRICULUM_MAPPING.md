# CURRICULUM_MAPPING.md

> Curriculum-to-Handbook Mapping Specification for the Master Computer Science Handbook (MCSH)

---

# Metadata

| Field | Value |
|-------|-------|
| Document | CURRICULUM_MAPPING.md |
| Version | 1.0.0 |
| Status | Frozen |
| Language | English |
| Purpose | Define how the official UIT curriculum is transformed into handbook volumes, chapters, and knowledge units |
| Used By | MASTER_SYSTEM_PROMPT.md, Volume Generation |

---

# Purpose

This document defines the mapping rules between the official Master of Computer Science curriculum and the handbook architecture.

It ensures that every academic course is translated into:

- Handbook Volumes
- Parts
- Chapters
- Knowledge Units
- Projects
- Research Topics

The objective is to preserve the academic integrity of the curriculum while organizing knowledge into a learner-friendly structure.

---

# Mapping Philosophy

University curricula are organized by academic courses.

Handbooks are organized by knowledge.

Therefore, this document transforms:

```text
Academic Course
        │
        ▼
Knowledge Domains
        │
        ▼
Knowledge Units
        │
        ▼
Book Chapters
        │
        ▼
Volumes
```

A single course may contribute to multiple handbook volumes.

Likewise, a single volume may integrate knowledge from several courses.

---

# Mapping Principles

The mapping process follows these principles:

1. Preserve all essential academic knowledge.
2. Eliminate unnecessary duplication.
3. Merge highly related concepts.
4. Keep prerequisite relationships intact.
5. Introduce concepts in a logical learning order.
6. Connect theory with engineering practice.
7. Connect engineering with research.

---

# Handbook Structure

```text
Volume
    ↓
Part
    ↓
Chapter
    ↓
Section
    ↓
Knowledge Unit
```

Each course is decomposed into reusable Knowledge Units before being assigned to chapters.

---

# Standard Mapping Record

Each course mapping follows the structure below.

```yaml
Course:

Course File:

Knowledge Units:

Prerequisites:

Volume Mapping:

Part Mapping:

Chapter Mapping:

Engineering Topics:

Research Topics:

Projects:

Exercises:

Assessment:
```

---

# Mapping Levels

The transformation is performed through five levels.

## Level 1 — Course Mapping

Maps official UIT courses.

Example:

```text
Machine Learning

↓

courses/MACHINE_LEARNING.md
```

---

## Level 2 — Knowledge Unit Mapping

Extract reusable concepts from each course.

Example:

```text
Regression

Classification

Loss Function

Gradient Descent

Cross Validation

Regularization
```

Knowledge Units should be reusable across multiple courses.

---

## Level 3 — Chapter Mapping

Group related Knowledge Units into coherent chapters.

Example:

```text
Chapter

Regression Models

contains

Linear Regression

Polynomial Regression

Evaluation Metrics
```

---

## Level 4 — Part Mapping

Group chapters into larger thematic parts.

Example:

```text
Machine Learning Foundations

↓

Regression

↓

Classification

↓

Evaluation
```

---

## Level 5 — Volume Mapping

Assign each Part to a handbook Volume.

---

# Mapping Table

> This table should be completed after the official UIT curriculum has been imported.

| UIT Course | Course File | Volume | Part | Chapters | Status |
|------------|-------------|--------|------|----------|--------|
| TBD | TBD | TBD | TBD | TBD | Pending |

---

# Example Mapping

## Course

Machine Learning

↓

Course File

```text
courses/MACHINE_LEARNING.md
```

↓

Knowledge Units

- Regression
- Classification
- Clustering
- Model Evaluation
- Ensemble Learning

↓

Volume

Volume 5

↓

Part

Machine Learning Foundations

↓

Chapters

- Introduction to Machine Learning
- Regression
- Classification
- Evaluation
- Ensemble Methods

↓

Projects

- Spam Email Classifier
- House Price Prediction
- Recommendation System

↓

Research

- Federated Learning
- Explainable AI
- AutoML

---

# Dependency Mapping

Learning dependencies must always be preserved.

Example:

```text
Linear Algebra
        ↓
Optimization
        ↓
Gradient Descent
        ↓
Machine Learning
        ↓
Deep Learning
```

No chapter should require knowledge that has not yet been introduced.

---

# Mathematics Mapping

Each course must declare its mathematical foundations.

Example:

| Course | Mathematics |
|---------|-------------|
| Machine Learning | Linear Algebra, Probability, Statistics, Optimization |
| Computer Vision | Linear Algebra, Calculus |
| NLP | Probability, Statistics, Linear Algebra |

---

# Engineering Mapping

Each course should map to engineering applications.

Examples:

| Course | Engineering Applications |
|---------|--------------------------|
| Algorithms | Search Engine, Routing |
| Distributed Systems | Cloud Infrastructure |
| Machine Learning | Recommendation Systems |
| Databases | Data Platform |

---

# Research Mapping

Each course should identify research directions.

Example:

| Course | Research Areas |
|---------|----------------|
| Machine Learning | AutoML, Federated Learning |
| Computer Vision | Object Detection, Medical Imaging |
| Distributed Systems | Edge Computing |

---

# Volume Mapping Overview

```text
Volume 1
Mathematics for Computer Science

↑

Courses

Advanced Mathematics

Probability

Statistics

==============================

Volume 2

Computer Science Foundations

↑

Courses

Algorithms

Operating Systems

Computer Architecture

Networks

==============================

Volume 3

Algorithms and Data Structures

==============================

Volume 4

Data Engineering & Systems

==============================

Volume 5

Artificial Intelligence

==============================

Volume 6

Advanced AI

==============================

Volume 7

Research Methodology

==============================

Volume 8

Capstone & Thesis
```

---

# Knowledge Flow

The handbook follows a cumulative learning path.

```text
Mathematics
        ↓
Computer Science Foundations
        ↓
Algorithms
        ↓
Computer Systems
        ↓
Artificial Intelligence
        ↓
Advanced AI
        ↓
Research
        ↓
Master's Thesis
```

Every mapping should reinforce this progression.

---

# Validation Checklist

Before approving the mapping:

□ Every UIT course has been mapped.

□ Every course has a corresponding course file.

□ Every Knowledge Unit appears only once as its primary definition.

□ Every chapter has identified prerequisites.

□ Every volume contains coherent content.

□ Engineering applications are included.

□ Research directions are included.

□ No prerequisite conflicts exist.

□ The complete handbook covers all official curriculum outcomes.

---

# Related Documents

- PROJECT.md
- BOOK_STRUCTURE.md
- COURSE_SCHEMA.md
- UIT_MASTER_CURRICULUM.md
- KNOWLEDGE_GRAPH.md
- MASTER_SYSTEM_PROMPT.md