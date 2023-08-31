# Lab: Username enumeration via different responses
- lab is vulnerable to username enumeration and password brute-force attacks
-  It has an account with a predictable username and password, which can be found in the following wordlists:
Candidate usernames
Candidate passwords
# Goal
- enumerate a valid username, brute-force this user's password, then access their account page.
# Solving the Lab
- With Burp running, investigate the login page and submit an invalid username and password.
- In Burp, go to Proxy > HTTP history and find the POST /login request. Highlight the value of the username parameter in the request and send it to Burp Intruder.
- In Burp Intruder, go to the Positions tab. Notice that the username parameter is automatically set as a payload position. This position is indicated by two § symbols, for example: username=§invalid-username§.
- the Sniper attack type is selected.
- On the Payloads tab, make sure that the Simple list payload type is selected.
- Under Payload settings, paste the list of candidate usernames. Finally, click Start attack. The attack will start in a new window.
- When the attack is finished, on the Results tab, examine the Length column. You can click on the column header to sort the results. Notice that one of the entries is longer than the others. Compare the response to this payload with the other responses. Notice that other responses contain the message Invalid username, but this response says Incorrect password. Make a note of the username in the Payload column.
- we find username adkit
- Close the attack and go back to the Positions tab. Click Clear, then change the username parameter to the username you just identified. Add a payload position to the password parameter. The result should look something like this:
```
username=adkit&password=§invalid-password§
```
- On the Payloads tab, clear the list of usernames and replace it with the list of candidate passwords. Click Start attack.
- When the attack is finished, look at the Status column. Notice that each request received a response with a 200 status code except for one, which got a 302 response. This suggests that the login attempt was successful
- now login and we solved the lab
