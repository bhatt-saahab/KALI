

## fping

**fping** is a command-line network tool designed to send **ICMP echo requests** (pings) to multiple hosts simultaneously, offering greater efficiency than the traditional `ping` command for scanning multiple targets or networks. It is widely used by network administrators for **live host discovery**, **subnet scanning**, and **network availability monitoring**.

### Key Features of fping

- **Parallel Pinging**: Sends ICMP echo requests to multiple hosts concurrently.
- **IPv4 and IPv6 Support**: Works with both IPv4 and IPv6 addresses.
- **Batch Processing**: Can read hostnames or IP addresses from a file for batch pinging.
- **Subnet Scanning**: Supports scanning entire IP ranges or subnets using **CIDR notation**.
- **Customizable Output**: Reports hosts as **alive**, **unreachable**, or provides summarized results.
- **Flexible Options**: Allows customization of **timeout**, **retries**, **packet size**, and **intervals**.
- **Script-Friendly**: Provides easy-to-parse output for scripting and automation.
- **Privileges Required**: Requires appropriate privileges (e.g., root) to send ICMP packets.

### Common fping Commands

1. **Display Help/Usage Information**:
    - `fping --help`
    - `fping -h`
2. **Ping a Single IP Address**:
    - `fping 192.168.1.1`
3. **Ping a Hostname**:
    - `fping dns.google`
4. **Force IPv4 Ping**:
    - `fping -4 www.google.com`
5. **Force IPv6 Ping**:
    - `fping -6 www.google.com`
6. **Ping Multiple IP Addresses**:
    - `fping 192.168.1.1 192.168.1.31 192.168.1.61 192.168.1.200`
7. **Ping Hosts from a File** (e.g., `ip.txt` containing IPs like `192.168.1.1`, `192.168.1.10`, `192.168.1.31`, `192.168.1.61`, `192.168.1.100`):
    - `fping -f ip.txt`
8. **Ping All Hosts in a Subnet (CIDR Notation)**:
    - `fping -g 192.168.1.0/24`
9. **Ping Subnet with Summary Statistics**:
    - `fping -g 192.168.1.0/24 -s`
10. **Show Only Alive Hosts (Suppress Errors)**:
    - `fping -g 192.168.1.0/24 -a 2>/dev/null`
11. **Save Alive Hosts to a File**:
    - `fping -g 192.168.1.0/24 -a 2>/dev/null > live-ip.txt`
12. **Numeric Output (Show IPs Instead of Hostnames)**:
    - `fping -n 8.8.8.8`
13. **Ping with Retry and Custom Interval** (e.g., retry unreachable hosts, set 10000ms interval):
    - `fping -g 192.168.84.1/24 -a -R`
    - `fping -g 192.168.84.1/24 -a -R -i 10000`

---

## hping3

**hping3** is an advanced, versatile command-line network tool for crafting and sending **custom TCP/IP packets**. It is used for **security testing**, **network diagnostics**, **firewall testing**, **port scanning**, and **traceroute-like operations**. Unlike traditional `ping`, which is limited to ICMP echo requests, hping3 supports **TCP**, **UDP**, **ICMP**, and **RAW IP** packets with precise control over flags, payloads, and parameters.

### Key Features of hping3

- **Packet Crafting**: Supports crafting TCP, UDP, ICMP, and RAW IP packets.
- **Custom TCP Flags**: Allows setting flags like **SYN**, **ACK**, **FIN**, **RST**, **URG**, **PSH**, and **Xmas**.
- **Use Cases**: Useful for TCP/IP stack auditing, firewall testing, advanced network scanning, and port scanning.
- **Source Spoofing**: Can spoof source IP addresses and ports for testing.
- **Network Troubleshooting**: Supports traceroute, packet flooding, and network stress testing.
- **Port Scanning**: Customizable port ranges and flag settings for scanning.
- **Privileges Required**: Requires root or administrative privileges to send raw packets.

### Common hping3 Commands

1. **Display Help/Usage**:
    - `hping3 -h`
    - `hping3 --help`
2. **Show Verbose Output**:
    - `hping3 -v`
3. **Send RAW IP Packets**:
    - `hping3 -0 192.168.56.104`
    - `hping3 --rawip 192.168.1.27`
4. **Send a Specific Number of Packets** (e.g., 2 packets):
    - `hping3 -c 2 192.168.1.31`
5. **ICMP Mode** (Similar to ping but with hping3 control):
    - `hping3 -1 192.168.56.104`
    - `hping3 --icmp 192.168.56.104`
    - `hping3 --icmp -c 2 192.168.1.51`
6. **UDP Mode**:
    - `hping3 -2 192.168.1.51`
    - `hping3 --udp 192.168.1.51`
7. **TCP Port Scanning (Port Ranges)**:
    - `hping3 -8 80-1000 192.168.1.51`
    - `hping3 --scan 80-1000 192.168.1.51`
8. **Scan Multiple Ports with SYN Flag and Verbose Output**:
    - `hping3 --scan 1-30,70-90 -S 192.168.1.51`
    - `hping3 --scan 1-30,70-90 -S www.armourinfosec.com`
9. **Scan Specific Ports with FIN Flag**:
    - `hping3 -8 80,443 -F 192.168.1.51`
    - `hping3 --scan 80,443 -S 192.168.1.51`
10. **Spoof Source IP Address**:
    - `hping3 -a 192.168.1.15 192.168.1.51`
11. **Set Specific ICMP Code**:
    - `hping3 -C --force-icmp 192.168.1.51`
12. **Traceroute with hping3**:
    - `hping3 -T 8.8.8.8`

---

### hping3 TCP Flags

hping3 allows crafting TCP packets with specific control flags to test host and firewall behaviors, probe ports, and analyze network stack responses.

### TCP Flag Descriptions

| **Flag** | **Description** | **Typical Use Case** |
| --- | --- | --- |
| **URG** | Urgent pointer is significant, signals urgent data in the TCP stream. | Rarely used in scanning; tests firewall and stack behavior. |
| **FIN** | Finish flag, requests a graceful close of a TCP connection. | Used in stealth FIN scans for port state discovery. |
| **SYN** | Synchronize; initiates a TCP connection. | Key for SYN scans to quickly find listening ports. |
| **RST** | Reset; abrupt termination of TCP connection. | Closes connections or tests firewall blocking. |
| **PSH** | Push; asks receiver to pass data to the application immediately. | Used for protocol or stack behavior tests. |
| **ACK** | Acknowledgment; confirms receipt of data. | Traces firewall rules or for advanced scans. |
| **Xmas** | Combo of FIN, PSH, and URG set ("Christmas Tree" packet). | Used in stealth scans to see how endpoints respond. |

### Example hping3 Commands for TCP Flags

1. **Set URG Flag** (Tests how target/firewall handles urgent data):
    - `hping3 -U 192.168.1.51`
2. **UDP Mode with URG Flag** (Experimental, as UDP has no urgency):
    - `hping3 -2 -U 192.168.1.51`
3. **Set FIN Flag** (Stealth FIN scan for port state discovery):
    - `hping3 -F 192.168.1.51`
4. **Set SYN Flag** (SYN scan to probe open TCP ports without completing handshake):
    - `hping3 -S 192.168.1.51`
5. **Set RST Flag** (Force connection closure or test firewall response):
    - `hping3 -R 192.168.1.51`
6. **Set PSH Flag** (Tests protocol or firewall behavior):
    - `hping3 -P 192.168.1.51`
7. **Set ACK Flag** (Probes system/firewall handling of ACK packets):
    - `hping3 -A 192.168.1.51`
8. **Set Xmas Flag** (Sends FIN, PSH, and URG for stealthy port scanning):
    - `hping3 -X 192.168.1.51`

---

### Summary of Differences

| **Feature** | **fping** | **hping3** |
| --- | --- | --- |
| **Primary Function** | Fast ICMP ping for multiple hosts | Custom TCP/IP packet crafting |
| **Protocols Supported** | ICMP (IPv4/IPv6) | TCP, UDP, ICMP, RAW IP |
| **Use Cases** | Host discovery, subnet scanning | Security testing, firewall testing, port scanning |
| **Customization** | Timeout, retries, intervals | Flags, payloads, port ranges, spoofing |
| **Output** | Simple, script-friendly | Detailed, verbose for diagnostics |
| **Privileges** | Requires ICMP privileges | Requires root for raw packets |

---

### Notes on Usage

- **fping** is ideal for quick, lightweight network scans and host discovery, especially when dealing with large numbers of hosts or subnets.
- **hping3** is more suited for advanced network testing, security auditing, and scenarios requiring precise control over packet structure.
- Both tools require appropriate permissions (e.g., root or sudo) to function fully due to their use of raw network packets.

---

These notes cover all provided details comprehensively, organized for clarity and ease of use. If you need further clarification or specific examples, let me know!
