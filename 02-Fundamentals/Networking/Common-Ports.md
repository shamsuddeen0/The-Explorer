# 🚪 Common Ports Cheat Sheet

**"Ports are the doors to the system. Knowing what they are tells you what's inside."**

When Nmap returns a list of open ports, I use this list to quickly identify the attack surface.

---

## 🌐 Web & File Transfer (The Most Common)

| Port | Service | Description | Attack Vector |
| :--- | :--- | :--- | :--- |
| **20/21** | FTP | File Transfer Protocol | Anonymous Login, Cleartext Creds |
| **22** | SSH | Secure Shell | Brute Force, Private Key Theft, Old Versions |
| **23** | Telnet | Unencrypted Shell | **Critical:** Sniffing passwords |
| **69** | TFTP | Trivial FTP (UDP) | No Auth - Read/Write any file |
| **80** | HTTP | Web Server | SQLi, XSS, RCE, Default Creds |
| **443** | HTTPS | Secure Web | Heartbleed, Poodle (Old SSL) |
| **445** | SMB | Windows Shares | **EternalBlue**, Null Session, PsExec |
| **8080** | HTTP-Proxy | Alt Web Port | Often Tomcat/Jenkins (Manager Consoles) |

---

## 📧 Email Services

| Port | Service | Description | Attack Vector |
| :--- | :--- | :--- | :--- |
| **25** | SMTP | Simple Mail Transfer | Open Relay (Spam), User Enumeration |
| **110** | POP3 | Post Office Protocol | Cleartext Auth |
| **143** | IMAP | Internet Message Access | Cleartext Auth |
| **465** | SMTPS | Secure SMTP | Weak Cipher Suites |
| **993** | IMAPS | Secure IMAP | Weak Cipher Suites |

---

## 🏗️ Infrastructure & Database (The Juicy Targets)

| Port | Service | Description | Attack Vector |
| :--- | :--- | :--- | :--- |
| **53** | DNS | Domain Name System | Zone Transfer (AXFR), DNS Cache Poisoning |
| **135** | RPC | Remote Procedure Call | User Enumeration |
| **139** | NetBIOS | Windows API | SMB Enumeration |
| **161** | SNMP | Network Management | Public Community String (Info Leak) |
| **389** | LDAP | Directory Access | Null Bind, User Enumeration |
| **1433** | MSSQL | Microsoft SQL Server | `sa` default password, XP_CmdShell |
| **3306** | MySQL | MySQL Database | `root` default password, SQLi |
| **3389** | RDP | Remote Desktop | **BlueKeep**, Brute Force |
| **5900** | VNC | Remote Access | No Auth, Weak Password |
| **5985** | WinRM | Windows Remote Mgmt | Evil-WinRM (Shell Access) |

---

## ☁️ Cloud & Modern Tech

| Port | Service | Description |
| :--- | :--- | :--- |
| **6379** | Redis | In-memory DB (Often unauthenticated RCE) |
| **27017** | MongoDB | NoSQL Database (Often no auth) |
| **9200** | Elasticsearch | Search Engine (Info Leak) |
| **2375** | Docker | Docker API (Root access if exposed!) |

---

## 🧠 What to do when you see a port?
1.  **Banner Grab:** `nc -v <IP> <PORT>` (See what version it is).
2.  **Searchsploit:** `searchsploit <service> <version>`.
3.  **Google:** "Port 27017 exploit github".