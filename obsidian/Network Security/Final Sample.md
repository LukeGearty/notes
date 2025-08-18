Alice purchased goods from Trudy's website. Alice wants to transfer $100 (one hundred dollars) to Trudy. Alice has a public and private key pair that the bank already knows the fingerprint of. Alice sends a digitally signed message to the bank stating that $100 should be transferred from
her account to Trudy’s account. The bank successfully authenticates Alice’s digital signature and performs the transfer.
1a. [4 pts] As stated above, how would Alice generate the digital signature?
Alice would take a hash of the entire message she sent to the bank and encrypt it to generate the digital signature.

1b. [4 pts] How can Trudy (who is evil) take advantage of this system to get herself more money? 
Trudy can perform a replay attack - intercepting Alice's message and capturing it, and repeatedly sending it to the bank.

1c. [4 pts] How can the communication be modified to prevent this attack?
The bank can provide Alice a Nonce that Alice can send with the message - the bank will validate both the digital signature and the nonce, and the nonce can only be used once before it is invalid. 


Message Integrity. Suppose Alice purchased something from Trudy on eBay, and now needs to send $100 to Trudy. Alice would send a message to the bank to make the transfer. The bank requires Alice’s password in the message, as follows:

Alice -> Bank: Hello
Bank -> Alice: Use this nonce R
Alice -> Bank: encrypt_using_bank’s_public_key(“Transfer $100 to Trudy”, R, Alice’s_password)

1a. [4 pts] How would Trudy use this protocol to steal money from Alice? Explain in detail.
Trudy can intercept the message and modify it to steal money from Alice.

1b. [2 pts] What information does Trudy need to capture in order for her attack to work?
Trudy would need to get Alice's password and the nonce R in order for her attack to work.

1c. [4 pts] How does the bank know that Alice is the original entity performing this request?
As the protocol currently is, it uses Alice's password to verify her identity.

1d. [4 pts] Describe in detail two ways to improve this protocol
Alice sends a digital signature of the message to the bank in order to verify her identity.
The bank verifies that the nonce used has not be used before, along with verifying the digital signature. 


1a. [4 pts] Suppose that in the SSL Full Handshake, as shown, the Finished messages do not contain a checksum of all previous handshake messages. Describe two ways that an attacker can take advantage of this flaw.
An attacker can modify any message before and there would be no way of verifying that the messages had not been tampered with

1b. [4 pts] Describe each algorithm of this ciphersuite its purpose in SSL/TLS:
TLS_DHE_RSA_WITH_AES_128_CBC_SHA
DHE: Diffie Hellman Ephemeral, the a and b values are newly generated each time, this is signed with server's RSA public key
RSA: server uses RSA private key to sign the certificate
AES_128_CBC: Bulk encryption, using AES with key length of 128 bits in CBC mode
SHA: uses SHA-1 for message integrity

1c. [2 pts] What’s the primary security difference between these two ciphersuites:
TLS_DHE_RSA_WITH_AES_128_CBC_SHA and
TLS_DH_RSA_WITH_AES_128_CBC_SHA

DHE is Diffie Hellman Ephemeral, and it generates new a and b values each time. DH uses the same A value each time. 


2a. [3 pts] What messages are hashed by each of the Finished messages in the SSL Full
Handshake? Be specific.
In the first Finished message in step 8, it takes a hash of messages 1 through 6. In the second finished message in Step 10, it takes a hash of 1 through 6 and 8.

2b. [3 pts] When is the first encrypted message sent from each side in the SSL Full Handshake?
Client: step 8
Server: Step 10

2c. [3 pts] What is the SSL Abbreviated Handshake and how are the messages different from the
SSL Full Handshake?
1. ClientHello: offers ciphersuite menu to server, and includes version, compression, client random and session ID
2. ServerHello: confirms session ID, changecipherspec, and sends finished
3. Client sends its own changecipherspec and finished message
It has less overhead than the full handshake as it reuses keys. 


2d. [3 pts] Why should the TLS_ECDH_ECDSA_WITH_AES_256_CBC_SHA ciphersuite, which is a TLS 1.2 ciphersuite, not be used anymore?
It is insecure: non-ephemeral key exchange (ECDH) that does not have the property of perfect forward secrecy, insecure hash (SHA), Does not support perfect forward secrecy.

2e. [3 pts] If an attacker sent a TCP RST message to reset a TLS connection, does TLS know that the TCP connection was attacked? How?
Through the SSL Alert Protocol - the client or server will notify each other connection terminations


1. PKI/TLS

In April 2014, a security vulnerability called Heartbleed was discovered which can obtain the private TLS keys from a server. Suppose Trudy used the Heartbleed bug to successfully obtain the private TLS keys from amazon.com.

1a. [4 pts] If amazon.com always uses the ciphersuite TLS_RSA_WITH_AES_256_CBC_SHA, are prior encrypted connections protected after Trudy steals the key? Explain why.
No - ephemeral key exchange was not used, if Trudy steals the private key she can decrypt all prior communications. The pre-master secret key is encrypted with the public key, and since Trudy has it she can decrypt it and get the session keys. 

1b. [4 pts] How can Trudy use the stolen private key to MITM a TLS connection and see encrypted data between a user and amazon.com? Explain why this cannot be easily done without the private key.
With the private key, she can compute the AES/HMAC keys for the session, decrypt and then read traffic. 

1c. [2 pts] Is it possible for a CA to issue more than one TLS certificate for amazon.com? Explain why or why not.
Yes

1d. [4 pts] Suppose a root CA was vulnerable to Heartbleed and lost its private keys. What can a user do to protect him or herself from being eavesdropped on?
Remove that root CA from the trusted cert store


1. 1. PKI

Alice, Bob, and Trudy are employees of ACME Corporation. Alice's PKI private certificate is generated on her laptop and never leaves the laptop. ACME Corporation has the ACME CA that digitally signs all the certificates.

a. [3 pts] Explain how Alice would mutually authenticate an ACME server using her PKI certificates.
Alice connects to the server via TLS, and the server sends over its certificate. Alice's laptop does a series of checks, following the chain of trust to verify that this is a valid certificate that comes from a trusted CA (Since it is her employee CA, her laptop should trust it). 
The same thing happens for the server receiving Alice's laptop, which receives a certificate with the TLS connection, and the server verifies that Alice is a trusted employee.

b. [3 pts] How does ACME and Alice know that each other’s certificate is valid?
Through checking the validity - date, trusted CA, certificate chain, issuer and digital signature (from the public key that comes with the certificate).


c. [3 pts] If Alice used her PKI certificates for encrypted communications to Bob, would ACME be able to read the encrypted conversation? Explain.
If the PKI certificates would come from ACME, yes they would be able to read encrypted conversations.

d. [3 pts] Trudy (who is evil) also worked at ACME corporation and has valid PKI certificates to authenticate into the ACME network. In what instances would Trudy be able to read the encrypted communication between Alice and Bob? Explain.