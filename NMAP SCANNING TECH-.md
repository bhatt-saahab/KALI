# Nmap Scan Techniques: Detailed Notes

This document provides comprehensive notes on advanced Nmap scan techniques, including Custom TCP Scan Flags, IP Protocol Scan, SCTP INIT Scan, SCTP COOKIE ECHO Scan, Idle Scan, and FTP Bounce Scan. Each section includes explanations, use cases, advantages, disadvantages, and example commands with options to copy and run.

---

## Custom TCP Scan Flags in Nmap (--scanflags)

The `--scanflags` option in Nmap allows users to manually specify TCP flags in probe packets, enabling highly customizable scans beyond predefined types like SYN (-sS), FIN (-sF), or Xmas (-sX). This is useful for stealthy, evasive, or experimental scans by crafting custom TCP packets.

### How It Works

- Users specify TCP flags (e.g., SYN, ACK, RST, FIN, PSH, URG) to create custom probe packets.

### Common Examples

- Responses depend on the target port state (open, closed, filtered) and the flags used.

### SYN Scan (--scanflags=SYN)

- Mimics the standard SYN scan (-sS).
- **Server Responses**:
    - **Open port**: SYN/ACK response.
    - **Closed port**: RST/ACK response.
    - **Filtered port**: No response.
- **Command**:
    
    ```bash
    nmap -v -p 23 --scanflags=SYN 192.168.1.46
    
    ```
    
    Copy Command
    

### RST Scan (--scanflags=RST)

- Sends packets with the RST flag, which is typically used to close connections.
- Generally ineffective as most systems ignore or drop RST packets.
- **Server Responses**: Rarely reveals useful information.
- **Command**:
    
    ```bash
    nmap -v -p 23 --scanflags=RST 192.168.1.46
    
    ```
    
    Copy Command
    

### ACK Scan (--scanflags=ACK)

- Useful for mapping firewall rules and checking if a port is filtered.
- **Server Responses**:
    - **Unfiltered port**: Sends RST (regardless of open or closed).
    - **Filtered port**: No response.
- **Command**:
    
    ```bash
    nmap -v -p 23 --scanflags=ACK 192.168.1.51
    
    ```
    
    Copy Command
    

### PSH Scan (--scanflags=PSH)

- Sends packets with the PSH (Push) flag, instructing the receiver to process immediately.
- Rarely triggers meaningful responses unless part of an established session.
- **Command**:
    
    ```bash
    nmap -v -p 23 --scanflags=PSH 192.168.1.51
    
    ```
    
    Copy Command
    

### FIN Scan (--scanflags=FIN)

- Similar to the FIN scan (-sF).
- **Server Responses**:
    - **Open port**: No response.
    - **Closed port**: RST/ACK response.
    - **Filtered port**: No response.
- **Command**:
    
    ```bash
    nmap -v -p 23 --scanflags=FIN 192.168.1.51
    
    ```
    
    Copy Command
    

### Combining Multiple TCP Flags

- Combine flags (e.g., SYN, FIN, URG) for custom packets, useful for stealth or IDS/IPS evasion.
- **Command**:
    
    ```bash
    nmap -v -p 23 --scanflags=SYN,FIN,URG 192.168.1.51
    
    ```
    
    Copy Command
    

### Use Cases

- **Bypassing Firewall Rules**: Custom flag combinations may evade firewalls blocking standard scans.
- **Testing IDS/IPS Detection**: Unusual flag combinations may avoid detection by intrusion detection systems.
- **Stealthy Reconnaissance**: Avoids typical TCP connection patterns, reducing logging likelihood.

### Advantages

- Highly customizable for specific scenarios.
- Enables stealthy scans to evade detection.
- Useful for protocol behavior exploration.

### Disadvantages

- Requires advanced understanding of TCP flags.
- May not always yield reliable results depending on target configuration.

---

## IP Protocol Scan (-sO)

The IP Protocol Scan (-sO) probes which IP protocols (e.g., ICMP, TCP, UDP, GRE) are supported by a target system, focusing on protocol-level discovery rather than port scanning.

### How It Works

- Sends IP packets with specific protocol numbers (e.g., ICMP=1, TCP=6, UDP=17).
- **Responses**:
    - **Open Protocol**: Target responds appropriately, confirming protocol support.
    - **Closed Protocol**: Target sends ICMP "Protocol Unreachable" (Type 3, Code 2).
    - **Filtered Protocol**: No response, indicating filtering by firewall.

### Example Commands

- Basic IP Protocol Scan:
    
    ```bash
    nmap -v -sO 192.168.1.51
    
    ```
    
    Copy Command
    
- With packet trace:
    
    ```bash
    nmap -v -sO --packet-trace 192.168.1.51
    
    ```
    
    Copy Command
    
- Disable ARP ping:
    
    ```bash
    nmap -v -sO --disable-arp-ping 192.168.1.51
    
    ```
    
    Copy Command
    
- No ping:
    
    ```bash
    nmap -v -sO -Pn 192.168.1.51
    
    ```
    
    Copy Command
    

### Interpreting Results

- **Supported Protocols**:
    - Example: `Proto (1) open` (ICMP supported).
    - Example: `Proto (6) open` (TCP supported).
- **Unsupported Protocols**:
    - Example: `Proto (47) closed` (GRE not supported).

### Common IP Protocol Numbers

| Protocol | Number | Description |
| --- | --- | --- |
| ICMP | 1 | Ping, network diagnostics |
| IGMP | 2 | Multicast communication |
| TCP | 6 | Reliable communication (HTTP, SSH) |
| UDP | 17 | Connectionless communication (DNS, VoIP) |
| GRE | 47 | VPN tunneling (e.g., PPTP) |
| ESP | 50 | IPSec encryption |
| AH | 51 | IPSec authentication |

### Use Cases

- Testing firewall policies for allowed/disallowed protocols.
- Detecting non-standard or tunneled protocols (e.g., GRE, ESP).
- Recon on restricted networks to uncover hidden protocols.

### Advantages

- Enumerates protocols beyond TCP/UDP.
- Identifies firewall rules and protocol support.

### Disadvantages

- Requires root/admin privileges for raw packet sending.
- Limited detail; only indicates protocol support, not services.
- May trigger IDS/firewall alerts.

---

## SCTP INIT Scan (-sY)

The SCTP INIT Scan (-sY) detects open SCTP (Stream Control Transmission Protocol) ports, commonly used in telephony, SS7 signaling, and industrial networks.

### How It Works

- Sends SCTP INIT packets to target ports.
- **Responses**:
    - **Open Port**: Server responds with INIT-ACK.
    - **Closed Port**: Server responds with ABORT.
    - **Filtered Port**: No response.

### Example Commands

- Basic SCTP INIT Scan:
    
    ```bash
    nmap -v -sY 192.168.1.51
    
    ```
    
    Copy Command
    
- Specific port (2905):
    
    ```bash
    nmap -v -sY -p 2905 192.168.1.51
    
    ```
    
    Copy Command
    
- Specific port (36412):
    
    ```bash
    nmap -v -sY -p 36412 192.168.1.51
    
    ```
    
    Copy Command
    
- Multiple ports:
    
    ```bash
    nmap -v -sY -p 2905,36412 192.168.1.51
    
    ```
    
    Copy Command
    

### Use Cases

- Identifying SCTP services in VoIP/telecom networks.
- Assessing SCTP firewall policies.
- Reconnaissance targeting less common protocols.

### Advantages

- Non-intrusive; does not complete the SCTP handshake.
- Effective for SCTP-specific environments.

### Disadvantages

- Requires root/admin privileges.
- Limited to networks using SCTP.

---

## SCTP COOKIE ECHO Scan (-sZ)

The SCTP COOKIE ECHO Scan (-sZ) confirms open SCTP ports by sending COOKIE ECHO packets, advancing beyond the INIT handshake for more reliable results.

### How It Works

- Sends SCTP INIT, receives INIT-ACK, then sends COOKIE ECHO.
- **Responses**:
    - **Open Port**: Server responds with COOKIE ACK.
    - **Closed Port**: Server responds with ABORT.
    - **Filtered Port**: No response.

### Example Commands

- Basic SCTP COOKIE ECHO Scan:
    
    ```bash
    nmap -v -sZ 192.168.1.46
    
    ```
    
    Copy Command
    
- Specific port (2905):
    
    ```bash
    nmap -v -sZ -p 2905 192.168.1.46
    
    ```
    
    Copy Command
    
- Specific port (36412):
    
    ```bash
    nmap -v -sZ -p 36412 192.168.1.46
    
    ```
    
    Copy Command
    
- Multiple ports:
    
    ```bash
    nmap -v -sZ -p 2905,36412 192.168.1.46
    
    ```
    
    Copy Command
    

### Use Cases

- Confirming SCTP service availability.
- Testing SCTP-based security controls.
- Detailed reconnaissance in SCTP-heavy environments.

### Advantages

- More accurate than SCTP INIT Scan.
- Reduces false positives by completing more of the handshake.

### Disadvantages

- Requires root/admin privileges.
- May trigger IDS/IPS alerts.

---

## Idle Scan (-sI) [Using a Zombie Host]

The Idle Scan (-sI) is a stealthy technique that uses a "zombie" host to scan a target indirectly, hiding the attacker's IP.

### How It Works

1. Find a zombie host with predictable IP ID sequence and low network traffic.
2. Probe the zombie to determine its IP ID.
3. Send SYN packets to the target via the zombie.
4. Analyze zombie’s IP ID changes:
    - **Open Port**: IP ID increases by 2 (SYN/ACK causes zombie to send RST).
    - **Closed Port**: IP ID increases by 1 or unchanged.
    - **Filtered Port**: IP ID unchanged or unpredictable.

### Example Commands

- Basic idle scan:
    
    ```bash
    nmap -sI 192.168.1.100 192.168.1.46
    
    ```
    
    Copy Command
    
- Using DNS name, port 80:
    
    ```bash
    nmap -sI zombie.example.com -p 80 target.com
    
    ```
    
    Copy Command
    
- Multiple ports, no ping:
    
    ```bash
    nmap -sI zombie.example.com -Pn -p 22,443,8080 10.10.10.10
    
    ```
    
    Copy Command
    
- Verbose idle scan:
    
    ```bash
    nmap -v -sI 192.168.1.151 192.168.1.46
    
    ```
    
    Copy Command
    
- No ping, verbose:
    
    ```bash
    nmap -v -Pn -sI 192.168.1.151 192.168.1.46
    
    ```
    
    Copy Command
    

### Use Cases

- Stealthy reconnaissance on monitored networks.
- Evading IDS/firewall logging.
- Scanning isolated networks via internal hosts.

### Advantages

- Complete anonymity; attacker’s IP never contacts the target.
- Bypasses firewalls using trusted zombie hosts.

### Disadvantages

- Requires a suitable zombie host.
- Slow due to indirect communication.
- Unreliable if zombie has high network activity.

---

## FTP Bounce Scan (-b)

The FTP Bounce Scan (-b) leverages an FTP server’s PORT command to scan targets indirectly, hiding the attacker’s IP.

### How It Works

- Attacker connects to an FTP server (anonymous or with credentials).
- Issues PORT command to make the FTP server connect to a target port.
- **Responses**:
    - **Open Port**: FTP server connects successfully.
    - **Closed Port**: FTP server receives RST (connection refused).
    - **Filtered Port**: No response (likely firewall).

### Example Commands

- Anonymous FTP, multiple ports:
    
    ```bash
    nmap -b anonymous@ftp.example.com -p 21,22,80 192.168.1.46
    
    ```
    
    Copy Command
    
- With credentials, port range:
    
    ```bash
    nmap -b user:pass@ftp.example.com -p 1-1000 10.10.10.5
    
    ```
    
    Copy Command
    
- Anonymous, specific ports:
    
    ```bash
    nmap -b anonymous@10.10.10.10 -p 80,443 target.com
    
    ```
    
    Copy Command
    
- With credentials, verbose:
    
    ```bash
    nmap -v -b msfadmin:msfadmin@192.168.1.244 -p 21,22,80 192.168.1.207
    
    ```
    
    Copy Command
    

### Use Cases

- Testing FTP servers for bounce attack vulnerabilities.
- Scanning internal networks behind firewalls.
- Bypassing IP-based access controls.

### Advantages

- Hides attacker’s IP; scan appears from FTP server.
- Bypasses firewalls via exposed FTP servers.

### Disadvantages

- Requires FTP server with PORT command proxying.
- Slow due to proxying.
- Mostly patched in modern FTP servers.

---
