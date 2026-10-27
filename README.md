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

## Attack Timeline

| Date (2026) | Event |
| --- | --- |
| 14 Jun | Attack begins: first failed SSH logins from 218.188.2.4 |
| 15 Jun | 37 failures from four sources, including attempts on root |
| 17 Jun | **First successful SSH session on `test`**  |
| 30 Jun | 10 `test` sessions in one day; repeated Kerberos failures from 163.27.187.39 |
| 1 to 2 Jul | 8 `test` sessions each day |
| 7 Jul | 6 `test` sessions; one root console login |
| 8 Jul | 4 failed `test` logins, all from 212.0.132.20 |
| 10 Jul | **Peak:** about 90 failed logins in one day |
| 13 Jul | Last 3 `test` sessions (36 in total over 6 days) |
| 18 Jul | Rapid username guessing ("user unknown") from one source |
| 24 Jul | FTP connection burst, then 2 anonymous FTP logins from 84.102.20.2 |

## Mitigation Plans and Defensive Controls

| # | Action | Why |
| --- | --- | --- |
| 1 | Lock or disable the `test` account | It is the only account with successful SSH sessions (36) |
| 2 | Block the top sources at the firewall, starting with 150.183.249.110 | Cuts off the heaviest sources of failed logins |
| 3 | Review what `test` could access and what commands ran in its 36 sessions | The log shows the logins, not what was done after them |
| 4 | Reset passwords for any account that could have been exposed | Removes a guessed or known password as a way in |


## Limitations
- Only authentication logs were available — could not determine what the attacker did after logging in
- Attacker IPs showed 0% abuse score on AbuseIPDB — likely used VPN or rented VPS, so geolocation is unreliable
