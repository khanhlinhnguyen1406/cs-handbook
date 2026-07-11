# GLOSSARY.md

> Terminology Standard for the Master Computer Science Handbook (MCSH)

---

# Purpose

This document defines the official terminology used throughout the Master Computer Science Handbook.

Its purpose is **not** to teach concepts, but to ensure that all generated content uses a consistent vocabulary, notation, and naming convention across every volume.

Whenever multiple translations or naming conventions exist, this document establishes the preferred terminology for the handbook.

---

# Design Philosophy

The glossary follows five principles:

1. Consistency
2. Accuracy
3. Readability
4. Academic Standard
5. International Compatibility

Readers should be able to move between different volumes without encountering conflicting terminology.

---

# Translation Principles

## General Rule

The main content of the handbook is written in Vietnamese.

Important technical terms should remain in English.

Example:

> Gradient Descent (Thuật toán Gradient Descent)

instead of

> Thuật toán hạ dốc

---

## First Appearance

When a technical term first appears:

Use:

> Vietnamese (English)

Example:

> Học tăng cường (Reinforcement Learning)

After that, English terminology may be used directly.

---

## Proper Nouns

Never translate:

- Transformer
- Linux
- Kubernetes
- TensorFlow
- PyTorch
- Docker
- CUDA
- OpenAI
- Git
- GitHub

---

## Algorithm Names

Keep original English names.

Examples:

- Merge Sort
- Quick Sort
- Binary Search
- Dijkstra Algorithm
- Bellman-Ford Algorithm

---

## Mathematical Symbols

Symbols are universal.

Never translate.

Example:

- x
- y
- λ
- σ
- μ
- ∇
- θ

---

## Variable Naming

Variable names should remain in English.

Good:

```python
learning_rate
batch_size
num_epochs
loss
accuracy
```

Avoid translating variable names into Vietnamese.

---

# Standard Terminology

## Mathematics

| English | Vietnamese |
|----------|------------|
| Vector | Vector |
| Matrix | Ma trận |
| Scalar | Vô hướng |
| Tensor | Tensor |
| Eigenvalue | Giá trị riêng |
| Eigenvector | Vector riêng |
| Gradient | Gradient |
| Jacobian | Ma trận Jacobian |
| Hessian | Ma trận Hessian |
| Probability | Xác suất |
| Random Variable | Biến ngẫu nhiên |
| Distribution | Phân phối |

---

## Programming

| English | Vietnamese |
|----------|------------|
| Variable | Biến |
| Function | Hàm |
| Method | Phương thức |
| Class | Lớp |
| Object | Đối tượng |
| Interface | Giao diện |
| Module | Mô-đun |
| Package | Gói |
| Framework | Framework |
| Library | Thư viện |

---

## Data Structures

| English | Vietnamese |
|----------|------------|
| Array | Mảng |
| Linked List | Danh sách liên kết |
| Stack | Ngăn xếp |
| Queue | Hàng đợi |
| Heap | Heap |
| Hash Table | Bảng băm |
| Graph | Đồ thị |
| Tree | Cây |

---

## Algorithms

| English | Vietnamese |
|----------|------------|
| Dynamic Programming | Quy hoạch động |
| Greedy Algorithm | Thuật toán tham lam |
| Divide and Conquer | Chia để trị |
| Backtracking | Quay lui |
| Branch and Bound | Nhánh cận |

---

## Operating Systems

| English | Vietnamese |
|----------|------------|
| Process | Tiến trình |
| Thread | Luồng |
| Scheduler | Bộ lập lịch |
| Semaphore | Semaphore |
| Deadlock | Deadlock |
| Virtual Memory | Bộ nhớ ảo |
| Paging | Phân trang |
| Cache | Bộ nhớ đệm |

---

## Computer Networks

| English | Vietnamese |
|----------|------------|
| Packet | Gói tin |
| Router | Bộ định tuyến |
| Switch | Bộ chuyển mạch |
| Gateway | Cổng kết nối |
| Throughput | Thông lượng |
| Latency | Độ trễ |
| Bandwidth | Băng thông |

---

## Databases

| English | Vietnamese |
|----------|------------|
| Database | Cơ sở dữ liệu |
| Table | Bảng |
| Row | Bản ghi |
| Column | Cột |
| Transaction | Giao dịch |
| Index | Chỉ mục |
| Query | Truy vấn |

---

## Artificial Intelligence

| English | Vietnamese |
|----------|------------|
| Artificial Intelligence | Trí tuệ nhân tạo |
| Machine Learning | Học máy |
| Deep Learning | Học sâu |
| Reinforcement Learning | Học tăng cường |
| Neural Network | Mạng nơ-ron |
| Embedding | Embedding |
| Token | Token |
| Attention | Attention |
| Transformer | Transformer |
| Large Language Model | Mô hình ngôn ngữ lớn |

---

## Software Engineering

| English | Vietnamese |
|----------|------------|
| Requirement | Yêu cầu |
| Architecture | Kiến trúc |
| Design Pattern | Mẫu thiết kế |
| Refactoring | Tái cấu trúc mã nguồn |
| Deployment | Triển khai |
| Continuous Integration | Tích hợp liên tục |
| Continuous Delivery | Phân phối liên tục |

---

# Naming Convention

Throughout the handbook:

- Keep English names for technologies.
- Translate explanatory text into Vietnamese.
- Keep API names unchanged.
- Keep library names unchanged.
- Keep programming language names unchanged.
- Keep mathematical notation unchanged.

---

# Abbreviations

When an abbreviation first appears:

Example:

Large Language Model (LLM)

Afterward:

LLM

is acceptable.

Examples:

- CNN
- RNN
- GAN
- API
- REST
- RPC
- GPU
- CPU
- SQL
- JSON
- XML
- HTTP
- HTTPS

---

# Capitalization Rules

Always capitalize:

- Programming Languages
- Frameworks
- Libraries
- Products
- Protocols
- Research Models

Examples:

- Python
- Java
- TensorFlow
- Kubernetes
- Docker
- Transformer
- GPT-4
- Llama
- DeepSeek

---

# Terms to Avoid

Avoid inconsistent translations.

For example:

❌ Thuật toán hạ dốc

Use:

✅ Gradient Descent

---

❌ Trí thông minh nhân tạo

Use:

✅ Trí tuệ nhân tạo (Artificial Intelligence)

---

❌ Mạng thần kinh

Prefer:

✅ Mạng nơ-ron (Neural Network)

---

# Updating Policy

New terminology should only be added when:

- It becomes widely accepted in academia.
- It is adopted by major conferences.
- It appears consistently in graduate textbooks.
- It becomes an industry standard.

Avoid adding temporary buzzwords.

---

# Final Goal

This glossary establishes a unified terminology standard for the entire Master Computer Science Handbook.

Its objective is to ensure that every volume, chapter, figure, table, code example, and research discussion uses consistent academic language, making the handbook easier to read, easier to maintain, and closer to the conventions used in international Computer Science literature.