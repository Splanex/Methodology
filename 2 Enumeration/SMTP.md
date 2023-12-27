
	nc [ip] [port] (connect to smpt)
		- help
		- RCPT TO: [user]@[domain].com
		- EXPN [username]
		- VRFY [username]

## Msfconsole
	use auxiliary/scanner/smtp/smtp_enum
### User enumeration

	for server in $(cat smtpmachines); do echo "******************" $server "*****************"; smtp-user-enum -M VRFY -U userlist.txt -t $server;done #for multiple servers

#### smtp-user-enum
	smtp-user-enum
	smtp-user-enum -M VRFY -U /usr/share/wordlists/metasploit/unix_users.txt -t $ip
	smtp-user-enum -M VRFY -U /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames-dup.txt -t $ip

### Command to check if a user exists

	nc [ip] [port] (connect to smpt)
		- help
		- RCPT TO: [user]@[domain].com
		- EXPN [username]
		- VRFY [username]

### Enumeration and vuln scanning
	nmap --script smtp-commands [IP]
	nmap --script smtp-enum-users [IP]
	nmap --script=smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 -p 25 $ip

### Brute-force

	hydra -P /usr/share/wordlistsnmap.lst $ip smtp -V

### Connection
	nc [ip] [port] 
	telnet $ip 25