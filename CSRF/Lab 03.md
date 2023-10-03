# CSRF where token validation depends on token being present
- lab's email change functionality is vulnerable to CSRF
- credentials: wiener:peter
# Goal
- use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address
# Solving The Lab
- log in and change email
- for CSRF To be possibel
  - Certain Action : change email
  - cookie based session handeling : session cookie\
  - no unpredictablle request parameters :
- Testing For CSRF Tokens
  - Remove CSRF Token and see if application accepts it : YES
  - change request method from post to get
- now Remove CSRF Token and send the request and follow the redirect and we changed the email now generate a csrf poc from burb and copy it to the exploit server and we solved the lab
