# SQL injection vulnerability allowing login bypass
- vulnerability in the login function
- Goal : logs in to the application as the administrator user
# Solving The Lab
- First we try loggin in with admin:admin and we get invalid uername or password
- try to break the application with ( ' )
- so ( ' ):admin and we got internal server error
- as in the academic the Subverting application logic we need to remove the password
- by using the SQL comment sequence -- to remove the password check from the WHERE clause of the query
- our query is like this
```
SELECT * FROM users WHERE username = 'admin' AND password = 'admin'
```
- when we have breaked it, it looked like this
```
SELECT * FROM users WHERE username = ''' AND password = 'admin'
```
- as there is no username with ( ' ) it gave internal server error
- what we can do is let it log in administrator and ignore the rest of the query like this
```
SELECT * FROM users WHERE username = 'administrator'--' AND password = 'admin'
```
- with the payload ( administrator'--) in username field and anything in the password field
- And we solved the lab
