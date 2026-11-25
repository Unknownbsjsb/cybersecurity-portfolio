
# Incident Response & System Hardening Case Study: Web Compromise & Brute-Force Remediation

**Analyst:** Nake Nsani Mesi Pius  
**Tools & Protocols:** `tcpdump` Packet Analysis, Sandbox Analysis, HTTP, DNS, Firewall Configuration, MFA  
**Incident Summary:** Investigated a compromised web application (`yummyrecipesforme.com`) serving malicious downloads and redirecting traffic over HTTP, followed by a system hardening plan to remediate admin brute-force vulnerabilities.

---

## 1. Incident Overview & Symptoms
Multiple end-users reported severe system slowdowns after visiting `yummyrecipesforme.com` and executing a prompted file disguised as a recipe/browser update. Simultaneously, the website administrator was locked out of the web server management account.

## 2. Sandbox & Packet Investigation (`tcpdump`)
To safely observe the behavior without exposing production infrastructure, the site was accessed inside an isolated sandbox while capturing packets with `tcpdump`:
* **Initial Connection:** The client resolved `yummyrecipesforme.com` via DNS and established an unencrypted **HTTP** session at the application layer.
* **Malicious Payload Delivery:** Upon interaction, the site prompted a file download transmitted over HTTP.
* **DNS Hijack / Traffic Redirection:** Immediately after executing the downloaded file, `tcpdump` logs captured an unauthorized DNS query for a rogue domain (`greatrecipesforme.com`), rerouting browser traffic to the attacker-controlled IP address.
* **Root Cause:** Source code review confirmed unauthorized code injection on the web server. Because the administrator was locked out, evidence indicated the attacker compromised a weak/default administrator password via a **brute-force attack** and altered the credentials.

## 3. Security Hardening & Remediation Plan
To contain the breach and prevent future unauthorized access, the following three hardening controls were recommended:
1. **Multifactor Authentication (MFA):** Enforce MFA (password + mobile OTP/authenticator token) on all administrative and server accounts so compromised credentials alone cannot grant access.
2. **Strict Password Policies & Rotation:** Disallow default or previously used passwords, enforce strong complexity rules, and require periodic credential updates to neutralize automated password-guessing scripts.
3. **Proactive Firewall Maintenance:** Audit and update perimeter firewall rules regularly to block traffic from unauthorized external IP addresses and restrict administrative ports.
