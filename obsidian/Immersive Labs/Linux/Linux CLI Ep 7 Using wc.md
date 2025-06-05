## **Linux CLI – Word Count (`wc`) Command**

### **Purpose**

- Use `wc` (word count) to **count**:
    
    - **Lines**
        
    - **Words**
        
    - **Characters**
        
- Useful for checking **document limits** or analyzing file content quickly via the command line.
    

---

## **Basic Usage**

### Command:

bash

CopyEdit

`wc [filename]`

### Output format:

css

CopyEdit

`[lines] [words] [characters] [filename]`

**Example:**

bash

CopyEdit

`wc my_log_file`

Output:

arduino

CopyEdit

`145 3464 34567 /home/linux/my_log_file`

Means:

- **145** lines
    
- **3464** words
    
- **34567** characters
    

---

## **Filtered Output (Using Flags)**

You can use flags to count only a specific item:

|Flag|Description|Example|
|---|---|---|
|`-w`|Count **words only**|`wc -w myfile`|
|`-l`|Count **lines only**|`wc -l myfile`|
|`-m`|Count **characters only**|`wc -m myfile`|
|`--help`|Show all options for `wc`|`wc --help`|

---

## **Commands Overview**

|Command|Function|
|---|---|
|`wc [file]`|Lines, words, and characters count|
|`wc -w [file]`|Word count only|
|`wc -l [file]`|Line count only|
|`wc -m [file]`|Character count only|

---

## **In This Lab**

- Practice using `wc` with various files.
    
- Focus on interpreting and using the output correctly.
    
- Try all parameter variations to gain confidence.