---
# 🛠️ Nmap Scanning Workflow: Step-by-Step 

---

Performing an effective Nmap scan requires a structured sequence to gather detailed information about a target host or network. Below is a streamlined workflow:

---

### **1. Host Discovery (Ping the Host)**

**Goal:** Confirm whether the target is online before proceeding with port scans.

### 🔍 Methods of Host Discovery:

- **ARPing** *(Local Networks Only)*:
    
    Sends ARP requests to find active hosts on the same subnet.
    
- **ICMP Ping**:
    
    Uses ICMP Echo Requests to determine host availability.
    
- **TCP Ping**:
    
    Sends TCP packets (usually to ports 80/443) to check for live hosts where ICMP is blocked.
    
- **UDP Ping**:
    
    Sends UDP packets to provoke responses from devices (can bypass some firewall rules).
    

---

### **2. Reverse DNS Lookup (PTR Record Check)**

**Goal:** Identify the domain name associated with an IP address using a reverse DNS query.

- Provides additional reconnaissance data.
- Useful for host identification and mapping.

---

### **3. Port Scanning (Top 1000 TCP Ports by Default)**

**Goal:** Discover which ports are open on the target host.

- Reveals running services.
- Helps assess the attack surface.
- Nmap default: scans the 1000 most commonly used TCP ports.

---

### **4. Service and Version Detection**

**Goal:** Identify specific services and their versions running on open ports.

- Helps pinpoint potential vulnerabilities.
- Aids in service inventory and threat modeling.

📌 **Example command:**

```bash
nmap -sV target.com

```

---

### **5. OS Detection**

**Goal:** Determine the operating system of the target host.

- Analyzes TCP/IP stack behavior and response patterns.
- Useful for understanding target environment and selecting exploits.

📌 **Example command:**

```bash
nmap -O target.com

```

---

### **6. Save the Scan Output**

**Goal:** Preserve scan results for reporting, analysis, or documentation.

### 🗃️ Common Output Formats:

- **Normal Output:**
    
    ```bash
    nmap -oN output.txt target.com
    
    ```
    
- **XML Output:**
    
    ```bash
    nmap -oX output.xml target.com
    
    ```
    
- **Grepable Output:**
    
    ```bash
    nmap -oG output.gnmap target.com
    
    ```
    

---

## ✅ Summary: Nmap Scanning Workflow

| Step | Task | Purpose |
| --- | --- | --- |
| 1 | Host Discovery | Check if host is online |
| 2 | PTR Record Check | Resolve IP to domain name |
| 3 | Port Scanning | Identify open ports |
| 4 | Service & Version Detection | Detect exact services and versions |
| 5 | OS Detection | Identify target operating system |
| 6 | Save Output | Keep scan results for future use |

---

Let me know if you'd like this formatted as a PDF, Markdown file, or converted into a visual diagram!
