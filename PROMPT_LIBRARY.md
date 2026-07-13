# PROMPT_LIBRARY.md

> Standard Prompt Library for the Master Computer Science Handbook (MCSH)

---

# Purpose

This document provides a collection of standardized prompts used to generate content consistently across the Master Computer Science Handbook.

Each prompt assumes that Claude has already loaded the project context, curriculum, templates, and reference layer.

---

# Generate a Volume

Generate the complete specification and educational content for Volume X following:

- PROJECT.md
- OUTPUT.md
- VOLUME_TEMPLATE.md
- CURRICULUM_MAPPING.md
- RESOURCE_MAP.md

Ensure consistency with previous Volumes.

---

# Generate a Part

Generate Part X of Volume Y.

Include:

- Introduction
- Learning objectives
- Chapters
- Transition to the next Part

---

# Generate a Chapter

Generate Chapter X following CHAPTER_TEMPLATE.md.

The chapter must include:

- Motivation
- Intuition
- Definitions
- Mathematics
- Algorithms
- Engineering Perspective
- Code Examples
- Figures
- Tables
- Exercises
- Summary
- Further Reading

---

# Generate a Figure

Create an educational figure that:

- Improves intuition
- Is self-explanatory
- Matches FIGURE_TEMPLATE.md
- Can be implemented using Mermaid or simple diagrams where possible

---

# Generate a Table

Create a comparison or summary table following TABLE_TEMPLATE.md.

The table should highlight relationships, trade-offs, or patterns rather than simply listing facts.

---

# Generate Exercises

Generate exercises at three difficulty levels:

- Basic
- Intermediate
- Advanced

Include conceptual questions, problem-solving tasks, and implementation exercises where appropriate.

---

# Generate Mini Projects

Design a project that reinforces the chapter's learning objectives.

Include:

- Objectives
- Requirements
- Suggested tools
- Expected outcomes
- Extension ideas

---

# Generate Further Reading

Recommend:

- Canonical textbooks (BOOKS.md)
- Landmark papers (PAPERS.md)
- Relevant tools and datasets
- Related chapters and Volumes

Avoid excessive recommendations; prioritize quality over quantity.

---

# Review a Chapter

Review an existing chapter using QUALITY_CHECKLIST.md.

Identify:

- Missing concepts
- Logical gaps
- Technical inaccuracies
- Weak explanations
- Opportunities for improvement

Provide actionable recommendations without rewriting the entire chapter unless requested.

---

# Update Existing Content

When revising content:

- Preserve curriculum alignment.
- Maintain terminology consistency.
- Keep chapter numbering and structure stable.
- Improve clarity without changing the intended learning objectives.

---

# Enhance Presentation Layer

Review an existing chapter without changing its academic content.

Improve only the presentation quality.

Your objectives are:

- improve visual hierarchy;
- increase readability;
- add educational figures where appropriate;
- introduce callout boxes;
- create Formula Boxes for important equations;
- replace long paragraphs with tables or diagrams when beneficial;
- add concept maps where appropriate;
- improve chapter identity through memorable visuals;
- maintain consistency with the project's Writing Standard.

Do not change:

- curriculum;
- learning objectives;
- technical correctness;
- mathematical rigor.

Only improve the educational presentation.

The final result should resemble a professionally published graduate-level textbook.

---

# Final Principle

Every prompt in this library should produce content that is:

- Consistent
- Academically rigorous
- Educationally effective
- Engineering-oriented
- Suitable for a graduate-level handbook

The Prompt Library should evolve only when recurring generation needs are identified, avoiding unnecessary prompt proliferation.