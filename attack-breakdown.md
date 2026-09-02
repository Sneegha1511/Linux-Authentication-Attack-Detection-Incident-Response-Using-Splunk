# Attack Breakdown

**Analyst:** Sneegha
**Tool:** Splunk
**Log File:** Linux_2k.log.txt
**Server:** Sneegha
**Total Events:** 2,000

---

## What Happened

47 IPs ran a brute force attack against SSH and FTP on a Linux server over 43 days.
The `test` account was cracked and the attacker kept coming back for 13 days across 36 sessions.

---

## Key Numbers

| What | Number |
|------|--------|
| Total events | 2,000 |
| Authentication failures | 490 |
| Unique attacking IPs | 47 |
| Accounts targeted | 3 |
| Successful sessions | 36 |
| Days of persistent access | 13 |
| Attack duration | 43 days |
| Peak attack day | July 10 (89 attempts) |

---

## Findings

**Attack type:**
- ftpd and sshd dominated — 916 and 677 events
- Both FTP (port 21) and SSH (port 22) were targeted at the same time

**Attacking IPs:**
- `150.183.249.110` was the most aggressive — 80 attempts, 3.5x more than anyone else
- Mix of raw IPs and hostnames — netvigator.com (Hong Kong), gtconnect.net
- Checked top 3 IPs on AbuseIPDB — all 0% abuse score
- Likely VPN or rented VPS —  geolocation is unreliable

**Targeted accounts:**
- Only 3 accounts tried across 490 failures
- root got 351 attempts — 94.9% of all failures
- test only got 4 attempts but was the one that got breached
- Automated script behavior — goes for default accounts first

**The breach:**
- 36 sessions opened on `test` account via SSH
- Attacker returned 5 separate times — Jun 30, Jul 1, Jul 2, Jul 7, Jul 13
- 4 attempts to crack it — password was clearly very weak
- uid=509 — low privilege account, not root

**Timeline:**
- Attacks started June 14
- Breach happened June 30 — before the biggest spike
- July 10 peak (~89 attempts) happened after the breach — attacker was already inside via test but still trying to crack root from outside
- Last activity July 27

---

## Threat Intelligence

Checked top 3 IPs on AbuseIPDB:

| IP | Country | ISP | Abuse Score |
|----|---------|-----|-------------|
| 150.183.249.110 | South Korea | KISTI (University) | 0% |
| 207.243.167.114 | USA | AT&T | 0% |
| 209.152.168.249 | Sweden | WebHostPlus (Data Center) | 0% |

All clean records — consistent with VPN or rented VPS to avoid detection.
