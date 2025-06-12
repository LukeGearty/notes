 Importance of Processes
- One of the **oldest and most vital abstractions** in OS design.
- Enables **(pseudo) concurrent execution**, even on a single CPU.
- Converts a **single CPU** into multiple **virtual CPUs**.
- Modern computing depends on this abstraction.

# Processes

## Real-World Concurrency

- **Modern computers** perform multiple tasks simultaneously.
- **Examples**:
  - **Web server**: Handles multiple page requests, often while waiting for slow disk I/O.
  - **User PC**:
    - Background processes (e.g., email checker, antivirus).
    - User processes (e.g., printing, USB backups, web browsing).

## Managing Concurrency

- Need for a mechanism to **model and control concurrent tasks**.
- **Processes and threads** provide the abstraction needed.

## Multiprogramming Systems

- **CPU rapidly switches** between processes (every few ms).
- Creates an **illusion of parallelism**, known as **pseudoparallelism**.
  - Differs from **true parallelism** in multiprocessor systems (multiple CPUs).

## Conceptual Model

- OS designers created the **sequential process model**.
- Helps manage and understand parallelism.
- This model and its implications are the focus of the chapter.

## 2.1.1 The Process Model

### Definition

- A **process** is an instance of an **executing program**.
  - Includes: program counter, registers, variables.
- Conceptually: each process has its **own virtual CPU**.
  - In reality: the **real CPU switches** between processes (multiprogramming).
#### CPU Assumptions

- Assumes **one CPU** for simplicity.
  - Real systems may have **multicore processors** (covered in Chapter 8).
  - Each core can still only run **one process at a time**.

#### Timing & Non-Determinism

- CPU switching causes **variable performance** for each process.
- **Processes must not assume specific timing**.
  - Example: An audio process using a delay loop can **break synchronization** if preempted.
  - **Real-time processes** require **special guarantees** for timing.

### Process vs Program

- **Program**: Static code, may be stored on disk.
- **Process**: Dynamic activity of executing the program.
  - Has: program, input, output, and state.
  - Managed by a **scheduler** deciding when to run/suspend.

#### Analogy: Baking a Cake

- **Program**: Recipe
- **CPU**: Baker
- **Input**: Ingredients
- **Process**: Act of baking
- **Context switch**: Baker pauses cake to treat son’s bee sting (switches tasks)

#### Multiple Processes, Same Program

- A program running **twice = two distinct processes**.
  - E.g., printing two files at once or opening two word processor windows.
- OS may share code in memory, but processes are **independent**.
## 2.1.2 Process Creation

#### Why Process Creation is Needed

- Simple systems may start with all needed processes already created.
- **General-purpose systems** must **create/terminate processes dynamically** during operation.

#### Four Main Events Triggering Process Creation

1. **System initialization**
2. **System call** by a running process (e.g. `fork`)
3. **User request** to start a new program (e.g. clicking an icon)
4. **Batch job initiation** (e.g. in mainframe systems)

---

#### Foreground vs Background Processes

- **Foreground**: Interact with users.
- **Background (Daemons)**: Run silently to handle tasks like:
  - Email reception
  - Web requests
  - Printing, etc.
- In UNIX: use `ps` to list processes  
- In Windows: use **Task Manager**

---

#### Creating New Processes at Runtime

- Often done via **system calls** by running processes.
- Useful for parallel tasks, e.g.:
  - One process fetches data
  - Another processes it
- Especially beneficial on **multiprocessor systems**

---

#### User-Initiated Process Creation

- Triggered by:
  - Typing a command
  - Clicking an icon
- New process runs the selected program
- In GUI systems:
  - UNIX: process takes over a window
  - Windows: process creates a window (usually)

---

#### Batch Job Initiation (Mainframes)

- Batch jobs (e.g. store inventory processing) are queued.
- When ready, OS creates a process to run the next job.

---

#### Technical Mechanism: System Calls

- Always created via **system calls** from an existing process.

---

#### Process Creation in UNIX

- `fork()`: creates a **clone** of the calling process.
  - Same memory image, env, open files.
- `execve()`: replaces child’s memory with a **new program**.
  - Common pattern: `fork()` → manipulate → `execve()`
  - Enables redirection of input/output.

---

#### Process Creation in Windows

- `CreateProcess()`:
  - Combines creation and program loading
  - Takes **10 parameters** (program, args, security, window info, etc.)
- Win32 also includes ~100 functions for process control/synchronization

---

### Address Spaces

- **UNIX**:
  - Parent & child get **separate address spaces**
  - Child's address space is initially a **copy** of the parent's
  - May use **copy-on-write** optimization
- **Windows**:
  - Parent & child always have **separate address spaces**
- Shared resources like **open files** may still be inherited.

## 2.1.3 Process Termination

After a process is created and runs its course, it must eventually terminate. This can happen due to various reasons, both **voluntary** and **involuntary**.

---

### Reasons for Process Termination

#### 1. **Normal Exit (Voluntary)**
- The process completes its intended task.
- Calls system function to terminate:
  - UNIX: `exit`
  - Windows: `ExitProcess`
- Example: Compiler finishes compiling code.

#### 2. **Error Exit (Voluntary)**
- The process detects a problem (e.g., missing file).
- Reports the error and exits cleanly.
- Example: User types `cc foo.c` but `foo.c` doesn’t exist.

#### 3. **Fatal Error (Involuntary)**
- The process encounters a **critical runtime error**, often due to bugs.
- Examples:
  - Illegal instruction
  - Memory access violation
  - Division by zero
- In UNIX-like systems:
  - The process can register signal handlers to **intercept and manage** some of these errors.

#### 4. **Killed by Another Process (Involuntary)**
- One process **explicitly kills** another using system calls:
  - UNIX: `kill`
  - Windows: `TerminateProcess`
- Requires appropriate **permissions**.
- In some systems, killing a parent also kills all its children, but:
  - **UNIX and Windows do not do this automatically**

---

### Additional Notes

- **Interactive programs** (e.g., browsers, word processors) usually prompt users before exiting.
- **Child processes** are *not* auto-terminated with the parent in major OSes like UNIX or Windows.
## 2.1.4 Process Hierarchies

When a process creates another, some operating systems maintain a **parent-child relationship**, forming a **process hierarchy**. A process can have:
- **One parent**
- **Zero or more children**

This structure is similar to a **hydra** (many heads from one body), not like animals with two parents.

---

#### UNIX: True Process Hierarchy

- Processes and their descendants form a **process group**.
- Signals sent from the keyboard are delivered to **all processes in the group**:
  - Each process can:
    - Catch the signal
    - Ignore the signal
    - Take the default action (usually termination)

#### Example: UNIX Boot Process

1. **`init` process** (present in boot image) starts running.
2. Reads a configuration file to determine how many terminals exist.
3. **Forks a new process per terminal** (waiting for login).
4. On login:
   - A **shell process** is started.
   - User commands spawn further child processes.

📌 All processes form a **single process tree**, with `init` at the **root**.

---

### Windows: No Formal Hierarchy

- Processes are **equal** (no enforced parent-child tree).
- When a child process is created:
  - The parent gets a **handle** (token) to control the child.
  - The handle can be **passed to other processes**, effectively breaking the hierarchy.

📌 In **Windows**, the hierarchy is **not preserved** or enforced.

---

## Key Differences

| Feature                | UNIX                             | Windows                          |
|------------------------|----------------------------------|----------------------------------|
| Process Tree           | Yes (true hierarchy)             | No (flat structure)              |
| Signals to Groups      | Yes (via process group)          | No                               |
| Parent Control         | Limited, structured              | Via handle (token), transferable |
| Disinheriting Children | Not allowed                      | Possible by passing handle       |
## 2.1.5 Process States

Processes often need to interact, and their execution can be interrupted or paused due to logical dependencies or system scheduling.

---

### Key Process States

A process can be in **one of three states**:

| State    | Description                                               |
|----------|-----------------------------------------------------------|
| Running  | Actively using the CPU at that moment                     |
| Ready    | Able to run but waiting for CPU time                      |
| Blocked  | Waiting for an external event (e.g. input) to proceed     |

- **Running** and **Ready** are conceptually similar: the process wants to run.
- **Blocked** is fundamentally different: the process *cannot* run, even if the CPU is free.

---

#### Example: Process Interaction

```bash
cat chapter1 chapter2 chapter3 | grep tree
```

### State Transitions

Processes move between states through **four main transitions**:

|Transition|From → To|Trigger|
|---|---|---|
|1|Running → Blocked|Process can't proceed (e.g., waiting for input)|
|2|Running → Ready|Scheduler preempts the process (time slice over)|
|3|Ready → Running|Scheduler selects it to run|
|4|Blocked → Ready|External event completes (e.g., input arrives)|

- **Transition 1**: Done via system calls like `pause`, or automatically (e.g., reading from empty pipe in UNIX).
    
- **Transitions 2 & 3**: Handled entirely by the **scheduler**.
    
- **Transition 4**: Triggered by an **external event**.
    

---

#### Scheduler's Role

- The **scheduler** decides:
    
    - Which process runs
        
    - For how long
        
- Goal: Balance system **efficiency** and **fairness**.
    

---

#### Process View vs Interrupt View

Rather than thinking in terms of low-level interrupts:

- Think in **process terms**:
    
    - **Disk process** waiting for I/O
        
    - **Terminal process** waiting for keystrokes
        
- When an event occurs (e.g., disk read completes), the **blocked process is unblocked** and becomes ready to run.
    

---
#### Layered Model of OS
- **Scheduler** is at the bottom.
- **All system services** (disk, terminal, file server, etc.) are processes above it.
- Each service waits (blocked) and resumes when needed.

## 2.1.6 Implementation of Processes

#### 🧱 Process Table (Process Control Blocks / PCBs)

- The operating system keeps a **process table**, a key data structure to manage all active processes.
- Each entry (often called a **Process Control Block**) contains:
  - Program Counter
  - Stack Pointer
  - Memory allocation details
  - Open file statuses
  - Accounting info
  - Scheduling info
  - And more...

📌 **Purpose**: Store everything needed to save and later restore a process's state during context switches.

---

#### Fields in the Process Table

Grouped into three broad categories:

| Category              | Example Fields                            |
|-----------------------|--------------------------------------------|
| **Process Management**| Program counter, process state, registers |
| **Memory Management** | Memory map, segment tables                |
| **File Management**   | Open file descriptors, permissions        |

📝 **Note**: Specific fields vary by operating system.

---

#### ⚙️ How OS Maintains the Illusion of Concurrent Processes

##### 🔁 The Role of Interrupts

- Each I/O device has an **interrupt vector**:
  - Fixed memory location storing the address of its **Interrupt Service Routine (ISR)**.

##### Example: Disk Interrupt

1. **User process 3** is running.
2. A **disk interrupt** occurs.
3. The **hardware** automatically:
   - Pushes:
     - Program Counter (PC)
     - Program Status Word (PSW)
     - (Sometimes) Registers
   - Jumps to the ISR using the interrupt vector.

---

#### What Happens Next (Handled by Software)

1. **Registers are saved**, typically into the process table entry.
2. **Stack pointer is changed** to a temporary kernel-mode stack.
3. **Low-level assembly routine** performs the initial save:
   - Required because some operations (like setting stack pointer) can't be done in C.

4. The assembly routine then calls a **C function** to:
   - Handle the specific interrupt type.
   - Possibly mark some process as ready.

5. The **scheduler** is called to decide which process runs next.
6. Control goes back to the assembly code to:
   - Load registers and memory map of the chosen process.
   - Resume execution.

---

#### Key Insight

> A process may be interrupted **thousands of times**, but the OS ensures that each time it resumes execution **as if nothing had happened**.

This is the foundation of **context switching** and multitasking in modern systems.

---

#### Summary Table

| Component                  | Function                                                       |
|---------------------------|----------------------------------------------------------------|
| **Process Table / PCB**    | Stores process state for saving and restoring                 |
| **Interrupt Vector**       | Maps interrupt types to handler addresses                     |
| **Assembly ISR Routine**   | Saves registers and prepares for high-level handler           |
| **C Interrupt Handler**    | Handles specific I/O or other interrupt logic                 |
| **Scheduler**              | Picks the next process to run                                 |
| **Context Switch Logic**   | Restores state of selected process and resumes it             |

# Threads
In traditional operating systems, a **process** is defined by its:
- **Address space:** A dedicated memory region.
- **Single thread of control:** A single execution path.
However, many situations benefit from having **multiple threads of control** within the **same address space**. These threads run in "quasi-parallel," acting almost like separate processes, but with the key distinction of **sharing the same memory space**.

## Thread Usage
Threads offer several advantages over traditional processes, especially in applications with multiple concurrent activities.

#### Simplified Programming Model
- Many applications involve multiple simultaneous activities, some of which may **block** (e.g., waiting for I/O).
- By breaking down an application into multiple **sequential threads** that run in **quasi-parallel**, the programming becomes simpler.
- This is similar to the argument for processes, where we think of parallel processes instead of complex interrupt handling.
- The key difference with threads is their ability to **share an address space and all its data**, which is crucial for certain applications where separate address spaces (like in processes) wouldn't work.

---

#### Efficiency and Performance

- **Lighter weight:** Threads are significantly **faster to create and destroy** than processes (10-100 times faster in many systems). This is beneficial when the number of concurrent tasks changes rapidly.
- **Overlapping activities:** While threads offer no performance gain for purely CPU-bound tasks, they greatly improve performance when there's a mix of **computing and I/O**. Threads allow these activities to **overlap**, speeding up the application.
- **Leveraging multiple CPUs:** On systems with **multiple CPUs**, threads enable true **parallelism**, allowing different threads to execute simultaneously on different cores.

---

#### Concrete Example: Word Processor

Consider a word processor that displays a document exactly as it will appear when printed.

- **The Problem:** If a user deletes a sentence from page 1 of an 800-page book, the word processor must reformat the entire book up to a requested page (e.g., page 600) before it can display it. This leads to substantial delays and a frustrated user.
- **Threads to the Rescue:**
    - **Two-threaded program:**
        - **Interactive thread:** Handles user input (keyboard, mouse) and responds to simple commands (like scrolling the current page).
        - **Reformatting thread:** Works in the background to reformat the entire document.
        - When a change is made, the interactive thread signals the reformatting thread. Meanwhile, the interactive thread remains responsive. Ideally, reformatting finishes before the user requests a distant page, making the display instantaneous.
    - **Adding a third thread:** Many word processors automatically save files to disk periodically. A **third thread** can manage these **disk backups** without interrupting the interactive or reformatting threads.

## The Classical Thread Model

### Concepts Behind Threads

- The **process model** separates:
  - **Resource grouping**
  - **Execution**
- Threads allow these to be **treated independently**.

---

### Process vs Thread

#### A **Process**:
- Groups related resources:
  - Address space (program text + data)
  - Open files
  - Child processes
  - Pending alarms
  - Signal handlers
  - Accounting info

#### A **Thread**:
- Represents an **execution flow**:
  - Program counter
  - Registers (working variables)
  - Stack (execution history)

> Threads run **within** a process, but they are **separate entities**.

---

### Multithreading

- **Multiple threads** can exist in **one process**, sharing:
  - Address space
  - Global variables
  - Open files
  - Child processes
  - Signals

- Analogous to:
  - Threads → multiple threads in one process
  - Processes → multiple processes in one system

#### Advantages
- Efficient use of shared resources
- Lightweight (sometimes called "lightweight processes")
- Fast context switching (especially on multithreading-supported CPUs)

---

### Visual Representations (Described)

- **Fig. 2-11(a)**: Three separate processes with individual address spaces and single threads.
- **Fig. 2-11(b)**: One process with three threads, sharing a single address space.

---

### CPU and Thread Execution

- On single-CPU systems:
  - Threads take **turns** (like process scheduling)
  - Illusion of **parallel execution**
  - Each gets a fraction of CPU time (e.g., 3 threads → ⅓ CPU each)

---

### Shared Address Space: Dangers & Implications

- Threads can:
  - Access **all memory** in the process
  - **Interfere** with each other's stacks and variables
- No memory protection:
  - Assumes threads are **cooperative** (same user/process)
- Use multithreading when threads are **closely related** and need to **collaborate**.

---

### Resource Sharing

- Only **processes** own resources, not threads.
- If each thread had its own:
  - Address space
  - Open files
  - Alarms, etc.
  - → It would be a **process**, not a thread.

---

### Thread Lifecycle

#### States:
- **Running**: Currently on the CPU
- **Blocked**: Waiting for an event
- **Ready**: Scheduled and waiting
- **Terminated**: Done executing

Transitions are similar to process state transitions.

---

### Stack Per Thread

- Each thread has its **own stack**:
  - One frame per active procedure
  - Keeps local variables + return addresses
- Required due to **different execution paths**.

---

### Thread Management

- A process starts with **one thread**
- **Creating a thread**:
  - `thread_create(procName)`
  - New thread runs in the same address space

- **Exiting a thread**:
  - `thread_exit()`

- **Waiting for a thread**:
  - `thread_join(threadID)`

- **Yielding CPU voluntarily**:
  - `thread_yield()`

---

### Complications of Threads
#### 1. Forking with Threads:
- Should child get parent’s threads?
  - If not: May break behavior
  - If yes: Confusion over blocked threads and shared I/O
#### 2. Data Races:
- Shared structures may cause:
  - File closed by one thread while another reads
  - Memory allocation conflicts if threads act simultaneously

Careful design is needed to avoid errors in multithreaded programs.

## 2.2.3 POSIX Threads

To enable portable threaded programs, IEEE defined a thread standard in **IEEE 1003.1c**, called **Pthreads**. Most UNIX systems support it.

The standard defines over 60 function calls. Here are some major ones to give an idea of how it works (see Fig. 2-14):

| Thread Call           | Description                           |
|----------------------|-------------------------------------|
| `pthread_create`      | Create a new thread                  |
| `pthread_exit`        | Terminate the calling thread        |
| `pthread_join`        | Wait for a specific thread to exit  |
| `pthread_yield`       | Release CPU to let another thread run |
| `pthread_attr_init`   | Create and initialize a thread’s attribute structure |
| `pthread_attr_destroy`| Remove a thread’s attribute structure|

---

#### Pthreads Properties

- Each thread has:
  - An **identifier**
  - A set of **registers** (including the program counter)
  - A set of **attributes** stored in a structure (e.g., stack size, scheduling parameters)

---

#### Key Thread Calls

- `pthread_create`  
  Creates a new thread and returns its thread identifier (like a PID). Parameters specify the thread’s start routine.

- `pthread_exit`  
  Terminates the calling thread and releases its stack.

- `pthread_join`  
  The calling thread waits for a specific other thread (given by thread id) to exit.

- `pthread_yield`  
  Allows a thread to voluntarily give up the CPU to let another thread run. Useful for cooperative multitasking among threads.

- `pthread_attr_init`  
  Creates and initializes a thread attribute structure with default values (like priority).

- `pthread_attr_destroy`  
  Removes and frees the attribute structure memory but does not affect running threads using it.

---

```c
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

#define NUMBER_OF_THREADS 10

void* print_hello_world(void* tid) {
    /* This function prints the thread’s identifier and then exits. */
    printf("Hello World. Greetings from thread %d\n", (int)(size_t)tid);
    pthread_exit(NULL);
}

int main(int argc, char *argv[]) {
    /* The main program creates 10 threads and then exits. */
    pthread_t threads[NUMBER_OF_THREADS];
    int status, i;

    for(i = 0; i < NUMBER_OF_THREADS; i++) {
        printf("Main here. Creating thread %d\n", i);
        status = pthread_create(&threads[i], NULL, print_hello_world, (void*)(size_t)i);
        if (status != 0) {
            printf("Oops. pthread_create returned error code %d\n", status);
            pthread_exit(-1);
        }
    }
    exit(EXIT_SUCCESS);
}
```

## Threads in User Space

There are two main places to implement threads:
- **User space**
- **Kernel space**

### Structure

- Threads are managed entirely in **user space**.
- Implemented using a **runtime library**.
- Operating systems that don’t support threads can still run them this way.

#### Example Library Functions

- `pthread_create`
- `pthread_exit`
- `pthread_join`
- `pthread_yield`

#### Thread Table

- Each process maintains its **own thread table**:
  - Similar to the kernel’s process table.
  - Stores:
    - Program counter
    - Stack pointer
    - Registers
    - Thread state

#### How Thread Switching Works

1. A thread blocks or yields.
2. The **runtime system**:
   - Saves the current thread’s state.
   - Finds a ready thread.
   - Loads that thread’s state.
3. Fast switching (possibly just a few CPU instructions).
4. **No kernel trap** required.

#### Advantages

- Can be used on **any OS**, even those without native thread support.
- **Fast context switching**:
  - No system calls
  - No context switching overhead
  - No cache flushing
- **Custom scheduling** per process.
- **Better scalability**:
  - No kernel resource usage (e.g., stacks or process table entries).

### Problems

#### 1. Blocking System Calls

- Blocking a thread (e.g., reading from keyboard) blocks the **entire process**.

##### Workarounds:

- **Nonblocking I/O**: Requires OS changes.
- **`select` + wrapper ("jacket") functions**:
  - Check if I/O will block before calling.
  - Inefficient and requires library modification.
#### 2. Page Faults

- Page Fault: A program calls or jumps to an instruction that is not in memory - the OS goes to get the missing info from disk 
- If a thread triggers a **page fault**, the **whole process** blocks.
- Kernel doesn’t know which thread caused it.
#### 3. Cooperative Scheduling

- No **clock interrupts** at user level.
- Threads run until they yield:
  - No preemption
  - Risk of one thread monopolizing the CPU

##### Workaround:

- Use **periodic timers**, but:
  - Crude
  - Hard to program
  - May interfere with application logic

#### 4. Frequent Blocking Applications

- Applications that block often (e.g., web servers):
  - Make frequent system calls.
  - Kernel is better suited to handle thread switching.

## 2.2.5 Implementing Threads in the Kernel

When threads are implemented in the **kernel**, the kernel is aware of and manages all threads directly.

- No **runtime system** is needed in each process.
- There is **no per-process thread table**.
- The **kernel maintains a global thread table** that tracks:
  - Thread registers
  - Thread state
  - Other thread-specific data

This is similar to the user-level thread table but managed **inside the kernel**. Additionally, the kernel retains the **traditional process table** to track processes.

- Thread creation/destruction is done via **kernel calls**.
- The **kernel thread table** is updated accordingly.
- All thread-related operations are **more costly** than user-space procedures because they involve **system calls**.

- All blocking operations are handled via **system calls**.
- When a thread blocks, the kernel may:
  - Run another **thread from the same process**, or
  - Run a **thread from a different process**
- This contrasts with **user-level threads**, where the runtime system can only run threads from the **same process**.

- Due to the **high cost** of creating/destroying threads in the kernel:
  - Some systems **recycle threads**:
    - Mark a thread as "not runnable" when destroyed.
    - Reactivate it later when needed.
- **User-level threads** can also be recycled, but the benefit is less due to **lower overhead**.

#### Advantages

- No need for **nonblocking system calls**.
- **Page fault handling** is more efficient:
  - If a thread causes a page fault, the kernel can run another thread from the **same process**.
- Can run threads across processes more flexibly.

#### Disadvantages

- **System calls are expensive**, increasing the overhead of:
  - Thread creation
  - Thread destruction
  - Context switching

#### Open Issues

##### 1. Forking a Multithreaded Process

- When a multithreaded process calls `fork()`, questions arise:
  - Should the child process have **all threads** of the parent?
  - Or just **one thread**?

**Depends on next action:**
- If calling `exec()` soon after, one thread is preferred.
- If continuing execution, duplicating all threads may be better.

##### 2. Signals and Threads

- Signals are traditionally sent to **processes**, not threads.
- Problems:
  - Which thread should handle the signal?
  - Possible solution: threads register interest in specific signals.
    - But what if **multiple threads** register for the same signal?

## 2.2.6 Hybrid Implementations

- Hybrid threading combines **user-level** and **kernel-level** threads.
- Goal: Get the **flexibility and efficiency** of user-level threads with the **robustness** of kernel-level threads.

- Use **kernel threads** as a base.
- **Multiplex** user-level threads onto one or more kernel threads.


- Programmers can decide:
  - How many **kernel threads** to create.
  - How many **user threads** to assign to each kernel thread.

- The **kernel**:
  - Only knows about and schedules **kernel threads**.
  - Has no visibility into the user threads.

- The **user-level threads**:
  - Are managed like regular user threads.
  - Share time on their assigned kernel thread.

- **Flexible** configuration.
- **Efficient** user-level thread switching.
- **Kernel support** for blocking and I/O.


# Interprocess Communication
Processes often need to communicate with one another. For example, in a shell pipeline, the output of one process is passed to the next. This communication should be structured and not rely on interrupts.

Three Key Issues in IPC
1. **Information Transfer**
   - How one process can send data to another.
   - Essential for structured inter-process workflows.
1. **Mutual Exclusion**
   - Prevents processes from interfering with each other.
   - Example: Two airline reservation processes trying to book the last seat simultaneously.
1. **Proper Sequencing**
   - Ensures correct order of operations when processes are dependent.
   - Example: Process B should wait for Process A to generate data before printing.

IPC vs. Thread Communication
- **Information Sharing:**
  - Easier with threads due to shared memory space.
  - If threads are in different address spaces, the problem becomes one of process communication.
- **Mutual Exclusion & Sequencing:**
  - These challenges apply equally to both threads and processes.
  - The same synchronization solutions (e.g., locks, semaphores) are used for both.

## 2.3.1 Race Conditions
A **race condition** occurs when two or more processes access shared data, and the **final result depends on the exact timing** of their execution.

- Race conditions are difficult to debug.
- Tests usually pass, but **rare failures** may occur unpredictably.
- **Increased parallelism** (more CPU cores) is making race conditions more common

### Shared Memory in IPC

- In some OSes, cooperating processes may share common storage (e.g., main memory or a shared file).
- The **location** of the shared memory doesn't change the nature of the **communication** or the **problems**.

### Example: Print Spooler

- When a process wants to print, it writes the file name to a **spooler directory**.
- A **printer daemon** checks the directory periodically, prints queued files, and removes them afterward.

#### Spooler Structure:
- Slots numbered `0, 1, 2, ...` hold file names.
- Two shared variables:
  - `in`: points to the next free slot.
  - `out`: points to the next file to print.
- These may be stored in a shared two-word file.

### The Race Condition Scenario

1. **Initial State**:  
   - Slots `0–3` are empty (already printed).  
   - Slots `4–6` are full (waiting to be printed).  
   - `in = 7`.

2. **Simultaneous Access**:
   - Processes **A** and **B** both try to add files at the same time.
   - Both read `in = 7` and store it in a local variable `next_free_slot`.

3. **Interleaving Execution**:
   - **Clock interrupt** occurs. Process A is paused.
   - **Process B**:
     - Writes its file to **slot 7**.
     - Updates `in = 8`.
   - **Process A resumes**:
     - Uses its old `next_free_slot = 7`.
     - Overwrites **slot 7** with its own file name.
     - Updates `in = 8`.

4. **Final Outcome**:
   - The spooler appears consistent.
   - Process B’s file is lost.
   - **User B** waits forever for a printout that will never come.
## 2.3.2 Critical Regions

- **Problem**: Race conditions occur when multiple processes read/write shared data **simultaneously**.
- **Solution**: Enforce **mutual exclusion** — ensure only one process accesses shared data at a time.

> Mutual exclusion prevents interference by allowing only one process to be in a **critical region** at a time.

- A **critical region (or critical section)** is the part of a program where a process accesses **shared memory** or **resources**.
- Outside the critical region, processes perform internal computations or operations that **don’t affect** shared data.

If **no two processes** are in their critical regions **simultaneously**, **race conditions can be avoided**.

---

### Four Conditions for a Correct Mutual Exclusion Solution

1. No two processes may be in their critical regions at the same time.
2. No assumptions can be made about **CPU speed** or the **number of CPUs**.
3. A process not in its critical region **should not block others** from entering theirs.
4. No process should have to **wait forever** to enter its critical region (i.e., no starvation).

---

### Conceptual Timeline of Critical Region Access
- **T1**: Process A enters its critical region.
- **T2**: Process B tries to enter but is **blocked** (A is inside).
- **T3**: A exits its critical region; B **enters immediately**.
- **T4**: B exits; **no process** is now in the critical region.

This sequence ensures mutual exclusion is preserved.

## 2.3.3 Mutual Exclusion with Busy Waiting

This section explores techniques to achieve mutual exclusion so that only one process at a time can enter its critical region and avoid race conditions.

---

Disabling Interrupts

On a single-processor system, one solution is to disable interrupts after entering the critical region and re-enable them before leaving. Since context switches occur due to interrupts, this prevents any process switch during that period.

However, this method is not suitable for general use:

- It is unsafe to allow user processes to disable interrupts; if they fail to re-enable them, the system may freeze.
- In multiprocessor systems, disabling interrupts on one CPU does not prevent others from accessing shared memory.
- Modern systems often have multiple cores, making this approach ineffective.

Disabling interrupts can still be useful in the operating system kernel for short, critical sections, but is not a viable mutual exclusion mechanism for user-level code.

---

Lock Variables

A software-based attempt at mutual exclusion uses a shared lock variable. The variable is initially set to 0. When a process wants to enter its critical region, it checks the lock:

- If the lock is 0, it sets it to 1 and proceeds.
- If the lock is 1, it waits until it becomes 0.

This method fails due to race conditions. Two processes could simultaneously see the lock as 0 and both set it to 1 before either enters the critical region, resulting in both entering at once.

Even adding a second check does not solve the problem, as the race can still occur between the checks and updates.

---

Strict Alternation

This method uses a shared integer variable `turn` to alternate access to the critical region. If `turn` is 0, process 0 can enter; if 1, process 1 can enter.

This works in terms of ensuring mutual exclusion, but it causes inefficiencies:

- If one process finishes its non-critical region quickly but the other is still busy outside its critical region, the first process must wait needlessly.
- This violates a required condition: no process running outside its critical region may block any process.

This solution requires strict alternation and prevents a process from entering its critical region twice in a row. Although it avoids race conditions, it is not practical.

---

Peterson’s Solution

Peterson’s algorithm is a software-only solution that combines intent signaling and turn-taking to achieve mutual exclusion for two processes.

It uses two shared components:

- `interested[2]`: an array indicating whether each process wants to enter its critical region.
- `turn`: an integer indicating whose turn it is.

When a process wants to enter:

1. It sets its `interested` flag to true.
2. It sets `turn` to itself.
3. It waits until the other process is not interested or it is its own turn.

This algorithm ensures that only one process enters the critical region at a time, and it satisfies all four necessary conditions for mutual exclusion. It is simple, correct, and does not rely on hardware support.

---

The TSL (Test-and-Set Lock) Instruction

Some CPUs provide special instructions like `TSL` (Test-and-Set Lock), which perform the following atomically:

- Copy the lock variable to a register.
- Set the lock variable to 1.

No other processor can access the lock variable during the execution of the `TSL` instruction, as it locks the memory bus.

This is different from disabling interrupts because it works even in multiprocessor systems.

Implementation involves:

1. A process calling `enter_region`, which repeatedly executes the `TSL` instruction until it successfully acquires the lock.
2. The process entering its critical region.
3. On exit, the process calls `leave_region`, which sets the lock to 0.

Correct operation depends on all processes calling `enter_region` and `leave_region` at appropriate times. If one process fails to release the lock, the system fails.

---
An alternative to `TSL` is the `XCHG` instruction, which atomically swaps the contents of a register and a memory location.

Intel x86 processors use `XCHG` for low-level synchronization. Its use is similar to `TSL`, and it provides another hardware-assisted way to ens

## 2.3.4 Sleep and Wakeup

 Busy Waiting Issues
- **Peterson's**, **TSL**, and **XCHG** solutions avoid race conditions but rely on **busy waiting**.
- Busy waiting wastes CPU time and can cause **priority inversion**:
  - A high-priority process may wait forever if a low-priority process is holding a lock and is never scheduled to release it.

---

 Sleep and Wakeup Mechanism
- **`sleep`**: Suspends the calling process until another process explicitly wakes it up.
- **`wakeup(process)`**: Wakes a specific process that is sleeping.
- Alternative design: both `sleep` and `wakeup` use a memory address to match sleeping and waking actions.

---

The Producer-Consumer Problem (Bounded Buffer)
- Two processes share a **fixed-size buffer**:
  - The **producer** adds items to the buffer.
  - The **consumer** removes items from the buffer.
- A variable `count` tracks the number of items in the buffer.
- If the buffer is **full**, the producer goes to `sleep` until the consumer removes an item.
- If the buffer is **empty**, the consumer goes to `sleep` until the producer adds an item.

---

 Race Condition Example
1. The buffer is empty.
2. The consumer reads `count == 0` and is about to go to sleep.
3. The scheduler switches to the producer.
4. The producer adds an item, sets `count = 1`, and calls `wakeup`.
5. The consumer hasn’t actually gone to sleep yet, so the `wakeup` is **lost**.
6. Later, the consumer goes to sleep (forever), and the producer eventually sleeps too—**deadlock**.

---
Wakeup Waiting Bit (Partial Fix)
- Add a **wakeup-waiting bit**:
  - If `wakeup` is called while the target process is still awake, set the bit.
  - Before sleeping, a process checks the bit:
    - If set, it clears the bit and **does not sleep**.
- This allows missed wakeups to be preserved temporarily.
- **Limitation**: Works for **two processes**, but not for more.
  - Generalizing this approach (multiple bits, flags, etc.) is complex and fragile.
- **Conclusion**: A more robust synchronization mechanism is needed for systems with more than two cooperating processes.

## 2.3.5 Semaphores

### Introduction to Semaphores
- Introduced by **E. W. Dijkstra (1965)** to solve the **lost wakeup problem**.
- A **semaphore** is an integer variable representing the number of saved wakeups:
  - **0**: No wakeups saved.
  - **Positive**: One or more wakeups are pending.

---

### Semaphore Operations
- Two atomic operations:
  - **`down`** (P): 
    - If value > 0: decrement and continue.
    - If value == 0: process sleeps until value becomes > 0.
  - **`up`** (V): 
    - Increments the value.
    - If processes are sleeping on the semaphore, one is awakened.
- These operations are **atomic**—cannot be interrupted.
- In Dijkstra’s notation:
  - **P**: Proberen (try)
  - **V**: Verhogen (raise)
  - More commonly referred to now as **down** and **up**.

---

### Solving Producer-Consumer Problem with Semaphores
- Semaphores resolve the **lost wakeup problem**.
- Must be implemented **indivisibly**:
  - Often done by **disabling interrupts** briefly during execution.
  - On **multiprocessor systems**, use a **lock** and instructions like **TSL** or **XCHG** to ensure only one CPU accesses the semaphore at a time.

---

### Semaphore Usage in Producer-Consumer
- Three semaphores:
  - **`full`**: Counts full slots (initially `0`)
  - **`empty`**: Counts empty slots (initially `buffer size`)
  - **`mutex`**: Ensures mutual exclusion when accessing the buffer (initially `1`)
- `mutex` is a **binary semaphore**:
  - Used to enforce **mutual exclusion**—only one process can access the critical section at a time.
  - Pattern: `down(mutex)` before entering critical region, `up(mutex)` after leaving.

---

### Semaphores and Interrupts
- Used to manage device I/O:
  - Each I/O device has a semaphore (initially `0`).
  - After initiating I/O, process does `down(device_semaphore)` to wait.
  - When interrupt occurs, the handler does `up(device_semaphore)` to unblock the process.

---

### Two Uses of Semaphores
1. **Mutual Exclusion (mutex semaphore)**:
   - Prevents multiple processes from accessing critical regions simultaneously.
   - Ensures **data integrity**.
2. **Synchronization (full/empty semaphores)**:
   - Controls the **order of events**.
   - Ensures:
     - Producer blocks if buffer is full.
     - Consumer blocks if buffer is empty.
