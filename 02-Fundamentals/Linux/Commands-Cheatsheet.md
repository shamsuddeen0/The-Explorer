
Here is the Commands-Cheatsheet.md. This isn't just ls and cd. This is a collection of the commands you actually use during a CTF or Pentest to find files, manipulate text, and move data.

# 🐧 Essential Linux Commands for Hackers

**"Master the terminal, master the system."**

This is not a generic Linux tutorial. These are the specific commands and flags you will find yourself using repeatedly during CTFs and assessments.

---

## 🧭 Navigation & File Management

| Command | Usage | Description |
| :--- | :--- | :--- |
| `ls -la` | List All | Shows hidden files (`.git`, `.ssh`) and permissions. |
| `cd -` | Go Back | Switches to the *previous* directory you were in. |
| `pwd` | Path | "Print Working Directory." Where am I? |
| `mkdir -p` | Make Parent | Creates a folder tree (e.g., `mkdir -p recon/nmap/scans`). |
| `cp -r` | Copy Recursive | Copies a folder and everything inside it. |
| `rm -rf` | **Nuke It** | Force deletes a folder. *Use with extreme caution.* |

---

## 🔍 Finding Things (The "Needle in a Haystack")

### `find`
The most powerful search tool.
```bash
# Find a file named "flag.txt" anywhere on the system
find / -name "flag.txt" 2>/dev/null

# Find files owned by a specific user (PrivEsc)
find / -user root -type f

# Find files with SUID bit set (PrivEsc)
find / -perm -4000 2>/dev/null
```
### `grep`

Searching inside files.

```bash
# Search for "password" in all files in the current folder
grep -r "password" .

# Search for a string but ignore case (-i)
grep -i "admin" users.txt
```
### `locate`

Faster than find, but uses a database.

```bash 
updatedb  # Update the database first!
locate exploit.py
```
### 📝 Text Manipulation (The "Awk/Sed" Magic)
```
cat file.txt           # Dump entire file.
less file.txt          # Scroll through file (press q to quit).
head -n 5 file.txt     # Show first 5 lines.
tail -f /var/log/syslog # Watch a log file update in real-time.
```
`cat`/`less`/`head`/`tail`

### Reading files.

cat file.txt: `Dump entire file.`

less file.txt: `Scroll through file (press q to quit).`

head -n 5 file.txt: `Show first 5 lines.`

tail -f /var/log/syslog: `Watch a log file update in real-time.`

### `sort & uniq`

Organizing lists.
```

```bash 
# Sort a list alphabetically and remove duplicates
sort subs.txt | uniq > clean_subs.txt
```
### `cut`

Extracting specific columns.

```bash 
# Extract usernames (column 1) from /etc/passwd
cat /etc/passwd | cut -d ":" -f 1
```
### `awk`

The advanced text processor.

```bash 
# Print the 4th word of every line
cat file.txt | awk '{print $4}'
```
### 🌐 Networking from Terminal

| Command | Usage |
| :--- | :--- |
| `ip a` | Show IP |
| `ping -c 4 <IP>` | Connectivity |
| `netstat -antp` | Connections |
| `ss -lntp` | Sockets |
| `curl -I <URL>` | Headers |
| `wget <URL>` | Download |
### 📦 System Info (Recon)
```bash 
```
`uname -a`: Kernel version (Critical for finding kernel exploits).

`whoami`: Current user.

`id`: Current group privileges.

`ps aux`: List all running processes (Is there an antivirus running?).

`env`: Show environment variables (Look for secrets/keys).

`history`: Show previous commands typed by the user (Often contains passwords!).
```
```
### 🗜️ Archiving & Compression
```bash
```
### Exfiltrating data.

Zip: `zip -r data.zip folder/`

Unzip: `unzip data.zip`

Tar (Compress): `tar -czvf backup.tar.gz folder/`

Tar (Extract): `tar -xzvf backup.tar.gz`

```
uname -a    # Kernel version (Critical for finding kernel exploits).
whoami      # Current user.
id          # Current group privileges.
ps aux      # List all running processes (Is there an antivirus running?).
env         # Show environment variables (Look for secrets/keys).
history     # Show previous commands typed by the user (Often contains passwords!).
```