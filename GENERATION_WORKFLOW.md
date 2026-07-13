# GENERATION_WORKFLOW.md

> Content Generation Workflow for the Master Computer Science Handbook (MCSH)

---

# Purpose

This document defines the official end-to-end workflow for generating educational content within the Master Computer Science Handbook.

Rather than generating chapters directly, every generation task should follow a structured pipeline that ensures consistency, academic quality, and alignment with the overall curriculum.

This workflow is mandatory for all Volumes.

---

# Core Principles

The generation process should always be:

- Curriculum-driven
- Knowledge-centered
- Educationally progressive
- Engineering-oriented
- Academically rigorous
- Consistent across all Volumes

---

# Overall Workflow

```text
User Request
      │
      ▼
Read PROJECT.md
      │
      ▼
Read OUTPUT.md
      │
      ▼
Identify Target Volume
      │
      ▼
Load Curriculum Mapping
      │
      ▼
Check Prerequisites
      │
      ▼
Load Shared References
      │
      ▼
Generate Chapter Outline
      │
      ▼
Generate Chapter Content
      │
      ▼
Generate Figures & Tables
      │
      ▼
Generate Exercises
      │
      ▼
Quality Validation
      │
      ▼
Final Output
```

---

# Step 1 — Understand the Request

Determine:

- Target Volume
- Target Part
- Target Chapter
- Learning objectives
- Expected difficulty

---

# Step 2 — Load Project Context

Always read:

- PROJECT.md
- OUTPUT.md

These define the educational philosophy and expected output quality.

---

# Step 3 — Load Academic Context

Consult:

- UIT_MASTER_CURRICULUM.md
- CURRICULUM_MAPPING.md
- KNOWLEDGE_GRAPH.md

Determine:

- Prerequisites
- Learning progression
- Related topics

---

# Step 4 — Load Shared References

Consult the Reference Layer:

- GLOSSARY.md
- TIMELINE.md
- SCIENTISTS.md
- PAPERS.md
- BOOKS.md
- DATASETS.md
- TOOLS.md
- VENUES.md
- RESOURCE_MAP.md

Only include references that are relevant to the chapter.

---

# Step 5 — Generate the Chapter Structure

Follow CHAPTER_TEMPLATE.md.

Ensure the chapter includes:

- Learning Objectives
- Motivation
- Intuition
- Formal Definitions
- Mathematics
- Algorithms
- Engineering Perspective
- Code Examples
- Figures
- Tables
- Summary
- Exercises
- Further Reading

---

# Step 6 — Generate Educational Content

Present concepts in the following order:

1. Motivation
2. Intuition
3. Visual Explanation
4. Formal Definition
5. Mathematical Foundation
6. Algorithm / Theory
7. Engineering Practice
8. Real-world Applications

---

# Step 7 — Generate Supporting Materials

Include where appropriate:

- Figures
- Tables
- Diagrams
- Code examples
- Worked examples
- Exercises
- Mini projects
- Common mistakes

---

# Step 8 — Self Validation

Run the Quality Checklist before producing the final output.

---

# Output Requirements

Every generated chapter should be:

- Self-contained
- Logically structured
- Consistent with previous chapters
- Suitable for graduate students
- Ready for publication after review

---

# Final Goal

Ensure that every generated chapter follows the same educational process, resulting in a coherent, high-quality handbook rather than isolated pieces of content.