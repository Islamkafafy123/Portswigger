# SQL injection attack, listing the database contents on non-Oracle databases
- SQL injection vulnerability in the product category filter
- results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables\
- application has a login function, and the database contains a table that holds usernames and passwords
- To solve the lab, log in as the administrator user.
# Goal
- determine the name of this table and the columns it contains
- retrieve the contents of the table to obtain the username and password of all users
# Solving The Lab
- First Find Number of Columns with
```
' ORDER by 1--
```
- url encode and change the value untill we got and 3 internal server error so columns are 2
- now to the columns type we try string and we got 200 response
```
' UNION SELECT 'a','a'--
```
- now to db version we know from description that its not oracle so we have three options left
```
Microsoft-->	SELECT @@version
PostgreSQL-->	SELECT version()
MySQL-->	SELECT @@version
```
- we tried all but oonly postgresql gave 200 response 
```
' UNION  SELECT version(),'a'--
```
- 
