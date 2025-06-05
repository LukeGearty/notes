## **Linux CLI – Using `find`**

### **Purpose**

- `find` is a **powerful CLI tool** for searching files and directories based on various filters.
    
- It can search:
    
    - By name
        
    - By partial match
        
    - By permissions
        
    - By ownership
        
    - And more
        
- More advanced and flexible than GUI searches.
    
- Useful for system administrators and general users alike.
    

---

## **Basic Usage**

bash

CopyEdit

`find /directory -name filename`

- Searches `/directory` for `filename`.
    

bash

CopyEdit

`find . -name filename`

- Searches the **current directory**.
    

bash

CopyEdit

`find / -name filename`

- Searches **entire system** (may require `sudo`).
    

---

## **Partial Match**

bash

CopyEdit

`find /directory -name 'word*'`

- Finds files starting with `word` in the given directory.
    

---

## **Search by Permissions**

bash

CopyEdit

`find /directory -perm 700`

- Finds files/directories with **700 permissions** in `/directory`.
    

---

## **Search by User Ownership**

bash

CopyEdit

`find /directory -user username`

- Finds all files/directories owned by the given user.
    

Example:

bash

CopyEdit

`find / -user linux`

- Finds all content owned by the user `linux` in the root directory.
    

---

## **Other Capabilities of `find`**

- **Search by extension/pattern**
    
- **Find and delete files**
    
- **Find empty files/directories**
    
- **Search within files for strings**
    
- **Search by file size**
    

---

## **Suppressing “Permission Denied” Errors**

- Add `2> /dev/null` to hide permission errors:
    

bash

CopyEdit

`find / -name "example" 2> /dev/null`

---

## **Command Summary**

|Command|Description|
|---|---|
|`find /dir -name filename`|Search for an exact filename in a directory|
|`find . -name filename`|Search current directory|
|`find / -name filename`|Search entire file system|
|`find /dir -name 'word*'`|Search for files with partial match|
|`find /dir -perm 700`|Search for files with specific permissions|
|`find /dir -user username`|Search for files owned by a user|

---

## **In This Lab**

- Practice using `find` with different parameters:
    
    - Search by name
        
    - Match patterns
        
    - Filter by permissions
        
    - Find files owned by specific users
        
- Each successful find returns a **token** to submit.
    

---