# Penetration Tester's Mindset and Motivation

There are two ways of solving problems:
1. Follow a defined strategy, be thorough, methodical, recording all data, maintaining organization, and improving proces to maximize efficiency
2. Experimenting, thinking outside the box, being pragmatic, flexible, break habits and don't think like atraditional IT administrator or developer
	1. There may not be a playbook for problems that you come across
You will need to utilize both simultaneously to be a succesful penetration tester
Learning to think like an attacker so we can help protect our systems from the threats to our security

Penetration Testing is intended to help a system owner get vulnerabilities patched before bad guys exploit them. 
Manage and reduce risk to a customer, system or organization
Help make the case for a higher security budget.
Find vulnerabilities, validating them through exploitation, and demonstrating the impact.

Presenting discoveries in a report:
- A penetration test ends with a report to the system owner
- Report details the risk of all findings and may provide mitigation recommendations
- Risk may be explained in business or mission impact, accounting for the value of the assets
	- May be protecting financial data to physical safety and lives

# Case Studies
## Phineas Fisher vs Hacking Team
In 2015, a lone hacker named Phineas Fisher hacked and owned Hacking Team - an organization based in Italy. They create tools for government agencies. They lost access to their twittr accounts and had personal data on the internet.
Phineas Fisher owned the network from external hosts, backdoored it, and stole a lot of information - customer data, source code for proprietary data, passwords for social media.
They were a company of experts but since they got hacked, their public image suffered. 
#### Methodology
Looked for internet facing IPs, hosts and services, and wrote zero days for embedded device / network appliance. 
Wrote a post-exploitation backdoor for the embedded device to gain persistence
Tested the exploit and the post-exploit tools against other devices and companies first
Launched exploit, gained foothold, scanned and listened slowly
Found an ISCSI backup device on the same network, mounted it and browsed files and emails
In the backups, found a domain admin password, used it to weaponize more powerful hacking tools
Started downloading entire company email, file shares.
Installed a keylogger and backdoored a users computer, then waited for the user to mount a TrueCrypt volume to steal very sensitive data
Reset passwords, stole their twitter account

PF wrote a long manifesto talking about motivations, calling for action, but also explained how it was done 
https://www.exploit-db.com/papers/41915

## Free Pizza
Found by a security researcher who disclosed everything - against an application
Less Serious case
Dominos App - sometimes the app gives a user a $10 coupon, sometimes not. The researcher was interested in why. 
### Methodology
Look at application source code
Use a web proxy (Burp Suite) to monitor traffic between app and sserver
Noticed that payment processing is done client side on the app and a 3rd party
	Not necessarily the worst thing to do
MITM https request from app with a bogus credit card #
	Not really looking for spoofed payments
	(payment == 'Declined') ----> (payment == 'Accepted')


# Vocabulary
Vulnerability: A weakness in a business process ,configuration, operating system or application that can be used to create unintended and undesired scenarios. These can be considered opportunities for threat events.
Exploit: A threat event that weaponizes code or an application, to take advantage of a weakness for the purpose of having an intended effect to a target that would otherwise be impossible, unintended by the target owner, or unauthorized
Asset: Something that holds value, whether it is a system or data, that a threat may be trying to access or compromise
	Ex: R&D data, Personnel data, credit cards, trade secrets
Threat: The thing that can cause our system harm, either adversarial, environmental, or accidental
Threat Source: Actor or agent that can trigger certain events which we try to protect against. Threat sources have certain properties such as posture, skill level, resources, intention and motivation.
Adversarial threat postures:
	Insider - threat posture internal to org
	Nearsider Threat - threat posture physically located with your system but has no system access (visitors, maintenance, janitorial)
	Outsider threat - threat posture which has no physical acces. Try to attack over the internet, wireless, or physical attacks.
Threat Event
	The event that is doing some harm against a target. Adversarial could take the form of recon, creating weapons, attacking, exfiltrating data, or other malicious actions against a target. Non-adversarial examples might be disk failure, employee negligence, or natural disaster.
Hacking - using something in a deliberate way to create effects that id against the original intention or design
Ethical Hacking - authorized behavior and actions to identify vulnerabilities of a system to help improve its security posture
Security Audit - regulatory requirements for finance, corporate, government, or military systems
	A thorough checklist of security controls are measured against both technical implementations, policies, and procedures

## Penetration Testing vs Vulnerability Analysis
These two terms are almost identical, since the end result is to create a more secure network or system. 
Both of these things accomplish this by making recommendations for fixing and mitigations, require permission, may model after adversarial threats. 

**Penetration Testing** : goes beyond identifying vulnerabilities, validates findings through exploitation and provide risk ratings for security findings according to assets and impacts

**Vulnerability Analysis and Assessment** : Like penetration testing but not as thorough, only part of the pen testing process
Same steps of pen testing, taken from ethical hacking, but stops at scanning
Typically doesn't exploit vulnerabilities but still helps secure a target by providing mitigation recommendations

## White Box/Black Box
White Box/ Crystal Box - 
	Provided full access to networks and systems, configurations, admin/root acces, source code
	Usually results in more details and findings, but not as in-depth as Black Box Testing
Black Box Testing
	Not provided any details on target, system, network, application, or asset
	May rely on OSINT, social engineering
	May target all exposures but will only go in-depth in certain vulns that can be exploited
	Most accurate to mimicking outsider real-world attacks

## Blue Team / Red Team
Blue Team
	Defensive side, may perform tests to better secure system
	Team works cooperatively with an organization to defend and protect their systems
	Any kind of testing would be White Box
Red Team
	Pentesting team used to exercise the Blue Team and assess the security and response of an organization
	Black Box Testing acts as an outsider, mirroring real world TTPs based off intelligence reports
	Still authorized but only known to a few people in an org
	May get details for white box testing for intentional planend exercises if the focus is to test the Blue Team
	May also assess physical security and utilize social engineering
	May not assess the entire system or network, only what is exposed to an outsider or nearsider
	Provides an incomplete risk assessment of all systems but does a great job of mirroring real-world risk and prioritizing what mitigations and organizational improvements are needed


# Test and Testing Methodology
Most tests have
	type - what does the test target
	Category or posture - Insider/Blue Team/white box or outsider/Red Team/Black box
		Insider vs adversarial
Network Tests
	The most common, scans the network/protocols/hosts/ and services available for vulns and weakness
	Typically a look at perimeter security (outsider threat) and/or internal network resources (nearsider and insider threat)
	Jack of all trades test
	Compare to Phineas Fisher hack - attacker hacks from outside by doing thorough steps
Client-side Tests
	Testing the client software - apps, browser, configuration, OS
Web and web application
	Testing the perimeter and functionality of internet-facing web resources

Wireless testing - assessing the use of wireless networks, encryption, authentication, DoS
	Focus on 802.11, IoT devices, cellular data, stuff running Zigbee, Bluetooth, making sure they are using certitficates and encryption properly
Application Security Test
	Testing software applications
		Custom Built
	Static Code Analysis Test
		Having the source code available to identify code flaws, errors, looking for Common Weakness Enumeration (CWE)
	Dynamic Analysis Test
		Black Box approach t oapplication security testing
		Debugging the binary, fuzzing memory, exposing sensitive data, looking for flaws and errors to exploit
Complet Red Team Tests
	Black Box, outsider threat-style test
	Different because this may include physical security and social engineering, stealing equipment, dumpster diving

## Methodologies
Testing methodologies cover the process of performing different types of pentests

The Cyber Kill Chain - understanding the attacker
	The PF hack follows this
	Developed to summarize how attackers compromise a system

Steps:
1. Reconnaissance - learning about the target, casing the joint
2. Weaponization - either write or find exploits for things that may be vulnerable
3. Delivery - getting the weaponization onto the system
4. Exploitation - exploiting the vulnerability t oeecute code on victim's system
5. Installation - installing malware on the asst
6. Command and Control / C2: Command channel for remote manipulation of victim
7. Actions on Objectives - Intruders accomplish their original goals

Penetration testing is to help secure against real-world threats. By understanding the kill chain an adversary would use, the tester can think more like the attacker
Steps to compromise, such as a kill chain, allow testers to suggest mitigations that stop the kill chain at different steps

https://web.archive.org/web/20250630153558/http://www.vulnerabilityassessment.co.uk/Penetration%20Test.html

http://www.pentest-standard.org/index.php/Main_Page

http://www.pentest-standard.org/index.php/PTES_Technical_Guidelines

