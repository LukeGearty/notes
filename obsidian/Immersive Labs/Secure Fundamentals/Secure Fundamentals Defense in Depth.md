## **Secure Fundamentals: Defense in Depth**

### **Overview**

- **Defense in depth** is a layered security strategy adapted from military defense models.
    
- It ensures that if one security layer is breached, **multiple additional layers** must also be bypassed.
    
- Purpose: Prevent unauthorized access to systems, applications, or services.
    

---

### **Three Main Areas of Defense in Depth**

#### 1. **Physical Controls**

- Restrict **physical access** to systems.
    
- **Examples:**
    
    - Locked doors
        
    - Security guards
        
    - CCTV surveillance
        

#### 2. **Technical Controls**

- Use **hardware/software** to limit access and detect threats.
    
- **Examples:**
    
    - Intrusion Detection Systems (IDS)
        
    - Firewalls
        
    - Antivirus software
        
    - Disk encryption
        
    - Authentication mechanisms
        

#### 3. **Administrative Controls**

- Enforce **policies and procedures**.
    
- **Examples:**
    
    - Corporate data handling procedures
        
    - Security policies and requirements
        

---

### **Example Scenario**

- A web application is protected by:
    
    1. **IDS**
        
    2. **Web Application Firewall**
        
    3. **Internal Network Firewall**
        
- An attacker must **bypass all three** layers to access the system.
    
- If only one layer existed, the attacker could exploit a single vulnerability.
    
- Defense in depth allows **legitimate access** while impeding **unauthorized intrusion**.
    

---

### **Impact of Defense in Depth**

- Slows or deters attackers by introducing multiple barriers.
    
- Increases chances of detecting or blocking threats.
    
- Protects against **various attack types** and reduces damage from successful breaches.
    

---

### **Real-World Examples**

#### **1. British Airways Breach (2018)**

- **Affected**: 400,000+ customers.
    
- **Issue**: Lack of MFA and Content Security Policy (CSP).
    
- **Attack**: Malicious JavaScript code injected and executed.
    
- **Consequences**:
    
    - £20 million fine (largest ever by ICO at the time).
        
    - Highlighted failure to implement layered security (MFA & CSP).
        

#### **2. Capital One Breach (2019)**

- **Affected**: 100+ million customers.
    
- **Issue**: Misconfigured Web Application Firewall (WAF).
    
- **Attack**: Gained access to cloud-based storage; decrypted sensitive data.
    
- **Consequences**:
    
    - $80M penalty by OCC
        
    - $190M class-action lawsuit settlement
        
    - Costs estimated at $100M–$150M
        
- **Key Learnings**:
    
    - Tokenization of SSNs reduced exposure.
        
    - Better **key management** and broader **tokenization** could have minimized damage.