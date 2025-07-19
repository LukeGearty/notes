Lesson Objectives: Understand:
how nonces improve authenication
RSA, DH, AES are the backbone of Internet comm
Apply modern crypto concepts to solve scenarios
Understand how PKI certs are issued and understand perfect Forward Sec
Understand protocol level details of a TLS connection
Identify vulns in PKI/TLS systems

Diffie-Hellman:
Everything requires a Diffie-Hellman exchange - an essential part of modern cryptography
Allows 2 entities to agree on a shared key, but does not provide encryption
n = large prime number
g = number < n

a, g, n is chosen by one party.
A = g^a mod n
Send to second party:
g, n, A
Second party chooses b
B = g^b mod n
K = A^b mod n
Sends B to first party
First Party: K = B^a mod n

The problem is that DH needs to be protected: (g, n, A, B) if they are modified, then key K can be modified.

How to stop DH from being susceptible to MITM attacks: DH always needs to be used with something that provides authentication. Something like RSA, certificates. Key exchanges need authentication.

Lab 3
ARP: Before a machine can talk to another machine on the same subnet, an ARP request that asks for the MAC address for that IP address will be performed. The request asks "Who has 10.0.0.1" (ex), and is broadcast to all machines on the subnet. The machine that matches the IP address replies to the request, only to the requesting machine. 
A Gratuitous ARP reply is broadcast to the whole network. This is a massive vulnerability (a long time ago) because it allows to update ARP cache for everyone on subnet. 

4 types of ARP packets:
Gratuitous ARP Request - normal "Who has 10.0.0.4". 
Gratuitous ARP Reply - does not work on modern OS, not going to try. 
Unicast ARP Request - not normal
Unicast ARP Reply - normal

Lab assignment is to try to perform an ARP cache poisoning. Which ones work for arp cache poisoning, which does not.

Arp Request/Reply Example:
"Who has B IP? Tell A IP/ A MAC"
Ethernet Header:
Dst Mac: ff:ff:ff:ff:ff
Src MAC: A MAC
ARP Header:
Type: Request
Sender MAC: A MAC
Sender IP: A IP
Target MAC: dst Mac
Target IP: B IP

Reply: 
B IP is at B MAC
Ethernet Header:
Dst Mac: A MAC, if gratuitous: ff:ff:ff:ff:ff
Src MAC: B MAC
ARP Header:
Type: reply
Sender MAC: B MAC
SENDER IP: B IP
Target MAC: A MAC
If gratuitous: ff:ff:ff:ff:ff
Target IP: A IP 

ARP Attack in Action
Router 10.1.1.1 and MAC A, switch, and a machine with 10.1.1.2, MAC B
Attacker with IP 10.1.1.1 and MAC C
It first poisons the ARP for the router, to tell the router that the MAC address for 10.1.1.2 is actually MAC C
The attacker then sends a message to the victim that the router's MAC is MAC C.
Both sides of the conversation must be poisoned - if it isn't, then the router will try to fix once it sees a packet go in a loop

Exercise A1:
Poisoning the router with the ARP Reply (Unicast)
Ethernet Header
Source MAC: MAC C
Dst MAC: MAC A
ARP Header:
Sender MAC: MAC C
Sender IP: 10.1.1.2
Target MAC: MAC A
Target IP: 10.1.1.1

Exercise A2:
Poisoning 10.1.1.2, MAC B with ARP Reply (Unicast)

Ethernet Header
Source MAC: MAC C
Dst MAC: MAC B
ARP Header
Sender MAC: MAC C
Sender IP: 10.1.1.1
Target MAC: MAC B
Target IP: 10.1.1.2

Task 2: 
Step 1: Launch Scapy Program AITM Attack
Step 2 & 3: Test to make sure it works, turn ip_forward on/off (show proof)
Step 4:
	Launch AITM attack
	Establish telnet
	Turn off ip_forward
	Replace characters Q with Z

Task 3:
Same as above using netcat instead

Submit:
Create attack
capture attack on wireshark
save it as netID.pcap
Create a settings.txt file
Upload those files to gradescope


-----

When you go to starbucks, you are susceptible to an ARP cache poisoning - it is pretty easy to do


------
Message Integrity: Allows communicating party to verify that received messages are authentic

Encrypt: keeps communications private, using same or different keys, needs key management, is reversible

Hashing: transforms message into fixed-size string, one-way hash function, strongly collision-free hash, viewed as a "digital fingerprint". Used for message integrity check and digital certificates, faster than encryption. 

Hash Functions
MD5 (broken)
SHA-1 (also broken)
These two are not collision resistant. Collision: when two different inputs produce the same output

Exercise #1:
What's the minimal recommended strength for each of the following algorithms:
-AES: 128 bits, 256 preferred. Technically there is no difference, so no reason why an application can't support 256 over 128. 128 is okay for legacy.
-DH: 2048 bit, 4096 for highly secure
-RSA: 2048 bits, 4096 for highly secure
-SHA (Which version):  => Sha-2 256 bits. Also Sha-3

When you are calculating DH, RSA, etc, it is difficult to figure out the prime numbers needed. Quantum computing threatens to break that by factoring the values extremely fast.

Digital Signature: a method to verify the authenticity and integrity of digital messages or documents
Requires Public Key algorithms. It has a public key and a private key. 
hash of a file encrypted using the private key. 
message -> hash of message -> encrypted using private key to create digital signature
To evaluate that the digital signature is from the right person, the digital signature is decrypted using the public key. The recipient takes the digital signature, decrypts it using public key, gets a hash of the message. 
Validating a digital signature:
Take a hash of the original message and decrypt the digital signature, compare hash
Getting another person's public key, it is sent with the email 
One of the properties of a public key is to say what email address it is associated with

HMAC vs Digital Signatures
Digital Signatures use public key while HMAC uses symmetric keys. Other than that they are extremely similar. 
HMAC keys must be pre-shared, digital signature keys don't. 
HMAC is extremely fast.Digital Signatures are slower than HMAC.
HMAC is used for automated processes, digital signatures are more used for human interactions. 

Playback Attack: An attacker intercepts communication and resends it.

Nonce is a number used only once
Someone sends a message, the recipient sends back a nonce, and the sender resends the message with the nonce
The recipient validates the HMAC/digital signature and validates the transfer
Nonce ensures that replay attacks/playback don't work
1. **1.** **Unique Number Generation:**
    When a user or system initiates a communication, a random or pseudo-random number (the nonce) is generated and included in the message. 
2. **2.** **Message Transmission:**
    The message, along with the nonce, is then sent to the intended recipient. 
3. **3.** **Verification:**
    The recipient system stores the nonce and checks it against previously received nonces. If the nonce has already been used, the message is rejected as a replay.


Exercise #2: 
Explain the difference between these terms: 
Symmetric key cryptography: cryptography that utilizes one single shared key
Public Key Cryptography: Cryptography that utilizes a publicly available key and private key
Hashing: A function that takes a message of any size and transforms it into a string of a fixed size

Exercise #3:
What are some ways to use symmetric keys, public keys, and hashes to create a message encrypt scheme? (Also list bad answers)
1. Sender: encrypts message with symmetric key
	Receiver: Decrypts message with same key
2. Sender: uses a secret key to encrypt the message, encrypt the secret key with the recipient's public key, send both to the recipient

Exercise B: Please find the cost of a one-year TLS certificate. Note the vendor and type of certificate issued.
Can range from free to over $2000.
Free: Let's Encrypt, basic Domain Validation. It certifies that you own a specific domain name.
DigiCert: Can be around $100 - $200 per month, depending on the need. 

Exercise C: Explain in detail how Diffie-Hellman is susceptible to an AITM attack. 
An attacker can intercept the public key exchange between two parties and modify the keys.