## Directory Discovery
### Dirsearch
	dirsearch -u [host] -w [wordlist]
	dirsearch -u [host] -w [wordlist] -e [extentions]
### Gobuster
	
#### Gobuster quick directory busting

	gobuster -u $ip -w /usr/share/seclists/Discovery/Web_Content/common.txt -t 80 -a Linux

#### Gobuster directory busting

	wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/Top1000-RobotsDisallowed.txt; gobuster -u http://$ip -w Top1000-RobotsDisallowed.txt

	gobuster dir -u http://$ip -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x php -o gobuster-root -t 50

#### Gobuster comprehensive directory busting

	gobuster -s 200,204,301,302,307,403 -u $ip -w /usr/share/seclists/Discovery/Web_Content/big.txt -t 80 -a 'Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0'

#### Gobuster search with file extension

	gobuster dir -u [url] -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x txt,php,zip,bak,sql,sqlite,tar,tgz,tar.gz
	gobuster -u $ip -w /usr/share/seclists/Discovery/Web_Content/common.txt -t 80 -a Linux -x .txt,.php

### wfuzz

	wfuzz -c -z file,/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --sc 200 http://$ip/FUZZ
	wfuzz -c --hw 977 -u [host]/FUZZ -w [wordlist] 
### Fuff
	fuff: ffuf -u http://10.10.218.33/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories-lowercase.txt -c -v
### Erodir by PinkP4nther

	./erodir -u http:/$ip -e /usr/share/wordlists/dirb/common.txt -t 20

### If really stuck, run this

	for file in $(ls /usr/share/seclists/Discovery/Web-Content); do gobuster -u http://$ip/ -w /usr/share/seclists/Discovery/Web-Content/$file -e -k -l -s "200,204,301,302,307" -t 20 ; done

### Other tools

- dirb
- dirbuster

### Backup files extension list	
	bak, _bak, bac, old, 000, 01, 001, ~, inc, Xxx

### Set extension
	sh,txt,php,html,htm,asp,aspx,js,xml,log,json,jpg,jpeg,png,gif,doc,pdf,mpg,mp3,zip,tar.gz,tar


## Subdomain Discovery
### Sublist3r
	sublist3r -d [domain] -t [threads] -e [search engine]

### wfuzz
	wfuzz -c --hw [halfword] -f sub-fighter -w [wordlist] -u [host] -H "Host: FUZZ.[host]"
	wfuzz -c --hw [halfword] -f sub-fighter -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u [host] -H "Host: FUZZ.[host]"
	wfuzz -c -w /seclist/Discovery/DNS/subdomains-top1million-110000.txt -H “Host: FUZZ.[host]” -u [host]
### ffuf
	 ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u [host] -H "Host: FUZZ.[host]" -fs 20637 -t 100
	 