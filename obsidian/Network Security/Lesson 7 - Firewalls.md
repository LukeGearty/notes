Firewall: enforces security between networks of different security policies. 
Firewalls can be configured so that every area of a network can be firewalled off from others. Firewalls can enable microsegmentation - CIsco, Juniper, all have firewalls that can be segmented. 
Prior to modern day firewalls, different projects/servers/networks would be placed on different VLANs. 

Packet Filters (traditional): fast, stateless firewalls, can handle massive DDoS attacks.
Stateful Firewalls: maintain/remembers connection info, can handle more complex traffic than packet filters
Proxies(Application Gateway): hosts that sit between servers and network, serves as an intermediary on a network - web, email, FTP
Bastion Host: host intended to withstand attacks. Don't want it to be compromised. It is a high-priority target, running very few services, and is hardened. 
DMZ: Network architecture that puts outward forward-facing services in a special area of the network. 


Stateful Firewalls: Kep track of connection states, may timeout entries which see no activity for set period of time. If rule table indicates that stateful table must be checked, check to see if there is already a connection in stateful table.


| Protocol | State    | source addr | dst addr     | src port | dst pot |
| -------- | -------- | ----------- | ------------ | -------- | ------- |
| tcp      | SYN sent | 222.22.1.7  | 37.96.87.123 | 126999   | 80      |

Application Gateways: something in between the user and the server, to inspect traffic and improve usage/performance. 
Example: When going to amazon.com, the web proxy is in the middle and takes the connection, recreates it, and goes to the real server. It can perform malware scans, caching, heuristic analysis, DLP policies. 
For TLS, and the TLS the proxy is also a CA - a proxy CA. 
Chaining Proxies: not usually done due to performance issues, can be done for email due to latency not really being a big concern.

Some protocols for the intended purpose of carrying another protocol.
SOCKS Proxy Protocol: Generic proxy protocol, doesn't have to redo all the code when proxifying an application.
Can be used by HTTP, FTP, telnet, SSL
Independent of application layer protocol
Includes authentication

DMZ: a network architecture philosophy/style in which some important forward facing servers are placed in special area of network that is neither internal nor external. 

Internal Network -> Firewall -> router -> Firewall
							|
							V
						   DMZ
Typically, each server gets its own area (Web, FTP, Email)

iptables - specific firewall, interface to the Linux firewall netfilter. Can do a lot - filter - allow, deny, drop, reject, etc. Can also do NAT and PAT (Network and Port Address Translation), manipulate packet.
Filter Table has 3 chains: INPUT, OUTPUT, FORWARD
INPUT: Is it coming to the firewall? The firewall is the destination
OUTPUT: Is it coming out of the firewall? The firewall is the source
FORWARD: Is it coming through the firewall? The firewall is neither source nor destination
Input and Output are for host based - FORWARD is for network based firewalls.

iptables cheat sheet
-A, --append
-i, --in-interface: interface in which packet is received
-j ACCEPT, DENY
iptables -L -v: see the rules
-F: flush tables
-I INPUT 1: insert rule into Input table position 1
-p protocol (tcp, udp, icmp)
--tcp-flags SYN
-s: source IP (can also be in CIDR notation)
--sport: source port
-d: dest IP
--dport: destination port

Default Policy: when iptables goes through rules and does not find a matching rule, it goes to the default policy. 
For this class: should drop everything (IMPORTANT, REMEMBER)
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP

Reject vs Drop:
Reject: "Nice" method: sends an ICMP unreachable
DROP: "Conventional": doesn't respond, discards packet

Connection tracking
-m conntrack --ctstate
NEW,ESTABLISHED,RELATED (for protocols that use multiple ports like FTP)