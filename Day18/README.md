Configure LAMP Server



1]

xFusionCorp Industries is planning to host a WordPress website on their infra in Stratos Datacenter. They have already done infrastructure configuration—for example, on the storage server they already have a shared directory /vaw/www/html that is mounted on each app host under /var/www/html directory. Please perform the following steps to accomplish the task:





a. Install httpd, php and its dependencies on all app hosts.



b. Apache should serve on port 8086 within the apps.



c. Install/Configure MariaDB server on DB Server.



d. Create a database named kodekloud\_db5 and create a database user named kodekloud\_tim identified as password YchZHRcLkL. Further make sure this newly created user is able to perform all operation on the database you created.



e. Finally you should be able to access the website on LBR link, by clicking on the App button on the top bar. You should see a message like App is able to connect to the database using user kodekloud\_tim



->



📝 Task Documentation – WordPress Hosting Setup (KodeKloud Scenario)



🎯 Task Objective



xFusionCorp Industries planned to host a WordPress website in Stratos Datacenter. The requirements were:



1]Install Apache (httpd), PHP, and dependencies on all app servers.

2]Configure Apache to run on port 8086.

3]Install and configure MariaDB on the DB server.

4]Create database kodekloud\_db5 with user kodekloud\_tim (password: YchZHRcLkL) and grant full privileges.

5]Deploy WordPress on the shared storage (/var/www/html) available to all app servers.

6]Configure Nginx on Load Balancer to route traffic.

7]Verify application connectivity via LBR URL.





🛠️ Step-by-Step Execution



1\. App Servers (stapp01, stapp02, stapp03)

Installed Apache, PHP, and dependencies:



sudo yum install httpd php php-mysqlnd -y





started and enabled Apache:

&nbsp;sudo systemctl start httpd

&nbsp;sudo systemctl enable httpd





2\. Configure Apache to run on port 8086

Edit the Apache config:

sudo vi /etc/httpd/conf/httpd.conf



Find the line:

Listen 80



Change it to:

Listen 8086



Restart Apache:

sudo systemctl restart httpd





3.Test from app1,2and 3 ip address

&nbsp;curl http://app-ip:8086 





3\. Install and Configure MariaDB on DB Server

Login to DB server (stdb01) and install MariaDB:



sudo yum install -y mariadb-server

sudo systemctl start mariadb

sudo systemctl enable mariadb





4: Create Database and User

Login to MariaDB:



mysql -u root  -- if got any issue then use sudo or sudo -i





CREATE DATABASE kodekloud\_db5;

CREATE USER 'kodekloud\_tim'@'%' IDENTIFIED BY 'YchZHRcLkL';

GRANT ALL PRIVILEGES ON kodekloud\_db5.\* TO 'kodekloud\_tim'@'%';

FLUSH PRIVILEGES;

exit





5: Setup WordPress (on Storage/Shared Directory)

Login to Storage server (ststor01) where /var/www/html is mounted.



Download and extract WordPress:



sudo yum install -y wget



cd /var/www/html



wget https://wordpress.org/latest.tar.gz



tar -xvzf latest.tar.gz



mv wordpress/\* .



rm -rf wordpress latest.tar.gz



Note-If here got any permission issue then use sudo or sudo -i





6: Configure WordPress to connect DB



Rename sample config:

mv wp-config-sample.php wp-config.php



Edit config:

vi wp-config.php



Set database settings:

define( 'DB\_NAME', 'kodekloud\_db5' );

define( 'DB\_USER', 'kodekloud\_tim' );

define( 'DB\_PASSWORD', 'YchZHRcLkL' );

define( 'DB\_HOST', 'stdb01' );   // or use IP of DB server





7\. Verify from LBR

Logged into the load balancer server



Install nginx in load balancer server

sudo yum install -y nginx





Edit nginx configuration-

&nbsp;sudo vi /etc/nginx/nginx.conf





http {

&nbsp;   upstream backend {

&nbsp;       server 172.16.238.10:8086;

&nbsp;       server 172.16.238.11:8086;

&nbsp;       server 172.16.238.12:8086;

&nbsp;   }



&nbsp;   server {

&nbsp;       listen 80;



&nbsp;       location / {

&nbsp;           proxy\_pass http://backend;

&nbsp;       }

&nbsp;   }

}



Note-\[Delete the overall default configuration after this]





8]Restart Nginx-

sudo nginx -t

sudo systemctl restart nginx 



After restarting nginx I got an issue here-

Job for nginx.service failed because the control process exited with error code.



Identifying error-

See "systemctl status nginx.service" and "journalctl -xeu nginx.service" for details.



Then I got the output i.e. nginx: \[emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)



means another service is already using port 80. This is why Nginx fails to start, even though your config syntax is fine.





✅ How to fix-



1]Check which service is using port 80

sudo ss -tulnp | grep :80



Output:

tcp LISTEN 0 2000 0.0.0.0:80 0.0.0.0:\* users:(("haproxy",pid=1375,fd=7))



Means it’s HAProxy that is already bound to port 80:





2]Stop and disable haproxy service

sudo systemctl stop haproxy

sudo systemctl disable haproxy



3]Again restarted nginx service

&nbsp;sudo systemctl restart nginx



And problem is solved.



9]Then finally test load balancer ip through port no 8086-

&nbsp;curl http://172.16.238.14:8086



And I got output i.e. App is able to connect to the database using user 



