# Network Scanning Tools: Comprehensive Notes

## nbtscan - NetBIOS Network Scanner

**Overview**:

nbtscan is a command-line tool used to scan a network and collect information about NetBIOS-enabled devices, such as hostnames, MAC addresses, and logged-in users. It sends NetBIOS Name Service queries (UDP port 137) across a subnet or IP range.

**Core NetBIOS Ports**:

- **UDP 137**: NetBIOS Name Service (NBNS) - Resolves NetBIOS names to IP addresses.
- **UDP 138**: NetBIOS Datagram Service - Handles broadcasts for messages and browsing.
- **TCP 139**: NetBIOS Session Service - Provides direct communication, often used by SMB for file and printer sharing.

### Common nbtscan Commands

1. **Scan the entire subnet**
    
    Scans all IPs in the specified subnet (e.g., 192.168.1.1 to 192.168.1.254).
    
    ```bash
    nbtscan 192.168.1.1/24
    
    ```
    
2. **Scan a specific IP range**
    
    Scans a defined range of IP addresses.
    
    ```bash
    nbtscan 192.168.1.1-100
    
    ```
    
    ```bash
    nbtscan 192.168.1.200-254
    
    ```
    
3. **Recursive scan**
    
    Rescans automatically after the first pass for updated information.
    
    ```bash
    nbtscan -r 192.168.1.1/24
    
    ```
    
4. **Custom output formatting**
    
    Uses a colon (`:`) as a separator instead of whitespace for cleaner output.
    
    ```bash
    nbtscan 192.168.1.1/24 -s:
    
    ```
    

### Difference Between nbtstat and nbtscan

| **Feature/Aspect** | **nbtstat (Windows built-in)** | **nbtscan (3rd-party, cross-platform)** |
| --- | --- | --- |
| **Purpose** | Query NetBIOS info on local or specific remote host | Sweep entire networks/IP ranges for NetBIOS info |
| **Scope** | Single machine focus | Network-wide reconnaissance |
| **Platform** | Windows only | Linux, macOS, Windows |
| **Examples** | `nbtstat -n` (local names)<br>`nbtstat -a <IP>` (remote host info) | `nbtscan 192.168.1.1/24` (scans whole subnet) |
| **Output** | NetBIOS name tables, cache, current sessions | Hostnames, MAC addresses, usernames (bulk output) |
| **Use Case** | Troubleshooting or small-scale checks | Security auditing, network mapping, penetration testing |

---

## netdiscover - ARP-based Network Discovery To

only worked on local network not on web , 

approch private ip only.

**Overview**:

netdiscover is a command-line network scanner that identifies live hosts on a local network by sending or listening for ARP (Address Resolution Protocol) requests. It is fast, lightweight, and widely used in penetration testing, network reconnaissance, and administration.

**Modes of Operation**:

- **Passive Mode**:
    - Listens for ARP packets without sending requests.
    - **Advantage**: Stealthy, harder to detect by IDS/IPS.
    - **Limitation**: Only detects hosts already communicating.
- **Active Mode**:
    - Sends ARP requests to a given range or subnet.
    - **Advantage**: Quickly discovers all live hosts.
    - **Limitation**: More detectable on monitored networks.

### Common netdiscover Commands

1. **Passive scan (default)**
    
    Listens on the default interface and displays hosts as they appear.
    
    ```bash
    netdiscover
    
    ```
    
2. **Active scan on a subnet**
    
    Scans all hosts in the specified subnet (e.g., 192.168.1.1 to 192.168.1.254).
    
    ```bash
    netdiscover -r 192.168.1.1/24
    
    ```
    
3. **Specify a network interface**
    
    Runs an active scan via a specific interface (e.g., eth0).
    
    ```bash
    netdiscover -i eth0 -r 192.168.1.1/24
    
    ```
    
4. **Printable/clean output**
    
    Formats output for documentation or exporting results.
    
    ```bash
    netdiscover -p
    
    ```
    
5. **Fast scan mode**
    
    Sends multiple ARP requests concurrently, ideal for large or congested networks.
    
    ```bash
    netdiscover -f -r 192.168.1.1/24
    
    ```
    

### Output Information

- IP address of discovered hosts
- MAC address of each device
- Vendor/manufacturer (from MAC prefix/OUI lookup)
- Hostnames (if resolvable)

### Example Active Scan Output

```
Currently scanning: 192.168.1.0/24 | Screen View: Unique Hosts
IP            At MAC Address       Count  Len  MAC Vendor
192.168.1.5   00:1A:28:3C:4D:5E   5      60   Intel Corporate
192.168.1.10  00:1E:68:9A:BC:DE   3      60   Cisco Systems

```

### Use Cases

- Map devices on a local subnet quickly
- Detect rogue or unauthorized hosts
- Build a network inventory for audits
- Troubleshoot connectivity issues
- Stealthy reconnaissance during penetration tests

**Note**:

- netdiscover requires root privileges to send/receive ARP packets.
- Active scans are faster but more detectable.
- Passive scans are stealthier but depend on existing ARP traffic.

---

## Traceroute-related Tools

if we comppromised a company pc and now we whant to see the the route or we can say the path between from compromised pc to our pc to explore more that network .

We send different packet types (ICMP, UDP, TCP) because **some network devices or firewalls block certain types**, and changing the type helps traceroute discover the path anyway.

**Overview**:

Traceroute tools trace the path packets take to a destination by sending probe packets (ICMP, UDP, or TCP) and measuring responses from each hop (router). They are used for network troubleshooting, latency analysis, and connectivity diagnostics.

### Tools and Their Features

1. **traceroute**
    - **Description**: Default on Linux/macOS. Sends UDP packets (Linux/macOS) or ICMP (some systems) to a target, displaying each hop with round-trip response times. Helps identify delays or packet drops.
    - **Requires**: Root privileges for certain options (raw sockets).
    - **Protocol Customization**: Supports UDP, ICMP, TCP.
    
    **Commands**:
    
    - Basic traceroute:
        
        ```bash
        traceroute 8.8.8.8
        
        ```
        
    - ICMP-based traceroute:
        
        ```bash
        traceroute -I 8.8.8.8
        
        ```
        
    - TCP SYN on port 443:
        
        ```bash
        traceroute -T -p 443 8.8.8.8
        
        ```
        
2. **tcptraceroute**
    - **Description**: Similar to traceroute but uses TCP SYN packets. Useful for bypassing firewalls that block ICMP/UDP but allow TCP (e.g., port 80/443). Common in penetration testing and firewall debugging.
    
    **Command**:
    
    - Trace to port 80:
        
        ```bash
        tcptraceroute 64.6.64.6 80
        
        ```
        
3. **tracepath**
    - **Description**: Linux alternative to traceroute. Sends UDP packets, does not require root privileges, and automatically detects MTU (Maximum Transmission Unit) along the path. Simpler but with fewer options.
    
    **Command**:
    
    - Trace with MTU detection:
        
        ```bash
        tracepath 64.6.64.6
        
        ```
        
4. **tracert (Windows)**
    - **Description**: Windows built-in equivalent of traceroute. Uses ICMP Echo Requests only. Displays hops and round-trip times but cannot bypass firewalls that block ICMP.
    
    **Command**:
    
    - Basic tracert:
        
        ```bash
        tracert 64.6.64.6
        
        ```
        

### Key Differences Between traceroute and tracert

| **Feature** | **tracert (Windows)** | **traceroute (Linux/macOS)** |
| --- | --- | --- |
| **Default Protocol** | ICMP Echo Requests | UDP (Linux/macOS) or ICMP (some) |
| **Platform** | Windows only | Linux, macOS |
| **Firewall Bypass** | No | Yes, with TCP/ICMP options |
| **MTU Detection** | No | Yes, with tracepath |
| **Requires Root** | No | Yes, for many options |
| **Protocol Options** | ICMP only | UDP, ICMP, TCP |

### Summary

- **traceroute/tracert**: General path mapping and latency diagnosis.
- **tcptraceroute**: Bypasses ICMP/UDP blocks, tests TCP connectivity through firewalls.
- **tracepath**: Linux non-root alternative with MTU detection.

**Additional Note**: If you’d like sample outputs (e.g., hop-by-hop listings) for each tool to make this feel like a hands-on lab guide, let me know!

---

## Angry IP Scanner

**Overview**:

Angry IP Scanner is a fast, lightweight, and cross-platform network scanner used to discover live hosts and open ports on a network. It is popular among network administrators, IT professionals, and penetration testers for quick IP range scanning and basic device information gathering.

**Key Features**:

- **Cross-Platform Support**: Works on Windows, macOS, and Linux without requiring installation (portable mode available).
- **Fast Multi-threaded Scanning**: Uses multiple threads to scan IP addresses quickly.
- **IP Range Scanning**: Supports scanning any specified IP range for active devices.
- **Ping Scanning**: Checks if an IP address is alive using ICMP ping requests.
- **Port Scanning**: Detects open TCP ports to identify running network services.
- **Information Gathering**: Retrieves hostnames, MAC addresses, NetBIOS info (computer name, workgroup, user), and more.
- **Plugins**: Extends functionality with Java-based plugins for additional data or automation.
- **Customizable**: Highly configurable with exportable results.
- **Export Results**: Saves scan results in CSV, TXT, XML, or IP-Port list formats.
- **User Interface**: Provides both GUI and command-line interfaces for flexibility.

### Typical Use Cases

- Network inventory and device management
- Identifying active devices and their open ports
- Detecting unauthorized or rogue devices on a network
- Basic penetration testing and security auditing

### Example Command (CLI)

Scans the specified IP range for ports 22, 80, and 443, saving results to a text file.

```bash
ipscan -r 192.168.1.1-192.168.1.255 -p 22,80,443 -o scan_results.txt

```

**Note**: Angry IP Scanner balances speed and ease of use, making it ideal for quick network diagnostics without the complexity of heavier tools like Nmap.

**Additional Note**: If you need details on installation, advanced usage, or specific configurations, let me know!

---

### Notes on Usage

- **Root Privileges**: Tools like netdiscover and some traceroute options require root privileges for raw packet handling.
- **Stealth vs. Speed**: Passive modes (e.g., netdiscover passive scan) are stealthier but slower, while active scans are faster but more detectable.
- **Output Customization**: Tools like nbtscan and Angry IP Scanner allow output formatting for easier integration into reports or scripts.
- **Firewall Considerations**: Use tcptraceroute or traceroute with TCP options to bypass firewalls blocking ICMP/UDP.

If you’d like me to expand on any section, provide sample outputs for traceroute tools, or include additional details (e.g., installation steps for Angry IP Scanner), please let me know!
