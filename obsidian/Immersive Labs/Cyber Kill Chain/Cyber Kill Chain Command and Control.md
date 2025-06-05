## **Cyber Kill Chain: Stage 5 – Command and Control (C2)**

### **Overview**

- At this stage, the **attacker establishes a remote connection** to the compromised system.
    
- Purpose: **Execute commands**, **monitor activity**, and **exfiltrate data**.
    
- A **Command-and-Control (C2) server** manages communication with infected hosts (a.k.a. **bots** or **zombies**).
    

---

### **What Is a C2 Server?**

- A **central server or infrastructure** used by attackers to:
    
    - Send instructions to compromised devices.
        
    - Receive data (e.g., stolen credentials, keystrokes).
        
    - Manage botnets and coordinate further attacks.
        
- Compromised devices typically contain **malware** allowing this remote control.
    
- Communication can occur through:
    
    - Internet protocols (e.g., HTTP, HTTPS, DNS)
        
    - Messaging protocols
        
    - **Covert channels** embedded in legitimate traffic
        

---

### **C2 Obfuscation Techniques**

- To avoid detection, attackers may:
    
    - Use **encryption** or **custom protocols**
        
    - Rely on **legitimate services** (e.g., social media, cloud platforms)
        
    - Employ **domain generation algorithms (DGAs)** to change server addresses frequently
        

---

### **Detection and Defense**

- **Network Intrusion Detection Systems (IDS)** help detect C2 traffic:
    
    - **Snort** and **Suricata** are leading tools with community-maintained rule sets
        
- Blocking or disabling C2 communication:
    
    - **Interrupts attack lifecycle**
        
    - Prevents further control or data theft
        

---

### **Case Study: Cerber Ransomware**

- A **notorious ransomware** strain active since 2016.
    
- **Encrypts victim files**, demands **ransom** (often in Bitcoin).
    
- Spread via:
    
    - Malicious email attachments
        
    - Exploit kits
        
    - Infected websites or software
        
- Targets **Windows systems**
    

---

### **In the Lab**

- Investigate C2 activity via logs in **Splunk**.
    
- **Steps:**
    
    1. Review **Suricata event logs** for signs of C2 traffic.
        
    2. Apply filters to specific **Splunk fields** to identify anomalies.
        
    3. Look for communication between internal assets and **known malicious IPs/domains**.
        
- **Splunk Tips:**
    
    - Use table views to analyze **top or rare values**.
        
    - Click field names for breakdowns and to build specific search queries quickly.
        

---