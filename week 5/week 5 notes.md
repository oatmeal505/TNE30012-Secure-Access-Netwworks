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