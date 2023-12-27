**Lightweight Directory Access Protocol**
## Enumeration

	ldapsearch -h $ip -p 389 -x -b "dc=mywebsite,dc=com"

## Email addresses enumeration

Find emails in google, bing, pgp etc

	theharvester -d $ip -b google

Contact information for the domains they host

	whois $ip