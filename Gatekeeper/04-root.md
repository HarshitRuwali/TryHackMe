in the desktop itself there is Firefox shortcut

![[Pasted image 20210531142053.png]]

upgraded the shell to meterpreter

and there might be some more info we can get from firefox.

`C:\users\natbat\AppData\roaming\Mozilla\Firefox\Profiles\ljfn812a.default-release`

we got the files including key4.db

get the files `cert9.db  cookies.sqlite  key4.db  logins.json`

and crack using https://github.com/unode/firefox_decrypt

![[Pasted image 20210531144324.png]]



```
Username: 'mayor'
Password: '8CL7O1N78MdrCIsV'
```

using `impacket-psexec` to login

![[Pasted image 20210531145347.png]]


![[Pasted image 20210531145504.png]]

root === {Th3_M4y0r_C0ngr4tul4t3s_U}
