---
layout: korean
title: "Domain 1 — Essential AI Knowledge (38%) (한국어)"
nav_order: 2
has_children: true

parent: 한국어 학습 가이드
permalink: /ko/01-essential-ai-knowledge/
lang: ko
---

{% assign original_page = site.pages | where: "path", "01-essential-ai-knowledge/index.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# Domain 1 — Essential AI Knowledge (38%)

시험에서 38%를 차지하는 domain입니다. NVIDIA software ecosystem, AI/ML/DL 기초, GPU와 CPU architecture, 전체 AI development lifecycle을 다룹니다. 가장 높은 비중은 Domain 2 (40%)입니다.

## Subdomains

| # | Topic | 문서 |
|---|---|---|
| 1.1 | AI 환경의 NVIDIA software stack | [nvidia-software-stack]({{ '/ko/01-essential-ai-knowledge/01-nvidia-software-stack/' | relative_url }}) |
| 1.2 | Training과 Inference의 architecture requirements | [training-vs-inference]({{ '/ko/01-essential-ai-knowledge/02-training-vs-inference/' | relative_url }}) |
| 1.3 | AI, ML, DL의 차이 | [ai-ml-dl]({{ '/ko/01-essential-ai-knowledge/03-ai-ml-dl/' | relative_url }}) |
| 1.4 | 최근 AI 성능 향상과 adoption 요인 | [ai-adoption-drivers]({{ '/ko/01-essential-ai-knowledge/04-ai-adoption-drivers/' | relative_url }}) |
| 1.5 | 주요 AI use cases와 산업 | [use-cases-industries]({{ '/ko/01-essential-ai-knowledge/05-use-cases-industries/' | relative_url }}) |
| 1.6 | NVIDIA solutions의 목적과 use cases | [nvidia-solutions]({{ '/ko/01-essential-ai-knowledge/06-nvidia-solutions/' | relative_url }}) |
| 1.7 | AI development/deployment lifecycle의 software components | [ai-lifecycle]({{ '/ko/01-essential-ai-knowledge/07-ai-lifecycle/' | relative_url }}) |
| 1.8 | GPU와 CPU architecture | [cpu-vs-gpu]({{ '/ko/01-essential-ai-knowledge/08-cpu-vs-gpu/' | relative_url }}) |

**학습 팁:** 1.6과 1.8에 공식 NVIDIA course screenshots가 있습니다. 시험은 구체적인 제품명과 기능을 물으므로 slide를 꼼꼼히 확인하세요.
