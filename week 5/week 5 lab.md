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
2. Log in to the console as Admin01 with a password of Admin01pass. Were you able to log in?
3. Attempt to log in to the console as baduser with any password. Were you able to log in? 
4. If no user accounts are configured in the local database, which users are permitted to access the device?
5. Verify that this authentication profile is used by opening an SSH session from PC-C to R3. Log in as Admin01 with a password of Admin01pass. Were you able to login?
6. Attempt to log in as baduser with any password. Were you able to login?
7. Exit the SSH session to the router R3. Then try to open an SSH session to the router again, but this time try to log in with the username Admin01 and a bad password. From the console window, the debug output should look similar to the following. What message was displayed on the SSH client screen? 
### Reflection 
1. Why would an organization want to use a centralized authentication server rather than configuring users and passwords on each individual router? 
2. Contrast local authentication and local authentication with AAA. 