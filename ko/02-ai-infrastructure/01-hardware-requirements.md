---
layout: korean
title: "2.1 Hardware Requirements for AI Training (한국어)"
parent: "Domain 2 — AI Infrastructure (40%) (한국어)"
nav_order: 1
grand_parent: 한국어 학습 가이드
permalink: /ko/02-ai-infrastructure/01-hardware-requirements/
lang: ko
---

{% assign original_page = site.pages | where: "path", "02-ai-infrastructure/01-hardware-requirements.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 2.1 Hardware Requirements for Specific AI Training Task Use Cases

## 시험에서 평가하는 내용

AI workload별로 적합한 NVIDIA GPU/system을 연결하고, memory, bandwidth, FLOPS, form factor 요구가 서로 다른 이유를 이해합니다.

## Workload별 GPU 선택

![Data Center GPU Use Case Matrix]({{ '/assets/screenshots/datacenter-gpu-use-case-matrix.png' | relative_url }})

NVIDIA의 공식 workload-to-GPU mapping입니다.

| Workload | Recommended GPUs | 이유 |
|---|---|---|
| **Deep Learning Training & Data Analytics** | B200, GH200, H100, L40S | Gradient compute용 높은 FLOPS와 HBM bandwidth |
| **Deep Learning Inference** | B200, GH200, H100, L40S, L40, L4 | Model size와 latency requirements에 따라 선택 |
| **HPC & Scientific Computing & AI** | B200, GH200, H100, L40S | FP64 compute (H100), 큰 memory capacity |
| **Omniverse / Render Farms** | L40S | RT Cores + GPU rendering, HBM이 필수는 아님 |
| **Virtual Workstation** | L40 | Professional graphics + AI |
| **Virtual Desktop (VDI)** | L4 | Power efficiency, vGPU 지원, 72W TDP |
| **AI Video** | L40S, L4 | Hardware video encode/decode engines |
| **Far Edge Acceleration** | L4 | Low power, standard PCIe slot에 장착 가능 |

## GPU 선택에 영향을 주는 Hardware 요소

### 1. GPU Memory (VRAM)

Model이 GPU memory에 들어가야 합니다. 부족하면 여러 GPU에 model parallelism을 적용하거나 memory가 더 큰 GPU를 사용합니다.

| GPU | Memory | Type |
|---|---|---|
| H100 SXM5 | 80 GB | HBM3 |
| H200 SXM5 | 141 GB | HBM3e |
| B200 SXM | 192 GB | HBM3e |
| L40S | 48 GB | GDDR6 |
| L4 | 24 GB | GDDR6 |

**LLM Inference의 간단한 계산:** 70B-parameter FP16 model의 weights는 약 140 GB (2 bytes × 70B)입니다. Weights 기준으로 2× H100 SXM (각 80GB) 또는 1× B200 (192GB)이 필요합니다. 실제 runtime에는 KV cache 등 추가 memory도 필요합니다.

### 2. Memory Bandwidth

작은 batch의 memory-bound Inference와 Training의 weights/activations loading에 중요합니다.

| GPU | Memory Bandwidth |
|---|---|
| H100 SXM5 | 3.35 TB/s |
| H200 SXM5 | 4.8 TB/s |
| B200 | ~8 TB/s |
| L40S | 864 GB/s |

### 3. Compute FLOPS (Tensor Core throughput)

Large-batch Training에 중요합니다. FLOPS가 높을수록 초당 처리하는 sample 수를 늘릴 수 있습니다.

| GPU | Peak (BF16 Tensor Core) |
|---|---|
| A100 80GB | 312 TFLOPS |
| H100 SXM5 | 989 TFLOPS |
| B200 | ~2,250 TFLOPS |

### 4. Interconnect (multi-GPU)

Model/data가 GPU 하나에 들어가지 않으면 GPU 간 communication이 필요하며 bandwidth가 bottleneck이 되기 쉽습니다.

- **NVLink (node 내부):** 900 GB/s (NVLink 4, Hopper) → 1.8 TB/s (NVLink 5, Blackwell).
- **NVSwitch (system 내부):** DGX의 8개 GPU 사이에 all-to-all NVLink를 구성합니다.
- **InfiniBand (node 간):** link당 200/400/800 Gbps. Multi-node Training에 사용합니다.

### 5. Form factor

- **SXM:** DGX 등에 사용하는 high-performance module입니다. 원본은 liquid cooling과 높은 power envelope (GPU당 700W–1000W)를 강조합니다.
- **PCIe:** standard server card. Air/liquid cooling을 사용하며 max TDP는 더 낮고 server compatibility는 넓습니다.
- **OAM (OCP Accelerator Module):** custom hyperscaler platform에서 사용합니다.

## Sizing 예시: LLM pre-training cluster

Hopper H100으로 70B-parameter LLM을 pre-train한다고 가정합니다.

| Requirement | 설명 |
|---|---|
| GPU model memory | 70B × 2 bytes (BF16) = 140 GB. Weights만으로 최소 2× H100 80GB |
| Optimizer states | 원본의 mixed precision 추정은 model memory의 ~4×, 총 ~560 GB |
| Training cluster | 최소 8-GPU DGX H100 node, 일반적인 예시는 64–512 nodes |
| Network | Node 사이 link당 NDR InfiniBand 400Gbps |
| Storage | Training data를 공급하는 high-throughput parallel FS (WEKA/Lustre) |

## Self-check questions

1. VDI에 권장되는 GPU는 무엇인가요?
2. FP16에서 141 GB인 model의 weights를 단일 GPU에 저장하려면 어떤 GPU를 선택하나요?
3. 작은 batch의 Inference에서 memory bandwidth가 FLOPS보다 중요한 이유는 무엇인가요?
4. SXM과 PCIe form factor의 차이는 무엇인가요?
5. 대규모 multi-node LLM Training에 필요한 두 종류의 interconnect는 무엇인가요?

<details>
<summary>정답과 해설</summary>

1. NVIDIA L4. 24 GB GDDR6, 72W TDP, vGPU 지원이며 VDI에 최적화되어 있습니다.<br>
2. 원본 정답은 H200 SXM5 (141 GB HBM3e)입니다. Model weights 용량과 같은 명목 capacity지만, 실제 Inference에는 KV cache/runtime memory를 위한 여유가 필요하므로 이 계산만으로 실행 가능하다고 판단하면 안 됩니다.<br>
3. 작은 batch에서는 weights를 memory에서 load하는 시간이 대부분을 차지합니다. Large batch에서 compute가 지속될 때 FLOPS가 bottleneck이 됩니다. Batch=1의 LLM generation은 bandwidth가 tokens/sec를 직접 제한합니다.<br>
4. SXM은 DGX 등에서 높은 TDP (700–1000W)와 full NVLink bandwidth를 지원합니다. PCIe는 standard slot을 사용해 server compatibility가 넓고 유연하지만 max TDP와 peak performance는 더 낮습니다.<br>
5. Node 내부 all-to-all communication용 NVLink/NVSwitch, cluster의 gradient all-reduce를 위한 node 간 InfiniBand.

</details>
