---
layout: korean
title: "Domain Blueprint (한국어)"
parent: "Exam Overview (한국어)"
nav_order: 2
grand_parent: 한국어 학습 가이드
permalink: /ko/00-exam-overview/domain-blueprint/
lang: ko
---

{% assign original_page = site.pages | where: "path", "00-exam-overview/domain-blueprint.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# Domain Blueprint — 전체 22개 Subdomain

공식 NCA-AIIO 시험은 세 개의 domain으로 구성되며, 채점 대상인 22개 subdomain으로 나뉩니다.

## Domain 1 — Essential AI Knowledge (38%)

| Subdomain | Topic |
|---|---|
| 1.1 | AI 환경의 NVIDIA software stack |
| 1.2 | Training과 Inference의 architecture requirements |
| 1.3 | AI, ML, DL의 차이 |
| 1.4 | 최근 AI 성능 향상과 adoption을 이끈 요인 |
| 1.5 | 주요 AI use cases와 산업 |
| 1.6 | NVIDIA solutions의 목적과 use cases |
| 1.7 | AI development/deployment lifecycle의 software components |
| 1.8 | GPU와 CPU architecture의 차이 |

## Domain 2 — AI Infrastructure (40%)

| Subdomain | Topic |
|---|---|
| 2.1 | 특정 AI training task use case에 필요한 hardware |
| 2.2 | GPU infrastructure scaling |
| 2.3 | Power와 cooling requirements |
| 2.4 | On-premises와 cloud 비교 |
| 2.5 | Accelerated infrastructure의 cluster components |
| 2.6 | Facility requirements |
| 2.7 | AI workload의 networking requirements |
| 2.8 | Data center networking protocols와 핵심 개념 |
| 2.9 | High-speed data center network 선택지와 use cases |
| 2.10 | DPU의 목적과 이점 |

## Domain 3 — AI Operations (22%)

| Subdomain | Topic |
|---|---|
| 3.1 | AI data center management와 monitoring의 기초 |
| 3.2 | Cluster orchestration과 job scheduling |
| 3.3 | GPU monitoring의 주요 metric |
| 3.4 | Accelerated infrastructure virtualization |

## Domain 비중 시각화

```
Domain 1 — Essential AI Knowledge    38%  ████████████████████░░░░░░░░░░
Domain 2 — AI Infrastructure         40%  ████████████████████████░░░░░░
Domain 3 — AI Operations             22%  █████████████░░░░░░░░░░░░░░░░░
```

총 50문항 · 60분 · passing score는 공개되지 않음
