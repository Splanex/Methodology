	cat words.txt | rsmangler --file - > words_new.txt

### Combine a small/semi-small dict with a custom

	cat wordlist.txt >> wordlist2.txt

### Create a custom wordlist

#### Html2dic

	curl http://example.com > example.txt

	html2dic example.txt
#### Cewl - Spider and build dictionary

	cewl -w createWordlist.txt https://www.example.com

Add minimum password length:

	cewl -w createWordlist.txt -m 6 https://www.example.com
	cewl -m [min chars] -d [depth] [url] -w [output file]

#### Crunch
	crunch [min_chars] [max_chars] -t [pattern]
	crunch 4 6 0123456789ABCDEF -o crunch1.txt #From length 4 to 6 using that alphabet
	crunch 4 4 -f /usr/share/crunch/charset.lst mixalpha # Only length 4 using charset mixalpha (inside file charset.lst)

	@ Lower case alpha characters
	
	, Upper case alpha characters
	
	% Numeric characters
	
	^ Special characters including spac
	
	crunch 6 8 -t ,@@^^%%

#### Cupp
	cupp -i