---
layout: korean
title: "GPU Families (한국어)"
parent: "NVIDIA Portfolio Reference (한국어)"
nav_order: 1
grand_parent: 한국어 학습 가이드
permalink: /ko/nvidia-portfolio/gpus/
lang: ko
---

{% assign original_page = site.pages | where: "path", "nvidia-portfolio/gpus.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# NVIDIA GPU Families

Data center GPU 비교 자료입니다. Workload mapping은 [Domain 1.6]({{ '/ko/01-essential-ai-knowledge/06-nvidia-solutions/' | relative_url }})을 확인하세요.

## Architecture 개요

![GPU Architectures Overview]({{ '/assets/screenshots/gpu-architectures-overview.png' | relative_url }})

## Blackwell / Blackwell Ultra — B200, B300

### B200/B300 소개

![Blackwell B200/B300 Intro]({{ '/assets/screenshots/blackwell-b200-b300-intro.png' | relative_url }})

### B200/B300 사양

![Blackwell B200/B300 Specs]({{ '/assets/screenshots/blackwell-b200-b300-specs.png' | relative_url }})

| Spec | Value |
|---|---|
| Architecture | Blackwell / Blackwell Ultra |
| Transistors | 208 billion |
| Transformer Engine | 2nd gen (FP4/FP8/BF16/FP16) |
| NVLink | 5th gen, bidirectional 1.8 TB/s |
| Memory | HBM3e |
| B200 TDP | SXM ~1,000 W |
| Key features | 2nd-gen Transformer Engine, Secure AI, Decompression Engine |

**주요 workload:** Generative AI (LLM Training/Inference), AI reasoning, biology/drug discovery.

## Hopper — H100, H200

![H100 Hopper]({{ '/assets/screenshots/hopper-h100.png' | relative_url }})

| Spec | H100 SXM5 | H100 PCIe | H200 SXM5 |
|---|---|---|---|
| Memory | 80 GB HBM3 | 80 GB HBM2e | 141 GB HBM3e |
| Memory BW | 3.35 TB/s | 2.0 TB/s | 4.8 TB/s |
| Peak TF (BF16) | 989 TFLOPS | 756 TFLOPS | 989 TFLOPS |
| NVLink | 4th gen, 900 GB/s | 4th gen | 4th gen |
| TDP | 700 W | 350 W | 700 W |

**주요 workload:** LLMs, data analytics, conversational AI, NLP, autonomous vehicles.

## Ada Lovelace — L40S, L40, L4

### L40S (Data Center)

![L40S Intro]({{ '/assets/screenshots/ada-lovelace-l40s-intro.png' | relative_url }})
![L40S Specs]({{ '/assets/screenshots/ada-lovelace-l40s-specs.png' | relative_url }})

| Spec | L40S | L40 | L4 |
|---|---|---|---|
| Memory | 48 GB GDDR6 | 48 GB GDDR6 | 24 GB GDDR6 |
| Memory BW | 864 GB/s | 864 GB/s | 300 GB/s |
| Tensor Core gen | 4th | 4th | 4th |
| RT Core gen | 3rd | 3rd | 3rd |
| TDP | 350 W | 300 W | 72 W |
| Video codec | AV1 enc/dec | H.264/H.265 | AV1 enc/dec |

**L40S:** Generative AI Inference + graphics, mainstream data center compute, AI video.<br>
**L4:** VDI, edge Inference, low-power AI video.

## Grace CPU

![Grace CPU]({{ '/assets/screenshots/grace-cpu.png' | relative_url }})

| Spec | Value |
|---|---|
| ISA | Arm Neoverse V2, 72 cores |
| Memory | LPDDR5X, proprietary subsystem |
| Interface | Superchip의 GPU 연결용 NVLink-C2C |
| TDP | 원본은 GB200 Superchip 구성에서 ~500 W로 표기 |

**주요 용도:** HPC, hyperscale cloud, Grace Superchip building block. CPU 단독과 Superchip 전체 power는 구분해야 합니다.

## RTX PRO Server

![RTX PRO Server]({{ '/assets/screenshots/rtx-pro-server.png' | relative_url }})

| Spec | RTX PRO 6000 Blackwell Server |
|---|---|
| CUDA cores | 24,064 |
| Tensor Cores | 752, 5th gen |
| RT Cores | 188, 4th gen |
| Architecture | Blackwell |
| Purpose | Data center의 AI + professional visualization |
