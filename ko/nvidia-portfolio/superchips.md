---
layout: korean
title: "Superchips (한국어)"
parent: "NVIDIA Portfolio Reference (한국어)"
nav_order: 2
grand_parent: 한국어 학습 가이드
permalink: /ko/nvidia-portfolio/superchips/
lang: ko
---

{% assign original_page = site.pages | where: "path", "nvidia-portfolio/superchips.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# NVIDIA Superchips

Superchip은 **NVLink-C2C**로 Grace CPU와 하나 이상의 GPU dies를 결합합니다. PCIe보다 높은 bandwidth의 coherent CPU↔GPU memory access를 제공합니다.

## NVLink-C2C와 Superchip

Standard server의 CPU/GPU는 PCIe (~64 GB/s 예시)로 통신하므로 memory copy가 bottleneck이 될 수 있습니다.

- **Coherent memory address space:** software에서 통합된 memory pool로 access합니다.
- 명시적인 copy 없이 CPU에서 GPU memory, GPU에서 CPU memory에 access할 수 있습니다.
- 원본은 C2C bandwidth를 GH200 900 GB/s, GB300 1.8 TB/s로 제시하고 PCIe보다 10–25× 빠르다고 설명합니다. **GPU-to-GPU NVLink와 CPU-to-GPU NVLink-C2C의 bandwidth를 같은 사양으로 단정하지 말고 해당 제품 specification을 확인하세요.**

## GH200 Grace Hopper Superchip

![Grace Hopper Superchip GH200]({{ '/assets/screenshots/grace-hopper-superchip-gh200.png' | relative_url }})

**구성:** Grace CPU + Hopper GPU, NVLink-C2C 연결. 아래는 원본의 사양 표기입니다.

| Feature | Value |
|---|---|
| GPU | 원본: NVIDIA H100 SXM, 80 GB HBM3 또는 96 GB HBM3e |
| CPU | NVIDIA Grace, 72-core Arm |
| GPU Memory | 96 GB HBM3e |
| CPU Memory | 624 GB LPDDR5X |
| NVLink-C2C BW | Bidirectional 900 GB/s |
| Key use | Large unified memory가 필요한 AI Training/HPC/scientific computing |

**주요 workload:** GPU compute와 large-memory access가 결합된 scientific AI. Computational fluid dynamics, molecular dynamics, genomics, large-context LLM Fine-tuning 등입니다.

**원본 주의:** GH200의 memory configuration과 표의 GPU/CPU memory 표기는 model별 확인이 필요합니다. 개념상 핵심은 Grace + Hopper + coherent NVLink-C2C입니다.

## GB200 Grace Blackwell Superchip

![Grace Blackwell Superchip GB200/GB300]({{ '/assets/screenshots/grace-blackwell-superchip-gb200-gb300.png' | relative_url }})

**구성:** Grace CPU + **두 Blackwell GPUs**, NVLink-C2C 연결.

| Feature | Value |
|---|---|
| GPUs | 원본: 2× NVIDIA Blackwell B200 |
| CPU | NVIDIA Grace, 72-core Arm |
| GPU Memory | 원본: ~384 GB HBM3e, 2× B200 계산 |
| CPU Memory | LPDDR5X |
| NVLink-C2C | 원본: 2nd generation |
| Key capability | 두 GPU와 CPU의 unified memory에 coherent access |

**주요 workload:** Large-scale Generative AI, scientific multi-task simulation, 3D data용 energy-efficient Transformer models. Standalone B200와 GB200의 GPU memory configuration은 구분해서 확인해야 합니다.

## Grace Superchip

![Grace Superchip]({{ '/assets/screenshots/grace-superchip.png' | relative_url }})

**구성:** NVLink-C2C로 **두 Grace CPU dies**를 연결한 CPU-only Superchip.

| Feature | Value |
|---|---|
| CPUs | 2× Grace, 2×72 = 144 Arm cores |
| Interconnect | NVLink C2C, bidirectional 900 GB/s |
| Memory | High-bandwidth LPDDR5X |
| TDP | Energy-efficient HPC를 목표로 설계 |
| Form factor | Flagship x86-64 workstation/server platform의 대안 |

**주요 용도:**

- 방대한 data를 처리하는 data center high-performance CPU.
- Cloud/hyperscale용 energy-efficient computing.
- GPU 없는 scientific computing/data analytics.
- Grace CPU는 GB200/GB300의 CPU building block입니다. **두 CPU로 구성된 Grace CPU Superchip 자체와, 한 Grace CPU를 쓰는 Grace Blackwell Superchip은 구분**합니다.

## 비교 — 원본 사양 참조

| 항목 | GH200 | GB200 | Grace Superchip |
|---|---|---|---|
| CPU | 1× Grace | 1× Grace | 2× Grace |
| GPU | 원본: 1× H100 | 원본: 2× B200 | 없음 |
| GPU Memory | 원본: 96 GB | 원본: ~384 GB | — |
| CPU Memory | 원본: 624 GB LPDDR5X | LPDDR5X | LPDDR5X |
| NVLink-C2C gen | 원본: 1st | 원본: 2nd | 원본: 1st |
| 핵심 차이 | Large unified memory AI/HPC | Blackwell AI compute | CPU-only energy-efficient HPC |
