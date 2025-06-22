Block Devices - transfer info in fixed size
character - delivers streams of characters

I / O Devices
Block devices store info in fixed sizes of blocks and transfer entire blocks at a time
Character devices stream one character at a time, non addressable

Examples:
Keyboards - 10 bytes sec, slow
Mouse - 100 bytres sec, a little faster

Digital Camcorder - 3.5 Mb/sec
Blue-ray disc - both input and output, 18 Mb/sec

There are others that are faster, these are just some examples

Getting data from I/O devices to memory:
1. Separate volatile memory, address space, with I/O ports specific to those devices
2. Mapped directly into memory
3. Hybrid approach

Special bus connects CPU with Memory with I/O

Direct Memory Access: takes some of the burden off the CPU, another controller is put into computer and the CPU can pass off responsibility to DMA controller to get data from I/O and put it in memory

Interrupts: Get data from I/O devices with interrupts
	interrupt comes in and stops the CPU

Precise Interrupts: 
	1. The PC saved in a known place (program counter - stores memory address of next instruction)
	2. All instructions before that pointed to by PC have been fully executed
	3. No instructions after have been executed
	4. Execution state of instruction pointed to by PC is known

Imprecise Interrupts: back up when the memory arrives and reexecute - middle of some instruction and stop what we're doing and backup when we come back in.
	So an instruction can be stopped at any point, whereas with precise interrupts the instruction can either be fully executed or not
