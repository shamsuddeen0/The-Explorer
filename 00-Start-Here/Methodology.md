# 📝 My Hacking Methodology

**"Process over Tools."**

This is my standard workflow when approaching a new target (CTF Box or Real Assessment). I follow this checklist to ensure I don't miss anything obvious before trying complex exploits.

---

## 🔁 1. The Cycle of Hacking
1.  **Reconnaissance:** Find the target.
2.  **Enumeration:** Find the doors (ports) and windows (services).
3.  **Exploitation:** Break in.
4.  **Privilege Escalation:** Become root/admin.
5.  **Post-Exploitation:** Loot and Persistence.

---

## 🕵️ Step 1: Reconnaissance (Passive)
*Before touching the target.*

- [ ] **OSINT:** Check Shodan, Google Dorks, and GitHub for leaks.
- [ ] **Tech Stack:** Use `BuiltWith` or `Wappalyzer` to see what they are running.
- [ ] **Subdomains:** Run `Subfinder` or `Amass` to find hidden development sites.

---

## 🔎 Step 2: Scanning & Enumeration (Active)
*Touching the target.*

### Port Scanning (The "Nmap" Phase)
- [ ] **Quick Scan:** `nmap -sC -sV -F <IP>` (Top 100 ports).
- [ ] **Full Scan:** `nmap -p- <IP>` (All ports).
- [ ] **UDP Scan:** `nmap -sU --top-ports 20 <IP>` (If stuck).

### Service Enumeration Checklist
*   **Port 21 (FTP):**
    - [ ] Check for `Anonymous` login.
*   **Port 22 (SSH):**
    - [ ] Check banner version (is it old?).
    - [ ] Do I have a key from another service?
*   **Port 80/443 (HTTP/Web):**
    - [ ] View Source Code (Ctrl+U).
    - [ ] Check `robots.txt`.
    - [ ] Run `Nikto` scan.
    - [ ] Run `Gobuster` / `FFUF` for directories.
*   **Port 139/445 (SMB):**
    - [ ] Run `enum4linux -a <IP>`.
    - [ ] Check for null sessions (`smbmap -u "null"`).

---

## 💥 Step 3: Exploitation (Gaining Access)
*Finding the crack in the armor.*

- [ ] **Searchsploit:** Search for the version numbers found in Step 2.
    - `searchsploit apache 2.4.49`
- [ ] **Metasploit:** we will talk about it later 
- [ ] **Default Creds:** Try `admin:admin`, `root:root`, `admin:password`.
- [ ] **Web Exploits:**
    - [ ] SQL Injection (Login forms, URL parameters).
    - [ ] RCE (Remote Code Execution) in upload forms.

---

## 🪜 Step 4: Privilege Escalation (PrivEsc)
*I'm in, but I'm just a "user". I want "root".*

### Linux Checklist
- [ ] **Sudo Rights:** Run `sudo -l` (Can I run anything as root?).
- [ ] **SUID Binaries:** `find / -perm -u=s -type f 2>/dev/null` (Check GTFOBins).
- [ ] **Cron Jobs:** Are there scripts running automatically that I can edit?
- [ ] **Automated Tool:** Run `LinPEAS.sh`.

### Windows Checklist
- [ ] **User Privileges:** `whoami /priv`.
- [ ] **Services:** Unquoted Service Paths?
- [ ] **Automated Tool:** Run `WinPEAS.exe`.

---

## 🏴‍☠️ Step 5: Post-Exploitation
*The house is mine. Now what?*

- [ ] **Loot:** Look for `flag.txt`, `passwords.txt`, or database configs.
- [ ] **Dump Hashes:** `/etc/shadow` (Linux) or SAM/SYSTEM (Windows).
- [ ] **Persistence:** Add a user or SSH key so I can get back in later.
- [ ] **Cleaning Up:** (For real engagements) Remove the scripts/files I uploaded.

---

## 🧠 When I get stuck...
1.  **Re-Scan:** Did I miss a UDP port?
2.  **Re-Read:** Did I miss a detail in the web source code?
3.  **Google:** "Apache 2.4.49 exploit github".
4.  **Reset:** Take a walk. The answer is usually simpler than I think.