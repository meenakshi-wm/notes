# Computer Organization & Architecture — Revision Notes for GATE 2027

> **Quick-reference sheet** | **GATE Weightage:** 4–6 marks

---

## 1. Key Formulas

| Formula | Expression |
|---------|-----------|
| Memory Size | 2^(address bits) × addressability |
| CPU Time | IC × CPI × T_clock = IC × CPI / f |
| Average CPI | Σ(CPIᵢ × Fractionᵢ) |
| MIPS | f / (CPI × 10⁶) |
| Speedup | T_old / T_new |
| **Amdahl's Law** | **1 / ((1−f) + f/S)** — max = 1/(1−f) |
| **Gustafson's Law** | **s + p×N** (scaled speedup) |
| Pipeline Speedup | nk / (k + n − 1) → k (for large n) |
| Pipeline Time | (k + n − 1) × τ |
| Pipeline CPI | 1 + stall_cycles_per_instruction |
| AMAT | Hit_time + Miss_rate × Miss_penalty |
| Effective CPI | Base_CPI + mem_access/instr × miss_rate × penalty |

---

## 2. IEEE 754 Floating Point

| | Single | Double |
|--|--------|--------|
| Total bits | 32 | 64 |
| Sign | 1 | 1 |
| Exponent | 8 | 11 |
| Mantissa | 23 | 52 |
| Bias | 127 | 1023 |

**Value = (−1)ˢ × 1.M × 2^(E−Bias)** (normalized)

| E | M | Meaning |
|---|---|---------|
| 0 | 0 | ±0 |
| 0 | ≠0 | Denormalized |
| 255 | 0 | ±∞ |
| 255 | ≠0 | NaN |

**Machine epsilon (single):** 2⁻²³ ≈ 1.19 × 10⁻⁷

---

## 3. Addressing Modes (Memory Accesses)

| Mode | EA | Mem Access |
|------|-----|-----------|
| Immediate | In instruction | 0 |
| Register | Register | 0 |
| Direct | Address | 1 |
| Register Indirect | [Reg] | 1 |
| Indirect | [Address] | 2 |
| Displacement | Reg + Offset | 1 |

---

## 4. Pipeline Hazards

| Hazard | Type | Solution |
|--------|------|----------|
| Structural | Resource conflict | Duplicate hardware |
| RAW | True dependency | Forwarding/Stalling |
| WAR | Anti dependency | Register renaming |
| WAW | Output dependency | Register renaming |
| Control | Branch | Prediction/Delay slot |

**Load-use:** Even with forwarding → **1 stall cycle**
**Branch penalty** = stages between IF and resolution

**Stall CPI formula:**
CPI = 1 + (branch_fraction × misprediction_rate × penalty)

### ILP & Advanced Pipelining
| Feature | Superscalar | VLIW |
|---------|-----------|------|
| Scheduling | Hardware (dynamic) | Compiler (static) |
| Compatibility | Binary compatible | Recompile needed |
| Example | Intel Core, ARM | Itanium |

- **Register Renaming:** Eliminates WAR, WAW (not RAW)
- **Out-of-Order:** Execute when operands ready, commit in order (ROB)
- **Speculative Execution:** Predict + execute; flush on misprediction
- **BTB:** Caches (branch PC, target, prediction) — eliminates 1 cycle penalty
- **Loop Unrolling:** Reduces branch frequency, exposes more ILP

---

## 5. Cache Quick Reference

### Address Breakdown (byte-addressable, m-bit address)
- **Offset** = log₂(Block_size)
- **Index** = log₂(Sets) = log₂(C / (B × k))
- **Tag** = m − Index − Offset

### Mapping Types
| Type | Sets | Ways | Conflict Miss |
|------|------|------|--------------|
| Direct | C/B | 1 | High |
| k-way SA | C/(B×k) | k | Medium |
| Fully Assoc | 1 | C/B | None |

### 3 Cs of Misses
| Miss | Cause | Fix |
|------|-------|-----|
| Compulsory | First access | Prefetch |
| Capacity | Cache too small | Bigger cache |
| Conflict | Mapping collision | Higher associativity |

### Write Policies
- **Write-through + No-write-allocate** (simple, consistent)
- **Write-back + Write-allocate** (efficient, needs dirty bit)

### Multi-level AMAT
AMAT = L1_hit + L1_miss × (L2_hit + L2_miss × Mem_time)

### Memory Interleaving
| Type | Module Selection | Best For |
|------|-----------------|----------|
| Low-order | Addr mod m | Sequential access (cache fills) |
| High-order | Addr / module_size | Multi-process independent |

- Effective access time (low-order) = T/m
- Need m ≥ T/τ for full pipelining

### Virtual Memory (COA)
- **TLB EAT** = h×(t_TLB + t_mem) + (1−h)×(t_TLB + (levels+1)×t_mem)
- Page table size = (Virtual pages) × (entry size)
- Multi-level: reduces actual memory used (allocate on demand)

| PTE Field | Purpose |
|-----------|--------|
| Frame # | Physical frame |
| Valid | In memory? |
| Dirty | Written? |
| Referenced | Recently used? |
| Protection | R/W/X bits |

### Cache Coherence (MESI)
| State | Description |
|-------|------------|
| **M** (Modified) | Only copy, dirty |
| **E** (Exclusive) | Only copy, clean |
| **S** (Shared) | Multiple copies, clean |
| **I** (Invalid) | Not valid |

- Write hit in E → M (silent, no bus)
- Write hit in S → M (bus invalidate)
- Read miss (other M) → writeback, both S
- **False sharing:** Different vars, same cache line → ping-pong. Fix: padding

---

## 6. I/O Summary

| Method | CPU Involvement | Speed |
|--------|----------------|-------|
| Programmed | 100% (polling) | Slow |
| Interrupt | ISR only | Medium |
| DMA | Setup only | Fast |

**DMA modes:** Cycle stealing (1 word), Burst (entire block), Interleaved.

### Bus Arbitration
| Method | Priority | Hardware |
|--------|----------|----------|
| Daisy chain | Fixed (closest=highest) | Minimal |
| Centralized parallel | Configurable | More wires |
| Distributed | Fair (self-selection) | Complex |

- **Synchronous bus:** Global clock, simple, short distance
- **Asynchronous bus:** Handshake, adapts to device speed, long distance

---

## 7. Number Quick Facts

| Item | Value |
|------|-------|
| Endian (x86) | Little-endian |
| Endian (network) | Big-endian |
| BCD invalid | 1010–1111 |
| 2's comp range (n bits) | −2ⁿ⁻¹ to 2ⁿ⁻¹−1 |
| Overflow (2's comp) | C_in ⊕ C_out at MSB |

---

## 8. GATE Traps

1. **AMAT:** Use LOCAL miss rate for L2, not global
2. **Pipeline speedup ≠ k** for small n: use nk/(k+n−1)
3. **Forwarding doesn't fix load-use:** Still 1 stall
4. **Cache size = data only:** Tags and valid bits are overhead
5. **IEEE 754 mantissa:** Hidden bit = 1 for normalized
6. **Byte vs word addressable:** Affects memory size calculation
7. **Page table entries:** Based on VIRTUAL address space, not physical
8. **Branch penalty:** Depends on WHERE branch is resolved (ID vs EX)
9. **Write-back eviction:** Extra penalty if dirty bit set
10. **Expanding opcode:** Each level's available codes = remaining × 2^(field_width)

---

*End of Revision Notes — Computer Organization & Architecture for GATE 2027*
