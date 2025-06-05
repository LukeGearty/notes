## **Cyber Kill Chain: Stage 6 – Actions on Objectives**

### **Overview**

- This is the **final stage** of a cyberattack.
    
- The attacker **executes their ultimate goal**, which may include:
    
    - **Data exfiltration** (e.g., stealing sensitive data)
        
    - **System compromise**
        
    - **Service disruption**
        
    - **Destruction or manipulation of data**
        
    - **Persistent access** setup for future use
        

---

### **Nature of Objectives**

- Attackers always have a **purpose**, such as:
    
    - Financial gain (e.g., ransomware)
        
    - Espionage (e.g., data theft)
        
    - Sabotage or destruction
        
    - Misinformation or framing (planting evidence)
        
    - Hacktivism or personal gratification
        
- Every attack is **context-specific**, based on attacker motives and target vulnerabilities.
    

---

### **Detection Challenge**

- If attackers are **stopped early**, their objectives may remain **unknown**.
    
- This makes **early detection and response** essential.
    
- Forensic analysis of logs and indicators of compromise (IoCs) can help **infer the objective** post-incident.
    

---

### **In the Lab**

You are tasked with:

- **Investigating actions** tied to ransomware activity detected during the **Command and Control** stage.
    

**Key focus:**

- Identify Windows utilities used for malicious purposes via **Splunk**.
    

---

### **Common Tools Abused by Attackers**

Attackers often use legitimate **Windows command-line utilities**, such as:

- `PowerShell` – script execution, downloading payloads
    
- `cmd.exe` – general command-line operations
    
- `taskkill` – killing processes
    
- `bcdedit`, `vssadmin`, `wmic` – used to disable recovery/shadow copies
    
- `robocopy` or `xcopy` – data exfiltration or replication
    

> **Note:** These tools are not inherently malicious—**context and timing** matter.

---

### **Using Splunk for Threat Hunting**

- Search log entries for **use of known tools**.
    
- Look for **anomalous command-line activity**, especially tied to:
    
    - Off-hours usage
        
    - Non-admin users running privileged commands
        
    - Correlation with known compromise timestamps
        
- Combine results with **C2 data** and **installation indicators** for full attack timeline.