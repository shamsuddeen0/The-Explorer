# 🎭 Social Engineering & Advanced OSINT

**"Amateurs hack systems. Professionals hack people."**

This section focuses on the human element of security. These are the specific tools I reach for when I need to connect a digital username to a real identity or test an organization's resilience against phishing.

---

### 🆔 I need to find a User...
*Connecting a digital persona to a real person.*

#### **Sherlock** `[CLI]` `[Python]`
> **The Scenario:** I have a username (e.g., `cool_hacker_99`) and I want to see where else this person exists on the internet.
> **Why I use it:** It is incredibly fast. It checks that specific username against 300+ social media platforms (Instagram, TikTok, Tinder, etc.) and gives me the direct links.
*   [View on GitHub](https://github.com/sherlock-project/sherlock)

#### **Maigret** `[CLI]` `[Advanced]`
> **The Scenario:** Sherlock didn't give me enough info, and I need a "Deep Dive."
> **Why I use it:** Maigret is like Sherlock on steroids. It doesn't just check if the account exists; it extracts metadata (names, locations, bios) from the profiles it finds to help build a dossier.
*   [View on GitHub](https://github.com/soxoj/maigret)

#### **WhatsMyName** `[WEB]` `[Quick]`
> **The Scenario:** I don't have access to my terminal or I need a quick check from my phone.
> **Why I use it:** A web-based alternative to Sherlock. Great for quick sanity checks.
*   [Visit Website](https://whatsmyname.app/)

---

### 📧 I need to find Emails & Leaks...
*Mapping the corporate attack surface.*

#### **theHarvester** `[CLI]` `[Recon]`
> **The Scenario:** I am starting a black-box test against a company and I need to find valid email addresses for employees.
> **Why I use it:** It scrapes public sources like LinkedIn, Bing, and PGP servers to generate a list of likely targets (e.g., `firstname.lastname@target.com`).
*   [View on GitHub](https://github.com/laramies/theHarvester)

#### **h8mail** `[CLI]` `[Breach-Data]`
> **The Scenario:** I found an email, but I want to know if their password has been leaked in the past.
> **Why I use it:** It connects to breach databases (like HaveIBeenPwned) to tell me *where* and *when* an email was compromised.
*   [View on GitHub](https://github.com/khast3x/h8mail)

#### **DeHashed** `[WEB]` `[Paid]`
> **The Scenario:** A client wants to know specifically *which* password was leaked so they can check for password reuse.
> **Why I use it:** Unlike HaveIBeenPwned (which just says "Yes/No"), DeHashed shows the actual leaked hash or plaintext. *Note: Use ethically.*
*   [Visit Website](https://dehashed.com/)

---

### 📱 I need to track a Phone Number...
*Validating contact numbers.*

#### **PhoneInfoga** `[CLI]` `[Scanner]`
> **The Scenario:** I found a phone number in a leaked document. I want to know the carrier, the location, and if it's a VoIP (fake) number.
> **Why I use it:** It scans international numbers for carrier details and searches the web for footprints of that number.
*   [View on GitHub](https://github.com/sundowndev/phoneinfoga)

#### **TrueCaller** `[WEB]` `[Identity]`
> **The Scenario:** I need to know the *name* of the person who owns the number.
> **Why I use it:** It has the world's largest database of phone owners. Great for identifying if a number belongs to a real person or a spam bot.
*   [Visit Website](https://www.truecaller.com/)

---

### 💻 I need to find Secrets in Code...
*Finding what developers forgot to hide.*

#### **GitHound** `[CLI]` `[GitHub]`
> **The Scenario:** A developer accidentally pushed an API key or a password to a public repository.
> **Why I use it:** It uses pattern matching to hunt for exposed secrets (AWS keys, Slack tokens) across GitHub that relate to my target.
*   [View on GitHub](https://github.com/tillson/git-hound)

#### **TruffleHog** `[CLI]` `[Deep-Scan]`
> **The Scenario:** The secret isn't in the current code, but maybe it was in a commit from 3 years ago?
> **Why I use it:** It digs through the entire *history* of a git repository (commits, branches) to find secrets that were "deleted" but are still in the history.
*   [View on GitHub](https://github.com/trufflesecurity/trufflehog)

---

### 📸 I need to analyze an Image (IMINT)...
*Finding location and details from a photo.*

#### **ExifTool** `[CLI]` `[Metadata]`
> **The Scenario:** I downloaded a photo from the target's blog. I want to see if it contains hidden GPS coordinates.
> **Why I use it:** It reads the metadata headers of a file. It can reveal the camera model, the software used to edit it, and sometimes the exact location.
*   [View on Website](https://exiftool.org/)

#### **Yandex Images** `[WEB]` `[Reverse-Search]`
> **The Scenario:** I have a profile picture of a person, but I don't know who they are.
> **Why I use it:** Google Images is good, but Yandex is *scary* good at facial recognition and finding matches on obscure social media sites.
*   [Visit Website](https://yandex.com/images/)

---

### 🎣 I need to simulate an Attack...
*Red Team Phishing & Cloning.*

#### **GoPhish** `[GUI]` `[Framework]`
> **The Scenario:** The client wants to know: "How many employees will click a fake link?"
> **Why I use it:** The industry standard. It gives me a dashboard to track who opened the email, who clicked the link, and who submitted data.
*   [View on GitHub](https://github.com/gophish/gophish)

#### **Zphisher** `[Bash]` `[Script]`
> **The Scenario:** I need to set up a quick Proof-of-Concept (POC) login page for a demonstration.
> **Why I use it:** A simple script that spins up clone login pages (Facebook, Google, Microsoft) in seconds. *Note: For educational/authorized use only.*
*   [View on GitHub](https://github.com/htr-tech/zphisher)