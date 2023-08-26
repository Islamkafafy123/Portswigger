# Exploiting blind SQL injection by triggering time delays
- it is often possible to exploit the blind SQL injection vulnerability by triggering time delays conditionally
- depending on an injected condition. Because SQL queries are generally processed synchronously by the application, delaying the execution of a SQL query will also delay the HTTP response
- allows us to infer the truth of the injected condition based on the time taken before the HTTP response is received
- techniques for triggering a time delay are highly specific to the type of database being used
- On Microsoft SQL Server, input like the following can be used to test a condition and trigger a delay depending on whether the expression is true
```
'; IF (1=2) WAITFOR DELAY '0:0:10'--
'; IF (1=1) WAITFOR DELAY '0:0:10'--
```
- The first of these inputs will not trigger a delay, because the condition 1=2 is false. The second input will trigger a delay of 10 seconds, because the condition 1=1 is true
- this technique, we can retrieve data in the way already described, by systematically testing one character at a time
```
'; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:{delay}'--
```
# Lab: Blind SQL injection with time delays
- blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie
- since the query is executed synchronously, it is possible to trigger conditional time delays to infer information
# Goal
- exploit the SQL injection vulnerability to cause a 10 second delay
# Solving The lab
- intercept and modify the request containing the TrackingId cookie
- we need to fuzz the app with different payload to find the db we are dealing with
- we start our payload like this and try to test the oracle version of the payload and no response
```
TrackingId=x'||dbms_pipe.receive_message(('a'),10)--
```
- try with microsoft and nothing again
```
TrackingId=x'||WAITFOR DELAY '0:0:10'--
```
- now with postgres and we got the respone late so we solved the lab
```
TrackingId=x'||(SELECT pg_sleep(10))--
```
