# TNE30012-Secure-Access-Netwworks

# House keeping 

----------------------------------------------------
## Reset Scripts
### Routers
```cisco
tclsh
puts [ open "flash:router_reset.tcl" w+ ] {
typeahead "\n"
copy running-config startup-config
typeahead "\n"
erase startup-config
typeahead "\n"
puts "Reloading the Router in 1 minute, type reload cancel to halt"
typeahead "\n"
reload in 1 
}
tclquit
```
#### Start reset
```cisco
tclsh router_reset.tcl
```

### Layer 2 Switches
```cisco
tclsh
puts [ open "flash:als_reset.tcl" w+ ] {
typeahead "\n"
copy running-config startup-config
typeahead "\n"
erase startup-config
delete /force vlan.dat
delete /force multiple-fs
ios_config "sdm prefer lanbase-routing"
typeahead "\n"
puts "Reloading the switch in 1 minute, type reload cancel to halt"
typeahead "\n"
reload in 1 ALS_RESET.TCL SCRIPT RUN
}
tclquit
```
#### Start reset
```cisco
tclsh als_reset.tcl
```

### Layer 3 Switches
```cisco
tclsh
puts [ open "flash:dls_reset.tcl" w+ ] {
typeahead "\n"
copy running-config startup-config
typeahead "\n"
erase startup-config
delete /force vlan.dat
delete /force multiple-fs
typeahead "\n"
puts "Reloading the switch in 1 minute, type reload cancel to halt"
typeahead "\n"
reload in 1
}
tclquit
```
#### Start reset
```cisco
tclsh dls_reset.tcl
```

----------------------------------------------------
## Router 1 
```cisco
hostname R1 
banner motd * Warning unauthorized access is prohibited *

int s0/1/0
ip address 10.1.1.1 255.255.255.252
no shut
int gi0/0/1
ip address 192.168.1.1 255.255.255.0 
no shut
exi

router ospf 1 
network 192.168.1.0 0.0.0.255 area 0 
network 10.1.1.0 0.0.0.3 area 0 
passive-interface g0/0/1
exi
exi
```

----------------------------------------------------
## Router 2
```cisco
hostname R2
banner motd * Warning unauthorized access is prohibited *

int s0/1/0
ip address 10.1.1.2 255.255.255.252
no shut
int s0/1/1
ip address 10.2.2.2 255.255.255.252
no shut
exi

router ospf 1 
network 10.2.2.0 0.0.0.3 area 0
network 10.1.1.0 0.0.0.3 area 0 
exi
exi
```
----------------------------------------------------
## Router 3
```cisco
hostname R3
banner motd * Warning unauthorized access is prohibited *

int s0/1/1
ip address 10.2.2.1 255.255.255.252
no shut
int gi0/0/1
ip address 192.168.3.1 255.255.255.0 
no shut
exi

router ospf 1 
network 192.168.3.0 0.0.0.255 area 0 
network 10.2.2.0 0.0.0.3 area 0 
passive-interface g0/0/1
exi
exi
```

----------------------------------------------------
## Generic Users 
```cisco
conf t 
security passwords min-length 10 
enable algorithm-type scrypt secret cisco12345
username admin01 algorithm-type scrypt secret admin01pass
end
```
## SSH local auth 
```cisco
conf t 
ip domain name netsec.com 
crypto key zeroize RSA
y
crypto key generate rsa general-key modulus 2048
ip ssh version 2 
line vty 0 4 
privilege level 15
login local

transport input ssh
end 
```
----------------------------------------------------
## Generic VPN config 

#### Router 1 
```cisco
conf t 
crypto isakmp enable 
crypto isakmp policy 10 
hash sha 
authentication pre-share 
group 24 
lifetime 3600
encryption aes 256 
end
!
conf t 
crypto isakmp key cisco123 address 10.2.2.1 
!
crypto ipsec transform-set R1-R3 esp-aes 256 esp-sha-hmac
!
crypto ipsec security-association lifetime seconds 1800 
end 
!
conf t 
access-list 101 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255 
!
crypto map CMAP 10 ipsec-isakmp 
match address 101 
set peer 10.2.2.1
set pfs group24 
set transform-set R1-R3
set security-association lifetime seconds 900
!
end
!
conf t 
int s0/1/0
crypto map CMAP
end
!
```
#### Router 1 
```cisco
conf t 
crypto isakmp enable 
crypto isakmp policy 10 
crypto isakmp policy 10 
hash sha 
authentication pre-share 
group 24 
lifetime 3600
encryption aes 256 
end
!
conf t 
crypto isakmp key cisco123 address 10.1.1.1 
!
crypto ipsec transform-set R1-R3 esp-aes 256 esp-sha-hmac
exit
!
crypto ipsec security-association lifetime seconds 1800 
end 
!
conf t 
access-list 101 permit ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255 
!
crypto map CMAP 10 ipsec-isakmp 
match address 101 
set peer 10.1.1.1
set pfs group24 
set transform-set R1-R3
set security-association lifetime seconds 900
!
end
!
conf t 
int S0/1/1
crypto map CMAP
end
!
```
