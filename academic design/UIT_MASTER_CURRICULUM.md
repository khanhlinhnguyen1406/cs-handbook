# UIT_MASTER_CURRICULUM.md

> Official Academic Specification for the Master of Computer Science Handbook (MCSH)

---

# Metadata

| Field | Value |
|-------|-------|
| Document | UIT_MASTER_CURRICULUM.md |
| Version | 3.1.0 |
| Status | Official curriculum imported — pending course file validation |
| Language | English |
| Purpose | Academic Specification |
| Scope | Master of Computer Science |
| University | University of Information Technology (UIT) |
| Used By | CURRICULUM_MAPPING.md, MASTER_SYSTEM_PROMPT.md |

---

# 1. Purpose

This document defines the academic specification for the Master Computer Science Handbook.

It is the **Single Source of Truth (SSOT)** for all curriculum-related information.

Every handbook volume, chapter, project, exercise, and research topic must ultimately trace back to this specification.

This document is intentionally independent of any AI model.

It serves as the permanent academic backbone of the project.

---

# 2. Educational Vision

The handbook is designed for software engineers transitioning into graduate-level Computer Science.

The educational journey is:

Software Engineering

↓

Computer Science Foundation

↓

Advanced Computer Science

↓

Artificial Intelligence

↓

Scientific Research

↓

Master's Thesis

↓

Independent Research

The objective is not merely to pass university courses.

The objective is to cultivate engineers capable of understanding, applying, and creating Computer Science knowledge.

---

# 3. Curriculum Design Principles

The curriculum follows these principles.

## Foundation First

Strong mathematical and Computer Science foundations precede advanced topics.

---

## Knowledge Progression

Every concept builds upon previous knowledge.

No isolated topics.

---

## Theory Before Practice

Understand principles before implementation.

---

## Engineering Before Research

Research capability is built upon engineering competence.

---

## Concept Reuse

Knowledge should never be duplicated.

Later courses extend earlier concepts.

---

## Lifelong Learning

The curriculum prepares learners for continuous learning beyond graduation.

---

# 4. Curriculum Structure

The curriculum consists of four academic groups.

| Group | Description |
|---------|------------|
| Foundation | Mathematics and Computer Science fundamentals |
| Core | Mandatory graduate Computer Science courses |
| Elective | Specialized topics |
| Research | Research methodology and thesis |

---

# 5. Course Record Standard

Every course must follow the COURSE_SCHEMA.md specification.

Each course contains the following information.

```yaml
Metadata

Academic Information

Learning Objectives

Learning Outcomes

Prerequisites

Knowledge Units

Supporting Mathematics

Programming Requirements

Engineering Applications

Research Applications

Projects

Research Papers

Books

Assessment

Volume Mapping

Chapter Mapping

Related Courses
```

---

# 6. Official Curriculum

> This section contains the official Master of Computer Science curriculum (Khung chương trình đào tạo Thạc sĩ ngành Khoa học Máy tính) as published by UIT.
>
> The program offers **three tracks**: Research Track – Method 1, Research Track – Method 2, and Application Track. Column legend: `TC` = credits (tín chỉ), `LT` = theory hours (lý thuyết), `TH` = practice hours (thực hành), `HK` = semester (học kỳ).

## 6.1 Research Track – Method 1 (Chương trình nghiên cứu – Phương thức 1)

### General Knowledge Block (Kiến thức chung, ≥ 3 TC)

- Philosophy (Triết học) is a compulsory course in the General Knowledge block.
- Learners may optionally take the Mathematics course.

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 1 | PH2001 | Triết học (Philosophy) | 3 | 3 | 0 | 1 |
| 2 | MA2001 | Toán học (Mathematics) | 4 | 4 | 0 | 1 |

### Foundation Knowledge Block (Kiến thức cơ sở, 4 TC)

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 3 | CS2205 | Phương pháp nghiên cứu khoa học (Scientific Research Methods) | 2 | 2 | 0 | 1 |
| 4 | CS3205 | Phương pháp nghiên cứu khoa học nâng cao (Advanced Scientific Research Methods) | 2 | 2 | 0 | 1 |

### Graduation Thesis (Luận văn tốt nghiệp, 53 TC)

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 5 | CS2505 | Luận văn tốt nghiệp hướng nghiên cứu – Phương thức 1 (Research-oriented Graduation Thesis – Method 1) | 53 | 53 | 0 | 2 |

**Total credits – Method 1:** ≥ 60 TC (3 + 4 + 53)

---

## 6.2 Research Track – Method 2 (Chương trình nghiên cứu – Phương thức 2)

### General Knowledge Block (Kiến thức chung, ≥ 3 TC)

- Philosophy (Triết học) is a compulsory course in the General Knowledge block.
- Learners may optionally take the Mathematics course.

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 1 | PH2001 | Triết học (Philosophy) | 3 | 3 | 0 | 1 |
| 2 | MA2001 | Toán học (Mathematics) | 4 | 4 | 0 | 1 |

### Foundation Knowledge Block (Kiến thức cơ sở, 2 TC)

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 3 | CS2205 | Phương pháp nghiên cứu khoa học (Scientific Research Methods) | 2 | 2 | 0 | 1 |

### Specialized Knowledge Block (Kiến thức chuyên ngành, ≥ 28 TC)

- Learners select and accumulate a minimum of 28 credits from the elective list below.

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 4 | NT2102 | An toàn và bảo mật hệ thống thông tin (Information System Security) | 4 | 4 | 0 | 2,3 |
| 5 | IT2011 | Cơ sở dữ liệu nâng cao (Advanced Databases) | 4 | 4 | 0 | 2,3 |
| 6 | IT2021 | Xử lý tín hiệu số nâng cao (Advanced Digital Signal Processing) | 3 | 3 | 0 | 2,3 |
| 7 | IT2030 | Hệ thống thông tin địa lý nâng cao (Advanced Geographic Information Systems) | 3 | 3 | 0 | 2,3 |
| 8 | CS2201 | Biểu diễn tri thức và suy luận (Knowledge Representation and Reasoning) | 4 | 4 | 0 | 2,3 |
| 9 | CS2202 | Ngôn ngữ học máy tính (Computational Linguistics) | 4 | 4 | 0 | 2,3 |
| 10 | CS2203 | Xử lý ảnh và thị giác máy tính (Image Processing and Computer Vision) | 4 | 4 | 0 | 2,3 |
| 11 | CS2207 | Khai thác dữ liệu và ứng dụng (Data Mining and Applications) | 4 | 4 | 0 | 2,3 |
| 12 | CS2208 | Hệ hỗ trợ quyết định (Decision Support Systems) | 3 | 3 | 0 | 2,3 |
| 13 | CS2209 | Dịch máy (Machine Translation) | 3 | 3 | 0 | 2,3 |
| 14 | CS2213 | Xử lý tiếng nói và giao tiếp người máy (Speech Processing and Human-Machine Interaction) | 4 | 4 | 0 | 2,3 |
| 15 | CS2215 | Điện toán lưới và đám mây (Grid and Cloud Computing) | 3 | 3 | 0 | 2,3 |
| 16 | CS2218 | Lý thuyết mã hóa thông tin (Information Coding Theory) | 3 | 3 | 0 | 2,3 |
| 17 | CS2223 | Nguyên lý và phương pháp lập trình (Programming Principles and Methods) | 3 | 3 | 0 | 2,3 |
| 18 | CS2224 | Tìm kiếm thông tin thị giác (Visual Information Retrieval) | 3 | 3 | 0 | 2,3 |
| 19 | CS2225 | Nhận dạng thị giác và ứng dụng (Visual Recognition and Applications) | 3 | 3 | 0 | 2,3 |
| 20 | CS2226 | Ontology và ứng dụng (Ontology and Applications) | 3 | 3 | 0 | 2,3 |
| 21 | CS2227 | Máy học trong xử lý dữ liệu Y khoa (Machine Learning in Medical Data Processing) | 4 | 4 | 0 | 2,3 |
| 22 | CS2228 | Các thuật toán tiến hóa (Evolutionary Algorithms) | 4 | 4 | 0 | 2,3 |
| 23 | CS2229 | Thuật toán và lý thuyết máy học (Machine Learning Algorithms and Theory) | 4 | 4 | 0 | 2,3 |
| 24 | CS2230 | Các mô hình học sâu và ứng dụng (Deep Learning Models and Applications) | 4 | 4 | 0 | 2,3 |
| 25 | CS2231 | Mô hình tri thức quan hệ và ứng dụng (Relational Knowledge Models and Applications) | 3 | 3 | 0 | 2,3 |

### Research Topics Block (Các chuyên đề nghiên cứu, ≥ 12 TC)

- Learners select and accumulate a minimum of 12 research credits from the list below.

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 26 | CS2307 | Chuyên đề nghiên cứu về công nghệ tri thức (Research Topic: Knowledge Engineering) | 4 | 3 | 1 | 2,3,4 |
| 27 | CS2308 | Chuyên đề nghiên cứu về xử lý ngôn ngữ tự nhiên (Research Topic: Natural Language Processing) | 4 | 3 | 1 | 2,3,4 |
| 28 | CS2309 | Chuyên đề nghiên cứu về thị giác máy tính (Research Topic: Computer Vision) | 4 | 3 | 1 | 2,3,4 |
| 29 | CS2310 | Chuyên đề nghiên cứu về máy học và trí tuệ nhân tạo (Research Topic: Machine Learning and Artificial Intelligence) | 4 | 3 | 1 | 2,3,4 |
| 30 | CS2311 | Chuyên đề nghiên cứu về một số vấn đề chọn lọc trong khoa học máy tính (Research Topic: Selected Topics in Computer Science) | 4 | 3 | 1 | 2,3,4 |

### Graduation Thesis (Luận văn tốt nghiệp, 15 TC)

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 31 | CS2506 | Luận văn tốt nghiệp hướng nghiên cứu – Phương thức 2 (Research-oriented Graduation Thesis – Method 2) | 15 | 15 | 0 | 4 |

**Total credits – Method 2:** ≥ 60 TC (3 + 2 + 28 + 12 + 15)

---

## 6.3 Application Track (Chương trình ứng dụng)

### General Knowledge Block (Kiến thức chung, ≥ 3 TC)

- Philosophy (Triết học) is a compulsory course in the General Knowledge block.
- Learners may optionally take the Mathematics course.

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 1 | PH2001 | Triết học (Philosophy) | 3 | 3 | 0 | 1 |
| 2 | MA2001 | Toán học (Mathematics) | 4 | 4 | 0 | 1 |

### Foundation Knowledge Block (Kiến thức cơ sở, 2 TC)

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 3 | CS2205 | Phương pháp nghiên cứu khoa học (Scientific Research Methods) | 2 | 2 | 0 | 1 |

### Specialized Knowledge Block (Kiến thức chuyên ngành, ≥ 43 TC)

- Learners select and accumulate a minimum of 43 credits from the elective list below.

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 4 | NT2102 | An toàn và bảo mật hệ thống thông tin (Information System Security) | 4 | 4 | 0 | 2,3 |
| 5 | IT2011 | Cơ sở dữ liệu nâng cao (Advanced Databases) | 4 | 4 | 0 | 2,3 |
| 6 | IT2021 | Xử lý tín hiệu số nâng cao (Advanced Digital Signal Processing) | 3 | 3 | 0 | 2,3 |
| 7 | IT2030 | Hệ thống thông tin địa lý nâng cao (Advanced Geographic Information Systems) | 3 | 3 | 0 | 2,3 |
| 8 | CS2201 | Biểu diễn tri thức và suy luận (Knowledge Representation and Reasoning) | 4 | 4 | 0 | 2,3 |
| 9 | CS2202 | Ngôn ngữ học máy tính (Computational Linguistics) | 4 | 4 | 0 | 2,3 |
| 10 | CS2203 | Xử lý ảnh và thị giác máy tính (Image Processing and Computer Vision) | 4 | 4 | 0 | 2,3 |
| 11 | CS2207 | Khai thác dữ liệu và ứng dụng (Data Mining and Applications) | 4 | 4 | 0 | 2,3 |
| 12 | CS2208 | Hệ hỗ trợ quyết định (Decision Support Systems) | 3 | 3 | 0 | 2,3 |
| 13 | CS2209 | Dịch máy (Machine Translation) | 3 | 3 | 0 | 2,3 |
| 14 | CS2213 | Xử lý tiếng nói và giao tiếp người máy (Speech Processing and Human-Machine Interaction) | 4 | 4 | 0 | 2,3 |
| 15 | CS2215 | Điện toán lưới và đám mây (Grid and Cloud Computing) | 3 | 3 | 0 | 2,3 |
| 16 | CS2218 | Lý thuyết mã hóa thông tin (Information Coding Theory) | 3 | 3 | 0 | 2,3 |
| 17 | CS2223 | Nguyên lý và phương pháp lập trình (Programming Principles and Methods) | 3 | 3 | 0 | 2,3 |
| 18 | CS2224 | Tìm kiếm thông tin thị giác (Visual Information Retrieval) | 3 | 3 | 0 | 2,3 |
| 19 | CS2225 | Nhận dạng thị giác và ứng dụng (Visual Recognition and Applications) | 3 | 3 | 0 | 2,3 |
| 20 | CS2226 | Ontology và ứng dụng (Ontology and Applications) | 3 | 3 | 0 | 2,3 |
| 21 | CS2227 | Máy học trong xử lý dữ liệu Y khoa (Machine Learning in Medical Data Processing) | 4 | 4 | 0 | 2,3 |
| 22 | CS2228 | Các thuật toán tiến hóa (Evolutionary Algorithms) | 4 | 4 | 0 | 2,3 |
| 23 | CS2229 | Thuật toán và lý thuyết máy học (Machine Learning Algorithms and Theory) | 4 | 4 | 0 | 2,3 |
| 24 | CS2230 | Các mô hình học sâu và ứng dụng (Deep Learning Models and Applications) | 4 | 4 | 0 | 2,3 |
| 25 | CS2231 | Mô hình tri thức quan hệ và ứng dụng (Relational Knowledge Models and Applications) | 3 | 3 | 0 | 2,3 |

### Graduation Thesis (Luận văn tốt nghiệp, 12 TC)

| STT | Mã môn học | Học phần | Số TC | LT | TH | HK |
|---|---|---|---|---|---|---|
| 26 | CS2501 | Luận văn tốt nghiệp hướng ứng dụng (Application-oriented Graduation Thesis) | 12 | 12 | 0 | 4 |

**Total credits – Application Track:** ≥ 60 TC (3 + 2 + 43 + 12)

**Note (Ghi chú):** Learners are allowed to select and accumulate courses from other master's programs of the University to fulfill the Specialized Knowledge block, up to a maximum of 12 credits (Học viên được phép chọn và tích lũy các môn học từ các CTĐT thạc sĩ khác của Trường để làm môn học thuộc khối kiến thức chuyên ngành, nhưng không quá 12 TC).

---

## 6.4 Semester Overview (Mapping to HK)

| Semester (HK) | Content |
|---|---|
| HK 1 | General Knowledge (Philosophy, optional Mathematics) + Foundation Knowledge (Scientific Research Methods) |
| HK 2 | Specialized/Elective courses begin; Method 1 thesis starts |
| HK 3 | Specialized/Elective courses continue; Research Topics (Method 2) |
| HK 4 | Graduation Thesis defense (Method 2 and Application Track) |

## Thesis

Three thesis options exist depending on the chosen track:

| Track | Course Code | Thesis Title | Credits |
|---|---|---|---|
| Research – Method 1 | CS2505 | Luận văn tốt nghiệp hướng nghiên cứu, phương thức 1 | 53 |
| Research – Method 2 | CS2506 | Luận văn tốt nghiệp hướng nghiên cứu, phương thức 2 | 15 |
| Application | CS2501 | Luận văn tốt nghiệp hướng ứng dụng | 12 |

**Note:** This curriculum was imported from the official UIT "Khung chương trình đào tạo Thạc sĩ ngành Khoa học Máy tính" document (p.29 onward). Course descriptions, prerequisites, and full COURSE_SCHEMA.md records for each code above should be authored in `courses/` and cross-referenced back to this table per Section 9 (Handbook Mapping) and Section 10 (Course Repository).

---

# 7. Curriculum Dependency Rules

Every course must explicitly declare:

- Required prerequisites
- Recommended background
- Subsequent courses
- Related courses

The dependency graph must remain acyclic.

Circular dependencies are prohibited.

---

# 8. Knowledge Dependency

Knowledge should progress as follows.

```text
Mathematics
        ↓
Computer Science Foundations
        ↓
Algorithms
        ↓
Computer Systems
        ↓
Artificial Intelligence
        ↓
Advanced AI
        ↓
Research Methodology
        ↓
Master's Thesis
```

---

# 9. Handbook Mapping

Every course must be mapped into the handbook.

```text
Course
        ↓
Volume
        ↓
Part
        ↓
Chapter
        ↓
Knowledge Units
```

No course should exist without a handbook mapping.

---

# 10. Course Repository

Each course is stored separately.

```text
courses/

ADVANCED_ALGORITHMS.md

MACHINE_LEARNING.md

DATA_MINING.md

DISTRIBUTED_SYSTEMS.md

COMPUTER_VISION.md

RESEARCH_METHODOLOGY.md
```

Each file follows COURSE_SCHEMA.md.

---

# 11. Graduate Competencies

After completing the handbook, learners should be able to:

- Understand graduate-level Computer Science.
- Analyze complex algorithms.
- Design scalable systems.
- Build AI applications.
- Read and evaluate research papers.
- Conduct independent research.
- Complete a master's thesis.
- Continue toward doctoral-level study.

---

# 12. Validation Checklist

Before freezing this document:

☑ Official UIT curriculum imported.

□ Every course has a dedicated course file.

□ Every prerequisite verified.

□ Every course mapped to handbook volumes.

□ Every course mapped to handbook chapters.

□ No duplicate course exists.

□ No circular dependency exists.

□ Curriculum approved.

---

# 13. Related Documents

- PROJECT.md
- BOOK_STRUCTURE.md
- COURSE_SCHEMA.md
- CURRICULUM_MAPPING.md
- KNOWLEDGE_GRAPH.md
- MASTER_SYSTEM_PROMPT.md