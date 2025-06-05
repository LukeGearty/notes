at### **Secure Fundamentals: The CIA Triad**

#### **What is the CIA Triad?**

- **CIA = Confidentiality, Integrity, Availability**
    
- Core model for **information security**.
    
- Used to guide:
    
    - **Security policies**
        
    - **Employee training**
        
    - **Secure application development**
        
- Relevant to standards like **ISO 27001**.
    
- Applicable across **traditional IT systems** and **application security (AppSec)**.
    

---

### **1. Confidentiality**

**Definition**: Ensures **only authorized individuals** have access to sensitive data.

**In AppSec**:

- Protects **proprietary code**, **API keys**, **user data**, etc.
    
- Prevents **unauthorized disclosure** that can harm competitive advantage or user trust.
    

**Real-World Example – Twitch (Oct 2021)**:

- **Cause**: Misconfigured servers allowed public access to **source code** and **streamers’ earnings**.
    
- **Impact**:
    
    - Severe **reputational damage**
        
    - Loss of **competitive edge**
        
    - Breach of **confidential business information**
        

**Key Practices**:

- Proper **access control**
    
- **Server hardening**
    
- **Data classification** and **encryption**
    

---

### **2. Integrity**

**Definition**: Ensures data is **accurate, consistent**, and **not tampered with**, in transit or at rest.

**In AppSec**:

- Maintains **trustworthiness** of systems.
    
- Supports proper **authentication** and **authorization**.
    

**Real-World Example – SolarWinds Breach**:

- **Cause**: Integrity of software updates was **not verified**.
    
- **Impact**:
    
    - Affected hundreds of organizations (e.g., Microsoft, NSA).
        
    - Breach distributed through routine software updates.
        

**Other Example – Motorola MBP853 (2018)**:

- **Cause**: Failure to **verify server certificates**.
    
- **Impact**: Exposed to **MITM (machine-in-the-middle)** attacks.
    

**Key Practices**:

- **Code signing**, **checksums**, and **hashes**
    
- **Secure update processes**
    
- QA in the **CI/CD pipeline**
    
- **Firewalls** and **network restrictions**
    

---

### **3. Availability**

**Definition**: Ensures **authorized users** have **reliable and timely** access to resources when needed.

**In AppSec**:

- Ensures **uptime** and **resilience** of apps.
    
- Prevents downtime due to **misconfiguration**, **attacks**, or **system failures**.
    

**Real-World Example – Facebook Outage (Oct 5, 2021)**:

- **Cause**: Incorrect update to **BGP (Border Gateway Protocol)** records.
    
- **Impact**:
    
    - Facebook, Instagram, WhatsApp down for **6 hours**
        
    - Employees locked out of **physical and digital systems**
        
    - Lost **billions** in revenue and public trust
        

**Key Practices**:

- **Redundancy and backups**
    
- **Failover mechanisms**
    
- **Proper network configuration**
    
- **DDoS protection**
    

---

### **Summary**

|Element|Goal|Failure Impact|Key Examples|
|---|---|---|---|
|Confidentiality|Prevent unauthorized access|Leaks, reputational/financial damage|Twitch (2021)|
|Integrity|Ensure data is correct and untampered|Corrupted systems, backdoors, MITM attacks|SolarWinds, Motorola|
|Availability|Keep services accessible|Outages, lost productivity, revenue loss|Facebook (2021)|

---