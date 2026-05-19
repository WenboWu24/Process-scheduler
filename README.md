# Process Scheduling Algorithm Simulator

A process scheduling algorithm simulator implemented in C, supporting various classic CPU scheduling algorithms.

## Features

- **Supported Scheduling Algorithms**:
  - FCFS (First-Come, First-Served) - Non-preemptive
  - SJF (Shortest Job First) - Non-preemptive
  - SRTF (Shortest Remaining Time First) - Preemptive
  - RR (Round Robin) - Configurable time quantum
  - MLFQ (Multi-Level Feedback Queue) - 3 levels with quantum times 2, 4, and FCFS

- **Output Statistics**:
  - Gantt chart visualization of scheduling process
  - Detailed process information (arrival time, burst time, start time, finish time, etc.)
  - Average waiting time, average turnaround time, average response time
  - Number of context switches
  - CPU utilization

#### 1. Demo Mode

./sched --demo

Run the preset demo cases.

#### 2. Specify Algorithm Mode

./sched <workload_file> --alg <algorithm>

**Supported Algorithm Parameters**:
- `FCFS` - First-Come, First-Served
- `SJF` - Shortest Job First
- `SRTF` - Shortest Remaining Time First
- `MFQ` - Multi-Level Feedback Queue

## Input File Format

Each line in the workload file represents a process with the following format:

<Process ID> <Arrival Time> <Burst Time>

**Example**:

P1 0 5
P2 1 3
P3 2 1
P4 3 2
```

## Context Switch Calculation

This simulator uses the following rules to calculate context switches:

1. IDLE → Process (P): counts as one context switch
2. Process (P) → IDLE: counts as one context switch
3. Process (P1) → Process (P2): counts as one context switch (including preemption)
4. P → IDLE after all processes finish: does NOT count as a context switch

Note: When the last process finishes execution, the system naturally enters IDLE state, and no context switch is recorded at this point.

## Project Structure
├── src/
│   ├── main.c          # Main program entry
│   ├── sched.h         # Header file and data structure definitions
│   ├── scheduler.c     # Scheduling algorithm implementation
│   ├── output.c        # Result output
│   ├── readfile.c      # File reading
│   └── demo.c          # Demo mode implementation
├── tests/              # Test cases directory
│   ├── workload1.txt
│   ├── workload2.txt
│   ├── workload_empty.txt
│   └── workload_error1.txt
├── workload.txt        # Default workload file
├── Makefile           # Build script
└── README.md          # Project documentation

## Test Cases

The project includes the following test cases:

| File | Description |
|------|-------------|
| `workload1.txt` | Basic test case |
| `workload2.txt` | Complex test case |
| `workload_empty.txt` | Empty file test |
| `workload_error1.txt` | Error format test |

## Output Example

```
~ $ ./sched workload.txt --alg SRTF
Gantt Chart: 0|P1|3|IDLE|5|P2|7|IDLE|8|P3|9|P4|10|P3|14

PID  Arrival  Burst  Start  Finish  Wait  Turnaround  Response
P1   0        3      0        3       0            3               0
P2   5        2      5        7       0     	 2               0
P3   8        5      8       14      1     	 6               0
P4   9        1      9       10      0     	 1               0

RESULT:OK
ALG=SRTF
AVG_WAIT= 0.25
AVG_TAT= 3.00
AVG_RESP=0.00
CONTEXT_SWITCHS=7
CPU_UTIL=78.00%

## Algorithm Descriptions

### FCFS (First-Come, First-Served)
- Executes processes in the order they arrive
- Non-preemptive: once a process starts, it must complete

### SJF (Shortest Job First)
- Selects the process with the shortest burst time from the ready queue
- Non-preemptive

### SRTF (Shortest Remaining Time First)
- Preemptive version of SJF
- When a new process arrives, compares remaining burst times and may preempt the current process

### RR (Round Robin)
- Each process executes for a time quantum in turn
- After the time quantum expires, the process is moved to the end of the ready queue

### MLFQ (Multi-Level Feedback Queue)
- 3 levels with priority from high to low
- Q1: quantum=2, Q2: quantum=4, Q3: FCFS
- Processes are demoted to lower priority queues after using their time quantum

