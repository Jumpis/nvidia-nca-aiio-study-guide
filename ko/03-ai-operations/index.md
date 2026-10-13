---
layout: korean
title: "Domain 3 — AI Operations (22%) (한국어)"
nav_order: 4
has_children: true

parent: 한국어 학습 가이드
permalink: /ko/03-ai-operations/
lang: ko
---

{% assign original_page = site.pages | where: "path", "03-ai-operations/index.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# Domain 3 — AI Operations (22%)

Hardware 가동 이후의 day-2 operations를 다룹니다. AI data center에서 GPU resource를 관리, scheduling, monitoring, virtualizing하는 방법을 학습합니다.

## Subdomains

| # | Topic | 문서 |
|---|---|---|
| 3.1 | AI data center management/monitoring 기초 | [datacenter-management]({{ '/ko/03-ai-operations/01-datacenter-management/' | relative_url }}) |
| 3.2 | Cluster orchestration/job scheduling | [orchestration-scheduling]({{ '/ko/03-ai-operations/02-orchestration-scheduling/' | relative_url }}) |
| 3.3 | GPU monitoring 주요 metrics | [gpu-monitoring]({{ '/ko/03-ai-operations/03-gpu-monitoring/' | relative_url }}) |
| 3.4 | Accelerated infrastructure virtualization | [virtualization]({{ '/ko/03-ai-operations/04-virtualization/' | relative_url }}) |

**학습 팁:** DCGM (telemetry/diagnostics), Base Command (DGX cluster management), Slurm/Kubernetes (job scheduling)의 차이를 익히세요. A100/H100의 MIG profiles와 MIG/vGPU/passthrough의 사용 상황을 구분하세요.
