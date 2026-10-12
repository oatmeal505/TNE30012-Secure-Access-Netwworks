## Pass 
###### From the output of the show interfaces trunk command in the previous step, what is the current native VLAN for the S1 F0/5 trunk interface?
- the native vlan was vlan 1 
###### The following message should display after a brief period of time:
```cisco 
02:16:28: %CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on
FastEthernet0/5 (99), with S2 FastEthernet0/5 (1).
```
What does the message mean?
- The native VLAN does not match at both ends on the trunk, this has happened because S2 is still using native vlan 1 where as S1 is using VLAN99
###### What appened to the message? 
- setting S2's native vlan to VLAN99
###### What is the MAC address of the R1 G0/0/1 interface? 
- 706e.6d2a.9eal
###### On S1, issue the show port-security 
- What is the Security Violation Count? 
	- 1
- What is the status of the F0/5 port? 
	- Secure-up
- What is the Last Source Address and VLAN?
	- 0000.0000.0000:0
###### From the R1 CLI, ping PC-A. Was the ping successful? Explain. 
- No, the port on S1 has been put into err disabled state as the mac address specified is not present. 
###### From R1, try to ping the PC-A again at 192.168.1.10. Was the ping successful? Explain. 
- No, the port is still in the err disabled state and needs to be manualy enabled. 

## Credit 
###### Which command is used to verify that a port is protected? 
```cisco
show interfaces [interface-id] switchport
```
###### Which command is used to de-activate the PVLAN Edge feature? 
```cisco 
int [interface-id]
no switchport protected 
```