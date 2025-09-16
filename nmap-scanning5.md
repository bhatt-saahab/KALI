# Nmap Timing and Performance Options

Nmap provides options to optimize scan speed, stealth, and accuracy by adjusting parallelism, retries, timeouts, and packet rates. These options help tailor scans to network conditions and target environment sensitivity.

## Timing Templates (-T<0-5>)

Predefined timing templates balance speed and stealth.

| Option | Description |
| --- | --- |
| `-T0` | **Paranoid mode**: Serial scan with long delays (most stealthy). |
| `-T1` | **Sneaky mode**: Very slow scan with long delays. |
| `-T2` | **Polite mode**: Slower scan to reduce bandwidth usage. |
| `-T3` | **Normal mode**: Default timing balance. |
| `-T4` | **Aggressive mode**: Faster scan, may trigger IDS/IPS. |
| `-T5` | **Insane mode**: Fastest scan, likely causes packet loss. |

**Examples**:

- `nmap -sS -T4 192.168.1.50`: Performs a SYN scan with aggressive timing.
- ⚡ **`nmap -sS` (SYN scan)** is faster and stealthier than full connect scans because it doesn’t complete the TCP handshake.
- `nmap -T5 192.168.1.1`: Performs a scan with insane timing.

**Copy & Run**:

```bash
nmap -sS -T4 192.168.1.50
nmap -T5 192.168.1.1

```

## Parallel Scanning Options

Control the number of hosts or probes scanned concurrently.

| Option | Description |
| --- | --- |
| `--min-hostgroup <num>` | Minimum number of hosts scanned in parallel. |
| `--max-hostgroup <num>` | Maximum number of hosts scanned in parallel. |
| `--min-parallelism <num>` | Minimum number of probes sent in parallel. |
| `--max-parallelism <num>` | Maximum number of probes sent in parallel. |

**Example**:

- `nmap --min-hostgroup 10 --max-parallelism 20 192.168.1.1/24`: Scans subnet with up to 20 simultaneous probes.

**Copy & Run**:

```bash
nmap --min-hostgroup 10 --max-parallelism 20 192.168.1.1/24

```

## Round Trip Time (RTT) Timeout Options

Tune how long Nmap waits for responses before retrying or timing out.

| Option | Description |
| --- | --- |
| `--min-rtt-timeout <time>` | Minimum wait time for response (e.g., `500ms`). |
| `--max-rtt-timeout <time>` | Maximum wait time for response (e.g., `500ms`). |
| `--initial-rtt-timeout <time>` | Initial RTT timeout estimate (e.g., `500ms`). |

**Example**:

- `nmap --max-rtt-timeout 500ms 192.168.1.1`: Limits RTT timeout to 500 milliseconds.

**Copy & Run**:

```bash
nmap --max-rtt-timeout 500ms 192.168.1.1

```

## Retries and Scan Timeouts

Configure retries and host scanning durations for slow or unresponsive hosts.

| Option | Description |
| --- | --- |
| `--max-retries <num>` | Maximum retries for failed probes. |
| `--host-timeout <time>` | Abort scanning a host if it exceeds time limit (e.g., `5m`). |
| `--scan-delay <time>` | Minimum delay between sending probes (e.g., `10ms`). |
| `--max-scan-delay <time>` | Maximum delay between probes (e.g., `1s`). |

**Example**:

- `nmap --max-retries 2 --host-timeout 5m 192.168.1.1`: Limits retries to 2 and aborts hosts after 5 minutes.

**Copy & Run**:

```bash
nmap --max-retries 2 --host-timeout 5m 192.168.1.1

```

## Packet Rate Control

Control the speed at which packets are sent.

| Option | Description |
| --- | --- |
| `--min-rate <num>` | Minimum packets per second. |
| `--max-rate <num>` | Maximum packets per second. |

**Examples**:

- `nmap -v -Pn -sT -p- --max-rate 10 192.168.1.50`: Scans with a maximum of 10 packets per second.
- `nmap --min-rate 1000 --max-rate 5000 192.168.1.1`: Sends packets at a rate between 1,000 and 5,000 per second.

**Copy & Run**:

```bash
nmap -v -Pn -sT -p- --max-rate 10 192.168.1.50
nmap --min-rate 1000 --max-rate 5000 192.168.1.1

```

# Firewall, IDS, and IPS Evasion & Spoofing in Nmap

Nmap offers techniques to bypass firewalls, Intrusion Detection Systems (IDS), and Intrusion Prevention Systems (IPS) for stealthy reconnaissance and penetration testing.

## Stateless vs Stateful Firewalls

### Stateless Firewall

**How it Works**:

- Examines each packet individually based on predefined rules (source/destination IP, port, protocol).
- No memory of past packets or connection state.

**Features**:

- Simple, fast filtering based on static rules.
- Cannot track TCP session states or packet sequences.
- Effective for blocking/allowing traffic by IP or port.
- Susceptible to spoofing and fragmented packet evasion.
- Requires extensive rule sets.

**Advantages**:

- High speed, low resource consumption.
- Simple configuration for basic filtering.
- Suitable for small networks or high-throughput environments.

**Limitations**:

- Cannot detect complex attacks (e.g., connection hijacking, DoS).
- Ineffective against spoofed packets or dynamic port negotiations.
- Lacks context-aware filtering, leading to security gaps.

### Stateful Firewall

**How it Works**:

- Tracks active connections (TCP streams, UDP flows) in a state table.
- Filters packets based on connection context, allowing only valid session packets.

**Features**:

- Context-aware inspection of packet sequences and connection stages.
- Differentiates legitimate session traffic from illegitimate attempts.
- Supports sophisticated security policies (e.g., blocking unsolicited packets, tracking fragmented packets).

**Advantages**:

- Stronger security with connection state tracking.
- Detects spoofing, session hijacking, and unauthorized traffic.
- Simplifies rule management with automatic return traffic allowances.
- Effective against a wide range of network attacks.

**Limitations**:

- More resource-intensive (CPU, memory) due to state tracking.
- Slightly slower than stateless firewalls under heavy load.
- Higher configuration complexity.

### Summary of Differences

| Aspect | Stateless Firewall | Stateful Firewall |
| --- | --- | --- |
| **Packet Inspection** | Inspects each packet independently | Tracks connection state and inspects packets in context |
| **Connection Awareness** | None | Maintains state tables of active connections |
| **Security Level** | Basic filtering, vulnerable to spoofing | Higher security, detects spoofing and hijacking |
| **Performance** | Fast, less resource-demanding | Slightly slower, more resource-heavy |
| **Configuration** | Manual, extensive ACLs needed | Simpler rules with automatic connection tracking |
| **Use Cases** | Basic filtering, routers, high-throughput | Enterprise/firewall security, VPN gateways |

**Typical Use Cases**:

- **Stateless Firewall**: Simple perimeter filtering, small networks, devices with limited resources, or environments prioritizing speed.
- **Stateful Firewall**: Enterprise networks, data centers, or security-sensitive environments requiring granular traffic control and attack detection.

**Additional Insights**:

- Stateful firewalls operate at OSI Layers 3 and 4, enabling context-aware decisions.
- They detect network attacks relying on connection patterns that stateless firewalls miss.
- Stateless firewalls prioritize performance but offer less protection.

## Common Nmap Evasion Techniques

### Packet Fragmentation in Nmap

**What is Fragmentation?**

- Network packets (e.g., TCP/IP) can be split into smaller fragments, sent separately, and reassembled at the destination.

**Why Fragment Packets?**

- Some firewalls/IDS struggle to reassemble fragmented packets.
- Fragmentation evades signature-based detection and content inspection relying on full packets.
- Bypasses simple packet filters that only examine initial fragments or the first few bytes.

**Fragmentation in Nmap**:

- Use the `f` option or specify a custom MTU (`-mtu`) to send fragmented IP packets.
- Default `f` fragments packets into chunks of 8 bytes or less (after IP header).
- Fragmented probes make it harder for firewalls/IDS to detect or block scanning activity.

**Example**:

- `nmap -f 192.168.1.1`: Sends fragmented packets to evade detection.

**Copy & Run**:na

```bash
nmap -f 192.168.1.1

```

Below are the properly structured, color-coded, and comprehensive notes based on the provided unstructured input. Each command and section is organized clearly, with no information omitted. The notes are formatted for clarity, with separate headings for each technique, command examples, and explanations. I've used a consistent structure and ensured all details are included.

---

# **Nmap Evasion and Scanning Techniques**

## **Fragmentation (-f)**

**Description**: Fragmentation in Nmap is an evasion technique that splits scan packets into smaller fragments to bypass less sophisticated firewalls or Intrusion Detection Systems (IDS). Modern security devices are more adept at handling fragmented packets, reducing the effectiveness of this technique.

### **Example Commands**

1. **Basic Fragmented Scan**
    
    ```bash
    nmap -v -Pn -f 192.168.1.151
    
    ```
    
    - `v`: Verbose output for detailed scan information.
    - `Pn`: Skips ping discovery (assumes host is up).
    - `f`: Enables packet fragmentation.
2. **Fragmented Scan on Specific Port (80)**
    
    ```bash
    nmap -v -Pn -f -f -p 80 192.168.1.151
    
    ```
    
    - `f -f`: Double fragmentation for smaller packet fragments.
    - `p 80`: Scans only port 80.
3. **Verbose Fragmented Scan, Skipping Ping Discovery**
    
    ```bash
    nmap -v -Pn -f 192.168.1.151
    
    ```
    
    - Same as the basic fragmented scan, emphasizing verbose output and no ping.
4. **Fragmented Scan with Custom MTU (16 Bytes)**
    
    ```bash
    nmap --mtu 16 192.168.1.1
    
    ```
    
    - `-mtu 16`: Sets Maximum Transmission Unit to 16 bytes for custom fragmentation.
5. **Verbose No-Ping Fragmented Scan on All Ports**
    
    ```bash
    nmap -v -Pn --mtu 8 -p- 192.168.1.243
    
    ```
    
    - `-mtu 8`: Sets MTU to 8 bytes.
    - `p-`: Scans all ports (1-65535).

### **Limitations and Considerations**

- **Modern Firewalls/IDS**: Advanced firewalls and IDS can reassemble fragmented packets, reducing the effectiveness of this technique.
- **Compatibility Issues**: Fragmented traffic may cause issues with some network devices or hosts.
- **Suspicion and Packet Loss**: Overuse of fragmentation may trigger alerts or lead to packet loss.

### **Summary**

Fragmentation splits packets to evade detection but is less effective against modern security systems due to improved packet reassembly capabilities.

---

## **Decoy Scanning (-D)**

**Description**: Sends scans from multiple fake IP addresses alongside the real scan to obscure the attacker's IP in logs, confusing defenders.

### **Example Commands**

1. **Decoy Scan with Multiple Fake IPs**
    
    ```bash
    nmap -D 192.168.1.10,192.168.1.20,192.168.1.30 192.168.1.151
    
    ```
    
    - `D`: Specifies decoy IP addresses to mask the real source.
2. **Decoy Scan with Additional Fake IPs**
    
    ```bash
    nmap -D 192.168.1.10,192.168.1.20,192.168.1.30,192.168.1.7 192.168.1.1
    
    ```
    
    - Adds an extra decoy IP (192.168.1.7) for increased obfuscation.

---

## **Spoofing Source IP (-S)**

**Description**: Fakes the source IP address to bypass IP-based filtering. Requires direct network access and proper routing.

### **Example Commands**

1. **Basic Source IP Spoofing**
    
    ```bash
    nmap -S 192.168.1.100 192.168.1.1
    
    ```
    
    - `S 192.168.1.100`: Sets the source IP to 192.168.1.100.
2. **Verbose Source IP Spoofing with Interface Specification**
    
    ```bash
    nmap -v -Pn -e eth0 -S 192.168.1.200 192.168.1.1
    
    ```
    
    - `e eth0`: Specifies the network interface (eth0).
    - `S 192.168.1.200`: Spoofs the source IP to 192.168.1.200.

---

## **Spoofing MAC Address (--spoof-mac)**

**Description**: Changes the MAC address to evade detection or impersonate trusted devices.

### **Example Commands**

1. **Spoofing Specific MAC Address**
    
    ```bash
    nmap --spoof-mac 00:11:22:33:44:55 192.168.1.1
    
    ```
    
    - `-spoof-mac 00:11:22:33:44:55`: Sets a custom MAC address.
2. **Spoofing Vendor-Based MAC Address**
    
    ```bash
    nmap -v -Pn --spoof-mac cisco 192.168.1.40
    
    ```
    
    - `-spoof-mac cisco`: Uses a Cisco vendor MAC address prefix.

---

## **Randomizing Source Port (-g or --source-port)**

**Description**: Sets the source port to commonly allowed ports (e.g., 53 for DNS) to bypass basic firewall filtering.

### **Example Commands**

1. **Randomizing Source Port to 53**
    
    ```bash
    nmap -g 53 192.168.1.1
    
    ```
    
    - `g 53`: Sets the source port to 53 (commonly used for DNS).

---

## **Scanning Through Proxies (--proxies)**

**Description**: Routes scan traffic through HTTP/SOCKS proxies to hide the scan’s origin.

### **Example Commands**

1. **Scanning via HTTP Proxy**
    
    ```bash
    nmap --proxies <http://proxy.example.com:8080> 192.168.1.151
    
    ```
    
    - `-proxies`: Specifies the proxy server for routing scan traffic.

---

## **Randomizing Scan Order (--randomize-hosts)**

**Description**: Scans hosts in a randomized order instead of sequentially to make pattern detection harder.

### **Example Commands**

1. **Randomized Scan Across a Subnet**
    
    ```bash
    nmap --randomize-hosts 192.168.1.0/24
    
    ```
    
    - `-randomize-hosts`: Randomizes the order of scanned hosts in the subnet.
2. **Randomized Scan on Specific Port (80)**
    
    ```bash
    nmap --randomize-hosts -p 80 192.168.1.0/24
    
    ```
    
    - `p 80`: Limits the scan to port 80.

---

## **Cloaking Nmap Signature (--data-length)**

**Description**: Appends random data payloads to packets to confuse signature-based IDS.

### **Example Commands**

1. **Appending 50 Bytes of Random Data**
    
    ```bash
    nmap --data-length 50 192.168.1.1
    
    ```
    
    - `-data-length 50`: Adds 50 bytes of random data to packets.
2. **Verbose Scan with 128 Bytes of Random Data**
    
    ```bash
    nmap -v -Pn --data-length 128 192.168.1.151 -p 80
    
    ```
    
    - Adds 128 bytes of random data, targeting port 80.
3. **Custom Data Payload (Hex)**
    
    ```bash
    nmap -v -Pn 192.168.1.151 -p 80 --data 41414141414141
    
    ```
    
    - `-data 41414141414141`: Specifies a custom hex data payload.
4. **Custom Data Payload (Hex, Alternative)**
    
    ```bash
    nmap -v -Pn 192.168.1.40 -p 80 --data 4142434445
    
    ```
    
    - Uses a different hex data payload.
5. **Custom Data Payload (String)**
    
    ```bash
    nmap -v -Pn 192.168.1.151 -p 80 --data-string armour
    
    ```
    
    - `-data-string armour`: Appends the string "armour" as the payload.

---

## **Avoiding Ping (-Pn)**

📡 **`ping` checks if a host is reachable by sending ICMP echo requests and measuring the response time.**

**Description**: Skips host discovery (ping), useful when ICMP is blocked but TCP/UDP traffic is allowed.

### **Example Commands**

1. **No-Ping Scan**
    
    ```bash
    nmap -Pn 192.168.1.1
    
    ```
    
    - `Pn`: Disables ping-based host discovery.

---

## **Custom IP Header Options (--ip-options)**

**Description**: Sets custom IP header options on scan packets to evade or test firewall processing.

### **Example Commands**

1. **Custom IP Header Options**
    
    ```bash
    nmap --ip-options "\\x83\\x03\\x10\\x10\\x00" 192.168.1.100
    
    ```
    
    - `-ip-options`: Specifies custom IP header options.
2. **Verbose No-Ping Scan with Custom TTL**
    
    ```bash
    nmap -v -Pn --ttl 50 192.168.1.151 -p 80
    
    ```
    
    - `-ttl 50`: Sets the Time-To-Live value to 50 for scan packets.

---

## **Nmap Scripting Engine (NSE) - Script Scan (-sC or --script)**

**Description**: The Nmap Scripting Engine (NSE) enhances scanning capabilities by automating tasks like discovery, version detection, vulnerability scanning, and brute-forcing. Scripts are written in Lua for flexibility.

### **Basic Usage and Syntax**

1. **Run Default Scripts**
    
    ```bash
    nmap -sC <target>
    
    ```
    
    - `sC`: Runs default NSE scripts (equivalent to `-script=default`).
2. **Run Specific Script(s)**
    
    ```bash
    nmap --script <script-name> <target>
    
    ```
    
    ```bash
    nmap --script <script1,script2,...> <target>
    
    ```
    
    - Specifies individual scripts or a comma-separated list.
3. **Run Scripts by Category**
    
    ```bash
    nmap --script <category> <target>
    
    ```
    
    - Runs all scripts in a specified category (e.g., `auth`, `discovery`).
4. **Use Wildcards or Expressions**
    
    ```bash
    nmap --script "http-*" <target>
    
    ```
    
    ```bash
    nmap --script "*smb*" <target>
    
    ```
    
    ```bash
    nmap --script "default and not intrusive" <target>
    
    ```
    
    - Uses wildcards or logical expressions to select scripts.
5. **Run Custom Script File**
    
    ```bash
    nmap --script /path/to/custom-script.nse <target>
    
    ```
    
    - Executes a user-provided NSE script.
6. **Full Scan with Default Scripts and Common Options**
    
    ```bash
    nmap -v -Pn -sT -sV -A -O -p- --script=default 192.168.1.208
    
    ```
    
    - Combines verbose output, no ping, TCP connect scan (`sT`), version detection (`sV`), aggressive scan (`A`), OS detection (`O`), all ports (`p-`), and default scripts.

### **Managing and Finding NSE Scripts**

1. **List All Scripts**
    
    ```bash
    ls /usr/share/nmap/scripts/
    
    ```
    
    ```bash
    ls /usr/share/nmap/scripts/ | wc -l
    
    ```
    
    - Displays all available scripts or counts them.
2. **Update Script Database**
    
    ```bash
    nmap --script-updatedb
    
    ```
    
    - Updates the NSE script database.
3. **Explore Scripts by Keyword**
    
    ```bash
    grep smb /usr/share/nmap/scripts/script.db
    
    ```
    
    ```bash
    ls /usr/share/nmap/scripts/ | grep ftp
    
    ```
    
    ```bash
    ls /usr/share/nmap/scripts/ | grep http
    
    ```
    
    - Searches for scripts related to specific protocols (SMB, FTP, HTTP).

### **NSE Script Categories**

| **Category** | **Description** |
| --- | --- |
| `auth` | Authentication bypass, brute-force attacks |
| `broadcast` | Network discovery via broadcast |
| `brute` | Brute-force login attempts |
| `discovery` | Host, service, and configuration discovery |
| `dos` | Denial-of-Service testing |
| `exploit` | Exploit known vulnerabilities |
| `external` | Leverage external services (e.g., WHOIS, Shodan) |
| `fuzzer` | Send unexpected data for robustness testing |
| `intrusive` | May affect targets or trigger alerts |
| `malware` | Detect malware-infected hosts |
| `safe` | No attack or disruption; safe scans |

### **Common and Useful NSE Scripts**

| **Script Name** | **Purpose** |
| --- | --- |
| `ftp-anon` | Checks for anonymous FTP login |
| `smb-brute` | SMB brute-force login |
| `ssh-brute` | SSH password brute-force |
| `http-methods` | Enumerates HTTP methods |
| `http-robots.txt` | Checks and parses robots.txt file |
| `smb-vuln-ms17-010` | Checks SMB vulnerability (EternalBlue) |
| `vuln` | Runs multiple vulnerability detection scripts |
| `modbus-discover` | Enumerates Modbus SCADA devices |

### **Example Commands**

1. **Check Anonymous FTP Login**
    
    ```bash
    nmap -p 21 --script ftp-anon.nse 192.168.1.35
    
    ```
    
2. **SMB Brute-Force Login**
    
    ```bash
    nmap -v -p 445 --script smb-brute 192.168.1.50
    
    ```
    
3. **SSH Brute-Force with Custom User/Password Lists**
    
    ```bash
    nmap -p 22 --script ssh-brute --script-args userdb=usr,passdb=pass 192.168.1.40
    
    ```
    
4. **Enumerate HTTP Methods**
    
    ```bash
    nmap -v -p 80 --script http-methods 192.168.1.35
    
    ```
    
5. **Check robots.txt File**
    
    ```bash
    nmap -v -Pn -sT -sV --script http-robots.txt.nse -p 443 example.com
    
    ```
    
6. **Check SMB Vulnerability (EternalBlue)**
    
    ```bash
    nmap -p 445 --script smb-vuln-ms17-010.nse 192.168.1.15
    
    ```
    
7. **Run Multiple Vulnerability Scripts**
    
    ```bash
    nmap -v -sT --script vuln 192.168.1.15
    
    ```
    
8. **SCADA Modbus Discovery**
    
    ```bash
    nmap -v -Pn -sT -sV -A -O -p- --script scada 192.168.1.208
    
    ```
    

---

## **Summary**

These Nmap techniques enhance stealth and effectiveness during network scanning by evading detection, obscuring the source, or automating advanced tasks via the Nmap Scripting Engine. Fragmentation, decoy scanning, spoofing, and proxy routing help bypass firewalls and IDS, while NSE scripts provide powerful automation for discovery, vulnerability scanning, and more. Effectiveness depends on the target environment, and modern security systems may mitigate some techniques. Always use these tools responsibly and with proper authorization.

---

**Notes**:

- All commands and details from the input have been included and organized.
- Typographical errors (e.g., "птар" instead of "nmap", "Bath" instead of "with") were corrected for clarity.
- Color-coded headings (in bold) separate each technique for readability.
- The structure follows a logical flow: description, example commands, and additional details (e.g., limitations, categories).
- No charts were generated, as the input did not explicitly request them or provide numerical data for plotting.
