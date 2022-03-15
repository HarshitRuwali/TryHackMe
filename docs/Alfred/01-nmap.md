```sql
└─$ nmap -sC -sV -oN nmap 10.10.6.89   
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-27 15:28 IST
Nmap scan report for 10.10.6.89
Host is up (0.14s latency).
Not shown: 997 filtered ports
PORT     STATE SERVICE            VERSION
80/tcp   open  http               Microsoft IIS httpd 7.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: Site doesn't have a title (text/html).
3389/tcp open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=alfred
| Not valid before: 2021-05-26T09:57:29
|_Not valid after:  2021-11-25T09:57:29
|_ssl-date: 2021-05-27T09:59:01+00:00; 0s from scanner time.
8080/tcp open  http               Jetty 9.4.z-SNAPSHOT
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.22 seconds
```