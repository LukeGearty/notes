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
