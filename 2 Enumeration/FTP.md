## Note

- Sometimes clues are put here.
- Old version of ftp might be vulnerable
- Look at the version
- Search the exploit using Google / Searchsploit / Rapid7
- If you find some credential, try it on SSH / Login page / database

## Connection

	ncftp $ip

	ftp $ip

Many ftp-servers allow anonymous users. `anonymous:anonymous`

## Nmap script enumeration

	nmap --script=ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221,tftp-enum -p 21 $ip

## Vulnerability scanning

	nmap --script=ftp-* -p 21 $ip

## Bruteforce password known username
	hydra -l $user -P /usr/share/john/password.lst ftp://$ip:21
	
	hydra -l $user -P /usr/share/wordlistsnmap.lst -f $ip ftp -V
	
	medusa -h $ip -u $user -P passwords.txt -M ftp

## Bruteforce Service Password

- Refer [[Brute-force]] note

## Enumeration of users

	ftp-user-enum.pl -U users.txt -t $ip
	
	ftp-user-enum.pl -M iu -U users.txt -t $ip

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

## Configuration Files

	ftpusers

	ftp.conf

	proftpd.conf

## Vulnerable versions

- ProFTPD-1.3.3c Backdoor
- ProFTPD 1.3.5 Mod_Copy Command Execution
- VSFTPD v2.3.4 Backdoor Command Execution
## Exploitation

- Gather version numbers
- Searchsploit
- Default Creds
- Creds previously gathered
- Download the software