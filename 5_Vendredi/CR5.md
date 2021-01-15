#CR5




### ACL dyna

'access-list 101 permit tcp any host 2090.0.2.1 eq telnet'
'access-list 101 dynamic testlist timeout 15 permit ip 

simuler un
no ip route

ip default-gateway a.b.c.d
