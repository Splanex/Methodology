Internet Standard protocol for collecting and organizing information about managed devices on IP networks and for modifying that information to change device behavior.

## Enumerate Community strings

	./onesixtyone -c /usr/share/seclists/Discovery/SNMP/common-snmp-community-strings.txt $ip

	python snmpbrute.py -t $ip

	nmap -sU $ip -p161 --script=snmp-brute -Pn --script-args snmp-brute.communitiesdb=list.txt

	## snmp-check
	snmp-check $ip -c public

## Nmap script
	nmap -sU -p 161 --script=[script] [ip]
	nmap -sU -p 161 --script=snmp-win32-services [ip]
	nmap -sU -p 161 --script snmp-brute [ip]
	nmap -sU -p 161 --script snmp-brute [ip] --script-args snmp-brute.communitiesdb=[wordlist]
	nmap -sU -p 161 --script snmp-brute [ip] --script-args snmp-brute.communitiesdb=/usr/share/seclists/Mist/wordlist-common-snmp-community-strings.txt
	nmap -sU -p161 --script "snmp-*" $ip
	nmap -n -vv -sV -sU -Pn -p 161,162 –script=snmp-processes,snmp-netstat IP

## snmpwalk

	apt install snmp-mibs-downloader #translates MIBs into readable format

for community in public private manager; do snmpwalk -c $community -v1 $ip; done

	snmpwalk -c public -v1 $ip
	snmpenum $ip public windows.txt

Less noisy

	snmpwalk -c public -v1 $ip 1.3.6.1.4.1.77.1.2.25

Based on UDP, stateless and susceptible to UDP spoofing

	nmap -sU --open -p 16110.1.1.1-254 -oG out.txt

	snmpwalk -c public -v1 $ip # we need to know that there is a community called public
	snmpwalk -c public -v1 $ip 1.3.6.1.4.1.77.1.2.25 # enumerate windows users
	snmpwalk -c public -v1 $ip 1.3.6.1.2.1.25.4.2.1.2 # enumerates running processes

	nmap -vv -sV -sU -Pn -p 161,162 --script=snmp-netstat,snmp-processes $ip

Mine

	snmpwalk -v 2c [ip] -c [community string]
	snmpwalk -v1 -c public [ip] hrsSWInstalledName
	snmpwalk -v 2c -c public [ip] system.sysContact.0

## snmpset

	snmpset -v 2c -c public [ip] system.sysContact.0 [value type] [value]
	snmpset -v 2c -c public [ip] system.sysContact.0 s bla@top.com
## SNMPv3 enumeration

	wget https://raw.githubusercontent.com/raesene/TestingScripts/master/snmpv3enum.rb; ./snmpv3enum.rb

## Wordlist

	/usr/share/metasploit-framework/data/wordlists/snmp_default_pass.txt

## SNMP MIB Trees

- 1.3.6.1.2.1.25.1.6.0 - System Processes
- 1.3.6.1.2.1.25.4.2.1.2 - Running Programs
- 1.3.6.1.2.1.25.4.2.1.4 - Processes Path
- 1.3.6.1.2.1.25.2.3.1.4 - Storage Units
- 1.3.6.1.2.1.25.6.3.1.2 - Software Name
- 1.3.6.1.4.1.77.1.2.25 - User Accounts
- 1.3.6.1.2.1.6.13.1.3 - TCP Local Ports

## Exploitation

- Gather version numbers
- Searchsploit
- Default Creds
- Creds previously gathered
- Download the software