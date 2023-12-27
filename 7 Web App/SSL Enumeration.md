## Open a connection

	openssl s_client -connect $ip:443
## Basic SSL ciphers check

	nmap --script ssl-enum-ciphers -p 443 $ip

## SSL auditing

- testssl.sh - finds BEAST, FREAK, POODLE, heart bleed, etc...

## Hearbleed Exploit

	sslscan $ip:443