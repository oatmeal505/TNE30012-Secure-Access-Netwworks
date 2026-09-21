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
enable secret 9 $9$Bbe8eNvzDTAYsW$i3hOj/ojyoHXuzHBUqzWB77lA6YNkvWaX43gmt2y2YY
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
username admin01 secret 9 $9$1eGdZIFFB0IMK0$WKazgzpFseN3.RFglLbwXMcsFl7Xf3jVZVTijSw.JZg
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
 ip address 200.100.1.1 255.255.255.0
 negotiation auto
!
interface Serial0/1/0
 ip address 172.16.1.1 255.255.255.252
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
ip http secure-server
ip tftp source-interface GigabitEthernet0
ip route 0.0.0.0 0.0.0.0 172.16.1.2
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
 logging synchronous
 login local
 transport input none
 stopbits 1
line aux 0
 exec-timeout 0 0
 login local
 stopbits 1
line vty 0 4
 exec-timeout 0 0
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
 ip address 172.16.1.2 255.255.255.0
!
interface Serial0/1/1
 ip address 172.16.2.2 255.255.255.252
!
interface GigabitEthernet0
 vrf forwarding Mgmt-intf
 no ip address
 shutdown
 negotiation auto
!
ip forward-protocol nd
ip http server
ip http authentication local
ip http secure-server
ip tftp source-interface GigabitEthernet0
ip route 200.100.1.0 255.255.255.0 172.16.1.1
ip route 200.100.3.0 255.255.255.0 172.16.2.1
ip route 200.100.33.0 255.255.255.0 172.16.2.1
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
class-map type inspect match-any CONFROOM_PROTOCOLS
 match protocol http
 match protocol https
 match protocol dns
class-map type inspect match-any INSIDE_PROTOCOLS
 match protocol tcp
 match protocol udp
 match protocol icmp
!
policy-map type inspect CONFROOM_TO_INTERNET
 class type inspect CONFROOM_PROTOCOLS
  inspect
 class class-default
policy-map type inspect INSIDE_TO_INTERNET
 class type inspect INSIDE_PROTOCOLS
  inspect
 class class-default
!
zone security INSIDE
zone security CONFROOM
zone security INTERNET
zone-pair security CONFROOM_TO_INTERNET source CONFROOM destination INTERNET
 service-policy type inspect CONFROOM_TO_INTERNET
zone-pair security INSIDE_TO_INTERNET source INSIDE destination INTERNET
 service-policy type inspect INSIDE_TO_INTERNET
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
 zone-member security INTERNET
 negotiation auto
!
interface GigabitEthernet0/0/1
 no ip address
 negotiation auto
!
interface GigabitEthernet0/0/1.3
 encapsulation dot1Q 3
 ip address 200.100.3.1 255.255.255.0
 zone-member security INSIDE
!
interface GigabitEthernet0/0/1.33
 encapsulation dot1Q 33
 ip address 200.100.33.1 255.255.255.0
 zone-member security CONFROOM
!
interface Serial0/1/0
 no ip address
 shutdown
!
interface Serial0/1/1
 ip address 172.16.2.1 255.255.255.252
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
ip forward-protocol nd
no ip http server
ip http secure-server
ip tftp source-interface GigabitEthernet0
ip route 0.0.0.0 0.0.0.0 172.16.2.2
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
## Switch 3
```cisco
hostname S3
!
!
vrf definition Mgmt-vrf
 !
 address-family ipv4
 exit-address-family
 !
 address-family ipv6
 exit-address-family
!
!
no aaa new-model
switch 1 provision ws-c3650-24ts
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
cpp system-default
!
!
!
diagnostic bootup level minimal
spanning-tree mode rapid-pvst
spanning-tree extend system-id
!
!
redundancy
 mode sso
!
!
!
class-map match-any system-cpp-police-topology-control
  description Topology control
class-map match-any system-cpp-police-sw-forward
  description Sw forwarding, L2 LVX data, LOGGING
class-map match-any system-cpp-default
  description DHCP Snooping, EWLC control, EWCL data
class-map match-any system-cpp-police-sys-data
  description Learning cache ovfl, Crypto Control, Exception, EGR Exception, NFL SAMPLED DATA, Gold Pkt, RPF Failed
class-map match-any system-cpp-police-punt-webauth
  description Punt Webauth
class-map match-any system-cpp-police-l2lvx-control
  description L2 LVX control packets
class-map match-any system-cpp-police-forus
  description Forus Address resolution and Forus traffic
class-map match-any system-cpp-police-multicast-end-station
  description MCAST END STATION
class-map match-any system-cpp-police-multicast
  description Transit Traffic and MCAST Data
class-map match-any system-cpp-police-l2-control
  description L2 control
class-map match-any system-cpp-police-dot1x-auth
  description DOT1X Auth
class-map match-any system-cpp-police-data
  description ICMP redirect, ICMP_GEN and BROADCAST
class-map match-any system-cpp-police-stackwise-virt-control
  description Stackwise Virtual
class-map match-any system-cpp-police-control-low-priority
  description General punt
class-map match-any non-client-nrt-class
class-map match-any system-cpp-police-routing-control
  description Routing control
class-map match-any system-cpp-police-protocol-snooping
  description Protocol snooping
!
policy-map system-cpp-policy
 class system-cpp-police-data
  police rate 600 pps
 class system-cpp-police-sys-data
  police rate 100 pps
 class system-cpp-police-sw-forward
  police rate 1000 pps
 class system-cpp-police-multicast
  police rate 500 pps
 class system-cpp-police-multicast-end-station
  police rate 2000 pps
 class system-cpp-police-punt-webauth
  police rate 1000 pps
 class system-cpp-police-l2-control
  police rate 2000 pps
 class system-cpp-police-stackwise-virt-control
  police rate 8000 pps
 class system-cpp-police-routing-control
  police rate 5400 pps
 class system-cpp-police-control-low-priority
  police rate 200 pps
 class system-cpp-police-l2lvx-control
  police rate 1000 pps
 class system-cpp-police-topology-control
  police rate 13000 pps
 class system-cpp-police-dot1x-auth
  police rate 1000 pps
 class system-cpp-police-protocol-snooping
  police rate 2000 pps
 class system-cpp-police-forus
  police rate 1000 pps
 class system-cpp-default
  police rate 1000 pps
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
interface GigabitEthernet0/0
 vrf forwarding Mgmt-vrf
 no ip address
 shutdown
 speed 1000
 negotiation auto
!
interface GigabitEthernet1/0/1
!
interface GigabitEthernet1/0/2
!
interface GigabitEthernet1/0/3
!
interface GigabitEthernet1/0/4
!
interface GigabitEthernet1/0/5
!
interface GigabitEthernet1/0/6
!
interface GigabitEthernet1/0/7
!
interface GigabitEthernet1/0/8
!
interface GigabitEthernet1/0/9
!
interface GigabitEthernet1/0/10
!
interface GigabitEthernet1/0/11
 switchport mode trunk
!
interface GigabitEthernet1/0/12
 switchport access vlan 3
 switchport mode access
!
interface GigabitEthernet1/0/13
!
interface GigabitEthernet1/0/14
!
interface GigabitEthernet1/0/15
!
interface GigabitEthernet1/0/16
!
interface GigabitEthernet1/0/17
!
interface GigabitEthernet1/0/18
!
interface GigabitEthernet1/0/19
!
interface GigabitEthernet1/0/20
!
interface GigabitEthernet1/0/21
!
interface GigabitEthernet1/0/22
!
interface GigabitEthernet1/0/23
!
interface GigabitEthernet1/0/24
 switchport access vlan 33
 switchport mode access
!
interface GigabitEthernet1/1/1
!
interface GigabitEthernet1/1/2
!
interface GigabitEthernet1/1/3
!
interface GigabitEthernet1/1/4
!
interface Vlan1
 no ip address
 shutdown
!
ip forward-protocol nd
ip http server
ip http authentication local
ip http secure-server
!
!
!
!
!
!
control-plane
 service-policy input system-cpp-policy
!
!
line con 0
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
 login
line vty 5 15
 login
!
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

