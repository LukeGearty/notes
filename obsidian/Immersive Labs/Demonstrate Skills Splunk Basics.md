### 🛡️ **Scenario: Junior SOC Analyst Role**

- You’ve completed initial training and are now expected to:
    
    - Monitor and search data independently.
        
    - Familiarize yourself with company log data.
        
    - Explore dashboards.
        
    - Identify HTTP traffic.
        
    - Perform searches on specific hosts.
        

---

### 📚 **Log Sources in the Lab (Splunk's Boss of the SOC CTF Data)**

1. **Windows Sysmon**
    
    - A logging tool from Windows Sysinternals.
        
    - Provides deep insight into system behavior (e.g., process start/stop, parent-child process relationships).
        
2. **Suricata**
    
    - Open-source Intrusion Detection System (IDS).
        
    - Monitors and analyzes network traffic for suspicious activity.
        
3. **Windows Event Logs**
    
    - Native Windows logs (system, security, application).
        
    - Useful for understanding user and system activities.
        
4. **Windows Registry Logs**
    
    - Tracks changes to the Windows Registry.
        
    - Important for identifying software installations or configuration changes.
        
5. **Stream (Splunk App)**
    
    - Monitors and captures live data streams.
        
    - Used to analyze real-time traffic patterns.
        
6. **fgt (Fortinet FortiGate Firewall)**
    
    - Firewall logs.
        
    - Provides information on blocked traffic, policies, threats, etc.
        

---

### 🔍 **Log Enumeration Command**

To list all sourcetypes within the `botsv1` index:

spl

CopyEdit

`| metadata type=sourcetypes index=botsv1`

---