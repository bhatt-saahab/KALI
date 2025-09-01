## IP Header Fields and Nmap Usage

![Screenshot 2025-08-04 201137.png](attachment:4a1f6e7c-48c8-4fb4-80c4-f49ad4297929:Screenshot_2025-08-04_201137.png)

![Screenshot 2025-08-05 204058.png](attachment:6bdf682d-5db1-42d7-aaa3-3579a644c676:Screenshot_2025-08-05_204058.png)

![Screenshot 2025-08-05 204117.png](attachment:593010ec-7168-4918-b71c-97fa895fde78:Screenshot_2025-08-05_204117.png)

The Internet Protocol (IP) header contains fields that Nmap manipulates to perform scans, optimize detection, and evade security controls. Below is a detailed breakdown of each field and its role in Nmap operations.

| **IP Header Field** | **Description** | **Role in Nmap** |
| --- | --- | --- |
| **Version** | Specifies the IP packet version: IPv4 (value 4) by default; IPv6 supported with `-6` option. | Nmap uses `-6` for IPv6 scans (e.g., `nmap -6 2001:db8::1`). |
| **Header Length (IHL)** | Length of the IP header in 32-bit words (default 5 = 20 bytes); increases with options. | Nmap may add options, increasing IHL for advanced scans. |
| **Type of Service (ToS)/DSCP** | Quality of Service bits affecting packet handling/routing. | Customized via `--tos` to influence traffic treatment or evade detection. |
| **Total Length** | Combined header and payload length in bytes. | Nmap manipulates for fragmentation or custom packet crafting. |
| **Identification** | 16-bit field to uniquely identify packet fragments for reassembly. | Used in fragmentation evasion (e.g., `-f` option). |
| **Flags** | Controls fragmentation: DF (Don't Fragment), MF (More Fragments). | Set in fragmented scans (`-f`) to bypass filters. |
| **Fragment Offset** | Indicates the position of a fragment’s data relative to the original packet. | Used in fragmented scans to confuse IDS/firewalls. |
| **Time to Live (TTL)** | Limits packet lifetime (hops), decremented per router to prevent loops. | Used for OS detection (`-O`) and traceroute (`--traceroute`) by analyzing TTL values. |
| **Protocol** | Specifies upper-layer protocol (e.g., 1=ICMP, 6=TCP, 17=UDP). | Used in protocol scans (`-sO`) to enumerate supported protocols. |
| **Header Checksum** | Validates IP header integrity, recalculated with changes. | Automatically calculated by Nmap, not user-manipulated. |
| **Source Address** | IP address of the sender. | Spoofed with `-S <IP>` for stealth or to bypass access controls. |
| **Destination Address** | IP of the target system(s); supports single or subnet notation. | Specified in commands like `nmap 192.168.1.1` or `nmap 192.168.1.0/24`. |
| **Options** | Rare features like Record Route, Timestamp; increases header size. | Used with `--ip-options` for advanced scans (e.g., `nmap --ip-options "\x07\x27\xB\x00" 192.168.1.1`). |

### Additional Details

- **Fragmentation for Evasion**: Nmap uses `f` to break packets into fragments, leveraging **Flags** and **Fragment Offset** to bypass firewalls/IDS that poorly handle fragmented packets.
- **TTL Manipulation**: Different OSes use distinct initial TTL values, enabling OS fingerprinting (`O`). TTL is also used in traceroute to map network hops.
- **Source IP Spoofing**: Spoofing (`S <IP>`) masks scan origins, useful for penetration testing or bypassing IP-based access controls.
- **Protocol Enumeration**: Protocol scans (`sO`) send raw IP packets with varying **Protocol** numbers to identify supported protocols (e.g., TCP, UDP, ICMP).
- **Packet Size and Options**: Manipulating **Total Length** and **Options** allows crafting custom packets for fingerprinting or evasion.

### Summary Table of Nmap IP Header Usage

| **IP Header Field** | **Nmap Usage** | **Typical Commands** |
| --- | --- | --- |
| TTL | OS detection, hop count | `-O`, `--traceroute` |
| Flags & Fragmentation | Firewall/IDS evasion | `-f` |
| Source Address | IP spoofing | `-S <IP>` |
| Protocol | Protocol enumeration | `-sO` |
| Total Length | Fragmentation & custom packets | `-f` |
| Type of Service | QoS & traffic shaping | `--tos` |
| Options | Rare header extensions | `--ip-options` |

## TCP Header Fields and Nmap Usage

The TCP header is critical for Nmap’s scanning techniques, leveraging fields for connection handling, evasion, and fingerprinting.

![Screenshot 2025-08-05 204226.png](attachment:557aa1f3-6fff-40d7-bd87-c6845274cf5f:Screenshot_2025-08-05_204226.png)

| **TCP Header Field** | **Description** | **Nmap Usage** |
| --- | --- | --- |
| **Source Port** | Port used by the sender. | Set via `--source-port` (e.g., `nmap --source-port 53 192.168.1.1`) to evade firewall rules. |
| **Destination Port** | Target port(s) on the receiving host. | Specified with `-p` (e.g., `nmap -p 22,80,443 192.168.1.1` or `-p 1-1024`). |
| **Sequence Number** | Orders TCP segments. | Analyzed in OS fingerprinting (`-O`) due to unique ISN generation per OS. |
| **Acknowledgment Number** | Used in TCP handshake. | Passive; analyzed during connection establishment. |
| **Data Offset** | Specifies TCP header length; increases with options. | Passive; affected by custom options. |
| **Reserved** | Unused, reserved for future use. | Not manipulated by Nmap. |
| **TCP Flags** | Controls connection state: URG, ACK, PSH, RST, SYN, FIN. | Used in various scans:  - **SYN Scan** (`-sS`): Sends SYN, awaits SYN-ACK/RST.  - **FIN Scan** (`-sF`): Sends FIN for evasion.  - **ACK Scan** (`-sA`): Maps firewall rules.  - **Null/Xmas Scans** (`-sN`, `-sX`): No flags or FIN+URG+PSH for evasion. |
| **Window Size** | Indicates buffer size. | Analyzed for OS detection (`-O`). |
| **Checksum** | Ensures packet integrity. | Automatically calculated by Nmap. |
| **Urgent Pointer** | Used if URG flag is set; rare in scanning. | Rarely used by Nmap. |
| **Options** | Supports MSS, Timestamps, SACK, etc. | Crafted with `--tcp-options` (e.g., `nmap --tcp-options "02:04:4000" 192.168.1.1`) for evasion/fingerprinting. |

### How Nmap Uses TCP Fields

- **Source Port**: Setting trusted ports (e.g., 53 for DNS) via `-source-port` can bypass firewall rules.
- **Destination Port**: Nmap targets specific ports or ranges (`p`) to identify open services.
- **Sequence Number**: OS fingerprinting (`O`) analyzes Initial Sequence Numbers for OS identification.
- **Flags**: Various flag combinations enable stealth scans (e.g., `sS`, `sF`) or firewall mapping (`sA`).
- **Window Size**: Default window sizes help identify OSes during fingerprinting.
- **Options**: Custom TCP options enhance evasion or fingerprinting capabilities.

## UDP Header Fields and Nmap Usage

UDP is a connectionless protocol, valued for speed but lacking TCP’s reliability. Nmap’s UDP scans (`-sU`) leverage its simplicity.

![Screenshot 2025-08-05 204316.png](attachment:a48d3a40-b19d-42d1-9ee5-0c31bb93cd78:Screenshot_2025-08-05_204316.png)

| **UDP Header Field** | **Size (Bits)** | **Description** | **Nmap Relevance** |
| --- | --- | --- | --- |
| **Source Port** | 16 | Port used by the sender. | Set via `--source-port` to evade firewall rules. |
| **Destination Port** | 16 | Target port on the receiver. | Specified with `-p` to scan services (e.g., `nmap -sU -p 53 192.168.1.1`). |
| **Length** | 16 | Total size of UDP header plus data. | Used in fragmentation and evasion techniques. |
| **Checksum** | 16 | Validates header and data integrity. | May be set incorrectly for stealth/evasion. |

### How Nmap Uses UDP Fields

- **Source Port**: Setting specific ports can bypass filters expecting trusted traffic.
- **Destination Port**: Targets services like DNS (53) or SNMP (161).
- **Length**: Manipulated for fragmentation to evade detection.
- **Checksum**: Incorrect checksums can be used for stealth scans to confuse targets.

## ICMP Header Fields and Nmap Usage

ICMP operates at the network layer for diagnostics and control. Nmap uses it for host discovery, network mapping, and firewall analysis.

![Screenshot 2025-08-05 204511.png](attachment:c87e2c64-4213-4533-8d0b-d3c1ba29ef75:Screenshot_2025-08-05_204511.png)

| **ICMP Header Field** | **Size (Bits)** | **Description** |
| --- | --- | --- |
| **Type** | 8 | Identifies message kind (e.g., 8 = Echo Request, 0 = Echo Reply). |
| **Code** | 8 | Provides additional info for the message type (e.g., reason for Destination Unreachable). |
| **Checksum** | 16 | Ensures header/data integrity. |
| **Rest of Header** | 32 | Varies; includes sequence numbers, identifiers, timestamps, etc. |

### Common ICMP Message Types

| **Type** | **Code** | **Message** | **Use Case** |
| --- | --- | --- | --- |
| 0 | 0 | Echo Reply | Response to Echo Request (ping reply). |
| 3 | 0-15 | Destination Unreachable | Reports unreachable target (host/port/network). |
| 4 | 0 | Source Quench (deprecated) | Requests slower transmission. |
| 5 | 0-3 | Redirect | Suggests alternate routing. |
| 8 | 0 | Echo Request | Tests connectivity (ping sweep). |
| 11 | 0 | Time Exceeded | Used in traceroute to map network paths. |

### How Nmap Uses ICMP

- **Echo Request/Reply (8/0)**: Used in ping sweeps (`sn`) for host discovery.
- **Destination Unreachable (3)**: Analyzed to identify filtered ports or firewall rules.
- **Time Exceeded (11)**: Used in traceroute (`-traceroute`) to map network paths.
- **Other Types/Codes**: Help detect network issues, route changes, or filtering.

## Summary

Nmap leverages IP, TCP, UDP, and ICMP header fields to perform versatile scans, evade defenses, and fingerprint systems. By manipulating fields like TTL, Flags, Source Address, and Protocol, Nmap achieves stealth, reliability, and detailed network analysis. Understanding these fields enhances the ability to craft effective scans and interpret results accurately.
