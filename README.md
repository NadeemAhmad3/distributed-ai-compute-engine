<a name="readme-top"></a>

<div align="center">
  <h1 align="center" style="border-bottom: none">⚡ Distributed AI Compute Engine & Model Serving Platform</h1>
  <p align="center">
    <strong>Unified Distributed Computing Framework for Scaling Machine Learning Training, Ray Serve Model Orchestration, and Cluster Execution.</strong>
  </p>
</div>

<div align="center">
  <a href="https://github.com/NadeemAhmad3"><img src="https://img.shields.io/badge/Author-Nadeem%20Ahmad-blue?style=for-the-badge&logo=github" alt="Author"></a>
  <a href="mailto:engrnadeem26@gmail.com"><img src="https://img.shields.io/badge/Email-engrnadeem26%40gmail.com-red?style=for-the-badge&logo=gmail" alt="Email"></a>
  <a href="https://github.com/NadeemAhmad3/distributed-ai-compute-engine"><img src="https://img.shields.io/badge/Status-Production%20Ready-brightgreen?style=for-the-badge" alt="Status"></a>
</div>

<hr>

## 📌 Executive Overview

**Distributed AI Compute Engine** is an enterprise-grade distributed execution framework designed to scale Python applications and Machine Learning workloads from a single developer laptop to multi-node, multi-GPU clusters.

It abstracts distributed systems complexity into three core primitives: **Tasks** (stateless distributed functions), **Actors** (stateful worker processes maintaining model state in VRAM), and **Objects** (a zero-copy shared memory object store).

```text
               ┌────────────────────────────────────────────────────────┐
               │    Ray Serve & Distributed API Gateway                 │
               └───────────────────────────┬────────────────────────────┘
                                           │
         ┌─────────────────────────────────┼─────────────────────────────────┐
         ▼                                 ▼                                 ▼
┌──────────────────┐             ┌──────────────────┐             ┌──────────────────┐
│ Ray Core Tasks   │             │ Distributed      │             │ Zero-Copy Shared │
│ & Remote Actors  │             │ Train & Tune     │             │ Memory (Plasma)  │
└──────────────────┘             └──────────────────┘             └──────────────────┘
```

---

## 🔥 Key Technical Architecture

### 1. 🤖 Distributed Core Primitives
- **Tasks (`@ray.remote`)**: Asynchronous, stateless functions scheduled across cluster nodes with dynamic load balancing.
- **Actors**: Stateful remote worker processes holding GPU model weights in VRAM across thousands of incoming inference calls.
- **Plasma Object Store**: Zero-copy shared-memory object store allowing high-speed NumPy and PyTorch tensor sharing between local worker processes without IPC serialization overhead.

### 2. 🌐 Ray Serve (Programmable Distributed Model Serving)
- **Multi-Model Orchestration**: Chain multiple LLMs, vision models, and embedding endpoints into unified production pipelines.
- **Dynamic Autoscaling**: Scales worker replicas automatically based on queue depth and GPU memory pressure.
- **Fractional GPU Allocation**: Run multiple model replicas on a single GPU (`num_gpus=0.25`).

### 3. 🚀 Ray Train & Ray Tune (Distributed ML Training & HPO)
- **Distributed Training**: Integrates PyTorch Distributed Data Parallel (DDP), DeepSpeed, and Megatron-LM for multi-node multi-GPU model fine-tuning.
- **Scalable Tuning**: Hyperparameter search engine executing hundreds of parallel trials with early-stopping algorithms (ASHA, PBT).

---

## 📂 Repository Architecture

```text
distributed-ai-compute-engine/
├── python/ray/                # Core Python Engine & Distributed Libraries
│   ├── serve/                 # Ray Serve programmable distributed inference engine
│   ├── train/                 # Ray Train distributed PyTorch / DeepSpeed trainers
│   ├── tune/                  # Ray Tune hyperparameter search engine
│   └── data/                  # Ray Data streaming dataset & shuffle engine
├── src/ray/                   # High-Performance C++ Distributed Core
│   ├── core_worker/           # C++ Core Worker executing tasks & actors
│   ├── object_manager/        # Plasma shared-memory object store & transfer manager
│   ├── gcs/                   # Global Control Store managing cluster state
│   └── raylet/                # Local node daemon managing resource allocation
├── rllib/                     # Scalable Distributed Reinforcement Learning
└── docs/                      # Technical documentation & architecture whitepapers
```

---

## ⚡ Quickstart Guide

### 1. Installation

```bash
pip install ray
```

### 2. Basic Distributed Task & Actor Example

```python
import ray

# Initialize Ray Cluster Runtime
ray.init()

# 1. Stateless Remote Task
@ray.remote
def square(x: int) -> int:
    return x * x

# Run 4 tasks in parallel across cluster
futures = [square.remote(i) for i in range(4)]
results = ray.get(futures)
print("Parallel Task Results:", results)  # [0, 1, 4, 9]

# 2. Stateful Remote Actor (Counter)
@ray.remote
class DistributedCounter:
    def __init__(self):
        self.value = 0

    def increment(self) -> int:
        self.value += 1
        return self.value

counter = DistributedCounter.remote()
print("Counter Value:", ray.get(counter.increment.remote()))  # 1
```

---

## 👤 Maintainer & Author Information

* **Author**: **Nadeem Ahmad**
* **GitHub**: [@NadeemAhmad3](https://github.com/NadeemAhmad3)
* **Email**: [engrnadeem26@gmail.com](mailto:engrnadeem26@gmail.com)
* **Repository**: [https://github.com/NadeemAhmad3/distributed-ai-compute-engine](https://github.com/NadeemAhmad3/distributed-ai-compute-engine)

---

## 📄 License

Distributed under the Apache 2.0 License. See [`LICENSE`](./LICENSE) for full details.
