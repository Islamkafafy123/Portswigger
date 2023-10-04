# SameSite Strict bypass via client-side redirect
- lab's change email function is vulnerable to CSRF
- credentials: wiener:peter
# Goal
- perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack
# Solving The Lab
- log in and change email
- for CSRF To be possibel
  - Certain Action : change email
  - cookie based session handeling : session cookie
  - no unpredictablle request parameters 
