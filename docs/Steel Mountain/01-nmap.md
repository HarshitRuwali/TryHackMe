## nmap

```sql
# Nmap 7.91 scan initiated Thu May 27 12:42:31 2021 as: nmap -sC -sV -oN nmap 10.10.143.187
Nmap scan report for 10.10.143.187
Host is up (0.13s latency).
Not shown: 988 closed ports
PORT      STATE SERVICE            VERSION
80/tcp    open  http               Microsoft IIS httpd 8.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/8.5
|_http-title: Site doesn't have a title (text/html).
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=steelmountain
| Not valid before: 2021-05-26T07:10:40
|_Not valid after:  2021-11-25T07:10:40
|_ssl-date: 2021-05-27T07:13:56+00:00; 0s from scanner time.
8080/tcp  open  http               HttpFileServer httpd 2.3
|_http-server-header: HFS 2.3
|_http-title: HFS /
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49157/tcp open  msrpc              Microsoft Windows RPC
49163/tcp open  msrpc              Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: STEELMOUNTAIN, NetBIOS user: <unknown>, NetBIOS MAC: 02:a2:8e:4d:4c:21 (unknown)
| smb-security-mode: 
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-05-27T07:13:50
|_  start_date: 2021-05-27T07:10:30

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu May 27 12:43:56 2021 -- 1 IP address (1 host up) scanned in 85.70 seconds
```