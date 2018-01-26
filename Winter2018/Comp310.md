# COMP310

## Intro
### Operating system history
* vaccum tubes
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
| Time-Sharing systemss           | Security, Response time, CPU scheduling       |
| Batch Systems (Super computers) | Maximize CPU usage                            |
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