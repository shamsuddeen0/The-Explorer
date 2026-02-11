# 🧪 Build Your Own Lab

**"You can't learn to defend if you don't know how to attack. And you can't attack if you don't have a safe place to practice."**

This document outlines the hardware, software, and network configurations I use to practice penetration testing safely. It is designed to be a roadmap: start small, and upgrade as you learn.

---

## 🐧 Step 1: The Attack Machine (Your Daily Driver)
*You need an Operating System pre-loaded with hacking tools. This is where the attacks come from.*

| Distro | Why choose this? | Link |
| :--- | :--- | :--- |
| **Kali Linux** | The industry standard. If you are following a tutorial online, they are probably using Kali. I use this as my main VM. | [Download](https://www.kali.org/get-kali/) |
| **Parrot OS** | A lighter, more privacy-focused alternative to Kali. It feels more like a standard desktop OS. | [Download](https://www.parrotsec.org/download/) |
| **Commando VM** | If you prefer Windows, this script turns a standard Windows VM into a full-fledged hacking machine (great for AD attacks). | [GitHub](https://github.com/mandiant/commando-vm) |

---

## 🎯 Step 2: The Targets (The Victims)
*These are the punching bags. **NEVER** expose these to the public internet.*

### 1. The Classics (Network Hacking)
*   **Metasploitable 2:** The "Hello World" of vulnerable machines. It has open ports everywhere (FTP, Telnet, SSH).
    *   [Download Link](https://sourceforge.net/projects/metasploitable/)
*   **Metasploitable 3:** The Windows version. Harder to set up, but essential for learning Windows exploitation.

### 2. Modern Web Apps (Bug Bounty Practice)
*   **OWASP Juice Shop:** The most advanced vulnerable web app. It handles SQL Injection, XSS, and modern vulnerabilities.
    *   *Run via Docker:* `docker run --rm -p 3000:3000 bkimminich/juice-shop`
*   **DVWA (Damn Vulnerable Web App):** Great for beginners because you can set the difficulty level (Low/Medium/High).

### 3. The Enterprise Network (Active Directory)
*   **GOAD (Game of Active Directory):** If you want to get a job in pentesting, you **must** learn Active Directory. This lab spins up a vulnerable corporate network with Domain Controllers and Servers.
    *   [GOAD GitHub](https://github.com/Orange-Cyberdefense/GOAD)

---

## 🏗️ Step 3: Choose Your Architecture

### Level 1: The "Laptop" Setup (Beginner)
*Perfect for starting out. Free and easy.*
*   **Hypervisor:** VirtualBox or VMware Workstation Player.
*   **Network Config:** **NAT Network**.
    *   *Why?* It creates a virtual router inside your laptop. Your VMs can talk to each other, but your home WiFi cannot access them (safety).

### Level 2: The "Home Server" Setup (Intermediate)
*For when your laptop runs out of RAM.*
*   **Hardware:** An old Desktop PC, Intel NUC, or used Enterprise Server.
*   **OS:** **Proxmox VE**.
*   **Why?** Proxmox allows you to run 20+ VMs at once and access them via a web browser from anywhere in your house.
*   **Automation:** Use **[Ludus](https://ludus.cloud/)** to automatically deploy complex ranges onto Proxmox.

### Level 3: The Cloud Lab (Advanced)
*For practicing AWS/Azure attacks.*
*   **Tools:** Terraform & Ansible.
*   **Resources:**
    *   **CloudGoat:** Vulnerable AWS scenarios by Rhino Security Labs.
    *   **SadCloud:** Spins up insecure AWS infrastructure to practice auditing.

---

## 🛡️ Step 4: Operational Security (OpSec)
*Distributions for anonymity and privacy.*

*   **Tails:** The "Amnesic" OS. It runs from a USB stick and forgets everything the moment you unplug it. Forces all traffic through Tor.
*   **Whonix:** The ultimate privacy setup. It uses two VMs (Gateway & Workstation) to mathematically ensure your IP address never leaks.

---

## 📚 Reference Guides
coming soon!!!