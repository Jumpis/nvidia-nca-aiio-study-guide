---
layout: korean
title: "2.4 On-Premises vs Cloud (한국어)"
parent: "Domain 2 — AI Infrastructure (40%) (한국어)"
nav_order: 4
grand_parent: 한국어 학습 가이드
permalink: /ko/02-ai-infrastructure/04-onprem-vs-cloud/
lang: ko
---

{% assign original_page = site.pages | where: "path", "02-ai-infrastructure/04-onprem-vs-cloud.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 2.4 On-Premises vs Cloud AI Infrastructure

## 시험에서 평가하는 내용

On-premises와 cloud의 trade-off, hybrid/multi-cloud가 적합한 상황을 이해합니다.

## 선택 기준

1. Data sovereignty/compliance requirements가 있나요?
2. Workload가 steady-state인가요, bursty/unpredictable인가요?
3. CapEx와 OpEx 중 어떤 cost model을 선호하나요?
4. 대규모 Training용 custom networking (InfiniBand)이 필요한가요?
5. 팀의 operational maturity는 어느 수준인가요?

## On-Premises

### 장점

| Factor | 설명 |
|---|---|
| **Data sovereignty** | Data를 facility 내부에 유지. Healthcare (HIPAA), finance (SOX), government, EU (GDPR)에 중요 |
| **Latency** | Data source까지 sub-millisecond. On-prem system에 연결된 real-time Inference에 중요 |
| **Cost at scale** | 지속적인 large-scale GPU utilization (>70%)에서는 3–5년 기준 CapEx가 OpEx보다 유리한 경우가 많음 |
| **Customization** | GPU generation, InfiniBand fabric, cooling을 선택. Shared tenancy 제약 감소 |
| **Network performance** | Cluster 내부 400/800 Gbps InfiniBand 구성 가능. 원본은 대부분 public cloud에서 제공되지 않는다고 설명 |
| **Predictable performance** | Dedicated hardware로 noisy-neighbor 영향 감소 |

### 단점

| Factor | 설명 |
|---|---|
| **High upfront cost** | 원본 예시: DGX H100 system당 ~$400K+, 전체 cluster는 수천만 dollar |
| **Long lead times** | GPU supply 제약. 대규모 procurement cycle 6–18개월 예시 |
| **Operational burden** | Network engineers, sysadmins, cooling/power operations 전문 인력 필요 |
| **Inflexible capacity** | Over-provisioning은 낭비, under-provisioning은 project 지연 |
| **Facility requirements** | Liquid cooling, power upgrade, physical security |

## Cloud

### 장점

| Factor | 설명 |
|---|---|
| **No CapEx** | Pay-per-use OpEx. 원본은 최신 GPU에 즉시 접근하는 장점을 설명 |
| **Elasticity** | 수분 내 0에서 수천 GPU로 확장하는 것이 목표 |
| **Speed to start** | 8-GPU H100 instance를 수분 내 provision 가능 |
| **Managed services** | Cloud-managed Kubernetes, databases, MLOps pipelines |
| **Geographic distribution** | 여러 region에서 global low-latency Inference |
| **Latest hardware** | Provider가 새 GPU generation을 빠르게 도입 |

### 단점

| Factor | 설명 |
|---|---|
| **Cost at scale** | 지속적인 대규모 GPU 사용은 비쌈. Spot instance는 비용을 줄이지만 복잡성 증가 |
| **Data transfer costs** | 큰 Training dataset의 cloud 이동은 비용과 시간이 큼 |
| **Limited InfiniBand** | 원본은 대부분 cloud cluster가 Ethernet을 사용해 가장 큰 Training에서 on-prem InfiniBand가 유리하다고 설명 |
| **Data privacy** | 규제 산업에서 cloud data processing에 제약 가능 |
| **Performance variability** | Shared infrastructure의 noisy-neighbor 영향 가능 |
| **Lock-in** | Cloud-native tools가 vendor dependency를 만듦 |

## Hybrid approach

Enterprise에서 흔히 사용하는 architecture입니다.

```
                    ┌──────────────────────────────┐
                    │         Cloud                 │
                    │  - Burst training capacity    │
                    │  - Dev/test environments       │
                    │  - Global inference endpoints  │
                    └──────────────┬───────────────┘
                                   │  VPN / Direct Connect
                    ┌──────────────┴───────────────┐
                    │        On-Premises            │
                    │  - Production training cluster │
                    │  - Sensitive data processing   │
                    │  - Edge inference (DGX/Jetson) │
                    │  - Core model registry         │
                    └──────────────────────────────┘
```

Cloud는 burst training capacity, dev/test, global Inference endpoints를 담당하고, on-prem은 production training, sensitive data, edge Inference (DGX/Jetson), core model registry를 담당합니다. VPN/Direct Connect로 연결합니다.

**일반적인 pattern:**

- 규제 대상/독점 data로 on-prem Training.
- Peak demand 때 cloud로 burst하여 capacity 추가.
- Global reach를 위해 cloud에 Inference deployment.

## Deployment별 NVIDIA solutions

| Environment | NVIDIA product |
|---|---|
| On-premises Training | DGX systems + InfiniBand + Base Command Platform |
| On-premises Inference | NVIDIA-Certified Servers + Triton + TensorRT |
| Cloud Training | 주요 cloud provider와 협력하는 DGX Cloud |
| Cloud Inference | Cloud GPU instance의 NGC containers + Triton |
| Hybrid management | NVIDIA Fleet Command (edge + cloud management) |
| Edge deployment | NVIDIA Jetson / Orin modules |

## 비교

| 항목 | On-Premises | Cloud |
|---|---|---|
| Cost model | CapEx, 큰 초기 비용 | OpEx, 사용량 기반 |
| Latency | 매우 낮음 | 낮음~중간 |
| Data control | 직접 통제 | Shared/계약 기반 |
| Scale flexibility | 낮음 | 매우 높음 |
| Operational overhead | 높음 | Managed services로 낮음 |
| 최신 GPU 접근 | Procurement 이후 | 원본은 즉시 접근으로 설명 |
| 적합한 상황 | 지속적, 대규모, 규제 대상 | Bursty, 실험, global |

## Self-check questions

1. Cloud보다 On-premises가 적합한 scenario 두 가지를 말해 보세요.
2. AI workload에서 Cloud의 주요 cost 이점은 무엇인가요?
3. 대규모 On-prem Training cluster가 유사한 Cloud보다 빠를 수 있는 이유는 무엇인가요?
4. Hybrid edge + cloud GPU deployment를 관리하는 NVIDIA platform은 무엇인가요?
5. Cloud AI compute의 noisy-neighbor 문제란 무엇인가요?

<details>
<summary>정답과 해설</summary>

1. 다음 중 두 가지: data sovereignty/compliance, 지속적인 높은 GPU utilization에서 CapEx의 이점, custom InfiniBand fabric 필요, on-prem system에 연결된 latency-sensitive real-time Inference.<br>
2. 초기 CapEx 없이 사용한 GPU 시간에만 비용을 냅니다. Scale-to-zero로 idle cost를 없애고 procurement 지연 없이 수천 GPU로 burst할 수 있습니다.<br>
3. 원본의 비교에서는 NDR 400G/800G InfiniBand가 일반적인 Cloud Ethernet보다 bandwidth가 높고 latency가 낮습니다. Node 간 gradient communication이 많은 model에서 Training 시간을 줄입니다. 실제 비교는 provider의 fabric 사양에 따라 달라집니다.<br>
4. NVIDIA Fleet Command.<br>
5. 물리 resource를 공유하므로 GPU instance가 dedicated여도 다른 tenant가 CPU, memory, network, storage를 사용하며 contention이 발생할 수 있습니다. Peak cloud utilization 때 Training이 느려지는 등 performance variability가 생깁니다.

</details>
