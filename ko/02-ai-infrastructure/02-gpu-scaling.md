---
layout: korean
title: "2.2 Scaling GPU Infrastructure (한국어)"
parent: "Domain 2 — AI Infrastructure (40%) (한국어)"
nav_order: 2
grand_parent: 한국어 학습 가이드
permalink: /ko/02-ai-infrastructure/02-gpu-scaling/
lang: ko
---

{% assign original_page = site.pages | where: "path", "02-ai-infrastructure/02-gpu-scaling.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 2.2 Scaling GPU Infrastructure

## 시험에서 평가하는 내용

Scale-up/scale-out, NVLink, NVSwitch, NVIDIA multi-GPU interconnect가 performance scaling을 지원하는 방식을 이해합니다.

## Scale-Up vs Scale-Out

![Scale-Up vs Scale-Out]({{ '/assets/screenshots/scale-up-vs-scale-out.png' | relative_url }})

| 항목 | Scale-Up (Multi-GPU) | Scale-Out (Multi-Node) |
|---|---|---|
| **정의** | 한 server에 GPU 추가 | Cluster에 server 추가 |
| **Interconnect** | Server 내부 NVLink/NVSwitch | Server 간 InfiniBand/Ethernet |
| **Communication** | 매우 높은 bandwidth (900 GB/s – 1.8 TB/s) | 상대적으로 낮은 bandwidth (400 – 800 Gbps) |
| **Latency** | Sub-microsecond | Microseconds |
| **Load balancing** | Node 내부 GPU 사이 | Node 사이 |
| **Fault tolerance** | **약함:** server 하나의 failure로 모든 GPU 영향 | **강함:** node 하나의 failure는 cluster 일부에 영향 |
| **Use case** | 한 node에 들어가는 model, latency-sensitive task | 한 node보다 큰 model, 최대 규모 확장 |

**시험 팁:** 두 방식은 함께 사용합니다. Production AI cluster는 DGX node 내부에서 scale-up하고 InfiniBand로 node 간 scale-out합니다.

## Multi-GPU Systems: PCIe bottleneck

![Multi-GPU Systems]({{ '/assets/screenshots/multi-gpu-systems.png' | relative_url }})

초기 multi-GPU system은 PCIe switch로 GPU를 연결했습니다.

- PCIe Gen4 x16의 bidirectional bandwidth는 약 64 GB/s입니다.
- Neural network layers는 GPU 사이에서 큰 activation tensors를 교환합니다.
- PCIe가 bottleneck이 되어 GPU가 compute보다 data 대기에 더 많은 시간을 썼습니다.
- 효율적인 multi-GPU system은 **high-bandwidth inter-GPU communication**이 필요합니다.

NVIDIA가 NVLink를 개발한 이유는 GPU-to-GPU transfer에서 PCIe bottleneck을 줄이기 위해서입니다.

## NVLink: GPU-to-GPU Communication

![NVLink C2C]({{ '/assets/screenshots/nvlink-c2c.png' | relative_url }})

**NVLink**는 GPU-to-GPU transfer에 사용하는 NVIDIA proprietary high-speed interconnect입니다.

- **NVLink-C2C (Chip-to-Chip):** Superchip 내부 Grace↔GPU의 direct die-to-die connection.
- **NVLink 4 (Hopper):** GPU당 total bidirectional 900 GB/s.
- **NVLink 5 (Blackwell):** GPU당 total bidirectional 1.8 TB/s.
- **Coherent memory:** Superchip에서는 application이 GPU/CPU memory를 unified address space로 봅니다.
- **Near-linear performance scaling:** 충분한 bandwidth 덕분에 NVLink로 연결된 GPU를 추가하면 performance를 거의 선형적으로 늘릴 수 있다는 것이 원본의 설명입니다.

### NVLink generations

| Generation | GPU | GPU당 total bandwidth | 핵심 특징 |
|---|---|---|---|
| NVLink 2 | V100 | 300 GB/s | 초기 주류 NVLink |
| NVLink 3 | A100 | 600 GB/s | SXM4 package |
| NVLink 4 | H100 | 900 GB/s | GH200용 C2C |
| NVLink 5 | B200 | 1.8 TB/s | GB300 NVL72의 5th gen |

## NVSwitch: All-to-All GPU Communication

![NVSwitch Fabric]({{ '/assets/screenshots/nvswitch-fabric.png' | relative_url }})

**NVSwitch**는 system 내 GPU 전체에 fully non-blocking all-to-all NVLink fabric을 구성하는 전용 silicon switch입니다.

- NVSwitch가 없으면 제한된 NVLink port로 peer-to-peer 연결하므로 모든 GPU pair가 동시에 통신하기 어렵습니다.
- **NVSwitch가 있으면** 모든 GPU가 다른 GPU와 동시에 full bandwidth로 통신할 수 있습니다.
- **DGX H100은 4× NVSwitch**로 8-GPU all-to-all fabric을 구성합니다.
- 각 GPU의 NVLink port가 NVSwitch에 연결되며 NVSwitch가 GPU 간 traffic을 routing합니다.

### AI에서 All-to-all이 중요한 이유

AI/HPC의 all-reduce (gradient sync), all-gather (parameter distribution), reduce-scatter는 GPU 전체의 data 교환을 요구합니다. NVSwitch는 이를 full NVLink bandwidth로 지원해 communication bottleneck을 줄입니다.

```
              NVSwitch (x4 in DGX H100)
                  /  |  |  |  \
    GPU0 --- GPU1 --- GPU2 --- GPU3
     |               |               |
    GPU4 --- GPU5 --- GPU6 --- GPU7

    Each GPU: full NVLink bandwidth to ALL other GPUs simultaneously
```

각 GPU가 다른 모든 GPU와 동시에 full NVLink bandwidth로 연결된다는 개념을 나타낸 diagram입니다.

## DGX H100: 실제 Scale-Up

![DGX H100 System]({{ '/assets/screenshots/dgx-h100-system.png' | relative_url }})
![DGX H100 Physical Specs]({{ '/assets/screenshots/dgx-h100-physical-specs.png' | relative_url }})

NVIDIA의 reference AI training system이며 주요 사양은 다음과 같습니다.

- **8× H100 SXM5 80GB** Tensor Core GPUs.
- **4× NVSwitch:** 900 GB/s all-to-all GPU interconnect.
- **2 TB** system memory.
- **30 TB** NVMe SSDs: local high-speed model checkpointing.
- **8× ConnectX-7 400Gbps InfiniBand** NICs: 다른 DGX node로 scale-out.
- **2× Intel Xeon Platinum 8480C** CPUs.
- AI performance: **초당 32 quadrillion operations**.

원본은 10U rack server로 설명하지만 공식 DGX H100/H200 사양은 **8U**입니다. [NVIDIA User Guide](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html) 참조. Front panel, fan modules, NVLink switches, power supplies, ConnectX-7 network modules로 구성됩니다.

## Self-check questions

1. Scale-up과 Scale-out의 주요 trade-off는 무엇인가요?
2. NVLink 이전에 PCIe가 multi-GPU bottleneck이 된 이유는 무엇인가요?
3. H100 GPU당 total NVLink bandwidth는 얼마인가요?
4. 단순 NVLink peer-to-peer가 해결하지 못하는 어떤 문제를 NVSwitch가 해결하나요?
5. DGX H100에는 NVSwitch chip이 몇 개 있나요?

<details>
<summary>정답과 해설</summary>

1. Scale-up은 높은 bandwidth/sub-microsecond latency를 제공하지만 server 크기에 제한되고 fault tolerance가 약합니다. Scale-out은 fault tolerance와 확장성이 좋지만 node 간 bandwidth가 낮고 latency가 높습니다.<br>
2. Slot당 약 64 GB/s의 PCIe bandwidth가 neural network의 GPU 간 data 교환 요구보다 낮아 GPU가 data를 기다리며 idle 상태가 되기 때문입니다.<br>
3. Bidirectional 900 GB/s (NVLink 4, Hopper). Blackwell NVLink 5는 1.8 TB/s입니다.<br>
4. Non-blocking crossbar를 구성해 DGX의 8개 GPU 모두가 서로 full bandwidth로 동시에 통신하도록 합니다. 직접 연결된 peer만 통신하는 방식의 제한을 해소합니다.<br>
5. 4× NVSwitch chips.

</details>
