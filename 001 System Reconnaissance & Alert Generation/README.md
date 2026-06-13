
# 🛡️ Lab 01 — System Reconnaissance & Alert Generation

<div align="center">

![Linux](https://img.shields.io/badge/Platform-Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Category](https://img.shields.io/badge/Category-Offensive_%26_Defensive_Security-red?style=for-the-badge)
![Difficulty](https://img.shields.io/badge/Difficulty-Beginner_Friendly-brightgreen?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Lab Series](https://img.shields.io/badge/Series-Linux_Security_Labs-blueviolet?style=for-the-badge)

> **"You can't defend what you don't understand. First, learn the attack."**
> — SOC Team Onboarding Manual, SecureNet Solutions

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Scenario](#-scenario)
- [Objectives](#-objectives)
- [Tools & Technologies](#-tools--technologies)
- [Lab Environment](#-lab-environment)
- [Step-by-Step Implementation](#-step-by-step-implementation)
  - [Phase 1 — System Reconnaissance](#phase-1--system-reconnaissance)
  - [Phase 2 — Suspicious Activity Simulation](#phase-2--suspicious-activity-simulation)
  - [Phase 3 — Log Detection & Analysis](#phase-3--log-detection--analysis)
  - [Phase 4 — Incident Writeup](#phase-4--incident-writeup)
- [Results & Screenshots](#-results--screenshots)
- [Key Learnings](#-key-learnings)
- [Skills Demonstrated](#-skills-demonstrated)
- [Challenges & Solutions](#-challenges--solutions)
- [Conclusion](#-conclusion)

---

## 🔍 Overview

This lab simulates a **real-world SOC (Security Operations Center) analyst workflow** — starting from system reconnaissance, generating a brute-force-style suspicious activity event, and tracking it through Linux authentication logs. 

The lab is structured around a fictional corporate onboarding scenario at **SecureNet Solutions**, where a Junior Security Analyst must perform their first hands-on system investigation. The goal is to build foundational skills in **threat simulation, log forensics, and incident documentation** — core competencies for any entry-level Blue Team or SOC role.

| Property | Details |
|---|---|
| **Lab Series** | Linux Security Labs |
| **Lab Number** | 01 |
| **Category** | System Reconnaissance + Brute Force Detection |
| **Difficulty** | Beginner |
| **Time Required** | ~60–90 minutes |
| **Platform** | Kali Linux (Single VM) |
| **Focus Area** | Authentication Logs, User Enumeration, Incident Response |

---

## 🏢 Scenario

> **Company:** SecureNet Solutions  
> **Role:** Junior Security Analyst (SOC Team)  
> **Supervisor:** Senior Analyst — Rafiq (Team Lead)

It's your **first day** at SecureNet Solutions' Security Operations Center. Your Senior Analyst, Rafiq, pulls you aside before the morning briefing:

> *"Welcome to the team. Before we throw you into live alerts, I want you to get familiar with our Linux servers from the ground up. Today's task: fingerprint the system, understand who's on it, what's running — then I want you to manually simulate a suspicious login activity so you can see exactly how it appears in our logs. Every analyst needs to understand both sides — the attack and the detection."*

Your mission is to:
1. Perform a full **system reconnaissance** of the Linux server
2. **Simulate a brute-force-style login failure** scenario
3. **Hunt the evidence** in authentication logs
4. Write a **mini incident report** documenting your findings

---

## 🎯 Objectives

- [x] Take the system's **basic fingerprint** (OS, hostname, kernel, uptime)
- [x] Enumerate **active users and running processes**
- [x] Simulate a **suspicious authentication activity** (repeated failed logins)
- [x] **Detect the event** through log analysis (`/var/log/auth.log`)
- [x] Produce a **structured incident writeup** documenting the finding

---

## 🛠️ Tools & Technologies

| Tool / Command | Purpose |
|---|---|
| `uname`, `hostname`, `uptime` | System fingerprinting |
| `who`, `w`, `last`, `id` | User enumeration & session info |
| `ps`, `top`, `htop` | Process reconnaissance |
| `su`, `sudo` | Authentication simulation (failed logins) |
| `/var/log/auth.log` | Authentication log analysis |
| `grep`, `tail`, `cat`, `awk` | Log parsing and filtering |
| `journalctl` | Systemd journal query |
| Kali Linux Terminal | Primary attack & analysis environment |

---

## 💻 Lab Environment

```
┌─────────────────────────────────────────┐
│           LAB TOPOLOGY                  │
│                                         │
│   ┌─────────────────────────────────┐   │
│   │        Kali Linux VM            │   │
│   │   (Attacker + Defender role)    │   │
│   │                                 │   │
│   │  - System Recon Tools           │   │
│   │  - Auth Log: /var/log/auth.log  │   │
│   │  - Simulated Failed Logins      │   │
│   └─────────────────────────────────┘   │
│                                         │
│   [ Single VM — No network needed ]     │
└─────────────────────────────────────────┘
```

> **Note:** This lab runs entirely on a **single Kali Linux VM**. No additional machines, networks, or external tools required.

---

## 🚀 Step-by-Step Implementation

---

### PHASE 1 — System Reconnaissance

> **Goal:** Understand the target system — just like a real attacker (or defender) would before taking any action.

---

#### Step 1.1 — OS & Kernel Fingerprinting

```bash
uname -a
```

**Sample Output:**
```
Linux kali 6.1.0-kali9-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.27-1kali1 (2023-05-12) x86_64 GNU/Linux
```

> **Explanation:** `uname -a` reveals the **kernel version**, **architecture (x86_64)**, and **OS build date**. In a real engagement, this tells an attacker which kernel exploits may apply. As a defender, you baseline this to detect unauthorized kernel changes.

---

#### Step 1.2 — Hostname & Network Identity

```bash
hostname
hostname -I
cat /etc/hostname
```

**Sample Output:**
```
kali
192.168.1.105
kali
```

> **Explanation:** Identifies the machine's network identity. In a SOC context, hostnames are critical for correlating alerts across multiple log sources (SIEM, IDS, firewall).

---

#### Step 1.3 — System Uptime & Load

```bash
uptime
```

**Sample Output:**
```
 10:35:22 up 2:14,  1 user,  load average: 0.12, 0.08, 0.05
```

> **Explanation:** High uptime suggests a production server. Sudden reboots are a red flag for rootkit installation or system tampering. Load averages indicate resource usage — unusually high load can signal crypto-mining malware or active exploitation.

---

#### Step 1.4 — Active User Enumeration

```bash
who
w
last | head -20
cat /etc/passwd | grep -v nologin | grep -v false
```

**Sample Output (`who`):**
```
kali     tty7         2024-01-15 08:21 (:0)
kali     pts/0        2024-01-15 10:30 (:0.0)
```

**Sample Output (`w`):**
```
 10:35:22 up 2:14,  1 user,  load average: 0.12, 0.08, 0.05
USER     TTY      FROM             LOGIN@   IDLE JCPU PCPU WHAT
kali     tty7     :0               08:21    2:14m  0.32s  0.04s /usr/bin/startplasma-x11
kali     pts/0    :0.0             10:30    0.00s  0.04s  0.00s w
```

**Sample Output (`/etc/passwd` filter):**
```
root:x:0:0:root:/root:/bin/bash
kali:x:1000:1000:Kali,,,:/home/kali:/bin/bash
```

> **Explanation:** `who` and `w` show **currently logged-in users** and their active commands. `last` shows **login history** including failed attempts. Parsing `/etc/passwd` reveals all accounts with valid shells — a key step in privilege escalation recon.

---

#### Step 1.5 — Process Reconnaissance

```bash
ps aux --sort=-%cpu | head -20
ps aux | grep -E "(ssh|sudo|cron|bash)" 
```

**Sample Output:**
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1  22596  8432 ?        Ss   08:21   0:00 /sbin/init
root       512  0.0  0.0  14864  2156 ?        Ss   08:21   0:00 /usr/sbin/sshd -D
kali      1842  0.0  0.1  14632  5432 pts/0    Ss   10:30   0:00 bash
```

> **Explanation:** Identifies suspicious processes. An analyst should watch for unexpected `netcat`, `nmap`, `python -c`, or `wget` commands in the process list — common indicators of compromise. The `sshd` daemon being active confirms remote access is possible.

---

### PHASE 2 — Suspicious Activity Simulation

> **Goal:** Simulate repeated failed authentication attempts to generate brute-force-like log entries — the same pattern an attacker would produce.

---

#### Step 2.1 — Enable Authentication Logging (Verify)

```bash
# Check if auth.log exists and is being written to
ls -lh /var/log/auth.log
tail -5 /var/log/auth.log
```

**Sample Output:**
```
-rw-r----- 1 root adm 24K Jan 15 10:35 /var/log/auth.log
Jan 15 10:35:01 kali CRON[1923]: pam_unix(cron:session): session opened for user root
```

> **Explanation:** Confirms the auth log is active and accessible. `pam_unix` entries come from PAM (Pluggable Authentication Modules) — the gatekeeper for all Linux logins.

---

#### Step 2.2 — Simulate Failed Login Attempts (Brute Force Pattern)

```bash
# Open a second terminal and run repeated failed su attempts
# This simulates what a brute-force attack looks like in logs

for i in {1..5}; do
    su - nonexistentuser 2>/dev/null
    sleep 1
done
```

**Alternative — Manual Method:**
```bash
# Attempt to switch to root with wrong password (repeat 5 times manually)
su - root
# Enter: wrongpassword123
# Enter: password
# Enter: admin
# Enter: toor
# Enter: root123
```

**Expected Terminal Output:**
```
Password: 
su: Authentication failure
Password: 
su: Authentication failure
Password: 
su: Authentication failure
```

> **Explanation:** Each failed `su` attempt generates a PAM authentication failure log entry. Five rapid consecutive failures from the same TTY simulates the **pattern signature of a brute-force attack**. Real brute-force tools like `hydra` produce thousands of such entries per minute via SSH.

---

#### Step 2.3 — Simulate SSH Failed Logins (Optional — Advanced)

```bash
# Simulate failed SSH login to localhost (requires sshd running)
sudo systemctl start ssh

for i in {1..3}; do
    ssh fakeuser@127.0.0.1 2>/dev/null
    sleep 2
done
```

**Sample Output:**
```
ssh: connect to host 127.0.0.1 port 22: Connection refused
# OR
fakeuser@127.0.0.1: Permission denied (publickey,password).
```

> **Explanation:** SSH failed logins are the most common brute-force vector in the real world. This generates `sshd` auth failure entries that a SIEM would flag as suspicious — especially if they originate from a foreign IP.

---

### PHASE 3 — Log Detection & Analysis

> **Goal:** Hunt the evidence. Find the simulated attack in the authentication logs using analyst techniques.

---

#### Step 3.1 — Real-Time Log Monitoring

```bash
# Watch auth.log in real-time during the simulation
sudo tail -f /var/log/auth.log
```

**Sample Output (during failed su attempts):**
```
Jan 15 10:42:11 kali su[2341]: FAILED SU (to root) kali on pts/0
Jan 15 10:42:14 kali su[2345]: FAILED SU (to root) kali on pts/0
Jan 15 10:42:16 kali su[2349]: FAILED SU (to root) kali on pts/0
Jan 15 10:42:19 kali su[2353]: FAILED SU (to root) kali on pts/0
Jan 15 10:42:22 kali su[2357]: FAILED SU (to root) kali on pts/0
```

> **Explanation:** `tail -f` is the SOC analyst's real-time monitoring command. Every failed login attempt is timestamped with the PID, source user, and TTY. Five failures in 11 seconds — this is **exactly what a brute-force pattern looks like** in logs.

---

#### Step 3.2 — Grep-Based IOC Hunting

```bash
# Hunt for all authentication failures
sudo grep "FAILED\|failure\|invalid\|Failed" /var/log/auth.log

# Filter specifically for su failures
sudo grep "FAILED SU" /var/log/auth.log

# Count total failure events (quick severity gauge)
sudo grep -c "FAILED\|failure" /var/log/auth.log

# Extract unique source usernames from failures
sudo grep "FAILED SU" /var/log/auth.log | awk '{print $NF}' | sort | uniq -c | sort -rn
```

**Sample Output:**
```
5

      5 pts/0
```

> **Explanation:** `grep -c` gives an **instant count** of suspicious events — critical for triaging alert volume. The `awk` pipeline extracts the TTY/source, allowing us to identify if failures came from a single source (focused attack) or many (distributed attack).

---

#### Step 3.3 — Time-Windowed Analysis

```bash
# Get failures within a specific time window
sudo grep "Jan 15 10:4" /var/log/auth.log | grep -i "fail\|FAILED"

# Use journalctl for more powerful time-based queries
sudo journalctl _COMM=su --since "10 minutes ago" --no-pager

# Show authentication events from last hour
sudo journalctl -u ssh --since "1 hour ago" --no-pager
```

**Sample Output (`journalctl`):**
```
Jan 15 10:42:11 kali su[2341]: FAILED SU (to root) kali on pts/0
Jan 15 10:42:14 kali su[2345]: FAILED SU (to root) kali on pts/0
Jan 15 10:42:16 kali su[2349]: FAILED SU (to root) kali on pts/0
Jan 15 10:42:19 kali su[2353]: FAILED SU (to root) kali on pts/0
Jan 15 10:42:22 kali su[2357]: FAILED SU (to root) kali on pts/0
```

> **Explanation:** `journalctl` is the **modern log query interface** for systemd-based systems. In a real SOC, you'd feed this data into a SIEM (like Splunk or Elastic) — but manually querying with time windows builds the same analytical intuition.

---

#### Step 3.4 — PAM & SSH Log Deep-Dive

```bash
# Check PAM authentication messages
sudo grep "pam_unix" /var/log/auth.log | grep "authentication failure" | tail -20

# Check for any successful logins after failures (potential compromise indicator)
sudo grep "Accepted\|session opened" /var/log/auth.log | tail -10

# Check for any account lockout triggers
sudo grep "account locked\|too many failures" /var/log/auth.log
```

**Sample Output:**
```
Jan 15 10:42:11 kali su[2341]: pam_unix(su:auth): authentication failure; logname=kali uid=1000 euid=0 tty=pts/0 ruser=kali rhost=  user=root
Jan 15 10:42:14 kali su[2345]: pam_unix(su:auth): authentication failure; logname=kali uid=1000 euid=0 tty=pts/0 ruser=kali rhost=  user=root
```

> **Explanation:** PAM log entries contain **rich forensic metadata** — the source UID (`uid=1000`), target user (`user=root`), and terminal (`tty=pts/0`). This is exactly the data a threat hunter uses to reconstruct an attack timeline.

---

### PHASE 4 — Incident Writeup

> **Goal:** Document the finding in a structured format — the final deliverable of every SOC investigation.

---

#### Incident Report Template

```
=======================================================
         INCIDENT REPORT — SecureNet Solutions
=======================================================
Analyst      : [Your Name]
Date         : 2024-01-15
Severity     : LOW (Simulated / Lab Environment)
Status       : Resolved

---[ INCIDENT SUMMARY ]---
At 10:42 on January 15, 2024, five consecutive authentication
failures for the root account were detected on host 'kali'
originating from TTY pts/0 under user account 'kali'.

---[ INDICATORS OF COMPROMISE (IOCs) ]---
  Type       : Authentication Failure
  Source     : kali (uid=1000) on pts/0
  Target     : root (uid=0)
  Timestamp  : 10:42:11 – 10:42:22 (11-second window)
  Count      : 5 failed attempts
  Tool/Vector: su command (simulated brute force)

---[ LOG EVIDENCE ]---
  File       : /var/log/auth.log
  Key Entry  : "FAILED SU (to root) kali on pts/0"
  PIDs       : 2341, 2345, 2349, 2353, 2357

---[ ANALYSIS ]---
  Pattern    : 5 failures in 11 seconds — consistent with
               automated brute force tooling behavior.
  Risk       : Low in this context (local TTY, known user).
               HIGH risk if observed over SSH from external IP.

---[ RECOMMENDATIONS ]---
  1. Implement account lockout after 3 failed attempts
     (/etc/pam.d/common-auth — pam_tally2 or faillock)
  2. Alert on >3 auth failures within 60 seconds (SIEM rule)
  3. Restrict SSH root login (PermitRootLogin no in sshd_config)
  4. Deploy Fail2Ban for automated IP blocking

---[ STATUS ]---
  No actual compromise detected. Activity confirmed as
  authorized lab simulation. Logs retained for training.
=======================================================
```

---

## 📸 Results 

> The following table summarizes the key evidence captured during this lab. Replace placeholder descriptions with actual screenshots from your terminal session.

| # | Evidence | Command Used | What It Shows |
|---|---|---|---|
| 1 | System Fingerprint | `uname -a`, `hostname` | Kernel version, OS identity |
| 2 | Active Users | `who`, `w` | Live session info |
| 3 | Process List | `ps aux` | Running services (sshd, bash) |
| 4 | Failed Login Simulation | `su - root` (×5) | Authentication failure triggers |
| 5 | Real-Time Log Detection | `tail -f /var/log/auth.log` | Live log entries during attack |
| 6 | Grep IOC Hunt | `grep "FAILED SU" auth.log` | Extracted attack indicators |
| 7 | PAM Deep-Dive | `grep "pam_unix"` | Forensic metadata in logs |
| 8 | Incident Report | Manual writeup | Structured analyst documentation |


---

## 🧠 Key Learnings

- **Linux auth.log is a goldmine** — every failed login, sudo attempt, and session open/close is recorded with timestamps, UIDs, and TTY info
- **PAM (Pluggable Authentication Modules)** is the core authentication layer on Linux; understanding its log format is essential for any SOC analyst
- **Pattern recognition matters more than single events** — 5 failures in 11 seconds is suspicious; 1 failure in a day is normal
- **`journalctl` vs `/var/log/auth.log`** — modern systems use systemd journals; older ones use flat log files; analysts must know both
- **System reconnaissance is the first step of BOTH attack and defense** — the same `uname`, `who`, `ps` commands used in recon are used in incident response
- **Incident documentation is a core skill** — finding the attack means nothing if you can't communicate it clearly in writing
- **Local brute force vs remote brute force** — local TTY-based failures are lower risk; SSH-based failures from external IPs are critical priority
- **`grep`, `awk`, `tail -f`** are the analyst's primary tools before investing in SIEM infrastructure

---

## ✅ Skills Demonstrated

<details>
<summary><b>🔵 Blue Team / Defensive Skills</b></summary>

| Skill | Details |
|---|---|
| Log Analysis | Parsed `/var/log/auth.log` and `journalctl` for authentication events |
| IOC Identification | Extracted Indicators of Compromise from raw log data |
| Incident Documentation | Produced structured incident report with timeline, evidence, and recommendations |
| Threat Detection | Identified brute-force pattern from authentication failure bursts |
| PAM Understanding | Interpreted PAM (`pam_unix`) log entries for forensic context |

</details>

<details>
<summary><b>🔴 Red Team / Offensive Awareness</b></summary>

| Skill | Details |
|---|---|
| System Reconnaissance | OS fingerprinting, user enumeration, process discovery |
| Attack Simulation | Replicated brute-force authentication failure pattern |
| Log Evasion Awareness | Understood how attack patterns appear from attacker's perspective |

</details>

<details>
<summary><b>⚙️ Technical / Linux Skills</b></summary>

| Skill | Details |
|---|---|
| Linux CLI Proficiency | Advanced use of `grep`, `awk`, `tail`, `ps`, `who`, `last`, `journalctl` |
| Service Management | Verified and managed `sshd` with `systemctl` |
| File System Navigation | Located, read, and filtered critical system log files |
| Scripting (Bash) | Used `for` loops for activity simulation |

</details>

---

## ⚠️ Challenges & Solutions

| # | Challenge | Solution |
|---|---|---|
| 1 | `auth.log` not present on some Kali installations | Used `journalctl -u ssh` and `journalctl _COMM=su` as alternative; also ran `sudo systemctl restart rsyslog` to regenerate the log file |
| 2 | `su` failures not generating enough log entries for pattern recognition | Wrote a `for` loop to rapidly simulate 5 consecutive failures in a controlled manner |
| 3 | SSH daemon not running for SSH simulation phase | Ran `sudo systemctl start ssh` to enable it for the lab duration |
| 4 | Log output too noisy (too many unrelated entries) | Used precise `grep` patterns (`"FAILED SU"`, `"pam_unix.*failure"`) to filter only relevant events |
| 5 | Unsure what "normal" log entries look like vs "suspicious" | Baselining: read 30 minutes of normal logs first, then simulated activity — the anomaly was immediately visible |

---

## 🏁 Conclusion

This lab established the **foundational analyst workflow** that underpins nearly every SOC investigation:

```
Reconnaissance → Activity Simulation → Log Detection → Documentation
```

The most important takeaway isn't any single command — it's the **mindset shift**: a security analyst must think like an attacker to defend effectively. Understanding exactly which log entries a `su` brute-force generates, and *why* they look the way they do (PAM layer, PID assignment, TTY attribution), is what separates a **competent SOC analyst** from someone who just runs `grep`.

In production environments, this same workflow scales up:
- **`grep` → Splunk/Elastic SIEM queries**
- **Manual log review → Automated alerting rules**
- **Single host → Fleet-wide log aggregation**
- **Incident report → Formal DFIR (Digital Forensics & Incident Response) documentation**

The fundamentals built here are **directly transferable** to enterprise security operations.

---


