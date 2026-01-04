# 🔢 The Hacker's Guide to Subnetting

**"Don't panic. It's just counting in groups."**

Subnetting is the process of dividing a network into smaller, manageable pieces. As a pentester, mastering this is non-negotiable. If you don't understand subnets, you won't know which IPs to scan, and you won't know how to pivot into internal networks.

---

## 📍 CIDR Notation (The "Slash" Number)
You will see IPs written like `192.168.1.0/24`. The `/24` is the CIDR (Classless Inter-Domain Routing). It tells you **how big the network is**.

| CIDR | Subnet Mask | Total IPs | Usable IPs | Cheat Sheet Meaning |
| :--- | :--- | :--- | :--- | :--- |
| **/32** | `255.255.255.255` | 1 | 1 | **Single Host** (e.g., specific target). |
| **/30** | `255.255.255.252` | 4 | 2 | **Router Link**. (Only 2 IPs for both ends). |
| **/29** | `255.255.255.248` | 8 | 6 | Small DMZ or static IP block. |
| **/24** | `255.255.255.0` | 256 | 254 | **Standard LAN**. (Home/Office WiFi). |
| **/23** | `255.255.254.0` | 512 | 510 | Medium Office (2 LANs combined). |
| **/16** | `255.255.0.0` | 65,536 | 65,534 | **Large Corp Network**. |
| **/8** | `255.0.0.0` | 16 Million | Huge | **Entire Company** (e.g., Apple, IBM, DoD). |

---

## 🏠 Private vs. Public IPs (RFC 1918)
*The most important rule of OSINT.*

Private IPs are used **inside** a network (LAN). They are not routable on the internet. If you scan these from your house, nothing will happen.

| Class | Range | Description |
| :--- | :--- | :--- |
| **A** | `10.0.0.0` - `10.255.255.255` | Large Enterprise Networks |
| **B** | `172.16.0.0` - `172.31.255.255` | Cloud (AWS/Azure/Docker/VirtualBox) |
| **C** | `192.168.0.0` - `192.168.255.255` | Home Routers / Small Office |

**The Hacker's Rule:**
1.  **Public IP:** The target's front door (Firewall/VPN).
2.  **Private IP:** The juicy internal servers (Database/AD). I must breach the Public IP first to reach these.

---

## 📡 Essential IP Concepts

### 1. Network ID vs. Broadcast IP
Every subnet has two IPs you **cannot** use for computers:
*   **First IP (Network ID):** Identifies the subnet itself (e.g., `192.168.1.0`).
*   **Last IP (Broadcast):** Used to shout "Hello!" to everyone (e.g., `192.168.1.255`).

### 2. Special Addresses
*   **`127.0.0.1` (Localhost):** "Home." Refers to the machine I am currently on.
*   **`0.0.0.0`:** "All Interfaces." If a server listens on `0.0.0.0`, it accepts connections from anywhere.
*   **`169.254.x.x` (APIPA):** "DHCP Failed." The computer couldn't find a router, so it gave itself a fake IP.

---

## 🔄 Pivoting: The Art of Moving Deeper
*This is why subnetting matters for hacking.*

**Scenario:**
1.  I hack a Web Server at `10.10.10.5` (Public facing).
2.  I run `ip a` on the compromised server.
3.  I see a second interface: `eth1: 172.16.50.5/24`.
4.  **Analysis:** Aha! This server is connected to a *hidden internal network* (`172.16.50.0/24`).
5.  **Action:** I set up a proxy (Chisel/Sshuttle) to tunnel my traffic through the web server to scan `172.16.50.1-254`.

---

## 🧮 How to Calculate Usable IPs (The Easy Way)
*Don't do binary. Just remember the formula:*

**Formula:** `2^(32 - CIDR) - 2`

**Example: A /28 network.**
1.  `32 - 28 = 4`.
2.  `2^4 = 16` (Total IPs).
3.  `16 - 2 = 14` (Usable Hosts).

**Why -2?**
Because you lose the Network ID (first) and Broadcast IP (last).

---

## 🧪 Practical Cheat Sheet: "How big is the scan?"
When running Nmap, check the CIDR to know how long it will take.

| Scan Command | Target Scope | Time Estimate |
| :--- | :--- | :--- |
| `nmap 192.168.1.5` | 1 IP | Seconds |
| `nmap 192.168.1.0/24` | 256 IPs | Minutes |
| `nmap 10.10.0.0/16` | 65,000 IPs | Hours (Don't do this without filtering!) |
| `nmap 10.0.0.0/8` | 16 Million IPs | Years (You will get fired) |