## 🧠 **Splunk Advanced Searching (SPL & Transforming) – Key Notes**

### 🔍 **Search Processing Language (SPL) Overview**

- SPL is Splunk’s language for interacting with event data.
    
- Used for **searching, filtering, manipulating, and visualizing** data.
    

---

### 🔑 **Core SPL Components**

1. **Search Terms**
    
    - Keywords/phrases used to filter data.
        
    - Covered extensively in _Ep.3 – Search_.
        
2. **Commands**
    
    - Actions to perform on search results: e.g., `format`, `filter`, `sort`, `count`.
        
    - Example:
        
        spl
        
        CopyEdit
        
        `stats count`
        
3. **Functions**
    
    - Used with commands like `stats` for computations:
        
        - Examples: `avg()`, `sum()`, `min()`, `max()`, `median()`, `var()`
            
4. **Arguments**
    
    - Parameters passed to commands.
        
    - Some are required, e.g.:
        
        spl
        
        CopyEdit
        
        `stats max(field)`
        
5. **Clauses**
    
    - Modify field behavior/output.
        
    - Examples:
        
        - `BY`: group results.
            
        - `AS`: rename fields.
            
        - `WHERE`: filter.
            
        - `AND` / `OR`: logical operators (AND is implied).
            

---

### 📊 **Transforming Commands**

Used to **convert event data into tables or visualizations**.

1. **`chart`**
    
    - Displays data series in chart form.
        
    - Syntax:
        
        spl
        
        CopyEdit
        
        `chart avg(clientip) over date_wday`
        
2. **`timechart`**
    
    - Trends over time.
        
    - Always uses `_time` as the x-axis.
        
    - Syntax:
        
        spl
        
        CopyEdit
        
        `timechart span=12h count BY status`
        
3. **`stats`**
    
    - Generates summary statistics.
        
    - Examples:
        
        spl
        
        CopyEdit
        
        `stats count BY status stats count as "Not Found"`
        
4. **`top`**
    
    - Most frequent values.
        
    - Default limit = 10.
        
    - Example:
        
        spl
        
        CopyEdit
        
        `top dst_port limit=5`
        
5. **`rare`**
    
    - Least frequent values.
        
    - Same options as `top`.
        
    - Example:
        
        spl
        
        CopyEdit
        
        `rare host showperc=f limit=1`
        

---

### 📚 **Types of SPL Commands**

|Category|Description|Examples|
|---|---|---|
|**Streaming**|Operate on each event as it is returned.|`dedup`, `head`, `eval`|
|**Generating**|Fetch data without transforming it.|`search`, `pivot`|
|**Transforming**|Convert data into statistical tables/visuals.|`stats`, `chart`, `top`|
|**Orchestrating**|Control how the search is processed.|`noop`, `localop`|
|**Dataset Processing**|Require the whole dataset to process.|`sort`, `eventstats`|

---

### 🔁 **Subsearches**

- A **search inside a search**, enclosed in square brackets `[ ]`.
    
- The subsearch **executes first**.
    
- Useful for refining searches or combining data sources.
    
- Must begin with a **generating command** (e.g., `search`).
    

---

### 🧪 **In This Lab**

- Provided with a **sample dataset** in Splunk.
    
- Tasks include:
    
    - **Basic searches** using fields from Ep.3.
        
    - **Transforming data** into visuals (e.g., `chart`, `stats`, `top`).
        
    - Investigating for **possible attack indicators**.
        

---