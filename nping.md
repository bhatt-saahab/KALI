

## Overview

- **Definition**: Nping is a versatile network packet generation tool and ping utility included in the Nmap suite.
- **Purpose**: Enables crafting and sending customized network packets for protocols like TCP, UDP, ICMP, and ARP with fine-grained control over headers and payloads.
- **Use Cases**:
    - Basic host availability checking (ping).
    - Advanced network testing: stress testing, ARP poisoning, denial-of-service (DoS) attacks, route tracing, and response time measurement.
    - Network diagnostics and security auditing.
- **Key Feature - Echo Mode**: Analyzes packet changes during transit, aiding in troubleshooting firewalls, packet corruption, and network path issues.
- **Platforms**: Runs on Linux, Windows, BSD, and macOS.
- **Privileges**: Requires appropriate permissions (e.g., root) for raw socket operations.
- **Caution**: Capable of generating high network traffic; use responsibly.

## Key Features

- **Packet Generation**: Custom TCP, UDP, ICMP, and ARP packets.
- **Multi-Target Support**: Handles multiple target hosts and ports.
- **Echo Mode**: Analyzes packet modifications in transit.
- **Route Tracing**: Supports network path diagnostics.
- **Ethernet Frame Generation**: Crafts Ethernet-level packets.
- **IPv6 Support**: Experimental support for IPv6.
- **Cross-Platform**: Compatible with multiple operating systems.
- **Custom Payloads**: Supports ASCII or random data payloads.
- **Advanced Options**: Spoofing, custom flags, TTL settings, and invalid checksums for specialized testing.

## General Usage

- **Command Structure**: `nping [protocol/options] [target]`
- **Privilege Requirement**: Some operations (e.g., spoofing, raw sockets) require elevated privileges (e.g., `sudo`).
- **Help Command**: Displays detailed options and usage.
    
    ```bash
    nping --help
    
    ```
    

---

## ICMP Examples

1. **Basic ICMP Ping**:
    - Sends ICMP echo requests to check host availability.
    
    ```bash
    nping dns.google
    
    ```
    
2. **ICMP Time Exceeded**:
    - Sends ICMP "time exceeded" packets (used in traceroute diagnostics).
    
    ```bash
    nping --icmp --icmp-type time 192.168.1.27
    
    ```
    

---

## ARP Examples

1. **Single ARP Request**:
    - Checks if a host responds to ARP (Layer 2 discovery).
    
    ```bash
    nping --arp 192.168.1.234
    
    ```
    
2. **Single ARP Request (One Packet)**:
    - Limits to one ARP packet using `c 1`.
    
    ```bash
    nping --arp 192.168.1.234 -c 1
    
    ```
    
3. **ARP Scan for Subnet**:
    - Sends one ARP request per IP in the subnet (e.g., 192.168.1.0/24).
    
    ```bash
    nping --arp 192.168.1.1/24 -c 1
    
    ```
    
4. **ARP Request to Multiple IPs**:
    - Targets specific comma-separated IPs.
    
    ```bash
    nping --arp 192.168.1.1,31,61,200 -c 1
    
    ```
    

---

## TCP Examples

1. **TCP Packet to Port 80 with TTL=2**:
    - Sends TCP packets to port 80 with a TTL of 2 (limits hops).
    
    ```bash
    nping --tcp -p 80 --ttl 2 192.168.1.205
    
    ```
    
2. **TCP SYN Packet to Port 443**:
    - Tests if HTTPS port (443) is open.
    
    ```bash
    nping --tcp -p 443 192.168.1.31
    
    ```
    
3. **TCP RST Packet to Port 443 with TTL=2**:
    - Sends TCP packets with RST flag to attempt connection reset.
    
    ```bash
    nping --tcp -p 443 --flags RST --ttl 2 192.168.1.31
    
    ```
    

---

## UDP Examples

1. **Basic UDP Packet**:
    - Sends a UDP packet to the default port (40125).
    
    ```bash
    nping --udp 192.168.1.51
    
    ```
    
2. **UDP Packet to Specific Port**:
    - Targets port 53 (DNS).
    
    ```bash
    nping --udp -p 53 192.168.1.51
    
    ```
    
3. **UDP to Multiple Ports**:
    - Targets multiple ports (comma-separated or range).
    
    ```bash
    nping --udp -p 53,161,12345 192.168.1.51
    
    ```
    
    ```bash
    nping --udp -p 1000-1010 192.168.1.51
    
    ```
    
4. **Spoof Source Port**:
    - Sets source port to 50505 (requires privileges).
    
    ```bash
    sudo nping --udp -g 50505 -p 53 192.168.1.51
    
    ```
    
5. **Send Specific Number of UDP Packets**:
    - Sends 5 UDP packets to port 53.
    
    ```bash
    nping --udp -p 53 -c 5 192.168.1.51
    
    ```
    
6. **UDP with Custom ASCII Payload**:
    - Sends "HelloUDP" as the packet payload.
    
    ```bash
    nping --udp -p 1337 --data-string "HelloUDP" 192.168.1.234
    
    ```
    
7. **UDP with Random Payload**:
    - Sends 100 bytes of random data.
    
    ```bash
    nping --udp -p 53 --data-length 100 192.168.1.234
    
    ```
    
8. **UDP with Invalid Checksum**:
    - Sends non-RFC-compliant packets to test firewalls/IDS.
    
    ```bash
    nping --udp --badsum -p 53 192.168.1.234
    
    ```
    
9. **Custom UDP Command Example**:
    - Sends 2 UDP packets with a custom message to port 1337.
    
    ```bash
    nping --udp -p 1337 --data-string "CustomMessage" -c 2 192.168.1.234
    
    ```
    

---

## UDP Command Syntax Overview

- `-udp`: Enables UDP protocol mode.
- `p <port>`: Specifies target port(s) (single, comma-separated, or range).
- `g <port>`: Sets source port (requires root privileges).
- `-data-string "DATA"`: Defines custom ASCII payload.
- `-data-length <len>`: Generates random data payload of specified length.
- `c <count>`: Sets the number of packets to send.
- `-badsum`: Uses an invalid UDP checksum for testing.

---

## Echo Mode

- **Purpose**: Runs an Nping echo server to analyze packet changes during transit.
- **Use Case**: Useful for troubleshooting network issues (firewalls, packet corruption, path diagnostics).
- **Command**:
    
    ```bash
    nping --echo-server "public"
    
    ```
    
- **Details**: Clients can target this server with a passphrase (e.g., "public") for echo-based testing.

---

## Best Practices

- **Privileges**: Use `sudo` for operations requiring raw socket access (e.g., spoofing, ARP, or invalid checksums).
- **Responsible Use**: Avoid generating excessive network traffic or performing DoS attacks without permission.
- **Testing Scenarios**: Combine options (ports, payloads, counts) to tailor commands for specific network diagnostics or security tests.
- **Documentation**: Refer to `nping --help` for detailed options and updates.

---

## Notes on Corrections

- The original text contained typographical errors (e.g., "nping" as "nging", "hoing", "noing", or "3mping"). These have been corrected to `nping` for consistency.
- Commands with incorrect flags (e.g.,  instead of `-` for long options) have been standardized (e.g., `-arp`, `-tcp`, `-udp`).
- Clarified ambiguous syntax (e.g., `c 1` for packet count, `-flags RST` for TCP flags).

---

These notes provide a complete and structured reference for using Nping, covering its features, command syntax, and practical examples for network testing and diagnostics. For further details, consult the official Nmap documentation or run `nping --help`.
