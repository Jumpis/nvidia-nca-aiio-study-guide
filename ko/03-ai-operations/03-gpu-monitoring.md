---
layout: korean
title: "3.3 GPU Monitoring Key Measures (한국어)"
parent: "Domain 3 — AI Operations (22%) (한국어)"
nav_order: 3
grand_parent: 한국어 학습 가이드
permalink: /ko/03-ai-operations/03-gpu-monitoring/
lang: ko
---

{% assign original_page = site.pages | where: "path", "03-ai-operations/03-gpu-monitoring.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 3.3 GPU Monitoring Key Measures

## 시험에서 평가하는 내용

DCGM, operator가 확인하는 주요 GPU metrics, DCGM → Prometheus → Grafana stack 구성을 이해합니다.

## DCGM — Data Center GPU Manager

**DCGM (NVIDIA Data Center GPU Manager)**는 data center GPU health monitoring/management의 주요 tool입니다.

- **Health monitoring:** GPU 상태를 지속 점검하고 errors 보고.
- **Diagnostics:** memory tests/compute stress tests 등 active diagnostics.
- **Telemetry collection:** GPU별 수백 metrics를 real time으로 수집.
- **Job statistics:** Slurm/Kubernetes와 통합해 job별 utilization/memory/power 추적.
- **Policy management:** operating modes, power limits, clock settings.
- **NVML binding:** `nvidia-smi`와 같은 NVIDIA Management Library (NVML) 기반.

### DCGM Deployment

```
┌─────────────────────────────────────────────────────────┐
│  Each GPU node                                           │
│                                                          │
│  ┌──────────────┐    ┌───────────────────────────────┐  │
│  │  GPU Driver  │◄───│         DCGM daemon           │  │
│  │  (NVML)      │    │  (datacenter-gpu-manager.service)│ │
│  └──────────────┘    └───────────────┬───────────────┘  │
│                                      │ metrics           │
│                               ┌──────┴──────┐            │
│                               │ DCGM Exporter│            │
│                               │ (Prometheus  │            │
│                               │  format)     │            │
│                               └──────┬──────┘            │
└──────────────────────────────────────┼──────────────────┘
                                       │ scrape
                                  ┌────┴────┐
                                  │Prometheus│
                                  └────┬────┘
                                       │
                                  ┌────┴────┐
                                  │ Grafana  │
                                  └─────────┘
```

Node의 GPU Driver/NVML → DCGM daemon → DCGM Exporter에서 metrics를 제공하고, Prometheus가 scrape하며 Grafana가 시각화합니다.

### DCGM Exporter

- GPU node의 sidecar container/DaemonSet으로 실행합니다.
- Metrics를 **Prometheus exposition format**으로 변환합니다.
- Prometheus가 endpoint를 scrape하고 Grafana가 visualization을 제공합니다.

## 주요 GPU Metrics

### Utilization metrics

| Metric | 측정 대상 | 원본의 healthy value 예시 |
|---|---|---|
| **GPU Utilization (%)** | Sample 기간 중 kernel이 실행된 시간 비율 | Training 중 >80% |
| **SM Active (%)** | SM에서 warp가 active인 정도. GPU utilization보다 세밀함 | Compute-bound에서 >80% |
| **SM Occupancy** | Active warps / maximum possible warps | Workload별로 다름 |
| **Memory Utilization (%)** | 원본은 VRAM 사용 비율로 설명 | <95%, 여유 확보 |
| **Tensor Active (%)** | Tensor Cores 실행 시간 비율 | AI workload의 핵심 |

**Metric 구분:** Framebuffer 사용량/비율과 memory copy/DRAM activity metric은 서로 다릅니다. `Memory Utilization`이라는 이름만 보고 VRAM capacity 사용량으로 단정하지 말고 실제 field 정의를 확인하세요.

**GPU utilization이 80% 미만일 때 가능한 원인:**

- CPU preprocessing bottleneck: DALI 활용.
- Network gradient exchange bottleneck: bandwidth/latency.
- Storage data loading bottleneck: 더 빠른 parallel FS 필요.
- Memory-bound job 또는 너무 많은 작은 operations.

### Memory/Bandwidth metrics

| Metric | 설명 |
|---|---|
| **GPU Memory Used (MB)** | 실제 VRAM 사용량 |
| **Memory Bandwidth Utilization (%)** | Peak 대비 HBM read/write bandwidth |
| **NVLink Bandwidth (GB/s)** | Port별 Tx/Rx throughput |
| **PCIe Bandwidth** | CPU↔GPU PCIe data transfer |

### Power/Thermal metrics

| Metric | 설명 | 원본의 Alert threshold 예시 |
|---|---|---|
| **Power Draw (W)** | 현재 GPU power consumption | TDP에 가까움, H100 SXM 700W 예시 |
| **GPU Temperature (°C)** | Die temperature | >85°C warning, >90°C critical |
| **Memory Temperature (°C)** | HBM temperature | 보통 die temperature와 함께 변화 |
| **Fan Speed (RPM)** | PCIe card active fan speed | Fan speed 증가는 temperature 증가의 신호 |

**Thermal throttling:** threshold를 넘으면 clock을 자동으로 낮춰 Training performance가 저하됩니다. 원본 예시는 85–90°C이며 실제 threshold는 GPU별로 다릅니다. Temperature monitoring으로 cooling 문제를 조기에 찾습니다.

### Error metrics

| Metric | 설명 |
|---|---|
| **ECC Single-bit Errors (SBE)** | Corrected memory errors. 빈도가 높으면 hardware 문제 가능 |
| **ECC Double-bit Errors (DBE)** | Uncorrectable memory errors. Page retirement/repair 또는 교체 검토 |
| **XID Errors** | Driver error codes. XID 79: GPU fallen off the bus, XID 48: ECC DBE |
| **NVLink Errors** | Lane errors. Cable/connector 문제 가능 |

## NVLink Bandwidth Monitoring

Multi-GPU Training에서 중요한 metric입니다.

```
During all-reduce (gradient sync):
  Expected: each GPU transmitting 900 GB/s (full NVLink 4 bandwidth)
  Actual < 80% of peak → investigation needed:
    - Job using tensor parallelism correctly?
    - NVLink health errors?
    - Gradient compression applied?
```

원본은 all-reduce 때 900 GB/s와 peak의 80%를 기준으로 예시를 제시합니다. **900 GB/s는 H100의 total bidirectional bandwidth**이므로 단일 방향 Tx throughput과 같은 값으로 해석하면 안 됩니다. 실제 utilization은 workload/collective pattern에 따라 달라집니다. 낮으면 tensor parallelism 사용, NVLink health, gradient compression을 확인합니다.

## Monitoring Scenarios

### Scenario 1: Training이 예상보다 느림

1. GPU Utilization <70%인지 확인: CPU/data pipeline bottleneck 가능.
2. NVLink bandwidth가 예상보다 낮은지 확인: communication bottleneck 가능.
3. SM Active와 memory bandwidth 확인: utilization이 높아도 memory-bound일 수 있음.
4. GPU temperature/clock 확인: thermal throttling 가능.

### Scenario 2: 예상하지 못한 GPU Errors

1. DCGM/`nvidia-smi -q`에서 XID error code 확인.
2. `dcgmi diag -r 3` 실행. Level 3는 긴 GPU stress test.
3. ECC DBE 확인: page retirement/repair 또는 GPU 교체 검토.
4. NVLink error counters 확인.

### Scenario 3: Power/Cooling Alert

1. 모든 GPU의 power draw가 TDP에 가까우면 facility power limit 확인.
2. Temperature >85°C이면 cooling/airflow/DLC flow 점검.
3. DCGM throttling events 확인: clock 감소로 performance 저하.

## 주요 DCGM Field IDs — 원본 참조

Field identifier와 metric 이름은 영어를 유지합니다.

| DCGM Field | Metric name |
|---|---|
| DCGM_FI_DEV_GPU_UTIL | GPU utilization (%) |
| DCGM_FI_DEV_MEM_COPY_UTIL | Memory copy engine utilization |
| DCGM_FI_DEV_FB_USED | Framebuffer (GPU memory) used |
| DCGM_FI_DEV_GPU_TEMP | GPU temperature |
| DCGM_FI_DEV_POWER_USAGE | Power draw (W) |
| DCGM_FI_DEV_NVLINK_BANDWIDTH_TOTAL | Total NVLink bandwidth |
| DCGM_FI_DEV_ECC_SBE_VOL_TOTAL | Single-bit ECC errors (volatile) |
| DCGM_FI_DEV_ECC_DBE_VOL_TOTAL | Double-bit ECC errors (volatile) |

## Self-check questions

1. DCGM의 full name과 네 가지 주요 기능은 무엇인가요?
2. Kubernetes monitoring에서 DCGM Exporter의 역할은 무엇인가요?
3. GPU Utilization (%)는 무엇을 측정하나요?
4. ECC DBE의 의미는 무엇인가요?
5. 어떤 GPU temperature에서 cooling을 조사해야 하나요?

<details>
<summary>정답과 해설</summary>

1. Data Center GPU Manager. Health monitoring, active diagnostics, telemetry collection, policy management (clock/power limits).<br>
2. DCGM metrics를 Prometheus format으로 바꿔 HTTP endpoint에 제공합니다. Prometheus가 node별 endpoint를 scrape하고 Grafana가 dashboards/alerts를 만듭니다.<br>
3. Sample 기간 중 최소 하나의 kernel이 실행된 시간 비율입니다. 모든 SM이 얼마나 꽉 찼는지를 뜻하지 않습니다. 일부 SM에서 kernel 하나만 계속 실행되어도 100%일 수 있으므로 SM Active를 함께 봅니다.<br>
4. Hardware가 자동 수정할 수 없는 memory errors입니다. Affected page를 retire/repair해야 하며 지속적인 DBE는 GPU 교체가 필요한 hardware failure 신호일 수 있습니다.<br>
5. 원본의 예시는 >85°C warning, >90°C critical입니다. GPU별로 thermal throttling이 약 83–87°C에 시작할 수 있으므로 실제 GPU threshold를 확인합니다.

</details>
