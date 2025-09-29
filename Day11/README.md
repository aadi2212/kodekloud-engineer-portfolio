Install and Configure Tomcat Server



1]

The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the tomcat application server. Based on the requirements mentioned below complete the task:



a. Install tomcat server on App Server 2.



b. Configure it to run on port 8088.



c. There is a ROOT.war file on Jump host at location /tmp.



Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e curl http://stapp02:8088





->







Steps to Complete the Task:



1\. SSH into App Server 2



ssh steve@172.16.238.11





2\. Install Tomcat



&nbsp;sudo yum install -y tomcat



sudo yum install -y tomcat tomcat-webapps tomcat-admin-webapps tomcat-docs-webapp 







3\. Configure Tomcat to Run on Port 8088



Edit the Tomcat server.xml file:



&nbsp;sudo vi /etc/tomcat/server.xml





Look for the following line (default port 8080):



&nbsp;

<Connector port="8080" protocol="HTTP/1.1"

&nbsp;          connectionTimeout="20000"

&nbsp;          redirectPort="8443" />







Change 8080 → 8088:





<Connector port="8088" protocol="HTTP/1.1"

&nbsp;          connectionTimeout="20000"

&nbsp;          redirectPort="8443" />





4\. Start \& Enable Tomcat



&nbsp;sudo systemctl start tomcat

&nbsp;sudo systemctl enable tomcat

&nbsp;sudo systemctl status tomcat 







5\. Copy the ROOT.war File from Jump Host





On Jump Host, copy ROOT.war to App Server 2 under Tomcat’s webapps directory:



&nbsp;ssh thor@jump\_host.stratos.xfusioncorp.com



&nbsp;scp /tmp/ROOT.war  steve@172.16.238.11:/tmp/





Then on App Server 2, move it into Tomcat’s webapps:



&nbsp;sudo mv /tmp/ROOT.war  /usr/share/tomcat/webapps/





⚡ Note: Default Tomcat deployment directory = /usr/share/tomcat/webapps (for CentOS/RHEL).



Once copied, Tomcat will auto-deploy it.





6\. Verify Deployment.



Wait a few seconds for Tomcat to unpack the WAR file. Then test:  





Curl http:// 172.16.238.11:8088





You should get the application’s homepage HTML output.



curl http://172.16.238.11:8088

<!DOCTYPE html>

<!--

To change this license header, choose License Headers in Project Properties.

To change this template file, choose Tools | Templates

and open the template in the editor.

-->

<html>

&nbsp;   <head>

&nbsp;       <title>SampleWebApp</title>

&nbsp;       <meta charset="UTF-8">

&nbsp;       <meta name="viewport" content="width=device-width, initial-scale=1.0">

&nbsp;   </head>

&nbsp;   <body>

&nbsp;       <h2>Welcome to xFusionCorp Industries!</h2>

&nbsp;       <br>

&nbsp;   

&nbsp;   </body>

</html>

Install and Configure Tomcat Server



1]

The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the tomcat application server. Based on the requirements mentioned below complete the task:



a. Install tomcat server on App Server 2.



b. Configure it to run on port 8088.



c. There is a ROOT.war file on Jump host at location /tmp.



Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e curl http://stapp02:8088





->







Steps to Complete the Task:



1\. SSH into App Server 2



ssh steve@172.16.238.11





2\. Install Tomcat



&nbsp;sudo yum install -y tomcat



sudo yum install -y tomcat tomcat-webapps tomcat-admin-webapps tomcat-docs-webapp 







3\. Configure Tomcat to Run on Port 8088



Edit the Tomcat server.xml file:



&nbsp;sudo vi /etc/tomcat/server.xml





Look for the following line (default port 8080):



&nbsp;

<Connector port="8080" protocol="HTTP/1.1"

&nbsp;          connectionTimeout="20000"

&nbsp;          redirectPort="8443" />







Change 8080 → 8088:





<Connector port="8088" protocol="HTTP/1.1"

&nbsp;          connectionTimeout="20000"

&nbsp;          redirectPort="8443" />





4\. Start \& Enable Tomcat



&nbsp;sudo systemctl start tomcat

&nbsp;sudo systemctl enable tomcat

&nbsp;sudo systemctl status tomcat 







5\. Copy the ROOT.war File from Jump Host





On Jump Host, copy ROOT.war to App Server 2 under Tomcat’s webapps directory:



&nbsp;ssh thor@jump\_host.stratos.xfusioncorp.com



&nbsp;scp /tmp/ROOT.war  steve@172.16.238.11:/tmp/





Then on App Server 2, move it into Tomcat’s webapps:



&nbsp;sudo mv /tmp/ROOT.war  /usr/share/tomcat/webapps/





⚡ Note: Default Tomcat deployment directory = /usr/share/tomcat/webapps (for CentOS/RHEL).



Once copied, Tomcat will auto-deploy it.





6\. Verify Deployment.



Wait a few seconds for Tomcat to unpack the WAR file. Then test:  





Curl http:// 172.16.238.11:8088





You should get the application’s homepage HTML output.



curl http://172.16.238.11:8088

<!DOCTYPE html>

<!--

To change this license header, choose License Headers in Project Properties.

To change this template file, choose Tools | Templates

and open the template in the editor.

-->

<html>

&nbsp;   <head>

&nbsp;       <title>SampleWebApp</title>

&nbsp;       <meta charset="UTF-8">

&nbsp;       <meta name="viewport" content="width=device-width, initial-scale=1.0">

&nbsp;   </head>

&nbsp;   <body>

&nbsp;       <h2>Welcome to xFusionCorp Industries!</h2>

&nbsp;       <br>

&nbsp;   

&nbsp;   </body>

</html>



