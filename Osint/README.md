# 🕵️ Open Source Intelligence (OSINT)

**"Hacking is 80% information gathering, 20% exploitation."**

OSINT is the art of finding information that is publicly available but hidden in plain sight. Before you ever touch a target system, you should want to know everything about it.

---

## 🛡️ The "Golden Rules" of OSINT (OpSec)
*Before you start searching, protect yourself.*
1.  **Don't Touch the Target:** OSINT is passive. Do not scan ports or try to log in unless you have permission.
2.  **Use "Sock Puppets":** Never investigate a target using your personal social media accounts. LinkedIn notifies people when you view their profile. Create a fake persona.
3.  **VPN Always:** Hide your IP address.
4.  **Sandbox It:** Open suspicious files or links in a Virtual Machine, not your main computer.

---

## 🔎 The OSINT Toolkit

### 1. 🏢 Hunting Infrastructure (Domains & IPs)
*Goal: Find every website, server, and technology stack the target owns.*

| Tool | Why I Use It | Link |
| :--- | :--- | :--- |
| **Shodan** | The "Search Engine for Hackers." Finds servers, webcams, and open ports. | [shodan.io](https://shodan.io) |
| **Censys** | Similar to Shodan, but excellent for finding SSL certificates and cloud hosts. | [censys.io](https://censys.io) |
| **crt.sh** | Searches Certificate Transparency logs. Finds subdomains that aren't even live yet. | [crt.sh](https://crt.sh) |
| **BuiltWith** | Tells you the exact technology stack of a website (CMS, Analytics, Frameworks). | [builtwith.com](https://builtwith.com) |
| **DNSDumpster** | Visualizes domain maps and finds related hosts. | [dnsdumpster.com](https://dnsdumpster.com) |

### 2. 👤 Hunting People (Social Engineering)
*Goal: Find valid usernames, emails, and connections.*

| Tool | Why I Use It | Link |
| :--- | :--- | :--- |
| **Sherlock** | Hunts down a specific username across 300+ social media sites instantly. | [GitHub](https://github.com/sherlock-project/sherlock) |
| **Hunter.io** | Finds the email naming convention for a company (e.g., `john.doe@corp.com`). | [hunter.io](https://hunter.io) |
| **Phonebook.cz** | Lists all domains and emails associated with a target domain. (Very powerful). | [phonebook.cz](https://phonebook.cz) |
| **theHarvester** | A CLI tool that scrapes Google, Bing, and LinkedIn for emails and subdomains. | [GitHub](https://github.com/laramies/theHarvester) |

### 3. 📸 Image Intelligence (IMINT)
*Goal: Extracting data from photos.*

*   **Reverse Image Search:** Don't just use Google.
    *   **[Yandex Images](https://yandex.com/images/):** Often the best for facial recognition and Russian/European sources.
    *   **[TinEye](https://tineye.com/):** Good for finding exact matches.
*   **Metadata (ExifTool):**
    Images often contain hidden data like GPS coordinates, Camera Model, and Software version.
    ```bash
    # Extract all metadata from an image
    exiftool image.jpg
    ```

### 4. 🌍 Geospatial Intelligence (GEOINT)
*Goal: Finding exactly where something is located.*

*   **[Google Earth Pro](https://www.google.com/earth/versions/):** The desktop version has a "Time Slider" to see satellite imagery from the past (useful to see when a building was built).
*   **[WiGLE.net](https://wigle.net/):** A map of WiFi networks. If you see a WiFi SSID in a photo, WiGLE can tell you exactly where in the world that router is located.
*   **[SunCalc](https://www.suncalc.org/):** Analyzing shadows in a photo to determine the time of day it was taken.

### 5. 🔓 Leak Hunting
*Goal: Checking if the target has been hacked before.*

*   **[HaveIBeenPwned](https://haveibeenpwned.com):** The standard for checking email breaches.
*   **[DeHashed](https://dehashed.com):** (Paid) Search engine for viewing actual leaked entries.
*   **[Wayback Machine](https://archive.org/web/):** View old versions of websites to find deleted contact info or old staff pages.

### 6. 🤖 Automation Frameworks
*Goal: Connecting all the dots.*

*   **[Maltego](https://www.maltego.com/):** The visual link analysis tool. Great for drawing maps of connections between people and servers.
*   **[SpiderFoot](https://github.com/smicallef/spiderfoot):** An automated OSINT collection tool. You give it a domain, and it queries 100+ sources automatically.
*   **[OSINT Framework](https://osintframework.com/):** A web-based interactive map of tools. If you are stuck, look here.

---

## 📚 Google Hacking (Dorks)
You don't always need tools. Sometimes you just need the right search query.

*   `site:target.com filetype:pdf` -> Find all PDF documents on their site.
*   `site:target.com "confidential"` -> Find pages containing sensitive words.
*   `intitle:"index of" "backup"` -> Find open directories containing backups.
*   **[Google Hacking Database (GHDB)](https://www.exploit-db.com/google-hacking-database):** The bible of search queries.

---

## 🏋️ Practice Resources
Where I practice my OSINT skills legally:
1.  **[Trace Labs](https://www.tracelabs.org/):** Using OSINT to help find real missing persons.
2.  **[OhSINT (TryHackMe)](https://tryhackme.com/room/ohsint):** A beginner CTF room for analyzing a photo.
3.  **[GeoGuessr](https://www.geoguessr.com/):** A game that trains your brain to recognize locations based on scenery.