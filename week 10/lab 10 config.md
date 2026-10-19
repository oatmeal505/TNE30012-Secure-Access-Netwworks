----------------------------------------------------
## ASA
```cisco
: Saved

:
: Serial Number: JAD21460F4Y
: Hardware:   ASA5506, 4096 MB RAM, CPU Atom C2000 series 1250 MHz, 1 CPU (4 cores)
:
ASA Version 9.8(1)
!
hostname NETSEC-ASA
domain-name netsec.com
enable password $sha512$5000$8Uhr0JD4ZR5B54H63gN4Yw==$iJnGn8N+VFWPZZqfRS2JzQ== pbkdf2
xlate per-session deny tcp any4 any4
xlate per-session deny tcp any4 any6
xlate per-session deny tcp any6 any4
xlate per-session deny tcp any6 any6
xlate per-session deny udp any4 any4 eq domain
xlate per-session deny udp any4 any6 eq domain
xlate per-session deny udp any6 any4 eq domain
xlate per-session deny udp any6 any6 eq domain
passwd 2KFQnbNIdI.2KYOU encrypted
names

!
interface GigabitEthernet1/1
 shutdown
 no nameif
 no security-level
 no ip address
!
interface GigabitEthernet1/2
 shutdown
 no nameif
 no security-level
 no ip address
!
interface GigabitEthernet1/3
 shutdown
 no nameif
 no security-level
 no ip address
!
interface GigabitEthernet1/4
 shutdown
 no nameif
 no security-level
 no ip address
!
interface GigabitEthernet1/5
 nameif OUTSIDE
 security-level 0
 ip address 209.165.200.226 255.255.255.248
!
interface GigabitEthernet1/6
 nameif INSIDE
 security-level 100
 ip address 192.168.1.1 255.255.255.0
!
interface GigabitEthernet1/7
 nameif DMZ
 security-level 70
 ip address 192.168.2.1 255.255.255.0
!
interface GigabitEthernet1/8
 shutdown
 no nameif
 no security-level
 no ip address
!
interface Management1/1
 management-only
 shutdown
 nameif management
 security-level 0
 no ip address
!
ftp mode passive
dns server-group DefaultDNS
 domain-name netsec.com
object network INSIDE-NET
 subnet 192.168.1.0 255.255.255.0
object network DMZ-SERVER
 host 192.168.2.3
access-list OUTSIDE-DMZ extended permit ip any host 192.168.2.3
pager lines 24
mtu management 1500
mtu INSIDE 1500
mtu OUTSIDE 1500
mtu DMZ 1500
icmp unreachable rate-limit 1 burst-size 1
no asdm history enable
arp timeout 14400
no arp permit-nonconnected
arp rate-limit 16384
!
object network INSIDE-NET
 nat (INSIDE,OUTSIDE) dynamic interface
object network DMZ-SERVER
 nat (DMZ,OUTSIDE) static 209.165.200.227
access-group OUTSIDE-DMZ in interface OUTSIDE
route OUTSIDE 0.0.0.0 0.0.0.0 209.165.200.225 1
timeout xlate 3:00:00
timeout pat-xlate 0:00:30
timeout conn 1:00:00 half-closed 0:10:00 udp 0:02:00 sctp 0:02:00 icmp 0:00:02
timeout sunrpc 0:10:00 h323 0:05:00 h225 1:00:00 mgcp 0:05:00 mgcp-pat 0:05:00
timeout sip 0:30:00 sip_media 0:02:00 sip-invite 0:03:00 sip-disconnect 0:02:00
timeout sip-provisional-media 0:02:00 uauth 0:05:00 absolute
timeout tcp-proxy-reassembly 0:01:00
timeout floating-conn 0:00:00
timeout conn-holddown 0:00:15
timeout igp stale-route 0:01:10
user-identity default-domain LOCAL
aaa authentication ssh console LOCAL
aaa authentication login-history
no snmp-server location
no snmp-server contact
service sw-reset-button
crypto ipsec security-association pmtu-aging infinite
crypto ca trustpool policy
telnet timeout 5
ssh stricthostkeycheck
ssh 192.168.1.0 255.255.255.0 INSIDE
ssh 172.16.3.3 255.255.255.255 OUTSIDE
ssh timeout 10
ssh key-exchange group dh-group1-sha1
console timeout 0

dhcpd dns 209.165.201.2
dhcpd option 3 ip 192.168.1.1
!
dhcpd address 192.168.1.5-192.168.1.100 INSIDE
dhcpd enable INSIDE
!
threat-detection basic-threat
threat-detection statistics access-list
no threat-detection statistics tcp-intercept
dynamic-access-policy-record DfltAccessPolicy
username amdin password $sha512$5000$UrB4PoiEXEcCoqjpx/Euqw==$xkTxdjSpD5aNwbGHp3FMQg== pbkdf2
!
class-map inspection_default
 match default-inspection-traffic
!
!
policy-map type inspect dns preset_dns_map
 parameters
  message-length maximum client auto
  message-length maximum 512
  no tcp-inspection
policy-map global_policy
 class inspection_default
  inspect ftp
  inspect h323 h225
  inspect h323 ras
  inspect ip-options
  inspect netbios
  inspect rsh
  inspect rtsp
  inspect skinny
  inspect esmtp
  inspect sqlnet
  inspect sunrpc
  inspect tftp
  inspect sip
  inspect xdmcp
  inspect dns preset_dns_map
  inspect icmp
policy-map type inspect dns migrated_dns_map_2
 parameters
  message-length maximum client auto
  message-length maximum 512
  no tcp-inspection
policy-map type inspect dns migrated_dns_map_1
 parameters
  message-length maximum client auto
  message-length maximum 512
  no tcp-inspection
!
service-policy global_policy global
prompt hostname context
no call-home reporting anonymous
call-home
 profile CiscoTAC-1
  no active
  destination address http https://tools.cisco.com/its/service/oddce/services/DDCEService
  destination address email callhome@cisco.com
  destination transport-method http
  subscribe-to-alert-group diagnostic
  subscribe-to-alert-group environment
  subscribe-to-alert-group inventory periodic monthly
  subscribe-to-alert-group configuration periodic monthly
  subscribe-to-alert-group telemetry periodic daily
Cryptochecksum:334a6afcf122d527b7c889a33655bbb4
: end
```
