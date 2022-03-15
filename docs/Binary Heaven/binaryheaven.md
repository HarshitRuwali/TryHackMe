# Binary Heaven

We are given the 2 binaries, from angel_A we get the username

## username

import into ghidra and get the username raw bytes 
![[Pasted image 20210523000749.png]]

```
6b 79 6d 7e 68 75 6d 72
```

now in the main function, calculations are done to get the alphabets

`username[i] = (entered[i]^4)+8`
now modifiying to get the entered values:

`(entered[i]-8)^4 = entered[i]`

and printing the chars for the decimals using the eqns

```python
a = [0x6b, 0x79, 0x6d, 0x7e, 0x68, 0x75, 0x6d, 0x72]

for i in a:
	print(chr((i-8)^4), end="")
```

and prints `guardian`


