Set Up Jenkins Server



1]The DevOps team at xFusionCorp Industries is initiating the setup of CI/CD pipelines and has decided to utilize Jenkins as their server. Execute the task according to the provided requirements:



1\. Install Jenkins on the jenkins server using the yum utility only, and start its service.

&nbsp;	• If you face a timeout issue while starting the Jenkins service, refer to this.

2\. Jenkin's admin user name should be theadmin, password should be Adm!n321, full name should be Mark and email should be mark@jenkins.stratos.xfusioncorp.com.



Note:

1\. To access the jenkins server, connect from the jump host using the root user with the password S3curePass.

2\. After Jenkins server installation, click the Jenkins button on the top bar to access the Jenkins UI and follow on-screen instructions to create an admin user.





->



Task: Jenkins Installation and Admin User Setup



Objective:

Set up Jenkins as the CI/CD server on the designated Jenkins host, ensuring it is installed via yum, running as a service, and configured with a specific admin user.





Prerequisites:

1]Access the Jenkins server from the jump host as root with password: S3curePass.

2]Internet access on the Jenkins server.

3]Required tools: wget, rpm, yum.





Step 1: Connect to Jenkins Server

ssh root@<jenkins\_server\_ip>



Password: S3curePass





Step 2: Add Jenkins Repository

wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key



This ensures yum can find the Jenkins package.







Step 3: Install Dependencies

yum install -y fontconfig java-21-openjdk



Jenkins requires Java 21 to run properly.







Step 4: Install Jenkins

yum install -y jenkins



If yum cannot find Jenkins, verify the repo is added correctly.



If you get an error related to the GPG key, you can bypass it using:



yum install -y jenkins --nogpgcheck



This ensures Jenkins installs even if the repository key verification fails. Always verify the key source is trusted when using --nogpgcheck.







Step 5: Start and Enable Jenkins Service

systemctl start jenkins

systemctl enable jenkins

systemctl status jenkins



Service should show active (running).

Jenkins runs on port 8080 by default.





If timeout occurs:

Check firewall and open port 8080:



firewall-cmd --permanent --add-port=8080/tcp

firewall-cmd --reload





Inspect logs for errors:

journalctl -xeu jenkins







Step 6: Access Jenkins UI

1]Open a browser:

http://<jenkins\_server\_ip>:8080



2]Retrieve the initial admin password:

cat /var/lib/jenkins/secrets/initialAdminPassword





3]Enter this password on the Jenkins setup page.





Step 7: Create Admin User



Field	Value

Username	theadmin

Password	Adm!n321

Full Name	Mark

Email	mark@jenkins.stratos.xfusioncorp.com



Complete the on-screen setup wizard to finalize Jenkins configuration.







Step 8: Verify Jenkins Setup



systemctl start jenkins

Systemctl enable jenkins

Systemctl status jenkins





1]Ensure the service is running.

2]Confirm login works with the created admin user.





Common Issues:

1]Timeout while starting Jenkins:

i]Verify system memory and CPU availability.

ii]Ensure firewall allows port 8080.

iii]Use journalctl -xeu jenkins to debug.



2]Wrong Java version installed:

Remove incorrect version:

yum remove java-11-openjdk



Install Java 21:

yum install java-21-openjdk





Outcome:

1]Jenkins installed and running via yum.

2]Admin user theadmin configured successfully.

3]Jenkins ready for CI/CD pipeline deployment.



