Docker EXEC Operations



1]

One of the Nautilus DevOps team members was working to configure services on a kkloud container that is running on App Server 1 in Stratos Datacenter. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:



a. Install apache2 in kkloud container using apt that is running on App Server 1 in Stratos Datacenter.

b. Configure Apache to listen on port 5001 instead of default http port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.

c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.



->



📘 OneNote Documentation – Apache Setup in kkloud Container



Task Objective:



Configure Apache service inside the kkloud container running on App Server 1 (Stratos Datacenter) as per requirements:



1]Install Apache2.

2]Configure Apache to listen on port 5001 instead of the default 80.

3]Ensure Apache is running and container remains active.



Steps Performed:



Step 1: Access Container

docker exec -it kkloud bash



Step 2: Install Apache

apt-get update -y

apt-get install apache2 -y



Step 3: Configure Apache Port

Install a text editor (recommended if you want to edit files)



apt-get install nano -y

nano /etc/apache2/ports.conf



Find the line:

Listen 80



and change it to:

Listen 5001



Then, edit the default site config:

&nbsp;nano /etc/apache2/sites-available/000-default.conf



Find:

<VirtualHost \*:80>



and change it to:

<VirtualHost \*:5001>



Step 4: Restart Apache

service apache2 restart



Step 5: Verify Apache is running on port 5001

service apache2 status



Check listening ports:

&nbsp;apt-get update -y

&nbsp;apt-get install net-tools -y



Step 6:Exit the container

&nbsp;exit



Step 7:Then confirm it’s still running

docker ps





Warnings Encountered:



1]Apache restart warning:



AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.12.0.2. 

Set the 'ServerName' directive globally to suppress this message

\[ OK ]



Explanation: This is only a warning, not a failure.







