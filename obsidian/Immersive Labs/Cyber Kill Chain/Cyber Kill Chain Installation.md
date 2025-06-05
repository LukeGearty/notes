## **Cyber Kill Chain: Stage 4 – Installation**

### **Overview**

- In this stage, the attacker **installs a backdoor** or persistent access mechanism.
    
- Goal: **Maintain remote access** to the compromised system.
    
- Typically achieved using **malware** (e.g., **Remote Access Trojan (RAT)**).
    
- Can be done **automatically (via software)** or **manually** by the attacker.
    

---

### **Purpose of Installation**

- Ensures **persistence** even after a system **restarts**.
    
- Malware must survive reboots since **RAM data is lost** on shutdown.
    
- Achieved by setting up autorun mechanisms or storing malware on disk.
    

---

### **Common Installation Methods**

#### **1. Windows Registry**

- Used to **autorun commands** or **re-download malware** at system startup.
    
- Commonly abused registry keys:
    
    - `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`
        
    - `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`
        
    - `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce`
        
    - `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce`
        
    - `HKEY_CURRENT_USER\Software\Microsoft\Command Processor\AutoRun`
        
- **Evasion Tactic:**  
    Attackers may **hide malware inside legitimate keys** or use commands to **download the payload remotely** at startup, avoiding file storage on disk.
    

---

#### **2. Startup Folder**

- Malware is stored as a file in the **Windows Startup folder**:
    
    - `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`
        
- Ensures execution at boot, but riskier because:
    
    - **Malicious file must exist on disk** (easier to detect)
        

---

### **In the Lab**

- Investigate signs of installation using **Splunk**.
    
- **Steps:**
    
    1. Search logs for **registry changes** and **startup folder activity**
        
    2. Look for use of suspicious **autorun keys**
        
    3. Identify any **backdoor or RAT installations**
        
- **Splunk Tips:**
    
    - Use **keywords** like `registry`, `run`, `runonce`, `autorun`
        
    - Use wildcards (`*`) to match registry terms:
        
        - e.g., search for `*run*` to include all relevant keys
            

---