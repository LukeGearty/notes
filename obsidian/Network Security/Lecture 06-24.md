Exercise #1
Explain the following nmap scan types:
TCP Connect Scan: Default scan, makes a full TCP connection (3 way handshake is completed) to discover open ports. Sends a RST packet after final ACK to kill connection.
TCP SYN Scan: Sends SYN packet and gets back a SYN ACK to discover open ports (partial 3 way handshake, kills connection before final ACK) , used when nmap has access to elevated privileges. Also known as a half open scan
TCP FIN Scan: Sends TCP packets with FIN flag set. If port is closed, the target responds with RST. 
TCP Null Scan: Does not set any bits, closed ports will respond with RST.
TCP XMAS: sets FIN, PSH, and URG flags. Closed ports respond with RST.
FIN,NULL, XMAS are used to identify operating system
TCP ACK scan: only sends ACK packets, used to map out firewall rulesets.


Cyber Kill Chain: 
The cyber kill chain maps out the cycle of an attacker getting into a system
The idea of the kill chain is to stop the attacker somewhere on the chain


Where do vulnerabilities come from? Why do systems have vulnerabilities?

Sources of Insecurity:
Standards
Requirements
Architecture
Design
Implementation
Configuration
Operations
People
Mindset

Security is not often the top of mind for organizations

Configuration Security
The configuration of a network is probably the number one source of access for a hacker
No easy way to view configurations for completeness - misconfigurations ar therefore hidden