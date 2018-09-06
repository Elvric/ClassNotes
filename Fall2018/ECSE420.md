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

---

# Lecture 2 06/09/2018 Multithreading and parallele computing

## Introduction
Requires to invoque system dependent procedures and functions to implement multithreadung. Java had that from multithreading - The concurrent running of multipl tasks within a program.

## Thread concept
Run multiple threaads on multiple CPU or run multiple thread on a single core machine using time sharing. Thread can be more efficient on a single core if for example we have a lot of IO coming from the app.

### Creating thread and Tasks
```java
public class TaskClass implements Runnable
{
   public TaskClass(...)
   {
   }
      // Implements the run method
   @Override
   public void run() 
   {
      // Tells system how to run custom thread
   }
}

public class client
{
   public void SomeMethod()
   {
      // Create task class
      TaskClass task = new TaskClass(...);
      // Create thread
      Thread thread = new Thread(Task);
      // Start the thread
      thread.start();
   }
}
```
Run should never be invoked directly since that will run the method in the same thread that the call is made. Start on a thread cannot be started twice this my cause an exception if the thread is already running and asked to start again

### The thread class
Subclass of the runnable interface, list non exaustive
```
Thread()
Thread (Runnable Task)
start() void
isAlive() boolean
setPriority(int p)
join()
```
Hence it is possible to do this
```java
public class CustomThread extends Thread
{
   public CustomThread(...)
   {

   }
   @Override
   public void run()
   {

   }
}
public class Client
{
   public void method()
   {
      CustomThread thread = new CustomThread(...);
      thread.start();
   }
}
```
However this is not recomended

#### Methods
```java
// temporarly release time for other thread can help to debug
thread.yield();  

// Sleep the thread of the ammount of milliseconds entered
try
{
   Thread.sleep(long mills);
}
catch (InterruptedException ex)
{

}

// use join method to force one thread to wait for another thread to finish here the main Thread joins thread4 and waits for it to finish before carrying on.
Thread thread4 = new Thread();
try
{
   thread4.join();
}
catch (InterruptedException ex)
{

}

// Priority goes from 1 to 10 normal priority goes from 1 to 5 
// MIN_PRIORITY, NORM_PRIORITY, MAX_PRIORITY
Thread.priority(6);
```

### Thread pools
Pool of available thread that can run different tasks. If we had to create a new thread for each task we want to run could get costly, hence starting and creating fewer thread maybe a better option. Example if we have 8 cores on our machine we may just want 8 threads.

Java provides the executor interface, that has an execute method executing thread and the executorservice has methods to manage the thread in the thread pool. The executorService inherits from the executor interface.

```
Excutor
   Execute(Runable)
ExecutorService
   shutdown
   shutdownNow()
   isShutDown() bool 
   isTerminated() bool
```

#### Creating Executors
use the already existing static class ``Executors``
```java
// Create a fixed number of thread in the thread pool
newFixedTheadPool(NumberOfThreads: int) ExecutorService returned
// create new threads as needed but will reuse prevousl constructed thread when available
newCachedThreadPool() ExecutorService returned
```

```java
import java.util.concurrent.*;
public class ExecutorDemo
{
   public static void main()
   {
      ExecutorService executor = Executors.newFixedThreadPool(3);
      executor.execute(new Runnable('a'));
      executor.execute(new Runnable('b'));
      executor.execute(new Runnable('c'));

      executor.shutdown();
   }
}
```
``if the number of available thread is lower than the number of executes then the some thread will perform two tasks and switch between them randomly``

The idea is if you want just one thread for just one task use the thread class but if you want more thread for more tasks use the thread pool.