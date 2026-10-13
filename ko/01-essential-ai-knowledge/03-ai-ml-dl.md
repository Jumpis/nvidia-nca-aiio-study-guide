---
layout: korean
title: "1.3 AI vs ML vs DL (한국어)"
parent: "Domain 1 — Essential AI Knowledge (38%) (한국어)"
nav_order: 3
grand_parent: 한국어 학습 가이드
permalink: /ko/01-essential-ai-knowledge/03-ai-ml-dl/
lang: ko
---

{% assign original_page = site.pages | where: "path", "01-essential-ai-knowledge/03-ai-ml-dl.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# 1.3 AI vs Machine Learning vs Deep Learning

## 시험에서 평가하는 내용

AI, ML, DL의 포함 관계와 neural networks, transformers, CNNs가 어느 범주에 속하는지 이해합니다.

## 세 분야의 포함 관계

```
┌─────────────────────────────────────────────────────────┐
│  Artificial Intelligence (AI)                            │
│  Any technique enabling machines to mimic human         │
│  intelligence: reasoning, planning, perception, NLP     │
│                                                         │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Machine Learning (ML)                             │  │
│  │  Systems that learn from data without being        │  │
│  │  explicitly programmed for each task               │  │
│  │                                                    │  │
│  │  ┌──────────────────────────────────────────────┐ │  │
│  │  │  Deep Learning (DL)                           │ │  │
│  │  │  ML using neural networks with many layers   │ │  │
│  │  │  (depth) — learns hierarchical features       │ │  │
│  │  │  directly from raw data                       │ │  │
│  │  └──────────────────────────────────────────────┘ │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

**AI ⊃ ML ⊃ DL**입니다. AI는 reasoning, planning, perception, NLP 등 사람의 지능을 모방하는 기법 전체이며, ML은 data에서 학습하는 방식, DL은 여러 layer의 neural network로 raw data에서 hierarchical features를 학습하는 방식입니다.

## 정의

### Artificial Intelligence (AI)

사람의 지능이 일반적으로 필요한 task를 수행하는 computer system을 포함하는 가장 넓은 범주입니다.

- **예:** rule-based expert systems, chess engines, speech recognition, image classification, language translation.
- **Learning이 필수는 아닙니다.** Hard-coded decision tree도 AI에 해당합니다.
- **Neural network가 필수는 아닙니다.**

### Machine Learning (ML)

사람이 작성한 명시적 rule을 따르는 대신 example (data)에서 학습하는 AI의 하위 분야입니다.

**세 가지 learning paradigm:**

| Paradigm | 동작 방식 | 예 |
|---|---|---|
| **Supervised** | Label이 있는 (input, output) pair로 학습 | Image classification, regression, fraud detection |
| **Unsupervised** | Label 없는 data에서 구조 발견 | Clustering, anomaly detection, dimensionality reduction |
| **Reinforcement** | Environment의 reward/penalty로 agent가 학습 | Game playing, robotic control, recommendation tuning |

**Non-deep ML algorithm 예:** Decision trees, random forests, gradient boosting (XGBoost), SVMs, k-means, PCA.

### Deep Learning (DL)

**많은 layer의 artificial neural networks**를 사용하는 ML의 하위 분야입니다. Depth는 input과 output 사이의 layer 수를 뜻합니다. Layer가 많아질수록 더 추상적인 features를 학습합니다.

**현대 AI에서 중요한 이유:**

- Manual feature engineering 없이 raw data에서 features를 자동 학습합니다.
- Shallow ML보다 많은 data와 compute를 활용해 확장하기 좋습니다.
- GPU parallelism이 training을 가능하게 했고 large datasets가 이를 뒷받침했습니다.

## 주요 Deep Learning architectures

| Architecture | Full name | 주요 용도 |
|---|---|---|
| **CNN** | Convolutional Neural Network | Image recognition, video, spatial data |
| **RNN/LSTM** | Recurrent NN / Long Short-Term Memory | Sequence 처리. 이전의 접근법으로 많은 경우 대체됨 |
| **Transformer** | — | Language models, vision, multimodal. 현재 주요 paradigm |
| **GAN** | Generative Adversarial Network | Image synthesis, data augmentation |
| **Diffusion Model** | — | Image/video generation (Stable Diffusion, DALL-E) |

### Transformer architecture

GPT, LLaMA, Gemini 같은 LLM과 ViT 등 vision model의 기반입니다. 핵심은 **attention mechanism**으로, sequence의 각 token이 다른 token을 참조해 long-range dependencies를 파악합니다.

**GPU가 필요한 이유:** sequence length n에 대한 attention은 O(n²) matrix operation입니다. 128K-token context window에서는 엄청난 matrix multiplication이 발생하므로 GPU Tensor Core에 잘 맞습니다.

## 비교

| 항목 | AI | ML | DL |
|---|---|---|---|
| Data에서 learning 필수 | 아니요 | 예 | 예 |
| Neural networks 필수 | 아니요 | 아니요 | 예 |
| GPU 필요 | 아니요 | 때에 따라 | 대규모일 때 필요 |
| Feature engineering | 수동 | 수동/자동 | 자동 |
| 예 | Expert system | Random forest | GPT-4, ResNet |

## Self-check questions

1. If/else logic을 사용하는 rule-based chatbot은 AI, ML, DL 중 어디에 해당하나요?
2. Deep Learning에서 "deep"은 무엇을 뜻하나요?
3. 현재 LLM에서 지배적인 neural network architecture는 무엇인가요?
4. Reinforcement learning은 어떤 ML paradigm에 해당하나요?
5. Deep Learning이 2012년 무렵 실용화된 이유는 무엇인가요?

<details>
<summary>정답과 해설</summary>

1. AI에만 해당합니다. Data에서 학습하지 않고 명시적인 rule을 따릅니다.<br>
2. Input과 output 사이의 layer 수 (depth)입니다. Layer가 많을수록 data의 더 추상적인 representation을 학습합니다.<br>
3. Self-attention을 사용하는 Transformer architecture.<br>
4. Supervised/Unsupervised와 별개의 paradigm입니다. Agent가 environment의 reward signal을 이용해 trial and error로 학습합니다.<br>
5. 두 요인이 결합했습니다. CUDA (2007)로 GPU general compute가 가능해져 training이 실용화되었고, ImageNet (2009) 같은 large labeled dataset이 등장했습니다. AlexNet (2012)이 이 조합의 효과를 보여 주었습니다.

</details>
