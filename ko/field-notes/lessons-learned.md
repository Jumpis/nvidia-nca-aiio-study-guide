---
layout: korean
title: "Lessons Learned (한국어)"
parent: "Field Notes (한국어)"
nav_order: 2
grand_parent: 한국어 학습 가이드
permalink: /ko/field-notes/lessons-learned/
lang: ko
---

{% assign original_page = site.pages | where: "path", "field-notes/lessons-learned.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# Lessons Learned

*날짜별 경험 기록입니다. Customer/vendor confidential information은 포함하지 않습니다.*

## 2026

**2026년 5월 — InfiniBand Cabling의 영향**<br>
NDR 400G cluster에서 저렴한 passive copper DAC의 5m 초과 구간을 사용했더니 all-reduce throughput이 예상보다 40% 낮았습니다. AOC로 교체한 뒤 회복했습니다. 200G 이상 InfiniBand에서는 vendor-certified cable을 사용하고 passive copper의 엄격한 reach limit을 확인해야 한다는 원본의 경험입니다.

**2026년 5월 — GPU Utilization 99%와 효율적인 Training은 다름**<br>
`nvidia-smi`의 GPU utilization은 99%였지만 MFU는 ~18%였습니다. 작은 CUDA kernel launch가 많아 launch overhead가 컸고 Tensor Core compute는 지속되지 않았습니다. GPU utilization은 무엇인가 실행 중인지를 나타내므로 실제 Tensor Core utilization에는 `DCGM_FI_PROF_TENSOR_ACTIVE`를 함께 봅니다.

**2026년 5월 — RDMA는 검증 전까지 동작한다고 가정하지 않기**<br>
RoCE cluster의 gradient all-reduce가 느렸고, PFC가 구성되지 않아 NCCL이 TCP로 fallback한 것이 원인이었습니다. 원본은 ToR/uplink를 포함한 end-to-end lossless/PFC 구성을 강조합니다. Workload 실행 전에 `perftest`/`ib_write_bw`로 검증합니다.

**2026년 5월 — Firmware Version 불일치**<br>
ConnectX firmware, GPU driver, CUDA, NCCL의 dependency가 맞지 않아 hyperparameter 문제처럼 보이는 loss spikes가 발생했습니다. Driver + CUDA + NCCL + framework 전체 version을 pin하고 production 전에 조합을 test합니다. NGC container가 version 관리에 도움이 됩니다.

**2026년 5월 — Storage Tiering의 효과**<br>
한 분기에 dataset이 3× 커졌지만 active data용 NVMe-backed parallel FS와 archive용 저렴한 S3 object storage의 two-tier 구조로 capacity 문제를 피했습니다. 비싼 hot tier에 cold data를 저장하지 않습니다. 공간이 부족해진 뒤가 아니라 처음부터 tiering policy를 설계합니다.

*새 경험이 생기면 추가합니다. 항목마다 2–4문장으로 쓰고 날짜를 기록합니다.*
