---
layout: korean
title: "Glossary (한국어)"
parent: "Practice & Glossary (한국어)"
nav_order: 1
grand_parent: 한국어 학습 가이드
permalink: /ko/practice/glossary/
lang: ko
---

{% assign original_page = site.pages | where: "path", "practice/glossary.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

# Glossary — NCA-AIIO Acronyms and Terms

세 개 exam domain의 약어와 주요 용어를 원본 순서로 정리합니다. Term과 full name은 영어로, 정의는 한글로 제공합니다.

---

## A

**AI (Artificial Intelligence)** — 일반적으로 사람의 지능이 필요한 task를 machine이 수행하도록 하는 기법 전체입니다. ML/DL을 포함합니다.

**AI Enterprise (NVIDIA AI Enterprise)** — NVIDIA의 full-stack enterprise AI software suite입니다. TensorRT, Triton, NeMo, RAPIDS와 enterprise SLA를 포함합니다. 원본은 vGPU deployment용으로 제시하며 실제 license는 edition별로 확인합니다.

**AI Factory** — 하나 또는 소수의 매우 큰 Training workload를 위해 설계한 AI data center입니다. NVLink + InfiniBand를 사용하며 AI Cloud와 구분합니다.

**AI Cloud** — 다양한 동시 workload를 제공하는 multi-tenant hyperscale AI infrastructure입니다. Ethernet + RoCE를 사용하며 AWS/Azure/GCP GPU cloud가 예입니다.

**ATS (Automatic Transfer Switch)** — Utility outage 때 generator로 power를 자동 전환하는 electrical switch입니다. 원본의 transfer time 예시는 <10초입니다.

**AOC (Active Optical Cable)** — 양 끝에 powered transceiver가 있는 fiber optic cable입니다. 원본은 3m를 넘는 InfiniBand 구간에 사용한다고 설명합니다.

---

## B

**BF16 (Brain Float 16)** — 8 exponent bits와 7 mantissa bits를 사용하는 16-bit floating-point format입니다. FP16보다 dynamic range가 넓어 Training에서 선호하며 Ampere 이후 Tensor Core에서 지원합니다.

**BlueField (BlueField-3)** — NVIDIA DPU product line입니다. BlueField-3는 400 GbE, Arm cores, hardware accelerators를 제공하며 DOCA로 programming합니다.

**BMC (Baseboard Management Controller)** — Server motherboard의 dedicated microcontroller입니다. Host OS와 독립적으로 OOB power control, console access, hardware monitoring을 제공합니다.

---

## C

**CapEx (Capital Expenditure)** — Hardware/infrastructure를 구매하는 초기 비용입니다. On-premises deployment에서 비중이 큽니다.

**C2C (Chip-to-Chip)** — Superchip 내부 CPU/GPU dies를 직접 연결하는 NVLink variant입니다. GH200의 Grace+Hopper 연결처럼 coherent memory access를 제공합니다.

**CRAC (Computer Room Air Conditioning)** — Data center의 self-contained cooling unit입니다. 원본의 실용적 density 한계 예시는 ~30 kW/rack입니다.

**CRAH (Computer Room Air Handler)** — Building의 chilled-water plant에 연결된 air handler입니다. Large deployment에서 CRAC보다 효율적입니다.

**CUDA (Compute Unified Device Architecture)** — NVIDIA parallel computing platform/programming model입니다. NVIDIA AI software의 기반입니다.

**cuDNN** — GPU-accelerated Deep Learning primitives library입니다. NVIDIA GPU의 PyTorch/TensorFlow/JAX 내부에서 사용합니다.

---

## D

**DAC (Direct Attach Copper)** — 짧은 InfiniBand/Ethernet 연결용 passive copper cable입니다. 원본 reach 예시는 최대 ~3m이며 실제 reach는 cable specification을 확인합니다.

**DCGM (Data Center GPU Manager)** — NVIDIA GPU monitoring/diagnostics framework입니다. Health monitoring, telemetry, active diagnostics, policy management를 제공합니다.

**DCIM (Data Center Infrastructure Management)** — Physical data center assets의 power/cooling/space/connectivity를 관리하는 software입니다.

**DCN (Data Center Networking)** — Data center 내부 network infrastructure를 뜻하는 일반 용어입니다.

**DL (Deep Learning)** — 여러 layer의 neural networks를 사용하는 ML의 하위 분야입니다. Transformers/CNNs/diffusion models 등 현대 AI의 기반입니다.

**DLC (Direct Liquid Cooling)** — GPU/CPU cold plate로 coolant를 직접 보내는 cooling 방식입니다. 원본은 >40 kW rack에서 필요하다고 설명하며 실제 design별 capacity는 확인해야 합니다.

**DMA (Direct Memory Access)** — CPU의 data-path 개입 없이 device가 local host memory에 직접 access하는 방식입니다.

**DOCA (Data Center Infrastructure on a Chip Architecture)** — BlueField DPU programming용 NVIDIA SDK입니다. Generation 간 unified API를 제공합니다.

**DPU (Data Processing Unit)** — CPU/GPU와 함께 computing의 third pillar입니다. Networking/storage/security task를 offload, accelerate, isolate합니다. NVIDIA 제품은 BlueField입니다.

---

## E

**E-W (East-West)** — 같은 cluster의 server 간 traffic입니다. AI에서는 GPU gradient exchange가 주요 예이며 high bandwidth/low latency가 필요합니다.

**ECC (Error Correction Code)** — Memory protection 기술입니다. SBE는 correctable이며 DBE는 uncorrectable로 page retirement/repair 등 조치가 필요합니다.

**ECN (Explicit Congestion Notification)** — Switch가 congestion을 packet에 mark하고 receiver가 sender에 감속을 알리는 방식입니다. RoCE의 DCQCN에 사용합니다.

---

## F

**FP4 / FP8 / FP16 / FP32** — Floating-point precision formats입니다. 낮은 precision은 memory footprint를 줄이고 throughput을 높일 수 있습니다. Blackwell은 FP4를 추가했습니다.

**Fat-tree** — 원본에서 nonblocking 구성의 aggregate uplink/downlink bandwidth가 같은 topology로 설명합니다. Oversubscription 없이 node 간 full line-rate communication을 목표로 합니다.

**FDR InfiniBand** — 56 Gbps InfiniBand generation입니다. Legacy입니다.

---

## G

**GRES (Generic Resource Scheduling)** — GPU 등 non-CPU resources를 추적/할당하는 Slurm mechanism입니다. GRES는 Generic Resource를 뜻하며 scheduling에 사용합니다.

**GPU (Graphics Processing Unit)** — Parallel processing에 최적화된 processor입니다. Matrix/tensor operations를 처리하며 AI workload의 주요 compute engine입니다.

**GPU Operator (NVIDIA GPU Operator)** — Drivers/CUDA/DCGM/Device Plugin/MIG Manager 등 NVIDIA GPU stack의 Kubernetes 설치/관리를 자동화합니다.

**GPUDirect RDMA** — NIC가 CPU memory를 경유하지 않고 GPU memory에 직접 read/write하도록 합니다. Distributed Training의 GPU→CPU→NIC copy를 제거합니다.

---

## H

**HBM (High Bandwidth Memory)** — GPU package의 stacked DRAM입니다. 원본 bandwidth 예시는 3–8 TB/s이며 H100/B200 같은 data center GPU에 사용합니다.

**HDR InfiniBand** — 200 Gbps InfiniBand generation입니다. 원본은 H100 cluster의 이전 standard로 소개합니다.

**HPC (High-Performance Computing)** — Scientific simulation/research용 computing입니다. Shared GPU cluster에서 AI와 함께 실행하기도 합니다.

---

## I

**IB (InfiniBand)** — Native RDMA의 high-throughput/low-latency interconnect입니다. HPC/AI Factory scale-out network에 사용합니다.

**IBTA (InfiniBand Trade Association)** — InfiniBand specification을 관리하는 organization입니다.

**iDRAC (Integrated Dell Remote Access Controller)** — Dell server용 BMC 구현입니다. Out-of-band management를 제공합니다.

**iLO (Integrated Lights-Out)** — HPE의 BMC 구현입니다.

**IPMI (Intelligent Platform Management Interface)** — BMC communication용 industry-standard protocol입니다. Version 2.0을 널리 지원하며 Redfish가 대체하는 추세입니다.

---

## J

**JBOD (Just a Bunch of Disks)** — RAID가 없는 storage enclosure입니다. Raw capacity를 제공합니다.

---

## K

**K8s (Kubernetes)** — Container orchestration platform입니다. AI Inference와 Training에 사용하며 GPU Operator로 GPU support를 구성합니다.

**KV cache (Key-Value cache)** — Transformer Inference에서 layer/token별 이미 계산한 key/value tensors를 저장합니다. Context가 길수록 memory 요구가 커집니다.

---

## L

**LoRA (Low-Rank Adaptation)** — Parameter-efficient Fine-tuning 기법입니다. 대부분 parameters를 freeze하고 작은 adapter weights만 학습해 compute 비용을 낮춥니다.

---

## M

**MFU (Model FLOP Utilization)** — 실제 model compute와 theoretical peak GPU FLOPS의 비율입니다. 원본의 좋은 Training run 예시는 40–60%입니다.

**MIG (Multi-Instance GPU)** — Hardware GPU partitioning입니다. A100/H100은 최대 7 instances에 SM/HBM/L2/bandwidth를 분배합니다. 최대 수는 GPU/profile마다 다릅니다.

**ML (Machine Learning)** — 개별 rule을 명시적으로 programming하는 대신 data에서 학습하는 AI의 하위 분야입니다.

**MLOps** — ML에 DevOps 원칙을 적용하여 Training/validation/deployment/monitoring lifecycle을 자동화하는 접근입니다.

**MMR (Meet-Me Room)** — Carrier fiber와 building network를 연결하는 data center의 물리적 공간입니다.

---

## N

**N-S (North-South)** — Data center와 users/internet 사이 traffic입니다. Management, user access, result retrieval 등에 standard Ethernet을 사용합니다.

**NCCL (NVIDIA Collective Communications Library)** — Multi-GPU/multi-node all-reduce/all-gather/broadcast 등의 collective library입니다. Distributed Training에 필수적입니다.

**NDR InfiniBand** — 400 Gbps InfiniBand generation입니다. 원본은 H100/B200 cluster의 standard로 소개합니다.

**NGC (NVIDIA GPU Cloud)** — GPU-optimized containers/pre-trained models/SDKs를 제공하는 NVIDIA catalog입니다. 무료로 접근할 수 있습니다.

**NVLink** — NVIDIA high-speed GPU interconnect입니다. GPU당 bidirectional bandwidth는 NVLink 4 (Hopper) 900 GB/s, NVLink 5 (Blackwell) 1.8 TB/s입니다.

**NVMe (Non-Volatile Memory Express)** — PCIe 기반 SSD protocol입니다. SATA/SAS보다 높은 IOPS/bandwidth를 제공합니다.

**NVMe-oF (NVMe over Fabrics)** — NVMe를 RDMA/TCP network fabric으로 확장합니다. BlueField-3가 processing을 offload할 수 있습니다.

**NVSwitch** — System 내부 fully nonblocking all-to-all NVLink fabric을 구성하는 switch chip입니다. DGX H100은 4× NVSwitch로 8개 GPU를 연결합니다.

---

## O

**OAM (OCP Accelerator Module)** — Hyperscaler platform의 GPU hosting용 server form factor입니다.

**OOB (Out-of-Band)** — Production traffic과 독립된 dedicated management network/traffic입니다.

**OpEx (Operating Expenditure)** — Power/maintenance/licensing/cloud subscription 등 지속적인 운영 비용입니다. Cloud에서 비중이 큽니다.

---

## P

**PCIe (Peripheral Component Interconnect Express)** — GPU/NIC/NVMe를 server에 연결하는 standard interface입니다. 원본 x16 bidirectional 예시는 Gen5 128 GB/s, Gen6 256 GB/s입니다.

**PDU (Power Distribution Unit)** — Rack-level power distribution 장치입니다. Intelligent PDU는 outlet별 power consumption을 monitoring합니다.

**PEFT (Parameter-Efficient Fine-Tuning)** — 일부 parameters만 training하는 large-model Fine-tuning 기법군입니다. LoRA/prefix tuning/adapters를 포함합니다.

**PFC (Priority Flow Control)** — Priority별 pause frame을 보내 packet drop을 줄이는 Ethernet mechanism입니다. 원본의 lossless RoCE 구성에서 사용합니다.

**PUE (Power Usage Effectiveness)** — Total Facility Power / IT Equipment Power입니다. 낮을수록 efficiency가 좋으며 hyperscale target 예시는 1.1–1.2입니다.

---

## R

**RDMA (Remote Direct Memory Access)** — Remote CPU/OS의 data-path 개입 없이 remote memory에 직접 read/write하는 network transfer입니다. Zero-copy/kernel bypass를 제공합니다.

**Redfish** — BMC/server management용 현대 REST/JSON API standard입니다. IPMI를 대체합니다.

**RLHF (Reinforcement Learning from Human Feedback)** — Human rankings로 학습한 reward model을 사용해 LLM을 human preferences에 맞추는 기법입니다.

**RoCE (RDMA over Converged Ethernet)** — Ethernet에서 RDMA semantics를 제공합니다. 원본은 lossless/PFC를 요구합니다. RoCEv1은 L2, RoCEv2는 IP-routable입니다.

---

## S

**SEL (System Event Log)** — Hardware errors/power events/temperature alerts 등을 기록하는 BMC log입니다.

**SM (Streaming Multiprocessor)** — NVIDIA GPU의 기본 compute unit입니다. CUDA Cores/Tensor Cores/cache를 포함합니다. 다수 SM이 병렬 동작합니다. 원본의 수천 SM이라는 표현은 수천 CUDA Cores와 혼동하지 마세요.

**SHARP (Scalable Hierarchical Aggregation and Reduction Protocol)** — InfiniBand switch 내부에서 all-reduce 등의 collective를 수행해 GPU data traffic을 줄입니다.

**Slurm** — Open-source HPC job scheduler입니다. AI Training cluster의 job queue/resource allocation/multi-node scheduling에 사용합니다.

**SXM** — DGX 등의 high-performance GPU module form factor입니다. 높은 NVLink bandwidth/TDP를 지원하며 standard PCIe card와 호환되지 않습니다.

---

## T

**TDP (Thermal Design Power)** — Component의 thermal design 기준 power입니다. Cooling/power 설계에는 해당 TDP와 system overhead를 고려합니다. 순간적인 절대 maximum power와는 구분합니다.

**Tensor Core** — SM의 matrix multiply-accumulate (GEMM) 전용 units입니다. AI의 핵심 accelerator이며 Blackwell의 FP4처럼 precision formats가 추가됩니다.

**TensorRT** — Deep Learning Inference optimization SDK입니다. Layer fusion, precision calibration (FP32→INT8), kernel auto-tuning을 수행합니다.

**TensorRT-LLM** — LLM Inference library입니다. Paged KV cache, continuous batching, in-flight batching을 제공합니다.

**Triton (NVIDIA Triton Inference Server)** — Open-source serving framework입니다. TensorRT/ONNX/PyTorch/TensorFlow backends, HTTP/gRPC, dynamic batching을 지원합니다.

**TTFT (Time to First Token)** — Request 수신부터 첫 generated token까지의 Inference latency입니다. Interactive LLM의 주요 metric입니다.

---

## U

**UPS (Uninterruptible Power Supply)** — Utility failure와 generator startup 사이를 연결하는 battery-backed power supply입니다.

---

## V

**vGPU (Virtual GPU)** — 여러 VM이 GPU를 공유하는 NVIDIA 기술입니다. 원본은 time-slicing을 중심으로 설명하며 MIG-backed 구성도 있습니다. vCS/vWS/vPC 등 edition별 license를 확인합니다.

**VRAM (Video RAM)** — GPU memory/framebuffer입니다. Data center GPU는 제품별로 HBM 또는 GDDR를 사용합니다. Weights와 runtime memory가 들어갈 capacity가 필요합니다.

---

## X

**XDR InfiniBand** — 800 Gbps InfiniBand generation입니다. Quantum-X800에서 사용합니다.

**XID Error** — NVIDIA GPU driver error code입니다. XID 79는 GPU fallen off the bus, XID 48은 double-bit ECC error를 나타냅니다. Code만으로 모든 root cause를 확정하지는 않습니다.
