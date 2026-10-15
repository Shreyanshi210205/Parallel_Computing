# MPI-Based Parallel Computing

> Implementation of a deadlock-free point-to-point MPI pipeline in C, benchmarked on the **PARAM Rudra** HPC cluster with scalability analysis across multiple process counts.


## Assignment 1 — Point-to-Point MPI Pipeline

### Overview
Simulates a parallel data-processing pipeline using **strictly point-to-point MPI** communication. Each process operates on a linear topology, acting simultaneously as a sender (to ranks D1 and D2 distances away) and a receiver, performing element-wise computations over T iterations.

### Algorithm
- **Parity-based Odd-Even Scheduling** — Processes are grouped into blocks of size D. Even blocks send first; odd blocks receive first. This prevents deadlock without non-blocking calls.
- **Binary-Tree Reduction** — Global maximum values are aggregated at Rank 0 in O(log P) steps, since `MPI_Reduce` is forbidden for data values.

### Compilation & Execution
```bash
mpicc -o src src.c -lm
mpirun -np <P> ./src <M> <D1> <D2> <T> <seed>

# Example
mpirun -np 16 ./src 262144 2 4 10 1000
```

### Configurations
| Parameter | Values |
|-----------|--------|
| P (processes) | 8, 16, 32 |
| M (doubles) | 262144, 1048576 |
| D1 / D2 / T / seed | 2 / 4 / 10 / 1000 |

Run each configuration 5 times. Use 16 processes per node.

### Results & Observations
Execution time **increases monotonically** with process count for both data sizes. Communication overhead grows faster than the parallel speedup gain, making inter-process communication the dominant factor. Low variance across runs confirms the trend is stable.

## Output Format

Single line per run:
```
<maxD1> <maxD2> <time_seconds>
```
