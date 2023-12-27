Change Password
```shell-session
Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
```

Force Password Change at Logon
```shell-session
Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose
```

Force Group Policy Update
```shell-session
gpupdate /force
```

