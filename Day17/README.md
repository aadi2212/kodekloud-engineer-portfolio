Install and Configure PostgreSQL



1]The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below: PostgreSQL database server is already installed on the Nautilus database server. 



a. Create a database user kodekloud\_joy and set its password to "LQfKeWWxWD".



b. Create a database kodekloud\_db5 and grant full permissions to user kodekloud\_joy on this database.





Note: Please do not try to restart PostgreSQL server service.



->





📝 PostgreSQL Database Setup – Stratos DC



Task Requirement:



1]PostgreSQL server is already installed on stdb01.

2]Create a database user kodekloud\_joy with password LQfKeWWxWD.

3]Create a database kodekloud\_db5.

4]Grant full permissions on this DB to kodekloud\_joy.

5]⚠️ Do not restart PostgreSQL service.





Steps Taken:



1\. Login to DB Server

From jump host:

ssh peter@stdb01



Switch to postgres system user:

sudo su - postgres

psql





2\. Create Database User

CREATE USER kodekloud\_joy WITH PASSWORD 'LQfKeWWxWD';



Verified user:

\\du



(User kodekloud\_joy listed ✅)





3\. Mistake While Creating Database

Initially, I typed without semicolon:



CREATE DATABASE kodekloud\_db5



This made Postgres wait for more input (postgres-# prompt).

Then I mistakenly typed another CREATE DATABASE inside it →



Error:

syntax error at or near "CREATE"

Finally, after typing ; it executed nothing valid, so database was not created.





4\. Correct Database Creation

CREATE DATABASE kodekloud\_db5;



Verified:

\\l



(Database kodekloud\_db5 listed ✅)





5\. Grant Permissions

GRANT ALL PRIVILEGES ON DATABASE kodekloud\_db5 TO kodekloud\_joy;





6\. Verification

Users:



\\du

(Confirmed kodekloud\_joy)





7]Databases:

\\l

(Confirmed kodekloud\_db5 owned and accessible)





8]Test connection (on stdb01):

psql -U kodekloud\_joy -d kodekloud\_db5 -h localhost



Password: LQfKeWWxWD



Prompt:



kodekloud\_db5=>



✅ Successful login.





Troubleshooting Notes

&nbsp;	• ❌ Forgot ; after SQL statements → Postgres went into multiline mode.

&nbsp;	• ❌ Entered second CREATE DATABASE inside unfinished command → syntax error.

&nbsp;	• ✅ Fixed by canceling with Ctrl+C and rerunning with proper semicolon.

&nbsp;	• ✅ User \& DB created, privileges granted, connection verified.



