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