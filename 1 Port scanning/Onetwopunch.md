The idea behind the script is to generate a scan of 65,535 ports on the targets. The script uses unicornscan to scan all ports, and makes a list of the ports that are open. The script then takes the open ports and passes them to nmap for service detection.
## Onetwopunch.sh

Grab the latest bash script

	git clone https://github.com/superkojiman/onetwopunch.git
	cd onetwopunch

Create a text file that contains the targets machines:

	192.168.1.144
	
	192.168.1.179
	
	192.168.1.182

Then, run the script and tell it to read our txt file and perform TCP scan against each target.

	./onetwopunch.sh -t ip-range.txt -p tcp
