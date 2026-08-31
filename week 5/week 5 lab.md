## Pass 
1. Can you read the console, aux, and vty passwords? 
	1. Yes, we have not configured the ```service password-encryption``` command which enables all passwords stored in the running config to be encrypted
2. Can you read the user’s password?
	1. No, as the command that we used to configure the users password specified an encryption method to use, in this case sha256 
3. Log in using the user01 account and password previously defined. What is the difference between logging in at the console now and previously?
	1. when logging in to the console you are now asked for a username and password instead of entering 
4. After logging in, issue the show run command. Were you able to issue the command?
	1. no as we are not in privileged exec mode 
5. Enter privileged EXEC mode using the enable command. Were you prompted for a password?
	1. Yes, it was the enable password we configured earlier in the lab 
6. Enable SSH on the vty lines, From PC-A, establish an SSH session with R1. Were you prompted for a user account? Explain.
	1. Yes, since we configured SSH via the VTY line and specified local login it initiate the same process that we see when logging into the line console 
7. While connected to R1 via SSH, access privileged EXEC mode with the enable command. What password did you use? 
	1. Yes, but as with like the line console password it required the password we configured at the start of the lab for entering enable mode 
## Credit 
1. Create a local user account with PDKDF2 hashing to encrypt the password. Can you read the user’s password in the running configuration? 
	1. No, same reason as before encryption is specified on account creation
2. Log in to the console as Admin01 with a password of Admin01pass. Were you able to log in?
	1. Yes, as we have setup a default login authentication list to not use ANY authentication by adding none to our default login command 
3. Attempt to log in to the console as baduser with any password. Were you able to log in?
	1. Yes, the same reason as above, AAA is operating without authentication currently 
4. If no user accounts are configured in the local database, which users are permitted to access the device?
	1. Currently any user can access the device as AAA is not using the local database, with no users in the database it would still be any user. note that users still can not enter privileged exec mode or configuration mode as that is still protected by our enable password configured at the start of the lab 
5. Verify that this authentication profile is used by opening an SSH session from PC-C to R3. Log in as Admin01 with a password of Admin01pass. Were you able to login?
	1. Yes, the prior command that we executed created a new access list that has been attached to the line input for SSH. this list uses the routes local account DB, we can see this in the privileged level that we are allocated while connected via SSH our user is already in privileged exec mode lining up with the access we configured earlier.  
6. Attempt to log in as baduser with any password. Were you able to login?
	1. No, as baduser does not exist in R3s local login database we cannot connect via SSH. 
7. Exit the SSH session to the router R3. Then try to open an SSH session to the router again, but this time try to log in with the username Admin01 and a bad password. From the console window, the debug output should look similar to the following. What message was displayed on the SSH client screen? 
	1. When attempting to login 2 times it will display nothing and prompt the user for another password attempt, if you persist for a 3rd time it will close the SSH session with the following error message:
```cisco 
[Connection to 10.2.2.1 closed by foreign host]
```
### Reflection 
1. Why would an organization want to use a centralized authentication server rather than configuring users and passwords on each individual router? 
	1. Configuring credentials centrally allows for better security oversight as they only need to secure 1 point. it is also less effort rather than repeatedly configuring credentials on each device, AAA also removes the risk of human error when creating duplicate credentials over and over again which leads to access issues when an error occurs. 
2. Contrast local authentication and local authentication with AAA. 
	1. local authentication operates on a micro level, this is evident through the creation of individual user accounts and configuring their access. AAA serves to assist with the macro scale of managing multiple devices and users across a network, AAA preforms this job well however, as is the cost with all of these types of tools you sacrifice the granular ability to configure individual user accounts in specific use cases with the ease of being able to manage 100s to 1000s of user accounts at once.
	2. AAA also operates as a single point of failure if backup servers are not in-place where as local login has no requirement for the network itself to be working in order to access devices via any entry method. 