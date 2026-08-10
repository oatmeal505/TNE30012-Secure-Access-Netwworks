1. Question: What is missing from the list of admin2 commands that is present in the admin1 commands?
	1. Config and debug are missing from admin2's list of commands 

2. Question: Were you able to do sh ip int br as the tech user? Explain
	1. Yes, we allowed access for the tech user to execute this command when we ran
```cisco
commands exec include show ip interface brief 
```
3. Question: Were you able run sh ip route as the tech user?
	1. No, we did not whitelist this command

4. Question: For tech view, why are the show and show ip commands listed as well as show ip interface and show ip interface brief?
	1. due to the constraints of left to right parsing, when allowing users access to: 
```cisco 
show ip interface brief
```
they will also be granted access to:
```cisco 
show 
show ip 
show ip interface
```
this is a quirk of the cisco ISO 



