## Find Scripts

Find script related to a service you're interested in, example here is ftp

	locate .nse | grep [service]

Example:

	locate .nse | grep ftp
	
	locate nse | grep script

Typically NSE scripts that scan for vulnerabilities are at

	ls -l /usr/share/nmap/scripts/

## Help manual for scripts

What does a script do?

	nmap --script-help [script name]

Example:

	nmap --script-help "smb*" and discovery
	nmap --script-help ftp-anon

##  Vulnerability Scanning

We can scan for vulnerability Scanning nmap scripts:

	nmap --script vuln [ip target]
	nmap -p 80 --script=all [ip target]
	# Scan a target using all NSE scripts. May take an hour to complete.

	nmap -p 80 --script=*vuln* [ip target]
	# Scan a target using all NSE vuln scripts.

	nmap -p 80 --script=http*vuln* [ip target]
	# Scan a target using all HTTP vulns NSE scripts.

	nmap -p 21 --script=ftp-anon [ip target]/24
	# Scan entire network for FTP servers that allow anonymous access.

	nmap -p 80 --script=http-vuln-cve2010-2861 [ip target]/24
	# Scan entire network for a directory traversal vulnerability. It can even retrieve admin's password hash.