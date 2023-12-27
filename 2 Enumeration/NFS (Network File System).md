	/etc/exports

## Nmap
	nmap --script-help "nfs*"
	nmap --script nfs-ls,nfs-showmount,nfs-statfs [IP]
	nmap -p 111 --script nfs* [IP]
## Show all mounts

	showmount -e $ip

## Mount a NFS share
	
	mkdir -p /mnt/home/bob
	mount -t $ip:/home/bob /mnt/home/bob -o nolock
	mount -t nfs $ip:/mnt/backups /mnt/home/bob

	mount #check if mount was successfull

	umount /mnt/home/bob

## Use nfspy to mount a share.
Will get around permission errors

	nfspysh -o server=$ip:/home/vulnix/
## Note

- Take a look of suid if permission denied
- Create new user and change the suid of user to the correct one

		sudo sed -i -e 's/1001/1014/g' /etc/passwd