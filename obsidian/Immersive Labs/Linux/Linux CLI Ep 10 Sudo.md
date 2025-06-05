## **Linux CLI – Using Sudo**

### **Permissions Recap**

- Users have different **read (r), write (w), and execute (x)** permissions.
    
- Permissions protect system security by restricting access to files and commands.
    
- Sometimes, users need **temporary elevated privileges** for administrative tasks (e.g., antivirus scans, accessing protected files).
    

---

## **sudo Command**

- **`sudo`** = “**super user do**”
    
- Allows a user to **run commands as another user** (usually root by default).
    
- Controlled by the **`/etc/sudoers`** file:
    
    - Lists users permitted to run sudo.
        
    - Logs unauthorized sudo attempts.
        

### **Common sudo Usage**

- Run a command as superuser:
    
    bash
    
    CopyEdit
    
    `sudo [command]`
    
- Example:
    
    bash
    
    CopyEdit
    
    `sudo cat restricted_file`
    

### **sudo -l**

- Lists all commands the current user is allowed to run with sudo.
    
    bash
    
    CopyEdit
    
    `sudo -l`
    
- Example output:
    
    bash
    
    CopyEdit
    
    `User alice may run the following commands on sudo:   (alice) NOPASSWD: /bin/ls`
    

### **Run as a Different User**

- Use `-u [username]` to run a command as a **specific user**:
    
    bash
    
    CopyEdit
    
    `sudo -u alice cat alice_file`
    

---

## **su Command (Switch User)**

- **`su`** = “**switch user**”
    
- Switches to another user account (defaults to root).
    
- Prompts for the target user’s password.
    
- Once switched, the terminal session runs as that user.
    

### **Usage Examples**

- Switch to root:
    
    bash
    
    CopyEdit
    
    `su`
    
- Switch to another user:
    
    bash
    
    CopyEdit
    
    `su bob`
    
    Then enter Bob’s password.
    

---

## **Commands Overview**

|Command|Description|
|---|---|
|`sudo [command]`|Run a command with root privileges|
|`sudo -u [user] [command]`|Run command as a specified user|
|`sudo -l`|List sudo permissions for current user|
|`su`|Switch to the root user|
|`su [username]`|Switch to a specified user|

---

## **In This Lab**

- Use `sudo -l` to check Alice's allowed commands.
    
- Use `sudo -u alice` to execute commands as Alice.
    
- Switch to Alice's user account using `su alice`.
    
- Read the file `/root/token.txt` after switching to root.
    

---