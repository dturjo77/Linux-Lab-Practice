<img width="956" height="873" alt="Screenshot 2026-06-12 111905" src="https://github.com/user-attachments/assets/a6cd8e3d-7b9e-425a-9e77-2f3d2ccf0d5c" />
<img width="952" height="493" alt="Screenshot 2026-06-12 112946" src="https://github.com/user-attachments/assets/f8b05198-9727-4da9-9712-74d67ca9d09d" />
<img width="942" height="485" alt="Screenshot 2026-06-12 113505" src="https://github.com/user-attachments/assets/71bf7a55-1497-4c77-b3e3-191db7a2a27f" />
<img width="910" height="693" alt="Screenshot 2026-06-12 113709" src="https://github.com/user-attachments/assets/39e427d7-f24d-43af-87c8-d1bab2f6aa24" />
<img width="957" height="772" alt="Screenshot 2026-06-12 114728" src="https://github.com/user-attachments/assets/9ddc2168-c417-4761-a365-df73ab62e4d0" />
<img width="932" height="742" alt="Screenshot 2026-06-12 115853" src="https://github.com/user-attachments/assets/cebd898b-7305-4226-81c5-cc995331bf79" />
<img width="732" height="348" alt="Screenshot 2026-06-12 122937" src="https://github.com/user-attachments/assets/2ed9fa8f-84fd-4129-a183-27f8a54409a7" />

---
001 System Reconnaissance & Alert Generation

                                                                                                                   
┌──(kali㉿kali)-[~]
└─$ whoami
kali
                                                                                            
┌──(kali㉿kali)-[~]
└─$ id
uid=1000(kali) gid=1000(kali) groups=1000(kali),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),100(users),101(netdev),102(scanner),104(bluetooth),113(lpadmin),122(wireshark),123(kaboxer)
                                                                                            
┌──(kali㉿kali)-[~]
└─$ hostname
kali
                                                                                            
┌──(kali㉿kali)-[~]
└─$ uname -a
Linux kali 6.18.12+kali-amd64 #1 SMP PREEMPT_DYNAMIC Kali 6.18.12-1kali1 (2026-02-25) x86_64 GNU/Linux
                                                                                            
┌──(kali㉿kali)-[~]
└─$ uptime  
 10:34:48 up 13:54,  1 user,  load average: 0.03, 0.08, 0.07
                                                                                            
┌──(kali㉿kali)-[~]
└─$ w     
 10:36:59 up 13:57,  1 user,  load average: 0.10, 0.07, 0.07
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU  WHAT
kali              -                15May26         0.00s  0.04s lightdm --session-child 13 
                                                                                            
┌──(kali㉿kali)-[~]
└─$ last | head -20
kali     tty7         :0               Fri May 15 09:14 - still logged in
lightdm  tty7         :0               Fri May 15 09:14 - 09:14  (00:00)
kali     tty7         :0               Sun May  3 12:55 - 13:40  (00:44)
lightdm  tty7         :0               Sun May  3 12:55 - 12:55  (00:00)
kali     tty7         :0               Sat May  2 20:56 - 20:58  (00:01)
lightdm  tty7         :0               Sat May  2 20:56 - 20:56  (00:00)
postgres                               Fri Mar 20 12:44 - 12:44  (00:00)

wtmpdb begins Fri Mar 20 12:44:39 2026
                                                                                            
┌──(kali㉿kali)-[~]
└─$ free -m                     
               total        used        free      shared  buff/cache   available
Mem:            3911        1310         726          27        2256        2601
Swap:            953           0         953
                                                                                            
┌──(kali㉿kali)-[~]
└─$ lscpu | grep -E "Architecture|CPU\(s\)|Model name"
Architecture:                            x86_64
CPU(s):                                  4
On-line CPU(s) list:                     0-3
Model name:                              AMD Ryzen 5 5625U with Radeon Graphics
NUMA node0 CPU(s):                       0-3
                                                                                            
┌──(kali㉿kali)-[~]
└─$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            1.9G     0  1.9G   0% /dev
tmpfs           392M  1.3M  390M   1% /run
/dev/sda1        79G   16G   59G  22% /
tmpfs           2.0G  4.0K  2.0G   1% /dev/shm
none            1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
tmpfs           2.0G  240K  2.0G   1% /tmp
none            1.0M     0  1.0M   0% /run/credentials/getty@tty1.service
tmpfs           392M  112K  392M   1% /run/user/1000
                                                                                            
┌──(kali㉿kali)-[~]
└─$ ps aux | head -20             
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.3  24796 15916 ?        Ss   Jun11   0:14 /sbin/init splash
root           2  0.0  0.0      0     0 ?        S    Jun11   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        S    Jun11   0:00 [pool_workqueue_release]
root           4  0.0  0.0      0     0 ?        I<   Jun11   0:00 [kworker/R-rcu_gp]
root           5  0.0  0.0      0     0 ?        I<   Jun11   0:00 [kworker/R-sync_wq]
root           6  0.0  0.0      0     0 ?        I<   Jun11   0:00 [kworker/R-kvfree_rcu_reclaim]
root           7  0.0  0.0      0     0 ?        I<   Jun11   0:00 [kworker/R-slub_flushwq]
root           8  0.0  0.0      0     0 ?        I<   Jun11   0:00 [kworker/R-netns]
root          13  0.0  0.0      0     0 ?        I<   Jun11   0:00 [kworker/R-mm_percpu_wq]
root          14  0.0  0.0      0     0 ?        S    Jun11   0:00 [ksoftirqd/0]
root          15  0.1  0.0      0     0 ?        I    Jun11   1:12 [rcu_preempt]
root          16  0.0  0.0      0     0 ?        S    Jun11   0:00 [rcu_exp_par_gp_kthread_worker/1]
root          17  0.0  0.0      0     0 ?        S    Jun11   0:00 [rcu_exp_gp_kthread_worker]
root          18  0.0  0.0      0     0 ?        S    Jun11   0:02 [migration/0]
root          19  0.0  0.0      0     0 ?        S    Jun11   0:00 [idle_inject/0]
root          20  0.0  0.0      0     0 ?        S    Jun11   0:00 [cpuhp/0]
root          21  0.0  0.0      0     0 ?        S    Jun11   0:00 [cpuhp/1]
root          22  0.0  0.0      0     0 ?        S    Jun11   0:00 [idle_inject/1]
root          23  0.0  0.0      0     0 ?        S    Jun11   0:02 [migration/1]
                                                                                            
┌──(kali㉿kali)-[~]
└─$ ps aux --sort=-%cpu | head -10
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
kali      432132  350  0.1   9540  4520 pts/0    R+   11:18   0:00 ps aux --sort=-%cpu
root         946  1.8  4.7 502288 190884 tty7    Rsl+ Jun11  16:10 /usr/lib/xorg/Xorg :0 -seat seat0 -auth /var/run/lightdm/root/:0 -nolisten tcp vt7 -novtswitch
kali        1396  1.7  3.4 1287324 140072 ?      Sl   Jun11  14:47 xfwm4
kali        1885  0.8  1.4 858412 56920 ?        Sl   Jun11   7:19 /usr/bin/vmtoolsd -n vmusr --blockFd 3
root         537  0.7  0.2 253024 10552 ?        Ssl  Jun11   6:25 /usr/bin/vmtoolsd
kali        1513  0.6  1.6 315380 67476 ?        Sl   Jun11   5:36 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libcpugraph.so 13 16777228 cpugraph CPU Graph Graphical representation of the CPU load
kali        1515  0.5  0.7 277060 30524 ?        Sl   Jun11   5:09 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libgenmon.so 15 16777230 genmon Generic Monitor Show output of a command.
kali      389006  0.1  1.7 812200 70732 ?        Sl   09:51   0:09 /usr/bin/qterminal
root          15  0.1  0.0      0     0 ?        I    Jun11   1:13 [rcu_preempt]
                                                                                            
┌──(kali㉿kali)-[~]
└─$ top                       
top - 11:18:33 up 14:21,  1 user,  load average: 0.04, 0.05, 0.07
Tasks: 230 total,   1 running, 229 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.0 us,  0.8 sy,  0.0 ni, 97.2 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st 
MiB Mem :   3911.3 total,    732.1 free,   1303.7 used,   2257.0 buff/cache     
MiB Swap:    953.7 total,    953.7 free,      0.0 used.   2607.6 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND              
    946 root      20   0  502288 190884  98304 S   3.6   4.8  16:11.38 Xorg                 
    537 root      20   0  253024  10552   8780 S   1.0   0.3   6:25.22 vmtoolsd             
   1396 kali      20   0 1287324 140072  91396 S   1.0   3.5  14:48.00 xfwm4                
   1885 kali      20   0  858412  56920  36328 S   1.0   1.4   7:19.64 vmtoolsd             
   1513 kali      20   0  315380  67476  24760 S   0.7   1.7   5:36.88 wrapper-2.0          
   1515 kali      20   0  277060  30524  23196 S   0.7   0.8   5:09.59 wrapper-2.0          
 389006 kali      20   0  812200  70796  52776 S   0.7   1.8   0:09.62 qterminal            
 432295 kali      20   0   10440   5980   3820 R   0.7   0.1   0:00.14 top                  
     15 root      20   0       0      0      0 I   0.3   0.0   1:13.16 rcu_preempt          
   1377 kali      20   0  168820   8000   7264 S   0.3   0.2   0:04.22 at-spi2-registr      
   1824 kali      20   0  269724  19636  17092 S   0.3   0.5   0:01.18 polkit-mate-aut      
 427838 root      20   0       0      0      0 I   0.3   0.0   0:00.30 kworker/2:0-events   
 429795 root      20   0       0      0      0 I   0.3   0.0   0:00.35 kworker/0:0-events   
      1 root      20   0   24796  15916  11468 S   0.0   0.4   0:14.67 systemd              
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.10 kthreadd             
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_workqueue_rele+ 
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-rcu_gp     
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-sync_wq    
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-kvfree_rc+ 
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-slub_flus+ 
      8 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-netns      
     13 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-mm_percpu+ 
     14 root      20   0       0      0      0 S   0.0   0.0   0:00.98 ksoftirqd/0          
     16 root      20   0       0      0      0 S   0.0   0.0   0:00.43 rcu_exp_par_gp_kthr+ 
     17 root      20   0       0      0      0 S   0.0   0.0   0:00.35 rcu_exp_gp_kthread_+ 
     18 root      rt   0       0      0      0 S   0.0   0.0   0:02.33 migration/0          
     19 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_inject/0        
     20 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/0              
     21 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/1              
     22 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_inject/1        
     23 root      rt   0       0      0      0 S   0.0   0.0   0:02.99 migration/1          
                                                                                            
┌──(kali㉿kali)-[~]
└─$ htop    
                                                                                            
┌──(kali㉿kali)-[~]
└─$ lsof | head -30                                   
lsof: WARNING: can't stat() tracefs file system /sys/kernel/debug/tracing
      Output information may be incomplete.
COMMAND      PID    TID TASKCMD               USER   FD      TYPE             DEVICE  SIZE/OFF    NODE NAME
systemd        1                              root  cwd   unknown                                      /proc/1/cwd (readlink: Permission denied)
systemd        1                              root  rtd   unknown                                      /proc/1/root (readlink: Permission denied)
systemd        1                              root  txt   unknown                                      /proc/1/exe (readlink: Permission denied)
systemd        1                              root NOFD      0000                                      /proc/1/fd (opendir: Permission denied)
kthreadd       2                              root  cwd   unknown                                      /proc/2/cwd (readlink: Permission denied)
kthreadd       2                              root  rtd   unknown                                      /proc/2/root (readlink: Permission denied)
kthreadd       2                              root  txt   unknown                                      /proc/2/exe (readlink: Permission denied)
kthreadd       2                              root NOFD      0000                                      /proc/2/fd (opendir: Permission denied)
pool_work      3                              root  cwd   unknown                                      /proc/3/cwd (readlink: Permission denied)
pool_work      3                              root  rtd   unknown                                      /proc/3/root (readlink: Permission denied)
pool_work      3                              root  txt   unknown                                      /proc/3/exe (readlink: Permission denied)
pool_work      3                              root NOFD      0000                                      /proc/3/fd (opendir: Permission denied)
kworker/R      4                              root  cwd   unknown                                      /proc/4/cwd (readlink: Permission denied)
kworker/R      4                              root  rtd   unknown                                      /proc/4/root (readlink: Permission denied)
kworker/R      4                              root  txt   unknown                                      /proc/4/exe (readlink: Permission denied)
kworker/R      4                              root NOFD      0000                                      /proc/4/fd (opendir: Permission denied)
kworker/R      5                              root  cwd   unknown                                      /proc/5/cwd (readlink: Permission denied)
kworker/R      5                              root  rtd   unknown                                      /proc/5/root (readlink: Permission denied)
kworker/R      5                              root  txt   unknown                                      /proc/5/exe (readlink: Permission denied)
kworker/R      5                              root NOFD      0000                                      /proc/5/fd (opendir: Permission denied)
kworker/R      6                              root  cwd   unknown                                      /proc/6/cwd (readlink: Permission denied)
kworker/R      6                              root  rtd   unknown                                      /proc/6/root (readlink: Permission denied)
kworker/R      6                              root  txt   unknown                                      /proc/6/exe (readlink: Permission denied)
kworker/R      6                              root NOFD      0000                                      /proc/6/fd (opendir: Permission denied)
kworker/R      7                              root  cwd   unknown                                      /proc/7/cwd (readlink: Permission denied)
kworker/R      7                              root  rtd   unknown                                      /proc/7/root (readlink: Permission denied)
kworker/R      7                              root  txt   unknown                                      /proc/7/exe (readlink: Permission denied)
kworker/R      7                              root NOFD      0000                                      /proc/7/fd (opendir: Permission denied)
kworker/R      8                              root  cwd   unknown                                      /proc/8/cwd (readlink: Permission denied)
                                                                                            
┌──(kali㉿kali)-[~]
└─$ lsof -i | head -20
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo useradd -m -s /bin/bash backdoor_user
[sudo] password for kali: 
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo passwd backdoor_user                 
New password: 
Retype new password: 
passwd: password updated successfully
                                                                                            
┌──(kali㉿kali)-[~]
└─$ w
 11:28:04 up 14:31,  1 user,  load average: 0.06, 0.08, 0.08
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU  WHAT
kali              -                15May26         0.00s  0.06s lightdm --session-child 13 
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo usermod -aG sudo backdoor_user       
                                                                                            
┌──(kali㉿kali)-[~]
└─$ whoami                             
kali
                                                                                            
┌──(kali㉿kali)-[~]
└─$ su - backdoor_user                 
Password: 
┌──(backdoor_user㉿kali)-[~]
└─$ w                                                                                       
 11:32:25 up 14:35,  1 user,  load average: 0.10, 0.08, 0.08
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU  WHAT
kali              -                15May26         0.00s  0.07s lightdm --session-child 13 

┌──(backdoor_user㉿kali)-[~]
└─$ whoami                                                                                  
backdoor_user

┌──(backdoor_user㉿kali)-[~]
└─$ sudo cat /etc/shadown                                                                   
[sudo] password for backdoor_user: 
cat: /etc/shadown: No such file or directory

┌──(backdoor_user㉿kali)-[~]
└─$ sudo cat /etc/passwd                                                                    
root:x:0:0:root:/root:/usr/bin/zsh
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
dhcpcd:x:996:996:DHCP Client Daemon:/usr/lib/dhcpcd:/bin/false
systemd-timesync:x:990:990:systemd Time Synchronization:/:/usr/sbin/nologin
messagebus:x:989:989:System Message Bus:/nonexistent:/usr/sbin/nologin
tss:x:987:987:tss user for tpm2:/:/usr/sbin/nologin
strongswan:x:100:65534::/var/lib/strongswan:/usr/sbin/nologin
tcpdump:x:986:986:tcpdump:/nonexistent:/usr/sbin/nologin
sshd:x:985:65534:sshd user:/run/sshd:/usr/sbin/nologin
_rpc:x:101:65534::/run/rpcbind:/usr/sbin/nologin
statd:x:102:65534::/var/lib/nfs:/usr/sbin/nologin
dnsmasq:x:984:65534:dnsmasq:/var/lib/misc:/usr/sbin/nologin
avahi:x:103:105:Avahi mDNS daemon:/run/avahi-daemon:/usr/sbin/nologin
nm-openvpn:x:983:983:NetworkManager OpenVPN:/var/lib/openvpn/chroot:/usr/sbin/nologin
speech-dispatcher:x:982:982:Speech Dispatcher:/run/speech-dispatcher:/bin/false
usbmux:x:104:46:usbmux daemon:/var/lib/usbmux:/usr/sbin/nologin
nm-openconnect:x:981:981:NetworkManager OpenConnect plugin:/:/usr/sbin/nologin
pipewire:x:980:980:system user for pipewire:/nonexistent:/usr/sbin/nologin
saned:x:105:106::/var/lib/saned:/usr/sbin/nologin
lightdm:x:106:107:Light Display Manager:/var/lib/lightdm:/bin/false
polkitd:x:979:979:User for polkitd:/:/usr/sbin/nologin
rtkit:x:978:978:RealtimeKit:/proc:/usr/sbin/nologin
colord:x:977:977:colord colour management daemon:/var/lib/colord:/usr/sbin/nologin
pcscd:x:976:976:PC/SC Smart Card Daemon:/:/usr/sbin/nologin
stunnel4:x:975:975:stunnel service system account:/var/run/stunnel4:/usr/sbin/nologin
geoclue:x:974:974::/var/lib/geoclue:/usr/sbin/nologin
Debian-snmp:x:107:109::/var/lib/snmp:/bin/false
sslh:x:108:110::/nonexistent:/usr/sbin/nologin
cups-pk-helper:x:109:113:user for cups-pk-helper service:/nonexistent:/usr/sbin/nologin
redsocks:x:110:114::/var/run/redsocks:/usr/sbin/nologin
_gophish:x:111:116::/var/lib/gophish:/usr/sbin/nologin
iodine:x:112:65534::/run/iodine:/usr/sbin/nologin
miredo:x:113:65534::/var/run/miredo:/usr/sbin/nologin
redis:x:114:117::/var/lib/redis:/usr/sbin/nologin
postgres:x:115:118:PostgreSQL administrator:/var/lib/postgresql:/bin/bash
mosquitto:x:116:119::/var/lib/mosquitto:/usr/sbin/nologin
inetsim:x:117:120::/var/lib/inetsim:/usr/sbin/nologin
mysql:x:973:973:MariaDB Server:/nonexistent:/bin/false
_gvm:x:118:121::/var/lib/openvas:/usr/sbin/nologin
kali:x:1000:1000::/home/kali:/usr/bin/zsh
rahim:x:1001:1003:Rahim - Senior Developer:/home/rahim:/bin/bash
nila:x:1003:1005:Nila - Database Admin:/home/nila:/bin/bash
mysqlsvc:x:999:984:MySQL Service Account:/home/mysqlsvc:/usr/sbin/nologin
backdoor_user:x:1004:1004::/home/backdoor_user:/bin/bash

┌──(backdoor_user㉿kali)-[~]
└─$ sudo ls /root                                                                           

┌──(backdoor_user㉿kali)-[~]
└─$ touch /tmp/.hidden_payload                                                              

┌──(backdoor_user㉿kali)-[~]
└─$ ls                                                                                      

┌──(backdoor_user㉿kali)-[~]
└─$ ls ls                                                                                   
ls: cannot access 'ls': No such file or directory

┌──(backdoor_user㉿kali)-[~]
└─$ ls -ls                                                                                  
total 0

┌──(backdoor_user㉿kali)-[~]
└─$ ls -la                                                                                  
total 68
drwx------ 5 backdoor_user backdoor_user  4096 Jun 12 11:33 .
drwxr-xr-x 6 root          root           4096 Jun 12 11:27 ..
-rw-r--r-- 1 backdoor_user backdoor_user   220 Feb 13 16:43 .bash_logout
-rw-r--r-- 1 backdoor_user backdoor_user  5578 Mar 20 12:40 .bashrc
-rw-r--r-- 1 backdoor_user backdoor_user  3526 Feb 13 16:43 .bashrc.original
drwxr-xr-x 7 backdoor_user backdoor_user  4096 Mar 20 12:40 .config
-rw-r--r-- 1 backdoor_user backdoor_user 11759 Mar  5 23:36 .face
lrwxrwxrwx 1 backdoor_user backdoor_user     5 Mar  5 23:36 .face.icon -> .face
drwxr-xr-x 3 backdoor_user backdoor_user  4096 Mar 20 12:40 .java
drwxr-xr-x 4 backdoor_user backdoor_user  4096 Mar 20 12:40 .local
-rw-r--r-- 1 backdoor_user backdoor_user   807 Feb 13 16:43 .profile
-rw-r--r-- 1 backdoor_user backdoor_user     0 Jun 12 11:33 .sudo_as_admin_successful
-rw-r--r-- 1 backdoor_user backdoor_user   336 Mar  4 21:09 .zprofile
-rw-r--r-- 1 backdoor_user backdoor_user 10882 Mar  4 21:09 .zshrc

┌──(backdoor_user㉿kali)-[~]
└─$ echo "This is a simulated malicious script" > /tmp/.hidden_payload

┌──(backdoor_user㉿kali)-[~]
└─$ cat /tmp/.hidden_payload
This is a simulated malicious script

┌──(backdoor_user㉿kali)-[~]
└─$ cat /etc/passwd | tail -5                                                               
kali:x:1000:1000::/home/kali:/usr/bin/zsh
rahim:x:1001:1003:Rahim - Senior Developer:/home/rahim:/bin/bash
nila:x:1003:1005:Nila - Database Admin:/home/nila:/bin/bash
mysqlsvc:x:999:984:MySQL Service Account:/home/mysqlsvc:/usr/sbin/nologin
backdoor_user:x:1004:1004::/home/backdoor_user:/bin/bash

┌──(backdoor_user㉿kali)-[~]
└─$ grep "backdoor_user" /etc/passwd                                                        
backdoor_user:x:1004:1004::/home/backdoor_user:/bin/bash

┌──(backdoor_user㉿kali)-[~]
└─$ sudo grep "backdoor_user" /var/log/auth.log | tail -20
grep: /var/log/auth.log: No such file or directory

┌──(backdoor_user㉿kali)-[~]
└─$ sudo grep "useradd" /var/log/auth.log | tail -20                                  
grep: /var/log/auth.log: No such file or directory

┌──(backdoor_user㉿kali)-[~]
└─$ su - kali                                                                               
Password: 
┌──(kali㉿kali)-[~]
└─$ sudo grep "backdoor_user" /var/log/auth.log | tail -20
[sudo] password for kali: 
grep: /var/log/auth.log: No such file or directory
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "useradd" /var/log/auth.log | tail -10
grep: /var/log/auth.log: No such file or directory
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "sudo" /var/log/auth.log | tail -20
grep: /var/log/auth.log: No such file or directory
                                                                                            
┌──(kali㉿kali)-[~]
└─$ find /tmp -name ".*" -type f
/tmp/.hidden_payload
find: ‘/tmp/systemd-private-7bb724ab6a9e45a69593dae060d0444b-colord.service-YsK9Qo’: Permission denied
find: ‘/tmp/systemd-private-7bb724ab6a9e45a69593dae060d0444b-upower.service-O0vbM3’: Permission denied
/tmp/.xfsm-ICE-HPOEP3
/tmp/.X0-lock
find: ‘/tmp/vmware-root_537-4257134911’: Permission denied
find: ‘/tmp/systemd-private-7bb724ab6a9e45a69593dae060d0444b-ModemManager.service-lNKnWh’: Permission denied
find: ‘/tmp/systemd-private-7bb724ab6a9e45a69593dae060d0444b-systemd-logind.service-HRTWPd’: Permission denied
find: ‘/tmp/systemd-private-7bb724ab6a9e45a69593dae060d0444b-polkit.service-L97Jgl’: Permission denied
find: ‘/tmp/systemd-private-7bb724ab6a9e45a69593dae060d0444b-haveged.service-iHAknM’: Permission denied
                                                                                            
┌──(kali㉿kali)-[~]
└─$ ls -la /tmp/
total 224
drwxrwxrwt 14 root          root             480 Jun 12 11:44 .
drwxr-xr-x 18 root          root            4096 Mar 20 13:20 ..
-rw-rw-r--  1 kali          kali           15050 May 25 15:57 complete_log.txt
-rw-------  1 kali          kali               0 May 15 09:14 config-err-LDwxKq
-rw-rw-r--  1 kali          kali           70832 May 25 15:56 errors.txt
-rw-rw-r--  1 kali          kali            2879 May 25 15:54 files_found.txt
drwxrwxrwt  2 root          root              40 May 15 09:14 .font-unix
-rw-rw-r--  1 kali          kali          103095 May 25 15:56 found.txt
-rw-rw-r--  1 backdoor_user backdoor_user     37 Jun 12 11:38 .hidden_payload
drwxrwxrwt  2 root          root              60 May 15 09:14 .ICE-unix
-rw-rw-r--  1 kali          kali            1350 May 25 15:58 ping_log.txt
-rw-r--r--  1 root          root            6908 May 25 15:52 strace_output.txt
drwx------  3 root          root              60 May 15 09:14 systemd-private-7bb724ab6a9e45a69593dae060d0444b-colord.service-YsK9Qo                                                    
drwx------  3 root          root              60 May 15 09:14 systemd-private-7bb724ab6a9e45a69593dae060d0444b-haveged.service-iHAknM                                                   
drwx------  3 root          root              60 May 15 09:14 systemd-private-7bb724ab6a9e45a69593dae060d0444b-ModemManager.service-lNKnWh
drwx------  3 root          root              60 May 15 09:14 systemd-private-7bb724ab6a9e45a69593dae060d0444b-polkit.service-L97Jgl                                                    
drwx------  3 root          root              60 May 15 09:14 systemd-private-7bb724ab6a9e45a69593dae060d0444b-systemd-logind.service-HRTWPd
drwx------  3 root          root              60 May 15 09:14 systemd-private-7bb724ab6a9e45a69593dae060d0444b-upower.service-O0vbM3                                                    
drwxrwxrwt  2 root          root              40 May 15 09:14 VMwareDnD
drwx------  2 root          root              40 May 15 09:14 vmware-root_537-4257134911
-r--r--r--  1 root          root              11 May 15 09:14 .X0-lock
drwxrwxrwt  2 root          root              60 May 15 09:14 .X11-unix
-rw-------  1 kali          kali             398 May 15 09:14 .xfsm-ICE-HPOEP3
drwxrwxrwt  2 root          root              40 May 15 09:14 .XIM-unix
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo userdel -r backdoor_user                
userdel: user backdoor_user is currently used by process 439025
                                                                                            
┌──(kali㉿kali)-[~]
└─$ w
 11:50:19 up 14:52,  1 user,  load average: 0.18, 0.13, 0.09
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU  WHAT
kali              -                15May26         0.00s  0.07s lightdm --session-child 13 
                                                                                            
┌──(kali㉿kali)-[~]
└─$ rm /tmp/.hidden_payload
rm: remove write-protected regular file '/tmp/.hidden_payload'? y
rm: cannot remove '/tmp/.hidden_payload': Operation not permitted
                                                                                            
┌──(kali㉿kali)-[~]
└─$ grep "backdoor_user" /etc/passwd
backdoor_user:x:1004:1004::/home/backdoor_user:/bin/bash
                                                                                            
┌──(kali㉿kali)-[~]
└─$ 

                                                                                            
┌──(kali㉿kali)-[~]
└─$ 

                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo killall -u backdoor_user

Session terminated, killing shell...                                                                                            
┌──(kali㉿kali)-[~]
└─$  ...killed.
Terminated                 su - kali

┌──(backdoor_user㉿kali)-[~]
└─$ sudo userdel -r backdoor_user                                                           
userdel: user backdoor_user is currently used by process 439025

┌──(backdoor_user㉿kali)-[~]
└─$ exit                                                                                    
logout
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo killall -u backdoor_user
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo userdel -r backdoor_user
userdel: backdoor_user mail spool (/var/mail/backdoor_user) not found
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo userdel -r backdoor_user
userdel: user 'backdoor_user' does not exist
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo rm /tmp/.hidden_payload
                                                                                            
┌──(kali㉿kali)-[~]
└─$ grep "backdoor_user" /etc/passwd
                                                                                            
┌──(kali㉿kali)-[~]
└─$ lastb             
Command 'lastb' not found, did you mean:
  command 'last' from deb wtmpdb
  command 'lastz' from deb lastz
  command 'lasts' from deb multicat
  command 'lastdb' from deb last-align
Try: sudo apt install <deb name>
                                                                                            
┌──(kali㉿kali)-[~]
└─$ who   
kali     seat0        2026-05-15 09:14 (:0)
                                                                                            
┌──(kali㉿kali)-[~]
└─$ w  
 12:02:13 up 15:04,  1 user,  load average: 0.06, 0.05, 0.07
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU  WHAT
kali              -                15May26         0.00s  0.07s lightdm --session-child 13 
                                                                                            
┌──(kali㉿kali)-[~]
└─$ last -f /var/log/btmp 2>/dev/null || echo "No failed logs or file empty"
/var/log/btmp has no entries
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "authentication failure" /var/log/auth.log
grep: /var/log/auth.log: No such file or directory
                                                                                            
┌──(kali㉿kali)-[~]
└─$ sudo grep "Failed password" /var/log/auth.log
grep: /var/log/auth.log: No such file or directory
                                                                                            
┌──(kali㉿kali)-[~]
└─$ 
