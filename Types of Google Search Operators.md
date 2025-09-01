---

## 📌 **1. Google Search Basic Operators**

1. **Force Inclusion Operator ( + )**
2. **Exclude Term Operator ( - )**
3. **Suggest Similar Terms Operator (`~`)**
4. **Search Within a Range Operator (`..`)**
5. **Wildcard Operator  ( * )**
6. **Exact Match Operator (`" "`)**
7. **Logical OR Operator (`OR`)**

---

---

# 🔍 **Google Search Advanced Operators**

Google’s advanced search operators refine queries with high precision, widely used in:

✅ **Cybersecurity**

✅ **OSINT (Open Source Intelligence)**

✅ **Bug Bounty Hunting**

✅ **Digital Forensics**

---

## 📘 **Search Rules & Syntax**

| Rule | Description | Example |
| --- | --- | --- |
| 🔸 No Space Between Operator and Term | Operators must be directly attached to the keyword. | ✅ `intitle:armourinfosec`❌ `intitle: armourinfosec` |
| 🔸 Case Insensitive | Google is not case-sensitive. | `intitle:CyberSecurity` = `intitle:cybersecurity` |
| 🔸 Multi-word Terms | Use double quotes `" "` or dots `.` for phrases. | `intitle:"Ethical Hacking"inurl:ethical.hacking.tutorial` |
| 🔸 Combine Operators | You can mix multiple operators for powerful queries. | `site:github.com intitle:"README" intext:"api_key"` |

---

## 🧠 **Key Advanced Operators**

| Operator | Function | Example |
| --- | --- | --- |
| `intitle:` | Word(s) in the page **title** | `intitle:"cybersecurity training"` |
| `allintitle:` | **All** words in the title | `allintitle:"cybersecurity training"` |
| `inurl:` | Word(s) in the **URL** | `inurl:cybersecurity-training` |
| `allinurl:` | **All** terms in the URL | `allinurl:cybersecurity training` |
| `intext:` | Word(s) in the **body text** | `intext:"penetration testing"` |
| `filetype:` | Search specific **file types** | `filetype:pdf "cybersecurity training"` |
| `ext:` | Alternative to filetype (for extensions) | `ext:ppt "cybersecurity training"` |
| `site:` | Restrict results to a **domain/site** | `site:armourinfosec.com` |
| `inanchor:` | Text used in a **hyperlink (anchor)** | `inanchor:armourinfosec` |
| `link:` | Pages **linking to** a site (limited now) | `link:armourinfosec.com` |
| `cache:` | View **cached version** by Google | `cache:armourinfosec.com` |
| `related:` | Find **similar sites** | `related:google.com` |
| `info:` | Info about a URL | `info:armourinfosec.com` |

---

## 📚 **Popular Google Dork Patterns**

| 🔍 **Purpose** | 🧪 **Example Dork** |
| --- | --- |
| Exposed Login Pages | `inurl:admin login` |
| Directory Listing | `"index of /" site:example.com` |
| Configuration Files | `ext:env` |
| Database Dumps | `filetype:sql "insert into" -github` |
| Logs with Passwords | `filetype:log intext:password` |
| Exposed Cameras | `inurl:view/view.shtml` |
| Search Within Site | `site:example.com confidential` |
| Find PDFs | `filetype:pdf "internal use only"` |
| Git Repositories | `inurl:.git github` |
| Backup Files | `ext:bak` |

---

## 🧪 **Example Use Cases (Scenarios)**

### 🔹 1. Find Webcams

`inurl:view/view.shtml`

### 🔹 2. Discover Exposed .env Files

`filetype:env intext:DB_PASSWORD`

### 🔹 3. Locate Admin or Login Pages

`inurl:admin | inurl:login | intitle:"admin panel"`

### 🔹 4. Search PDF Docs with Confidential Data

`filetype:pdf "confidential" site:example.com`

---
