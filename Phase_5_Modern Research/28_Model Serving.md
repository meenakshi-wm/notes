📘 DAY 28 — Model Serving & Inference Optimization

Goal:

Master LLM inference optimization — understand the prefill/decode phases, KV cache management, batching strategies, and the inference frameworks used in production.

---

## 🍽️ The Big Picture Analogy for Beginners

> ⭐ **Model Serving = A Restaurant** 🍕
>
> Imagine your trained AI model is a **chef** in a kitchen. "Model serving" is everything else — the **waiters** (API layer), the **order queue** (request batching), the **kitchen manager** (inference server like vLLM or Triton), and the **menu** (API endpoints). Without serving, your chef just sits idle!
>
> | Restaurant Part | AI Serving Part |
> |---|---|
> | 🧑‍🍳 Chef | Trained Model |
> | 📝 Taking orders | Receiving API requests |
> | 🪧 Order queue | Request queue / scheduler |
> | 🍣 Sushi conveyor belt | Continuous batching |
> | 🧠 Waiter remembering your past orders | KV Cache |
> | 👔 Restaurant manager | Model server (Triton / vLLM) |

### ⭐ Key Formulas at a Glance

**End-to-end latency** for a single request:

$$L = L_{queue} + L_{preprocess} + L_{inference} + L_{postprocess}$$

**Throughput** (how many requests you can handle):

$$T = \frac{N_{requests}}{t_{wall}}$$

**SLO target**: Production systems track $P99$ latency — the latency that 99% of requests fall under.

---

# 1️⃣ LLM Inference: Two Phases

> 🍽️ **Analogy**: Prefill = the chef **reads the full order** at once. Decode = the chef **plates one dish at a time**, handing each out before making the next.

### Prefill (Prompt Processing)
Process all input tokens in parallel.
- Compute-bound (matrix multiply heavy)
- Runs at high GPU utilization
- Time: proportional to prompt length

### Decode (Token Generation)
Generate tokens one at a time (autoregressive).
- Memory-bandwidth-bound (load full model weights for each token)
- GPU utilization often <30%
- Time: proportional to output length

```
Input: "Explain quantum computing in simple terms."
       ↓ [Prefill: parallel] → KV cache built
       ↓ [Decode: sequential]
Output: "Quantum" → "computing" → "uses" → "..." (one token at a time)
```

---

# 2️⃣ KV Cache: The Memory Cost

> 🧠 **Analogy**: The KV cache is like a **waiter remembering your previous orders**. Instead of re-asking "what did you order last time?" for every new dish, the waiter keeps notes. More conversation history = bigger notebook!

During decode, we cache Key and Value tensors to avoid recomputation:

$$\text{KV size per token} = 2 \times n_{layers} \times n_{heads} \times d_{head} \times b_{dtype}$$

KV cache size per token = 2 × num_layers × num_heads × head_dim × dtype_bytes

For Llama-2 7B with FP16:
= 2 × 32 × 32 × 128 × 2 = 524 KB per token
For 4096 context: 524KB × 4096 = 2.1 GB per request!

For long contexts (128K), KV cache can exceed model weight memory.

---

# 3️⃣ Continuous Batching

> 🍣 **Analogy**: **Static batching** = a waiter who collects 8 orders, goes to the kitchen, waits for ALL 8 dishes, then serves them — even if 5 are done early, they sit getting cold. **Continuous batching** = a **sushi conveyor belt** 🍣 that's always moving — as soon as one plate is taken, a new one goes on!

### Static Batching (Naive)
Wait for all requests in a batch to finish → wastes GPU on padding.

### Continuous Batching (vLLM, TGI)
Add new requests as slots free up. No waiting!

```
Time →
Req A: [prefill][decode decode decode done]
Req B: [prefill prefill][decode decode decode decode decode done]
Req C:           [prefill][decode decode done]
Req D:                    [prefill][decode decode decode done]
```

GPU is always busy. 2-3x higher throughput than static batching.

> 📊 **Throughput improvement with continuous batching** (Yu et al., 2022 — Orca):
>
> $$T_{continuous} \approx 2\text{–}3 \times T_{static}$$
>
> Because the GPU never idles waiting for the longest request in a batch to finish.

---

# 4️⃣ PagedAttention (vLLM)

> 📦 **Analogy**: Imagine a parking lot where you must reserve a whole row per car, even if the car is tiny — huge waste! **PagedAttention** is like a **valet parking system** 🅿️ that parks cars in any open spot, no wasted space.
>
> 📄 **Paper**: Kwon et al. (2023) — *"Efficient Memory Management for Large Language Model Serving with PagedAttention"* — the paper behind vLLM.

Problem: KV cache memory fragmentation. Each request needs different lengths.

Solution: Page the KV cache like OS virtual memory!

```python
# PagedAttention concept:
# Instead of contiguous KV cache per request,
# allocate fixed-size "pages" (e.g., 16 tokens each)

class PagedKVCache:
    def __init__(self, num_pages, page_size, num_layers, num_heads, head_dim):
        self.page_size = page_size
        # Physical pages: shared pool
        self.k_cache = torch.zeros(num_pages, num_layers, num_heads, page_size, head_dim)
        self.v_cache = torch.zeros(num_pages, num_layers, num_heads, page_size, head_dim)
        self.free_pages = list(range(num_pages))
        
    def allocate_page(self):
        return self.free_pages.pop()
    
    def free_page(self, page_id):
        self.free_pages.append(page_id)

# Each request has a page table: [page_id_0, page_id_1, ...]
# Pages can be non-contiguous in physical memory
# Enables: memory sharing (e.g., shared system prompt), no fragmentation
```

---

# 5️⃣ Inference Optimization Techniques

| Technique | Speedup | Description |
|-----------|---------|-------------|
| Quantization (INT4/INT8) | 2-4x | Smaller model, faster loads |
| KV cache quantization | 2x memory | INT8 KV cache |
| Flash Attention | 2-4x | Memory-efficient attention |
| Speculative decoding | 2-3x | Draft model proposes, main verifies |
| Continuous batching | 2-3x | Better GPU utilization |
| PagedAttention | 2-4x memory | No fragmentation |
| Tensor parallelism | Linear | Split across GPUs |

---

# 6️⃣ Speculative Decoding

> ✏️ **Analogy**: Like a **junior chef** who quickly plates 5 dishes, then the **head chef** checks them all in one glance — approving most and only re-doing the bad ones. Much faster than the head chef doing every dish alone!

Use a small "draft" model to generate candidate tokens, then verify with the large model in parallel:

```python
def speculative_decode(draft_model, target_model, prompt, K=5):
    """Generate K draft tokens, verify in parallel."""
    tokens = prompt
    
    while not done:
        # 1. Draft model generates K candidate tokens (fast)
        draft_tokens = []
        draft_probs = []
        for _ in range(K):
            logits = draft_model(tokens + draft_tokens)
            prob = softmax(logits[-1])
            token = sample(prob)
            draft_tokens.append(token)
            draft_probs.append(prob[token])
        
        # 2. Target model verifies ALL K tokens in ONE forward pass
        target_logits = target_model(tokens + draft_tokens)  # processes K+1 positions
        
        # 3. Accept/reject each token
        accepted = 0
        for i in range(K):
            target_prob = softmax(target_logits[len(tokens) + i])
            if random() < target_prob[draft_tokens[i]] / draft_probs[i]:
                accepted += 1
            else:
                # Resample from adjusted distribution
                break
        
        tokens.extend(draft_tokens[:accepted])
        # Average acceptance: 70-90% if draft model is good
```

---

# 7️⃣ Production Inference Frameworks

> ⭐ **In production today**, the most important serving stacks are:
> - **vLLM** — PagedAttention + continuous batching. The go-to open-source LLM server.
> - **TensorRT-LLM** — NVIDIA's optimized engine. Best raw throughput on NVIDIA GPUs.
> - **Triton Inference Server** — NVIDIA's general model server. Handles multi-model, multi-framework.
> - **TGI (Text Generation Inference)** — HuggingFace's server. Easy integration with HF models.

| Framework | Key Feature | Best For |
|-----------|------------|---------|
| vLLM | PagedAttention, continuous batching | Production LLM serving |
| TGI | HuggingFace integration | Quick deployment |
| TensorRT-LLM | NVIDIA optimized | Maximum throughput |
| llama.cpp | CPU inference, GGUF | Edge, consumer hardware |
| Ollama | Easy local deployment | Development |
| SGLang | Structured generation | Constrained output |

```python
# vLLM example
from vllm import LLM, SamplingParams

llm = LLM(model="meta-llama/Llama-2-7b-chat-hf", tensor_parallel_size=2)
sampling = SamplingParams(temperature=0.7, max_tokens=512)
outputs = llm.generate(["Explain quantum computing"], sampling)
```

---

# 8️⃣ Inference Metrics

> ⭐ **The 3 metrics every ML engineer tracks**:
>
> | Metric | What It Means | Target (LLM chat) |
> |---|---|---|
> | **TTFT** (Time to First Token) | How fast the user sees the first word | < 500 ms |
> | **TPS** (Tokens Per Second) | Decode speed — how fast text streams | 30–80 tok/s |
> | **P99 Latency** | 99th percentile end-to-end latency | Depends on SLO |
>
> $$\text{TTFT} = L_{queue} + L_{prefill}$$
>
> $$\text{TPS}_{decode} = \frac{N_{output\_tokens}}{t_{decode}}$$

### 🔄 Production Concerns: Autoscaling & A/B Testing

- **Autoscaling**: Scale GPU replicas based on request queue depth or P99 latency. Kubernetes HPA or cloud-native (SageMaker, Vertex AI).
- **A/B testing**: Route a % of traffic to a new model version to compare quality/latency before full rollout.
- **Canary deploys**: Gradually shift traffic from old → new model, monitoring error rates.

```python
def measure_inference_performance(model, prompts, max_new_tokens):
    import time
    total_tokens = 0
    
    start = time.perf_counter()
    outputs = model.generate(prompts, max_new_tokens=max_new_tokens)
    elapsed = time.perf_counter() - start
    
    total_tokens = sum(len(o) for o in outputs)
    prompt_tokens = sum(len(p) for p in prompts)
    
    ttft = measure_time_to_first_token()
    
    print(f"TTFT (Time to First Token): {ttft*1000:.0f} ms")
    print(f"Tokens/sec (decode): {(total_tokens - prompt_tokens) / elapsed:.0f}")
    print(f"Total latency: {elapsed:.2f}s")
    print(f"Throughput: {total_tokens / elapsed:.0f} tokens/sec")
```

---

# 📝 Summary

| Concept | Key Insight |
|---------|------------|
| Prefill vs Decode | Compute-bound vs memory-bandwidth-bound |
| KV Cache | ~0.5MB/token/layer, dominates memory for long contexts |
| Continuous Batching | Don't wait for longest request |
| PagedAttention | Virtual memory for KV cache |
| Speculative Decoding | Draft then verify for 2-3x speedup |

---

# 🏁 Cheat Sheet

| Symbol | Formula | Meaning |
|---|---|---|
| $L$ | $L_{queue} + L_{pre} + L_{inf} + L_{post}$ | Total request latency |
| $T$ | $\frac{N_{req}}{t_{wall}}$ | Throughput |
| KV size | $2 n_l n_h d_h b$ per token | Memory cost of caching |
| TTFT | $L_{queue} + L_{prefill}$ | Time to first token |
| TPS | $\frac{N_{out}}{t_{decode}}$ | Decode speed |
| P99 | 99th percentile of $L$ | SLO target |

---

# 📚 Resources

### Books 📖
- **"Designing Machine Learning Systems"** — Chip Huyen (2022). Covers serving, monitoring, and production ML end-to-end.
- **"Machine Learning Engineering"** — Andriy Burkov. Practical deployment & serving patterns.

### Papers 📄
- ⭐ Kwon et al. (2023) — *"Efficient Memory Management for Large Language Model Serving with PagedAttention"*. The foundational paper behind vLLM. [arXiv:2309.06180]
- ⭐ Yu et al. (2022) — *"Orca: A Distributed Serving System for Transformer-Based Generative Models"*. Introduced continuous (iteration-level) batching. [OSDI '22]
- Leviathan et al. (2023) — *"Fast Inference from Transformers via Speculative Decoding"*. [ICML '23]

### Docs & Tools 🔧
- [vLLM Documentation](https://docs.vllm.ai/) — PagedAttention, continuous batching, OpenAI-compatible API
- [TensorRT-LLM Documentation](https://nvidia.github.io/TensorRT-LLM/) — NVIDIA's optimized LLM inference
- [Triton Inference Server](https://github.com/triton-inference-server/server) — Multi-model, multi-framework serving
- [Text Generation Inference (TGI)](https://huggingface.co/docs/text-generation-inference) — HuggingFace's serving solution

---

**Tomorrow**: Networking & Communication for Distributed Systems.
