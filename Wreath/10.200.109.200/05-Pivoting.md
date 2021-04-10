Running nmap for getting the other machines in the network

```bash
-bash-4.4# ./nmap_sp00f -sn 10.200.109.1-255 -oN scan_sp00f

Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2021-04-10 06:32 BST
Cannot find nmap-payloads. UDP payloads are disabled.
Nmap scan report for ip-10-200-109-1.eu-west-1.compute.internal (10.200.109.1)
Cannot find nmap-mac-prefixes: Ethernet vendor correlation will not be performed
Host is up (0.00028s latency).
MAC Address: 02:10:71:E5:86:01 (Unknown)
Nmap scan report for ip-10-200-109-100.eu-west-1.compute.internal (10.200.109.100)
Host is up (0.00014s latency).
MAC Address: 02:CF:1B:5D:57:4F (Unknown)
Nmap scan report for ip-10-200-109-150.eu-west-1.compute.internal (10.200.109.150)
Host is up (-0.10s latency).
MAC Address: 02:0B:6D:4B:ED:13 (Unknown)
Nmap scan report for ip-10-200-109-250.eu-west-1.compute.internal (10.200.109.250)
Host is up (0.00016s latency).
MAC Address: 02:2F:98:4C:5D:77 (Unknown)
Nmap scan report for ip-10-200-109-200.eu-west-1.compute.internal (10.200.109.200)
Host is up.
Nmap done: 255 IP addresses (5 hosts up) scanned in 3.73 seconds
-bash-4.4# 
```

Scanning port 10.200.109.100

```bash
-bash-4.4# ./nmap_sp00f -v 10.200.109.100 -oN sp00f_scan_100

Starting Nmap 6.49BETA1 ( http://nmap.org ) at 2021-04-10 06:37 BST
Unable to find nmap-services!  Resorting to /etc/services
Cannot find nmap-payloads. UDP payloads are disabled.
Initiating ARP Ping Scan at 06:37
Scanning 10.200.109.100 [1 port]
Completed ARP Ping Scan at 06:37, 0.21s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 06:37
Completed Parallel DNS resolution of 1 host. at 06:37, 0.00s elapsed
Initiating SYN Stealth Scan at 06:37
Scanning ip-10-200-109-100.eu-west-1.compute.internal (10.200.109.100) [6150 ports]
SYN Stealth Scan Timing: About 24.07% done; ETC: 06:39 (0:01:38 remaining)
SYN Stealth Scan Timing: About 48.33% done; ETC: 06:39 (0:01:05 remaining)
SYN Stealth Scan Timing: About 72.64% done; ETC: 06:39 (0:00:34 remaining)
Completed SYN Stealth Scan at 06:39, 124.32s elapsed (6150 total ports)
Nmap scan report for ip-10-200-109-100.eu-west-1.compute.internal (10.200.109.100)
Cannot find nmap-mac-prefixes: Ethernet vendor correlation will not be performed
Host is up (-0.20s latency).
All 6150 scanned ports on ip-10-200-109-100.eu-west-1.compute.internal (10.200.109.100) are filtered
MAC Address: 02:CF:1B:5D:57:4F (Unknown)

Read data files from: /etc
Nmap done: 1 IP address (1 host up) scanned in 124.57 seconds
           Raw packets sent: 12302 (541.256KB) | Rcvd: 1 (28B)
-bash-4.4# 
```

Here all ports are filtered

scanning port 150

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

pivoting using sshuttle

```bash
sshuttle -r root@10.200.109.200 --ssh-cmd "ssh -i id_rsa_root" 10.200.109.0/24 -x 10.200.109.200
```
![[Screenshot 2021-04-10 at 11.32.14 AM.png]]

![[Screenshot 2021-04-10 at 11.32.25 AM.png]]
