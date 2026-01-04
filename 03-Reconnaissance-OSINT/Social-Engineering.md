
This file focuses on the Human side of reconnaissance: hunting down usernames, emails, and connecting digital identities.

# 🎭 Social Engineering & Identity Hunting

**"Amateurs hack systems. Professionals hack people."**

This section focuses on finding the people behind the keyboards.

---

## 🆔 Finding Users (Username Enumeration)
*Goal: Connect a username (`CoolHacker99`) to a real identity.*

### **Sherlock**
The standard tool. Checks a username against 300+ sites.
```bash
python3 sherlock.py username
```
### Maigret

A more advanced version of Sherlock that extracts metadata (bios, locations).

```bash 
maigret username --pdf
```
## WhatsMyName (Web)

A fast web-based alternative if I can't use the CLI.

whatsmyname.app

## 📧 Finding Emails (Corporate Recon)

Goal: Build a list of valid emails for phishing or password spraying.

theHarvester

Scrapes Google, Bing, LinkedIn, and PGP servers.

```bash
theHarvester -d target.com -b google,linkedin
```
### Hunter.io

(Web) Finds the email naming convention (e.g., `first.last@company.com`) and verifies if emails are valid.

Phonebook.cz

(Web) An incredibly powerful search engine for domains and emails.

## 🔓 Breach Data (The "Lazy" Hack)

Why hack a password when it was leaked 3 years ago?

1. HaveIBeenPwned: Check if an email was compromised.

2. DeHashed: (Paid) View the actual cleartext passwords and hashes.

3. IntelX: Search for leaks in paste sites (Pastebin).

## 📸 Image Intelligence (IMINT)

Analyzing photos for location data.

### ExifTool

Extracts hidden metadata (GPS, Camera Model, Time).

```bash
exiftool image.jpg
```
### Reverse Image Search

Yandex Images: The best for facial recognition and obscure backgrounds.

Google Lens: Good for identifying landmarks and products.

```