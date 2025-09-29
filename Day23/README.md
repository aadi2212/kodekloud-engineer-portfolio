Fork a git repository



1]

There is a Git server utilized by the Nautilus project teams. Recently, a new developer named Jon joined the team and needs to begin working on a project. To begin, he must fork an existing Git repository. Follow the steps below:

1\. Click on the Gitea UI button located on the top bar to access the Gitea page.

2\. Login to Gitea server using username jon and password Jon\_pass123.

3\. Once logged in, locate the Git repository named sarah/story-blog and fork it under the jon user.



Note: For tasks requiring web UI changes, screenshots are necessary for review purposes. Additionally, consider utilizing screen recording software such as loom.com to record and share your task completion process.



->



Steps to Fork Repository in Gitea for Jon:



1]Access Gitea Web UI:

On your lab environment/dashboard, click on the “Gitea UI” button in the top bar.

This will open the Gitea web interface.



2]Login as Jon:

Username: jon

Password: Jon\_pass123

Click Sign In.



3]Locate the Repository:

Once logged in, go to the search bar (usually top-right) and type:



sarah/story-blog





4]Fork the Repository:

1]Open the repository.

2]On the top-right of the repository page, click the Fork button.

3]Select the jon account as the destination.



✅ This creates a new repository jon/story-blog under Jon’s account.





5]Verify the Fork:

1]After forking, you should be redirected to the new repo under jon.

2]Confirm that the repository URL is now something like:



http://<gitea-url>/jon/story-blog



