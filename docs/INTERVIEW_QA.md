# 🎤 Interview Cheat Sheet: EdgeMatrix

## 1. The Elevator Pitch (2 Minutes)

"EdgeMatrix is a **Distributed AI Runtime** built on **Bun** and **WebAssembly**.
It solves the problem of running massive AI models on small devices by 'Sharding' the model across a cluster of lightweight workers.
Instead of needing one $10,000 GPU, I can link 10 MacBooks or Raspberry Pis together to run Llama-70B.
I used **Bun** for its ultra-fast startup and **WASM** for safe, near-native execution of the matrix multiplication kernels."

---

## 2. "Explain Like I'm 5" (The Assemble Line)

"Imagine building a car (The AI Result).
*   **Monolithic AI**: One giant factory does everything. If you don't have a giant factory, you can't build cars.
*   **EdgeMatrix**: Is an assembly line of small garages.
*   **Garage A**: Puts on the wheels.
*   **Garage B**: Installs the engine.
*   **Garage C**: Paints the car.
*   Combined, these small garages can build a Ferrari, even though none of them are big enough to hold the whole assembly line."

---

## 3. Tough Technical Questions

### Q: Why Bun? Why not Node.js?
**A:** "For Serverless/Edge, **Startup Time** is the #1 metric.
Node.js (V8) takes ~100ms to boot a context. Bun (Zig/JSC) takes ~4ms.
When I am spinning up ephemeral workers on demand to handle a spike, that 96ms difference is huge.
Also, Bun has a faster FFI (Foreign Function Interface) for calling my C++ kernels."

### Q: Isn't Distributed Inference slow due to Network Latency?
**A:** "Yes, Bandwidth is the bottleneck.
Sending 1GB of data over WiFi is slow.
**My Optimization**: I only transmit the **Activations** (small intermediate states), not the Model Weights.
The Weights are downloaded *once* and cached. During inference, we only pass small vectors (KB sized) between nodes. This makes it viable."

### Q: How do you handle quantization?
**A:** "I implemented **INT8 Quantization**.
Standard AI uses Float32 (4 bytes per number).
I compress it to Int8 (1 byte per number).
This reduces memory usage by 4x. I wrote a custom WASM kernel to perform 'Integer-to-Float' dequantization on the fly during the compute step."
