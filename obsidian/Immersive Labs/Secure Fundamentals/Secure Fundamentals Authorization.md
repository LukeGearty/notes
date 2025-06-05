### **Authorization - Notes**

#### **Definition**

- **Authorization** determines if an entity has sufficient permissions or privileges to perform a specific action on a system.
    
- Ensures **only trusted entities** are allowed to perform actions.
    

#### **Process**

- If the entity **has sufficient permissions**, the action proceeds.
    
- If **not**, an appropriate **error message** is returned.
    

#### **Relationship with Authentication**

- **Authentication** and **Authorization** are **distinct but related**.
    
- **Authentication precedes authorization**: you must first confirm the entity’s identity before checking its permissions.
    

---

### **Entities**

- An **entity** is any object attempting to act on a system.
    
- Examples:
    
    - Humans (e.g., users)
        
    - Machines (e.g., services, computers)
        
    - Other objects (e.g., vehicles)
        

---

### **Roles**

- **Roles** are a collection of permissions assigned to an entity.
    
- Useful for managing **groups** of entities with shared permissions (e.g., admin roles).
    
- Help simplify permission management.
    

---

### **Impact of Improper Authorization**

- **Incorrect or missing authorization checks** can allow unauthorized access.
    
- Risks:
    
    - Unauthorized modification or deletion of data
        
    - Potential **data loss** or **security breaches**
        

---

### **Real-World Example: Fidelity Investments Breach (October 2024)**

- **Incident**: Data breach affected **77,000 customers**.
    
- **Cause**:
    
    - Attackers used **authenticated** customer accounts to access document images of other customers.
        
    - Indicates a **failure in authorization**, not authentication.
        
- **Impact**:
    
    - Exposure of private customer data
        
    - Potential **financial misuse** of data
        
    - **Reputational damage** to Fidelity
        
    - **Second breach** in a year; prior breach (28,000 customers) cost an estimated **$30 million**
        

---

### **Related Labs**

- Understanding **authentication** is critical, as it typically precedes authorization.