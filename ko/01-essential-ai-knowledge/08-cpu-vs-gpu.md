---
layout: korean
title: "1.8 GPU vs CPU Architecture (한국어)"
parent: "Domain 1 — Essential AI Knowledge (38%) (한국어)"
nav_order: 8
grand_parent: 한국어 학습 가이드
permalink: /ko/01-essential-ai-knowledge/08-cpu-vs-gpu/
lang: ko
---

{% assign original_page = site.pages | where: "path", "01-essential-ai-knowledge/08-cpu-vs-gpu.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 1.8 GPU vs CPU Architecture

## 시험에서 평가하는 내용

Serial/parallel processing의 차이, GPU가 AI workload에 적합한 이유, Blackwell/Hopper/Ada Lovelace/Grace와 workload의 대응 관계를 이해합니다.

## CPU와 GPU의 핵심 차이

![CPUs and GPUs: Components of a System]({{ '/assets/screenshots/cpu-vs-gpu-components.png' | relative_url }})

| 항목 | CPU | GPU |
|---|---|---|
| 최적화 대상 | **Serial tasks** | **Parallel tasks** |
| Core design | 소수의 강력한 cores (4–128) | 수천 개의 단순한 cores |
| Instruction model | 복잡한 instruction sets | 단순하고 반복적인 instructions |
| Memory model | 큰 cache hierarchy, low latency | High bandwidth, 다수 thread로 latency를 숨김 |
| 일반 용도 | OS, databases, business logic | Matrix math, convolutions, rendering |

**AI에서 중요한 이유:** Neural network Training/Inference는 matrix multiplication (GEMM)이 주를 이룹니다. Large Transformer의 forward pass는 수십억 multiply-accumulate operations를 동시에 수행하므로 GPU의 massively parallel architecture에 적합합니다. CPU에서 처리하면 parallelism이 제한되어 Training이 매우 느려집니다.

## Workload별 Architecture

![Different Architectures for Different Workloads]({{ '/assets/screenshots/gpu-architectures-overview.png' | relative_url }})

NVIDIA의 네 processor architecture는 서로 다른 분야를 대상으로 합니다.

| Architecture | Product | 주요 workload |
|---|---|---|
| **Blackwell / Blackwell Ultra** | B200, B300 | Generative AI, AI reasoning, LLM Training/Inference. AI Factory의 핵심 |
| **Hopper** | H100, H200 | Data analytics, conversational AI, language processing. 세계 AI infrastructure의 핵심 |
| **Ada Lovelace** | L40S, L40, L4 | Gaming, AI-powered graphics, ray tracing, premium rendering |
| **Grace** | Grace CPU | Superchip building block, energy efficiency, HPC cloud |

**시험에서 주의:** Ada Lovelace (L40S)는 data center에서 Inference + graphics를 함께 처리하는 GPU로 자주 등장합니다. Generative AI Inference와 professional visualization을 지원해 VDI/AI video에 활용됩니다. H100은 이러한 graphics보다는 HPC/AI compute를 위해 설계되었습니다.

## 알아야 할 GPU architecture features

### Tensor Cores

NVIDIA GPU의 SMs (Streaming Multiprocessors)에 내장된 matrix-math accelerators입니다. 세대가 바뀔 때마다 throughput이 증가하고 precision format이 추가됩니다.

| Generation | GPU | Supported precisions |
|---|---|---|
| 1st | Volta (V100) | FP16 |
| 2nd | Turing (T4) | FP16, INT8, INT4 |
| 3rd | Ampere (A100) | FP16, BF16, TF32, INT8, FP64 |
| 4th | Hopper (H100), Ada (L40S) | FP8 추가 |
| 5th | Blackwell (B200) | FP4 추가, 2nd-gen Transformer Engine |

### CUDA Cores

General-purpose shader processors입니다. 각 SM에는 다수의 CUDA Cores와 소수의 Tensor Cores가 있습니다. CUDA Cores는 memory operations, activation functions 등 matrix 이외의 workload를 처리합니다.

### High Bandwidth Memory (HBM)

GPU package에 직접 탑재된 stacked DRAM입니다. GDDR보다 훨씬 높은 memory bandwidth를 제공합니다.

- H100 SXM: **3.35 TB/s** (HBM3).
- B200: **8 TB/s** (HBM3e). H100의 두 배 이상입니다.

LLM Inference는 token마다 model weights를 load하므로 memory bandwidth가 bottleneck인 경우가 많습니다. HBM bandwidth가 중요한 이유입니다.

### NVLink (chip-to-chip)

High-speed direct GPU interconnect입니다. System 내부 GPU↔GPU 연결과 Superchip의 CPU↔GPU 연결에 사용되며 세대별 bandwidth는 다음과 같습니다.

- NVLink 3 (Ampere): bidirectional 600 GB/s.
- NVLink 4 (Hopper): bidirectional 900 GB/s.
- NVLink 5 (Blackwell): bidirectional 1.8 TB/s.

## Self-check questions

1. CPU는 어떤 task에, GPU는 어떤 task에 최적화되어 있나요?
2. "The engine for the world's AI infrastructure"라고 소개되는 NVIDIA architecture는 무엇인가요?
3. NVIDIA data center GPU에서 가장 높은 bandwidth를 제공하는 memory는 무엇인가요?
4. 한 GPU로 LLM Inference와 professional graphics rendering을 처리하려면 어떤 family가 적합한가요?
5. Tensor Cores와 CUDA Cores의 목적은 어떻게 다른가요?

<details>
<summary>정답과 해설</summary>

1. Serial; Parallel.<br>
2. Hopper (H100).<br>
3. HBM (High Bandwidth Memory).<br>
4. Ada Lovelace (L40S).<br>
5. Tensor Cores는 Deep Learning의 핵심인 matrix multiply-accumulate (GEMM)에 특화되어 있습니다. CUDA Cores는 general parallel compute tasks를 처리합니다.

</details>
