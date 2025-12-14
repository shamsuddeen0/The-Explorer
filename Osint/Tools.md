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




### **MORE MORE TOOLS**

# Osint Tools

This is a curated list of tools for this category.

---

- [10Minutemail - Python Temporary Email](http://feedproxy.google.com/~r/PentestTools/~3/6P5wkV_3yTU/10minutemail-python-temporary-email.html)
- [AIL Framework - Framework for Analysis of Information Leaks](http://feedproxy.google.com/~r/PentestTools/~3/91FEC7M0yz8/ail-framework-framework-for-analysis-of.html)
- [AdvPhishing - This Is Advance Phishing Tool! OTP PHISHING](http://feedproxy.google.com/~r/PentestTools/~3/9rL0P-wabG0/advphishing-this-is-advance-phishing.html)
- [BaseQuery - A Way To Organize Public Combo-Lists And Leaks In A Way That You Can Easily Search Through Everything](http://feedproxy.google.com/~r/PentestTools/~3/xagTe4W9uT4/basequery-way-to-organize-public-combo.html)
- [Brute_Force - BruteForce Gmail, Hotmail, Twitter, Facebook & Netflix](http://feedproxy.google.com/~r/PentestTools/~3/Bovu29IujOM/bruteforce-bruteforce-gmail-hotmail.html)
- [Buster - Find Emails Of A Person And Return Info Associated With Them](http://feedproxy.google.com/~r/PentestTools/~3/y2mAo4j8218/buster-find-emails-of-person-and-return.html)
- [Combobulator - Framework To Detect And Prevent Dependency Confusion Leakage And Potential Attacks](http://www.kitploit.com/2022/01/combobulator-framework-to-detect-and.html)
- [CredsLeaker v3 - Tool to Display A Powershell Credentials Box](http://feedproxy.google.com/~r/PentestTools/~3/9y08bFtnHNg/credsleaker-v3-tool-to-display.html)
- [DataSurgeon - Quickly Extracts IP's, Email Addresses, Hashes, Files, Credit Cards, Social Secuirty Numbers And More From Text](http://www.kitploit.com/2023/03/datasurgeon-quickly-extracts-ips-email.html)
- [EMAGNET - Tool For Find Leaked Databases With 97.1% Accurate To Grab Mail + Password Together From Pastebin Leaks](http://feedproxy.google.com/~r/PentestTools/~3/YIAfk2yhMRY/emagnet-tool-for-find-leaked-databases.html)
- [Email-Prediction-Asterisks - Script That Allows You To Identify The Emails Hidden Behind Asterisks](http://www.kitploit.com/2022/05/email-prediction-asterisks-script-that.html)
- [Email-Vulnerablity-Checker - Find Email Spoofing Vulnerablity Of Domains](http://www.kitploit.com/2023/02/email-vulnerablity-checker-find-email.html)
- [Espoofer - An Email Spoofing Testing Tool That Aims To Bypass SPF/DKIM/DMARC And Forge DKIM Signatures](http://www.kitploit.com/2022/01/espoofer-email-spoofing-testing-tool.html)
- [EvilnoVNC - Ready To Go Phishing Platform](http://www.kitploit.com/2022/10/evilnovnc-ready-to-go-phishing-platform.html)
- [Facebash - Facebook Brute Forcer In Shellscript Using TOR](http://feedproxy.google.com/~r/PentestTools/~3/f3cso_9atWo/facebash-facebook-brute-forcer-in.html)
- [Fbchecker - Facebook Mass Account Checker](http://feedproxy.google.com/~r/PentestTools/~3/PeOX84N6efU/fbchecker-facebook-mass-account-checker.html)
- [FisherMan - CLI Program That Collects Information From Facebook User Profiles Via Selenium](http://feedproxy.google.com/~r/PentestTools/~3/nUJjKv3m7dw/fisherman-cli-program-that-collects.html)
- [Formphish - Auto Phishing Form-Based Websites](http://feedproxy.google.com/~r/PentestTools/~3/KpG4QCw9F6s/formphish-auto-phishing-form-based.html)
- [GitMonitor - A Github Scanning System To Look For Leaked Sensitive Information Based On Rules](http://feedproxy.google.com/~r/PentestTools/~3/icGYa6lk_F4/gitmonitor-github-scanning-system-to.html)
- [Gitjacker - Leak Git Repositories From Misconfigured Websites](http://feedproxy.google.com/~r/PentestTools/~3/9wKv0oGAU6g/gitjacker-leak-git-repositories-from.html)
- [Gophish - Open-Source Phishing Toolkit](http://feedproxy.google.com/~r/PentestTools/~3/btpn4JOATyY/gophish-open-source-phishing-toolkit.html)
- [HashCheck - Tool To Assist In The Search For Leaked Passwords](http://feedproxy.google.com/~r/PentestTools/~3/IKIzL1W4yTE/hashcheck-tool-to-assist-in-search-for.html)
- [ISeeYou - Bash And Javascript Tool To Find The Exact Location Of The Users During Social Engineering Or Phishing Engagements](http://feedproxy.google.com/~r/PentestTools/~3/kBe1Xfh9iTc/iseeyou-bash-and-javascript-tool-to.html)
- [Inshackle - Instagram Hacks: Track Unfollowers, Increase Your Followers, Download Stories, Etc](http://feedproxy.google.com/~r/PentestTools/~3/hUu-VErDuek/inshackle-instagram-hacks-track.html)
- [InstaSave - Python Script To Download Images, Videos & Profile Pictures From Instagram](http://feedproxy.google.com/~r/PentestTools/~3/MkEScdqkcss/instasave-python-script-to-download.html)
- [Instainsane - Multi-threaded Instagram Brute Forcer](http://feedproxy.google.com/~r/PentestTools/~3/n2J8ihzAvxI/instainsane-multi-threaded-instagram.html)
- [Jeopardize - A Low(Zero) Cost Threat Intelligence & Response Tool Against Phishing Domains](http://feedproxy.google.com/~r/PentestTools/~3/1OfTItxHps8/jeopardize-lowzero-cost-threat.html)
- [Jsleak - A Go Code To Detect Leaks In JS Files Via Regex Patterns](http://feedproxy.google.com/~r/PentestTools/~3/nMMZ9f_sz-g/jsleak-go-code-to-detect-leaks-in-js.html)
- [Karma - Search of Emails and Passwords on Pwndb](http://feedproxy.google.com/~r/PentestTools/~3/Z2_HhIVSkSU/karma-search-of-emails-and-passwords-on.html)
- [Keyhacks - A Repository Which Shows Quick Ways In Which API Keys Leaked By A Bug Bounty Program Can Be Checked To See If They'Re Valid](http://feedproxy.google.com/~r/PentestTools/~3/XNN85kEDGgM/keyhacks-repository-which-shows-quick.html)
- [Kit_Hunter - A Basic Phishing Kit Scanner For Dedicated And Semi-Dedicated Hosting](http://www.kitploit.com/2021/11/kithunter-basic-phishing-kit-scanner.html)
- [LeakedHandlesFinder - Leaked Windows Processes Handles Identification Tool](http://www.kitploit.com/2022/05/leakedhandlesfinder-leaked-windows.html)
- [Leaktopus - Keep Your Source Code Under Control](http://www.kitploit.com/2023/02/leaktopus-keep-your-source-code-under.html)
- [LinkedInDumper - Tool To Dump Company Employees From LinkedIn API](http://www.kitploit.com/2023/06/linkedindumper-tool-to-dump-company.html)
- [Mail-Swipe - Script To Create Temporary Email Addresses And Receive Emails](http://feedproxy.google.com/~r/PentestTools/~3/OZ0PzLUJC0Y/mail-swipe-script-to-create-temporary.html)
- [Maltego CE - An Interactive Data Mining Tool That Renders Directed Graphs For Link Analysis](http://feedproxy.google.com/~r/PentestTools/~3/up3tM_gz8JE/maltego-ce-interactive-data-mining-tool.html)
- [MaskPhish - Give A Mask To Phishing URL](http://feedproxy.google.com/~r/PentestTools/~3/C2ylG7pQvrQ/maskphish-give-mask-to-phishing-url.html)
- [Mip22 - An Advanced Phishing Tool](http://www.kitploit.com/2022/03/mip22-advanced-phishing-tool.html)
- [Miteru - An Experimental Phishing Kit Detection Tool](http://feedproxy.google.com/~r/PentestTools/~3/T974-FHaask/miteru-experimental-phishing-kit.html)
- [Modlishka - An Open Source Phishing Tool With 2FA Authentication](http://feedproxy.google.com/~r/PentestTools/~3/Z2CV9SS3UmA/modlishka-open-source-phishing-tool.html)
- [Nexphisher - Advanced Phishing Tool For Linux & Termux](http://feedproxy.google.com/~r/PentestTools/~3/8La5H1VOOps/nexphisher-advanced-phishing-tool-for.html)
- [O365Enum - Enumerate Valid Usernames From Office 365 Using ActiveSync, Autodiscover V1, Or Office.Com Login Page](http://feedproxy.google.com/~r/PentestTools/~3/2PdAA_3kJRQ/o365enum-enumerate-valid-usernames-from.html)
- [OSIF - Open Source Information Facebook](http://feedproxy.google.com/~r/PentestTools/~3/kYJPsFZc8UQ/osif-open-source-information-facebook.html)
- [PasteMonitor - Scrape Pastebin API To Collect Daily Pastes, Setup A Wordlist And Be Alerted By Email When You Have A Match](http://www.kitploit.com/2022/01/pastemonitor-scrape-pastebin-api-to.html)
- [Pepe - Collect Information About Email Addresses From Pastebin](http://feedproxy.google.com/~r/PentestTools/~3/UWHcybSkf3A/pepe-collect-information-about-email.html)
- [Phishing-Simulation - Aims To Increase Phishing Awareness By Providing An Intuitive Tutorial And Customized Assessment](http://feedproxy.google.com/~r/PentestTools/~3/-hbGrpX44oM/phishing-simulation-aims-to-increase.html)
- [PhishingKitTracker - Let's Track Phishing Kits To Give To Research Community Raw Material To Stud](http://feedproxy.google.com/~r/PentestTools/~3/tYI46yw5MEg/phishingkittracker-lets-track-phishing.html)
- [Pickl3 - Windows Active User Credential Phishing Tool](http://feedproxy.google.com/~r/PentestTools/~3/_iEA0MZdCwY/pickl3-windows-active-user-credential.html)
- [Pmanager - Store And Retrieve Your Passwords From A Secure Offline Database. Check If Your Passwords Has Leaked Previously To Prevent Targeted Password Reuse Attacks](http://www.kitploit.com/2022/09/pmanager-store-and-retrieve-your.html)
- [Project iKy - Tool That Collects Information From An Email And Shows Results In A Nice Visual Interface](http://feedproxy.google.com/~r/PentestTools/~3/Z24DwjUEpe4/project-iky-tool-that-collects.html)
- [Project iKy - Tool That Collects Information From An Email And Shows Results In A Nice Visual Interface](http://feedproxy.google.com/~r/PentestTools/~3/M4KiPTUKSVo/project-iky-tool-that-collects.html)
- [Project iKy v2.0.0 - Tool That Collects Information From An Email And Shows Results In A Nice Visual Interface](http://feedproxy.google.com/~r/PentestTools/~3/1W_lCE0_ys4/project-iky-v200-tool-that-collects.html)
- [Project iKy v2.1.0 - Tool That Collects Information From An Email And Shows Results In A Nice Visual Interface](http://feedproxy.google.com/~r/PentestTools/~3/4hKlInqj0IM/project-iky-v210-tool-that-collects.html)
- [Project iKy v2.2.0 - Tool That Collects Information From An Email And Shows Results In A Nice Visual Interface](http://feedproxy.google.com/~r/PentestTools/~3/UUzvzCFnYJE/project-iky-v220-tool-that-collects.html)
- [Project iKy v2.4.0 - Tool That Collects Information From An Email And Shows Results In A Nice Visual Interface](http://feedproxy.google.com/~r/PentestTools/~3/gp-sptDrrHc/project-iky-v240-tool-that-collects.html)
- [Project iKy v2.5.0 - Tool That Collects Information From An Email And Shows Results In A Nice Visual Interface](http://feedproxy.google.com/~r/PentestTools/~3/7hKPbEXH2Wo/project-iky-v250-tool-that-collects.html)
- [Project iKy v2.6.0 - Tool That Collects Information From An Email And Shows Results In A Nice Visual Interface](http://feedproxy.google.com/~r/PentestTools/~3/oiunjpsRvSc/project-iky-v260-tool-that-collects.html)
- [Project iKy v2.7.0 - Tool That Collects Information From An Email And Shows Results In A Nice Visual Interface](http://feedproxy.google.com/~r/PentestTools/~3/ndDXcQpIHDY/project-iky-v270-tool-that-collects.html)
- [Pwndb - Search For Creadentials Leaked On Pwndb](http://feedproxy.google.com/~r/PentestTools/~3/StIgYaSXjQ8/pwndb-search-for-creadentials-leaked-on.html)
- [Pwned - Simple CLI Script To Check If You Have A Password That Has Been Compromised In A Data Breach](http://feedproxy.google.com/~r/PentestTools/~3/HrSDHu1CbE0/pwned-simple-cli-script-to-check-if-you.html)
- [Scavenger - Crawler Searching For Credential Leaks On Different Paste Sites](http://feedproxy.google.com/~r/PentestTools/~3/TibOQ-WmQVE/scavenger-crawler-searching-for.html)
- [Shellphish - Phishing Tool For 18 Social Media (Instagram, Facebook, Snapchat, Github, Twitter...)](http://feedproxy.google.com/~r/PentestTools/~3/5hBi829B8IU/shellphish-phishing-tool-for-18-social.html)
- [SniperPhish - The Web-Email Spear Phishing Toolkit](http://feedproxy.google.com/~r/PentestTools/~3/5EqZRCDX6vA/sniperphish-web-email-spear-phishing.html)
- [Social-Analyzer - API And Web App For Analyzing And Finding A Person Profile Across +300 Social Media Websites (Detections Are Updated Regularly)](http://feedproxy.google.com/~r/PentestTools/~3/kXzr1LAzNS0/social-analyzer-api-and-web-app-for.html)
- [Socialhunter - Crawls The Website And Finds Broken Social Media Links That Can Be Hijacked](http://www.kitploit.com/2022/06/socialhunter-crawls-website-and-finds.html)
- [Socialscan - Check Email Address And Username Availability On Online Platforms With 100% Accuracy](http://feedproxy.google.com/~r/PentestTools/~3/yHydtjSLSqU/socialscan-check-email-address-and.html)
- [Solr-GRAB - Steal Apache Solr Instance Queries With Or Without A Username And Password](http://feedproxy.google.com/~r/PentestTools/~3/BS31nUP4BmE/solr-grab-steal-apache-solr-instance.html)
- [Spamscanner - Spam Scanner Is The Best Anti-Spam, Email Filtering, And Phishing Prevention Service](http://www.kitploit.com/2021/12/spamscanner-spam-scanner-is-best-anti.html)
- [Squarephish - An advanced phishing tool that uses a technique combining the OAuth Device code authentication flow and QR codes](http://www.kitploit.com/2022/12/squarephish-advanced-phishing-tool-that.html)
- [SteaLinG - Open-Source Penetration Testing Framework Designed For Social Engineering](http://www.kitploit.com/2022/10/stealing-open-source-penetration.html)
- [SwiftyInsta - Instagram Unofficial Private API Swift](http://feedproxy.google.com/~r/PentestTools/~3/AjmuVpxXbjo/swiftyinsta-instagram-unofficial.html)
- [Tempomail - Generate A Custom Email Address In 1 Second And Receive Emails](http://feedproxy.google.com/~r/PentestTools/~3/Bkhk6dBTp6U/tempomail-generate-custom-email-address.html)
- [Toutatis - A Tool That Allows You To Extract Information From Instagrams Accounts Such As E-Mails, Phone Numbers And More](http://www.kitploit.com/2021/12/toutatis-tool-that-allows-you-to.html)
- [Tracgram - Use Instagram Location Features To Track An Account](http://www.kitploit.com/2023/02/tracgram-use-instagram-location.html)
- [Tweetshell - Multi-thread Twitter BruteForcer In Shell Script](http://feedproxy.google.com/~r/PentestTools/~3/vWpgJ70dlTM/tweetshell-multi-thread-twitter.html)
- [Twifo-Cli - Get User Information Of A Twitter User](http://feedproxy.google.com/~r/PentestTools/~3/Sbc3gunRkBE/twifo-cli-get-user-information-of.html)
- [TwitWork - Monitor Twitter Stream](http://feedproxy.google.com/~r/PentestTools/~3/b-cPMo5l19E/twitwork-monitor-twitter-stream.html)
- [TwitterShadowBan - Twitter Shadowban Tests](http://feedproxy.google.com/~r/PentestTools/~3/xIWKkM5Hleo/twittershadowban-twitter-shadowban-tests.html)
- [URLCADIZ - A Simple Script To Generate A Hidden Url For Social Engineering](http://feedproxy.google.com/~r/PentestTools/~3/61lQnh22cpM/urlcadiz-simple-script-to-generate.html)
- [URLCrazy - Generate And Test Domain Typos And Variations To Detect And Perform Typo Squatting, URL Hijacking, Phishing, And Corporate Espionage](http://feedproxy.google.com/~r/PentestTools/~3/pCTRDE2dl1M/urlcrazy-generate-and-test-domain-typos.html)
- [Unfollow-Plus - Automated Instagram Unfollower Bot](http://feedproxy.google.com/~r/PentestTools/~3/V_Pju0doVxo/unfollow-plus-automated-instagram.html)
- [Userrecon-Py v2.0 - Username Recognition On Various Websites](http://feedproxy.google.com/~r/PentestTools/~3/c7uPNvH8iLk/userrecon-py-v20-username-recognition.html)
- [Vthunting - A Tiny Script Used To Generate Report About VirusTotal Hunting And Send It By Email, Slack Or Telegram](http://feedproxy.google.com/~r/PentestTools/~3/oKh1run6pi8/vthunting-tiny-script-used-to-generate.html)
- [XLL_Phishing - XLL Phishing Tradecraft](http://www.kitploit.com/2022/09/xllphishing-xll-phishing-tradecraft.html)
- [Zphisher - Automated Phishing Tool](http://feedproxy.google.com/~r/PentestTools/~3/j5xeLa9VQ88/zphisher-automated-phishing-tool.html)
- [cve_manager_VS - A Collection Of Python Apps And Shell Scripts To Email An Xlsx Spreadsheet Of New Vulnerabilities In The NIST CVE Database And Their Associated Products On A Daily Schedule](http://feedproxy.google.com/~r/PentestTools/~3/AW1ePPa2tPE/cvemanagervs-collection-of-python-apps.html)
- [gitGraber - Tool To Monitor GitHub To Search And Find Sensitive Data For Different Online Services Such As: Google, Amazon, Paypal, Github, Mailgun, Facebook, Twitter, Heroku, Stripe...](http://feedproxy.google.com/~r/PentestTools/~3/j4Ms9uZ-OTY/gitgraber-tool-to-monitor-github-to.html)
- [iCULeak - Tool To Find And Extract Credentials From Phone Configuration Files Hosted On Cisco CUCM](http://feedproxy.google.com/~r/PentestTools/~3/1QY0SIyYtbU/iculeak-tool-to-find-and-extract.html)
- [openSquat - Detection Of Phishing Domains And Domain Squatting. Supports Permutations Such As Homograph Attack, Typosquatting And Bitsquatting](http://www.kitploit.com/2022/02/opensquat-detection-of-phishing-domains.html)
- [paradoxiaRAT - Native Windows Remote Access Tool](http://feedproxy.google.com/~r/PentestTools/~3/bqljBWuxsdw/paradoxiarat-native-windows-remote.html)
- [pyWhat - Identify Anything. Easily Lets You Identify Emails, IP Addresses, And More...](http://feedproxy.google.com/~r/PentestTools/~3/jOygJhiVqds/pywhat-identify-anything-easily-lets.html)
- [ripVT - Virus Total API Maltego Transform Set For Canari](http://feedproxy.google.com/~r/PentestTools/~3/n4rLmMXJVa4/ripvt-virus-total-api-maltego-transform.html)