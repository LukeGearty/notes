A modern computer consists of a lot of different devices that, if an application programmer needed to understand everything, nothing would get done. Managing all the components of the computer is the job of the operating system.

The interface the user sees -either the shell or the gui - is not the operating system, it just interacts with the operating system. 

Kernel Mode - has access to all hardware and instructions, operating system runs here
User Mode - only a subset of the machine instructions are available. The user interface is the lowest level of user-mode software

Distinction between user mode and kernel mode is blended in some systems like embedded (may not have kernel mode), and there are some user mode programs that perform privileged functions (password changing).

# What is an Operating System?

## The operating System as an Extended Machine
- **Machine-level architecture** (e.g., instruction sets, I/O, memory) is complex and difficult to program, especially for input/output tasks.
- Example: SATA hard disks have highly complex interfaces—early documentation exceeded 450 pages and has only grown more complicated.
- **Disk drivers** abstract this complexity, allowing programs to read/write disk blocks without handling hardware details.
- **Operating systems** provide an even higher abstraction: **files**, enabling programs to interact with data (e.g., photos, emails, songs) without dealing with hardware.
- **Abstractions** simplify complex problems by separating the definition/implementation of the abstraction from its use.
- The **operating system's main role** is to create and manage useful abstractions, hiding the messy and inconsistent hardware interfaces.
- Hardware is inherently **ugly and inconsistent**, often due to legacy requirements, cost-cutting, or lack of design consideration for software developers.
- Operating systems transform this "ugly" hardware into **clean, elegant, and consistent** interfaces for software.

## The operating system as a resource manager
- There are **two views** of an operating system:
    - **Top-down view:** OS provides **abstractions** to make hardware easier to use.
    - **Bottom-up view:** OS **manages resources** in a complex system (CPUs, memory, I/O, etc.).
- In the **bottom-up view**, the OS handles:
    - **Controlled allocation** of hardware among competing programs.
    - **Avoiding conflicts**, e.g., when multiple programs try to use the same device (like a printer).
    - **Buffering output** to manage simultaneous access (e.g., storing print jobs on disk).
- On **multi-user systems**, the OS must:
    - **Protect resources** from user interference.
    - **Support sharing** of both hardware and data (e.g., files, databases).
    - **Track and mediate** resource usage and conflicts.
- **Resource management** includes **multiplexing**:
    - **Time multiplexing:** Users/programs take turns using a resource.
        - Example: One CPU is shared by rotating programs.
    - **Space multiplexing:** Each user/program gets part of a resource.
        - Example: Memory is divided among running programs.
        - Disks store files from multiple users simultaneously
- The OS ensures **fairness, protection**, and **efficiency** when sharing resources in both time and space.


# The History of Operating Systems
- **Operating systems have evolved** alongside the development of computer hardware.
- OS history is roughly organized into **generations**, aligned with **computer generations**—though this is an **imprecise but useful framework** 
- **Evolution was not linear**: developments often overlapped, with **false starts and dead ends** 
- The **first true digital computer** was Charles Babbage’s **analytical engine** (1800s), which was **mechanical** and never fully operational due to technological limitations.
- Babbage’s machine **had no operating system**.
- **Ada Lovelace**, daughter of poet Lord Byron, is considered the **first programmer**, having written software for Babbage’s design.
    - The programming language **Ada®** is named in her honor.


## The First Generation

- **World War II** sparked a surge in digital computer development after Babbage’s earlier attempts
- Key early computers:
    - **Atanasoff-Berry Computer (Iowa State):** First functioning digital computer, used vacuum tubes.
    - **Z3 (Germany, Konrad Zuse):** Built using electromechanical relays.
    - **Colossus (UK):** Built at Bletchley Park, involved Alan Turing.
    - **Mark I (Harvard):** Created by Howard Aiken.
    - **ENIAC (USA):** Developed by Mauchley and Eckert.
- These machines were **primitive**, slow, and lacked operating systems.
- **Early computing was manual**:
    - Engineers handled all aspects: building, programming, operating, and maintenance.
    - Programming done in **machine language** or by **plugboards** (physical wiring).
    - **No high-level or assembly languages** existed.
    - Programmers signed up for time on the machine and physically interacted with it.
    - Machines were unreliable due to components like **vacuum tubes** burning out.
    - Typical tasks: basic mathematical computations (e.g., tables, trajectories).
- **1950s improvement**: **Punched cards** replaced plugboards for entering programs, simplifying programming input.

## The second generation
- **Transistor era (mid-1950s):** Made computers more **reliable**, allowing them to be **sold commercially** and used productively
- Clear **role separation** emerged: designers, builders, operators, programmers, and maintenance personnel
- **Mainframes**:
    - Expensive, large machines housed in **air-conditioned computer rooms**.
    - Operated by professionals; only large institutions could afford them.
    - **Program submission process**:
        - Write code (e.g., FORTRAN/assembly) on paper → punch onto cards → submit to input room.
        - Operators manually loaded compilers and programs; results collected later.
    - **Inefficiencies** due to manual handling led to wasted computer time.
- **Batch systems** introduced to reduce idle time:
    - Jobs collected, read to **magnetic tape** using smaller computers like the IBM 1401.
    - **Computation done on powerful machines** like IBM 7094.
    - Jobs run automatically one after another via a **basic operating system**.
    - **Output written to tape** and printed later offline.
- **Job structure** (controlled by punch cards):
    - `$JOB`: job info (run time, account, name)
    - `$FORTRAN`: load FORTRAN compiler
    - Program code
    - `$LOAD`: load compiled program
    - `$RUN`: execute program with input data
    - `$END`: mark end of job
    - These **control cards** were early versions of modern **command-line interfaces**.
- **Applications**: Mainly scientific and engineering tasks using **FORTRAN** and **assembly**.
- **Typical OS**: FMS (Fortran Monitor System), IBSYS (for IBM 7094).

## The third gen
By the early 1960s, the computer industry had split into two incompatible product lines: word-oriented scientific computers like the IBM 7094 and character-oriented commercial machines like the IBM 1401. This division was costly and inefficient. IBM addressed the problem by launching the System/360 series—a family of software-compatible computers ranging from small to powerful models. The innovation of a unified architecture capable of both scientific and commercial computing made the System/360 an immediate success. This approach was later adopted by other manufacturers.

The System/360 was the first major line to use small-scale integrated circuits (ICs), offering better price-to-performance than earlier transistor-based systems. Over time, backward-compatible successors like the IBM 370, 4300, 3080, 3090, and ultimately the zSeries were introduced. These systems are still used today for demanding applications such as airline reservations and web servers.

However, the ambition to have a single operating system (OS/360) work across all models led to massive complexity. OS/360 became a sprawling system of millions of lines of assembly code written by thousands of programmers. Although full of bugs and continuously updated, OS/360 still satisfied many users and introduced crucial concepts like **multiprogramming**, which allowed multiple jobs to reside in memory, improving CPU utilization.

Another advancement was **spooling** (Simultaneous Peripheral Operation On Line), which queued input/output on disks instead of requiring manual tape operations. This innovation eliminated the need for intermediary systems like the IBM 1401.

Despite their capabilities, third-generation systems remained batch-oriented, frustrating programmers due to delayed feedback cycles. This prompted the development of **timesharing** systems, allowing multiple users to interact with the system via terminals. One notable early timesharing system was CTSS, developed at MIT.

MIT, Bell Labs, and General Electric aimed to create a centralized "computer utility" with **MULTICS** (MULTiplexed Information and Computing Service), designed to support hundreds of users. Though ambitious, MULTICS suffered delays and technical challenges, especially with its problematic PL/I compiler. It eventually found limited commercial use and loyal users like GM, Ford, and the NSA. Many of its ideas influenced modern operating systems, especially **UNIX**.

UNIX emerged from Bell Labs as a simplified version of MULTICS, initially written by Ken Thompson for a PDP-7 minicomputer. Its wide source code availability led to many variants, primarily **System V** and **BSD**. The **POSIX** standard was developed to bring consistency across UNIX systems.

MINIX, an educational UNIX-like OS, was created in 1987 and later evolved into **MINIX 3**, which features fault tolerance and high reliability. Inspired by MINIX, **Linux** was developed by Linus Torvalds as a free, production-ready UNIX clone. It has since grown into a major operating system used globally.

Though the original vision of a computer utility faded, it resurfaced in the form of **cloud computing**, where computing is centralized in large data centers and accessed via simple client devices—an echo of the MULTICS philosophy.

## The fourth gen
The advent of LSI (Large Scale Integration) circuits led to the personal computer era, enabling individuals to own computers, unlike minicomputers which were typically used by institutions. The Intel 8080 microprocessor (1974) inspired the development of the CP/M operating system by Gary Kildall, who later founded Digital Research. CP/M became a dominant OS for microcomputers through the late 1970s.

In the early 1980s, IBM sought an OS for its new PC and initially approached Kildall. A missed opportunity led IBM to Bill Gates, who acquired DOS from Seattle Computer Products. Microsoft modified it into MS-DOS, which became widespread due to bundling strategies, contrasting with Kildall’s direct-to-consumer approach. CP/M quickly lost relevance.

Apple’s Steve Jobs, inspired by Xerox PARC’s GUI innovations, launched the Apple Macintosh—a successful, user-friendly system. Apple later adopted UNIX-based technology, making macOS a UNIX derivative.

Microsoft’s Windows began as a GUI shell for MS-DOS and evolved into standalone OS versions: Windows 95, 98, and eventually the NT line. Windows NT, created by David Cutler, was a full 32-bit system influenced by VAX VMS. Windows 2000 and XP followed, the latter becoming widely adopted. Despite the failure of Windows Me and the mixed reception of Vista, Windows 7 succeeded due to its performance and stability. Windows 8 targeted touchscreen devices but was slow to gain traction.

UNIX remained significant, especially in servers and enterprise settings. Linux, a UNIX-like OS, gained popularity on desktops and servers. BSD UNIX derivatives, such as FreeBSD, also became influential—modern macOS is based on FreeBSD. UNIX systems support both command-line interfaces and GUIs like Gnome and KDE on the X Window System.

The 1980s also saw the rise of network and distributed operating systems. Network operating systems allowed users to interact with multiple connected machines. In contrast, distributed systems presented a unified interface, masking the complexity of multiple processors. These systems required advanced scheduling and resource management due to inherent communication delays and incomplete system state knowledge.

## The fifth Gen
- **1940s**: The concept of portable communication begins with _Dick Tracy's two-way wrist radio_ in comics.
- **1946**: The first mobile phone debuts—large (40 kg), car-bound.
- **1970s**: The first true handheld mobile phone emerges, nicknamed _“the brick”_ due to its 1 kg weight.
- **Today**: Nearly 90% of the global population uses mobile phones, now capable of far more than calls (e.g., web, email, GPS, gaming, wearables).
- **Mid-1990s**: First _real smartphones_ appear with Nokia’s **N9000** (a phone + PDA combo).
- **1997**: Ericsson coins the term _smartphone_ with the GS88 “Penelope”.
**Smartphone OS Timeline**
**Early 2000s**:
	**Symbian OS** dominates (used by Nokia, Sony Ericsson, etc.).
    **BlackBerry OS** (by RIM) launches in 2002, targets business users.

**2007**: Apple releases the first iPhone with **iOS**, reshaping consumer expectations. **2008**: Google launches **Android** (Linux-based, open source), quickly gains traction with manufacturers and developers.
**2011**: Nokia abandons Symbian in favor of **Windows Phone**.
- **Android**: Market leader due to openness, customization, Java support, and a large app ecosystem.
- **iOS**: Strong second, especially in consumer and premium markets
- **Others** (Symbian, BlackBerry, Windows Phone): Rapid decline or discontinued.

# Computer Hardware Review
**Operating Systems and Computer Hardware: Summary Notes**
1. **Relationship Between OS and Hardware**
- An operating system (OS) is closely connected to the hardware of the computer it operates on.
- It plays two key roles:
    - Extends the computer’s instruction set.
    - Manages hardware resources effectively.
- To function properly, an OS must understand hardware behavior, especially how it appears to the programmer.
1. **Importance of Hardware Knowledge**
- Since OS design depends on hardware, a basic understanding of computer hardware is necessary.
- OS designers must be aware of hardware issues and interactions.
1. **Basic Computer Model (Simplified)**
- Components:
    - CPU
    - Memory
    - I/O Devices
- All are connected via a system bus, which facilitates communication among them.
- This model is a simplified abstraction suitable for introductory understanding.
1. **Real-World Complexity**
- Modern personal computers feature multiple interconnected buses, making their structure more complex than the basic model.

1. **Further Reading**
- For deeper understanding, refer to:
    - Tanenbaum and Austin (2012)
    - Patterson and Hennessy (2013)



## Processors
### **1. CPU Basics**

- The **CPU** (Central Processing Unit) is the **“brain” of the computer**.    
- Follows a cycle:

    1. **Fetch** instruction from memory        
    2. **Decode** the instruction
    3. **Execute** the instruction
    4. Repeat until program ends
- Each CPU can only execute its own **instruction set** (e.g., x86 ≠ ARM).
### **2. CPU Registers**
- **Registers**: Small, fast storage inside the CPU to avoid slow memory access.
- Common instructions: Load from memory → register, and store from register → memory.
- Types of registers:
    - **General-purpose registers**: Hold variables and temporary results.
    - **Program Counter (PC)**: Holds the address of the next instruction.
    - **Stack Pointer (SP)**: Points to the top of the current call stack in memory.
    - **Program Status Word (PSW)**: Holds CPU state info (mode, priority, condition codes).
### **3. CPU Context Switching**
- The OS must save all registers when switching from one process to another to **preserve program state**
### **4. Performance Enhancements**
- **Pipelining**: Overlapping instruction stages (fetch, decode, execute) for efficiency.    
    - e.g., while executing instruction `n`, CPU decodes `n+1` and fetches `n+2`.
- **Superscalar CPUs**:
    - Multiple execution units (integer, floating-point, logic).
    - Multiple instructions can be processed **out-of-order**.
    - Requires hardware and OS to maintain correct results.
### **5. CPU Modes and System Calls**

- **Kernel mode**
    - Full access to all instructions and hardware.
    - OS runs in this mode.
- **User mode**:
    - Restricted instruction access (no direct I/O or memory protection changes).
- **System Call**:
    - Switches from user mode to kernel mode using `TRAP` instruction.
    - A way for user programs to request OS services.
### **6. Hardware Traps**
- Other traps (besides system calls) are triggered by hardware for exceptions (e.g., divide-by-zero).
- OS decides how to handle them (terminate, ignore, or return control to program).
### **7. Multithreading**
- **Multithreading (Hyperthreading)**
    - CPU holds state for multiple threads.
    - Can switch between threads quickly (nanoseconds) to hide latency.
    - Appears to OS as multiple CPUs.
### **8. Multicore Processors**
- **Multicore chips** have multiple independent CPUs (cores) on one chip.
    - Examples: Intel Xeon Phi, Tilera TilePro (60+ cores).
    - Requires **multiprocessor-aware OS** to manage effectively.
- Poor scheduling can result in underutilization (e.g., two threads on one core while another is idle).
### **9. GPUs**

- **GPUs** have **thousands of simple cores** for massive parallel tasks (e.g., graphics rendering)    
- Great for tasks with lots of small computations.
- Not suitable for general OS processing but can assist with specific tasks (e.g., encryption, networking).


## Memory
### **Memory in Computers – Key Concepts**
#### **1. Memory Goals & Hierarchy**
- **Ideal memory**: extremely fast, large, and cheap – but no technology achieves all three.
- **Solution**: use a **memory hierarchy**, trading off speed, size, and cost:
    - **Registers** (fastest, smallest, most expensive)
    - **Cache** (very fast, limited size)
    - **Main Memory (RAM)** (slower, larger, cheaper)
    - **Secondary Storage** (e.g., SSDs, HDDs, flash)
#### **2. Registers**
- Located **within the CPU**, made from same material = **no delay** in access.
- Example: 32×32 bits (32-bit CPU), 64×64 bits (64-bit CPU) → typically **<1 KB** total.
- **Managed by programs/software** (not hardware).
#### **3. Cache Memory**
- **Hardware-managed** memory between CPU and RAM.
- Divides RAM into **cache lines** (e.g., 64 bytes each).
- **Cache hit**: data found in cache → fast access (~2 clock cycles).
- **Cache miss**: fetch from RAM → slower.
- **Multiple levels**:
    - **L1**: inside CPU (e.g., 16 KB each for instructions and data), fastest.
    - **L2**: larger (megabytes), slower than L1.
    - Some CPUs also have **L3** caches (shared across cores).
#### **4. Cache Design Considerations**
- Key questions in any caching system:
    1. **When** to cache new items?
    2. **Where** in the cache to store them?
    3. **What** to evict when full?
    4. **Where** to store evicted items?
- Cache line selection may use **bits of memory address**.
- Modified cache lines must be **written back** to RAM if changed.
#### **5. Caching Beyond Memory**
- Used throughout computing:
    - File data
    - Path-to-disk mappings
    - DNS (URL-to-IP conversions)
        

#### **6. Cache in Multicore CPUs**

- Two main strategies:
    - **Shared L2 cache** (Intel): needs complex control logic.
    - **Separate L2 caches per core** (AMD): simpler but harder to keep consistent.
#### **7. Main Memory (RAM)**
- **Workhorse** of the memory system.
- All **cache misses** go to RAM.
- Typically **hundreds of MBs to GBs**.
- Also known as **core memory** (historical term from ferrite cores in the 1950s–60s).
#### **8. Non-Volatile Memory**
- **ROM (Read-Only Memory)**:
    - Programmed at factory, **unchangeable**, fast, inexpensive.
    - Stores **bootstrap loader**, sometimes device firmware.
- **EEPROM / Flash Memory**:
    - Can be **rewritten**, but **slower than RAM**.
    - Allows **bug fixes** and updates in the field.
    - **Wears out** with too many write-erase cycles.
- **Flash** is common in:    
    - Digital cameras
    - Portable music players
    - Mobile devices
#### **9. CMOS Memory**
- **Volatile**, low-power memory.
- Stores:
    - **System time/date**
    - **Hardware configuration settings**
- Powered by a small **battery**.
- Battery failure can cause the system to **"forget"** settings.

## Disks
### **Magnetic Disks, SSDs, and Virtual Memory – Key Concepts**
#### **1. Magnetic Disk (Hard Disk Drive - HDD)**
- **Cheaper** and **larger** than RAM:
    - ~100× cheaper per bit
    - ~100× larger capacity
- **Much slower** than RAM:
    - ~1000× slower for random access (due to mechanical parts)

#### **2. Disk Structure & Operation**
- **Components**:
    - One or more **metal platters**
    - **Spinning speeds**: 5400, 7200, 10,800+ RPM
    - **Mechanical arm** with **read/write heads** (like phonograph needle)
- **Data organization**:
    - **Tracks**: concentric rings per platter surface
    - **Cylinders**: group of tracks under one arm position
    - **Sectors**: subdivisions of a track (typically 512 bytes each)
    - Outer tracks can hold **more sectors** than inner ones
#### **3. Disk Access Times**
- **Seek time**: moving arm to correct track:
    - ~1 ms (adjacent track), **5–10 ms** (random access)
- **Rotational latency**: wait for sector to rotate under head:
    - ~5–10 ms depending on RPM
- **Transfer rate** (once in position):
    - ~50 MB/s (low-end) to 160 MB/s (high-end)
#### **4. SSDs (Solid State Drives)**
- **No moving parts** (unlike HDDs
- Use **flash memory**
- **Non-volatile**: retain data after power loss
- Only resemble disks in:
    - Storing **large amounts of data**
    - Being **non-volatile**
- Typically **faster** and more **durable** than HDD
---

#### **5. Virtual Memory**
- Enables programs to exceed physical RAM size.
- Stores **inactive data** on disk, keeps **active parts** in RAM.
- Uses RAM as a **cache for disk**.
- Requires **on-the-fly address translation** via:
    - **MMU (Memory Management Unit)** – maps virtual addresses to physical ones
- Affects performance due to:
    - **Page faults** (if needed data not in RAM)
    - **Translation delays**
---

#### **6. Impact on System Performance**
- **Context switches** (changing running program) can be costly:
    - Cache may need to be **flushed**
    - MMU mappings must be **updated**
- Programmers try to **minimize context switches** for efficiency.


## I/O Devices
#### **1. I/O Devices and Controllers**
- **I/O devices** interact **heavily with the OS**.
- Each device is split into:
    - **Controller**: hardware that accepts OS commands and controls the device.
    - **Device**: physical hardware (e.g., disk, scanner, keyboard).
- **Example (Disk Controller)**:
    - Converts sector number → cylinder/sector/head
    - Adjusts for remapped/bad sectors
    - Moves the arm, waits for the correct sector, reads data, checks checksum
    - Assembles bits into words and stores them in memory
- Controllers often contain **embedded computers** for complex logic.
#### **2. Device Interfaces**
- **Devices have simple, standardized interfaces** (e.g., SATA).
    - **SATA** = Serial ATA → AT Attachment → IBM's 1984 PC-AT (6 MHz 80286)
    - Standardization allows **interchangeability** (any SATA disk with any SATA controller).
- **OS only sees controller interface**, not actual device details.
    
---

#### **3. Device Drivers**
- **Software that interacts with a controller**
- Translates OS commands into hardware-specific actions.
- **Each OS requires a separate driver per device type**.
    - Example: A scanner may need drivers for Windows, Linux, macOS.
- **Drivers usually run in kernel mode**, though some systems (e.g., MINIX 3) support user-space drivers
#### **4. Loading Drivers**
Three methods
1. **Recompile kernel with driver** → reboot (old UNIX)
2. **Declare driver in config file** → OS loads on boot (e.g., Windows
3. **Dynamic loading** while running (common now, essential for USB/hot-plug devices)
#### **5. Device Registers & Communication**
- Controllers have **registers** for parameters:
    - Disk address, memory address, sector count, direction
- **I/O Port Space**: the set of all device registers
- **Two register access methods**:
    1. **Memory-mapped I/O**:
        - Registers mapped into memory space.
        - Accessed like regular memory.
        - No special I/O instructions needed.
        - Uses up part of the address space.
    2. **Port-mapped I/O**:
        - Registers in separate port space.
        - Requires special `IN` and `OUT` CPU instructions.
        - Doesn’t consume memory space.

#### **6. I/O Methods**
**Three main ways to perform I/O**:
1. **Busy Waiting (Polling)**:
    - Driver continuously checks if device is done.
    - Ties up CPU = inefficient.
2. **Interrupt-Driven I/O**:
    - Driver starts I/O and **returns**.
    - **Device generates interrupt** upon completion.
    - **Interrupt handling process** (see Fig. 1-11a & b):
        1. Driver sets registers, starts controller.
        2. Controller signals interrupt controller chip.
        3. If ready, interrupt controller signals CPU.
        4. CPU acknowledges, gets device number from bus.
        5. CPU saves current state (PC, PSW), switches to kernel mode.
        6. Uses **interrupt vector** to find the handler.
        7. Handler finishes processing, returns to interrupted user code.
3. **DMA (Direct Memory Access)**:
    - **DMA chip** moves data **without CPU intervention**.
    - CPU initializes DMA: transfer size, direction, memory address, device.
    - DMA runs independently; signals completion via **interrupt**.
    - **Efficient** for large data transfers.

#### **7. Interrupt Handling**
- **Interrupts can happen at bad times**, even during another interrupt.
- CPU can **disable/enable interrupts**:
    - While disabled, interrupts are pending but not handled.
    - **Interrupt controller** queues them and picks highest priority.
- Devices have **fixed priorities**; higher-priority interrupts are serviced first.

## Buses

### Bus Architecture Evolution & PCIe
- Original single-bus designs (like in IBM PC) couldn't handle increased CPU/memory speed and traffic.
- Modern x86 systems use **multiple buses** (cache, memory, PCIe, PCI, USB, SATA, DMI).
- The **main system bus is PCIe** (Peripheral Component Interconnect Express).
- PCIe was developed by Intel to replace PCI and ISA
- PCIe uses **dedicated point-to-point serial lanes**, not shared parallel buses
- **Parallel PCI**: 32-bit words sent over 32 wires.  
    **Serial PCIe**: sends all bits through a single lane, with multiple lanes used for bandwidth.
- PCIe is upgraded frequently (e.g., PCIe 2.0 = 64 Gbps with 16 lanes; PCIe 3.0 & 4.0 double each time).
- Legacy **PCI devices** still exist and are connected via a separate hub processor, potentially leading to a **bus tree**.

### System Bus Layout (Fig. 1-12)
- CPU connects to memory over **fast DDR3 bus**.
- External graphics cards connect over **PCIe**.
- All other devices go through a **hub via DMI (Direct Media Interface)**
- Hub connects to:
    - USB devices via **USB bus**
    - Storage via **SATA**
    - Network via **PCIe**
    - Legacy devices via **PCI**
### Caches

- Each CPU core has a **dedicated cache**.
- There is also a **shared cache** among cores.
- Each cache introduces an additional bus.

### USB (Universal Serial Bus)
- Designed for connecting **slow I/O devices** (e.g., keyboard, mouse).
- Now very fast:
    - USB 1.0: 12 Mbps
    - USB 2.0: 480 Mbps
    - USB 3.0: 5 Gbps
- Uses a **centralized polling system** every 1 ms.
- Devices connect with **4–11 wire connectors**; some wires provide power.
- Supports **hot-plugging** (no reboot required).

### SCSI (Small Computer System Interface)
- High-performance bus for fast devices (e.g., **disks, scanners**)
- Found mostly in **servers and workstations**.
- Speed: up to **640 MB/sec**.

### Plug and Play (PnP)
- Developed by **Intel & Microsoft** for automatic I/O configuration.
- Inspired by Apple's early approach.
- Before PnP:
    - Each I/O card had fixed **interrupt levels** and **I/O addresses**.
    - Examples:
        - Keyboard: IRQ 1, I/O 0x60–0x64
        - Floppy: IRQ 6, I/O 0x3F0–0x3F7
        - Printer: IRQ 7, I/O 0x378–0x37A
- Conflicts occurred when two devices (e.g., sound card and modem) shared the same IRQ.
- Solution before PnP: **manual DIP switches/jumpers**—error-prone and complex
- PnP automatically:
    - Detects connected devices.
    - Assigns non-conflicting **IRQ and I/O addresses**.
    - Communicates this info to each device.
- Closely tied to the **booting process**.

## Boot
- PC has a **parentboard** with a **BIOS** program in flash RAM.
- BIOS handles **low-level I/O** (keyboard, screen, disk)
- On boot:
    - BIOS runs and **checks RAM** and **basic devices**.
    - **Scans PCIe and PCI buses** for connected devices.
    - **Configures new devices** if found.
- BIOS checks **boot order** from CMOS memory.
    - Tries **CD-ROM or USB** first.
    - If that fails, **boots from hard disk**.
- **Reads first sector** (boot sector) from boot device.
    - Runs code to find **active partition**.
    - Loads **secondary bootloader** from that partition.
    - Secondary loader loads the **operating system**.
- Operating system:
    - **Queries BIOS** for hardware info.
    - Checks for **device drivers**.
        - Prompts for CD-ROM or **downloads** if missing.
    - **Loads drivers into kernel**.
    - Initializes system, starts **background processes**.
    - Launches **login screen or GUI**

# The Operating System Zoo

## Mainframe Operating Systems
- **Mainframe OS Overview**:    
    - Designed for room-sized mainframe computers used in large corporate data centers.
    - Optimized for high I/O capacity (e.g., 1000+ disks, millions of gigabytes of data).
    - Often used as high-end web servers or for large-scale business transactions.
- **Key Functions**
    - **Batch Processing**
        - Runs routine, non-interactive jobs.
        - Example: Insurance claims processing, sales reports.
    - **Transaction Processing**:
        - Handles many small, fast transactions.
        - Example: Bank check processing, airline reservations.
    - **Timesharing**:
        - Allows multiple users to access the system remotely and simultaneously.
        - Example: Querying large databases.            
- **Notable OS Examples**:
    - OS/390 (descendant of OS/360).
    - Being gradually replaced by UNIX-based systems like Linux

## Server Operating Systems
- **Server OS Overview**:
    - Runs on servers, which can be large personal computers, workstations, or mainframes.
    - Designed to support multiple users over a network.
    - Enables sharing of hardware and software resources.
- **Common Services Provided**
    - **Print Service** – manages networked printers.
    - **File Service** – stores and manages access to files.
    - **Web Service** – hosts websites and handles incoming requests.
- **Usage Examples**
    - Internet service providers use servers to support customers.
    - Websites use servers to store web content and manage traffic.
- **Notable OS Examples**:
    - Solaris
    - FreeBSD
    - Linux
    - Windows Server 201x

## Multiprocessor Operating Systems
- **Multiprocessor OS Overview**:
    - Designed for systems with multiple CPUs connected in a single system.
    - System types include **parallel computers**, **multicomputers**, and **multiprocessors**, depending on connection and resource sharing.
    - Requires specialized OS features for **communication**, **connectivity**, and **consistency**
- **Relation to Other OS Types**:
    - Often based on or adapted from server operating systems.
    - Includes additional capabilities for managing multiple processors efficiently.
- **Modern Relevance**:
    - Multicore chips in personal computers are a small-scale example of multiprocessors.
    - As core counts increase, desktop and notebook OSes must manage multiprocessor systems.    
    - The challenge lies more in application design than OS design
- **Notable OS Examples**
    - Windows
    - Linux
    - Both support and operate on multiprocessor systems.

## Personal Computer Operating Systems

- **Personal Computer OS Overview**
    - Designed to support a **single user** efficiently.
    - Modern versions support **multiprogramming** (running many programs simultaneously).
    - Often launch **dozens of programs at boot time**.
- **Common Uses**:
    - Word processing
    - Spreadsheets
    - Games
    - Internet access
- **Notable OS Examples**:
    - Linux
    - FreeBSD
    - Windows 7
    - Windows 8
    - macOS (formerly OS X)
- **General Note**
    - Most widely known and used category of operating systems.
    - Many people are unaware that other types of OSes even exist.

## Handheld Computer Operating Systems
- **Handheld Computer OS Overview**:
    - Designed for small, portable devices like **smartphones**, **tablets**, and formerly **PDAs** (Personal Digital Assistants).
    - Must support operation while held in the user’s hand.
- **Typical Features of Devices**
    - **Multicore CPUs**
    - **GPS**, **cameras**, and various **sensors**
    - Large amounts of memory
    - Sophisticated, responsive operating systems
    - Support for a wide range of third-party **apps**
- **Notable OS Examples**:
    - **Android** (by Google)
    - **iOS** (by Apple)
    - Plus various smaller competitors
- **General Note**:
    - These OSes dominate the mobile market.
    - Known for massive ecosystems of downloadable applications.

## Embedded Operating Systems
- **Embedded OS Overview**:
    - Runs on computers embedded in devices **not typically viewed as computers**.
    - Devices **do not allow user-installed software**; all software is built-in.
- **Common Devices**:
    - Microwave ovens
    - TV sets
    - Cars
    - DVD recorders
    - Traditional phones
    - MP3 players
- **Key Characteristics**
    - **Software is stored in ROM** (read-only memory).
    - **No untrusted or third-party apps** can be installed.
    - **No need for application-level protection**, simplifying system design.
- **Notable OS Examples**:
    - Embedded Linux
    - QNX
    - VxWorks


## Sensor-Node Operating Systems
## Sensor Node Operating Systems

- **Overview**:  
  - Used in networks of tiny, wireless sensor nodes that communicate with each other and a base station.  
  - Deployed for a wide range of tasks, including:
    - Building security
    - Border protection
    - Forest fire detection
    - Weather monitoring
    - Military surveillance

- **Hardware Characteristics**:  
  - Small, battery-powered computers with:
    - CPU
    - RAM
    - ROM
    - Radios for wireless communication
    - One or more environmental sensors

- **Challenges & Requirements**:  
  - Must function for long periods unattended in harsh environments.  
  - Must tolerate frequent node failures (e.g., due to battery depletion).  
  - Energy efficiency and simplicity are critical due to limited power and memory.

- **OS Characteristics**:  
  - Small and lightweight.  
  - Typically **event-driven** (responds to external inputs or timers).  
  - All software is pre-installed (no user downloads), simplifying design.  

- **Notable OS Example**:  
  - **TinyOS**
## Real-Time Operating Systems
## Real-Time Operating Systems (RTOS)

- **Overview**:  
  - Designed for applications where **timing is critical**.  
  - Used to monitor and control real-world events with strict timing constraints.

- **Key Types**:  
  - **Hard Real-Time Systems**:  
    - Must meet deadlines **absolutely**.  
    - Missing a deadline can result in system failure or physical damage.  
    - Examples:
      - Industrial process control
      - Avionics
      - Military systems
      - Car assembly lines (e.g., welding robots)
  
  - **Soft Real-Time Systems**:  
    - Occasional missed deadlines are **acceptable**, though not ideal.  
    - Examples:
      - Digital audio systems
      - Multimedia playback
      - Smartphones

- **Design Characteristics**:  
  - Often implemented as a **library linked directly** with applications.  
  - **Tightly coupled** components with **little to no protection** between parts.  
  - Designed for **deterministic behavior** and **low latency**.  
  - Software is **pre-installed** by system designers (no user downloads).

- **Notable OS Example**:  
  - **eCos** (Embedded Configurable Operating System)

- **Overlap with Other Systems**:  
  - Shares features with:
    - **Embedded systems** (pre-installed software, simplified protection)  
    - **Handheld systems** (soft real-time behavior)  
  - Main difference: real-time systems are focused on **industrial use** rather than consumer devices.

## Smart Card Operating Systems

## Smart Card Operating Systems

- **Overview**:  
  - Run on **credit-card-sized devices** containing a **CPU chip**.  
  - Designed for **extremely constrained environments** in terms of processing power and memory.

- **Power Constraints**:  
  - **Contact smart cards**: powered by physical connection to a reader.  
  - **Contactless smart cards**: powered inductively, which severely limits capabilities.

- **Functionality**:  
  - Some support only a **single function** (e.g., electronic payments).  
  - Others support **multiple functions**, requiring more complex OS behavior.  
  - Many use **proprietary operating systems**.

- **Java-Oriented Smart Cards**:  
  - Contain a **Java Virtual Machine (JVM) interpreter** in ROM.  
  - Can download and run **Java applets**.  
  - Some support **multiple applets simultaneously**, enabling:
    - **Multiprogramming**  
    - **Scheduling**  
    - **Resource management**  
    - **Protection between applets**

- **OS Characteristics**:  
  - Usually **very primitive** due to hardware limitations.  
  - Must still handle key OS tasks when running multiple applets.

# Operating System Concepts

## Processes
## Processes in Operating Systems

- **Definition**:  
  - A **process** is a **program in execution**.  
  - It includes:
    - **Address space**: memory it can read/write (code, data, stack).
    - **Resources**: registers, open files, alarms, related processes, etc.

- **Process State**:  
  - A process can be **temporarily suspended** and must later resume in the **exact same state**.
  - Key elements to preserve:
    - Open file positions
    - Register contents
    - Stack state

- **Core Components**:  
  - **Core image**: the process’s address space.  
  - **Process table entry**: OS-maintained structure storing:
    - Register values
    - File descriptors
    - State info needed to restart/resume process

- **Process Management System Calls**:  
  - **Create a process** (e.g., shell spawning a compiler).  
  - **Terminate a process** when finished.  
  - **Request or release memory**.  
  - **Overlay a process** with a different program.  
  - **Wait for child processes** to finish.

- **Process Hierarchy**:  
  - Processes can create **child processes**, forming a **process tree**.  
    - Example:  
	```
	      A
	     / \
	    B   C
	   /|\
	  D E F
      ```

- **Interprocess Communication (IPC)**:  
  - Used by related processes to **communicate** and **synchronize**.  
  - Examples include messaging and signaling mechanisms.

- **Signals**:  
  - Software analogs to hardware interrupts.  
  - Triggered by events such as:
    - Timers expiring
    - Errors (e.g., illegal instruction, invalid address)
  - When received:
    - Process saves its state
    - Executes a **signal handler**
    - Resumes where it left off after handling

- **User and Group IDs**:  
  - Every user has a **UID**; every process inherits the UID from its parent.  
  - Users can belong to groups, each with a **GID**.  
  - A special UID (superuser/administrator) has **elevated privileges**.  
  - Security is important, as users may try to exploit flaws to gain superuser access.

> Note: These concepts are explored in greater detail in Chapter 2.

## Address Space

## Memory Management in Operating Systems

- **Basic Concept**:  
  - Every computer has **main memory** used to hold **executing programs**.
  - In simple systems:  
    - Only **one program** is in memory at a time.  
    - To run a second, the first must be **removed**.

- **Multiprogramming Support**:  
  - Modern OSes allow **multiple programs** to reside in memory **simultaneously**.
  - This requires **protection mechanisms** to:
    - Prevent programs from interfering with each other.
    - Protect the operating system itself.
  - These mechanisms are **hardware-based** but **controlled by the OS**.

- **Address Space Management**:  
  - Each process has an **address space** (e.g., from 0 to some max value).
  - In simple systems, the address space is **smaller than physical memory**, so the entire process fits.

- **The Virtual Memory Challenge**:  
  - Modern systems use **32- or 64-bit addresses**, yielding address spaces of:
    - `2^32` bytes for 32-bit systems.
    - `2^64` bytes for 64-bit systems.
  - **Physical memory** is often **much smaller** than the full address space.

- **Virtual Memory Solution**:  
  - The OS uses **virtual memory** to:
    - Store **part of the process** in main memory.
    - Store the **rest on disk**.
    - **Swap pieces in and out** as needed.
  - This allows:
    - Larger address spaces than physical memory.
    - **Decoupling** of address space from physical memory.

- **Key Role of the OS**:
  - The OS **creates the abstraction** of an address space.
  - It **manages both**:
    - **Physical memory**
    - **Virtual address spaces**

> This topic is covered in greater detail in **Chapter 3**.

## Files
## File Systems in Operating Systems

### Abstract View of Files
- Operating systems provide an **abstract, device-independent** view of files.
- System calls allow:
  - Creating and removing files
  - Opening and closing files
  - Reading from and writing to files

---

### Directories and Hierarchies
- **Directories** group files for organization.
- System calls exist to:
  - Create/remove directories
  - Add/remove files in directories
- Directories may contain:
  - **Files**
  - **Other directories** (supports hierarchy)
- The result is a **tree-structured file system hierarchy**.

---

### File vs. Process Hierarchies
| Aspect | Process Hierarchies | File Hierarchies |
|--------|----------------------|------------------|
| Depth  | Usually shallow (≤ 3 levels) | Often deep (4+ levels) |
| Lifespan | Short-lived (minutes) | Long-lived (years) |
| Access | Parents access children | Broader sharing via permissions |

---

### Path Names and Working Directories
- **Absolute path**: starts from the root `/`  
  Example: `/Faculty/Prof.Brown/Courses/CS101`
- **Relative path**: starts from current working directory  
  Example: `Courses/CS101` (if current directory is `/Faculty/Prof.Brown`)
- System calls allow changing the **current working directory**.

---

### File Access
- A file must be **opened** before use:
  - OS checks **permissions**.
  - If allowed, returns a **file descriptor** (small integer).
  - If denied, returns an error.
- File descriptors are used in later reads/writes.

---

### Mounted File Systems (UNIX Concept)
- Allows **external media** (CDs, USBs) to be attached to the main file tree.
- Example:
  - Before mounting: CD-ROM and main tree are **unrelated**.
  - After mounting: CD-ROM appears under a directory (e.g., `/b/`).
  - **Note**: Preexisting files in `/b/` become **inaccessible** while mounted.
- Multiple hard disks can be mounted into **one unified tree**.

---

### Special Files
- Special files make **devices look like files**, enabling unified I/O.
- Stored in `/dev` directory.
- Two types:
  - **Block special files**: for block devices (e.g., disks)
  - **Character special files**: for stream devices (e.g., printers, modems)

---

### Pipes
- A **pipe** connects two processes.
- Behaves like a **pseudofile**:
  - Process A writes to the pipe.
  - Process B reads from the pipe.
- From the processes’ perspective, using pipes looks like normal file I/O.
- Pipes facilitate **interprocess communication** in UNIX.

---

> File systems play a critical role in OS functionality. More will be covered in **Chapters 4, 10, and 11**.


## Input/Output (I/O) Devices and Subsystem

- **Purpose of I/O Devices**:
  - All computers use physical devices for **input** (e.g., keyboards) and **output** (e.g., monitors, printers).
  - These devices allow users to **interact with the computer** — giving it instructions and receiving results.

- **Role of the Operating System**:
  - The OS is responsible for **managing I/O devices**.
  - This management is handled by a dedicated component called the **I/O subsystem**.

- **Types of I/O Software**:
  - **Device-Independent Software**:
    - Works with **many or all I/O devices** in a generic way.
  - **Device Drivers**:
    - Handle **specific hardware**.
    - Provide the OS with a way to communicate with individual I/O devices.

- **Further Reading**:
  - More on **I/O software** will be covered in **Chapter 5**.


## 1.5.5 Protection

### Purpose of Protection
- Computers store **sensitive and confidential information**:
  - Examples: email, business plans, tax returns.
- The **operating system (OS)** is responsible for managing **system security**.
  - Ensures files and resources are **accessible only to authorized users**.

### Example: File Protection in UNIX
- UNIX uses a **9-bit binary protection code** for each file:
  - Divided into **three 3-bit fields**:
    1. **Owner**
    2. **Group members** (users grouped by admin)
    3. **Everyone else**
- Each 3-bit field includes:
  - **r** – read permission
  - **w** – write permission
  - **x** – execute/search permission
- These are known as the **rwx bits**.
- Example:
  - `rwxr-x--x`
    - Owner: read, write, execute
    - Group: read, execute (no write)
    - Others: execute only

> Note: For **directories**, the `x` bit means **search permission**.

### Broader Security Concerns
- Protection goes beyond files:
  - Includes defense against **intruders** (human and nonhuman like viruses).
- Further details will be covered in **Chapter 9**.

## 1.5.6 The Shell

### What Is the Shell?
- A **command interpreter** that acts as the main interface between the user and the operating system (when no GUI is used).
- Not technically part of the OS, but:
  - Uses many **system calls**.
  - Demonstrates **how users interact with the OS**.

### Common UNIX Shells
- Examples: `sh`, `csh`, `ksh`, `bash`.
- All provide similar core functionality based on the original `sh`.

### How the Shell Works
- When a user logs in, the shell:
  - Starts automatically.
  - Uses the terminal for **standard input** and **standard output**.
- The shell displays a **prompt** (e.g., `$`) to wait for user input.
- Example:
	```bash
	  date
	```
	
Shell creates a child process to run the date program.
Waits for the process to finish.
Displays the prompt again.

Input and Output Redirection
Redirect standard output to a file:

```bash
date > file
```
Redirect standard input and output:
```bash
sort < file1 > file2
```
Using Pipes
Send the output of one command into another:

```bash
cat file1 file2 file3 | sort > /dev/lp
```
cat merges the files.
sort arranges lines alphabetically.
Output goes to /dev/lp (printer).

Background Processes
Add & to run a command in the background:

```bash
cat file1 file2 file3 | sort > /dev/lp &
```
Shell does not wait for completion.

Prompt appears immediately; user can continue working.

Shell vs GUI
Most modern systems use a Graphical User Interface (GUI).
A GUI is just another program running on top of the OS, like a shell.
Linux:
Common GUIs: Gnome, KDE, or none (via terminal/X11).

Windows:
The GUI (Windows Explorer) can technically be replaced by changing registry settings, though rarely done.
📚 Additional Resources:
Kernighan & Pike (1984)
Quigley (2004)
Robbins (2005)

## 1.5.7 Ontogeny Recapitulates Phylogeny

### Analogy from Biology
- **Ernst Haeckel's idea**: Ontogeny (embryo development) repeats phylogeny (evolution of the species).
- Though oversimplified in biology, this concept loosely applies to **computer evolution**.
- Each new computing platform (mainframes, PCs, embedded systems) **repeats the development path** of its predecessors in both hardware and software.

### Technology Drives Evolution
- **Historical limitations** (e.g., no cars in Roman times) stem from a lack of technology, not lack of demand.
- Computers became widespread **not because of desire**, but because they became **cheap and practical**.

### Technological Pendulum
- **Concepts become obsolete** as technology changes, then **reappear** when context shifts again.
- **Example: Caches**
  - Became important when CPUs outpaced memory.
  - Could become obsolete if memory surpasses CPU speed.
  - May re-emerge if the CPU regains the speed edge.

---

## Historical Examples of Recurring Concepts

#### Example 1: Direct Execution vs. Interpretation
- **Early Computers**: Hardwired instruction sets.
- **IBM 360**: Introduced **microprogramming** (interpreted execution).
- **RISC Era**: Favored direct hardware execution.
- **Modern Era**: Interpreted Java bytecode (e.g., applets) becomes common again due to network delay tolerances.

---

### Hardware Evolution and Its Effects on Software

### A. Large Memories
- **Mainframes (IBM 7090/7094)**: ~128 KB memory, required **assembly language**.
- **Minicomputers (PDP-1)**: Small memory led to **resurgence of assembly**.
- **Microcomputers (1980s)**: 4 KB memory → assembly again.
- **Modern PCs**: Large memory → high-level languages (C, Java).
- **Smart Cards**: Similar path, now using Java interpreters.

### B. Protection Hardware
- **Early Mainframes**: No protection; ran one program at a time.
- **IBM 360**: Introduced basic protection → allowed **multiprogramming**.
- **Minicomputers (PDP-1/PDP-8)**: No protection initially, later added (PDP-11).
- **Microcomputers (8080)**: No protection → monoprogramming.
- **Intel 80286**: Introduced protection; enabled multiprogramming.
- **Embedded Systems**: Many still lack protection, run one program at a time.

### Operating System Evolution
- Followed hardware capabilities:
  - Initially **manual, single-program systems**.
  - Progressed to **multiprogramming** and **timesharing**.
  - Each generation (mainframes → micros → handhelds) repeated this pattern.

---

## Disks and File Systems

### Storage Evolution
- **Early Mainframes**:
  - Tape-based systems, no file systems.
  - IBM RAMAC (1956): First hard disk, extremely limited and expensive.
- **CDC 6600 (1964)**: Added **single-level directories**.
- **Mainframes**: Eventually developed **hierarchical file systems** (e.g., MULTICS).

### On Other Platforms
- **Minicomputers (PDP-11)**: Used RK05 disks (~2.5 MB), began with single-level directories.
- **Microcomputers (CP/M)**: Supported only one directory on floppy disks.

---

### Virtual Memory

- Allows programs **larger than physical RAM** by swapping data with disk.
- First appeared in **mainframes**, later moved to **minis** and **micros**.
- Enables **dynamic linking** of libraries at runtime.
- First fully realized in **MULTICS**.
- Now standard in most **UNIX and Windows** systems.

---

#### Summary: The Reincarnation of Ideas

- Many "obsolete" concepts (assembly, monoprogramming, simple file systems) are **context-dependent** and often **resurface**:
  - Example: Smart cards may reintroduce "old" ideas due to hardware limits.
- **Important takeaway**: Understand *why* a concept became obsolete and *what changes* could bring it back.

# 1.6 System Calls - Notes

- Operating systems have two main functions: providing abstractions to programs and managing resources.
- Interaction between user programs and the OS mostly deals with abstractions like files and processes.
- System calls are the interface through which user programs access OS services.

- System calls differ slightly between operating systems, but concepts are similar.
- This discussion focuses on POSIX systems (UNIX, Linux, BSD, MINIX 3, etc.).

- On most systems, C library procedures are used to make system calls.
- Actual system call requires switching from user mode to kernel mode.
- This is done using a special instruction called a trap.

- A trap is like a procedure call that also switches the CPU into kernel mode.
- The OS inspects parameters, executes the requested service, then returns control back.

- Example: the read system call has three parameters:
  - file descriptor (which file to read)
  - buffer (where to store data)
  - number of bytes to read

- The library procedure places the system call number in a register.
- It then executes a trap instruction to switch to kernel mode.
- The kernel uses the call number to find the correct handler.
- The handler executes and returns to the library procedure, which returns to the user program.
- After the call, the user program adjusts the stack and continues.

- If a system call can't complete (e.g., no input available), the process may be blocked.
- The OS will schedule another process until the original one can continue.

- POSIX has about 100 standard procedures for common tasks:
  - Process creation and termination
  - File reading and writing
  - Directory operations
  - Input and output

- Not all POSIX procedures map 1-to-1 with system calls.
- If a procedure can be done in user space, it usually is, for performance.
- Some system calls support multiple similar library procedures.

## 1.6.1 System Calls for Process Management

- **fork()** is the only way to create a new process in POSIX.
  - It creates a duplicate of the parent process: registers, file descriptors, etc.
  - Both parent and child continue execution from the same point.
  - Return value: 0 in child, child PID in parent.

- After fork, parent and child may execute different code.
  - Common in shells: parent waits, child executes the command.
  - Parent uses **waitpid()** to wait for a specific (or any) child to finish.
    - statloc stores the child’s exit status.
    - Options allow behavior like non-blocking return.

- **execve()** replaces the current process image with a new program.
  - Used by the child process to run a command (e.g., cp file1 file2).
  - Parameters:
    - program name (file path)
    - argv (array of argument strings)
    - environp (array of environment variables)
  - Other variants (execl, execv, etc.) simplify how parameters are passed.

- Shell example usage:
  - Display prompt.
  - Read command.
  - Call fork().
    - Parent: waitpid().
    - Child: execve() with command and arguments.

- Arguments in `main()`:
  - `argc`: number of command-line arguments.
  - `argv[]`: array of argument strings.
    - e.g., for `cp file1 file2`: argv[0]="cp", argv[1]="file1", argv[2]="file2"
  - `envp[]`: array of environment variable strings (name=value).

- Environment variables are often used for user preferences (e.g., default printer, terminal type).

- **exit(status)** terminates a process and returns status (0–255) to parent via waitpid().

- UNIX memory layout:
  - **Text segment**: program code (read-only, shared between parent and child).
  - **Data segment**: program variables (grows upward).
  - **Stack segment**: function call stack (grows downward).
  - **Gap** between stack and data used for dynamic memory.

- **brk()** is used to expand the data segment but is not POSIX-standard.
  - POSIX prefers using **malloc()** for dynamic memory allocation.
  - `brk()` is low-level and rarely used directly by programmers.

## 1.6.2 System Calls for File Management

- File system-related system calls allow programs to open, read, write, and manipulate files.

- **open()**
  - Used to open a file before accessing it.
  - Parameters:
    - File name (absolute or relative path).
    - Access mode: `O_RDONLY`, `O_WRONLY`, or `O_RDWR`.
    - Optional flags like `O_CREAT` (to create a new file if it doesn’t exist).
  - Returns a **file descriptor** used in subsequent read/write operations.

- **close()**
  - Closes an open file and frees its file descriptor for reuse.

- **read()** and **write()**
  - Primary system calls for file input/output.
  - Parameters:
    - File descriptor.
    - Buffer (to read into or write from).
    - Number of bytes to read/write.
  - Used for sequential access by default.

- **lseek()**
  - Allows **random access** within a file (not just sequential).
  - Parameters:
    1. File descriptor.
    2. Offset (in bytes).
    3. Reference point:
       - `SEEK_SET`: from beginning of file.
       - `SEEK_CUR`: from current position.
       - `SEEK_END`: from end of file.
  - Returns new absolute file position.

- Every open file has a **position pointer**, updated automatically on read/write.
  - `lseek()` lets you manually change it.

- **stat()**
  - Retrieves metadata about a file:
    - File type (regular, directory, special, etc.)
    - File size.
    - Last modification time.
    - Permissions and more.
  - Parameters:
    1. File name.
    2. Pointer to a structure where the info will be stored.

- **fstat()**
  - Like `stat()`, but works on an already open file (using its file descriptor).

## 1.6.3 System Calls for Directory Management

- These system calls operate on directories or the file system as a whole, not just individual files.

### Directory Manipulation

- **mkdir()**
  - Creates a new, empty directory.
  - Requires the directory name and permission mode.

- **rmdir()**
  - Removes an empty directory.

### File Linking

- **link()**
  - Creates a **hard link**: a new directory entry for an existing file.
  - Both names refer to the **same file** and **share the same i-node**.
  - Changes via one name are visible from the other.
  - Example:  
    ```c
    link("/usr/jim/memo", "/usr/ast/note");
    ```
    - This makes both paths refer to the same file.
  - If one link is removed (with `unlink()`), the other remains.
  - When the last link is removed, the file is deleted from disk.

### Directories as (i-number, name) Pairs

- Each file has an **i-number**, which indexes into a table of i-nodes.
- A directory is a file containing a list of `(i-number, ASCII name)` pairs.
- **link()** adds a new name pointing to an existing i-number.
- UNIX tracks the number of directory entries pointing to each file in the i-node.

### Mounting File Systems

- **mount()**
  - Integrates one file system into another by attaching it to a mount point.
  - Example:
    ```c
    mount("/dev/sdb0", "/mnt", 0);
    ```
    - Mounts USB drive `/dev/sdb0` at `/mnt`.

- After mounting:
  - Files on the new device can be accessed using normal paths.
  - Integrates removable or external media (USB, CD-ROM, etc.) into the file tree.
  - Supports mounting of partitions, USB sticks, and more.

- **umount()**
  - Detaches a mounted file system when it is no longer needed.

## 1.6.4 Miscellaneous System Calls

### chdir

- Changes the current working directory.
- After calling chdir with a directory path, all relative file paths are resolved based on that new working directory.
- This reduces the need to type long absolute paths repeatedly.

### chmod

- Changes the mode (permissions) of a file.
- The mode includes read, write, and execute bits for the owner, group, and others.
- For example, setting the mode to 0644 makes the file readable and writable by the owner, and readable by everyone else.

### kill

- Sends signals to processes.
- If the process has a handler for the signal, it runs that handler when the signal is received.
- If the process does not handle the signal, it is terminated.
- Commonly used to control or stop processes.

### time

- Returns the current system time in seconds since January 1, 1970 (the UNIX epoch).
- On 32-bit systems, time is typically stored as an unsigned 32-bit integer.
- This limits the time to a maximum of 2^32 - 1 seconds (~136 years), which means it will overflow in the year 2106.
- Similar to the Y2K issue, this is a potential problem for legacy systems.
- Recommendation: switch to 64-bit systems before 2106.


# Operating System Structure
