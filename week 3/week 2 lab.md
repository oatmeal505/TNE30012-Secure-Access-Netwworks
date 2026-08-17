## Pass Task
1. What security-related configuration changes were performed on R3 by AutoSecure that were not performed in previous sections of the lab on R1?
	1. 
2. What security-related configuration changes were performed in previous sections of the lab that were not performed by AutoSecure?
	1. 
3. Identify at least five unneeded services that were locked down by AutoSecure and at least three security measures applied to each interface. 
	1. Services disabled include:
		1. 
	2. For each interface, the following were disabled:
		1. 
4. What are some advantages to using AutoSecure?
	1. 

## Credit task
1. Ping PC-C using the serial address of R1 as the source IP. Did it work? Explain why it did or did not work!
	1. No, Router 2's Serial interfaces have not been setup with OSPF RPA and will not carry traffic for router 1. 
## Distinction task
1. In the process issue the same test as with OSPF. Enable authentication on R1 and then try and ping PC-C. Did it
	1. No, like with OSPF R1 will refuse to communicate with its neighbors without the correct authentication protocol and a matching key. 
work? Explain
2. Besides “sh run” which show command tells me that authentication has been enabled in EIGRP?

Besides show run you can also use the following command to confirm if EIGRP authentication has been enabled 
```cisco 
debug eigrp packets
``` 
if it is the debug messages will read.	   
```cisco 
*Aug 16 23:37:16.616: EIGRP: Sending HELLO on Se0/1/0 - paklen 60
*Aug 16 23:37:16.616:   AS 1, Flags 0x0:(NULL), Seq 0/0 interfaceQ 0/0 iidbQ un/rely 0/0
*Aug 16 23:37:17.472: EIGRP: received packet with MD5 authentication, key id = 1
*Aug 16 23:37:17.472: EIGRP: Received HELLO on Se0/1/0 - paklen 60 nbr 10.1.1.2
*Aug 16 23:37:17.472:   AS 1, Flags 0x0:(NULL), Seq 0/0 interfaceQ 0/0 iidbQ un/rely 0/0 peerQ un/rely 0/0 
```

 ### EIGRP evidence

```cisco 
### R1 ###

R1#sh ip eigrp nei
EIGRP-IPv4 Neighbors for AS(1)
H   Address                 Interface              Hold Uptime   SRTT   RTO  Q  Seq
                                                   (sec)         (ms)       Cnt Num
0   10.1.1.2                Se0/1/0                  12 00:10:29    2   100  0  17

R1#sh ip eigrp interfaces
EIGRP-IPv4 Interfaces for AS(1)
                              Xmit Queue   PeerQ        Mean   Pacing Time   Multicast    Pending
Interface              Peers  Un/Reliable  Un/Reliable  SRTT   Un/Reliable   Flow Timer   Routes
Se0/1/0                  1        0/0       0/0           2       0/12          50           0

### R2 ### 

R2#sh ip eigrp nei
EIGRP-IPv4 Neighbors for AS(1)
H   Address                 Interface              Hold Uptime   SRTT   RTO  Q  Seq
                                                   (sec)         (ms)       Cnt Num
1   10.2.2.1                Se0/1/1                  14 00:10:41    3   100  0  11
0   10.1.1.1                Se0/1/0                  12 00:11:43    1   100  0  8

R2#sh ip eigrp interfaces
EIGRP-IPv4 Interfaces for AS(1)
                              Xmit Queue   PeerQ        Mean   Pacing Time   Multicast    Pending
Interface              Peers  Un/Reliable  Un/Reliable  SRTT   Un/Reliable   Flow Timer   Routes
Se0/1/0                  1        0/0       0/0           1       0/12          50           0
Se0/1/1                  1        0/0       0/0           3       0/12          50           0

### R3 ###

R3#sh ip eigrp neighbors
EIGRP-IPv4 Neighbors for AS(1)
H   Address                 Interface              Hold Uptime   SRTT   RTO  Q  Seq
                                                   (sec)         (ms)       Cnt Num
0   10.2.2.2                Se0/1/1                  14 00:11:15    2   100  0  16

R3#sh ip eigrp interfaces
EIGRP-IPv4 Interfaces for AS(1)
                              Xmit Queue   PeerQ        Mean   Pacing Time   Multicast    Pending
Interface              Peers  Un/Reliable  Un/Reliable  SRTT   Un/Reliable   Flow Timer   Routes
Se0/1/1                  1        0/0       0/0           2       0/12          50           0
```