Create a Cron Job



1]

The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:



a. Install cronie package on all Nautilus app servers and start crond service.



b. Add a cron \*/5 \* \* \* \* echo hello > /tmp/cron\_text for root user.



->



Step 1 – Install cronie



SSH into each App Server (App Server 1, App Server 2, App Server 3) using the credentials provided in the task, then run:



&nbsp;sudo yum install cronie -y 





Step 2 – Start and Enable crond



Start the cron daemon and make sure it runs on boot:



&nbsp;sudo systemctl start crond



&nbsp;sudo systemctl enable crond



&nbsp;sudo systemctl status crond



You should see it active (running).





Step 3 – Add the Cron Job for Root



&nbsp;sudo crontab -e





Add this line at the end:

\*/5 \* \* \* \* echo hello > tmp/cron\_text





Step 4 – Verify the Cron Job

1]List root’s cron jobs:

sudo crontab -l



2]Wait at least 5 minutes, then check if the file exists and contains hello:

&nbsp;cat /tmp/cron\_text 



You should see output- 

hello



