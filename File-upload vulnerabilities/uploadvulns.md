jewel.uploadvulns.thm

web is  in node.js 


fuzzing

![[Pasted image 20210603202507.png]]

![[Pasted image 20210603201835.png]]



burp config allow the js to intercept 
![[Pasted image 20210603202039.png]]

and clear the cache

intercept the request 
![[Pasted image 20210603202119.png]]

![[Pasted image 20210603202346.png]]

![[Pasted image 20210603202430.png]]

After uploading re run the go buster scan as the file uploaded gets renamed

I have uploaded few file to test so result may differ

![[Pasted image 20210603205043.png]]

so getting the file name AMV.jpg

call the file from admin page
![[Pasted image 20210603205124.png]]

we got the flag
![[Pasted image 20210603205157.png]]

flag === THM{NzRlYTUwNTIzODMwMWZhMzBiY2JlZWU2}
