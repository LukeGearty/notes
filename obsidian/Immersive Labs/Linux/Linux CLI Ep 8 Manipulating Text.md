## **Linux CLI – Manipulating Text**

### **Why Manipulate Text?**

- Text editors are great for single edits.
    
- For multiple or automated edits, tools like `tr` and `sed` are much more efficient.
    

---

## **`tr` – Translate Characters**

### **Basic Use**

bash

CopyEdit

`tr [original] [replacewith]`

- Replaces all instances of one character with another.
    
- Example:
    
    bash
    
    CopyEdit
    
    `tr a b`
    
    Replaces every `a` with `b`.  
    _Note: Waits for terminal input unless used with redirection._
    

---

### **With Redirection**

- Apply to file input and/or save to output file:
    

bash

CopyEdit

`tr a b < input.txt > output.txt`

- Reads `input.txt`, replaces `a` with `b`, and saves to `output.txt`.
    

---

### **Character Sets**

- Convert lowercase to uppercase:
    
    bash
    
    CopyEdit
    
    `tr [:lower:] [:upper:] < file.txt`
    
- Common character sets:
    
    |Set|Description|
    |---|---|
    |`[:lower:]`|Lowercase letters (a–z)|
    |`[:upper:]`|Uppercase letters (A–Z)|
    |`[:alpha:]`|All letters (a–z, A–Z)|
    |`[:alnum:]`|All letters + digits (A–Z, a–z, 0–9)|
    |`[:digit:]`|All digits (0–9)|
    |`[:punct:]`|All punctuation characters|
    

---

### **Delete Characters**

bash

CopyEdit

`tr -d [:chars:] < file.txt`

- Removes the specified character set from the file.
    

---

## **`sed` – Stream Editor**

### **Basic Use**

bash

CopyEdit

`sed 's/pattern/replacement/g' filename`

- Finds and replaces string patterns.
    
- Outputs result to terminal.
    

**Example:**

bash

CopyEdit

`sed 's/alice/Alice/g' file.txt > newfile.txt`

- Finds all `alice`, replaces with `Alice`, and saves to `newfile.txt`.
    

### **Why use `sed` over `tr`?**

- `tr` only replaces characters.
    
- `sed` can replace full **string patterns**.
    

---

## **Redirection Recap**

- `<` – Input from file
    
- `>` – Output to file
    
- Can be combined in most Linux commands.
    

**Example:**

bash

CopyEdit

`tr a b < input.txt > output.txt`

---

## **Commands Overview**

|Command|Description|
|---|---|
|`sed 's/[original]/[replacewith]/g' filename`|Replace string patterns in a file|
|`tr [original] [replacewith] < filename`|Replace characters from file input|
|`tr [original] [replacewith] > filename`|Save result of character replacement to file|
|`tr [original] [replacewith] < filename > newfile`|Full pipeline: read from file, transform, save output|

---

## **In This Lab**

- Practice using `tr` and `sed`.
    
- Perform controlled text replacements.
    
- Avoid unauthorized changes to preserve the lab token.
    

---