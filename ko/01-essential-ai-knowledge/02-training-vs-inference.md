---
layout: korean
title: "1.2 Training vs Inference (한국어)"
parent: "Domain 1 — Essential AI Knowledge (38%) (한국어)"
nav_order: 2
grand_parent: 한국어 학습 가이드
permalink: /ko/01-essential-ai-knowledge/02-training-vs-inference/
lang: ko
---

{% assign original_page = site.pages | where: "path", "01-essential-ai-knowledge/02-training-vs-inference.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 1.2 Training vs Inference Architecture Requirements

## 시험에서 평가하는 내용

Training과 Inference workload의 compute, memory, latency 특성이 어떻게 다른지, 각 workload에 맞는 NVIDIA hardware/software가 무엇인지 이해합니다.

## AI deployment의 두 단계

```
┌──────────────────────────────────────────────────────────────┐
│  TRAINING                          INFERENCE                  │
│                                                               │
│  Objective: Find model weights     Objective: Use weights to  │
│  that minimize loss function       answer a query fast        │
│                                                               │
│  Runs: Once (or iteratively)       Runs: Millions of times    │
│  Duration: Hours to weeks          Duration: Milliseconds     │
│  Users: Data scientists            Users: End-users/apps      │
└──────────────────────────────────────────────────────────────┘
```

**Training**은 loss function을 최소화하는 model weights를 찾습니다. 한 번 또는 반복적으로 실행하며 수시간부터 수주가 걸리고, 주 사용자는 data scientist입니다. **Inference**는 학습된 weights로 query에 빠르게 응답합니다. 수백만 번 실행되며 보통 milliseconds 단위이고, end-user/application이 사용합니다.

## Training requirements

### Compute 특성

- **Massive FLOPS:** forward + backward pass마다 수십억~수조 번의 multiply-accumulate operation이 필요합니다.
- **FP16/BF16 training:** 현대 training은 주로 BF16을 사용합니다. FP16보다 dynamic range가 넓으며 Tensor Core로 가속합니다.
- **Gradient computation:** backpropagation은 intermediate activations를 저장해야 하므로 memory 사용량이 큽니다.

### Memory requirements

- **Large model memory:** GPT-4급 model은 전체 GPU memory가 수십 TB에 이를 수 있습니다.
- **High memory bandwidth:** weights loading과 gradient accumulation이 HBM bandwidth를 많이 사용합니다.
- **Distributed training의 NVLink + InfiniBand:** model, data, pipeline parallelism으로 작업을 여러 GPU에 분산합니다.

### Parallelism strategies

| Strategy | 분산 대상 | 사용하는 상황 |
|---|---|---|
| **Data parallelism** | GPU별 mini-batch | Model이 GPU 하나에 들어가며 throughput을 확장할 때 |
| **Model/tensor parallelism** | Model layers/tensors | Model이 GPU 하나에 들어가지 않을 때 |
| **Pipeline parallelism** | Pipeline stage별 layers | 매우 깊은 model에서 micro-batch pipelining을 할 때 |
| **Sequence parallelism** | 긴 context의 attention | Sequence가 매우 길 때 |

NCCL은 GPU/node 사이의 gradients를 synchronize하는 all-reduce를 처리합니다.

### Training에 적합한 hardware

- **H100 SXM5 / B200 SXM:** 높은 HBM bandwidth, 4th/5th-gen NVLink, 높은 compute density.
- **DGX H100 / DGX B200:** NVSwitch로 8개 GPU를 all-to-all 연결하여 large model training에 적합합니다.
- **InfiniBand (HDR/NDR/800G):** 대규모 multi-node training용이며 RoCE도 대안입니다.

## Inference requirements

### Compute 특성

- **Low latency:** 사용자 대상 application은 100ms 미만의 response time을 요구할 수 있습니다.
- **High throughput:** 많은 동시 사용자를 serving해야 합니다.
- **INT8 / FP8 / FP4 quantization:** precision을 줄이면서 accuracy를 유지하고 FP16 대비 throughput을 2배 또는 4배 높이는 것이 목표입니다.

### Memory requirements

- **KV cache:** Transformer inference는 token/layer별 key-value tensors를 저장합니다. Context length가 길수록 커집니다.
- **Model이 GPU memory에 들어가야 함:** 일반적으로 단일 GPU 또는 작은 GPU cluster에서 실행합니다.
- 작은 batch에서는 **FLOPS보다 memory bandwidth가 더 중요**합니다 (memory-bandwidth bound).

### Inference optimization techniques

| Technique | 설명 |
|---|---|
| **Quantization** | FP32 → FP16 → INT8 → FP8 → FP4. Weights 크기를 줄이고 throughput을 높임 |
| **Layer fusion** | 여러 operation을 하나의 kernel launch로 결합 (TensorRT) |
| **Continuous batching** | 도착하는 여러 request의 token을 같은 forward pass에서 처리 |
| **Paged KV cache** | OS virtual memory처럼 연속되지 않은 page에 KV cache 저장 (TensorRT-LLM) |
| **Speculative decoding** | Draft model이 candidate를 생성하고 target model이 병렬 검증 |

### Inference에 적합한 hardware/software

- **H100 / B200:** 최대 throughput이 필요한 large LLM inference.
- **L40S:** Inference와 graphics를 함께 처리하며 medium model에 비용 효율적입니다.
- **L4:** 작은 model의 edge/cloud inference. TDP 72W로 power efficiency가 높습니다.
- **TensorRT:** 원본은 NVIDIA GPU deployment 전에 TensorRT로 model을 최적화하도록 권장합니다.

## 비교

| 항목 | Training | Inference |
|---|---|---|
| 주요 metric | Throughput (samples/sec, training tokens/sec) | Latency (TTFT, generation tokens/sec) |
| Precision | FP32, BF16, FP16 | INT8, FP8, FP4, FP16 |
| Batch size | 큼. GPU utilization 향상 | 작거나 큼. Dynamic batching 활용 |
| Memory need | 매우 큼: activations + gradients + weights | 상대적으로 적음: weights + KV cache |
| Communication | 다수 GPU의 all-reduce (NCCL) | 소수 GPU의 tensor parallel |
| 주요 NVIDIA tool | NeMo, Megatron-LM, NCCL | TensorRT, Triton, TensorRT-LLM |
| 선호 interconnect | InfiniBand / NVLink | Node 내부 NVLink, scale-out Ethernet |

## 주요 용어

- **TTFT (Time to First Token):** request에서 첫 token 반환까지의 latency. Interactive application에 중요합니다.
- **Tokens/sec:** generation workload의 throughput metric.
- **MFU (Model FLOP Utilization):** theoretical peak FLOPS 중 실제 사용 비율. 좋은 training run의 예시는 40–60%입니다.
- **Batch size:** 병렬 처리하는 sample 수. 커질수록 GPU utilization이 좋아질 수 있으나 latency도 늘어납니다.

## Self-check questions

1. Training에서 intermediate activations를 저장하는 이유는 무엇인가요?
2. Inference를 위해 FP16 model을 INT8로 quantize하는 목적은 무엇인가요?
3. Distributed training에서 GPU 간 gradients를 synchronize하는 NCCL operation은 무엇인가요?
4. Model parallelism과 Data parallelism의 차이는 무엇인가요?
5. 한 card에서 Inference와 professional visualization을 함께 처리하도록 설계된 NVIDIA GPU는 무엇인가요?

<details>
<summary>정답과 해설</summary>

1. Backpropagation은 output에서 input 방향으로 layer별 gradient를 계산합니다. 각 layer는 forward pass에서 계산한 input activation이 필요하므로 backward pass가 끝날 때까지 저장합니다.<br>
2. 16-bit float 대신 8-bit integer를 사용해 memory를 절반으로 줄이고, INT8을 지원하는 Tensor Core에서 throughput을 2배로 높이는 것이 목적입니다. Accuracy 손실은 calibrated quantization으로 최소화합니다.<br>
3. All-reduce. 각 GPU의 local gradient를 합산한 뒤 모든 GPU가 같은 결과를 얻어 weights를 synchronize해 update합니다.<br>
4. Data parallelism은 각 GPU에 전체 model copy를 두고 서로 다른 data batch를 처리한 뒤 gradients를 평균냅니다. Model parallelism은 model 자체를 나누어 GPU별로 다른 layers/tensor slices를 저장합니다.<br>
5. L40S (Ada Lovelace). 하나의 card에서 AI inference와 rendering/visualization을 처리합니다.

</details>
