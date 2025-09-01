

- **Notes**: Set up alerts for company names, domains, or key personnel.
- **Tool**: https://www.google.com/alerts
- **Objective**: Monitor real-time mentions of the target.

## 26. Google Alerts

---

- **Notes**: Look for posts revealing internal processes or events.
- **Tools**:
    - [https://tweetdeck.x.com](https://tweetdeck.x.com/)
    - [https://hootsuite.com](https://hootsuite.com/)
    - [https://www.social-searcher.com](https://www.social-searcher.com/)
- **Objective**: Analyze company and employee activity across social platforms.

## 25. Social Media Profiling

---

- **Notes**: Search for company-specific keywords or employee names.
- **Sources**:
    - Medium
    - Reddit (e.g., r/netsec, r/sysadmin)
    - Blogger, WordPress
- **Objective**: Identify articles or discussions authored by employees.

## 24. Blogs and Community Forums

---

- **Notes**: Look for posts mentioning specific tools or configurations.
- **Sources**:
    - [https://stackoverflow.com](https://stackoverflow.com/)
    - [https://quora.com](https://quora.com/)
    - [https://glassdoor.com](https://glassdoor.com/)
- **Objective**: Find employee discussions or questions leaking internal details.

## 23. Tech Support Forums

---

- **Notes**: Resumes may reveal outdated or sensitive information.
- **Search Queries**:
    
    ```bash
    site:linkedin.com/in "company name"
    filetype:pdf resume "company name"
    
    ```
    
- **Objective**: Extract details on tech usage, internal tools, emails, and infrastructure.

## 22. Resumes

---

- **Notes**: Job descriptions often reveal internal tools or infrastructure.
- **Sources**:
    - LinkedIn Jobs
    - Indeed
    - Glassdoor
    - Target’s careers page
- **Objective**: Gather details on tech stack, tools, locations, and departments.

## 21. Online Job Listings

---

- **Notes**: Search for employee posts or historical data leaks.
- **Tools**:
    - [https://groups.google.com](https://groups.google.com/)
    - Archived Yahoo Groups
- **Objective**: Find legacy discussions, leaks, or company-related messages.

## 20. Newsgroup Searches

---

- **Notes**: Ensure compliance with legal and ethical standards.
- **Tools**:
    - Mobile apps: TextMe, SpoofCard
    - Web: [http://www.crazycall.net](http://www.crazycall.net/)
- **Objective**: Make calls with a fake caller ID for social engineering (with permission).

## 19. Call Spoofing

---

- **Notes**: Use only in authorized testing scenarios to avoid legal issues.
- **Tools**:
    - [https://emkei.cz](https://emkei.cz/)
    - [https://anonymailer.net](https://anonymailer.net/)
- **Objective**: Test email security by sending spoofed emails (with permission).

### Spoofing Emails

- **Notes**: Weak configurations may indicate vulnerabilities to spoofing.
- **Tools**:
    - https://easydmarc.com/tools/dmarc-lookup
    - Command:
        
        ```bash
        dig TXT _dmarc.example.com
        
        ```
        
- **Objective**: Verify email server configurations.

### DMARC, SPF, DKIM Lookup

- **Notes**: Look for anomalies in headers that may indicate spoofing.
- **Tools**: Email client header analysis or online parsers.
- **Objective**: Trace sender details, IP addresses, and geolocation.

### Email Header Analysis

## 18. Email Tracking and Spoofing

---

- **Notes**: Use ethically and with permission, as this can be considered invasive.
- **Tools**:
    - [https://grabify.link](https://grabify.link/)
    - [https://iplogger.org](https://iplogger.org/)
- **Objective**: Track IP addresses by sending custom tracking URLs.

## 17. IP Logger

---

- **Notes**: Cross-reference with LinkedIn or other social media for accuracy.
- **Tools**:
    - [https://pipl.com](https://pipl.com/)
    - [https://peekyou.com](https://peekyou.com/)
    - [https://zabasearch.com](https://zabasearch.com/)
- **Objective**: Gather information on employees or stakeholders.

## 16. People Search Engines

---

- **Notes**: Use as a reference for additional tools and resources.
- **Tool**: [https://osintframework.com](https://osintframework.com/)
- **Objective**: Access a comprehensive collection of OSINT tools.

## 15. OSINT Framework

---

- **Notes**: Ensure compliance with ethical guidelines when using AI tools.
- **Use Cases**:
    - Parse log files for patterns.
    - Generate custom Google Dorks.
    - Predict email patterns (e.g., [firstname.lastname@company.com](mailto:firstname.lastname@company.com)).
- **Objective**: Use large language models to analyze and summarize reconnaissance data.

## 14. GPT-Powered Recon

---

- **Notes**: Search for company-specific keywords, email domains, or sensitive file names.
- **Additional Tools**: gitrob, truffleHog, Gitleaks, GitDorker
- **Tool**: https://github.com/search
- **Objective**: Find exposed code, credentials, or API keys in public repositories.

## 13. GitHub Recon

---

- **Notes**: Focus on specific ports or services relevant to the target.
- **Example**:
    
    ```bash
    shodan search org:"Target Organization" port:21
    
    ```
    
- **Tool**: [https://www.shodan.io](https://www.shodan.io/)
- **Objective**: Search for publicly exposed devices and services.

## 12. Shodan Reconnaissance

---

- **Notes**: Use specific queries to find exposed files, admin panels, or login pages.
- **Database**: https://www.exploit-db.com/google-hacking-database
- **Operators**:
    - Basic: `site:`, `inurl:`, `intitle:`, `filetype:`
    - Advanced: `cache:`, `link:`, `related:`, `inanchor:`
- **Objective**: Find sensitive data using advanced Google search operators.

## 11. Google Dorking (Google Hacking)

---

- **Notes**: Use `Pn` to skip host discovery if the target blocks ping. Be cautious to avoid detection.
- **Online Tool**: [https://pentest-tools.com](https://pentest-tools.com/)
- **Command**:
    
    ```bash
    nmap -Pn -sV -T4 example.com
    
    ```
    
- **Objective**: Identify open ports and running services.

## 10. Open Port Scanning

---

- **Notes**: Can reveal additional domains managed by the same organization.
- **Tool**: https://viewdns.info/reversens
- **Objective**: List domains using the same name server.

## 9. Reverse Name Server Lookup

---

- **Notes**: Helps uncover related infrastructure or email configurations.
- **Tool**: [https://mxtoolbox.com](https://mxtoolbox.com/)
- **Objective**: Identify domains using the same mail server.

## 8. Reverse Mail Server Lookup

---

- **Notes**: Useful for discovering related websites or shared hosting environments.
- **Tools**:
    - https://viewdns.info/reverseip
    - https://hackertarget.com/reverse-ip-lookup
- **Objective**: Identify all domains hosted on the same server/IP address.

## 7. Reverse IP Lookup

---

- **Notes**: Historical IPs may reveal old infrastructure still in use.
- **Tools**:
    - [https://securitytrails.com](https://securitytrails.com/)
    - [https://viewdns.info](https://viewdns.info/)
- **Objective**: View historical IP addresses associated with the domain.

## 6. IP Address History

---

- **Notes**: Identify intermediate routers, ISPs, or potential network bottlenecks.
- **Commands**:
    
    ```bash
    traceroute example.com  # Linux/macOS
    tracert example.com    # Windows
    
    ```
    
- **Objective**: Trace the network path to the target.

## 5. IP Traceroute

---

- **Notes**: Geolocation may not be precise but can indicate hosting providers or data center locations.
- **Tools**:
    - [https://ipinfo.io](https://ipinfo.io/)
    - [https://iplocation.net](https://iplocation.net/)
- **Objective**: Map the IP address to a physical location.

## 4. IP Geolocation

---

- **Notes**: Compare results across tools to ensure accuracy and identify load balancers or CDNs.
- **Commands**:
    
    ```bash
    nslookup example.com
    dig example.com
    
    ```
    
- **Objective**: Resolve the domain to its IP address.

## 3. Domain to IP Resolution

---

- **Notes**: Historical data may reveal changes in infrastructure or ownership.
- **Tool**: [https://whois-history.whoisxmlapi.com](https://whois-history.whoisxmlapi.com/)
- **Objective**: Track past ownership or registrar changes.

### WHOIS History

- **Notes**: Useful for identifying related domains or subsidiaries.
- **Tool**: https://viewdns.info/reversewhois
- **Objective**: Find all domains registered with the same email or organization.

### Reverse WHOIS

- **Notes**: Check for privacy protection services that may obscure registrant details.
- **Command**:
    
    ```bash
    whois example.com
    
    ```
    
- **Objective**: Gather domain registrant details, creation/expiry dates, registrar, and nameservers.

### Basic WHOIS Lookup

## 2. WHOIS Lookup

---

- **Notes**: Knowing the tech stack helps identify vulnerabilities specific to frameworks or CMS versions.
- **Tools**:
    - [https://builtwith.com](https://builtwith.com/)
    - [https://wappalyzer.com](https://wappalyzer.com/)
    - Browser extensions: Wappalyzer, BuiltWith
- **Objective**: Determine backend technologies, CMS, frameworks, and JavaScript libraries.

### Identify Website Technologies

- **Notes**: Look for outdated pages, exposed credentials, or removed features that may still be relevant.
- **Tool**: https://web.archive.org/web/*/example.com
- **Objective**: Access historical versions of the website or deleted content.

### Wayback Machine (Archive.org)

- **Notes**:
    - Analyze source code for comments, hardcoded credentials, or links to development/test environments.
    - Be cautious with aggressive crawling to avoid triggering security mechanisms.
- **Command for Local Mirroring**:
    
    ```bash
    wget --mirror --convert-links --adjust-extension --page-requisites --no-parent https://example.com
    
    ```
    
- **Online Tool**: [https://www.xml-sitemaps.com](https://www.xml-sitemaps.com/)
- **Tools**:
    - Burp Suite (Professional/Community)
    - OWASP ZAP
    - HTTrack
    - Scrapy
    - wget
- **Objective**: Map the website’s structure using automated tools.

### Spidering and Crawling

- **Notes**: Disallowed directories in `robots.txt` may contain valuable content like admin panels or backups.
- **Command Example**:
    
    ```bash
    curl https://example.com/robots.txt
    
    ```
    
- **Files to Check**:
    - **sitemap.html**: Designed for human users; may reveal hidden or unlinked pages.
    - **sitemap.xml**: Used by search engines; lists all indexed pages.
    - **robots.txt**: Specifies directories disallowed for search engine crawlers, often pointing to sensitive areas.
- **Objective**: Analyze files that reveal website structure or sensitive information.

### Check Public Files

- **Notes**: Pay attention to hidden pages, outdated content, or embedded metadata (e.g., author names in PDFs).
- **Actions**:
    - Browse all public pages, including "About," "Contact," and "Careers" sections.
    - Identify employee details: names, job titles, contact information, and organizational structure hints.
    - Note location details: office addresses, facility names, or embedded maps.
    - Collect addresses: headquarters, branch offices, or PO boxes.
    - Gather phone numbers: helpdesk, sales, support, or direct extensions.
- **Objective**: Manually explore the target's public website to extract visible information.

### Visit the Target Website

## 1. Website Reconnaissance

---

This checklist outlines the key steps and tools for conducting reconnaissance during the footprinting phase of a security assessment. The goal is to gather as much publicly available information about the target as possible without direct interaction. Each section includes tools, techniques, and notes for effective execution.

# Reconnaissance Checklist (Footprinting Phase)
