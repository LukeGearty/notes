

TLS Handshake: after the TCP handshake
Client Side:
1. ClientHello: After the TCP connection, the browser sends over specifications that it supports. Version, cipher suites, compression (never), random #
Server Side:
2. ServerHello: Server selects the specification - which version of TLS (1.2, 1.3 ex), selects ciphersuites, encryption algorithms. Sends over a server_random #
3. Certificate: server sends over its side of the certificates
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