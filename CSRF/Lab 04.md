# CSRF where token is not tied to user session
- lab's email change functionality is vulnerable to CSRF
- uses tokens to try to prevent CSRF attacks, but they aren't integrated into the site's session handling system
- The credentials are as follows:
```
wiener:peter
carlos:montoya
```
# Goal
- use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address
# Solving the Lab
- log in and change email
- for CSRF To be possibel
  - Certain Action : change email
  - cookie based session handeling : session cookie
  - no unpredictablle request parameters :
- Testing For CSRF Tokens
  - Remove CSRF Token and see if application accepts it 
  - change request method from post to get
  - see if csrf token is tied to user session : YES
- we sign in with different account and copy the csrf of the other account and we put it instead of the one used in the first account and we got a redirection > we got a 200 and changed the email
- generate csrf poc from burp and send it to exploit server and we solved the lab
