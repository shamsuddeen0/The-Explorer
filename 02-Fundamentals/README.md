# 🧱 The Fundamentals: Building the Foundation

**"You cannot hack what you do not understand."**

Before running complex exploit scripts or using automated tools, I realized I needed to understand how systems actually work. A hacker is simply someone who knows the rules of a system so well that they know how to bend them.

This directory contains my notes on the "Boring Stuff" that actually makes the "Cool Stuff" possible.

## 📂 Directory Structure

### 🐧 [Linux/](Linux/)
**"The Hacker's Operating System."**
*   **[Commands-Cheatsheet.md](Linux/Commands-Cheatsheet.md)**: The essential commands I use daily (`grep`, `awk`, `sed`).
*   **[File-Permissions.md](Linux/File-Permissions.md)**: Understanding `chmod`, `chown`, and the difference between user, group, and world.
*   **[User-Management.md](Linux/User-Management.md)**: How to create users and manage sudo rights (essential for Privilege Escalation).
*   **[Bash-Scripting.md](Linux/Bash-Scripting.md)**: Automating tasks so I don't have to type the same thing twice.

### 🌐 [Networking/](Networking/)
**"How computers talk to each other."**
*   **[Protocols.md](Networking/Protocols.md)**: Deep dives into TCP, UDP, HTTP, DNS, and SSH.
*   **[Subnetting-Guide.md](Networking/Subnetting-Guide.md)**: Making sense of CIDR notation (`/24`) and IP ranges.
*   **[Common-Ports.md](Networking/Common-Ports.md)**: A quick reference for the top 20 ports I scan for.

---

## 🧠 The "Must-Know" Checklist
*Before moving to the Offensive Security section, I ensured I could do the following:*

1.  **Linux:** Can I navigate the terminal without a mouse?
2.  **Permissions:** Do I understand why `chmod 777` is dangerous?
3.  **Networking:** Can I explain the 3-Way Handshake (SYN, SYN-ACK, ACK)?
4.  **IPs:** Do I know the difference between a Public IP and a Private IP (RFC1918)?

---

## 📚 External Learning Resources
*Where I learned these concepts:*
*   **[Linux Journey](https://linuxjourney.com/):** The best free site for learning Linux.
*   **[Professor Messer (Network+)](https://www.professormesser.com/):** Great video series on networking basics.
*   **[OverTheWire: Bandit](https://overthewire.org/wargames/bandit/):** A CTF game specifically designed to teach Linux commands.