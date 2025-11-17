Linux Network Services



1]Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port 8086 (which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue.





Use tools like telnet, netstat, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.



Once fixed, you can test the same using command curl http://stapp01:8086 command from jump host



->





Apache Service Not Reachable on Port 8086 – Troubleshooting



Problem Statement

The monitoring system reported that Apache service was not reachable on port 8086 on App Server 1 (stapp01).

The requirement was to make sure Apache is accessible on port 8086 from the Jump Host.





🛠️ Investigation Steps:



1]Checked Apache service status:



sudo systemctl status httpd



Found service in failed state.



Logs indicated



(98)Address already in use: AH00072: could not bind to address 0.0.0.0:808





2]Checked which process was using port 8086:



&nbsp;sudo netstat -tulnp | grep 8086



Output:

tcp  0  0 127.0.0.1:8086   0.0.0.0:\*   LISTEN   429/sendmail: accep



Found that Sendmail service was already bound to port 8086.







3]Stopped Sendmail service (first root cause)



&nbsp;sudo systemctl stop sendmail

&nbsp;sudo systemctl disable sendmail





4]Started Apache again



&nbsp;sudo systemctl start httpd

&nbsp;sudo systemctl enable httpd





5]Verified Apache listening address



&nbsp;sudo netstat -tulnp | grep 8086





Output showed:



tcp  0  0 0.0.0.0:8086   0.0.0.0:\*   LISTEN   <pid>/httpd





But when I logged into Jump\_Server and given command curl http://172.16.238.10:8086 then I got issue which is:



curl: (7) Failed to connect to 172.16.238.10 port 8086: No route to host 



The actual issue is Firewall on stapp01 is blocking external access to port 8086.





6]Checked firewall (iptables) blocking



Confirmed iptables is active:



sudo systemctl status iptables





Opened port 8086 for TCP traffic:



sudo iptables -I INPUT -p tcp --dport 8086 -j ACCEPT

sudo service iptables save





7]Verified rule:

sudo iptables -L -n | grep 8086





Output:

ACCEPT     tcp  --  0.0.0.0/0  0.0.0.0/0  tcp dpt:8086





8]Verification



1]On App Server 1:



Curl http://localhost:8086



Returns Apache Test Page.





2]From Jump Host:



&nbsp;curl http://172.16.238.10:8086 



Returns Apache Test Page successfully.





📌 Root Causes:



1]Sendmail service was using port 8086 → prevented Apache from starting.

2]iptables firewall was blocking external access to port 8086 → prevented Jump Host access.





Resolution:

• Stopped and disabled Sendmail service.

• Configured Apache to listen on 0.0.0.0:8086.

• Added iptables rule to allow TCP traffic on port 8086.

• Verified Apache is reachable locally and remotely from Jump Host.





Final Outcome

&nbsp;	• Apache is fully operational on port 8086.

&nbsp;	• Accessible from Jump Host without compromising security.

&nbsp;	• Task completed successfully. ✅



