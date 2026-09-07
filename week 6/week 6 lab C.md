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
enable secret 8 $8$gF8IAKvriyvOIj$C6ERROtpC97AJKsICmOEU286B1IN5FORXn5j8KBE0sY
!
aaa new-model
!
!
aaa authentication login default group radius none
aaa authentication login SSH_LINES group radius
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
no ip domain lookup
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
license udi pid ISR4321/K9 sn FDO214729W7
license boot suite FoundationSuiteK9
license boot level securityk9
no license smart enable
diagnostic bootup level minimal
!
spanning-tree extend system-id
!
!
!
username user01 secret 8 $8$Bg.MFBQWO98tED$JdcZ45Pkoz6lGgx8C7EYB5YZvRl5JddmHRrA6jKhJsk
username admin privilege 15 secret 8 $8$2q2XhyOnqNDuJz$pBaF.cjexRhSM23yC4LTLu3kwOUaSGQQoon/MlEgKT.
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
 ip address 192.168.1.1 255.255.255.0
 negotiation auto
!
interface Serial0/1/0
 ip address 10.1.1.1 255.255.255.252
!
interface Serial0/1/1
 no ip address
 shutdown
!
interface GigabitEthernet0
 vrf forwarding Mgmt-intf
 no ip address
 shutdown
 negotiation auto
!
router ospf 1
 passive-interface GigabitEthernet0/0/1
 network 10.1.1.0 0.0.0.3 area 0
 network 192.168.1.0 0.0.0.255 area 0
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
!
radius server NetSec
 address ipv4 192.168.1.3 auth-port 1812 acct-port 1813
 key $trongPass
!
!
control-plane
!
!
line con 0
 exec-timeout 5 0
 logging synchronous
 transport input none
 stopbits 1
line aux 0
 exec-timeout 5 0
 stopbits 1
line vty 0 4
 exec-timeout 5 0
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
----------
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
enable secret 8 $8$2t9GQjOLvHmAn0$7GYWykSmOu0siCG6jQhEVLQShxGC6WwXyCqPA8k/xzg
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
license udi pid ISR4321/K9 sn FDO21472B3D
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
username user01 secret 8 $8$bHjvjUykszuKxG$rJd/X5gMVh/z/QbJDbbNZMEPNzg8P9tiP7yDA6Lq6Fw
username admin privilege 15 secret 8 $8$9Le0T4AueTFLQG$AjNUi4zauoaf5pen8xETAzrfnONwU9DfDohGeVSTjys
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
 passive-interface GigabitEthernet0/0/1
 network 10.1.1.0 0.0.0.3 area 0
 network 10.2.2.0 0.0.0.3 area 0
!
ip forward-protocol nd
ip http server
ip http authentication local
ip http secure-server
ip tftp source-interface GigabitEthernet0
ip route 192.168.1.0 255.255.255.0 10.1.1.1
ip route 192.168.3.0 255.255.255.0 10.2.2.1
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
!
line con 0
 exec-timeout 5 0
 logging synchronous
 login local
 transport input none
 stopbits 1
line aux 0
 exec-timeout 5 0
 login local
 stopbits 1
line vty 0 4
 exec-timeout 5 0
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
----------
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
enable secret 8 $8$FtnO4qlI7BDLlh$WSquGAaTzMEYv/QHzeZiF54GKTWC1zN6Gy9eYl7B1M.
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
no ip domain lookup
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
license udi pid ISR4321/K9 sn FDO214504YE
license accept end user agreement
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
username user01 secret 8 $8$UO5BPeJ3wTZqvA$3y.G962aqr5uWh0sx89h0pzjlECFmJVdLhpQJD.VBLM
username admin privilege 15 secret 8 $8$yxtap1Sc45PiGA$8J8MDON9W49/S03iLKC1uQDayu8J16mN6lClkrR0yW2
username Admin01 privilege 15 secret 8 $8$7cmDmjJRm9Ff/w$NdSVQd7/mT8tovNfKCchy0L7FdyFynjXl8ElysPwnMs
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
 ip address 192.168.3.1 255.255.255.0
 negotiation auto
!
interface Serial0/1/0
!
interface Serial0/1/1
 ip address 10.2.2.1 255.255.255.252
!
interface Serial0/2/0
!
interface Serial0/2/1
 no ip address
 shutdown
!
interface GigabitEthernet0
 vrf forwarding Mgmt-intf
 no ip address
 shutdown
 negotiation auto
!
router ospf 1
 passive-interface GigabitEthernet0/0/1
 network 10.2.2.0 0.0.0.3 area 0
 network 192.168.3.0 0.0.0.255 area 0
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
!
line con 0
 exec-timeout 5 0
 logging synchronous
 transport input none
 stopbits 1
line aux 0
 exec-timeout 5 0
 stopbits 1
line vty 0 4
 exec-timeout 5 0
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
