Install Jenkins Plugins



1]The Nautilus DevOps team has recently setup a Jenkins server, which they want to use for some CI/CD jobs. Before that they want to install some plugins which will be used in most of the jobs. Please find below more details about the task



1\. Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and Adm!n321 password.



2\. Once logged in, install the Git and GitLab plugins. Note that you may need to restart Jenkins service to complete the plugins installation, If required, opt to Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre.



Note:



1\. After restarting the Jenkins service, wait for the Jenkins login page to reappear before proceeding.



2\. For tasks involving web UI changes, capture screenshots to share for review or consider using screen recording software like loom.com for documentation and sharing.



->



Task Name: Install Git \& GitLab Plugins on Jenkins Server

Environment: xFusionCorp / KodeKloud Jenkins setup

Objective: Install required source-code integration plugins to support CI/CD workflows.





🔐 Step-by-Step Execution:



1️⃣ Access Jenkins UI

1]Click Jenkins icon on top navigation.

2]Login using:

1]Username: admin

2] Password: Adm!n321







2️⃣ Install Plugins

Navigate:



Dashboard → Manage Jenkins → Manage Plugins → Available



Search and install:

1]✅ Git Plugin

2]✅ GitLab Plugin



Click Install without restart





3️⃣ Restart Jenkins if prompted

Once installation completes → choose:



✔ “Restart Jenkins when installation is complete and no jobs are running”





⏳ Wait until Jenkins login page comes back before proceeding.







📌 Verification:

Check plugins are installed:



Dashboard → Manage Jenkins → Manage Plugins → Installed





You should now see:

✅ Git

✅ GitLab





🎯 Outcome:

1]Jenkins is now ready for CI/CD integrations with GitHub/GitLab 🎉

2]No configuration issues encountered

3]Login verified post-restart ✅



