## **Linux CLI – Stream Redirection**

### **Introduction**

- Programs in Linux interact through **data streams**.
    
- Stream manipulation is useful for:
    
    - Debugging programs
        
    - Logging output and errors
        
    - Redirecting inputs/outputs
        

---

## **Three Main Data Streams**

|Stream Name|Description|Descriptor|
|---|---|---|
|**stdin**|Standard input (input to program)|`0`|
|**stdout**|Standard output (normal output)|`1`|
|**stderr**|Standard error (error messages)|`2`|

---

## **Standard Input (stdin)**

- Input **into** the program.
    
- Example:
    
    bash
    
    CopyEdit
    
    `cat myfile`
    
    `myfile` is stdin.
    
- Redirect file as input:
    
    bash
    
    CopyEdit
    
    `tr a b < myfile`
    
    This feeds `myfile` into `tr` as stdin.
    

---

## **Standard Output (stdout) & Standard Error (stderr)**

- **stdout**: Normal results/output.
    
- **stderr**: Error messages or warnings.
    

---

## **Redirecting Streams**

### **Redirect stdout**

- Overwrite file:
    
    bash
    
    CopyEdit
    
    `ls > list`
    
    Saves `ls` output to `list` file (overwrites if exists).
    
- Append to file:
    
    bash
    
    CopyEdit
    
    `ls >> list`
    
    Appends `ls` output to bottom of `list`.
    

---

### **File Descriptors**

Use file descriptor numbers to specify which stream to redirect.

|Usage|Description|
|---|---|
|`1>`|Redirect stdout|
|`2>`|Redirect stderr|
|`0<`|Redirect stdin|

---

### **Examples**

- Redirect stdout:
    
    bash
    
    CopyEdit
    
    `some-program 1> output.txt`
    
- Redirect stderr:
    
    bash
    
    CopyEdit
    
    `some-program 2> error.txt`
    
- Redirect both stdout and stderr to different files:
    
    bash
    
    CopyEdit
    
    `some-program 1> output.txt 2> error.txt`
    
- Redirect stderr to stdout (combine both outputs):
    
    bash
    
    CopyEdit
    
    `some-program 1> output-and-error.txt 2>&1`
    

---

## **Special Symbols**

|Symbol|Meaning|
|---|---|
|`>`|Redirect (overwrite)|
|`>>`|Redirect (append)|
|`<`|Redirect input from a file|
|`2>&1`|Redirect stderr to the same place as stdout|

---

## **In This Lab**

- Practice using stream redirection tools.
    
- Redirect outputs and errors into specific files.
    
- Complete guided tasks for stream manipulation.
    

---