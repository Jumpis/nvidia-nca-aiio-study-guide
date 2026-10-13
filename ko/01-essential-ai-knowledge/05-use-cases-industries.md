---
layout: korean
title: "1.5 AI Use Cases and Industries (한국어)"
parent: "Domain 1 — Essential AI Knowledge (38%) (한국어)"
nav_order: 5
grand_parent: 한국어 학습 가이드
permalink: /ko/01-essential-ai-knowledge/05-use-cases-industries/
lang: ko
---

{% assign original_page = site.pages | where: "path", "01-essential-ai-knowledge/05-use-cases-industries.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 1.5 Key AI Use Cases and Industries

## 시험에서 평가하는 내용

AI capability별 적용 산업과 이를 지원하는 NVIDIA products/platforms를 연결합니다.

## AI capability별 Use cases

### Generative AI

- **Text generation / LLMs:** Copilots, chatbots, summarization, code generation, document drafting.
- **Image synthesis:** 제품 design, marketing, 약물 분자 생성.
- **Video generation:** entertainment, autonomous vehicle용 synthetic training data.
- **NVIDIA solution:** Blackwell B200/B300 (Training), H100/L40S (Inference), NeMo framework.

### Computer Vision

- **Object detection / classification:** 제조 quality control, medical imaging, autonomous vehicles.
- **Video analytics:** surveillance, retail 방문 흐름 분석, sports analytics.
- **NVIDIA solution:** NVIDIA Metropolis (smart cities/retail), Holoscan (medical), L40S/L4 (Inference).

### Natural Language Processing (NLP)

- **Semantic search:** enterprise knowledge retrieval, 법률 문서 검토.
- **Translation:** real-time 다국어 customer support.
- **Sentiment analysis:** financial trading signals, brand monitoring.
- **NVIDIA solution:** NeMo, TensorRT-LLM, H100/B200.

### Recommendation Systems

- **Collaborative filtering, Deep Learning ranking:** e-commerce, streaming, social media.
- **NVIDIA solution:** NVIDIA Merlin (전체 recommender system framework), A100/H100 (Training).

### Autonomous Systems

- **Self-driving vehicles:** perception, path planning, sensor fusion.
- **Robotics:** pick-and-place, warehouse automation, surgical robots.
- **NVIDIA solution:** NVIDIA DRIVE (automotive), Isaac (robotics), Orin SoC.

### High-Performance Computing (HPC) + AI

- **Scientific simulation:** climate modeling, molecular dynamics, computational fluid dynamics.
- **Drug discovery:** protein structure prediction (AlphaFold급 model), virtual screening.
- **NVIDIA solution:** Grace CPU, Hopper H100, DGX SuperPOD.

## 산업별 AI application

| Industry | 주요 AI applications | NVIDIA platforms |
|---|---|---|
| **Healthcare & Life Sciences** | Medical imaging 진단, drug discovery, genomics, robotic surgery | Clara (medical imaging), BioNeMo (drug discovery) |
| **Financial Services** | Fraud detection, algorithmic trading, risk modeling, AML | RAPIDS (data analytics), Morpheus (cybersecurity) |
| **Manufacturing** | Predictive maintenance, visual inspection, digital twins, robotics | Metropolis, Isaac, Omniverse |
| **Retail & E-commerce** | Recommendation engines, demand forecasting, 무인 결제 매장 | Merlin (recommenders), Metropolis (vision) |
| **Telecommunications** | Network optimization, predictive maintenance, 5G RAN processing | AI-RAN, telco용 BlueField DPU |
| **Energy** | Seismic analysis, equipment fault prediction, smart grid optimization | HPC clusters, DGX systems |
| **Media & Entertainment** | AI content creation, upscaling, rendering acceleration, VFX | L40S, Omniverse, DLSS |
| **Government & Defense** | Intelligence analysis, logistics optimization, autonomous systems | NVIDIA-Certified Systems, edge AI |

## AI Factory vs AI Cloud

NVIDIA course에서 구분하는 두 가지 AI infrastructure deployment model입니다.

**AI Factory:**

- 하나 또는 소수의 대규모 AI training workload를 위해 설계합니다.
- Trillion-parameter급의 가장 큰 AI model을 실행합니다.
- Scale-up에 NVLink fabric, scale-out에 InfiniBand를 사용합니다.
- 예: NVIDIA DGX SuperPOD, hyperscaler 내부 training clusters.

**AI Cloud:**

- 다양한 AI workload를 hosting하는 hyperscale multi-tenant service입니다.
- 서로 다른 model size와 task를 사용하는 많은 사용자를 동시에 지원합니다.
- Cloud 규모의 flexibility/cost efficiency를 위해 Ethernet networking을 사용합니다.
- 예: AWS, Azure, GCP GPU instances.

## Self-check questions

1. End-to-end recommendation system 개발용 NVIDIA platform은 무엇인가요?
2. AI Factory와 AI Cloud deployment는 어떻게 다른가요?
3. Medical imaging AI용 NVIDIA SDK는 무엇인가요?
4. Financial services의 AI use case 두 가지를 말해 보세요.
5. 5G RAN processing과 telco AI workload를 처리하는 NVIDIA platform은 무엇인가요?

<details>
<summary>정답과 해설</summary>

1. NVIDIA Merlin.<br>
2. AI Factory는 하나 또는 소수의 매우 큰 Training task에 최대 throughput을 제공하도록 NVLink + InfiniBand로 구성합니다. AI Cloud는 multi-tenant로 다양한 동시 사용자/model/workload를 지원하며 Ethernet으로 flexibility와 여러 job의 utilization을 높입니다.<br>
3. NVIDIA Clara.<br>
4. 다음 중 두 가지: fraud detection, algorithmic trading, risk modeling, anti-money laundering (AML), credit scoring, customer churn prediction.<br>
5. NVIDIA AI-RAN. 5G deployment에서 BlueField DPU는 RAN processing offload에 사용됩니다.

</details>
