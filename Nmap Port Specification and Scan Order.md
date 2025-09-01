This guide provides a detailed overview of Nmap’s port specification and scan order features, allowing users to customize scans based on real-world port usage or tailored port lists for efficiency or focus on critical services. The content is organized for clarity and ease of use, with commands, explanations, and practical tips.

---

## 1. Understanding Port Specification in Nmap

Nmap relies on the `nmap-services` file (typically located at `/usr/share/nmap/nmap-services`) to determine port and service data. Each entry in this file contains:

- **Service Name**: e.g., `http`, `ftp`
- **Port Number/Protocol**: e.g., `80/tcp`, `53/udp`
- **Open Frequency**: A decimal value between 0 and 1, indicating how frequently Nmap’s global scans find the port open
- **Optional Comments**: Descriptions of the service

The **open frequency** helps Nmap prioritize scanning by testing more commonly open ports first, optimizing scan performance.

---

## 2. Viewing and Using Port Data

### 2.1 Viewing Top Ports

To view the top ports sorted by open frequency:

```bash
sort -r -k3 /usr/share/nmap/nmap-services | grep -v '^#' | head -n 10

```

- **Explanation**: Sorts the `nmap-services` file by the third column (open frequency) in reverse order (`r`), excludes commented lines (`grep -v '^#'`), and displays the top 10 ports.

### 2.2 Extracting Top TCP Ports

To extract the top 100 TCP ports as a comma-separated list for scanning:

```bash
awk '$2~/tcp$/' /usr/share/nmap/nmap-services | sort -r -k3 | head -n 100 | awk -F'/tcp' '{print $1}' | awk '{print $2}' | tr '\\n' ','

```

- **Explanation**: Filters TCP ports, sorts by open frequency, selects the top 100, extracts port numbers, and formats them into a comma-separated list.

### 2.3 Extracting Top UDP Ports

To extract the top 100 UDP ports as a comma-separated list:

```bash
awk '$2~/udp$/' /usr/share/nmap/nmap-services | sort -r -k3 | head -n 100 | awk -F'/udp' '{print $1}' | awk '{print $2}' | tr '\\n' ','

```

- **Explanation**: Similar to the TCP command but filters for UDP ports.

---

## 3. Common Ways to Specify Ports in Nmap

Nmap provides flexible options to specify which ports to scan, allowing precise control over the scan scope.

### 3.1 Single or Multiple Ports

- Scan a single port:
    
    ```bash
    nmap -p 21 192.168.1.35
    
    ```
    
- Scan multiple specific ports:
    
    ```bash
    nmap -p 21,22,25,80,443 192.168.1.35
    
    ```
    

### 3.2 Port Ranges

- Scan a range of ports:
    
    ```bash
    nmap -p 1-200 192.168.1.35
    
    ```
    
- Scan multiple ranges and specific ports:
    
    ```bash
    nmap -p 1-200,443,5000-5050 192.168.1.35
    
    ```
    

### 3.3 Scan All TCP Ports

- Scan all 65,535 TCP ports:
    
    ```bash
    nmap -p- 192.168.1.35
    nmap -p 0- 192.168.1.35
    
    ```
    
- **Note**: The `p-` flag scans all TCP ports from 1 to 65,535.

### 3.4 Explicit TCP and UDP Ports

- Scan specific TCP and UDP ports together:
    
    ```bash
    nmap -p T:80,443,U:53,161 192.168.1.35
    
    ```
    
- **Format**: Use `T:` for TCP ports and `U:` for UDP ports.

### 3.5 Scan Top N Most Common Ports

- Scan the top 20 most common ports based on open frequency:
    
    ```bash
    nmap --top-ports 20 192.168.1.35
    
    ```
    
- **Note**: The `-top-ports` option uses Nmap’s `nmap-services` data to select the most frequently open ports.

---

## 4. Practical Tips

- **Optimize Scans**: Use `-top-ports` for quick scans when time is limited, as it prioritizes commonly open ports.
- **Custom Port Lists**: Extract port lists from `nmap-services` for targeted scans of specific services or protocols.
- **Combine TCP/UDP**: Explicitly specify `T:` and `U:` when scanning both protocols to avoid ambiguity.
- **Performance**: Scanning all 65,535 ports (`p-`) is thorough but slow; use it only when necessary.

---

This formatted guide provides a clear, professional, and easy-to-read reference for using Nmap’s port specification and scan order features effectively.
