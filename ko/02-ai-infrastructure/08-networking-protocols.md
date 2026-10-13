---
layout: korean
title: "2.8 Data Center Networking Protocols (한국어)"
parent: "Domain 2 — AI Infrastructure (40%) (한국어)"
nav_order: 8
grand_parent: 한국어 학습 가이드
permalink: /ko/02-ai-infrastructure/08-networking-protocols/
lang: ko
---

{% assign original_page = site.pages | where: "path", "02-ai-infrastructure/08-networking-protocols.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 2.8 Data Center Networking Protocols and Key Concepts

## 시험에서 평가하는 내용

InfiniBand와 Ethernet, RDMA 동작, GPUDirect RDMA, AI networking에서 CPU offload의 중요성을 이해합니다.

## InfiniBand

![InfiniBand Overview]({{ '/assets/screenshots/infiniband-overview.png' | relative_url }})

**InfiniBand (IB)**는 high-performance data center interconnect를 목적으로 설계한 networking technology입니다.

- High throughput, **low latency** (sub-1 microsecond 예시), low processing overhead.
- 처음부터 native RDMA를 지원합니다.
- **InfiniBand Trade Association (IBTA)**가 관리합니다.
- 원본은 TOP500 supercomputing list의 >50%를 연결한다고 설명합니다.
- AI Factory의 E-W fabric에 사용합니다.

### InfiniBand Speed Generations

| Generation | Speed per link | 설명 |
|---|---|---|
| SDR | 10 Gbps | Legacy |
| DDR | 20 Gbps | Legacy |
| QDR | 40 Gbps | Legacy |
| FDR | 56 Gbps | Legacy |
| EDR | 100 Gbps | 아직 사용되는 deployment가 있음 |
| HDR | 200 Gbps | H100 cluster에서도 사용 |
| NDR | 400 Gbps | H100/B200용 표준으로 원본에서 소개 |
| **XDR (원본: QM9700)** | **800 Gbps** | NVIDIA Quantum-X800 |

제품 model 번호와 generation을 혼동하지 마세요. 원본은 QM9700과 XDR/Quantum-X800을 혼용하므로 QM9700은 **NDR 400 Gbps**, Quantum-X800은 **XDR 800 Gbps**로 구분합니다. [QM97XX Manual](https://networking-docs.nvidia.com/qm97x0hw/introduction) 참조.

### InfiniBand Topology

![InfiniBand Topology]({{ '/assets/screenshots/infiniband-topology.png' | relative_url }})

Fat-tree는 compute node를 leaf에, spine switch를 상단에 배치합니다. Nonblocking fat-tree에서는 aggregate uplink/downlink bandwidth가 같아 oversubscription이 없습니다. Node들이 동시에 full line rate로 통신할 수 있습니다.

## RDMA — Remote Direct Memory Access

### DMA vs RDMA

![RDMA DMA Concept]({{ '/assets/screenshots/rdma-dma-concept.png' | relative_url }})

**DMA (Direct Memory Access)**는 CPU 개입 없이 device가 local host memory에 직접 access하는 방식입니다. GPU도 memory transfer에 DMA를 사용합니다.

**RDMA**는 이를 network 너머로 확장합니다. Remote CPU/OS의 data-path 개입 없이 **remote machine**의 memory를 read/write합니다.

### RDMA 동작

![RDMA CPU Offload]({{ '/assets/screenshots/rdma-cpu-offload.png' | relative_url }})

Traditional TCP:

```
Application → OS Kernel → TCP stack → NIC → Network → NIC → TCP stack → OS Kernel → Application
(multiple copies; CPU involved every step; high latency; high CPU utilization)
```

여러 copy가 발생하고 CPU가 각 단계에 관여하여 latency와 CPU utilization이 높습니다.

RDMA:

```
Application (user space) → RDMA NIC (HCA) → Network → RDMA NIC → Application
(zero copy; CPU offloaded; sub-microsecond latency)
```

User-space application에서 RDMA NIC (HCA)로 직접 전달해 zero-copy/kernel bypass와 CPU offload를 수행합니다.

**RDMA 이점:**

- **Efficient data transfer:** OS bypass로 data movement를 최적화합니다.
- **CPU offloading:** hardware-accelerated InfiniBand HCA가 transfer를 담당해 CPU는 application을 처리합니다.
- **Low power/cooling:** networking용 CPU compute가 줄어 power를 낮춥니다.
- **Efficient computing:** power, cooling, space requirements를 줄일 수 있습니다.
- **지원:** message passing, sockets, NVMe-oF 등 storage protocols.

## GPUDirect RDMA

![GPUDirect RDMA]({{ '/assets/screenshots/gpudirect-rdma.png' | relative_url }})

CPU memory를 경유하는 GPU transfer:

```
GPU Memory → CPU Memory (copy) → NIC → Network
```

**GPUDirect RDMA**는 NIC가 GPU memory에 직접 read/write하여 CPU memory copy를 제거합니다.

```
GPU Memory → NIC → Network → NIC → GPU Memory
```

- **Copy operations 절약:** GPU→CPU→NIC copy를 제거합니다.
- **PCI transactions 감소:** PCIe traversals를 줄입니다.
- **CPU usage 감소:** CPU의 data movement 개입을 줄입니다.
- **End-to-end latency 개선:** Training gradient all-reduce에 중요합니다.

InfiniBand + GPUDirect RDMA + NVLink 조합이 large-scale distributed Training을 지원합니다.

## AI Workload용 Ethernet

### Standard Ethernet의 한계

![Ethernet for AI Workloads]({{ '/assets/screenshots/infiniband-topology.png' | relative_url }})

Ethernet은 일반적인 network 표준이지만 전통적으로 TCP, oversubscription, statistical multiplexing을 중심으로 사용되었습니다.

- **TCP overhead:** kernel 개입으로 latency와 CPU 사용량이 증가합니다.
- **Lossy Ethernet:** congestion의 packet drop과 TCP retransmit이 latency spike를 만듭니다.
- **ECMP load balancing:** hash-based ECMP에서 flow collision (hash polarization)이 생길 수 있습니다.

### RoCE — RDMA over Converged Ethernet

**RoCE**는 Ethernet infrastructure에서 RDMA semantics를 제공합니다.

- Standard Ethernet physical layer (ports/cables/transceivers)를 사용합니다.
- Ethernet 위에 InfiniBand transport semantics를 적용합니다.
- 원본은 packet drop 방지를 위한 **lossless Ethernet/PFC (Priority Flow Control)** 구성을 요구합니다.
- Cost/flexibility로 Ethernet을 선호하는 AI Cloud에 사용합니다.

### RoCE 두 종류

| 항목 | RoCEv1 | RoCEv2 |
|---|---|---|
| Layer | Layer 2 | Layer 3, IP-routable |
| Scope | 같은 subnet | Subnet 간 routing 가능 |
| 사용 | Legacy | 현대 deployment의 표준 |

## Protocol 비교

| 항목 | InfiniBand | RoCE | TCP/Ethernet |
|---|---|---|---|
| Latency 예시 | <1 µs | 1–3 µs | 10–100 µs |
| CPU overhead | HCA offload로 최소화 | 최소화 | 높음 |
| Loss handling | 설계상 lossless | PFC/lossless config 사용 | Loss 허용, TCP retransmit |
| Deployment | AI Factory, HPC | AI Cloud, mixed | Traditional IT |
| NVIDIA product | Quantum-X800 | Spectrum-X | Spectrum |

## Self-check questions

1. RDMA의 full name과 standard TCP 대비 해결하는 문제는 무엇인가요?
2. DMA와 RDMA의 차이는 무엇인가요?
3. Link당 400 Gbps를 제공하는 InfiniBand generation은 무엇인가요?
4. GPUDirect RDMA가 data path에서 제거하는 단계는 무엇인가요?
5. Packet drop을 방지하려면 RoCE에 어떤 network configuration을 적용하나요?

<details>
<summary>정답과 해설</summary>

1. Remote Direct Memory Access. TCP는 network operation마다 kernel이 관여해 CPU cycles와 latency가 증가합니다. RDMA는 remote CPU/OS 개입 없이 memory를 직접 read/write하여 overhead를 줄입니다. 원본은 sub-microsecond latency 예시를 제시합니다.<br>
2. DMA는 local machine memory에, RDMA는 network 너머 remote machine memory에 CPU data-path 개입 없이 access합니다.<br>
3. NDR (Next Data Rate), port당 400 Gbps. XDR/Quantum-X800은 800 Gbps입니다.<br>
4. GPU→CPU memory→NIC 경유 copy를 제거하여 GPU Memory→NIC→Network로 전달합니다. Copy, PCI transactions, CPU involvement를 줄입니다.<br>
5. 원본의 정답은 lossless Ethernet, 특히 PFC입니다. Priority별 pause frame으로 congestion 중 packet drop을 막습니다. Drop이 발생하면 RDMA performance가 크게 저하될 수 있습니다.

</details>
