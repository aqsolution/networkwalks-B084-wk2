# PENETRATION TESTING REPORT

### Footprinting & Network Scanning Phases

**W2-PM-FINAL | CYBERSECURITY | NETWORKWALKS**

| Field | Detail |
| :--- | :--- |
| **Pentester Name (Cybersecurity Professional)** | Ali ur Rehman Qureshi |
| **Program/Batch** | B084-Networkwalks |
| **Date** | 12th Nov 2026 |
| **Modules completed** | W2-PM1 (Multiple Kali Tools)<br>W2-PM5 (Zenmap Scanning) |
| **Client/Target** | 1. Networkwalks (secured written permission already)<br>2. My own local LAN Network |
| **Permission secured from client?** | Yes |
| **Phases covered** | Phase 1: Reconnaissance & Footprinting<br>Phase 2: Scanning & Network Discovery<br>Phase 3-5: In Progress |

---

## 1. Liability Disclaimer
The activities detailed in this document were performed exclusively on authorized systems with prior written consent or on personal lab equipment. All content is provided strictly for educational and research purposes. The author accepts no liability for any misuse of these materials. You are solely responsible for ensuring your actions comply with the law; unauthorized system access is a criminal offense in most countries, regardless of intent or damage, and can lead to fines, job loss, and a permanent criminal record.

---

## 2. Introduction
As part of Week 2 of my Networkwalks internship, this report walks through two practical exercises: gathering public information (footprinting) on `networkwalks.com` using Kali Linux (W2-PM1), and mapping active devices on my local network with Zenmap on Windows (W2-PM5). Together, these tasks show the step-by-step process of moving from passive research to active network discovery. Throughout the report, each step includes the exact command I ran, the results I found, screenshot proof, and a quick summary of what those findings mean from a security perspective.

---

## 3. Tools Used
The table below lists each tool used in this report and its purpose.

| Tool | Purpose |
| :--- | :--- |
| **Kali Linux & Windows** | Operating systems used for reconnaissance activities |
| **WHOIS** | Find domain registration details (owner, dates, name servers) |
| **WhatWeb** | Fingerprint web technologies (server, CMS, plugins, IP) |
| **Nslookup** | Resolve the domain name to its IP address using DNS |
| **Curl (-I)** | Read the HTTP response headers of the website |
| **Wafw00f** | Detect whether a Web Application Firewall protects the site |
| **DNSRecon** | Enumerate all DNS records (NS, MX, SPF, TXT, SRV) |
| **Zenmap (Nmap GUI)** | Scan the local subnet to find live hosts, IPs, and MAC addresses |
| **Windows CMD** | Local IP and MAC address identification |

---

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance
To gather public information about `networkwalks.com`, I ran six different tools in Kali Linux, with each one revealing a specific piece of the website's setup:

* **WHOIS:** I checked the domain’s public registration records to find its name servers and basic hosting details.
* **WhatWeb:** I scanned the site to see what software it runs on, which revealed it is using **WordPress 7.0.4** and **WP Download Manager 3.3.58**.
* **Nslookup:** I looked up the domain's actual IP address, which resolved to **192.232.216.135**.
* **Curl (`-I`):** I inspected the site’s HTTP response headers to get more details about the web server, uncovering the WordPress REST API endpoint (`/wp-json/`).
* **Wafw00f:** I tested the site for a Web Application Firewall (WAF) and discovered it is protected by **ModSecurity (SpiderLabs)**.
* **DNSRecon:** I mapped out the domain's DNS records, gathering details on its name servers, mail servers, SPF/TXT records, service records, and DNS software.

### 4.2 Network Scanning with Zenmap
For the second task, I used Zenmap on Windows to scan my local network. The goal was to find my local IP address and subnet, discover active devices (live hosts) along with their IP and MAC addresses, and create a visual network map.

* **Checking Local Details:** I first ran the `ipconfig /all` command in Windows Command Prompt to find my local IP address, subnet mask, and physical (MAC) address.
* **Scanning the Network:** I entered my local subnet into Zenmap and ran a **Ping Scan** to detect active devices on the network.
* **Scan Results:** My scan identified **1 live host** on my local network (`192.168.1.XXX` — *Note: Host-specific IP and MAC identifiers have been partially redacted for privacy*).
* **Mapping the Network:** After the scan finished, I opened the **Topology** tab in Zenmap, turned on the legend, and exported the network map as required for the task.

---

## 5. Risk Analysis / Impact
Based on the information collected during the footprinting and network scanning activities, I identified the following potential risks:

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **Web technology information exposed** | WhatWeb identified WordPress and WP Download Manager | Attackers may use exposed technology/version information to identify software requiring further security review | ● **Medium** |
| **2** | **Server IP address identifiable** | Nslookup resolved the domain to `192.232.216.135` | Provides information about the network location of the web service | ● **Low** |
| **3** | **HTTP technical information exposed** | Curl returned HTTP response headers and exposed `/wp-json/` | May assist technology fingerprinting and further enumeration | ● **Low** |
| **4** | **WAF technology identifiable** | Wafw00f identified ModSecurity (SpiderLabs) | Reveals information about the web application’s security architecture | ● **Low** |
| **5** | **DNS infrastructure information exposed** | DNSRecon identified NS servers, mail servers (`mx`), TXT/SPF records, and service-related SRV records | DNS information can help build a broader infrastructure profile | ● **Medium** |
| **6** | **Multiple live hosts visible on local network** | Zenmap identified 1 live host on the local network (`192.168.1.XXX` — redacted for privacy) | Demonstrates how active devices are discovered on a local subnet | ● **Medium** |

*Risk level key: ● Critical &nbsp; ● Medium &nbsp; ● Low*

The risks above are observations from the footprinting and scanning exercises, not confirmed vulnerabilities. The practical exercises primarily involved information gathering and host discovery. No exploitation or vulnerability validation was performed as part of these modules. Therefore, the presence of information such as a software version, IP address, or DNS record does not by itself mean that the system is vulnerable. Further authorized security testing would be required to confirm any actual vulnerability.

---

## 6. Recommendations
Based on the observations from these activities, the following security improvements are recommended:

1. **Limit exposed software details:** Hide version numbers for WordPress and plugins like WP Download Manager so attackers cannot easily look up known vulnerabilities.
2. **Keep CMS and plugins updated:** Regularly update WordPress, installed plugins, and server software to patch security flaws as soon as fixes are released.
3. **Harden HTTP headers:** Configure the web server to remove or mask unnecessary response headers that reveal backend technologies.
4. **Audit DNS records and enable DNSSEC:** Review public DNS records periodically to ensure only necessary entries are visible, and enable DNSSEC to protect domain lookups from spoofing.
5. **Maintain and monitor the WAF:** Keep the ModSecurity Web Application Firewall active and properly tuned to filter out automated reconnaissance and malicious traffic.
6. **Run regular internal network scans:** Periodically scan the local network to keep an accurate inventory of active devices and verify that no unauthorized hosts are connected.
7. **Always test with authorization:** Ensure all reconnaissance and scanning activities are strictly limited to owned systems or networks with explicit written permission.

---

## 7. Conclusion
During Week 2 of my Cybersecurity & Ethical Hacking internship, I completed practical exercises covering passive footprinting and active network scanning. Using six Kali Linux tools—WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, and DNSRecon—I collected domain registration details, server IP addresses, web software versions, HTTP headers, firewall status, and DNS records for `networkwalks.com`. Using Zenmap on Windows, I also scanned my local subnet, identified active host details (`192.168.1.XXX`), and exported a visual network topology.

These tasks demonstrated how much technical information can be uncovered simply by analyzing public records and standard network responses before any exploitation occurs. Furthermore, documenting these findings reinforced the importance of translating raw tool outputs into clear security risks and actionable fixes while operating strictly within an authorized scope.

---

## 8. Evidences Collected

### WHOIS Results
![WHOIS Results](screenshots/whois-results.jpg)

### WhatWeb Results
![WhatWeb Results](screenshots/whatweb-results.jpg)

### Nslookup Results
![Nslookup Results](screenshots/nslookup-results.jpg)

### Curl Results
![Curl Results](screenshots/curl-results.jpg)

### Wafw00f Results
![Wafw00f Results](screenshots/wafw00f-results.jpg)

### DNSRecon Results
![DNSRecon Results](screenshots/dnsrecon-results.jpg)

### IPConfig & Zenmap Scan Results
![Zenmap Scan](screenshots/zenmap-scan.jpg)

### Zenmap Topology
![Topology](screenshots/topology.jpg)

---

## 👤 Author
* **Ali ur Rehman Qureshi** (Cybersecurity Professional - B084)
* **LinkedIn:** [A.R. Qureshi](https://www.linkedin.com/in/a-r-qureshi/)

## 📌 Project Information
* **Program Name:** Cybersecurity program at Networkwalks
* **Week:** 02
* **Repository:** GitHub
