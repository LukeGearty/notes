# Introduction to the Link Layer
Node: Any device that runs a link-layer protocol, can include hosts, routers, switches, and WiFi access points.
Links: the channels that connect nodes.
Over a given link, a transmitting node encapsulates the datagram in a link-layer frame and transmits the frame into the link.

## Services Provided by the Link Layer
The baisc service is to move a datagram from one node to an adjacent 
Helpful memorization tool: Framing the link access with reliable delivery to ensure error detection and correction.
- Framing: Encapsulate network layer datagram before transmitting. The frame has a number of header fields, and the data payload which is the datagram.
- Link Access: The Medium Access Control protocol specifies the rules by which a frame is transmitted onto the link. 
- Reliable Delivery: Some link layer protocols provide reliable delivery. Often used with links with high error rates ,like wireless. 
- Error Detection and Correction: bit errors can occur, caused by signals weakening (attentuation) or electromagnetic noise. Some protocols implement error detection because there is no need to forward a datagram that has an error.

## Where is the Link Layer Implemented
Network adapter, sometimes called the Network Interface Card. Much of it is implemented in hardware. Most of the link layer is implemented in hardware.
On the receiving side, the controller takes a datagram created and stored in host memory, encapsulates into a frame, and then transmits it on to the link. On the receiving side, a controller receives the entire frame, and extracts the network-layer datagram. 
Some of it is implemented in software, for assembling link-layer addressing info and activating the controller hardware.

# Error-Detection and -Correction Techniques

## Parity Check
A scheme where a bit is added to the data, *d* being sent. The number of ones in this d + 1 will be either even if an even parity scheme is chosen, or odd if an odd parity scheme is chosen.
1011 + 1 for even, for ex
0110 + 1 for odd, for ex

The problem is that if there is an even number of errors for an even parity scheme (and odd number for odd parity), then it would go undetected.
For example, 1011 + 1 is the original message, it becomes 0111 + 1. This would go undetected.
A 2 dimensional parity scheme is where the d bits are divided into i rows and j columns, with a parity bit computed for each row. 
This can detect and correct one bit errors.
## Checksumming
Recall TCP/UDP checksumming.
Bytes of data are treated as 16-bit integers and summed. Overflow is wrapped around. 1s complement is taken of the sum to form the checksum. The receiver takes the 1s complement of the sum of the received data, including the checksum, and checking whether the result is all 0 bits. 

## Cyclic Redundancy Checks
More powerful error detection coding
D data bits (think of it is as a binary number)
G bit patter (generator) of r + 1 bits (given, specified in CRC standard)

D + R 
Compute D + R first by left shifting data bits to left r positions (D * 2^r) and XOR R
sender: compute r CRC bits, R, such that <D, R> is exactly divisible by G (mod 2)
Can detect all burst errors less than r + 1 bits
Widely used in practice (Ethernet, 802.11 WiFi)

Sender computes R such that D * 2^r XOR R = nG
If we divide D * ^r by G, we want remainder R to satisfy R = remainder [(D * 2^r)/G]



# Multiple Access Links and Protocols
Point to Point Link: Single sender and single receiver
Broadcast Link: multiple sending and receiving nodes all connected to the same, single, shared broadcast channel

The multiple access problem is how to coordinate the access of multiple sending and receiving nodes to a shared broadcast channel.

Because all nodes are capable of transmitting frames, when two nodes transmit frames at the same time, all of the nodes receive multiple frames at the same time. This is called a collision, and when it happens no node can make sense of what just happens. It's like talking at the same time as someone else. Coordinating these multiple nodes and their transmissions is the job of the multiple access protocol.  

Desired traits of a Multiple Access Protocol:
1. When only one node wants to transmit, it can transmit at the full rate of the channel, R
2. When M nodes want to transmit, each can send at average M / R
3. Decentralized:
	1. No special node to coordinate transmissions
	2. No synchronization of clocks, slots needed
4. Simple to implement


Taxonomy of the Multiple Access Protocols:
- Channel Partitioning Protocols: the channel is divided into smaller pieces based on time, frequency, code, and each piece is allocated a piece of the channel for use
	- TDM, FDM, CDMA
- Random Access: the nodes transmit at full rate, R, allows collisions. When collisions happen, the nodes wait a randomized amount of time before retransmitting.
- Taking-Turns Protocols: The nodes take turns transmitting when they have to transmit. 

## Channel Partitioning Protocols
Time Division Multiplexing: time is divided into time frames, and further divided into time slots which are assigned to one of N nodes. Whenever a node has to send, it does so during its assigned time slot.
Frequency Division Multiplexing: divides R bps channel into different frequencies (bandwidth of R/N) and assigns each frequency to N nodes. 
Advantages of both: fair, avoids collision.
Disadvantage: node is limited to R/N bps, even if it is the only node with packets to send.
Code Division Multiple Access: a different code is assigned to each node, which is used to encode the data bits it sends. Can have nodes transmit simultaneously.

## Random Access Protocols
Nodes have the full transmission rate of the channel, R bps. When there is a collision, the node waits a random delay before retransmitting again. Each node involved in the collision independently chooses a random delay. 

### Slotted Aloha
Frames consist of exactly L bits, time is divided into slots of size L / R (slot equals the time to transmit one frame). The nodes are synchronized so that each node knows when slot begins and when collisions happen. The nodes can only transmit at the beginning of the next slot. If there is a collision, the node detects the collision before the end of the slot, and will retransmit its frame in the subsequent slot with probability *p* until the frame is transmitted without a collision. 

### ALOHA
The first ALOHA protocol was unslotted, fully decentralized. It is less efficient because of the lack of synchronization between nodes. 

### Carrier Sense Multiple Access (CSMA)
CSMA is a "polite protocol" - it waits for detecting no transmissions and then transmits, and if someone else begins talking at tthe same time, it stops talking.

Collisions can still occur in CSMA, even though devices "listen" before sending. Signals take time to travel across the network - "propagation delay". When one device starts sending, and its signal hasn't reached another device yet, collisions can occur because the second device thinks the channel is free.

### CSMA/CD
CSMA/CD adds collision detection - when it detects a collision, a node will cease transmission. 
From the perspective of an adapter in a node attached to the broadcast channel:
1. The adapter obtains a datagram from the network layer, prepares a link-layer frame, and puts the frame adapter buffer
2. If the adapter senses the channel is idle, it starts to transmit the frame. If it is busy, it waits until it senses no signal energy.
3. While transmitting, the adapter monitors for the presence of signal energy coming from other adapters using the broadcast channel.
4. If the adapter transmits the entire frame without detecting signal energy from other adapters, the adapter is finished with the frame. If the adapter detects signal energy from other adapters while transmitting, it aborts the transmission.
5. After aborting, the adapter waits a random amount of time and then returns to step 2.

To determine the interval of time from which to use, the **binary exponential backoff** algorithm is used. When transmitting a frame that has experienced n collisions, the value of K at random from {0, 1, 2, ... 2^n - 1}. The more collisions experienced, the larger the interval. In ethernet, the actual amount of time a node waits is K * 512 bit times (K * the amount of time needed to send 512 bits into the Ethernet), and the maximum value that n can take is capped at 10.
For example: on a 100 Mbps Ethernet, if there is one collision, then the choices are K = 0 or K = 1. If it chooses one, it waits 1 * 5.12 microseconds.

## Taking-Turns Protocols

**Polling Protocol**: There is a master node that polls each of the nodes in the network in a round robin fashion. First it sends a message to node 1, say ing it can transmit up to some maximum number of frames. Then it moves on to node 2, then node 3, etc. 
Drawbacks: Polling delay, the amounto f time it takes to notify a node it can transmit. It also has a single point of failure.
Token Passing protocol: A small, special purpose frame is exchanged among the nodes in some fixed order. Node 1 gets the frame, transmits, passes it to node 2, who passes it to node 3, etc. It only holds on to the token if it has frames to transmit. This protocol is decentralized and highly efficient, but the failure of one node can crash the entire channel, and if a node doesn't release the token then there has to be some kind of recovery procedure.


# Switched Local Area Networks

## Link-Layer Addressing and ARP
Hosts and routers have link-layer addresses associated with their adapters (network interfaces). These addresses are called MAC addresses. They are 6 bytes long, 2^48 possible MAC addresses. IEEE manages the MAC address space to ensure that manufacturers have unique MAC addresses for their products. 
The MAC broadcast address: Similar to IP broadcast address, FF-FF-FF-FF-FF-FF, it is used when all adapters need to get the frame.

Routers and hosts have MAC addresses in addition to IP because LANs are designed for arbitrary network-layer protocols, not just for IP. 

### ARP
Address Resolution Protocol maps IP addresses to MAC addresses. Each host an router has an ARP table, which contains the mappings, and a TTL for each mapping. If it does not have a mapping for the IP address, it constructs an ARP packet and sends it to the broadcast address asking for the MAC address "Who has a.b.c.d". The correct host responds and tells the sender.

### Sending a Datagram off the subnet
When sending a datagram to a host on the same subnet, the destination MAC is that host's MAC address, which is in the ARP Table. Off the subnet, the destination MAC is going to originally be the MAC address for the router for that subnet. The router then routes it off the subnet into a new frame.

## Ethernet
The dominant LAN wired tech. It was successful due to relative simplicity and cost. Originally used a coaxial bus topology, it evolved to use a hub based star topology, and then replaced the hub with a switch. 

### Ethernet Technologies
Ethernet technologies are identified by various acronyms, like 10BASE-T, 10BASE-2, etc. The first part refers to the speed, 10 meaning 10 Megabit per second. BASE is baseband Ethernet, the physical media only carries Ethernet traffic. The final part refers to the physical media, like twisted-pair copper wires.

## Link Layer Switches
The switch is transparent to the host and routers in the subnet; they address a frame to another host/router and happily send it to the LAN, blissfully unaware that a switch will be receiving the frame and forwarding it.

### Forward and Filtering
**Filtering**: determines whether a frame should be forwarded or dropped
**Forwarding**: Determines the interface to which a frame should be directed.
This is done with a switch table, that contains the addresses for some (not all) hosts/routers on a LAN.
When a frame comes to a switch on interface *x*, there are 3 possible cases:
1. There is no entry for the dst address, so the switch broadcasts the frame
2. There is an entry which associates the dst address from x, the switch realized the frame already reached its local area and simply discards it
3. If there is an entry and points to another interface, the switch sends the frame only to that specific location. 

### Self-Learning
The switch table is initially empty, when an incoming frame comes in, the switch stores in its table: The Mac Address in the frame's source addr, the interface from which the frame arrived, and the current time. The switch deletes an address if no frames are received with that address as the src addr after some period of time. 

### Properties of Link-Layer Switching
- Elimination of collisions: switches buffer frames and never transmit more than one frame on a segment at any time
- Heterogeneous Links: different links in the LAN can operate at different speeds because a switch isolates one link from another
- Management: switches ease network management. They can detect and isolate problems (a faulty device sending too much data, for instance), and can provide useful info about network traffic

### Switches versus Routers
Switches and routers work at different levels layer 2 vs layer 3. Switches are plug and play and they can forward frames quickly, but the active topology of the network is restricted to a spanning tree to prevent the cycling of broadcast frames. They require large ARP tables. They are also susceptible to broadcast storms - a host transmitting an endless stream of Ethernet frames, the switch forwards all of them. Routers - packets do not cycle through routers even when the network has redundant path. Routers allow the Internet to be built with a rich topology. 
They are however not plug and play and take longer to process. Small networks are better for switches, larger networks are better for routers.

## Virtual Local Area Networks (VLANS)
The problem with having a setup of switched LANs connected to the switched LAN of other groups via switch hierarchy: 
1. Lack of traffic isolation - ARP And DHCP traffic for example will spread through the entire network
2. Inefficient use of switches: If you have many small groups a single switch can be large enough to accomodate everyone, but there wouldn't be any traffic isolation
3. Managing Users: if someone moves between groups, the physical cabling must be changed.

VLANs allow you to create multiple separate virtual networks on a single physical network switch or infrastructure. Devices within the same VLAN act as if they are connected to their own dedicated switch, isolated from devices in other VLANs.

To allow communication between VLANs, connect switch ports to an external router, or use a switch that has routing capabilities.

VLAN Trunking: a scalable solution to interconnecting VLAN switches, they share a physical link between network devices, such as switches.


# Questions
## Sections 6.1 - 6.2
**R1.  Consider the transportation analogy in Section 6.1.1. If the passenger is analagous to a datagram, what is analogous to the link layer frame?**
The link layer frame is analagous to the transportation segment - the bus, the plane, the train ride, etc. 

**R2.  If all the links in the Internet were to provide reliable delivery service, would the TCP reliable delivery service be redundant? Why or why not?**
It would not, because the TCP reliable delivery service also ensures that packets arrive in the proper order. If the links provided rdt, then it would most likely just be to ensure the packets arrive without errors. 

**R3.  What are some of the possible services that a link-layer protocol can offer to the network layer? Which of these link-layer services have corresponding services in IP? In TCP?**
Framing: Encapsulating network-layer datagrams within a link-layer frame, adding a header where the data field is the datagram.
Link Access: The rules which a frame is transmitted onto the link
Reliable Delivery: Some move the datagram across the link without error
Error Detection and Correction: bit errors are introduced by signal strength waning (attenuation) and electromagnetic noise, with EDC implemented in hardware.

## Section 6.3
**R4.  Suppose two nodes start to transmit at the same time a packet of length L over a broadcast channel of rate R. Denote the propagation delay between the two nodes as d_prop. Will there be a collision if d_prop < L/R? Why or why not?**
L/R = transmission delay, how long it takes to get on the wire.
D_prop = the time it will take to get from one node to another.

Yes there will be a collision because the other nodes frame will be received in the middle of transmitting its own frame, because the propagation delay (time it takes to reach the other destination) is less than the time it takes to put the packet on the wire.

**R5.  In Section 6.3, we listed four desirable characteristics of a broadcast channel. Which of these characteristics does slotted ALOHA have? Which of these characteristics does token passing have?**
The four desired characteristics of a broadcast channel:
1. If there is only one node to send, that node has a throughput of R bps.
2. When M nodes have data to send, the nodes have an average throughput each of R / M bps. 
3. The protocol is decentralized - there is no node that represents a single point of failure for the network
4. The protocol is simple and inexpensive to implement

Slotted ALOHA is a simple protocol - nodes only transmit at the beginning of a time slot, if there is a collision then the nodes stop and wait for the next time slot, and they transmit with probability p.
If there is only one node to send, Slotted ALOHA will have the throughput of the entire channel. If there are M nodes, the nodes will have an average throughput of R / M.
Slotted ALOHA is only partially decentralized. It requires all the clocks in the nodes to be synchronized.

Token Passing is a simple protocol - the token is passed around and if there is data to send when the node has the token, then they can send data.
If there is only one node to send, the token passing will have the throughput of the entire channel.
When M nodes have data to send, the nodes will have an average of R / M bps in token passing.
Token passing is decentralized.


**R6.  In CSMA/CD, after the fifth collision, what is the probability that a node chooses K = 4? The result K = 4 corresponds to a delay of how many seconds on a 10 Mbps Ethernet?**
The binary exponential backoff:
After experiencing n collisions, the node will choose the value of K at random from {0, 1, 2, ... (2^n) - 1}. 2^n - 1 in this case will be 2^5 - 1 = 32 - 1 = 31. The probability that k = 4 will be 1 / 32. 
The delay if k = 4:
Ethernet speed  = 10 Mbps = 10 * 10^6
Slot time = 512 bits / 10 x 10^6 = 51.2 microseconds
The delay is = k * slot time
4 * 51.2 = 204.8 microseconds

    
**R7.  Describe polling and token-passing protocols using the analogy of cocktail party interactions.**
Polling: Someone is the master curator of the party and allows one person to talk at a time.
Token-Passing: Some object is held that is passed. When you get the object, you can talk. When you finish, you have to pass it to the next person. 

**R8.  Why would the token-ring protocol be inefficient if a LAN had a very large perimeter?**