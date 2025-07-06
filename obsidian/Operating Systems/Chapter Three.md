- Main memory (RAM) is a crucial part of a computer, and it has to be carefully controlled.
- Even though today's computers have _much_ more memory than old ones (like IBM 7094 in the 1960s), modern programs are also getting much larger.
- This creates a problem: more memory is available, but programs keep using more of it.
- There’s a saying (based on Parkinson’s Law): “Programs grow to use all the memory you give them.  
- Ideally, programmers want memory that is:
    - **Private** (not shared with others)
    - **Infinitely large** (so they never run out)
    - **Infinitely fast** (for instant access)
    - **Nonvolatile** (doesn’t lose data when the power is off)
    - **Cheap**
- Sadly, no current technology can give us all of these features at once
- Instead, we use a **memory hierarchy**—a layered system of different memory types:
    - **Cache memory**:
        - Very fast
        - Very expensive
        - Small size (a few megabytes)
        - Volatile (data is lost when power is off)
    - **Main memory (RAM)**:
        - Medium speed
        - Medium cost
        - Moderate size (a few gigabytes)
        - Also volatile
    - **Disk storage (like hard drives or SSDs)**:
        - Very slow
        - Very cheap            
        - Large size (a few terabytes)
        - Nonvolatile (keeps data even when power is off)
    - **Removable storage**:
        - Examples: DVDs, USB stick
        - Can be taken out and reused
- The **operating system (OS)** turns all these layers into something that feels like a single, usable memory system.
- The OS also has to manage this system so it runs efficiently
- The **memory manager** is the part of the OS that handles memory-related tasks
    - It keeps track of what parts of memory are being used.
    - It gives memory to programs (processes) when they need it.
    - It takes memory back when programs are done.

# No Memory Abstraction
- The simplest way to handle memory is to **not abstract it at all** — programs access physical memory directly.
- Early computers (before 1980) did this: programs worked with **physical memory addresses**, like `MOV REGISTER1,1000`.

- In this model:
  - Memory is just a big list of addresses, like 0 to MAX.
  - Each address stores some bits (usually 8 bits = 1 byte).
  - There’s **no protection** between programs.

- Problem: **Multiple programs can't run at the same time**.
  - If Program A writes to address 2000, it might **overwrite data** used by Program B.
  - This leads to **program crashes** and system instability.

- Even without abstraction, there were some variations in memory layout:
  - **Model (a)**: OS is at the **bottom** of RAM.
  - **Model (b)**: OS is in **ROM** at the **top** of memory.
  - **Model (c)**: **Device drivers** in ROM at top, rest of OS in RAM below.
    - Used in early PCs (e.g., MS-DOS) — the ROM part was called the **BIOS**.
  - **Issues with (a) and (c)**: A **bug in a user program** could erase the OS.

- In this setup, **only one process can run at a time**:
  - User types a command.
  - OS loads the program into memory and runs it.
  - When the program finishes, OS waits for the next command.

- Some parallelism is possible with **threads**, since they share memory:
  - But threads are limited — people usually want **separate programs** running.
  - Also, a system this primitive likely doesn't support threads anyway.

- It's still possible to run multiple programs using **swapping**:
  - OS **saves** the current memory contents to disk.
  - Then loads and runs another program.
  - Only **one program is in memory at a time**, so no conflicts.

- Early IBM 360 computers allowed **concurrent programs** with special hardware:
  - Memory divided into **2 KB blocks**.
  - Each block had a **4-bit protection key** in a CPU register.
  - Each program had a **PSW (Program Status Word)** with its own key.
  - If a program tried to access a block with a different key, the hardware would block it.
  - This **protected programs from each other**.

- Problem with this protection system:
  - Programs still used **absolute addresses** (e.g., jump to address 28).
  - If a program is moved in memory, the addresses **no longer point to the right place**.

- Example problem:
  - Program A and Program B are both 16 KB.
  - Program A starts at address 0 and jumps to address 24 — works fine.
  - Program B is loaded starting at address 16,384 but still jumps to address 28.
  - It ends up jumping into Program A’s code and **crashes**.

- What we really want is for each program to have its **own local memory space**:
  - So addresses like "jump to 28" mean something **inside the program**, not in global memory.

- IBM 360 tried to fix this with **static relocation**:
  - When a program is loaded at a specific address (e.g., 16,384), that number is **added to all addresses** in the program.
  - So “JMP 28” becomes “JMP 16412”.

- Issues with static relocation:
  - Slows down loading.
  - Requires knowing which values are **addresses** and which are **constants**.
    - For example: `MOV REGISTER1,28` should **not** be changed.
  - Loader needs extra data to tell which values to change.

- Even though modern computers don’t use direct physical memory anymore:
  - **Some devices still do**, like embedded systems and smart cards.
    - Radios, washing machines, and microwaves use programs in ROM that access absolute memory.
    - This is okay because the software is **preinstalled** and users can't run new programs.

- High-end embedded systems (e.g., smartphones) have real operating systems.
- Simple embedded systems may just use a **library that acts like an OS**, providing basic I/O and system calls.
  - Example: **e-COS** operating system, which is linked with the app like a regular code library.

# A memory Abstraction: Address Spaces
- Giving user programs direct access to physical memory has **major problems**:
  - Programs can accidentally or intentionally **overwrite the OS**, crashing the system.
  - It's **hard to run multiple programs at once**, even if they take turns on a single CPU.

- Modern systems (like personal computers) commonly run many programs at once.
  - This isn't possible without a memory **abstraction** — a way to make memory safer and more manageable.

- Two main problems to solve when allowing multiple programs in memory:
  - **Protection**: prevent programs from interfering with each other or the OS.
  - **Relocation**: allow programs to run from **different memory locations** than they were written for.

- The IBM 360’s memory keys helped with **protection**, but not **relocation**.
  - Relocating programs manually is possible, but slow and error-prone.

### The Address Space Abstraction

- A **better solution** is the idea of an **address space**.
  - Just like a **process** is an abstract CPU, an **address space** is an **abstract memory** for a process.
  - Each process gets its **own set of addresses** (its own "private" memory).
  - This prevents one program’s `address 28` from meaning the same as another’s.

- Address spaces are used everywhere:
  - Phone numbers (7-digit numbers = a number space).
  - I/O ports on x86 (0 to 16383).
  - IP addresses (IPv4 has 2³² possibilities).
  - Domain names like `.com` (a space of valid string combinations).

- The **hard part**: making sure address `28` in one program maps to **a different physical memory** than `28` in another.

### Base and Limit Registers (Dynamic Relocation)

- An early solution used on old systems (like CDC 6600 and Intel 8088).
- Each CPU has two hardware registers:
  - **Base register**: start address of the program in physical memory.
  - **Limit register**: how large the program is.

- When a process runs:
  - **Base** = starting location in memory (e.g., 16,384).
  - **Limit** = size of the program (e.g., 16,384).
  - Every time a memory access happens:
    - CPU **adds base** to the address.
    - **Checks** if the address is within the limit.
    - If not: **fault occurs**, stopping the invalid access.

- Example:
  - A program executes `JMP 28`, but the hardware translates it to `JMP 16412`.

- **Benefits**:
  - Each process has a **private address space**.
  - Programs don’t have to be rewritten when loaded into new locations.
  - Base/limit values can only be changed by the **OS** (on some CPUs).

- **Limitations**:
  - CPU must do an **addition + comparison** on every memory access.
  - Adds some **hardware overhead**.
  - Some CPUs (like Intel 8088) **didn’t fully support** this — missing limit registers or protection features.

---

### Swapping (Handling Too Many Processes)

- In real systems, the **total memory needed > available RAM**.
- OSes (like Windows, Linux, macOS) often run **dozens of background processes**.
  - Example: update checkers, email fetchers, network monitors.
  - User apps (like Photoshop) can use **hundreds of MB to GBs** of memory.

- Keeping everything in RAM at once is **impossible** on most systems.

- Two strategies to manage this:
  1. **Swapping**:
     - Load an entire process into memory.
     - Run it for a while.
     - Save it to **disk** and load another process.
     - Idle processes are kept on disk, not in memory.
     - May wake up periodically to check for events.
  2. **Virtual Memory**
     - Allows processes to run while only **partially loaded** in memory.

- Example of **swapping**:
  - A is in memory.
  - Then B and C are loaded in.
  - A is swapped out to disk.
  - D comes in and B goes out.
  - A is swapped back in — now at a new memory location.
  - **Addresses must be relocated** either:
    - By software when reloaded.
    - Or by hardware using base and limit registers.

- **Memory compaction**:
  - If swapping leaves small **holes** in memory, they can be combined by **moving programs down**.
  - Not often done — takes a **lot of CPU time**.
    - E.g., moving 16 GB of memory takes ~16 seconds on fast hardware.

- **Allocating memory to processes**:
  - If process size is **fixed**, memory allocation is simple.
  - If processes can **grow dynamically** (e.g., heap allocation):
    - OS needs to allow for growth.
    - If a process hits another, it might need to:
      - Move to a larger **hole**.
      - Cause another process to be **swapped out**.
      - Or be **paused** or **killed** if no memory is available.

- To avoid frequent moves, OS can **allocate extra space** when swapping in.
  - But don’t **swap unused space** to disk — it's wasteful.

- **Two growing segments**:
  - Many programs have:
    - A **stack** (grows downward).
    - A **heap/data segment** (grows upward).
  - OS can place stack at the top of a process’s memory and heap at the bottom.
  - If stack and heap **collide**, the process must:
    - Be moved to a bigger hole.
    - Be swapped out.
    - Or be killed.
- When memory is assigned dynamically, the OS must **track free and used memory**.
- Two main ways to manage this:
  1. **Bitmaps**
  2. **Free lists (linked lists)**

---

### Memory Management with Bitmaps

- **Memory is divided** into small **allocation units** (e.g., 4 bytes or several KB).
- For each unit, a **bit in the bitmap** tracks whether it’s used:
  - `0` = free, `1` = used (or the reverse).
- Example: If memory has 32n bits, the bitmap has only n bits — uses little space.

- **Tradeoff with allocation unit size**:
  - **Smaller units**: more accurate, less wasted memory, but **larger bitmap**.
  - **Larger units**: smaller bitmap, but **wastes more memory** if process size isn’t a perfect multiple.

- **Main issue with bitmaps**:
  - To allocate k units, OS must **scan for k consecutive 0s** in the bitmap.
  - Searching is **slow**, especially if it spans word boundaries in memory.

---

### Memory Management with Linked Lists

- Memory is seen as a list of **segments**:
  - Each segment is either:
    - A **process (P)**
    - A **hole (H)** — free space
- Each segment entry stores:
  - Type (P/H), start address, length, and pointer to next segment.

- List is often **sorted by memory address**:
  - Makes it easy to **merge holes** when processes terminate.
  - Can be implemented as a **double-linked list** for easy backward access.

- **When a process ends**, 4 cases occur:
  1. Surrounded by two processes → just change P to H.
  2. Next to one hole → merge two into one.
  3. Between two holes → merge all three into one.
  4. At end/start of memory → merge as appropriate.

---

### Memory Allocation Algorithms (Using the List)

- **First Fit**:
  - Search from beginning.
  - Allocate the **first hole big enough**.
  - Fast and simple.

- **Next Fit**:
  - Like first fit, but **starts search from where it left off** last time.
  - Slightly worse performance than first fit.

- **Best Fit**:
  - Searches entire list.
  - Picks the **smallest hole** that fits the request.
  - Tries to leave large holes unbroken for later use.
  - **Slower** and can create lots of **tiny unusable holes**.

- **Worst Fit**:
  - Always takes the **largest hole**.
  - Goal: leave a big leftover hole that’s still useful.
  - Usually performs **poorly** in practice.

---

### Optimizing the Allocation Process

- To improve speed:
  - Maintain **separate lists** for holes and processes.
    - Speeds up allocation.
    - But makes **deallocation (merging)** more complex.

- If using a **hole list sorted by size**:
  - **Best fit becomes faster**:
    - When a large-enough hole is found, it's automatically the smallest one.
  - In this setup:
    - **First fit and best fit** are equally fast.
    - **Next fit is unnecessary**.

- **Optimization for hole data**:
  - Store hole info **inside the hole** itself.
  - First word = size, second word = pointer to next hole.
  - Saves space (no need for full external structure for holes).

---

### Quick Fit Allocation

- **Quick fit** maintains **separate lists for common hole sizes**.
  - Example:
    - One list for 4 KB holes
    - One for 8 KB holes
    - One for 12 KB holes, etc.
  - Odd-sized holes can go in a "miscellaneous" list.

- **Pros**:
  - Very **fast allocation** — just check the right list.
  
- **Cons**:
  - **Merging** free memory is **hard** and expensive.
  - If no merging is done, memory becomes **fragmented** with many small unusable holes.

---

# Virtual Memory

