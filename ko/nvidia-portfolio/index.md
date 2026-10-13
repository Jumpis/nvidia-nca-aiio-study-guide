---
layout: korean
title: "NVIDIA Portfolio Reference (한국어)"
nav_order: 5
has_children: true

parent: 한국어 학습 가이드
permalink: /ko/nvidia-portfolio/
lang: ko
---

{% assign original_page = site.pages | where: "path", "nvidia-portfolio/index.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# NVIDIA Portfolio Reference

시험에 나오는 NVIDIA hardware/software를 한곳에서 비교합니다. Domain 1/2와 함께 사용하세요. 제품 선택 문항에 필요한 catalog입니다.

| 문서 | 내용 |
|---|---|
| [GPUs]({{ '/ko/nvidia-portfolio/gpus/' | relative_url }}) | Blackwell (B200/B300), Hopper (H100/H200), Ada Lovelace (L40S), Grace CPU |
| [Superchips]({{ '/ko/nvidia-portfolio/superchips/' | relative_url }}) | GH200, GB200, GB300: CPU+GPU packages |
| [DGX Systems]({{ '/ko/nvidia-portfolio/dgx-systems/' | relative_url }}) | DGX H100/B200/B300, GB300 NVL72, RTX PRO Server |
| [Networking Hardware]({{ '/ko/nvidia-portfolio/networking/' | relative_url }}) | ConnectX, BlueField, Quantum-X800, Spectrum-X, Photonics switches |
| [Software Stack]({{ '/ko/nvidia-portfolio/software/' | relative_url }}) | CUDA, NGC, AI Enterprise, Triton, TensorRT, NeMo, DOCA, Base Command |
