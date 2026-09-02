# Linux Brute Force Attack Analysis Using Splunk

A log analysis project using Splunk to investigate a Linux authentication log file, identify brute force attacks, and find a confirmed account breach.

---

## Tools Used
- Splunk (log analysis)
- AbuseIPDB (IP reputation check)

## Dataset
- **File:** Linux_2k.log.txt
- **Total Events:** 2,000
- **Log Type:** Linux authentication logs (linux_secure)
- **Server Name:** Sneegha

## What I Did
Loaded a Linux authentication log into Splunk and investigated it step by step to answer these questions:

- What kind of attack happened?
- Who was attacking?
- Which accounts were targeted?
- Did anyone get in?
- When did it happen?

## What I Found
- **Attack type:** Brute force on SSH (port 22) and FTP (port 21)
- **Total auth failures:** 490 across 47 unique IPs
- **Top attacker:** 150.183.249.110 with 80 attempts
- **Accounts targeted:** root (351 attempts), guest (17), test (4)
- **Breach confirmed:** test account cracked — 36 successful SSH sessions
- **Persistent access:** Attacker returned 5 times over 13 days
- **Attack period:** June 14 to July 27
- **Peak attack day:** July 10 (~89 attempts in one day)


## Limitations
- Only authentication logs were available — could not determine what the attacker did after logging in
- Attacker IPs showed 0% abuse score on AbuseIPDB — likely used VPN or rented VPS, so geolocation is unreliable