## Pass
###### Why is R3 displaying a zone named “self”? What is the significance of this zone? 
The self zone is present by default on all cisco routers, this zone is dedicated to any traffic that has a destination of the router or origonates from the router itself. this is nessacary for managing any traffic that is destined to the router or coming from the routers. 

#### To test the firewall’s effectiveness, ping the follwing PCs. Where the pings successful? Explain. 

###### PC-B from PC-A 
No, we do not have any zone pairs configured with a destination of INSIDE from the INTERNET zone meaning all packets are being discarded 

###### Ping PC-C from PC-A
No, we do not have any zone pairs configured with a destination of CONFROOM from the INTERNET zone meaning all packets are being discarded 

###### Ping PC-A from PC-B
Yes, we have permited TCP trafic to be sent from the INSIDE security group out to the internet. 

###### From PC-A ping R3’s G0/0/1.3 interface
Yes, because the destination of these pings are being sent to the routers self security zone which does not have a security pair confugred. 

###### From PC-C ping R3’s G0/0/1.3 interface 
Yes, because the destination of these pings are being sent to the routers self security zone which does not have a security pair confugred. Even though the CONFROOM group has been configured to explicitly forbid TCP, UDP and ICMP traffic the packets are not arriving at S0/1/1 as it is being sent directly to the router. 

## Credit 
*Create the proper zone-pair, class-maps, and policy-maps and configure R3 to prevent Internet originating traffic from reaching the Self Zone.*

```cisco
configure terminal 
class-map type inspect match-any SELF_PROTOCOLS
match protocol tcp 
match protocol udp 
match protocol icmp 
end 


configure terminal 
policy-map type inspect SELF_TO_INTERNET
class type inspect SELF_PROTOCOLS
inspect
end

configure terminal 
zone-pair security SELF_TO_INTERNET source self destination INTERNET

zone-pair security SELF_TO_INTERNET
service-policy type inspect SELF_TO_INTERNET
```
