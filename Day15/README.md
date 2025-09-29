Setup SSL For Nginx



1]

The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 2 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below: 





1\. Install and configure nginx on App Server 2. 



2\. On App Server 2 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx. 



3\. Create an index.html file with content Welcome! under Nginx document root. 



4\. For final testing try to access the App Server 2 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://<app-server-ip>/.



->





1]Log into the app server2

&nbsp;ssh 172.16.238.11



2]Install and Configure Nginx



First, connect to App Server 2 from the jump host and switch to the root user. Then, install nginx.



&nbsp;sudo yum install -y nginx





3]Once installed, start and enable the nginx service so it runs on boot.

&nbsp;sudo systemctl start nginx

&nbsp;sudo systemctl enable nginx





4]Configure SSL



To configure SSL for nginx, create a directory for the SSL certificates and move the provided files into it.



&nbsp;sudo mkdir -p /etc/nginx/ssl

&nbsp;sudo mv /tmp/nautilus.crt /etc/nginx/ssl/

&nbsp;sudo mv /tmp/nautilus.key /etc/nginx/ssl/



5]Now open the main nginx configuration file, /etc/nginx/nginx.conf, for editing.

&nbsp;sudo vi /etc/nginx/nginx.conf



Inside the server block, modify the configuration to listen on port 443 and configure SSL using the moved certificate and key files.





server {

&nbsp;   listen       443 ssl http2 default\_server;

&nbsp;   listen       \[::]:443 ssl http2 default\_server;

&nbsp;   server\_name  \_;

&nbsp;   root         /usr/share/nginx/html;



&nbsp;   ssl\_certificate "/etc/nginx/ssl/nautilus.crt";

&nbsp;   ssl\_certificate\_key "/etc/nginx/ssl/nautilus.key";

&nbsp;   ssl\_session\_cache shared:SSL:1m;

&nbsp;   ssl\_session\_timeout  10m;

&nbsp;   ssl\_ciphers HIGH:!aNULL:!MD5;

&nbsp;   ssl\_prefer\_server\_ciphers on;



&nbsp;   # Load configuration files for the default server block.

&nbsp;   include /etc/nginx/default.d/\*.conf;



&nbsp;   error\_page 404 /404.html;

&nbsp;       location = /40x.html {

&nbsp;   }



&nbsp;   error\_page 500 502 503 504 /50x.html;

&nbsp;       location = /50x.html {

&nbsp;   }

}





6]Now, check the nginx configuration for syntax errors and restart the service to apply the changes.

&nbsp;sudo nginx -t 

&nbsp;sudo systemctl restart nginx





7]Create index.html



Create the index.html file with the specified content in the nginx document root.



echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html





8]Verify the setup-

From the jump host, use the curl command with the -k option to ignore the self-signed certificate and -I to show only the headers. This confirms that the SSL configuration is working and the nginx server is responding.



curl -Ik https://<App-Server-2-IP-or-hostname>



The output should show a HTTP/1.1 200 OK status, indicating the connection was successful. You should also see the Server: nginx/... header.



