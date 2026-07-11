# LANGUAGE_CONVENTION.md

> Language Convention for Master Computer Science Handbook (MCSH)

---

# Metadata

| Field | Value |
|-------|-------|
| Document | LANGUAGE_CONVENTION.md |
| Version | 1.0.0 |
| Status | Active |
| Language | English |
| Purpose | Define language usage across the entire repository |
| Used By | MASTER_SYSTEM_PROMPT.md |

---

# Purpose

This document defines the language conventions used throughout the Master Computer Science Handbook project.

The objective is to maximize learning efficiency while maintaining compatibility with international Computer Science terminology.

---

# General Principle

The repository is intended for Vietnamese learners studying graduate-level Computer Science.

Therefore:

- Framework documentation should use **English**.
- Educational content should use **Vietnamese**.
- Technical terminology should remain in **English** whenever appropriate.

This approach preserves academic accuracy while improving readability.

---

# Repository Language

The following components must be written in English:

- Repository name
- Folder names
- File names
- Metadata
- Specifications
- AI prompts
- Framework documents
- Architecture documents
- Diagrams
- Graph labels
- Configuration files

Examples:

```text
README.md

PROJECT.md

BOOK_STRUCTURE.md

KNOWLEDGE_GRAPH.md

MASTER_SYSTEM_PROMPT.md
```

---

# Educational Content

All educational materials should be written in Vietnamese.

This includes:

- Explanations
- Stories
- Intuition
- Mathematical explanations
- Engineering discussions
- Research discussions
- Exercises
- Mini projects
- Case studies
- Chapter summaries

The writing style should resemble a university textbook written for Vietnamese graduate students.

---

# Technical Terms

Important Computer Science terminology should remain in English.

Do NOT translate widely accepted technical terms.

Examples include:

- Machine Learning
- Deep Learning
- Artificial Intelligence
- Transformer
- Attention
- Embedding
- Gradient Descent
- Learning Rate
- Dataset
- Epoch
- Batch
- Tokenization
- Feature Engineering
- Knowledge Graph
- Information Retrieval
- Knowledge Representation
- Backpropagation
- Reinforcement Learning
- Computer Vision
- Natural Language Processing

---

# First Appearance Rule

When a technical term appears for the first time in a chapter:

1. Keep the English term.
2. Explain its meaning naturally in Vietnamese.
3. Continue using the English term consistently throughout the chapter.

Example:

Gradient Descent là một thuật toán tối ưu giúp mô hình tìm giá trị nhỏ nhất của hàm Loss bằng cách di chuyển theo hướng ngược với Gradient.

---

# Mathematical Notation

Mathematical notation should always remain international.

Examples:

- x
- y
- θ
- ∇
- Σ
- μ
- σ

Never translate mathematical symbols.

Variable names should remain consistent throughout the entire handbook.

---

# Programming Language

Programming languages are never translated.

Examples:

- Python
- C++
- Java
- TypeScript
- JavaScript
- SQL

Code comments should use Vietnamese when they are intended for educational purposes.

Variable names should remain in English.

Good Example:

```python
learning_rate = 0.01

# Cập nhật trọng số sau mỗi vòng lặp
weights -= learning_rate * gradient
```

---

# Figures

Figure titles should use Vietnamese.

Technical labels inside diagrams may remain in English.

Example:

Title:

Quy trình huấn luyện mô hình Machine Learning

Diagram Labels:

Dataset

Training

Validation

Testing

Deployment

---

# Tables

Column names should follow the surrounding language.

Educational books:

Vietnamese

Framework documents:

English

---

# Chapter Titles

Chapter titles should be written in Vietnamese while preserving important English terminology.

Examples:

Gradient Descent

Hàm Loss (Loss Function)

Mạng Neural (Neural Network)

Vector Embedding

Transformer Architecture

---

# Research Papers

Paper titles should never be translated.

Examples:

Attention Is All You Need

ImageNet Classification with Deep Convolutional Neural Networks

The original title should always be preserved.

---

# Citations

Academic citations should follow international standards.

Examples:

APA

IEEE

ACM

Do not translate paper titles inside citations.

---

# Terminology Consistency

One concept should use one consistent term throughout the handbook.

Avoid switching between multiple translations.

Example:

Always use:

Gradient Descent

Do NOT alternate between:

Gradient Descent

Thuật toán hạ Gradient

Giảm Gradient

Thuật toán Gradient

Consistency is more important than literal translation.

---

# AI Writing Rules

When generating educational content:

- Explain in Vietnamese.
- Preserve technical terminology in English.
- Avoid machine-like literal translations.
- Prioritize natural educational writing.

The learner should feel as if reading a Vietnamese university textbook rather than a translated document.

---

# Acceptance Criteria

Before publishing any chapter, verify:

□ Repository structure follows English naming.

□ Book content is written in Vietnamese.

□ Technical terms remain in English.

□ Mathematical notation is unchanged.

□ Programming languages remain unchanged.

□ Variable names remain in English.

□ Academic citations remain international.

□ Terminology is consistent throughout the chapter.

---

# Guiding Principle

Use Vietnamese to teach.

Use English to preserve precision.

The objective is not translation.

The objective is understanding.

---

# Related Documents

- PROJECT.md
- AI_ROLE.md
- READER_PERSONA.md
- WRITING_STANDARD.md
- LEARNING_PHILOSOPHY.md