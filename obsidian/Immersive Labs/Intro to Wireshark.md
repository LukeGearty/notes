## **Wireshark Overview**

- **Type**: Free, open-source packet analyzer
    
- **Main Uses**:
    
    - Network troubleshooting
        
    - Network analysis
        
    - Protocol and software development
        
    - Education
        
- **CLI Version**: TShark (non-GUI terminal version)
    

---

## **Key Features**

- Capture live network data or read from packet capture (PCAP) files.
    
- Supports various network types: Ethernet, IEEE 802.11 (Wi-Fi), PPP, LoopBack.
    
- **Display Filters**: Refine visible data for focused analysis.
    
- **Plugin Support**: Extend functionality for new protocols.
    
- **VoIP Analysis**: Detect VoIP calls and play media (if format is supported).
    
- **USB Capture**: Supports raw USB traffic capture.
    
- **Wireless Filtering**: Wi-Fi traffic can be filtered if it passes through monitored Ethernet.
    
- **Customizable Output**: Use filters, timers, and settings to manage traffic display.
    
- **Name Resolution**: Translates IPs and ports into readable names (e.g., port 80 → HTTP).
    

---

## **Using Wireshark**

- **GUI Navigation**: Presents packet data in list format with default views.
    
- **Display Filters Menu**: Accessible via `Analyze > Display Filters`.
    

---

## **Lab Instructions Summary**

- Learn to set up Wireshark for packet sniffing.
    
- Analyze a PCAP file to understand packet flow.
    
- Apply filters to isolate specific traffic types.
    

---

## **Filtering in Wireshark**

- **Purpose**: Locate specific packets or traffic patterns.
    
- **Language**: Wireshark's display filter syntax.
    
- **Example Filters**:
    
    - `dns` → Shows DNS packets
        
    - `tcp.port == 161 or icmp` → Shows SNMP (port 161) and ICMP traffic
        
- **Operators**:
    
    - `==` (equals)
        
    - `&&` (AND)
        
    - `||` (OR)
        
- **Filter Creation Tips**:
    
    - Right-click packet fields in the details pane → "Apply as Filter"
        

---

## **Inspecting Packets**

- **Steps**:
    
    1. Capture or open a PCAP file.
        
    2. Click a packet to reveal:
        
        - **Tree view**: Protocol breakdown
            
        - **Byte view**: Raw data
            
    3. Expand items to analyze deeper protocol details.
        
- **Use Case**: Detect network misuse or anomalies by identifying suspicious patterns.
    

---

## **Helpful Resources**

- Wireshark Official User Guide
    
- Wireshark Display Filter Cheat Sheet
    
- Upcoming/prerequisite lab: **Wireshark: Display Filters – Introduction to Filters**