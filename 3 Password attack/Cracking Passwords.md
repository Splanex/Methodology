## Cracking Passwords

### Identify hash

Use hash-identifier to determine the hash type. [https://hashkiller.co.uk](https://hashkiller.co.uk)

In kali:

	hash-identifier
	hashid

Online:

- [http://www.onlinehashcrack.com/hash-identification.php](http://www.onlinehashcrack.com/hash-identification.php)
- [https://md5hashing.net/hash_type_checker](https://md5hashing.net/hash_type_checker)

### Crack shadow using john

Paste the entire /etc/shadow file in a test file and run hashcat with the text file after john.

	john hashes.txt

	hashcat -m 500 -a 0 -o output.txt –remove hashes.txt /usr/share/wordlists/rockyou.txt

## Cracking the hash

#### Hashcat

- `-m` - mode
- `-a 0` - straight
- `-o found.txt` - where the cracked hash outputs
- `admin.hash` - the hash you want to crack.
- `/usr/share/hashcat/rules/rockyou-30000.rule` - the wordlist we use
    hashcat -m 11 -a 0 -o found.txt admin.hash /usr/share/hashcat/rules/rockyou-30000.rule
#### John the ripper

	john --wordlist=wordlist.txt dump.txt
	john --rules --wordlist=wordlist.txt dump.txt

#### Linux shadow passwd

Combine the passwd file with the shadow file using the unshadow-program.

	unshadow passwd-file.txt shadow-file.txt > unshadowed.txt
	
	john --rules --wordlist=wordlist.txt unshadowed.txt

### Crack using online tools

#### findmyhash

	findmyhash LM -h 6c3d4c343f999422aad3b435b51404ee:bcd477bfdb45435a34c6a38403ca4364

#### Cracking

- Crackstation [https://crackstation.net/](https://crackstation.net/)
- Hashkiller [https://hashkiller.co.uk/](https://hashkiller.co.uk/)
- Hashes, WPA2 captures, and archives MSOffice, ZIP, PDF [<https://www.onlinehashcrack.com/>](https://www.onlinehashcrack.com/)
- Google hashes Search pastebin.
## Others file format

#### zip

	fcrackzip -u -D -p '/usr/share/wordlists/rockyou.txt' chall.zip

	zip2john file.zip > zip.john

	john zip.john

#### 7z

	cat /usr/share/wordlists/rockyou.txt | 7za t backup.7z

	#Download and install requirements for 7z2john

	wget https://raw.githubusercontent.com/magnumripper/JohnTheRipper/bleeding-jumbo/run/7z2john.pl
	
	apt-get install libcompress-raw-lzma-perl
	
	./7z2john.pl file.7z > 7zhash.john

#### PDF

	apt-get install pdfcrack
	
	pdfcrack encrypted.pdf -w /usr/share/wordlists/rockyou.txt
	
	#pdf2john didnt worked well, john didnt know which hash type was
	
	#To permanently decrypt the pdf
	
	sudo apt-get install qpdf
	
	qpdf --password=<PASSWORD> --decrypt encrypted.pdf plaintext.pdf

### JWT

	git clone https://github.com/Sjord/jwtcrack.git

	cd jwtcrack

	#Bruteforce using crackjwt.py
	python crackjwt.py eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJkYXRhIjoie1widXNlcm5hbWVcIjpcImFkbWluXCIsXCJyb2xlXCI6XCJhZG1pblwifSJ9.8R-KVuXe66y_DXVOVgrEqZEoadjBnpZMNbLGhM8YdAc /usr/share/wordlists/rockyou.txt

	#Bruteforce using john
	python jwt2john.py eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJkYXRhIjoie1widXNlcm5hbWVcIjpcImFkbWluXCIsXCJyb2xlXCI6XCJhZG1pblwifSJ9.8R-KVuXe66y_DXVOVgrEqZEoadjBnpZMNbLGhM8YdAc > jwt.john
	john jwt.john #It does not work with Kali-John

### NTLM cracking

	Format:USUARIO:ID:HASH_LM:HASH_NT:::
	
	john --wordlist=/usr/share/wordlists/rockyou.txt --format=NT file_NTLM.hashes
	
	hashcat -a 0 -m 1000 --username file_NTLM.hashes /usr/share/wordlists/rockyou.txt --potfile-path salida_NT.pot
	hashcat -a 0 -m 5600 [hash] [wordlist]
### Keepass
	
	sudo apt-get install -y kpcli #Install keepass tools like keepass2john
	
	keepass2john file.kdbx > hash #The keepass is only using password
	
	keepass2john -k <file-password> file.kdbx > hash # The keepas is also using a file as a needed credential
	
	#The keepass can use password and/or a file as credentials, if it is using both you need to provide them to keepass2john
	
	john --wordlist=/usr/share/wordlists/rockyou.txt hash

### Lucks image

Method 1

Install: [https://github.com/glv2/bruteforce-luks​](https://github.com/glv2/bruteforce-luks%E2%80%8B)
	
	bruteforce-luks -f ./list.txt ./backup.img
	
	cryptsetup luksOpen backup.img mylucksopen
	
	ls /dev/mapper/ #You should find here the image mylucksopen
	
	mount /dev/mapper/mylucksopen /mnt

Method 2

cryptsetup luksDump backup.img: Check that the payload offset is set to 4096
	
	dd if=backup.img of=luckshash bs=512 count=4097 #Payload offset +1
	
	hashcat -m 14600 luckshash
	
	cryptsetup luksOpen backup.img mylucksopen
	
	ls /dev/mapper/ #You should find here the image mylucksopen
	
	mount /dev/mapper/mylucksopen /mnt