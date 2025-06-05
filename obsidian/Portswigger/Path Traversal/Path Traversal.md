### **Path Traversal (Directory Traversal) - Notes**

#### **Definition**

- Path traversal is a vulnerability that allows attackers to access files and directories that are outside the intended scope of the web application.
    
- Can lead to exposure of:
    
    - Application source code and internal data.
        
    - Backend credentials.
        
    - Sensitive OS files.
        
    - (In some cases) File modification or remote code execution.
        

---

#### **Example Scenario: File Read Vulnerability**

- Application use case: Loads images using:
    
    html
    
    CopyEdit
    
    `<img src="/loadImage?filename=218.png">`
    
- Internally accesses:
    
    swift
    
    CopyEdit
    
    `/var/www/images/218.png`
    

#### **Vulnerable Behavior**

- No validation/sanitization of the `filename` parameter.
    
- Allows traversal using `../` to access files outside `/images/`:
    
    bash
    
    CopyEdit
    
    `https://insecure-website.com/loadImage?filename=../../../etc/passwd`
    
- Actual path read:
    
    bash
    
    CopyEdit
    
    `/etc/passwd`
    

---

#### **Path Traversal Mechanics**

- `../` (or `..\` on Windows) steps up one directory level.
    
- Used to escape the intended directory and access arbitrary files.
    

---

#### **OS-Specific Notes**

- **Unix/Linux**:
    
    - `/etc/passwd` is commonly targeted.
        
- **Windows**:
    
    - Uses both `../` and `..\` for traversal.
        
    - Example:
        
        arduino
        
        CopyEdit
        
        `https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini`

### **Common Obstacles to Exploiting Path Traversal Vulnerabilities**

#### **1. Input Sanitization**

- Applications may strip/block `../` sequences.
    
- **Bypass techniques:**
    
    - Use **absolute paths** (e.g., `filename=/etc/passwd`).
        
    - Use **nested traversal**: `....//` or `....\/`.
        
    - Use **URL encoding**: `%2e%2e%2f` (for `../`)
        
    - **Double URL encoding**: `%252e%252e%252f`
        
    - **Non-standard encodings**: `..%c0%af`, `..%ef%bc%8f`
        

#### **2. Web Server Preprocessing**

- Some servers strip traversal sequences before input reaches the app.
    
- **Bypass**: Use encoded or double-encoded input.
    

#### **3. Required Base Folder**

- Some apps enforce a base path (e.g., `/var/www/images`).
    
- **Bypass**: Prefix base path then traverse back:
    
    - `filename=/var/www/images/../../../etc/passwd`
        

#### **4. Required File Extensions**

- Some apps require specific file extensions (e.g., `.png`).
    
- **Bypass**: Use **null byte injection** to terminate the path early:
    
    - `filename=../../../etc/passwd%00.png`  
        _(Null byte is `%00`)_
        

#### **5. Tool Support**

- **Burp Suite Pro** has a predefined list:
    
    - `Fuzzing - path traversal` with encoded payloads.
        

---

### **How to Prevent Path Traversal Attacks**

#### **Best Practice: Avoid Direct Filesystem Access**

- Don't pass user input to filesystem APIs if possible.
    

#### **If Filesystem Access is Required: Use Two Layers of Defense**

1. **Input Validation**
    
    - Use a **whitelist** of permitted values if possible.
        
    - Alternatively, restrict to **safe characters** (e.g., alphanumeric only).
        
2. **Path Canonicalization**
    
    - Append input to a base directory.
        
    - Canonicalize and verify that the final path stays within the base directory.
        
    
    **Example (Java):**
    
    java
    
    CopyEdit
    
    `File file = new File(BASE_DIRECTORY, userInput); if (file.getCanonicalPath().startsWith(BASE_DIRECTORY)) {     // process file }`