Diffie Hellman - allows two entities to agree on a shared key
n is a large prime; g is a number les than n, that are made public.
Alice:
a, g, n
A = g^a mod n
Alice sends g, n, A to Bob.
Bob:
b
B = g^b mod n
Bob sends B to Alice.
Alice: K = B^a mod n
Bob: K = A^b mod n
Trudy is in the middle, and she won't be able to determine what a and b are, but Trudy can instead pretend to be Bob and Alice and form her own keys with both Alice and Bob at the same time. Alice will think she's talking to Bob, Bob will think he's talking to Alice, and both will be talking to Trudy. Diffie Hellman is susceptible to active AITM, and needs to be protected with RSA/certificates that provides authentication. That way, Alice and Bob know they are talking to each other. Diffie-Hellman cannot be used by itself. 

Message Integrity
Encryption: Taking something and transforming it so it is unrecognizable without the key - it can be decrypted back to it's original form with the key.
Hash: Taking a message of any size and creating a fixed size string - message digest. Changing any character will completely change the outcome. 
Hashing functions: MD5 (broken), Sha-1 (also broken) - still sometimes used, but not for security.
Collision: When 2 different inputs produce the same hash value with the same hash function. They should always be different. 

Security Levels of Crytpo Algorithms

| Security Level | Work Factor | Algorithms                              |
| -------------- | ----------- | --------------------------------------- |
| Weak           | O(2^40)     | DES, MD5                                |
| Legacy         | O(2^64)     | RC4, Sha1                               |
| Minimum        | O(2^80)     | 3DES, SEAL, SKIPJACK, RSA-1024, DH-1024 |
| Standard       | O(2^128)    | AES-128, SHA-256, RSA-2048, DH-2048     |
| High           | O(2^192)    | AES-192*, SHA-284                       |
| Ultra          | O(2^256)    | AES-256, SHA-512, RSA-4096, DH-4096     |
\*AES-192 not used in practice

Digital Signature: Requires public key algorithms (public private key pair) - it is a hash of a file that was encrypted (the hash, not the file itself) using the private key. 
To verify a digital signature, make sure that it is correct, the recipient decrypts it with the public key and validate the hash to make sure it matches the hash of the message.

| "Hello this is my secret message" | Hash of message |
| --------------------------------- | --------------- |
This becomes a digital signature
The recipient takes the digital signature, decrypts it using public key, and gets a hash of the message. In email, the public key is sent with the email. In email, there is no easy and no good way to trust public keys. You need to manually confirm that the email comes from who it says it comes from. 
- In a corporate environment using the same CA (e.g., everyone on Outlook at the same company), the CA issues certificates for users.
- Outside of that, when sending a digitally signed email to someone at another company, the sender’s public key is included with the email itself.
- This key contains identifying information like the sender’s email address.
- Verification usually involves manually confirming the key’s authenticity—e.g., calling the sender to check key details—before trusting it.
- There’s no universal CA lookup for these public keys in personal or cross-company email.
- The process for secure email exchange involves first sending a signed message so the recipient can get and verify your public key, and then both sides can exchange encrypted emails.
- The weakness is that the public key comes in the same email as the signature, so without independent verification, it could be spoofed.

Digital Signatures and HMAC are used for similar things, but there are some differences.

| Digital Signatures                       | HMAC                                                                                          |
| ---------------------------------------- | --------------------------------------------------------------------------------------------- |
| Uses public keys for hashing             | Uses symmetric keys                                                                           |
| Keys do not need to be shared in advance | Keys must be pre-shared                                                                       |
| Not as fast as HMAC                      | Extremely fast                                                                                |
| More used for human based discussions    | Things that require extreme speed - for automated processes, computer to computer discussions |

Playback Attacks:
When Alice and Bob are talking, Trudy intercepts the message Alice sends to Bob and sends it again to Bob - Bob doesn't know that the message did not come from Alice. 
Solution: Nonce - number used only once. Alice initiates a conversation with Bob, Bob validates digital signature/HMAc and sends Alice a nonce to be used. Alice sends the message back to Bob with the nonce, and Bob validates that nonce was not used before. If it wasn't, then the message is good.

**Public Key Infrastructure (PKI)**
RSA, Diffie-Hellman, AES serve as backbone of modern cryptography - variations exist.
Public Key Algorithm: RSA
Key Sharing: Diffie-Hellman
Bulk encryption: AES

**PKI** is an infrastructure for numerous services that work together to enable interoperability of digital certificates
More of a framework
When you go to a website, you use TLS, which uses PKI and it enables interoperability across websites.

Reminder: > A digital signature is created by first hashing the message, and then **encrypting the hash with the sender’s private key**.
Validating a digital signature: the recipient decrypts the signature using the public key and validate the hash
A public key is issued by a Certificate Authority - because the key is issued, the recipient has a way to validate the signature
Digital Signatures vs HMAC:
Digital Signatures use public keys
HMAC uses symmetric keys, and are faster
Nonce - Number used only once

Certificate Authorities
On a website, clicking on a lock logo will bring up the certificate.

TLS_AES_128_GCM_SHA256: GCM is mode of operation, the only one that is secure right now. 
TLS v 1.3 - RSA or Diffie-Hellman, or equivalent of them, are always required. That is the reason it is not mentioned in ciphersuite.
Certificate:
Common Name: what the certificate is for (nordstrom.com for example)
Issuer: Who issued the certificate, the CA
Subject Alt Names: The names the certificate is good for: ex: nordstrom.com, \*.about.nordstrom.com

A certificate can be valid for multiple domain names - if the subject alternative name for multiple domains
The purpose of a certificate is to hold the public key
Basic Constraints: says whether the certificate is a CA or not

PKI Certificate Authority:
The Root CA is typically loaded into a browser. Intermediary CA is on the webserver and cached by browser. SSL Certificate is sent with website.
Root CA is so important that it is typically offline. 

Because browser trusts  CA, it will trust certificates issued by CA

SSL/TLS
Transport Layer Security - a protocol built on top of PKI (used for everything)
TLS has been in use for 30 years - it has withstood the test of time.

Types of Certificates:
Domain Validation - ensures ownership of the domain name
Organization Validation - ensures ownership of a company with that domain  (never really used)
Extended Validation - most secure, validate business documents. Name must match, there needs to be a call with CEO of company, etc. 
These mean how much checking the CA does to make sure it is a proper domain?

Ciphersuite Examples When keys are exchanged:
TLS_DHE_RSA_WITH_AES_128_CBC_SHA256
RSA(n,e): step 3 (RSA Public keys)
RSA(n,d): never exchanged RSA Private keys
DH(A,B,g,n,a,b): A,g,n: step 4, B: step 6, a, b: never exchanged, discarded after the session
Key K: "pre-master secret" key generated on step 6 from DH values. PMS + client_random + server_random -> Master Secret, where we get the AES/HMAC keys
This has perfect forward secrecy - if the private RSA key is compromised, all past messages are still protected because nothing is encrypted with that the private key.

TLS_RSA_WITH_AES_128_CBC_SHA256
RSA(n, e): step 3
RSA(n,d): never
DH(A,B,g,n,a,b): not used in this ciphersuite
Key K: "pre-master secret": step 6, encrypted with RSA public key. 
From the PMS, they generate the Master Secret, and from the master secret the AES/HMAC key is created.
This does not have perfect forward secrecy - if the RSA private keys are compromised, then all past messages are compromised - Trudy will be able to decrypt Key K. 

TLS 1.2 vs TLS 1.3:
Approximately 20-25% of TLS connections use TLS v1.3. 
Client Side: starts the ephemeral key exchange, when it sends over supported ciphersuites, it sends over all possible keys. 
Server side chooses which ciphersuite and select which keys to use. It sends over certificates, the signature.
Client then sends over the finished message and the HTTP GET request. 
Because TLS 1.3 isn't supported by appliances, TLS 1.3 is only supported by extensions.
Inspect TLS headers, it will only say it supports TLS 1.3 in extensions. 

TLS is secure because it has been extensively studied over the last few decades. There are some vulnerabilities discovered -authentication issues, issues with infrastructure related to checking certificates. Key exchange issues - web browsers check to make sure keys are strong enough. 
TLS vulnerabilities:
Physical attacks (private keys can be stolen), does not protect against compromised hosts, improper validation of certificate fields
Adversary-in-the-Middle Attacks:
A fake root CA can be installed on a target computer - if this happens, attackers can masquerade as any website with a TLS certificate. 
