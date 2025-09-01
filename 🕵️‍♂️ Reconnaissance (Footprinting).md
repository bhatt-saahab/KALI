

# 🕵️‍♂️ **Reconnaissance (Footprinting)**

*“The art of gathering intelligence before launching an attack or penetration test.”*

---

## 🔍 **What is Reconnaissance?**

Reconnaissance is the **first phase** in ethical hacking or cyberattacks. The goal is to **collect as much information as possible** about the target **without detection**, to plan the attack or security assessment.

---

## 📂 **Types of Reconnaissance**

| Type | Description | Examples |
| --- | --- | --- |
| 🔹 **Passive Reconnaissance** | No direct interaction with target | WHOIS, Google, social media, DNS records |
| 🔸 **Active Reconnaissance** | Directly interacting with the target | Nmap scans, phishing, web probing |

---

## 🛠️ **Key Areas of Focus**

### 1️⃣ **Network Vulnerabilities**

- 🔓 **Open Ports**: Exposed services (e.g., SSH, FTP)
- ⚠️ **Misconfigured Devices**: Default settings on routers/firewalls
- 🕸️ **Weak Protocols**: Use of outdated (Telnet, FTP)
- 🚪 **Unauthorized Access**: No/weak authentication on systems

### 2️⃣ **Web Application Vulnerabilities**

- ⚔️ OWASP Top 10 issues like:
    - **SQL/Command Injections**
    - **Broken Authentication**
    - **XSS / CSRF**
    - **Insecure Deserialization**

### 3️⃣ **System/Server Vulnerabilities**

- 🔧 **Unpatched Software**
- 🗝️ **Default Credentials**
- ⬆️ **Privilege Escalation**
- 🗃️ **Insecure File Permissions**

---

## 🏢 **Organization Information Sources**

| Source | Information |
| --- | --- |
| 🌐 Company Website | General info, services, internal links |
| 🧑‍💼 Employee Profiles | From LinkedIn, public platforms |
| 🏢 Office Details | Locations, phone numbers |
| 💬 HTML Comments | “TODO”, developer hints |
| 📰 Press Releases | Recent updates/expansions |

---

## 🌐 **Network Information**

- 🔗 **Domain Names/Subdomains**: e.g., `example.com`, `intranet.example.com`
- 🧮 **IP Addresses**: Public/private ranges
- 📶 **TCP/UDP Services**: HTTP (80), SSH (22), etc.
- 🔐 **VPN/IDS/IPS**: Look for exposed configurations
- 📞 **VoIP Systems**: For social engineering

---

## 💻 **Operating System Info**

- 👥 **Users/Groups**: Naming conventions reveal roles
- 🧾 **Banner Grabbing**: Software/version info
- 🧭 **Routing Tables**
- 📡 **SNMP**: Network device details
- 🏗️ **System Architecture**: 32-bit or 64-bit
- 📛 **System Names**: e.g., `SQLServer`, `DC01`

---

## 🌍 **External Reconnaissance Techniques**

| Method | Description |
| --- | --- |
| 📞 Phone Numbers | Leaked or public numbers |
| 🧠 Google Hacking | `filetype:`, `inurl:`, `site:` queries |
| 🕷️ Website Mirroring | Using `wget`, `HTTrack` |
| 📦 GitHub Recon | Tools: Gitrob, truffleHog |
| 🗄️ Archive Sites | Archive.org snapshots |
| 📬 Email Headers | Reveal IPs, server paths |
| 🔎 WHOIS Lookup | Domain owner & registrar info |

---

## 🧪 **Internal Reconnaissance**

- 🖧 **Internal IPs/DNS Records**
- 🌐 **Private Websites**: Intranet portals
- 🗑️ **Dumpster Diving**: Old docs, USBs, sticky notes
- 👀 **Shoulder Surfing**: Watching users type passwords

---

# 🌐 **Website Reconnaissance**

### 🧠 Why Perform Website Recon?

- 🛡️ **Security Assessment**
- 📊 **Competitive Analysis**
- 🔎 **SEO/Marketing Insights**
- ✅ **Compliance Checks**

### 🔍 Typical Activities

- 🌐 **Identify IP, domain, SSL cert**
- ⚙️ **Tech Stack**: CMS, JS frameworks (via BuiltWith)
- 🔒 **Find exposed info**: emails, phone numbers
- 🕸️ **Spidering**: Map links & structure
- 🕰️ **Historical Snapshots**: Wayback Machine

### 🧰 Key Tools

| Tool Type | Examples |
| --- | --- |
| 🌍 Online Tools | Shodan, Censys, BuiltWith |
| 🧩 Crawlers | Screaming Frog, XML Sitemaps |
| 🧾 CLI Tools | wget, HTTrack |
| 🗃️ Archive | Archive.org, Archive.ph |

---

## 📂 **Website Files to Investigate**

| File | Purpose |
| --- | --- |
| `sitemap.html` | Human-readable navigation |
| `sitemap.xml` | For search engine indexing |
| `robots.txt` | Controls crawler access |

---

## 🧪 **Example Footprint Checklist**

1. ✅ Visit Website
2. 👥 Gather employee info
3. 🗺️ Check location/address
4. 📧 Find contact details
5. 🧾 Examine sitemap and robots.txt
6. 🧰 Use Shodan/Censys for tech info
7. 📚 Search Archive.org for old versions
8. 🔍 Google Dorking
9. 📦 Look on GitHub for exposed code
10. 📥 Analyze email headers if available

Would you like a printable PDF version or visual mind map of this too?

---

Here's your professionally formatted and **comprehensive notes** on `sitemap.html`, `sitemap.xml`, and `robots.txt`, including **real-life examples**, **tools**, and **differences in spidering vs crawling** — in a clear, attractive structure.

---

# 🌐 Sitemap, Robots.txt, and Crawling Explained

## 📁 1. **Sitemap Overview**

### 🗂️ `sitemap.html` – *User-Facing Index*

- 📌 A human-readable **HTML page**.
- Acts as a **"table of contents"** for website visitors.
- Helps users find key sections/pages easily.

**🧭 Real Examples**:

- [India TV Sitemap](https://www.indiatv.in/cms/sitemap.html)
- [MCA India Sitemap](https://www.mca.gov.in/MinistryV2/sitemap.html)
- [Telangana Transport Sitemap](https://www.transport.telangana.gov.in/html/sitemap.html)

---

### 🔗 `sitemap.xml` – *Search Engine Bot Index*

- 📌 A machine-readable **XML file**.
- Guides search engines to **index** specific pages.
- Useful for large or frequently updated websites.

**🎯 Key Purpose**:

- Helps bots (e.g., Googlebot) find all important URLs.

**🛠 Online Generator**:

- [XML Sitemap Generator](https://www.xml-sitemaps.com/)

---

### 🚫 `robots.txt` – *Crawler Restrictions*

- 📌 A plain text file placed at root (`example.com/robots.txt`)
- Tells **search engines what to avoid** indexing.

**Example Use Cases**:

- Block `/admin/` or private areas
- Avoid indexing duplicate content

**Syntax Example**:

```
User-agent: *
Disallow: /private/
Allow: /public/

```

---

## 🕷️ 2. **Spidering vs Crawling** – What’s the Difference?

| Concept | Description | Key Focus | Tools |
| --- | --- | --- | --- |
| **Spidering** | Following links recursively to **map site structure** | Link discovery & structure | Screaming Frog, Burp Suite |
| **Crawling** | Visiting pages, downloading data, and following links | Data collection & analysis | Googlebot, Katana, HTTrack |

🧠 In **cybersecurity recon**, both are used to discover content & vulnerabilities.

---

## 🧪 3. **Recon Tools for Sitemap & Crawling**

### 🔧 Common Tools:

| Tool | Purpose |
| --- | --- |
| 🕷️ Screaming Frog | Website spidering & SEO auditing |
| 🔄 ConvertCSV URL Extractor | Extract URLs from site data |
| 🧠 Katana | Recon spidering & vulnerability checking |
| 📥 Wget | Mirror websites and inspect HTML |
| 📡 HTTrack | Full site clone (offline browsing) |

---

### ⚙️ Example: **Wget Usage**

```bash
wget https://example.com

```

✔️ Downloads the page and resources locally.

---

### ⚙️ Example: **HTTrack Usage**

```bash
apt install httrack
httrack

```

✔️ Launches an interactive website mirroring session.

---

## 🕰️ 4. **View Historical Versions – Wayback Machine**

| Site | Use |
| --- | --- |
| 🌍 [Archive.org](https://archive.org/web/) | View past versions of websites |
| 📜 Orkut Archive | Legacy archives (if available) |
| 🖼️ Archive.ph | Snapshot of websites |

---

## ✅ Summary Table

| File | Purpose | Audience | Example |
| --- | --- | --- | --- |
| `sitemap.html` | Navigation | Human users | Visitors browsing site |
| `sitemap.xml` | Indexing | Search bots | Google indexing |
| `robots.txt` | Restrictions | Search bots | Block private pages |

---

Let me know if you want these notes exported as a **PDF**, **Markdown**, or ready for **GitHub formatting**!
