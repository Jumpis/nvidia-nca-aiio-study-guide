---
layout: korean
title: "2.7 Networking Requirements for AI Workloads (한국어)"
parent: "Domain 2 — AI Infrastructure (40%) (한국어)"
nav_order: 7
grand_parent: 한국어 학습 가이드
permalink: /ko/02-ai-infrastructure/07-networking-requirements/
lang: ko
---

{% assign original_page = site.pages | where: "path", "02-ai-infrastructure/07-networking-requirements.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 2.7 Networking Requirements for AI Workloads

## 시험에서 평가하는 내용

AI workload의 networking 요구가 traditional application과 다른 이유, AI Fabric과 control/user-access network의 차이, AI Factory와 AI Cloud의 차이를 이해합니다.

## AI Workload의 Networking Requirements

![Networking Requirements for AI Workloads]({{ '/assets/screenshots/networking-requirements-ai.png' | relative_url }})

| 항목 | Traditional App | AI Workload |
|---|---|---|
| **Network topology** | Tree, star | Fat-tree, non-blocking |
| **Bandwidth** | Server당 1–25 Gbps | GPU당 400–800 Gbps |
| **Latency** | Milliseconds 허용 | Microseconds 필요 |
| **Protocols** | TCP/IP, retransmits | RDMA, zero-copy/CPU offload |
| **Management** | SNMP, standard monitoring | AI-aware telemetry |

## Network의 핵심: GPU를 계속 동작시키기

![GPU Busy — Network Critical]({{ '/assets/screenshots/gpu-busy-network-critical.png' | relative_url }})

**GPU는 data가 있어야 compute할 수 있습니다.** Distributed Training에서:

- Forward pass마다 storage에서 training batch가 필요합니다.
- Backward pass마다 network를 통해 다른 GPU들과 gradients를 교환합니다.
- Network가 data를 기다리며 멈추면 GPU가 idle 상태가 됩니다. 원본 cost 예시는 GPU당 $2–4/hour입니다.

Network는 GPU의 data supply pipeline을 계속 채워야 합니다.

- **High bandwidth:** GPU당 수백 Gbps로 training data와 gradients를 전달합니다.
- **Low latency:** 다음 Training step 전에 gradient all-reduce가 완료되어야 합니다.
- **Low jitter:** latency가 달라지면 먼저 끝난 GPU가 대기하며 "GPU bubble"이 발생합니다.

## AI Data Center의 두 Network

### AI Fabric (East-West)

![AI Fabric vs Control Network]({{ '/assets/screenshots/ai-fabric-vs-control-network.png' | relative_url }})

원본 heading은 AI Fabric을 scale-up network라고 부르지만, 여기의 E-W fabric은 **node 사이 scale-out communication**도 포함합니다. Node 내부 NVLink scale-up과 구분해서 읽으세요.

| Attribute | AI Fabric (E-W) | Control/User Network (N-S) |
|---|---|---|
| Traffic pattern | Tightly-coupled GPU-to-GPU | Loosely-coupled app traffic |
| Protocol | RDMA, 높은 bandwidth/낮은 CPU overhead | TCP, standard/더 큰 CPU overhead |
| Bandwidth | High bandwidth, high utilization | Low-bandwidth flows, statistical multiplexing |
| Latency | Jitter 허용치 낮음, microseconds | Jitter 허용치 높음, milliseconds 가능 |
| Topology | Nonblocking fat-tree/dragonfly | Oversubscription 허용 가능 |
| Traffic type | Bursty, predictable, collective operations | Heterogeneous, statistical multiplexing |

AI-optimized network의 **핵심 평가 기준은 Training job의 시작부터 끝까지 걸리는 시간 (job completion time)**입니다. 개별 packet latency/throughput뿐 아니라 전체 job의 완료 시간을 평가합니다.

## AI Factories vs AI Cloud

![AI Factories Networking]({{ '/assets/screenshots/ai-factories-networking.png' | relative_url }})
![AI Cloud Networking]({{ '/assets/screenshots/ai-cloud-networking.png' | relative_url }})

NVIDIA는 workload profile에 따라 다음처럼 구분합니다.

### AI Factories

- **목적:** 하나 또는 소수의 매우 큰 AI Training workload.
- **Models:** trillion-parameter급 large models.
- **Network:** scale-up NVLink + scale-out InfiniBand.
- **Topology:** non-blocking fat-tree InfiniBand.
- **예:** DGX SuperPOD, hyperscaler 내부 training clusters.

### AI Cloud

- **목적:** hyperscale multi-tenant services.
- **Workloads:** 다양한 동시 workloads와 많은 작은 jobs.
- **Network:** flexibility와 port cost에 유리한 Ethernet.
- **Topology:** ECMP Ethernet + RDMA over Converged Ethernet (RoCE).
- **예:** AWS, Azure, GCP GPU clouds.

**시험 팁:** 원본의 대응 관계는 AI Factory = NVLink + InfiniBand, AI Cloud = Ethernet + RoCE입니다.

## RDMA: Protocol 기반

**RDMA (Remote Direct Memory Access)**는 remote machine의 CPU/OS가 data transfer에 개입하지 않고 memory를 직접 read/write하도록 합니다.

- **Zero-copy:** CPU staging buffer를 줄이고 GPU↔network로 직접 전달합니다. GPU memory 직접 access에는 GPUDirect RDMA가 필요합니다.
- **CPU offload:** kernel bypass로 OS overhead를 줄입니다.
- **Low latency:** 원본의 round-trip 예시는 sub-2 microsecond입니다.
- **High bandwidth:** 400–800 Gbps link를 효율적으로 사용합니다.

RDMA는 native **InfiniBand**, 또는 standard Ethernet hardware 위의 **RoCE**에서 실행합니다.

자세한 내용은 [2.8 Networking Protocols]({{ '/ko/02-ai-infrastructure/08-networking-protocols/' | relative_url }})를 확인하세요.

## GPU당 Bandwidth Requirements

GPU performance가 높아지면 network bandwidth도 증가해야 합니다.

| GPU Generation | Compute (TFLOPS BF16) | Required network BW 예시 |
|---|---|---|
| A100 (2020) | 312 | 200 Gbps |
| H100 (2022) | 989 | 400 Gbps |
| B200 (2024) | ~2,250 | 800 Gbps |

Network가 compute에 비례해 확장되지 않으면 더 빠른 GPU도 data를 기다리므로 Training speedup이 제한됩니다.

## Self-check questions

1. AI cluster의 E-W traffic은 무엇인가요?
2. AI Training network에서 average latency보다 low jitter가 중요한 이유는 무엇인가요?
3. Workload 관점에서 AI Factory와 AI Cloud의 차이는 무엇인가요?
4. RDMA의 full name과 해결하는 CPU overhead 문제는 무엇인가요?
5. N-S/E-W 중 nonblocking topology를 사용하는 쪽과 이유는 무엇인가요?

<details>
<summary>정답과 해설</summary>

1. Distributed Training의 GPU gradient exchange입니다. 같은 cluster의 server 사이를 흐르는 주요 traffic입니다.<br>
2. All-reduce에서는 모든 GPU가 synchronize해야 합니다. 하나의 latency spike가 다른 GPU 전체의 대기를 유발해 GPU bubble이 생깁니다. Average latency만으로는 이러한 worst-case jitter를 알기 어렵습니다.<br>
3. AI Factory는 소수의 거대한 Training job을 dedicated/non-blocking InfiniBand에서 실행합니다. AI Cloud는 다양한 tenant의 동시 job을 cost-optimized Ethernet/RoCE에서 실행합니다.<br>
4. Remote Direct Memory Access. Remote CPU/OS의 kernel copy overhead 없이 직접 memory에 access하여 CPU utilization과 latency를 낮춥니다.<br>
5. E-W AI Fabric. Nonblocking fat-tree는 oversubscription 없이 GPU 간 full line-rate communication을 지원하여 collective operations에 적합합니다.

</details>
