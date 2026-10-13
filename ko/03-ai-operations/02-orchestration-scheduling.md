---
layout: korean
title: "3.2 Cluster Orchestration and Job Scheduling (한국어)"
parent: "Domain 3 — AI Operations (22%) (한국어)"
nav_order: 2
grand_parent: 한국어 학습 가이드
permalink: /ko/03-ai-operations/02-orchestration-scheduling/
lang: ko
---

{% assign original_page = site.pages | where: "path", "03-ai-operations/02-orchestration-scheduling.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 3.2 Cluster Orchestration and Job Scheduling

## 시험에서 평가하는 내용

AI workload의 Kubernetes/Slurm, NVIDIA GPU Operator, Run:ai, production cluster의 GPU resource request/allocation을 이해합니다.

## AI Scheduling이 다른 이유

Traditional scheduler는 CPU cores를 배분하지만 AI scheduler는 다음을 고려합니다.

- **전체 GPU 또는 MIG partition**을 job에 할당.
- **GPU-aware gang scheduling:** 필요한 GPU 전체를 동시에 할당한 뒤 Training 시작. Partial allocation은 낭비/deadlock을 유발할 수 있습니다.
- **Heterogeneous resources:** GPU type/memory/NVLink topology/InfiniBand locality.
- **Preemption:** lower-priority job을 중단해 high-priority job에 GPU 제공.
- 팀/project별 **GPU utilization/quota** 추적.

## Slurm — HPC Job Scheduler

**Slurm (Simple Linux Utility for Resource Management)**은 HPC에서 널리 사용하며 AI Training cluster에도 사용하는 scheduler입니다.

### Architecture

```
┌─────────────────┐     ┌─────────────────────────────────┐
│  Submit host     │────▶│           Slurm Controller       │
│  (sbatch, srun)  │     │  (slurmctld — tracks jobs,       │
└─────────────────┘     │   nodes, resource state)          │
                         └──────────────┬──────────────────┘
                                        │
                    ┌───────────────────┼───────────────────┐
                    │                   │                   │
              ┌─────┴────┐       ┌──────┴───┐       ┌──────┴────┐
              │  Node 1   │       │  Node 2   │       │  Node N   │
              │ (slurmd)  │       │ (slurmd)  │       │ (slurmd)  │
              └─────┬────┘       └──────┬───┘       └──────┬────┘
                    │                   │                   │
                GPU, CPU, RAM      GPU, CPU, RAM       GPU, CPU, RAM
```

Submit host의 sbatch/srun → Slurm Controller (slurmctld) → 각 node의 slurmd 순서로 job과 CPU/GPU/RAM을 관리합니다.

### 주요 개념

| Concept | 설명 |
|---|---|
| **Partition** | Priority/time limit/GPU type 등의 policy를 적용한 node queue |
| **Job** | Resource requirements와 함께 제출한 작업 단위 |
| **sbatch** | Non-interactive batch script 제출 |
| **srun** | Parallel application을 interactive 또는 job step으로 실행 |
| **GRES (Generic Resource)** | GPU resource 추적. `--gres=gpu:8`은 8개 GPU request |
| **Gang scheduling** | Job에 필요한 nodes를 동시에 할당 |

### AI에서 Slurm을 사용하는 이유

- HPC에서 성숙하고 검증된 운영 경험.
- 뛰어난 multi-node job support.
- InfiniBand topology-aware scheduling으로 같은 switch의 nodes를 할당해 latency 감소.
- Traditional simulation과 AI를 함께 실행하는 HPC center에서 널리 사용.

## Kubernetes — Container Orchestration

**Kubernetes (K8s)**는 containerized workload platform이며 AI Inference와 Training에서도 사용합니다.

### Kubernetes의 GPU Support

GPU workload scheduling에는 다음 세 layer가 필요합니다.

**1. NVIDIA GPU Operator**

GPU workload 실행에 필요한 component deployment를 자동화합니다.

- NVIDIA drivers (kernel module).
- CUDA runtime.
- DCGM monitoring.
- Node Feature Discovery: GPU capability로 node labeling.
- Device Plugin: GPU를 schedulable resource로 노출.
- MIG Manager: MIG partition 구성.
- Validator: 전체 stack 동작 확인.

Operator가 전체를 하나의 단위로 관리합니다. `helm install`로 node별 GPU stack 설치/관리를 시작할 수 있습니다.

**2. NVIDIA Device Plugin**

GPU를 Kubernetes resource로 노출합니다. 아래 request/limit은 8개 GPU를 의미합니다.

```yaml
resources:
  requests:
    nvidia.com/gpu: "8"  # Request 8 GPUs
  limits:
    nvidia.com/gpu: "8"
```

**3. Container runtime**

NVIDIA Container Toolkit (원본 표기 nvidia-docker2)이 container에서 GPU hardware에 access하도록 합니다. GPU node마다 필요합니다.

### Workload별 Kubernetes Mechanism

| Workload | Kubernetes mechanism |
|---|---|
| **Batch Training** | Job 또는 MPIJob (Kubeflow MPI Operator) |
| **Distributed Training** | PyTorchJob, TFJob (Kubeflow Training Operator) |
| **Inference serving** | Deployment + HPA, GPU utilization 기반 autoscaling |
| **Jupyter notebooks** | GPU request가 있는 StatefulSet |

## Slurm vs Kubernetes

| 항목 | Slurm | Kubernetes |
|---|---|---|
| 주요 용도 | HPC batch, 대규모 Training | Container apps, Inference, mixed workloads |
| Scheduling model | Priority가 있는 job queue | Declarative pod scheduling |
| GPU support | GRES | GPU Operator + Device Plugin |
| Multi-node MPI | Native 지원이 뛰어남 | MPI Operator add-on |
| Inference serving | 주 목적이 아님 | 적합함 |
| Containers | enroot/pyxis 사용 가능 | Native |
| Real-time scaling | 수동 | HPA/KEDA 자동 |
| 일반 환경 | Research HPC, academic, enterprise AI | Cloud-native enterprise, Inference platform |

**대규모 cluster에서 함께 사용:** 연구자의 large Training job은 Slurm, Inference services/MLOps pipeline은 Kubernetes로 운영하는 pattern이 있습니다.

## Run:ai — Kubernetes-Based GPU Scheduling

Commercial Kubernetes-native GPU scheduling platform으로 다음 AI 기능을 추가합니다.

- **GPU fractions:** software-level time-slicing으로 한 GPU를 여러 job에서 공유. MIG hardware partition과 구분합니다.
- **Dynamic GPU allocation:** 필요할 때 할당하고 idle이면 반환.
- **Guaranteed quotas + fair-share:** 팀의 보장 quota와 다른 팀의 idle quota 공유.
- **Preemption with checkpointing:** 낮은 priority job을 중단하고, checkpoint가 있으면 마지막 checkpoint에서 resume.
- **GPU utilization analytics:** project/user별 reports.

GPU workload에 별도 scheduler를 적용해 default Kubernetes scheduler의 기능을 확장합니다.

## Docker — Containerization

- **Image packaging:** application/libraries/CUDA/framework를 portable image로 묶습니다.
- **Runtime isolation:** host OS/다른 container와 분리하여 workload를 실행합니다.
- **NGC integration:** NGC image는 Docker image입니다. 예: `docker pull nvcr.io/nvidia/pytorch:24.01-py3`.

NVIDIA Container Toolkit (`nvidia-ctk`)이 container runtime을 통해 GPU access를 지원합니다.

## Self-check questions

1. NVIDIA GPU Operator는 무엇을 설치하나요?
2. Slurm은 GPU를 어떻게 request하나요?
3. Gang scheduling이란 무엇이며 AI Training에 중요한 이유는 무엇인가요?
4. Run:ai는 기본 Kubernetes에 어떤 기능을 추가하나요?
5. Large multi-node Training과 Inference serving에는 보통 어떤 scheduler를 사용하나요?

<details>
<summary>정답과 해설</summary>

1. NVIDIA drivers, CUDA runtime, DCGM, Device Plugin, Node Feature Discovery, MIG Manager, Container Toolkit 등 GPU software stack을 자동 배포합니다.<br>
2. GRES flag를 사용합니다. `--gres=gpu:8`을 SBATCH script/srun에 넣으면 8개 GPU를 request합니다.<br>
3. Distributed job의 nodes/GPUs 전체를 시작 전에 동시에 할당합니다. 8개 중 7개만 받은 채 마지막 node를 기다리면 GPU가 idle 상태가 되고 deadlock 가능성이 생깁니다.<br>
4. GPU fractions, dynamic quota allocation, guaranteed/fair-share scheduling, checkpoint-aware preemption/resumption.<br>
5. Slurm은 MPI/topology-aware placement가 필요한 large multi-node Training에, Kubernetes는 declarative deployment/autoscaling/service mesh/rolling update가 필요한 Inference serving에 적합합니다.

</details>
