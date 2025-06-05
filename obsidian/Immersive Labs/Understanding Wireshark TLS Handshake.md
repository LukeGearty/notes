## 🧪 **Packet Analysis: TLS Handshake & Incident Response**

---

### 🔍 **Reading Packet Data in Wireshark**

- **Wireshark**: Free, open-source packet analyzer.
    
- **Uses**:
    
    - Network troubleshooting
        
    - Protocol development
        
    - Education
        
    - Software and network analysis
        
- **Capabilities**:
    
    - Capture live traffic or analyze existing capture files (PCAP, PCAPNG).
        
    - Includes built-in statistics.
        
    - Offers detailed inspection of network packets (expandable protocol layers).
        

---

### 🔐 **HTTPS and the TLS Handshake**

#### ✅ **What is HTTPS?**

- HTTPS = HTTP + TLS encryption.
    
- TLS (Transport Layer Security) is the modern protocol replacing the deprecated SSL.
    

#### ❗ **Terminology Note**

- Terms like _SSL_ and _SSL/TLS_ are often used interchangeably, but TLS is the current protocol in use.
    

---

### 📡 **TLS Handshake Overview**

#### 🧾 **Purpose**

- Establish a secure, encrypted communication channel between client and server.
    
- Used in HTTPS, secure API calls, DNS over HTTPS, etc.
    

#### 🔒 **Cipher Suite**

- A set of encryption algorithms for securing communication.
    
- Chosen during the handshake process based on supported options.
    

---

### 🤝 **TLS Handshake Steps**

1. **ClientHello** (Client → Server)
    
    - TLS version supported
        
    - Supported cipher suites
        
    - Client random bytes
        
2. **ServerHello** (Server → Client)
    
    - Chosen cipher suite/TLS version
        
    - Server certificate (SSL certificate)
        
    - Server random bytes
        
3. **Certificate Authentication**
    
    - Client verifies server certificate against a certificate authority (CA).
        
    - Ensures identity and trust.
        
4. **Key Exchange**
    
    - Client creates a **premaster secret** (random bytes).
        
    - Encrypts premaster secret using the server's public key (from certificate).
        
    - Server decrypts it with its private key.
        
    - Both generate a shared **session key** using:
        
        - Client random
            
        - Server random
            
        - Premaster secret
            
5. **Symmetric Encryption Achieved**
    
    - Both parties send a “Finished” message encrypted with session keys.
        
    - Secure session is now established using symmetric encryption.
        

---

### 🧪 **Lab Objective**

- Understand and analyze TLS handshake packets using Wireshark.
    
- Use provided PCAP file for inspection.
    

#### 🔧 **Key Learning Goals**

- Familiarity with Wireshark interface.
    
- Ability to recognize and analyze handshake packets.
    
- Understanding encryption setup in secure communications.
    

---

### 📚 **Additional Labs to Explore**

#### 🧩 **Prerequisite Labs**

|Title|Difficulty|Time|Status|
|---|---|---|---|
|Wireshark: Display Filters – Introduction to Filters|4/9|10 mins|Not started|
|Packet Analysis: BPF Syntax|4/9|27 mins|Not started|

#### 🛠️ **Further Exploration**

- Lab: Decrypting TLS encrypted traffic using Wireshark.