## **Cyber Kill Chain: Stage 2 – Weaponization**

### **Overview**

- At this stage, the attacker **develops their attack strategy**.
    
- May involve **cloning the target environment** for testing/tuning exploits.
    
- Uses intel from **Reconnaissance** to build targeted **exploits or malware**.
    
- Entirely **invisible to the victim**—no direct interaction with the target yet.
    

---

### **Key Component: Payload Creation**

- **Payload =** Code or instructions used to initiate next attack steps.
    
- Payloads are tailored to the **victim’s specific environment**.
    
    - Example: An attacker finds the server runs **IIS 8.5**, so they avoid using Apache-specific exploits.
        
- Common types of payloads:
    
    - Malicious URLs with crafted parameters (e.g., **SQL injection**)
        
    - Scripts or binaries used in later stages
        
- **Multiple payloads** may be created for:
    
    - Different systems
        
    - Different attack phases
        

---

### **Indicators of Weaponization**

- **Custom user-agent strings** in HTTP requests
    
- **Unusual data structures** in compressed files
    
- Use of tools like:
    
    - **CeWL** – creates targeted **word lists** for brute-force attacks
        

---

### **Attack Infrastructure**

- Attackers may set up infrastructure to **support and deliver payloads**:
    
    - Domains
        
    - Servers
        
    - Websites
        
- This infrastructure allows control throughout the **entire attack lifecycle**
    
- **Disclosure of attacker infrastructure** (e.g., IPs, domains):
    
    - Forces attacker to **modify or abandon** campaign
        
    - **Key opportunity for defenders** to disrupt the attack
        

---

### **In the Lab**

- Investigate a cybersecurity incident using **Splunk**
    
- **Steps:**
    
    1. Identify the **malicious domain** receiving the most packets
        
    2. Inspect Splunk logs to:
        
        - Detect tools used
            
        - Uncover signs of Weaponization (e.g., payload indicators, suspicious infrastructure)
            
- **Splunk Tips:**
    
    - Use filters and tables for top/rare values
        
    - Click field names in the left-hand menu for expanded views and targeted queries