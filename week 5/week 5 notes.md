logging sync + no console time out 
```cisco 
line console 0 
logging synchronous 
exec-timeout 0 0 
end 
```
password type commands 
- Console
```cisco
line console 0 
password ciscoconpass
login 
end
```
- Aux
```cisco
line vty 0 4 
password ciscoauxpass
login
end
```
- VTY
```cisco
line aux 0 
password ciscovtypass
login
end 
```
local users 
```cisco
username user01 algorithm-type sha256 secret user01pass
end 
conf t 
line console 0 
login local 
end 
```
ssh 
```cisco 
ip domain-name netsec.com
line vty 0 4 
privilege level 15 
login local 
transport input ssh 
end 
conf t 
crypto key generate rsa general-keys modulus 1024
ip ssh time-out 90
ip ssh authentication-retries 2
ip ssh version 2
```
