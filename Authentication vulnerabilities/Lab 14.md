# Password brute-force via password change
- lab's password change functionality makes it vulnerable to brute-force attacks
- Your credentials: wiener:peter
- Victim's username: carlos
# Goal
- use the list of candidate passwords to brute-force Carlos's account and access his "My account" page
# Solving The Lab
- with burp running we submit password correctly once and password incorrectly once and observe the requests
- the username is submitted as hidden input in the request
- when you enter the wrong current password. If the two entries for the new password match, the account is locked
- if we enter two different new passwords, an error message simply states Current password is incorrect
- if we enter a valid current password, but two different new passwords, the message says New passwords do not match. We can use this message to enumerate correct passwords
- Enter  correct current password and two new passwords that do not match. Send this POST /my-account/change-password request to Burp Intruder
- change the username parameter to carlos and add a payload position to the current-password parameter.
- Make sure that the new password parameters are set to two different values
- On the Payloads tab, enter the list of passwords as the payload set
- On the Settings tab, add a grep match rule to flag responses containing New passwords do not match. Start the attack.
- notice that one response was found that contains the New passwords do not match message
- now that response contain the password now log out of current account and log in with carlos and we solved the lab
