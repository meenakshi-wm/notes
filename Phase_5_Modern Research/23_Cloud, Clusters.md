📘 DAY 23 — Training Infrastructure: Cloud, Clusters & Cost

> *"The cloud is just someone else's computer — but for AI, it's someone else's $30,000 GPU."* 🌩️

Goal:

Understand the infrastructure landscape for AI training — cloud providers, GPU rental, cluster design, and cost optimization. Essential knowledge for AI founders.

---

## 🔰 Prerequisites & What You'll Learn

**No AI knowledge needed!** This guide assumes you're starting from scratch.

| What You'll Learn | Why It Matters |
|---|---|
| ☁️ Cloud GPU providers | Know where to rent powerful computers for AI |
| 💰 Pricing models | Spend wisely — costs can explode fast |
| 🖥️ Cluster architecture | Understand how multiple GPUs work together |
| 🔧 Job scheduling | Automate and manage training runs |
| 📊 Cost optimization | Save 60-90% on your GPU bills |

### 🎯 The Big Picture: Why Do We Need Cloud GPUs?

Training an AI model (like ChatGPT) requires **massive amounts of computation**. Your laptop GPU can't do it — you need specialized hardware (like NVIDIA H100s) that cost $30,000+ each. Instead of buying them, you **rent them by the hour** from cloud providers.

> 🏠 **Beginner Analogy**: Think of it like renting a car 🚗. You don't buy a Ferrari just to drive to the airport once — you rent one when you need it. Cloud GPUs work the same way: rent powerful hardware only when you need it, return it when you're done, and pay only for what you used.

---

# 1️⃣ GPU Cloud Providers

> ☁️ **What is a Cloud GPU Provider?** A company that owns thousands of powerful GPUs in data centers worldwide and rents them to you by the hour, like a library lending books.

### ⭐ Key Concepts for Beginners

- **GPU** (Graphics Processing Unit): A chip originally designed for video games, now the workhorse of AI training. It can do thousands of math operations simultaneously (called **parallelism**).
- **On-demand**: Pay per hour, start/stop anytime (like a taxi 🚕)
- **Reserved**: Commit to 1-3 years for a discount (like a monthly subway pass 🚇)
- **Spot/Preemptible**: Get unused GPUs at huge discounts, but the provider can take them back anytime (like standby airline tickets ✈️ — cheap but you might get bumped!)
- **Instance**: A virtual computer you rent in the cloud, with a specific number of GPUs, CPUs, and memory attached

| Provider | GPUs | Pricing Model | Best For |
|----------|------|--------------|---------|
| AWS (p4d/p5) | A100, H100 | On-demand, reserved, spot | Enterprise, largest scale |
| GCP (a2/a3) | A100, H100 | On-demand, preemptible | Research, TPU alternative |
| Azure (ND) | A100, H100 | On-demand, reserved | Microsoft ecosystem |
| Lambda Labs | A100, H100 | On-demand | Simple, GPU-focused |
| Together AI | A100, H100 | API + clusters | Training + inference |
| RunPod | A100, RTX | On-demand, serverless | Budget-friendly |
| Vast.ai | Various | Marketplace | Cheapest, less reliable |
| CoreWeave | H100 | Reserved | Large-scale training |

### 🔍 Provider Deep-Dive

| Feature | AWS | GCP | Azure | Lambda Labs |
|---|---|---|---|---|
| 🌍 Global regions | 30+ | 35+ | 60+ | 5 |
| 🛠️ Managed ML service | SageMaker | Vertex AI | Azure ML | None (DIY) |
| 📦 Storage | S3 | GCS | Blob Storage | Local NVMe |
| 🔗 GPU interconnect | EFA (100 Gbps) | GPUDirect (200 Gbps) | InfiniBand (400 Gbps) | NVLink within node |
| 🧑‍💼 Enterprise support | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| 💡 Ease of use | Medium | Medium | Medium | Easy |

> 🏭 **Production Scenario**: A startup fine-tuning a 7B model might start on **Lambda Labs** (simple, no setup overhead), then move to **AWS** or **GCP** when they need managed services (SageMaker/Vertex AI) for production deployment, monitoring, and auto-scaling.

> 📚 **Research Reference**: Meta's OPT-175B (Zhang et al., 2022) was trained on 992 A100 GPUs — a scale only possible with large reserved clusters from providers like AWS or dedicated hardware (Meta's own RSC cluster).

---

# 2️⃣ Cost Comparison (2024 Typical Rates)

> 💡 **Why does this matter?** GPU costs are often the **#1 expense** in AI development. Choosing the right pricing model can save you tens of thousands of dollars.

### ⭐ The Three Pricing Models Explained

| Model | Analogy | Discount | Risk |
|---|---|---|---|
| 🟢 **On-Demand** | Taxi — pay per ride | 0% (base price) | None — always available |
| 🟡 **Reserved** | Monthly subway pass — commit upfront | 30-50% off | Stuck paying even if unused |
| 🔴 **Spot/Preemptible** | Standby airline ticket — cheap but can be bumped | **60-90% off** ✈️ | Can be interrupted anytime! |

> 🎓 **Beginner Analogy**: Spot instances are like standby airline tickets ✈️. You get an amazing deal (60-90% off!), but if the airline needs your seat for a full-fare passenger, you get bumped. For AI training, this means your training job can be stopped at any moment — so you must save your progress frequently (called **checkpointing**).

| GPU | On-Demand ($/hr) | Spot/Preempt ($/hr) | Reserved ($/hr) |
|-----|-------------------|--------------------|-----------------| 
| A100 40GB | $3.00-4.00 | $1.00-1.50 | $1.50-2.00 |
| A100 80GB | $4.00-5.00 | $1.50-2.50 | $2.00-3.00 |
| H100 80GB | $8.00-12.00 | $3.00-5.00 | $4.00-6.00 |
| RTX 4090 | $0.50-0.80 | $0.30-0.50 | N/A |
| 8×H100 node | $80-96 | $24-40 | $32-48 |

### 📐 Key Formulas

**Cost per token** — How much does it cost to process each piece of text?

$$C = \frac{\text{GPU cost/hr} \times \text{Training hours}}{\text{Tokens processed}}$$

> 🔢 **Example**: If you pay \$4/hr for an A100, train for 1,000 hours, and process 2 trillion tokens:
> $C = \frac{4 \times 1000}{2 \times 10^{12}} = 2 \times 10^{-9}$ dollars per token (fractions of a penny!)

**Spot savings formula**:

$$\text{Savings} = 1 - \frac{\text{Spot price}}{\text{On-demand price}} \approx 60\%-90\%$$

> 🏭 **Production Tip**: Always use **spot instances + automatic checkpointing** for training jobs. A 70B model training run costing \$500K on-demand could cost as little as \$50K-\$200K on spot instances — that's **house money** saved! 🏠💰

---

# 3️⃣ Training Cost Estimates

> 🤔 **What determines training cost?** Three things: (1) how big your model is, (2) how much data you train on, and (3) how efficiently your GPUs are used.

### ⭐ The Chinchilla Scaling Law (Simplified)

The total compute needed to train a model is approximately:

$$\text{Total FLOPs} \approx 6 \times N \times D$$

Where:
- $N$ = number of parameters (e.g., 7 billion for Llama-2 7B)
- $D$ = number of training tokens (e.g., 2 trillion)
- $6$ = a constant from the Chinchilla paper (Hoffmann et al., 2022)
- **FLOP** = Floating Point Operation — one tiny math operation (like $3.14 \times 2.71$)

> 🍳 **Beginner Analogy**: Think of FLOPs like individual cooking steps — chopping one onion, stirring once, flipping one pancake. A 7B parameter model trained on 2T tokens needs about $8.4 \times 10^{22}$ of these tiny steps. That's why we need powerful GPUs — they can do trillions of these steps per second!

### 📐 GPU Utilization Formula

$$U = \frac{\text{Active compute time}}{\text{Total wall time}}$$

- $U = 1.0$ (100%) means perfect efficiency (impossible in practice)
- $U = 0.4$ (40%) is typical for large training runs
- Below $U = 0.3$ (30%) means something is wrong — GPUs are waiting around idle 😴

```python
def estimate_training_cost(
    model_params_B,      # Billions of parameters
    tokens_T,            # Trillions of tokens
    gpu_type='H100',     # GPU model
    gpu_utilization=0.4, # MFU (Model FLOPS Utilization)
    cost_per_gpu_hr=4.0  # $/hour
):
    # Chinchilla scaling: 6 * N * D FLOPs
    total_flops = 6 * model_params_B * 1e9 * tokens_T * 1e12
    
    # GPU throughput
    gpu_flops = {'H100': 989e12, 'A100': 312e12}[gpu_type]
    effective_flops = gpu_flops * gpu_utilization
    
    # GPU-hours needed
    gpu_seconds = total_flops / effective_flops
    gpu_hours = gpu_seconds / 3600
    
    cost = gpu_hours * cost_per_gpu_hr
    
    print(f"Model: {model_params_B}B params, {tokens_T}T tokens")
    print(f"Total FLOPs: {total_flops:.2e}")
    print(f"GPU-hours: {gpu_hours:,.0f}")
    print(f"Cost: ${cost:,.0f}")
    print(f"With 8 GPUs: {gpu_hours/8:,.0f} hours = {gpu_hours/8/24:.0f} days")
    return cost

# Examples
estimate_training_cost(7, 2, 'H100')      # Llama-2 7B scale
estimate_training_cost(70, 2, 'H100')     # Llama-2 70B scale
estimate_training_cost(405, 15, 'H100')   # Llama-3 405B scale
```

Approximate costs:
- 7B model, 2T tokens: ~$50K on H100s
- 70B model, 2T tokens: ~$500K
- 405B model, 15T tokens: ~$50M+

### 🧮 Cost Breakdown Visualization

```
Training Cost Breakdown (typical 70B model):
┌───────────────────────────────────────────────┐
│ 💻 GPU Compute         ██████████████████ 75% │
│ 💾 Storage (S3/GCS)    ███                10% │
│ 🌐 Networking          ██                  5% │
│ 👷 Engineering time    ███                10% │
└───────────────────────────────────────────────┘
```

> ⭐ **Key Insight**: GPU compute dominates cost. A 10% improvement in GPU utilization (MFU) can save tens of thousands of dollars on a single training run!

> 📚 **Research Reference**: Hoffmann et al., "Training Compute-Optimal Large Language Models" (Chinchilla paper, 2022) showed that many models were over-parameterized and under-trained — leading to the $6ND$ scaling law used above.

---

# 4️⃣ Cluster Architecture

> 🤔 **What is a cluster?** A group of computers (called **nodes**) connected by super-fast cables, working together on the same AI training job. Think of it as multiple kitchens 🍳🍳🍳 all cooking the same meal — each kitchen handles a portion, and they coordinate to produce the final dish.

### ⭐ Key Networking Concepts for Beginners

| Term | What It Is | Analogy |
|---|---|---|
| **NVLink** | Ultra-fast connection *between GPUs within the same computer* | 🏢 A direct hallway between offices in the same building — super fast! |
| **NVSwitch** | A switch that connects all GPUs in a node via NVLink | 🔀 A central hub in the hallway that lets any office reach any other office instantly |
| **PCIe** | The standard (slower) connection between GPU and CPU | 🚶 Walking through the lobby to get between offices — works but slower |
| **InfiniBand** | Ultra-fast network cable *between separate computers* | 🛤️ A high-speed train between buildings — fast but not as fast as walking down the hall |
| **RoCE** | RDMA over Converged Ethernet — a cheaper alternative to InfiniBand | 🚌 A bus between buildings — cheaper than the train, almost as fast |

> 🏢 **Beginner Analogy: NVLink vs PCIe**: Imagine two coworkers. With **NVLink**, they sit in adjacent offices with a direct door between them — they can pass papers back and forth instantly (900 GB/s). With **PCIe**, they're in different wings of the building and have to walk through the lobby (64 GB/s). NVLink is ~14× faster!

### 📐 Network Bandwidth Formula

How much network bandwidth do you need between nodes?

$$BW \geq \frac{2 \times \text{Model size} \times N_{\text{GPUs}}}{\text{Allreduce time budget}}$$

> 🔢 **Example**: For a 70B parameter model (each parameter = 2 bytes in fp16 = 140 GB), with 32 GPUs and a 1-second allreduce budget:
> $BW \geq \frac{2 \times 140\text{GB} \times 32}{1\text{s}} = 8,960 \text{ GB/s}$ — this is why InfiniBand at 400 Gbps (50 GB/s) per link with many parallel links is essential!

```
Internet
    ↓
┌─────────────────────────────┐
│ Head Node / Scheduler       │
│ (SLURM / Kubernetes)        │
└──────────┬──────────────────┘
           │ InfiniBand / RoCE (400 Gbps)
    ┌──────┼──────┐
    │      │      │
┌───┴──┐┌──┴───┐┌─┴────┐
│Node 0││Node 1││Node 2│  ...
│8×H100││8×H100││8×H100│
│NVLink││NVLink││NVLink│
│(intra)│(intra)│(intra)│
└──────┘└──────┘└──────┘
```

Intra-node: NVLink (900 GB/s between GPUs), fast
Inter-node: InfiniBand (400 Gbps = 50 GB/s), slower

### 📊 Bandwidth Comparison Table

| Connection | Bandwidth | Latency | Where Used |
|---|---|---|---|
| NVLink 4.0 (H100) | 900 GB/s | ~1 μs | GPU↔GPU within node |
| NVSwitch | 900 GB/s (all-to-all) | ~1 μs | Full mesh within node |
| PCIe Gen5 | 64 GB/s | ~1 μs | GPU↔CPU |
| InfiniBand NDR | 400 Gbps (50 GB/s) | ~1 μs | Node↔Node |
| Ethernet 100GbE | 100 Gbps (12.5 GB/s) | ~10 μs | General networking |

Training communication patterns:
- AllReduce for DDP: proportional to model size
- AllGather for FSDP: proportional to largest layer
- Point-to-point for pipeline parallel

### 🗄️ Cloud Storage Systems

Your training data and model checkpoints need to live somewhere. Cloud providers offer specialized storage:

| Storage | Provider | Best For | Cost (approx) |
|---|---|---|---|
| **S3** | AWS | Training data, checkpoints, logs | $0.023/GB/month |
| **GCS** | Google Cloud | Same as S3 for GCP users | $0.020/GB/month |
| **Azure Blob** | Microsoft | Same for Azure users | $0.018/GB/month |
| **Local NVMe** | All | Fast scratch space during training | Included with instance |
| **EFS/Filestore** | AWS/GCP | Shared filesystem across nodes | $0.30/GB/month |

> 💡 **Tip**: Store training data on object storage (S3/GCS) for durability, but copy it to local NVMe SSD at the start of training for speed. Checkpoints go back to S3/GCS for safety.

> 📚 **Research Reference**: NVIDIA's DGX SuperPOD architecture connects 32+ DGX H100 nodes (256+ GPUs) via InfiniBand in a fat-tree topology, achieving ~1.6 TB/s bisection bandwidth — purpose-built for training frontier models.

---

# 5️⃣ Cost Optimization Strategies

> 💸 **This section can save you thousands of dollars.** Cost optimization is the difference between a startup that runs out of money and one that ships a product.

### ⭐ The Golden Rules of GPU Cost Optimization

1. **Never leave GPUs idle** — if utilization < 30%, you're burning money 🔥
2. **Right-size your GPU** — don't use a \$8/hr H100 when a \$0.50/hr RTX 4090 works
3. **Use spot instances** — save 60-90% with proper checkpointing
4. **Monitor everything** — you can't optimize what you don't measure
5. **Consider managed services** — SageMaker/Vertex AI handle the ops so you focus on ML

> 🏭 **Production Scenario**: A real startup was spending \$40K/month on on-demand A100s for fine-tuning. By switching to spot instances with 10-minute checkpointing and using RTX 4090s for LoRA fine-tuning, they cut costs to \$8K/month — an 80% savings! 📉

### 🎯 Right-Sizing Your GPU

| Task | Right GPU | Wrong GPU | Why |
|---|---|---|---|
| 🧪 Prototyping/debugging | RTX 4090 (\$0.50/hr) | H100 (\$8/hr) | Don't need power for small experiments |
| 🔧 Fine-tuning 7B with LoRA | 1× RTX 4090 | 1× H100 | LoRA only updates ~1% of parameters |
| 🔧 Full fine-tuning 7B | 1× A100 80GB | 8× H100s | Model fits in one GPU's memory |
| 🚀 Pretraining 70B | 8× H100 cluster | A100s | H100 is 3× faster, saves wall-clock time |
| 🤖 Inference (serving users) | T4 or L4 (quantized) | A100 | Inference is memory-bound, not compute-bound |

### 📐 Managed Services vs DIY

| Factor | Managed (SageMaker/Vertex AI) | DIY (Raw GPU instances) |
|---|---|---|
| 💰 Cost | 20-40% markup | Base GPU price |
| ⏱️ Setup time | Minutes | Hours to days |
| 🔧 Maintenance | Provider handles it | You handle everything |
| 📊 Monitoring | Built-in dashboards | Set up Prometheus/Grafana |
| 🔄 Auto-scaling | Built-in | Manual or custom scripts |
| ⭐ Best for | Teams < 5 ML engineers | Teams with dedicated MLOps |

```python
# 1. Spot/Preemptible instances (2-4x cheaper, but can be interrupted)
# Requires: robust checkpointing every 5-10 minutes

def save_checkpoint(model, optimizer, scheduler, epoch, step, path):
    torch.save({
        'model': model.state_dict(),
        'optimizer': optimizer.state_dict(),
        'scheduler': scheduler.state_dict(),
        'epoch': epoch,
        'step': step,
        'rng_state': torch.cuda.get_rng_state(),
    }, path)

# 2. Use the RIGHT GPU for the task
# Fine-tuning 7B with LoRA? → 1× RTX 4090 ($0.50/hr) not 1× H100 ($8/hr)
# Pretraining 70B? → H100 cluster (only option)
# Inference? → Quantized model on smaller GPU

# 3. Optimize MFU (Model FLOPS Utilization)
# Target: 40-55% MFU for training
# Below 30%? → memory-bound, need better batching/optimization
```

> 📚 **Research Reference**: Google's "Efficiently Scaling Transformer Inference" (Pope et al., 2022) showed that careful hardware selection and batching strategies can improve inference throughput by 2-3× at the same cost — hardware choice matters enormously.

---

# 6️⃣ Job Scheduling with SLURM

> 🤔 **What is SLURM?** A job scheduler — software that manages who gets to use which GPUs and when. Think of it as a **restaurant reservation system** 🍽️: you submit your order (job), specify how many tables (GPUs) you need and for how long, and SLURM seats you when resources are available.

### ⭐ Key Scheduling Concepts

| Concept | What It Means | Analogy |
|---|---|---|
| **Job** | A training run you submit | An order at a restaurant 🍽️ |
| **Node** | One physical computer with GPUs | One kitchen |
| **Partition** | A group of nodes (e.g., "gpu", "cpu") | Different sections of the restaurant |
| **Queue** | Jobs waiting for resources | The waiting list |
| **Preemption** | Higher-priority jobs kick out lower ones | VIP guests bumping regular reservations |

### 🐳 Kubernetes for ML

**Kubernetes** (K8s) is another way to manage GPU clusters, especially popular in cloud environments.

> ✈️ **Beginner Analogy**: If SLURM is a restaurant reservation system, **Kubernetes is airport traffic control** 🛫. It manages hundreds of "containers" (packaged applications), decides which runway (GPU) each plane (training job) uses, handles failures (rerouting if a runway closes), and scales up/down automatically.

| Feature | SLURM | Kubernetes |
|---|---|---|
| 🏠 Environment | On-premise clusters, HPC | Cloud-native, any environment |
| 📦 Packaging | Modules, conda envs | Docker containers |
| 🔄 Auto-scaling | No (fixed cluster size) | Yes (add/remove nodes) |
| 🔧 Complexity | Moderate | High (steep learning curve) |
| ⭐ Best for | Research labs, fixed clusters | Production, cloud deployments |
| 🛠️ ML tools | Basic | Kubeflow, Ray, Volcano |

```bash
#!/bin/bash
#SBATCH --job-name=train_llm
#SBATCH --nodes=4
#SBATCH --gpus-per-node=8
#SBATCH --time=72:00:00
#SBATCH --partition=gpu

# Setup
export MASTER_ADDR=$(scontrol show hostnames $SLURM_JOB_NODELIST | head -n 1)
export MASTER_PORT=12355

# Launch distributed training
srun torchrun \
    --nnodes=$SLURM_NNODES \
    --nproc_per_node=8 \
    --rdzv_id=$SLURM_JOB_ID \
    --rdzv_backend=c10d \
    --rdzv_endpoint=$MASTER_ADDR:$MASTER_PORT \
    train.py --config config.yaml
```

### 🔍 Line-by-Line Explanation for Beginners

| Line | What It Does |
|---|---|
| `#SBATCH --job-name=train_llm` | Names your job (like labeling a package 📦) |
| `#SBATCH --nodes=4` | Requests 4 computers |
| `#SBATCH --gpus-per-node=8` | 8 GPUs per computer = 32 total GPUs |
| `#SBATCH --time=72:00:00` | Maximum 72 hours before auto-kill ⏰ |
| `#SBATCH --partition=gpu` | Use the GPU partition (not the CPU one) |
| `MASTER_ADDR=...` | Finds which node is the "leader" for coordination |
| `srun torchrun` | Launches the training script on all nodes simultaneously |
| `--nnodes=$SLURM_NNODES` | Tells PyTorch how many nodes to expect |
| `--nproc_per_node=8` | 8 processes per node (one per GPU) |

---

# 7️⃣ Founder's Infrastructure Decision Tree

> 🚀 **Use this decision tree to decide where to train your models.** Start at the top and follow the branches based on your monthly budget.

```
Budget < $1K/month?
├─ Yes → Consumer GPU (RTX 4090) or RunPod spot
│        Fine-tune with QLoRA, small experiments only
└─ No
   Budget < $10K/month?
   ├─ Yes → Lambda Labs / RunPod (1-2 A100s)
   │        Fine-tune full models, small pretraining
   └─ No
      Budget < $100K/month?
      ├─ Yes → Reserved A100/H100 cluster (8-32 GPUs)
      │        Pretrain 7B models, serious fine-tuning
      └─ No → CoreWeave / AWS reserved (64+ H100s)
              Pretrain 70B+ models
```

### ⭐ Quick Decision Matrix

| If You're... | Do This | Don't Do This |
|---|---|---|
| 🎓 Learning/experimenting | Free tiers (Colab, Kaggle) or RTX 4090 | Renting H100 clusters |
| 🔧 Fine-tuning a model | 1× A100 on Lambda/RunPod | Building your own cluster |
| 🚀 Training a 7B model | 8× H100 reserved instance | Using spot without checkpointing |
| 🏢 Training a 70B+ model | 64+ H100 reserved cluster | Trying to do it on A100s (3× slower) |
| 🤖 Serving inference to users | T4/L4 with quantized model | Using training GPUs for serving |

---

# 📝 Summary

| Decision | Key Factor |
|----------|-----------|
| GPU choice | H100 for pretraining, A100 for fine-tuning, RTX for dev |
| Pricing | Spot (cheapest), Reserved (reliable), On-demand (flexible) |
| Cost driver | GPU-hours = FLOPs / (GPU speed × MFU) |
| Reliability | Checkpoint frequently, handle preemption |
| Scaling | NVLink intra-node, InfiniBand inter-node |

---

# 🧾 Cheat Sheet: Cloud & Clusters at a Glance

| Concept | One-Liner | Formula / Key Number |
|---|---|---|
| ☁️ Cloud GPU | Rent powerful GPUs by the hour | A100 ~\$3-5/hr, H100 ~\$8-12/hr |
| ✈️ Spot instance | Cheap but interruptible GPU rental | 60-90% off on-demand |
| 💰 Cost per token | $C = \frac{\text{GPU cost/hr} \times \text{Hours}}{\text{Tokens}}$ | ~$10^{-9}$ for large runs |
| 🧮 Total FLOPs | Compute needed for training | $6 \times N \times D$ |
| 📊 GPU utilization | How busy your GPUs are | $U = \frac{\text{Active time}}{\text{Wall time}}$, target 40-55% |
| 🔗 NVLink | GPU↔GPU within node | 900 GB/s |
| 🌐 InfiniBand | Node↔Node networking | 400 Gbps (50 GB/s) |
| 🔀 NVSwitch | Full-mesh GPU interconnect | All-to-all within node |
| 🐳 Kubernetes | Container orchestration for ML | Auto-scaling, cloud-native |
| 📋 SLURM | HPC job scheduler | `#SBATCH` directives |
| 💾 Checkpointing | Save training progress frequently | Every 5-10 min on spot |
| 🎯 Right-sizing | Match GPU to task | Don't use H100 for LoRA fine-tuning |
| 🏢 Managed services | SageMaker / Vertex AI | 20-40% markup, saves engineering time |
| 🌐 BW needed | $BW \geq \frac{2 \times \text{Model size} \times N_{\text{GPUs}}}{\text{Allreduce budget}}$ | Scales with model size |

---

# 🗺️ Beginner's Analogy Map

| Technical Concept | Real-World Analogy |
|---|---|
| ☁️ Cloud GPU rental | 🚗 Renting a car — pay by the hour, return when done |
| ✈️ Spot/preemptible instances | ✈️ Standby airline tickets — cheap but can be bumped |
| 🐳 Kubernetes | 🛫 Airport traffic control — manages containers/arrivals |
| 🍳 Multi-node training | 🍳 Multiple kitchens cooking the same meal together |
| 🔗 NVLink vs PCIe | 🏢 Direct hallway between offices vs going through the lobby |
| 📋 SLURM scheduler | 🍽️ Restaurant reservation system — manages who uses what GPU when |
| 💾 Checkpointing | 💾 Auto-saving a video game — so you don't lose progress if power goes out |
| 📦 S3/GCS storage | 🏦 A bank vault — safe, durable, but not instant access |
| 🖥️ Local NVMe | 🎒 Your backpack — fast access, limited space |

---

# 📚 Resources

### 📖 Books
| Book | Author(s) | Why Read It |
|---|---|---|
| *Cloud Computing: Theory and Practice* | Dan C. Marinescu | Comprehensive cloud fundamentals |
| *Designing Data-Intensive Applications* | Martin Kleppmann | Distributed systems essentials |
| *Kubernetes in Action* | Marko Lukša | Deep dive into K8s for production |
| *High Performance Computing* | Thomas Sterling et al. | HPC cluster design and management |
| *Programming Massively Parallel Processors* | Hwu, Kirk, El Hajj | GPU architecture internals |

### 📄 Key Papers
| Paper | Year | Key Contribution |
|---|---|---|
| Hoffmann et al., "Training Compute-Optimal Large Language Models" (Chinchilla) | 2022 | The $6ND$ scaling law for training compute |
| Pope et al., "Efficiently Scaling Transformer Inference" (Google) | 2022 | Hardware-aware inference optimization |
| Zhang et al., "OPT: Open Pre-trained Transformer Language Models" (Meta) | 2022 | Detailed 175B training infrastructure log — invaluable for understanding large-scale clusters |
| Shoeybi et al., "Megatron-LM: Training Multi-Billion Parameter Language Models" (NVIDIA) | 2020 | Efficient model parallelism across GPU clusters |
| Rajbhandari et al., "ZeRO: Memory Optimizations Toward Training Trillion Parameter Models" (Microsoft) | 2020 | DeepSpeed ZeRO — training huge models with less memory |

### 🔗 Online Resources
| Resource | Link / Where to Find | What It Covers |
|---|---|---|
| AWS EC2 GPU Instances | AWS docs → "Accelerated Computing" | p4d, p5 instance specs, pricing |
| GCP GPU VMs | Google Cloud docs → "GPUs on Compute Engine" | A2, A3 instances, preemptible pricing |
| Azure ND-series | Azure docs → "N-series" | ND A100 v4, NDm H100 v5 specs |
| NVIDIA DGX SuperPOD Docs | NVIDIA developer → "DGX SuperPOD" | Reference cluster architecture |
| Lambda Labs GPU Benchmark | lambdalabs.com/gpu-benchmarks | Real-world GPU performance comparisons |
| SLURM Documentation | slurm.schedmd.com | Complete job scheduler reference |
| Kubeflow | kubeflow.org | ML toolkit for Kubernetes |
| Together AI Cluster Guide | together.ai/docs | Cloud cluster setup for training |

### 🛠️ Tools to Know
| Tool | Purpose |
|---|---|
| `nvidia-smi` | Monitor GPU utilization, memory, temperature |
| `nvitop` | Beautiful GPU monitoring dashboard |
| Weights & Biases (W&B) | Track training metrics, GPU utilization, costs |
| Prometheus + Grafana | Production monitoring stack |
| `sky` (SkyPilot) | Automatically find cheapest GPU across clouds |
| `kubectl` | Kubernetes command-line tool |

---

**Tomorrow**: Profiling & Debugging GPU Training.
