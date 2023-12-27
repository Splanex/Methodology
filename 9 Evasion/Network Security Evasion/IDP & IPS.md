Evading a signature-based IDS/IPS requires that you manipulate your traffic so that it does not match any IDS/IPS signatures. Here are four general approaches you might consider to evade IDS/IPS systems.

1. Evasion via Protocol Manipulation
2. Evasion via Payload Manipulation
3. Evasion via Route Manipulation
4. Evasion via Tactical Denial of Service (DoS)

## Protocol Manipulation
Protocol manipulation includes:

- Relying on a different protocol
- Manipulating (Source) TCP/UDP port
	- nmap -sS -Pn -g 80 -F 10.10.14.181
	- nmap -sU -Pn -g 53 -F 10.10.14.181
	- ncat -lvnp 80
	- ncat -ulvnp 53
- Using session splicing (IP packet fragmentation)
	- nmap -f
	- nmap -ff
	- nmap --mtu [size]
- Sending invalid packets
	- nmap --backsum
	- nmap --scanflags

## Payload Manipulation
Payload manipulation includes:

- Obfuscating and encoding the payload
	- Base64
	- URL encoding
	- Unicode escape sequence
- Encrypting the communication channel
	-  socat ([[Encrypted Communication]])
- Modifying the shellcode
	- Changing the order of the flags
	- Add an extra white space
	- use a different command such as `nc` or `socat`

## Route Manipulation
Route manipulation includes:

- Relying on source routing
	- nmap --ip-options "L 10.10.10.50 10.10.50.250"
	- nmap --ip-options "S 10.10.10.50 10.10.50.100 10.10.50.250"
- Using proxy servers
	- nmap -sS HTTP://PROXY_HOST1:8080,SOCKS4://PROXY_HOST2:4153 10.10.14.181

## Tactical DoS
Evasion via tactical DoS includes:

- Launching denial of service against the IDS/IPS
- Launching denial of Service against the logging server

