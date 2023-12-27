In a runas.exe window, we can start MMC, which will ensure that all MMC network connections will use our injected AD credentials.

Add the AD RSAT Snap-In

1. Press Start
2. Search "Apps & Features" and press enter
3. Click Manage Optional Features
4. Click Add a feature
5. Search for "RSAT"
6. Select "RSAT: Active Directory Domain Services and Lightweight Directory Tools" and click Install

In MMC, we can now attach the AD RSAT Snap-In:

1. Click File -> Add/Remove Snap-in
2. Select and Add all three Active Directory Snap-ins
3. Click through any errors and warnings
4. Right-click on Active Directory Domains and Trusts and select Change Forest
5. Enter za.tryhackme.com as the Root domain and Click OK
6. Right-click on Active Directory Sites and Services and select Change Forest
7. Enter za.tryhackme.com as the Root domain and Click OK
8. Right-click on Active Directory Users and Computers and select Change Domain
9. Enter za.tryhackme.com as the Domain and Click OK
10. Right-click on Active Directory Users and Computers in the left-hand pane
11. Click on View -> Advanced Features