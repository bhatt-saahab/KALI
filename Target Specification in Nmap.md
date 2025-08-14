---
🎯 Target Specification in Nmap
Nmap offers flexible ways to **specify target hosts or networks** for scanning. You can target **single hosts, multiple hosts, IP ranges, subnets, files of targets**, or even **random hosts**, and exclude specific IPs as needed.

---

## 1️⃣ **Single & Multiple Hosts**

You can specify **one or more hosts** (IP addresses or domain names) separated by spaces.

**Examples:**

```bash
nmap armourinfosec.com thehackersworld.com
nmap 192.168.1.35 192.168.1.1
nmap 192.168.1.31 192.168.1.1 192.168.1.247

```

---

## 2️⃣ **Using IP Ranges & Subnets**

### 📌 **CIDR Notation** (Scan entire subnet)

```bash
nmap 192.168.1.0/24
nmap 192.168.56.1/24

```

### 📌 **Hyphen-Separated Ranges**

```bash
nmap 192.168.1.1-254
nmap 192.168.1.1-51,250-254
nmap -v 192.168.1.1-51,250-254
nmap 192.168.1.1-10,50-60,100,200,240-254

```

### 📌 **Scanning Across Octets**

```bash
nmap -v 192.168.1,2.1-100
nmap 192.168.1-3.1/24
nmap 192.168.1,2.1/24

```

### 📌 **Multiple Specific IPs (Comma-Separated)**

```bash
nmap 192.168.1.1,5,27,100,207
nmap 192.168.1.1,10,50,60,100,200,244,231,31,61

```

### 📌 **Restricted Range in a Subnet**

```bash
nmap 192.168.1.200-254

```

---

## 3️⃣ **Using Input File for Target List**

Store target IPs or hostnames in a file (one per line).

**Example file (`ip.txt`):**

```
192.168.1.1
192.168.1.10
192.168.1.31
192.168.1.61
192.168.1.100
192.168.1.101
192.168.1.102
192.168.1.103
192.168.1.104

```

**Scan from file:**

```bash
nmap -iL ip.txt

```

---

## 4️⃣ **Random Hosts Scanning**

Nmap can scan random IPs from the internet.

**Example:**

```bash
nmap -v -iR 10   # Scans 10 random hosts

```

---

## 5️⃣ **Excluding Hosts from Scans**

### 📌 **Exclude with `-exclude`**

```bash
nmap -v 192.168.1.1/24 --exclude 192.168.1.1-10
nmap -v 192.168.1.1/24 --exclude 192.168.1.1-25
nmap -v 192.168.1.1/24 --exclude 192.168.1.52-100,250

```

### 📌 **Exclude from File**

```bash
nmap -v 192.168.1.1/24 --excludefile ip.txt

```

---

## 6️⃣ **Generating IP Lists with `prips`**

`prips` is a utility to generate sequential IP addresses.

**Install:**

```bash
apt install prips

```

**Examples:**

```bash
prips 192.168.1.0/24
prips 192.168.1.0/28
prips 192.168.1.0/26 | grep 192.168.1.231

```

---

## 📌 **Quick Summary Table**

| **Method** | **Example Command** | **Purpose** |
| --- | --- | --- |
| Single host | `nmap 192.168.1.10` | Scan one IP/domain |
| Multiple hosts | `nmap host1.com host2.com` | Scan several targets |
| Subnet scan (CIDR) | `nmap 192.168.0.0/24` | Scan all in subnet |
| IP range | `nmap 192.168.1.1-50` | Scan consecutive IPs |
| Multiple ranges | `nmap 192.168.1.1-5,50-60,100` | Scan selected ranges |
| Across octets | `nmap 192.168.1,2.1-100` | Multi-octet scan |
| Input file | `nmap -iL ip.txt` | Scan list from file |
| Random scan | `nmap -iR 10` | Scan random hosts |
| Exclude IPs | `nmap --exclude 192.168.1.1-5` | Skip specific IPs |
| Exclude from file | `nmap --excludefile skip.txt` | Skip list from file |
| Generate list with prips | `prips 192.168.1.0/24` | Create full IP list |

---
