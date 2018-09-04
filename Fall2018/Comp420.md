# COMP 420 Parallel computing
---

# Lecture 1 04/09/2018
## Cours info
Tutorial/lab sessions are the same  
Office hours: TRF 1:00 - 2:00 ENGMC 625  
Recomended book: Art of Multiprocessor Programming (Revised First edition) M. Herlihy and N. Shavit  

## Assignments
3 Assignments (3 or 4 weeks per assignements)

## Terminology
- Concurrency: Correctly and efficiently controlling access to shared resources. 
- Parallelism: Using additional resources to produce answers faster.

### Goal
Solve larger problem faster with more paralleism and concurrency.  
Granulariy of parallelism: How big are the paralelle parts.
instructions, blocks, loop iterations methods, ... 
``on this course we will focus on threads``

### Thread
A thread is a unit of execution managed either by the runtime system or the OS.

## Course objectives
- Programming moedls and methods
- Principles of parallel / concurrent algortithm design
- Parallel computer architectures
- Parallel computer algorithms and data structures

## Before mutlicore implicit parallelism
### Pipline parallelism
Build in the hardware instructions are pipline into bits and executed one at a time.
### Explicit parallelism
SIMD extentions process data in a vector fation and added up in parallel rather than one by one
### Superscalar design
Look at past preducted braches and guess where the next branch is. Dynamic scheduling.

## Limits to instruction level parallelism
Delays go up as the queue fill up.  
Code with difficult to predict branches  
Dependencies in instructions order  
Instructions requiering the same resources  

## Sources of wasted slots
### Memory Hiarchy
Cache miss
### Control flow
Waiting for branching conditions, branch miss prediction
### Instruction stream
Instruction dependencies  
Memory conflict

### Conclusion
Highly underutilized processor 19% only of the time <1.5 IPC (Instruction per second)  
Clock frequency is limited by power and heat. We have reach the thermal wall so we must obtain more computing power by using more processors.  
So now the frequency has plateaued the transistors are still being doubled and we are now starting to increase the number of cores.

Memory latency: CPU rates have increased 4x faster than memory access speed. Multicore processors makes things harded since we are accessing memory at the same time.

## Chalenges

### Sophisticated applications
Flight stimulation require a lot of computing power, as the system evolve the computation load on each processor used might vary so we must equilibrate that somehow. As the scale of the process increases the time difference might go from 2 extra seconds to 2 extra days.
