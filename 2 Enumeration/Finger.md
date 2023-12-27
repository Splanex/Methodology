### Find Logged in users on target

	finger @$ip

### Check User exists or not

	finger USERNAME@$ip

### finger-user-enum

	cd /tmp/

	wget http://pentestmonkey.net/tools/finger-user-enum/finger-user-enum-1.0.tar.gz

	tar -xvf finger-user-enum-1.0.tar.gz

	cd finger-user-enum-1.0

	perl finger-user-enum.pl -t 10.22.1.11 -U /tmp/rockyou-top1000.txt