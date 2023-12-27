	nmap -sC -sV -oA [path] $ip
	nmap -sC -sV -O -p- -oA nmap/full $ip
	nmap -sU -O -p- -oA nmap/udp $ip
	nmap -A $ip
	nmap -sn $ip/24
	nmap -vvv -sn $ip/24
	nmap -sn -n $ip/24 > ip-range.txt
	nmap -sP 10.0.0.0-10
	cat ip-range.txt | grep -B 1 "Host is up"
	grep -o '[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}' ip-range.txt > only-ip.txt
	nmap www.testhostname.com
	nmap -A -oA filename $ip/24
	nmap -F $ip
	nmap -sS -D [decoy1],[decoy2],[decoy3] [options] [target ip]
	nmap -sS -T[1-5] [options] [target ip]
	nmap -sS -g [source port] [options] [ip]
	nmap -sS --data-length [length in bytes] [options] [ip]
	nmap -sS --spoof-mac [mac] [options] [ip]
	nmap -sS -iL [host list] [options]
	nmap -sS -iL [host list] [options] --randomize-hosts

