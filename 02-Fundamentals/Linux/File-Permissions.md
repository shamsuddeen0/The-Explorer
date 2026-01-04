
# 🔐 Linux File Permissions & Ownership

**"In Linux, everything is a file. If you control the permissions, you control the system."**

One of the most common ways to hack a Linux machine (Privilege Escalation) is by finding a file with weak permissions. This guide explains how to read, modify, and understand Linux security controls.

---

## 📜 Reading Permissions (`ls -l`)
When you run `ls -l`, you see something like this:
```bash 
-rwxr-xr--  1  root  staff  4096  Jan 1  12:00  script.sh
```

``` 
```
## `Breaking it down`:

1. Type (`-`):

~ `-` = Regular File

~ `d` = Directory

2. Permissions (`rwxr-xr--`):

Broken into 3 sets of 3: `Owner` | `Group` | `World (Everyone)`

~ `rwx` (Owner can Read, Write, Execute)

~ `r-x` (Group can Read, Execute)

~ `r--` (World can Read only)

3. Owner (`root`): The user who owns the file.

4. Group (`staff`): The group that owns the file.
```  `

### 🧮 The "Chmod" Math (Octal Notation)

Permissions are often represented by numbers.

Value	Letter	Meaning
4	`r`	Read (View contents)
2	`w`	Write (Modify/Delete)
1	`x`	Execute (Run as a program)
0	`-`	No Permission
Common Examples:

`chmod 777`: (4+2+1) Everyone can do everything. (Dangerous!)

`chmod 755`: Owner (7), Group (5), World (5). Standard for scripts.

`chmod 600`: Owner (6), Group (0), World (0). Standard for SSH keys (Read/Write for owner ONLY).

`chmod +x`: Adds "Execute" permission for everyone.

### 👮 Changing Ownership (chown)

Only the root user can give away ownership of a file.
```bash
# Syntax: chown user:group file
sudo chown kali:kali exploit.py
```

`chown root file`: Change owner to root.

`chown :admin file`: Change group to admin.

`chown -R: Recursive`. Changes ownership for a folder and everything inside it.

### ☣️ Special Permissions (The Dangerous Ones)

These are critical for Privilege Escalation.

1. SUID (Set User ID)

Symbol: `s` in the owner section (e.g., `-rwsr-xr-x`).

What it means: When you run this file, it runs with the permissions of the Owner (usually root), not the user who ran it.

Risk: If `nmap` has SUID, I can run `nmap` as root and escape to a shell!

2. SGID (Set Group ID)

Symbol: `s` in the group section.

What it means: Files created in this directory inherit the group ownership of the directory. Useful for shared folders.

3. Sticky Bit

Symbol: `t` at the end (e.g., `drwxrwxrwt`).

What it means: Only the owner (or root) can delete files in this folder.

Example: `/tmp` directory. Anyone can write there, but I can't delete your files.

### 🛠️ Practical Commands
Fixing "Permission Denied" on a Script
```bash 
chmod +x script.sh
./script.sh
```
## Making an SSH Key usable

If you try to use a key with bad permissions, SSH will reject it.

```bash 
chmod 600 id_rsa
ssh -i id_rsa user@target
``` 
## Hunting for SUID Files (CTF Trick)
```bash 
find / -perm -4000 2>/dev/null
```
