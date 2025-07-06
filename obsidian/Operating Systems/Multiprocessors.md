Past generations got more transistors on CPU. 
This gen: more opportunities for connectivity, more processors

Multiple Processor Systems:
More CPUs make things more complicated.
The challenge of this gen is getting more processors.
1. Shared memory for the CPUs
2. Each CPU has its own internal memory
3. CPU and memory that are connected - distributed systems

Shared Memory: 
with no caching, CPUs talk to memory down a bus
Local cache to each CPU with shared memory
Private memory for each CPU and a shared memory of each cache

UMA Multiprocessors using crossbar switches (Uniform Memory Access)

Multistage switching

Nonuniform Memory Access (NUMA
A Single addres space visible to all CPUs
Access to remote memory is via LOAD and STORE
Access to remote memory is slower than access to local memory