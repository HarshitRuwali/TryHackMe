## port 80

![[Pasted image 20210529105604.png]]

## fuzzing

`gobuster dir -u http://10.10.57.119/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50 -o gobuster`

and it gives bunch of 400 nothing else 

![[Pasted image 20210529123133.png]]



## port 49663
![[Pasted image 20210529123248.png]]

## fuzzing 

`gobuster dir -u http://10.10.57.119:49663/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50 -o gobuster`

and we got a hit for /nt4wrksv the same name as of smb folder

![[Pasted image 20210529132602.png]]

but its empty but /passwords.txt returns the passwords

![[Pasted image 20210529132757.png]]

