1. Change the hostnames of the router and switch to the appropriate names (R1, SW1)
     ##Use the 'hostname' command in global configuration mode##
     
	   進入global configuration模式後
	   下指令hostname R1

2.  Configure an unencrypted enable password of 'CCNA' on both devices

	    進入global configuration模式後
	    enable password CCNA


3. Exit back to user EXEC mode and test the password

	   輸入指令exit離開global configuration模式，再次輸入exit指令離開priviledge EXEC mode

4.  View the password in the running configuration

	    在global configuration
	    do sh running-config

		在priviledge模式
		sh running-config
		可以直接sh run


5. Ensure that the current password, and all future passwords, are encrypted

	   進入global configuration模式後
	   service password-encryption


6. View the password in the running configuration

	   在global configuration
	   do sh running-config
	    
	   在priviledge模式
	   sh running-config



7. Configure a more secure, encrypted enable password of 'Cisco' on both devices

	   在global configuration
	   enable secret Cisco

8. Exit back to user EXEC mode and then return to privileged EXEC mode.
    Which password do you have to use?
    
	   should use 'Cisco'


9. View the passwords in the running configuration.
	What encryption type number is used for the encrypted 'enable password'?`7`
	What encryption type number is used for the encrypted 'enable secret'?`5`

11. Save the running configuration to the startup configuration

		在global configuration
		show startup-config
		或sh start

