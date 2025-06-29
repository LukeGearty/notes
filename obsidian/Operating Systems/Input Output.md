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

Goals of I/O Software
1. Device Independence - support any deivce
2. Uniform Naming Convention - don't need to know a lot about it
3. error handling
4. Synchronous (piece of information asked and immediately returned) versus asynchronous (ask for info and get it later through interrupt)
5. Buffering 

Device-Independent I/O Software
A uniform interfacing for device drivers - a driver is a software program that enables communication between an operating system and a hardware device
Buffering: We're not dealing with one char at a time, we don't lose data
Error Reporting
Allocating and releasing dedicated devices - one user at a time
Providing a device independent block size


Uniform Interfacing for Device Drivers
All different drivers (SATA, USB, SCSI) - the I/O Software to have the same interface

Buffering: 4 ways to buffer
1. No buffer
2. Buffering in user space
3. Buffering in kernel space - kernel is talking to I/O, so this is faster
4. Double buffering in the kernel - to transfer in blocks, redundant needs of data


Magnetic Disks:
For most of PC computing history, every 18 months the number of transistors was doubled (not the case now)
Disks followed a similar pattern
Number of cylinders, tracks per cylinder, other parameters have increased dramatically

RAID - Redundant Array of Independent Disks
**RAID 0.** This configuration has striping but no data redundancy. It offers the best performance, but it does not provide fault tolerance.
**RAID 1.** Also known as _disk mirroring_, this configuration consists of at least two drives that duplicate the storage of data. There is no striping. Read performance is improved since either disk can be read at the same time. Write performance is the same as that of single-disk storage. RAID 1 is an effective tool for disaster recovery.
**RAID 2.** This configuration uses striping across drives, with some drives storing error-correction code (ECC) information. RAID 2 also uses dedicated [Hamming code](https://www.techtarget.com/whatis/definition/Hamming-code) parity, a linear form of ECC. RAID 2 has no advantage over RAID 3 and is no longer used.
**RAID 3.** [RAID 3](https://www.techtarget.com/searchstorage/definition/RAID-3-redundant-array-of-independent-disks) uses striping and dedicates one drive to store [parity](https://www.techtarget.com/searchstorage/definition/parity) information. The embedded ECC information is used to detect errors. Data recovery is accomplished by calculating the exclusive information recorded on the other drives. Because an I/O operation addresses all the drives at the same time, RAID 3 cannot overlap I/O. For this reason, RAID 3 is best for single-user systems with long-record applications.
**RAID 4.** [RAID 4](https://www.techtarget.com/searchstorage/definition/RAID-4-redundant-array-of-independent-disks) uses large stripes, which means a user can read records from any single drive. Overlapped I/O can then be used for read operations. Because all write operations are required to update the parity disk drive, no I/O overlapping is possible.
**RAID 5.** [RAID 5](https://www.techtarget.com/searchstorage/definition/RAID-5-redundant-array-of-independent-disks) is based on parity block-level striping. The parity information is striped across each drive, enabling the array to function, even if one drive were to fail. The array's architecture enables read/write operations to span multiple drives. This results in performance better than that of a single drive but not as high as a RAID 0 array. RAID 5 requires at least three drives, but it is often recommended to use at least five for performance reasons.
**RAID 6.** [RAID 6](https://www.techtarget.com/searchstorage/definition/RAID-6-redundant-array-of-independent-disks) is similar to RAID 5, but it includes a second parity scheme distributed across the drives in the array. The use of additional parity enables the array to continue functioning, even if two drives fail simultaneously. However, this extra protection comes at a cost. RAID 6 arrays often have slower write performance than RAID 5 arrays.
**RAID 10 (RAID 1+0).** [RAID 10](https://www.techtarget.com/searchstorage/definition/RAID-10-redundant-array-of-independent-disks), which combines [RAID 1 and RAID 0](https://www.techtarget.com/searchdatabackup/tip/RAID-1-vs-RAID-0-Which-level-is-best-for-data-protection), offers higher performance than RAID 1 but at a much higher cost. In RAID 10, the data is mirrored, and the mirrors are striped.


Disk Arm Scheduling Algorithms

**Factors of a disk block read/write:**

1. Seek time (the time to move the arm to the proper cylinder).
    
2. Rotational delay (how long for the proper sector to come under the head).
    
3. Actual data transfer time.
Shortest Seek First


Clock Hardware
Neither block nor character device

Duties:
Maintaining time of day
preventing processes from running longer than allowed

