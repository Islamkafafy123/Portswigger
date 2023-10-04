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
  - no unpredictablle request parameters : yes
- need to only bypass the samesite as we see the website explicitly specifies SameSite=Strict when setting session cookies
- now we Identify a suitable gadget
  - i noticed that after we post a comment the site sent to a confirmation page at /post/comment/confirmation?postId=x but, after a few seconds, the site is taken back to the blog post
  - looking at burp i notice that this redirect is handled client-side using the imported JavaScript file /resources/js/commentConfirmationRedirect.js
  - this uses the postId query parameter to dynamically construct the path for the client-side redirect
  - visit this URL, but change the postId parameter to an anything
    ```
    /post/comment/confirmation?postId=islam
    ```
  - i see the post confirmation page before the client-side JavaScript attempts to redirect you to a path containing my string
  - trying path traversal to my account
  ```
  /post/comment/confirmation?postId=1/../../my-account
  ```
  - the browser normalizes this URL and successfully takes me to  account page. This confirms that i can use the postId parameter to elicit a GET request for an arbitrary endpoint on the target site
- Bypass the SameSite restrictions
  - going to the exploit server and create a script that induces the viewer's browser to send the GET request i just tested
  ```
  <script>
    document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=../my-account";
  </script>
  ```
  - when the client-side redirect takes place,  still end up on logged-in account page. This confirms that the browser included  authenticated session cookie in the second request, even though the initial comment-submission request was initiated from an arbitrary external site
- now to final exploit
```
<script>
    document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=pwned%40web-security-academy.net%26submit=1";
</script>
```
- and lab solved
