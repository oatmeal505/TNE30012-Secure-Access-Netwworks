## Pass 
##### What software version is ASA running? 
7.8(1)
##### Wat is the name of the system image file and fro where was it loaded? 
asa981-lfbff-k8.spa, located at disk0 

##### The ASA can be managed using a built in GUI knows as ASDM. What version of ASDM is this ASA running? 

##### What is the Firepower extension operating system version 
2.2(1.47)
##### How much RAM does this ASA have 
4gb / 4096mb 
##### How many Ethernet ports does this ASA have? 
8 
##### What type of license does this ASA have? 
Base license 

##### What is another name for flash? 
root, boot, operating system

##### What is the name of the ASDM file in flash? 
asdm-781.bin

## Distinction 

##### was the ping addressed at R1 G0/0/1 ip address 209.165.200.225? 
Yes 
##### was the ping addressed at R1 G0/0/0 ip address 172.16.3.1?
No 
##### after adding a static route does the ping to 172.16.3.1 work? 
Yes
##### after adding the NAT network object, can you ping R1 from PC-B? 
No 
##### after implementing the class map can you now ping R1 from PC-B? 
Yes 
## Reflection 
##### How does the configuration of the ASA firewall differ from that of an ISR 
ASA Firewalls have a lot more control of how zones talk to each-other with the security level options, otherwise the interface application is largely the same. 
##### what does the ASA use to define address translation and what is the benefit? 
ASA uses NATs to define its address translation, this removes the need for us to know the internet facing addresses of traffic coming in as all addresses will be passed through the NAT. 
