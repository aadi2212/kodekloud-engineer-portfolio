&nbsp;Jenkins Scheduled Jobs



1]The devops team of xFusionCorp Industries is working on to setup centralised logging management system to maintain and analyse server logs easily. Since it will take some time to implement, they wanted to gather some server logs on a regular basis. At least one of the app servers is having issues with the Apache server. The team needs Apache logs so that they can identify and troubleshoot the issues easily if they arise. So they decided to create a Jenkins job to collect logs from the server. Please create/configure a Jenkins job as per details mentioned below:





Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321



1\. Create a Jenkins jobs named copy-logs.



2\. Configure it to periodically build every 2 minutes to copy the Apache logs (both access\_log and error\_logs) from App Server 1 (from default logs location) to location /usr/src/data on Storage Server.



Note:



1\. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.



2\. Please make sure to define you cron expression like this \*/10 \* \* \* \* (this is just an example to run job every 10 minutes).



3\. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.



->



Jenkins Task Documentation – copy-logs Job (With All Errors \& Fixes)



xFusionCorp Industries – Centralized Logging Setup





1\. Task Objective



The DevOps team needed a Jenkins job named copy-logs that:

&nbsp;	• Collects Apache logs (access\_log and error\_log) from App Server 1

&nbsp;	• Transfers them periodically (every 2 minutes)

&nbsp;	• Stores them on the Storage Server at:

&nbsp;      /usr/src/data



This helps in early detection and troubleshooting of Apache-related issues.







2\. Jenkins Setup



2.1 Login to Jenkins

• Username → admin

• Password → Adm!n321





3\. Required Plugin Installation



Installed plugin:

1]Publish Over SSH



Steps:

&nbsp;	1. Manage Jenkins → Plugins → Available

&nbsp;	2. Search Publish Over SSH

&nbsp;	3. Install \& restart Jenkins

&nbsp;	4. Refresh UI (UI may freeze while restarting)







4\. Configure Storage Server for SSH

Manage Jenkins → Configure System → Publish Over SSH



Configured:



Field	Value

Name	StorageServer

Hostname	Storage server IP / hostname

Username	natasha

Password	( password) 

Remote Directory	/usr/src/data



Test connection → Success





5\. Create Jenkins Job

New Item → Freestyle Project → copy-logs



Build Trigger

Cron to run every 2 minutes:



\*/2 \* \* \* \*







6\. Build Step – Fetch Logs from App Server 1



Issue 1 → Hostname not resolved



Error:

ssh: Could not resolve hostname app01



Fix: Updated correct hostname/IP:

172.16.238.10





Issue 2 → sudo: a terminal is required



Error:

sudo: a terminal is required to read the password



Fix: Use sudo -S and pipe password:



sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@172.16.238.10 \\

"echo 'Ir0nM@n' | sudo -S cat /var/log/httpd/access\_log" > access\_log



sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@172.16.238.10 \\

"echo 'Ir0nM@n' | sudo -S cat /var/log/httpd/error\_log" > error\_log





This successfully fetched both log files into Jenkins workspace.





7\. Post-build Action – Transfer Logs via SSH

Initial Issue → Nested folder creation



Wrong Output:

/usr/src/data/usr/src/data/access\_log

/usr/src/data/usr/src/data/error\_log





Cause

• Incorrect “Source files”

• Misconfigured “Remote directory”

• Missing “Remove prefix” handling





Fix – Correct Settings



Source files:

access\_log,error\_log





Remove prefix:

(Leave Empty)





Remote directory:

(Leave Blank - use server default configured  earlier)





This ensured logs were stored as:

/usr/src/data/access\_log

/usr/src/data/error\_log







8\. Final Verification



On Storage Server:

ls -l /usr/src/data



Output:

access\_log

error\_log





✔ Files transferred

✔ Naming correct

✔ No nested directory

✔ Job runs automatically every 2 minutes

✔ Task completed successfully







✅ Final Status: SUCCESS

All issues were resolved:

1]Hostname resolution error → fixed.

2]sudo TTY requirement → fixed using sudo -S.

3]Nested folder structure → fixed with correct Publish Over SSH config.

4]Logs are now correctly transferred \& visible in /usr/src/data



