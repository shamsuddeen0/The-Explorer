This section focuses on SMB (Server Message Block), which is arguably the most common and critical protocol you will attack in internal networks.

I have rewritten this as a "Hacker's Workflow" rather than just a list of commands. It explains the progression: Finding it -> Checking for "Null" Access -> Listing Shares -> Hunting for Users.


# 📂 SMB & NetBIOS Enumeration

**"The bread and butter of internal network pentesting."**

SMB (Server Message Block) is the protocol Windows uses to share files and printers. If I am on an internal network, finding an open SMB share is often like finding a sticky note with a password on it.

This page documents my workflow for enumerating Ports **139 (NetBIOS)** and **445 (SMB)**.

---

## 1️⃣ Discovery & Version Detection
Before attacking, I need to know what I am dealing with. Is it Windows 7? Windows 10? A Samba server on Linux?

### Using Nmap
I don't just scan the port; I want the OS version and the computer name.
```bash
# -sV: Version detection
# --script smb-os-discovery: The best script to get the exact Windows version
nmap -p 139,445 --script smb-os-discovery 192.168.10.10

```
Using Metasploit (If Nmap fails)

Sometimes Nmap gets stuck. Metasploit has a dedicated scanner that is often more accurate for version detection.

```bash
msfconsole -q -x "use auxiliary/scanner/smb/smb_version; set RHOSTS 192.168.10.10; run; exit"
```

### 2️⃣ The "Null Session" Check (No Credentials)

This is the first thing I check. A "Null Session" means the server allows me to log in with no username and no password.

Checking Access with SMBMap

smbmap is my favorite tool for a quick overview because it shows permissions (Read/Write) right away.

```bash
# -u "null" forces a null session check
# -H is the host
smbmap -H 192.168.10.10 -u "null"
```

Result READ: I can list files.

Result WRITE: I can upload malicious files (Exploit opportunity!).

Checking Access with SMBClient

If I prefer a manual interaction (like an FTP client):

```bash
# -L lists shares
# -N means "No Password"
smbclient -L //192.168.10.10 -N
```
### 3️⃣ Enumerating Users (RID Cycling)

If I can't access files, I might still be able to query the server for a list of valid users. This helps me build a username list for password spraying later.

Using Lookupsid (Metasploit)

Windows assigns every user a numeric ID (RID). The Administrator is always 500. We can "guess" RIDs to find usernames.

```bash
use auxiliary/scanner/smb/smb_lookupsid
set RHOSTS 192.168.10.10
run
```
Using Python (More portable)

If I don't want to open Metasploit:

```bash
# Part of Impacket scripts
lookupsid.py anonymous@192.168.10.10
```
### 4️⃣ "The Heavy Lifter": Enum4Linux

When I want to run everything at once, I use enum4linux. It is a wrapper that runs smbclient, rpcclient, net, and nmblookup all together.

```bash
# -a: Do EVERYTHING (Users, Shares, Groups, OS Info)
enum4linux -a 192.168.10.10
```

Note: This tool is noisy and produces a lot of output. I usually pipe it to a file: enum4linux -a target > enum.txt

### 5️⃣ Interacting with Shares (The Loot)

Once I find a share I can access (e.g., a share named Finance or Backups), I connect to it to hunt for passwords.

Connecting via SMBClient
```bash
# Syntax: //IP/ShareName
# -U "%" tries to login with a blank user
smbclient //192.168.10.10/Finance -U "%"
```

Useful Commands inside the shell:

dir: List files.

get file.txt: Download a file.

recurse ON: Turn on recursive search (like ls -R).

mget *: Download everything.

Mounting the Share (Linux)

Sometimes it's easier to mount the share to my Kali machine so I can use grep or open files in a text editor.

```bash
mkdir /mnt/target_share
mount -t cifs //192.168.10.10/Finance /mnt/target_share -o user=guest,password=""
```
### 6️⃣ Advanced: RPCClient

If smbclient fails, rpcclient talks directly to the Remote Procedure Call (RPC) interface. It is less user-friendly but very powerful for finding info.

```bash
# Connect as a blank user
rpcclient -U "" 192.168.10.10
```
Key Commands to type once connected:

srvinfo: System information.

enumdomusers: List all users in the domain.

enumdomgroups: List all groups (look for "Domain Admins").

querygroupmem 0x200: List members of the group (0x200 is usually Domain Admins).

### ⚠️ Dangerous Nmap Scripts

Nmap has a category of scripts that check for vulnerabilities (like EternalBlue).
Warning: These can crash older servers (Legacy Windows 2003/XP or Windows 7). Use with caution.

```bash
# --script-args=unsafe=1 tells Nmap "I don't care if it crashes, run the check."
nmap -p 445 --script smb-vuln-* --script-args=unsafe=1 192.168.10.10
```

Common Hit: smb-vuln-ms17-010 (EternalBlue). If you see this, it's usually an instant root shell.

### 📚 Mounting Shares on Windows (Post-Exploitation)

If I have compromised a Windows machine and want to connect to another share on the network:

```bash
net use Z: \\192.168.10.10\Backups /USER:domain\user password
```