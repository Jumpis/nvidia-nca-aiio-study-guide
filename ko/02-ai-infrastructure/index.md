---
layout: korean
title: "Domain 2 — AI Infrastructure (40%) (한국어)"
nav_order: 3
has_children: true

parent: 한국어 학습 가이드
permalink: /ko/02-ai-infrastructure/
lang: ko
---

{% assign original_page = site.pages | where: "path", "02-ai-infrastructure/index.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# Domain 2 — AI Infrastructure (40%)

가장 비중이 큰 domain입니다. AI data center의 사양 결정, 구축, 연결, storage에 필요한 내용을 다루며 공식 NVIDIA course screenshots가 많이 포함되어 있습니다.

## Subdomains

| # | Topic | 문서 |
|---|---|---|
| 2.1 | 특정 AI Training use case의 hardware requirements | [hardware-requirements]({{ '/ko/02-ai-infrastructure/01-hardware-requirements/' | relative_url }}) |
| 2.2 | GPU infrastructure scaling | [gpu-scaling]({{ '/ko/02-ai-infrastructure/02-gpu-scaling/' | relative_url }}) |
| 2.3 | Power와 cooling requirements | [power-and-cooling]({{ '/ko/02-ai-infrastructure/03-power-and-cooling/' | relative_url }}) |
| 2.4 | On-prem과 cloud 비교 | [onprem-vs-cloud]({{ '/ko/02-ai-infrastructure/04-onprem-vs-cloud/' | relative_url }}) |
| 2.5 | Accelerated infrastructure의 cluster components | [cluster-components]({{ '/ko/02-ai-infrastructure/05-cluster-components/' | relative_url }}) |
| 2.6 | Facility requirements | [facility-requirements]({{ '/ko/02-ai-infrastructure/06-facility-requirements/' | relative_url }}) |
| 2.7 | AI workload의 networking requirements | [networking-requirements]({{ '/ko/02-ai-infrastructure/07-networking-requirements/' | relative_url }}) |
| 2.8 | Data center networking protocols와 핵심 개념 | [networking-protocols]({{ '/ko/02-ai-infrastructure/08-networking-protocols/' | relative_url }}) |
| 2.9 | High-speed data center network 선택지와 use cases | [high-speed-network-options]({{ '/ko/02-ai-infrastructure/09-high-speed-network-options/' | relative_url }}) |
| 2.10 | DPU의 목적과 이점 | [dpu]({{ '/ko/02-ai-infrastructure/10-dpu/' | relative_url }}) |

**학습 팁:** 2.7–2.10에는 screenshots와 구체적인 제품명이 가장 많습니다. AI Fabric (E-W)과 control/user-access network (N-S)를 구분하고, RoCE adaptive routing/congestion control을 지원하는 hardware (BlueField-3 + Spectrum-4)를 익히세요.
