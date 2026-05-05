# Bachelor's Thesis – Computer Engineering

## Performance Evaluation of the Vicuna Vector Processor for Neural Network Workloads

This repository contains my Bachelor's Thesis in Computer Engineering, developed at the Autonomous University of Barcelona (UAB).

The project evaluates the performance of the **Vicuna vector coprocessor**, based on the **RISC-V Vector Extension (RVV)**, for accelerating fundamental neural network operations in **embedded systems**, with a particular focus on **New Space** applications.

---

## Project Overview

Embedded space systems face strict constraints in terms of power consumption, communication bandwidth, and predictability. In this context, **on-board neural network inference** becomes a key enabler for reducing downlink data and increasing autonomy.

This thesis explores whether **RISC-V vector computing** can efficiently accelerate neural-network-related workloads under these constraints, using a **configurable and timing-predictable vector architecture**.

The study is carried out entirely in **RTL simulation**, allowing precise control over architectural parameters and reproducible measurements.

---

## Objectives

- Evaluate the performance impact of the RISC-V Vector Extension (RVV) on neural network primitives
- Study how **vector width (VREG_W)** affects execution time and scalability
- Compare **scalar vs vector implementations** under identical conditions
- Identify whether kernels are **memory-bound or compute-bound**
- Assess architectural bottlenecks and scalability limits

---

## Architecture and Technologies

### Processor
- **Vicuna**: Open-source, configurable RISC-V vector coprocessor
- RVV variant: **Zve32x**
- Timing-predictable design (no out-of-order execution or speculation)

### Key RVV Parameters
- **SEW** (Standard Element Width)
- **LMUL** (Register grouping)
- **VL** (Vector Length)
- **VREG_W** (Configurable vector register width)

### Tools and Environment
- C / C++
- RVV intrinsics (explicit vectorization)
- Bare-metal RISC-V toolchain
- RTL simulation using **Verilator**
- Linux-based development environment

---

## Implemented Kernels

The following kernels were implemented in both **scalar** and **vectorized** versions using RVV intrinsics:

- **VVADD** – Element-wise vector addition  
- **SAXPY** – Scalar multiplication and vector addition  
- **REDUCE** – Element-wise multiplication with horizontal reduction  
- **MATMUL** – Matrix multiplication with transposed matrix optimization  

All vector kernels follow a **strip-mining approach**, ensuring portability across different vector widths without modifying the source code.

---

## Methodology

1. **Kernel implementation** using explicit RVV intrinsics to ensure controlled vectorization
2. **Parametric hardware configuration** via Vicuna Makefiles
3. **Cycle-accurate performance measurement** using RISC-V cycle counters
4. Systematic comparison of scalar vs vector versions
5. Roofline-based performance analysis in the cycle domain

---

## Key Results

- Vector performance improves as **VREG_W increases**, due to reduced loop iterations and amortized overhead
- All kernels remain **memory-bound** in the tested configuration
- **VVADD and SAXPY** show clear but saturating speedups
- **REDUCE** suffers from reduction overhead for narrow vector widths and benefits only with larger VREG_W
- **MATMUL**, despite higher arithmetic intensity, does not reach compute-bound behavior due to memory limitations
- The vector unit executes RVV correctly, but performance is ultimately limited by memory bandwidth

---

## Limitations

- Cache (I/D cache) could not be enabled in RTL simulation
- Memory bandwidth becomes the dominant bottleneck
- Autovectorization via compilers (GCC/LLVM) is not mature for RVV, requiring intrinsics
- Other architectural parameters (VMEM_W, pipeline configuration) were not explored in depth

---

## Conclusions

This work demonstrates that **RISC-V vector extensions are viable for accelerating neural network primitives in embedded systems**, provided memory performance is sufficient.

While increasing vector width significantly reduces execution time, **memory bandwidth sets the performance ceiling**. Future improvements should focus on enabling cache, increasing memory throughput, and scaling problem sizes to reach compute-bound execution.

The thesis highlights the importance of **hardware–software co-design** and provides a structured methodology to evaluate configurable vector architectures in controlled environments.

---

## Keywords

RISC-V · Vector Extension · RVV · Vicuna · Embedded Systems · Neural Networks · Performance Analysis · Hardware–Software Co-design