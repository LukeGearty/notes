
**Public Key Infrastructure (PKI)**
RSA, Diffie-Hellamn, AES serve as backbone of modern cryptography - variations exist.
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
TLS v 1.3 - RSA or Diffie-Hellman, or equivalent
A certificate can be valid for multiple domain names - if the subject alternative name for multiple domains


Exercise #2
Suppose Prof. Mak sent you a digitally signed email using a valid X.509v3 certiifcate. Is there any reason not to trust it? (Hint: Yes)
CAs for emails don't have a Certificate trust store to ensure that they are valid CAs
Private key might have been leaked, someone could be impersonating Prof Mak, the CA itself might not be trustworthy


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