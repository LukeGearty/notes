## **Linux CLI – Grep and Sort**

### 🔍 **Purpose**

- While `find` locates files/directories, `grep` and `sort` are for **searching and organizing file content**.
    
- Useful in cybersecurity roles like SOC analysts for log reviews and data parsing.
    

---

## **Grep – Search Inside Files**

### **Basic Usage**

bash

CopyEdit

`grep 'word' filename`

- Searches for `'word'` in `filename` and prints matching lines.
    

### **Search Example**

bash

CopyEdit

`grep 'iron' characters`

**Output:**

nginx

CopyEdit

`iron man   ironheart   ironmonger`

### **Search Multiple Files**

bash

CopyEdit

`grep 'word' file1 file2`

### **Search All Files in Directory**

bash

CopyEdit

`grep 'word' *`

---

### **Redirect Output to File**

bash

CopyEdit

`grep 'word' filename > output-file`

- Saves matching results to a new file instead of printing on-screen.
    

**Example:**

bash

CopyEdit

`grep 'iron' characters > results cat results`

---

## **Sort – Organize Data**

### **Basic Usage**

bash

CopyEdit

`sort filename`

- Sorts file content **alphabetically**, with **numbers before letters**.
    

**Example:**

bash

CopyEdit

`sort usernames`

**Output:**

nginx

CopyEdit

`bbarnes   cdanvers   sstrange   swilson   todinson`

### **Reverse Order**

bash

CopyEdit

`sort -r filename`

**Output:**

nginx

CopyEdit

`todinson   swilson   sstrange   cdanvers   bbarnes`

### **Redirect Output to New File**

bash

CopyEdit

`sort filename > sorted_filename`

- Writes sorted results into a new file.
    

---

### **Explore More Options**

bash

CopyEdit

`sort --help`

- View all available sorting options (e.g., numeric, month, version, etc.).
    

---

## **Command Summary**

|Command|Description|
|---|---|
|`grep 'word' filename`|Search for a word in a file|
|`grep 'word' filename > output.txt`|Save search result to file|
|`sort filename`|Sort file contents alphabetically|
|`sort -r filename`|Sort in reverse order|
|`sort filename > output.txt`|Save sorted output to a new file|

---

## **In This Lab**

- Use `grep` and `sort` to search and organize data.
    
- Practice:
    
    - Grep with output redirection
        
    - Sort files
        
    - Reverse sort
        
- Complete **3 correct commands** to earn a **completion token**.
    

---