```sql
└─$ nmap -sC -sV -p- -oN nmap 10.10.23.86
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-29 17:49 IST
Nmap scan report for 10.10.23.86
Host is up (0.13s latency).
Not shown: 65533 filtered ports
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: RETROWEB
|   NetBIOS_Domain_Name: RETROWEB
|   NetBIOS_Computer_Name: RETROWEB
|   DNS_Domain_Name: RetroWeb
|   DNS_Computer_Name: RetroWeb
|   Product_Version: 10.0.14393
|_  System_Time: 2021-05-29T12:58:37+00:00
| ssl-cert: Subject: commonName=RetroWeb
| Not valid before: 2021-05-28T12:18:58
|_Not valid after:  2021-11-27T12:18:58
|_ssl-date: 2021-05-29T12:58:39+00:00; -1s from scanner time.
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -1s, deviation: 0s, median: -1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 2359.27 seconds
```