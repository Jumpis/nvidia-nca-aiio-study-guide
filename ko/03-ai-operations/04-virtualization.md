---
layout: korean
title: "3.4 Virtualizing Accelerated Infrastructure (한국어)"
parent: "Domain 3 — AI Operations (22%) (한국어)"
nav_order: 4
grand_parent: 한국어 학습 가이드
permalink: /ko/03-ai-operations/04-virtualization/
lang: ko
---

{% assign original_page = site.pages | where: "path", "03-ai-operations/04-virtualization.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 3.4 Virtualizing Accelerated Infrastructure

## 시험에서 평가하는 내용

MIG, vGPU, passthrough의 use cases, trade-offs, 지원 NVIDIA products를 이해합니다.

## GPU Virtualization의 목적

Physical GPU는 비쌉니다. 원본 cost 예시는 $10K–$30K입니다. Virtualization 없이 user/workload별 GPU를 배정하면 충분히 활용하지 못할 수 있고 shared workload isolation/QoS를 관리하기 어렵습니다.

- **Multi-tenancy:** 여러 users/VMs/containers가 GPU 공유.
- **Isolation:** workload 간 간섭 제한.
- **QoS:** workload별 resource allocation 보장.
- **Better utilization:** 작은 job으로 idle resource 활용.

## 세 가지 방식

```
┌────────────────────────────────────────────────────────────────┐
│  Physical GPU (e.g., H100 80GB)                                 │
│                                                                  │
│  Method 1: GPU Passthrough                                      │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │          VM gets the ENTIRE GPU — no sharing             │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Method 2: vGPU (NVIDIA Virtual GPU)                            │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐   │
│  │  VM 1      │ │  VM 2      │ │  VM 3      │ │  VM 4      │   │
│  │ vGPU 20GB  │ │ vGPU 20GB  │ │ vGPU 20GB  │ │ vGPU 20GB  │   │
│  └────────────┘ └────────────┘ └────────────┘ └────────────┘   │
│  (time-sliced, shared GPU — software-based partitioning)        │
│                                                                  │
│  Method 3: MIG (Multi-Instance GPU)                             │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐          │
│  │  MIG 1   │ │  MIG 2   │ │  MIG 3   │ │  MIG 4   │  ...     │
│  │ 20GB HBM │ │ 20GB HBM │ │ 10GB HBM │ │ 10GB HBM │          │
│  │ SM slice │ │ SM slice │ │ SM slice │ │ SM slice │          │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘          │
│  (hardware-partitioned — fully isolated, no interference)       │
└────────────────────────────────────────────────────────────────┘
```

Passthrough는 전체 GPU를 한 VM에, time-sliced vGPU는 GPU 시간을 여러 VM에, MIG는 hardware partition을 여러 workload에 배정합니다.

## Method 1: GPU Passthrough

### 정의

VMware ESXi, KVM, Hyper-V 같은 hypervisor가 PCIe passthrough로 physical GPU 전체를 단일 VM에 할당합니다. VFIO를 활용할 수 있습니다. 원본은 SR-IOV VF passthrough도 같은 표현으로 사용하지만 **전체 physical device passthrough와 SR-IOV virtual function passthrough는 구분**해야 합니다.

### 특성

- VM이 **exclusive/dedicated full GPU** access를 가집니다.
- VM 내부에서 bare-metal과 유사하게 사용합니다.
- **Sharing 없음:** GPU 하나당 VM 하나.
- Virtualization overhead를 최소화해 performance를 높입니다.
- Host 사용에서 분리해 VM에 할당합니다.

### Use cases

- Full GPU와 최대 performance가 필요한 workload.
- Dedicated VM의 high-priority Training job.
- MIG/vGPU를 지원하지 않는 legacy application.

### 한계

- 작은 workload에서는 utilization이 낮을 수 있습니다.
- 원본의 기본 비교에서는 live VM migration을 지원하지 않습니다.
- Physical GPU당 하나의 VM.

## Method 2: vGPU — NVIDIA Virtual GPU

### 정의

여러 VM이 하나의 physical GPU를 사용하는 NVIDIA virtualization technology입니다. 여기서는 GPU compute를 time-slicing하는 방식을 비교합니다. **MIG-backed vGPU 구성도 있으므로 vGPU 전체가 time-slicing만을 뜻하는 것은 아닙니다.**

### 동작

- Hypervisor에서 NVIDIA Virtual GPU Manager가 실행됩니다.
- Physical GPU를 여러 virtual GPU instances로 노출합니다.
- VM에는 vGPU guest driver를 설치합니다.
- 이 비교의 time-sliced 방식은 MIG spatial partition 대신 time multiplexing을 사용합니다.

### vGPU Profiles — 원본 A100 80GB 예시

| Profile | Memory | Use case |
|---|---|---|
| A100-80C | 80 GB | Full GPU compute, 1 VM |
| A100-40C | 40 GB | GPU를 2 VMs가 공유 |
| A100-20C | 20 GB | GPU를 4 VMs가 공유 |
| A100-10C | 10 GB | GPU를 8 VMs가 공유 |

### vGPU Editions

| Edition | Workload |
|---|---|
| **vCS (Virtual Compute Server)** | AI/ML Inference, HPC, data science |
| **vWS (Virtual Workstation)** | Professional graphics, design |
| **vPC (Virtual PC)** | VDI, Windows desktop |

### Use cases

- GPU-accelerated virtual desktops를 여러 user가 사용하는 VDI.
- 여러 user가 capacity를 공유하는 multi-tenant AI Inference.
- User별 dedicated hardware 없이 enterprise AI 제공.

### 한계

- Time-slicing에서는 여러 VM이 경쟁하면 latency spike 가능.
- Time-sliced 방식의 memory partition과 MIG hardware error isolation은 다릅니다.
- NVIDIA license가 필요합니다. 원본은 NVIDIA AI Enterprise를 제시하며 실제 edition/workload에 맞는 license를 확인해야 합니다.

## Method 3: MIG — Multi-Instance GPU

### 정의

Ampere A100에서 도입된 **hardware-level GPU partitioning**입니다. 최대 **7 hardware-isolated GPU instances**로 나누며 각 instance에 다음 resource를 배분합니다.

- Dedicated SM slice.
- Dedicated HBM partition.
- Dedicated L2 cache slice.
- Dedicated memory bandwidth.
- 원본은 dedicated PCIe bandwidth도 포함한다고 설명하지만, PCIe 등 공유 resource의 실제 보장 범위는 device specification을 확인해야 합니다.

### Hardware isolation이 중요한 이유

Time-sliced 방식은 context switch/shared state로 다른 workload의 latency에 영향을 줄 수 있습니다. MIG는 compute/memory resources를 partition하여 **instance 간 performance/memory interference를 제한**합니다.

### Supported GPUs

- **A100 (SXM4/PCIe):** 최대 7 MIG instances.
- **H100 (SXM5/PCIe):** 최대 7.
- **A30:** 최대 4.
- **H200:** 최대 7.

### A100 80GB MIG Profiles

| Instance profile | SMs (원본의 비율 표기) | GPU Memory | GPU당 최대 수 |
|---|---|---|---|
| 1g.10gb | 1/7 | 10 GB | 7 |
| 2g.20gb | 2/7 | 20 GB | 3 |
| 3g.40gb | 3/7 | 40 GB | 2 |
| 4g.40gb | 4/7 | 40 GB | 1 |
| 7g.80gb | 7/7 | 80 GB | 1, full GPU in MIG mode |

### MIG와 Kubernetes

**NVIDIA GPU Operator + MIG Manager**로 partition을 구성합니다.

```yaml
resources:
  requests:
    nvidia.com/mig-3g.40gb: "1"  # Request one 3g.40gb MIG instance
```

이 예시는 `3g.40gb` instance 하나를 request합니다. Pod에 isolated 40 GB partition을 배정합니다.

### Use cases

- Strict QoS/isolation이 필요한 multi-tenant AI Inference.
- 여러 small model을 guaranteed throughput으로 동시에 실행.
- 여러 data scientist가 cluster를 공유하는 development 환경.
- GPU 하나에서 여러 experiment를 실행하는 research cluster.

## 비교

| 항목 | Passthrough | Time-sliced vGPU | MIG |
|---|---|---|---|
| Isolation | Full GPU, hardware | Software/time share | Hardware partition |
| Tenants | GPU당 1 | 원본 예시 GPU당 8–16 | A100/H100 최대 7 |
| Performance isolation | Dedicated | Soft | **Hardware** |
| Memory isolation | Full | Partitioned, soft | **Partitioned, hardware** |
| Live VM migration | 원본 비교: 불가 | 제약 조건하에 가능 | 원본 비교: 불가 |
| Supported on | Passthrough 지원 GPU/platform | 지원 NVIDIA vGPU/platform | A100, H100, A30, H200 |
| License | 별도 vGPU license 없음 | NVIDIA license 필요 | Hardware MIG 자체는 별도 license 없음 |
| 적합한 상황 | 한 tenant, 최대 performance | VDI, soft multi-tenancy | Production multi-tenancy/QoS |

MIG-backed vGPU, hypervisor/version별 기능은 이 단순 비교와 별도로 확인해야 합니다. [NVIDIA vGPU Overview](https://docs.nvidia.com/ai-enterprise/release-8/latest/infra-software/vgpu/overview.html)에는 time-sliced, MIG-backed, time-sliced MIG-backed 구성이 설명되어 있습니다.

## 선택 가이드

- Isolation/QoS가 중요 → 지원 GPU의 **MIG** 또는 한 tenant의 **passthrough**.
- A100/H100처럼 MIG를 지원하며 hardware isolation으로 공유 → **MIG**.
- MIG 미지원 GPU에서 soft multi-tenancy → **vGPU**.
- 한 workload가 full GPU 필요 → **passthrough** 또는 **7g.80gb MIG**.
- VDI/여러 Windows desktop users → **vGPU (vPC/vWS)**.

## Self-check questions

1. A100/H100의 최대 MIG instances 수는 얼마인가요?
2. MIG와 time-sliced vGPU의 isolation 차이는 무엇인가요?
3. 별도 NVIDIA virtualization license 없이 사용하는 방식은 무엇인가요?
4. 4명에게 H100 하나에서 각각 guaranteed 20 GB MIG partition을 배정할 수 있나요?
5. 원본 비교에서 live VM migration을 지원하는 방식은 무엇인가요?

<details>
<summary>정답과 해설</summary>

1. 최대 7개. A100/H100 80GB의 `1g.10gb` profile 예시입니다.<br>
2. MIG는 SM/HBM/L2/bandwidth를 hardware partition합니다. Time-sliced vGPU는 hardware를 시간별로 공유해 context switching/다른 instance의 영향이 생길 수 있습니다.<br>
3. GPU passthrough와 hardware MIG 자체. VM에 MIG-backed vGPU를 제공하는 경우는 별도의 vGPU licensing을 확인해야 합니다.<br>
4. **원본 정답은 MIG, 4×2g.20gb이지만 해당 구성은 불가능합니다.** 2g.20gb는 GPU당 최대 3개이며 4개는 compute slices를 초과합니다. H100 80GB에서 20 GB씩 4개가 필요하면 **4×1g.20gb**를 사용할 수 있습니다. 2g와 1g는 compute allocation이 다릅니다. [NVIDIA MIG Profiles](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/supported-mig-profiles.html) 참조. Memory 합계만으로 MIG 배치를 판단하지 마세요.<br>
5. 원본의 단순 비교에서는 vGPU가 지원 hypervisor/조건에서 가능합니다. Passthrough/hardware MIG의 standalone 비교에서는 불가로 제시합니다.

</details>
