Jenkins Parameterized Builds



1]A new DevOps Engineer has joined the team and he will be assigned some Jenkins related tasks. Before that, the team wanted to test a simple parameterized job to understand basic functionality of parameterized builds. He is given a simple parameterized job to build in Jenkins. Please find more details below:





Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.





1\. Create a parameterized job which should be named as parameterized-job





2\. Add a string parameter named Stage; its default value should be Build.





3\. Add a choice parameter named env; its choices should be Development, Staging and Production.





4\. Configure job to execute a shell command, which should echo both parameter values (you are passing in the job).





5\. Build the Jenkins job at least once with choice parameter value Staging to make sure it passes.





Note:



1\. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.





2\. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.



->





Jenkins Parameterized Job Task Documentation:



Task Objective:

Create a simple parameterized Jenkins job to understand basic functionality of parameterized builds.





Steps Performed:

1]Access Jenkins UI

Open Jenkins dashboard → login using:

Username: admin

Password: Adm!n321





2]Create a New Job

Click “New Item” → Enter job name: parameterized-job

Select Freestyle project → Click OK





3]Configure Job Parameters

Check “This project is parameterized”



String Parameter:

Name: Stage

Default value: Build

Description: Enter the stage name



Choice Parameter:

&nbsp;	• Name: env

&nbsp;	

&nbsp;	• Choices:

Development

Staging

Production

&nbsp;	• Description: Select the environment to deploy/build







4]Add Build Step

Build → Add build step → Execute shell



Command:

echo "Stage parameter value: $Stage"

echo "Env parameter value: $env"

&nbsp;	





5]Build the Job

Click Build with Parameters

Set env = Staging and Stage = Build

Verify output in Console Output





Error Encountered:

1]Choice Parameter Mistake:

Initially wrote Stage instead of Staging in choice parameter.



Error:

job 'parameterized-job' choice parameter's choices are not 'Development', 'Staging' and 'Production'





Resolution:

Corrected the spelling to Staging.





2]Job Not Found Error:

Created job with name paramerized-job (typo).



Error:

job 'parameterized-job' not found on Jenkins





Resolution:

Renamed/recreated job to exact name: parameterized-job (case-sensitive, no typos).





Result:

• Job successfully built with parameters:



Stage parameter value: Build

Env parameter value: Staging





Notes

&nbsp;	• Always double-check job names and parameter values for case and spelling.

&nbsp;	• Plugins may need installation/restart if parameters are not available.



