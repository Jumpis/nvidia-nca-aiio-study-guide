---
layout: korean
title: 번역 원칙과 원문 주의사항
parent: 한국어 학습 가이드
nav_order: 8
permalink: /ko/translation-notes/
lang: ko
---

# 번역 원칙과 원문 주의사항

[한국어 홈]({{ '/ko/' | relative_url }})

## 영어 시험과 연결하는 번역

원본의 43개 Markdown 문서를 같은 구조로 번역합니다. Exam Overview, 22개 subdomain, NVIDIA Portfolio, Field Notes, 87개 Glossary terms, 28개 Flashcards, 50문항 Self-quiz를 포함합니다.

- **영어 유지:** 용어와 full name, 약어, 제품명, architecture 이름, metric/field IDs, command flags, code, diagram, screenshot 안의 표기.
- **한글 설명:** 정의, 원리, 비교 기준, 학습용 scenario, 학습 전략, Self-check/Flashcards와 해설. Use-case 이름처럼 시험에서 식별해야 하는 표현은 영어로 유지합니다.
- **원본 대응:** 각 site page의 `English original`로 원문을 바로 확인할 수 있습니다. Site navigation과 내부 link는 한국어 edition 안에서 이동합니다.
- **연습 자료:** Self-check 문항/정답, Flashcards 순서, Self-quiz 번호/A–D 순서/원본 Answer Key를 유지합니다. Self-quiz의 문제·상황·선택지·진행 안내는 원본 영어 그대로 유지하고, 해설만 한글로 제공합니다.

뜻을 이해한 뒤 `RDMA`, `MIG`, `Inference`, `All-reduce` 같은 영어 용어만 보고 설명할 수 있는지 확인하세요. 이어서 원문 문항을 읽으면 한글 설명과 영어 시험 표현을 연결하기 좋습니다.

## 확인된 원본 불일치

단순 번역으로 잘못된 정보를 암기하지 않도록, 원본 표기와 교정을 구분했습니다. 영문 원본은 수정하지 않았습니다.

| 항목 | 원본 표기/문제 | 학습할 때의 구분과 근거 |
|---|---|---|
| Domain 비중 | Domain 1을 가장 높은 비중으로 설명 | 원본 Blueprint의 Domain 2 **40%**가 Domain 1 **38%**보다 큼 |
| L40S RT Cores | 1.6의 5th-gen과 Portfolio의 3rd-gen이 불일치 | L40S는 **3rd-generation RT Cores**, 4th-generation Tensor Cores. [NVIDIA L40S](https://www.nvidia.com/en-us/data-center/l40s/) |
| DGX H100 Power | GPU만의 power와 system power를 ~10.2 kW로 혼용 | 8×700W GPU TDP 합은 **5.6 kW**. System maximum은 **10.2 kW**. [DGX User Guide](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html) |
| DGX H100 Physical | 10U, ~350 kg로 설명 | 공식 DGX H100/H200는 **8U**, system maximum **130.45 kg**. Rack 전체와 system 한 대를 구분. [DGX User Guide](https://docs.nvidia.com/dgx/dgxh100-user-guide/introduction-to-dgxh100.html) |
| H100 MIG 4명 예시 | 4×2g.20gb를 제시 | 2g.20gb는 최대 **3개**. 20 GB씩 4개는 **4×1g.20gb**가 가능하며 compute allocation은 다름. [MIG Profiles](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/supported-mig-profiles.html) |
| InfiniBand model | QM9700과 Quantum-X800을 혼용 | QM9700은 Quantum-2 **NDR 400 Gbps**, Quantum-X800 Q3400은 **XDR 800 Gbps**. [QM97XX Manual](https://networking-docs.nvidia.com/qm97x0hw/introduction), [Quantum-X800 Clusters](https://networking-docs.nvidia.com/nvidia-quantum-x800-xdr-clusters) |
| Facility circuit | 30A @ 208V 3-phase를 30 kW로 제시 | Power factor 1에서도 √3×208×30 ≈ **10.8 kW**. 원본 수치가 일치하지 않음 |
| Floor load | kg/m²와 lbs/ft² 환산 불일치 | 100–150 kg/m²는 약 **20.5–30.7 lbs/ft²**. 실제 facility rating 확인 필요 |
| GPU memory sizing | Weights와 같으면 Inference가 실행되는 것처럼 설명 | Weights 이외 **KV cache/runtime overhead**도 포함해야 함 |
| NVLink monitoring | H100의 900 GB/s를 Tx 기대치로 제시 | **Total bidirectional** 사양과 단일 방향 throughput은 다름 |
| vGPU/MIG 구분 | 모든 vGPU를 time-slicing으로만 설명 | **MIG-backed vGPU**도 있음. VM migration/license는 edition/platform/version별로 확인. [NVIDIA vGPU Overview](https://docs.nvidia.com/ai-enterprise/release-8/latest/infra-software/vgpu/overview.html) |
| SM 수 | 수천 SM이라고 설명 | **CUDA Cores 수와 SM 수를 구분**. 하나의 SM에 여러 CUDA/Tensor Cores가 있음 |

Self-quiz **8, 36, 42, 44, 46, 49번**은 조건에 따른 해석이나 원본의 모호함을 해설에 표시했습니다. 특히 44번은 원본 정답 B `--gres=gpu:4` 외에 A `--gpus=4`도 지원되는 option입니다. [Slurm sbatch](https://slurm.schedmd.com/sbatch.html)에서 job 전체 request와 node별 request를 구분하세요.

## 원본을 유지한 정보

시험 정책/가격, 제품 configuration, cooling density, latency/performance 배수, 권장 학습 시간은 저장소 원본을 기준으로 합니다. 발견한 불일치에 대한 주석은 전체 기술 사실의 최신성 검증을 뜻하지 않습니다. GH200/GB200 memory/C2C configuration, GB300 요약 table의 memory 불일치 등은 본문에 원본 표기를 표시했습니다.

시험 등록 전에는 [NVIDIA Certification Page](https://www.nvidia.com/en-us/learn/certification/ai-infrastructure-operations/)의 현재 안내를 확인하세요. Field Notes는 원본 작성자의 경험과 의견이며 보편적인 설계 기준이나 공식 시험 정책이 아닙니다.

## 출처와 License

원본 작성자: **Zia Zeeshan**. 원본: [NVIDIA NCA-AIIO Study Guide](https://github.com/ziash/nvidia-nca-aiio-study-guide). 번역 기준 commit: `a868c48`.

**CC BY 4.0**에 따라 원본 attribution을 유지하며 한국어 번역과 위 주석을 추가했습니다. NVIDIA의 공식 번역/승인 자료가 아니며 exam dump를 포함하지 않습니다.
