# EdgeMatrix

![Status](https://img.shields.io/badge/Status-Initializing-blue?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Runtime](https://img.shields.io/badge/Runtime-Bun_v1.1-black?style=for-the-badge)

## Distributed Edge AI Runtime

**EdgeMatrix** is a high-performance Distributed Inference Engine built with **Bun** and **WebAssembly (WASM)**. It orchestrates a "Matrix" of lightweight edge nodes (IoT, Browsers, Workers) to execute massive AI models (like Llama 3) by sharding the compute layers, bypassing the need for single heavy GPU instances.

---

## 🚀 Quick Start

Start the cluster locally:

```bash
# 1. Compile Kernels
cd kernels && bun run build:wasm

# 2. Start Controller
bun run start:controller

# 3. Join a Worker
bun run start:worker
```

> **Setup Guide**: See [GETTING_STARTED.md](./docs/GETTING_STARTED.md).

---

## 🏗️ Architecture

### The Sharded Pipeline
![Architecture](docs/assets/thumbnail.png)
*Flow: User -> Controller -> Shard A (Layers 0-10) -> Shard B (Layers 11-20) -> Result*

### The "Zero-Copy" Data Plane
EdgeMatrix uses `SharedArrayBuffer` to move data between the JS Runtime and the WASM Compute Kernel without serialization overhead, enabling near-native C++ performance in a JavaScript environment.

---

## ✨ Key Features

*   **⚡ Zero-Python**: Pure TypeScript/WASM stack. No Conda/Pip hell.
*   **🧅 Layer Sharding**: Split 70B models across 10 low-RAM devices.
*   **🐇 Bun Runtime**: 4ms Cold Start times for Serverless execution.
*   **🔢 INT8 Quantization**: Native support for memory-efficient execution.

---

## 📚 Documentation

| Document | Description |
| :--- | :--- |
| [**System Architecture**](./docs/ARCHITECTURE.md) | WASM Kernels and Model Sharding. |
| [**Getting Started**](./docs/GETTING_STARTED.md) | Bun Setup and Compilation. |
| [**Failure Scenarios**](./docs/FAILURE_SCENARIOS.md) | Handling Node Dropouts. |
| [**Interview Q&A**](./docs/INTERVIEW_QA.md) | "Why Bun?" and "Distributed Latency". |

---

## 🔧 Tech Stack

| Component | Technology | Role |
| :--- | :--- | :--- |
| **Runtime** | **Bun** | High-Perf JS Engine. |
| **Compute** | **WASM (C++)** | Math Kernels (GEMM). |
| **Orchestration** | **TypeScript** | Logic & Networking. |
| **Protocol** | **WebSockets** | Inter-Node Sync. |

---

## 👤 Author

**Harshan Aiyappa**  
Senior Full-Stack Hybrid Engineer  
[GitHub Profile](https://github.com/Kimosabey)

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
