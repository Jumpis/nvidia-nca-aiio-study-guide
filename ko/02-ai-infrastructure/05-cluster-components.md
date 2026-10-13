---
layout: korean
title: "2.5 Cluster Components (한국어)"
parent: "Domain 2 — AI Infrastructure (40%) (한국어)"
nav_order: 5
grand_parent: 한국어 학습 가이드
permalink: /ko/02-ai-infrastructure/05-cluster-components/
lang: ko
---

{% assign original_page = site.pages | where: "path", "02-ai-infrastructure/05-cluster-components.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 2.5 Cluster Components of Accelerated Infrastructure

## 시험에서 평가하는 내용

AI cluster의 compute nodes (DGX), storage hierarchy, networking 등 주요 hardware components와 연결 구조를 이해합니다.

## AI Cluster Architecture 개요

```
┌─────────────────────────────────────────────────────────────────┐
│                     AI CLUSTER                                   │
│                                                                  │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐        │
│  │ DGX Node │  │ DGX Node │  │ DGX Node │  │ DGX Node │  ...   │
│  │ 8× GPU   │  │ 8× GPU   │  │ 8× GPU   │  │ 8× GPU   │        │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘        │
│       │              │              │              │              │
│  ─────┴──────────────┴──────────────┴──────────────┴──────       │
│               InfiniBand / Ethernet AI Fabric (E-W)               │
│  ──────────────────────────────────────────────────────────       │
│                                                                   │
│  ┌──────────────────────────────────────────────────────────┐    │
│  │             Parallel Storage (WEKA / Lustre / GPFS)      │    │
│  └──────────────────────────────────────────────────────────┘    │
│                                                                   │
│  ─────────────────────────────────────────────────────────────   │
│               Management / Control Network (N-S)                  │
│  ─────────────────────────────────────────────────────────────   │
│                                                                   │
│  ┌──────────────┐  ┌──────────────┐  ┌─────────────────────┐    │
│  │ BMC/OOB Mgmt │  │ Job Scheduler│  │ Object / Cold Store  │    │
│  │   Network    │  │ (Slurm/K8s)  │  │ (S3-compatible)      │    │
│  └──────────────┘  └──────────────┘  └─────────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
```

DGX nodes는 E-W AI Fabric으로 통신하고 parallel storage에서 data를 읽습니다. N-S management/control network에는 BMC/OOB management, job scheduler (Slurm/K8s), object/cold storage가 연결됩니다.

## Compute nodes: DGX systems

DGX는 AI Training을 위해 설계한 NVIDIA system family입니다.

![DGX Systems Lineup]({{ '/assets/screenshots/dgx-systems-lineup.png' | relative_url }})

| System | GPUs | GPU Memory | 주요 용도 |
|---|---|---|---|
| DGX H100 | 8× H100 SXM5 80GB | 640 GB | LLM Training, data analytics |
| DGX B200 | 8× B200 SXM | 1.4 TB+ | 차세대 Generative AI |
| DGX B300 | 8× B300 SXM | ~1.6 TB | Blackwell Ultra, 높은 AI throughput |
| GB300 NVL72 | 72× Blackwell Ultra + 36× Grace | ~13 TB | Rack-scale 대규모 AI reasoning |

위 table은 원본의 값을 보존합니다. 아래 GB300 상세 사양의 20 TB와 서로 다른 값이므로 제품/configuration을 확인하며 학습하세요.

### DGX H100 내부 구성

![DGX H100 System]({{ '/assets/screenshots/dgx-h100-system.png' | relative_url }})
![DGX H100 Physical Specs]({{ '/assets/screenshots/dgx-h100-physical-specs.png' | relative_url }})

- 8× H100 SXM5 Tensor Core GPUs.
- 4× NVSwitch: GPU 간 all-to-all 900 GB/s.
- 2 TB system memory.
- Checkpoints/local datasets용 30 TB NVMe storage.
- 8× ConnectX-7 400 Gbps InfiniBand NICs.
- 2× Intel Xeon Platinum 8480C CPUs.
- Physical: 원본은 10U이지만 공식 DGX H100/H200는 **8U**입니다. [NVIDIA User Guide](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html) 참조.

### GB300 NVL72: Rack-scale AI

![GB300 NVL72]({{ '/assets/screenshots/gb300-nvl72.png' | relative_url }})

Rack 전체를 하나의 computing unit으로 사용하는 NVIDIA AI system입니다.

- **72 NVIDIA Blackwell Ultra GPUs (B300)**.
- **36 NVIDIA Grace CPUs**.
- Rack 전체에 **5th-generation NVLink**.
- Total NVLink bandwidth: **130 TB/s**.
- Total GPU memory: **20 TB**, bandwidth **576 TB/s**.
- CPU memory: **14 TB LPDDR5X**.

## Storage components

### Storage 선택 기준

![Storage Solution Considerations]({{ '/assets/screenshots/storage-solution-considerations.png' | relative_url }})

- Data를 어떻게 write/read하나요?
- 얼마나 자주 access하나요?
- Storage가 충분히 빠르게 data를 공급하나요?
- 누가 access해야 하나요?
- System failure 시 어떻게 동작해야 하나요?
- 언제 data를 retire하나요?

### Storage hierarchy

![Storage Hierarchy]({{ '/assets/screenshots/storage-hierarchy.png' | relative_url }})

| Tier | Type | 특성 |
|---|---|---|
| **Local NVMe** | Node별 SSD | 가장 높은 IOPS/bandwidth. Node 전용. Checkpoints/tmp에 사용 |
| **Network File System (NFS)** | NAS/network shared storage | 단순, 중간 bandwidth. 작은 dataset, code, config에 적합 |
| **Parallel/Distributed FS** | Lustre, GPFS, WEKA | 수천 client의 동시 access로 확장. Training data용 high throughput |
| **Object Storage** | S3-compatible | 낮은 $/TB, 큰 확장성, 낮은 IOPS. Dataset archive/model artifacts에 적합 |
| **Other** | HCI, tape archive | 특수 목적 |

### Network File Systems

![Network File Systems]({{ '/assets/screenshots/network-file-systems.png' | relative_url }})

**NFS (Network File System):**

- 가장 흔한 shared storage protocol.
- Sun Microsystems가 1984년에 개발했습니다.
- File은 blocks로 저장하며 각 client에서 local filesystem처럼 보입니다.
- 안정적인 performance와 단순한 interface를 제공합니다.
- Snapshots, data replication, performance profiling 등 enterprise features를 지원합니다.
- 주요 vendors: **Dell EMC, NetApp, Pure Storage**.

**Training workload:** 수백 GPU가 동시에 batch를 읽으면 NFS가 bottleneck이 될 수 있어 Lustre/GPFS/WEKA 같은 parallel file system을 선호합니다.

## Networking components

AI cluster는 두 종류의 network를 구분합니다.

| Network | Direction | 목적 | Technology |
|---|---|---|---|
| **AI Fabric (E-W)** | East-West | Training의 GPU 간 gradient exchange | InfiniBand NDR/HDR 또는 RoCE |
| **Management/User Network (N-S)** | North-South | User access, control plane, internet | Ethernet (10/25/100G) |

자세한 내용은 [2.7 Networking Requirements]({{ '/ko/02-ai-infrastructure/07-networking-requirements/' | relative_url }})와 [2.9 High-Speed Options]({{ '/ko/02-ai-infrastructure/09-high-speed-network-options/' | relative_url }})를 확인하세요.

## CPU nodes (host servers)

각 compute node의 CPU host는 다음을 담당합니다.

- OS/system software 실행.
- I/O, PCIe bus, NVMe storage 관리.
- GPU에서 처리하지 않는 pre/post-processing.
- Slurm agent, Kubernetes kubelet 같은 job scheduler daemon 실행.

NVIDIA system에서 사용하는 선택지:

- **Intel Xeon:** DGX H100은 dual Xeon Platinum 8480C.
- **AMD EPYC:** Dell/HPE 등의 NVIDIA-Certified Servers.
- **NVIDIA Grace:** Arm 기반 Superchip 구성.

## Self-check questions

1. DGX H100의 NVSwitch 수와 역할은 무엇인가요?
2. 500-node cluster에서 동시에 Training data를 읽으려면 어떤 storage가 필요한가요?
3. AI cluster의 E-W와 N-S traffic은 어떻게 다른가요?
4. GB300 NVL72에는 GPU가 몇 개 있나요?
5. NFS slide에 등장하는 storage vendor 세 곳은 어디인가요?

<details>
<summary>정답과 해설</summary>

1. 4× NVSwitch. Fully non-blocking all-to-all NVLink fabric으로 GPU가 서로 full 900 GB/s bandwidth에서 동시에 통신하도록 합니다.<br>
2. Parallel/distributed file system (Lustre, GPFS, WEKA). 높은 aggregate throughput으로 수천 client를 지원합니다. NFS는 bottleneck이 될 수 있습니다.<br>
3. E-W는 cluster 내부 server 간 GPU gradient communication으로 high bandwidth InfiniBand/RoCE가 필요합니다. N-S는 user/internet과 cluster 사이 management, job submission, result retrieval로 standard Ethernet을 사용합니다.<br>
4. 72 Blackwell Ultra GPUs와 36 Grace CPUs.<br>
5. Dell EMC, NetApp, Pure Storage.

</details>
