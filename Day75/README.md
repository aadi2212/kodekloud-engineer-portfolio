Jenkins Slave Nodes

1]The Nautilus DevOps team has installed and configured new Jenkins server in Stratos DC which they will use for CI/CD and for some automation tasks. There is a requirement to add all app servers as slave nodes in Jenkins so that they can perform tasks on these servers using Jenkins. Find below more details and accomplish the task accordingly.

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.
1. Add all app servers as SSH build agent/slave nodes in Jenkins. Slave node name for app server 1, app server 2 and app server 3 must be App_server_1, App_server_2, App_server_3 respectively.

2. Add labels as below:
App_server_1 : stapp01

App_server_2 : stapp02

App_server_3 : stapp03

3. Remote root directory for App_server_1 must be /home/tony/jenkins, for App_server_2 must be /home/steve/jenkins and for App_server_3 must be /home/banner/jenkins.

4. Make sure slave nodes are online and working properly.

Note:
1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.


2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

->

OneNote Documentation: Jenkins Slave Node Configuration


Task Objective:
Configure all three application servers as SSH build agents in Jenkins.
Ensure each node has the correct credentials, labels, and remote directories.
Install Java on app servers so agents can connect successfully.


1️⃣ Install Required Plugin

Steps:
	1. Go to Manage Jenkins → Plugins
	2. Select Available Plugins
	3. Search and install: SSH Build Agents Plugin
	4. After installation → Click Restart Jenkins when installation is complete


2️⃣ Add SSH Credentials for App Servers

Navigate:
Manage Jenkins → Credentials → System → Global credentials → Add Credentials

Credentials Added:


🔹 For App Server 1
	• Username: tony
	• Password: (from server details)
	• ID: stapp01
	• Save


🔹 For App Server 2
	• Username: steve
	• Password: (from server details)
	• ID: stapp02
	• Save


🔹 For App Server 3
	• Username: banner
	• Password: (from server details)
	• ID: stapp03
	• Save



3️⃣ Add Jenkins Nodes (SSH Build Agents)

Navigate:
Manage Jenkins → Nodes → New Node



🔹 Node: App_server_1
	• Node Name: App_server_1
	• Type: Permanent Agent → Create
	• Remote Root Directory: /home/tony/jenkins
	• Labels: stapp01
	• Launch Method: Change to Launch agents via SSH
	• Host: stapp01
	• Credentials: tony
	• Host Key Verification Strategy: Non verifying verification strategy
	• Save



Launch Error:

On first launch, error appeared:

"Launch failed – SSH connection closed"

Reason: Java not installed on app_server_1


Fix: Install Java on App Server 1

SSH into the server:
ssh tony@172.16.238.10


Install Java 17:
yum search java*
yum install java-17-openjdk-devel.x86_64 -y


Verify:
java --version

Now launch the agent again 

✅ Agent successfully connected and online


4️⃣ Configure App_server_2 and App_server_3
Follow same steps as App_server_1:


🔹 Node: App_server_2
	• Node Name: App_server_2
	• Remote Root Directory: /home/steve/jenkins
	• Labels: stapp02
	• Launch Method: Launch agents via SSH
	• Host: stapp02
	• Credentials: steve
	• Host Key Verification Strategy: Non verifying verification strategy
	• Save


Install Java on App Server 2
ssh steve@<server_ip>
yum install java-17-openjdk-devel.x86_64 -y
java --version


Launch agent →
✅ Connected



🔹 Node: App_server_3
	• Node Name: App_server_3
	• Remote Root Directory: /home/banner/jenkins
	• Labels: stapp03
	• Launch Method: Launch agents via SSH
	• Host: stapp03
	• Credentials: banner
	• Host Key Verification Strategy: Non verifying verification strategy
	• Save


Install Java on App Server 3
ssh banner@<server_ip>
yum install java-17-openjdk-devel.x86_64 -y
java --version


Launch agent →
✅ Connected



5️⃣ Verify All Three Nodes Are Running
In Manage Nodes, all three agents should show:
🟢 Connected & Online
	• App_server_1 – stapp01
	• App_server_2 – stapp02
	• App_server_3 – stapp03



6️⃣ Test Each Node with a Job

Create Test Job
	1. New Item → test-job → Freestyle Project
	2. Under Restrict where this project can run
	Label Expression: App_server_1
	3. Build Steps → Execute Shell
          echo "Hello"

	4. Save → Build Now
	5. Console Output shows:
	
	Building remotely on App_server_1 (: stapp01) in workspace  /home/tony/jenkins/workspace/test-job
	(test-job) $  /bin/sh -xe  /tmp/jenkins71711117867274492569.sh
	+ echo Hello
	Hello
	Finished: Success
	 
	
	Repeat for:
		• App_server_2
		• App_server_3
	Each test job runs successfully on its respective node.
	
	
	✅ Final Result
	✔ All three application servers added as Jenkins SSH agents
	✔ Correct labels & directories configured
	✔ Java installed to support agent launch
	✔ Agents online and verified with test jobs
	Your task is fully completed successfully.
![Uploading image.png…]()
