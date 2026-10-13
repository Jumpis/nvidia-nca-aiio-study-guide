---
layout: korean
title: "Software Stack (한국어)"
parent: "NVIDIA Portfolio Reference (한국어)"
nav_order: 5
grand_parent: 한국어 학습 가이드
permalink: /ko/nvidia-portfolio/software/
lang: ko
---

{% assign original_page = site.pages | where: "path", "nvidia-portfolio/software.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# NVIDIA Software Stack Reference

상세 설명은 [Domain 1.1]({{ '/ko/01-essential-ai-knowledge/01-nvidia-software-stack/' | relative_url }})을 확인하세요. 이 문서는 quick-reference index입니다.

## Stack layers

```
Application Layer
  NeMo · RAPIDS · Triton IS · TensorRT-LLM · Merlin · Clara · Morpheus

Middleware / Frameworks
  PyTorch · TensorFlow · JAX (on NGC) · NeMo · RAPIDS

Inference Optimization
  TensorRT · TensorRT-LLM

Serving
  Triton Inference Server

Distributed Communication
  NCCL (GPU↔GPU collective ops)

Compute Primitives
  cuDNN · cuBLAS · cuSPARSE · DALI · cuDF

Platform
  CUDA · CUDA Runtime · CUDA Toolkit

GPU Management
  DCGM · nvidia-smi · NVML

Infrastructure / DPU
  DOCA SDK (BlueField programming)

Enterprise Platform
  NVIDIA AI Enterprise · NGC (container/model registry)
  Base Command Platform · Fleet Command
```

Application/framework → Inference optimization/serving → distributed communication → compute primitives → CUDA platform으로 연결됩니다. GPU management는 DCGM/NVML, DPU programming은 DOCA, enterprise 운영은 AI Enterprise/Base Command/Fleet Command가 담당합니다.

## Quick Reference

| Product | Category | 목적 |
|---|---|---|
| **CUDA** | Foundation | GPU compute programming model, NVIDIA AI software 기반 |
| **cuDNN** | Library | DL primitives: convolution/pooling/normalization 최적화 |
| **cuBLAS** | Library | GPU-accelerated BLAS/matrix operations |
| **NCCL** | Library | Multi-GPU all-reduce/broadcast 등 collective communications |
| **DALI** | Library | Training의 GPU data augmentation pipeline |
| **RAPIDS** | Framework | cuDF/cuML/cuGraph 기반 GPU data science |
| **TensorRT** | Optimizer | Inference model optimization, FP16/INT8/FP8/FP4 |
| **TensorRT-LLM** | Optimizer | Paged KV cache/continuous batching 기반 LLM Inference |
| **Triton IS** | Server | HTTP/gRPC production serving, dynamic batching |
| **NeMo** | Framework | LLM/ASR/TTS Training/Fine-tuning |
| **Merlin** | Framework | cuDF + HugeCTR + Triton 기반 recommendation system |
| **Clara** | Platform | Medical imaging AI |
| **BioNeMo** | Platform | Drug discovery/protein structure |
| **Morpheus** | Platform | Real-time network analytics 등 cybersecurity AI |
| **NGC** | Registry | GPU-optimized containers/pre-trained models |
| **NVIDIA AI Enterprise** | Suite | Enterprise SLA의 full-stack software, vGPU support |
| **Base Command** | Management | DGX cluster/job scheduling/software lifecycle |
| **Fleet Command** | Management | Edge + cloud deployment 관리 |
| **DCGM** | Monitoring | GPU health/telemetry/diagnostics |
| **DOCA** | DPU SDK | BlueField programming |
| **NVLink / NVSwitch** | Interconnect | Hardware GPU-to-GPU high-bandwidth fabric |
| **GPU Operator** | Kubernetes | GPU software stack 설치/관리 자동화 |
| **MIG Manager** | Kubernetes | MIG partitions 구성 |

## NGC Container Tags

Format: `nvcr.io/nvidia/<framework>:<version>-py3`

```
nvcr.io/nvidia/pytorch:24.01-py3
nvcr.io/nvidia/tensorflow:24.01-tf2-py3
nvcr.io/nvidia/tritonserver:24.01-py3
```

Tag에는 NVIDIA monthly release (YY.MM), framework/Python version 정보가 들어갑니다. Container README에 명시된 CUDA + driver 조합에서 test된 image를 사용합니다.
