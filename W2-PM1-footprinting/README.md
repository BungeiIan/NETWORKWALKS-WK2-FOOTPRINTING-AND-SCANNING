# NETWORKWALKS-WK2-PM1-FOOTPRINTING-RECONNAISSANCE

## Footprinting & Reconnaissance: README

This project records a footprinting exercise against the live website **networkwalks.com**. Six command-line tools that come with Kali Linux were used to collect publicly available information about the site, and the output of each tool was saved as evidence. The runs were done on 18 September 2026.

## What You Need

- Kali Linux (all six tools are pre-installed)
- An internet connection
- Target: `networkwalks.com` (the course's designated lab domain)
- A screenshot and a saved text file for every tool

## Steps Carried Out

### 1. WHOIS: domain registration
`whois networkwalks.com | tee whois_output.txt`

The record shows the domain is registered with GoDaddy. It was created on 6 November 2019, last updated on 12 November 2025 and runs until 6 November 2027. The name servers are `NS6135.HOSTGATOR.COM` and `NS6136.HOSTGATOR.COM`, so the site is hosted with HostGator. Delete, renew, transfer and update locks are set, and DNSSEC is listed as unsigned.

### 2. WhatWeb: technology fingerprint
`whatweb networkwalks.com | tee whatweb_output.txt`

The plain HTTP address redirects (301) to HTTPS. The site runs on Apache with WordPress 7.1.1 and the WP Download Manager 3.3.58 plugin, plus jQuery 3.7.1 and Google Tag Manager. The output also exposes the server IP `192.232.216.135`, the contact address `info@networkwalks.com` and an HttpOnly cookie named `__wpdm_client`.

### 3. Nslookup: DNS resolution
`nslookup networkwalks.com | tee nslookup_output.txt`

Asking Google's resolver (8.8.8.8) returned a non-authoritative answer of `192.232.216.135`, the same address WhatWeb reported.

### 4. Curl: HTTP headers
`curl -I https://networkwalks.com | tee curl_output.txt`

The server replied `HTTP/2 200` and identified itself as Apache. Cache headers (`x-nginx-cache: WordPress`, `x-endurance-cache-level: 0`) show a caching layer in front of WordPress, and a `Link` header advertises the REST API root at `/wp-json/`. The session cookie is marked Secure and HttpOnly.

### 5. Wafw00f: firewall detection
`wafw00f networkwalks.com | tee wafw00f_output.txt`

After two requests, wafw00f (v2.4.2) reported that the site sits behind a **ModSecurity (SpiderLabs)** Web Application Firewall.

### 6. DNSRecon: DNS enumeration
`dnsrecon -d networkwalks.com | tee dnsrecon_output.txt`

The scan returned the SOA record and both name servers (`ns6135.hostgator.com` at 50.87.144.87 and `ns6136.hostgator.com` at 192.232.216.131) and flagged that **recursion is enabled** on each of them. It also listed the mail server `mail.networkwalks.com`, the A record, an SPF record ending in `~all`, a Google site-verification TXT record and `_autodiscover._tcp` SRV records pointing to cPanel's email discovery service on port 443. The DNSSEC query received no answer.

## Quick Reference

| Item | Result |
|---|---|
| Domain | networkwalks.com |
| Registrar | GoDaddy (created 2019-11-06, expires 2027-11-06) |
| Hosting provider | HostGator |
| Server IP | 192.232.216.135 |
| Web server | Apache |
| CMS and plugin | WordPress 7.1.1, WP Download Manager 3.3.58 |
| Other technology | jQuery 3.7.1, Google Tag Manager |
| Exposed endpoint | WordPress REST API at `/wp-json/` |
| Mail server | mail.networkwalks.com |
| Name servers | ns6135.hostgator.com, ns6136.hostgator.com (recursion enabled) |
| SPF policy | Soft-fail (`~all`) |
| DNSSEC | Not enabled |
| WAF | ModSecurity (SpiderLabs) |

## Notes

- These tools do not attack the target. They read information the site and its DNS already publish, which is why footprinting is quiet and hard for the target to notice.
- The results are observations, not proven vulnerabilities. Software versions, IP addresses and DNS records need further authorised testing before anything can be called a weakness.
- Only run these tools against systems you own or have permission to test.

## Evidence

```
W2-PM1-footprinting/
├── README.md
├── outputs/        # whois_output.txt, whatweb_output.txt, nslookup_output.txt,
│                   # curl_output.txt, wafw00f_output.txt, dnsrecon_output.txt
└── screenshots/    # one screenshot per tool
```

**Author:** Ian Bungei Imbayi

Based on the "Footprinting & Reconnaissance Attacks with Multiple Kali Tools" task from Networkwalks Academy: [www.networkwalks.com](https://www.networkwalks.com)
