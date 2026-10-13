---
layout: korean
title: "Sizing a First AI Cluster (한국어)"
parent: "Field Notes (한국어)"
nav_order: 1
grand_parent: 한국어 학습 가이드
permalink: /ko/field-notes/01-sizing-a-first-cluster/
lang: ko
---

{% assign original_page = site.pages | where: "path", "field-notes/01-sizing-a-first-cluster.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# Sizing a First AI Cluster

*Enterprise GPU cluster 구축 경험을 익명화한 Field note입니다. 독점 수치는 포함하지 않습니다.*

## 먼저 물어야 하는 질문

"AI용 GPU cluster가 필요하다"는 말만으로 GPU 수를 결정할 수 없습니다. 실행할 workload와 일정, 실제 bottleneck이 compute인지 data/software/인력인지를 먼저 확인해야 합니다.

Spreadsheet를 만들기 전에:

1. **무엇을 실행하나요?** From-scratch Training, Fine-tuning, Inference, 또는 시간대별 세 가지 모두?
2. **가장 큰 Training model은 무엇인가요?** 최소 GPU memory를 결정합니다.
3. **Time-to-result 요구는 무엇인가요?** 2주 experiment window와 6시간 deadline은 필요한 resource가 다릅니다.
4. **Training data가 준비되어 있나요?** Data pipeline이 없으면 비싼 GPU cluster가 idle 상태가 됩니다.

## Memory-first Sizing

**GPU memory**는 중요한 constraint입니다. 원본은 VRAM에 들어가지 않는 model을 단순히 해결할 수 없다는 점을 강조합니다. 실제 sizing에서는 precision, quantization, sharding/offload 여부를 명시해야 합니다.

### LLM Training의 간단한 계산

```
GPU memory needed (bytes) = model parameters × bytes per parameter × overhead factor

FP16/BF16 training:  parameters × 2 bytes × ~4 (weights + gradients + optimizer states)
Example: 7B model    7,000,000,000 × 2 × 4 = 56 GB
Example: 70B model   70,000,000,000 × 2 × 4 = 560 GB
```

Parameter 수 × parameter당 bytes × overhead factor입니다. 원본의 FP16/BF16 예시는 2 bytes×~4 (weights + gradients + optimizer states)입니다. 7B는 56 GB, 70B는 560 GB가 됩니다.

70B는 aggregate memory 최소 560 GB, 즉 7× H100 80GB라는 model-state 계산입니다. **Activations는 포함하지 않아 실제 Training sizing에는 추가 여유가 필요**합니다.

### Inference Memory

```
Inference memory = parameters × bytes per parameter × quantization factor

FP16:    parameters × 2 bytes
INT8:    parameters × 1 byte
FP4:     parameters × 0.5 bytes

Example: 70B in FP16 = 140 GB → needs 2× H100 80GB minimum
Example: 70B in INT8 = 70 GB → fits on 1× H100 80GB
```

FP16은 parameter당 2 bytes, INT8은 1 byte, FP4는 0.5 bytes입니다. 70B weights는 FP16 140 GB (최소 2× H100), INT8 70 GB (1× H100)라는 계산입니다. KV cache/runtime overhead는 별도로 더합니다.

## Compute Sizing: Proxy 방법

Profiling이 없을 때의 간단한 추정입니다.

1. ML 팀과 GPU-hours 기준 주간 demand 추정.
2. Iteration/debugging/failed runs용 30% overhead 추가.
3. 168 hours/week로 나누어 steady-state GPU count 계산.
4. Burst용 20% headroom 추가.

**예시:**

- 주 5 experiments, 각 ~8 GPU-hours.
- 5×8 = 40 GPU-hours, ×1.3 = 52 GPU-hours/week.
- 52/168 = 0.31 GPU steady-state.
- Queue를 허용하면 minimum viable은 1 GPU.
- 원본은 팀 동시 작업을 위한 실용적 예시로 4–8 GPUs를 제안합니다. 동시성/대기 시간 요구에 따라 조정합니다.

## Network/Storage Bottlenecks

**실수 1: 저렴한 Network만 선택**

Multi-node Training에 10GbE를 사용하면 large gradient all-reduce가 step마다 수초를 소비할 수 있습니다. 원본은 7B model에서 400 Gbps InfiniBand는 milliseconds급으로 줄어드는 예시를 제시합니다. 실제 scaling은 collective/topology/model에 따라 profiling해야 합니다.

**경험칙:** multi-node Training은 GPU당 최소 1×100 Gbps NIC, 본격적인 cluster는 GPU당 400 Gbps InfiniBand를 원본의 기준으로 제시합니다.

**실수 2: Training Data를 NFS로만 제공**

수백 data workers의 random read를 storage가 감당하지 못하면 GPU utilization이 40% 수준으로 떨어질 수 있습니다. WEKA/Lustre parallel FS 또는 최소한 solid-state 기반 high-IOPS NFS appliance를 고려합니다.

**경험칙:** standard image classification은 8 GPUs당 aggregate storage throughput 최소 10 GB/s라는 원본 예시입니다. Large tokenized LLM dataset은 먼저 profiling합니다.

## Hardware 선택 전 Facility에 확인할 질문

"Rack당 최대 power delivery/cooling capacity는 얼마이며, upgrade 일정은 언제인가요?"

10 kW/rack이며 liquid cooling 계획이 없다면 원본은 L40S급 이하 server를 고려하도록 설명합니다. DGX H100 cluster rack은 ~30 kW cooling capacity, GB300 NVL72 rack은 ~120 kW라는 deployment 예시입니다. **DGX system 하나의 power와 여러 system을 담은 rack power는 구분**해야 합니다.

## First-cluster 경험

- **한 node를 먼저 구매해 전체 stack을 검증한 뒤 나머지를 주문합니다.** Lead time이 길고 반품이 어려울 수 있습니다. Storage/network/software integration 문제를 드러내는 하나의 DGX가 절반만 배포된 20-node cluster보다 유용합니다.
- **처음부터 OOB network를 계획합니다.** Remote power cycle, imaging, hardware diagnosis가 필요합니다. BMC network를 나중에 추가하기는 어렵습니다.
- **초기부터 자주 checkpoint합니다.** Power interruption/hardware failure로 수일의 작업을 잃지 않도록 허용 recovery time 기준으로 interval을 정합니다.
- **첫 bottleneck은 예상과 다를 수 있습니다.** Spec sheet만 믿고 GPU를 추가하기 전에 profiling합니다.
