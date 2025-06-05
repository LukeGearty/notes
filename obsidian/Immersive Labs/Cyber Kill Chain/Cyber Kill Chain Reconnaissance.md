## **Cyber Kill Chain: Reconnaissance**

### **Overview**

- **Purpose:** Attacker gathers information relevant to launching an attack.
    
- **Methods:**
    
    - **Open-source intelligence (OSINT)**
        
    - **Interaction with platform/artifact**
        

---

### **Types of Reconnaissance**

1. **Active Reconnaissance**
    
    - Direct interaction with the target system
        
    - Activities are typically logged
        
    - **Examples:** Visiting a website, port scanning
        
2. **Passive Reconnaissance**
    
    - No direct interaction with the target
        
    - No logs are generated on the target system
        
    - **Examples:** Viewing cached sites, reading public articles
        

---

### **Reconnaissance Tools**

#### **Vulnerability Scanners**

- Used to identify known security flaws
    
- **Example:** **Nessus**
    
    - Produces reports detailing vulnerabilities and their severity
        
    - Detection through unusual or excessive requests, abnormal headers
        

#### **Nmap**

- Network scanning tool
    
- Used in the early attack stages to:
    
    - Scan IPs and enumerate open ports/services
        
    - Identify likely web servers (e.g., port 80/443)
        
- Results guide further active reconnaissance tools like:
    
    - **Burp Suite**
        
    - **DirBuster**
        

#### **Shodan.io**

- A **passive** reconnaissance tool
    
- Internet search engine for connected devices
    
- Allows users to find:
    
    - Open ports
        
    - Running services
        
    - DNS information
        
- Targeted asset is unaware of the query (no logs)
    

---

### **In the Lab Exercise**

- Goal: Investigate a cybersecurity incident using **Splunk**
    
- **Steps:**
    
    1. Locate the **malicious domain** with the most packets sent
        
    2. Analyze logs to identify:
        
        - Tools used by the attacker
            
        - Useful data gathered for next attack stages
            
- **Splunk Tips:**
    
    - Use filters and tables to break down top/rare values
        
    - Click field names for detailed views and query links
        

---