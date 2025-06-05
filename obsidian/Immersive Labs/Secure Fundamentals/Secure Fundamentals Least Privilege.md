### **Principle of Least Privilege - Notes**

#### **Definition**

- The **principle of least privilege (PoLP)** ensures that users or services are granted **only the permissions necessary** to perform their legitimate tasks—**nothing more**.
    
- These **privileges** are provided through the **authorization process**.
    

---

### **Scope of Application**

Applies mainly to:

#### **1. User Accounts**

- **Associated with humans**.
    
- Examples:
    
    - Restricting access to **root/admin accounts** to only IT staff.
        
    - Granting **read-only access** to sensitive documents where appropriate.
        

#### **2. Service Accounts**

- **Used in service-to-service** communication.
    
- Examples:
    
    - A build automation tool with **read-only access** to a source code repository.
        

---

### **Privilege Hierarchy**

- Privileges should be **cumulative and tiered**, starting from the lowest and building up.
    
- **Example of cascading roles**:
    
    - **Employee**: Read-only access to application.
        
    - **Manager**: Employee privileges + team management capabilities.
        
    - **Administrator**: Manager privileges + control over all accounts and teams.
        

---

### **Impact of Applying Least Privilege**

- **Mitigates damage** if an account is compromised.
    
    - E.g., a compromised read-only account limits the attacker's capabilities.
        
- Helps prevent **accidental damage** due to excessive privileges.
    
    - E.g., users without delete rights can't accidentally erase critical data.
        
- Contributes to **system stability and security** by minimizing risk exposure.
    

---

### **Real-World Examples**

#### **1. Zyxel (Dec 2020 – Jan 2021)**

- **Issue**: Devices contained hardcoded **admin-level credentials**.
    
- **Impact**: Attackers could gain **full control** of devices.
    
- **Lesson**: With **least privilege**, impact of exposed credentials could have been limited.
    

#### **2. Keepnet (June 2020)**

- **Issue**: Third-party contractor **disabled cloud firewall** for 10 minutes during database migration.
    
- **Impact**: Resulted in **database exposure**.
    
- **Lesson**: Enforcing least privilege for third-party access could have minimized damage.
    

---

### **Conclusion**

- PoLP doesn't **prevent incidents** but can **significantly reduce their impact**.
    
- Critical for:
    
    - **Minimizing blast radius** of breaches.
        
    - **Preventing human errors** due to over-permissioned users or services.