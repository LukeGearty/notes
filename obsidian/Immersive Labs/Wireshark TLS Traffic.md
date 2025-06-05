## 🧪 **Wireshark & TLS/SSL Decryption – Incident Response Lab**

---

### 🛠️ **Wireshark Overview**

- **Wireshark**: A free, open-source **packet analyzer**.
    
- **Primary Uses**:
    
    - Network troubleshooting & analysis
        
    - Protocol & software development
        
    - Educational purposes
        
- **Functionality**:
    
    - Captures **live network traffic** or analyzes **pre-recorded PCAP/PCAPNG files**
        
    - Offers **built-in statistics** and detailed inspection tools
        
- **Launch Command**:  
    `wireshark` (opens the GUI)
    

#### 🔍 **Use Case in Security**

- Commonly used to **analyze network traffic from malware** to identify outbound/inbound connections post-execution.
    

---

### 🔐 **TLS/SSL Protocol**

- **TLS (Transport Layer Security)**: A modern cryptographic protocol for securing communication between two hosts.
    
- **SSL**: Older, deprecated predecessor of TLS.
    
- **Terminology Note**:  
    TLS and SSL are used interchangeably in many contexts, but TLS is the current standard.
    

#### 📜 **Purpose of TLS/SSL Certificates**

- **Encrypts** data in transit
    
- Provides **identity assurance** and builds trust between client and server
    

#### ⚠️ **Security Caveat**

- TLS certificates are **freely available** and widely used—even by **cyber attackers** to create misleading “secure” websites.
    

---

### 🔓 **Decrypting TLS Traffic in Wireshark**

Wireshark supports decryption if the required secrets are available.

#### 🔑 **Methods of Decryption**

1. **Key log file (per-session secrets)**
    
2. **RSA private key**
    

#### 🧪 **Focus in This Lab**

- Using a **(Pre)-Master Secret** from a **key log file**
    

##### 🔧 **Steps to Use a Key Log File**:

1. **Capture Setup**:  
    Use an **environment variable** to set a key log path pre-capture, or extract post-capture using tools/scripts.
    
2. **Configure Wireshark**:
    
    - Navigate to:  
        `Edit > Preferences > Protocols > SSL (or TLS)`
        
    - Set the **(Pre)-Master-Secret log filename** to the key log file location.
        

---

### 🛡️ **Lab Scenario: Incident Response**

#### 👤 **Your Role**:

Cyber Defense Analyst responding to a security incident.

#### 🧩 **Scenario Summary**:

- An employee attempted to **exfiltrate proprietary data**.
    
- A portion of their **network traffic** and **client secrets** were captured.
    
- The only clue from the suspect:
    
    > “**The Ghost is in the URL**.”
    

#### 🎯 **Objective**:

- Decrypt **HTTPS traffic** using Wireshark and the provided **key log file**.
    
- Identify sensitive information hidden in encrypted traffic.
    

---

### 📘 **Recommended Further Learning**

- **Lab Suggestion**:  
    Explore more about **TLS handshake** and **Wireshark decryption techniques** in the associated follow-up labs.