# Overview of Network Layer
The primary purpose of the network layer is to move packets from the sending host to the receiving host. Host H1 sends a packet to host H2, with several routers in between them. H1 encapsulates a transport segment into a datagram and sends it to the nearby router. When H2 receives it, the nearby router extracts the transport-layer segments and delivers it up the protocol stack.

## Forwarding and Routing: The Data and Control Planes
Forwarding is the router local action of moving a packet from the input link to the appropriate output link. It occurs exclusively at the individual router. Forwarding typically takes place in hardware and happens on a very short timescale like nanoseconds.
Routing is the global action of determining the route that a packet will take from source to destination. Routing is implemented in software and can take longer like seconds.
In every network router is the forwarding table, which is a table that indicates the outgoing link. A router uses headers in the packet to index into the forwarding table. 
There are two approaches to computing the forwarding table.
### Control Plane: The Traditional Approach
The routing algorithm computes the forwarding table, and in the traditional approach a routing algorithm runs in each and every router. 

### Control Plane: The SDN Approach
The second approach is that a logically centralized, remote controller computes and distributes forwarding tables to the routers. The routers and controllers communicate with each other. This is the Software-Defined Networking (SDN). 
## Network Service Model
Potential services of a network:
Guaranteed Delivery, guaranteed delivery with bounded delay, in-order packet delivery, guaranteed minimal bandwidth, security.
The Internet's network layer provides a single service: best effort. There is no guarantee, but there will be an effort made.

# What's Inside a Router?
The four main components of a router:
1. Input Port
2. Switch Fabric
3. Output Port
4. Routing Processor

The input port performs the physical layer function of terminating the incoming physical link at a router, and performs the link-layer functions of operating with the link layer at the other side of the incoming link. It also performs a lookup, consulting the forwarding table to decide which output port the arriving packet will be forwarded to, via the switching fabric.

The switching fabric connects the input and output ports.

The output ports stores packets received from the switching fabric and transmits them, using the necessary physical and link-layer functions. When links are bidirectional, output ports will be paired with an input port. 

The above are all implemented in hardware, and make up the data plane. They are implemented in hardware because they need to be fast, operating on the nanosecond timeline. 

In traditional routers, the routing processor executes the routing protocols in , and maintains routing tables and attached link state information, and computes the forwarding table for the router. In the SDN approach, the routing processor communicates with the remote controller to receive the forwarding table and install them into the router's input ports.

The routing processor can be implemented in software, and operates at the millisecond or second timescale. 

Packet forwarding is a lot like cars leaving and entering an interchange. 
In destination-based forwarding, suppose a car stops at an entry station and indicates the final destination, and that determines the exit they leave.
In generalized-forwarding, suppose that as the car stops at the entry station, other factors besides the destination determine the exit. Those factors could include the origin point for example.

## Input Port Processing and Destination-Based Forwarding
The line-termination function and link-layer processing implement the physical and link layers for that individual input link. 
The lookup is performed at the input port. The forwarding table is copied from the routing processor to the line cards over a separate bus. With a shadow copy, forwarding decisions are made locally, which removes any central processing bottleneck.
As a metaphor, your boss gives you a booklet of instructions for you to perform your task, along with a lot of other employees. Each employee has a copy of this booklet (forwarding table). Everytime you need to do something, instead of asking your boss, you consult the booklet.

There are more than 4 billion possible 32-bit IP addresses (and a lot more IPv6 addresses), so having an entry for each individual IP address would be really bad. Instead, forwarding tables can have ranges to determine the proper output link.
For ex:
128.23.45.0 - 130.23.45.0: Interface 0

Even simpler, we can use prefixes.


| Prefix                     | Link Interface |
| -------------------------- | -------------- |
| 1100100 00010111 00010     | 0              |
| 11001000 00010111 00011000 | 1              |
| 11001000 00010111 00011    | 2              |
| Otherwise                  | 3              |
Longest prefix matching is used if a packet comes in that matches multiple entries. 

Lookups need to be fast - that's why they are implemented in hardware. Once the output port has been determined, the packet is sent into the switching fabric. 

Input ports also check the packet's version number, checksum, and TTL field, and also keep track of counters used for network management.


## Switching
Switching can be accomplished with three ways:
1. Switching via Memory: The earliest routers were computers - the CPU handled switching. The input and output ports were traditional I/O devices. The input port with an arriving packet signaled the processor with an interrupt, the packet was copied to processor memory, the processor extracted the destination address from the header, looked up the appropriate output port and copied the packet to the output port's buffers.
	1. If the memory bandwidth was such that a maximum of B packets per second can be written into, or read from memory, then the overall forwarding throughput must be less than B/2.
2. Switching via bus: an input port transfers a packet directly to the output port via a shared bus. There is no needed intervention by the processor. There is a switch-internal label prepended to the packet indicating the output port. All output ports receive the packet, but it is dropped by all except the one were the port matches the label, which is removed.
	1. The switching speed is limited to the speed of the bus. 
	2. Packets can only cross the bus one at a time. 
	3. Imagine if when you're waiting for the bus, everytme a bus comes, only one person can get on it.
3. Interconnection Network: A way to overcome the bandwidth limitation of a single bus is to use an interconnected network.
	1. Instead of 1 bus, there are multiple buses
	2. In a crossbar switch, there are 2N Buses that connect N input ports to N output ports.
	3. It can forward packets at the same time, and this is non-blocking: as long as the packets are destined for different output ports, they can be switched at the same time.

## Output Port Processing
The processing in the output port takes packets that have been stored in output port's memory and transmits them over the link.
1. Scheduling
2. Dequeuing
3. The needed link-layer and physical-layer transmission functions

## Where Does Queuing Occur?
Packet queues can form at both the input ports and output ports. 
If the input and output line speeds all have transmission rate of R_line packets per second, and there are N input ports and N output ports, and the switching fabric transfer rate is R_switch, if R_switch is N times faster than R_line, then negligible queuing will occur at input ports.

### Input queueing
If the switching fabric is not fast enough to transfer all arriving packets without delay, packet queues can occur at the input ports as they wait to be transferred to the output ports. 
Head of the Line (HOL) Blocking: A queued packet in an input queue must wait for transfer through the fabric (even though the output port is free) because it is blocked by the processing of another packet.

### Output Queueing
Even if the R_switch is N times faster than R_line, output port queuing can still occur. In the time it takes to send a single packet onto the outgoing link, N new packets will arrive and will have to queue. 
When there isn't enough memory to buffer an incoming packet, a decision must be made to either drop the arriving packet (called **drop-tail**) or to remove already queued packets to make room. it can also be decided to remove packets before the buffer is full and provide a congestion signal to the sender.
Active Queue Management algorithms are proactive packet-dropping and -marking policies.

### How much buffering is enough?
Old formula:
B = amount of buffer
RTT = round trip time in seconds
C = link capacity
B = RTT * C 
10 Gbps link, RTT of 250 msec(0.25 seconds), would need 2.5 Gbits of buffers.
1 Gbps link, RTT of 300 msec would need 0.3 Gbits of buffers.

A new formula, when a large number of independent TCP flows (N) pas through a link, the amount of buffering needed is B = RTT * C/sqrt(N).

More buffering doesn't always mean it's better. It could mean larger queuing delays.

## Packet Scheduling
This occurs in the output port.

**First-in-First-Out**: The first packet to arrive is the first to first to be served.
**Priority Queuing**: Packets arriving at the output link are classified into priority classes upon arrival att the queue. These classes can be based on TCP/UDP port numbers, src or dst, for example. Real-time VOIP packets would receiving greater priority than email packets.
The priority queueing discipline will transmit a packet from the highest priority class that has a nonempty queue. 
Non-preemptive priority queuing: the transmission of a packet is not interrupted once it begins. If a lower priority packet is being transmitted, and while that happens a high priority packet arrives, the lower priority packet will be transmitted before the higher priority packet is processed.
**Round Robin and Weighted Fair Queuing**: In Round Robin, packets are sorted into classes. The service for each class is alternating. For example, class 1, then class 2, etc.
In WFQ, the packets are classified, and are serviced according to a certain amount of time. Each class, i, is assigned a weight, w_i. During any interval of time where there are class i packets to send, class i will be guaranteed to receive a fraction of service according to w_i​/(∑​w_j)
Where the sum is taken over all the classes that have packets queued for transmission. 


# The Internet Protocol (IP): IPv4, Addressing, IPv6, and More

## IPv4 Datagram Format:
Version Number: specifies the IP protocol version number. The router can determine how to interpret the remainder of the IP datagram.
Header Length: the length of the header, determines where the payload (the transport segment) begins. There are many options for a header.
Type of Service: Allows different types of IP datagrams to be distinguished for each other. Shows what kind of traffic it is.
Datagram Length: The total length of the IP datagram (header plus data). Max length is 64,535 (16 bits long), rarely goes over 1500, which allows an IP datagram to fit in the payload field of a maximally sized Ethernet frame.
Identifier, flags, fragmentation offset. Have to do with fragmentation - a large IP datagram is broken into several smaller datagrams, which is forwarded to the destination and reassembled there.
TTL: Time to live, the amount of hops (routers) the datagram can pass through before it needs to be dropped.
Protocol: Indicates specific transport layer protocol to passed to. 6 indicates TCP, 17 indicates UDP. 
Header Checksum: aids routers in detected bit errors. Computed by treating each 2 bytes in the headr and summing these numbers using 1s complement arithmetic. Only IP header is checksummed at the IP layer, not entire datagram. There is a TCP error checking because the entire TCP/UDP segment is checksummed, not just the header. 
Source and destination IP address
Options

## IPv4 Addressing
The boundary between a host and the link into the network is called the interface. The IP address for the host is associated with the interface. A router can have more than one interface, so it can have more than one IP address.
Dotted Decimal Notation: the way 32 bit long addresses are written. 193.15.65.32 is an example of an IP address, and in binary it would be:
11000001 00001111 01000001 00100000
A subnet is taking a network and dividing it into smaller sub-networks.
*"To determine the subnets, detach each interface from its host or router, creating islands of isolated networks, with interfaces terminating the end points of the isolated networks. Each of these isolated networks is called a subnet."*

The Internet's Address assignment strategy is known as Classless Interdomain Routing. The 32 bit Ip address is divided into 2 parts - the network portion and the host portion. It has this notation: a.b.c.d/x, where x indicates the number of bits in the first part of the address.

The network portion of the IP address is also referred to as the prefix. An organization is typically assigned a block of contiguous addresses.

Classful Addressing is the addresing scheme used before CIDR, and network portions of an IP address were constrained to be 8, 16, or 24 bits in length. The problem is the huge discrepancies between these numbers.
A /24 address would have 2^(32 - 24) - 2 = 254 hosts. If an organization needed 300, it would be given a /16, which has 2^(32-16) - 2 = 65,534. Which is obviously way too many unnecessary IP addresses.

Route/Address Aggregation is the ability of an ISP to advertise a single prefix for multiple organizations that have that prefix. For example, an ISP has 200.23.16.0/20. It gives out 8 200.23.16.0/23 addresses.  The ISP only advertises 200.23.16.0/20, not each one individually. 

### Obtaining a Block of Addresses
ISPs get address spaces from ICANN, and ISPs give them out to customers.

### Obtaining a Host Address: The Dynamic Host Configuration Protocol
DHCP automatically assigns IP addresses in a network. It is plug-and-play, or zeroconf.
Four step process:
Discover: The client sends a message within UDP with port 67 to the broadcast address 255.255.255.255. The src is 0.0.0.0 port 68 It does this because it doesn't know the IP address of the DHCP server. It has a transaction ID. 
Offer: The server gets a message from 0.0.0.0 on port 67 requesting IP address. It sends an offer message on port 68 to broadcast address. This contains the transaction ID, along with the proposed IP address and address lease time. 
Request: The client chooses from among one or more server offers and responds with a request message.
ACK: The server ACKs the request message. 

## NAT
There aren't enough IP addresses out there. There are only 2^32. Think of how many devices in your home contain IP address.
Phones, gaming stations, laptops, tablets. Printer. Smart Devices if you get one.
A NAT router acts as a single machine to the internet with a public IP address. Connecting to the NAT router is a private network where everyone has their own private IP address. 
Routers have a NAT translation table to know the internal host it should forward a datagram.
NAT Traversal - a technical solution to the problem of how to connect to a host behind a NAT. 

## IPv6
A successor to IPv4 to solve the depleting IPv4 space.

### IPv6 Datagram format
Expanded address capabilities from 2^32 in IPv4 to 2^128. 
A Streamlined 40-byte header
Flow labeling: Flow labeling is a way for IPv6 to label packets belonging to a particular "flow". Audio and video might be a flow, but file transfer and email might not.

Fields:
Version: identifies the version number. 6 makes it IPv6. 4 does not make it IPv4.
Traffic Class: gives priority to certain packets, like TOS in IPv4.
Flow Label
Payload Length: 16 bit unsigned integer (can only be non-negative), number of bytes following the 40 byte header.
Next Header: Identifies the protocol the datagram will be delivered to - TCP or UDP.
Hop Limit: same as TTL
Src and Dst Address
Data: The payload portion.

IPv6 does not allow for fragmentation/reassembly. It removes the header checksum. 

Tunneling: The sending IPv6 node takes the entire IPv6 datagram and puts it into the data field of an IPv4 datagram. 

# Generalized Forwarding and SDN
Generalized forwarding: a match can be made over multiple header stacks and the action can include forwarding, load balancing, rewriting header values, blocking/dropping, etc.

OpenFlow is a standard of SDN. The Match-plus-action forwarding table is known as a flow table in OpenFlow. 
- A set of header field values where an incoming packet will be matched.
- A set of counters that are updated as packets are matched.
- A set of actions to be taken.

## Match
Matches can be taken on a number of link-layer, network layer, and transport layer fields. Not all fields can be matched, like TTL or datagram length.

## Action
Forwarding: Incoming packets can be forwarded to a particular physical output port, or many (multicast), or all (broadcast). It can also be encapsulated and sent to the remote controller for ctions to be taken there.
Dropping
Modify-field: packet headers can be modified (except IP protocol field)


# Middleboxes
A middlebox sits between the sender and receiver and does more than just forward data - it performs other functions.
*“any intermediary box performing functions apart from normal, standard func- tions of an IP router on the data path between a source host and destination host”*
Functions performed by a middlebox:
- NAT Translation
- Security Services
- Performance Enhancement

# Questions

## Section 4.1
**R1.  Let’s review some of the terminology used in this textbook. Recall that the name of a transport-layer packet is segment and that the name of a link-layer packet is frame. What is the name of a network-layer packet? Recall that both routers and link-layer switches are called packet switches. What is the fundamental difference between a router and link-layer switch?**
Network-layer packet is a datagram
A router is a layer 3 device that forwards based on IP addresses, a link-layer switch is a layer 2 device that forwards based on MAC addresses. 

**R2.  We noted that network layer functionality can be broadly divided into data plane functionality and control plane functionality. What are the main functions of the data plane? Of the control plane?**
Data Plane: Forwarding packets from input to output ports
Control Plane: Controls the routes that packets take from source to destination.

**R3.  We made a distinction between the forwarding function and the routing func- tion performed in the network layer. What are the key differences between routing and forwarding?**
Routing: The complete route the packet takes from source to destination. A global action.
Forwarding: The router local action of getting packets from input ports to output ports.

**R4.  What is the role of the forwarding table within a router?**
A forwarding table is used by the router to determine which outbound link a packet goes to.

**R5.  We said that a network layer’s service model “defines the characteristics of end-to-end transport of packets between sending and receiving hosts.” What is the service model of the Internet’s network layer? What guarantees are made by the Internet’s service model regarding the host-to-host delivery of datagrams?**

The service model is "best-effort" : no guarantees are made.

## Section 4.2
**R6. In Section 4.2, we saw that a router typically consists of input ports, output ports, a switching fabric and a routing processor. Which of these are implemented in hardware and which are implemented in software? Why? Returning to the notion of the network layer’s data plane and control plane, which are imple- mented in hardware and which are implemented in software? Why?**
Hardware: input ports, output ports, switching fabric. - Data Plane
Software: routing processor - Control plane
The input ports, output ports, and switching fabric, responsible for forwarding packets, needs to be fast enough to do so on a nanosecond scale. The Routing processor - responsible for either computing the forwarding table (traditional) or communicating with remote controller that computes the forwarding table (SDN) needs to operate on a millisecond or second scale.

**R7.  Discuss why each input port in a high-speed router stores a shadow copy of the forwarding table.**
To prevent a central processing bottleneck.

**R8.  What is meant by destination-based forwarding? How does this differ from generalized forwarding (assuming you’ve read Section 4.4, which of the two approaches are adopted by Software-Defined Networking)?**
Destination-based forwarding is where the packet's destination determines the outgoing link. In generalized forwarding, forwarding decisions can be made based on a number of headers.

**R9.  Suppose that an arriving packet matches two or more entries in a router’s forwarding table. With traditional destination-based forwarding, what rule does a router apply to determine which of these rules should be applied  to determine the output port to which the arriving packet should be switched?****
Longest-prefix matching.

**R10.  Three types of switching fabrics are discussed in Section 4.2. List and briefly describe each type. Which, if any, can send multiple packets across the fabric in parallel?**
Switching via memory
Switching via bus
Switching via interconnection network
Interconnection network can send multiple packets across the fabric as long as they are not destined for the same output port.

**R11.  Describe how packet loss can occur at input ports. Describe how packet loss at input ports can be eliminated (without using infinite buffers).**
If there are N input ports and N output ports, if the switching fabric is not N times faster than the speed of the input ports, then queuing can occur. If the queues grow too much, packet loss can occur. To eliminate packet loss at input ports, the switching fabric needs to be N times faster than the input ports.

**R12.  Describe how packet loss can occur at output ports. Can this loss be pre- vented by increasing the switch fabric speed?****
If the switch fabric is N times faster than the speed of the input ports and output ports, then by the time it takes an output link to forward one packet, N packets will arrive. Increasing the switch fabric speed will not prevent this problem.

**R13.  What is HOL blocking? Does it occur in input ports or output ports?**
When a packet being sent through the fabric is blocked because another packet is being processed. It occurs in input ports.

**R14.  In Section 4.2 ,we studied FIFO, Priority, Round Robin(RR), and Weighted Fair Queueing (WFQ) packet scheduling disciplines? Which of these queueing disciplines ensure that all packets depart in the order in which they arrived?**
FIFO

**R15.  Give an example showing why a network operator might want one class of packets to be given priority over another class of packets.**
Real time video conferencing or voice call traffic over FTP or email traffic. 

**R16.  What is an essential different between RR and WFQ packet scheduling? Is there a case (Hint: Consider the WFQ weights) where RR and WFQ will behave exactly the same?**
RR -> All classes are treated equally. WFQ -> each class is assigned a weight.
If all classes have the same service weight, then WFQ will behave like RR.

## Section 4.3
