# 🧪 The Laboratory: Safe Testing Grounds

**"Don't practice on production."**

This directory documents the infrastructure I use to learn. You cannot become a skilled security professional without a safe, isolated environment to launch attacks and analyze malware.

## 📄 Contents
1.  **[Build-Your-Own-Lab.md](Build-Your-Own-Lab.md)**
    *   The complete guide to setting up your attack machine (Kali) and targets (Metasploitable, Juice Shop).
    *   Covers VirtualBox, VMware, and Proxmox setups.

2.  **[Active-Directory-Setup.md](Active-Directory-Setup.md)** *(Coming Soon)*
    *   How to deploy GOAD (Game of Active Directory) to practice enterprise network attacks.

3.  **[Cloud-Lab-Setup.md](Cloud-Lab-Setup.md)** 
    *   Using Terraform to spin up vulnerable AWS/Azure instances for cloud pentesting.

---

### ⚠️ Safety Warning
The machines detailed in this folder are **intentionally vulnerable**.
*   **DO NOT** expose them to the public internet (use Host-Only or NAT Networks).
*   **DO NOT** store personal data on them.
*   If you are running malware analysis, ensure your network isolation is strictly configured.