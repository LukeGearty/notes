## What is an Operating System?

Components of a Modern Computer:
1. One or more processors
2. Main memory
3. Disks
4. Printers
5. Keyboard
6. Mouse 
7. Display
8. Network Interfaces
9. I/O Devices
Where does the Operating System fit in?

From a top down perspective:
Web Browser, E-mail Reader, Music Player - different programs
User Interface Program - what the user interacts with - either CLI or GUI
Operating System
Hardware - the physical components of the computer

When designing software, you don't care about users' hardware like printer. You want to abstract that away and just deal with the interface. The programmer will use the system calls to deal with the hardware.

Resource Manager:
Top Down View: Provide abstraction to application programs
Bottom Up View: Manage pieces of complex system
Alternative View: provide orderly, controlled allocation of resources


## History of Operating Systems
They've been around a long time 

The First Generation (1945 -55)
Vacuum Tubes - [Looks](https://www.google.com/search?sca_esv=483d1e70275a4045&sxsrf=AHTn8zrnUZ7DKPioEQeyPif2rHkyYmUF5A:1747673055188&q=vacuum+tube+computer&udm=2&fbs=ABzOT_CWdhQLP1FcmU5B0fn3xuWpA-dk4wpBWOGsoR7DG5zJBsxayPSIAqObp_AgjkUGqel3rTRMIJGV_ECIUB00muput9Zp8VMKUi0ZjqPs3JlrgPeFrAnFlUitTiL3WcJlFn10ZVAeuxL5fSn-ULNu9lz3DIW3cy7rkKNmgHapdAFAoBFSl5-LQE_swXRSgVvZGy87KiusPiw1DSGvVAMCLf6f2K4DEg&sa=X&sqi=2&ved=2ahUKEwjqjY29_a-NAxW0F1kFHSBtKrgQtKgLegQIFhAB&biw=1662&bih=812&dpr=1#vhid=TVc6lbQnuB56-M&vssid=mosaic])

The Second Generation (1955 - 1965)
Transistors and Batch Systems

Third Generation (1965 - 1980)
ICs and Multiprogramming (more than one process running at the same time)

The Fourth Generation (1980 - Present)
Personal Computers 

The Fifth Generation (1990 - Present)
Mobil Computers


## Hardware Overview

All data goes down the bus and the processor communicates with the components using this bus
There is a 3-stage pipeline:
Fetch - get the instruction
Decode - figure out what the instruction means
Execute - execute the instruction

Execution has different amount of time. Adding two registers can be instantaneous. Moving memory can take a long time depending on where it is - from disk is really long. 

A superscalar CPU will have multiple fetch units and decode units, a holding buffer for all the decoded instructions, and multiple execution units.


Memory - volatile memory - when you pull the plug (shut the computer off) the content of the memory goes away
Moving data from Main memory takes time.
There is memory in the cpu. Called L1 Cache - it is really fast but small.
L2 cache is bigger than L1, close to the CPU, but takes a little bit more time to access. An L2 cache can be in the cores of the CPU or it can be shared across the cores.
Memory hierarchy:
Access Time - memory component - typical capacity 
1 nsec - Register - < 1KB
2 nsec - Cache - 4 MB
10 nsec - Main Memory - 1-8 GB
10 msec - Magnetic Disk - 1-4 TB

Magnetic Disk - spinning head, magnetized parts

Caching System Issues/Considerations: 
1. When to put a new item into the cache
	1. Think about leaving the house - what do you need?
	2. Things you'll need and use often
2. Which cache line to put the new item in
3. Which item to remove from the cache when a slot is needed
4. Where to put a newly evicted item in the larger memory

Magnetic Disks
Multiple cylinders and there is a head for each - the head needs to move and the disk needs to move - it takes time

I/O Devices

Buses
x86 - used in UNIX and macintosh machines


## Operating System Zoo

Mainframe Operating Systems
Server Operating Systems - Windows, UNIX, in the Cloud, care less about user interface and user experiences and more about making sure services are scheduled appropriately
Multiprocessor Operating Systems - Figure out how to use multiple processors better
	Moore's Law - every 18 months - 2 years, the # of transistors is doubled
		Doesn't happen anymore - we get more cores
Personal Computer Operating Systems - why do we use our computers? Write, read, watch movies, etc - think of all the dfiferent use cases and this OS tries to accommodate 
Handheld Computer Operating Systems
Embedded Operating Systems - for devices embedded within - think of IoTs
Sensor Node Operating Systems - There is a sensor, it has a specific job that it does
Real-Time Operating Systems - Deal with data in real-time - think of streaming data - think of something like a heart monitor - it needs to always be monitoring
Smart Card Operating Systems 

Processes: a key concept in all OSs. It is a program in execution. There might be two intances of the same process - think of multiple tabs of google chrome. It is associated with an address space, a set of resources. 
Think of a process as a container - it holds everything needed to run a program.

A process tree: 
Process A spins up different processes to form a tree. A spins up B and C, B spins up D, E, F. A is the parent of B and C, B is the parent of D, E, F.

FIle Systems:
There is a hierarchy. Root directory, and there are directories within. The leaves are the files.
Some file systems get mounted periodically - CD-ROM is a file system that isn't always there. After they are mounted, it becomes a part of the hierarchy.
If we have two processes, they sometiems need to talk to each other - there are Pipes where processes can read and write across each other.

Ontogeny Recapitulates Phylogeny:
Each new "species" of computer goes through the same development as ancestors
Consequences of impermanence - changes in technology may bring back "obsolete" concepts
	See it happen with memory - we started out with limited memory (16 KB of volatile memory in the past) - programming was saving memory. Today, we can have 8 GB memory.
		Sensor Operatng Systems need to use less memory.

## System Calls
The read call:
takes in file descriptor (a resource pointer to a specific file), a buffer, and the number of bytes to read.

POSIX System Calls
pid=fork(): Create a child process identical to the parent 
pid = waitpid(pid, &statloc, options): Wait for a child to terminate

a = execve(name, argv, environp): replace a process' core image
exit(status): Terminate process execution and return status

File management:
fd = open(file, how, ...): opens a file for rrading, writing, or both
s = close(fd): Close an open file
n = read(fd, buffer, nbytes): read data from a file into a buffer
n = write(fd, buffer, nbytes): write data from a buffer into a file
position = lseek(fd, offset, whence): move the file pointer

The return code is -1 if an error has occured.

## Operating System Structure
### Monolithic Systems
Basic Structure: A main program invokes the requested service procedure
A Set of service procedures that carry out the system calls
A set of utility procedures that help the service procedures
Think of this like a tree 

### Layered Systems
Dijkstra (of Dijkstra's Algorithm) THE - introduced a layer operating system
Makes it easier to modify software
5 - The operator
4 - user programs
3 - input/output management
2 - operator-process communication
1 -memory and drum management
0 processor allocation and multiprogramming

Each layer can only talk to the layer right below

### Microkernels
Microkernels handle interrupts, processes, scheduling, interprocess communication
on top there are drivers, then servers, then user programs

### Client-Server Model
Multiple machines