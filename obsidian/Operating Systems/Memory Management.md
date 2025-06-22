As memory increases, memory requirements of programs increase. There is never enough memory.

Computers today have a lot more memory than those of the past

Address Spaces
No Memory Abstraction:
Single Process with operating system with no virtual memory:
1. OS runs in memory from base 0 up to some point, and the user program runs on top of that
2. The user program runs from base 0 and there is a fixed point above the user program the OS runs
3. Low level at base 0: OS, user program above, device drivers in ROM

Multi programs running at the same time:
With two programs with a jump location, offsets can be wrong and lead to one program accessing the memory of another
Base Register: Where the lowest point in memory it starts
Limit Registers: The extent in which that program can reach. Program can not point to anything beyond.
Jump instructions can be added to base registers to point to real address

Swapping in and out programs, each of different size
There are multiple processes that are running in the background, not even accounting for the programs you use

Bitmaps: shows for each chunk (maybe a word or page) whether there is something in there.
Linked List: Easier to manage than a bitmap. As programs start, there are algorithms to decide where to fit a program in the list
There are also needs to manipulate the linked list
Entries may need to be merged, split, etc

5 Algorithms for memory management, with advantages and disadvantage
First Fit: Take the first empty space hwere program fits
Next Fit: Remembers where we searched from last time, and start there next time the program runs
Best Fit: Searches through whole list and tries to find the smallest space where program can fits. 
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

