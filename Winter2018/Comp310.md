# COMP310

## Intro
### Operating system history
* vacuum tubes
* trensitors and batch system
* ICs and multiprogramming
* Pesonal computer
* mobile computer

### Evolution
* first generation
    * computer cost millions
    * hard to operate and program
    * No need for OS just one operation at a time
    * Idle was very expensive
* Second generation
    * Operators fetch compilers and card decks to get job read
    * Batch systems came up then to optimize processes.
    * Program where executed in batches and programing could now be done offline allowing us to start subroutines that can be used
* Third Generation
    * Memory partition used so that we can run multiple jobs together. Seeming parallelism, the CPU swaps between jobs. 
* How
    * Move from maximizing computer utilisation using 100% of CPU at a time to ensure that no power is wasted
    * Personal computers have lead to user wait time to be low as user activity must be spread out leaving CPU iddle more

### Design Concerns:
| Personal/Embedded system        | Security, space management, power consumption |
|---------------------------------|-----------------------------------------------|
| Time-Sharing systems           | Security, Response time, CPU scheduling       |
| Batch Systems (Super computers) | Maximize CPU usage                            |

## System calls
Happen when we want to open a file, first system service is requested, goes to the system call in memory then that goes to the system call table return from service function.


---

## 16-01-2018
### Process
    Is a running instance of an executable that supports concurrent operations.

### Process representation
* Program counter
    * tells us which statment will be executed next
    * General program counter that tells us which program will be executed next
* registers
    * Holds certain values
* Input data
    * Data that will be used by the program
* State variables
    * Current states of the variables stored in memmory
* output

### Process management
* Lifecycle managment
    * Process state changes
    * Process creation
    * Process termination
* Flow managment

### Process creation
    Upon start up certain systems are started.
    Shell gets created and takes in commands from the user
    A process can hence start other processes
    Users can hence through the shell start processes

    There are 2 ways for creating a process
    Windows build from scratch:
        Create a new process from scratch
        Create empty memory space
        Load code and data in memory
        Make process known to scheduler
    UNIX clone existing process:
        Stop current process and saves the state
        Make a copy of code data, heap and process control block
        make process known to the scheduler

        fork() is what is used
        first we are in the shell and perform a fork()
        So we clone the process which becomes a child of the
        shell and now both parent and the child compete for CPU 
        time. 
        fork() > 0 for the parent and = 0 for the child.
        This number is represented by the process ID, which in 
        this case the parent gets the process ID of the child. 
        exec() command is launched by the child to change the
        memory image.
``` C
    //What prints?
    main()
    {
        int i;
        i=10;
        if(fork() == 0) i += 20;
        printf("%d",i);
    }
```
    It will print 1030 or 3010 depending on which gets processor time. 
### Process information (process controled lock)
* ID (Identifier)
* CPU_State 
    * What was the process doing
* Processor ID
    * Which process was it running on
* Memory of the process
* Open files and resources required for the process to execute
* State 
* Creation tree
    * Process can create a new process child
    * Process can come from a parent
* Priority
### System:
* Bash system
    * Runs every program from begenning to end so there is no process
---
## 18-01-2018

### Process diagram:
| ready | stopped | active| exiting | blocked |
|---: |---:|---:|---:|---:|
| dispatch (active)|resume (active) |time out (active)||resources availalbe (active)|
||kill (exit)|suspended (stopped)|
|||event or resources awayts (blocked)|
* Ready: preocesses are prepared to execute when given the opportunity
* Active: the process is being executed by the CPU
* Blocked: process cannot execute until some event occur
* Stopped: a special case of blocked where the process is suspended by the operator or the user
* Exiting: process that is about to be removed from pool of executable.

### Tiny shell
We wait for commands to be entered by the user, then we fork and execute the command in a chid process using the exec(line) or execvp(line) replacing the memory copy of the parent changing the memory image of the parent for a new one.

### Implementing process
Create a loop run a process for some time, save its state then repeat with a new process. 

1. Hardware stack program counter
2. Hardway loads new program count from interupted vector
3. Assembly language save the registers
4. Assembly language sets up a new stack
5. C interrupt service runs
6. Scheduler decides which process is to run next
7. C procedure returns to assembly code
8. Assembly language precedure starts up new current process

### Dispatcher working:
Selects from a list of ready processes which one is to run next. \
The dispatcher trust the process to wake up the dispatcher when done. \
The dispatcher can wake up the dispatcher after a certain amount of clock ticks. \
The alarm clock is better as it forces processes to give up the CPU after a certain time. 

### Information to be saved:
While saving the OS should mask all interrupts
* Program counter
* Program status
* CPU registers 
* FIle access pointers
    * What files are open and could be access by the process?
* Memory 
    * Save it all on a disk
        * Slows down the system a lot
    * Do not save memory trust the next process
        * If memory is overwritten it can be bad
    * Isolate (protect) memory from next process
        * This is the approach taken currently called memory managment. 

### Processes and threads
Processes have an array of handles. \
Handles corresponds to an external device and is used to access it. \
To print somthing on screen we call handle 1 \
To read from the keyboard we use handle 0 \
Handles are henced abstract indicators used to access external resources. \
An array structure maintains by the kernel for each processs holds the handles. They can be modified through syscalls. 

---
## 19-01-2018
### Writing to the screen
```C
char *p="hello\n"
write(1,p,6)
// stdout: 1, source, 6 bytes
int fd = open()
// the value returned is the file descripter pointing to the given file.
```
### Process creation
\#0 stdin and \#1 stdout \#2 is stderr by default are set on the array

### Output redirection
If we close(1) and open(file.txt) the file descriptor value will take the spot of 1 and so all the STDOUT will go to the file now.

### Memory in processes
Memory is viewed as a large array. \
Logical view of process: looks like it has all the memory required dividing into 2 parts: half is held by the kernel the other half is held by the process (user space). \
Part of the memory is mapped to the kernel and using the piece of memory processes can comunicate with other process.

### Address space of User
1. Stack
2. Dynamic alocation
3. bss
4. data
5. text \
The handles of each process is maintained in the kernel memory, each child recieves a duplicate of the file descriptor. 

### Communication between processes
Can be achieve through the kernel space. \
First we create a pipe() system call, a pipe contains a read end and a write end. \
Second we fork knowing that the array of file handles is the same between child and parent. \
We can also use the dup(fildes) duplicates an existing file descriptor. \
So now in one process we close(1) and dup(Writtingpipe) in the other we close(0) and dup(Readingpipe)

### Concurrency vs Parallelism
Concurency: executing multiple processes on a single core processes. \
Parallelism: 2 processes running at the same time using dual core processor for instance.

---
## 23-01-2018
### Modeling Multiprogramming
1. $p^n$ is the probability that all processes are waiting on an IO operation. 
2. The Higher the IO wait the lower CPU utilization. 
### Parallelism
It is used to run computation faster by allowing us to run processes simultaneously. Applications are not always fully parallel, they are made of a parallel part and a series part.
```
int a[5]={...}
int b[5]={...}
int c[[5]={...}
for (i=0;i<5;i++)
{
    a[i]=b[i]*c[i]; //Parallisable
}
prod=a[3]+2*a[2]+a[0] //Not parallisable
```
### Amdhal's law
Performance improvements by applying an enhancement on an application is limited to the time the enhancement can be applied.

### Speed up for an application
$speedup \leq \frac{1}{s+\frac{1-s}{N}}$ \
Here S is the serial portion of the application and N is the number of cores available.

### Motivation for threads
Threads can happen at the "same" time, most modern application are multithreaded. Used when we want to perform different not so tightly coupled activities. \
Note that threads are not processes. Process is what actually holds the threads. We have one process and within that process we have multiple threads. 

Also used in multithreaded web servers. We have multiple threads to handle different requests at the same time.  
1. Connection is established
2. The web looks to see if the web page information already exists in the cache.
3. Cache hit is kick and cash miss forces us to go to the disk
4. While waiting for I/O another thread can access the cache

### Why use processes rather than thread on their own
Processes contain data that needs to be loaded and handled but threads do not have these properties. So processes are heavy weight compare to thread that are light weight. Thus threads are used because they are easier to create, threads that run in a process also need access to the same memory so making them exist in the same memory space makes it easier to implement and use them. \
Disadvantage of the method is that threads are are less fault tolerant. \
Sharing memory we do not want two threads to access and update a memory location at the same time. 

### Process creation
1. Needs to setu up not address space update resources
2. Kernel per process data structures need to be allocated and initialized.

### Dif mult vs single threaded process
#### single
If the code runs that thread is always executed and contains a single stack that tells us where the process is executing, and a single line of registers.
#### multi
We have multiple thread of execution swapping between each other. Registers,stack are different for all threads. The code, file and data is the same for all otherwise.

### User vs Kernel threads
#### User threads
Managed in the user space (user-level threads library)
Three primary library
1. Pthreads
2. Windows threads
3. Java threads

Many user level thread are linked to one kernel thread, when one thread is executing the Kernel thread is locked and the other threads cannot execute. (only one maybe in the kernel at a time). This is used by older systems, few systems use it today.
#### Kernel threads 
Supported by the kernel.
Each user level thread maps to kernel thread
A user created kernel thread creates a kernel thread. \
Number of threads per process maybe restricted because of overheads.  

### Implementing threads
In user space the thread reside in a process with a thread table in the process but the kernel only knows about the Process table. \
In kernel level threads the Kernel can see the thread table. 

### Hybrid threads
Many user level threads mapping to several kernel threads. 

### Thread libraries 
Provide programming with an interface that allows user to create threads.
#### In Linux

* Kernel level threads
* Thread have specific stacks
* They are all executing the same program but at different location so they have their own program counter.

#### Pthreads
* Can be user level or kernel level threads
* Specification not implementation
```C
#include <pthread.h>
int pthread_create(pthread_t *thread, pass attributesm void, *(*strart)(void *), void *args); //Here at start we pass in the method that we want the thread to execute.

pthread_t pthread_self(void); //ID too fast in class help us get the ID of the threads.

int pthread_equals(thread1,thread2); //Used to see if two threads are equal

void pthread_exit(void *retval);

int pthread_join(pthread_t thread, void **retval); //Here we when a thread wait for another thread, 0 success or a positive error number

pthread_cancel();

//Detached threads cannot be joinable.
int pthread_detach(pthread_t thread); //Use to stop for the waiting I assume
```
---

## 25-01-2018

### Thread continue
Detached thread clears its resources when finishing we pass this as the attributes can be passed for that. 
``` C
int pthread_mutex_lock(pthread_mutext_t *mutex); 
//Use to lock shared variables in all threads.
int s = pthread_mutex_unlock(...);
if(s !=0)
{
    ErrExitEN(s, "pthread_mutex_unlock");
}

    
```
This is important when we deal with share variables that we do not want modified or used by multiple threads.

### Thread cancelation
1. Do it imidiatly
2. Do it after some code was run 

```C
int threadfun(void *args)
{
     ...
}
int main()
{
    int pthread_testcancel(thread);
    int pthread_cancel(); 
    //invoke cancelation requests cancelation but actual cancelation depends on thread state.
    p_tread_t thr;
    p_thread_attr_t attr;
    int s;

    s = p_tread_attr_init(&attr); //Here the attribute is initialized so now it exists

    if(s !=0)
    {
        errExitEn(s,"p_tread_attr_init");
    }

    s = p_tread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED); //Here we set the detached state of the attribute to detached
    if(s !=0)
    {
        errExitEn(s,"p_tread_attr_setdetachstate");
    }

    s=pthread_create(&thr,&attr,threadfunc,(void *) 1); //When we create the thread we give it the detached attribute so it gets detached.
    if(s != 0)
    {
        ...
    }
}
```

### Sychronization 

#### Concurrent process
* Compete for shared resources.
* Any process can run at any given time.
* Cooperate with each other in sharing global resources.
* Competing (no need for sycrcho)
    * Do not work together.
    * Ex 2 independent process running no matter the order the output will remain the same. 
    * It is deterministic, reproducible (can stop and restart);
* Cooperating
    * Ex Transaction process in airline reservation system
    * Share a common object or exchange messages, may be irreproducible, subject to **race conditions**.
    * For some of these process the order of some instruction is irrelevant however certain instruction combination must be avoided.
* purpose of Cooperation
    * Share resources
    * things faster
    * Solve problem modularly 
        * Output of one process is the used by another one.

### Race condition
When 2 or more process are reading or writing some shared data and final result depends on who runs precisely when in a race condition. 
#### Mutual exclusion:
For cooperation to occur we need no processes to be in their critical section at the same time. We cannot make any assumption about the speed and the number of CPU that we have. No process running outside of its critical section may block other processes. No process should wait forever to enter its critical section. 

#### Critical section
Should be atomic it runs all at the same time or not we do not switch process while a process is running its critical section. So it is important to minimize the size of the critical section.

##### Solution
Simplest: disable all interrupts, no process can interrupt while the process is running. Works for a single CPU. But it is not practical as the OS operation will be hindered and it will not work on multiple CPU.

---
p thread test cancel and cancellation points?
Go back to first slide?
## 26-01-2018

## Solutions continued
### Use a variable lock:
```
if lock == 0 set lock=1 and enter_region
if lock ==  1 wait until lock becomes 0

    Process 0
    {
        while(TRUE)
        {
            while (turn !=0);
            critical_section();
            turn=1;
            non_critical section;
        }
    }

     Process 1
    {
        while(TRUE);
        {
            while (turn !=1);
            critical_section();
            turn=0;
            non_critical section();
        }
    }
```
But starvation can occur: one of the two processes will wait for a long time starve to enter the critical section but the other process will not give it the turn. \
The second problem is that we are contineously checking the term variable until the process is available just like I/O which is a problem. \
Example: \
Kernel level multi threads are used for concurrent processing. Each thread has 2 second for the critical section and 8 seconds for the non-critical section. If we have to thread what is the runtime? If the thread keeps changing every second we get (nc=non critical, c= critical).

|1|2|
|:--:|:--:|
| nc | nc |
| cs | cs (so blocked) |
| cs | waiting |
| nc | cs |

Here we can see the Process 2 will wait for some time while not performing any operations. 

**Def: Busy waiting is a terminology that implies that there will be some cpu runtime where a process will just idle and not perform any operations. (Passed midterm)** \
**Def: no parllelization overhead: means that there when processes switch on CPU they do not take any time.** 

### Use own key to critical section:
This removes starvation as they do not have to wait on each other.
```
 Process 0
{
    while(TRUE)
    {
        while (flag[1]);
        flag[0]=true
        critical_section();
        flag[0]=false;
        non_critical section;
    }
}

Process 1
{
    while(TRUE)
    {
        while(flag[0]);
        flag[1]=true
        critical_section();
        flag[1]=false;
        non_critical section;
    }
}
```

---
## 30-01-2018
Continued: \
Problem occurs when both flags are set to false P1 executes the while loop and enters critical section. Then process swap so P2 also enters the critical section which defeats the purpose as both program are now both in the critical section. 

### Mutal exclusion deadlock
Set the flag to true before entering the critical section then executes the while loop.
```
 Process 0
{
    while(TRUE)
    {
        flag[0]=true
        while (flag[1]);
        critical_section();
        flag[0]=false;
        non_critical section;
    }
}

Process 1
{
    while(TRUE)
    {
        flag[1]=true
        while (flag[0]);
        critical_section();
        flag[1]=false;
        non_critical section;
    }
}
```
Problem they both turn on their flag and then they both execute the while loop due to processor time swap causing them to both be locked and never entering their critical section.

### Live lock 
```
 Process 0
{
    while(TRUE)
    {
        flag[0]=true
        while (flag[1])
        {
            flag[0]=false;
            // random delay
            flag[0]=true;
        }
        critical_section();
        flag[0]=false;
        non_critical section;
    }
}

Process 1
{
    while(TRUE)
    {
        flag[1]=true
        while (flag[0])
        {
            flag[1]=false;
            // random delay
            flag[1]=true;
        }
        critical_section();
        flag[1]=false;
        non_critical section;
    }
}
```
Works because both program give a time to the other just in case to enter its critical section before them.

## Decker's Algorithm
Each process has its own flag to enter the critical section and another flag for arbitration the turn flag. \
Works in 2 steps we have interest with the flag and an arbitration with the turn.
```
 Process 0
{
    while(TRUE)
    {
        flag[0]=true
        while (flag[1])
        {
            if(turn==1)
            {
                flag[0]=false;
                while(turn==1);
                flag[0]=true;
            }
        }
        critical_section();
        turn=1;
        flag[0]=false;
        non_critical section;
    }
}

Process 1
{
    while(TRUE)
    {
        flag[1]=true
        while (flag[0])
        {
            if(turn==0)
            {
                flag[1]=false;
                while(turn==0);
                flag[1]=true;
            }
        }
        critical_section();
        turn=0;
        flag[1]=false;
        non_critical section;
    }
}
```
## Peterson's algorithm
Decker's is hard to prove and this one is much simpler. It is however based on the same idea than above.

```
 Process 0
{
    while(TRUE)
    {
        flag[0]=true
        turn=1;
        while (flag[1]&&turn==1);
        critical_section();
        flag[0]=false;
        non_critical section;
    }
}

Process 1
{
    while(TRUE)
    {
        flag[1]=true
        turn=0
        while (flag[0]&&turn==0);
        critical_section();
        flag[1]=false;
        non_critical section;
    }
}
```
## Hardware implementations?
The solutions seen are just software solutions and it is very hard to extend that to more processes. \
Current hardware today have microprocessor that allows for that. 
### Test and lock
LOCK is the shared variable in memory so that all processes can see it. \
Read the content of memory location LOCK into register RX and stores a non-zero value at memory LOCK if RX is equal to 0. RX is local and used by processes to check the value of LOCK. \
Advantage we can have a single LOCK variable for multiple process. \
Disadvantage, busy waiting and starvation are possible outcomes of the hardware.

## Busy Waiting Approaches Problems
Priority inversion problem is a problem: lower priorities can run before higher priorities. 

### Alternative
Sleep wake up approach: used so that a process never gets CPU time when it is busy waiting.

## Semaphores (sleep and wake up)
Two or more processes can cooperate by sending simple messages. This allow a process to be stopped until it recieves a messages the variable used for messages is Semaphore. If we want to transfer a message we use **signal** otherwise we use **wait**. s is the messaging variable uses: signal(s), wait(s).\
* can be initializedd to a non-negative values
* wait decrement the semaphore value by 1 if it becomes negative then the process that is executing that wait is blocked
* signal increment the semaphore value if value is not positive a process is unblocked since that means that a process was waiting. \
    * Not that 0 is taken into account

---
# 01-02-2018

```C
struct semaphore
{
    int count;
    queutType queue
}
void wait(semaphores s) //wait is atomic
{
    s.count--;
    if(s.count<0)
    {
        place process in 
        s.queue;
        block this process
    }
}

void signal(semaphore s) //atomic
{
    s.count++;
    if (s.count <= 0)
    {
        remove a process P from s.queue;
        place P on ready list
    }
}
```
#### How process removed?
FIFO: strong semaphore. \
Random: weak semaphore

### Using semaphores
binary- is a semaphore initialize to 1 because first program make it go to 0 so it can execute the other programs then will make it negative and hence will not enter. \
Counting is a semaphore with an integer value ranging between 0 
and an arbitrarily large number of units \
 of the critical resrouces that are available. -Also known as general semaphores

```C
semaphore mutex =1;
semaphore empty =N; //all slots are empty N is an int
semaphore full =0; //no slots are full

//*First run*
producer()
{
    int item;
    while(1)
    {
        item=produce_item();
        wait(&empty); //decrement N by 1 so can enter
        wait(&mutex); //ensures no other process enters
        insert_item(item);
        signal(&mutex); //give other processes a chance
        singal(&full); //adds 1 to the full list
    }
}
consumer()
{
    int item;
    while(1)
    {
        wait(&full); 
        wait(&mutex); 
        item = remove_item();
        signal(&mutex);
        signal(&empty);
        consume_item(item);
    }
}
```
## Primitives revisited Monitors (more at the end of semester)
Monitor is a high-level language abstraction that combines shared data operations on the data synchronization using condition variables. \
Monitor can automate semaphores to reduce chances of error. Protects data from unstructured access.

# Deadlocks
Permanent blocking of a set of processes that compete for the system resources. \
Hard to fine an efficient solution for that. \
A set of processes are in a wait state because each process is waiting for the resources that the other one has. (two children each having a toy, they want the other toy but will not give theirs away)
1. P1 holds the disk needs the printer
2. P2 holds the printer but needs the disk
3. Neither give their resources away

## Classification of resources
### Reusable
Something can be safely used by one process at a time ans is not depleted by that use.
### Consumable
When a resource is acquired by a process it ceases to exist. (messages, signals)
### Preemptable: 
these can be take away form the process owning it with no ill effects (save restore) i.e CPU

### Non-Preemptable:
Cannot be taken away from its owner with without causing the computation to fail. (printer or floppy disk)

## Condition for deadlocks
All these conditions are necessary but not sufficient all 4 are needed for deadlock to occur
* Mutual exclusion
    * process requires exclusive usage control of its resources 
* Hold and wait
    * process may wait for resources while holding others
* No preemption
    * Process will not give up a resource until it is done with it
* Circular wait
    * each process in the chain holds a resource requested by another. (chain of processes)
* Process cannot be reset to an earlier state where the resources are not held


---
# 02-02-2018

## Solving deadlock
* Preemptions -> make sure a process does use any resources until it has acquired all it needs
* System that prevents and avoid deadlocks -> prevents cycles.
* Transaction give check point so that processes can move move back to the checkpoints -> negates irreversible processes.

## Resource allocation graphs

Set of processes {P1,P2,..,Pn} depicted as circles\
Set of resources {R1,R2,..,Rn} depicted as squares \
Dots inside a resource show how many units are available \
Pi->Rj Pi waits for ( has requested ) Rj \
Pi<-Rj Resource Rj has been allocated to Pi 

## No deadlock
* If there is a cycle but at least one process is still active and moving forward no deadlock 
* If resources contains multiple units a circle is necessary but not sufficient for deadlock, we must have a knot meaning that no resources are allocated to processes that are outside of the circle. (No processes that can run)

## Strategies for deadlock
1. Ignorance: pretend there is no problem (restart your system)
2. Prevention: design a system in such a way that deadlock is excluded a priori
3. Avoidance: make a decision dynamically checking whether a request will, if granted, potentially lead to a deadlock or not.
4. Detection: le the deadlock occur and detect when it happens, and take some action to recover after the fact.

## Reaction to deadlock
#### People
Mathematicians find it not acceptable
Engineers ask hw serious it is and do not want to pay a penalty of performance.

#### UNIX
Ignore the problem as prevention price is hight

## Deadlock prevention
Design a system that excludes the possibility of deadlock.
* indirect method
    * Prevent the occurrence of one of the necessary conditions
* direct method
    * prevent the occurrence of a circular wait condition \
Deadlock prevention strategies are ...

## How to set it in action
#### Mutual exclusion
In general this cannot be disallowed
#### Hold-and-wait
Require a process to request all of its required resources at a time making it block otherwise only activating it when all the resources are available. This prohibits resources from being use optimally. Sometimes it is hard to know in advance all the resources that a process will need. The programmer will have to give all the possible resources required by the process note that these are possible not required.
#### No preemption
If a process holding resources is denied a further request that process must release all its unused resources and request them again.
#### Circular wait
Can be prevented by defining a linear ordering of resource type. 
If a process ahs been allocated resource of type R, then it may subsequently request only those resources of types following R in the ordering. So P1 holds are R1 so it can only request Ri>1, P2 holds R2 so it can only request Ri>2. 

## Deadlock avoidance
Make judicious resources allocation choices to ensure a deadlock-free system. However we might get to a state where a process can still run but deadlock is still unavoidable. 
* Evaluate current request and see if it will need to deadlock
* we need to have knowledge of future request. 

---
# 06-02-2018
## Deadlock avoidance continued
* Simple model require that all processes declare the maximum number of resources of each type that it may need
* The algorithm dynamically examines the resource-allocation state to ensure that there can never be a circular wait.
* The resource allocation state is defined by the number of available and allocated resources, and the maximum demands of the process.

### Safe state
If there exists a sequence of <P1,P2,...,Pn> such that each Pi can get its resources for currently available resources plus resources held by all the Pj with j<i. Basically saying that there is an order of execution lowest process first followed by the second lowest and so on. So all the resources are available when a process has to execute. \
If a system is in an unsafe state there is a possibility for deadlock. Hence deadlock is a subset of the unsafe state. 

### Avoidance algorithm
* Single instance of resource type
    * Simply use a resource allocation graph
* If we have multiple instance 
    * Use banker's algorithm

#### Resource allocation graph
* Claimed edge is represented this way Pi --> Rj with a dash line
* Claim edges convert to request edge when the request is made.
* Request edge is converted to an assignment edge when the resource is allocated to the process
* When the resource is released by the process then the edge goes back to a claim edge
* **Draw back is that the resources must be claimed a priori**
* Suppose that Pi->Rj then we only allow Rj to be assigned to Pi if the resulting graph does not lead into a cycle with claimed edges included. 

#### Banker's algorithm
* Each process must claim maximum use of every resource
* When a process request a resource it may have to wait
* When a process gets all its resources it must return them in a finite amount of time. \
Assume N processes and M resources \
Availability vector Availj units of each resource (init to maximum, changes dynamically)
```
Let [Maxij] be an N x M matrix
Maxij=L means Processes Pi will request at most L units of Rj
[Holdij] Units of Rj currently held by Pi
[Needij] Remaining need by Pi for units of Rj
Needij=Maxij-Holdij for all i & j
```
1. Initilize 
```
if REGj > Needij abbort--error
```
2. Check if the requested amount is available
```
if REQj > Availj got to Step 1 Pi must wait
```
3. Provisional location
```
Availj=Availj-REQj
Holdij=Holdij + REQj
Needij=Needij-REGj
if isSafe() then grant resource
else cancel allocation got to step 1 Pi must wait and revert back the operations done.
```
This allows the algorithm to see what the new system will look like if the resources are allocated if.

#### safeState()
1. Initilize
```
Workj =  Availj for all j; Finishi false for all i
```
2. Find a process Pi such that
```
Finish = false and Need ij ≤ Workj
```
if no such process go to step 4 \
3. Simulates the idea of a process getting all the resources that it needs.
```
Workj = Workj + Holdij
Finish = true
```
4. 
```
if Finishi = true for all i then return true //sytem safe
else return false no the system is not safe
```
### What is safe
* very safe
    * NEEDi <= Avail for all Pi so processes can run in any order
* safe (but take care)
    * NEEDi > Avail for some Pi
    * NEEDi <= Avail for at least one Pi such theat there is at least one correct order in which the processes may complete their resources
* Unsafe 
    * Need i > Avail for some Pi
    * Need <= Avail for at least 1 Pi
* Deadlock
    * Needi > Avail for all Pi

---
# 08-2-2018
#### Example
P1 wants 1 resource

| | max| hold | need | finish |
|----|----|----|----|----|
|P1| 5| 1+1| 4-1|F |
|P2| 5| 2| 3| F|
|P3| 2| 1| 1| F|

avail=2
work=2-1\
P1 cannot finish so it is not good good \
P2 connot finish so not good. \
P3 connot finish so we stop. \
We will go mutliple time through the same process as resources may be allocated with time.

## Deadlock detection
This does not prevent deadlock, it let them happen and then deals with it. \
It detects deadlock by running a periodically running an algorithm to detect a circular wait condition. Then we take some action to recover after the fact. \
With this method resources are granted whenever possible. \
We check for deadlock everytime there is a resource request or based on how likely it is for a deadlock to occur. \
Checking every time a resource is granted leads to early detection and the algorithm is simple because it is based on incemental changes to the state of the system. \
On the other hand such frequent checks consume a lot of processor time. 

### After detection the recovery
* Recovery through Preemption
    * Take resources away from its owner temporarily
* Reovery through Rollback
    * If it s known that deadlocks are likely we can have checkpoint for processes, ie undo transcation and free lock on table database records
* Recovery through terminal
    * Kills some processes in the cycle, irrocverable loss may occur

# Secondary Storage After midterm
It is a non-volitile repository for both user and systems. This means that when power is turned off the data remains in the repo. \
It is managed by the opperating system as part of the file System. \
It can store programs (source, objects), temporary storage and virtual memory. 

## Requierments 
* large memory to handly large information
* Concurrent access
* Persistent storage (resources are not dependent on power)

### users requierments
* file naming protection and operations are allowed on files
* How storage appear to the user

## File concept
All the data in the file are not stored in the same space it is fragmented. \
* Logical view is the way the user sees it a file is a continuous sequence of data
* Physical view, as it actually resides on secondary storage as a sequence of bits.
* Files are non volitile they should be long lasting
* Files aer intended to be moved arround and accessed by different programs.

#### File system takes a file and breaks it down into bits of data ready to be stored on the disk it is the abstraction of the OS

## Elements of storage managment
Logical view: user -> file Structure -> access method -> records
Pysical view: Buffering -> block caches <-> File alocation, free space managment -> Controller chacnes <-> disk scheduling

* A file is a sequence of fix-length record
* read/write: one record at a time
* buffer cache data blocks 
* Block caches (hold the changes in the files)
* controller cache is in hardware space (This is for quickly sending the data close to the hard drive)
* disk controller allows the CPU to communicate with the disk, holds data while being read to disk.

---
# 09-02-2018
## File attibutes:
* NAME,owner,creator
* Type of file (source code. binary type)
* Location (I-nodo or disk address)
* Organiziation (sequential, indexed and random)
* access permissions (sequrity purposes)
* time and date (creation, modification and last accessed)
* Size
* variety of others (maintenance, when was the file last saved)

### File name
Provides a way to store and retrieve data from backing storage using file names. \
Many OSs support two or more parts separated by a period in names. \
In unix file conventions are not enforced. \
Every os must recogonize its own executables.

## Directory
It is a table that can be searched for information about files implemeneted as a file. It is a file that contain a list of files it contains. \
Give a name space for the objects to be saved \
Driectory entry contains information about the file. 

### Type of directory

#### Single-level (flat)
Shared by all users CDC 6600 world's first computer

#### Two-level:
One level for each user, they should have some form of user login to identify the user and located the user's files.

#### Tree
Arbitrary (sub-tree) for each user login required. 

### Tree structure
We have a root directory parent to bin, usr, lib, tmp. If we want to go to user we go through the file that tells us where user location is, we move their and then go through the user file. \
Note that it is a directed acycled graph but 2 directory can point to the same file. 
### Name space
Is a set of symboles used to identify files.

## File systems
* Keep track of files (knowing the location)
* I/O support, especially the transimission mechanism to and from main memory.
* management of secondary storage
* sharing of I\O services
* providing protection mechanisms for information held on the system.
### Abstraction layers of file system
1. Shell the provides access to files open, close
2. Application and system programs c codes once a file is open go to a specific location in that file
3. File system: translating to specific actions on the disk what needs to be read ensuring the c programs read what it wants. 


### Unix file system
We have a tree structure with the root directory. Each subdirectory can map to a different physical disk. They allow us to view the system as a tree and abstract the actuall location of the data on disks. 

### File system layout (disk partition)
MBR is a sector 0 and used to boot the computer when starting. \
Parition table: load a specific partition (windows or unix partition)\
Go to the bootlock of the partition loading it. \
Superblock: holds the key parameters about the file system (number of block in the file system). 
I nodes (file info), free space mgt (what is free) and files and directory (actulal files and directory).

#### I-nodes
Gives us information about the blocks in our data and where they are stored. 

### File sharing
Brings up the issue of protection. One approached is to provide controlled access to files through a set of operations such as read, write, delete, ,list and append. \
One popular mechanism is: 

## Files operations
 * create
    * file created with no data
        * read and EOF pointer points to the same place.
    * sets of some attributes
* Delete
    * File is no longer needed
    * storage released to free pool
* Open
    * fetch the attributes and list of disk addresses into main memory.
* Close
    * free the internal table space
    * OSs enforce by limiting the number of open files.

### Read, Seek, Write
* read
    * start to where the read pointer is 
    * specified by the number of bytes that need to be read
* seek
    * Repositions the pointer for random access
    * after seeking data can be read or writen
* write
    * Write from the current position of the write pointer
    * If current position is at the end of file then we are appending.
* Truckate/append:
    * If we want to append to the file we go to the end of file pointer.
    * If we want to trunckate it brings back the end of the file so that less characters are read.

## Open a file
Look into the directory structure to see where the file is which in turns points to where the file is stored. \
When reading the file it points to the per-process open-file table that in turns points to the system-wide table (placed in faster memory) open file table that in turns points to the file location along with the file attributes. 

## File access methods
#### Sequential
in order, one record after another. Convenient with sequential access devices (magnetic tape).
#### Direct (randome)
in any order, skipping the previous recordes
#### Indexed:
In any order, but accessed using a particular key(s); eg hash table, dictionary, database access. }

---
# 13-02-2018
## Tips for the test
* Ends at the section for deadlock.
* Theory either we know it or we do not
* Numerical questions
* Format
    * 12 short answers 6% each
        * deadlock more understanding rather than numerical
        * State banker algorithm 
        * What is safe unsafe
        * Look at the system and see if it is unsafe.
    * 2 long answer 14% each
* Have a calculator

## What is the File alocation problem?
* Contiguous
    * All next to each other
* Chained (linked)
    * Places all over the disk with links
    * We do not need to bother weather there are enough contigous space.
* Indexed 

Disk space is allocated on a block basis. If the block are small then we will access a lot of blocks per program which will take time. \
If we store blocks of a very large size then we can read a block faster yet the wasted space increases. \
As our disk get larger we might want to favor large blocks of data.

## File system layout
They are stored on partitions with their own file system. Always keep Sector 0 of the disk is called the master boot record. This locates the active partition which has a boot block to load the system. \
Maste Boot block tells us the active partition, go to it. \
The active partition has its own boot block, next is the super block that has information about the layout of the system (how much space for system, inodes and the data). Free space mgmt is a strcuture that tells us where in the system can I store my files. Then we have I-nodes that gives us information about the file, I-node number indentifies the file. Then we have the files and directory where the I-nodes will be pointing to.

## Contigous allocation
Alocate space in the file in a contiguous way. 
Advantage: simple, few seeks, easy access, both sequential and random. 
#### random access means getting block number 10 lets say of the file. 
Disadvantages: external fragmentationn may not know the size of the file in advance. 

## Chained linked allocation
Mark a block as in use and one block points to the other block. \
Advantages: No external fragmentation and the file can get bigger. \
Lot of seeking between blocks and random access is difficult.\
Enhancment: Use a in memory table that will tell us where the file is directly on ram known as file allocation table.

### File allocation table
Follow the links in memory (RAM) but not on disk to speed up the process. So when I look for let say the 3 block I will go at the table and look for it using the addresses in the file table then once I found the 3 address I go to that on my disk and retrieve the data. 

## Indexed allocation
Allocate an array of pointers during file creation, the array tells me where my blocks are. This limits the size, if I have an array of 10 then I can only have 10 blocks. \
Advantages: small internal fragmentation, easy sequential and random access. \
Disadvantage: lots of seeks if the file is big. maximum file size is limited by the size of the block since the array size of a block at most.

## Free space managment
Disk space is fixed everytime a file is deleted we must make use of that space. We must hence keep track of the free space.
* Bit vectores
* Linked Lists or chains
* Indexing

### Bit vectores
Use 1 bit per fixed size free area, reserving 1 bit of data to tell if the block is free or not. The bitmap is stored in a disk at a well known address. 1 is free and 0 is allocated. We waste space if the system is almost full. It is easy to use however. 

### Chain/link pointers
The pointer at the start of the list is at a specific location and the list holds the path to all the free blocks. 

### Index pointers
Here we can have sequence of free blocks. An index tells us the size of the sequence it is associated with. 

---
# 15-02-2018
## Finding free space
We want to find space of specific size and the way it is done depend on the free space managment technique used. 
* Bitmaps search for bitmap that contains a large enough sequence of 1 bit.
* Index: store the number of available blocks in each entry
* Chained
    * First fit that is big enough to accomodate the request (fastest)
    * Best fit find the region on the chest that is nearest in size but at least as large as the request (leave smallest amount of holes)
    * Worst fit: allocate from the largest region on the chain
    * Next fit: like the first fit but we continue from where the pointer was last time.

## Unix file system
1. There is the boot block that is used to boot the system
2. The super block contains critical information about the layout of the file system, how to connect to I-nodes telling us where they start and stop so we know where the data starts. It gives us a layout of the system
3. Each I-node entry has the file attribute excetp the name the name of the file is actually stored in the directory. The first I-node number is what points to the root directory. Each files are indentified by there unique I-node number. 

This is the structure of every partition in the disk.

## Inner working of I-nodes in UNIX
So we have the file text.txt. The directory has first the I-node number followed by the filename. Hence the directory is just a list of I-nodes number and names. \
 I-nodes gives me
* Mode
* Link count (keeps track of how many directories contain a name number mapping for that I node)
* Uid (user identifiers)
* Gid (group identifiers)
* Size
* Time
* Disk block (direct pointer)
    * We go to the first address of the I-node we go there and read the entire block at that address.
    * Then we go to the second block and so on.
    * The I-node contains 12 blocks it is a standard
* Indirect pointer to extend the size of the file
    * Single indirect
        * Points to a block that itself points to addresses
        * The block stores pointers that themselves lead to data.
    * Double indirect
        * points to a block that contains addresses that themselves point to a block that itself contains addresses
    * Triple indirect
        * Same as above with three levels of addresses.

## Access file UNIX
1. File descriptor table (one per process, keeping track of open files)
2. Open File Tables: one per system that points to all the files in the system currently open
3. I-node table: one per system that keeps track of all the files open or close. 

So First we have the file descriptor table that maps to a index in the open file descriptor table. Then the open file mapes to the Inode in memory that also points to disk. When a file is close then we update the file that the Inodes point to on disk.

---
# 20-02-2018
Reading data from sector grid?

## Disk memory basics
Disk are made of rotors and for every surface we have a recording data. Each disk/rotor is split into sectors ex 63 sectors per track. Reading data from disk is long because we have to locate the right track and right sector with finally the time taken to read the data.

#### Sector is defined by an angle leaving the center of the disk. So they are wider in length the further away from the disk therefore we are going to assume that closer tracks have more dense information.

Over time disk got better but the access time has not improve by much. 

### Organize data on a disk
Keep all the sectors on tracks that are close to minimize the seek time. It takes on everage half the rotation speed to get to the right sector.

### Read data
When OS gets data request place them in such a way that we can get minimum seek time regardless of the order in which the requests have been sent.

### Disk caching
Once there is one sector that needs to be access, we read the next 100 free and place it in the cache so if we are asked for the data we can retrieve it fast.

### Disk Scheduling
Maxiimize the throughput with concurrent I/O requests from. The Disk driver see which requests get serviced first.

### Strategies
* Random
    * select pool randomely 
    * worst performer
    * sometimes usefull as a benchmark for analysis and simulation
* First Come Fiirst Served
    * No Starvation and is the fairest of all of them.
    * Works well for few processes
    * Approaches to random as number of processes competing with each other.
* Priority
    * Not controled by disk managment
    * Based on process execution priority 
    * Design to meet job throughout put
* Last in First out
    * Service the most recently arriving request
    * Useful for sequential file
    * Issue of starvation
* Shortest Service Time First
    * select the item requiring the shortest seek time
    * In general better that FIFO
    * Randome breaker if required
    * No garentee that the time is actually better.
* Scan back and forth
    * Scan until you reach the end of the disk only reversed when reaching the end.
    * Bad if a new request arrives and it on the opposit side of the disk
* C-SCAN
    * Scins in only one direction to the last track and then returns heands quickly to the begenning
    * redcues the maximum delay
    * We do not count the track when we go back because we are just moving not reading so we can do that fast.
* LOOK
    * LOOK for requests before moving in that direction.
    * When there are no more request we look for requests in the other direction.
* C-LOOK
    * LOOK for requests in the same direction and then go back to the begining when we are done

---
# 22-02-2018
## Grouped arrivals
Even if we do something specific when taking care of the reading method we do so in groups as we do not know what will be requested in the future.

# CPU Scheduling

## Scheduling
Finding the sequence of jobs that optimize the goodness of RAM.
Ex: if we have 4 jobs then we schedule them in an order to reduce the average response time. \
Scheduling itself is the set of policies and mechanisms to control the sequence of jobs to be performed.

### Allocation
Determines what job get to use which resources.

### Our goals for this course
* Single CPU
    * Maximize CPU utilization
        * CPU over head
        * Idling
    * Minimuze job response times
    * Maximize throughput

### Information required
* Resources
    * only one CPU
    * I/O resources
    * Memory used in shared manner
* Jobs
    * Number of process
    * Process creation rate
    * How long process runs
    * How long process uses CPU versus I/O
* Other issues
    * Scheduling policies, priorities.

## Nature of processes
* CPU burst which is the time the CPU is used
* I\O burst waiting for I\O while not using the CPU 

Some processes may require the processes more frequently than others. \
Some crunching program may do a lot of computation and minimal I/O so it is CPU bound. \
Data processes job may do little computation and a lot of I/O so it is more I/O bound.

### CPU bound process
We look at how long each CPU burst is

## Scheduling approches
When to schedule:
1. Switches from running to waiting state
2. running to ready state
3. waiting to ready state
4. Terminates
5. Process gets created

Here 1 and 4 are non preemptive.
### Preemptive
Scheduler forces a process to release the CPU at a clock interrupt or I\O interrupt. 
* Cosider access to share data
* Consider preemption while in kernel mode
* Consider interupt occuring during crucial operations
s
### Non-Preemptive 
Scheduler does not interupt a running process. The process runs until it is done.

#### Short-term Scheduler:
Selects from among the processes in ready queue and allocates the CPU to one of them.

## Evaluation schedulers
#### CPU Utilization
* Capacity used / total capacity
* Varies from 100% to 0% 
* When all processes are blocked CPU is not used

#### Efficiency
Useful work / total work \
Busy waiting or process switching is not useful work

#### Throughput
Number of jobs / unit of time

#### Turnaround time
Time to complete a job, time laps between the completion and arrival events of the first job. No substraction just the time. \
Assume a,b,c,d are jobs that run in order then turarround for each is:
* a = a
* b = a+b
* c = a+b+c
* d = a+b+c+d

Total: 4a+3b+2c+d

#### Waiting time
How long did the job spent waiting in the ready queue.

#### Service time
The time that the job has spent being active total.

#### Response time
Time between the first respons is produced and the arrival of the job. Linked to just one running time for example.

## Scheduler design

### Decision
* select one or more primary performance
* rank them in order of importance
* Performance criteria may nt be independent from each other. 
* Design of scheduler involves a careful balance of the requirements. 

### Scheduling approaches
* Preemptive (foreground interactive jobs)
    * Round-Robin
    * Priority
* Non-Preemptive (Background jobs)
    * First Come First served
    * Shortest Job first

#### First come first serve
Have a queue of jobs the job that arrives first is run first, arriving jobs are inserted at the tails. Performes well for long jobs since the scheduler does not need to make any calculations.

Problems: long CPU jobs may hog the CPU. Short I/O jobs have to wait longer. This can lead to convoy effect as more and more jobs enter the queue.

---
# 23-02-2018

#### Shortest job first
Everytime a new job arrives to the queue we need to run the scheduler to see which one is the new shortest job. 

Problem: need to know or estimate the processing time of each job which cost CPU time. Long running jobs may starve for the CPU when there is a steady supply of short jobs.

Preemptive SJF called SRTF (Shortest remaining time first) demoed below: 

| Jobs | Arrival Time | Burst Time |
| :---: | :---: | :---:|
|P0 | 0.0 | 7 |
| P1 | 2.0 | 4 |
| P2 | 4.0 | 1 |
| P4 | 5.0 | 4 |

1. Run P0
2. P1 arrives P0 has 5 units left so we run P1
3. P2 arrives P1 has 2 units left so we run P2
4. P2 finishes so we run P1 since P4 has 4 units
5. P1 finishes so we run P4
6. P4 finishes so we run P0

Avg waiting time: time complete- time arrived - time run

## Determining the length of Next CPU burst
We can only estimate the length by saying that its running time should be similar to the last time it ran. 
1. tn = actual length of nth cpu burst
2. toa n+1 predictedvalue for the next CPU burst
3. alpha, 0≤alpha≤1
4. Define toa n+1 = alpha tn + (1-alpha)toa n

Alpha is usually set to 1/2. \
From there we can predict every time the running time.

#### Effect of alpha
* = 0 then recent prediction do not count
* = 1 only the last cpu burst counts
* If it is another value then each successive term has less weight than its predecessor. So predicition in the passed have less weight than recent prediction

---
# 27-02-2018
## Round-Robin (RR)
Periodically preempt a running job. CPU supspends the current job when time-slice (called quantum) expires. Job is rescheduled after all other ready job have been executed at least once. \
Its efficiency depends on the quantum length of RR:
* too long: becomes like first come first serve
* too short: a lot of switching so the context switiching time becomes a significant porsion of the running time. 

## Priority-based
Each process is assigned a priority. This does not reduce the running time.
*  Priority + preemption allows process switch when a higher priority job arrives. 
    *  Issue arrise when a lot of high priority job arrives which can lead to starvation for low prioriy jobs.
*  Priority and aging: increase the priority of a process as it stands in the ready queue to prevent starvation

## Comparaison of Running Polocies
We should know how to use them and their advantage vs their disadvantage.
* FCFS
    * Better for long processes
    * Not suitable for interactive jobs (short burst like Word)
    * Negligible processing overheads.
* RR
    * Not suitable for long batch jobs because of context switching overheads.
    * Good for interactive jobs
    * Medium processing overhead
* SJF
    * Long processes can starve
    * Can have high processing overhead.

## Multi-level feedback queue scheduler
We have different level of schedulers that have different scheduling policies. In this case we will look at ones with interactive work stations. What matters the most in this case is the response time to the screen hence response time is the key. Therefore priority is suited for that. We use feedbacks to schedules

### Feedback schedulers
1. Has several queues where each queue represent a priority level. Jobs with highest priority goes to queue 1 and lowest to queue k.
2. If a job arrives at higher priority queue we will preempt a job that is running form a lower priority.
3. If a job does not run to completion we have 2 rules
    1. Either no-preemption
    2. Or preemption

### Priority
Interactive task have a higher priority than non-interactive. Interactive jobs will have short CPU burst follow by long I/O activity. Hence if a job goes to wait before its allotted time quantum then it is priorities. 

First on the arrival each job is in the highest priority queue. We run them for time T. If it is not done by that time then it is not that interactive hence we move it down to the next queue that has a quantum time of 2T. If quantum not completed then preemption then we move it to the back of the queue it came from.\
The priority of a job reduces as time goes by.

### Job escalation in priority
Needs to happen if a job moves from bash job to interactive. We apply that rule so that if a job finishes before its time quantum it is moved to a higher queue. \
We should not penalize long jobs for having a single long burst by the previous algorithm. After some time period S all the jobs are moved to queue 1.

### MLF parameter
* numbe of queues
* The scheduling algorithm can be different per queue
* The method determne to promote or demote a process to a queue
* The method used to determine which queue the process enters when arriving for the first time. 

### LINUX variation (not on exam)
* 3 priority level (priority based scheduler)
    * Each process is assigned a priority number level 1 (1-98) level 2 (99-198) etc...
* first queue Real-Time FIFO and not preemptable unless new ready real-time FIFO thread arrives
* Second queue Real-time round robin (not really realt-ime)
* Third queue Time sharing each process is given the same runtime everytime. 
* Higher priority level have higher quanta

# Memory management
Multiprogramming systems capable of storing more than one probram + data in memeory at the same time. Its fundamental taks is to ensure the safe execution of programs by providing sharing of memory and memory protexction.

---
# 01/03/2018
## Issues in sharing memory
#### Transparency
Several procresses may co-exist unaware of each other and run regardless of the number of processes and location of processes
#### Safety
Processes must not corrupt each other
#### Efficiency
CPU utilization must be preserved and memory must be fairly alocated
#### Relocation
Ability of a program to run in different memory locations.

## Storage allocation
* Information in main memory
    * Program code
    * Read only corde (constants) and read write
    * Address(pointers) or data binding static or dynamic
* Compiler, linker, loader, run-time librairies all cooperate to manage this information.

## Creating executable programs 
Before a program can be executed by a CPU it needs to be compiled, linked and loaded then executed.

### From source to exectuable
Memory is allocated through these steps. Compile -> binary instruction and data. Linker -> links the library. Executable we have variables created upon linking with specific locations. Loader -> relocates the code in RAM.

#### Loading
Places executable code in main memory starting at predetermined address.
* Absolute loading: always load the program at the same location
* Relocatable loading: allows loading programs in different memory location
* Dynamic loading (run-time) loading: loads functions when first called (if ever)

### Address binding (relocation)
Assigning program instructions and data (addresses) to physical memroy addresses is called address binding or relocation.
* static: new locations are determined before execution
* Dynamic: new locations are determined during execution.

#### Static address binding
Compile time or assembler translates symbolic addresses to absolute addresses (here the addresses always remain in the same locations). \
Load time: translate symbolic addresses to relative addresses. The loader translates these to absolute addresses. (note that in this case the addresses will change place between 2 different loads).

#### Dynamic address binding
New locations are determined at run time, the program retains its relative addresses. The absolute addresses are generated by hardware.

### Absolute and relocatable modules
#### Absolute
Load all the code always at the same location when we call a function for example we allways jump to the same place in memory. (relative address is fixed)

#### Relocatable
1. Static

Address is relative to the load module. Always jumping the same amount of memory away from the function call but the address of the function may change between loads. (relative address is computed)

2. Relocatable Dynamic

The address is not at all relative so we do not compute it since we do not know yet where the function will be loaded.

## Simple management schemes
An important task of memory management system is to bring programs into main memory for execution. Earlier allocation technics used by all systems included:
* Direct Placement
* Overlays
* Partitioning

### direct placement
Absolutly no relocation the program is always loaded to the same location in memory. The linker produces the same loading address for every program. Hence we can only run one program at a time. Program larger than memory size cannot run
* Early batch monitors
* MS-DOS

### Overlays
With no memory management the size of the file can be larger than the memory. Developers created technics to split programs in smaller chunks. \
The program is organized in a tree structure. We only load the subparts of the tree when we need them. We always load the root into memory. There could be cycled in the tree.

**Cons**: Overlays had to be made by a person.

### Partitioning
Allows several programs in memory at the same time memory is divided into a number of contiguous regions. Static and Dynamic partitioning.

#### Static partitioning
Memory is divided into fixed number of partitions fixed in size (note that they do not need to have the same size). When running a job we enqueue it to the partion location that best matches the program size. Program that runs in one partition may not be able to run in another, without being re-linked -- absolute loading.

#### Dynamic partitioning
Programs can be loaded into memory as long as there is space this is relocatable loading and is allocated the exact amount that it needs. Addresses in the programs are fixed after loaded OS keeps track of each partition (size and location). 

**Cons**: There maybe hols in memory between partitions that cannot be used by any program waste of space.

### Dynamic memory allocation
#### FF First Fit
Allocates the memory to the first hole that is big enough for the program. Shorter time than BF and WF. Better memory allocation than WF and comparable memory allocation to BF

#### Next Fit (NF)
Start looking to allocate memory after the last location where the last program was allocated

#### Best Fit (BF)
Allocate the smallest hole that is big enough. Shorter time than WF better memory utilization. 

#### Worst Fit WF 
Allocates memory to the largest partition possible. \
Better in certain cases say we have a program of size 90 best fit puts it in a 100 so we have then we get 10 units never used. But with WF if we allocate it to a 200 partition we still have 110 that can be used by the program. 

---
# 02-03-2018
## Fragmentation 
Referes to the unused memory that the memory management system cannot allocate.

##### Internal fragmentation
Waste of memory withint a partition caused by the difference of the program and the size of the partition (static partitioning).

#### External fragmentation
Waste of memory between partitions caused by scattered noncontiguous free space (dynamic partitioning).

## Swaping
Treats main memory as a preemptable resource high-speed swapping is used as the backing storage of the preempted process. So we take the data store it in a storage divice to load new data required for the program.  

#### Medium-term
Everytime we need to run a specific process we dispatch it from memory. If we need to run a process that is not in memory we swap from memory then we run it. The process needs to be in memory before starting to run. It can be done by both static and non-static but it requires a mechanism for swapping in the OS.

### Swapper responsability
swapping device (storage units like a subset of the disk)
* Selection process to swap out
    * Blocked/suspended processes
    * Low priority process
* Selection of processes to swap in
    * time spent on swaping device, priority
* Allocation and management of the swap space on a swapping device. 
    * System wide
    * dedicated

## Memory protection
Management systems most important task is to protect the memory of processes covering the OS itself as well. It can be provided at two levels hardware and software levels:

### Address translation
Memory address mapping. A process is given access to a certain range of addresses let say between 3000 and 5000. When program asks to access memory relative limit register are checked to see weather the jump is not greater than 2000 compare to the start location.  
Then if it passed the test we compute the new address by doing an addtion between the relative address and value of the base register that is in this case at 3000.

## Dynamic relocation
Each program generated address (logical/virtual address) is translated to hardware address (physical address) at the runtime of every reference, by the hardware device known as the memory management unit (MMU).

### Two views of memory
Logical view and the physical view. Each process has its own logical view that makes them believe that it has memory allocated to everything that it needs from let say 0 to Pa. These are then mapped to physical memory with the based address Pa-start and the limits Pa-end.

### Base and bounds relocation
Each program is loaded into a contiguous region of memory. The bound register limits the range of the logical address of each program.  
Easy to implement 2 registers plus an adder and a comparator.

## Segmentation
A segment is a region of contiguous memory and is for *based-and-bounds* relocation where all the process must be stored as there is only one segment per process.  

### Segment number
Loaded into the logical address as it gets loaded into logical address when the process gets first loaded into memory. 

### Segment table same for same vs different programs
When the program is being loaded everything is initialized this is a very dynamic running idea.
We could have a table that has 5 segments so 5 contiguous chunks of memory in physical space. Each chunk has its own base and bounds. Bounds is the logical limit.  
Process imputs the logical address and the Table computes the physical address. The logical address is split into 2 components one storing which segment do I need to go at and the other the offset that I want to go to. Then we check if the addition of the offset does not go over then size of the segment

---
# 13-03-2018
- When a process is created a new segmentation table is made.
- The segments return to free segment pool when program terminates
- Only segments instead of all process may be swapped when wanting to access other processes memory
- Fragmentation problem

## Paging
**Anything that is being pointed by a page table is a page**  
Physical memory is divided into a number of fixed size blocks, called frames. A page table defines the base address of pages for each frame in the main memory keeping track of free and used frames. A page is in logical memory and a frame is in physical memory page size = frame size

### address translation with paging
We have a page number and a page offset for the logical address
Page number gives us the address in the page table and outputs a frame number and a frame offset. The frame number occupies the higher order bits.

### Simple pages
1. Logical address is process specific
2. We get a logical address
3. Go to page table and locate the page number
4. Get the physical address composed of frame number and offset

### Hardware support for paging
- Set of dedicated registers holding the base address for each of the frame
    - Quick to look up
    - But it is costly as we need a lot of registers
- In memory page table with a page table based registers  
    - Gives overhead costs when accessing pages
    - Problems the larger the logical memory leads to larger size page tables. So holding a page table with empty slots of pages is a waste of space since every process has its unique page table which implies that it is not necessary to have full size page tables per processes
- Same as above with multi-level pages tables.
    - Solves the problem directly above

### Multi level pages
To access a page in memory I go to a Top-level Page table that leads me to lower level page tables that will then lead me to the frames addresses. If I have 2 levels then I will have P1 page number and P2 page number. The lower level page tables are only loaded when needed. The way it reduces page table size is that a process will load often the same page tables. 

### Inverted page tables
Maintains a mapping for each physical frame. Therefore the inverted page table is not affected by the logical address space. In this case searching can be very inefficient. We need to search through the page table in order to find the page number.

#### Hashing functions
We get the page number into a hash so that we can access the page in the page table faster. 

## Segmentation with paging
Use both first we go to a segment table that leads us to a page table that itself leads us to physical memory. So we have non fixed segments and fixed pages. Hence the size of the page table we access can vary. 

## Assocative memory
Both segments and paging scheme introduce extra memory references to access translation tables.  

### Translation buffers
Based on the notion of locality (at a given time a process is only using a few pages) a very fast but small associative memory. Memory is used to store a few of the translation table entries. This is known as a translation look aside buffer. 

### Address translation with TLB
We first go to the TLB in the cache if the page number is there TLB hit fetch frame. If page is not there we go to the page table and store the page number and corresponding frame number in the TLB. The more TLB miss we have the longer it will take to retrieve memory, small TLB will hinder the process much more than regular page tables since if we get a lot of misses we would have searched in TLB and in page tables after wards. 

## Dynamic memory allocation
Two basic operations are neededd: allocate and free. In unix these are malloc() and free().  

### Stack organization
The free and malloc are predicatable. Lets say we call A from our main then we push a on the stack, when we return from A we pop its countent. 

### Heap organization
More randome but leads to fragmentation with unuse space which becomes wasted.

## Free memory mamangement
- bit maps
    - 1 bit per block of memory and keeps an array of bits to see what block is used and free
- Linked list
    - Keep track of unused memory.
- Buddy system
    - Taking advantage of binary systems

---
# 15-02-2018
## Info on midterm
No multi level queue full on question rather a subset.

## Buddy algorithm 
When a request for memory comes we sub devide the space by half until we reach the request size. Let say we have 64 pages and the request asks for 8 pages then we are going to devide our space to 32 still to large so 16 still to large so 8 just the right size so we put our pages at the bottom of the memory.  
When we get another request we start at the bottom, if we are asked for 8 pages in this case we have already subdivided and hence had two compartment one used one not used for 8 pages hence we can allocate the 8 pages to the request.  
If we are asked for 4 we would devide the 8 by 2 to get the right side.  
When used spaces are freed we fuse them together if they are next to each other. If we do not have enough space in our memory based on our splits we can fuse adjacent sections with eachother to get larger space.

### Reclaiming memory
When used by one process it is easy. However when used by multiple process uses that block 
- Dangling pointers: occur when the original allocator frees a shared pointer
- Memory Leak: Happen when a process exit without freeing the space hence we think it is still being used.

### Reference count
Use a counter to keep track of references, when the counter goes to 0 the memory is freed. 

### Garbage collection
Implemented by algorithms following mark/sweep. Garbage collection is however expensive.

## Principle of locality
- Locality of reference
- Locality
- Spatial locality
    - The idea that a process will access memory location that are closed to each other. Caused by arrays for example. In order to take advantage of that we can prefetch that data to cache before the process asks for it as it is likely that the process will need it again.
- Temporal locality
    - Tendency for a process to reference memory locations that have been used recently
    - Ex: loops
    - Technique: make sufficient room in cache to hold that data.

# Virtual memory
Either use paging or segmentation to allocated it on physical memory.  
The overlay idea is now however done by the OS.
## Principal of opperation
Create the illusion of memory that is as large as a disk and as fast as memory.  
Locality of references only puts in physical what the program actually needs.
- main memory (RAM)
    - in ram fast but small
- Paging device (Disk)
    - in disk large but slow
- None
    - Has not been alocated
    - This is what will cause a segfaut and is not good at all.

## Missing pages
1. The page table is extended with an extra bit that tells us whether the page is present. So when we fetch an address we actually look at that bit. 
2. If the present bit is not there then we have a page fault
3. The OS marks it as blocked (waiting for page) because we have to do a disk access.
4. Finds an empty frame or make a frame empty
5. performs an I/O operation to fetch the page to main memory if it cannot find the page on disk seg fault.
6. trigers a page fetched eveny (form of I/O interrupt) to wake up the process.

## Multi level pages reviewed
Before we would go to the first page table then to the second level page table. Now instead of going to main memory with the frames some addresses will be pointing to disk and that is when we will get a page fault which will hence get their present bit set to 0.

### Additional hardware support
What if we have uninteruptable code as bellow 
 ```
fetch (instruction); decrement D0;
if DO is zero, set PC to next else increment PC
```
What if the address NEXT and the instructions are on two different pages, Page Fault from where? where do we start the instruction?

### Possible support
- Instructions are undone and restarted
- Resumes from the point of the fault
- Prefetch all addresses before executing.

---
# 16-03-2018
 ## Basic allocation policies
 - The fewer frames allocated for a program, the higher the page fault rate
 - The fewer frames allocated for a program the more programs can reside in memory decreasing the cost of swapping. 
 - Allocating additional frams to a program beyound a certain number results in little or only moderate gain in because a process uses generaly resources in a certain region more than others.

## Fetch policy
### Demand paging (pure)
Start a program with no page loaded, wait until it references a page, then load the page

### Request paging
Similar to overlays, let the user identify which pages are needed (not practical, leads to over estimation and also user may not know what to ask for)

### Pre-paging
Start with one or a few pages pre-loaded. As pages are references bring in other not yet referenced pages too.
 ### Opposit 
 Cleaning policy when to write a page back the content of a frame to the disk.

 ## Placement policy
 Find a free frame:
 - Easy since page and frame have the same size.
 - Segmentation requires more careful placement especially when not combined with paging with pure segmentation we must consider free memory mamnagement policy.

With the recent developement of non-uniform memory access (NUMA) distributed memory multiprocessor systems, placement becomes a major concern. 

### FIFO
the frames are treated as a circular list, the oldest page is replaced

### LRU
the frames whose contents have not been used for the longest time is replaced.

### OPT
The page that wihll not be referenced againf or the longest time is replaced (prediction of the future, purely theaoritical)

### Random
A frame is selected at random.

### More on replacement
- Scope
    - Global: selected among all processes
    - Local: select a frame from the faulted process
- Frame locking
    - frames that belong to the kernel or are used for critical purposes, may be locked for improvement performance.
- Page buffering 
    - Grouped into 2 categories, unmodified frames with clean pages and modified frames with dirty pages.

## Algorithm
All frames are in a circular queue, when a frame is needed the pointer advances to the first frame with a 0 used bit. As the pointer advances it clears the used bits. Once a frame is found, we replace the frame and marked it as used, the hardware on the other hand, sets the used bit each time an address in the page is referenced.

Some system also uses a dirty bit to see if the memory has been modified in this case we give preference to dirty pages since replacing a dirty pages costs more than a clean one.

If the clock moves quickly then that means that we have a lot of page faults, if the clock moves slow then the page fault is quite low.

---
# 20-03-2018 (After 2nd Midterm)
## Thrashing
The number of process in memory determines the multiprogramming (MP) level. The effectiveness of VM is closely related the the MP level. Thrashing: the system is spending its time moving pages in and out of memory and hardly doing anything useful.  
Hence the goal generaly is to find the best spot between CPU utilization and multiprogramming level.  
Removing processes for:
- lowest priority
- faulting process
- newest process
- process with lowest memory usage
- process with highest memory usage 

## Working Set
The set of pages taht are accessed by the last x memory references at a given time t and denoted by W(t,x)  
**Denning's Wirking Set Principla states that:**  
- a program should run if and only if its working set is in memory
- A page may not be victimized if it is a member of the current working set of any runnable (not blocked) program.  

The down side of the working set is that for every process we need to keep track of what pages where accessed last.  
Solutions:
- Maintain idl time value (amount of CPU time received by the process)
- Update the information once in a while rather than every time scan for used bit and set there idl time back to 0, resets the used bit to 0 for all pages.
- Pages with lowest idle time are part of our working set

## Page fault frequency
How frequently are page fault occuring P = # page fault / time period T. Where T is the critical inter page fault time. When a process runs bellow the lower bound a frame is taken away from it (its resident set size is reduced). Similarly, an additional frame is assigned to a process which runs above its upper limit. 

### Current trends
- Larger physical memory
    - page replacement is less important
    - less hardware required to support replacement policy
    - Larger page sizes
        - Better TLB coverage
        - Smaller page table
- Larger address spaces
    - sparse address spaces
    - single (combined) address spaces (part of the OS part for the user processes)
- File Systems using virtual memory (not covered)
    - memory mapped files
    - file caching with VM

# Virtual Machines

## General Idea
- Multi threading
    - Have virtual CPU that gives us the illusion that enables threads to persist their computation on the CPU as switching happens - without clobbering each other.
- Multi processing
    - Adds memory virtualization to multi-threading. Every process has its own memory view
    - File system is sahred and so is the kernel. However, kernel is a protected resource. In a perfect scenario, we do not need to worry about this sharing. 
- Beyond is virtual machine.

## Virtual machine
- Hardware (shared physical machine)
    - Virtual machine monitor
        - Virtual machine 1
        - Virtual machine 2
        - Virtural machine 3

Definition: Provides an environment for programs that is essentially identical to physical machine  

### Motivation for VM
The idea came from the need of file sharing system. TTS was tried by IBM then stopped they then tried CMS. The main goal was to get a multiuser time-sharing system.  

### Components:
- Host actual phyisical machine
- VMM or hypervisor, creates the virtual machines on top of it and runs them
- Guest processes provided with virtual copy of the host.

#### Benefits
- Host system protected from VMs, VMs protected from each other.
    - Virus will not spread
    - Sharing is provided through via shared file system.
- Freeze, suspend running VM
    - They can be copied and move somewhere else
    - Snapshot of a given state can be taken and we can go back to it.
    - Clone by creating copy and running both original and copy
- Great for OS research, better system development efficiency
- Run multiple, different OS on a single machine.
- Templating create an OS + application VM, provide it to customersm us it to create multiplle instance of tha combination
- Live migration move a running VM from one host to another
    - No interruption of user accesses
- All those featurs taken together -> cloud computing.

---
# 22-03-2018
## Data centers 
Multiple physical servers, faster deployment, easier maintnance for the client, cheaper with economies of scale, we are working on energy efficiency now.

## Physical Vs virtual
- Physical
    - Hardware
    - Kernel checks all the instructions of the user and executes them
- VM
    - hardware
    - Virtual machine manager
    - then the VM 
    - Then the kernel of each VM.

## VMMs types
0. type 0 hypervisor: just hardware support via firmware
1. type 1: OS like software build to provide virtualization also includes general OS that provide standard functions as well VMM funtion
2. runs on standard OS systems but provide vm feature to guest os

## Building blocks
Generally difficult to provide an exact duplicate of underlying machine. Most VMMs implement a virtual CPU (VCPU) to represent state of CPU per guest as guest believes it to be, when guest get swap by VMM, information from VCPU loaded and stored

### Requierments
An efficient machine is an **efficient isolated duplicate** of real machine

### Implementation privilege/kernel mode
The virtual machine runs on the user mode of the physical machine. They cannot all run on privileged mode (defined by the hardware) in the hardware so the machine must execute in user mode. When a privilege instruction is being executed in the VM then we have a problem.  

### Sensitive instructions
Instructions that behave differently whether in kernel or in user mode. `cause a fault only in certain systems not all` 
- Control-sensitive instructions
    - Affect allocation of resources on the VM
    - chenge processor mode without causing trap
- Behaviour sensitve instruction
    - Effect depends on location in real memory or on processor mode.

#### Privileged instruction
Cause fault in user mode work fine in privileged mode. We use a trap mode catching instructions that cannot be run in the user mode to make them run in kernel on the pysical machine

### Theorem
For any conventional third-generation computer, a virtual machine monitor may be constructed if the set of sensitive instructions for that computer is a subset of the set of privileged instructions.

## The (REAL) 369 architectute
2 exections mode supervisor (kernel) mode and problem mode (user), all the sensitive instructions are privileged instructions. The kernel sensitive information however will not be trapped which will cause different output on kernel or user


| | user mode | Privileged Mode
| --- | --- | --- |
|non-sensitive| executes fine | executes fine
|errant instructions | Traps to VMM; VMM causes trap to occur on guest OS | traps to VMM; VMM causes trap to occur on guest OS |
|sensitive instructions | trapst ot VMMl causes trap to occur on guest OS | traps to VMM; VMM verifies and emulates the instructions |

## Trap and emulate
Actions in the guest that cause a switch to kernel mode must cause a switch to kernel mode on VM.

- How does a switch happen?
    - Attempt to run in on user causes instruction to get Traped
    - VMM gains control, analzes error, executes operation as attempted by guest.
    - Returns control to user mode
    - Known as trap and emulate
    - Most virtualization products use this at least in part
- User mode in guest runs at same speed as if not a guest
- But kerneel mode privilege mode code runs slower due to trap and emulate.
- CPUs adding hardware support, more CPU modes to improve virtualization performance.

### Implementation of trap and emmulate
User processes are in guest user mode then we have guest kernel mode with the OS and privileged instructions.  
Then the VMM traps the privileged instructions and emulate the action to give the same outcome as if on a real machine then update the state of the system such as VCPU.

## Intel x86
Sensitive instructions are not a subset of privilegde instructions so they cannot be trapped and emulates

popf: pops word off stack, setting processor flages according to world's content. sets all flages if in kernel mode, ignore in user mode. Hence this intruction will have a different outcome on this computer.

---
# 27-03-2018
The VMM runns in privileged mode.
#### POPF
pop word off stack, setting processor flages according to word content. set all flags in kernel none in user mode
## Building Block - Trap and Emulate
Guest OS runs in user mode and the VMM runs in kernel mode.  
Attempting a privileged instruction in user mode will cause an error -> trap. So the VMM gets control analyse the error executes operations as attempted by guest. Returns the result to the guest updating the virtual CPU as well. At the end the user mode in VM runs as fast as physical user mode but a kernel instruction on VM will run slower than the ones in physical kernel.

### Rings
Allows different leveles of privilege. the lower the more privileged. Given a virtual machine when running in ring 1 will have more privileges than just ring 3.

| rank | type |
| :---: | :---: |
| 0 | kernel |
| 1 | guest OS |
| 2 | |
| 3 | app |

For popf app that sets flag in kernel and non in user. In the case of rings we interrupt-disable flag in ring 1 where it ring 0 it would not but at least now we are giving some additional privileges to the guest modes.

### What to do?
- Binary rewriting
    - rewrite kernel binaries of guest OSes
        - Replace sensitive instructions with hypercalls (trap to the hypervisor VMM)
        - do so dynamically
- Hardware virtualization
    - fix the hardware so it is virtualizable
        - Processor allow virtualization
- Paravirtualization
    - Virtual machine differs from real machine
        - More convenient interfaces for virtualization
        - hypervisor interface between virtual and real machine
        - guest OS is modified

---
# 29-03-2018
## Binary rewriting
Privileged mode code run via binary translator, replace sensitive instructions with hypercalls. However this remains a very slow process since we must translate instructions. Hence translated code is cached usually translated just once so the instructions can be reused multiple time.  
CPU knows what modes it runs on and the instruction outcome depends on that state.

## Building Binary translation
1. If guest VCPU is in user mode, guest can run instructions natively.
2. If guest VCPU is in kernel mode (guest believes it is in kernel mode but the physical machine is not)

Here there is a problem and we solve it this way:
1. VMM examines every instructions guest is about to execute by reading a few instructions ahead of program counter (this is to speed up the process)
2. Non-special instruction run natively
3. Special instructions translated into new set of instructions that perform equivalent tasks (ie: changing the flags in the CPU).

Code reads native instructions dynamically from guest, on demand, generates native binary code that executes in place of original code. Test have shown that booting windows XP as guest causes 950000 translations at 3 microseconds each which slow it down by a factor of 5% compare to regular OS.

## Fixing the Hardware
Use rings with 0 as the most privileged and then other levels, 0 is the kernel mode but we can allow other programs to have more privileged than just being in user mode. We can for instance run the guest OS is in ring 1 and run the processes of the user in the guest OS in ring 3.

Intel Vanderpool technology created a new processor mode ring -1 that is only used by the hypervisor (VMM). So that the OS kernel running on top of the VMM will have ring 0. Which means that both OS of pysicall and virtual machine will run on ring 0.

## Paravirtualization
The guest OS is modified so that it is aware that it is a VM. This is done by removing some of complexity needed to support unmodified OS, ie: have the binary rewritting already done in the OS rather than having to do it dynamically. The applications themselves see no changes.

---
# 03-04-2018
# Protection
Referes to the activity of controlling the access of programs, processes, or users. Enforced through password associated with a user. Policies: what is enforced (access to mycourse page). Protectin was first thought of in multi programming to protect one program from another. Kernel memory is shared between processes but the user memory is not.

## Introduction
- Reasons
    - Need to ensure that each program component in the system uses resources consistent with stated policies.
- Reliability
    - Be able to detect event at the interfaces between subsystems. 
    - This detection can often prevent a malfunctioning subsystem from contaminating a healthy subsystem.
    - Distinguish between authorized and unauthorized usage.
- Where
    - Done by the OS and applications
    - Application created entities should be protected by the application itself and not only depend on the OS itself.

### Policy
State what are the rules that must be respected

### Mechanisms
How the policies are going to be enforced.

## Terminology
- Objects refers to both hardware and software objects. Software include programs, files, semaphores. Hardware: CPU, memory segments.
- A computer system is collection of processes and objects
- *need-to-know* a process needs to have accesss to only those objects that are essential for the process.

- A process operates in a protection domain that specifies the resources the process may access.
    - Each domain defines a set of objects and the types of operations that may be invoked on each object. Domain may not always be disjoint.
    - The ability to execute an operation on an object is an access right.
    - A domain is a collection of access rights -  each of which is an ordered pair <object-name,rights-set>

## Protection domains
Assocation between process and a domain may be static or dynamic
- static
    - we should be able to change the domain contents
    - if a process reads in phase 1 and writes in phase 2 the needs will change with time.
    - With static we will have both read and write permision violiating the *need to know* principal since read is not essential to part 2. 
- dynamic
    - switch domain as processes need new permissions controling that switching.

---
# 05-04-2018

## Domains
- Each user may be a domain - set of objects that can be accessed depends on the user identity
- Each process
- Each procedure

## Protection in UNIX
Domain is associated with a UID, when executing the program Z will have the privileged associated with UID A. When executing a file Y with setuid bit set and the fie Y is owned by UID B, the process will have associated with UID B for the duration of execution of Y. setuid and setgid access added using chmod.
**uid bit: unix access right flags that allow user to run an executable with the permissions of the executable's owner or group**

### Protecton domain in unix example
- Printer spooling problem
    - various commands to access line printer queues are setuid files (to user root) so the UID bit is set to 1 because we need to access the printer port/dev/printer that is owned by the root.
- Problem
    - If the user manages to create a file with root as user ID and setuid bit set, then the user can become the root and do anything
- Alternate aproach
    - Restrict all setuid programs to reside in a predefined directory- cna have no secret setuid programs- because only those residing in the system directory can run.
    - Have a deemon process to control access to protected objects allow no domain changes through setuid mechanism

## Process Based Protection Domains
Typically a function running within a process inherits the process' access rights. Use **sandboxing** restrict access to limited resources (file resources, CPU time, memory space) and terminate function if conditions are violated.  
Sandboxing is a security mechanism for separating running programs.

## Access matrix
- Provides us a *scheme* for specifying a variety of policies
- *Need a mechanism* that can implement the access matrix and ensure that the semantic properties hold.
- Access matrix specifies what a process executing in domian D_i can accesss and what operations are allowed on the accessed objects.
- Used for static or dynamic association
    - Dynamic are implemented by domain switches.

### Matrix in theory

| Object/Domain | F1 | F2 | F3 | printer | D1 |
| :---: | :---: | :---: | :---: | :---: | :---: |
|D1| read | | read | | |
|D2| | | | print |switch |
|D3| | read* | execute | |
|D4| read write | | owner read write | | switch |

Note that the switch present in the matrix only exists for dynamic domain so that a switch can only be done when allowed.

### Access matrix modification
frequently it becomes necessary to change the access matrix itself
- Grant permissions to new entities
- Transfer permisions

Propagation of rights should however be limited. We denote when a permission can be copied within the same column with an asterix (read*,write*,execute*). For the **owner** privileges in a domain we can add or remove privileges to other domains in the same column than owner is in.

### Access matrix size
Access matrix can be very spares in general.  
Implemented in two ways:
- By columns/object (access control list)
- by row/domain (capability lists for domain)

## Access control list
Associate each domain with a list of domain (A,B)  
F1 -> A: RW; B: R  
F2 -> A: R; B: RW  
For each access list we only store domains that have rights. Instead of user ID we can have group IDs and user IDs. Using group IDs we can introduce the notion of roles.

---
# 06-04-2018

## Capability control list
Each domain rights are stored by domains. For every domain we store what its access rights are. Users are in user space and the rights list are stored in the kernel space.

## Access control scheme
User logs in with a password to ensure the user is really who he says he is. Then we have a reference monitor who every time the user makes a request for an object sends it the the authorization database where the access matrix is. Finnally then the user can access his request.

## Trusted systems
Most computer systems leak information. Damages that cost a lot of money due to viruses, damages data. In theory a secure system can be build but security must be the primary goal of the system.

### Reasons
1. Support for legacy systems e.g Windows
2. Only way to build a secure system is to keep it simple featues addtions complicate things. e.i ASCII (just text) emails and emails with binary attachments are difficult to trust. (Binary attachment could be a malicious program).  
Same idea with HTML that is secure when passive than HTML with applets

To build a secure system, the model at the core of the OS should be easily understood and verified.

## Trusted computing based
1. System that has formal security requirements and meet these requierments.
2. At the center of a trusted system is a minimal trusted computing base TCB. Should also be separate from the OS
3. TCB consists of hardware and software necessary for enforcing the security rules.
4. If TCB works according to specs the system cannot be compromised.
5. TCB consists of most hardware, a portion of the OS-kernel and most if not all of the user programs that have super user power (setuid programs).
6. All operations should go through the reference monitor in the TCB.

## Multi-level security
Most OS have **dicretionary access control**
- Individual users determine who may read, write, execute their files and other objects.
- In some environments, tighter security is necessary
- Organization will set the access policies that cannot be overridden by individual users without exception

Stricter access control is provided by **mendatory access control**
- Regulate the flow of information in addition to the standard discretionary access control through stricct rules that are implemented system wide.

---
# 10-04-2018
## Bell-la Padula Model
Developped by the military to restricts the information flow.

- Simple security property: a process running at security level k can only read objects at its level or lower
- The *property: A process at security level k can only write objects at its level or higher.

This is called a read down and write up model to prevent information leak from a higher level to another one.

## Biba model
Problem with Bell_la Pendula: lower level security user cna corrupt the files of a higher security level not respecting the data integrity.

This model is the exact opposit of Bell-la Padula to counter the negative affects.

## Trojan horse
Malicious computer program which misleads users of its true intent.
- keylogger that sends all recorded key strokes back to the attacker.

### Attack system
Used a back pocket file so that when the victim executes the trojan we go to a file we are interested in, read the content of the file and write them to our back pocket file. The back pocket file has the following privleges, read write for the hacker and write for the victim.

### Defense system
Security propery, *-property is violated if we try to do the above since we assign a security level are assigned to subjects so if the hacker has a lower security level than the victim then the write to the back pocket file will not happen.

## Orange book security
Divides OS into seven categories based on their security properties. Today it has been replaced with a newer DoD standard - However still used as a guide.

### Orange book categories
- Level D: No security requierments at all (Windows 98)
- Level C: Environment with cooperative users, authentification login, ability for user to decide which files are made available to other users C1. C2 adds DAC to other users.
- B,A All controlled users and objects to be tagged using security labels interpreted according to La-Bell Padula model.

## Covert Channels
Provide a way to comunicate between a server and a collaborator. In this case the covert channel leaks information to the collaborator without the client noticing.

It is hidden from the access control mechanism, instead of sending the actual data to the collaborator it may send data for example at a certain speed or with certain patterns that can be decoded into information.

## Intruders
Unauthorazed individual exploiting a legitimate user account or authaurized individual missusing privileges.
- guessing passwords
- Distributing pirated software
- viewing sensitive data without authorization

---
# 12-04-2018
## Intrusion detection
Detects attempted or successful intrusion and can take different forms:
- Time of detection: real time or afterwards
- Types of input examined: shell commands, process system calls, network packet headers or content.
- Responsible types: killing the offending process, alerting the administrator, leading the intruder to a **trap**

Multiple imputs must be analysed to detect intrusion.

### Signature-based detection:
Attempts to characterize attack conditions and detects whether any such behavior occurs

### Anomlay detection
Attempts to characterize normal conditions and detects whether anything other that normal behavior occurs.
- Auditing and logging
    - Keep track of normal behavior over time and when something occurs we see if that behavior respects the usal behavioural pattern
- Tripwires
    - software security tool to moniter and alert on specific file changes on a range of systems which can be donw through system call monitering

# Dining Philospher 
N philosophers seated arround a circular table either thinking or eating, N plates and N forks.
- A philiospher that wants to it take the fork to its left and then to the right in order to it
- A fork can be used by only 1 phylospher at a time
- Devise an algorithm th allow all phylosphers to it.

Avoid deadlock, livelock, starvation

## Attempt 1
If each philospher grabs left fork the semaphore is at 0 but now we have a deadlock
```
sem fork[n];
while(1)
{
    P(fork[i])
    P(fork[i+1%n])
    //eat
    V(fork[i+1%n])
    V(fork[i+1%n])
}
```

## Deadlock solution
All at most n-1 philisopher at the table but not really practical in real life.

## Attempt 2
```
sem fork[n] = {1}
sem mutex = 1
sem stopped = 0
int eaters = 0
philospher[i]
{
    while(1)
    {
        get_fork(i)
        put_fork(i)
    }
}

get_fork(i)
{
    get_waiter_permission();
    wait(fork[i])
    wait(fork(i+1)%n)
}

put_fork(i)
{
    signal(fork[i])
    signal(fork(i+1)%n)
    inform_waiter()
}

get_waiter_permission()
{
    wait(mutex)
    eaters++
    {
        if(eaters>n-1)
        {
            signal(mutex)
            wait(stopped)
        }
        else
        {
            signal(mutex)
        }
    }
}

inform_waiter()
{
    wait(mutex);
    eaters--
    if(eaters == 4)
    {
        signal(stopped)
    }
    signal(mutex)
}
```

# Final exam format
- 20 short questions 3 points each
- 4 long or multi part questions 10 mark each
- Similar to sample exams posted fall 2016
- May have programming questions and algorithm

## Material
All the lecture slides seen in class (excluding trojan horse, covert channels, Meltdown, moniter)

**THE END**

--- 
