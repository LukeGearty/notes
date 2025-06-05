## **Linux CLI – Using SSH and SCP**

### **Remote Access Overview**

- Needed to connect to and interact with **remote systems** (e.g., for file access, updates, remote management).
    
- Common in IT/security workflows.
    
- This episode focuses on **SSH** for secure connection and **SCP** for file transfers.
    

---

## **SSH – Secure Shell**

- **SSH** = Secure Shell
    
- A **secure network protocol** for accessing remote machines.
    
- Replaces older insecure protocols like Telnet.
    
- Encrypts session data and uses **strong authentication methods**.
    

### **Basic SSH Usage**

bash

CopyEdit

`ssh username@host`

- Prompts for the **user’s password**.
    
- Example:
    
    bash
    
    CopyEdit
    
    `ssh linux@10.10.10.10`
    

### **Public-Key Authentication**

- Uses a **key pair**: public and private.
    
- Public key is stored on the server, private key remains with the user.
    
- If the keys match, access is granted without a password.
    
- Usage:
    
    bash
    
    CopyEdit
    
    `ssh -i /path/to/privatekey username@host`
    

---

## **SCP – Secure Copy**

- Used to **transfer files securely** between local and remote systems.
    
- Based on SSH, so it shares encryption and authentication features.
    

### **Download File from Remote to Local**

bash

CopyEdit

`scp username@host:/path/to/remotefile /path/to/localfile`

- Example:
    
    bash
    
    CopyEdit
    
    `scp linux@10.10.10.10:/home/linux/file_to_copy /home/linux/copied_file`
    

### **Upload File from Local to Remote**

bash

CopyEdit

`scp /path/to/localfile username@host:/path/to/remotefile`

- Example:
    
    bash
    
    CopyEdit
    
    `scp /home/linux/myfile linux@10.10.10.10:/home/linux/uploadedfile`
    

### **SCP with Public-Key Authentication**

bash

CopyEdit

`scp -i /path/to/privatekey username@host:/remote/file /local/file`

---

## **Command Overview**

|Command|Description|
|---|---|
|`ssh user@host`|SSH login using password authentication|
|`ssh -i /path/to/key user@host`|SSH login using public-key authentication|
|`scp user@host:/remote/file /local/file`|Download file from remote to local|
|`scp /local/file user@host:/remote/file`|Upload file from local to remote|
|`cat file_name`|View the contents of a file|

---

## **In This Lab**

- Use **SSH**:
    
    - First: connect to the remote system using **password authentication**.
        
    - Then: retrieve Bob’s **SSH private key** using **SCP**.
        
- Use **SCP** to:
    
    - Download the private key file from the remote server.
        
    - Use the key for **public-key authentication** to log in as Bob.
        
- After authenticating as Bob, read the file containing the token.