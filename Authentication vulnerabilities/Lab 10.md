# Brute-forcing a stay-logged-in cookie
- lab allows users to stay logged in even after they close their browser session
- The cookie used to provide this functionality is vulnerable to brute-forcing.
# Goal
- brute-force Carlos's cookie to gain access to his "My account" page.
- Your credentials: wiener:peter
- Victim's username: carlos
# Solving The lab
- with burp log in to wiener with the Stay logged in option selected. Notice that this sets a stay-logged-in cookie
- this cookie in Inspector panel and we notice that it is Base64-encoded. Its decoded value is wiener:51dc30ddc473d43a6011e9ebba6ca770
- the length and character set of this string and could be an MD5 hash
- so we can deduce the cookie is something like that
```
base64(username+':'+md5HashOfPassword)
```
- now log out
- go to requests and send the myaccount request with the staylogged in cookie to intruder
- now add the stayed looged in cookie and remove the session cookie value
- Under Payload processing, add the following rules in order
```
Hash: MD5
Add prefix: wiener:
Encode: Base64-encode
```
- now start the attack and check for 200 in response and we solved the lab
