# Fast Fourier Transform (FFT) Implementation

  This repository provides comprehensive study and **implementation of FFT** using **recursive**, **parallel** (multithreaded), and **distributed** (MPI) techniques, aimed at analyzing performance and scalability in C++.
  
  The goal is to understand trade-offs between execution time, synchronization overhead, and scalability for different FFT execution models.

## Versions of the Cooley–Tukey FFT algorithm:
1. **Serial Implementation**: A baseline single-threaded version.
2. **Parallel Implementation**: A multi-threaded version leveraging **C++ Threads**.
3. **Distributed Implementation**: A scalable solution using **Message Passing Interface** (**MPI**) for large-scale computations.

---

## Features
- **Serial Version**:
  - Recursive and iterative implementations.
  - Single-threaded execution.
- **Parallel Version**:
  - Multi-core optimization with C++ Threads.
- **Distributed Version**:
  - MPI-based FFT across processes with bit-reversal, synchronization, and data aggregation using MPI_Gather.


## Implementation Details
- **Serial FFT**:
  - Acts as baseline for performance comparison.
  - Developed both recursive and iterative versions of radix-2 Cooley-Tukey FFT.
  - Includes a sine wave generator for customizable frequency, amplitude, and sample size.
  - Validates input for power-of-2 sample lengths, a requirement for radix-2 FFT.

- **Parallel FFT**:
  - Each thread is assigned a static data partition.
  - Handles butterfly operations in stages, adjusting thread ranges dynamically as block size grows.
  - Synchronization via custom barriers to avoid race conditions and ensure thread coordination.
  - Thread 0 is responsible for bit-reversal preprocessing.
  - Includes range checks to avoid invalid memory access.

- **Distributed FFT**:
  - Divides the data set across multiple processes.
  - Each process independently: Generates its sine wave dataset, and performs bit-reversal and butterfly operations on its partition.
  - Handles alignment issues by skipping stages with non-aligned ranges.
  - Uses MPI_Barrier and MPI_Gather to synchronize and consolidate results.


## Implementation Details
- **Serial**:
  - Iterative version significantly outperforms the recursive version for large input sizes (e.g., 2^15 samples).
  - Averaged over 3 runs on SFU CSIL machines.

<img width="728" height="438" alt="Image" src="https://github.com/user-attachments/assets/e476da3a-0b05-4400-998f-8895fa7fb712" />

- **Parallel**:
  - Execution time increases with thread count due to excessive synchronization from threads skipping stages.
  - Barrier usage introduces overhead for thread coordination.

<img width="728" height="438" alt="Image" src="https://github.com/user-attachments/assets/8221e638-b28f-44e7-beb1-28a17b83860b" />

- **Distributed**:
  - Scalability with increasing process count is underwhelming.
  - Communication cost and imbalanced workload in process 0 hinder performance.
  - Measured using 1, 2, 4, and 8 processes with sample sizes of 2^10 and 2^15.

<img width="728" height="438" alt="Image" src="https://github.com/user-attachments/assets/6b9531e3-d911-410f-823d-b6cb45d60fbd" />

## Conclusion
This project demonstrated practical application of FFT in different computing environments. Main takeaways:
- Serial FFT gives best performance for moderate input sizes.
- Parallel FFT is sensitive to thread synchronization strategy.
- Distributed FFT has potential for scalability but is bottlenecked by communication and process coordination.

Through this project, we improved our understanding of:
- Low-level algorithm design.
- Thread synchronization and workload balancing.
- MPI communication and real-world distributed systems challenges.


## Contributors
- Arvin Bayat Manesh: Serial implementation, parallel (threaded) FFT, bit-reversal logic
- Dmitrii Beliaev: Distributed MPI FFT implementation, data partitioning logic, process synchronization and result aggregation


## References
SIAM News – Next-Generation FFT Algorithms at PP24: https://www.siam.org/publications/siam-news/articles/next-generation-fft-algorithms-at-pp24/
