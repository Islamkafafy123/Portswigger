# Username enumeration via account lock
- lab is vulnerable to username enumeration
- It uses account locking, but this contains a logic flaw
# Goal
- To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page
# solving The lab
- we first check if we have a lockout policy we try 7 times and we dont get lockout so probebly no lockout policy
- now  submit an invalid username and password. Send the POST /login request to Burp Intruder
- attack type Cluster bomb. Add a payload position to the username parameter. Add a blank payload position to the end of the request body by clicking Add § twice. The result should look something like this
```
username=§carlos§&password=example§§
```
- add the list of usernames to the first payload set. For the second set, select the Null payloads type and choose the option to generate 5 payloads. This will effectively cause each username to be repeated 5 times
- notice that the responses for one of the usernames were longer than responses when using other usernames
- notice that it contains a different error message: You have made too many incorrect login attempts
```
You have made too many incorrect login attempts.
```
- now we have a username which is
```
ads
```
- now bruteforce password
- this time select the Sniper attack type. Set the username parameter to the ' ads ' that you just identified and add a payload position to the password parameter
- create a grep extraction rule for the error message
- In the results, look at the grep extract column. Notice that there are a couple of different error messages, but one of the responses did not contain any error message.
- and its the password
```
2000
```
