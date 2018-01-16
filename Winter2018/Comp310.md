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




