```sql
─$ nmap -sC -sV -oN nmap 10.10.201.122
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-27 18:03 IST
Nmap scan report for 10.10.201.122
Host is up (0.14s latency).
Not shown: 998 filtered ports
PORT     STATE SERVICE            VERSION
80/tcp   open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| http-methods: 
|_  Potentially risky methods: TRACE
| http-robots.txt: 6 disallowed entries 
| /Account/*.* /search /search.aspx /error404.aspx 
|_/archive /archive.aspx
|_http-server-header: Microsoft-IIS/8.5
|_http-title: hackpark | hackpark amusements
3389/tcp open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=hackpark
| Not valid before: 2021-05-26T12:31:05
|_Not valid after:  2021-11-25T12:31:05
|_ssl-date: 2021-05-27T12:33:33+00:00; 0s from scanner time.
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 33.24 seconds
```
