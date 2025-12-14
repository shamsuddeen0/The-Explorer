# 🥷 Stealth Scanning: How to Avoid Detection

**"Loud scanners get blocked. Quiet scanners get shells."**

When I first started, I used to run `nmap -A -T4` on everything. In a real environment (Red Teaming) or a protected lab, this gets me banned in seconds.

Here are the techniques I use when I need to be "Ninja Mode."

---

## 🐢 Rule 1: Slow Down (Timing)
IDS (Intrusion Detection Systems) look for patterns. If I send 1000 packets in 1 second, I trigger an alert.
*   **The Flag:** `-T` (0-5)
*   **My Choice:** `-T2` (Polite).
    *   `-T0` (Paranoid) is too slow (wait 5 minutes between packets).
    *   `-T2` is the "Sweet Spot." It slows down the scan enough to often slip past rate-limiting firewalls without taking 100 years to finish.

## 🎭 Rule 2: Hide in the Crowd (Decoys)
If I can't stop the firewall from seeing the scan, I make it confused about *who* is scanning.
*   **The Flag:** `-D RND:10`
*   **The Concept:** Nmap sends packets from my IP, but *also* sends fake packets from 10 other random IPs. The defender sees 11 IPs scanning them at once and doesn't know which one is the real attacker.

## 🧩 Rule 3: Fragment Packets
Firewalls inspect packets to see if they look malicious (like a TCP SYN scan).
*   **The Flag:** `-f`
*   **The Concept:** This splits my packets into tiny little pieces (fragments). The firewall is too lazy (or unable) to reassemble them, so it lets the pieces through. The target machine reassembles them and responds to me.

## 🎯 Rule 4: Be Selective (Don't be Greedy)
Scanning 65,535 ports (`-p-`) is incredibly loud.
*   **My Strategy:** I only scan the "Top 20" or specific ports I suspect are open (80, 443, 22, 3389).
*   **The Flag:** `-p 80,443,22` instead of `-p-` or `-F`.

---

## 🕵️ My "Ultimate Stealth" Command
When I am terrified of getting caught, this is the specific command string I build:

```bash
nmap -sS -p 80,443,22,3389 -T2 --scan-delay 500ms -D RND:10 -f -Pn -n <TARGET_IP>
```
🧠 Breaking it down:

| Flag | What it does | Why I use it here |
|---|---|---|
| `-sS` | SYN Scan | "Half-open" scan. I don't complete the connection, so it's less likely to be logged by the application. |
| `-p 80,443,22,3389` | Specific Ports | I am only checking 4 ports. Scanning 65k ports is suicide for stealth. |
| `-T2` | Polite Timing | Slows down the scan to be respectful/evasive. |
| `--scan-delay 500ms` | Forced Delay | I force Nmap to wait half a second between probes. This defeats rate-limiting rules. |
| `-D RND:10` | Decoys | I spoof 10 random IPs so my IP is hidden in the logs. |
| `-f` | Fragment | Splits packets into small chunks to bypass packet inspection. |
| `-Pn` | No Ping | Treat host as online — I avoid pinging the host first to reduce noise. |
| `-n` | No DNS | I skip DNS lookups to avoid extra noisy queries. |

