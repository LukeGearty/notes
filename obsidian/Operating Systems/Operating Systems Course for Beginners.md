 [Link](https://youtu.be/yK1uBHPdp30?si=CSd6cUn53z_I5CB5)

# 06/02
## What is an OS?
An interface between user and computer
Input/Output, Memory, and CPU
Within CPU, there is the control unit and arithmetic logic unit
CU: Unit that controls timing/control signals, controls the operations, responsible for sequential execution
ALU:  Arithmetic Functions and Logical Operations - adders, subtractors
```
		Memory
		  |
Input  - CU   - Output
		 ALU
		CPU (Processor)
```

Memory:
Primary - Main Memory - RAM, ROM, Cache, Registers - volatile - faster to access, expensive and size
Secondary Hard disk - nonvolatile - slower to access, less expensive, greater size

Where is secondary memory?
Von Neumann Architecture: part of Input/Output devices - monitor, keyboard, mouse, etc

How are programs executed in VNA?
Test.c -> Compiler -> .exe -> OS loads into main memory
When program is loaded in memory for execution: stored program concept
CPU can't execute directly from hard disk. Why?
Hard disks are slow - CPU is fast, can't fetch that much data from hard disks
OS needs to transfer program from secondary memory to main memory
Then there is the sequential execution of instructions
Any program that has to be executed must be stored in main memory

Os consists of several Modules:
Process Manager, Memory manager, file manager, device manager, protection manager.
All of these combine to make the Kernel - also known as the Core/Nucleus
OS as an interface between user and hardware
OS as a resource manager: responsible for allocation, deallocation, management
OS as a control program - a program that control all the operations of the computer
A set of utilities to simplify application development

What are the goals of the OS?
Convenience - user friendly
Efficiency - utilization of resources
Reliability - For the purpose the OS was designed, it should be achieved
Robustness - strong enough to bear errors
Scalability - ability to evolve
Portability - ability to work across different platforms

Some OS prioritize different goals. UNIX is less convenient than Windows for example. 

## Types of OS
1st Generation of Computers: No OS
2nd Generation: Magnetic Tapes introduced, No OS
3rd Generation: Magnetic Disk - hard disk
Divided into two parts:
Uni Programming: ability of an OS to hold a single operating system in Memory
OS get loaded in main memory at the time of booting - system area
user area - programs get loaded, in uni programming only one program can be here at a time
If there is a need of I/O devices, CPU is empty - efficiency issue
Less Throughput - # of programs completed in a unit of time
Ex: MS-DOS 
	Command based/ No GUI
Multiprogramming: more than one program can be loaded in memory 
The CPU can only execute/work on a single program at a time. Only one program will run in any case
	Even in multiprocessor/multiple CPUs.
Maximize CPU utilization, efficiency, throughput
Impression of Multiplexing of CPU among differnet programs
Multitasking: Program = Task (UNIX vs Windows) = Job
Lets say 4 Jobs are in Main Memory. job 1 has the CPU, then goes to IO, Job 2 then gets the CPU, Job 3 and 4 are waiting for the CPU. 
Types of Multiprogramming:
Preemptive: forceful deallocation of CPU. If a high priority job comes into memory and a lower priority job is being worked on. The higher priority job gets the CPU before lower priority job finishes.
Non Preemptive: Nobody forces the program out of the CPU. Job releases the CPU voluntarily - all instructions are complete, it needs I/O, or a system call happens
Drawbacks of Multiprogramming:
Starvation/ Indefinite waitng for other programs - Non-preemptive

Preemptive improves responsiveness by forceful deallocation to get a chance
Preemption is based on either time or priority. A job is given a set amount of time Time-sharing

Architectural Requirement (Hardware requirement) of multiprogramming:
-Secondary Storage Device (IO): DMA (Direct Memory Access) compatible
	Efficient data transfer b/w secondary storage and main memory
-Memory system should support address translation
	Running Program generates logical address (type to access its instruction or data from main memory) 
	Physical address is needed to access the instruction or data from main memory.
	There must be a Memory Management Unit to convert logical address to physical address (actual address in main memory)
	With physical address, each program would directly access physical memory locations - a buggy program could overwrite memory used by another program corrupting data and causing crashes. Logical address prevent this by creating an abstraction layer. Programs operate with logical addresses and the OS translates them to physical addresses, ensuring programs don't interfere with each other's memory space.
-CPU - must support dual mode operation (User mode & Kernel Mode)
	user mode: non privileged mode
	kernel mode: privileged
	PSW Register: Processor Status Word has a bit that signifies the mode - 0 for kernel, 1 for user
	It is sometimes necessary to shift - mode shifting is required. 
	Kernel Mode: Executing code has complete and unrestricted access to the underlying hardware. Reserved for lowest-level, most trusted functions of the operating system. Crashes are catastrophic.
	User Mode: executing code has no ability to directly access hardware or reference memory. Code must delegate to system APIs to access hardware or memory. 

Timestamp: 00:00:58
## 06/04
User Applications are pre-emptive
Kernel Programs are non-preemptive

Whenever we need some OS services we need to shift user mode -> kernel mode, and when it completes it needs to go back.
API/SCI (System Call Interface)

```
{
	int a, b, c;
	b = 1;
	c = 2;
	a = b + c;
	f(a)
}
```
This code goes to the compiler, and it translates it to binary, and it runs in user mode.
But let's say f is this:
```
f(int a) {
	a++;
	printf("%d", a);
}
```
Where is printf defined? In the header file, like stdio.h in C? No. It is defined in the library files. The header file contains the function prototype, not the actual implementation. Header files are used for type checking and syntax checking.
Whether it is predefined function or user defined, it is executed in user mode because it is executed in user mode. 
Compilers are a user application, not a part of the OS. 
The above code is user mode.
```
main() {
	int a, b, c;
	b = 1;
	c = 2;
	a = b + c;
	fork();
	printf("%d", a);
}
```
fork is a system call that creates a child process.
User defined functions are executed in user mode, compiled into Branch & Save Address (non privileged instructions)
Fork is compiled into Supervisory Call (SVC), executed in kernel mode
SVC at runtime is  A software interrupt - informs OS of some activity.

When the compiler finds there is an interrupt, it generates an SVC. SVC generates a software interrupt. 
Every interrupt has a routine to serve: Interrupt Service Routine
Changes mode in PSW to go from user mode -> kernel mode. 1 -> 0
How to find where our fork is?
	First activity by ISR.
	OS maintains a dispatch table (data scrutcture in RAM) - tells what all services OS provides
	Goes to fork, execute instructions 1 by 1, then changes from kernel mode -> user mode 0 -> 1

OS routines are accessed through API/System Calls
fork -> SVC -> ISR -> PSW
			  |-> Dispatch Table 

User Process Executing -> Calls System Call -> Trap Mode Bit=0 -> execute system call -> Return Mode Bit=1 -> return from system call
Kernel Mode: 0
User Mode: 1

0 -> 1 = Kernel Mode -> User Mode
1 -> 0 = User Mode -> kernel Mode
User mode is preemptive
Kernel Mode is atomic, mode bit 0, privileged

Parent Process
	Child Process
		Child Process
			Child Process
		Child Process
	Child Process
		Child Process
	Child Process

Timestamp: 1:29:00

# 06/05

Program vs Process: 
A program is in the hard disk, and a process is a program in main memory/execution
A process is an instance of a program
Dat - static (fixed size) or dynamic (memory allocated at runtime)
At what time of program execution, memory allocation of static time?
Not at compile time - meant for checking syntactic correctness and generating the code.
If you compile the program but not run (gcc test.c for example), memory allocated at CT would be waste
Compiler only decides how much memory allocation
**Load Time** - when the program is loaded from disk to memory for execution

int n, a[n];
scanf("%d", &n);
This isn't allowed in C, allowed in C++ (technically)
malloc is used to dynamically allocate memory in C

int n, \*ptr
scanf("%d", &n);
ptr = (int \*) malloc(sizeof(int)) \* n;
As an object is to class (OOP), process is to the program.

From a developer's perspective:
Process is an abstract data type
Each data structure has four parts:
definition, representation/implementation, operations, attributes

Process has: stack (activation records of function calls), heap (area in memory for dynamic memory allocation), data (static data), text(code section)
Activation Record contains information and space of local variables of function and return address of that function.

Process operations:
create
schedule
execute/run
block/wait
suspend - move from memory to disk
resume - move from disk to memory
terminate

Attributes:
Identification: Process ID, Parent Proccess ID, Group ID
CPU Related: Program Counter (Points to next instruction), priority, state, burst time)
Memory Related: size, limits(process is in which area of memory)
File Related: List of files in use
Device Related
Accounting Related

PCB - Process Control Block - contains all the information for the process - a kind of process descriptor - stored in memory
Total content of PCB - Process context/environment

During the process life time, it goes from one state to another
1. New: gets created, resource allocation
2. Ready: ready to run on CPU - Multiprogramming OS
3. Running: Process is executing instructions on CPU
4. Block/Wait: leaves CPU, needs to perform I/O, execute system call (reading something from device, writing something to device)
5. Terminate: Process has completed all of its instructions - resource deallocation

Scheduling Dispatch chooses which process will be chosen for CPU
N


Timestamp: 00:02:02:15

# 06/06

Multiprogramming Model:
Process starts in new -> Ready -> scheduling dispatch chooses to run -> Running -> I/O or system call -> Blocked/Wait -> I/O or system call completion -> Running -> waits for scheduling dispatch to run process again

The existence of the Ready state implies the multiprogramming system, because uniprogramming can only have one process in memory at a time
This is a model of non-preemptive, because the process only exits the CPU when there is an I/O or system call. Preemptive will have process shift between running and ready.

When process completes from Running State, it terminates. Exit in the end of the program.

When Process is in ready, running or blocked, it is in main memory. When it is in new, it is entering main memory. 

## Suspension
Transferring processes from main memory to disk, from Ready state. Running it is being executed by CPU, blocked it is waiting for I/O. Ready is the most desirable state for suspension. It is not running, it is not waiting for I/O.

Can suspend from other states, but it isn't quite as desirable as from Ready state.

```

		 Suspend Ready -----------------------------|
		/	          \                             |
New - Ready --------- Running -----Terminat.        |
        \              /                            |
         -----Block---                              |
               |                                    |
	          / \                                   |
	         Suspend                                |
	        Block -----------------------------------
```

To go from Running to ready, preempt
I/O or system call to go from block or system call
A process is moved from blocked to suspended block due to resource management and multitasking needs. 
Blocked, means the process is waiting for I/O. Sometimes the OS needs to free up resources, or need to pause the process like user switches to a different application. 
In suspended block, it is also out of main memory. 
Suspend Block to Suspend Ready: When it goes from suspended block to suspended ready, it is ready in a logical sense but its not in main memory. It needs to be brought from disk to main memory. Suspended Ready is a way for OS to acknowledge that the process is ready to run but it isn't in main memory. 

## Scheduling Queues  & State Queueing Diagram
Queue follows FIFO discipline

Ready Queue contains the list of PCBs of Ready Processes. 

Device Queues: also known as blocked queue, contains the list of blocked processes. Holds processes to manage I/O requests.

Ondisk Queues: job Queues (contain programs ready to be loaded in memory), Suspend Queue 
(list of process that gets suspended from memoy onto disk)

Timestamp: 2:35:16

# 06/09
Job Queue: Contains list of PCBs ready to loaded
Scheduler chooses which in the ready queue gets to CPU

Schedulers and Dispatchers:
Scheduling: choosing the process to run in job queue, ready queue, suspend queue
Scheduler is the component of Operating System that makes the decision
Long Term Scheduler: new-> ready, controls how many programs get loaded from disk to main memory
Short Term Scheduler: Ready -> running 
Medium term scheduler: process suspension & resuming

Dispatcher: Responsible for carrying out the activity of context switching
context switching: activity of loading and saving the process during a process switch on CPU
Dispatch is not involved in the suspension of process - only the Medium term scheduler is there. Dispatcher is involved with short term scheduler

## CPU Scheduling
Process Switch Time > user to kernel mode switch time
Process switching time involves the user to kernel mode switch

Arrival Time: The time at which the process enters the ready queue for the first time
Waiting Time: The time spent by the process waiting in the ready queue
Burst TIme: The time spent by the process running on CPU
I/O Burst Time: Time spent by the process waiting for IO operation in blocked state (IO operation not done by process, the operating system does)
Completion Time: When process terminates
Turn Around TIme: Time spent by the process from new -> terminate. The lifetime of the process.
	TAT = Completion Time - Arrival Time
	Waiting Time = TAT - (BT + IOBT)
Schedule Length: Total time taken to complete all n processes as per schedule

Schedule:
The order in which processes are run

How many schedules are possible with n process: n!  in non-preemptive - In preemeptive = ∞

schedule length = completion time of last process - Arrival time of first process

Throughput = no of process completed per unit time


Algorithms:
1. First Come First Serve (FCFS)
	1. Selection Criteria: Arrival Time
	2. Mode of Operation: non preemptive
	3. Conflict Resolution: best one is choose the lower process ID if two processes come in at the same time
	4. Assumption:
		1. Time is in clock ticks
		2. No IOBTs
		3. Scheduling overhead = 0

TAT = CT - AT
WT = TAT - (BT + IOBT)


| PID | Arrival Time | Burst tIme | Completion Time | Turn Around Time | Waiting Time |
| --- | ------------ | ---------- | --------------- | ---------------- | ------------ |
| 1   | 0            | 4          | 4               | 4                | 0            |
| 2   | 0            | 3          | 7               | 7                | 4            |
| 3   | 0            | 5          | 12              | 12               | 7            |
```
  _________________________________________________________
    P1            | P2        | P3                        |     
  _________________________________________________________
  0   1   2   3   4   5   6   7    8    9    10    11    12
```

TImestamp: 3:17:01

# 06/11

Second example:

| PID | Arrival Time | Burst Time | Completion Time | Turn Around Time | Waiting Time |
| --- | ------------ | ---------- | --------------- | ---------------- | ------------ |
| 1   | 0            | 2          | 2               | 2                | 0            |
| 2   | 0            | 1          | 3               | 3                | 1            |
| 3   | 2            | 3          | 6               | 4                | 1            |
| 4   | 3            | 2          | 8               | 5                | 3            |
| 5   | 5            | 4          | 12              | 7                | 3            |

Third Example:

| PID | Arrival Time | Burst Time | Completion Time | Turn Around Time | Waiting Time |
| --- | ------------ | ---------- | --------------- | ---------------- | ------------ |
| 1   | 3            | 5          | 8               | 5                | 0            |
| 2   | 10           | 2          | 12              | 2                | 0            |
| 3   | 15           | 4          | 19              | 4                | 0            |
| 4   | 18           | 5          | 24              | 6                | 1            |

Fourth Example:

| PID | Arrival Time | Burst Time | Completion Time | Turn Around Time | Waiting Time |
| --- | ------------ | ---------- | --------------- | ---------------- | ------------ |
| 1   | 5            | 2          | 7               | 2                | 0            |
| 2   | 3            | 1          | 4               | 1                | 0            |
| 3   | 8            | 4          | 12              | 4                | 0            |

Example with IO Burst Time

WT = TAT - (BT + IOBT + NS)
NS = # of times a process get scheduled on CPU

| PID | Arrival Time | Burst Time | IO Burst Time | Burst Time after IO | Completion Time | Turn Around Time | Waiting Time |
| --- | ------------ | ---------- | ------------- | ------------------- | --------------- | ---------------- | ------------ |
| 1   | 0            | 3          | 7             | 2                   | 15              | 15               | 1            |
| 2   | 2            | 5          | 2             | 3                   | 19              | 17               | 5            |
| 3   | 5            | 1          | 4             | 2                   | 22              | 17               | 8            |
P1 arives at T0, runs at T1, runs for 3, goes to IO at T4 for 7, and goes back to ready queue at T11
P2 arrives at T2, starts running at T5, runs from 5->10, goes to IO queue for 2 units at T10, goes back to reay queue at T12
At T11, CPU is empty so P3 starts running from T11->T12, goes to IO Queue for 4 units, goes back to ready queue at T16
At T13, P1 is running again and completes at T15.
At T16, P2 runs for 3 units, completes at 19
At T20, P3 runs again for 2 units, completes at T22


SJF - Shortest Job First - Based on Burst Time
Selection Criteria: Shortest Burst Time
Mode of Operation: Non-preemptive
Conflict Resolution: Lower PID (P1 > P2 Preference)

Among the process present in BQ, we select the process with least burst time & schedule it on CP


| Process No | Arrival Time | Burst Time |
| ---------- | ------------ | ---------- |
| 1          | 0            | 2          |
| 2          | 2            | 3          |
| 3          | 3            | 1          |
| 4          | 5            | 2          |
| 5          | 8            | 3          |
| 6          | 10           | 2          |
Burst time is only compared between the processes in ready queue


Shortest Remaining Time First / Preemptive SJF
Selection Criteria: Burst Time
Mode of Operation: Preemptive
Tie Breaking: Preemption of running process is based on the availability of strictly shorter process
