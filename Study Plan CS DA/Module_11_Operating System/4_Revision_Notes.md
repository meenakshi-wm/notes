# Operating Systems — Revision Notes for GATE 2027

> **Last-day crisp revision** — All key concepts, formulas, and traps at a glance.
> **GATE Weightage:** 5–8 marks | **Priority:** Very High

---

## 1. Process Basics

- Process = program in execution = code + data + stack + heap + PCB
- PCB: PID, state, PC, registers, memory info, scheduling info
- **5 states:** New → Ready → Running → Waiting → Terminated
- **Waiting → Running is INVALID** (must go through Ready)

### fork() Rules
- Returns **twice**: child PID to parent, 0 to child, −1 on error
- n sequential fork() → **2ⁿ** total processes
- `fork()` in loop: `for(i=0;i<n;i++) fork()` → **2ⁿ** total
- Parent and child have **separate address spaces** (copy-on-write)
- `exec()` replaces process image; PID stays same

### Threads
- Share: code, data, heap, open files
- Own: stack, registers, PC, thread ID
- **ULT:** Fast switch, blocks entire process, no parallelism
- **KLT:** Slower switch, independent blocking, true parallelism
- **Models:** Many-to-One, One-to-One, Many-to-Many

---

## 2. CPU Scheduling

### Metrics
| Metric | Formula |
|--------|---------|
| TAT | Completion − Arrival |
| WT | TAT − Burst |
| RT | First CPU − Arrival |

### Algorithms Summary
| Algorithm | Preemptive? | Optimal? | Starvation? | Special |
|-----------|------------|----------|-------------|---------|
| FCFS | No | No | No | Convoy effect |
| SJF | No | Yes (non-preemptive) | Yes | Needs burst prediction |
| SRTF | Yes | Yes (overall) | Yes | Preemptive SJF |
| RR | Yes | No | No | TQ → ∞ becomes FCFS |
| Priority | Both | No | Yes | Fix with aging |

### Key Points
- **SJF is optimal** for avg WT among non-preemptive
- **SRTF is optimal** for avg WT among all algorithms
- **RR:** TQ too small → too many context switches; TQ too large → FCFS
- **Convoy effect:** Short processes behind long one (FCFS)
- **Aging:** Increase priority over time → prevents starvation
- If all arrive at t=0: SJF = SRTF (no preemption needed)

### Real-Time Scheduling
| | RM | EDF |
|--|-----|-----|
| Priority | Static (shorter period) | Dynamic (nearest deadline) |
| Bound | $n(2^{1/n}-1)$ → ~0.69 | 1.0 (100%) |
| Test type | Sufficient only | Necessary & sufficient |
| Optimal among | Fixed-priority | All uniprocessor |

- RM n=2: 0.828, n=3: 0.780

---

## 3. Process Synchronization

### Critical Section Requirements
1. **Mutual Exclusion** — max 1 process in CS
2. **Progress** — no unnecessary delay if CS is free
3. **Bounded Waiting** — finite wait time guaranteed

### Peterson's Solution (2-process)
- Uses: flag[2], turn
- Satisfies ALL three requirements
- Only for 2 processes
- Requires memory barrier on modern hardware

### Bakery Algorithm (N-process)
- Each process picks Number[i] = max(all) + 1
- Tie broken by smaller process ID (lexicographic)
- Satisfies all 3 CS requirements for N processes

### Test-and-Set
- Atomic read-modify-write
- Satisfies mutual exclusion
- Does NOT guarantee bounded waiting

### Semaphores
- **wait(S)/P:** if S>0 then S-- else block
- **signal(S)/V:** S++ (may unblock waiting process)
- **Binary:** S ∈ {0,1} (mutex)
- **Counting:** S ∈ ℤ (resource counting)
- Negative value = |S| processes blocked

### Classic Problems

**Producer-Consumer:**
```
mutex=1, empty=N, full=0
Producer: wait(empty)→wait(mutex)→produce→signal(mutex)→signal(full)
Consumer: wait(full)→wait(mutex)→consume→signal(mutex)→signal(empty)
```
⚠️ **Deadlock if mutex acquired before empty/full!**

**Readers-Writers (readers preference):**
- First reader locks writer out (wait rw_mutex)
- Last reader releases writer (signal rw_mutex)
- Writers may **starve**

**Dining Philosophers — Deadlock solutions:**
1. Allow max 4 to sit
2. Odd=left first, Even=right first
3. Acquire both atomically

### Monitors
- One process active at a time (built-in mutual exclusion)
- Condition variables: wait() and signal()

### IPC Quick Reference
| Method | Sync | Direction | Relationship |
|--------|------|-----------|--------------|
| Shared Memory | Manual | N/A | Any |
| Message Passing | Built-in | Uni/Bi | Any |
| Pipe | Built-in | Unidirectional | Parent-child |
| Named Pipe (FIFO) | Built-in | Bidirectional | Any |
| Signal | Async | One-way | Any |

- Blocking send + blocking receive = **rendezvous**
- Pipe: fd[0] read, fd[1] write

---

## 4. Deadlocks

### Four Necessary Conditions (ALL must hold)
1. Mutual Exclusion
2. Hold and Wait
3. No Preemption
4. **Circular Wait** (easiest to prevent → impose ordering)

### Resource Allocation Graph
- Cycle + single instance → **Deadlock guaranteed**
- Cycle + multiple instances → **May or may not deadlock**

### Banker's Algorithm (Safety Check)
```
Work = Available
Repeat: find Pi where Need[i] ≤ Work
  → Work = Work + Allocation[i], mark finished
If all finished → SAFE
```

### Deadlock-free Resource Formula
- n processes, each needs max k of identical resource
- **Minimum m = n(k−1) + 1** guarantees no deadlock

---

## 5. Memory Management

### Address Translation
- Logical: [Page Number | Offset]
- Page bits = ⌈log₂(total pages)⌉
- Offset bits = ⌈log₂(page size)⌉
- **Physical addr = Frame × Page_size + Offset**

### Paging Key Formulas
| What | Formula |
|------|---------|
| # Pages | Virtual space / Page size |
| # Frames | Physical memory / Frame size |
| Page table entries | # Pages |
| PT size | # entries × entry size |
| Entries per page (multi-level) | Page size / PTE size |

### Multi-Level Paging
- n-level paging → **n+1 memory accesses** per address translation (without TLB)
- Address split: [P1 | P2 | ... | Pn | Offset]
- Each Pi has bits = log₂(entries per page table page)

### TLB (Translation Lookaside Buffer)
$$EAT = h \times (t_{TLB} + t_{mem}) + (1-h) \times (t_{TLB} + (n+1) \times t_{mem})$$

- h = hit ratio, n = page table levels
- TLB time appears in BOTH hit and miss cases

### Segmentation
- Logical: (Segment#, Offset)
- Physical = Base[segment] + Offset (if Offset < Limit)
- Offset ≥ Limit → **Segment Fault**

---

## 6. Virtual Memory & Page Replacement

### Demand Paging EAT
$$EAT = (1-p) \times t_{mem} + p \times t_{page\_fault}$$
- p = page fault rate
- Page fault time ≈ 8–10 ms (disk I/O dominated)

### Page Replacement Algorithms
| Algorithm | Method | Belady's? | Optimal? |
|-----------|--------|-----------|----------|
| FIFO | Replace oldest | **YES** | No |
| Optimal | Replace farthest future use | No | Yes |
| LRU | Replace least recently used | No | No (but good) |
| Clock | Second chance with ref bit | No | No |

### Key Facts
- **Belady's Anomaly:** More frames → More faults (only FIFO, non-stack algorithms)
- **LRU is a stack algorithm** → immune to Belady's
- **Optimal** is theoretical benchmark (needs future knowledge)
- **Thrashing:** Demand > Available frames → constant paging
- **Working Set:** Pages referenced in last Δ time units

---

## 7. File Systems

### Allocation Methods
| Method | Sequential | Random | Ext. Fragmentation | Overhead |
|--------|-----------|--------|-------------------|----------|
| Contiguous | Fast | Fast | Yes | None |
| Linked | OK | Slow | No | Pointers |
| Indexed | OK | Fast | No | Index block |

### Unix Inode — Max File Size
Block size B, pointer size P, pointers/block = B/P = K
- Direct: d × B
- Single indirect: K × B
- Double indirect: K² × B
- Triple indirect: K³ × B

**Example (B=4KB, P=4B):** K=1024
Max ≈ 12×4KB + 4MB + 4GB + 4TB

### Free Space Management
- **Bitmap:** 1 bit/block. Size = total_blocks/8 bytes
- **Linked List:** Free blocks chained together

### Directory Structures
- Single → Two-level → Tree → Acyclic graph (links) → General graph
- Hard link: points to inode directly. Survives deletion. Can't cross FS.
- Soft link: stores path. Dangles if original deleted. Can cross FS.

### Disk Scheduling
| Algorithm | Key feature |
|-----------|------------|
| FCFS | Fair, high seek time |
| SSTF | Nearest first, starvation possible |
| SCAN | Elevator, fair |
| C-SCAN | One direction only, uniform wait |
| LOOK/C-LOOK | Like SCAN/C-SCAN but reverse at last request (not disk end) |

### Protection & Security
- ACL: per object (Unix rwx). Revocation easy.
- Capability: per subject. Revocation hard.
- chmod 755 = rwxr-xr-x. setuid: run as file owner.

---

## 8. Critical Traps & GATE Mistakes

| # | Trap | Reality |
|---|------|---------|
| 1 | Waiting → Running | INVALID. Must go through Ready |
| 2 | fork() in if-else | Draw process tree — trace each path |
| 3 | Producer-Consumer order | empty/full BEFORE mutex (not after!) |
| 4 | Cycle in RAG = deadlock | Only for single-instance resources |
| 5 | Belady's in LRU | NO — only FIFO suffers Belady's |
| 6 | TLB miss cost | TLB_time + (n+1)×mem_time, not just n+1 |
| 7 | Banker's: compare Need, not Allocation | Need = Max − Allocation |
| 8 | Page table size | entries × entry_size, not entries alone |
| 9 | Semaphore negative value | |value| = number of blocked processes |
| 10 | SRTF starvation | Yes — long processes can starve! |
| 11 | Internal vs External fragmentation | Internal=paging, External=segmentation/variable partition |
| 12 | File block calc (linked) | Usable = block_size − pointer_size per block |

---

## 9. Quick Number Crunching

- 2¹⁰ = 1024 (1 KB)
- 2²⁰ = 1,048,576 (1 MB)
- 2³⁰ = 1 GB, 2⁴⁰ = 1 TB
- log₂(1024) = 10
- 4 KB = 2¹², 8 KB = 2¹³, 16 KB = 2¹⁴

---

*Quick revision complete — Operating Systems for GATE 2027* ⚡
