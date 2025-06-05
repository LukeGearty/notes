## **Linux CLI: Ep.4 – Changing Things**

### **Introduction**

- The **CLI** allows for efficient **file and directory manipulation**.
    
- This lab focuses on using commands to **create**, **move**, **copy**, and **delete** files and directories.
    
- Recommended: Complete earlier labs first if you're new to the CLI.
    

---

## **Key Commands & Examples**

### **1. Create Directory – `mkdir`**

- **Syntax:** `mkdir [directory-name]`
    
- Creates a **new empty directory**.
    
- Example:
    
    bash
    
    CopyEdit
    
    `mkdir examplefolder`
    
- Check contents with `ls` before and after creation.
    

---

### **2. Create File – `touch`**

- **Syntax:** `touch [filename]`
    
- Creates a **new empty file**.
    
    - If file exists: updates last modified time.
        
    - Add extensions manually: e.g., `touch file.txt`, `touch image.jpg`
        
- Example:
    
    bash
    
    CopyEdit
    
    `touch newfile.txt`
    

---

### **3. Move or Rename File – `mv`**

- **Syntax to move:** `mv [file] [destination]`
    
- **Syntax to rename:** `mv [file] [newname]`
    
- You can also **move and rename** in one command:
    
    bash
    
    CopyEdit
    
    `mv myfile ../newfile`
    
- Example (rename only):
    
    bash
    
    CopyEdit
    
    `mv helloworld.txt hey.txt`
    

---

### **4. Copy File – `cp`**

- **Syntax:** `cp [source] [destination]`
    
- Creates a **duplicate** of the file.
    
- Does **not** affect the original file.
    
- Example:
    
    bash
    
    CopyEdit
    
    `cp villains.txt villains_2.txt`
    

---

### **5. Delete File or Directory – `rm`**

- **Delete file:** `rm [file]`
    
- **Delete directory and contents recursively:** `rm -r [directory]`
    
- **Caution:**
    
    - Files deleted with `rm` are **permanently removed**.
        
    - No Trash/Recycle Bin—**irreversible** without advanced tools.
        
- Example:
    
    bash
    
    CopyEdit
    
    `rm delete_me.txt rm -r old_folder`
    

---

## **Commands Overview**

|Command|Description|
|---|---|
|`mkdir [dir]`|Create new directory|
|`touch [file]`|Create new file (or update timestamp)|
|`mv [file] [name]`|Rename file|
|`mv [file] ..`|Move file to parent directory|
|`cp [file] [copy]`|Copy file|
|`rm [file]`|Permanently delete file|
|`rm -r [dir]`|Permanently delete directory and its contents|

---

## **Lab Activity Instructions**

- Use the **command line** to complete file and directory tasks.
    
- Earn **tokens** by correctly executing commands.
    
- **Important:**
    
    - Only modify files specified in the task.
        
    - Accidental deletion or modification of unrelated files may break the lab.
        
    - Restart the lab environment if this occurs.
        

---