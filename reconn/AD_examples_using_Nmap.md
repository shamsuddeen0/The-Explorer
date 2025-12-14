# 🏰 Active Directory Enumeration (Example: GOAD)

Scanning an Active Directory environment is different from a typical CTF. You aren't just looking for "Open Ports"; you are looking for **Roles** (Is it a DC? A File Server?) and **Misconfigurations**.

Below is a real-world example from my time hacking the [GOAD (Game of Active Directory)](https://github.com/Orange-Cyberdefense/GOAD/) lab.

---

## ⚡ The Command
I ran a full port scan (`-p-`) with scripts and versions (`-sC -sV`). I used `-Pn` because Windows hosts often block Ping requests.


This is the "Deep Dive" version. Since you want to document your learning journey, this version breaks down exactly how you read the matrix.

It explains the specific ports that identify a Domain Controller, how to spot the difference between a DC and a regular server, and why "SMB Signing" is the first thing hackers look for in these logs.

Here is the code for your AD Enumeration file:

# 🏰 Deep Dive: Active Directory Enumeration (GOAD Lab)

Scanning an Active Directory (AD) environment is the most complex Nmap scan you will do. You aren't just looking for "vulnerabilities"—you are mapping an entire political structure (Forests, Domains, Trusts, and Roles).

Below is a detailed breakdown of how I analyze a scan from the **GOAD (Game of Active Directory)** environment.

---

## ⚡ The Command Breakdown
```bash
nmap -Pn -p- -sC -sV -oA full_scan_goad 192.168.56.10-12,22-23
```

-Pn: Crucial for Windows. Windows Defender often blocks the "Ping" (ICMP) that Nmap sends to check if a host is alive. This flag tells Nmap: "Assume it's alive and scan it anyway."

-p-: Scan all 65,535 ports. AD services (like RPC) often run on high random ports (49000+).

-sC -sV: Run default scripts (very important for SMB info) and grab version banners.

### 🔍 Detailed Analysis: Reading the Matrix
1. How to Spot a Domain Controller (DC)

Looking at the scan for 192.168.56.10 (King's Landing), 192.168.56.11 (Winterfell), and 192.168.56.12 (Meereen), I know immediately they are Domain Controllers.

The Smoking Gun Ports:

Port 88 (Kerberos): Only DCs run the Kerberos authentication service. If this is open, it is the boss.

Port 389 / 636 (LDAP): This is the directory listing service.

Port 53 (DNS): In AD, the DC usually acts as the DNS server.

Why this matters:
I now know these are the "Crown Jewels." I also know I can perform User Enumeration (looking for valid usernames) against these IPs using tools like Kerbrute.

2. Spotting "Member Servers" (The Weak Points)

Looking at 192.168.56.22 (Castel Black) and 192.168.56.23 (Braavos), notice what is missing:

❌ No Port 88 (Kerberos)

❌ No Port 389 (LDAP)

These are regular servers joined to the domain.

Port 1433 (MSSQL): Detected on both. This is a huge attack vector. I can check for default credentials (sa:password) or try to force the SQL server to authenticate back to me (capturing its hash).

Port 80 (IIS): Web servers. These are likely the entry points into the network via a web vulnerability.

3. The Critical Finding: SMB Signing

The Nmap script (smb2-security-mode) is the most valuable part of this entire log. It dictates my attack path.

Target: 192.168.56.10 (The DC)

```bash 
| smb2-security-mode: 
|_    Message signing enabled and required
```

Interpretation: This machine forces security. I cannot perform an NTLM Relay attack against it. If I try to forward a stolen login to this machine, it will reject it.

Target: 192.168.56.23 (Braavos)

```bash 
| smb2-security-mode: 
|_    Message signing enabled but not required (or disabled)
```
### Interpretation: 🚨 VULNERABILITY FOUND.

Since signing is not required, I can perform an SMB Relay Attack.

Scenario: If an administrator logs into a different machine, I can steal their session and relay it to Braavos to get a remote shell (using impacket-ntlmrelayx).

### 4. Mapping the Domain Structure (DNS)

Nmap pulls the "CommonName" from the SSL certificates and the DNS info.

.10 = sevenkingdoms.local (Parent Domain)

.11 = north.sevenkingdoms.local (Child Domain)

.12 = essos.local (Separate Domain/Forest)

Action Item: I must immediately add these IPs and hostnames to my /etc/hosts file. Without this, tools like Kerbrute or Bloodhound will fail to resolve the targets.

5. Time Synchronization
```bash
server time: 2025-07-03 15:14:43Z
```

Kerberos is extremely sensitive to time. If my attacker machine is more than 5 minutes out of sync with the DC, authentication attacks will fail.

Action Item: Run ntpdate 192.168.56.10 to sync my clock before attacking.

### 📂 Raw Scan Output

```
(websploit# nmap -Pn -p- -sC -sV -oA full_scan_goad 192.168.56.10-12,22-23
Nmap scan report for sevenkingdoms.local (192.168.56.10)
Host is up (0.00066s latency).
Not shown: 65511 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-07-03 15:14:43Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername:<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2025-06-30T06:55:19
|_Not valid after:  2026-06-30T06:55:19
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername:<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2025-06-30T06:55:19
|_Not valid after:  2026-06-30T06:55:19
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername:<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2025-06-30T06:55:19
|_Not valid after:  2026-06-30T06:55:19
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Subject Alternative Name: othername:<unsupported>, DNS:kingslanding.sevenkingdoms.local
| Not valid before: 2025-06-30T06:55:19
|_Not valid after:  2026-06-30T06:55:19
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=kingslanding.sevenkingdoms.local
| Not valid before: 2025-06-27T22:40:05
|_Not valid after:  2025-12-27T22:40:05
| rdp-ntlm-info: 
|   Target_Name: SEVENKINGDOMS
|   NetBIOS_Domain_Name: SEVENKINGDOMS
|   NetBIOS_Computer_Name: KINGSLANDING
|   DNS_Domain_Name: sevenkingdoms.local
|   DNS_Computer_Name: kingslanding.sevenkingdoms.local
|   DNS_Tree_Name: sevenkingdoms.local
|   Product_Version: 10.0.17763
|_  System_Time: 2025-07-03T15:16:37+00:00
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
5986/tcp  open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
| tls-alpn: 
|_  http/1.1
| ssl-cert: Subject: commonName=VAGRANT
| Subject Alternative Name: DNS:VAGRANT, DNS:vagrant
| Not valid before: 2025-06-27T15:18:01
|_Not valid after:  2025-06-26T15:18:01
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49677/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49678/tcp open  msrpc         Microsoft Windows RPC
49680/tcp open  msrpc         Microsoft Windows RPC
49693/tcp open  msrpc         Microsoft Windows RPC
49708/tcp open  msrpc         Microsoft Windows RPC
59098/tcp open  msrpc         Microsoft Windows RPC
MAC Address: 08:00:27:57:A4:F2 (Oracle VirtualBox virtual NIC)
Service Info: Host: KINGSLANDING; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: KINGSLANDING, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:57:a4:f2 (Oracle VirtualBox virtual NIC)
| smb2-time: 
|   date: 2025-07-03T15:16:34
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required

Nmap scan report for winterfell.north.sevenkingdoms.local (192.168.56.11)
Host is up (0.00068s latency).
Not shown: 65513 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-07-03 15:14:49Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername:<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2025-06-30T13:20:52
|_Not valid after:  2026-06-30T13:20:52
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername:<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2025-06-30T13:20:52
|_Not valid after:  2026-06-30T13:20:52
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername:<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2025-06-30T13:20:52
|_Not valid after:  2026-06-30T13:20:52
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername:<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2025-06-30T13:20:52
|_Not valid after:  2026-06-30T13:20:52
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Not valid before: 2025-06-27T22:49:00
|_Not valid after:  2025-12-27T22:49:00
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: NORTH
|   NetBIOS_Domain_Name: NORTH
|   NetBIOS_Computer_Name: WINTERFELL
|   DNS_Domain_Name: north.sevenkingdoms.local
|   DNS_Computer_Name: winterfell.north.sevenkingdoms.local
|   DNS_Tree_Name: sevenkingdoms.local
|   Product_Version: 10.0.17763
|_  System_Time: 2025-07-03T15:16:36+00:00
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
5986/tcp  open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| tls-alpn: 
|_  http/1.1
|_http-server-header: Microsoft-HTTPAPI/2.0
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
|_http-title: Not Found
| ssl-cert: Subject: commonName=VAGRANT
| Subject Alternative Name: DNS:VAGRANT, DNS:vagrant
| Not valid before: 2025-06-27T15:21:08
|_Not valid after:  2025-06-26T15:21:08
9389/tcp  open  mc-nmf        .NET Message Framing
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49670/tcp open  msrpc         Microsoft Windows RPC
49672/tcp open  msrpc         Microsoft Windows RPC
49702/tcp open  msrpc         Microsoft Windows RPC
65091/tcp open  msrpc         Microsoft Windows RPC
MAC Address: 08:00:27:3E:C3:EF (Oracle VirtualBox virtual NIC)
Service Info: Host: WINTERFELL; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: WINTERFELL, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:3e:c3:ef (Oracle VirtualBox virtual NIC)
| smb2-time: 
|   date: 2025-07-03T15:16:37
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required

Nmap scan report for essos.local (192.168.56.12)
Host is up (0.00046s latency).
Not shown: 65513 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-07-03 15:15:01Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername:<unsupported>, DNS:meereen.essos.local
| Not valid before: 2025-06-29T08:30:39
|_Not valid after:  2026-06-29T08:30:39
445/tcp   open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds (workgroup: ESSOS)
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername:<unsupported>, DNS:meereen.essos.local
| Not valid before: 2025-06-29T08:30:39
|_Not valid after:  2026-06-29T08:30:39
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername:<unsupported>, DNS:meereen.essos.local
| Not valid before: 2025-06-29T08:30:39
|_Not valid after:  2026-06-29T08:30:39
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: essos.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=meereen.essos.local
| Subject Alternative Name: othername:<unsupported>, DNS:meereen.essos.local
| Not valid before: 2025-06-29T08:30:39
|_Not valid after:  2026-06-29T08:30:39
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=meereen.essos.local
| Not valid before: 2025-06-27T22:40:08
|_Not valid after:  2025-12-27T22:40:08
| rdp-ntlm-info: 
|   Target_Name: ESSOS
|   NetBIOS_Domain_Name: ESSOS
|   NetBIOS_Computer_Name: MEEREEN
|   DNS_Domain_Name: essos.local
|   DNS_Computer_Name: meereen.essos.local
|   DNS_Tree_Name: essos.local
|   Product_Version: 10.0.14393
|_  System_Time: 2025-07-03T15:16:38+00:00
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
5986/tcp  open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
|_http-title: Not Found
| ssl-cert: Subject: commonName=VAGRANT
| Subject Alternative Name: DNS:VAGRANT, DNS:vagrant
| Not valid before: 2025-06-27T15:23:42
|_Not valid after:  2025-06-26T15:23:42
|_http-server-header: Microsoft-HTTPAPI/2.0
| tls-alpn: 
|   h2
|_  http/1.1
9389/tcp  open  mc-nmf        .NET Message Framing
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49677/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49678/tcp open  msrpc         Microsoft Windows RPC
49680/tcp open  msrpc         Microsoft Windows RPC
49693/tcp open  msrpc         Microsoft Windows RPC
60502/tcp open  msrpc         Microsoft Windows RPC
MAC Address: 08:00:27:33:DF:2F (Oracle VirtualBox virtual NIC)
Service Info: Host: MEEREEN; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-07-03T15:16:38
|_  start_date: 2025-07-03T13:56:01
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_nbstat: NetBIOS name: MEEREEN, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:33:df:2f (Oracle VirtualBox virtual NIC)
|_clock-skew: mean: 42m00s, deviation: 2h12m50s, median: 0s
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: meereen
|   NetBIOS computer name: MEEREEN\x00
|   Domain name: essos.local
|   Forest name: essos.local
|   FQDN: meereen.essos.local
|_  System time: 2025-07-03T08:16:38-07:00

Nmap scan report for castelblack.north.sevenkingdoms.local (192.168.56.22)
Host is up (0.00066s latency).
Not shown: 65525 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-title: Site doesn't have a title (text/html).
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2025-07-03T13:57:05
|_Not valid after:  2052-07-03T13:57:05
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
| ms-sql-ntlm-info: 
|   Target_Name: NORTH
|   NetBIOS_Domain_Name: NORTH
|   NetBIOS_Computer_Name: CASTELBLACK
|   DNS_Domain_Name: north.sevenkingdoms.local
|   DNS_Computer_Name: castelblack.north.sevenkingdoms.local
|   DNS_Tree_Name: sevenkingdoms.local
|_  Product_Version: 10.0.17763
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=castelblack.north.sevenkingdoms.local
| Not valid before: 2025-06-27T22:56:08
|_Not valid after:  2025-12-27T22:56:08
| rdp-ntlm-info: 
|   Target_Name: NORTH
|   NetBIOS_Domain_Name: NORTH
|   NetBIOS_Computer_Name: CASTELBLACK
|   DNS_Domain_Name: north.sevenkingdoms.local
|   DNS_Computer_Name: castelblack.north.sevenkingdoms.local
|   DNS_Tree_Name: sevenkingdoms.local
|   Product_Version: 10.0.17763
|_  System_Time: 2025-07-03T15:16:40+00:00
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
5986/tcp  open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| ssl-cert: Subject: commonName=VAGRANT
| Subject Alternative Name: DNS:VAGRANT, DNS:vagrant
| Not valid before: 2025-06-27T15:26:52
|_Not valid after:  2025-06-26T15:26:52
| tls-alpn: 
|_  http/1.1
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
|_ssl-date: 2025-07-03T15:17:20+00:00; 0s from scanner time.
49666/tcp open  msrpc         Microsoft Windows RPC
49677/tcp open  msrpc         Microsoft Windows RPC
MAC Address: 08:00:27:B9:6C:19 (Oracle VirtualBox virtual NIC)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-07-03T15:16:38
|_  start_date: N/A
| ms-sql-info: 
|   192.168.56.22:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
|_nbstat: NetBIOS name: CASTELBLACK, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:b9:6c:19 (Oracle VirtualBox virtual NIC)
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required

Stats: 0:09:09 elapsed; 4 hosts completed (5 up), 1 undergoing Script Scan
NSE Timing: About 99.93% done; ETC: 17:20 (0:00:00 remaining)
Nmap scan report for braavos.essos.local (192.168.56.23)
Host is up (0.00044s latency).
Not shown: 65524 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info: 
|   Target_Name: ESSOS
|   NetBIOS_Domain_Name: ESSOS
|   NetBIOS_Computer_Name: BRAAVOS
|   DNS_Domain_Name: essos.local
|   DNS_Computer_Name: braavos.essos.local
|   DNS_Tree_Name: essos.local
|_  Product_Version: 10.0.14393
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2025-07-03T13:57:51
|_Not valid after:  2052-07-03T13:57:51
|_ssl-date: 2025-07-03T15:20:40+00:00; 0s from scanner time.
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: ESSOS
|   NetBIOS_Domain_Name: ESSOS
|   NetBIOS_Computer_Name: BRAAVOS
|   DNS_Domain_Name: essos.local
|   DNS_Computer_Name: braavos.essos.local
|   DNS_Tree_Name: essos.local
|   Product_Version: 10.0.14393
|_  System_Time: 2025-07-03T15:20:00+00:00
| ssl-cert: Subject: commonName=braavos.essos.local
| Not valid before: 2025-06-27T22:56:08
|_Not valid after:  2025-12-27T22:56:08
|_ssl-date: 2025-07-03T15:20:40+00:00; 0s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
5986/tcp  open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| ssl-cert: Subject: commonName=VAGRANT
| Subject Alternative Name: DNS:VAGRANT, DNS:vagrant
| Not valid before: 2025-06-27T15:30:05
|_Not valid after:  2025-06-26T15:30:05
| tls-alpn: 
|   h2
|_  http/1.1
|_ssl-date: 2025-07-03T15:20:40+00:00; 0s from scanner time.
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49669/tcp open  msrpc         Microsoft Windows RPC
49685/tcp open  msrpc         Microsoft Windows RPC
49778/tcp open  msrpc         Microsoft Windows RPC
MAC Address: 08:00:27:A3:67:1D (Oracle VirtualBox virtual NIC)
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-07-03T15:20:00
|_  start_date: 2025-07-03T13:57:42
|_nbstat: NetBIOS name: BRAAVOS, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:a3:67:1d (Oracle VirtualBox virtual NIC)
| ms-sql-info: 
|   192.168.56.23:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: braavos
|   NetBIOS computer name: BRAAVOS\x00
|   Domain name: essos.local
|   Forest name: essos.local
|   FQDN: braavos.essos.local
|_  System time: 2025-07-03T08:20:00-07:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: 52m29s, deviation: 2h28m29s, median: 0s

Post-scan script results:
| clock-skew: 
|   0s: 
|     192.168.56.11 (winterfell.north.sevenkingdoms.local)
|     192.168.56.22 (castelblack.north.sevenkingdoms.local)
|     192.168.56.12 (essos.local)
|     192.168.56.10 (sevenkingdoms.local)
|_    192.168.56.23 (braavos.essos.local)
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 5 IP addresses (5 hosts up) scanned in 572.00 seconds
```)

```

