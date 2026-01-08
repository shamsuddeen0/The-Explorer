
# 🕸️ Deep Dive: Web Fuzzing (Directory & Parameter Busting)

I have added sections on Parameter Fuzzing (finding hidden variables like ?admin=true), VHost Discovery, and Filtering Noise, which is the most important skill when using FFUF.

**"A web server will only tell you what exists if you ask the right question."**

Fuzzing (or brute-forcing) is the act of sending thousands of requests to a web server to find hidden resources that aren't linked on the homepage. This includes admin panels, backup files, hidden API endpoints, and development subdomains.

---

## 🛠️ The Tools of the Trade

| Tool | Pros | Cons | Use Case |
| :--- | :--- | :--- | :--- |
| **FFUF** | Insanely fast, highly customizable, scriptable. | Harder syntax to learn. | **My daily driver** for everything. |
| **Gobuster** | Simple syntax, easy to remember. | Slower than FFUF, less flexible. | Quick scans, directory busting. |
| **Dirbuster** | GUI-based, easy for beginners. | Slow, crashes often, outdated. | I rarely use this anymore. |

---

## 🏎️ FFUF: The Master Guide
*Syntax: `ffuf -u <URL> -w <WORDLIST>`*
*The magic word is `FUZZ`. Wherever you put `FUZZ`, FFUF injects the wordlist.*

### 1. Directory Discovery (The Basic Scan)
Finding folders like `/admin`, `/login`, `/dev`.
```bash
ffuf -u http://target.com/FUZZ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```
### 2. Extension Hunting (Finding Files)

Finding specific files like index.php, config.yaml, backup.zip.

```bash
# -e specifies extensions
ffuf -u http://target.com/FUZZ -w common.txt -e .php,.html,.txt,.bak,.zip
```
### 3. VHost / Subdomain Discovery

Sometimes a "subdomain" is just a virtual host configuration on the same IP. We fuzzy the Host Header.

```bash
# We scan the IP, but we change the Host Header
ffuf -u http://10.10.10.10 -w subdomains.txt -H "Host: FUZZ.target.com"
```
### 4. Parameter Fuzzing (Hidden Inputs)

The page exists, but maybe it behaves differently if I send a specific parameter?

Example: http://target.com/index.php?debug=true

```bash
ffuf -u http://target.com/index.php?FUZZ=1 -w parameters.txt
```
## 🔇 Filtering the Noise (Critical Skill)

Websites often return a 200 OK for everything but show a "Page Not Found" text. This fills your screen with junk results.

How to fix it:

Run the scan for 10 seconds.

Note the size (Size), words (Words), or lines (Lines) of the junk responses.

Filter them out.

The Command:

```bash
# -fs 4242 : Filter out responses that are exactly 4242 bytes (Size)
# -fc 403  : Filter out 403 Forbidden codes
ffuf -u http://target.com/FUZZ -w common.txt -fs 4242 -fc 404
```

## 🔁 Recursion (Going Deeper)

If FFUF finds /admin, I want it to automatically scan inside /admin/ without me stopping.

```bash
# -recursion : Turn on recursion
# -recursion-depth 2 : How many folders deep to go
ffuf -u http://target.com/FUZZ -w common.txt -recursion
```
## 🏁 Gobuster Cheat Sheet

If FFUF is too complex, Gobuster is reliable and simple.

Directory Mode
```bash
gobuster dir -u http://target.com -w common.txt -x php,txt,html -t 50
# -x: extensions
# -t: threads (speed)
```
### DNS Mode (Subdomains)
```bash
gobuster dns -d target.com -w subdomains.txt
``` 
### VHost Mode
```bash
gobuster vhost -u http://target.com -w subdomains.txt
```
## 📚 The Golden Wordlists (Seclists)

Don't use the default Kali lists if you can help it. Install `Seclists`.

Directories: `Discovery/Web-Content/directory-list-2.3-medium.txt`

Files: `Discovery/Web-Content/raft-medium-files.txt`

Subdomains: `Discovery/DNS/subdomains-top1million-5000.txt`

Parameters: `Discovery/Web-Content/burp-parameter-names.txt`

API Endpoints: `Discovery/Web-Content/api-endpoints.txt`

## 🧠 Pro-Tips for Real Engagements

1. Rate Limiting: Don't hammer a production server with 100 threads. Use `-t 10` or set a delay `-p 0.1` (0.1 seconds between requests).

2. User-Agent: Some firewalls block the default "FFUF" user agent. Change it:

`ffuf -u URL -w LIST -H "User-Agent: Mozilla/5.0 Windows..."`

3. Status Codes: Don't ignore 403 Forbidden. Sometimes 403 means "You found the admin panel, you just need to bypass the login."
