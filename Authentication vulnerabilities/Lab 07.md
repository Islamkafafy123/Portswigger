# 2FA simple bypass
- two-factor authentication can be bypassed. You have already obtained a valid username and password
- Your credentials: wiener:peter
- Victim's credentials carlos:montoya
# Goal
- access Carlos's account page.
# Solving the lab
- Log in using the victim's credentials
- intercept request
- when request is made to login2 (2fa) drop the request and manually change the URL to navigate to /my-account
