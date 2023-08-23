# Visible error-based SQL injection
- Extracting sensitive data via verbose SQL error messages
  - Misconfiguration of the database sometimes results in verbose error messages
  - can provide information that may be useful to an attacker
  - the following error message, which occurs after injecting a single quote into an id paramete
  ```
  Unterminated string literal started at position 52 in SQL SELECT * FROM tracking WHERE id = '''. Expected char
  ```
  - This shows the full query that the application constructed using our input
  - we can see the context that we're injecting into
  - that is, a single-quoted string inside a WHERE statement. This makes it easier to construct a valid query containing a malicious payload
  - we can see that commenting out the rest of the query would prevent the superfluous single-quote from breaking the syntax
  - may be able to induce the application to generate an error message that contains some of the data that is returned by the query
  - effectively turns an otherwise blind SQL injection vulnerability into a "visible" one
  - One way of achieving this is to use the CAST() function, which enables you to convert one data type to another
  - a query containing the following statement
  ```
  CAST((SELECT example_column FROM example_table) AS int)
  ```
  - Often, the data that you're trying to read is a string
  - Attempting to convert this to an incompatible data type, such as an int, may cause an error similar to the following
  ```
  ERROR: invalid input syntax for type integer: "Example data"
  ```
  - may also be useful in cases where you're unable to trigger conditional responses because of a character limit imposed on the query.
# Lab
- SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie
- The results of the SQL query are not returned
- database contains a different table called users, with columns called username and password
# Goal
- leak the password for the administrator
- Log in via Administrator
# Solving The lab  
- Go to the Proxy > HTTP history tab and find a GET / request that contains a TrackingId cookie
- 

