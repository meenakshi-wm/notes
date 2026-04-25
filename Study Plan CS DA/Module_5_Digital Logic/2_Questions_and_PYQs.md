# Digital Logic — Questions and Previous Year Questions for GATE 2027

> **Total Questions:** 70 | **Sections:** 7
> **Difficulty Mix:** Easy (30%) | Medium (40%) | GATE PYQ level (30%)

---

## Section A: Boolean Algebra & Laws (10 Questions)

### Easy
**Q1.** Simplify using Boolean algebra: F = AB + AB' + A'B

**Q2.** Find the complement of F = (A + B)(A'C + B')

**Q3.** Find the dual of F = AB + A'C + 0

### Medium
**Q4.** Using Boolean identities only (no K-map), simplify:
F = A'BC + A'BC' + AB'C + AB'C' + ABC

**Q5.** Prove using algebraic manipulation: AB + A'C + BC = AB + A'C (Consensus theorem)

**Q6.** Express F(A,B,C) = A + B'C in canonical SOP form (sum of minterms).

**Q7.** Convert F = Σm(0,2,4,5,6) into POS form with three variables A, B, C.

### GATE Level
**Q8.** [GATE] The Boolean function F = A'C + B'C' + A'B is equivalent to:
(a) A'C + B'C'  (b) A'B + B'C'  (c) AB' + BC  (d) A'C + AB'

**Q9.** How many self-dual functions are possible with 3 variables?

**Q10.** Using algebraic methods, simplify: F = (A⊕B)C + (A⊕B)'C' + AC

---

## Section B: K-Map and Minimization (10 Questions)

### Easy
**Q11.** Minimize using K-map: F(A,B,C) = Σm(0,2,4,6)

**Q12.** Minimize using K-map: F(A,B,C) = Σm(1,3,5,7)

### Medium
**Q13.** Minimize: F(A,B,C,D) = Σm(0,1,2,5,8,9,10) + d(3,7,11,15)

**Q14.** Minimize: F(A,B,C,D) = Σm(4,5,6,7,12,13,14,15)

**Q15.** For F(A,B,C,D) = Σm(0,2,3,5,7,8,10,11,14,15), find all prime implicants and identify essential prime implicants.

**Q16.** Minimize the POS form: F(A,B,C,D) = ΠM(0,1,2,5,8,9,10)

### GATE Level
**Q17.** [GATE 2015] Consider the function F(w,x,y,z) = Σm(0,1,2,3,7,8,10). The number of essential prime implicants is _____.

**Q18.** [GATE 2016] The number of prime implicants of F(A,B,C,D) = Σm(0,2,5,7,8,10,13,15) is:
(a) 2  (b) 3  (c) 4  (d) 5

**Q19.** Minimize and compare the cost of SOP vs POS for:
F(A,B,C,D) = Σm(1,3,4,5,6,7,9,11,12,13,14,15)

**Q20.** How many minimum SOP expressions exist for F(A,B,C,D) = Σm(0,2,5,7,8,10,13,15)?

---

## Section C: Number Systems & Codes (10 Questions)

### Easy
**Q21.** Convert (247)₈ to binary and hexadecimal.

**Q22.** Represent −35 in 8-bit: (a) Sign-magnitude, (b) 1's complement, (c) 2's complement.

**Q23.** Convert binary 1101.101 to decimal.

### Medium
**Q24.** Perform 45 − 78 using 8-bit 2's complement. Is there overflow?

**Q25.** What range of integers can be represented in 12-bit 2's complement?

**Q26.** Convert binary 10110111 to Gray code and back.

**Q27.** What is the BCD representation of 397? Can 1010 0110 be a valid BCD?

### GATE Level
**Q28.** [GATE] An 8-bit 2's complement system performs A + B where A = 01100011 and B = 01011010. What is the result and is there overflow?

**Q29.** In a system using excess-3 code, what corresponds to decimal 47?

**Q30.** [GATE] The 2's complement representation of −43 in 8 bits is _______.

---

## Section D: Combinational Circuits (15 Questions)

### Easy
**Q31.** A half adder is implemented. Write the circuit equation for Sum and Carry.

**Q32.** How many 3:8 decoders are needed to implement a 5:32 decoder?

**Q33.** Implement F(A,B) = A'B + AB' using a 2:1 MUX with A as select.

### Medium
**Q34.** Implement F(A,B,C) = Σm(0,2,6,7) using an 8:1 MUX.

**Q35.** Implement F(A,B,C) = Σm(1,3,5,6) using a 4:1 MUX with A,B as select lines and C as the data variable.

**Q36.** Implement the function F(A,B,C) = Σm(0,3,5,6) using a 3:8 decoder and external gates.

**Q37.** A 4-bit ripple carry adder has gates with 2-unit delay each. What is the worst-case delay for the carry output?

**Q38.** In a carry look-ahead adder, derive C₃ in terms of G, P, and C₀.

**Q39.** Design a BCD to Excess-3 code converter using combinational logic.

**Q40.** How many 2:1 MUX are needed to implement a 16:1 MUX?

### GATE Level
**Q41.** [GATE 2017] Consider a 4:1 MUX with two select lines S₁S₀. Inputs are I₀=1, I₁=S₁'S₀, I₂=0, I₃=S₁S₀'. Find the Boolean function implemented.

**Q42.** [GATE] A priority encoder has inputs D₀, D₁, D₂, D₃ (D₃ highest priority). If D₁=D₃=1 and D₀=D₂=0, what is the output?

**Q43.** [GATE] What is the total propagation delay for an n-bit CLA adder compared to an n-bit ripple carry adder?

**Q44.** A 4-bit ALU performs both addition and subtraction using a single circuit. Draw the block diagram using a 4-bit adder and XOR gates.

**Q45.** [GATE] The carry propagation delay of a 64-bit CLA adder built using 4-bit CLA blocks in a two-level hierarchy is ______ gate delays.

---

## Section E: Functional Completeness (5 Questions)

### Easy
**Q46.** Show that {NAND} is a functionally complete set by expressing AND, OR, NOT using NAND only.

### Medium
**Q47.** Is {AND, XOR} a functionally complete set? Prove using Post's theorem.

**Q48.** Is {NOR, 1} a functionally complete set?

### GATE Level
**Q49.** [GATE] Which of the following is a functionally complete set?
(a) {AND, OR}  (b) {NAND}  (c) {XOR, AND}  (d) {XOR, OR}

**Q50.** Determine if {→ (implication)} is functionally complete. (Hint: A→B = A'+B)

---

## Section F: Sequential Circuits — Flip-Flops (15 Questions)

### Easy
**Q51.** Write the characteristic equation for JK, D, and T flip-flops.

**Q52.** A D flip-flop with D = Q' is clocked. If initial Q = 0, what are the next 4 states?

**Q53.** What is the difference between a latch and a flip-flop?

### Medium
**Q54.** Convert a JK flip-flop to a D flip-flop. What is the connection?

**Q55.** Convert a D flip-flop to a T flip-flop.

**Q56.** A sequential circuit has one flip-flop Q and one input X. The next-state equation is Q(t+1) = XQ' + X'Q. What type of flip-flop behavior does this represent?

**Q57.** Design a circuit that detects the sequence "101" using a Mealy machine. Draw the state diagram.

**Q58.** In a Master-Slave JK flip-flop, explain the race-around problem and how master-slave resolves it.

### GATE Level
**Q59.** [GATE] A circuit has two D flip-flops A and B. DA = A⊕B, DB = A. Initial state A=1, B=0. What is the state after 4 clock pulses?

**Q60.** [GATE] How many minimum states are needed for a Moore machine that detects the sequence "1011" in an overlapping manner?

**Q61.** [GATE 2018] A Mealy machine has output dependent on current state and input. If an equivalent Moore machine is constructed, what is the maximum number of states in the Moore machine if the Mealy has n states and the output alphabet has m symbols?

**Q62.** A circuit uses a T flip-flop with input T = Q XOR X, where X is external input. Trace the output for X = 1, 0, 1, 1, 0 with initial Q = 0.

**Q63.** [GATE] A sequential circuit uses two JK flip-flops. J₁ = Q₂, K₁ = Q₂', J₂ = Q₁', K₂ = Q₁. Find the state transition table and identify the sequence generated.

**Q64.** What is the maximum clock frequency if flip-flop propagation delay is 5ns and setup time is 2ns?

**Q65.** [GATE] In a positive edge-triggered D flip-flop, the setup time is 2ns and hold time is 1ns. If data changes exactly 1ns before the clock edge, is the setup time violated?

---

## Section G: Counters (5 Questions)

### Easy
**Q66.** A 3-bit ripple counter counts from 0 to 7. If each flip-flop has tₚ = 10ns, what is the maximum clock frequency?

### Medium
**Q67.** Design a mod-5 synchronous counter using T flip-flops.

**Q68.** A ring counter uses 4 flip-flops. How many unique states does it cycle through? What is the modulus?

### GATE Level
**Q69.** [GATE] A 4-bit Johnson counter has an initial state 0000. List all states in one complete cycle. What is the modulus?

**Q70.** [GATE] An asynchronous counter counts from 0 to 11 and resets. How many flip-flops are required? Write the reset logic.

---

*End of Questions — Digital Logic for GATE 2027*
