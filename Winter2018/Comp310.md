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
    * computer cost milions
    * hard to operate and program
    * No need for OS just one operation at a time
    * Idle was very expensive
* Second generation
    * Operators fetche compilers and card decks to get job reade
    * Batch systems came up then to optimize processes.
    * Program where executed in batches and programing could now be done onfline allowing us to start subroutines that can be used
* Third Generation
    * Memory partition used so that we can run multiple jobs together. Seeming paralellism, the CPU swaps between jobs. 
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