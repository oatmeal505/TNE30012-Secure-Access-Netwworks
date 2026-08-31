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