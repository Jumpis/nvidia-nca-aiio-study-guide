---
layout: korean
title: "2.6 Facility Requirements (한국어)"
parent: "Domain 2 — AI Infrastructure (40%) (한국어)"
nav_order: 6
grand_parent: 한국어 학습 가이드
permalink: /ko/02-ai-infrastructure/06-facility-requirements/
lang: ko
---

{% assign original_page = site.pages | where: "path", "02-ai-infrastructure/06-facility-requirements.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 2.6 Facility Requirements for AI Data Centers

## 시험에서 평가하는 내용

GPU-dense AI workload를 수용하는 power, cooling, space, connectivity, security 등 physical infrastructure requirements를 이해합니다.

## AI가 Facility Requirements를 바꾸는 이유

기존 enterprise data center는 Air cooling과 3–10 kW/rack을 기준으로 설계되었습니다. 현대 AI rack은 30–120+ kW를 사용하므로 facility 설계도 달라져야 합니다.

```
Traditional enterprise rack:  3–10 kW      → standard air cooling, standard PDU
AI-optimized rack (H100):    ~30 kW         → high-density air or rear-door DLC
AI rack (B200/DGX):          60–80 kW       → direct liquid cooling (mandatory)
GB300 NVL72 rack:            ~120 kW        → purpose-built liquid infrastructure
```

원본의 예시는 traditional rack 3–10 kW, H100 rack ~30 kW, B200/DGX rack 60–80 kW, GB300 NVL72 ~120 kW입니다. Density가 높아질수록 rear-door/direct liquid cooling과 전용 liquid infrastructure가 중요해집니다.

## Power infrastructure

### Power feed

- **3-phase power:** high-current/high-density rack의 표준입니다.
- **Per-rack circuit:** 원본은 30 kW rack에 30A @ 208V 3-phase circuit을 제시합니다. **이 수치는 서로 맞지 않습니다.** 이상적인 power factor 1에서도 √3×208V×30A ≈10.8 kW이므로 30 kW 설계값으로 사용하면 안 됩니다. 더 높은 density에는 더 큰 capacity가 필요합니다.
- **Transformer proximity:** voltage drop을 줄이도록 step-down transformer를 row 가까이에 배치합니다.

### UPS (Uninterruptible Power Supply)

- Utility outage 또는 generator switchover 때 battery backup을 제공합니다.
- AI Training job은 수분~수시간 이상 걸리므로 UPS가 generator를 대체하지는 않습니다. 전환 시간을 연결하는 역할입니다.
- Full load에서 일반적인 runtime 예시는 10–15분입니다.
- GPU cluster에는 clean power와 zero transfer time의 double-conversion online UPS를 선호합니다.

### Redundancy tiers (Uptime Institute)

원본의 redundancy/availability 참조 table입니다.

| Tier | Redundancy | Expected availability |
|---|---|---|
| Tier I | 없음 (N) | 99.671% |
| Tier II | N+1 | 99.741% |
| Tier III | N+1, concurrently maintainable | 99.982% |
| Tier IV | 2N, fully fault-tolerant | 99.995% |

원본은 AI Training cluster의 일반적인 target을 **Tier III**로 제시합니다. Downtime 없이 maintenance할 수 있고, Training을 checkpoint/resume할 수 있다면 Tier IV의 추가 비용이 정당화되지 않을 수도 있습니다.

### Generators

- Diesel/natural gas generator는 UPS runtime 이후에도 power를 제공합니다.
- Transfer time 예시: manual >30s, automatic transfer switch (ATS) <10s.
- Full IT load와 cooling equipment를 모두 포함해 sizing합니다.

## Cooling infrastructure

자세한 내용은 [2.3 Power and Cooling]({{ '/ko/02-ai-infrastructure/03-power-and-cooling/' | relative_url }})을 확인하세요.

### Facility-level Cooling components

- **CRAC (Computer Room Air Conditioning):** traditional raised-floor air-based unit. 원본의 실용적 한계 예시는 ~30 kW/rack.
- **CRAH (Computer Room Air Handler):** building chilled-water plant와 함께 사용하며 self-contained CRAC보다 효율적입니다.
- **Chilled Water Plant:** chillers, cooling towers, pumps. 대규모 cooling의 기반입니다.
- **DLC infrastructure:** CDUs (Coolant Distribution Units), manifolds, facility chilled-water supply/return.

### Hot/Cold Aisle Containment

- Cold intake와 hot exhaust air를 분리합니다.
- **Cold-aisle containment:** door/roof로 cold aisle을 가두고 hot air는 room return으로 흐릅니다.
- **Hot-aisle containment:** hot aisle을 가두고 room에는 cold air를 공급합니다.

## Space와 Physical layout

### Raised floor

- 전통적으로 cold-air distribution plenum과 cable management에 사용합니다.
- Overhead cabling/in-row cooler를 사용하면 필수가 아닙니다.
- AI cluster는 overhead cable management와 leak 대응용 floor drains를 사용하는 경우가 많습니다.

### Floor load capacity

- 원본은 standard floor를 **100–150 kg/m² (약 300–450 lbs/ft²)**로 표기합니다. **두 단위의 환산은 일치하지 않습니다.** Floor rating은 실제 facility specification을 확인해야 합니다.
- Liquid-cooled GPU rack의 load는 높습니다. 원본의 DGX H100 weight 예시는 ~350 kg이지만, 공식 system maximum은 **130.45 kg**입니다. Rack 전체 weight와 구분합니다.
- 기존 facility에 설치하기 전 **structural assessment가 필요**합니다. System weight/form factor는 [DGX User Guide](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html)를 기준으로 구분합니다.

### Rack unit (U) 계획

- 1U = 1.75 inches. Standard rack은 42U.
- DGX H100: 원본 10U, 공식 사양 **8U**.
- GB300 NVL72 = purpose-built full rack.
- Top-of-rack switches, patch panels, cable management 공간을 남깁니다.

## Connectivity

### External network connectivity

- 서로 다른 provider의 **multiple diverse fiber paths**로 building의 single point of failure를 줄입니다.
- Meet-Me Room (MMR)의 cross-connect로 carriers/internet exchanges에 연결합니다.
- External data transfer가 많은 cluster에는 100G/400G uplink를 사용합니다.

### Internal network runs

- 원본은 짧은 구간에 low-loss fiber/copper DAC (최대 3m 예시), 긴 구간에 AOC (active optical cable)를 제시합니다.
- Fat-tree의 compute-to-spine structured cabling을 고려해 rack 설치 전에 cable path를 계획합니다.

## Physical security

| Layer | 조치 |
|---|---|
| **Perimeter** | Fence, vehicle barriers, security guard |
| **Building access** | Biometric/badge reader, mantrap/airlock |
| **Data center floor** | 별도 badge zone, CCTV, visitor log |
| **Rack level** | 잠금 rack door, multi-tenant cage isolation |
| **OOB management** | Production과 물리적으로 분리한 dedicated IPMI/BMC network |

## AI Rack Deployment 전 Checklist

- [ ] 필요한 rack power circuit capacity (30–120 kW/rack) 확인
- [ ] GPU rack weight에 맞는 floor load rating 확인
- [ ] Air/rear-door DLC/direct DLC/immersion 중 cooling 선택
- [ ] DLC 사용 시 chilled-water supply 확인
- [ ] High-density InfiniBand cable management 계획
- [ ] Redundant power feeds (UPS + generator) 확인
- [ ] Training data ingestion/model upload용 external bandwidth 평가
- [ ] Compliance에 맞는 physical security 확인

## Self-check questions

1. AI Training cluster에서 일반적으로 target하는 Uptime Tier는 무엇인가요?
2. GPU rack deployment에서 Floor load capacity가 중요한 이유는 무엇인가요?
3. Hot/Cold Aisle Containment의 목적은 무엇인가요?
4. Utility power failure와 generator startup 사이를 연결하는 장치는 무엇인가요?
5. 40 kW 이상 rack에서 일반적으로 필요한 cooling 방식은 무엇인가요?

<details>
<summary>정답과 해설</summary>

1. 원본의 답은 Tier III, concurrently maintainable, availability 99.982%입니다. Tier IV 전체 비용보다 낮은 비용으로 redundancy를 제공하며 Training은 checkpoint/resume할 수 있습니다.<br>
2. GPU rack은 무거워 structural floor rating을 초과할 수 있습니다. 원본은 ~350 kg을 제시하지만 공식 DGX H100/H200 system maximum은 **130.45 kg**입니다. Load assessment와 필요 시 reinforcement가 필요합니다. 본문의 단위 불일치 때문에 원본 floor 수치를 설계에 그대로 쓰면 안 됩니다.<br>
3. Server를 식히기 전에 cold supply와 hot exhaust가 섞이는 것을 막습니다. 섞이면 cooling effectiveness가 떨어지고 더 큰 cooling capacity가 필요합니다.<br>
4. UPS. Generator가 시작되고 안정화될 때까지 10–15분 battery-backed power를 제공합니다.<br>
5. GPU/CPU cold plates에 chilled water를 공급하는 DLC. 원본은 30–50 kW에서 rear-door heat exchangers도 사용한다고 설명합니다.

</details>
