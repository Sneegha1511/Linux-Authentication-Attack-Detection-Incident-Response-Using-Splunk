# Investigation Methodology

**Tool:** Splunk  
**Dataset:** Linux_2k.log.txt  
**Total Events:** 2,000  

---

## 1. Overall Scope
Started with a basic search to load all events and check the sidebar fields.

**Query:** `source="Linux_2k.log.txt"`

- Sidebar immediately showed `rhost 47` — 47 unique remote IPs connecting to this server
- `date_month 2` — activity spans 2 months
- `process 30` — 30 unique processes recorded

---
## 2. Attack Type
Ran a process breakdown to see what was generating the most events.

**Query:** `source="Linux_2k.log.txt" | stats count by process | sort -count`

- `ftpd` — 916 events and `sshd(pam_unix)` — 677 events dominated everything else
- Combined they made up 79% of all events
- Confirmed dual brute force attack on FTP (port 21) and SSH (port 22)

---
## 3. Attacking IPs
Filtered to authentication failures and ranked by source IP.

`source="Linux_2k.log.txt" "authentication failure" | stats count by rhost | sort -count`

- 490 failures from 47 unique IPs
- `150.183.249.110` had 80 attempts — highest by far
- Mix of raw IPs and hostnames 

---
## 4. Targeted Accounts
Checked which usernames were being targeted.

`source="Linux_2k.log.txt" "authentication failure" | stats count by user | sort -count`

- Only 3 usernames across 490 failures — `root` (351), `guest` (17), `test` (4)
- 94.9% of attempts aimed at root
- `test` only received 4 attempts but was the account that got breached

---
## 5. Breach Confirmed
Searched for successful logins.

`source="Linux_2k.log.txt" "session opened for user test"`

- 36 successful sessions confirmed on the `test` account
- All via `sshd(pam_unix)` — 100% SSH
- Clicked `process` in the sidebar — confirmed single process, no need for extra query
- Attacker returned on Jun 30, Jul 1, Jul 2, Jul 7, Jul 13 — persistent access over 13 days
---

## 6. Attack Timeline
Charted authentication failures per day.

`source="Linux_2k.log.txt" "authentication failure" | timechart count span=1d`

- Brute force started June 14
- First breach June 30 — well before the biggest spike
- Peak activity July 10 — ~89 attempts in one day
- Last recorded activity July 27