# WRITING_STANDARD.md

> Writing Specification for Master Computer Science Handbook (MCSH)

---

# Metadata

| Field | Value |
|-------|-------|
| Document | WRITING_STANDARD.md |
| Version | 1.0.0 |
| Status | Active |
| Language | English |
| Purpose | Define the writing standard for every generated chapter |
| Used By | MASTER_SYSTEM_PROMPT.md |

---

# Purpose

This document defines the mandatory writing standards for every chapter generated in the Master Computer Science Handbook.

It ensures that every volume, chapter, exercise, project, and explanation follows a consistent educational philosophy.

These standards are mandatory.

---

# Core Principle

The objective is NOT to generate documentation.

The objective is NOT to summarize textbooks.

The objective is to create an educational experience.

Every chapter should help the learner understand, connect, apply, and remember.

---

# Writing Philosophy

Every chapter should answer four questions:

1. Why does this topic exist?

2. What problem does it solve?

3. How does it work?

4. Why should the learner care?

If one of these questions remains unanswered, the chapter is incomplete.

---

# Reader First

Always write for the learner.

Never write to demonstrate knowledge.

Never write to impress.

Never write as if speaking to another researcher.

The learner should feel guided rather than lectured.

---

# Language Rules

Framework documents use English.

Book content uses Vietnamese.

Technical terms remain in English.

Examples:

✓ Machine Learning

✓ Gradient Descent

✓ Transformer

✓ Embedding

✓ Attention

✓ Dataset

The first occurrence of each technical term should include a natural Vietnamese explanation.

---

# Chapter Structure

Every chapter must follow the same high-level structure.

```text
Learning Objectives

↓

Motivation

↓

Real-world Problem

↓

Story

↓

Visualization

↓

Intuition

↓

Theory

↓

Mathematics

↓

Algorithm

↓

Implementation

↓

Engineering Perspective

↓

Research Perspective

↓

Summary

↓

Exercises

↓

Mini Project

↓

Further Reading
```

Do not skip sections unless explicitly instructed.

---

# Learning Objectives

Each chapter begins with learning outcomes.

Example:

After completing this chapter, the learner will be able to:

- explain the concept
- solve typical problems
- implement the algorithm
- identify limitations
- understand research directions

Learning outcomes must be measurable.

---

# Motivation

Never start with a definition.

Start with a problem.

The learner should immediately understand why this topic exists.

---

# Storytelling

Whenever appropriate:

Introduce concepts through stories.

Possible stories include:

- historical development

- engineering problems

- scientific discoveries

- industrial applications

Stories should support learning rather than entertain.

---

# Visualization

Encourage visual thinking.

Whenever possible:

Explain using:

- diagrams

- timelines

- flowcharts

- concept maps

- comparisons

If an idea cannot easily be visualized, create a mental model.

---

# Intuition

Before introducing mathematics:

Develop intuition.

The learner should already have an approximate understanding before seeing equations.

---

# Mathematics

Mathematics must follow this order:

```text
Meaning

↓

Notation

↓

Simple Example

↓

Equation

↓

Interpretation

↓

Derivation

↓

Generalization
```

Never present equations without explanation.

Every symbol must be explained.

---

# Algorithms

Algorithms should include:

- intuition

- input

- output

- assumptions

- complexity

- strengths

- weaknesses

Pseudo-code should appear before implementation.

---

# Programming

Programming exists to validate theory.

Code should be:

- readable

- simple

- educational

Avoid unnecessary abstraction.

Avoid premature optimization.

Python is preferred for AI-related topics.

---

# Engineering Perspective

Every chapter should explain:

How is this concept used in industry?

Possible examples:

- Search Engine

- Recommendation System

- Autonomous Driving

- Medical AI

- Cloud Computing

- Cybersecurity

Theory should always connect to practice.

---

# Research Perspective

Every major chapter should include:

Historical background.

Milestone papers.

Current limitations.

Open research questions.

Future directions.

The learner should understand where research continues today.

---

# Knowledge Connections

Every chapter must explicitly connect to:

Previous chapters.

Future chapters.

Related courses.

Related mathematical concepts.

Related engineering concepts.

Knowledge should form a graph rather than isolated pages.

---

# Examples

Every important idea should include at least one example.

Example types:

- numerical example

- engineering example

- real-world example

- visual example

Examples should become progressively more complex.

---

# Exercises

Every chapter should include exercises at multiple difficulty levels.

Recommended structure:

### Level 1

Concept Checking

### Level 2

Problem Solving

### Level 3

Implementation

### Level 4

Analysis

### Level 5

Research Thinking

Exercises should reinforce understanding rather than memorization.

---

# Mini Project

Every major chapter should include one practical project.

A mini project should:

- apply the chapter

- integrate previous knowledge

- encourage exploration

Projects should resemble real engineering tasks whenever possible.

---

# Visualization Standard

## Purpose

Learning becomes significantly easier when abstract concepts are transformed into visual representations.

Every important concept should include at least one visualization.

---

## Rules

Whenever appropriate, include one or more of the following:

- Concept Diagram
- Flowchart
- Timeline
- Architecture Diagram
- Data Flow Diagram
- State Machine
- Tree
- Graph
- Comparison Table
- Knowledge Graph
- Mind Map

If an actual image cannot be generated, provide a detailed **Illustration Suggestion** that can later be used by AI image generation tools.

---

## Illustration Suggestion Template

Illustration Type:

Purpose:

Description:

Key Elements:

Expected Learning Outcome:

# Formula Intuition Standard

## Purpose

Formulas should explain ideas rather than intimidate learners.

Mathematics is introduced only after learners understand the intuition.

---

## Required Learning Flow

Problem

↓

Motivation

↓

Visualization

↓

Intuition

↓

Variable Explanation

↓

Formula

↓

Formula Interpretation

↓

Formula Derivation (if necessary)

↓

Worked Example

↓

Engineering Meaning

---

## Rules

Before introducing any equation, always explain:

- Why the formula exists.
- What problem it solves.
- The meaning of every variable.
- The intuition behind the equation.
- Engineering interpretation.
- Limitations.

Never present formulas without explanation.

# Mental Model Standard

## Purpose

Every abstract concept should be transformed into a concrete mental image.

Learners remember ideas more effectively when they can visualize them.

---

## Rules

Every major chapter should contain at least one mental model.

Examples:

Gradient Descent

→ Walking downhill while blindfolded.

Transformer

→ A meeting where everyone can directly communicate with everyone else.

Embedding

→ Placing similar objects close together on a map.

Database Index

→ The index pages at the end of a textbook.

The mental model should simplify understanding without sacrificing correctness.

# Socratic Learning

Instead of immediately providing answers, encourage learners to think.

Each major chapter should begin with curiosity questions.

Examples:

- Why can ChatGPT understand language?
- Why does YouTube recommend videos?
- How does Google Search rank webpages?
- Why does Gradient Descent move in the opposite direction of the gradient?

Questions should naturally motivate the learner to continue reading.

# Chapter Enhancement Checklist

Before completing any chapter, verify that it includes:

□ Learning Objectives

□ Curiosity Questions

□ Motivation

□ Real-world Problem

□ Story

□ Illustration Suggestion

□ Mental Model

□ Visualization

□ Intuition

□ Formula Explanation

□ Worked Example

□ Engineering Perspective

□ Research Perspective

□ Compare & Contrast

□ Common Misconceptions

□ Exercises

□ Mini Project

□ Further Reading

□ Preview of the Next Chapter

A chapter should not be considered complete unless all applicable items have been addressed.

# Further Reading

Each chapter ends with recommended resources.

Possible resources include:

- textbooks

- documentation

- seminal papers

- survey papers

- benchmark datasets

The learner should know where to continue learning.

---

# Tone

The writing style should be:

- patient

- encouraging

- academically rigorous

- technically accurate

- conversational when appropriate

Avoid unnecessary complexity.

Simple language is preferred over complicated wording.

---

# Things To Avoid

Do NOT:

- copy textbook paragraphs

- overload readers with notation

- introduce mathematics too early

- explain only APIs

- skip intuition

- use unexplained terminology

- write isolated facts

Every explanation should build understanding.

---

# Chapter Completion Checklist

Before considering a chapter complete, verify:

✓ Learning objectives are defined.

✓ Motivation is clear.

✓ Problem is introduced.

✓ Intuition is developed.

✓ Mathematics is explained.

✓ Algorithm is described.

✓ Code reinforces theory.

✓ Engineering applications are discussed.

✓ Research directions are introduced.

✓ Exercises are included.

✓ Mini project is provided.

✓ Further reading is available.

If any item is missing, revise the chapter.

---

# Ultimate Goal

Every chapter should leave the learner thinking:

"I understand not only what this is, but also why it exists, how it works, when to use it, when not to use it, and where I should learn next."

That is the standard of this handbook.

---

# Related Documents

- PROJECT.md
- AI_ROLE.md
- READER_PERSONA.md
- LEARNING_PHILOSOPHY.md
- CHAPTER_TEMPLATE.md
- QUALITY_CHECKLIST.md