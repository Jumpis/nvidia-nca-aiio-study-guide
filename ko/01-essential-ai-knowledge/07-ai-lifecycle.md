---
layout: korean
title: "1.7 AI Development and Deployment Lifecycle (한국어)"
parent: "Domain 1 — Essential AI Knowledge (38%) (한국어)"
nav_order: 7
grand_parent: 한국어 학습 가이드
permalink: /ko/01-essential-ai-knowledge/07-ai-lifecycle/
lang: ko
---

{% assign original_page = site.pages | where: "path", "01-essential-ai-knowledge/07-ai-lifecycle.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 1.7 AI Development and Deployment Lifecycle Software Components

## 시험에서 평가하는 내용

AI/ML lifecycle의 단계, 각 단계의 tool, production ML workflow (MLOps)를 지원하는 NVIDIA 제품을 이해합니다.

## AI lifecycle

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  1. Data    │───▶│  2. Model   │───▶│  3. Model   │───▶│  4. Deploy  │
│  Collection │    │  Training   │    │  Validation │    │  & Serve    │
│  & Prep     │    │             │    │             │    │             │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
       │                  │                  │                  │
       ▼                  ▼                  ▼                  ▼
  Data pipelines     NeMo, PyTorch     Experiment         TensorRT +
  RAPIDS (GPU         Megatron-LM       tracking           Triton
  data prep)          NCCL (dist.)      MLflow             Kubernetes
                       DGX cluster      W&B                Base Command
                                                                │
                                                                ▼
                                                    ┌──────────────────┐
                                                    │  5. Monitor &    │
                                                    │  Iterate         │
                                                    │  DCGM, Grafana   │
                                                    │  model drift,    │
                                                    │  retraining      │
                                                    └──────────────────┘
```

Data Collection & Prep → Model Training → Model Validation → Deploy & Serve → Monitor & Iterate 순서입니다. Monitoring 결과를 바탕으로 data와 model을 개선하여 반복합니다.

## Stage 1: Data collection and preparation

### 주요 활동

- Databases, object storage, real-time streams 등에서 data ingestion.
- Cleaning, deduplication, normalization, augmentation.
- Supervised learning용 labeling: 수동 또는 AI-assisted.
- Traditional ML의 feature engineering, LLM의 tokenization.

### NVIDIA tools

- **RAPIDS (cuDF, cuML):** pandas/scikit-learn workflow를 GPU에서 실행하는 GPU-accelerated data processing.
- **DALI (Data Loading Library):** GPU-accelerated data augmentation pipeline으로 vision training의 CPU bottleneck을 줄입니다.

### Storage requirements

- Training dataset은 petabyte 규모에 이를 수 있습니다.
- High-throughput read에 parallel/distributed file systems (Lustre, GPFS, WEKA)가 필요합니다.
- Active training dataset은 hot storage (NVMe/SSD), cold archive는 S3-compatible object storage에 저장합니다.

## Stage 2: Model training

### 주요 활동

- Learning rate, batch size, architecture 등 hyperparameter 선택.
- Multi-GPU/multi-node distributed training.
- Failure recovery를 위한 주기적인 model state checkpointing.
- Experiment tracking.

### NVIDIA tools

- **NeMo:** LLM, ASR, TTS Training/Fine-tuning.
- **Megatron-LM:** NVIDIA large-scale Transformer training의 핵심.
- **NCCL:** Multi-GPU training collective communications.
- **NVIDIA Base Command Platform:** DGX cluster, job scheduling, Training monitoring 관리.

### Infrastructure

- NVSwitch가 있는 DGX: 8개 GPU, all-to-all 900GB/s.
- Multi-node용 InfiniBand fabric: NDR 400G 또는 port당 800G.
- Slurm/Kubernetes job scheduling.

## Stage 3: Model validation and experimentation

### 주요 활동

- Held-out test set으로 model accuracy 평가.
- Experiment tracking으로 run 비교.
- Manual 또는 automated hyperparameter tuning (grid search, Bayesian).
- Model interpretability/explainability.

### 주요 tools

- **MLflow:** open-source experiment tracking, model registry, artifact management.
- **Weights & Biases (W&B):** experiment tracking, visualization, hyperparameter sweeps.
- **NVIDIA Triton model analyzer:** 최적 batch size를 찾도록 model profiling.

## Stage 4: Deployment and serving

### 주요 활동

- Target hardware에 맞춘 model optimization.
- Inference serving infrastructure 구성.
- A/B testing, canary deployments.
- SLA 정의: latency p99, throughput target.

### NVIDIA tools

- **TensorRT:** production GPU에 맞춘 optimization과 FP16/INT8/FP8/FP4 quantization.
- **TensorRT-LLM:** LLM inference 전용, paged KV cache/continuous batching.
- **Triton Inference Server:** production serving, dynamic batching, multi-model/multi-backend.
- **NVIDIA Fleet Command:** edge/branch location의 Inference deployment 관리.

### Deployment targets

- **Cloud:** GPU node의 Kubernetes pods. Queue depth에 따른 KEDA/HPA autoscaling.
- **On-prem:** DGX 또는 NVIDIA-Certified Servers. Triton + Kubernetes 또는 bare metal.
- **Edge:** embedded AI용 NVIDIA Jetson/Orin modules. Fleet Command로 관리.

## Stage 5: Monitoring and iteration

### 주요 활동

- Production model accuracy 추적: data drift/concept drift 감지.
- GPU utilization, temperature, memory 등 infrastructure health monitoring.
- Model performance 저하 시 retraining 실행.
- 새 model version의 A/B testing.

### NVIDIA tools

- **DCGM (Data Center GPU Manager):** GPU health, utilization, errors 추적.
- **Prometheus + Grafana:** time-series metrics와 dashboards. DCGM Exporter가 DCGM → Prometheus를 연결합니다.
- **NVIDIA AI Enterprise:** monitoring integrations와 lifecycle management 포함.

## MLOps pipeline 요약

```
Data Prep → Training → Validation → Registry → Deploy → Monitor → (retrain)
RAPIDS      NeMo       MLflow       NGC         TensorRT   DCGM
DALI        Megatron   W&B          Triton      Triton     Grafana
            NCCL                    Base Cmd    Kubernetes  MLflow
```

Data Prep에는 RAPIDS/DALI, Training에는 NeMo/Megatron/NCCL, Validation에는 MLflow/W&B, deployment/serving에는 TensorRT/Triton/Kubernetes, Monitoring에는 DCGM/Grafana 등을 사용합니다.

## Self-check questions

1. Distributed training의 GPU 간 communication을 담당하는 NVIDIA library는 무엇인가요?
2. Deployment 단계의 TensorRT 목적은 무엇인가요?
3. Training 중 DGX cluster job과 monitoring을 관리하는 NVIDIA tool은 무엇인가요?
4. High-throughput training data access에는 어떤 storage가 필요한가요?
5. Model drift는 무엇이며 retraining을 유발하는 이유는 무엇인가요?

<details>
<summary>정답과 해설</summary>

1. NCCL (NVIDIA Collective Communications Library).<br>
2. 특정 NVIDIA GPU에 맞춰 layer fusion, precision calibration, kernel auto-tuning을 수행하여 Inference throughput을 높이고 latency를 줄입니다.<br>
3. NVIDIA Base Command Platform.<br>
4. NVMe-backed parallel/distributed file systems (Lustre, GPFS, WEKA, WekaFS). GPU에 지속적으로 data를 공급할 높은 IOPS와 bandwidth를 제공합니다.<br>
5. 실제 input data distribution이 training distribution과 달라져 accuracy가 시간에 따라 감소하는 현상입니다. 허용 threshold 아래로 떨어지면 새 data로 retraining해야 합니다.

</details>
