# 📋 The OSINT Workflow Checklist

**"How to find the needle in the haystack."**

This is the exact process you should follow when you are given a domain name (e.g., `tesla.com`) and told to find vulnerabilities.

---

## 🟢 Phase 1: The Broad Sweep (Who are they?)
*Goal: Understand the business, acquisitions, and physical locations.*

- [ ] **Crunchbase / Wikipedia:** Who owns them? Do they have subsidiaries?
- [ ] **ASN Lookup (Hurricane Electric):** What IP ranges do they own? (`bgp.he.net`)
- [ ] **Google Dorks:**
    - `site:target.com` (Index pages)
    - `site:target.com filetype:pdf` (Leaked documents)
    - `site:target.com intitle:"index of"` (Open directories)

---

## 🟡 Phase 2: Technical Infrastructure (What do they own?)
*Goal: Find the servers.*

- [ ] **DNS Enumeration:**
    - `A`, `MX`, `TXT` records.
    - Check SPF records (can I spoof their email?).
- [ ] **Subdomain Discovery:**
    - Run `Subfinder` and `Amass`.
    - Check `crt.sh` for SSL certificates.
- [ ] **Cloud Recon:**
    - Look for open S3 buckets (`grayhatwarfare.com`).
    - Check GitHub for leaked API keys (`trufflehog`).

---

## 🔴 Phase 3: The Human Element (Who works there?)
*Goal: Find potential entry points via phishing or weak passwords.*

- [ ] **LinkedIn:** Harvest employee names (Potential usernames: `j.doe`, `john.d`).
- [ ] **Breach Data:** Check `HaveIBeenPwned` or `DeHashed` for leaked passwords.
- [ ] **Social Media:** Use `Sherlock` to find developer accounts.

---

## 🏁 The Output (My Deliverable)
By the end of this phase, I should have:
1.  A list of **IP Addresses** (The scope).
2.  A list of **Subdomains** (The attack surface).
3.  A list of **Emails** (The phishing targets).
4.  A list of **Technologies** (The exploit search terms).