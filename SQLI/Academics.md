# What Is SQLI
- web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database
- generally allows an attacker to view data that they are not normally able to retrieve. This might include data belonging to other users, or any other data that the application itself is able to access
- In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior
- an attacker can escalate a SQL injection attack to compromise the underlying server or other back-end infrastructure, or perform a denial-of-service attack
# impact of a successful SQL injection
- can result in unauthorized access to sensitive data, such as passwords, credit card details, or personal user information
- In some cases, an attacker can obtain a persistent backdoor into an organization's systems, leading to a long-term compromise that can go unnoticed for an extended period
# Detecting SQL injection vulnerabilities
- SQL injection can be detected manually by using a systematic set of tests
  - Submitting the single quote character ' and looking for errors or other anomalies
  - Submitting some SQL-specific syntax that evaluates to the base (original) value of the entry point, and to a different value, and looking for systematic differences in the resulting application responses
  - Submitting Boolean conditions such as OR 1=1 and OR 1=2, and looking for differences in the application's responses
  - Submitting payloads designed to trigger time delays when executed within a SQL query, and looking for differences in the time taken to respond
  - Submitting OAST payloads designed to trigger an out-of-band network interaction when executed within a SQL query, and monitoring for any resulting interactions
# SQL injection in different parts of the query
- Most SQL injection vulnerabilities arise within the WHERE clause of a SELECT query.
- SQL injection vulnerabilities can in principle occur at any location within the query, and within different query types
  - In UPDATE statements, within the updated values or the WHERE clause
  - In INSERT statements, within the inserted values
  - In SELECT statements, within the table or column name
  - In SELECT statements, within the ORDER BY clause
# Examples
- Retrieving hidden data, where you can modify a SQL query to return additional results
- Subverting application logic, where you can change a query to interfere with the application's logic
- UNION attacks, where you can retrieve data from different database tables.
- Blind SQL injection, where the results of a query you control are not returned in the application's responses
# Blind SQL injection
- application does not return the results of the SQL query or the details of any database errors within its responses
- to exploit blind SQL injection vulnerabilities:
  - change the logic of the query to trigger a detectable difference in the application's response depending on the truth of a single condition
    - involve injecting a new condition into some Boolean logic, or conditionally triggering an error such as a divide-by-zero
  - trigger a time delay in the processing of the query, allowing you to infer the truth of the condition based on the time that the application takes to respond
  - trigger an out-of-band network interaction, using OAST techniques
# Second-order SQL injection
- First-order SQL injection arises where the application takes user input from an HTTP request and, in the course of processing that request, incorporates the input into a SQL query in an unsafe way
- In second-order SQL injection (also known as stored SQL injection), the application takes user input from an HTTP request and stores it for future use
  - done by placing the input into a database, but no vulnerability arises at the point where the data is stored.
  - Later, when handling a different HTTP request, the application retrieves the stored data and incorporates it into a SQL query in an unsafe way
# How to prevent SQL injection
-  prevented by using parameterized queries (also known as prepared statements) instead of string concatenation within the query.
-  vulnerable to SQL injection :
```
String query = "SELECT * FROM products WHERE category = '"+ input + "'";
    Statement statement = connection.createStatement();
    ResultSet resultSet = statement.executeQuery(query);
```
- Mitigation
```
PreparedStatement statement = connection.prepareStatement("SELECT * FROM products WHERE category = ?");
    statement.setString(1, input);
    ResultSet resultSet = statement.executeQuery();
```
- Parameterized queries can be used for any situation where untrusted input appears as data within the query, including the WHERE clause and values in an INSERT or UPDATE statement. They can't be used to handle untrusted input in other parts of the query, such as table or column names, or the ORDER BY clause
- Application functionality that places untrusted data into those parts of the query will need to take a different approach, such as white-listing permitted input values
- For a parameterized query to be effective in preventing SQL injection, the string that is used in the query must always be a hard-coded constant
# Examining the database in SQL injection attacks
- necessary to gather some information about the database itself. This includes the type and version of the database software, and the contents of the database in terms of which tables and columns it contains
- UNION attack
```
' UNION SELECT @@version--
```
# Union Attacks
- The UNION keyword lets you execute one or more additional SELECT queries and append the results to the original query
- For a UNION query to work, two key requirements must be met:
  - The individual queries must return the same number of columns.
  - The data types in each column must be compatible between the individual queries.
- When performing a SQL injection UNION attack, there are two effective methods to determine how many columns are being returned from the original query.
  - The first method involves injecting a series of ORDER BY clauses and incrementing the specified column index until an error occurs
    - you would submit:
    ```
    ' ORDER BY 1--
    ' ORDER BY 2--
    ' ORDER BY 3--
     etc.
    ```
    - This series of payloads modifies the original query to order the results by different columns in the result set
    - The column in an ORDER BY clause can be specified by its index, so you don't need to know the names of any columns. When the specified column index 
     exceeds the number of actual columns in the result set, the database returns an error,
  - The second method involves submitting a series of UNION SELECT payloads specifying a different number of null values
  ```
  ' UNION SELECT NULL--
  ' UNION SELECT NULL,NULL--
  ' UNION SELECT NULL,NULL,NULL--
  etc.
  ```
    - If the number of nulls does not match the number of columns, the database returns an error
