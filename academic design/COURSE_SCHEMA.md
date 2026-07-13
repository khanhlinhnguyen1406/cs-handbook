# COURSE_SCHEMA.md

> Standard Course Specification for Master Computer Science Handbook (MCSH)

---

# Metadata

| Field | Value |
|-------|-------|
| Document | COURSE_SCHEMA.md |
| Version | 1.0.0 |
| Status | Active |
| Language | English |
| Purpose | Define the standard structure of every academic course |
| Used By | UIT_MASTER_CURRICULUM.md, CURRICULUM_MAPPING.md, MASTER_SYSTEM_PROMPT.md |

---

# Purpose

This document defines the standard specification for every course included in the Master Computer Science Handbook.

A course is the primary academic unit used to organize learning outcomes, knowledge units, prerequisites, applications, and curriculum mapping.

Every course in the handbook must follow this schema.

---

# Educational Philosophy

A course is not simply a collection of lectures.

It represents a coherent learning journey designed to help learners move from foundational understanding to engineering practice and, ultimately, academic research.

Each course should answer the following questions:

- Why does this subject exist?
- What problems does it solve?
- What knowledge should learners gain?
- How does it connect to other subjects?
- How is it applied in engineering?
- How does it support research?

---

# Standard Course Schema

```yaml
Course ID:

Course Name:

Vietnamese Name:

Category:

Course Type:

Credits:

Estimated Learning Hours:

Difficulty:

Purpose:

Course Description:

Learning Outcomes:

Prerequisites:

Recommended Background:

Core Topics:

Knowledge Units:

Supporting Mathematics:

Programming Requirements:

Engineering Applications:

Research Applications:

Industrial Applications:

Suggested Mini Projects:

Suggested Capstone Projects:

Suggested Research Topics:

Classic Papers:

Modern Papers:

Books & References:

Related Courses:

Subsequent Courses:

Volume Mapping:

Part Mapping:

Chapter Mapping:

Assessment Suggestions:

Expected Competencies:

Keywords:
```

---

# Field Definitions

## Course ID

A unique identifier.

Example:

```text
CS501

AI601

DS701
```

---

## Course Name

Official English name.

Example:

Machine Learning

---

## Vietnamese Name

Official Vietnamese course title.

Example:

Học máy

---

## Category

Examples:

- Mathematics
- Algorithms
- Systems
- Artificial Intelligence
- Data Science
- Research
- Software Engineering

---

## Course Type

Possible values:

- Foundation
- Core
- Elective
- Research
- Seminar
- Thesis

---

## Credits

Academic credit value.

---

## Estimated Learning Hours

Total expected study time, including:

- Reading
- Practice
- Assignments
- Projects
- Review

Example:

```text
120 Hours
```

---

## Difficulty

Five-level scale.

```text
★☆☆☆☆

★★☆☆☆

★★★☆☆

★★★★☆

★★★★★
```

Difficulty should consider:

- Mathematical maturity
- Programming complexity
- Conceptual depth
- Research readiness

---

## Purpose

Explain why the course exists.

Focus on the educational objective rather than listing topics.

---

## Course Description

Provide a concise overview of the course.

The description should summarize:

- scope
- objectives
- expected outcomes

---

## Learning Outcomes

Describe measurable competencies.

Examples:

- Analyze algorithms.
- Design experiments.
- Build AI models.
- Read academic papers.
- Evaluate system performance.

Learning outcomes should use action verbs.

---

## Prerequisites

Knowledge required before studying the course.

Example:

- Linear Algebra
- Calculus
- Data Structures
- Probability
- Programming Fundamentals

---

## Recommended Background

Helpful—but not mandatory—knowledge.

Example:

- Python
- Linux
- Git
- SQL

---

## Core Topics

High-level concepts covered by the course.

Example:

Machine Learning:

- Regression
- Classification
- Clustering
- Evaluation
- Feature Engineering

---

## Knowledge Units

List reusable learning components.

Example:

- Gradient Descent
- Loss Function
- Bias–Variance Tradeoff
- Cross Validation
- Regularization

Knowledge Units should be atomic and reusable across courses.

---

## Supporting Mathematics

Identify the mathematical foundations required.

Possible areas:

- Logic
- Set Theory
- Linear Algebra
- Calculus
- Probability
- Statistics
- Optimization
- Information Theory

---

## Programming Requirements

Programming languages, frameworks, or tools.

Examples:

- Python
- C++
- Java
- SQL
- PyTorch
- TensorFlow

---

## Engineering Applications

Explain how the course is used in software engineering and industry.

Examples:

- Search Engines
- Recommendation Systems
- Autonomous Vehicles
- Cloud Platforms

---

## Research Applications

Describe research areas related to the course.

Examples:

- Explainable AI
- Distributed Systems
- Computer Vision
- Human-Computer Interaction

---

## Industrial Applications

Provide examples from real-world products and services.

Examples:

- Google Search
- Netflix Recommendation
- GitHub Copilot
- ChatGPT

Applications should emphasize concepts rather than proprietary implementation details.

---

## Suggested Mini Projects

Small practical exercises.

Example:

- Spam Email Classifier
- URL Shortener
- Image Classifier
- Graph Visualizer

---

## Suggested Capstone Projects

Larger integrative projects.

Example:

- Search Engine
- Recommendation Platform
- Distributed File System
- Intelligent Chatbot

---

## Suggested Research Topics

Potential master's thesis or research directions.

Examples:

- Federated Learning
- Graph Neural Networks
- Explainable AI
- Efficient LLM Inference

---

## Classic Papers

Seminal papers that established the field.

Always preserve the original paper title.

---

## Modern Papers

Recent influential papers that reflect current research trends.

---

## Books & References

Recommended textbooks and reference materials.

Separate into:

- Beginner
- Intermediate
- Advanced
- Research

---

## Related Courses

Courses with strong conceptual relationships.

---

## Subsequent Courses

Courses that naturally build on this course.

---

## Volume Mapping

Specify where the course belongs in the handbook.

Example:

Volume 5

Artificial Intelligence

---

## Part Mapping

Specify the Part within the Volume.

---

## Chapter Mapping

List the chapters generated from this course.

---

## Assessment Suggestions

Possible evaluation methods.

Examples:

- Quiz
- Programming Assignment
- Literature Review
- Presentation
- Project
- Research Proposal

Assessment should align with learning outcomes.

---

## Expected Competencies

Summarize what learners should be capable of after completing the course.

Competencies should include:

- conceptual understanding
- implementation ability
- analytical thinking
- research readiness

---

## Keywords

A concise list of important terms.

Keywords support indexing, search, and knowledge graph construction.

---

# Design Principles

Every course should:

- Build upon previous knowledge.
- Connect to future courses.
- Balance theory and practice.
- Integrate mathematics with implementation.
- Highlight engineering relevance.
- Introduce research perspectives.
- Encourage lifelong learning.

---

# Acceptance Criteria

Before approving a course specification, verify:

□ All metadata is complete.

□ Learning outcomes are measurable.

□ Prerequisites are clearly identified.

□ Knowledge Units are reusable.

□ Mathematical foundations are specified.

□ Engineering applications are included.

□ Research applications are included.

□ Projects are appropriate for the course.

□ Volume mapping is complete.

□ Chapter mapping is complete.

---

# Related Documents

- PROJECT.md
- BOOK_STRUCTURE.md
- LANGUAGE_CONVENTION.md
- LEARNING_PHILOSOPHY.md
- WRITING_STANDARD.md
- UIT_MASTER_CURRICULUM.md
- CURRICULUM_MAPPING.md
- MASTER_SYSTEM_PROMPT.md