# 🕸️ Maltego: The Digital Detective Board

## 🧠 What is Maltego?
**Think of those "conspiracy walls" in detective movies**—the ones with photos, maps, and notes connected by red string. Maltego is the digital version of that.

It is a **link analysis tool** that visualizes relationships. Instead of looking at a boring list of IP addresses or names, Maltego draws a map showing how they are all connected.
- **The Node:** A person, a website, an email, or an IP (called an "Entity").
- **The Red String:** The relationship between them.
- **The Magic:** It automates the searching for you using "Transforms."

---

## 🛠️ Key Concepts
Before opening the tool, here is the vocabulary I needed to learn:

1.  **The Graph:** The big blank canvas where you work.
2.  **Entities:** The icons you drag onto the graph (e.g., a specific Domain, Person, or Phonenumber).
3.  **Transforms:** These are the scripts you run on an Entity.
    *   *Example:* I right-click a "Domain" entity and run the "To IP Address" transform. Maltego queries a DNS server and draws the IP for me automatically.

---

## 🚀 How I Use Maltego (My Workflow)
I primarily use Maltego during the **OSINT (Passive Recon)** phase to visualize the attack surface.

### Step 1: Start with what you know
I drag a **Domain** entity onto the graph and change the name to the target (e.g., `tesla.com`).

### Step 2: Map the Infrastructure (The "Machines")
*   Right-click the Domain.
*   Run Transform: **"To DNS Name [Robtex]"** or **"To IP Address."**
*   *Result:* Now I see the mail servers, web servers, and the IP addresses they sit on.

### Step 3: Find the People (The "Humans")
*   I click on the main Domain again.
*   Run Transform: **"To Email Addresses [Search Engine]."**
*   *Result:* I now have a list of emails associated with that company.

### Step 4: Connect the dots
*   I pick a specific email address found in Step 3.
*   Run Transform: **"To Social Networks."**
*   *Result:* Maltego checks if that email is registered on Twitter, LinkedIn, etc.

---

## 🔌 The "Transform Hub"
Maltego is only as good as the data you feed it. The "Hub" is like an App Store for data.
*   **Standard Transforms (Free):** Comes installed. Good for basic DNS and Whois.
*   **VirusTotal (API Key needed):** Essential for checking if an IP or Hash is malicious.
*   **Shodan:** Connects Shodan results directly to your graph (great for finding open ports on an IP).

---

## 💡 Pros & Cons (My Experience)

| 👍 The Good | 👎 The Bad |
| :--- | :--- |
| **Visuals:** It looks amazing in a report. Clients love seeing the "map" of their network. | **Rabbit Holes:** It is very easy to get overwhelmed with too much data. You have to know when to stop. |
| **Automation:** It does 10 manual Google searches in 1 click. | **Cost:** The Pro version is expensive. The Community Edition (CE) is free but limits the number of results you can see. |

---

## 📚 Resources
*   **[Official Maltego Documentation](https://docs.maltego.com/)**
*   **Note:** Maltego comes pre-installed on Kali Linux, but you must register for a free "Community Account" on their website to use it.