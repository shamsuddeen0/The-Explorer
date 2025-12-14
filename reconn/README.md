# 🕵️ Active & Passive Reconnaissance

**"If you can't find it, you can't hack it."**

This section documents the tools and techniques I use to map out a target. I have broken this down into **Passive** (gathering info without touching the target) and **Active** (sending packets to the target).

---

## ☁️ Passive Recon (OSINT)
*The goal here is to learn as much as possible without alerting the target.*

### 🔍 Search Engines & Dorks
*   **[Google Hacking Database (GHDB)](https://www.exploit-db.com/google-hacking-database)**: A collection of search queries to find exposed files, login portals, and errors.
*   **[Shodan](https://www.shodan.io/)**: The "Google" for IoT devices, servers, and webcams.
*   **[Censys](https://censys.io)**: Similar to Shodan, excellent for finding certificates and hosts.
*   **[Wayback Machine](https://archive.org/web/)**: I use this to find old versions of a site, which might contain removed comments or old backup files.

### 🌐 DNS & Subdomains
*   **[crt.sh](https://crt.sh/)**: Search for SSL/TLS certificates to find subdomains that aren't even active yet.
*   **[DNSDumpster](https://dnsdumpster.com/)**: Visualizes domain mapping and finds related hosts.
*   **[Amass](https://github.com/owasp-amass/amass)**: The industry standard for deep DNS enumeration.
*   **[Sublist3r](https://github.com/aboul3la/Sublist3r)**: A fast python tool to enumerate subdomains across multiple search engines.

### 👤 People & Social Media
*   **[Sherlock](https://github.com/sherlock-project/sherlock)**: I use this to hunt down a username across hundreds of social media sites instantly.
*   **[theHarvester](https://github.com/laramies/theHarvester)**: Gathers emails, names, subdomains, IPs, and URLs from public sources.
*   **[LinkedIn](https://www.linkedin.com)**: Essential for understanding the "Tech Stack" a company uses based on their job postings.

### 📋 Infrastructure & Whois
*   **[Whois.domaintools.com](https://whois.domaintools.com/)**: Good for historical ownership data.
*   **[BGP Looking Glasses](https://bgp.he.net/)**: Hurricane Electric's tool to find IP ranges (ASN) owned by an organization.

---

## ⚡ Active Recon
*⚠️ **Warning:** These tools generate traffic and logs. Only use these on targets you have permission to test.*

### 🗺️ Network Scanning
*   **[Nmap](https://nmap.org/)**: The most important tool in this list.
    *   *My standard command:* `nmap -sC -sV -oA scan_results <IP>`
    *   *Reference:* See my [Nmap Cheat Sheet](../cheat-sheets/nmap.md).
*   **[RustScan](https://github.com/RustScan/RustScan)**: I use this when I need to scan ports extremely fast (faster than Nmap).

### 🕸️ Web Enumeration
*   **[Burp Suite](https://portswigger.net/burp)**: The #1 tool for web hacking. I use the Community Edition for manual testing.
*   **[FFUF](https://github.com/ffuf/ffuf)**: "Fuzz Faster U Fool" - My go-to tool for brute-forcing directories and files.
*   **[Nikto](https://github.com/sullo/nikto)**: Scans for outdated server software and misconfigurations.
*   **[Wappalyzer](https://www.wappalyzer.com/)**: A browser extension that tells me what technologies a website is built with (CMS, Frameworks, etc).

### ☢️ Vulnerability Scanning
*   **[Nuclei](https://github.com/projectdiscovery/nuclei)**: A modern, template-based scanner. Highly effective for bug bounties.
*   **[OWASP ZAP](https://www.zaproxy.org)**: A great open-source alternative to Burp Suite.