
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


Exercise #1: How would a nonce help to solve the bank transfer problem?
Without a Nonce, Trudy can capture a message and send it again to the bank, and the bank would have no way of validating if the message was new or old.
With a nonce, the bank has a way of validating if the message was sent before. 


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


Exercise #2
Suppose Prof. Mak sent you a digitally signed email using a valid X.509v3 certiifcate. Is there any reason not to trust it? (Hint: Yes)
CAs for emails don't have a Certificate trust store to ensure that they are valid CAs
Private key might have been leaked, someone could be impersonating Prof Mak, the CA itself might not be trustworthy
The only way to verify is by using some kind of offline checking


PKI Certificate Authority:
The Root CA is typically loaded into a browser. Intermediary CA is on the webserver and cached by browser. SSL Certificate is sent with website.
Root CA is so important that it is typically offline. 

Because browser trusts  CA, it will trust certificates issued by CA


Exercise #3:
When you go to https://www.nytimes.com, how does the web browser verify the TLS certificate is valid?
The browser needs to follow the chain of trust. The web browser checks if the certificate is valid, it checks if the certificate was issued by a trusted CA, it verifies that certificate.


Exercise #4:
What is the price of a TLS certificate? 1-year cert
Why do you think the price of TLS certificates vary so much?
A price of a TLS certificate can vary, it can be free to hundreds of dollars per year. The price probably varies because the level of validation and number of domains it supports probably varies from price to price. 


Shorter certificate periods are more secure - less time to be compromised


SSL/TLS
Transport Layer Security - a protocol built on top of PKI (used for everything)
TLS has been in use for 30 years - it has withstood the test of time.

Types of Certificates:
Domain Validation - ensures ownership of the domain name
Organization Validation - ensures ownership of a company with that domain  (never really used)
Extended Validation - most secure, validate business documents. Name must match, there needs to be a call with CEO of company, etc. 
These mean how much checking the CA does to make sure it is a proper domain?

TLS is a protocol that runs on top of TCP. When you go to a website, your computer first makes the TCP connection with the handshake. After the connection is made, the TLS connection is made. 

TLS architecture consists of 4 different "records" - Handshake, change cipher spec, SSL Alert Protocol, Application Data (HTTP, FTP).

TLS <= 1.2
When you go to a website, the client will initiate with a ClientHello to the website. It will contain specifications that the web browser (Ex: Chrome, Firefox) supports. Protocols (version), ciphersuites (set of crypto algorithm), compression, a random number (client_random), session ID (for resumptions) and options.
The server responds with the ServerHello - it selects specifications it supports - which TLS algorithm, which cryptography algorithm. 
The server then sends over its certificate. 
Server sends over its side of the key exchange - for ex, if Diffie Hellman is used, it is the A, g, n keys. 
Then the server sends ServerHelloDone - it is done.

The client key exchange is done, where the client sends over it's side of the key exchange - in Diffie Hellman, B is sent over. 
Client's ChangeCipherSpec: ready to start encryption, everything is good.
Client's Finished Message: Hash of all prior handshake message, encrypted. 
Server's ChangeCipherSpec: ready to start encryption.
Server's Finished: hash of all prior handshake messages, (except for changecipherspec messages), encrypted.

TLS Handshake: after the TCP handshake
Client Side:
1. ClientHello: After the TCP connection, the browser sends over specifications that it supports. Version, cipher suites, compression (never), random #
Server Side:
2. ServerHello: Server selects the specification - which version of TLS (1.2, 1.3 ex), selects ciphersuites, encryption algorithms. Sends over a server_random #
3. Certificate: server sends over its side of the certificates, it's certificate and the issuer's certificate
4. ServerKeyExchange: Server sends over it's side of the key exchange. For ex, if Diffie-Hellman is used, the values of "A, g, n" are sent over
5. ServerHelloDone: server is done
Client Side:
6. ClientKeyExchange: Client sends over it's side of the key exchange. For example, if DH is used, the value of "B" is sent over. Browser calculates key K. On the server side, it also calculates key K, because they will both have the information needed. 
   K + client_random(step 1) + server_random (step 2) -> AES Key + HMAC Key
   Done on both sides
7. ChangeCipherSpec: Everything has been okay, starting encryption in the next message
8. Finished: first encrypted message, has a hash of all prior handshake messages (1 through 6). Since it is after ChangeCipherSpec, it is encrypted.
Server Side:
9. ChangeCipherSpec: Server receives encrypted message on step 8, decrypts message using AES  + HMAC Key, if everything checks out (validates that it sees the same hashes. Sends a ChangeCipherSpec to start encryption.
10. Finished: Finished message has a hash of all prior handshake message (step 1 -> 6, 8). Client reads encrypted message using the AES+HMAC Key. If everything is good, they start the HTTP communication. 

TLS Abbreviated Handshake:
Removes certificates exchanges and keys, remembers what was done last time and resumes the connection. 

Ciphersuites
TLS_DHE_RSA_WITH_AES_128_CBC_SHA256

DHE_RSA: Means the Diffie Hellman exchanged is being protected with RSA. DH can never be used by itself because it is always presumed there is an attacker in the middle. The values exchanged, g, n, A, B, needs to be protected. This is the key exchange/authentication algorithm
AES_128_CBC: AES-128 using the CBC mode (mode of operation), cipher block chaining. This is the bulk encryption algorithm.
SHA-256: HMAC key This is the HMAC algorithm. 

TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256

ECDHE_RSA: Elliptic Curve Diffie Hellman Ephemeral, key exchange
AES_128_CBC: Bulk encryption
SHA-256, HMAC

If it is DH_RSA, it is Fixed Diffie Hellman, and that means the Diffie-Hellman value from the server is always the same. 