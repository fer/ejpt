---
description: >-
  OSINT: Widening the attack surface. Mounting targeted attacks. Sharpening your
  tools in preparation for the next phases.
---

# Information Gathering

## Open-Source Intelligence

### Information Gathering from Social Networks

* **CrunchBase**: find detailed information about founders, investors, employees, buyouts and acquisitions.

### Government Sites

* System for Award Management.
* GSA eLibrary.

### Whois database&#x20;

Also accessible through Linux command `whois`:

* Owner name.
* Street addresses.
* Email Address.
* Technical Contacts.

### Browsing Client's sites

* Check products.
* Services.
* Technologies.
* Company Culture.

### Discovering Emai Pattern

* `name.surname@company.com`
* `surname.name@company.com`
* Many email systems tend to inform the sender that mail was not delivered because it does not exit.

## Subdomain Enumeration

* We keep on widening the attack surface, discovering as many websites owned by the company as possible.
* It's common for websites of the same company to share the same top-level domain name.
* Likely to find resources that:
  * May contain outdated software.
  * Buggy software.
  * Administrative Interfaces.
* Bug bounty program writeups.

### Online services

* [VirusTotal](https://www.virustotal.com)
* [DNSdumpster](https://dnsdumpster.com): a subscription is needed.
* [crt.sh](https://crt.sh): view certificates and see associated domains and subdomains.

### Automated tools

* `sublist3r` / `subbrute`: use domain wordlist in order to bruteforce subdomains.

```bash
# sublist3r using Passive DNS services
sublist3r -v -d google.com -b

# -v : verbose
# -d <domain>
# -b bruteforce

# amass
amass -ip -d google.com
```
