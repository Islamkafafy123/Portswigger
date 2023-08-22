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
