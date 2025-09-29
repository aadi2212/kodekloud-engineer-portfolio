Configure Nginx + PHP-FPM Using Unix Sock



1]

The Nautilus application development team is planning to launch a new PHP-based application, which they want to deploy on Nautilus infra in Stratos DC. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:





a. Install nginx on app server 1 , configure it to use port 8094 and its document root should be /var/www/html.



b. Install php-fpm version 8.3 on app server 1, it must use the unix socket /var/run/php-fpm/default.sock (create the parent directories if don't exist).



c. Configure php-fpm and nginx to work together.



d. Once configured correctly, you can test the website using curl http://stapp01:8094/index.php command from jump host.

NOTE: We have copied two files, index.php and info.php, under /var/www/html as part of the PHP-based application setup. Please do not modify these files.





->



📘 Documentation: PHP Application Setup on App Server 1



🎯 Task Objective



The Nautilus application development team planned to launch a PHP-based application on App Server 1 (stapp01) in Stratos DC.



The requirements were:



1]Install nginx on App Server 1, configure it to use port 8094, and set document root to /var/www/html.

2]Install PHP-FPM 8.3, using the Unix socket /var/run/php-fpm/default.sock.

3]Configure nginx and php-fpm to work together.

4]Validate setup with:





Step 1: SSH into App Server 1

ssh tony@stapp01





Step 2: Installed Nginx

sudo dnf install -y nginx

sudo systemctl enable --now nginx



Configured nginx to listen on port 8094.

Set document root to /var/www/html.





Step 3: Installed PHP-FPM 8.3

At first, tried enabling PHP 8.3 using yum-config-manager with Remi repo:



sudo yum-config-manager --enable remi-php83



⚠️ Mistake: Error occurred → "No matching repo to modify: remi-php83."





Step 4: Resolution

Realized the server was running CentOS Stream 9, which already provides PHP 8.3 in the AppStream repo.



Verified available modules:



yum module list php

Output showed php 8.1, php 8.2, and php 8.3 streams. ✅





Correct installation steps:

sudo dnf module reset -y php

sudo dnf module enable -y php:8.3

sudo dnf install -y php php-fpm





Step 5: Configured PHP-FPM

sudo vi /etc/php-fpm.d/www.conf





Changes made:

listen = /var/run/php-fpm/default.sock

listen.owner = nginx

listen.group = nginx

listen.mode = 0660

user = nginx

group = nginx





Step6: Created the socket directory:

sudo mkdir -p /var/run/php-fpm

sudo chown -R nginx:nginx /var/run/php-fpm





Restarted service:

sudo systemctl enable --now php-fpm





Step 7: Configured Nginx

Created /etc/nginx/conf.d/app.conf:



server {

&nbsp;   listen 8094;

&nbsp;   server\_name stapp01;

&nbsp;   root /var/www/html;

&nbsp;   index index.php index.html;



&nbsp;   location / {

&nbsp;       try\_files $uri $uri/ =404;

&nbsp;   }



&nbsp;   location ~ \\.php$ {

&nbsp;       include fastcgi\_params;

&nbsp;       fastcgi\_pass unix:/var/run/php-fpm/default.sock;

&nbsp;       fastcgi\_index index.php;

&nbsp;       fastcgi\_param SCRIPT\_FILENAME $document\_root$fastcgi\_script\_name;

&nbsp;   }

}





Remove default config if necessary:

sudo rm -f /etc/nginx/conf.d/default.conf





Step8:Test and reload:

sudo nginx -t

sudo systemctl restart nginx





Step 9: Verify

From jump host, run:

curl http://stapp01:8094/index.php

curl http://stapp01:8094/info.php





You should see:

&nbsp;	• index.php → app content.

&nbsp;	• info.php → PHP info page.





🐞 Mistake \& Resolution

&nbsp;	• Mistake: Tried enabling remi-php83 repo using yum-config-manager.

&nbsp;	• Reason: That works on CentOS 7/8 with Remi repos, but CentOS Stream 9 already provides PHP 8.3 natively.

&nbsp;	• Resolution: Used dnf module enable php:8.3 to install PHP-FPM 8.3 directly from AppStream.





✅ Final Outcome

nginx running on port 8094.

php-fpm 8.3 running via unix socket /var/run/php-fpm/default.sock.

Application files served correctly from /var/www/html.









