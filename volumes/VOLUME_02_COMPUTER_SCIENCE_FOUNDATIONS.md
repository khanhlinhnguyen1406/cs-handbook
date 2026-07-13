# VOLUME_02_COMPUTER_SCIENCE_FOUNDATIONS.md

> Volume Specification for Volume 02 of the Master Computer Science Handbook (MCSH)

---

# Metadata

| Field | Value |
|------|------|
| Volume | 02 |
| English Title | Computer Science Foundations |
| Vietnamese Title | Nền tảng Khoa học Máy tính |
| Version | 1.0 |
| Status | Frozen |
| Estimated Length | 900–1300 pages |
| Target Level | Graduate Foundation |
| Prerequisite | Volume 01 — Mathematics for Computer Science |

---

# Purpose

This volume introduces the theoretical foundations of Computer Science that every graduate student should master before studying advanced topics such as Algorithms, Artificial Intelligence, Distributed Systems, Data Mining, Computer Vision, and academic research.

Unlike introductory programming books, this volume focuses on **how computers represent, process, organize, and reason about information**. It bridges the gap between practical software development and the scientific principles underlying computation.

Readers should finish this volume with a solid mental model of how software, hardware, data, and computation interact.

---

# Target Audience

This volume is intended for readers who:

- Have professional software development experience.
- Completed Volume 01 or possess equivalent mathematical knowledge.
- Understand programming but lack formal Computer Science foundations.
- Want to transition from Software Engineering to Computer Science.
- Plan to pursue graduate-level study or research.

---

# Position in the Handbook

This is the **second volume** of the Master Computer Science Handbook.

It transforms mathematical reasoning into computational thinking and establishes the conceptual foundations for all subsequent technical volumes.

Every later volume assumes familiarity with the concepts introduced here.

---

# Learning Objectives

After completing this volume, readers should be able to:

- Understand what computation really is.
- Explain how computers execute programs.
- Analyze the efficiency of algorithms using asymptotic notation.
- Understand data representation and memory organization.
- Explain the principles behind operating systems, databases, and networks.
- Understand formal models of computation.
- Reason about software from a Computer Science perspective.
- Build the theoretical foundation necessary for graduate research.

---

# Core Philosophy

Programming teaches **how to build software**.

Computer Science explains **why software works**.

Every concept should answer:

- What problem motivated this idea?
- Why is this concept fundamental?
- How does it relate to computation?
- How is it used in modern software systems?
- What limitations does it have?
- How does it prepare us for advanced topics?

Readers should gradually shift from writing code to reasoning about computation.

---

# Learning Journey

```text
Mathematics
        │
        ▼
Computational Thinking
        │
        ▼
Data Representation
        │
        ▼
Programming Paradigms
        │
        ▼
Data Structures
        │
        ▼
Computer Organization
        │
        ▼
Computer Architecture
        │
        ▼
Operating Systems
        │
        ▼
Database Systems
        │
        ▼
Computer Networks
        │
        ▼
Theory of Computation
        │
        ▼
Formal Languages & Automata
```

Each topic builds the conceptual framework required for later volumes.

---

# Volume Structure

## Part I — Computational Thinking

Goal:

Develop a scientific understanding of computation and problem solving.

Topics:

- What is Computer Science?
- Models of Computation
- Computational Thinking
- Abstraction
- Decomposition
- Algorithmic Thinking

---

## Part II — Data Representation

Goal:

Understand how information is represented inside computers.

Topics:

- Binary Number System
- Number Representation
- Character Encoding
- Floating Point Representation
- Data Encoding
- Memory Layout

---

## Part III — Programming Paradigms

Goal:

Understand different approaches to programming.

Topics:

- Imperative Programming
- Procedural Programming
- Object-Oriented Programming
- Functional Programming
- Declarative Programming
- Concurrent Programming

---

## Part IV — Data Structures

Goal:

Understand how data is organized efficiently.

Topics:

- Arrays
- Linked Lists
- Stacks
- Queues
- Hash Tables
- Trees
- Heaps
- Graphs

---

## Part V — Computer Organization & Architecture

Goal:

Understand how hardware executes software.

Topics:

- CPU
- Memory Hierarchy
- Cache
- Instruction Set Architecture
- Pipeline
- Parallelism
- Input / Output Systems

---

## Part VI — Operating Systems

Goal:

Understand how operating systems manage computing resources.

Topics:

- Processes
- Threads
- Scheduling
- Synchronization
- Deadlocks
- Virtual Memory
- File Systems

---

## Part VII — Database Systems

Goal:

Understand how modern systems manage persistent data.

Topics:

- Relational Model
- SQL
- Transactions
- ACID
- Indexing
- Query Optimization
- NoSQL Overview

---

## Part VIII — Computer Networks

Goal:

Understand communication between distributed systems.

Topics:

- Network Models
- TCP/IP
- Routing
- DNS
- HTTP
- REST
- WebSocket
- RPC
- Distributed Communication

---

## Part IX — Theory of Computation

Goal:

Understand the limits of computation.

Topics:

- Automata
- Regular Languages
- Context-Free Grammars
- Pushdown Automata
- Turing Machines
- Decidability
- Computability

---

# Knowledge Dependency

```text
Computational Thinking
        ↓
Data Representation
        ↓
Programming Paradigms
        ↓
Data Structures
        ↓
Computer Architecture
        ↓
Operating Systems
        ↓
Database Systems
        ↓
Computer Networks
        ↓
Theory of Computation
```

Topics must follow this dependency order.

---

# Connection to UIT Curriculum

This volume consolidates the foundational Computer Science knowledge required by the UIT Master of Computer Science curriculum, including:

- Data Structures and Algorithms (foundational concepts)
- Computer Architecture
- Operating Systems
- Database Systems
- Computer Networks
- Theory of Computation
- Advanced Programming Concepts
- Distributed Computing (foundational concepts)

Rather than treating these as isolated university courses, this volume integrates them into a coherent conceptual framework.

---

# Writing Requirements

All generated content must:

- Be written in Vietnamese.
- Preserve important English technical terms.
- Begin with intuition before formal definitions.
- Explain architectural diagrams step by step.
- Connect concepts to practical software engineering.
- Frequently reference concepts introduced in Volume 01.

---

# Teaching Methodology

Every major concept should follow this sequence:

1. Motivation
2. Real-world Problem
3. Historical Background
4. Intuition
5. Visualization
6. Formal Definition
7. Internal Mechanism
8. Code Demonstration
9. System-Level Perspective
10. Engineering Applications
11. Research Perspective
12. Common Mistakes
13. Summary
14. Exercises
15. Mini Projects
16. Further Reading

---

# Programming Integration

Examples should primarily use:

- C (memory and systems)
- Python (conceptual demonstrations)
- TypeScript (high-level comparisons when appropriate)

Whenever possible, concepts should be demonstrated using executable code.

---

# Visualization Standards

Every abstract concept should include visual aids such as:

- Memory diagrams
- CPU execution diagrams
- Stack and Heap illustrations
- Network architecture diagrams
- State transition diagrams
- Finite automata
- Tables
- Flowcharts

Visualization should help readers build accurate mental models of computer systems.

---

# Expected Outcomes

After completing this volume, readers should:

- Understand the theoretical foundations of Computer Science.
- Reason about programs beyond source code.
- Understand how software interacts with hardware.
- Analyze computational systems scientifically.
- Be fully prepared for advanced study in Algorithms, Computer Systems, Artificial Intelligence, and Research.

---

# Relationship with Other Volumes

This volume prepares readers for:

- Volume 03 — Algorithms and Data Structures
- Volume 04 — Computer Systems
- Volume 05 — Artificial Intelligence
- Volume 06 — Advanced AI
- Volume 07 — Research Methodology
- Volume 08 — Master's Thesis

Concepts introduced here should be referenced and extended throughout subsequent volumes.

---

# Generation Instruction

When generating content for this volume:

- Follow the dependency order defined above.
- Prioritize conceptual understanding over implementation details.
- Frequently connect theory with real-world software systems.
- Maintain consistency with Volume 01.
- Treat this document as the authoritative specification for Volume 02.