Introducing Alice and Bob, two people who want to communicate securely. (They can be client-server, two routers, anything really)
What does security mean?
It means their communication is secret. There is confirmation they are talking to each other. Their messages aren't tampered with. 

# What is Network Security
Networks are an insecure medium. There are a few desirable properties of secure communication.
Confidentiality: only sender and receiver should understand the contents of the transmitted message. It must be encrypted.
Message Integrity: The messages are not altered in transit, either accidentally or maliciously.
End-Point Authentication: Sender and receiver confirm the identity of the other party.
Operational Security: devices that are used to counter attacks against an organization's network like firewalls, IDS, etc.

Intruders can eavesdrop or modify messages or message content. Unless apropriate countermeasures are taken, intruders can steal data, impersonate another entity, hijack an ongoing session, deny service to legitimate network users, etc. 

# Principles of Cryptography
Reading: The Codebreakers, The Code Book: The Science of Secrecy from Ancient Egypt to Quantum Cryptography.
Plaintext: The original message, also called cleartext.
Encryption Algorithm: Used to transform the plaintext to the ciphertext to make it look unintelligble to anyone. Encryption techniques are often known to everyone including the potential attackers.
A key is a string of numbers or characters that acts as input to the encryption algorithm. The sender uses that key to encrypt the message. Key_A(m) refers to the ciphertext form of the plaintext message, m. The receiver provides a key, Key_B, to the decryption algorithm to get the plaintext message back. Key_B(Key_A(m)) = m.
In symmetric key systems, Key_A = Key_B. In Public Key systems, two different keys are used. One key is known to everyone, the other key is private to the individual. 

## Symmetric Key Cryptography
Caesar Cipher takes each letter in the plain-text message and substituting the letter that is k letters later, allowing wraparound. If k = 3, Hello World becomes Khoor Zruog. 

Monalphabetic cipher substitutes one letter of the alphabet with another letter of the alphabet. Any letter can be substituted for another letter. A -> M, B-> N, C->B, etc. There are 26! (on the order of 10^26) possible pairings. 

The problem is that, for the English language, the letters e and t are the most frequently occuring letters in typical text. Also, if the intruder has some knowledge of the message (say something that will definitely occur), then that is also a potentially weakness.

Three different scenarios:
-Ciphertext-only attack: the attacker only has access to the intercepted ciphertext.
-Known-plaintext attack: If the attacker knows for sure that certain words (like Love in a love letter) will occur, then the attacker can determine plaintext for at least a portion of the message.
-Chosen-plaintext attack: The intruder can choose the plaintext message and obtain the ciphertext. If a message with every single English letter is sent, "The quick brown fox jumps over the lazy dog", for example, then the encryption scheme can break.

Polyalphabetic Encryption uses multiple monalphabetic ciphers in different parts of the message. For example, a message can be encrypted with a Caesar Cipher with two keys, C_1(k=5), C_2(k=19). The first letter may be encrpyted with C_1, the second with C_2, and so on. 

### Block Ciphers
In a block cipher, the message to be encrypted is processed in blocks of k bits. For ex: if k = 64, the message is broken into 64-bit blocks and each block is encrypted independently. 
In a 3-bit block example, there are 3-bit inputs mapped to 3-bit outputs. For example, 000 -> 010. There 2^3 = 8 possible inputs for this, and these eight inputs can be permuted in 8! = 40,320 ways. This can be easily bruteforced, so this can be mitigated when k is larger. (2^k)! i very large even when k isn't that large.
Problem is that a table would be needed to decrpyt it, which would get more difficult as k grows. If the keys change, then a new table needs to computed.
Block ciphers instead use functions that simulate randomly permuted tables. For ex, if k=64, the function breaks a 64-bit block into 8 chunks of 8 bits, and each 8-bit chunk is processed by an 8-bit to 8-bit table. Then the 8 output chunks are reassembled into a 64-bit block. The positions of the 64 bits in the block are then scrambled to produce a 64-bit output. This output is fed into the 64-bit input, where another cycle begins. 

### Cipher-Block Chaining
A problem with the chopping up the message into k-bit blocks is that two or more of the cleartext blocks can be identtical. For example, cleartext in two or more blocks can be "HTTP/1.1". The same ciphertext is produced. An attacker could guess that.
Some randomness can be mixed in. 
M(i) = the ith plaintext block.
C(i) = the ith ciphertext block.
A XOR B is the XOR of two bit strings. 
K_S = block-cipher encryption algorithm with Key S.
The sender sends a random k-bit number r(i) for the ith block and calculates c(i) = K_s(m(i) XOR r(i)). A new k-bit random number is chosen for each block. 

## Public Key Encryption
A problem with shared key is how the two communicating parties agree on the key - that communication also requires secure communication. 
Say Alice and Bob are communicating. Rather than sharing a single secret key, Bob has two keys - a public key available to everyone in the world, including a potential attacker. He also has a private key known only to Bob. In order to communicate with Bob, Alice first fetches Bob's public key and uses a known encryption algorithm to encrypt a message. Bob receives the message and uses the private key and a known decryption algorithm to decrypt the message. 
A problem is that since Bob's encryption key is public, anyone can send an encrypted message to Bob, including someone pretending to be Alice. Digital signatures (8.3) are needed.

### RSA
RSA is almost synonymous with public key cryptography. 
RSA uses modulo-n arithmetic. X mod N means the remainder of x when n is divided by n. 
In modular arithmetic, the usual operations (addition, subtraction, multiplication, exponentation) are performed, and the result of each operation is replaced by the integer remainder that is left when the result is divided by n. 

To generate the public and private RSA keys, these are the following steps.
1. Choose two large prime numbers, p and q. The larger, the more difficult it is to break but the longer it takes to perform encoding and decoding.
2. Compute n = pq and z = (p -1)(q - 1).
3. Chose a number, e, less than n, that has no common factors (other than 1) with z. This is used in encryption.
4. Find a number, d, such that ed -1 is exactly divisible by z. This is used in decryption. Given e, we chood d such that ed mod z = 1.
5. The public key is the pair of numbers (n, e). The private key is the pair of numbers (n, d).

### Session Keys
The exponentiation required by RSA is time-consuming. RSA is often used in practice in combination with symmetric key cryptography. 
Say Alice wants to send Bob a large amount of encrypted data. She chooses a key that will be used to encode the data (session key), denoted by K_S. Alice must inform Bob of the session key. Alice encrypts the session key using Bob's public key. Bob receives the RSA encrypted session key, and decrypts it to obtain the session key


# Message Integrity and Digital Signatures
Message integrity/authentication basically says the message originated from the correct sender and was not tampered with on the way to receiver. 
Consider OSPF, broadcasting link-state messages from router to router. An easy attack could be distributed bogus link-state messages.
### Cryptographic Hash Functions
A hash function takes an input, m, and computes a fixed-size string H(m) known as a hash. Think of a checksum/CRC. It should be computationally infeasible to find any two different messages, x and y, such that H(x) = H(y). 
### Message Authentication Code
Hashes can be used to perform message integrity. Say Alice is sending a message to Bob.
Alice creates message m and calculates the hash H(m).
Alice then appends H(m) to the message m, creating an extended message (m, H(m)) and sends the extended message to Bob. 
Bob receives an extended message (m, h) and calculates H(m). If H(m) = h, Bob concludes everything is fine. 

This is flawed because an intruder can create a bogus message, m', and say she is alice, calculate H(m') and send that to Bob. Everything would still check out.

To perform message integrity, there needs to be a shared secret s (a string of bits) called the authentication key. 

Alice creates message m, concatenates s with m to create m + s, and calculates hash H(m + s), called the message authentication code.
Alice then appends the MAC to the message m, creating an extended message (m, H(m + s)) and sends it to Bob.
Bob receives message (m, h) and knowing s, calculates the MAC H(m + s). If H(m + s) = h, everything is fine.
A standard of MAC is HMAC. 

### Digital Signatures
A digital signature is a cryptographic technique for achieving these goals in a digital world. 
Suppose Bob wants to digitally sign a document, m. Bob uses his private key to generate the digital signature of m, which he sends to Alice.
Alice takes Bob's public key, and applies it to the digital signature associated with the document. If it produces m, it exactly matches the original document. 
Signing data by encryption is computationally expensive. A more efficient approach is to introduce the hash function into the digital signature. Bob instead signs the hash of the message rather than the message itself. 

### Public Key Certification
Certifying that a public key belongs to a specific entity. Verifying that the entity you are communicating with is actually that entity.
This is done by a Certification Authority who validates identifies and issue certificates.
A CA verifies the entity is who it says it is. Once the identity is verified, the CA creates a certificate that binds the public key of the entity to the identity. 

# End-Point Authentication
The process of proving its identity to another entity over a computer network.
Humans authenticate through: voices, faces, driver's licenses, etc.
Networks can't do it exactly like that, so it is instead done through the authentication protocol, run before the communicating parties run some other protocol. 
Simplest authentication protocol: Alice sends a message to Bob saying she is Alice. 
Flaw: Anyone can say I am Alice.
We could authenticate from an IP address. It's not hard to fake IP addresses though.
We can use a secret password, but if that password gets out...
We can encrypt that password, but that could be subject to other kinds of attacks, like playbacks if the intruder captures the encrypted password.
A failure scenario is that Bob can't distinguish original authentication from later playbacks. A TCP handshake also addresses this problem - if the TCP server receives a copy of a SYN segment from an old transmission, how does it determine whether the client is live? It chooses an initial sequence number that hasn't been used for a long time and sends that to the client, and waits for the ACK.
A nonce is a number that will be used once in a lifetime. 
Alice sends the message "I am Alice " to Bob
Bob chooses a nonce, R, and sends it to Alice.
Alice encrypts the nonce using Alice and Bob's symmetric shared key, and sends the encrypted nonce back to Bob. 
Bob decrypts the nonce to determine if the decrypted nonce matches the original nonce.
