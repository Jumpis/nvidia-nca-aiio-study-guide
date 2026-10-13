---
layout: korean
title: "Flashcards (한국어)"
parent: "Practice & Glossary (한국어)"
nav_order: 2
grand_parent: 한국어 학습 가이드
permalink: /ko/practice/flashcards/
lang: ko
---

{% assign original_page = site.pages | where: "path", "practice/flashcards.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# Flashcards — NCA-AIIO

Domain별 Q&A입니다. Spaced repetition으로 반복하며 영어 용어와 한글 설명을 연결하세요.

## Domain 1 — Essential AI Knowledge

**Q: AI, ML, DL은 어떻게 다른가요?**<br>
A: AI는 사람의 지능을 모방하는 가장 넓은 범주입니다. ML은 data에서 학습하는 AI, DL은 multi-layer neural network를 사용하는 ML입니다. DL⊂ML⊂AI이며 역방향 포함은 성립하지 않습니다.

---

**Q: NCCL의 full name과 역할은 무엇인가요?**<br>
A: NVIDIA Collective Communications Library. Multi-GPU/multi-node Training의 all-reduce, all-gather, broadcast, reduce-scatter 등 collective operations를 제공합니다.

---

**Q: Transformer는 무엇이며 현대 LLM을 가능하게 한 이유는 무엇인가요?**<br>
A: Sequential RNN과 달리 self-attention으로 Training token을 병렬 처리합니다. GPU utilization, long-range dependencies, scalability가 좋아졌습니다. GPT/LLaMA/Gemini 등은 Transformer 기반입니다.

---

**Q: NVIDIA software framework 세 가지와 목적을 말해 보세요.**<br>
A: TensorRT: Inference optimization. Triton Inference Server: production serving. NeMo: LLM/ASR/TTS Training/Fine-tuning. RAPIDS (GPU data science), DOCA (BlueField programming), NCCL (distributed communications)도 가능합니다.

---

**Q: VDI/Virtual Desktop용 GPU family는 무엇인가요?**<br>
A: Ada Lovelace의 L4 (VDI/VPC), L40 (Virtual Workstation)입니다. L4는 72W TDP로 standard server slot에 적합합니다.

---

**Q: Blackwell은 Hopper와 architecture가 어떻게 다른가요?**<br>
A: B200은 FP4의 2nd-gen Transformer Engine, 1.8 TB/s의 5th-gen NVLink (Hopper 900 GB/s), 208B transistors (Hopper 80B), hardware decompression, Secure AI enclave를 제공합니다. HBM3e 제품 간 capacity/bandwidth도 다릅니다. Hopper H100은 HBM3, H200은 HBM3e입니다.

---

**Q: RLHF는 무엇이며 AI lifecycle의 어느 단계인가요?**<br>
A: Reinforcement Learning from Human Feedback. Training/Fine-tuning 단계입니다. Human output rankings로 reward model을 training한 뒤 RL로 LLM을 align합니다. 원본은 ChatGPT의 핵심 요소로 소개합니다.

---

## Domain 2 — AI Infrastructure

**Q: GPU Scale-up과 Scale-out의 차이는 무엇인가요?**<br>
A: Scale-up은 한 node에 NVLink/NVSwitch로 GPU를 추가하여 bandwidth가 높지만 server failure의 영향이 큽니다. Scale-out은 IB/Ethernet으로 nodes를 추가하여 link당 bandwidth는 더 낮지만 fault tolerance/확장성이 좋습니다. Production에서는 함께 사용합니다.

---

**Q: DGX H100의 NVSwitch 목적은 무엇인가요?**<br>
A: 8개 GPU의 fully nonblocking all-to-all NVLink fabric을 만듭니다. 동시에 full bandwidth로 통신해 all-reduce 등을 지원합니다. DGX H100은 4× NVSwitch를 사용합니다.

---

**Q: PUE란 무엇이며 우수한 Data Center의 값은 얼마인가요?**<br>
A: Power Usage Effectiveness = Total Facility Power / IT Equipment Power. 1.0은 이론적 최선, 1.1–1.2는 hyperscale급 우수한 예시입니다. 높을수록 cooling/infrastructure overhead가 큽니다.

---

**Q: AI Fabric E-W와 Control/User Network N-S의 차이는 무엇인가요?**<br>
A: E-W는 Training GPU gradient exchange로 high bandwidth/low latency/nonblocking/RDMA가 중요합니다. N-S는 user access/management/internet의 loosely-coupled traffic으로 TCP와 oversubscribed topology를 사용할 수 있습니다.

---

**Q: RDMA란 무엇이며 AI Network에서 사용하는 이유는 무엇인가요?**<br>
A: Remote Direct Memory Access. Remote CPU/OS의 data-path 개입 없이 memory를 read/write합니다. Zero-copy, kernel bypass, 낮은 latency/CPU overhead를 제공하며 InfiniBand/RoCE Training에 중요합니다. 원본 latency 예시는 <1 µs입니다.

---

**Q: 소개된 RoCE Adaptive Routing/Congestion Control의 Hardware는 무엇인가요?**<br>
A: Server별 BlueField-3 DPU + Spectrum-4 switch입니다. Spectrum-4가 routing을 결정하고 BlueField-3가 in-order delivery/congestion signals를 처리합니다.

---

**Q: DPU의 세 가지 기능은 무엇인가요?**<br>
A: Offload: networking/storage/security를 CPU에서 이전. Accelerate: hardware로 더 빠르게 처리. Isolate: infrastructure를 host와 별도 security domain에 두어 host OS compromise 시에도 enforcement를 유지.

---

**Q: GPUDirect RDMA란 무엇인가요?**<br>
A: NIC가 CPU memory를 우회해 GPU memory에 직접 read/write합니다. GPU→CPU copy를 제거하여 PCI transactions, CPU usage, end-to-end latency를 낮춥니다.

---

**Q: 세 가지 Cooling과 대략적인 Rack Density는 무엇인가요?**<br>
A: 원본 예시: Air cooling ~30 kW/rack, Direct Liquid Cooling 50–80 kW/rack, Immersion cooling 100+ kW/rack. 실제 한계는 facility/system design에 따라 달라집니다.

---

**Q: Quantum-X800 Q3400-RA의 사양은 무엇인가요?**<br>
A: 144 ports×800 Gbps = 115.2 Tbps. 4th-gen SHARP, adaptive routing, congestion control을 제공하며 AI Factory InfiniBand fabric에 사용합니다.

---

**Q: Port당 400 Gbps인 InfiniBand Generation은 무엇인가요?**<br>
A: NDR (Next Data Rate). XDR (Extra Data Rate, Quantum-X800)는 port당 800 Gbps입니다.

---

**Q: GB300 NVL72의 GPUs/CPUs 수는 얼마인가요?**<br>
A: 72 Blackwell Ultra (B300) GPUs + 36 Grace CPUs. NVLink 5로 연결합니다. 원본의 total 사양: GPU memory 20 TB, CPU memory 14 TB, NVLink bandwidth 130 TB/s.

---

## Domain 3 — AI Operations

**Q: DCGM의 full name과 주요 기능은 무엇인가요?**<br>
A: Data Center GPU Manager. GPU health monitoring, active diagnostics/stress tests, 수백 telemetry metrics, power limits/clocks policy management를 제공합니다.

---

**Q: Slurm에서 8 GPUs를 Request하는 GRES Flag는 무엇인가요?**<br>
A: `--gres=gpu:8`

---

**Q: MIG와 time-sliced vGPU의 Isolation은 어떻게 다른가요?**<br>
A: MIG는 SM/HBM/L2/bandwidth를 hardware partition하여 performance/error isolation을 제공합니다. Time-sliced vGPU는 시간을 나누어 hardware를 공유하므로 다른 VM이 영향을 줄 수 있습니다. MIG-backed vGPU는 별도 구성입니다.

---

**Q: NVIDIA GPU Operator는 무엇을 설치하나요?**<br>
A: Drivers, CUDA runtime, DCGM, Device Plugin, Node Feature Discovery, MIG Manager, Container Toolkit 등 전체 GPU software stack을 한 단위로 관리합니다.

---

**Q: 수정할 수 없어 Page Retirement가 필요한 ECC Error는 무엇인가요?**<br>
A: DBE (Double-bit errors)입니다. SBE는 hardware가 수정합니다. DBE는 hardware 문제 가능성을 나타내므로 repair/retirement 등 조치가 필요합니다.

---

**Q: Gang Scheduling이란 무엇이며 Training에 필요한 이유는 무엇인가요?**<br>
A: Job의 모든 nodes/GPUs를 시작 전에 동시에 배정합니다. 일부만 받은 채 기다리면 GPU가 memory를 점유하며 idle 상태가 되어 compute 낭비/deadlock이 생길 수 있습니다.

---

**Q: 세 GPU Virtualization 방식과 Use Case는 무엇인가요?**<br>
A: Passthrough: 한 VM에 full GPU, sharing 없음, 최대 performance. Time-sliced vGPU: 여러 VM의 software sharing, license 필요, VDI. MIG: hardware partition, A100/H100 최대 7 instances, QoS가 중요한 multi-tenant Inference.

---

**Q: 어떤 GPU Temperature에서 Cooling을 점검하나요?**<br>
A: 원본 예시는 >85°C warning, >90°C critical입니다. GPU별로 83–87°C 무렵 clock reduction이 시작될 수 있으며 실제 model threshold를 확인해야 합니다.

---

**Q: Redfish와 IPMI의 차이는 무엇인가요?**<br>
A: 둘 다 BMC management용입니다. IPMI v2.0은 legacy binary protocol (1998), Redfish는 DMTF REST/JSON API (~2015)입니다. Dell iDRAC9+/HPE iLO5+ 등에서 Redfish가 IPMI를 대체하는 추세입니다.

---
