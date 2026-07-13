# README.md

> Template System Specification for the Master Computer Science Handbook (MCSH)

---

# Overview

The `templates/` directory defines the canonical document structures used throughout the entire Master Computer Science Handbook (MCSH) project.

These templates ensure that every generated volume, part, chapter, figure, and table follows a consistent academic writing style, organizational structure, and pedagogical approach.

The purpose of this directory is **not** to provide content.

Instead, it defines **how content should be organized and presented**.

---

# Design Philosophy

All templates are designed according to the following principles:

- Consistency across all volumes.
- Progressive learning from fundamentals to advanced topics.
- Graduate-level academic writing.
- Strong emphasis on intuition before formalism.
- Visual explanations before mathematical derivations.
- Integration of theory, engineering, and research.
- Reusability across all Computer Science domains.

Every generated chapter should feel as though it belongs to the same professionally written handbook.

---

# Template Hierarchy

The templates follow the same hierarchy as the handbook itself.

```text
Volume
│
├── Part
│     │
│     ├── Chapter
│     │      │
│     │      ├── Figure
│     │      └── Table
│     │
│     └── ...
│
└── ...
```

Each lower-level template inherits the writing philosophy and quality requirements of its parent.

---

# Available Templates

## VOLUME_TEMPLATE.md

Defines the standard structure of an entire volume.

It specifies:

- Volume introduction
- Learning roadmap
- Knowledge graph
- Parts
- Summary
- Recommended readings
- Mini projects

Every volume in the handbook should follow this structure.

---

## PART_TEMPLATE.md

Defines the structure of each Part within a volume.

It specifies:

- Part overview
- Learning objectives
- Prerequisites
- Relationship to previous parts
- Chapters
- Part summary

All Parts should maintain a consistent learning progression.

---

## CHAPTER_TEMPLATE.md

The most important template in this directory.

It defines the complete structure of every chapter.

Typical sections include:

- Introduction
- Motivation
- Historical Background
- Intuition
- Visualization
- Mathematical Foundation
- Algorithms
- Implementation
- Engineering Applications
- Research Perspective
- Exercises
- Mini Projects
- Summary

Every chapter generated for the handbook must follow this template.

---

## FIGURE_TEMPLATE.md

Defines the standard format for diagrams and illustrations.

Figures should not merely decorate the content.

Every figure should support learning by:

- Explaining concepts visually.
- Clarifying abstract ideas.
- Simplifying mathematical reasoning.
- Illustrating workflows and architectures.

---

## TABLE_TEMPLATE.md

Defines the standard format for comparison tables, summaries, datasets, benchmarks, algorithms, and other structured information.

Tables should emphasize readability, consistency, and analytical comparison rather than listing raw data.

---

# Relationship with Other Directories

The template system works together with the rest of the project.

```text
foundations/
        │
        ▼
academic_design/
        │
        ▼
templates/
        │
        ▼
volumes/
        │
        ▼
OUTPUT.md
```

Each directory has a distinct responsibility.

- **foundations/** defines project philosophy and writing standards.
- **academic_design/** defines curriculum and knowledge organization.
- **templates/** defines document structures.
- **volumes/** define subject-specific requirements.
- **OUTPUT.md** defines the expected quality of generated content.

---

# Usage

Templates should be used as structural guides.

They should not contain subject-specific knowledge.

Instead, they define the organization that generated content must follow.

For every generated volume:

1. Read the project specifications.
2. Read the writing standards.
3. Read the academic design.
4. Read the corresponding template.
5. Generate content according to the selected volume specification.

---

# General Rules

All templates should satisfy the following requirements:

- Maintain a clear hierarchical structure.
- Encourage intuitive learning before formal theory.
- Support visual explanations whenever possible.
- Promote connections between concepts.
- Encourage both engineering practice and academic thinking.
- Be reusable across different Computer Science disciplines.

---

# Maintenance Principles

Templates should remain stable over time.

New educational content should be added to the volumes rather than modifying template structures unless a significant improvement to the overall handbook architecture is required.

The goal is to establish a long-term, maintainable document generation framework.

---

# Final Note

The `templates/` directory serves as the presentation layer of the Master Computer Science Handbook.

Its purpose is to ensure that every generated chapter, regardless of topic or volume, follows a unified educational style, making the entire handbook feel like a single coherent publication rather than a collection of independently generated documents.