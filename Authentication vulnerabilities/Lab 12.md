# Password reset broken logic
- lab's password reset functionality is vulnerable.
- our credentials: wiener:peter
- Victim's username: carlos
# Goal
- reset Carlos's password then log in and access his "My account" page
# Solving The Lab
- Burp running, click the Forgot your password? link and enter wiener
- Click the Email client button to view the password reset email that was sent. Click the link in the email and reset your password
- go to burp
- the reset token is provided as a URL query parameter in the reset email. Notice that when you submit your new password, the POST /forgot-password?temp-forgot-password-token request contains the username as hidden input.
- Send this request to Burp Repeater
- the password reset functionality still works even if you delete the value of the temp-forgot-password-token parameter in both the URL and request body.
- the token is not being checked when you submit the new password
- request a new password reset and change your password again. Send the POST /forgot-password?temp-forgot-password-token request to Burp Repeater again
- delete the value of the temp-forgot-password-token parameter in both the URL and request body. Change the username parameter to carlos. Set the new password
- log in to Carlos's account using the new password you just set. Click My account and we solved the lab
