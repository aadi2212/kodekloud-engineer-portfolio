Configure Jenkins User Access 



1]The Nautilus team is integrating Jenkins into their CI/CD pipelines. After setting up a new Jenkins server, they're now configuring user access for the development team, Follow these steps:



1\. Click on the Jenkins button on the top bar to access the Jenkins UI. Login with username admin and password Adm!n321.

2\. Create a jenkins user named jim with the passwordRc5C9EyvbU. Their full name should match Jim.



3\. Utilize the Project-based Matrix Authorization Strategy to assign overall read permission to the jim user.



4\. Remove all permissions for Anonymous users (if any) ensuring that the admin user retains overall Administer permissions.



5\. For the existing job, grant jim user only read permissions, disregarding other permissions such as Agent, SCM etc.





Note:



1\. You may need to install plugins and restart Jenkins service. After plugins installation, select Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page.





2\. After restarting the Jenkins service, wait for the Jenkins login page to reappear before proceeding. Avoid clicking Finish immediately after restarting the service.





3\. Capture screenshots of your configuration for review purposes. Consider using screen recording software like loom.com for documentation and sharing.





->



Task: Configure Jenkins User Access for “Jim”



🎯 Objective:



Set up Jenkins security using Project-based Matrix Authorization Strategy, ensuring:

1]admin retains full control

2]jim has only read access

3]anonymous users have no access





Step 1: Access Jenkins

Open your Jenkins UI in a browser:

http://<your-server-ip>:8080





&nbsp;Login with:

1]Username: admin

2]Password: Adm!n321





Step2: Create a User “jim”

1]Go to Manage Jenkins → Users (or Manage Users).



2]Click Create User.



3]Fill in the details:

1]Username: jim

2]Password: Rc5C9EyvbU

3]Confirm password: Rc5C9EyvbU

4]Full name: Jim





4]Click Create User.

✅ You should now see both users: admin and jim.







Step 3: Install and Enable Project-based Matrix Authorization Strategy Plugin

If you don’t see “Project-based Matrix Authorization Strategy” under security settings (as in your previous screenshot), do this first:



1]Go to Manage Jenkins → Plugins → Available Plugins.



2]Search for:

Matrix Authorization Strategy





3]Select it and click Install without restart.





4]Once installed, check:

✅ Restart Jenkins when installation is complete and no jobs are running.





5]Wait for Jenkins to restart completely.

⚠️ Don’t click “Finish” too early — wait for the login screen.)







Step4: Configure Global Security

1]Go to Manage Jenkins → Configure Global Security.



2]Under Authorization, select:

✅ Project-based Matrix Authorization Strategy.





3]Click Add user or group and add:

1]admin

2]jim





4]Assign permissions:



User	Overall	Job	Other

admin	Administer ✅	All ✅	All ✅

jim	Read ✅	(none)	(none)

anonymous	❌ None	❌ None	❌ None





5]Click Save







Step 5: Configure Job-Level Permissions

1]Go to Dashboard → Select your existing job.



2]Click Configure.



3]Scroll to the Build Environment / Properties section.



4]Check ✅ “Enable project-based security.”



5]Click Add user or group, then add:

&nbsp;   jim



6]Assign only:

✅ Job → Read



7]Leave all other permissions unchecked.



8]Click Save.







Step 6: Verify Access

1]Log out as admin.



2]Log in as:

• Username: jim

• Password: Rc5C9EyvbU





3]Verify:

• You can view jobs.

• You cannot build, configure, or delete anything.





✅ Final Access Summary



User	Permission	Access Level

admin	Overall → Administer	Full access

jim	Overall → Read	Read-only

anonymous	None	No access





Step 7: Documentation

📸 Capture screenshots of:

&nbsp;	1. User creation page (jim)

&nbsp;	2. Plugin installation (Matrix Authorization Strategy)

&nbsp;	3. Global Security configuration (Matrix table)

&nbsp;	4. Job configuration (project-based permissions)

&nbsp;	5. Login verification as jim



