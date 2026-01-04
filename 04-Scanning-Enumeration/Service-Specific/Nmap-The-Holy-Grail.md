
# 🗺️ Nmap Cheat Sheet

**"If you only learn one tool, make it Nmap."**

Nmap (Network Mapper) answers: "What ports are open?" and "What is running on them?"

---

## ⚡ My "Go-To" Scans

### 1. The Quick Scan (Is it alive?)
```bash
nmap -sn 192.168.1.0/24
```
### 2. The Standard Scan (Top 1000 Ports)
```bash 
nmap -sC -sV -oA initial_scan <IP>
# -sC: Default Scripts
# -sV: Version Detection
# -oA: Save Output in all formats
```
### 3. The "All Ports" Scan (Thorough)
```bash
nmap -p- <IP>
```
### 4. The UDP Scan (The "Hidden" Ports)
```bash
nmap -sU --top-ports 20 <IP>
```
## 🎯 Scan Types Explained
Flag	Name	Description
`-sS`	SYN Scan	Stealthy. Does not complete the connection. (Root only).
`-sT`	Connect	Noisy. Completes the full handshake.
`-A`	Aggressive	Enables OS detection, Version detection, Script scanning, and Traceroute.
`-O`	OS Detect	Guesses the Operating System.
## 🕵️ Evasion & Performance

`-T4`: Speed up the scan (Recommended).

`-T2`: Slow down (Polite / Evasion).

`-D` RND:10: Decoys. Spoof 10 random IPs to hide yours.

`-Pn`: Treat host as online (Skip Ping). Useful for Windows firewalls.

## 📜 Nmap Scripting Engine (NSE)

Nmap can do vulnerability scanning.

```bash 
# Scan for known vulnerabilities
nmap --script vuln <IP>

# Scan for SMB vulnerabilities (EternalBlue)
nmap -p 445 --script smb-vuln* <IP>
```