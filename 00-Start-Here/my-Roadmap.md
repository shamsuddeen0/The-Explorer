 
# 🗺️ My Cyber Security Roadmap

**"A goal without a plan is just a wish."**

This document tracks my journey from "I don't know what I'm doing" to "Professional Security Engineer." It is broken down into phases. If you are following along, do not skip Phase 1. You cannot hack what you do not understand.

---

## 📍 Current Status
*   **Current Phase:** Phase 2 (Scripting & Basics)
*   **Current Focus:** Learning Python and automating Nmap scans.
*   **Reading:** "The Art of Hacking"
*   **Next Cert:** CompTIA Security+

---

## 🏗️ Phase 1: The Foundations (The "Boring" Stuff)
*Goal: Understand how computers talk to each other before trying to break them.*

- [x] **Networking Basics**
    - [x] OSI Model & TCP/IP (The layers of the internet)
    - [x] IP Addressing & Subnetting (CIDR notation)
    - [x] Common Ports (DNS, HTTP, SSH, FTP, SMB)
    - [ ] Wireshark (Analyzing packet captures)
- [x] **Linux Essentials**
    - [x] Command Line Navigation (`cd`, `ls`, `grep`, `cat`)
    - [x] File Permissions (`chmod`, `chown`)
    - [x] User Management (`sudo`, `/etc/passwd`)
- [ ] **Windows Internals**
    - [ ] Registry & Services
    - [ ] PowerShell Basics

---

## 🐍 Phase 2: The Toolsmith (Scripting)
*Goal: Stop relying on other people's tools and start writing my own.*

- [ ] **Bash Scripting**
    - [ ] Automating Nmap scans
    - [ ] Log parsing with `grep` and `awk`
- [ ] **Python for Security**
    - [ ] Basic syntax (Variables, Loops, Functions)
    - [ ] Requests library (Interacting with Web APIs)
    - [ ] Socket library (Building a port scanner)

---

## ⚔️ Phase 3: Offensive Security (Red Team)
*Goal: Learn the methodology of a penetration test.*

- [ ] **Reconnaissance (OSINT)**
    - [ ] Passive Recon (Shodan, Google Dorks)
    - [ ] Active Recon (Nmap, Nikto, Burp Suite)
- [ ] **Web Application Hacking**
    - [ ] OWASP Top 10 (SQLi, XSS, IDOR)
    - [ ] Fuzzing (FFUF, Gobuster)
- [ ] **Exploitation**
    - [ ] Metasploit Framework
    - [ ] Reverse Shells & Listeners (Netcat)
    - [ ] Privilege Escalation (LinPEAS, WinPEAS)

---

## 🛡️ Phase 4: Defensive Security (Blue Team)
*Goal: Learn how to catch the attacks I learned in Phase 3.*

- [ ] **Security Controls**
    - [ ] Firewalls (UFW, Iptables)
    - [ ] IDS/IPS (Snort, Suricata)
- [ ] **SIEM & Logs**
    - [ ] Splunk / ELK Stack basics
    - [ ] Windows Event Logs (Detecting Mimikatz/PsExec)
- [ ] **Incident Response**
    - [ ] The PICERL process
    - [ ] Forensics (Volatility for RAM dumps)

---

## ☁️ Phase 5: Advanced & Enterprise (The Job Getter)
*Goal: Hacking modern corporate environments.*

- [ ] **Active Directory (The Beast)**
    - [ ] Kerberos & Authentication
    - [ ] Attacks: Kerberoasting, AS-REP Roasting, Golden Ticket
    - [ ] Tools: Bloodhound, Impacket, CrackMapExec
- [ ] **Cloud Security**
    - [ ] AWS/Azure Fundamentals
    - [ ] S3 Bucket Enumeration
    - [ ] Terraform & Infrastructure as Code (IaC)

---

## 🏆 Certification Tracker
*My verification of knowledge.*

| Certification | Organization | Status | Date |
| :--- | :--- | :--- | :--- |
| **Google Cyber Security** | Google | ✅ Done | Jan 2024 |
| **Security+** | CompTIA | 🚧 In Progress | TBD |
| **eJPTv2** | eLearnSecurity | 📅 Planned | -- |
| **Blue Team Level 1** | SecurityBlueTeam | 📅 Planned | -- |
| **OSCP** | OffSec | 🔮 Future Goal | -- |

---

## 📝 Learning Philosophy
1.  **Document Everything:** If I didn't write it down, I didn't learn it.
2.  **Labs > Theory:** Reading is good, but doing is better. Break the VMs.
3.  **Consistency:** 1 hour a day is better than 10 hours on Sunday.