# README.md

> Reference Layer Specification for the Master Computer Science Handbook (MCSH)

---

# Overview

The `reference/` directory serves as the **Shared Knowledge Layer** of the Master Computer Science Handbook (MCSH).

Unlike the `volumes/` directory, which contains subject-specific learning specifications, the `reference/` directory stores reusable academic references that are shared across the entire handbook.

Its purpose is to ensure consistency, terminology, historical accuracy, and academic coherence throughout all volumes.

This directory is **not** intended to teach Computer Science concepts.

Instead, it provides authoritative reference material that supports content generation.

---

# Design Philosophy

The reference layer follows four guiding principles:

1. **Single Source of Truth (SSOT)**

Every reusable academic resource should have exactly one authoritative definition.

Examples include:

- Technical terminology
- Historical events
- Landmark papers
- Famous scientists
- Standard datasets

---

2. **Consistency**

All generated content should use the same:

- Terminology
- Naming conventions
- Timeline
- Citations
- Resource recommendations

Readers should never encounter conflicting explanations between different volumes.

---

3. **Reusability**

Reference files should be reusable across multiple domains.

For example:

- Alan Turing appears in Mathematics, Algorithms, Artificial Intelligence, and Theory of Computation.

His biography should exist only once.

---

4. **Maintainability**

Updating a shared reference should automatically improve future generated content without modifying every volume individually.

---

# Relationship with Other Directories

The project architecture follows a layered design.

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
reference/
        │
        ▼
volumes/
        │
        ▼
OUTPUT.md
```

Each layer has a distinct responsibility.

- **foundations/** defines project philosophy.
- **academic_design/** defines curriculum and knowledge organization.
- **templates/** defines document structures.
- **reference/** defines reusable academic resources.
- **volumes/** define subject-specific content.
- **OUTPUT.md** defines the expected quality of generated books.

---

# Available Reference Files

## GLOSSARY.md

Defines standardized terminology used throughout the handbook.

Includes:

- English term
- Vietnamese translation
- Definition
- Common abbreviations
- Usage guidelines

---

## TIMELINE.md

Defines important historical milestones in Computer Science.

Examples:

- Turing Machine
- Information Theory
- UNIX
- TCP/IP
- World Wide Web
- Deep Learning
- Transformer
- Large Language Models

---

## SCIENTISTS.md

Provides concise profiles of influential researchers.

Includes:

- Biography
- Major contributions
- Landmark publications
- Related fields
- Historical significance

---

## PAPERS.md

Curates foundational and influential research papers.

Organized by topic and categorized as:

- Classic Papers
- Landmark Papers
- Survey Papers
- Recent Influential Papers

---

## BOOKS.md

Lists recommended textbooks and reference books.

Each entry should include:

- Title
- Authors
- Edition (if applicable)
- Intended audience
- Related topics

---

## DATASETS.md

Summarizes commonly used datasets.

Includes:

- Domain
- Size
- Typical tasks
- Common evaluation metrics

---

## TOOLS.md

Lists important software tools, libraries, and frameworks.

Examples:

- Python
- NumPy
- PyTorch
- Docker
- Kubernetes

Each tool should include its primary purpose and common use cases.

---

## VENUES.md

Summarizes major academic publication venues.

Includes:

- Conferences
- Journals
- Research communities
- Typical research topics

---

## RESOURCE_MAP.md

Maps important topics to recommended learning resources.

For each topic, provide:

- Textbooks
- Survey papers
- Online courses
- Documentation
- Open-source projects

This file serves as a learning navigation guide.

---

# Usage Guidelines

Reference files should:

- Be concise.
- Remain academically accurate.
- Avoid unnecessary implementation details.
- Focus on reusable information.
- Be independent of any specific Volume.

Whenever a Volume references shared knowledge, it should rely on the corresponding reference file rather than redefining the information.

---

# Maintenance Principles

The reference layer should evolve slowly.

Changes should occur only when:

- New terminology becomes widely accepted.
- Major scientific milestones emerge.
- New landmark papers significantly influence the field.
- Important datasets or tools become industry standards.

Frequent structural changes should be avoided.

---

# Final Goal

The `reference/` directory acts as the shared academic foundation of the Master Computer Science Handbook.

It ensures that every generated Volume remains consistent in terminology, historical context, recommended resources, and academic references.

Together with the other project layers, it helps produce a unified handbook that reads as a single coherent publication rather than a collection of independently generated chapters.