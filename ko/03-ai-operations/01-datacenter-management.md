---
layout: korean
title: "3.1 AI Data Center Management and Monitoring (한국어)"
parent: "Domain 3 — AI Operations (22%) (한국어)"
nav_order: 1
grand_parent: 한국어 학습 가이드
permalink: /ko/03-ai-operations/01-datacenter-management/
lang: ko
---

{% assign original_page = site.pages | where: "path", "03-ai-operations/01-datacenter-management.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 3.1 AI Data Center Management and Monitoring Essentials

## 시험에서 평가하는 내용

Out-of-band management, BMC/IPMI, NVIDIA Base Command Platform, 대규모 AI infrastructure의 일상 관리 tools를 이해합니다.

## 두 Management Planes

```
┌────────────────────────────────────────────────────────────────┐
│  In-Band Management                                             │
│  • Uses production network (same cables as workload traffic)    │
│  • Requires the server OS to be running                         │
│  • Tools: SSH, SNMP, Kubernetes API, Slurm CLI                  │
│                                                                  │
│  Out-of-Band (OOB) Management                                   │
│  • Separate dedicated management network                        │
│  • Works regardless of server OS state                          │
│  • Can power cycle, image, diagnose before OS loads             │
│  • Tools: BMC, IPMI, Redfish, iDRAC (Dell), iLO (HPE)          │
└────────────────────────────────────────────────────────────────┘
```

**In-Band Management**는 workload와 같은 production network를 사용하며 server OS가 실행 중이어야 합니다. SSH, SNMP, Kubernetes API, Slurm CLI가 예입니다. **Out-of-Band (OOB) Management**는 별도 dedicated network에서 OS 상태와 무관하게 power cycle, imaging, OS load 전 진단을 수행합니다. BMC, IPMI, Redfish, Dell iDRAC, HPE iLO를 사용합니다.

## BMC — Baseboard Management Controller

**BMC**는 server motherboard의 dedicated microcontroller로 OOB management를 제공합니다.

### 기능

- **Power control:** OS가 hung 상태여도 power on/off, hard reset, graceful shutdown 요청.
- **Serial console access (SOL):** boot messages/OS console을 remote에서 확인.
- **Hardware sensor monitoring:** CPU/memory/fan/power sensor readings.
- **Remote firmware updates:** 물리 접근 없이 BIOS/UEFI update.
- **Hardware event logging:** SEL (System Event Log)에 hardware errors 기록.
- **Remote media mounting:** network ISO image로 boot.

### BMC protocols/구현

| 항목 | 목적 |
|---|---|
| **IPMI (Intelligent Platform Management Interface)** | BMC communication의 industry-standard protocol. v2.0을 널리 지원 |
| **Redfish** | IPMI를 대체하는 REST/JSON API, DMTF standard |
| **iDRAC** | Dell BMC 구현. iDRAC9는 Redfish 지원 |
| **iLO** | HPE BMC 구현 |

### OOB Network Design

- Server별 dedicated 1GbE management port. Production NIC와 분리합니다.
- VLAN-isolated/물리적으로 분리된 management switch에 연결합니다.
- Management VLAN에서만 access하며 production에서 직접 routing하지 않습니다.
- Kernel panic, BIOS POST, physical maintenance 중에도 access할 수 있습니다.

## NVIDIA Base Command Platform

DGX cluster용 AI infrastructure management software입니다.

- **Cluster management:** DGX provision/configure/monitoring.
- **Job scheduling:** AI Training workload scheduler 통합.
- **Software lifecycle:** cluster의 OS/drivers/CUDA/NGC containers deployment/update.
- **Monitoring:** GPU utilization/health/alerts/capacity planning dashboards.
- **Multi-cluster support:** 하나의 interface에서 여러 DGX cluster 관리.

### NGC와의 관계

NGC container와 dataset을 지정한 Training job을 정의하면 Base Command가 scheduling, resource allocation, monitoring을 처리합니다.

### 사용 대상

DGX H100/B200 cluster를 가진 enterprise입니다. 수백 DGX에서 24/7 AI workload를 운영하는 복잡성을 줄입니다.

## Data Center Management Tools

### DCIM — Data Center Infrastructure Management

Sunbird, Nlyte, Vertiv 같은 DCIM platform은 다음을 제공합니다.

- Asset inventory: rack 위치와 연결.
- Rack/server별 power monitoring.
- Cooling/thermal management.
- Capacity planning.
- Change management.

DCGM의 GPU telemetry와 통합하면 physical power consumption부터 GPU별 compute utilization까지 볼 수 있습니다.

### Monitoring Stack

```
GPU Hardware Metrics
       │
       ▼
DCGM (Data Center GPU Manager)
       │
       ▼
DCGM Exporter (Prometheus format)
       │
       ▼
Prometheus (time-series collection)
       │
       ▼
Grafana (dashboards & alerting)
```

GPU metrics → DCGM → DCGM Exporter (Prometheus format) → Prometheus (time-series collection) → Grafana (dashboards/alerting) 순서입니다.

자세한 내용은 [3.3 GPU Monitoring]({{ '/ko/03-ai-operations/03-gpu-monitoring/' | relative_url }})을 확인하세요.

## Management Network Topology

```
                ┌─────────────────┐
                │  Management     │
                │  Workstation    │
                └────────┬────────┘
                         │
                ┌────────┴────────┐
                │   OOB Switch    │  ← 1GbE dedicated
                │  (mgmt-only)    │
                └────────┬────────┘
              ┌──────────┴───────────┐
    ┌─────────┴──┐            ┌──────┴──────┐
    │  BMC (DGX1) │  ...      │  BMC (DGX N) │
    └─────────────┘            └─────────────┘
    (powered, always            (powered, always
     accessible)                 accessible)
```

Management workstation이 dedicated 1GbE OOB switch를 통해 각 DGX BMC에 연결됩니다. BIOS POST, OS crash, production NIC failure 중에도 BMC power와 OOB 경로가 유지되면 접근할 수 있습니다.

## Self-check questions

1. In-band와 Out-of-band management의 차이는 무엇인가요?
2. OS의 SSH로는 어렵지만 BMC로 가능한 작업은 무엇인가요?
3. IPMI를 대체하는 현대 BMC protocol은 무엇인가요?
4. DGX cluster의 job scheduling/software lifecycle을 관리하는 NVIDIA platform은 무엇인가요?
5. BMC access에 dedicated OOB network를 사용하는 이유는 무엇인가요?

<details>
<summary>정답과 해설</summary>

1. In-band는 production network와 실행 중인 OS가 필요합니다. OOB는 BMC의 별도 network를 사용해 OS crash/BIOS 상태에서도 동작합니다. Server가 off여도 BMC standby power가 공급되면 접근할 수 있습니다.<br>
2. Hung OS의 power cycle/reset, OS load 전 serial console, 독립적인 sensor reading, remote firmware update, virtual media로 OS reinstallation.<br>
3. Redfish. DMTF의 REST/JSON API로 이전 binary IPMI를 대체합니다.<br>
4. NVIDIA Base Command Platform.<br>
5. Production network failure/misconfiguration이 management access까지 끊지 않도록 별도 경로를 유지하기 위해서입니다.

</details>
