# 🚀 Getting Started with EdgeMatrix

> **Prerequisites**
> *   **Bun v1.1+** (The runtime)
> *   **make/cmake** (For compiling WASM kernels)

## 1. Environment Setup

EdgeMatrix relies on **Bun**. Install it first:
```bash
# Mac/Linux/WSL
curl -fsSL https://bun.sh/install | bash
```

Clone the repo:
```bash
git clone https://github.com/Kimosabey/edge-matrix.git
cd edge-matrix
bun install
```

---

## 2. Compiling the Kernels (WASM)

Before running JS, we must build the C++ math kernels.
```bash
cd kernels
bun run build:wasm
# Output: /dist/core.wasm
```

---

## 3. Running the Runtime

### Mode A: Single Node (Local Testing)
Runs the Control Plane and 1 Worker in the same process.
```bash
bun run dev:local
```

### Mode B: Distributed Cluster
Start the Control Plane:
```bash
bun run start:controller --port 3000
```
Start Workers (in different terminals):
```bash
bun run start:worker --connect ws://localhost:3000
```

---

## 4. Usage Example

Send an Inference Request:
```bash
curl -X POST http://localhost:3000/api/inference \
  -H "Content-Type: application/json" \
  -d '{"model": "llama-3-8b-int8", "prompt": "Explain Quantum Physics"}'
```

*Watch the logs to see the request split across workers!*
