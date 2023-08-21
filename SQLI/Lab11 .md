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
