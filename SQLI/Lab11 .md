# Blind SQL injection with conditional responses
- application that uses tracking cookies to gather analytics about usage. Requests to the application include a cookie header like this
```
Cookie: TrackingId=u5YD3PapBcR4lN3e7Tj4
```
- When a request containing a TrackingId cookie is processed, the application determines whether this is a known user using a SQL query like this
```
SELECT TrackingId FROM TrackedUsers WHERE TrackingId = 'u5YD3PapBcR4lN3e7Tj4'
```
- This query is vulnerable to SQL injection, but the results from the query are not returned to the user.
- the application does behave differently depending on whether the query returns any data. If it returns data (because a recognized TrackingId was submitted), then a "Welcome back" message is displayed within the page
- This behavior is enough to be able to exploit the blind SQL injection vulnerability and retrieve information by triggering different responses conditionally
- to see how this works, suppose that two requests are sent containing the following TrackingId cookie values in turn
```
…xyz' AND '1'='1
…xyz' AND '1'='2
```
- The first of these values will cause the query to return results, because the injected AND '1'='1 condition is true, and so the "Welcome back" message will be displayed
-  the second value will cause the query to not return any results, because the injected condition is false, and so the "Welcome back" message will not be displayed.
# Lab
- blind SQL injection vulnerability
- The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie
- The results of the SQL query are not returned, and no error messages are displayed. But the application includes a "Welcome back" message in the page if the query returns any rows
- database contains a different table called users, with columns called username and password
# Goal
- enumrate the password of the administrator
- log in as the administrator user
# Solving the lab
- we open the cookie editor and we see 2 cookies one is session cookie and one is trackinf cookie the lab says the tracking cookie is vulnurable
- to determine which is vulnurable we need to fuzz every parameter that talks to the backend and see the responses
- when we open the lab we dont see a welcome message because its the first time using the cookie
- when we click ony any button we see the welcome back message
- now open burp to confirm that this is vulnurable, intercept request and send request to repeater
- to test for bling based SQLI is to force 2 use cases and see how the application response one false and one true
  - if two responses are different then its expoitable
- now to confirm parameter is vulnurable to blind SQLI
- the Query we will assume it will look like this
```
SELECT tracking-id FROM tracking-table WHERE tracking-id = '73otf20UOtx4AXLk' 
```
- If this tracking id exists -> query returns value -> Welcome back message
- If the tracking id doesn't exist -> query returns nothing -> no Welcome back message
- select tracking-id from tracking-table where trackingId = '73otf20UOtx4AXLk' and 1=1--'
- we comment to not get syntax error
- we send the payload and url encode it 
-> TRUE -> Welcome back

- select tracking-id from tracking-table where trackingId = '73otf20UOtx4AXLk' and 1=0--'
-> FALSE -> no Welcome back
- since we dont get a welcome back message when query is false so its vulnurable to blind SQLI
