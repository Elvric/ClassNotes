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

---
# 20/09/2019
## Mutual exclusion
- Focuse first on 2 threads
- Then for n threads
- Fair solutions
- Inherent costs

### Event
An even is a is something a thread A does and happen instentaneously at a specific time for 0 amount of time. If two events happen simultaneously one must happen before the other.
Invoke a method, returning of a method, assign local variables, assign shared variables.

### Thread
A is a sequence of events. We can think of threads of state machine where the thread is in a state and after an event we have a transition to a new state.

### Thread state
We can look at the program counter to know what instruction is going to be read next.

### Concurency
Events of two different threads interleaved. Still one happen before the other

### Interval
We take to events and the time between these two events represent the interval. Intervals may overlap or be disjoint.

### Precedence
Interval A precede interval B if interval A happens before interval B. 

### Repeated events
To repeat an event we just use a super script to indicate the number of time an event a happened.

### Deadlock free
If a thread calls lock and never returns means that other thread must compete lock and unlock infinetely many times. System as a whole makes progress even if an individual starve.

## Mutual exclusion on 2 threads
```java
class LockOne implements Lock {
    private boolean[] flag = new boolean[2];
    // Thread ID is either 0 or 1
    public void lock() {
        int i = ThreadID.get();
        int j = 1 - i;
        // if i is the current thread then j is the other
        flag[i] = true;
        while (flag[j]) {};
    }
}
```
Assume concurency
$$ CS_A^j \\
CS_B^k
$$
That means that flag[A] was set to true and then flag[B] was false and flag[b] was set to true and flag[A] was false. Hence we can assume that both thread read the flag variable of the other before the other had the chance to write to the flag variable. Then looking at the code we can see that there is hence a contradiction.

Deadlock freedome however is not safe in our case as both of them could raise their flag and end up in deadlock.

```java
public class LockTwo implements Lock {
    private int victim;
    public void lock() {
        victim = i;
        while (victim == i) {};
    }
}
```
Satisfies mutal exclusion but does not satisfy deadlock free. Sequential execution deadlocks.

### Peterson's Algorithm
```java
public void lock() {
    flag[i] = true;
    victim = i;
    while (flag[j] && victim == i) {};
}
public void unlock() {
    flag[i] = false;
}
```

It satisfies mutual exclusion through the order of execution in the code. And is deadlock free for the following reasons: solo other's flag is false, duo just one thread at a time can be the victim.

Starvation free. As when one thread reads the while loop it does so over and over, when the thread that wants to enter the lock then it will be the victim hence then the other thread that was waiting will get in.

## Filter algorithm that works for n threads
There are n-1 waiting rooms, at each level at least one thread gets through and at least one thread gets blocked. Only one thread makes it through.
```Java
public class Filter implements Lock {
    int[] level;
    int[] victim;
    public Filter(int n) {
        level = new int[n];
        victim = new int[n];
        for (int i = 0; i < n; i++) {
            level[i] = 0;
        }
    }
    public void lock() {
        for (int L = 1; L < n ; L++) {
            level[i] = L;
            victim[L] = i;
        }
        while (((exists k != i) level[k] >= L) && victim[L] = i) {};
    }
}
```
The only difference with Pererson's in terms of usefullness is that the system is not fair, threads can be taken over by overthreads. As if A starts before B then A should enter before B.

---
# 25/09/2018
The filter lock satisifies unbounded waiting as in our case r is unbounded. 
$$ CS_A^k \rightarrow CS_B^{j+r}$$

## Fairness
No one starves, thread enter in a predefined order established either on the arrival of the thread or before the arrivale of the thread.

## Bakery algorithm
```java
public class Bakery {
    boolean[] flag;
    Label[] label;
    public Bakery(int n) {
        flag = new boolean(n);
        label = new Label();
        for (int i=0; i < n; i++) {
            flag[i] = false;
            label[i] = 0
        }
    public void lock() {
        flag[i] = true;
        label[i] = max(label[0],label[1],...)
        while(exists k flag[k] && (i,label[i]) > (k,label[k])) {}
    }
    public void unlock() {
        // Note that labels always keep increasing
        flag[i] = false;
    }
}
```
### No deadlock
There will always be 1 thread with the lowest label and if there is a tie then we still let one of the thread in.

### First come first serve
If we assume that thead A enters before thread B then when thread A wants to renter the critical section it must have a higher label than thread B.

### Mutual exclusion
Suppose 2 threads end up in the critical section. If we assume one thread has an earlier label it should have a smaller label and enter the critical section before it or if it had a bigger label then it should not have been able to enter the critical section.

### Problem
Comes as the number keeps increasing at a certain points there will not be enough bits to go to the next number hence we get overflow and the number becomes negative.'

### Time stamps
Label variable is really a timestamp. And we must be able to generate other time stamp, compare them, generate later time stamps. 

In theory it is possible to create a syncronized method to generate time stamps that is wait-free, concurrent and never overflow.

THe bad new that is that it is hard to create an wait free and concurent system for that.

## Time stamping system
### Sequential idea
Think of time stemps as precedence graph, we have a bunch of node and if a node has an edge going to another node means that this node dominates other node (has to wait for the other nodes before doing anything here the word dominate is counter intuitive).

In order for that to work for thread systems each node actually represents a set of circular node. The formula is:
$$ T^k = T^2*T^{k-1}\\ \log_2(3^k) \approx 2^k$$

### Shared memory
Registers themselves that store memory come in different forms:
- Multi-Reader-Single-writer(flag[])
- Multi-Reader-multi-writer(victim[])
- Not that interesting: SRMW, MRMW
  
## Proving that we need at least n SWMR register for mutual exclusion with n threads.
In our case lets assume that we can achieve mutual exclusion for n threads with n-1 registers. N-MRSW, we have 3 threads [a,b,c]. Assume that thread a goes in the critical section and b tries to enter the critical section. Then assuming that b and c are linked to a SWMR register then a cannot inform b that it is in the critical section hence the algorithm cannot work

### Proving the same fact for n with MRMW
Now we have two threads [a,b] assume that just one MRMW register. Assume that thread b has to write to the register before entering the critical section but before it can write the tread gets suspened. Thread a comes allong writes to the register and goes into the critical section. Then thread b jumps back into action and writes then enter the critical section. So that proves the fact for MRMW.

### Bakery algorithm register
Uses 2n MRSW register. And is the smallest number for FIFO that we can reach.

### Summary
In order to do better than this we need to have a better hardware that allow us to perform other operations than just read write. 

---
# 27/09/2018
# Concurrent objects
FIFO queue in a multi threaded environment

### Use locks
We have a circular array lets say and any thread that wants to access such an array has to modify acquire the lock.

### Two threads no waiting
In this case we have 2 threads one only enqueues the other one only dequeus and we let them access the array at the same time.

## Object specification
### Correctness and Progress
We need to specify both safety and the liveness properties of an object. We need to define a way to see an implementationa and the condition in which the algorithm is going to garentee progress.

## Sequential objects
Each object has a state, given by a set of fields. And each object has a set of methods used to manipulate the state.

### Sequential specification
if (precondition) the object is in such state  
Then (postcondition) the method will return a particular value  
and (postcondition) the object will be in a different state at the end of the method.

In our case:  
- Precondition:  
    - queue is empty  
- Postcondition:  
    - Throughs empty excetpion  
- Poscondition:  
    - State unchanged

This is easy since each method can be described in isolation, state of the object is only meaningfull between method calls. We can add a new method without changing the descritption of new methods.

## Concurrent objects
### Concurrent specifications
Not the same can be said.  
Method take time in real life which can cause problems as two methods perform actions on the same object. Since there can be an overlap that could mean that such an object never exists between method calls.  
Everything can potentially interact with other things which means that even the documentation can be more complex when adding a new method.

#### Big question
What does it mean for a concurrent FIFO queue. It means strict temporal order but with concurrency we have an ambiguous concurrent order since methods overlap.

### Linearizability
Each method should:
- take effect
- Be instentaneous
- Between invocations and response events

An object is correct if its sequential behavior is correct any such object is linearizable. This means that all possible executions of an object are linearizable.

### Formal model of execution
We are going to split method calls into 2 events. Invocation and response.

### Annotations
Invocation: A q:enq(x) (A = thread, q = objects, enq = method, x=arguments)    
Response: A q: void (A = thread, q = objects, void is return)  
Match: Is said when an invocation and response event match on thread name and object name.

### History of execution
Represents a vew that we have from the outsided world where we can see everything that happens to the program. History can be denoted as H. If we want to see what happens to object q then we project the history onto the object H|q. We can do projections on threads as well. An invocation is pending if it has no matching response and pending otherwise. The sequential history occurs when it looks like there is no intervals between matching and method calls.

---
# 02/10/2018
### Linearization formal description
An invocation is pending if an invocation has no matching response in the history we have.  
We could discard that invocation to make the thread subhistory complete.

### Sequential history
Happens when thread method calls get their output instantly in a sequential order directly in the history

### Well-formed history
Is a history where if we project the history onto a given thread it will look sequential to that thread.

### Equivalence history
Thread sees the same execution sequence in two different histroy's.

### Legal history/Composability theorem
H is legal if for any object x H|x is a sequential spec for x.

### Precedence
We say thread A preced B If the method call and return of thread A happens before the method call of B.

## Notation
$$ m_0 \rightarrow_H m_1$$
If m_0 precede m_1 in the history

## Linearizability
History H is linearizable if it can be extended to G by, appending 0 or more responses to the pending invocaions, discaring other pending invocation. Such that G is a subset of S where S is legal sequential history.

## Locking and Linearization
Here we could say that the linearization point or end point of the method call for a given object is when the thread unlocks. (Provided that the locking mechanism makes sense)

## Sequential consistency
Same as the linearizability except that now we do not require G to be a subset of S. We no longer need to respect the sequential ordering of thread method calls since hardware can reorder instructions.

However this does not result in composable systems.

## Instruction reordering at hardware level
Compiler are allowed to reorder information when the memory instructions are different and there is no concurrency.

### Memory barriers
Instructions that can ask the hardware not to reorder the memory accesses. This make cost us some more time though. In java this is done by declaring a volatile variable to ensure that it happens in program order for that variable.

---
# 04/10/2018
## Progress condtions
Deadlock-free: some thread that tries to acquire a lock will get it.  
Starvation free: Every thread that wants to acquire the lock will eventually get it.  
Lock-free: some thread calling method will eventually return.  
Wait-free: every thread calling a method will return at some point.

# Fundation of shared memory
### Register
So far we know that a thread can read or write to a register. 
```java
public interface Register<T> {
    public T read();
    public void write (T object);
}
```
There are single read single writer register.  SRSW
Multi reader single writter register.  MRSW
Mult reader, multi writer register. MRMW

## Safe registers
If there exists some valid sequence of actions such that the read write can occur. Some valid value is read when overlapping

## Regular register
When there is an overlap it returns a value that was neither the old nor the new value then it is not regular. It is important to realize that regular is not linearizable.

## Locking within registers
Not interested to rely on mutal exclusion for registers. We want the register to achieve mutual exclusion.

### MRSW safe boolean register safe
```java
public class SafeBoolMRSWRegister implements Register<Boolean> {
    public boolean read();
    public void write (boolean a);
}
```
One way of achieving that is to use SRSW safe registers and multiply them so we have enough registers for the number of reading thread. Then we add only on writter thread. So when the writter threads wants to write, we make it wirte the new value to all the SRSW registers. When done every other thread will read the new values

```java
public class SafeBoolMRSWRegister implements BooleanRegister {
    private SafeBoolSRSWRegister = new SafeBoolSRSWRegister[N];
    public void write (boolean a) {
        for (int i = 0; i < n; i++)
            r[i].write(a);
    }
    public boolean read() {
        int i = ThreadID.get();
        return r[i].read();
    }
}
```

### MRSW regular register
In this case if we write a 0 to the regiter that already contains a 0 other register could read a 1 which is not allowed for regular register. Our solution is to keep the value of the register saved else where for the thread such that when the thread is about to write something it can check wether or not it is useful to write that new value.
```java
public class RegBoolMRSWRegister implements BoleanRegister {
    private SafeBoolMRSWRegister value;
    threadLocal boolean old;
    public void write (boolean a) {
        if (old != a) {
            value.write(a);
            old = a;
        }   
    }
    public boolean read() {
        return value.read();
    }
}
```
### Regular Multi-Value MRSW registers
We are going to represent m Values in the register for example 1001010 then bit[0] = 1 bit[2] = 3 and so on so we have an array of bits. When a writer thread wants to write a number then it goes to its coresponding bit after that it writes 0 backwards.

```java
public class RegMRSWRegister implements BoleanRegister {
    private RegBoolMRSWRegister[M] bit;
    public void write (int x) {
        this.bit[x].write(true);
        for (int i = x-1;i i >= 0; i++) 
            this.bit[x].write(false);
        }   
    }
    public boolean read() {
        for (int i = 0 ; i < m; i++) {
            if (this.bit[i].read())
                return i;
        }
    }
}
```

### Atomic SRSW from SRSW regular
We are going to use a time stamp. The writer writes a value but uses a counter to indicate the order in which writes happen. When a reader reads, it saves the time stamp and last value.

This is there to stop the following situations, one register gets written to and during that time the same register reads it twice during that time. This is when we use the timer, if the time stamp is smaller than the time stamp of that register alredy has then it will not read the regiter as it would read an old value.

---
# 09/10/2018
### Atomic SRMR
We solve that by using a 2 dimensionaly array of register where each thread gets its own column to write and its own row to read. So the writer thread gets column 0 and the other thread reads every row and writes to their own column to inform threads they read a new value. If a thread reads an old value while a thread writes it is fine as we can still linarize it since a thread does not return until it finishes writting to its row.

### MRMR atomic FROM MR atomic
Each writter comes allong and read the register time stamp, each writer reads all then writes Max+1 to its register. If two time stamp overlap then they use the thread ID to break the tie. Otherwise they always read the value of the latest time stamps.

### Atomic snapshot
How far can we push this idea before we reach a plateau.
A thread wants to read multiple registers and get an atomic snapshot of what happends at a certain time.
- Array SWMR atomic registers
- Take instantaneous snapshot of all
- Generalizes to MRMW registers

```java
public interface Snapshot {
    public int update(int v);
    public int[] scan();
}
```
- Collect read the values one at a time
- Problem incompatible concurret collects result not linearizable.

#### Clean collects
Collect during which nothing changes if we go back and read again is it possible to detect that?

The idea is to use time stamps, scan once then go back and do another scan if we see the same value with the same time stamp on the second run that means that we have a clean collect. (we use time stamps as otherwise a clean collect may be artificial not really clean the same value was seen yet it was not written at that same time)

However this does not garentee that we can get a clean collect.

```java
public class SimpleSnapshot implements Snapshot {
    private AtomicMRSWRegister[] register;

    public void update(int value) {
        int i = Thread.myIndex(x);
        LabelValue oldValue = register[i].read();
        LabelValue newValue = new LabelValue (oldValue.label+1, value);
        register[i].write(newValue);
    }

    private LabelValue[] collect() {
        LabelValue[] copy = new LabelValue(n);
        for (int i = 0; i < n; i++) {
            copy[i] = this.register[j].read();
        }
        return copy;
    }

    public int[] scan() {
        LabelValue[] oldCopy, newCopy;
        oldCopy = collect();
    }
    collect: while(True) {
        newCopy = collect();
        if (!equals(oldCopy, newCopy)) {
            oldCopy = newCopy;
            continue collect;
        }
        return getValues(newCopy);
    }
}
```

### Wait free snapshot
The idea is to include a scan along with any update that we do.
A scans once, then looks at B scan realise the the B row was updated, then if we collect again and B is updated again that means that the second write happened during A scan so we can update that cell to the new value in A scan. The idea is that every writter takes a snapshot of the array before writting.

#### Observations
We have unbounded counters which could cause overflow. 

## Summary
We saw we could implement MRMW multi valued snapshot objects
From SRSW safe registers. But what is the next step to attempt with read-write register?  

The __great challenge__ what about doing atomic writes to multiple locations? Write many then snapshot? The answer so far is __NO__.

---
# 11/10/2018
Implement a FIFO queue that is wait free and linearizable with atomic registers and without any locks.

# Consensus
Each thread has a private input. They can communicate, after communication they have to agree on one thread's input.

## Theorem
The is no wait-free implementation of n-thread concesensus for read and write register.

Implicates that asyncronous computability is different from Turing computability.

### Protocal history as thread transitions
Change the method call into transitions between different states.

In our case if we have 2 threads then the only transitions available are between the 2 threads method calls. The translates into a bianry tree. 

To illustrate that lets consider a 2 thread and a binary tree of hight 4 as we only allow each thread to perform 2 moves before terminating.

A state is bivalent if the outcome from that states is still undetermined. Univalent means the opposit.

Critical state: The state that is just before some univalent states (needs to be all the substates to be univalent).

Table of interactions of critical states

|       |x.read()|y.read()|x.write()|y.write|
|-------|-------|--------|---------|-------|
|x.read()| no |no|no|no|
|y.read()|no|no|no|no|
|x.write()|no|no|no|no|
|y.write()|not|no|no|no|

Hence using atomic registers we cannot achieve consensus.

## Consensus object
```java
public interface Consensus<T> {
    T decide(T value);
}
```
We consider only one time use of the method, the first thread that calls that method wins.

```java
public abstract class ConsensusProtocol<T> implements Consensus {
protected T[] proposed = new T[N];
protected void propose(T value) {
    proposed[ThreadID.get()] = value;
}
abstract public T decide(T value);
}
```
The protocol is we show up propose our value to the array, go to a queue, dequeue the element if the element is red the thread decides its value, if we get a black then we return the other threads value. In our case we only have 2 threads.

```java
public class QueueConsensus<T> extends ConsensusProtocol<T>{
    private Queue queue;
    public QueueConsensus() {
        queue = new Queue();
        queue.enq(RED);
        queue.enq(BLACK);
    }
    public T decide(T value) {
        propose(value);
        Ball ball = this.queue.deq();
        if (ball == RED {
            return propose[i];
        }
        else {
            return proposed[1-ij];
        }
    }
}
```

### Consensus Numbers
An object X has a consensus number N if it can be used to solve N thread consensus.

Atomic read right register have consensus number 1.

Multi dequeuer FIFO queue has consensus number at least 2.

If we can implement X from Y and X has consensus c then Y has consensus number at least c. **Certain exeptions apply**

---
# 16/10/2018
### Consensus theorem
If we can implement object X from Y and X has consensus c then Y must have at least consensus c. (weird exceptions exists like quantum computers)

## Multiple assignment problem
Write multiple assignment then take a snapshot. With atomic registers this is not possible.

### Proof
take an array of 3 elements. Thread A writes to slots 0 and 1 B writes to 1 and 2 and any thread can read to any location. 

If thread A reads thread B value at slot 1 or slot 3 is value is empty it won the race and its value is proposed. If thread B reads thread A value at slot 1 or no value at slot 0 then it won and we use its value. 

Hence this structure can be used to implement 2 thread consensus therefore we cannot use read-write atomic register for that structure. 

## Read-Modify-Write Objects
Method call, return x prior value

```java
public abstract class RMWRegister {
    private int value;
    public int synchronized getAndMumble() {
        int prior = value;
        value = mumble(value);
        return prior;
    }
}
```
In our case we want mumble to be non-trivial like adding 1 to it. just reading is not enough.

### Theorem
Any non-trivial RMW object has consensus of at least 2.

#### proof
Subclasses of consensus have propose(x) method which stores x into proposed[i] built-in method with a decide(object value) to determine which thread value wins.

```java
public class RMWConsensus extends ConsensusProtocol {
    private RMWRegister r = v;
    public T decide(T value) {
        propose(value);
        if (r.getAndMumble() == v) {
            return proposed[i];
        }
        else {
            return proposed[j];
        }
    }
}
```
This cannot be implemented with RWM register as here we solve 2 thread consensus.

### Generalize to more that 2 threads
Let F ba set of functions such that for fi and fj either
- fi(fj(v)) = fj(fi(v)) commute
- fj(fj(v)) = fi(v) overwrite

`Claim is any set of RMW objects that commutes or overwrites has consensus of exactly 2.`

#### Examples
- test-and-set getAndSet(1) f(v) = 1 Overwrite
- swap getAndSet(x) f(v,x) = x Overwrite
- fetch-and-inc getAndIncrement() f(v) = v+1 commute

#### Proof
Assuming that the function commutes A applies fa B applies fb C runs solo 0-valent  
Assuming tha tthe function cummetes B applies fb, A applies fa c runs solo 1-valent must be decided.  
In these 2 cases to thread c both of these look the same, it cannot distingish between the two states hence this is cannot have concensus number 3.

Assuming the function overwrites. A applies fa, c runs solo and decides 0-valent  
B applies fb first then A applies fa, c runs solo and must decide 1-valent.  
But to thread c these two states look identitcal so we cannot do 3 thread consensus.

## Compare and Swap CASObject
```java
public abstract class CASObject {
    private int value;
    public int syncronized getAndSet(int v) {
        int prior = value;
        value = v;
        return prior;
    }
}
```

## Compare and set
```java
public abstract class CASObject {
    private int value;
    public boolean syncronized compareAndSet(int expected, int update) {
        if(value==expected) {
            value = update;
            return true;
        }
        return false;
    }
}
```

This code above has infinit consensus number.

```java
public class RMWConsensus extends ConsensusProtocol {
    private AtomicInteger r = new AtomicInteger(-1);
    public T decide(T value) {
        propose(value);
        r.compareAndSet(-1,i);
        return proposed[r.get()];
    }
}
```

## Consensus Hiarchy
1. Read/Write registers, snapshot
2. getAndSet, getAndIncrement  
inft. comparAndSet

We can reach anything in between using atomic k-assignment slots solves consensus of 2k-2

## Lock-Freedom
Lock-free:  
In an infinite execution often some method call finishes. So some thread may starve but there will be no deadlock. It is a pragmatic aproach.

## Wait-free
Each method call takes a finite number of steps to finish

Lock free and wait free are the same if the method execution is finite.

## Universality
If there is an object that can do consensus for n thread. Then out of that object we can build anything we want wait-free/lock-free linearizable n-threaded sequentially specified objects.

---
# 18/10/2018
# Spin Locks and contention
## Kinds of architecture
- SISD (Uniprocessor)
    - Single instruction stream
    - Single date stream
- SIMD (Vector/multiple processor doing the same thing on different data)
    - Single Instrunction
    - Multiple data
- MIMD (multiprocessor doing different thing on different data)
    - Mulitinstruction
    - Multidata

## MIMD architecture
We have a bunch of cores that share some memory in cache. They all access the same space as we get more processor we need to change the structure.

Distributed system have their own independent cache but can still access each other cache.

Issuses
- Memory contention
    - If all the processor want to access memory in the same location we are going to get some delays
- Communication contention, one processor can broadcast a message at a time which can slow things down.
- Communication latency
     - How long does it take to access the memory.

## What to do when not getting a lock
Keep trying, known as spin or busy-wait good if delays are short. Could be good for multiprocessor
Give up the processor, known as blocking, good if delays are long, always good on uniprocessor.

### Basic spin lock
Lock suffer from contentions as thread just try to access the same memory space (checking if the lock is available).

### Test and set instruction
Has a boolean value, set it to true with current value, return value tells if prior value was true or false. Can be reset by just writing false.
```java
public AtomicBoolean {
    boolean value;
    public synchronized boolean getAndSet(boolean newValue) {
        boolean prior = value;
        value = newValue;
        return prior;
    }
}
```
The code above is part of the ``java.util.concurrent.access`` 

```java
AtomicBoolean lock = new AtomicBoolean(false);
boolean prior = lock.getAndSet(true);
if (!prior) {
    //enter critical section
    lock.getAndSet(false);
}
else {
    //try again
} 
```
Can be implemented that way:
```java
class TASlock {
    AtomicBoolean lock = new AtomicBoolean(false);
    void lock() {
        while(lock.getAndSet(true)) {}
    }
    void unlock() {
        lock.set(false);
    }
}
```

#### Space complexity
N thread spin-kicj uses O(1) thread. As oppose to O(n) peterson/bakery. Overcomed by using MRMW.

#### Performance
Take n threads and set the counter to a million. And every interation takes 1 unit of time. Then with 1 thread it will take a million. Idealy increasing the thread should not change anything. But with our TAS lock the more thread we use the longer it takes.

## Test-and-test-and-set locks
Lurking stage, wait until the lock looks free.

Pouncing state, as soon as lock loocks avaialbe 

```java
class TTASlock {
    AtomicBoolean lock = new AtomicBoolean(false);
    void lock() {
    while(true)
        {
            while(lock.get()) {}
            if (!state.getAndSet(true))
                return;
        }
    }
    void unlock() {
        lock.set(false);
    }
}
```
This one does better as thread does not write to the memory location over and overagain but why?

### Understanding memory with processor
We have 3 layers of memory, first shared cache, processor access it through the bus (10 ms random access, broadcast medium one at a time), then local cache that is unique to each processor. (L1 not shared, L2 shared).

L1 is small and very faset, L2 is larger and slower 10s of cycle to access.

### Jargon
Cache hit, found what we needed.  
Cache miss, did not find the info in cache hence we have to go to other memory location.

#### Cave Canem
This model is still a simplification.

### Cache becomes full
When it is full we may need to make space for new entry by evicting older entres. Hence we must have a replacement policy.

#### Fully associative cache
Any line can be anywhere in the cache, so super flexible with what we can evict, hard to find lines.

#### Direct map cache
When someting is brought from memory it can only go to one place in cache (HashMap?). So it is easy to find a line, disadvantage is that we must replace fixed lines.

#### K-way set associative cache
Each slot holds k lines. Uses the best of both worlds, easy to find a line and some choice in replacing lines.  

k is increasing overtime as more and more cores share data.

### Cache coherence
A and B are both accessing cache address x, A writes to x (update cache). How does B finds out? Many cache coherence protocols in literature.

#### MESI protocol
Modified cached data, must write back to memory, so we mark it as **M**.

Exclusive **E**, the thread has the only copy and it has not been modified yet.

Shared **S**, not modified yet my be cached by someone else sometime soon.

Invalid **I**, cache content is no longer up to date.

### Write through cache
Update the value in the other location imediatly as soon as we update the value of our cache. Bus traffic, must writes back to main memory, most writes back to unshared data.

### Write back cache
Accumulate the changes we make in cache and write it back when a new process wants it or we need the cache for somthing else.

## Mutal exclusion what to opptimize?
- Bus bandwith used by spinning threads
- Release/Acquire latency
- Acquire latency for idle lock

### The TASLock seen above
Everytime we do a testAndSet opperation we invalidate cache lines. Miss in cache so all other threads have to use the bus to get the data. When thread wants to release the lock delayed behind the spinners that keep updating the value.

### Test-and-test-and-test
Wait until lock looks free, spin on local cache hence no need to use the bus while lock busy. The only issue is when the lock is released all threads are going to want to check the new value causing an invalidation storm. Forcing each thread to access the new value is main memory.

### Solution Introduced delay
Quiescence time: measure the time taken for acquire lock, pause without using bus, using the bus to get the new value increases linearly with the number of threads.   
If the lock looks free but I fail to get it, then I will delay myself by time x, if I fail again double the sleep time

---
# 23-10-2018
## Queuing idea
Avoid useless invalidation by keeping a queue of threads, each thread notifying next in line when lock is free.

## Anderson queue lock
Atomic variable next with an array. Each thread gets its own slot in the array. 

When a thread is done, it calls the getandIncrement on n method getting the first value of the array which is set to True so thread has the lock, incrementing n by one. Then if another thread comes along it will get the next slot on the array which is false so it spins on a while loop there, then when the first thread finishes it updates the next slot to true, messaging the next thread in line without disturbing the others

```java
class Alock implements Lock {
    boolean[] flags={true,false,..,false};
    AtomicInteger next = new AtomicInteger(0);
    ThreadLocal<Integer> mySlot;

    public void lock() {
        mySlot = next.getAndIncrement();
        while(!falgs[mySlot%n]) {};
        flags[mySlot%n] = false;
    }
    public voi unlock() {
        flags[(mySlot+1)%n] = true;
    }
}
```
The issue is that cache line could be shared by the threads in the array if we update our value we update the cache of all threads. One solution is to use padding (dummy values) between each real thread/queuing location to ensure that every update only update one area of the cache where only one thread looks at. Memory ineficient but removes the contegency problem.

#### Issues
- Space ineficient (1 thread per cache line)
- What if there is not that many contender?
- What if we have an unknown number of threads

## CLH lock
Initinally we have a tail the points to false. To acquire the lock, the thread creates a new slot for itself set to true, does an atomic move of the tail pointer to the previous slot and gets a pointer to look at the prev slot. Setting the taile to its own new cell.  
Another thread comes, creates a cell for itself setting it to true, then moves the tail pointer to its slot and keeps a pointer to the previous slot and spins on it.  
When the first thread is done with the lock, it will set its slot to false.

This structure can be recylced by a new thread that gets the slots of thread that finished executing on the queue. Once a cell is done with we can recycle it (C,C++).

### Space useage
- L = number of locks
- N = number of locks
- ALock(LN)
- CLH lock O(L+N)

```java
public class QNode {
    AtomicBoolean locked = new AtomicBoolean(true);
}

public class CLHLock implements Lock {
    AtomicReference<QNode> tail;
    ThreadLocal<QNode> myNode = new QNode();
    public void lock() {
        Qnode pred = tail.getAndSet(myNode);
        while(pred.locked) {}
    }
    public void unlock() {
        myNode.set(false);
        myNode = pred; //for reuse
    }
}
```

The only is that this does not work well for certain architecture (large server with no cache).

### NUMA and cc-NUMA architercture (Non-Uniform Memory Access) (cache coherent)
- Illusion flat shared memory
- Truth: no cache sometimes, some memory region faster than others

Structure looks like a lot of processors with its own cache, but memory is shared between processors in a network of nodes. So everytime we want to check if the value is false, then it will have to access the value from far away.

## MCS Lock
Solves that for spinning on local memory.  
The idea is the same, the only difference is that the node that we are on actually has a pointer to the next node.

So we have a taile that points to the next node set to false that itself points to nothing. The thread creats its own node se to true. Then set the tail to its own node, set the previous node to its own node.  

When another thread comes allong it does the same thing. So it spins on its own value. 

When the previous thread is done it goes to the next node if there is one and updates it. O

```java
public class MCSLock implements Lock {
    AtomicReference tail;
    public void lock() {
        Qnode qnode = new Qnode();
        Qnode pred = tail.getAndSet(qnode);
        if (pred==null) {
            qnode.locked=true;
            pred.next=qnode;
        }
        while (qnode.locked) {}

    }
    public void unlock() {
        if (qnode.next == null) {
            if(tail.CAS(qnode,null)
                return;
        while(qnode.next==null) {}
        }
        qnode.next.locked = false;
    }
}
```

## Abortable lock
### Back-off lock
Aborting is trivial, just return from the lock

### Queue locks
This is harder to quite as the next in line waits for that thread to gets signaled.

### Abortable CLH locks
The idea is to have the successor deal with it.  
Here the node itself holds a boolean but also the address of a next node (can be null if there is no node or a special identifiable node that tells the thread it can acquire the lock).

Thread arrives sets its own node, which points to null, then it make the tail points to its node and keep a reference to where the taile with pointing from. 

If it sees that that node points to the special node then it can acquire the lock.

When a thread whishes to leave the queue it makes its node point to the previous node it held the reference to. So that the next thread can see that the node it spins on no longer points to null, so it will move to the node it points to and spin on that.

## Lock Data Access in NUMA Machine
Works by node

### Hierarcical backoff lock
A thread wishing to acquire Back off by a smaller number if the thread that accessed the lock is on the same node than if the thread that acquire the lock is on a different node.

Disadvantage is that it adds unfairness.

## Hierarchical CLH lock (HCLH)
Have a global tail. The a local queue per node, the last thread on the queue points to the next local queue in a new node.

## Lcok Cohorting
Use a local lock (based on thread) and global lock (locked for the cohort) on the server.  
First a thread accesses a local lock, then acquires a global lock.

When it is done it releases the local lock of its node, if a thread acquires it then it can die othewise it releases the global lock as well.

### Thread Oblivious
Means that a thread can be release by a different thread than the one that acquired it originally.

GLOBAL lock: has to be thread oblivious 

LOCAL lock: has to have a field that tells the previous thread if a thread is trying to acquire the lock.

---
# 25-10-2018
## Concurrent Effects
Contention (all threads want to access the same memory location) effects, mostly fixed by queue locks. Although it is suppose to increase performance.

Problem arise when the code looks inherently sequential.

### Coarse-Grained Synchronization
Each method locks the object, so we have a sequential bottle neck as more thread stand in line. Thus adding more threads does not improve throughput.

## Fine Grained synchronization
Instead of usin ga single lock, we can have a lock for each part that needs to be synchronized

## Optimistic Syncronization
Read the objects without locking until we find what we want to modify if it is still untouched lock it and work on it else search again.

Usually cheaper than locking as we lock only when we are ready to perform a critical action. Mistakes are expensive

## Lazy Syncronization
Postpone hard work. Removing components from a data structure is tricking with multithread. Checking if it is there and adding to it is alright removing something is harded. 

The removal is done in 2 phases:
- Logical removal
    - Mark something as deleted so other threads can see that this element is technically no longer there.
- Physical remove
    - The object is removed physically from the data structure.

## Lock-Free Synchronization
Do not use locks at all  
Use atomic operations like compareAndSet() & relatives.  
This makes no scheduling assumption which is easier. However the implementation can be tricky and cause some overheads.

### Reasoning about concurrent objects
Invariant: property that always holds, when the object is created, preserved by each method at each step of the method.

This definition makes sense only if objects can only be modified through its method. In terms of java this can be done with private variables.

Careful with interference needed even for removed nodes, some algorithm traverses removed node, careful with malloc & free. With java again its alright as there is a garbage collector.

### Representation inveriante
Which concrete values are meaningful?  
Sorted, duplicates?  
It is important to ensure that the property we chose is preserved by other methods.

This is a contract, all method must respect that invariant.

## Linked List as a data structure
We will view it as a concrete representation of items a set. So it has no duplicates and contains an add,remove,contains methods. We will use a hashkey to locate it in the set

The list will be ordered in increasing order, the head and tail will be called sentinels and mark the min and max of our set.

**Invariants**:
- Sentinel node (the tail is always reachable from head)
- Sorted
- No duplicates

### Coarse-Grained Locking patter on List
We lock the head of the objects so that only one thread can access the list. The behavior is hence very similar to a sequential implementation. But we can get contention on the lock as more thread may want to access the list.

Easy to implement however as we only need one lock. Can work well if the contention is low.

### Fine grained Locking on list

#### Hand-over-Hand-locking
A lock exists from every node. First lock the lock of the first node, then acquire the lock of the next node, unlock the lock of the previous node at some point. 

For remove we lock the previous and current node lock. Assuming the current node is the node we want to remove, then we move the pointer of the previous node to the next node. Then unlock both locks. It is important to lock both nodes in case another thread is already trying to remove the next node.

To add the node we do the same thing lock the predecessor and the successor (predecessor->new node->successor). So neither of these nodes can be deleted, we can then insert our item in between them. Note that we can just lock the predecessor in this case, yet it may still be good practice to lock both incase somethign changes in other methods.

At this stage this is better than using a single lock, more threads can travers the list.

Not ideal as we have a long succession of acquire and release lock, also ineficient if one of the thread tries to remove a node early on the same convoy effect can happen.

### Optimistic syncronization on list
First find the node without locking, lock the nodes of interest,perform the operation.

Works almost the same as hand-over-hand-locking but we do not lock any node if we are not interested in them.

However to avoid ghost nodes happening while we are asleep (we find our nodes but before we lock them we go to sleep, another thread comes and deletes one of the node, when we go back we are going to add our node to a node that got deleted hence the node we just added is not actually in the list).

To avoid that when we lock the nodes we go back to the head of the list and check if our nodes are still part of the list. We also check if the previous node still points to our successor node.

Hence we may travers deleted nodes but the property of the list itself still holds.

Performs better as we do not lock unecessary things. Travaersal are hence wait free. Optimial if cost of scanning twice is lower than cost of scanning once.

Issue is that we need to travers the list twice and that the contains method also need to acquire a lock.

---
# 30-10-2018
## Lazy list syncronization
Removing nodes causes trouble so lets do that part lazily.

Here the remove() scans the list as before, lock the predecessor and the current node but the deletion is going to happen in two phases. First phase is logical, we mark that node as removed but it stays in the list. Then we physically remove the node as before.

The main improvement is in the validation process, there is no need to rescan, check the pred, cur, and pred points to curr without having to scan the list again.

So now we can make the contains method lock free
```java
public boolean contains(T item) {
    int key = item.hasCode();
    Node curr = head;
    while (curr.key < key) {
        curr = curr.next;
    }
    return curr.key == key && !curr.marked;
}
```

This is what is actually use in java for the java concurrent contrainers.

However the remove and add calls must re-traverse the list if contended.  
Traffic jam if one thread delays. Let say a thread gets the loc,gets a page fault, descheduled, everyone else using the lock is stuck.

## Lock free data structure
Garentees minimal progress in any execution.

The contains method is already wait free so we only need to think about add and remove implementation.

### Using compare and Set
Reminder when an object implement compare and set, it has a method that check if the value in the method is the expected value entered, if it is, it updates that value with the one wanted. This should be done atomically. This is implemented in hardware in most architecture.

#### Removal
We are going to combine our marked bit and the pointer to the next node into one object in Java we call that ``AtomicMarkableReference``. Which allows to do a compare and set on two things at the same time, it make us as we do compare and set on the reference also do a compare and set on the marker and thus see that the node is being removed. 

It atomically swing reference and update flag.
```java
public Object get(boolean[] marked) {
    // boolean is used to read the marked bit
    // reference is returned by Object
}

public boolean isMarked() {
}

public boolean compareAndSet(Object expectedRef;
    Object updateRef;
    boolean expectedMark;
    boolean updatedMark) {
}

public boolean attemptMark(Object expectedRef, 
                           boolean updateMark) {
}
```

When we find a logically deleted node in our path when scanning we finish the job by physically deleting such nodes.

This is the window class that keeps information about the list that we are interested on
```java
class Window {
    public Node pred;
    public Node curr;
    public Window(Node pred, Node curr) {
        this.pred = pred;
        this.curr = curr;
    }
    Window window = find(head, key);
    Node pred = window.pred;
    Node curr = window.curr;
}
```
---
# 01-11-2018
## Queues, stacks
Now we are going to see how to apply the locking systems to queues and stacks. These structures can be unbounded or bounded. Blocking (remove from empty structure then we block it) making it go to sleep as there is no point for it to hold the resource same thing when the strcture is full. The other variation is non-blocking where we through exceptions instead.

# Queues
enq() and deq() work at different ends of the object. Problem arises when the queue is empty or full or when the queue is small and both head and tail pointer point to the same location which can cause problems.

## Bounded queue
We are going to have a lock for the dequeu end (deqLock). Similarly we are going to have an enqLock. We also need a way to know the size of the queue, so a counter that counts the number of items in the queue (atomic integer here to avoid acquiering another lock).

Enqueu thread acquires the enque lock, then it checks the size ensuring the queue is not full. It then adds the node to the queue. Swing tail to the new node plus the previous tail to the new node. It then increments the size of the queue then release the lock (we might want to add the feature that the thread also notifies the dequeue lock that something has been added to the queue). 

Dequeuer thread showes up, acquires the dequeue lock, read the santinel next field **santinel is the head of the queue that contains something we are not interested in**. If there is a next field then it goes to it, copy the value of the field carried by that node and finally swing the prev node pointer to the node that the node of interest pointed to. Then we release the dequeu lock and decrement that size.

## Condition review
```java
public interface Condition {
    public void await();
    public void signal();
    public void signalAll();
}
```
``q.await()``. Releases the lock, sleep to give up the processor, awake when getting signaled, reaquire the lock and finish the run. 

Interesting each java class has a bilt in lock syncronized then if we just call wait() in that method then it is the equivalent of await. Same with notifyAll() and signalAll().

Issues can arrise in the implementation when having conditions where the signaling of just one thread is not done properly which can cause lost wake up calls. The solution is to use signaAll() everytime.

## Bounded queue improved
This time we are going to split up the conters, the enqueue method is only going to increment the counter.  And the dequeue will only decrement the counter. 

Hence we create 2 different counters one that gets incremented the other one gets decremented.

When the enqueue size rechease its capacity. Then it will acquire the dequeue lock and get its number and take the difference between its counter and the dequeue counter. We do the same thing for the dequeue counter. One down side however is that both thread have to acquire both locks however.

---
# 06-11-2018
# A lock free queue
First we have a head pointer, pointing to the sentinel node and the tail pointer pointing to the same sentinel when the queue is empty.

We are going to be using compare and set as atomic machine instructions,

## Enqueuer
Thread 1 wants to enqueue something, it does a compare and set on the tail and if the tail pointer points to a node that points to null then we swing the last node to poins to our node (logical enqueue). Then we check the tail to see if it points to the same node then we can swing the tail pointer to our new node. If we look at the tail node and happens to have a non null next field then we do a compare and set on the queu head pointer to make it point to the actual last node of the queue.

### Final actions
Logical enqueue, faile then restart. Physical enqueue, fails then ignore it as it means that some other thread did it for us.

## Dequeuer
Looks at the head, reads the value making a copy of it, then it is going to make that node the new sentinel by doing a cast on the head pointer and swing it to point to the node it just dequeued. Hence we can reuse our previous sentinel.

### Final actions
For java it will cleanup that memory automatically with the garbage collector. In other languages the simplest thing to do is to have each thread keep a list of unused nodes in a local pool. And allocate pop from the list with free node push into the list.

#### Issues
Let say a thread is about to dequeue something so it knows about the sentinel and the head node but it goes to sleep. Then other thread comes along and perform some dequeuing, recycling the sentinel node again.

##### Called Dreaded ABA fail
IBM goes arround it by using a specific cache store the even when the thread sleeps ensures that it gets updated if the list information changes

#### Solution
In java we use a AtomicStampedReference class, allows to do a compare and set on two things, 1 the address of the object and 2 a stamp that can be a number.

# Concurrent Stack
```
push(x)
pop()
```
## Lock free implementations
Same as the queue initially with a top pointer but without sentinels.

### Push thread
A thread comes creates a node places the item in there by pointing to the top of the stack and make the top pointer point to the top of the stack.

### Pop thread
Copies the first node and move the top pointer to the next node. 

### TryPush()
This is what we must use when implementing a lock free stack, this method returns true or false if it succeeds.  
First we get a pointer to the top node, then it will get the next node after the top. Finally it does a compare and set on oldtop node and the new one. Trying to swing the pointers.

This method is implemented in push, we trypush if it works we go out of the method else we backof for sometime then come back to actually do it at some point.

- advantages
    - No locking
- disadvantages
    - fear ABA
    - without backoff huge contention at the top
    - in any case no paralellism

## Making Stacks parallelisable
We are going to look into an elimination-backoff stack

### Observations
After an equal number of pushes and pops the stack will look the same. 

So we have an array that we associate with the stack (elimination array), when pushing we push at random on the array and pop at random on the array if they both collide we are good else we wait for out item to either be picked up at somepoint or go to the stack when freed. So when a thread is working on pushing or poping something in the stack then we use the array instead. So while they back of the use the elimination array.

### Dynamic range and delay
#### Optimization
Pick a random range and the max time we want to wait, we go to the array try, nothing happen so we go back to the stack but there is still contention so we go back to the elimination array with a wider stack and a longer back of time.

##### Asymetric rendez vous
The pop thread finds the next vacant slot (for pop pointers) in the array waiting and what the pushers do, they go to the elimination array find the first pop thread taken space and then put their item there.

### Linearizability
Before the stack was linearizable as mentioned, so the issue comes when looking at the array. If we assume that the pop and push call overalap at the array then we can linearize them.

### Beauty of elimination array
We turn something that use to be sequential to something that can now be parallelised. As well as cuting on the number of thread that want to access the stack. The more contention on the stack we get the more parallel we can be.

---
# 13-11-2018
# Matrix multiplication
The most obvious way to parallelise that we can do it for one entry of c we compute with an independent thread the aik and kj entries in A and B. 
```Java
class Worker extends Thread {
    int row, col;
    Worker(int row, int col) {
        row = row;
        col = col;
    }
    public void run {
        double dotProd = 0.0;
        for (int i = 0; i < c[0].length; i++) {
            dotProd += a[row][i]*b[i][column];
        }
    }
}
```
This is good if we have a small matrix but as soon as the matrix size increases the number of threads required increases in a square fashion.

Also each Thread takes resources, memory for stacks, setup teardown, scheduler overhead. Some Threads are short lived as the work is not equal.

## Thread pool
It makes more sense to have a pool of thread, Thread are assigned a short task, run it, rehoin the pool then wait for the next assignment.

It also provides and abstraction, the type of machine size does not really matter in this case writing the program the same way on a big server or a regular computer. This gives more portable code.

### Executor Service
Is what is used in Java.
java.util.concurrent  
Runnable object, call run method (when not needing a result)  
Callable<T> object so it returns a value of type T when using the task with call() menthod.

```
Callable<T> task = 
Future<T> future = executor.submit(task);
T value = future.get();
```
This sends to the executor to go ahead and execute the call method of this task object, Future<T> is used to retrieve the value afterwards.

If we submit a Runnable task then we can just use Future<?> then use future.get() to block the execution of our main thread until the runnable task has computed.

## Matrix addtion
Can be done in parallel by breaking down a n by n matrix into an n/core matrix to get a thread computing a region of the matrix per core.

```Java
classs AddTask implements Runnable {
    Matrix a,b;
    public run() {
        if (a.dim == 1) {
            c[0][0] = a[0][0] + b[0][0]
        }
        else {
            // partition here multiple times (see slides if unclear)
            Future<?> f00 = exec.submit(AddTask,(a[0][0],b[0][0]));
            f00.get();
        }
    }
}
```

### Dependencies
Matrix example is not typical as each Task is independent which is unusual, often tasks are not independent such as fibonacci

## Fibonacci
The following implementation is inefficient but explains our idea with dependencies.

```java
public class FibTask implements Callable<Integer> {
    int arg;
    int answer;
    public FibTask(int n) {
        arg = n;
    }

    public Integer call() {
        if (arg > 2) {
            Future<Integer> left = executor.submit( new FibTask(arg-1));
            Future<Integer> right = executor.submit( new FibTask(arg-2));
            answer = right.get()+left.get();
        }
        else {
            answer = arg;
        }
    }
}
```

Using a DAG graph we can see the recursive approach of the program. 

## How much parallelism?
- Work: Total time on one processor
- Critical-path length: longest dependency path (if we had infinit processor what is the longest path that we will have to make to compute the results)

### Notation watch
Tp = time on P processor
T1 = Work on one processor
Tinf = longest path if we had infinitely many processor.

#### Work Law
$$T_p \geq \frac{T_1}{P}$$

#### Critical law
$$T_p \geq T_{\infty}$$

## Performance measure
- Spead up P processir
    - Ration T1/Tp
- Linear speed up
    - T1/Tp = Theta(P)
- Max speedUp
    - T1/Tinft

## Matrix addition calculation
Work is:  
$$A_1(n) = 4A_1(n/2) + \Theta(1) = \Theta(n^2)$$
Which is the same as double loop stimulation

Critical path:
$$ A_\infty(n) = A_\infty(n/2)+\Theta(1) = \Theta(\log(n))$$

Max SpeedUp:
$$\frac{A_1}{A_\infty}$$

## What about matrix multiplication
Work is:  
$$M_1(n) = 8M_1(n/2)+A_1(n) = \Theta(n^3)$$

Critical path:
$$M_\infty(n) = 8M_\infty(n/2)+A_\infty(n) = \Theta(\log^2(n))$$

Max SpeedUp:
$$\Theta(n^3/\log^2(n))$$
If we had a 1000 x 1000 matrix then the maximum speed up would be:  
$$10^7$$  
Showing that this is highly paralizable.

## Shared memory Multiprocessors
Here we do not have direct access to the processors, threads are scheduled by the system.

### Ideal
Tasks are scheduled by a User-level scheduler to the processor.

### Reality
Taks are first schedule by user-level scheduler then these threads are then schedule by kernel-level scheduler processors.

## Uncertainty example
Initially all P processors available for the application

Then some serial computation comes along which takes one processor and that thread decides to do some I/O and gives back the processor so we havea fluctuation overtime of resources available for a given processor, thus we need to have a way of being able to deal with such changes in resources.

---
# 15-11-2018
## Speedup
The idea is to map threads onto P processes, what if the kenrel does not cooperate?

### Scheduling Hierarchy
- User level scheduler
    - Tells the kernel which threads are ready
- Kernel level scheduler
    - Syncronous
    - Picks a thread pi and schedule them.

## Greedy Scheduling
A node is ready if all its predecessors are done, the scheduler schedules as many nodes as possible. But the optimal version would pick appropriate nodes in order to make the algorithm finish the fastest.

### Greedy theorem
The gready algorithm will finish in:  
$$T_p \leq \frac{T_1}{P}+T_\infty$$

### Theorem on ration
Any greedy scheduler is in a factor of 2 of optimal. Comming up with an optimal scheduler is NP-hard which makes the greedy scheduler pretty good the star represents the optimal algorithm.

$$T_p^* \geq \max\{\frac{T_1}{P},T_\infty\}\\
T_p \leq \frac{T_1}{P}+T_\infty\\
T_p \leq 2\max\{\frac{T_1}{P},T_\infty\}\\
T_p \leq 2T_p^*$$

## Work distribution
Certain threads may finish there tasks faster than other which makes them useless.

### Work dealing
Distribute the work in an even fashion so every thread works, the issue arrises if all thread try to exchange work then they may spend a lot of time spreading the work arround in a recursive fashion which is not optimal.

### Lock free work stealing
1. Each thread is going to keep a pool of tasks it must do
2. Remove the work without syncronizing
3. If the thread runs out of work it steels some from another thread but from the top of the double ended queue

Thread push and pop from the bottom of there queue, but steal work from the top of other queues.

#### Poping and pulling out of the queue at the same time issue
```
popTop() fails if concurrent thread succeeds
popBootmo() fails if concurrent thread succeeds
```

#### The ABA problem
1. Read the top of the queue to still the job
2. Then it will do a compare and set on the top field but it is put to sleep
3. What if before it can do the compare and set the owner thread starts poping from the bottom and pushes new work to the queue. 
4. Then when the stealing thread wakes up it will do a compare and set on the wrong work destroying it although it was never executed then it will execute a command that was already done,

To avoid that in java we are going to use an atomic stamped reference such that every time we get an address we access a reference we also get an integer. The bottom pointer is set to be volatile. 

So now we read the top of the queue and retreive the stamp, then we are going to set our new top to be oldTop+1 same for the Stamp, but we are then going to check if the bootom <= oldTop that means that the queue is empty, if not we are going to try to steal the taks by doing a compare and set on the top ensuring that we are doing a compare and set on the oldtop with the same stamp, if not then that means that some other thread took the work thus we do not perform the task.

#### Variation
Randomly balance the loads, when a thread is out of work it sends a message to other thread to verify what thread has the most work and steal work from that thread.

The thread and work are kept in queues, a thread checks its work size, if there is not that much work then the thread tries to steal work at randome in order to balance the load between the victim and itself.

---
# 20-11-2018
#### Simple video game
We want to prepare frames for display, we need to ensure at least 35 frames/second, it is alright to make certain mistakes sometimes.

```java
while (true) {
    frame.prepare();
    frame.display();
}
```

But we may want to split that into multiple threads with phases so that a thread displays a phase then update the frame and display again in an "asyncronous" manner.
```java
while (true) {
    if (phase) {
        frame[0].display();
    }
    else {
        frame[1].display();
    }
    phase = !phase;
}

while (true) {
    if (phase) {
        frame[1].display();
    }
    else {
        frame[0].display();
    }
    phase = !phase;
}
```

## Synchronization problems
We do not want thread to display frames too soon nore too late.
s

## Barriers
Used a lot in scientific & numeric computations

Systems do not use them that often but can be used for garbage collection. 

### Implementation
- Cache coherence
    - Spin on local cached locations?
    - Spin on statically defined locations?
- Latency
    - How many steps
    - How long do other threads have to wait before the barrier breaks?
- Symmetry
    - Do all threads do the same thing?
    - If they are not on the same compuation structure (GPU, CPU) then maybe not

### Sense-Reversion Barriers
The barrier will have a falg associated with it and when that flag changes then we let the threads throught.

```java
threadSense = new ThreadLocal<boolean>
```

In java the class CyclicBarrier is the one that can be reused after usage. For just one time use we can use CountDownLatch.

### Waiting to decrement
An issue may arrise when a lot of thread queue up to decrement the barrier counter, the way arround that is to actually have multiple smaller barriers that allow threads to move up to levels. Once the last level is reached then we spread the message down the tree freeing up every threads stuck in hte tree. This causes more latency as we have to access different memory regions for different barriers, yet it causes let contagency which can be positive.

Not that great for servers but alright for local computations.

### Tournament Tree Barrier
We no longer do getAndDecrement(). Since there are only 2 threads per node we can determine which thread is going to move up at each level, at each level i if i-th bit of thread is 0 move up else keep back. We also add a flag to the node when the winner thread arrives it spins on its own flag, until it flips which means that the looser thread shows up and fliped the winner thread flag so the winner thread can go to the next level. The looser thread just spins on its own flag. This is better for the server as now we know exactly where each flag location is so we can make thread spin locally with their flag. **Good for bus based architecture**.

### Dissemination Barrier
Work in rounds, each thread A notifies a thread that is 2^i+A (mod m) away. So after log(n) rounds we know that the barrier is over. The issue is that we are notifying things all over the place and in terms of cache performance it is not that great. This is a good algorithm on the ohter hand to prove certain theorems.

## Multi core machine algorithm
### Static tree Barrier
We have one node per thread statically assigned good for server architecture so that the Threads do not have to move arround. We are going to have one global flag. Each node will have a count of how many of its children have not yet finished yet.

When a node has its counter set to 0 then it decrements its parent counter by 1 then start spining on the global flag.

If a node has no parent that means that it is the root of the tree thus when the counter of that node reaches 0, the thread flips the flag instead freeing all the threads. This can cause a lot of bus trafic for all thread spinning to retrieve a new value. The good thing though is that these threads did not have to go up and down the tree to figure out what node the are at and what node they should go to.

---
# 27-11-2018
# Concurrent Hashing and natural parallelism
We can use the lock free list we implemented in chapter 9.
```java
public hashSet(int capicity) {
    table = newLockFreeList[capacity]();
    for (int i = 0; i<capcity; i++) {
        table.add(new LockFreeList());
    }

    public boolean add(Object key) {
        int hash = key.hashCode() % capcity;
        table[hash].add(key);
    }
}
```
The issue arises when the length of inside list grows and we want to resize the list. Usually we resize a hash table when the a global threshold such as 1/4 of buckets exceed value x. Another one could be when any bucket has size x.

We can use coarsed grain or fine grain locking to be able to resize.

## Fine grain locking
To resize a thread ascquires the locks in ascending order. But first we ensure that the root pointer points to the original array before acquiering the first lock, create a new array and place all the entrie inside it then just make the root pointer point to the new array. But in this case we will be missing half the locks!

### Striped Locks
A solution would be to map each lock to 2 cells rather than 1. So now a lock, locks two cells entry rather than one. 

So now we can use regular lists.

## Read Write lock use
### Lock the table in read, shared mode
add(), remove(), contains()

### Resize
Here we have to get the lock in write mode. This may be hard to achieve if we have a writer thread that gets starved by a constant stream of readers. Hence we need to include a fairness property from the lock.

In this case each bucket will be of type lock free list as the read lock can be acquired by as many thread as wanted

## Concurent resize
When moving entries arround to the new table we may run into issues if a thread tries to look up an entry in the table.

### Do not move the items
Instead of moving the items when resizing we will just move the pointers to the entries.

### Recursive split ordering
Make sure that all the values after a pointer have the same key than the starting node. This is the missing thing. First when we split our array in 2 we look at the first least significan bit, then for the second time we look at the 2 list significant bit and so on. If we have a table size 2^i then we just look at the i significant bits. 

### The bit magic
We just have to revers the bits to know where to put what on the list.

### Removing a node
The issue now is what happens if we remove a node that is pointed to by the bucket array. Thus we can use sentinels node in order to achieve that, these nodes will never be removed.

### Bucket split in lock free maner
Here we have to insert a sentinels thus we have to use to CAS calls, one to add the sentinel to the list and the second to put a pointer from the bucket to the sentinel.

If we want to add a node whose hash bucket has not been initialize then first we add the sentinel then we add the node. 

Things can get more complex if parent of a hash function have not been initialized as well then we have to go all the way to the main parent initialize it and do it again and so on.

## Open address hashing
We keep all items in an array instead of a link list, we have one item per bucket and if we have a collision we map it to an empty bucket in the array.

### Linear probing 
When we hash a value to a bucket and we reach a value in the bucket that is not x, then we look at an extra feature of the bucket that tells us what is the furthest value that a hashed value reaching that index can be at from that index. This value can be updated as we have more and more collisions. 

This works well when the number of items is less than the size of the array. But if M aproaches N then the performance can go bad very fast. 
$$ \frac{1}{2}(1+\frac{1}{(1-\frac{M}{N})^2})$$

## Cuckoo Hashing
Use 2 hash functions on 2 different arrays. First we hash with one hash function and its full then we hash it with the next hash function independently, if both are full then you go back and hash the value on the first location with the second location and if it is empty you put it there otherwise repeat.

One issue here is when we run into circles. But when it is used properly then you are only 2 search away from an input.

Disadvantages: as the ratio to of entries to array size increases we run into cicle (half full in this case). This structure is a bit outdated

## Hopscotch Hashing
Use a single array with a single hashfunction. 

When we hash something we have a bit map associated with each bucket telling us if there is something after it if there is a 1 telling us that there is something or not for it that could be the value searched.

For adding we look for the closest open slot then swap them with other entries to make it closer and closer to the original hash value updating the bit map of hte neighbour hood we are affecting until we are close enough to the hash cell.

---
# 29-11-2018
When working with a set 90% of the time just contains, 9% add and 1% remove.

# Skip list
A node has a height associated with it giving it a number of pointers pointing to other nodes. When a node is created we randomely assign heights to the node. 

Each higher level is a sublist of the lowest level. The lowest level contains the entire list but the higher we got the less node we get.

The idea is that each pointer at a level jumps over 2^i nodes.