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

// use join method to force one thread to wait for another
// thread to finish here the main Thread joins thread4 and waits 
// for it to finish before carrying on.
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
// create new threads as needed but will reuse prevousl 
// constructed thread when available
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

---
# Lecture 3 11/09/2018
## Thread Sychronization
### Race condition
Threads reading the old imput instead of the new one overiding each other. A class is said to be __thread safe__ if an object of the class does not cause a race condition. 

To avoid race conditions we must ensure that no more than one thread enter the critical region of the program. t

### Syncronized in Java
For the method we use that:
```java
public synchronized void deposit(int amount)
public void deposit(int amount)
{
    syncronized(this)
    {
        // method body
    }
}
```
Internally the synchronized method acquires a lock before executing the method, the lock is on the object from which the lock was involve and if we are on a static method the lock is for the entire class.

We can also synchronized block of code
```java
public void test()
{
    synchronized (expr)
    {
        statments or expr.method();
    }
}
```
The expression expr must evaluate to an object reference in order to lock the object.

### Syncronization Using Locks
This is already done implicitly with the synchronized method however this can be done explicitly
```java
import java.util.concurrent.locks.Lock; // (interface)
lock(): void
unlock(): void
newCondition(): Condition
```
This class we use inherits from that and is called
```java
import java.util.concurrent.locks.ReentrantLock;
ReentrantLock()
ReentrantLock(fair: boolean) 
// if fair is true then thread access method by
// first come first (longest waiting thread)
// serve else it is based on priority or random factor
```
#### Creating a lock in Java
```java
import java.util.concurrent.locks.ReentrantLock;
public class AccountWithSync
{
    private Lock lock = new ReentrantLock();
    private int balance = 0;
    public void addPennyTask(int amount)
    {
        try
        {
            lock.lock() // Acquire the lock
            int newBalance = balance + 1;
            balance = newBalance;
        }
        catch (Exception e)
        {
        }
        finally
        {
            lock.unlock(); // Release the lock
        }
    }
}
```

### Conditions
Can be used to facilitate communication among threads. A thread can specify ahat to do under a certain condition. Conditions are objects created by invojing the newCondition() monthod on lock objects
```java
public class Condition
{
    public void await() // current thread waits
    {}
    public void signal() // wakes up one waiting thread
    {}
    public void signalALl() // wakes up all waiting thread.
    {}
}
```
#### Example with withdraw and deposit tasks
```java
import java.util.concurrent.locks.*;
public class DrawWithdraw
{
    private Lock lock = new ReentrantLock();
    private Condition newDeposit = lock.newCondition();
    private int balance = 0;
    public void withdraw(int amount)
    {
        lock.lock();
        while (balance < amount)
            newDeposit.await();
        blance -= amount
        catch ()
        {}
        finally
        {
            lock.unlock();
        }
    }
    public void deposit(int amount)
    {
        try
        {
        lock.lock();
        balance += amount;
        newDeposit.signalAll();
        }
        catch()
        {
        }
        finally
        {
            lock.unlock();
        }
    }
}
```

---
# Lecture 4 13/09/2018
## Java build in lock condition
If we have a syncronized block of code associated with an abject we can use these methods to make the thread wait. They must be call in the syncronized block of code otherwise an exception in throughn.
```
wait()
notify()
notifyAll()
```

```java
syncronized (anObject)
{
    try
    {
        while(!condition)
        {
            anObject.wait();
        }
    }
    catch (InterruptedException ex)
    {
        ex.printStackTrace();
    }
}

syncronized (anObject)
{
    anObject.notify() ;
    anObject.notifyAll();
}
```

## Using different conditions
```java
public class ConsumerProducer
{
    private static final CAPACITY = 1;
    private LinkedList<Integer> queue = new LinkedList<Integer>();
    private Lock lock = new ReentrantLock();
    private Condition notFull = lock.newCondition();
    private Condition notEmpty = lock.newCondition();
    public void write()
    {
        try
        {
            lock.lock();
            while(queue.size() == CAPACITY)
                notFull.await();
            queue.enqueu(0);
            notEmpty.signal();
        }
        catch
        {}
        finally
            lock.unlock();
    }

    public void remove()
    {
        int value = 0;
        try
        {
            lock.lock();
            while(queue.size() == 0)
                notEmpty.await();
            value = queue.remove();
            notFull.signal();
        }
        catch
        {}
        finally
            lock.unlock();
    }
}
```

## Blocking Queues
Is a datastructure that allow syncronized access to it part of the BlockingQueue interface that itself inherits from queue that inherites from collection. There are 3 concrete blocking queue array, linked and priority
```
put(Element: E): void
take(): E
```
### Array blocking queue
```
ArrayBlockkingQueue(capacity: int)
ArrayBlockingQueue(capacity: int, fair: boolean)
```
### Linked
```
LinkedBlockingQueue()
LinkedBlockingQueue(int: capacity)
```
### Priority
Same as linked with priorities
```
PriorityBlockingQueue()
PriorityBlockingQueue(int: capacity)
```

## Semaphore
```
Semaphore(numberofPermits: int)
Semaphore(numberofPermits: int, fair: boolean)
acquire(): void
release(): void
```
## Deadlock
they are used to avoid deadlock when multiple threads need to access the same object at the same time we can have a problem if O1 has A but wants B and O2 has B but wants A.

### Preventing Deadlock
Assign an order on all the objects whose locks must be acquired and ensure that each thread acquires the locks in that order.

## Thread States
- new
- ready
    - Ready state until it starts running
    - yield()
- running
- wait for target to finish (blocked)
    - join()
- wait for time out
    - sleep()
- wait to be notified
    - await()
- finished

### Information on thread state
isAlive() true if ready, blocked, running false if new or finished  

interrupt() interupt the thread, if the thread is in Ready or 
Running then its interrupted flag is set if the thread is
blocked it awakens and enter ready state throwing interupt exeption

isInterrupted() true if interupted false otherwise

## Syncronized collection
The classes in java collection are not thread safe but can be made thread safe data in a collection. The collection class provides 6 methods to wrapp a collection in a syncronized maner.

### Fail-Fast
We can have an issue when using an iterator, using the Collection class we are protected when adding or removing (modifiying) the collection but the iterator itself does not modify the collection hence we must ensure that any iteration occurs in a syncronized fashion manualy.

## Fork Join Framwork in java
ForkJoinPool inherits from ExecutorService
```
cancel(interrupt: boolean): boolean
get(): V
isDone(): boolean
ForkJoinPool()
ForkJoinPool(parallelism: int) define the number of processes we want to create
invoke(ForkJoinTast<T>): T
```

ForkJoinTask abstract implements the Future interface
```
+fork(): ForkJoinTask<V>
+join(): V
+invoke(): V
+invokeAll(ForkJoinTask<a>): void
```

Then extended with RecursiveAction or RecursiveTask
```
#compute()
```
void for action but V for Task for the return value, these must be overwritten in our class as this is the method that runs when performing the task

ForkJoinTask are thread like entities that are light weight threads. 

---
# Lecture 5 18/09/2018
## Moore's Law
The clock speed is now almost flat altought the number of transistors doubles every 2 years. This is because the master a transisitor is used the more it hits up and we have now reached the heat limit. 

## CMP (Chip multi-processor)
All on the same chip we have multiple CPU with multiple cache that access a shared memory. (multi core). 

So now the issue is that we can no longer wait overtime for our processors to be more powerful so that our code runs faster. Now we have to take advantage of the multi core on the chips and split the code so that they can run concurrently on the multicore to speed it up. Like using two processors should allow us to double the speed.  
The issue comes when we have to lock resources for the thread and multiple threads want to access the same resource then there is an idle time for certain threads which hence do not really speed up as expected the runtime of the code.

## Concurrent computation
The issue is that there computation are asynchronous which can case delays such as:
* cache misss (short)
* Page fault (long)
* Scheduling quantum used up (really long)

This is out of our control the JVM or OS is the one that manages that. These are unpredictable delays and we cannot do much about these delays.

### concurency Jargon
Hardware: cores processors  
Software: threads and processes

## Finding prime numbers from 1 to 10^9
We have 10 processors so the first idea is to split the work evenly between the 10 threads, thread 1 gets number from 1 to 10^9, thread 2 from 10^9 to 2*10^9 and so on but this can cause some speed up issues. As some number may take longer to be identified and the number of primes in a segment may vary. 

### Concrency Idea: Counter
```java
Counter counter = new Counter()
void printPrime()
{
    long j = 0;
    while (j<10^10)
    {
        j = counter.getAndIncrement();
        if (isPrime(j))
        {
            print(j);
        }
    }
}
```
In this case this is better than what we had before but we need some sychronization when accessing the counter.

### Single thread Counter class
```java
public class Counter {
    private long value;
    public long getAndIncrement() {
        return value++;
    }
}
```
This can definitly cause some synchronization issues as seen earlier in class. So we would want this action to be done atomically.

### Multiple thread Counter class
```java
public class Counter {
    private long value;
    public long getAndIncrement() {
        sychronized (this) {
        return value++;
        }
    }
}
```
In this case here we have mutual exclusion.
``Question what does syncronized do when accessing static variables``

## Alice and bob pet issue
2 people have 2 pets but there is only one pond we must have an algorithm so that the two pets are not in the pond at the same time. We want to ensure both pets never in the pond at the same time __Safety__ and if both want to get in at least on gets in __liveness__.

Message passing does not work in this case as the other partie may not be there or respond. The communication must be __persistent__ and not __transient__.

Cannot be solved by an interupt system by using a bit in another thread to cause interupts as we may run out of bits.

## Amdahl's law
$$
speedup = \frac{1}{1-p+\frac{p}{n}}\\
p = \text{parallel fraction}\\
n = \text{\# of threads}
$$  
So there is always a sort of bottle neck in case of speed up. The only way to get a speed up equal to the number of threads created we must almost have all the code parallisable. 

Hence our goal is going to minimize the overhead assocated with parallelizing a fraction of the code P. And learn how to introduce parallelism in the 1-p part.