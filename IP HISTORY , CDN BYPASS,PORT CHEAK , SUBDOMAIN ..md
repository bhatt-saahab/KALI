---

# 🌐 **IP Address History: Concept & Use**

### 🔍 **What is IP Address History?**

Records of past IP addresses a domain has pointed to (A records) over time.

### 🧰 **Main Tool:**

🔗 **ViewDNS IP History**

Website: [viewdns.info/iphistory](https://viewdns.info/iphistory)

---

## 🧪 **Example Lookup:**

**Domain:** `armourinfosec.com`

| IP Address | First Seen | Last Seen |
| --- | --- | --- |
| 103.21.59.29 | 2023-05-12 | 2024-11-20 |
| 162.241.123.20 | 2022-01-05 | 2023-05-11 |
| 192.185.129.4 | 2020-08-18 | 2022-01-04 |

> 📅 These dates show when ViewDNS first/last saw the domain resolving to each IP.
> 

---

## ✅ **Use Cases of IP Address History**

🔹 Investigate previous hosting providers

🔹 Detect domain hijacking or redirection

🔹 Security research (attribution, timeline)

🔹 Recon in Penetration Testing & OSINT

---

# 🛠️ **More Tools for IP History**

| Tool | Purpose |
| --- | --- |
| 🛡️ SecurityTrails | Full DNS, IP, Domain history |
| 🧭 DNSDumpster | Subdomains & Passive DNS |
| 🗃️ ViewDNS Whois | WHOIS history timeline |
| 📊 RiskIQ | IP resolution + asset timeline |

---

# 🚪 **Common CDN Bypass Techniques**

### 🎯 Goal: Find real origin IP behind CDN

### 1. 🔄 **Historical DNS (Passive DNS)**

- Tools:
    
    🔹 **SecurityTrails**
    
    🔹 **ViewDNS IP History**
    
    🔹 **RiskIQ PassiveTotal**
    

### 2. 🧩 **Subdomain Enumeration**

- Command: `amass enum -d target.com`
- Some subdomains may not use CDN

### 3. 🔎 **Shodan / Censys**

- Search SSL cert or favicon hash:

```
ssl.cert.subject.CN:"target.com"
http.favicon.hash:123456789

```

### 4. 📧 **Email/Webmail Headers**

- Leaked headers may reveal true IP

### 5. ⚠️ **Misconfigured DNS**

- e.g., `ftp.target.com` points to origin server directly

### 6. 📜 **SSL Cert Transparency Logs**

- Tools:
    
    🔍 `crt.sh`
    
    🛡️ `Certspotter`
    

---

## 🧪 **Testing Discovered IPs**

Use:

```
curl -H "Host: target.com" http://<origin_ip>

```

✔️ If page loads correctly, you found the origin!

---

## 🔥 **Check for Firewall Misconfigurations**

- Server accepting traffic outside CDN IPs
- Use `wafw00f http://target.com` to detect WAF

---

## 📁 **Leaked Files**

- Look for `.git`, `.env`, etc., on subdomains

---

## 🔐 **Blue Team Defense Tips**

🛡️ Restrict origin to only allow CDN IP ranges

🧱 Setup strong firewall rules

🔍 Monitor DNS leaks

🔁 Regular scans for exposed ports/services

---

# 🔁 **Reverse IP Domain Lookup**

🔄 **What:** Find all domains hosted on the same IP

### 📍 **Use Cases**

- Discover related/staging/internal domains
- See shared hosting or weak isolation
- Expand target surface in bug bounty

### 🛠️ Tools:

🔗 **YouGetSignal** — free & easy

🛡️ **Security Trails** — DNS/IP associations + subdomains

---

# ✉️ **Reverse Mail Server Lookup**

🔄 Find all domains using the same mail (MX) server

### 📍 Use Cases:

- Multi-tenant shared email infra
- Mapping orgs using same mail host
- Exposure due to mail misconfig

### 🧪 How To:

```bash
dig armourinfosec.com mx
nslookup -type=mx armourinfosec.com

```

🔗 **ViewDNS Reverse MX Lookup**

---

# 📡 **Open Ports Check**

🚪 Test if ports (like 22, 80, 443) are open to the internet

### 📍 Use Cases:

- Check firewall/NAT rules
- Discover exposed services
- Verify port forwarding setup

### 🛠️ Online Tools:

- 🔍 **ViewDNS Port Scan**
- ⚡ **YouGetSignal Open Port Tool**
- 🌍 **DNSChecker Port Scanner**

---

💡 **Quick Tip:** Use these techniques/tools in **red teaming, recon, security audits, and competitive bug bounty** research.

---

Would you like these notes as a PDF, PNG poster, or Anki flashcards?
