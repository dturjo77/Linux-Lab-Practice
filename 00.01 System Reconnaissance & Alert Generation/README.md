# 🔐 Linux Security Labs

<div align="center">

![Security](https://img.shields.io/badge/Category-Cybersecurity-red?style=for-the-badge&logo=shield&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20Kali-blue?style=for-the-badge&logo=linux&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner--Intermediate-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![Role](https://img.shields.io/badge/Role-SOC%20Analyst-orange?style=for-the-badge&logo=security&logoColor=white)

</div>

---

# 🛡️ Lab 01 — Brute Force Attack Simulation & Detection

> **SOC Intern Onboarding Series** | System Reconnaissance & Suspicious Activity Alert Generation

<div align="center">

| 📅 Lab Date | 🎯 Difficulty | ⏱️ Duration | 🖥️ Environment |
|:-----------:|:-------------:|:-----------:|:---------------:|
| 2025 | Beginner | ~60 mins | Kali Linux VM |

</div>

---

## 📌 Table of Contents

- [Description](#-description)
- [Scenario](#-scenario)
- [Objectives](#-objectives)
- [Tools & Technologies](#️-tools--technologies)
- [Step-by-Step Implementation](#-step-by-step-implementation)
  - [Phase 1 — System Reconnaissance](#phase-1--system-reconnaissance)
  - [Phase 2 — User & Process Enumeration](#phase-2--user--process-enumeration)
  - [Phase 3 — Simulating Suspicious Activity](#phase-3--simulating-suspicious-activity)
  - [Phase 4 — Log Analysis & Detection](#phase-4--log-analysis--detection)
  - [Phase 5 — Incident Writeup](#phase-5--incident-writeup)
- [Results & Screenshots](#-results--screenshots)
- [Key Learnings](#-key-learnings)
- [Skills Demonstrated](#-skills-demonstrated)
- [Challenges & Solutions](#-challenges--solutions)
- [Conclusion](#-conclusion)

---

## 📖 Description

This lab is the **first entry** in the *Linux Security Labs* series, designed to simulate real-world SOC (Security Operations Center) workflows. As part of a structured intern onboarding exercise, this lab covers Linux system fingerprinting, active user and process enumeration, suspicious activity simulation (brute force attempt), and detection through log analysis.

The lab follows an **attacker-defender mindset** — first understanding the environment from a defender's perspective, then deliberately triggering an alert to practice detection techniques used by junior security analysts in enterprise environments.

> 💡 **No extra virtual machines required.** All activities are performed on a single Kali Linux VM, making this lab fully self-contained and beginner-friendly.

---

## 🎭 Scenario

> *You have just joined **SecureNet Solutions** as a Junior Security Analyst in their SOC team. On your first day, your Senior Analyst walks over to your desk.*

---

**Senior Analyst (Rafiq):** *"Welcome to the team. Before we throw you into live alerts, you need to understand the battlefield. Today's task: get familiar with our Linux server. I want to know — who's logged in, what's running, and what's suspicious. Then we're going to manually trigger a brute force attempt and I want you to detect it from the logs. Think of it as your first real drill."*

---

You open your terminal. The clock is ticking. Your job: **reconnaissance first, detection second.**

This is a controlled simulation of what SOC analysts do every day — understand normal system behavior so that *abnormal* behavior stands out immediately.

---

## 🎯 Objectives

| # | Objective | Status |
|---|-----------|--------|
| 1 | Perform basic system fingerprinting (OS, hostname, kernel) | ✅ |
| 2 | Identify active users and running processes | ✅ |
| 3 | Simulate a suspicious brute force activity | ✅ |
| 4 | Detect the activity using system logs | ✅ |
| 5 | Produce a mini incident report / writeup | ✅ |

---

## 🛠️ Tools & Technologies

<div align="center">

| Tool / Technology | Purpose | Category |
|:-----------------:|:-------:|:--------:|
| 🐉 **Kali Linux** | Primary lab environment | Operating System |
| 🖥️ **Bash / Terminal** | Command execution | Interface |
| 🔍 **`who`, `w`, `id`** | User enumeration | Recon |
| ⚙️ **`ps`, `top`, `netstat`** | Process & network enumeration | Recon |
| 🔐 **`hydra` / `ssh`** | Brute force simulation | Attack Simulation |
| 📋 **`/var/log/auth.log`** | Authentication log analysis | Detection |
| 🔎 **`grep`, `awk`, `tail`** | Log parsing & filtering | DFIR |
| 📝 **`journalctl`** | Systemd log viewer | Detection |

</div>

---

## 🚀 Step-by-Step Implementation

---

### PHASE 1 — System Reconnaissance

> **Goal:** Build a complete fingerprint of the target Linux system before any action is taken. This is equivalent to an attacker performing initial enumeration — but here, we do it as defenders to understand our own environment.

---

#### 🔹 Step 1.1 — Identify Hostname & OS Information

```bash
hostname
uname -a
cat /etc/os-release
```

**Sample Output:**
```
kali
Linux kali 6.1.0-kali9-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.27-1kali1 x86_64 GNU/Linux

PRETTY_NAME="Kali GNU/Linux Rolling"
NAME="Kali GNU/Linux"
ID=kali
VERSION_ID="2024.1"
```

> 📌 **Explanation:** `uname -a` reveals the kernel version, architecture (`x86_64`), and build date. This is critical for identifying potential kernel exploits. `cat /etc/os-release` confirms the exact OS distribution and version — key information in any reconnaissance phase.

---

#### 🔹 Step 1.2 — Check System Uptime & Load

```bash
uptime
last reboot
```

**Sample Output:**
```
10:34:21 up 2:13,  1 user,  load average: 0.12, 0.08, 0.05

reboot   system boot  6.1.0-kali9-amd  Mon Jan  6 08:21   still running
```

> 📌 **Explanation:** Uptime indicates how long the system has been running since its last boot. High load averages can signal resource abuse (e.g., crypto mining, brute force scripts). `last reboot` provides a reboot history — unexpected reboots are a red flag for incident responders.

---

#### 🔹 Step 1.3 — Discover Network Interfaces & IP Address

```bash
ip a
ip route
```

**Sample Output:**
```
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP
    inet 192.168.1.105/24 brd 192.168.1.255 scope global eth0

default via 192.168.1.1 dev eth0 proto dhcp
```

> 📌 **Explanation:** Knowing the IP address and active interfaces is essential before simulating network-based attacks. In a real SOC investigation, IP address correlation is used to track malicious connections in firewall and SIEM logs.

---

### PHASE 2 — User & Process Enumeration

> **Goal:** Identify who is on the system and what's running. An analyst must know the baseline to identify anomalies.

---

#### 🔹 Step 2.1 — Check Currently Logged-In Users

```bash
who
w
```

**Sample Output:**
```
kali     tty7         2024-01-06 08:21 (:0)
kali     pts/0        2024-01-06 10:22 (192.168.1.1)

USER     TTY      FROM             LOGIN@   IDLE JCPU PCPU WHAT
kali     tty7     :0               08:21   2:13m  0.08s  0.04s /usr/bin/startx
kali     pts/0    192.168.1.1      10:22   0.00s  0.02s  0.00s w
```

> 📌 **Explanation:** `w` is more informative than `who` — it shows the source IP of remote sessions, process being run, and idle time. A login from an unexpected IP or at an unusual time is an immediate red flag for SOC analysts.

---

#### 🔹 Step 2.2 — Enumerate All User Accounts

```bash
cat /etc/passwd | grep -v nologin | grep -v false
getent passwd {1000..1100}
```

**Sample Output:**
```
root:x:0:0:root:/root:/bin/bash
kali:x:1000:1000:Kali,,,:/home/kali:/bin/bash
```

> 📌 **Explanation:** Filtering out service accounts (nologin/false shells) helps identify real human user accounts. In a real incident, unknown accounts with valid shells are prime indicators of persistence mechanisms left by attackers.

---

#### 🔹 Step 2.3 — Identify Running Processes

```bash
ps aux --sort=-%cpu | head -20
ps aux | grep -i suspicious
```

**Sample Output:**
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 168332 11268 ?        Ss   08:21   0:02 /sbin/init
kali      1842  0.0  0.3 612440 28512 tty7     Sl+  08:21   0:04 /usr/bin/Xorg
kali      2201  0.0  0.1  23376  9756 pts/0    Ss   10:22   0:00 bash
```

> 📌 **Explanation:** Sorting by CPU usage quickly surfaces resource-hungry processes. In a real investigation, unexpected processes owned by non-root users running as root, or processes with random/obfuscated names, are strong indicators of compromise.

---

#### 🔹 Step 2.4 — Check Open Network Connections

```bash
ss -tulnp
netstat -antp 2>/dev/null
```

**Sample Output:**
```
Netid  State   Recv-Q Send-Q  Local Address:Port   Peer Address:Port
tcp    LISTEN  0      128     0.0.0.0:22            0.0.0.0:*    users:(("sshd",pid=823))
tcp    LISTEN  0      5       127.0.0.1:631         0.0.0.0:*    users:(("cupsd",pid=756))
```

> 📌 **Explanation:** `ss -tulnp` reveals all listening ports and the processes bound to them. Seeing SSH (`port 22`) listening is expected; seeing an unknown service on a high port would be suspicious. This forms the **baseline** before we simulate the attack.

---

### PHASE 3 — Simulating Suspicious Activity

> **Goal:** Deliberately trigger brute force login attempts against the local SSH service so that we can practice detection from the defender's perspective.

---

#### 🔹 Step 3.1 — Verify SSH Service Is Running

```bash
sudo systemctl status ssh
sudo systemctl start ssh  # if not running
```

**Sample Output:**
```
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled)
     Active: active (running) since Sat 2024-01-06 08:21:14 EST; 2h 14min ago
   Main PID: 823 (sshd)
```

> 📌 **Explanation:** Before launching a brute force simulation, confirming SSH is active and listening is essential. In a real penetration test, this step is part of service enumeration.

---

#### 🔹 Step 3.2 — Simulate Failed SSH Login Attempts (Manual Method)

```bash
# Simulate multiple failed SSH logins from the terminal
for i in {1..10}; do
    ssh wronguser@localhost 2>/dev/null || true
done
```

**Sample Output:**
```
ssh: connect to host localhost port 22: Connection refused
# (Or if SSH is running:)
wronguser@localhost: Permission denied (publickey,password).
wronguser@localhost: Permission denied (publickey,password).
wronguser@localhost: Permission denied (publickey,password).
... [repeated 10 times]
```

> 📌 **Explanation:** This loop deliberately generates failed authentication attempts — exactly what a brute force attack looks like. Each failed attempt gets recorded in `/var/log/auth.log`, which we will analyze in Phase 4.

---

#### 🔹 Step 3.3 — Using Hydra for Brute Force Simulation (Advanced)

```bash
# Create a small test wordlist
echo -e "password\n123456\nwrongpass\nadmin\ntest123" > /tmp/testlist.txt

# Run Hydra against local SSH (simulation only — controlled environment)
hydra -l kali -P /tmp/testlist.txt ssh://127.0.0.1 -t 4 -V
```

**Sample Output:**
```
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak

[DATA] max 4 tasks per 1 server, overall 4 tasks, 5 login tries (l:1/p:5)
[DATA] attacking ssh://127.0.0.1:22/
[ATTEMPT] target 127.0.0.1 - login "kali" - pass "password" - 1 of 5
[ATTEMPT] target 127.0.0.1 - login "kali" - pass "123456" - 2 of 5
[ATTEMPT] target 127.0.0.1 - login "kali" - pass "wrongpass" - 3 of 5
[ATTEMPT] target 127.0.0.1 - login "kali" - pass "admin" - 4 of 5
[ATTEMPT] target 127.0.0.1 - login "kali" - pass "test123" - 5 of 5
[ERROR] target ssh://127.0.0.1:22/ - all passwords tried
1 of 1 target completed, 0 valid passwords found
```

> ⚠️ **Important Note:** This simulation is performed **only on your own local machine** in a controlled lab environment. Never perform brute force attacks against systems you do not own or have explicit written permission to test. Unauthorized access is a criminal offense.

---

### PHASE 4 — Log Analysis & Detection

> **Goal:** Detect the simulated brute force attack using system logs. This is the core SOC analyst skill: finding the signal in the noise.

---

#### 🔹 Step 4.1 — Read Authentication Logs

```bash
sudo tail -50 /var/log/auth.log
```

**Sample Output:**
```
Jan  6 10:45:02 kali sshd[3201]: Failed password for invalid user wronguser from 127.0.0.1 port 54312 ssh2
Jan  6 10:45:03 kali sshd[3202]: Failed password for invalid user wronguser from 127.0.0.1 port 54318 ssh2
Jan  6 10:45:04 kali sshd[3203]: Invalid user wronguser from 127.0.0.1 port 54322
Jan  6 10:45:05 kali sshd[3204]: Failed password for invalid user wronguser from 127.0.0.1 port 54330 ssh2
Jan  6 10:45:05 kali sshd[3205]: Failed password for kali from 127.0.0.1 port 54336 ssh2
```

> 📌 **Explanation:** Multiple `Failed password` entries within a few seconds from the same IP is the **textbook signature of a brute force attack**. In a SIEM, this pattern would automatically trigger an alert. As an analyst, this is your smoking gun.

---

#### 🔹 Step 4.2 — Count & Aggregate Failed Login Attempts

```bash
# Count total failed attempts
grep "Failed password" /var/log/auth.log | wc -l

# Identify attacking IPs and count per IP
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn
```

**Sample Output:**
```
10

     10 127.0.0.1
```

> 📌 **Explanation:** This one-liner is a classic quick-triage command used by SOC analysts. It aggregates failed attempts by source IP and sorts by frequency. In a real attack, you might see hundreds or thousands of attempts from a single IP — or distributed attempts from many IPs (distributed brute force).

---

#### 🔹 Step 4.3 — Filter by Timeframe (Incident Window)

```bash
# Filter auth.log for a specific time window
grep "Jan  6 10:4" /var/log/auth.log | grep "Failed password"

# Using journalctl for systemd-based log access
sudo journalctl -u ssh --since "1 hour ago" | grep "Failed"
```

**Sample Output:**
```
Jan  6 10:45:02 kali sshd[3201]: Failed password for invalid user wronguser from 127.0.0.1 port 54312 ssh2
Jan  6 10:45:03 kali sshd[3202]: Failed password for invalid user wronguser from 127.0.0.1 port 54318 ssh2
... [continued]
```

> 📌 **Explanation:** Scoping log analysis to the incident timeframe is critical in real investigations to reduce noise. `journalctl` is the modern alternative to reading raw log files and allows precise time-based filtering.

---

#### 🔹 Step 4.4 — Check for Successful Logins After Brute Force

```bash
# Critical check: did anyone succeed after the failed attempts?
grep "Accepted password\|Accepted publickey" /var/log/auth.log
grep "session opened" /var/log/auth.log | tail -20
```

**Sample Output:**
```
# (No output = no successful logins — attack was unsuccessful)
# OR in a compromised scenario:
Jan  6 10:48:21 kali sshd[3301]: Accepted password for kali from 127.0.0.1 port 54399 ssh2
```

> 📌 **Explanation:** This is the **most critical check** during a brute force investigation. If a successful login follows a series of failures, the attack may have succeeded — triggering an immediate escalation. A clean result here means the attack failed, but the attempt must still be documented.

---

### PHASE 5 — Incident Writeup

> **Goal:** Document findings in a structured incident report format — a core skill for any SOC analyst.

---

#### 🔹 Sample Incident Report

```
╔══════════════════════════════════════════════════════════════════╗
║              SECURITY INCIDENT REPORT — INC-2024-001            ║
╠══════════════════════════════════════════════════════════════════╣
║ Date/Time     : 2024-01-06 | 10:45 - 10:47 EST                  ║
║ Analyst       : SOC Intern (Junior Security Analyst)             ║
║ Severity      : MEDIUM                                           ║
║ Status        : Closed — Simulated / No Breach                  ║
╠══════════════════════════════════════════════════════════════════╣
║ SUMMARY                                                          ║
║ Multiple failed SSH authentication attempts detected originating ║
║ from 127.0.0.1 against the local SSH service. 10 failed login   ║
║ attempts recorded within a 2-minute window targeting the         ║
║ 'kali' user account and non-existent 'wronguser' account.       ║
╠══════════════════════════════════════════════════════════════════╣
║ INDICATORS OF COMPROMISE (IOCs)                                  ║
║ • Source IP     : 127.0.0.1 (localhost — simulated)             ║
║ • Target Port   : 22 (SSH)                                       ║
║ • Attempted Users: wronguser, kali                               ║
║ • Log Source    : /var/log/auth.log                              ║
║ • Alert Type    : Brute Force / Credential Stuffing              ║
╠══════════════════════════════════════════════════════════════════╣
║ RESPONSE ACTIONS                                                  ║
║ 1. Confirmed no successful authentication occurred               ║
║ 2. Identified source IP and attempted usernames                  ║
║ 3. Documented timeline from log evidence                         ║
║ 4. Recommended: Implement fail2ban or account lockout policy     ║
╚══════════════════════════════════════════════════════════════════╝
```

---

## 📸 Results & Screenshots

> 📁 Screenshots are stored in the `/screenshots/` directory of this repository.

<details>
<summary>📂 Click to expand screenshot index</summary>

| # | Screenshot | Description |
|---|-----------|-------------|
| 01 | `01_uname_hostname.png` | System fingerprint — kernel version and hostname |
| 02 | `02_logged_users_w.png` | Active users via `w` command |
| 03 | `03_process_list_ps.png` | Running processes sorted by CPU usage |
| 04 | `04_ss_open_ports.png` | Open ports and listening services |
| 05 | `05_hydra_bruteforce.png` | Hydra brute force simulation output |
| 06 | `06_auth_log_failed.png` | Failed login attempts in auth.log |
| 07 | `07_grep_count_ips.png` | Aggregated failed attempts by source IP |
| 08 | `08_incident_report.png` | Final incident documentation |

</details>

---

## 💡 Key Learnings

- 🔍 **System Reconnaissance is the foundation** — Before detecting threats, analysts must know what "normal" looks like on a system. Baseline knowledge is critical.

- 📋 **`/var/log/auth.log` is a goldmine** — Authentication events, failed logins, session opens, and sudo usage are all recorded here. This file is the first place to check during a breach investigation.

- ⚡ **Brute force attacks leave obvious log signatures** — Multiple `Failed password` entries in rapid succession from a single IP are unmistakable. SIEM rules commonly use this exact pattern to fire alerts.

- 🕒 **Timing and volume matter** — 3 failed logins over an hour is normal. 100 failed logins in 30 seconds is an incident. Analysts learn to think in rates and thresholds.

- 🔗 **Log correlation over a timeline** — Cross-referencing failed attempts WITH subsequent successful logins is how you determine if an attack succeeded vs. just occurred.

- 📝 **Documentation is a security skill** — Incident reports must be precise, timestamped, and factual. Vague documentation fails in legal and compliance contexts.

- 🛡️ **Detection before prevention** — You can't block what you can't see. This lab reinforces why logging, monitoring, and alerting are the first pillars of a mature security program.

---

## 🏆 Skills Demonstrated

<div align="center">

| Skill Category | Specific Skills |
|:--------------:|:---------------:|
| 🔍 **Reconnaissance** | Linux system fingerprinting, user enumeration, network enumeration |
| 🖥️ **Linux Administration** | Process management, log reading, service management |
| ⚔️ **Offensive Concepts** | Brute force attack mechanics, credential stuffing, SSH exploitation |
| 🛡️ **Defensive Operations** | Log analysis, attack detection, IOC identification |
| 🔎 **DFIR** | Log triage, timeline reconstruction, evidence documentation |
| 📊 **SOC Analyst Skills** | Alert triage, incident documentation, threat classification |
| 🖊️ **Reporting** | Incident report writing, IOC extraction, severity classification |

</div>

---

## 🧩 Challenges & Solutions

<details>
<summary>⚠️ Challenge 1: SSH service not running by default</summary>

**Problem:** After attempting the brute force simulation, no logs were generated because SSH was not active.

**Solution:**
```bash
sudo systemctl enable ssh --now
sudo systemctl status ssh
```
**Lesson:** Always verify target services are running before testing detection logic. In a real SOC, confirming service availability is part of alert validation.

</details>

<details>
<summary>⚠️ Challenge 2: Auth log location differs across distributions</summary>

**Problem:** On some newer Kali Linux versions, `/var/log/auth.log` may not exist if `rsyslog` is not installed.

**Solution:**
```bash
# Check if rsyslog is installed
dpkg -l rsyslog

# Install if missing
sudo apt install rsyslog -y
sudo systemctl start rsyslog

# Alternative: use journalctl
sudo journalctl _SYSTEMD_UNIT=ssh.service
```
**Lesson:** Security tools and log locations vary across Linux distributions. Adaptability and knowing alternative methods is essential.

</details>

<details>
<summary>⚠️ Challenge 3: Hydra rate limiting & connection refusal</summary>

**Problem:** Hydra was generating connection errors when attempting too many concurrent threads.

**Solution:**
```bash
# Reduce threads to avoid self-DoS
hydra -l kali -P /tmp/testlist.txt ssh://127.0.0.1 -t 2 -W 1 -V
```
**Lesson:** Brute force tools can inadvertently DoS services. In real engagements, rate limiting is a critical consideration to maintain stealth and avoid crashing target systems.

</details>

---

## 🔚 Conclusion

This lab successfully demonstrated the **end-to-end workflow of a SOC analyst** responding to a brute force incident — from initial system reconnaissance and baseline establishment, to attack simulation, log-based detection, and professional incident documentation.

The most important takeaway is that **effective defense starts with knowing your environment.** Before an analyst can recognize something anomalous, they must deeply understand what normal looks like. This lab built that foundational muscle memory.

The skills practiced here — log analysis, `grep`-based triage, timeline reconstruction, and structured reporting — are directly transferable to enterprise SOC environments and form the bedrock of tools like **Splunk, ELK Stack, and Microsoft Sentinel**.

> *"The attacker only has to be right once. The defender has to be right every time. Start with knowing your logs."*

---

<div align="center">

### 🔗 Connect & Explore More

[![GitHub](https://img.shields.io/badge/GitHub-Portfolio-black?style=for-the-badge&logo=github)](https://github.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://linkedin.com)
[![TryHackMe](https://img.shields.io/badge/TryHackMe-Profile-red?style=for-the-badge&logo=tryhackme&logoColor=white)](https://tryhackme.com)

---

**⭐ If this lab helped you, consider starring the repository!**

*Part of the [Linux Security Labs](../README.md) series — hands-on cybersecurity labs for aspiring SOC analysts and security engineers.*

</div>

---

<div align="center">
<sub>Built with 🛡️ by a Security Enthusiast | For Educational Purposes Only</sub>
</div>
