# SQL injection UNION attack, determining the number of columns returned by the query
- SQL injection vulnerability in the product category filter.
- results from the query are returned in the application's response
- use a UNION attack to retrieve data from other tables
- determine the number of columns that are being returned by the query
# Goal
- determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values
# solving the Lab 
- we try the union method for determining the number of columns
```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
```
- inserting the last query solves the lab
