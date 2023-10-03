# CSRF where token validation depends on request method
- lab's email change functionality is vulnerable to CSRF
- It attempts to block CSRF attacks, but only applies defenses to certain types of requests
- credentials: wiener:peter
# Goal
- use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address
# Solving the Lab
- change mail and send request to repeater        
- Testing For CSRF Tokens
  - change request method from post to get from right click of the request on the repeater and click change request
  - follow redirection and we got the email changed
  - we try different emails and yes it works
  - we right click on engagment tool and generate csrf poc and change mail > regenrate > copy html
  - go to exploit server and paste the payload ,deliver to vitim and we solved the lab
