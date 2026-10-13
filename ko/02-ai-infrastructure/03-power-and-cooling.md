---
layout: korean
title: "2.3 Power and Cooling Requirements (한국어)"
parent: "Domain 2 — AI Infrastructure (40%) (한국어)"
nav_order: 3
grand_parent: 한국어 학습 가이드
permalink: /ko/02-ai-infrastructure/03-power-and-cooling/
lang: ko
---

{% assign original_page = site.pages | where: "path", "02-ai-infrastructure/03-power-and-cooling.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 2.3 Power and Cooling Requirements

## 시험에서 평가하는 내용

GPU thermal design power (TDP), rack power density, cooling 방법의 trade-off, data center efficiency metric인 PUE를 이해합니다.

## 제품별 GPU Power (TDP)

| GPU | Form Factor | TDP |
|---|---|---|
| NVIDIA L4 | PCIe | 72 W |
| NVIDIA L40S | PCIe | 350 W |
| NVIDIA H100 | PCIe | 350 W |
| NVIDIA H100 | SXM5 | 700 W |
| NVIDIA B200 | SXM | ~1,000 W |
| NVIDIA GB300 NVL72 (full rack) | Rack | 전체 rack ~120 kW |

**계산 시 구분:** 8× H100 SXM5의 GPU TDP 합은 8×700W = 5.6 kW입니다. 원본의 ~10.2 kW는 CPU, memory, storage, networking 등을 포함한 DGX H100 system 전체 power로 읽어야 합니다. 원본은 같은 문단에서 GPU만의 power와 system 전체 power를 혼용합니다. [DGX User Guide](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html)는 maximum system power를 10.2 kW로 명시합니다. GB300 NVL72 full rack 예시는 ~120 kW입니다.

Traditional enterprise server rack은 3–10 kW, AI rack은 30–120+ kW가 될 수 있어 AI data center에는 이에 맞는 power infrastructure가 필요합니다.

## Rack Power Density의 변화

```
Traditional enterprise IT:    3–10 kW / rack
AI-ready data center:        20–40 kW / rack
Modern GPU cluster:          40–80 kW / rack
DGX B200/GB300 dense:       80–120+ kW / rack
```

Traditional enterprise IT는 3–10 kW/rack, AI-ready는 20–40, modern GPU cluster는 40–80, dense DGX B200/GB300은 80–120+ kW/rack 수준의 예시입니다.

**실무 의미:** 기존 data center는 power/cooling upgrade 없이 현대 GPU cluster를 수용하기 어려운 경우가 많습니다. Colocation provider는 DLC와 40–80 kW/rack circuits를 갖춘 AI-ready 공간을 제공하는 추세입니다.

## Cooling 방법

### Air cooling

- CRAC/CRAH가 차가운 air를 server 전면 intake로 보내고 후면에서 hot air를 배출합니다.
- Hot-aisle/cold-aisle containment로 efficiency를 높입니다.
- 원본의 **실용적 한계 예시는 ~30 kW/rack**입니다.
- Standard rack의 L40S급 (350W PCIe) server 등에 사용합니다.

### Direct Liquid Cooling (DLC)

- Water/coolant를 GPU/CPU의 cold plate로 직접 순환시킵니다.
- Heat가 room air에 전달되기 전에 chip에서 제거합니다.
- 원본의 density 예시는 50–80 kW/rack입니다.
- 원본은 DGX H100 SXM 이상에 DLC가 필요하다고 설명하지만, cooling 방식은 실제 system/facility 설계에 따라 확인해야 합니다.
- **Rear-door heat exchangers:** rack 후면에서 exhaust air를 식혀 room으로 전달되는 heat를 줄입니다.

### Immersion cooling

- Server를 dielectric fluid (mineral oil/synthetic fluid)에 담급니다.
- **Single-phase:** fluid가 liquid 상태를 유지하며 heat exchanger로 heat를 제거합니다.
- **Two-phase:** fluid가 boil하고 vapor가 chiller에서 condense합니다. 높은 efficiency를 제공합니다.
- 100+ kW/rack density가 가능하며 거의 소음이 없습니다.
- Upfront cost가 높고 specialized facility가 필요합니다.
- 원본은 ultra-dense GB300/B200 deployment에서의 활용을 소개합니다.

### 비교

| Method | Max density 예시 | Cost | Retrofit 용이성 |
|---|---|---|---|
| Air (hot/cold aisle) | ~30 kW/rack | 낮음 | 높음. Standard CRAC |
| Direct Liquid Cooling | 50–80 kW/rack | 중간 | 중간. Plumbing 필요 |
| Rear-door HX | ~40 kW/rack | 중간 | 중간 |
| Immersion | 100+ kW/rack | 높음 | 낮음. Specialized facility |

## PUE — Power Usage Effectiveness

**PUE**는 data center의 energy efficiency를 나타냅니다.

```
PUE = Total Facility Power / IT Equipment Power

Where:
  Total Facility Power = power consumed by ALL equipment (IT + cooling + lighting + UPS losses)
  IT Equipment Power = power consumed by servers, storage, networking only
```

**PUE = Total Facility Power / IT Equipment Power**입니다. 분자는 IT + cooling + lighting + UPS losses 등 facility 전체 power, 분모는 servers/storage/networking의 power입니다.

| PUE | 평가 |
|---|---|
| 1.0 | 이론적으로 완벽함. 모든 power가 IT에 사용됨 |
| 1.1–1.2 | 매우 우수. Hyperscale data center (Google, Meta) |
| 1.3–1.4 | 우수. 목적에 맞게 설계한 colocation |
| 1.5–1.7 | 보통. 이전 enterprise data center |
| 2.0+ | 낮은 efficiency. Legacy facilities |

원본은 DLC 기반 AI data center의 target을 **PUE < 1.2**, air-cooled facility의 일반적인 예시를 1.3–1.5로 제시합니다.

**시험에서 주의:** PUE가 낮을수록 좋습니다. 1.0이 이론적 최선이며 높은 값은 IT 이외 overhead가 크다는 뜻입니다.

## Power infrastructure requirements

### Power delivery

- **3-phase power:** high current에서 single-phase보다 효율적이므로 rack-scale AI에 일반적입니다.
- **UPS (Uninterruptible Power Supply):** utility power 전환 중 short-term battery backup을 제공합니다. Graceful shutdown을 위한 원본 예시는 10–15분입니다.
- **PDUs (Power Distribution Units):** intelligent PDU는 outlet별 power monitoring과 remote switching을 지원합니다.
- **Redundancy:** mission-critical training cluster에는 N+1/2N power paths를 사용합니다.

### Power Capacity Planning

```
Total AI rack power = GPU TDP × number of GPUs
                    + CPU TDP × number of CPUs
                    + Storage (NVMe: ~10W per drive)
                    + Networking (ConnectX-7: ~20W per NIC)
                    + Memory, fans, board overhead

Rule of thumb: multiply calculated IT load × 1.25 for cooling overhead at PUE 1.25
```

GPU TDP×GPU 수 + CPU TDP×CPU 수 + storage (NVMe drive당 ~10W) + networking (ConnectX-7 NIC당 ~20W) + memory/fans/board overhead를 합산합니다. PUE가 1.25라면 IT load×1.25로 facility power를 추정합니다.

## Self-check questions

1. H100 SXM5의 TDP는 얼마인가요?
2. PUE의 full name과 높은 efficiency를 나타내는 값은 무엇인가요?
3. 40 kW를 넘는 rack에서는 어떤 cooling 방법을 고려하나요?
4. 기존 enterprise data center가 upgrade 없이 GPU cluster를 수용하기 어려운 이유는 무엇인가요?
5. PUE 1.6인 data center에서 IT equipment가 1 MW를 사용하면 facility 전체 power는 얼마인가요?

<details>
<summary>정답과 해설</summary>

1. H100 SXM5의 TDP는 700W.<br>
2. Power Usage Effectiveness. 1.1–1.2는 매우 우수한 예시이며 낮을수록 좋습니다. 1.0은 이론적 최선입니다.<br>
3. Direct Liquid Cooling (DLC), chip의 cold plate 또는 rear-door heat exchanger 등입니다. 원본은 ~30 kW/rack 이상에서 Air cooling이 어려워진다고 설명합니다.<br>
4. 기존 facility는 대체로 3–10 kW/rack과 Air cooling용입니다. 30–120+ kW/rack GPU rack은 PDU/breaker와 CRAC cooling capacity를 모두 초과할 수 있습니다.<br>
5. 1.6×1 MW = 1.6 MW. 나머지 0.6 MW는 cooling, lighting, UPS losses 등입니다.

</details>
