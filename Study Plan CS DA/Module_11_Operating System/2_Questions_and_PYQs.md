# Operating Systems — Questions & Previous Year Questions (PYQs) for GATE 2027

> **Total: 80 Questions** — Covering all modules with mixed difficulty
> Detailed solutions in `3_Solutions.md`

---

## Section A: Processes, Threads & System Calls (15 Questions)

### Q1. [GATE 2005 — 2 marks]
Consider the following C code:
```c
int main() {
    int i;
    for (i = 0; i < 3; i++)
        fork();
    return 0;
}
```
How many total processes exist after the execution (including the original)?

(a) 3  
(b) 7  
(c) 8  
(d) 9

---

### Q2. [Practice — 2 marks]
What is the output of the following code?
```c
int main() {
    int x = 5;
    if (fork() == 0) {
        x = x + 10;
        printf("%d ", x);
    } else {
        x = x - 5;
        printf("%d ", x);
    }
    return 0;
}
```

(a) 15 0  
(b) 0 15  
(c) 15 0 or 0 15  
(d) 5 5

---

### Q3. [GATE 2010 — 1 mark]
Which of the following is NOT a valid state transition in a process state diagram?

(a) Running → Ready  
(b) Ready → Running  
(c) Waiting → Running  
(d) Running → Waiting

---

### Q4. [Practice — 1 mark]
The Process Control Block (PCB) does NOT contain:

(a) Program counter  
(b) Process ID  
(c) Source code of the process  
(d) CPU register values

---

### Q5. [GATE 2017 — 2 marks]
How many processes will the following code create (excluding the original)?
```c
fork();
if (fork()) {
    fork();
}
```

(a) 3  
(b) 4  
(c) 5  
(d) 6

---

### Q6. [Practice — 1 mark]
In a one-to-one threading model, if a process has 5 user-level threads, how many kernel threads are allocated?

(a) 1  
(b) 5  
(c) Depends on OS  
(d) 0

---

### Q7. [Practice — 2 marks]
A process has a virtual address space of 4 GB. It creates 3 threads. What is the total virtual address space consumed?

(a) 4 GB  
(b) 12 GB  
(c) 16 GB  
(d) Cannot be determined

---

### Q8. [GATE 2014 — 1 mark]
Which system call creates a child that executes a different program?

(a) fork() only  
(b) exec() only  
(c) fork() followed by exec()  
(d) wait()

---

### Q9. [Practice — 2 marks]
The following code uses fork(). How many times is "Hello" printed?
```c
int main() {
    fork();
    fork();
    printf("Hello\n");
    return 0;
}
```

(a) 2  
(b) 3  
(c) 4  
(d) 8

---

### Q10. [Practice — 1 mark]
Context switching between kernel-level threads is generally ______ than between user-level threads.

(a) faster  
(b) slower  
(c) same speed  
(d) not comparable

---

### Q11. [GATE 2006 — 2 marks]
Consider the following code:
```c
int main() {
    if (fork() || fork())
        fork();
    return 0;
}
```
Total number of processes (including parent)?

---

### Q12. [Practice — 1 mark]
Which of the following is TRUE about threads?

(a) Threads of the same process share the stack  
(b) Threads of the same process share the heap  
(c) Threads cannot access global variables  
(d) Each thread has its own address space

---

### Q13. [Practice — 2 marks]
A process makes the following system calls in sequence: fork(), exec("ls"), wait(). What happens?

(a) Parent executes "ls" and waits  
(b) Child executes "ls", parent waits for child  
(c) Both execute "ls"  
(d) Compilation error

---

### Q14. [GATE 2003 — 1 mark]
Which scheduling is involved when a process switches from running state to ready state?

(a) Non-preemptive  
(b) Preemptive  
(c) Both  
(d) Neither

---

### Q15. [Practice — 2 marks]
How many processes are created by:
```c
fork() && fork() || fork();
```

---

## Section B: CPU Scheduling (15 Questions)

### Q16. [GATE 2008 — 2 marks]
Consider the following processes:

| Process | AT | BT |
|---------|----|----|
| P1 | 0 | 4 |
| P2 | 1 | 3 |
| P3 | 2 | 1 |
| P4 | 3 | 2 |

Using SRTF scheduling, the average waiting time is:

(a) 0.5  
(b) 1.0  
(c) 1.5  
(d) 2.0

---

### Q17. [Practice — 2 marks]
Using Round Robin with time quantum = 2:

| Process | AT | BT |
|---------|----|----|
| P1 | 0 | 5 |
| P2 | 1 | 3 |
| P3 | 2 | 4 |

What is the average turnaround time?

---

### Q18. [GATE 2015 — 2 marks]
Which scheduling algorithm can cause Belady's anomaly in the context of page replacement, but when used as a CPU scheduling algorithm is a simple non-preemptive approach?

(a) SJF  
(b) FCFS  
(c) Round Robin  
(d) Priority

---

### Q19. [Practice — 1 mark]
The convoy effect is primarily associated with:

(a) SJF  
(b) FCFS  
(c) SRTF  
(d) Round Robin

---

### Q20. [GATE 2019 — 2 marks]
Five processes arrive simultaneously (AT=0) with burst times 10, 6, 2, 4, 8. What is the average waiting time using SJF?

---

### Q21. [Practice — 2 marks]
Using non-preemptive priority scheduling (lower number = higher priority):

| Process | AT | BT | Priority |
|---------|----|----|----------|
| P1 | 0 | 10 | 3 |
| P2 | 1 | 1 | 1 |
| P3 | 2 | 2 | 4 |
| P4 | 3 | 5 | 2 |

What is the order of execution?

---

### Q22. [Practice — 1 mark]
Which CPU scheduling algorithm is optimal in terms of average waiting time among non-preemptive algorithms?

(a) FCFS  
(b) SJF  
(c) Round Robin  
(d) Priority

---

### Q23. [GATE 2021 — 2 marks]
In a system using RR scheduling with TQ = 3, the following processes are given:

| Process | AT | BT |
|---------|----|----|
| P1 | 0 | 8 |
| P2 | 5 | 2 |
| P3 | 1 | 7 |

Calculate the completion time for each process.

---

### Q24. [Practice — 2 marks]
Given n processes with burst times b₁, b₂, ..., bₙ (all arrive at time 0), the minimum possible average waiting time with SRTF is _______ compared to SJF?

(a) Less  
(b) Equal  
(c) Greater  
(d) Cannot determine

---

### Q25. [Practice — 1 mark]
The solution to starvation in priority scheduling is:

(a) Preemption  
(b) Aging  
(c) Banker's algorithm  
(d) Round Robin

---

### Q26. [GATE 2012 — 2 marks]
A multilevel queue scheduler has 3 queues: Q1 (RR, TQ=4), Q2 (RR, TQ=8), Q3 (FCFS). A process starts in Q1, after timeout moves to Q2, then Q3. A process with burst time 20 starts at time 0. When does it complete?

---

### Q27. [Practice — NAT — 2 marks]
In RR with TQ = 4, three processes P1(BT=10), P2(BT=4), P3(BT=6) all arrive at t=0. The number of context switches is _____.

---

### Q28. [Practice — 1 mark]
Among FCFS, SJF, SRTF, and RR, which can lead to starvation?

(a) FCFS and RR  
(b) SJF and SRTF  
(c) Only SRTF  
(d) FCFS only

---

### Q29. [GATE 2016 — 2 marks]
Consider a preemptive priority scheduling algorithm and the following set of processes:

| Process | AT | BT | Priority |
|---------|----|----|----------|
| P1 | 0 | 4 | 2 |
| P2 | 1 | 3 | 1 |
| P3 | 2 | 5 | 3 |
| P4 | 3 | 2 | 1 |

What is the average waiting time? (Lower number = higher priority)

---

### Q30. [Practice — 2 marks]
SRTF scheduling with the following:

| Process | AT | BT |
|---------|----|----|
| P1 | 0 | 9 |
| P2 | 1 | 3 |
| P3 | 3 | 2 |

Draw the Gantt chart and find average TAT.

---

## Section C: Process Synchronization (15 Questions)

### Q31. [GATE 2007 — 2 marks]
A critical section solution must satisfy which of the following?

I. Mutual Exclusion  
II. Progress  
III. Bounded Waiting

(a) I only  
(b) I and II  
(c) I, II, and III  
(d) I and III

---

### Q32. [Practice — 2 marks]
Consider Peterson's solution. If process P0 executes `flag[0] = true; turn = 1;` and process P1 executes `flag[1] = true; turn = 0;` simultaneously, which process enters the CS?

---

### Q33. [GATE 2013 — 2 marks]
```c
Semaphore S1 = 1, S2 = 0;

Process P1:          Process P2:
wait(S1);            wait(S2);
// CS1               // CS2
signal(S2);          signal(S1);
```
What is the order of execution?

(a) CS1 then CS2 always  
(b) CS2 then CS1 always  
(c) Either CS1 or CS2 first  
(d) Deadlock

---

### Q34. [Practice — 2 marks]
A bounded buffer of size 5 uses semaphores: mutex=1, empty=5, full=0. After 3 items are produced and 1 consumed, what are the values of empty and full?

---

### Q35. [GATE 2020 — 2 marks]
Consider the following code for two processes:
```
P1:                 P2:
wait(S);            wait(Q);
wait(Q);            wait(S);
...                 ...
signal(Q);          signal(S);
signal(S);          signal(Q);
```
If S = 1 and Q = 1, can this lead to deadlock?

---

### Q36. [Practice — 1 mark]
The initial value of a counting semaphore is 5. After 10 P (wait) operations and 4 V (signal) operations, what is the value?

(a) −1  
(b) 0  
(c) 1  
(d) −6

---

### Q37. [GATE 2018 — 2 marks]
Three processes P1, P2, P3 need to execute in the order: P1 → P2 → P3. Using minimum number of binary semaphores initialized to 0, how many semaphores are needed?

(a) 1  
(b) 2  
(c) 3  
(d) 0

---

### Q38. [Practice — 2 marks]
In the readers-writers problem, if there are currently 3 readers reading and a writer arrives, followed by 2 more readers, what happens in the readers-preference solution?

---

### Q39. [Practice — 2 marks]
Identify which of the following satisfies mutual exclusion, progress, and bounded waiting:

```c
// Attempt 1
while (true) {
    while (turn != i);
    // CS
    turn = j;
}
```

---

### Q40. [GATE 2009 — 2 marks]
A binary semaphore is initialized to 1. After P operations by 5 processes, how many processes are blocked?

(a) 0  
(b) 1  
(c) 4  
(d) 5

---

### Q41. [Practice — 1 mark]
In the dining philosophers problem with 5 philosophers, what is the minimum number of forks needed to avoid deadlock if we limit sitting?

(a) 3  
(b) 4  
(c) 5  
(d) 10

---

### Q42. [Practice — 2 marks]
```c
Semaphore S = 2;
// Process code:
wait(S);
// use resource
signal(S);
```
What is the maximum number of processes that can be in the CS simultaneously?

---

### Q43. [GATE 2022 — 2 marks]
Consider the following:
```
Process A:           Process B:
x = 1;              y = 1;
turn = 2;           turn = 1;
while (y == 1 &&    while (x == 1 &&
  turn == 2);         turn == 1);
// CS                // CS
x = 0;              y = 0;
```
Does this satisfy mutual exclusion, progress, and bounded waiting?

---

### Q44. [Practice — 2 marks]
Implement a solution using semaphores where Process A must print "Go" only after Process B prints "Ready" and Process C prints "Set".

---

### Q45. [Practice — 1 mark]
A monitor with condition variables uses which of the following to suspend a process?

(a) wait() on semaphore  
(b) cwait() on condition variable  
(c) sleep()  
(d) block()

---

## Section D: Deadlocks (10 Questions)

### Q46. [GATE 2014 — 2 marks]
A system has 3 resource types: A (7 instances), B (2), C (6). Snapshot:

```
        Allocation   Max
P0:     0 1 0       7 5 3
P1:     2 0 0       3 2 2
P2:     3 0 2       9 0 2
P3:     2 1 1       2 2 2
P4:     0 0 2       4 3 3
```
Available = [3, 3, 2]. Find a safe sequence.

---

### Q47. [Practice — 1 mark]
Which condition can be most practically denied to prevent deadlock?

(a) Mutual exclusion  
(b) Hold and wait  
(c) No preemption  
(d) Circular wait

---

### Q48. [GATE 2011 — 2 marks]
A system has 12 instances of a resource. Three processes P, Q, R have maximum needs 5, 4, 7 respectively. Currently they hold 2, 2, 3 resources. Is the system in a safe state?

---

### Q49. [Practice — 2 marks]
Draw the resource allocation graph for:
- P1 holds R1 and requests R2
- P2 holds R2 and requests R3
- P3 holds R3 and requests R1
Is there a deadlock?

---

### Q50. [GATE 2007 — 2 marks]
In a system with n processes and m identical resources, deadlock is guaranteed to be avoided if:

(a) m ≥ n  
(b) m > n  
(c) m ≥ n(k−1) + 1 where k is max need per process  
(d) None of the above

---

### Q51. [Practice — 2 marks]
A system has 4 tape drives. Processes P1, P2, P3 have max needs 3, 2, 3. Currently: P1=1, P2=1, P3=1. Available=1. Is this safe? What is the safe sequence?

---

### Q52. [GATE 2018 — 1 mark]
If a system uses only spooling for all I/O, which deadlock condition is eliminated?

(a) Mutual exclusion  
(b) Hold and wait  
(c) No preemption  
(d) Circular wait

---

### Q53. [Practice — 2 marks]
Using the Banker's algorithm, if a request from P1 for [1, 0, 2] in Q46 is made, should it be granted?

---

### Q54. [Practice — 1 mark]
A Resource Allocation Graph has a cycle but no deadlock. This is possible when:

(a) Single instance of each resource  
(b) Multiple instances of resources  
(c) No waiting processes  
(d) Not possible

---

### Q55. [Practice — 2 marks]
n processes share m identical resources. Each needs at most k resources. What minimum m guarantees no deadlock?

---

## Section E: Memory Management & Paging (15 Questions)

### Q56. [GATE 2015 — 2 marks]
A system has 32-bit virtual address, 4 KB page size, and 24-bit physical address. How many entries are in the page table?

(a) 2²⁰  
(b) 2¹²  
(c) 2²⁴  
(d) 2³²

---

### Q57. [Practice — 2 marks]
In a 2-level paging system with 32-bit virtual address, 4KB pages, and 4-byte PTEs, how is the address split?

---

### Q58. [GATE 2020 — 2 marks]
A TLB has a hit ratio of 90%. Memory access time is 100 ns. TLB access time is 10 ns. With 2-level paging, what is the effective memory access time?

---

### Q59. [Practice — 2 marks]
A process has 16 pages. The page table is:

| Page | Frame |
|------|-------|
| 0 | 5 |
| 1 | 3 |
| 2 | - (invalid) |
| 3 | 8 |

If page size = 1024 bytes and logical address = 1028, what is the physical address?

---

### Q60. [GATE 2017 — 2 marks]
A virtual memory system with page size 4KB has a page table where each entry is 4 bytes. If the virtual address space is 2³² bytes, what is the total size of the page table?

(a) 1 MB  
(b) 2 MB  
(c) 4 MB  
(d) 8 MB

---

### Q61. [Practice — 2 marks]
With inverted page table, if physical memory = 256 MB and frame size = 4 KB, how many entries are in the inverted page table?

---

### Q62. [GATE 2019 — 2 marks]
In a segmented system, the segment table has:

| Segment | Base | Limit |
|---------|------|-------|
| 0 | 200 | 100 |
| 1 | 1500 | 400 |
| 2 | 3000 | 200 |

What is the physical address for logical address (1, 350)?

(a) 1850  
(b) 1500  
(c) Segment fault  
(d) 350

---

### Q63. [Practice — 2 marks]
A 3-level paging system has 48-bit virtual addresses and 4KB pages. PTEs are 8 bytes. If each page table fits in exactly one page, how is the address split?

---

### Q64. [GATE 2013 — 2 marks]
In a paged system with page size 2KB, the logical address 12345 maps to which page number and offset?

---

### Q65. [Practice — 2 marks]
What is internal fragmentation compared to external fragmentation? Which memory allocation schemes suffer from each?

---

### Q66. [Practice — 2 marks]
A Unix-like file system uses an inode with 10 direct, 1 single indirect, 1 double indirect, and 1 triple indirect block pointer. Block size = 4KB, pointer = 4 bytes. What is the maximum file size?

---

### Q67. [GATE 2020 — 2 marks]
A TLB has access time 5ns, page table is in memory with 100ns access time. With single level paging:
- EAT with TLB hit rate 80%: ______
- EAT with TLB hit rate 98%: ______

---

### Q68. [Practice — 1 mark]
If page size is doubled while keeping the address space same, the page table size:

(a) Doubles  
(b) Halves  
(c) Stays same  
(d) Quadruples

---

### Q69. [Practice — 2 marks]
In demand paging, if EAT must be no more than 10% slower than memory access time (200ns), and page fault service time is 8ms, what is the maximum acceptable page fault rate?

---

### Q70. [GATE 2010 — 2 marks]
An inverted page table is searched using:

(a) Virtual address  
(b) Physical address  
(c) Hash of virtual address  
(d) Process ID only

---

## Section F: Page Replacement & Virtual Memory (5 Questions)

### Q71. [GATE 2016 — 2 marks]
Reference string: 1, 2, 3, 4, 2, 1, 5, 6, 2, 1, 2, 3, 7, 6, 3. Using LRU with 4 frames, how many page faults occur?

---

### Q72. [Practice — 2 marks]
Show Belady's Anomaly with FIFO: Reference string 1,2,3,4,1,2,5,1,2,3,4,5 with 3 frames vs 4 frames.

---

### Q73. [GATE 2018 — 2 marks]
Reference string: 7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0, 1. Using Optimal with 3 frames, count page faults.

---

### Q74. [Practice — 1 mark]
Which of the following is a stack algorithm (no Belady's Anomaly)?

(a) FIFO  
(b) LRU  
(c) Clock  
(d) Both (b) and (c)

---

### Q75. [Practice — 2 marks]
A system has 100 frames. Working sets of processes: P1=25, P2=30, P3=20, P4=35. Will thrashing occur? If total demand exceeds available frames by 10%, what happens?

---

## Section G: File Systems (5 Questions)

### Q76. [GATE 2012 — 2 marks]
A file system uses linked allocation with block size = 512 bytes and pointer = 4 bytes. If a file is 2000 bytes, how many blocks are needed?

---

### Q77. [Practice — 2 marks]
Using Unix-style inode indexing: 12 direct pointers, 1 single indirect, 1 double indirect, 1 triple indirect. Block size = 1KB, pointer = 4 bytes. What is the maximum file size in blocks?

---

### Q78. [GATE 2019 — 2 marks]
A bitmap for free-space management on a disk with 2²⁰ blocks requires how much space?

(a) 128 KB  
(b) 256 KB  
(c) 512 KB  
(d) 1 MB

---

### Q79. [Practice — 2 marks]
Compare contiguous, linked, and indexed file allocation methods in terms of: sequential access, random access, external fragmentation, and space overhead.

---

### Q80. [Practice — 2 marks]
A disk has tracks numbered 0–199. The head is currently at track 53 and moving toward 0. Requests: 98, 183, 37, 122, 14, 124, 65, 67. Calculate total head movement using:
(a) SSTF  
(b) SCAN  
(c) C-LOOK

---

## Section H: Real-Time Scheduling, IPC & Advanced Topics (20 Questions)

### Q81. [GATE] Three periodic tasks with (C, T): T1=(1,4), T2=(2,6), T3=(3,12). Is this task set schedulable under Rate Monotonic (RM)?

### Q82. [GATE 2019] For the task set in Q81, is it schedulable under EDF?

### Q83. [Practice — 1 mark]
Rate Monotonic scheduling assigns priority based on:
(a) execution time (b) period (c) deadline (d) arrival time

### Q84. [GATE] The utilization bound for RM with 2 tasks is:
(a) 0.693 (b) 0.828 (c) 1.000 (d) 0.780

### Q85. [Practice — 1 mark]
EDF is optimal among:
(a) fixed-priority algorithms (b) all uniprocessor algorithms (c) multiprocessor algorithms (d) non-preemptive algorithms

### Q86. [GATE] The Bakery Algorithm works for:
(a) 2 processes only (b) any number of processes (c) only single-threaded programs (d) distributed systems

### Q87. [Practice — 2 marks]
In the Bakery Algorithm, if two processes choose the same number, how is the tie broken?

### Q88. [GATE 2017] Which IPC mechanism requires explicit synchronization by the programmer?
(a) Pipes (b) Message passing (c) Shared memory (d) Signals

### Q89. [Practice — 1 mark]
An ordinary (anonymous) pipe in Unix is:
(a) bidirectional (b) unidirectional (c) usable by unrelated processes (d) persistent

### Q90. [GATE] In message passing, blocking send + blocking receive is called:
(a) asynchronous (b) rendezvous (c) mailbox (d) buffered

### Q91. [Practice — 2 marks]
```c
int fd[2];
pipe(fd);
if (fork() == 0) {
    close(fd[0]);
    write(fd[1], "hello", 5);
    close(fd[1]);
} else {
    close(fd[1]);
    char buf[10];
    read(fd[0], buf, 5);
    printf("%s", buf);
    close(fd[0]);
}
```
What does this program print?

### Q92. [GATE] Which directory structure allows file sharing through links but may have dangling pointer issues?
(a) Single-level (b) Tree-structured (c) Acyclic graph (d) Two-level

### Q93. [Practice — 1 mark]
A hard link differs from a symbolic link in that:
(a) Hard links can cross file systems (b) Hard links point to the inode directly (c) Hard links disappear when the original is deleted (d) Hard links are files containing paths

### Q94. [GATE] In Unix, the permission bits 755 correspond to:
(a) rwxrwxrwx (b) rwxr-xr-x (c) rw-r--r-- (d) rwx------ (e) none

### Q95. [Practice — 1 mark]
An Access Control List (ACL) is associated with:
(a) each subject (b) each object (c) the kernel (d) the CPU

### Q96. [GATE 2018] A disk with 200 tracks (0-199), head at 100 moving toward 199. Requests: 45, 134, 72, 180, 11, 156. Calculate total head movement using C-SCAN.

### Q97. [Practice — 2 marks]
For the same disk as Q96, calculate total head movement using LOOK and C-LOOK.

### Q98. [GATE] SSTF disk scheduling may cause:
(a) deadlock (b) starvation of far-away requests (c) excessive fragmentation (d) buffer overflow

### Q99. [GATE] The setuid bit in Unix is used to:
(a) set user ID (b) run the program with the file owner's permissions (c) encrypt the file (d) lock the file

### Q100. [Practice — 2 marks]
Three periodic tasks: T1=(2,5), T2=(1,8), T3=(5,20). Verify schedulability under (i) RM (ii) EDF. If RM fails the utilization test, can it still be schedulable?

---

*End of Questions — Operating Systems for GATE 2027*
