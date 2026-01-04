We are now creating the Service-Specific directory inside Scanning. This is where you detail exactly how to attack common services.

# 📂 SMB Enumeration (Port 445/139)

**"The bread and butter of internal hacking."**

SMB (Server Message Block) allows Windows computers to share files and printers. Misconfigurations here can lead to easy access.

---

## 🛑 The "Null Session" Check
*Can I log in without a password?*

### Using SMBClient
```bash
# List shares with NO password
smbclient -L //192.168.1.10 -N
```

Result: If it works, check shares like C$, ADMIN$, or Backups.

### Using SMBMap

A faster tool that shows permissions (Read/Write) instantly.

```bash
smbmap -H 192.168.1.10 -u "null"
```
## 📜 Using Enum4Linux

The all-in-one tool for SMB.
It tries to list users, shares, password policies, and OS info.

```bash
enum4linux -a 192.168.1.10
```
## 🔎 Connecting to a Share

If I find a share called Secret, I connect like this:

```bash 
smbclient //192.168.1.10/Secret
```

`ls`: List files.

`get <file>`: Download file.

`recurse ON`: Enable recursive mode.

`mget *`: Download everything.

## 🔓 Common Vulnerabilities

EternalBlue (MS17-010):

Scan: `nmap -p 445 --script smb-vuln-ms17-010 <IP>`

Exploit: `Metasploit exploit/windows/smb/ms17_010_eternalblue`.

Anonymous Write Access:

If I can write to a share, I can upload a malicious file or replace a config.

## FOR ADVANCE SMB_ENUMERATION GO TO :
[Smb_enum] (https://github.com/shamsuddeen0/The-Explorer/blob/main/reconn/smb_enum.md#-smb--netbios-enumeration)