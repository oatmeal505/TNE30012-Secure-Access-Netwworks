----------
## Router 1 
```cisco
hostname R1
!
boot-start-marker
boot-end-marker
!
!
vrf definition Mgmt-intf
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
 exit-address-family
!
enable secret 9 $9$VegNgeD7LQ7HNU$J5B9FJ//xixdRMvtVwVtDV5unJy07zfo50wGmDnjpEs
!
no aaa new-model
!
!
!
!
!
!
!
no ip domain lookup
!
!
!
!
!
!
!
!
!
!
subscriber templating
!
!
!
!
!
!
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
license udi pid ISR4321/K9 sn FDO21162L3U
license boot suite FoundationSuiteK9
license boot level securityk9
no license smart enable
diagnostic bootup level minimal
!
spanning-tree extend system-id
!
!
!
!
redundancy
 mode none
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
interface GigabitEthernet0/0/0
 no ip address
 negotiation auto
!
interface GigabitEthernet0/0/1
 ip address 192.168.1.1 255.255.255.0
 negotiation auto
!
interface Serial0/1/0
 no ip address
!
interface Serial0/1/1
 no ip address
!
interface GigabitEthernet0
 vrf forwarding Mgmt-intf
 no ip address
 negotiation auto
!
ip forward-protocol nd
no ip http server
no ip http secure-server
ip tftp source-interface GigabitEthernet0
!
!
!
!
!
!
control-plane
!
!
line con 0
 exec-timeout 0 0
 password ciscoconpass
 logging synchronous
 login
 transport input none
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
 login
!
wsma agent exec
!
wsma agent config
!
wsma agent filesys
!
wsma agent notify
!
!
end
```
----------
## Switch 1 
```cisco
hostname S1
!
boot-start-marker
boot-end-marker
!
enable secret 9 $9$erAaJ/FHaVV0xq$XD/pR2ZNYxoM4fg8em5ZeTpEjKcQMWc8PPgWZAkkd06
!
no aaa new-model
system mtu routing 1500
!
!
no ip domain-lookup
!
!
!
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
spanning-tree vlan 1 priority 0
!
vlan internal allocation policy ascending
!
!
!
!
!
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
 switchport trunk native vlan 99
 switchport mode trunk
 switchport nonegotiate
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
 switchport mode access
 spanning-tree portfast
!
interface FastEthernet0/12
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 ip address 192.168.1.2 255.255.255.0
!
no ip http server
no ip http secure-server
!
!
!
line con 0
 exec-timeout 0 0
 password ciscoconpass
 logging synchronous
 login
line vty 0 4
 login
line vty 5 15
 login
!
end
```
----------
## Switch 2
```cisco
hostname S2
!
boot-start-marker
boot-end-marker
!
enable secret 9 $9$mHrz1OpckzC6Ja$rlA2FGLGupJlKI86Tv8N88rLhKzjYhvZQcIZxlqTTSo
!
no aaa new-model
system mtu routing 1500
!
!
no ip domain-lookup
!
!
!
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree loopguard default
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
!
!
!
!
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
 switchport trunk native vlan 99
 switchport mode trunk
 switchport nonegotiate
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface GigabitEthernet0/1
 spanning-tree guard root
!
interface GigabitEthernet0/2
!
interface Vlan1
 ip address 192.168.1.3 255.255.255.0
!
no ip http server
no ip http secure-server
!
!
!
line con 0
 exec-timeout 0 0
 password ciscoconpass
 logging synchronous
 login
line vty 0 4
 login
line vty 5 15
 login
!
end
```
