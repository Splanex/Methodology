
## Dnsenum
	dnsenum [domain]
	dnsenum [domain] --dnsserver [domain dns]
	dnsenum --subfile [file to save to] -v -f /usr/share/dnsenum/dns.txt -u a -r [domain]
## Fierce – Domain DNS scannerer

	fierce -dns $domain
	fierce -dns [domain] --dnsserver [domain dns]

## Dnsmap
	dnsmap [domain] (get subdomains)

## Dnsrecon
	
	dnsrecon -d [domain]
	dnsrecon -d [domain] -r [range]

## Nmap command

	nmap -sC -sV -p53 $ip/24

## Find the IP and authoritative servers.

	nslookup $ip

## Dig deeper

	dig axfr $ip @10.10.10.13
	dig axfr $ip @10.10.130.160

## Find name servers

	host -t ns $ip

## Find txt records

	host -t txt $ip

## Find email servers

	host -t mx $ip

## Subdomain bruteforcing using common hostname
	
	for ip in $(cat list.txt); do host $ip.website.com; done

## Reverse dns lookup bruteforcing

	for ip in $(seq 155 190);do host 50.7.67.$ip;done |grep -v "not found"

The ip is based on subdomain bruteforcing result

## Zone transfer request

	host -l $ip ns1.$ip
	
	host -l $ip ns2.$ip

Bash script for zone transfer:

	#!/bin/bash
	
	# Simple Zone Transfer Bash Script
	
	# $1 is the first argument given after the bash script
	
	# Check if argument was given, if not, print usage
	
	if [ -z "$1" ]; then
	
	echo "[*] Simple Zone transfer script"
	
	echo "[*] Usage : $0 <domain name> "
	
	exit 0
	
	fi
	
	# if argument was given, identify the DNS servers for the domain
	
	for server in $(host -t ns $1 | cut -d " " -f4); do
	
	# For each of these servers, attempt a zone transfer
	
	host -l $1 $server |grep "has address"
	
	done

## DNS enumeration script

	dnsrecon -d $ip -t axfr

### Bruteforce using wordlist

	dnsrecon -d $ip -D ~/list.txt -t brt

## Finds nameservers for a given domain

	host -t ns $ip| cut -d " " -f 4

	dnsenum $ip

## Nmap zone transfer scan

	nmap $ip --script=dns-zone-transfer -p 53

## Finds the domain names for a host.

	whois $ip

## Finds miss configured DNS entries.

	host -t ns $ip

## TheHarvester finds subdomains in google, bing, etc

	python theHarvester.py -l 500 -b all -d $ip

## Find DNS (A) records by trying a list of common sub-domains from a wordlist.

	nmap -p 80 --script dns-brute.nse domain.com

	python dnscan.py -d domain.com -w ./subdomains-10000.txt

## Exploitation

- Gather version numbers
- Searchsploit
- Default Creds
- Creds previously gathered
- Download the software