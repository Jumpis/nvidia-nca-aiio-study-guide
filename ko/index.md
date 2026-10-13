---
layout: korean
title: 한국어 학습 가이드
nav_order: 9
has_children: true
permalink: /ko/
lang: ko
---

{% assign original_page = site.pages | where: "path", "index.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# NVIDIA NCA-AIIO Study Guide — 한국어 학습 가이드
{: .fs-9 }

**NVIDIA Certified Associate: AI Infrastructure and Operations (NCA-AIIO)** 대비 자료입니다. Study notes, 공식 course screenshots, architecture diagrams, 실제 enterprise AI 구축 경험을 한글로 설명합니다.
{: .fs-6 .fw-300 }

[Exam Overview부터 시작]({{ '/ko/00-exam-overview/' | relative_url }}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Domain 1 학습]({{ '/ko/01-essential-ai-knowledge/' | relative_url }}){: .btn .fs-5 .mb-4 .mb-md-0 }

**영어 시험 대비:** 기술 용어와 제품명은 영어로 유지합니다. 한글 설명을 읽은 뒤 영어 용어만 보고 개념을 설명해 보세요. Diagram과 screenshot의 영어 표기는 그대로 유지합니다. 이 번역본은 저장소 원본을 기준으로 하며 시험 정책과 제품 사양의 최신 여부는 별도로 확인해야 합니다.

[번역 원칙과 원문 주의사항]({{ '/ko/translation-notes/' | relative_url }})에서 번역 기준과 확인된 원본 불일치를 확인하세요.

## NCA-AIIO란?

**NVIDIA Certified Associate: AI Infrastructure and Operations**는 enterprise 규모에서 GPU-accelerated AI infrastructure를 배포, 관리, 최적화하는 능력을 평가하는 시험입니다. NVIDIA가 발급하고 online proctoring으로 진행하며, AI data center를 구축하는 hyperscaler, system integrator, enterprise IT 팀에서 활용합니다.

| 항목 | 내용 |
|---|---|
| **Questions** | 50문항 |
| **Duration** | 60분 |
| **Fee** | $125 USD |
| **Validity** | 2년 |
| **Format** | Online proctored (Pearson VUE) |

## 세 개의 exam domain

```
38% ████████████████░░░░░░░░░░  Domain 1 — Essential AI Knowledge
40% ████████████████████░░░░░░  Domain 2 — AI Infrastructure
22% ███████████░░░░░░░░░░░░░░░  Domain 3 — AI Operations
```

### [Domain 1 — Essential AI Knowledge (38%)]({{ '/ko/01-essential-ai-knowledge/' | relative_url }})
NVIDIA software stack · Training과 Inference · AI/ML/DL 기초 · AI adoption 요인 · Use cases · GPU와 CPU · AI lifecycle

### [Domain 2 — AI Infrastructure (40%)]({{ '/ko/02-ai-infrastructure/' | relative_url }})
Hardware requirements · GPU scaling (NVLink/NVSwitch) · Power와 cooling · On-prem과 cloud · Cluster components · Networking (InfiniBand/RoCE/Ethernet) · DPU와 BlueField · Storage

### [Domain 3 — AI Operations (22%)]({{ '/ko/03-ai-operations/' | relative_url }})
Data center management · Orchestration (Kubernetes/Slurm) · GPU monitoring (DCGM) · Virtualization (MIG/vGPU)

## 학습 방법

1. **[Exam Overview]({{ '/ko/00-exam-overview/' | relative_url }})**에서 domain 비중과 학습 전략을 확인합니다.
2. **Domain 1 → 2 → 3 순서로 학습**합니다. 각 section에 설명, diagram, Self-check questions가 있습니다.
3. Domain 2를 공부할 때 **[NVIDIA Portfolio Reference]({{ '/ko/nvidia-portfolio/' | relative_url }})**를 함께 봅니다. Hardware 문항에 필요한 제품 비교 자료입니다.
4. 시험 전 **[Practice & Glossary]({{ '/ko/practice/' | relative_url }})**의 Flashcards와 자체 제작 Quiz로 이해도를 확인합니다.
5. **[Field Notes]({{ '/ko/field-notes/' | relative_url }})**에서 시험 주제가 실제 현장에서 어떻게 적용되는지 읽습니다.

## 권장 NVIDIA training 빠른 참조

Self-paced course: **"AI Infrastructure and Operations Fundamentals"** (NVIDIA DLI, 약 7시간)

| Unit | Topic | Domain |
|---|---|---|
| 1 | AI Transformation Across Industries | 1.5 |
| 2 | Introduction to Artificial Intelligence | 1.3, 1.4 |
| 4 | Accelerating AI With GPUs | 1.8, 2.1 |
| 5 | AI Software Ecosystems | 1.1, 1.7 |
| 7.1 | Data Center Platform | 2.5 |
| 7.4 | Data Center Transformation with DPUs | 2.10 |
| 8 | Networking for AI | 2.7–2.9 |
| 10–11 | Energy-Efficient Computing | 2.3, 2.6 |
| 12.4 | AI in the Cloud | 2.4 |
| 13 | AI Data Center Management & Monitoring | 3.1, 3.3 |
| 14 | Orchestration, MLOps, and Job Scheduling | 3.2, 3.4 |

*CC BY 4.0 · 원본 작성자 Zia Zeeshan · 한국어 번역본 · NVIDIA Corporation과 제휴 관계 없음 · Exam dump 미포함*
