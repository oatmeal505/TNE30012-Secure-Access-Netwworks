## Pass
###### What are the two ends of the VPN tunnel? Identify the physical entities that represent the ends. 
- they are our routers internet facing serial addresses. 
###### What is the function of the IPsec transform set? 
- The transform set allows both end devices to aggree upon a particular data flow, our data set agrees upon ESP which dictates the payload authentication. transformation sets can include multiple data flows 
###### Does IPsec evaluate whether the access lists are mirrored as a requirement to negotiate its security association?
- No, they do not need to match. 
###### Why haven’t any SAs been negotiated? 
- No traffic has been sent from either network and has not been caught by the ACL 
###### Was an SA created for these pings? Explain. 
- Yes, as interesting traffic was captured that is classifed by anything from our network on S1's side to or from S3's network 
###### Was an SA created between R1 and R3 after issueing debug ospf hello? Explain. 
- OSPF hello's are not part of our interesting traffic, if they where mapped to our ACL they would be sent via the tunnel but as of now no. 
###### Why was an SA created between R1 and R3 this time? 
- because traffic that matched our ACL was sent and passed through the tunnel 
###### What are the endpoints of the IPsec VPN tunnel? 
- R1 S0/1/0- 10.1.1.1 | R3 S0/1/1-10.2.2.1
###### How many packets have been transformed between R1 and R3?
- 4 packets 
###### What other types of traffic would result in an SA forming and tunnel establishment? 
- currently any traffic with a source and destination specified by our ACL, this could be further refined if we teaked our ACL.
###### How many packets have been encrypted? Was this interesting traffic? 
- 0, No as this did not have the destination address listed in the ACL 
#### Reflection 
###### Would traffic on the Gigabit Ethernet link between PC-A and the R1 G0/0 interface be encrypted by the site-to-site IPsec VPN tunnel? Explain.
- No, as the traffic will not be captured by the ACL and the tunnel is not running on the switch. traffic will only be encrypted when captured by the ACL running on R1 

## Credit 
###### Ping the new loopback and source your traffic from loopback 5 on R3. Does it work?
- Yes 
###### Did it go through the VPN tunnel?
- Yes, a new entry has apeared in show crypto ipsec sa that is for our desination network of 60.60.600 and the packets have been encrypted 

###### Ping the 2nd new loopback and source your traffic from loopback 5 on R3. Does it work? 
- yes 
###### Did it go through the VPN tunnel?
- No, the traffic does not apear in show crypto ipsec for our new loopback