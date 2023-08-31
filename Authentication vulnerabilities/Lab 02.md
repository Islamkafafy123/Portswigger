# Lab: Username enumeration via subtly different responses
- lab is subtly vulnerable to username enumeration and password brute-force attacks.
- has an account with a predictable username and password, which can be found in the following wordlists:
Candidate usernames
Candidate passwords
# Goal
- enumerate a valid username, brute-force this user's password, then access their account page.
# Solving The Lab
- With Burp running, submit an invalid username and password. Highlight the username parameter in the POST /login request and send it to Burp Intruder
- On the Payloads tab, notice that the username parameter is automatically marked as a payload position. Make sure that the Simple list payload type is selected and add the list of candidate usernames
- we get creative with going to repeater with the request and submit a definitely wrong username and password and grep the text for invalid username and go to intruder and filter this text with negative search
- When the attack is finished Sort the results with the filter
- Look closer at this response and notice that it contains a typo in the error message - instead of a full stop/period, there is a trailing space. Make a note of this username.
- Close the attack and go back to the Positions tab. Insert the username you just identified and add a payload position to the password parameter:
```
username=analyzer&password=§invalid-password§
```
- On the Payloads tab, clear the list of usernames and replace it with the list of passwords. Start the attack.
- When the attack is finished, notice that one of the requests received a 302 response. Make a note of this password.
- login and we solved the lab
