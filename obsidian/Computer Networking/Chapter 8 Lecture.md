Chapter Goals: 
	understand principles of network security:
		cryptograhpy and its many uses beyond "confidentiality"
		authentication
		message integrity
	securtiy in practice:
		firewalls and intrusion detection systems
		security in application, transport, network, link layers

What is network Security?
Confidentiality: only sender and intended recipient should understand
	Done through encryption
Authentication: confirmation of identity
Message Integrity: sender, receiver want to ensure message not altered (in transit or afterwards) w/o detection
Access and availability: services must be accessible and available to users


Principles of Cryptography:
M: plaintext message
K_a(M): ciphertext, encrypted with key K_A


Breaking the scheme:
cipher-text only: the attacker only has the intercepted ciphertext with no other info
	Brute force or statistical analysis
Known-plaintext attack: attacker has some pairings for ciphertext -> plaintext
chosen-plaintext: attacker can get the ciphertext for a chosen plaintext
