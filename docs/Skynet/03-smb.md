`smbmap -H 10.10.82.251`   

![[Pasted image 20210528150219.png]]

from here we see the user name for miles dyson is `milesdyson`

`smbclient "\\\\10.10.82.251\anonymous"`

get the logs and attention 
![[Pasted image 20210528150411.png]]

![[Pasted image 20210528150631.png]]

## logging in as milesdyson

![[Pasted image 20210528155057.png]]

here there a file important.txt different from the others

![[Pasted image 20210528155113.png]]

cat important.txt
![[Pasted image 20210528155149.png]]

`dir: /45kra24zxs28v3yd`