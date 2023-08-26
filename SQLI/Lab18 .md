# SQL injection with filter bypass via XML encoding
- SQL injection vulnerability in its stock check feature
- results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables.
- database contains a users table, which contains the usernames and passwords of registered users
# Goal
- retrieve the admin user's credentials, then log in to their account
- The hint Says the lab has WAF
# Solving The lab
- First thing To do Is to look for input vectors 
