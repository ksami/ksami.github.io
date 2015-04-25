---
layout: post
date: 2015-04-26 01:56:08
title: Multicore Pt. 2
tags: [multicore, notes]
---

Talk about unbalanced... [Part 1](/2015/04/23/Multicore.html) was about 300 lines, this part 2 is 700 lines... I am exhausted OTL

Some have been translated from Korean as I struggle through my lecture notes so remember to cross-reference.

-----


# Multicore Computing Pt 2

## Chapter 8 - Cache and Virtual Memory

### Principle of Locality
- Temporal Locality
    - Reuse of data/instructions in the near future
    - Data: Same variable access in each iteration in for loop
    - Instructions: Cycling through for loop
- Spatial Locality
    - Data/instructions in locations near to each other likely to be used together
    - Data: Array elements accessed in succession
    - Instructions: Referenced in sequence

### Memory Hierarchies
- Hierarchical arrangement of storage to exploit locality of reference
- Trade-off between speed, capacity and cost

### Caching
- Exploit temporal and spatial locality
- Cache block = cache line
    - Basic unit of cache storage
    - Multiple bytes/words (Intel Haswell: 64 bytes/line)
- Cache hit: block requested and found in cache
- Cache miss: block requested but not in cache, requires fetching from memory

### Cache Organization
- Set: collection of cache locations in which a fetched block can be placed
- Addressing a word: Tag (part of address of data in main memory) | Set index (set) | Offset (word)

#### Direct-Mapped Caches
- One cache line per set
- Block from main memory can only be placed in one location in cache
- Simple replacement
- Collisions between blocks for the same cache line can occur

#### Set-Associative Caches
- Data block can be placed in a few locations in the cache
- Requires complex replacement policy
- Requires complex tag comparison hardware on lines in a set
- Less collisions between blocks for the same cache line than direct-mapped

#### Fully Associative Caches
- Only one set
- Data block can be in any place in the cache
- Requires complex tag comparison hardware on lines in a set
- Less collisions between blocks for the same cache line than set-associative

### Types of Cache Misses
- Cold (compulsory) miss: when cache is empty
- Conflict miss: when cache is large enough but multiple data items map to the same cache line
- Capacity miss: when working set is larger than cache size

### Replacement Policies
- Least Recently Used (LRU)
    - Replace block that has not been used for the longest time
    - Need to maintain LRU statistics for each cache line in a set
    - Costly, time consuming read/modify/write cycle to maintain set state on a cache access
- Pseudo LRU
    - Binary decision tree to keep track of LRU statistics
    - One write cycle to update stats on a hit
    - One read cycle during line replacement
- First in, First out (FIFO)
    - Replace block that has been in set for longest time
- Random

### Write Policies
- Combine following policies for each case

#### On cache hit
- Write through
    - Both block in cache and lower level memory are modified
    - Simple to implement
    - Lower level memory always consistent with cache
- Write back
    - Block in cache is modified, only written back to lower level memory when replaced, uses dirty bit
    - Harder to implement
    - Lower level memory not always consistent with cache

#### On cache miss
- Write allocate
    - Block is loaded into the cache
- No write allocate
    - Block is directly modified in lower level memory and not loaded into cache

### Non-blocking/Lockup-free caches
- Continues to supply cache hits during a miss instead of being blocked waiting for lower level memory
- Reduces effective miss penalty
- Optional: support multiple outstanding misses by maintaining state for each outstanding miss

### Cache Performance Metrics
- Miss rate
    - Fraction of memory references not found in cache
- Hit time
    - Time required to deliver a line in cache to the processor
    - Includes time to determine if line is in cache
- Miss penalty
    - Additional time required due to miss


### Virtual Memory
- Abstraction of main memory by operating system
- Provides each process with large and uniform address space
- Size of address space larger than main memory
- Protect address space of each process from corruption by other processes
- Main memory treated as cache of permanent secondary storage eg. hard disk

### Pages
- Each byte in main memory has unique physical address
- CPU generates virtual address to access main memory
- Memory management unit (MMU) translates virtual address to physical address using look-up table (page table) stored in main memory

#### Page Tables
- Array of page table entries
- Includes valid bit and n-bit address field (physical page frame number)
- OS (page fault handler) maintains contents of page table and transferring pages between main memory and secondary storage
- Swapping (paging): activity of transferring pages between secondary storage and main memory
- Demand paging: wait until the last moment to swap in a page when a miss (page fault) occurs

#### Address Translation
- 3 types of virtual pages
    + Unallocated: pages not yet allocated by VM due to no space on secondary storage
    + Cached: allocated pages currently cached in main memory
    + Uncached: allocated pages not cached in main memory (residing in secondary storage)

#### Page Replacement Policies
- LRU
- FIFO
- Second chance
- Clock
    + Maintain circular list of pages in memory keeping track of when pages are first loaded in memory and when page is referenced
    + Exploits spatial locality

#### Translation Look-aside Buffer (TLB)
- High overhead when MMU refers to page table for address translation since page table is stored in main memory
- Small, virtually addressed cache with each line holding a block of one page table entry
- Micro-TLB
    + Small TLB placed over main TLB to boost speed of address translation for cache accesses
    + Main TLB handles micro-TLB misses

### Caches and Virtual Memory
- Virtually addressed cache: faster (no address translation) but security issues (OS needs to flush cache on context switch)
- Physically addressed cache: slower but no security issues (no OS intervention)
- Physically indexed, physically tagged
    + No OS intervention
    + TLB in critical path
- Physically indexed, virtually tagged
    + Never used
    + No OS intervention
    + TLB in critical path
- Virtually indexed, physically tagged
    + Common in real world
    + Address translation at same time as cache indexing
    + TLB not in critical path
    + Much faster than physically indexed caches
- Virtually indexed, virtually tagged
    + Address translation occurs on cache miss
    + TLB (address translation) not in critical path


=======

## Chapter 9 - Parallelism

### Types of Parallelism
#### Task Parallelism
- Performing distinct tasks at the same time
- Dividing an application into these separate tasks

#### Data Parallelism
- Loop-level parallelism
- Performing same operation to different items of data at the same time as in a `for` loop operating on data in an array

### SIMD
- Single Instruction Multiple Data
- Performing same operation on multiple data simultaneously with a single instruction
- Single program counter


=====

## Chapter 10 - Multithreaded Architecture, Cache Coherence and GPU Architecture

### Multithreaded Processors
- Issues instructions from multiple threads of control for the pipeline
- To guarantee no dependences between instructions in a pipeline
- Exploit ILP from multiple threads executing at the same time

### Thread-level Parallelism (TLP)
- TLP represented by use of multiple threads of execution
- To improve throughput
- More cost-effective to exploit than ILP

### Superscalar Processors
- Aim to issue an instruction on every clock cycle
- Issue-width: max number of instructions that can be issued by a processor simultaneously
- When hardware can issue `n` instructions on every cycle
    + Processor has `n` issue slots
    + `n`-issue processor
- Inefficiency
    + Vertical waste: An issue slot not having an issue for some time
        * Schedule threads to hide long latency
        * switch different thread contexts on each cycle
    + Horizontal waste: In a particular cycle, not all issue slots are being used
        * Context switch among threads every cycle
        * Context switch among threads every few cycles on data hazards, cache misses etc.
    - Simultaneous Multi-threading
        + Selects instructions for execution from all threads on each cycle to remove horizontal and vertical waste

### Cache Coherence
- Private caches in a multiprocessor system results in copies of a variable present in multiple places
- Write by one processor may not become visible to others
- Coherence problems between I/O devices and processor caches
- Solutions
    + Software-based
        * Compiler/runtime
        * Perfect memory access information needed
    + Hardware-based
        * More common
        * Requests for data always return most recent value
        * Coherence misses due to invalidations
        * Shared caches do not have coherence problem
    + Snooping: each processor snoops every address on bus during a memory read request, if possess dirty copy of requested block, provide it to requester and abort memory access instead
    + Write-through Caches: all processor writes update local cache and global bus write
        * False Sharing: invalidations can lead to different cores writing to different locations in the same cache block repeatedly

### GPU Architecture
- Many simple compute units
- Many threads, no synchronization between threads for graphics applications

#### Shader
- Program that runs on GPU to execute one of the programmable stages in rendering pipeline
- Shader cores are simple, programmable
- Optimized for a single instruction stream
- Graphics pipeline stages implemented by vising shader core several times

#### Characteristics of GPU Applications
- Data independence between triangles/pixels
- Millions of triangles/pixels
- Massively parallel processing possible

#### Exploiting Massive Parallelism
- SIMD processing across many ALUs
- Single program counter across these multiple ALUs
- SIMT: Single Instruction Multiple Thread
- Warp: group of instruction execution instances, all threads in warp execute same instruction at a time, unit of context switch
- Uses hardware context switching to avoid stalls caused by high latency operations
- Branches automatically handled by hardware, conditional execution in a warp

### Multiple Streaming Multiprocessors
- Different streaming multiprocessors execute different shaders
- Simultaneous instruction streams


==========

## Chapter 11 - Memory Consistency

### Memory Consistency
- Memory should respect order between accesses to different locations in a thread
- Reordering instructions may mess this up
- Coherence does not help since it is for a single location

### Memory Consistency Models
- To specify constraints on the order in which memory operations from any processor can appear to execute w.r.t. to one another
- Balance between programming complexity and performance

#### Sequential Consistency
- Observable outcome of multi-threaded program should be the same as outcome of a single thread executing all operations in the same program
- Total order between operations is defined assuming atomic operations
- Inefficient with hardware implementation
- Program Order
    + Order in which operations appear in source code
    + At most one memory operation per instruction
    + Not the same as order presented to hardware by compiler

#### Relaxed Memory Consistency Model
- Relaxation in program order, only need to preserve dependences
- Difficult for the programmer
- Memory fence/barrier instructions prevent reordering of instructions

##### Processor Consistency
- Before a read is allowed to perform w.r.t. any other processor, all previous reads must be performed
- Before a write is allowed to perform w.r.t. any other processor, all previous accesses must be performed
- Relaxing write->read order
    + Allow read to complete before an earlier write in program order
    + Write buffer with read bypassing

#### Weak Ordering
- Relax all program orders
- No program orders are guaranteed by underlying hardware or compiler optimizations
- Only synchronization operations
    + Need to distinguish between ordinary read/writes and synchronizations
- Before r/w allowed to perform w.r.t. any processor, all previous sync operations must be performed w.r.t. every processor and vice-versa
- Sync operations obey sequential Consistency
- Multiple r/w requests outstanding at the same time to hide r/w latency
- Ordering w.r.t. sync must be guaranteed

#### Release Consistency
- Relax all program orders but not w.r.t. sync operations
- Two separate sync operations
    + Acquire: a read operation such as lock
    + Release: a write operation such as unlock
- Need to distinguish between ordinary r/w and sync and between acquire and release operations
- Before r/w allowed to perform w.r.t. any processor, all previous acquires operations must be performed w.r.t. every processor and vice-versa with release
- Acquire and release operations are sequentially consistent
- Specific memory access ordering w.r.t. acquire/release must be guaranteed
- Memory access inside and outside of the critical section do not delay each other


===========

## Chapter 12 - Process and Thread

### Process
- Instance of a computer program being executed
    + Stream of instructions being executed
    + Abstraction used by OS
- Consists of 
    + Registers
    + Memory (code, data, stack, heap etc.)
    + I/O status (open file tables etc.)
    + Signal management info

### Supervisor Mode vs. User Mode
- Modern processors have two different modes of execution
    + Supervisor (kernel) mode
        * Any instruction can be executed
        * For the OS kernel
        * eg. I/O instructions
    + User mode
        * Only a subset of instructions allowed to execute
        * For any other software other than kernel

### System Calls
- Program running in user mode requesting a service from the OS
- Implemented with a trap/exception/fault
- Triggers switch to kernel mode to perform requested action

### Uniprogramming vs. Multiprogramming
- Uniprogramming
    + Only one process at a time
    + Poor resource utilization
    + eg. DOS
- Multiprogramming
    + Multiple processes at a time
    + Increased resource utilization
    + e.g modern OSes like Windows, Linux etc.

### Virtual Memory
- OS abstraction of physical memory
- Provides each process with illusion of exclusive use of memory and a larger memory space than is actually available
- Logical (virtual) address
    + Address generated by CPU
    + Logical address space - set of all logical addresses generated by a program
- Physical address
    + Address used by physical memory
    + Physical address space - set of all physical addresses corresponding to the logical addresses
- Virtual memory in OS in charge of run-time mapping from virtual to physical addresses using MMU for fast translation

### Address Space of a Process
- Each process has its own private (virtual) address space
- Process cannot affect state of another process directly
- Memory protection
- Kernel address space vs. user address space

### Communication between Processes
- Coordination between processes through r/w to location in shared address space
- Also through Inter-Process Communication (IPC) mechanism
    + Exchange data without sharing virtual address space
    + Expensive

### Concurrency
- Computer system is typically multiprogramming
- Concurrently, kernel processes execute system code while user processes execute user code
- Instructions of one process interleaved with those of another process
- Using context switches

### Context Switch
- CPU scheduler in OS switches between processes
- Context is info allowing the continuation of a process later

### Process State Transitions
-  Running: using CPU
-  Ready: no CPU available
-  Blocked: waiting for event eg. I/O to occur

### Preemptive vs. Cooperative
- Preemptive multitasking
    + Permits preemption of tasks
    + All processes will get some CPU time
    + More reliably guarantees each process a slice of operating time
    + Supported by nearly all modern OSes
- Cooperative multitasking
    + Tasks explicitly programmed to yield when they do not need system resources (eg. CPU)
    + Rarely used

### Threads
- Independent Fetch/Decode/Execute loop
- Smallest unit of processing that can be scheduled by OS
- Consists of
    + Code
    + Registers
    + Stack
    + Thread-local data
- User-level thread vs. kernel-level thread
- Threads contained in a process
- Multiple threads can exist in the same process
- Shares resources with other threads
    + Code
    + Data
    + OS resources: open files, signals etc.

### Communication between Threads
- Multiple threads within a process share portions of virtual address space of process by default eg. code and data sections
- Coordination between threads in same process through r/w variables allocated in shared space
    + Writes to shared address visible to every thread

### Thread Library
- Provide API for creating and managing threads
- User-level library entirely in user space with no kernel support
    + Threads occur in user space
    + Threads managed by runtime library
- Kernel-level library supported directly by OS
    + Each thread has its own execution context
    + Threads managed by OS
- POSIX Pthread

### Linux Schedulers
- Completely Fair Scheduler (CFS)
- No distinction between processes and threads when scheduling
- To maintain fairness
- Run-queue for each processor containing processes whose state is 'ready'
- Nice values used to weight processes

#### Time Slice
- Time interval a process can run for without being preempted
- Proportional to processes' weight

#### Virtual Runtime
- Measure for amount of time provided to a process
- The smaller a process' virtual runtime, the higher its need for the processor
- Cumulative execution time inversely scaled by its weight

#### Red-black Tree
- Self-balanced tree
- No path in the tree will ever be more than twice as long as any other
- Operations on the tree occur in O(log n) where n is number of nodes in the tree
- CFS maintains red-black tree for run-queue for each processor

### Scheduling Domains
- Set of processors whose workloads should be kept balanced by kernel
- Partitioned in one or more groups
- Hierarchically organized: top scheduling domain is the set of all processors in the system

### Run-queue Balancing
- Perform load balancing on each re-balancing tick using push migration
- Check hierarchically if a scheduling domain is significantly unbalanced
    + Find busiest run-queue in the domain and migrate processes to another

#### Push vs. Pull
- Push migration
    + Specific process periodically checks load on each processor
    + Re-distributes by moving/pushing processes to less-busy processors
- Pull migration
    + Idle processors pull waiting processes from a busy processor
- Linux scheduler implements both techniques

### Negative Aspect of Process Migration
- New processor's cache is cold for the migrated process
- Needs to pull its data into the cache


===========

## Chapter 13 - High Performance Computing System Architecture and Parallelization Patterns

### Symmetric Multiprocessing
- Two or more identical processors connect to a single, shared main memory, have full access to all I/O devices, and are controlled by a single operating system instance that treats all processors equally
- Architecture of most modern multiprocessor systems

### Cluster
- Nodes comprising processors and main memory
- Many nodes connected together using an Interconnection Network
- Architecture of many High Performance Computing systems

#### Interconnection Network
- Connects nodes in a cluster together
- Commonly using Ethernet or InfiniBand
- HPC usually using 10-gigabit Ethernet
- InfiniBand used by HPC and data centers

#### Advantages of Clusters
- High performance utilizing existing systems
- Fault tolerance since system continues working even with a malfunctioning node
- Scalability
- Centralized management

### Heterogeneous Clusters
- Each node comprised of CPU and GPU/co-processor

### Massively Parallel Processing
- Large number of processors being used to executed one program
- MPP aims to effectively exploit parallelism of thousands of nodes in clusters

### Parallelism Patterns
- Control dependence: `if` statement
- Data dependence: flow, anti, output dependences
- Loop-carried dependence: dependence exists across iterations ie. if loop is removed dependence no longer exists
- Loop-independent dependence: dependence exists within an iteration ie. if loop is removed, dependence still exists

### Conditions for Parallelism
- Reordering of instructions produces the same result as the original program

### Matrix and Vector Multiplication
- Good problem for parallelization
- Few dependences
- Nested `for` loop

### Map
- Simple operation applied to all elements in a sequence
- Split a task into many independent sub-tasks which require no synchronization/communication between each sub-task

### Reduction
- A summary operation, the opposite of map
- Combines many sub-results into one final result
- eg. add, multiply, min, max, count etc.

### MapReduce
- A Map operation followed by a Reduction
- Exploits parallelism in sub-tasks


==========

## Chapter 14 - Synchronization and Parallelization Considerations

### Need for Synchronization
- Maintain sequence between processes and threads
- Achieve mutual exclusion

### Barrier
- Barrier halts execution of a thread until all specified threads have reached the barrier
- Implemented in code

### Race Condition
- When two or more threads are attempting to access the same resource eg. r/w to the same variable
- Result is non-deterministic (different runs, different results) and depends on relative timing between interfering threads
- Solutions
    + Busy-wait: One thread keeps checking if the other thread has left the critical section before proceeding
    + Lock-free algorithms

### Thread Safety
- Multiple threads can execute the same piece of code without conflicts
- Thread-safe library

### Atomicity
- Operation is atomic if either the entire operation succeeds or it entirely fails

### Mutual Exclusion
- No two threads are in their critical section at the same time
- Concurrency control to prevent race conditions

### Lock
- Atomic operation
- Lock variable with two states, locked and unlocked
- Thread wanting to enter critical section has to set lock variable to lock, when leaving unlock
- Thread not able to set lock variable to lock has to wait

#### Spinlocks
- Thread requesting lock spins constantly checking the lock until it is released
- Thread proceeds as soon as lock is released
- Saves time for locks held for short time since no context switching
- Wastes CPU cycles spinning
- Cannot be used on uniprocessor systems
- Uses test_and_set()
    + Tests a memory location L for false
    + If L == false, set to true, return false
    + If L == true, return true
    + Uses two while loops with inner loop checking the pointer to avoid performance degradation due to frequent memory accesses

#### Sleeplocks
- Thread requesting lock is blocked and put back on ready queue once lock is released
- Can be used on uniprocessor
- Saves CPU time on locks held for long time

### Pros and Cons of Locks
- Easy to understand and use
- Is expensive
- Can result in
    + Deadlock
    + Priority Inversion

### Priority Inversion
- Only one processor
- Lower priority thread preempted by higher priority thread while in its critical section, higher priority thread will wait forever for suspended lower priority thread to release the lock
- Solutions
    + Disable preemption while lock is held
    + Priority inheritance

### Semaphores
- Semaphore S is a variable that takes only non-negative integer values
- Manipulated by two atomic operations P (down) and V (up)
    + P(S): If S>0, P decrements/downs S and returns; else suspends until S becomes nonzero
    + V(S): V increments/ups S and returns
- To schedule shared resources
- Thread suspended on a semaphore does not need to execute instructions checking variables in a busy-wait loop

#### Binary Semaphore
- Value initially 1, always either 0 or 1

#### Counting (general) Semaphores
- Value any integer greater than or equal to 0
- Represents resource with many units available

### Producers and Consumers
- Producer threads create data element
- Consumer threads receive data element and perform computation on the element

#### Single Producer, Single Consumer
- Use circular queue to store data elements
- For asynchronous communication between the producer and consumer
- Producer appends to tail of queue if not full
- Consumer removes from head of queue if not empty
- No. of elements currently in queue and total number of available slots in queue are implemented as counting semaphores

### Non-blocking Synchronization
- Algorithm is non-blocking if suspension of one or more threads will not stop progress of other threads
- Blocking is undesirable
    + Nothing is accomplished while blocked
    + Coarse-grained locking reduces opportunities for parallelism
    + Fine-grained locking increases overhead
- Non-blocking synchronization algorithms exist but are hard to understand
- Advantages
    + No deadlocks
    + No priority inversion
    + No performance degradation from
        * Context switching
        * Page faults
        * Cache misses

### Considerations for Parallelization
- Potential for parallelization in a program
    + Amdahl's Law
    + Speedup determined by sections which have to be sequentially executed
- Locality
    + Best to use data while it is in the cache
    + Exploit spatial and temporal locality where possible
- Load Balance
    + Load needs to be distributed evenly across processors
    + Total time taken for execution depends on the slowest/busiest processor
- Parallelization Overheads
    + Costs for starting multiple threads
    + Costs for communication
    + Costs for synchronization
    + Extra computation for parallelization

### Scalability
 - Suitably efficient and practical when applied to cases with large number of processors
 - Overhead increases as number of processors increases
 - Strong scaling: how the solution time varies with the number of processors for a fixed total problem size
 - Weak scaling: how the solution time varies with the number of processors for a fixed problem size per processor


=========

## Chapter 15 - Hardware Support for Synchronization

### Deadlock and Livelock
- Deadlock occurs when each thread in a set is waiting for an event only another thread in the set can cause
    + No possible execution sequence will ever succeed
- Livelock occurs when states of threads constantly change with regard to one another but none progress
    + Possible execution sequences exist in which no thread ever succeeds

### Deadlock
- Four necessary conditions for deadlock
    + Mutual Exclusion: at most one thread holds a resource at any one time
    + Hold and Wait: threads already holding resources attempt to hold new resources
    + No Preemption: thread already holding resource has to voluntarily release it
    + Circular Wait: circular chain of threads requesting resources held by next thread in the chain

### Mutual Exclusion Problem
- Each thread executing instructions in an infinite loop
- Satisfies following requirements
    + Mutual Exclusion: No two threads in critical section at the same time
    + Deadlock-freedom: If some threads are trying to enter critical section, one thread eventually enters
    + Starvation-freedom: If a thread is trying to enter its critical section, it must eventually enter
    + In absence of contention for critical section, thread trying to enter critical section will succeed

### Dekker's Algorithm
- Between two threads
- Right to enter critical section explicitly passed between threads

### Bakery Algorithm for Two Threads
- Does not have a variable that is both read and written by more than one thread
- Thread wanting to enter critical section required to take numbered ticket whose value is greater than values of all outstanding tickets
- Thread waits until its ticket has the lowest value
- Not practical for more than two threads

### Hardware Support for Mutual Exclusion
- All architectures support atomic read and write operations
- Difficult to solve mutual exclusion problem with interleaved individual read and write instructions
- Read and write can be combined into a single atomic instruction

#### Atomic Swap
- Takes a register and memory location
- Exchanges the value in register for value in memory location

#### Test and Set
- Takes a memory location
- If value in memory location is false, set memory location to true, return false
- Else return true

#### Fetch and Add
- Takes a memory location and a value
- Increments value in memory location by given value, return old value of memory location

#### Read-Modify-Write
- Takes memory location and function `f`
- Read value in memory location, writes new value `f(value)` into memory location, return `void`

#### Compare and Swap
- Takes a memory location and two values `expected` and `new`
- Compares value in memory location with `expected`
- If equal, modifies memory location to `new`, return true
- Else return false


===========

## Chapter 16 - Optimization in the Memory Hierarchy

### Parallel Merge Sort
- Each child processor provides sorted result to parent processor

### Matrix Multiplication
- Need to consider
    + Total cache size to exploit temporal locality and keep working set small
    + Cache block size to exploit spatial locality
- Between N * N matrices: O(N^3)

### Layout of C Arrays in Memory
- C arrays allocated in row-major order
    + Each row in an array in contiguous memory locations
- Stepping through columns in one row accesses successive elements
    + If block size B > 8 bytes, exploits spatial locality
    + Compulsory miss rate = 8/B
- Stepping through rows in one column accesses distant elements
    + No spatial locality
    + Compulsory miss rate = 1 (100%)

### Reuse and Locality
- Reuse occurs when accessing a location that has been accessed in the past
- Locality occurs when accessing a location currently in the cache
- Locality only occurs when there is reuse

### Strip Mining
- Breaks loops into pieces eg. instead of one loop from 0-11, 4 loops each taking 3 elements

### Tiling (Blocking) for Matrix Multiplication
- Algorithm accesses all elements of second matrix b column by column
- Elements of a column stored among N different cache lines
- If cache is big enough to hold N cache lines and no lines are replaced, no cache miss when reading b
- Since all of a, b, c cannot fit in cache, choose block size B such that it becomes possible to fit one block from each of the matrices in the cache
    + To improve spatial and temporal locality
- Sub-blocks can be treated like scalars
