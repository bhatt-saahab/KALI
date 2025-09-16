# Naabu - Fast and Simple Port Scanner

Naabu is a fast, lightweight port scanning tool written in Go, optimized for ease of use, reliability, and integration with attack surface discovery workflows. It performs SYN, CONNECT, and UDP scans to identify open ports on hosts or networks.

## Features

- **Scan Types**: SYN, CONNECT, UDP
- **DNS Port Scan**: Supports scanning by DNS names with automatic IP deduplication
- **IPv4 & IPv6 Support**: IPv4 stable, IPv6 experimental
- **Host Discovery**: Optional host discovery using multiple probe methods
- **Passive Enumeration**: Uses Shodan InternetDB API
- **Nmap Integration**: Run Nmap on discovered ports
- **Input Methods**: Host, list files, IPs, CIDR blocks, ASNs
- **Output Formats**: JSON, TXT, CSV, STDOUT
- **CDN/WAF Exclusion**: Skips full port scan on CDN IPs, scans only ports 80, 443
- **Scan Metrics**: Local metrics endpoint for running scans

## Installation

### Prerequisites

Install `libpcap` for packet capturing:

- **Linux (Debian/Ubuntu)**:
    
    ```bash
    apt install libpcap-dev
    
    ```
    
- **macOS**:
    
    ```bash
    brew install libpcap
    
    ```
    

### Install Naabu

- **Using Go (Recommended)**:
    
    ```bash
    go install -v github.com/projectdiscovery/naabu/v2/cmd/naabu@latest
    
    ```
    
- **Install via Binary Release**:
    1. Download the binary:
        
        ```bash
        wget https://github.com/projectdiscovery/naabu/releases/download/v2.3.5/naabu_2.3.5_linux_amd64.zip
        
        ```
        
    2. Extract the archive:
        
        ```bash
        unzip naabu_2.3.5_linux_amd64.zip
        
        ```
        
    3. Move the binary to `/usr/local/bin`:
        
        ```bash
        cp -v naabu /usr/local/bin
        
        ```
        

## Usage Examples

### Basic Scan

- **Display Help**:
    
    ```bash
    naabu -h
    
    ```
    
- **Update Naabu**:
    
    ```bash
    naabu -up
    
    ```
    
- **Scan ports on a hostname**:
    
    ```bash
    naabu -host hackerone.com
    
    ```
    
- **Scan all TCP ports (1-65535) on a host**:
    
    ```bash
    naabu -p - -host 192.168.1.51
    
    ```
    
- **Scan with 50 concurrent workers**:
- 👉 So yes, `-c 50` just means it will scan **50 ports at the same time**.
    
    ```bash
    naabu -c 50 -p - -host armourinfosec.com
    
    ```
    
- **Scan hosts from a file**:
    
    ```bash
    naabu -list hosts.txt
    
    ```
    
- **Scan an ASN for ports 80 and 443**:
- 👉 ASN works like a **SIM card code** for ISPs — it tells the internet **which company owns your IP network**.
    
    ```bash
    echo AS14421 | naabu -p 80,443
    
    ```
    

### Host Discovery Only

- **Perform only host discovery with ARP/ping probes**:
    
    ```bash
    naabu -sn -host 192.168.1.1/24
    
    ```
    
- **Enable host discovery with TCP SYN ping on port 80**:
    
    ```bash
    naabu -sn -ps 80 -host 192.168.1.1/24
    
    ```
    

### Scan Both IPv4 and IPv6 Addresses

- **Scan all associated IPv4 and IPv6 addresses for a hostname**:
    
    ```bash
    echo hackerone.com | naabu -ip-version 4 -sa -p 80 -silent
    echo hackerone.com | naabu -ip-version 6 -sa -p 80 -silent
    
    ```
    

### Perform Basic Network Scans

- **Scan a Network (Ping Scan)**:
    
    ```bash
    naabu -sn -host 192.168.1.1/24
    
    ```
    
- **Silent Mode Scan**:
    
    ```bash
    naabu -sn -silent -host 192.168.1.1/24
    
    ```
    

### Scan Specific TCP Flags

- **SYN Ping (-ps)**:
    
    ```bash
    naabu -sn -ps -silent -host 192.168.1.1/24
    
    ```
    
- **ACK Ping (-pa)**:
    
    ```bash
    naabu -sn -pa -silent -host 192.168.1.1/24
    
    ```
    

### Scan Specific Hosts/Domains

- **Scan a Single Host**:
    
    ```bash
    naabu -host armourinfosec.com
    
    ```
    
- **Scan All Ports (1-65535)**:
or
    
    ```bash
    naabu -p - -host armourinfosec.com
    
    ```
    
    ```bash
    naabu -p - -host 192.168.1.151
    
    ```
    
- **Set Concurrency for Faster Scanning**:
    
    ```bash
    naabu -c 50 -p - -host armourinfosec.com
    
    ```
    

### Output Results

- **Save Output to a File**:
    
    ```bash
    naabu -host armourinfosec.com -o output.txt
    
    ```
    
- **Verify Ports and Save Output**:
    
    ```bash
    naabu -c 50 -p - -verify -host armourinfosec.com
    
    ```
    

### Customizing Scan Rate and Concurrency

- **Increase Scan Rate and Concurrency**:
    
    ```bash
    naabu -p - -host 192.168.1.51 -c 1000 -rate 1000
    
    ```
    

### Scan with Specific Flags and Types

- **Skip Host Discovery (-Pn)**:
    
    ```bash
    naabu -v -Pn -scan-type c -p - -host 192.168.1.51
    
    ```
    
- **Increase Concurrency and Scan All Ports**:
    
    ```bash
    naabu -v -Pn -scan-type c -c 100 -p - -host 192.168.1.51
    
    ```
    
- **Silent Mode with Maximum Concurrency**:
    
    ```bash
    naabu -silent -Pn -scan-type c -c 1000 -p - -host 192.168.1.51
    
    ```
    
- **Scan from a File**:
or
    
    ```bash
    cat ip2.txt | naabu -silent -Pn -scan-type c -c 1000 -p -
    
    ```
    
    ```bash
    naabu -l ip.txt -p - -c 1000 -rate 1000
    
    ```
    
- **Echo Input for Silent Mode Scanning**:
    
    ```bash
    echo 192.168.1.51 | naabu -silent -Pn -scan-type c -c 1000
    
    ```
    

### Output Options

- **Save Output to a File**:
    
    ```bash
    naabu -host armourinfosec.com -o output.txt
    
    ```
    
- **JSON Format (JSON Lines)**:
    
    ```bash
    naabu -host 104.16.99.52 -json
    
    ```
    
- **CSV Format**:
    
    ```bash
    naabu -host example.com -csv output.csv
    
    ```
    

### Nmap Integration

- **Run Nmap scans on discovered ports automatically (requires Nmap install)**:
    
    ```bash
    echo hackerone.com | naabu -nmap-cli 'nmap -sV'
    
    ```
    

### Advanced Options

- **Rate Limit Packets per Second**:
    
    ```bash
    naabu -rate 1000 -p - -host 192.168.1.51
    
    ```
    
- **Exclude Ports from Scan**:
    
    ```bash
    naabu -p - -exclude-ports 80,443
    
    ```
    
- **Enable CDN/WAF Exclusion**:
    
    ```bash
    naabu -exclude-cdn -host example.com
    
    ```
    

## Notes

- Run Naabu as root for full SYN scanning capabilities.
- Tune concurrency (`c`) and rate (`rate`) based on network and system capabilities.
- Supports arbitrary binary execution for extended integration.
- Designed for mass port scanning and attack surface discovery.

## Resources

- **GitHub Repository**: https://github.com/projectdiscovery/naabu
- **Documentation**: Project Discovery website
- **License**: MIT License
