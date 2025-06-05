## **Tcpdump**

**Category**: Network Analysis / Command-Line Tools  
**Purpose**: Command-line packet analysis tool useful when GUI tools (like Wireshark) are not available.

---

## **Key Features**

- Text-based alternative to **Wireshark**
    
- Can **read** and **analyze** PCAP files
    
- Uses **BPF (Berkeley Packet Filter)** syntax for filtering, similar to Wireshark
    

---

## **Basic Commands**

### **1. Read a PCAP File**

bash

CopyEdit

`tcpdump -r [filename.pcapng]`

- Displays all packets in the file, unfiltered
    

### **2. Show Help Menu**

bash

CopyEdit

`tcpdump --help`

- Lists available options and syntax
    

---

## **Filtering with BPF Syntax**

Use to narrow down analysis to relevant packets (reduces "noise").

### **Filter by IP Address**

bash

CopyEdit

`tcpdump -r [filename.pcapng] host [IPADDRESS]`

- Shows only packets to/from the specified IP
    

---

## **Output Options**

### **1. Write Output to File**

bash

CopyEdit

`tcpdump -r [filename.pcapng] -w [filename.csv/txt]`

- Saves packet output for future reference or structured analysis
    

### **2. Disable DNS and Port Resolution**

bash

CopyEdit

`tcpdump -nn -r [filename.pcapng] -w [filename]`

- **`-nn`**: Prevents name/port resolution → Faster execution and more raw output
    

---

## **In This Lab**

- You will:
    
    - Be provided with a PCAP file
        
    - Use simple **tcpdump** commands
        
    - Answer guided questions to build familiarity with the tool
        

---

## **Why Tcpdump?**

- Useful for **headless systems**, minimal environments, or remote analysis
    
- Great for **quick command-line packet investigation**