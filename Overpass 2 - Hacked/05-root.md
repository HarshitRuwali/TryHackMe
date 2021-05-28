as we dont have the james passwd nor can run sudo directly 

but in the directory there is binary .suid_bash

![[Pasted image 20210529011048.png]]

and simply running it spawns a shell but but a new pipe line (-p flag gives us root)

![[Pasted image 20210529011147.png]]


root === thm{d53b2684f169360bb9606c333873144d}