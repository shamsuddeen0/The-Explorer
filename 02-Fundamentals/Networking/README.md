# 🌐 Networking Fundamentals

**"You can't break the network if you don't know how the packets flow."**

This directory contains my study notes on computer networking. Understanding these concepts is the single most important skill for a hacker. If you don't understand IP addressing, ports, and protocols, tools like Nmap and Burp Suite will just look like magic to you.

## 📄 Contents

### 1. [Protocols.md](Protocols.md)
*   **What it covers:** The languages computers speak.
*   **Key Topics:**
    *   **TCP vs. UDP:** The difference between "Reliable" and "Fast".
    *   **Common Ports:** A cheat sheet of the services I scan for (SSH, HTTP, SMB).
    *   **Attack Vectors:** Why FTP is dangerous and how DNS can be used to smuggle data.

### 2. [Subnetting-Guide.md](Subnetting-Guide.md)
*   **What it covers:** How IP addresses and networks are divided.
*   **Key Topics:**
    *   **CIDR Notation:** What `/24` actually means.
    *   **Public vs. Private IPs:** Why I can't hack `192.168.1.5` from across the internet.
    *   **The Binary Math:** A simplified way to calculate network ranges without a calculator.

### 3. [Common-Ports.md](Common-Ports.md)
*   **What it covers:** A quick reference list.
*   **Key Topics:**
    *   The "Top 20" ports every pentester memorizes.
    *   What tools to run against each port (e.g., Port 445 -> Run `enum4linux`).

---

## 🧠 Why this matters for Hacking
*   **Nmap:** Uses TCP flags (SYN, ACK, RST) to map networks. If you don't know TCP, you can't interpret Nmap.
*   **Wireshark:** Shows raw packets. You need to know the structure of an IP packet to read it.
*   **Pivoting:** Hacking deep into a network requires understanding routing and subnets.

---

## 📚 Resources
*   [Professor Messer's Network+ Course](https://www.professormesser.com/network-plus/n10-008/n10-008-video/n10-008-training-course/) (Free Video Training)
*   [Practical Packet Analysis (Book)](https://nostarch.com/packetanalysis3)