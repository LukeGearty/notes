As memory increases, memory requirements of programs increase. There is never enough memory.

Computers today have a lot more memory than those of the past

Address Spaces
No Memory Abstraction:
Single Process with operating system with no virtual memory:
1. OS runs in memory from base 0 up to some point, and the user program runs on top of that
2. The user program runs from base 0 and there is a fixed point above the user program the OS runs
3. Low level at base 0: OS, user program above, device drivers in ROM

Multi programs running at the same time:
Base Register: Where the lowest point in memory it starts
Limit Registers: The extent in which that program can reach

Swapping in and out programs, each of different size
There are multiple processes that are running in the background, not even accounting for the programs you use

Algorithms:
First Fit
Next Fit
Best Fit
Worst Fit
Quick Fit


Virtual Memory
We recycle ideas in comp sci - revisit our old ideas as technology adapts
Idea of virtual memory:
we need more memory than we have - programs are too large to fit in memory
1960s: 
	Solution called overlay - a piece of the program is put on the disk (called overlay) and then load it back in - address stays the same, the content may change
Virtual Memory: each program has its own address space, broken up into chunks called pages that map to physical frames of memory
A new chip was deployed called Memory Management Unit - take in a page and translate it to a physical memory address.

Page Replacement Algorithms
Optimal Algorithm - needs a crystal ball
Not recently used algorithm
FIFO algorithm
Second Chance Algorithm
Clock Algorithm
Least Recently Used Algorithm
Working set algorithm
WSClcock Algorithm

