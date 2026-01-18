# 🛡️ Failure Scenarios & Resilience

> "The Edge is unreliable. Devices disappear."

This document details how EdgeMatrix handles worker disconnects during inference.

## 1. Failure Matrix

| Component | Failure Mode | Impact | Recovery Strategy |
| :--- | :--- | :--- | :--- |
| **Worker Node** | Disconnect (Wifi Loss) | **Job Fail**. Inference halts mid-layer. | **Reschedule**. The Controller detects WebSocket close, marks the shard as "Lost", and re-assigns the job to a standby worker. |
| **WASM Runtime** | Memory Panic (OOM) | **Crash**. Single worker dies. | **Isolation**. Since Bun workers are separate processes, one crash does not kill the Controller. System restarts the worker. |
| **Network** | Slow Link (High Latency) | **Straggler**. One node slows down the whole chain. | **Speculative Exec**. The Controller sends the same shard to 2 nodes. Whoever finishes first wins. |

---

## 2. Deep Dive: The Dead Node Problem

### The Scenario
User requests a 100-token generation.
Node A (Layers 1-10) finishes.
Node B (Layers 11-20) unplugs.
Node C (Layers 21-30) waits forever.

### The Solution: Heartbeats & Timeouts
1.  **Strict Timeouts**: Every "Layer Block" has a max expected duration (e.g., 200ms).
2.  If Node B doesn't ACK within 200ms, Controller assumes death.
3.  **Fast Retransfer**: Controller sends the Input Tensor (cached in RAM) to Node D (Backup).
4.  **Result**: 200ms freeze, then resume.

---

## 3. Resilience Testing

### Test 1: The "Kill -9"
1.  Start a long inference (1000 tokens).
2.  Manually kill one worker process in the terminal.
3.  **Expectation**: The generated text should pause for a split second, then continue (as the backup worker takes over).
