## **Splunk Search – Key Concepts & Usage**

### **Overview**

- Splunk enables **free-form searches** using **Search Processing Language (SPL)**.
    
- You can analyze:
    
    - Raw events
        
    - Logs
        
    - Statistical information
        
- SPL allows for **transformation into charts and visualizations**.
    

---

### **Prerequisite Labs**

- **Recommended**: Review prior lab _"Splunk Basics: Ep.1 – The Splunk Interface"_
    
    - Difficulty: 3/9
        
    - Duration: 15 minutes
        
    - Status: Not started
        

---

### **Anatomy of a Search**

- A **search** consists of **commands separated by the pipe character (`|`)**.
    
    - First string after each pipe = the command
        
    - Following text = command-specific input
        
- **Visualization process**:
    
    1. **Initial table**: All indexed data (rows = events, columns = fields)
        
    2. **Filtered rows**: Based on search terms
        
    3. **Filtered columns**: Summarized (e.g., using `top` command)
        
    4. **Final table**: Refined results (e.g., columns removed)
        

---

### **Search Pipeline**

- A **pipeline** is a chain of commands connected with `|`.
    
- Output from one command is **input for the next**.
    
- Enables **refinement** at each stage.
    

---

### **Fields in Splunk**

- **Fields** = key/value pairs in event data.
    
- Not all fields appear in every event.
    
- Fields can have single or multiple string values.
    

---

### **Quotes and Escaping Characters**

- Use **quotes (" ")** around:
    
    - Phrases with spaces, commas, pipes, or special characters
        
- **Escape characters** with backslash (`\`), e.g.:
    
    - `C:\\Windows\\Temp`
        
- Example:
    
    - `error | stats count` → counts events with “error”
        
    - `"error | stats count"` → searches literal string
        

---

### **Basic Search Usage**

- **Search terms** can include:
    
    - Keywords
        
    - Field names
        
    - Log contents
        
    - Index names
        
- Examples:
    
    - `web`
        
    - `error`
        
    - `web error`
        
    - `"web error"`
        
    - `index="webmain"`
        

#### **Boolean Expressions**

- Supports `AND`, `OR`, `NOT`
    
- Parentheses `()` change evaluation order
    
    - `a=foo AND b=bar OR c=baz` → `a=foo AND (b=bar OR c=baz)`
        
    - `(a=foo AND b=bar) OR c=baz` → different results
        
- `AND` is implied:
    
    - `web error` == `web AND error`
        

#### **Wildcards**

- `*` matches any characters:
    
    - `"my*"` → matches `myhost`, `myhost.domain.com`
        

---

### **Search Assistant**

- Autocomplete for:
    
    - Common terms
        
    - Recent searches
        

---

### **Using Fields to Search**

- Splunk **auto-discovers fields** based on data format.
    
- Fields are shown in the **Fields sidebar**:
    
    - Clicking reveals top 10 values
        
    - Add to search by selecting a value
        
- Syntax:
    
    - `field_name=field_value`
        
- Comparison operators:
    
    - `=`, `!=`, `<`, `>`, `<=`, `>=`
        

---

### **Interpreting Results**

- Main interface: **Events Viewer**
    
- Shows raw event data
    
- Use **“Add to search”** by clicking on event data fields
    

---

### **In This Lab**

- You’re given:
    
    - A sample dataset
        
    - Splunk interface
        
- Objective:
    
    - Query data with increasingly narrow requirements to find key info
        

---