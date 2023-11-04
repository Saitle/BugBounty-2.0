### Testing URL Redirect Vulnerabilities

URL redirect vulnerabilities can pose security risks, allowing attackers to manipulate or trick users into visiting unintended websites. Here are various techniques to test and identify such vulnerabilities:

**Using `waybackurl` and `gf` Tool:**

You can automate the task of testing for URL redirects using the `waybackurl` tool in conjunction with `gf` for finding patterns. The following command can be used:

```shell
waybackurl example.com | gf <plugin of choice, e.g., interestingparams | grep <the provided keywords>
```

**Leveraging Google Dorks:**

You can utilize Google dorks to automate the process of finding potential URL redirects on a website. Examples of dorks include:

```shell
inurl:redir site:example.com
inurl:redirect site:example.com
inurl:redirecturi site:example.com
inurl:redirect_uri site:example.com
inurl:redirecturl site:example.com
inurl:redirect_uri site:example.com
inurl:return site:example.com
inurl:returnurl site:example.com
inurl:relaystate site:example.com
inurl:forward site:example.com
inurl:forwardurl site:example.com
inurl:forward_url site:example.com
inurl:url site:example.com
inurl:uri site:example.com
inurl:dest site:example.com
inurl:destination site:example.com
inurl:next site:example.com
```

**Testing with Injected Domain:**

Start testing by injecting a domain into the URL, for example:

```shell
http://example.com/redir?=<attackerdomain>.com
```

**Bypass Techniques:**

URL redirect vulnerabilities can sometimes be mitigated using certain bypass techniques. Some common bypasses include:

- Using `@` symbol: `https://example.com/@attacker.com`
- Using browser autocorrect: `https:attacker.com`, `https;attacker.com`, `https:\/\/attacker.com`, `https:/\/\attacker.com`
- Flawed validator: `https://example.com/login?redir=http://example.com.attacker.com`
- Using data URLs: 
    ```shell
    data:text/html;base64,
    PHNjcmlwdD5sb2NhdGlvbj0iaHR0cHM6Ly9leGFtcGxlLmNvbSI8L3NjcmlwdD4=
    ```
    Decodes to `<script>location="https://example.com"</script>`
- Encoding bypass: `https://attacker.com%252f@example.com`
- Non-ASCII character: `https://attacker.com%ff.example.com` (using character ÿ)
- Combining bypass techniques to bypass mitigations

**Automating URL Redirect Testing:**

[BONUS] Soon, there will be a tool developed to automate URL redirect vulnerability testing with various bypass techniques as options.

Testing and identifying URL redirect vulnerabilities is crucial for ensuring web application security. Proper testing, responsible disclosure, and mitigation are essential to protect against potential threats.
