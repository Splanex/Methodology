## System
### Meterpreter
	ps
	sysinfo
	getuid
	post/windows/gather/
	post/windows/gather/credentials/
	post/windows/gather/enum_services
	post/windows/gather/enum_applications
	post/windows/gather/enum_shares
	post/windows/gather/enum_domain	
	run winenum
	run scraper
	
### Windows
	ps
	net start
	net user
	net user /domain
	net localgroup
	net view /domain
	net group "Domain Controllers" /domain
	wminc service get Caption,StartName,State,pathname
### Linux
	ps
	service --status-all
	cat /etc/passwd
## Network
### Meterpreter
	ifconfig
	netstat
	arp
##### Modules
	search arp_scanner
	search ping_sweep
	search portscan
### Windows
	ipconfig
	arp
	route print
	netstat
### Linux
	ifconfig
	arp
	route -v
	netstat