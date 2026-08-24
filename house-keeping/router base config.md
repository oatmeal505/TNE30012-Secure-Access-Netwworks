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
