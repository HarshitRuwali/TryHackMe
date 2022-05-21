Getting system information `systeminfo`

![[Pasted image 20220521134553.png]]

Get details about a user `net user username`

![[Pasted image 20220521134805.png]]


Details for all the process that are executed when a system starts are can be found in one of the Registry Entries i.e. `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`

And get details for those processes 

`Get-Item "Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"`

![[Pasted image 20220521143746.png]]


Get details who have admin privilages `net localgroup administrators`

![[Pasted image 20220521143949.png]]


List of all the scheduled tasks can be found using the command: `Get-ScheduledTask`

![[Pasted image 20220521144137.png]]

- So, we can get details of the process using the command: `Get-ScheduledTaskInfo -TaskName "<scheduled_task_name>"`

- But this does not provide us with the actions that are being performed by the scheduled task. To get that information, we can use the command: `Get-ScheduledTask -TaskName "<scheduled_task_name>" | Select *`
	This query provides all the details of the scheduled task but again not the exact thing that is the command being executed by the task.
    
- To get the details of the action that are being performed by the scheduled task, the command that can be used is: `(Get-ScheduledTask -TaskName "<scheduled_task_name>").Actions`

![[Pasted image 20220521144750.png]]

To get the date of compromise we first see when the files were created in the TMP folder

![[Pasted image 20220521145259.png]]

Here, it can be seen the date when the files were written but to be sure about the creation time of file, we can use the command: `(Get-ChildItem <suspicious_task_file>).CreationTime`

![[Pasted image 20220521145429.png]]


The event ID generated when special privileges are assigned to a new logon is 4672. So, we can look for events associated with this event ID around the time when the system got compromised.

Again, there are going to be a lot many entries for the username “SYSTEM” in the output so we can filter them out

`Get-EventLog -LogName Security -After 3/2/2019 -InstanceId 4672 | where {$_.Message -notmatch "SYSTEM"} | select *``

This generates a lot many entries but we need to find the specific one when required for answering this question.

But even after trying a lot, I was not able to find the exact event. So, looked up in the hint and found an entry with similar entry in the output and that worked as the answer to the question as well. 
`Get-EventLog -LogName Security -Index 151109 -InstanceId 4672 | select *`

![[Pasted image 20220521145652.png]]

Reading the min-out.file we get to know that Mimikatz was used to get passwords from the machine

![[Pasted image 20220521145751.png]]

Control and Command(CnC/C2) server. In general, when an attacker hacks a machine that is connected to a network, they will set up a server between them and the target machine: the attacker will send the commands to the C2 server, which will then feed it to the target machine. The C2 server as well serves as a quick repository for any data exfiltrated from the target machine, though the attacker will generally remove the data from the C2 server and place it somewhere more secure.

In general, an attacker will oftentimes add the C2 server IP address to the hosts file: if you catch the attacker before they are finished and covered their tracks, you may be able to see it there. 
`Get-ChildItem -Path C:\ -Include hosts -Recurse`

![[Pasted image 20220521150541.png]]

from here we get to know the path and then simply read the hosts file

![[Pasted image 20220521150618.png]]

Here there is an extra entry of google with external ip, rest are all redirected to localhost. So the ip of the c2 server is 76.32.97.132

All the files associated with a Web Server on a Windows server machine are stored at `C:\inetpub\wwwroot`

![[Pasted image 20220521150903.png]]

Details for the ports that are open or close can be found in the firewall rules. So, we can try to dump the contents of the inbound and outbound firewall rules to look for any odd port for which a rule has been specifically created.

`Get-NetFirewallRule -Direction Inbound | select DisplayName, DisplayGroup, @{Name='LocalPort';Expression={($PSItem | Get-NetFirewallPortFilter).LocalPort}}`

![[Pasted image 20220521151219.png]]
Here its the last port opened by the attacker


Check for DNS poisoning, what site was targeted?
In this we get google.com was targeted, as seen in the hosts file.
