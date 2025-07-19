Host Security
In security, there are 3 goals - the CIA triad

Is it possible to build a secure computer system? Yes
If so, why is it not done? Many reasons, but one of them is legacy.
It is possible to scrap an OS and build one that is secure, but what about the applications that ran on those old systems? 

Controlling Access to Resources
Trusted Computing Base
Reference Monitor: Making sure a user process is not malicious. A user process makes a system call, the reference monitor makes sure it is not malicious.

Three protected domains:
Domain 1
Domain 2
Domain 3
All have different permissions (read, write, execute) on different resources. You can think of this as a matrix.
In linux, sudo can be thought of as entering the administrator domain. 
Access Control Lists can be used manage access to resources.
Different kinds of resources, for ex in Unix there is a password file that needs to be more secure than another file. 

Formal Models of Secure Systems
Think of the matrix, what happens when permissions mutate?

Bell-LaPadula Model: Process running at security level k can read only objects at its level or lower
	The * property: process running at sec level k can write only objects at its level or higher
Multilevel security model

Biba: designed to guarantee integrity of data
	Process at sec level K can write only objects at its level or lower (no write up). Can't write up because we don't know enough
	The integrity * property: process running @ sec level k can read only objects at its level or higher (no read down)


Basics of Cryptography
Two keys to encrypt and decrypt. if they are the same it is synchronous, if they are different it is asynchronous. Pass in the plaintext to an encryption algorithm to create the cipher. Pass in the cipher to the decryption to get the original message. Synchronous is fast, but how to get the key securely?
HTTPS works by using asynchronous encryption to encrypt the shared key and use that for encryption.

Authentication
Methods of authenticating users when they attempt to log in based on one of three general principles: something the user knows (password ex), something the user has (like a code texted to you), something the user is (fingerprint for example). 
Security in OS is still user based rather than process based. A process doesn't have biometrics or a code. 

