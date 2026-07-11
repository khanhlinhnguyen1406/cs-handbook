# DATASETS.md

> Canonical Dataset Reference for the Master Computer Science Handbook (MCSH)

---

# Purpose

This document defines the canonical datasets referenced throughout the Master Computer Science Handbook.

Rather than collecting every publicly available dataset, this document focuses on benchmark datasets that have played an important role in research, education, and industrial practice.

Each dataset represents a milestone or standard benchmark within its domain.

---

# Design Philosophy

Datasets are selected based on:

- Academic influence
- Educational value
- Benchmark popularity
- Long-term relevance
- Wide adoption

Preference is given to datasets that appear frequently in graduate courses, research papers, and international competitions.

---

# Dataset Entry Structure

Every dataset should include:

---

## Dataset Name

Official English name.

---

## Domain

Examples:

- Computer Vision
- NLP
- Machine Learning
- Reinforcement Learning
- Speech
- Recommendation Systems

---

## Primary Tasks

Examples:

- Classification
- Detection
- Segmentation
- Translation
- Question Answering
- Recommendation

---

## Data Scale

Approximate dataset size.

Example:

- Number of samples
- Number of classes
- Storage size

---

## Evaluation Metrics

Examples:

- Accuracy
- Precision
- Recall
- F1-score
- BLEU
- ROUGE
- mAP
- IoU
- Perplexity

---

## Related Volumes

Volumes where the dataset naturally appears.

---

# Classical Machine Learning

## Iris

Domain

Machine Learning

Tasks

Classification

Importance

Classic introductory dataset for supervised learning.

Related Volume

Volume 5

---

## Boston Housing

Domain

Regression

Importance

Historical regression benchmark.

Note

Mention its historical significance while noting that modern teaching often prefers alternative datasets due to ethical and methodological concerns.

Related Volume

Volume 5

---

## Titanic

Domain

Classification

Importance

Educational dataset for feature engineering and classification.

Related Volume

Volume 5

---

# Computer Vision

## MNIST

Tasks

Handwritten Digit Recognition

Importance

The classic introductory benchmark for image classification.

Related Volume

Volume 5

---

## CIFAR-10

Tasks

Image Classification

Importance

Standard benchmark for CNN architectures.

Related Volume

Volume 5

---

## CIFAR-100

Importance

More challenging image classification benchmark.

---

## ImageNet

Tasks

Large-scale Image Classification

Importance

Catalyst for the Deep Learning revolution.

Related Volume

Volume 5

---

## COCO

Tasks

- Object Detection
- Segmentation
- Captioning

Importance

Industry-standard benchmark.

Related Volume

Volume 5

---

# Natural Language Processing

## Penn Treebank

Importance

Language Modeling

---

## SQuAD

Tasks

Question Answering

Importance

Reading comprehension benchmark.

---

## GLUE

Tasks

Natural Language Understanding

---

## SuperGLUE

Importance

Advanced language understanding benchmark.

---

## WMT

Tasks

Machine Translation

---

## Common Crawl

Importance

Large-scale web corpus used for foundation model training.

Related Volume

Volume 6

---

# Recommendation Systems

## MovieLens

Importance

Collaborative Filtering

Recommendation Systems

---

# Reinforcement Learning

## OpenAI Gym

Importance

Standard reinforcement learning environments.

---

## Atari 2600

Importance

Deep Reinforcement Learning benchmark.

---

# Graph Learning

## Cora

Importance

Node Classification

---

## Citeseer

Importance

Graph Neural Networks

---

## OGB (Open Graph Benchmark)

Importance

Modern benchmark suite.

---

# Speech

## LibriSpeech

Importance

Automatic Speech Recognition.

---

# Large Language Models

## The Pile

Importance

Large-scale language model pretraining.

---

## C4

Importance

Massive web corpus.

---

## FineWeb

Importance

Modern large-scale web dataset.

---

# Dataset Categories

| Domain | Representative Dataset |
|----------|------------------------|
| Machine Learning | Iris |
| Regression | Boston Housing |
| Vision | ImageNet |
| Detection | COCO |
| NLP | SQuAD |
| Translation | WMT |
| Recommendation | MovieLens |
| RL | Atari |
| Graph | OGB |
| Speech | LibriSpeech |
| Foundation Models | C4 |

---

# Usage Guidelines

Datasets should be introduced when:

- Explaining benchmark results.
- Demonstrating algorithms.
- Presenting experiments.
- Discussing evaluation metrics.
- Building mini projects.

Avoid introducing datasets without explaining:

- Why they became benchmarks.
- Their strengths.
- Their limitations.

---

# Relationship with Other References

This document complements:

- PAPERS.md
- TOOLS.md
- RESOURCE_MAP.md

Datasets provide experimental foundations for research papers and engineering implementations.

---

# Maintenance Policy

New datasets should only be added if they become:

- Widely cited benchmarks.
- Common educational resources.
- Industry-standard evaluation datasets.

Short-lived competition datasets should generally be excluded.

---

# Final Goal

This document provides a curated collection of benchmark datasets that support learning, experimentation, and research throughout the Master Computer Science Handbook.

Readers should understand not only which datasets are commonly used, but also why they became important milestones in the evolution of Computer Science and Artificial Intelligence.