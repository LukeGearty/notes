# Final Exam Review

Final Exam Details
Saturday, August 16, starting time 10 AM - NOON ET
Final only topics:
	Lecture 6, 7, 8
	Weekly Videos #8 - 12 & Firewall Video
	Lab 3 & 4
	HW #3 TLS and HW #1 - Immersive Labs
	Reading Materials
	2.5
	Open Book, but no free internet - need to print things out
Access to predetermined websites will be allowed
Lockdown Browser


Diffie Hellman
a = 6
g = 5
n =23
b = 15

A = g^a mod n
K = B^a mod n

B = g^b mod n
K = A^b mod n
Always presume that Trudy is watching - if she sees just the public values (g, n, A, B), she would not be able to calculate a, b, and key
Which values need to be protected?
a, b, K - secret values, they're never sent over. When a computer generates these values, they're generated in TPM module. Once it is created and calculations done, it can be discarded
Do the values exchanged need to be encrypted or just signed?
Just signed.
If Trudy is in the middle, she can pretend to be the other person and form her own key values with both sides of the conversation. Trudy would need to be an active attacker in the middle to perform .
There needs to be some kind of authentication with Diffie Hellman. 

Encryption vs Hashing
Encryption: keeps communication private, decryption can use same or different key, achieved by various algorithms, need key management
Hashing: transforms message into fixed size digest

Digital Signature vs HMAC: Digital signatures use public keys, HMAC uses symmetric keys. Both are used for the same message integrity goals. Digital signatures are slower, but you don't need to create symmetric keys.
Digital Signatures: encryption of a hash using the private key

Playback Attacks
Digital Signatures and HMACs are susceptible to playback attacks. The recipient cannot distinguish between original communication and later playbacks. 
Nonce is a number used only once
can defend against playback attacks - sender sends a message, recipient gives Alice a nonce to use, sender sends the message again with Nonce that was encrypted with sender and recipient symmetric shared secret key and sends encrypted Nonce back

In TLS 1.3 there is no key exchange algorithm mentioned because it always uses a key exchange algorithm with Diffie Hellman and RSA like algorithms. 
Ex: TLS_AES_128_GCM_SHA256 TLS 1.3

Types of Certificates:
DV - Domain Validation, only checks that you own the domain
OV - Organization Validation - ensures that a real company owns and registered
EV - Extended Validation - do a real check to ensure that it is the real website. Real check meaning a call with executives

The browser uses the public key to check that the private key of the CA signed the certificate
When browser gets to the Root CA, the final check is if the certificate is the root store - the certificates the browser trusts
The certificate is issued by an intermediary CA, whose certificate is issued by a Root CA - everything the Root CA is trusted

How is the certificate for www.google.com validated by the browser?
www.google.com sends the certificate, browser checks the chain of trust to see if it trusts issuer. 
What are some criteria the browser uses to validate the certificate?
Common name for issuer's certificates, not the server's certificates (it checks subject alt names)

Certificate Issuance
User goes to Certificate Authority to request a certificate
Authority performs business and technical checks on the requester
CA generats and issues certificate
There is no standard on who becomes a CA
	Trust of a root CA depends on browsers/OS

Certificate Validation
Browser download's servers certs, checks Subject alt names, validation dates


Perfect Forward Secrecy
Whether past messages get compromised if a key is compromised. If PFS is achieved, then past messages are unencryptable. 
All ciphersuites for TLS 1.3 have perfect forward secrecy. 


SSL Architecture
Different records: handshake protocol, change cipher spec protocol, ssl alert protocol, applications data (HTTP, FTP)
TLS is on top of TCP, which is on top of IP


up to TLS v1.2 SSL Full Handshake
Client:
1. ClientHello - sends version, ciphersuite, client_random, session ID
	In TLS v1.3, it would still say it supports v1.2, but there will be an extension that says it supports 1.3
Server:
2. ServerHello: selects ciphersuite, version, sends server_random
3. Certificates
4. ServerKeyExchange - it's side of the key exchange. If it is using Diffie Hellman, it sends over A, g, n
5. ServerHelloDone
Client
6. ClientKeyExchange: It's side of the key echange. In DH, it sends over B. It figures out Key K (server does this as well)
7. ChangeCipherSpec: start encryption
8. Finished: hash of messages step 1 through 6, encrypted, server is able to decrypt it and verify it
Server
9. ChangeCipherSpec: start encryption
10. Finished: Hash of prior messages, steps 1 through 6 and 8, encrypted. Client is able to decrypt it and verify it
If RSA is only used (no PFS), step 4 is not used - the key is encrypted and sent over to the client side

Abbreviated TLS Handshake
Client sends over everything with Session ID from a previous section. Server gets it, and immediately changes to encryption and sends a finished message. This is done after a TLS session was done before. 

Common Types of Keys (Examples)
Key exchange and authentication protocols
RSA: client encrypts random secret key with server's RSA key in certificate
DH_RSA (Fixed Diffie hellman, non-ephemeral), server uses the same "A" each time, specified in the certificate
	Signed with server's RSA public key
DHE_RSA (Ephemeral Diffie-Hellman)
ECDHE_RSA - variant of DHE_RSA except it uses the Elliptic Curve concept

Symmetric Protocols: RC4, AES, DES
Hashing: SHA, MD5

In TLS_DHE_RSA_AES_128_GCM-SHA256: TLS 1.2 & Below

RSA(n, e): Step 3
RSA(n, d): never
DH(A, g, n): step 4
DH (B): step 6
DH (a, b): never sent
DH(A, g, n) are signed
Key K: generated at step 6 - pre-master secret, used to generate AES and HMAC key

In TLS_RSA_AES_128_GCM_SHA256
RSA(n, e): Step 3
RSA(n, d): never
DH(A, g, n, B, b): DH is not used
Key K: pre-master secret, generated in step 6
PMS + client_random + server_random = master secret
master secret goes through a secret to generate AES /SHA256 HMAC


Updates in TLS 1.3
Conversation is shorter because at the beginning, the client sends over all the information immediately. ClientHello - key exchange is part.
Because there are a lot of legacy appliances that don't support TLS 1.3, TLS v1.3 lives inside the extensions of TLS v1.2 as support version information

Firewalls
Packet Filters: fast, stateless, traditional firewalls
Stateful Firewalls: Fw that maintain connection info
Proxies: a server used as an intermediary on a  network such as web, email, FTP
Bastion Host: Host intended to withstand attacks
DMZ: the area of a network where outward facing servers are exposed
Access Control Lists: another term for packet filter firewall, commonly used when referring to non-firewall

If Alice was using the company-issued computer, how can the web gateway view and capture encrypted traffic? How?
A certificate issued by the web gateway is installed in Alice’s machine and the traffic can be decrypted by using the private key stored in the web grateway

If Alice was using her own computer, how can encrypted traffic be captured?
SSL strip downgrades connection

Iptables:
Input, output, forward chains
Can run in two different modes:
1. As a network appliance
2. Working like an endpoint

Connection tracking:
-m conntrack --ctstate NEW,ESTABLISHED

NEW: Allows new connections
Established: Allow established sessions to receive traffic
Related: Allow related established connection, e.g., FTP 20/21

Options:
-p protcol type (tcp, icmp, udp)
-s source IP and port
-d dest IP and port
--sport source port
--dport dest port
-j target (Accept, Deny, Drop)
--icmp-type (echo-request, echo-reply)

Firewalls need to send syslog messages (TCP 6514) to SIEM (10.10.111.0)
Destination Port: 6514
Destination IP: 10.10.111.0

Firewall 1:
eth0:
10.10.111.2
eth1: 10.10.111.3

Firewall 2:
eth0:
10.30.111.2
eth1:
10.30.111.3

Default policies for both firewalls (DON'T FORGET THIS):
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP

Firewall 1:
iptables -A OUTPUT -p tcp -s 10.10.111.2 -d 10.10.111.10 --dport 6514 -o eth0 -j ACCEPT -m conntrack --ctstate NEW,ESTABLISHED
iptables -A INPUT -p tcp -s 10.10.111.10 -d 10.10.111.2 --sport 6514 -i eth0 -j ACCEPT -m conntrack --ctstate ESTABLISHED

iptables -A FORWARD -p tcp -s 10.30.111.2 -d 10.10.111.0 --dport 6514 -i eth1 -o eth0 -j ACCEPT -m conntrack NEW,ESTABLISHED
iptables -A FORWARD -j ACCEPT -m conntrack --ctstate ESTABLISHED

Firewall 2:
iptables -A OUTPUT -p tcp -s 10.30.111.2 -d 10.10.111.10 --dport 6514 -o eth0 -j ACCEPT -m conntrack --ctstate NEW,ESTABLISHED
iptables -A INPUT -p tcp -s 10.10.111.10 -d 10.30.111.2 --sport 6514 -i eth0 -j ACCEPT -m conntrack --ctstate ESTABLISHED