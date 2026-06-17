# Network-Traffice-Investigation-
This project documents about the investigation of HawkEye Blue team .  The lab simulates a real‑world cyber incident where packet captures (PCAPs) reveal malicious activity, data exfiltration, and attacker infrastructure.CTF
---

## 1. Capture Metadata
Packets count, first timestamp, duration → These establish the scope of evidence. Analysts must know how much data they’re working with, when the incident began, and how long it persisted. This sets the timeline for attack reconstruction.

- **File:** `stealer.pcap`
-  navigate to the Statistics menu at the top of Wireshark and select Capture File Properties.
-  **Packets:** 4003  
- **First Packet:** 2019‑04‑11 02:07:07 UTC  
- **Duration:** 01:03:41  
- **Encapsulation:** Ethernet  
- **Average Packet Size:** 597 B  

---
## 2. Link‑Level Activity
Most active MAC address, NIC manufacturer, headquarters → Identifying the busiest device at the link layer helps pinpoint the primary victim or attacker. Manufacturer details aid in asset inventory and attribution, while headquarters location can support supply‑chain risk analysis.


- By navigating to Statistics → Endpoints → Ethernet (ipv4,mac address,ipv6,ethernet)
- check which machine are mostly active
- the first three octets of a MAC address, we can determine the manufacturer of the NIC 
- **Most active MAC:** `00:08:02:1c:47:ae`  
- **Manufacturer:** Hewlett‑Packard  
- **Headquarters:** Palo Alto, California, USA 

---

## 3. Internal Network
Number of hosts (/24 subnet), most active hostname → Reveals how many systems were involved and which one was targeted. This helps scope incident impact and prioritize containment.

- To determine the number of actual computers in the capture, the Wireshark Endpoints section under *Statistics → Endpoints → IPv4* provides a summary of all IP addresses detected during the session. 
- DHCP provides additional informations ,hostname,mac address,default gateway.
- **Hostname:** `Beijing‑5cd1‑PC`  
- **Vendor Class:** MSFT 5.0  
- **Private Network:** 10.4.10.0/24 (3 hosts involved)  

---
## 4. DNS Analysis
DNS server IP, queried domain, resolved IP, country → DNS queries often expose attacker infrastructure. Mapping domains to IPs and countries highlights command‑and‑control servers or malicious download sites, crucial for threat intelligence.

- filter DNS for DNS quries connecting with client and server 

- **DNS Server:** 10.4.10.4  
- **Victim Query (Packet 204):** `proforma‑invoices.com`  
- **Resolved IP:** 217.182.138.150  
- **Geolocation:** Roubaix, France (OVH SAS)  
---
5. Host System
Victim OS → Knowing the operating system allows analysts to assess vulnerabilities exploited and tailor remediation steps (patches, hardening, EDR deployment).

6. Malware Delivery
Malicious file name, MD5 hash, webserver software → Identifying the payload and its hash enables IOC sharing across SOC tools. Webserver fingerprinting helps track attacker infrastructure and block similar hosts.

7. Public IP Disclosure
Victim’s public IP → Confirms the external identity of the compromised system. This is critical for ISP coordination and external threat hunting.

8. Data Exfiltration
Email server country, software, recipient account, password, malware variant, stolen credentials, exfiltration interval → These questions uncover the exfiltration channel. Analysts learn where the stolen data went, how often, and which malware family was responsible. This is the heart of incident response: stopping data loss and understanding attacker tradecraft.

9. MITRE ATT&CK Mapping
Each finding maps to ATT&CK techniques (Execution, Persistence, Credential Access, Exfiltration). This shows analyst maturity in aligning evidence with global frameworks.

10. Indicators of Compromise (IOCs)
Domains, IPs, hashes, emails, passwords → These are the artifacts SOCs share with peers, threat intel feeds, and detection systems to prevent repeat attacks.
