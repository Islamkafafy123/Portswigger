# Visible error-based SQL injection
- Extracting sensitive data via verbose SQL error messages
  - Misconfiguration of the database sometimes results in verbose error messages
  - can provide information that may be useful to an attacker
  - the following error message, which occurs after injecting a single quote into an id paramete
  ```
  Unterminated string literal started at position 52 in SQL SELECT * FROM tracking WHERE id = '''. Expected char
  ```
  - This shows the full query that the application constructed using our input
  -  we can see the context that we're injecting into
  -  that is, a single-quoted string inside a WHERE statement. This makes it easier to construct a valid query containing a malicious payload
  -  we can see that commenting out the rest of the query would prevent the superfluous single-quote from breaking the syntax
