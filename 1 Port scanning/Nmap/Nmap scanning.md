
## Initial scan TCP

	nmap -sC -sV -oA [path] $ip

## Full scan TCP

Comprehensive nmap scans in the background to make sure we cover all bases.

	nmap -sC -sV -O -p- -oA nmap/full $ip

## Full scan UDP

	nmap -sU -O -p- -oA nmap/udp $ip

## Normal Scan

	nmap -A $ip

## Scan for alive hosts

	nmap -sn $ip/24
	
	nmap -vvv -sn $ip/24

If you want a little faster,

	nmap -sn -n $ip/24 > ip-range.txt

## Scan specific IP range

	nmap -sP 10.0.0.0-100

## Sort out the machines that are up

	cat ip-range.txt | grep -B 1 "Host is up"

and now filter all the IPs and create a file.

	grep -o '[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}' ip-range.txt > only-ip.txt

## Scan a host

	nmap www.testhostname.com

## Scan specific machine

### Scan common port

	nmap -A -oA filename $ip/24

The command:

- Scan 1024 most common ports

- Run OS detection

- Run default nmap scripts

- Save the result into `.nmap`, `.gnmap` and `.xml`

- Faster

## Fast scanning

Scan 100 most common ports

	nmap -F $ip

## Port knock

	for x in 7000 8000 9000; do nmap -Pn --host_timeout 201 --max-retries 0 -p $x $ip; done

## Scan deeply

Scanning more deeply:

	nmap -v -p- -sT $ip

Example:

	nmap -v -p- -sT 10.0.1.0/24
## Scan for specific port

	nmap -p T:80,443,8080 $ip/24

Use `-T`: specifies TCP ports. Use `-U`: for UDP ports.

## Scan for unused IP addresses and store in text file

	nmap -v -sn $ip/24 | grep down | awk '{print $5}' > filename.txt

#### Scan targets from a text file

	nmap -iL list-of-ips.txt

## Firewall/IDS Evasion

	nmap -sS -D [decoy1],[decoy2],[decoy3] [options] [target ip]
	nmap -sS -T[1-5] [options] [target ip]
	nmap -sS -g [source port] [options] [ip]
	nmap -sS --data-length [length in bytes] [options] [ip]
	nmap -sS --spoof-mac [mac] [options] [ip]
	nmap -sS -iL [host list] [options]
	nmap -sS -iL [host list] [options] --randomize-hosts

## IDLE/Zombie scan

	nmap -O -v [target ip]
	nmap -Pn -sI [zombie ip]:[zombie port] [target ip] -p [port] -v