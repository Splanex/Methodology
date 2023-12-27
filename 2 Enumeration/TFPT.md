## Command

	send # Send single file
	
	put # Send one file.
	
	mput # Send multiple files.
	
	mget # Get multiple files.
	
	get # Get file from the remote computer.
	
	ls # list
	
	mget * # Download everything
	
	binary = Switches to binary transfer mode.
	
	ascii = Switch to ASCII transfer mode

## Nmap script

	nmap -sU -p 69 --script tftp-enum.nse $ip

## Vulnerable version

	Vuln tftp server 1.3, 1.4, 1.9, 2.1, and a few more

## Exploitation

- Gather version numbers
- Searchsploit
- Default Creds
- Creds previously gathered
- Download the software