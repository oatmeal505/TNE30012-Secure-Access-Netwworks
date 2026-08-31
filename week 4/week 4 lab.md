## Pass
1. What is the main reason for having synchronized time in a network?
	1. To ensure that network events have consistent time stamps, necessary in putting together a time line during major incident reports 
2. Is the key number relevant or can I use any number I like? 
	1. The key number is global, so it must match the same number on other NTP clients or peers. in theory yes you can use any number that is within the specified range but it should be the same as other devices on your network. 
## Credit 
3. What is the problem with setting the level of severity too high or too low? 
	1. If you set the alert level too high you will got notified of some events that are not network critical, this could lead to network admins missing more important alerts. in contrast if the alert level is to low you might miss alerts that explain the root cause of a network outage or are critical in incident review after an event has taken place. 
4. If the command logging trap critical were issued, which severity levels of messages would be logged?
	1. 0,1,2 this is because the number that syslog is set too will include all previous numbers counting down to zero 
5. At what level is console logging enabled?
	1. 7, Debugging
6. At what level is trap logging enabled?
	1. 4, Warnings 
7. What is the IP address of the syslog server?
	1. 192.168.1.3, this is the IP address for PC-A where we are running the syslog server
8. What port is syslog using?
	1. Port 514 via UDP