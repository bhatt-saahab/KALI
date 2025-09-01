---

# Penetration Testing Operating Systems

A curated list of popular Linux distributions tailored for ethical hacking, penetration testing, cybersecurity, and security research.

## BackTrack Linux (2006–2013)

- **Overview**: The original penetration testing distribution that laid the groundwork for Kali Linux. No longer maintained but historically significant.
- **History**: Created by merging:
    - **WHAX (Whoppix/WHAX)**: Focused on wireless auditing.
    - **Auditor Security Collection**: Emphasized security tools.
- **Base System**: Initially based on Slackware, later transitioned to Ubuntu.
- **Key Features**:
    - Bootable Live CD.
    - Combined WHAX and Auditor toolsets.
    - Focused on wireless auditing, network scanning, and vulnerability assessment.
- **Evolution of BackTrack Versions**:
    
    
    | Version | Release Year | Base System | Key Notes |
    | --- | --- | --- | --- |
    | BackTrack v1 | 2006 | Slackware | First version, Live CD, merged toolsets. |
    | BackTrack v2 | 2007 | Slackware | Improved hardware detection. |
    | BackTrack v3 | 2008 | Slackware | KDE GUI improvements, expanded toolset. |
    | BackTrack v4 | 2009 | Ubuntu | Shifted to Ubuntu, added persistence. |
    | BackTrack v5 | 2011 | Ubuntu | "Revolution", GNOME/KDE desktop options. |

## Kali Linux

- **Overview**: The most widely used penetration testing and ethical hacking OS, developed and maintained by Offensive Security.
- **Base System**: Debian-based Linux distribution.
- **Purpose**:
    - Penetration Testing
    - Ethical Hacking
    - Digital Forensics
    - Security Research
- **Key Features**:
    - Pre-installed with hundreds of specialized tools for reconnaissance, exploitation, privilege escalation, and more.
    - Regular updates and a comprehensive toolset.
    - Supports Live Boot and persistent installations.
- **Resources**:
    - **Release Archive**: Available on the official Kali Linux website.
    - **Full Tool Listing**: Comprehensive documentation of tools on the Kali Tools site.
    - **History & Introduction**: Detailed on Offensive Security’s resources.

## Parrot OS

- **Overview**: A Debian-based distribution focused on security, privacy, and development.
- **Purpose**:
    - Penetration Testing
    - Digital Forensics
    - Privacy and Anonymity
    - Software Development
- **Key Features**:
    - Lightweight and customizable.
    - Includes tools for penetration testing and privacy (e.g., Tor, AnonSurf).
    - Supports cloud and development environments.

## BlackArch Linux

- **Overview**: An Arch Linux-based distribution designed for security researchers and penetration testers.
- **Key Features**:
    - Offers over 3,000 preconfigured tools for cybersecurity.
    - Highly customizable due to Arch Linux’s rolling release model.
    - Community-driven with extensive documentation.

## BackBox Linux

- **Overview**: An Ubuntu-based distribution designed for penetration testing and security assessments.
- **Key Features**:
    - Focuses on simplicity, performance, and a minimalistic interface.
    - Pre-installed tools for vulnerability assessment, network analysis, and forensics.
    - Regular updates via Ubuntu’s repositories.

---

# Kali Linux Fundamentals

## What is Kali Linux?

Kali Linux is a specialized Debian-based Linux distribution developed by Offensive Security for security professionals. It is designed for:

- **Penetration Testing**: Identifying vulnerabilities in systems and networks.
- **Ethical Hacking**: Simulating cyberattacks to improve security.
- **Digital Forensics**: Analyzing and recovering digital evidence.
- **Security Research**: Developing and testing security tools.

Kali comes pre-installed with hundreds of tools for various cybersecurity domains, including reconnaissance, scanning, exploitation, and post-exploitation.

## Birth of Kali Linux (2013)

- **Introduction**: Launched in March 2013 as the successor to BackTrack Linux.
- **Why the Shift?**:
    - BackTrack lacked robust package management, making updates challenging.
    - Kali Linux adopted Debian’s package management system (APT) for easier updates and maintenance.
    - Improved hardware support and a modernized toolset.

## Basic Linux Commands

Below is a list of essential Linux commands for managing systems, files, and networks in Kali Linux.

### File and Directory Management

- **Show current directory**:
    
    ```bash
    pwd
    
    ```
    
- **List files in a directory**:
    
    ```bash
    ls -l
    
    ```
    
- **Change directory**:
    
    ```bash
    cd /path/to/directory
    
    ```
    
- **View contents of a file**:
    
    ```bash
    cat filename.txt
    
    ```
    
- **Copy a file**:
    
    ```bash
    cp file1.txt /destination/
    
    ```
    
- **Move or rename a file**:
    
    ```bash
    mv oldname.txt newname.txt
    
    ```
    
- **Remove a file or directory**:
    
    ```bash
    rm file.txt
    rm -r folder/
    
    ```
    
- **Search for a string in files**:
    
    ```bash
    grep "search_string" file.txt
    
    ```
    

### System Management

- **View current users**:
    
    ```bash
    who
    
    ```
    
- **Show logged-in user**:
    
    ```bash
    whoami
    
    ```
    
- **Check IP address**:
    
    ```bash
    ip a
    
    ```
    
- **Display running processes**:
    
    ```bash
    ps aux
    
    ```
    
- **Kill a process**:
    
    ```bash
    kill -9 <PID>
    
    ```
    

### User and Permission Basics

- **Create a new user**:
    
    ```bash
    sudo adduser hacker
    
    ```
    
- **Change file permissions**:
    
    ```bash
    chmod 755 script.sh
    
    ```
    
- **Change ownership**:
    
    ```bash
    chown user:user file.txt
    
    ```
    

### Package Management (APT)

- **Update repositories**:
    
    ```bash
    apt update
    
    ```
    
- **Upgrade packages**:
    
    ```bash
    apt upgrade
    
    ```
    
- **Install a tool**:
    
    ```bash
    apt install nmap
    
    ```
    
- **Remove a tool**:
    
    ```bash
    apt remove hydra
    
    ```
    

### Service Management

- **Start a service**:
    
    ```bash
    systemctl start apache2
    
    ```
    
- **Enable service at boot**:
    
    ```bash
    systemctl enable apache2
    
    ```
    
- **Check service status**:
    
    ```bash
    systemctl status ssh
    
    ```
    

### Networking Basics

- **Ping a host**:
    
    ```bash
    ping google.com
    
    ```
    
- **Scan open ports with Nmap**:
    
    ```bash
    nmap -sS 192.168.1.1
    
    ```
    
- **Display routing table**:
    
    ```bash
    route -n
    
    ```
    

### File Compression

- **Create a tar.gz archive**:
    
    ```bash
    tar -czvf archive.tar.gz folder/
    
    ```
    
- **Extract a tar.gz archive**:
    
    ```bash
    tar -xzvf archive.tar.gz
    
    ```
    

## Kali-Specific Commands

- **Launch Metasploit**:
    
    ```bash
    msfconsole
    
    ```
    
- **Run Social Engineering Toolkit (SET)**:
    
    ```bash
    setoolkit
    
    ```
    
- **Start Burp Suite**:
    
    ```bash
    burpsuite
    
    ```
    
- **Start airmon-ng for wireless monitoring**:
    
    ```bash
    airmon-ng start wlan0
    
    ```
    

## Privilege Escalation Tips

- **Check for SUID binaries**:
    
    ```bash
    find / -perm -4000 2>/dev/null
    
    ```
    
- **Look for world-writable files**:
    
    ```bash
    find / -type f -perm -o+w 2>/dev/null
    
    ```
    
- **Enumerate OS and kernel version**:
    
    ```bash
    uname -a
    cat /etc/os-release
    
    ```
    

## Pro Tips

- **Run tools as root when needed**:
    
    ```bash
    sudo su
    
    ```
    
- **Use VirtualBox Snapshots**: Create snapshots before testing exploits to revert changes if needed.
- **Maintain a notes file**: Record findings during penetration tests for better organization and reporting.

---
