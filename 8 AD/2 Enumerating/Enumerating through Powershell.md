Enumerate AD users:

	Get-ADUser -Identity gordon.stevens -Server za.tryhackme.com -Properties *

Use the -Filter parameter that allows more control over enumeration and use the Format-Table cmdlet to display the results neatly:

	Get-ADUser -Filter 'Name -like "*stevens"' -Server za.tryhackme.com | Format-Table Name,SamAccountName -A

Enumerate AD groups:

	Get-ADGroup -Identity Administrators -Server za.tryhackme.com

Enumerate group membership

	Get-ADGroupMember -Identity Administrators -Server za.tryhackme.com

Enumerate all AD objects that were changed after a specific date

	 $ChangeDate = New-Object DateTime(2022, 02, 28, 12, 00, 00)
	Get-ADObject -Filter 'whenChanged -gt $ChangeDate' -includeDeletedObjects -Server za.tryhackme.com

Enumerate accounts that have a badPwdCount that is greater than 0

	Get-ADObject -Filter 'badPwdCount -gt 0' -Server za.tryhackme.com

Retrieve additional information about the specific domain

	Get-ADDomain -Server za.tryhackme.com

