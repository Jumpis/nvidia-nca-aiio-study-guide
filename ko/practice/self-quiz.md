---
layout: korean
title: "Self-Quiz (영어 문제 · 한글 해설)"
parent: "Practice & Glossary (한국어)"
nav_order: 3
grand_parent: 한국어 학습 가이드
permalink: /ko/practice/self-quiz/
lang: ko
---

{% assign original_page = site.pages | where: "path", "practice/self-quiz.md" | first %}
[English original]({{ original_page.url | relative_url }}) · [한국어 홈]({{ '/ko/' | relative_url }})

<div lang="en" markdown="1">

# Self-Quiz — Original Practice Questions

50 original exam-style questions. No dumps. Answers at the bottom.

**Instructions:** Set a 60-minute timer. Read each question fully before answering. Some questions are multiple-select (indicated).

---

## Domain 1 — Essential AI Knowledge

**1.** Which NVIDIA framework is specifically designed for training, fine-tuning, and deploying large language models, automatic speech recognition, and text-to-speech models?

- A) TensorRT
- B) RAPIDS
- C) NeMo
- D) Triton Inference Server

---

**2.** A data scientist wants GPU-accelerated equivalents of pandas and scikit-learn to speed up data preprocessing on an NVIDIA GPU cluster. Which NVIDIA product addresses this?

- A) NCCL
- B) RAPIDS
- C) TensorRT
- D) DOCA

---

**3.** Which statement correctly describes the relationship between AI, ML, and Deep Learning?

- A) ML is a subset of DL; DL is a subset of AI
- B) AI is a subset of ML; ML is a subset of DL
- C) DL is a subset of ML; ML is a subset of AI
- D) All three are independent, parallel fields

---

**4.** What two resources do scaling laws (Chinchilla) indicate primarily determine the quality of a trained language model?

- A) Number of layers and number of attention heads
- B) Compute (FLOPS) and dataset size (tokens)
- C) GPU memory and interconnect bandwidth
- D) Batch size and learning rate

---

**5.** An organization needs to run LLM inference alongside professional visualization (ray tracing, rendering) on a single GPU server. Which GPU family best meets this requirement?

- A) Hopper (H100)
- B) Ada Lovelace (L40S)
- C) Blackwell (B200)
- D) Grace CPU

---

**6.** What is the primary purpose of TensorRT in an AI deployment pipeline?

- A) Training large language models
- B) Collective communications for distributed training
- C) Optimizing trained models for inference on NVIDIA GPUs
- D) Monitoring GPU health in production

---

**7.** Which statement about NVIDIA NGC is correct?

- A) NGC is a paid cloud service for running AI training jobs
- B) NGC is a free catalog of GPU-optimized containers and pre-trained models
- C) NGC is NVIDIA's BMC management platform
- D) NGC requires an NVIDIA AI Enterprise license

---

**8.** During transformer model training, which operation consumes the most GPU memory?

- A) Storing model weights in FP32
- B) Storing all intermediate activations for backpropagation
- C) Loading training data from storage
- D) Gradient clipping

---

**9.** Which GPU is recommended for Virtual Desktop Infrastructure (VDI) workloads in the NVIDIA data center GPU use-case matrix?

- A) H100
- B) B200
- C) L40S
- D) L4

---

**10.** The NVIDIA B200 GPU introduced which new precision format not present in Hopper?

- A) INT8
- B) FP8
- C) BF16
- D) FP4

---

## Domain 2 — AI Infrastructure

**11.** A 70B parameter model is to be served in FP16 precision. What is the minimum GPU memory required to hold the model weights?

- A) 35 GB
- B) 70 GB
- C) 140 GB
- D) 280 GB

---

**12.** What is the key advantage of scale-up (multi-GPU within one node) compared to scale-out (multi-node)?

- A) Higher fault tolerance
- B) Infinite horizontal scalability
- C) Much higher inter-GPU bandwidth and lower latency (NVLink)
- D) Lower cost per GPU

---

**13.** How many NVSwitch chips does the DGX H100 contain?

- A) 2
- B) 4
- C) 8
- D) 16

---

**14.** What does PUE of 1.15 indicate?

- A) 15% of IT power is wasted on cooling
- B) 15% of total power goes to IT equipment
- C) Total facility power is 15% higher than IT equipment power
- D) The facility is 15% below average efficiency

---

**15.** Which cooling method is required for racks exceeding 40–50 kW power density?

- A) Hot-aisle/cold-aisle containment with CRAC units
- B) Direct Liquid Cooling (DLC)
- C) Rear-door fan walls
- D) Immersion cooling only

---

**16.** What is the fundamental difference between E-W (East-West) and N-S (North-South) traffic in an AI cluster?

- A) E-W is encrypted; N-S is plaintext
- B) E-W is GPU-to-GPU training traffic requiring RDMA; N-S is user/management traffic using standard TCP
- C) E-W uses Ethernet; N-S uses InfiniBand
- D) E-W is higher latency than N-S

---

**17.** NVIDIA states the key measurement for an AI-optimized network is:

- A) Peak bandwidth utilization
- B) 99th percentile packet latency
- C) How long an AI training job takes from start to finish
- D) Total throughput in Gbps

---

**18.** What does RDMA's "kernel bypass" feature provide?

- A) Allows the OS kernel to skip memory validation for faster transfers
- B) Enables data transfer without involving the OS kernel, reducing latency and CPU utilization
- C) Bypasses encryption for lower overhead
- D) Allows GPUs to skip PCIe and use NVLink for network transfers

---

**19.** Which NVIDIA products must work together to enable RoCE adaptive routing and congestion control?

- A) ConnectX-8 SuperNIC + Quantum-X800 switch
- B) BlueField-3 DPU + Spectrum-4 switch
- C) H100 GPU + ConnectX-7 NIC
- D) DOCA + NCCL

---

**20.** InfiniBand NDR provides what data rate per port?

- A) 200 Gbps
- B) 400 Gbps
- C) 800 Gbps
- D) 1,600 Gbps

---

**21.** What is the purpose of NVIDIA SHARP in the Quantum-X800 InfiniBand platform?

- A) Adaptive routing to avoid congested switch ports
- B) Hardware security acceleration for encrypted communications
- C) Performing collective operations (e.g., all-reduce) inside the switch fabric
- D) Compressing packets to increase effective bandwidth

---

**22.** Which storage type is most appropriate for active training datasets on a large GPU cluster with hundreds of concurrent data workers?

- A) Local NVMe (per node)
- B) NFS over 1GbE
- C) Parallel distributed file system (Lustre, WEKA)
- D) Object storage (S3-compatible)

---

**23.** The DPU's "Isolate" function protects the infrastructure by:

- A) Encrypting all data in transit between GPU and CPU
- B) Moving infrastructure control/data plane to a separate DPU domain that operates independently if the host OS is compromised
- C) Creating isolated VLAN segments for each tenant
- D) Partitioning GPU memory into isolated regions

---

**24.** The GB300 NVL72 contains how many Blackwell Ultra GPUs and Grace CPUs?

- A) 8 GPUs, 4 CPUs
- B) 36 GPUs, 72 CPUs
- C) 72 GPUs, 36 CPUs
- D) 128 GPUs, 64 CPUs

---

**25.** What does "co-packaged optics" (CPO) enable in NVIDIA Photonics switches?

- A) Running optical transceivers in a separate switch chassis
- B) Integrating optical components directly in the switch ASIC package, reducing power and enabling higher port density
- C) Connecting CPUs and GPUs via fiber instead of PCIe
- D) Using photonic quantum computing for switching decisions

---

**26.** An AI Factory uses NVLink for scale-up and which technology for scale-out across nodes?

- A) Standard Ethernet
- B) InfiniBand
- C) RoCE
- D) PCIe Gen6

---

**27.** NVIDIA BlueField-3 provides how much networking bandwidth?

- A) 100 GbE
- B) 200 GbE
- C) 400 GbE
- D) 800 GbE

---

**28.** Which two factors determine GPU selection for an AI training workload? *(select two)*

- A) GPU memory (model must fit)
- B) Number of PCIe lanes available
- C) Memory bandwidth
- D) GPU height (1U vs 2U)

---

**29.** What is GPUDirect RDMA's primary benefit?

- A) Connects GPUs across data centers via fiber
- B) Eliminates the GPU→CPU memory copy in the data transfer path, saving copy operations and reducing PCIe transactions
- C) Provides encrypted GPU-to-GPU communication
- D) Enables GPUs to directly access InfiniBand switches without a NIC

---

**30.** What type of data center management does a BMC provide?

- A) In-band management via the production network
- B) Out-of-band management independent of server OS state
- C) Application-level monitoring via SNMP
- D) Container orchestration

---

## Domain 3 — AI Operations

**31.** What does DCGM stand for?

- A) Data Compute GPU Metrics
- B) Data Center GPU Manager
- C) Distributed CUDA GPU Monitor
- D) Device Configuration and GPU Management

---

**32.** In a Kubernetes cluster, what does the NVIDIA Device Plugin provide?

- A) Physical GPU drivers for the host kernel
- B) Exposes NVIDIA GPUs as schedulable Kubernetes resources (nvidia.com/gpu)
- C) Monitors GPU utilization and sends alerts
- D) Configures MIG partitions automatically

---

**33.** What is gang scheduling and why is it required for distributed AI training?

- A) Scheduling jobs in priority order to maximize GPU utilization
- B) Allocating all nodes of a job simultaneously before the job starts, preventing partial allocation deadlocks
- C) Grouping similar jobs together to batch process them
- D) Scheduling GPU operations in parallel within a single node

---

**34.** Which NVIDIA tool automatically deploys the entire GPU software stack on Kubernetes nodes?

- A) DCGM
- B) Triton Inference Server
- C) NVIDIA GPU Operator
- D) NVIDIA Base Command Platform

---

**35.** What is the maximum number of MIG instances on a single H100 80GB GPU?

- A) 4
- B) 7
- C) 8
- D) 16

---

**36.** Which virtualization method provides the strongest isolation between GPU tenants?

- A) GPU passthrough
- B) NVIDIA vGPU
- C) MIG (Multi-Instance GPU)
- D) Docker containers without virtualization

---

**37.** A training job shows GPU Utilization at 95% but Model FLOP Utilization (MFU) is only 20%. What is the most likely root cause?

- A) Network bottleneck causing gradient exchange delays
- B) Many small CUDA kernel launches with high overhead, rather than sustained Tensor Core compute
- C) Insufficient GPU memory
- D) Thermal throttling

---

**38.** The monitoring stack for GPU clusters typically flows in which order?

- A) Grafana → Prometheus → DCGM Exporter → DCGM
- B) DCGM → DCGM Exporter → Prometheus → Grafana
- C) DCGM → Grafana → Prometheus → Alerts
- D) Prometheus → DCGM → Grafana → DCGM Exporter

---

**39.** What ECC error type requires immediate investigation and potentially GPU page retirement?

- A) Single-bit errors (SBE) — correctable
- B) Double-bit errors (DBE) — uncorrectable
- C) Triple-bit errors (TBE)
- D) Parity errors

---

**40.** Which platform is NVIDIA's purpose-built solution for managing DGX clusters, including job scheduling, software lifecycle, and monitoring?

- A) NVIDIA Fleet Command
- B) NVIDIA Base Command Platform
- C) Kubernetes with GPU Operator
- D) Slurm with DCGM

---

**41.** Which scheduler is most appropriate for managing large multi-node distributed training jobs in a research/HPC environment?

- A) Kubernetes (vanilla)
- B) Slurm
- C) Docker Swarm
- D) Apache Mesos

---

**42.** NVIDIA vGPU requires which software license?

- A) No license — vGPU is included with GPU hardware
- B) NVIDIA AI Enterprise
- C) NVIDIA DGX software
- D) CUDA Enterprise License

---

**43.** At what GPU temperature does thermal throttling typically begin?

- A) 60°C
- B) 75°C
- C) 83–87°C
- D) 100°C

---

**44.** What Slurm flag requests 4 GPUs for a training job?

- A) `--gpus=4`
- B) `--gres=gpu:4`
- C) `--resource=gpu:4`
- D) `--accelerator=4`

---

**45.** What is the primary purpose of Run:ai in a GPU cluster?

- A) Replace DCGM for GPU health monitoring
- B) Kubernetes-native GPU scheduling with AI-specific features: quotas, fractions, preemption
- C) Serve inference via REST API
- D) Manage InfiniBand switch configurations

---

## Mixed / Integration Questions

**46.** An organization is deploying a multi-tenant GPU cluster for 20 data scientists. They need each scientist to have a guaranteed, isolated GPU resource allocation with no performance interference between users. The cluster uses H100 GPUs. Which virtualization method is best?

- A) GPU passthrough (20 GPUs for 20 scientists)
- B) vGPU with time-slicing
- C) MIG with 2g.20gb profiles (3 instances per GPU, ~7 GPUs for 20 users with queuing)
- D) Docker containers without virtualization

---

**47.** A training cluster has 99% GPU utilization but the team complains training is slow and gradient all-reduce is taking too long. The cluster uses 100 Gbps Ethernet. What should the infrastructure team investigate?

- A) Increase GPU memory
- B) Upgrade to InfiniBand or higher-bandwidth Ethernet (400+ Gbps)
- C) Add more CPU cores
- D) Increase local NVMe storage

---

**48.** Which statement about NVIDIA-Certified Systems is correct?

- A) They are only available from NVIDIA directly
- B) They validate a best baseline configuration for performance, security, and scale from partner OEMs
- C) They require NVIDIA AI Enterprise license to operate
- D) They are only available in PCIe form factor

---

**49.** A company needs to build an AI cluster that processes regulated healthcare data. The team wants maximum GPU performance for LLM training. Which deployment model is most appropriate?

- A) Public cloud (AWS/Azure/GCP)
- B) On-premises with DGX systems and InfiniBand
- C) Hybrid: training in cloud, data stored on-prem
- D) Edge deployment using Jetson modules

---

**50.** NVIDIA Spectrum-X is described as the "World's First Ethernet Platform for AI." What specific combination of technologies enables near-InfiniBand RDMA performance over standard Ethernet?

- A) PCIe Gen6 + 800 Gbps Ethernet
- B) RoCEv2 + NCCL-optimized adaptive routing + congestion control, requiring BlueField-3 DPU + Spectrum-4 switch
- C) TCP over 100 Gbps Ethernet with hardware offload
- D) InfiniBand protocol encapsulated in Ethernet frames

---

## Answers

| Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|
| 1 | C | 11 | C | 21 | C | 31 | B | 41 | B |
| 2 | B | 12 | C | 22 | C | 32 | B | 42 | B |
| 3 | C | 13 | B | 23 | B | 33 | B | 43 | C |
| 4 | B | 14 | C | 24 | C | 34 | C | 44 | B |
| 5 | B | 15 | B | 25 | B | 35 | B | 45 | B |
| 6 | C | 16 | B | 26 | B | 36 | C | 46 | C |
| 7 | B | 17 | C | 27 | C | 37 | B | 47 | B |
| 8 | B | 18 | B | 28 | A,C | 38 | B | 48 | B |
| 9 | D | 19 | B | 29 | B | 39 | B | 49 | B |
| 10 | D | 20 | B | 30 | B | 40 | B | 50 | B |

---

</div>

## 한글 해설과 번역 주석

1. NeMo는 LLM/ASR/TTS Training과 Fine-tuning framework입니다.
2. RAPIDS의 cuDF/cuML은 pandas/scikit-learn에 대응합니다.
3. DL⊂ML⊂AI입니다. AI 전체가 data learning을 요구하지는 않습니다.
4. Compute와 training tokens 같은 data scale의 관계입니다.
5. L40S는 AI compute와 graphics를 함께 지원합니다.
6. TensorRT는 학습된 model의 Inference optimization을 수행합니다.
7. NGC는 catalog입니다. GPU 실행 시간을 판매하는 cloud service와 구분합니다.
8. 원본 정답은 B입니다. Intermediate activations가 backpropagation에 필요합니다. 다만 실제 최대 memory 항목은 model/sequence/batch/optimizer에 따라 달라 B가 항상 최대라는 일반화는 피하세요.
9. VDI용 권장 제품은 L4입니다.
10. Blackwell의 2nd-gen Transformer Engine은 FP4를 추가했습니다.
11. 70B×2 bytes = 140 GB의 weights입니다. KV cache/runtime overhead는 별도입니다.
12. NVLink/NVSwitch의 높은 bandwidth와 낮은 latency가 Scale-up의 이점입니다.
13. DGX H100은 4× NVSwitch입니다.
14. Facility power = IT power×1.15입니다. Overhead 15%는 IT power 대비이며 전체 power 대비 비율은 다릅니다.
15. 원본은 DLC를 답으로 제시합니다. 실제 cooling capacity는 system/facility design마다 다릅니다.
16. E-W는 tightly-coupled Training communication, N-S는 user/management traffic입니다.
17. End-to-end Training job completion time을 평가합니다.
18. Kernel bypass가 data-path copy/CPU overhead를 줄입니다.
19. 소개된 NVIDIA enhancement는 BlueField-3 + Spectrum-4 구성입니다.
20. NDR는 400 Gbps입니다. HDR 200, XDR 800과 구분합니다.
21. SHARP는 switch 내부 collective reduction을 수행합니다.
22. Parallel FS는 많은 concurrent data readers를 지원합니다.
23. DPU의 독립된 domain이 host compromise 시 infrastructure enforcement를 유지합니다.
24. 72 Blackwell Ultra GPUs + 36 Grace CPUs입니다.
25. CPO는 optical components를 ASIC package에 통합해 electrical path/power를 줄입니다.
26. 원본 AI Factory mapping은 scale-up NVLink, scale-out InfiniBand입니다.
27. BlueField-3는 400 GbE입니다.
28. 원본 정답은 A,C입니다. Model fit과 memory bandwidth가 중요합니다. 실제 선택에서는 FLOPS/interconnect/power도 고려합니다.
29. CPU memory staging copy를 제거해 NIC↔GPU를 직접 연결합니다.
30. BMC는 OS와 독립적인 OOB management를 제공합니다.
31. Data Center GPU Manager입니다.
32. Device Plugin은 GPU를 resource로 노출합니다. Driver 설치와 MIG 구성은 별도 component 역할입니다.
33. 모든 resource를 동시에 배정해 부분 할당에 따른 대기를 줄입니다.
34. GPU Operator가 stack 설치/관리를 자동화합니다.
35. H100 80GB는 최대 7 MIG instances입니다. Profile별 가능한 수는 다릅니다.
36. 원본 정답은 C입니다. **한 physical GPU를 여러 tenant가 공유하면서 hardware isolation**한다는 조건이면 MIG입니다. 질문에는 공유 조건이 명시되지 않아, 전체 GPU를 한 VM에 dedicated하는 A (passthrough)도 강한 isolation을 제공합니다. 원본 문항의 모호함입니다.
37. GPU utilization은 kernel 실행 시간이지 Tensor Core 효율이 아닙니다. 작은 kernel overhead가 MFU를 낮출 수 있습니다.
38. DCGM→Exporter→Prometheus→Grafana입니다.
39. DBE는 uncorrectable이므로 즉시 조사하고 device별 recovery 절차를 적용합니다.
40. Base Command Platform의 DGX management 역할입니다.
41. Slurm은 HPC/multi-node batch/MPI Training에 적합합니다.
42. 원본 정답은 B이며 vGPU for Compute/enterprise AI 맥락입니다. vPC/vWS 등 모든 NVIDIA vGPU edition이 같은 AI Enterprise license를 요구한다는 뜻은 아닙니다.
43. 원본 예시는 83–87°C입니다. 실제 threshold는 GPU별로 확인합니다.
44. **원본 정답은 B지만 A도 지원되는 Slurm option입니다.** `--gres=gpu:4`는 node별 GRES request, `--gpus=4`는 지원 configuration에서 job 전체 GPU request입니다. Single-node에서는 둘 다 4 GPUs를 요청할 수 있어 단일 정답 문항으로 모호합니다. [Slurm sbatch](https://slurm.schedmd.com/sbatch.html)를 확인하세요.
45. Run:ai는 GPU-specific scheduling/quota/fractions/preemption을 추가합니다.
46. 원본 정답은 C입니다. H100의 2g.20gb는 최대 3개이므로 7 GPUs면 최대 21 instances로 20명을 배정할 수 있습니다. 각 user에 full GPU가 필요 없다는 효율성 조건을 전제한 답이며, A도 dedicated isolation 자체는 제공합니다.
47. All-reduce communication이 병목인지 측정하고 bandwidth/fabric을 개선합니다. GPU utilization만으로 bottleneck을 확정하지 않습니다.
48. Partner OEM system의 NVIDIA baseline validation입니다.
49. 원본 정답은 B입니다. Data control과 dedicated performance를 중시하는 scenario입니다. 규제 data라는 이유만으로 모든 cloud가 불가능하다는 뜻은 아니며 실제 선택은 compliance 요구에 따릅니다.
50. RoCEv2 + adaptive routing + congestion control의 통합 구성입니다.

<div lang="en" markdown="1">

## Score interpretation

| Score | Readiness |
|---|---|
| 45–50 | Exam ready — schedule it |
| 40–44 | Nearly ready — review weak domains |
| 35–39 | Good foundation — 1 more week of study |
| < 35 | Focus on Domain 2 (networking + DPU) then retake |

</div>
