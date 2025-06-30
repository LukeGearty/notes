Resources
Preemptable: if someone has that resource, we can take it away (we means OS)
Nonpreemptable - no one can take it away

Sequence of events to use the resource:
request resource
use resource
Release resource

Potential Deadlocks: we say potential because it may not be known to user if a deadlock is occurring


Intro to Deadlocks
Deadlock: needs more than one process for a deadlock
A set of processes is deadlocked if each process in the set waiting ofr an event and that even can be caused only by another process

Four Conditions for a Deadlock:
Mutual Exclusion: a section of code where only one process can get in
Hold and Wait
No preemption - the OS can't reach in and take a resource from a process
Circular Wait Condition

Need all four for deadlocks

An example of a deadlock:
Process C is holding resource U
Process C wants resource T
Process D is holding resource T
Process D wants resource U

Process C and D are waiting for the other process to let go of their resources so they can use it

Strategies for dealing with deadlocks:
Ignore
Detect and Recover
Dynamic avoidance by careful resource allocation
Prevention

Detection and Recover
Process A holds R, wants S
Process B holds nothing, wants T
Process C holds nothing, wants S
Process D holds U, wants S and T
Process E holds T, wants V
Process F holds W, wants S
Process G holds V, wants U

Algorithm to detect deadlock:
1. For each node, N, in the graph, perform following 5 steps with N as starting node
2. Initialize L to empty list and designate all arcs as unmarked
3. Add current node to end of L, check to see if node now appears in L two times. If so, graph contains a cycle (listed in L) and algorithm terminates
4. From given node, see if there are any unmared outgoing arcs. If so go to step 5; if not, go to step 6
5. Pick unmarked outgoing arc at random, mark it. Then follow to new current node and go to step 3.
6. If this is the initial node, graph does not contan cycles, algorithm terminates. Otherwise, dead end. Remove it and go back to the previous node.

Deadlock detection with multiple resources:
by Dijkstra (not Dijkstra's algorithm)

Resources in existence
Resources Available
Current allocation matrix
Request matrix

Deadlock Detection algorithm:
1. Look for unmarked process, Pi, for which the i-th row of R is less than or equal to A
2. If such a process is found, add the i-th row of C to A, mark the process, go back to step 1
3. If no such process exists, algorithm terminates


Recovery from Deadlock:
None are that attractive, it's a messy thing to recover from a deadlock
1. Preemption
2. Rollback
3. Killing Process

