# Nmap (Network Mapper)

## Overview

Nmap is a powerful, open-source tool for network discovery and security auditing, widely used by network administrators and security professionals to map networks, discover hosts, and identify services.

### Purpose

- **Network Discovery**: Identifies live hosts, open ports, and services on a network.
- **Security Auditing**: Detects vulnerabilities and misconfigurations in network systems.
- **Versatility**: Supports scanning of single hosts or large networks with advanced techniques.

### How It Works

- Sends specially crafted packets to target hosts and analyzes their responses.
- Identifies hosts, services, operating systems, and firewall configurations.
- Utilizes raw IP packets, TCP/UDP protocols, and ICMP for comprehensive scanning.

### Compatibility

- Runs on multiple platforms: Linux, Windows, macOS, BSD, Solaris, and more.
- Offers both command-line interface and a graphical user interface (Zenmap).

### Key Features

- Supports advanced scanning techniques: TCP, UDP, ICMP, and IP protocol scans.
- Efficiently handles large-scale network scans or single-host scans.
- Includes Nmap Scripting Engine (NSE) for automated vulnerability detection and custom scripting.
- Recognized globally, featured in films, publications, and awarded for its contributions to cybersecurity.

---

## Nmap Interaction with OSI Model Layers

### 1. Network Layer (Layer 3)

- **Functionality**: Manages packet movement and routing across networks.
- **Nmap Utilization**:
    - **Raw IP Packets**: Sends raw IP packets for host discovery, ping sweeps, and protocol scans.
    - **ICMP**: Uses ICMP Echo Request/Reply for host discovery and ping sweeps.
    - **Packet Manipulation**:
        - Fragmentation (`f`): Breaks packets to evade firewalls/IDS.
        - TTL Control (`-ttl`): Adjusts time-to-live for probing or hop limit management.
        - Source IP Spoofing (`S`): Masks the source IP for stealth.
    - **Firewall/IDS Evasion**:
        - Decoy Scanning (`D`): Sends packets from spoofed addresses to hide the real source.
        - Custom MTU (`-mtu`): Adjusts packet size to complicate detection.
    - **Protocol Scan (`sO`)**: Discovers supported network-layer protocols (e.g., for VPNs or lesser-known services).
- **Sample Commands**:
    - `nmap -f 192.168.1.1` (Fragmented packets)
    - `nmap -sO 192.168.1.1` (Protocol scan)
    - `nmap -S 192.168.1.100 192.168.1.1` (IP spoofing)

### 2. Transport Layer (Layer 4)

- **Functionality**: Ensures end-to-end communication, reliability, and flow control.
- **Nmap Utilization**:
    - **Port Scanning**:
        - **TCP Scans**:
            - SYN Scan (`sS`): "Half-open" scan, sends SYN packets without completing the handshake (stealthy).
            - Connect Scan (`sT`): Completes the TCP three-way handshake.
            - FIN Scan (`sF`), Null Scan (`sN`), Xmas Scan (`sX`): Uses specific TCP flags to probe for firewall evasion.
        - **UDP Scan (`sU`)**: Probes for open UDP ports (e.g., DNS, SNMP, TFTP), slower due to UDP’s connectionless nature.
    - **Service Version Detection (`sV`)**: Identifies software and versions running on open ports.
    - **Firewall/IDS Evasion**:
        - Scan Delay (`-scan-delay`): Controls timing to avoid detection.
        - Max Retries (`-max-retries`): Limits retransmissions to bypass thresholds.
- **Sample Commands**:
    - `nmap -sS 192.168.1.1` (TCP SYN scan)
    - `nmap -sU -p 53 192.168.1.1` (UDP scan on port 53)
    - `nmap -sV 192.168.1.1` (Service version detection)

### 3. ICMP Protocol (Network Layer)

- **Functionality**: Supports error messaging and diagnostics.
- **Nmap Utilization**:
    - Uses ICMP Echo Request/Reply for host discovery (`sn`).
    - Analyzes Destination Unreachable and Time Exceeded responses to detect firewalls and map routes.
    - Traceroute (`-traceroute`): Maps the path to the target.
- **Sample Commands**:
    - `nmap -sn 192.168.1.0/24` (Ping sweep)
    - `nmap --traceroute 192.168.1.1` (Traceroute)

---

## Installation Commands

### Debian/Ubuntu-based Linux (APT)

```bash
sudo apt install nmap

```

### Red Hat/CentOS/Fedora-based Linux (YUM)

```bash
sudo yum install nmap

```

### UNIX Platforms (Solaris, FreeBSD, NetBSD, OpenBSD, etc.) - From Source

1. Download the latest `nmap-<version>.tar.bz2` from the [official Nmap website](https://nmap.org/download.html).
2. Run the following commands:

```bash
bzip2 -dc nmap-<version>.tar.bz2 | tar xvf -
cd nmap-<version>
./configure
make
sudo make install

```

### Notes

- Use `sudo` for commands requiring administrative privileges.
- Package managers (`apt`, `yum`) handle dependencies automatically.
- Building from source is recommended for platforms without pre-compiled binaries or for the latest version.

---

![Screenshot 2025-08-04 201137.png](attachment:0b2ac480-e7d3-4ee6-b96e-c8faf057718d:Screenshot_2025-08-04_201137.png)

## Network Layer Operations in Nmap

The Network Layer (Layer 3) is responsible for routing and forwarding packets. Nmap leverages this layer for advanced discovery and evasion.

### IP Packet Handling

- **Raw IP Packets**: Used for scanning and identifying live hosts.
- **ICMP Echo Requests**: Performs ping sweeps to detect active hosts.
- **Fragmented Packets**: Sends fragmented packets (`f`) to bypass firewalls/IDS.

### Routing & Reachability

- **Route Selection**: Determines optimal paths to targets.
- **Custom Gateways**: Specifies gateways or source IPs (`-source-ip`, `-route-dst`).

### Custom IP Packet Manipulation

- **Raw Packet Crafting**: Supports various scan types (`sS`, `sU`, `A`).
- **TTL Control**: Adjusts TTL (`-ttl`) for probing or hop limit control.
- **Packet Fragmentation**: Uses `f` to fragment packets for evasion.

### Firewall & IDS Evasion Techniques

- **Decoy Scanning (`D`)**: Spoofs source addresses to mask the real source.
- **Custom MTU (`-mtu`)**: Adjusts packet size to evade detection.
- **Protocol Scan (`sO`)**: Identifies supported protocols for discovering services like VPNs.

### Summary

Nmap’s Network Layer capabilities provide robust tools for probing, enumerating, and bypassing network security, offering detailed insights into network topology and host availability.

---

## Transport Layer Operations in Nmap

The Transport Layer (Layer 4) handles end-to-end communication. Nmap uses this layer for detailed port and service scanning.

### Port Scanning

- **TCP Scans**:
    - **SYN Scan (`sS`)**: Sends SYN packets, stealthy due to incomplete handshake.
    - **Connect Scan (`sT`)**: Completes the TCP three-way handshake.
    - **FIN (`sF`), Null (`sN`), Xmas (`sX`) Scans**: Uses specific TCP flags to evade firewalls.
- **UDP Scan (`sU`)**: Probes UDP ports (e.g., DNS, SNMP), slower due to retransmissions.

### Service Version Detection (`sV`)

- Identifies software and versions running on open ports for precise enumeration.

### Firewall/IDS Evasion

- **TCP Flags**: Uses FIN, Null, or Xmas scans to bypass basic firewall rules.
- **Timing Controls**: Adjusts scan speed (`-scan-delay`, `-max-retries`) to avoid triggering IDS/IPS.

### Summary

Nmap’s Transport Layer functionality enables comprehensive port scanning, service detection, and stealthy probing, making it essential for network security assessments.

---

## TCP Three-Way Handshake in Nmap Scans

- **SYN**: Client sends SYN packet with an initial sequence number.
- **SYN-ACK**: Server responds with SYN-ACK and its sequence number.
- **ACK**: Client acknowledges, establishing the connection.
- **Importance**: Ensures reliable communication and supports Nmap’s stealthy scans (e.g., SYN scan) by avoiding full handshakes.

---

## Basic Scanning Examples

- **Scan a specific IP**:
    
    ```bash
    nmap 192.168.1.35
    
    ```
    
- **Scan by hostname**:
    
    ```bash
    nmap target.com
    
    ```
    
- **Verbose scan**:
    
    ```bash
    nmap -v 192.168.1.35
    
    ```
    
- **Scan Nmap’s test server** (authorized for testing):
    
    ```bash
    nmap -v scanme.nmap.org
    
    ```
    

### Help Commands

- **Full Help**:
    
    ```bash
    nmap --help
    
    ```
    
- **Short Help**:
    
    ```bash
    nmap -h
    
    ```
    

---

## Summary

Nmap is an indispensable tool for network exploration and security auditing, leveraging the OSI model’s Network and Transport Layers for advanced scanning, enumeration, and evasion. Its flexibility, combined with powerful features like NSE, makes it a cornerstone of cybersecurity practices.
