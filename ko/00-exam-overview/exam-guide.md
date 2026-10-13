---
layout: korean
title: "Exam Guide & Study Strategy (한국어)"
parent: "Exam Overview (한국어)"
nav_order: 1
grand_parent: 한국어 학습 가이드
permalink: /ko/00-exam-overview/exam-guide/
lang: ko
---

{% assign original_page = site.pages | where: "path", "00-exam-overview/exam-guide.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# Exam Guide & Study Strategy

## 시험 정보

| 항목 | 내용 |
|---|---|
| Full name | NVIDIA Certified Associate: AI Infrastructure and Operations |
| Short code | NCA-AIIO |
| Delivered by | Pearson VUE (online proctored) |
| Questions | 50문항, multiple choice / multiple select |
| Duration | 60분 |
| Fee | $125 USD |
| Validity | 합격일로부터 2년 |
| Retake policy | 첫 불합격 후 14일, 두 번째 불합격 후 60일 대기 |

## 권장 사전 학습 (NVIDIA 안내)

NVIDIA DLI의 무료 self-paced course **"AI Infrastructure and Operations Fundamentals"** (약 7시간)를 수강합니다. 집중할 unit은 다음과 같습니다.

- Unit 1 — AI Transformation Across Industries
- Unit 2 — Introduction to Artificial Intelligence
- Unit 4 — Accelerating AI With GPUs
- Unit 5 — AI Software Ecosystems
- Unit 7.1 — Data Center Platform
- Unit 7.4 — Data Center Transformation with DPUs
- Unit 8 — Networking for AI
- Units 10–11 — Energy-Efficient Computing
- Unit 12.4 — AI in the Cloud
- Unit 13 — AI Data Center Management and Monitoring
- Unit 14 — Orchestration, MLOps, and Job Scheduling

## 권장 읽기 자료 (공식 study guide 기준)

NVIDIA TensorRT · NVIDIA GPU Operator · DCGM · Base Command · MIG · BlueField DPU offload · DGX SuperPOD reference architecture · DGX H100 system intro · InfiniBand key features · NVSwitch · Network IO · Kubernetes · Slurm · Docker · Out-of-Band management · Run:ai · IBM/SAS ML · Intel CPU vs GPU

## 학습 전략

### Domain 비중에 따른 시간 배분

Domain 2 (AI Infrastructure)가 40%로 가장 큰 비중이므로 우선순위를 높입니다. 대략 다음과 같이 배분합니다.

- Domain 2: 약 3회 학습. 특히 networking 2.7–2.9와 DPU 2.10에 집중합니다.
- Domain 1: 약 2회 학습. NVIDIA product portfolio가 자주 출제됩니다.
- Domain 3: 약 1회 학습.

### 집중 학습할 주제

원본은 공식 study guide의 강조 정도를 바탕으로 다음 주제를 중요하게 다룹니다.

1. **NVLink vs NVSwitch:** 각 역할과 사용하는 system을 구분합니다.
2. **InfiniBand vs RoCE:** Use cases, speed tiers (HDR/NDR/800G), RDMA를 이해합니다.
3. **MIG vs vGPU vs passthrough:** 각 방식을 언제 사용하는지 구분합니다.
4. **DPU의 세 가지 기능:** offload, accelerate, isolate.
5. **AI Fabric vs N-S network:** E-W의 high bandwidth 통신과 N-S의 management/user access를 구분합니다.
6. **DCGM metrics:** SM utilization, memory bandwidth, NVLink throughput, temperature.
7. **GPU architecture families:** Blackwell, Hopper, Ada Lovelace, Grace CPU. Workload별로 적합한 제품을 구분합니다.

### 시험 당일 전략

- 선택 전 모든 option을 읽습니다. 그럴듯한 답이 두 개 있는 문항이 많습니다.
- 원본의 전략에 따르면 "NVIDIA-specific" 문항은 generic solution보다 NVIDIA 제품명을 답으로 요구하는 경우가 많습니다.
- 문항당 72초이므로 시간이 빠듯합니다. 막히면 flag를 표시하고 나중에 돌아옵니다.
- RDMA, RoCE, DPU, MIG, NCCL, DCGM, NGC, TRT, NVL의 뜻을 정확히 익힙니다.
