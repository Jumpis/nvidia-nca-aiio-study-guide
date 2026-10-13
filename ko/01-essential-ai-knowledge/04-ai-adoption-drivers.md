---
layout: korean
title: "1.4 AI Adoption Drivers (한국어)"
parent: "Domain 1 — Essential AI Knowledge (38%) (한국어)"
nav_order: 4
grand_parent: 한국어 학습 가이드
permalink: /ko/01-essential-ai-knowledge/04-ai-adoption-drivers/
lang: ko
---

{% assign original_page = site.pages | where: "path", "01-essential-ai-knowledge/04-ai-adoption-drivers.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 1.4 Factors Behind Recent AI Improvements and Adoption

## 시험에서 평가하는 내용

지난 10년간 AI가 크게 발전한 이유와 현대 Deep Learning을 가능하게 한 data, compute, algorithms의 결합을 이해합니다.

## 현대 AI 발전의 세 가지 축

```
         Data          ×        Compute        ×      Algorithms
  (labeled datasets,      (GPU parallelism,         (Transformers,
   internet scale,         Tensor Cores,            attention, RLHF,
   synthetic data)         cloud AI infra)            scaling laws)
         │                      │                         │
         └──────────────────────┴─────────────────────────┘
                                │
                     Modern AI capabilities
                 (LLMs, image generation, AlphaFold,
                  autonomous driving, drug discovery)
```

Data (labeled datasets, internet scale, synthetic data), Compute (GPU parallelism, Tensor Cores, cloud AI infrastructure), Algorithms (Transformers, attention, RLHF, scaling laws)가 결합해 LLM, image generation, AlphaFold, autonomous driving, drug discovery를 가능하게 했습니다.

## 1. Data — 학습의 재료

### Scale

- 2010년 이전에는 수천~수백만 개의 labeled example을 사용했습니다.
- 현대 LLM은 internet text의 수조 token을, vision model은 수십억 image를 사용합니다.
- ImageNet (2009)의 1.2M labeled images가 Deep Learning의 도약을 뒷받침했습니다.

### Data types

- **Labeled data (Supervised learning):** human annotation이 필요합니다. 비용이 크지만 high-accuracy classifier에 중요합니다.
- **Unlabeled/self-supervised data (BERT, GPT 등):** text에서 다음 word를 예측하는 task는 human label 없이 supervision을 만들 수 있습니다.
- **Synthetic data:** simulation이나 training image를 만드는 diffusion model처럼 AI가 생성한 training data입니다.

### Data velocity

IoT, logs, cameras, user interactions의 real-time data는 지속적인 model 개선과 online learning 기회를 제공합니다.

## 2. Compute — 연산의 기반

### GPU parallelism

가장 큰 기여 요인입니다. Deep Learning은 대규모 matrix multiplication이며, 일반적인 DL workload에서 GPU는 CPU보다 100–1000× 빠르게 이러한 연산을 수행한다는 것이 원본의 설명입니다.

### Tensor Core 발전

원본은 NVIDIA 세대마다 effective training throughput이 대략 두 배씩 증가한다고 설명합니다.

| Year | GPU | Peak Tensor Core TFLOPS (FP16) |
|---|---|---|
| 2017 | V100 | 125 |
| 2020 | A100 | 312 |
| 2022 | H100 | 989 |
| 2024 | B200 | ~2,250 |

### Scaling laws (Chinchilla / GPT)

Compute와 dataset size가 증가하면 model performance가 예측 가능한 방식으로 개선된다는 경험적 관계입니다. 더 많은 compute와 data가 더 좋은 model로 이어진다는 roadmap을 제공해 대규모 GPU 투자의 근거가 되었습니다.

### Cloud AI infrastructure

AWS, Azure, GCP, Oracle Cloud는 초기 capital investment 없이 GPU cluster를 사용할 수 있게 하여 조직이 large AI model을 실험하는 진입 장벽을 낮췄습니다.

## 3. Algorithms — Model architecture의 발전

### Transformer (2017 — "Attention Is All You Need")

RNN은 sequence를 token별로 처리해 느리고 long-range dependencies를 다루기 어려웠습니다. Transformer의 self-attention은 모든 token을 동시에 처리하고 먼 위치의 dependencies도 파악합니다. 이를 통해 다음이 가능해졌습니다.

- Training parallelism과 높은 GPU utilization.
- Trillion-parameter 규모의 model scaling.
- GPT, BERT, LLaMA, Gemini, Claude 등 현대 LLM.

### Transfer learning과 Fine-tuning

대규모 data로 한 번 pre-train한 뒤 특정 task에 적은 비용으로 fine-tune합니다. 한 번의 training으로 얻은 foundation model을 수백 개 downstream application에서 활용할 수 있어 경제성이 높아졌습니다.

### RLHF (Reinforcement Learning from Human Feedback)

LLM을 human preferences에 맞추는 기법입니다. 원본은 ChatGPT 발전의 핵심 요소로 설명하며, instructions를 따르고 도움이 되는 답을 제공하며 harmful output을 줄이는 데 활용한다고 소개합니다.

### PEFT / LoRA (Parameter-Efficient Fine-Tuning)

Adapter 등 일부 parameter만 fine-tune하여 compute와 storage 비용을 크게 줄입니다. 대규모 GPU cluster가 없는 조직도 LLM을 customize할 수 있게 했습니다.

## 4. Ecosystem의 성숙

| 요인 | 영향 |
|---|---|
| **Open source frameworks** (PyTorch, TensorFlow, JAX) | 개발 진입 장벽 감소, 큰 community |
| **HuggingFace / model hubs** | Pre-trained models 공개, 가치 창출까지 걸리는 시간 단축 |
| **MLOps tooling** (MLflow, Kubeflow, W&B) | Model lifecycle management를 체계적으로 운영 |
| **Cloud APIs** (OpenAI, Anthropic, Google) | Infrastructure 전문 지식 없이 AI 기능 사용 |
| **NVIDIA NGC** | 배포 준비가 된 optimized/certified containers와 models |

## Self-check questions

1. Scaling laws에서 model quality를 결정하는 두 resource는 무엇인가요?
2. 2017년의 어떤 architecture 혁신이 현대 LLM 시대를 가능하게 했나요?
3. LLM에서 Self-supervised learning이 중요한 이유는 무엇인가요?
4. RLHF란 무엇이며 어떤 문제를 해결하나요?
5. Enterprise가 GPU compute에 쉽게 접근하도록 한 요인 두 가지를 말해 보세요.

<details>
<summary>정답과 해설</summary>

1. Compute (FLOPS)와 data입니다. 원본은 tokens/parameters를 함께 언급하며 Chinchilla scaling laws가 최적 비율을 제시했다고 설명합니다.<br>
2. Transformer architecture (self-attention). Token을 병렬 처리하고 long-range dependencies를 효율적으로 파악합니다.<br>
3. Next-token prediction은 Self-supervised라 비싼 human labeling이 필요하지 않습니다. Internet의 방대한 unlabeled text를 사용해 trillion-token corpus로 training할 수 있습니다.<br>
4. Reinforcement Learning from Human Feedback. Human rankings로 reward model을 학습하고 RL로 LLM을 최적화하여 output을 human preferences에 맞춥니다.<br>
5. 다음 중 두 가지: cloud GPU rentals (AWS/Azure/GCP), NVIDIA NGC containers, GPU cost per FLOP 감소, HuggingFace의 pre-trained models.

</details>
