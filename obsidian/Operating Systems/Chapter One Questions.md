1. What are the two main functions of an operating system?
	   The first function is as an interface to the user to abstract away the hardware
	   The second is to manage the resources of the computer
2. In section 1.4, nine different types of operating systems are described. Give a list of applications for each of these systems.
	1. Mainframe Operating Systems: high end Web servers, servers for large-scale electronic commerce sites, b2b transactions
	2. Server Operating Systems: any kind of server - web server, mail server, 
	3. Multiprocessor Operating Systems: Anything that requires a lot of processing power and multithreaded - game engines
	4. Personal Computer Operating Systems: Windows 10, MacOS
	5. Handheld Computer Operating Systems: ios
	6. Embedded Operating Systems: A car's operating system
	7. Sensor-Node Operating System: An operating system for a device that monitors temperatures
	8. Real-Time Operating Systems: An operating system for a heart monitor
	9. Smart Card Operating System: The operating system for a security system that requires a card to enter
3. What is the difference between timesharing and multiprogramming systems?
   Multiprogramming allows multiple applications to run at the same time. Timesharing refers to a system being used by multiple users.
4. To use cache memory, main memory is divided into cache lines, typically 32 or 64 bytes long. An entire cache line is cached at once. What is the advantage of caching an entire line instead of a single byte or word at a time?
   Since cache space is limited, the process running will want all the needed data nearby. Caching an entire line allows all the needed data to be nearby to improve performance by reducing the need to go to main memory.
5. On early computers, every byte of data read or written was handled by the CPU (i.e., there was no DMA). What implications does this have for multiprogramming?
   Multiprogramming was pretty much impossible on early computers since the CPU is handling everything - it was never free. 
6. Instructions related to accessing I/O devices are typically privileged instructions, that is, they can be executed in kernel mode but not in user mode. Give a reason why these instructions are privileged. 
   Since the I/O devices interact directly with the operating system and the hardware, the instructions must be executed in kernel mode to restrict any potential damage and to manage the system safely. 
7. The family of computers idea was introduced in the 1960s with the IBM System/360 mainframes. Is this idea now dead as a doornail or does it live on?
   Every major OS has a family of computers - Windows won't be able to operate on a Macbook hardware without some major changes
8. One reason GUIs were initially slow to be adopted was the cost of the hardware needed to support them. How much video RAM is needed to support a 25-line x 80-row character monochrome text screen? How much for a 1200 x 900-pixel 24-bit color bit-map? What was the cost of this RAM at 1980 prices($5/KB)? How much is it now?
   Each character in a monochrome ASCII text represents 1 byte, no color means no extra bit info.
   25 x 80 = 2000 bytes = 2 KB
   2 KB x 5 = $10 for text screen
   24 bits = 3 bytes
   1200 x 900 - 1080000 pixels
   1,080,000 x 3 = 3240000 bytes = 3240 KB
   3240 x 5 = 16200 for Bitmap screen
9. There are several design goals in building an operating system, for example, resource utilization, timeliness, robustness, and so on. Give an example of two design goals that may contradict one another. 
   Fairness and real time - a real time process is going to get more attention and resources than other non-real time processes.
10. What is the difference between kernel mode and user mode? Explain how having two distinct modes aids in designing an operating system.
    kernel mode: complete access to hardware and can execute any instruction the machine is capable of executing
    User Mode: Only a subset of the machine's instructions are available.
    Having two distinct modes ensures that applications can run safely without damaging the machine (user mode) and the operating system can effectively manage the resources of the computer (kernel mode)
11. A 255-GB disk has 65,546 cylinders with 255 sectors per track and 512 bytes per sector. How many platters and heads does this disk have? Assuming an average cylinder seek time of 11 ms, average rotational delay of 7 msec and reading rate of 100 MB/sec, calculate the average time it will take to read 400 KB from one sector.
12. Which of the following instructions should be allowed only ink ernel mode?
    a. Disable all interrupts - kernel
    b. Read the time-of-day clock
    c. Set the time-of-day clock - kernel mode
    d. Change the memory map - kernel mode
13. Consider a system that has two CPUs, each CPU having two threads (hyperthreading). Suppose three programs, P0, P1, and P2, are started with run times of 5, 10 and 20 msec respectively. How long will it take to complete the execution of these programs? Assume that all three programs are 100% CPU bound, do not block during execution, and do not change CPUs once assigned?
    Either 20, 25, or 30 depending on which CPU gets which program. The total time will be the CPU with the longes time to finish. Best case scenario, one CPU gets 5 and 10 while the other gets 20 (15 msec for the first, 20 msec for the second). Worst case, one CPU gets 5 and the other gets 10 and 20.
14. A computer has a pipeline with four stages. Each stage takes the same time to do its work, namely 1 nsec. How many instructions per second can this machine execute?
    1 billion instructions per second. 1 nsec = 1 billionth of a second. 
15. Consider a computer system that has cache memory, main memory (RAM) and disk, and an operating system that uses virtual memory. It takes 1 nsec to access a word from the cache, 10 nsec to access a word from the RAM, and 10 ms to access a word from the disk. If the cache hit rate is 95% and main memory hit rate (after a cache miss) is 99%, what is the average time to access a word?
    95% of the time, it takes 1 nsec: 0.95 x (1 x 10^-9)
	5% of the time:
		99% of the time, it will take 10 nsec: (0.05 x 0.99 x (10 x 10^-9))
		1% of the time, it will take 10 ms: (0.05 x 0.01 x (10 x 10^-3))
		0.95 x (1 x 10^-9) + (0.05 x 0.99 x (10 x 10^-9)) + (0.05 x 0.01 x (10 x 10^-3))
		x 10^9 = 5001.445 nsec = 5.0014 seconds
16. When a user program makes a system call to read or write a disk file, it provides an indication of which file it wants, a pointer to the data buffer, and the count. Control is then transferred to the operating system, which calls the appropriate driver. Suppose that the driver starts the disk and terminates until an interrupt occurs. In the case of reading from the disk, obviously the caller will have to be blocked (because there are no data for it). What about the case of writing to the disk? Need the caller be blocked awaiting completion of the disk transfer?
    A user program issues a system call to write to a file
    The OS takes control and invokes the driver
    The driver initiates the operation and terminates until an interrupt occurs.
    If reading from the disk:
	    The caller will have to be blocked (no data for it)
	If writing to the disk:
		maybe. If the caller gets control back and immediately overwrites the data, the wrong data will be written. If the driver first copies the data to a private buffer before returning, the caller can be allowed to continue immediately. 
		Also, the callr may be allowed to continue and given a signal when the buffer may be reused. This is tricky
17. What is a trap instruction? Explain its use in operating systems.
    A special instruction that causes the processor to switch from user mode to kernel mode - triggers a system call
18. Why is the process table needed in a timesharing system? Is it also needed in personal computer systems running UNIX or Windows with a single user?
    The process table keeps track of processes and allocate resources. It is used in UNIX or Windows with a single user, but not really needed.
19. Is there any reason why you might want to mount a file system on a nonempty directory? If so, what is it?
    If you want to overwrite the stuff in the nonempty directory
20. For each of the following system calls, give a condition that causes it to fail: 
    fork: there is a limit on the total number of processes and the fork will cause that limit to exceed
    exec: Execute permissions are wrong
    unlink: file pathname cannot be unlinked because it is being used by the system of an other process
21. What type of multiplexing (time, space, or both) can be used for sharing the following resources: CPU, memory, disk, network card, printer, keyboard, and display?
    CPU: Time multiplexing
    Memory: Space
    Disk: Space
    Network Card: Time
    Printer: Time
    Keyboard: Time
    Display: Time
22. 13. Can the  count = write(fd, buffer, nbytes); call return any value in count other than nbytes? If so, why?
    It can return -1 if there is an error. It can also return fewer than nbytes if there was insufficient space on the disk to write all requested bytes, or if it was blocked.
23. A file whose file descriptor is fd contains the following sequence of bytes: 3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5. The following system calls are made: 
    lseek(fd, 3, SEEK SET); 
    read(fd, &buffer, 4);
    where the lseek call makes a seek to byte 3 of the file. What does buffer contain after the read has completed?
24. Suppose that a 10-MB file is stored on a disk on the same track (track 50) in consecutive sectors. The disk arm is currently situated over track number 100. How long will it take to retrieve this file from the disk? Assume that it takes about 1 ms to move the arm from one cylinder to the next and about 5 ms for the sector where the beginning of the file is stored to rotate under the head. Also, assume that reading occurs at a rate of 200 MB/s.
    50 ms to move arm from track 100 to track 50
    5 ms for first sector to rate
    10/200 seconds to read the 10 MB = 50 ms
    105 ms
25. What is the essential difference between a block special file and a character special file?
    