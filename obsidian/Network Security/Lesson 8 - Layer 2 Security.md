Layer 2 refers to Ethernet and WiFi. It pertains to how you connect to the network - plug in, connect to WiFi.
In OSI or TCP/IP, different layers work together without knowledge of each other. An attack on Layer 2 is an attack on other layers as well.

CAM Table vs ARP Table
ARP Table: Maps IP address to MAC addresses, Layer 3 and Layer 2.
CAM Table: physical ports to MAC, mapping between Layer 2 and Layer 1. Which MAC address relates to which physical port it comes from. CAM Tables are only on physical devices. TCAM memory is extremely fast and expensive physical memory - a switch is able to route extremely fast, so it needs a CAM table to route very quickly.
A CAM Table has limited space because it is physical memory. 

| MAC | Port |
| --- | ---- |
| A   | 1    |
| C   | 3    |

MAC A sends a message to port 3 (Switch) destined for MAC B. Port 3 doesn't know where B is, so it sends out a broadcast message. B sends back the response, and it updates the CAM table. After that, traffic between A and B will be private.

| MAC | Port |
| --- | ---- |
| A   | 1    |
| B   | 2    |
| C   | 3    |

Let's say C is an attacker. A CAM overflow attack (or table flooding) is sending out a lot of messages to the switch. The messages will have bogus  MAC addresses and will cause the switch to dump the valid addresses it has in the CAM table to make room for the bogus information. After it happens, the switch will default to broadcast private messages, including to the attacker. This attack does not work on virtual routers or virtual switches - that memory can be unlimited. It only works for physical features.
Countermeasures: Port Security
Limits the Amount of MACs on an interface. Port security limits MAC flooding and locks down port and sends an SNMP trap. It can be configured so that only one MAC address can come from a specific port. If it sees a second MAC address, it will shut down the port. 

VLAN Hopping Attacks
VLAN: A way of logically separating/segmenting a physical network into much smaller, isolated networks. Although devices are connected to the same switch, devices on different VLANS cannot talk to each other unless they go through a router.
Basic Trunk Ports: An ethernet connection that can handle multiple VLANs. Trunk ports have access to all VLANs by default, used to route traffic for multiple VLANs across the same physical link. Encapsulation can be 802.1q or ISL. 
VLANs can span multiple different switches. 
Auto-trunking: switches automatically negotiate and configure trunk ports between themselves. Eliminates the need for manual configuration of trunk links. 
**Switch Spoofing**: The attacker comes in and pretends to be a switch. Years ago, auto-trunking is on, allowing this attack. To mitigate, turn auto-trunking, don't use VLAN 1 for user traffic, explicitly configure trunking on infrastructure ports.
**Double 802.1q Encapsulation Attack**: a computer puts 2 different VLAN tags on their headers. When it comes to the first switch, it removes the first tag and accidentally sends it to a second VLAN. The attacker needs to come from the native VLAN - most enterprise networks have VLANs enabled, but there can be legacy part of the network that doesn't have VLANs enabled. Attack only works one way. 

DHCP Attacks
DHCP: The protocol that gives IP addresses to anyone connecting to a network. The person coming into the network does a **Discover** as a broadcast message to get to the DHCP server. The DHCP server responds with an **Offer** that offers an IP address to that client. The client then sends a **Request** with the IP address it wants, broadcast. The server then responds with an **Ack** to the client. There are sometimes more than one DHCP servers for redundancy. 
If the client gets more than one DHCP offer, it selects the one that comes first. When DHCP server gives IP address, it gives out based on MAC address. If a machine connects again, DHCP will give out same IP, keeping track of MAC addresses. It doesn't look at layer 2 MAC address, it looks at the MAC address inside the DHCP header (called the Client Hardware Address, but it is the same as the MAC address).
DHCP Starvation Attack: When an attacker comes in and takes all the IP addresses. This is done with Gobbler (tool that does it). Let's say DHCP server has 155 IP addresses to give out. Gobbler will make 155 DHCP discoveries, get 155 offers, make 155 requests, and get 155 acks. The attacker needs to change MAC address in the DHCP header, not necessarily the Ethernet Header. Can be easily done in python - scapy.
Mitigation: Port Security, limits number of src MAC addresses on port. Port Security only limits on Ethernet Header - does not limit the Ethernet Header. If port security is only mitigation, it is easy to bypass this type of attack. 
Rogue DHCP Server: Attacker pretends to be a DHCP server. Client sends the broadcast discovery, the attacker acts as a DHCP server alongside the actual DHCP server - the client will take the first one that comes, so if the attacker is faster, this attack will work. A DHCP offer has an IP address, the subnet mask, the default routers, the DNS servers, the lease time, and if it's windows, it will also have the proxy servers. They can then manipulate any of that information - for example, route the client to a malicious DNS server. 

Countermeasures: On mid-level devices, there is something called DHCP snooping. It has a set of 3 features: it sets each interface as trusted or untrusted: client side as untrusted, server as trusted. The untrusted side can only send stuff the client is able to send - DHCP discovery or request in this example. Cannot send offers or acks. The second feature is keeping a binding table that records all information/messages. Third feature is to match the DHCP MAC address to the Ethernet Header. 
DHCP Rogue server can stop by blocking DHCP dport on the switch side if you do not have a more expensive switch. This won't block starvation attacks
Discovery/Request: sport 68, dport 67
Offer/Acks/Naks: sport 67 dport 68

ARP Attacks
Before a station can talk to another, ARP is used to map the IP address to MAC address. Done through an ARP request, broadcast. The reply is sent from that host (I am IP X with MAC Y) and the requester updates the ARP cache. 
In ARP header, the MAC address is on it multiple times (as part of the Ethernet Header). These don't have to be the same. 
Attacker can poison the ARP table to point the IP addresses to the attacker IP address. There needs to be a poison on both sides of the conversation - poison router and target, otherwise it will be fixed quickly. 
Countermeasures: 
Dynamic ARP Inspection - only works on more expensive devices, first it requires DHCP snooping to be turned on. It checks the DHCP snooping bind table

Spoofing Attacks
Attacker sends packets with incorrect source MAC address
IPSG: IP source guard, countermeasure. Requires DHCP binding table, reads info and then from that information it looks at IP packet and compares it to table. If the mapping (MAC to IP) in the binding table is correct, it lets it through.
This feature is very expensive ($). 

Spanning Tree Protocol
Protocol on enterprise networks that stops loops in networks. In network architecture, you do want loops for redundancy, but you also want the topology to resemble a tree. 
STP looks at network and creates the best tree-like architecture by sending Bridge Protocol Data Units. It helps avoid loops and ensure broadcast traffic does not become storms. 
An attacker sends BPDU to become root bridge - where all messages are sent through this bridge, and can see messages. 
BPDU Guard makes sure that only specific devices can send BPDU messages, switches do not accept BPDU messages from random hosts, just other switches. 