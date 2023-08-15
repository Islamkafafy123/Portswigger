# SQL injection attack, querying the database type and version on Oracle
- SQL injection vulnerability in the product category filter
# Goal
- display the database version string
# Solving The Lab
- First Thing we need to do is Determin The number of Columns with order by
```
' ORDER by 1--
```
- lets intercept the request by burp and sent to repeater
- copy the payload put it after any category and CTRL U to encode it and send
- we get 200 response and it confirms we have atleast one column
- when we try 2 we get 200 also bt when we try 3 we get internal server error
- the hint says since this lab is On Oracle databases, every SELECT statement must specify a table to select FROM
 - There is a built-in table on Oracle called dual which you can use for this purpose
- now we got 2 columns we build our payload like this
```
' UNION SELECT 'a','a' FROM DUAL--
```
- put it after any category choosen and CTRL U To url encode and send and we got 200
- now lets output the version from the cheatsheat the oracle version is outputed like this
```
SELECT banner FROM v$version
```
- lets test another payload like this
```
' UNION SELECT banner, NULL FROM v$version--
```
- and we solved the lab
