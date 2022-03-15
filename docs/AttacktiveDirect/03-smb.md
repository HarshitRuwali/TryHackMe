```bash
└─$ enum4linux 10.10.121.26                                                                                    127 ⨯
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Tue Jun  1 15:20:26 2021

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 10.10.121.26
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ==================================================== 
|    Enumerating Workgroup/Domain on 10.10.121.26    |
 ==================================================== 
[E] Can't find workgroup/domain


 ============================================ 
|    Nbtstat Information for 10.10.121.26    |
 ============================================ 
Looking up status of 10.10.121.26
No reply from 10.10.121.26

 ===================================== 
|    Session Check on 10.10.121.26    |
 ===================================== 
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 437.
[+] Server 10.10.121.26 allows sessions using username '', password ''
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 451.
[+] Got domain/workgroup name: 

 =========================================== 
|    Getting domain SID for 10.10.121.26    |
 =========================================== 
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 359.
Domain Name: THM-AD
Domain Sid: S-1-5-21-3591857110-2884097990-301047963
[+] Host is part of a domain (not a workgroup)

 ====================================== 
|    OS information on 10.10.121.26    |
 ====================================== 
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 458.
Use of uninitialized value $os_info in concatenation (.) or string at ./enum4linux.pl line 464.
[+] Got OS info for 10.10.121.26 from smbclient: 
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 467.
[+] Got OS info for 10.10.121.26 from srvinfo:
Could not initialise srvsvc. Error was NT_STATUS_ACCESS_DENIED

 ============================= 
|    Users on 10.10.121.26    |
 ============================= 
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 866.
[E] Couldn't find users using querydispinfo: NT_STATUS_ACCESS_DENIED

Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 881.
[E] Couldn't find users using enumdomusers: NT_STATUS_ACCESS_DENIED

 ========================================= 
|    Share Enumeration on 10.10.121.26    |
 ========================================= 
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 640.

        Sharename       Type      Comment
        ---------       ----      -------
SMB1 disabled -- no workgroup available

[+] Attempting to map shares on 10.10.121.26

 ==================================================== 
|    Password Policy Information for 10.10.121.26    |
 ==================================================== 
[E] Unexpected error from polenum:


[+] Attaching to 10.10.121.26 using a NULL share

[+] Trying protocol 139/SMB...

        [!] Protocol failed: Cannot request session (Called Name:10.10.121.26)

[+] Trying protocol 445/SMB...

        [!] Protocol failed: SAMR SessionError: code: 0xc0000022 - STATUS_ACCESS_DENIED - {Access Denied} A process has requested access to an object but has not been granted those access rights.

Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 501.

[E] Failed to get password policy with rpcclient


 ============================== 
|    Groups on 10.10.121.26    |
 ============================== 
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 542.

[+] Getting builtin groups:

[+] Getting builtin group memberships:
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 542.

[+] Getting local groups:

[+] Getting local group memberships:
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 593.

[+] Getting domain groups:

[+] Getting domain group memberships:

```

 
`smbclient -U spookysec.local/svc-admin -L //10.10.121.26 `

![[Pasted image 20210601155629.png]]

`smbclient -U spookysec.local/svc-admin  //10.10.121.26/backup`

![[Pasted image 20210601155802.png]]

![[Pasted image 20210601155824.png]]

`backup@spookysec.local:backup2517860`