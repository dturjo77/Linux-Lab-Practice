**Screenshots**

<img width="935" height="762" alt="image" src="https://github.com/user-attachments/assets/3e05a73f-881f-4b6f-8c39-861a0997f002" />
<img width="862" height="827" alt="image" src="https://github.com/user-attachments/assets/099c2670-f589-4710-99ec-cb0ddf29df4e" />
<img width="1096" height="840" alt="image" src="https://github.com/user-attachments/assets/bdbd8598-ac79-408e-9bf9-72981638d290" />
<img width="952" height="346" alt="image" src="https://github.com/user-attachments/assets/30b8383e-b7f6-4689-9d70-5b9bc3e6d8cf" />
<img width="936" height="441" alt="image" src="https://github.com/user-attachments/assets/49c4ee58-d7dc-4fdd-a3fa-541c10509b21" />
<img width="937" height="642" alt="image" src="https://github.com/user-attachments/assets/ed4c3bce-8b8d-46aa-8b85-9b05dc87f625" />
<img width="926" height="622" alt="image" src="https://github.com/user-attachments/assets/33b4d402-c0ae-4840-aff4-00f527d281d9" />
<img width="832" height="426" alt="image" src="https://github.com/user-attachments/assets/ef859688-f8ed-4b00-8165-b38d1ff83278" />

---

┌──(kali㉿kali)-[~]
└─$ sudo systemctl start ssh                                         
[sudo] password for kali: 
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; disabled; preset: disabled)
     Active: active (running) since Sat 2026-06-13 09:50:06 +06; 36s ago
 Invocation: 73c9bec0ec724e7eb973899e61c0e4aa
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 490473 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 490475 (sshd)
      Tasks: 1 (limit: 4446)
     Memory: 1.9M (peak: 2.9M)
        CPU: 112ms
     CGroup: /system.slice/ssh.service
             └─490475 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Jun 13 09:50:05 kali systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
Jun 13 09:50:06 kali sshd[490475]: Server listening on 0.0.0.0 port 22.
Jun 13 09:50:06 kali sshd[490475]: Server listening on :: port 22.
Jun 13 09:50:06 kali systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo useradd -m -s /bin/bash testserver   
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo passwd testserver                 
New password: 
Retype new password: 
passwd: password updated successfully
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo ls -lh /var/log/auth.log          
ls: cannot access '/var/log/auth.log': No such file or directory
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo systemctl start rsyslog
Failed to start rsyslog.service: Unit rsyslog.service not found.
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo apt update                              
Get:1 http://kali.download/kali kali-rolling InRelease [34.0 kB]
Get:2 http://kali.download/kali kali-rolling/main amd64 Packages [21.2 MB]
Get:3 http://kali.download/kali kali-rolling/main amd64 Contents (deb) [53.3 MB]           
Get:4 http://kali.download/kali kali-rolling/contrib amd64 Packages [104 kB]               
Get:5 http://kali.download/kali kali-rolling/contrib amd64 Contents (deb) [189 kB]         
Get:6 http://kali.download/kali kali-rolling/non-free amd64 Packages [175 kB]              
Get:7 http://kali.download/kali kali-rolling/non-free amd64 Contents (deb) [891 kB]        
Get:8 http://kali.download/kali kali-rolling/non-free-firmware amd64 Packages [15.8 kB]    
Get:9 http://kali.download/kali kali-rolling/non-free-firmware amd64 Contents (deb) [38.9 kB]
Fetched 75.9 MB in 36s (2,084 kB/s)                                                        
1551 packages can be upgraded. Run 'apt list --upgradable' to see them.
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo apt install rsyslog -y
Installing:                     
  rsyslog
                                                                                            
Installing dependencies:
  libestr0  libfastjson4  liblognorm5
                                                                                            
Suggested packages:
  rsyslog-doc      rsyslog-elasticsearch  rsyslog-kubernetes  | rsyslog-gnutls
  rsyslog-mysql    rsyslog-kafka          rsyslog-docker      rsyslog-gssapi
  rsyslog-pgsql    rsyslog-hiredis        rsyslog-clickhouse  rsyslog-relp
  rsyslog-mongodb  rsyslog-snmp           rsyslog-openssl

Summary:
  Upgrading: 0, Installing: 4, Removing: 0, Not Upgrading: 1551
  Download size: 967 kB
  Space needed: 2,566 kB / 62.8 GB available

Get:1 http://http.kali.org/kali kali-rolling/main amd64 libestr0 amd64 0.1.11-2+b2 [9,596 B]
Get:2 http://http.kali.org/kali kali-rolling/main amd64 libfastjson4 amd64 1.2304.0-2+b2 [29.2 kB]
Get:3 http://kali.download/kali kali-rolling/main amd64 liblognorm5 amd64 2.1.0-1 [72.7 kB]
Get:4 http://kali.download/kali kali-rolling/main amd64 rsyslog amd64 8.2604.0-4 [855 kB]
Fetched 967 kB in 2s (643 kB/s)
Selecting previously unselected package libestr0:amd64.
(Reading database… 426176 files and directories currently installed.)
Preparing to unpack …/libestr0_0.1.11-2+b2_amd64.deb…
Unpacking libestr0:amd64 (0.1.11-2+b2)…
Selecting previously unselected package libfastjson4:amd64.
Preparing to unpack …/libfastjson4_1.2304.0-2+b2_amd64.deb…
Unpacking libfastjson4:amd64 (1.2304.0-2+b2)…
Selecting previously unselected package liblognorm5:amd64.
Preparing to unpack …/liblognorm5_2.1.0-1_amd64.deb…
Unpacking liblognorm5:amd64 (2.1.0-1)…
Selecting previously unselected package rsyslog.
Preparing to unpack …/rsyslog_8.2604.0-4_amd64.deb…
Unpacking rsyslog (8.2604.0-4)…
Setting up libestr0:amd64 (0.1.11-2+b2)…
Setting up libfastjson4:amd64 (1.2304.0-2+b2)…
Setting up liblognorm5:amd64 (2.1.0-1)…
Setting up rsyslog (8.2604.0-4)…
Created symlink '/etc/systemd/system/syslog.service' → '/usr/lib/systemd/system/rsyslog.service'.
Created symlink '/etc/systemd/system/multi-user.target.wants/rsyslog.service' → '/usr/lib/systemd/system/rsyslog.service'.
Processing triggers for kali-menu (2026.1.5)…
Processing triggers for libc-bin (2.42-13)…
Processing triggers for man-db (2.13.1-1)…
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo systemctl start rsyslog
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo systemctl start rsyslog
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo ls -lh /var/log/auth.log
-rw-r----- 1 root adm 1.2K Jun 13 10:07 /var/log/auth.log
                                                                                            
┌──(kali㉿kali)-[~]
└─$ ssh testserver@localhost     
The authenticity of host 'localhost (::1)' can't be established.
ED25519 key fingerprint is: SHA256:/i1aQfoUaB5DjxIYN9uBeuQd5dcRfgGV07kFT0pWg8o
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'localhost' (ED25519) to the list of known hosts.
testserver@localhost's password: 
Permission denied, please try again.
testserver@localhost's password: 
Permission denied, please try again.
testserver@localhost's password: 
testserver@localhost: Permission denied (publickey,password).
                                                                                            
┌──(kali㉿kali)-[~]
└─$ ssh testserver@localhost
testserver@localhost's password: 
Permission denied, please try again.
testserver@localhost's password: 
Permission denied, please try again.
testserver@localhost's password: 
testserver@localhost: Permission denied (publickey,password).
                                                                                            
┌──(kali㉿kali)-[~]
└─$ ssh admin@localhost     
admin@localhost's password: 
Permission denied, please try again.
admin@localhost's password: 
Permission denied, please try again.
admin@localhost's password: 
admin@localhost: Permission denied (publickey,password).
                                                                                            
┌──(kali㉿kali)-[~]
└─$ ssh root@localhost 
root@localhost's password: 
Permission denied, please try again.
root@localhost's password: 
Permission denied, please try again.
root@localhost's password: 
root@localhost: Permission denied (publickey,password).
                                                                                            
┌──(kali㉿kali)-[~]
└─$ ssh administrator@localhost
administrator@localhost's password: 
Permission denied, please try again.
administrator@localhost's password: 
Permission denied, please try again.
administrator@localhost's password: 
administrator@localhost: Permission denied (publickey,password).
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo lastb                   
[sudo] password for kali: 
sudo: lastb: command not found
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo last -f /var/log/btmp | head -20
/var/log/btmp has no entries
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep -i "failed" /var/log/auth.log | head -20
2026-06-13T10:42:32.133604+06:00 kali xfce4-screensaver-dialog: pam_unix(xfce4-screensaver:account): setuid failed: Operation not permitted
2026-06-13T10:58:22.628732+06:00 kali xfce4-screensaver-dialog: pam_unix(xfce4-screensaver:auth): conversation failed
2026-06-13T11:01:29.725066+06:00 kali xfce4-screensaver-dialog: pam_unix(xfce4-screensaver:account): setuid failed: Operation not permitted
2026-06-13T11:02:14.438729+06:00 kali unix_chkpwd[521276]: password check failed for user (testserver)
2026-06-13T11:02:15.968601+06:00 kali sshd-session[521182]: Failed password for testserver from ::1 port 43920 ssh2
2026-06-13T11:02:23.961217+06:00 kali unix_chkpwd[521357]: password check failed for user (testserver)
2026-06-13T11:02:25.727938+06:00 kali sshd-session[521182]: Failed password for testserver from ::1 port 43920 ssh2
2026-06-13T11:02:34.964978+06:00 kali unix_chkpwd[521449]: password check failed for user (testserver)
2026-06-13T11:02:36.799177+06:00 kali sshd-session[521182]: Failed password for testserver from ::1 port 43920 ssh2
2026-06-13T11:02:50.773321+06:00 kali unix_chkpwd[521574]: password check failed for user (testserver)
2026-06-13T11:02:53.171515+06:00 kali sshd-session[521540]: Failed password for testserver from ::1 port 49862 ssh2
2026-06-13T11:03:03.068592+06:00 kali unix_chkpwd[521676]: password check failed for user (testserver)
2026-06-13T11:03:04.844368+06:00 kali sshd-session[521540]: Failed password for testserver from ::1 port 49862 ssh2
2026-06-13T11:03:13.530847+06:00 kali unix_chkpwd[521759]: password check failed for user (testserver)
2026-06-13T11:03:15.375961+06:00 kali sshd-session[521540]: Failed password for testserver from ::1 port 49862 ssh2
2026-06-13T11:04:28.085832+06:00 kali sshd-session[522318]: Failed password for invalid user admin from ::1 port 56904 ssh2
2026-06-13T11:04:34.080223+06:00 kali sshd-session[522318]: Failed password for invalid user admin from ::1 port 56904 ssh2
2026-06-13T11:04:39.515225+06:00 kali sshd-session[522318]: Failed password for invalid user admin from ::1 port 56904 ssh2
2026-06-13T11:05:40.375414+06:00 kali unix_chkpwd[522959]: password check failed for user (root)
2026-06-13T11:05:42.182784+06:00 kali sshd-session[522933]: Failed password for root from ::1 port 50452 ssh2
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "Failed password" /var/log/auth.log | tail -20
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
2026-06-13T11:18:59.058878+06:00 kali sudo:     kali : TTY=pts/0 ; PWD=/home/kali ; USER=root ; COMMAND=/usr/bin/grep 'Failed password' /var/log/auth.log
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "Failed password" /var/log/auth.log | awk '{print $9}' | sort | uniq -c | sort -rn
      9 ::1
      3 administrator
      3 admin
      2 ;
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn
      6 ::1
      3 50452
      3 49862
      3 43920
      2 ;
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "Failed password" /var/log/auth.log | awk '{print $1, $2, $3}' | head -20
2026-06-13T11:02:15.968601+06:00 kali sshd-session[521182]:
2026-06-13T11:02:25.727938+06:00 kali sshd-session[521182]:
2026-06-13T11:02:36.799177+06:00 kali sshd-session[521182]:
2026-06-13T11:02:53.171515+06:00 kali sshd-session[521540]:
2026-06-13T11:03:04.844368+06:00 kali sshd-session[521540]:
2026-06-13T11:03:15.375961+06:00 kali sshd-session[521540]:
2026-06-13T11:04:28.085832+06:00 kali sshd-session[522318]:
2026-06-13T11:04:34.080223+06:00 kali sshd-session[522318]:
2026-06-13T11:04:39.515225+06:00 kali sshd-session[522318]:
2026-06-13T11:05:42.182784+06:00 kali sshd-session[522933]:
2026-06-13T11:05:49.707308+06:00 kali sshd-session[522933]:
2026-06-13T11:05:59.207577+06:00 kali sshd-session[522933]:
2026-06-13T11:06:31.601696+06:00 kali sshd-session[523337]:
2026-06-13T11:06:39.972726+06:00 kali sshd-session[523337]:
2026-06-13T11:06:51.919134+06:00 kali sshd-session[523337]:
2026-06-13T11:18:59.058878+06:00 kali sudo:
2026-06-13T11:21:47.190846+06:00 kali sudo:
2026-06-13T11:23:06.873648+06:00 kali sudo:
2026-06-13T11:24:57.102006+06:00 kali sudo:
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "Accepted password" /var/log/auth.log | tail -10
2026-06-13T11:26:40.556879+06:00 kali sudo:     kali : TTY=pts/0 ; PWD=/home/kali ; USER=root ; COMMAND=/usr/bin/grep 'Accepted password' /var/log/auth.log
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "Accepted password" /var/log/auth.log | tail -10
2026-06-13T11:26:40.556879+06:00 kali sudo:     kali : TTY=pts/0 ; PWD=/home/kali ; USER=root ; COMMAND=/usr/bin/grep 'Accepted password' /var/log/auth.log
2026-06-13T11:27:28.614770+06:00 kali sudo:     kali : TTY=pts/0 ; PWD=/home/kali ; USER=root ; COMMAND=/usr/bin/grep 'Accepted password' /var/log/auth.log
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "Failed password" /var/log/auth.log | wc -l
20
                                                                                            
┌──(kali㉿kali)-[~]
└─$ grep "Invalid user" /var/log/auth.log | awk '{print $8}' | sort | uniq -c | sort -rn
      2 ::1
                                                                                            
┌──(kali㉿kali)-[~]
└─$ echo "=== FAILED LOGIN SUMMARY ===" && \
echo "Total Failed Attempts:" && \
sudo grep "Failed password" /var/log/auth.log | wc -l && \
echo "" && \
echo "Targeted Usernames:" && \
sudo grep "Failed password" /var/log/auth.log | awk '{print $9}' | sort | uniq -c | sort -rn && \
echo "" && \
echo "Source IPs:" && \
sudo grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn
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
                                                                                            
┌──(kali㉿kali)-[~]
└─$ which fail2ban-client
fail2ban-client not found
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo apt list --installed 2>/dev/null | grep fail2ban
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo apt update && sudo apt install fail2ban -y      
Hit:1 http://http.kali.org/kali kali-rolling InRelease
1551 packages can be upgraded. Run 'apt list --upgradable' to see them.
Installing:                     
  fail2ban
                                                                                            
Installing dependencies:
  python3-systemd
                                                                                            
Suggested packages:
  mailx  monit

Summary:
  Upgrading: 0, Installing: 2, Removing: 0, Not Upgrading: 1551
  Download size: 508 kB
  Space needed: 2,603 kB / 62.8 GB available

Get:1 http://http.kali.org/kali kali-rolling/main amd64 python3-systemd amd64 235-1+b7 [43.3 kB]
Get:2 http://kali.download/kali kali-rolling/main amd64 fail2ban all 1.1.0-10 [465 kB]
Fetched 508 kB in 1s (407 kB/s)   
Selecting previously unselected package python3-systemd.
(Reading database… 426260 files and directories currently installed.)
Preparing to unpack …/python3-systemd_235-1+b7_amd64.deb…
Unpacking python3-systemd (235-1+b7)…
Selecting previously unselected package fail2ban.
Preparing to unpack …/fail2ban_1.1.0-10_all.deb…
Unpacking fail2ban (1.1.0-10)…
Setting up python3-systemd (235-1+b7)…
Setting up fail2ban (1.1.0-10)…
update-rc.d: We have no instructions for the fail2ban init script.
update-rc.d: It looks like a network service, we disable it.
fail2ban.service is a disabled or a static unit, not starting it.
Processing triggers for kali-menu (2026.1.5)…
Processing triggers for man-db (2.13.1-1)…
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo systemctl start fail2ban                  
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo systemctl enable fail2ban
Synchronizing state of fail2ban.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
Executing: /usr/lib/systemd/systemd-sysv-install enable fail2ban
Created symlink '/etc/systemd/system/multi-user.target.wants/fail2ban.service' → '/usr/lib/systemd/system/fail2ban.service'.
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo systemctl status fail2ban
● fail2ban.service - Fail2Ban Service
     Loaded: loaded (/usr/lib/systemd/system/fail2ban.service; enabled; preset: disabled)
     Active: active (running) since Sat 2026-06-13 11:50:11 +06; 1min 10s ago
 Invocation: a7601fa7749d451a8cf99b3e95b484c7
       Docs: man:fail2ban(1)
   Main PID: 544873 (fail2ban-server)
      Tasks: 5 (limit: 4446)
     Memory: 13.3M (peak: 15.3M)
        CPU: 554ms
     CGroup: /system.slice/fail2ban.service
             └─544873 /usr/bin/python3 /usr/bin/fail2ban-server -xf start

Jun 13 11:50:11 kali systemd[1]: Started fail2ban.service - Fail2Ban Service.
Jun 13 11:50:11 kali fail2ban-server[544873]: Server ready
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo fali2ban-client status        
sudo: fali2ban-client: command not found
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo fali2ban-client status sshd
sudo: fali2ban-client: command not found
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo fail2ban-client status     
Status
|- Number of jail:      1
`- Jail list:   sshd
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo fail2ban-client status sshd
Status for the jail: sshd
|- Filter
|  |- Currently failed: 0
|  |- Total failed:     0
|  `- Journal matches:  _SYSTEMD_UNIT=ssh.service + _COMM=sshd
`- Actions
   |- Currently banned: 0
   |- Total banned:     0
   `- Banned IP list:
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo cat /etc/fail2ban/jail.conf | grep -A 10 "\[sshd\]"
# [sshd]
# enabled = true
#
# See jail.conf(5) man page for more information



# Comments: use '#' for comment lines and ';' (following a space) for inline comments


[INCLUDES]
--
[sshd]

# To use more aggressive sshd modes set filter parameter "mode" in jail.local:
# normal (default), ddos, extra or aggressive (combines all).
# See "tests/files/logs/sshd" or "filter.d/sshd.conf" for usage example and details.
#mode   = normal
port    = ssh
logpath = %(sshd_log)s
backend = %(sshd_backend)s


                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep -E "bantime|findtime|maxretry" /etc/fail2ban/jail.conf | grep -v "#"
bantime  = 10m
findtime  = 10m
maxretry = 5
maxmatches = %(maxretry)s
bantime  = 48h
maxretry = 1
maxretry = 2
maxretry = 2
maxretry = 2
maxretry = 1
maxretry = 2
maxretry = 1
maxretry = 1
maxretry = 10
maxretry = 10
bantime  = 1w
findtime = 1d
maxretry  = 2
maxretry = 1
maxretry = 1
bantime      = 1h
maxretry     = 1
findtime     = 1
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo userdel -r testserver
userdel: testserver mail spool (/var/mail/testserver) not found
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo systemctl stop fail2ban
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "Failed" /var/log/auth.log | awk '{print $3}' | cut -d: -f1 | sort | uniq -c
      3 sshd-session[521182]
      3 sshd-session[521540]
      3 sshd-session[522318]
      3 sshd-session[522933]
      3 sshd-session[523337]
      9 sudo
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "Accepted" /var/log/auth.log
2026-06-13T11:26:40.556879+06:00 kali sudo:     kali : TTY=pts/0 ; PWD=/home/kali ; USER=root ; COMMAND=/usr/bin/grep 'Accepted password' /var/log/auth.log
2026-06-13T11:27:28.614770+06:00 kali sudo:     kali : TTY=pts/0 ; PWD=/home/kali ; USER=root ; COMMAND=/usr/bin/grep 'Accepted password' /var/log/auth.log
2026-06-13T12:05:58.614815+06:00 kali sudo:     kali : TTY=pts/0 ; PWD=/home/kali ; USER=root ; COMMAND=/usr/bin/grep Accepted /var/log/auth.log
                                                                                            
┌──(kali㉿kali)-[~]
└─$ 
