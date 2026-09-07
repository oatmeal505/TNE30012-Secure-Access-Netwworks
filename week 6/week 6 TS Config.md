--------------
## Router 1
```cisco
enable
config terminal
no ip domain lookup
enable algorithm-type sha256 secret cisco12345
username user01 algorithm-type sha256 secret user01pass
username admin privilege 15 algorithm-type sha256 secret cisco12345
ip domain-name netsec.com
host R1
interface Serial0/1/0
ip address 10.1.1.1 255.255.255.252
no shutdown
interface GigabitEthernet0/0/1
ip address 192.168.1.1 255.255.255.0
no shutdown
router ospf 1
passive-interface GigabitEthernet0/0/1
network 10.1.1.0 0.0.0.3 area 0
network 192.168.1.0 0.0.0.255 area 0
line con 0
login local
logging synchronous
exec-timeout 5 0
line aux 0
login local
exec-timeout 5 0
line vty 0 4
login local
exec-timeout 5 0
transport input ssh
crypto key generate rsa general-key modulus 1024
end
```
--------------
## Router 2 
```cisco
enable
config terminal
no ip domain lookup
host R2
enable algorithm-type sha256 secret cisco12345
username user01 algorithm-type sha256 secret user01pass
username admin privilege 15 algorithm-type sha256 secret cisco12345
ip domain-name netsec.com
interface Serial0/1/0
ip address 10.1.1.2 255.255.255.252
no shutdown
interface Serial0/1/1
ip address 10.2.2.2 255.255.255.252
no shutdown
router ospf 1
passive-interface GigabitEthernet0/0/1
network 10.1.1.0 0.0.0.3 area 0
network 10.2.2.0 0.0.0.3 area 0
ip route 192.168.1.0 255.255.255.0 10.1.1.1
ip route 192.168.3.0 255.255.255.0 10.2.2.1
line con 0
login local
logging synchronous
exec-timeout 5 0
line aux 0
login local
exec-timeout 5 0
line vty 0 4
login local
exec-timeout 5 0
transport input ssh
crypto key generate rsa general-key modulus 1024
end
```
--------------
## Router 3 
```cisco
enable
config terminal
no ip domain lookup
enable algorithm-type sha256 secret cisco12345
username user01 algorithm-type sha256 secret user01pass
username admin privilege 15 algorithm-type sha256 secret cisco12345
ip domain-name netsec.com
host R3
interface Serial0/1/1
ip address 10.2.2.1 255.255.255.252
no shutdown
interface GigabitEthernet0/0/1
ip address 192.168.3.1 255.255.255.0
no shutdown
router ospf 1
passive-interface GigabitEthernet0/0/1
network 10.2.2.0 0.0.0.3 area 0
network 192.168.1.0 0.0.0.255 area 0
line con 0
login local
logging synchronous
exec-timeout 5 0
line aux 0
login local
exec-timeout 5 0
line vty 0 4
login local
exec-timeout 5 0
transport input ssh
crypto key generate rsa general-key modulus 1024
end
```
