---
layout: korean
title: "1.6 NVIDIA Solutions: Purpose and Use Cases (한국어)"
parent: "Domain 1 — Essential AI Knowledge (38%) (한국어)"
nav_order: 6
grand_parent: 한국어 학습 가이드
permalink: /ko/01-essential-ai-knowledge/06-nvidia-solutions/
lang: ko
---

{% assign original_page = site.pages | where: "path", "01-essential-ai-knowledge/06-nvidia-solutions.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 1.6 NVIDIA Solutions: Purpose and Use Cases

## 시험에서 평가하는 내용

주어진 workload에 적합한 GPU/system/software를 선택합니다. Scenario를 제시하고 이를 해결하는 NVIDIA product family를 묻는 문항이 자주 등장합니다.

## GPU families와 Workloads

![Different Architectures for Different Workloads]({{ '/assets/screenshots/gpu-architectures-overview.png' | relative_url }})

### Blackwell / Blackwell Ultra (B200, B300)

![Blackwell B200/B300 — intro]({{ '/assets/screenshots/blackwell-b200-b300-intro.png' | relative_url }})
![Blackwell B200/B300 — specs]({{ '/assets/screenshots/blackwell-b200-b300-specs.png' | relative_url }})

- **주요 용도:** Generative AI, AI reasoning, LLM Training/Inference, biology/AI drug discovery.
- **Architecture 특징:**
  - 208 billion transistors.
  - 2nd-generation Transformer Engine (FP4/FP8/FP16/BF16).
  - 5th-generation NVLink (GPU당 bidirectional 1.8 TB/s).
  - HBM3e high bandwidth memory.
  - Secure AI enclave.
  - Hardware decompression engine.

**핵심 표현:** NVIDIA는 B200/B300을 "the engine of the new industrial revolution"으로 소개합니다. 가장 큰 Generative AI Training/Inference job을 위한 제품이라는 의미입니다.

### Hopper (H100, H200)

![H100 Hopper]({{ '/assets/screenshots/hopper-h100.png' | relative_url }})

- **주요 용도:** LLMs, data analytics, conversational AI, image creation, NLP, autonomous vehicles.
- **주요 사양 (H100 SXM5):**
  - 80 GB HBM3 (H100) / 141 GB HBM3e (H200).
  - Memory bandwidth: 3.35 TB/s (H100) / 4.8 TB/s (H200).
  - FP8을 지원하는 4th-gen Tensor Cores.
  - 4th-gen NVLink (900 GB/s).
  - Layer별 FP8/FP16을 자동 전환하는 Transformer Engine.
  - Confidential Computing 지원.

### Ada Lovelace (L40S, L40, L4)

![L40S — intro]({{ '/assets/screenshots/ada-lovelace-l40s-intro.png' | relative_url }})
![L40S — specs]({{ '/assets/screenshots/ada-lovelace-l40s-specs.png' | relative_url }})

- **주요 용도:** 한 GPU에서 Generative AI Inference + graphics, AI video, premium visualization, 일반 data center compute, VDI/VPC.
- **주요 사양 (L40S):**
  - 48 GB GDDR6. HBM보다 bandwidth가 낮지만 Inference + compute에 활용합니다.
  - 4th-gen Tensor Cores, **3rd-gen RT Cores**. 원본의 5th-gen RT Cores는 잘못된 표기입니다. [NVIDIA L40S 사양](https://www.nvidia.com/en-us/data-center/l40s/) 참조.
  - AV1 encode/decode를 포함한 advanced video acceleration.
  - Data center scalability, security, power efficiency.

**Hopper와의 차이:** L40S는 한 GPU에서 AI Inference와 rendering을 함께 수행하는 범용 card입니다. H100은 compute를 위한 GPU입니다.

### Grace CPU

![Grace CPU]({{ '/assets/screenshots/grace-cpu.png' | relative_url }})

- **주요 용도:** HPC, cloud computing, hyperscale data centers.
- **Architecture:**
  - Arm 기반, 72 Arm Neoverse V2 cores.
  - NVIDIA proprietary memory subsystem.
  - 높은 scalability와 energy efficiency.
  - 대용량/high-bandwidth LPDDR5X memory 지원.
- **역할:** Grace Superchips (GH200, GB200)의 building block입니다. NVLink-C2C로 CPU↔GPU를 긴밀히 통합합니다.

### Data center GPU use-case matrix

![Data Center GPU Use Case Matrix]({{ '/assets/screenshots/datacenter-gpu-use-case-matrix.png' | relative_url }})

NVIDIA의 workload별 GPU 권장 조합입니다.

| Workload | Recommended GPUs |
|---|---|
| Deep Learning Training & Data Analytics | B200, GH200, H100, L40S |
| Deep Learning Inference | B200, GH200, H100, L40S, L40, L4 |
| HPC & Scientific Computing & AI | B200, GH200, H100, L40S |
| Omniverse / Render Farms | L40S |
| Virtual Workstation | L40 |
| Virtual Desktop (VDI) | L4 |
| AI Video | L40S, L4 |
| Far Edge Acceleration | L4 |

## RTX PRO Server

![RTX PRO Server]({{ '/assets/screenshots/rtx-pro-server.png' | relative_url }})

- **제품:** NVIDIA RTX PRO 6000 Blackwell Server Edition GPU.
- **목적:** AI와 visual computing을 함께 처리하는 강력한 Blackwell data center platform.
- **주요 사양:**
  - 24,064 CUDA parallel processing cores.
  - 752 5th-gen Tensor Cores.
  - 188 4th-gen RT Cores.

Engineering simulation, digital twin 등 data center에서 AI acceleration과 photorealistic rendering이 모두 필요한 workload를 대상으로 합니다.

## Self-check questions

1. Generative AI의 "the engine of the new industrial revolution"으로 소개되는 GPU family는 무엇인가요?
2. LLM Inference와 professional visualization을 하나의 GPU에서 수행하려면 어떤 family가 적합한가요?
3. Blackwell의 Transformer Engine이 Hopper 대비 추가한 기능은 무엇인가요?
4. VDI (Virtual Desktop Infrastructure)용 권장 NVIDIA GPU는 무엇인가요?
5. B200에는 transistors가 몇 개 있나요?

<details>
<summary>정답과 해설</summary>

1. Blackwell / Blackwell Ultra (B200, B300).<br>
2. Ada Lovelace (L40S).<br>
3. FP4를 지원하는 2nd-generation Transformer Engine. Hopper는 FP8을 지원하는 1st-generation입니다.<br>
4. VDI에는 L4, Virtual Workstation에는 L40.<br>
5. 208 billion transistors.

</details>
