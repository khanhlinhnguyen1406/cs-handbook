# FIGURE_TEMPLATE.md

> Standard Figure Template for the Master Computer Science Handbook (MCSH)

---

# Purpose

This document defines the standard for all figures, diagrams, illustrations, flowcharts, timelines, architecture diagrams, and conceptual visualizations used throughout the Master Computer Science Handbook.

Figures are not decorative elements.

Every figure should have a clear educational purpose and help readers build intuition before learning formal definitions, mathematics, or implementation details.

---

# Design Philosophy

Every figure should:

- Explain a concept visually.
- Reduce cognitive load.
- Complement the surrounding text.
- Improve conceptual understanding.
- Be self-explanatory whenever possible.
- Maintain a consistent visual style across the entire handbook.

Figures should answer:

> "Can a reader understand this concept faster because of this figure?"

If the answer is no, the figure should not be included.

---

# Figure Types

The handbook may include different categories of figures.

## Concept Diagram

Illustrates abstract concepts.

Examples:

- Vector Space
- Stack vs Heap
- Graph Structure
- Neural Network
- Memory Layout

---

## Workflow Diagram

Illustrates execution flow.

Examples:

- Compiler Pipeline
- Operating System Scheduling
- Machine Learning Pipeline
- Data Processing Pipeline

---

## Architecture Diagram

Illustrates system structure.

Examples:

- CPU Architecture
- Transformer Architecture
- Client-Server Architecture
- Distributed System
- Kubernetes Cluster

---

## Timeline

Illustrates historical evolution.

Examples:

- Artificial Intelligence
- Operating Systems
- Database Development
- Programming Languages

---

## Comparison Diagram

Highlights similarities and differences.

Examples:

- DFS vs BFS
- TCP vs UDP
- CNN vs Transformer
- RNN vs LSTM

---

## Mathematical Visualization

Illustrates mathematical intuition.

Examples:

- Gradient Descent
- Linear Transformation
- Eigenvectors
- Probability Distribution
- Decision Boundary

---

## State Diagram

Illustrates state transitions.

Examples:

- TCP State Machine
- Process States
- DFA
- Protocol State

---

## Knowledge Graph

Illustrates relationships among concepts.

Examples:

- Data Structures
- Computer Networks
- Artificial Intelligence
- Compiler Components

---

# Standard Figure Structure

Every figure should include:

## Figure Number

Example:

Figure 3.2

---

## Figure Title

A concise and descriptive title.

Example:

Transformer Encoder Architecture

---

## Purpose

Describe:

- Why this figure exists.
- Which concept it explains.
- Which learning difficulty it addresses.

---

## Description

Explain:

- What the reader is looking at.
- The overall meaning of the figure.
- Important components.

---

## Key Components

List the major elements shown.

Example:

- Input Layer
- Hidden Layer
- Attention Block
- Output Layer

---

## Reading Guide

Explain:

How should the reader read the figure?

Example:

- Left to right
- Top to bottom
- Outer layer to inner layer

---

## Related Concepts

Reference:

- Chapters
- Sections
- Formulas
- Algorithms

---

## Key Takeaways

Summarize:

The most important insights readers should obtain after studying the figure.

---

# Visualization Principles

Figures should emphasize:

- Simplicity
- Readability
- Consistency
- Accuracy
- Educational value

Avoid unnecessary decorative elements.

---

# Recommended Visualization Order

Whenever introducing a difficult concept:

```text
Problem
        │
        ▼
Visual Illustration
        │
        ▼
Intuition
        │
        ▼
Definition
        │
        ▼
Mathematics
        │
        ▼
Algorithm
        │
        ▼
Implementation
```

Visualization should come before formalization.

---

# Diagram Style

Whenever possible:

- Use Mermaid diagrams for structural illustrations.
- Use ASCII diagrams for simple layouts.
- Use tables for comparisons.
- Use mathematical plots only when necessary.
- Use flowcharts to explain processes.
- Use architecture diagrams for systems.

The style should remain consistent throughout the handbook.

---

# Figure Caption Style

Each figure caption should include:

1. Figure number.
2. Figure title.
3. One-sentence summary.
4. Educational purpose.

Example:

> **Figure 5.3. Transformer Encoder Architecture**  
> Minh họa luồng xử lý của một Transformer Encoder, giúp người học hiểu mối quan hệ giữa Multi-Head Attention, Add & Norm và Feed Forward Network trước khi đi vào công thức toán học.

---

# Quality Checklist

Before including a figure, verify:

- The figure supports a learning objective.
- The figure is referenced in the text.
- The figure is easy to understand.
- Labels are accurate.
- Terminology is consistent.
- The figure is not redundant.
- The figure improves intuition.

---

# Final Goal

Every figure should help readers understand complex Computer Science concepts more quickly and intuitively.

A reader should be able to grasp the main idea of a topic by studying its figures before reading the detailed explanations.