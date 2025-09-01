---

### Overview

- **Default Nmap scanning method** when SYN scan (-sS) is unavailable (e.g., without root/administrator privileges).
- Completes the full **TCP three-way handshake** (SYN, SYN/ACK, ACK) to establish a connection, then terminates it with a RST packet.
- Prioritizes accessibility and simplicity over stealth and speed.

### How It Works

1. **Open Port**:
    - **Client**: Sends a SYN packet to initiate a connection.
    - **Server**: Responds with a SYN/ACK packet, indicating the port is open.
    - **Client**: Sends an ACK packet to complete the handshake.
    - **Client**: Sends a RST packet to terminate the connection.
    - **Flow**:
        
        ```
        1. Client → SYN → Server
        2. Server → SYN/ACK → Client
        3. Client → ACK → Server
        4. Client → RST → Server
        
        ```
        
2. **Closed Port**:
    - **Client**: Sends a SYN packet.
    - **Server**: Responds with a RST/ACK packet, indicating no service is listening on the port.
    - **Flow**:
        
        ```
        1. Client → SYN → Server
        2. Server → RST/ACK → Client
        
        ```
        
3. **Filtered Port**:
    - **Client**: Sends a SYN packet.
    - **Server**: No response (typically due to a firewall blocking packets).
    - **Flow**:
        
        ```
        1. Client → SYN → Server
        2. (No response)
        
        ```
        

### Advantages

- **No Special Privileges Required**: Uses standard OS system calls, allowing non-root users to perform the scan without raw socket access.
- **Reliable**: Full connection completion ensures accurate results, as some services respond differently to full connections.

### Disadvantages

- **Easily Logged**: Full connection is detected and recorded by firewalls, intrusion detection systems (IDS), and server logs.
- **Slower**: Completing the full handshake introduces overhead compared to half-open scans (e.g., SYN scan).

### Use Cases

- Scanning systems where elevated privileges are unavailable.
- Scenarios where stealth is not a priority, and logging is not a concern.
- Differentiating service behavior between full and half-open connections.

### Example Commands

- Verbose TCP Connect scan on port 80:
    
    ```bash
    nmap -v -sT -p 80 192.168.1.46
    
    ```
    
- Verbose TCP Connect scan on port 23:
    
    ```bash
    nmap -v -sT -p 23 192.168.1.46
    
    ```
    

---

## TCP ACK Scan (-sA)  (filtered hai ke nahi) (not entry in log file)

### Overview

- Used to **probe firewall rules** and map network filtering.
- Sends an **ACK packet** (not part of a connection initiation) to determine if ports are **filtered** or **unfiltered**.
- Does not establish or complete a TCP connection, making it stealthier than TCP Connect Scan.

### How It Works

1. **Unfiltered Port (Open or Closed)**:
    - **Client**: Sends an ACK packet to the target port.
    - **Server**: Responds with a RST packet, indicating the port is not filtered by a firewall.
    - **Note**: The response is the same for open or closed ports, as ACK packets are unexpected outside established connections.
    - **Flow**:
        
        ```
        1. Client → ACK → Server
        2. Server → RST → Client
        
        ```
        
2. **Filtered Port**:
    - **Client**: Sends an ACK packet.
    - **Server**: No response, suggesting the packet was dropped or filtered by a firewall.
    - **Flow**:
        
        ```
        1. Client → ACK → Server
        2. (No response)
        
        ```
        

### Advantages

- **Firewall Detection**: Efficiently identifies filtered (blocked) vs. unfiltered ports.
- **Stealthier**: Does not initiate connections, reducing IDS/IPS alerts.
- **Bypasses Some Stateful Firewalls**: Some firewalls allow ACK packets (expecting established connections) while blocking SYN packets.

### Disadvantages

- **Cannot Detect Port State**: Only determines filtering status, not whether a port is open or closed.
- **May Be Blocked**: Some firewalls/IPS drop unexpected ACK packets, reducing effectiveness.

### Use Cases

- Penetration testing to discover firewall rules.
- Network mapping to understand filtering policies.
- Combined with other scans (e.g., SYN, Connect, Null) for comprehensive analysis.

### Example Command

- Verbose TCP ACK scan on port 23:
    
    ```bash
    nmap -v -sA -p 23 192.168.1.51
    
    ```
    

---

## TCP Window Scan (-sW)

### Overview

- An **advanced variation** of the ACK scan that analyzes the **TCP window size** in RST packets to differentiate between **open** and **closed** ports.
- Maintains stealth while providing more detailed port state information than the ACK scan.

### How It Works

1. **Open Port**:
    - **Client**: Sends an ACK packet to the target port.
    - **Server**: Responds with a RST packet.
    - **Key**: If the TCP window size in the RST packet is **nonzero**, the port is considered **open**.
    - **Flow**:
        
        ```
        1. Client → ACK → Server
        2. Server → RST (window size > 0) → Client
        
        ```
        
2. **Closed Port**:
    - **Client**: Sends an ACK packet.
    - **Server**: Responds with a RST packet.
    - **Key**: If the TCP window size is **zero**, the port is considered **closed**.
    - **Flow**:
        
        ```
        1. Client → ACK → Server
        2. Server → RST (window size = 0) → Client
        
        ```
        
3. **Filtered Port**:
    - **Client**: Sends an ACK packet.
    - **Server**: No response, indicating packets may be blocked by a firewall.
    - **Flow**:
        
        ```
        1. Client → ACK → Server
        2. (No response)
        
        ```
        

### Advantages

- **Stealthier**: Avoids connection initiation, reducing logging and IDS/IPS triggers.
- **Firewall Detection**: Identifies filtered ports, similar to ACK scan.
- **Better Differentiation**: Uses TCP window size to distinguish open vs. closed ports, improving accuracy over ACK scan.

### Disadvantages

- **Not Always Effective**: Some systems return RST packets with fixed or non-informative window sizes, reducing reliability.
- **OS-Dependent**: Effectiveness depends on predictable TCP stack behavior.
- **Firewall Interference**: ACK packets may be dropped by firewalls, limiting results.

### Use Cases

- Detecting firewall rules and filtered ports.
- Stealthy reconnaissance to evade logging and alerts.
- Analyzing port states when other scans are inconclusive.

### Example Command

- Verbose TCP Window scan on port 80:
    
    ```bash
    nmap -v -sW -p 80 192.168.1.46
    
    ```
    

---

## TCP Maimon Scan (-sM)

### Overview

- A **stealth scan** discovered by Uriel Maimon, targeting specific TCP/IP stack behaviors (primarily BSD-based systems).
- Sends **FIN/ACK packets** to exploit quirks in how certain systems handle these packets.
- Effective in niche environments but less reliable on modern systems.

### How It Works

1. **Open Port**:
    - **Client**: Sends a FIN/ACK packet.
    - **Server**: Ignores the packet (no response), indicating the port is **open** or **filtered**.
    - **Flow**:
        
        ```
        1. Client → FIN/ACK → Server
        2. (No response)
        
        ```
        
2. **Closed Port**:
    - **Client**: Sends a FIN/ACK packet.
    - **Server**: Responds with a RST packet, indicating the port is **closed**.
    - **Flow**:
        
        ```
        1. Client → FIN/ACK → Server
        2. Server → RST → Client
        
        ```
        
3. **Filtered Port**:
    - **Client**: Sends a FIN/ACK packet.
    - **Server**: No response, suggesting the packet was blocked by a firewall.
    - **Flow**:
        
        ```
        1. Client → FIN/ACK → Server
        2. (No response)
        
        ```
        

### Advantages

- **Stealthy**: Avoids SYN packets, reducing logging and IDS/IPS detection.
- **Firewall Evasion**: Bypasses firewalls that block SYN scans but allow unusual TCP flag combinations (e.g., FIN/ACK).
- **Targets Specific TCP Stacks**: Exploits quirks in BSD-based systems for detecting open ports.

### Disadvantages

- **Limited Scope**: Only effective on systems with specific TCP/IP stack behaviors (e.g., BSD derivatives).
- **Potential Firewall Blocking**: Modern stateful firewalls may drop FIN/ACK packets, reducing effectiveness.
- **Inconsistency**: Some OS treat FIN/ACK packets like FIN packets, leading to unreliable results.

### Use Cases

- Stealth scanning in environments with vulnerable TCP stack implementations (e.g., BSD-based systems).
- Evading firewalls that block SYN scans but allow unusual TCP flag combinations.
- Complementary scan in penetration tests to enhance detection coverage.

### Example Command

- Verbose TCP Maimon scan on port 21:
    
    ```bash
    nmap -v -sM -p 21 192.168.1.46
    
    ```
    

---

### Summary Table

| **Scan Type** | **Flags Used** | **Port State Detection** | **Stealth Level** | **Privileges Required** | **Primary Use Case** |
| --- | --- | --- | --- | --- | --- |
| **TCP Connect (-sT)** | SYN, ACK, RST | Open, Closed, Filtered | Low | None | General scanning without root privileges |
| **TCP ACK (-sA)** | ACK | Filtered, Unfiltered | Moderate | Root (raw sockets) | Firewall rule discovery |
| **TCP Window (-sW)** | ACK | Open, Closed, Filtered | Moderate | Root (raw sockets) | Stealthy port state detection |
| **TCP Maimon (-sM)** | FIN/ACK | Open, Closed, Filtered | High | Root (raw sockets) | Niche stealth scanning (BSD systems) |

---

# Network Scanning UDP, Null, FIN, and Xmas Scans

## UDP Scan (-sU)

### Overview

- Attackers / pentesters check UDP because firewalls may allow it even when TCP is locked down.
- Unlike TCP, UDP has **no handshake** (no SYN/ACK).
- 
- Used to detect **open UDP ports** on a target system.
- UDP is **connectionless**, lacking handshakes, so the scan relies on responses or their absence to infer port status.
- Essential for discovering services invisible to TCP scans (e.g., DNS, DHCP, SNMP).

### How It Works

1. **Open Port**:
    - **Client**: Sends a UDP packet to a target port (e.g., port 53 for DNS).
    - **Server**: Responds with a valid UDP reply, indicating the port is **open**.
    - **Flow**:
        
        ```
        1. Client → UDP (Port 53) → Server
        2. Server → UDP Reply (Port 53) → Client
        
        ```
        
2. **Closed Port**:
    - **Client**: Sends a UDP packet to a target port.
    - **Server**: Responds with an **ICMP Port Unreachable** message (Type 3, Code 3), indicating the port is **closed**.
    - **Flow**:
        
        ```
        1. Client → UDP (Port 53) → Server
        2. Server → ICMP Port Unreachable → Client
        
        ```
        
3. **Open or Filtered Port**:
    - **Client**: Sends a UDP packet to a target port.
    - **Server**: No response, indicating the port is either **open but silent** or **filtered** by a firewall.
    - **Flow**:
        
        ```
        1. Client → UDP (Port 53) → Server
        2. (No response)
        
        ```
        

### Advantages

- **Lower Overhead**: Connectionless nature requires no handshake, making it lightweight.
- **No Connection Limits**: Not constrained by TCP connection limits.
- **Useful for Windows Networks**: Identifies UDP-based services like DNS (53), NetBIOS (137), and SNMP (161).

### Disadvantages

- **Requires Privileged Access**: Needs root/administrator privileges for raw packet manipulation.
- **Slow Scans**: Many UDP services do not respond, slowing down the scan process.
- **ICMP Rate Limiting**: Firewalls may block or limit ICMP Port Unreachable messages, leading to false positives (open/filtered).

### Use Cases

- Discovering UDP-based services (e.g., DNS, DHCP, SNMP, NTP, TFTP).
- Scanning for hidden services in networks that primarily filter TCP.
- Penetration testing Windows environments for services like NetBIOS and MSRPC.

### Example Commands

- Verbose UDP scan on all common ports:
    
    ```bash
    nmap -v -sU 192.168.1.51
    
    ```
    
- Verbose UDP scan on port 53 (DNS):
    
    ```bash
    nmap -v -sU -p 53 192.168.1.51
    
    ```
    

### Common UDP Ports

- 53 (DNS), 67 (DHCP), 69 (TFTP), 123 (NTP), 135 (MSRPC), 137 (NetBIOS), 138 (NetBIOS Datagram), 161 (SNMP), 445 (SMB), 631 (IPP), 1434 (MS SQL Server).

---

## TCP Null Scan (-sN)

### Overview

- A **stealthy scanning technique** that sends TCP packets with **no flags set** (Null packets).
- Determines whether ports are **open**, **closed**, or **filtered** based on responses or lack thereof.
- Effective on non-Windows systems adhering to RFC 793.

### How It Works

1. **Open Port**:
    - **Client**: Sends a TCP packet with no flags.
    - **Server**: No response (per RFC 793), indicating the port is **open** or **filtered**.
    - **Flow**:
        
        ```
        1. Client → None → Server
        2. (No response)
        
        ```
        
2. **Closed Port**:
    - **Client**: Sends a TCP packet with no flags.
    - **Server**: Responds with a RST/ACK packet, indicating the port is **closed**.
    - **Flow**:
        
        ```
        1. Client → None → Server
        2. Server → RST/ACK → Client
        
        ```
        
3. **Filtered Port**:
    - **Client**: Sends a TCP packet with no flags.
    - **Server**: No response, likely due to a firewall dropping the packet.
    - **Flow**:
        
        ```
        1. Client → None → Server
        2. (No response)
        
        ```
        

### Advantages

- **Stealthy**: Avoids triggering firewall/IDS logs by not using SYN packets.
- **Firewall Evasion**: May bypass firewalls that only block SYN packets.
- **Effective on Non-Windows Systems**: Works well on Unix/Linux systems following RFC 793.

### Disadvantages

- **Ineffective on Windows**: Windows treats closed ports as filtered, making results unreliable.
- **Blocked by Modern Firewalls**: Many firewalls detect and block Null scans.
- **Ambiguous for Open Ports**: No response can indicate either open or filtered ports.

### Use Cases

- Evading IDS and firewall logging by avoiding SYN packets.
- Scanning Unix/Linux/BSD systems where RFC 793 compliance is expected.
- Testing firewall rules for SYN packet filtering.

### Example Command

- Verbose TCP Null scan on port 23:
    
    ```bash
    nmap -sN -p 23 192.168.1.46
    
    ```
    

---

## TCP FIN Scan (-sF)

### Overview

- A **stealthy scanning technique** that sends TCP packets with only the **FIN flag** set.
- Identifies **open**, **closed**, or **filtered** ports based on the target’s response.
- Effective on Unix-like systems but less reliable on Windows.

### How It Works

1. **Open Port**:
    - **Client**: Sends a TCP FIN packet.
    - **Server**: No response (per RFC 793), indicating the port is **open** or **filtered**.
    - **Flow**:
        
        ```
        1. Client → FIN → Server
        2. (No response)
        
        ```
        
2. **Closed Port**:
    - **Client**: Sends a TCP FIN packet.
    - **Server**: Responds with a RST/ACK packet, indicating the port is **closed**.
    - **Flow**:
        
        ```
        1. Client → FIN → Server
        2. Server → RST/ACK → Client
        
        ```
        
3. **Filtered Port**:
    - **Client**: Sends a TCP FIN packet.
    - **Server**: No response, likely due to a firewall blocking the packet.
    - **Flow**:
        
        ```
        1. Client → FIN → Server
        2. (No response)
        
        ```
        

### Advantages

- **Stealthy**: Does not establish a TCP session, reducing detection by IDS/IPS.
- **Firewall Evasion**: May bypass firewalls that block only SYN packets.
- **Effective on Unix/Linux**: Works well on systems following RFC 793.

### Disadvantages

- **Ineffective on Windows**: Windows treats closed ports as filtered, reducing reliability.
- **Blocked by Modern Firewalls**: Many firewalls detect and block FIN scans.
- **Open Port Ambiguity**: No response can indicate either open or filtered ports.

### Use Cases

- Evading IDS/IPS that monitor for SYN scans.
- Scanning Unix/Linux environments with consistent RFC 793 behavior.
- Testing firewall rules to identify SYN packet filtering.

### Example Command

- Verbose TCP FIN scan on port 23:
    
    ```bash
    nmap -sF -p 23 192.168.1.46
    
    ```
    

---

## TCP Xmas Scan (-sX)

### Overview

- A **stealthy and advanced scanning technique** that sends TCP packets with **FIN, PSH, and URG flags** set (resembling a “lit-up” Christmas tree).
- Identifies **open**, **closed**, or **filtered** ports based on responses or lack thereof.
- Effective on Unix-like systems but unreliable on Windows.

### How It Works

1. **Open Port**:
    - **Client**: Sends a TCP packet with FIN, PSH, and URG flags.
    - **Server**: No response (per RFC 793), indicating the port is **open** or **filtered**.
    - **Flow**:
        
        ```
        1. Client → FIN/PSH/URG → Server
        2. (No response)
        
        ```
        
2. **Closed Port**:
    - **Client**: Sends a TCP packet with FIN, PSH, and URG flags.
    - **Server**: Responds with a RST/ACK packet, indicating the port is **closed**.
    - **Flow**:
        
        ```
        1. Client → FIN/PSH/URG → Server
        2. Server → RST/ACK → Client
        
        ```
        
3. **Filtered Port**:
    - **Client**: Sends a TCP packet with FIN, PSH, and URG flags.
    - **Server**: No response, likely due to a firewall dropping the packet.
    - **Flow**:
        
        ```
        1. Client → FIN/PSH/URG → Server
        2. (No response)
        
        ```
        

### Advantages

- **Stealthy**: Does not establish sessions, reducing detection in logs.
- **Firewall Evasion**: May bypass stateless firewalls filtering only SYN packets.
- **Effective on Unix-Based Systems**: Works well on Linux, BSD, and other Unix-like OSes.

### Disadvantages

- **Ineffective on Windows**: Windows treats closed ports as filtered, making results unreliable.
- **Easily Detected by Modern Firewalls**: Many firewalls and IDS detect Xmas packets.
- **Ambiguous Open Ports**: No response can indicate either open or filtered ports.

### Use Cases

- Evading IDS/IPS detection by avoiding SYN packets.
- Scanning Unix/Linux systems consistent with RFC 793 behavior.
- Testing firewalls for filtering of SYN vs. other TCP flags.

### Example Command

- Verbose TCP Xmas scan on port 23:
    
    ```bash
    nmap -sX -p 23 192.168.1.46
    
    ```
    

---

## Comparison: FIN Scan vs. Null Scan vs. Xmas Scan

| **Scan Type** | **Flags Sent** | **Open Port Response** | **Closed Port Response** | **Filtered Port Response** | **Works on Windows?** | **Stealth Level** |
| --- | --- | --- | --- | --- | --- | --- |
| **FIN Scan (-sF)** | FIN | No response | RST/ACK | No response | No | High |
| **Null Scan (-sN)** | No flags | No response | RST/ACK | No response | No | High |
| **Xmas Scan (-sX)** | FIN, PSH, URG | No response | RST/ACK | No response | No | High |

---

## Summary Notes

- **UDP Scan (-sU)**:
    - Essential for discovering UDP-based services (e.g., DNS, SNMP) but slow and requires privileged access.
    - Susceptible to ICMP rate limiting, causing ambiguous open/filtered results.
- **TCP Null (-sN), FIN (-sF), and Xmas (-sX) Scans**:
    - **Stealthy** techniques effective on Unix-like systems due to RFC 793 compliance.
    - Ineffective on Windows systems, which treat closed ports as filtered.
    - Limited by modern firewalls detecting unusual TCP flag combinations.
    - Ambiguous results for open ports (no response can mean open or filtered).
- **Use in Combination**: These scans are often used with TCP Connect (-sT), SYN (-sS), or ACK (-sA) scans for comprehensive network reconnaissance.
- **Privilege Requirements**: UDP, Null, FIN, and Xmas scans typically require root privileges for raw packet manipulation, unlike TCP Connect Scan (-sT).
- **Firewall Evasion**: Null, FIN, and Xmas scans excel at bypassing stateless firewalls but are less effective against modern stateful firewalls or IDS/IPS.
