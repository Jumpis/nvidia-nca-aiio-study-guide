---
layout: korean
title: "Networking Hardware (한국어)"
parent: "NVIDIA Portfolio Reference (한국어)"
nav_order: 4
grand_parent: 한국어 학습 가이드
permalink: /ko/nvidia-portfolio/networking/
lang: ko
---

{% assign original_page = site.pages | where: "path", "nvidia-portfolio/networking.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# NVIDIA Networking Hardware

Protocol/use case는 [Domain 2.8]({{ '/ko/02-ai-infrastructure/08-networking-protocols/' | relative_url }})과 [Domain 2.9]({{ '/ko/02-ai-infrastructure/09-high-speed-network-options/' | relative_url }})를 확인하세요.

## Portfolio 개요

![NVIDIA Networking Portfolio]({{ '/assets/screenshots/nvidia-networking-portfolio.png' | relative_url }})

| Product family | Technology | Cluster 내 역할 |
|---|---|---|
| ConnectX / SuperNIC | InfiniBand + Ethernet | Server NIC, endpoint adapter |
| BlueField-3 | DPU (SmartNIC) | Infrastructure offload/security/storage |
| Quantum-X800 | InfiniBand switch | Scale-out E-W AI Fabric |
| Spectrum-X | Ethernet switch | Ethernet 기반 E-W AI Fabric |
| Spectrum | Standard Ethernet | N-S management/user network |
| Photonics switches | CPO IB/Eth | Ultra-high-density scale-out |

## ConnectX — Server Adapters (SuperNICs)

| Product | Bandwidth | 주요 기능 |
|---|---|---|
| ConnectX-7 | 400 Gbps | InfiniBand/Ethernet dual-mode, PCIe Gen5 |
| ConnectX-8 | **800 Gbps** | PCIe Gen6, 48 lanes, IB/Ethernet, AI compute offloads |

원본은 ConnectX-8을 Blackwell 시대의 standard NIC로 소개합니다. 800 Gbps가 B200의 network 요구에 대응합니다.

## BlueField-3 DPU

![BlueField-3 Overview]({{ '/assets/screenshots/bluefield-3-overview.png' | relative_url }})

| Spec | BlueField-3 |
|---|---|
| Network | 400 GbE |
| Arm cores | 16× Arm A78 |
| Accelerators | Crypto, regex, compression, NVMe-oF |
| SDK | DOCA |
| Security | Zero-Trust, secure boot |
| Storage | Composable NVMe-oF |

400 GbE NIC와 programmable infrastructure DPU 역할을 함께 합니다. 소개된 RoCE adaptive routing/congestion control에는 Spectrum-4 switch와 함께 구성합니다.

## Quantum-X800 InfiniBand

![Quantum-X800]({{ '/assets/screenshots/quantum-x800-infiniband.png' | relative_url }})

### Q3400-RA Switch

| Spec | Value |
|---|---|
| Ports | 144×800 Gbps |
| Total bandwidth | 115.2 Tbps |
| SHARP generation | 4th |
| Features | Adaptive routing, congestion control, power management |

**Use case:** AI Factory의 maximum performance, low latency, SHARP in-network compute.

## Spectrum-X Ethernet

![Spectrum-X Ethernet]({{ '/assets/screenshots/spectrum-x-ethernet.png' | relative_url }})

| Feature | 설명 |
|---|---|
| Protocol | Standard Ethernet physical layer |
| RDMA | NVIDIA enhancements가 적용된 RoCEv2 |
| Routing | Spectrum-4 + BlueField-3 기반 Adaptive routing |
| Congestion control | BlueField-3의 DCQCN |
| Software | Cumulus, NetQ, Air, RCP, DOCA |

**핵심 positioning:** "World's First Ethernet Platform for AI". Ethernet infrastructure에서 InfiniBand에 가까운 RDMA performance를 목표로 합니다.

## Photonics Switches

![Photonics Switches]({{ '/assets/screenshots/photonics-switches.png' | relative_url }})

Ultra-scale AI Factory용 CPO입니다.

| Product | BW | Ports | Cooling |
|---|---|---|---|
| Quantum-X Photonics (3450-LD) | 115 Tb/s | 144×800G (576×200G) | Liquid |
| Spectrum SN6810 | 102.4 Tb/s | 128×800G (512×200G) | Liquid |
| Spectrum SN6800 | **409.6 Tb/s** | 512×800G (2048×200G) | Liquid |

## 선택 가이드

| Scenario | 선택 |
|---|---|
| AI Factory, 최대 throughput | Quantum-X800 IB + ConnectX-8 |
| Ethernet 표준화 AI Cloud | Spectrum-X + BlueField-3 |
| Infrastructure security offload | BlueField-3 DPU |
| 수백만 GPU의 hyperscale | Photonics switches |
| Management/user access | Standard Ethernet Spectrum |
