---
layout: korean
title: "DGX Systems (한국어)"
parent: "NVIDIA Portfolio Reference (한국어)"
nav_order: 3
grand_parent: 한국어 학습 가이드
permalink: /ko/nvidia-portfolio/dgx-systems/
lang: ko
---

{% assign original_page = site.pages | where: "path", "nvidia-portfolio/dgx-systems.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# NVIDIA DGX Systems

AI Training을 위해 설계한 systems입니다. NVIDIA는 "the gold standard for AI infrastructure"로 소개합니다.

## DGX Systems Lineup

![DGX Systems Lineup]({{ '/assets/screenshots/dgx-systems-lineup.png' | relative_url }})

| System | GPUs | 제품 positioning |
|---|---|---|
| DGX H100/H200 | 8× H100/H200 SXM | AI와 모든 Transformer-based workloads에 최적화 |
| DGX B200 | 8× B200 SXM | Blackwell 기반 Training/Inference/Transformer workloads |
| DGX B300 | 8× B300 SXM | Blackwell Ultra, 높은 node당 AI throughput |
| DGX GB200 | Multiple GB200 | 대규모 foundation model Training |
| DGX GB300 NVL72 | 72× B300 + 36× Grace | Advanced Superchip AI server rack |

## DGX H100 — The Gold Standard

![DGX H100 System]({{ '/assets/screenshots/dgx-h100-system.png' | relative_url }})

Data center 수준의 compute를 제공하는 AI system으로 소개됩니다.

| Component | Specification |
|---|---|
| GPUs | 8× NVIDIA H100 SXM5 80GB Tensor Core GPUs |
| GPU Interconnect | 4× NVSwitch, GPU당 all-to-all 900 GB/s |
| System Memory | 2 TB |
| Storage | 30 TB NVMe SSDs |
| Network | 8× ConnectX-7 400 Gbps InfiniBand NICs |
| CPUs | 2× Intel Xeon Platinum 8480C |
| AI Performance | 초당 32 quadrillion operations |

### Physical Design

![DGX H100 Physical Specs]({{ '/assets/screenshots/dgx-h100-physical-specs.png' | relative_url }})

- **Form factor:** 원본은 10U로 표기합니다. 공식 User Guide의 DGX H100/H200는 **8U**이므로 원본과 구분하세요.
- Components: front panel (power/I/O), fan modules, NVLink switches, 8× H100, NVMe drives, power supplies, 4× ConnectX-7 network modules, motherboard tray.

## GB300 NVL72 — Rack-Scale AI

![GB300 NVL72]({{ '/assets/screenshots/gb300-nvl72.png' | relative_url }})

AI reasoning 시대를 위한 rack-scale system입니다. Rack 전체가 하나의 computing unit으로 동작합니다.

| Component | Specification |
|---|---|
| GPUs | 72× NVIDIA Blackwell Ultra (B300) |
| CPUs | 36× NVIDIA Grace CPUs |
| NVLink generation | 5th |
| NVLink Bandwidth | Total **130 TB/s** |
| GPU Memory | Total **20 TB**, bandwidth **576 TB/s** |
| CPU Memory | **14 TB LPDDR5X** |

**핵심:** 72 GPUs가 5th-gen NVLink로 연결되어 rack 전체가 unified compute fabric을 형성합니다. 이 NVLink domain 내부 GPU communication은 node 간 InfiniBand hop을 경유하는 방식과 다릅니다.

## RTX PRO Server

![RTX PRO Server]({{ '/assets/screenshots/rtx-pro-server.png' | relative_url }})

AI와 Visual Computing용 Blackwell data center platform입니다.

| Spec | Value |
|---|---|
| CUDA parallel processing cores | 24,064 |
| Tensor Cores | 752, 5th gen |
| RT Cores | 188, 4th gen |
| Architecture | Blackwell |
| Target | Data center AI + professional visualization |

같은 server infrastructure에서 high-performance Inference와 photorealistic rendering/simulation/digital twins가 모두 필요한 조직에 적합합니다.

## DGX vs NVIDIA-Certified Servers

| 항목 | DGX Systems | NVIDIA-Certified Servers |
|---|---|---|
| Built by | NVIDIA | Dell/HPE/Lenovo 등 OEM |
| Configuration | 고정, NVIDIA-engineered | 다양함, NVIDIA-validated |
| Support | NVIDIA Support, optional | OEM support |
| Use case | 최대 performance, reference design | 유연한 deployment, 기존 vendor 관계 |
| GPU options | High-performance SXM | OEM에 따라 PCIe/SXM |
