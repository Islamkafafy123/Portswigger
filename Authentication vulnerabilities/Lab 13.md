# Password reset poisoning via middleware
- lab is vulnerable to password reset poisoning
- user carlos will carelessly click on any links in emails that he receives
# Goal
- og in to Carlos's account. You can log in to your own account using the following credentials: wiener:peter.
- Any emails sent to this account can be read via the email client on the exploit server
# Solving The Lab
- we go to the lab with burp running and forget the password for wiener and observe the requests
- Send the POST /forgot-password request to Burp Repeater
- now we try the X-Forwarded-Host header which is supported and you can use it to point the dynamically generated reset link to an arbitrary domain
- Go to the exploit server and make a note of your exploit server URL
- in Burp Repeater and add the X-Forwarded-Host header with your exploit server URL
```
X-Forwarded-Host: exploit-0a4b00bf048106808331d285017a00ac.exploit-server.net
```
- now Change the username parameter to carlos and send the request
- now open the exploit server and make a note of the token from the forget pass get request
- now go back to email client and copy the valid password reset link (not the one that points to the exploit server).
- Paste this into the browser and change the value of the temp-forgot-password-token parameter to the value that you stole from the victim
- Load this URL and set a new password for Carlos's account
- Log in to Carlos's account using the new password and we solved the lab
