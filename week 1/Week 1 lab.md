1. Question: How does configuring an enable secret password help to protect a router from being compromised by an attack?
	1. configuring an enable secret password prevents users without the password from elevating their access further into enable or configuration mode, this can prevent malicious actors from obtaining or destroying information or inexperienced users accidentally causing damage. 

2. Question: When you configured the password for the console line, what message was displayed?
	1. Invalid password length, must contain 10-127 characters. Password configuration failed 

3. Questions:
	1. Were you able to login? Explain.
		1. No, we have not set the VTY password yet
	2. What messages were displayed?
		1. password required, but none set

4. Question: Were you able to login this time?
	1. Yes, after setting the line vty password we can now login

5. Question: 
	1. Can you read the enable secret password? Explain.
		1. No, the password is currently hashed 
	2. Can you read the console, aux, and vty passwords? Explain. 
		1. Yes, when the enable password was set we clarified that we wanted to encrypt it and set the encryption type. we did not do that for the other passwords

6. Questions:
	1. Can you read the console, aux, and vty passwords? Explain.
		1. No, the passwords have no been encrypted 
	2. At what level (number) is the default enable secret password encrypted?
		1. type 9. 
	3. At what level (number) are the other passwords encrypted?
		1. type 7. 
	4. Which level of encryption is harder to crack. Explain.
		1. type 9, type 7's purpose is to prevent viewing of passwords via the show run command and is easily broken. type 9 is a more complex hashing algorithm that is more computationally expensive to crack and encrypt 

7. Question: What does the $ convert to in the output?
	1. a ^c or a Control + C input

8. Question: What options are available
	1. MD5, Scrypt and sha256

9. Question: Which hashing method is used for the password?
	1. Scrypt 

10. Question: What is the difference between logging in at the console now and previously?
	1. you are now asked for a username and password

11. Question: Were you able to issue the command? Explain.
	1. No, as we are not in exec mode 

12. Question: Were you prompted for a password? Explain. 
	1. yes we are asked for the enable password

13. Question: What was the router prompt after you entered the password?
	1. the router was already in enable mode after logging in as an admin 

14. Question: What users are connected to router R1 at this time?
	1. Admin via the console, and admin via SSH

Reflection
1. Explain the importance of securing router access and monitoring network devices.
	1. 
Type your answers here.
2. What advantages does SSH have over Telnet?
	1. 