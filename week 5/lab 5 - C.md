---------------------------------------------------------
## Router 3
```cisco
hostname R3
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
security passwords min-length 10
enable secret 8 $8$UxGw7y1YnuFA9w$H9WpkNn9QmIhslx2D/g0jM7m0iXmWITsOZ8wMebRep2
!
aaa new-model
!
!
aaa authentication login default local-case none
aaa authentication login SSH_LINES local
!
!
!
!
!
!
aaa session-id common
!
!
!
!
!
!
!
ip domain name netsec.com
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
license udi pid ISR4321/K9 sn FDO2144302L
license boot suite FoundationSuiteK9
license boot level securityk9
no license smart enable
diagnostic bootup level minimal
!
spanning-tree extend system-id
!
!
!
username user01 secret 8 $8$WGEvFIoucHkX8A$IP00xKM4FCxefseDRKFq0M4UnlYMQ4JwjTw3uOxb/do
username Admin01 privilege 15 secret 8 $8$wrKFbX3FFpyk0g$SXA9Ykh4We2SHdM7OwGU9Qu2FrraycatZ3QpiAk3eWY
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
 ip address 192.168.3.1 255.255.255.0
 negotiation auto
!
interface Serial0/1/0
 no ip address
!
interface Serial0/1/1
 ip address 10.2.2.1 255.255.255.252
!
interface Serial0/2/0
 no ip address
!
interface Serial0/2/1
 no ip address
!
interface GigabitEthernet0
 vrf forwarding Mgmt-intf
 no ip address
 negotiation auto
!
router ospf 1
 passive-interface GigabitEthernet0/0/1
 network 10.2.2.0 0.0.0.3 area 0
 network 192.168.3.0 0.0.0.255 area 0
!
ip forward-protocol nd
no ip http server
ip http secure-server
ip tftp source-interface GigabitEthernet0
!
ip ssh time-out 90
ip ssh authentication-retries 2
ip ssh version 2
!
!
!
!
!
!
!
control-plane
!
banner motd ^C Warning unauthorized access is prohibited ^C
!
line con 0
 exec-timeout 0 0
 password ciscoconpass
 logging synchronous
 transport input none
 stopbits 1
line aux 0
 password ciscovtypass
 stopbits 1
line vty 0 4
 privilege level 15
 password ciscoauxpass
 login authentication SSH_LINES
 transport input ssh
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
---------------------------------------------------------
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
security passwords min-length 10
enable secret 8 $8$SyOjuWYclhxl1I$3xqM7Z0.nmuIxbYF4MsrNjRNpKtwDJEbT4Dn7dERp6I
!
no aaa new-model
!
!
!
!
!
!
!
ip domain name netsec.com
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
vtp mode transparent
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
license udi pid ISR4321/K9 sn FDO211806H7
license boot suite FoundationSuiteK9
no license smart enable
diagnostic bootup level minimal
!
spanning-tree extend system-id
!
!
!
username user01 secret 8 $8$Og74u8v5IGy.sY$MvPs77UE/FQ1pfnnPKq800AqX.0Xop/qDDaU3m294U.
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
 ip address 10.1.1.1 255.255.255.252
!
interface Serial0/1/1
 no ip address
!
interface GigabitEthernet0
 vrf forwarding Mgmt-intf
 no ip address
 negotiation auto
!
router ospf 1
 passive-interface GigabitEthernet0/0/1
 network 10.1.1.0 0.0.0.3 area 0
 network 192.168.1.0 0.0.0.255 area 0
!
ip forward-protocol nd
no ip http server
ip http secure-server
ip tftp source-interface GigabitEthernet0
!
ip ssh time-out 90
ip ssh authentication-retries 2
ip ssh version 2
!
!
!
!
!
control-plane
!
banner motd ^C Warning unauthorized access is prohibited ^C
!
line con 0
 exec-timeout 0 0
 password ciscoconpass
 logging synchronous
 login local
 transport input none
 stopbits 1
line aux 0
 password ciscovtypass
 login local
 stopbits 1
line vty 0 4
 privilege level 15
 password ciscoauxpass
 login local
 transport input ssh
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
---------------------------------------------------------
## Router 2 
```cisco
hostname R2
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
!
no aaa new-model
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
license udi pid ISR4321/K9 sn FDO211513TL
license boot suite FoundationSuiteK9
license boot suite AdvUCSuiteK9
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
 shutdown
 negotiation auto
!
interface GigabitEthernet0/0/1
 no ip address
 shutdown
 negotiation auto
!
interface Serial0/1/0
 ip address 10.1.1.2 255.255.255.252
!
interface Serial0/1/1
 ip address 10.2.2.2 255.255.255.252
!
interface GigabitEthernet0
 vrf forwarding Mgmt-intf
 no ip address
 shutdown
 negotiation auto
!
router ospf 1
 network 10.1.1.0 0.0.0.3 area 0
 network 10.2.2.0 0.0.0.3 area 0
!
ip forward-protocol nd
ip http server
ip http authentication local
ip http secure-server
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
mgcp behavior rsip-range tgcp-only
mgcp behavior comedia-role none
mgcp behavior comedia-check-media-src disable
mgcp behavior comedia-sdp-force disable
!
mgcp profile default
!
!
!
!
!
banner motd ^C Warning unauthorized access is prohibited ^C
!
line con 0
 exec-timeout 0 0
 logging synchronous
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
