1. Evasion via controlling the source MAC/IP/Port
2. Evasion via fragmentation, MTU, and data length
3. Evasion through modifying header fields
4. Port hopping
5. Port tunneling
6. Use of non-standard ports


## Evasion via Controlling the Source MAC/IP/Port
- Decoy(s) 
	- nmap -sS -Pn -D 10.10.10.1,10.10.10.2,ME
	- nmap -sS -Pn -D RND, RND, ME
- Proxy
	- nmap -sS -Pn --proxies PROXY_URL 
- Spoofed MAC address (Only if in the same network segment)
- Spoofed IP adress (Only if in the same network segment)
	- nmap -S IP_ADDRESS
- Use a specific source port number
	- nmap -g PORT_NUM 
	- nmap --source-port PORT_NUM
## Evasion via fragmentation, MTU, and data length
- Fragment packets, optionally with given MTU.
	- nmap -f
	- nmap -ff
	- nmap --mtu [multiple of 8]
- Send packets with specific data lengths.
	- nmap --data-length [multiple of 8]
## Evasion through modifying header fields
- Set IP time-to-live
	- nmap --ttl
- Send packets with specified IP options
	- nmap --ip-options HEX_STRING
	- nmap --badsum
- Send packets with a wrong TCP/UDP checksum

