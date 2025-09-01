---

# **✴️ CIA Triad – Foundation of Information Security**

The **CIA Triad** consists of **Confidentiality**, **Integrity**, and **Availability** – the three pillars of securing information.

---

## **1. Confidentiality**

**Goal:** Prevent unauthorized access to sensitive data.

**Key Strategies:**

- **Encryption** (e.g., **AES**, **TLS**)
- **Authentication & Access Controls**
    
    (e.g., Passwords, **Biometrics**, **RBAC**)
    
- **Network Segmentation**
- **Data Classification**

**Examples:**

- Encrypting customer data in databases.
- Using **Multi-Factor Authentication (MFA)**.

**Symbol:** **🔐**

---

## **2. Integrity**

**Goal:** Ensure data is **accurate**, **consistent**, and **trustworthy**.

**Key Strategies:**

- **Hashing** (e.g., **SHA-256**)
- **Digital Signatures**
- **Audit Trails & Version Control**
- **Input Validation**

**Examples:**

- Using hash checks to verify file integrity.
- Logging changes in a system.

**Symbol:** **✅**

---

## **3. Availability**

**Goal:** Ensure systems are accessible when needed by authorized users.

**Key Strategies:**

- **Failover & Redundancy**
- **DDoS Protection**
- **Regular Backups**
- **Monitoring & Alerts**

**Examples:**

- Hosting services on scalable cloud platforms.
- Performing **disaster recovery drills**.

**Symbol:** **☁️⚙️**

---

# **⚖️ CVSS – Common Vulnerability Scoring System**

**Purpose:** Standardized method to **rate vulnerability severity**.

### **Key Components:**

- **Base Score:** Severity based on inherent characteristics
    
    *(e.g., exploitability, impact)*
    
- **Temporal Score:** Time-dependent factors
    
    *(e.g., availability of exploit)*
    
- **Environmental Score:** Custom to organization’s setup

**Useful Tools:**

- [CVSS v3.1 Calculator](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)
- [CVSS v4.0 Calculator](https://www.first.org/cvss/calculator/4.0)

**Symbol:** **📊**

---

## **Example Use:**

> A CVSS score of 9.8–10.0 = CRITICAL
> 
> 
> Requires **urgent patching** to protect **Confidentiality, Integrity, and Availability**.
> 

---

# **🛑 Major CVEs and Their Impact**

| **CVE** | **Description** | **CVSS Score** | **Impact** |
| --- | --- | --- | --- |
| **CVE-2021-44228***(Log4Shell)* | Remote code execution via JNDI in Log4j | **10.0 (Critical)** | Full system compromise |
| **CVE-2017-0144***(EternalBlue)* | SMBv1 RCE exploited by **WannaCry** | **9.8 (Critical)** | Mass ransomware infection |
| **CVE-2021-41773***(Apache Path Traversal)* | File disclosure & RCE via URL paths | **9.8 (Critical)** | Access to sensitive server files |
| **CVE-2022-0847***(Dirty Pipe)* | Local Privilege Escalation in Linux | **7.8 (High)** | Bypass of read-only protections |
| **CVE-2010-0708***(BlueKeep)* | RCE via Remote Desktop Protocol | **9.8 (Critical)** | Unauthenticated remote system control |

---

Let me know if you'd like this formatted into a **PDF**, **infographic**, or **flashcards**!

Here's a **clean, clear, and professional** version of your input, organizing it into the **CIA Triad**: Confidentiality, Integrity, and Availability — with examples and explanations:

---

### 🔐 **Confidentiality**

> Definition: Ensuring that information is not accessed by unauthorized users.
> 

✅ **Example:**

Protecting private data (like login credentials, financial details) from being accessed by hackers or unauthorized personnel.

🔒 **Goal:**

Prevent **data leakage**, **eavesdropping**, or **unauthorized access** to sensitive information.

---

### 🧩 **Integrity**

> Definition: Ensuring that data is accurate and unchanged from its original form.
> 

✅ **Example:**

If someone intercepts and **modifies a chat message** on Instagram during transmission, it's a violation of integrity.

🚫 **Threat:**

Unauthorized manipulation of data — e.g., changing a sender's message without permission.

---

### 🌐 **Availability**

> Definition: Ensuring that authorized users have access to systems and data when needed.
> 

✅ **Example:**

If a hacker finds a **vulnerability**, exploits it, and causes a **Denial-of-Service (DoS)** attack, it affects system availability.

⚠️ **Key Concepts:**

- **Vulnerability:** Weakness in the system (rated using **CVSS – Common Vulnerability Scoring System**).
- **Exploit:** Using that weakness to attack.
- **NAP (Network Access Protection):** Helps enforce health policies and secure access.
- **DirectAccess:** Ensures remote users can access resources securely, maintaining availability.

---

If you'd like this in a more colorful and visual note format (with diagrams or icons), I can help create that too!
