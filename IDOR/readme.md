### Insecure Direct Object References (IDOR) Bypass Techniques

Insecure Direct Object References (IDOR) are among the most common vulnerabilities found in web applications and can potentially yield substantial rewards in bug bounty programs. This guide will briefly introduce IDOR vulnerabilities and provide some bypass techniques.

**What is IDOR?**

IDOR occurs when an attacker can manipulate input that is used to access objects or data within the application, often leading to unauthorized access to resources.

**Learning about IDOR:**

To learn more about IDOR vulnerabilities and how to identify them, you can refer to resources like [PortSwigger's Web Security Academy - Access Control - IDOR](https://portswigger.net/web-security/access-control/idor).

**IDOR Bypass Techniques:**

1. **Encoded IDs and Hashed IDs:**

   Applications sometimes use encoded or hashed IDs in URLs. For example:
   - `https://example.com/messages?user_id=MTIzNg`

2. **Check for Leaked IDs:**

   It's possible that an application may inadvertently leak user IDs through other API endpoints or public pages.

3. **Offer an Application ID:**

   You can try offering an application-specific user ID in the requests to access data or resources.

   Example:
   ```http
   GET /api_v1/messages
   Host: example.com
   Cookies: session=YOUR_SESSION_COOKIE
   ```

   Try accessing data of another user:
   ```http
   GET /api_v1/messages?user_id=ANOTHER_USERS_ID
   ```

4. **Keep Checking for Blind IDORs:**

   Sometimes, you may not receive a direct response or see changes on the webpage, but the request might trigger in the background. Keep monitoring.

5. **Change the Request Method:**

   Experiment by changing the request method from `GET` to `POST` or vice versa, as some IDOR defenses may rely on request methods.

6. **Change the Requested File Type:**

   Alter the file type in the request to check if the application handles different content types differently.

   Example:
   ```http
   GET /get_receipt?receipt_id=2983
   ```

   Try with a different file type:
   ```http
   GET /get_receipt?receipt_id=2983.json
   ```

7. **Use Burp Suite Extensions:**

   After getting familiar with IDOR vulnerabilities, consider using Burp Suite extensions like Autorize, Automatrix, and Autorepeater to automate and streamline your testing and exploration.

IDOR vulnerabilities can be lucrative for bug hunters, but it's crucial to responsibly disclose and report these issues to protect the application's security. Proper understanding and responsible disclosure are key factors in the bug bounty process.
