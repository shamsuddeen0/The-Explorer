This file is all about mapping the Infrastructure. It focuses on finding subdomains (the most common place to find vulnerabilities) and cloud assets.

# 🗺️ Asset Discovery & Subdomain Enumeration

**"You can't hack what you don't see."**

Modern companies have thousands of subdomains (`dev.target.com`, `admin.target.com`). The main website is usually secure. The forgotten developer portal is usually not.

---

## 🌐 Subdomain Enumeration Tools

### 1. **Subfinder** (Fast)
My go-to tool for a quick list. It queries passive sources (VirusTotal, Censys).
```bash
subfinder -d target.com -o subdomains.txt
2. Amass (Comprehensive)

The heavy hitter. It does brute forcing, scraping, and recursive searching. It takes longer but finds more.

```bash 
amass enum -d target.com
```
3. crt.sh (Certificate Transparency)

A manual check. Every SSL certificate is logged here.

Go to `https://crt.sh/?q=target.com` to see every subdomain that ever requested an SSL cert.

## ☁️ Cloud Asset Discovery

Finding S3 buckets and Azure blobs.

S3Scanner / CloudEnum

Checks if the company has open AWS S3 buckets (which often contain backups or source code).

```bash 
# CloudEnum example
pip3 install cloud_enum
python3 cloud_enum.py -k target_keyword
```
## 🕸️ Content Discovery (Fuzzing)

Once you have a subdomain, you need to find the hidden folders.

FFUF (Fuzz Faster U Fool)

The industry standard for directory busting.

```bash 
ffuf -u https://target.com/FUZZ -w /usr/share/wordlists/dirb/common.txt
```
### Gobuster

A classic alternative.
```bash 
gobuster dir -u https://target.com -w common.txt
```
## ⚙️ The "Tech Stack" Fingerprint

What are they running?

Wappalyzer: Browser extension that tells me the CMS (WordPress, Drupal), Web Server (Nginx, Apache), and OS.

BuiltWith: Shows the history of the technology stack.

WhatWeb: CLI tool.

```bash
whatweb target.com
```
