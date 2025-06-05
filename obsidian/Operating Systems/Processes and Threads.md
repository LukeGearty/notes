## The Process Model
Four process: A, B, C, and D
One Program Counter, it sequentially executes A, then B, then C, then D
Four Program Counters for each process
Only one program is active at once, which program gets activity?

## Processes
A process is an application in memory - a container - all data on stack and heap, code, file descriptors
Four ways to create a process:
1. System initialization - processes scheduled to start with OS
2. Execution a process creation system call by a running process
3. A user requests to create a new process - types a command, clicks on a program
4. Initiation of a batch job

Termination
1. Normal exit (voluntary) - exit(1): passes 1 back to OS
2. Error exit (voluntary) - divide by 0?
3. Fatal error (involuntary) - maybe stack is corrupted
4. Killed by another process (involuntary) - typing the kill command

Three States a Process May be in:
1. Running - the process is using the CPU
2. Ready - runnable, but it is stopped to let another process run
3. Blocked - unable to run until some external event happens - waiting for some I/O device

Running -> Blocked: Waiting for input, data from somewhere that requires a little bit of time - like from main memory, I/O
Blocked -> Ready: gets the data it needs
Ready -> Running: The CPU chooses to run a process

Implementation of Processes
Some of the fields of a typical process table entry:
1. Process Management: Registers, program counter (next instruction), program status word (contains a bunch of info about process), stack pointer, process state, priority, scheduling parameters, process ID, parent process, process group, signals, time when process started, CPU time used, children's CPU time, time of next alarm
2. Memory management: pointer to text segment info, pointer to data segment info, pointer to stack segment info
3. File management: root directory, working directory, file descriptors, user ID, group ID

As we swap out processes, we need interrupts
Skeeleton of the lowest level of what the OS does when an interrupt occurs
1. Hardware stacks program counter, etc
2. Hardware loads new program counter from interrupt vector
3. Assembly-language procedure saves registers
4. Assembly-language procedure sets up new stack
5. C interrupt service runs
6. Scheduler decides which process to run next
7. C Procedure returns to the assembly code
8. Assembly-language procedure starts up new current process.

CPU utilization as a function of the number of processes in memory

## Threads
Think of a word processor:
1. One thread can wait for the keyboard input
2. One thread can be spellchecking
3. One thread can be saving to the disk after some amount of time

Web Server:
1. One thread is waiting for incoming HTTP requests (Dispatcher thread)
2. That dispatcher thread creates a worker thread when it gets a request

Three ways to construct a server
Threads - parallelism, blocking system calls
Single-threaded process - no parallelism, blocking system calls
Finite State Machines 

Classical Thread Model:
Per-process Items: address space, global variables, open files, child processes, pending alarms, signals and signal handlers, accounting info
Per-Thread Item: program counter, registers, stack, state

# Interprocess Communication

Solves the problem of sharing some shared resource

## Race Conditions
Two processes want to access shared memory at the same time - okay if both are reading, not okay if they are both writing

Requirements to avoid race conditions:
1. No two processes may be simultaneously inside their critical regions
2. No assumptions may be made about speeds or the number of CPUs
3. No process running outside its critical region may block other processes

Process A enters critical region at t = 1, Process B tries to enter at t = 2, is blocked at the entrance

Mutual Exclusion 