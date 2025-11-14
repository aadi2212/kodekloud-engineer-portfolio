📌 Task Objective

Automate the backup of the kodekloud\_db01 database on the Database Server and store the backup on the Backup Server. 



The Jenkins job must:

1]Generate MySQL dump using user kodekloud\_roy (password: asdfgdsd)

2]Filename format: db\_$(date +%F).sql

3]Copy dump to Backup Server → /home/clint/db\_backups/

4]Schedule job to run every 10 minutes: \*/10 \* \* \* \*





📌 Step 1: Login to Jenkins

Open Jenkins UI.

Login credentials:

&nbsp;	○ Username: admin

&nbsp;	○ Password: Adm!n321





📌 Step 2: Install Required Plugins

Navigate:

Manage Jenkins → Plugins



Install the following plugins:

&nbsp;	• SSH

&nbsp;	• SSH Credentials

&nbsp;	• SSH Build Agents





Then click:

&nbsp;	• Restart Jenkins when installation is complete





📌 Step 3: Add SSH Credentials for Database Server (stdb01)



Navigate:

Manage Jenkins → Credentials → System → Global credentials → Add Credentials





Enter:

&nbsp;	• Username: peter (DB server user)

&nbsp;	• Password: (password of DB server)

&nbsp;	• ID: db\_creds

&nbsp;	• Save.





📌 Step 4: Configure SSH Remote Host (DB Server)

Navigate:

Manage Jenkins → Configure System





Under SSH remote hosts / SSH Sites:

• Click Add

• Hostname: stdb01

• Port: 22

• Credentials: select peter (db\_creds)

• Click Test Connection

• It should show “Successful connection”



Save configuration.





📌 Step 5: Create Jenkins Job — database-backup

Navigate:

New Item → Freestyle Project → database-backup → OK



Configure:

Build Triggers:



Enable:

Build periodically



Schedule:

\*/10 \* \* \* \*





📌 Step 6: Create DB Dump (First Part of Build Step)

Under Build Steps →



Select:

Execute shell script on remote host using SSH



Jenkins auto-detects:

peter@stdb01:22



In Command section, write:

mysqldump -u kodekloud\_roy -pasdfgdsd kodekloud\_db01 > db\_$(date +%F).sql



Save job.





📌 Step 7: Configure SSH Key Authentication (DB → Backup Server)

Login to Jump Host, then connect to DB Server:

ssh peter@stdb01



Generate SSH Key on DB Server:

ssh-keygen -t rsa

(Press Enter for all prompts)



Copy Key to Backup Server:

ssh-copy-id clint@stbkp01



• Type yes

• Enter backup server password





Test Passwordless Login

ssh clint@stbkp01



It should connect without password.



Exit:





📌 Step 8: Add SCP Command to Copy Backup File

Edit Jenkins job → Configure → Build Step command:



mysqldump -u kodekloud\_roy -pasdfgdsd kodekloud\_db01 > db\_$(date +%F).sql

scp -o StrictHostKeyChecking=no db\_$(date +%F).sql clint@stbkp01:/home/clint/db\_backups/



Save job.





📌 Step 9: Run the Jenkins Job

• Click Build Now

• Job should complete successfully.





📌 Step 10: Validate Backup on Backup Server

From DB server:



ssh clint@stbkp01

ls

cd db\_backups

ls



You should see a file like:

db\_2025-11-14.sql



View contents:

cat db\_2025-11-14.sql



You should see the MySQL dump.





✅ Task Successfully Completed



You now have:



✔ Jenkins job

✔ Automated MySQL backup

✔ Passwordless transfer to Backup Server

✔ Cron scheduling every 10 minutes

✔ Verified dump on backup server



