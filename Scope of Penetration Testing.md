

**Definition:**

The **scope of a penetration test** defines the **boundaries, targets, and objectives** of the engagement. It ensures that testing is **effective**, **efficient**, and conducted within **legal and contractual limits**.

---

## **Key Elements of Scope**

### **1. Target Assets**

**Identify systems and components in scope:**

- **🌐 Web Applications**
- **🔗 Internal & External Networks**
- **📱 Mobile Apps (iOS, Android)**
- **⚙️ APIs (REST, SOAP, GraphQL)**
- **☁️ Cloud Environments (AWS, Azure, GCP)**
- **📡 IoT Devices**
- **📶 Wireless Networks**
- **🎭 Social Engineering** *(if explicitly permitted)*
- **🏢 Physical Security** *(e.g., data center access)*

---

### **2. Testing Boundaries**

Define exclusions to prevent disruption or legal issues:

- **🚫 Are production systems excluded?**
- **🚫 Are third-party systems not owned by the client excluded?**
- **⏰ Should testing be restricted to business hours only?**

---

### **3. Access Level**

**Specify tester’s access level:**

- **⬛ Black Box** – No access or credentials *(external attacker simulation)*
- **⬜ Gray Box** – Limited credentials/documentation *(insider simulation)*
- **◻️ White Box** – Full access *(system internals, code, documentation)*

---

### **4. Type of Testing**

Clarify the nature of testing involved:

- **🌍 External Testing** – Internet-facing systems (websites, VPNs)
- **🏠 Internal Testing** – Insider threats within the network
- **🛠️ Application Testing** – Business logic, session, validation, etc.
- **☁️ Cloud Security Assessment** – IAM, misconfigs, exposed storage
- **🎯 Red Team Engagement** – APT simulations, real-world attack emulation

---

### **5. Timeframe & Resources**

Include operational details:

- **📅 Start & End Dates**
- **🕒 Daily Testing Windows** *(to reduce disruption)*
- **👥 Number of Testers & Access Requirements**

---

### **6. Rules of Engagement**

Outline procedures for smooth execution:

- **⚠️ How will vulnerabilities be reported?**
- **☎️ Who is the escalation point-of-contact?**
- **❓ What is the process for unexpected findings?**

---

## **Example Scope Statement**

> "This penetration test will focus on the external web application https://portal.clientsite.com, including associated APIs and authentication mechanisms. Testing will be gray-box using a limited test account. Third-party services, internal systems, and production databases are out of scope. Testing will occur during business hours, with daily reports sent to the IT security team."
> 

---

## **Network Penetration Testing – IPs & Ranges**

### ✅ **Included**

**Specific IPs:**

- `192.31.242.107`
    
    **IP Ranges:**
    
- `192.31.242.0/24`
- `192.31.243.0/26`, `192.31.243.1` to `192.31.243.62`
- `192.31.243.64/28`
- `192.31.243.96/27`
- `IPv6: 2628:134:6000::/40`
    
    **Private/Internal Ranges:**
    
- `10.0.0.0/8`
- `172.16.0.0/12`
- `192.168.0.0/16`

### ❌ **Excluded IPs**

- `192.31.242.107, 108, 109, 111, 112` *(from `192.31.242.0/24`)*

---

## **Example Use Cases**

### **1. Internal Network Penetration Test**

> "Testing the internal corporate LAN at HQ using gray-box techniques with domain credentials. In scope: workstations, internal apps, file shares, AD infrastructure. Out of scope: production DBs, employee devices. Scheduled after business hours."
> 

---

### **2. Cloud Security Assessment**

> "Focusing on AWS: EC2, S3, IAM, Lambda, etc. With white-box access (read-only credentials). Excludes Azure, GCP. No exploitation or privilege escalation without consent."
> 

---

### **3. Web Application & API Test**

> "Covers https://app.clientdomain.com and https://api.clientdomain.com. Gray-box test using standard user credentials. Excludes: admin portals, payment gateways, and third-party auth. Conducted during business hours."
> 

---

### **4. Mobile App Security Testing**

> "Testing Android & iOS mobile banking app, including APIs and backend services. Uses jailbroken/rooted devices and client test accounts. Excludes production payment systems."
> 

---

### **5. Red Team Assessment**

> "Simulates APT against global infra. Includes social engineering, recon, lateral movement. Scope: public domains + internal systems post-compromise. SOC not informed except for one emergency POC."
> 

---

### **6. Physical Security Test**

> "Assesses physical security at San Jose data center. Attempts to bypass badges, guards, locks. Tests done during business hours using gray-box approach with basic facility maps. Excludes employee & non-IT areas."
> 

---

Let me know if you'd like a **printable PDF**, **infographic**, or if you want this added to your **Information Security notes collection**!
