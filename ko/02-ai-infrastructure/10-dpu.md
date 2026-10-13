---
layout: korean
title: "2.10 DPU Purpose and Benefits (한국어)"
parent: "Domain 2 — AI Infrastructure (40%) (한국어)"
nav_order: 10
grand_parent: 한국어 학습 가이드
permalink: /ko/02-ai-infrastructure/10-dpu/
lang: ko
---

{% assign original_page = site.pages | where: "path", "02-ai-infrastructure/10-dpu.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 2.10 DPU — Data Processing Unit Purpose and Benefits

## 시험에서 평가하는 내용

DPU의 정의, 세 기능 (offload, accelerate, isolate), BlueField-3 사양, DOCA SDK, NVIDIA-Certified Systems를 이해합니다.

## NVIDIA DPU와 Data Center Transformation

![DPU Data Center Transformation]({{ '/assets/screenshots/dpu-datacenter-transformation.png' | relative_url }})

DPU는 cloud/data center/edge에서 **secure and accelerated infrastructure**를 제공하는 세 기능을 수행합니다.

### 1. Offload

Server CPU의 infrastructure task를 맡아 CPU resource를 application에 사용할 수 있게 합니다.

Networking/storage/security software는 원래 CPU에서 실행되어 ML Training, Inference, database query에 쓸 core를 소비합니다. DPU의 dedicated Arm cores와 accelerators로 이를 옮깁니다.

### 2. Accelerate

DPU hardware acceleration으로 infrastructure functions를 CPU보다 빠르게 실행합니다.

- Packet processing: 100–400 Gbps wire speed.
- Cryptography: TLS/IPsec offload.
- Storage: NVMe-oF, compression, deduplication.
- Regular expressions: security pattern matching.

원본 예시에서는 CPU가 encrypted traffic ~10–40 Gbps를 처리하고, BlueField-3는 hardware crypto acceleration으로 **400 Gbps**를 처리합니다.

### 3. Isolate

핵심 data plane/control plane functions를 별도의 DPU domain으로 옮겨 CPU 부담을 줄이고, host CPU/software가 compromise되어도 보호합니다.

Server OS가 ransomware/privilege escalation으로 침해되어도 DPU는 독립된 OS에서 network policies, encryption, access control을 계속 enforce할 수 있습니다.

## DPU Architecture: Software-Defined Data Center Infrastructure on a Chip

![DPU Architecture]({{ '/assets/screenshots/dpu-architecture.png' | relative_url }})

### DPU가 없는 Traditional server

```
CPU handles:
  ├── Application workload
  ├── VMs / Containers
  ├── Infrastructure Management
  ├── Software-Defined Security
  ├── Software-Defined Storage
  ├── Software-Defined Networking
  └── NIC (basic forwarding only)
```

CPU가 application, VMs/containers뿐 아니라 infrastructure management, software-defined security/storage/networking까지 담당합니다. NIC는 기본 forwarding을 수행합니다.

### NVIDIA DPU가 있는 Server

```
CPU handles:              DPU handles:
  ├── Application           ├── Infrastructure Management
  ├── VMs/Containers        ├── Software-Defined Security
                            ├── Software-Defined Storage
                            ├── Software-Defined Networking
                            └── Acceleration Engine
```

CPU는 application과 VMs/containers에 집중하고, DPU는 infrastructure management와 software-defined security/storage/networking, acceleration을 담당합니다.

**결과:** CPU의 infrastructure overhead가 줄어 실제 workload에 더 많은 core를 사용할 수 있습니다. GPU server의 CPU는 network/storage stack 대신 preprocessing, checkpointing, orchestration에 집중합니다.

## NVIDIA BlueField-3 DPU

![BlueField-3 Overview]({{ '/assets/screenshots/bluefield-3-overview.png' | relative_url }})

원본은 BlueField-3를 "400Gbps infrastructure compute platform"인 현 세대 DPU로 소개합니다.

| Feature | Specification |
|---|---|
| **Network** | **400GbE** |
| **Programmable Engines** | Infrastructure services용 16× Arm A78, 64-bit cores |
| **Accelerated Pipelines** | Crypto, regex, compression, storage |
| **Zero-Trust Security** | Platform security, secure boot |
| **Composable Storage** | NVMe-oF, PCIe/TCP 기반 storage disaggregation |

### BlueField-3 Use cases

![BlueField Use Cases]({{ '/assets/screenshots/bluefield-use-cases.png' | relative_url }})

| Domain | Use Case |
|---|---|
| **Cloud Computing** | Bare-metal provisioning, VM/container isolation, private/public/hybrid cloud |
| **Cybersecurity** | Host와 독립적인 NIC의 distributed security, NGFW, micro-segmentation |
| **HPC & AI** | Scientific computing, accelerated DLRM |
| **Telco & Edge** | O-RAN, 5G edge compute |
| **Data Storage** | HCI, Elastic Block Storage, Instance Storage, NVMe-oF offload |
| **Media Streaming** | High-quality visual, 8K video, CDN |

## NVIDIA DOCA — BlueField SDK

![DOCA SDK]({{ '/assets/screenshots/doca-sdk.png' | relative_url }})

**DOCA (Data Center Infrastructure on a Chip Architecture)**는 BlueField DPU programming용 종합 software framework입니다.

### 제공 기능

- 모든 BlueField generation을 위한 **unified software framework**.
- Orchestration, Security, Networking, Storage, Management, Telemetry libraries.
- Hardware accelerators를 사용하는 **Acceleration Libraries**.
- 원본은 BlueField-2용 code가 BlueField-3에서도 동작하는 generation 간 software compatibility를 강조합니다.

### Partner ecosystem

| Category | Partners |
|---|---|
| Platform Infrastructure | Canonical, Nutanix, Red Hat, VMware |
| Storage | DDN, NetApp, VAST, WEKA |
| Cyber Security | Check Point, Cisco, Fortinet, Palo Alto, Aria (Arista) |
| Edge | CloudFlare, Juniper, StackPath |

## NVIDIA-Certified Systems

![NVIDIA-Certified Systems]({{ '/assets/screenshots/nvidia-certified-systems.png' | relative_url }})
![Certified Servers Features]({{ '/assets/screenshots/certified-servers-features.png' | relative_url }})

NVIDIA GPU/DPU를 대규모로 배포할 때 baseline configuration을 검증한 system입니다.

### System design options

- NVIDIA Server GPUs.
- NVIDIA SmartNICs/DPUs (BlueField).
- Dell, HPE, Lenovo, Supermicro 등 partner servers.
- NVIDIA Workstation GPUs.
- Partner laptops/desktops.

### Certification의 검증 대상

| Pillar | Features |
|---|---|
| **Performance** | AI/data science acceleration, high-speed network/GPU interconnect, video encode/decode, rendering throughput |
| **Security** | Network encryption offload, key management, secure boot |
| **Scale** | Accelerated data transfer, Multi-Instance GPU (MIG) 지원 |

### 중요한 이유

대규모 accelerated computing deployment를 단순화합니다. 구매 후 incompatibility를 발견하기보다 사전 검증된 configuration을 사용할 수 있습니다.

## DPU vs CPU vs GPU

```
┌─────────────────────────────────────────────────────────────┐
│  GPU — AI/ML compute, rendering, HPC                        │
│  CPU — Application logic, orchestration, preprocessing      │
│  DPU — Network, storage, security (offloaded from CPU)      │
└─────────────────────────────────────────────────────────────┘

Together: The three-chip architecture that powers modern AI servers
```

- **GPU:** AI/ML compute, rendering, HPC.
- **CPU:** application logic, orchestration, preprocessing.
- **DPU:** CPU에서 offload한 network, storage, security.

DPU는 CPU/GPU와 함께 computing의 **third pillar**로 불립니다. 각각 다른 workload에 특화된 three-chip architecture입니다.

## Self-check questions

1. DPU의 세 가지 핵심 기능은 무엇인가요?
2. Isolation은 security를 어떻게 개선하나요?
3. DOCA는 무엇이며 DPU developer의 어떤 문제를 해결하나요?
4. 400GbE를 제공하는 BlueField generation은 무엇인가요?
5. NVIDIA-Certified System은 무엇이며 왜 중요한가요?

<details>
<summary>정답과 해설</summary>

1. Offload: CPU의 infrastructure task 이전. Accelerate: hardware로 빠르게 처리. Isolate: data/control plane을 별도 DPU domain으로 분리해 보호.<br>
2. DPU는 host CPU/OS와 분리된 Arm cores에서 별도 OS를 실행합니다. Host가 compromise되어도 DPU의 network policies, encryption, firewall enforcement가 독립적으로 동작하도록 합니다.<br>
3. BlueField programming용 SDK입니다. Generation 간 unified API를 제공하여 infrastructure software를 새 hardware마다 다시 작성하는 부담을 줄입니다.<br>
4. BlueField-3, 400GbE.<br>
5. NVIDIA가 GPU/DPU baseline configuration의 performance, security, scalability를 검증한 partner server입니다. 검증되지 않은 조합의 integration 문제를 줄여 enterprise deployment를 돕습니다.

</details>
