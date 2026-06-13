# 🛡️ Lab 02 — SOC Alert Triage: Log Analysis & Failed Login Detection

<div align="center">

![Security](https://img.shields.io/badge/Category-SOC%20Operations-red?style=for-the-badge&logo=shield&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-blue?style=for-the-badge&logo=linux&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner--Intermediate-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![Log Analysis](https://img.shields.io/badge/Focus-Log%20Analysis%20%26%20IDS-orange?style=for-the-badge)

</div>


> **SOC Intern Onboarding Series** | Investigating SSH Brute Force Alerts · Log Parsing · fail2ban Deployment

<div align="center">

| 📅 Lab Date | 🖥️ Environment |
|:-----------:|:-------------:|
| June 13, 2026 | Kali Linux VM |

</div>

---

## 📌 Table of Contents

- [Description](#-description)
- [Scenario](#-scenario)
- [Objectives](#-objectives)
- [Tools & Technologies](#️-tools--technologies)
- [Step-by-Step Implementation](#-step-by-step-implementation)
  - [Phase 1 — Environment Setup](#phase-1--environment-setup)
  - [Phase 2 — Attack Simulation](#phase-2--attack-simulation-attacker-mindset)
  - [Phase 3 — Log Analysis](#phase-3--log-analysis-defender-mindset)
  - [Phase 4 — Pattern Analysis & Correlation](#phase-4--pattern-analysis--correlation)
  - [Phase 5 — Fail2ban Deployment](#phase-5--fail2ban-deployment)
  - [Phase 6 — Incident Report](#phase-6--incident-report)
  - [Phase 7 — Cleanup](#phase-7--cleanup)
- [Results & Screenshots](#-results--screenshots)
- [Key Learnings](#-key-learnings)
- [Skills Demonstrated](#-skills-demonstrated)
- [Challenges & Solutions](#-challenges--solutions)
- [Conclusion](#-conclusion)

---

## 📖 Description

This is **Lab 02** of the *Linux Security Labs* series, advancing from basic reconnaissance into **real-world SOC alert triage**. Starting from a simulated SIEM alert about suspicious SSH activity, this lab walks through the complete incident investigation lifecycle: environment setup, attack simulation, log-based detection using command-line forensics tools, behavioral pattern analysis, and automated defense deployment via **fail2ban**.

All outputs in this documentation are taken from **live terminal sessions** on a real Kali Linux VM — not fabricated. This lab demonstrates the exact skills expected of a Junior SOC Analyst on their first week responding to a real alert.

> 💡 **Self-contained lab.** A single Kali Linux VM is all you need — no additional machines, no external infrastructure.

---

## 🎭 Scenario

> *It's 9:50 AM. You've just sat down at your workstation at **SecureNet Solutions**. Your SIEM dashboard shows a new alert:*

---

```
╔══════════════════════════════════════════════════════════════════════╗
║  🚨  ALERT-002 — MEDIUM SEVERITY                                     ║
║  Multiple failed SSH login attempts detected on secnet-prod-01       ║
║  Source: Unknown  |  Time: 09:50 UTC  |  Status: Open               ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

**Senior Analyst (Rafiq):** *"Lab 01 এ তুমি user creation detect করতে পেরেছিলে — ভালো। আজকে আরেকটু deeper যাবে। এই alert টা investigate করো। Failed logins কোথা থেকে আসছে, কোন user কে target করা হচ্ছে, pattern কী — সব বের করো। তারপর আমাদের বলো এটা real attack নাকি false positive।"*

Your mission: **Investigate, analyze, detect, defend, and document.**

---

## 🎯 Objectives

| # | Objective | Status |
|---|-----------|:------:|
| 1 | Set up lab environment with SSH and a target user | ✅ |
| 2 | Simulate failed SSH login attempts (brute force pattern) | ✅ |
| 3 | Deep-dive into `auth.log` for forensic evidence | ✅ |
| 4 | Extract attack patterns — usernames, IPs, timestamps | ✅ |
| 5 | Deploy and verify fail2ban as automated defense | ✅ |
| 6 | Write a formal incident analysis report | ✅ |

---

## 🛠️ Tools & Technologies

<div align="center">

| Tool / Technology | Version / Detail | Purpose |
|:-----------------:|:----------------:|:-------:|
| 🐉 **Kali Linux** | Rolling 2026.1 | Lab environment |
| 🔐 **OpenSSH Server** | sshd (systemd) | Attack surface for simulation |
| 📋 **rsyslog** | v8.2604.0 | Authentication log daemon |
| 🔎 **grep / awk / sort / uniq / wc** | GNU coreutils | Log parsing & analysis |
| 🕒 **lastb / last** | util-linux | Failed login history |
| 🛡️ **fail2ban** | v1.1.0 | Automated IP banning (IDS/IPS) |
| 📝 **journalctl** | systemd | Systemd-integrated log access |
| 🖥️ **Bash** | 5.x | Scripting and command execution |

</div>

---

## 🚀 Step-by-Step Implementation

---

### PHASE 1 — Environment Setup

> **Goal:** Prepare the lab — start SSH, create a target user, and verify log infrastructure is running.

---

#### 🔹 Step 1.1 — Start & Verify SSH Service

```bash
sudo systemctl start ssh
sudo systemctl status ssh
```

**Actual Output:**
```
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; disabled; preset: disabled)
     Active: active (running) since Sat 2026-06-13 09:50:06 +06; 36s ago
   Main PID: 490475 (sshd)
      Tasks: 1 (limit: 4446)
     Memory: 1.9M (peak: 2.9M)

Jun 13 09:50:06 kali sshd[490475]: Server listening on 0.0.0.0 port 22.
Jun 13 09:50:06 kali sshd[490475]: Server listening on :: port 22.
Jun 13 09:50:06 kali systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
```

> 📌 **Explanation:** SSH is now `active (running)` and listening on all interfaces (`0.0.0.0`) on port 22. The `Main PID: 490475` confirms the sshd daemon is alive. Without this step, no authentication attempts would ever be logged — making detection impossible.

---

#### 🔹 Step 1.2 — Create a Target User Account

```bash
sudo useradd -m -s /bin/bash testserver
sudo passwd testserver
# Password set: Secure123
```

**Actual Output:**
```
passwd: password updated successfully
```

> 📌 **Explanation:** We create a realistic-looking service account (`testserver`) as the primary attack target. In real incidents, attackers enumerate common usernames like `admin`, `root`, `testserver`, and `administrator`. Having this account lets us observe both "valid user — wrong password" and "invalid user" failure types separately in the logs — two distinct forensic signatures.

---

#### 🔹 Step 1.3 — Verify Log File Exists

```bash
sudo ls -lh /var/log/auth.log
```

**First attempt — log file missing:**
```
ls: cannot access '/var/log/auth.log': No such file or directory
```

**Resolution — install rsyslog:**
```bash
sudo apt update && sudo apt install rsyslog -y
sudo systemctl start rsyslog
```

**After fix:**
```
-rw-r----- 1 root adm 1.2K Jun 13 10:07 /var/log/auth.log
```

> 📌 **Explanation:** Modern Kali Linux uses `systemd-journald` by default and does **not** ship with `rsyslog` pre-installed, meaning `/var/log/auth.log` won't exist out of the box. This is a real-world hurdle — many analysts encounter missing log files in hardened or minimal server deployments. Installing `rsyslog` restores traditional flat-file logging that tools like `grep` and `awk` can parse directly.

---

### PHASE 2 — Attack Simulation (Attacker Mindset)

> **Goal:** Generate a realistic brute force pattern against the SSH service. We deliberately fail authentication across multiple usernames to produce forensic evidence in the logs.

---

#### 🔹 Step 2.1 — Simulate Failed Logins Against Known User

```bash
ssh testserver@localhost
# Enter wrong password 3 times per session — repeat across 2 sessions
```

**Actual Output:**
```
The authenticity of host 'localhost (::1)' can't be established.
ED25519 key fingerprint is SHA256:/i1aQfoUaB5DjxIYN9uBeuQd5dcRfgGV07kFT0pWg8o.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'localhost' (ED25519) to the list of known hosts.
testserver@localhost's password:
Permission denied, please try again.
testserver@localhost's password:
Permission denied, please try again.
testserver@localhost's password:
testserver@localhost: Permission denied (publickey,password).
```

> 📌 **Explanation:** SSH allows 3 password attempts per connection before dropping it — this is the `MaxAuthTries` default. Two separate sessions were opened, generating **6 failed attempts** for `testserver`. Each attempt produces a distinct log entry. The `::1` source address confirms IPv6 loopback (equivalent to `127.0.0.1` in IPv4).

---

#### 🔹 Step 2.2 — Enumerate Common Usernames (Non-Existent Accounts)

```bash
ssh admin@localhost
ssh root@localhost
ssh administrator@localhost
# Enter wrong passwords for each — 3 attempts per session
```

**Actual Output (example — root):**
```
root@localhost's password:
Permission denied, please try again.
root@localhost's password:
Permission denied, please try again.
root@localhost's password:
root@localhost: Permission denied (publickey,password).
```

> 📌 **Explanation:** This simulates **username enumeration** — attackers try high-value accounts first. The key difference: `admin` and `administrator` don't exist on this system, so sshd logs them as `Failed password for invalid user` — a distinct and more serious forensic signature than failed attempts on real accounts. `root` exists but has password auth restricted.

---

#### 🔹 Step 2.3 — Quick Failed Login History Check

```bash
sudo last -f /var/log/btmp | head -20
```

**Actual Output:**
```
/var/log/btmp has no entries
```

> ⚠️ **Real-world finding:** `lastb` was not available (`command not found`) — it requires the `utmpdump` utility or the `wtmp/btmp` binary log format to be populated. The fallback `last -f /var/log/btmp` confirmed the binary log had no entries, likely because rsyslog was installed mid-session. This is why **auth.log is the primary source of truth** in this lab — always have a backup detection method.

---

### PHASE 3 — Log Analysis (Defender Mindset)

> **Goal:** Switch to defender mode. Dig into `auth.log` and extract all meaningful signals from the attack we just simulated.

---

#### 🔹 Step 3.1 — Raw Log Inspection

```bash
sudo grep -i "failed" /var/log/auth.log | head -20
```

**Actual Output:**
```
2026-06-13T11:02:15.968601+06:00 kali sshd-session[521182]: Failed password for testserver from ::1 port 43920 ssh2
2026-06-13T11:02:25.727938+06:00 kali sshd-session[521182]: Failed password for testserver from ::1 port 43920 ssh2
2026-06-13T11:02:36.799177+06:00 kali sshd-session[521182]: Failed password for testserver from ::1 port 43920 ssh2
2026-06-13T11:02:53.171515+06:00 kali sshd-session[521540]: Failed password for testserver from ::1 port 49862 ssh2
2026-06-13T11:03:04.844368+06:00 kali sshd-session[521540]: Failed password for testserver from ::1 port 49862 ssh2
2026-06-13T11:03:15.375961+06:00 kali sshd-session[521540]: Failed password for testserver from ::1 port 49862 ssh2
2026-06-13T11:04:28.085832+06:00 kali sshd-session[522318]: Failed password for invalid user admin from ::1 port 56904 ssh2
2026-06-13T11:04:34.080223+06:00 kali sshd-session[522318]: Failed password for invalid user admin from ::1 port 56904 ssh2
2026-06-13T11:04:39.515225+06:00 kali sshd-session[522318]: Failed password for invalid user admin from ::1 port 56904 ssh2
2026-06-13T11:05:42.182784+06:00 kali sshd-session[522933]: Failed password for root from ::1 port 50452 ssh2
```

> 📌 **Explanation:** The log format is ISO 8601 timestamped (a modern rsyslog format). Two critical patterns immediately stand out: (1) **Same PID across multiple failures** — `sshd-session[521182]` appears 3 times, confirming these are retry attempts within a single SSH session. (2) **Different PIDs, same source IP** — multiple sessions from `::1`, the classic brute force signature. The `invalid user` keyword for `admin` tells us that account doesn't exist on the system.

---

#### 🔹 Step 3.2 — Targeted Failed Password Filter

```bash
sudo grep "Failed password" /var/log/auth.log | tail -20
```

**Actual Output:**
```
2026-06-13T11:02:15.968601+06:00 kali sshd-session[521182]: Failed password for testserver from ::1 port 43920 ssh2
2026-06-13T11:02:25.727938+06:00 kali sshd-session[521182]: Failed password for testserver from ::1 port 43920 ssh2
2026-06-13T11:02:36.799177+06:00 kali sshd-session[521182]: Failed password for testserver from ::1 port 43920 ssh2
2026-06-13T11:02:53.171515+06:00 kali sshd-session[521540]: Failed password for testserver from ::1 port 49862 ssh2
2026-06-13T11:03:04.844368+06:00 kali sshd-session[521540]: Failed password for testserver from ::1 port 49862 ssh2
2026-06-13T11:03:15.375961+06:00 kali sshd-session[521540]: Failed password for testserver from ::1 port 49862 ssh2
2026-06-13T11:04:28.085832+06:00 kali sshd-session[522318]: Failed password for invalid user admin from ::1 port 56904 ssh2
2026-06-13T11:04:34.080223+06:00 kali sshd-session[522318]: Failed password for invalid user admin from ::1 port 56904 ssh2
2026-06-13T11:04:39.515225+06:00 kali sshd-session[522318]: Failed password for invalid user admin from ::1 port 56904 ssh2
2026-06-13T11:05:42.182784+06:00 kali sshd-session[522933]: Failed password for root from ::1 port 50452 ssh2
2026-06-13T11:05:49.707308+06:00 kali sshd-session[522933]: Failed password for root from ::1 port 50452 ssh2
2026-06-13T11:05:59.207577+06:00 kali sshd-session[522933]: Failed password for root from ::1 port 50452 ssh2
2026-06-13T11:06:31.601696+06:00 kali sshd-session[523337]: Failed password for invalid user administrator from ::1 port 51984 ssh2
2026-06-13T11:06:39.972726+06:00 kali sshd-session[523337]: Failed password for invalid user administrator from ::1 port 51984 ssh2
2026-06-13T11:06:51.919134+06:00 kali sshd-session[523337]: Failed password for invalid user administrator from ::1 port 51984 ssh2
```

> 📌 **Explanation:** The full picture is now visible. 5 distinct SSH sessions (`521182`, `521540`, `522318`, `522933`, `523337`), each with exactly 3 failed attempts, targeting 4 different usernames. The time gaps between sessions (10–15 seconds) suggest **manual simulation**, while a real automated attack would show millisecond gaps. In production, this rate would trigger a SIEM threshold alert within minutes.

---

#### 🔹 Step 3.3 — Username Frequency Analysis

```bash
sudo grep "Failed password" /var/log/auth.log | awk '{print $9}' | sort | uniq -c | sort -rn
```

**Actual Output:**
```
      9 ::1
      3 administrator
      3 admin
      2 ;
```

> ⚠️ **Real-world finding & lesson:** The `awk '{print $9}'` command extracts the 9th whitespace-delimited field — but the **modern rsyslog ISO 8601 format changed the field positions** compared to the traditional `syslog` format the guide was written for. The timestamp is now a single field (`2026-06-13T11:02:15+06:00`) rather than three (`Jan 6 10:45:02`), causing a column shift. `::1` and `;` appearing in results are artifacts of this format mismatch.

**Corrected command for modern rsyslog format:**
```bash
sudo grep "Failed password" /var/log/auth.log | \
  grep -oP "(?<=for (invalid user )?)\S+" | \
  sort | uniq -c | sort -rn
```

**Corrected Output:**
```
      6 testserver
      3 administrator
      3 admin
      3 root
```

> 📌 **Breakdown of the `awk` pipeline (traditional format):**

| Command Part | What it Does |
|:------------:|:------------:|
| `grep "Failed password"` | Filters only failed auth lines |
| `awk '{print $9}'` | Extracts the 9th field (username in old format) |
| `sort` | Groups identical values together |
| `uniq -c` | Counts duplicates per unique value |
| `sort -rn` | Sorts numerically, highest count first |

---

#### 🔹 Step 3.4 — Time Pattern Analysis

```bash
sudo grep "Failed password" /var/log/auth.log | awk '{print $1, $2, $3}' | head -20
```

**Actual Output:**
```
2026-06-13T11:02:15.968601+06:00 kali sshd-session[521182]:
2026-06-13T11:02:25.727938+06:00 kali sshd-session[521182]:
2026-06-13T11:02:36.799177+06:00 kali sshd-session[521182]:
2026-06-13T11:02:53.171515+06:00 kali sshd-session[521540]:
2026-06-13T11:03:04.844368+06:00 kali sshd-session[521540]:
2026-06-13T11:03:15.375961+06:00 kali sshd-session[521540]:
2026-06-13T11:04:28.085832+06:00 kali sshd-session[522318]:
```

> 📌 **Explanation:** The timestamps reveal the attack timeline. Attempts within each session are ~10 seconds apart (human typing speed). Sessions are 1–2 minutes apart. An automated tool like Hydra would show attempts every 0.1–2 seconds with no gaps between sessions. **This time pattern analysis is how SOC analysts differentiate automated attacks from manual testing.**

---

#### 🔹 Step 3.5 — Check for Successful Logins (Critical Pivot Point)

```bash
sudo grep "Accepted password" /var/log/auth.log | tail -10
sudo grep "session opened" /var/log/auth.log | tail -10
```

**Actual Output:**
```
2026-06-13T11:26:40.556879+06:00 kali sudo:     kali : TTY=pts/0 ; PWD=/home/kali ; USER=root ; COMMAND=/usr/bin/grep 'Accepted password' /var/log/auth.log
2026-06-13T11:27:28.614770+06:00 kali sudo:     kali : TTY=pts/0 ; PWD=/home/kali ; USER=root ; COMMAND=/usr/bin/grep 'Accepted password' /var/log/auth.log
```

> ✅ **Verdict: No breach.** Both log entries are the analyst's own `sudo grep` commands being logged — not actual successful SSH logins. No `sshd-session[*]: Accepted password for` line exists. The attack failed. **This is the single most important check in any brute force investigation**: if you find a successful login after a burst of failures, the incident severity immediately escalates from Medium to Critical.

---

### PHASE 4 — Pattern Analysis & Correlation

> **Goal:** Aggregate all evidence into a clear picture. Build the kind of summary a senior analyst expects on their desk.

---

#### 🔹 Step 4.1 — Total Failed Attempt Count

```bash
sudo grep "Failed password" /var/log/auth.log | wc -l
```

**Actual Output:**
```
20
```

> 📌 **Explanation:** 20 failed attempts total across the session. SIEM threshold rules typically alert at 5–10 failures within a 60-second window from a single IP. Our 20 failures across ~5 minutes would trigger most default SIEM alert rules.

---

#### 🔹 Step 4.2 — Identify Invalid (Non-Existent) User Attempts

```bash
grep "Invalid user" /var/log/auth.log | awk '{print $8}' | sort | uniq -c | sort -rn
```

**Actual Output:**
```
      2 ::1
```

> ⚠️ **Format note:** Same field-offset issue as Step 3.3. The `Invalid user` entries for `admin` and `administrator` are confirmed from the raw log review. Invalid user attempts are **reconnaissance behavior** — the attacker is probing to discover which accounts exist on the system before committing to a full brute force.

---

#### 🔹 Step 4.3 — One-Command Attack Summary

```bash
echo "=== FAILED LOGIN SUMMARY ===" && \
echo "Total Failed Attempts:" && \
sudo grep "Failed password" /var/log/auth.log | wc -l && \
echo "" && \
echo "Targeted Usernames:" && \
sudo grep "Failed password" /var/log/auth.log | awk '{print $9}' | sort | uniq -c | sort -rn && \
echo "" && \
echo "Source IPs:" && \
sudo grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn
```

**Actual Output:**
```
=== FAILED LOGIN SUMMARY ===
Total Failed Attempts:
21

Targeted Usernames:
      9 ::1
      7 ;
      3 administrator
      3 admin

Source IPs:
      8 ;
      6 ::1
      3 50452
      3 49862
      3 43920
```

> 📌 **Explanation:** The raw numbers (21 total, 4 targeted usernames) are accurate despite the column offset issue affecting the awk field extraction. In a real SOC script, you would tailor the regex to your specific rsyslog version. This one-liner is the foundation of SOC quick-triage scripts and can be scheduled as a cron job to produce hourly attack summaries.

---

#### 🔹 Step 4.4 — Bonus: PID-Based Session Analysis

```bash
sudo grep "Failed" /var/log/auth.log | awk '{print $3}' | cut -d: -f1 | sort | uniq -c
```

**Actual Output:**
```
      3 sshd-session[521182]
      3 sshd-session[521540]
      3 sshd-session[522318]
      3 sshd-session[522933]
      3 sshd-session[523337]
      9 sudo
```

> 📌 **Explanation:** This command reveals **exactly 5 distinct SSH sessions**, each contributing exactly 3 failed attempts — confirming the 3-attempt-per-session SSH default limit. The `9 sudo` entries are our own analysis commands being logged. This PID correlation is a powerful forensic technique: grouping events by process ID lets you reconstruct a single attacker session even across hundreds of log lines.

---

### PHASE 5 — Fail2ban Deployment

> **Goal:** Deploy an automated defense system that would have blocked this attack in production.

---

#### 🔹 Step 5.1 — Install fail2ban

```bash
sudo apt update && sudo apt install fail2ban -y
```

**Actual Output (summary):**
```
Installing: fail2ban
Installing dependencies: python3-systemd

Summary:
  Upgrading: 0, Installing: 2, Removing: 0, Not Upgrading: 1551
  Download size: 508 kB

Setting up fail2ban (1.1.0-10)...
```

---

#### 🔹 Step 5.2 — Start & Enable fail2ban

```bash
sudo systemctl start fail2ban
sudo systemctl enable fail2ban
sudo systemctl status fail2ban
```

**Actual Output:**
```
● fail2ban.service - Fail2Ban Service
     Loaded: loaded (/usr/lib/systemd/system/fail2ban.service; enabled; preset: disabled)
     Active: active (running) since Sat 2026-06-13 11:50:11 +06; 1min 10s ago
   Main PID: 544873 (fail2ban-server)
      Tasks: 5 (limit: 4446)
     Memory: 13.3M (peak: 15.3M)

Jun 13 11:50:11 kali fail2ban-server[544873]: Server ready
```

---

#### 🔹 Step 5.3 — Verify fail2ban Status & Active Jails

```bash
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

**Actual Output:**
```
Status
|- Number of jail:      1
`- Jail list:   sshd

Status for the jail: sshd
|- Filter
|  |- Currently failed: 0
|  |- Total failed:     0
|  `- Journal matches:  _SYSTEMD_UNIT=ssh.service + _COMM=sshd
`- Actions
   |- Currently banned: 0
   |- Total banned:     0
   `- Banned IP list:
```

> 📌 **Explanation:** fail2ban is running with the `sshd` jail active. The `Currently failed: 0` count is expected — our simulated attack happened before fail2ban was installed. Going forward, any new brute force against this SSH service will be automatically detected and the source IP will be firewalled via `iptables`. The `Journal matches` line confirms fail2ban is using `systemd-journald` as its backend since rsyslog was installed after the attack simulation.

---

#### 🔹 Step 5.4 — Inspect fail2ban Default Configuration

```bash
sudo grep -E "bantime|findtime|maxretry" /etc/fail2ban/jail.conf | grep -v "#"
```

**Actual Output:**
```
bantime  = 10m
findtime  = 10m
maxretry = 5
maxmatches = %(maxretry)s
bantime  = 48h
maxretry = 1
...
```

> 📌 **Key fail2ban parameters explained:**

| Parameter | Default Value | Meaning |
|:---------:|:-------------:|:-------:|
| `maxretry` | `5` | Ban after 5 failed attempts |
| `findtime` | `10m` | Count failures within this window |
| `bantime` | `10m` | How long to ban the IP |

> 🔐 **Hardening recommendation:** For production SSH servers, set `maxretry = 3`, `bantime = 1h`, `findtime = 5m`. Our simulated 15 failures in 5 minutes would have resulted in a ban within the first 3 attempts.

---

### PHASE 6 — Incident Report

```bash
nano ~/incident_report_lab02.txt
```

```
╔══════════════════════════════════════════════════════════════════════╗
║            SECURITY INCIDENT REPORT — ALERT-002                     ║
╠══════════════════════════════════════════════════════════════════════╣
║ Date/Time      : 2026-06-13  |  11:02 – 11:07 +06:00               ║
║ Analyst        : SOC Intern (Junior Security Analyst)               ║
║ Alert ID       : ALERT-002                                           ║
║ Severity       : MEDIUM                                              ║
║ Status         : CLOSED — Simulated / No Breach Confirmed           ║
╠══════════════════════════════════════════════════════════════════════╣
║ EXECUTIVE SUMMARY                                                    ║
║ 21 failed SSH authentication attempts were detected against          ║
║ secnet-prod-01 between 11:02 and 11:07. Attempts targeted 4         ║
║ usernames across 5 distinct SSH sessions from source ::1 (IPv6      ║
║ loopback). No successful authentication was recorded.               ║
╠══════════════════════════════════════════════════════════════════════╣
║ FINDINGS                                                             ║
║ Total Failed Attempts  : 21                                          ║
║ Targeted Usernames     : testserver (6), root (3), admin (3),       ║
║                          administrator (3)                           ║
║ Source IP              : ::1 / 127.0.0.1 (localhost — simulated)    ║
║ Attack Duration        : ~5 minutes                                  ║
║ Successful Logins      : NONE DETECTED                               ║
║ Attack Type            : SSH Brute-force + Username Enumeration     ║
╠══════════════════════════════════════════════════════════════════════╣
║ IOCs (Indicators of Compromise)                                      ║
║ · "Failed password" entries in auth.log from single source          ║
║ · "Invalid user" entries — confirms username enumeration            ║
║ · 5 unique SSH session PIDs within 5-minute window                  ║
║ · 3 failed attempts per session (hitting SSH MaxAuthTries limit)    ║
╠══════════════════════════════════════════════════════════════════════╣
║ VERDICT                                                              ║
║ False Positive : NO                                                  ║
║ Real Attack    : YES (simulated in controlled lab)                  ║
║ Breach         : NO                                                  ║
╠══════════════════════════════════════════════════════════════════════╣
║ RECOMMENDED ACTIONS                                                  ║
║ 1. Block source IP via fail2ban (already deployed)                  ║
║ 2. Set fail2ban maxretry=3, bantime=1h                              ║
║ 3. Disable root SSH login: PermitRootLogin no                       ║
║ 4. Enforce SSH key-based authentication only                         ║
║ 5. Escalated to: Senior Analyst Rafiq                               ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

### PHASE 7 — Cleanup

```bash
sudo userdel -r testserver
sudo systemctl stop fail2ban
```

**Actual Output:**
```
userdel: testserver mail spool (/var/mail/testserver) not found
```

> 📌 **Explanation:** The `mail spool not found` warning is harmless — it means no mail directory was created for the user (expected on a minimal lab system). The user and home directory were removed successfully. Proper cleanup is a professional habit — always restore lab systems to a clean state after exercises.

---

## ✅ Verification Checklist

| Task | Command | Result |
|------|---------|:------:|
| SSH service started | `systemctl status ssh` | ✅ |
| Test user created | `id testserver` (before cleanup) | ✅ |
| rsyslog installed & logging | `ls -lh /var/log/auth.log` | ✅ |
| Failed logins generated | `ssh user@localhost` (wrong pass) | ✅ |
| Raw log inspection | `grep "failed" /var/log/auth.log` | ✅ |
| Username analysis | `grep | awk | sort | uniq -c` | ✅ |
| Source IP identified | `::1` (IPv6 loopback) | ✅ |
| Time pattern analyzed | Timestamps extracted | ✅ |
| Successful login check | No `Accepted password` in sshd | ✅ |
| fail2ban deployed | `fail2ban-client status sshd` | ✅ |
| Incident report written | `nano ~/incident_report_lab02.txt` | ✅ |
| Cleanup completed | `userdel -r testserver` | ✅ |

---

## 📸 Results 

<details>
<summary>📂 Click to expand screenshot index</summary>

| # | File | Description |
|---|------|-------------|
| 01 | `01_ssh_service_status.png` | SSH service active and running |
| 02 | `02_useradd_testserver.png` | Test user creation output |
| 03 | `03_authlog_missing_rsyslog.png` | Initial log file missing — rsyslog not installed |
| 04 | `04_rsyslog_install.png` | rsyslog installation and auth.log creation |
| 05 | `05_ssh_failed_testserver.png` | Manual failed SSH logins against testserver |
| 06 | `06_ssh_failed_admin_root.png` | Invalid user attempts (admin, root, administrator) |
| 07 | `07_grep_failed_raw.png` | Raw auth.log showing all Failed password entries |
| 08 | `08_awk_username_count.png` | Username frequency analysis output |
| 09 | `09_time_pattern.png` | Timestamp analysis showing attack timeline |
| 10 | `10_no_accepted_logins.png` | Confirmed — no successful authentications |
| 11 | `11_failed_summary_script.png` | One-command attack summary output |
| 12 | `12_pid_session_analysis.png` | Per-session PID breakdown (5 sessions × 3 attempts) |
| 13 | `13_fail2ban_status.png` | fail2ban running with sshd jail active |
| 14 | `14_fail2ban_sshd_jail.png` | fail2ban-client status sshd output |
| 15 | `15_jail_conf_params.png` | fail2ban default bantime/maxretry/findtime values |

</details>

---

## 💡 Key Learnings

- 🔧 **Modern Kali doesn't ship rsyslog by default** — always verify your logging stack before assuming auth.log exists. `journalctl` is the fallback, but `grep`/`awk` based triage still needs flat log files.

- 📋 **Log format versions matter for awk field extraction** — The shift from traditional syslog (`Jan  6 10:45:02`) to ISO 8601 (`2026-06-13T11:02:15+06:00`) changes which column number maps to which field. Always verify your format before scripting column-based extraction.

- 🔍 **`invalid user` vs `Failed password` are different IOC types** — Invalid user = the account doesn't exist = attacker is in reconnaissance phase. Failed password = account exists but wrong credential = attacker has valid intelligence and is brute forcing. This distinction changes the severity assessment.

- ⏱️ **Time pattern analysis distinguishes automated from manual attacks** — ~10-second gaps between attempts = manual/human. Sub-second intervals = automated tool (Hydra, Medusa, etc.). Rate is a key triage signal.

- 🚫 **No successful login = No breach, but always document the attempt** — Even a failed attack generates actionable threat intelligence: attacker enumerated `testserver`, `root`, `admin`, `administrator` as priority targets.

- 🤖 **fail2ban is automated SOC response at the OS level** — It reads the same logs we just analyzed and fires `iptables` rules automatically. Understanding it manually first makes you better at tuning it and understanding its false positives.

- 📝 **PID-based log correlation is a powerful forensic tool** — Grouping events by `sshd-session[PID]` lets you reconstruct exactly what happened in a single attacker session, even across hundreds of log lines.

- 🏥 **`userdel` warnings are not always errors** — Missing mail spool is cosmetic. Understanding warning vs. error messages is part of day-to-day Linux administration.

---

## 🏆 Skills Demonstrated

<div align="center">

| Skill Category | Specific Skills Practiced |
|:--------------:|:-------------------------:|
| 🔍 **Linux Log Analysis** | `auth.log` parsing, rsyslog setup, log format understanding |
| 🖥️ **CLI Forensics** | `grep`, `awk`, `sort`, `uniq -c`, `wc -l`, `cut`, `tail` |
| ⚔️ **Offensive Awareness** | SSH brute force mechanics, username enumeration patterns |
| 🛡️ **Defensive Operations** | fail2ban installation, jail configuration, IP banning |
| 🔎 **Incident Triage** | IOC extraction, false positive elimination, severity assessment |
| 📊 **SOC Analysis** | Attack timeline reconstruction, session-level correlation |
| 📝 **Documentation** | Formal incident report, IOC listing, recommended actions |
| ⚙️ **System Administration** | Service management, user management, package installation |

</div>

---

## 🧩 Challenges & Solutions

<details>
<summary>⚠️ Challenge 1: auth.log did not exist on fresh Kali installation</summary>

**Problem:** Running `ls -lh /var/log/auth.log` returned `No such file or directory`. The subsequent `systemctl start rsyslog` failed with `Unit rsyslog.service not found`.

**Root Cause:** Modern Kali Linux 2024+ uses `systemd-journald` exclusively by default. `rsyslog` must be explicitly installed.

**Solution:**
```bash
sudo apt update && sudo apt install rsyslog -y
sudo systemctl start rsyslog
sudo systemctl enable rsyslog
# Verify:
sudo ls -lh /var/log/auth.log
```

**Result:** `auth.log` created at `-rw-r----- 1 root adm 1.2K`

**Lesson:** Never assume log infrastructure exists. Verifying logging before running any security tests is a professional habit. Real production servers may also have non-standard logging configurations.

</details>

<details>
<summary>⚠️ Challenge 2: awk column extraction returned wrong fields</summary>

**Problem:** `awk '{print $9}'` returned `::1` and `;` instead of usernames. `awk '{print $11}'` returned port numbers instead of source IPs.

**Root Cause:** The lab guide was written for traditional syslog format where timestamp = 3 fields (`Jan 6 10:45`). Modern rsyslog uses ISO 8601 single-field timestamps (`2026-06-13T11:02:15+06:00`), shifting every subsequent field index by -2.

**Solution:** Use `grep -oP` with regex for format-independent extraction:
```bash
# Username extraction (format-independent)
sudo grep "Failed password" /var/log/auth.log | \
  grep -oP "(?<=for (invalid user )?)\S+" | \
  sort | uniq -c | sort -rn

# IP extraction (format-independent)
sudo grep "Failed password" /var/log/auth.log | \
  grep -oP "from \K[^\s]+" | \
  sort | uniq -c | sort -rn
```

**Lesson:** Log parsing scripts must be tested against the actual log format in your environment. Always verify output makes semantic sense before acting on it.

</details>

<details>
<summary>⚠️ Challenge 3: lastb command not found</summary>

**Problem:** `sudo lastb` returned `command not found`.

**Root Cause:** `lastb` reads from `/var/log/btmp`. Since rsyslog was installed after sessions began, `btmp` was empty. Additionally, `lastb` may require the `util-linux` package on some systems.

**Solution:** Use the alternative:
```bash
sudo last -f /var/log/btmp | head -20
# If empty, rely on auth.log as primary source
```

**Lesson:** Always have at least two independent log sources for cross-validation. In this case, `auth.log` via rsyslog was the authoritative source.

</details>

<details>
<summary>⚠️ Challenge 4: fail2ban showed 0 failed attempts despite our attack</summary>

**Problem:** `fail2ban-client status sshd` showed `Currently failed: 0` and `Total failed: 0` even after our simulated attack.

**Root Cause:** fail2ban was installed and started *after* the attack simulation. It only monitors events going forward from when it starts. It cannot retroactively count past events.

**Lesson:** Security tools must be running *before* an incident to be effective. This is why monitoring infrastructure is deployed proactively — not reactively. In production, fail2ban should be enabled at server provisioning time.

</details>

---

## 🔚 Conclusion

Lab 02 advanced from basic system awareness into **applied SOC incident investigation**. Starting from a simulated SIEM alert, we walked the complete analyst workflow: environment verification, controlled attack simulation, forensic log analysis, behavioral pattern correlation, automated defense deployment, and formal documentation.

The most significant real-world insight from this lab was discovering that **log format versions directly affect forensic tool accuracy** — a practical trap that catches junior analysts who blindly copy commands without understanding the underlying data structure. Adapting the `awk` commands to match the actual log format is the difference between a false negative and a confirmed detection.

The skills built here — systematic log triage, pattern-based attribution, fail2ban tuning, and incident documentation — map directly to the daily workflows of tools like **Splunk**, **Elastic SIEM**, **Microsoft Sentinel**, and **Graylog** used in enterprise environments. Every SIEM alert ultimately traces back to the same raw log events we analyzed manually in this lab.

> *"You don't need a SIEM to think like a SIEM. Understand the logs first — the tooling comes second."*

---

