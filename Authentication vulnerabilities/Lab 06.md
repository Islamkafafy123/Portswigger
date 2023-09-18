# Broken brute-force protection, multiple credentials per request
- lab is vulnerable due to a logic flaw in its brute-force protection
# Goal
- lab is vulnerable due to a logic flaw in its brute-force protection
# Solving The lab
-  Notice that the POST /login request submits the login credentials in JSON format. Send this request to Burp Repeater
-  in Repeater, replace the single string value of the password with an array of strings containing all of the candidate passwords
```
"username" : "carlos",
"password" : [
    "123456",
    "password",
    "qwerty"
    ...
]
```
- This will return a 302 response and we solved the lab if we follow the redirect

