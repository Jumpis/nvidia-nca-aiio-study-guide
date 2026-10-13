---
layout: korean
title: "Field Notes (한국어)"
nav_order: 6
has_children: true

parent: 한국어 학습 가이드
permalink: /ko/field-notes/
lang: ko
---

{% assign original_page = site.pages | where: "path", "field-notes/index.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# Field Notes

Enterprise AI infrastructure deployment에서 얻은 경험입니다. Customer/vendor confidential data와 NDA 대상 수치는 제거한 원본의 1인칭 기록과 의견을 번역합니다.

**자료 범위:** 일반 engineering 경험을 바탕으로 한 original writing입니다. 독점 정보, 고객명, vendor별 계약 세부 사항을 포함하지 않습니다.

| 문서 | 내용 |
|---|---|
| [Sizing a First Cluster]({{ '/ko/field-notes/01-sizing-a-first-cluster/' | relative_url }}) | Use case에 따른 GPU count/storage/networking 추정 |
| [Lessons Learned]({{ '/ko/field-notes/lessons-learned/' | relative_url }}) | 날짜별 짧은 경험 기록 |
