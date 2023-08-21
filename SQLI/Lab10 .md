# SQL injection UNION attack, retrieving multiple values in a single column
- SQL injection vulnerability in the product category filter.
- use a UNION attack to retrieve data from other tables.
- The database contains a different table called users, with columns called username and password.
# Goal
- To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.
# Solving The lab
- finding number of columns
```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
etc.
```
- after trying payloads we fnd that db has 2 columns
- now to determining datatypes of columns or which columns has string datatype
