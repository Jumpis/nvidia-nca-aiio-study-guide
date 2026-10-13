---
layout: korean
title: "2.9 High-Speed Network Options (한국어)"
parent: "Domain 2 — AI Infrastructure (40%) (한국어)"
nav_order: 9
grand_parent: 한국어 학습 가이드
permalink: /ko/02-ai-infrastructure/09-high-speed-network-options/
lang: ko
---

{% assign original_page = site.pages | where: "path", "02-ai-infrastructure/09-high-speed-network-options.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 2.9 High-Speed Data Center Network Options and Use Cases

## 시험에서 평가하는 내용

Quantum-X800 InfiniBand, Spectrum-X Ethernet, Photonics switches의 사양과 선택 기준을 이해합니다.

## NVIDIA Networking Portfolio

![NVIDIA Networking Portfolio]({{ '/assets/screenshots/nvidia-networking-portfolio.png' | relative_url }})

| Segment | Technology | NVIDIA Products |
|---|---|---|
| **Scale-up (node 내부)** | NVLink + NVSwitch | NVLink Switch, NVSwitch |
| **Scale-out E-W (node 간)** | InfiniBand | Quantum-X800, ConnectX-8 SuperNIC |
| **Scale-out E-W (Ethernet)** | Ethernet + RoCE | Spectrum-X, BlueField-3 DPU |
| **N-S / User Access / Storage Management** | Ethernet | Spectrum, BlueField-3 |

## Option 1: NVIDIA Quantum-X800 InfiniBand

![Quantum-X800 InfiniBand Platform]({{ '/assets/screenshots/quantum-x800-infiniband.png' | relative_url }})

**Quantum-X800**은 원본에서 최신 AI Factory용 InfiniBand switch platform으로 소개합니다. 원본은 QM9700 명칭도 함께 사용하지만 아래 Q3400-RA와 model 표기가 달라, 제품 model별 사양을 구분해야 합니다. **QM9700은 Quantum-2 NDR 400 Gbps**, Quantum-X800의 Q3400은 XDR 800 Gbps입니다. [QM97XX Manual](https://networking-docs.nvidia.com/qm97x0hw/introduction) 참조.

### Quantum-X800 Q3400-RA Switch

- **144 ports**, port당 **800 Gb/s**.
- **4th-generation NVIDIA SHARP (Scalable Hierarchical Aggregation and Reduction Protocol):** all-reduce 같은 collective operation을 network 내부에서 수행하여 communication 부담을 줄입니다.
- Adaptive routing, congestion control.
- Advanced power management.
- Total switch bandwidth: **144×800 Gbps = 115.2 Tbps**.

### ConnectX-8 SuperNIC (endpoint adapter)

- NIC당 end-to-end **800 Gb/s**.
- InfiniBand와 Ethernet 모두 지원.
- PCIe Gen6, 최대 48 lanes. 높은 CPU↔NIC bandwidth.
- In-network compute offloads.

### Quantum-X800 선택 상황

- 가장 큰 Training job을 실행하는 AI Factory.
- GPU당 최소 latency와 최대 bandwidth가 필요한 경우.
- Nonblocking fat-tree topology 필요.
- SHARP in-network compute로 gradient all-reduce 시간 단축.

## Option 2: NVIDIA Spectrum-X Ethernet

![Spectrum-X Ethernet]({{ '/assets/screenshots/spectrum-x-ethernet.png' | relative_url }})

**Spectrum-X**는 NVIDIA가 AI용으로 설계한 최초의 Ethernet platform으로 소개합니다. Standard Ethernet connectivity와 RDMA급 performance를 결합합니다.

### Components

| Component | 설명 |
|---|---|
| **Spectrum-X Ethernet Switch** | AI-optimized routing/congestion control을 갖춘 switch |
| **Spectrum-X Ethernet SuperNICs** | RDMA/AI offloads를 지원하는 ConnectX-7/8 SmartNICs |

### Software Stack

- Cumulus, NetQ, NVIDIA Air, RCP, CloudAI.
- SAI/SPSDK, DOCA.

### 주요 기능

- **높은 effective bandwidth:** 많은 concurrent flow에서도 near-line-rate throughput을 유지해 standard Ethernet의 AI workload bandwidth 저하를 줄입니다.
- **매우 낮은 latency:** intra-cluster AI traffic에서 InfiniBand에 경쟁할 수준을 목표로 합니다.
- **NCCL-optimized RoCE extensions:** Adaptive Routing, Congestion Control.
- In-network processing용 **BlueField-3 DPU와 긴밀한 통합**.
- CUDA, NCCL, AI Enterprise, DOCA 등 **NVIDIA software stack에서 검증**.

### Spectrum-X 선택 상황

- Multi-tenant/diverse workload의 AI Cloud.
- Operational simplicity를 위해 Ethernet으로 표준화한 환경.
- InfiniBand switch cost/management overhead가 부담되는 경우.
- Enterprise AI networking을 요구하는 RoCE workload.

## AI용 Ethernet Enhancements

### RoCE Adaptive Routing

![RoCE Adaptive Routing]({{ '/assets/screenshots/roce-adaptive-routing.png' | relative_url }})

**문제:** ECMP (Equal-Cost Multi-Path)는 hash로 flow path를 선택합니다. Heavy flow가 같은 path에 충돌하면 그 path는 congested되고 다른 path는 idle 상태가 됩니다.

**동작:**

- Packet/flowlet 수준의 fine-grained load balancing.
- Congestion 상태에 따라 RDMA data를 다른 path로 dynamic routing.
- **Spectrum-4 switch**가 port utilization을 monitoring하고 real-time routing 결정.
- 전체 path에 걸친 **optimal load balancing**.
- **Network utilization 향상/congestion 감소**.

### RoCE Congestion Control

![RoCE Congestion Control]({{ '/assets/screenshots/roce-congestion-control.png' | relative_url }})

**문제:** Lossless Ethernet의 PFC pause가 upstream으로 퍼지면 head-of-line blocking이 fabric 전체에 확산될 수 있습니다.

**동작:**

- Congestion을 만드는 flow의 source injection rate를 제한합니다.
- Switch buffer occupancy를 줄여 latency를 낮춥니다.
- Burst tolerance를 개선합니다.
- ECN (Explicit Congestion Notification)으로 congested packet을 mark하고 receiver가 sender에 감속을 알립니다 (DCQCN).

### 함께 필요한 Hardware

![RoCE Adaptive Routing + Congestion Control]({{ '/assets/screenshots/roce-adaptive-congestion-combined.png' | relative_url }})

원본에서 소개하는 NVIDIA RoCE enhancement 구성은 두 요소가 함께 동작해야 합니다.

- **Server별 BlueField-3 DPU:** in-order delivery/congestion signals 처리.
- **Spectrum-4 switch:** adaptive routing/congestion notifications.

> NVIDIA의 RoCE adaptive routing과 congestion control은 Spectrum-4 switch와 BlueField-3 DPU의 협력으로 동작합니다.

## Option 3: NVIDIA Photonics Switches

![Photonics Switches]({{ '/assets/screenshots/photonics-switches.png' | relative_url }})

가장 큰 scale-out AI cluster를 위한 co-packaged optics (CPO) switch platform입니다.

| Product | Bandwidth | Ports | Cooling |
|---|---|---|---|
| **Quantum-X Photonics** (Quantum 3450-LD) | **115 Tb/s** | 144×800G (576×200G) | Liquid cooled |
| **Spectrum-X Photonics** (Spectrum SN6810) | **102.4 Tb/s** | 128×800G (512×200G) | Liquid cooled |
| **Spectrum-X Photonics** (Spectrum SN6800) | **409.6 Tb/s** | 512×800G (2048×200G) | Liquid cooled |

### Co-packaged optics란?

Traditional switch는 외부 pluggable transceiver (QSFP/OSFP)를 사용합니다. CPO는 optical components를 switch ASIC package에 직접 통합합니다. ASIC과 transceiver 사이 electrical signal path를 줄여 power consumption을 낮추고 더 많은 port를 지원합니다.

### Use case

원본은 **수백만 GPU**로 확장하는 hyperscale AI Factory를 대상으로 소개합니다. 매우 높은 density와 bandwidth당 낮은 power가 필요합니다.

## Network 선택 기준

| Scenario | Recommended | 이유 |
|---|---|---|
| 가장 큰 LLM Training, 최대 throughput | **Quantum-X800 IB** | Low latency, SHARP, nonblocking |
| Multi-tenant AI Cloud, Ethernet 선호 | **Spectrum-X** | Ethernet 운영, RoCE adaptive routing/CC |
| 수백만 GPU의 hyperscale AI Factory | **Photonics switches** | Ultra-high density, CPO power efficiency |
| IB/Ethernet 모두 필요 | ConnectX-8 SuperNIC | 하나의 adapter에서 두 protocol 지원 |

## Self-check questions

1. Quantum-X800 Q3400-RA의 port 수와 speed는 얼마인가요?
2. SHARP의 full name과 network 내부 역할은 무엇인가요?
3. 소개된 RoCE adaptive routing/congestion control에 필요한 두 NVIDIA hardware는 무엇인가요?
4. CPO는 무엇이며 많은 port를 지원하는 이유는 무엇인가요?
5. 언제 Quantum-X800보다 Spectrum-X를 선택하나요?

<details>
<summary>정답과 해설</summary>

1. Port당 800 Gb/s인 144 ports.<br>
2. Scalable Hierarchical Aggregation and Reduction Protocol. Gradient synchronization의 all-reduce 등을 switch 내부에서 수행해 GPU 간 교환하는 data 양을 줄입니다.<br>
3. Server별 BlueField-3 DPU + Spectrum-4 switch. 함께 구성해야 합니다.<br>
4. Optical components를 ASIC package에 통합해 외부 transceiver와의 electrical path를 줄입니다. Power와 electrical-interface bottleneck을 낮추어 aggregate bandwidth를 높입니다.<br>
5. Multi-tenant 환경에서 Ethernet으로 표준화할 때, 단일 network 종류의 운영 편의가 중요할 때, AI Cloud일 때, InfiniBand 관리 전문 인력이 없을 때입니다. 익숙한 Ethernet tooling으로 RoCE performance를 제공합니다.

</details>
