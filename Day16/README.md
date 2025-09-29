Install and configure nginx as an LBR



1]

Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:





a. Install nginx on LBR (load balancer) server.



b. Configure load-balancing with the an http context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.



c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.



d. Once done, you can access the website using StaticApp button on the top bar.



->



✅ Nginx Load Balancer Setup \& Troubleshooting (Checklist)



Task Goal

&nbsp;	• Setup Nginx on LBR server.

&nbsp;	• Load balance across 3 app servers.

&nbsp;	• Use the correct Apache port (don’t change it).

&nbsp;	• Verify access from Jump Host / StaticApp.





Steps:

1]Install \& Start Nginx

sudo yum install -y nginx



sudo systemctl status nginx



If stopped then start using:

&nbsp;sudo systemctl start nginx 

&nbsp;sudo systemctl enable nginx



2]Check apache service on each app server first



ssh tony@172.16.238.10

ssh steve@172.16.238.11

ssh banner@172.16.238.12 



Check status using command sudo systemctl status httpd





3]Check Apache Port on App Servers:

sudo grep -i Listen /etc/httpd/conf/httpd.conf



• Found: Listen 3003

• So Apache runs on 3003, not 8080.







4]Fix Nginx Config



sudo vi /etc/nginx/nginx.conf



user nginx;

worker\_processes auto;

error\_log /var/log/nginx/error.log;

pid /run/nginx.pid;



include /usr/share/nginx/modules/\*.conf;



events {

&nbsp;   worker\_connections 1024;

}



http {

&nbsp;   upstream backend {

&nbsp;       server stapp01:3003;

&nbsp;       server stapp02:3003;

&nbsp;       server stapp03:3003;

&nbsp;   }



&nbsp;   server {

&nbsp;       listen 80;



&nbsp;       location / {

&nbsp;           proxy\_pass http://backend;

&nbsp;       }

&nbsp;   }



&nbsp;   include /etc/nginx/conf.d/\*.conf;

}



5]Test config:

&nbsp;sudo  nginx -t 

&nbsp;sudosystemctl restart nginx



6]Verify all app servers



curl http://172.16.238.10

Curl http://172.16.238.11

Curl http://172.16.238.12



Welcome to xFusionCorp Industries!



From Jump Host



Curl http://172.16.238.14



7]Expected output:

curl http://172.16.238.11:3003

Welcome to xFusionCorp Industries!



Troubleshooting Notes:



❌ Error: "server" directive not allowed → caused by missing braces {} in nginx.conf.

❌ Error: Failed to connect to port 8080 → Apache was running on 3003, not 8080.

❌ Due to default configuration I got errors so make sure to delete default configuration.

✅ Fixed both → Nginx config validated \& site accessible.

&nbsp;



