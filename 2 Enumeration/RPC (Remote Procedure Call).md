
## Enumerate, shows if any NFS mount is exposed:

	rpcinfo -p $ip
	nmap --script rpc-grind, rpcinfo $ip
	nmap --script=msrpc-enum $ip
	nmap -n -v -sV -Pn $ip --script=msrpc-enum

	impacket-rpcdump $ip
	impacket-rpcdump $ip | egrep 'MS-RPRN|MS-PAR'
## Connection

Connect to an RPC share without a username and password and enumerate privledges

	rpcclient --user="" --command=enumprivs -N $ip

Connect to an RPC share with a username and enumerate privledges

	rpcclient --user="<Username>" --command=enumprivs $ip

## Commands

	rpcclient>srvinfo

	rpcclient>enumdomusers

	rpcclient>getdompwinfo