---
layout: korean
title: "1.1 NVIDIA Software Stack (한국어)"
parent: "Domain 1 — Essential AI Knowledge (38%) (한국어)"
nav_order: 1
grand_parent: 한국어 학습 가이드
permalink: /ko/01-essential-ai-knowledge/01-nvidia-software-stack/
lang: ko
---

{% assign original_page = site.pages | where: "path", "01-essential-ai-knowledge/01-nvidia-software-stack.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 1.1 NVIDIA Software Stack in an AI Environment

## 시험에서 평가하는 내용

Bare-metal driver부터 application framework까지 NVIDIA software ecosystem의 주요 layer 이름, 목적, 상호 관계를 이해합니다.

## Stack 개요

```
┌─────────────────────────────────────────────────────────────┐
│              AI Applications & Services                      │
│   (LLM inference, recommendation, computer vision, etc.)    │
├──────────────┬──────────────┬──────────────┬────────────────┤
│   TensorRT   │     NeMo     │    RAPIDS    │   Triton IS    │
│  (inference  │  (LLM/ASR/  │  (data sci)  │  (inference    │
│   optim.)    │   TTS fw.)   │              │   serving)     │
├──────────────┴──────────────┴──────────────┴────────────────┤
│              NVIDIA AI Enterprise (software suite)           │
├──────────────────────────────────────────────────────────────┤
│                    NGC (Container Registry)                   │
├──────────────────────────────────────────────────────────────┤
│           CUDA / cuDNN / cuBLAS / NCCL / cuSPARSE           │
│                   (compute primitives)                        │
├──────────────────────────────────────────────────────────────┤
│              GPU Driver + CUDA Runtime                        │
├──────────────────────────────────────────────────────────────┤
│                   GPU Hardware                                │
└──────────────────────────────────────────────────────────────┘
```

Diagram을 아래에서 위로 읽으면 GPU Hardware → GPU Driver/CUDA Runtime → compute libraries → NGC → NVIDIA AI Enterprise → framework → application 순서입니다.

## Layer별 설명

### CUDA — 기반 platform

**CUDA (Compute Unified Device Architecture)**는 NVIDIA의 parallel computing platform이자 programming model입니다. C/C++/Python 확장을 통해 general-purpose program에서 GPU의 수천 개 core를 활용하도록 합니다.

- 2007년에 등장했으며 GPU compute의 사실상 표준입니다.
- NVIDIA GPU에서 PyTorch, TensorFlow, JAX 같은 AI framework는 내부적으로 CUDA를 사용합니다.
- CUDA toolkit에는 compiler (nvcc), runtime libraries, debugger, profiler (Nsight)가 포함됩니다.

**CUDA 기반 주요 libraries:**

| Library | 목적 |
|---|---|
| **cuDNN** | Deep neural network용 최적화 primitives: convolutions, pooling, normalization |
| **cuBLAS** | GPU-accelerated BLAS: matrix/vector operations |
| **NCCL** | Multi-GPU/multi-node collective communications (all-reduce, broadcast). Distributed training의 핵심 |
| **cuSPARSE** | Sparse matrix operations |
| **RAPIDS** | GPU-accelerated data science. 아래 참조 |

### NGC — NVIDIA GPU Cloud

**NGC**는 GPU-optimized containers, pre-trained models, SDKs를 제공하는 NVIDIA catalog입니다.

- PyTorch, TensorFlow, JAX, TensorRT, NeMo 등을 사전 구성하고 성능을 조정해 제공합니다.
- Container마다 version을 관리하고 특정 GPU driver + CUDA 조합에서 test합니다.
- On-prem server로 pull하거나 주요 cloud GPU instance에서 사용할 수 있습니다.
- **무료로 접근**할 수 있습니다. 이름에 Cloud가 있지만 NGC 자체는 catalog입니다.

**Enterprise AI에서 중요한 이유:** 상호 의존하는 20개 library를 직접 설치하고 version을 관리하는 대신, 사전 구성된 하나의 NGC container를 pull합니다. NVIDIA-certified AI deployment의 일반적인 시작점입니다.

### NVIDIA AI Enterprise

**NVIDIA AI Enterprise**는 NVIDIA-certified hardware (bare metal, VMware vSphere, Red Hat OpenShift 등)에서 실행되는 full-stack enterprise software suite입니다.

- NVIDIA Triton Inference Server, TensorRT, NeMo, RAPIDS, CUDA-X libraries를 포함합니다.
- Production deployment에 중요한 NVIDIA **enterprise support SLA**를 제공합니다.
- GPU 단위의 annual subscription license를 사용합니다.
- 원본은 VM (vGPU)에서도 bare metal과 같은 performance guarantee로 AI를 실행할 수 있다고 설명합니다.

### TensorRT — Inference optimization

**TensorRT**는 high-performance deep learning inference를 위한 NVIDIA SDK입니다.

- 학습된 model (ONNX, TensorFlow, PyTorch)을 target GPU에 맞게 최적화합니다.
- Layer fusion, precision calibration (FP32 → FP16 → INT8 → FP4), kernel auto-tuning, memory layout optimization을 수행합니다.
- 원본의 성능 예시는 raw framework 대비 2–10× 낮은 latency와 높은 throughput입니다.
- **TensorRT-LLM:** LLM inference 전용 확장입니다. Paged KV cache, in-flight batching, continuous batching을 지원합니다.

### Triton Inference Server

Open source **NVIDIA Triton Inference Server**는 표준 HTTP/gRPC inference serving framework를 제공합니다.

- TensorRT, ONNX, PyTorch TorchScript, TensorFlow SavedModel, Python custom backends 등 다양한 model format을 serving합니다.
- **Dynamic batching:** 동시 request를 자동으로 batch로 묶어 GPU utilization을 높입니다.
- **Concurrent model execution:** 한 GPU에서 여러 model을 동시에 실행합니다.
- **Model management:** server 재시작 없이 runtime에 model을 load/unload합니다.
- Kubernetes와 통합해 horizontal scaling을 지원합니다.

### NeMo — Conversational AI framework

**NVIDIA NeMo**는 LLM, automatic speech recognition (ASR), text-to-speech (TTS) model을 개발, training, fine-tuning하는 open-source framework입니다.

- PyTorch Lightning 기반이며 Megatron-LM으로 distributed training을 수행합니다.
- LLM fine-tuning (SFT, RLHF, PEFT/LoRA)을 지원합니다.
- NGC에서 pre-trained model collection을 제공합니다.
- NeMo Guardrails는 production LLM application에 safety/topic control을 추가합니다.

### RAPIDS — GPU-accelerated data science

**RAPIDS**는 data science pipeline 전체를 GPU에서 가속하는 open-source libraries입니다.

| Library | 대응 library | 목적 |
|---|---|---|
| cuDF | pandas | GPU에서 DataFrame operations 수행 |
| cuML | scikit-learn | GPU에서 ML algorithms 실행 |
| cuGraph | NetworkX | GPU에서 graph analytics 수행 |
| cuSpatial | GeoPandas | Geospatial analytics |

PyTorch, TensorFlow와 통합합니다. GPU preprocessing 결과를 GPU training에 직접 전달하여 CPU↔GPU를 반복해서 오가는 비용을 줄입니다.

## Software stack 관계 — 시험 요약

- 빠른 Inference 실행 → **TensorRT**
- 대규모 Inference serving → **Triton Inference Server**
- LLM training/fine-tuning → **NeMo** (+ multi-GPU에는 NCCL)
- GPU-accelerated data preparation → **RAPIDS**
- Enterprise support + vGPU → **NVIDIA AI Enterprise**
- Optimized containers/models → **NGC**
- 위 모든 기술의 기반 → **CUDA**

## Self-check questions

1. NCCL의 full name은 무엇이며 distributed training에 필수적인 이유는 무엇인가요?
2. TensorRT는 deployment 전에 학습된 model에 어떤 작업을 수행하나요?
3. NGC와 NVIDIA AI Enterprise의 차이는 무엇인가요?
4. LLM과 conversational AI training에 사용하는 NVIDIA framework는 무엇인가요?
5. VMware VM에서 enterprise support를 받으며 AI model을 실행하려면 어떤 NVIDIA 제품이 필요한가요?

<details>
<summary>정답과 해설</summary>

1. NVIDIA Collective Communications Library. 여러 node의 GPU에 걸쳐 all-reduce, broadcast 등 collective operations를 제공하여 distributed training의 gradient synchronization을 지원합니다.<br>
2. 특정 GPU에 맞게 layer를 fuse하고 precision (FP32→INT8)을 calibrate하며 kernel을 auto-tune합니다. 결과물인 TensorRT Engine은 latency를 크게 줄입니다.<br>
3. NGC는 GPU-optimized containers/models를 제공하는 무료 catalog입니다. NVIDIA AI Enterprise는 NGC content에 vGPU support와 production tools, enterprise SLA support를 더한 유료 software suite입니다.<br>
4. NeMo (NVIDIA NeMo).<br>
5. NVIDIA AI Enterprise. VMware vSphere (vGPU)에서 인증되어 있으며 enterprise support를 제공합니다.

</details>
