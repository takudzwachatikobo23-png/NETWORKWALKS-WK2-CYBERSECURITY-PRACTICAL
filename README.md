# NETWORKWALKS-WK2-CYBERSECURITY-PRACTICAL
🚀 Showcasing hands-on cybersecurity experience from Week 2 of the Networkwalks Cybersecurity Internship (Batch B084H), featuring practical exercises in footprinting, reconnaissance, and network scanning using Kali Linux and Zenmap to develop essential ethical hacking and network security skills.
# Week 2 Projects: Footprinting & Network Scanning

Cybersecurity & Ethical Hacking course (Networkwalks)

> For educational purposes only. Work was performed on the course-designated target (`networkwalks.com`) and on my own home network.

---

## Project Module 1 (W2-PM1): Footprinting & Reconnaissance with Kali Tools

**Environment:** Kali Linux (VirtualBox, NAT)  
**Target:** `networkwalks.com`  
**Date:** 12 Nov 2026

### Task 1: WHOIS

**Command:** `whois networkwalks.com`

| Item | Finding |
|---|---|
| Registrar | GoDaddy.com, LLC |
| Created | 2019-11-06 |
| Expires | 2027-11-06 |
| Name servers | NS6135.HOSTGATOR.COM, NS6136.HOSTGATOR.COM |
| DNSSEC | Unsigned |

**Attacker use:** Name servers may indicate the hosting provider. Registration details and abuse contacts could be misused for social engineering.  
**Defence:** Enable WHOIS privacy and DNSSEC where appropriate.

**Screenshot evidence:**

<img width="1280" height="720" alt="1" src="https://github.com/user-attachments/assets/b2c61acb-8bfa-4a46-b934-71fd8e25c72e" />


---

### Task 2: WhatWeb

**Command:** `whatweb networkwalks.com`

| Item | Finding |
|---|---|
| Web server | Apache |
| CMS | WordPress 7.1.4 |
| Plugin | WordPress Download Manager 3.3.76 |
| Libraries | Bootstrap, jQuery 3.7.1 |
| IP | 192.232.216.135 |
| Email displayed | info@networkwalks.com |

**Attacker use:** Technology and version details can be checked against public vulnerability information.  
**Defence:** Keep software updated, limit unnecessary version disclosure, and avoid publishing email addresses when not needed.

**Screenshot evidence:**

<img width="1280" height="720" alt="3" src="https://github.com/user-attachments/assets/9eb1c4d8-f2b0-4f67-92b0-5454b1c11b68" />


---

### Task 3: Nslookup

**Command:** `nslookup networkwalks.com`

The domain resolved to **192.232.216.135** via Google DNS (`8.8.8.8`), matching the WhatWeb result.

**Attacker use:** DNS results can help identify systems for further assessment.  
**Defence:** Consider a CDN or reverse proxy when appropriate, and protect the origin infrastructure.

**Screenshot evidence:**

<img width="1280" height="720" alt="3" src="https://github.com/user-attachments/assets/6185116a-9cc2-4c04-8804-d18e8830297b" />


---

### Task 4: Curl Headers

**Command:** `curl -I https://networkwalks.com`

| Item | Finding |
|---|---|
| Status | HTTP/1.1 200 OK |
| Server | Apache |
| Cache headers | X-nginx-cache: WordPress, X-Endurance-Cache-Level: 0 |
| Cookie | `__wpdm_client` (Secure, HttpOnly) |
| Endpoint observed | WordPress REST API at `/wp-json/` |

**Attacker use:** Response headers can reveal technology and publicly accessible endpoints.  
**Defence:** Review exposed headers and restrict endpoints only where appropriate.

**Screenshot evidence:**
<img width="1280" height="720" alt="3" src="https://github.com/user-attachments/assets/5460231f-e950-4c52-99f8-317d1955bd4f" />


---

### Task 5: Wafw00f

**Command:** `wafw00f networkwalks.com`

**Result:** The site was identified as being behind a ModSecurity (SpiderLabs) WAF.

**Attacker use:** Identifying a WAF provides information about the site's protective controls.  
**Defence:** Keep WAF rules updated, monitor blocked requests, and patch the application itself.

**Screenshot evidence:**

<img width="1280" height="720" alt="2" src="https://github.com/user-attachments/assets/719acb9d-c246-4d21-8eea-869710d3784c" />


---

### Task 6: DNSRecon

**Command:** `dnsrecon -d networkwalks.com`

| Item | Finding |
|---|---|
| DNSSEC | Not configured |
| Name servers | ns6135.hostgator.com, ns6136.hostgator.com |
| DNS software | BIND 9.16.23-RH |
| Mail server (MX) | mail.networkwalks.com (192.232.216.135) |
| SPF | `v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all` |
| SRV records | 8 cPanel `_autodiscover._tcp` records |

**Attacker use:** DNS records can reveal information about domain services and configuration.  
**Defence:** Review DNS records, consider DNSSEC, and configure SPF according to the organization’s mail requirements.

**Screenshot evidence:**
<img width="1280" height="720" alt="2" src="https://github.com/user-attachments/assets/bd6a0a0f-50e4-4d7f-8f16-f2c95324632d" />


---

### Combined target profile

The results indicate that the domain uses GoDaddy for registration, HostGator name servers, Apache and WordPress, and a ModSecurity WAF. The mail server resolved to the same IP address shown in the scan results.

### Module 1 evidence

Command output files and screenshots are saved in `w2-pm1-evidence/`.

---

## Project Module 5 (W2-PM5): Network Scanning with Zenmap

**Environment:** Windows laptop, Zenmap (Nmap 7.993)  
**Scan:** `nmap -sn 192.168.0.0/24` (ping scan)

### Task 1: Install Zenmap

Zenmap was installed using the download page:  
https://nmap.org/download.html


---

### Task 2: Local IP and subnet

| Item | Value |
|---|---|
| IPv4 address | 192.168.0.147 |
| Subnet mask | 255.255.255.0 |
| LAN subnet | 192.168.0.0/24 |
| Gateway | 192.168.0.1 |

<img width="1280" height="720" alt="nmap2" src="https://github.com/user-attachments/assets/ca8c2f77-4079-4c42-8667-084bc6998e3a" />


---

### Task 3: Configure and run the ping scan

**Command:** `nmap -sn 192.168.0.0/24`

The ping scan checked the local subnet for live hosts.

<img width="1280" height="720" alt="nmap2" src="https://github.com/user-attachments/assets/8d132a8a-34be-4d3a-861f-c5ea0dbf1dcf" />


---

### Task 4: Live-host results

**Result:** 6 live hosts were detected, including my laptop. The scan took 3.64 seconds.

| IP | MAC address (last 3 bytes) | Vendor |
|---|---|---|
| 192.168.0.1 | xx:xx:xx:62:82:69 | Oppo (router) |
| 192.168.0.140 | xx:xx:xx:0C:B8:0C | Apple |
| 192.168.0.141 | xx:xx:xx:3B:8C:4C | Unknown (randomized private MAC) |
| 192.168.0.147 | Not shown | Dell (my laptop) |
| 192.168.0.162 | xx:xx:xx:9B:FB:8C | Hui Zhou Gaoshengda Technology |
| 192.168.0.180 | xx:xx:xx:71:9C:AE | Unknown (randomized private MAC) |

Full MAC addresses have been shortened for privacy. Device MAC addresses from the home network are not published in this repository.

**Observation:** Two devices showed “Unknown” vendors. Their MAC addresses may be randomized private Wi-Fi addresses.

<img width="1280" height="720" alt="nmap2" src="https://github.com/user-attachments/assets/36270a41-daab-4280-94c0-5b7a08f142f9" />


---

### Task 5: Review host and vendor details

The scan results were reviewed to identify host IP addresses and available MAC/vendor information.


---

### Task 6: Review scan output

The completed scan output was saved and reviewed as evidence of the results.



---

### Task 7: Network topology

The Zenmap topology graph was saved as `w2-pm5-evidence/Zenmap1.pdf`.

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/e931c118-b975-45f4-b9f5-1d4c817cc777" />


---

## Key takeaways

- Footprinting gathers information that is publicly available.
- Limiting unnecessary technology, version, and DNS disclosures can reduce information exposure.
- Defenders can use authorized assessment tools to understand what their systems reveal.
