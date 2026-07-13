# VOLUME_04_COMPUTER_SYSTEMS.md

> Volume Specification for Volume 04 of the Master Computer Science Handbook (MCSH)

---

# Metadata

| Field | Value |
|------|------|
| Volume | 04 |
| English Title | Computer Systems |
| Vietnamese Title | Hệ thống Máy tính |
| Version | 1.0 |
| Status | Frozen |
| Estimated Length | 1000–1400 pages |
| Target Level | Graduate Core |
| Prerequisite | Volume 01 — Mathematics for Computer Science<br>Volume 02 — Computer Science Foundations<br>Volume 03 — Algorithms and Data Structures |

---

# Purpose

This volume provides a comprehensive understanding of modern computer systems, from hardware architecture to distributed cloud platforms.

Rather than treating Operating Systems, Computer Architecture, Computer Networks, Distributed Systems, Cloud Computing, and High Performance Computing as isolated subjects, this volume presents them as interconnected layers of a complete computing ecosystem.

Readers will understand not only **how software is executed**, but also **how large-scale computing systems are designed, optimized, secured, and operated**.

The emphasis is on building a system-level mindset that prepares readers for graduate research and industrial-scale software engineering.

---

# Target Audience

This volume is intended for readers who:

- Have completed Volumes 01–03.
- Have experience developing software.
- Want to understand how computers actually execute programs.
- Plan to work with backend systems, cloud infrastructure, AI platforms, or distributed systems.
- Intend to conduct Computer Systems or AI Systems research.

---

# Position in the Handbook

This is the **fourth volume** of the Master Computer Science Handbook.

It transforms readers from application developers into engineers capable of reasoning about complete computing systems.

Concepts introduced here become essential foundations for:

- Artificial Intelligence
- Distributed AI
- Big Data
- Cloud Computing
- Cybersecurity
- High Performance Computing
- Scientific Computing

---

# Learning Objectives

After completing this volume, readers should be able to:

- Explain how programs execute from source code to hardware.
- Understand CPU, memory, storage, and I/O interactions.
- Analyze operating system behavior.
- Design scalable distributed systems.
- Understand networking protocols and cloud infrastructure.
- Explain concurrency and synchronization mechanisms.
- Reason about performance bottlenecks.
- Understand how modern AI systems are deployed at scale.

---

# Core Philosophy

Computer systems should be understood as **layers of abstraction**.

Every topic should answer:

- Why does this abstraction exist?
- What problem does it solve?
- What happens underneath?
- What are the trade-offs?
- How does this affect software performance?
- How is it applied in modern cloud and AI systems?

Readers should constantly move between high-level software and low-level implementation.

---

# Learning Journey

```text
Computer Organization
        │
        ▼
Computer Architecture
        │
        ▼
Memory Systems
        │
        ▼
Operating Systems
        │
        ▼
Concurrency
        │
        ▼
Computer Networks
        │
        ▼
Distributed Systems
        │
        ▼
Cloud Computing
        │
        ▼
High Performance Computing
        │
        ▼
AI Infrastructure
```

Each stage expands the reader's understanding from a single computer to globally distributed computing platforms.

---

# Volume Structure

## Part I — Computer Organization and Architecture

Goal:

Understand how hardware executes software.

Topics:

- Digital Logic Overview
- Instruction Set Architecture (ISA)
- CPU Organization
- Instruction Execution Cycle
- Pipeline
- Superscalar Architecture
- Branch Prediction
- Out-of-Order Execution
- SIMD
- GPU Fundamentals

---

## Part II — Memory Systems

Goal:

Understand how computers store and access information efficiently.

Topics:

- Memory Hierarchy
- Registers
- Cache Memory
- Main Memory
- Virtual Memory
- Paging
- Segmentation
- Memory Allocation
- NUMA
- Persistent Memory

---

## Part III — Operating Systems

Goal:

Understand resource management in modern operating systems.

Topics:

- Operating System Architecture
- Processes
- Threads
- Scheduling
- Synchronization
- Deadlocks
- Virtual Memory Management
- File Systems
- Device Management
- Linux Internals

---

## Part IV — Concurrency and Parallel Computing

Goal:

Develop the ability to build efficient concurrent applications.

Topics:

- Concurrency Models
- Parallel Programming
- Synchronization Primitives
- Locks
- Lock-Free Programming
- Multi-threading
- Message Passing
- Actor Model
- GPU Computing
- CUDA Fundamentals

---

## Part V — Computer Networks

Goal:

Understand communication across modern computing systems.

Topics:

- Network Architecture
- OSI Model
- TCP/IP
- Routing
- Switching
- DNS
- HTTP
- HTTPS
- HTTP/2
- HTTP/3
- QUIC
- REST
- gRPC
- WebSocket

---

## Part VI — Distributed Systems

Goal:

Understand how software operates across multiple machines.

Topics:

- Distributed System Principles
- RPC
- Distributed Transactions
- Replication
- Consistency Models
- CAP Theorem
- Consensus Algorithms
- Raft
- Paxos
- Leader Election
- Service Discovery
- Event-Driven Architecture

---

## Part VII — Cloud Computing

Goal:

Understand modern cloud-native architecture.

Topics:

- Virtualization
- Containers
- Docker
- Kubernetes
- Microservices
- Serverless Computing
- Cloud Storage
- Load Balancing
- Auto Scaling
- Observability
- Infrastructure as Code

---

## Part VIII — High Performance Computing

Goal:

Understand techniques for large-scale computation.

Topics:

- Parallel Architectures
- Distributed Computing
- MPI
- OpenMP
- GPU Clusters
- Task Scheduling
- Scientific Computing
- Performance Optimization

---

## Part IX — System Performance Engineering

Goal:

Analyze and optimize complex computing systems.

Topics:

- Profiling
- Benchmarking
- Performance Metrics
- Latency
- Throughput
- Scalability
- Bottleneck Analysis
- Monitoring
- Capacity Planning

---

## Part X — Foundations for AI Infrastructure

Goal:

Prepare readers for large-scale AI systems.

Topics:

- GPU Clusters
- Distributed Training
- Parameter Servers
- Vector Databases
- Storage Systems
- Model Serving
- AI Deployment Pipelines
- MLOps Foundations

---

# Knowledge Dependency

```text
Computer Organization
        ↓
Computer Architecture
        ↓
Memory Systems
        ↓
Operating Systems
        ↓
Concurrency
        ↓
Computer Networks
        ↓
Distributed Systems
        ↓
Cloud Computing
        ↓
Performance Engineering
        ↓
AI Infrastructure
```

All topics must follow this dependency order.

---

# Connection to UIT Curriculum

This volume integrates knowledge from multiple Computer Systems subjects commonly found in graduate Computer Science programs, including:

- Computer Architecture
- Operating Systems
- Computer Networks
- Distributed Systems
- Parallel Computing
- Cloud Computing
- High Performance Computing
- System Performance Engineering

It also establishes the infrastructure knowledge required for later AI and Big Data courses.

---

# Writing Requirements

All generated content must:

- Be written in Vietnamese.
- Preserve important English technical terms.
- Explain system architecture using intuitive diagrams.
- Progress from a single machine to distributed systems.
- Relate every concept to practical software engineering.
- Frequently reference previous volumes.

---

# Teaching Methodology

Every major concept should follow this sequence:

1. Motivation
2. Real-World Scenario
3. Historical Evolution
4. Intuition
5. Architecture Diagram
6. Internal Mechanism
7. Mathematical / Performance Analysis
8. System Implementation
9. Case Studies
10. Engineering Trade-offs
11. Research Perspective
12. Common Pitfalls
13. Summary
14. Exercises
15. Mini Projects
16. Further Reading

---

# Programming Integration

Examples should primarily use:

- C/C++ (systems programming)
- Python (simulation and experimentation)
- Bash (Linux administration)
- Docker & Kubernetes configuration
- Go (distributed systems examples where appropriate)

Hands-on experiments should accompany theoretical discussions whenever possible.

---

# Visualization Standards

Every major topic should include visual representations such as:

- CPU architecture diagrams
- Memory hierarchy illustrations
- Process lifecycle diagrams
- Thread scheduling timelines
- Network topology diagrams
- Distributed system architectures
- Kubernetes cluster layouts
- Cloud deployment diagrams
- Performance comparison charts
- Sequence diagrams

Visualization should enable readers to mentally simulate system behavior.

---

# Expected Outcomes

After completing this volume, readers should:

- Understand modern computer systems from hardware to cloud.
- Analyze performance bottlenecks systematically.
- Design scalable distributed applications.
- Understand infrastructure supporting modern AI systems.
- Be prepared for advanced research in Systems, Cloud, AI Infrastructure, and High Performance Computing.

---

# Relationship with Other Volumes

This volume provides the infrastructure foundation for:

- Volume 05 — Artificial Intelligence
- Volume 06 — Advanced AI
- Volume 07 — Research Methodology
- Volume 08 — Master's Thesis

Subsequent volumes should assume readers understand the execution environment introduced here rather than re-explaining system concepts.

---

# Generation Instruction

When generating content for this volume:

- Always explain system abstractions from the bottom up.
- Use architecture diagrams extensively.
- Connect every concept to real-world computing platforms.
- Emphasize engineering trade-offs rather than memorization.
- Frequently relate concepts to AI infrastructure, cloud-native applications, and distributed computing.
- Treat this document as the authoritative specification for Volume 04.