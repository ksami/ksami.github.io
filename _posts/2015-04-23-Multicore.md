---
layout: post
date: 2015-04-23 16:44:43
title: Multicore
tags: [multicore, notes]
---
Notes for multicore computing. Midterm week so there'll be more of these posts coming up!

Some have been translated from Korean as I struggle through my lecture notes so remember to cross-reference.
-----


# Multicore Computing

## Chapter 1 - Overview

### Capabilities
- Supercomputers moving on to operate in peta-flops (10^15)
- Moore's Law: no. of transistors double every year

### Hardware techniques
- Instruction pipeline
- Out-of-order execution
- Superscalar execution
- On-chip cache

### ILP Wall
- Limit to Instruction Level Parallelism (ILP)

### Power Wall
- Instructions per second directly proportional to CPU clock frequency
- Requires more power to run at a higher clock frequency

### Multicore
- Two or more processors on one chip
- Manycore: 8/16 or more cores
- Addresses power and ILP walls

### Homogeneous multicore
- All cores are the same
- Intel Xeon, AMD Opteron, ARM Cortex A15 MPCore

### Heterogeneous multicore
- Different processors on the same chip
- Intel i7, AMD APU, ARM big.LITTLE, Nvidia Tegra

### GPGPU
- General Purpose computing on Graphics Processing Units

### Accelerator
- Attached to systems to increase speed
- GPU, FPGA, Intel Xeon Phi coprocessor

### Amdahl's Law
- Used to find the maximum expected improvement to an overall system when only part of the system is improved

### Programming Wall
- Programmers have yet to efficiently exploit the many number of cores available
- Hardware has advanced much more rapidly than software

### Programming Model
- Programmer -> Programming Model -> Parallel Processing System
- A balance between delivering high performance and ease of programming

#### Shared memory parallel programming model
- OpenMP
- Pthreads

#### Message passing parallel programming model
- MPI

#### Accelerator programming model
- OpenCL
- SnuCL
- CUDA
- OpenMP, OpenACC

===========

## Chapter 2 - Computers and Notation

### Computer
- Input -> Computer -> Output

### Data
- Bit: binary digit (1 or 0)
  - encoded in electronic circuits as a change in voltage
- Byte: 8 bits

### Hardware and Software
- Hardware: physical component
- Software: set of machine-readable instructions
- Programs: sequence of instructions written to perform a specific task

### Radix Notation
- Positional number system
- Binary (base 2), Octal (base 8), Decimal (base 10), Hexadecimal (base 16)
- Conversions between the different number systems
- Signed and unsigned numbers
  - 2's complement representation: flip all bits and add 1

### Modulo
- Divide and take remainder
- Written as `mod`
- eg. 7+3 = 2 (mod 8)


===========

## Chapter 3 - Boolean Algebra and Logic

### Boolean Algebra
- OR, AND, NOT
- XOR, XNOR, NOR, NAND
- Minimise a boolean expression using Karnaugh map

### Logic Gates
- Made of transistors
- Combinations of them implement all Boolean operations
- Gate or propagation delay exists
  - Time taken for signal/changes to travel/propagate down to later stages of the circuit
- Tristate buffer
  - Acts as a switch, output is input only if Enable is 1
- Multiplexer (MUX)
  - Selects/forwards one of many inputs to the output
- Decoder
  - Splits input into many outputs

### Combinational Logic circuit
- Logic of the circuit depends solely on logic gates


==========

## Chapter 4 - Sequential Logic

### Sequential Logic circuit
- Circuit depends on a clock
- Memory exists in the circuit
- Output depends also on previous results and not just inputs

### Clock
- Square wave signal
- Rising edge, falling edge

### Latch and Flip-flop
- Circuit component which has memory
- Component in registers

### Finite State Machines
- Mealy FSM: Output values are determined by both its current state and current inputs
- Moore FSM: Output values are determined solely by its current state

### RAM
- Random Access Memory
- kilo (2^10), mega (2^20), giga (2^30) etc.
- Composed of cells which hold one bit each
- Multiplexers control read/write operations
- Memory access time exists, product of propagation delay


==========

## Chapter 5 - Binary Integer Arithmetic

### Operations
- Uses NOT, AND, OR, NOR, NAND, XOR, XNOR
- In C language, `~`, `&`, `|`, `^` correspond to NOT, AND, OR, XOR

#### Addition/Subtraction
- Uses XOR, AND, OR
- Sign extension for signed arithmetic

#### Multiplication/Division
- Shift right/left by the appropriate number of bits
- Sign extension done on 2's complement representation for signed number arithmetic


===========

## Chapter 6 - Floating Point Arithmetic

### Scientific Notation for Numbers
- `m x b^e`
- `m` mantissa, coefficient
- `b` base, radix
- `e` exponent
- eg. `-0.12345 = -1.2345 * 10^-1`

### IEEE Floating Point Representation
- Exponent E has to be biased
  - E has to be signed
  - If E is in 2's complement it would be difficult to compare
  - When encoding add minimum value of E so encoded E is always positive
- Exponent E = 111..., fraction F = 000...
  - Represents infinity
  - Overflow
- Exponent E = 111..., fraction F not 000...
  - Represents NaN (Not a Number)
  - No numeric value can be determined
- As if computed with inifinite precision then rounded

#### Precision
- Single (32-bit) precision: sign(1), exponent(8), fraction(23), bias=127
- Double (64-bit) precision: sign(1), exponent(11), fraction(52), bias=1023
- Quadruple (128-bit) precision: sign(1), exponent(15), fraction(112), bias=16383

#### Rounding
- Rounding errors since binary can only approximate the true value of a decimal number

#### Normalized numbers
- Ones place of at least 1 and smallest exponent, eg. <= 2^-127 and >= 2^-127

#### Denormalized numbers
- Ones place of 0 and smallest exponent, eg. > -2^-127 and < 2^-127
- For representing numbers smaller than normal (underflow)

### Floating Point Arithmetic
- Add is **NOT** associative ie. (a+b)+c != a+(b+c)
  - Due to overflow and inexactness of rounding

### Fused Multiply-Add
- Multiply and add operation together in one hardware unit
- Matrix dot product uses many multiply and add operations
- Better precision than doing two seperate operations
  - One rounding error compared to two


=============

## Chapter 7 - Von Neumann Architecture

### Von Neumann Architecture
- Four basic hardware components
  - Input devices
  - Output devices
  - Main memory
  - Central Processing Unit (CPU)

### CPU
- Carries out the instructions of a computer program
- Arithmetic Logic Unit (ALU)
- Control Unit (CU) fetches instructions from main memory, decodes and executes them using ALU where necessary

### Stored Program Concept
- Programs treated as data, both stored in main memory
- No need to reprogram computers for each specific task all the time
- Solve different problems by just switching between different programs

### Dependences
- Any ordering of execution that obeys all dependeces will produce the same result as the original program
- Flow dependence
  - True dependence: cannot be removed
  - Read after Write
- Anti dependence
  - False dependence: can be removed
  - Write after Read
- Output dependence
  - False dependence: can be removed
  - Write after Write
- Input not-really-a-dependence
  - For caches
  - Read after Read

### Pipelined Processors
- Increases instruction throughput ie. no. of instructions executed per CPU clock cycle
- Typical five-stage pipline:
  - IF: instruction fetch
  - ID: instruction decode and register fetch
  - EX: execute
  - MEM: memory access
  - WB: register write back

#### Pipline hazards
- Occur when the next instruction does not execute in the next clock cycle
- Data hazards
  - Result needed before it is available
  - To resolve:
    - Stall the pipeline/insert a bubble of no operation
    - Data forwarding
- Structural hazards
  - Hardware component required by more than one instruction at the same time
  - Resource conflict eg. single memory unit accessed in both IF and MEM
  - To resolve:
    - Stall the pipeline
- Control hazards
  - Branches in instruction flow
  - Branch target unknown until instruction reaches MEM stage
  - Instructions fetched after branch should not execute since control has been diverted
  - To resolve:
    - Delay branch scheduling instructions into branch delay slots
    - Not having branches at all
    - Stalling
    - Branch predictors

### In-order Execution
- Fetch and decode next instruction
- If operands available, instruction dispatched to appropriate functional unit
- Else stall until available
- Result written back to register file

### Out of Order Execution
- Dynamic instriction scheduling by hardware
- Fetch and decode next instruction
- Issue to reservation station
- Instruction waits in reservation station until operands available
- Instruction dispatched to functional unit and executes
- Execution completed, result queued in reorder buffer in commit unit
- Commit unit writes results to registers and memory in program fetch order

### Issuing and Dispatching an Instruction
- Instruction that has passed ID stage is issued
- When operands are available, instruction is dispatched to functional/execution unit

### Precise Exception (Interrupt)
- Instructions before faulting instruction are committed
- Instructions after faulting instruction are restarted from scratch

### Retirement (Graduation)
- Instruction commited or removed

### Superscalar Processors
- Dynamically issue multiple instructions in each clock cycle
  - Fetches and decodes several instructions at a time
  - In-order or out-of-order issue
-Exploit Instruction Level Parallelism (ILP)
  - Instructions that can be executed simultaneously
  - Limited amount of ILP in an application

### VLIW Processors
- Static instruction scheduling by compiler
- Very Long Instruction Word
- Programs able to explicitly specify instructions to be executed simultaneously

