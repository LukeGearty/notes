ACME Corporation has a shopping website that makes a profit of $1000 per day. It’s vulnerable to a particular DDOS attack that can bring the entire web server down for five days in a single incident. The engineers calculated each DDOS incident will cost the company $200 per day in security consulting fees

along with the lost profit. The engineers also calculate that the chance of a DDOS attack is once in two years.

1a. [3pts] What’s the Single Loss Expectancy (SLE)?

$5000 + $1000  = $6,000.00

1b. [2 pts] What’s the Annualized Rate of Occurrence (ARO)?

Once every two years: 0.5

  

1c. [2 pts] What’s the Annualized Loss Expectancy (ALE)?

6000 * 0.5  = 3,000

  

1d. [3 pts] Would a firewall that can prevent this particular DDOS attack that cost $10k to purchase with an annual licensing cost of $1000 per year be worth it? Explain.

A SLE is $6000 with an ARO of 0.5, and a ALE of $3,000. the firewall is $10,000, with $1000 fees every year. 

The ALE and Firewall will meet in 5 years, at that point the cost w/o the firewall will exceeed the cost of the firewall. 

However by that time, there might be more vulnerabilities and the particular DDOS attack might be outdated. I do not think it is worth it. 

  

  

Risk Assessment: ACME Corporation has a MySQL database that contains credit card numbers. The company does not have a Cybersecurity specialist to keep the database continuously secure from the latest attacks. Suppose the MySQL database as a probability of being compromised once in four years,

and each time it’s compromised it loses 1.5 million CC numbers which will cost the company $1 per CC number lost and $500k one-time marketing fee to repair ACME’s reputation.

1a. [3 pts] What’s the Single Loss Expectancy (SLE)?

(1 * 1.5 million) = $1.5 million + 500k) = $2,000,000

1b. [2 pts] What’s the Annualized Rate of Occurrence (ARO)?

0.25

1c. [2 pts] What’s the Annualized Loss Expectancy (ALE)?

$2,000,000 * 0.25 = $500,000

1d. [3 pts] Would hiring a team of Cybersecurity database specialists which costs $500k/year be worth it if the specialist can stop all database attacks? Why or why not? 

Unless they do other things besides stop database attacks, no because the cost of this matches the ALE

  

  

Risk Assessment: ACME Corporation has a corporate intranet where software for medical devices are being developed. ACME wants to secure the network by upgrading the firewall and installing an intrusion detection and monitoring program. The cost of the upgrade is a one-time fee of $300k with

$100k a year of maintenance each year. Suppose the intranet has a probability of being compromised twice per year, and each time a compromise occurs, ACME will need to pay an outside consultant $100k to fix the compromise.

1a. [2 pts] What’s the Single Loss Expectancy (SLE)?

100,000

1b. [2 pts] What’s the Annualized Rate of Occurrence (ARO)?

2

1c. [2 pts] What’s the Annualized Loss Expectancy (ALE)?

2 * 100,000  = 200,000

1d. [2 pts] Would upgrading the security of the network be worth it? Why or why not?

With a cost of 300,000, + 100,000 per year, the ALE will match it in 3 years at 600,000. After that time, it will be worth it. 

  

Session hijacking: Suppose Alice, Bob, and Trudy are connected to the same local switch. Alice has opened a telnet connection to Bob. Trudy knows that there’s a telnet connection and wants to hijack the telnet connection.

2a. [4 pts] if Trudy cannot see the traffic, explain in detail (protocol level details) how Trudy can inject traffic into Bob and how difficult it is do that.

It would be very difficult. Trudy would need transport level TCP sequence numbers to hijack the session, and that is extremely difficult to get if she cannot see traffic. She would essentially have to guess.

  

2b. [2 pts] Explain if two-way communication is possible between Trudy and Bob if Trudy cannot see the traffic.

Two-way communication is near impossible if Trudy cannot see the traffic, because Bob’s replies will go to Alice’s IP. She may be able to inject a reverse shell into a hijacking packet, which is very difficult. 

  

2c. [6 pts] Answer 2a and 2b supposing that Trudy can see the traffic between Alice and Bob.

a: It becomes easier because Trudy can sniff the packets and see the sequence and acknowledgment numbers.

b: It is very easy if Trudy can see the traffic

  

  

nmap:

  

3a. [10 pts] Using the standard nmap UDP scan, how does nmap decide if a port is open, closed, or filtered?

Open: A UDP probe gets a response

Closed: ICMP destination unreachable

Filtered: there is no response. This could mean filtered or a network error

  

Attacks

2a. [3 pts] What is a technical way in which Trudy would obtain the email server’s address (i.e., mail.acmecorporation.com) using DNS without being detected by ACME in any way? Explain.

Using a DNS lookup tool, such as MXtoolbox

  

2b. [3 pts] What kind of attack does Split DNS mitigate, and how does it do it?

Brute Force Forward DNS - it splits the DNS server into an external and internal server. Internal server stores subdomains only used internally. 

  

2c. [3 pts] When using the nmap TCP ACK Scan, what is the meaning if a RST is returned for a port? 2d. 
The port is unfiltered
[3 pts] What is the meaning if the response is nothing?
The port is filtered

[6 pts] Using the standard nmap TCP SYN scan, how does nmap decide if a port is open, closed, or filtered?
SYN Scan
If port is open: gets a syn ack
If port is closed: gets a rst
if filtered: nothing or ICMP unreachable

Exploits: In the nmap scan option called FTP bounce scan:
3a. [4 pts] What vulnerability in FTP is being used by nmap for port scanning?
nmap is exploiting a feature in FTP that allows the FTP server to send a file to a remote host. This allows for nmap to scan a target without the target knowing who scanned it.

3b. [6 pts] Explain how this feature works and describe the possible outcomes.
nmap would connect to a FTP server that’s vulnerable (that is, has this feature enabled), and use the “port” command to try to list directory. If target is listening on the port it will respond with a 150 or 226 response. If the port is not listening or closed it will respond with “425 Can't build data connection: Connection refused.” Port numbers and command name not required for full points.

DNS Reconnaissance: Suppose an attacker is performing reconnaissance on ACME Corporation using only the DNS protocol.

2a. [6pts] What are three methods using only the DNS protocol that an attacker can use to perform reconnaissance on ACME Corporation? Identify what type of information can be obtained.
DNS Request - A (IPvv4), AAAA (IPv6), MX (Mail server)
Brute-force Forward DNS: Trying DNS requests on different URLs, such as ftp.nyu.edu and see if it works. This can obtain information about other hosts on the network
DNS Zone Transfer: If successful, will obtain all the records on DNS server

