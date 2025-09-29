Linux Process Troubleshooting



1]

The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.





Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not have hosted any code yet on these servers, so you don’t need to worry if Apache isn’t serving any pages. Just make sure the service is up and running. Also, make sure Apache is running on port 5000 on all app servers.



->





📘 Task Documentation – Apache Service Unavailability



🔹 Task Name:

Apache Service Troubleshooting (Port 5000 Configuration)





🔹 Problem Statement:



Monitoring tools reported that the Apache service is unavailable on one of the app servers in the Stratos Datacenter.



Your responsibility is to:



1]Identify the faulty app host.

2]Ensure Apache service is running on all app servers.

3]Verify Apache is configured to listen on port 5000 across all app servers.





🔹 Investigation:



1\. Checked Apache status on each App Server



&nbsp;sudo systemctl status httpd



• On App Server 1 →  Failed ❌

• On App Server 2 → Running ✅

• On App Server 3 →Running  ✅





2] Checked logs for failure



sudo journalctl -u httpd --no-pager | tail -20





Found error:



(98)Address already in use: AH00072: make\_sock: could not bind to address \[::]:5000







3] Checked which process is using port 5000



sudo netstat -tulnp | grep 5000



Discovered some other process bound to port 5000.





🔹 Root Cause:

• On App Server 1, another process was already using port 5000, which prevented Apache from starting.





🔹 Fix Implementation:



1]Stopped the conflicting service (example: sendmail or custom process).



&nbsp;sudo systemctl stop sendmail

&nbsp;sudo systemctl disable sendmail





2]Edit Apache configuration to ensure it listens on port 5000



sudo vi /etc/httpd/conf/httpd.conf





3]Updated

Listen 5000





4]Restarted and enabled Apache



&nbsp;sudo systemctl daemon-reload

&nbsp;sudo systemctl start httpd 

&nbsp;sudo systemctl enable httpd





5]Verified listening port

&nbsp;sudo  netstat -tulnp | grep httpd 



Output:

tcp   0   0 0.0.0.0:5000   0.0.0.0:\*   LISTEN   <pid>/httpd





🔹 Verification:

6]Checked locally on each server:

Curl http://localhost:5000



Apache test page returned ✅





7]From jump host:



curl http://stapp01:5000

curl http://stapp02:5000

curl http://stapp03:5000



All responded successfully ✅





🔹 Final Outcome:



• Apache service is up and running on all app servers.

• Configured and verified to listen on port 5000.

• Monitoring alert resolved.







