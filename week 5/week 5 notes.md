password type commands 
- Console
```cisco
line console 0 
password cisco12345
login 
end
```
- Aux
```cisco
line vty 0 4 
password cisco12345
login
end
```
- VTY
```cisco
line aux 0 
password cisco 12345
login
end 
```