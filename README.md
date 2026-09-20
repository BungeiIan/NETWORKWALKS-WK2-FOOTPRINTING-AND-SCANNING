# NETWORKWALKS-WK2-FOOTPRINTING-AND-SCANNING
## Week 2: Cybersecurity & Ethical Hacking

This repository holds my Week 2 practical work for the Networkwalks Cybersecurity & Ethical Hacking program. It covers two project modules that follow the first steps of a security test: collecting public information about a target, then finding which devices are alive on a network.

### Contents
Module	Topic	Tools	Folder
W2-PM1	Footprinting & Reconnaissance	whois, whatweb, nslookup, curl, wafw00f, dnsrecon (Kali Linux)	W2-PM1-footprinting
W2-PM5	Network Scanning	Zenmap / Nmap (Windows)	W2-PM5-zenmap

The full write-up of both modules is in the report: report/W2-PM-FINAL_Penetration_Testing_Report_Ian_Imbayi.docx.

### Highlights

## W2-PM1: footprinting networkwalks.com

Hosted with HostGator, registered with GoDaddy, server IP 192.232.216.135
Apache running WordPress 7.1.1 with the WP Download Manager 3.3.58 plugin
Protected by a ModSecurity (SpiderLabs) web application firewall
DNS check flagged recursion enabled on both name servers, and the SPF record uses a soft-fail policy (~all)


### W2-PM5: scanning my own hotspot network (192.168.5.0/24)

2 live hosts found out of 256 addresses
My own PC showed ports 80, 135, 139, 445, 3306 (MySQL) and 5357 open on the hotspot-facing address
The other device uses a randomised MAC address, so Nmap could not name its vendor
Repository structure
.
├── README.md
├── report/
│   └── W2-PM-FINAL_Penetration_Testing_Report_Ian_Imbayi.docx
├── W2-PM1-footprinting/
│   ├── README.md
│   ├── outputs/        # saved text output from each tool
│   └── screenshots/
└── W2-PM5-zenmap/
    ├── README.md
    ├── screenshots/
    └── topology.pdf
### Key takeaways
Footprinting reads only what a target already makes public, so it is quiet and hard to detect.
Software versions, open ports and DNS records are observations, not proven vulnerabilities. Confirming a weakness needs further authorised testing.
Trimming what is exposed, patching promptly, strengthening email authentication and limiting services on exposed interfaces all reduce what an attacker can learn.

Author: Ian Bungei Imbayi

Based on the Week 2 project tasks from Networkwalks Academy: www.networkwalks.com
