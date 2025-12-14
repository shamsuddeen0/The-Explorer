
# ☢️ Nuclei: The Fast & Customizable Scanner

**"If Nmap finds the doors, Nuclei checks if they are left unlocked."**

# ☢️ Nuclei: The Fast & Customizable Scanner

## 🧠 What is Nuclei?
Think of Nuclei as a **high-speed, automated checklist**. 

Unlike traditional scanners that try to guess everything, Nuclei uses specific "templates" (YAML files) to send exact requests to a server. If the server responds in a specific way (like showing a password or a specific error), Nuclei alerts you.

[Nuclei](https://github.com/projectdiscovery/nuclei) is hands down my favorite vulnerability scanner. Unlike traditional scanners (like Nessus) that are slow and bloated, Nuclei is:
1.  **Fast:** It sends requests based on simple templates and it can scan thousands of hosts in minutes 
2.  **Community Driven:** If a new vulnerability comes out (like Log4J), the community usually writes a Nuclei template for it within hours.
3.  **Customizable:** I can write my own checks in simple YAML.

---

## 🚀 Installation
Since I use Go often, I install it this way:
```bash
go install -v github.com/projectdiscovery/nuclei/v2/cmd/nuclei@latest

Don't forget to update templates regularly: nuclei -update-templates

🛠️ My Go-To Commands
1. The "Sanity Check" (Basic Scan)

When I first approach a target and want to see if there are any obvious low-hanging fruit.

code
Bash
download
content_copy
expand_less
nuclei -u https://target.com
2. Hunting for Specific CVEs

If I see a specific technology (like Apache Struts) and only want to check for high-severity CVEs.

code
Bash
download
content_copy
expand_less
nuclei -u https://target.com -t cves/ -severity critical,high
3. The "Bug Bounty" Scan (Exposure & Tokens)

When looking for API keys, env files, or exposed panels.

code
Bash
download
content_copy
expand_less
nuclei -u https://target.com -tags exposure,token,config
4. Scanning a List of Targets

I rarely scan one host. Usually, I pipe my subdomain list (from httpx or assetfinder) directly into Nuclei.

code
Bash
download
content_copy
expand_less
cat live_subdomains.txt | nuclei -t cves/ -o results.txt
5. Being "Gentle" (Rate Limiting)

If I am testing a sensitive production server or a fragile CTF box, I slow it down.

code
Bash
download
content_copy
expand_less
nuclei -u https://target.com -rate-limit 50 -concurrency 10
📝 Understanding Templates

Nuclei works by using YAML files called Templates.

Here is how I read a template to understand what it's doing. This example checks for a "Vulnerable Endpoint":

code
Yaml
download
content_copy
expand_less
id: example-vuln
info:
  name: Example Vulnerability
  severity: high

requests:
  - method: GET
    path:
      - "{{BaseURL}}/vulnerable-path"  # Where it looks
    
    matchers:
      - type: word
        words:
          - "root:x:0:0"  # What it looks for in the response

If Nuclei sees root:x:0:0 in the response body, it alerts me.

💡 Pro-Tips for Using Nuclei

Don't run blindly: Running all templates takes a long time and is very noisy. Filter by tags!

Update often: Security moves fast. Run nuclei -ut before every engagement.

Chain it: I almost always use Nuclei in a chain:
subfinder -d target.com | httpx | nuclei -t cves/

📚 Resources

Official ProjectDiscovery GitHub

Nuclei Templates List

code
Code
download
content_copy
expand_less
### Why this works for you:
1.  **The "Sanity Check":** It uses language that implies you are *doing* the work ("When I first approach a target...").
2.  **Simplified YAML:** Instead of a complex Log4J example, I wrote a pseudo-code example that explains the *logic* (Look at URL -> Find this Word -> Alert) so beginners can understand it instantly.
3.  **Pro-Tips:** This adds value that official documentation doesn't have. It shows you have experience with the tool.