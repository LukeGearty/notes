## **Secure Fundamentals: Authentication**

### **Definition**

- **Authentication** is the process of **confirming and validating an entity’s identity**.
    
- Often **confused with authorization**, but they are different:
    
    - **Authentication**: Proves _who_ you are.
        
    - **Authorization**: Determines _what_ you're allowed to do.
        

---

### **Key Concepts**

#### **Entity**

- Any object trying to access a system.
    
- Can be:
    
    - **Human**
        
    - **Computer**
        
    - **Service**
        
    - **Other objects** (e.g., vehicles)
        

#### **Identity**

- A **collection of attributes** linked to an entity.
    
- Used to uniquely identify and authenticate the entity.
    
- **Examples**:
    
    - Email address
        
    - Home address
        
    - Username
        
    - User ID
        

---

### **Authentication Process**

- Begins when an entity requests access to a system.
    
- The system prompts the entity to **prove its identity** using credentials.
    
- **Success** → Access granted.
    
- **Failure** → Access denied.
    

---

### **Common Types of Authentication**

#### 1. **Password Authentication**

- Requires **username/identifier + password**.
    
- Password should be:
    
    - **Private**
        
    - **Hard to guess**
        

#### 2. **Token / Access Key Authentication**

- Requires only a **token** to authenticate.
    
- No username needed if token is identity-linked.
    
- Token must be:
    
    - **Private**
        
    - **Difficult to replicate**
        

#### 3. **Hardware Token**

- Uses a **physical device** to provide or generate a token.
    
- Access to device = proof of identity.
    
- Device must be:
    
    - **Kept secure**
        
    - **Not shared**
        

#### 4. **Biometric Authentication**

- Uses **unique biological traits** for authentication.
    
- **Common examples**:
    
    - Fingerprints
        
    - Facial recognition
        
    - Voice recognition
        

#### 5. **Multi-Factor Authentication (MFA)**

- Uses **two or more independent credentials** to verify identity.
    
- Enhances security by combining different forms of authentication.
    
- **2FA (Two-Factor Authentication)**: A subset using two methods.
    
- Protects against:
    
    - **Phishing**
        
    - **Credential stuffing**
        
    - **Password guessing**
        

---

### **Impact**

- **Critical for trust**: Without authentication, a system cannot trust or verify users/entities.
    
- **Precedes authorization**: Must confirm identity before assigning permissions.
    

---

### **Real-World Examples**

#### **Facebook (2013)**

- **OAuth vulnerability** allowed attackers to hijack accounts via a malicious website.
    

#### **Apple (2020)**

- **“Sign in with Apple” flaw** allowed unauthorized access to third-party accounts.
    

#### **Dropbox (2011)**

- For ~4 hours, users could log in using **any password** due to a severe bug.
    
- Highlighted the danger of weak or broken authentication mechanisms.