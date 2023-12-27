```
Sharphound.exe --CollectionMethods <Methods> --Domain za.tryhackme.com --ExcludeDCs
SharpHound.exe --CollectionMethods All --Domain za.tryhackme.com --ExcludeDCs
SharpHound.exe --CollectionMethods Default --Domain za.tryhackme.com --ExcludeDCs
SharpHound.exe --CollectionMethods Session --Domain za.tryhackme.com --ExcludeDCs
```



## Blood Hound

	neo4j console
	bloodhound --no-sandbox

## Others

- LDAP enumeration - Any valid AD credential pair should be able to bind to a Domain Controller's LDAP interface. This will allow you to write LDAP search queries to enumerate information regarding the AD objects in the domain.
- PowerView - PowerView is a recon script part of the PowerSploit project. Although this project is no longer receiving support, scripts such as PowerView can be incredibly useful to perform semi-manual enumeration of AD objects in a pinch.
- Windows Management Instrumentation (WMI) - WMI can be used to enumerate information from Windows hosts. It has a provider called "root\directory\ldap" that can be used to interact with AD. We can use this provider and WMI in PowerShell to perform AD enumeration.