Install and Configure Web Application



1]

xFusionCorp Industries is planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:





a. Install httpd package and dependencies on app server 1.



b. Apache should serve on port 5003.



c. There are two website's backups /home/thor/blog and /home/thor/demo on jump\_host. Set them up on Apache in a way that blog should work on the link http://localhost:5003/blog/ and demo should work on link http://localhost:5003/demo/ on the mentioned app server.



d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:5003/blog/ and curl http://localhost:5003/demo/



->



step by step to set up two static websites (blog \& demo) on App Server 1 with Apache running on port 5003.



🛠 Step-by-Step Solution:

1\. Login to App Server 1

ssh tony@stapp01





2\. Install Apache (httpd) and dependencies:

&nbsp;sudo yum install -y httpd





3.Enable \& start Apache:

&nbsp;sudo systemctl start httpd

sudo systemctl enable httpd





4\. Change Apache Port to 5003

Edit Apache config:

vi /etc/httpd/conf/httpd.conf



Find:

Listen 80



Change it to:

Listen 5003



Restart Apache:

&nbsp;sudo systemctl restart httpd





5.Verify:

sudo ss -tulnp | grep 5003



You should see :5003 in LISTEN state.





6\. Copy Websites from Jump Host to App Server

On jump host, the websites are in /home/thor/blog and /home/thor/demo.

You need to copy them to App Server 1 under /var/www/html.



From jump host, run:

scp -r /home/thor/blog tony@stapp01\[IP Of App1]:/tmp/

scp -r /home/thor/demo tony@stapp01\[IP Of App1]:/tmp/



Then on App Server 1:

&nbsp;sudo mv /tmp/blog /var/www/html/

&nbsp;sudo mv /tmp/demo /var/www/html/





7.Configure Virtual Aliases in Apache

vi /etc/httpd/conf/httpd.conf



At the bottom, add:

Alias /blog "/var/www/html/blog"

<Directory "/var/www/html/blog">

&nbsp;   AllowOverride All

&nbsp;   Require all granted

</Directory>



Alias /demo "/var/www/html/demo"

<Directory "/var/www/html/demo">

&nbsp;   AllowOverride All

&nbsp;   Require all granted

</Directory>





8.Restart Apache

&nbsp;sudo systemctl restart httpd





9.Test Websites

On App Server 1:



curl http://localhost:5003/blog/

curl http://localhost:5003/demo/



✅ Both should return their respective website contents.



