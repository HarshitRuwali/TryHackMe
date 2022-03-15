```bash
-bash-4.4# ./nmap_sp00f -v 10.200.109.150 -oN sp00f_scan_150

Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2021-04-10 06:40 BST
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Initiating ARP Ping Scan at 06:40
Scanning 10.200.109.150 [1 port]
Completed ARP Ping Scan at 06:40, 0.20s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 06:40
Completed Parallel DNS resolution of 1 host. at 06:40, 0.00s elapsed
Initiating SYN Stealth Scan at 06:40
Scanning ip-10-200-109-150.eu-west-1.compute.internal (10.200.109.150) [6150 ports]
Discovered open port 80/tcp on 10.200.109.150
Discovered open port 3389/tcp on 10.200.109.150
Discovered open port 5985/tcp on 10.200.109.150
Increasing send delay for 10.200.109.150 from 0 to 5 due to 17 out of 56 dropped probes since last increase.
Completed SYN Stealth Scan at 06:41, 67.99s elapsed (6150 total ports)
Nmap scan report for ip-10-200-109-150.eu-west-1.compute.internal (10.200.109.150)
Cannot find nmap-mac-prefixes: Ethernet vendor correlation will not be performed
Host is up (0.00037s latency).
Not shown: 6147 filtered ports
PORT     STATE SERVICE
80/tcp   open  http
3389/tcp open  ms-wbt-server
5985/tcp open  wsman
MAC Address: 02:0B:6D:4B:ED:13 (Unknown)

Read data files from: /etc
Nmap done: 1 IP address (1 host up) scanned in 68.24 seconds
           Raw packets sent: 18499 (813.924KB) | Rcvd: 57 (2.492KB)
-bash-4.4# 
```