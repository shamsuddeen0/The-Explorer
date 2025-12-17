
# 🗺️ Nmap Cheat Sheet (The Holy Grail)

**"If you only learn one tool in cybersecurity, make it Nmap."**

Nmap (Network Mapper) is essential for penetration testing, network inventory, and security assessments. It answers the questions: "What is alive?", "What ports are open?", and "What is running on those ports?"

---

## 📋 Table of Contents
- [Basic Syntax](#basic-syntax)
- [Target Specification](#target-specification)
- [Port Specification](#port-specification)
- [Scan Types (TCP/UDP)](#scan-types)
- [Host Discovery (Ping Sweeps)](#host-discovery)
- [Timing & Performance](#timing-options)
- [Evasion (Stealth Mode)](#evasion-techniques)
- [NSE (Scripting Engine)](#nmap-scripting-engine-nse)
- [Output Formats](#output-options)
- [Practical Workflows](#practical-examples)

---

## 🛠️ Basic Syntax
```bash
nmap [ScanType] [Options] {targets}
```

If no port range is specified, Nmap scans the 1,000 most popular ports.

### 🎯 Target Specification

How to tell Nmap what to scan.

```bash
# Single IP
nmap 192.168.1.1

# IP Range
nmap 192.168.1.1-254

# Subnet (CIDR)
nmap 192.168.1.0/24

# Multiple Targets (Space separated)
nmap 192.168.1.1 192.168.1.2 192.168.1.3

# From a File (Essential for large assessments)
nmap -iL targets.txt

# Exclude Targets
nmap 192.168.1.0/24 --exclude 192.168.1.1
nmap 192.168.1.0/24 --excludefile exclude.txt
```
### 🔢 Port Specification
```
Don't scan everything if you don't have to.

Option	Description	Example
-p <port>	Scan specific port	nmap -p 22 192.168.1.1
-p <range>	Scan port range	nmap -p 1-100 192.168.1.1
-p <list>	Scan port list	nmap -p 22,80,443 192.168.1.1
-p-	Scan ALL 65535 ports	nmap -p- 192.168.1.1
-F	Fast scan (Top 100 ports)	nmap -F 192.168.1.1
--top-ports <n>	Scan n most popular ports	nmap --top-ports 20 192.168.1.1
-r	Scan ports linearly (Random is default)	nmap -r 192.168.1.1
```
### 📡 Scan Types
```
How Nmap talks to the target.

Option	Scan Type	Description	Root Required?
-sS	SYN Scan	The Standard. Stealthy, half-open connection.	Yes
-sT	TCP Connect	The "Noisy" one. Completes the handshake.	No
-sU	UDP Scan	Scans UDP ports (DNS, SNMP). Very slow.	Yes
-sV	Version Detect	Interrogates ports to find software versions.	No
-O	OS Detect	Guesses Operating System based on packet signatures.	Yes
-A	Aggressive	Enables OS Detect, Version Detect, Script Scanning, and Traceroute.	Yes
Advanced Scan Flags
Option	Type	Use Case
-sA	ACK Scan	Maps firewall rulesets (Stateful vs Stateless).
-sN	NULL Scan	No flags set (Stealth/Evasion).
-sF	FIN Scan	Only FIN flag set (Evasion).
-sX	Xmas Scan	FIN, PSH, and URG flags (Lit up like a tree).
-sI	Idle Scan	Uses a "Zombie" host to scan for you (Ultra Stealth).
```
### 🔎 Host Discovery (Ping)

Finding live hosts before scanning ports.
```
Option	Description
-sn	Ping Scan. Disables port scan. Just finds live IPs.
-Pn	Treat all hosts as online. Skips discovery. (Crucial for firewalls).
-PS <ports>	TCP SYN Ping (Default is usually 80,443).
-PA <ports>	TCP ACK Ping.
-PU <ports>	UDP Ping.
-PR	ARP Ping (Fastest for local network).
--traceroute	Trace path to host.
```
### ⏱️ Timing & Performance

Speed vs. Stealth.
```
Template	Name	Speed	Use Case
-T0	Paranoid	5 min/probe	IDS Evasion (Painfully slow)
-T1	Sneaky	15 sec/probe	IDS Evasion
-T2	Polite	0.4 sec/probe	Low bandwidth / Polite
-T3	Normal	Default	Standard scan
-T4	Aggressive	Fast	My Default. Good for CTFs/Labs.
-T5	Insane	Very Fast	Can miss ports / crash targets.
Fine-Tuning
```
```bash
# Scan really fast
nmap --min-rate 1000 --max-retries 2 192.168.1.1

# Slow down to avoid detection
nmap --scan-delay 1s 192.168.1.1
```
### 🥷 Evasion Techniques

Bypassing Firewalls and IDS.

Fragmentation

Splits packets into tiny chunks to confuse firewalls.

```bash
nmap -f 192.168.1.1      # 8 byte chunks
nmap --mtu 16 192.168.1.1 # Custom size
```
Decoy Scanning (The "Crowd")

Makes the scan look like it is coming from multiple IPs.

```bash
# RND:10 = Generate 10 random IPs
nmap -D RND:10 192.168.1.1
```
Spoofing
```bash 
# Spoof Source IP (Requires configuration)
nmap -S 192.168.1.100 192.168.1.1

# Spoof Source Port (Good for bypassing simple firewalls)
nmap --source-port 53 192.168.1.1
nmap -g 53 192.168.1.1
```
### 📜 Nmap Scripting Engine (NSE)

Nmap's superpower. Scripts are located in /usr/share/nmap/scripts/.

Basic Usage
```bash 
# Run default scripts (Safe and useful)
nmap -sC 192.168.1.1

# Run Vulnerability scanners
nmap --script vuln 192.168.1.1

# Run a specific category
nmap --script "safe" 192.168.1.1
```bash
Useful Scripts
Script Name	Function
http-title	Grabs the HTML title of the webpage.
http-robots.txt	Checks for robots.txt file.
ssl-cert	Grabs SSL certificate info.
smb-os-discovery	Finds Windows OS version via SMB.
dns-zone-transfer	Attempts to dump DNS records.
ftp-anon	Checks for Anonymous FTP login.
vuln	Runs a suite of vulnerability checks.
```
### 💾 Output Options
```bash
Always save your scans!

Option	Format	Description
-oN <file>	Normal	Text file. Human readable.
-oX <file>	XML	For importing into other tools (Metasploit, Searchsploit).
-oG <file>	Grepable	Easy to search with grep.
-oA <name>	All	Best Practice. Saves all 3 formats at once.
⚡ Practical Examples (My Workflow)
```
### 1. The "Quick Sweep" (Network Discovery)

Find all live hosts on the subnet.

```bash
nmap -sn 192.168.1.0/24 -oG live_hosts.txt
```
### 2. The "CTF Start" (Fast & Loud)

Scan all ports, identify versions, run scripts, save output.

```bash
nmap -p- -sC -sV -T4 -oA initial_scan 10.10.10.10
```
### 3. The "Stealthy Approach" (Red Team)

Syn scan, limited ports, slow timing, decoys.
```bash
sudo nmap -sS -p 80,443,22 -T2 -D RND:5 --randomize-hosts 192.168.1.0/24
```
### 4. Vulnerability Hunting

Target specific services (e.g., SMB) for known bugs.

```bash
nmap -p 445 --script smb-vuln-* 192.168.1.10
```
### ⚠️ Legal Notice

WARNING: Nmap is an active scanning tool. It generates traffic that is easily detected.

Only scan networks you own or have explicit written permission to test.

Unauthorized scanning is illegal in many jurisdictions.

