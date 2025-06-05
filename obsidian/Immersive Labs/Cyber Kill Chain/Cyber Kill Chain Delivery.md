## **Cyber Kill Chain: Stage 3 – Delivery**

### **Overview**

- The attacker **delivers the malware or exploit** created during the **Weaponization** stage.
    
- Marks the **first interaction** between the payload and the target system.
    
- Delivery methods can vary depending on target type (user vs. system).
    

---

### **Common Delivery Methods**

1. **Phishing Emails**
    
    - Contain malicious **links or attachments**
        
    - Target users likely to open or click the payload
        
2. **Network-Based Delivery**
    
    - **Malicious traffic** is sent to a target service
        
    - Intent: Trigger a known **vulnerability** in the service
        
3. **Removable Media**
    
    - **USB drives** used to deliver malicious files
        
    - Often used when physical or local access is available
        
4. **File Uploads**
    
    - **Malware uploaded** to network shares or services (e.g., via file sharing or web forms)
        

---

### **In the Lab**

- Investigate delivery techniques using **Splunk logs**.
    
- **Objectives:**
    
    1. **Identify Executable File Uploads**
        
        - Look for **HTTP POST requests** with `.exe` filenames
            
        - Indicates a file being uploaded to a target system
            
    2. **Identify USB-Based Delivery**
        
        - Search for USB metadata:
            
            - **Vendor ID (VID)**
                
            - **Product ID (PID)**
                
            - **Serial number**
                
        - These values are tracked in the **Windows Registry**
            
        - Registry logs are **indexed in Splunk**
            

---

### **Splunk Tips**

- Use filters and search features to locate:
    
    - Suspicious HTTP requests
        
    - USB device logs
        
- Click on field names in the side menu to open detailed table views and links to prebuilt queries