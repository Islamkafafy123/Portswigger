# SQL injection attack, querying the database type and version on MySQL and Microsoft
- SQL injection vulnerability in the product category filter
# Goal
- display the database version string
# Solving The Lab
- First add single quetes in the category and see respone and we see internal server error so there is a possibilty that sqli is here
- now need to find number of columns
```
' ORDER by 1--
```
- but we get internal server error whenver we change the value so the server doesnt like our request we can change it to be like this
```
' ORDER by 1##
```
- after url encoding it
```
'+ORDER+by+1%23%23
```
- we got 200 we increase the value and we got 2 columns when we put 3 we get server error
- now we want to figure out the columns type and we can deduce it from the site as it returns 2 texts one is the header and other is description
- now to the union attack
```
SELECT @@version
```
- this is the payload for mysql db now we concat with the union attack
```
' UNION SELECT @@version , 'a' ##
```
- after url encoding it
```
'+UNION+SELECT+%40%40version+,+'a'+%23%23
```
