# 2FA broken logic
- This lab's two-factor authentication is vulnerable due to its flawed logic.
- Your credentials: wiener:peter
- Victim's username: carlos

# Goal
- access Carlos's account page.
# Solving the Lab
- log in to your own account and investigate the 2FA verification process. Notice that in the POST /login2 request, the verify parameter is used to determine which user's account is being accessed
- Log out of your account.
- Send the GET /login2 request to Burp Repeater. Change the value of the verify parameter to carlos and send the request. This ensures that a temporary 2FA code is generated for Carlos
- Go to the login page and enter your username and password. Then, submit an invalid 2FA code
- intercept request and send it to intruder
- in intruder set the verify parameter to carlos and add a payload position to the mfa-code parameter. Brute-force the verification code like rana khalil
- check the 302 response and copy the session and replace it when logged in with wiener and change url to my account and hit enter and we solved the lab
