	 runas.exe /netonly /user:<domain>\<username> cmd.exe

Configure  DNS
```powershell
$dnsip = "<DC IP>"
$index = Get-NetAdapter -Name '<Interface name>' | Select-Object -ExpandProperty 'ifIndex'
Set-DnsClientServerAddress -InterfaceIndex $index -ServerAddresses $dnsip
```

List the SYSVOL directory:

	dir \\<FQDN>\SYSVOL	
