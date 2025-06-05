## **Packet Capture Basics**

**Category**: Incident Response  
**Objective**: Improve skills in analyzing PCAP files and using packet analysis tools effectively.

---

## **Prerequisites**

- **Prior Tool Knowledge Required**
    
    - Especially tools like **tcpdump** and **Wireshark**
        
- **Recommended Labs Before Starting**:
    
    1. **Packet Analysis: Using tcpdump** – 20 mins (Difficulty: 5/9)
        
    2. **Wireshark: Display Filters – Introduction to Filters** – 10 mins (Difficulty: 4/9)
        
    3. **Packet Analysis: BPF Syntax** – 27 mins (Difficulty: 4/9)
        

---

## **Filters in Packet Analysis**

### **Types of Filters**

- **Capture Filters (BPF Syntax)**:
    
    - Applied _before_ packet capture
        
    - Used to select only desired packets for capture
        
- **Display Filters (Wireshark)**:
    
    - Applied _after_ capture
        
    - Hide non-relevant packets without discarding data
        

### **Examples of Display Filters (Wireshark)**

- `ip.addr == 172.21.2.116` → Filters packets involving this IP
    
- `http contains google.com` → Shows packets with references to “google.com”
    

---

## **Useful Information to Extract from PCAP Files**

- **IP Addresses** (source and destination)
    
- **Web Domains & URLs**
    
- **Filenames**
    
- **Search Queries**
    

---

## **Helpful Tips**

- **Find IPv4 Conversations**:  
    `Statistics > Conversations > IPv4`
    
- **Search Inside Packets**:  
    Use `Ctrl + F` or `Edit > Find Packet` to search for keywords (e.g., web domains).
    

---

## **In This Lab**

- **Goal**: Examine a provided PCAP file and answer questions about the network traffic.
    
- **Tools**: Wireshark (available in the lab environment or for download).
    
- **Skills Developed**:
    
    - Locating specific packet data
        
    - Applying and understanding filters
        
    - Interpreting network traffic for incident response
        

---