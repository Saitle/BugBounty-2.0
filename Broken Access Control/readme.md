# Exposed Admin Panels Hunting

## Directory Bruteforcing and Google Dorks

- Look for exposed admin panels using either directory bruteforcing or Google dorks.

### Directory Traversal

- Example:
  - `http://example.com/upload?file=../../../../../etc/shadow`
- Use the command `waybackurls <domain name> | gf interesting params | grep file=`
- Find extensive payloads on [PayloadsAllTheThings Directory Traversal README](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Directory%20Traversal/README.md)

## Hunting

1. Learn about your target application. The more you understand about the architecture and development process of the web application, the better you’ll be at spotting these vulnerabilities.

2. Intercept requests while browsing the site and pay attention to sensitive functionalities. Keep track of every request sent during these actions.

3. Use your creativity to think of ways to bypass access control or otherwise interfere with application logic.

4. Think of ways to combine the vulnerability you’ve found with other vulnerabilities to maximize the potential impact of the flaw.

5. Draft your report! Be sure to communicate to the receiver of the report how the issue could be exploited by malicious users.
