## **Linux CLI: Ep.6 – Editing Files**

### **Introduction**

- On the **Linux command line**, you’ll often need to **edit text files**.
    
- CLI is especially useful for editing **hidden files** or config files that are harder to access via GUI.
    
- Files can be created using:
    
    bash
    
    CopyEdit
    
    `touch [filename]`
    

---

## **Text Editors in the CLI**

### **1. Nano**

- A **simple, user-friendly text editor**, commonly pre-installed.
    
- **Open file:**
    
    bash
    
    CopyEdit
    
    `nano [filename]`
    

#### **Nano Shortcuts (shown at bottom of editor)**

|Shortcut|Function|
|---|---|
|`^G`|Help|
|`^O`|Save (Write out)|
|`^X`|Exit|
|`^K`|Cut|
|`^U`|Paste|
|`^W`|Search|
|`^R`|Read file|
|`^\`|Replace|
|`^J`|Justify|
|`^C`|Show cursor pos|
|`^_`|Go to line|
|`^T`|Spell checker|

> Use `CTRL` + [letter] to activate shortcuts (e.g., `CTRL + O` to save).

---

### **2. Vim**

- A **powerful**, modal text editor with steep learning curve but advanced features.
    
- Based on the older `Vi` editor.
    
- **Open file:**
    
    bash
    
    CopyEdit
    
    `vim [filename]`
    

#### **Vim Modes**

|Mode|Description|
|---|---|
|**Command**|Default mode. Move, delete, copy, paste, save, quit.|
|**Insert**|Input text. Enter via `i`.|
|**Visual**|Select text. Enter via `v`.|
|**Command-line**|Enter commands/search. Enter via `:`.|
|**Replace**|Replace text. Enter via `R`.|
|**Select**|Similar to Visual; replaced when typing.|

- **Return to Command mode** at any time using the `Esc` key.
    
- **Open at specific line:**
    
    bash
    
    CopyEdit
    
    `vim +[line_number] [file] # Example: vim +26 ~/.bashrc`
    
- **Get help in Vim:**
    
    bash
    
    CopyEdit
    
    `vim -h`
    

---

## **Commands Overview**

|Command|Description|
|---|---|
|`nano [file]`|Open file with Nano|
|`vim [file]`|Open file with Vim|
|`touch [file]`|Create an empty file|

---

## **In This Lab**

- **Practice using both Nano and Vim**:
    
    - Open files
        
    - Create and edit content
        
    - Save and quit correctly
        
- Only **modify files required** by the lab instructions.
    
- Editing or deleting other files may **break the lab** and **prevent token rewards**.
    

---