### **Base64 Encoding - Summary Notes**

#### **Definition:**

- **Base64** is a binary-to-text encoding scheme.
    
- It converts binary data into an **ASCII string format**.
    
- Used when data must be **stored or transferred** over **text-based systems**.
    

---

#### **How It Works:**

1. **Break binary data into 6-bit blocks** (instead of 8-bit bytes).
    
    - 3 bytes (24 bits) = 4 Base64 characters.
        
    - Ensures output is readable and avoids special ASCII characters.
        
2. **Use 64-character encoding table:**
    
    - Characters:
        
        - 0–9 (10 digits)
            
        - a–z (26 lowercase)
            
        - A–Z (26 uppercase)
            
        - - (plus)
                
        - / (forward-slash)
            
    - **Pad character**: `=` (used to make the last group 24 bits when data isn’t divisible by 3 bytes)
        

---

#### **Example: Encoding `ab@yz`**

- **Binary string**:  
    `01100001 01100010 01000000 01111001 01111010`
    
- **Group into 6-bit blocks**:  
    `011000 010110 001001 000000 011110 010111 101000`  
    _(Added two trailing 0s to complete final 6-bit group)_
    
- **Convert to decimal**:  
    `24, 22, 9, 0, 30, 23, 40`
    
- **Base64 encoding**:  
    `Y, W, J, A, e, X, o`
    
- **Final encoded string**:  
    `YWJAeXo=`
    

---

#### **Why Use Base64 Encoding?**

- Converts binary media (images, video, etc.) for transfer over **ASCII-only systems**.
    
- Prevents issues with **binary misinterpretation** in **email (MIME)**, **XML**, or **SSL certificates**.
    
- Ensures **safe and reliable** data transmission over **text-based protocols**.
    

---

#### **Key Uses:**

- **Email** (MIME format)
    
- **SSL Certificates**
    
- **XML Data**
    
- **Embedding images/videos in web pages or JSON**
    

---