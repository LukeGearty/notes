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

Exercise #1: If the ciphersuite TLS_DHE_RSA_WITH_AES_128_CBC_SHA256 was used, where are the following keys exchanged?
1. DHE(A, g,n , B, a, b)
	1. In steps 4 (A, g, n), signed with server's private RSA keys, step 6 (B), a and b are never exchanged
2. AES keys: 
	1. Never exchanged, AES keys are created in step 6 (along with HMAC key)
3. RSA Public
	1. Step 3, public certificate contains RSA public key
4. RSA private
	1. Never exchanged, generated and stored on a server ideally in the Trusted Platform Module
5. Public cert
	1. Step 3
6. Private cert
	1. Never exchanged

Perfect Forward Secrecy: If a key is compromised, only the specific session it protected will be protected. The security of past or future encrypted sessions is not affected. 
Keys are securely deleted after use. Without these keys, there is no way captured cipher text can be decrypted.

When the server sends over the Diffie Hellman keys in step 4, what is protecting it? Trudy is assumed to always be in the middle watching the conversation. 
The A, g, n values are signed with the server RSA private keys (n, d). The browser validates the digital signature. The browser uses the public RSA key, sent over with the certificate, to validate the digital signature. 
How do we know the certificate on step 3 isn't changed by Trudy? The browser checks all the values in the certificate and checks the certificate all the way to the Root CA. Then when the server sends over the A, g, n values, it has to be signed with the private RSA keys, and the browser validates it with the RSA public key. 


How is the value of B, sent by client to server, protected from Trudy?
It could be encrypted using RSA public keys - not effective because Trudy could do the same thing.
It is not digitally signed - digitally signed is encrypted using private key.
It is actually sent in plain text.
It is okay, because there is a finished message with a hash of all prior handshake messages (step 1 through 6) - the hash itself is encrypted using AES and protected by HMAC, and the AES and HMAC keys are generated by the A, g, n, and B values. Once this is sent, the server verifies that it using the same keys as it is supposed. 

How is Key K protected when it sends over the Key in step 6? 
It must be encrypted - the public RSA key is used to encrypt  (When Diffie Hellman is not used)


Exercise #2: 
a. What TLS record type for each message?
1. ClientHello - SSL Handshake Protocol
2. ServerHello - SSL Handshake Protocol
3. Certificate - SSL Handshake
4. ServerKeyExchange - SSL Handshake
5. ServerHelloDone - SSL Handshake
6. ClientKeyExchange - SSL Handshake
7. ChangeCipherSpec - SSL Change Cipher Spec Protocol
8. Finished - SSL Handshake
9. ChangeCipherSpec - SSL Change Cipher Spec Protocol
10. Finished - SSL Handshake
b. Which messages are encrypted or not encrypted?
Steps 8 and 10 are encrypted, everything else is not


