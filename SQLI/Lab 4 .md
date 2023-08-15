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

