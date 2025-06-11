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

## Benefits

- **Flexible** configuration.
- **Efficient** user-level thread switching.
- **Kernel support** for blocking and I/O.



