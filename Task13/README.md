Iptables Installation And Configuration



1]We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 5002 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:





1\. Install iptables and all its dependencies on each app host.



2\. Block incoming port 5002 on all apps for everyone except for LBR host.



3\. Make sure the rules remain, even after system reboot.



->



🔐 Task: Secure Apache Port (5002) using iptables



Problem Statement



Currently, Apache is running on port 5002 across application servers in Stratos DC. Since no firewall is installed, port 5002 is accessible to all.



The requirement is to:



1]Install iptables.

2]Allow port 5002 access only from the Load Balancer (LBR) host.

3]Ensure rules persist after system reboot.





🛠️ Steps to Fix



1\. SSH into Each App Server.



ssh tony@stapp01

ssh steve@stapp02

ssh banner@stapp03





2\. Install iptables



On each app host:



sudo yum install -y iptables iptables-services



Enable the service:

&nbsp;sudo systemctl enable iptables

&nbsp;sudo systemctl start iptables





3\. Add Firewall Rules



First, flush old rules to start clean (optional, but recommended if testing):

&nbsp;sudo iptables -F



Allow Apache (port 5002) only from LBR host (172.16.238.14):

sudo iptables -A INPUT -p tcp -s 172.16.238.14 --dport 5002 -j ACCEPT



Block port 5002 from everyone else:

sudo iptables -A INPUT -p tcp --dport 5002 -j DROP



Allow existing/related connections (best practice):

sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT



Allow loopback (to avoid local issues):

sudo iptables -A INPUT -i lo -j ACCEPT



Allow SSH (port 22), so you don’t get locked out:

sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT



Deny everything else as a default policy:

sudo iptables -P INPUT DROP





4\. Save Rules for Persistence

sudo service iptables save



This writes rules into /etc/sysconfig/iptables, ensuring they load on reboot.



Verify file:

sudo cat /etc/sysconfig/iptables





5\. Verify Rules

sudo iptables -L -n





Expected output (relevant lines):



ACCEPT     tcp  --  172.16.238.14       0.0.0.0/0            tcp dpt:5002

DROP       tcp  --  0.0.0.0/0           0.0.0.0/0            tcp dpt:5002





6\. Test Access



curl http://172.16.238.10:5002





From Jump Host (should fail):



&nbsp;curl  http://172.16.238.10:5002





Final Outcome

&nbsp;	• iptables installed and running on all App Servers.

&nbsp;	• Apache port 5002 is accessible only from LBR host.

&nbsp;	• Rules persist after reboot.

&nbsp;	• Security compliance achieved. ✅





