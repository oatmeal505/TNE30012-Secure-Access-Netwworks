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
security passwords min-length 10
enable secret 9 $9$A8y8UgS02Bp6Gq$4hxYGS5R.Q/2l3/NUnv.SjX9XAEuYeDZQGiaDIONYbg
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
username admin01 secret 9 $9$IBK30Ki42vyGH4$zjGjrLWDt9mljkAG6AJkKjNh0nqo35niYwYrNEtY0Ws
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
crypto isakmp policy 10
 encr aes 256
 authentication pre-share
 group 24
 lifetime 3600
crypto isakmp key cisco123 address 10.2.2.1
!
crypto ipsec security-association lifetime seconds 1800
!
crypto ipsec transform-set R1-R3 esp-aes 256 esp-sha-hmac
 mode tunnel
!
!
!
crypto map CMAP 10 ipsec-isakmp
 set peer 10.2.2.1
 set security-association lifetime seconds 900
 set transform-set R1-R3
 set pfs group24
 match address 101
!
!
!
!
!
!
!
!
interface Loopback6
 ip address 60.60.60.1 255.255.255.0
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
 crypto map CMAP
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
 network 60.60.60.0 0.0.0.255 area 0
 network 192.168.1.0 0.0.0.255 area 0
!
ip forward-protocol nd
no ip http server
ip http secure-server
ip tftp source-interface GigabitEthernet0
!
ip ssh version 2
!
access-list 101 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255
access-list 101 permit ip 60.60.60.0 0.0.0.255 192.168.3.0 0.0.0.255
!
!
!
!
control-plane
!
banner motd ^C Warning unauthorized access is prohibited ^C
!
line con 0
 transport input none
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
 privilege level 15
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
license udi pid ISR4321/K9 sn FDO21472A5L
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
enable secret 9 $9$fxMG4GdSeUZnkv$/yBZq1HOdgX0IDf0vSMu.5BuPw2FyGEmgbr4UsQ0ABQ
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
license udi pid ISR4321/K9 sn FDO214504AL
license boot suite FoundationSuiteK9
license boot level securityk9
no license smart enable
diagnostic bootup level minimal
!
spanning-tree extend system-id
!
!
!
username admin01 secret 9 $9$tRYVQDQ7hV6XMv$oXf0zb9157lcYE3zVedmOafJ63FS6BJUQ2JTBv7M4iY
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
crypto isakmp policy 10
 encr aes 256
 authentication pre-share
 group 24
 lifetime 3600
crypto isakmp key cisco123 address 10.1.1.1
!
crypto ipsec security-association lifetime seconds 1800
!
crypto ipsec transform-set R1-R3 esp-aes 256 esp-sha-hmac
 mode tunnel
!
!
!
crypto map CMAP 10 ipsec-isakmp
 set peer 10.1.1.1
 set security-association lifetime seconds 900
 set transform-set R1-R3
 set pfs group24
 match address 101
!
!
!
!
!
!
!
!
interface Loopback5
 ip address 50.50.50.1 255.255.255.0
!
interface Loopback7
 ip address 70.70.70.1 255.255.255.0
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
 crypto map CMAP
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
 network 50.50.50.0 0.0.0.255 area 0
 network 192.168.3.0 0.0.0.255 area 0
!
ip forward-protocol nd
no ip http server
ip http secure-server
ip tftp source-interface GigabitEthernet0
!
ip ssh version 2
!
access-list 101 permit ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255
access-list 101 permit ip 192.168.3.0 0.0.0.255 60.60.60.0 0.0.0.255
!
!
!
!
control-plane
!
banner motd ^C Warning unauthorized access is prohibited ^C
!
line con 0
 transport input none
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
 privilege level 15
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
